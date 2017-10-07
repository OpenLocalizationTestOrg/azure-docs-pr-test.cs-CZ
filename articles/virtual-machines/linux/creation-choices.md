---
title: "aaaDifferent způsoby toocreate virtuálního počítače s Linuxem v Azure | Microsoft Azure"
description: "Přečtěte si hello různé způsoby toocreate virtuální počítač s Linuxem v Azure, včetně tootools odkazy a kurzy pro každou metodu."
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
ms.openlocfilehash: 250e92c063c87a8c1279097dc2264777d95478d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-linux-vm"></a><span data-ttu-id="233ca-103">Různé způsoby toocreate virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="233ca-103">Different ways toocreate a Linux VM</span></span>
<span data-ttu-id="233ca-104">Budete mít hello flexibilitu v Azure toocreate Linux virtuálního počítače (VM) pomocí nástroje a pracovní postupy možnost tooyou.</span><span class="sxs-lookup"><span data-stu-id="233ca-104">You have hello flexibility in Azure toocreate a Linux virtual machine (VM) using tools and workflows comfortable tooyou.</span></span> <span data-ttu-id="233ca-105">Tento článek obsahuje souhrn tyto rozdíly a příklady pro vytváření virtuálních počítačů Linux, včetně hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="233ca-105">This article summarizes these differences and examples for creating your Linux VMs, including hello Azure CLI 2.0.</span></span> <span data-ttu-id="233ca-106">Můžete také zobrazit možnosti vytvoření včetně hello [Azure CLI 1.0](creation-choices-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="233ca-106">You can also view creation choices including hello [Azure CLI 1.0](creation-choices-nodejs.md).</span></span>

<span data-ttu-id="233ca-107">Hello [Azure CLI 2.0](/cli/azure/install-az-cli2) napříč platformami pomocí balíčku npm, zadaný distro balíčky nebo kontejner Docker je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="233ca-107">hello [Azure CLI 2.0](/cli/azure/install-az-cli2) is available across platforms via an npm package, distro-provided packages, or Docker container.</span></span> <span data-ttu-id="233ca-108">Nainstalujte hello sestavení nejvhodnější pro vaše prostředí a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login)</span><span class="sxs-lookup"><span data-stu-id="233ca-108">Install hello most appropriate build for your environment and log in tooan Azure account using [az login](/cli/azure/#login)</span></span>

* [<span data-ttu-id="233ca-109">Vytvoření virtuálního počítače s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="233ca-109">Create a Linux VM with hello Azure CLI 2.0</span></span>](quick-create-cli.md)
  
  * <span data-ttu-id="233ca-110">Pomocí příkazu [az group create](/cli/azure/group#create) vytvořte skupinu prostředků *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="233ca-110">Create a resource group with [az group create](/cli/azure/group#create) named *myResourceGroup*:</span></span> 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * <span data-ttu-id="233ca-111">Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) s názvem *Můjvp* pomocí nejnovější hello *UbuntuLTS* bitovou kopii a generovat klíče SSH, pokud už neexistují v *~/.ssh*:</span><span class="sxs-lookup"><span data-stu-id="233ca-111">Create a VM with [az vm create](/cli/azure/vm#create) named *myVM* using hello latest *UbuntuLTS* image and generate SSH keys if they do not already exist in *~/.ssh*:</span></span>

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [<span data-ttu-id="233ca-112">Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure</span><span class="sxs-lookup"><span data-stu-id="233ca-112">Create a Linux VM with an Azure template</span></span>](create-ssh-secured-vm-from-template.md)
  
  * <span data-ttu-id="233ca-113">Hello následující příklad používá [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) toocreate virtuálního počítače ze šablony uložené na Githubu:</span><span class="sxs-lookup"><span data-stu-id="233ca-113">hello following example uses [az group deployment create](/cli/azure/group/deployment#create) toocreate a VM from a template stored on GitHub:</span></span>
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [<span data-ttu-id="233ca-114">Vytvoření a přizpůsobení virtuálního počítače s Linuxem pomocí cloud-init</span><span class="sxs-lookup"><span data-stu-id="233ca-114">Create a Linux VM and customize with cloud-init</span></span>](tutorial-automate-vm-deployment.md)

* [<span data-ttu-id="233ca-115">Vytvoření vysoce dostupné aplikace s vyrovnáváním zatížení na více virtuálních počítačích s Linuxem</span><span class="sxs-lookup"><span data-stu-id="233ca-115">Create a load balanced and highly available application on multiple Linux VMs</span></span>](tutorial-load-balancer.md)


## <a name="azure-portal"></a><span data-ttu-id="233ca-116">portál Azure</span><span class="sxs-lookup"><span data-stu-id="233ca-116">Azure portal</span></span>
<span data-ttu-id="233ca-117">Hello [portál Azure](https://portal.azure.com) vám umožní tooquickly vytvoření virtuálního počítače, protože není nic tooinstall ve vašem systému.</span><span class="sxs-lookup"><span data-stu-id="233ca-117">hello [Azure portal](https://portal.azure.com) allows you tooquickly create a VM since there is nothing tooinstall on your system.</span></span> <span data-ttu-id="233ca-118">Pomocí portálu Azure toocreate hello hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="233ca-118">Use hello Azure portal toocreate hello VM:</span></span>

* [<span data-ttu-id="233ca-119">Vytvoření virtuálního počítače s Linuxem pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="233ca-119">Create a Linux VM using hello Azure portal</span></span>](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a><span data-ttu-id="233ca-120">Operační systém a volba image</span><span class="sxs-lookup"><span data-stu-id="233ca-120">Operating system and image choices</span></span>
<span data-ttu-id="233ca-121">Při vytváření virtuálního počítače, zvolíte image podle operačního systému, které chcete toorun hello.</span><span class="sxs-lookup"><span data-stu-id="233ca-121">When creating a VM, you choose an image based on hello operating system you want toorun.</span></span> <span data-ttu-id="233ca-122">Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje.</span><span class="sxs-lookup"><span data-stu-id="233ca-122">Azure and its partners offer many images, some of which include applications and tools pre-installed.</span></span> <span data-ttu-id="233ca-123">Nebo nahrát některou ze svých vlastních imagí (viz [hello následující části](#use-your-own-image)).</span><span class="sxs-lookup"><span data-stu-id="233ca-123">Or, upload one of your own images (see [hello following section](#use-your-own-image)).</span></span>

### <a name="azure-images"></a><span data-ttu-id="233ca-124">Image dostupné v Azure</span><span class="sxs-lookup"><span data-stu-id="233ca-124">Azure images</span></span>
<span data-ttu-id="233ca-125">Použití hello [image virtuálního počítače az](/cli/azure/vm/image) toosee příkazy, které jsou dostupné vydavatele, verzi distro a sestavení.</span><span class="sxs-lookup"><span data-stu-id="233ca-125">Use hello [az vm image](/cli/azure/vm/image) commands toosee what's available by publisher, distro release, and builds.</span></span>

<span data-ttu-id="233ca-126">Seznam dostupných vydavatelů:</span><span class="sxs-lookup"><span data-stu-id="233ca-126">List available publishers:</span></span>

```azurecli
az vm image list-publishers --location eastus
```

<span data-ttu-id="233ca-127">Seznam dostupných produktů (nabídek) pro daného vydavatele:</span><span class="sxs-lookup"><span data-stu-id="233ca-127">List available products (offers) for a given publisher:</span></span>

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

<span data-ttu-id="233ca-128">Seznam dostupných SKU (vydání distribuce) pro danou nabídku:</span><span class="sxs-lookup"><span data-stu-id="233ca-128">List available SKUs (distro releases) of a given offer:</span></span>

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

<span data-ttu-id="233ca-129">Seznam všech dostupných imagí pro dané vydání:</span><span class="sxs-lookup"><span data-stu-id="233ca-129">List all available images for a given release:</span></span>

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

<span data-ttu-id="233ca-130">Další příklady vyhledávání a použití dostupných imagí najdete v tématu [vyhledání a výběr imagí virtuálních počítačů Azure pomocí rozhraní příkazového řádku Azure hello](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="233ca-130">For more examples on browsing and using available images, see [Navigate and select Azure virtual machine images with hello Azure CLI](cli-ps-findimage.md).</span></span>

<span data-ttu-id="233ca-131">Hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz má aliasy, můžete přístup tooquickly hello častější distribucích a o jejich nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="233ca-131">hello [az vm create](/cli/azure/vm#create) command has aliases you can use tooquickly access hello more common distros and their latest releases.</span></span> <span data-ttu-id="233ca-132">Použití aliasy je často rychlejší než zadání hello vydavatele, nabídky, SKU a verzi pokaždé, když vytvoření virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="233ca-132">Using aliases is often quicker than specifying hello publisher, offer, SKU, and version each time you create a VM:</span></span>

| <span data-ttu-id="233ca-133">Alias</span><span class="sxs-lookup"><span data-stu-id="233ca-133">Alias</span></span> | <span data-ttu-id="233ca-134">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="233ca-134">Publisher</span></span> | <span data-ttu-id="233ca-135">Nabídka</span><span class="sxs-lookup"><span data-stu-id="233ca-135">Offer</span></span> | <span data-ttu-id="233ca-136">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="233ca-136">SKU</span></span> | <span data-ttu-id="233ca-137">Verze</span><span class="sxs-lookup"><span data-stu-id="233ca-137">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="233ca-138">CentOS</span><span class="sxs-lookup"><span data-stu-id="233ca-138">CentOS</span></span> |<span data-ttu-id="233ca-139">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="233ca-139">OpenLogic</span></span> |<span data-ttu-id="233ca-140">Centos</span><span class="sxs-lookup"><span data-stu-id="233ca-140">Centos</span></span> |<span data-ttu-id="233ca-141">7.2</span><span class="sxs-lookup"><span data-stu-id="233ca-141">7.2</span></span> |<span data-ttu-id="233ca-142">nejnovější</span><span class="sxs-lookup"><span data-stu-id="233ca-142">latest</span></span> |
| <span data-ttu-id="233ca-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="233ca-143">CoreOS</span></span> |<span data-ttu-id="233ca-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="233ca-144">CoreOS</span></span> |<span data-ttu-id="233ca-145">CoreOS</span><span class="sxs-lookup"><span data-stu-id="233ca-145">CoreOS</span></span> |<span data-ttu-id="233ca-146">Stable</span><span class="sxs-lookup"><span data-stu-id="233ca-146">Stable</span></span> |<span data-ttu-id="233ca-147">nejnovější</span><span class="sxs-lookup"><span data-stu-id="233ca-147">latest</span></span> |
| <span data-ttu-id="233ca-148">Debian</span><span class="sxs-lookup"><span data-stu-id="233ca-148">Debian</span></span> |<span data-ttu-id="233ca-149">credativ</span><span class="sxs-lookup"><span data-stu-id="233ca-149">credativ</span></span> |<span data-ttu-id="233ca-150">Debian</span><span class="sxs-lookup"><span data-stu-id="233ca-150">Debian</span></span> |<span data-ttu-id="233ca-151">8</span><span class="sxs-lookup"><span data-stu-id="233ca-151">8</span></span> |<span data-ttu-id="233ca-152">nejnovější</span><span class="sxs-lookup"><span data-stu-id="233ca-152">latest</span></span> |
| <span data-ttu-id="233ca-153">openSUSE</span><span class="sxs-lookup"><span data-stu-id="233ca-153">openSUSE</span></span> |<span data-ttu-id="233ca-154">SUSE</span><span class="sxs-lookup"><span data-stu-id="233ca-154">SUSE</span></span> |<span data-ttu-id="233ca-155">openSUSE</span><span class="sxs-lookup"><span data-stu-id="233ca-155">openSUSE</span></span> |<span data-ttu-id="233ca-156">13.2</span><span class="sxs-lookup"><span data-stu-id="233ca-156">13.2</span></span> |<span data-ttu-id="233ca-157">nejnovější</span><span class="sxs-lookup"><span data-stu-id="233ca-157">latest</span></span> |
| <span data-ttu-id="233ca-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="233ca-158">RHEL</span></span> |<span data-ttu-id="233ca-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="233ca-159">Redhat</span></span> |<span data-ttu-id="233ca-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="233ca-160">RHEL</span></span> |<span data-ttu-id="233ca-161">7.2</span><span class="sxs-lookup"><span data-stu-id="233ca-161">7.2</span></span> |<span data-ttu-id="233ca-162">nejnovější</span><span class="sxs-lookup"><span data-stu-id="233ca-162">latest</span></span> |
| <span data-ttu-id="233ca-163">SLES</span><span class="sxs-lookup"><span data-stu-id="233ca-163">SLES</span></span> |<span data-ttu-id="233ca-164">SLES</span><span class="sxs-lookup"><span data-stu-id="233ca-164">SLES</span></span> |<span data-ttu-id="233ca-165">SLES</span><span class="sxs-lookup"><span data-stu-id="233ca-165">SLES</span></span> |<span data-ttu-id="233ca-166">12-SP1</span><span class="sxs-lookup"><span data-stu-id="233ca-166">12-SP1</span></span> |<span data-ttu-id="233ca-167">nejnovější</span><span class="sxs-lookup"><span data-stu-id="233ca-167">latest</span></span> |
| <span data-ttu-id="233ca-168">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="233ca-168">UbuntuLTS</span></span> |<span data-ttu-id="233ca-169">Canonical</span><span class="sxs-lookup"><span data-stu-id="233ca-169">Canonical</span></span> |<span data-ttu-id="233ca-170">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="233ca-170">UbuntuServer</span></span> |<span data-ttu-id="233ca-171">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="233ca-171">14.04.4-LTS</span></span> |<span data-ttu-id="233ca-172">nejnovější</span><span class="sxs-lookup"><span data-stu-id="233ca-172">latest</span></span> |

### <a name="use-your-own-image"></a><span data-ttu-id="233ca-173">Použití vlastní image</span><span class="sxs-lookup"><span data-stu-id="233ca-173">Use your own image</span></span>
<span data-ttu-id="233ca-174">Pokud budete potřebovat image se zvláštními úpravami, můžete zachytit stávající virtuální počítač a použít image založenou na tomto virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="233ca-174">If you require specific customizations, you can use an image based on an existing Azure VM by capturing that VM.</span></span> <span data-ttu-id="233ca-175">Můžete také nahrát místně vytvořenou image.</span><span class="sxs-lookup"><span data-stu-id="233ca-175">You can also upload an image created on-premises.</span></span> <span data-ttu-id="233ca-176">Další informace o podporovaných distribucích a jak toouse vlastní Image, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="233ca-176">For more information on supported distros and how toouse your own images, see hello following articles:</span></span>

* [<span data-ttu-id="233ca-177">Distribuce schválené pro Azure</span><span class="sxs-lookup"><span data-stu-id="233ca-177">Azure endorsed distributions</span></span>](endorsed-distros.md)
* [<span data-ttu-id="233ca-178">Informace pro neschválené distribuce</span><span class="sxs-lookup"><span data-stu-id="233ca-178">Information for non-endorsed distributions</span></span>](create-upload-generic.md)
* <span data-ttu-id="233ca-179">[Jak toocreate bitové kopie ze stávajícího virtuálního počítače Azure](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="233ca-179">[How toocreate an image from an existing Azure VM](tutorial-custom-images.md).</span></span>
  
  * <span data-ttu-id="233ca-180">Příklad úvodní příkazy toocreate bitové kopie ze stávajícího virtuálního počítače Azure:</span><span class="sxs-lookup"><span data-stu-id="233ca-180">Quick-start example commands toocreate an image from an existing Azure VM:</span></span>
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a><span data-ttu-id="233ca-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="233ca-181">Next steps</span></span>
* <span data-ttu-id="233ca-182">Vytvoření virtuálního počítače s Linuxem pomocí hello [rozhraní příkazového řádku](quick-create-cli.md), z hello [portál](quick-create-portal.md), nebo pomocí [šablony Azure Resource Manageru](../windows/cli-deploy-templates.md).</span><span class="sxs-lookup"><span data-stu-id="233ca-182">Create a Linux VM with hello [CLI](quick-create-cli.md), from hello [portal](quick-create-portal.md), or using an [Azure Resource Manager template](../windows/cli-deploy-templates.md).</span></span>
* <span data-ttu-id="233ca-183">Po vytvoření virtuálního počítače s Linuxem [si přečtěte o discích a úložišti Azure](tutorial-manage-disks.md).</span><span class="sxs-lookup"><span data-stu-id="233ca-183">After creating a Linux VM, [learn about Azure disks and storage](tutorial-manage-disks.md).</span></span>
* <span data-ttu-id="233ca-184">Rychlé kroky příliš[resetování hesla nebo klíčů SSH a spravujte uživatele](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="233ca-184">Quick steps too[reset a password or SSH keys and manage users](using-vmaccess-extension.md).</span></span>
