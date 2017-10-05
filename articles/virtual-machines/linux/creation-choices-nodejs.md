---
title: "Různé způsoby vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Další informace o různých způsobech vytvoření virtuálního počítače s Linuxem na platformě Azure, včetně odkazů na nástroje a kurzy pro jednotlivé metody."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 1eb90d44797d66f3e09811918ce5a7f4ad4287c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="48d68-103">Různé způsoby vytvoření virtuálního počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="48d68-103">Different ways to create a Linux virtual machine in Azure</span></span>
<span data-ttu-id="48d68-104">V Azure máte flexibilitu vytvoření virtuálního počítače s Linuxem pomocí nástrojů a pracovních postupů, které vám vyhovují.</span><span class="sxs-lookup"><span data-stu-id="48d68-104">You have the flexibility in Azure to create a Linux virtual machine (VM) using tools and workflows comfortable to you.</span></span> <span data-ttu-id="48d68-105">Tento článek shrnuje tyto rozdíly a příklady vytvoření virtuálních počítačů s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="48d68-105">This article summarizes these differences and examples for creating your Linux VMs.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="48d68-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="48d68-106">Azure CLI</span></span>
<span data-ttu-id="48d68-107">Virtuální počítače můžete v Azure vytvářet pomocí některé z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="48d68-107">You can create VMs in Azure using one of the following CLI versions:</span></span>

