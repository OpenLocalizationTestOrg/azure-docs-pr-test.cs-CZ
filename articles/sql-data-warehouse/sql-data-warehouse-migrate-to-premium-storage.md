---
title: "aaaMigrate vaše stávající Azure datového skladu toopremium úložiště | Microsoft Docs"
description: "Pokyny k migraci existujících dat skladu toopremium úložiště"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: barbkess
editor: 
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 11/29/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 145199c2da1f6f1fb8898626cd04886c42d82204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data-warehouse-toopremium-storage"></a><span data-ttu-id="d6015-103">Migrace úložiště toopremium datového skladu</span><span class="sxs-lookup"><span data-stu-id="d6015-103">Migrate your data warehouse toopremium storage</span></span>
<span data-ttu-id="d6015-104">Azure SQL Data Warehouse nedávno zaveden [storage úrovně premium pro vyšší výkon, předvídatelnost][premium storage for greater performance predictability].</span><span class="sxs-lookup"><span data-stu-id="d6015-104">Azure SQL Data Warehouse recently introduced [premium storage for greater performance predictability][premium storage for greater performance predictability].</span></span> <span data-ttu-id="d6015-105">Stávající data, která teď může být sklady aktuálně na standardní úložiště migrovat toopremium úložiště.</span><span class="sxs-lookup"><span data-stu-id="d6015-105">Existing data warehouses currently on standard storage can now be migrated toopremium storage.</span></span> <span data-ttu-id="d6015-106">Můžete využít výhod Automatická migrace, nebo pokud dáváte přednost toocontrol při toomigrate (které zahrnovat výpadky), můžete provést migraci hello sami.</span><span class="sxs-lookup"><span data-stu-id="d6015-106">You can take advantage of automatic migration, or if you prefer toocontrol when toomigrate (which does involve some downtime), you can do hello migration yourself.</span></span>

<span data-ttu-id="d6015-107">Pokud máte více než jeden datový sklad, použijte hello [automatickou migraci plánu] [ automatic migration schedule] toodetermine, když bude taky migrovat.</span><span class="sxs-lookup"><span data-stu-id="d6015-107">If you have more than one data warehouse, use hello [automatic migration schedule][automatic migration schedule] toodetermine when it will also be migrated.</span></span>

## <a name="determine-storage-type"></a><span data-ttu-id="d6015-108">Stanovení typů úložiště</span><span class="sxs-lookup"><span data-stu-id="d6015-108">Determine storage type</span></span>
<span data-ttu-id="d6015-109">Pokud jste vytvořili datového skladu před hello následující data, se aktuálně používá standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="d6015-109">If you created a data warehouse before hello following dates, you are currently using standard storage.</span></span>

| <span data-ttu-id="d6015-110">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="d6015-110">**Region**</span></span> | <span data-ttu-id="d6015-111">**Vytvořené před tímto datem datového skladu**</span><span class="sxs-lookup"><span data-stu-id="d6015-111">**Data warehouse created before this date**</span></span> |
|:--- |:--- |
| <span data-ttu-id="d6015-112">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="d6015-112">Australia East</span></span> |<span data-ttu-id="d6015-113">Storage úrovně Premium zatím není k dispozici</span><span class="sxs-lookup"><span data-stu-id="d6015-113">Premium storage not yet available</span></span> |
| <span data-ttu-id="d6015-114">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="d6015-114">China East</span></span> |<span data-ttu-id="d6015-115">1. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="d6015-115">November 1, 2016</span></span> |
| <span data-ttu-id="d6015-116">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="d6015-116">China North</span></span> |<span data-ttu-id="d6015-117">1. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="d6015-117">November 1, 2016</span></span> |
| <span data-ttu-id="d6015-118">Německo – střed</span><span class="sxs-lookup"><span data-stu-id="d6015-118">Germany Central</span></span> |<span data-ttu-id="d6015-119">1. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="d6015-119">November 1, 2016</span></span> |
| <span data-ttu-id="d6015-120">Německo – severovýchod</span><span class="sxs-lookup"><span data-stu-id="d6015-120">Germany Northeast</span></span> |<span data-ttu-id="d6015-121">1. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="d6015-121">November 1, 2016</span></span> |
| <span data-ttu-id="d6015-122">Indie – západ</span><span class="sxs-lookup"><span data-stu-id="d6015-122">India West</span></span> |<span data-ttu-id="d6015-123">Storage úrovně Premium zatím není k dispozici</span><span class="sxs-lookup"><span data-stu-id="d6015-123">Premium storage not yet available</span></span> |
| <span data-ttu-id="d6015-124">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="d6015-124">Japan West</span></span> |<span data-ttu-id="d6015-125">Storage úrovně Premium zatím není k dispozici</span><span class="sxs-lookup"><span data-stu-id="d6015-125">Premium storage not yet available</span></span> |
| <span data-ttu-id="d6015-126">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="d6015-126">North Central US</span></span> |<span data-ttu-id="d6015-127">10 od listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="d6015-127">November 10, 2016</span></span> |

