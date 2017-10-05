---
title: "Databáze SQL - automatické ladění | Microsoft Docs"
description: "Databáze SQL analyzuje dotazu SQL a automaticky přizpůsobí zatížení uživatele."
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
ms.openlocfilehash: f5552793db2d89542782b7d9e8f996314f62e429
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automatic-tuning"></a>Automatické ladění

Azure SQL Database je plně spravovaná data služba, která monitoruje dotazy, které jsou spouštěny v databázi a automaticky může zlepšit výkon pracovního vytížení. Azure SQL Database má vestavěné inteligentní mechanismus, který můžete automaticky ladit a dynamicky přizpůsobení databázi do úlohy vylepšit výkon vašich dotazů. Automatické ladění ve službě Azure SQL Database, může být jedním z nejdůležitějších funkcích, můžete povolit na Azure SQL Database za účelem optimalizace výkonu dotazů.

Najdete v tomto článku postup [povolit automatické ladění](sql-database-automatic-tuning-enable.md) pomocí portálu Azure.

## <a name="why-automatic-tuning"></a>Proč automatické ladění?

Jeden z hlavních úkolů v classic databáze správy je monitorování zátěže identifikace důležitých dotazy SQL, indexy, které mají být přidány ke zlepšení výkonu a zřídka používané indexů. Databáze SQL Azure poskytuje podrobný přehled o dotazy a indexy, které je nutné sledovat. Neustálé monitorování databáze je však náročný a zdlouhavý úkol, zejména při práci s mnoha databázemi. Správa velký počet databáze může být možné provádět efektivně i se všemi dostupnou nástroje a sestavy, které obsahují Azure SQL Database a portálu Azure. Místo sledování a ladění databázi ručně, můžete zvážit některé akce sledování a ladění do Azure SQL Database pomocí funkce Automatické ladění delegování. 


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="how-does-automatic-tuning-work"></a>Jak funguje automatické ladění?

Azure SQL Database má průběžné výkonu monitorování a analýzu proces, který neustále dozví o vlastnosti úlohy a identifikovat potenciální problémy a vylepšení.

![Automatický proces ladění](./media/sql-database-automatic-tuning/tuning-process.png)

Tento proces umožní Azure SQL Database se vaše úlohy dynamicky přizpůsobit tak, že najdete jaké indexy a plány může zvýšit výkon vašich úloh a jaké indexy ovlivnit úlohy. Podle těchto zjištění, automatické ladění platí vyladění akce, které zlepšují výkon vašich úloh. Kromě toho Azure SQL Database neustále monitoruje výkonu po všechny změny provedené automatické ladění zajistit, že vylepšuje výkon vašich úloh. Všechny akce, která nebyla zlepšit výkon je automaticky vrátila. Tento proces ověření je klíčovou funkcí, které zajišťuje, že všechny změny provedené automatické ladění ne snížit výkon vašich úloh.

Existují dva automatické ladění aspekty, které jsou k dispozici ve službě Azure SQL Database:

 -  **Správa automatického indexu** identifikující indexy, které mají být přidány do databáze a indexy, které má být odebrána.
 -  **Automatické plán oprava** (již brzy, již k dispozici v SQL serveru 2017) identifikující problematické plány a opravy SQL plánování problémy s výkonem.

## <a name="automatic-index-management"></a>Správa automatického indexu

V databázi SQL Azure je snadné Správa indexu, protože Azure SQL Database dozví o velikosti pracovní zátěže a zajistí, že je vaše data vždy optimálně indexovaný. Správné index návrhu je zásadní pro optimální výkon vašich úloh a správu automatické indexu můžete optimalizovat vaše indexy. Správa automatického indexu může buď opravte problémy s výkonem v nesprávně indexované databáze, nebo zachovat a zlepšit indexy na existující schématu databáze. 

### <a name="why-do-you-need-index-management"></a>Proč potřebujete Správa indexu?

