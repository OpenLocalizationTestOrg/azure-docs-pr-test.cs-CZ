---
title: "Omezení prostředků Azure SQL Database | Microsoft Docs"
description: "Tato stránka popisuje některé běžné limitů prostředků pro databázi SQL Azure."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 884e519f-23bb-4b73-a718-00658629646a
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/12/2017
ms.author: carlrab
ms.openlocfilehash: e75458db79e6c15f8d5155b71f04a20fa21818ff
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-sql-database-resource-limits"></a>Omezení prostředků Azure SQL Database
## <a name="overview"></a>Přehled
Spravuje dostupné k databázi pomocí dvou mechanismů různé prostředky Azure SQL Database: **zásad správného řízení prostředky** a **vynucení omezení**. Toto téma vysvětluje tyto dvě hlavní oblasti správy prostředků.

## <a name="resource-governance"></a>Zásady správného řízení prostředků
Jedním z cílů návrhu úrovně služeb Basic, Standard, Premium a Premium RS je pro databázi SQL Azure se bude chovat, jako kdyby databázi běží na vlastním počítači. jsou izolovány od ostatních databázích. Zásady správného řízení prostředků emuluje toto chování. Pokud využití prostředků agregované dosáhne maximálního výkonu procesoru, prostředky paměti, vstupně-výstupních operací protokolu a vstupně-výstupních dat přiřazené k databázi prostředků zásad správného řízení front dotazy v provádění a přidělování prostředků ve frontě dotazů, jak se uvolní.

Jako na vyhrazený počítač využívá všechny dostupné prostředky výsledky delší provádění aktuálně zpracování dotazů, což může vést ke vypršení časových limitů příkaz na straně klienta. Aplikace s logika opakovaných pokusů agresivní a aplikace, které spouštění dotazů na databázi s vysoká frekvence se zprávy chyby při pokusu o provedení nové dotazy, pokud bylo dosaženo maximálního počtu souběžných požadavků.

### <a name="recommendations"></a>Doporučení:
Sledování využití prostředků a průměrná odezvy dotazů blíží maximální využití databáze. Při zjištění vyšší latence dotazu obecně máte tři možnosti:

1. Snižte počet příchozích požadavků do databáze, aby se zabránilo vypršení časového limitu a relačních operátorů požadavky.
2. Přiřadíte vyšší úroveň výkonu do databáze.
3. Optimalizujte dotazy ke snížení využití prostředků každý dotaz. Další informace najdete v části dotazu optimalizace/Hinting v článku Azure SQL Database – Průvodce výkonem.

## <a name="enforcement-of-limits"></a>Vynucení mezní hodnoty
Prostředky než procesoru, paměti, vstupně-výstupních operací protokolu a vstupně-výstupních dat vynucuje odepření nové žádosti o po dosažení omezení. Když databáze dosáhne nakonfigurovaný maximální limit pro velikost, vložení a aktualizace, které zvýšit velikost dat selžou, vybere a odstranění pokračovat v práci. Klienti obdrží [chybová zpráva](sql-database-develop-error-messages.md) v závislosti na tom, že bylo dosaženo limitu.

Počet připojení k databázi SQL a počet souběžných požadavků, které lze zpracovat jsou například s omezeným přístupem. Databáze SQL umožňuje počet připojení k databázi a být větší než počet souběžných požadavků pro podporu sdružování připojení. Když počet připojení, které jsou k dispozici lze snadno řídit aplikací, počet paralelní požadavků, které je často časy těžší k zjištění přibližné hodnoty a k řízení. Zejména při zatížení ve špičce při aplikace buď odešle příliš mnoho požadavků nebo databáze dosáhne jeho prostředků omezuje a spustí kumulování pracovních vláken kvůli delší spuštěné dotazy, může být zjištěny chyby.

## <a name="service-tiers-and-performance-levels"></a>Úrovně služby a úrovně výkonu
Existují úrovně služeb a úrovně výkonu pro izolované databáze i elastické fondy.

### <a name="single-databases"></a>Izolované databáze
Pro jednu databázi limity databáze jsou definovány databázi služby vrstvy a úroveň výkonu. Následující tabulka popisuje vlastnosti databází Basic, Premium, Standard a Premium RS v různých úrovní výkonu.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!IMPORTANT]
> Zákazníci, kteří používají P11 a P15 úrovně výkonu můžete použít až 4 TB zahrnuté úložiště bez dalších poplatků. Tato možnost 4 TB je aktuálně k dispozici v následujících oblastech: nám East2, západní USA, nám verze pro státní správu Virginia, západní Evropa, Německo centrální, Jižní východní Asie, Japonsko – východ, Austrálie – východ, Kanada centrální a Východní Kanada.
>

### <a name="elastic-pools"></a>Elastické fondy
[Elastické fondy](sql-database-elastic-pool.md) sdílení prostředků mezi databází ve fondu. Následující tabulka popisuje vlastnosti Basic, Standard, Premium a Premium RS elastické fondy.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Rozšířené definice všechny prostředky uvedené v předchozích tabulkách v popisech v [služby možnosti a omezení úrovní](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Přehled o úrovních služeb najdete v tématu [úrovních služby databáze SQL Azure a úrovně výkonu](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Další omezení databáze SQL
| Oblast | Omezení | Popis |
| --- | --- | --- |
| Databáze pomocí automatizovaný export na předplatné |10 |Automatizovaném exportu vám umožní vytvořit vlastní plán pro zálohování vaší databáze SQL. Verze preview tato funkce se ukončí na 1. března 2017.  |
| Databáze na serveru |Až 5000 |Až 5000 databáze jsou povoleny na server. |
| Počet jednotek Dtu na server |45000 |Dtu 45000 jsou povoleny na server pro zřizování samostatné databáze i elastické fondy. Celkový počet samostatné databáze a fondy povoleno na serveru je omezena pouze číslo serveru Dtu.  

> [!IMPORTANT]
> Azure SQL Database automatizované Export je teď ve verzi preview a vyřadí na 1. března 2017. Od 1. prosinci 2016 už nebudete moct konfigurovat automatizovaném exportu na žádné databáze SQL. Všechny stávající automatizované úlohy budou nadále fungovat až do 1. března 2017 exportu. Po 1. prosinci 2016, můžete použít [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md) nebo [Azure Automation](../automation/automation-intro.md) k archivaci SQL databáze pomocí prostředí PowerShell pravidelně pravidelně podle plánu, podle vašeho výběru. Pro ukázkový skript si můžete stáhnout [ukázkový skript z Githubu](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export).
>


## <a name="resources"></a>Zdroje
[Předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md)

[Úrovně služeb databáze Azure SQL a úrovně výkonu](sql-database-service-tiers.md)

[Chybové zprávy klientských programů služby Azure SQL Database](sql-database-develop-error-messages.md)
