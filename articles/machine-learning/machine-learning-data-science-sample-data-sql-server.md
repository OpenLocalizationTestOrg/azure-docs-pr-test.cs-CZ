---
title: "Ukázková Data v systému SQL Server na platformě Azure | Microsoft Docs"
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
ms.openlocfilehash: 1bdcc7175dac325de1144d805e977264524b3fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="4a038-103"><a name="heading"></a>Ukázková data v systému SQL Server v Azure</span><span class="sxs-lookup"><span data-stu-id="4a038-103"><a name="heading"></a>Sample data in SQL Server on Azure</span></span>
<span data-ttu-id="4a038-104">Tento dokument ukazuje, jak ukázková data uložená v systému SQL Server na platformě Azure pomocí SQL nebo programovací jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="4a038-104">This document shows how to sample data stored in SQL Server on Azure using either SQL or the Python programming language.</span></span> <span data-ttu-id="4a038-105">Také ukazuje, jak přesunout jen Vzorkovaná data do Azure Machine Learning ukládání do souboru, odesílání do objektu blob Azure a pak ho čtení do Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="4a038-105">It also shows how to move sampled data into Azure Machine Learning by saving it to a file, uploading it to an Azure blob, and then reading it into Azure Machine Learning Studio.</span></span>

<span data-ttu-id="4a038-106">Používá Python vzorkování [pyodbc](https://code.google.com/p/pyodbc/) ODBC – knihovna pro připojení k systému SQL Server na platformě Azure a [Pandas](http://pandas.pydata.org/) knihovnu, která má odběr vzorků.</span><span class="sxs-lookup"><span data-stu-id="4a038-106">The Python sampling uses the [pyodbc](https://code.google.com/p/pyodbc/) ODBC library to connect to SQL Server on Azure and the [Pandas](http://pandas.pydata.org/) library to do the sampling.</span></span>

> [!NOTE]
> <span data-ttu-id="4a038-107">Ukázkový kód SQL v tomto dokumentu se předpokládá, že data jsou v systému SQL Server na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="4a038-107">The sample SQL code in this document assumes that the data is in a SQL Server on Azure.</span></span> <span data-ttu-id="4a038-108">Pokud není, získáte informace [přesun dat do systému SQL Server na platformě Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) tématu pokyny o tom, jak přesunout data do systému SQL Server v Azure.</span><span class="sxs-lookup"><span data-stu-id="4a038-108">If it is not, please refer to [Move data to SQL Server on Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) topic for instructions on how to move your data to SQL Server on Azure.</span></span>
> 
> 

<span data-ttu-id="4a038-109">Následující **nabídky** odkazy na témata, které popisují, jak ukázková data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a038-109">The following **menu** links to topics that describe how to sample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="4a038-110">**Proč ukázková data?**</span><span class="sxs-lookup"><span data-stu-id="4a038-110">**Why sample your data?**</span></span>
<span data-ttu-id="4a038-111">Pokud je velké datové sady, které chcete analyzovat, je obvykle vhodné nižší ukázková data, která mají snížit velikost menší, ale reprezentativní a lepší správu bitlockeru.</span><span class="sxs-lookup"><span data-stu-id="4a038-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="4a038-112">To usnadňuje pochopení dat, zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="4a038-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="4a038-113">V jeho role [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) je umožnit rychlé vytváření prototypů zpracování dat funkcí a modelů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="4a038-113">Its role in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

<span data-ttu-id="4a038-114">Tato úloha vzorkování je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="4a038-114">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <span data-ttu-id="4a038-115"><a name="SQL"></a>Pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="4a038-115"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="4a038-116">Tato část popisuje několik způsobů použití SQL k provedení prostý náhodný výběr proti data v databázi.</span><span class="sxs-lookup"><span data-stu-id="4a038-116">This section describes several methods using SQL to perform simple random sampling against the data in the database.</span></span> <span data-ttu-id="4a038-117">Vyberte metodu na základě vaší velikost dat a jeho distribuci.</span><span class="sxs-lookup"><span data-stu-id="4a038-117">Please choose a method based on your data size and its distribution.</span></span>

<span data-ttu-id="4a038-118">Následující dvě položky ukazují, jak provádět vzorkuje pomocí newid v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4a038-118">The two items below show how to use newid in SQL Server to perform the sampling.</span></span> <span data-ttu-id="4a038-119">Metoda zvolíte závisí na tom, jak náhodné chcete vzorku, který má být (pk_id v ukázkovém kódu níže se předpokládá, že se automaticky vygeneruje primární klíč).</span><span class="sxs-lookup"><span data-stu-id="4a038-119">The method you choose depends on how random you want the sample to be (pk_id in the sample code below is assumed to be an auto-generated primary key).</span></span>

1. <span data-ttu-id="4a038-120">Méně přísná náhodného vzorku</span><span class="sxs-lookup"><span data-stu-id="4a038-120">Less strict random sample</span></span>
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. <span data-ttu-id="4a038-121">Více náhodného vzorku</span><span class="sxs-lookup"><span data-stu-id="4a038-121">More random sample</span></span> 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

<span data-ttu-id="4a038-122">Klauzule Tablesample může být použit pro vzorkování, stejně jako ukázáno níže.</span><span class="sxs-lookup"><span data-stu-id="4a038-122">Tablesample can be used for sampling as well as demonstrated below.</span></span> <span data-ttu-id="4a038-123">To může být lepším řešením, pokud (za předpokladu, že data na různých stránkách není korelační) má velká velikost dat a pro dotaz na dokončení v přiměřené době.</span><span class="sxs-lookup"><span data-stu-id="4a038-123">This may be a better approach if your data size is large (assuming that data on different pages is not correlated) and for the query to complete in a reasonable time.</span></span>

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> <span data-ttu-id="4a038-124">Vám umožní zkoumat a generovat funkce z této jen Vzorkovaná data ukládáním do nové tabulky</span><span class="sxs-lookup"><span data-stu-id="4a038-124">You can explore and generate features from this sampled data by storing it in a new table</span></span>
> 
> 

### <span data-ttu-id="4a038-125"><a name="sql-aml"></a>Připojení k Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4a038-125"><a name="sql-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="4a038-126">Ukázkové dotazy výše můžete použít přímo v Azure Machine Learning [importovat Data] [ import-data] modulu nižší – ukázka data za chodu a tím, že ji do experimentu Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4a038-126">You can directly  use the sample queries above in the Azure Machine Learning [Import Data][import-data] module to down-sample the data on the fly and bring it into an Azure Machine Learning experiment.</span></span> <span data-ttu-id="4a038-127">Snímek obrazovky pomocí modulu reader číst jen Vzorkovaná data je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="4a038-127">A screen shot of using the reader module to read the sampled data is shown below:</span></span>

![Čtečka sql][1]

## <span data-ttu-id="4a038-129"><a name="python"></a>Pomocí programovacího jazyka Python</span><span class="sxs-lookup"><span data-stu-id="4a038-129"><a name="python"></a>Using the Python programming language</span></span>
<span data-ttu-id="4a038-130">V této části ukazuje, jak pomocí [pyodbc knihovny](https://code.google.com/p/pyodbc/) k navázání připojení rozhraní ODBC do databáze systému SQL server v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="4a038-130">This section demonstrates using the [pyodbc library](https://code.google.com/p/pyodbc/) to establish an ODBC connect to a SQL server database in Python.</span></span> <span data-ttu-id="4a038-131">Připojovací řetězec databáze je následující: (nahraďte servername, dbname, uživatelské jméno a heslo s konfigurací):</span><span class="sxs-lookup"><span data-stu-id="4a038-131">The database connection string is as follows: (replace servername, dbname, username and password with your configuration):</span></span>

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="4a038-132">[Pandas](http://pandas.pydata.org/) knihovna v Pythonu obsahuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python.</span><span class="sxs-lookup"><span data-stu-id="4a038-132">The [Pandas](http://pandas.pydata.org/) library in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="4a038-133">Následující kód načte 0,1 % ukázkových dat z tabulky v databázi Azure SQL do Pandas dat:</span><span class="sxs-lookup"><span data-stu-id="4a038-133">The code below reads a 0.1% sample of the data from a table in Azure SQL database into a Pandas data :</span></span>

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

<span data-ttu-id="4a038-134">Teď můžete pracovat s jen Vzorkovaná data v rámci Pandas data.</span><span class="sxs-lookup"><span data-stu-id="4a038-134">You can now work with the sampled data in the Pandas data frame.</span></span> 

### <span data-ttu-id="4a038-135"><a name="python-aml"></a>Připojení k Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4a038-135"><a name="python-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="4a038-136">Následující vzorový kód slouží k uložení dat vzorkovat dolů do souboru a nahrajte ho do Azure blob.</span><span class="sxs-lookup"><span data-stu-id="4a038-136">You can use the following sample code to save the down-sampled data to a file and upload it to an Azure blob.</span></span> <span data-ttu-id="4a038-137">Data v objektu blob může číst přímo do experimentu Azure Machine Learning pomocí [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="4a038-137">The data in the blob can be directly read into an Azure Machine Learning Experiment using the [Import Data][import-data] module.</span></span> <span data-ttu-id="4a038-138">Kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="4a038-138">The steps are as follows:</span></span> 

1. <span data-ttu-id="4a038-139">Zápis dat rámečku pandas do místního souboru</span><span class="sxs-lookup"><span data-stu-id="4a038-139">Write the pandas data frame to a local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="4a038-140">Nahrát místní soubor do objektu blob Azure</span><span class="sxs-lookup"><span data-stu-id="4a038-140">Upload local file to Azure blob</span></span>
   
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
3. <span data-ttu-id="4a038-141">Čtení dat z Azure blob pomocí Azure Machine Learning [importovat Data] [ import-data] modulu, jak je znázorněno v okamžitými výsledky obrazovky níže:</span><span class="sxs-lookup"><span data-stu-id="4a038-141">Read data from Azure blob using Azure Machine Learning [Import Data][import-data] module as shown in the screen grab below:</span></span>

![Čtečka objektů blob][2]

## <a name="the-team-data-science-process-in-action-example"></a><span data-ttu-id="4a038-143">Proces Team dat. vědecké účely v příkladu akce</span><span class="sxs-lookup"><span data-stu-id="4a038-143">The Team Data Science Process in Action example</span></span>
<span data-ttu-id="4a038-144">Příklad začátku do konce návod procesu vědecké účely Team dat pomocí veřejné datové sady, najdete v tématu [proces vědecké účely Team dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="4a038-144">For an end-to-end walkthrough example of the Team Data Science Process a using a public dataset, see [Team Data Science Process in Action: using SQL Sever](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
