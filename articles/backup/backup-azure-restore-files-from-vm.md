---
title: "Zálohování Azure: Obnovovat soubory a složky ze zálohy virtuálního počítače Azure | Microsoft Docs"
description: "Obnovit soubory z bodu obnovení virtuálního počítače Azure"
services: backup
documentationcenter: dev-center-name
author: pvrk
manager: shivamg
keywords: "obnovení na úrovni položek; obnovení souborů ze zálohy virtuálního počítače Azure; Obnovit soubory z virtuálního počítače Azure"
ms.assetid: f1c067a2-4826-4da4-b97a-c5fd6c189a77
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/20/2017
ms.author: pullabhk;markgal
ms.openlocfilehash: ae7c345c11a7db25413d60ad822f16f84ca37362
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a><span data-ttu-id="ade1d-104">Obnovit soubory ze zálohy virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="ade1d-104">Recover files from Azure virtual machine backup</span></span>

<span data-ttu-id="ade1d-105">Azure backup poskytuje možnosti obnovení [disky a virtuální počítače Azure](./backup-azure-arm-restore-vms.md) ze záloh virtuálních počítačů Azure.</span><span class="sxs-lookup"><span data-stu-id="ade1d-105">Azure backup provides the capability to restore [Azure VMs and disks](./backup-azure-arm-restore-vms.md) from Azure VM backups.</span></span> <span data-ttu-id="ade1d-106">Teď tento článek vysvětluje, jak můžete položek, jako jsou soubory a složky obnovit ze zálohy virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="ade1d-106">Now this article explains how you can recover items such as files and folders from an Azure VM backup.</span></span>

> [!Note]
> <span data-ttu-id="ade1d-107">Tato funkce je k dispozici pro virtuální počítače Azure nasazení pomocí modelu Resource Manager a chránit do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-107">This feature is available for Azure VMs deployed using the Resource Manager model and protected to a Recovery services vault.</span></span>
> <span data-ttu-id="ade1d-108">Obnovení souborů ze šifrovaných zálohy virtuálního počítače není podporováno.</span><span class="sxs-lookup"><span data-stu-id="ade1d-108">File recovery from an encrypted VM backup is not supported.</span></span>
>

## <a name="mount-the-volume-and-copy-files"></a><span data-ttu-id="ade1d-109">Připojte svazek a zkopírujte soubory</span><span class="sxs-lookup"><span data-stu-id="ade1d-109">Mount the volume and copy files</span></span>

