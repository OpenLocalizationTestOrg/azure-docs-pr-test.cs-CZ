---
title: "Zatížení – Azure Data Lake Store k SQL Data Warehouse | Microsoft Docs"
description: "Naučte se používat k načtení dat z Azure Data Lake Store do Azure SQL Data Warehouse PolyBase externí tabulky."
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
ms.openlocfilehash: ab951c30aae0d4afdd931e245f25d4645bba1681
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a><span data-ttu-id="f2dc1-103">Načtení dat z Azure Data Lake Store do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f2dc1-103">Load data from Azure Data Lake Store into SQL Data Warehouse</span></span>
<span data-ttu-id="f2dc1-104">Tento dokument poskytuje všechny kroky, které je třeba načíst z Azure Data Lake Store (ADLS) svoje vlastní data do SQL Data Warehouse pomocí PolyBase.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-104">This document gives you all steps you  need to load your own data from Azure Data Lake Store (ADLS) into SQL Data Warehouse using PolyBase.</span></span>
<span data-ttu-id="f2dc1-105">Zatímco budete moci spouštět dotazy ad hoc přes data uložená v ADLS pomocí externí tabulky, jako osvědčený postup doporučujeme importu dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-105">While you are able to run adhoc queries over the data stored in ADLS using the External Tables, as a best practice we suggest importing the data into the SQL Data Warehouse.</span></span>
<span data-ttu-id="f2dc1-106">Odhad času: 10 minut za předpokladu, že splňujete požadavky nutné k dokončení.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-106">Time Estimate: 10 minutes assuming you have the prerequisites need to complete.</span></span>
<span data-ttu-id="f2dc1-107">V tomto kurzu se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="f2dc1-107">In this tutorial you will learn how to:</span></span>

1. <span data-ttu-id="f2dc1-108">Vytváření objektů externí databáze načíst z Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-108">Create External Database objects to load from Azure Data Lake Store.</span></span>
2. <span data-ttu-id="f2dc1-109">Připojení k adresáři Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-109">Connect to an Azure Data Lake Store Directory.</span></span>
3. <span data-ttu-id="f2dc1-110">Načtení dat do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-110">Load data into Azure SQL Data Warehouse.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f2dc1-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f2dc1-111">Before you begin</span></span>
<span data-ttu-id="f2dc1-112">Chcete-li spustit tento kurz, je třeba:</span><span class="sxs-lookup"><span data-stu-id="f2dc1-112">To run this tutorial, you need:</span></span>

* <span data-ttu-id="f2dc1-113">Azure Active Directory aplikace bude používat pro ověření Service-to-Service.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-113">Azure Active Directory Application to use for Service-to-Service authentication.</span></span> <span data-ttu-id="f2dc1-114">Pokud chcete vytvořit, postupujte podle [ověřování služby Active directory](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)</span><span class="sxs-lookup"><span data-stu-id="f2dc1-114">To create, follow [Active directory authentication](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)</span></span>

>[!NOTE] 
> <span data-ttu-id="f2dc1-115">Potřebujete ID klienta, klíč a hodnotu tokenu koncového bodu OAuth2.0 Active Directory aplikace pro připojení k vaší Azure Data Lake z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-115">You need the client ID, Key, and OAuth2.0 Token Endpoint Value of your Active Directory Application to connect to your Azure Data Lake from SQL Data Warehouse.</span></span> <span data-ttu-id="f2dc1-116">Podrobnosti o tom, jak získat tyto hodnoty jsou ve výše uvedený odkaz.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-116">Details for how to get these values are in the link above.</span></span>
><span data-ttu-id="f2dc1-117">Poznámka pro Azure Active Directory aplikace registrace pomocí ID aplikace jako ID klienta.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-117">Note for Azure Active Directory App Registration use the 'Application ID' as the Client ID.</span></span>

