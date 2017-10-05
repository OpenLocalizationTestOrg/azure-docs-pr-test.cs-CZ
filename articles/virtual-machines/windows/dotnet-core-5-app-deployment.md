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
# <a name="application-deployment-with-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="bf560-103">Nasazení aplikací pomocí šablon Azure Resource Manageru pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="bf560-103">Application deployment with Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="bf560-104">Po zjištění a přeložit na šablonu nasazení máte všechny požadavky pro Azure infrastruktury, nasazení skutečné aplikace, které musí vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="bf560-104">Once all Azure infrastructural requirements have been identified and translated into a deployment template, the actual application deployment needs to be addressed.</span></span> <span data-ttu-id="bf560-105">Nasazení aplikace v tomto poli odkazuje na instalaci binárních souborů skutečné aplikace do prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bf560-105">Application deployment here is referring to installing the actual application binaries onto Azure resources.</span></span> <span data-ttu-id="bf560-106">Pro ukázku Hudba úložiště, rozhraní .net Core a IIS musí být nainstalovaná a nakonfigurovaná na každém virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="bf560-106">For the Music Store sample, .Net Core and IIS need to be installed and configured on each virtual machine.</span></span> <span data-ttu-id="bf560-107">Binární soubory Hudba úložiště je potřeba nainstalovat na virtuální počítač a předem vytvořené databáze hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf560-107">The Music Store binaries need to be installed onto the virtual machine, and the Music Store database pre-created.</span></span>

<span data-ttu-id="bf560-108">Tento dokument podrobně popisuje, jak rozšíření virtuálního počítače můžete automatizovat nasazení aplikace a konfigurace virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="bf560-108">This document details how Virtual Machine extensions can automate application deployment and configuration to Azure virtual machines.</span></span> <span data-ttu-id="bf560-109">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="bf560-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="bf560-110">Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="bf560-110">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="bf560-111">Úplnou šablonu naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="bf560-111">The complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="configuration-script"></a><span data-ttu-id="bf560-112">Konfigurační skript</span><span class="sxs-lookup"><span data-stu-id="bf560-112">Configuration script</span></span>
<span data-ttu-id="bf560-113">Rozšíření virtuálního počítače jsou specializované programy, které provést před virtuálními počítači poskytnout automatizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bf560-113">Virtual Machine extensions are specialized programs that execute against virtual machines to provide configuration automation.</span></span> <span data-ttu-id="bf560-114">Rozšíření jsou k dispozici pro mnoho specifické účely, například Ochrana proti virům, konfigurace protokolování a Docker konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bf560-114">Extensions are available for many specific purposes such as anti-virus, logging configuration, and Docker configuration.</span></span> <span data-ttu-id="bf560-115">Rozšíření vlastních skriptů slouží ke spouštění všech skriptů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="bf560-115">The Custom Script extension can be used to run any script against a virtual machine.</span></span> <span data-ttu-id="bf560-116">Pomocí ukázky hudby úložiště je rozšíření vlastních skriptů konfigurovat virtuální počítače s Windows a nainstalovat aplikaci Store Hudba.</span><span class="sxs-lookup"><span data-stu-id="bf560-116">With the Music Store sample, it is up to the custom script extension to configure the Windows virtual machines and install the Music Store application.</span></span>

<span data-ttu-id="bf560-117">Před s podrobnostmi o tom, jak rozšíření virtuálního počítače jsou deklarované v šablonu Azure Resource Manager, zkontrolujte skript, který je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="bf560-117">Before detailing how virtual machine extensions are declared in an Azure Resource Manager template, examine the script that is run.</span></span> <span data-ttu-id="bf560-118">Tento skript nakonfiguruje virtuálního počítače s Windows, který bude hostovat aplikaci Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf560-118">This script configures the Windows virtual machine to host the Music Store application.</span></span> <span data-ttu-id="bf560-119">Při spuštění skriptu nainstaluje všechny, potřebný software, nainstalujte aplikaci store hudba od správy zdrojového kódu a Příprava databáze.</span><span class="sxs-lookup"><span data-stu-id="bf560-119">When run, the script installs all needed software, install the Music store application from source control, and prepare the database.</span></span> 

> <span data-ttu-id="bf560-120">Tato ukázka je pro demonstrační účely.</span><span class="sxs-lookup"><span data-stu-id="bf560-120">This sample is for demonstration purposes.</span></span>

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

## <a name="vm-script-extension"></a><span data-ttu-id="bf560-121">Skript rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bf560-121">VM Script Extension</span></span>
<span data-ttu-id="bf560-122">Rozšíření virtuálního počítače lze spustit na virtuálním počítači v čase vytvoření buildu, přiložením rozšíření prostředků do šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="bf560-122">VM Extensions can be run against a virtual machine at build time by including the extension resource in the Azure Resource Manager template.</span></span> <span data-ttu-id="bf560-123">Rozšíření můžete přidat pomocí Průvodce přidáním prostředku Visual Studio, nebo vložením platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="bf560-123">The extension can be added with the Visual Studio Add Resource wizard, or by inserting valid JSON into the template.</span></span> <span data-ttu-id="bf560-124">Skript rozšíření prostředků je vnořit prostředek virtuálního počítače; To můžete vidět v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="bf560-124">The Script Extension resource is nested inside the Virtual Machine resource; this can be seen in the following example.</span></span>

