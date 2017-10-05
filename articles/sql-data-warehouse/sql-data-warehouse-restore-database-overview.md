---
title: "Obnovení Azure datového skladu, a-místní a geograficky redundantní | Microsoft Docs"
description: "Přehled možností obnovení databáze pro obnovení databáze v Azure SQL Data Warehouse."
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
ms.openlocfilehash: ea42b7135d0695b66d569095e70bb3d9f8b9594b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="sql-data-warehouse-restore"></a><span data-ttu-id="4d93c-103">Obnovení SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4d93c-103">SQL Data Warehouse restore</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="4d93c-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="4d93c-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="4d93c-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="4d93c-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="4d93c-106">[Prostředí PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="4d93c-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="4d93c-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="4d93c-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="4d93c-108">SQL Data Warehouse nabízí místní i zeměpisné obnovení jako součást po havárii jeho datového skladu obnovení.</span><span class="sxs-lookup"><span data-stu-id="4d93c-108">SQL Data Warehouse offers both local and geographical restores as part of its data warehouse disaster recovery capabilities.</span></span> <span data-ttu-id="4d93c-109">Použijte k obnovení datového skladu pro bod obnovení v primární oblasti datového skladu zálohy nebo použijte geograficky redundantní zálohy obnovit do jiné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="4d93c-109">Use data warehouse backups to restore your data warehouse to a restore point in the primary region, or use geo-redundant backups to restore to a different geographical region.</span></span> <span data-ttu-id="4d93c-110">Tento článek vysvětluje, jaké jsou specifikace obnovení datového skladu.</span><span class="sxs-lookup"><span data-stu-id="4d93c-110">This article explains the specifics of restoring a data warehouse.</span></span>

## <a name="what-is-a-data-warehouse-restore"></a><span data-ttu-id="4d93c-111">Co je datový sklad obnovení?</span><span class="sxs-lookup"><span data-stu-id="4d93c-111">What is a data warehouse restore?</span></span>
<span data-ttu-id="4d93c-112">Obnovení datového skladu je nový datový sklad, který je vytvořený z existující zálohy nebo odstraněné datového skladu.</span><span class="sxs-lookup"><span data-stu-id="4d93c-112">A data warehouse restore is a new data warehouse that is created from a backup of an existing or deleted data warehouse.</span></span> <span data-ttu-id="4d93c-113">Obnovená data warehouse znovu vytvoří skladu zálohovaná data v určitém čase.</span><span class="sxs-lookup"><span data-stu-id="4d93c-113">The restored data warehouse re-creates the backed-up data warehouse at a specific time.</span></span> <span data-ttu-id="4d93c-114">Vzhledem k tomu, že SQL Data Warehouse je distribuovaný systém, obnovení datového skladu je vytvořený z mnoha záložní soubory, které jsou uložené v Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="4d93c-114">Since SQL Data Warehouse is a distributed system, a data warehouse restore is created from many backup files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="4d93c-115">Obnovení databáze je nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože znovu vytváří data po náhodného poškození nebo odstranění.</span><span class="sxs-lookup"><span data-stu-id="4d93c-115">Database restore is an essential part of any business continuity and disaster recovery strategy because it re-creates your data after accidental corruption or deletion.</span></span>

<span data-ttu-id="4d93c-116">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="4d93c-116">For more information, see:</span></span>

