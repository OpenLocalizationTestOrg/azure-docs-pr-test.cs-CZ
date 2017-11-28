---
title: "aaaUse moderní úložiště záloh s v2 serveru Azure Backup | Microsoft Docs"
description: "Další informace o nových funkcích serveru Azure Backup v2 hello. Tento článek popisuje, jak tooupgrade instalaci zálohování serveru."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: b2a1ed27a6a682bd611fea1d2df9ef93314404e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-storage-tooazure-backup-server-v2"></a><span data-ttu-id="5a72c-104">Přidání úložiště tooAzure v2 zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="5a72c-104">Add storage tooAzure Backup Server v2</span></span>

<span data-ttu-id="5a72c-105">Azure Backup Server v2 se dodává s System Center 2016 Data Protection Manager moderních úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="5a72c-105">Azure Backup Server v2 comes with System Center 2016 Data Protection Manager Modern Backup Storage.</span></span> <span data-ttu-id="5a72c-106">Moderní úložiště záloh nabízí úspory úložiště 50 procent, zálohování, které jsou třikrát rychlejší a efektivnější úložiště.</span><span class="sxs-lookup"><span data-stu-id="5a72c-106">Modern Backup Storage offers storage savings of 50 percent, backups that are three times faster, and more efficient storage.</span></span> <span data-ttu-id="5a72c-107">Nabízí také deklaracemi pracovního vytížení úložiště.</span><span class="sxs-lookup"><span data-stu-id="5a72c-107">It also offers workload-aware storage.</span></span> 

> [!NOTE]
> <span data-ttu-id="5a72c-108">toouse moderní úložiště záloh, je nutné spustit v2 zálohování serveru v systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="5a72c-108">toouse Modern Backup Storage, you must run Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="5a72c-109">Pokud spustíte v2 zálohování serveru ve starší verzi systému Windows Server, Azure Backup Server nelze využít výhod moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="5a72c-109">If you run Backup Server v2 on an earlier version of Windows Server, Azure Backup Server can't take advantage of Modern Backup Storage.</span></span> <span data-ttu-id="5a72c-110">Místo toho chrání úlohy, stejně jako s v1 zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="5a72c-110">Instead, it protects workloads as it does with Backup Server v1.</span></span> <span data-ttu-id="5a72c-111">Další informace najdete v tématu verze zálohování serveru hello [ochrany matice](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="5a72c-111">For more information, see hello Backup Server version [protection matrix](backup-mabs-protection-matrix.md).</span></span>

## <a name="volumes-in-backup-server-v2"></a><span data-ttu-id="5a72c-112">Svazky v v2 zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="5a72c-112">Volumes in Backup Server v2</span></span>

<span data-ttu-id="5a72c-113">Zálohování serveru v2 přijme svazky úložiště.</span><span class="sxs-lookup"><span data-stu-id="5a72c-113">Backup Server v2 accepts storage volumes.</span></span> <span data-ttu-id="5a72c-114">Při přidání svazek naformátuje zálohovat Server hello tooResilient svazku systému souborů (ReFS), který vyžaduje moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="5a72c-114">When you add a volume, Backup Server formats hello volume tooResilient File System (ReFS), which Modern Backup Storage requires.</span></span> <span data-ttu-id="5a72c-115">tooadd svazek a tooexpand později Pokud budete potřebovat, doporučujeme použít tento pracovní postup:</span><span class="sxs-lookup"><span data-stu-id="5a72c-115">tooadd a volume, and tooexpand it later if you need to, we suggest that you use this workflow:</span></span>

1.  <span data-ttu-id="5a72c-116">Nastavte zálohování serveru v2 na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="5a72c-116">Set up Backup Server v2 on a VM.</span></span>
2.  <span data-ttu-id="5a72c-117">Vytvoření svazku na virtuálním disku ve fondu úložiště:</span><span class="sxs-lookup"><span data-stu-id="5a72c-117">Create a volume on a virtual disk in a storage pool:</span></span>
    1.  <span data-ttu-id="5a72c-118">Přidejte fond úložiště disku tooa a virtuální disk vytvořit s jednoduché rozložení.</span><span class="sxs-lookup"><span data-stu-id="5a72c-118">Add a disk tooa storage pool and create a virtual disk with simple layout.</span></span>
    2.  <span data-ttu-id="5a72c-119">Přidejte všechny další disky a rozšířit hello virtuálního disku.</span><span class="sxs-lookup"><span data-stu-id="5a72c-119">Add any additional disks, and extend hello virtual disk.</span></span>
    3.  <span data-ttu-id="5a72c-120">Vytvořte svazky na virtuálním disku hello.</span><span class="sxs-lookup"><span data-stu-id="5a72c-120">Create volumes on hello virtual disk.</span></span>
