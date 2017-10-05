---
title: "Vytvoření a obnovení zálohy ve službě BizTalk Services | Microsoft Docs"
description: "BizTalk Services zahrnuje zálohování a obnovení. Zjistěte, jak vytvořit a obnovení zálohy a zjistěte, co se zálohuje. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c55d1ab124441c42101b4ad60924a9ea28231408
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-backup-and-restore"></a><span data-ttu-id="a022d-105">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="a022d-105">BizTalk Services: Backup and Restore</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="a022d-106">Služba Azure BizTalk Services obsahuje funkce zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="a022d-106">Azure BizTalk Services includes Backup and Restore capabilities.</span></span> <span data-ttu-id="a022d-107">Toto téma popisuje postup zálohování a obnovení služby BizTalk Services pomocí portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="a022d-107">This topic describes how to backup and restore BizTalk Services using the Azure classic portal.</span></span>

<span data-ttu-id="a022d-108">Také můžete zálohovat pomocí služby BizTalk Services [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span><span class="sxs-lookup"><span data-stu-id="a022d-108">You can also back up BizTalk Services using the [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span></span> 

> [!NOTE]
> <span data-ttu-id="a022d-109">Hybridní připojení se nebudou zálohovány, bez ohledu na verzi.</span><span class="sxs-lookup"><span data-stu-id="a022d-109">Hybrid Connections are NOT backed up, regardless of the Edition.</span></span> <span data-ttu-id="a022d-110">Hybridní připojení, musíte znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="a022d-110">You must recreate your hybrid connections.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="a022d-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a022d-111">Before you Begin</span></span>
* <span data-ttu-id="a022d-112">Zálohování a obnovení nemusí být dostupné pro všechny edice.</span><span class="sxs-lookup"><span data-stu-id="a022d-112">Backup and Restore may not be available for all editions.</span></span> <span data-ttu-id="a022d-113">V tématu [služby BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="a022d-113">See [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>
* <span data-ttu-id="a022d-114">Pomocí portálu Azure classic, můžete vytvořit zálohu na vyžádání nebo vytvoření naplánovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="a022d-114">Using the Azure classic portal, you can create an On Demand backup or create a scheduled backup.</span></span> 
* <span data-ttu-id="a022d-115">Zálohování obsahu lze obnovit do stejné služby BizTalk nebo novou službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-115">Backup content can be restored to the same BizTalk Service or to a new BizTalk Service.</span></span> <span data-ttu-id="a022d-116">Chcete-li obnovit službu BizTalk pomocí stejného názvu, musíte odstranit stávající službu BizTalk a název musí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a022d-116">To restore the BizTalk Service using the same name, the existing BizTalk Service must be deleted and the name must be available.</span></span> <span data-ttu-id="a022d-117">Po odstranění služby BizTalk, může trvat delší dobu, než chtěli pro stejný název, který se má k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a022d-117">After you delete a BizTalk Service, it can take longer time than wanted for the same name to be available.</span></span> <span data-ttu-id="a022d-118">Pokud nemůžete počkat na stejný název být k dispozici, pak obnovte novou službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-118">If you cannot wait for the same name to be available, then restore to a new BizTalk Service.</span></span>
* <span data-ttu-id="a022d-119">BizTalk Services můžete obnovit do stejné edice, nebo vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="a022d-119">BizTalk Services can be restored to the same edition or a higher edition.</span></span> <span data-ttu-id="a022d-120">Obnovení služby BizTalk Services na nižší verzi, z při vytvoření zálohy, není podporováno.</span><span class="sxs-lookup"><span data-stu-id="a022d-120">Restoring BizTalk Services to a lower edition, from when the backup was taken, is not supported.</span></span>
  
    <span data-ttu-id="a022d-121">Například lze obnovit zálohu pomocí Základní edice na edici Premium.</span><span class="sxs-lookup"><span data-stu-id="a022d-121">For example, a backup using the Basic Edition can be restored to the Premium Edition.</span></span> <span data-ttu-id="a022d-122">Zálohování pomocí edice Premium není možné obnovit do Standard Edition.</span><span class="sxs-lookup"><span data-stu-id="a022d-122">A backup using the Premium Edition cannot be restored to the Standard Edition.</span></span>
* <span data-ttu-id="a022d-123">Ovládací prvek EDI čísla, která jsou zálohovány zachování kontinuity čísel ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="a022d-123">The EDI Control numbers are backed up to maintain continuity of the control numbers.</span></span> <span data-ttu-id="a022d-124">Pokud po poslední záloze zpracování zpráv, obnovení tohoto obsahu zálohování může způsobit duplicitní řízení čísla.</span><span class="sxs-lookup"><span data-stu-id="a022d-124">If messages are processed after the last backup, restoring this backup content can cause duplicate control numbers.</span></span>
* <span data-ttu-id="a022d-125">Pokud dávce má aktivní zprávy, zpracování dávky **před** spuštěním zálohování.</span><span class="sxs-lookup"><span data-stu-id="a022d-125">If a batch has active messages, process the batch **before** running a backup.</span></span> <span data-ttu-id="a022d-126">Při vytváření zálohy (jako potřebné nebo plánované), ukládají nikdy zprávy v dávkách.</span><span class="sxs-lookup"><span data-stu-id="a022d-126">When creating a backup (as needed or scheduled), messages in batches are never stored.</span></span> 
  
    <span data-ttu-id="a022d-127">**Pokud nedojde k zálohování s active zprávy v dávce, tyto zprávy nejsou zálohovány a proto budou ztraceny.**</span><span class="sxs-lookup"><span data-stu-id="a022d-127">**If a backup is taken with active messages in a batch, these messages are not backed up and are therefore lost.**</span></span>
* <span data-ttu-id="a022d-128">Volitelné: Na portálu služby BizTalk zastavte č. všechny operace správy.</span><span class="sxs-lookup"><span data-stu-id="a022d-128">Optional: In the BizTalk Services Portal, stop any management operations.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="a022d-129">Vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="a022d-129">Create a backup</span></span>
<span data-ttu-id="a022d-130">Zálohu můžete provést kdykoli a zcela řídí můžete.</span><span class="sxs-lookup"><span data-stu-id="a022d-130">A backup can be taken at any time and is completely controlled by you.</span></span> <span data-ttu-id="a022d-131">Tato část obsahuje kroky k vytvoření zálohy pomocí portálu Azure classic, včetně:</span><span class="sxs-lookup"><span data-stu-id="a022d-131">This section lists the steps to create backups using the Azure classic portal, including:</span></span>

[<span data-ttu-id="a022d-132">Na vyžádání zálohování</span><span class="sxs-lookup"><span data-stu-id="a022d-132">On Demand backup</span></span>](#backupnow)

[<span data-ttu-id="a022d-133">Plán zálohování</span><span class="sxs-lookup"><span data-stu-id="a022d-133">Schedule a backup</span></span>](#backupschedule)

#### <span data-ttu-id="a022d-134"><a name="backupnow"></a>Na vyžádání zálohování</span><span class="sxs-lookup"><span data-stu-id="a022d-134"><a name="backupnow"></a>On Demand backup</span></span>
1. <span data-ttu-id="a022d-135">Na portálu Azure classic, vyberte **BizTalk Services**a potom vyberte službu BizTalk, které chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="a022d-135">In the Azure classic portal, select **BizTalk Services**, and then select the BizTalk Service you want to backup.</span></span>
2. <span data-ttu-id="a022d-136">V **řídicí panel** vyberte **zálohování** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="a022d-136">In the **Dashboard** tab, select **Back up** at the bottom of the page.</span></span>
3. <span data-ttu-id="a022d-137">Zadejte název zálohy.</span><span class="sxs-lookup"><span data-stu-id="a022d-137">Enter a backup name.</span></span> <span data-ttu-id="a022d-138">Zadejte například *myBizTalkService*BU*datum*.</span><span class="sxs-lookup"><span data-stu-id="a022d-138">For example, enter *myBizTalkService*BU*Date*.</span></span>
4. <span data-ttu-id="a022d-139">Vyberte účet úložiště blob a klepněte na značku zaškrtnutí zahájíte zálohování.</span><span class="sxs-lookup"><span data-stu-id="a022d-139">Choose a blob storage account and select the checkmark to start the backup.</span></span>

<span data-ttu-id="a022d-140">Po dokončení zálohování se vytvoří kontejner s názvem Zálohování, které zadáte v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a022d-140">Once the backup completes, a container with the backup name you enter is created in the storage account.</span></span> <span data-ttu-id="a022d-141">Tento kontejner obsahuje konfiguraci zálohování služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-141">This container contains your BizTalk Service backup configuration.</span></span>

#### <span data-ttu-id="a022d-142"><a name="backupschedule"></a>Plán zálohování</span><span class="sxs-lookup"><span data-stu-id="a022d-142"><a name="backupschedule"></a>Schedule a backup</span></span>
1. <span data-ttu-id="a022d-143">Na portálu Azure classic, vyberte **BizTalk Services**, vyberte název služby BizTalk, které chcete naplánovat zálohování a pak vyberte **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="a022d-143">In the Azure classic portal, select **BizTalk Services**, select the BizTalk Service name you want to schedule the backup, and then select the **Configure** tab.</span></span>
2. <span data-ttu-id="a022d-144">Nastavte **zálohování stav** k **automatické**.</span><span class="sxs-lookup"><span data-stu-id="a022d-144">Set the **Backup Status** to **Automatic**.</span></span> 
3. <span data-ttu-id="a022d-145">Vyberte **účet úložiště** pro uložení zálohy, zadejte **frekvence** vytvářet zálohy a jak dlouho chcete ponechat zálohování (**dní uchovávání**):</span><span class="sxs-lookup"><span data-stu-id="a022d-145">Select the **Storage Account** to store the backup, enter the **Frequency** to create the backups, and how long to keep the backups (**Retention Days**):</span></span>
   
    ![][AutomaticBU]
   
    <span data-ttu-id="a022d-146">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="a022d-146">**Notes**</span></span>     
   
   * <span data-ttu-id="a022d-147">V **dní uchovávání**, doba uchování musí být větší než je četnost záloh.</span><span class="sxs-lookup"><span data-stu-id="a022d-147">In **Retention Days**, the retention period must be greater than the backup frequency.</span></span>
   * <span data-ttu-id="a022d-148">Vyberte **vždy vždycky mějte aspoň jednu zálohu**i v případě, že je po dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="a022d-148">Select **Always keep at least one backup**, even if it is past the retention period.</span></span>
4. <span data-ttu-id="a022d-149">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a022d-149">Select **Save**.</span></span>

<span data-ttu-id="a022d-150">Když spustí naplánované úlohy zálohování, vytvoří kontejner (pro uložení zálohy dat) v účtu úložiště, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="a022d-150">When a scheduled backup job runs, it creates a container (to store backup data) in the storage account you entered.</span></span> <span data-ttu-id="a022d-151">Název kontejneru je s názvem *služby BizTalk název data a času*.</span><span class="sxs-lookup"><span data-stu-id="a022d-151">The name of the container is named *BizTalk Service Name-date-time*.</span></span> 

<span data-ttu-id="a022d-152">Pokud se zobrazí řídicí panel služby BizTalk **se nezdařilo** stavu:</span><span class="sxs-lookup"><span data-stu-id="a022d-152">If the BizTalk Service dashboard shows a **Failed** status:</span></span>

![Stav poslední naplánované zálohy][BackupStatus] 

<span data-ttu-id="a022d-154">Odkaz otevře protokoly operaci služby pro správu za účelem odstranění.</span><span class="sxs-lookup"><span data-stu-id="a022d-154">The link opens the Management Services Operation Logs to help troubleshoot.</span></span> <span data-ttu-id="a022d-155">V tématu [BizTalk Services: řešení problémů pomocí protokolů operací](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span><span class="sxs-lookup"><span data-stu-id="a022d-155">See [BizTalk Services: Troubleshoot using operation logs](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span></span>

## <a name="restore"></a><span data-ttu-id="a022d-156">Obnovení</span><span class="sxs-lookup"><span data-stu-id="a022d-156">Restore</span></span>
<span data-ttu-id="a022d-157">Můžete obnovit zálohy z portálu Azure classic nebo z [obnovit REST API služby BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span><span class="sxs-lookup"><span data-stu-id="a022d-157">You can restore backups from the Azure classic portal or from the [Restore BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span></span> <span data-ttu-id="a022d-158">Tato část obsahuje kroky k obnovení pomocí portálu classic.</span><span class="sxs-lookup"><span data-stu-id="a022d-158">This section lists the steps to restore using the classic portal.</span></span>

#### <a name="before-restoring-a-backup"></a><span data-ttu-id="a022d-159">Před obnovením zálohy</span><span class="sxs-lookup"><span data-stu-id="a022d-159">Before restoring a backup</span></span>
* <span data-ttu-id="a022d-160">Nové sledování, archivace a monitorování úložiště lze zadat během obnovování služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-160">New tracking, archiving, and monitoring stores can be entered while restoring a BizTalk Service.</span></span>
* <span data-ttu-id="a022d-161">Je obnoven stejná data EDI Runtime.</span><span class="sxs-lookup"><span data-stu-id="a022d-161">The same EDI Runtime data is restored.</span></span> <span data-ttu-id="a022d-162">Zálohování EDI Runtime ukládá čísla ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="a022d-162">The EDI Runtime backup stores the control numbers.</span></span> <span data-ttu-id="a022d-163">Obnovené řízení čísla, která jsou v pořadí od okamžiku zálohy.</span><span class="sxs-lookup"><span data-stu-id="a022d-163">The restored control numbers are in sequence from the time of the backup.</span></span> <span data-ttu-id="a022d-164">Pokud po poslední záloze zpracování zpráv, obnovení tohoto obsahu zálohování může způsobit duplicitní řízení čísla.</span><span class="sxs-lookup"><span data-stu-id="a022d-164">If messages are processed after the last backup, restoring this backup content can cause duplicate control numbers.</span></span>

#### <a name="restore-a-backup"></a><span data-ttu-id="a022d-165">Obnovení zálohy</span><span class="sxs-lookup"><span data-stu-id="a022d-165">Restore a backup</span></span>
1. <span data-ttu-id="a022d-166">Na portálu Azure classic, vyberte **nový** > **App Services** > **služby BizTalk** > **obnovení**:</span><span class="sxs-lookup"><span data-stu-id="a022d-166">In the Azure classic portal, select **New** > **App Services** > **BizTalk Service** > **Restore**:</span></span>
   
    ![Obnovení zálohy][Restore]
2. <span data-ttu-id="a022d-168">V **zálohování URL**vyberte ikonu složky a rozbalte účtu úložiště Azure, která ukládá zálohu konfigurace s názvem služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-168">In **Backup URL**, select the folder icon and expand the Azure storage account that stores the BizTalk Service configuration backup.</span></span> <span data-ttu-id="a022d-169">Rozbalte kontejner a v pravém podokně, vyberte odpovídající záložní soubor .txt.</span><span class="sxs-lookup"><span data-stu-id="a022d-169">Expand the container and in the right pane, select the corresponding back up .txt file.</span></span> 
   <br/><br/>
   <span data-ttu-id="a022d-170">Vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="a022d-170">Select **Open**.</span></span>
3. <span data-ttu-id="a022d-171">Na **obnovení služby BizTalk** zadejte **název služby BizTalk** a ověřte **adresa URL domény**, **edice**, a **oblast** obnovené služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-171">On the **Restore BizTalk Service** page, enter a **BizTalk Service Name** and verify the **Domain URL**, **Edition**, and **Region** for the restored BizTalk Service.</span></span> <span data-ttu-id="a022d-172">**Vytvoření nové instance databáze SQL** pro databázi sledování:</span><span class="sxs-lookup"><span data-stu-id="a022d-172">**Create a new SQL database instance** for the tracking database:</span></span>
   
    ![][RestoreBizTalkService]
   
    <span data-ttu-id="a022d-173">Vyberte šipku Další.</span><span class="sxs-lookup"><span data-stu-id="a022d-173">Select the next arrow.</span></span>
4. <span data-ttu-id="a022d-174">Ověřte název databáze SQL, zadejte fyzický server, kde bude vytvořena databáze SQL a uživatelské jméno a heslo pro tento server.</span><span class="sxs-lookup"><span data-stu-id="a022d-174">Verify the name of the SQL database, enter the physical server where the SQL database will be created, and a username/password for that server.</span></span>

    <span data-ttu-id="a022d-175">Pokud chcete nakonfigurovat edice databáze SQL, velikost a další vlastnosti, vyberte **nakonfigurovat rozšířené nastavení databáze**.</span><span class="sxs-lookup"><span data-stu-id="a022d-175">If you want to configure the SQL database edition, size, and other properties, select  **Configure Advanced Database Settings**.</span></span> 

    <span data-ttu-id="a022d-176">Vyberte šipku Další.</span><span class="sxs-lookup"><span data-stu-id="a022d-176">Select the next arrow.</span></span>

1. <span data-ttu-id="a022d-177">Vytvořit nový účet úložiště nebo zadejte existující účet úložiště pro službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-177">Create a new storage account or enter an existing storage account for the BizTalk Service.</span></span>
2. <span data-ttu-id="a022d-178">Vyberte na značku zaškrtnutí zahájíte obnovení.</span><span class="sxs-lookup"><span data-stu-id="a022d-178">Select the checkmark to start the restore.</span></span>

<span data-ttu-id="a022d-179">Po úspěšném dokončení obnovení novou službu BizTalk je uvedena v pozastaveném stavu na stránce služby BizTalk Services na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="a022d-179">Once the restoration successfully completes, a new BizTalk Service is listed in a suspended state on the BizTalk Services page in the Azure classic portal.</span></span>

### <span data-ttu-id="a022d-180"><a name="postrestore"></a>Po obnovení ze zálohy</span><span class="sxs-lookup"><span data-stu-id="a022d-180"><a name="postrestore"></a>After restoring a backup</span></span>
<span data-ttu-id="a022d-181">Služba BizTalk je vždy obnovit **pozastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="a022d-181">The BizTalk Service is always restored in a **Suspended** state.</span></span> <span data-ttu-id="a022d-182">V tomto stavu můžete provést změny konfigurace před nové prostředí je funkční, včetně:</span><span class="sxs-lookup"><span data-stu-id="a022d-182">In this state, you can make any configuration changes before the new environment is functional, including:</span></span>

* <span data-ttu-id="a022d-183">Pokud jste vytvořili pomocí sady SDK Azure BizTalk Services aplikace služby BizTalk, budete muset aktualizovat přihlašovací údaje řízení přístupu (ACS) v těchto aplikacích pro práci s prostředím obnovený.</span><span class="sxs-lookup"><span data-stu-id="a022d-183">If you created BizTalk Service applications using the Azure BizTalk Services SDK, you may need to to update the Access Control (ACS) credentials in those applications to work with the restored environment.</span></span>
* <span data-ttu-id="a022d-184">Obnovení služby BizTalk k replikaci stávajícího prostředí služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-184">You restore a BizTalk Service to replicate an existing BizTalk Service environment.</span></span> <span data-ttu-id="a022d-185">V takovém případě-li nakonfigurovat na portálu služby BizTalk Services původní dohody, které používají zdrojové složky FTP, musíte aktualizovat smlouvy v nově obnovený prostředí použít jiné zdrojové složky FTP.</span><span class="sxs-lookup"><span data-stu-id="a022d-185">In this situation, if there are agreements configured in the original BizTalk Services portal that use a source FTP folder, you may need to update the agreements in the newly restored environment to use a different source FTP folder.</span></span> <span data-ttu-id="a022d-186">Jinak může být dva různé smlouvy pokusu stejnou zprávu pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="a022d-186">Otherwise, there may be two different agreements trying to pull the same message.</span></span>
* <span data-ttu-id="a022d-187">Pokud jste obnovili tak, aby měl prostředí s více služby BizTalk, zkontrolujte, zda že cílíte správné prostředí aplikace Visual Studio, rutiny prostředí PowerShell, rozhraní REST API nebo Trading Partner OM rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="a022d-187">If you restored to have multiple BizTalk Service environments, make sure you target the correct environment in the Visual Studio applications, PowerShell cmdlets, REST APIs, or Trading Partner Management OM APIs.</span></span>
* <span data-ttu-id="a022d-188">Je vhodné nakonfigurovat automatické zálohování na nově obnovený prostředí služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="a022d-188">It's a good practice to configure automated backups on the newly restored BizTalk Service environment.</span></span>

<span data-ttu-id="a022d-189">Spusťte službu BizTalk v portálu Azure classic, vyberte službu BizTalk obnovené a vyberte **obnovit** na hlavním panelu.</span><span class="sxs-lookup"><span data-stu-id="a022d-189">To start the BizTalk Service in the Azure classic portal, select the restored BizTalk Service and select **Resume** in the task bar.</span></span> 

## <a name="what-gets-backed-up"></a><span data-ttu-id="a022d-190">Co se zálohuje</span><span class="sxs-lookup"><span data-stu-id="a022d-190">What gets backed up</span></span>
<span data-ttu-id="a022d-191">Při vytvoření zálohy, budou zálohovány následující položky:</span><span class="sxs-lookup"><span data-stu-id="a022d-191">When a backup is created, the following items are backed up:</span></span>

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH><span data-ttu-id="a022d-192">Zálohovaných položek</span><span class="sxs-lookup"><span data-stu-id="a022d-192">Items backed up</span></span></TH> 
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="a022d-193">
 <strong>Portál Azure BizTalk Services</strong></span><span class="sxs-lookup"><span data-stu-id="a022d-193">
 <strong>Azure BizTalk Services Portal</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="a022d-194">Konfigurace a prostředí Runtime</span><span class="sxs-lookup"><span data-stu-id="a022d-194">Configuration and Runtime</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="a022d-195">Podrobnosti o partnera a profilu</span><span class="sxs-lookup"><span data-stu-id="a022d-195">Partner and profile details</span></span></li>
<li><span data-ttu-id="a022d-196">Partner smlouvy</span><span class="sxs-lookup"><span data-stu-id="a022d-196">Partner Agreements</span></span></li>
<li><span data-ttu-id="a022d-197">Vlastní sestavení, nasazení</span><span class="sxs-lookup"><span data-stu-id="a022d-197">Custom assemblies deployed</span></span></li>
<li><span data-ttu-id="a022d-198">Mosty nasazení</span><span class="sxs-lookup"><span data-stu-id="a022d-198">Bridges deployed</span></span></li>
<li><span data-ttu-id="a022d-199">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="a022d-199">Certificates</span></span></li>
<li><span data-ttu-id="a022d-200">Transformace nasazení</span><span class="sxs-lookup"><span data-stu-id="a022d-200">Transforms deployed</span></span></li>
<li><span data-ttu-id="a022d-201">Kanály</span><span class="sxs-lookup"><span data-stu-id="a022d-201">Pipelines</span></span></li>
<li><span data-ttu-id="a022d-202">Šablony vytvořili a uložili na portálu služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="a022d-202">Templates created and saved in the BizTalk Services Portal</span></span></li>
<li><span data-ttu-id="a022d-203">X12 mapování ST01 a GS01</span><span class="sxs-lookup"><span data-stu-id="a022d-203">X12 ST01 and GS01 mappings</span></span></li>
<li><span data-ttu-id="a022d-204">Ovládací prvek čísla (EDI)</span><span class="sxs-lookup"><span data-stu-id="a022d-204">Control numbers (EDI)</span></span></li>
<li><span data-ttu-id="a022d-205">Hodnoty AS2 zprávy povinná kontrola úrovně Důvěryhodnosti</span><span class="sxs-lookup"><span data-stu-id="a022d-205">AS2 Message MIC values</span></span></li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2"><span data-ttu-id="a022d-206">
 <strong>Služba Azure BizTalk</strong></span><span class="sxs-lookup"><span data-stu-id="a022d-206">
 <strong>Azure BizTalk Service</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="a022d-207">Certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="a022d-207">SSL Certificate</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="a022d-208">Data certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="a022d-208">SSL Certificate Data</span></span></li>
<li><span data-ttu-id="a022d-209">Heslo certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="a022d-209">SSL Certificate Password</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td><span data-ttu-id="a022d-210">Nastavení služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="a022d-210">BizTalk Service Settings</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="a022d-211">Počet jednotek škálování</span><span class="sxs-lookup"><span data-stu-id="a022d-211">Scale unit count</span></span></li>
<li><span data-ttu-id="a022d-212">Edice</span><span class="sxs-lookup"><span data-stu-id="a022d-212">Edition</span></span></li>
<li><span data-ttu-id="a022d-213">Verze produktu</span><span class="sxs-lookup"><span data-stu-id="a022d-213">Product Version</span></span></li>
<li><span data-ttu-id="a022d-214">Oblast nebo Datacenter</span><span class="sxs-lookup"><span data-stu-id="a022d-214">Region/Datacenter</span></span></li>
<li><span data-ttu-id="a022d-215">Ovládací prvek Service (ACS) přístup k oboru názvů a klíče</span><span class="sxs-lookup"><span data-stu-id="a022d-215">Access Control Service (ACS) namespace and key</span></span></li>
<li><span data-ttu-id="a022d-216">Sledování připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="a022d-216">Tracking database connection string</span></span></li>
<li><span data-ttu-id="a022d-217">Archivace připojovací řetězce k účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="a022d-217">Archiving Storage account connection string</span></span></li>
<li><span data-ttu-id="a022d-218">Monitorování připojovací řetězce k účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="a022d-218">Monitoring storage account connection string</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="a022d-219">
 <strong>Další položky</strong></span><span class="sxs-lookup"><span data-stu-id="a022d-219">
 <strong>Additional Items</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="a022d-220">Sledování databáze</span><span class="sxs-lookup"><span data-stu-id="a022d-220">Tracking Database</span></span></td> 
<td><span data-ttu-id="a022d-221">Při vytváření služby BizTalk jsou zadat podrobnosti sledování databáze, včetně serveru Azure SQL Database a název databáze sledování.</span><span class="sxs-lookup"><span data-stu-id="a022d-221">When the BizTalk Service is created, the Tracking Database details are entered, including the Azure SQL Database Server and the Tracking Database name.</span></span> <span data-ttu-id="a022d-222">Sledování databázi není automaticky zálohovat.</span><span class="sxs-lookup"><span data-stu-id="a022d-222">The Tracking Database is not automatically backed up.</span></span>
<br/><br/><span data-ttu-id="a022d-223">
<strong>Důležité upozornění</strong></span><span class="sxs-lookup"><span data-stu-id="a022d-223">
<strong>Important</strong></span></span><br/>
<span data-ttu-id="a022d-224">Pokud se odstraní databázi sledování a potřeby databáze obnovena, musí existovat předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="a022d-224">If the Tracking Database is deleted and the database needs recovered, a previous backup must exist.</span></span> <span data-ttu-id="a022d-225">Pokud zálohu neexistuje, databázi sledování a jeho data nejsou použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="a022d-225">If a backup does not exist, the Tracking Database and its data are not recoverable.</span></span> <span data-ttu-id="a022d-226">V této situaci se vytvořte novou databázi pro sledování se stejným názvem databáze.</span><span class="sxs-lookup"><span data-stu-id="a022d-226">In this situation, create a new Tracking Database with the same database name.</span></span> <span data-ttu-id="a022d-227">Doporučuje se geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="a022d-227">Geo-Replication is recommended.</span></span></td>
</tr> 
</table>

## <a name="next"></a><span data-ttu-id="a022d-228">Další</span><span class="sxs-lookup"><span data-stu-id="a022d-228">Next</span></span>
<span data-ttu-id="a022d-229">Vytvoření služby Azure BizTalk Services na portálu Azure classic, přejděte na [BizTalk Services: zřízení Azure pomocí portálu classic](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span><span class="sxs-lookup"><span data-stu-id="a022d-229">To create Azure BizTalk Services in the Azure classic portal, go to [BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span></span> <span data-ttu-id="a022d-230">Pokud chcete začít vytvářet aplikace, přejděte na článek [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span><span class="sxs-lookup"><span data-stu-id="a022d-230">To start creating applications, go to [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="a022d-231">Viz také</span><span class="sxs-lookup"><span data-stu-id="a022d-231">See Also</span></span>
* [<span data-ttu-id="a022d-232">Zálohování služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="a022d-232">Backup BizTalk Service</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [<span data-ttu-id="a022d-233">Služba BizTalk obnovit ze zálohy</span><span class="sxs-lookup"><span data-stu-id="a022d-233">Restore BizTalk Service from Backup</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [<span data-ttu-id="a022d-234">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="a022d-234">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [<span data-ttu-id="a022d-235">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="a022d-235">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [<span data-ttu-id="a022d-236">BizTalk Services: Tabulka stavů zřízení</span><span class="sxs-lookup"><span data-stu-id="a022d-236">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [<span data-ttu-id="a022d-237">BizTalk Services: Karty Řídicí panel, Sledování a Škálování</span><span class="sxs-lookup"><span data-stu-id="a022d-237">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [<span data-ttu-id="a022d-238">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="a022d-238">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [<span data-ttu-id="a022d-239">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="a022d-239">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [<span data-ttu-id="a022d-240">Jak začít používat sadu SDK Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="a022d-240">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

