---
title: "aaaCustom rozšíření skriptů na virtuální počítač s Windows | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače Azure pomocí skriptů prostředí PowerShell toorun rozšíření vlastních skriptů hello na vzdálené virtuální počítač s Windows"
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
ms.openlocfilehash: cf7bb895dd211f07fd010dc03b68cd77df1127b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows-using-hello-classic-deployment-model"></a><span data-ttu-id="d4b8f-103">Vlastní skript rozšíření pro systém Windows s použitím modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="d4b8f-103">Custom Script Extension for Windows using hello classic deployment model</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="d4b8f-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d4b8f-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d4b8f-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="d4b8f-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="d4b8f-107">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d4b8f-107">Learn how too[perform these steps using hello Resource Manager model](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="d4b8f-108">Hello rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-108">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="d4b8f-109">Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-109">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="d4b8f-110">Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat toohello portál Azure na dobu běhu rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-110">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span> <span data-ttu-id="d4b8f-111">Hello rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí hello rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo hello REST API pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-111">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="d4b8f-112">Tento dokument podrobně popisuje, jak pomocí rozšíření vlastních skriptů hello toouse hello modul Azure PowerShell, šablon Azure Resource Manageru a podrobnosti o řešení potíží s kroky v systémech Windows.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-112">This document details how toouse hello Custom Script Extension using hello Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4b8f-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d4b8f-113">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="d4b8f-114">Operační systém</span><span class="sxs-lookup"><span data-stu-id="d4b8f-114">Operating System</span></span>

<span data-ttu-id="d4b8f-115">Hello rozšíření vlastních skriptů pro pro Windows Server 2008 R2, můžete spouštět Windows 2012, 2012 R2 a 2016 uvolní.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-115">hello Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="d4b8f-116">Umístění skriptu</span><span class="sxs-lookup"><span data-stu-id="d4b8f-116">Script Location</span></span>

<span data-ttu-id="d4b8f-117">skript Hello musí toobe uložení do úložiště Azure, nebo jakéhokoli jiného umístění, které jsou přístupné prostřednictvím platnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-117">hello script needs toobe stored in Azure storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="d4b8f-118">Připojení k Internetu</span><span class="sxs-lookup"><span data-stu-id="d4b8f-118">Internet Connectivity</span></span>

<span data-ttu-id="d4b8f-119">Hello vlastní skript rozšíření pro Windows vyžaduje, aby hello cílový virtuální počítač je připojený toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-119">hello Custom Script Extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="d4b8f-120">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="d4b8f-120">Extension schema</span></span>

<span data-ttu-id="d4b8f-121">Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-121">hello following JSON shows hello schema for hello Custom Script Extension.</span></span> <span data-ttu-id="d4b8f-122">rozšíření Hello vyžaduje umístění skriptu (Azure Storage nebo jiného umístění s platnou adresu URL) a příkaz tooexecute.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-122">hello extension requires a script location (Azure Storage or other location with valid URL), and a command tooexecute.</span></span> <span data-ttu-id="d4b8f-123">Pokud používáte Azure Storage jako zdroj skriptu hello, je třeba klíč účtu úložiště Azure účet a název.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-123">If using Azure Storage as hello script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="d4b8f-124">Tyto položky by měl být považován za citlivá data a zadaný v konfiguraci chráněných nastavení rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-124">These items should be treated as sensitive data and specified in hello extensions protected setting configuration.</span></span> <span data-ttu-id="d4b8f-125">Azure data virtuálního počítače chráněný rozšíření nastavení je zašifrovaná a dešifrovat jenom na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-125">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="d4b8f-126">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="d4b8f-126">Property values</span></span>

| <span data-ttu-id="d4b8f-127">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="d4b8f-127">Name</span></span> | <span data-ttu-id="d4b8f-128">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="d4b8f-128">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="d4b8f-129">apiVersion</span><span class="sxs-lookup"><span data-stu-id="d4b8f-129">apiVersion</span></span> | <span data-ttu-id="d4b8f-130">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="d4b8f-130">2015-06-15</span></span> |
| <span data-ttu-id="d4b8f-131">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="d4b8f-131">publisher</span></span> | <span data-ttu-id="d4b8f-132">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="d4b8f-132">Microsoft.Compute</span></span> |
| <span data-ttu-id="d4b8f-133">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="d4b8f-133">extension</span></span> | <span data-ttu-id="d4b8f-134">CustomScriptExtension</span><span class="sxs-lookup"><span data-stu-id="d4b8f-134">CustomScriptExtension</span></span> |
| <span data-ttu-id="d4b8f-135">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="d4b8f-135">typeHandlerVersion</span></span> | <span data-ttu-id="d4b8f-136">1.8</span><span class="sxs-lookup"><span data-stu-id="d4b8f-136">1.8</span></span> |
| <span data-ttu-id="d4b8f-137">fileUris (např.)</span><span class="sxs-lookup"><span data-stu-id="d4b8f-137">fileUris (e.g)</span></span> | <span data-ttu-id="d4b8f-138">https://RAW.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="d4b8f-138">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="d4b8f-139">commandToExecute (např.)</span><span class="sxs-lookup"><span data-stu-id="d4b8f-139">commandToExecute (e.g)</span></span> | <span data-ttu-id="d4b8f-140">prostředí PowerShell - ExecutionPolicy Unrestricted - souboru konfigurace app.ps1 Hudba</span><span class="sxs-lookup"><span data-stu-id="d4b8f-140">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="d4b8f-141">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="d4b8f-141">Template deployment</span></span>

<span data-ttu-id="d4b8f-142">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-142">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="d4b8f-143">popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager rozšíření vlastních skriptů při nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-143">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="d4b8f-144">Ukázka šablony, která obsahuje hello rozšíření vlastních skriptů je zde uveden, [Githubu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="d4b8f-144">A sample template that includes hello Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="d4b8f-145">Nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4b8f-145">PowerShell deployment</span></span>

<span data-ttu-id="d4b8f-146">Hello `Set-AzureVMCustomScriptExtension` příkaz lze použít tooadd hello vlastní skript rozšíření tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-146">hello `Set-AzureVMCustomScriptExtension` command can be used tooadd hello Custom Script extension tooan existing virtual machine.</span></span> <span data-ttu-id="d4b8f-147">Další informace najdete v tématu [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="d4b8f-147">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="d4b8f-148">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="d4b8f-148">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="d4b8f-149">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d4b8f-149">Troubleshoot</span></span>

<span data-ttu-id="d4b8f-150">Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí modulu Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-150">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="d4b8f-151">Stav nasazení toosee hello rozšíření pro daný virtuální počítač, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-151">toosee hello deployment state of extensions for a given VM, run hello following command.</span></span>

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="d4b8f-152">Rozšíření spuštění výstupu nám zaznamenané toofiles najít v hello následující adresáře na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-152">Extension execution output us logged toofiles found in hello following directory on hello target virtual machine.</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="d4b8f-153">samotný skript Hello se stáhne do hello následující adresáře na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-153">hello script itself is downloaded into hello following directory on hello target virtual machine.</span></span>

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a><span data-ttu-id="d4b8f-154">Podpora</span><span class="sxs-lookup"><span data-stu-id="d4b8f-154">Support</span></span>

<span data-ttu-id="d4b8f-155">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="d4b8f-155">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="d4b8f-156">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-156">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="d4b8f-157">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="d4b8f-157">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="d4b8f-158">Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="d4b8f-158">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