## <a name="automatic-migration-details"></a><span data-ttu-id="d6015-128">Automatická migrace podrobnosti</span><span class="sxs-lookup"><span data-stu-id="d6015-128">Automatic migration details</span></span>
<span data-ttu-id="d6015-129">Ve výchozím nastavení, jsme bude databázi migrujte pro vás od 18:00:00 do 6:00:00 ve vaší oblasti místní čas během hello [automatickou migraci plánu][automatic migration schedule].</span><span class="sxs-lookup"><span data-stu-id="d6015-129">By default, we will migrate your database for you between 6:00 PM and 6:00 AM in your region's local time during hello [automatic migration schedule][automatic migration schedule].</span></span> <span data-ttu-id="d6015-130">Během migrace hello nepoužitelný existující datový sklad.</span><span class="sxs-lookup"><span data-stu-id="d6015-130">Your existing data warehouse will be unusable during hello migration.</span></span> <span data-ttu-id="d6015-131">Hello migrace bude trvat přibližně za jednu hodinu za terabajt úložiště za datového skladu.</span><span class="sxs-lookup"><span data-stu-id="d6015-131">hello migration will take approximately one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="d6015-132">Vám nebude nic účtováno během jakékoli její části hello automatickou migraci.</span><span class="sxs-lookup"><span data-stu-id="d6015-132">You will not be charged during any portion of hello automatic migration.</span></span>

> [!NOTE]
> <span data-ttu-id="d6015-133">Po dokončení migrace hello datového skladu bude zpět online a dá se použít.</span><span class="sxs-lookup"><span data-stu-id="d6015-133">When hello migration is complete, your data warehouse will be back online and usable.</span></span>
>
>

