---
title: "aaaOMS rozšíření virtuálního počítače Azure pro Linux | Microsoft Docs"
description: "Nasaďte agenta OMS hello na virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 0fc8003d1fae6c043eef18ae78d12f9304b70832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a><span data-ttu-id="1aeca-103">OMS rozšíření virtuálního počítače pro Linux</span><span class="sxs-lookup"><span data-stu-id="1aeca-103">OMS virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="1aeca-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1aeca-104">Overview</span></span>

<span data-ttu-id="1aeca-105">Operations Management Suite (OMS) poskytuje možnosti nápravy monitorování, výstrahy a výstrahy v cloudové a místní prostředky.</span><span class="sxs-lookup"><span data-stu-id="1aeca-105">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="1aeca-106">Hello rozšíření virtuálního počítače OMS agenta pro Linux je publikována a společnost Microsoft podporuje.</span><span class="sxs-lookup"><span data-stu-id="1aeca-106">hello OMS Agent virtual machine extension for Linux is published and supported by Microsoft.</span></span> <span data-ttu-id="1aeca-107">rozšíření Hello nainstaluje agenta OMS hello na virtuálních počítačích Azure a zaregistruje virtuální počítače do existující pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="1aeca-107">hello extension installs hello OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="1aeca-108">Tento dokument podrobnosti hello podporované platformy, konfigurace a možnosti nasazení pro hello OMS rozšíření virtuálního počítače pro Linux.</span><span class="sxs-lookup"><span data-stu-id="1aeca-108">This document details hello supported platforms, configurations, and deployment options for hello OMS virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1aeca-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1aeca-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="1aeca-110">Operační systém</span><span class="sxs-lookup"><span data-stu-id="1aeca-110">Operating system</span></span>

<span data-ttu-id="1aeca-111">Hello rozšíření agenta OMS je možné spustit proti tyto Linuxových distribucích.</span><span class="sxs-lookup"><span data-stu-id="1aeca-111">hello OMS Agent extension can be run against these Linux distributions.</span></span>

| <span data-ttu-id="1aeca-112">Distribuce</span><span class="sxs-lookup"><span data-stu-id="1aeca-112">Distribution</span></span> | <span data-ttu-id="1aeca-113">Verze</span><span class="sxs-lookup"><span data-stu-id="1aeca-113">Version</span></span> |
|---|---|
| <span data-ttu-id="1aeca-114">CentOS Linux</span><span class="sxs-lookup"><span data-stu-id="1aeca-114">CentOS Linux</span></span> | <span data-ttu-id="1aeca-115">5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="1aeca-115">5, 6, and 7</span></span> |
| <span data-ttu-id="1aeca-116">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="1aeca-116">Oracle Linux</span></span> | <span data-ttu-id="1aeca-117">5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="1aeca-117">5, 6, and 7</span></span> |
| <span data-ttu-id="1aeca-118">Red Hat Enterprise Linux Server</span><span class="sxs-lookup"><span data-stu-id="1aeca-118">Red Hat Enterprise Linux Server</span></span> | <span data-ttu-id="1aeca-119">5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="1aeca-119">5, 6 and 7</span></span> |
| <span data-ttu-id="1aeca-120">Debian GNU/Linux</span><span class="sxs-lookup"><span data-stu-id="1aeca-120">Debian GNU/Linux</span></span> | <span data-ttu-id="1aeca-121">6, 7 a 8</span><span class="sxs-lookup"><span data-stu-id="1aeca-121">6, 7, and 8</span></span> |
| <span data-ttu-id="1aeca-122">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="1aeca-122">Ubuntu</span></span> | <span data-ttu-id="1aeca-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="1aeca-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span></span> |
| <span data-ttu-id="1aeca-124">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="1aeca-124">SUSE Linux Enterprise Server</span></span> | <span data-ttu-id="1aeca-125">11 a 12</span><span class="sxs-lookup"><span data-stu-id="1aeca-125">11 and 12</span></span> |

### <a name="internet-connectivity"></a><span data-ttu-id="1aeca-126">Připojení k internetu</span><span class="sxs-lookup"><span data-stu-id="1aeca-126">Internet connectivity</span></span>

