---
title: "Moderní úložiště zálohy pomocí v2 Azure Backup Server | Microsoft Docs"
description: "Další informace o nových funkcích v v2 serveru Azure Backup. Tento článek popisuje postup upgradu instalaci zálohování serveru."
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
ms.openlocfilehash: 751b9b495fd368dff1f72429707f5f33a0ccb569
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-storage-to-azure-backup-server-v2"></a><span data-ttu-id="e0a1b-104">Přidání úložiště do serveru Azure Backup v2</span><span class="sxs-lookup"><span data-stu-id="e0a1b-104">Add storage to Azure Backup Server v2</span></span>

<span data-ttu-id="e0a1b-105">Azure Backup Server v2 se dodává s System Center 2016 Data Protection Manager moderních úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-105">Azure Backup Server v2 comes with System Center 2016 Data Protection Manager Modern Backup Storage.</span></span> <span data-ttu-id="e0a1b-106">Moderní úložiště záloh nabízí úspory úložiště 50 procent, zálohování, které jsou třikrát rychlejší a efektivnější úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-106">Modern Backup Storage offers storage savings of 50 percent, backups that are three times faster, and more efficient storage.</span></span> <span data-ttu-id="e0a1b-107">Nabízí také deklaracemi pracovního vytížení úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-107">It also offers workload-aware storage.</span></span> 

> [!NOTE]
> <span data-ttu-id="e0a1b-108">Pokud chcete použít moderní úložiště záloh, musíte spustit v2 zálohování serveru v systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-108">To use Modern Backup Storage, you must run Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="e0a1b-109">Pokud spustíte v2 zálohování serveru ve starší verzi systému Windows Server, Azure Backup Server nelze využít výhod moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-109">If you run Backup Server v2 on an earlier version of Windows Server, Azure Backup Server can't take advantage of Modern Backup Storage.</span></span> <span data-ttu-id="e0a1b-110">Místo toho chrání úlohy, stejně jako s v1 zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-110">Instead, it protects workloads as it does with Backup Server v1.</span></span> <span data-ttu-id="e0a1b-111">Další informace najdete v tématu verze zálohování serveru [ochrany matice](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="e0a1b-111">For more information, see the Backup Server version [protection matrix](backup-mabs-protection-matrix.md).</span></span>

## <a name="volumes-in-backup-server-v2"></a><span data-ttu-id="e0a1b-112">Svazky v v2 zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="e0a1b-112">Volumes in Backup Server v2</span></span>

<span data-ttu-id="e0a1b-113">Zálohování serveru v2 přijme svazky úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-113">Backup Server v2 accepts storage volumes.</span></span> <span data-ttu-id="e0a1b-114">Když přidáte svazek, zálohovat Server naformátuje svazek odolný systém souborů (ReFS), který vyžaduje moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-114">When you add a volume, Backup Server formats the volume to Resilient File System (ReFS), which Modern Backup Storage requires.</span></span> <span data-ttu-id="e0a1b-115">Můžete přidat svazek a rozbalte ho později, pokud je potřeba, doporučujeme použít tento pracovní postup:</span><span class="sxs-lookup"><span data-stu-id="e0a1b-115">To add a volume, and to expand it later if you need to, we suggest that you use this workflow:</span></span>

1.  <span data-ttu-id="e0a1b-116">Nastavte zálohování serveru v2 na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-116">Set up Backup Server v2 on a VM.</span></span>
2.  <span data-ttu-id="e0a1b-117">Vytvoření svazku na virtuálním disku ve fondu úložiště:</span><span class="sxs-lookup"><span data-stu-id="e0a1b-117">Create a volume on a virtual disk in a storage pool:</span></span>
    1.  <span data-ttu-id="e0a1b-118">Přidejte disk do fondu úložiště a virtuální disk vytvořit s jednoduché rozložení.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-118">Add a disk to a storage pool and create a virtual disk with simple layout.</span></span>
    2.  <span data-ttu-id="e0a1b-119">Přidejte všechny další disky a rozšířit virtuální disk.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-119">Add any additional disks, and extend the virtual disk.</span></span>
    3.  <span data-ttu-id="e0a1b-120">Vytvořte svazky na virtuálním disku.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-120">Create volumes on the virtual disk.</span></span>
