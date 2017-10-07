---
title: "zobrazení aaaUsing T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání zobrazení jazyka Transact-SQL v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 3990b133946621691bdfa4b09523d21867470c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-sql-data-warehouse"></a>Zobrazení v SQL Data Warehouse
Zobrazení jsou obzvláště užitečná v SQL Data Warehouse. Použitím mnoha různými způsoby tooimprove hello kvality vašeho řešení.  V tomto článku klade důraz několik příkladů jak tooenrich řešení pomocí zobrazení, jakož i hello omezení, které je třeba toobe považována za.

> [!NOTE]
> Syntaxe pro `CREATE VIEW` není popsané v tomto článku. Podrobnosti najdete toohello [vytvořit zobrazení] [ CREATE VIEW] článku na webu MSDN pro tuto referenční informace.
> 
> 

## <a name="architectural-abstraction"></a>Abstrakce architektury
Je velmi běžné vzor aplikací toore-vytvořit pomocí vytvoření tabulky AS vyberte funkce CTAS () a potom objektem přejmenování vzor, zatímco načítání dat tabulky.

Následující příklad Hello přidá nové datum záznamy tooa datum dimenze. Všimněte si, jak nové tabble, DimDate_New, je poprvé vytvořen a pak přejmenovat tooreplace hello původní verzi hello tabulky.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate tooDimDate_Old;
RENAME OBJECT DimDate_New tooDimDate;

```

Tento přístup může však způsobit tabulky zobrazování a ztrácejí ze zobrazení uživatele, jakož i "tabulka neexistuje" chybové zprávy. Zobrazení může být použité tooprovide uživatelům konzistentní prezentační vrstvy, i když jsou přejmenovány hello základní objekty. Tím, že poskytuje uživatelům přístup k datům toohello prostřednictvím zobrazení, znamená, že uživatelé nepotřebují toohave viditelnost hello základní tabulky. To nabízí konzistentní uživatelské prostředí a zajistit, že můžete hello data warehouse Designer momentální hello datový model a maximalizovat výkon pomocí funkce CTAS během procesu načítání dat hello.    

## <a name="performance-optimization"></a>Optimalizace výkonu
Zobrazení může být také využívané tooenforce výkonu optimalizované spojení mezi tabulkami. Například zobrazení můžete začlenit redundantní distribučního klíče jako součást hello připojení přesun dat toominimize kritéria.  Další výhodou zobrazení může být spojující pomocný parametr nebo tooforce specifického dotazu. Použití zobrazení tímto způsobem zaručuje, že vždy spojení optimální způsobem zabraňující hello potřebu uživatelé tooremember hello správné konstrukce pro jejich spojení.

## <a name="limitations"></a>Omezení
Zobrazení v SQL Data Warehouse jsou pouze metadata.  V důsledku toho nejsou k dispozici hello následující možnosti:

* Neexistuje žádná vazba možnost schématu
* Základní tabulky nelze aktualizovat prostřednictvím zobrazení hello
* Zobrazení nelze vytvořit přes dočasných tabulek
* Neexistuje žádná podpora pro hello ROZBALTE / NOEXPAND pomocné parametry
* Nejsou žádná indexované zobrazení v SQL Data Warehouse

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].
Pro `CREATE VIEW` syntaxe naleznete příliš[vytvořit zobrazení][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
