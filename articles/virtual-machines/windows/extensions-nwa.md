---
title: "Rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows | Microsoft Docs"
description: "Nasaďte agenta sledovací proces sítě v systému Windows virtuálního počítače pomocí rozšíření virtuálního počítače."
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
ms.openlocfilehash: b8d6a998bc86337b286a3434f44f762cca9b7e68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a><span data-ttu-id="4e0ca-103">Sítě rozšíření virtuálního počítače sledovacích procesů agenta pro Windows</span><span class="sxs-lookup"><span data-stu-id="4e0ca-103">Network Watcher Agent virtual machine extension for Windows</span></span>

## <a name="overview"></a><span data-ttu-id="4e0ca-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="4e0ca-104">Overview</span></span>

<span data-ttu-id="4e0ca-105">[Azure sledovací proces sítě](https://review.docs.microsoft.com/en-us/azure/network-watcher/) je sítě výkonu monitorování, diagnostiku a analýzy služba, která umožňuje monitorování pro sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="4e0ca-106">Rozšíření sítě sledovacích procesů agenta virtuálního počítače není pro některé funkce sledovací proces sítě na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-106">The Network Watcher Agent virtual machine extension is a requirement for some of the Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="4e0ca-107">To zahrnuje Zachytávání síťových přenosů na vyžádání a další pokročilé funkce.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="4e0ca-108">Tento dokument podrobně popisuje podporované platformy a možnosti nasazení pro rozšíření sítě sledovacích procesů agenta virtuálního počítače pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-108">This document details the supported platforms and deployment options for the Network Watcher Agent virtual machine extension for Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e0ca-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4e0ca-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="4e0ca-110">Operační systém</span><span class="sxs-lookup"><span data-stu-id="4e0ca-110">Operating system</span></span>

<span data-ttu-id="4e0ca-111">Rozšíření sítě sledovacích procesů agenta pro Windows můžete spustit na Windows Server 2008 R2, 2012, 2012 R2 a 2016 uvolní.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-111">The Network Watcher Agent extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span> <span data-ttu-id="4e0ca-112">Všimněte si, že není v současné době podporovaný Nano Server.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-112">Note that the Nano Server is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="4e0ca-113">Připojení k internetu</span><span class="sxs-lookup"><span data-stu-id="4e0ca-113">Internet connectivity</span></span>

<span data-ttu-id="4e0ca-114">Některé funkce sítě sledovacích procesů agenta vyžaduje, aby cílový virtuální počítač připojen k Internetu.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-114">Some of the Network Watcher Agent functionality requires that the target virtual machine be connected to the Internet.</span></span> <span data-ttu-id="4e0ca-115">Bez možnosti navázat odchozí připojení některé funkce sítě sledovacích procesů agenta nebude fungovat správně nebo nedostupná.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-115">Without the ability to establish outgoing connections some of the Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="4e0ca-116">Další podrobnosti najdete v tématu [sledovací proces sítě dokumentaci](../../network-watcher/network-watcher-monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4e0ca-116">For more details, please see the [Network Watcher documentation](../../network-watcher/network-watcher-monitoring-overview.md).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="4e0ca-117">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="4e0ca-117">Extension schema</span></span>

<span data-ttu-id="4e0ca-118">Následujícím kódu JSON znázorňuje schéma pro rozšíření sítě sledovacích procesů agenta.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-118">The following JSON shows the schema for the Network Watcher Agent extension.</span></span> <span data-ttu-id="4e0ca-119">Rozšíření ani jeden z nich vyžaduje ani podporuje nastavení uživatelem zadané v tuto chvíli a spoléhá na jeho výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-119">The extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="4e0ca-120">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="4e0ca-120">Property values</span></span>

| <span data-ttu-id="4e0ca-121">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="4e0ca-121">Name</span></span> | <span data-ttu-id="4e0ca-122">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="4e0ca-122">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="4e0ca-123">apiVersion</span><span class="sxs-lookup"><span data-stu-id="4e0ca-123">apiVersion</span></span> | <span data-ttu-id="4e0ca-124">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="4e0ca-124">2015-06-15</span></span> |
| <span data-ttu-id="4e0ca-125">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="4e0ca-125">publisher</span></span> | <span data-ttu-id="4e0ca-126">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="4e0ca-126">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="4e0ca-127">type</span><span class="sxs-lookup"><span data-stu-id="4e0ca-127">type</span></span> | <span data-ttu-id="4e0ca-128">NetworkWatcherAgentWindows</span><span class="sxs-lookup"><span data-stu-id="4e0ca-128">NetworkWatcherAgentWindows</span></span> |
| <span data-ttu-id="4e0ca-129">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="4e0ca-129">typeHandlerVersion</span></span> | <span data-ttu-id="4e0ca-130">1.4</span><span class="sxs-lookup"><span data-stu-id="4e0ca-130">1.4</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="4e0ca-131">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="4e0ca-131">Template deployment</span></span>

<span data-ttu-id="4e0ca-132">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-132">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="4e0ca-133">Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření sítě sledovacích procesů agenta při nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-133">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="4e0ca-134">Nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e0ca-134">PowerShell deployment</span></span>

<span data-ttu-id="4e0ca-135">`Set-AzureRmVMExtension` Příkaz lze použít k nasazení rozšíření sítě sledovacích procesů agenta virtuálního počítače do existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-135">The `Set-AzureRmVMExtension` command can be used to deploy the Network Watcher Agent virtual machine extension to an existing virtual machine.</span></span>

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="4e0ca-136">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="4e0ca-136">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="4e0ca-137">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4e0ca-137">Troubleshooting</span></span>

<span data-ttu-id="4e0ca-138">Data o stavu nasazení rozšíření mohou být načteny z portálu Azure a pomocí modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-138">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="4e0ca-139">Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-139">To see the deployment state of extensions for a given VM, run the following command using the Azure PowerShell module.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

<span data-ttu-id="4e0ca-140">Výstupu spuštění rozšíření se zaznamenává soubory, které jsou v následujícím adresáři:</span><span class="sxs-lookup"><span data-stu-id="4e0ca-140">Extension execution output is logged to files found in the following directory:</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a><span data-ttu-id="4e0ca-141">Podpora</span><span class="sxs-lookup"><span data-stu-id="4e0ca-141">Support</span></span>

<span data-ttu-id="4e0ca-142">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, můžete v uživatelské příručce sledovací proces sítě dokumentaci nebo se obraťte na Azure odborníky [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="4e0ca-142">If you need more help at any point in this article, you can refer to the Network Watcher User Guide documentation or contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="4e0ca-143">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-143">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="4e0ca-144">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="4e0ca-144">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="4e0ca-145">Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="4e0ca-145">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
