---
title: "Rozšíření virtuálního počítače OMS Azure pro Linux | Microsoft Docs"
description: "Nasaďte agenta OMS na virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače."
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
ms.openlocfilehash: 138fc8c98ea6f409b28407b20851c96ecc618b09
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a><span data-ttu-id="6cfec-103">OMS rozšíření virtuálního počítače pro Linux</span><span class="sxs-lookup"><span data-stu-id="6cfec-103">OMS virtual machine extension for Linux</span></span>

## <a name="overview"></a><span data-ttu-id="6cfec-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6cfec-104">Overview</span></span>

<span data-ttu-id="6cfec-105">Operations Management Suite (OMS) poskytuje možnosti nápravy monitorování, výstrahy a výstrahy v cloudové a místní prostředky.</span><span class="sxs-lookup"><span data-stu-id="6cfec-105">Operations Management Suite (OMS) provides monitoring, alerting, and alert remediation capabilities across cloud and on-premises assets.</span></span> <span data-ttu-id="6cfec-106">Rozšíření virtuálního počítače OMS agenta pro Linux je publikována a společnost Microsoft podporuje.</span><span class="sxs-lookup"><span data-stu-id="6cfec-106">The OMS Agent virtual machine extension for Linux is published and supported by Microsoft.</span></span> <span data-ttu-id="6cfec-107">Rozšíření nainstaluje agenta OMS na virtuálních počítačích Azure a zaregistruje virtuální počítače do existující pracovní prostor OMS.</span><span class="sxs-lookup"><span data-stu-id="6cfec-107">The extension installs the OMS agent on Azure virtual machines, and enrolls virtual machines into an existing OMS workspace.</span></span> <span data-ttu-id="6cfec-108">Tento dokument podrobně popisuje podporované platformy, konfigurace a možnosti nasazení pro rozšíření virtuálního počítače OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="6cfec-108">This document details the supported platforms, configurations, and deployment options for the OMS virtual machine extension for Linux.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6cfec-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6cfec-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="6cfec-110">Operační systém</span><span class="sxs-lookup"><span data-stu-id="6cfec-110">Operating system</span></span>

<span data-ttu-id="6cfec-111">Rozšíření agenta OMS se může spouštět na těchto Linuxových distribucích.</span><span class="sxs-lookup"><span data-stu-id="6cfec-111">The OMS Agent extension can be run against these Linux distributions.</span></span>

| <span data-ttu-id="6cfec-112">Distribuce</span><span class="sxs-lookup"><span data-stu-id="6cfec-112">Distribution</span></span> | <span data-ttu-id="6cfec-113">Verze</span><span class="sxs-lookup"><span data-stu-id="6cfec-113">Version</span></span> |
|---|---|
| <span data-ttu-id="6cfec-114">CentOS Linux</span><span class="sxs-lookup"><span data-stu-id="6cfec-114">CentOS Linux</span></span> | <span data-ttu-id="6cfec-115">5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="6cfec-115">5, 6, and 7</span></span> |
| <span data-ttu-id="6cfec-116">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="6cfec-116">Oracle Linux</span></span> | <span data-ttu-id="6cfec-117">5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="6cfec-117">5, 6, and 7</span></span> |
| <span data-ttu-id="6cfec-118">Red Hat Enterprise Linux Server</span><span class="sxs-lookup"><span data-stu-id="6cfec-118">Red Hat Enterprise Linux Server</span></span> | <span data-ttu-id="6cfec-119">5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="6cfec-119">5, 6 and 7</span></span> |
| <span data-ttu-id="6cfec-120">Debian GNU/Linux</span><span class="sxs-lookup"><span data-stu-id="6cfec-120">Debian GNU/Linux</span></span> | <span data-ttu-id="6cfec-121">6, 7 a 8</span><span class="sxs-lookup"><span data-stu-id="6cfec-121">6, 7, and 8</span></span> |
| <span data-ttu-id="6cfec-122">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="6cfec-122">Ubuntu</span></span> | <span data-ttu-id="6cfec-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="6cfec-123">12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS</span></span> |
| <span data-ttu-id="6cfec-124">SUSE Linux Enterprise Server</span><span class="sxs-lookup"><span data-stu-id="6cfec-124">SUSE Linux Enterprise Server</span></span> | <span data-ttu-id="6cfec-125">11 a 12</span><span class="sxs-lookup"><span data-stu-id="6cfec-125">11 and 12</span></span> |

### <a name="internet-connectivity"></a><span data-ttu-id="6cfec-126">Připojení k internetu</span><span class="sxs-lookup"><span data-stu-id="6cfec-126">Internet connectivity</span></span>

