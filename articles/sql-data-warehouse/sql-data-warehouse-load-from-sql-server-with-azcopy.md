---
title: aaaLoad dat z SQL serveru do Azure SQL Data Warehouse (PolyBase) | Microsoft Docs
description: "Využívá bcp tooexport data z tooflat soubory systému SQL Server, úložiště objektů blob tooAzure data tooimport AZCopy a PolyBase tooingest hello data do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4d42786a-fb28-43c9-9c3b-72d19c0ecc11
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 89e2a91bc97642e9fc18545cb802b42d8dc4ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Načtení dat z SQL Serveru do Azure SQL Data Warehouse (AZCopy)
Pomocí bcp a AZCopy nástroje příkazového řádku tooload dat z úložiště objektů blob tooAzure systému SQL Server. Potom pomocí funkce PolyBase nebo Azure Data Factory tooload hello data do Azure SQL Data Warehouse. 

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím tohoto kurzu potřebujete:

* Databázi SQL Data Warehouse
* nainstalovaný nástroj příkazového řádku bcp Hello
* Hello nainstalovaný nástroj příkazového řádku SQLCMD

> [!NOTE]
> Hello nástroje bcp a sqlcmd si můžete stáhnout z hello [Microsoft Download Center][Microsoft Download Center].
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a>Import dat do SQL Data Warehouse
V tomto kurzu vytvoříte tabulku v Azure SQL Data Warehouse a importovat data do tabulky hello.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Krok 1: Vytvoření tabulky v Azure SQL Data Warehouse
Z příkazového řádku použijte hello toorun sqlcmd následující dotaz toocreate tabulku ve vaší instanci:

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

> [!NOTE]
> V tématu [tabulky přehled] [ Table Overview] nebo [syntaxe CREATE TABLE] [ CREATE TABLE syntax] Další informace o vytváření tabulek v SQL Data Warehouse a hello  Možnosti dostupné v klauzuli WITH hello.
> 
> 

### <a name="step-2-create-a-source-data-file"></a>Krok 2: Vytvoření zdrojového datového souboru
Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového textového souboru a potom uložte tento soubor tooyour místního dočasného adresáře C:\Temp\DimDate2.txt.

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

> [!NOTE]
> Je důležité tooremember této bcp.exe nepodporuje kódování souborů UTF-8 hello. Při použití nástroje bcp.exe prosím použijte soubory ASCII nebo UTF-16.
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a>Krok 3: Připojení a import dat hello
Při použití nástroje bcp můžete připojit a import dat hello pomocí následující příkazu nahraďte hello hodnoty podle potřeby hello:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Můžete ověřit hello byla data načtena spuštěním následujícího dotazu pomocí sqlcmd hello:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

To by měla vrátit hello následující výsledky:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Krok 4: Vytvoření statistiky pro vaše nově načtená data
Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik. V pořadí tooget hello nejlepší výkon ze své dotazy je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením hello nebo dojít k významné změny v datech hello. Podrobné vysvětlení statistiky najdete v tématu hello [statistiky] [ Statistics] tématu ve skupině témat věnovaných vývoji hello. Níže je zběžný příklad jak načíst toocreate statistiku hello sestavily v tomto příkladu

Spusťte následující příkazy CREATE STATISTICS z na příkazovém řádku sqlcmd hello:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Export dat z SQL Data Warehouse
V tomto kurzu vytvoříte z tabulky v SQL Data Warehouse datový soubor. Bude exportu hello data, která jsme vytvořili výše tooa nového datového souboru s názvem DimDate2_export.txt.

### <a name="step-1-export-hello-data"></a>Krok 1: Export dat hello
Hello nástroj bcp můžete připojit a vyexportovat data pomocí následující příkazu nahraďte hello hodnoty podle potřeby hello:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Můžete ověřit hello data vyexportovala správně otevřením hello nový soubor. Hello data v souboru hello shodovat následujícím textem hello:

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

> [!NOTE]
> Z důvodu toohello povaze distribuovaných systémů nemusí být pořadí dat hello hello stejné napříč databázemi SQL Data Warehouse. Další možností je toouse hello **queryout** funkce bcp toowrite dotazu extrahovat místo vyexportování celé tabulky hello.
> 
> 

## <a name="next-steps"></a>Další kroky
Přehled načítání najdete v tématu [Načtení dat do SQL Data Warehouse][Load data into SQL Data Warehouse].
Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].

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