1. <span data-ttu-id="ade1d-110">Přihlaste se k webu [Azure Portal](http://portal.Azure.com).</span><span class="sxs-lookup"><span data-stu-id="ade1d-110">Sign into the [Azure portal](http://portal.Azure.com).</span></span> <span data-ttu-id="ade1d-111">Najděte příslušné trezoru služeb zotavení a bude požadovaná položka zálohování.</span><span class="sxs-lookup"><span data-stu-id="ade1d-111">Find the relevant Recovery services vault and the required backup item.</span></span>

2. <span data-ttu-id="ade1d-112">V okně Zálohování položky, klikněte na **obnovení souborů**</span><span class="sxs-lookup"><span data-stu-id="ade1d-112">On the Backup Item blade, click **File Recovery**</span></span>

    ![Otevřete položky zálohování trezoru služeb zotavení](./media/backup-azure-restore-files-from-vm/open-vault-item.png)

    <span data-ttu-id="ade1d-114">**Obnovení souboru** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="ade1d-114">The **File Recovery** blade opens.</span></span>

    ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

3. <span data-ttu-id="ade1d-116">Z **vyberte bod obnovení** rozevírací nabídky vyberte bod obnovení, který obsahuje soubory, které chcete.</span><span class="sxs-lookup"><span data-stu-id="ade1d-116">From the **Select recovery point** drop-down menu, select the recovery point that contains the files you want.</span></span> <span data-ttu-id="ade1d-117">Ve výchozím nastavení je již vybrán poslední bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-117">By default, the latest recovery point is already selected.</span></span>

4. <span data-ttu-id="ade1d-118">Klikněte na tlačítko **spustitelný soubor stáhnout** (pro virtuální počítač Windows Azure) nebo **stáhnout skript** (pro virtuální počítač Azure s Linuxem) ke stažení softwaru, které budete používat pro kopírování souborů z bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-118">Click **Download Executable** (for Windows Azure VM) or **Download Script** (for Linux Azure VM) to download the software that you'll use to copy files from the recovery point.</span></span>

  <span data-ttu-id="ade1d-119">Spustitelný soubor nebo skript vytvoří připojení mezi místní počítač a pro zadaný bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-119">The executable/script creates a connection between the local computer and the specified recovery point.</span></span>

5. <span data-ttu-id="ade1d-120">Je třeba heslo ke spuštění staženého skriptu nebo spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="ade1d-120">You need a password to run the downloaded script/executable.</span></span> <span data-ttu-id="ade1d-121">Heslo můžete zkopírovat z portálu pomocí tlačítko Kopírovat, pokud vedle vygenerované heslo</span><span class="sxs-lookup"><span data-stu-id="ade1d-121">You can copy the password from the portal using the copy button beside the generated password</span></span>

    ![Vygenerované heslo](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

6. <span data-ttu-id="ade1d-123">Na počítači, ve které chcete obnovit soubory spusťte skript nebo spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="ade1d-123">On the computer where you want to recover the files, run the executable/script.</span></span> <span data-ttu-id="ade1d-124">Je nutné jej spustit pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="ade1d-124">You must run it with Administrator credentials.</span></span> <span data-ttu-id="ade1d-125">Pokud spustíte skript na počítači s omezeným přístupem, ujistěte se, je přístup k:</span><span class="sxs-lookup"><span data-stu-id="ade1d-125">If you run the script on a computer with restricted access, ensure there is access to:</span></span>

    - <span data-ttu-id="ade1d-126">download.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="ade1d-126">download.microsoft.com</span></span>
    - <span data-ttu-id="ade1d-127">Koncové body Azure použít pro zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="ade1d-127">Azure endpoints used for Azure VM backups</span></span>
    - <span data-ttu-id="ade1d-128">odchozí port 3260</span><span class="sxs-lookup"><span data-stu-id="ade1d-128">outbound port 3260</span></span>

   <span data-ttu-id="ade1d-129">Pro systémy Linux vyžaduje skript 'open-iscsi' a 'lshw' součásti pro připojení k bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-129">For Linux, the script requires 'open-iscsi' and 'lshw' components to connect to the recovery point.</span></span> <span data-ttu-id="ade1d-130">Pokud těch, které nejsou k dispozici na počítači, na kterém je spuštěná, požádá o oprávnění k instalaci příslušné komponenty a nainstaluje je po souhlasu.</span><span class="sxs-lookup"><span data-stu-id="ade1d-130">If those do not exist on the machine where it is run, it asks for permission to install the relevant components and installs them upon consent.</span></span>
   
   <span data-ttu-id="ade1d-131">Zadejte heslo z portálu po zobrazení výzvy zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="ade1d-131">Enter the password copied from the portal when prompted.</span></span> <span data-ttu-id="ade1d-132">Jakmile je zadáno platné heslo skripty se spojí s bodem obnovení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-132">Once the valid password is entered the scripts connects to the recovery point.</span></span>
      
    ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/executable-output.png)
    
   
   <span data-ttu-id="ade1d-134">Tento skript můžete spustit na jakýkoli počítač, který má operační systém stejného (nebo kompatibilní) jako zálohované virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ade1d-134">You can run the script on any machine that has the same (or compatible) operating system as the backed-up VM.</span></span> <span data-ttu-id="ade1d-135">Najdete v článku [kompatibilní operační systém tabulky](backup-azure-restore-files-from-vm.md#compatible-os) pro kompatibilní operační systémy.</span><span class="sxs-lookup"><span data-stu-id="ade1d-135">See the [Compatible OS table](backup-azure-restore-files-from-vm.md#compatible-os) for compatible operating systems.</span></span> <span data-ttu-id="ade1d-136">Pokud chráněný virtuální počítač Azure používá prostory úložiště ve Windows (pro virtuální počítače Windows Azure) nebo Arrays(for Linux VMs) LVM/RAID, nebudete moci na stejném virtuálním počítači spustit spustitelný soubor nebo skript.</span><span class="sxs-lookup"><span data-stu-id="ade1d-136">If the protected Azure virtual machine uses Windows Storage Spaces (for Windows Azure VMs) or LVM/RAID Arrays(for Linux VMs), then you can't run the executable/script on the same virtual machine.</span></span> <span data-ttu-id="ade1d-137">Místo toho jej spusťte v jiným počítačem s kompatibilní operační systém.</span><span class="sxs-lookup"><span data-stu-id="ade1d-137">Instead, run it on any other machine with a compatible operating system.</span></span>

### <a name="compatible-os"></a><span data-ttu-id="ade1d-138">Kompatibilní operační systém</span><span class="sxs-lookup"><span data-stu-id="ade1d-138">Compatible OS</span></span>

#### <a name="for-windows"></a><span data-ttu-id="ade1d-139">Pro Windows</span><span class="sxs-lookup"><span data-stu-id="ade1d-139">For Windows</span></span>

<span data-ttu-id="ade1d-140">V následující tabulce jsou uvedeny kompatibilitu mezi serverem a počítačem operační systémy.</span><span class="sxs-lookup"><span data-stu-id="ade1d-140">The following table shows the compatibility between server and computer operating systems.</span></span> <span data-ttu-id="ade1d-141">Při obnovování souborů, nelze obnovit soubory mezi operační systémy nekompatibilní.</span><span class="sxs-lookup"><span data-stu-id="ade1d-141">When recovering files, you can't restore files between incompatible operating systems.</span></span>

|<span data-ttu-id="ade1d-142">OS serveru</span><span class="sxs-lookup"><span data-stu-id="ade1d-142">Server OS</span></span> | <span data-ttu-id="ade1d-143">Kompatibilní klientského operačního systému</span><span class="sxs-lookup"><span data-stu-id="ade1d-143">Compatible client OS</span></span>  |
| --------------- | ---- |
| <span data-ttu-id="ade1d-144">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="ade1d-144">Windows Server 2012 R2</span></span> | <span data-ttu-id="ade1d-145">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="ade1d-145">Windows 8.1</span></span> |
| <span data-ttu-id="ade1d-146">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="ade1d-146">Windows Server 2012</span></span>    | <span data-ttu-id="ade1d-147">Windows 8</span><span class="sxs-lookup"><span data-stu-id="ade1d-147">Windows 8</span></span>  |
| <span data-ttu-id="ade1d-148">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="ade1d-148">Windows Server 2008 R2</span></span> | <span data-ttu-id="ade1d-149">Windows 7</span><span class="sxs-lookup"><span data-stu-id="ade1d-149">Windows 7</span></span>   |

#### <a name="for-linux"></a><span data-ttu-id="ade1d-150">Pro Linux</span><span class="sxs-lookup"><span data-stu-id="ade1d-150">For Linux</span></span>

<span data-ttu-id="ade1d-151">V systému Linux základní požadavek je, že by měl operačního systému počítače, kde se skript spouští podporují systém souborů v virtuálního počítače s Linuxem zálohované soubory.</span><span class="sxs-lookup"><span data-stu-id="ade1d-151">In Linux, the fundamental requirement is that the OS of the machine where the script is run should support the filesystem of the files present in the backed-up Linux VM.</span></span> <span data-ttu-id="ade1d-152">Při výběru počítače ke spuštění skriptu, ujistěte se, že má kompatibilní operační systém a verze, jak je uvedeno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="ade1d-152">While selecting a machine to run the script, please ensure it has the compatible OS and the versions as mentioned in the table below.</span></span>

|<span data-ttu-id="ade1d-153">Linux operačního systému</span><span class="sxs-lookup"><span data-stu-id="ade1d-153">Linux OS</span></span> | <span data-ttu-id="ade1d-154">Verze</span><span class="sxs-lookup"><span data-stu-id="ade1d-154">Versions</span></span>  |
| --------------- | ---- |
| <span data-ttu-id="ade1d-155">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="ade1d-155">Ubuntu</span></span> | <span data-ttu-id="ade1d-156">12.04 a vyšší</span><span class="sxs-lookup"><span data-stu-id="ade1d-156">12.04 and above</span></span> |
| <span data-ttu-id="ade1d-157">CentOS</span><span class="sxs-lookup"><span data-stu-id="ade1d-157">CentOS</span></span> | <span data-ttu-id="ade1d-158">verze 6.5 a vyšší</span><span class="sxs-lookup"><span data-stu-id="ade1d-158">6.5 and above</span></span>  |
| <span data-ttu-id="ade1d-159">RHEL</span><span class="sxs-lookup"><span data-stu-id="ade1d-159">RHEL</span></span> | <span data-ttu-id="ade1d-160">6.7 a vyšší</span><span class="sxs-lookup"><span data-stu-id="ade1d-160">6.7 and above</span></span> |
| <span data-ttu-id="ade1d-161">Debian</span><span class="sxs-lookup"><span data-stu-id="ade1d-161">Debian</span></span> | <span data-ttu-id="ade1d-162">7 a vyšší</span><span class="sxs-lookup"><span data-stu-id="ade1d-162">7 and above</span></span> |
| <span data-ttu-id="ade1d-163">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="ade1d-163">Oracle Linux</span></span> | <span data-ttu-id="ade1d-164">6.4 a vyšší</span><span class="sxs-lookup"><span data-stu-id="ade1d-164">6.4 and above</span></span> |

<span data-ttu-id="ade1d-165">Skript také vyžaduje python a bash komponent ke spouštění a bezpečně připojit k bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-165">The script also requires python and bash components to execute and connect securely to the recovery point.</span></span>

|<span data-ttu-id="ade1d-166">Komponenta</span><span class="sxs-lookup"><span data-stu-id="ade1d-166">Component</span></span> | <span data-ttu-id="ade1d-167">Verze</span><span class="sxs-lookup"><span data-stu-id="ade1d-167">Version</span></span>  |
| --------------- | ---- |
| <span data-ttu-id="ade1d-168">Bash</span><span class="sxs-lookup"><span data-stu-id="ade1d-168">bash</span></span> | <span data-ttu-id="ade1d-169">4 a novější</span><span class="sxs-lookup"><span data-stu-id="ade1d-169">4 and above</span></span> |
| <span data-ttu-id="ade1d-170">python</span><span class="sxs-lookup"><span data-stu-id="ade1d-170">python</span></span> | <span data-ttu-id="ade1d-171">2.6.6 a vyšší</span><span class="sxs-lookup"><span data-stu-id="ade1d-171">2.6.6 and above</span></span>  |


### <a name="identifying-volumes"></a><span data-ttu-id="ade1d-172">Identifikace svazky</span><span class="sxs-lookup"><span data-stu-id="ade1d-172">Identifying Volumes</span></span>

#### <a name="for-windows"></a><span data-ttu-id="ade1d-173">Pro Windows</span><span class="sxs-lookup"><span data-stu-id="ade1d-173">For Windows</span></span>

<span data-ttu-id="ade1d-174">Při spuštění exectuable připojí nové svazky operačního systému a přiřadí písmena jednotek.</span><span class="sxs-lookup"><span data-stu-id="ade1d-174">When you run the exectuable, the operating system mounts the new volumes and assigns drive letters.</span></span> <span data-ttu-id="ade1d-175">Můžete procházet tyto jednotky Průzkumníka Windows nebo Průzkumníka souborů.</span><span class="sxs-lookup"><span data-stu-id="ade1d-175">You can use Windows Explorer or File Explorer to browse those drives.</span></span> <span data-ttu-id="ade1d-176">Písmena jednotek přiřazená k daným svazkům nesmí být stejná písmena jako původní virtuální počítač, ale je zachováno název svazku.</span><span class="sxs-lookup"><span data-stu-id="ade1d-176">The drive letters assigned to the volumes may not be the same letters as the original virtual machine, however, the volume name is preserved.</span></span> <span data-ttu-id="ade1d-177">Například, pokud byl svazek v původní virtuální počítač "datový Disk (E:\)", že svazek může být připojené jako "datový Disk ("Kterékoli písmeno jednotky k dispozici":\) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="ade1d-177">For example, if the volume on the original virtual machine was “Data Disk (E:\)”, that volume can be attached as “Data Disk ('Any drive letter available':\) on the local computer.</span></span> <span data-ttu-id="ade1d-178">Procházejte všechny svazky uvedeno ve výstupu skriptu vyhledejte soubory nebo složku.</span><span class="sxs-lookup"><span data-stu-id="ade1d-178">Browse through all volumes mentioned in the script output until you find your files/folder.</span></span>  
       
   ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/volumes-attached.png)
           
