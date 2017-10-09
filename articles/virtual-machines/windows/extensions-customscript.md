---
title: "aaaAzure vlastní skript rozšíření pro Windows | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače s Windows pomocí rozšíření vlastních skriptů hello"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: nepeters
ms.openlocfilehash: 97e065242e9fed116ee20b074f4e302a0cd10585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows"></a><span data-ttu-id="397d0-103">Rozšíření vlastních skriptů pro Windows</span><span class="sxs-lookup"><span data-stu-id="397d0-103">Custom Script Extension for Windows</span></span>

<span data-ttu-id="397d0-104">Hello rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="397d0-104">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="397d0-105">Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy.</span><span class="sxs-lookup"><span data-stu-id="397d0-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="397d0-106">Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat toohello portál Azure na dobu běhu rozšíření.</span><span class="sxs-lookup"><span data-stu-id="397d0-106">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span> <span data-ttu-id="397d0-107">Hello rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí hello rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo hello REST API pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="397d0-107">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="397d0-108">Tento dokument podrobně popisuje, jak pomocí rozšíření vlastních skriptů hello toouse hello modul Azure PowerShell, šablon Azure Resource Manageru a podrobnosti o řešení potíží s kroky v systémech Windows.</span><span class="sxs-lookup"><span data-stu-id="397d0-108">This document details how toouse hello Custom Script Extension using hello Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="397d0-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="397d0-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="397d0-110">Operační systém</span><span class="sxs-lookup"><span data-stu-id="397d0-110">Operating System</span></span>

<span data-ttu-id="397d0-111">Hello rozšíření vlastních skriptů pro pro Windows Server 2008 R2, můžete spouštět Windows 2012, 2012 R2 a 2016 uvolní.</span><span class="sxs-lookup"><span data-stu-id="397d0-111">hello Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="397d0-112">Umístění skriptu</span><span class="sxs-lookup"><span data-stu-id="397d0-112">Script Location</span></span>

<span data-ttu-id="397d0-113">skript Hello musí toobe uložení do úložiště objektů Blob v Azure nebo jakéhokoli jiného umístění, které jsou přístupné prostřednictvím platnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="397d0-113">hello script needs toobe stored in Azure Blob storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="397d0-114">Připojení k Internetu</span><span class="sxs-lookup"><span data-stu-id="397d0-114">Internet Connectivity</span></span>

<span data-ttu-id="397d0-115">Hello vlastní skript rozšíření pro Windows vyžaduje, aby hello cílový virtuální počítač je připojený toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="397d0-115">hello Custom Script Extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="397d0-116">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="397d0-116">Extension schema</span></span>

<span data-ttu-id="397d0-117">Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="397d0-117">hello following JSON shows hello schema for hello Custom Script Extension.</span></span> <span data-ttu-id="397d0-118">rozšíření Hello vyžaduje umístění skriptu (Azure Storage nebo jiného umístění s platnou adresu URL) a příkaz tooexecute.</span><span class="sxs-lookup"><span data-stu-id="397d0-118">hello extension requires a script location (Azure Storage or other location with valid URL), and a command tooexecute.</span></span> <span data-ttu-id="397d0-119">Pokud používáte Azure Storage jako zdroj skriptu hello, je třeba klíč účtu úložiště Azure účet a název.</span><span class="sxs-lookup"><span data-stu-id="397d0-119">If using Azure Storage as hello script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="397d0-120">Tyto položky by měl být považován za citlivá data a zadaný v konfiguraci chráněných nastavení rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="397d0-120">These items should be treated as sensitive data and specified in hello extensions protected setting configuration.</span></span> <span data-ttu-id="397d0-121">Azure data virtuálního počítače chráněný rozšíření nastavení je zašifrovaná a dešifrovat jenom na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="397d0-121">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span>

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
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a><span data-ttu-id="397d0-122">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="397d0-122">Property values</span></span>

