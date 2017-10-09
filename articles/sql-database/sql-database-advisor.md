---
title: "doporučení aaaPerformance – Azure SQL Database | Microsoft Docs"
description: "Hello Azure SQL Database nabízí doporučení pro vaše databáze SQL, který může zlepšit výkon aktuální dotaz."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 1db441ff-58f5-45da-8d38-b54dc2aa6145
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 77db338a0a395aec78c9e02849ae5ba4f2d01680
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-recommendations"></a>Doporučení výkonu

Azure SQL Database zjišťuje a přizpůsobuje s vaší aplikací a poskytuje přizpůsobené doporučení, takže se budete toomaximize hello výkon vaší databáze SQL. Výkon se nepřetržitě hodnotí analýzou historie využití vaší databáze SQL. Hello doporučení, které jsou k dispozici jsou založené na vzor jedinečný zatížení databáze a zvýšit jeho výkon.

> [!NOTE]
> Doporučený způsob použití doporučení je v databázi povolíte, automatické ladění'. Podrobnosti najdete v tématu [automatické ladění](sql-database-automatic-tuning.md).
>

## <a name="create-index-recommendations"></a>Vytvoření doporučení indexu
Azure SQL Database nepřetržitě monitoruje hello dotazy spouštěna a identifikuje hello indexy, které může zlepšit výkon hello. Po dostatek spolehlivosti, určité index chybí, nový **vytvořit index** doporučení se vytvoří. Azure SQL Database sestavení spolehlivosti odhadem výkonu nárůst hello indexu by přepněte prostřednictvím čas. V závislosti na hello odhadované zvýšení výkonu je zde uvedena doporučení, která jsou klasifikovány jako Vysoká, střední nebo Nízká. 

Indexy vytvořené pomocí doporučení jsou vždy označeny jako auto_created indexy. Uvidíte, jaké indexy jsou auto_created prohlížením sys.indexes zobrazení. Automaticky vytvořit indexy neblokovat příkazy ALTER nebo přejmenovat. Pokud se pokusíte toodrop hello sloupec, který má automaticky vytvořit index nad ním, předá příkaz hello a hello automaticky vytvořit index je vyřadit pomocí příkazu hello také. Příkaz ALTER/přejmenování hello na sloupce, které jsou indexované by bloku obyčejné indexy.

Po vytvoření hello doporučení indexu se použije, Azure SQL Database se porovná výkonu dotazů hello hello základní výkon. Pokud nový index uvést do režimu vylepšení výkonu hello, doporučení se označilo jako úspěšné a dopad sestava bude k dispozici. V případě, že hello index nebylo přineste hello výhody, ji budou automaticky zrušeny. Tímto způsobem Azure SQL Database zajišťuje, že pomocí doporučení bude pouze zlepšit výkon databáze hello.

Všechny **vytvořit index** doporučení obsahuje regrese zásadu, která nebude povolovat použití hello doporučení, pokud využití DTU databáze nebo fond hello byla vyšší než 80 % v posledních 20 minut nebo pokud hello úložiště je více než 90 % využití. V takovém případě bude odložit hello doporučení.

## <a name="drop-index-recommendations"></a>Doporučení k odstranění indexu
Kromě toho toodetecting chybí index, Azure SQL Database průběžně analyzuje hello výkon stávající indexy. Pokud se nepoužívá index, bude Azure SQL Database doporučujeme jej vyřadíte. Vyřazení indexu se doporučuje ve dvou případech:
* Index je duplicitní s jiným indexem (stejná indexována a zahrnuté sloupce, schéma oddílu a filtry)
* Index se nepoužívá po delší dobu (93 dny)

Doporučení k odstranění indexu také projít hello ověření po implementaci. Pokud vyšší výkon hello hello dopad sestavy je k dispozici. V případě, že se detekuje snížení výkonu, budou vráceny doporučení.


## <a name="parameterize-queries-recommendations"></a>Parametrizace doporučení dotazy
**Parametrizace dotazy** doporučení se zobrazí, když máte jeden nebo více dotazů, které jsou neustále překompilovat, ale nebudete mít hello stejný plán spuštění dotazu. Tato podmínka otevře tooapply možnost vynutit Parametrizace, což vám umožní plány dotazů toobe do mezipaměti a opakovaně používat ve hello budoucí zvýšení výkonu a snížení využití prostředků. 

