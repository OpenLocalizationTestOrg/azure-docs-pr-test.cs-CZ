---
title: "aaaUse a řešení potíží s virtuálních počítačů s hello Azure CLI 1.0 Linux | Microsoft Docs"
description: "Zjistěte, jak hello tootroubleshoot, které vystavuje virtuálního počítače s Linuxem pomocí připojování hello operačního systému disku tooa obnovení virtuálního počítače Azure CLI 1.0"
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
ms.openlocfilehash: 398f681d1149299d444fcfdab20737315db02855
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-cli-10"></a><span data-ttu-id="0e005-103">Řešení potíží s virtuálního počítače s Linuxem připojením hello operačního systému disku tooa obnovení virtuálních počítačů pomocí Azure CLI 1.0 hello</span><span class="sxs-lookup"><span data-stu-id="0e005-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="0e005-104">Pokud systém Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe.</span><span class="sxs-lookup"><span data-stu-id="0e005-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="0e005-105">Běžným příkladem by neplatná položka v `/etc/fstab` , který brání hello virtuálních počítačů je možné tooboot úspěšně.</span><span class="sxs-lookup"><span data-stu-id="0e005-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="0e005-106">Tento článek podrobnosti jak toouse hello Azure CLI 1.0 tooconnect vaše virtuálního pevného disku virtuálního počítače s Linuxem toofix tooanother všechny chyby a potom ho znovu vytvořit původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="0e005-106">This article details how toouse hello Azure CLI 1.0 tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="0e005-107">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="0e005-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="0e005-108">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="0e005-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="0e005-109">[Azure CLI 1.0](#recovery-process-overview) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="0e005-109">[Azure CLI 1.0](#recovery-process-overview) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="0e005-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="0e005-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="0e005-111">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="0e005-111">Recovery process overview</span></span>
<span data-ttu-id="0e005-112">řešení potíží s procesem Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0e005-112">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="0e005-113">Odstraňte hello virtuálních počítačů zjištění problémy, udržování hello virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="0e005-113">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="0e005-114">Připojení a připojte tooanother hello virtuální pevný disk virtuálního počítače s Linuxem pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="0e005-114">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="0e005-115">Připojte toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-115">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="0e005-116">Úpravy souborů nebo spuštěním žádné nástroje toofix problémy v hello původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="0e005-116">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="0e005-117">Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-117">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="0e005-118">Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="0e005-118">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="0e005-119">Ujistěte se, že máte [hello nejnovější Azure CLI 1.0](../../cli-install-nodejs.md) nainstalován a přihlášení a pomocí režimu Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="0e005-119">Make sure that you have [hello latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="0e005-120">V hello následujících příkladech nahraďte vlastními hodnotami názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="0e005-120">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="0e005-121">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="0e005-121">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="0e005-122">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="0e005-122">Determine boot issues</span></span>
<span data-ttu-id="0e005-123">Zkontrolujte toodetermine sériové výstup hello proč virtuálního počítače není možné tooboot správně.</span><span class="sxs-lookup"><span data-stu-id="0e005-123">Examine hello serial output toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="0e005-124">Běžným příkladem jsou neplatná položka v `/etc/fstab`, nebo hello základní virtuální pevný disk se odstranil nebo přesunul.</span><span class="sxs-lookup"><span data-stu-id="0e005-124">A common example is an invalid entry in `/etc/fstab`, or hello underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="0e005-125">Hello následující příklad načte sériové výstup hello ze hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0e005-125">hello following example gets hello serial output from hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="0e005-126">Zkontrolujte toodetermine sériové výstup hello proč hello virtuálního počítače selhává tooboot.</span><span class="sxs-lookup"><span data-stu-id="0e005-126">Review hello serial output toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="0e005-127">Pokud sériové výstup hello neposkytuje jakoukoli indikaci toho, může být nutné tooreview souborů protokolů v `/var/log` až budete mít hello virtuální pevný disk připojený tooa řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-127">If hello serial output isn't providing any indication, you may need tooreview log files in `/var/log` once you have hello virtual hard disk connected tooa troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="0e005-128">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="0e005-128">View existing virtual hard disk details</span></span>
<span data-ttu-id="0e005-129">Než můžete připojit vaše tooanother virtuální pevný disk virtuálního počítače, je třeba název hello tooidentify hello virtuálního pevného disku (VHD).</span><span class="sxs-lookup"><span data-stu-id="0e005-129">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span> 

<span data-ttu-id="0e005-130">Hello následující příklad načte informace o hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0e005-130">hello following example gets information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="0e005-131">Vyhledejte `Vhd URI` v hello výstup hello předcházející příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e005-131">Look for `Vhd URI` in hello output from hello preceding command.</span></span> <span data-ttu-id="0e005-132">Hello následující zkrácený příklad výstupu zobrazuje následující stavy hello `Vhd URI` na poslední řádek hello:</span><span class="sxs-lookup"><span data-stu-id="0e005-132">hello following truncated example output shows hello `Vhd URI` on hello last line:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up hello VM "myVM"
+ Looking up hello NIC "myNic"
+ Looking up hello public ip "myPublicIP"
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


## <a name="delete-existing-vm"></a><span data-ttu-id="0e005-133">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="0e005-133">Delete existing VM</span></span>
<span data-ttu-id="0e005-134">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="0e005-134">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="0e005-135">Virtuální pevný disk je, kde jsou uloženy hello operačního systému, samotné, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0e005-135">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="0e005-136">Hello virtuální počítač je jenom metadata, která definuje hello velikosti nebo umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="0e005-136">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="0e005-137">Každý virtuální pevný disk má zapůjčení přiřazen při připojené tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-137">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="0e005-138">I když datových disků můžete připojit a odpojit i hello virtuálních počítačů se systémem, disk operačního systému hello nejde odpojit, pokud se odstraní hello prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e005-138">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="0e005-139">Hello zapůjčení pokračuje tooassociate hello operačního systému disku v případě virtuálních počítačů i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated.</span><span class="sxs-lookup"><span data-stu-id="0e005-139">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="0e005-140">první krok toorecover Hello virtuálního počítače je prostředků virtuálního počítače hello toodelete sám sebe.</span><span class="sxs-lookup"><span data-stu-id="0e005-140">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="0e005-141">Odstraňování hello virtuálního počítače zůstane hello virtuální pevné disky ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="0e005-141">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="0e005-142">Po hello je odstranit virtuální počítač připojte hello virtuálního pevného disku tooanother virtuálních počítačů tootroubleshoot a vyřešte chyby hello.</span><span class="sxs-lookup"><span data-stu-id="0e005-142">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="0e005-143">Následující příklad odstranění Hello hello virtuálního počítače s názvem `myVM` z hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0e005-143">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="0e005-144">Počkejte, dokud hello virtuálních počítačů dokončí odstraňování před připojením tooanother hello virtuální pevný disk virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e005-144">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="0e005-145">Hello zapůjčení na hello virtuální pevný disk, který přidruží k němu hello virtuálních počítačů musí toobe vydán dříve, než je možné připojit virtuální pevný disk tooanother hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-145">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="0e005-146">Připojit existující virtuální pevný disk tooanother virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e005-146">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="0e005-147">Pro hello vedle několik kroků, použijte jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="0e005-147">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="0e005-148">Připojte hello existující virtuální pevný disk toothis řešení potíží s toobrowse virtuálních počítačů a upravit obsah hello disku.</span><span class="sxs-lookup"><span data-stu-id="0e005-148">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="0e005-149">Tento proces vám umožní toocorrect všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů, například protokolu.</span><span class="sxs-lookup"><span data-stu-id="0e005-149">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="0e005-150">Vyberte nebo vytvořte jinou toouse virtuálních počítačů pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="0e005-150">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="0e005-151">Když připojíte hello existující virtuální pevný disk, zadejte hello URL toohello disk získaných v předchozím hello `azure vm show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e005-151">When you attach hello existing virtual hard disk, specify hello URL toohello disk obtained in hello preceding `azure vm show` command.</span></span> <span data-ttu-id="0e005-152">Hello následující příklad připojí existující virtuální pevný disk toohello řešení potíží s virtuálním počítačem s názvem `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0e005-152">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="0e005-153">Připojte disk připojená data hello</span><span class="sxs-lookup"><span data-stu-id="0e005-153">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="0e005-154">Hello následující příklady podrobnosti hello kroky na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="0e005-154">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="0e005-155">Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, hello umístění souborů protokolu a `mount` příkazy se můžou mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="0e005-155">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="0e005-156">Naleznete v dokumentaci toohello pro vaše konkrétní distro hello příslušné změny v příkazy.</span><span class="sxs-lookup"><span data-stu-id="0e005-156">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="0e005-157">Řešení potíží s virtuálního počítače pomocí příslušných přihlašovacích údajů hello tooyour SSH.</span><span class="sxs-lookup"><span data-stu-id="0e005-157">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="0e005-158">Pokud tento disk je hello první datový disk připojený tooyour řešení potíží virtuální počítač, hello disk pravděpodobně připojený příliš`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="0e005-158">If this disk is hello first data disk attached tooyour troubleshooting VM, hello disk is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="0e005-159">Použití `dmseg` tooview připojenými disky:</span><span class="sxs-lookup"><span data-stu-id="0e005-159">Use `dmseg` tooview attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="0e005-160">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0e005-160">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="0e005-161">V předchozím příkladu hello, je disk hello operačního systému na `/dev/sda` a hello dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="0e005-161">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="0e005-162">Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="0e005-162">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="0e005-163">Vytvořte adresář toomount existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="0e005-163">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="0e005-164">Hello následující příklad vytvoří adresář s názvem `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="0e005-164">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="0e005-165">Pokud máte více oddílů na existující virtuální pevný disk, připojte hello požadované oddílu.</span><span class="sxs-lookup"><span data-stu-id="0e005-165">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="0e005-166">Hello následující příklad připojí hello na první primární oddíl `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="0e005-166">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="0e005-167">Osvědčeným postupem je, že toomount datové disky na virtuálních počítačích v Azure pomocí hello identifikátor UUID (UUID) hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="0e005-167">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="0e005-168">Pro tento krátký odstraňování potíží není nutné připojování hello virtuální pevný disk pomocí hello UUID.</span><span class="sxs-lookup"><span data-stu-id="0e005-168">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="0e005-169">Ale při normálním používání úpravy `/etc/fstab` toomount virtuálních pevných disků pomocí název zařízení, nikoli UUID může způsobit hello tooboot toofail virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-169">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="0e005-170">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="0e005-170">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="0e005-171">S hello existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="0e005-171">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="0e005-172">Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="0e005-172">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="0e005-173">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="0e005-173">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="0e005-174">Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit hello existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="0e005-174">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="0e005-175">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení připojení hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-175">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="0e005-176">Z relace tooyour SSH hello řešení potíží virtuální počítač odpojte hello existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="0e005-176">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="0e005-177">Nejprve změňte mimo hello nadřazený adresář pro přípojného bodu:</span><span class="sxs-lookup"><span data-stu-id="0e005-177">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="0e005-178">Nyní odpojte hello existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="0e005-178">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="0e005-179">Hello následující příklad odpojí hello zařízení na `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="0e005-179">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="0e005-180">Nyní odpojte hello virtuálního pevného disku z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-180">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="0e005-181">Ukončete tooyour relace SSH hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-181">Exit hello SSH session tooyour troubleshooting VM.</span></span> <span data-ttu-id="0e005-182">V hello rozhraní příkazového řádku Azure první hello seznamu připojený datové disky tooyour řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-182">In hello Azure CLI, first list hello attached data disks tooyour troubleshooting VM.</span></span> <span data-ttu-id="0e005-183">Hello následující příklad zobrazí hello datových disků připojených toohello virtuálního počítače s názvem `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0e005-183">hello following example lists hello data disks attached toohello VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    <span data-ttu-id="0e005-184">Poznámka: hello `Lun` hodnotu pro existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="0e005-184">Note hello `Lun` value for your existing virtual hard disk.</span></span> <span data-ttu-id="0e005-185">Hello výstupu příkazu následující příklad ukazuje hello existujícího virtuálního disku připojená na logické jednotce 0:</span><span class="sxs-lookup"><span data-stu-id="0e005-185">hello following example command output shows hello existing virtual disk attached at LUN 0:</span></span>

    ```azurecli
    info:    Executing command vm disk list
    + Looking up hello VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    <span data-ttu-id="0e005-186">Odpojit hello datový disk od virtuálního počítače pomocí hello použít `Lun` hodnotu:</span><span class="sxs-lookup"><span data-stu-id="0e005-186">Detach hello data disk from your VM using hello applicable `Lun` value:</span></span>

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="0e005-187">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="0e005-187">Create VM from original hard disk</span></span>
<span data-ttu-id="0e005-188">použijte virtuální počítač z původní virtuální pevný disk, toocreate [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="0e005-188">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="0e005-189">Šablona JSON skutečné Hello je na hello následující odkaz:</span><span class="sxs-lookup"><span data-stu-id="0e005-189">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="0e005-190">https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="0e005-190">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="0e005-191">Šablona Hello nasadí virtuální počítač do existující virtuální síť pomocí hello adresu URL VHD z hello dříve příkaz.</span><span class="sxs-lookup"><span data-stu-id="0e005-191">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="0e005-192">Hello následující příklad nasadí hello šablony toohello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0e005-192">hello following example deploys hello template toohello resource group named `myResourceGroup`:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

<span data-ttu-id="0e005-193">Odpověď hello vyzve k zadání hello šablony, jako je například název virtuálního počítače (`myDeployedVM` hello v následujícím příkladu), typ operačního systému (`Linux`) a velikost virtuálního počítače (`Standard_DS1_v2`).</span><span class="sxs-lookup"><span data-stu-id="0e005-193">Answer hello prompts for hello template such as VM name (`myDeployedVM` hello following example), OS type (`Linux`), and VM size (`Standard_DS1_v2`).</span></span> <span data-ttu-id="0e005-194">Hello `osDiskVhdUri` hello je stejný jako použil při připojení hello existující virtuální pevný disk toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e005-194">hello `osDiskVhdUri` is hello same as previously used when attaching hello existing virtual hard disk toohello troubleshooting VM.</span></span> <span data-ttu-id="0e005-195">Příklad výstupu příkazu hello a vyzve vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0e005-195">An example of hello command output and prompts is as follows:</span></span>

```azurecli
info:    Executing command group deployment create
info:    Supply values for hello following parameters
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
+ Waiting for deployment toocomplete
+
```


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="0e005-196">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="0e005-196">Re-enable boot diagnostics</span></span>

<span data-ttu-id="0e005-197">Při vytváření virtuálního počítače z hello existující virtuální pevný disk, Diagnostika spouštění není automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="0e005-197">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="0e005-198">Hello následující příklad povolí rozšíření diagnostiky hello na hello virtuálního počítače s názvem `myDeployedVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0e005-198">hello following example enables hello diagnostic extension on hello VM named `myDeployedVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="0e005-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e005-199">Next steps</span></span>
<span data-ttu-id="0e005-200">Pokud máte problémy s připojením tooyour virtuálních počítačů, přečtěte si téma [řešení SSH připojení tooan virtuálního počítače Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0e005-200">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="0e005-201">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0e005-201">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
