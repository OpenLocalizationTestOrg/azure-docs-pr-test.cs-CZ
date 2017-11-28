---
title: "aaaUse a Linux, řešení potíží s virtuálních počítačů v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello tootroubleshoot Linux virtuálního počítače problémy pomocí připojování hello operačního systému disku tooa obnovení virtuálního počítače pomocí portálu Azure"
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
ms.date: 11/14/2016
ms.author: iainfou
ms.openlocfilehash: 794daa06d7436215af84a61ab9088524254c47df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a><span data-ttu-id="2a2a2-103">Řešení potíží s virtuálního počítače s Linuxem připojením obnovení tooa disku hello operačního systému virtuálního počítače pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2a2a2-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM using hello Azure portal</span></span>
<span data-ttu-id="2a2a2-104">Pokud systém Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="2a2a2-105">Běžným příkladem by neplatná položka v `/etc/fstab` , který brání hello virtuálních počítačů je možné tooboot úspěšně.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="2a2a2-106">Tento článek podrobnosti jak toouse hello Azure portálu tooconnect vaše virtuálního pevného disku virtuálního počítače s Linuxem toofix tooanother všechny chyby a potom ho znovu vytvořit původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-106">This article details how toouse hello Azure portal tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span>

## <a name="recovery-process-overview"></a><span data-ttu-id="2a2a2-107">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="2a2a2-107">Recovery process overview</span></span>
<span data-ttu-id="2a2a2-108">řešení potíží s procesem Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-108">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="2a2a2-109">Odstraňte hello virtuálních počítačů zjištění problémy, udržování hello virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-109">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="2a2a2-110">Připojení a připojte tooanother hello virtuální pevný disk virtuálního počítače s Linuxem pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-110">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="2a2a2-111">Připojte toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-111">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="2a2a2-112">Úpravy souborů nebo spuštěním žádné nástroje toofix problémy v hello původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-112">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="2a2a2-113">Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-113">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="2a2a2-114">Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-114">Create a VM using hello original virtual hard disk.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="2a2a2-115">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="2a2a2-115">Determine boot issues</span></span>
<span data-ttu-id="2a2a2-116">Zkontrolujte Diagnostika spouštění hello a toodetermine snímek virtuálního počítače, proč váš virtuální počítač není možné tooboot správně.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-116">Examine hello boot diagnostics and VM screenshot toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="2a2a2-117">Běžným příkladem by neplatná položka v `/etc/fstab`, nebo virtuální pevný disk, na kterém se odstranil nebo přesunul.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-117">A common example would be an invalid entry in `/etc/fstab`, or an underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="2a2a2-118">Vyberte virtuální počítač hello portálu a posuňte se dolů toohello **podporu + Poradce při potížích s** části.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-118">Select your VM in hello portal and then scroll down toohello **Support + Troubleshooting** section.</span></span> <span data-ttu-id="2a2a2-119">Klikněte na tlačítko **spouštění diagnostiky** tooview hello konzoly zprávy pomocí datového proudu vysílána z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-119">Click **Boot diagnostics** tooview hello console messages streamed from your VM.</span></span> <span data-ttu-id="2a2a2-120">Pokud můžete zjistit, proč hello virtuálního počítače došlo k problému, protokoly konzoly hello zkontrolujte toosee.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-120">Review hello console logs toosee if you can determine why hello VM is encountering an issue.</span></span> <span data-ttu-id="2a2a2-121">Hello následující příklad ukazuje, že virtuální počítač zasekla v automatickém režimu údržby, který vyžaduje ruční zásah:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-121">hello following example shows a VM stuck in maintenance mode that requires manual interaction:</span></span>

