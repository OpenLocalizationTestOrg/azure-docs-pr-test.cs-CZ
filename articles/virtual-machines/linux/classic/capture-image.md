---
title: "aaaCapture bitové kopie virtuálního počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak toocapture bitové kopie založené na systému Linux Azure virtuálního počítače (VM) vytvořené pomocí modelu nasazení classic hello."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 33c4059d5bb919a86bdc3492abca540750f365ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocapture-a-classic-linux-virtual-machine-as-an-image"></a><span data-ttu-id="9d566-103">Jak toocapture klasický Linuxový virtuální počítač jako image</span><span class="sxs-lookup"><span data-stu-id="9d566-103">How toocapture a classic Linux virtual machine as an image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9d566-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9d566-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9d566-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="9d566-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="9d566-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="9d566-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="9d566-107">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9d566-107">Learn how too[perform these steps using hello Resource Manager model](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="9d566-108">Tento článek ukazuje, jak toocapture classic Azure virtuálního počítače (VM) s Linuxem jako image toocreate jiné virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9d566-108">This article shows you how toocapture a classic Azure virtual machine (VM) running Linux as an image toocreate other virtual machines.</span></span> <span data-ttu-id="9d566-109">Tento image obsahuje hello disk operačního systému a datové disky připojené toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9d566-109">This image includes hello OS disk and data disks attached toohello VM.</span></span> <span data-ttu-id="9d566-110">Proto musíte tooconfigure neobsahuje síťové konfigurace, které při vytváření hello další virtuální počítač z bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="9d566-110">It doesn't include networking configuration, so you need tooconfigure that when you create hello other VM from hello image.</span></span>

<span data-ttu-id="9d566-111">Úložiště Azure hello bitové kopie v rámci **bitové kopie**, společně s všechny Image, které jste odeslali.</span><span class="sxs-lookup"><span data-stu-id="9d566-111">Azure stores hello image under **Images**, along with any images you've uploaded.</span></span> <span data-ttu-id="9d566-112">Další informace o bitových kopiích naleznete v tématu [o Image virtuálních počítačů v Azure][About Virtual Machine Images in Azure].</span><span class="sxs-lookup"><span data-stu-id="9d566-112">For more information about images, see [About Virtual Machine Images in Azure][About Virtual Machine Images in Azure].</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9d566-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="9d566-113">Before you begin</span></span>
<span data-ttu-id="9d566-114">Tento postup předpokládá, že jste už vytvořili pomocí modelu nasazení Classic hello a nakonfigurované hello operačního systému, včetně připojení jakýchkoli datových disků virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9d566-114">These steps assume that you've already created an Azure VM using hello Classic deployment model and configured hello operating system, including attaching any data disks.</span></span> <span data-ttu-id="9d566-115">Pokud potřebujete toocreate virtuálního počítače, přečtěte si [jak tooCreate virtuální počítač s Linuxem][How tooCreate a Linux Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="9d566-115">If you need toocreate a VM, read [How tooCreate a Linux Virtual Machine][How tooCreate a Linux Virtual Machine].</span></span>