| <span data-ttu-id="397d0-123">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="397d0-123">Name</span></span> | <span data-ttu-id="397d0-124">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="397d0-124">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="397d0-125">apiVersion</span><span class="sxs-lookup"><span data-stu-id="397d0-125">apiVersion</span></span> | <span data-ttu-id="397d0-126">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="397d0-126">2015-06-15</span></span> |
| <span data-ttu-id="397d0-127">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="397d0-127">publisher</span></span> | <span data-ttu-id="397d0-128">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="397d0-128">Microsoft.Compute</span></span> |
| <span data-ttu-id="397d0-129">type</span><span class="sxs-lookup"><span data-stu-id="397d0-129">type</span></span> | <span data-ttu-id="397d0-130">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="397d0-130">extensions</span></span> |
| <span data-ttu-id="397d0-131">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="397d0-131">typeHandlerVersion</span></span> | <span data-ttu-id="397d0-132">1.9</span><span class="sxs-lookup"><span data-stu-id="397d0-132">1.9</span></span> |
| <span data-ttu-id="397d0-133">fileUris (např.)</span><span class="sxs-lookup"><span data-stu-id="397d0-133">fileUris (e.g)</span></span> | <span data-ttu-id="397d0-134">https://RAW.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="397d0-134">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="397d0-135">commandToExecute (např.)</span><span class="sxs-lookup"><span data-stu-id="397d0-135">commandToExecute (e.g)</span></span> | <span data-ttu-id="397d0-136">prostředí PowerShell - ExecutionPolicy Unrestricted - souboru konfigurace app.ps1 Hudba</span><span class="sxs-lookup"><span data-stu-id="397d0-136">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |
| <span data-ttu-id="397d0-137">storageAccountName (např.)</span><span class="sxs-lookup"><span data-stu-id="397d0-137">storageAccountName (e.g)</span></span> | <span data-ttu-id="397d0-138">examplestorageacct</span><span class="sxs-lookup"><span data-stu-id="397d0-138">examplestorageacct</span></span> |
| <span data-ttu-id="397d0-139">storageAccountKey (např.)</span><span class="sxs-lookup"><span data-stu-id="397d0-139">storageAccountKey (e.g)</span></span> | <span data-ttu-id="397d0-140">TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg ==</span><span class="sxs-lookup"><span data-stu-id="397d0-140">TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg==</span></span> |

<span data-ttu-id="397d0-141">**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="397d0-141">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="397d0-142">Pomocí názvů hello, jak je vidět výše tooavoid problémy při nasazení.</span><span class="sxs-lookup"><span data-stu-id="397d0-142">Use hello names as seen above tooavoid deployment issues.</span></span>

## <a name="template-deployment"></a><span data-ttu-id="397d0-143">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="397d0-143">Template deployment</span></span>

