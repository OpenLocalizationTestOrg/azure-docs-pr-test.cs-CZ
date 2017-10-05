---
title: "Vytvořte bitovou kopii virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku 2.0 | Microsoft Docs"
description: "Vytvořte bitovou kopii virtuálního počítače Azure pro velkokapacitní nasazení pomocí Azure CLI 2.0."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 19b573f77f2ee84600955d00d30bdb16c84e3623
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a><span data-ttu-id="61647-103">Postup vytvoření bitové kopie virtuálního počítače nebo virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="61647-103">How to create an image of a virtual machine or VHD</span></span>

<!-- generalize, image - extended version of the tutorial-->

<span data-ttu-id="61647-104">Pokud chcete vytvořit více kopií virtuálního počítače (VM) pro použití v Azure, vytvořte bitovou kopii virtuálního počítače nebo virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="61647-104">To create multiple copies of a virtual machine (VM) to use in Azure, capture an image of the VM or the OS VHD.</span></span> <span data-ttu-id="61647-105">Pokud chcete vytvořit bitovou kopii, je nutné odebrat informace osobního účtu, takže je bezpečnější nasadit více než jednou..</span><span class="sxs-lookup"><span data-stu-id="61647-105">To create an image, you need remove personal account information which makes it safer to deploy multiple times.</span></span> <span data-ttu-id="61647-106">V následujících krocích zrušení zřízení existující virtuální počítač, navrácení a vytvořit bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="61647-106">In the following steps you deprovision an existing VM, deallocate and create an image.</span></span> <span data-ttu-id="61647-107">Tuto bitovou kopii můžete použít k vytvoření virtuálních počítačů v libovolné skupině prostředků v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="61647-107">You can use this image to create VMs across any resource group within your subscription.</span></span>

<span data-ttu-id="61647-108">Pokud chcete vytvořit kopii existující virtuální počítač Linux zálohování nebo ladění, nebo nahrát specializované Linux virtuální pevný disk z virtuálního počítače místní najdete v tématu [nahrát a vytvoření virtuálního počítače s Linuxem z bitové kopie disku vlastní](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="61647-108">If you want to create a copy of your existing Linux VM for backup or debugging, or upload a specialized Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md).</span></span>  