* [<span data-ttu-id="4d93c-117">Zálohování SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4d93c-117">SQL Data Warehouse backups</span></span>](sql-data-warehouse-backups.md)
* [<span data-ttu-id="4d93c-118">Přehled kontinuity obchodních</span><span class="sxs-lookup"><span data-stu-id="4d93c-118">Business continuity overview</span></span>](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a><span data-ttu-id="4d93c-119">Body obnovení datového skladu</span><span class="sxs-lookup"><span data-stu-id="4d93c-119">Data warehouse restore points</span></span>
<span data-ttu-id="4d93c-120">Jako výhodou používání Azure Premium Storage SQL Data Warehouse používá snímky Azure Storage Blob pro zálohování primárního datového skladu.</span><span class="sxs-lookup"><span data-stu-id="4d93c-120">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots to backup the primary data warehouse.</span></span> <span data-ttu-id="4d93c-121">Každý snímek je bod obnovení, který představuje spuštění snímku.</span><span class="sxs-lookup"><span data-stu-id="4d93c-121">Each snapshot has a restore point that represents the time the snapshot started.</span></span> <span data-ttu-id="4d93c-122">Chcete-li obnovit datového skladu, vyberte bod obnovení a vydejte příkaz restore.</span><span class="sxs-lookup"><span data-stu-id="4d93c-122">To restore a data warehouse, you choose a restore point and issue a restore command.</span></span>  

<span data-ttu-id="4d93c-123">SQL Data Warehouse vždy obnoví zálohování na nový datový sklad.</span><span class="sxs-lookup"><span data-stu-id="4d93c-123">SQL Data Warehouse always restores the backup to a new data warehouse.</span></span> <span data-ttu-id="4d93c-124">Můžete buď ponechat obnovené datový sklad a stávající nebo odstraňte jednu z nich.</span><span class="sxs-lookup"><span data-stu-id="4d93c-124">You can either keep the restored data warehouse and the current one, or delete one of them.</span></span> <span data-ttu-id="4d93c-125">Pokud chcete nahradit aktuální datového skladu obnovené datového skladu, můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="4d93c-125">If you want to replace the current data warehouse with the restored data warehouse, you can rename it.</span></span>

<span data-ttu-id="4d93c-126">Pokud potřebujete obnovit odstraněné nebo pozastavený datového skladu, můžete [vytvořit lístek podpory](sql-data-warehouse-get-started-create-support-ticket.md).</span><span class="sxs-lookup"><span data-stu-id="4d93c-126">If you need to restore a deleted or paused data warehouse, you can [create a support ticket](sql-data-warehouse-get-started-create-support-ticket.md).</span></span> 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a><span data-ttu-id="4d93c-127">Geograficky redundantní obnovení</span><span class="sxs-lookup"><span data-stu-id="4d93c-127">Geo-redundant restore</span></span>
<span data-ttu-id="4d93c-128">Datový sklad můžete obnovit do libovolné oblasti podpora Azure SQL Data Warehouse stejné úrovni zvolené výkonu.</span><span class="sxs-lookup"><span data-stu-id="4d93c-128">You can restore your data warehouse to any region supporting Azure SQL Data Warehouse at your chosen performance level.</span></span> <span data-ttu-id="4d93c-129">Upozorňujeme, že 9000 a 18000 DWU nejsou podporovány ve všech oblastech ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="4d93c-129">Please note that 9000 and 18000 DWU are not supported in all regions during the preview.</span></span>

> [!NOTE]
> <span data-ttu-id="4d93c-130">Geograficky redundantní obnovení nesmí Nesouhlasili jste tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="4d93c-130">To perform a geo-redundant restore you must not have opted out of this feature.</span></span>
> 
> 

## <a name="restore-timeline"></a><span data-ttu-id="4d93c-131">Obnovení časové osy</span><span class="sxs-lookup"><span data-stu-id="4d93c-131">Restore timeline</span></span>
<span data-ttu-id="4d93c-132">Databázi můžete obnovit do libovolného bodu obnovení k dispozici v posledních sedmi dnů.</span><span class="sxs-lookup"><span data-stu-id="4d93c-132">You can restore a database to any available restore point within the last seven days.</span></span> <span data-ttu-id="4d93c-133">Snímky spuštění každých čtyř do osmi hodin a jsou k dispozici sedm dní.</span><span class="sxs-lookup"><span data-stu-id="4d93c-133">Snapshots start every four to eight hours and are available for seven days.</span></span> <span data-ttu-id="4d93c-134">Pokud snímek je starší než 7 dní, vypršení jeho platnosti a jeho bod obnovení již není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4d93c-134">When a snapshot is older than seven days, it expires and its restore point is no longer available.</span></span>

## <a name="restore-costs"></a><span data-ttu-id="4d93c-135">Obnovení náklady</span><span class="sxs-lookup"><span data-stu-id="4d93c-135">Restore costs</span></span>
<span data-ttu-id="4d93c-136">Zřizování úložiště pro obnovená data warehouse se fakturuje rychlostí Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="4d93c-136">The storage charge for the restored data warehouse is billed at the Azure Premium Storage rate.</span></span> 

<span data-ttu-id="4d93c-137">Pokud pozastavíte obnovené datového skladu, budou se vám účtovat pro úložiště rychlostí Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="4d93c-137">If you pause a restored data warehouse, you are charged for storage at the Azure Premium Storage rate.</span></span> <span data-ttu-id="4d93c-138">Výhodou pozastavení je, že se vám neúčtují poplatky za výpočetní prostředky DWU.</span><span class="sxs-lookup"><span data-stu-id="4d93c-138">The advantage of pausing is you are not charged for the DWU computing resources.</span></span>

<span data-ttu-id="4d93c-139">Další informace o cenách služby SQL Data Warehouse najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="4d93c-139">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="uses-for-restore"></a><span data-ttu-id="4d93c-140">Používá pro obnovení</span><span class="sxs-lookup"><span data-stu-id="4d93c-140">Uses for restore</span></span>
<span data-ttu-id="4d93c-141">Primární použití pro obnovení datového skladu je k obnovení dat po před náhodnou ztrátou dat nebo poškození.</span><span class="sxs-lookup"><span data-stu-id="4d93c-141">The primary use for data warehouse restore is to recover data after accidental data loss or corruption.</span></span>

<span data-ttu-id="4d93c-142">Obnovení datového skladu můžete taky uchovávat zálohu po dobu delší než 7 dní.</span><span class="sxs-lookup"><span data-stu-id="4d93c-142">You can also use data warehouse restore to retain a backup for longer than seven days.</span></span> <span data-ttu-id="4d93c-143">Po obnovení zálohy online máte datového skladu a můžete ho po neomezenou dobu, abyste ušetřili náklady výpočetní pozastavit.</span><span class="sxs-lookup"><span data-stu-id="4d93c-143">Once the backup is restored, you have the data warehouse online and can pause it indefinitely to save compute costs.</span></span> <span data-ttu-id="4d93c-144">Pozastavený databáze způsobuje poplatky za úložiště rychlostí Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="4d93c-144">The paused database incurs storage charges at the Azure Premium Storage rate.</span></span> 

## <a name="related-topics"></a><span data-ttu-id="4d93c-145">Související témata</span><span class="sxs-lookup"><span data-stu-id="4d93c-145">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="4d93c-146">Scénáře</span><span class="sxs-lookup"><span data-stu-id="4d93c-146">Scenarios</span></span>
* <span data-ttu-id="4d93c-147">Přehled kontinuity obchodních, najdete v části [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="4d93c-147">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

<span data-ttu-id="4d93c-148">Pokud chcete provést obnovení datového skladu, obnovte pomocí:</span><span class="sxs-lookup"><span data-stu-id="4d93c-148">To perform a data warehouse restore, restore using:</span></span>

* <span data-ttu-id="4d93c-149">Azure portálu, najdete v tématu [obnovit data warehouse pomocí portálu Azure](sql-data-warehouse-restore-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="4d93c-149">Azure portal, see [Restore a data warehouse using the Azure portal](sql-data-warehouse-restore-database-portal.md)</span></span>
* <span data-ttu-id="4d93c-150">Rutiny prostředí PowerShell najdete v části [obnovení datového skladu pomocí rutin prostředí PowerShell](sql-data-warehouse-restore-database-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="4d93c-150">PowerShell cmdlets, see [Restore a data warehouse using PowerShell cmdlets](sql-data-warehouse-restore-database-powershell.md)</span></span>
* <span data-ttu-id="4d93c-151">Rozhraní REST API najdete v tématu [obnovit data warehouse pomocí rozhraní REST API](sql-data-warehouse-restore-database-rest-api.md)</span><span class="sxs-lookup"><span data-stu-id="4d93c-151">REST APIs, see [Restore a data warehouse using the REST APIs](sql-data-warehouse-restore-database-rest-api.md)</span></span>

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
