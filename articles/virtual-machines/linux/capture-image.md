---
title: "aaaCapture bitové kopie virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku 2.0 | Microsoft Docs"
description: "Vytvořte bitovou kopii virtuálnímu počítači Azure toouse velkokapacitního nasazení pomocí hello 2.0 rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: 9558332a86186b282775097428df462709373012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-image-of-a-virtual-machine-or-vhd"></a><span data-ttu-id="f84d1-103">Jak toocreate bitové kopie virtuálního počítače nebo virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="f84d1-103">How toocreate an image of a virtual machine or VHD</span></span>

<!-- generalize, image - extended version of hello tutorial-->

<span data-ttu-id="f84d1-104">toocreate více kopií toouse virtuální počítač (VM) v Azure, vytvořte bitovou kopii hello virtuálního počítače nebo hello virtuálního pevného disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="f84d1-104">toocreate multiple copies of a virtual machine (VM) toouse in Azure, capture an image of hello VM or hello OS VHD.</span></span> <span data-ttu-id="f84d1-105">toocreate bitovou kopii, je nutné odebrat informace osobního účtu, takže je bezpečnější toodeploy vícekrát.</span><span class="sxs-lookup"><span data-stu-id="f84d1-105">toocreate an image, you need remove personal account information which makes it safer toodeploy multiple times.</span></span> <span data-ttu-id="f84d1-106">V hello postupu zrušení zřízení existující virtuální počítač, navrácení a vytvořit bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="f84d1-106">In hello following steps you deprovision an existing VM, deallocate and create an image.</span></span> <span data-ttu-id="f84d1-107">Můžete použít tento toocreate bitové kopie virtuálních počítačů v libovolné skupině prostředků v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="f84d1-107">You can use this image toocreate VMs across any resource group within your subscription.</span></span>

<span data-ttu-id="f84d1-108">Pokud chcete použít pro zálohování nebo ladění toocreate kopii vaše stávající virtuální počítač s Linuxem nebo nahrát specializované Linux virtuální pevný disk z virtuálního počítače místní, přečtěte si téma [nahrát a vytvoření virtuálního počítače s Linuxem z bitové kopie disku vlastní](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="f84d1-108">If you want toocreate a copy of your existing Linux VM for backup or debugging, or upload a specialized Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md).</span></span>  

<span data-ttu-id="f84d1-109">Můžete také použít **balírna** toocreate vlastní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f84d1-109">You can also use **Packer** toocreate your custom configuration.</span></span> <span data-ttu-id="f84d1-110">Další informace o používání balírna najdete v tématu [jak Image toouse balírna toocreate Linux virtuálního počítače v nástroji Azure](build-image-with-packer.md).</span><span class="sxs-lookup"><span data-stu-id="f84d1-110">For more information on using Packer, see [How toouse Packer toocreate Linux virtual machine images in Azure](build-image-with-packer.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="f84d1-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f84d1-111">Before you begin</span></span>
<span data-ttu-id="f84d1-112">Ujistěte se, že splňujete hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="f84d1-112">Ensure that you meet hello following prerequisites:</span></span>

