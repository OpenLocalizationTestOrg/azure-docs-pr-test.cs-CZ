---
title: "aaaBack nahoru systému Exchange server tooAzure zálohování pomocí System Center 2012 R2 DPM | Microsoft Docs"
description: "Zjistěte, jak tooback nahoru systému Exchange server tooAzure zálohování pomocí System Center 2012 R2 DPM"
services: backup
documentationcenter: 
author: MaanasSaran
manager: NKolli1
editor: 
ms.assetid: 13f32256-888e-416e-a78b-40c2a26a5939
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: masaran;jimpark;delhan;trinadhk;markgal
ms.openlocfilehash: fa99296d095c180333474b6d419ebc5ec727547a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-system-center-2012-r2-dpm"></a><span data-ttu-id="bc178-103">Zálohování systému Exchange server tooAzure zálohování pomocí System Center 2012 R2 DPM</span><span class="sxs-lookup"><span data-stu-id="bc178-103">Back up an Exchange server tooAzure Backup with System Center 2012 R2 DPM</span></span>
<span data-ttu-id="bc178-104">Tento článek popisuje, jak tooconfigure System Center 2012 R2 Data Protection Manager (DPM) tooback serveru Microsoft Exchange Server příliš Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="bc178-104">This article describes how tooconfigure a System Center 2012 R2 Data Protection Manager (DPM) server tooback up a Microsoft Exchange server too Azure Backup.</span></span>  