<span data-ttu-id="6cfec-127">Rozšíření OMS agenta pro Linux vyžaduje, aby cílový virtuální počítač je připojený k Internetu.</span><span class="sxs-lookup"><span data-stu-id="6cfec-127">The OMS Agent extension for Linux requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="6cfec-128">Rozšíření schématu</span><span class="sxs-lookup"><span data-stu-id="6cfec-128">Extension schema</span></span>

<span data-ttu-id="6cfec-129">Následujícím kódu JSON znázorňuje schéma pro rozšíření agenta OMS.</span><span class="sxs-lookup"><span data-stu-id="6cfec-129">The following JSON shows the schema for the OMS Agent extension.</span></span> <span data-ttu-id="6cfec-130">Rozšíření vyžaduje ID a klíč pracovního prostoru z cílový pracovní prostor OMS; Tyto hodnoty můžete najít na portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="6cfec-130">The extension requires the workspace ID and workspace key from the target OMS workspace; these values can be found in the OMS portal.</span></span> <span data-ttu-id="6cfec-131">Vzhledem k tomu, že klíč pracovního prostoru by měl být považován za citlivá data, by měly být uložené v chráněném nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6cfec-131">Because the workspace key should be treated as sensitive data, it should be stored in a protected setting configuration.</span></span> <span data-ttu-id="6cfec-132">Data Azure nastavení rozšíření chráněný virtuální počítač je zašifrovaná a dešifrovat jenom na cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6cfec-132">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span> <span data-ttu-id="6cfec-133">Všimněte si, že **workspaceId** a **workspaceKey** malých a velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="6cfec-133">Note that **workspaceId** and **workspaceKey** are case-sensitive.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="6cfec-134">Hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="6cfec-134">Property values</span></span>

| <span data-ttu-id="6cfec-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="6cfec-135">Name</span></span> | <span data-ttu-id="6cfec-136">Hodnota nebo příklad</span><span class="sxs-lookup"><span data-stu-id="6cfec-136">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="6cfec-137">apiVersion</span><span class="sxs-lookup"><span data-stu-id="6cfec-137">apiVersion</span></span> | <span data-ttu-id="6cfec-138">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="6cfec-138">2015-06-15</span></span> |
| <span data-ttu-id="6cfec-139">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="6cfec-139">publisher</span></span> | <span data-ttu-id="6cfec-140">Microsoft.EnterpriseCloud.Monitoring</span><span class="sxs-lookup"><span data-stu-id="6cfec-140">Microsoft.EnterpriseCloud.Monitoring</span></span> |
| <span data-ttu-id="6cfec-141">type</span><span class="sxs-lookup"><span data-stu-id="6cfec-141">type</span></span> | <span data-ttu-id="6cfec-142">OmsAgentForLinux</span><span class="sxs-lookup"><span data-stu-id="6cfec-142">OmsAgentForLinux</span></span> |
| <span data-ttu-id="6cfec-143">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="6cfec-143">typeHandlerVersion</span></span> | <span data-ttu-id="6cfec-144">1.4</span><span class="sxs-lookup"><span data-stu-id="6cfec-144">1.4</span></span> |
| <span data-ttu-id="6cfec-145">ID pracovního prostoru (např.)</span><span class="sxs-lookup"><span data-stu-id="6cfec-145">workspaceId (e.g)</span></span> | <span data-ttu-id="6cfec-146">6f680a37-00c6-41C7-a93f-1437e3462574</span><span class="sxs-lookup"><span data-stu-id="6cfec-146">6f680a37-00c6-41c7-a93f-1437e3462574</span></span> |
| <span data-ttu-id="6cfec-147">workspaceKey (např.)</span><span class="sxs-lookup"><span data-stu-id="6cfec-147">workspaceKey (e.g)</span></span> | <span data-ttu-id="6cfec-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ ==</span><span class="sxs-lookup"><span data-stu-id="6cfec-148">z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ==</span></span> |


## <a name="template-deployment"></a><span data-ttu-id="6cfec-149">Nasazení šablon</span><span class="sxs-lookup"><span data-stu-id="6cfec-149">Template deployment</span></span>

