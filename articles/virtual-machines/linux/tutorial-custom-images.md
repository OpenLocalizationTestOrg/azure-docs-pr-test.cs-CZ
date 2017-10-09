---
title: "aaaCreate vlastní Image virtuálních počítačů s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – vytvoření vlastní image virtuálního počítače pomocí hello rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: 217a993c0c1d48939b74108ac6c5f7a1a619416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-hello-cli"></a><span data-ttu-id="03da7-103">Vytvořit vlastní image virtuálního počítače Azure pomocí hello rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="03da7-103">Create a custom image of an Azure VM using hello CLI</span></span>

<span data-ttu-id="03da7-104">Vlastní Image jsou jako obrázky marketplace, ale můžete vytvořit sami.</span><span class="sxs-lookup"><span data-stu-id="03da7-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="03da7-105">Vlastní Image může být použité toobootstrap konfigurace, jako je například předběžného načítání aplikace, konfigurace aplikace a další konfigurace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="03da7-105">Custom images can be used toobootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="03da7-106">V tomto kurzu vytvoříte svoji vlastní image virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="03da7-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="03da7-107">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="03da7-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="03da7-108">Zrušení zřízení a generalize virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="03da7-108">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="03da7-109">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="03da7-109">Create a custom image</span></span>
> * <span data-ttu-id="03da7-110">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="03da7-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="03da7-111">Zobrazí seznam všech hello bitové kopie v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="03da7-111">List all hello images in your subscription</span></span>
> * <span data-ttu-id="03da7-112">Odstranit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="03da7-112">Delete an image</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="03da7-113">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="03da7-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="03da7-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="03da7-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="03da7-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="03da7-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="03da7-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="03da7-116">Before you begin</span></span>

<span data-ttu-id="03da7-117">Následující postup Hello podrobnosti, jak tootake existující virtuální počítač a zapněte ji do opakovaně použitelné vlastní image, které můžete použít toocreate nové instance virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="03da7-117">hello steps below detail how tootake an existing VM and turn it into a re-usable custom image that you can use toocreate new VM instances.</span></span>

<span data-ttu-id="03da7-118">Příklad hello toocomplete v tomto kurzu, musí mít existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="03da7-118">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="03da7-119">V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="03da7-119">If needed, this [script sample](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) can create one for you.</span></span> <span data-ttu-id="03da7-120">Při absolvování hello kurzu, nahraďte hello skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.</span><span class="sxs-lookup"><span data-stu-id="03da7-120">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

## <a name="create-a-custom-image"></a><span data-ttu-id="03da7-121">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="03da7-121">Create a custom image</span></span>

<span data-ttu-id="03da7-122">toocreate bitové kopie virtuálního počítače, musíte tooprepare hello virtuálních počítačů zrušení zřízení, rušení přidělení a pak označením hello zdrojového virtuálního počítače jako zobecněn.</span><span class="sxs-lookup"><span data-stu-id="03da7-122">toocreate an image of a virtual machine, you need tooprepare hello VM by deprovisioning, deallocating, and then marking hello source VM as generalized.</span></span> <span data-ttu-id="03da7-123">Jednou hello, kterou virtuální počítač připravený, můžete vytvořit bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="03da7-123">Once hello VM has been prepared, you can create an image.</span></span>

### <a name="deprovision-hello-vm"></a><span data-ttu-id="03da7-124">Zrušení zřízení hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="03da7-124">Deprovision hello VM</span></span> 

<span data-ttu-id="03da7-125">Zrušení zřízení umožňuje zobecnit hello virtuálních počítačů odebráním informace specifické pro počítač.</span><span class="sxs-lookup"><span data-stu-id="03da7-125">Deprovisioning generalizes hello VM by removing machine-specific information.</span></span> <span data-ttu-id="03da7-126">Díky této generalizace je možné toodeploy mnoho virtuálních počítačů z jedné bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="03da7-126">This generalization makes it possible toodeploy many VMs from a single image.</span></span> <span data-ttu-id="03da7-127">Během zrušení zřízení, název hostitele hello resetovat příliš*localhost.localdomain*.</span><span class="sxs-lookup"><span data-stu-id="03da7-127">During deprovisioning, hello host name is reset too*localhost.localdomain*.</span></span> <span data-ttu-id="03da7-128">Klíče hostitele SSH, konfigurace názvový server, kořenové heslo a uložené v mezipaměti zapůjčení DHCP se také odstraní.</span><span class="sxs-lookup"><span data-stu-id="03da7-128">SSH host keys, nameserver configurations, root password, and cached DHCP leases are also deleted.</span></span>

