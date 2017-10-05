---
title: "Vytvoření vlastní Image virtuálních počítačů pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – vytvoření vlastní image virtuálního počítače pomocí rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/21/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d32980f05ad17a76793021d0a5355d597974a4e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a><span data-ttu-id="28a75-103">Vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="28a75-103">Create a custom image of an Azure VM using the CLI</span></span>

<span data-ttu-id="28a75-104">Vlastní Image jsou jako obrázky marketplace, ale můžete vytvořit sami.</span><span class="sxs-lookup"><span data-stu-id="28a75-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="28a75-105">Vlastní Image slouží k zavedení konfigurace, třeba předběžného načítání aplikace, konfigurace aplikace a další konfigurace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="28a75-105">Custom images can be used to bootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="28a75-106">V tomto kurzu vytvoříte svoji vlastní image virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="28a75-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="28a75-107">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="28a75-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="28a75-108">Zrušení zřízení a generalize virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="28a75-108">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="28a75-109">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="28a75-109">Create a custom image</span></span>
> * <span data-ttu-id="28a75-110">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="28a75-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="28a75-111">Seznam všechny bitové kopie v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="28a75-111">List all the images in your subscription</span></span>
> * <span data-ttu-id="28a75-112">Odstranit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="28a75-112">Delete an image</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="28a75-113">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="28a75-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="28a75-114">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="28a75-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="28a75-115">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="28a75-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="28a75-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="28a75-116">Before you begin</span></span>

<span data-ttu-id="28a75-117">Následující postup podrobnosti jak využít existující virtuální počítač a zapnout ho do opakovaně použitelné vlastní obrázek, který můžete použít k vytvoření nové instance virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28a75-117">The steps below detail how to take an existing VM and turn it into a re-usable custom image that you can use to create new VM instances.</span></span>

<span data-ttu-id="28a75-118">Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="28a75-118">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="28a75-119">V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="28a75-119">If needed, this [script sample](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) can create one for you.</span></span> <span data-ttu-id="28a75-120">Při absolvování kurzu, nahraďte skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.</span><span class="sxs-lookup"><span data-stu-id="28a75-120">When working through the tutorial, replace the resource group and VM names where needed.</span></span>

## <a name="create-a-custom-image"></a><span data-ttu-id="28a75-121">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="28a75-121">Create a custom image</span></span>

<span data-ttu-id="28a75-122">Chcete-li vytvořit bitovou kopii virtuálního počítače, je nutné připravit virtuální počítač zrušení zřízení, rušení přidělení a pak označením zdrojového virtuálního počítače jako zobecněn.</span><span class="sxs-lookup"><span data-stu-id="28a75-122">To create an image of a virtual machine, you need to prepare the VM by deprovisioning, deallocating, and then marking the source VM as generalized.</span></span> <span data-ttu-id="28a75-123">Jakmile je virtuální počítač připravený, můžete vytvořit bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="28a75-123">Once the VM has been prepared, you can create an image.</span></span>

### <a name="deprovision-the-vm"></a><span data-ttu-id="28a75-124">Zrušení zřízení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="28a75-124">Deprovision the VM</span></span> 

<span data-ttu-id="28a75-125">Zrušení zřízení umožňuje zobecnit virtuální počítač odebráním informace specifické pro počítač.</span><span class="sxs-lookup"><span data-stu-id="28a75-125">Deprovisioning generalizes the VM by removing machine-specific information.</span></span> <span data-ttu-id="28a75-126">Tato generalizace umožňuje nasadit mnoho virtuálních počítačů z jedné bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="28a75-126">This generalization makes it possible to deploy many VMs from a single image.</span></span> <span data-ttu-id="28a75-127">Během zrušení zřízení, název hostitele je obnovena na *localhost.localdomain*.</span><span class="sxs-lookup"><span data-stu-id="28a75-127">During deprovisioning, the host name is reset to *localhost.localdomain*.</span></span> <span data-ttu-id="28a75-128">Klíče hostitele SSH, konfigurace názvový server, kořenové heslo a uložené v mezipaměti zapůjčení DHCP se také odstraní.</span><span class="sxs-lookup"><span data-stu-id="28a75-128">SSH host keys, nameserver configurations, root password, and cached DHCP leases are also deleted.</span></span>

