---
title: "aaaOMS rozšíření virtuálního počítače Azure pro Windows | Microsoft Docs"
description: "Nasaďte agenta OMS hello v systému Windows virtuálního počítače pomocí rozšíření virtuálního počítače."
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
ms.openlocfilehash: 3000f66c0acdec1d1fad2125b8c6b72a92b1ec92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-windows"></a><span data-ttu-id="9e093-103">Rozšíření virtuálního počítače OMS pro Windows</span><span class="sxs-lookup"><span data-stu-id="9e093-103">OMS virtual machine extension for Windows</span></span>

<span data-ttu-id="9e093-104">Operations Management Suite (OMS) poskytuje možnosti nápravy monitorování, výstrahy a výstrahy v cloudové a místní prostředky.</span><span class="sxs-lookup"><span data-stu-id="9e093-104">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="9e093-105">rozšíření virtuálního počítače OMS Agent pro Windows Hello je publikována a společnost Microsoft podporuje.</span><span class="sxs-lookup"><span data-stu-id="9e093-105">hello OMS Agent virtual machine extension for Windows is published and supported by Microsoft.</span></span> <span data-ttu-id="9e093-106">rozšíření Hello nainstaluje agenta OMS hello na virtuálních počítačích Azure a zaregistruje virtuální počítače do existující pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="9e093-106">hello extension installs hello OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="9e093-107">Tento dokument podrobnosti hello podporované platformy, konfigurace a možnosti nasazení pro hello OMS rozšíření virtuálního počítače pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="9e093-107">This document details hello supported platforms, configurations, and deployment options for hello OMS virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e093-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9e093-108">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="9e093-109">Operační systém</span><span class="sxs-lookup"><span data-stu-id="9e093-109">Operating system</span></span>
<span data-ttu-id="9e093-110">Hello agenta OMS rozšíření pro Windows můžete spustit na Windows Server 2008 R2, 2012, 2012 R2 a 2016 uvolní.</span><span class="sxs-lookup"><span data-stu-id="9e093-110">hello OMS Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="9e093-111">Připojení k internetu</span><span class="sxs-lookup"><span data-stu-id="9e093-111">Internet connectivity</span></span>
<span data-ttu-id="9e093-112">Hello agenta OMS rozšíření pro Windows vyžaduje, aby hello cílový virtuální počítač je připojený toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="9e093-112">hello OMS Agent extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="9e093-113">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="9e093-113">Extension schema</span></span>

<span data-ttu-id="9e093-114">Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření agenta OMS.</span><span class="sxs-lookup"><span data-stu-id="9e093-114">hello following JSON shows hello schema for hello OMS Agent extension.</span></span> <span data-ttu-id="9e093-115">rozšíření Hello vyžaduje klíč Id a pracovního prostoru pracovní prostor hello z hello cílového OMS pracovního prostoru, ty lze najít na portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="9e093-115">hello extension requires hello workspace Id and workspace key from hello target OMS workspace, these can be found in hello OMS portal.</span></span> <span data-ttu-id="9e093-116">Protože hello klíč pracovního prostoru by měl být považován za citlivá data, by měly být uložené v chráněném nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9e093-116">Because hello workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="9e093-117">Azure data virtuálního počítače chráněný rozšíření nastavení je zašifrovaná a dešifrovat jenom na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9e093-117">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span> <span data-ttu-id="9e093-118">Všimněte si, že **workspaceId** a **workspaceKey** malých a velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="9e093-118">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

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
### <a name="property-values"></a><span data-ttu-id="9e093-119">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="9e093-119">Property values</span></span>

| <span data-ttu-id="9e093-120">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9e093-120">Name</span></span> | <span data-ttu-id="9e093-121">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="9e093-121">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="9e093-122">apiVersion</span><span class="sxs-lookup"><span data-stu-id="9e093-122">apiVersion</span></span> | <span data-ttu-id="9e093-123">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="9e093-123">2015-06-15</span></span> |
| <span data-ttu-id="9e093-124">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="9e093-124">publisher</span></span> | <span data-ttu-id="9e093-125">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="9e093-125">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="9e093-126">type</span><span class="sxs-lookup"><span data-stu-id="9e093-126">type</span></span> | <span data-ttu-id="9e093-127">MicrosoftMonitoringAgent</span><span class="sxs-lookup"><span data-stu-id="9e093-127">MicrosoftMonitoringAgent</span></span> |
| <span data-ttu-id="9e093-128">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="9e093-128">typeHandlerVersion</span></span> | <span data-ttu-id="9e093-129">1.0</span><span class="sxs-lookup"><span data-stu-id="9e093-129">1.0</span></span> |
| <span data-ttu-id="9e093-130">ID pracovního prostoru (např.)</span><span class="sxs-lookup"><span data-stu-id="9e093-130">workspaceId (e.g)</span></span> | <span data-ttu-id="9e093-131">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="9e093-131">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="9e093-132">workspaceKey (např.)</span><span class="sxs-lookup"><span data-stu-id="9e093-132">workspaceKey (e.g)</span></span> | <span data-ttu-id="9e093-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="9e093-133">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="9e093-134">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="9e093-134">Template deployment</span></span>

