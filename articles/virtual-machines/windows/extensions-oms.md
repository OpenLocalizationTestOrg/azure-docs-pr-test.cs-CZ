---
title: "Rozšíření virtuálního počítače OMS Azure pro Windows | Microsoft Docs"
description: "Nasaďte agenta OMS na systému Windows virtuálního počítače pomocí rozšíření virtuálního počítače."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: nepeters
ms.openlocfilehash: d933f488fdda0c1d37892be65f2712cf0eb5694e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="oms-virtual-machine-extension-for-windows"></a><span data-ttu-id="9c20e-103">Rozšíření virtuálního počítače OMS pro Windows</span><span class="sxs-lookup"><span data-stu-id="9c20e-103">OMS virtual machine extension for Windows</span></span>

<span data-ttu-id="9c20e-104">Operations Management Suite (OMS) poskytuje možnosti nápravy monitorování, výstrahy a výstrahy v cloudové a místní prostředky.</span><span class="sxs-lookup"><span data-stu-id="9c20e-104">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="9c20e-105">Rozšíření virtuálního počítače OMS Agent pro systém Windows je publikována a společnost Microsoft podporuje.</span><span class="sxs-lookup"><span data-stu-id="9c20e-105">The OMS Agent virtual machine extension for Windows is published and supported by Microsoft.</span></span> <span data-ttu-id="9c20e-106">Rozšíření nainstaluje agenta OMS na virtuálních počítačích Azure a zaregistruje virtuální počítače do existující pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="9c20e-106">The extension installs the OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="9c20e-107">Tento dokument podrobně popisuje podporované platformy, konfigurace a možnosti nasazení pro rozšíření virtuálního počítače OMS pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="9c20e-107">This document details the supported platforms, configurations, and deployment options for the OMS virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c20e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9c20e-108">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="9c20e-109">Operační systém</span><span class="sxs-lookup"><span data-stu-id="9c20e-109">Operating system</span></span>
<span data-ttu-id="9c20e-110">Rozšíření agenta OMS pro pro Windows Server 2008 R2, můžete spouštět Windows 2012, 2012 R2 a 2016 uvolní.</span><span class="sxs-lookup"><span data-stu-id="9c20e-110">The OMS Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="9c20e-111">Připojení k internetu</span><span class="sxs-lookup"><span data-stu-id="9c20e-111">Internet connectivity</span></span>
<span data-ttu-id="9c20e-112">OMS Agent rozšíření pro Windows vyžaduje, aby cílový virtuální počítač je připojený k Internetu.</span><span class="sxs-lookup"><span data-stu-id="9c20e-112">The OMS Agent extension for Windows requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="9c20e-113">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="9c20e-113">Extension schema</span></span>

<span data-ttu-id="9c20e-114">Následujícím kódu JSON znázorňuje schéma pro rozšíření agenta OMS.</span><span class="sxs-lookup"><span data-stu-id="9c20e-114">The following JSON shows the schema for the OMS Agent extension.</span></span> <span data-ttu-id="9c20e-115">Rozšíření vyžaduje pracovní prostor Id a klíč pracovního prostoru z je cílový pracovní prostor OMS, ty lze najít na portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="9c20e-115">The extension requires the workspace Id and workspace key from the target OMS workspace, these can be found in the OMS portal.</span></span> <span data-ttu-id="9c20e-116">Vzhledem k tomu, že klíč pracovního prostoru by měl být považován za citlivá data, by měly být uložené v chráněném nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9c20e-116">Because the workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="9c20e-117">Data Azure nastavení rozšíření chráněný virtuální počítač je zašifrovaná a dešifrovat jenom na cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9c20e-117">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span> <span data-ttu-id="9c20e-118">Všimněte si, že **workspaceId** a **workspaceKey** malých a velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="9c20e-118">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a><span data-ttu-id="9c20e-119">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="9c20e-119">Property values</span></span>

