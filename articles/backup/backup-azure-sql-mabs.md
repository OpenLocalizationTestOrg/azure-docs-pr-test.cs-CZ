---
title: "aaaAzure zálohování pro úlohy SQL serveru pomocí serveru Azure Backup | Microsoft Docs"
description: "Toobacking Úvod do databáze SQL serveru pomocí serveru Azure Backup"
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
ms.openlocfilehash: 3a94338e8aca3f9d8611a72bcd223397ffb96f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-sql-server-tooazure-with-azure-backup-server"></a><span data-ttu-id="5973c-103">Zálohování serveru SQL Server tooAzure pomocí serveru Azure Backup</span><span class="sxs-lookup"><span data-stu-id="5973c-103">Back up SQL Server tooAzure With Azure Backup Server</span></span>
<span data-ttu-id="5973c-104">Tento článek vás provede kroky konfigurace hello pro zálohování databáze SQL serveru pomocí Microsoft Azure Backup Server (MABS).</span><span class="sxs-lookup"><span data-stu-id="5973c-104">This article leads you through hello configuration steps for backup of SQL Server databases using Microsoft Azure Backup Server (MABS).</span></span>

<span data-ttu-id="5973c-105">Správa Hello tooAzure zálohování databáze systému SQL Server a obnovení z Azure zahrnuje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="5973c-105">hello management of SQL Server database backup tooAzure and recovery from Azure involves three steps:</span></span>