<span data-ttu-id="6cfec-150">Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6cfec-150">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="6cfec-151">Šablony jsou ideální, pokud nasazujete jednu nebo více virtuálních počítačů, které vyžadují konfiguraci nasazení post jako je registrace k OMS.</span><span class="sxs-lookup"><span data-stu-id="6cfec-151">Templates are ideal when deploying one or more virtual machines that require post deployment configuration such as onboarding to OMS.</span></span> <span data-ttu-id="6cfec-152">Ukázka šablonu Resource Manager, která obsahuje rozšíření virtuálního počítače agenta OMS naleznete na [Azure rychlý Start Galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span><span class="sxs-lookup"><span data-stu-id="6cfec-152">A sample Resource Manager template that includes the OMS Agent VM extension can be found on the [Azure Quick Start Gallery](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm).</span></span> 

<span data-ttu-id="6cfec-153">Konfigurace JSON pro rozšíření virtuálního počítače můžete vnořit prostředek virtuálního počítače nebo umístěn na kořenový server WSUS nebo nejvyšší úrovně šablony JSON Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6cfec-153">The JSON configuration for a virtual machine extension can be nested inside the virtual machine resource, or placed at the root or top level of a Resource Manager JSON template.</span></span> <span data-ttu-id="6cfec-154">Umístění konfigurace JSON ovlivňuje hodnota název prostředku a typem.</span><span class="sxs-lookup"><span data-stu-id="6cfec-154">The placement of the JSON configuration affects the value of the resource name and type.</span></span> <span data-ttu-id="6cfec-155">Další informace najdete v tématu [nastavte název a typ pro podřízené prostředky](../../azure-resource-manager/resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="6cfec-155">For more information, see [Set name and type for child resources](../../azure-resource-manager/resource-manager-template-child-resource.md).</span></span> 

<span data-ttu-id="6cfec-156">V následujícím příkladu se předpokládá, že je rozšíření OMS vnořit prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6cfec-156">The following example assumes the OMS extension is nested inside the virtual machine resource.</span></span> <span data-ttu-id="6cfec-157">Při vnoření rozšíření prostředků, JSON je umístěn v `"resources": []` objektu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6cfec-157">When nesting the extension resource, the JSON is placed in the `"resources": []` object of the virtual machine.</span></span>

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

<span data-ttu-id="6cfec-158">Při vkládání rozšíření JSON v kořenovém adresáři šablony, názvu prostředku obsahuje odkaz na nadřazený virtuální počítač a typ odráží vnořené konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6cfec-158">When placing the extension JSON at the root of the template, the resource name includes a reference to the parent virtual machine, and the type reflects the nested configuration.</span></span>  

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

## <a name="azure-cli-deployment"></a><span data-ttu-id="6cfec-159">Nasazení Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6cfec-159">Azure CLI deployment</span></span>

<span data-ttu-id="6cfec-160">Rozhraní příkazového řádku Azure můžete použít k nasazení rozšíření virtuálního počítače agenta OMS na existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6cfec-160">The Azure CLI can be used to deploy the OMS Agent VM extension to an existing virtual machine.</span></span> <span data-ttu-id="6cfec-161">Nahraďte klíč OMS a OMS ID s těmi, která z pracovního prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="6cfec-161">Replace the OMS key and OMS ID with those from your OMS workspace.</span></span> 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="6cfec-162">Řešení potíží a podpora</span><span class="sxs-lookup"><span data-stu-id="6cfec-162">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="6cfec-163">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6cfec-163">Troubleshoot</span></span>

<span data-ttu-id="6cfec-164">Z portálu Azure a pomocí rozhraní příkazového řádku Azure je možné načíst data o stavu nasazení rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6cfec-164">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure CLI.</span></span> <span data-ttu-id="6cfec-165">Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6cfec-165">To see the deployment state of extensions for a given VM, run the following command using the Azure CLI.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="6cfec-166">Výstupu spuštění rozšíření je zaznamenána do následujícího souboru:</span><span class="sxs-lookup"><span data-stu-id="6cfec-166">Extension execution output is logged to the following file:</span></span>

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a><span data-ttu-id="6cfec-167">Kódy chyb a jejich významů</span><span class="sxs-lookup"><span data-stu-id="6cfec-167">Error codes and their meanings</span></span>

