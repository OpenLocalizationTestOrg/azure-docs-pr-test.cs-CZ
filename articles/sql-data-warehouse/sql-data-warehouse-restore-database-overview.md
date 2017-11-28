---
title: "aaaRestore Azure datového skladu, a-místní a geograficky redundantní | Microsoft Docs"
description: "Přehled možností obnovení hello databáze pro obnovení databáze v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: 3e01c65c-6708-4fd7-82f5-4e1b5f61d304
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: a96b898372b29d420e1416ca93a172ff8af47fc7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-restore"></a><span data-ttu-id="b69b1-103">Obnovení SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b69b1-103">SQL Data Warehouse restore</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="b69b1-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="b69b1-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="b69b1-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="b69b1-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="b69b1-106">[Prostředí PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="b69b1-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="b69b1-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="b69b1-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="b69b1-108">SQL Data Warehouse nabízí místní i zeměpisné obnovení jako součást po havárii jeho datového skladu obnovení.</span><span class="sxs-lookup"><span data-stu-id="b69b1-108">SQL Data Warehouse offers both local and geographical restores as part of its data warehouse disaster recovery capabilities.</span></span> <span data-ttu-id="b69b1-109">Použijte datového skladu zálohy toorestore vašeho datového skladu tooa obnovení bodu v primární oblasti hello nebo použít geograficky redundantní zálohy toorestore tooa jiné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="b69b1-109">Use data warehouse backups toorestore your data warehouse tooa restore point in hello primary region, or use geo-redundant backups toorestore tooa different geographical region.</span></span> <span data-ttu-id="b69b1-110">Tento článek vysvětluje hello specifika obnovení datového skladu.</span><span class="sxs-lookup"><span data-stu-id="b69b1-110">This article explains hello specifics of restoring a data warehouse.</span></span>

## <a name="what-is-a-data-warehouse-restore"></a><span data-ttu-id="b69b1-111">Co je datový sklad obnovení?</span><span class="sxs-lookup"><span data-stu-id="b69b1-111">What is a data warehouse restore?</span></span>
<span data-ttu-id="b69b1-112">Obnovení datového skladu je nový datový sklad, který je vytvořený z existující zálohy nebo odstraněné datového skladu.</span><span class="sxs-lookup"><span data-stu-id="b69b1-112">A data warehouse restore is a new data warehouse that is created from a backup of an existing or deleted data warehouse.</span></span> <span data-ttu-id="b69b1-113">Hello obnovené datového skladu znovu vytvoří hello zálohovaná datového skladu v určitém čase.</span><span class="sxs-lookup"><span data-stu-id="b69b1-113">hello restored data warehouse re-creates hello backed-up data warehouse at a specific time.</span></span> <span data-ttu-id="b69b1-114">Vzhledem k tomu, že SQL Data Warehouse je distribuovaný systém, obnovení datového skladu je vytvořený z mnoha záložní soubory, které jsou uložené v Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="b69b1-114">Since SQL Data Warehouse is a distributed system, a data warehouse restore is created from many backup files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="b69b1-115">Obnovení databáze je nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože znovu vytváří data po náhodného poškození nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="b69b1-115">Database restore is an essential part of any business continuity and disaster recovery strategy because it re-creates your data after accidental corruption or deletion.</span></span>

<span data-ttu-id="b69b1-116">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="b69b1-116">For more information, see:</span></span>

* [<span data-ttu-id="b69b1-117">Zálohování SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b69b1-117">SQL Data Warehouse backups</span></span>](sql-data-warehouse-backups.md)
* [<span data-ttu-id="b69b1-118">Přehled kontinuity obchodních</span><span class="sxs-lookup"><span data-stu-id="b69b1-118">Business continuity overview</span></span>](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a><span data-ttu-id="b69b1-119">Body obnovení datového skladu</span><span class="sxs-lookup"><span data-stu-id="b69b1-119">Data warehouse restore points</span></span>
<span data-ttu-id="b69b1-120">Jako výhodou použití služby Azure Premium Storage SQL Data Warehouse používá Azure Storage Blob snímky toobackup hello primární datového skladu.</span><span class="sxs-lookup"><span data-stu-id="b69b1-120">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots toobackup hello primary data warehouse.</span></span> <span data-ttu-id="b69b1-121">Každý snímek je bod obnovení, který představuje hello čas spuštění snímku hello.</span><span class="sxs-lookup"><span data-stu-id="b69b1-121">Each snapshot has a restore point that represents hello time hello snapshot started.</span></span> <span data-ttu-id="b69b1-122">toorestore datového skladu, vyberte bod obnovení a vydejte příkaz restore.</span><span class="sxs-lookup"><span data-stu-id="b69b1-122">toorestore a data warehouse, you choose a restore point and issue a restore command.</span></span>  

<span data-ttu-id="b69b1-123">SQL Data Warehouse vždy obnoví hello zálohování tooa nový datový sklad.</span><span class="sxs-lookup"><span data-stu-id="b69b1-123">SQL Data Warehouse always restores hello backup tooa new data warehouse.</span></span> <span data-ttu-id="b69b1-124">Můžete buď ponechat hello obnovené datového skladu a hello stávající nebo odstraňte jednu z nich.</span><span class="sxs-lookup"><span data-stu-id="b69b1-124">You can either keep hello restored data warehouse and hello current one, or delete one of them.</span></span> <span data-ttu-id="b69b1-125">Pokud chcete tooreplace hello aktuální datového skladu s hello obnovit datového skladu, ho mohli přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="b69b1-125">If you want tooreplace hello current data warehouse with hello restored data warehouse, you can rename it.</span></span>

<span data-ttu-id="b69b1-126">Pokud potřebujete toorestore odstraněné nebo pozastavený datového skladu, můžete [vytvořit lístek podpory](sql-data-warehouse-get-started-create-support-ticket.md).</span><span class="sxs-lookup"><span data-stu-id="b69b1-126">If you need toorestore a deleted or paused data warehouse, you can [create a support ticket](sql-data-warehouse-get-started-create-support-ticket.md).</span></span> 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore hello last available restore point.

Yes, for hello next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps hello data warehouse and its snapshots for seven days just in case you need hello data. After seven days, you won't be able toorestore tooany of hello restore points. -->

## <a name="geo-redundant-restore"></a><span data-ttu-id="b69b1-127">Geograficky redundantní obnovení</span><span class="sxs-lookup"><span data-stu-id="b69b1-127">Geo-redundant restore</span></span>
<span data-ttu-id="b69b1-128">Můžete obnovit vaše datové oblasti tooany skladu podpora Azure SQL Data Warehouse stejné úrovni zvolené výkonu.</span><span class="sxs-lookup"><span data-stu-id="b69b1-128">You can restore your data warehouse tooany region supporting Azure SQL Data Warehouse at your chosen performance level.</span></span> <span data-ttu-id="b69b1-129">Upozorňujeme, že 9000 a 18000 DWU nejsou podporovány ve všech oblastech během hello preview.</span><span class="sxs-lookup"><span data-stu-id="b69b1-129">Please note that 9000 and 18000 DWU are not supported in all regions during hello preview.</span></span>

> [!NOTE]
> <span data-ttu-id="b69b1-130">obnovení geograficky redundantní tooperform nesmí Nesouhlasili jste tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b69b1-130">tooperform a geo-redundant restore you must not have opted out of this feature.</span></span>
> 
> 

## <a name="restore-timeline"></a><span data-ttu-id="b69b1-131">Obnovení časové osy</span><span class="sxs-lookup"><span data-stu-id="b69b1-131">Restore timeline</span></span>
<span data-ttu-id="b69b1-132">Můžete obnovit bod databáze tooany obnovení k dispozici v rámci hello posledních sedmi dnů.</span><span class="sxs-lookup"><span data-stu-id="b69b1-132">You can restore a database tooany available restore point within hello last seven days.</span></span> <span data-ttu-id="b69b1-133">Snímky spuštění každé čtyři hodiny tooeight a jsou k dispozici sedm dní.</span><span class="sxs-lookup"><span data-stu-id="b69b1-133">Snapshots start every four tooeight hours and are available for seven days.</span></span> <span data-ttu-id="b69b1-134">Pokud snímek je starší než 7 dní, vypršení jeho platnosti a jeho bod obnovení již není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b69b1-134">When a snapshot is older than seven days, it expires and its restore point is no longer available.</span></span>

## <a name="restore-costs"></a><span data-ttu-id="b69b1-135">Obnovení náklady</span><span class="sxs-lookup"><span data-stu-id="b69b1-135">Restore costs</span></span>
<span data-ttu-id="b69b1-136">Hello úložiště zdarma pro obnovená data warehouse hello se fakturuje Azure Premium Storage rychlostí hello.</span><span class="sxs-lookup"><span data-stu-id="b69b1-136">hello storage charge for hello restored data warehouse is billed at hello Azure Premium Storage rate.</span></span> 

<span data-ttu-id="b69b1-137">Pokud pozastavíte obnovené datového skladu, budou se vám účtovat pro úložiště Azure Premium Storage rychlostí hello.</span><span class="sxs-lookup"><span data-stu-id="b69b1-137">If you pause a restored data warehouse, you are charged for storage at hello Azure Premium Storage rate.</span></span> <span data-ttu-id="b69b1-138">Výhodou Hello pozastavení je, že vám není účtován hello DWU výpočetních prostředků.</span><span class="sxs-lookup"><span data-stu-id="b69b1-138">hello advantage of pausing is you are not charged for hello DWU computing resources.</span></span>

<span data-ttu-id="b69b1-139">Další informace o cenách služby SQL Data Warehouse najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="b69b1-139">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="uses-for-restore"></a><span data-ttu-id="b69b1-140">Používá pro obnovení</span><span class="sxs-lookup"><span data-stu-id="b69b1-140">Uses for restore</span></span>
<span data-ttu-id="b69b1-141">Hello primárního použití pro obnovení datového skladu je toorecover data po před náhodnou ztrátou dat nebo poškození.</span><span class="sxs-lookup"><span data-stu-id="b69b1-141">hello primary use for data warehouse restore is toorecover data after accidental data loss or corruption.</span></span>

<span data-ttu-id="b69b1-142">Můžete taky datového skladu obnovení tooretain zálohu po dobu delší než 7 dní.</span><span class="sxs-lookup"><span data-stu-id="b69b1-142">You can also use data warehouse restore tooretain a backup for longer than seven days.</span></span> <span data-ttu-id="b69b1-143">Po obnovení hello zálohování online máte hello datového skladu a můžete pozastavit ho neomezeně dlouhou dobu toosave výpočetní náklady.</span><span class="sxs-lookup"><span data-stu-id="b69b1-143">Once hello backup is restored, you have hello data warehouse online and can pause it indefinitely toosave compute costs.</span></span> <span data-ttu-id="b69b1-144">Pozastavený databáze Hello způsobuje poplatky za úložiště Storage úrovně Premium rychlostí hello.</span><span class="sxs-lookup"><span data-stu-id="b69b1-144">hello paused database incurs storage charges at hello Azure Premium Storage rate.</span></span> 

## <a name="related-topics"></a><span data-ttu-id="b69b1-145">Související témata</span><span class="sxs-lookup"><span data-stu-id="b69b1-145">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="b69b1-146">Scénáře</span><span class="sxs-lookup"><span data-stu-id="b69b1-146">Scenarios</span></span>
* <span data-ttu-id="b69b1-147">Přehled kontinuity obchodních, najdete v části [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="b69b1-147">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

<span data-ttu-id="b69b1-148">tooperform datového skladu obnovit, obnovte pomocí:</span><span class="sxs-lookup"><span data-stu-id="b69b1-148">tooperform a data warehouse restore, restore using:</span></span>

* <span data-ttu-id="b69b1-149">Azure portálu, najdete v tématu [obnovit data warehouse pomocí hello portálu Azure](sql-data-warehouse-restore-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="b69b1-149">Azure portal, see [Restore a data warehouse using hello Azure portal](sql-data-warehouse-restore-database-portal.md)</span></span>
* <span data-ttu-id="b69b1-150">Rutiny prostředí PowerShell najdete v části [obnovení datového skladu pomocí rutin prostředí PowerShell](sql-data-warehouse-restore-database-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="b69b1-150">PowerShell cmdlets, see [Restore a data warehouse using PowerShell cmdlets](sql-data-warehouse-restore-database-powershell.md)</span></span>
* <span data-ttu-id="b69b1-151">Rozhraní REST API najdete v tématu [obnovit data warehouse pomocí hello rozhraní REST API](sql-data-warehouse-restore-database-rest-api.md)</span><span class="sxs-lookup"><span data-stu-id="b69b1-151">REST APIs, see [Restore a data warehouse using hello REST APIs](sql-data-warehouse-restore-database-rest-api.md)</span></span>

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
