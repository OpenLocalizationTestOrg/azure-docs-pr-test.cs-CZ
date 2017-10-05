---
title: "Zálohování Azure pro úlohy SQL serveru pomocí aplikace DPM | Microsoft Docs"
description: "Úvod k zálohování databází systému SQL Server pomocí služby zálohování Azure"
services: backup
documentationcenter: 
author: adigan
manager: Nkolli
editor: 
ms.assetid: 59df5bec-d959-457d-8731-7b20f7f1013e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adigan;giridham;jimpark;markgal;trinadhk
ms.openlocfilehash: c9edc066ea2edc9cd4b8453047d5584a588174dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-sql-server-to-azure-as-a-dpm-workload"></a><span data-ttu-id="faf83-103">Zálohování serveru SQL do Azure jako zatížení aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="faf83-103">Back up SQL Server to Azure as a DPM workload</span></span>
<span data-ttu-id="faf83-104">Tento článek vás provede kroky konfigurace pro zálohování databází systému SQL Server pomocí služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="faf83-104">This article leads you through the configuration steps for backup of SQL Server databases using Azure Backup.</span></span>

<span data-ttu-id="faf83-105">K zálohování databází systému SQL Server do Azure, potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="faf83-105">To back up SQL Server databases to Azure, you need an Azure account.</span></span> <span data-ttu-id="faf83-106">Pokud nemáte účet, můžete jenom několika minut vytvořit Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="faf83-106">If you don’t have an account, you can create a free trial account in just couple of minutes.</span></span> <span data-ttu-id="faf83-107">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="faf83-107">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="faf83-108">Správa zálohování databáze serveru SQL Server do Azure a obnovení z Azure zahrnuje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="faf83-108">The management of SQL Server database backup to Azure and recovery from Azure involves three steps:</span></span>