<span data-ttu-id="28a75-129">Chcete-li zrušit jejich zřízení virtuálního počítače, použijte agenta virtuálního počítače Azure (příkaz waagent).</span><span class="sxs-lookup"><span data-stu-id="28a75-129">To deprovision the VM, use the Azure VM agent (waagent).</span></span> <span data-ttu-id="28a75-130">Agent virtuálního počítače Azure je nainstalovaný na virtuálním počítači a spravuje zřizování a interakci s Kontroleru prostředků infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="28a75-130">The Azure VM agent is installed on the VM and manages provisioning and interacting with the Azure Fabric Controller.</span></span> <span data-ttu-id="28a75-131">Další informace najdete v tématu [Azure Linux Agent uživatelská příručka](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="28a75-131">For more information, see the [Azure Linux Agent user guide](agent-user-guide.md).</span></span>

<span data-ttu-id="28a75-132">Připojení k virtuálnímu počítači pomocí protokolu SSH a spusťte příkaz pro zrušení zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28a75-132">Connect to your VM using SSH and run the command to deprovision the VM.</span></span> <span data-ttu-id="28a75-133">Pomocí `+user` argument, poslední účet zřízení uživatele a všechna přidružená data budou také odstraněny.</span><span class="sxs-lookup"><span data-stu-id="28a75-133">With the `+user` argument, the last provisioned user account and any associated data are also deleted.</span></span> <span data-ttu-id="28a75-134">Nahraďte IP adresu příklad veřejnou IP adresu vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28a75-134">Replace the example IP address with the public IP address of your VM.</span></span>

<span data-ttu-id="28a75-135">SSH k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="28a75-135">SSH to the VM.</span></span>
```bash
ssh azureuser@52.174.34.95
```
<span data-ttu-id="28a75-136">Zrušení zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28a75-136">Deprovision the VM.</span></span>

```bash
sudo waagent -deprovision+user -force
```
<span data-ttu-id="28a75-137">Ukončení relace SSH.</span><span class="sxs-lookup"><span data-stu-id="28a75-137">Close the SSH session.</span></span>

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a><span data-ttu-id="28a75-138">Deallocate a označit virtuální počítač jako zobecněn</span><span class="sxs-lookup"><span data-stu-id="28a75-138">Deallocate and mark the VM as generalized</span></span>

<span data-ttu-id="28a75-139">Pokud chcete vytvořit bitovou kopii, virtuální počítač musí být navrácena.</span><span class="sxs-lookup"><span data-stu-id="28a75-139">To create an image, the VM needs to be deallocated.</span></span> <span data-ttu-id="28a75-140">Zrušit přidělení virtuálního počítače pomocí [az OM deallocate](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="28a75-140">Deallocate the VM using [az vm deallocate](/cli//azure/vm#deallocate).</span></span> 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="28a75-141">Nakonec nastavit stav virtuálního počítače jako zobecněn s [az virtuálních počítačů zobecní](/cli//azure/vm#generalize) tak platformy Azure zná zobecněný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="28a75-141">Finally, set the state of the VM as generalized with [az vm generalize](/cli//azure/vm#generalize) so the Azure platform knows the VM has been generalized.</span></span> <span data-ttu-id="28a75-142">Bitovou kopii lze vytvořit pouze z zobecněný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="28a75-142">You can only create an image from a generalized VM.</span></span>
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a><span data-ttu-id="28a75-143">Vytvoření bitové kopie</span><span class="sxs-lookup"><span data-stu-id="28a75-143">Create the image</span></span>

<span data-ttu-id="28a75-144">Nyní můžete vytvořit image virtuálního počítače pomocí [vytvoření bitové kopie az](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="28a75-144">Now you can create an image of the VM by using [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="28a75-145">Následující příklad vytvoří bitovou kopii s názvem *myImage* z virtuálního počítače s názvem *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="28a75-145">The following example creates an image named *myImage* from a VM named *myVM*.</span></span>
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a><span data-ttu-id="28a75-146">Vytvořit virtuální počítače z image</span><span class="sxs-lookup"><span data-stu-id="28a75-146">Create VMs from the image</span></span>

<span data-ttu-id="28a75-147">Teď, když máte image, můžete vytvořit jeden nebo více nové virtuální počítače z image pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="28a75-147">Now that you have an image, you can create one or more new VMs from the image using [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="28a75-148">Následující příklad vytvoří virtuální počítač s názvem *myVMfromImage* z bitové kopie s názvem *myImage*.</span><span class="sxs-lookup"><span data-stu-id="28a75-148">The following example creates a VM named *myVMfromImage* from the image named *myImage*.</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a><span data-ttu-id="28a75-149">Správy bitových kopií</span><span class="sxs-lookup"><span data-stu-id="28a75-149">Image management</span></span> 

<span data-ttu-id="28a75-150">Tady jsou některé příklady běžné úlohy správy bitové kopie a postup dokončete pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="28a75-150">Here are some examples of common image management tasks and how to complete them using the Azure CLI.</span></span>

<span data-ttu-id="28a75-151">Zobrazí seznam všech Image podle názvu ve formátu tabulky.</span><span class="sxs-lookup"><span data-stu-id="28a75-151">List all images by name in a table format.</span></span>

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

<span data-ttu-id="28a75-152">Odstraňte bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="28a75-152">Delete an image.</span></span> <span data-ttu-id="28a75-153">Tento příklad odstraní image s názvem *myOldImage* z *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="28a75-153">This example deletes the image named *myOldImage* from the *myResourceGroup*.</span></span>

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="28a75-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28a75-154">Next steps</span></span>

<span data-ttu-id="28a75-155">V tomto kurzu jste vytvořili vlastní image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28a75-155">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="28a75-156">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="28a75-156">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="28a75-157">Zrušení zřízení a generalize virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="28a75-157">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="28a75-158">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="28a75-158">Create a custom image</span></span>
> * <span data-ttu-id="28a75-159">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="28a75-159">Create a VM from a custom image</span></span>
> * <span data-ttu-id="28a75-160">Seznam všechny bitové kopie v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="28a75-160">List all the images in your subscription</span></span>
> * <span data-ttu-id="28a75-161">Odstranit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="28a75-161">Delete an image</span></span>

<span data-ttu-id="28a75-162">Přechodu na v dalším kurzu se dozvíte o vysoce dostupných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="28a75-162">Advance to the next tutorial to learn about highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="28a75-163">[Vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="28a75-163">[Create highly available VMs](tutorial-availability-sets.md).</span></span>