Indexy urychlení některé své dotazy, které čtení dat z tabulky; Nicméně se může zpomalit dotazy, které umožňuje aktualizovat data. Budete muset pečlivě analýza kdy vytvořit index a které sloupce je nutné zahrnout v indexu. Některé indexy nemusí být potřeba po určité době. Musíte tedy pravidelně identifikovat a vyřadit indexy, které nelze zobrazit žádné výhody. Pokud budete ignorovat nepoužívané indexy, by být výkon dotazů, které umožňuje aktualizovat data snížena bez nějaké výhody na dotazy, které číst data. Nepoužívané indexy také ovlivnit celkový výkon systému, protože další aktualizace vyžadují nepotřebné protokolování.

Hledání optimální sadu indexy, které zlepšit výkon dotazů, které čtení dat z tabulky a mít minimální dopad na aktualizace může vyžadovat nepřetržitý a komplexní analýzy.

Databáze SQL Azure používá vestavěné inteligentní a rozšířených pravidel, které analyzovat vaše dotazy, identifikovat indexy, které by být optimální pro aktuální zatížení a může být odebrán indexy. Databáze SQL Azure zajistí, že minimální potřebné sadu indexy, které optimalizovat dotazy, které čtení dat pomocí minimalizovaném okně dopad na jiné dotazy.

### <a name="how-to-identify-indexes-that-need-to-be-changed-in-your-database"></a>Jak identifikovat indexy, které se musí změnit ve vaší databázi?

Azure SQL Database usnadňuje proces správy indexu. Místo proces zdlouhavé ruční úlohy analýzy a monitorování index analyzuje vaše úlohy Azure SQL Database, identifikuje dotazy, které by bylo možné provést rychlejší s nového indexu a identifikuje nepoužívá, nebo duplicitní indexy. Další informace o identifikaci indexy, které by mělo být změněno na [najít doporučení indexu na portálu Azure](sql-database-advisor-portal.md).

### <a name="automatic-index-management-considerations"></a>Aspekty správy automatické indexu

Pokud zjistíte, že vestavěné pravidla zlepšit výkon vaší databáze, vám může nechat automaticky spravovat vaše indexů Azure SQL database.

Akce, které jsou potřebné pro vytvoření nezbytné indexy v databázích SQL Azure může spotřebovávat prostředky a dočasně ovlivnit výkon pracovního vytížení. Minimalizovat dopad vytvoření indexu na výkon úloh, Azure SQL Database najde odpovídající časový interval pro všechny operace správy indexu. Ladění akce je odložen. Pokud databáze potřebuje prostředky k provedení úlohy a začínat splněním databáze nemá dostatek nepoužívané prostředky, které lze použít pro úlohu údržby. Jeden důležité funkce v Správa automatické indexu je ověření akce. Databáze SQL Azure vytvoří nebo poklesne index, proces monitorování analyzuje výkon vašich úloh ověření, že akce vylepšení výkonu. Pokud nebylo ji převést výrazné zlepšení – akce okamžitě vrátit zpět. Tímto způsobem, Azure SQL Database zajišťuje, že automatické akce nemá negativní dopad na výkon vašich úloh. Indexy vytvořené automatické ladění jsou transparentní pro operace údržby na základní schéma. Změny schématu, jako je například vyřazení nebo přejmenování sloupce nejsou blokována bránou přítomnost automatické vytváření indexů. Indexy, které jsou automaticky vytvořené pomocí Azure SQL Database při okamžitě vyřadit související tabulky nebo sloupce je vyřazeno.

## <a name="automatic-plan-choice-correction"></a>Oprava volbou automatické plán

Oprava automatické plán je nová automatické ladění funkce přidán do systému SQL Server 2017 CTP2.0, který identifikuje SQL plány, které nesprávnému chování. Tato možnost Automatické ladění brzy v databázi SQL Azure. Další informace v [automatické ladění v SQL serveru 2017](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="next-steps"></a>Další kroky

- Povolit automatické ladění ve službě Azure SQL Database a nechat automatické ladění funkce plně spravovat vaše úlohy najdete v tématu [povolit automatické ladění](sql-database-automatic-tuning-enable.md).
- Chcete-li použít ruční ladění, můžete zjistit [ladění doporučení na portálu Azure](sql-database-advisor-portal.md) a ty, které zlepšit výkon vašich dotazů použít ručně.
