---
title: "aaaLoad – Azure Data Lake Store tooSQL datového skladu | Microsoft Docs"
description: "Zjistěte, jak toouse PolyBase externí tabulky tooload dat z Azure Data Lake Store do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 01/25/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 50ef23b3eba5f58bc9974095f84140dc5c11fa4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a>Načtení dat z Azure Data Lake Store do SQL Data Warehouse
Tento dokument poskytuje všechny kroky nutné svoje vlastní data z Azure Data Lake Store (ADLS) pro tooload do SQL Data Warehouse pomocí PolyBase.
Zatímco přes hello data uložená v ADLS pomocí hello externí tabulky se může toorun ad hoc dotazy jako osvědčený postup doporučujeme importování dat hello do hello SQL Data Warehouse.
Odhad času: 10 minut za předpokladu, že splňujete požadavky hello potřebovat toocomplete.
V tomto kurzu se dozvíte, jak:

1. Vytvořte objekty tooload externí databáze z Azure Data Lake Store.
2. Připojte tooan Azure Data Lake Store Directory.
3. Načtení dat do Azure SQL Data Warehouse.

## <a name="before-you-begin"></a>Než začnete
toorun tohoto kurzu potřebujete:

* Pro ověřování do služby Azure toouse aplikace Active Directory. toocreate, postupujte podle [ověřování služby Active directory](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)

>[!NOTE] 
> Potřebujete ID klienta hello, klíč a hodnotu OAuth2.0 tokenu koncového bodu z vaší aplikace Active Directory tooconnect tooyour Azure Data Lake z SQL Data Warehouse. Podrobnosti o tom, jak tooget tyto hodnoty jsou v hello výše uvedený odkaz.
>Poznámka pro registrace Azure Active Directory aplikace pomocí hello "ID aplikace, jako hello ID klienta.

* SQL Server Management Studio nebo SQL Server Data Tools, toodownload SSMS a připojte najdete [dotazu SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)

* Postupujte podle toocreate jeden Azure SQL Data Warehouse: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision

* Azure Data Lake Store, s nebo bez šifrování povolené. postupujte podle jednoho toocreate: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal




## <a name="configure-hello-data-source"></a>Konfigurace zdroje dat hello
PolyBase používá T-SQL externí objekty toodefine hello umístění a atributy externích dat hello. Hello externí objekty jsou uloženy v SQL Data Warehouse a referenční dokumentace hello data, která je tý uložených externě.


