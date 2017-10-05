---
title: "Rozšíření Azure vlastních skriptů pro Windows | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače s Windows pomocí rozšíření vlastních skriptů"
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
ms.openlocfilehash: a6f417ea6575b81258998ae3b31c10e9df59b603
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="custom-script-extension-for-windows"></a><span data-ttu-id="8e5c0-103">Rozšíření vlastních skriptů pro Windows</span><span class="sxs-lookup"><span data-stu-id="8e5c0-103">Custom Script Extension for Windows</span></span>

<span data-ttu-id="8e5c0-104">Rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-104">The Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="8e5c0-105">Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="8e5c0-106">Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat na portál Azure na dobu běhu rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-106">Scripts can be downloaded from Azure storage or GitHub, or provided to the Azure portal at extension run time.</span></span> <span data-ttu-id="8e5c0-107">Rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo REST API pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-107">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="8e5c0-108">Tento dokument podrobně popisuje postup používání rozšíření vlastních skriptů pomocí modulu Azure PowerShell, šablon Azure Resource Manageru a podrobnosti o řešení potíží s kroky v systémech Windows.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-108">This document details how to use the Custom Script Extension using the Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e5c0-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8e5c0-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="8e5c0-110">Operační systém</span><span class="sxs-lookup"><span data-stu-id="8e5c0-110">Operating System</span></span>

<span data-ttu-id="8e5c0-111">Rozšíření vlastních skriptů pro pro Windows Server 2008 R2, můžete spouštět Windows 2012, 2012 R2 a 2016 uvolní.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-111">The Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="8e5c0-112">Umístění skriptu</span><span class="sxs-lookup"><span data-stu-id="8e5c0-112">Script Location</span></span>

<span data-ttu-id="8e5c0-113">Skript musí být uložen do úložiště objektů Blob v Azure, nebo jakéhokoli jiného umístění, které jsou přístupné prostřednictvím platnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-113">The script needs to be stored in Azure Blob storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="8e5c0-114">Připojení k Internetu</span><span class="sxs-lookup"><span data-stu-id="8e5c0-114">Internet Connectivity</span></span>

<span data-ttu-id="8e5c0-115">Vlastní skript rozšíření pro Windows vyžaduje, aby cílový virtuální počítač je připojený k Internetu.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-115">The Custom Script Extension for Windows requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="8e5c0-116">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="8e5c0-116">Extension schema</span></span>

<span data-ttu-id="8e5c0-117">Následujícím kódu JSON znázorňuje schéma pro rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-117">The following JSON shows the schema for the Custom Script Extension.</span></span> <span data-ttu-id="8e5c0-118">Rozšíření vyžaduje umístění skriptu (Azure Storage nebo jiného umístění s platnou adresu URL) a příkaz ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-118">The extension requires a script location (Azure Storage or other location with valid URL), and a command to execute.</span></span> <span data-ttu-id="8e5c0-119">Pokud používáte Azure Storage jako zdroj skriptu, je třeba klíč účtu úložiště Azure účet a název.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-119">If using Azure Storage as the script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="8e5c0-120">Tyto položky by měl být považován za citlivá data a zadaný v konfiguraci chráněných nastavení rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-120">These items should be treated as sensitive data and specified in the extensions protected setting configuration.</span></span> <span data-ttu-id="8e5c0-121">Data Azure nastavení rozšíření chráněný virtuální počítač je zašifrovaná a dešifrovat jenom na cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-121">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="8e5c0-122">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="8e5c0-122">Property values</span></span>

