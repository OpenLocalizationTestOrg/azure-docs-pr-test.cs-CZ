---
title: "aaaAzure rozšíření virtuálního počítače sítě sledovacích procesů agenta pro Windows | Microsoft Docs"
description: "Nasaďte hello sítě sledovacích procesů agenta v systému Windows virtuálního počítače pomocí rozšíření virtuálního počítače."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 21298706e462ff32c4d314f9a1ad127074ddf481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a><span data-ttu-id="8ca61-103">Sítě rozšíření virtuálního počítače sledovacích procesů agenta pro Windows</span><span class="sxs-lookup"><span data-stu-id="8ca61-103">Network Watcher Agent virtual machine extension for Windows</span></span>

## <a name="overview"></a><span data-ttu-id="8ca61-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8ca61-104">Overview</span></span>

<span data-ttu-id="8ca61-105">[Azure sledovací proces sítě](https://review.docs.microsoft.com/en-us/azure/network-watcher/) je sítě výkonu monitorování, diagnostiku a analýzy služba, která umožňuje monitorování pro sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="8ca61-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="8ca61-106">Hello rozšíření sítě sledovacích procesů agenta virtuálního počítače není pro některé funkce hello sledovací proces sítě na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="8ca61-106">hello Network Watcher Agent virtual machine extension is a requirement for some of hello Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="8ca61-107">To zahrnuje Zachytávání síťových přenosů na vyžádání a další pokročilé funkce.</span><span class="sxs-lookup"><span data-stu-id="8ca61-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="8ca61-108">Tento dokument podrobnosti hello podporované platformy a možnosti nasazení pro hello rozšíření virtuálního počítače sítě sledovacích procesů agenta pro Windows.</span><span class="sxs-lookup"><span data-stu-id="8ca61-108">This document details hello supported platforms and deployment options for hello Network Watcher Agent virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ca61-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8ca61-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="8ca61-110">Operační systém</span><span class="sxs-lookup"><span data-stu-id="8ca61-110">Operating system</span></span>

<span data-ttu-id="8ca61-111">Hello rozšíření sítě sledovacích procesů agenta pro Windows můžete spustit na Windows Server 2008 R2, 2012, 2012 R2 a 2016 uvolní.</span><span class="sxs-lookup"><span data-stu-id="8ca61-111">hello Network Watcher Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span> <span data-ttu-id="8ca61-112">Všimněte si, že hello Nano Server není v současné době podporovaný.</span><span class="sxs-lookup"><span data-stu-id="8ca61-112">Note that hello Nano Server is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="8ca61-113">Připojení k internetu</span><span class="sxs-lookup"><span data-stu-id="8ca61-113">Internet connectivity</span></span>

<span data-ttu-id="8ca61-114">Některé hello funkce sítě sledovacích procesů agenta vyžaduje, aby hello cílový virtuální počítač připojené toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="8ca61-114">Some of hello Network Watcher Agent functionality requires that hello target virtual machine be connected toohello Internet.</span></span> <span data-ttu-id="8ca61-115">Bez hello možnost tooestablish odchozí připojení některé funkce sítě sledovacích procesů agenta hello nebude fungovat správně nebo nedostupná.</span><span class="sxs-lookup"><span data-stu-id="8ca61-115">Without hello ability tooestablish outgoing connections some of hello Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="8ca61-116">Další podrobnosti najdete v tématu hello [sledovací proces sítě dokumentaci](../../network-watcher/network-watcher-monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8ca61-116">For more details, please see hello [Network Watcher documentation](../../network-watcher/network-watcher-monitoring-overview.md).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="8ca61-117">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="8ca61-117">Extension schema</span></span>

<span data-ttu-id="8ca61-118">Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření sítě sledovacích procesů agenta.</span><span class="sxs-lookup"><span data-stu-id="8ca61-118">hello following JSON shows hello schema for hello Network Watcher Agent extension.</span></span> <span data-ttu-id="8ca61-119">rozšíření Hello ani jeden z nich vyžaduje ani podporuje nastavení uživatelem zadané v tuto chvíli a spoléhá na jeho výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8ca61-119">hello extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a><span data-ttu-id="8ca61-120">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="8ca61-120">Property values</span></span>

| <span data-ttu-id="8ca61-121">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8ca61-121">Name</span></span> | <span data-ttu-id="8ca61-122">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="8ca61-122">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="8ca61-123">apiVersion</span><span class="sxs-lookup"><span data-stu-id="8ca61-123">apiVersion</span></span> | <span data-ttu-id="8ca61-124">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="8ca61-124">2015-06-15</span></span> |
| <span data-ttu-id="8ca61-125">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="8ca61-125">publisher</span></span> | <span data-ttu-id="8ca61-126">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="8ca61-126">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="8ca61-127">type</span><span class="sxs-lookup"><span data-stu-id="8ca61-127">type</span></span> | <span data-ttu-id="8ca61-128">NetworkWatcherAgentWindows</span><span class="sxs-lookup"><span data-stu-id="8ca61-128">NetworkWatcherAgentWindows</span></span> |
| <span data-ttu-id="8ca61-129">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="8ca61-129">typeHandlerVersion</span></span> | <span data-ttu-id="8ca61-130">1.4</span><span class="sxs-lookup"><span data-stu-id="8ca61-130">1.4</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="8ca61-131">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="8ca61-131">Template deployment</span></span>

<span data-ttu-id="8ca61-132">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8ca61-132">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="8ca61-133">popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager sítě sledovacích procesů agenta rozšíření během nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8ca61-133">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="8ca61-134">Nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ca61-134">PowerShell deployment</span></span>

<span data-ttu-id="8ca61-135">Hello `Set-AzureRmVMExtension` příkaz lze použít toodeploy hello sítě sledovacích procesů agenta virtuálního počítače rozšíření tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8ca61-135">hello `Set-AzureRmVMExtension` command can be used toodeploy hello Network Watcher Agent virtual machine extension tooan existing virtual machine.</span></span>

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="8ca61-136">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="8ca61-136">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="8ca61-137">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="8ca61-137">Troubleshooting</span></span>

<span data-ttu-id="8ca61-138">Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí modulu Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="8ca61-138">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="8ca61-139">Stav nasazení hello toosee rozšíření pro daný virtuální počítač, spusťte hello následující pomocí příkazu hello modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8ca61-139">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

<span data-ttu-id="8ca61-140">Rozšíření spuštění výstup je zaznamenané toofiles najít v hello následující adresář:</span><span class="sxs-lookup"><span data-stu-id="8ca61-140">Extension execution output is logged toofiles found in hello following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a><span data-ttu-id="8ca61-141">Podpora</span><span class="sxs-lookup"><span data-stu-id="8ca61-141">Support</span></span>

<span data-ttu-id="8ca61-142">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, můžete naleznete v dokumentaci toohello Příručka uživatele sledovací proces sítě nebo se obraťte hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="8ca61-142">If you need more help at any point in this article, you can refer toohello Network Watcher User Guide documentation or contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="8ca61-143">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="8ca61-143">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="8ca61-144">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="8ca61-144">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="8ca61-145">Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="8ca61-145">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
