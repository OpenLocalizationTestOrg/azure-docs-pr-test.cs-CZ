---
title: "Zálohování serveru Exchange server Azure Backup pomocí serveru Azure Backup | Microsoft Docs"
description: "Zjistěte, jak pro zálohování serveru Exchange server do Azure Backup pomocí serveru Azure Backup"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 60b784fd00013c2b9504f8635c6b5c4c592563be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-azure-backup-server"></a><span data-ttu-id="2b47e-103">Zálohování serveru Exchange server Azure Backup pomocí serveru Azure Backup</span><span class="sxs-lookup"><span data-stu-id="2b47e-103">Back up an Exchange server to Azure Backup with Azure Backup Server</span></span>
<span data-ttu-id="2b47e-104">Tento článek popisuje, jak nakonfigurovat Microsoft Azure Backup Server (MABS) na server Microsoft Exchange zálohovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="2b47e-104">This article describes how to configure Microsoft Azure Backup Server (MABS) to back up a Microsoft Exchange server to Azure.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="2b47e-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2b47e-105">Prerequisites</span></span>
<span data-ttu-id="2b47e-106">Než budete pokračovat, ujistěte se, zda je Azure Backup Server [nainstalované a připravené](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="2b47e-106">Before you continue, make sure that Azure Backup Server is [installed and prepared](backup-azure-microsoft-azure-backup.md).</span></span>

## <a name="mabs-protection-agent"></a><span data-ttu-id="2b47e-107">Agent ochrany MABS</span><span class="sxs-lookup"><span data-stu-id="2b47e-107">MABS protection agent</span></span>
<span data-ttu-id="2b47e-108">Chcete-li nainstalovat agenta ochrany MABS na serveru Exchange, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2b47e-108">To install the MABS protection agent on the Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="2b47e-109">Ujistěte se, zda jsou správně nakonfigurovány bránu firewall.</span><span class="sxs-lookup"><span data-stu-id="2b47e-109">Make sure that the firewalls are correctly configured.</span></span> <span data-ttu-id="2b47e-110">V tématu [konfigurace výjimek brány firewall pro agenta](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="2b47e-110">See [Configure firewall exceptions for the agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="2b47e-111">Nainstalujte agenta na serveru Exchange kliknutím **správy > agenti > nainstalujte** v konzole pro správu MABS.</span><span class="sxs-lookup"><span data-stu-id="2b47e-111">Install the agent on the Exchange server by clicking **Management > Agents > Install** in MABS Administrator Console.</span></span> <span data-ttu-id="2b47e-112">V tématu [nainstalovat agenta ochrany MABS](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="2b47e-112">See [Install the MABS protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-the-exchange-server"></a><span data-ttu-id="2b47e-113">Vytvořte skupinu ochrany pro Exchange server</span><span class="sxs-lookup"><span data-stu-id="2b47e-113">Create a protection group for the Exchange server</span></span>
1. <span data-ttu-id="2b47e-114">V konzole správce MABS, klikněte na tlačítko **ochrany**a potom klikněte na **nový** na pásu karet otevřete **vytvořením nové skupiny ochrany** průvodce.</span><span class="sxs-lookup"><span data-stu-id="2b47e-114">In the MABS Administrator Console, click **Protection**, and then click **New** on the tool ribbon to open the **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="2b47e-115">Na **úvodní** obrazovce průvodce, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-115">On the **Welcome** screen of the wizard click **Next**.</span></span>
3. <span data-ttu-id="2b47e-116">Na **vybrat typ skupiny ochrany** vyberte **servery** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-116">On the **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="2b47e-117">Vyberte databázi serveru Exchange, který chcete chránit a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-117">Select the Exchange server database that you want to protect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2b47e-118">Pokud chráníte Exchange 2013, podívejte se [požadavky serveru Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="2b47e-118">If you are protecting Exchange 2013, check the [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="2b47e-119">V následujícím příkladu je vybraná databáze Exchange 2010.</span><span class="sxs-lookup"><span data-stu-id="2b47e-119">In the following example, the Exchange 2010 database is selected.</span></span>

    ![Vybrat členy skupiny](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="2b47e-121">Vyberte způsob ochrany dat.</span><span class="sxs-lookup"><span data-stu-id="2b47e-121">Select the data protection method.</span></span>

    <span data-ttu-id="2b47e-122">Název skupiny ochrany a poté vyberte možnost z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="2b47e-122">Name the protection group, and then select both of the following options:</span></span>

   * <span data-ttu-id="2b47e-123">Chci krátkodobou ochranu pomocí disku.</span><span class="sxs-lookup"><span data-stu-id="2b47e-123">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="2b47e-124">Chci online ochranu.</span><span class="sxs-lookup"><span data-stu-id="2b47e-124">I want online protection.</span></span>
6. <span data-ttu-id="2b47e-125">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-125">Click **Next**.</span></span>
7. <span data-ttu-id="2b47e-126">Vyberte **spuštěním programu Eseutil zkontrolujete integritu dat** možnost, pokud chcete provést kontrolu integrity databází systému Exchange Server.</span><span class="sxs-lookup"><span data-stu-id="2b47e-126">Select the **Run Eseutil to check data integrity** option if you want to check the integrity of the Exchange Server databases.</span></span>

    <span data-ttu-id="2b47e-127">Když vyberete tuto možnost, kontrola konzistence zálohovaných souborů se spustí na MABS, aby se zabránilo vstupně-výstupní provoz, který je generovaný systémem **eseutil** příkaz na serveru Exchange.</span><span class="sxs-lookup"><span data-stu-id="2b47e-127">After you select this option, backup consistency checking will be run on MABS to avoid the I/O traffic that’s generated by running the **eseutil** command on the Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2b47e-128">Chcete-li použít tuto možnost, musíte zkopírovat soubory Ese.dll a Eseutil.exe do adresáře C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin na serveru MAB.</span><span class="sxs-lookup"><span data-stu-id="2b47e-128">To use this option, you must copy the Ese.dll and Eseutil.exe files to the C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin directory on the MAB server.</span></span> <span data-ttu-id="2b47e-129">Jinak se aktivuje k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="2b47e-129">Otherwise, the following error is triggered:</span></span>  
   > <span data-ttu-id="2b47e-130">![Chyba eseutil](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="2b47e-130">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="2b47e-131">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-131">Click **Next**.</span></span>
9. <span data-ttu-id="2b47e-132">Vyberte databázi pro **zálohování kopírováním**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-132">Select the database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2b47e-133">Pokud nevyberete "Úplného zálohování" pro alespoň jednu DAG kopii databáze, nebudou zkráceny protokoly.</span><span class="sxs-lookup"><span data-stu-id="2b47e-133">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="2b47e-134">Konfigurace cílů pro **krátkodobé zálohování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-134">Configure the goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="2b47e-135">Zkontrolujte dostupné místo na disku a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-135">Review the available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="2b47e-136">Vyberte dobu, kdy serveru MAB bude vytvoření počáteční replikace a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-136">Select the time at which the MAB Server will create the initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="2b47e-137">Vyberte možnosti kontroly konzistence a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-137">Select the consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="2b47e-138">Vyberte databázi, která chcete zálohovat do Azure a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-138">Choose the database that you want to back up to Azure, and then click **Next**.</span></span> <span data-ttu-id="2b47e-139">Například:</span><span class="sxs-lookup"><span data-stu-id="2b47e-139">For example:</span></span>

    ![Zadejte data pro online ochranu](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="2b47e-141">Definujte plán pro **Azure Backup**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-141">Define the schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="2b47e-142">Například:</span><span class="sxs-lookup"><span data-stu-id="2b47e-142">For example:</span></span>

    ![Zadejte plán online zálohování](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="2b47e-144">Všimněte si, body obnovení Online jsou založené na expresní úplné body obnovení.</span><span class="sxs-lookup"><span data-stu-id="2b47e-144">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="2b47e-145">Po dobu, která je určená pro expresní úplné bod obnovení, proto musí naplánovat bodu obnovení online.</span><span class="sxs-lookup"><span data-stu-id="2b47e-145">Therefore, you must schedule the online recovery point after the time that’s specified for the express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="2b47e-146">Konfigurace zásad uchovávání informací pro **Azure Backup**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-146">Configure the retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="2b47e-147">Zvolte možnost online replikace a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-147">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="2b47e-148">Pokud máte velké databáze, může trvat dlouhou dobu pro prvotní zálohování, který se má vytvořit přes síť.</span><span class="sxs-lookup"><span data-stu-id="2b47e-148">If you have a large database, it could take a long time for the initial backup to be created over the network.</span></span> <span data-ttu-id="2b47e-149">K tomuto problému vyhnout, můžete vytvořit zálohu do offline režimu.</span><span class="sxs-lookup"><span data-stu-id="2b47e-149">To avoid this issue, you can create an offline backup.</span></span>  

    ![Zadejte zásady online uchovávání dat.](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="2b47e-151">Potvrďte nastavení a pak klikněte na tlačítko **vytvořit skupinu**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-151">Confirm the settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="2b47e-152">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-152">Click **Close**.</span></span>

## <a name="recover-the-exchange-database"></a><span data-ttu-id="2b47e-153">Obnovení databáze systému Exchange</span><span class="sxs-lookup"><span data-stu-id="2b47e-153">Recover the Exchange database</span></span>
1. <span data-ttu-id="2b47e-154">Chcete-li obnovit databáze serveru Exchange, klikněte na tlačítko **obnovení** v konzole správce MABS.</span><span class="sxs-lookup"><span data-stu-id="2b47e-154">To recover an Exchange database, click **Recovery** in the MABS Administrator Console.</span></span>
2. <span data-ttu-id="2b47e-155">Vyhledejte databázi serveru Exchange, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="2b47e-155">Locate the Exchange database that you want to recover.</span></span>
3. <span data-ttu-id="2b47e-156">Vyberte bod obnovení online z *čas obnovení* rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="2b47e-156">Select an online recovery point from the *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="2b47e-157">Klikněte na tlačítko **obnovit** spustit **Průvodce obnovením**.</span><span class="sxs-lookup"><span data-stu-id="2b47e-157">Click **Recover** to start the **Recovery Wizard**.</span></span>

<span data-ttu-id="2b47e-158">Pro body obnovení online se pět typů obnovení:</span><span class="sxs-lookup"><span data-stu-id="2b47e-158">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="2b47e-159">**Obnovit na původní umístění serveru Exchange Server:** data budou obnoveny do původního serveru Exchange.</span><span class="sxs-lookup"><span data-stu-id="2b47e-159">**Recover to original Exchange Server location:** The data will be recovered to the original Exchange server.</span></span>
* <span data-ttu-id="2b47e-160">**Obnovit do jiné databáze na serveru Exchange Server:** data budou obnoveny do jiné databáze na jiný server Exchange.</span><span class="sxs-lookup"><span data-stu-id="2b47e-160">**Recover to another database on an Exchange Server:** The data will be recovered to another database on another Exchange server.</span></span>
* <span data-ttu-id="2b47e-161">**Obnovit do databáze obnovení:** data bude obnovena obnovení databáze Exchange (RDB).</span><span class="sxs-lookup"><span data-stu-id="2b47e-161">**Recover to a Recovery Database:** The data will be recovered to an Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="2b47e-162">**Kopírovat do síťové složky:** data budou obnoveny do síťové složky.</span><span class="sxs-lookup"><span data-stu-id="2b47e-162">**Copy to a network folder:** The data will be recovered to a network folder.</span></span>
* <span data-ttu-id="2b47e-163">**Kopírovat na pásku:** Pokud máte knihovnu pásků nebo samostatné páskové jednotky připojené a nakonfigurováno na MABS, vytvoření bodu obnovení se zkopírují na volnou pásku.</span><span class="sxs-lookup"><span data-stu-id="2b47e-163">**Copy to tape:** If you have a tape library or a stand-alone tape drive attached and configured on MABS, the recovery point will be copied to a free tape.</span></span>

    ![Zvolte online replikace](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="2b47e-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b47e-165">Next steps</span></span>
* [<span data-ttu-id="2b47e-166">Azure Backup – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="2b47e-166">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
