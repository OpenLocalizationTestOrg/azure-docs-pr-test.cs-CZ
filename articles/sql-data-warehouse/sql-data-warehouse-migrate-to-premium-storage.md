---
title: "Migrovat existující Azure datového skladu do úložiště úrovně premium | Microsoft Docs"
description: "Pokyny k migraci existující datový sklad storage úrovně premium"
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
ms.openlocfilehash: 860e50b532b4b0a21d3be54f087730070b0e56bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-data-warehouse-to-premium-storage"></a><span data-ttu-id="98df3-103">Migrace na storage úrovně premium datového skladu</span><span class="sxs-lookup"><span data-stu-id="98df3-103">Migrate your data warehouse to premium storage</span></span>
<span data-ttu-id="98df3-104">Azure SQL Data Warehouse nedávno zaveden [storage úrovně premium pro vyšší výkon, předvídatelnost][premium storage for greater performance predictability].</span><span class="sxs-lookup"><span data-stu-id="98df3-104">Azure SQL Data Warehouse recently introduced [premium storage for greater performance predictability][premium storage for greater performance predictability].</span></span> <span data-ttu-id="98df3-105">Existující datových skladů aktuálně na standardní úložiště lze nyní přenést do úložiště úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="98df3-105">Existing data warehouses currently on standard storage can now be migrated to premium storage.</span></span> <span data-ttu-id="98df3-106">Můžete využít výhod Automatická migrace, nebo pokud chcete řídit, kdy k migraci (které zahrnují výpadky), můžete provést migraci sami.</span><span class="sxs-lookup"><span data-stu-id="98df3-106">You can take advantage of automatic migration, or if you prefer to control when to migrate (which does involve some downtime), you can do the migration yourself.</span></span>

<span data-ttu-id="98df3-107">Pokud máte více než jeden datový sklad, použijte [automatickou migraci plánu] [ automatic migration schedule] k určení, kdy ho bude migrována také.</span><span class="sxs-lookup"><span data-stu-id="98df3-107">If you have more than one data warehouse, use the [automatic migration schedule][automatic migration schedule] to determine when it will also be migrated.</span></span>

## <a name="determine-storage-type"></a><span data-ttu-id="98df3-108">Stanovení typů úložiště</span><span class="sxs-lookup"><span data-stu-id="98df3-108">Determine storage type</span></span>
<span data-ttu-id="98df3-109">Pokud jste vytvořili datový sklad před následující daty, jsou právě používá standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="98df3-109">If you created a data warehouse before the following dates, you are currently using standard storage.</span></span>

| <span data-ttu-id="98df3-110">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="98df3-110">**Region**</span></span> | <span data-ttu-id="98df3-111">**Vytvořené před tímto datem datového skladu**</span><span class="sxs-lookup"><span data-stu-id="98df3-111">**Data warehouse created before this date**</span></span> |
|:--- |:--- |
| <span data-ttu-id="98df3-112">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="98df3-112">Australia East</span></span> |<span data-ttu-id="98df3-113">Storage úrovně Premium zatím není k dispozici</span><span class="sxs-lookup"><span data-stu-id="98df3-113">Premium storage not yet available</span></span> |
| <span data-ttu-id="98df3-114">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="98df3-114">China East</span></span> |<span data-ttu-id="98df3-115">1. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="98df3-115">November 1, 2016</span></span> |
| <span data-ttu-id="98df3-116">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="98df3-116">China North</span></span> |<span data-ttu-id="98df3-117">1. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="98df3-117">November 1, 2016</span></span> |
| <span data-ttu-id="98df3-118">Německo – střed</span><span class="sxs-lookup"><span data-stu-id="98df3-118">Germany Central</span></span> |<span data-ttu-id="98df3-119">1. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="98df3-119">November 1, 2016</span></span> |
| <span data-ttu-id="98df3-120">Německo – severovýchod</span><span class="sxs-lookup"><span data-stu-id="98df3-120">Germany Northeast</span></span> |<span data-ttu-id="98df3-121">1. listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="98df3-121">November 1, 2016</span></span> |
| <span data-ttu-id="98df3-122">Indie – západ</span><span class="sxs-lookup"><span data-stu-id="98df3-122">India West</span></span> |<span data-ttu-id="98df3-123">Storage úrovně Premium zatím není k dispozici</span><span class="sxs-lookup"><span data-stu-id="98df3-123">Premium storage not yet available</span></span> |
| <span data-ttu-id="98df3-124">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="98df3-124">Japan West</span></span> |<span data-ttu-id="98df3-125">Storage úrovně Premium zatím není k dispozici</span><span class="sxs-lookup"><span data-stu-id="98df3-125">Premium storage not yet available</span></span> |
| <span data-ttu-id="98df3-126">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="98df3-126">North Central US</span></span> |<span data-ttu-id="98df3-127">10 od listopadu 2016</span><span class="sxs-lookup"><span data-stu-id="98df3-127">November 10, 2016</span></span> |