* <span data-ttu-id="f2dc1-118">SQL Server Management Studio nebo SQL Server Data Tools a stáhnout aplikaci SSMS připojení najdete v části [dotazu SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)</span><span class="sxs-lookup"><span data-stu-id="f2dc1-118">SQL Server Management Studio or SQL Server Data Tools, to download SSMS and connect see [Query SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)</span></span>

* <span data-ttu-id="f2dc1-119">Azure SQL Data Warehouse, chcete-li vytvořit jeden postupujte podle: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision</span><span class="sxs-lookup"><span data-stu-id="f2dc1-119">An Azure SQL Data Warehouse, to create one follow: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision</span></span>

* <span data-ttu-id="f2dc1-120">Azure Data Lake Store, s nebo bez šifrování povolené.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-120">An Azure Data Lake Store, with or without encryption enabled.</span></span> <span data-ttu-id="f2dc1-121">Chcete-li vytvořit jeden postupujte podle: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal</span><span class="sxs-lookup"><span data-stu-id="f2dc1-121">To create one follow: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal</span></span>




## <a name="configure-the-data-source"></a><span data-ttu-id="f2dc1-122">Nakonfigurujte zdroj dat</span><span class="sxs-lookup"><span data-stu-id="f2dc1-122">Configure the data source</span></span>
<span data-ttu-id="f2dc1-123">PolyBase používá externí objekty T-SQL zadat umístění a atributy externí data.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-123">PolyBase uses T-SQL external objects to define the location and attributes of the external data.</span></span> <span data-ttu-id="f2dc1-124">Externí objekty jsou uložené v SQL Data Warehouse a odkazovat na data, která je tý uložených externě.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-124">The external objects are stored in SQL Data Warehouse and reference the data th is stored externally.</span></span>


###  <a name="create-a-credential"></a><span data-ttu-id="f2dc1-125">Vytvoření přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="f2dc1-125">Create a credential</span></span>
<span data-ttu-id="f2dc1-126">Pro přístup k Azure Data Lake Store, musíte vytvořit hlavní klíč databáze pro zašifrování váš tajný klíč pověření použít v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-126">To access your Azure Data Lake Store, you will need to create a Database Master Key to encrypt your credential secret used in the next step.</span></span>
<span data-ttu-id="f2dc1-127">Pak vytvořte přihlašovací údaje databáze obor, který ukládá hlavní přihlašovací údaje služby nastavit v AAD.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-127">You then create a Database scoped credential, which stores the service principal credentials set up in AAD.</span></span> <span data-ttu-id="f2dc1-128">Pro ty z vás kdo PolyBase použili pro připojení k systému Windows Azure Storage Blobs syntaxe přihlašovacích údajů se liší.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-128">For those of you who have used PolyBase to connect to Windows Azure Storage Blobs, note that the credential syntax is different.</span></span>
<span data-ttu-id="f2dc1-129">Abyste mohli připojit k Azure Data Lake Store, musíte **první** vytvoření aplikace Azure Active Directory, vytvořte přístupový klíč a udělit aplikaci přístup k prostředku Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-129">To connect to Azure Data Lake Store, you must **first** create an Azure Active Directory Application, create an access key, and grant the application access to the Azure Data Lake resource.</span></span> <span data-ttu-id="f2dc1-130">Instrucitons provést tyto kroky jsou umístěné [zde](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).</span><span class="sxs-lookup"><span data-stu-id="f2dc1-130">Instrucitons to perform these steps are located [here](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).</span></span>

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://msdn.microsoft.com/en-us/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
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