<span data-ttu-id="9e093-135">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9e093-135">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="9e093-136">popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager rozšíření agenta OMS při nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9e093-136">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello OMS Agent extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="9e093-137">Ukázka šablony, která obsahuje rozšíření virtuálního počítače agenta OMS hello naleznete na hello [Azure rychlý Start Galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="9e093-137">A sample template that includes hello OMS Agent VM extension can be found on hello [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm).</span></span> 

<span data-ttu-id="9e093-138">Hello JSON pro rozšíření virtuálního počítače lze vnořit hello prostředek virtuálního počítače nebo umístěny na nejvyšší úrovni šablony Resource Manageru JSON nebo hello kořenové.</span><span class="sxs-lookup"><span data-stu-id="9e093-138">hello JSON for a virtual machine extension can be nested inside hello virtual machine resource, or placed at hello root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="9e093-139">umístění Hello hello JSON ovlivní hodnotu hello hello název prostředku a typem.</span><span class="sxs-lookup"><span data-stu-id="9e093-139">hello placement of hello JSON affects hello value of hello resource name and type.</span></span> <span data-ttu-id="9e093-140">Další informace najdete v tématu [nastavte název a typ pro podřízené prostředky](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="9e093-140">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="9e093-141">Hello následující příklad předpokládá, že rozšíření OMS hello je vnořit hello prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9e093-141">hello following example assumes hello OMS extension is nested inside hello virtual machine resource.</span></span> <span data-ttu-id="9e093-142">Při vnoření hello rozšíření prostředků, hello JSON je umístěn v hello `"resources": []` objekt hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9e093-142">When nesting hello extension resource, hello JSON is placed in hello `"resources": []` object of hello virtual machine.</span></span>


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

<span data-ttu-id="9e093-143">Při vkládání hello rozšíření JSON v kořenu hello hello šablony, název prostředku hello zahrnuje odkaz toohello nadřazený virtuální počítač a typ hello odráží hello vnořené konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9e093-143">When placing hello extension JSON at hello root of hello template, hello resource name includes a reference toohello parent virtual machine, and hello type reflects hello nested configuration.</span></span> 

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

## <a name="powershell-deployment"></a><span data-ttu-id="9e093-144">Nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9e093-144">PowerShell deployment</span></span>

<span data-ttu-id="9e093-145">Hello `Set-AzureRmVMExtension` příkaz lze použít toodeploy hello OMS agenta virtuálního počítače rozšíření tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9e093-145">hello `Set-AzureRmVMExtension` command can be used toodeploy hello OMS Agent virtual machine extension tooan existing virtual machine.</span></span> <span data-ttu-id="9e093-146">Před spuštěním příkazu hello, třeba hello veřejné a privátní konfigurace toobe uložené v prostředí PowerShell zatřiďovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="9e093-146">Before running hello command, hello public and private configurations need toobe stored in a PowerShell hash table.</span></span> 

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

## <a name="troubleshoot-and-support"></a><span data-ttu-id="9e093-147">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="9e093-147">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="9e093-148">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="9e093-148">Troubleshoot</span></span>

<span data-ttu-id="9e093-149">Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí modulu Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="9e093-149">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="9e093-150">Stav nasazení hello toosee rozšíření pro daný virtuální počítač, spusťte hello následující pomocí příkazu hello modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e093-150">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="9e093-151">Rozšíření spuštění výstup je zaznamenané toofiles najít v hello následující adresář:</span><span class="sxs-lookup"><span data-stu-id="9e093-151">Extension execution output is logged toofiles found in hello following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a><span data-ttu-id="9e093-152">Podpora</span><span class="sxs-lookup"><span data-stu-id="9e093-152">Support</span></span>

<span data-ttu-id="9e093-153">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="9e093-153">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="9e093-154">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="9e093-154">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="9e093-155">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="9e093-155">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="9e093-156">Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="9e093-156">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