## <a name="automatic-migration-details"></a><span data-ttu-id="98df3-128">Automatická migrace podrobnosti</span><span class="sxs-lookup"><span data-stu-id="98df3-128">Automatic migration details</span></span>
<span data-ttu-id="98df3-129">Ve výchozím nastavení, jsme bude databázi migrujte pro vás od 18:00:00 do 6:00:00 ve vaší oblasti místní čas během [automatickou migraci plánu][automatic migration schedule].</span><span class="sxs-lookup"><span data-stu-id="98df3-129">By default, we will migrate your database for you between 6:00 PM and 6:00 AM in your region's local time during the [automatic migration schedule][automatic migration schedule].</span></span> <span data-ttu-id="98df3-130">Během migrace budou existující datový sklad nepoužitelné.</span><span class="sxs-lookup"><span data-stu-id="98df3-130">Your existing data warehouse will be unusable during the migration.</span></span> <span data-ttu-id="98df3-131">Migrace bude trvat přibližně za jednu hodinu za terabajt úložiště za datového skladu.</span><span class="sxs-lookup"><span data-stu-id="98df3-131">The migration will take approximately one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="98df3-132">Vám nebude nic účtováno během jakékoli její části automatickou migraci.</span><span class="sxs-lookup"><span data-stu-id="98df3-132">You will not be charged during any portion of the automatic migration.</span></span>

> [!NOTE]
> <span data-ttu-id="98df3-133">Po dokončení migrace datového skladu bude zpět online a dá se použít.</span><span class="sxs-lookup"><span data-stu-id="98df3-133">When the migration is complete, your data warehouse will be back online and usable.</span></span>
>
>

<span data-ttu-id="98df3-134">Společnost Microsoft provádí následující kroky k dokončení migrace (ty nevyžadují žádné zapojení z vaší strany).</span><span class="sxs-lookup"><span data-stu-id="98df3-134">Microsoft is taking the following steps to complete the migration (these do not require any involvement on your part).</span></span> <span data-ttu-id="98df3-135">V tomto příkladu Představte si, že vaše existující datový sklad na standardní úložiště je aktuálně s názvem "MyDW."</span><span class="sxs-lookup"><span data-stu-id="98df3-135">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="98df3-136">Microsoft přejmenuje "MyDW" na "MyDW_DO_NOT_USE_ [časové razítko]."</span><span class="sxs-lookup"><span data-stu-id="98df3-136">Microsoft renames “MyDW” to “MyDW_DO_NOT_USE_[Timestamp].”</span></span>
2. <span data-ttu-id="98df3-137">Microsoft pozastaví "MyDW_DO_NOT_USE_ [časové razítko]."</span><span class="sxs-lookup"><span data-stu-id="98df3-137">Microsoft pauses “MyDW_DO_NOT_USE_[Timestamp].”</span></span> <span data-ttu-id="98df3-138">Během této doby je převzat zálohu.</span><span class="sxs-lookup"><span data-stu-id="98df3-138">During this time, a backup is taken.</span></span> <span data-ttu-id="98df3-139">Pokud jsme dojde k potížím během tohoto procesu může se zobrazit více zastaví a obnoví.</span><span class="sxs-lookup"><span data-stu-id="98df3-139">You may see multiple pauses and resumes if we encounter any issues during this process.</span></span>
3. <span data-ttu-id="98df3-140">Microsoft vytvoří nový datový sklad s názvem "MyDW" na storage úrovně premium ze zálohy pořízené v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="98df3-140">Microsoft creates a new data warehouse named “MyDW” on premium storage from the backup taken in step 2.</span></span> <span data-ttu-id="98df3-141">"MyDW" se zobrazí až po dokončení obnovení.</span><span class="sxs-lookup"><span data-stu-id="98df3-141">“MyDW” will not appear until after the restore is complete.</span></span>
4. <span data-ttu-id="98df3-142">Po obnovení je dokončeno, "MyDW" vrátí do stejné jednotky datového skladu a stav (pozastaven nebo aktivní) aby byla před migrací.</span><span class="sxs-lookup"><span data-stu-id="98df3-142">After the restore is complete, “MyDW” returns to the same data warehouse units and state (paused or active) that it was before the migration.</span></span>
5. <span data-ttu-id="98df3-143">Po dokončení migrace Microsoft odstraní "MyDW_DO_NOT_USE_ [časové razítko]".</span><span class="sxs-lookup"><span data-stu-id="98df3-143">After the migration is complete, Microsoft deletes “MyDW_DO_NOT_USE_[Timestamp]”.</span></span>