### <a name="create-the-external-data-source"></a><span data-ttu-id="f2dc1-131">Vytvoření externího zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-131">Create the external data source</span></span>
<span data-ttu-id="f2dc1-132">Použít [vytvořit externí zdroj dat] [ CREATE EXTERNAL DATA SOURCE] příkazu umístění dat a typu dat úložiště.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-132">Use this [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] command to store the location of the data, and the type of data.</span></span>
<span data-ttu-id="f2dc1-133">Identifikátor URI ADL můžete najít v portálu Azure a www.portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-133">You can find the ADL URI in the Azure portal and www.portal.azure.com.</span></span>

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Store.
-- LOCATION: Provide Azure Data Lake accountname and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalakestore.net',
    CREDENTIAL = ADLCredential
);
```



## <a name="configure-data-format"></a><span data-ttu-id="f2dc1-134">Konfigurovat formát dat</span><span class="sxs-lookup"><span data-stu-id="f2dc1-134">Configure data format</span></span>
<span data-ttu-id="f2dc1-135">Pro import dat z ADLS, budete muset zadat formát externích souborů.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-135">To import the data from ADLS, you need to specify the external file format.</span></span> <span data-ttu-id="f2dc1-136">Tento příkaz má formát specifické možnosti popisují vaše data.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-136">This command has format-specific options to describe your data.</span></span>
<span data-ttu-id="f2dc1-137">Dole je příklad formát běžně používané souboru, který je oddělený kanálu textový soubor.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-137">Below is an example of a commonly used file format that is a pipe-delimited text file.</span></span>
<span data-ttu-id="f2dc1-138">Podívejte se na dokumentaci T-SQL pro úplný seznam [vytvořit EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT]</span><span class="sxs-lookup"><span data-stu-id="f2dc1-138">Look at our T-SQL documentation for a complete list of [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT]</span></span>

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
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

## <a name="create-the-external-tables"></a><span data-ttu-id="f2dc1-139">Vytvoření externí tabulky</span><span class="sxs-lookup"><span data-stu-id="f2dc1-139">Create the external tables</span></span>
<span data-ttu-id="f2dc1-140">Formát dat zdroje a soubor jste zadali, jste připraveni k vytvoření externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-140">Now that you have specified the data source and file format, you are ready to create the external tables.</span></span> <span data-ttu-id="f2dc1-141">Externí tabulky se, jak pracovat s externí data.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-141">External tables are how you interact with external data.</span></span> <span data-ttu-id="f2dc1-142">PolyBase používá rekurzivní directory traversal číst všechny soubory v všechny podadresáře zadaný v parametru umístění adresáře.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-142">PolyBase uses recursive directory traversal to read all files in all subdirectories of the directory specified in the location parameter.</span></span> <span data-ttu-id="f2dc1-143">Navíc následující příklad ukazuje, jak vytvořit objekt.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-143">Also, the following example shows how to create the object.</span></span> <span data-ttu-id="f2dc1-144">Budete muset přizpůsobit příkaz pracovat s daty, které máte v ADLS.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-144">You need to customize the statement to work with the data you have in ADLS.</span></span>

```sql
-- D: Create an External Table
-- LOCATION: Folder under the ADLS root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

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

## <a name="external-table-considerations"></a><span data-ttu-id="f2dc1-145">Aspekty externí tabulky</span><span class="sxs-lookup"><span data-stu-id="f2dc1-145">External Table Considerations</span></span>
<span data-ttu-id="f2dc1-146">Vytvoření externí tabulky je jednoduché, ale existují některé drobné odlišnosti, které musí být popsané.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-146">Creating an external table is easy, but there are some nuances that need to be discussed.</span></span>

<span data-ttu-id="f2dc1-147">Načítání dat pomocí funkce PolyBase je silného typu.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-147">Loading data with PolyBase is strongly typed.</span></span> <span data-ttu-id="f2dc1-148">To znamená, že každý řádek dat probíhá požity musí splňovat definice schématu tabulky.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-148">This means that each row of the data being ingested must satisfy the table schema definition.</span></span>
<span data-ttu-id="f2dc1-149">Pokud daný řádek neodpovídá definici schématu, řádek byl odmítnut z zatížení.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-149">If a given row does not match the schema definition, the row is rejected from the load.</span></span>

