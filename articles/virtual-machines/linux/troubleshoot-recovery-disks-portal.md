---
title: "Použít Linux, řešení potíží s virtuálního počítače na portálu Azure | Microsoft Docs"
description: "Zjistěte, jak k řešení potíží virtuální počítač Linux připojením disk operačního systému k obnovení virtuálního počítače pomocí portálu Azure"
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
ms.openlocfilehash: c96ff625c3e83f6fc9057f1163c877e8e0aed5e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a><span data-ttu-id="5b92e-103">Odstranění virtuálního počítače s Linuxem pomocí disk operačního systému se připojuje k obnovení virtuálního počítače pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5b92e-103">Troubleshoot a Linux VM by attaching the OS disk to a recovery VM using the Azure portal</span></span>
<span data-ttu-id="5b92e-104">Pokud Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, musíte provést na virtuálním pevném disku, sám sebe pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="5b92e-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="5b92e-105">Běžným příkladem by neplatná položka v `/etc/fstab` , který brání virtuálního počítače se úspěšně spustil.</span><span class="sxs-lookup"><span data-stu-id="5b92e-105">A common example would be an invalid entry in `/etc/fstab` that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="5b92e-106">Tento článek popisuje, jak připojit virtuální pevný disk na jiný virtuální počítač opravte případné chyby a pak znovu vytvořte původní virtuální počítač s Linuxem pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5b92e-106">This article details how to use the Azure portal to connect your virtual hard disk to another Linux VM to fix any errors, then re-create your original VM.</span></span>

## <a name="recovery-process-overview"></a><span data-ttu-id="5b92e-107">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="5b92e-107">Recovery process overview</span></span>
<span data-ttu-id="5b92e-108">Proces řešení potíží je následující:</span><span class="sxs-lookup"><span data-stu-id="5b92e-108">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="5b92e-109">Odstraňte virtuální počítač, na zjištění problémy, zachovat virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="5b92e-109">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="5b92e-110">Připojte a připojit virtuální pevný disk na jiný virtuální počítač s Linuxem pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="5b92e-110">Attach and mount the virtual hard disk to another Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="5b92e-111">Připojení k virtuálnímu počítači pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="5b92e-111">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="5b92e-112">Úpravy souborů nebo spustit žádné nástroje na opravte problémy v původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-112">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="5b92e-113">Odpojení virtuálního pevného disku od virtuálního počítače pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="5b92e-113">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="5b92e-114">Vytvoření virtuálního počítače pomocí původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-114">Create a VM using the original virtual hard disk.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="5b92e-115">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="5b92e-115">Determine boot issues</span></span>
<span data-ttu-id="5b92e-116">Zkontrolujte snímky obrazovky virtuálních počítačů a zjistit, proč je virtuální počítač není možné správně spustit a Diagnostika spouštění.</span><span class="sxs-lookup"><span data-stu-id="5b92e-116">Examine the boot diagnostics and VM screenshot to determine why your VM is not able to boot correctly.</span></span> <span data-ttu-id="5b92e-117">Běžným příkladem by neplatná položka v `/etc/fstab`, nebo virtuální pevný disk, na kterém se odstranil nebo přesunul.</span><span class="sxs-lookup"><span data-stu-id="5b92e-117">A common example would be an invalid entry in `/etc/fstab`, or an underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="5b92e-118">Vyberte virtuální počítač na portálu a poté přejděte dolů k **podporu + Poradce při potížích s** části.</span><span class="sxs-lookup"><span data-stu-id="5b92e-118">Select your VM in the portal and then scroll down to the **Support + Troubleshooting** section.</span></span> <span data-ttu-id="5b92e-119">Klikněte na tlačítko **spouštění diagnostiky** zobrazení zpráv konzoly pomocí datového proudu vysílána z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b92e-119">Click **Boot diagnostics** to view the console messages streamed from your VM.</span></span> <span data-ttu-id="5b92e-120">Zkontrolujte protokoly konzoly zobrazíte, pokud je určit, proč je virtuální počítač zjištění problému.</span><span class="sxs-lookup"><span data-stu-id="5b92e-120">Review the console logs to see if you can determine why the VM is encountering an issue.</span></span> <span data-ttu-id="5b92e-121">Následující příklad ukazuje, že virtuální počítač zasekla v automatickém režimu údržby, který vyžaduje ruční zásah:</span><span class="sxs-lookup"><span data-stu-id="5b92e-121">The following example shows a VM stuck in maintenance mode that requires manual interaction:</span></span>