![Zobrazení virtuálních počítačů Diagnostika spouštění protokoly konzoly](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

<span data-ttu-id="2a2a2-123">Můžete také kliknout na **– snímek obrazovky** hello horním okraji hello spouštění diagnostiky protokolu toodownload zachycení hello snímek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-123">You can also click **Screenshot** across hello top of hello boot diagnostics log toodownload a capture of hello VM screenshot.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="2a2a2-124">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="2a2a2-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="2a2a2-125">Než můžete připojit vaše tooanother virtuální pevný disk virtuálního počítače, je třeba název hello tooidentify hello virtuálního pevného disku (VHD).</span><span class="sxs-lookup"><span data-stu-id="2a2a2-125">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span> 

<span data-ttu-id="2a2a2-126">Vyberte skupinu prostředků z portálu hello a potom vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-126">Select your resource group from hello portal, then select your storage account.</span></span> <span data-ttu-id="2a2a2-127">Klikněte na tlačítko **objekty BLOB**, jako v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-127">Click **Blobs**, as in hello following example:</span></span>

![Vyberte úložiště objektů BLOB](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

<span data-ttu-id="2a2a2-129">Obvykle mají kontejner s názvem **virtuální pevné disky** , ukládá virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-129">Typically you have a container named **vhds** that stores your virtual hard disks.</span></span> <span data-ttu-id="2a2a2-130">Vyberte kontejner tooview hello seznam virtuálních pevných disků.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-130">Select hello container tooview a list of virtual hard disks.</span></span> <span data-ttu-id="2a2a2-131">Poznámka: hello název vašeho virtuálního pevného disku (hello předpona je obvykle hello název vašeho virtuálního počítače):</span><span class="sxs-lookup"><span data-stu-id="2a2a2-131">Note hello name of your VHD (hello prefix is usually hello name of your VM):</span></span>

![Identifikovat virtuální pevný disk v kontejneru úložiště](./media/troubleshoot-recovery-disks-portal/storage-container.png)

<span data-ttu-id="2a2a2-133">Vyberte ze seznamu hello existující virtuální pevný disk a zkopírujte hello URL pro použití v hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-133">Select your existing virtual hard disk from hello list and copy hello URL for use in hello following steps:</span></span>

![Zkopírujte adresu URL existující virtuální pevný disk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a><span data-ttu-id="2a2a2-135">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="2a2a2-135">Delete existing VM</span></span>
<span data-ttu-id="2a2a2-136">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-136">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="2a2a2-137">Virtuální pevný disk je, kde jsou uloženy hello operačního systému, samotné, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-137">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="2a2a2-138">Hello virtuální počítač je jenom metadata, která definuje hello velikosti nebo umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="2a2a2-138">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="2a2a2-139">Každý virtuální pevný disk má zapůjčení přiřazen při připojené tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-139">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="2a2a2-140">I když datových disků můžete připojit a odpojit i hello virtuálních počítačů se systémem, disk operačního systému hello nejde odpojit, pokud se odstraní hello prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-140">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="2a2a2-141">Hello zapůjčení pokračuje tooassociate hello operačního systému disku v případě virtuálních počítačů i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-141">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="2a2a2-142">první krok toorecover Hello virtuálního počítače je prostředků virtuálního počítače hello toodelete sám sebe.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-142">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="2a2a2-143">Odstraňování hello virtuálního počítače zůstane hello virtuální pevné disky ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-143">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="2a2a2-144">Po hello je odstranit virtuální počítač připojte hello virtuálního pevného disku tooanother virtuálních počítačů tootroubleshoot a vyřešte chyby hello.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-144">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="2a2a2-145">Vyberte virtuální počítač hello portálu a pak klikněte na tlačítko **odstranit**:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-145">Select your VM in hello portal, then click **Delete**:</span></span>

![Virtuální počítač spouštěcí diagnostiky snímek obrazovky zobrazující chyby spouštěcí](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

<span data-ttu-id="2a2a2-147">Počkejte, dokud hello virtuálních počítačů dokončí odstraňování před připojením tooanother hello virtuální pevný disk virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-147">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="2a2a2-148">Hello zapůjčení na hello virtuální pevný disk, který přidruží k němu hello virtuálních počítačů musí toobe vydán dříve, než je možné připojit virtuální pevný disk tooanother hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-148">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="2a2a2-149">Připojit existující virtuální pevný disk tooanother virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="2a2a2-149">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="2a2a2-150">Pro hello vedle několik kroků, použijte jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-150">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="2a2a2-151">Připojte hello existující virtuální pevný disk toothis řešení potíží s možné toobrowse toobe virtuálních počítačů a upravit obsah hello disku.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-151">You attach hello existing virtual hard disk toothis troubleshooting VM toobe able toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="2a2a2-152">Tento proces vám umožní toocorrect všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů, například protokolu.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-152">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="2a2a2-153">Vyberte nebo vytvořte jinou toouse virtuálních počítačů pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-153">Choose or create another VM toouse for troubleshooting purposes.</span></span>

1. <span data-ttu-id="2a2a2-154">Vyberte skupinu prostředků z portálu hello a potom vyberte řešení potíží virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-154">Select your resource group from hello portal, then select your troubleshooting VM.</span></span> <span data-ttu-id="2a2a2-155">Vyberte **disky** a pak klikněte na **připojit existující**:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-155">Select **Disks** and then click **Attach existing**:</span></span>

    ![Připojit stávající disk hello portálu](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. <span data-ttu-id="2a2a2-157">tooselect existující virtuální pevný disk, klikněte na tlačítko **souboru virtuálního pevného disku**:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-157">tooselect your existing virtual hard disk, click **VHD File**:</span></span>

    ![Vyhledání existujícího VHD procházením](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. <span data-ttu-id="2a2a2-159">Vyberte účet úložiště a kontejneru a pak klikněte na existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-159">Select your storage account and container, then click your existing VHD.</span></span> <span data-ttu-id="2a2a2-160">Klikněte na tlačítko hello **vyberte** tlačítko tooconfirm zvoleného:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-160">Click hello **Select** button tooconfirm your choice:</span></span>

    ![Výběr existujícího VHD](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. <span data-ttu-id="2a2a2-162">S svůj disk VHD nyní vybraný, klikněte na tlačítko **OK** tooattach hello existující virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-162">With your VHD now selected, click **OK** tooattach hello existing virtual hard disk:</span></span>

    ![Zkontrolujte připojení existujícího virtuálního pevného disku](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. <span data-ttu-id="2a2a2-164">Za několik sekund, hello **disky** podokno pro virtuální počítač obsahuje existující virtuální pevný disk připojený jako datový disk:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-164">After a few seconds, hello **Disks** pane for your VM lists your existing virtual hard disk connected as a data disk:</span></span>

    ![Existující virtuální pevný disk připojený jako datový disk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="2a2a2-166">Připojte disk připojená data hello</span><span class="sxs-lookup"><span data-stu-id="2a2a2-166">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="2a2a2-167">Hello následující příklady podrobnosti hello kroky na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-167">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="2a2a2-168">Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, hello umístění souborů protokolu a `mount` příkazy se můžou mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-168">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="2a2a2-169">Naleznete v dokumentaci toohello pro vaše konkrétní distro hello příslušné změny v příkazy.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-169">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="2a2a2-170">Řešení potíží s virtuálního počítače pomocí příslušných přihlašovacích údajů hello tooyour SSH.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-170">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="2a2a2-171">Pokud tento disk je hello první datový disk připojený tooyour řešení potíží s virtuálních počítačů, je pravděpodobně připojený příliš`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-171">If this disk is hello first data disk attached tooyour troubleshooting VM, it is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="2a2a2-172">Použití `dmseg` toolist připojenými disky:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-172">Use `dmseg` toolist attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```
    <span data-ttu-id="2a2a2-173">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-173">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="2a2a2-174">V předchozím příkladu hello, je disk hello operačního systému na `/dev/sda` a hello dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-174">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="2a2a2-175">Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-175">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="2a2a2-176">Vytvořte adresář toomount existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-176">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="2a2a2-177">Hello následující příklad vytvoří adresář s názvem `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-177">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="2a2a2-178">Pokud máte více oddílů na existující virtuální pevný disk, připojte hello požadované oddílu.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-178">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="2a2a2-179">Hello následující příklad připojí hello na první primární oddíl `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-179">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="2a2a2-180">Osvědčeným postupem je, že toomount datové disky na virtuálních počítačích v Azure pomocí hello identifikátor UUID (UUID) hello virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-180">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="2a2a2-181">Pro tento krátký odstraňování potíží není nutné připojování hello virtuální pevný disk pomocí hello UUID.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-181">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="2a2a2-182">Ale při normálním používání úpravy `/etc/fstab` toomount virtuálních pevných disků pomocí název zařízení, nikoli UUID může způsobit hello tooboot toofail virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-182">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="2a2a2-183">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="2a2a2-183">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="2a2a2-184">S hello existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-184">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="2a2a2-185">Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-185">Once you have addressed hello issues, continue with hello following steps.</span></span>

## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="2a2a2-186">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="2a2a2-186">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="2a2a2-187">Jakmile jsou vaše chyby vyřešeny, odpojte hello existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-187">Once your errors are resolved, detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="2a2a2-188">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení připojení hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-188">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="2a2a2-189">Z relace tooyour SSH hello řešení potíží virtuální počítač odpojte hello existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-189">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="2a2a2-190">Nejprve změňte mimo hello nadřazený adresář pro přípojného bodu:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-190">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="2a2a2-191">Nyní odpojte hello existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-191">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="2a2a2-192">Hello následující příklad odpojí hello zařízení na `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-192">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="2a2a2-193">Nyní odpojte hello virtuálního pevného disku z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-193">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="2a2a2-194">Vyberte virtuální počítač hello portálu a klikněte na **disky**.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-194">Select your VM in hello portal and click **Disks**.</span></span> <span data-ttu-id="2a2a2-195">Vyberte existující virtuální pevný disk a potom klikněte na **odpojení**:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-195">Select your existing virtual hard disk and then click **Detach**:</span></span>

    ![Odpojte existující virtuální pevný disk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    <span data-ttu-id="2a2a2-197">Počkejte, dokud hello virtuálního počítače úspěšně odpojil hello datový disk než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-197">Wait until hello VM has successfully detached hello data disk before continuing.</span></span>

## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="2a2a2-198">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="2a2a2-198">Create VM from original hard disk</span></span>
<span data-ttu-id="2a2a2-199">použijte virtuální počítač z původní virtuální pevný disk, toocreate [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="2a2a2-199">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="2a2a2-200">Šablona Hello nasadí virtuální počítač do existující virtuální síť pomocí hello adresu URL VHD z hello dříve příkaz.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-200">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="2a2a2-201">Klikněte na tlačítko hello **nasazení tooAzure** tlačítko následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-201">Click hello **Deploy tooAzure** button as follows:</span></span>

![Nasazení virtuálních počítačů z šablony z Githubu](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

<span data-ttu-id="2a2a2-203">Šablona Hello je načten do hello portál Azure pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-203">hello template is loaded into hello Azure portal for deployment.</span></span> <span data-ttu-id="2a2a2-204">Zadejte názvy hello pro nový virtuální počítač a prostředky existující Azure a vložte hello URL tooyour existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-204">Enter hello names for your new VM and existing Azure resources, and paste hello URL tooyour existing virtual hard disk.</span></span> <span data-ttu-id="2a2a2-205">toobegin hello nasazení, klikněte na tlačítko **nákupu**:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-205">toobegin hello deployment, click **Purchase**:</span></span>

![Nasazení virtuálního počítače ze šablony](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="2a2a2-207">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="2a2a2-207">Re-enable boot diagnostics</span></span>
<span data-ttu-id="2a2a2-208">Při vytváření virtuálního počítače z hello existující virtuální pevný disk, Diagnostika spouštění není automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-208">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="2a2a2-209">toocheck hello stav Diagnostika spouštění a zapnout, vyberte virtuální počítač hello portálu v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-209">toocheck hello status of boot diagnostics and turn on if needed, select your VM in hello portal.</span></span> <span data-ttu-id="2a2a2-210">V části **monitorování**, klikněte na tlačítko **nastavení diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-210">Under **Monitoring**, click **Diagnostics settings**.</span></span> <span data-ttu-id="2a2a2-211">Zkontrolujte stav hello **na**, a hello zaškrtnutí vedle příliš**spouštění diagnostiky** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="2a2a2-211">Ensure hello status is **On**, and hello check mark next too**Boot diagnostics** is selected.</span></span> <span data-ttu-id="2a2a2-212">Pokud provedete změny, klikněte na tlačítko **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="2a2a2-212">If you make any changes, click **Save**:</span></span>

![Aktualizovat nastavení diagnostiky spouštění](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="2a2a2-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a2a2-214">Next steps</span></span>
<span data-ttu-id="2a2a2-215">Pokud máte problémy s připojením tooyour virtuálních počítačů, přečtěte si téma [řešení SSH připojení tooan virtuálního počítače Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2a2a2-215">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="2a2a2-216">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2a2a2-216">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="2a2a2-217">Další informace o používání správce prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2a2a2-217">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
