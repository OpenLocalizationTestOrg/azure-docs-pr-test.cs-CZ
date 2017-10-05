---
title: "Zálohování Azure pro úlohy SQL serveru pomocí serveru Azure Backup | Microsoft Docs"
description: "Úvod k zálohování databáze SQL serveru pomocí serveru Azure Backup"
services: backup
documentationcenter: 
author: pvrk
manager: Shivamg
editor: 
ms.assetid: c8b1f7ec-26b1-4ef0-a3f2-91aec959daea
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 2af9ebaa8f52690ed63406cbd85b77544d2d900d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-sql-server-to-azure-with-azure-backup-server"></a><span data-ttu-id="cff9a-103">Zálohování serveru SQL Server Azure s Azure Backup</span><span class="sxs-lookup"><span data-stu-id="cff9a-103">Back up SQL Server to Azure With Azure Backup Server</span></span>
<span data-ttu-id="cff9a-104">Tento článek vás provede kroky konfigurace pro zálohování databáze SQL serveru pomocí Microsoft Azure Backup Server (MABS).</span><span class="sxs-lookup"><span data-stu-id="cff9a-104">This article leads you through the configuration steps for backup of SQL Server databases using Microsoft Azure Backup Server (MABS).</span></span>

<span data-ttu-id="cff9a-105">Správa zálohování databáze serveru SQL Server do Azure a obnovení z Azure zahrnuje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="cff9a-105">The management of SQL Server database backup to Azure and recovery from Azure involves three steps:</span></span>

