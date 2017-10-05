---
title: "Zálohování serveru Exchange server Azure Backup se System Center 2012 R2 DPM | Microsoft Docs"
description: "Zjistěte, jak pro zálohování serveru Exchange server do Azure Backup pomocí System Center 2012 R2 DPM"
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
ms.openlocfilehash: 2a0e416440e55cfde70cbd20d40c99fb29b4229c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a><span data-ttu-id="2020c-103">Zálohování serveru Exchange do služby Azure Backup pomocí nástroje System Center 2012 R2 DPM</span><span class="sxs-lookup"><span data-stu-id="2020c-103">Back up an Exchange server to Azure Backup with System Center 2012 R2 DPM</span></span>
<span data-ttu-id="2020c-104">Tento článek popisuje postup konfigurace serveru System Center 2012 R2 Data Protection Manager (DPM) pro server Microsoft Exchange zálohovat do služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="2020c-104">This article describes how to configure a System Center 2012 R2 Data Protection Manager (DPM) server to back up a Microsoft Exchange server to  Azure Backup.</span></span>  

## <a name="updates"></a><span data-ttu-id="2020c-105">Aktualizace</span><span class="sxs-lookup"><span data-stu-id="2020c-105">Updates</span></span>
<span data-ttu-id="2020c-106">Chcete-li úspěšně registrace serveru DPM pomocí zálohování Azure, musíte nainstalovat nejnovější kumulativní aktualizace pro System Center 2012 R2 DPM a nejnovější verzi Azure Backup Agent.</span><span class="sxs-lookup"><span data-stu-id="2020c-106">To successfully register the DPM server with Azure Backup, you must install the latest update rollup for System Center 2012 R2 DPM and the latest version of the Azure Backup Agent.</span></span> <span data-ttu-id="2020c-107">Získat nejnovější kumulativní aktualizaci z [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span><span class="sxs-lookup"><span data-stu-id="2020c-107">Get the latest update rollup from the [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).</span></span>

> [!NOTE]
> <span data-ttu-id="2020c-108">Příklady v tomto článku je nainstalována verze 2.0.8719.0 Azure Backup Agent a kumulativní aktualizací 6 je nainstalovaná na System Center 2012 R2 DPM.</span><span class="sxs-lookup"><span data-stu-id="2020c-108">For the examples in this article, version 2.0.8719.0 of the Azure Backup Agent is installed, and Update Rollup 6 is installed on System Center 2012 R2 DPM.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="2020c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2020c-109">Prerequisites</span></span>
<span data-ttu-id="2020c-110">Než budete pokračovat, ujistěte se, že všechny [požadavky](backup-azure-dpm-introduction.md#prerequisites) pro použití Microsoft Azure Backup k ochraně byly splněny úlohy.</span><span class="sxs-lookup"><span data-stu-id="2020c-110">Before you continue, make sure that all the [prerequisites](backup-azure-dpm-introduction.md#prerequisites) for using Microsoft Azure Backup to protect workloads have been met.</span></span> <span data-ttu-id="2020c-111">Tyto součásti zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="2020c-111">These prerequisites include the following:</span></span>

* <span data-ttu-id="2020c-112">Byla vytvořena trezor záloh na webu Azure.</span><span class="sxs-lookup"><span data-stu-id="2020c-112">A backup vault on the Azure site has been created.</span></span>
* <span data-ttu-id="2020c-113">K serveru aplikace DPM byly staženy agenta a přihlašovací údaje trezoru.</span><span class="sxs-lookup"><span data-stu-id="2020c-113">Agent and vault credentials have been downloaded to the DPM server.</span></span>
* <span data-ttu-id="2020c-114">Agent byl nainstalován na serveru DPM.</span><span class="sxs-lookup"><span data-stu-id="2020c-114">The agent is installed on the DPM server.</span></span>
* <span data-ttu-id="2020c-115">Přihlašovací údaje trezoru jste použili k registrace serveru DPM.</span><span class="sxs-lookup"><span data-stu-id="2020c-115">The vault credentials were used to register the DPM server.</span></span>
* <span data-ttu-id="2020c-116">Pokud chráníte Exchange 2016, proveďte upgrade na DPM 2012 R2 UR9 nebo novější</span><span class="sxs-lookup"><span data-stu-id="2020c-116">If you are protecting Exchange 2016, please upgrade to DPM 2012 R2 UR9 or later</span></span>

## <a name="dpm-protection-agent"></a><span data-ttu-id="2020c-117">Agent ochrany aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="2020c-117">DPM protection agent</span></span>
<span data-ttu-id="2020c-118">Chcete-li nainstalovat agenta ochrany DPM na serveru Exchange, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2020c-118">To install the DPM protection agent on the Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="2020c-119">Ujistěte se, zda jsou správně nakonfigurovány bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="2020c-119">Make sure that the firewalls are correctly configured.</span></span> <span data-ttu-id="2020c-120">V tématu [konfigurace výjimek brány firewall pro agenta](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="2020c-120">See [Configure firewall exceptions for the agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="2020c-121">Nainstalujte agenta na serveru Exchange kliknutím **správy > agenti > nainstalujte** v konzoli správce aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="2020c-121">Install the agent on the Exchange server by clicking **Management > Agents > Install** in DPM Administrator Console.</span></span> <span data-ttu-id="2020c-122">V tématu [nainstalovat agenta ochrany DPM](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="2020c-122">See [Install the DPM protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-the-exchange-server"></a><span data-ttu-id="2020c-123">Vytvořte skupinu ochrany pro Exchange server</span><span class="sxs-lookup"><span data-stu-id="2020c-123">Create a protection group for the Exchange server</span></span>
1. <span data-ttu-id="2020c-124">V konzole správce aplikace DPM klikněte na **ochrany**a potom klikněte na **nový** na pásu karet otevřete **vytvořením nové skupiny ochrany** průvodce.</span><span class="sxs-lookup"><span data-stu-id="2020c-124">In the DPM Administrator Console, click **Protection**, and then click **New** on the tool ribbon to open the **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="2020c-125">Na **úvodní** obrazovce průvodce, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-125">On the **Welcome** screen of the wizard click **Next**.</span></span>
3. <span data-ttu-id="2020c-126">Na **vybrat typ skupiny ochrany** vyberte **servery** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-126">On the **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="2020c-127">Vyberte databázi serveru Exchange, který chcete chránit a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-127">Select the Exchange server database that you want to protect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2020c-128">Pokud chráníte Exchange 2013, podívejte se [požadavky serveru Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="2020c-128">If you are protecting Exchange 2013, check the [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="2020c-129">V následujícím příkladu je vybraná databáze Exchange 2010.</span><span class="sxs-lookup"><span data-stu-id="2020c-129">In the following example, the Exchange 2010 database is selected.</span></span>

    ![Vybrat členy skupiny](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="2020c-131">Vyberte způsob ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="2020c-131">Select the data protection method.</span></span>

    <span data-ttu-id="2020c-132">Název skupiny ochrany a poté vyberte možnost z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="2020c-132">Name the protection group, and then select both of the following options:</span></span>

   * <span data-ttu-id="2020c-133">Chci krátkodobou ochranu pomocí disku.</span><span class="sxs-lookup"><span data-stu-id="2020c-133">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="2020c-134">Chci online ochranu.</span><span class="sxs-lookup"><span data-stu-id="2020c-134">I want online protection.</span></span>
6. <span data-ttu-id="2020c-135">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-135">Click **Next**.</span></span>
7. <span data-ttu-id="2020c-136">Vyberte **spuštěním programu Eseutil zkontrolujete integritu dat** možnost, pokud chcete provést kontrolu integrity databází systému Exchange Server.</span><span class="sxs-lookup"><span data-stu-id="2020c-136">Select the **Run Eseutil to check data integrity** option if you want to check the integrity of the Exchange Server databases.</span></span>

    <span data-ttu-id="2020c-137">Když vyberete tuto možnost, bude na serveru DPM, aby se zabránilo vstupně-výstupní provoz, který je generovaný systémem spustit kontrolu konzistence zálohy **eseutil** příkaz na serveru Exchange.</span><span class="sxs-lookup"><span data-stu-id="2020c-137">After you select this option, backup consistency checking will be run on the DPM server to avoid the I/O traffic that’s generated by running the **eseutil** command on the Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2020c-138">Chcete-li použít tuto možnost, musíte zkopírovat soubory Ese.dll a Eseutil.exe do adresáře C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin na serveru DPM.</span><span class="sxs-lookup"><span data-stu-id="2020c-138">To use this option, you must copy the Ese.dll and Eseutil.exe files to the C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin directory on the DPM server.</span></span> <span data-ttu-id="2020c-139">Jinak se aktivuje k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="2020c-139">Otherwise, the following error is triggered:</span></span>  
   > <span data-ttu-id="2020c-140">![Chyba eseutil](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="2020c-140">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="2020c-141">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-141">Click **Next**.</span></span>
9. <span data-ttu-id="2020c-142">Vyberte databázi pro **zálohování kopírováním**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-142">Select the database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2020c-143">Pokud nevyberete "Úplného zálohování" pro alespoň jednu DAG kopii databáze, nebudou zkráceny protokoly.</span><span class="sxs-lookup"><span data-stu-id="2020c-143">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="2020c-144">Konfigurace cílů pro **krátkodobé zálohování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-144">Configure the goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="2020c-145">Zkontrolujte dostupné místo na disku a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-145">Review the available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="2020c-146">Vyberte dobu, kdy DPM server bude počáteční replikace a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-146">Select the time at which the DPM server will create the initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="2020c-147">Vyberte možnosti kontroly konzistence a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-147">Select the consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="2020c-148">Vyberte databázi, která chcete zálohovat do Azure a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-148">Choose the database that you want to back up to Azure, and then click **Next**.</span></span> <span data-ttu-id="2020c-149">Například:</span><span class="sxs-lookup"><span data-stu-id="2020c-149">For example:</span></span>

    ![Zadejte data pro online ochranu](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="2020c-151">Definujte plán pro **Azure Backup**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-151">Define the schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="2020c-152">Například:</span><span class="sxs-lookup"><span data-stu-id="2020c-152">For example:</span></span>

    ![Zadejte plán online zálohování](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="2020c-154">Všimněte si, body obnovení Online jsou založené na expresní úplné body obnovení.</span><span class="sxs-lookup"><span data-stu-id="2020c-154">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="2020c-155">Po dobu, která je určená pro expresní úplné bod obnovení, proto musí naplánovat bodu obnovení online.</span><span class="sxs-lookup"><span data-stu-id="2020c-155">Therefore, you must schedule the online recovery point after the time that’s specified for the express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="2020c-156">Konfigurace zásad uchovávání informací pro **Azure Backup**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-156">Configure the retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="2020c-157">Zvolte možnost online replikace a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2020c-157">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="2020c-158">Pokud máte velké databáze, může trvat dlouhou dobu pro prvotní zálohování, který se má vytvořit přes síť.</span><span class="sxs-lookup"><span data-stu-id="2020c-158">If you have a large database, it could take a long time for the initial backup to be created over the network.</span></span> <span data-ttu-id="2020c-159">K tomuto problému vyhnout, můžete vytvořit zálohu do offline režimu.</span><span class="sxs-lookup"><span data-stu-id="2020c-159">To avoid this issue, you can create an offline backup.</span></span>  

    ![Zadejte zásady online uchovávání dat.](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="2020c-161">Potvrďte nastavení a pak klikněte na tlačítko **vytvořit skupinu**.</span><span class="sxs-lookup"><span data-stu-id="2020c-161">Confirm the settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="2020c-162">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="2020c-162">Click **Close**.</span></span>

## <a name="recover-the-exchange-database"></a><span data-ttu-id="2020c-163">Obnovení databáze systému Exchange</span><span class="sxs-lookup"><span data-stu-id="2020c-163">Recover the Exchange database</span></span>
1. <span data-ttu-id="2020c-164">Chcete-li obnovit databáze serveru Exchange, klikněte na tlačítko **obnovení** v konzole správce aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="2020c-164">To recover an Exchange database, click **Recovery** in the DPM Administrator Console.</span></span>
2. <span data-ttu-id="2020c-165">Vyhledejte databázi serveru Exchange, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="2020c-165">Locate the Exchange database that you want to recover.</span></span>
3. <span data-ttu-id="2020c-166">Vyberte bod obnovení online z *čas obnovení* rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="2020c-166">Select an online recovery point from the *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="2020c-167">Klikněte na tlačítko **obnovit** spustit **Průvodce obnovením**.</span><span class="sxs-lookup"><span data-stu-id="2020c-167">Click **Recover** to start the **Recovery Wizard**.</span></span>

<span data-ttu-id="2020c-168">Pro body obnovení online se pět typů obnovení:</span><span class="sxs-lookup"><span data-stu-id="2020c-168">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="2020c-169">**Obnovit na původní umístění serveru Exchange Server:** data budou obnoveny do původního serveru Exchange.</span><span class="sxs-lookup"><span data-stu-id="2020c-169">**Recover to original Exchange Server location:** The data will be recovered to the original Exchange server.</span></span>
* <span data-ttu-id="2020c-170">**Obnovit do jiné databáze na serveru Exchange Server:** data budou obnoveny do jiné databáze na jiný server Exchange.</span><span class="sxs-lookup"><span data-stu-id="2020c-170">**Recover to another database on an Exchange Server:** The data will be recovered to another database on another Exchange server.</span></span>
* <span data-ttu-id="2020c-171">**Obnovit do databáze obnovení:** data bude obnovena obnovení databáze Exchange (RDB).</span><span class="sxs-lookup"><span data-stu-id="2020c-171">**Recover to a Recovery Database:** The data will be recovered to an Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="2020c-172">**Kopírovat do síťové složky:** data budou obnoveny do síťové složky.</span><span class="sxs-lookup"><span data-stu-id="2020c-172">**Copy to a network folder:** The data will be recovered to a network folder.</span></span>
* <span data-ttu-id="2020c-173">**Kopírovat na pásku:** Pokud máte knihovnu pásků nebo samostatné páskové jednotky připojené a nakonfigurováno na serveru aplikace DPM, vytvoření bodu obnovení se zkopírují na volnou pásku.</span><span class="sxs-lookup"><span data-stu-id="2020c-173">**Copy to tape:** If you have a tape library or a stand-alone tape drive attached and configured on the DPM server, the recovery point will be copied to a free tape.</span></span>

    ![Zvolte online replikace](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="2020c-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2020c-175">Next steps</span></span>
* [<span data-ttu-id="2020c-176">Azure Backup – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2020c-176">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
