---
title: aaaLoad dat z SQL serveru do Azure SQL Data Warehouse (bcp) | Microsoft Docs
description: "Pro malou velikost dat využívá bcp tooexport data z tooflat soubory systému SQL Server a importovat data hello přímo do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: a03b5403d123e8814ae73a7cce8e6851c8b264a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Načtení dat z SQL Serveru do Azure SQL Data Warehouse (ploché soubory)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Pro malé datové sady můžete použít hello bcp nástroj příkazového řádku tooexport data z SQL serveru a pak můžete načíst přímo tooAzure SQL datového skladu.

V tomto kurzu použijete nástroj bcp k následujícím operacím:

* Export tabulky z ze serveru SQL Server pomocí hello příkazu bcp out (nebo vytvoření jednoduchého ukázkového souboru)
* Importovat hello tabulky z plochého souboru tooSQL datového skladu.
* Vytvoření statistiky pro hello načíst data.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a>Než začnete
### <a name="prerequisites"></a>Požadavky
toostep prostřednictvím tohoto kurzu potřebujete:

* Databázi SQL Data Warehouse
* Hello bcp nainstalovaný nástroj příkazového řádku
* Hello sqlcmd nainstalovaný nástroj příkazového řádku

Hello nástroje bcp a sqlcmd si můžete stáhnout z hello [Microsoft Download Center][Microsoft Download Center].

### <a name="data-in-ascii-or-utf-16-format"></a>Data ve formátu ASCII nebo UTF-16
Pokud se tento kurz používáte svoje vlastní data, musí vaše data toouse hello ASCII nebo UTF-16 kódování, protože bcp nepodporuje kódování UTF-8. 

PolyBase podporuje UTF-8, ale zatím nepodporuje UTF-16. Všimněte si, že pokud chcete, aby toocombine bcp pomocí funkce PolyBase budete potřebovat data hello tootransform tooUTF-8 po vyexportování z SQL serveru. 

## <a name="1-create-a-destination-table"></a>1. Vytvoření cílové tabulky
Definujte tabulky v SQL Data Warehouse, které se bude hello cílové tabulky pro zatížení hello. Hello sloupců v tabulce hello musí odpovídat toohello dat v jednotlivých řádcích vašeho datového souboru.

toocreate tabulku, otevřete příkazový řádek a pomocí sqlcmd.exe toorun hello následující příkaz:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a>2. Vytvoření zdrojového datového souboru
Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového textového souboru a potom uložte tento soubor tooyour místního dočasného adresáře C:\Temp\DimDate2.txt. Tato data jsou ve formátu ASCII.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Volitelné) tooexport svoje vlastní data z databáze systému SQL Server, otevřete příkazový řádek a spusťte následující příkaz hello. TableName, ServerName, DatabaseName, Username a Password nahraďte svými vlastními informacemi.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-hello-data"></a>3. Načtení dat hello
tooload hello data, otevřete příkazový řádek a spusťte následující příkaz, nahraďte hello hodnoty pro název serveru, název databáze, uživatelské jméno a heslo s informacemi o sobě hello.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Použijte tento příkaz tooverify hello data načetla správně.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

výsledky Hello by měl vypadat takto:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

## <a name="4-create-statistics"></a>4. Vytvoření statistiky
SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik. tooget hello nejlepší výkon dotazů, je důležité toocreate statistiky pro všechny sloupce všech tabulek po prvním načtením hello nebo po dojít k významné změny v datech hello. Podrobné vysvětlení statistiky najdete v tématu [statistiky][Statistics]. 

Spusťte následující příkaz toocreate statistiky na nově načtenou tabulku hello.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. Export dat z SQL Data Warehouse
Cvičně si můžete vyexportovat hello dat, který jste právě načetli zpět z SQL Data Warehouse.  příkaz tooexport Hello je přesně hello stejné jako Export z SQL serveru.

Je však rozdíl ve výsledcích hello. Vzhledem k tomu, že hello data je uložená v distribuovaných umístěních v rámci SQL Data Warehouse, při exportu dat zapíše ho každý výpočetní uzel data toohello výstupní soubor. pořadí Hello hello dat v hello výstupní soubor je pravděpodobně toobe liší od pořadí hello hello dat ve vstupním souboru hello.

### <a name="export-a-table-and-compare-exported-results"></a>Export tabulky a porovnání vyexportovaných výsledků
toosee hello exportovaná data, otevřete příkazový řádek a spusťte tento příkaz pomocí svých vlastních parametrů. ServerName je název hello Azure logického SQL serveru.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Můžete ověřit hello data vyexportovala správně otevřením hello nový soubor. Hello data v souboru hello shodovat následujícím textem hello, ale bude pravděpodobně uvedená v jiném pořadí:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-hello-results-of-a-query"></a>Export hello výsledků dotazu
Můžete použít hello **queryout** funkce bcp tooexport hello výsledků dotazu místo vyexportování celé tabulky hello. 

## <a name="next-steps"></a>Další kroky
Přehled načítání najdete v tématu [Načtení dat do SQL Data Warehouse][Load data into SQL Data Warehouse].
Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].
V tématu [tabulky přehled] [ Table Overview] nebo [syntaxe CREATE TABLE] [ CREATE TABLE syntax] Další informace o vytváření tabulek v SQL Data Warehouse.

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
