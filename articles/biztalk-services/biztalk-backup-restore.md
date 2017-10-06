---
title: "aaaCreate a obnovit zálohu ve službě BizTalk Services | Microsoft Docs"
description: "BizTalk Services zahrnuje zálohování a obnovení. Zjistěte, jak toocreate a obnovení zálohy a zjistit, co se zálohuje. MABS, WABS"
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
ms.openlocfilehash: 32356ad870678fa5fd5bbbbf13d9377188f770a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-backup-and-restore"></a><span data-ttu-id="4a500-105">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="4a500-105">BizTalk Services: Backup and Restore</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="4a500-106">Služba Azure BizTalk Services obsahuje funkce zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="4a500-106">Azure BizTalk Services includes Backup and Restore capabilities.</span></span> <span data-ttu-id="4a500-107">Toto téma popisuje, jak hello toobackup a obnovení služby BizTalk Services pomocí portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="4a500-107">This topic describes how toobackup and restore BizTalk Services using hello Azure classic portal.</span></span>

<span data-ttu-id="4a500-108">Můžete také zálohovat služby BizTalk Services pomocí hello [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span><span class="sxs-lookup"><span data-stu-id="4a500-108">You can also back up BizTalk Services using hello [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span></span> 

> [!NOTE]
> <span data-ttu-id="4a500-109">Hybridní připojení se nebudou zálohovány, bez ohledu na to hello Edition.</span><span class="sxs-lookup"><span data-stu-id="4a500-109">Hybrid Connections are NOT backed up, regardless of hello Edition.</span></span> <span data-ttu-id="4a500-110">Hybridní připojení, musíte znovu vytvořit.</span><span class="sxs-lookup"><span data-stu-id="4a500-110">You must recreate your hybrid connections.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="4a500-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4a500-111">Before you Begin</span></span>
* <span data-ttu-id="4a500-112">Zálohování a obnovení nemusí být dostupné pro všechny edice.</span><span class="sxs-lookup"><span data-stu-id="4a500-112">Backup and Restore may not be available for all editions.</span></span> <span data-ttu-id="4a500-113">V tématu [služby BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="4a500-113">See [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>
* <span data-ttu-id="4a500-114">Pomocí hello portál Azure classic, můžete vytvořit zálohu na vyžádání nebo vytvoření naplánovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="4a500-114">Using hello Azure classic portal, you can create an On Demand backup or create a scheduled backup.</span></span> 
* <span data-ttu-id="4a500-115">Zálohování obsah může být obnovené toohello stejné služby BizTalk nebo tooa novou službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-115">Backup content can be restored toohello same BizTalk Service or tooa new BizTalk Service.</span></span> <span data-ttu-id="4a500-116">toorestore hello služby BizTalk pomocí hello stejný název, hello stávající službu BizTalk musí být odstraněny a hello název musí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4a500-116">toorestore hello BizTalk Service using hello same name, hello existing BizTalk Service must be deleted and hello name must be available.</span></span> <span data-ttu-id="4a500-117">Po odstranění služby BizTalk, může trvat delší dobu, než chtěli pro hello stejný název toobe k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4a500-117">After you delete a BizTalk Service, it can take longer time than wanted for hello same name toobe available.</span></span> <span data-ttu-id="4a500-118">Pokud nemůžete počkat hello stejný název toobe k dispozici, pak obnovit tooa novou službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-118">If you cannot wait for hello same name toobe available, then restore tooa new BizTalk Service.</span></span>
* <span data-ttu-id="4a500-119">BizTalk Services může být obnovené toohello stejné edice, nebo vyšší verze.</span><span class="sxs-lookup"><span data-stu-id="4a500-119">BizTalk Services can be restored toohello same edition or a higher edition.</span></span> <span data-ttu-id="4a500-120">Obnovení služby BizTalk Services tooa nižší verzi, z při hello zálohy, není podporováno.</span><span class="sxs-lookup"><span data-stu-id="4a500-120">Restoring BizTalk Services tooa lower edition, from when hello backup was taken, is not supported.</span></span>
  
    <span data-ttu-id="4a500-121">Například zálohy pomocí hello základní verzi může být obnovena toohello edici Premium.</span><span class="sxs-lookup"><span data-stu-id="4a500-121">For example, a backup using hello Basic Edition can be restored toohello Premium Edition.</span></span> <span data-ttu-id="4a500-122">Zálohování pomocí hello edice Premium nemůže být obnovena toohello Standard Edition.</span><span class="sxs-lookup"><span data-stu-id="4a500-122">A backup using hello Premium Edition cannot be restored toohello Standard Edition.</span></span>
* <span data-ttu-id="4a500-123">čísla EDI řízení Hello jsou zálohovány toomaintain kontinuity hello řízení čísel.</span><span class="sxs-lookup"><span data-stu-id="4a500-123">hello EDI Control numbers are backed up toomaintain continuity of hello control numbers.</span></span> <span data-ttu-id="4a500-124">Pokud po poslední záloze hello zpracování zpráv, obnovení tohoto obsahu zálohování může způsobit duplicitní řízení čísla.</span><span class="sxs-lookup"><span data-stu-id="4a500-124">If messages are processed after hello last backup, restoring this backup content can cause duplicate control numbers.</span></span>
* <span data-ttu-id="4a500-125">Pokud dávce má aktivní zprávy, zpracování dávky hello **před** spuštěním zálohování.</span><span class="sxs-lookup"><span data-stu-id="4a500-125">If a batch has active messages, process hello batch **before** running a backup.</span></span> <span data-ttu-id="4a500-126">Při vytváření zálohy (jako potřebné nebo plánované), ukládají nikdy zprávy v dávkách.</span><span class="sxs-lookup"><span data-stu-id="4a500-126">When creating a backup (as needed or scheduled), messages in batches are never stored.</span></span> 
  
    <span data-ttu-id="4a500-127">**Pokud nedojde k zálohování s active zprávy v dávce, tyto zprávy nejsou zálohovány a proto budou ztraceny.**</span><span class="sxs-lookup"><span data-stu-id="4a500-127">**If a backup is taken with active messages in a batch, these messages are not backed up and are therefore lost.**</span></span>
* <span data-ttu-id="4a500-128">Volitelné: V hello portál služby BizTalk, zastavte č. všechny operace správy.</span><span class="sxs-lookup"><span data-stu-id="4a500-128">Optional: In hello BizTalk Services Portal, stop any management operations.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="4a500-129">Vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="4a500-129">Create a backup</span></span>
<span data-ttu-id="4a500-130">Zálohu můžete provést kdykoli a zcela řídí můžete.</span><span class="sxs-lookup"><span data-stu-id="4a500-130">A backup can be taken at any time and is completely controlled by you.</span></span> <span data-ttu-id="4a500-131">Tato část uvádí hello kroky toocreate zálohování pomocí hello Azure classic portálu, včetně:</span><span class="sxs-lookup"><span data-stu-id="4a500-131">This section lists hello steps toocreate backups using hello Azure classic portal, including:</span></span>

[<span data-ttu-id="4a500-132">Na vyžádání zálohování</span><span class="sxs-lookup"><span data-stu-id="4a500-132">On Demand backup</span></span>](#backupnow)

[<span data-ttu-id="4a500-133">Plán zálohování</span><span class="sxs-lookup"><span data-stu-id="4a500-133">Schedule a backup</span></span>](#backupschedule)

#### <span data-ttu-id="4a500-134"><a name="backupnow"></a>Na vyžádání zálohování</span><span class="sxs-lookup"><span data-stu-id="4a500-134"><a name="backupnow"></a>On Demand backup</span></span>
1. <span data-ttu-id="4a500-135">V hello portál Azure classic, vyberte **BizTalk Services**, a pak vyberte hello chcete toobackup služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-135">In hello Azure classic portal, select **BizTalk Services**, and then select hello BizTalk Service you want toobackup.</span></span>
2. <span data-ttu-id="4a500-136">V hello **řídicí panel** vyberte **zálohování** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4a500-136">In hello **Dashboard** tab, select **Back up** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="4a500-137">Zadejte název zálohy.</span><span class="sxs-lookup"><span data-stu-id="4a500-137">Enter a backup name.</span></span> <span data-ttu-id="4a500-138">Zadejte například *myBizTalkService*BU*datum*.</span><span class="sxs-lookup"><span data-stu-id="4a500-138">For example, enter *myBizTalkService*BU*Date*.</span></span>
4. <span data-ttu-id="4a500-139">Zvolte účet úložiště blob a vyberte hello zaškrtnutí toostart hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="4a500-139">Choose a blob storage account and select hello checkmark toostart hello backup.</span></span>

<span data-ttu-id="4a500-140">Po dokončení zálohování hello, vytvoří se v účtu úložiště hello kontejner s názvem Zálohování hello, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="4a500-140">Once hello backup completes, a container with hello backup name you enter is created in hello storage account.</span></span> <span data-ttu-id="4a500-141">Tento kontejner obsahuje konfiguraci zálohování služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-141">This container contains your BizTalk Service backup configuration.</span></span>

#### <span data-ttu-id="4a500-142"><a name="backupschedule"></a>Plán zálohování</span><span class="sxs-lookup"><span data-stu-id="4a500-142"><a name="backupschedule"></a>Schedule a backup</span></span>
1. <span data-ttu-id="4a500-143">V hello portál Azure classic, vyberte **BizTalk Services**, vyberte hello název služby BizTalk mají tooschedule hello zálohování a potom vyberte hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="4a500-143">In hello Azure classic portal, select **BizTalk Services**, select hello BizTalk Service name you want tooschedule hello backup, and then select hello **Configure** tab.</span></span>
2. <span data-ttu-id="4a500-144">Sada hello **zálohování stav** příliš**automatické**.</span><span class="sxs-lookup"><span data-stu-id="4a500-144">Set hello **Backup Status** too**Automatic**.</span></span> 
3. <span data-ttu-id="4a500-145">Vyberte hello **účet úložiště** toostore hello zálohování, zadejte hello **frekvence** toocreate hello a jak dlouho tookeep hello zálohování (**dní uchovávání**):</span><span class="sxs-lookup"><span data-stu-id="4a500-145">Select hello **Storage Account** toostore hello backup, enter hello **Frequency** toocreate hello backups, and how long tookeep hello backups (**Retention Days**):</span></span>
   
    ![][AutomaticBU]
   
    <span data-ttu-id="4a500-146">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="4a500-146">**Notes**</span></span>     
   
   * <span data-ttu-id="4a500-147">V **dní uchovávání**, doba uchování hello musí být větší než četnost záloh hello.</span><span class="sxs-lookup"><span data-stu-id="4a500-147">In **Retention Days**, hello retention period must be greater than hello backup frequency.</span></span>
   * <span data-ttu-id="4a500-148">Vyberte **vždy vždycky mějte aspoň jednu zálohu**i v případě, že je po dobu uchování hello.</span><span class="sxs-lookup"><span data-stu-id="4a500-148">Select **Always keep at least one backup**, even if it is past hello retention period.</span></span>
4. <span data-ttu-id="4a500-149">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4a500-149">Select **Save**.</span></span>

<span data-ttu-id="4a500-150">Když spustí naplánované úlohy zálohování, vytvoří kontejner (toostore zálohovaná data) v hello účet úložiště, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="4a500-150">When a scheduled backup job runs, it creates a container (toostore backup data) in hello storage account you entered.</span></span> <span data-ttu-id="4a500-151">Hello název kontejneru hello jmenuje *služby BizTalk název data a času*.</span><span class="sxs-lookup"><span data-stu-id="4a500-151">hello name of hello container is named *BizTalk Service Name-date-time*.</span></span> 

<span data-ttu-id="4a500-152">Pokud se bude zobrazovat hello řídicího panelu služby BizTalk **se nezdařilo** stavu:</span><span class="sxs-lookup"><span data-stu-id="4a500-152">If hello BizTalk Service dashboard shows a **Failed** status:</span></span>

![Stav poslední naplánované zálohy][BackupStatus] 

<span data-ttu-id="4a500-154">Hello odkaz otevře hello toohelp řešení potíží s protokolů pro operace správy služeb.</span><span class="sxs-lookup"><span data-stu-id="4a500-154">hello link opens hello Management Services Operation Logs toohelp troubleshoot.</span></span> <span data-ttu-id="4a500-155">V tématu [BizTalk Services: řešení problémů pomocí protokolů operací](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span><span class="sxs-lookup"><span data-stu-id="4a500-155">See [BizTalk Services: Troubleshoot using operation logs](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span></span>

## <a name="restore"></a><span data-ttu-id="4a500-156">Obnovení</span><span class="sxs-lookup"><span data-stu-id="4a500-156">Restore</span></span>
<span data-ttu-id="4a500-157">Můžete obnovit zálohy z hello portál Azure classic nebo z hello [obnovit REST API služby BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span><span class="sxs-lookup"><span data-stu-id="4a500-157">You can restore backups from hello Azure classic portal or from hello [Restore BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span></span> <span data-ttu-id="4a500-158">Tato část uvádí toorestore kroky hello pomocí portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="4a500-158">This section lists hello steps toorestore using hello classic portal.</span></span>

#### <a name="before-restoring-a-backup"></a><span data-ttu-id="4a500-159">Před obnovením zálohy</span><span class="sxs-lookup"><span data-stu-id="4a500-159">Before restoring a backup</span></span>
* <span data-ttu-id="4a500-160">Nové sledování, archivace a monitorování úložiště lze zadat během obnovování služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-160">New tracking, archiving, and monitoring stores can be entered while restoring a BizTalk Service.</span></span>
* <span data-ttu-id="4a500-161">Dobrý den, je stejné EDI Runtime data obnovit.</span><span class="sxs-lookup"><span data-stu-id="4a500-161">hello same EDI Runtime data is restored.</span></span> <span data-ttu-id="4a500-162">zálohování EDI Runtime Hello ukládá hello řízení čísla.</span><span class="sxs-lookup"><span data-stu-id="4a500-162">hello EDI Runtime backup stores hello control numbers.</span></span> <span data-ttu-id="4a500-163">čísla řízení Hello obnovení jsou v pořadí od času hello hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="4a500-163">hello restored control numbers are in sequence from hello time of hello backup.</span></span> <span data-ttu-id="4a500-164">Pokud po poslední záloze hello zpracování zpráv, obnovení tohoto obsahu zálohování může způsobit duplicitní řízení čísla.</span><span class="sxs-lookup"><span data-stu-id="4a500-164">If messages are processed after hello last backup, restoring this backup content can cause duplicate control numbers.</span></span>

#### <a name="restore-a-backup"></a><span data-ttu-id="4a500-165">Obnovení zálohy</span><span class="sxs-lookup"><span data-stu-id="4a500-165">Restore a backup</span></span>
1. <span data-ttu-id="4a500-166">V hello portál Azure classic, vyberte **nový** > **App Services** > **služby BizTalk** > **obnovení** :</span><span class="sxs-lookup"><span data-stu-id="4a500-166">In hello Azure classic portal, select **New** > **App Services** > **BizTalk Service** > **Restore**:</span></span>
   
    ![Obnovení zálohy][Restore]
2. <span data-ttu-id="4a500-168">V **zálohování URL**vyberte ikonu hello složky a rozbalte hello účtu úložiště Azure, ukládá hello zálohu konfigurace služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-168">In **Backup URL**, select hello folder icon and expand hello Azure storage account that stores hello BizTalk Service configuration backup.</span></span> <span data-ttu-id="4a500-169">Rozbalte kontejner hello a v pravém podokně hello, vyberte odpovídající zálohování souboru .txt hello.</span><span class="sxs-lookup"><span data-stu-id="4a500-169">Expand hello container and in hello right pane, select hello corresponding back up .txt file.</span></span> 
   <br/><br/>
   <span data-ttu-id="4a500-170">Vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="4a500-170">Select **Open**.</span></span>
3. <span data-ttu-id="4a500-171">Na hello **obnovení služby BizTalk** stránky, zadejte **název služby BizTalk** a ověřte hello **adresa URL domény**, **edice**a **Oblast** pro hello obnovit služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-171">On hello **Restore BizTalk Service** page, enter a **BizTalk Service Name** and verify hello **Domain URL**, **Edition**, and **Region** for hello restored BizTalk Service.</span></span> <span data-ttu-id="4a500-172">**Vytvoření nové instance databáze SQL** pro hello sledování databáze:</span><span class="sxs-lookup"><span data-stu-id="4a500-172">**Create a new SQL database instance** for hello tracking database:</span></span>
   
    ![][RestoreBizTalkService]
   
    <span data-ttu-id="4a500-173">Vyberte šipku další hello.</span><span class="sxs-lookup"><span data-stu-id="4a500-173">Select hello next arrow.</span></span>
4. <span data-ttu-id="4a500-174">Ověřte název hello hello SQL databáze, zadejte hello fyzický server, kde bude vytvořen hello SQL database a uživatelské jméno a heslo pro tento server.</span><span class="sxs-lookup"><span data-stu-id="4a500-174">Verify hello name of hello SQL database, enter hello physical server where hello SQL database will be created, and a username/password for that server.</span></span>

    <span data-ttu-id="4a500-175">Pokud chcete tooconfigure hello SQL databáze edice, velikosti a další vlastnosti, vyberte **nakonfigurovat rozšířené nastavení databáze**.</span><span class="sxs-lookup"><span data-stu-id="4a500-175">If you want tooconfigure hello SQL database edition, size, and other properties, select  **Configure Advanced Database Settings**.</span></span> 

    <span data-ttu-id="4a500-176">Vyberte šipku další hello.</span><span class="sxs-lookup"><span data-stu-id="4a500-176">Select hello next arrow.</span></span>

1. <span data-ttu-id="4a500-177">Vytvořit nový účet úložiště nebo zadejte existující účet úložiště pro hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-177">Create a new storage account or enter an existing storage account for hello BizTalk Service.</span></span>
2. <span data-ttu-id="4a500-178">Vyberte hello zaškrtnutí toostart hello obnovení.</span><span class="sxs-lookup"><span data-stu-id="4a500-178">Select hello checkmark toostart hello restore.</span></span>

<span data-ttu-id="4a500-179">Po úspěšném dokončení obnovení hello novou službu BizTalk je uvedena v pozastaveném stavu na stránce služby BizTalk Services hello hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="4a500-179">Once hello restoration successfully completes, a new BizTalk Service is listed in a suspended state on hello BizTalk Services page in hello Azure classic portal.</span></span>

### <span data-ttu-id="4a500-180"><a name="postrestore"></a>Po obnovení ze zálohy</span><span class="sxs-lookup"><span data-stu-id="4a500-180"><a name="postrestore"></a>After restoring a backup</span></span>
<span data-ttu-id="4a500-181">Hello služba BizTalk je vždy obnovit **pozastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="4a500-181">hello BizTalk Service is always restored in a **Suspended** state.</span></span> <span data-ttu-id="4a500-182">V tomto stavu můžete provést změny konfigurace před hello nové prostředí je funkční, včetně:</span><span class="sxs-lookup"><span data-stu-id="4a500-182">In this state, you can make any configuration changes before hello new environment is functional, including:</span></span>

* <span data-ttu-id="4a500-183">Pokud jste vytvořili pomocí sady SDK Azure BizTalk Services hello aplikace služby BizTalk, musíte tootooupdate hello řízení přístupu (ACS) přihlašovací údaje v těchto aplikací toowork s prostředím hello obnovit.</span><span class="sxs-lookup"><span data-stu-id="4a500-183">If you created BizTalk Service applications using hello Azure BizTalk Services SDK, you may need tootooupdate hello Access Control (ACS) credentials in those applications toowork with hello restored environment.</span></span>
* <span data-ttu-id="4a500-184">Obnovení služby BizTalk tooreplicate stávajícího prostředí služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="4a500-184">You restore a BizTalk Service tooreplicate an existing BizTalk Service environment.</span></span> <span data-ttu-id="4a500-185">V této situaci pokud jsou nakonfigurované služby BizTalk Services portálu původní hello smlouvy, které použijte složku zdroje FTP, může být nutné tooupdate hello smluv v prostředí toouse hello nově obnovit jiné zdrojové složky FTP.</span><span class="sxs-lookup"><span data-stu-id="4a500-185">In this situation, if there are agreements configured in hello original BizTalk Services portal that use a source FTP folder, you may need tooupdate hello agreements in hello newly restored environment toouse a different source FTP folder.</span></span> <span data-ttu-id="4a500-186">Jinak, mohou existovat dvě různé smlouvy při toopull hello stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="4a500-186">Otherwise, there may be two different agreements trying toopull hello same message.</span></span>
* <span data-ttu-id="4a500-187">Pokud jste obnovili toohave prostředí s více služby BizTalk, zkontrolujte, zda že cílíte hello správné prostředí v sadě Visual Studio aplikace hello, rutiny prostředí PowerShell, rozhraní REST API nebo Trading Partner OM rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="4a500-187">If you restored toohave multiple BizTalk Service environments, make sure you target hello correct environment in hello Visual Studio applications, PowerShell cmdlets, REST APIs, or Trading Partner Management OM APIs.</span></span>
* <span data-ttu-id="4a500-188">Je dobrým zvykem tooconfigure automatizované v prostředí služby BizTalk hello nově obnovit zálohování.</span><span class="sxs-lookup"><span data-stu-id="4a500-188">It's a good practice tooconfigure automated backups on hello newly restored BizTalk Service environment.</span></span>

<span data-ttu-id="4a500-189">toostart hello služby BizTalk v hello portál Azure classic, vyberte hello obnovit služby BizTalk a vyberte **obnovit** hello hlavním panelu.</span><span class="sxs-lookup"><span data-stu-id="4a500-189">toostart hello BizTalk Service in hello Azure classic portal, select hello restored BizTalk Service and select **Resume** in hello task bar.</span></span> 

## <a name="what-gets-backed-up"></a><span data-ttu-id="4a500-190">Co se zálohuje</span><span class="sxs-lookup"><span data-stu-id="4a500-190">What gets backed up</span></span>
<span data-ttu-id="4a500-191">Při vytváření zálohy hello následující položky jsou zálohovány:</span><span class="sxs-lookup"><span data-stu-id="4a500-191">When a backup is created, hello following items are backed up:</span></span>

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH><span data-ttu-id="4a500-192">Zálohovaných položek</span><span class="sxs-lookup"><span data-stu-id="4a500-192">Items backed up</span></span></TH> 
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="4a500-193">
 <strong>Portál Azure BizTalk Services</strong></span><span class="sxs-lookup"><span data-stu-id="4a500-193">
 <strong>Azure BizTalk Services Portal</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="4a500-194">Konfigurace a prostředí Runtime</span><span class="sxs-lookup"><span data-stu-id="4a500-194">Configuration and Runtime</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="4a500-195">Podrobnosti o partnera a profilu</span><span class="sxs-lookup"><span data-stu-id="4a500-195">Partner and profile details</span></span></li>
<li><span data-ttu-id="4a500-196">Partner smlouvy</span><span class="sxs-lookup"><span data-stu-id="4a500-196">Partner Agreements</span></span></li>
<li><span data-ttu-id="4a500-197">Vlastní sestavení, nasazení</span><span class="sxs-lookup"><span data-stu-id="4a500-197">Custom assemblies deployed</span></span></li>
<li><span data-ttu-id="4a500-198">Mosty nasazení</span><span class="sxs-lookup"><span data-stu-id="4a500-198">Bridges deployed</span></span></li>
<li><span data-ttu-id="4a500-199">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="4a500-199">Certificates</span></span></li>
<li><span data-ttu-id="4a500-200">Transformace nasazení</span><span class="sxs-lookup"><span data-stu-id="4a500-200">Transforms deployed</span></span></li>
<li><span data-ttu-id="4a500-201">Kanály</span><span class="sxs-lookup"><span data-stu-id="4a500-201">Pipelines</span></span></li>
<li><span data-ttu-id="4a500-202">Šablony vytvořili a uložili v hello portál služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="4a500-202">Templates created and saved in hello BizTalk Services Portal</span></span></li>
<li><span data-ttu-id="4a500-203">X12 mapování ST01 a GS01</span><span class="sxs-lookup"><span data-stu-id="4a500-203">X12 ST01 and GS01 mappings</span></span></li>
<li><span data-ttu-id="4a500-204">Ovládací prvek čísla (EDI)</span><span class="sxs-lookup"><span data-stu-id="4a500-204">Control numbers (EDI)</span></span></li>
<li><span data-ttu-id="4a500-205">Hodnoty AS2 zprávy povinná kontrola úrovně Důvěryhodnosti</span><span class="sxs-lookup"><span data-stu-id="4a500-205">AS2 Message MIC values</span></span></li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2"><span data-ttu-id="4a500-206">
 <strong>Služba Azure BizTalk</strong></span><span class="sxs-lookup"><span data-stu-id="4a500-206">
 <strong>Azure BizTalk Service</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="4a500-207">Certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="4a500-207">SSL Certificate</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="4a500-208">Data certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="4a500-208">SSL Certificate Data</span></span></li>
<li><span data-ttu-id="4a500-209">Heslo certifikátu SSL</span><span class="sxs-lookup"><span data-stu-id="4a500-209">SSL Certificate Password</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td><span data-ttu-id="4a500-210">Nastavení služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="4a500-210">BizTalk Service Settings</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="4a500-211">Počet jednotek škálování</span><span class="sxs-lookup"><span data-stu-id="4a500-211">Scale unit count</span></span></li>
<li><span data-ttu-id="4a500-212">Edice</span><span class="sxs-lookup"><span data-stu-id="4a500-212">Edition</span></span></li>
<li><span data-ttu-id="4a500-213">Verze produktu</span><span class="sxs-lookup"><span data-stu-id="4a500-213">Product Version</span></span></li>
<li><span data-ttu-id="4a500-214">Oblast nebo Datacenter</span><span class="sxs-lookup"><span data-stu-id="4a500-214">Region/Datacenter</span></span></li>
<li><span data-ttu-id="4a500-215">Ovládací prvek Service (ACS) přístup k oboru názvů a klíče</span><span class="sxs-lookup"><span data-stu-id="4a500-215">Access Control Service (ACS) namespace and key</span></span></li>
<li><span data-ttu-id="4a500-216">Sledování připojovací řetězec databáze</span><span class="sxs-lookup"><span data-stu-id="4a500-216">Tracking database connection string</span></span></li>
<li><span data-ttu-id="4a500-217">Archivace připojovací řetězce k účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="4a500-217">Archiving Storage account connection string</span></span></li>
<li><span data-ttu-id="4a500-218">Monitorování připojovací řetězce k účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="4a500-218">Monitoring storage account connection string</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="4a500-219">
 <strong>Další položky</strong></span><span class="sxs-lookup"><span data-stu-id="4a500-219">
 <strong>Additional Items</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="4a500-220">Sledování databáze</span><span class="sxs-lookup"><span data-stu-id="4a500-220">Tracking Database</span></span></td> 
<td><span data-ttu-id="4a500-221">Při vytváření služby BizTalk hello jsou zadány hello sledování databáze podrobnosti, včetně hello Server databází SQL Azure a název databáze sledování hello.</span><span class="sxs-lookup"><span data-stu-id="4a500-221">When hello BizTalk Service is created, hello Tracking Database details are entered, including hello Azure SQL Database Server and hello Tracking Database name.</span></span> <span data-ttu-id="4a500-222">Hello sledování databáze není automaticky zálohovat.</span><span class="sxs-lookup"><span data-stu-id="4a500-222">hello Tracking Database is not automatically backed up.</span></span>
<br/><br/><span data-ttu-id="4a500-223">
<strong>Důležité upozornění</strong></span><span class="sxs-lookup"><span data-stu-id="4a500-223">
<strong>Important</strong></span></span><br/>
<span data-ttu-id="4a500-224">Pokud hello sledování databáze bude odstraněna a hello potřeby databáze obnovena, musí existovat předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="4a500-224">If hello Tracking Database is deleted and hello database needs recovered, a previous backup must exist.</span></span> <span data-ttu-id="4a500-225">Pokud zálohu neexistuje, hello sledování databáze a jeho data nejsou použitelná pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="4a500-225">If a backup does not exist, hello Tracking Database and its data are not recoverable.</span></span> <span data-ttu-id="4a500-226">V této situaci, vytvořte novou databázi pro sledování s hello název stejné databáze.</span><span class="sxs-lookup"><span data-stu-id="4a500-226">In this situation, create a new Tracking Database with hello same database name.</span></span> <span data-ttu-id="4a500-227">Doporučuje se geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="4a500-227">Geo-Replication is recommended.</span></span></td>
</tr> 
</table>

## <a name="next"></a><span data-ttu-id="4a500-228">Další</span><span class="sxs-lookup"><span data-stu-id="4a500-228">Next</span></span>
<span data-ttu-id="4a500-229">toocreate Azure BizTalk Services v hello Azure classic přejděte příliš[BizTalk Services: zřízení Azure pomocí portálu classic](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span><span class="sxs-lookup"><span data-stu-id="4a500-229">toocreate Azure BizTalk Services in hello Azure classic portal, go too[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span></span> <span data-ttu-id="4a500-230">vytváření aplikací, přejděte příliš toostart[služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span><span class="sxs-lookup"><span data-stu-id="4a500-230">toostart creating applications, go too[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="4a500-231">Viz také</span><span class="sxs-lookup"><span data-stu-id="4a500-231">See Also</span></span>
* [<span data-ttu-id="4a500-232">Zálohování služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="4a500-232">Backup BizTalk Service</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [<span data-ttu-id="4a500-233">Služba BizTalk obnovit ze zálohy</span><span class="sxs-lookup"><span data-stu-id="4a500-233">Restore BizTalk Service from Backup</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [<span data-ttu-id="4a500-234">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="4a500-234">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [<span data-ttu-id="4a500-235">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="4a500-235">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [<span data-ttu-id="4a500-236">BizTalk Services: Tabulka stavů zřízení</span><span class="sxs-lookup"><span data-stu-id="4a500-236">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [<span data-ttu-id="4a500-237">BizTalk Services: Karty Řídicí panel, Sledování a Škálování</span><span class="sxs-lookup"><span data-stu-id="4a500-237">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [<span data-ttu-id="4a500-238">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="4a500-238">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [<span data-ttu-id="4a500-239">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="4a500-239">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [<span data-ttu-id="4a500-240">Jak začít používat hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="4a500-240">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