3.  <span data-ttu-id="e0a1b-121">Přidáte svazky do zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-121">Add the volumes to Backup Server.</span></span>
4.  <span data-ttu-id="e0a1b-122">Konfigurace úlohy používající úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-122">Configure workload-aware storage.</span></span>

## <a name="create-a-volume-for-modern-backup-storage"></a><span data-ttu-id="e0a1b-123">Vytvoření svazku pro moderní úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="e0a1b-123">Create a volume for Modern Backup Storage</span></span>

<span data-ttu-id="e0a1b-124">Pomocí zálohování serveru v2 pro svazky, diskového úložiště vám pomůže udržet kontrolu nad úložištěm.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-124">Using Backup Server v2 with volumes as disk storage can help you maintain control over storage.</span></span> <span data-ttu-id="e0a1b-125">Svazek může být jediný disk.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-125">A volume can be a single disk.</span></span> <span data-ttu-id="e0a1b-126">Pokud budete chtít v budoucnu rozšířit úložiště, ale vytvořte svazek mimo disk, který vytvořili pomocí prostorů úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-126">However, if you want to extend storage in the future, create a volume out of a disk created by using storage spaces.</span></span> <span data-ttu-id="e0a1b-127">To může pomoct, pokud chcete rozšířit svazek pro zálohování úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-127">This can help if you want to expand the volume for backup storage.</span></span> <span data-ttu-id="e0a1b-128">Tato část nabízí osvědčené postupy pro vytváření svazku s tímto nastavením.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-128">This section offers best practices for creating a volume with this setup.</span></span>

