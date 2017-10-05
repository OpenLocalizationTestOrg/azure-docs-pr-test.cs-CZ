---
title: "Zálohování Azure SQL Data Warehouse – snímky geograficky redundantní | Microsoft Docs"
description: "Další informace o zálohování integrovanou databází SQL Data Warehouse, které vám umožní obnovit do bodu obnovení nebo jiné zeměpisné oblasti Azure SQL Data Warehouse."
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
ms.openlocfilehash: 54c0149a769e654139bbdf709802d49127f041ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="sql-data-warehouse-backups"></a><span data-ttu-id="4dc1d-103">Zálohování SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4dc1d-103">SQL Data Warehouse backups</span></span>
<span data-ttu-id="4dc1d-104">SQL Data Warehouse nabízí místní i zeměpisné záloh v rámci možnosti zálohování datového skladu.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-104">SQL Data Warehouse offers both local and geographical backups as part of its data warehouse backup capabilities.</span></span> <span data-ttu-id="4dc1d-105">Mezi ně patří Azure Storage Blob snímky a geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-105">These include Azure Storage Blob snapshots and geo-redundant storage.</span></span> <span data-ttu-id="4dc1d-106">Pomocí zálohování datového skladu obnovit váš datový sklad na bod obnovení v primární oblasti nebo obnovit do jiné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-106">Use data warehouse backups to restore your data warehouse to a restore point in the primary region, or restore to a different geographical region.</span></span> <span data-ttu-id="4dc1d-107">Tento článek vysvětluje, jaké jsou specifikace záloh v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-107">This article explains the specifics of backups in SQL Data Warehouse.</span></span>

## <a name="what-is-a-data-warehouse-backup"></a><span data-ttu-id="4dc1d-108">Co je zálohu datového skladu?</span><span class="sxs-lookup"><span data-stu-id="4dc1d-108">What is a data warehouse backup?</span></span>
<span data-ttu-id="4dc1d-109">Zálohu datového skladu jsou data, která můžete použít k obnovení datového skladu v určený čas.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-109">A data warehouse backup is the data that you can use to restore a data warehouse to a specific time.</span></span>  <span data-ttu-id="4dc1d-110">Vzhledem k tomu, že datový sklad SQL je distribuovaného systému, zálohu datového skladu se skládá z mnoho souborů, které jsou uložené v Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-110">Since SQL Data Warehouse is a distributed system, a data warehouse backup consists of many files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="4dc1d-111">Zálohování databáze jsou nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože se data chránit před náhodnou poškození nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-111">Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.</span></span> <span data-ttu-id="4dc1d-112">Další informace najdete v tématu [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-112">For more information, see [Business continuity overview](../sql-database/sql-database-business-continuity.md).</span></span>

## <a name="data-redundancy"></a><span data-ttu-id="4dc1d-113">Redundance dat</span><span class="sxs-lookup"><span data-stu-id="4dc1d-113">Data redundancy</span></span>
<span data-ttu-id="4dc1d-114">SQL Data Warehouse chrání vaše data ukládáním dat v místně redundantní (LRS) Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-114">SQL Data Warehouse protects your data by storing your data in locally redundant (LRS) Azure Premium Storage.</span></span> <span data-ttu-id="4dc1d-115">Tato funkce Azure Storage ukládá několik synchronních kopií dat v místní datové centrum tak zajistila transparentní ochrana dat, pokud existují lokálních selhání.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-115">This Azure Storage feature stores multiple synchronous copies of the data in the local data center to guarantee transparent data protection if there are localized failures.</span></span> <span data-ttu-id="4dc1d-116">Redundanci dat zajišťuje, že vaše data přežijí problémy infrastruktury, jako je selhání disku.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-116">Data redundancy ensures that your data can survive infrastructure issues such as disk failures.</span></span> <span data-ttu-id="4dc1d-117">Redundanci dat zajišťuje kontinuity podnikových procesů s odolné proti chybám a vysoce dostupné infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-117">Data redundancy ensures business continuity with a fault tolerant and highly available infrastructure.</span></span>

<span data-ttu-id="4dc1d-118">Další informace o:</span><span class="sxs-lookup"><span data-stu-id="4dc1d-118">To learn more about:</span></span>

