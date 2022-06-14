---
layout: post
lang: C#
title: ECDSA signature generation and verification using BouncyCastle
comments: true
description : Generate and verify ECDSA signature using BouncyCastle
date: 2017-08-02
header_desc: ECDSA signature generation and verification
---

## Generate a sample PKCS12(*.p12) certificate.

```cs
// Generate a private key (prime256 name curve is used)
openssl ecparam -name prime256v1 -genkey > private-key.pem

// Generate a public key
openssl ec -in private-key.pem -pubout -out public-key.pem

// Generate certificate file
openssl req -new -key private-key.pem -x509 -nodes -days 365 -out public.cer

// Add private key to the certificate
winpty openssl pkcs12 -export -inkey private-key.pem -in public.cer -out private.p12
```

## Sign your data with ECDSA using BouncyCastle library

```cs
public byte[] SignUsingEcdsa(byte[] bytesData)
{
    // Read the p12 certificate, exportable flag is activated to allow certificate.Export() on line 25
    var certificate = new X509Certificate2("private.p12", "testpass", X509KeyStorageFlags.Exportable);
    var bcCert = DotNetUtilities.FromX509Certificate(certificate);
    
    // Export the certificate to read pkcs12 bytes
    byte[] pkcs12Bytes = certificate.Export(X509ContentType.Pkcs12, "testpass");
    Pkcs12Store pkcs12 = new Pkcs12StoreBuilder().Build();
    pkcs12.Load(new MemoryStream(pkcs12Bytes, false), "testpass".ToCharArray());

    // Extract private key from the pkcs12 bytes
    ECPrivateKeyParameters privKey = null;
    foreach (string alias in pkcs12.Aliases)
    {
        if (pkcs12.IsKeyEntry(alias))
        {
            AsymmetricKeyEntry key = pkcs12.GetKey(alias);
            if (key.Key.IsPrivate)
            {
                privKey = (ECPrivateKeyParameters)pkcs12.GetKey(alias).Key;
                break;
            }
        }
    }
    
    ISigner signer = SignerUtilities.GetSigner(bcCert.SigAlgName);
    signer.Init(true, privKey);
    
    /* Calculate the signature */
    signer.BlockUpdate(bytesData, 0, bytesData.Length);
    byte[] signature = signer.GenerateSignature();
    return signature;
}
```

## Verify ECDSA Signature

```cs
private void VerifyEcdsaSignature(X509Certificate2 cert, byte[] bytesData, byte[] bytesSignature)
{
    var bcCert = DotNetUtilities.FromX509Certificate(cert);
    var pubKey = bcCert.GetPublicKey();

    ISigner signer = SignerUtilities.GetSigner(bcCert.SigAlgName);
    signer.Init(false, pubKey);
    signer.BlockUpdate(bytesData, 0, bytesData.Length);

    Console.WriteLine(Asn1Dump.DumpAsString(Asn1Object.FromByteArray(signature)));

    if (signer.VerifySignature(bytesSignature))
        System.Console.WriteLine("Verified Signature");
    else
        System.Console.WriteLine("Not Verified Signature");
}
```

## Signature transclude method to support JWT ES256 (requires 64 bytes signature)

```cs
/**
* Transcodes the JCA ASN.1/DER-encoded signature into the concatenated
* R + S format expected by ECDSA JWS.
*
* @param derSignature The ASN1./DER-encoded. Must not be {@code null}.
* @param outputLength The expected length of the ECDSA JWS signature.
*
* @return The ECDSA JWS encoded signature.
*
* @throws JwtException If the ASN.1/DER signature format is invalid.
*/
public static byte[] transcodeSignatureToConcat(byte[] derSignature, int outputLength)
{
    if (derSignature.Length < 8 || derSignature[0] != 48)
    {
        throw new Exception("Invalid ECDSA signature format");
    }

    int offset;
    if (derSignature[1] > 0)
    {
        offset = 2;
    }
    else if (derSignature[1] == (byte)0x81)
    {
        offset = 3;
    }
    else
    {
        throw new Exception("Invalid ECDSA signature format");
    }

    byte rLength = derSignature[offset + 1];

    int i = rLength;
    while ((i > 0)
            && (derSignature[(offset + 2 + rLength) - i] == 0))
        i--;

    byte sLength = derSignature[offset + 2 + rLength + 1];

    int j = sLength;
    while ((j > 0)
            && (derSignature[(offset + 2 + rLength + 2 + sLength) - j] == 0))
        j--;

    int rawLen = Math.Max(i, j);
    rawLen = Math.Max(rawLen, outputLength / 2);

    if ((derSignature[offset - 1] & 0xff) != derSignature.Length - offset
            || (derSignature[offset - 1] & 0xff) != 2 + rLength + 2 + sLength
            || derSignature[offset] != 2
            || derSignature[offset + 2 + rLength] != 2)
    {
        throw new Exception("Invalid ECDSA signature format");
    }

    byte[] concatSignature = new byte[2 * rawLen];

    Array.Copy(derSignature, (offset + 2 + rLength) - i, concatSignature, rawLen - i, i);
    Array.Copy(derSignature, (offset + 2 + rLength + 2 + sLength) - j, concatSignature, 2 * rawLen - j, j);

    return concatSignature;
}
```
