---
title: "aaaMigrate tooSQL vašeho schématu datového skladu | Microsoft Docs"
description: "Tipy pro migraci vašeho schématu tooAzure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 1309b743b78564575695038a4856d9d25a2b18d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-schemas-toosql-data-warehouse"></a>Migrace vaší schémata tooSQL datového skladu
Pokyny k migraci vaší tooSQL schémata SQL datového skladu. 

## <a name="plan-your-schema-migration"></a>Plánování migrace schématu

Při plánování migrace najdete v části hello [tabulky přehled] [ table overview] toobecome obeznámeni s aspekty návrhu tabulky například statistiky, distribuci, vytváření oddílů a indexování.  Také uvádí některé [nepodporované funkce tabulky] [ unsupported table features] a jejich řešení.

## <a name="use-user-defined-schemas-tooconsolidate-databases"></a>Použít vlastní schémata tooconsolidate databáze

Vaše stávající úlohy pravděpodobně obsahuje více než jednu databázi. Datového skladu SQL serveru může například obsahovat pracovní databáze, databáze datového skladu a některé databáze datového tržiště. V této topologii spouští každou databázi jako samostatné zatížení zásadám samostatné zabezpečení.

Naopak spouští SQL Data Warehouse hello celého datového skladu zatížení v rámci jedné databáze. Mezi databáze nejsou povoleny spojení. Proto SQL Data Warehouse očekává, že všechny tabulky použité ve hello datového skladu toobe uložené v rámci jedné databáze hello.

Doporučujeme používat vlastní schémata tooconsolidate vaše stávající úlohy do jedné databáze. Příklady najdete v tématu [uživatelem definované schémata](sql-data-warehouse-develop-user-defined-schemas.md)

## <a name="use-compatible-data-types"></a>Použít kompatibilní datové typy
Upravte vaše toobe typy dat kompatibilní s SQL Data Warehouse. Seznam podporované a nepodporované datové typy, naleznete v části [datové typy][data types]. Toto téma poskytuje řešení pro hello nepodporované typy. Poskytuje také dotazu tooidentify existující typy, které nejsou podporované v SQL Data Warehouse.

## <a name="minimize-row-size"></a>Minimální velikost řádku
Pro nejlepší výkon minimalizujte hello délku řádku tabulky. Vzhledem k tomu, že kratší délky řádek vést toobetter výkonu, použijte hello nejmenší typy dat, které fungovat pro vaše data. 

Šířka řádku tabulky má PolyBase omezení 1 MB.  Pokud máte v plánu tooload dat do SQL Data Warehouse pomocí PolyBase, aktualizujte vaše tabulky toohave maximální šířky menší než 1 MB. 

<!--
- For example, this table uses variable length data but hello largest possible size of hello row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and hello defined row width is less than one MB. When loading rows, PolyBase allocates hello full length of hello variable-length data. hello full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-hello-distribution-option"></a>Zadejte možnosti distribuce hello
SQL Data Warehouse je systém distribuovanou databázi. Každá tabulka je distribuovat nebo replikovat mezi hello výpočetní uzly. Není možnost tabulky, který umožňuje určit, jak toodistribute hello data. Hello jsou možnosti kruhové dotazování, replikují, nebo distribuovat algoritmu hash. Každý má výhody a nevýhody. Pokud nezadáte možnosti distribuce hello, SQL Data Warehouse použije jako výchozí hello kruhového dotazování.

- Výchozí hello je kruhového dotazování. Je nejjednodušší toouse hello a načte data hello tak rychlý jako možná, ale spojení budou vyžadovat přesun dat, což zpomalí výkon dotazů.
- Replikované uloží kopie hello tabulky na každém výpočetním uzlu. Replikované tabulky jsou původce, protože nevyžadují přesun dat pro spojování a agregaci. Vyžadovat dodatečné úložiště a proto nejvhodnější pro menší tabulky.
- Hodnota hash distribuované distribuuje hello řádků pro všechny uzly hello prostřednictvím funkce hash. Hodnota hash distribuované tabulky jsou hello vysílat služby SQL Data Warehouse vzhledem k tomu, že jsou navrženou tooprovide vysokého výkonu dotazu na velké tabulky. Tato možnost vyžaduje některé plánování tooselect hello nejlepší sloupce v datových toodistribute hello. Ale když nebude hello nejlepší sloupec hello poprvé, můžete snadno znovu distribuovat hello dat na jiný sloupec. 

najdete v části toochoose hello nejvhodnější distribuční možnosti pro každou tabulku [distribuované tabulky](sql-data-warehouse-tables-distribute.md).


## <a name="next-steps"></a>Další kroky
Po úspěšné migraci vaší tooSQL schéma databáze datového skladu pokračujte tooone hello následující články:

* [Migrace dat][Migrate your data]
* [Migrace vašeho kódu][Migrate your code]

Další informace o osvědčených postupech pro SQL Data Warehouse najdete v tématu hello [osvědčené postupy] [ best practices] článku.

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->
