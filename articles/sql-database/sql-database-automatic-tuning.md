---
title: "aaaSQL databáze - automatické ladění | Microsoft Docs"
description: "Databáze SQL analyzuje dotazu SQL a automaticky přizpůsobí toouser zatížení."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: jovanpop
ms.openlocfilehash: 897a8deaabedb6727dc49958c64d0433c5f2e4f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-tuning"></a>Automatické ladění

Azure SQL Database je plně spravovaná data služba, která monitoruje hello dotazy, které jsou spouštěny na hello databázi a automaticky může zlepšit výkon hello úlohy. Azure SQL Database má vestavěné inteligentní mechanismus, který můžete automaticky ladit a dynamicky přizpůsobení hello databáze tooyour zatížení vylepšit výkon vašich dotazů. Automatické ladění ve službě Azure SQL Database, může být jedním z nejdůležitějších funkcích hello, které se dají povolit na Azure SQL Database toooptimize výkon vašich dotazů.

Najdete v tomto článku kroky hello příliš[povolit automatické ladění](sql-database-automatic-tuning-enable.md) pomocí hello portálu Azure.

## <a name="why-automatic-tuning"></a>Proč automatické ladění?

Jeden z hello hlavní úlohy v classic databáze správy je monitorování hello zatížení identifikace důležitých SQL dotazuje indexy, které má být přidána tooimprove výkonu a je používána zřídka indexy. Databáze SQL Azure poskytuje podrobný přehled o hello dotazy a indexy, je nutné, aby toomonitor. Neustálé monitorování databáze je však náročný a zdlouhavý úkol, zejména při práci s mnoha databázemi. Správa velký počet databáze může být možné toodo efektivně i se všemi dostupnou nástroje a sestavy, které obsahují Azure SQL Database a portálu Azure. Místo sledování a ladění databázi ručně, můžete zvážit delegování některé hello sledování a ladění akce tooAzure SQL Database pomocí funkce Automatické ladění. 


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="how-does-automatic-tuning-work"></a>Jak funguje automatické ladění?

Azure SQL Database má průběžné výkonu monitorování a analýzu proces, který neustále dozví o hello charakteristik vašich úloh a identifikovat potenciální problémy a vylepšení.

![Automatický proces ladění](./media/sql-database-automatic-tuning/tuning-process.png)

Tento proces umožňuje Azure SQL Database toodynamically přizpůsobit tooyour zatížení podle hledání jaké indexy a plány může zvýšit výkon vašich úloh a co indexuje ovlivnit úlohy. Podle těchto zjištění, automatické ladění platí vyladění akce, které zlepšují výkon vašich úloh. Kromě toho Azure SQL Database neustále monitoruje výkonu po všechny změny provedené automatické ladění tooensure vylepšuje výkon vašich úloh. Všechny akce, která nebyla zlepšit výkon je automaticky vrátila. Tento proces ověření je klíčovou funkcí, které zajišťuje, že všechny změny provedené automatické ladění poklesu hello výkon vašich úloh.

Existují dva automatické ladění aspekty, které jsou k dispozici ve službě Azure SQL Database:

 -  **Správa automatického indexu** identifikující indexy, které mají být přidány do databáze a indexy, které má být odebrána.
 -  **Automatické plán oprava** (již brzy, již k dispozici v SQL serveru 2017) identifikující problematické plány a opravy SQL plánování problémy s výkonem.

## <a name="automatic-index-management"></a>Správa automatického indexu

V databázi SQL Azure je snadné Správa indexu, protože Azure SQL Database dozví o velikosti pracovní zátěže a zajistí, že je vaše data vždy optimálně indexovaný. Správné index návrhu je zásadní pro optimální výkon vašich úloh a správu automatické indexu můžete optimalizovat vaše indexy. Správa automatického indexu může buď opravte problémy s výkonem v nesprávně indexované databáze, nebo zachovat a zlepšit indexy na existující schématu databáze hello. 

### <a name="why-do-you-need-index-management"></a>Proč potřebujete Správa indexu?