## <a name="capture-hello-virtual-machine"></a><span data-ttu-id="9d566-116">Zachytit virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="9d566-116">Capture hello virtual machine</span></span>
1. <span data-ttu-id="9d566-117">[Připojit virtuální počítač toohello](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pomocí SSH klienta podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="9d566-117">[Connect toohello VM](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) using an SSH client of your choice.</span></span>
2. <span data-ttu-id="9d566-118">V okně hello SSH zadejte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="9d566-118">In hello SSH window, type hello following command.</span></span> <span data-ttu-id="9d566-119">Hello výstup z `waagent` může mírně lišit v závislosti na verzi hello tohoto obslužného nástroje:</span><span class="sxs-lookup"><span data-stu-id="9d566-119">hello output from `waagent` may vary slightly depending on hello version of this utility:</span></span>

    ```bash
    sudo waagent -deprovision+user
    ```

    <span data-ttu-id="9d566-120">Hello předchozí příkaz pokusí tooclean hello systému a nastavit jej jako vhodný pro reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="9d566-120">hello preceding command attempts tooclean hello system and make it suitable for reprovisioning.</span></span> <span data-ttu-id="9d566-121">Tato operace provádí hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="9d566-121">This operation performs hello following tasks:</span></span>

   * <span data-ttu-id="9d566-122">Odebere hostitele klíče SSH (Pokud Provisioning.RegenerateSshHostKeyPair 'y' v konfiguračním souboru hello)</span><span class="sxs-lookup"><span data-stu-id="9d566-122">Removes SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in hello configuration file)</span></span>
   * <span data-ttu-id="9d566-123">Vymaže konfiguraci názvový server v /etc/resolv.conf</span><span class="sxs-lookup"><span data-stu-id="9d566-123">Clears nameserver configuration in /etc/resolv.conf</span></span>
   * <span data-ttu-id="9d566-124">Odebere hello `root` heslo uživatele z/etc/stínové (Pokud Provisioning.DeleteRootPassword 'y' v konfiguračním souboru hello)</span><span class="sxs-lookup"><span data-stu-id="9d566-124">Removes hello `root` user password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in hello configuration file)</span></span>
   * <span data-ttu-id="9d566-125">Odstraní zapůjčení DHCP klientů uložená v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9d566-125">Removes cached DHCP client leases</span></span>
   * <span data-ttu-id="9d566-126">Resetování hostitele toolocalhost.localdomain název</span><span class="sxs-lookup"><span data-stu-id="9d566-126">Resets host name toolocalhost.localdomain</span></span>
   * <span data-ttu-id="9d566-127">Odstraní poslední účet zřízení uživatele hello (získaný z /var/lib/waagent) **a související data**.</span><span class="sxs-lookup"><span data-stu-id="9d566-127">Deletes hello last provisioned user account (obtained from /var/lib/waagent) **and associated data**.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9d566-128">Zrušení zřízení odstraní soubory a data příliš "generalize" hello image.</span><span class="sxs-lookup"><span data-stu-id="9d566-128">Deprovisioning deletes files and data too"generalize" hello image.</span></span> <span data-ttu-id="9d566-129">Spusťte pouze na virtuálním počítači tento příkaz, že máte v úmyslu toocapture jako novou šablonu bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="9d566-129">Only run this command on a VM that you intend toocapture as a new image template.</span></span> <span data-ttu-id="9d566-130">Nezaručuje se této bitové kopie hello vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci toothird strany.</span><span class="sxs-lookup"><span data-stu-id="9d566-130">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution toothird parties.</span></span>

3. <span data-ttu-id="9d566-131">Typ **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="9d566-131">Type **y** toocontinue.</span></span> <span data-ttu-id="9d566-132">Můžete přidat hello `-force` parametr tooavoid tento krok potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9d566-132">You can add hello `-force` parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="9d566-133">Typ **ukončení** tooclose hello SSH klienta.</span><span class="sxs-lookup"><span data-stu-id="9d566-133">Type **Exit** tooclose hello SSH client.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9d566-134">Hello zbývající kroky předpokládají už máte [nainstalované rozhraní příkazového řádku Azure hello](../../../cli-install-nodejs.md) na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="9d566-134">hello remaining steps assume you have already [installed hello Azure CLI](../../../cli-install-nodejs.md) on your client computer.</span></span> <span data-ttu-id="9d566-135">Všechny hello následující kroky lze provést v hello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9d566-135">All hello following steps can also be done in hello [Azure portal](http://portal.azure.com).</span></span>

