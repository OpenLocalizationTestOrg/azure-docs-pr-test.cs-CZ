---
title: "aaaAzure limitů prostředků databáze SQL | Microsoft Docs"
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
ms.openlocfilehash: 3e7be4fdc74e802d37274690531caaf138bcedb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-resource-limits"></a>Omezení prostředků Azure SQL Database
## <a name="overview"></a>Přehled
Azure SQL Database spravuje hello prostředky k dispozici tooa databáze pomocí dvou různých mechanismů: **zásad správného řízení prostředky** a **vynucení omezení**. Toto téma vysvětluje tyto dvě hlavní oblasti správy prostředků.

## <a name="resource-governance"></a>Zásady správného řízení prostředků
Jedním z cílů návrhu hello úrovně služeb Basic, Standard, Premium a Premium RS hello je pro Azure SQL Database toobehave, jako kdyby hello databáze běží na vlastním počítači. jsou izolovány od ostatních databázích. Zásady správného řízení prostředků emuluje toto chování. Pokud hello agregovat využití prostředků dosáhne hello maximální dostupné procesoru, paměti, vstupně-výstupních operací protokolu a vstupně-výstupních dat databáze přiřazené toohello prostředků, prostředků zásad správného řízení front dotazy v provádění a přiřaďte prostředků, které toohello ve frontě dotazů jako jejich uvolněte.

Jako na vyhrazený počítač využívá všechny dostupné prostředky výsledky delší provádění aktuálně zpracování dotazů, což může vést ke vypršení časových limitů příkaz hello klienta. Aplikace s logika opakovaných pokusů agresivní a aplikace, které spouštění dotazů na databázi hello s vysoká frekvence se zprávy chyby při pokusu o nové dotazy tooexecute, pokud bylo dosaženo limitu hello souběžných požadavků.

### <a name="recommendations"></a>Doporučení:
Sledování využití prostředků hello a hello průměrná odezvy dotazů blíží maximální využití hello databáze. Při zjištění vyšší latence dotazu obecně máte tři možnosti:

1. Snížení hello počtu příchozích požadavků toohello databáze tooprevent časový limit a hello relačních operátorů až požadavků.
2. Přiřadíte vyšší úrovně toohello databáze výkonu.
3. Optimalizujte využití prostředků hello dotazy tooreduce každý dotazu. Další informace najdete v tématu hello část dotazu optimalizace/Hinting v článku Azure SQL Database – Průvodce výkonem hello.

## <a name="enforcement-of-limits"></a>Vynucení mezní hodnoty
Prostředky než procesoru, paměti, vstupně-výstupních operací protokolu a vstupně-výstupních dat vynucuje odepření nové žádosti o po dosažení omezení. Když databáze dosáhne hello nakonfigurovaný limit maximální velikosti, vložení a aktualizace, které se zvyšují velikost dat selžou, zatímco vybere a odstranění dále toowork. Klienti obdrží [chybová zpráva](sql-database-develop-error-messages.md) v závislosti na limitu hello, který byl dosažen.

Například hello počet připojení tooa SQL database a hello počet souběžných požadavků, které lze zpracovat omezeny. Databáze SQL umožňuje hello počet připojení toohello databáze toobe větší než počet hello sdružování připojení toosupport souběžných požadavků. Při hello počet připojení, které jsou k dispozici může být snadno řízena hello aplikace, počet hello paralelní požadavků, které je často těžší tooestimate časy a toocontrol. Zejména při zatížení ve špičce, když aplikace hello buď odešle příliš mnoho požadavků nebo databáze hello dosaženo omezení prostředků a spustí kumulování pracovních vláken kvůli toolonger spuštěné dotazy, můžete k chybám.

## <a name="service-tiers-and-performance-levels"></a>Úrovně služby a úrovně výkonu
Existují úrovně služeb a úrovně výkonu pro izolované databáze i elastické fondy.

### <a name="single-databases"></a>Izolované databáze
Pro jednu databázi jsou definovány hello omezení databáze hello databáze služby vrstvy a úroveň výkonu. Hello následující tabulka popisuje vlastnosti hello databází Basic, Premium, Standard a Premium RS v různých úrovní výkonu.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!IMPORTANT]
> Zákazníci, kteří používají P11 a P15 úrovně výkonu můžete použít až too4 TB zahrnuté úložiště bez dalších poplatků. Tato možnost 4 TB je aktuálně k dispozici v následujících oblastech hello: nám East2, západní USA, nám verze pro státní správu Virginia, západní Evropa, Německo centrální, Jižní východní Asie, Japonsko – východ, Austrálie – východ, Kanada centrální a Východní Kanada.
>

### <a name="elastic-pools"></a>Elastické fondy
[Elastické fondy](sql-database-elastic-pool.md) sdílení prostředků mezi databází ve fondu hello. Hello následující tabulka popisuje hello charakteristiky Basic, Standard, Premium a Premium RS elastické fondy.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Rozšířené definice všechny prostředky uvedené v předchozích tabulkách hello popisech hello v [služby možnosti a omezení úrovní](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Přehled o úrovních služeb najdete v tématu [úrovních služby databáze SQL Azure a úrovně výkonu](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Další omezení databáze SQL
| Oblast | Omezení | Popis |
| --- | --- | --- |
| Databáze pomocí automatizovaný export na předplatné |10 |Automatizovaném exportu umožňuje toocreate vlastní plán pro zálohování vaší databáze SQL. 1. března 2017 ukončen Hello preview této funkce.  |
| Databáze na serveru |Až too5000 |Až too5000 jsou povoleny databází na serveru. |
| Počet jednotek Dtu na server |45000 |Dtu 45000 jsou povoleny na server pro zřizování samostatné databáze i elastické fondy. Celkový počet Hello samostatné databáze a fondy povoleno na serveru je omezena pouze hello číslo serveru Dtu.  

> [!IMPORTANT]
> Azure SQL Database automatizované Export je teď ve verzi preview a vyřadí na 1. března 2017. Od 1. prosinci 2016 budou nadále již nebudou moct tooconfigure automatizované export na žádné databáze SQL. Všechny vaše stávající úlohy automatizovaném exportu bude toowork až 1. března 2017. Po 1. prosinci 2016, můžete použít [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md) nebo [Azure Automation](../automation/automation-intro.md) databází SQL tooarchive pravidelně pomocí prostředí PowerShell pravidelně podle plánu tooa podle svého výběru. Pro ukázkový skript si můžete stáhnout hello [ukázkový skript z Githubu](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export).
>


## <a name="resources"></a>Zdroje
[Předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md)

[Úrovně služeb databáze Azure SQL a úrovně výkonu](sql-database-service-tiers.md)

[Chybové zprávy klientských programů služby Azure SQL Database](sql-database-develop-error-messages.md)