#### <a name="for-linux"></a><span data-ttu-id="ade1d-180">Pro Linux</span><span class="sxs-lookup"><span data-stu-id="ade1d-180">For Linux</span></span>

<span data-ttu-id="ade1d-181">V systému Linux svazky bodu obnovení jsou připojené do složky, kde je skript spuštěn.</span><span class="sxs-lookup"><span data-stu-id="ade1d-181">In Linux, the volumes of the recovery point are mounted to the folder where the script is run.</span></span> <span data-ttu-id="ade1d-182">Připojené disky, svazky a odpovídající cesty připojení se zobrazí odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="ade1d-182">The attached disks, volumes and the corresponding mount paths are shown accordingly.</span></span> <span data-ttu-id="ade1d-183">Tyto cesty připojení jsou viditelné pro uživatele s kořenové úrovně přístupu.</span><span class="sxs-lookup"><span data-stu-id="ade1d-183">These mount paths are visible to users having root level access.</span></span> <span data-ttu-id="ade1d-184">Procházejte svazky uvedeno ve výstupu skriptu.</span><span class="sxs-lookup"><span data-stu-id="ade1d-184">Browse through the volumes mentioned in the script output.</span></span>

  ![Okno obnovení souboru Linux](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  

## <a name="closing-the-connection"></a><span data-ttu-id="ade1d-186">Probíhá ukončování připojení</span><span class="sxs-lookup"><span data-stu-id="ade1d-186">Closing the connection</span></span>

<span data-ttu-id="ade1d-187">Po identifikování souborů a jejich kopírování do umístění místního úložiště, odeberte (nebo odpojit) další jednotky.</span><span class="sxs-lookup"><span data-stu-id="ade1d-187">After identifying the files and copying them to a local storage location, remove (or unmount) the additional drives.</span></span> <span data-ttu-id="ade1d-188">Odpojit jednotky, na **obnovení souboru** na portálu Azure klikněte na **odpojit disky**.</span><span class="sxs-lookup"><span data-stu-id="ade1d-188">To unmount the drives, on the **File Recovery** blade in the Azure portal, click **Unmount Disks**.</span></span>

![Odpojení disky](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

<span data-ttu-id="ade1d-190">Po disky se odpojily, zobrazí se, že byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="ade1d-190">Once the disks have been unmounted, you receive a message letting you know it was successful.</span></span> <span data-ttu-id="ade1d-191">To může trvat pár minut pro připojení k aktualizaci, které umožní odstranit disky.</span><span class="sxs-lookup"><span data-stu-id="ade1d-191">It may take a few minutes for the connection to refresh so that you can remove the disks.</span></span>

<span data-ttu-id="ade1d-192">V systému Linux po připojení k bodu obnovení je porušeno, operačního systému nemá odpovídající cesty připojení automaticky odebrat.</span><span class="sxs-lookup"><span data-stu-id="ade1d-192">In Linux, after the connection to the recovery point is severed, the OS doesn't remove the corresponding mount paths automatically.</span></span> <span data-ttu-id="ade1d-193">Tyto existují jako "oddělena" svazky a že jsou viditelné, ale vyvolá chybu, když je přístup a zápis souborů.</span><span class="sxs-lookup"><span data-stu-id="ade1d-193">These exist as "orphan" volumes and they are visible but throw an error when you access/write the files.</span></span> <span data-ttu-id="ade1d-194">Je lze ručně odebrat.</span><span class="sxs-lookup"><span data-stu-id="ade1d-194">They can be manually removed.</span></span> <span data-ttu-id="ade1d-195">Skript, při spuštění, identifikuje všechny tyto svazky existující z jakékoli předchozí body obnovení a je vyčistí po souhlasu.</span><span class="sxs-lookup"><span data-stu-id="ade1d-195">The script, when run, identifies any such volumes existing from any previous recovery points and cleans them up upon consent.</span></span>

## <a name="special-configurations"></a><span data-ttu-id="ade1d-196">Zvláštní konfigurace</span><span class="sxs-lookup"><span data-stu-id="ade1d-196">Special configurations</span></span>

### <a name="dynamic-disks"></a><span data-ttu-id="ade1d-197">Dynamické disky</span><span class="sxs-lookup"><span data-stu-id="ade1d-197">Dynamic Disks</span></span>

<span data-ttu-id="ade1d-198">Pokud virtuální počítač Azure, která byla zálohována má svazky, které jsou rozmístěny v několika disků (předané a prokládané svazky) nebo odolné proti chybám svazky (RAID-5 a zrcadlené) na dynamické disky, nelze spustit spustitelný soubor skriptu na stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ade1d-198">If the Azure VM that was backed up has volumes that span multiple disks (spanned and striped volumes) and/or fault-tolerant volumes (mirrored and RAID-5 volumes) on dynamic disks, then you can't run the executable script on the same VM.</span></span> <span data-ttu-id="ade1d-199">Místo toho spusťte spustitelný soubor skriptu z jakéhokoli počítače s kompatibilní operační systém.</span><span class="sxs-lookup"><span data-stu-id="ade1d-199">Instead, run the executable script on any other machine with a compatible operating system.</span></span>

### <a name="windows-storage-spaces"></a><span data-ttu-id="ade1d-200">Prostory úložiště ve Windows</span><span class="sxs-lookup"><span data-stu-id="ade1d-200">Windows Storage Spaces</span></span>

<span data-ttu-id="ade1d-201">Prostory úložiště ve Windows je technologie v úložišti systému Windows, která umožňuje Virtualizovat úložiště.</span><span class="sxs-lookup"><span data-stu-id="ade1d-201">Windows Storage Spaces is a technology in Windows storage that enables you to virtualize storage.</span></span> <span data-ttu-id="ade1d-202">Prostory úložiště ve Windows můžete seskupit standardní disky do fondů úložiště a pak vytvořit virtuálních disků, nazývaných prostory úložiště z dostupné místo v těchto fondů úložiště.</span><span class="sxs-lookup"><span data-stu-id="ade1d-202">With Windows Storage Spaces you can group industry-standard disks into storage pools, and then create virtual disks, called storage spaces, from the available space in those storage pools.</span></span>

<span data-ttu-id="ade1d-203">Pokud virtuální počítač Azure, která byla zálohována používá prostory úložiště ve Windows, nebudete moci na stejný virtuální počítač spustit spustitelný soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="ade1d-203">If the Azure VM that was backed up uses Windows Storage Spaces, then you can't run the executable script on the same VM.</span></span> <span data-ttu-id="ade1d-204">Místo toho spusťte spustitelný soubor skriptu z jakéhokoli počítače s kompatibilní operační systém.</span><span class="sxs-lookup"><span data-stu-id="ade1d-204">Instead, run the executable script on any other machine with a compatible operating system.</span></span>

### <a name="lvmraid-arrays"></a><span data-ttu-id="ade1d-205">Pole LVM/RAID</span><span class="sxs-lookup"><span data-stu-id="ade1d-205">LVM/RAID Arrays</span></span>

<span data-ttu-id="ade1d-206">V systému Linux Správce logických svazků (LVM) nebo softwaru pole RAID se používají ke správě logické svazky přes několik disků.</span><span class="sxs-lookup"><span data-stu-id="ade1d-206">In Linux, Logical volume manager (LVM) and/or software RAID Arrays are used to manage logical volumes over multiple disks.</span></span> <span data-ttu-id="ade1d-207">Pokud zálohovaných virtuálního počítače s Linuxem používá LVM nebo pole RAID, nelze spustit skript v stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ade1d-207">If the backed up Linux VM uses LVM and/or RAID Arrays, you can't run the script on the same VM.</span></span> <span data-ttu-id="ade1d-208">Místo toho spuštění skriptu na jiné počítače s operačním systémem kompatibilní a který podporuje filesystem zálohovaných virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ade1d-208">Instead run the script on any other machine with compatible OS and which supports filesystem of the backed up VM.</span></span>

<span data-ttu-id="ade1d-209">Výstup skriptu zobrazí disky LVM nebo pole RAID a svazky s typem oddílu, jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="ade1d-209">The script output displays the LVM and/or RAID Arrays disks and the volumes with the partition type as shown below</span></span>

   ![Okno výstup LVM Linux](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)
   
<span data-ttu-id="ade1d-211">Následující příkazy nutné spustit uživatel, aby tyto oddíly online.</span><span class="sxs-lookup"><span data-stu-id="ade1d-211">The following commands need to be run by the user to bring these partitions online.</span></span> 

<span data-ttu-id="ade1d-212">**Pro LVM oddíly**</span><span class="sxs-lookup"><span data-stu-id="ade1d-212">**For LVM Partitions**</span></span>

```
$ pvs <volume name as shown above in the script output> 
```
<span data-ttu-id="ade1d-213">Rutina Vypíše seznam názvů skupin svazku v části fyzický svazku.</span><span class="sxs-lookup"><span data-stu-id="ade1d-213">This lists the volume group names under a physical volume.</span></span>

```
$ lvdisplay <volume-group-name from the pvs command’s results> 
```
<span data-ttu-id="ade1d-214">Rutina Vypíše seznam všech logických svazků, názvy a jejich cesty ve skupině svazku.</span><span class="sxs-lookup"><span data-stu-id="ade1d-214">This lists all logical volumes, names and their paths in a volume group.</span></span>

```
$ mount <LV path> </mountpath>
```
<span data-ttu-id="ade1d-215">Chcete-li připojit logické svazky na cestu podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="ade1d-215">To mount the logical volumes to the path of your choice.</span></span>


<span data-ttu-id="ade1d-216">**Pro pole RAID**</span><span class="sxs-lookup"><span data-stu-id="ade1d-216">**For RAID Arrays**</span></span>

```
$ mdadm –detail –scan
```
<span data-ttu-id="ade1d-217">Zobrazí podrobnosti o všech disků raid.</span><span class="sxs-lookup"><span data-stu-id="ade1d-217">This displays details about all raid disks.</span></span> <span data-ttu-id="ade1d-218">Zobrazí se příslušné disku RAID jako`/dev/mdm/<RAID array name in the backed up VM>`</span><span class="sxs-lookup"><span data-stu-id="ade1d-218">The relevant RAID disk will be displayed as `/dev/mdm/<RAID array name in the backed up VM>`</span></span>

<span data-ttu-id="ade1d-219">Pokud RAID disk fyzických svazků, použijte příkaz připojení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-219">Use the mount command if the RAID disk has physical volumes.</span></span>
```
$ mount [RAID Disk Path] [/mounthpath]
```

<span data-ttu-id="ade1d-220">Pokud tento disk RAID má jiné LVM v nich konfigurovali potom postupujte stejným způsobem, jak je uvedeno výše pro LVM oddíly s názvem svazku probíhá na název disku diskového pole RAID</span><span class="sxs-lookup"><span data-stu-id="ade1d-220">If this RAID disk has another LVM configured in it then follow the same procedure as outlined above for LVM partitions with the volume name being the RAID Disk name</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ade1d-221">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ade1d-221">Troubleshooting</span></span>

<span data-ttu-id="ade1d-222">Pokud máte potíže při obnovování soubory z virtuálních počítačů, zkontrolujte následující tabulce najdete další informace.</span><span class="sxs-lookup"><span data-stu-id="ade1d-222">If you have problems while recovering files from the virtual machines, check the following table for additional information.</span></span>

| <span data-ttu-id="ade1d-223">Chybová zpráva nebo scénáře</span><span class="sxs-lookup"><span data-stu-id="ade1d-223">Error Message / Scenario</span></span> | <span data-ttu-id="ade1d-224">Pravděpodobná příčina</span><span class="sxs-lookup"><span data-stu-id="ade1d-224">Probable Cause</span></span> | <span data-ttu-id="ade1d-225">Doporučená akce</span><span class="sxs-lookup"><span data-stu-id="ade1d-225">Recommended action</span></span> |
| ------------------------ | -------------- | ------------------ |
| <span data-ttu-id="ade1d-226">Výstupní soubor EXE: *výjimka připojení k cíli*</span><span class="sxs-lookup"><span data-stu-id="ade1d-226">Exe output: *Exception connecting to the target*</span></span> |<span data-ttu-id="ade1d-227">Skript není možné získat přístup k bodu obnovení</span><span class="sxs-lookup"><span data-stu-id="ade1d-227">Script is not able to access the recovery point</span></span> | <span data-ttu-id="ade1d-228">Zkontrolujte, zda počítač splňuje všechny požadavky na přístup zmíněné</span><span class="sxs-lookup"><span data-stu-id="ade1d-228">Check whether the machine fulfills the access requirements mentioned above</span></span>|  
|   <span data-ttu-id="ade1d-229">Výstupní soubor EXE: *cíl již byla zaznamenána prostřednictvím relace ISCSI.*</span><span class="sxs-lookup"><span data-stu-id="ade1d-229">Exe output: *The target has already been logged in via an ISCSI session.*</span></span> | <span data-ttu-id="ade1d-230">Skript byl již spuštěn na stejném počítači a byly připojeny jednotky</span><span class="sxs-lookup"><span data-stu-id="ade1d-230">The script was already executed on the same machine and the drives have been attached</span></span> | <span data-ttu-id="ade1d-231">Již byly připojeny svazků bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-231">The volumes of the recovery point have already been attached.</span></span> <span data-ttu-id="ade1d-232">Může připojených není s stejná písmena jednotek původní virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ade1d-232">They may NOT be mounted with the same drive letters of the original VM.</span></span> <span data-ttu-id="ade1d-233">Procházet všechny svazky, které jsou k dispozici v Průzkumníku souborů souboru</span><span class="sxs-lookup"><span data-stu-id="ade1d-233">Browse through all the available volumes in the file explorer for your file</span></span> |
| <span data-ttu-id="ade1d-234">Výstupní soubor EXE: *tento skript je neplatný, protože disky mají byly odpojeny prostřednictvím 12-hr portál nebo překročil limit. Stáhněte si prosím nový skript z portálu.*</span><span class="sxs-lookup"><span data-stu-id="ade1d-234">Exe output: *This script is invalid because the disks have been dismounted via portal/exceeded the 12-hr limit. Please download a new script from the portal.*</span></span> |  <span data-ttu-id="ade1d-235">Disky mají byla odpojena z portálu nebo byl překročen limit 12 hr</span><span class="sxs-lookup"><span data-stu-id="ade1d-235">The disks have been dismounted from the portal or the 12-hr limit exceeded</span></span> |    <span data-ttu-id="ade1d-236">Tato konkrétní exe je neplatný a nelze spustit.</span><span class="sxs-lookup"><span data-stu-id="ade1d-236">This particular exe is now invalid and can’t be run.</span></span> <span data-ttu-id="ade1d-237">Pokud chcete přístup k souborům tohoto obnovení bodu v čase, přejděte na portálu pro nové exe</span><span class="sxs-lookup"><span data-stu-id="ade1d-237">If you want to access the files of that recovery point-in-time, visit the portal for a new exe</span></span>|
| <span data-ttu-id="ade1d-238">Na počítači, kde se spouští exe: nové svazky nejsou odpojeny po kliknutí na tlačítko odpojení</span><span class="sxs-lookup"><span data-stu-id="ade1d-238">On the machine where the exe is run: The new volumes are not dismounted after the dismount button is clicked</span></span> |    <span data-ttu-id="ade1d-239">Iniciátor ISCSI na počítači není reagovat nebo obnovení připojení k cíli a údržbu mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ade1d-239">The ISCSI initiator on the machine is not responding/refreshing its connection to the target and maintaining the cache</span></span> |    <span data-ttu-id="ade1d-240">Počkejte několik minut, po stisknutí tlačítka odpojení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-240">Wait for some mins after the dismount button is pressed.</span></span> <span data-ttu-id="ade1d-241">Pokud ještě nejsou odpojeny nové svazky, přejděte prosím prostřednictvím všechny svazky.</span><span class="sxs-lookup"><span data-stu-id="ade1d-241">If the new volumes are still not dismounted, please browse through all the volumes.</span></span> <span data-ttu-id="ade1d-242">Vynutí se tak iniciátoru aktualizovat připojení a svazek je odpojený s chybovou zprávou, disk není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ade1d-242">This forces the initiator to refresh the connection and the volume is dismounted with an error message that the disk is not available</span></span>|
| <span data-ttu-id="ade1d-243">Výstupní soubor EXE: skript je spuštěn úspěšně, ale "Nové svazky připojené" se nezobrazí na výstup skriptu</span><span class="sxs-lookup"><span data-stu-id="ade1d-243">Exe output: Script is run successfully but “New volumes attached” is not displayed on the script output</span></span> | <span data-ttu-id="ade1d-244">Toto je přechodné chybě.</span><span class="sxs-lookup"><span data-stu-id="ade1d-244">This is a transient error</span></span>   | <span data-ttu-id="ade1d-245">Svazky by byly již připojeny.</span><span class="sxs-lookup"><span data-stu-id="ade1d-245">The volumes would have been already attached.</span></span> <span data-ttu-id="ade1d-246">Otevřete Průzkumníka a přejděte.</span><span class="sxs-lookup"><span data-stu-id="ade1d-246">Open Explorer to browse.</span></span> <span data-ttu-id="ade1d-247">Pokud ke spouštění skriptů pokaždé, když používáte stejný počítač, zvažte, restartování počítače a v seznamu má být zobrazena v běží následné exe.</span><span class="sxs-lookup"><span data-stu-id="ade1d-247">If you are using the same machine for running scripts every time, consider restarting the machine and the list should be displayed in the subsequent exe runs.</span></span> |
| <span data-ttu-id="ade1d-248">Konkrétní Linux: Nepodařilo se zobrazit požadované svazky</span><span class="sxs-lookup"><span data-stu-id="ade1d-248">Linux specific: Not able to view the desired volumes</span></span> | <span data-ttu-id="ade1d-249">Operačního systému počítače, kde se skript spouští nemusí rozpoznat základní systém souborů zálohovaných virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ade1d-249">The OS of the machine where the script is run may not recognize the underlying filesystem of the backed up VM</span></span> | <span data-ttu-id="ade1d-250">Zkontrolujte, zda bod obnovení je havárií konzistentní nebo konzistentními soubory.</span><span class="sxs-lookup"><span data-stu-id="ade1d-250">Check whether the recovery point is crash consistent or file-consistent.</span></span> <span data-ttu-id="ade1d-251">Pokud soubor konzistentní, spusťte skript na jiném počítači jejichž operační systém rozpozná zálohy zálohu systému souborů Virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ade1d-251">If file consistent, run the script on another machine whose OS recognizes the backed up VM's filesystem</span></span> |
| <span data-ttu-id="ade1d-252">Specifické pro Windows: Nepodařilo se zobrazit požadované svazky</span><span class="sxs-lookup"><span data-stu-id="ade1d-252">Windows specific: Not able to view the desired volumes</span></span> | <span data-ttu-id="ade1d-253">Jsou disky připojené, ale nebyly nakonfigurované svazky</span><span class="sxs-lookup"><span data-stu-id="ade1d-253">The disks may have been attached but the volumes were not configured</span></span> | <span data-ttu-id="ade1d-254">Na obrazovce Správa disků zjistěte, které další disky týkající se bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="ade1d-254">From the disk management screen, identify the additional disks related to the recovery point.</span></span> <span data-ttu-id="ade1d-255">Pokud jsou některé z těchto disků v režimu offline stavu zkuste přitom online kliknutím pravým tlačítkem na disk a klikněte na tlačítko 'Online.</span><span class="sxs-lookup"><span data-stu-id="ade1d-255">If any of these disks are in offline state try making them online by right-clicking on the disk and click 'Online'</span></span>|