| <span data-ttu-id="8e5c0-123">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8e5c0-123">Name</span></span> | <span data-ttu-id="8e5c0-124">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="8e5c0-124">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="8e5c0-125">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8e5c0-125">apiVersion</span></span> | <span data-ttu-id="8e5c0-126">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="8e5c0-126">2015-06-15</span></span> |
| <span data-ttu-id="8e5c0-127">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="8e5c0-127">publisher</span></span> | <span data-ttu-id="8e5c0-128">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="8e5c0-128">Microsoft.Compute</span></span> |
| <span data-ttu-id="8e5c0-129">type</span><span class="sxs-lookup"><span data-stu-id="8e5c0-129">type</span></span> | <span data-ttu-id="8e5c0-130">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="8e5c0-130">extensions</span></span> |
| <span data-ttu-id="8e5c0-131">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="8e5c0-131">typeHandlerVersion</span></span> | <span data-ttu-id="8e5c0-132">1.9</span><span class="sxs-lookup"><span data-stu-id="8e5c0-132">1.9</span></span> |
| <span data-ttu-id="8e5c0-133">fileUris (např.)</span><span class="sxs-lookup"><span data-stu-id="8e5c0-133">fileUris (e.g)</span></span> | <span data-ttu-id="8e5c0-134">https://RAW.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="8e5c0-134">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="8e5c0-135">commandToExecute (např.)</span><span class="sxs-lookup"><span data-stu-id="8e5c0-135">commandToExecute (e.g)</span></span> | <span data-ttu-id="8e5c0-136">prostředí PowerShell - ExecutionPolicy Unrestricted - souboru konfigurace app.ps1 Hudba</span><span class="sxs-lookup"><span data-stu-id="8e5c0-136">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |
| <span data-ttu-id="8e5c0-137">storageAccountName (např.)</span><span class="sxs-lookup"><span data-stu-id="8e5c0-137">storageAccountName (e.g)</span></span> | <span data-ttu-id="8e5c0-138">examplestorageacct</span><span class="sxs-lookup"><span data-stu-id="8e5c0-138">examplestorageacct</span></span> |
| <span data-ttu-id="8e5c0-139">storageAccountKey (např.)</span><span class="sxs-lookup"><span data-stu-id="8e5c0-139">storageAccountKey (e.g)</span></span> | <span data-ttu-id="8e5c0-140">TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg ==</span><span class="sxs-lookup"><span data-stu-id="8e5c0-140">TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg==</span></span> |

<span data-ttu-id="8e5c0-141">**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-141">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="8e5c0-142">Používejte názvy, jak je vidět výše, abyste předešli problémům s nasazení.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-142">Use the names as seen above to avoid deployment issues.</span></span>

## <a name="template-deployment"></a><span data-ttu-id="8e5c0-143">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="8e5c0-143">Template deployment</span></span>

