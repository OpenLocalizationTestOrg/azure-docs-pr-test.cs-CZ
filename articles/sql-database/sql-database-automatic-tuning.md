---
title: "Azure SQL Database – automatické ladění | Microsoft Docs"
description: "Azure SQL Database analyzuje dotazu SQL a automaticky přizpůsobí zatížení uživatele."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: drasumic
editor: danimir
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 11/08/2017
ms.author: jovanpop
ms.openlocfilehash: 50d1548637ecc6862f680f73e333e8d7f8d20e39
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatické ladění ve službě Azure SQL Database

Azure SQL Database automatické ladění poskytuje špičkový výkon a stabilní úlohy prostřednictvím ladění využitím umělé Intelligence průběžné výkonu.

Automatické ladění je plně spravovaná služba, která používá vestavěné inteligentní neustále monitorovat dotazy spouštěné v databázi a automaticky vylepšuje výkonu. Toho je dosaženo pomocí dynamicky přizpůsobení databáze změna úloh a použití vyladění doporučení. Automatické ladění z všechny databáze na Azure prostřednictvím umělé Intelligence vodorovně zjišťuje a dynamicky vylepšuje vyladění akcí. Čím delší běží databáze SQL Azure s automatické ladění na, tím lépe provede.

Azure SQL Database automatické ladění, může být jedním z nejdůležitějších funkcích, které se dají povolit zajistit stabilní a provádění úloh ve špičce.

## <a name="what-can-automatic-tuning-do-for-you"></a>Co můžete automatické ladění to pro vás?

- Optimalizace výkonu automatizované databází SQL Azure
- Automatické ověření zvýšení výkonu
- Automatické vrácení a vlastní oprava
- Ladění historie protokolu
- Ladění skriptů T-SQL akce pro ruční nasazení
- Monitorování výkonu proaktivní pracovního vytížení
- Horizontální navýšení kapacity možnost na stovky tisíc databáze
- Kladný dopad na prostředky DevOps a celkové náklady na vlastnictví

## <a name="safe-reliable-and-proven"></a>Bezpečné, spolehlivé a osvědčené

Ladění operace u databází SQL Azure jsou plně bezpečné pro výkon vašich nejvíce intenzivního zatížení. Systém má byly navrženy dát pozor který nenaruší úlohy uživatele. Automatické ladění doporučení se používají pouze v časech nízkou využití. Systém můžete také dočasně zakázat automatické ladění operations k ochraně zatížení výkonu. V takovém případě se zobrazí zpráva "Zakázáno systémem" na portálu Azure. Automatické ladění, pokud o úlohy s nejvyšší prioritou prostředků.

Automatické ladění mechanismy jsou vyspělá a mít byla dohoda na stovky tisíc databáze spuštěné v Azure. Automatické ladění operations použít automaticky ověření zajistit, že je kladné zlepšování úloh na výkon. Který poklesl výkonu doporučení jsou zjištěna dynamicky a rychle vrátili zpět. Prostřednictvím protokolu ladění historie je zrušte trasování ladění vylepšení provedené v každé databáze SQL Azure. 

![Jak funguje automatické ladění](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Azure SQL Database automatické ladění je svou logikou základní sdílení s automatické ladění stroj SQL Server. Další technické informace o mechanismus vestavěné inteligentní najdete v tématu [automatické ladění serveru SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="use-automatic-tuning"></a>Použít automatické ladění

Automatické ladění musí se zapnout ručně na vaše předplatné. Pokud chcete povolit automatické ladění pomocí portálu Azure, najdete v části [povolit automatické ladění](sql-database-automatic-tuning-enable.md).

Automatické ladění můžou fungovat samostatně pomocí automatické použití vyladění doporučení, včetně automatické ověření zvýšení výkonu. 

Pro další ovládací prvek může být vypnuto automatické použití ladění doporučení a ladění doporučení se dá ručně použít prostřednictvím portálu Azure. Je také možné použít řešení a zobrazit pouze automatické ladění doporučení použít ručně pomocí skriptů a nástrojů dle vašeho výběru. 

Přehled funkce Automatické ladění funguje a příklady typických scénářů použití naleznete v tématu vložené video:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

## <a name="automatic-tuning-options"></a>Automatické možnosti ladění

Jsou automatické ladění možnosti, které jsou k dispozici ve službě Azure SQL Database:
 1. **Vytvoření indexu** identifikující indexy, které může zvýšit výkon vašich úloh, vytváří indexy a ověřuje, že se zlepšit výkon dotazů.
 2. **DROP INDEX** identifikující redundantní a duplicitní indexy a indexy, které nebyly použity v dlouhé časové období.
 3. **POSLEDNÍ KVALITNÍ plán VYNUTIT** , identifikuje dotazy SQL, které používají plán spuštění, které jsou nižší než předchozí kvalitní plán a místo regressed plánu používá poslední známé funkční plánu.

Azure SQL Database identifikuje **CREATE INDEX**, **DROP INDEX**, a **FORCE poslední dobré plánování** doporučení, které můžete optimalizovat vaši databázi a zobrazí je v portálu Azure. Další informace o identifikaci indexy, které by mělo být změněno na [najít doporučení indexu na portálu Azure](sql-database-advisor-portal.md). Můžete buď použít ručně doporučení pomocí portálu nebo můžete nechat Azure SQL Database automaticky používat doporučení, sledování úloh po provedení změny a ověřte, že doporučení zvýšený výkon vašich úloh.

Automatické ladění možnosti může být nezávisle zapnout nebo vypnout na databázi, nebo můžou být nakonfigurované na logickém serveru a použít na každou databázi, která dědí nastavení ze serveru. Konfigurace automatické ladění možnosti na serveru a dědí nastavení u databází na serveru, doporučuje se metoda pro konfiguraci automatické ladění, protože se zjednodušuje Správa automatické možnosti ladění na velký počet databází.

## <a name="next-steps"></a>Další kroky

- Pokud chcete povolit automatické ladění v databázi SQL Azure ke správě velikosti pracovní zátěže, najdete v části [povolit automatické ladění](sql-database-automatic-tuning-enable.md).
- Chcete-li ručně zkontrolovat a použít automatické ladění doporučení, přečtěte si téma [najít a použít výkonu doporučení](sql-database-advisor-portal.md).
- Další informace o předdefinovaných intelligence používají při automatické ladění, najdete v části [umělé Intelligence vyladí databází SQL Azure](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Další informace o tom, jak automatické ladění funguje v Azure SQL Database a SQL server 2017 najdete v tématu [automatické ladění serveru SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).