> [!NOTE]
> <span data-ttu-id="98df3-144">Jako součást migrace se nepřenesou následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="98df3-144">The following settings do not carry over as part of the migration:</span></span>
>
> * <span data-ttu-id="98df3-145">Auditování na úrovni databáze musí být znovu zapnout.</span><span class="sxs-lookup"><span data-stu-id="98df3-145">Auditing at the database level needs to be re-enabled.</span></span>
> * <span data-ttu-id="98df3-146">Pravidla brány firewall na úrovni databáze musí být znovu přidat.</span><span class="sxs-lookup"><span data-stu-id="98df3-146">Firewall rules at the database level need to be re-added.</span></span> <span data-ttu-id="98df3-147">Ovlivněné nejsou pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="98df3-147">Firewall rules at the server level are not affected.</span></span>
>
>

### <a name="automatic-migration-schedule"></a><span data-ttu-id="98df3-148">Automatická migrace plánu</span><span class="sxs-lookup"><span data-stu-id="98df3-148">Automatic migration schedule</span></span>
<span data-ttu-id="98df3-149">Automatické migrace dojít od 18:00:00 do 6:00:00 (místní čas na oblast) během následujícího plánu výpadku.</span><span class="sxs-lookup"><span data-stu-id="98df3-149">Automatic migrations occur between 6:00 PM and 6:00 AM (local time per region) during the following outage schedule.</span></span>