<span data-ttu-id="d6015-134">Microsoft trvá hello následující kroky toocomplete hello migrace (ty nevyžadují žádné zapojení z vaší strany).</span><span class="sxs-lookup"><span data-stu-id="d6015-134">Microsoft is taking hello following steps toocomplete hello migration (these do not require any involvement on your part).</span></span> <span data-ttu-id="d6015-135">V tomto příkladu Představte si, že vaše existující datový sklad na standardní úložiště je aktuálně s názvem "MyDW."</span><span class="sxs-lookup"><span data-stu-id="d6015-135">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="d6015-136">Microsoft přejmenuje "MyDW" příliš "MyDW_DO_NOT_USE_ [časové razítko]."</span><span class="sxs-lookup"><span data-stu-id="d6015-136">Microsoft renames “MyDW” too“MyDW_DO_NOT_USE_[Timestamp].”</span></span>
2. <span data-ttu-id="d6015-137">Microsoft pozastaví "MyDW_DO_NOT_USE_ [časové razítko]."</span><span class="sxs-lookup"><span data-stu-id="d6015-137">Microsoft pauses “MyDW_DO_NOT_USE_[Timestamp].”</span></span> <span data-ttu-id="d6015-138">Během této doby je převzat zálohu.</span><span class="sxs-lookup"><span data-stu-id="d6015-138">During this time, a backup is taken.</span></span> <span data-ttu-id="d6015-139">Pokud jsme dojde k potížím během tohoto procesu může se zobrazit více zastaví a obnoví.</span><span class="sxs-lookup"><span data-stu-id="d6015-139">You may see multiple pauses and resumes if we encounter any issues during this process.</span></span>
3. <span data-ttu-id="d6015-140">Microsoft vytvoří nový datový sklad s názvem "MyDW" na storage úrovně premium ze zálohy hello prováděné v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="d6015-140">Microsoft creates a new data warehouse named “MyDW” on premium storage from hello backup taken in step 2.</span></span> <span data-ttu-id="d6015-141">"MyDW" se zobrazí až po dokončení obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="d6015-141">“MyDW” will not appear until after hello restore is complete.</span></span>
4. <span data-ttu-id="d6015-142">Po dokončení obnovení hello "MyDW" vrátí toohello stejný datový sklad jednotky a stavu (pozastaven nebo aktivní), který byl před migrací hello.</span><span class="sxs-lookup"><span data-stu-id="d6015-142">After hello restore is complete, “MyDW” returns toohello same data warehouse units and state (paused or active) that it was before hello migration.</span></span>
5. <span data-ttu-id="d6015-143">Po dokončení migrace hello, odstraní Microsoft "MyDW_DO_NOT_USE_ [časové razítko]".</span><span class="sxs-lookup"><span data-stu-id="d6015-143">After hello migration is complete, Microsoft deletes “MyDW_DO_NOT_USE_[Timestamp]”.</span></span>

> [!NOTE]
> <span data-ttu-id="d6015-144">Hello následující nastavení se nepřenesou jako součást migrace hello:</span><span class="sxs-lookup"><span data-stu-id="d6015-144">hello following settings do not carry over as part of hello migration:</span></span>
>
> * <span data-ttu-id="d6015-145">Auditování na úrovni databáze hello musí toobe funkce znovu povolena.</span><span class="sxs-lookup"><span data-stu-id="d6015-145">Auditing at hello database level needs toobe re-enabled.</span></span>
> * <span data-ttu-id="d6015-146">Pravidla brány firewall na úrovni databáze hello potřebovat toobe znovu přidat.</span><span class="sxs-lookup"><span data-stu-id="d6015-146">Firewall rules at hello database level need toobe re-added.</span></span> <span data-ttu-id="d6015-147">Pravidla brány firewall na úrovni serveru hello nemá vliv.</span><span class="sxs-lookup"><span data-stu-id="d6015-147">Firewall rules at hello server level are not affected.</span></span>
>
>

### <a name="automatic-migration-schedule"></a><span data-ttu-id="d6015-148">Automatická migrace plánu</span><span class="sxs-lookup"><span data-stu-id="d6015-148">Automatic migration schedule</span></span>
<span data-ttu-id="d6015-149">Automatické migrace dojde během hello následující výpadku plán od 18:00:00 do 6:00 AM (místní čas na oblast).</span><span class="sxs-lookup"><span data-stu-id="d6015-149">Automatic migrations occur between 6:00 PM and 6:00 AM (local time per region) during hello following outage schedule.</span></span>