1. <span data-ttu-id="5973c-106">Vytvořte tooAzure databáze systému SQL Server tooprotect zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="5973c-106">Create a backup policy tooprotect SQL Server databases tooAzure.</span></span>
2. <span data-ttu-id="5973c-107">Vytvořte tooAzure záložní kopie na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5973c-107">Create on-demand backup copies tooAzure.</span></span>
3. <span data-ttu-id="5973c-108">Obnovte databázi hello z Azure.</span><span class="sxs-lookup"><span data-stu-id="5973c-108">Recover hello database from Azure.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="5973c-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="5973c-109">Before you start</span></span>
<span data-ttu-id="5973c-110">Než začnete, ujistěte se, že máte [nainstalované a připravené hello serveru Azure Backup](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="5973c-110">Before you begin, ensure that you have [installed and prepared hello Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span></span>

## <a name="create-a-backup-policy-tooprotect-sql-server-databases-tooazure"></a><span data-ttu-id="5973c-111">Vytvoření tooAzure databáze systému SQL Server tooprotect zásady zálohování</span><span class="sxs-lookup"><span data-stu-id="5973c-111">Create a backup policy tooprotect SQL Server databases tooAzure</span></span>
1. <span data-ttu-id="5973c-112">Na hello uživatelské rozhraní serveru služby zálohování Azure, klikněte na tlačítko hello **ochrany** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="5973c-112">On hello Azure Backup Server UI, click hello **Protection** workspace.</span></span>
2. <span data-ttu-id="5973c-113">Na pásu karet nástroje hello, klikněte na tlačítko **nový** toocreate nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="5973c-113">On hello tool ribbon, click **New** toocreate a new protection group.</span></span>

    ![Vytvořit skupinu ochrany](./media/backup-azure-backup-sql/protection-group.png)
3. <span data-ttu-id="5973c-115">MABS zobrazí úvodní obrazovka start s hello pokyny o vytváření **skupiny ochrany**.</span><span class="sxs-lookup"><span data-stu-id="5973c-115">MABS shows hello start screen with hello guidance on creating a **Protection Group**.</span></span> <span data-ttu-id="5973c-116">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-116">Click **Next**.</span></span>
4. <span data-ttu-id="5973c-117">Vyberte **servery**.</span><span class="sxs-lookup"><span data-stu-id="5973c-117">Select **Servers**.</span></span>

    ![Vybrat typ skupiny ochrany – 'servery.](./media/backup-azure-backup-sql/pg-servers.png)
5. <span data-ttu-id="5973c-119">Rozbalte počítači serveru SQL Server hello, kde jsou přítomny toobe databáze hello zálohovat.</span><span class="sxs-lookup"><span data-stu-id="5973c-119">Expand hello SQL Server machine where hello databases toobe backed up are present.</span></span> <span data-ttu-id="5973c-120">MABS zobrazí různé zdroje dat, které lze zálohovat z tohoto serveru.</span><span class="sxs-lookup"><span data-stu-id="5973c-120">MABS shows various data sources that can be backed up from that server.</span></span> <span data-ttu-id="5973c-121">Rozbalte hello **všechny sdílené složky SQL** a vyberte hello databáze (v tomto případě jsme vybrali ReportServer$ MSDPM2012 a ReportServer$ MSDPM2012TempDB) toobe zálohovat.</span><span class="sxs-lookup"><span data-stu-id="5973c-121">Expand hello **All SQL Shares** and select hello databases (in this case we selected ReportServer$MSDPM2012 and ReportServer$MSDPM2012TempDB) toobe backed up.</span></span> <span data-ttu-id="5973c-122">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-122">Click **Next**.</span></span>

    ![Vyberte databázi SQL](./media/backup-azure-backup-sql/pg-databases.png)
6. <span data-ttu-id="5973c-124">Zadejte název pro skupinu ochrany hello a vyberte hello **chci online ochranu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="5973c-124">Provide a name for hello protection group and select hello **I want online Protection** checkbox.</span></span>

    ![Způsob ochrany dat – krátkodobou disku & Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. <span data-ttu-id="5973c-126">V hello **zadat krátkodobé cíle** obrazovky, zahrnují hello nezbytné vstupy toocreate body zálohy toodisk.</span><span class="sxs-lookup"><span data-stu-id="5973c-126">In hello **Specify Short-Term Goals** screen, include hello necessary inputs toocreate backup points toodisk.</span></span>

    <span data-ttu-id="5973c-127">Tady vidíte, **rozsah uchování** je nastaven příliš*5 dní*, **četnost synchronizace** nastavena tooonce každých *15 minut* tedy hello frekvence, při které zálohování se.</span><span class="sxs-lookup"><span data-stu-id="5973c-127">Here we see that **Retention range** is set too*5 days*, **Synchronization frequency** is set tooonce every *15 minutes* which is hello frequency at which backup is taken.</span></span> <span data-ttu-id="5973c-128">**Expresní úplná záloha** je nastaven příliš*východního*.</span><span class="sxs-lookup"><span data-stu-id="5973c-128">**Express Full Backup** is set too*8:00 P.M*.</span></span>

    ![Krátkodobé cíle](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > <span data-ttu-id="5973c-130">V 8:00 PM (podle toohello obrazovky vstup) zálohování vytváří bod obnovení každý den přenosu hello data, která byla změněna z hello bodu zálohy 8:00 PM předchozí den.</span><span class="sxs-lookup"><span data-stu-id="5973c-130">At 8:00 PM (according toohello screen input) a backup point is created every day by transferring hello data that has been modified from hello previous day’s 8:00 PM backup point.</span></span> <span data-ttu-id="5973c-131">Tento proces se nazývá **expresní úplné zálohování**.</span><span class="sxs-lookup"><span data-stu-id="5973c-131">This process is called **Express Full Backup**.</span></span> <span data-ttu-id="5973c-132">Při protokoly transakcí hello se synchronizují každých 15 minut, pokud je nutné toorecover hello databáze na 21:00:00 – pak vytváří bod hello přehrání hello protokoly z hello poslední expresní úplné zálohování bod (20: 00 v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="5973c-132">While hello transaction logs are synchronized every 15 minutes, if there is a need toorecover hello database at 9:00 PM – then hello point is created by replaying hello logs from hello last express full backup point (8pm in this case).</span></span>
   >
   >

8. <span data-ttu-id="5973c-133">Klikněte na **Další**</span><span class="sxs-lookup"><span data-stu-id="5973c-133">Click **Next**</span></span>

    <span data-ttu-id="5973c-134">Zobrazuje MABS hello celkového prostoru úložiště k dispozici a využití místa na disku potenciální hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-134">MABS shows hello overall storage space available and hello potential disk space utilization.</span></span>

    ![Přidělení disku](./media/backup-azure-backup-sql/pg-storage.png)

    <span data-ttu-id="5973c-136">Ve výchozím nastavení vytvoří MABS jeden svazek na zdroj dat (databázi systému SQL Server), který se používá pro hello kopie prvotní zálohy.</span><span class="sxs-lookup"><span data-stu-id="5973c-136">By default, MABS creates one volume per data source (SQL Server database) which is used for hello initial backup copy.</span></span> <span data-ttu-id="5973c-137">Tento přístup hello Správce logických disků (LDM) omezuje MABS ochrany too300 datové zdroje (databáze systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="5973c-137">Using this approach, hello Logical Disk Manager (LDM) limits MABS protection too300 data sources (SQL Server databases).</span></span> <span data-ttu-id="5973c-138">toowork obejít toto omezení, vyberte hello **společně umísťovat data do fondu úložiště DPM**, možnost.</span><span class="sxs-lookup"><span data-stu-id="5973c-138">toowork around this limitation, select hello **Co-locate data in DPM Storage Pool**, option.</span></span> <span data-ttu-id="5973c-139">Pokud použijete tuto možnost, MABS používá na jednom svazku pro více zdrojů dat, která umožňuje MABS tooprotect až too2000 databází SQL.</span><span class="sxs-lookup"><span data-stu-id="5973c-139">If you use this option, MABS uses a single volume for multiple data sources, which allows MABS tooprotect up too2000 SQL databases.</span></span>

    <span data-ttu-id="5973c-140">Pokud **automaticky rozšiřovat svazky hello** je vybraná možnost, MABS můžete účet pro hello vyšší zálohování svazku s růstem hello provozními daty.</span><span class="sxs-lookup"><span data-stu-id="5973c-140">If **Automatically grow hello volumes** option is selected, MABS can account for hello increased backup volume as hello production data grows.</span></span> <span data-ttu-id="5973c-141">Pokud **automaticky rozšiřovat svazky hello** možnost není vybraná, MABS omezuje hello používá úložiště záloh toohello zdroje dat ve skupině ochrany hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-141">If **Automatically grow hello volumes** option is not selected, MABS limits hello backup storage used toohello data sources in hello protection group.</span></span>
9. <span data-ttu-id="5973c-142">Správci mají hello volba přenosu tato počáteční zálohování ručně (vypnuto síť) tooavoid zahlcení šířky pásma nebo přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-142">Administrators are given hello choice of transferring this initial backup manually (off network) tooavoid bandwidth congestion or over hello network.</span></span> <span data-ttu-id="5973c-143">Mohou také nakonfigurovat hello čas, na které hello může dojít počáteční přenos.</span><span class="sxs-lookup"><span data-stu-id="5973c-143">They can also configure hello time at which hello initial transfer can happen.</span></span> <span data-ttu-id="5973c-144">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-144">Click **Next**.</span></span>

    ![Metodu počáteční replikace](./media/backup-azure-backup-sql/pg-manual.png)

    <span data-ttu-id="5973c-146">kopie prvotní zálohy Hello vyžaduje přenos hello celého datového zdroje (databázi systému SQL Server) ze tooMABS produkční server (počítač systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="5973c-146">hello initial backup copy requires transfer of hello entire data source (SQL Server database) from production server (SQL Server machine) tooMABS.</span></span> <span data-ttu-id="5973c-147">Tato data mohou být velký a přenášení hello dat přes síť hello může překročit šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="5973c-147">This data might be large, and transferring hello data over hello network could exceed bandwidth.</span></span> <span data-ttu-id="5973c-148">Z tohoto důvodu mohou správci tootransfer hello prvotní zálohování: **ručně** (pomocí vyměnitelného média) tooavoid zahlcení šířky pásma, nebo **automaticky přes síť hello** (na zadané čas).</span><span class="sxs-lookup"><span data-stu-id="5973c-148">For this reason, administrators can choose tootransfer hello initial backup: **Manually** (using removable media) tooavoid bandwidth congestion, or **Automatically over hello network** (at a specified time).</span></span>

    <span data-ttu-id="5973c-149">Po dokončení prvotní zálohování hello hello zbytek hello zálohy jsou přírůstkových záloh v hello kopie prvotní zálohy.</span><span class="sxs-lookup"><span data-stu-id="5973c-149">Once hello initial backup is complete, hello rest of hello backups are incremental backups on hello initial backup copy.</span></span> <span data-ttu-id="5973c-150">Přírůstkové zálohování mívají toobe malé a snadno přenést přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-150">Incremental backups tend toobe small and are easily transferred across hello network.</span></span>
10. <span data-ttu-id="5973c-151">Vyberte, když chcete toorun Kontrola konzistence hello a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-151">Choose when you want hello consistency check toorun and click **Next**.</span></span>

    ![Kontrola konzistence](./media/backup-azure-backup-sql/pg-consistent.png)

    <span data-ttu-id="5973c-153">MABS můžete provést konzistence kontrola toocheck hello integrity hello bodu zálohy.</span><span class="sxs-lookup"><span data-stu-id="5973c-153">MABS can perform a consistency check toocheck hello integrity of hello backup point.</span></span> <span data-ttu-id="5973c-154">Vypočte kontrolního součtu hello hello záložního souboru na provozním serveru hello (počítači serveru SQL Server v tomto scénáři) a hello zálohovaná data pro tento soubor na MABS.</span><span class="sxs-lookup"><span data-stu-id="5973c-154">It calculates hello checksum of hello backup file on hello production server (SQL Server machine in this scenario) and hello backed-up data for that file at MABS.</span></span> <span data-ttu-id="5973c-155">V případě hello jsou v konfliktu se předpokládá, že hello soubor zálohy v MABS je poškozený.</span><span class="sxs-lookup"><span data-stu-id="5973c-155">In hello case of a conflict, it is assumed that hello backed-up file at MABS is corrupt.</span></span> <span data-ttu-id="5973c-156">MABS rectifies hello zálohovaná data odesláním hello bloky odpovídající neshoda toohello kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="5973c-156">MABS rectifies hello backed-up data by sending hello blocks corresponding toohello checksum mismatch.</span></span> <span data-ttu-id="5973c-157">Kontrola konzistence hello je operace náročné na výkon, Správci mají možnost hello plánovaná kontrola konzistence hello nebo spuštěním automaticky.</span><span class="sxs-lookup"><span data-stu-id="5973c-157">As hello consistency check is a performance-intensive operation, administrators have hello option of scheduling hello consistency check or running it automatically.</span></span>
11. <span data-ttu-id="5973c-158">toospecify online ochrany hello zdrojů dat, vyberte hello databáze toobe chráněné tooAzure a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-158">toospecify online protection of hello datasources, select hello databases toobe protected tooAzure and click **Next**.</span></span>

    ![Vyberte zdroje dat](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. <span data-ttu-id="5973c-160">Správci mohou plánů zálohování a zásady uchovávání informací, která vyhovuje své zásady organizace.</span><span class="sxs-lookup"><span data-stu-id="5973c-160">Administrators can choose backup schedules and retention policies that suit their organization policies.</span></span>

    ![Plán a jejich uchovávání](./media/backup-azure-backup-sql/pg-schedule.png)

    <span data-ttu-id="5973c-162">V tomto příkladu jsou provedeny zálohy jednou za den v 12:00 PM a 20: 00 (dolní část úvodní obrazovka)</span><span class="sxs-lookup"><span data-stu-id="5973c-162">In this example, backups are taken once a day at 12:00 PM and 8 PM (bottom part of hello screen)</span></span>

    > [!NOTE]
    > <span data-ttu-id="5973c-163">Je dobrým zvykem toohave několik krátkodobé body obnovení na disku pro rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="5973c-163">It’s a good practice toohave a few short-term recovery points on disk, for quick recovery.</span></span> <span data-ttu-id="5973c-164">Tyto body obnovení se používají pro "provozní obnovení".</span><span class="sxs-lookup"><span data-stu-id="5973c-164">These recovery points are used for “operational recovery".</span></span> <span data-ttu-id="5973c-165">Azure slouží jako dobrý mimo pracoviště s vyšší SLA a záruku dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="5973c-165">Azure serves as a good offsite location with higher SLAs and guaranteed availability.</span></span>
    >
    >

    <span data-ttu-id="5973c-166">**Nejvhodnější**: Ujistěte se, že zálohy Azure jsou naplánovány po dokončení hello místního disku zálohování pomocí aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="5973c-166">**Best Practice**: Make sure that Azure Backups are scheduled after hello completion of local disk backups using DPM.</span></span> <span data-ttu-id="5973c-167">To umožňuje hello tooAzure zálohování toobe zkopírovat nejnovější disku.</span><span class="sxs-lookup"><span data-stu-id="5973c-167">This enables hello latest disk backup toobe copied tooAzure.</span></span>

13. <span data-ttu-id="5973c-168">Zvolte plán zásad uchovávání informací hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-168">Choose hello retention policy schedule.</span></span> <span data-ttu-id="5973c-169">Hello informace o tom, jak funguje hello zásady uchovávání informací jsou k dispozici v [pomocí Azure Backup tooreplace váš článek infrastruktury pásky](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="5973c-169">hello details on how hello retention policy works are provided at [Use Azure Backup tooreplace your tape infrastructure article](backup-azure-backup-cloud-as-tape.md).</span></span>

    ![Zásady uchovávání informací](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    <span data-ttu-id="5973c-171">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="5973c-171">In this example:</span></span>

    * <span data-ttu-id="5973c-172">Zálohování se pořídí jednou denně na 12:00 PM a 20: 00 (dolní část úvodní obrazovka) a jsou uchovány na 180 dní.</span><span class="sxs-lookup"><span data-stu-id="5973c-172">Backups are taken once a day at 12:00 PM and 8 PM (bottom part of hello screen) and are retained for 180 days.</span></span>
    * <span data-ttu-id="5973c-173">Hello zálohování na sobotu ve 12:00</span><span class="sxs-lookup"><span data-stu-id="5973c-173">hello backup on Saturday at 12:00 P.M.</span></span> <span data-ttu-id="5973c-174">se zachovává kvůli 104 týdny</span><span class="sxs-lookup"><span data-stu-id="5973c-174">is retained for 104 weeks</span></span>
    * <span data-ttu-id="5973c-175">Hello zálohování na poslední sobotu ve 12:00</span><span class="sxs-lookup"><span data-stu-id="5973c-175">hello backup on Last Saturday at 12:00 P.M.</span></span> <span data-ttu-id="5973c-176">se uchovávají po dobu 60 měsíců</span><span class="sxs-lookup"><span data-stu-id="5973c-176">is retained for 60 months</span></span>
    * <span data-ttu-id="5973c-177">Hello zálohy na poslední sobota dne ve 12:00</span><span class="sxs-lookup"><span data-stu-id="5973c-177">hello backup on Last Saturday of March at 12:00 P.M.</span></span> <span data-ttu-id="5973c-178">se uchovávají 10 let.</span><span class="sxs-lookup"><span data-stu-id="5973c-178">is retained for 10 years</span></span>
14. <span data-ttu-id="5973c-179">Klikněte na tlačítko **Další** a hello vyberte příslušnou možnost pro přenos počáteční kopie zálohy tooAzure hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-179">Click **Next** and select hello appropriate option for transferring hello initial backup copy tooAzure.</span></span> <span data-ttu-id="5973c-180">Můžete zvolit **automaticky přes síť hello** nebo **Offline zálohování**.</span><span class="sxs-lookup"><span data-stu-id="5973c-180">You can choose **Automatically over hello network** or **Offline Backup**.</span></span>

    * <span data-ttu-id="5973c-181">**Automaticky přes síť hello** přenosy hello tooAzure zálohovaná data podle plánu hello zvolené pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="5973c-181">**Automatically over hello network** transfers hello backup data tooAzure as per hello schedule chosen for backup.</span></span>
    * <span data-ttu-id="5973c-182">Jak **Offline zálohování** funguje je vysvětleno v [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="5973c-182">How **Offline Backup** works is explained at [Offline Backup workflow in Azure Backup](backup-azure-backup-import-export.md).</span></span>

    <span data-ttu-id="5973c-183">Vyberte relevantní přenos hello mechanismus toosend hello kopie prvotní zálohy tooAzure a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-183">Choose hello relevant transfer mechanism toosend hello initial backup copy tooAzure and click **Next**.</span></span>
15. <span data-ttu-id="5973c-184">Po prostudování hello podrobnosti zásady v hello **Souhrn** obrazovky, klikněte na hello **vytvořit skupinu** tlačítko toocomplete hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="5973c-184">Once you review hello policy details in hello **Summary** screen, click on hello **Create group** button toocomplete hello workflow.</span></span> <span data-ttu-id="5973c-185">Můžete kliknout na hello **Zavřít** tlačítko a monitorování průběhu úlohy hello v pracovním prostoru monitorování.</span><span class="sxs-lookup"><span data-stu-id="5973c-185">You can click hello **Close** button and monitor hello job progress in Monitoring workspace.</span></span>

    ![Vytvoření skupiny ochrany v průběhu](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a><span data-ttu-id="5973c-187">Zálohování na vyžádání databáze systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="5973c-187">On-demand backup of a SQL Server database</span></span>
<span data-ttu-id="5973c-188">Při předchozí kroky hello vytvořili zásadu zálohování, "bodu obnovení" se vytvoří jenom v případě, že dochází k zálohování první hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-188">While hello previous steps created a backup policy, a “recovery point” is created only when hello first backup occurs.</span></span> <span data-ttu-id="5973c-189">Místo čekání hello tookick plánovače v, bodu hello kroků vytváření aktivační události hello obnovení ručně.</span><span class="sxs-lookup"><span data-stu-id="5973c-189">Rather than waiting for hello scheduler tookick in, hello steps below trigger hello creation of a recovery point manually.</span></span>

1. <span data-ttu-id="5973c-190">Počkejte, až se zobrazí stav skupiny ochrany hello **OK** hello databáze před vytvořením bodu obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-190">Wait until hello protection group status shows **OK** for hello database before creating hello recovery point.</span></span>

    ![Členové skupiny ochrany](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. <span data-ttu-id="5973c-192">Klikněte pravým tlačítkem na hello databáze a vyberte **vytvořit bod obnovení**.</span><span class="sxs-lookup"><span data-stu-id="5973c-192">Right-click on hello database and select **Create Recovery Point**.</span></span>

    ![Vytvořit bod obnovení Online](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. <span data-ttu-id="5973c-194">Zvolte **Online ochrany** hello rozevírací nabídky a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5973c-194">Choose **Online Protection** in hello drop-down menu and click **OK**.</span></span> <span data-ttu-id="5973c-195">Spustí se hello vytvoření bodu obnovení v Azure.</span><span class="sxs-lookup"><span data-stu-id="5973c-195">This starts hello creation of a recovery point in Azure.</span></span>

    ![Vytvoření bodu obnovení](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. <span data-ttu-id="5973c-197">Hello průběh úlohy můžete zobrazit v hello **monitorování** prostoru, kde najdete v průběhu úlohy jako jeden znázorňuje následující obrázek hello hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-197">You can view hello job progress in hello **Monitoring** workspace where you'll find an in progress job like hello one depicted in hello next figure.</span></span>

    ![Konzola monitorování](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a><span data-ttu-id="5973c-199">Obnovení databáze systému SQL Server z Azure</span><span class="sxs-lookup"><span data-stu-id="5973c-199">Recover a SQL Server database from Azure</span></span>
<span data-ttu-id="5973c-200">Hello následující kroky jsou požadované toorecover chráněná entita (databázi systému SQL Server) ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="5973c-200">hello following steps are required toorecover a protected entity (SQL Server database) from Azure.</span></span>

1. <span data-ttu-id="5973c-201">Otevřete konzolu pro správu serveru aplikace DPM hello.</span><span class="sxs-lookup"><span data-stu-id="5973c-201">Open hello DPM server Management Console.</span></span> <span data-ttu-id="5973c-202">Přejděte příliš**obnovení** prostoru, kde můžete sledovat servery hello zálohované aplikací DPM.</span><span class="sxs-lookup"><span data-stu-id="5973c-202">Navigate too**Recovery** workspace where you can see hello servers backed up by DPM.</span></span> <span data-ttu-id="5973c-203">Procházejte hello požadovaná databáze (v této rozlišují ReportServer$ MSDPM2012).</span><span class="sxs-lookup"><span data-stu-id="5973c-203">Browse hello required database (in this case ReportServer$MSDPM2012).</span></span> <span data-ttu-id="5973c-204">Vyberte **obnovení z** čas, který končí **Online**.</span><span class="sxs-lookup"><span data-stu-id="5973c-204">Select a **Recovery from** time which ends with **Online**.</span></span>

    ![Vyberte bod obnovení](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. <span data-ttu-id="5973c-206">Klikněte pravým tlačítkem na název databáze hello a klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="5973c-206">Right-click hello database name and click **Recover**.</span></span>

    ![Obnovení z Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. <span data-ttu-id="5973c-208">Aplikace DPM zobrazí podrobnosti o hello hello bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="5973c-208">DPM shows hello details of hello recovery point.</span></span> <span data-ttu-id="5973c-209">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-209">Click **Next**.</span></span> <span data-ttu-id="5973c-210">toooverwrite hello databáze, typ obnovení vyberte hello **obnovit toooriginal instance systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="5973c-210">toooverwrite hello database, select hello recovery type **Recover toooriginal instance of SQL Server**.</span></span> <span data-ttu-id="5973c-211">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-211">Click **Next**.</span></span>

    ![Obnovit tooOriginal umístění](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    <span data-ttu-id="5973c-213">V tomto příkladu aplikace DPM umožňuje obnovení instance systému SQL Server tooanother hello databáze nebo tooa samostatné síťové složky.</span><span class="sxs-lookup"><span data-stu-id="5973c-213">In this example, DPM allows recovery of hello database tooanother SQL Server instance or tooa standalone network folder.</span></span>
4. <span data-ttu-id="5973c-214">V hello **možnosti obnovení zadejte** obrazovce můžete vybrat možnosti obnovení hello jako toothrottle hello pásma sítě používané při obnovení omezení využití šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="5973c-214">In hello **Specify Recovery options** screen, you can select hello recovery options like Network bandwidth usage throttling toothrottle hello bandwidth used by recovery.</span></span> <span data-ttu-id="5973c-215">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="5973c-215">Click **Next**.</span></span>
5. <span data-ttu-id="5973c-216">V hello **Souhrn** obrazovky, uvidíte všechny konfigurace hello obnovení dosavadní zadat.</span><span class="sxs-lookup"><span data-stu-id="5973c-216">In hello **Summary** screen, you see all hello recovery configurations provided so far.</span></span> <span data-ttu-id="5973c-217">Klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="5973c-217">Click **Recover**.</span></span>

    <span data-ttu-id="5973c-218">Hello stav obnovení zobrazí hello databáze obnovení.</span><span class="sxs-lookup"><span data-stu-id="5973c-218">hello Recovery status shows hello database being recovered.</span></span> <span data-ttu-id="5973c-219">Můžete kliknout na **Zavřít** tooclose hello průvodce a zobrazení hello průběh hello **monitorování** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="5973c-219">You can click **Close** tooclose hello wizard and view hello progress in hello **Monitoring** workspace.</span></span>

    ![Zahájení obnovení](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    <span data-ttu-id="5973c-221">Po dokončení obnovení hello hello obnovit databáze je aplikace, které jsou konzistentní.</span><span class="sxs-lookup"><span data-stu-id="5973c-221">Once hello recovery is completed, hello restored database is application consistent.</span></span>

### <a name="next-steps"></a><span data-ttu-id="5973c-222">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="5973c-222">Next Steps:</span></span>
<span data-ttu-id="5973c-223">• [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)</span><span class="sxs-lookup"><span data-stu-id="5973c-223">•    [Azure Backup FAQ](backup-azure-backup-faq.md)</span></span>