| <span data-ttu-id="98df3-150">**Oblast**</span><span class="sxs-lookup"><span data-stu-id="98df3-150">**Region**</span></span> | <span data-ttu-id="98df3-151">**Odhadované datum**</span><span class="sxs-lookup"><span data-stu-id="98df3-151">**Estimated start date**</span></span> | <span data-ttu-id="98df3-152">**Odhadované datum ukončení**</span><span class="sxs-lookup"><span data-stu-id="98df3-152">**Estimated end date**</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="98df3-153">Austrálie – východ</span><span class="sxs-lookup"><span data-stu-id="98df3-153">Australia East</span></span> |<span data-ttu-id="98df3-154">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="98df3-154">Not determined yet</span></span> |<span data-ttu-id="98df3-155">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="98df3-155">Not determined yet</span></span> |
| <span data-ttu-id="98df3-156">Čína – východ</span><span class="sxs-lookup"><span data-stu-id="98df3-156">China East</span></span> |<span data-ttu-id="98df3-157">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-157">January 9, 2017</span></span> |<span data-ttu-id="98df3-158">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-158">January 13, 2017</span></span> |
| <span data-ttu-id="98df3-159">Čína – sever</span><span class="sxs-lookup"><span data-stu-id="98df3-159">China North</span></span> |<span data-ttu-id="98df3-160">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-160">January 9, 2017</span></span> |<span data-ttu-id="98df3-161">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-161">January 13, 2017</span></span> |
| <span data-ttu-id="98df3-162">Německo – střed</span><span class="sxs-lookup"><span data-stu-id="98df3-162">Germany Central</span></span> |<span data-ttu-id="98df3-163">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-163">January 9, 2017</span></span> |<span data-ttu-id="98df3-164">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-164">January 13, 2017</span></span> |
| <span data-ttu-id="98df3-165">Německo – severovýchod</span><span class="sxs-lookup"><span data-stu-id="98df3-165">Germany Northeast</span></span> |<span data-ttu-id="98df3-166">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-166">January 9, 2017</span></span> |<span data-ttu-id="98df3-167">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-167">January 13, 2017</span></span> |
| <span data-ttu-id="98df3-168">Indie – západ</span><span class="sxs-lookup"><span data-stu-id="98df3-168">India West</span></span> |<span data-ttu-id="98df3-169">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="98df3-169">Not determined yet</span></span> |<span data-ttu-id="98df3-170">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="98df3-170">Not determined yet</span></span> |
| <span data-ttu-id="98df3-171">Japonsko – západ</span><span class="sxs-lookup"><span data-stu-id="98df3-171">Japan West</span></span> |<span data-ttu-id="98df3-172">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="98df3-172">Not determined yet</span></span> |<span data-ttu-id="98df3-173">Není-li určit ještě</span><span class="sxs-lookup"><span data-stu-id="98df3-173">Not determined yet</span></span> |
| <span data-ttu-id="98df3-174">Střed USA – sever</span><span class="sxs-lookup"><span data-stu-id="98df3-174">North Central US</span></span> |<span data-ttu-id="98df3-175">9. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-175">January 9, 2017</span></span> |<span data-ttu-id="98df3-176">13. ledna 2017</span><span class="sxs-lookup"><span data-stu-id="98df3-176">January 13, 2017</span></span> |

## <a name="self-migration-to-premium-storage"></a><span data-ttu-id="98df3-177">Vlastní migrace na storage úrovně premium</span><span class="sxs-lookup"><span data-stu-id="98df3-177">Self-migration to premium storage</span></span>
<span data-ttu-id="98df3-178">Pokud chcete řídit, když dojde k odstávka, můžete použít následující kroky k migraci existující datový sklad na standardní úložiště do úložiště úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="98df3-178">If you want to control when your downtime will occur, you can use the following steps to migrate an existing data warehouse on standard storage to premium storage.</span></span> <span data-ttu-id="98df3-179">Pokud zvolíte tuto možnost, musíte vlastní migrace dokončit před zahájením Automatická migrace v této oblasti.</span><span class="sxs-lookup"><span data-stu-id="98df3-179">If you choose this option, you must complete the self-migration before the automatic migration begins in that region.</span></span> <span data-ttu-id="98df3-180">To zajistí, že zabránění nebezpečí automatickou migraci způsobuje konflikt (odkazovat [automatickou migraci plánu][automatic migration schedule]).</span><span class="sxs-lookup"><span data-stu-id="98df3-180">This ensures that you avoid any risk of the automatic migration causing a conflict (refer to the [automatic migration schedule][automatic migration schedule]).</span></span>

### <a name="self-migration-instructions"></a><span data-ttu-id="98df3-181">Pokyny k migraci vlastní</span><span class="sxs-lookup"><span data-stu-id="98df3-181">Self-migration instructions</span></span>
<span data-ttu-id="98df3-182">Pro migraci datového skladu, použijte zálohování a obnovení funkce.</span><span class="sxs-lookup"><span data-stu-id="98df3-182">To migrate your data warehouse yourself, use the backup and restore features.</span></span> <span data-ttu-id="98df3-183">Očekává se, že část obnovení migrace trvat přibližně jednu hodinu za terabajt úložiště za datového skladu.</span><span class="sxs-lookup"><span data-stu-id="98df3-183">The restore portion of the migration is expected to take around one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="98df3-184">Pokud chcete použít stejný název po dokončení migrace, postupujte [kroky pro přejmenování během migrace][steps to rename during migration].</span><span class="sxs-lookup"><span data-stu-id="98df3-184">If you want to keep the same name after migration is complete, follow the [steps to rename during migration][steps to rename during migration].</span></span>