## <a name="updates"></a><span data-ttu-id="bc178-105">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="bc178-105">Updates</span></span>
<span data-ttu-id="bc178-106">toosuccessfully registrace hello serveru DPM pomocí zálohování Azure, je nutné nainstalovat hello nejnovější kumulativní aktualizace pro System Center 2012 R2 DPM a hello nejnovější verzi hello Azure Backup Agent.</span><span class="sxs-lookup"><span data-stu-id="bc178-106">toosuccessfully register hello DPM server with Azure Backup, you must install hello latest update rollup for System Center 2012 R2 DPM and hello latest version of hello Azure Backup Agent.</span></span> <span data-ttu-id="bc178-107">Získat nejnovější kumulativní aktualizaci hello z hello [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span><span class="sxs-lookup"><span data-stu-id="bc178-107">Get hello latest update rollup from hello [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span></span>

> [!NOTE]
> <span data-ttu-id="bc178-108">Hello příklady v tomto článku je nainstalována verze 2.0.8719.0 hello Azure Backup Agent a kumulativní aktualizací 6 je nainstalovaná na System Center 2012 R2 DPM.</span><span class="sxs-lookup"><span data-stu-id="bc178-108">For hello examples in this article, version 2.0.8719.0 of hello Azure Backup Agent is installed, and Update Rollup 6 is installed on System Center 2012 R2 DPM.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="bc178-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bc178-109">Prerequisites</span></span>
<span data-ttu-id="bc178-110">Než budete pokračovat, ujistěte se, že všechny hello [požadavky](backup-azure-dpm-introduction.md#prerequisites) pro pomocí služby Microsoft Azure Backup byly splněny tooprotect úlohy.</span><span class="sxs-lookup"><span data-stu-id="bc178-110">Before you continue, make sure that all hello [prerequisites](backup-azure-dpm-introduction.md#prerequisites) for using Microsoft Azure Backup tooprotect workloads have been met.</span></span> <span data-ttu-id="bc178-111">Tyto součásti zahrnují hello následující:</span><span class="sxs-lookup"><span data-stu-id="bc178-111">These prerequisites include hello following:</span></span>

* <span data-ttu-id="bc178-112">Trezor záloh na hello Azure lokality byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="bc178-112">A backup vault on hello Azure site has been created.</span></span>
* <span data-ttu-id="bc178-113">Agent a přihlašovací údaje trezoru byly stažené toohello serveru aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="bc178-113">Agent and vault credentials have been downloaded toohello DPM server.</span></span>
* <span data-ttu-id="bc178-114">Hello agent je nainstalován na serveru DPM hello.</span><span class="sxs-lookup"><span data-stu-id="bc178-114">hello agent is installed on hello DPM server.</span></span>
* <span data-ttu-id="bc178-115">přihlašovací údaje trezoru Hello byly použité tooregister hello DPM server.</span><span class="sxs-lookup"><span data-stu-id="bc178-115">hello vault credentials were used tooregister hello DPM server.</span></span>
* <span data-ttu-id="bc178-116">Pokud chráníte Exchange 2016, upgradujte prosím tooDPM 2012 R2 UR9 nebo novější</span><span class="sxs-lookup"><span data-stu-id="bc178-116">If you are protecting Exchange 2016, please upgrade tooDPM 2012 R2 UR9 or later</span></span>

## <a name="dpm-protection-agent"></a><span data-ttu-id="bc178-117">Agent ochrany aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="bc178-117">DPM protection agent</span></span>
<span data-ttu-id="bc178-118">agent ochrany DPM hello tooinstall v hello systému Exchange server, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="bc178-118">tooinstall hello DPM protection agent on hello Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="bc178-119">Ujistěte se, že jsou správně nakonfigurované brány firewall hello.</span><span class="sxs-lookup"><span data-stu-id="bc178-119">Make sure that hello firewalls are correctly configured.</span></span> <span data-ttu-id="bc178-120">V tématu [konfigurace výjimek brány firewall pro agenta hello](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc178-120">See [Configure firewall exceptions for hello agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="bc178-121">Nainstalujte agenta hello na serveru Exchange hello kliknutím **správy > agenty > nainstalujte** v konzoli správce aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="bc178-121">Install hello agent on hello Exchange server by clicking **Management > Agents > Install** in DPM Administrator Console.</span></span> <span data-ttu-id="bc178-122">V tématu [agenta ochrany aplikace DPM nainstalujte hello](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="bc178-122">See [Install hello DPM protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-hello-exchange-server"></a><span data-ttu-id="bc178-123">Vytvořte skupinu ochrany pro hello Exchange server</span><span class="sxs-lookup"><span data-stu-id="bc178-123">Create a protection group for hello Exchange server</span></span>
1. <span data-ttu-id="bc178-124">V konzole správce aplikace DPM hello, klikněte na **ochrany**a potom klikněte na **nový** na hello tooopen pásu karet nástroje hello **vytvořením nové skupiny ochrany** průvodce.</span><span class="sxs-lookup"><span data-stu-id="bc178-124">In hello DPM Administrator Console, click **Protection**, and then click **New** on hello tool ribbon tooopen hello **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="bc178-125">Na hello **úvodní** obrazovky hello průvodce klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-125">On hello **Welcome** screen of hello wizard click **Next**.</span></span>
3. <span data-ttu-id="bc178-126">Na hello **vybrat typ skupiny ochrany** vyberte **servery** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-126">On hello **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="bc178-127">Databáze serveru Exchange vyberte hello má tooprotect a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-127">Select hello Exchange server database that you want tooprotect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bc178-128">Pokud chráníte Exchange 2013, zkontrolujte hello [požadavky serveru Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc178-128">If you are protecting Exchange 2013, check hello [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="bc178-129">V následujícím příkladu hello je vybraná databáze hello Exchange 2010.</span><span class="sxs-lookup"><span data-stu-id="bc178-129">In hello following example, hello Exchange 2010 database is selected.</span></span>

    ![Vybrat členy skupiny](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="bc178-131">Vyberte způsob ochrany dat hello.</span><span class="sxs-lookup"><span data-stu-id="bc178-131">Select hello data protection method.</span></span>

    <span data-ttu-id="bc178-132">Název skupiny ochrany hello a pak vyberte oba hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="bc178-132">Name hello protection group, and then select both of hello following options:</span></span>

   * <span data-ttu-id="bc178-133">Chci krátkodobou ochranu pomocí disku.</span><span class="sxs-lookup"><span data-stu-id="bc178-133">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="bc178-134">Chci online ochranu.</span><span class="sxs-lookup"><span data-stu-id="bc178-134">I want online protection.</span></span>
6. <span data-ttu-id="bc178-135">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-135">Click **Next**.</span></span>
7. <span data-ttu-id="bc178-136">Vyberte hello **integritu dat toocheck spuštěním programu Eseutil** možnost, pokud chcete, aby toocheck hello integritu databáze systému Exchange Server hello.</span><span class="sxs-lookup"><span data-stu-id="bc178-136">Select hello **Run Eseutil toocheck data integrity** option if you want toocheck hello integrity of hello Exchange Server databases.</span></span>

    <span data-ttu-id="bc178-137">Když vyberete tuto možnost, kontrola konzistence zálohovaných souborů se spustí na hello DPM serveru tooavoid hello vstupně-výstupní provoz, který se vygeneruje spuštěním hello **eseutil** příkaz hello systém Exchange Server.</span><span class="sxs-lookup"><span data-stu-id="bc178-137">After you select this option, backup consistency checking will be run on hello DPM server tooavoid hello I/O traffic that’s generated by running hello **eseutil** command on hello Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bc178-138">toouse, tato možnost, musíte zkopírovat hello Ese.dll a Eseutil.exe soubory toohello C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin adresáře na serveru aplikace DPM hello.</span><span class="sxs-lookup"><span data-stu-id="bc178-138">toouse this option, you must copy hello Ese.dll and Eseutil.exe files toohello C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin directory on hello DPM server.</span></span> <span data-ttu-id="bc178-139">Jinak se aktivuje hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="bc178-139">Otherwise, hello following error is triggered:</span></span>  
   > <span data-ttu-id="bc178-140">![Chyba eseutil](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="bc178-140">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="bc178-141">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-141">Click **Next**.</span></span>
9. <span data-ttu-id="bc178-142">Vyberte hello databáze pro **zálohování kopírováním**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-142">Select hello database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bc178-143">Pokud nevyberete "Úplného zálohování" pro alespoň jednu DAG kopii databáze, nebudou zkráceny protokoly.</span><span class="sxs-lookup"><span data-stu-id="bc178-143">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="bc178-144">Konfigurace cíle hello **krátkodobé zálohování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-144">Configure hello goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="bc178-145">Zkontrolujte hello volného místa na disku a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-145">Review hello available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="bc178-146">Vyberte hello dobu, na které hello DPM server bude hello počáteční replikace a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-146">Select hello time at which hello DPM server will create hello initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="bc178-147">Vyberte možnosti kontroly konzistence hello a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-147">Select hello consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="bc178-148">Zvolte hello databáze má tooback až tooAzure a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-148">Choose hello database that you want tooback up tooAzure, and then click **Next**.</span></span> <span data-ttu-id="bc178-149">Například:</span><span class="sxs-lookup"><span data-stu-id="bc178-149">For example:</span></span>

    ![Zadejte data pro online ochranu](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="bc178-151">Definujte plán hello pro **Azure Backup**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-151">Define hello schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="bc178-152">Například:</span><span class="sxs-lookup"><span data-stu-id="bc178-152">For example:</span></span>

    ![Zadejte plán online zálohování](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="bc178-154">Všimněte si, body obnovení Online jsou založené na expresní úplné body obnovení.</span><span class="sxs-lookup"><span data-stu-id="bc178-154">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="bc178-155">Proto musíte naplánovat bodu obnovení online hello po dobu hello, která je určená pro hello expresní úplné obnovení bodu.</span><span class="sxs-lookup"><span data-stu-id="bc178-155">Therefore, you must schedule hello online recovery point after hello time that’s specified for hello express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="bc178-156">Konfigurace zásad uchovávání hello **Azure Backup**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-156">Configure hello retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="bc178-157">Zvolte možnost online replikace a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bc178-157">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="bc178-158">Pokud máte velké databáze, může trvat dlouhou dobu pro počáteční zálohování toobe hello vytvořili přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="bc178-158">If you have a large database, it could take a long time for hello initial backup toobe created over hello network.</span></span> <span data-ttu-id="bc178-159">tooavoid tento problém, můžete vytvořit zálohu do offline režimu.</span><span class="sxs-lookup"><span data-stu-id="bc178-159">tooavoid this issue, you can create an offline backup.</span></span>  

    ![Zadejte zásady online uchovávání dat.](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="bc178-161">Potvrďte nastavení hello a pak klikněte na tlačítko **vytvořit skupinu**.</span><span class="sxs-lookup"><span data-stu-id="bc178-161">Confirm hello settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="bc178-162">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="bc178-162">Click **Close**.</span></span>

## <a name="recover-hello-exchange-database"></a><span data-ttu-id="bc178-163">Obnovení databáze systému Exchange hello</span><span class="sxs-lookup"><span data-stu-id="bc178-163">Recover hello Exchange database</span></span>
1. <span data-ttu-id="bc178-164">Klikněte na tlačítko toorecover databáze serveru Exchange, **obnovení** v hello konzole správce aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="bc178-164">toorecover an Exchange database, click **Recovery** in hello DPM Administrator Console.</span></span>
2. <span data-ttu-id="bc178-165">Vyhledejte hello databáze systému Exchange, které chcete toorecover.</span><span class="sxs-lookup"><span data-stu-id="bc178-165">Locate hello Exchange database that you want toorecover.</span></span>
3. <span data-ttu-id="bc178-166">Vyberte bod obnovení online z hello *čas obnovení* rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="bc178-166">Select an online recovery point from hello *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="bc178-167">Klikněte na tlačítko **obnovit** toostart hello **Průvodce obnovením**.</span><span class="sxs-lookup"><span data-stu-id="bc178-167">Click **Recover** toostart hello **Recovery Wizard**.</span></span>

<span data-ttu-id="bc178-168">Pro body obnovení online se pět typů obnovení:</span><span class="sxs-lookup"><span data-stu-id="bc178-168">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="bc178-169">**Obnovit umístění serveru Exchange toooriginal:** hello data budou obnovené toohello původní systému Exchange server.</span><span class="sxs-lookup"><span data-stu-id="bc178-169">**Recover toooriginal Exchange Server location:** hello data will be recovered toohello original Exchange server.</span></span>
* <span data-ttu-id="bc178-170">**Obnovení databáze tooanother na serveru Exchange Server:** hello data budou tooanother obnovenou databázi na jiném serveru Exchange.</span><span class="sxs-lookup"><span data-stu-id="bc178-170">**Recover tooanother database on an Exchange Server:** hello data will be recovered tooanother database on another Exchange server.</span></span>
* <span data-ttu-id="bc178-171">**Obnovit tooa obnovení databáze:** hello data budou obnovené tooan Exchange obnovení databáze (RDB).</span><span class="sxs-lookup"><span data-stu-id="bc178-171">**Recover tooa Recovery Database:** hello data will be recovered tooan Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="bc178-172">**Kopírování tooa síťové složky:** hello data budou obnovené tooa síťové složky.</span><span class="sxs-lookup"><span data-stu-id="bc178-172">**Copy tooa network folder:** hello data will be recovered tooa network folder.</span></span>
* <span data-ttu-id="bc178-173">**Zkopírujte tootape:** Pokud máte páskovou knihovnu nebo samostatnou páskovou jednotku hello připojené a nakonfigurované na serveru aplikace DPM, hello bod obnovení bude zkopírován tooa volnou pásku.</span><span class="sxs-lookup"><span data-stu-id="bc178-173">**Copy tootape:** If you have a tape library or a stand-alone tape drive attached and configured on hello DPM server, hello recovery point will be copied tooa free tape.</span></span>

    ![Zvolte online replikace](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="bc178-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc178-175">Next steps</span></span>
* [<span data-ttu-id="bc178-176">Azure Backup – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="bc178-176">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
