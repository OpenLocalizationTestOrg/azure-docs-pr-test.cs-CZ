---
title: "Nasazení aplikací pomocí rozšíření virtuálního počítače aaaAutomating | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 79c91304-6c1b-4354-a185-fecc129b139d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d52537fbd4e935f19d3864def11484f519f8598
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-windows-vms"></a>Nasazení aplikací pomocí šablon Azure Resource Manageru pro virtuální počítače Windows

Po zjištění a přeložit na šablonu nasazení máte všechny požadavky pro Azure infrastruktury, musí nasazení skutečné aplikace hello toobe řešit. Nasazení aplikace zde odkazuje tooinstalling hello skutečné aplikačních binárních souborů do prostředků Azure. Pro ukázku hello Hudba úložiště, rozhraní .net Core a IIS potřebovat toobe nainstalovaná a nakonfigurovaná na každém virtuálním počítači. Hello Hudba úložiště binární soubory nutné toobe nainstaluje do hello virtuálního počítače a hello Hudba úložiště databáze předem vytvořené.

Tento dokument podrobně popisuje, jak rozšíření virtuálního počítače můžete automatizovat aplikace nasazení a konfigurace tooAzure virtuálních počítačů. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru. úplnou šablonu Hello naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="configuration-script"></a>Konfigurační skript
Rozšíření virtuálního počítače jsou specializované programy, které spuštění u virtuálních počítačů tooprovide konfigurace automatizace. Rozšíření jsou k dispozici pro mnoho specifické účely, například Ochrana proti virům, konfigurace protokolování a Docker konfigurace. použít toorun žádný skript pro virtuální počítač může být Hello rozšíření vlastních skriptů. Pomocí hello Ukázky hudby úložiště je až toohello vlastní skript rozšíření tooconfigure hello virtuální počítače s Windows a nainstalovat aplikaci Store Hudba hello.

Před s podrobnostmi o tom, jak rozšíření virtuálního počítače jsou deklarované v šablonu Azure Resource Manager, zkontrolujte hello skript, který je spuštěn. Tento skript nakonfiguruje hello Windows virtuální počítač toohost hello aplikace Hudba úložiště. Při spuštění skriptu hello nainstaluje všechny potřebné software, nainstalujte aplikaci store Hudba hello od správy zdrojového kódu a příprava hello databáze. 

> Tato ukázka je pro demonstrační účely.

```PowerShell
<#
    .SYNOPSIS
        Downloads and configures .Net Core Music Store application sample across IIS and Azure SQL DB.
#>

Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# Firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# Folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# Install iis
Install-WindowsFeature web-server -IncludeManagementTools

# Install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# Download music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music

# Azure SQL connection sting in environment variable
[Environment]::SetEnvironmentVariable("Data:DefaultConnection:ConnectionString", "Server=$sqlserver;Database=MusicStore;Integrated Security=False;User Id=$user;Password=$password;MultipleActiveResultSets=True;Connect Timeout=30",[EnvironmentVariableTarget]::Machine)

# Pre-create database
$env:Data:DefaultConnection:ConnectionString = "Server=$sqlserver;Database=MusicStore;Integrated Security=False;User Id=$user;Password=$password;MultipleActiveResultSets=True;Connect Timeout=30"
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# Configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>Skript rozšíření virtuálního počítače
Rozšíření virtuálního počítače lze spustit na virtuálním počítači v čase vytvoření buildu včetně hello rozšíření prostředků v hello šablony Azure Resource Manageru. rozšíření Hello lze přidat pomocí Průvodce přidáním prostředku Visual Studio hello nebo vložením platný kód JSON do šablony hello. Hello skriptu rozšíření prostředků je vnořit hello prostředek virtuálního počítače; To si můžete prohlédnout ve hello následující ukázka.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [rozšíření virtuálního počítače skriptu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Všimněte si v hello níže formátu JSON, který hello skript je uložený na webu GitHub. Tento skript by také může být uložený v úložišti objektů Blob Azure. Šablony Azure Resource Manager umožnit hello skriptu provádění řetězec toobe vytvořený tak, aby hodnoty parametrů šablony lze použít jako parametry pro spuštění skriptu. V takovém případě se data poskytuje při nasazování šablony hello a tyto hodnoty lze při provádění skriptu hello.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Jak je uvedeno nahoře, je také možné toostore vaše vlastní skripty v Azure Blob storage. Existují dvě možnosti pro ukládání hello skriptu prostředků v úložišti objektů blob; buď zajistěte hello veřejného kontejneru nebo skript a postupujte podle pokynů hello stejné přístupu, jak je uvedeno výše, nebo můžete také nacházet v privátní blob storage, který vyžaduje, abyste tooprovide hello storageAccountName a storageAccountKey toohello CustomScriptExtension definice prostředků.

V následujícím příkladu hello jsme další došli krok. I když je možné tooprovide hello název účtu úložiště a klíč jako parametr nebo proměnná během nasazení, Resource Manager šablony poskytují hello `listKeys` funkce, která můžete získat účet úložiště hello prostřednictvím kódu programu klíče a vložte ho do toohello Šablona pro vás v době nasazení.

V příkladu hello CustomScriptExtension definice prostředků níže, naše vlastních skriptů je už nahraný tooan účet úložiště Azure s názvem `mystorageaccount9999` který existuje v jiné skupině prostředků s názvem `mysa999rgname`. Když jsme nasadit šablonu, která obsahuje tento prostředek, hello `listKeys` funkce získává prostřednictvím kódu programu hello klíč účtu úložiště pro účet úložiště hello `mystorageaccount9999` v hello skupiny prostředků `mysa999rgname` a vloží je v šabloně toohello pro nás.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://mystorageaccount9999.blob.core.windows.net/container/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]",
      "storageAccountName": "mystorageaccount9999",
      "storageAccountKey": "[listKeys(resourceId('mysa999rgname','Microsoft.Storage/storageAccounts', mystorageaccount9999), '2015-06-15').key1]"
    }
  }
}
```

Hello hlavní výhody tohoto přístupu je, že není nutné je toochange šablony nebo parametry nasazení v události hello hello Změna klíče účtu úložiště.

Další informace o použití rozšíření vlastních skriptů hello najdete v tématu [vlastní skript rozšíření pomocí šablony Resource Manageru](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-step"></a>Další krok
<hr>

[Prozkoumejte šablony více Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates)