<span data-ttu-id="1aeca-127">Hello rozšíření OMS agenta pro Linux vyžaduje, aby hello cílový virtuální počítač je připojený toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="1aeca-127">hello OMS Agent extension for Linux requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="1aeca-128">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="1aeca-128">Extension schema</span></span>

<span data-ttu-id="1aeca-129">Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření agenta OMS.</span><span class="sxs-lookup"><span data-stu-id="1aeca-129">hello following JSON shows hello schema for hello OMS Agent extension.</span></span> <span data-ttu-id="1aeca-130">rozšíření Hello vyžaduje klíč ID a pracovního prostoru pracovní prostor hello z pracovní prostor OMS cíl hello; Tyto hodnoty můžete najít na portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="1aeca-130">hello extension requires hello workspace ID and workspace key from hello target OMS workspace; these values can be found in hello OMS portal.</span></span> <span data-ttu-id="1aeca-131">Protože hello klíč pracovního prostoru by měl být považován za citlivá data, by měly být uložené v chráněném nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1aeca-131">Because hello workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="1aeca-132">Azure data virtuálního počítače chráněný rozšíření nastavení je zašifrovaná a dešifrovat jenom na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1aeca-132">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span> <span data-ttu-id="1aeca-133">Všimněte si, že **workspaceId** a **workspaceKey** malých a velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="1aeca-133">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a><span data-ttu-id="1aeca-134">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="1aeca-134">Property values</span></span>

| <span data-ttu-id="1aeca-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="1aeca-135">Name</span></span> | <span data-ttu-id="1aeca-136">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="1aeca-136">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="1aeca-137">apiVersion</span><span class="sxs-lookup"><span data-stu-id="1aeca-137">apiVersion</span></span> | <span data-ttu-id="1aeca-138">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="1aeca-138">2015-06-15</span></span> |
| <span data-ttu-id="1aeca-139">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="1aeca-139">publisher</span></span> | <span data-ttu-id="1aeca-140">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="1aeca-140">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="1aeca-141">type</span><span class="sxs-lookup"><span data-stu-id="1aeca-141">type</span></span> | <span data-ttu-id="1aeca-142">OmsAgentForLinux</span><span class="sxs-lookup"><span data-stu-id="1aeca-142">OmsAgentForLinux</span></span> |
| <span data-ttu-id="1aeca-143">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="1aeca-143">typeHandlerVersion</span></span> | <span data-ttu-id="1aeca-144">1.4</span><span class="sxs-lookup"><span data-stu-id="1aeca-144">1.4</span></span> |
| <span data-ttu-id="1aeca-145">ID pracovního prostoru (např.)</span><span class="sxs-lookup"><span data-stu-id="1aeca-145">workspaceId (e.g)</span></span> | <span data-ttu-id="1aeca-146">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="1aeca-146">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="1aeca-147">workspaceKey (např.)</span><span class="sxs-lookup"><span data-stu-id="1aeca-147">workspaceKey (e.g)</span></span> | <span data-ttu-id="1aeca-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="1aeca-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="1aeca-149">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="1aeca-149">Template deployment</span></span>

<span data-ttu-id="1aeca-150">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1aeca-150">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="1aeca-151">Šablony jsou ideální, pokud nasazujete jednu nebo více virtuálních počítačů, které vyžadují konfiguraci nasazení post například tooOMS registrace.</span><span class="sxs-lookup"><span data-stu-id="1aeca-151">Templates are ideal when deploying one or more virtual machines that require post deployment configuration such as onboarding tooOMS.</span></span> <span data-ttu-id="1aeca-152">Ukázka šablonu Resource Manager, která obsahuje rozšíření virtuálního počítače agenta OMS hello naleznete na hello [Azure rychlý Start Galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span><span class="sxs-lookup"><span data-stu-id="1aeca-152">A sample Resource Manager template that includes hello OMS Agent VM extension can be found on hello [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span></span> 

