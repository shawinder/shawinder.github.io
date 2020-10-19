---
layout: post
lang: Windows10
title: Disable Cortana using Powershell
comments: true
description : Disable Cortana for Windows 10 using a Powershell script
date: 2020-10-19
header_desc: Disable Cortana for Windows 10 using a Powershell script
---
1. Open `Powershell` as an Administrator.

2. Run following command to temporarily disable the `digital signature` execution policy.
```
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

3. Create a new powershell script `script-name.ps1` and add following content:
```
Function DisableCortana
{  
    $path = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Windows Search"    
    IF(!(Test-Path -Path $path)) { 
        New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows" -Name "Windows Search"
    } 
    Set-ItemProperty -Path $path -Name "AllowCortana" -Value 0 
    #Restart Explorer to change it immediately    
    Stop-Process -name explorer
}
DisableCortana
```

4. Run the above script to disable the Cortana service.
