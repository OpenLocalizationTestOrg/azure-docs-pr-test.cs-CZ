---
title: "Použití Windows řešení potíží s virtuálního počítače na portálu Azure | Microsoft Docs"
description: "Zjistěte, jak k řešení potíží virtuální počítač Windows v Azure připojením disk operačního systému k obnovení virtuálního počítače pomocí portálu Azure"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 2c1524949931d69d7553d284bb92c550a61c521a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a><span data-ttu-id="7a0d4-103">Řešení potíží s virtuální počítač s Windows pomocí disk operačního systému se připojuje k obnovení virtuálního počítače pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7a0d4-103">Troubleshoot a Windows VM by attaching the OS disk to a recovery VM using the Azure portal</span></span>
<span data-ttu-id="7a0d4-104">Pokud Windows virtuálního počítače (VM) v prostředí Azure dojde k chybě spouštěcí nebo disk, musíte provést na virtuálním pevném disku, sám sebe pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-104">If your Windows virtual machine (VM) in Azure encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="7a0d4-105">Běžným příkladem bude aplikaci, která selhala aktualizace, která brání virtuálního počítače nebudou moct úspěšně spustil.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-105">A common example would be a failed application update that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="7a0d4-106">Tento článek popisuje, jak připojit virtuální pevný disk na jiný virtuální počítač opravte případné chyby a pak znovu vytvořte původní virtuální počítač s Windows pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-106">This article details how to use the Azure portal to connect your virtual hard disk to another Windows VM to fix any errors, then re-create your original VM.</span></span>

## <a name="recovery-process-overview"></a><span data-ttu-id="7a0d4-107">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="7a0d4-107">Recovery process overview</span></span>
<span data-ttu-id="7a0d4-108">Proces řešení potíží je následující:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-108">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="7a0d4-109">Odstraňte virtuální počítač, na zjištění problémy, zachovat virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-109">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="7a0d4-110">Připojte a připojit virtuální pevný disk na jiný virtuální počítač s Windows pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-110">Attach and mount the virtual hard disk to another Windows VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="7a0d4-111">Připojení k virtuálnímu počítači pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-111">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="7a0d4-112">Úpravy souborů nebo spustit žádné nástroje na opravte problémy v původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-112">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="7a0d4-113">Odpojení virtuálního pevného disku od virtuálního počítače pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-113">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="7a0d4-114">Vytvoření virtuálního počítače pomocí původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-114">Create a VM using the original virtual hard disk.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="7a0d4-115">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="7a0d4-115">Determine boot issues</span></span>
<span data-ttu-id="7a0d4-116">Pokud chcete zjistit, proč váš virtuální počítač není možné správně spustit, zkontrolujte Diagnostika spouštění snímek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-116">To determine why your VM is not able to boot correctly, examine the boot diagnostics VM screenshot.</span></span> <span data-ttu-id="7a0d4-117">Běžným příkladem by být aktualizaci selhání aplikace nebo virtuální pevný disk, na kterém se odstranil nebo přesunul.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-117">A common example would be a failed application update, or an underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="7a0d4-118">Vyberte virtuální počítač na portálu a poté přejděte dolů k **podporu + Poradce při potížích s** části.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-118">Select your VM in the portal and then scroll down to the **Support + Troubleshooting** section.</span></span> <span data-ttu-id="7a0d4-119">Klikněte na tlačítko **spouštění diagnostiky** zobrazíte na snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-119">Click **Boot diagnostics** to view the screenshot.</span></span> <span data-ttu-id="7a0d4-120">Poznámka: všechny specifické chybové zprávy nebo kódy chyb, které vám pomohou určit, proč je virtuální počítač zjištění problému.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-120">Note any specific error messages or error codes to help determine why the VM is encountering an issue.</span></span> <span data-ttu-id="7a0d4-121">Následující příklad ukazuje čekání na zastavení služeb virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-121">The following example shows a VM waiting on stopping services:</span></span>

![Zobrazení virtuálních počítačů Diagnostika spouštění protokoly konzoly](./media/troubleshoot-recovery-disks-portal/screenshot-error.png)

<span data-ttu-id="7a0d4-123">Můžete také kliknout na **– snímek obrazovky** ke stažení zachycení snímek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-123">You can also click **Screenshot** to download a capture of the VM screenshot.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="7a0d4-124">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="7a0d4-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="7a0d4-125">Než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk, musíte určit název virtuálního pevného disku (VHD).</span><span class="sxs-lookup"><span data-stu-id="7a0d4-125">Before you can attach your virtual hard disk to another VM, you need to identify the name of the virtual hard disk (VHD).</span></span> 