| <span data-ttu-id="d6015-150">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="d6015-150">**Region**</span></span> | <span data-ttu-id="d6015-151">**Odhadované datum**</span><span class="sxs-lookup"><span data-stu-id="d6015-151">**Estimated start date**</span></span> | <span data-ttu-id="d6015-152">**Odhadované datum ukončení**</span><span class="sxs-lookup"><span data-stu-id="d6015-152">**Estimated end date**</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="d6015-153">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="d6015-153">Australia East</span></span> |<span data-ttu-id="d6015-154">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="d6015-154">Not determined yet</span></span> |<span data-ttu-id="d6015-155">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="d6015-155">Not determined yet</span></span> |
| <span data-ttu-id="d6015-156">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="d6015-156">China East</span></span> |<span data-ttu-id="d6015-157">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-157">January 9, 2017</span></span> |<span data-ttu-id="d6015-158">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-158">January 13, 2017</span></span> |
| <span data-ttu-id="d6015-159">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="d6015-159">China North</span></span> |<span data-ttu-id="d6015-160">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-160">January 9, 2017</span></span> |<span data-ttu-id="d6015-161">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-161">January 13, 2017</span></span> |
| <span data-ttu-id="d6015-162">Německo – střed</span><span class="sxs-lookup"><span data-stu-id="d6015-162">Germany Central</span></span> |<span data-ttu-id="d6015-163">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-163">January 9, 2017</span></span> |<span data-ttu-id="d6015-164">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-164">January 13, 2017</span></span> |
| <span data-ttu-id="d6015-165">Německo – severovýchod</span><span class="sxs-lookup"><span data-stu-id="d6015-165">Germany Northeast</span></span> |<span data-ttu-id="d6015-166">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-166">January 9, 2017</span></span> |<span data-ttu-id="d6015-167">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-167">January 13, 2017</span></span> |
| <span data-ttu-id="d6015-168">Indie – západ</span><span class="sxs-lookup"><span data-stu-id="d6015-168">India West</span></span> |<span data-ttu-id="d6015-169">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="d6015-169">Not determined yet</span></span> |<span data-ttu-id="d6015-170">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="d6015-170">Not determined yet</span></span> |
| <span data-ttu-id="d6015-171">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="d6015-171">Japan West</span></span> |<span data-ttu-id="d6015-172">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="d6015-172">Not determined yet</span></span> |<span data-ttu-id="d6015-173">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="d6015-173">Not determined yet</span></span> |
| <span data-ttu-id="d6015-174">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="d6015-174">North Central US</span></span> |<span data-ttu-id="d6015-175">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-175">January 9, 2017</span></span> |<span data-ttu-id="d6015-176">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="d6015-176">January 13, 2017</span></span> |

## <a name="self-migration-toopremium-storage"></a><span data-ttu-id="d6015-177">Vlastní migrace toopremium úložiště</span><span class="sxs-lookup"><span data-stu-id="d6015-177">Self-migration toopremium storage</span></span>
<span data-ttu-id="d6015-178">Pokud chcete toocontrol, když dojde k vaší výpadku, můžete použít následující kroky toomigrate existující datový sklad na standardní úložiště toopremium úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="d6015-178">If you want toocontrol when your downtime will occur, you can use hello following steps toomigrate an existing data warehouse on standard storage toopremium storage.</span></span> <span data-ttu-id="d6015-179">Pokud zvolíte tuto možnost, musíte dokončit migraci vlastní hello před zahájením hello Automatická migrace v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="d6015-179">If you choose this option, you must complete hello self-migration before hello automatic migration begins in that region.</span></span> <span data-ttu-id="d6015-180">To zajistí, že zabránění nebezpečí hello automatickou migraci způsobuje konflikt (odkazovat toohello [automatickou migraci plánu][automatic migration schedule]).</span><span class="sxs-lookup"><span data-stu-id="d6015-180">This ensures that you avoid any risk of hello automatic migration causing a conflict (refer toohello [automatic migration schedule][automatic migration schedule]).</span></span>

### <a name="self-migration-instructions"></a><span data-ttu-id="d6015-181">Pokyny k migraci vlastní</span><span class="sxs-lookup"><span data-stu-id="d6015-181">Self-migration instructions</span></span>
<span data-ttu-id="d6015-182">toomigrate data skladu sami, použijte hello zálohování a obnovení funkce.</span><span class="sxs-lookup"><span data-stu-id="d6015-182">toomigrate your data warehouse yourself, use hello backup and restore features.</span></span> <span data-ttu-id="d6015-183">část obnovení Hello hello migrace je očekávané tootake přibližně jedné hodiny za terabajt úložiště za datového skladu.</span><span class="sxs-lookup"><span data-stu-id="d6015-183">hello restore portion of hello migration is expected tootake around one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="d6015-184">Pokud chcete, aby tookeep hello stejný název po dokončení migrace, postupujte podle hello [toorename kroky při migraci][steps toorename during migration].</span><span class="sxs-lookup"><span data-stu-id="d6015-184">If you want tookeep hello same name after migration is complete, follow hello [steps toorename during migration][steps toorename during migration].</span></span>

