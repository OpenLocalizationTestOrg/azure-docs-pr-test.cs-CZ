---
title: "Vytvořte bitovou kopii virtuálního počítače s Linuxem | Microsoft Docs"
description: "Zjistěte, jak pro zachycení image systémem Linux Azure virtuálního počítače (VM) vytvořené pomocí modelu nasazení classic."
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
ms.openlocfilehash: ecde5dd3211bfbb290e6910d7d55136d079c6cf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a><span data-ttu-id="745d7-103">Jak zachytit klasický virtuální počítač s Linuxem jako image</span><span class="sxs-lookup"><span data-stu-id="745d7-103">How to capture a classic Linux virtual machine as an image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="745d7-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="745d7-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="745d7-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="745d7-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="745d7-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="745d7-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="745d7-107">Zjistěte, jak [provést tento postup pomocí modelu Resource Manageru](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="745d7-107">Learn how to [perform these steps using the Resource Manager model](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="745d7-108">Tento článek ukazuje, jak zachytit klasický Azure virtuální počítač (VM) s Linuxem jako bitovou kopii k vytvoření dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="745d7-108">This article shows you how to capture a classic Azure virtual machine (VM) running Linux as an image to create other virtual machines.</span></span> <span data-ttu-id="745d7-109">Tento image obsahuje disk operačního systému a datové disky připojené k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="745d7-109">This image includes the OS disk and data disks attached to the VM.</span></span> <span data-ttu-id="745d7-110">Konfigurace sítí, neobsahuje, je nutné nakonfigurovat, když vytvoříte další virtuální počítač z bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="745d7-110">It doesn't include networking configuration, so you need to configure that when you create the other VM from the image.</span></span>

<span data-ttu-id="745d7-111">Obrázek v Azure uloží **bitové kopie**, společně s všechny Image, které jste odeslali.</span><span class="sxs-lookup"><span data-stu-id="745d7-111">Azure stores the image under **Images**, along with any images you've uploaded.</span></span> <span data-ttu-id="745d7-112">Další informace o bitových kopiích naleznete v tématu [o Image virtuálních počítačů v Azure][About Virtual Machine Images in Azure].</span><span class="sxs-lookup"><span data-stu-id="745d7-112">For more information about images, see [About Virtual Machine Images in Azure][About Virtual Machine Images in Azure].</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="745d7-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="745d7-113">Before you begin</span></span>
<span data-ttu-id="745d7-114">Tento postup předpokládá, že jste vytvořili virtuální počítač Azure pomocí modelu nasazení Classic a nakonfigurovat operačního systému, včetně připojení jakýchkoli datových disků.</span><span class="sxs-lookup"><span data-stu-id="745d7-114">These steps assume that you've already created an Azure VM using the Classic deployment model and configured the operating system, including attaching any data disks.</span></span> <span data-ttu-id="745d7-115">Pokud potřebujete k vytvoření virtuálního počítače, přečtěte si [jak vytvořit virtuální počítač s Linuxem][How to Create a Linux Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="745d7-115">If you need to create a VM, read [How to Create a Linux Virtual Machine][How to Create a Linux Virtual Machine].</span></span>

## <a name="capture-the-virtual-machine"></a><span data-ttu-id="745d7-116">Zachytit virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="745d7-116">Capture the virtual machine</span></span>
1. <span data-ttu-id="745d7-117">[Připojte se k Virtuálnímu](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pomocí SSH klienta podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="745d7-117">[Connect to the VM](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) using an SSH client of your choice.</span></span>
2. <span data-ttu-id="745d7-118">V okně SSH zadejte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="745d7-118">In the SSH window, type the following command.</span></span> <span data-ttu-id="745d7-119">Výstup z `waagent` může mírně lišit v závislosti na verzi tohoto obslužného nástroje:</span><span class="sxs-lookup"><span data-stu-id="745d7-119">The output from `waagent` may vary slightly depending on the version of this utility:</span></span>

    ```bash
    sudo waagent -deprovision+user
    ```

    <span data-ttu-id="745d7-120">Předchozí příkaz pokusí vyčistit systému a nastavit jej jako vhodný pro reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="745d7-120">The preceding command attempts to clean the system and make it suitable for reprovisioning.</span></span> <span data-ttu-id="745d7-121">Tato operace provede následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="745d7-121">This operation performs the following tasks:</span></span>

   * <span data-ttu-id="745d7-122">Odebere hostitele klíče SSH (Pokud Provisioning.RegenerateSshHostKeyPair 'y' v konfiguračním souboru)</span><span class="sxs-lookup"><span data-stu-id="745d7-122">Removes SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in the configuration file)</span></span>
   * <span data-ttu-id="745d7-123">Vymaže konfiguraci názvový server v /etc/resolv.conf</span><span class="sxs-lookup"><span data-stu-id="745d7-123">Clears nameserver configuration in /etc/resolv.conf</span></span>
   * <span data-ttu-id="745d7-124">Odebere `root` heslo uživatele z/etc/stínové (Pokud Provisioning.DeleteRootPassword 'y' v konfiguračním souboru)</span><span class="sxs-lookup"><span data-stu-id="745d7-124">Removes the `root` user password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in the configuration file)</span></span>
   * <span data-ttu-id="745d7-125">Odstraní zapůjčení DHCP klientů uložená v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="745d7-125">Removes cached DHCP client leases</span></span>
   * <span data-ttu-id="745d7-126">Resetuje název hostitele na localhost.localdomain</span><span class="sxs-lookup"><span data-stu-id="745d7-126">Resets host name to localhost.localdomain</span></span>
   * <span data-ttu-id="745d7-127">Odstranění posledního zřízený uživatelský účet (získaný z /var/lib/waagent) **a související data**.</span><span class="sxs-lookup"><span data-stu-id="745d7-127">Deletes the last provisioned user account (obtained from /var/lib/waagent) **and associated data**.</span></span>

     > [!NOTE]
     > <span data-ttu-id="745d7-128">Zrušení zřízení odstraní soubory a data "zobecní" bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="745d7-128">Deprovisioning deletes files and data to "generalize" the image.</span></span> <span data-ttu-id="745d7-129">Tento příkaz lze spusťte pouze na virtuální počítač, který chcete zaznamenat jako novou šablonu bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="745d7-129">Only run this command on a VM that you intend to capture as a new image template.</span></span> <span data-ttu-id="745d7-130">Není zaručeno, že bitovou kopii vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci třetím stranám.</span><span class="sxs-lookup"><span data-stu-id="745d7-130">It does not guarantee that the image is cleared of all sensitive information or is suitable for redistribution to third parties.</span></span>

3. <span data-ttu-id="745d7-131">Typ **y** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="745d7-131">Type **y** to continue.</span></span> <span data-ttu-id="745d7-132">Můžete přidat `-force` parametr předejdete tento krok potvrzení.</span><span class="sxs-lookup"><span data-stu-id="745d7-132">You can add the `-force` parameter to avoid this confirmation step.</span></span>
4. <span data-ttu-id="745d7-133">Typ **ukončení** zavřít klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="745d7-133">Type **Exit** to close the SSH client.</span></span>

   > [!NOTE]
   > <span data-ttu-id="745d7-134">Zbývající kroky předpokládají, že už máte [nainstalované rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="745d7-134">The remaining steps assume you have already [installed the Azure CLI](../../../cli-install-nodejs.md) on your client computer.</span></span> <span data-ttu-id="745d7-135">Všechny následující kroky lze provést [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="745d7-135">All the following steps can also be done in the [Azure portal](http://portal.azure.com).</span></span>

5. <span data-ttu-id="745d7-136">Na klientském počítači otevřete rozhraní příkazového řádku Azure a přihlaste se k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="745d7-136">From your client computer, open Azure CLI and login to your Azure subscription.</span></span> <span data-ttu-id="745d7-137">Podrobnosti najdete v tématu [připojení k předplatnému Azure z rozhraní příkazového řádku Azure](../../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="745d7-137">For details, read [Connect to an Azure subscription from the Azure CLI](../../../xplat-cli-connect.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="745d7-138">Na portálu Azure Přihlaste se k portálu.</span><span class="sxs-lookup"><span data-stu-id="745d7-138">In the Azure portal, log in to the portal.</span></span>

6. <span data-ttu-id="745d7-139">Ujistěte se, že jste v režimu správy služby:</span><span class="sxs-lookup"><span data-stu-id="745d7-139">Make sure you are in Service Management mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

7. <span data-ttu-id="745d7-140">Vypněte virtuální počítač, který je již zrušit.</span><span class="sxs-lookup"><span data-stu-id="745d7-140">Shut down the VM that is already deprovisioned.</span></span> <span data-ttu-id="745d7-141">Následující příklad vypne virtuální počítač s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="745d7-141">The following example shuts down the VM named `myVM`:</span></span>

    ```azurecli
    azure vm shutdown myVM
    ```
   <span data-ttu-id="745d7-142">V případě potřeby můžete zobrazit seznam všech virtuálních počítačů ve vašem předplatném vytvořily s použitím`azure vm list`</span><span class="sxs-lookup"><span data-stu-id="745d7-142">If needed, you can view a list all the VMs created in your subscription by using `azure vm list`</span></span>

   > [!NOTE]
   > <span data-ttu-id="745d7-143">Pokud používáte portál Azure, vyberte virtuální počítač a klikněte na tlačítko **Zastavit** vypnutí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="745d7-143">If you're using the Azure portal, select the VM and click **Stop** to shut down the VM.</span></span>

8. <span data-ttu-id="745d7-144">Při zastavení virtuálního počítače je zachycení bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="745d7-144">When the VM is stopped, capture the image.</span></span> <span data-ttu-id="745d7-145">Následující příklad zaznamená virtuálního počítače s názvem `myVM` a vytváří zobecněný bitovou kopii s názvem `myNewVM`:</span><span class="sxs-lookup"><span data-stu-id="745d7-145">The following example captures the VM named `myVM` and creates a generalized image named `myNewVM`:</span></span>

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    <span data-ttu-id="745d7-146">`-t` Podpříkaz odstraní původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="745d7-146">The `-t` subcommand deletes the original virtual machine.</span></span>

    > [!NOTE]
    > <span data-ttu-id="745d7-147">Na portálu Azure můžete zaznamenat bitovou kopii výběrem **Image** v nabídce centra.</span><span class="sxs-lookup"><span data-stu-id="745d7-147">In the Azure portal, you can capture an image by selecting **Image** from the hub menu.</span></span> <span data-ttu-id="745d7-148">Budete muset zadat následující informace pro bitovou kopii: název, skupinu prostředků, umístění, typ operačního systému a cestu k úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="745d7-148">You need to supply the following information for the image: name, resource group, location, operating system type, and storage blob path.</span></span>

9. <span data-ttu-id="745d7-149">Nová bitová kopie je nyní k dispozici v seznamu bitové kopie, které můžete použít ke konfiguraci žádné nové virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="745d7-149">The new image is now available in the list of images that can be used to configure any new VM.</span></span> <span data-ttu-id="745d7-150">Můžete ji zobrazit pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="745d7-150">You can view it with the command:</span></span>

   ```azurecli
   azure vm image list
   ```

   <span data-ttu-id="745d7-151">Na [portál Azure](http://portal.azure.com), nová bitová kopie se zobrazí v **Image virtuálních počítačů (klasické)** který patří do **výpočetní** služby.</span><span class="sxs-lookup"><span data-stu-id="745d7-151">On the [Azure portal](http://portal.azure.com), the new image appears in the **VM images (classic)** that belongs to the **Compute** services.</span></span> <span data-ttu-id="745d7-152">Dostanete **Image virtuálních počítačů (klasické)** kliknutím _další služby_ v dolní části Azure služby seznamu a pak vyhledávání **výpočetní** služby.</span><span class="sxs-lookup"><span data-stu-id="745d7-152">You can access **VM images (classic)** by clicking _More services_ at the bottom of the Azure service list, and then looking in the **Compute** services.</span></span>   

   ![Zachycení Image úspěšné](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a><span data-ttu-id="745d7-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="745d7-154">Next steps</span></span>
<span data-ttu-id="745d7-155">Obrázek je připraven použít k vytvoření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="745d7-155">The image is ready to be used to create VMs.</span></span> <span data-ttu-id="745d7-156">Můžete použít příkaz příkazového řádku Azure CLI `azure vm create` a zadejte název bitové kopie, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="745d7-156">You can use the Azure CLI command `azure vm create` and supply the image name you created.</span></span> <span data-ttu-id="745d7-157">Další informace najdete v tématu [pomocí rozhraní příkazového řádku Azure s modelem nasazení Classic](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="745d7-157">For more information, see [Using the Azure CLI with Classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

<span data-ttu-id="745d7-158">Můžete taky použít [portál Azure](http://portal.azure.com) k vytvoření vlastních virtuálních počítačů pomocí **bitové kopie** metoda a vybrat image jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="745d7-158">Alternatively, use the [Azure portal](http://portal.azure.com) to create a custom VM by using the **Image** method and selecting the image you created.</span></span> <span data-ttu-id="745d7-159">Další informace najdete v tématu [postup vytvoření virtuálního počítače s vlastní][How to Create a Custom Virtual Machine].</span><span class="sxs-lookup"><span data-stu-id="745d7-159">For more information, see [How to Create a Custom VM][How to Create a Custom Virtual Machine].</span></span>

<span data-ttu-id="745d7-160">**Viz také:** [Azure Linux Agent uživatelská příručka](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="745d7-160">**See also:** [Azure Linux Agent User Guide](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]:create-custom.md
[How to Attach a Data Disk to a Virtual Machine]:attach-disk.md
[How to Create a Linux Virtual Machine]:create-custom.md