- <span data-ttu-id="48d68-108">Azure CLI 1.0 – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků (tento článek)</span><span class="sxs-lookup"><span data-stu-id="48d68-108">Azure CLI 1.0 – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="48d68-109">[Azure CLI 2.0](../windows/creation-choices.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="48d68-109">[Azure CLI 2.0](../windows/creation-choices.md) - our next generation CLI for the resource management deployment model</span></span>

<span data-ttu-id="48d68-110">Rozhraní Azure CLI 1.0 je k dispozici napříč platformami jako balíček npm, balíček distribuce nebo kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="48d68-110">The Azure CLI 1.0 is available across platforms via an npm package, distro-provided packages, or Docker container.</span></span> <span data-ttu-id="48d68-111">Můžete si přečíst další informace o tom, [jak nainstalovat a nakonfigurovat Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="48d68-111">You can read more about [how to install and configure the Azure CLI](../../cli-install-nodejs.md).</span></span> <span data-ttu-id="48d68-112">V následujících kurzech najdete příklady použití Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="48d68-112">The following tutorials provide examples on using the Azure CLI 1.0.</span></span> <span data-ttu-id="48d68-113">V každém článku si přečtěte další podrobnosti o zobrazených příkazech rychlého startu CLI:</span><span class="sxs-lookup"><span data-stu-id="48d68-113">Read each article for more details on the CLI quick-start commands shown:</span></span>

* [<span data-ttu-id="48d68-114">Vytvoření virtuálního počítače s Linuxem z Azure CLI pro vývoj a testování</span><span class="sxs-lookup"><span data-stu-id="48d68-114">Create a Linux VM from the Azure CLI for dev and test</span></span>](quick-create-cli-nodejs.md)
  
  * <span data-ttu-id="48d68-115">Následující příklad vytvoří pomocí veřejného klíče s názvem počítače s CoreOS *azure_id_rsa.pub*:</span><span class="sxs-lookup"><span data-stu-id="48d68-115">The following example creates a CoreOS VM using a public key named *azure_id_rsa.pub*:</span></span>
    
    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [<span data-ttu-id="48d68-116">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure</span><span class="sxs-lookup"><span data-stu-id="48d68-116">Create a secured Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md)
  
  * <span data-ttu-id="48d68-117">Následující příklad vytvoří virtuální počítač pomocí šablony uložené na GitHubu:</span><span class="sxs-lookup"><span data-stu-id="48d68-117">The following example creates a VM using a template stored on GitHub:</span></span>
    
    ```azurecli
    azure group create --name myResourceGroup --location eastus 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [<span data-ttu-id="48d68-118">Vytvoření kompletního linuxového prostředí pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="48d68-118">Create a complete Linux environment using the Azure CLI</span></span>](create-cli-complete-nodejs.md)
  
  * <span data-ttu-id="48d68-119">Zahrnuje vytvoření nástroje pro vyrovnávání zatížení a více virtuálních počítačů ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="48d68-119">Includes creating a load balancer and multiple VMs in an availability set.</span></span>
* [<span data-ttu-id="48d68-120">Přidání disku do virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="48d68-120">Add a disk to a Linux VM</span></span>](add-disk.md)
  
  * <span data-ttu-id="48d68-121">Následující příklad přidá *5* GB místa na disku na existující virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="48d68-121">The following example adds a *5* Gb disk to an existing VM named *myVM*:</span></span>
    
    ```azurecli
    azure vm disk attach-new \
        --resource-group myResourceGroup \
        --vm-name myVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a><span data-ttu-id="48d68-122">portál Azure</span><span class="sxs-lookup"><span data-stu-id="48d68-122">Azure portal</span></span>
<span data-ttu-id="48d68-123">[Azure Portal](https://portal.azure.com) umožňuje rychle vytvořit virtuální počítač, protože není nutné nic instalovat na váš systém.</span><span class="sxs-lookup"><span data-stu-id="48d68-123">The [Azure portal](https://portal.azure.com) allows you to quickly create a VM since there is nothing to install on your system.</span></span> <span data-ttu-id="48d68-124">Vytvoření virtuálního počítače pomocí portálu Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="48d68-124">Use the Azure portal to create the VM:</span></span>

* [<span data-ttu-id="48d68-125">Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="48d68-125">Create a Linux VM using the Azure portal</span></span>](quick-create-portal.md) 

## <a name="operating-system-and-image-choices"></a><span data-ttu-id="48d68-126">Operační systém a volba image</span><span class="sxs-lookup"><span data-stu-id="48d68-126">Operating system and image choices</span></span>
<span data-ttu-id="48d68-127">Při vytváření virtuálního počítače zvolíte image podle operačního systému, který chcete používat.</span><span class="sxs-lookup"><span data-stu-id="48d68-127">When creating a VM, you choose an image based on the operating system you want to run.</span></span> <span data-ttu-id="48d68-128">Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje.</span><span class="sxs-lookup"><span data-stu-id="48d68-128">Azure and its partners offer many images, some of which include applications and tools pre-installed.</span></span> <span data-ttu-id="48d68-129">Nebo nahrajte některou z vlastních imagí (viz [následující oddíl](#use-your-own-image)).</span><span class="sxs-lookup"><span data-stu-id="48d68-129">Or, upload one of your own images (see [the following section](#use-your-own-image)).</span></span>

### <a name="azure-images"></a><span data-ttu-id="48d68-130">Image dostupné v Azure</span><span class="sxs-lookup"><span data-stu-id="48d68-130">Azure images</span></span>
<span data-ttu-id="48d68-131">Pomocí příkazů rozhraní příkazového řádku `azure vm image` zobrazíte dostupné image podle vydavatele, vydání distribuce a sestavení.</span><span class="sxs-lookup"><span data-stu-id="48d68-131">Use the `azure vm image` CLI commands to see what's available by publisher, distro release, and builds.</span></span>

<span data-ttu-id="48d68-132">Seznam dostupných vydavatelů zobrazíte takto:</span><span class="sxs-lookup"><span data-stu-id="48d68-132">List available publishers as follows:</span></span>

```azurecli
azure vm image list-publishers --location eastus
```

<span data-ttu-id="48d68-133">Seznam dostupných produktů (nabídek) pro daného vydavatele zobrazíte takto:</span><span class="sxs-lookup"><span data-stu-id="48d68-133">List available products (offers) for a given publisher as follows:</span></span>

```azurecli
azure vm image list-offers --location eastus --publisher Canonical
```

<span data-ttu-id="48d68-134">Seznam dostupných SKU (vydání distribuce) pro danou nabídku zobrazíte takto:</span><span class="sxs-lookup"><span data-stu-id="48d68-134">List available SKUs (distro releases) of a given offer as follows:</span></span>

```azurecli
azure vm image list-skus --location eastus --publisher Canonical --offer UbuntuServer
```

<span data-ttu-id="48d68-135">Seznam všech dostupných imagí pro dané vydání zobrazíte takto:</span><span class="sxs-lookup"><span data-stu-id="48d68-135">List all available images for a given release follows:</span></span>

```azurecli
azure vm image list --location eastus --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

<span data-ttu-id="48d68-136">Příkazy `azure vm quick-create` a `azure vm create` mají aliasy, které můžete použít pro rychlý přístup k častějším distribucím a jejich nejnovějším vydáním.</span><span class="sxs-lookup"><span data-stu-id="48d68-136">The `azure vm quick-create` and `azure vm create` commands have aliases you can use to quickly access the more common distros and their latest releases.</span></span> <span data-ttu-id="48d68-137">Použití aliasů je často rychlejší, než zadávání vydavatele, nabídky, SKU a verze při každém vytváření virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="48d68-137">Using aliases is often quicker than specifying the publisher, offer, SKU, and version each time you create a VM:</span></span>

| <span data-ttu-id="48d68-138">Alias</span><span class="sxs-lookup"><span data-stu-id="48d68-138">Alias</span></span> | <span data-ttu-id="48d68-139">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="48d68-139">Publisher</span></span> | <span data-ttu-id="48d68-140">Nabídka</span><span class="sxs-lookup"><span data-stu-id="48d68-140">Offer</span></span> | <span data-ttu-id="48d68-141">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="48d68-141">SKU</span></span> | <span data-ttu-id="48d68-142">Verze</span><span class="sxs-lookup"><span data-stu-id="48d68-142">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="48d68-143">CentOS</span><span class="sxs-lookup"><span data-stu-id="48d68-143">CentOS</span></span> |<span data-ttu-id="48d68-144">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="48d68-144">OpenLogic</span></span> |<span data-ttu-id="48d68-145">Centos</span><span class="sxs-lookup"><span data-stu-id="48d68-145">Centos</span></span> |<span data-ttu-id="48d68-146">7.2</span><span class="sxs-lookup"><span data-stu-id="48d68-146">7.2</span></span> |<span data-ttu-id="48d68-147">nejnovější</span><span class="sxs-lookup"><span data-stu-id="48d68-147">latest</span></span> |
| <span data-ttu-id="48d68-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="48d68-148">CoreOS</span></span> |<span data-ttu-id="48d68-149">CoreOS</span><span class="sxs-lookup"><span data-stu-id="48d68-149">CoreOS</span></span> |<span data-ttu-id="48d68-150">CoreOS</span><span class="sxs-lookup"><span data-stu-id="48d68-150">CoreOS</span></span> |<span data-ttu-id="48d68-151">Stable</span><span class="sxs-lookup"><span data-stu-id="48d68-151">Stable</span></span> |<span data-ttu-id="48d68-152">nejnovější</span><span class="sxs-lookup"><span data-stu-id="48d68-152">latest</span></span> |
| <span data-ttu-id="48d68-153">Debian</span><span class="sxs-lookup"><span data-stu-id="48d68-153">Debian</span></span> |<span data-ttu-id="48d68-154">credativ</span><span class="sxs-lookup"><span data-stu-id="48d68-154">credativ</span></span> |<span data-ttu-id="48d68-155">Debian</span><span class="sxs-lookup"><span data-stu-id="48d68-155">Debian</span></span> |<span data-ttu-id="48d68-156">8</span><span class="sxs-lookup"><span data-stu-id="48d68-156">8</span></span> |<span data-ttu-id="48d68-157">nejnovější</span><span class="sxs-lookup"><span data-stu-id="48d68-157">latest</span></span> |
| <span data-ttu-id="48d68-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="48d68-158">openSUSE</span></span> |<span data-ttu-id="48d68-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="48d68-159">SUSE</span></span> |<span data-ttu-id="48d68-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="48d68-160">openSUSE</span></span> |<span data-ttu-id="48d68-161">13.2</span><span class="sxs-lookup"><span data-stu-id="48d68-161">13.2</span></span> |<span data-ttu-id="48d68-162">nejnovější</span><span class="sxs-lookup"><span data-stu-id="48d68-162">latest</span></span> |
| <span data-ttu-id="48d68-163">RHEL</span><span class="sxs-lookup"><span data-stu-id="48d68-163">RHEL</span></span> |<span data-ttu-id="48d68-164">Redhat</span><span class="sxs-lookup"><span data-stu-id="48d68-164">Redhat</span></span> |<span data-ttu-id="48d68-165">RHEL</span><span class="sxs-lookup"><span data-stu-id="48d68-165">RHEL</span></span> |<span data-ttu-id="48d68-166">7.2</span><span class="sxs-lookup"><span data-stu-id="48d68-166">7.2</span></span> |<span data-ttu-id="48d68-167">nejnovější</span><span class="sxs-lookup"><span data-stu-id="48d68-167">latest</span></span> |
| <span data-ttu-id="48d68-168">SLES</span><span class="sxs-lookup"><span data-stu-id="48d68-168">SLES</span></span> |<span data-ttu-id="48d68-169">SLES</span><span class="sxs-lookup"><span data-stu-id="48d68-169">SLES</span></span> |<span data-ttu-id="48d68-170">SLES</span><span class="sxs-lookup"><span data-stu-id="48d68-170">SLES</span></span> |<span data-ttu-id="48d68-171">12-SP1</span><span class="sxs-lookup"><span data-stu-id="48d68-171">12-SP1</span></span> |<span data-ttu-id="48d68-172">nejnovější</span><span class="sxs-lookup"><span data-stu-id="48d68-172">latest</span></span> |
| <span data-ttu-id="48d68-173">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="48d68-173">UbuntuLTS</span></span> |<span data-ttu-id="48d68-174">Canonical</span><span class="sxs-lookup"><span data-stu-id="48d68-174">Canonical</span></span> |<span data-ttu-id="48d68-175">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="48d68-175">UbuntuServer</span></span> |<span data-ttu-id="48d68-176">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="48d68-176">14.04.4-LTS</span></span> |<span data-ttu-id="48d68-177">nejnovější</span><span class="sxs-lookup"><span data-stu-id="48d68-177">latest</span></span> |

### <a name="use-your-own-image"></a><span data-ttu-id="48d68-178">Použití vlastní image</span><span class="sxs-lookup"><span data-stu-id="48d68-178">Use your own image</span></span>
<span data-ttu-id="48d68-179">Pokud budete potřebovat image se zvláštními úpravami, můžete *zachytit* stávající virtuální počítač a použít image založenou na tomto virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="48d68-179">If you require specific customizations, you can use an image based on an existing Azure VM by *capturing* that VM.</span></span> <span data-ttu-id="48d68-180">Můžete také nahrát místně vytvořenou image.</span><span class="sxs-lookup"><span data-stu-id="48d68-180">You can also upload an image created on-premises.</span></span> <span data-ttu-id="48d68-181">Další informace o podporovaných distribucích a o tom, jak používat vlastní image, najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="48d68-181">For more information on supported distros and how to use your own images, see the following articles:</span></span>

* [<span data-ttu-id="48d68-182">Distribuce schválené pro Azure</span><span class="sxs-lookup"><span data-stu-id="48d68-182">Azure endorsed distributions</span></span>](endorsed-distros.md)
* [<span data-ttu-id="48d68-183">Informace pro neschválené distribuce</span><span class="sxs-lookup"><span data-stu-id="48d68-183">Information for non-endorsed distributions</span></span>](create-upload-generic.md)
* [<span data-ttu-id="48d68-184">Nahrání a vytvoření virtuálního počítače s Linuxem z vlastní image disku</span><span class="sxs-lookup"><span data-stu-id="48d68-184">Upload and create a Linux VM from custom disk image</span></span>](upload-vhd.md)
* <span data-ttu-id="48d68-185">[Jak zachytit virtuální počítač s Linuxem do šablony Resource Manageru](capture-image.md)</span><span class="sxs-lookup"><span data-stu-id="48d68-185">[How to capture a Linux virtual machine as a Resource Manager template](capture-image.md).</span></span>
  
  * <span data-ttu-id="48d68-186">Příkazy rychlého startu pro zachycení stávajícího virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="48d68-186">Quick-start example commands to capture an existing VM:</span></span>
    
    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a><span data-ttu-id="48d68-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48d68-187">Next steps</span></span>
* <span data-ttu-id="48d68-188">Vytvořte virtuální počítač s Linuxem z webu [Azure Portal](quick-create-portal.md), přes [rozhraní příkazového řádku](quick-create-cli.md) nebo pomocí [šablony Azure Resource Manageru](../windows/cli-deploy-templates.md).</span><span class="sxs-lookup"><span data-stu-id="48d68-188">Create a Linux VM from the [portal](quick-create-portal.md), with the [CLI](quick-create-cli.md), or using an [Azure Resource Manager template](../windows/cli-deploy-templates.md).</span></span>
* <span data-ttu-id="48d68-189">Po vytvoření virtuálního počítače s Linuxem můžete [přidat datový disk](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="48d68-189">After creating a Linux VM, [add a data disk](add-disk.md).</span></span>
* <span data-ttu-id="48d68-190">Rychlé kroky pro [resetování hesla nebo klíčů SSH a správu uživatelů](using-vmaccess-extension.md)</span><span class="sxs-lookup"><span data-stu-id="48d68-190">Quick steps to [reset a password or SSH keys and manage users](using-vmaccess-extension.md)</span></span>