1. <span data-ttu-id="98df3-185">[Pozastavení] [ Pause] datového skladu.</span><span class="sxs-lookup"><span data-stu-id="98df3-185">[Pause][Pause] your data warehouse.</span></span> <span data-ttu-id="98df3-186">Tato akce trvá automatické zálohování.</span><span class="sxs-lookup"><span data-stu-id="98df3-186">This takes an automatic backup.</span></span>
2. <span data-ttu-id="98df3-187">[Obnovit] [ Restore] z vaší poslední snímek.</span><span class="sxs-lookup"><span data-stu-id="98df3-187">[Restore][Restore] from your most recent snapshot.</span></span>
3. <span data-ttu-id="98df3-188">Odstraňte existující datový sklad na standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="98df3-188">Delete your existing data warehouse on standard storage.</span></span> <span data-ttu-id="98df3-189">**Pokud tento krok, vám bude účtována pro obě datových skladů.**</span><span class="sxs-lookup"><span data-stu-id="98df3-189">**If you fail to do this step, you will be charged for both data warehouses.**</span></span>

> [!NOTE]
> <span data-ttu-id="98df3-190">Jako součást migrace se nepřenesou následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="98df3-190">The following settings do not carry over as part of the migration:</span></span>
>
> * <span data-ttu-id="98df3-191">Auditování na úrovni databáze musí být znovu zapnout.</span><span class="sxs-lookup"><span data-stu-id="98df3-191">Auditing at the database level needs to be re-enabled.</span></span>
> * <span data-ttu-id="98df3-192">Pravidla brány firewall na úrovni databáze musí být znovu přidat.</span><span class="sxs-lookup"><span data-stu-id="98df3-192">Firewall rules at the database level need to be re-added.</span></span> <span data-ttu-id="98df3-193">Ovlivněné nejsou pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="98df3-193">Firewall rules at the server level are not affected.</span></span>
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a><span data-ttu-id="98df3-194">Přejmenování datového skladu během migrace (volitelné)</span><span class="sxs-lookup"><span data-stu-id="98df3-194">Rename data warehouse during migration (optional)</span></span>
<span data-ttu-id="98df3-195">Dvě databáze na stejného logického serveru nemůžou mít stejný název.</span><span class="sxs-lookup"><span data-stu-id="98df3-195">Two databases on the same logical server cannot have the same name.</span></span> <span data-ttu-id="98df3-196">SQL Data Warehouse nyní podporuje funkce přejmenování datového skladu.</span><span class="sxs-lookup"><span data-stu-id="98df3-196">SQL Data Warehouse now supports the ability to rename a data warehouse.</span></span>