<span data-ttu-id="03da7-129">toodeprovision hello virtuálních počítačů, použijte agenta virtuálního počítače Azure hello (příkaz waagent).</span><span class="sxs-lookup"><span data-stu-id="03da7-129">toodeprovision hello VM, use hello Azure VM agent (waagent).</span></span> <span data-ttu-id="03da7-130">agent virtuálního počítače Azure Hello je nainstalován na hello virtuálních počítačů a spravuje zřizování a interakci s hello Kontroleru prostředků infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="03da7-130">hello Azure VM agent is installed on hello VM and manages provisioning and interacting with hello Azure Fabric Controller.</span></span> <span data-ttu-id="03da7-131">Další informace najdete v tématu hello [Azure Linux Agent uživatelská příručka](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="03da7-131">For more information, see hello [Azure Linux Agent user guide](agent-user-guide.md).</span></span>

<span data-ttu-id="03da7-132">Připojit tooyour virtuálních počítačů pomocí protokolu SSH a spuštění hello příkaz toodeprovision hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="03da7-132">Connect tooyour VM using SSH and run hello command toodeprovision hello VM.</span></span> <span data-ttu-id="03da7-133">S hello `+user` argument, hello poslední zřízené uživatelský účet a všechna související data se také odstraní.</span><span class="sxs-lookup"><span data-stu-id="03da7-133">With hello `+user` argument, hello last provisioned user account and any associated data are also deleted.</span></span> <span data-ttu-id="03da7-134">Nahraďte hello příklad IP adresu hello veřejnou IP adresu vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="03da7-134">Replace hello example IP address with hello public IP address of your VM.</span></span>

<span data-ttu-id="03da7-135">SSH toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="03da7-135">SSH toohello VM.</span></span>
```bash
ssh azureuser@52.174.34.95
```
<span data-ttu-id="03da7-136">Zrušení zřízení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="03da7-136">Deprovision hello VM.</span></span>

```bash
sudo waagent -deprovision+user -force
```
<span data-ttu-id="03da7-137">Zavřete relace SSH hello.</span><span class="sxs-lookup"><span data-stu-id="03da7-137">Close hello SSH session.</span></span>

```bash
exit
```

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a><span data-ttu-id="03da7-138">Deallocate a označit hello jako zobecněný virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="03da7-138">Deallocate and mark hello VM as generalized</span></span>

<span data-ttu-id="03da7-139">toocreate bitovou kopii, hello virtuálních počítačů musí toobe navrácena.</span><span class="sxs-lookup"><span data-stu-id="03da7-139">toocreate an image, hello VM needs toobe deallocated.</span></span> <span data-ttu-id="03da7-140">Deallocate – hello virtuální počítač pomocí [az OM deallocate](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="03da7-140">Deallocate hello VM using [az vm deallocate](/cli//azure/vm#deallocate).</span></span> 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="03da7-141">Nakonec nastavit stav hello hello virtuálního počítače jako zobecněn s [az virtuálních počítačů zobecní](/cli//azure/vm#generalize) tak hello platformy Azure zná hello zobecněný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="03da7-141">Finally, set hello state of hello VM as generalized with [az vm generalize](/cli//azure/vm#generalize) so hello Azure platform knows hello VM has been generalized.</span></span> <span data-ttu-id="03da7-142">Bitovou kopii lze vytvořit pouze z zobecněný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="03da7-142">You can only create an image from a generalized VM.</span></span>
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-hello-image"></a><span data-ttu-id="03da7-143">Vytvoření bitové kopie hello</span><span class="sxs-lookup"><span data-stu-id="03da7-143">Create hello image</span></span>

<span data-ttu-id="03da7-144">Nyní můžete vytvořit image hello virtuálních počítačů pomocí [vytvoření bitové kopie az](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="03da7-144">Now you can create an image of hello VM by using [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="03da7-145">Hello následující příklad vytvoří bitovou kopii s názvem *myImage* z virtuálního počítače s názvem *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="03da7-145">hello following example creates an image named *myImage* from a VM named *myVM*.</span></span>
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-hello-image"></a><span data-ttu-id="03da7-146">Vytvořit virtuální počítače z hello image</span><span class="sxs-lookup"><span data-stu-id="03da7-146">Create VMs from hello image</span></span>

<span data-ttu-id="03da7-147">Teď, když máte image, můžete vytvořit jeden nebo více nové virtuální počítače pomocí bitové kopie hello [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="03da7-147">Now that you have an image, you can create one or more new VMs from hello image using [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="03da7-148">Hello následující příklad vytvoří virtuální počítač s názvem *myVMfromImage* z hello image s názvem *myImage*.</span><span class="sxs-lookup"><span data-stu-id="03da7-148">hello following example creates a VM named *myVMfromImage* from hello image named *myImage*.</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a><span data-ttu-id="03da7-149">Správy bitových kopií</span><span class="sxs-lookup"><span data-stu-id="03da7-149">Image management</span></span> 

<span data-ttu-id="03da7-150">Zde jsou některé příklady běžné úlohy správy bitové kopie a jak toocomplete je pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="03da7-150">Here are some examples of common image management tasks and how toocomplete them using hello Azure CLI.</span></span>

<span data-ttu-id="03da7-151">Zobrazí seznam všech Image podle názvu ve formátu tabulky.</span><span class="sxs-lookup"><span data-stu-id="03da7-151">List all images by name in a table format.</span></span>

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

<span data-ttu-id="03da7-152">Odstraňte bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="03da7-152">Delete an image.</span></span> <span data-ttu-id="03da7-153">Tento příklad odstranění hello image s názvem *myOldImage* z hello *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="03da7-153">This example deletes hello image named *myOldImage* from hello *myResourceGroup*.</span></span>

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="03da7-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="03da7-154">Next steps</span></span>

<span data-ttu-id="03da7-155">V tomto kurzu jste vytvořili vlastní image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="03da7-155">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="03da7-156">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="03da7-156">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="03da7-157">Zrušení zřízení a generalize virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="03da7-157">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="03da7-158">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="03da7-158">Create a custom image</span></span>
> * <span data-ttu-id="03da7-159">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="03da7-159">Create a VM from a custom image</span></span>
> * <span data-ttu-id="03da7-160">Zobrazí seznam všech hello bitové kopie v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="03da7-160">List all hello images in your subscription</span></span>
> * <span data-ttu-id="03da7-161">Odstranit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="03da7-161">Delete an image</span></span>

<span data-ttu-id="03da7-162">Posunutí další kurz toolearn toohello o vysoce dostupných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="03da7-162">Advance toohello next tutorial toolearn about highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="03da7-163">[Vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="03da7-163">[Create highly available VMs](tutorial-availability-sets.md).</span></span>

