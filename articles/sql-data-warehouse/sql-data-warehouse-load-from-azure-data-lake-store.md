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
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a><span data-ttu-id="f3464-103">Načtení dat z Azure Data Lake Store do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f3464-103">Load data from Azure Data Lake Store into SQL Data Warehouse</span></span>
<span data-ttu-id="f3464-104">Tento dokument poskytuje všechny kroky nutné svoje vlastní data z Azure Data Lake Store (ADLS) pro tooload do SQL Data Warehouse pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="f3464-104">This document gives you all steps you  need tooload your own data from Azure Data Lake Store (ADLS) into SQL Data Warehouse using PolyBase.</span></span>
<span data-ttu-id="f3464-105">Zatímco přes hello data uložená v ADLS pomocí hello externí tabulky se může toorun ad hoc dotazy jako osvědčený postup doporučujeme importování dat hello do hello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f3464-105">While you are able toorun adhoc queries over hello data stored in ADLS using hello External Tables, as a best practice we suggest importing hello data into hello SQL Data Warehouse.</span></span>
<span data-ttu-id="f3464-106">Odhad času: 10 minut za předpokladu, že splňujete požadavky hello potřebovat toocomplete.</span><span class="sxs-lookup"><span data-stu-id="f3464-106">Time Estimate: 10 minutes assuming you have hello prerequisites need toocomplete.</span></span>
<span data-ttu-id="f3464-107">V tomto kurzu se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="f3464-107">In this tutorial you will learn how to:</span></span>

1. <span data-ttu-id="f3464-108">Vytvořte objekty tooload externí databáze z Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f3464-108">Create External Database objects tooload from Azure Data Lake Store.</span></span>
2. <span data-ttu-id="f3464-109">Připojte tooan Azure Data Lake Store Directory.</span><span class="sxs-lookup"><span data-stu-id="f3464-109">Connect tooan Azure Data Lake Store Directory.</span></span>
3. <span data-ttu-id="f3464-110">Načtení dat do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f3464-110">Load data into Azure SQL Data Warehouse.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f3464-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f3464-111">Before you begin</span></span>
<span data-ttu-id="f3464-112">toorun tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="f3464-112">toorun this tutorial, you need:</span></span>

* <span data-ttu-id="f3464-113">Pro ověřování do služby Azure toouse aplikace Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f3464-113">Azure Active Directory Application toouse for Service-to-Service authentication.</span></span> <span data-ttu-id="f3464-114">toocreate, postupujte podle [ověřování služby Active directory](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)</span><span class="sxs-lookup"><span data-stu-id="f3464-114">toocreate, follow [Active directory authentication](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)</span></span>

>[!NOTE] 
> <span data-ttu-id="f3464-115">Potřebujete ID klienta hello, klíč a hodnotu OAuth2.0 tokenu koncového bodu z vaší aplikace Active Directory tooconnect tooyour Azure Data Lake z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f3464-115">You need hello client ID, Key, and OAuth2.0 Token Endpoint Value of your Active Directory Application tooconnect tooyour Azure Data Lake from SQL Data Warehouse.</span></span> <span data-ttu-id="f3464-116">Podrobnosti o tom, jak tooget tyto hodnoty jsou v hello výše uvedený odkaz.</span><span class="sxs-lookup"><span data-stu-id="f3464-116">Details for how tooget these values are in hello link above.</span></span>
><span data-ttu-id="f3464-117">Poznámka pro registrace Azure Active Directory aplikace pomocí hello "ID aplikace, jako hello ID klienta.</span><span class="sxs-lookup"><span data-stu-id="f3464-117">Note for Azure Active Directory App Registration use hello 'Application ID' as hello Client ID.</span></span>

* <span data-ttu-id="f3464-118">SQL Server Management Studio nebo SQL Server Data Tools, toodownload SSMS a připojte najdete [dotazu SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)</span><span class="sxs-lookup"><span data-stu-id="f3464-118">SQL Server Management Studio or SQL Server Data Tools, toodownload SSMS and connect see [Query SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)</span></span>

* <span data-ttu-id="f3464-119">Postupujte podle toocreate jeden Azure SQL Data Warehouse: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision</span><span class="sxs-lookup"><span data-stu-id="f3464-119">An Azure SQL Data Warehouse, toocreate one follow: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision</span></span>