<span data-ttu-id="98df3-197">V tomto příkladu Představte si, že vaše existující datový sklad na standardní úložiště je aktuálně s názvem "MyDW."</span><span class="sxs-lookup"><span data-stu-id="98df3-197">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="98df3-198">Přejmenujte "MyDW" pomocí následujícího příkazu ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="98df3-198">Rename "MyDW" by using the following ALTER DATABASE command.</span></span> <span data-ttu-id="98df3-199">(V tomto příkladu jsme budete přejmenujte ji "MyDW_BeforeMigration.")  Tento příkaz zastaví všechny stávající transakce a je třeba provést v hlavní databázi proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="98df3-199">(In this example, we'll rename it "MyDW_BeforeMigration.")  This command stops all existing transactions, and must be done in the master database to succeed.</span></span>
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. <span data-ttu-id="98df3-200">[Pozastavení] [ Pause] "MyDW_BeforeMigration."</span><span class="sxs-lookup"><span data-stu-id="98df3-200">[Pause][Pause] "MyDW_BeforeMigration."</span></span> <span data-ttu-id="98df3-201">Tato akce trvá automatické zálohování.</span><span class="sxs-lookup"><span data-stu-id="98df3-201">This takes an automatic backup.</span></span>
3. <span data-ttu-id="98df3-202">[Obnovit] [ Restore] z vaší poslední snímek novou databázi s názvem dříve (například "MyDW").</span><span class="sxs-lookup"><span data-stu-id="98df3-202">[Restore][Restore] from your most recent snapshot a new database with the name it used to be (for example, "MyDW").</span></span>
4. <span data-ttu-id="98df3-203">Odstranit "MyDW_BeforeMigration."</span><span class="sxs-lookup"><span data-stu-id="98df3-203">Delete "MyDW_BeforeMigration."</span></span> <span data-ttu-id="98df3-204">**Pokud tento krok, vám bude účtována pro obě datových skladů.**</span><span class="sxs-lookup"><span data-stu-id="98df3-204">**If you fail to do this step, you will be charged for both data warehouses.**</span></span>


## <a name="next-steps"></a><span data-ttu-id="98df3-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98df3-205">Next steps</span></span>
<span data-ttu-id="98df3-206">Změny do úložiště úrovně premium máte také zvýšením počtu objektů blob soubory databáze v základní Architektura datového skladu.</span><span class="sxs-lookup"><span data-stu-id="98df3-206">With the change to premium storage, you also have an increased number of database blob files in the underlying architecture of your data warehouse.</span></span> <span data-ttu-id="98df3-207">Pokud chcete maximalizovat výkon výhody této změny, znovu sestavte vaše Clusterované indexy columnstore pomocí následujícího skriptu.</span><span class="sxs-lookup"><span data-stu-id="98df3-207">To maximize the performance benefits of this change, rebuild your clustered columnstore indexes by using the following script.</span></span> <span data-ttu-id="98df3-208">Skript funguje tak, že některé z vašich existujících dat vynucení na další objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="98df3-208">The script works by forcing some of your existing data to the additional blobs.</span></span> <span data-ttu-id="98df3-209">Pokud nepodniknete žádnou akci, data se přirozeně znovu distribuovat časem jako načtení více dat do tabulek.</span><span class="sxs-lookup"><span data-stu-id="98df3-209">If you take no action, the data will naturally redistribute over time as you load more data into your tables.</span></span>

<span data-ttu-id="98df3-210">**Požadavky:**</span><span class="sxs-lookup"><span data-stu-id="98df3-210">**Prerequisites:**</span></span>

- <span data-ttu-id="98df3-211">Datový sklad měly být spuštěny s jednotky 1000 datového skladu nebo vyšší (viz [škálování výpočetního výkonu][scale compute power]).</span><span class="sxs-lookup"><span data-stu-id="98df3-211">The data warehouse should run with 1,000 data warehouse units or higher (see [scale compute power][scale compute power]).</span></span>
- <span data-ttu-id="98df3-212">Uživatel spouštění skriptu by měl být v [mediumrc role] [ mediumrc role] nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="98df3-212">The user executing the script should be in the [mediumrc role][mediumrc role] or higher.</span></span> <span data-ttu-id="98df3-213">Chcete-li přidat uživatele do této role, spusťte následující:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span><span class="sxs-lookup"><span data-stu-id="98df3-213">To add a user to this role, execute the following: ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span></span>

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table to control index rebuild
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
-- Step 2: Execute index rebuilds. If script fails, the below can be re-run to restart where last left off.
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

<span data-ttu-id="98df3-214">Pokud narazíte na potíže s datovým skladem, [vytvořit lístek podpory] [ create a support ticket] a referenční dokumentace "migrace na storage úrovně premium" jako možnou příčinu.</span><span class="sxs-lookup"><span data-stu-id="98df3-214">If you encounter any issues with your data warehouse, [create a support ticket][create a support ticket] and reference “migration to premium storage” as the possible cause.</span></span>

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps to rename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