1. <span data-ttu-id="d6015-185">[Pozastavení] [ Pause] datového skladu.</span><span class="sxs-lookup"><span data-stu-id="d6015-185">[Pause][Pause] your data warehouse.</span></span> <span data-ttu-id="d6015-186">Tato akce trvá automatické zálohování.</span><span class="sxs-lookup"><span data-stu-id="d6015-186">This takes an automatic backup.</span></span>
2. <span data-ttu-id="d6015-187">[Obnovit] [ Restore] z vaší poslední snímek.</span><span class="sxs-lookup"><span data-stu-id="d6015-187">[Restore][Restore] from your most recent snapshot.</span></span>
3. <span data-ttu-id="d6015-188">Odstraňte existující datový sklad na standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="d6015-188">Delete your existing data warehouse on standard storage.</span></span> <span data-ttu-id="d6015-189">**Pokud selže toodo tento krok vám bude účtována pro obě datových skladů.**</span><span class="sxs-lookup"><span data-stu-id="d6015-189">**If you fail toodo this step, you will be charged for both data warehouses.**</span></span>

> [!NOTE]
> <span data-ttu-id="d6015-190">Hello následující nastavení se nepřenesou jako součást migrace hello:</span><span class="sxs-lookup"><span data-stu-id="d6015-190">hello following settings do not carry over as part of hello migration:</span></span>
>
> * <span data-ttu-id="d6015-191">Auditování na úrovni databáze hello musí toobe funkce znovu povolena.</span><span class="sxs-lookup"><span data-stu-id="d6015-191">Auditing at hello database level needs toobe re-enabled.</span></span>
> * <span data-ttu-id="d6015-192">Pravidla brány firewall na úrovni databáze hello potřebovat toobe znovu přidat.</span><span class="sxs-lookup"><span data-stu-id="d6015-192">Firewall rules at hello database level need toobe re-added.</span></span> <span data-ttu-id="d6015-193">Pravidla brány firewall na úrovni serveru hello nemá vliv.</span><span class="sxs-lookup"><span data-stu-id="d6015-193">Firewall rules at hello server level are not affected.</span></span>
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a><span data-ttu-id="d6015-194">Přejmenování datového skladu během migrace (volitelné)</span><span class="sxs-lookup"><span data-stu-id="d6015-194">Rename data warehouse during migration (optional)</span></span>
<span data-ttu-id="d6015-195">Dvě databáze na hello nemůže mít stejný logický server hello stejný název.</span><span class="sxs-lookup"><span data-stu-id="d6015-195">Two databases on hello same logical server cannot have hello same name.</span></span> <span data-ttu-id="d6015-196">SQL Data Warehouse teď podporuje hello možnost toorename datového skladu.</span><span class="sxs-lookup"><span data-stu-id="d6015-196">SQL Data Warehouse now supports hello ability toorename a data warehouse.</span></span>