| <span data-ttu-id="6cfec-168">Kód chyby</span><span class="sxs-lookup"><span data-stu-id="6cfec-168">Error Code</span></span> | <span data-ttu-id="6cfec-169">Význam</span><span class="sxs-lookup"><span data-stu-id="6cfec-169">Meaning</span></span> | <span data-ttu-id="6cfec-170">Možné akce</span><span class="sxs-lookup"><span data-stu-id="6cfec-170">Possible Action</span></span> |
| :---: | --- | --- |
| <span data-ttu-id="6cfec-171">10</span><span class="sxs-lookup"><span data-stu-id="6cfec-171">10</span></span> | <span data-ttu-id="6cfec-172">Virtuální počítač je již připojen k pracovnímu prostoru OMS</span><span class="sxs-lookup"><span data-stu-id="6cfec-172">VM is already connected to an OMS workspace</span></span> | <span data-ttu-id="6cfec-173">Připojit virtuální počítač do pracovního prostoru, určená ve schématu rozšíření, nastavena na hodnotu false v nastavení veřejných stopOnMultipleConnections nebo odeberte tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6cfec-173">To connect the VM to the workspace specified in the extension schema, set stopOnMultipleConnections to false in public settings or remove this property.</span></span> <span data-ttu-id="6cfec-174">Tento virtuální počítač získá účtován pro každý pracovní prostor po připojení.</span><span class="sxs-lookup"><span data-stu-id="6cfec-174">This VM gets billed once for each workspace it is connected to.</span></span> |
| <span data-ttu-id="6cfec-175">11</span><span class="sxs-lookup"><span data-stu-id="6cfec-175">11</span></span> | <span data-ttu-id="6cfec-176">Neplatná konfigurace poskytuje rozšíření</span><span class="sxs-lookup"><span data-stu-id="6cfec-176">Invalid config provided to the extension</span></span> | <span data-ttu-id="6cfec-177">Postupujte podle předchozích ukázkách nastavit všechny hodnoty vlastností nezbytné pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="6cfec-177">Follow the preceding examples to set all property values necessary for deployment.</span></span> |
| <span data-ttu-id="6cfec-178">12</span><span class="sxs-lookup"><span data-stu-id="6cfec-178">12</span></span> | <span data-ttu-id="6cfec-179">Správce balíčků dpkg je uzamčena.</span><span class="sxs-lookup"><span data-stu-id="6cfec-179">The dpkg package manager is locked</span></span> | <span data-ttu-id="6cfec-180">Zajistěte, aby všechny dpkg operace aktualizace na počítači dokončení a akci opakujte.</span><span class="sxs-lookup"><span data-stu-id="6cfec-180">Make sure all dpkg update operations on the machine have finished and retry.</span></span> |
| <span data-ttu-id="6cfec-181">20</span><span class="sxs-lookup"><span data-stu-id="6cfec-181">20</span></span> | <span data-ttu-id="6cfec-182">Povolit názvem předčasně</span><span class="sxs-lookup"><span data-stu-id="6cfec-182">Enable called prematurely</span></span> | <span data-ttu-id="6cfec-183">[Aktualizovat Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) na nejnovější dostupnou verzi.</span><span class="sxs-lookup"><span data-stu-id="6cfec-183">[Update the Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) to the latest available version.</span></span> |
| <span data-ttu-id="6cfec-184">51</span><span class="sxs-lookup"><span data-stu-id="6cfec-184">51</span></span> | <span data-ttu-id="6cfec-185">Toto rozšíření není podporována na operační systém Virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6cfec-185">This extension is not supported on the VM's operation system</span></span> | |
| <span data-ttu-id="6cfec-186">55</span><span class="sxs-lookup"><span data-stu-id="6cfec-186">55</span></span> | <span data-ttu-id="6cfec-187">Nelze připojit ke službě Microsoft Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="6cfec-187">Cannot connect to the Microsoft Operations Management Suite service</span></span> | <span data-ttu-id="6cfec-188">Zkontrolujte, jestli systém má přístup k Internetu nebo že bylo zadáno platný proxy server HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cfec-188">Check that the system either has Internet access, or that a valid HTTP proxy has been provided.</span></span> <span data-ttu-id="6cfec-189">Kromě toho zkontrolujte správnost ID pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="6cfec-189">Additionally, check the correctness of the workspace ID.</span></span> |

<span data-ttu-id="6cfec-190">Další informace o odstraňování potíží naleznete na [Průvodce odstraňováním potíží OMS agenta pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span><span class="sxs-lookup"><span data-stu-id="6cfec-190">Additional troubleshooting information can be found on the [OMS-Agent-for-Linux Troubleshooting Guide](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).</span></span>

### <a name="support"></a><span data-ttu-id="6cfec-191">Podpora</span><span class="sxs-lookup"><span data-stu-id="6cfec-191">Support</span></span>

<span data-ttu-id="6cfec-192">Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="6cfec-192">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="6cfec-193">Alternativně můžete soubor incidentu podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="6cfec-193">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="6cfec-194">Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory.</span><span class="sxs-lookup"><span data-stu-id="6cfec-194">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="6cfec-195">Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="6cfec-195">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
