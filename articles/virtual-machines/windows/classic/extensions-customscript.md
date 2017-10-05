---
title: "Rozšíření vlastních skriptů na virtuální počítač s Windows | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače Azure pomocí rozšíření vlastních skriptů ke spouštění skriptů prostředí PowerShell na vzdálené virtuální počítač s Windows"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: nepeters
ms.openlocfilehash: 986ab1025dc188cd7fa1cf8b131a9d4b859be8f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="custom-script-extension-for-windows-using-the-classic-deployment-model"></a><span data-ttu-id="78011-103">Vlastní skript rozšíření pro systém Windows s použitím modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="78011-103">Custom Script Extension for Windows using the classic deployment model</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="78011-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="78011-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="78011-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="78011-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="78011-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="78011-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="78011-107">Zjistěte, jak [provést tento postup pomocí modelu Resource Manageru](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="78011-107">Learn how to [perform these steps using the Resource Manager model](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="78011-108">Rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="78011-108">The Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="78011-109">Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy.</span><span class="sxs-lookup"><span data-stu-id="78011-109">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="78011-110">Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat na portál Azure na dobu běhu rozšíření.</span><span class="sxs-lookup"><span data-stu-id="78011-110">Scripts can be downloaded from Azure storage or GitHub, or provided to the Azure portal at extension run time.</span></span> <span data-ttu-id="78011-111">Rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo REST API pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="78011-111">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="78011-112">Tento dokument podrobně popisuje postup používání rozšíření vlastních skriptů pomocí modulu Azure PowerShell, šablon Azure Resource Manageru a podrobnosti o řešení potíží s kroky v systémech Windows.</span><span class="sxs-lookup"><span data-stu-id="78011-112">This document details how to use the Custom Script Extension using the Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78011-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="78011-113">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="78011-114">Operační systém</span><span class="sxs-lookup"><span data-stu-id="78011-114">Operating System</span></span>

<span data-ttu-id="78011-115">Rozšíření vlastních skriptů pro pro Windows Server 2008 R2, můžete spouštět Windows 2012, 2012 R2 a 2016 uvolní.</span><span class="sxs-lookup"><span data-stu-id="78011-115">The Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="78011-116">Umístění skriptu</span><span class="sxs-lookup"><span data-stu-id="78011-116">Script Location</span></span>

<span data-ttu-id="78011-117">Skript musí být uložená v úložišti Azure nebo jakéhokoli jiného umístění, které jsou přístupné prostřednictvím platnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="78011-117">The script needs to be stored in Azure storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="78011-118">Připojení k Internetu</span><span class="sxs-lookup"><span data-stu-id="78011-118">Internet Connectivity</span></span>

<span data-ttu-id="78011-119">Vlastní skript rozšíření pro Windows vyžaduje, aby cílový virtuální počítač je připojený k Internetu.</span><span class="sxs-lookup"><span data-stu-id="78011-119">The Custom Script Extension for Windows requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="78011-120">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="78011-120">Extension schema</span></span>

<span data-ttu-id="78011-121">Následujícím kódu JSON znázorňuje schéma pro rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="78011-121">The following JSON shows the schema for the Custom Script Extension.</span></span> <span data-ttu-id="78011-122">Rozšíření vyžaduje umístění skriptu (Azure Storage nebo jiného umístění s platnou adresu URL) a příkaz ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="78011-122">The extension requires a script location (Azure Storage or other location with valid URL), and a command to execute.</span></span> <span data-ttu-id="78011-123">Pokud používáte Azure Storage jako zdroj skriptu, je třeba klíč účtu úložiště Azure účet a název.</span><span class="sxs-lookup"><span data-stu-id="78011-123">If using Azure Storage as the script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="78011-124">Tyto položky by měl být považován za citlivá data a zadaný v konfiguraci chráněných nastavení rozšíření.</span><span class="sxs-lookup"><span data-stu-id="78011-124">These items should be treated as sensitive data and specified in the extensions protected setting configuration.</span></span> <span data-ttu-id="78011-125">Data Azure nastavení rozšíření chráněný virtuální počítač je zašifrovaná a dešifrovat jenom na cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="78011-125">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span>

```json
{
    "name": "config-app",
    "type": "Microsoft.ClassicCompute/virtualMachines/extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-01",
    "properties": {
        "publisher": "Microsoft.Compute",
        "extension": "CustomScriptExtension",
        "version": "1.8",
        "parameters": {
            "public": {
                "fileUris": "[myScriptLocation]"
            },
            "private": {
                "commandToExecute": "[myExecutionString]"
            }
        }
    }
}
```

### <a name="property-values"></a><span data-ttu-id="78011-126">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="78011-126">Property values</span></span>

| <span data-ttu-id="78011-127">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="78011-127">Name</span></span> | <span data-ttu-id="78011-128">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="78011-128">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="78011-129">apiVersion</span><span class="sxs-lookup"><span data-stu-id="78011-129">apiVersion</span></span> | <span data-ttu-id="78011-130">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="78011-130">2015-06-15</span></span> |
| <span data-ttu-id="78011-131">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="78011-131">publisher</span></span> | <span data-ttu-id="78011-132">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="78011-132">Microsoft.Compute</span></span> |
| <span data-ttu-id="78011-133">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="78011-133">extension</span></span> | <span data-ttu-id="78011-134">CustomScriptExtension</span><span class="sxs-lookup"><span data-stu-id="78011-134">CustomScriptExtension</span></span> |
| <span data-ttu-id="78011-135">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="78011-135">typeHandlerVersion</span></span> | <span data-ttu-id="78011-136">1.8</span><span class="sxs-lookup"><span data-stu-id="78011-136">1.8</span></span> |
| <span data-ttu-id="78011-137">fileUris (např.)</span><span class="sxs-lookup"><span data-stu-id="78011-137">fileUris (e.g)</span></span> | <span data-ttu-id="78011-138">https://RAW.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="78011-138">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="78011-139">commandToExecute (např.)</span><span class="sxs-lookup"><span data-stu-id="78011-139">commandToExecute (e.g)</span></span> | <span data-ttu-id="78011-140">prostředí PowerShell - ExecutionPolicy Unrestricted - souboru konfigurace app.ps1 Hudba</span><span class="sxs-lookup"><span data-stu-id="78011-140">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="78011-141">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="78011-141">Template deployment</span></span>

<span data-ttu-id="78011-142">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="78011-142">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="78011-143">Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření vlastních skriptů při nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="78011-143">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="78011-144">Ukázka šablony, která obsahuje rozšíření vlastních skriptů je zde uveden, [Githubu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="78011-144">A sample template that includes the Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="78011-145">Nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="78011-145">PowerShell deployment</span></span>

<span data-ttu-id="78011-146">`Set-AzureVMCustomScriptExtension` Příkaz lze použít k přidání rozšíření vlastních skriptů do existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="78011-146">The `Set-AzureVMCustomScriptExtension` command can be used to add the Custom Script extension to an existing virtual machine.</span></span> <span data-ttu-id="78011-147">Další informace najdete v tématu [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="78011-147">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="78011-148">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="78011-148">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="78011-149">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="78011-149">Troubleshoot</span></span>

<span data-ttu-id="78011-150">Data o stavu nasazení rozšíření mohou být načteny z portálu Azure a pomocí modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="78011-150">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="78011-151">Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="78011-151">To see the deployment state of extensions for a given VM, run the following command.</span></span>

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="78011-152">Rozšíření spuštění výstupu nám protokolovány soubory, které jsou v následujícím adresáři na cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="78011-152">Extension execution output us logged to files found in the following directory on the target virtual machine.</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="78011-153">Samotný skript se stáhne do následujícího adresáře na cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="78011-153">The script itself is downloaded into the following directory on the target virtual machine.</span></span>

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a><span data-ttu-id="78011-154">Podpora</span><span class="sxs-lookup"><span data-stu-id="78011-154">Support</span></span>

<span data-ttu-id="78011-155">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="78011-155">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="78011-156">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="78011-156">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="78011-157">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="78011-157">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="78011-158">Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="78011-158">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>