3.  <span data-ttu-id="5a72c-121">Přidejte hello svazky tooBackup serveru.</span><span class="sxs-lookup"><span data-stu-id="5a72c-121">Add hello volumes tooBackup Server.</span></span>
4.  <span data-ttu-id="5a72c-122">Konfigurace úlohy používající úložiště.</span><span class="sxs-lookup"><span data-stu-id="5a72c-122">Configure workload-aware storage.</span></span>

## <a name="create-a-volume-for-modern-backup-storage"></a><span data-ttu-id="5a72c-123">Vytvoření svazku pro moderní úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="5a72c-123">Create a volume for Modern Backup Storage</span></span>

<span data-ttu-id="5a72c-124">Pomocí zálohování serveru v2 pro svazky, diskového úložiště vám pomůže udržet kontrolu nad úložištěm.</span><span class="sxs-lookup"><span data-stu-id="5a72c-124">Using Backup Server v2 with volumes as disk storage can help you maintain control over storage.</span></span> <span data-ttu-id="5a72c-125">Svazek může být jediný disk.</span><span class="sxs-lookup"><span data-stu-id="5a72c-125">A volume can be a single disk.</span></span> <span data-ttu-id="5a72c-126">Pokud chcete tooextend úložiště v hello budoucí, ale vytvořte svazek mimo disk, který vytvořili pomocí prostorů úložiště.</span><span class="sxs-lookup"><span data-stu-id="5a72c-126">However, if you want tooextend storage in hello future, create a volume out of a disk created by using storage spaces.</span></span> <span data-ttu-id="5a72c-127">To může pomoct, pokud chcete tooexpand hello svazku pro úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="5a72c-127">This can help if you want tooexpand hello volume for backup storage.</span></span> <span data-ttu-id="5a72c-128">Tato část nabízí osvědčené postupy pro vytváření svazku s tímto nastavením.</span><span class="sxs-lookup"><span data-stu-id="5a72c-128">This section offers best practices for creating a volume with this setup.</span></span>

