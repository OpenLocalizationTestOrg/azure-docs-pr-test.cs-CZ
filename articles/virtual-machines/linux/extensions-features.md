---
title: "Rozšíření virtuálního počítače a funkce pro Linux | Microsoft Docs"
description: "Zjistěte, jaká rozšíření jsou k dispozici pro virtuální počítače Azure, seskupené podle co se poskytují nebo zvýšit."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: 8a5b39351f665c51ae7d83f755329e54ff3cf786
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a><span data-ttu-id="b920d-103">Rozšíření virtuálního počítače a funkce pro Linux</span><span class="sxs-lookup"><span data-stu-id="b920d-103">Virtual machine extensions and features for Linux</span></span>

<span data-ttu-id="b920d-104">Rozšíření virtuálního počítače Azure se malých aplikacích, které poskytují konfiguraci a automatizaci úloh po nasazení na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="b920d-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="b920d-105">Například pokud virtuální počítač vyžaduje instalace softwaru, ochrana proti virům nebo Docker konfigurace, rozšíření virtuálního počítače můžete použít k dokončení těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="b920d-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used to complete these tasks.</span></span> <span data-ttu-id="b920d-106">Rozšíření virtuálního počítače Azure můžete spustit pomocí rozhraní příkazového řádku Azure, prostředí PowerShell, šablony Azure Resource Manager a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b920d-106">Azure VM extensions can be run using the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal.</span></span> <span data-ttu-id="b920d-107">Rozšíření můžete dodávat s nové nasazení virtuálního počítače nebo spouštění všechny existující systém.</span><span class="sxs-lookup"><span data-stu-id="b920d-107">Extensions can be bundled with a new virtual machine deployment, or run against any existing system.</span></span>

<span data-ttu-id="b920d-108">Tento dokument obsahuje přehled rozšíření virtuálních počítačů, požadavků na používání rozšíření virtuálního počítače Azure, a pokyny o tom, jak zjistit, spravovat a odeberte rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-108">This document provides an overview of VM extensions, prerequisites for using Azure VM extensions, and guidance on how to detect, manage, and remove VM extensions.</span></span> <span data-ttu-id="b920d-109">Tento dokument obsahuje zobecněný informace, protože mnoho rozšíření virtuálního počítače nejsou k dispozici, každý s konfigurací potenciálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="b920d-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="b920d-110">Podrobnosti o konkrétní rozšíření najdete v každý dokument specifické pro jednotlivé rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b920d-110">Extension-specific details can be found in each document specific to the individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="b920d-111">Případy použití a ukázky</span><span class="sxs-lookup"><span data-stu-id="b920d-111">Use cases and samples</span></span>

<span data-ttu-id="b920d-112">Jsou k dispozici několik různých rozšíření virtuálního počítače Azure, každý s konkrétní případ použití.</span><span class="sxs-lookup"><span data-stu-id="b920d-112">Several different Azure VM extensions are available, each with a specific use case.</span></span> <span data-ttu-id="b920d-113">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="b920d-113">Some examples are:</span></span>

