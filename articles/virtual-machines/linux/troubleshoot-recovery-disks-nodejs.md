---
title: "Použijte řešení potíží s virtuálního počítače pomocí Azure CLI 1.0 Linux | Microsoft Docs"
description: "Naučte se virtuální počítač s Linuxem potíží s připojením k obnovení virtuálního počítače pomocí Azure CLI 1.0 disk operačního systému"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: d817358211f123c96d899c5cff88cc47aeb5c9c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-cli-10"></a><span data-ttu-id="b61bf-103">Odstranění virtuálního počítače s Linuxem pomocí disk operačního systému se připojuje k obnovení virtuálního počítače pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b61bf-103">Troubleshoot a Linux VM by attaching the OS disk to a recovery VM using the Azure CLI 1.0</span></span>
<span data-ttu-id="b61bf-104">Pokud Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, musíte provést na virtuálním pevném disku, sám sebe pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="b61bf-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="b61bf-105">Běžným příkladem by neplatná položka v `/etc/fstab` , který brání virtuálního počítače se úspěšně spustil.</span><span class="sxs-lookup"><span data-stu-id="b61bf-105">A common example would be an invalid entry in `/etc/fstab` that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="b61bf-106">Tento článek popisuje, jak pomocí Azure CLI 1.0 připojit virtuální pevný disk na jiný virtuální počítač s Linuxem opravte případné chyby a pak znovu vytvořte původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b61bf-106">This article details how to use the Azure CLI 1.0 to connect your virtual hard disk to another Linux VM to fix any errors, then re-create your original VM.</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="b61bf-107">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="b61bf-107">CLI versions to complete the task</span></span>
<span data-ttu-id="b61bf-108">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="b61bf-108">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="b61bf-109">[Azure CLI 1.0](#recovery-process-overview) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="b61bf-109">[Azure CLI 1.0](#recovery-process-overview) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b61bf-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="b61bf-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="b61bf-111">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="b61bf-111">Recovery process overview</span></span>
<span data-ttu-id="b61bf-112">Proces řešení potíží je následující:</span><span class="sxs-lookup"><span data-stu-id="b61bf-112">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="b61bf-113">Odstraňte virtuální počítač, na zjištění problémy, zachovat virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="b61bf-113">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="b61bf-114">Připojte a připojit virtuální pevný disk na jiný virtuální počítač s Linuxem pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="b61bf-114">Attach and mount the virtual hard disk to another Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="b61bf-115">Připojení k virtuálnímu počítači pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="b61bf-115">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="b61bf-116">Úpravy souborů nebo spustit žádné nástroje na opravte problémy v původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="b61bf-116">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="b61bf-117">Odpojení virtuálního pevného disku od virtuálního počítače pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="b61bf-117">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="b61bf-118">Vytvoření virtuálního počítače pomocí původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="b61bf-118">Create a VM using the original virtual hard disk.</span></span>

<span data-ttu-id="b61bf-119">Ujistěte se, že máte [nejnovější Azure CLI 1.0](../../cli-install-nodejs.md) nainstalován a přihlášení a pomocí režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="b61bf-119">Make sure that you have [the latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="b61bf-120">V následujících příkladech nahraďte názvy parametrů s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="b61bf-120">In the following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="b61bf-121">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="b61bf-121">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="b61bf-122">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="b61bf-122">Determine boot issues</span></span>
<span data-ttu-id="b61bf-123">Prohlédněte si výstup sériové určit, proč váš virtuální počítač není možné správně spustit.</span><span class="sxs-lookup"><span data-stu-id="b61bf-123">Examine the serial output to determine why your VM is not able to boot correctly.</span></span> <span data-ttu-id="b61bf-124">Běžným příkladem jsou neplatná položka v `/etc/fstab`, nebo základní virtuální pevný disk se odstranil nebo přesunul.</span><span class="sxs-lookup"><span data-stu-id="b61bf-124">A common example is an invalid entry in `/etc/fstab`, or the underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="b61bf-125">Následující příklad načte sériové výstup z virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-125">The following example gets the serial output from the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b61bf-126">Zkontrolujte sériové výstup chcete-li zjistit, proč se nedaří spustit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b61bf-126">Review the serial output to determine why the VM is failing to boot.</span></span> <span data-ttu-id="b61bf-127">Pokud sériové výstup neposkytuje jakoukoli indikaci toho, budete muset zkontrolujte soubory protokolu ve `/var/log` až budete mít virtuální pevný disk připojený k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b61bf-127">If the serial output isn't providing any indication, you may need to review log files in `/var/log` once you have the virtual hard disk connected to a troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="b61bf-128">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="b61bf-128">View existing virtual hard disk details</span></span>
<span data-ttu-id="b61bf-129">Než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk, musíte určit název virtuálního pevného disku (VHD).</span><span class="sxs-lookup"><span data-stu-id="b61bf-129">Before you can attach your virtual hard disk to another VM, you need to identify the name of the virtual hard disk (VHD).</span></span> 

<span data-ttu-id="b61bf-130">Následující příklad načte informace pro virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-130">The following example gets information for the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b61bf-131">Vyhledejte `Vhd URI` ve výstupu z předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="b61bf-131">Look for `Vhd URI` in the output from the preceding command.</span></span> <span data-ttu-id="b61bf-132">Zkrácená následující příklad ukazuje výstup `Vhd URI` na posledním řádku:</span><span class="sxs-lookup"><span data-stu-id="b61bf-132">The following truncated example output shows the `Vhd URI` on the last line:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up the VM "myVM"
+ Looking up the NIC "myNic"
+ Looking up the public ip "myPublicIP"
...
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :myVM
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
```


## <a name="delete-existing-vm"></a><span data-ttu-id="b61bf-133">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="b61bf-133">Delete existing VM</span></span>
<span data-ttu-id="b61bf-134">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="b61bf-134">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="b61bf-135">Virtuální pevný disk je, kde jsou uloženy samotného operačního systému, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b61bf-135">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="b61bf-136">Virtuální počítač je jenom metadata, která definuje velikosti či umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="b61bf-136">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="b61bf-137">Každý virtuální pevný disk má zapůjčení přiřazen při připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="b61bf-137">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="b61bf-138">Přestože datové disky je možné připojovat a odpojovat dokonce i za běhu virtuálního počítače, disk s operačním systémem není možné odpojit, dokud se neodstraní prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b61bf-138">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="b61bf-139">Zapůjčení i nadále i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated přidružení disk operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b61bf-139">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="b61bf-140">Prvním krokem k obnovení virtuálního počítače je odstranit samotné prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b61bf-140">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="b61bf-141">Když odstraníte virtuální počítač, virtuální pevné disky zůstanou ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b61bf-141">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="b61bf-142">Po odstranění virtuálního počítače připojit virtuální pevný disk k jiným virtuálním Počítačem vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="b61bf-142">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="b61bf-143">Následující příklad odstraní virtuální počítač s názvem `myVM` ze skupiny prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-143">The following example deletes the VM named `myVM` from the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="b61bf-144">Počkejte, dokud je virtuální počítač dokončí odstraňování před připojit virtuální pevný disk k jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="b61bf-144">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="b61bf-145">Zapůjčení na virtuální pevný disk, který přidruží k němu virtuální počítač je nutné uvolnit předtím, než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="b61bf-145">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="b61bf-146">Připojit existující virtuální pevný disk k jiným virtuálním Počítačem</span><span class="sxs-lookup"><span data-stu-id="b61bf-146">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="b61bf-147">Pro několika dalších krocích použijete jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="b61bf-147">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="b61bf-148">Existující virtuální pevný disk se připojit k řešení potíží VM na Procházet a upravovat obsah na disk.</span><span class="sxs-lookup"><span data-stu-id="b61bf-148">You attach the existing virtual hard disk to this troubleshooting VM to browse and edit the disk's content.</span></span> <span data-ttu-id="b61bf-149">Tento proces umožňuje opravte všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů protokolu, např.</span><span class="sxs-lookup"><span data-stu-id="b61bf-149">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="b61bf-150">Vyberte nebo vytvořte jiným virtuálním Počítačem používat pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="b61bf-150">Choose or create another VM to use for troubleshooting purposes.</span></span>

<span data-ttu-id="b61bf-151">Když připojíte existující virtuální pevný disk, zadejte adresu URL na disk získaných v předchozím `azure vm show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b61bf-151">When you attach the existing virtual hard disk, specify the URL to the disk obtained in the preceding `azure vm show` command.</span></span> <span data-ttu-id="b61bf-152">Následující příklad připojí k řešení potíží virtuální počítač s názvem existujícího virtuálního pevného disku `myVMRecovery` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-152">The following example attaches an existing virtual hard disk to the troubleshooting VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="b61bf-153">Připojit disk připojená data</span><span class="sxs-lookup"><span data-stu-id="b61bf-153">Mount the attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="b61bf-154">Následující příklady jsou upřesněny kroky na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="b61bf-154">The following examples detail the steps required on an Ubuntu VM.</span></span> <span data-ttu-id="b61bf-155">Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, umístění souborů protokolu a `mount` příkazy se můžou mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="b61bf-155">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, the log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="b61bf-156">Naleznete v dokumentaci k vaší konkrétní distro pro příslušné změny v příkazech.</span><span class="sxs-lookup"><span data-stu-id="b61bf-156">Refer to the documentation for your specific distro for the appropriate changes in commands.</span></span>

1. <span data-ttu-id="b61bf-157">SSH k řešení potíží virtuální počítač pomocí příslušných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b61bf-157">SSH to your troubleshooting VM using the appropriate credentials.</span></span> <span data-ttu-id="b61bf-158">Pokud tento disk je první datový disk připojen k řešení potíží virtuální počítač, disk je pravděpodobně připojen k `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="b61bf-158">If this disk is the first data disk attached to your troubleshooting VM, the disk is likely connected to `/dev/sdc`.</span></span> <span data-ttu-id="b61bf-159">Použití `dmseg` zobrazíte připojené disky:</span><span class="sxs-lookup"><span data-stu-id="b61bf-159">Use `dmseg` to view attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="b61bf-160">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="b61bf-160">The output is similar to the following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="b61bf-161">V předchozím příkladu je disk operačního systému na `/dev/sda` a dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="b61bf-161">In the preceding example, the OS disk is at `/dev/sda` and the temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="b61bf-162">Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="b61bf-162">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="b61bf-163">Vytvořte adresář připojit existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="b61bf-163">Create a directory to mount your existing virtual hard disk.</span></span> <span data-ttu-id="b61bf-164">Následující příklad vytvoří adresář s názvem `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-164">The following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="b61bf-165">Pokud máte více oddílů na existující virtuální pevný disk, připojte požadovaný oddíl.</span><span class="sxs-lookup"><span data-stu-id="b61bf-165">If you have multiple partitions on your existing virtual hard disk, mount the required partition.</span></span> <span data-ttu-id="b61bf-166">Následující příklad připojí na první primární oddíl `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-166">The following example mounts the first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="b61bf-167">Osvědčeným postupem je připojit datové disky na virtuální počítače v Azure pomocí identifikátor (UUID) virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="b61bf-167">Best practice is to mount data disks on VMs in Azure using the universally unique identifier (UUID) of the virtual hard disk.</span></span> <span data-ttu-id="b61bf-168">Tento krátký odstraňování potíží připojení virtuálního pevného disku pomocí identifikátoru UUID není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="b61bf-168">For this short troubleshooting scenario, mounting the virtual hard disk using the UUID is not necessary.</span></span> <span data-ttu-id="b61bf-169">Ale při normálním používání úpravy `/etc/fstab` připojit virtuální pevné disky použití název zařízení, nikoli UUID může způsobit selhání spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b61bf-169">However, under normal use, editing `/etc/fstab` to mount virtual hard disks using device name rather than UUID may cause the VM to fail to boot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="b61bf-170">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="b61bf-170">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="b61bf-171">S existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="b61bf-171">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="b61bf-172">Jakmile vyřešíte problémy, pokračujte následujícími kroky.</span><span class="sxs-lookup"><span data-stu-id="b61bf-172">Once you have addressed the issues, continue with the following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="b61bf-173">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="b61bf-173">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="b61bf-174">Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="b61bf-174">Once your errors are resolved, you unmount and detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="b61bf-175">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání zapůjčení virtuální pevný disk se připojuje k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b61bf-175">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="b61bf-176">Z relace SSH k řešení potíží virtuální počítač odpojte existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="b61bf-176">From the SSH session to your troubleshooting VM, unmount the existing virtual hard disk.</span></span> <span data-ttu-id="b61bf-177">Nejprve změňte mimo nadřazený adresář pro přípojného bodu:</span><span class="sxs-lookup"><span data-stu-id="b61bf-177">Change out of the parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="b61bf-178">Nyní odpojte existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="b61bf-178">Now unmount the existing virtual hard disk.</span></span> <span data-ttu-id="b61bf-179">Následující příklad odpojí zařízení na `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-179">The following example unmounts the device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="b61bf-180">Nyní Odpojte virtuální pevný disk z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b61bf-180">Now detach the virtual hard disk from the VM.</span></span> <span data-ttu-id="b61bf-181">Ukončení relace SSH k řešení potíží virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b61bf-181">Exit the SSH session to your troubleshooting VM.</span></span> <span data-ttu-id="b61bf-182">V rozhraní příkazového řádku Azure seznam připojených datových disků k řešení potíží virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b61bf-182">In the Azure CLI, first list the attached data disks to your troubleshooting VM.</span></span> <span data-ttu-id="b61bf-183">Následující příklad vypíše datových disků připojených k virtuálnímu počítači s názvem `myVMRecovery` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-183">The following example lists the data disks attached to the VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    <span data-ttu-id="b61bf-184">Poznámka: `Lun` hodnotu pro existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="b61bf-184">Note the `Lun` value for your existing virtual hard disk.</span></span> <span data-ttu-id="b61bf-185">Příkaz výstupu v následujícím příkladu zobrazuje existujícího virtuálního disku připojená na logické jednotce 0:</span><span class="sxs-lookup"><span data-stu-id="b61bf-185">The following example command output shows the existing virtual disk attached at LUN 0:</span></span>

    ```azurecli
    info:    Executing command vm disk list
    + Looking up the VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    <span data-ttu-id="b61bf-186">Odpojit datový disk od virtuálního počítače pomocí odpovídajících `Lun` hodnotu:</span><span class="sxs-lookup"><span data-stu-id="b61bf-186">Detach the data disk from your VM using the applicable `Lun` value:</span></span>

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="b61bf-187">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="b61bf-187">Create VM from original hard disk</span></span>
<span data-ttu-id="b61bf-188">Chcete-li vytvořit virtuální počítač z původní virtuální pevný disk, použijte [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="b61bf-188">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="b61bf-189">Skutečné šablona JSON je na následující odkaz:</span><span class="sxs-lookup"><span data-stu-id="b61bf-189">The actual JSON template is at the following link:</span></span>

- <span data-ttu-id="b61bf-190">https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b61bf-190">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="b61bf-191">Šablona nasadí virtuální počítač do existující virtuální síť pomocí adresy URL virtuálního pevného disku z dřívějších příkazu.</span><span class="sxs-lookup"><span data-stu-id="b61bf-191">The template deploys a VM into an existing virtual network, using the VHD URL from the earlier command.</span></span> <span data-ttu-id="b61bf-192">Následující příklad nasadí šablony do skupiny prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-192">The following example deploys the template to the resource group named `myResourceGroup`:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

<span data-ttu-id="b61bf-193">Odpovězte pokynů pro šablonu, jako je například název virtuálního počítače (`myDeployedVM` v následujícím příkladu), typ operačního systému (`Linux`) a velikost virtuálního počítače (`Standard_DS1_v2`).</span><span class="sxs-lookup"><span data-stu-id="b61bf-193">Answer the prompts for the template such as VM name (`myDeployedVM` the following example), OS type (`Linux`), and VM size (`Standard_DS1_v2`).</span></span> <span data-ttu-id="b61bf-194">`osDiskVhdUri` Je stejný jako použil při připojení existujícího virtuálního pevného disku k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b61bf-194">The `osDiskVhdUri` is the same as previously used when attaching the existing virtual hard disk to the troubleshooting VM.</span></span> <span data-ttu-id="b61bf-195">Příklad výstupu příkazu a vyzve vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b61bf-195">An example of the command output and prompts is as follows:</span></span>

```azurecli
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName:  myDeployedVM
osType:  Linux
osDiskVhdUri:  https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
vmSize:  Standard_DS1_v2
existingVirtualNetworkName:  myVnet
existingVirtualNetworkResourceGroup:  myResourceGroup
subnetName:  mySubnet
dnsNameForPublicIP:  mypublicipdeployed
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "mydeployment"
+ Waiting for deployment to complete
+
```


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="b61bf-196">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="b61bf-196">Re-enable boot diagnostics</span></span>

<span data-ttu-id="b61bf-197">Při vytváření virtuálního počítače z existujícího virtuálního pevného disku, nemusí být Diagnostika spouštění automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="b61bf-197">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="b61bf-198">Následující příklad povolí diagnostiky rozšíření ve virtuálním počítači s názvem `myDeployedVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b61bf-198">The following example enables the diagnostic extension on the VM named `myDeployedVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="b61bf-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b61bf-199">Next steps</span></span>
<span data-ttu-id="b61bf-200">Pokud máte problémy s připojením k virtuálnímu počítači, přečtěte si téma [řešení SSH připojení k virtuální počítač Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b61bf-200">If you are having issues connecting to your VM, see [Troubleshoot SSH connections to an Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="b61bf-201">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b61bf-201">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>