![Zobrazení virtuálních počítačů Diagnostika spouštění protokoly konzoly](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

<span data-ttu-id="5b92e-123">Můžete také kliknout na **– snímek obrazovky** v horní části protokolu diagnostiky spouštěcí stáhnout zachycení snímku obrazovky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5b92e-123">You can also click **Screenshot** across the top of the boot diagnostics log to download a capture of the VM screenshot.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="5b92e-124">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="5b92e-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="5b92e-125">Než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk, musíte určit název virtuálního pevného disku (VHD).</span><span class="sxs-lookup"><span data-stu-id="5b92e-125">Before you can attach your virtual hard disk to another VM, you need to identify the name of the virtual hard disk (VHD).</span></span> 

<span data-ttu-id="5b92e-126">Vyberte skupinu prostředků z portálu a potom vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="5b92e-126">Select your resource group from the portal, then select your storage account.</span></span> <span data-ttu-id="5b92e-127">Klikněte na tlačítko **objekty BLOB**, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5b92e-127">Click **Blobs**, as in the following example:</span></span>

![Vyberte úložiště objektů BLOB](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

<span data-ttu-id="5b92e-129">Obvykle mají kontejner s názvem **virtuální pevné disky** , ukládá virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="5b92e-129">Typically you have a container named **vhds** that stores your virtual hard disks.</span></span> <span data-ttu-id="5b92e-130">Vyberte kontejner zobrazení seznamu virtuálních pevných disků.</span><span class="sxs-lookup"><span data-stu-id="5b92e-130">Select the container to view a list of virtual hard disks.</span></span> <span data-ttu-id="5b92e-131">Poznamenejte si název vašeho virtuálního pevného disku (předpona, která je obvykle název vašeho virtuálního počítače):</span><span class="sxs-lookup"><span data-stu-id="5b92e-131">Note the name of your VHD (the prefix is usually the name of your VM):</span></span>

![Identifikovat virtuální pevný disk v kontejneru úložiště](./media/troubleshoot-recovery-disks-portal/storage-container.png)

<span data-ttu-id="5b92e-133">Vyberte ze seznamu existující virtuální pevný disk a zkopírujte adresu URL pro použití v následujících krocích:</span><span class="sxs-lookup"><span data-stu-id="5b92e-133">Select your existing virtual hard disk from the list and copy the URL for use in the following steps:</span></span>

![Zkopírujte adresu URL existující virtuální pevný disk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a><span data-ttu-id="5b92e-135">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="5b92e-135">Delete existing VM</span></span>
<span data-ttu-id="5b92e-136">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="5b92e-136">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="5b92e-137">Virtuální pevný disk je, kde jsou uloženy samotného operačního systému, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5b92e-137">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="5b92e-138">Virtuální počítač je jenom metadata, která definuje velikosti či umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="5b92e-138">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="5b92e-139">Každý virtuální pevný disk má zapůjčení přiřazen při připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5b92e-139">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="5b92e-140">Přestože datové disky je možné připojovat a odpojovat dokonce i za běhu virtuálního počítače, disk s operačním systémem není možné odpojit, dokud se neodstraní prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b92e-140">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="5b92e-141">Zapůjčení i nadále i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated přidružení disk operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b92e-141">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="5b92e-142">Prvním krokem k obnovení virtuálního počítače je odstranit samotné prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b92e-142">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="5b92e-143">Když odstraníte virtuální počítač, virtuální pevné disky zůstanou ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5b92e-143">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="5b92e-144">Po odstranění virtuálního počítače připojit virtuální pevný disk k jiným virtuálním Počítačem vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="5b92e-144">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="5b92e-145">Vyberte virtuální počítač na portálu a pak klikněte na tlačítko **odstranit**:</span><span class="sxs-lookup"><span data-stu-id="5b92e-145">Select your VM in the portal, then click **Delete**:</span></span>

![Virtuální počítač spouštěcí diagnostiky snímek obrazovky zobrazující chyby spouštěcí](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

<span data-ttu-id="5b92e-147">Počkejte, dokud je virtuální počítač dokončí odstraňování před připojit virtuální pevný disk k jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="5b92e-147">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="5b92e-148">Zapůjčení na virtuální pevný disk, který přidruží k němu virtuální počítač je nutné uvolnit předtím, než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-148">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="5b92e-149">Připojit existující virtuální pevný disk k jiným virtuálním Počítačem</span><span class="sxs-lookup"><span data-stu-id="5b92e-149">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="5b92e-150">Pro několika dalších krocích použijete jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="5b92e-150">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="5b92e-151">Existující virtuální pevný disk se připojit k řešení potíží VM být schopni prohlížet a upravovat obsah na disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-151">You attach the existing virtual hard disk to this troubleshooting VM to be able to browse and edit the disk's content.</span></span> <span data-ttu-id="5b92e-152">Tento proces umožňuje opravte všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů protokolu, např.</span><span class="sxs-lookup"><span data-stu-id="5b92e-152">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="5b92e-153">Vyberte nebo vytvořte jiným virtuálním Počítačem používat pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="5b92e-153">Choose or create another VM to use for troubleshooting purposes.</span></span>

1. <span data-ttu-id="5b92e-154">Vyberte skupinu prostředků z portálu a potom vyberte řešení potíží virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b92e-154">Select your resource group from the portal, then select your troubleshooting VM.</span></span> <span data-ttu-id="5b92e-155">Vyberte **disky** a pak klikněte na **připojit existující**:</span><span class="sxs-lookup"><span data-stu-id="5b92e-155">Select **Disks** and then click **Attach existing**:</span></span>

    ![Připojit stávající disk na portálu](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. <span data-ttu-id="5b92e-157">Klikněte na **Soubor VHD** a vyberte váš existující virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="5b92e-157">To select your existing virtual hard disk, click **VHD File**:</span></span>

    ![Vyhledání existujícího VHD procházením](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. <span data-ttu-id="5b92e-159">Vyberte účet úložiště a kontejneru a pak klikněte na existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-159">Select your storage account and container, then click your existing VHD.</span></span> <span data-ttu-id="5b92e-160">Klikněte **vyberte** tlačítko potvrďte svou volbu:</span><span class="sxs-lookup"><span data-stu-id="5b92e-160">Click the **Select** button to confirm your choice:</span></span>

    ![Výběr existujícího VHD](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. <span data-ttu-id="5b92e-162">S svůj disk VHD nyní vybraný, klikněte na tlačítko **OK** připojit existující virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="5b92e-162">With your VHD now selected, click **OK** to attach the existing virtual hard disk:</span></span>

    ![Zkontrolujte připojení existujícího virtuálního pevného disku](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. <span data-ttu-id="5b92e-164">Za několik sekund **disky** podokno pro virtuální počítač obsahuje existující virtuální pevný disk připojený jako datový disk:</span><span class="sxs-lookup"><span data-stu-id="5b92e-164">After a few seconds, the **Disks** pane for your VM lists your existing virtual hard disk connected as a data disk:</span></span>

    ![Existující virtuální pevný disk připojený jako datový disk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="5b92e-166">Připojit disk připojená data</span><span class="sxs-lookup"><span data-stu-id="5b92e-166">Mount the attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="5b92e-167">Následující příklady jsou upřesněny kroky na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-167">The following examples detail the steps required on an Ubuntu VM.</span></span> <span data-ttu-id="5b92e-168">Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, umístění souborů protokolu a `mount` příkazy se můžou mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="5b92e-168">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, the log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="5b92e-169">Naleznete v dokumentaci k vaší konkrétní distro pro příslušné změny v příkazech.</span><span class="sxs-lookup"><span data-stu-id="5b92e-169">Refer to the documentation for your specific distro for the appropriate changes in commands.</span></span>

1. <span data-ttu-id="5b92e-170">SSH k řešení potíží virtuální počítač pomocí příslušných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5b92e-170">SSH to your troubleshooting VM using the appropriate credentials.</span></span> <span data-ttu-id="5b92e-171">Pokud tento disk je první datový disk připojen k řešení potíží virtuální počítač, je pravděpodobně připojený k `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="5b92e-171">If this disk is the first data disk attached to your troubleshooting VM, it is likely connected to `/dev/sdc`.</span></span> <span data-ttu-id="5b92e-172">Použití `dmseg` seznam připojených disků:</span><span class="sxs-lookup"><span data-stu-id="5b92e-172">Use `dmseg` to list attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```
    <span data-ttu-id="5b92e-173">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="5b92e-173">The output is similar to the following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="5b92e-174">V předchozím příkladu je disk operačního systému na `/dev/sda` a dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="5b92e-174">In the preceding example, the OS disk is at `/dev/sda` and the temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="5b92e-175">Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5b92e-175">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="5b92e-176">Vytvořte adresář připojit existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-176">Create a directory to mount your existing virtual hard disk.</span></span> <span data-ttu-id="5b92e-177">Následující příklad vytvoří adresář s názvem `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="5b92e-177">The following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="5b92e-178">Pokud máte více oddílů na existující virtuální pevný disk, připojte požadovaný oddíl.</span><span class="sxs-lookup"><span data-stu-id="5b92e-178">If you have multiple partitions on your existing virtual hard disk, mount the required partition.</span></span> <span data-ttu-id="5b92e-179">Následující příklad připojí na první primární oddíl `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="5b92e-179">The following example mounts the first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="5b92e-180">Osvědčeným postupem je připojit datové disky na virtuální počítače v Azure pomocí identifikátor (UUID) virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="5b92e-180">Best practice is to mount data disks on VMs in Azure using the universally unique identifier (UUID) of the virtual hard disk.</span></span> <span data-ttu-id="5b92e-181">Tento krátký odstraňování potíží připojení virtuálního pevného disku pomocí identifikátoru UUID není nezbytné.</span><span class="sxs-lookup"><span data-stu-id="5b92e-181">For this short troubleshooting scenario, mounting the virtual hard disk using the UUID is not necessary.</span></span> <span data-ttu-id="5b92e-182">Ale při normálním používání úpravy `/etc/fstab` připojit virtuální pevné disky použití název zařízení, nikoli UUID může způsobit selhání spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b92e-182">However, under normal use, editing `/etc/fstab` to mount virtual hard disks using device name rather than UUID may cause the VM to fail to boot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="5b92e-183">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="5b92e-183">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="5b92e-184">S existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5b92e-184">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="5b92e-185">Jakmile vyřešíte problémy, pokračujte následujícími kroky.</span><span class="sxs-lookup"><span data-stu-id="5b92e-185">Once you have addressed the issues, continue with the following steps.</span></span>

## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="5b92e-186">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="5b92e-186">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="5b92e-187">Jakmile jsou vaše chyby vyřešeny, odpojte existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="5b92e-187">Once your errors are resolved, detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="5b92e-188">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání zapůjčení virtuální pevný disk se připojuje k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5b92e-188">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="5b92e-189">Z relace SSH k řešení potíží virtuální počítač odpojte existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-189">From the SSH session to your troubleshooting VM, unmount the existing virtual hard disk.</span></span> <span data-ttu-id="5b92e-190">Nejprve změňte mimo nadřazený adresář pro přípojného bodu:</span><span class="sxs-lookup"><span data-stu-id="5b92e-190">Change out of the parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="5b92e-191">Nyní odpojte existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-191">Now unmount the existing virtual hard disk.</span></span> <span data-ttu-id="5b92e-192">Následující příklad odpojí zařízení na `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="5b92e-192">The following example unmounts the device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="5b92e-193">Nyní Odpojte virtuální pevný disk z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b92e-193">Now detach the virtual hard disk from the VM.</span></span> <span data-ttu-id="5b92e-194">Vyberte virtuální počítač v portálu a klikněte na **disky**.</span><span class="sxs-lookup"><span data-stu-id="5b92e-194">Select your VM in the portal and click **Disks**.</span></span> <span data-ttu-id="5b92e-195">Vyberte existující virtuální pevný disk a potom klikněte na **odpojení**:</span><span class="sxs-lookup"><span data-stu-id="5b92e-195">Select your existing virtual hard disk and then click **Detach**:</span></span>

    ![Odpojte existující virtuální pevný disk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    <span data-ttu-id="5b92e-197">Počkejte, dokud virtuálního počítače úspěšně odpojil datový disk než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="5b92e-197">Wait until the VM has successfully detached the data disk before continuing.</span></span>

## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="5b92e-198">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="5b92e-198">Create VM from original hard disk</span></span>
<span data-ttu-id="5b92e-199">Chcete-li vytvořit virtuální počítač z původní virtuální pevný disk, použijte [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="5b92e-199">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="5b92e-200">Šablona nasadí virtuální počítač do existující virtuální síť pomocí adresy URL virtuálního pevného disku z dřívějších příkazu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-200">The template deploys a VM into an existing virtual network, using the VHD URL from the earlier command.</span></span> <span data-ttu-id="5b92e-201">Klikněte **nasadit do Azure** tlačítko následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5b92e-201">Click the **Deploy to Azure** button as follows:</span></span>

![Nasazení virtuálních počítačů z šablony z Githubu](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

<span data-ttu-id="5b92e-203">Šablona je načten do portálu Azure pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="5b92e-203">The template is loaded into the Azure portal for deployment.</span></span> <span data-ttu-id="5b92e-204">Zadejte názvy pro nový virtuální počítač a existující prostředky Azure a vložte adresu URL na existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="5b92e-204">Enter the names for your new VM and existing Azure resources, and paste the URL to your existing virtual hard disk.</span></span> <span data-ttu-id="5b92e-205">Chcete-li zahájit nasazení, klikněte na tlačítko **nákupu**:</span><span class="sxs-lookup"><span data-stu-id="5b92e-205">To begin the deployment, click **Purchase**:</span></span>

![Nasazení virtuálního počítače ze šablony](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="5b92e-207">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="5b92e-207">Re-enable boot diagnostics</span></span>
<span data-ttu-id="5b92e-208">Při vytváření virtuálního počítače z existujícího virtuálního pevného disku, nemusí být Diagnostika spouštění automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="5b92e-208">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="5b92e-209">Zkontrolujte stav Diagnostika spouštění a v případě potřeby zapnout, vyberte virtuální počítač na portálu.</span><span class="sxs-lookup"><span data-stu-id="5b92e-209">To check the status of boot diagnostics and turn on if needed, select your VM in the portal.</span></span> <span data-ttu-id="5b92e-210">V části **monitorování**, klikněte na tlačítko **nastavení diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="5b92e-210">Under **Monitoring**, click **Diagnostics settings**.</span></span> <span data-ttu-id="5b92e-211">Ujistěte se, že stav je **na**a zaškrtnutí políčka vedle **spouštění diagnostiky** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="5b92e-211">Ensure the status is **On**, and the check mark next to **Boot diagnostics** is selected.</span></span> <span data-ttu-id="5b92e-212">Pokud provedete změny, klikněte na tlačítko **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="5b92e-212">If you make any changes, click **Save**:</span></span>

![Aktualizovat nastavení diagnostiky spouštění](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="5b92e-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b92e-214">Next steps</span></span>
<span data-ttu-id="5b92e-215">Pokud máte problémy s připojením k virtuálnímu počítači, přečtěte si téma [řešení SSH připojení k virtuální počítač Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5b92e-215">If you are having issues connecting to your VM, see [Troubleshoot SSH connections to an Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="5b92e-216">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5b92e-216">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="5b92e-217">Další informace o používání správce prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5b92e-217">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