* <span data-ttu-id="4dc1d-119">Azure Premium storage, najdete v části [Úvod do Azure Premium Storage](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-119">Azure Premium storage, see [Introduction to Azure Premium Storage](../storage/common/storage-premium-storage.md).</span></span>
* <span data-ttu-id="4dc1d-120">Místně redundantní úložiště, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md#locally-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-120">Locally Redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md#locally-redundant-storage).</span></span>

## <a name="azure-storage-blob-snapshots"></a><span data-ttu-id="4dc1d-121">Azure Storage Blob snímky</span><span class="sxs-lookup"><span data-stu-id="4dc1d-121">Azure Storage Blob snapshots</span></span>
<span data-ttu-id="4dc1d-122">Jako výhodou používání Azure Premium Storage SQL Data Warehouse používá snímky Azure Storage Blob pro zálohování do datového skladu místně.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-122">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots to backup the data warehouse locally.</span></span> <span data-ttu-id="4dc1d-123">Datový sklad můžete obnovit do bodu obnovení snímku.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-123">You can restore a data warehouse to a snapshot restore point.</span></span> <span data-ttu-id="4dc1d-124">Snímky spuštění minimálně každých 8 hodin a jsou k dispozici sedm dní.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-124">Snapshots start a minimum of every eight hours and are available for seven days.</span></span>  

<span data-ttu-id="4dc1d-125">Další informace o:</span><span class="sxs-lookup"><span data-stu-id="4dc1d-125">To learn more about:</span></span>

* <span data-ttu-id="4dc1d-126">Snímky objektů blob v Azure, najdete v části [vytvoření snímku blob](../storage/blobs/storage-blob-snapshots.md).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-126">Azure blob snapshots, see [Create a blob snapshot](../storage/blobs/storage-blob-snapshots.md).</span></span>

## <a name="geo-redundant-backups"></a><span data-ttu-id="4dc1d-127">Geograficky redundantní zálohy</span><span class="sxs-lookup"><span data-stu-id="4dc1d-127">Geo-redundant backups</span></span>
<span data-ttu-id="4dc1d-128">Každých 24 hodin, SQL Data Warehouse ukládá úplnou datového skladu do standardního úložiště.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-128">Every 24 hours, SQL Data Warehouse stores the full data warehouse in Standard storage.</span></span> <span data-ttu-id="4dc1d-129">Úplné datového skladu je vytvořit tak, aby odpovídaly čas poslední snímek.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-129">The full data warehouse is created to match the time of the last snapshot.</span></span> <span data-ttu-id="4dc1d-130">Standardní úložiště patří k účtu geograficky redundantní úložiště s přístupem pro čtení (RA-GRS).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-130">The standard storage belongs to a geo-redundant storage account with read access (RA-GRS).</span></span> <span data-ttu-id="4dc1d-131">Funkce úložiště Azure RA-GRS replikuje záložní soubory do [spárované datového centra](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-131">The Azure Storage RA-GRS feature replicates the backup files to a [paired data center](../best-practices-availability-paired-regions.md).</span></span> <span data-ttu-id="4dc1d-132">Geografická replikace zajistí, že datový sklad můžete obnovit v případě, že snímky nelze získat přístup v primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-132">This geo-replication ensures you can restore data warehouse in case you cannot access the snapshots in your primary region.</span></span> 

<span data-ttu-id="4dc1d-133">Tato funkce je ve výchozím.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-133">This feature is on by default.</span></span> <span data-ttu-id="4dc1d-134">Pokud nechcete použít geograficky redundantní zálohy, které můžete [chodit] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-134">If you don't want to use geo-redundant backups, you can [opt out] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span></span> 

> [!NOTE]
> <span data-ttu-id="4dc1d-135">V úložišti Azure termín *replikace* odkazuje na kopírování souborů z jednoho umístění do druhého.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-135">In Azure storage, the term *replication* refers to copying files from one location to another.</span></span> <span data-ttu-id="4dc1d-136">SQL *replikace databáze* odkazuje na zadáte-li více sekundárních databází synchronizovány s primární databáze.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-136">SQL's *database replication* refers to keeping to multiple secondary databases synchronized with a primary database.</span></span> 
> 
> 

> [!NOTE]
> <span data-ttu-id="4dc1d-137">Nelze zamítnutí geograficky redundantní zálohy DWU 9000 a DWU 18000.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-137">You cannot opt out of geo-redundant backups with DWU 9000 and DWU 18000.</span></span> 
>
> 

<span data-ttu-id="4dc1d-138">Další informace o:</span><span class="sxs-lookup"><span data-stu-id="4dc1d-138">To learn more about:</span></span>

* <span data-ttu-id="4dc1d-139">Geograficky redundantní úložiště, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-139">Geo-redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md).</span></span>
* <span data-ttu-id="4dc1d-140">RA-GRS úložiště, najdete v části [geograficky redundantní úložiště s přístupem pro čtení](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-140">RA-GRS storage, see [Read-access geo-redundant storage](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span></span>

## <a name="data-warehouse-backup-schedule-and-retention-period"></a><span data-ttu-id="4dc1d-141">Datový sklad plán zálohování a období uchovávání dat</span><span class="sxs-lookup"><span data-stu-id="4dc1d-141">Data warehouse backup schedule and retention period</span></span>
<span data-ttu-id="4dc1d-142">SQL Data Warehouse vytvoří snímky na online datových skladů každých čtyř do osmi hodin a udržuje jednotlivých snímků sedm dní.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-142">SQL Data Warehouse creates snapshots on your online data warehouses every four to eight hours and keeps each snapshot for seven days.</span></span> <span data-ttu-id="4dc1d-143">Online databáze můžete obnovit k jednomu z bodů obnovení v posledních sedmi dnech.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-143">You can restore your online database to one of the restore points in the past seven days.</span></span> 

<span data-ttu-id="4dc1d-144">Pokud chcete zobrazit při spuštění poslední snímek, spusťte tento dotaz na online SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-144">To see when the last snapshot started, run this query on your online SQL Data Warehouse.</span></span> 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

<span data-ttu-id="4dc1d-145">Pokud je potřeba zachovat snímek po dobu delší než 7 dní, můžete obnovit bod obnovení na nový datový sklad.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-145">If you need to retain a snapshot for longer than seven days, you can restore a restore point to a new data warehouse.</span></span> <span data-ttu-id="4dc1d-146">Po dokončení obnovení SQL Data Warehouse spustí vytváření snímků na nový datový sklad.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-146">After the restore is finished, SQL Data Warehouse starts creating snapshots on the new data warehouse.</span></span> <span data-ttu-id="4dc1d-147">Pokud neprovedete změny do nového datového skladu, snímky zůstane prázdný a proto je minimální náklady na snímku.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-147">If you don't make changes to the new data warehouse, the snapshots stay empty and therefore the snapshot cost is minimal.</span></span> <span data-ttu-id="4dc1d-148">Může také pozastavit databázi do SQL Data Warehouse zabránit ve vytváření snímků.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-148">You could also pause the database to keep SQL Data Warehouse from creating snapshots.</span></span>

### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a><span data-ttu-id="4dc1d-149">Co se stane Moje uchovávání záloh, zatímco Moje datového skladu byla pozastavena?</span><span class="sxs-lookup"><span data-stu-id="4dc1d-149">What happens to my backup retention while my data warehouse is paused?</span></span>
<span data-ttu-id="4dc1d-150">SQL Data Warehouse nevytváří žádné snímky a nevyprší snímky, zatímco datového skladu je pozastavena.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-150">SQL Data Warehouse does not create snapshots and does not expire snapshots while a data warehouse is paused.</span></span> <span data-ttu-id="4dc1d-151">Stáří snímku nedojde ke změně při datového skladu je pozastavena.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-151">The snapshot age does not change while the data warehouse is paused.</span></span> <span data-ttu-id="4dc1d-152">Počet dnů, po který datový sklad je online, není dny kalendáře vychází uchování snímku.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-152">Snapshot retention is based on the number of days the data warehouse is online, not calendar days.</span></span>

<span data-ttu-id="4dc1d-153">Například pokud snímku spustí říjen 1 na 16: 00 a datového skladu je pozastavena. října 3 na 16: 00, snímku je dva dny.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-153">For example, if a snapshot starts October 1 at 4 pm and the data warehouse is paused October 3 at 4 pm, the snapshot is two days old.</span></span> <span data-ttu-id="4dc1d-154">Vždy, když přejde do režimu online datový sklad je snímek dva dny.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-154">Whenever the data warehouse comes back online the snapshot is two days old.</span></span> <span data-ttu-id="4dc1d-155">Pokud datový sklad přechodu do režimu online. října 5 na 16: 00, snímku je dva dny a zůstane další pět dní.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-155">If the data warehouse comes online October 5 at 4 pm, the snapshot is two days old and remains for five more days.</span></span>

<span data-ttu-id="4dc1d-156">Když datového skladu přejde do režimu online, SQL Data Warehouse obnoví nových snímků a vyprší platnost snímků, pokud mají víc než sedm dnů s daty.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-156">When the data warehouse comes back online, SQL Data Warehouse resumes new snapshots and expires snapshots when they have more than seven days of data.</span></span>

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a><span data-ttu-id="4dc1d-157">Jak dlouho je doba uchování vynechaných datového skladu?</span><span class="sxs-lookup"><span data-stu-id="4dc1d-157">How long is the retention period for a dropped data warehouse?</span></span>
<span data-ttu-id="4dc1d-158">Když datový sklad je vynechána, datový sklad a snímky jsou uložit sedm dní a pak odebrat.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-158">When a data warehouse is dropped, the data warehouse and the snapshots are saved for seven days and then removed.</span></span> <span data-ttu-id="4dc1d-159">Datový sklad můžete obnovit všechny body obnovení uložené.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-159">You can restore the data warehouse to any of the saved restore points.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4dc1d-160">Pokud odstraníte logické instance systému SQL server, všechny databáze patřící k instanci budou také odstraněny a nelze jej obnovit.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-160">If you delete a logical SQL server instance, all databases that belong to the instance are also deleted and cannot be recovered.</span></span> <span data-ttu-id="4dc1d-161">Nelze obnovit odstraněné serveru.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-161">You cannot restore a deleted server.</span></span>
> 
> 

## <a name="data-warehouse-backup-costs"></a><span data-ttu-id="4dc1d-162">Zálohování náklady datového skladu</span><span class="sxs-lookup"><span data-stu-id="4dc1d-162">Data warehouse backup costs</span></span>
<span data-ttu-id="4dc1d-163">Celková cena vaší primární datového skladu a sedm dní snímků objektů Blob v Azure se zaokrouhlí na nejbližší TB.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-163">The total cost for your primary data warehouse and seven days of Azure Blob snapshots is rounded to the nearest TB.</span></span> <span data-ttu-id="4dc1d-164">Například pokud váš datový sklad je 1,5 TB a snímky používat 100 GB, se vám účtuje 2 TB dat tempem Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-164">For example, if your data warehouse is 1.5 TB and the snapshots use 100 GB, you are billed for 2 TB of data at Azure Premium Storage rates.</span></span> 

> [!NOTE]
> <span data-ttu-id="4dc1d-165">Každý snímek je původně prázdné a zvětšování při provádění změn do primárního datového skladu.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-165">Each snapshot is empty initially and grows as you make changes to the primary data warehouse.</span></span> <span data-ttu-id="4dc1d-166">Všechny snímky zvýšit velikost jako změny datového skladu.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-166">All snapshots increase in size as the data warehouse changes.</span></span> <span data-ttu-id="4dc1d-167">Proto náklady na úložiště pro snímky růst podle rychlost změny.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-167">Therefore, the storage costs for snapshots grow according to the rate of change.</span></span>
> 
> 

<span data-ttu-id="4dc1d-168">Pokud používáte geograficky redundantní úložiště, obdržíte samostatné úložiště poplatků.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-168">If you are using geo-redundant storage, you receive a separate storage charge.</span></span> <span data-ttu-id="4dc1d-169">Geograficky redundantní úložiště se fakturuje standardní sazbou přístup pro čtení geograficky redundantní úložiště (RA-GRS).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-169">The geo-redundant storage is billed at the standard Read-Access Geographically Redundant Storage (RA-GRS) rate.</span></span>

<span data-ttu-id="4dc1d-170">Další informace o cenách služby SQL Data Warehouse najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-170">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="using-database-backups"></a><span data-ttu-id="4dc1d-171">Pomocí zálohování databáze</span><span class="sxs-lookup"><span data-stu-id="4dc1d-171">Using database backups</span></span>
<span data-ttu-id="4dc1d-172">Primárního použití pro SQL data warehouse zálohy je obnovení datového skladu k jednomu z bodů obnovení v rámci dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="4dc1d-172">The primary use for SQL data warehouse backups is to restore the data warehouse to one of the restore points within the retention period.</span></span>  

* <span data-ttu-id="4dc1d-173">Chcete-li obnovit SQL data warehouse, najdete v části [obnovení SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4dc1d-173">To restore a SQL data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span></span>

## <a name="related-topics"></a><span data-ttu-id="4dc1d-174">Související témata</span><span class="sxs-lookup"><span data-stu-id="4dc1d-174">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="4dc1d-175">Scénáře</span><span class="sxs-lookup"><span data-stu-id="4dc1d-175">Scenarios</span></span>
* <span data-ttu-id="4dc1d-176">Přehled kontinuity obchodních, najdete v části [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="4dc1d-176">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

* <span data-ttu-id="4dc1d-177">Chcete-li obnovit datového skladu, přečtěte si téma [obnovení SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4dc1d-177">To restore a data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span></span>

<!-- ### Tutorials -->