1. <span data-ttu-id="faf83-109">Vytvořte zásady zálohování pro ochranu databází systému SQL Server do Azure.</span><span class="sxs-lookup"><span data-stu-id="faf83-109">Create a backup policy to protect SQL Server databases to Azure.</span></span>
2. <span data-ttu-id="faf83-110">Vytvořte záložní kopie na vyžádání do Azure.</span><span class="sxs-lookup"><span data-stu-id="faf83-110">Create on-demand backup copies to Azure.</span></span>
3. <span data-ttu-id="faf83-111">Obnovte databázi z Azure.</span><span class="sxs-lookup"><span data-stu-id="faf83-111">Recover the database from Azure.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="faf83-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="faf83-112">Before you start</span></span>
<span data-ttu-id="faf83-113">Než začnete, ujistěte se, že všechny [požadavky](backup-azure-dpm-introduction.md#prerequisites) pro použití Microsoft Azure Backup k ochraně byly splněny úlohy.</span><span class="sxs-lookup"><span data-stu-id="faf83-113">Before you begin, ensure that all the [prerequisites](backup-azure-dpm-introduction.md#prerequisites) for using Microsoft Azure Backup to protect workloads have been met.</span></span> <span data-ttu-id="faf83-114">Požadované součásti zahrnují úlohy, jako: Vytvoření úložiště záloh, stáhnete pověření k úložišti, instalace aplikace Azure Backup Agent a registrace serveru s úložištěm.</span><span class="sxs-lookup"><span data-stu-id="faf83-114">The prerequisites cover tasks such as: creating a backup vault, downloading vault credentials, installing the Azure Backup Agent, and registering the server with the vault.</span></span>

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a><span data-ttu-id="faf83-115">Vytvořit zásady zálohování pro ochranu databází systému SQL Server do Azure</span><span class="sxs-lookup"><span data-stu-id="faf83-115">Create a backup policy to protect SQL Server databases to Azure</span></span>
1. <span data-ttu-id="faf83-116">Na serveru aplikace DPM, klikněte **ochrany** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="faf83-116">On the DPM server, click the **Protection** workspace.</span></span>
2. <span data-ttu-id="faf83-117">Na pásu karet nástroje klikněte na tlačítko **nový** k vytvoření nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="faf83-117">On the tool ribbon, click **New** to create a new protection group.</span></span>

    ![Vytvořit skupinu ochrany](./media/backup-azure-backup-sql/protection-group.png)
3. <span data-ttu-id="faf83-119">Aplikace DPM zobrazuje na úvodní obrazovce s pokyny k vytváření **skupiny ochrany**.</span><span class="sxs-lookup"><span data-stu-id="faf83-119">DPM shows the start screen with the guidance on creating a **Protection Group**.</span></span> <span data-ttu-id="faf83-120">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-120">Click **Next**.</span></span>
4. <span data-ttu-id="faf83-121">Vyberte **servery**.</span><span class="sxs-lookup"><span data-stu-id="faf83-121">Select **Servers**.</span></span>

    ![Vybrat typ skupiny ochrany – 'servery.](./media/backup-azure-backup-sql/pg-servers.png)
5. <span data-ttu-id="faf83-123">Rozbalte počítači serveru SQL Server, kde nejsou databáze k zálohování.</span><span class="sxs-lookup"><span data-stu-id="faf83-123">Expand the SQL Server machine where the databases to be backed up are present.</span></span> <span data-ttu-id="faf83-124">Aplikace DPM zobrazuje různé zdroje dat, které lze zálohovat z tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="faf83-124">DPM shows various data sources that can be backed up from that server.</span></span> <span data-ttu-id="faf83-125">Rozbalte **všechny sdílené složky SQL** a vyberte k zálohování databáze (v tomto případě jsme vybrali ReportServer$ MSDPM2012 a ReportServer$ MSDPM2012TempDB).</span><span class="sxs-lookup"><span data-stu-id="faf83-125">Expand the **All SQL Shares** and select the databases (in this case we selected ReportServer$MSDPM2012 and ReportServer$MSDPM2012TempDB) to be backed up.</span></span> <span data-ttu-id="faf83-126">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-126">Click **Next**.</span></span>

    ![Vyberte databázi SQL](./media/backup-azure-backup-sql/pg-databases.png)
6. <span data-ttu-id="faf83-128">Zadejte název pro skupinu ochrany a vyberte **chci online ochranu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="faf83-128">Provide a name for the protection group and select the **I want online Protection** checkbox.</span></span>

    ![Způsob ochrany dat – krátkodobou disku & Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. <span data-ttu-id="faf83-130">V **zadat krátkodobé cíle** obrazovky, zahrnují nezbytné vstupy pro vytvoření body zálohy na disk.</span><span class="sxs-lookup"><span data-stu-id="faf83-130">In the **Specify Short-Term Goals** screen, include the necessary inputs to create backup points to disk.</span></span>

    <span data-ttu-id="faf83-131">Tady vidíte, **rozsah uchování** je nastaven na *5 dní*, **četnost synchronizace** je nastavena na jednou každých *15 minut* tedy frekvenci, kdy se provede zálohování.</span><span class="sxs-lookup"><span data-stu-id="faf83-131">Here we see that **Retention range** is set to *5 days*, **Synchronization frequency** is set to once every *15 minutes* which is the frequency at which backup is taken.</span></span> <span data-ttu-id="faf83-132">**Expresní úplná záloha** je nastaven na *východního*.</span><span class="sxs-lookup"><span data-stu-id="faf83-132">**Express Full Backup** is set to *8:00 P.M*.</span></span>

    ![Krátkodobé cíle](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > <span data-ttu-id="faf83-134">V 8:00 PM (podle vstup obrazovky) je bod zálohy vytvořené přenosu dat, která byla změněna z bodu zálohy 8:00 PM předchozí den každý den.</span><span class="sxs-lookup"><span data-stu-id="faf83-134">At 8:00 PM (according to the screen input) a backup point is created every day by transferring the data that has been modified from the previous day’s 8:00 PM backup point.</span></span> <span data-ttu-id="faf83-135">Tento proces se nazývá **expresní úplné zálohování**.</span><span class="sxs-lookup"><span data-stu-id="faf83-135">This process is called **Express Full Backup**.</span></span> <span data-ttu-id="faf83-136">Při transakce, které protokoly se synchronizují každých 15 minut Pokud je třeba obnovit databázi na 21:00:00 – bod je vytvořen pomocí přehrání protokoly z poslední expresní úplné zálohování bod (20: 00 v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="faf83-136">While the transaction logs are synchronized every 15 minutes, if there is a need to recover the database at 9:00 PM – then the point is created by replaying the logs from the last express full backup point (8pm in this case).</span></span>
   >
   >

8. <span data-ttu-id="faf83-137">Klikněte na **Další**</span><span class="sxs-lookup"><span data-stu-id="faf83-137">Click **Next**</span></span>

    <span data-ttu-id="faf83-138">Aplikace DPM zobrazuje celkového prostoru úložiště, která je k dispozici a potenciální využití místa na disku.</span><span class="sxs-lookup"><span data-stu-id="faf83-138">DPM shows the overall storage space available and the potential disk space utilization.</span></span>

    ![Přidělení disku](./media/backup-azure-backup-sql/pg-storage.png)

    <span data-ttu-id="faf83-140">Ve výchozím nastavení aplikace DPM vytvoří jeden svazek na zdroj dat (databázi systému SQL Server), který se používá pro kopie prvotní zálohy.</span><span class="sxs-lookup"><span data-stu-id="faf83-140">By default, DPM creates one volume per data source (SQL Server database) which is used for the initial backup copy.</span></span> <span data-ttu-id="faf83-141">Tento přístup Správce logických disků (LDM) omezuje ochrany DPM do 300 datové zdroje (databáze systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="faf83-141">Using this approach, the Logical Disk Manager (LDM) limits DPM protection to 300 data sources (SQL Server databases).</span></span> <span data-ttu-id="faf83-142">Chcete-li toto omezení obejít, vyberte **společně umísťovat data do fondu úložiště DPM**, možnost.</span><span class="sxs-lookup"><span data-stu-id="faf83-142">To work around this limitation, select the **Co-locate data in DPM Storage Pool**, option.</span></span> <span data-ttu-id="faf83-143">Pokud použijete tuto možnost, aplikace DPM používá na jednom svazku více zdrojů dat, která umožňuje DPM chránit až 2 000 databází SQL.</span><span class="sxs-lookup"><span data-stu-id="faf83-143">If you use this option, DPM uses a single volume for multiple data sources, which allows DPM to protect up to 2000 SQL databases.</span></span>

    <span data-ttu-id="faf83-144">Pokud **automaticky rozšiřovat svazky** je vybraná možnost, aplikace DPM můžete účet vyšší zálohování svazku s růstem provozními daty.</span><span class="sxs-lookup"><span data-stu-id="faf83-144">If **Automatically grow the volumes** option is selected, DPM can account for the increased backup volume as the production data grows.</span></span> <span data-ttu-id="faf83-145">Pokud **automaticky rozšiřovat svazky** možnost není vybrána, DPM omezuje zálohování úložiště používá ke zdrojům dat ve skupině ochrany.</span><span class="sxs-lookup"><span data-stu-id="faf83-145">If **Automatically grow the volumes** option is not selected, DPM limits the backup storage used to the data sources in the protection group.</span></span>
9. <span data-ttu-id="faf83-146">Správci mají možnost vyhnout zahlcení šířky pásma přenosu tuto prvotní zálohování ručně (vypnuto síť) nebo přes síť.</span><span class="sxs-lookup"><span data-stu-id="faf83-146">Administrators are given the choice of transferring this initial backup manually (off network) to avoid bandwidth congestion or over the network.</span></span> <span data-ttu-id="faf83-147">Mohou také nakonfigurovat čas, kdy může dojít počáteční přenos.</span><span class="sxs-lookup"><span data-stu-id="faf83-147">They can also configure the time at which the initial transfer can happen.</span></span> <span data-ttu-id="faf83-148">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-148">Click **Next**.</span></span>

    ![Metodu počáteční replikace](./media/backup-azure-backup-sql/pg-manual.png)

    <span data-ttu-id="faf83-150">Kopie prvotní zálohy vyžaduje přenos celého datového zdroje (databázi systému SQL Server) z provozního serveru (počítači serveru SQL Server) do serveru DPM.</span><span class="sxs-lookup"><span data-stu-id="faf83-150">The initial backup copy requires transfer of the entire data source (SQL Server database) from production server (SQL Server machine) to the DPM server.</span></span> <span data-ttu-id="faf83-151">Tato data mohou být velký a přenos dat přes síť může překročit šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="faf83-151">This data might be large, and transferring the data over the network could exceed bandwidth.</span></span> <span data-ttu-id="faf83-152">Z tohoto důvodu se mohou správci k přenosu prvotní zálohování: **ručně** (pomocí vyměnitelného média) předejdete zahlcení šířky pásma, nebo **automaticky prostřednictvím sítě** (v určený čas).</span><span class="sxs-lookup"><span data-stu-id="faf83-152">For this reason, administrators can choose to transfer the initial backup: **Manually** (using removable media) to avoid bandwidth congestion, or **Automatically over the network** (at a specified time).</span></span>

    <span data-ttu-id="faf83-153">Po dokončení prvotní zálohování zbývající zálohy jsou přírůstkových záloh v kopie prvotní zálohy.</span><span class="sxs-lookup"><span data-stu-id="faf83-153">Once the initial backup is complete, the rest of the backups are incremental backups on the initial backup copy.</span></span> <span data-ttu-id="faf83-154">Přírůstkové zálohování jsou obvykle malé a snadno přenést přes síť.</span><span class="sxs-lookup"><span data-stu-id="faf83-154">Incremental backups tend to be small and are easily transferred across the network.</span></span>
10. <span data-ttu-id="faf83-155">Vyberte, pokud chcete kontrolu konzistence, spusťte a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-155">Choose when you want the consistency check to run and click **Next**.</span></span>

    ![Kontrola konzistence](./media/backup-azure-backup-sql/pg-consistent.png)

    <span data-ttu-id="faf83-157">Aplikace DPM může kontrola konzistence kontroly k integritu bodu zálohy.</span><span class="sxs-lookup"><span data-stu-id="faf83-157">DPM can perform a consistency check to check the integrity of the backup point.</span></span> <span data-ttu-id="faf83-158">Vypočte kontrolního součtu záložní soubor na provozním serveru (počítači serveru SQL Server v tomto scénáři) a zálohovaná data pro tento soubor v aplikaci DPM.</span><span class="sxs-lookup"><span data-stu-id="faf83-158">It calculates the checksum of the backup file on the production server (SQL Server machine in this scenario) and the backed-up data for that file at DPM.</span></span> <span data-ttu-id="faf83-159">V případě konfliktu se předpokládá, že soubor zálohy v DPM je poškozený.</span><span class="sxs-lookup"><span data-stu-id="faf83-159">In the case of a conflict, it is assumed that the backed-up file at DPM is corrupt.</span></span> <span data-ttu-id="faf83-160">Aplikace DPM rectifies zálohovaná data odesláním bloky odpovídající neshoda kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="faf83-160">DPM rectifies the backed-up data by sending the blocks corresponding to the checksum mismatch.</span></span> <span data-ttu-id="faf83-161">Kontrola konzistence je operace náročné na výkon, mají správci možnost plánovaná kontrola konzistence nebo spuštěním automaticky.</span><span class="sxs-lookup"><span data-stu-id="faf83-161">As the consistency check is a performance-intensive operation, administrators have the option of scheduling the consistency check or running it automatically.</span></span>
11. <span data-ttu-id="faf83-162">K určení online ochrany zdroje dat, vyberte databáze chránit do služby Azure a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-162">To specify online protection of the datasources, select the databases to be protected to Azure and click **Next**.</span></span>

    ![Vyberte zdroje dat](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. <span data-ttu-id="faf83-164">Správci mohou plánů zálohování a zásady uchovávání informací, která vyhovuje své zásady organizace.</span><span class="sxs-lookup"><span data-stu-id="faf83-164">Administrators can choose backup schedules and retention policies that suit their organization policies.</span></span>

    ![Plán a jejich uchovávání](./media/backup-azure-backup-sql/pg-schedule.png)

    <span data-ttu-id="faf83-166">V tomto příkladu jsou provedeny zálohy jednou za den v 12:00 PM a 20: 00 (dolní části obrazovky)</span><span class="sxs-lookup"><span data-stu-id="faf83-166">In this example, backups are taken once a day at 12:00 PM and 8 PM (bottom part of the screen)</span></span>

    > [!NOTE]
    > <span data-ttu-id="faf83-167">Je vhodné mít několik krátkodobé body obnovení na disku pro rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="faf83-167">It’s a good practice to have a few short-term recovery points on disk, for quick recovery.</span></span> <span data-ttu-id="faf83-168">Tyto body obnovení se používají pro "provozní obnovení".</span><span class="sxs-lookup"><span data-stu-id="faf83-168">These recovery points are used for “operational recovery".</span></span> <span data-ttu-id="faf83-169">Azure slouží jako dobrý mimo pracoviště s vyšší SLA a záruku dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="faf83-169">Azure serves as a good offsite location with higher SLAs and guaranteed availability.</span></span>
    >
    >

    <span data-ttu-id="faf83-170">**Nejvhodnější**: Ujistěte se, že zálohy Azure jsou naplánovány po dokončení zálohování místního disku pomocí aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="faf83-170">**Best Practice**: Make sure that Azure Backups are scheduled after the completion of local disk backups using DPM.</span></span> <span data-ttu-id="faf83-171">To umožňuje nejnovější zálohu disku zkopírovány do Azure.</span><span class="sxs-lookup"><span data-stu-id="faf83-171">This enables the latest disk backup to be copied to Azure.</span></span>

13. <span data-ttu-id="faf83-172">Zvolte plán zásad uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="faf83-172">Choose the retention policy schedule.</span></span> <span data-ttu-id="faf83-173">Podrobnosti o způsobu fungování zásady uchovávání informací jsou k dispozici v [pomocí Azure Backup k nahrazení váš článek infrastruktury pásky](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="faf83-173">The details on how the retention policy works are provided at [Use Azure Backup to replace your tape infrastructure article](backup-azure-backup-cloud-as-tape.md).</span></span>

    ![Zásady uchovávání informací](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    <span data-ttu-id="faf83-175">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="faf83-175">In this example:</span></span>

    * <span data-ttu-id="faf83-176">Zálohování se pořídí jednou denně na 12:00 PM a 20: 00 (dolní části obrazovky) a jsou uchovány na 180 dní.</span><span class="sxs-lookup"><span data-stu-id="faf83-176">Backups are taken once a day at 12:00 PM and 8 PM (bottom part of the screen) and are retained for 180 days.</span></span>
    * <span data-ttu-id="faf83-177">Zálohování na sobotu ve 12:00</span><span class="sxs-lookup"><span data-stu-id="faf83-177">The backup on Saturday at 12:00 P.M.</span></span> <span data-ttu-id="faf83-178">se zachovává kvůli 104 týdny</span><span class="sxs-lookup"><span data-stu-id="faf83-178">is retained for 104 weeks</span></span>
    * <span data-ttu-id="faf83-179">Zálohování na poslední sobotu ve 12:00</span><span class="sxs-lookup"><span data-stu-id="faf83-179">The backup on Last Saturday at 12:00 P.M.</span></span> <span data-ttu-id="faf83-180">se uchovávají po dobu 60 měsíců</span><span class="sxs-lookup"><span data-stu-id="faf83-180">is retained for 60 months</span></span>
    * <span data-ttu-id="faf83-181">Zálohování na poslední sobotu dne ve 12:00</span><span class="sxs-lookup"><span data-stu-id="faf83-181">The backup on Last Saturday of March at 12:00 P.M.</span></span> <span data-ttu-id="faf83-182">se uchovávají 10 let.</span><span class="sxs-lookup"><span data-stu-id="faf83-182">is retained for 10 years</span></span>
14. <span data-ttu-id="faf83-183">Klikněte na tlačítko **Další** a vyberte příslušnou možnost pro přenesení kopie prvotní zálohy do Azure.</span><span class="sxs-lookup"><span data-stu-id="faf83-183">Click **Next** and select the appropriate option for transferring the initial backup copy to Azure.</span></span> <span data-ttu-id="faf83-184">Můžete zvolit **automaticky přes síť** nebo **Offline zálohování**.</span><span class="sxs-lookup"><span data-stu-id="faf83-184">You can choose **Automatically over the network** or **Offline Backup**.</span></span>

    * <span data-ttu-id="faf83-185">**Automaticky prostřednictvím sítě** přenosy dat zálohování do Azure podle plánu zvolené pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="faf83-185">**Automatically over the network** transfers the backup data to Azure as per the schedule chosen for backup.</span></span>
    * <span data-ttu-id="faf83-186">Jak **Offline zálohování** funguje je vysvětleno v [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="faf83-186">How **Offline Backup** works is explained at [Offline Backup workflow in Azure Backup](backup-azure-backup-import-export.md).</span></span>

    <span data-ttu-id="faf83-187">Vyberte relevantní přenos mechanismus odeslat kopie prvotní zálohy do Azure a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-187">Choose the relevant transfer mechanism to send the initial backup copy to Azure and click **Next**.</span></span>
15. <span data-ttu-id="faf83-188">Po prostudování podrobnosti zásady v **Souhrn** obrazovky, klikněte na **vytvořit skupinu** tlačítko dokončení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="faf83-188">Once you review the policy details in the **Summary** screen, click on the **Create group** button to complete the workflow.</span></span> <span data-ttu-id="faf83-189">Můžete kliknout na **Zavřít** tlačítko a sledovat průběh úlohy v pracovním prostoru monitorování.</span><span class="sxs-lookup"><span data-stu-id="faf83-189">You can click the **Close** button and monitor the job progress in Monitoring workspace.</span></span>

    ![Vytvoření skupiny ochrany v průběhu](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a><span data-ttu-id="faf83-191">Zálohování na vyžádání databáze systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="faf83-191">On-demand backup of a SQL Server database</span></span>
<span data-ttu-id="faf83-192">Když v předchozím postupu vytvořili zásadu zálohování, "bodu obnovení" se vytvoří jenom v případě, že dochází k první zálohování.</span><span class="sxs-lookup"><span data-stu-id="faf83-192">While the previous steps created a backup policy, a “recovery point” is created only when the first backup occurs.</span></span> <span data-ttu-id="faf83-193">Místo čekání plánovače na nové, kroků aktivační událost vytvoření obnovení bodu ručně.</span><span class="sxs-lookup"><span data-stu-id="faf83-193">Rather than waiting for the scheduler to kick in, the steps below trigger the creation of a recovery point manually.</span></span>

1. <span data-ttu-id="faf83-194">Počkejte, až se zobrazí stav skupiny ochrany **OK** pro databázi před vytvořením bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="faf83-194">Wait until the protection group status shows **OK** for the database before creating the recovery point.</span></span>

    ![Členové skupiny ochrany](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. <span data-ttu-id="faf83-196">Klikněte pravým tlačítkem na databázi a vyberte **vytvořit bod obnovení**.</span><span class="sxs-lookup"><span data-stu-id="faf83-196">Right-click on the database and select **Create Recovery Point**.</span></span>

    ![Vytvořit bod obnovení Online](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. <span data-ttu-id="faf83-198">Zvolte **Online ochrany** rozevírací nabídky a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="faf83-198">Choose **Online Protection** in the drop-down menu and click **OK**.</span></span> <span data-ttu-id="faf83-199">Vytvoření bodu obnovení se spustí v Azure.</span><span class="sxs-lookup"><span data-stu-id="faf83-199">This starts the creation of a recovery point in Azure.</span></span>

    ![Vytvoření bodu obnovení](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. <span data-ttu-id="faf83-201">Můžete zobrazit průběh úlohy v **monitorování** prostoru, kde najdete v průběhu úlohy, jako je ta, které popsané v následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="faf83-201">You can view the job progress in the **Monitoring** workspace where you'll find an in progress job like the one depicted in the next figure.</span></span>

    ![Konzola monitorování](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a><span data-ttu-id="faf83-203">Obnovení databáze systému SQL Server z Azure</span><span class="sxs-lookup"><span data-stu-id="faf83-203">Recover a SQL Server database from Azure</span></span>
<span data-ttu-id="faf83-204">Následující kroky jsou potřebné k obnovení chráněné entity (databázi systému SQL Server) ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="faf83-204">The following steps are required to recover a protected entity (SQL Server database) from Azure.</span></span>

1. <span data-ttu-id="faf83-205">Otevřete konzolu pro správu serveru aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="faf83-205">Open the DPM server Management Console.</span></span> <span data-ttu-id="faf83-206">Přejděte na **obnovení** prostoru, kde můžete sledovat servery zálohované aplikací DPM.</span><span class="sxs-lookup"><span data-stu-id="faf83-206">Navigate to **Recovery** workspace where you can see the servers backed up by DPM.</span></span> <span data-ttu-id="faf83-207">Procházejte požadovaná databáze (v této rozlišují ReportServer$ MSDPM2012).</span><span class="sxs-lookup"><span data-stu-id="faf83-207">Browse the required database (in this case ReportServer$MSDPM2012).</span></span> <span data-ttu-id="faf83-208">Vyberte **obnovení z** čas, který končí **Online**.</span><span class="sxs-lookup"><span data-stu-id="faf83-208">Select a **Recovery from** time which ends with **Online**.</span></span>

    ![Vyberte bod obnovení](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. <span data-ttu-id="faf83-210">Klikněte pravým tlačítkem na název databáze a klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="faf83-210">Right-click the database name and click **Recover**.</span></span>

    ![Obnovení z Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. <span data-ttu-id="faf83-212">Aplikace DPM zobrazuje podrobnosti o vytvoření bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="faf83-212">DPM shows the details of the recovery point.</span></span> <span data-ttu-id="faf83-213">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-213">Click **Next**.</span></span> <span data-ttu-id="faf83-214">Chcete-li přepsat databázi, vyberte typ obnovení **obnovit na původní instanci systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="faf83-214">To overwrite the database, select the recovery type **Recover to original instance of SQL Server**.</span></span> <span data-ttu-id="faf83-215">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-215">Click **Next**.</span></span>

    ![Obnovení do původního umístění](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    <span data-ttu-id="faf83-217">V tomto příkladu aplikace DPM umožňuje obnovení databáze do jiné instance systému SQL Server nebo do samostatné síťové složky.</span><span class="sxs-lookup"><span data-stu-id="faf83-217">In this example, DPM allows recovery of the database to another SQL Server instance or to a standalone network folder.</span></span>
4. <span data-ttu-id="faf83-218">V **možnosti obnovení zadejte** obrazovce můžete vybrat možnosti obnovení jako omezení využití šířky pásma šířky pásma sítě používané při obnovení.</span><span class="sxs-lookup"><span data-stu-id="faf83-218">In the **Specify Recovery options** screen, you can select the recovery options like Network bandwidth usage throttling to throttle the bandwidth used by recovery.</span></span> <span data-ttu-id="faf83-219">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="faf83-219">Click **Next**.</span></span>
5. <span data-ttu-id="faf83-220">V **Souhrn** obrazovky, uvidíte všechny konfigurace obnovení dosavadní zadat.</span><span class="sxs-lookup"><span data-stu-id="faf83-220">In the **Summary** screen, you see all the recovery configurations provided so far.</span></span> <span data-ttu-id="faf83-221">Klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="faf83-221">Click **Recover**.</span></span>

    <span data-ttu-id="faf83-222">Stav obnovení zobrazuje databázi obnovení.</span><span class="sxs-lookup"><span data-stu-id="faf83-222">The Recovery status shows the database being recovered.</span></span> <span data-ttu-id="faf83-223">Můžete kliknout na **zavřete** zavřete průvodce a sledovat průběh v **monitorování** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="faf83-223">You can click **Close** to close the wizard and view the progress in the **Monitoring** workspace.</span></span>

    ![Zahájení obnovení](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    <span data-ttu-id="faf83-225">Po dokončení obnovení je obnovenou databázi aplikace, které jsou konzistentní.</span><span class="sxs-lookup"><span data-stu-id="faf83-225">Once the recovery is completed, the restored database is application consistent.</span></span>

### <a name="next-steps"></a><span data-ttu-id="faf83-226">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="faf83-226">Next Steps:</span></span>
<span data-ttu-id="faf83-227">• [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)</span><span class="sxs-lookup"><span data-stu-id="faf83-227">•    [Azure Backup FAQ](backup-azure-backup-faq.md)</span></span>