| <span data-ttu-id="9c20e-120">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9c20e-120">Name</span></span> | <span data-ttu-id="9c20e-121">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="9c20e-121">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="9c20e-122">apiVersion</span><span class="sxs-lookup"><span data-stu-id="9c20e-122">apiVersion</span></span> | <span data-ttu-id="9c20e-123">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="9c20e-123">2015-06-15</span></span> |
| <span data-ttu-id="9c20e-124">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="9c20e-124">publisher</span></span> | <span data-ttu-id="9c20e-125">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="9c20e-125">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="9c20e-126">type</span><span class="sxs-lookup"><span data-stu-id="9c20e-126">type</span></span> | <span data-ttu-id="9c20e-127">MicrosoftMonitoringAgent</span><span class="sxs-lookup"><span data-stu-id="9c20e-127">MicrosoftMonitoringAgent</span></span> |
| <span data-ttu-id="9c20e-128">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="9c20e-128">typeHandlerVersion</span></span> | <span data-ttu-id="9c20e-129">1.0</span><span class="sxs-lookup"><span data-stu-id="9c20e-129">1.0</span></span> |
| <span data-ttu-id="9c20e-130">ID pracovního prostoru (např.)</span><span class="sxs-lookup"><span data-stu-id="9c20e-130">workspaceId (e.g)</span></span> | <span data-ttu-id="9c20e-131">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="9c20e-131">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="9c20e-132">workspaceKey (např.)</span><span class="sxs-lookup"><span data-stu-id="9c20e-132">workspaceKey (e.g)</span></span> | <span data-ttu-id="9c20e-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="9c20e-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="9c20e-134">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="9c20e-134">Template deployment</span></span>

<span data-ttu-id="9c20e-135">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9c20e-135">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="9c20e-136">Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření agenta OMS při nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9c20e-136">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the OMS Agent extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="9c20e-137">Ukázka šablony, která obsahuje rozšíření virtuálního počítače agenta OMS naleznete na [Azure rychlý Start Galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="9c20e-137">A sample template that includes the OMS Agent VM extension can be found on the [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span></span> 

<span data-ttu-id="9c20e-138">JSON pro rozšíření virtuálního počítače můžete vnořit prostředek virtuálního počítače nebo umístěn na kořenový server WSUS nebo nejvyšší úrovně šablony JSON Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9c20e-138">The JSON for a virtual machine extension can be nested inside the virtual machine resource, or placed at the root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="9c20e-139">Umístění formátu JSON, ovlivňuje hodnota název prostředku a typem.</span><span class="sxs-lookup"><span data-stu-id="9c20e-139">The placement of the JSON affects the value of the resource name and type.</span></span> <span data-ttu-id="9c20e-140">Další informace najdete v tématu [nastavte název a typ pro podřízené prostředky](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="9c20e-140">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="9c20e-141">V následujícím příkladu se předpokládá, že je rozšíření OMS vnořit prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9c20e-141">The following example assumes the OMS extension is nested inside the virtual machine resource.</span></span> <span data-ttu-id="9c20e-142">Při vnoření rozšíření prostředků, JSON je umístěn v `"resources": []` objektu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9c20e-142">When nesting the extension resource, the JSON is placed in the `"resources": []` object of the virtual machine.</span></span>


```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

<span data-ttu-id="9c20e-143">Při vkládání rozšíření JSON v kořenovém adresáři šablony, názvu prostředku obsahuje odkaz na nadřazený virtuální počítač a typ odráží vnořené konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9c20e-143">When placing the extension JSON at the root of the template, the resource name includes a reference to the parent virtual machine, and the type reflects the nested configuration.</span></span> 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a><span data-ttu-id="9c20e-144">Nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9c20e-144">PowerShell deployment</span></span>

<span data-ttu-id="9c20e-145">`Set-AzureRmVMExtension` Příkaz lze použít k nasazení rozšíření virtuálního počítače agenta OMS na existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9c20e-145">The `Set-AzureRmVMExtension` command can be used to deploy the OMS Agent virtual machine extension to an existing virtual machine.</span></span> <span data-ttu-id="9c20e-146">Před spuštěním příkazu, veřejné a privátní konfigurace muset být uložena v tabulce hash prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9c20e-146">Before running the command, the public and private configurations need to be stored in a PowerShell hash table.</span></span> 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzureRmVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS ` 
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="9c20e-147">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="9c20e-147">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="9c20e-148">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="9c20e-148">Troubleshoot</span></span>

<span data-ttu-id="9c20e-149">Data o stavu nasazení rozšíření mohou být načteny z portálu Azure a pomocí modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9c20e-149">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="9c20e-150">Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9c20e-150">To see the deployment state of extensions for a given VM, run the following command using the Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="9c20e-151">Výstupu spuštění rozšíření se zaznamenává soubory, které jsou v následujícím adresáři:</span><span class="sxs-lookup"><span data-stu-id="9c20e-151">Extension execution output is logged to files found in the following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a><span data-ttu-id="9c20e-152">Podpora</span><span class="sxs-lookup"><span data-stu-id="9c20e-152">Support</span></span>

<span data-ttu-id="9c20e-153">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="9c20e-153">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="9c20e-154">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="9c20e-154">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="9c20e-155">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="9c20e-155">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="9c20e-156">Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="9c20e-156">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
