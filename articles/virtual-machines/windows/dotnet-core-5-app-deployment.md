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
# <a name="application-deployment-with-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="cab9e-103">Nasazení aplikací pomocí šablon Azure Resource Manageru pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="cab9e-103">Application deployment with Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="cab9e-104">Po zjištění a přeložit na šablonu nasazení máte všechny požadavky pro Azure infrastruktury, musí nasazení skutečné aplikace hello toobe řešit.</span><span class="sxs-lookup"><span data-stu-id="cab9e-104">Once all Azure infrastructural requirements have been identified and translated into a deployment template, hello actual application deployment needs toobe addressed.</span></span> <span data-ttu-id="cab9e-105">Nasazení aplikace zde odkazuje tooinstalling hello skutečné aplikačních binárních souborů do prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="cab9e-105">Application deployment here is referring tooinstalling hello actual application binaries onto Azure resources.</span></span> <span data-ttu-id="cab9e-106">Pro ukázku hello Hudba úložiště, rozhraní .net Core a IIS potřebovat toobe nainstalovaná a nakonfigurovaná na každém virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="cab9e-106">For hello Music Store sample, .Net Core and IIS need toobe installed and configured on each virtual machine.</span></span> <span data-ttu-id="cab9e-107">Hello Hudba úložiště binární soubory nutné toobe nainstaluje do hello virtuálního počítače a hello Hudba úložiště databáze předem vytvořené.</span><span class="sxs-lookup"><span data-stu-id="cab9e-107">hello Music Store binaries need toobe installed onto hello virtual machine, and hello Music Store database pre-created.</span></span>

<span data-ttu-id="cab9e-108">Tento dokument podrobně popisuje, jak rozšíření virtuálního počítače můžete automatizovat aplikace nasazení a konfigurace tooAzure virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cab9e-108">This document details how Virtual Machine extensions can automate application deployment and configuration tooAzure virtual machines.</span></span> <span data-ttu-id="cab9e-109">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cab9e-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="cab9e-110">Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cab9e-110">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="cab9e-111">úplnou šablonu Hello naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="cab9e-111">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="configuration-script"></a><span data-ttu-id="cab9e-112">Konfigurační skript</span><span class="sxs-lookup"><span data-stu-id="cab9e-112">Configuration script</span></span>
<span data-ttu-id="cab9e-113">Rozšíření virtuálního počítače jsou specializované programy, které spuštění u virtuálních počítačů tooprovide konfigurace automatizace.</span><span class="sxs-lookup"><span data-stu-id="cab9e-113">Virtual Machine extensions are specialized programs that execute against virtual machines tooprovide configuration automation.</span></span> <span data-ttu-id="cab9e-114">Rozšíření jsou k dispozici pro mnoho specifické účely, například Ochrana proti virům, konfigurace protokolování a Docker konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cab9e-114">Extensions are available for many specific purposes such as anti-virus, logging configuration, and Docker configuration.</span></span> <span data-ttu-id="cab9e-115">použít toorun žádný skript pro virtuální počítač může být Hello rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="cab9e-115">hello Custom Script extension can be used toorun any script against a virtual machine.</span></span> <span data-ttu-id="cab9e-116">Pomocí hello Ukázky hudby úložiště je až toohello vlastní skript rozšíření tooconfigure hello virtuální počítače s Windows a nainstalovat aplikaci Store Hudba hello.</span><span class="sxs-lookup"><span data-stu-id="cab9e-116">With hello Music Store sample, it is up toohello custom script extension tooconfigure hello Windows virtual machines and install hello Music Store application.</span></span>

<span data-ttu-id="cab9e-117">Před s podrobnostmi o tom, jak rozšíření virtuálního počítače jsou deklarované v šablonu Azure Resource Manager, zkontrolujte hello skript, který je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="cab9e-117">Before detailing how virtual machine extensions are declared in an Azure Resource Manager template, examine hello script that is run.</span></span> <span data-ttu-id="cab9e-118">Tento skript nakonfiguruje hello Windows virtuální počítač toohost hello aplikace Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="cab9e-118">This script configures hello Windows virtual machine toohost hello Music Store application.</span></span> <span data-ttu-id="cab9e-119">Při spuštění skriptu hello nainstaluje všechny potřebné software, nainstalujte aplikaci store Hudba hello od správy zdrojového kódu a příprava hello databáze.</span><span class="sxs-lookup"><span data-stu-id="cab9e-119">When run, hello script installs all needed software, install hello Music store application from source control, and prepare hello database.</span></span> 

> <span data-ttu-id="cab9e-120">Tato ukázka je pro demonstrační účely.</span><span class="sxs-lookup"><span data-stu-id="cab9e-120">This sample is for demonstration purposes.</span></span>

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

## <a name="vm-script-extension"></a><span data-ttu-id="cab9e-121">Skript rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cab9e-121">VM Script Extension</span></span>
<span data-ttu-id="cab9e-122">Rozšíření virtuálního počítače lze spustit na virtuálním počítači v čase vytvoření buildu včetně hello rozšíření prostředků v hello šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cab9e-122">VM Extensions can be run against a virtual machine at build time by including hello extension resource in hello Azure Resource Manager template.</span></span> <span data-ttu-id="cab9e-123">rozšíření Hello lze přidat pomocí Průvodce přidáním prostředku Visual Studio hello nebo vložením platný kód JSON do šablony hello.</span><span class="sxs-lookup"><span data-stu-id="cab9e-123">hello extension can be added with hello Visual Studio Add Resource wizard, or by inserting valid JSON into hello template.</span></span> <span data-ttu-id="cab9e-124">Hello skriptu rozšíření prostředků je vnořit hello prostředek virtuálního počítače; To si můžete prohlédnout ve hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="cab9e-124">hello Script Extension resource is nested inside hello Virtual Machine resource; this can be seen in hello following example.</span></span>