Indexy urychlení některé z vašich dotazů, které číst data z tabulek hello; ale že může zpomalit hello dotazy, které umožňuje aktualizovat data. Je třeba toocarefully analyzovat při toocreate indexu a které sloupce je třeba tooinclude v hello index. Některé indexy nemusí být potřeba po určité době. Proto byste potřebovali tooperiodically identifikovat a vyřadit hello indexy, které nelze zobrazit žádné výhody. Pokud budete ignorovat hello nepoužívané indexy, by se ke snížení výkonu hello dotazů, které umožňuje aktualizovat data bez nějaké výhody na hello dotazy, které číst data. Nepoužívané indexy také vliv na celkový výkon systému hello, protože další aktualizace vyžadují nepotřebné protokolování.

Hledání hello optimální sadu indexy, které zlepšují výkon hello dotazy, které čtení dat z tabulky a mít minimální dopad na aktualizace může vyžadovat nepřetržitý a komplexní analýzy.

Databáze SQL Azure používá vestavěné inteligentní a rozšířených pravidel, které analyzovat vaše dotazy, identifikovat indexy, které by být optimální pro aktuální zatížení a může být odebrán hello indexy. Azure SQL Database zajišťuje, že máte nezbytné nejzákladnější indexy, které optimalizovat hello dotazy, které čtení dat, s hello minimalizovat dopad na hello jiné dotazy.

### <a name="how-tooidentify-indexes-that-need-toobe-changed-in-your-database"></a>Jak tooidentify indexy tohoto toobe nutné změnit ve vaší databázi?

Azure SQL Database usnadňuje proces správy indexu. Místo hello zdlouhavé proces ruční úlohy analýzy a monitorování index analyzuje vaše úlohy Azure SQL Database, identifikuje hello dotazy, které by bylo možné provést rychlejší s nového indexu a identifikuje nepoužívá, nebo duplicitní indexy. Další informace o identifikaci indexy, které by mělo být změněno na [najít doporučení indexu na portálu Azure](sql-database-advisor-portal.md).

### <a name="automatic-index-management-considerations"></a>Aspekty správy automatické indexu

Pokud zjistíte, že vestavěné pravidla hello zlepšit hello výkon vaší databáze, vám může nechat automaticky spravovat vaše indexů Azure SQL database.

Akce vyžaduje toocreate nezbytné indexy v databázích SQL Azure může spotřebovávat prostředky a dočasně ovlivnit výkon pracovního vytížení. toominimize hello dopad vytvoření indexu na výkon pracovního vytížení, Azure SQL Database vyhledá hello odpovídající časový interval pro všechny operace správy indexu. Ladění akce je odložen. Pokud je hello databázi prostředků tooexecute vaše úlohy a začínat splněním hello databáze nemá dostatek nepoužívané prostředky, které lze použít pro úlohu údržby hello. Jeden důležité funkce v Správa automatické indexu je ověření hello akcí. Databáze SQL Azure vytvoří nebo poklesne index, analyzuje proces monitorování výkonu vaše zatížení tooverify, že akce hello zvýšení výkonu hello. Pokud nebylo ji převést výrazné zlepšení – hello akce okamžitě vrátit zpět. Tímto způsobem, Azure SQL Database zajišťuje, že automatické akce nemá negativní dopad na výkon vašich úloh. Indexy vytvořené automatické ladění jsou transparentní pro operace údržby hello na základní schématu hello. Změny schématu, jako je například vyřazení nebo přejmenování sloupce nejsou blokována bránou hello přítomnost automatické vytváření indexů. Indexy, které jsou automaticky vytvořené pomocí Azure SQL Database při okamžitě vyřadit související tabulky nebo sloupce je vyřazeno.

## <a name="automatic-plan-choice-correction"></a>Oprava volbou automatické plán

Oprava automatické plán je nová automatické ladění funkce přidán do systému SQL Server 2017 CTP2.0, který identifikuje SQL plány, které nesprávnému chování. Tato možnost Automatické ladění brzy v databázi SQL Azure. Další informace v [automatické ladění v SQL serveru 2017](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="next-steps"></a>Další kroky

- tooenable automatické ladění v Azure SQL Database a umožňují automatické ladění funkce plně spravovat vaše úlohy najdete v tématu [povolit automatické ladění](sql-database-automatic-tuning-enable.md).
- toouse ruční ladění, můžete zkontrolovat [ladění doporučení na portálu Azure](sql-database-advisor-portal.md) a použít ručně hello ty, které zlepšit výkon vašich dotazů.