<span data-ttu-id="7a0d4-126">Vyberte skupinu prostředků z portálu a potom vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-126">Select your resource group from the portal, then select your storage account.</span></span> <span data-ttu-id="7a0d4-127">Klikněte na tlačítko **objekty BLOB**, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-127">Click **Blobs**, as in the following example:</span></span>

![Vyberte úložiště objektů BLOB](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

<span data-ttu-id="7a0d4-129">Obvykle mají kontejner s názvem **virtuální pevné disky** , ukládá virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-129">Typically you have a container named **vhds** that stores your virtual hard disks.</span></span> <span data-ttu-id="7a0d4-130">Vyberte kontejner zobrazení seznamu virtuálních pevných disků.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-130">Select the container to view a list of virtual hard disks.</span></span> <span data-ttu-id="7a0d4-131">Poznamenejte si název vašeho virtuálního pevného disku (předpona, která je obvykle název vašeho virtuálního počítače):</span><span class="sxs-lookup"><span data-stu-id="7a0d4-131">Note the name of your VHD (the prefix is usually the name of your VM):</span></span>

![Identifikovat virtuální pevný disk v kontejneru úložiště](./media/troubleshoot-recovery-disks-portal/storage-container.png)

<span data-ttu-id="7a0d4-133">Vyberte ze seznamu existující virtuální pevný disk a zkopírujte adresu URL pro použití v následujících krocích:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-133">Select your existing virtual hard disk from the list and copy the URL for use in the following steps:</span></span>

![Zkopírujte adresu URL existující virtuální pevný disk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a><span data-ttu-id="7a0d4-135">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7a0d4-135">Delete existing VM</span></span>
<span data-ttu-id="7a0d4-136">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-136">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="7a0d4-137">Virtuální pevný disk je, kde jsou uloženy samotného operačního systému, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-137">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="7a0d4-138">Virtuální počítač je jenom metadata, která definuje velikosti či umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="7a0d4-138">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="7a0d4-139">Každý virtuální pevný disk má zapůjčení přiřazen při připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-139">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="7a0d4-140">Přestože datové disky je možné připojovat a odpojovat dokonce i za běhu virtuálního počítače, disk s operačním systémem není možné odpojit, dokud se neodstraní prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-140">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="7a0d4-141">Zapůjčení i nadále i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated přidružení disk operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-141">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="7a0d4-142">Prvním krokem k obnovení virtuálního počítače je odstranit samotné prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-142">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="7a0d4-143">Když odstraníte virtuální počítač, virtuální pevné disky zůstanou ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-143">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="7a0d4-144">Po odstranění virtuálního počítače připojit virtuální pevný disk k jiným virtuálním Počítačem vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-144">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="7a0d4-145">Vyberte virtuální počítač na portálu a pak klikněte na tlačítko **odstranit**:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-145">Select your VM in the portal, then click **Delete**:</span></span>

![Virtuální počítač spouštěcí diagnostiky snímek obrazovky zobrazující chyby spouštěcí](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

<span data-ttu-id="7a0d4-147">Počkejte, dokud je virtuální počítač dokončí odstraňování před připojit virtuální pevný disk k jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-147">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="7a0d4-148">Zapůjčení na virtuální pevný disk, který přidruží k němu virtuální počítač je nutné uvolnit předtím, než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-148">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="7a0d4-149">Připojit existující virtuální pevný disk k jiným virtuálním Počítačem</span><span class="sxs-lookup"><span data-stu-id="7a0d4-149">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="7a0d4-150">Pro několika dalších krocích použijete jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-150">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="7a0d4-151">Existující virtuální pevný disk se připojit k řešení potíží VM být schopni prohlížet a upravovat obsah na disk.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-151">You attach the existing virtual hard disk to this troubleshooting VM to be able to browse and edit the disk's content.</span></span> <span data-ttu-id="7a0d4-152">Tento proces umožňuje opravte všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů protokolu, např.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-152">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="7a0d4-153">Vyberte nebo vytvořte jiným virtuálním Počítačem používat pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-153">Choose or create another VM to use for troubleshooting purposes.</span></span>

1. <span data-ttu-id="7a0d4-154">Vyberte skupinu prostředků z portálu a potom vyberte řešení potíží virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-154">Select your resource group from the portal, then select your troubleshooting VM.</span></span> <span data-ttu-id="7a0d4-155">Vyberte **disky** a pak klikněte na **připojit existující**:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-155">Select **Disks** and then click **Attach existing**:</span></span>

    ![Připojit stávající disk na portálu](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. <span data-ttu-id="7a0d4-157">Klikněte na **Soubor VHD** a vyberte váš existující virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-157">To select your existing virtual hard disk, click **VHD File**:</span></span>

    ![Vyhledání existujícího VHD procházením](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. <span data-ttu-id="7a0d4-159">Vyberte účet úložiště a kontejneru a pak klikněte na existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-159">Select your storage account and container, then click your existing VHD.</span></span> <span data-ttu-id="7a0d4-160">Klikněte **vyberte** tlačítko potvrďte svou volbu:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-160">Click the **Select** button to confirm your choice:</span></span>

    ![Výběr existujícího VHD](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. <span data-ttu-id="7a0d4-162">S svůj disk VHD nyní vybraný, klikněte na tlačítko **OK** připojit existující virtuální pevný disk:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-162">With your VHD now selected, click **OK** to attach the existing virtual hard disk:</span></span>

    ![Zkontrolujte připojení existujícího virtuálního pevného disku](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. <span data-ttu-id="7a0d4-164">Za několik sekund **disky** podokno pro virtuální počítač obsahuje existující virtuální pevný disk připojený jako datový disk:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-164">After a few seconds, the **Disks** pane for your VM lists your existing virtual hard disk connected as a data disk:</span></span>

    ![Existující virtuální pevný disk připojený jako datový disk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="7a0d4-166">Připojit disk připojená data</span><span class="sxs-lookup"><span data-stu-id="7a0d4-166">Mount the attached data disk</span></span>

1. <span data-ttu-id="7a0d4-167">Otevřete připojení vzdálené plochy k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-167">Open a Remote Desktop connection to your VM.</span></span> <span data-ttu-id="7a0d4-168">Vyberte virtuální počítač v portálu a klikněte na **Connect**.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-168">Select your VM in the portal and click **Connect**.</span></span> <span data-ttu-id="7a0d4-169">Stáhněte a otevřete soubor RDP připojení.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-169">Download and open the RDP connection file.</span></span> <span data-ttu-id="7a0d4-170">Zadejte své přihlašovací údaje pro přihlášení k virtuálnímu počítači následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-170">Enter your credentials to log in to your VM as follows:</span></span>

    ![Přihlaste se k virtuálnímu počítači pomocí vzdálené plochy](./media/troubleshoot-recovery-disks-portal/open-remote-desktop.png)

2. <span data-ttu-id="7a0d4-172">Otevřete **správce serveru**, pak vyberte **Souborová služba a služba úložiště**.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-172">Open **Server Manager**, then select **File and Storage Services**.</span></span> 

    ![Vyberte Souborová služba a služba úložiště ve Správci serveru](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

3. <span data-ttu-id="7a0d4-174">Datový disk je automaticky zjistil a připojené.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-174">The data disk is automatically detected and attached.</span></span> <span data-ttu-id="7a0d4-175">Pokud chcete zobrazit seznam připojených disků, vyberte **disky**.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-175">To see a list of the connected disks, select **Disks**.</span></span> <span data-ttu-id="7a0d4-176">Můžete vybrat datový disk k zobrazení informací o svazku, včetně písmeno jednotky.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-176">You can select your data disk to view volume information, including the drive letter.</span></span> <span data-ttu-id="7a0d4-177">Následující příklad ukazuje datový disk připojený a pomocí **F:**:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-177">The following example shows the data disk attached and using **F:**:</span></span>

    ![Disk připojený a informace o svazku ve Správci serveru](./media/troubleshoot-recovery-disks-portal/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="7a0d4-179">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="7a0d4-179">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="7a0d4-180">S existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-180">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="7a0d4-181">Jakmile vyřešíte problémy, pokračujte následujícími kroky.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-181">Once you have addressed the issues, continue with the following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="7a0d4-182">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="7a0d4-182">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="7a0d4-183">Jakmile jsou vaše chyby vyřešeny, odpojte existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-183">Once your errors are resolved, detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="7a0d4-184">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání zapůjčení virtuální pevný disk se připojuje k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-184">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="7a0d4-185">Z relace protokolu RDP pro virtuální počítač, otevřete **správce serveru**, pak vyberte **Souborová služba a služba úložiště**:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-185">From the RDP session to your VM, open **Server Manager**, then select **File and Storage Services**:</span></span>

    ![Vyberte Souborová služba a služba úložiště ve Správci serveru](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

2. <span data-ttu-id="7a0d4-187">Vyberte **disky** a pak vyberte datový disk.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-187">Select **Disks** and then select your data disk.</span></span> <span data-ttu-id="7a0d4-188">Klikněte pravým tlačítkem na datový disk a vyberte **převést do offline režimu**:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-188">Right-click on your data disk and select **Take Offline**:</span></span>

    ![Nastavte datový disk jako offline ve Správci serveru](./media/troubleshoot-recovery-disks-portal/server-manager-set-disk-offline.png)

3. <span data-ttu-id="7a0d4-190">Nyní Odpojte virtuální pevný disk z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-190">Now detach the virtual hard disk from the VM.</span></span> <span data-ttu-id="7a0d4-191">Vyberte virtuální počítač na portálu Azure a klikněte na **disky**.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-191">Select your VM in the Azure portal and click **Disks**.</span></span> <span data-ttu-id="7a0d4-192">Vyberte existující virtuální pevný disk a potom klikněte na **odpojení**:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-192">Select your existing virtual hard disk and then click **Detach**:</span></span>

    ![Odpojte existující virtuální pevný disk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    <span data-ttu-id="7a0d4-194">Počkejte, dokud virtuálního počítače úspěšně odpojil datový disk než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-194">Wait until the VM has successfully detached the data disk before continuing.</span></span>

## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="7a0d4-195">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="7a0d4-195">Create VM from original hard disk</span></span>
<span data-ttu-id="7a0d4-196">Chcete-li vytvořit virtuální počítač z původní virtuální pevný disk, použijte [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="7a0d4-196">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="7a0d4-197">Šablona nasadí virtuální počítač do existující virtuální síť pomocí adresy URL virtuálního pevného disku z dřívějších příkazu.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-197">The template deploys a VM into an existing virtual network, using the VHD URL from the earlier command.</span></span> <span data-ttu-id="7a0d4-198">Klikněte **nasadit do Azure** tlačítko následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-198">Click the **Deploy to Azure** button as follows:</span></span>

![Nasazení virtuálních počítačů z šablony z Githubu](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

<span data-ttu-id="7a0d4-200">Šablona je načten do portálu Azure pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-200">The template is loaded into the Azure portal for deployment.</span></span> <span data-ttu-id="7a0d4-201">Zadejte názvy pro nový virtuální počítač a existující prostředky Azure a vložte adresu URL na existující virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-201">Enter the names for your new VM and existing Azure resources, and paste the URL to your existing virtual hard disk.</span></span> <span data-ttu-id="7a0d4-202">Chcete-li zahájit nasazení, klikněte na tlačítko **nákupu**:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-202">To begin the deployment, click **Purchase**:</span></span>

![Nasazení virtuálního počítače ze šablony](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="7a0d4-204">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="7a0d4-204">Re-enable boot diagnostics</span></span>
<span data-ttu-id="7a0d4-205">Při vytváření virtuálního počítače z existujícího virtuálního pevného disku, nemusí být Diagnostika spouštění automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-205">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="7a0d4-206">Zkontrolujte stav Diagnostika spouštění a v případě potřeby zapnout, vyberte virtuální počítač na portálu.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-206">To check the status of boot diagnostics and turn on if needed, select your VM in the portal.</span></span> <span data-ttu-id="7a0d4-207">V části **monitorování**, klikněte na tlačítko **nastavení diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-207">Under **Monitoring**, click **Diagnostics settings**.</span></span> <span data-ttu-id="7a0d4-208">Ujistěte se, že stav je **na**a zaškrtnutí políčka vedle **spouštění diagnostiky** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="7a0d4-208">Ensure the status is **On**, and the check mark next to **Boot diagnostics** is selected.</span></span> <span data-ttu-id="7a0d4-209">Pokud provedete změny, klikněte na tlačítko **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="7a0d4-209">If you make any changes, click **Save**:</span></span>

![Aktualizovat nastavení diagnostiky spouštění](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="7a0d4-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a0d4-211">Next steps</span></span>
<span data-ttu-id="7a0d4-212">Pokud máte problémy s připojením k virtuálnímu počítači, přečtěte si téma [připojení řešení potíží s RDP na virtuální počítač Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7a0d4-212">If you are having issues connecting to your VM, see [Troubleshoot RDP connections to an Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="7a0d4-213">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuálním počítači Windows](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7a0d4-213">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="7a0d4-214">Další informace o používání správce prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7a0d4-214">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>