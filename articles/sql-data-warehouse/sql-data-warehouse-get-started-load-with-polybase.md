---
title: aaaPolyBase v SQL Data Warehouse kurzu | Microsoft Docs
description: "Zjistěte, co je PolyBase a jak toouse pro scénáře datových skladů."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 0a0103b4-ddd6-4d1e-87be-4965d6e99f3f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/01/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 3e680ec407c1d920dd59ea922b82c9208b5e9a84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Načtení dat pomocí funkce PolyBase v SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

Tento kurz ukazuje, jak tooload dat do SQL Data Warehouse pomocí AzCopy a PolyBase. Po dokončení tohoto kurzu budete vědět, jak:

* Používání úložiště blob tooAzure data toocopy AzCopy
* Vytvoření databázových objektů toodefine hello dat
* Hello dat tooload dotazu T-SQL

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím tohoto kurzu budete potřebovat.

* Databázi SQL Data Warehouse
* Účet úložiště Azure typu Standard Locally Redundant Storage (Standard-LRS), Standard Geo-Redundant Storage (Standard-GRS) nebo Standard Read-Access Geo-Redundant Storage (Standard-RAGRS)
* Nástroj příkazového řádku AzCopy. Stáhněte a nainstalujte hello [nejnovější verzi AzCopy] [ latest version of AzCopy] nainstalované s hello Microsoft Azure Storage Tools.
  
    ![Nástroje Azure Storage Tools](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-tooazure-blob-storage"></a>Krok 1: Přidání ukázkových dat tooAzure objektu blob úložiště
V datech tooload pořadí potřebujeme tooput ukázková data do Azure blob storage. V tomto kroku naplníme objekt blob úložiště Azure ukázkovými daty. Později použijeme PolyBase tooload Tato ukázková data do databáze SQL Data Warehouse.

### <a name="a-prepare-a-sample-text-file"></a>A. Příprava ukázkového textového souboru
tooprepare ukázkový textový soubor:

1. Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového souboru. Uložte tento tooyour místního dočasného adresáře jako % temp%\DimDate2.txt.

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

### <a name="b-find-your-blob-service-endpoint"></a>B. Vyhledání vašeho koncového bodu Služby objektů blob
toofind vašeho koncového bodu služby objektů blob:

1. Hello portálu Azure vyberte **Procházet** > **účty úložiště**.
2. Klikněte na tlačítko hello úložiště účet, že který má toouse.
3. V okně účtu úložiště hello klikněte na objekty BLOB
   
    ![Klikněte na Objekty blob.](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. Uložte si adresu URL koncového bodu Služby objektů blob pro pozdější použití.
   
    ![Koncový bod Služby objektů blob](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Vyhledání klíče účtu úložiště Azure
toofind klíče účtu úložiště Azure:

1. Hello portálu Azure, vyberte **Procházet** > **účty úložiště**.
2. Klikněte na účet úložiště hello, že chcete toouse.
3. Vyberte **Všechna nastavení** > **Přístupové klíče**.
4. Klikněte na tlačítko hello kopírování pole toocopy mezi schránky toohello klíče přístup.
   
    ![Zkopírování klíče úložiště Azure](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-hello-sample-file-tooazure-blob-storage"></a>D. Zkopírujte hello ukázkový soubor tooAzure objektu blob úložiště
toocopy úložiště objektů blob tooAzure dat:

1. Otevřete příkazový řádek a změnit instalační adresář AzCopy toohello adresáře. Tento příkaz změní toohello výchozí instalační adresář v klientovi Windows 64-bit.
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. Spusťte následující příkaz tooupload hello soubor hello. Zadejte svoji adresu URL koncového bodu Služby objektů blob pro <blob service endpoint URL> a klíč účtu úložiště Azure pro <klíč_účtu_úložiště_azure>.
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Viz také [Začínáme s hello příkazového řádku Azcopy][Getting Started with hello AzCopy Command-Line Utility].

### <a name="e-explore-your-blob-storage-container"></a>E. Prozkoumání vašeho kontejneru úložiště objektů blob
toosee hello soubor, který jste nahráli tooblob úložiště:

1. Vraťte se zpátky tooyour objektu Blob služby okno.
2. V části Kontejnery poklikejte na **datacontainer**.
3. tooexplore hello cesta tooyour data, klikněte na složku hello **datedimension** a zobrazí se nahraný soubor **DimDate2.txt**.
4. Klikněte na tlačítko Vlastnosti tooview **DimDate2.txt**.
5. Všimněte si, že v okně Vlastnosti hello objektů Blob můžete stáhnout nebo odstranit soubor hello.
   
    ![Zobrazení objektu blob úložiště Azure](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-hello-sample-data"></a>Krok 2: Vytvoření externí tabulky pro hello ukázková data
V této části vytvoříme externí tabulku, která definuje ukázková data hello.

PolyBase používá externí tabulky tooaccess data v úložišti objektů blob Azure. Vzhledem k tomu, že hello data nejsou uložená v SQL Data Warehouse, zpracovává PolyBase ověřování toohello externí data pomocí přihlašovacích údajů platných pro databázi.

Příklad Hello v tomto kroku používá tyto toocreate příkazy jazyka Transact-SQL externí tabulky.

* [Vytvoření hlavního klíče (Transact-SQL)] [ Create Master Key (Transact-SQL)] tooencrypt hello tajný klíč vaší databáze vašich přihlašovacích údajů.
* [Create Database Scoped Credential (Transact-SQL)] [ Create Database Scoped Credential (Transact-SQL)] toospecify ověřovacích informací pro váš účet úložiště Azure.
* [Create External Data Source (Transact-SQL)] [ Create External Data Source (Transact-SQL)] toospecify hello umístění úložiště objektů blob v Azure.
* [Create External File Format (Transact-SQL)] [ Create External File Format (Transact-SQL)] toospecify hello formát data.
* [Create External Table (Transact-SQL)] [ Create External Table (Transact-SQL)] definice tabulky hello toospecify a umístění hello data.

Spusťte tento dotaz na databázi SQL Data Warehouse. Vytvoří externí tabulku s názvem DimDate2External ve schématu dbo hello, který odkazuje ukázková data DimDate2.txt toohello v hello Azure blob storage.

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required tooencrypt hello credential secret in hello next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication tooAzure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs tooaccess data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide hello credential created in hello previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create hello external table
-- Specify column names and data types. This needs toomatch hello data in hello sample file.
-- LOCATION: Specify path toofile or directory that contains hello data (relative toohello blob container).
-- toopoint tooall files under hello blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on hello external table

SELECT count(*) FROM dbo.DimDate2External;

```


V Průzkumníku objektů SQL Server v sadě Visual Studio uvidíte formát externích souborů hello, externí zdroj dat a tabulku DimDate2External hello.

![Zobrazení externí tabulky](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Krok 3: Načtení dat do SQL Data Warehouse
Po vytvoření externí tabulky hello můžete načíst hello data do nové tabulky nebo vložit do existující tabulky.

* tooload hello data do nové tabulky, spusťte hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] příkaz. Hello nová tabulka bude mít hello sloupce pojmenované v dotazu hello. Hello datové typy sloupců hello bude odpovídat hello datových typů v definici externí tabulky hello.
* tooload hello data do existující tabulky, použijte hello [INSERT... SELECT (Transact-SQL)] [ INSERT...SELECT (Transact-SQL)] příkaz.

```sql
-- Load hello data from Azure blob storage tooSQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Krok 4: Vytvoření statistiky pro nově načtená data
SQL Data Warehouse nevytváří ani neaktualizuje statistiku automaticky. Proto tooachieve vysokého výkonu dotazu, je důležité nejdřív načíst toocreate statistiku pro každý sloupec každé tabulky po hello. Je také důležité tooupdate statistiku po důležitých změnách v datech hello.

Tento příklad vytvoří jednosloupcovou statistiku na novou tabulku DimDate2 hello.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Další, najdete v části toolearn [statistiky][Statistics].  

## <a name="next-steps"></a>Další kroky
V tématu hello [Průvodce funkcí PolyBase] [ PolyBase guide] Další informace, které byste měli vědět, když budete vyvíjet řešení využívající funkci PolyBase.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[PolyBase guide]: ./sql-data-warehouse-load-polybase-guide.md
[Getting Started with hello AzCopy Command-Line Utility]:../storage/common/storage-use-azcopy.md
[latest version of AzCopy]:../storage/common/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