<span data-ttu-id="8e5c0-144">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-144">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="8e5c0-145">Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření vlastních skriptů při nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-145">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="8e5c0-146">Ukázka šablony, která obsahuje rozšíření vlastních skriptů je zde uveden, [Githubu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="8e5c0-146">A sample template that includes the Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="8e5c0-147">Nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e5c0-147">PowerShell deployment</span></span>

<span data-ttu-id="8e5c0-148">`Set-AzureRmVMCustomScriptExtension` Příkaz lze použít k přidání rozšíření vlastních skriptů do existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-148">The `Set-AzureRmVMCustomScriptExtension` command can be used to add the Custom Script extension to an existing virtual machine.</span></span> <span data-ttu-id="8e5c0-149">Další informace najdete v tématu [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="8e5c0-149">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="8e5c0-150">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="8e5c0-150">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="8e5c0-151">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="8e5c0-151">Troubleshoot</span></span>

<span data-ttu-id="8e5c0-152">Data o stavu nasazení rozšíření mohou být načteny z portálu Azure a pomocí modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-152">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="8e5c0-153">Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-153">To see the deployment state of extensions for a given VM, run the following command.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="8e5c0-154">Soubory, které jsou v adresáři následující na cílový virtuální počítač je protokolovány výstupu spuštění rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-154">Extension execution output is logged to files found under the following directory on the target virtual machine.</span></span>
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="8e5c0-155">Zadané soubory staženy do následujícího adresáře na cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-155">The specified files are downloaded into the following directory on the target virtual machine.</span></span>
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
<span data-ttu-id="8e5c0-156">kde `<n>` je desítkové celé číslo, které může změnit mezi jednotlivými spuštěními rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-156">where `<n>` is a decimal integer which may change between executions of the extension.</span></span>  <span data-ttu-id="8e5c0-157">`1.*` Hodnota odpovídá skutečným, aktuální `typeHandlerVersion` hodnotu rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-157">The `1.*` value matches the actual, current `typeHandlerVersion` value of the extension.</span></span>  <span data-ttu-id="8e5c0-158">Skutečný adresář může být například `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-158">For example, the actual directory could be `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span></span>  

<span data-ttu-id="8e5c0-159">Při provádění `commandToExecute` příkaz, bude mít tento adresář nastavit rozšíření (například `...\Downloads\2`) jako aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-159">When executing the `commandToExecute` command, the extension will have set this directory (e.g., `...\Downloads\2`) as the current working directory.</span></span> <span data-ttu-id="8e5c0-160">To umožňuje použití relativní cesty vyhledejte soubory stáhli prostřednictvím `fileURIs` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-160">This enables the use of relative paths to locate the files downloaded via the `fileURIs` property.</span></span> <span data-ttu-id="8e5c0-161">Najdete v následující tabulce příklady.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-161">See the table below for examples.</span></span>

<span data-ttu-id="8e5c0-162">Vzhledem k tomu, že cesta pro stažení absolutní se mohou lišit v čase, je lepší zvolit relativní skriptu nebo cesty k souboru v `commandToExecute` řetězce, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-162">Since the absolute download path may vary over time, it is better to opt for relative script/file paths in the `commandToExecute` string, whenever possible.</span></span> <span data-ttu-id="8e5c0-163">Například:</span><span class="sxs-lookup"><span data-stu-id="8e5c0-163">For example:</span></span>
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

<span data-ttu-id="8e5c0-164">Informace o cestě po první segment identifikátoru URI se zachovává kvůli soubory stažené prostřednictvím `fileUris` seznam vlastností.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-164">Path information after the first URI segment is retained for files downloaded via the `fileUris` property list.</span></span>  <span data-ttu-id="8e5c0-165">Jak je znázorněno v následující tabulce, stažené soubory jsou mapovány na stažení podadresáře tak, aby odrážela strukturu `fileUris` hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-165">As shown in the table below, downloaded files are mapped into download subdirectories to reflect the structure of the `fileUris` values.</span></span>  

#### <a name="examples-of-downloaded-files"></a><span data-ttu-id="8e5c0-166">Příklady stažené soubory</span><span class="sxs-lookup"><span data-stu-id="8e5c0-166">Examples of Downloaded Files</span></span>

| <span data-ttu-id="8e5c0-167">Identifikátor URI v fileUris</span><span class="sxs-lookup"><span data-stu-id="8e5c0-167">URI in fileUris</span></span> | <span data-ttu-id="8e5c0-168">Relativní umístění stažené</span><span class="sxs-lookup"><span data-stu-id="8e5c0-168">Relative downloaded location</span></span> | <span data-ttu-id="8e5c0-169">Absolutní stáhli umístění *</span><span class="sxs-lookup"><span data-stu-id="8e5c0-169">Absolute downloaded location *</span></span> |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

<span data-ttu-id="8e5c0-170">\*Jako výš, cesty absolutní adresářů se změní za dobu virtuálního počítače, ale není v rámci jednoho spuštění rozšíření CustomScript.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-170">\* As above, the absolute directory paths will change over the lifetime of the VM, but not within a single execution of the CustomScript extension.</span></span>

### <a name="support"></a><span data-ttu-id="8e5c0-171">Podpora</span><span class="sxs-lookup"><span data-stu-id="8e5c0-171">Support</span></span>

<span data-ttu-id="8e5c0-172">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [MSDN Azure a Stack Overflow fórech] (https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="8e5c0-172">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums] (https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="8e5c0-173">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-173">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="8e5c0-174">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="8e5c0-174">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="8e5c0-175">Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="8e5c0-175">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