* <span data-ttu-id="f3464-120">Azure Data Lake Store, s nebo bez šifrování povolené.</span><span class="sxs-lookup"><span data-stu-id="f3464-120">An Azure Data Lake Store, with or without encryption enabled.</span></span> <span data-ttu-id="f3464-121">postupujte podle jednoho toocreate: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal</span><span class="sxs-lookup"><span data-stu-id="f3464-121">toocreate one follow: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal</span></span>




## <a name="configure-hello-data-source"></a><span data-ttu-id="f3464-122">Konfigurace zdroje dat hello</span><span class="sxs-lookup"><span data-stu-id="f3464-122">Configure hello data source</span></span>
<span data-ttu-id="f3464-123">PolyBase používá T-SQL externí objekty toodefine hello umístění a atributy externích dat hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-123">PolyBase uses T-SQL external objects toodefine hello location and attributes of hello external data.</span></span> <span data-ttu-id="f3464-124">Hello externí objekty jsou uloženy v SQL Data Warehouse a referenční dokumentace hello data, která je tý uložených externě.</span><span class="sxs-lookup"><span data-stu-id="f3464-124">hello external objects are stored in SQL Data Warehouse and reference hello data th is stored externally.</span></span>


###  <a name="create-a-credential"></a><span data-ttu-id="f3464-125">Vytvoření přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="f3464-125">Create a credential</span></span>
<span data-ttu-id="f3464-126">tooaccess vaše Azure Data Lake úložiště, budete potřebovat toocreate hlavní klíč databáze tooencrypt váš tajný klíč pověření použít v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-126">tooaccess your Azure Data Lake Store, you will need toocreate a Database Master Key tooencrypt your credential secret used in hello next step.</span></span>
<span data-ttu-id="f3464-127">Pak vytvořte přihlašovací údaje databáze obor, který ukládá hello hlavní přihlašovací údaje služby nastavit v AAD.</span><span class="sxs-lookup"><span data-stu-id="f3464-127">You then create a Database scoped credential, which stores hello service principal credentials set up in AAD.</span></span> <span data-ttu-id="f3464-128">Syntaxe pro těch, které jste, který jste použili PolyBase tooconnect tooWindows objektů BLOB služby Azure Storage, Všimněte si, že pověření hello se liší.</span><span class="sxs-lookup"><span data-stu-id="f3464-128">For those of you who have used PolyBase tooconnect tooWindows Azure Storage Blobs, note that hello credential syntax is different.</span></span>
<span data-ttu-id="f3464-129">tooconnect tooAzure Data Lake Store, musíte **první** vytvoření aplikace Azure Active Directory, vytvořte přístupový klíč a udělte hello aplikace přístup toohello Azure Data Lake prostředek.</span><span class="sxs-lookup"><span data-stu-id="f3464-129">tooconnect tooAzure Data Lake Store, you must **first** create an Azure Active Directory Application, create an access key, and grant hello application access toohello Azure Data Lake resource.</span></span> <span data-ttu-id="f3464-130">Instrucitons tooperform tyto kroky jsou umístěné [zde](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).</span><span class="sxs-lookup"><span data-stu-id="f3464-130">Instrucitons tooperform these steps are located [here](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).</span></span>

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


### <a name="create-hello-external-data-source"></a><span data-ttu-id="f3464-131">Vytvoření hello externí zdroj dat</span><span class="sxs-lookup"><span data-stu-id="f3464-131">Create hello external data source</span></span>
<span data-ttu-id="f3464-132">Použít [vytvořit externí zdroj dat] [ CREATE EXTERNAL DATA SOURCE] příkaz toostore hello umístění hello dat a datový typ hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-132">Use this [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] command toostore hello location of hello data, and hello type of data.</span></span>
<span data-ttu-id="f3464-133">Hello ADL URI najdete v hello portál Azure a www.portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="f3464-133">You can find hello ADL URI in hello Azure portal and www.portal.azure.com.</span></span>

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



