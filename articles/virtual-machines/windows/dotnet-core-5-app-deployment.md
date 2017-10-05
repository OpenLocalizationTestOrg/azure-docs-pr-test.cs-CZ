---
title: "Automatizace nasazení aplikací pomocí rozšíření virtuálního počítače | Microsoft Docs"
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
ms.openlocfilehash: f2996eef71b39c6240fac5484854f72d3e657d0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-windows-vms"></a>Nasazení aplikací pomocí šablon Azure Resource Manageru pro virtuální počítače Windows

Po zjištění a přeložit na šablonu nasazení máte všechny požadavky pro Azure infrastruktury, nasazení skutečné aplikace, které musí vzít v úvahu. Nasazení aplikace v tomto poli odkazuje na instalaci binárních souborů skutečné aplikace do prostředků Azure. Pro ukázku Hudba úložiště, rozhraní .net Core a IIS musí být nainstalovaná a nakonfigurovaná na každém virtuálním počítači. Binární soubory Hudba úložiště je potřeba nainstalovat na virtuální počítač a předem vytvořené databáze hudba úložiště.

Tento dokument podrobně popisuje, jak rozšíření virtuálního počítače můžete automatizovat nasazení aplikace a konfigurace virtuálních počítačů Azure. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru. Úplnou šablonu naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="configuration-script"></a>Konfigurační skript
Rozšíření virtuálního počítače jsou specializované programy, které provést před virtuálními počítači poskytnout automatizace konfigurace. Rozšíření jsou k dispozici pro mnoho specifické účely, například Ochrana proti virům, konfigurace protokolování a Docker konfigurace. Rozšíření vlastních skriptů slouží ke spouštění všech skriptů na virtuálním počítači. Pomocí ukázky hudby úložiště je rozšíření vlastních skriptů konfigurovat virtuální počítače s Windows a nainstalovat aplikaci Store Hudba.

Před s podrobnostmi o tom, jak rozšíření virtuálního počítače jsou deklarované v šablonu Azure Resource Manager, zkontrolujte skript, který je spuštěn. Tento skript nakonfiguruje virtuálního počítače s Windows, který bude hostovat aplikaci Hudba úložiště. Při spuštění skriptu nainstaluje všechny, potřebný software, nainstalujte aplikaci store hudba od správy zdrojového kódu a Příprava databáze. 

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
Rozšíření virtuálního počítače lze spustit na virtuálním počítači v čase vytvoření buildu, přiložením rozšíření prostředků do šablony Azure Resource Manageru. Rozšíření můžete přidat pomocí Průvodce přidáním prostředku Visual Studio, nebo vložením platný kód JSON do šablony. Skript rozšíření prostředků je vnořit prostředek virtuálního počítače; To můžete vidět v následujícím příkladu.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [rozšíření virtuálního počítače skriptu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Všimněte si v níže JSON, aby tento skript je uložený na webu GitHub. Tento skript by také může být uložený v úložišti objektů Blob Azure. Šablony Azure Resource Manager umožnit řetězec provádění skriptu, který má být vytvořený tak, aby hodnoty parametrů šablony lze použít jako parametry pro spuštění skriptu. V takovém případě se data zadaná při nasazování šablony a tyto hodnoty lze při provádění skriptu.

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

Jak je uvedeno nahoře, je také možné uložit vlastní skripty v Azure Blob storage. Existují dvě možnosti pro ukládání prostředků skriptu v úložišti objektů blob; buď zveřejnit kontejneru nebo skript a postupujte podle stejné jako vyšší přístupu, nebo můžete také uchovává v privátní blob storage, který vyžaduje, abyste zadejte storageAccountName a storageAccountKey CustomScriptExtension definici zdrojů.

V následujícím příkladu jsme další došli krok. I když je možné zadat název účtu úložiště a klíč jako parametr nebo proměnná během nasazení, Resource Manager šablony poskytují `listKeys` funkce, která můžete získat klíč účtu úložiště prostřednictvím kódu programu a jeho vložení do šablony pro můžete v době nasazení.

V příkladu níže definice prostředků CustomScriptExtension již byl odeslán naše vlastní skript k účtu úložiště Azure, názvem `mystorageaccount9999` který existuje v jiné skupině prostředků s názvem `mysa999rgname`. Když jsme nasazení šablony obsahující tento prostředek `listKeys` funkce prostřednictvím kódu programu získá klíč účtu úložiště pro účet úložiště `mystorageaccount9999` ve skupině prostředků `mysa999rgname` a vloží ho do šablony pro nás.

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

Hlavní výhodou tohoto přístupu je, že nevyžaduje měnit parametry šablony nebo nasazení v případě úložiště účet klíče změnit.

Další informace o použití rozšíření vlastních skriptů, najdete v části [vlastní skript rozšíření pomocí šablony Resource Manageru](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-step"></a>Další krok
<hr>

[Prozkoumejte šablony více Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates)