<span data-ttu-id="d6015-197">V tomto příkladu Představte si, že vaše existující datový sklad na standardní úložiště je aktuálně s názvem "MyDW."</span><span class="sxs-lookup"><span data-stu-id="d6015-197">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="d6015-198">Přejmenujte "MyDW" pomocí hello následující příkaz ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="d6015-198">Rename "MyDW" by using hello following ALTER DATABASE command.</span></span> <span data-ttu-id="d6015-199">(V tomto příkladu jsme budete přejmenujte ji "MyDW_BeforeMigration.")  Tento příkaz zastaví všechny stávající transakce a je třeba provést v hlavní databázi toosucceed hello.</span><span class="sxs-lookup"><span data-stu-id="d6015-199">(In this example, we'll rename it "MyDW_BeforeMigration.")  This command stops all existing transactions, and must be done in hello master database toosucceed.</span></span>
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. <span data-ttu-id="d6015-200">[Pozastavení] [ Pause] "MyDW_BeforeMigration."</span><span class="sxs-lookup"><span data-stu-id="d6015-200">[Pause][Pause] "MyDW_BeforeMigration."</span></span> <span data-ttu-id="d6015-201">Tato akce trvá automatické zálohování.</span><span class="sxs-lookup"><span data-stu-id="d6015-201">This takes an automatic backup.</span></span>
3. <span data-ttu-id="d6015-202">[Obnovit] [ Restore] z nejnovější snímku novou databázi s názvem hello použije toobe (například "MyDW").</span><span class="sxs-lookup"><span data-stu-id="d6015-202">[Restore][Restore] from your most recent snapshot a new database with hello name it used toobe (for example, "MyDW").</span></span>
4. <span data-ttu-id="d6015-203">Odstranit "MyDW_BeforeMigration."</span><span class="sxs-lookup"><span data-stu-id="d6015-203">Delete "MyDW_BeforeMigration."</span></span> <span data-ttu-id="d6015-204">**Pokud selže toodo tento krok vám bude účtována pro obě datových skladů.**</span><span class="sxs-lookup"><span data-stu-id="d6015-204">**If you fail toodo this step, you will be charged for both data warehouses.**</span></span>


## <a name="next-steps"></a><span data-ttu-id="d6015-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6015-205">Next steps</span></span>
<span data-ttu-id="d6015-206">Hello změnit toopremium úložiště, máte také zvýšením počtu objektů blob soubory databáze v hello základní Architektura datového skladu.</span><span class="sxs-lookup"><span data-stu-id="d6015-206">With hello change toopremium storage, you also have an increased number of database blob files in hello underlying architecture of your data warehouse.</span></span> <span data-ttu-id="d6015-207">toomaximize hello výkonu výhody této změny znovu sestavit vaše Clusterované indexy columnstore pomocí hello následující skript.</span><span class="sxs-lookup"><span data-stu-id="d6015-207">toomaximize hello performance benefits of this change, rebuild your clustered columnstore indexes by using hello following script.</span></span> <span data-ttu-id="d6015-208">skript Hello funguje tak, že některé z existujících dat toohello další objektů BLOB vynucení.</span><span class="sxs-lookup"><span data-stu-id="d6015-208">hello script works by forcing some of your existing data toohello additional blobs.</span></span> <span data-ttu-id="d6015-209">Pokud nepodniknete žádnou akci, hello dat bude přirozeně znovu distribuovat časem jako načtení více dat do tabulek.</span><span class="sxs-lookup"><span data-stu-id="d6015-209">If you take no action, hello data will naturally redistribute over time as you load more data into your tables.</span></span>

<span data-ttu-id="d6015-210">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="d6015-210">**Prerequisites:**</span></span>

- <span data-ttu-id="d6015-211">Hello datového skladu měly být spuštěny s jednotky 1000 datového skladu nebo vyšší (viz [škálování výpočetního výkonu][scale compute power]).</span><span class="sxs-lookup"><span data-stu-id="d6015-211">hello data warehouse should run with 1,000 data warehouse units or higher (see [scale compute power][scale compute power]).</span></span>
- <span data-ttu-id="d6015-212">Hello uživatel provádění skriptu hello by měl být v hello [mediumrc role] [ mediumrc role] nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d6015-212">hello user executing hello script should be in hello [mediumrc role][mediumrc role] or higher.</span></span> <span data-ttu-id="d6015-213">tooadd toothis role uživatele, spusťte následující hello:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span><span class="sxs-lookup"><span data-stu-id="d6015-213">tooadd a user toothis role, execute hello following: ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span></span>

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table toocontrol index rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go

--------------------------------------------------------------------------------
-- Step 2: Execute index rebuilds. If script fails, hello below can be re-run toorestart where last left off.
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Clean up table created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

<span data-ttu-id="d6015-214">Pokud narazíte na potíže s datovým skladem, [vytvořit lístek podpory] [ create a support ticket] a odkazovat na "migrace toopremium úložiště" jako možnou příčinou hello.</span><span class="sxs-lookup"><span data-stu-id="d6015-214">If you encounter any issues with your data warehouse, [create a support ticket][create a support ticket] and reference “migration toopremium storage” as hello possible cause.</span></span>

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration tooPremium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps toorename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
