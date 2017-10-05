---
title: "Různé způsoby vytvoření virtuálního počítače s Linuxem v Azure | Microsoft Azure"
description: "Další informace o různých způsobech vytvoření virtuálního počítače s Linuxem na platformě Azure, včetně odkazů na nástroje a kurzy pro jednotlivé metody."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: b2f93579eb1c7a69d0dbd1b0ef112aed9b2168c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="different-ways-to-create-a-linux-vm"></a><span data-ttu-id="b5982-103">Různé způsoby vytváření virtuálních počítačů s Linuxem</span><span class="sxs-lookup"><span data-stu-id="b5982-103">Different ways to create a Linux VM</span></span>
<span data-ttu-id="b5982-104">V Azure máte flexibilitu vytvoření virtuálního počítače s Linuxem pomocí nástrojů a pracovních postupů, které vám vyhovují.</span><span class="sxs-lookup"><span data-stu-id="b5982-104">You have the flexibility in Azure to create a Linux virtual machine (VM) using tools and workflows comfortable to you.</span></span> <span data-ttu-id="b5982-105">Tento článek shrnuje tyto rozdíly a příklady vytvoření virtuálních počítačů s Linuxem, včetně Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="b5982-105">This article summarizes these differences and examples for creating your Linux VMs, including the Azure CLI 2.0.</span></span> <span data-ttu-id="b5982-106">Můžete si také prohlédnout možnosti vytvoření včetně [Azure CLI 1.0](creation-choices-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="b5982-106">You can also view creation choices including the [Azure CLI 1.0](creation-choices-nodejs.md).</span></span>

<span data-ttu-id="b5982-107">Rozhraní [Azure CLI 2.0](/cli/azure/install-az-cli2) je dostupné napříč platformami jako balíček npm, balíček distribuce nebo kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="b5982-107">The [Azure CLI 2.0](/cli/azure/install-az-cli2) is available across platforms via an npm package, distro-provided packages, or Docker container.</span></span> <span data-ttu-id="b5982-108">Nainstalujte sestavení nejvhodnější pro vaše prostředí a přihlaste se k účtu Azure pomocí příkazu [az login](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="b5982-108">Install the most appropriate build for your environment and log in to an Azure account using [az login](/cli/azure/#login)</span></span>

* [<span data-ttu-id="b5982-109">Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b5982-109">Create a Linux VM with the Azure CLI 2.0</span></span>](quick-create-cli.md)
  
  * <span data-ttu-id="b5982-110">Pomocí příkazu [az group create](/cli/azure/group#create) vytvořte skupinu prostředků *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="b5982-110">Create a resource group with [az group create](/cli/azure/group#create) named *myResourceGroup*:</span></span> 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * <span data-ttu-id="b5982-111">Pomocí příkazu [az vm create](/cli/azure/vm#create) vytvořte virtuální počítač *myVM* s použitím nejnovější image *UbuntuLTS* a vygenerujte klíče SSH (pokud už ve složce *~/.ssh* neexistují):</span><span class="sxs-lookup"><span data-stu-id="b5982-111">Create a VM with [az vm create](/cli/azure/vm#create) named *myVM* using the latest *UbuntuLTS* image and generate SSH keys if they do not already exist in *~/.ssh*:</span></span>

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [<span data-ttu-id="b5982-112">Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure</span><span class="sxs-lookup"><span data-stu-id="b5982-112">Create a Linux VM with an Azure template</span></span>](create-ssh-secured-vm-from-template.md)
  
  * <span data-ttu-id="b5982-113">Následující příklad pomocí příkazu [az group deployment create](/cli/azure/group/deployment#create) vytvoří virtuální počítač ze šablony uložené na GitHubu:</span><span class="sxs-lookup"><span data-stu-id="b5982-113">The following example uses [az group deployment create](/cli/azure/group/deployment#create) to create a VM from a template stored on GitHub:</span></span>
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [<span data-ttu-id="b5982-114">Vytvoření a přizpůsobení virtuálního počítače s Linuxem pomocí cloud-init</span><span class="sxs-lookup"><span data-stu-id="b5982-114">Create a Linux VM and customize with cloud-init</span></span>](tutorial-automate-vm-deployment.md)

* [<span data-ttu-id="b5982-115">Vytvoření vysoce dostupné aplikace s vyrovnáváním zatížení na více virtuálních počítačích s Linuxem</span><span class="sxs-lookup"><span data-stu-id="b5982-115">Create a load balanced and highly available application on multiple Linux VMs</span></span>](tutorial-load-balancer.md)


## <a name="azure-portal"></a><span data-ttu-id="b5982-116">portál Azure</span><span class="sxs-lookup"><span data-stu-id="b5982-116">Azure portal</span></span>
<span data-ttu-id="b5982-117">[Azure Portal](https://portal.azure.com) umožňuje rychle vytvořit virtuální počítač, protože není nutné nic instalovat na váš systém.</span><span class="sxs-lookup"><span data-stu-id="b5982-117">The [Azure portal](https://portal.azure.com) allows you to quickly create a VM since there is nothing to install on your system.</span></span> <span data-ttu-id="b5982-118">Vytvoření virtuálního počítače pomocí portálu Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="b5982-118">Use the Azure portal to create the VM:</span></span>

* [<span data-ttu-id="b5982-119">Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b5982-119">Create a Linux VM using the Azure portal</span></span>](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a><span data-ttu-id="b5982-120">Operační systém a volba image</span><span class="sxs-lookup"><span data-stu-id="b5982-120">Operating system and image choices</span></span>
<span data-ttu-id="b5982-121">Při vytváření virtuálního počítače zvolíte image podle operačního systému, který chcete používat.</span><span class="sxs-lookup"><span data-stu-id="b5982-121">When creating a VM, you choose an image based on the operating system you want to run.</span></span> <span data-ttu-id="b5982-122">Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje.</span><span class="sxs-lookup"><span data-stu-id="b5982-122">Azure and its partners offer many images, some of which include applications and tools pre-installed.</span></span> <span data-ttu-id="b5982-123">Nebo nahrajte některou z vlastních imagí (viz [následující oddíl](#use-your-own-image)).</span><span class="sxs-lookup"><span data-stu-id="b5982-123">Or, upload one of your own images (see [the following section](#use-your-own-image)).</span></span>

### <a name="azure-images"></a><span data-ttu-id="b5982-124">Image dostupné v Azure</span><span class="sxs-lookup"><span data-stu-id="b5982-124">Azure images</span></span>
<span data-ttu-id="b5982-125">Pomocí příkazů [az vm image](/cli/azure/vm/image) zobrazíte dostupné image podle vydavatele, distribuce nebo sestavení.</span><span class="sxs-lookup"><span data-stu-id="b5982-125">Use the [az vm image](/cli/azure/vm/image) commands to see what's available by publisher, distro release, and builds.</span></span>

<span data-ttu-id="b5982-126">Seznam dostupných vydavatelů:</span><span class="sxs-lookup"><span data-stu-id="b5982-126">List available publishers:</span></span>

```azurecli
az vm image list-publishers --location eastus
```

<span data-ttu-id="b5982-127">Seznam dostupných produktů (nabídek) pro daného vydavatele:</span><span class="sxs-lookup"><span data-stu-id="b5982-127">List available products (offers) for a given publisher:</span></span>

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

<span data-ttu-id="b5982-128">Seznam dostupných SKU (vydání distribuce) pro danou nabídku:</span><span class="sxs-lookup"><span data-stu-id="b5982-128">List available SKUs (distro releases) of a given offer:</span></span>

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

<span data-ttu-id="b5982-129">Seznam všech dostupných imagí pro dané vydání:</span><span class="sxs-lookup"><span data-stu-id="b5982-129">List all available images for a given release:</span></span>

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

<span data-ttu-id="b5982-130">Další příklady vyhledávání a použití dostupných imagí najdete v tématu [Vyhledání a výběr imagí virtuálních počítačů Azure pomocí Azure CLI](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="b5982-130">For more examples on browsing and using available images, see [Navigate and select Azure virtual machine images with the Azure CLI](cli-ps-findimage.md).</span></span>

<span data-ttu-id="b5982-131">Příkaz [az vm create](/cli/azure/vm#create) má aliasy, které můžete použít pro rychlý přístup k častějším distribucím a jejich nejnovějším vydáním.</span><span class="sxs-lookup"><span data-stu-id="b5982-131">The [az vm create](/cli/azure/vm#create) command has aliases you can use to quickly access the more common distros and their latest releases.</span></span> <span data-ttu-id="b5982-132">Použití aliasů je často rychlejší, než zadávání vydavatele, nabídky, SKU a verze při každém vytváření virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="b5982-132">Using aliases is often quicker than specifying the publisher, offer, SKU, and version each time you create a VM:</span></span>

| <span data-ttu-id="b5982-133">Alias</span><span class="sxs-lookup"><span data-stu-id="b5982-133">Alias</span></span> | <span data-ttu-id="b5982-134">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="b5982-134">Publisher</span></span> | <span data-ttu-id="b5982-135">Nabídka</span><span class="sxs-lookup"><span data-stu-id="b5982-135">Offer</span></span> | <span data-ttu-id="b5982-136">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="b5982-136">SKU</span></span> | <span data-ttu-id="b5982-137">Verze</span><span class="sxs-lookup"><span data-stu-id="b5982-137">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="b5982-138">CentOS</span><span class="sxs-lookup"><span data-stu-id="b5982-138">CentOS</span></span> |<span data-ttu-id="b5982-139">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="b5982-139">OpenLogic</span></span> |<span data-ttu-id="b5982-140">Centos</span><span class="sxs-lookup"><span data-stu-id="b5982-140">Centos</span></span> |<span data-ttu-id="b5982-141">7.2</span><span class="sxs-lookup"><span data-stu-id="b5982-141">7.2</span></span> |<span data-ttu-id="b5982-142">nejnovější</span><span class="sxs-lookup"><span data-stu-id="b5982-142">latest</span></span> |
| <span data-ttu-id="b5982-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="b5982-143">CoreOS</span></span> |<span data-ttu-id="b5982-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="b5982-144">CoreOS</span></span> |<span data-ttu-id="b5982-145">CoreOS</span><span class="sxs-lookup"><span data-stu-id="b5982-145">CoreOS</span></span> |<span data-ttu-id="b5982-146">Stable</span><span class="sxs-lookup"><span data-stu-id="b5982-146">Stable</span></span> |<span data-ttu-id="b5982-147">nejnovější</span><span class="sxs-lookup"><span data-stu-id="b5982-147">latest</span></span> |
| <span data-ttu-id="b5982-148">Debian</span><span class="sxs-lookup"><span data-stu-id="b5982-148">Debian</span></span> |<span data-ttu-id="b5982-149">credativ</span><span class="sxs-lookup"><span data-stu-id="b5982-149">credativ</span></span> |<span data-ttu-id="b5982-150">Debian</span><span class="sxs-lookup"><span data-stu-id="b5982-150">Debian</span></span> |<span data-ttu-id="b5982-151">8</span><span class="sxs-lookup"><span data-stu-id="b5982-151">8</span></span> |<span data-ttu-id="b5982-152">nejnovější</span><span class="sxs-lookup"><span data-stu-id="b5982-152">latest</span></span> |
| <span data-ttu-id="b5982-153">openSUSE</span><span class="sxs-lookup"><span data-stu-id="b5982-153">openSUSE</span></span> |<span data-ttu-id="b5982-154">SUSE</span><span class="sxs-lookup"><span data-stu-id="b5982-154">SUSE</span></span> |<span data-ttu-id="b5982-155">openSUSE</span><span class="sxs-lookup"><span data-stu-id="b5982-155">openSUSE</span></span> |<span data-ttu-id="b5982-156">13.2</span><span class="sxs-lookup"><span data-stu-id="b5982-156">13.2</span></span> |<span data-ttu-id="b5982-157">nejnovější</span><span class="sxs-lookup"><span data-stu-id="b5982-157">latest</span></span> |
| <span data-ttu-id="b5982-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="b5982-158">RHEL</span></span> |<span data-ttu-id="b5982-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="b5982-159">Redhat</span></span> |<span data-ttu-id="b5982-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="b5982-160">RHEL</span></span> |<span data-ttu-id="b5982-161">7.2</span><span class="sxs-lookup"><span data-stu-id="b5982-161">7.2</span></span> |<span data-ttu-id="b5982-162">nejnovější</span><span class="sxs-lookup"><span data-stu-id="b5982-162">latest</span></span> |
| <span data-ttu-id="b5982-163">SLES</span><span class="sxs-lookup"><span data-stu-id="b5982-163">SLES</span></span> |<span data-ttu-id="b5982-164">SLES</span><span class="sxs-lookup"><span data-stu-id="b5982-164">SLES</span></span> |<span data-ttu-id="b5982-165">SLES</span><span class="sxs-lookup"><span data-stu-id="b5982-165">SLES</span></span> |<span data-ttu-id="b5982-166">12-SP1</span><span class="sxs-lookup"><span data-stu-id="b5982-166">12-SP1</span></span> |<span data-ttu-id="b5982-167">nejnovější</span><span class="sxs-lookup"><span data-stu-id="b5982-167">latest</span></span> |
| <span data-ttu-id="b5982-168">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="b5982-168">UbuntuLTS</span></span> |<span data-ttu-id="b5982-169">Canonical</span><span class="sxs-lookup"><span data-stu-id="b5982-169">Canonical</span></span> |<span data-ttu-id="b5982-170">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="b5982-170">UbuntuServer</span></span> |<span data-ttu-id="b5982-171">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="b5982-171">14.04.4-LTS</span></span> |<span data-ttu-id="b5982-172">nejnovější</span><span class="sxs-lookup"><span data-stu-id="b5982-172">latest</span></span> |

### <a name="use-your-own-image"></a><span data-ttu-id="b5982-173">Použití vlastní image</span><span class="sxs-lookup"><span data-stu-id="b5982-173">Use your own image</span></span>
<span data-ttu-id="b5982-174">Pokud budete potřebovat image se zvláštními úpravami, můžete zachytit stávající virtuální počítač a použít image založenou na tomto virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="b5982-174">If you require specific customizations, you can use an image based on an existing Azure VM by capturing that VM.</span></span> <span data-ttu-id="b5982-175">Můžete také nahrát místně vytvořenou image.</span><span class="sxs-lookup"><span data-stu-id="b5982-175">You can also upload an image created on-premises.</span></span> <span data-ttu-id="b5982-176">Další informace o podporovaných distribucích a o tom, jak používat vlastní image, najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="b5982-176">For more information on supported distros and how to use your own images, see the following articles:</span></span>

* [<span data-ttu-id="b5982-177">Distribuce schválené pro Azure</span><span class="sxs-lookup"><span data-stu-id="b5982-177">Azure endorsed distributions</span></span>](endorsed-distros.md)
* [<span data-ttu-id="b5982-178">Informace pro neschválené distribuce</span><span class="sxs-lookup"><span data-stu-id="b5982-178">Information for non-endorsed distributions</span></span>](create-upload-generic.md)
* <span data-ttu-id="b5982-179">[Vytvoření image z existujícího virtuálního počítače Azure](tutorial-custom-images.md)</span><span class="sxs-lookup"><span data-stu-id="b5982-179">[How to create an image from an existing Azure VM](tutorial-custom-images.md).</span></span>
  
  * <span data-ttu-id="b5982-180">Příklady příkazů pro vytvoření image z existujícího virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="b5982-180">Quick-start example commands to create an image from an existing Azure VM:</span></span>
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a><span data-ttu-id="b5982-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5982-181">Next steps</span></span>
* <span data-ttu-id="b5982-182">Vytvořte virtuální počítač s Linuxem pomocí [rozhraní příkazového řádku](quick-create-cli.md), z [portálu](quick-create-portal.md) nebo pomocí [šablony Azure Resource Manageru](../windows/cli-deploy-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b5982-182">Create a Linux VM with the [CLI](quick-create-cli.md), from the [portal](quick-create-portal.md), or using an [Azure Resource Manager template](../windows/cli-deploy-templates.md).</span></span>
* <span data-ttu-id="b5982-183">Po vytvoření virtuálního počítače s Linuxem [si přečtěte o discích a úložišti Azure](tutorial-manage-disks.md).</span><span class="sxs-lookup"><span data-stu-id="b5982-183">After creating a Linux VM, [learn about Azure disks and storage](tutorial-manage-disks.md).</span></span>
* <span data-ttu-id="b5982-184">Rychlý postup k [resetování hesla nebo klíčů SSH a správě uživatelů](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="b5982-184">Quick steps to [reset a password or SSH keys and manage users](using-vmaccess-extension.md).</span></span>
