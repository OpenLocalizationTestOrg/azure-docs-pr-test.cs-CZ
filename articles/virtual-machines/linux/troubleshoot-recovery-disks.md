---
title: "aaaUse a řešení potíží s virtuálních počítačů s hello Azure CLI 2.0 Linux | Microsoft Docs"
description: "Zjistěte, jak hello tootroubleshoot, které vystavuje virtuálního počítače s Linuxem pomocí připojování hello operačního systému disku tooa obnovení virtuálního počítače Azure CLI 2.0"
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
ms.openlocfilehash: 776d61b61280f46e3699157addcdb1e7dfb6818e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-with-hello-azure-cli-20"></a><span data-ttu-id="bce32-103">Řešení potíží s virtuálního počítače s Linuxem připojením hello operačního systému disku tooa obnovení virtuálních počítačů s hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="bce32-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM with hello Azure CLI 2.0</span></span>
<span data-ttu-id="bce32-104">Pokud systém Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe.</span><span class="sxs-lookup"><span data-stu-id="bce32-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="bce32-105">Běžným příkladem by neplatná položka v `/etc/fstab` , který brání hello virtuálních počítačů je možné tooboot úspěšně.</span><span class="sxs-lookup"><span data-stu-id="bce32-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="bce32-106">Tento článek podrobnosti jak toouse hello Azure CLI 2.0 tooconnect vaše virtuálního pevného disku virtuálního počítače s Linuxem toofix tooanother všechny chyby a potom ho znovu vytvořit původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="bce32-106">This article details how toouse hello Azure CLI 2.0 tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span> <span data-ttu-id="bce32-107">Můžete také provést tyto kroky hello [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce32-107">You can also perform these steps with hello [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="bce32-108">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="bce32-108">Recovery process overview</span></span>
<span data-ttu-id="bce32-109">řešení potíží s procesem Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="bce32-109">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="bce32-110">Odstraňte hello virtuálních počítačů zjištění problémy, udržování hello virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="bce32-110">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="bce32-111">Připojení a připojte tooanother hello virtuální pevný disk virtuálního počítače s Linuxem pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="bce32-111">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="bce32-112">Připojte toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-112">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="bce32-113">Úpravy souborů nebo spuštěním žádné nástroje toofix problémy v hello původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="bce32-113">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="bce32-114">Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-114">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="bce32-115">Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="bce32-115">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="bce32-116">tooperform těchto potíží, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="bce32-116">tooperform these troubleshooting steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="bce32-117">V hello následujících příkladech nahraďte vlastními hodnotami názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="bce32-117">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="bce32-118">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="bce32-118">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="bce32-119">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="bce32-119">Determine boot issues</span></span>
<span data-ttu-id="bce32-120">Zkontrolujte toodetermine sériové výstup hello proč virtuálního počítače není možné tooboot správně.</span><span class="sxs-lookup"><span data-stu-id="bce32-120">Examine hello serial output toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="bce32-121">Běžným příkladem jsou neplatná položka v `/etc/fstab`, nebo hello základní virtuální pevný disk se odstranil nebo přesunul.</span><span class="sxs-lookup"><span data-stu-id="bce32-121">A common example is an invalid entry in `/etc/fstab`, or hello underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="bce32-122">Získání protokolů spouštěcí hello s [Diagnostika spouštění virtuálních počítačů az – get spouštěcí protokolu](/cli/azure/vm/boot-diagnostics#get-boot-log).</span><span class="sxs-lookup"><span data-stu-id="bce32-122">Get hello boot logs with [az vm boot-diagnostics get-boot-log](/cli/azure/vm/boot-diagnostics#get-boot-log).</span></span> <span data-ttu-id="bce32-123">Hello následující příklad načte sériové výstup hello ze hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="bce32-123">hello following example gets hello serial output from hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="bce32-124">Zkontrolujte toodetermine sériové výstup hello proč hello virtuálního počítače selhává tooboot.</span><span class="sxs-lookup"><span data-stu-id="bce32-124">Review hello serial output toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="bce32-125">Pokud sériové výstup hello neposkytuje jakoukoli indikaci toho, může být nutné tooreview souborů protokolů v `/var/log` až budete mít hello virtuální pevný disk připojený tooa řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-125">If hello serial output isn't providing any indication, you may need tooreview log files in `/var/log` once you have hello virtual hard disk connected tooa troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="bce32-126">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="bce32-126">View existing virtual hard disk details</span></span>
<span data-ttu-id="bce32-127">Než můžete připojit vaše tooanother virtuální pevný disk (VHD) virtuálního počítače, je třeba tooidentify hello URI hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="bce32-127">Before you can attach your virtual hard disk (VHD) tooanother VM, you need tooidentify hello URI of hello OS disk.</span></span> 

<span data-ttu-id="bce32-128">Zobrazit informace o virtuální počítač s [az virtuálních počítačů zobrazit](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="bce32-128">View information about your VM with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="bce32-129">Použití hello `--query` příznak tooextract hello URI toohello operačního systému disku.</span><span class="sxs-lookup"><span data-stu-id="bce32-129">Use hello `--query` flag tooextract hello URI toohello OS disk.</span></span> <span data-ttu-id="bce32-130">Hello následující příklad načte informace o disku pro virtuální počítač s názvem hello `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="bce32-130">hello following example gets disk information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

<span data-ttu-id="bce32-131">Hello identifikátor URI je příliš podobné**https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span><span class="sxs-lookup"><span data-stu-id="bce32-131">hello URI is similar too**https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span></span>

## <a name="delete-existing-vm"></a><span data-ttu-id="bce32-132">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="bce32-132">Delete existing VM</span></span>
<span data-ttu-id="bce32-133">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="bce32-133">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="bce32-134">Virtuální pevný disk je, kde jsou uloženy hello operačního systému, samotné, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bce32-134">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="bce32-135">Hello virtuální počítač je jenom metadata, která definuje hello velikosti nebo umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="bce32-135">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="bce32-136">Každý virtuální pevný disk má zapůjčení přiřazen při připojené tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-136">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="bce32-137">I když datových disků můžete připojit a odpojit i hello virtuálních počítačů se systémem, disk operačního systému hello nejde odpojit, pokud se odstraní hello prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bce32-137">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="bce32-138">Hello zapůjčení pokračuje tooassociate hello operačního systému disku v případě virtuálních počítačů i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated.</span><span class="sxs-lookup"><span data-stu-id="bce32-138">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="bce32-139">první krok toorecover Hello virtuálního počítače je prostředků virtuálního počítače hello toodelete sám sebe.</span><span class="sxs-lookup"><span data-stu-id="bce32-139">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="bce32-140">Odstraňování hello virtuálního počítače zůstane hello virtuální pevné disky ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bce32-140">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="bce32-141">Po hello je odstranit virtuální počítač připojte hello virtuálního pevného disku tooanother virtuálních počítačů tootroubleshoot a vyřešte chyby hello.</span><span class="sxs-lookup"><span data-stu-id="bce32-141">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="bce32-142">Odstranit hello virtuálního počítače s [odstranit virtuální počítač az](/cli/azure/vm#delete).</span><span class="sxs-lookup"><span data-stu-id="bce32-142">Delete hello VM with [az vm delete](/cli/azure/vm#delete).</span></span> <span data-ttu-id="bce32-143">Následující příklad odstranění Hello hello virtuálního počítače s názvem `myVM` z hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="bce32-143">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="bce32-144">Počkejte, dokud hello virtuálních počítačů dokončí odstraňování před připojením tooanother hello virtuální pevný disk virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bce32-144">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="bce32-145">Hello zapůjčení na hello virtuální pevný disk, který přidruží k němu hello virtuálních počítačů musí toobe vydán dříve, než je možné připojit virtuální pevný disk tooanother hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-145">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="bce32-146">Připojit existující virtuální pevný disk tooanother virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="bce32-146">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="bce32-147">Pro hello vedle několik kroků, použijte jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="bce32-147">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="bce32-148">Připojte hello existující virtuální pevný disk toothis řešení potíží s toobrowse virtuálních počítačů a upravit obsah hello disku.</span><span class="sxs-lookup"><span data-stu-id="bce32-148">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="bce32-149">Tento proces vám umožní toocorrect všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů, například protokolu.</span><span class="sxs-lookup"><span data-stu-id="bce32-149">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="bce32-150">Vyberte nebo vytvořte jinou toouse virtuálních počítačů pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="bce32-150">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="bce32-151">Připojte hello existující virtuální pevný disk s [nespravované virtuální počítač az-disk připojit](/cli/azure/vm/unmanaged-disk#attach).</span><span class="sxs-lookup"><span data-stu-id="bce32-151">Attach hello existing virtual hard disk with [az vm unmanaged-disk attach](/cli/azure/vm/unmanaged-disk#attach).</span></span> <span data-ttu-id="bce32-152">Když připojíte hello existující virtuální pevný disk, zadejte hello URI toohello disk získaných v předchozím hello `az vm show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="bce32-152">When you attach hello existing virtual hard disk, specify hello URI toohello disk obtained in hello preceding `az vm show` command.</span></span> <span data-ttu-id="bce32-153">Hello následující příklad připojí existující virtuální pevný disk toohello řešení potíží s virtuálním počítačem s názvem `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="bce32-153">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="bce32-154">Připojte disk připojená data hello</span><span class="sxs-lookup"><span data-stu-id="bce32-154">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="bce32-155">Hello následující příklady podrobnosti hello kroky na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="bce32-155">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="bce32-156">Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, hello umístění souborů protokolu a `mount` příkazy se můžou mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="bce32-156">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="bce32-157">Naleznete v dokumentaci toohello pro vaše konkrétní distro hello příslušné změny v příkazy.</span><span class="sxs-lookup"><span data-stu-id="bce32-157">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="bce32-158">Řešení potíží s virtuálního počítače pomocí příslušných přihlašovacích údajů hello tooyour SSH.</span><span class="sxs-lookup"><span data-stu-id="bce32-158">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="bce32-159">Pokud tento disk je hello první datový disk připojený tooyour řešení potíží virtuální počítač, hello disk pravděpodobně připojený příliš`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="bce32-159">If this disk is hello first data disk attached tooyour troubleshooting VM, hello disk is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="bce32-160">Použití `dmseg` tooview připojenými disky:</span><span class="sxs-lookup"><span data-stu-id="bce32-160">Use `dmseg` tooview attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="bce32-161">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="bce32-161">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="bce32-162">V předchozím příkladu hello, je disk hello operačního systému na `/dev/sda` a hello dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="bce32-162">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="bce32-163">Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="bce32-163">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="bce32-164">Vytvořte adresář toomount existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="bce32-164">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="bce32-165">Hello následující příklad vytvoří adresář s názvem `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="bce32-165">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="bce32-166">Pokud máte více oddílů na existující virtuální pevný disk, připojte hello požadované oddílu.</span><span class="sxs-lookup"><span data-stu-id="bce32-166">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="bce32-167">Hello následující příklad připojí hello na první primární oddíl `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="bce32-167">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="bce32-168">Osvědčeným postupem je, že toomount datové disky na virtuálních počítačích v Azure pomocí hello identifikátor UUID (UUID) hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="bce32-168">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="bce32-169">Pro tento krátký odstraňování potíží není nutné připojování hello virtuální pevný disk pomocí hello UUID.</span><span class="sxs-lookup"><span data-stu-id="bce32-169">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="bce32-170">Ale při normálním používání úpravy `/etc/fstab` toomount virtuálních pevných disků pomocí název zařízení, nikoli UUID může způsobit hello tooboot toofail virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-170">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="bce32-171">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="bce32-171">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="bce32-172">S hello existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="bce32-172">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="bce32-173">Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="bce32-173">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="bce32-174">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="bce32-174">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="bce32-175">Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit hello existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="bce32-175">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="bce32-176">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení připojení hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-176">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="bce32-177">Z relace tooyour SSH hello řešení potíží virtuální počítač odpojte hello existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="bce32-177">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="bce32-178">Nejprve změňte mimo hello nadřazený adresář pro přípojného bodu:</span><span class="sxs-lookup"><span data-stu-id="bce32-178">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="bce32-179">Nyní odpojte hello existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="bce32-179">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="bce32-180">Hello následující příklad odpojí hello zařízení na `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="bce32-180">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="bce32-181">Nyní odpojte hello virtuálního pevného disku z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-181">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="bce32-182">Ukončete tooyour relace SSH hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bce32-182">Exit hello SSH session tooyour troubleshooting VM.</span></span> <span data-ttu-id="bce32-183">Seznam hello připojené datové disky tooyour řešení potíží virtuální počítač s [seznamu nespravované disku virtuálního počítače az](/cli/azure/vm/unmanaged-disk#list).</span><span class="sxs-lookup"><span data-stu-id="bce32-183">List hello attached data disks tooyour troubleshooting VM with [az vm unmanaged-disk list](/cli/azure/vm/unmanaged-disk#list).</span></span> <span data-ttu-id="bce32-184">Hello následující příklad zobrazí hello datových disků připojených toohello virtuálního počítače s názvem `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="bce32-184">hello following example lists hello data disks attached toohello VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    <span data-ttu-id="bce32-185">Poznamenejte si název hello pro existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="bce32-185">Note hello name for your existing virtual hard disk.</span></span> <span data-ttu-id="bce32-186">Například hello název disku s hello identifikátor URI **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** je **myVHD**.</span><span class="sxs-lookup"><span data-stu-id="bce32-186">For example, hello name of a disk with hello URI of **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** is **myVHD**.</span></span> 

    <span data-ttu-id="bce32-187">Odpojit hello datový disk od virtuálního počítače [nespravované virtuální počítač az-disk odpojit](/cli/azure/vm/unmanaged-disk#detach).</span><span class="sxs-lookup"><span data-stu-id="bce32-187">Detach hello data disk from your VM [az vm unmanaged-disk detach](/cli/azure/vm/unmanaged-disk#detach).</span></span> <span data-ttu-id="bce32-188">Hello následující příklad odpojí hello disk s názvem `myVHD` z hello virtuálního počítače s názvem `myVMRecovery` v hello `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="bce32-188">hello following example detaches hello disk named `myVHD` from hello VM named `myVMRecovery` in hello `myResourceGroup` resource group:</span></span>

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="bce32-189">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="bce32-189">Create VM from original hard disk</span></span>
<span data-ttu-id="bce32-190">použijte virtuální počítač z původní virtuální pevný disk, toocreate [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="bce32-190">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="bce32-191">Šablona JSON skutečné Hello je na hello následující odkaz:</span><span class="sxs-lookup"><span data-stu-id="bce32-191">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="bce32-192">https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bce32-192">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="bce32-193">Hello šablony nasadí virtuálního počítače pomocí hello URI virtuálního pevného disku z hello dříve příkaz.</span><span class="sxs-lookup"><span data-stu-id="bce32-193">hello template deploys a VM using hello VHD URI from hello earlier command.</span></span> <span data-ttu-id="bce32-194">Nasazení šablony hello s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="bce32-194">Deploy hello template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="bce32-195">Zadejte identifikátor URI tooyour hello původní virtuální pevný disk a pak zadejte typ hello operačního systému, velikost virtuálního počítače a název virtuálního počítače takto:</span><span class="sxs-lookup"><span data-stu-id="bce32-195">Provide hello URI tooyour original VHD and then specify hello OS type, VM size, and VM name as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="bce32-196">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="bce32-196">Re-enable boot diagnostics</span></span>
<span data-ttu-id="bce32-197">Při vytváření virtuálního počítače z hello existující virtuální pevný disk, Diagnostika spouštění není automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="bce32-197">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="bce32-198">Povolit Diagnostika spouštění s [povolit az virtuálního počítače – Diagnostika spouštění](/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="bce32-198">Enable boot diagnostics with [az vm boot-diagnostics enable](/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="bce32-199">Hello následující příklad povolí rozšíření diagnostiky hello na hello virtuálního počítače s názvem `myDeployedVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="bce32-199">hello following example enables hello diagnostic extension on hello VM named `myDeployedVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="bce32-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bce32-200">Next steps</span></span>
<span data-ttu-id="bce32-201">Pokud máte problémy s připojením tooyour virtuálních počítačů, přečtěte si téma [řešení SSH připojení tooan virtuálního počítače Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce32-201">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="bce32-202">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bce32-202">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
