---
title: "aaaGuide pro používání funkce PolyBase v SQL Data Warehouse | Microsoft Docs"
description: "Pokyny a doporučení pro používání funkce PolyBase v SQL Data Warehouse scénáře."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 6/5/2016
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: b05e4c5d528f2fe1c60d6855b5333065f0c908ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Příručka pro používání funkce PolyBase v SQL Data Warehouse
Tato příručka poskytuje praktické informace pro používání funkce PolyBase v SQL Data Warehouse.

tooget začít, najdete v části hello [načtení dat pomocí funkce PolyBase] [ Load data with PolyBase] kurzu.

## <a name="rotating-storage-keys"></a>Rotaci klíčů k úložišti
Z času tootime chcete úložiště objektů blob klíče tooyour přístup hello toochange z bezpečnostních důvodů.

Hello nejvíce elegantní tooperform způsob, jak tuto úlohu je toofollow tento proces se označuje jako "rotaci klíčů hello". Jste si všimli, že máte dva klíče úložiště pro váš účet úložiště objektů blob. Toto je tak, aby můžete přejít

Otáčení klíče účtu úložiště Azure je jednoduchý třech krocích

1. Vytvoření druhého oboru databáze pověření podle hello úložiště sekundární přístupový klíč
2. Vytvořte druhý externí zdroj dat založena na této nových přihlašovacích údajů
3. Vyřaďte a vytvořte hello externích tabulek odkazující toohello nový zdroj externích dat

Pokud jste migrovali všechny externí tabulky toohello nového externí zdroje dat. pak můžete provádět hello vyčištění úlohy:

1. Vyřaďte první externí zdroj dat
2. První vyřaďte databázi vašich přihlašovacích údajů podle hello primárního úložiště přístupového klíče
3. Přihlaste se k Azure a znovu vygenerovat primární přístupový klíč hello připravené pro hello příště

## <a name="query-azure-blob-storage-data"></a>Dotaz na data úložiště objektů blob v Azure
Dotazy na externí tabulky jednoduše použijte název tabulky hello, jako kdyby byl relační tabulku.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> Dotaz na externí tabulky může selhat s chybou hello *"dotaz byl zrušen--při čtení z externího zdroje bylo dosaženo prahové hodnoty pro maximální odmítněte k hello"*. To znamená, že externích dat obsahuje *nekonzistence* záznamy. Záznam dat se považuje za 'nekonzistence' Pokud hello skutečná data typy nebo počet sloupců neodpovídají žádné definice sloupců hello hello externí tabulky nebo pokud hello data neodpovídají toohello zadaný externí soubor formátu. toofix, zajistěte, aby externí tabulky a definice formátu externího souboru jsou správné a externích dat vyhovuje toothese definice. V případě, že podmnožinu záznamů externích dat jsou chybná, můžete tooreject tyto záznamy pro své dotazy pomocí možnosti odmítněte hello ve vytvoření externí tabulky DDL.
> 
> 

## <a name="load-data-from-azure-blob-storage"></a>Načtení dat z Azure Blob Storage
Tento příklad načte data z databáze datového skladu tooSQL úložiště objektů blob v Azure.

Ukládání dat přímo odebere hello doba přenosu dat pro dotazy. Ukládání dat s indexem columnstore zlepšuje výkon dotazů pro analýzu dotazy pomocí až too10x.

Tento příklad používá hello CREATE TABLE AS SELECT příkaz tooload data. Nová tabulka Hello dědí hello sloupce pojmenované v dotazu hello. Zdědil hello datové typy tyto sloupce z hello definici externí tabulky.

CREATE TABLE AS SELECT je vysoce původce příkazu Transact-SQL, který načítá data hello v paralelní tooall hello výpočetní uzly SQL Data Warehouse.  To byl původně vytvořen pro modul hello (MPP) massively parallel processing v Analytics Platform System a je nyní v SQL Data Warehouse.

```sql
-- Load data from Azure blob storage tooSQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

V tématu [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].

## <a name="create-statistics-on-newly-loaded-data"></a>Vytvoření statistiky pro nově načtená data
Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.  V pořadí tooget hello nejlepší výkon ze své dotazy je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením hello nebo dojít k významné změny v datech hello.  Podrobné vysvětlení statistiky najdete v tématu hello [statistiky] [ Statistics] tématu ve skupině témat věnovaných vývoji hello.  Níže je zběžný příklad jak načíst toocreate statistiku hello sestavily v tomto příkladu.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-tooazure-blob-storage"></a>Export úložiště objektů blob tooAzure dat
Tato část uvádí, jak tooexport dat z SQL Data Warehouse tooAzure blob storage. Tento příklad používá vytvořit externí TABLE AS SELECT což je vysoce původce Transact-SQL příkaz tooexport hello dat paralelně ze všech hello výpočetních uzlů.

Hello následující příklad vytvoří externí tabulku Weblogs2014 pomocí definice sloupců a data z dbo. Tabulka Weblogů. definici externí tabulky Hello je uložená v SQL Data Warehouse a hello výsledky příkazu SELECT hello jsou exportovaný toohello "/ archivaci/log2014 /" adresář zadaný zdroj dat hello kontejner objektů blob hello. Hello data se exportují ve formátu souboru zadaným textem hello.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a>Izolovat načítání uživatelů
Je často nutné toohave více uživatelů, které můžete načíst data do datového skladu SQL. Protože hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] vyžaduje oprávnění pro řízení hello databáze, budete mít s více uživateli pomocí řízení přístupu přes všechny schémat. toolimit, můžete použít příkaz ODEPŘÍT řízení hello.

Příklad: Vzít schema_A schémat databáze pro oddělení A a schema_B pro oddělení B umožní user_A databáze uživatelů a user_B uživatelů pro PolyBase načtení z oddělení A a B, v uvedeném pořadí. Oba mají udělené oprávnění pro řízení databáze.
Tvůrci Hello schématu A a B nyní zamknout jejich schémata pomocí ODEPŘÍT:

```sql
   DENY CONTROL ON SCHEMA :: schema_A toouser_B;
   DENY CONTROL ON SCHEMA :: schema_B toouser_A;
```   
 Pomocí tohoto user_A a user_B by měl nyní být uzamčený hello schématu jiných oddělení.
 


## <a name="next-steps"></a>Další kroky
toolearn Další informace o přesunutí dat tooSQL datového skladu, najdete v části hello [Přehled migrace dat][data migration overview].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