## <a name="configure-data-format"></a><span data-ttu-id="f3464-134">Konfigurovat formát dat</span><span class="sxs-lookup"><span data-stu-id="f3464-134">Configure data format</span></span>
<span data-ttu-id="f3464-135">tooimport hello data z ADLS, musíte formát externích souborů toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-135">tooimport hello data from ADLS, you need toospecify hello external file format.</span></span> <span data-ttu-id="f3464-136">Tento příkaz má toodescribe možnosti specifické pro formát data.</span><span class="sxs-lookup"><span data-stu-id="f3464-136">This command has format-specific options toodescribe your data.</span></span>
<span data-ttu-id="f3464-137">Dole je příklad formát běžně používané souboru, který je oddělený kanálu textový soubor.</span><span class="sxs-lookup"><span data-stu-id="f3464-137">Below is an example of a commonly used file format that is a pipe-delimited text file.</span></span>
<span data-ttu-id="f3464-138">Podívejte se na dokumentaci T-SQL pro úplný seznam [vytvořit EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT]</span><span class="sxs-lookup"><span data-stu-id="f3464-138">Look at our T-SQL documentation for a complete list of [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT]</span></span>

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

## <a name="create-hello-external-tables"></a><span data-ttu-id="f3464-139">Vytvoření hello externí tabulky</span><span class="sxs-lookup"><span data-stu-id="f3464-139">Create hello external tables</span></span>
<span data-ttu-id="f3464-140">Teď, když jste zadali hello datového zdroje a formát souboru, jste připravené toocreate hello externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="f3464-140">Now that you have specified hello data source and file format, you are ready toocreate hello external tables.</span></span> <span data-ttu-id="f3464-141">Externí tabulky se, jak pracovat s externí data.</span><span class="sxs-lookup"><span data-stu-id="f3464-141">External tables are how you interact with external data.</span></span> <span data-ttu-id="f3464-142">PolyBase používá rekurzivní directory traversal tooread všechny soubory v všechny podadresáře adresáře hello zadaný v parametru umístění hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-142">PolyBase uses recursive directory traversal tooread all files in all subdirectories of hello directory specified in hello location parameter.</span></span> <span data-ttu-id="f3464-143">Navíc hello následující příklad ukazuje, jak toocreate hello objektu.</span><span class="sxs-lookup"><span data-stu-id="f3464-143">Also, hello following example shows how toocreate hello object.</span></span> <span data-ttu-id="f3464-144">Je nutné toocustomize hello příkaz toowork hello daty, které máte v ADLS.</span><span class="sxs-lookup"><span data-stu-id="f3464-144">You need toocustomize hello statement toowork with hello data you have in ADLS.</span></span>

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

## <a name="external-table-considerations"></a><span data-ttu-id="f3464-145">Aspekty externí tabulky</span><span class="sxs-lookup"><span data-stu-id="f3464-145">External Table Considerations</span></span>
<span data-ttu-id="f3464-146">Vytvoření externí tabulky je jednoduché, ale existují některé drobné odlišnosti, které je třeba toobe popsané.</span><span class="sxs-lookup"><span data-stu-id="f3464-146">Creating an external table is easy, but there are some nuances that need toobe discussed.</span></span>

<span data-ttu-id="f3464-147">Načítání dat pomocí funkce PolyBase je silného typu.</span><span class="sxs-lookup"><span data-stu-id="f3464-147">Loading data with PolyBase is strongly typed.</span></span> <span data-ttu-id="f3464-148">To znamená, že každý řádek dat hello se požity musí splňovat definice schématu tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-148">This means that each row of hello data being ingested must satisfy hello table schema definition.</span></span>
<span data-ttu-id="f3464-149">Pokud daný řádek neodpovídá definice schématu hello, hello řádek byl odmítnut z hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="f3464-149">If a given row does not match hello schema definition, hello row is rejected from hello load.</span></span>

<span data-ttu-id="f3464-150">Hello možnosti REJECT_TYPE a REJECT_VALUE umožňují toodefine řádků, kolik nebo jaké procento hello dat musí být v tabulce konečné hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-150">hello REJECT_TYPE and REJECT_VALUE options allow you toodefine how many rows or what percentage of hello data must be present in hello final table.</span></span>
<span data-ttu-id="f3464-151">Během procesu načítání Pokud je dosaženo hodnoty odmítněte hello, zatížení hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="f3464-151">During load, if hello reject value is reached, hello load fails.</span></span> <span data-ttu-id="f3464-152">Nejčastější příčinou Hello odmítnutých řádků je neshody definice schématu.</span><span class="sxs-lookup"><span data-stu-id="f3464-152">hello most common cause of rejected rows is a schema definition mismatch.</span></span>
<span data-ttu-id="f3464-153">Pokud sloupec je nesprávně zadána hello schéma int, při hello data v souboru hello je řetězec, například každý řádek selže tooload.</span><span class="sxs-lookup"><span data-stu-id="f3464-153">For example, if a column is incorrectly given hello schema of int when hello data in hello file is a string, every row will fail tooload.</span></span>