5. <span data-ttu-id="9d566-136">Na klientském počítači otevřete rozhraní příkazového řádku Azure a přihlášení tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="9d566-136">From your client computer, open Azure CLI and login tooyour Azure subscription.</span></span> <span data-ttu-id="9d566-137">Podrobnosti najdete v tématu [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure hello](../../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="9d566-137">For details, read [Connect tooan Azure subscription from hello Azure CLI](../../../xplat-cli-connect.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="9d566-138">V hello portálu Azure Přihlaste se toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="9d566-138">In hello Azure portal, log in toohello portal.</span></span>

6. <span data-ttu-id="9d566-139">Ujistěte se, že jste v režimu správy služby:</span><span class="sxs-lookup"><span data-stu-id="9d566-139">Make sure you are in Service Management mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

7. <span data-ttu-id="9d566-140">Vypněte virtuální počítač, který je již zrušit hello.</span><span class="sxs-lookup"><span data-stu-id="9d566-140">Shut down hello VM that is already deprovisioned.</span></span> <span data-ttu-id="9d566-141">Hello následující příklad vypne hello virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="9d566-141">hello following example shuts down hello VM named `myVM`:</span></span>

    ```azurecli
    azure vm shutdown myVM
    ```
   <span data-ttu-id="9d566-142">V případě potřeby můžete zobrazit seznam všech hello virtuální počítače vytvořené v rámci vašeho předplatného pomocí`azure vm list`</span><span class="sxs-lookup"><span data-stu-id="9d566-142">If needed, you can view a list all hello VMs created in your subscription by using `azure vm list`</span></span>

   > [!NOTE]
   > <span data-ttu-id="9d566-143">Pokud používáte hello portálu Azure, vyberte hello virtuálních počítačů a klikněte na tlačítko **Zastavit** vypnout hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9d566-143">If you're using hello Azure portal, select hello VM and click **Stop** to shut down hello VM.</span></span>

8. <span data-ttu-id="9d566-144">Pokud je zastavená hello virtuálních počítačů, zachycení bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="9d566-144">When hello VM is stopped, capture hello image.</span></span> <span data-ttu-id="9d566-145">Následující příklad zachycení Hello hello virtuálního počítače s názvem `myVM` a vytváří zobecněný bitovou kopii s názvem `myNewVM`:</span><span class="sxs-lookup"><span data-stu-id="9d566-145">hello following example captures hello VM named `myVM` and creates a generalized image named `myNewVM`:</span></span>

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    <span data-ttu-id="9d566-146">Hello `-t` podpříkaz odstranění hello původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9d566-146">hello `-t` subcommand deletes hello original virtual machine.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9d566-147">V hello portálu Azure, budete moct zachytit image výběrem **Image** nabídce hello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9d566-147">In hello Azure portal, you can capture an image by selecting **Image** from hello hub menu.</span></span> <span data-ttu-id="9d566-148">Budete potřebovat následující informace k bitové kopii hello hello toosupply: název, skupinu prostředků, umístění, typ operačního systému a cestu k úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9d566-148">You need toosupply hello following information for hello image: name, resource group, location, operating system type, and storage blob path.</span></span>

9. <span data-ttu-id="9d566-149">Hello nová bitová kopie je nyní k dispozici v seznamu hello bitových kopií, které se dají použít tooconfigure žádné nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9d566-149">hello new image is now available in hello list of images that can be used tooconfigure any new VM.</span></span> <span data-ttu-id="9d566-150">Můžete ji zobrazit pomocí příkazu hello:</span><span class="sxs-lookup"><span data-stu-id="9d566-150">You can view it with hello command:</span></span>

   ```azurecli
   azure vm image list
   ```

   <span data-ttu-id="9d566-151">Na hello [portál Azure](http://portal.azure.com), hello nová bitová kopie se zobrazí v hello **Image virtuálních počítačů (klasické)** , patří toohello **výpočetní** služby.</span><span class="sxs-lookup"><span data-stu-id="9d566-151">On hello [Azure portal](http://portal.azure.com), hello new image appears in hello **VM images (classic)** that belongs toohello **Compute** services.</span></span> <span data-ttu-id="9d566-152">Dostanete **Image virtuálních počítačů (klasické)** kliknutím _další služby_ dole hello hello Azure služby seznamu a pak vyhledávání v hello **výpočetní** služby.</span><span class="sxs-lookup"><span data-stu-id="9d566-152">You can access **VM images (classic)** by clicking _More services_ at hello bottom of hello Azure service list, and then looking in hello **Compute** services.</span></span>   

   ![Zachycení Image úspěšné](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="9d566-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d566-154">Next steps</span></span>
<span data-ttu-id="9d566-155">Hello bitová kopie je připravená toobe používá toocreate virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9d566-155">hello image is ready toobe used toocreate VMs.</span></span> <span data-ttu-id="9d566-156">Můžete použít příkaz příkazového řádku Azure CLI hello `azure vm create` a název bitové kopie hello napájení jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9d566-156">You can use hello Azure CLI command `azure vm create` and supply hello image name you created.</span></span> <span data-ttu-id="9d566-157">Další informace najdete v tématu [hello pomocí rozhraní příkazového řádku Azure s modelem nasazení Classic](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="9d566-157">For more information, see [Using hello Azure CLI with Classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

<span data-ttu-id="9d566-158">Můžete taky použít hello [portál Azure](http://portal.azure.com) toocreate vlastní virtuální počítač pomocí hello **Image** metoda a výběrem hello bitovou kopii, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9d566-158">Alternatively, use hello [Azure portal](http://portal.azure.com) toocreate a custom VM by using hello **Image** method and selecting hello image you created.</span></span> <span data-ttu-id="9d566-159">Další informace najdete v tématu [jak tooCreate vlastní virtuální počítač][How tooCreate a Custom Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="9d566-159">For more information, see [How tooCreate a Custom VM][How tooCreate a Custom Virtual Machine].</span></span>

<span data-ttu-id="9d566-160">**Viz také:** [Azure Linux Agent uživatelská příručka](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="9d566-160">**See also:** [Azure Linux Agent User Guide](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How tooCreate a Custom Virtual Machine]:create-custom.md
[How tooAttach a Data Disk tooa Virtual Machine]:attach-disk.md
[How tooCreate a Linux Virtual Machine]:create-custom.md