1. <span data-ttu-id="cff9a-106">Vytvořte zásady zálohování pro ochranu databází systému SQL Server do Azure.</span><span class="sxs-lookup"><span data-stu-id="cff9a-106">Create a backup policy to protect SQL Server databases to Azure.</span></span>
2. <span data-ttu-id="cff9a-107">Vytvořte záložní kopie na vyžádání do Azure.</span><span class="sxs-lookup"><span data-stu-id="cff9a-107">Create on-demand backup copies to Azure.</span></span>
3. <span data-ttu-id="cff9a-108">Obnovte databázi z Azure.</span><span class="sxs-lookup"><span data-stu-id="cff9a-108">Recover the database from Azure.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="cff9a-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="cff9a-109">Before you start</span></span>
<span data-ttu-id="cff9a-110">Než začnete, ujistěte se, že máte [nainstalované a připravené serveru Azure Backup](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="cff9a-110">Before you begin, ensure that you have [installed and prepared the Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span></span>

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a><span data-ttu-id="cff9a-111">Vytvořit zásady zálohování pro ochranu databází systému SQL Server do Azure</span><span class="sxs-lookup"><span data-stu-id="cff9a-111">Create a backup policy to protect SQL Server databases to Azure</span></span>
1. <span data-ttu-id="cff9a-112">V Uživatelském rozhraní Azure Backup Server, klikněte **ochrany** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="cff9a-112">On the Azure Backup Server UI, click the **Protection** workspace.</span></span>
2. <span data-ttu-id="cff9a-113">Na pásu karet nástroje klikněte na tlačítko **nový** k vytvoření nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="cff9a-113">On the tool ribbon, click **New** to create a new protection group.</span></span>

    ![Vytvořit skupinu ochrany](./media/backup-azure-backup-sql/protection-group.png)
3. <span data-ttu-id="cff9a-115">MABS zobrazí na úvodní obrazovce s pokyny k vytváření **skupiny ochrany**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-115">MABS shows the start screen with the guidance on creating a **Protection Group**.</span></span> <span data-ttu-id="cff9a-116">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-116">Click **Next**.</span></span>
4. <span data-ttu-id="cff9a-117">Vyberte **servery**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-117">Select **Servers**.</span></span>

    ![Vybrat typ skupiny ochrany – 'servery.](./media/backup-azure-backup-sql/pg-servers.png)
5. <span data-ttu-id="cff9a-119">Rozbalte počítači serveru SQL Server, kde nejsou databáze k zálohování.</span><span class="sxs-lookup"><span data-stu-id="cff9a-119">Expand the SQL Server machine where the databases to be backed up are present.</span></span> <span data-ttu-id="cff9a-120">MABS zobrazí různé zdroje dat, které lze zálohovat z tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="cff9a-120">MABS shows various data sources that can be backed up from that server.</span></span> <span data-ttu-id="cff9a-121">Rozbalte **všechny sdílené složky SQL** a vyberte k zálohování databáze (v tomto případě jsme vybrali ReportServer$ MSDPM2012 a ReportServer$ MSDPM2012TempDB).</span><span class="sxs-lookup"><span data-stu-id="cff9a-121">Expand the **All SQL Shares** and select the databases (in this case we selected ReportServer$MSDPM2012 and ReportServer$MSDPM2012TempDB) to be backed up.</span></span> <span data-ttu-id="cff9a-122">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-122">Click **Next**.</span></span>

    ![Vyberte databázi SQL](./media/backup-azure-backup-sql/pg-databases.png)
6. <span data-ttu-id="cff9a-124">Zadejte název pro skupinu ochrany a vyberte **chci online ochranu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="cff9a-124">Provide a name for the protection group and select the **I want online Protection** checkbox.</span></span>

    ![Způsob ochrany dat – krátkodobou disku & Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. <span data-ttu-id="cff9a-126">V **zadat krátkodobé cíle** obrazovky, zahrnují nezbytné vstupy pro vytvoření body zálohy na disk.</span><span class="sxs-lookup"><span data-stu-id="cff9a-126">In the **Specify Short-Term Goals** screen, include the necessary inputs to create backup points to disk.</span></span>

    <span data-ttu-id="cff9a-127">Tady vidíte, **rozsah uchování** je nastaven na *5 dní*, **četnost synchronizace** je nastavena na jednou každých *15 minut* tedy frekvenci, kdy se provede zálohování.</span><span class="sxs-lookup"><span data-stu-id="cff9a-127">Here we see that **Retention range** is set to *5 days*, **Synchronization frequency** is set to once every *15 minutes* which is the frequency at which backup is taken.</span></span> <span data-ttu-id="cff9a-128">**Expresní úplná záloha** je nastaven na *východního*.</span><span class="sxs-lookup"><span data-stu-id="cff9a-128">**Express Full Backup** is set to *8:00 P.M*.</span></span>

    ![Krátkodobé cíle](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > <span data-ttu-id="cff9a-130">V 8:00 PM (podle vstup obrazovky) je bod zálohy vytvořené přenosu dat, která byla změněna z bodu zálohy 8:00 PM předchozí den každý den.</span><span class="sxs-lookup"><span data-stu-id="cff9a-130">At 8:00 PM (according to the screen input) a backup point is created every day by transferring the data that has been modified from the previous day’s 8:00 PM backup point.</span></span> <span data-ttu-id="cff9a-131">Tento proces se nazývá **expresní úplné zálohování**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-131">This process is called **Express Full Backup**.</span></span> <span data-ttu-id="cff9a-132">Při transakce, které protokoly se synchronizují každých 15 minut Pokud je třeba obnovit databázi na 21:00:00 – bod je vytvořen pomocí přehrání protokoly z poslední expresní úplné zálohování bod (20: 00 v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="cff9a-132">While the transaction logs are synchronized every 15 minutes, if there is a need to recover the database at 9:00 PM – then the point is created by replaying the logs from the last express full backup point (8pm in this case).</span></span>
   >
   >

8. <span data-ttu-id="cff9a-133">Klikněte na **Další**</span><span class="sxs-lookup"><span data-stu-id="cff9a-133">Click **Next**</span></span>

    <span data-ttu-id="cff9a-134">MABS zobrazuje celkového prostoru úložiště, která je k dispozici a potenciální využití místa na disku.</span><span class="sxs-lookup"><span data-stu-id="cff9a-134">MABS shows the overall storage space available and the potential disk space utilization.</span></span>

    ![Přidělení disku](./media/backup-azure-backup-sql/pg-storage.png)

    <span data-ttu-id="cff9a-136">Ve výchozím nastavení vytvoří MABS jeden svazek na zdroj dat (databázi systému SQL Server), který se používá pro kopie prvotní zálohy.</span><span class="sxs-lookup"><span data-stu-id="cff9a-136">By default, MABS creates one volume per data source (SQL Server database) which is used for the initial backup copy.</span></span> <span data-ttu-id="cff9a-137">Tento přístup Správce logických disků (LDM) omezuje MABS ochranu zdroje dat 300 (databáze systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="cff9a-137">Using this approach, the Logical Disk Manager (LDM) limits MABS protection to 300 data sources (SQL Server databases).</span></span> <span data-ttu-id="cff9a-138">Chcete-li toto omezení obejít, vyberte **společně umísťovat data do fondu úložiště DPM**, možnost.</span><span class="sxs-lookup"><span data-stu-id="cff9a-138">To work around this limitation, select the **Co-locate data in DPM Storage Pool**, option.</span></span> <span data-ttu-id="cff9a-139">Pokud použijete tuto možnost, MABS používá na jednom svazku více zdrojů dat, která umožňuje MABS chránit až 2 000 databází SQL.</span><span class="sxs-lookup"><span data-stu-id="cff9a-139">If you use this option, MABS uses a single volume for multiple data sources, which allows MABS to protect up to 2000 SQL databases.</span></span>

    <span data-ttu-id="cff9a-140">Pokud **automaticky rozšiřovat svazky** je vybraná možnost, MABS můžete účet vyšší zálohování svazku s růstem provozními daty.</span><span class="sxs-lookup"><span data-stu-id="cff9a-140">If **Automatically grow the volumes** option is selected, MABS can account for the increased backup volume as the production data grows.</span></span> <span data-ttu-id="cff9a-141">Pokud **automaticky rozšiřovat svazky** možnost není vybraná, MABS omezuje zálohování úložiště používá ke zdrojům dat ve skupině ochrany.</span><span class="sxs-lookup"><span data-stu-id="cff9a-141">If **Automatically grow the volumes** option is not selected, MABS limits the backup storage used to the data sources in the protection group.</span></span>
9. <span data-ttu-id="cff9a-142">Správci mají možnost vyhnout zahlcení šířky pásma přenosu tuto prvotní zálohování ručně (vypnuto síť) nebo přes síť.</span><span class="sxs-lookup"><span data-stu-id="cff9a-142">Administrators are given the choice of transferring this initial backup manually (off network) to avoid bandwidth congestion or over the network.</span></span> <span data-ttu-id="cff9a-143">Mohou také nakonfigurovat čas, kdy může dojít počáteční přenos.</span><span class="sxs-lookup"><span data-stu-id="cff9a-143">They can also configure the time at which the initial transfer can happen.</span></span> <span data-ttu-id="cff9a-144">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-144">Click **Next**.</span></span>

    ![Metodu počáteční replikace](./media/backup-azure-backup-sql/pg-manual.png)

    <span data-ttu-id="cff9a-146">Kopie prvotní zálohy vyžaduje přenos celého datového zdroje (databázi systému SQL Server) z provozního serveru (počítači serveru SQL Server) do MABS.</span><span class="sxs-lookup"><span data-stu-id="cff9a-146">The initial backup copy requires transfer of the entire data source (SQL Server database) from production server (SQL Server machine) to MABS.</span></span> <span data-ttu-id="cff9a-147">Tato data mohou být velký a přenos dat přes síť může překročit šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="cff9a-147">This data might be large, and transferring the data over the network could exceed bandwidth.</span></span> <span data-ttu-id="cff9a-148">Z tohoto důvodu se mohou správci k přenosu prvotní zálohování: **ručně** (pomocí vyměnitelného média) předejdete zahlcení šířky pásma, nebo **automaticky prostřednictvím sítě** (v určený čas).</span><span class="sxs-lookup"><span data-stu-id="cff9a-148">For this reason, administrators can choose to transfer the initial backup: **Manually** (using removable media) to avoid bandwidth congestion, or **Automatically over the network** (at a specified time).</span></span>

    <span data-ttu-id="cff9a-149">Po dokončení prvotní zálohování zbývající zálohy jsou přírůstkových záloh v kopie prvotní zálohy.</span><span class="sxs-lookup"><span data-stu-id="cff9a-149">Once the initial backup is complete, the rest of the backups are incremental backups on the initial backup copy.</span></span> <span data-ttu-id="cff9a-150">Přírůstkové zálohování jsou obvykle malé a snadno přenést přes síť.</span><span class="sxs-lookup"><span data-stu-id="cff9a-150">Incremental backups tend to be small and are easily transferred across the network.</span></span>
10. <span data-ttu-id="cff9a-151">Vyberte, pokud chcete kontrolu konzistence, spusťte a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-151">Choose when you want the consistency check to run and click **Next**.</span></span>

    ![Kontrola konzistence](./media/backup-azure-backup-sql/pg-consistent.png)

    <span data-ttu-id="cff9a-153">MABS může kontrola konzistence kontroly k integritu bodu zálohy.</span><span class="sxs-lookup"><span data-stu-id="cff9a-153">MABS can perform a consistency check to check the integrity of the backup point.</span></span> <span data-ttu-id="cff9a-154">Vypočte kontrolního součtu záložní soubor na provozním serveru (počítači serveru SQL Server v tomto scénáři) a zálohovaná data pro tento soubor na MABS.</span><span class="sxs-lookup"><span data-stu-id="cff9a-154">It calculates the checksum of the backup file on the production server (SQL Server machine in this scenario) and the backed-up data for that file at MABS.</span></span> <span data-ttu-id="cff9a-155">V případě konfliktu se předpokládá, že soubor zálohy v MABS je poškozený.</span><span class="sxs-lookup"><span data-stu-id="cff9a-155">In the case of a conflict, it is assumed that the backed-up file at MABS is corrupt.</span></span> <span data-ttu-id="cff9a-156">MABS rectifies zálohovaná data odesláním bloky odpovídající neshoda kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="cff9a-156">MABS rectifies the backed-up data by sending the blocks corresponding to the checksum mismatch.</span></span> <span data-ttu-id="cff9a-157">Kontrola konzistence je operace náročné na výkon, mají správci možnost plánovaná kontrola konzistence nebo spuštěním automaticky.</span><span class="sxs-lookup"><span data-stu-id="cff9a-157">As the consistency check is a performance-intensive operation, administrators have the option of scheduling the consistency check or running it automatically.</span></span>
11. <span data-ttu-id="cff9a-158">K určení online ochrany zdroje dat, vyberte databáze chránit do služby Azure a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-158">To specify online protection of the datasources, select the databases to be protected to Azure and click **Next**.</span></span>

    ![Vyberte zdroje dat](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. <span data-ttu-id="cff9a-160">Správci mohou plánů zálohování a zásady uchovávání informací, která vyhovuje své zásady organizace.</span><span class="sxs-lookup"><span data-stu-id="cff9a-160">Administrators can choose backup schedules and retention policies that suit their organization policies.</span></span>

    ![Plán a jejich uchovávání](./media/backup-azure-backup-sql/pg-schedule.png)

    <span data-ttu-id="cff9a-162">V tomto příkladu jsou provedeny zálohy jednou za den v 12:00 PM a 20: 00 (dolní části obrazovky)</span><span class="sxs-lookup"><span data-stu-id="cff9a-162">In this example, backups are taken once a day at 12:00 PM and 8 PM (bottom part of the screen)</span></span>

    > [!NOTE]
    > <span data-ttu-id="cff9a-163">Je vhodné mít několik krátkodobé body obnovení na disku pro rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="cff9a-163">It’s a good practice to have a few short-term recovery points on disk, for quick recovery.</span></span> <span data-ttu-id="cff9a-164">Tyto body obnovení se používají pro "provozní obnovení".</span><span class="sxs-lookup"><span data-stu-id="cff9a-164">These recovery points are used for “operational recovery".</span></span> <span data-ttu-id="cff9a-165">Azure slouží jako dobrý mimo pracoviště s vyšší SLA a záruku dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="cff9a-165">Azure serves as a good offsite location with higher SLAs and guaranteed availability.</span></span>
    >
    >

    <span data-ttu-id="cff9a-166">**Nejvhodnější**: Ujistěte se, že zálohy Azure jsou naplánovány po dokončení zálohování místního disku pomocí aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="cff9a-166">**Best Practice**: Make sure that Azure Backups are scheduled after the completion of local disk backups using DPM.</span></span> <span data-ttu-id="cff9a-167">To umožňuje nejnovější zálohu disku zkopírovány do Azure.</span><span class="sxs-lookup"><span data-stu-id="cff9a-167">This enables the latest disk backup to be copied to Azure.</span></span>

13. <span data-ttu-id="cff9a-168">Zvolte plán zásad uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="cff9a-168">Choose the retention policy schedule.</span></span> <span data-ttu-id="cff9a-169">Podrobnosti o způsobu fungování zásady uchovávání informací jsou k dispozici v [pomocí Azure Backup k nahrazení váš článek infrastruktury pásky](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="cff9a-169">The details on how the retention policy works are provided at [Use Azure Backup to replace your tape infrastructure article](backup-azure-backup-cloud-as-tape.md).</span></span>

    ![Zásady uchovávání informací](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    <span data-ttu-id="cff9a-171">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="cff9a-171">In this example:</span></span>

    * <span data-ttu-id="cff9a-172">Zálohování se pořídí jednou denně na 12:00 PM a 20: 00 (dolní části obrazovky) a jsou uchovány na 180 dní.</span><span class="sxs-lookup"><span data-stu-id="cff9a-172">Backups are taken once a day at 12:00 PM and 8 PM (bottom part of the screen) and are retained for 180 days.</span></span>
    * <span data-ttu-id="cff9a-173">Zálohování na sobotu ve 12:00</span><span class="sxs-lookup"><span data-stu-id="cff9a-173">The backup on Saturday at 12:00 P.M.</span></span> <span data-ttu-id="cff9a-174">se zachovává kvůli 104 týdny</span><span class="sxs-lookup"><span data-stu-id="cff9a-174">is retained for 104 weeks</span></span>
    * <span data-ttu-id="cff9a-175">Zálohování na poslední sobotu ve 12:00</span><span class="sxs-lookup"><span data-stu-id="cff9a-175">The backup on Last Saturday at 12:00 P.M.</span></span> <span data-ttu-id="cff9a-176">se uchovávají po dobu 60 měsíců</span><span class="sxs-lookup"><span data-stu-id="cff9a-176">is retained for 60 months</span></span>
    * <span data-ttu-id="cff9a-177">Zálohování na poslední sobotu dne ve 12:00</span><span class="sxs-lookup"><span data-stu-id="cff9a-177">The backup on Last Saturday of March at 12:00 P.M.</span></span> <span data-ttu-id="cff9a-178">se uchovávají 10 let.</span><span class="sxs-lookup"><span data-stu-id="cff9a-178">is retained for 10 years</span></span>
14. <span data-ttu-id="cff9a-179">Klikněte na tlačítko **Další** a vyberte příslušnou možnost pro přenesení kopie prvotní zálohy do Azure.</span><span class="sxs-lookup"><span data-stu-id="cff9a-179">Click **Next** and select the appropriate option for transferring the initial backup copy to Azure.</span></span> <span data-ttu-id="cff9a-180">Můžete zvolit **automaticky přes síť** nebo **Offline zálohování**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-180">You can choose **Automatically over the network** or **Offline Backup**.</span></span>

    * <span data-ttu-id="cff9a-181">**Automaticky prostřednictvím sítě** přenosy dat zálohování do Azure podle plánu zvolené pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="cff9a-181">**Automatically over the network** transfers the backup data to Azure as per the schedule chosen for backup.</span></span>
    * <span data-ttu-id="cff9a-182">Jak **Offline zálohování** funguje je vysvětleno v [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="cff9a-182">How **Offline Backup** works is explained at [Offline Backup workflow in Azure Backup](backup-azure-backup-import-export.md).</span></span>

    <span data-ttu-id="cff9a-183">Vyberte relevantní přenos mechanismus odeslat kopie prvotní zálohy do Azure a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-183">Choose the relevant transfer mechanism to send the initial backup copy to Azure and click **Next**.</span></span>
15. <span data-ttu-id="cff9a-184">Po prostudování podrobnosti zásady v **Souhrn** obrazovky, klikněte na **vytvořit skupinu** tlačítko dokončení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="cff9a-184">Once you review the policy details in the **Summary** screen, click on the **Create group** button to complete the workflow.</span></span> <span data-ttu-id="cff9a-185">Můžete kliknout na **Zavřít** tlačítko a sledovat průběh úlohy v pracovním prostoru monitorování.</span><span class="sxs-lookup"><span data-stu-id="cff9a-185">You can click the **Close** button and monitor the job progress in Monitoring workspace.</span></span>

    ![Vytvoření skupiny ochrany v průběhu](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a><span data-ttu-id="cff9a-187">Zálohování na vyžádání databáze systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="cff9a-187">On-demand backup of a SQL Server database</span></span>
<span data-ttu-id="cff9a-188">Když v předchozím postupu vytvořili zásadu zálohování, "bodu obnovení" se vytvoří jenom v případě, že dochází k první zálohování.</span><span class="sxs-lookup"><span data-stu-id="cff9a-188">While the previous steps created a backup policy, a “recovery point” is created only when the first backup occurs.</span></span> <span data-ttu-id="cff9a-189">Místo čekání plánovače na nové, kroků aktivační událost vytvoření obnovení bodu ručně.</span><span class="sxs-lookup"><span data-stu-id="cff9a-189">Rather than waiting for the scheduler to kick in, the steps below trigger the creation of a recovery point manually.</span></span>

1. <span data-ttu-id="cff9a-190">Počkejte, až se zobrazí stav skupiny ochrany **OK** pro databázi před vytvořením bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="cff9a-190">Wait until the protection group status shows **OK** for the database before creating the recovery point.</span></span>

    ![Členové skupiny ochrany](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. <span data-ttu-id="cff9a-192">Klikněte pravým tlačítkem na databázi a vyberte **vytvořit bod obnovení**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-192">Right-click on the database and select **Create Recovery Point**.</span></span>

    ![Vytvořit bod obnovení Online](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. <span data-ttu-id="cff9a-194">Zvolte **Online ochrany** rozevírací nabídky a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-194">Choose **Online Protection** in the drop-down menu and click **OK**.</span></span> <span data-ttu-id="cff9a-195">Vytvoření bodu obnovení se spustí v Azure.</span><span class="sxs-lookup"><span data-stu-id="cff9a-195">This starts the creation of a recovery point in Azure.</span></span>

    ![Vytvoření bodu obnovení](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. <span data-ttu-id="cff9a-197">Můžete zobrazit průběh úlohy v **monitorování** prostoru, kde najdete v průběhu úlohy, jako je ta, které popsané v následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="cff9a-197">You can view the job progress in the **Monitoring** workspace where you'll find an in progress job like the one depicted in the next figure.</span></span>

    ![Konzola monitorování](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a><span data-ttu-id="cff9a-199">Obnovení databáze systému SQL Server z Azure</span><span class="sxs-lookup"><span data-stu-id="cff9a-199">Recover a SQL Server database from Azure</span></span>
<span data-ttu-id="cff9a-200">Následující kroky jsou potřebné k obnovení chráněné entity (databázi systému SQL Server) ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="cff9a-200">The following steps are required to recover a protected entity (SQL Server database) from Azure.</span></span>

1. <span data-ttu-id="cff9a-201">Otevřete konzolu pro správu serveru aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="cff9a-201">Open the DPM server Management Console.</span></span> <span data-ttu-id="cff9a-202">Přejděte na **obnovení** prostoru, kde můžete sledovat servery zálohované aplikací DPM.</span><span class="sxs-lookup"><span data-stu-id="cff9a-202">Navigate to **Recovery** workspace where you can see the servers backed up by DPM.</span></span> <span data-ttu-id="cff9a-203">Procházejte požadovaná databáze (v této rozlišují ReportServer$ MSDPM2012).</span><span class="sxs-lookup"><span data-stu-id="cff9a-203">Browse the required database (in this case ReportServer$MSDPM2012).</span></span> <span data-ttu-id="cff9a-204">Vyberte **obnovení z** čas, který končí **Online**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-204">Select a **Recovery from** time which ends with **Online**.</span></span>

    ![Vyberte bod obnovení](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. <span data-ttu-id="cff9a-206">Klikněte pravým tlačítkem na název databáze a klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-206">Right-click the database name and click **Recover**.</span></span>

    ![Obnovení z Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. <span data-ttu-id="cff9a-208">Aplikace DPM zobrazuje podrobnosti o vytvoření bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="cff9a-208">DPM shows the details of the recovery point.</span></span> <span data-ttu-id="cff9a-209">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-209">Click **Next**.</span></span> <span data-ttu-id="cff9a-210">Chcete-li přepsat databázi, vyberte typ obnovení **obnovit na původní instanci systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-210">To overwrite the database, select the recovery type **Recover to original instance of SQL Server**.</span></span> <span data-ttu-id="cff9a-211">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-211">Click **Next**.</span></span>

    ![Obnovení do původního umístění](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    <span data-ttu-id="cff9a-213">V tomto příkladu aplikace DPM umožňuje obnovení databáze do jiné instance systému SQL Server nebo do samostatné síťové složky.</span><span class="sxs-lookup"><span data-stu-id="cff9a-213">In this example, DPM allows recovery of the database to another SQL Server instance or to a standalone network folder.</span></span>
4. <span data-ttu-id="cff9a-214">V **možnosti obnovení zadejte** obrazovce můžete vybrat možnosti obnovení jako omezení využití šířky pásma šířky pásma sítě používané při obnovení.</span><span class="sxs-lookup"><span data-stu-id="cff9a-214">In the **Specify Recovery options** screen, you can select the recovery options like Network bandwidth usage throttling to throttle the bandwidth used by recovery.</span></span> <span data-ttu-id="cff9a-215">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-215">Click **Next**.</span></span>
5. <span data-ttu-id="cff9a-216">V **Souhrn** obrazovky, uvidíte všechny konfigurace obnovení dosavadní zadat.</span><span class="sxs-lookup"><span data-stu-id="cff9a-216">In the **Summary** screen, you see all the recovery configurations provided so far.</span></span> <span data-ttu-id="cff9a-217">Klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="cff9a-217">Click **Recover**.</span></span>

    <span data-ttu-id="cff9a-218">Stav obnovení zobrazuje databázi obnovení.</span><span class="sxs-lookup"><span data-stu-id="cff9a-218">The Recovery status shows the database being recovered.</span></span> <span data-ttu-id="cff9a-219">Můžete kliknout na **zavřete** zavřete průvodce a sledovat průběh v **monitorování** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="cff9a-219">You can click **Close** to close the wizard and view the progress in the **Monitoring** workspace.</span></span>

    ![Zahájení obnovení](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    <span data-ttu-id="cff9a-221">Po dokončení obnovení je obnovenou databázi aplikace, které jsou konzistentní.</span><span class="sxs-lookup"><span data-stu-id="cff9a-221">Once the recovery is completed, the restored database is application consistent.</span></span>

### <a name="next-steps"></a><span data-ttu-id="cff9a-222">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="cff9a-222">Next Steps:</span></span>
<span data-ttu-id="cff9a-223">• [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)</span><span class="sxs-lookup"><span data-stu-id="cff9a-223">•    [Azure Backup FAQ](backup-azure-backup-faq.md)</span></span>