1. <span data-ttu-id="e0a1b-129">Ve Správci serveru vyberte **Souborová služba a služba úložiště** > **svazky** > **fondy úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-129">In Server Manager, select **File and Storage Services** > **Volumes** > **Storage Pools**.</span></span> <span data-ttu-id="e0a1b-130">V části **fyzické disky**, vyberte **nový fond úložiště**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-130">Under **PHYSICAL DISKS**, select **New Storage Pool**.</span></span> 

    ![Vytvořit nový fond úložiště](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. <span data-ttu-id="e0a1b-132">V **úlohy** rozevíracího seznamu vyberte **nový virtuální Disk**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-132">In the **TASKS** drop-down box, select **New Virtual Disk**.</span></span>

    ![Přidání virtuálního disku](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. <span data-ttu-id="e0a1b-134">Vyberte fond úložiště a pak vyberte **přidat fyzický Disk**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-134">Select the storage pool, and then select **Add Physical Disk**.</span></span>

    ![Přidat fyzický disk](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. <span data-ttu-id="e0a1b-136">Vyberte fyzický disk a pak vyberte **zvětšit virtuální Disk**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-136">Select the physical disk, and then select **Extend Virtual Disk**.</span></span>

    ![Rozšíření virtuálního disku](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. <span data-ttu-id="e0a1b-138">Vyberte virtuální disk a pak vyberte **nový svazek**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-138">Select the virtual disk, and then select **New Volume**.</span></span>

    ![Vytvořte nový svazek](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. <span data-ttu-id="e0a1b-140">V **vyberte server a disk** dialogovém okně, vyberte server a nový disk.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-140">In the **Select the server and disk** dialog, select the server and the new disk.</span></span> <span data-ttu-id="e0a1b-141">Pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-141">Then, select **Next**.</span></span>

    ![Vyberte server a disk](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a><span data-ttu-id="e0a1b-143">Přidat svazky do úložiště disku zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="e0a1b-143">Add volumes to Backup Server disk storage</span></span>

<span data-ttu-id="e0a1b-144">K přidání svazku do zálohování serveru v **správy** podokně znovu prohledat úložiště a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-144">To add a volume to Backup Server, in the **Management** pane, rescan the storage, and then select **Add**.</span></span> <span data-ttu-id="e0a1b-145">Zobrazí se seznam všech svazků, které jsou k dispozici přidávaného pro úložiště záloh serveru.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-145">A list of all the volumes available to be added for Backup Server Storage appears.</span></span> <span data-ttu-id="e0a1b-146">Po dostupných svazků jsou přidány do seznamu vybraných svazků, můžete přiřadit popisný název, který vám pomůžou spravovat je.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-146">After available volumes are added to the list of selected volumes, you can give them a friendly name to help you manage them.</span></span> <span data-ttu-id="e0a1b-147">K formátování tyto svazky odolném systému souborů, takže zálohování serveru můžete použít výhod moderní úložiště záloh, vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-147">To format these volumes to ReFS so Backup Server can use the benefits of Modern Backup Storage, select **OK**.</span></span>

![Přidat k dispozici svazky](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a><span data-ttu-id="e0a1b-149">Nastavení úlohy používající úložiště</span><span class="sxs-lookup"><span data-stu-id="e0a1b-149">Set up workload-aware storage</span></span>

<span data-ttu-id="e0a1b-150">S deklaracemi zatížení úložiště můžete vybrat svazků, které ukládají přednostně určité druhy zátěží.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-150">With workload-aware storage, you can select the volumes that preferentially store certain kinds of workloads.</span></span> <span data-ttu-id="e0a1b-151">Například můžete nastavit nákladné svazky, které podporují vysoký počet vstupně výstupních operací za sekundu (IOPS) pro uložení pouze úlohy, které vyžadují časté, vysoký počet záloh.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-151">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) to store only the workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="e0a1b-152">Příkladem je SQL Server s protokoly transakcí.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-152">An example is SQL Server with transaction logs.</span></span> <span data-ttu-id="e0a1b-153">Jiné úlohy, které se dají zálohovat méně často, jako jsou virtuální počítače, můžete zálohovat do svazků nízkými náklady.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-153">Other workloads that are backed up less frequently, like VMs, can be backed up to low-cost volumes.</span></span>

### <a name="update-dpmdiskstorage"></a><span data-ttu-id="e0a1b-154">Aktualizace DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="e0a1b-154">Update-DPMDiskStorage</span></span>

<span data-ttu-id="e0a1b-155">Úlohy využívající úložiště můžete nastavit pomocí rutiny prostředí PowerShell aktualizace DPMDiskStorage, která aktualizuje vlastnosti svazku ve fondu úložiště na serveru Data Protection Manager.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-155">You can set up workload-aware storage by using the PowerShell cmdlet Update-DPMDiskStorage, which updates the properties of a volume in the storage pool on a Data Protection Manager server.</span></span>

<span data-ttu-id="e0a1b-156">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="e0a1b-156">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
<span data-ttu-id="e0a1b-157">Následující snímek obrazovky ukazuje rutinu Update-DPMDiskStorage v okně prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-157">The following screenshot shows the Update-DPMDiskStorage cmdlet in the PowerShell window.</span></span>

![Příkaz Update-DPMDiskStorage v okně prostředí PowerShell](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

<span data-ttu-id="e0a1b-159">V konzole správce zálohování serveru se projeví změny, které provedete pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-159">The changes you make by using PowerShell are reflected in the Backup Server Administrator Console.</span></span>

![Disky a svazky v konzole pro správu](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a><span data-ttu-id="e0a1b-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0a1b-161">Next steps</span></span>
<span data-ttu-id="e0a1b-162">Po instalaci serveru zálohování, zjistěte, jak připravit server nebo začít chránit zatížení.</span><span class="sxs-lookup"><span data-stu-id="e0a1b-162">After you install Backup Server, learn how to prepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="e0a1b-163">Příprava úlohy zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="e0a1b-163">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="e0a1b-164">Pomocí zálohování serveru zálohovat VMware server</span><span class="sxs-lookup"><span data-stu-id="e0a1b-164">Use Backup Server to back up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="e0a1b-165">Použít zálohování serveru k zálohování systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="e0a1b-165">Use Backup Server to back up SQL Server</span></span>](backup-azure-sql-mabs.md)

