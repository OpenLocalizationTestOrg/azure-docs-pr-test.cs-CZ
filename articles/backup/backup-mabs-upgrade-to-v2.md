---
title: aaaInstall v2 serveru Azure Backup | Microsoft Docs
description: "Azure v2 zálohování serveru poskytuje vylepšené možnosti zálohování pro ochranu virtuálních počítačů, soubory a složky, úlohy a další. Zjistěte, jak tooinstall nebo upgradu tooAzure v2 zálohování serveru."
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
ms.openlocfilehash: 5b1699dadd3a173f1c0ef91a1a600bc5e12f20ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-azure-backup-server-v2"></a><span data-ttu-id="d25de-104">Nainstalujte Azure Backup Server v2</span><span class="sxs-lookup"><span data-stu-id="d25de-104">Install Azure Backup Server v2</span></span>

<span data-ttu-id="d25de-105">Azure Backup Server pomáhá chránit vaše virtuální počítače (VM), úlohy, soubory a složky a další.</span><span class="sxs-lookup"><span data-stu-id="d25de-105">Azure Backup Server helps protect your virtual machines (VMs), workloads, files and folders, and more.</span></span> <span data-ttu-id="d25de-106">Azure Backup Server v2 staví na serveru Azure Backup v1 a poskytuje nové funkce, které nejsou k dispozici v v1.</span><span class="sxs-lookup"><span data-stu-id="d25de-106">Azure Backup Server v2 builds on Azure Backup Server v1, and gives you new features that are not available in v1.</span></span> <span data-ttu-id="d25de-107">Porovnání funkcí mezi v1 a v2 najdete v tématu [matice ochrany serveru Azure Backup](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="d25de-107">For a comparison of features between v1 and v2, see [Azure Backup Server protection matrix](backup-mabs-protection-matrix.md).</span></span> 

<span data-ttu-id="d25de-108">Hello další funkce Zálohování serveru v2 jsou upgrade z verze 1 zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="d25de-108">hello additional features in Backup Server v2 are an upgrade from Backup Server v1.</span></span> <span data-ttu-id="d25de-109">V1 zálohování serveru ale není předpoklady pro instalaci v2 zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="d25de-109">However, Backup Server v1 is not a prerequisite for installing Backup Server v2.</span></span> <span data-ttu-id="d25de-110">Pokud chcete tooupgrade z zálohovat Server v1 tooBackup Server v2, nainstalujte na hello zálohování serveru ochrany v2 zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="d25de-110">If you want tooupgrade from Backup Server v1 tooBackup Server v2, install Backup Server v2 on hello Backup Server protection server.</span></span> <span data-ttu-id="d25de-111">Existující nastavení zálohování serveru zůstanou beze změn.</span><span class="sxs-lookup"><span data-stu-id="d25de-111">Your existing Backup Server settings remain intact.</span></span>

<span data-ttu-id="d25de-112">Zálohování serveru v2 můžete nainstalovat na Windows Server 2012 R2 nebo Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="d25de-112">You can install Backup Server v2 on Windows Server 2012 R2 or Windows Server 2016.</span></span> <span data-ttu-id="d25de-113">tootake výhod nových funkcí, například System Center 2016 Data Protection Manager moderních úložiště záloh, v2 zálohování serveru je třeba nainstalovat do systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="d25de-113">tootake advantage of new features like System Center 2016 Data Protection Manager Modern Backup Storage, you must install Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="d25de-114">Před provedením upgradu tooor instalace v2 Backup Server, přečtěte si informace o hello [požadavky pro instalaci](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="d25de-114">Before you upgrade tooor install Backup Server v2, read about hello [installation prerequisites](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="d25de-115">Azure Backup Server má hello stejný kód základní jako System Center Data Protection Manager.</span><span class="sxs-lookup"><span data-stu-id="d25de-115">Azure Backup Server has hello same code base as System Center Data Protection Manager.</span></span> <span data-ttu-id="d25de-116">Zálohování serveru v1 je ekvivalentní tooData Protection Manager 2012 R2 a v2 zálohování serveru je ekvivalentní tooData Protection Manager 2016.</span><span class="sxs-lookup"><span data-stu-id="d25de-116">Backup Server v1 is equivalent tooData Protection Manager 2012 R2, and Backup Server v2 is equivalent tooData Protection Manager 2016.</span></span> <span data-ttu-id="d25de-117">Tento článek příležitostně odkazuje dokumentace k Data Protection Manager hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-117">This article occasionally references hello Data Protection Manager documentation.</span></span>
>
>

## <a name="upgrade-backup-server-toov2"></a><span data-ttu-id="d25de-118">Upgrade toov2 zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="d25de-118">Upgrade Backup Server toov2</span></span>
<span data-ttu-id="d25de-119">tooupgrade z zálohovat Server v1 tooBackup v2 serveru, zajistěte, aby vaše instalace má hello požadované aktualizace:</span><span class="sxs-lookup"><span data-stu-id="d25de-119">tooupgrade from Backup Server v1 tooBackup Server v2, make sure your installation has hello required updates:</span></span>

- <span data-ttu-id="d25de-120">[Aktualizovat agenty ochrany hello](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) na hello chráněných serverů.</span><span class="sxs-lookup"><span data-stu-id="d25de-120">[Update hello protection agents](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) on hello protected servers.</span></span>
- <span data-ttu-id="d25de-121">Upgrade systému Windows Server 2012 R2 tooWindows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="d25de-121">Upgrade Windows Server 2012 R2 tooWindows Server 2016.</span></span>
- <span data-ttu-id="d25de-122">Správce vzdáleného serveru Azure Backup upgradujte na všech provozních serverech.</span><span class="sxs-lookup"><span data-stu-id="d25de-122">Upgrade Azure Backup Server Remote Administrator on all production servers.</span></span>
- <span data-ttu-id="d25de-123">Ujistěte se, že jsou zálohy nastaveny toocontinue bez restartování provozním serveru.</span><span class="sxs-lookup"><span data-stu-id="d25de-123">Ensure that backups are set toocontinue without restarting your production server.</span></span>


### <a name="upgrade-steps-for-backup-server-v2"></a><span data-ttu-id="d25de-124">Postup upgradu serveru zálohování v2</span><span class="sxs-lookup"><span data-stu-id="d25de-124">Upgrade steps for Backup Server v2</span></span>

1. <span data-ttu-id="d25de-125">V hello Download Center [stažení aktualizace instalačního programu hello](https://go.microsoft.com/fwlink/?LinkId=626082).</span><span class="sxs-lookup"><span data-stu-id="d25de-125">In hello Download Center, [download hello upgrade installer](https://go.microsoft.com/fwlink/?LinkId=626082).</span></span>

2. <span data-ttu-id="d25de-126">Po extrahování hello Průvodce instalací, ujistěte se, že **spuštění setup.exe** vybrané, a pak vyberte možnost **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d25de-126">After you extract hello setup wizard, make sure that **Execute setup.exe** is selected, and then select **Finish**.</span></span>

  ![Instalační program instalaci - spustit instalační program](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. <span data-ttu-id="d25de-128">V průvodci Microsoft Azure Backup Server hello pod **nainstalovat**, vyberte **Microsoft Azure Backup Server**.</span><span class="sxs-lookup"><span data-stu-id="d25de-128">In hello Microsoft Azure Backup Server wizard, under **Install**, select **Microsoft Azure Backup Server**.</span></span>

  ![Instalační program instalaci - vyberte možnost instalace](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. <span data-ttu-id="d25de-130">Na hello **úvodní** , přečtěte si upozornění hello a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-130">On hello **Welcome** page, review hello warnings, and then select **Next**.</span></span>

  ![Instalační program instalaci – úvodní stránka](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. <span data-ttu-id="d25de-132">Průvodce instalací Hello provede toomake kontrol požadovaných součástí se, že můžete upgradovat prostředí.</span><span class="sxs-lookup"><span data-stu-id="d25de-132">hello setup wizard performs prerequisite checks toomake sure your environment can upgrade.</span></span> <span data-ttu-id="d25de-133">Na hello **požadovaných součástí kontroluje** vyberte **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="d25de-133">On hello **Prerequisite Checks** page, select **Check**.</span></span>

  ![Instalační program instalaci - požadovaných součástí kontroluje stránky](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. <span data-ttu-id="d25de-135">Prostředí musí projít hello kontrol požadovaných součástí.</span><span class="sxs-lookup"><span data-stu-id="d25de-135">Your environment must pass hello prerequisite checks.</span></span> <span data-ttu-id="d25de-136">Pokud vaše prostředí neprojde hello kontroly, poznamenejte si hello problémy a opravte je.</span><span class="sxs-lookup"><span data-stu-id="d25de-136">If your environment doesn't pass hello checks, note hello issues and fix them.</span></span> <span data-ttu-id="d25de-137">Pak vyberte **zkontrolujte znovu**.</span><span class="sxs-lookup"><span data-stu-id="d25de-137">Then, select **Check Again**.</span></span> <span data-ttu-id="d25de-138">Když předáte hello kontroly splnění podmínek, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-138">After you pass hello prerequisite checks, select **Next**.</span></span>

  ![Instalační program instalace - zkontrolujte znovu tlačítko](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. <span data-ttu-id="d25de-140">Na hello **nastavení SQL** vyberte hello relevantní možnost pro instalaci SQL a potom vyberte **zkontrolovat a nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="d25de-140">On hello **SQL Settings** page, select hello relevant option for your SQL installation, and then select **Check and Install**.</span></span>

  ![Instalační program instalaci - stránka nastavení SQL](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  <span data-ttu-id="d25de-142">Hello kontroly může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="d25de-142">hello checks might take a few minutes.</span></span> <span data-ttu-id="d25de-143">Když jsou hello kontroluje dokončení, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-143">When hello checks are finished, select **Next**.</span></span>

  ![Instalační program instalace - zkontrolujte nastavení SQL a tlačítko Instalovat](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. <span data-ttu-id="d25de-145">Na hello **nastavení instalace** stránky, zajistěte, aby všechny změny toohello umístění, kde je nainstalován Backup Server nebo toohello pomocné umístění.</span><span class="sxs-lookup"><span data-stu-id="d25de-145">On hello **Installation Settings** page, make any changes toohello location where Backup Server is installed, or toohello Scratch Location.</span></span> <span data-ttu-id="d25de-146">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-146">Select **Next**.</span></span>

  ![Instalační program instalaci - stránka nastavení instalace](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. <span data-ttu-id="d25de-148">toofinish hello Průvodce instalací, vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d25de-148">toofinish hello setup wizard, select **Finish**.</span></span>

  ![Instalační program instalaci - dokončit](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a><span data-ttu-id="d25de-150">Přidání úložiště pro moderní úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="d25de-150">Add storage for Modern Backup Storage</span></span>

<span data-ttu-id="d25de-151">tooimprove efektivitu úložiště zálohování, zálohování serveru v2 přidává podporu pro svazky.</span><span class="sxs-lookup"><span data-stu-id="d25de-151">tooimprove backup storage efficiency, Backup Server v2 adds support for volumes.</span></span> <span data-ttu-id="d25de-152">Podobně jako Backup Server v1 v2 zálohování serveru podporuje disky.</span><span class="sxs-lookup"><span data-stu-id="d25de-152">Like Backup Server v1, Backup Server v2 supports disks.</span></span>

### <a name="add-volumes-and-disks"></a><span data-ttu-id="d25de-153">Přidat svazky a disků</span><span class="sxs-lookup"><span data-stu-id="d25de-153">Add volumes and disks</span></span>
<span data-ttu-id="d25de-154">Pokud spustíte v2 zálohování serveru v systému Windows Server 2016, můžete data záloh toostore svazky.</span><span class="sxs-lookup"><span data-stu-id="d25de-154">If you run Backup Server v2 on Windows Server 2016, you can use volumes toostore backup data.</span></span> <span data-ttu-id="d25de-155">Svazky nabízejí úspory úložiště a rychlejší zálohování.</span><span class="sxs-lookup"><span data-stu-id="d25de-155">Volumes offer storage savings and faster backups.</span></span> <span data-ttu-id="d25de-156">Vzhledem k tomu, že jsou svazky tooBackup nový Server, musíte je přidat.</span><span class="sxs-lookup"><span data-stu-id="d25de-156">Because volumes are new tooBackup Server, you must add them.</span></span> 

<span data-ttu-id="d25de-157">Když přidáte tooBackup svazku serveru, můžete přiřadit svazek hello popisný název.</span><span class="sxs-lookup"><span data-stu-id="d25de-157">When you add a volume tooBackup Server, you can give hello volume a friendly name.</span></span> <span data-ttu-id="d25de-158">Klikněte na tlačítko hello **popisný název** sloupec hello svazku chcete tooname.</span><span class="sxs-lookup"><span data-stu-id="d25de-158">Click hello **Friendly Name** column of hello volume you want tooname.</span></span> <span data-ttu-id="d25de-159">V případě potřeby můžete později změnit název hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-159">You can change hello name later, if necessary.</span></span> <span data-ttu-id="d25de-160">Také můžete použít tooadd prostředí PowerShell nebo změnit popisné názvy pro svazky.</span><span class="sxs-lookup"><span data-stu-id="d25de-160">You also can use PowerShell tooadd or change friendly names for volumes.</span></span>

<span data-ttu-id="d25de-161">tooadd svazek v hello konzoly pro správu:</span><span class="sxs-lookup"><span data-stu-id="d25de-161">tooadd a volume in hello Administrator Console:</span></span>

1. <span data-ttu-id="d25de-162">V konzole správce serveru zálohování Azure hello, vyberte **správy** > **diskového úložiště** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d25de-162">In hello Azure Backup Server Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Průvodce přidáním úložiště disku otevřete hello](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    <span data-ttu-id="d25de-164">Otevře se Průvodce přidáním úložiště disku hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-164">This opens hello Add Disk Storage wizard.</span></span>

2. <span data-ttu-id="d25de-165">Na hello **přidání disku úložiště** stránku hello **dostupných svazků** , vyberte svazek a pak vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d25de-165">On hello **Add Disk Storage** page, in hello **Available volumes** box, select a volume, and then select **Add**.</span></span>
3. <span data-ttu-id="d25de-166">V hello **vybrané svazky** pole, zadejte popisný název pro hello svazek a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="d25de-166">In hello **Selected volumes** box, enter a friendly name for hello volume, and then select **OK**.</span></span>

      ![Průvodce přidáním úložiště disku – Přidání svazku](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  <span data-ttu-id="d25de-168">Pokud chcete tooadd disk, hello disk musí patřit tooa ochranné skupiny, která je starší verze úložiště.</span><span class="sxs-lookup"><span data-stu-id="d25de-168">If you want tooadd a disk, hello disk must belong tooa protection group that has legacy storage.</span></span> <span data-ttu-id="d25de-169">Tyto disky použít jenom pro tyto skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-169">These disks can only be used for these protection groups.</span></span> <span data-ttu-id="d25de-170">Pokud Backup Server nemá zdroje, které mají další ochranu starší verze, se neuvádějí hello disku.</span><span class="sxs-lookup"><span data-stu-id="d25de-170">If Backup Server doesn't have sources that have legacy protection, hello disk isn't listed.</span></span>

  <span data-ttu-id="d25de-171">Další informace o přidávání disků, najdete v tématu [přidávání disků tooincrease starší verze úložiště](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span><span class="sxs-lookup"><span data-stu-id="d25de-171">For more information about adding disks, see [Adding disks tooincrease legacy storage](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span></span> <span data-ttu-id="d25de-172">Disk nelze udělit popisný název.</span><span class="sxs-lookup"><span data-stu-id="d25de-172">You can't give a disk a friendly name.</span></span>


### <a name="assign-workloads-toovolumes"></a><span data-ttu-id="d25de-173">Přiřadit toovolumes úlohy</span><span class="sxs-lookup"><span data-stu-id="d25de-173">Assign workloads toovolumes</span></span>

<span data-ttu-id="d25de-174">Zálohování serveru zadejte, které úlohy jsou přiřazeny toowhich svazky.</span><span class="sxs-lookup"><span data-stu-id="d25de-174">In Backup Server, you specify which workloads are assigned toowhich volumes.</span></span> <span data-ttu-id="d25de-175">Například můžete nastavit nákladné svazky, které podporují vysoký počet vstupně-výstupních operací na druhý (IOPS) úlohy pouze toostore, které vyžadují časté, vysoký počet záloh.</span><span class="sxs-lookup"><span data-stu-id="d25de-175">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) toostore only workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="d25de-176">Příkladem je SQL Server s protokoly transakcí.</span><span class="sxs-lookup"><span data-stu-id="d25de-176">An example is SQL Server with transaction logs.</span></span>

#### <a name="update-dpmdiskstorage"></a><span data-ttu-id="d25de-177">Aktualizace DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="d25de-177">Update-DPMDiskStorage</span></span>

<span data-ttu-id="d25de-178">Vlastnosti hello tooupdate svazku ve fondu úložiště hello zálohování serveru, použijte rutinu Powershellu hello DPMDiskStorage aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d25de-178">tooupdate hello properties of a volume in hello storage pool in Backup Server, use hello PowerShell cmdlet Update-DPMDiskStorage.</span></span>

<span data-ttu-id="d25de-179">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="d25de-179">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

<span data-ttu-id="d25de-180">Všechny změny, které můžete provést pomocí prostředí PowerShell se projeví v hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d25de-180">All changes that you make by using PowerShell are reflected in hello UI.</span></span>


## <a name="protect-data-sources"></a><span data-ttu-id="d25de-181">Chránit zdroje dat</span><span class="sxs-lookup"><span data-stu-id="d25de-181">Protect data sources</span></span>
<span data-ttu-id="d25de-182">toobegin ochranu datových zdrojů, vytvořte skupinu ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-182">toobegin protecting data sources, create a protection group.</span></span> <span data-ttu-id="d25de-183">Hello následující kroky zvýraznění změny nebo přidání toohello průvodci nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-183">hello following steps highlight changes or additions toohello New Protection Group wizard.</span></span>

<span data-ttu-id="d25de-184">toocreate skupinu ochrany:</span><span class="sxs-lookup"><span data-stu-id="d25de-184">toocreate a protection group:</span></span>

1. <span data-ttu-id="d25de-185">V hello konzoly správce zálohování serveru, vyberte **ochrany**.</span><span class="sxs-lookup"><span data-stu-id="d25de-185">In hello Backup Server Administrator Console, select **Protection**.</span></span>

2. <span data-ttu-id="d25de-186">Na pásu karet nástroje hello, vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="d25de-186">On hello tool ribbon, select **New**.</span></span>

    <span data-ttu-id="d25de-187">Otevře se Průvodce vytvořením nové skupiny ochrany hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-187">This opens hello Create New Protection Group wizard.</span></span>

  ![Průvodce vytvořením nové skupiny ochrany](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. <span data-ttu-id="d25de-189">Na hello **úvodní** vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-189">On hello **Welcome** page, select **Next**.</span></span>
4. <span data-ttu-id="d25de-190">Na hello **vybrat typ skupiny ochrany** vyberte hello typ skupiny ochrany má toocreate a potom vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-190">On hello **Select Protection Group Type** page, select hello type of protection group you want toocreate, and then select **Next**.</span></span>

  ![Stránka Typ vyberte skupinu ochrany](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. <span data-ttu-id="d25de-192">Na hello **vybrat členy skupiny** stránku hello **Dostupní členové** podokně, hello členy s jsou uvedeny agenty ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-192">On hello **Select Group Members** page, in hello **Available members** pane, hello members with protection agents are listed.</span></span> <span data-ttu-id="d25de-193">V tomto příkladu vyberte svazek D:\ a E:\ a přidat je toohello **vybrané členy** podokně.</span><span class="sxs-lookup"><span data-stu-id="d25de-193">For this example, select volume D:\ and E:\ and add them toohello **Selected members** pane.</span></span> <span data-ttu-id="d25de-194">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-194">Select **Next**.</span></span>

  ![Vyberte skupiny členy stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. <span data-ttu-id="d25de-196">Na hello **vyberte způsob ochrany dat** stránky, zadejte **název skupiny ochrany**, vyberte způsob ochrany hello a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-196">On hello **Select Data Protection Method** page, enter a **Protection group name**, select hello protection method, and then select **Next**.</span></span> <span data-ttu-id="d25de-197">Pokud chcete krátkodobou ochranu, je nutné vybrat hello **disku** zálohování metoda.</span><span class="sxs-lookup"><span data-stu-id="d25de-197">If you want short-term protection, you must select hello **Disk** backup method.</span></span>

  ![Vyberte způsob ochrany dat stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. <span data-ttu-id="d25de-199">Na hello **zadat krátkodobé cíle** stránky, vyberte hello podrobnosti **rozsah uchování** a **četnost synchronizace**.</span><span class="sxs-lookup"><span data-stu-id="d25de-199">On hello **Specify Short-Term Goals** page, select hello details for **Retention range** and **Synchronization frequency**.</span></span> <span data-ttu-id="d25de-200">Pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d25de-200">Then, select **Next**.</span></span> <span data-ttu-id="d25de-201">Volitelně můžete toochange hello plán pro při body obnovení jsou přijatých, vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="d25de-201">Optionally, toochange hello schedule for when recovery points are taken, select **Modify**.</span></span>

  ![Určení krátkodobých cílů stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. <span data-ttu-id="d25de-203">Na hello **zkontrolovat přidělení diskového úložiště** zkontrolujte podrobnosti o zdrojích dat hello jste vybrali, jejich velikosti a hodnoty pro toobe místo hello zřízený a hello cílový svazek úložiště.</span><span class="sxs-lookup"><span data-stu-id="d25de-203">On hello **Review Disk Storage Allocation** page, review details about hello data sources you selected, their size, and  values for hello space toobe provisioned and hello target storage volume.</span></span>

  ![Stránka Kontrola přidělení diskového úložiště](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  <span data-ttu-id="d25de-205">Svazky úložiště jsou založené na hello zatížení svazku přidělení (nastavte pomocí prostředí PowerShell) a hello úložiště k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d25de-205">Storage volumes are based on hello workload volume allocation (set by using PowerShell) and hello available storage.</span></span> <span data-ttu-id="d25de-206">Svazky úložiště hello můžete změnit výběrem jiných svazků v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-206">You can change hello storage volumes by selecting other volumes in hello drop-down menu.</span></span> <span data-ttu-id="d25de-207">Pokud změníte hodnotu hello **úložiště v cíli**, hello hodnotu pro **úložiště disku** dynamicky změní tooreflect hodnoty v části **volného místa** a **Underprovisioned místo**.</span><span class="sxs-lookup"><span data-stu-id="d25de-207">If you change hello value for **Target Storage**, hello value for **Available disk storage** dynamically changes tooreflect values under **Free Space** and **Underprovisioned Space**.</span></span>

  <span data-ttu-id="d25de-208">Pokud zdroje dat hello růst podle plánu, hello hodnotu hello **Underprovisioned místo** sloupec v **úložiště disku** odráží hello množství další úložiště, které je potřeba.</span><span class="sxs-lookup"><span data-stu-id="d25de-208">If hello data sources grow as planned, hello value for hello **Underprovisioned Space** column in **Available disk storage** reflects hello amount of additional storage that's needed.</span></span> <span data-ttu-id="d25de-209">Použijte tento plán toohelp hodnotu úložiště, musí technologie smooth zálohy.</span><span class="sxs-lookup"><span data-stu-id="d25de-209">Use this value toohelp plan your storage needs for smooth backups.</span></span> <span data-ttu-id="d25de-210">Pokud hello hodnota nula, neexistují v blízké budoucnosti hello potenciální problémy s úložištěm.</span><span class="sxs-lookup"><span data-stu-id="d25de-210">If hello value is zero, there are no potential problems with storage in hello foreseeable future.</span></span> <span data-ttu-id="d25de-211">Pokud je hodnota hello číslo větší než nula, nemáte dostatečné úložiště přidělené (podle vaší ochrany zásady a hello velikost dat chráněných členů).</span><span class="sxs-lookup"><span data-stu-id="d25de-211">If hello value is a number other than zero, you do not have sufficient storage allocated  (based on your protection policy and hello data size of your protected members).</span></span>

  ![Nevytížených diskového úložiště](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   <span data-ttu-id="d25de-213">toofinish vytváření hello skupiny, dokončení Průvodce ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-213">toofinish creating your protection group, complete hello wizard.</span></span>

## <a name="migrate-legacy-storage-toomodern-backup-storage"></a><span data-ttu-id="d25de-214">Migrovat starší verze úložiště tooModern úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="d25de-214">Migrate legacy storage tooModern Backup Storage</span></span>
<span data-ttu-id="d25de-215">Po upgradu tooor instalace v2 zálohování serveru a tooWindows hello upgradu operačního systému serveru 2016 se aktualizujte vaši toouse skupiny ochrany moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="d25de-215">After you upgrade tooor install Backup Server v2 and upgrade hello operating system tooWindows Server 2016, update your protection groups toouse Modern Backup Storage.</span></span> <span data-ttu-id="d25de-216">Ve výchozím nastavení se nezmění skupin ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-216">By default, protection groups are not changed.</span></span> <span data-ttu-id="d25de-217">Budou pokračovat v práci toofunction jako původně určené.</span><span class="sxs-lookup"><span data-stu-id="d25de-217">They continue toofunction as they were initially set up.</span></span> 

<span data-ttu-id="d25de-218">Aktualizace toouse skupiny ochrany moderní úložiště záloh je volitelné.</span><span class="sxs-lookup"><span data-stu-id="d25de-218">Updating protection groups toouse Modern Backup Storage is optional.</span></span> <span data-ttu-id="d25de-219">skupiny ochrany hello tooupdate, zastavte ochranu všech zdrojů dat pomocí hello zachovat data možnost.</span><span class="sxs-lookup"><span data-stu-id="d25de-219">tooupdate hello protection group, stop protection of all data sources by using hello retain data option.</span></span> <span data-ttu-id="d25de-220">Pak přidejte hello datového zdroje tooa nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-220">Then, add hello data sources tooa new protection group.</span></span>

1. <span data-ttu-id="d25de-221">V konzole pro správu hello, vyberte hello **ochrany** funkce.</span><span class="sxs-lookup"><span data-stu-id="d25de-221">In hello Administrator Console, select hello **Protection** feature.</span></span> <span data-ttu-id="d25de-222">V hello **člena skupiny ochrany** seznamu, klikněte pravým tlačítkem na hello člen a pak vyberte **zastavit ochranu člena**.</span><span class="sxs-lookup"><span data-stu-id="d25de-222">In hello **Protection Group Member** list, right-click hello member, and then select **Stop protection of member**.</span></span>

  ![Zastavit ochranu člena](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. <span data-ttu-id="d25de-224">V hello **odebrat ze skupiny** dialogové okno, zkontrolujte místo na disku hello používá a dostupné volné místo pro fond úložiště hello hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-224">In hello **Remove from Group** dialog box, review hello used disk space and hello available free space for hello storage pool.</span></span> <span data-ttu-id="d25de-225">Výchozí Hello je tooleave hello body obnovení na disku hello a mohly tooexpire podle jejich přidružené uchování zásad.</span><span class="sxs-lookup"><span data-stu-id="d25de-225">hello default is tooleave hello recovery points on hello disk and allow them tooexpire per their associated retention policy.</span></span> <span data-ttu-id="d25de-226">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d25de-226">Click **OK**.</span></span>

  <span data-ttu-id="d25de-227">Pokud chcete tooimmediately návratový hello používá disku fondu úložiště volné místo toohello, vyberte hello **odstranit repliku na disku** políčko toodelete hello zálohovaná data (a bodů obnovení) přidružené tohoto člena.</span><span class="sxs-lookup"><span data-stu-id="d25de-227">If you want tooimmediately return hello used disk space toohello free storage pool, select hello **Delete replica on disk** check box toodelete hello backup data (and recovery points) associated with that member.</span></span>

  ![Odebrat ze skupiny, dialogové okno](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. <span data-ttu-id="d25de-229">Vytvořte skupinu ochrany, která používá moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="d25de-229">Create a protection group that uses Modern Backup Storage.</span></span> <span data-ttu-id="d25de-230">Zahrnout zdroje dat hello bez ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-230">Include hello unprotected data sources.</span></span>


## <a name="add-disks-tooincrease-legacy-storage"></a><span data-ttu-id="d25de-231">Přidat disky tooincrease starší verze úložiště</span><span class="sxs-lookup"><span data-stu-id="d25de-231">Add disks tooincrease legacy storage</span></span>

<span data-ttu-id="d25de-232">Pokud chcete, aby starší verze úložiště dat toouse pomocí zálohování serveru, bude pravděpodobně nutné tooadd disky tooincrease starší verze úložiště.</span><span class="sxs-lookup"><span data-stu-id="d25de-232">If you want toouse legacy storage with Backup Server, you might need tooadd disks tooincrease legacy storage.</span></span> 

<span data-ttu-id="d25de-233">tooadd diskového úložiště:</span><span class="sxs-lookup"><span data-stu-id="d25de-233">tooadd disk storage:</span></span>

1. <span data-ttu-id="d25de-234">V konzole pro správu hello, vyberte **správy** > **diskového úložiště** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d25de-234">In hello Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Přidat dialogové okno diskového úložiště](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. <span data-ttu-id="d25de-236">V hello **přidání disku úložiště** dialogovém okně, vyberte **přidat disky**.</span><span class="sxs-lookup"><span data-stu-id="d25de-236">In hello **Add Disk Storage** dialog, select **Add disks**.</span></span>

5. <span data-ttu-id="d25de-237">V seznamu hello dostupných disků, vyberte hello disky, které chcete tooadd, vyberte **přidat**a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="d25de-237">In hello list of available disks, select hello disks you want tooadd, select **Add**, and then select **OK**.</span></span>

## <a name="update-hello-data-protection-manager-protection-agent"></a><span data-ttu-id="d25de-238">Aktualizovat agenta ochrany hello Data Protection Manager</span><span class="sxs-lookup"><span data-stu-id="d25de-238">Update hello Data Protection Manager protection agent</span></span>

<span data-ttu-id="d25de-239">Zálohování serveru používá hello System Center Data Protection Manager protection agent aktualizací.</span><span class="sxs-lookup"><span data-stu-id="d25de-239">Backup Server uses hello System Center Data Protection Manager protection agent for updates.</span></span> <span data-ttu-id="d25de-240">Pokud provádíte upgrade agenta ochrany, který není připojený toohello sítí, nemůžete použít hello konzole pro správu Data Protection Manager toocomplete upgrade připojeného agenta.</span><span class="sxs-lookup"><span data-stu-id="d25de-240">If you are upgrading a protection agent that is not connected toohello network, you cannot use hello Data Protection Manager Administrator Console toocomplete a connected agent upgrade.</span></span> <span data-ttu-id="d25de-241">Je nutné upgradovat agenta ochrany hello v prostředí neaktivní domény.</span><span class="sxs-lookup"><span data-stu-id="d25de-241">You must upgrade hello protection agent in a nonactive domain environment.</span></span> <span data-ttu-id="d25de-242">Dokud hello klientský počítač je připojený toohello síť, hello konzole pro správu Data Protection Manager ukazuje, že aktualizace agenta ochrany hello čeká na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="d25de-242">Until hello client computer is connected toohello network, hello Data Protection Manager Administrator Console shows that hello protection agent update is pending.</span></span>

<span data-ttu-id="d25de-243">Hello následující části popisují, jak tooupdate agenty ochrany pro klientské počítače, které jsou připojené a klientské počítače, které nejsou připojené.</span><span class="sxs-lookup"><span data-stu-id="d25de-243">hello following sections describe how tooupdate protection agents for client computers that are connected and client computers that are not connected.</span></span>

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a><span data-ttu-id="d25de-244">Aktualizace agenta ochrany pro připojené klientské počítače</span><span class="sxs-lookup"><span data-stu-id="d25de-244">Update a protection agent for a connected client computer</span></span>

1. <span data-ttu-id="d25de-245">V hello konzoly správce zálohování serveru, vyberte **správy** > **agenti**.</span><span class="sxs-lookup"><span data-stu-id="d25de-245">In hello Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="d25de-246">V podokně zobrazení hello vyberte hello klientské počítače, pro které chcete agenta ochrany tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-246">In hello display pane, select hello client computers for which you want tooupdate hello protection agent.</span></span>

  > [!NOTE]
  > <span data-ttu-id="d25de-247">Hello **aktualizací agenta** sloupec zobrazuje, když je k dispozici pro každý chráněný počítač aktualizace agenta ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-247">hello **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="d25de-248">V hello **akce** podokně, hello **aktualizace** akce je dostupná, pouze když je vybrán chráněný počítač a jsou k dispozici aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d25de-248">In hello **Actions** pane, hello **Update** action is available only when a protected computer is selected and updates are available.</span></span>
  >
  >

3. <span data-ttu-id="d25de-249">tooinstall aktualizovat agenty ochrany na počítačích hello vybrané v hello **akce** podokně, vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="d25de-249">tooinstall updated protection agents on hello selected computers, in hello **Actions** pane, select **Update**.</span></span>

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a><span data-ttu-id="d25de-250">Aktualizace agenta ochrany na klientském počítači, který není připojen</span><span class="sxs-lookup"><span data-stu-id="d25de-250">Update a protection agent on a client computer that is not connected</span></span>

1. <span data-ttu-id="d25de-251">V hello konzoly správce zálohování serveru, vyberte **správy** > **agenti**.</span><span class="sxs-lookup"><span data-stu-id="d25de-251">In hello Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="d25de-252">V podokně zobrazení hello vyberte hello klientské počítače, pro které chcete agenta ochrany tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-252">In hello display pane, select hello client computers for which you want tooupdate hello protection agent.</span></span>

  > [!NOTE]
   > <span data-ttu-id="d25de-253">Hello **aktualizací agenta** sloupec zobrazuje, když je k dispozici pro každý chráněný počítač aktualizace agenta ochrany.</span><span class="sxs-lookup"><span data-stu-id="d25de-253">hello **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="d25de-254">V hello **akce** podokně, hello **aktualizace** akce není dostupná, když je vybrán chráněný počítač, pokud jsou k dispozici aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d25de-254">In hello **Actions** pane, hello **Update** action is not available when a protected computer is selected unless updates are available.</span></span>
  >
  >

3. <span data-ttu-id="d25de-255">tooinstall aktualizovat agenty ochrany na počítačích hello vybrána, vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="d25de-255">tooinstall updated protection agents on hello selected computers, select **Update**.</span></span>

4. <span data-ttu-id="d25de-256">Pro klientský počítač, který není připojený toohello síti, dokud hello počítači je připojených toohello síť, hello **stav agenta** sloupci se zobrazuje stav **aktualizace čeká na vyřízení**.</span><span class="sxs-lookup"><span data-stu-id="d25de-256">For a client computer that is not connected toohello network, until hello computer is connected toohello network, hello **Agent Status** column shows a status of **Update Pending**.</span></span>

  <span data-ttu-id="d25de-257">Až klientský počítač je připojený toohello síť, hello **aktualizací agenta** sloupci hello klientského počítače se zobrazuje stav **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="d25de-257">After a client computer is connected toohello network, hello **Agent Updates** column for hello client computer shows a status of **Updating**.</span></span>
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-hello-new-version-with-azure"></a><span data-ttu-id="d25de-258">Přesunout starší verze skupin ochrany z předchozí verzi aplikace a synchronizace hello novou verzi pomocí Azure</span><span class="sxs-lookup"><span data-stu-id="d25de-258">Move legacy Protection groups from old version and sync hello new version with Azure</span></span>

<span data-ttu-id="d25de-259">Jakmile serveru Azure Backup a hello operačního systému jsou obě aktualizovány, jste nové zdroje dat připravené tooprotect pomocí moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="d25de-259">Once Azure Backup Server and hello OS are both updated, you are ready tooprotect new data sources using Modern Backup Storage.</span></span> <span data-ttu-id="d25de-260">Ale již chráněného zdroje dat bude toobe chráněné v hello starší verze způsobem, jak byly v serveru Azure Backup, ale všechny nové ochrany použije moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="d25de-260">However already protected data sources will continue toobe protected in hello legacy way as they were in Azure Backup Server but all new protection will use Modern Backup Storage.</span></span>

<span data-ttu-id="d25de-261">Následující kroky jsou toomigrate zdroje dat z režimu starší verze ochrany tooModern zálohování úložiště.</span><span class="sxs-lookup"><span data-stu-id="d25de-261">Below steps are toomigrate data sources from legacy  mode of protection tooModern backup storage.</span></span>

<span data-ttu-id="d25de-262">• Přidat hello nové svazky toohello fondu úložiště DPM a přiřadit popisné názvy a data zdroj značky v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="d25de-262">• Add hello new volume(s) toohello DPM storage pool and assign friendly names and data source tags if desired.</span></span>
<span data-ttu-id="d25de-263">• Pro každý zdroj dat, který je v režimu starší verze, zastavte ochranu zdroje dat hello a "Uchovat chráněná Data".</span><span class="sxs-lookup"><span data-stu-id="d25de-263">• For each data source that is in legacy mode, stop protection of hello data sources and “Retain Protected Data”.</span></span>  <span data-ttu-id="d25de-264">To vám umožní obnovení starých bodů obnovení po migraci.</span><span class="sxs-lookup"><span data-stu-id="d25de-264">This will allow recovery of old recovery points after migration.</span></span>

<span data-ttu-id="d25de-265">• Vytvořte nové PG a vyberte hello zdroje dat, které jsou toobe uložené pomocí nový formát.</span><span class="sxs-lookup"><span data-stu-id="d25de-265">• Create a new PG and select hello data sources that are toobe stored using new format.</span></span>
<span data-ttu-id="d25de-266">• Aplikace DPM provede kopie repliky ze starší verze úložiště záloh hello do hello moderní úložiště zálohování svazku místně.</span><span class="sxs-lookup"><span data-stu-id="d25de-266">• DPM will do a replica copy from hello legacy backup storage into hello Modern Backup Storage volume locally.</span></span>
<span data-ttu-id="d25de-267">Poznámka: Toto se zobrazí jako • operaci po obnovení úlohy, které všechny nové synchronizace a bodů obnovení bude uložen v moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="d25de-267">Note: This will be seen as a post-recovery operation job • All new sync and recovery points will then be stored in Modern Backup Storage.</span></span>
<span data-ttu-id="d25de-268">• Staré body obnovení se vyřazují, protože platnost a nakonec uvolněte místo na disku hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-268">• Old recovery points will be pruned out as they expire and eventually free up hello disk space.</span></span>
<span data-ttu-id="d25de-269">• Po všechny starší verze svazky hello jsou odstraněny z hello původní úložiště, hello disku lze odebrat z Azure backup a hello systému.</span><span class="sxs-lookup"><span data-stu-id="d25de-269">• Once all hello legacy volumes are deleted from hello old storage, hello disk can be removed from Azure backup and hello system.</span></span>
<span data-ttu-id="d25de-270">• Proveďte zálohování hello Azure DPMDB.</span><span class="sxs-lookup"><span data-stu-id="d25de-270">• Take a backup of hello  Azure DPMDB.</span></span>

<span data-ttu-id="d25de-271">Část 2:-důležité položky > Nový server hello potřebovat toobe stejný název jako původní server Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-271">Part 2: -Important items> hello new server will need toobe named same as hello original Azure Backup server.</span></span> <span data-ttu-id="d25de-272">Pokud chcete toouse staré fondu úložiště a body obnovení tooretain DPMDB - musí mít zálohu databáze aplikace DPMDB je nutné obnovit toobe nelze změnit název hello hello nový server Azure backup</span><span class="sxs-lookup"><span data-stu-id="d25de-272">You cannot change hello name of hello new Azure backup server if you want toouse old storage pool and DPMDB tooretain recovery points -Must have backup of DPMDB  as it will need toobe restored</span></span>

1) <span data-ttu-id="d25de-273">Vypnutí hello původní server Azure backup nebo trvat vypnout hello přenosu.</span><span class="sxs-lookup"><span data-stu-id="d25de-273">Shutdown hello original Azure backup server or take it off hello wire.</span></span>
2) <span data-ttu-id="d25de-274">Resetovat hello účet počítače ve službě active directory.</span><span class="sxs-lookup"><span data-stu-id="d25de-274">Reset hello machine account in active directory.</span></span>
3) <span data-ttu-id="d25de-275">Nainstalujte Server 2016 na nový počítač a název je hello stejný název počítače jako původní server Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="d25de-275">Install Server 2016 on new machine and name it hello same machine name as hello original Azure Backup server.</span></span>
4) <span data-ttu-id="d25de-276">Připojení k hello domény</span><span class="sxs-lookup"><span data-stu-id="d25de-276">Join hello Domain</span></span>
5) <span data-ttu-id="d25de-277">Nainstalujte server Azure Backup V2 (disků fondu úložiště aplikace DPM přesunout z původní server a import)</span><span class="sxs-lookup"><span data-stu-id="d25de-277">Install Azure Backup server V2 (Move DPM Storage pool disks from old server and import)</span></span>
6) <span data-ttu-id="d25de-278">Obnovení hello DPMDB prováděné od konce část 2</span><span class="sxs-lookup"><span data-stu-id="d25de-278">Restore hello DPMDB taken from end of part 2</span></span>
7) <span data-ttu-id="d25de-279">Připojte hello úložiště z hello původní backup server toohello nový server.</span><span class="sxs-lookup"><span data-stu-id="d25de-279">Attach hello storage from hello original backup server toohello new server.</span></span>
8) <span data-ttu-id="d25de-280">Z obnovení SQL hello DPMDB</span><span class="sxs-lookup"><span data-stu-id="d25de-280">From SQL Restore hello DPMDB</span></span>
9) <span data-ttu-id="d25de-281">Z příkazového řádku správce na disku cd tooMicrosoft nový server Azure Backup nainstalujte umístění a složky koše</span><span class="sxs-lookup"><span data-stu-id="d25de-281">From admin command line on new server cd tooMicrosoft Azure Backup install location and bin folder</span></span>

<span data-ttu-id="d25de-282">Příklad cesty: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span><span class="sxs-lookup"><span data-stu-id="d25de-282">Path example: C:\windows\system32>cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span></span>
<span data-ttu-id="d25de-283">zálohování tooAzure spusťte příkaz DPMSYNC-SYNC</span><span class="sxs-lookup"><span data-stu-id="d25de-283">tooAzure backup Run DPMSYNC -SYNC</span></span>

10) <span data-ttu-id="d25de-284">Spusťte synchronizaci DPMSYNC-SYNC Poznámka: Pokud jste přidali nový fond úložiště DPM toohello disky místo přesunutím hello staré, spusťte příkaz DPMSYNC - Reallocatereplica</span><span class="sxs-lookup"><span data-stu-id="d25de-284">Run DPMSYNC -SYNC Note If you have added NEW disks toohello DPM Storage pool instead of moving hello old ones, then run DPMSYNC -Reallocatereplica</span></span>

## <a name="new-powershell-cmdlets-in-v2"></a><span data-ttu-id="d25de-285">Nové rutiny prostředí PowerShell v v2</span><span class="sxs-lookup"><span data-stu-id="d25de-285">New PowerShell cmdlets in v2</span></span>

<span data-ttu-id="d25de-286">Při instalaci serveru Azure Backup v2 jsou k dispozici dvě nové rutiny:</span><span class="sxs-lookup"><span data-stu-id="d25de-286">When you install Azure Backup Server v2, two new cmdlets are available:</span></span> 
* [<span data-ttu-id="d25de-287">DPMRecoveryPoint připojení</span><span class="sxs-lookup"><span data-stu-id="d25de-287">Mount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787159.aspx)
* [<span data-ttu-id="d25de-288">Odpojení DPMRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="d25de-288">Dismount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a><span data-ttu-id="d25de-289">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d25de-289">Next steps</span></span>

<span data-ttu-id="d25de-290">Zjistěte, jak tooprepare serveru nebo začít chránit zatížení:</span><span class="sxs-lookup"><span data-stu-id="d25de-290">Learn how tooprepare your server or begin protecting a workload:</span></span>
- [<span data-ttu-id="d25de-291">Příprava úlohy zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="d25de-291">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="d25de-292">Pomocí zálohování serveru tooback server VMware</span><span class="sxs-lookup"><span data-stu-id="d25de-292">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="d25de-293">Použít tooback zálohování serveru SQL Server</span><span class="sxs-lookup"><span data-stu-id="d25de-293">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="d25de-294">Moderní úložiště zálohování pomocí zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="d25de-294">Use Modern Backup Storage with Backup Server</span></span>](backup-mabs-add-storage.md)

