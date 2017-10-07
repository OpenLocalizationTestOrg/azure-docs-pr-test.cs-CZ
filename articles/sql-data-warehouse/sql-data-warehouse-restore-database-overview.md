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
# <a name="sql-data-warehouse-restore"></a>Obnovení SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Portál][Portal]
> * [Prostředí PowerShell][PowerShell]
> * [REST][REST]
> 
> 

SQL Data Warehouse nabízí místní i zeměpisné obnovení jako součást po havárii jeho datového skladu obnovení. Použijte datového skladu zálohy toorestore vašeho datového skladu tooa obnovení bodu v primární oblasti hello nebo použít geograficky redundantní zálohy toorestore tooa jiné zeměpisné oblasti. Tento článek vysvětluje hello specifika obnovení datového skladu.

## <a name="what-is-a-data-warehouse-restore"></a>Co je datový sklad obnovení?
Obnovení datového skladu je nový datový sklad, který je vytvořený z existující zálohy nebo odstraněné datového skladu. Hello obnovené datového skladu znovu vytvoří hello zálohovaná datového skladu v určitém čase. Vzhledem k tomu, že SQL Data Warehouse je distribuovaný systém, obnovení datového skladu je vytvořený z mnoha záložní soubory, které jsou uložené v Azure BLOB. 

Obnovení databáze je nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože znovu vytváří data po náhodného poškození nebo odstranění.

Další informace naleznete v tématu:

* [Zálohování SQL Data Warehouse](sql-data-warehouse-backups.md)
* [Přehled kontinuity obchodních](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Body obnovení datového skladu
Jako výhodou použití služby Azure Premium Storage SQL Data Warehouse používá Azure Storage Blob snímky toobackup hello primární datového skladu. Každý snímek je bod obnovení, který představuje hello čas spuštění snímku hello. toorestore datového skladu, vyberte bod obnovení a vydejte příkaz restore.  

SQL Data Warehouse vždy obnoví hello zálohování tooa nový datový sklad. Můžete buď ponechat hello obnovené datového skladu a hello stávající nebo odstraňte jednu z nich. Pokud chcete tooreplace hello aktuální datového skladu s hello obnovit datového skladu, ho mohli přejmenovat.

Pokud potřebujete toorestore odstraněné nebo pozastavený datového skladu, můžete [vytvořit lístek podpory](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore hello last available restore point.

Yes, for hello next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps hello data warehouse and its snapshots for seven days just in case you need hello data. After seven days, you won't be able toorestore tooany of hello restore points. -->

## <a name="geo-redundant-restore"></a>Geograficky redundantní obnovení
Můžete obnovit vaše datové oblasti tooany skladu podpora Azure SQL Data Warehouse stejné úrovni zvolené výkonu. Upozorňujeme, že 9000 a 18000 DWU nejsou podporovány ve všech oblastech během hello preview.

> [!NOTE]
> obnovení geograficky redundantní tooperform nesmí Nesouhlasili jste tuto funkci.
> 
> 

## <a name="restore-timeline"></a>Obnovení časové osy
Můžete obnovit bod databáze tooany obnovení k dispozici v rámci hello posledních sedmi dnů. Snímky spuštění každé čtyři hodiny tooeight a jsou k dispozici sedm dní. Pokud snímek je starší než 7 dní, vypršení jeho platnosti a jeho bod obnovení již není k dispozici.

## <a name="restore-costs"></a>Obnovení náklady
Hello úložiště zdarma pro obnovená data warehouse hello se fakturuje Azure Premium Storage rychlostí hello. 

Pokud pozastavíte obnovené datového skladu, budou se vám účtovat pro úložiště Azure Premium Storage rychlostí hello. Výhodou Hello pozastavení je, že vám není účtován hello DWU výpočetních prostředků.

Další informace o cenách služby SQL Data Warehouse najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Používá pro obnovení
Hello primárního použití pro obnovení datového skladu je toorecover data po před náhodnou ztrátou dat nebo poškození.

Můžete taky datového skladu obnovení tooretain zálohu po dobu delší než 7 dní. Po obnovení hello zálohování online máte hello datového skladu a můžete pozastavit ho neomezeně dlouhou dobu toosave výpočetní náklady. Pozastavený databáze Hello způsobuje poplatky za úložiště Storage úrovně Premium rychlostí hello. 

## <a name="related-topics"></a>Související témata
### <a name="scenarios"></a>Scénáře
* Přehled kontinuity obchodních, najdete v části [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

tooperform datového skladu obnovit, obnovte pomocí:

* Azure portálu, najdete v tématu [obnovit data warehouse pomocí hello portálu Azure](sql-data-warehouse-restore-database-portal.md)
* Rutiny prostředí PowerShell najdete v části [obnovení datového skladu pomocí rutin prostředí PowerShell](sql-data-warehouse-restore-database-powershell.md)
* Rozhraní REST API najdete v tématu [obnovit data warehouse pomocí hello rozhraní REST API](sql-data-warehouse-restore-database-rest-api.md)

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