- <span data-ttu-id="b920d-114">Použít PowerShell požadovaná stav konfigurace virtuálního počítače pomocí rozšíření DSC pro Linux.</span><span class="sxs-lookup"><span data-stu-id="b920d-114">Apply PowerShell Desired State configurations to a virtual machine using the DSC extension for Linux.</span></span> <span data-ttu-id="b920d-115">Další informace najdete v tématu [stavu Azure požadovaná konfigurace rozšíření](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span><span class="sxs-lookup"><span data-stu-id="b920d-115">For more information, see [Azure Desired State configuration extension](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span></span>
- <span data-ttu-id="b920d-116">Konfigurace monitorování virtuálního počítače pomocí rozšíření Microsoft Monitoring Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-116">Configure monitoring of a virtual machine with the Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="b920d-117">Další informace najdete v tématu [postupy k monitorování virtuálního počítače s Linuxem](tutorial-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="b920d-117">For more information, see [How to monitor a Linux VM](tutorial-monitoring.md).</span></span>
- <span data-ttu-id="b920d-118">Konfigurace sledování infrastruktury Azure s příponou Datadog.</span><span class="sxs-lookup"><span data-stu-id="b920d-118">Configure monitoring of your Azure infrastructure with the Datadog extension.</span></span> <span data-ttu-id="b920d-119">Další informace najdete v tématu [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="b920d-119">For more information, see the [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="b920d-120">Konfigurace hostitele Docker na virtuální počítač Azure pomocí rozšíření Docker virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-120">Configure a Docker host on an Azure virtual machine using the Docker VM extension.</span></span> <span data-ttu-id="b920d-121">Další informace najdete v tématu [rozšíření virtuálního počítače Docker](dockerextension.md).</span><span class="sxs-lookup"><span data-stu-id="b920d-121">For more information, see [Docker VM extension](dockerextension.md).</span></span>

<span data-ttu-id="b920d-122">Kromě rozšíření specifické pro proces rozšíření vlastních skriptů je k dispozici pro virtuální počítače Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="b920d-122">In addition to process-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="b920d-123">Rozšíření vlastních skriptů pro Linux umožňuje všech skriptů Bash ke spuštění na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="b920d-123">The Custom Script extension for Linux allows any Bash script to be run on a virtual machine.</span></span> <span data-ttu-id="b920d-124">Vlastní skripty jsou užitečné pro návrh Azure nasazení, které vyžadují konfiguraci nad rámec jaké nativní Azure nástrojů může poskytnout.</span><span class="sxs-lookup"><span data-stu-id="b920d-124">Custom scripts are useful for designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="b920d-125">Další informace najdete v tématu [rozšíření skriptů vlastní virtuálních počítačů Linux](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="b920d-125">For more information, see [Linux VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="b920d-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b920d-126">Prerequisites</span></span>

<span data-ttu-id="b920d-127">Každé rozšíření virtuálního počítače může mít vlastní sadu požadavků.</span><span class="sxs-lookup"><span data-stu-id="b920d-127">Each virtual machine extension might have its own set of prerequisites.</span></span> <span data-ttu-id="b920d-128">Například rozšíření virtuálního počítače Docker má předpokladem podporované distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="b920d-128">For instance, the Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="b920d-129">Požadavky jednotlivých rozšíření jsou podrobně popsané v dokumentaci k konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b920d-129">Requirements of individual extensions are detailed in the extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="b920d-130">Agent virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="b920d-130">Azure VM agent</span></span>

<span data-ttu-id="b920d-131">Agent virtuálního počítače Azure spravuje interakce mezi virtuální počítač Azure a kontroleru prostředků infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="b920d-131">The Azure VM agent manages interactions between an Azure virtual machine and the Azure fabric controller.</span></span> <span data-ttu-id="b920d-132">Agent virtuálního počítače je zodpovědná za funkční aspekty nasazení a správa virtuálních počítačích Azure, včetně spuštění rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-132">The VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="b920d-133">Agent virtuálního počítače Azure je předinstalován v Azure Marketplace bitové kopie a může být nainstalován ručně na podporovaných operačních systémů.</span><span class="sxs-lookup"><span data-stu-id="b920d-133">The Azure VM agent is preinstalled on Azure Marketplace images and can be installed manually on supported operating systems.</span></span>

<span data-ttu-id="b920d-134">Informace o podporovaných operačních systémů a pokyny k instalaci najdete v tématu [agent virtuálního počítače Azure](../windows/classic/agents-and-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="b920d-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](../windows/classic/agents-and-extensions.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="b920d-135">Zjistit rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b920d-135">Discover VM extensions</span></span>

<span data-ttu-id="b920d-136">Mnoho různých rozšíření virtuálního počítače jsou k dispozici pro použití s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="b920d-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="b920d-137">Pokud chcete zobrazit úplný seznam, spusťte následující příkaz pomocí Azure CLI, nahraďte umístění Příklad umístění podle vaší volby.</span><span class="sxs-lookup"><span data-stu-id="b920d-137">To see a complete list, run the following command with the Azure CLI, replacing the example location with the location of your choice.</span></span>

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a><span data-ttu-id="b920d-138">Spuštění rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b920d-138">Run VM extensions</span></span>

<span data-ttu-id="b920d-139">Rozšíření virtuálního počítače Azure můžete spustit na existující virtuální počítače, které jsou užitečné, když potřebujete udělat změny konfigurace nebo obnovit připojení již nasazené virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-139">Azure virtual machine extensions can be run on existing virtual machines, which are useful when you need to make configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="b920d-140">Rozšíření virtuálního počítače můžete také dodávat s nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b920d-140">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="b920d-141">Virtuální počítače Azure můžete pomocí rozšíření šablony Resource Manageru, nasazení a nakonfigurování bez zásahu po nasazení.</span><span class="sxs-lookup"><span data-stu-id="b920d-141">By using extensions with Resource Manager templates, Azure virtual machines can be deployed and configured without post-deployment intervention.</span></span>

<span data-ttu-id="b920d-142">Tyto metody slouží ke spuštění rozšíření stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-142">The following methods can be used to run an extension against an existing virtual machine.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="b920d-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b920d-143">Azure CLI</span></span>

<span data-ttu-id="b920d-144">Pro existující virtuální počítač můžete spouštět rozšíření virtuálního počítače Azure pomocí `az vm extension set` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b920d-144">Azure virtual machine extensions can be run against an existing virtual machine by using the `az vm extension set` command.</span></span> <span data-ttu-id="b920d-145">Tento příklad spouští rozšíření vlastních skriptů virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="b920d-145">This example runs the custom script extension against a virtual machine.</span></span>

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

<span data-ttu-id="b920d-146">Tento skript vytvoří výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="b920d-146">The script produces output similar to the following text:</span></span>

```azurecli
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a><span data-ttu-id="b920d-147">portál Azure</span><span class="sxs-lookup"><span data-stu-id="b920d-147">Azure portal</span></span>

<span data-ttu-id="b920d-148">Rozšíření virtuálního počítače je použít pro existující virtuální počítač prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b920d-148">VM extensions can be applied to an existing virtual machine through the Azure portal.</span></span> <span data-ttu-id="b920d-149">To pokud chcete udělat, vyberte virtuální počítač, vyberte **rozšíření**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b920d-149">To do so, select the virtual machine, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="b920d-150">Vyberte požadované rozšíření ze seznamu dostupných rozšíření a postupujte podle pokynů v průvodci.</span><span class="sxs-lookup"><span data-stu-id="b920d-150">Select the extension you want from the list of available extensions and follow the instructions in the wizard.</span></span>

<span data-ttu-id="b920d-151">Následující obrázek znázorňuje instalaci rozšíření Linux vlastních skriptů na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b920d-151">The following image shows the installation of the Linux Custom Script extension from the Azure portal.</span></span>

![Instalace rozšíření vlastních skriptů](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="b920d-153">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="b920d-153">Azure Resource Manager templates</span></span>

<span data-ttu-id="b920d-154">Rozšíření virtuálních počítačů můžete přidat do šablony Azure Resource Manager a provést při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="b920d-154">VM extensions can be added to an Azure Resource Manager template and executed with the deployment of the template.</span></span> <span data-ttu-id="b920d-155">Když nasadíte rozšíření pomocí šablony, můžete vytvořit plně nakonfigurované Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="b920d-155">When you deploy an extension with a template, you can create fully configured Azure deployments.</span></span> <span data-ttu-id="b920d-156">Například následující kód JSON je převzat ze šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="b920d-156">For example, the following JSON is taken from a Resource Manager template.</span></span> <span data-ttu-id="b920d-157">Šablona nasadí sadu Vyrovnávání zatížení sítě virtuálních počítačů a Azure SQL database a potom nainstaluje aplikace .NET Core na každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b920d-157">The template deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="b920d-158">Rozšíření virtuálního počítače má na starosti instalace softwaru.</span><span class="sxs-lookup"><span data-stu-id="b920d-158">The VM extension takes care of the software installation.</span></span>

<span data-ttu-id="b920d-159">Další informace najdete v tématu kompletní [šablony Resource Manageru](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="b920d-159">For more information, see the full [Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

<span data-ttu-id="b920d-160">Další informace najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span><span class="sxs-lookup"><span data-stu-id="b920d-160">For more information, see [Authoring Azure Resource Manager templates](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="b920d-161">Zabezpečení dat rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b920d-161">Secure VM extension data</span></span>

<span data-ttu-id="b920d-162">Když používáte rozšíření virtuálního počítače, může být nutné zahrnovat citlivé informace, jako je například přihlašovací údaje, názvy účtů úložiště a přístupových klíčů k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b920d-162">When you're running a VM extension, it may be necessary to include sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="b920d-163">Mnoho rozšíření virtuálního počítače zahrnují chráněné konfigurace, která data šifruje a dešifruje ji pouze uvnitř cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-163">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside the target virtual machine.</span></span> <span data-ttu-id="b920d-164">Každé rozšíření má schéma konkrétní chráněné konfigurace a každý je podrobně popsaná v dokumentaci konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b920d-164">Each extension has a specific protected configuration schema, and each is detailed in extension-specific documentation.</span></span>

<span data-ttu-id="b920d-165">Následující příklad ukazuje instanci rozšíření vlastních skriptů pro Linux.</span><span class="sxs-lookup"><span data-stu-id="b920d-165">The following example shows an instance of the Custom Script extension for Linux.</span></span> <span data-ttu-id="b920d-166">Všimněte si, že příkaz k provedení obsahuje sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b920d-166">Notice that the command to execute includes a set of credentials.</span></span> <span data-ttu-id="b920d-167">V tomto příkladu spuštění příkazu se šifrovat nebude.</span><span class="sxs-lookup"><span data-stu-id="b920d-167">In this example, the command to execute will not be encrypted.</span></span>


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

<span data-ttu-id="b920d-168">Přesun **příkaz k provedení** vlastnost, která má **chráněné** konfigurace zabezpečuje provádění řetězec.</span><span class="sxs-lookup"><span data-stu-id="b920d-168">Moving the **command to execute** property to the **protected** configuration secures the execution string.</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="b920d-169">Řešení potíží s rozšířeními virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b920d-169">Troubleshoot VM extensions</span></span>

<span data-ttu-id="b920d-170">Každé rozšíření virtuálního počítače může mít při řešení potíží konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b920d-170">Each VM extension may have troubleshooting steps specific to the extension.</span></span> <span data-ttu-id="b920d-171">Například pokud používáte rozšíření vlastních skriptů, podrobnosti provádění skriptu najdete místně na virtuálním počítači, na kterém byl rozšíření spustit.</span><span class="sxs-lookup"><span data-stu-id="b920d-171">For example, when you're using the Custom Script extension, script execution details can be found locally on the virtual machine on which the extension was run.</span></span> <span data-ttu-id="b920d-172">Kroky řešení potíží konkrétní rozšíření jsou podrobně popsané v dokumentaci k konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b920d-172">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="b920d-173">Následující kroky řešení potíží použít na všechny přípony virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-173">The following troubleshooting steps apply to all virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="b920d-174">Zobrazit stav rozšíření</span><span class="sxs-lookup"><span data-stu-id="b920d-174">View extension status</span></span>

<span data-ttu-id="b920d-175">Po virtuálního počítače rozšíření spustit na virtuálním počítači, pomocí následujícího příkazu příkazového řádku Azure CLI vrátí stav rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b920d-175">After a virtual machine extension has been run against a virtual machine, use the following Azure CLI command to return extension status.</span></span> <span data-ttu-id="b920d-176">Názvy parametrů příkladu nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b920d-176">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="b920d-177">Výstup vypadá následující text:</span><span class="sxs-lookup"><span data-stu-id="b920d-177">The output looks like the following text:</span></span>

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

<span data-ttu-id="b920d-178">Stav spuštění rozšíření možné také najít na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b920d-178">Extension execution status can also be found in the Azure portal.</span></span> <span data-ttu-id="b920d-179">Chcete-li zobrazit stav rozšíření, vyberte virtuální počítač, zvolte **rozšíření**a vyberte požadované rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b920d-179">To view the status of an extension, select the virtual machine, choose **Extensions**, and select the desired extension.</span></span>

### <a name="rerun-a-vm-extension"></a><span data-ttu-id="b920d-180">Znovu spustit, rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b920d-180">Rerun a VM extension</span></span>

<span data-ttu-id="b920d-181">Můžou nastat případy, ve kterých musí být znovu rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b920d-181">There may be cases in which a virtual machine extension needs to be rerun.</span></span> <span data-ttu-id="b920d-182">Rozšíření může znovu odebrat, a pak znovu spustit rozšíření s metodu provádění podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="b920d-182">You can rerun an extension by removing it, and then rerunning the extension with an execution method of your choice.</span></span> <span data-ttu-id="b920d-183">Chcete-li odebrat rozšíření, spusťte následující příkaz pomocí Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b920d-183">To remove an extension, run the following command with the Azure CLI.</span></span> <span data-ttu-id="b920d-184">Názvy parametrů příkladu nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b920d-184">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

<span data-ttu-id="b920d-185">Rozšíření můžete odebrat pomocí následujících kroků na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="b920d-185">You can remove an extension by using the following steps in the Azure portal:</span></span>

1. <span data-ttu-id="b920d-186">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b920d-186">Select a virtual machine.</span></span>
2. <span data-ttu-id="b920d-187">Zvolte **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="b920d-187">Choose **Extensions**.</span></span>
3. <span data-ttu-id="b920d-188">Vyberte požadované rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b920d-188">Select the desired extension.</span></span>
4. <span data-ttu-id="b920d-189">Zvolte **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="b920d-189">Choose **Uninstall**.</span></span>

## <a name="common-vm-extension-reference"></a><span data-ttu-id="b920d-190">Běžné odkaz na rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b920d-190">Common VM extension reference</span></span>
| <span data-ttu-id="b920d-191">Název rozšíření</span><span class="sxs-lookup"><span data-stu-id="b920d-191">Extension name</span></span> | <span data-ttu-id="b920d-192">Popis</span><span class="sxs-lookup"><span data-stu-id="b920d-192">Description</span></span> | <span data-ttu-id="b920d-193">Další informace</span><span class="sxs-lookup"><span data-stu-id="b920d-193">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b920d-194">Rozšíření vlastních skriptů pro Linux</span><span class="sxs-lookup"><span data-stu-id="b920d-194">Custom Script extension for Linux</span></span> |<span data-ttu-id="b920d-195">Spouštění skriptů na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="b920d-195">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="b920d-196">Rozšíření vlastních skriptů pro Linux</span><span class="sxs-lookup"><span data-stu-id="b920d-196">Custom Script extension for Linux</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="b920d-197">Rozšíření docker</span><span class="sxs-lookup"><span data-stu-id="b920d-197">Docker extension</span></span> |<span data-ttu-id="b920d-198">Instalace démona Docker pro podporu vzdálených příkazů Docker.</span><span class="sxs-lookup"><span data-stu-id="b920d-198">Install the Docker daemon to support remote Docker commands.</span></span> |[<span data-ttu-id="b920d-199">Rozšíření Docker VM</span><span class="sxs-lookup"><span data-stu-id="b920d-199">Docker VM extension</span></span>](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="b920d-200">Rozšíření přístupu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b920d-200">VM Access extension</span></span> |<span data-ttu-id="b920d-201">Znovu získat přístup do virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="b920d-201">Regain access to an Azure virtual machine</span></span> |[<span data-ttu-id="b920d-202">Rozšíření přístupu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b920d-202">VM Access extension</span></span>](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| <span data-ttu-id="b920d-203">Rozšíření Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="b920d-203">Azure Diagnostics extension</span></span> |<span data-ttu-id="b920d-204">Správa Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="b920d-204">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="b920d-205">Rozšíření Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="b920d-205">Azure Diagnostics extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="b920d-206">Rozšíření přístup virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="b920d-206">Azure VM Access extension</span></span> |<span data-ttu-id="b920d-207">Spravovat uživatele a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="b920d-207">Manage users and credentials</span></span> |[<span data-ttu-id="b920d-208">Rozšíření pro přístup virtuálních počítačů pro Linux</span><span class="sxs-lookup"><span data-stu-id="b920d-208">VM Access extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
