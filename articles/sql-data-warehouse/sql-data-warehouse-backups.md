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
# <a name="sql-data-warehouse-backups"></a>Zálohování SQL Data Warehouse
SQL Data Warehouse nabízí místní i zeměpisné záloh v rámci možnosti zálohování datového skladu. Mezi ně patří Azure Storage Blob snímky a geograficky redundantní úložiště. Použijte datového skladu zálohy toorestore vašeho datového skladu tooa obnovení bodu v primární oblasti hello nebo obnovit tooa jiné zeměpisné oblasti. Tento článek vysvětluje hello specifika záloh v SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Co je zálohu datového skladu?
Zálohu datového skladu je hello dat, které můžete použít toorestore určitý čas tooa datového skladu.  Vzhledem k tomu, že datový sklad SQL je distribuovaného systému, zálohu datového skladu se skládá z mnoho souborů, které jsou uložené v Azure BLOB. 

Zálohování databáze jsou nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože se data chránit před náhodnou poškození nebo odstranění. Další informace najdete v tématu [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Redundance dat
SQL Data Warehouse chrání vaše data ukládáním dat v místně redundantní (LRS) Azure Premium Storage. Tato funkce Azure Storage ukládá několik synchronních kopií dat hello v hello místní data center tooguarantee transparentní ochrana dat, pokud existují lokálních selhání. Redundanci dat zajišťuje, že vaše data přežijí problémy infrastruktury, jako je selhání disku. Redundanci dat zajišťuje kontinuity podnikových procesů s odolné proti chybám a vysoce dostupné infrastruktuře.

Další informace o toolearn:

* Azure Premium storage, najdete v části [tooAzure Úvod Storage úrovně Premium](../storage/common/storage-premium-storage.md).
* Místně redundantní úložiště, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md#locally-redundant-storage).

## <a name="azure-storage-blob-snapshots"></a>Azure Storage Blob snímky
Jako výhodou použití služby Azure Premium Storage SQL Data Warehouse používá Azure Storage Blob snímky toobackup hello datového skladu místně. Můžete obnovit bod obnovení snímku tooa datového skladu. Snímky spuštění minimálně každých 8 hodin a jsou k dispozici sedm dní.  

Další informace o toolearn:

* Snímky objektů blob v Azure, najdete v části [vytvoření snímku blob](../storage/blobs/storage-blob-snapshots.md).

## <a name="geo-redundant-backups"></a>Geograficky redundantní zálohy
Každých 24 hodin, SQL Data Warehouse ukládá hello úplné datového skladu do standardního úložiště. Vítejte úplné datového skladu se vytvoří toomatch hello čas poslední snímek hello. standardní úložiště Hello patří účet tooa geograficky redundantní úložiště s přístupem pro čtení (RA-GRS). funkce úložiště Azure RA-GRS Hello replikuje hello záložní soubory tooa [spárované datového centra](../best-practices-availability-paired-regions.md). Geografická replikace zajistí, že datový sklad můžete obnovit v případě, že snímky hello nemůže získat přístup k vaší primární oblasti. 

Tato funkce je ve výchozím. Pokud nechcete, aby geograficky redundantní zálohy toouse, vám může [chodit] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn). 

> [!NOTE]
> V úložišti Azure termín hello *replikace* odkazuje toocopying soubory z jednoho umístění tooanother. SQL *replikace databáze* odkazuje tookeeping toomultiple sekundární databáze synchronizovat se službou primární databáze. 
> 
> 

> [!NOTE]
> Nelze zamítnutí geograficky redundantní zálohy DWU 9000 a DWU 18000. 
>
> 

Další informace o toolearn:

* Geograficky redundantní úložiště, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md).
* RA-GRS úložiště, najdete v části [geograficky redundantní úložiště s přístupem pro čtení](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Datový sklad plán zálohování a období uchovávání dat
SQL Data Warehouse vytvoří snímky online datových skladů každé čtyři hodiny tooeight a udržuje každý snímek sedm dní. Můžete obnovit vaše online databáze tooone hello bodů obnovení v hello posledních sedmi dnech. 

toosee při spuštění poslední snímek hello, spusťte tento dotaz na online SQL Data Warehouse. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Pokud potřebujete tooretain snímek po dobu delší než 7 dní, můžete obnovit nový datový sklad tooa bodu obnovení. Po dokončení obnovení hello SQL Data Warehouse spustí vytváření snímků na nový datový sklad hello. Pokud nevytvoříte změny toohello nový datový sklad, hello snímky zůstat prázdné a proto je minimální hello snímku náklady. Může také pozastavit tookeep hello databáze SQL Data Warehouse z vytváření snímků.

### <a name="what-happens-toomy-backup-retention-while-my-data-warehouse-is-paused"></a>Když můj datového skladu byla pozastavena, co se stane uchovávání záloh toomy?
SQL Data Warehouse nevytváří žádné snímky a nevyprší snímky, zatímco datového skladu je pozastavena. stáří snímku Hello nezmění při hello datového skladu je pozastavena. Uchování snímku je založena na hello počet dní, po které hello datový sklad je online, není dny kalendáře.

Například pokud snímek začíná říjen 1 na 16: 00 a hello datového skladu je pozastavena. října 3 ve 4, je snímek hello dva dny. Vždy, když přejde do režimu online hello datového skladu je snímek hello dva dny. Pokud hello datového skladu přechodu do režimu online. října 5 na 16: 00, hello snímku je dva dny a zůstane další pět dní.

Když hello datového skladu přejde do režimu online, SQL Data Warehouse obnoví nových snímků a vyprší platnost snímků, pokud mají víc než sedm dnů s daty.

### <a name="how-long-is-hello-retention-period-for-a-dropped-data-warehouse"></a>Jak dlouho je doba uchování hello vynechaných datového skladu?
Pokud datový sklad je vynechána, uložit sedm dní hello datového skladu a hello snímky a pak odebral. Můžete obnovit hello datového skladu tooany hello uložit bodů obnovení.

> [!IMPORTANT]
> Pokud odstraníte logické instance systému SQL server, všechny databáze patřící toohello instance budou odstraněny také a nelze jej obnovit. Nelze obnovit odstraněné serveru.
> 
> 

## <a name="data-warehouse-backup-costs"></a>Zálohování náklady datového skladu
Hello celkové náklady pro primární datového skladu a sedm dní snímků objektů Blob v Azure je zaokrouhlené toohello nejbližší TB. Například pokud váš datový sklad je 1,5 TB a hello snímky využívají 100 GB, se vám účtuje 2 TB dat tempem Azure Premium Storage. 

> [!NOTE]
> Každý snímek je původně prázdné a zvětšování jako provedete změny toohello primární datového skladu. Všechny snímky zvýšit velikost jako hello datového skladu změny. Proto hello náklady na úložiště pro snímky růst podle rychlosti toohello změny.
> 
> 

Pokud používáte geograficky redundantní úložiště, obdržíte samostatné úložiště poplatků. geograficky redundantní úložiště Hello se fakturuje hello standardní sazbou přístup pro čtení geograficky redundantní úložiště (RA-GRS).

Další informace o cenách služby SQL Data Warehouse najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Pomocí zálohování databáze
Hello primárního použití pro SQL data warehouse zálohy je toorestore hello datového skladu tooone hello bodů obnovení v rámci doby uchování hello.  

* toorestore SQL data warehouse, najdete v části [obnovení SQL data warehouse](sql-data-warehouse-restore-database-overview.md).

## <a name="related-topics"></a>Související témata
### <a name="scenarios"></a>Scénáře
* Přehled kontinuity obchodních, najdete v části [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

* toorestore datového skladu, najdete v části [obnovení SQL data warehouse](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

