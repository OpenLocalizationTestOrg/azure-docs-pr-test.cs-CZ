---
title: "Použijte řešení potíží s virtuálního počítače pomocí Azure CLI 2.0 Linux | Microsoft Docs"
description: "Naučte se virtuální počítač s Linuxem potíží s připojením k obnovení virtuálního počítače pomocí Azure CLI 2.0 disk operačního systému"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: iainfou
ms.openlocfilehash: 7a28accce1bd328b2b486b588c44d91b03e42122
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli-20"></a><span data-ttu-id="759fc-103">Odstranění virtuálního počítače s Linuxem pomocí disk operačního systému se připojuje k obnovení virtuálního počítače pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="759fc-103">Troubleshoot a Linux VM by attaching the OS disk to a recovery VM with the Azure CLI 2.0</span></span>
<span data-ttu-id="759fc-104">Pokud Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, musíte provést na virtuálním pevném disku, sám sebe pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="759fc-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="759fc-105">Běžným příkladem by neplatná položka v `/etc/fstab` , který brání virtuálního počítače se úspěšně spustil.</span><span class="sxs-lookup"><span data-stu-id="759fc-105">A common example would be an invalid entry in `/etc/fstab` that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="759fc-106">Tento článek popisuje, jak pomocí Azure CLI 2.0 připojit virtuální pevný disk na jiný virtuální počítač s Linuxem opravte případné chyby a pak znovu vytvořte původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="759fc-106">This article details how to use the Azure CLI 2.0 to connect your virtual hard disk to another Linux VM to fix any errors, then re-create your original VM.</span></span> <span data-ttu-id="759fc-107">K provedení těchto kroků můžete také využít [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="759fc-107">You can also perform these steps with the [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="759fc-108">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="759fc-108">Recovery process overview</span></span>
<span data-ttu-id="759fc-109">Proces řešení potíží je následující:</span><span class="sxs-lookup"><span data-stu-id="759fc-109">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="759fc-110">Odstraňte virtuální počítač, na zjištění problémy, zachovat virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="759fc-110">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="759fc-111">Připojte a připojit virtuální pevný disk na jiný virtuální počítač s Linuxem pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="759fc-111">Attach and mount the virtual hard disk to another Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="759fc-112">Připojení k virtuálnímu počítači pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="759fc-112">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="759fc-113">Úpravy souborů nebo spustit žádné nástroje na opravte problémy v původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="759fc-113">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="759fc-114">Odpojení virtuálního pevného disku od virtuálního počítače pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="759fc-114">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="759fc-115">Vytvoření virtuálního počítače pomocí původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="759fc-115">Create a VM using the original virtual hard disk.</span></span>

