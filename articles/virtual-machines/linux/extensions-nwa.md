---
title: "Rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Linux | Microsoft Docs"
description: "Nasaďte agenta sledovací proces sítě na virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače."
services: virtual-machines-linux
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: eaadd531b9e05a54446e61f98584ae9d75470a5f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a><span data-ttu-id="053ee-103">Sítě rozšíření virtuálního počítače sledovacích procesů agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="053ee-103">Network Watcher Agent virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="053ee-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="053ee-104">Overview</span></span>

<span data-ttu-id="053ee-105">[Azure sledovací proces sítě](https://review.docs.microsoft.com/en-us/azure/network-watcher/) je sítě výkonu monitorování, diagnostiku a analýzy služba, která umožňuje monitorování pro sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="053ee-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="053ee-106">Rozšíření sítě sledovacích procesů agenta virtuálního počítače není pro některé funkce sledovací proces sítě na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="053ee-106">The Network Watcher Agent virtual machine extension is a requirement for some of the Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="053ee-107">To zahrnuje Zachytávání síťových přenosů na vyžádání a další pokročilé funkce.</span><span class="sxs-lookup"><span data-stu-id="053ee-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="053ee-108">Tento dokument podrobně popisuje možnosti nasazení pro rozšíření sítě sledovacích procesů agenta virtuálního počítače pro Linux a podporované platformy.</span><span class="sxs-lookup"><span data-stu-id="053ee-108">This document details the supported platforms and deployment options for the Network Watcher Agent virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="053ee-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="053ee-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="053ee-110">Operační systém</span><span class="sxs-lookup"><span data-stu-id="053ee-110">Operating system</span></span>

<span data-ttu-id="053ee-111">Rozšíření agenta sledovací proces sítě může být spuštěn proti tyto Linuxových distribucích:</span><span class="sxs-lookup"><span data-stu-id="053ee-111">The Network Watcher Agent extension can be run against these Linux distributions:</span></span>

| <span data-ttu-id="053ee-112">Distribuce</span><span class="sxs-lookup"><span data-stu-id="053ee-112">Distribution</span></span> | <span data-ttu-id="053ee-113">Verze</span><span class="sxs-lookup"><span data-stu-id="053ee-113">Version</span></span> |
|---|---|
| <span data-ttu-id="053ee-114">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="053ee-114">Ubuntu</span></span> | <span data-ttu-id="053ee-115">16.04 LTS, 14.04 LTS a 12.04 LTS</span><span class="sxs-lookup"><span data-stu-id="053ee-115">16.04 LTS, 14.04 LTS and 12.04 LTS</span></span> |
| <span data-ttu-id="053ee-116">Debian</span><span class="sxs-lookup"><span data-stu-id="053ee-116">Debian</span></span> | <span data-ttu-id="053ee-117">7 a 8</span><span class="sxs-lookup"><span data-stu-id="053ee-117">7 and 8</span></span> |
| <span data-ttu-id="053ee-118">RedHat</span><span class="sxs-lookup"><span data-stu-id="053ee-118">RedHat</span></span> | <span data-ttu-id="053ee-119">6.x a 7.x</span><span class="sxs-lookup"><span data-stu-id="053ee-119">6.x and 7.x</span></span> |
| <span data-ttu-id="053ee-120">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="053ee-120">Oracle Linux</span></span> | <span data-ttu-id="053ee-121">7 x</span><span class="sxs-lookup"><span data-stu-id="053ee-121">7x</span></span> |
| <span data-ttu-id="053ee-122">SuSE</span><span class="sxs-lookup"><span data-stu-id="053ee-122">Suse</span></span> | <span data-ttu-id="053ee-123">11 a 12</span><span class="sxs-lookup"><span data-stu-id="053ee-123">11 and 12</span></span> |
| <span data-ttu-id="053ee-124">OpenSuse</span><span class="sxs-lookup"><span data-stu-id="053ee-124">OpenSuse</span></span> | <span data-ttu-id="053ee-125">7.0</span><span class="sxs-lookup"><span data-stu-id="053ee-125">7.0</span></span> |
| <span data-ttu-id="053ee-126">CentOS</span><span class="sxs-lookup"><span data-stu-id="053ee-126">CentOS</span></span> | <span data-ttu-id="053ee-127">7.0</span><span class="sxs-lookup"><span data-stu-id="053ee-127">7.0</span></span> |

<span data-ttu-id="053ee-128">Všimněte si, že se v tuto chvíli nepodporuje CoreOS.</span><span class="sxs-lookup"><span data-stu-id="053ee-128">Note that CoreOS is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="053ee-129">Připojení k internetu</span><span class="sxs-lookup"><span data-stu-id="053ee-129">Internet connectivity</span></span>

<span data-ttu-id="053ee-130">Některé funkce sítě sledovacích procesů agenta vyžaduje, aby cílový virtuální počítač připojen k Internetu.</span><span class="sxs-lookup"><span data-stu-id="053ee-130">Some of the Network Watcher Agent functionality requires that the target virtual machine be connected to the Internet.</span></span> <span data-ttu-id="053ee-131">Bez možnosti navázat odchozí připojení některé funkce sítě sledovacích procesů agenta nebude fungovat správně nebo nedostupná.</span><span class="sxs-lookup"><span data-stu-id="053ee-131">Without the ability to establish outgoing connections some of the Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="053ee-132">Další podrobnosti najdete v tématu [sledovací proces sítě dokumentaci](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span><span class="sxs-lookup"><span data-stu-id="053ee-132">For more details, please see the [Network Watcher documentation](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="053ee-133">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="053ee-133">Extension schema</span></span>

<span data-ttu-id="053ee-134">Následujícím kódu JSON znázorňuje schéma pro rozšíření sítě sledovacích procesů agenta.</span><span class="sxs-lookup"><span data-stu-id="053ee-134">The following JSON shows the schema for the Network Watcher Agent extension.</span></span> <span data-ttu-id="053ee-135">Rozšíření ani jeden z nich vyžaduje ani podporuje nastavení uživatelem zadané v tuto chvíli a spoléhá na jeho výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="053ee-135">The extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

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
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a><span data-ttu-id="053ee-136">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="053ee-136">Property values</span></span>

| <span data-ttu-id="053ee-137">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="053ee-137">Name</span></span> | <span data-ttu-id="053ee-138">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="053ee-138">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="053ee-139">apiVersion</span><span class="sxs-lookup"><span data-stu-id="053ee-139">apiVersion</span></span> | <span data-ttu-id="053ee-140">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="053ee-140">2015-06-15</span></span> |
| <span data-ttu-id="053ee-141">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="053ee-141">publisher</span></span> | <span data-ttu-id="053ee-142">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="053ee-142">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="053ee-143">type</span><span class="sxs-lookup"><span data-stu-id="053ee-143">type</span></span> | <span data-ttu-id="053ee-144">NetworkWatcherAgentLinux</span><span class="sxs-lookup"><span data-stu-id="053ee-144">NetworkWatcherAgentLinux</span></span> |
| <span data-ttu-id="053ee-145">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="053ee-145">typeHandlerVersion</span></span> | <span data-ttu-id="053ee-146">1.4</span><span class="sxs-lookup"><span data-stu-id="053ee-146">1.4</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="053ee-147">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="053ee-147">Template deployment</span></span>

<span data-ttu-id="053ee-148">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="053ee-148">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="053ee-149">Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření sítě sledovacích procesů agenta při nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="053ee-149">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="azure-cli-deployment"></a><span data-ttu-id="053ee-150">Nasazení Azure CLI</span><span class="sxs-lookup"><span data-stu-id="053ee-150">Azure CLI deployment</span></span>

<span data-ttu-id="053ee-151">Rozhraní příkazového řádku Azure můžete použít k nasazení rozšíření sítě sledovacích procesů agenta virtuálního počítače do existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="053ee-151">The Azure CLI can be used to deploy the Network Watcher Agent VM extension to an existing virtual machine.</span></span>

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="053ee-152">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="053ee-152">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="053ee-153">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="053ee-153">Troubleshooting</span></span>

<span data-ttu-id="053ee-154">Z portálu Azure a pomocí rozhraní příkazového řádku Azure je možné načíst data o stavu nasazení rozšíření.</span><span class="sxs-lookup"><span data-stu-id="053ee-154">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure CLI.</span></span> <span data-ttu-id="053ee-155">Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="053ee-155">To see the deployment state of extensions for a given VM, run the following command using the Azure CLI.</span></span>

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

<span data-ttu-id="053ee-156">Výstupu spuštění rozšíření se zaznamenává soubory, které jsou v následujícím adresáři:</span><span class="sxs-lookup"><span data-stu-id="053ee-156">Extension execution output is logged to files found in the following directory:</span></span>

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a><span data-ttu-id="053ee-157">Podpora</span><span class="sxs-lookup"><span data-stu-id="053ee-157">Support</span></span>

<span data-ttu-id="053ee-158">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, můžete v dokumentaci sledovací proces sítě nebo se obraťte na Azure odborníky [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="053ee-158">If you need more help at any point in this article, you can refer to the Network Watcher documentation or contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="053ee-159">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="053ee-159">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="053ee-160">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="053ee-160">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="053ee-161">Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="053ee-161">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