<span data-ttu-id="61647-109">Můžete také použít **balírna** k vytvoření vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="61647-109">You can also use **Packer** to create your custom configuration.</span></span> <span data-ttu-id="61647-110">Další informace o používání balírna najdete v tématu [postup k vytvoření bitové kopie virtuálních počítačů Linux v Azure použijte balírna](build-image-with-packer.md).</span><span class="sxs-lookup"><span data-stu-id="61647-110">For more information on using Packer, see [How to use Packer to create Linux virtual machine images in Azure](build-image-with-packer.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="61647-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="61647-111">Before you begin</span></span>
<span data-ttu-id="61647-112">Ujistěte se, že splňujete následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="61647-112">Ensure that you meet the following prerequisites:</span></span>

* <span data-ttu-id="61647-113">Je nutné virtuální počítač Azure vytvořené v modelu nasazení Resource Manager pomocí spravovaných disků.</span><span class="sxs-lookup"><span data-stu-id="61647-113">You need an Azure VM created in the Resource Manager deployment model using managed disks.</span></span> <span data-ttu-id="61647-114">Pokud jste nevytvořili virtuální počítač s Linuxem, můžete použít [portál](quick-create-portal.md), [rozhraní příkazového řádku Azure](quick-create-cli.md), nebo [šablony Resource Manageru](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="61647-114">If you haven't created a Linux VM, you can use the [portal](quick-create-portal.md), the [Azure CLI](quick-create-cli.md), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> <span data-ttu-id="61647-115">Podle potřeby nakonfigurujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="61647-115">Configure the VM as needed.</span></span> <span data-ttu-id="61647-116">Například [přidat datových disků](add-disk.md), aktualizace a instalovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="61647-116">For example, [add data disks](add-disk.md), apply updates, and install applications.</span></span> 

* <span data-ttu-id="61647-117">Také je potřeba mít nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a být přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="61647-117">You also need to have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and be logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="61647-118">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="61647-118">Quick commands</span></span>

<span data-ttu-id="61647-119">Zjednodušené verzi tohoto tématu pro testování, vyhodnocení nebo získávání informací o virtuálních počítačů v Azure, naleznete v části [vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="61647-119">For a simplified version of this topic, for testing, evaluating or learning about VMs in Azure, see [Create a custom image of an Azure VM using the CLI](tutorial-custom-images.md).</span></span>


## <a name="step-1-deprovision-the-vm"></a><span data-ttu-id="61647-120">Krok 1: Zrušení zřízení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="61647-120">Step 1: Deprovision the VM</span></span>
<span data-ttu-id="61647-121">Můžete zrušit jejich zřízení virtuálního počítače pomocí agenta virtuálního počítače Azure se odstranit počítače konkrétní soubory a data.</span><span class="sxs-lookup"><span data-stu-id="61647-121">You deprovision the VM, using the Azure VM agent, to delete machine specific files and data.</span></span> <span data-ttu-id="61647-122">Použití `waagent` s *-deprovision + uživatele* parametr na svůj zdroj virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="61647-122">Use the `waagent` command with the *-deprovision+user* parameter on your source Linux VM.</span></span> <span data-ttu-id="61647-123">Další informace najdete v tématu [Azure Linux Agent uživatelská příručka](../windows/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="61647-123">For more information, see the [Azure Linux Agent user guide](../windows/agent-user-guide.md).</span></span>

1. <span data-ttu-id="61647-124">Připojte k virtuálním počítačům s Linuxem pomocí klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="61647-124">Connect to your Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="61647-125">V okně SSH zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="61647-125">In the SSH window, type the following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > <span data-ttu-id="61647-126">Tento příkaz lze spusťte pouze na virtuální počítač, který máte v úmyslu bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="61647-126">Only run this command on a VM that you intend to capture as an image.</span></span> <span data-ttu-id="61647-127">Není zaručeno, že bitovou kopii vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci.</span><span class="sxs-lookup"><span data-stu-id="61647-127">It does not guarantee that the image is cleared of all sensitive information or is suitable for redistribution.</span></span> <span data-ttu-id="61647-128">*+ Uživatele* parametr také odebere poslední účet zřízení uživatele.</span><span class="sxs-lookup"><span data-stu-id="61647-128">The *+user* parameter also removes the last provisioned user account.</span></span> <span data-ttu-id="61647-129">Pokud chcete zachovat přihlašovací údaje účtu ve virtuálním počítači, použijte *-deprovision* opustit uživatelský účet na místě.</span><span class="sxs-lookup"><span data-stu-id="61647-129">If you want to keep account credentials in the VM, just use *-deprovision* to leave the user account in place.</span></span>
 
3. <span data-ttu-id="61647-130">Typ **y** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="61647-130">Type **y** to continue.</span></span> <span data-ttu-id="61647-131">Můžete přidat **-force** parametr předejdete tento krok potvrzení.</span><span class="sxs-lookup"><span data-stu-id="61647-131">You can add the **-force** parameter to avoid this confirmation step.</span></span>
4. <span data-ttu-id="61647-132">Po dokončení příkazu, zadejte **ukončete**.</span><span class="sxs-lookup"><span data-stu-id="61647-132">After the command completes, type **exit**.</span></span> <span data-ttu-id="61647-133">Tento krok zavře použije klient SSH.</span><span class="sxs-lookup"><span data-stu-id="61647-133">This step closes the SSH client.</span></span>

## <a name="step-2-create-vm-image"></a><span data-ttu-id="61647-134">Krok 2: Vytvoření image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="61647-134">Step 2: Create VM image</span></span>
<span data-ttu-id="61647-135">Pomocí Azure CLI 2.0 označit virtuální počítač jako zobecněn a zachycení bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="61647-135">Use the Azure CLI 2.0 to mark the VM as generalized and capture the image.</span></span> <span data-ttu-id="61647-136">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="61647-136">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="61647-137">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="61647-137">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

1. <span data-ttu-id="61647-138">Zrušit přidělení virtuálního počítače, který jste se zrušit [az OM deallocate](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="61647-138">Deallocate the VM that you deprovisioned with [az vm deallocate](/cli//azure/vm#deallocate).</span></span> <span data-ttu-id="61647-139">Následující příklad zruší přidělení virtuálního počítače s názvem *Můjvp* ve skupině prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="61647-139">The following example deallocates the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. <span data-ttu-id="61647-140">Označit virtuální počítač jako zobecněn s [az virtuálních počítačů zobecní](/cli//azure/vm#generalize).</span><span class="sxs-lookup"><span data-stu-id="61647-140">Mark the VM as generalized with [az vm generalize](/cli//azure/vm#generalize).</span></span> <span data-ttu-id="61647-141">Následující příklad značky virtuální počítač s názvem *Můjvp* ve skupině prostředků s názvem *myResourceGroup* jako zobecněn:</span><span class="sxs-lookup"><span data-stu-id="61647-141">The following example marks the the VM named *myVM* in the resource group named *myResourceGroup* as generalized:</span></span>
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. <span data-ttu-id="61647-142">Nyní vytvoření bitové kopie prostředků virtuálního počítače s [vytvoření bitové kopie az](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="61647-142">Now create an image of the VM resource with [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="61647-143">Následující příklad vytvoří bitovou kopii s názvem *myImage* ve skupině prostředků s názvem *myResourceGroup* pomocí prostředků virtuálního počítače s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="61647-143">The following example creates an image named *myImage* in the resource group named *myResourceGroup* using the VM resource named *myVM*:</span></span>
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > <span data-ttu-id="61647-144">Obrázek se vytvoří ve stejné skupině prostředků jako vašeho zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="61647-144">The image is created in the same resource group as your source VM.</span></span> <span data-ttu-id="61647-145">Virtuální počítače můžete vytvořit v libovolné skupině prostředků v rámci vašeho předplatného z této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="61647-145">You can create VMs in any resource group within your subscription from this image.</span></span> <span data-ttu-id="61647-146">Z hlediska správy můžete chtít vytvořit skupinu prostředků specifické pro vaše prostředky virtuálních počítačů a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="61647-146">From a management perspective, you may wish to create a specific resource group for your VM resources and images.</span></span>

## <a name="step-3-create-a-vm-from-the-captured-image"></a><span data-ttu-id="61647-147">Krok 3: Vytvoření virtuálního počítače ze zaznamenané bitové kopie</span><span class="sxs-lookup"><span data-stu-id="61647-147">Step 3: Create a VM from the captured image</span></span>
<span data-ttu-id="61647-148">Vytvoření virtuálního počítače pomocí bitové kopie vytvořené pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="61647-148">Create a VM using the image you created with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="61647-149">Následující příklad vytvoří virtuální počítač s názvem *myVMDeployed* z bitové kopie s názvem *myImage*:</span><span class="sxs-lookup"><span data-stu-id="61647-149">The following example creates a VM named *myVMDeployed* from the image named *myImage*:</span></span>

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a><span data-ttu-id="61647-150">Vytvoření virtuálního počítače v jiné skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="61647-150">Creating the VM in another resource group</span></span> 

<span data-ttu-id="61647-151">Virtuální počítače můžete vytvořit z image v libovolné skupině prostředků v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="61647-151">You can create VMs from an image in any resource group within your subscription.</span></span> <span data-ttu-id="61647-152">Pokud chcete vytvořit virtuální počítač v jiné skupině prostředků než bitovou kopii, zadejte je úplné ID prostředku do bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="61647-152">To create a VM in a different resource group than the image, specify the full resource ID to your image.</span></span> <span data-ttu-id="61647-153">Použití [seznamu obrázků az](/cli/azure/image#list) zobrazení seznamu obrázků.</span><span class="sxs-lookup"><span data-stu-id="61647-153">Use [az image list](/cli/azure/image#list) to view a list of images.</span></span> <span data-ttu-id="61647-154">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="61647-154">The output is similar to the following example:</span></span>

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

<span data-ttu-id="61647-155">Následující příklad používá [vytvořit virtuální počítač az](/cli/azure/vm#create) vytvoření virtuálního počítače v jiné skupině prostředků než zdrojové bitové kopie zadáním ID prostředku bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="61647-155">The following example uses [az vm create](/cli/azure/vm#create) to create a VM in a different resource group than the source image by specifying the image resource ID:</span></span>

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a><span data-ttu-id="61647-156">Krok 4: Ověření nasazení</span><span class="sxs-lookup"><span data-stu-id="61647-156">Step 4: Verify the deployment</span></span>

<span data-ttu-id="61647-157">Nyní SSH k virtuálnímu počítači, který jste vytvořili pro ověření nasazení a začít používat nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="61647-157">Now SSH to the virtual machine you created to verify the deployment and start using the new VM.</span></span> <span data-ttu-id="61647-158">Pro připojení pomocí protokolu SSH, najít IP adresu nebo plně kvalifikovaný název domény vašeho virtuálního počítače s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="61647-158">To connect via SSH, find the IP address or FQDN of your VM with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a><span data-ttu-id="61647-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61647-159">Next steps</span></span>
<span data-ttu-id="61647-160">Můžete vytvořit víc virtuálních počítačů z vaší zdrojové bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="61647-160">You can create multiple VMs from your source VM image.</span></span> <span data-ttu-id="61647-161">Pokud potřebujete provést změny do bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="61647-161">If you need to make changes to your image:</span></span> 

- <span data-ttu-id="61647-162">Vytvořte virtuální počítač z bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="61647-162">Create a VM from your image.</span></span>
- <span data-ttu-id="61647-163">Nastavit všechny aktualizace nebo změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="61647-163">Make any updates or configuration changes.</span></span>
- <span data-ttu-id="61647-164">Postup opakujte zrušení zřízení, navrácení, generalize a vytvořte bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="61647-164">Follow the steps again to deprovision, deallocate, generalize, and create an image.</span></span>
- <span data-ttu-id="61647-165">Použijte tuto novou bitovou kopii pro budoucí nasazení.</span><span class="sxs-lookup"><span data-stu-id="61647-165">Use this new image for future deployments.</span></span> <span data-ttu-id="61647-166">V případě potřeby odstraňte původní bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="61647-166">If desired, delete the original image.</span></span>

<span data-ttu-id="61647-167">Další informace týkající se správy virtuálních počítačů pomocí rozhraní příkazového řádku najdete v tématu [Azure CLI 2.0](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="61647-167">For more information on managing your VMs with the CLI, see [Azure CLI 2.0](/cli/azure/overview).</span></span>