<span data-ttu-id="f2dc1-150">Možnosti REJECT_TYPE a REJECT_VALUE umožňují definovat, kolik řádků nebo jaké procento dat musí být v posledním tabulce.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-150">The REJECT_TYPE and REJECT_VALUE options allow you to define how many rows or what percentage of the data must be present in the final table.</span></span>
<span data-ttu-id="f2dc1-151">Během procesu načítání Pokud je dosaženo hodnoty odmítněte, zatížení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-151">During load, if the reject value is reached, the load fails.</span></span> <span data-ttu-id="f2dc1-152">Nejčastější příčinou odmítnutých řádků je neshody definice schématu.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-152">The most common cause of rejected rows is a schema definition mismatch.</span></span>
<span data-ttu-id="f2dc1-153">Například pokud sloupec je nesprávně zadána schéma int, když jsou data v souboru řetězec, každý řádek nebude možné načíst.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-153">For example, if a column is incorrectly given the schema of int when the data in the file is a string, every row will fail to load.</span></span>

<span data-ttu-id="f2dc1-154">Umístění určuje nejhornější adresáře, který chcete číst data z.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-154">The Location specifies the topmost directory that you want to read data from.</span></span>
<span data-ttu-id="f2dc1-155">V takovém případě pokud byly nějaké podadresářů /DimProduct/ PolyBase k importu všech dat v rámci podadresářů.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-155">In this case, if there were subdirectories under /DimProduct/ PolyBase would import all the data within the subdirectories.</span></span>

## <a name="load-the-data"></a><span data-ttu-id="f2dc1-156">Načtení dat</span><span class="sxs-lookup"><span data-stu-id="f2dc1-156">Load the data</span></span>
<span data-ttu-id="f2dc1-157">Načtení dat z Azure Data Lake Store pomocí [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] příkaz.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-157">To load data from Azure Data Lake Store use the [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="f2dc1-158">Načítání se funkce CTAS používá silného typu externí tabulky, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-158">Loading with CTAS uses the strongly typed external table you have created.</span></span>

<span data-ttu-id="f2dc1-159">Funkce CTAS vytvoří novou tabulku a naplní s výsledky příkazu select.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-159">CTAS creates a new table and populates it with the results of a select statement.</span></span> <span data-ttu-id="f2dc1-160">Funkce CTAS definuje novou tabulku tak, aby měl stejné sloupce a typy dat jako výsledky příkazu select.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-160">CTAS defines the new table to have the same columns and data types as the results of the select statement.</span></span> <span data-ttu-id="f2dc1-161">Pokud vyberete všechny sloupce z externí tabulky, je nová tabulka repliku sloupce a typy dat v externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-161">If you select all the columns from an external table, the new table is a replica of the columns and data types in the external table.</span></span>

<span data-ttu-id="f2dc1-162">V tomto příkladu vytváříme zatřiďovací tabulku distribuované volat DimProduct z našich DimProduct_external externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-162">In this example, we are creating a hash distributed table called DimProduct from our External Table DimProduct_external.</span></span>

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a><span data-ttu-id="f2dc1-163">Optimalizace columnstore komprese</span><span class="sxs-lookup"><span data-stu-id="f2dc1-163">Optimize columnstore compression</span></span>
<span data-ttu-id="f2dc1-164">Ve výchozím nastavení SQL Data Warehouse ukládá jako clusterovaný index columnstore v tabulce.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-164">By default, SQL Data Warehouse stores the table as a clustered columnstore index.</span></span> <span data-ttu-id="f2dc1-165">Po dokončení zatížení některé řádky dat nemusí být komprimovány do columnstore.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-165">After a load completes, some of the data rows might not be compressed into the columnstore.</span></span>  <span data-ttu-id="f2dc1-166">Je z různých důvodů, proč k tomu může dojít.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-166">There's a variety of reasons why this can happen.</span></span> <span data-ttu-id="f2dc1-167">Další informace najdete v tématu [spravovat indexy columnstore][manage columnstore indexes].</span><span class="sxs-lookup"><span data-stu-id="f2dc1-167">To learn more, see [manage columnstore indexes][manage columnstore indexes].</span></span>