<span data-ttu-id="cab9e-125">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [rozšíření virtuálního počítače skriptu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339).</span><span class="sxs-lookup"><span data-stu-id="cab9e-125">Follow this link toosee hello JSON sample within hello Resource Manager template – [VM Script Extension](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339).</span></span> 

<span data-ttu-id="cab9e-126">Všimněte si v hello níže formátu JSON, který hello skript je uložený na webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="cab9e-126">Notice in hello below JSON that hello script is stored in GitHub.</span></span> <span data-ttu-id="cab9e-127">Tento skript by také může být uložený v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="cab9e-127">This script could also be stored in Azure Blob storage.</span></span> <span data-ttu-id="cab9e-128">Šablony Azure Resource Manager umožnit hello skriptu provádění řetězec toobe vytvořený tak, aby hodnoty parametrů šablony lze použít jako parametry pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="cab9e-128">Also, Azure Resource Manager templates allow hello script execution string toobe constructed such that template parameters values can be used as parameters for script execution.</span></span> <span data-ttu-id="cab9e-129">V takovém případě se data poskytuje při nasazování šablony hello a tyto hodnoty lze při provádění skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="cab9e-129">In this case data is provided when deploying hello templates, and these values can then be used when executing hello script.</span></span>

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

<span data-ttu-id="cab9e-130">Jak je uvedeno nahoře, je také možné toostore vaše vlastní skripty v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="cab9e-130">As mentioned above, it is also possible toostore your custom scripts in Azure Blob storage.</span></span> <span data-ttu-id="cab9e-131">Existují dvě možnosti pro ukládání hello skriptu prostředků v úložišti objektů blob; buď zajistěte hello veřejného kontejneru nebo skript a postupujte podle pokynů hello stejné přístupu, jak je uvedeno výše, nebo můžete také nacházet v privátní blob storage, který vyžaduje, abyste tooprovide hello storageAccountName a storageAccountKey toohello CustomScriptExtension definice prostředků.</span><span class="sxs-lookup"><span data-stu-id="cab9e-131">There are two options for storing hello script resources in blob storage; either make hello container/script public and follow hello same approach as above, or it can also be kept in private blob storage which requires you tooprovide hello storageAccountName and storageAccountKey toohello CustomScriptExtension resource definition.</span></span>

<span data-ttu-id="cab9e-132">V následujícím příkladu hello jsme další došli krok.</span><span class="sxs-lookup"><span data-stu-id="cab9e-132">In hello example below we have gone a step further.</span></span> <span data-ttu-id="cab9e-133">I když je možné tooprovide hello název účtu úložiště a klíč jako parametr nebo proměnná během nasazení, Resource Manager šablony poskytují hello `listKeys` funkce, která můžete získat účet úložiště hello prostřednictvím kódu programu klíče a vložte ho do toohello Šablona pro vás v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="cab9e-133">While it is possible tooprovide hello storage account name and key as a parameter or variable during deployment, Resource Manager templates provide hello `listKeys` function that can obtain hello storage account key programmatically and insert it in toohello template for you at deployment time.</span></span>

<span data-ttu-id="cab9e-134">V příkladu hello CustomScriptExtension definice prostředků níže, naše vlastních skriptů je už nahraný tooan účet úložiště Azure s názvem `mystorageaccount9999` který existuje v jiné skupině prostředků s názvem `mysa999rgname`.</span><span class="sxs-lookup"><span data-stu-id="cab9e-134">In hello example CustomScriptExtension resource definition below, our custom script has already been uploaded tooan Azure storage account called `mystorageaccount9999` which exists in another Resource Group called `mysa999rgname`.</span></span> <span data-ttu-id="cab9e-135">Když jsme nasadit šablonu, která obsahuje tento prostředek, hello `listKeys` funkce získává prostřednictvím kódu programu hello klíč účtu úložiště pro účet úložiště hello `mystorageaccount9999` v hello skupiny prostředků `mysa999rgname` a vloží je v šabloně toohello pro nás.</span><span class="sxs-lookup"><span data-stu-id="cab9e-135">When we deploy a template containing this resource, hello `listKeys` function programmatically obtains hello storage account key for hello storage account `mystorageaccount9999` in hello Resource Group `mysa999rgname` and inserts it in toohello template for us.</span></span>

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

<span data-ttu-id="cab9e-136">Hello hlavní výhody tohoto přístupu je, že není nutné je toochange šablony nebo parametry nasazení v události hello hello Změna klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cab9e-136">hello main benefit of this approach is that it does not require you toochange your template or deployment parameters in hello event of hello storage account key changing.</span></span>

<span data-ttu-id="cab9e-137">Další informace o použití rozšíření vlastních skriptů hello najdete v tématu [vlastní skript rozšíření pomocí šablony Resource Manageru](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cab9e-137">For more information on using hello custom script extension, see [Custom script extensions with Resource Manager templates](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-step"></a><span data-ttu-id="cab9e-138">Další krok</span><span class="sxs-lookup"><span data-stu-id="cab9e-138">Next Step</span></span>
<hr>

[<span data-ttu-id="cab9e-139">Prozkoumejte šablony více Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="cab9e-139">Explore More Azure Resource Manager Templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