<span data-ttu-id="f3464-154">Hello umístění určuje hello nejhornější adresář, který chcete tooread data z.</span><span class="sxs-lookup"><span data-stu-id="f3464-154">hello Location specifies hello topmost directory that you want tooread data from.</span></span>
<span data-ttu-id="f3464-155">V takovém případě kdyby existovalo podadresářů /DimProduct/ PolyBase k importu všech hello dat v rámci hello podadresářů.</span><span class="sxs-lookup"><span data-stu-id="f3464-155">In this case, if there were subdirectories under /DimProduct/ PolyBase would import all hello data within hello subdirectories.</span></span>

## <a name="load-hello-data"></a><span data-ttu-id="f3464-156">Načtení dat hello</span><span class="sxs-lookup"><span data-stu-id="f3464-156">Load hello data</span></span>
<span data-ttu-id="f3464-157">tooload dat z Azure Data Lake Store pomocí hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] příkaz.</span><span class="sxs-lookup"><span data-stu-id="f3464-157">tooload data from Azure Data Lake Store use hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="f3464-158">Načtení se funkce CTAS hello používá silného typu externí tabulky, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f3464-158">Loading with CTAS uses hello strongly typed external table you have created.</span></span>

<span data-ttu-id="f3464-159">Funkce CTAS vytvoří novou tabulku a naplní s hello výsledky příkazu select.</span><span class="sxs-lookup"><span data-stu-id="f3464-159">CTAS creates a new table and populates it with hello results of a select statement.</span></span> <span data-ttu-id="f3464-160">Funkce CTAS definuje hello nové tabulky toohave hello stejnou sloupce a datové typy jako hello výsledky hello vyberte příkaz.</span><span class="sxs-lookup"><span data-stu-id="f3464-160">CTAS defines hello new table toohave hello same columns and data types as hello results of hello select statement.</span></span> <span data-ttu-id="f3464-161">Pokud vyberete všechny sloupce hello z externí tabulky, je nová tabulka hello repliku hello sloupce a typy dat v hello externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="f3464-161">If you select all hello columns from an external table, hello new table is a replica of hello columns and data types in hello external table.</span></span>

<span data-ttu-id="f3464-162">V tomto příkladu vytváříme zatřiďovací tabulku distribuované volat DimProduct z našich DimProduct_external externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="f3464-162">In this example, we are creating a hash distributed table called DimProduct from our External Table DimProduct_external.</span></span>

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a><span data-ttu-id="f3464-163">Optimalizace columnstore komprese</span><span class="sxs-lookup"><span data-stu-id="f3464-163">Optimize columnstore compression</span></span>
<span data-ttu-id="f3464-164">Ve výchozím nastavení ukládá SQL Data Warehouse tabulku hello jako clusterovaný index columnstore.</span><span class="sxs-lookup"><span data-stu-id="f3464-164">By default, SQL Data Warehouse stores hello table as a clustered columnstore index.</span></span> <span data-ttu-id="f3464-165">Po dokončení zatížení, nemusí některé řádky dat hello komprimované do hello columnstore.</span><span class="sxs-lookup"><span data-stu-id="f3464-165">After a load completes, some of hello data rows might not be compressed into hello columnstore.</span></span>  <span data-ttu-id="f3464-166">Je z různých důvodů, proč k tomu může dojít.</span><span class="sxs-lookup"><span data-stu-id="f3464-166">There's a variety of reasons why this can happen.</span></span> <span data-ttu-id="f3464-167">Další, najdete v části toolearn [spravovat indexy columnstore][manage columnstore indexes].</span><span class="sxs-lookup"><span data-stu-id="f3464-167">toolearn more, see [manage columnstore indexes][manage columnstore indexes].</span></span>