* <span data-ttu-id="f84d1-113">Je nutné virtuální počítač Azure vytvořené v modelu nasazení Resource Manager hello pomocí spravovaných disků.</span><span class="sxs-lookup"><span data-stu-id="f84d1-113">You need an Azure VM created in hello Resource Manager deployment model using managed disks.</span></span> <span data-ttu-id="f84d1-114">Pokud jste nevytvořili virtuální počítač s Linuxem, můžete použít hello [portál](quick-create-portal.md), hello [rozhraní příkazového řádku Azure](quick-create-cli.md), nebo [šablony Resource Manageru](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="f84d1-114">If you haven't created a Linux VM, you can use hello [portal](quick-create-portal.md), hello [Azure CLI](quick-create-cli.md), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> <span data-ttu-id="f84d1-115">Nakonfigurujte hello virtuální počítač podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="f84d1-115">Configure hello VM as needed.</span></span> <span data-ttu-id="f84d1-116">Například [přidat datových disků](add-disk.md), aktualizace a instalovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="f84d1-116">For example, [add data disks](add-disk.md), apply updates, and install applications.</span></span> 

* <span data-ttu-id="f84d1-117">Musíte taky toohave hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a být přihlášeni pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="f84d1-117">You also need toohave hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and be logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="f84d1-118">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="f84d1-118">Quick commands</span></span>

<span data-ttu-id="f84d1-119">Zjednodušené verzi tohoto tématu pro testování, vyhodnocení nebo získávání informací o virtuálních počítačů v Azure, naleznete v části [vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku hello](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="f84d1-119">For a simplified version of this topic, for testing, evaluating or learning about VMs in Azure, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md).</span></span>


## <a name="step-1-deprovision-hello-vm"></a><span data-ttu-id="f84d1-120">Krok 1: Zrušení zřízení hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f84d1-120">Step 1: Deprovision hello VM</span></span>
<span data-ttu-id="f84d1-121">Můžete zrušit jejich zřízení hello virtuální počítač pomocí hello agenta virtuálního počítače Azure, toodelete počítače konkrétní soubory a data.</span><span class="sxs-lookup"><span data-stu-id="f84d1-121">You deprovision hello VM, using hello Azure VM agent, toodelete machine specific files and data.</span></span> <span data-ttu-id="f84d1-122">Použití hello `waagent` s hello *-deprovision + uživatele* parametr na svůj zdroj virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="f84d1-122">Use hello `waagent` command with hello *-deprovision+user* parameter on your source Linux VM.</span></span> <span data-ttu-id="f84d1-123">Další informace najdete v tématu hello [Azure Linux Agent uživatelská příručka](../windows/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="f84d1-123">For more information, see hello [Azure Linux Agent user guide](../windows/agent-user-guide.md).</span></span>

1. <span data-ttu-id="f84d1-124">Připojte tooyour virtuálního počítače s Linuxem pomocí klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="f84d1-124">Connect tooyour Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="f84d1-125">V okně hello SSH zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f84d1-125">In hello SSH window, type hello following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > <span data-ttu-id="f84d1-126">Spusťte pouze na virtuálním počítači tento příkaz, že máte v úmyslu toocapture jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="f84d1-126">Only run this command on a VM that you intend toocapture as an image.</span></span> <span data-ttu-id="f84d1-127">Nezaručuje se této bitové kopie hello vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci.</span><span class="sxs-lookup"><span data-stu-id="f84d1-127">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution.</span></span> <span data-ttu-id="f84d1-128">Hello *+ uživatele* parametr také odebere poslední účet zřízení uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="f84d1-128">hello *+user* parameter also removes hello last provisioned user account.</span></span> <span data-ttu-id="f84d1-129">Pokud chcete, aby tookeep přihlašovací údaje hello virtuálních počítačů, použijte *-deprovision* tooleave hello uživatelský účet na místě.</span><span class="sxs-lookup"><span data-stu-id="f84d1-129">If you want tookeep account credentials in hello VM, just use *-deprovision* tooleave hello user account in place.</span></span>
 
3. <span data-ttu-id="f84d1-130">Typ **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="f84d1-130">Type **y** toocontinue.</span></span> <span data-ttu-id="f84d1-131">Můžete přidat hello **-force** parametr tooavoid tento krok potvrzení.</span><span class="sxs-lookup"><span data-stu-id="f84d1-131">You can add hello **-force** parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="f84d1-132">Po dokončení příkazu hello zadejte **ukončete**.</span><span class="sxs-lookup"><span data-stu-id="f84d1-132">After hello command completes, type **exit**.</span></span> <span data-ttu-id="f84d1-133">Tento krok zavře klient SSH hello.</span><span class="sxs-lookup"><span data-stu-id="f84d1-133">This step closes hello SSH client.</span></span>

## <a name="step-2-create-vm-image"></a><span data-ttu-id="f84d1-134">Krok 2: Vytvoření image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f84d1-134">Step 2: Create VM image</span></span>
<span data-ttu-id="f84d1-135">Použijte hello Azure CLI 2.0 toomark hello virtuálního počítače jako zobecněn a zachycení bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="f84d1-135">Use hello Azure CLI 2.0 toomark hello VM as generalized and capture hello image.</span></span> <span data-ttu-id="f84d1-136">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="f84d1-136">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="f84d1-137">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="f84d1-137">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

1. <span data-ttu-id="f84d1-138">Deallocate hello virtuální počítač, který jste se zrušit [az OM deallocate](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="f84d1-138">Deallocate hello VM that you deprovisioned with [az vm deallocate](/cli//azure/vm#deallocate).</span></span> <span data-ttu-id="f84d1-139">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="f84d1-139">hello following example deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. <span data-ttu-id="f84d1-140">Označit hello virtuálního počítače jako zobecněn s [az virtuálních počítačů zobecní](/cli//azure/vm#generalize).</span><span class="sxs-lookup"><span data-stu-id="f84d1-140">Mark hello VM as generalized with [az vm generalize](/cli//azure/vm#generalize).</span></span> <span data-ttu-id="f84d1-141">Následující příklad značky hello hello virtuálních počítačů s názvem Hello *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup* jako zobecněn:</span><span class="sxs-lookup"><span data-stu-id="f84d1-141">hello following example marks hello hello VM named *myVM* in hello resource group named *myResourceGroup* as generalized:</span></span>
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. <span data-ttu-id="f84d1-142">Nyní vytvořit bitovou kopii hello prostředků virtuálního počítače s [vytvoření bitové kopie az](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="f84d1-142">Now create an image of hello VM resource with [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="f84d1-143">Hello následující příklad vytvoří bitovou kopii s názvem *myImage* v hello skupinu prostředků s názvem *myResourceGroup* pomocí hello prostředků virtuálního počítače s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="f84d1-143">hello following example creates an image named *myImage* in hello resource group named *myResourceGroup* using hello VM resource named *myVM*:</span></span>
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > <span data-ttu-id="f84d1-144">Hello bitové kopie je vytvořen v hello stejné skupině prostředků jako vašeho zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f84d1-144">hello image is created in hello same resource group as your source VM.</span></span> <span data-ttu-id="f84d1-145">Virtuální počítače můžete vytvořit v libovolné skupině prostředků v rámci vašeho předplatného z této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="f84d1-145">You can create VMs in any resource group within your subscription from this image.</span></span> <span data-ttu-id="f84d1-146">Z hlediska správy můžete toocreate určité skupiny zdrojů pro vaše prostředky virtuálních počítačů a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="f84d1-146">From a management perspective, you may wish toocreate a specific resource group for your VM resources and images.</span></span>

## <a name="step-3-create-a-vm-from-hello-captured-image"></a><span data-ttu-id="f84d1-147">Krok 3: Vytvoření virtuálního počítače z hello zachycení image</span><span class="sxs-lookup"><span data-stu-id="f84d1-147">Step 3: Create a VM from hello captured image</span></span>
<span data-ttu-id="f84d1-148">Vytvoření virtuálního počítače pomocí bitové kopie hello jste vytvořili pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="f84d1-148">Create a VM using hello image you created with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="f84d1-149">Hello následující příklad vytvoří virtuální počítač s názvem *myVMDeployed* z hello image s názvem *myImage*:</span><span class="sxs-lookup"><span data-stu-id="f84d1-149">hello following example creates a VM named *myVMDeployed* from hello image named *myImage*:</span></span>

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-hello-vm-in-another-resource-group"></a><span data-ttu-id="f84d1-150">Vytváření hello virtuálních počítačů v jiné skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="f84d1-150">Creating hello VM in another resource group</span></span> 

<span data-ttu-id="f84d1-151">Virtuální počítače můžete vytvořit z image v libovolné skupině prostředků v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="f84d1-151">You can create VMs from an image in any resource group within your subscription.</span></span> <span data-ttu-id="f84d1-152">toocreate virtuální počítač v jiné skupině prostředků než hello image, zadejte hello úplné prostředků ID tooyour bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="f84d1-152">toocreate a VM in a different resource group than hello image, specify hello full resource ID tooyour image.</span></span> <span data-ttu-id="f84d1-153">Použití [seznamu obrázků az](/cli/azure/image#list) tooview seznam bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="f84d1-153">Use [az image list](/cli/azure/image#list) tooview a list of images.</span></span> <span data-ttu-id="f84d1-154">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f84d1-154">hello output is similar toohello following example:</span></span>

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

<span data-ttu-id="f84d1-155">Hello následující příklad používá [vytvořit virtuální počítač az](/cli/azure/vm#create) toocreate virtuální počítač v jiné skupině prostředků než hello zdrojové bitové kopie zadáním hello ID prostředku bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="f84d1-155">hello following example uses [az vm create](/cli/azure/vm#create) toocreate a VM in a different resource group than hello source image by specifying hello image resource ID:</span></span>

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-hello-deployment"></a><span data-ttu-id="f84d1-156">Krok 4: Ověření nasazení hello</span><span class="sxs-lookup"><span data-stu-id="f84d1-156">Step 4: Verify hello deployment</span></span>

<span data-ttu-id="f84d1-157">Nyní SSH toohello virtuálního počítače jste vytvořili tooverify hello nasazení a spuštění pomocí hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f84d1-157">Now SSH toohello virtual machine you created tooverify hello deployment and start using hello new VM.</span></span> <span data-ttu-id="f84d1-158">tooconnect pomocí protokolu SSH, najít hello IP adresu nebo plně kvalifikovaný název domény vašeho virtuálního počítače s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="f84d1-158">tooconnect via SSH, find hello IP address or FQDN of your VM with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a><span data-ttu-id="f84d1-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f84d1-159">Next steps</span></span>
<span data-ttu-id="f84d1-160">Můžete vytvořit víc virtuálních počítačů z vaší zdrojové bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f84d1-160">You can create multiple VMs from your source VM image.</span></span> <span data-ttu-id="f84d1-161">Pokud budete potřebovat image tooyour toomake změny:</span><span class="sxs-lookup"><span data-stu-id="f84d1-161">If you need toomake changes tooyour image:</span></span> 

- <span data-ttu-id="f84d1-162">Vytvořte virtuální počítač z bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="f84d1-162">Create a VM from your image.</span></span>
- <span data-ttu-id="f84d1-163">Nastavit všechny aktualizace nebo změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f84d1-163">Make any updates or configuration changes.</span></span>
- <span data-ttu-id="f84d1-164">Postupujte podle hello kroky opakujte toodeprovision, navrácení, generalize a vytvořte bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="f84d1-164">Follow hello steps again toodeprovision, deallocate, generalize, and create an image.</span></span>
- <span data-ttu-id="f84d1-165">Použijte tuto novou bitovou kopii pro budoucí nasazení.</span><span class="sxs-lookup"><span data-stu-id="f84d1-165">Use this new image for future deployments.</span></span> <span data-ttu-id="f84d1-166">V případě potřeby odstraňte původní image hello.</span><span class="sxs-lookup"><span data-stu-id="f84d1-166">If desired, delete hello original image.</span></span>

<span data-ttu-id="f84d1-167">Další informace týkající se správy virtuálních počítačů s hello rozhraní příkazového řádku najdete v tématu [Azure CLI 2.0](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f84d1-167">For more information on managing your VMs with hello CLI, see [Azure CLI 2.0](/cli/azure/overview).</span></span>
