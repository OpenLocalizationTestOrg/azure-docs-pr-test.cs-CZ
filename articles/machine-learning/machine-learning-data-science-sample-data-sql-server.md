---
title: "aaaSample dat v systému SQL Server na platformě Azure | Microsoft Docs"
description: "Ukázková Data v systému SQL Server v Azure"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: dc7f9529c771f6deb633775557e64a04b774f5b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="bc321-103"><a name="heading"></a>Ukázková data v systému SQL Server v Azure</span><span class="sxs-lookup"><span data-stu-id="bc321-103"><a name="heading"></a>Sample data in SQL Server on Azure</span></span>
<span data-ttu-id="bc321-104">Tento dokument ukazuje, jak toosample data uložená v systému SQL Server na platformě Azure pomocí SQL nebo hello programovací jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="bc321-104">This document shows how toosample data stored in SQL Server on Azure using either SQL or hello Python programming language.</span></span> <span data-ttu-id="bc321-105">Také ukazuje, jak toomove vzorků dat do Azure Machine Learning uložením souboru tooa, pak ho nahrát tooan objektů blob v Azure a pak ho čtení do Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="bc321-105">It also shows how toomove sampled data into Azure Machine Learning by saving it tooa file, uploading it tooan Azure blob, and then reading it into Azure Machine Learning Studio.</span></span>

<span data-ttu-id="bc321-106">vzorkování Python Hello používá hello [pyodbc](https://code.google.com/p/pyodbc/) rozhraní ODBC knihovny tooconnect tooSQL Server v Azure a hello [Pandas](http://pandas.pydata.org/) knihovny toodo hello vzorkování.</span><span class="sxs-lookup"><span data-stu-id="bc321-106">hello Python sampling uses hello [pyodbc](https://code.google.com/p/pyodbc/) ODBC library tooconnect tooSQL Server on Azure and hello [Pandas](http://pandas.pydata.org/) library toodo hello sampling.</span></span>

> [!NOTE]
> <span data-ttu-id="bc321-107">Hello ukázkový SQL kód v tomto dokumentu se předpokládá, že hello dat je v systému SQL Server na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="bc321-107">hello sample SQL code in this document assumes that hello data is in a SQL Server on Azure.</span></span> <span data-ttu-id="bc321-108">Pokud není, naleznete příliš[přesunout data tooSQL Server na platformě Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) tématu pokyny, jak toomove vaše data tooSQL Server na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="bc321-108">If it is not, please refer too[Move data tooSQL Server on Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) topic for instructions on how toomove your data tooSQL Server on Azure.</span></span>
> 
> 

<span data-ttu-id="bc321-109">Následující Hello **nabídky** odkazy tootopics, které popisují, jak toosample data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="bc321-109">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="bc321-110">**Proč ukázková data?**</span><span class="sxs-lookup"><span data-stu-id="bc321-110">**Why sample your data?**</span></span>
<span data-ttu-id="bc321-111">Pokud datovou sadu hello plánování tooanalyze velká, je obvykle vhodné hello toodown ukázková data tooreduce ho tooa menší, ale reprezentativní a lepší správu bitlockeru velikost.</span><span class="sxs-lookup"><span data-stu-id="bc321-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="bc321-112">To usnadňuje pochopení dat, zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="bc321-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="bc321-113">Jeho role v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) je rychlé vytváření prototypů tooenable hello zpracování dat funkcí a modelů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="bc321-113">Its role in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="bc321-114">Tato úloha vzorkování je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="bc321-114">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <span data-ttu-id="bc321-115"><a name="SQL"></a>Pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="bc321-115"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="bc321-116">Tato část popisuje několik metod, pomocí SQL tooperform jednoduché náhodné vzorkování pro hello data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="bc321-116">This section describes several methods using SQL tooperform simple random sampling against hello data in hello database.</span></span> <span data-ttu-id="bc321-117">Vyberte metodu na základě vaší velikost dat a jeho distribuci.</span><span class="sxs-lookup"><span data-stu-id="bc321-117">Please choose a method based on your data size and its distribution.</span></span>

<span data-ttu-id="bc321-118">Hello dvě položky ukazují, jak toouse newid v systému SQL Server tooperform hello vzorkování.</span><span class="sxs-lookup"><span data-stu-id="bc321-118">hello two items below show how toouse newid in SQL Server tooperform hello sampling.</span></span> <span data-ttu-id="bc321-119">Hello metoda zvolíte, závisí na jak náhodné chcete toobe ukázka hello (hodnota pk_id v hello následující ukázkový kód je toobe automaticky vygeneruje primární klíč).</span><span class="sxs-lookup"><span data-stu-id="bc321-119">hello method you choose depends on how random you want hello sample toobe (pk_id in hello sample code below is assumed toobe an auto-generated primary key).</span></span>

1. <span data-ttu-id="bc321-120">Méně přísná náhodného vzorku</span><span class="sxs-lookup"><span data-stu-id="bc321-120">Less strict random sample</span></span>
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. <span data-ttu-id="bc321-121">Více náhodného vzorku</span><span class="sxs-lookup"><span data-stu-id="bc321-121">More random sample</span></span> 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