<span data-ttu-id="1aeca-153">Konfigurace Hello JSON pro rozšíření virtuálního počítače můžete vnořit hello prostředek virtuálního počítače nebo umístěny na nejvyšší úrovni šablony Resource Manageru JSON nebo hello kořenové.</span><span class="sxs-lookup"><span data-stu-id="1aeca-153">hello JSON configuration for a virtual machine extension can be nested inside hello virtual machine resource, or placed at hello root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="1aeca-154">umístění Hello hello JSON konfigurace ovlivní hodnotu hello hello název prostředku a typem.</span><span class="sxs-lookup"><span data-stu-id="1aeca-154">hello placement of hello JSON configuration affects hello value of hello resource name and type.</span></span> <span data-ttu-id="1aeca-155">Další informace najdete v tématu [nastavte název a typ pro podřízené prostředky](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="1aeca-155">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="1aeca-156">Hello následující příklad předpokládá, že rozšíření OMS hello je vnořit hello prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1aeca-156">hello following example assumes hello OMS extension is nested inside hello virtual machine resource.</span></span> <span data-ttu-id="1aeca-157">Při vnoření hello rozšíření prostředků, hello JSON je umístěn v hello `"resources": []` objekt hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1aeca-157">When nesting hello extension resource, hello JSON is placed in hello `"resources": []` object of hello virtual machine.</span></span>

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

<span data-ttu-id="1aeca-158">Při vkládání hello rozšíření JSON v kořenu hello hello šablony, název prostředku hello zahrnuje odkaz toohello nadřazený virtuální počítač a typ hello odráží hello vnořené konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1aeca-158">When placing hello extension JSON at hello root of hello template, hello resource name includes a reference toohello parent virtual machine, and hello type reflects hello nested configuration.</span></span>  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a><span data-ttu-id="1aeca-159">Nasazení Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1aeca-159">Azure CLI deployment</span></span>

<span data-ttu-id="1aeca-160">Hello rozhraní příkazového řádku Azure se dá použít toodeploy hello VM agenta OMS rozšíření tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1aeca-160">hello Azure CLI can be used toodeploy hello OMS Agent VM extension tooan existing virtual machine.</span></span> <span data-ttu-id="1aeca-161">Nahraďte klíč OMS hello a OMS ID s těmi, která z pracovního prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="1aeca-161">Replace hello OMS key and OMS ID with those from your OMS workspace.</span></span> 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="1aeca-162">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="1aeca-162">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="1aeca-163">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="1aeca-163">Troubleshoot</span></span>

<span data-ttu-id="1aeca-164">Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1aeca-164">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure CLI.</span></span> <span data-ttu-id="1aeca-165">Stav nasazení hello toosee rozšíření pro daný virtuální počítač, spusťte následující příkaz pomocí hello hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1aeca-165">toosee hello deployment state of extensions for a given VM, run hello following command using hello Azure CLI.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="1aeca-166">Rozšíření spuštění výstup protokolu toohello následující soubor:</span><span class="sxs-lookup"><span data-stu-id="1aeca-166">Extension execution output is logged toohello following file:</span></span>

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a><span data-ttu-id="1aeca-167">Kódy chyb a jejich významů</span><span class="sxs-lookup"><span data-stu-id="1aeca-167">Error codes and their meanings</span></span>

| <span data-ttu-id="1aeca-168">Kód chyby</span><span class="sxs-lookup"><span data-stu-id="1aeca-168">Error Code</span></span> | <span data-ttu-id="1aeca-169">Význam</span><span class="sxs-lookup"><span data-stu-id="1aeca-169">Meaning</span></span> | <span data-ttu-id="1aeca-170">Možné akce</span><span class="sxs-lookup"><span data-stu-id="1aeca-170">Possible Action</span></span> |
| :---: | --- | --- |
| <span data-ttu-id="1aeca-171">10</span><span class="sxs-lookup"><span data-stu-id="1aeca-171">10</span></span> | <span data-ttu-id="1aeca-172">Virtuální počítač je již připojené tooan pracovním prostorem OMS</span><span class="sxs-lookup"><span data-stu-id="1aeca-172">VM is already connected tooan OMS workspace</span></span> | <span data-ttu-id="1aeca-173">tooconnect hello virtuálních počítačů toohello prostoru zadaný v hello rozšíření schématu, nastavte stopOnMultipleConnections toofalse v nastavení veřejných nebo odeberte tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1aeca-173">tooconnect hello VM toohello workspace specified in hello extension schema, set stopOnMultipleConnections toofalse in public settings or remove this property.</span></span> <span data-ttu-id="1aeca-174">Tento virtuální počítač získá účtován pro každý pracovní prostor po připojení.</span><span class="sxs-lookup"><span data-stu-id="1aeca-174">This VM gets billed once for each workspace it is connected to.</span></span> |
| <span data-ttu-id="1aeca-175">11</span><span class="sxs-lookup"><span data-stu-id="1aeca-175">11</span></span> | <span data-ttu-id="1aeca-176">Neplatná konfigurace poskytuje toohello rozšíření</span><span class="sxs-lookup"><span data-stu-id="1aeca-176">Invalid config provided toohello extension</span></span> | <span data-ttu-id="1aeca-177">Postupujte podle hello předcházející příklady tooset všechny hodnoty vlastností nezbytné pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="1aeca-177">Follow hello preceding examples tooset all property values necessary for deployment.</span></span> |
| <span data-ttu-id="1aeca-178">12</span><span class="sxs-lookup"><span data-stu-id="1aeca-178">12</span></span> | <span data-ttu-id="1aeca-179">Správce balíčků dpkg Hello je uzamčena.</span><span class="sxs-lookup"><span data-stu-id="1aeca-179">hello dpkg package manager is locked</span></span> | <span data-ttu-id="1aeca-180">Zajistěte, aby všechny dpkg operace aktualizace na počítači hello dokončení a akci opakujte.</span><span class="sxs-lookup"><span data-stu-id="1aeca-180">Make sure all dpkg update operations on hello machine have finished and retry.</span></span> |
| <span data-ttu-id="1aeca-181">20</span><span class="sxs-lookup"><span data-stu-id="1aeca-181">20</span></span> | <span data-ttu-id="1aeca-182">Povolit názvem předčasně</span><span class="sxs-lookup"><span data-stu-id="1aeca-182">Enable called prematurely</span></span> | <span data-ttu-id="1aeca-183">[Aktualizace hello Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello nejnovější dostupnou verzi.</span><span class="sxs-lookup"><span data-stu-id="1aeca-183">[Update hello Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) toohello latest available version.</span></span> |
| <span data-ttu-id="1aeca-184">51</span><span class="sxs-lookup"><span data-stu-id="1aeca-184">51</span></span> | <span data-ttu-id="1aeca-185">Toto rozšíření není podporována v systému operace hello Virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1aeca-185">This extension is not supported on hello VM's operation system</span></span> | |
| <span data-ttu-id="1aeca-186">55</span><span class="sxs-lookup"><span data-stu-id="1aeca-186">55</span></span> | <span data-ttu-id="1aeca-187">Nelze se připojit službu Microsoft Operations Management Suite toohello</span><span class="sxs-lookup"><span data-stu-id="1aeca-187">Cannot connect toohello Microsoft Operations Management Suite service</span></span> | <span data-ttu-id="1aeca-188">Zkontrolujte, zda text hello systém má přístup k Internetu nebo že bylo zadáno platný proxy server HTTP.</span><span class="sxs-lookup"><span data-stu-id="1aeca-188">Check that hello system either has Internet access, or that a valid HTTP proxy has been provided.</span></span> <span data-ttu-id="1aeca-189">Kromě toho zkontrolujte správnost hello hello ID pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="1aeca-189">Additionally, check hello correctness of hello workspace ID.</span></span> |

<span data-ttu-id="1aeca-190">Další informace o odstraňování potíží naleznete na hello [Průvodce odstraňováním potíží OMS agenta pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span><span class="sxs-lookup"><span data-stu-id="1aeca-190">Additional troubleshooting information can be found on hello [OMS-Agent-for-Linux Troubleshooting Guide](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span></span>

### <a name="support"></a><span data-ttu-id="1aeca-191">Podpora</span><span class="sxs-lookup"><span data-stu-id="1aeca-191">Support</span></span>

<span data-ttu-id="1aeca-192">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="1aeca-192">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="1aeca-193">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="1aeca-193">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="1aeca-194">Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="1aeca-194">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="1aeca-195">Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="1aeca-195">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
