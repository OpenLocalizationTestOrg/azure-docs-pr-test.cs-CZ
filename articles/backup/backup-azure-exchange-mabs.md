---
title: "aaaBack nahoru systému Exchange server tooAzure zálohování pomocí serveru Azure Backup | Microsoft Docs"
description: "Zjistěte, jak tooback nahoru systému Exchange server tooAzure zálohování pomocí serveru Azure Backup"
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
ms.openlocfilehash: db874161151fc57c5b79c41531e18d577f567f66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-azure-backup-server"></a><span data-ttu-id="299e7-103">Zálohování systému Exchange server tooAzure zálohování pomocí serveru Azure Backup</span><span class="sxs-lookup"><span data-stu-id="299e7-103">Back up an Exchange server tooAzure Backup with Azure Backup Server</span></span>
<span data-ttu-id="299e7-104">Tento článek popisuje, jak Microsoft Azure Backup Server (MABS) tooback tooconfigure až tooAzure Microsoft Exchange server.</span><span class="sxs-lookup"><span data-stu-id="299e7-104">This article describes how tooconfigure Microsoft Azure Backup Server (MABS) tooback up a Microsoft Exchange server tooAzure.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="299e7-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="299e7-105">Prerequisites</span></span>
<span data-ttu-id="299e7-106">Než budete pokračovat, ujistěte se, zda je Azure Backup Server [nainstalované a připravené](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="299e7-106">Before you continue, make sure that Azure Backup Server is [installed and prepared](backup-azure-microsoft-azure-backup.md).</span></span>

## <a name="mabs-protection-agent"></a><span data-ttu-id="299e7-107">Agent ochrany MABS</span><span class="sxs-lookup"><span data-stu-id="299e7-107">MABS protection agent</span></span>
<span data-ttu-id="299e7-108">tooinstall hello MABS agenta ochrany na hello systému Exchange server, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="299e7-108">tooinstall hello MABS protection agent on hello Exchange server, follow these steps:</span></span>

1. <span data-ttu-id="299e7-109">Ujistěte se, že jsou správně nakonfigurované brány firewall hello.</span><span class="sxs-lookup"><span data-stu-id="299e7-109">Make sure that hello firewalls are correctly configured.</span></span> <span data-ttu-id="299e7-110">V tématu [konfigurace výjimek brány firewall pro agenta hello](https://technet.microsoft.com/library/Hh758204.aspx).</span><span class="sxs-lookup"><span data-stu-id="299e7-110">See [Configure firewall exceptions for hello agent](https://technet.microsoft.com/library/Hh758204.aspx).</span></span>
2. <span data-ttu-id="299e7-111">Nainstalujte agenta hello na serveru Exchange hello kliknutím **správy > agenty > nainstalujte** v konzole pro správu MABS.</span><span class="sxs-lookup"><span data-stu-id="299e7-111">Install hello agent on hello Exchange server by clicking **Management > Agents > Install** in MABS Administrator Console.</span></span> <span data-ttu-id="299e7-112">V tématu [agenta ochrany MABS hello instalace](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="299e7-112">See [Install hello MABS protection agent](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) for detailed steps.</span></span>

## <a name="create-a-protection-group-for-hello-exchange-server"></a><span data-ttu-id="299e7-113">Vytvořte skupinu ochrany pro hello Exchange server</span><span class="sxs-lookup"><span data-stu-id="299e7-113">Create a protection group for hello Exchange server</span></span>
1. <span data-ttu-id="299e7-114">V konzole pro správu MABS hello, klikněte na **ochrany**a potom klikněte na **nový** na hello tooopen pásu karet nástroje hello **vytvořením nové skupiny ochrany** průvodce.</span><span class="sxs-lookup"><span data-stu-id="299e7-114">In hello MABS Administrator Console, click **Protection**, and then click **New** on hello tool ribbon tooopen hello **Create New Protection Group** wizard.</span></span>
2. <span data-ttu-id="299e7-115">Na hello **úvodní** obrazovky hello průvodce klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-115">On hello **Welcome** screen of hello wizard click **Next**.</span></span>
3. <span data-ttu-id="299e7-116">Na hello **vybrat typ skupiny ochrany** vyberte **servery** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-116">On hello **Select protection group type** screen, select **Servers** and click **Next**.</span></span>
4. <span data-ttu-id="299e7-117">Databáze serveru Exchange vyberte hello má tooprotect a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-117">Select hello Exchange server database that you want tooprotect and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="299e7-118">Pokud chráníte Exchange 2013, zkontrolujte hello [požadavky serveru Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).</span><span class="sxs-lookup"><span data-stu-id="299e7-118">If you are protecting Exchange 2013, check hello [Exchange 2013 prerequisites](https://technet.microsoft.com/library/dn751029.aspx).</span></span>
   >
   >

    <span data-ttu-id="299e7-119">V následujícím příkladu hello je vybraná databáze hello Exchange 2010.</span><span class="sxs-lookup"><span data-stu-id="299e7-119">In hello following example, hello Exchange 2010 database is selected.</span></span>

    ![Vybrat členy skupiny](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. <span data-ttu-id="299e7-121">Vyberte způsob ochrany dat hello.</span><span class="sxs-lookup"><span data-stu-id="299e7-121">Select hello data protection method.</span></span>

    <span data-ttu-id="299e7-122">Název skupiny ochrany hello a pak vyberte oba hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="299e7-122">Name hello protection group, and then select both of hello following options:</span></span>

   * <span data-ttu-id="299e7-123">Chci krátkodobou ochranu pomocí disku.</span><span class="sxs-lookup"><span data-stu-id="299e7-123">I want short-term protection using Disk.</span></span>
   * <span data-ttu-id="299e7-124">Chci online ochranu.</span><span class="sxs-lookup"><span data-stu-id="299e7-124">I want online protection.</span></span>
6. <span data-ttu-id="299e7-125">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-125">Click **Next**.</span></span>
7. <span data-ttu-id="299e7-126">Vyberte hello **integritu dat toocheck spuštěním programu Eseutil** možnost, pokud chcete, aby toocheck hello integritu databáze systému Exchange Server hello.</span><span class="sxs-lookup"><span data-stu-id="299e7-126">Select hello **Run Eseutil toocheck data integrity** option if you want toocheck hello integrity of hello Exchange Server databases.</span></span>

    <span data-ttu-id="299e7-127">Když vyberete tuto možnost, kontrola konzistence zálohovaných souborů se spustí na MABS tooavoid hello vstupně-výstupní provoz, který je generovaný systémem hello **eseutil** příkaz hello systém Exchange Server.</span><span class="sxs-lookup"><span data-stu-id="299e7-127">After you select this option, backup consistency checking will be run on MABS tooavoid hello I/O traffic that’s generated by running hello **eseutil** command on hello Exchange server.</span></span>

   > [!NOTE]
   > <span data-ttu-id="299e7-128">toouse, tato možnost, musíte zkopírovat hello Ese.dll a Eseutil.exe soubory toohello C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin adresáře na serveru MAB hello.</span><span class="sxs-lookup"><span data-stu-id="299e7-128">toouse this option, you must copy hello Ese.dll and Eseutil.exe files toohello C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin directory on hello MAB server.</span></span> <span data-ttu-id="299e7-129">Jinak se aktivuje hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="299e7-129">Otherwise, hello following error is triggered:</span></span>  
   > <span data-ttu-id="299e7-130">![Chyba eseutil](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span><span class="sxs-lookup"><span data-stu-id="299e7-130">![eseutil error](./media/backup-azure-backup-exchange-server/eseutil-error.png)</span></span>
   >
   >
8. <span data-ttu-id="299e7-131">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-131">Click **Next**.</span></span>
9. <span data-ttu-id="299e7-132">Vyberte hello databáze pro **zálohování kopírováním**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-132">Select hello database for **Copy Backup**, and then click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="299e7-133">Pokud nevyberete "Úplného zálohování" pro alespoň jednu DAG kopii databáze, nebudou zkráceny protokoly.</span><span class="sxs-lookup"><span data-stu-id="299e7-133">If you do not select “Full backup” for at least one DAG copy of a database, logs will not be truncated.</span></span>
   >
   >
10. <span data-ttu-id="299e7-134">Konfigurace cíle hello **krátkodobé zálohování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-134">Configure hello goals for **Short-Term backup**, and then click **Next**.</span></span>
11. <span data-ttu-id="299e7-135">Zkontrolujte hello volného místa na disku a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-135">Review hello available disk space, and then click **Next**.</span></span>
12. <span data-ttu-id="299e7-136">Vyberte hello dobu, na které hello MAB Server vytvoří hello počáteční replikace a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-136">Select hello time at which hello MAB Server will create hello initial replication, and then click **Next**.</span></span>
13. <span data-ttu-id="299e7-137">Vyberte možnosti kontroly konzistence hello a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-137">Select hello consistency check options, and then click **Next**.</span></span>
14. <span data-ttu-id="299e7-138">Zvolte hello databáze má tooback až tooAzure a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-138">Choose hello database that you want tooback up tooAzure, and then click **Next**.</span></span> <span data-ttu-id="299e7-139">Například:</span><span class="sxs-lookup"><span data-stu-id="299e7-139">For example:</span></span>

    ![Zadejte data pro online ochranu](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. <span data-ttu-id="299e7-141">Definujte plán hello pro **Azure Backup**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-141">Define hello schedule for **Azure Backup**, and then click **Next**.</span></span> <span data-ttu-id="299e7-142">Například:</span><span class="sxs-lookup"><span data-stu-id="299e7-142">For example:</span></span>

    ![Zadejte plán online zálohování](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="299e7-144">Všimněte si, body obnovení Online jsou založené na expresní úplné body obnovení.</span><span class="sxs-lookup"><span data-stu-id="299e7-144">Note Online recovery points are based on express full recovery points.</span></span> <span data-ttu-id="299e7-145">Proto musíte naplánovat bodu obnovení online hello po dobu hello, která je určená pro hello expresní úplné obnovení bodu.</span><span class="sxs-lookup"><span data-stu-id="299e7-145">Therefore, you must schedule hello online recovery point after hello time that’s specified for hello express full recovery point.</span></span>
    >
    >
16. <span data-ttu-id="299e7-146">Konfigurace zásad uchovávání hello **Azure Backup**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-146">Configure hello retention policy for **Azure Backup**, and then click **Next**.</span></span>
17. <span data-ttu-id="299e7-147">Zvolte možnost online replikace a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="299e7-147">Choose an online replication option and click **Next**.</span></span>

    <span data-ttu-id="299e7-148">Pokud máte velké databáze, může trvat dlouhou dobu pro počáteční zálohování toobe hello vytvořili přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="299e7-148">If you have a large database, it could take a long time for hello initial backup toobe created over hello network.</span></span> <span data-ttu-id="299e7-149">tooavoid tento problém, můžete vytvořit zálohu do offline režimu.</span><span class="sxs-lookup"><span data-stu-id="299e7-149">tooavoid this issue, you can create an offline backup.</span></span>  

    ![Zadejte zásady online uchovávání dat.](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. <span data-ttu-id="299e7-151">Potvrďte nastavení hello a pak klikněte na tlačítko **vytvořit skupinu**.</span><span class="sxs-lookup"><span data-stu-id="299e7-151">Confirm hello settings, and then click **Create Group**.</span></span>
19. <span data-ttu-id="299e7-152">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="299e7-152">Click **Close**.</span></span>

## <a name="recover-hello-exchange-database"></a><span data-ttu-id="299e7-153">Obnovení databáze systému Exchange hello</span><span class="sxs-lookup"><span data-stu-id="299e7-153">Recover hello Exchange database</span></span>
1. <span data-ttu-id="299e7-154">Klikněte na tlačítko toorecover databáze serveru Exchange, **obnovení** v konzole pro správu MABS hello.</span><span class="sxs-lookup"><span data-stu-id="299e7-154">toorecover an Exchange database, click **Recovery** in hello MABS Administrator Console.</span></span>
2. <span data-ttu-id="299e7-155">Vyhledejte hello databáze systému Exchange, které chcete toorecover.</span><span class="sxs-lookup"><span data-stu-id="299e7-155">Locate hello Exchange database that you want toorecover.</span></span>
3. <span data-ttu-id="299e7-156">Vyberte bod obnovení online z hello *čas obnovení* rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="299e7-156">Select an online recovery point from hello *recovery time* drop-down list.</span></span>
4. <span data-ttu-id="299e7-157">Klikněte na tlačítko **obnovit** toostart hello **Průvodce obnovením**.</span><span class="sxs-lookup"><span data-stu-id="299e7-157">Click **Recover** toostart hello **Recovery Wizard**.</span></span>

<span data-ttu-id="299e7-158">Pro body obnovení online se pět typů obnovení:</span><span class="sxs-lookup"><span data-stu-id="299e7-158">For online recovery points, there are five recovery types:</span></span>

* <span data-ttu-id="299e7-159">**Obnovit umístění serveru Exchange toooriginal:** hello data budou obnovené toohello původní systému Exchange server.</span><span class="sxs-lookup"><span data-stu-id="299e7-159">**Recover toooriginal Exchange Server location:** hello data will be recovered toohello original Exchange server.</span></span>
* <span data-ttu-id="299e7-160">**Obnovení databáze tooanother na serveru Exchange Server:** hello data budou tooanother obnovenou databázi na jiném serveru Exchange.</span><span class="sxs-lookup"><span data-stu-id="299e7-160">**Recover tooanother database on an Exchange Server:** hello data will be recovered tooanother database on another Exchange server.</span></span>
* <span data-ttu-id="299e7-161">**Obnovit tooa obnovení databáze:** hello data budou obnovené tooan Exchange obnovení databáze (RDB).</span><span class="sxs-lookup"><span data-stu-id="299e7-161">**Recover tooa Recovery Database:** hello data will be recovered tooan Exchange Recovery Database (RDB).</span></span>
* <span data-ttu-id="299e7-162">**Kopírování tooa síťové složky:** hello data budou obnovené tooa síťové složky.</span><span class="sxs-lookup"><span data-stu-id="299e7-162">**Copy tooa network folder:** hello data will be recovered tooa network folder.</span></span>
* <span data-ttu-id="299e7-163">**Zkopírujte tootape:** Pokud máte knihovny pásků nebo samostatné páskové jednotky připojené a nakonfigurovaná v MABS, hello bod obnovení bude zkopírován tooa volnou pásku.</span><span class="sxs-lookup"><span data-stu-id="299e7-163">**Copy tootape:** If you have a tape library or a stand-alone tape drive attached and configured on MABS, hello recovery point will be copied tooa free tape.</span></span>

    ![Zvolte online replikace](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a><span data-ttu-id="299e7-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="299e7-165">Next steps</span></span>
* [<span data-ttu-id="299e7-166">Azure Backup – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="299e7-166">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