<span data-ttu-id="759fc-116">Pokud chcete provést následující kroky pro řešení, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="759fc-116">To perform these troubleshooting steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="759fc-117">V následujících příkladech nahraďte názvy parametrů s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="759fc-117">In the following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="759fc-118">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="759fc-118">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="759fc-119">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="759fc-119">Determine boot issues</span></span>
<span data-ttu-id="759fc-120">Prohlédněte si výstup sériové určit, proč váš virtuální počítač není možné správně spustit.</span><span class="sxs-lookup"><span data-stu-id="759fc-120">Examine the serial output to determine why your VM is not able to boot correctly.</span></span> <span data-ttu-id="759fc-121">Běžným příkladem jsou neplatná položka v `/etc/fstab`, nebo základní virtuální pevný disk se odstranil nebo přesunul.</span><span class="sxs-lookup"><span data-stu-id="759fc-121">A common example is an invalid entry in `/etc/fstab`, or the underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="759fc-122">Získání protokolů spouštěcí s [Diagnostika spouštění virtuálních počítačů az – get spouštěcí protokolu](/cli/azure/vm/boot-diagnostics#get-boot-log).</span><span class="sxs-lookup"><span data-stu-id="759fc-122">Get the boot logs with [az vm boot-diagnostics get-boot-log](/cli/azure/vm/boot-diagnostics#get-boot-log).</span></span> <span data-ttu-id="759fc-123">Následující příklad načte sériové výstup z virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="759fc-123">The following example gets the serial output from the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="759fc-124">Zkontrolujte sériové výstup chcete-li zjistit, proč se nedaří spustit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="759fc-124">Review the serial output to determine why the VM is failing to boot.</span></span> <span data-ttu-id="759fc-125">Pokud sériové výstup neposkytuje jakoukoli indikaci toho, budete muset zkontrolujte soubory protokolu ve `/var/log` až budete mít virtuální pevný disk připojený k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="759fc-125">If the serial output isn't providing any indication, you may need to review log files in `/var/log` once you have the virtual hard disk connected to a troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="759fc-126">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="759fc-126">View existing virtual hard disk details</span></span>
<span data-ttu-id="759fc-127">Předtím, než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk (VHD), musíte určit identifikátor URI disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="759fc-127">Before you can attach your virtual hard disk (VHD) to another VM, you need to identify the URI of the OS disk.</span></span> 

<span data-ttu-id="759fc-128">Zobrazit informace o virtuální počítač s [az virtuálních počítačů zobrazit](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="759fc-128">View information about your VM with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="759fc-129">Použití `--query` příznak k extrakci identifikátor URI pro disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="759fc-129">Use the `--query` flag to extract the URI to the OS disk.</span></span> <span data-ttu-id="759fc-130">Následující příklad získá informace o disku pro virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="759fc-130">The following example gets disk information for the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

<span data-ttu-id="759fc-131">Identifikátor URI je podobná **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span><span class="sxs-lookup"><span data-stu-id="759fc-131">The URI is similar to **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span></span>

## <a name="delete-existing-vm"></a><span data-ttu-id="759fc-132">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="759fc-132">Delete existing VM</span></span>
<span data-ttu-id="759fc-133">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="759fc-133">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="759fc-134">Virtuální pevný disk je, kde jsou uloženy samotného operačního systému, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="759fc-134">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="759fc-135">Virtuální počítač je jenom metadata, která definuje velikosti či umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="759fc-135">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="759fc-136">Každý virtuální pevný disk má zapůjčení přiřazen při připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="759fc-136">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="759fc-137">Přestože datové disky je možné připojovat a odpojovat dokonce i za běhu virtuálního počítače, disk s operačním systémem není možné odpojit, dokud se neodstraní prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="759fc-137">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="759fc-138">Zapůjčení i nadále i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated přidružení disk operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="759fc-138">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="759fc-139">Prvním krokem k obnovení virtuálního počítače je odstranit samotné prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="759fc-139">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="759fc-140">Když odstraníte virtuální počítač, virtuální pevné disky zůstanou ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="759fc-140">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="759fc-141">Po odstranění virtuálního počítače připojit virtuální pevný disk k jiným virtuálním Počítačem vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="759fc-141">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="759fc-142">Odstraňte virtuální počítač s [odstranit virtuální počítač az](/cli/azure/vm#delete).</span><span class="sxs-lookup"><span data-stu-id="759fc-142">Delete the VM with [az vm delete](/cli/azure/vm#delete).</span></span> <span data-ttu-id="759fc-143">Následující příklad odstraní virtuální počítač s názvem `myVM` ze skupiny prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="759fc-143">The following example deletes the VM named `myVM` from the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="759fc-144">Počkejte, dokud je virtuální počítač dokončí odstraňování před připojit virtuální pevný disk k jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="759fc-144">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="759fc-145">Zapůjčení na virtuální pevný disk, který přidruží k němu virtuální počítač je nutné uvolnit předtím, než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="759fc-145">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="759fc-146">Připojit existující virtuální pevný disk k jiným virtuálním Počítačem</span><span class="sxs-lookup"><span data-stu-id="759fc-146">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="759fc-147">Pro několika dalších krocích použijete jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="759fc-147">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="759fc-148">Existující virtuální pevný disk se připojit k řešení potíží VM na Procházet a upravovat obsah na disk.</span><span class="sxs-lookup"><span data-stu-id="759fc-148">You attach the existing virtual hard disk to this troubleshooting VM to browse and edit the disk's content.</span></span> <span data-ttu-id="759fc-149">Tento proces umožňuje opravte všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů protokolu, např.</span><span class="sxs-lookup"><span data-stu-id="759fc-149">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="759fc-150">Vyberte nebo vytvořte jiným virtuálním Počítačem používat pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="759fc-150">Choose or create another VM to use for troubleshooting purposes.</span></span>

<span data-ttu-id="759fc-151">Připojit existující virtuální pevný disk s [nespravované virtuální počítač az-disk připojit](/cli/azure/vm/unmanaged-disk#attach).</span><span class="sxs-lookup"><span data-stu-id="759fc-151">Attach the existing virtual hard disk with [az vm unmanaged-disk attach](/cli/azure/vm/unmanaged-disk#attach).</span></span> <span data-ttu-id="759fc-152">Když připojíte existující virtuální pevný disk, zadejte identifikátor URI na disk získaných v předchozím `az vm show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="759fc-152">When you attach the existing virtual hard disk, specify the URI to the disk obtained in the preceding `az vm show` command.</span></span> <span data-ttu-id="759fc-153">Následující příklad připojí k řešení potíží virtuální počítač s názvem existujícího virtuálního pevného disku `myVMRecovery` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="759fc-153">The following example attaches an existing virtual hard disk to the troubleshooting VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="759fc-154">Připojit disk připojená data</span><span class="sxs-lookup"><span data-stu-id="759fc-154">Mount the attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="759fc-155">Následující příklady jsou upřesněny kroky na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="759fc-155">The following examples detail the steps required on an Ubuntu VM.</span></span> <span data-ttu-id="759fc-156">Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, umístění souborů protokolu a `mount` příkazy se můžou mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="759fc-156">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, the log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="759fc-157">Naleznete v dokumentaci k vaší konkrétní distro pro příslušné změny v příkazech.</span><span class="sxs-lookup"><span data-stu-id="759fc-157">Refer to the documentation for your specific distro for the appropriate changes in commands.</span></span>

1. <span data-ttu-id="759fc-158">SSH k řešení potíží virtuální počítač pomocí příslušných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="759fc-158">SSH to your troubleshooting VM using the appropriate credentials.</span></span> <span data-ttu-id="759fc-159">Pokud tento disk je první datový disk připojen k řešení potíží virtuální počítač, disk je pravděpodobně připojen k `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="759fc-159">If this disk is the first data disk attached to your troubleshooting VM, the disk is likely connected to `/dev/sdc`.</span></span> <span data-ttu-id="759fc-160">Použití `dmseg` zobrazíte připojené disky:</span><span class="sxs-lookup"><span data-stu-id="759fc-160">Use `dmseg` to view attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="759fc-161">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="759fc-161">The output is similar to the following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="759fc-162">V předchozím příkladu je disk operačního systému na `/dev/sda` a dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="759fc-162">In the preceding example, the OS disk is at `/dev/sda` and the temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="759fc-163">Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="759fc-163">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="759fc-164">Vytvořte adresář připojit existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="759fc-164">Create a directory to mount your existing virtual hard disk.</span></span> <span data-ttu-id="759fc-165">Následující příklad vytvoří adresář s názvem `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="759fc-165">The following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="759fc-166">Pokud máte více oddílů na existující virtuální pevný disk, připojte požadovaný oddíl.</span><span class="sxs-lookup"><span data-stu-id="759fc-166">If you have multiple partitions on your existing virtual hard disk, mount the required partition.</span></span> <span data-ttu-id="759fc-167">Následující příklad připojí na první primární oddíl `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="759fc-167">The following example mounts the first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="759fc-168">Osvědčeným postupem je připojit datové disky na virtuální počítače v Azure pomocí identifikátor (UUID) virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="759fc-168">Best practice is to mount data disks on VMs in Azure using the universally unique identifier (UUID) of the virtual hard disk.</span></span> <span data-ttu-id="759fc-169">Tento krátký odstraňování potíží připojení virtuálního pevného disku pomocí identifikátoru UUID není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="759fc-169">For this short troubleshooting scenario, mounting the virtual hard disk using the UUID is not necessary.</span></span> <span data-ttu-id="759fc-170">Ale při normálním používání úpravy `/etc/fstab` připojit virtuální pevné disky použití název zařízení, nikoli UUID může způsobit selhání spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="759fc-170">However, under normal use, editing `/etc/fstab` to mount virtual hard disks using device name rather than UUID may cause the VM to fail to boot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="759fc-171">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="759fc-171">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="759fc-172">S existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="759fc-172">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="759fc-173">Jakmile vyřešíte problémy, pokračujte následujícími kroky.</span><span class="sxs-lookup"><span data-stu-id="759fc-173">Once you have addressed the issues, continue with the following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="759fc-174">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="759fc-174">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="759fc-175">Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="759fc-175">Once your errors are resolved, you unmount and detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="759fc-176">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání zapůjčení virtuální pevný disk se připojuje k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="759fc-176">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="759fc-177">Z relace SSH k řešení potíží virtuální počítač odpojte existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="759fc-177">From the SSH session to your troubleshooting VM, unmount the existing virtual hard disk.</span></span> <span data-ttu-id="759fc-178">Nejprve změňte mimo nadřazený adresář pro přípojného bodu:</span><span class="sxs-lookup"><span data-stu-id="759fc-178">Change out of the parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="759fc-179">Nyní odpojte existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="759fc-179">Now unmount the existing virtual hard disk.</span></span> <span data-ttu-id="759fc-180">Následující příklad odpojí zařízení na `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="759fc-180">The following example unmounts the device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="759fc-181">Nyní Odpojte virtuální pevný disk z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="759fc-181">Now detach the virtual hard disk from the VM.</span></span> <span data-ttu-id="759fc-182">Ukončení relace SSH k řešení potíží virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="759fc-182">Exit the SSH session to your troubleshooting VM.</span></span> <span data-ttu-id="759fc-183">Seznam připojených datových disků k řešení potíží virtuální počítač s [seznamu nespravované disku virtuálního počítače az](/cli/azure/vm/unmanaged-disk#list).</span><span class="sxs-lookup"><span data-stu-id="759fc-183">List the attached data disks to your troubleshooting VM with [az vm unmanaged-disk list](/cli/azure/vm/unmanaged-disk#list).</span></span> <span data-ttu-id="759fc-184">Následující příklad vypíše datových disků připojených k virtuálnímu počítači s názvem `myVMRecovery` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="759fc-184">The following example lists the data disks attached to the VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    <span data-ttu-id="759fc-185">Poznamenejte si název pro existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="759fc-185">Note the name for your existing virtual hard disk.</span></span> <span data-ttu-id="759fc-186">Například název disku s identifikátor URI **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** je **myVHD**.</span><span class="sxs-lookup"><span data-stu-id="759fc-186">For example, the name of a disk with the URI of **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** is **myVHD**.</span></span> 

    <span data-ttu-id="759fc-187">Odpojit datový disk od virtuálního počítače [nespravované virtuální počítač az-disk odpojit](/cli/azure/vm/unmanaged-disk#detach).</span><span class="sxs-lookup"><span data-stu-id="759fc-187">Detach the data disk from your VM [az vm unmanaged-disk detach](/cli/azure/vm/unmanaged-disk#detach).</span></span> <span data-ttu-id="759fc-188">Následující příklad umožňuje odpojit disk s názvem `myVHD` z virtuálního počítače s názvem `myVMRecovery` v `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="759fc-188">The following example detaches the disk named `myVHD` from the VM named `myVMRecovery` in the `myResourceGroup` resource group:</span></span>

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="759fc-189">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="759fc-189">Create VM from original hard disk</span></span>
<span data-ttu-id="759fc-190">Chcete-li vytvořit virtuální počítač z původní virtuální pevný disk, použijte [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="759fc-190">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="759fc-191">Skutečné šablona JSON je na následující odkaz:</span><span class="sxs-lookup"><span data-stu-id="759fc-191">The actual JSON template is at the following link:</span></span>

- <span data-ttu-id="759fc-192">https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="759fc-192">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="759fc-193">Šablona nasadí virtuálního počítače pomocí identifikátoru URI virtuálního pevného disku z dřívějších příkazu.</span><span class="sxs-lookup"><span data-stu-id="759fc-193">The template deploys a VM using the VHD URI from the earlier command.</span></span> <span data-ttu-id="759fc-194">Nasazení šablony s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="759fc-194">Deploy the template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="759fc-195">Zadejte identifikátor URI na vaše původní virtuální pevný disk a pak zadejte typ operačního systému, velikost virtuálního počítače a název virtuálního počítače takto:</span><span class="sxs-lookup"><span data-stu-id="759fc-195">Provide the URI to your original VHD and then specify the OS type, VM size, and VM name as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="759fc-196">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="759fc-196">Re-enable boot diagnostics</span></span>
<span data-ttu-id="759fc-197">Při vytváření virtuálního počítače z existujícího virtuálního pevného disku, nemusí být Diagnostika spouštění automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="759fc-197">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="759fc-198">Povolit Diagnostika spouštění s [povolit az virtuálního počítače – Diagnostika spouštění](/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="759fc-198">Enable boot diagnostics with [az vm boot-diagnostics enable](/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="759fc-199">Následující příklad povolí diagnostiky rozšíření ve virtuálním počítači s názvem `myDeployedVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="759fc-199">The following example enables the diagnostic extension on the VM named `myDeployedVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="759fc-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="759fc-200">Next steps</span></span>
<span data-ttu-id="759fc-201">Pokud máte problémy s připojením k virtuálnímu počítači, přečtěte si téma [řešení SSH připojení k virtuální počítač Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="759fc-201">If you are having issues connecting to your VM, see [Troubleshoot SSH connections to an Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="759fc-202">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="759fc-202">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>