<span data-ttu-id="bf560-125">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [rozšíření virtuálního počítače skriptu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339).</span><span class="sxs-lookup"><span data-stu-id="bf560-125">Follow this link to see the JSON sample within the Resource Manager template – [VM Script Extension](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339).</span></span> 

<span data-ttu-id="bf560-126">Všimněte si v níže JSON, aby tento skript je uložený na webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="bf560-126">Notice in the below JSON that the script is stored in GitHub.</span></span> <span data-ttu-id="bf560-127">Tento skript by také může být uložený v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="bf560-127">This script could also be stored in Azure Blob storage.</span></span> <span data-ttu-id="bf560-128">Šablony Azure Resource Manager umožnit řetězec provádění skriptu, který má být vytvořený tak, aby hodnoty parametrů šablony lze použít jako parametry pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="bf560-128">Also, Azure Resource Manager templates allow the script execution string to be constructed such that template parameters values can be used as parameters for script execution.</span></span> <span data-ttu-id="bf560-129">V takovém případě se data zadaná při nasazování šablony a tyto hodnoty lze při provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="bf560-129">In this case data is provided when deploying the templates, and these values can then be used when executing the script.</span></span>

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

<span data-ttu-id="bf560-130">Jak je uvedeno nahoře, je také možné uložit vlastní skripty v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="bf560-130">As mentioned above, it is also possible to store your custom scripts in Azure Blob storage.</span></span> <span data-ttu-id="bf560-131">Existují dvě možnosti pro ukládání prostředků skriptu v úložišti objektů blob; buď zveřejnit kontejneru nebo skript a postupujte podle stejné jako vyšší přístupu, nebo můžete také uchovává v privátní blob storage, který vyžaduje, abyste zadejte storageAccountName a storageAccountKey CustomScriptExtension definici zdrojů.</span><span class="sxs-lookup"><span data-stu-id="bf560-131">There are two options for storing the script resources in blob storage; either make the container/script public and follow the same approach as above, or it can also be kept in private blob storage which requires you to provide the storageAccountName and storageAccountKey to the CustomScriptExtension resource definition.</span></span>

<span data-ttu-id="bf560-132">V následujícím příkladu jsme další došli krok.</span><span class="sxs-lookup"><span data-stu-id="bf560-132">In the example below we have gone a step further.</span></span> <span data-ttu-id="bf560-133">I když je možné zadat název účtu úložiště a klíč jako parametr nebo proměnná během nasazení, Resource Manager šablony poskytují `listKeys` funkce, která můžete získat klíč účtu úložiště prostřednictvím kódu programu a jeho vložení do šablony pro můžete v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="bf560-133">While it is possible to provide the storage account name and key as a parameter or variable during deployment, Resource Manager templates provide the `listKeys` function that can obtain the storage account key programmatically and insert it in to the template for you at deployment time.</span></span>

<span data-ttu-id="bf560-134">V příkladu níže definice prostředků CustomScriptExtension již byl odeslán naše vlastní skript k účtu úložiště Azure, názvem `mystorageaccount9999` který existuje v jiné skupině prostředků s názvem `mysa999rgname`.</span><span class="sxs-lookup"><span data-stu-id="bf560-134">In the example CustomScriptExtension resource definition below, our custom script has already been uploaded to an Azure storage account called `mystorageaccount9999` which exists in another Resource Group called `mysa999rgname`.</span></span> <span data-ttu-id="bf560-135">Když jsme nasazení šablony obsahující tento prostředek `listKeys` funkce prostřednictvím kódu programu získá klíč účtu úložiště pro účet úložiště `mystorageaccount9999` ve skupině prostředků `mysa999rgname` a vloží ho do šablony pro nás.</span><span class="sxs-lookup"><span data-stu-id="bf560-135">When we deploy a template containing this resource, the `listKeys` function programmatically obtains the storage account key for the storage account `mystorageaccount9999` in the Resource Group `mysa999rgname` and inserts it in to the template for us.</span></span>

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

<span data-ttu-id="bf560-136">Hlavní výhodou tohoto přístupu je, že nevyžaduje měnit parametry šablony nebo nasazení v případě úložiště účet klíče změnit.</span><span class="sxs-lookup"><span data-stu-id="bf560-136">The main benefit of this approach is that it does not require you to change your template or deployment parameters in the event of the storage account key changing.</span></span>

<span data-ttu-id="bf560-137">Další informace o použití rozšíření vlastních skriptů, najdete v části [vlastní skript rozšíření pomocí šablony Resource Manageru](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bf560-137">For more information on using the custom script extension, see [Custom script extensions with Resource Manager templates](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-step"></a><span data-ttu-id="bf560-138">Další krok</span><span class="sxs-lookup"><span data-stu-id="bf560-138">Next Step</span></span>
<hr>

[<span data-ttu-id="bf560-139">Prozkoumejte šablony více Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="bf560-139">Explore More Azure Resource Manager Templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