1. <span data-ttu-id="5a72c-129">Ve Správci serveru vyberte **Souborová služba a služba úložiště** > **svazky** > **fondy úložiště**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-129">In Server Manager, select **File and Storage Services** > **Volumes** > **Storage Pools**.</span></span> <span data-ttu-id="5a72c-130">V části **fyzické disky**, vyberte **nový fond úložiště**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-130">Under **PHYSICAL DISKS**, select **New Storage Pool**.</span></span> 

    ![Vytvořit nový fond úložiště](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. <span data-ttu-id="5a72c-132">V hello **úlohy** rozevíracího seznamu vyberte **nový virtuální Disk**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-132">In hello **TASKS** drop-down box, select **New Virtual Disk**.</span></span>

    ![Přidání virtuálního disku](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. <span data-ttu-id="5a72c-134">Vyberte fond úložiště hello a pak vyberte **přidat fyzický Disk**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-134">Select hello storage pool, and then select **Add Physical Disk**.</span></span>

    ![Přidat fyzický disk](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. <span data-ttu-id="5a72c-136">Vyberte fyzický disk hello a pak vyberte **zvětšit virtuální Disk**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-136">Select hello physical disk, and then select **Extend Virtual Disk**.</span></span>

    ![Rozšíření virtuálního disku hello](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. <span data-ttu-id="5a72c-138">Vyberte hello virtuální disk a pak vyberte **nový svazek**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-138">Select hello virtual disk, and then select **New Volume**.</span></span>

    ![Vytvořte nový svazek](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. <span data-ttu-id="5a72c-140">V hello **vyberte hello server a disk** dialogové okno, vyberte hello server a hello nový disk.</span><span class="sxs-lookup"><span data-stu-id="5a72c-140">In hello **Select hello server and disk** dialog, select hello server and hello new disk.</span></span> <span data-ttu-id="5a72c-141">Pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-141">Then, select **Next**.</span></span>

    ![Vyberte hello server a disku](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-toobackup-server-disk-storage"></a><span data-ttu-id="5a72c-143">Přidat svazky tooBackup serveru diskového úložiště</span><span class="sxs-lookup"><span data-stu-id="5a72c-143">Add volumes tooBackup Server disk storage</span></span>

<span data-ttu-id="5a72c-144">tooadd tooBackup svazku serveru v hello **správy** podokně Prohledat znovu hello úložiště a pak vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-144">tooadd a volume tooBackup Server, in hello **Management** pane, rescan hello storage, and then select **Add**.</span></span> <span data-ttu-id="5a72c-145">Zobrazí se seznam všech hello svazky dostupné toobe přidat pro úložiště záloh serveru.</span><span class="sxs-lookup"><span data-stu-id="5a72c-145">A list of all hello volumes available toobe added for Backup Server Storage appears.</span></span> <span data-ttu-id="5a72c-146">Po přidání dostupných svazků toohello seznam vybraných svazků, můžete přiřadit popisný název toohelp, můžete spravovat.</span><span class="sxs-lookup"><span data-stu-id="5a72c-146">After available volumes are added toohello list of selected volumes, you can give them a friendly name toohelp you manage them.</span></span> <span data-ttu-id="5a72c-147">tyto svazky tooReFS tak zálohování serveru můžete použít hello výhod moderní úložiště záloh, vyberte tooformat **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a72c-147">tooformat these volumes tooReFS so Backup Server can use hello benefits of Modern Backup Storage, select **OK**.</span></span>

![Přidat k dispozici svazky](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a><span data-ttu-id="5a72c-149">Nastavení úlohy používající úložiště</span><span class="sxs-lookup"><span data-stu-id="5a72c-149">Set up workload-aware storage</span></span>

<span data-ttu-id="5a72c-150">S deklaracemi zatížení úložiště můžete vybrat hello svazků, které přednostně ukládají určité druhy zátěží.</span><span class="sxs-lookup"><span data-stu-id="5a72c-150">With workload-aware storage, you can select hello volumes that preferentially store certain kinds of workloads.</span></span> <span data-ttu-id="5a72c-151">Například můžete nastavit nákladné svazky, které podporují vysoký počet vstupně-výstupních operací na druhý (IOPS) toostore pouze hello úlohy, které vyžadují časté, vysoký počet záloh.</span><span class="sxs-lookup"><span data-stu-id="5a72c-151">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) toostore only hello workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="5a72c-152">Příkladem je SQL Server s protokoly transakcí.</span><span class="sxs-lookup"><span data-stu-id="5a72c-152">An example is SQL Server with transaction logs.</span></span> <span data-ttu-id="5a72c-153">Další úlohy, které se dají zálohovat méně často, jako jsou virtuální počítače, můžete zálohovat svazky toolow náklady.</span><span class="sxs-lookup"><span data-stu-id="5a72c-153">Other workloads that are backed up less frequently, like VMs, can be backed up toolow-cost volumes.</span></span>

### <a name="update-dpmdiskstorage"></a><span data-ttu-id="5a72c-154">Aktualizace DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="5a72c-154">Update-DPMDiskStorage</span></span>

<span data-ttu-id="5a72c-155">Úlohy využívající úložiště můžete nastavit pomocí rutiny prostředí PowerShell hello aktualizace DPMDiskStorage, která aktualizuje hello vlastnosti svazku ve fondu úložiště hello na serveru Data Protection Manager.</span><span class="sxs-lookup"><span data-stu-id="5a72c-155">You can set up workload-aware storage by using hello PowerShell cmdlet Update-DPMDiskStorage, which updates hello properties of a volume in hello storage pool on a Data Protection Manager server.</span></span>

<span data-ttu-id="5a72c-156">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="5a72c-156">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
<span data-ttu-id="5a72c-157">Hello následující snímek obrazovky ukazuje rutinu Update-DPMDiskStorage hello v okně PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="5a72c-157">hello following screenshot shows hello Update-DPMDiskStorage cmdlet in hello PowerShell window.</span></span>

![Hello aktualizace DPMDiskStorage příkazu v okně PowerShell hello](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

<span data-ttu-id="5a72c-159">Hello změny, které provedete pomocí prostředí PowerShell se projeví v hello konzoly správce zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="5a72c-159">hello changes you make by using PowerShell are reflected in hello Backup Server Administrator Console.</span></span>

![Disky a svazky v konzole pro správu hello](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a><span data-ttu-id="5a72c-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a72c-161">Next steps</span></span>
<span data-ttu-id="5a72c-162">Když nainstalujete Backup Server, zjistěte, jak tooprepare váš server, nebo začít chránit zatížení.</span><span class="sxs-lookup"><span data-stu-id="5a72c-162">After you install Backup Server, learn how tooprepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="5a72c-163">Příprava úlohy zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="5a72c-163">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="5a72c-164">Pomocí zálohování serveru tooback server VMware</span><span class="sxs-lookup"><span data-stu-id="5a72c-164">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="5a72c-165">Použít tooback zálohování serveru SQL Server</span><span class="sxs-lookup"><span data-stu-id="5a72c-165">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)