<span data-ttu-id="f2dc1-168">Chcete-li optimalizovat výkon dotazů a komprese columnstore po zatížení, znovu sestavte vynutit index columnstore komprimovat všechny řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-168">To optimize query performance and columnstore compression after a load, rebuild the table to force the columnstore index to compress all the rows.</span></span>

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

<span data-ttu-id="f2dc1-169">Další informace o údržbě indexů columnstore, najdete v článku [spravovat indexy columnstore] [ manage columnstore indexes] článku.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-169">For more information on maintaining columnstore indexes, see the [manage columnstore indexes][manage columnstore indexes] article.</span></span>

## <a name="optimize-statistics"></a><span data-ttu-id="f2dc1-170">Optimalizace statistiky</span><span class="sxs-lookup"><span data-stu-id="f2dc1-170">Optimize statistics</span></span>
<span data-ttu-id="f2dc1-171">Je vhodné vytvořit jednosloupcovou statistiku okamžitě po zatížení.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-171">It is best to create single-column statistics immediately after a load.</span></span> <span data-ttu-id="f2dc1-172">Existuje několik možností pro statistiky.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-172">There are some choices for statistics.</span></span> <span data-ttu-id="f2dc1-173">Například pokud vytvoříte jednosloupcovou statistiku pro každý sloupec může trvat dlouhou dobu znovu vytvořit všechny statistiky.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-173">For example, if you create single-column statistics on every column it might take a long time to rebuild all the statistics.</span></span> <span data-ttu-id="f2dc1-174">Pokud víte, že některé sloupce nejsou má být v predikátech dotazu, můžete přeskočit vytvoření statistiky pro tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-174">If you know certain columns are not going to be in query predicates, you can skip creating statistics on those columns.</span></span>

<span data-ttu-id="f2dc1-175">Pokud se rozhodnete vytvořit jednosloupcovou statistiku pro každý sloupec každé tabulky, můžete použít ukázka kódu uložené procedury `prc_sqldw_create_stats` v [statistiky] [ statistics] článku.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-175">If you decide to create single-column statistics on every column of every table, you can use the stored procedure code sample `prc_sqldw_create_stats` in the [statistics][statistics] article.</span></span>

<span data-ttu-id="f2dc1-176">V následujícím příkladu je to dobrý výchozí bod pro vytvoření statistiky.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-176">The following example is a good starting point for creating statistics.</span></span> <span data-ttu-id="f2dc1-177">Vytvoří jednosloupcovou statistiku pro každý sloupec v tabulce dimenze a pro každý sloupec spojující v tabulkách faktů.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-177">It creates single-column statistics on each column in the dimension table, and on each joining column in the fact tables.</span></span> <span data-ttu-id="f2dc1-178">Vždy přidáním statistiky jeden nebo více sloupců do ostatních sloupců tabulky faktů později.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-178">You can always add single or multi-column statistics to other fact table columns later on.</span></span>


## <a name="achievement-unlocked"></a><span data-ttu-id="f2dc1-179">Dosažení odemčený!</span><span class="sxs-lookup"><span data-stu-id="f2dc1-179">Achievement unlocked!</span></span>
<span data-ttu-id="f2dc1-180">Úspěšně jste načetli data do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-180">You have successfully loaded data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="f2dc1-181">Skvělá práce!</span><span class="sxs-lookup"><span data-stu-id="f2dc1-181">Great job!</span></span>

##<a name="next-steps"></a><span data-ttu-id="f2dc1-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f2dc1-182">Next Steps</span></span>
<span data-ttu-id="f2dc1-183">Načítání dat je prvním krokem k vývoji řešení datového skladu pomocí SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f2dc1-183">Loading data is the first step to developing a data warehouse solution using SQL Data Warehouse.</span></span> <span data-ttu-id="f2dc1-184">Podívejte se na naše vývoj prostředky na [tabulky](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) a [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).</span><span class="sxs-lookup"><span data-stu-id="f2dc1-184">Check out our development resources on [Tables](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) and [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).</span></span>


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
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
