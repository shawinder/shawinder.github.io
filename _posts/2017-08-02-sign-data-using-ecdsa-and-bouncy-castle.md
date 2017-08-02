---
layout: post
lang: C#
nav_blog: class="selected"
title: ECDSA signature generation and verification
comments: true
description : Generate and verify ECDSA signature using bouncy castle
date: 2017-08-02
header_desc: ECDSA signature generation and verification
---

## Generate a sample PKCS12(*.p12) certificate.

```
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

```
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

```
private void VerifyEcdsaSignature(X509Certificate2 cert, byte[] bytesData, byte[] bytesSignature)
{
    var bcCert = DotNetUtilities.FromX509Certificate(cert);
    var pubKey = bcCert.GetPublicKey();

    ISigner signer = SignerUtilities.GetSigner(bcCert.SigAlgName);
    signer.Init(false, pubKey);
    signer.BlockUpdate(bytesData, 0, bytesData.Length);

    //Console.WriteLine(Asn1Dump.DumpAsString(Asn1Object.FromByteArray(signature)));

    if (signer.VerifySignature(bytesSignature))
        System.Console.WriteLine("Verified Signature");
    else
        System.Console.WriteLine("Not Verified Signature");
}
```
