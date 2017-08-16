---
layout: post
lang: C#
nav_blog: class="selected"
title: Wix check if component is already installed
comments: true
description : Wix check if component is already installed
date: 2017-08-16
header_desc: Wix check if component is already installed
---

## `Install` - Write component install flag for each individual component in the `registry`:

```xml
<Wix xmlns="http://schemas.microsoft.com/wix/2006/wi">
  <Fragment>
    <Component Directory="[COMPONENT1-INSTALL-FOLDER]" Id="Component1Registry" Guid="[UNIQUE-GUID]">
      <RegistryKey Root="HKLM" Key="Software\FM\[PRODUCT-NAME]\Installer\Component1" Action="createAndRemoveOnUninstall">
        <RegistryValue Type="integer" Name="Installed" Value="1" KeyPath="yes"/>
      </RegistryKey>
    </Component>
  </Fragment>
  .......
</Wix>
```

## `Upgrade` - Read existing `registry` values and set local `Wix properties`

```xml
<Wix xmlns="http://schemas.microsoft.com/wix/2006/wi">
  <Product ...>
    
    <!-- Allow upgrades and prevent downgrades (http://wixtoolset.org/documentation/manual/v3/xsd/wix/majorupgrade.html) -->
    <MajorUpgrade AllowSameVersionUpgrades="yes" DowngradeErrorMessage="A later version of [ProductName] is already installed. Setup will now exit." />
    
    <!-- Read existing component flag stored in the registry -->
    <Property Id="COMPONENT1INSTALLED">
      <RegistrySearch Id="RegSearchComponent1" Root="HKLM" Key="Software\FM\[PRODUCT-NAME]\Installer\Component1"  Name="Installed" Type="raw" />
    </Property>
    
    .......
    
    <InstallExecuteSequence>
      .......
      <!-- WIX_UPGRADE_DETECTED is a property/flag automatically set by MajorUpgrade element -->
      <Custom Action="CustomActionName" Before="RemoveFiles">COMPONENT1INSTALLED AND (NOT WIX_UPGRADE_DETECTED)</Custom>
      .......
    </InstallExecuteSequence>
  </Product>
</Wix>
```