Každý dotaz původně vydaný pro SQL Server musí toobe zkompilovat toogenerate plán spuštění. Každý generovaného plán se přidá toohello mezipaměti plánu a následné spuštění služby hello stejný dotaz můžete opakovaně použít tento plán z mezipaměti hello, což eliminuje potřebu hello další kompilace. 

Aplikace, které odesílají dotazy, které obsahují hodnoty parametry, může způsobit tooa zatížení, které pro každý dotaz s jiným parametrem hodnotami plán spuštění hello kompiluje znovu. V mnoha případech hello stejné dotazy s jiným parametrem, který generování hodnoty hello stejné provádění plánů, ale tyto plány jsou stále samostatně přidat toohello mezipaměti plánu. Nutnosti rekompilace provádění plány pomocí databáze prostředků, zvýšit hello trvání čas a přetečení hello plán dotazu, že příčinou mezipaměti plány toobe vyřazování z mezipaměti hello. Toto chování systému SQL Server může být změněna nastavením hello vynutit Parametrizace možnost v databázi hello. 

toohelp odhad hello dopad toto doporučení, které jsou k dispozici s porovnání mezi skutečné procesoru hello využití a hello předpokládané využití procesoru (jako v případě, že bylo použito hello doporučení). Kromě tooCPU úspory, doba trvání vašeho dotazu zkracuje dobu hello stráví v kompilaci. Také bude mnohem menší nároky na mezipaměti plánu, což většina hello plány toostay v mezipaměti a znovu použít. Toto doporučení můžete použít snadno a rychle kliknutím na hello **použít** příkaz. 

Když použijete toto doporučení, zapne vynucené Parametrizace minut ve vaší databázi a začne hello monitorování proces, který přibližně trvá po dobu 24 hodin. Po uplynutí této doby budete mít možnost toosee hello ověření sestavu, která ukazuje využití procesoru vaší databáze 24 hodin, než se a po použití hello doporučení. Poradce pro funkci SQL Database má bezpečnostní mechanismus, který automaticky vrátí doporučení hello použije v případě, že byla zjištěna snížení výkonu.

## <a name="fix-schema-issues-recommendations"></a>Opravte problémy doporučení schématu
**Opravte problémy schématu** doporučení se zobrazí při hello služba SQL Database oznámení anomálií hello počet chyby související s schématu SQL aktivit ve vaší databázi SQL Azure. Toto doporučení se obvykle zobrazují, když databáze dojde více schématu související chyby (neplatný název sloupce, neplatný název objektu atd.) v rámci hodiny.

"Schématu problémy" jsou třídou chyby syntaxe v systému SQL Server, které dojít v případě, že nejsou zarovnány hello Definice dotazu SQL hello a hello definice schématu databáze hello. Například jeden ze sloupců hello očekávanou hello dotazu může být chybějící v cílové tabulce hello nebo naopak. 

"Opravit problém schématu" doporučení se zobrazí, když služba Azure SQL Database oznámení anomálií hello počet chyby související s schématu SQL aktivit ve vaší databázi SQL Azure. Následující tabulka ukazuje hello chyby, které jsou problémy související tooschema Hello:

| Kód chyby SQL | Zpráva |
| --- | --- |
| 201 |Procedura nebo funkce,*'očekává parametr'*', která nebyla zadána. |
| 207 |Neplatný název sloupce ' *'. |
| 208 |Neplatný název objektu ' *'. |
| 213 |Název sloupce nebo počet zadaných hodnot neodpovídá definici tabulky. |
| 2812 |Nelze nalézt uloženou proceduru ' *'. |
| 8144 |Procedura nebo funkce * má příliš mnoho zadaných argumentů. |

## <a name="next-steps"></a>Další kroky
Sledovat vaše doporučení a pokračovat tooapply je toorefine výkonu. Databázové úlohy jsou dynamické a průběžně změnu. Databáze SQL advisor pokračuje toomonitor a poskytovat doporučení, které může zlepšit výkon vaší databáze. 

* V tématu [výkonu doporučení v hello portál Azure](sql-database-advisor-portal.md) kroky jak hello toouse výkonu doporučení v portálu Azure.
* V tématu [Query Performance Insight](sql-database-query-performance.md) toolearn o a zobrazení hello vlivu na výkon nejčastějších dotazů.

## <a name="additional-resources"></a>Další zdroje
* [Úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx)
* [VYTVOŘENÍ INDEXU](https://msdn.microsoft.com/library/ms188783.aspx)
* [Řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md)