<span data-ttu-id="f3464-168">toooptimize výkon dotazů a komprese columnstore po zatížení, znovu sestavit index columnstore toocompress pro hello tabulky tooforce hello všechny řádky hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-168">toooptimize query performance and columnstore compression after a load, rebuild hello table tooforce hello columnstore index toocompress all hello rows.</span></span>

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

<span data-ttu-id="f3464-169">Další informace o údržbě indexů columnstore najdete v tématu hello [spravovat indexy columnstore] [ manage columnstore indexes] článku.</span><span class="sxs-lookup"><span data-stu-id="f3464-169">For more information on maintaining columnstore indexes, see hello [manage columnstore indexes][manage columnstore indexes] article.</span></span>

## <a name="optimize-statistics"></a><span data-ttu-id="f3464-170">Optimalizace statistiky</span><span class="sxs-lookup"><span data-stu-id="f3464-170">Optimize statistics</span></span>
<span data-ttu-id="f3464-171">Je nejlepší toocreate jednosloupcovou statistiku ihned po zatížení.</span><span class="sxs-lookup"><span data-stu-id="f3464-171">It is best toocreate single-column statistics immediately after a load.</span></span> <span data-ttu-id="f3464-172">Existuje několik možností pro statistiky.</span><span class="sxs-lookup"><span data-stu-id="f3464-172">There are some choices for statistics.</span></span> <span data-ttu-id="f3464-173">Například pokud vytvoříte jednosloupcovou statistiku pro každý sloupec může trvat dlouhou dobu toorebuild všechny statistické údaje o hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-173">For example, if you create single-column statistics on every column it might take a long time toorebuild all hello statistics.</span></span> <span data-ttu-id="f3464-174">Pokud víte, že některé sloupce nebudete toobe v predikátech dotazu, můžete přeskočit vytvoření statistiky pro tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="f3464-174">If you know certain columns are not going toobe in query predicates, you can skip creating statistics on those columns.</span></span>

<span data-ttu-id="f3464-175">Pokud se rozhodnete toocreate jednosloupcovou statistiku pro každý sloupec každé tabulky, můžete použít ukázka kódu uložené procedury hello `prc_sqldw_create_stats` v hello [statistiky] [ statistics] článku.</span><span class="sxs-lookup"><span data-stu-id="f3464-175">If you decide toocreate single-column statistics on every column of every table, you can use hello stored procedure code sample `prc_sqldw_create_stats` in hello [statistics][statistics] article.</span></span>

<span data-ttu-id="f3464-176">Následující ukázka Hello je to dobrý výchozí bod pro vytvoření statistiky.</span><span class="sxs-lookup"><span data-stu-id="f3464-176">hello following example is a good starting point for creating statistics.</span></span> <span data-ttu-id="f3464-177">Vytvoří jednosloupcovou statistiku pro každý sloupec v tabulce dimenze hello a na každém spojující sloupci tabulky faktů hello.</span><span class="sxs-lookup"><span data-stu-id="f3464-177">It creates single-column statistics on each column in hello dimension table, and on each joining column in hello fact tables.</span></span> <span data-ttu-id="f3464-178">Sloupců tabulky faktů tooother statistiky jeden nebo více sloupci můžete vždy přidat později.</span><span class="sxs-lookup"><span data-stu-id="f3464-178">You can always add single or multi-column statistics tooother fact table columns later on.</span></span>


## <a name="achievement-unlocked"></a><span data-ttu-id="f3464-179">Dosažení odemčený!</span><span class="sxs-lookup"><span data-stu-id="f3464-179">Achievement unlocked!</span></span>
<span data-ttu-id="f3464-180">Úspěšně jste načetli data do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f3464-180">You have successfully loaded data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="f3464-181">Skvělá práce!</span><span class="sxs-lookup"><span data-stu-id="f3464-181">Great job!</span></span>

##<a name="next-steps"></a><span data-ttu-id="f3464-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3464-182">Next Steps</span></span>
<span data-ttu-id="f3464-183">Načítání dat je hello první krok toodeveloping řešení datového skladu pomocí SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f3464-183">Loading data is hello first step toodeveloping a data warehouse solution using SQL Data Warehouse.</span></span> <span data-ttu-id="f3464-184">Podívejte se na naše vývoj prostředky na [tabulky](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) a [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).</span><span class="sxs-lookup"><span data-stu-id="f3464-184">Check out our development resources on [Tables](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) and [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).</span></span>


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
