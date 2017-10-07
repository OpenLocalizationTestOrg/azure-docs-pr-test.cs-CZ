---
title: "zálohování SQL Data Warehouse aaaAzure - snímky geograficky redundantní | Microsoft Docs"
description: "Další informace o zálohování integrovanou databází SQL Data Warehouse, které umožňují toorestore bod obnovení tooa Azure SQL Data Warehouse nebo jiné zeměpisné oblasti."
services: sql-data-warehouse
documentationcenter: 
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 34659480485246f54a1490e185fc1b903fb2520d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-backups"></a><span data-ttu-id="c4ca5-103">Zálohování SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c4ca5-103">SQL Data Warehouse backups</span></span>
<span data-ttu-id="c4ca5-104">SQL Data Warehouse nabízí místní i zeměpisné záloh v rámci možnosti zálohování datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-104">SQL Data Warehouse offers both local and geographical backups as part of its data warehouse backup capabilities.</span></span> <span data-ttu-id="c4ca5-105">Mezi ně patří Azure Storage Blob snímky a geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-105">These include Azure Storage Blob snapshots and geo-redundant storage.</span></span> <span data-ttu-id="c4ca5-106">Použijte datového skladu zálohy toorestore vašeho datového skladu tooa obnovení bodu v primární oblasti hello nebo obnovit tooa jiné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-106">Use data warehouse backups toorestore your data warehouse tooa restore point in hello primary region, or restore tooa different geographical region.</span></span> <span data-ttu-id="c4ca5-107">Tento článek vysvětluje hello specifika záloh v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-107">This article explains hello specifics of backups in SQL Data Warehouse.</span></span>

## <a name="what-is-a-data-warehouse-backup"></a><span data-ttu-id="c4ca5-108">Co je zálohu datového skladu?</span><span class="sxs-lookup"><span data-stu-id="c4ca5-108">What is a data warehouse backup?</span></span>
<span data-ttu-id="c4ca5-109">Zálohu datového skladu je hello dat, které můžete použít toorestore určitý čas tooa datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-109">A data warehouse backup is hello data that you can use toorestore a data warehouse tooa specific time.</span></span>  <span data-ttu-id="c4ca5-110">Vzhledem k tomu, že datový sklad SQL je distribuovaného systému, zálohu datového skladu se skládá z mnoho souborů, které jsou uložené v Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-110">Since SQL Data Warehouse is a distributed system, a data warehouse backup consists of many files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="c4ca5-111">Zálohování databáze jsou nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože se data chránit před náhodnou poškození nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-111">Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.</span></span> <span data-ttu-id="c4ca5-112">Další informace najdete v tématu [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-112">For more information, see [Business continuity overview](../sql-database/sql-database-business-continuity.md).</span></span>

## <a name="data-redundancy"></a><span data-ttu-id="c4ca5-113">Redundance dat</span><span class="sxs-lookup"><span data-stu-id="c4ca5-113">Data redundancy</span></span>
<span data-ttu-id="c4ca5-114">SQL Data Warehouse chrání vaše data ukládáním dat v místně redundantní (LRS) Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-114">SQL Data Warehouse protects your data by storing your data in locally redundant (LRS) Azure Premium Storage.</span></span> <span data-ttu-id="c4ca5-115">Tato funkce Azure Storage ukládá několik synchronních kopií dat hello v hello místní data center tooguarantee transparentní ochrana dat, pokud existují lokálních selhání.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-115">This Azure Storage feature stores multiple synchronous copies of hello data in hello local data center tooguarantee transparent data protection if there are localized failures.</span></span> <span data-ttu-id="c4ca5-116">Redundanci dat zajišťuje, že vaše data přežijí problémy infrastruktury, jako je selhání disku.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-116">Data redundancy ensures that your data can survive infrastructure issues such as disk failures.</span></span> <span data-ttu-id="c4ca5-117">Redundanci dat zajišťuje kontinuity podnikových procesů s odolné proti chybám a vysoce dostupné infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-117">Data redundancy ensures business continuity with a fault tolerant and highly available infrastructure.</span></span>

<span data-ttu-id="c4ca5-118">Další informace o toolearn:</span><span class="sxs-lookup"><span data-stu-id="c4ca5-118">toolearn more about:</span></span>

