---
title: "aaaVirtual počítače rozšíření a funkce pro Linux | Microsoft Docs"
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
ms.openlocfilehash: e0d2ce794c76815ccc6743e8788ee5d9d931e9a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a><span data-ttu-id="e4274-103">Rozšíření virtuálního počítače a funkce pro Linux</span><span class="sxs-lookup"><span data-stu-id="e4274-103">Virtual machine extensions and features for Linux</span></span>

<span data-ttu-id="e4274-104">Rozšíření virtuálního počítače Azure se malých aplikacích, které poskytují konfiguraci a automatizaci úloh po nasazení na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="e4274-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="e4274-105">Například pokud virtuální počítač vyžaduje instalace softwaru, ochrana proti virům nebo Docker konfigurace, rozšíření virtuálního počítače může být použité toocomplete tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="e4274-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used toocomplete these tasks.</span></span> <span data-ttu-id="e4274-106">Rozšíření virtuálního počítače Azure lze spouštět s využitím hello rozhraní příkazového řádku Azure, prostředí PowerShell, šablon Azure Resource Manageru a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e4274-106">Azure VM extensions can be run using hello Azure CLI, PowerShell, Azure Resource Manager templates, and hello Azure portal.</span></span> <span data-ttu-id="e4274-107">Rozšíření můžete dodávat s nové nasazení virtuálního počítače nebo spouštění všechny existující systém.</span><span class="sxs-lookup"><span data-stu-id="e4274-107">Extensions can be bundled with a new virtual machine deployment, or run against any existing system.</span></span>

<span data-ttu-id="e4274-108">Tento dokument obsahuje přehled rozšíření virtuálního počítače, požadavky na tom, jak spravovat toodetect a odeberte rozšíření virtuálního počítače pomocí rozšíření virtuálního počítače Azure a pokyny.</span><span class="sxs-lookup"><span data-stu-id="e4274-108">This document provides an overview of VM extensions, prerequisites for using Azure VM extensions, and guidance on how toodetect, manage, and remove VM extensions.</span></span> <span data-ttu-id="e4274-109">Tento dokument obsahuje zobecněný informace, protože mnoho rozšíření virtuálního počítače nejsou k dispozici, každý s konfigurací potenciálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="e4274-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="e4274-110">Podrobnosti o konkrétní rozšíření najdete v každé jednotlivé rozšíření konkrétní toohello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e4274-110">Extension-specific details can be found in each document specific toohello individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="e4274-111">Případy použití a ukázky</span><span class="sxs-lookup"><span data-stu-id="e4274-111">Use cases and samples</span></span>

<span data-ttu-id="e4274-112">Jsou k dispozici několik různých rozšíření virtuálního počítače Azure, každý s konkrétní případ použití.</span><span class="sxs-lookup"><span data-stu-id="e4274-112">Several different Azure VM extensions are available, each with a specific use case.</span></span> <span data-ttu-id="e4274-113">Tady je několik příkladů:</span><span class="sxs-lookup"><span data-stu-id="e4274-113">Some examples are:</span></span>