###  <a name="create-a-credential"></a>Vytvoření přihlašovacích údajů
tooaccess vaše Azure Data Lake úložiště, budete potřebovat toocreate hlavní klíč databáze tooencrypt váš tajný klíč pověření použít v dalším kroku hello.
Pak vytvořte přihlašovací údaje databáze obor, který ukládá hello hlavní přihlašovací údaje služby nastavit v AAD. Syntaxe pro těch, které jste, který jste použili PolyBase tooconnect tooWindows objektů BLOB služby Azure Storage, Všimněte si, že pověření hello se liší.
tooconnect tooAzure Data Lake Store, musíte **první** vytvoření aplikace Azure Active Directory, vytvořte přístupový klíč a udělte hello aplikace přístup toohello Azure Data Lake prostředek. Instrucitons tooperform tyto kroky jsou umístěné [zde](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required tooencrypt hello credential secret in hello next step.
-- For more information on Master Key: https://msdn.microsoft.com/en-us/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass hello client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/en-us/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- It should look something like this:
CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```


### <a name="create-hello-external-data-source"></a>Vytvoření hello externí zdroj dat
Použít [vytvořit externí zdroj dat] [ CREATE EXTERNAL DATA SOURCE] příkaz toostore hello umístění hello dat a datový typ hello.
Hello ADL URI najdete v hello portál Azure a www.portal.azure.com.

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs tooaccess data in Azure Data Lake Store.
-- LOCATION: Provide Azure Data Lake accountname and URI
-- CREDENTIAL: Provide hello credential created in hello previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalakestore.net',
    CREDENTIAL = ADLCredential
);
```



## <a name="configure-data-format"></a>Konfigurovat formát dat
tooimport hello data z ADLS, musíte formát externích souborů toospecify hello. Tento příkaz má toodescribe možnosti specifické pro formát data.
Dole je příklad formát běžně používané souboru, který je oddělený kanálu textový soubor.
Podívejte se na dokumentaci T-SQL pro úplný seznam [vytvořit EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT]

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks hello end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies hello field terminator for data of type string in hello text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store all Missing values as NULL

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-hello-external-tables"></a>Vytvoření hello externí tabulky
Teď, když jste zadali hello datového zdroje a formát souboru, jste připravené toocreate hello externí tabulky. Externí tabulky se, jak pracovat s externí data. PolyBase používá rekurzivní directory traversal tooread všechny soubory v všechny podadresáře adresáře hello zadaný v parametru umístění hello. Navíc hello následující příklad ukazuje, jak toocreate hello objektu. Je nutné toocustomize hello příkaz toowork hello daty, které máte v ADLS.

```sql
-- D: Create an External Table
-- LOCATION: Folder under hello ADLS root folder.
-- DATA_SOURCE: Specifies which Data Source Object toouse.
-- FILE_FORMAT: Specifies which File Format Object toouse
-- REJECT_TYPE: Specifies how you want toodeal with rejected rows. Either Value or percentage of hello total
-- REJECT_VALUE: Sets hello Reject value based on hello reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStore
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Aspekty externí tabulky
Vytvoření externí tabulky je jednoduché, ale existují některé drobné odlišnosti, které je třeba toobe popsané.

Načítání dat pomocí funkce PolyBase je silného typu. To znamená, že každý řádek dat hello se požity musí splňovat definice schématu tabulky hello.
Pokud daný řádek neodpovídá definice schématu hello, hello řádek byl odmítnut z hello zatížení.

Hello možnosti REJECT_TYPE a REJECT_VALUE umožňují toodefine řádků, kolik nebo jaké procento hello dat musí být v tabulce konečné hello.
Během procesu načítání Pokud je dosaženo hodnoty odmítněte hello, zatížení hello se nezdaří. Nejčastější příčinou Hello odmítnutých řádků je neshody definice schématu.
Pokud sloupec je nesprávně zadána hello schéma int, při hello data v souboru hello je řetězec, například každý řádek selže tooload.

Hello umístění určuje hello nejhornější adresář, který chcete tooread data z.
V takovém případě kdyby existovalo podadresářů /DimProduct/ PolyBase k importu všech hello dat v rámci hello podadresářů.

## <a name="load-hello-data"></a>Načtení dat hello
tooload dat z Azure Data Lake Store pomocí hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] příkaz. Načtení se funkce CTAS hello používá silného typu externí tabulky, které jste vytvořili.

Funkce CTAS vytvoří novou tabulku a naplní s hello výsledky příkazu select. Funkce CTAS definuje hello nové tabulky toohave hello stejnou sloupce a datové typy jako hello výsledky hello vyberte příkaz. Pokud vyberete všechny sloupce hello z externí tabulky, je nová tabulka hello repliku hello sloupce a typy dat v hello externí tabulky.

V tomto příkladu vytváříme zatřiďovací tabulku distribuované volat DimProduct z našich DimProduct_external externí tabulky.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>Optimalizace columnstore komprese
Ve výchozím nastavení ukládá SQL Data Warehouse tabulku hello jako clusterovaný index columnstore. Po dokončení zatížení, nemusí některé řádky dat hello komprimované do hello columnstore.  Je z různých důvodů, proč k tomu může dojít. Další, najdete v části toolearn [spravovat indexy columnstore][manage columnstore indexes].

toooptimize výkon dotazů a komprese columnstore po zatížení, znovu sestavit index columnstore toocompress pro hello tabulky tooforce hello všechny řádky hello.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

Další informace o údržbě indexů columnstore najdete v tématu hello [spravovat indexy columnstore] [ manage columnstore indexes] článku.

## <a name="optimize-statistics"></a>Optimalizace statistiky
Je nejlepší toocreate jednosloupcovou statistiku ihned po zatížení. Existuje několik možností pro statistiky. Například pokud vytvoříte jednosloupcovou statistiku pro každý sloupec může trvat dlouhou dobu toorebuild všechny statistické údaje o hello. Pokud víte, že některé sloupce nebudete toobe v predikátech dotazu, můžete přeskočit vytvoření statistiky pro tyto sloupce.

Pokud se rozhodnete toocreate jednosloupcovou statistiku pro každý sloupec každé tabulky, můžete použít ukázka kódu uložené procedury hello `prc_sqldw_create_stats` v hello [statistiky] [ statistics] článku.

Následující ukázka Hello je to dobrý výchozí bod pro vytvoření statistiky. Vytvoří jednosloupcovou statistiku pro každý sloupec v tabulce dimenze hello a na každém spojující sloupci tabulky faktů hello. Sloupců tabulky faktů tooother statistiky jeden nebo více sloupci můžete vždy přidat později.


## <a name="achievement-unlocked"></a>Dosažení odemčený!
Úspěšně jste načetli data do Azure SQL Data Warehouse. Skvělá práce!

##<a name="next-steps"></a>Další kroky
Načítání dat je hello první krok toodeveloping řešení datového skladu pomocí SQL Data Warehouse. Podívejte se na naše vývoj prostředky na [tabulky](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) a [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).


<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load hello full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