* <span data-ttu-id="c4ca5-119">Azure Premium storage, najdete v části [tooAzure Úvod Storage úrovně Premium](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-119">Azure Premium storage, see [Introduction tooAzure Premium Storage](../storage/common/storage-premium-storage.md).</span></span>
* <span data-ttu-id="c4ca5-120">Místně redundantní úložiště, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md#locally-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-120">Locally Redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md#locally-redundant-storage).</span></span>

## <a name="azure-storage-blob-snapshots"></a><span data-ttu-id="c4ca5-121">Azure Storage Blob snímky</span><span class="sxs-lookup"><span data-stu-id="c4ca5-121">Azure Storage Blob snapshots</span></span>
<span data-ttu-id="c4ca5-122">Jako výhodou použití služby Azure Premium Storage SQL Data Warehouse používá Azure Storage Blob snímky toobackup hello datového skladu místně.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-122">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots toobackup hello data warehouse locally.</span></span> <span data-ttu-id="c4ca5-123">Můžete obnovit bod obnovení snímku tooa datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-123">You can restore a data warehouse tooa snapshot restore point.</span></span> <span data-ttu-id="c4ca5-124">Snímky spuštění minimálně každých 8 hodin a jsou k dispozici sedm dní.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-124">Snapshots start a minimum of every eight hours and are available for seven days.</span></span>  

<span data-ttu-id="c4ca5-125">Další informace o toolearn:</span><span class="sxs-lookup"><span data-stu-id="c4ca5-125">toolearn more about:</span></span>

* <span data-ttu-id="c4ca5-126">Snímky objektů blob v Azure, najdete v části [vytvoření snímku blob](../storage/blobs/storage-blob-snapshots.md).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-126">Azure blob snapshots, see [Create a blob snapshot](../storage/blobs/storage-blob-snapshots.md).</span></span>

## <a name="geo-redundant-backups"></a><span data-ttu-id="c4ca5-127">Geograficky redundantní zálohy</span><span class="sxs-lookup"><span data-stu-id="c4ca5-127">Geo-redundant backups</span></span>
<span data-ttu-id="c4ca5-128">Každých 24 hodin, SQL Data Warehouse ukládá hello úplné datového skladu do standardního úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-128">Every 24 hours, SQL Data Warehouse stores hello full data warehouse in Standard storage.</span></span> <span data-ttu-id="c4ca5-129">Vítejte úplné datového skladu se vytvoří toomatch hello čas poslední snímek hello.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-129">hello full data warehouse is created toomatch hello time of hello last snapshot.</span></span> <span data-ttu-id="c4ca5-130">standardní úložiště Hello patří účet tooa geograficky redundantní úložiště s přístupem pro čtení (RA-GRS).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-130">hello standard storage belongs tooa geo-redundant storage account with read access (RA-GRS).</span></span> <span data-ttu-id="c4ca5-131">funkce úložiště Azure RA-GRS Hello replikuje hello záložní soubory tooa [spárované datového centra](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-131">hello Azure Storage RA-GRS feature replicates hello backup files tooa [paired data center](../best-practices-availability-paired-regions.md).</span></span> <span data-ttu-id="c4ca5-132">Geografická replikace zajistí, že datový sklad můžete obnovit v případě, že snímky hello nemůže získat přístup k vaší primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-132">This geo-replication ensures you can restore data warehouse in case you cannot access hello snapshots in your primary region.</span></span> 

<span data-ttu-id="c4ca5-133">Tato funkce je ve výchozím.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-133">This feature is on by default.</span></span> <span data-ttu-id="c4ca5-134">Pokud nechcete, aby geograficky redundantní zálohy toouse, vám může [chodit] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-134">If you don't want toouse geo-redundant backups, you can [opt out] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span></span> 

> [!NOTE]
> <span data-ttu-id="c4ca5-135">V úložišti Azure termín hello *replikace* odkazuje toocopying soubory z jednoho umístění tooanother.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-135">In Azure storage, hello term *replication* refers toocopying files from one location tooanother.</span></span> <span data-ttu-id="c4ca5-136">SQL *replikace databáze* odkazuje tookeeping toomultiple sekundární databáze synchronizovat se službou primární databáze.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-136">SQL's *database replication* refers tookeeping toomultiple secondary databases synchronized with a primary database.</span></span> 
> 
> 

> [!NOTE]
> <span data-ttu-id="c4ca5-137">Nelze zamítnutí geograficky redundantní zálohy DWU 9000 a DWU 18000.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-137">You cannot opt out of geo-redundant backups with DWU 9000 and DWU 18000.</span></span> 
>
> 

<span data-ttu-id="c4ca5-138">Další informace o toolearn:</span><span class="sxs-lookup"><span data-stu-id="c4ca5-138">toolearn more about:</span></span>

* <span data-ttu-id="c4ca5-139">Geograficky redundantní úložiště, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-139">Geo-redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md).</span></span>
* <span data-ttu-id="c4ca5-140">RA-GRS úložiště, najdete v části [geograficky redundantní úložiště s přístupem pro čtení](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-140">RA-GRS storage, see [Read-access geo-redundant storage](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span></span>

## <a name="data-warehouse-backup-schedule-and-retention-period"></a><span data-ttu-id="c4ca5-141">Datový sklad plán zálohování a období uchovávání dat</span><span class="sxs-lookup"><span data-stu-id="c4ca5-141">Data warehouse backup schedule and retention period</span></span>
<span data-ttu-id="c4ca5-142">SQL Data Warehouse vytvoří snímky online datových skladů každé čtyři hodiny tooeight a udržuje každý snímek sedm dní.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-142">SQL Data Warehouse creates snapshots on your online data warehouses every four tooeight hours and keeps each snapshot for seven days.</span></span> <span data-ttu-id="c4ca5-143">Můžete obnovit vaše online databáze tooone hello bodů obnovení v hello posledních sedmi dnech.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-143">You can restore your online database tooone of hello restore points in hello past seven days.</span></span> 

<span data-ttu-id="c4ca5-144">toosee při spuštění poslední snímek hello, spusťte tento dotaz na online SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-144">toosee when hello last snapshot started, run this query on your online SQL Data Warehouse.</span></span> 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

<span data-ttu-id="c4ca5-145">Pokud potřebujete tooretain snímek po dobu delší než 7 dní, můžete obnovit nový datový sklad tooa bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-145">If you need tooretain a snapshot for longer than seven days, you can restore a restore point tooa new data warehouse.</span></span> <span data-ttu-id="c4ca5-146">Po dokončení obnovení hello SQL Data Warehouse spustí vytváření snímků na nový datový sklad hello.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-146">After hello restore is finished, SQL Data Warehouse starts creating snapshots on hello new data warehouse.</span></span> <span data-ttu-id="c4ca5-147">Pokud nevytvoříte změny toohello nový datový sklad, hello snímky zůstat prázdné a proto je minimální hello snímku náklady.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-147">If you don't make changes toohello new data warehouse, hello snapshots stay empty and therefore hello snapshot cost is minimal.</span></span> <span data-ttu-id="c4ca5-148">Může také pozastavit tookeep hello databáze SQL Data Warehouse z vytváření snímků.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-148">You could also pause hello database tookeep SQL Data Warehouse from creating snapshots.</span></span>

### <a name="what-happens-toomy-backup-retention-while-my-data-warehouse-is-paused"></a><span data-ttu-id="c4ca5-149">Když můj datového skladu byla pozastavena, co se stane uchovávání záloh toomy?</span><span class="sxs-lookup"><span data-stu-id="c4ca5-149">What happens toomy backup retention while my data warehouse is paused?</span></span>
<span data-ttu-id="c4ca5-150">SQL Data Warehouse nevytváří žádné snímky a nevyprší snímky, zatímco datového skladu je pozastavena.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-150">SQL Data Warehouse does not create snapshots and does not expire snapshots while a data warehouse is paused.</span></span> <span data-ttu-id="c4ca5-151">stáří snímku Hello nezmění při hello datového skladu je pozastavena.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-151">hello snapshot age does not change while hello data warehouse is paused.</span></span> <span data-ttu-id="c4ca5-152">Uchování snímku je založena na hello počet dní, po které hello datový sklad je online, není dny kalendáře.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-152">Snapshot retention is based on hello number of days hello data warehouse is online, not calendar days.</span></span>

<span data-ttu-id="c4ca5-153">Například pokud snímek začíná říjen 1 na 16: 00 a hello datového skladu je pozastavena. října 3 ve 4, je snímek hello dva dny.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-153">For example, if a snapshot starts October 1 at 4 pm and hello data warehouse is paused October 3 at 4 pm, hello snapshot is two days old.</span></span> <span data-ttu-id="c4ca5-154">Vždy, když přejde do režimu online hello datového skladu je snímek hello dva dny.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-154">Whenever hello data warehouse comes back online hello snapshot is two days old.</span></span> <span data-ttu-id="c4ca5-155">Pokud hello datového skladu přechodu do režimu online. října 5 na 16: 00, hello snímku je dva dny a zůstane další pět dní.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-155">If hello data warehouse comes online October 5 at 4 pm, hello snapshot is two days old and remains for five more days.</span></span>

<span data-ttu-id="c4ca5-156">Když hello datového skladu přejde do režimu online, SQL Data Warehouse obnoví nových snímků a vyprší platnost snímků, pokud mají víc než sedm dnů s daty.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-156">When hello data warehouse comes back online, SQL Data Warehouse resumes new snapshots and expires snapshots when they have more than seven days of data.</span></span>

### <a name="how-long-is-hello-retention-period-for-a-dropped-data-warehouse"></a><span data-ttu-id="c4ca5-157">Jak dlouho je doba uchování hello vynechaných datového skladu?</span><span class="sxs-lookup"><span data-stu-id="c4ca5-157">How long is hello retention period for a dropped data warehouse?</span></span>
<span data-ttu-id="c4ca5-158">Pokud datový sklad je vynechána, uložit sedm dní hello datového skladu a hello snímky a pak odebral.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-158">When a data warehouse is dropped, hello data warehouse and hello snapshots are saved for seven days and then removed.</span></span> <span data-ttu-id="c4ca5-159">Můžete obnovit hello datového skladu tooany hello uložit bodů obnovení.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-159">You can restore hello data warehouse tooany of hello saved restore points.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4ca5-160">Pokud odstraníte logické instance systému SQL server, všechny databáze patřící toohello instance budou odstraněny také a nelze jej obnovit.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-160">If you delete a logical SQL server instance, all databases that belong toohello instance are also deleted and cannot be recovered.</span></span> <span data-ttu-id="c4ca5-161">Nelze obnovit odstraněné serveru.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-161">You cannot restore a deleted server.</span></span>
> 
> 

## <a name="data-warehouse-backup-costs"></a><span data-ttu-id="c4ca5-162">Zálohování náklady datového skladu</span><span class="sxs-lookup"><span data-stu-id="c4ca5-162">Data warehouse backup costs</span></span>
<span data-ttu-id="c4ca5-163">Hello celkové náklady pro primární datového skladu a sedm dní snímků objektů Blob v Azure je zaokrouhlené toohello nejbližší TB.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-163">hello total cost for your primary data warehouse and seven days of Azure Blob snapshots is rounded toohello nearest TB.</span></span> <span data-ttu-id="c4ca5-164">Například pokud váš datový sklad je 1,5 TB a hello snímky využívají 100 GB, se vám účtuje 2 TB dat tempem Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-164">For example, if your data warehouse is 1.5 TB and hello snapshots use 100 GB, you are billed for 2 TB of data at Azure Premium Storage rates.</span></span> 

> [!NOTE]
> <span data-ttu-id="c4ca5-165">Každý snímek je původně prázdné a zvětšování jako provedete změny toohello primární datového skladu.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-165">Each snapshot is empty initially and grows as you make changes toohello primary data warehouse.</span></span> <span data-ttu-id="c4ca5-166">Všechny snímky zvýšit velikost jako hello datového skladu změny.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-166">All snapshots increase in size as hello data warehouse changes.</span></span> <span data-ttu-id="c4ca5-167">Proto hello náklady na úložiště pro snímky růst podle rychlosti toohello změny.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-167">Therefore, hello storage costs for snapshots grow according toohello rate of change.</span></span>
> 
> 

<span data-ttu-id="c4ca5-168">Pokud používáte geograficky redundantní úložiště, obdržíte samostatné úložiště poplatků.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-168">If you are using geo-redundant storage, you receive a separate storage charge.</span></span> <span data-ttu-id="c4ca5-169">geograficky redundantní úložiště Hello se fakturuje hello standardní sazbou přístup pro čtení geograficky redundantní úložiště (RA-GRS).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-169">hello geo-redundant storage is billed at hello standard Read-Access Geographically Redundant Storage (RA-GRS) rate.</span></span>

<span data-ttu-id="c4ca5-170">Další informace o cenách služby SQL Data Warehouse najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-170">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="using-database-backups"></a><span data-ttu-id="c4ca5-171">Pomocí zálohování databáze</span><span class="sxs-lookup"><span data-stu-id="c4ca5-171">Using database backups</span></span>
<span data-ttu-id="c4ca5-172">Hello primárního použití pro SQL data warehouse zálohy je toorestore hello datového skladu tooone hello bodů obnovení v rámci doby uchování hello.</span><span class="sxs-lookup"><span data-stu-id="c4ca5-172">hello primary use for SQL data warehouse backups is toorestore hello data warehouse tooone of hello restore points within hello retention period.</span></span>  

* <span data-ttu-id="c4ca5-173">toorestore SQL data warehouse, najdete v části [obnovení SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4ca5-173">toorestore a SQL data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span></span>

## <a name="related-topics"></a><span data-ttu-id="c4ca5-174">Související témata</span><span class="sxs-lookup"><span data-stu-id="c4ca5-174">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="c4ca5-175">Scénáře</span><span class="sxs-lookup"><span data-stu-id="c4ca5-175">Scenarios</span></span>
* <span data-ttu-id="c4ca5-176">Přehled kontinuity obchodních, najdete v části [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="c4ca5-176">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

* <span data-ttu-id="c4ca5-177">toorestore datového skladu, najdete v části [obnovení SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c4ca5-177">toorestore a data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span></span>

<!-- ### Tutorials -->