<span data-ttu-id="397d0-144">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="397d0-144">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="397d0-145">popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager rozšíření vlastních skriptů při nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="397d0-145">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="397d0-146">Ukázka šablony, která obsahuje hello rozšíření vlastních skriptů je zde uveden, [Githubu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="397d0-146">A sample template that includes hello Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="397d0-147">Nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="397d0-147">PowerShell deployment</span></span>

<span data-ttu-id="397d0-148">Hello `Set-AzureRmVMCustomScriptExtension` příkaz lze použít tooadd hello vlastní skript rozšíření tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="397d0-148">hello `Set-AzureRmVMCustomScriptExtension` command can be used tooadd hello Custom Script extension tooan existing virtual machine.</span></span> <span data-ttu-id="397d0-149">Další informace najdete v tématu [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="397d0-149">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="397d0-150">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="397d0-150">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="397d0-151">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="397d0-151">Troubleshoot</span></span>

<span data-ttu-id="397d0-152">Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí modulu Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="397d0-152">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="397d0-153">Stav nasazení toosee hello rozšíření pro daný virtuální počítač, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="397d0-153">toosee hello deployment state of extensions for a given VM, run hello following command.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="397d0-154">Rozšíření spuštění výstupu je zaznamenané toofiles v hello následující adresáře na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="397d0-154">Extension execution output is logged toofiles found under hello following directory on hello target virtual machine.</span></span>
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="397d0-155">Hello zadat, že soubory se stahují do hello následující adresáře na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="397d0-155">hello specified files are downloaded into hello following directory on hello target virtual machine.</span></span>
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
<span data-ttu-id="397d0-156">kde `<n>` je desítkové celé číslo, které může změnit mezi jednotlivými spuštěními hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="397d0-156">where `<n>` is a decimal integer which may change between executions of hello extension.</span></span>  <span data-ttu-id="397d0-157">Hello `1.*` hodnota odpovídá skutečným, aktuální hello `typeHandlerVersion` hodnota hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="397d0-157">hello `1.*` value matches hello actual, current `typeHandlerVersion` value of hello extension.</span></span>  <span data-ttu-id="397d0-158">Například může být hello skutečný adresář `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span><span class="sxs-lookup"><span data-stu-id="397d0-158">For example, hello actual directory could be `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span></span>  

<span data-ttu-id="397d0-159">Při provádění hello `commandToExecute` příkaz hello rozšíření bude nastavili tento adresář (například `...\Downloads\2`) jako hello aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="397d0-159">When executing hello `commandToExecute` command, hello extension will have set this directory (e.g., `...\Downloads\2`) as hello current working directory.</span></span> <span data-ttu-id="397d0-160">Tato umožňuje hello použít relativní cesty toolocate hello souborů stažené prostřednictvím hello `fileURIs` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="397d0-160">This enables hello use of relative paths toolocate hello files downloaded via hello `fileURIs` property.</span></span> <span data-ttu-id="397d0-161">Viz následující příklady tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="397d0-161">See hello table below for examples.</span></span>

<span data-ttu-id="397d0-162">Vzhledem k tomu, že hello stažení absolutní cesta se může lišit v čase je lepší tooopt pro relativní skriptu nebo cesty k souboru v hello `commandToExecute` řetězce, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="397d0-162">Since hello absolute download path may vary over time, it is better tooopt for relative script/file paths in hello `commandToExecute` string, whenever possible.</span></span> <span data-ttu-id="397d0-163">Například:</span><span class="sxs-lookup"><span data-stu-id="397d0-163">For example:</span></span>
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

<span data-ttu-id="397d0-164">Informace o cestě po první segment identifikátoru URI hello se zachovává kvůli soubory stáhli prostřednictvím hello `fileUris` seznam vlastností.</span><span class="sxs-lookup"><span data-stu-id="397d0-164">Path information after hello first URI segment is retained for files downloaded via hello `fileUris` property list.</span></span>  <span data-ttu-id="397d0-165">Jak ukazuje následující tabulka hello, stažené soubory jsou mapované na stažení podadresáře tooreflect hello struktura hello `fileUris` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="397d0-165">As shown in hello table below, downloaded files are mapped into download subdirectories tooreflect hello structure of hello `fileUris` values.</span></span>  

#### <a name="examples-of-downloaded-files"></a><span data-ttu-id="397d0-166">Příklady stažené soubory</span><span class="sxs-lookup"><span data-stu-id="397d0-166">Examples of Downloaded Files</span></span>

| <span data-ttu-id="397d0-167">Identifikátor URI v fileUris</span><span class="sxs-lookup"><span data-stu-id="397d0-167">URI in fileUris</span></span> | <span data-ttu-id="397d0-168">Relativní umístění stažené</span><span class="sxs-lookup"><span data-stu-id="397d0-168">Relative downloaded location</span></span> | <span data-ttu-id="397d0-169">Absolutní stáhli umístění *</span><span class="sxs-lookup"><span data-stu-id="397d0-169">Absolute downloaded location *</span></span> |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

<span data-ttu-id="397d0-170">\*Jako výš, hello absolutní adresářové cesty se změní průběhu životnosti hello hello virtuálních počítačů, ale není v rámci jednoho spuštění rozšíření CustomScript hello.</span><span class="sxs-lookup"><span data-stu-id="397d0-170">\* As above, hello absolute directory paths will change over hello lifetime of hello VM, but not within a single execution of hello CustomScript extension.</span></span>

### <a name="support"></a><span data-ttu-id="397d0-171">Podpora</span><span class="sxs-lookup"><span data-stu-id="397d0-171">Support</span></span>

<span data-ttu-id="397d0-172">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow] (https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="397d0-172">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums] (https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="397d0-173">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="397d0-173">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="397d0-174">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="397d0-174">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="397d0-175">Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="397d0-175">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
