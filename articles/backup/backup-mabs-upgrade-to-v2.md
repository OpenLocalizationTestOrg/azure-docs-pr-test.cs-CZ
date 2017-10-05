---
title: Nainstalujte Azure Backup Server v2 | Microsoft Docs
description: "Azure v2 zálohování serveru poskytuje vylepšené možnosti zálohování pro ochranu virtuálních počítačů, soubory a složky, úlohy a další. Informace o instalaci nebo upgradu na Azure Backup Server v2."
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
ms.openlocfilehash: 1bbb16afef7940933b4c3ae23873f212770137e0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="install-azure-backup-server-v2"></a><span data-ttu-id="b675e-104">Nainstalujte Azure Backup Server v2</span><span class="sxs-lookup"><span data-stu-id="b675e-104">Install Azure Backup Server v2</span></span>

<span data-ttu-id="b675e-105">Azure Backup Server pomáhá chránit vaše virtuální počítače (VM), úlohy, soubory a složky a další.</span><span class="sxs-lookup"><span data-stu-id="b675e-105">Azure Backup Server helps protect your virtual machines (VMs), workloads, files and folders, and more.</span></span> <span data-ttu-id="b675e-106">Azure Backup Server v2 staví na serveru Azure Backup v1 a poskytuje nové funkce, které nejsou k dispozici v v1.</span><span class="sxs-lookup"><span data-stu-id="b675e-106">Azure Backup Server v2 builds on Azure Backup Server v1, and gives you new features that are not available in v1.</span></span> <span data-ttu-id="b675e-107">Porovnání funkcí mezi v1 a v2 najdete v tématu [matice ochrany serveru Azure Backup](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="b675e-107">For a comparison of features between v1 and v2, see [Azure Backup Server protection matrix](backup-mabs-protection-matrix.md).</span></span> 

<span data-ttu-id="b675e-108">Další funkce v zálohování serveru v2 jsou upgrade z verze 1 zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="b675e-108">The additional features in Backup Server v2 are an upgrade from Backup Server v1.</span></span> <span data-ttu-id="b675e-109">V1 zálohování serveru ale není předpoklady pro instalaci v2 zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="b675e-109">However, Backup Server v1 is not a prerequisite for installing Backup Server v2.</span></span> <span data-ttu-id="b675e-110">Pokud chcete upgradovat z v1 zálohování serveru k zálohování serveru v2, nainstalujte na server ochrany zálohovat Server v2 zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="b675e-110">If you want to upgrade from Backup Server v1 to Backup Server v2, install Backup Server v2 on the Backup Server protection server.</span></span> <span data-ttu-id="b675e-111">Existující nastavení zálohování serveru zůstanou beze změn.</span><span class="sxs-lookup"><span data-stu-id="b675e-111">Your existing Backup Server settings remain intact.</span></span>

<span data-ttu-id="b675e-112">Zálohování serveru v2 můžete nainstalovat na Windows Server 2012 R2 nebo Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="b675e-112">You can install Backup Server v2 on Windows Server 2012 R2 or Windows Server 2016.</span></span> <span data-ttu-id="b675e-113">Abyste mohli využívat nové funkce, například System Center 2016 Data Protection Manager moderních úložiště záloh, je nutné nainstalovat v2 zálohování serveru v systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="b675e-113">To take advantage of new features like System Center 2016 Data Protection Manager Modern Backup Storage, you must install Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="b675e-114">Před upgradovat nebo instalací v2 Backup Server, přečtěte si informace o [požadavky pro instalaci](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="b675e-114">Before you upgrade to or install Backup Server v2, read about the [installation prerequisites](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="b675e-115">Azure Backup Server má stejný kód základní jako System Center Data Protection Manager.</span><span class="sxs-lookup"><span data-stu-id="b675e-115">Azure Backup Server has the same code base as System Center Data Protection Manager.</span></span> <span data-ttu-id="b675e-116">Zálohování serveru v1 je ekvivalentní k Data Protection Manager 2012 R2 a v2 zálohování serveru je ekvivalentní k Data Protection Manager 2016.</span><span class="sxs-lookup"><span data-stu-id="b675e-116">Backup Server v1 is equivalent to Data Protection Manager 2012 R2, and Backup Server v2 is equivalent to Data Protection Manager 2016.</span></span> <span data-ttu-id="b675e-117">Tento článek příležitostně odkazuje dokumentace k Data Protection Manager.</span><span class="sxs-lookup"><span data-stu-id="b675e-117">This article occasionally references the Data Protection Manager documentation.</span></span>
>
>

## <a name="upgrade-backup-server-to-v2"></a><span data-ttu-id="b675e-118">Upgrade záložní Server pro v2</span><span class="sxs-lookup"><span data-stu-id="b675e-118">Upgrade Backup Server to v2</span></span>
<span data-ttu-id="b675e-119">K upgradu na Backup Server v2 zálohovat Server v1, zkontrolujte, zda že má instalace požadovaných aktualizací:</span><span class="sxs-lookup"><span data-stu-id="b675e-119">To upgrade from Backup Server v1 to Backup Server v2, make sure your installation has the required updates:</span></span>

- <span data-ttu-id="b675e-120">[Aktualizovat agenty ochrany](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) na chráněných serverech.</span><span class="sxs-lookup"><span data-stu-id="b675e-120">[Update the protection agents](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) on the protected servers.</span></span>
- <span data-ttu-id="b675e-121">Upgrade systému Windows Server 2012 R2 na Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="b675e-121">Upgrade Windows Server 2012 R2 to Windows Server 2016.</span></span>
- <span data-ttu-id="b675e-122">Správce vzdáleného serveru Azure Backup upgradujte na všech provozních serverech.</span><span class="sxs-lookup"><span data-stu-id="b675e-122">Upgrade Azure Backup Server Remote Administrator on all production servers.</span></span>
- <span data-ttu-id="b675e-123">Ujistěte se, že jsou zálohy nastaveny na pokračovat bez restartování provozním serveru.</span><span class="sxs-lookup"><span data-stu-id="b675e-123">Ensure that backups are set to continue without restarting your production server.</span></span>


### <a name="upgrade-steps-for-backup-server-v2"></a><span data-ttu-id="b675e-124">Postup upgradu serveru zálohování v2</span><span class="sxs-lookup"><span data-stu-id="b675e-124">Upgrade steps for Backup Server v2</span></span>

1. <span data-ttu-id="b675e-125">V centru stažení [stažení aktualizace instalačního programu](https://go.microsoft.com/fwlink/?LinkId=626082).</span><span class="sxs-lookup"><span data-stu-id="b675e-125">In the Download Center, [download the upgrade installer](https://go.microsoft.com/fwlink/?LinkId=626082).</span></span>

2. <span data-ttu-id="b675e-126">Po extrahování Průvodce instalací, ujistěte se, že **spuštění setup.exe** vybrané, a pak vyberte možnost **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b675e-126">After you extract the setup wizard, make sure that **Execute setup.exe** is selected, and then select **Finish**.</span></span>

  ![Instalační program instalaci - spustit instalační program](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. <span data-ttu-id="b675e-128">V průvodci Microsoft Azure Backup Server v části **nainstalovat**, vyberte **Microsoft Azure Backup Server**.</span><span class="sxs-lookup"><span data-stu-id="b675e-128">In the Microsoft Azure Backup Server wizard, under **Install**, select **Microsoft Azure Backup Server**.</span></span>

  ![Instalační program instalaci - vyberte možnost instalace](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. <span data-ttu-id="b675e-130">Na **úvodní** , přečtěte si upozornění a potom vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-130">On the **Welcome** page, review the warnings, and then select **Next**.</span></span>

  ![Instalační program instalaci – úvodní stránka](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. <span data-ttu-id="b675e-132">Průvodce instalací provádí kontroly předpokladů, abyste měli jistotu, že můžete upgradovat prostředí.</span><span class="sxs-lookup"><span data-stu-id="b675e-132">The setup wizard performs prerequisite checks to make sure your environment can upgrade.</span></span> <span data-ttu-id="b675e-133">Na **požadovaných součástí kontroluje** vyberte **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="b675e-133">On the **Prerequisite Checks** page, select **Check**.</span></span>

  ![Instalační program instalaci - požadovaných součástí kontroluje stránky](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. <span data-ttu-id="b675e-135">Prostředí musí projít kontrol požadovaných součástí.</span><span class="sxs-lookup"><span data-stu-id="b675e-135">Your environment must pass the prerequisite checks.</span></span> <span data-ttu-id="b675e-136">Pokud vaše prostředí neprojde kontrol, Všimněte si, problémy a opravit je.</span><span class="sxs-lookup"><span data-stu-id="b675e-136">If your environment doesn't pass the checks, note the issues and fix them.</span></span> <span data-ttu-id="b675e-137">Pak vyberte **zkontrolujte znovu**.</span><span class="sxs-lookup"><span data-stu-id="b675e-137">Then, select **Check Again**.</span></span> <span data-ttu-id="b675e-138">Když předáte kontroly předpokladů, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-138">After you pass the prerequisite checks, select **Next**.</span></span>

  ![Instalační program instalace - zkontrolujte znovu tlačítko](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. <span data-ttu-id="b675e-140">Na **nastavení SQL** vyberte příslušnou volbu pro instalaci SQL a potom vyberte **zkontrolovat a nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="b675e-140">On the **SQL Settings** page, select the relevant option for your SQL installation, and then select **Check and Install**.</span></span>

  ![Instalační program instalaci - stránka nastavení SQL](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  <span data-ttu-id="b675e-142">Kontroly může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="b675e-142">The checks might take a few minutes.</span></span> <span data-ttu-id="b675e-143">Po dokončení kontroly vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-143">When the checks are finished, select **Next**.</span></span>

  ![Instalační program instalace - zkontrolujte nastavení SQL a tlačítko Instalovat](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. <span data-ttu-id="b675e-145">Na **nastavení instalace** stránky, proveďte požadované změny umístění, kde je nainstalován Backup Server, nebo cesta k pomocnému umístění.</span><span class="sxs-lookup"><span data-stu-id="b675e-145">On the **Installation Settings** page, make any changes to the location where Backup Server is installed, or to the Scratch Location.</span></span> <span data-ttu-id="b675e-146">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-146">Select **Next**.</span></span>

  ![Instalační program instalaci - stránka nastavení instalace](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. <span data-ttu-id="b675e-148">Chcete-li dokončit Průvodce instalací, vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b675e-148">To finish the setup wizard, select **Finish**.</span></span>

  ![Instalační program instalaci - dokončit](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a><span data-ttu-id="b675e-150">Přidání úložiště pro moderní úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="b675e-150">Add storage for Modern Backup Storage</span></span>

<span data-ttu-id="b675e-151">Pokud chcete zlepšit efektivitu úložiště záloh, v2 zálohování serveru přidává podporu pro svazky.</span><span class="sxs-lookup"><span data-stu-id="b675e-151">To improve backup storage efficiency, Backup Server v2 adds support for volumes.</span></span> <span data-ttu-id="b675e-152">Podobně jako Backup Server v1 v2 zálohování serveru podporuje disky.</span><span class="sxs-lookup"><span data-stu-id="b675e-152">Like Backup Server v1, Backup Server v2 supports disks.</span></span>

### <a name="add-volumes-and-disks"></a><span data-ttu-id="b675e-153">Přidat svazky a disků</span><span class="sxs-lookup"><span data-stu-id="b675e-153">Add volumes and disks</span></span>
<span data-ttu-id="b675e-154">Pokud spustíte v2 zálohování serveru v systému Windows Server 2016, můžete k uložení zálohy dat svazky.</span><span class="sxs-lookup"><span data-stu-id="b675e-154">If you run Backup Server v2 on Windows Server 2016, you can use volumes to store backup data.</span></span> <span data-ttu-id="b675e-155">Svazky nabízejí úspory úložiště a rychlejší zálohování.</span><span class="sxs-lookup"><span data-stu-id="b675e-155">Volumes offer storage savings and faster backups.</span></span> <span data-ttu-id="b675e-156">Protože svazky jsou nové zálohování serveru, musíte je přidat.</span><span class="sxs-lookup"><span data-stu-id="b675e-156">Because volumes are new to Backup Server, you must add them.</span></span> 

<span data-ttu-id="b675e-157">Když přidáte Server zálohování svazku, můžete přiřadit svazek popisný název.</span><span class="sxs-lookup"><span data-stu-id="b675e-157">When you add a volume to Backup Server, you can give the volume a friendly name.</span></span> <span data-ttu-id="b675e-158">Klikněte **popisný název** sloupec chcete název svazku.</span><span class="sxs-lookup"><span data-stu-id="b675e-158">Click the **Friendly Name** column of the volume you want to name.</span></span> <span data-ttu-id="b675e-159">V případě potřeby můžete později změnit název.</span><span class="sxs-lookup"><span data-stu-id="b675e-159">You can change the name later, if necessary.</span></span> <span data-ttu-id="b675e-160">Také můžete použít PowerShell k přidání nebo změna popisné názvy pro svazky.</span><span class="sxs-lookup"><span data-stu-id="b675e-160">You also can use PowerShell to add or change friendly names for volumes.</span></span>

<span data-ttu-id="b675e-161">Postup přidání svazku v konzole pro správu:</span><span class="sxs-lookup"><span data-stu-id="b675e-161">To add a volume in the Administrator Console:</span></span>

1. <span data-ttu-id="b675e-162">V konzole správce serveru zálohování Azure vyberte **správy** > **diskového úložiště** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b675e-162">In the Azure Backup Server Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Otevřete Průvodce přidáním úložiště disku](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    <span data-ttu-id="b675e-164">Otevře se Průvodce přidáním úložiště disku.</span><span class="sxs-lookup"><span data-stu-id="b675e-164">This opens the Add Disk Storage wizard.</span></span>

2. <span data-ttu-id="b675e-165">Na **přidání disku úložiště** stránky v **dostupných svazků** , vyberte svazek a pak vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b675e-165">On the **Add Disk Storage** page, in the **Available volumes** box, select a volume, and then select **Add**.</span></span>
3. <span data-ttu-id="b675e-166">V **vybrané svazky** pole, zadejte popisný název pro svazek a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="b675e-166">In the **Selected volumes** box, enter a friendly name for the volume, and then select **OK**.</span></span>

      ![Průvodce přidáním úložiště disku – Přidání svazku](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  <span data-ttu-id="b675e-168">Pokud chcete přidat disk, disk musí patřit do skupiny ochrany, která má starší verze úložiště.</span><span class="sxs-lookup"><span data-stu-id="b675e-168">If you want to add a disk, the disk must belong to a protection group that has legacy storage.</span></span> <span data-ttu-id="b675e-169">Tyto disky použít jenom pro tyto skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-169">These disks can only be used for these protection groups.</span></span> <span data-ttu-id="b675e-170">Pokud Backup Server nemá zdroje, které mají další ochranu starší verze, se neuvádějí disku.</span><span class="sxs-lookup"><span data-stu-id="b675e-170">If Backup Server doesn't have sources that have legacy protection, the disk isn't listed.</span></span>

  <span data-ttu-id="b675e-171">Další informace o přidávání disků, najdete v tématu [přidávání disků, pokud chcete zvýšit starší verze úložiště](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span><span class="sxs-lookup"><span data-stu-id="b675e-171">For more information about adding disks, see [Adding disks to increase legacy storage](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span></span> <span data-ttu-id="b675e-172">Disk nelze udělit popisný název.</span><span class="sxs-lookup"><span data-stu-id="b675e-172">You can't give a disk a friendly name.</span></span>


### <a name="assign-workloads-to-volumes"></a><span data-ttu-id="b675e-173">Přiřadit svazky úlohy</span><span class="sxs-lookup"><span data-stu-id="b675e-173">Assign workloads to volumes</span></span>

<span data-ttu-id="b675e-174">Zálohování serveru zadejte, které úlohy jsou přiřazeny k svazků, které.</span><span class="sxs-lookup"><span data-stu-id="b675e-174">In Backup Server, you specify which workloads are assigned to which volumes.</span></span> <span data-ttu-id="b675e-175">Například můžete nastavit nákladné svazky, které podporují vysoký počet vstupně výstupních operací za sekundu (IOPS) pro uložení pouze úlohy, které vyžadují časté, vysoký počet záloh.</span><span class="sxs-lookup"><span data-stu-id="b675e-175">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) to store only workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="b675e-176">Příkladem je SQL Server s protokoly transakcí.</span><span class="sxs-lookup"><span data-stu-id="b675e-176">An example is SQL Server with transaction logs.</span></span>

#### <a name="update-dpmdiskstorage"></a><span data-ttu-id="b675e-177">Aktualizace DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="b675e-177">Update-DPMDiskStorage</span></span>

<span data-ttu-id="b675e-178">Pokud chcete aktualizovat vlastnosti svazku ve fondu úložiště zálohování serveru, použijte rutinu prostředí PowerShell DPMDiskStorage aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b675e-178">To update the properties of a volume in the storage pool in Backup Server, use the PowerShell cmdlet Update-DPMDiskStorage.</span></span>

<span data-ttu-id="b675e-179">Syntaxe:</span><span class="sxs-lookup"><span data-stu-id="b675e-179">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

<span data-ttu-id="b675e-180">V uživatelském rozhraní se projeví všechny změny, které můžete provést pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b675e-180">All changes that you make by using PowerShell are reflected in the UI.</span></span>


## <a name="protect-data-sources"></a><span data-ttu-id="b675e-181">Chránit zdroje dat</span><span class="sxs-lookup"><span data-stu-id="b675e-181">Protect data sources</span></span>
<span data-ttu-id="b675e-182">Chcete-li začít chránit zdroje dat, vytvořte skupinu ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-182">To begin protecting data sources, create a protection group.</span></span> <span data-ttu-id="b675e-183">Následující kroky zvýrazněte změny nebo přidání do Průvodce vytvořením nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-183">The following steps highlight changes or additions to the New Protection Group wizard.</span></span>

<span data-ttu-id="b675e-184">Chcete-li vytvořit skupinu ochrany:</span><span class="sxs-lookup"><span data-stu-id="b675e-184">To create a protection group:</span></span>

1. <span data-ttu-id="b675e-185">V konzole správce zálohování serveru vyberte **ochrany**.</span><span class="sxs-lookup"><span data-stu-id="b675e-185">In the Backup Server Administrator Console, select **Protection**.</span></span>

2. <span data-ttu-id="b675e-186">Na pásu karet vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="b675e-186">On the tool ribbon, select **New**.</span></span>

    <span data-ttu-id="b675e-187">Otevře se Průvodce vytvořením nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-187">This opens the Create New Protection Group wizard.</span></span>

  ![Průvodce vytvořením nové skupiny ochrany](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. <span data-ttu-id="b675e-189">Na **úvodní** vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-189">On the **Welcome** page, select **Next**.</span></span>
4. <span data-ttu-id="b675e-190">Na **vybrat typ skupiny ochrany** vyberte typ skupiny ochrany, kterou chcete vytvořit a potom vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-190">On the **Select Protection Group Type** page, select the type of protection group you want to create, and then select **Next**.</span></span>

  ![Stránka Typ vyberte skupinu ochrany](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. <span data-ttu-id="b675e-192">Na **vybrat členy skupiny** stránky v **Dostupní členové** podokně, členy s jsou uvedeny agenty ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-192">On the **Select Group Members** page, in the **Available members** pane, the members with protection agents are listed.</span></span> <span data-ttu-id="b675e-193">V tomto příkladu vyberte svazek D:\ a E:\ a přidat je do **vybrané členy** podokně.</span><span class="sxs-lookup"><span data-stu-id="b675e-193">For this example, select volume D:\ and E:\ and add them to the **Selected members** pane.</span></span> <span data-ttu-id="b675e-194">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-194">Select **Next**.</span></span>

  ![Vyberte skupiny členy stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. <span data-ttu-id="b675e-196">Na **vyberte způsob ochrany dat** stránky, zadejte **název skupiny ochrany**, zvolte metodu ochrany a pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-196">On the **Select Data Protection Method** page, enter a **Protection group name**, select the protection method, and then select **Next**.</span></span> <span data-ttu-id="b675e-197">Pokud chcete krátkodobou ochranu, je nutné vybrat **disku** zálohování metoda.</span><span class="sxs-lookup"><span data-stu-id="b675e-197">If you want short-term protection, you must select the **Disk** backup method.</span></span>

  ![Vyberte způsob ochrany dat stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. <span data-ttu-id="b675e-199">Na **zadat krátkodobé cíle** vyberte podrobnosti **rozsah uchování** a **četnost synchronizace**.</span><span class="sxs-lookup"><span data-stu-id="b675e-199">On the **Specify Short-Term Goals** page, select the details for **Retention range** and **Synchronization frequency**.</span></span> <span data-ttu-id="b675e-200">Pak vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="b675e-200">Then, select **Next**.</span></span> <span data-ttu-id="b675e-201">Pokud chcete změnit plán, kdy jsou pořizovány body obnovení, vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="b675e-201">Optionally, to change the schedule for when recovery points are taken, select **Modify**.</span></span>

  ![Určení krátkodobých cílů stránky](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. <span data-ttu-id="b675e-203">Na **zkontrolovat přidělení diskového úložiště** stránka, zkontrolujte podrobnosti o zdroji dat jste vybrali, jejich velikosti a hodnoty pro místo, na které se má zřídit a cílový svazek úložiště.</span><span class="sxs-lookup"><span data-stu-id="b675e-203">On the **Review Disk Storage Allocation** page, review details about the data sources you selected, their size, and  values for the space to be provisioned and the target storage volume.</span></span>

  ![Stránka Kontrola přidělení diskového úložiště](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  <span data-ttu-id="b675e-205">Svazky úložiště jsou založené na svazku přidělení zatížení (nastavte pomocí prostředí PowerShell) a úložiště k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b675e-205">Storage volumes are based on the workload volume allocation (set by using PowerShell) and the available storage.</span></span> <span data-ttu-id="b675e-206">Svazky úložiště můžete změnit výběrem jiných svazků v rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="b675e-206">You can change the storage volumes by selecting other volumes in the drop-down menu.</span></span> <span data-ttu-id="b675e-207">Pokud změníte hodnotu **úložiště v cíli**, hodnota **úložiště disku** dynamicky se změní podle hodnoty v části **volného místa** a  **Underprovisioned místo**.</span><span class="sxs-lookup"><span data-stu-id="b675e-207">If you change the value for **Target Storage**, the value for **Available disk storage** dynamically changes to reflect values under **Free Space** and **Underprovisioned Space**.</span></span>

  <span data-ttu-id="b675e-208">Pokud zdroje dat růst podle plánu, hodnota **Underprovisioned místo** sloupec v **úložiště disku** odráží množství další úložiště, které je potřeba.</span><span class="sxs-lookup"><span data-stu-id="b675e-208">If the data sources grow as planned, the value for the **Underprovisioned Space** column in **Available disk storage** reflects the amount of additional storage that's needed.</span></span> <span data-ttu-id="b675e-209">Tato hodnota se používá k pomohou naplánovat požadavky na ukládání smooth zálohování.</span><span class="sxs-lookup"><span data-stu-id="b675e-209">Use this value to help plan your storage needs for smooth backups.</span></span> <span data-ttu-id="b675e-210">Pokud je hodnota nula, neexistují v blízké budoucnosti potenciální problémy s úložištěm.</span><span class="sxs-lookup"><span data-stu-id="b675e-210">If the value is zero, there are no potential problems with storage in the foreseeable future.</span></span> <span data-ttu-id="b675e-211">Pokud je hodnota číslo větší než nula, nemáte dostatečné úložiště přidělené (podle zásady ochrany vaší a velikost dat chráněných členů).</span><span class="sxs-lookup"><span data-stu-id="b675e-211">If the value is a number other than zero, you do not have sufficient storage allocated  (based on your protection policy and the data size of your protected members).</span></span>

  ![Nevytížených diskového úložiště](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   <span data-ttu-id="b675e-213">Dokončete vytváření skupinu ochrany, dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="b675e-213">To finish creating your protection group, complete the wizard.</span></span>

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a><span data-ttu-id="b675e-214">Migrovat starší verze úložiště do moderní úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="b675e-214">Migrate legacy storage to Modern Backup Storage</span></span>
<span data-ttu-id="b675e-215">Po upgradu na Backup Server v2 a upgradovat operační systém na Windows Server 2016, aktualizujte skupin ochrany používat moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="b675e-215">After you upgrade to or install Backup Server v2 and upgrade the operating system to Windows Server 2016, update your protection groups to use Modern Backup Storage.</span></span> <span data-ttu-id="b675e-216">Ve výchozím nastavení se nezmění skupin ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-216">By default, protection groups are not changed.</span></span> <span data-ttu-id="b675e-217">Budou i nadále fungovat jako původně určené.</span><span class="sxs-lookup"><span data-stu-id="b675e-217">They continue to function as they were initially set up.</span></span> 

<span data-ttu-id="b675e-218">Aktualizace skupiny ochrany a používat moderní úložiště zálohy je volitelné.</span><span class="sxs-lookup"><span data-stu-id="b675e-218">Updating protection groups to use Modern Backup Storage is optional.</span></span> <span data-ttu-id="b675e-219">Aktualizace skupiny ochrany, ukončete ochranu všech zdrojů dat pomocí možnosti zachovat data.</span><span class="sxs-lookup"><span data-stu-id="b675e-219">To update the protection group, stop protection of all data sources by using the retain data option.</span></span> <span data-ttu-id="b675e-220">Pak přidejte zdroje dat do nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-220">Then, add the data sources to a new protection group.</span></span>

1. <span data-ttu-id="b675e-221">V konzole pro správu, vyberte **ochrany** funkce.</span><span class="sxs-lookup"><span data-stu-id="b675e-221">In the Administrator Console, select the **Protection** feature.</span></span> <span data-ttu-id="b675e-222">V **člena skupiny ochrany** seznamu, klikněte pravým tlačítkem na člen a pak vyberte **zastavit ochranu člena**.</span><span class="sxs-lookup"><span data-stu-id="b675e-222">In the **Protection Group Member** list, right-click the member, and then select **Stop protection of member**.</span></span>

  ![Zastavit ochranu člena](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. <span data-ttu-id="b675e-224">V **odebrat ze skupiny** dialogové okno pole, zkontrolujte použité místo na disku a dostupné volné místo pro fond úložiště.</span><span class="sxs-lookup"><span data-stu-id="b675e-224">In the **Remove from Group** dialog box, review the used disk space and the available free space for the storage pool.</span></span> <span data-ttu-id="b675e-225">Ve výchozím nastavení se nechte body obnovení na disku a povolení jejich platnost vyprší za jejich zásady uchovávání informací přidružených.</span><span class="sxs-lookup"><span data-stu-id="b675e-225">The default is to leave the recovery points on the disk and allow them to expire per their associated retention policy.</span></span> <span data-ttu-id="b675e-226">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="b675e-226">Click **OK**.</span></span>

  <span data-ttu-id="b675e-227">Pokud chcete hned vrátí do fondu úložiště volné použité místo na disku, vyberte **odstranit repliku na disku** políčko Odstranit záložní data (a bodů obnovení) přidružené k tohoto člena.</span><span class="sxs-lookup"><span data-stu-id="b675e-227">If you want to immediately return the used disk space to the free storage pool, select the **Delete replica on disk** check box to delete the backup data (and recovery points) associated with that member.</span></span>

  ![Odebrat ze skupiny, dialogové okno](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. <span data-ttu-id="b675e-229">Vytvořte skupinu ochrany, která používá moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="b675e-229">Create a protection group that uses Modern Backup Storage.</span></span> <span data-ttu-id="b675e-230">Zahrnout nechráněné datových zdrojů.</span><span class="sxs-lookup"><span data-stu-id="b675e-230">Include the unprotected data sources.</span></span>


## <a name="add-disks-to-increase-legacy-storage"></a><span data-ttu-id="b675e-231">Přidat disky, které chcete zvýšit velikost úložiště starší verze</span><span class="sxs-lookup"><span data-stu-id="b675e-231">Add disks to increase legacy storage</span></span>

<span data-ttu-id="b675e-232">Pokud chcete používat starší verze úložiště s zálohování serveru, vám může být nutné přidat disky, které chcete zvýšit starší verze úložiště.</span><span class="sxs-lookup"><span data-stu-id="b675e-232">If you want to use legacy storage with Backup Server, you might need to add disks to increase legacy storage.</span></span> 

<span data-ttu-id="b675e-233">Přidání disku úložiště:</span><span class="sxs-lookup"><span data-stu-id="b675e-233">To add disk storage:</span></span>

1. <span data-ttu-id="b675e-234">V konzole pro správu vyberte **správy** > **diskového úložiště** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b675e-234">In the Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Přidat dialogové okno diskového úložiště](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. <span data-ttu-id="b675e-236">V **přidání disku úložiště** dialogovém okně, vyberte **přidat disky**.</span><span class="sxs-lookup"><span data-stu-id="b675e-236">In the **Add Disk Storage** dialog, select **Add disks**.</span></span>

5. <span data-ttu-id="b675e-237">Vyberte disky, které chcete přidat, vyberte v seznamu dostupných disků, **přidat**a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="b675e-237">In the list of available disks, select the disks you want to add, select **Add**, and then select **OK**.</span></span>

## <a name="update-the-data-protection-manager-protection-agent"></a><span data-ttu-id="b675e-238">Aktualizujte agenta ochrany aplikace Data Protection Manager</span><span class="sxs-lookup"><span data-stu-id="b675e-238">Update the Data Protection Manager protection agent</span></span>

<span data-ttu-id="b675e-239">Zálohování serveru System Center Data Protection Manager protection agent používá pro aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b675e-239">Backup Server uses the System Center Data Protection Manager protection agent for updates.</span></span> <span data-ttu-id="b675e-240">Pokud provádíte upgrade agenta ochrany, který není připojen k síti, nelze použít konzolu nástroje Data Protection Manager Administrator k dokončení upgradu připojeného agenta.</span><span class="sxs-lookup"><span data-stu-id="b675e-240">If you are upgrading a protection agent that is not connected to the network, you cannot use the Data Protection Manager Administrator Console to complete a connected agent upgrade.</span></span> <span data-ttu-id="b675e-241">Je nutné upgradovat agenta ochrany v prostředí neaktivní domény.</span><span class="sxs-lookup"><span data-stu-id="b675e-241">You must upgrade the protection agent in a nonactive domain environment.</span></span> <span data-ttu-id="b675e-242">Dokud klientský počítač je připojený k síti, konzolu nástroje Data Protection Manager Administrator ukazuje, že aktualizace agenta ochrany čeká na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="b675e-242">Until the client computer is connected to the network, the Data Protection Manager Administrator Console shows that the protection agent update is pending.</span></span>

<span data-ttu-id="b675e-243">Následující části popisují, jak aktualizovat agenty ochrany pro klientské počítače, které jsou připojené a klientské počítače, které nejsou připojené.</span><span class="sxs-lookup"><span data-stu-id="b675e-243">The following sections describe how to update protection agents for client computers that are connected and client computers that are not connected.</span></span>

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a><span data-ttu-id="b675e-244">Aktualizace agenta ochrany pro připojené klientské počítače</span><span class="sxs-lookup"><span data-stu-id="b675e-244">Update a protection agent for a connected client computer</span></span>

1. <span data-ttu-id="b675e-245">V konzole správce zálohování serveru vyberte **správy** > **agenti**.</span><span class="sxs-lookup"><span data-stu-id="b675e-245">In the Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="b675e-246">V podokně zobrazení vyberte klientské počítače, pro které chcete aktualizovat agenta ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-246">In the display pane, select the client computers for which you want to update the protection agent.</span></span>

  > [!NOTE]
  > <span data-ttu-id="b675e-247">**Aktualizací agenta** sloupec zobrazuje, když je k dispozici pro každý chráněný počítač aktualizace agenta ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-247">The **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="b675e-248">V **akce** podokně **aktualizace** akce je dostupná, pouze když je vybrán chráněný počítač a jsou k dispozici aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b675e-248">In the **Actions** pane, the **Update** action is available only when a protected computer is selected and updates are available.</span></span>
  >
  >

3. <span data-ttu-id="b675e-249">Chcete-li nainstalovat aktualizované agenty ochrany na vybraných počítačích, v **akce** podokně, vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="b675e-249">To install updated protection agents on the selected computers, in the **Actions** pane, select **Update**.</span></span>

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a><span data-ttu-id="b675e-250">Aktualizace agenta ochrany na klientském počítači, který není připojen</span><span class="sxs-lookup"><span data-stu-id="b675e-250">Update a protection agent on a client computer that is not connected</span></span>

1. <span data-ttu-id="b675e-251">V konzole správce zálohování serveru vyberte **správy** > **agenti**.</span><span class="sxs-lookup"><span data-stu-id="b675e-251">In the Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="b675e-252">V podokně zobrazení vyberte klientské počítače, pro které chcete aktualizovat agenta ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-252">In the display pane, select the client computers for which you want to update the protection agent.</span></span>

  > [!NOTE]
   > <span data-ttu-id="b675e-253">**Aktualizací agenta** sloupec zobrazuje, když je k dispozici pro každý chráněný počítač aktualizace agenta ochrany.</span><span class="sxs-lookup"><span data-stu-id="b675e-253">The **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="b675e-254">V **akce** podokně **aktualizace** akce není dostupná, když je vybrán chráněný počítač, pokud jsou k dispozici aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b675e-254">In the **Actions** pane, the **Update** action is not available when a protected computer is selected unless updates are available.</span></span>
  >
  >

3. <span data-ttu-id="b675e-255">Chcete-li nainstalovat aktualizované agenty ochrany na vybraných počítačích, vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="b675e-255">To install updated protection agents on the selected computers, select **Update**.</span></span>

4. <span data-ttu-id="b675e-256">Pro klientské počítače, který není připojen k síti, dokud je počítač připojen k síti **stav agenta** sloupci se zobrazuje stav **aktualizace čeká na vyřízení**.</span><span class="sxs-lookup"><span data-stu-id="b675e-256">For a client computer that is not connected to the network, until the computer is connected to the network, the **Agent Status** column shows a status of **Update Pending**.</span></span>

  <span data-ttu-id="b675e-257">Až se klientský počítač připojí k síti, **aktualizací agenta** sloupci pro klientský počítač se zobrazuje stav **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="b675e-257">After a client computer is connected to the network, the **Agent Updates** column for the client computer shows a status of **Updating**.</span></span>
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-the-new-version-with-azure"></a><span data-ttu-id="b675e-258">Přesunout z původní verze starší verze skupin ochrany a synchronizovat s Azure na novou verzi</span><span class="sxs-lookup"><span data-stu-id="b675e-258">Move legacy Protection groups from old version and sync the new version with Azure</span></span>

<span data-ttu-id="b675e-259">Jakmile serveru Azure Backup a operačního systému jsou obě aktualizovány, jste připraveni k ochraně nové zdroje dat pomocí moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="b675e-259">Once Azure Backup Server and the OS are both updated, you are ready to protect new data sources using Modern Backup Storage.</span></span> <span data-ttu-id="b675e-260">Ale již chráněných zdrojů dat nadále chráněné starší verze způsobem, jako byly v serveru Azure Backup, ale všechny nové ochrany použije moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="b675e-260">However already protected data sources will continue to be protected in the legacy way as they were in Azure Backup Server but all new protection will use Modern Backup Storage.</span></span>

<span data-ttu-id="b675e-261">Následující kroky jsou při migraci zdroje dat z režimu starší verze ochrany do moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="b675e-261">Below steps are to migrate data sources from legacy  mode of protection to Modern backup storage.</span></span>

<span data-ttu-id="b675e-262">• Přidat nové svazky do fondu úložiště aplikace DPM a přiřadit popisné názvy a data zdroj značky v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b675e-262">• Add the new volume(s) to the DPM storage pool and assign friendly names and data source tags if desired.</span></span>
<span data-ttu-id="b675e-263">• Pro každý zdroj dat, který je v režimu starší verze, zastavte ochranu zdroje dat a "Uchovat chráněná Data".</span><span class="sxs-lookup"><span data-stu-id="b675e-263">• For each data source that is in legacy mode, stop protection of the data sources and “Retain Protected Data”.</span></span>  <span data-ttu-id="b675e-264">To vám umožní obnovení starých bodů obnovení po migraci.</span><span class="sxs-lookup"><span data-stu-id="b675e-264">This will allow recovery of old recovery points after migration.</span></span>

<span data-ttu-id="b675e-265">• Vytvořte nové PG a vyberte zdroje dat, které mají být uloženy pomocí nový formát.</span><span class="sxs-lookup"><span data-stu-id="b675e-265">• Create a new PG and select the data sources that are to be stored using new format.</span></span>
<span data-ttu-id="b675e-266">• Aplikace DPM provede kopie repliky ze starší verze zálohování úložiště do úložiště moderní zálohování svazku místně.</span><span class="sxs-lookup"><span data-stu-id="b675e-266">• DPM will do a replica copy from the legacy backup storage into the Modern Backup Storage volume locally.</span></span>
<span data-ttu-id="b675e-267">Poznámka: Toto se zobrazí jako • operaci po obnovení úlohy, které všechny nové synchronizace a bodů obnovení bude uložen v moderní úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="b675e-267">Note: This will be seen as a post-recovery operation job • All new sync and recovery points will then be stored in Modern Backup Storage.</span></span>
<span data-ttu-id="b675e-268">• Staré body obnovení se vyřazují, protože platnost a nakonec uvolněte místo na disku.</span><span class="sxs-lookup"><span data-stu-id="b675e-268">• Old recovery points will be pruned out as they expire and eventually free up the disk space.</span></span>
<span data-ttu-id="b675e-269">• Po všechny starší verze svazky jsou odstraněny z původní úložiště, disk může odebrat z Azure backup a v systému.</span><span class="sxs-lookup"><span data-stu-id="b675e-269">• Once all the legacy volumes are deleted from the old storage, the disk can be removed from Azure backup and the system.</span></span>
<span data-ttu-id="b675e-270">• Proveďte zálohu databáze dpmdb Azure.</span><span class="sxs-lookup"><span data-stu-id="b675e-270">• Take a backup of the  Azure DPMDB.</span></span>

<span data-ttu-id="b675e-271">Část 2:-důležité položky > Nový server se musí mít stejný název jako původní server Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="b675e-271">Part 2: -Important items> The new server will need to be named same as the original Azure Backup server.</span></span> <span data-ttu-id="b675e-272">Název nový server Azure backup nelze změnit, pokud chcete zachovat pomocí staré fondu úložiště a databáze aplikace DPMDB bodů obnovení - musí mít zálohu DPMDB ji budou muset obnovit</span><span class="sxs-lookup"><span data-stu-id="b675e-272">You cannot change the name of the new Azure backup server if you want to use old storage pool and DPMDB to retain recovery points -Must have backup of DPMDB  as it will need to be restored</span></span>

1) <span data-ttu-id="b675e-273">Vypnutí původní Azure zálohování serveru nebo trvat vypnout drátové síti.</span><span class="sxs-lookup"><span data-stu-id="b675e-273">Shutdown the original Azure backup server or take it off the wire.</span></span>
2) <span data-ttu-id="b675e-274">Resetovat účet počítače ve službě active directory.</span><span class="sxs-lookup"><span data-stu-id="b675e-274">Reset the machine account in active directory.</span></span>
3) <span data-ttu-id="b675e-275">Nainstalujte Server 2016 na nový počítač s názvem stejný název počítače jako původní server Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="b675e-275">Install Server 2016 on new machine and name it the same machine name as the original Azure Backup server.</span></span>
4) <span data-ttu-id="b675e-276">Připojení k doméně</span><span class="sxs-lookup"><span data-stu-id="b675e-276">Join the Domain</span></span>
5) <span data-ttu-id="b675e-277">Nainstalujte server Azure Backup V2 (disků fondu úložiště aplikace DPM přesunout z původní server a import)</span><span class="sxs-lookup"><span data-stu-id="b675e-277">Install Azure Backup server V2 (Move DPM Storage pool disks from old server and import)</span></span>
6) <span data-ttu-id="b675e-278">Obnovte DPMDB prováděné od konce část 2</span><span class="sxs-lookup"><span data-stu-id="b675e-278">Restore the DPMDB taken from end of part 2</span></span>
7) <span data-ttu-id="b675e-279">Připojení úložiště z původního zálohování serveru na nový server.</span><span class="sxs-lookup"><span data-stu-id="b675e-279">Attach the storage from the original backup server to the new server.</span></span>
8) <span data-ttu-id="b675e-280">Z obnovení SQL databáze DPMDB</span><span class="sxs-lookup"><span data-stu-id="b675e-280">From SQL Restore the DPMDB</span></span>
9) <span data-ttu-id="b675e-281">Z příkazového řádku správce na nový disk cd server Microsoft Azure Backup nainstalujte umístění a složky koše</span><span class="sxs-lookup"><span data-stu-id="b675e-281">From admin command line on new server cd to Microsoft Azure Backup install location and bin folder</span></span>

<span data-ttu-id="b675e-282">Příklad cesty: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span><span class="sxs-lookup"><span data-stu-id="b675e-282">Path example: C:\windows\system32>cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span></span>
<span data-ttu-id="b675e-283">zálohování do Azure, spusťte příkaz DPMSYNC-SYNC</span><span class="sxs-lookup"><span data-stu-id="b675e-283">to Azure backup Run DPMSYNC -SYNC</span></span>

10) <span data-ttu-id="b675e-284">Spusťte synchronizaci DPMSYNC-SYNC Poznámka: Pokud jste přidali nové disky do fondu úložiště DPM místo přesunutím staré, spusťte příkaz DPMSYNC - Reallocatereplica</span><span class="sxs-lookup"><span data-stu-id="b675e-284">Run DPMSYNC -SYNC Note If you have added NEW disks to the DPM Storage pool instead of moving the old ones, then run DPMSYNC -Reallocatereplica</span></span>

## <a name="new-powershell-cmdlets-in-v2"></a><span data-ttu-id="b675e-285">Nové rutiny prostředí PowerShell v v2</span><span class="sxs-lookup"><span data-stu-id="b675e-285">New PowerShell cmdlets in v2</span></span>

<span data-ttu-id="b675e-286">Při instalaci serveru Azure Backup v2 jsou k dispozici dvě nové rutiny:</span><span class="sxs-lookup"><span data-stu-id="b675e-286">When you install Azure Backup Server v2, two new cmdlets are available:</span></span> 
* [<span data-ttu-id="b675e-287">DPMRecoveryPoint připojení</span><span class="sxs-lookup"><span data-stu-id="b675e-287">Mount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787159.aspx)
* [<span data-ttu-id="b675e-288">Odpojení DPMRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="b675e-288">Dismount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a><span data-ttu-id="b675e-289">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b675e-289">Next steps</span></span>

<span data-ttu-id="b675e-290">Zjistěte, jak připravit server nebo začít chránit zatížení:</span><span class="sxs-lookup"><span data-stu-id="b675e-290">Learn how to prepare your server or begin protecting a workload:</span></span>
- [<span data-ttu-id="b675e-291">Příprava úlohy zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="b675e-291">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="b675e-292">Pomocí zálohování serveru zálohovat VMware server</span><span class="sxs-lookup"><span data-stu-id="b675e-292">Use Backup Server to back up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="b675e-293">Použít zálohování serveru k zálohování systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="b675e-293">Use Backup Server to back up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="b675e-294">Moderní úložiště zálohování pomocí zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="b675e-294">Use Modern Backup Storage with Backup Server</span></span>](backup-mabs-add-storage.md)

