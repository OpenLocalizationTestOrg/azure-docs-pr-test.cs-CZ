---
title: "aaaAzure rozšíření virtuálního počítače sítě sledovacích procesů agenta pro Linux | Microsoft Docs"
description: "Nasaďte hello sítě sledovacích procesů agenta na virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače."
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
ms.openlocfilehash: 84bed132cbda83d0917be490f9a50914578952a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a><span data-ttu-id="b240c-103">Sítě rozšíření virtuálního počítače sledovacích procesů agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="b240c-103">Network Watcher Agent virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="b240c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b240c-104">Overview</span></span>

<span data-ttu-id="b240c-105">[Azure sledovací proces sítě](https://review.docs.microsoft.com/en-us/azure/network-watcher/) je sítě výkonu monitorování, diagnostiku a analýzy služba, která umožňuje monitorování pro sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="b240c-105">[Azure Network Watcher](https://review.docs.microsoft.com/en-us/azure/network-watcher/) is a network performance monitoring, diagnostic, and analytics service that allows monitoring for Azure networks.</span></span> <span data-ttu-id="b240c-106">Hello rozšíření sítě sledovacích procesů agenta virtuálního počítače není pro některé funkce hello sledovací proces sítě na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="b240c-106">hello Network Watcher Agent virtual machine extension is a requirement for some of hello Network Watcher features on Azure virtual machines.</span></span> <span data-ttu-id="b240c-107">To zahrnuje Zachytávání síťových přenosů na vyžádání a další pokročilé funkce.</span><span class="sxs-lookup"><span data-stu-id="b240c-107">This includes capturing network traffic on demand and other advanced functionality.</span></span>

<span data-ttu-id="b240c-108">Tento dokument podrobnosti hello podporované platformy a možnosti nasazení pro hello rozšíření virtuálního počítače sítě sledovacích procesů agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="b240c-108">This document details hello supported platforms and deployment options for hello Network Watcher Agent virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b240c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b240c-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="b240c-110">Operační systém</span><span class="sxs-lookup"><span data-stu-id="b240c-110">Operating system</span></span>

<span data-ttu-id="b240c-111">pro tyto Linuxových distribucích můžete spouštět Hello sítě sledovacích procesů agenta rozšíření:</span><span class="sxs-lookup"><span data-stu-id="b240c-111">hello Network Watcher Agent extension can be run against these Linux distributions:</span></span>

| <span data-ttu-id="b240c-112">Distribuce</span><span class="sxs-lookup"><span data-stu-id="b240c-112">Distribution</span></span> | <span data-ttu-id="b240c-113">Verze</span><span class="sxs-lookup"><span data-stu-id="b240c-113">Version</span></span> |
|---|---|
| <span data-ttu-id="b240c-114">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="b240c-114">Ubuntu</span></span> | <span data-ttu-id="b240c-115">16.04 LTS, 14.04 LTS a 12.04 LTS</span><span class="sxs-lookup"><span data-stu-id="b240c-115">16.04 LTS, 14.04 LTS and 12.04 LTS</span></span> |
| <span data-ttu-id="b240c-116">Debian</span><span class="sxs-lookup"><span data-stu-id="b240c-116">Debian</span></span> | <span data-ttu-id="b240c-117">7 a 8</span><span class="sxs-lookup"><span data-stu-id="b240c-117">7 and 8</span></span> |
| <span data-ttu-id="b240c-118">RedHat</span><span class="sxs-lookup"><span data-stu-id="b240c-118">RedHat</span></span> | <span data-ttu-id="b240c-119">6.x a 7.x</span><span class="sxs-lookup"><span data-stu-id="b240c-119">6.x and 7.x</span></span> |
| <span data-ttu-id="b240c-120">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="b240c-120">Oracle Linux</span></span> | <span data-ttu-id="b240c-121">7 x</span><span class="sxs-lookup"><span data-stu-id="b240c-121">7x</span></span> |
| <span data-ttu-id="b240c-122">SuSE</span><span class="sxs-lookup"><span data-stu-id="b240c-122">Suse</span></span> | <span data-ttu-id="b240c-123">11 a 12</span><span class="sxs-lookup"><span data-stu-id="b240c-123">11 and 12</span></span> |
| <span data-ttu-id="b240c-124">OpenSuse</span><span class="sxs-lookup"><span data-stu-id="b240c-124">OpenSuse</span></span> | <span data-ttu-id="b240c-125">7.0</span><span class="sxs-lookup"><span data-stu-id="b240c-125">7.0</span></span> |
| <span data-ttu-id="b240c-126">CentOS</span><span class="sxs-lookup"><span data-stu-id="b240c-126">CentOS</span></span> | <span data-ttu-id="b240c-127">7.0</span><span class="sxs-lookup"><span data-stu-id="b240c-127">7.0</span></span> |

<span data-ttu-id="b240c-128">Všimněte si, že se v tuto chvíli nepodporuje CoreOS.</span><span class="sxs-lookup"><span data-stu-id="b240c-128">Note that CoreOS is not supported at this time.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="b240c-129">Připojení k internetu</span><span class="sxs-lookup"><span data-stu-id="b240c-129">Internet connectivity</span></span>

<span data-ttu-id="b240c-130">Některé hello funkce sítě sledovacích procesů agenta vyžaduje, aby hello cílový virtuální počítač připojené toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="b240c-130">Some of hello Network Watcher Agent functionality requires that hello target virtual machine be connected toohello Internet.</span></span> <span data-ttu-id="b240c-131">Bez hello možnost tooestablish odchozí připojení některé funkce sítě sledovacích procesů agenta hello nebude fungovat správně nebo nedostupná.</span><span class="sxs-lookup"><span data-stu-id="b240c-131">Without hello ability tooestablish outgoing connections some of hello Network Watcher Agent features may malfunction or become unavailable.</span></span> <span data-ttu-id="b240c-132">Další podrobnosti najdete v tématu hello [sledovací proces sítě dokumentaci](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span><span class="sxs-lookup"><span data-stu-id="b240c-132">For more details, please see hello [Network Watcher documentation](https://review.docs.microsoft.com/en-us/azure/network-watcher/).</span></span>

## <a name="extension-schema"></a><span data-ttu-id="b240c-133">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="b240c-133">Extension schema</span></span>

<span data-ttu-id="b240c-134">Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření sítě sledovacích procesů agenta.</span><span class="sxs-lookup"><span data-stu-id="b240c-134">hello following JSON shows hello schema for hello Network Watcher Agent extension.</span></span> <span data-ttu-id="b240c-135">rozšíření Hello ani jeden z nich vyžaduje ani podporuje nastavení uživatelem zadané v tuto chvíli a spoléhá na jeho výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b240c-135">hello extension neither requires nor supports any user-supplied settings at this time and relies on its default configuration.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="b240c-136">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="b240c-136">Property values</span></span>

| <span data-ttu-id="b240c-137">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b240c-137">Name</span></span> | <span data-ttu-id="b240c-138">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="b240c-138">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="b240c-139">apiVersion</span><span class="sxs-lookup"><span data-stu-id="b240c-139">apiVersion</span></span> | <span data-ttu-id="b240c-140">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="b240c-140">2015-06-15</span></span> |
| <span data-ttu-id="b240c-141">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="b240c-141">publisher</span></span> | <span data-ttu-id="b240c-142">Microsoft.Azure.NetworkWatcher</span><span class="sxs-lookup"><span data-stu-id="b240c-142">Microsoft.Azure.NetworkWatcher</span></span> |
| <span data-ttu-id="b240c-143">type</span><span class="sxs-lookup"><span data-stu-id="b240c-143">type</span></span> | <span data-ttu-id="b240c-144">NetworkWatcherAgentLinux</span><span class="sxs-lookup"><span data-stu-id="b240c-144">NetworkWatcherAgentLinux</span></span> |
| <span data-ttu-id="b240c-145">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="b240c-145">typeHandlerVersion</span></span> | <span data-ttu-id="b240c-146">1.4</span><span class="sxs-lookup"><span data-stu-id="b240c-146">1.4</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="b240c-147">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="b240c-147">Template deployment</span></span>

<span data-ttu-id="b240c-148">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b240c-148">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="b240c-149">popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager sítě sledovacích procesů agenta rozšíření během nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b240c-149">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Network Watcher Agent extension during an Azure Resource Manager template deployment.</span></span>

## <a name="azure-cli-deployment"></a><span data-ttu-id="b240c-150">Nasazení Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b240c-150">Azure CLI deployment</span></span>

<span data-ttu-id="b240c-151">Hello rozhraní příkazového řádku Azure se dá použít toodeploy hello sítě sledovacích procesů agenta virtuálního počítače rozšíření tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b240c-151">hello Azure CLI can be used toodeploy hello Network Watcher Agent VM extension tooan existing virtual machine.</span></span>

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a><span data-ttu-id="b240c-152">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="b240c-152">Troubleshooting and support</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="b240c-153">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b240c-153">Troubleshooting</span></span>

<span data-ttu-id="b240c-154">Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b240c-154">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure CLI.</span></span> <span data-ttu-id="b240c-155">Stav nasazení hello toosee rozšíření pro daný virtuální počítač, spusťte následující příkaz pomocí hello hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b240c-155">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure CLI.</span></span>

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

<span data-ttu-id="b240c-156">Rozšíření spuštění výstup je zaznamenané toofiles najít v hello následující adresář:</span><span class="sxs-lookup"><span data-stu-id="b240c-156">Extension execution output is logged toofiles found in hello following directory:</span></span>

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a><span data-ttu-id="b240c-157">Podpora</span><span class="sxs-lookup"><span data-stu-id="b240c-157">Support</span></span>

<span data-ttu-id="b240c-158">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, můžete naleznete v dokumentaci toohello sledovací proces sítě nebo se obraťte hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="b240c-158">If you need more help at any point in this article, you can refer toohello Network Watcher documentation or contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="b240c-159">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="b240c-159">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="b240c-160">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="b240c-160">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="b240c-161">Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="b240c-161">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