<span data-ttu-id="bc321-122">Klauzule Tablesample může být použit pro vzorkování, stejně jako ukázáno níže.</span><span class="sxs-lookup"><span data-stu-id="bc321-122">Tablesample can be used for sampling as well as demonstrated below.</span></span> <span data-ttu-id="bc321-123">To může být lepším řešením Pokud (za předpokladu, že data na různých stránkách není korelační) má velká velikost dat a pro toocomplete hello dotazu v přiměřené době.</span><span class="sxs-lookup"><span data-stu-id="bc321-123">This may be a better approach if your data size is large (assuming that data on different pages is not correlated) and for hello query toocomplete in a reasonable time.</span></span>

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> <span data-ttu-id="bc321-124">Vám umožní zkoumat a generovat funkce z této jen Vzorkovaná data ukládáním do nové tabulky</span><span class="sxs-lookup"><span data-stu-id="bc321-124">You can explore and generate features from this sampled data by storing it in a new table</span></span>
> 
> 

### <span data-ttu-id="bc321-125"><a name="sql-aml"></a>Připojení tooAzure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="bc321-125"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="bc321-126">Ukázkové dotazy hello výše můžete použít přímo v Azure Machine Learning hello [importovat Data] [ import-data] modulu hello toodown ukázková data na hello fyzicky dostavili a převeďte ho do experimentu Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="bc321-126">You can directly  use hello sample queries above in hello Azure Machine Learning [Import Data][import-data] module toodown-sample hello data on hello fly and bring it into an Azure Machine Learning experiment.</span></span> <span data-ttu-id="bc321-127">Snímek obrazovky s použitím hello čtečky modulu tooread hello vzorků dat je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="bc321-127">A screen shot of using hello reader module tooread hello sampled data is shown below:</span></span>

![Čtečka sql][1]

## <span data-ttu-id="bc321-129"><a name="python"></a>Pomocí programovacího jazyka Python hello</span><span class="sxs-lookup"><span data-stu-id="bc321-129"><a name="python"></a>Using hello Python programming language</span></span>
<span data-ttu-id="bc321-130">V této části ukazuje, jak pomocí hello [pyodbc knihovny](https://code.google.com/p/pyodbc/) tooestablish ODBC připojení databáze serveru SQL tooa v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="bc321-130">This section demonstrates using hello [pyodbc library](https://code.google.com/p/pyodbc/) tooestablish an ODBC connect tooa SQL server database in Python.</span></span> <span data-ttu-id="bc321-131">připojovací řetězec databáze Hello je následující: (nahraďte servername, dbname, uživatelské jméno a heslo s konfigurací):</span><span class="sxs-lookup"><span data-stu-id="bc321-131">hello database connection string is as follows: (replace servername, dbname, username and password with your configuration):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="bc321-132">Hello [Pandas](http://pandas.pydata.org/) knihovna v Pythonu obsahuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python.</span><span class="sxs-lookup"><span data-stu-id="bc321-132">hello [Pandas](http://pandas.pydata.org/) library in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="bc321-133">Následující kód Hello načte ukázku 0,1 % hello dat z tabulky v databázi Azure SQL do Pandas dat:</span><span class="sxs-lookup"><span data-stu-id="bc321-133">hello code below reads a 0.1% sample of hello data from a table in Azure SQL database into a Pandas data :</span></span>

    import pandas as pd

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

<span data-ttu-id="bc321-134">Teď můžete pracovat s daty hello vzorkovat hello Pandas datové rámce.</span><span class="sxs-lookup"><span data-stu-id="bc321-134">You can now work with hello sampled data in hello Pandas data frame.</span></span> 

### <span data-ttu-id="bc321-135"><a name="python-aml"></a>Připojení tooAzure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="bc321-135"><a name="python-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="bc321-136">Můžete použít následující ukázkový kód toosave hello nižší vzorků dat tooa soubor hello a nahrajte ho tooan objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="bc321-136">You can use hello following sample code toosave hello down-sampled data tooa file and upload it tooan Azure blob.</span></span> <span data-ttu-id="bc321-137">Hello hello objektu BLOB může být přímo číst data do experimentu Azure Machine Learning pomocí hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="bc321-137">hello data in hello blob can be directly read into an Azure Machine Learning Experiment using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="bc321-138">Hello kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="bc321-138">hello steps are as follows:</span></span> 

1. <span data-ttu-id="bc321-139">Zápis hello pandas dat rámce tooa místního souboru</span><span class="sxs-lookup"><span data-stu-id="bc321-139">Write hello pandas data frame tooa local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="bc321-140">Nahrát objekt blob tooAzure místního souboru</span><span class="sxs-lookup"><span data-stu-id="bc321-140">Upload local file tooAzure blob</span></span>
   
        from azure.storage import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. <span data-ttu-id="bc321-141">Čtení dat z Azure blob pomocí Azure Machine Learning [importovat Data] [ import-data] modulu, jak je znázorněno níže okamžitými hello obrazovky výsledky:</span><span class="sxs-lookup"><span data-stu-id="bc321-141">Read data from Azure blob using Azure Machine Learning [Import Data][import-data] module as shown in hello screen grab below:</span></span>

![Čtečka objektů blob][2]

## <a name="hello-team-data-science-process-in-action-example"></a><span data-ttu-id="bc321-143">v příkladu akce Hello procesu Team dat vědecké účely</span><span class="sxs-lookup"><span data-stu-id="bc321-143">hello Team Data Science Process in Action example</span></span>
<span data-ttu-id="bc321-144">Příklad začátku do konce návod hello proces vědecké účely Team dat pomocí veřejné datové sady, najdete v tématu [proces vědecké účely Team dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="bc321-144">For an end-to-end walkthrough example of hello Team Data Science Process a using a public dataset, see [Team Data Science Process in Action: using SQL Sever](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