- <span data-ttu-id="e4274-114">Použití prostředí PowerShell požadovaná stav konfigurace tooa virtuálního počítače pomocí hello rozšíření DSC pro Linux.</span><span class="sxs-lookup"><span data-stu-id="e4274-114">Apply PowerShell Desired State configurations tooa virtual machine using hello DSC extension for Linux.</span></span> <span data-ttu-id="e4274-115">Další informace najdete v tématu [stavu Azure požadovaná konfigurace rozšíření](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span><span class="sxs-lookup"><span data-stu-id="e4274-115">For more information, see [Azure Desired State configuration extension](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span></span>
- <span data-ttu-id="e4274-116">Konfigurace monitorování virtuálního počítače s hello rozšíření Microsoft Monitoring Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4274-116">Configure monitoring of a virtual machine with hello Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="e4274-117">Další informace najdete v tématu [jak toomonitor virtuálního počítače s Linuxem](tutorial-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="e4274-117">For more information, see [How toomonitor a Linux VM](tutorial-monitoring.md).</span></span>
- <span data-ttu-id="e4274-118">Konfigurace sledování infrastruktury Azure s hello Datadog rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e4274-118">Configure monitoring of your Azure infrastructure with hello Datadog extension.</span></span> <span data-ttu-id="e4274-119">Další informace najdete v tématu hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="e4274-119">For more information, see hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="e4274-120">Konfigurace hostitele Docker na virtuální počítač Azure pomocí rozšíření virtuálního počítače Docker hello.</span><span class="sxs-lookup"><span data-stu-id="e4274-120">Configure a Docker host on an Azure virtual machine using hello Docker VM extension.</span></span> <span data-ttu-id="e4274-121">Další informace najdete v tématu [rozšíření virtuálního počítače Docker](dockerextension.md).</span><span class="sxs-lookup"><span data-stu-id="e4274-121">For more information, see [Docker VM extension](dockerextension.md).</span></span>

<span data-ttu-id="e4274-122">Kromě toho tooprocess konkrétní rozšíření, rozšíření vlastních skriptů je k dispozici pro virtuální počítače Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="e4274-122">In addition tooprocess-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="e4274-123">Hello rozšíření vlastních skriptů pro Linux umožňuje žádné Bash toobe skript spustit na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e4274-123">hello Custom Script extension for Linux allows any Bash script toobe run on a virtual machine.</span></span> <span data-ttu-id="e4274-124">Vlastní skripty jsou užitečné pro návrh Azure nasazení, které vyžadují konfiguraci nad rámec jaké nativní Azure nástrojů může poskytnout.</span><span class="sxs-lookup"><span data-stu-id="e4274-124">Custom scripts are useful for designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="e4274-125">Další informace najdete v tématu [rozšíření skriptů vlastní virtuálních počítačů Linux](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="e4274-125">For more information, see [Linux VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="e4274-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e4274-126">Prerequisites</span></span>

<span data-ttu-id="e4274-127">Každé rozšíření virtuálního počítače může mít vlastní sadu požadavků.</span><span class="sxs-lookup"><span data-stu-id="e4274-127">Each virtual machine extension might have its own set of prerequisites.</span></span> <span data-ttu-id="e4274-128">Například hello rozšíření virtuálního počítače Docker má předpokladem podporované distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="e4274-128">For instance, hello Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="e4274-129">Požadavky jednotlivých rozšíření jsou podrobně popsané v dokumentaci pro konkrétní rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="e4274-129">Requirements of individual extensions are detailed in hello extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="e4274-130">Agent virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="e4274-130">Azure VM agent</span></span>

<span data-ttu-id="e4274-131">agent virtuálního počítače Azure Hello spravuje interakce mezi virtuální počítač Azure a prostředků infrastruktury Azure řadiče hello.</span><span class="sxs-lookup"><span data-stu-id="e4274-131">hello Azure VM agent manages interactions between an Azure virtual machine and hello Azure fabric controller.</span></span> <span data-ttu-id="e4274-132">agent virtuálního počítače Hello je zodpovědná za funkční aspekty nasazení a správa virtuálních počítačích Azure, včetně spuštění rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4274-132">hello VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="e4274-133">agent virtuálního počítače Azure Hello je předinstalován v Azure Marketplace bitové kopie a může být nainstalován ručně na podporovaných operačních systémů.</span><span class="sxs-lookup"><span data-stu-id="e4274-133">hello Azure VM agent is preinstalled on Azure Marketplace images and can be installed manually on supported operating systems.</span></span>

<span data-ttu-id="e4274-134">Informace o podporovaných operačních systémů a pokyny k instalaci najdete v tématu [agent virtuálního počítače Azure](../windows/classic/agents-and-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="e4274-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](../windows/classic/agents-and-extensions.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="e4274-135">Zjistit rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e4274-135">Discover VM extensions</span></span>

<span data-ttu-id="e4274-136">Mnoho různých rozšíření virtuálního počítače jsou k dispozici pro použití s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="e4274-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="e4274-137">toosee úplný seznam, spusťte následující příkaz s hello rozhraní příkazového řádku Azure, nahraďte hello Příklad umístění hello umístění podle vaší volby hello.</span><span class="sxs-lookup"><span data-stu-id="e4274-137">toosee a complete list, run hello following command with hello Azure CLI, replacing hello example location with hello location of your choice.</span></span>

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a><span data-ttu-id="e4274-138">Spuštění rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e4274-138">Run VM extensions</span></span>

<span data-ttu-id="e4274-139">Rozšíření virtuálního počítače Azure můžete spustit na existující virtuální počítače, které jsou užitečné, když potřebujete toomake změny konfigurace nebo obnovit připojení již nasazené virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4274-139">Azure virtual machine extensions can be run on existing virtual machines, which are useful when you need toomake configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="e4274-140">Rozšíření virtuálního počítače můžete také dodávat s nasazení šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e4274-140">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="e4274-141">Virtuální počítače Azure můžete pomocí rozšíření šablony Resource Manageru, nasazení a nakonfigurování bez zásahu po nasazení.</span><span class="sxs-lookup"><span data-stu-id="e4274-141">By using extensions with Resource Manager templates, Azure virtual machines can be deployed and configured without post-deployment intervention.</span></span>

<span data-ttu-id="e4274-142">Hello následující metody se dá použít toorun rozšíření proti existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4274-142">hello following methods can be used toorun an extension against an existing virtual machine.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="e4274-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e4274-143">Azure CLI</span></span>

<span data-ttu-id="e4274-144">Pro existující virtuální počítač můžete spouštět rozšíření virtuálního počítače Azure pomocí hello `az vm extension set` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e4274-144">Azure virtual machine extensions can be run against an existing virtual machine by using hello `az vm extension set` command.</span></span> <span data-ttu-id="e4274-145">Tento příklad spouští rozšíření vlastních skriptů hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4274-145">This example runs hello custom script extension against a virtual machine.</span></span>

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

<span data-ttu-id="e4274-146">vytváří skript Hello výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="e4274-146">hello script produces output similar toohello following text:</span></span>

```azurecli
info:    Executing command vm extension set
+ Looking up hello VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a><span data-ttu-id="e4274-147">portál Azure</span><span class="sxs-lookup"><span data-stu-id="e4274-147">Azure portal</span></span>

<span data-ttu-id="e4274-148">Rozšíření virtuálního počítače může být použité tooan existujícího virtuálního počítače prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e4274-148">VM extensions can be applied tooan existing virtual machine through hello Azure portal.</span></span> <span data-ttu-id="e4274-149">toodo tedy vyberte hello virtuálního počítače, zvolte **rozšíření**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e4274-149">toodo so, select hello virtual machine, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="e4274-150">Vyberte rozšíření hello ze hello seznam dostupných rozšíření a postupujte podle pokynů hello v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="e4274-150">Select hello extension you want from hello list of available extensions and follow hello instructions in hello wizard.</span></span>

<span data-ttu-id="e4274-151">Hello následující obrázek znázorňuje instalaci hello hello rozšíření vlastních skriptů Linux z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e4274-151">hello following image shows hello installation of hello Linux Custom Script extension from hello Azure portal.</span></span>

![Instalace rozšíření vlastních skriptů](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="e4274-153">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e4274-153">Azure Resource Manager templates</span></span>

<span data-ttu-id="e4274-154">Rozšíření virtuálního počítače může být přidané tooan šablony Azure Resource Manageru a provést s hello nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="e4274-154">VM extensions can be added tooan Azure Resource Manager template and executed with hello deployment of hello template.</span></span> <span data-ttu-id="e4274-155">Když nasadíte rozšíření pomocí šablony, můžete vytvořit plně nakonfigurované Azure nasazení.</span><span class="sxs-lookup"><span data-stu-id="e4274-155">When you deploy an extension with a template, you can create fully configured Azure deployments.</span></span> <span data-ttu-id="e4274-156">Například následující JSON hello je převzat ze šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="e4274-156">For example, hello following JSON is taken from a Resource Manager template.</span></span> <span data-ttu-id="e4274-157">Šablona Hello nasadí sadu Vyrovnávání zatížení sítě virtuálních počítačů a Azure SQL database a potom nainstaluje aplikace .NET Core na každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e4274-157">hello template deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="e4274-158">Instalace softwaru hello postará Hello rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4274-158">hello VM extension takes care of hello software installation.</span></span>

<span data-ttu-id="e4274-159">Další informace najdete v tématu hello úplné [šablony Resource Manageru](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="e4274-159">For more information, see hello full [Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

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

<span data-ttu-id="e4274-160">Další informace najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span><span class="sxs-lookup"><span data-stu-id="e4274-160">For more information, see [Authoring Azure Resource Manager templates](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="e4274-161">Zabezpečení dat rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e4274-161">Secure VM extension data</span></span>

<span data-ttu-id="e4274-162">Když používáte rozšíření virtuálního počítače, může být nutné tooinclude citlivé informace, jako je například přihlašovací údaje, názvy účtů úložiště a přístupových klíčů k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e4274-162">When you're running a VM extension, it may be necessary tooinclude sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="e4274-163">Mnoho rozšíření virtuálního počítače zahrnují chráněné konfigurace, která data šifruje a dešifruje ji pouze uvnitř hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4274-163">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside hello target virtual machine.</span></span> <span data-ttu-id="e4274-164">Každé rozšíření má schéma konkrétní chráněné konfigurace a každý je podrobně popsaná v dokumentaci konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e4274-164">Each extension has a specific protected configuration schema, and each is detailed in extension-specific documentation.</span></span>

<span data-ttu-id="e4274-165">Hello následující příklad ukazuje instanci hello rozšíření vlastních skriptů pro Linux.</span><span class="sxs-lookup"><span data-stu-id="e4274-165">hello following example shows an instance of hello Custom Script extension for Linux.</span></span> <span data-ttu-id="e4274-166">Všimněte si, že tento příkaz tooexecute hello zahrnuje sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e4274-166">Notice that hello command tooexecute includes a set of credentials.</span></span> <span data-ttu-id="e4274-167">V tomto příkladu hello příkaz tooexecute se šifrovat nebude.</span><span class="sxs-lookup"><span data-stu-id="e4274-167">In this example, hello command tooexecute will not be encrypted.</span></span>


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

<span data-ttu-id="e4274-168">Přesunutí hello **příkaz tooexecute** vlastnost toohello **chráněné** konfigurace zabezpečuje hello provádění řetězec.</span><span class="sxs-lookup"><span data-stu-id="e4274-168">Moving hello **command tooexecute** property toohello **protected** configuration secures hello execution string.</span></span>

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

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="e4274-169">Řešení potíží s rozšířeními virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e4274-169">Troubleshoot VM extensions</span></span>

<span data-ttu-id="e4274-170">Každé rozšíření virtuálního počítače může mít řešení potíží s kroky konkrétní toohello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e4274-170">Each VM extension may have troubleshooting steps specific toohello extension.</span></span> <span data-ttu-id="e4274-171">Například pokud používáte rozšíření vlastních skriptů hello, podrobnosti provádění skriptu najdete místně na hello virtuálním počítači, na kterém byl spuštěn hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e4274-171">For example, when you're using hello Custom Script extension, script execution details can be found locally on hello virtual machine on which hello extension was run.</span></span> <span data-ttu-id="e4274-172">Kroky řešení potíží konkrétní rozšíření jsou podrobně popsané v dokumentaci k konkrétní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e4274-172">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="e4274-173">Hello následující kroky řešení potíží použít tooall rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4274-173">hello following troubleshooting steps apply tooall virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="e4274-174">Zobrazit stav rozšíření</span><span class="sxs-lookup"><span data-stu-id="e4274-174">View extension status</span></span>

<span data-ttu-id="e4274-175">Po spuštění rozšíření virtuálního počítače pro virtuální počítač, použijte následující stav rozšíření tooreturn příkazu příkazového řádku Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="e4274-175">After a virtual machine extension has been run against a virtual machine, use hello following Azure CLI command tooreturn extension status.</span></span> <span data-ttu-id="e4274-176">Názvy parametrů příkladu nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="e4274-176">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="e4274-177">výstup Hello vypadá hello následující text:</span><span class="sxs-lookup"><span data-stu-id="e4274-177">hello output looks like hello following text:</span></span>

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

<span data-ttu-id="e4274-178">Stav spuštění rozšíření naleznete také v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e4274-178">Extension execution status can also be found in hello Azure portal.</span></span> <span data-ttu-id="e4274-179">Stav hello tooview rozšíření, vyberte hello virtuálního počítače, zvolte **rozšíření**, a vyberte hello požadované rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e4274-179">tooview hello status of an extension, select hello virtual machine, choose **Extensions**, and select hello desired extension.</span></span>

### <a name="rerun-a-vm-extension"></a><span data-ttu-id="e4274-180">Znovu spustit, rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e4274-180">Rerun a VM extension</span></span>

<span data-ttu-id="e4274-181">Můžou nastat případy, ve kterých rozšíření virtuálního počítače potřebuje toobe spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="e4274-181">There may be cases in which a virtual machine extension needs toobe rerun.</span></span> <span data-ttu-id="e4274-182">Rozšíření může znovu odebrat, a pak znovu spustit hello rozšíření s metodu provádění podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="e4274-182">You can rerun an extension by removing it, and then rerunning hello extension with an execution method of your choice.</span></span> <span data-ttu-id="e4274-183">tooremove rozšíření, spusťte následující příkaz s hello rozhraní příkazového řádku Azure hello.</span><span class="sxs-lookup"><span data-stu-id="e4274-183">tooremove an extension, run hello following command with hello Azure CLI.</span></span> <span data-ttu-id="e4274-184">Názvy parametrů příkladu nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="e4274-184">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

<span data-ttu-id="e4274-185">Rozšíření můžete odebrat pomocí hello proveďte kroky v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e4274-185">You can remove an extension by using hello following steps in hello Azure portal:</span></span>

1. <span data-ttu-id="e4274-186">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e4274-186">Select a virtual machine.</span></span>
2. <span data-ttu-id="e4274-187">Zvolte **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="e4274-187">Choose **Extensions**.</span></span>
3. <span data-ttu-id="e4274-188">Vyberte hello požadované rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e4274-188">Select hello desired extension.</span></span>
4. <span data-ttu-id="e4274-189">Zvolte **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="e4274-189">Choose **Uninstall**.</span></span>

## <a name="common-vm-extension-reference"></a><span data-ttu-id="e4274-190">Běžné odkaz na rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e4274-190">Common VM extension reference</span></span>
| <span data-ttu-id="e4274-191">Název rozšíření</span><span class="sxs-lookup"><span data-stu-id="e4274-191">Extension name</span></span> | <span data-ttu-id="e4274-192">Popis</span><span class="sxs-lookup"><span data-stu-id="e4274-192">Description</span></span> | <span data-ttu-id="e4274-193">Další informace</span><span class="sxs-lookup"><span data-stu-id="e4274-193">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e4274-194">Rozšíření vlastních skriptů pro Linux</span><span class="sxs-lookup"><span data-stu-id="e4274-194">Custom Script extension for Linux</span></span> |<span data-ttu-id="e4274-195">Spouštění skriptů na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="e4274-195">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="e4274-196">Rozšíření vlastních skriptů pro Linux</span><span class="sxs-lookup"><span data-stu-id="e4274-196">Custom Script extension for Linux</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="e4274-197">Rozšíření docker</span><span class="sxs-lookup"><span data-stu-id="e4274-197">Docker extension</span></span> |<span data-ttu-id="e4274-198">Nainstalujte hello Docker démon toosupport vzdálených Docker příkazů.</span><span class="sxs-lookup"><span data-stu-id="e4274-198">Install hello Docker daemon toosupport remote Docker commands.</span></span> |[<span data-ttu-id="e4274-199">Rozšíření Docker VM</span><span class="sxs-lookup"><span data-stu-id="e4274-199">Docker VM extension</span></span>](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="e4274-200">Rozšíření přístupu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e4274-200">VM Access extension</span></span> |<span data-ttu-id="e4274-201">Obnoví přístup tooan virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="e4274-201">Regain access tooan Azure virtual machine</span></span> |[<span data-ttu-id="e4274-202">Rozšíření přístupu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e4274-202">VM Access extension</span></span>](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| <span data-ttu-id="e4274-203">Rozšíření Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="e4274-203">Azure Diagnostics extension</span></span> |<span data-ttu-id="e4274-204">Správa Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="e4274-204">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="e4274-205">Rozšíření Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="e4274-205">Azure Diagnostics extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="e4274-206">Rozšíření přístup virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="e4274-206">Azure VM Access extension</span></span> |<span data-ttu-id="e4274-207">Spravovat uživatele a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="e4274-207">Manage users and credentials</span></span> |[<span data-ttu-id="e4274-208">Rozšíření pro přístup virtuálních počítačů pro Linux</span><span class="sxs-lookup"><span data-stu-id="e4274-208">VM Access extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
