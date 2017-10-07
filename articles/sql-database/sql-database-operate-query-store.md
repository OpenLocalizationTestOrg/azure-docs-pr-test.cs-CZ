---
title: "aaaOperating úložiště dotazů v databázi SQL Azure"
description: "Zjistěte, jak toooperate hello úložiště dotazů v databázi SQL Azure"
keywords: 
services: sql-database
documentationcenter: 
author: bonova
manager: jhubbard
editor: 
ms.assetid: 0cccf6bd-1327-44f7-a6f9-8eff0c210463
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: sqldb-performance
ms.workload: data-management
ms.date: 11/08/2016
ms.author: bonova
ms.openlocfilehash: ab741e49685bf6df3bceab219aaf57ea51cdb25d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="operating-hello-query-store-in-azure-sql-database"></a>Provozní hello úložiště dotazů v databázi SQL Azure
Úložiště dotazů v Azure je plně spravovaná databáze funkce, která nepřetržitě shromažďuje a uvede podrobné historické informace o všech dotazů. O úložišti dotazů si můžete představit jako podobné tooan letadle dat záznam výrazně zjednodušuje výkon dotazů řešení potíží pro cloudové a místní zákazníků. Tento článek vysvětluje specifických aspektů operačního úložiště dotazů v Azure. Pomocí této předem shromážděných dotaz na data, můžete rychle diagnostikovat a vyřešit problémy s výkonem a proto věnovat více času, které jsou zaměřené na své firmy. 

Úložiště dotazů byl [globálně dostupnou](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) ve službě Azure SQL Database od listopadu 2015. Úložiště dotazů je hello foundation pro analýzu výkonu a ladění funkcí, jako například [Poradce pro funkci SQL Database a řídicí panel výkonu](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Úložiště dotazů je momentálně hello publikování v tomto článku spuštěna ve více než 200 000 uživatelských databází v Azure, shromažďování související s informací o několik měsíců, bez přerušení.

> [!IMPORTANT]
> Společnost Microsoft se v procesu hello aktivace úložiště dotazů pro všechny databáze Azure SQL (existujícím a novým). 
> 
> 

## <a name="optimal-query-store-configuration"></a>Konfigurace optimální úložiště dotazů
Tato část popisuje výchozí hodnoty optimální konfigurace, které jsou navržené tooensure spolehlivé operace úložiště dotazů hello a závislé součásti, například [Poradce pro funkci SQL Database a řídicí panel výkonu](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Výchozí konfigurace je optimalizovaná pro souvislá datová kolekci, ve kterých je minimální čas strávený v OFF/READ_ONLY stavy.

| Konfigurace | Popis | Výchozí | Komentář |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Určuje hello limit pro hello datového prostoru, který úložiště dotazů můžete provést uvnitř databáze z zákazníka |100 |Vynutí nové databáze |
| HODNOTA INTERVAL_LENGTH_MINUTES |Definuje velikost časový interval, během kterého se agregují a trvalé statistiku shromážděných modulu runtime pro plány dotazů. Každý dotaz služby active plán má maximálně jeden řádek pro určitou dobu definované s touto konfigurací |60 |Vynutí nové databáze |
| HODNOTA STALE_QUERY_THRESHOLD_DAYS |Zásady založené na čase čištění, která řídí dobu uchování hello statistiku trvalou modulu runtime a neaktivní dotazů |30 |Vynucování nové databáze a databáze s předchozí výchozí (367) |
| SIZE_BASED_CLEANUP_MODE |Určuje, zda automatické čištění probíhá při velikost dat úložiště dotazů blíží limitu hello |AUTOMATICKY |Vynutit pro všechny databáze |
| HODNOTA PRO QUERY_CAPTURE_MODE |Určuje, zda jsou sledovány všechny dotazy nebo pouze podmnožinu dotazy |AUTOMATICKY |Vynutit pro všechny databáze |
| FLUSH_INTERVAL_SECONDS |Určuje maximální dobu, během které statistiku modulu runtime zaznamenané před vyčištění toodisk udržovaly v paměti |900 |Vynutí nové databáze |
|  | | | |

> [!IMPORTANT]
> Tato výchozí nastavení budou automaticky použita v konečné fázi hello aktivace úložiště dotazů v všechny databáze Azure SQL (viz předchozí důležitá poznámka). Po této light až Azure SQL Database nebude mění konfigurace hodnotami nastavenými v zákazníků, pokud budou mít negativní vliv na primární úlohy nebo spolehlivé operace hello úložiště dotazů.
> 
> 

Pokud chcete toostay pomocí vlastních nastavení, použijte [ALTER DATABASE s možností úložiště dotazů](https://msdn.microsoft.com/library/bb522682.aspx) toorevert konfigurace toohello předchozího stavu. Podívejte se na [osvědčené postupy s hello úložiště dotazů](https://msdn.microsoft.com/library/mt604821.aspx) v pořadí toolearn jak nejvyšší zvolili optimální konfigurační parametry.

## <a name="next-steps"></a>Další kroky
[Informace o výkonu databáze SQL](sql-database-performance.md)

## <a name="additional-resources"></a>Další zdroje
Pro další informace o rezervaci hello následující články:

* [Záznam dat pro vaši databázi](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 
* [Monitorování výkonu s použitím úložiště dotazů hello](https://msdn.microsoft.com/library/dn817826.aspx)
* [Scénáře použití úložiště dotazů](https://msdn.microsoft.com/library/mt614796.aspx)
* [Monitorování výkonu s použitím úložiště dotazů hello](https://msdn.microsoft.com/library/dn817826.aspx) 

