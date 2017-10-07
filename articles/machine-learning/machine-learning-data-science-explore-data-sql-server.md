---
title: "aaaExplore data v SQL serveru virtuálního počítače na Azure | Microsoft Docs"
description: "Jak tooexplore data, která je uložená ve virtuálním počítači serveru SQL v Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: fcc449fc0d0e49be9b673cfb2de347cf44804017
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a><span data-ttu-id="01806-103">Zkoumání dat na virtuálním počítači s SQL Serverem v Azure</span><span class="sxs-lookup"><span data-stu-id="01806-103">Explore data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="01806-104">Tento dokument popisuje jak tooexplore data, která je uložená ve virtuálním počítači serveru SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="01806-104">This document covers how tooexplore data that is stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="01806-105">Tento krok můžete provést pomocí dat wrangling pomocí SQL nebo pomocí programovacího jazyka jako Python.</span><span class="sxs-lookup"><span data-stu-id="01806-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

<span data-ttu-id="01806-106">Následující Hello **nabídky** odkazy tootopics, které popisují, jak toouse nástroje tooexplore data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="01806-106">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span> <span data-ttu-id="01806-107">Tato úloha je krok v hello Cortana Analytics procesu (CAP).</span><span class="sxs-lookup"><span data-stu-id="01806-107">This task is a step in hello Cortana Analytics Process (CAP).</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> <span data-ttu-id="01806-108">Hello vzorové příkazy SQL v tomto dokumentu předpokládají, že data jsou v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="01806-108">hello sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="01806-109">Pokud tomu tak není, podívejte toohello cloudu datové vědy proces mapy toolearn jak toomove vaše data tooSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="01806-109">If it isn't, refer toohello cloud data science process map toolearn how toomove your data tooSQL Server.</span></span>
> 
> 

## <span data-ttu-id="01806-110"><a name="sql-dataexploration"></a>Prozkoumejte data SQL pomocí skriptů SQL</span><span class="sxs-lookup"><span data-stu-id="01806-110"><a name="sql-dataexploration"></a>Explore SQL data with SQL scripts</span></span>
<span data-ttu-id="01806-111">Tady jsou několik ukázkové skripty SQL, které se dají použít tooexplore datová úložiště v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="01806-111">Here are a few sample SQL scripts that can be used tooexplore data stores in SQL Server.</span></span>

1. <span data-ttu-id="01806-112">Získat hello počet připomínky za den</span><span class="sxs-lookup"><span data-stu-id="01806-112">Get hello count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="01806-113">Získat hello úrovně ve sloupci kategorií</span><span class="sxs-lookup"><span data-stu-id="01806-113">Get hello levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="01806-114">Získat hello počet úrovní v kombinaci dvou kategorií sloupců</span><span class="sxs-lookup"><span data-stu-id="01806-114">Get hello number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="01806-115">Získat hello distribuce pro číselné sloupce</span><span class="sxs-lookup"><span data-stu-id="01806-115">Get hello distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> <span data-ttu-id="01806-116">Praktické příklad, můžete použít hello [datovou sadu NYC taxíkem](http://www.andresmh.com/nyctaxitrips/) a toohello IPNB s názvem [NYC Data wrangling pomocí poznámkového bloku IPython a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro návod začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="01806-116">For a practical example, you can use hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <span data-ttu-id="01806-117"><a name="python"></a>Prozkoumejte data SQL s Pythonem</span><span class="sxs-lookup"><span data-stu-id="01806-117"><a name="python"></a>Explore SQL data with Python</span></span>
<span data-ttu-id="01806-118">Pomocí Python tooexplore data a vygenerovat funkce, když hello data jsou v systému SQL Server je podobná tooprocessing data v Azure blob pomocí Python, jak je uvedeno v [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="01806-118">Using Python tooexplore data and generate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python, as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="01806-119">Hello data musí načíst z databáze hello do pandas DataFrame toobe a pak můžete další zpracování.</span><span class="sxs-lookup"><span data-stu-id="01806-119">hello data needs toobe loaded from hello database into a pandas DataFrame and then can be processed further.</span></span> <span data-ttu-id="01806-120">Jsme dokumentů hello proces připojení toohello databázi a načítání dat hello do hello DataFrame v této části.</span><span class="sxs-lookup"><span data-stu-id="01806-120">We document hello process of connecting toohello database and loading hello data into hello DataFrame in this section.</span></span>

<span data-ttu-id="01806-121">Hello následující formátu řetězce připojení může být databáze systému SQL Server používané tooconnect tooa z Pythonu pomocí pyodbc (servername nahraďte, dbname, uživatelské jméno a heslo s konkrétními hodnotami):</span><span class="sxs-lookup"><span data-stu-id="01806-121">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="01806-122">Hello [Pandas knihovny](http://pandas.pydata.org/) v Pythonu poskytuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python.</span><span class="sxs-lookup"><span data-stu-id="01806-122">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="01806-123">Hello následující kód čte vráceny výsledky hello do rámečku Pandas data z databáze SQL serveru:</span><span class="sxs-lookup"><span data-stu-id="01806-123">hello following code reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="01806-124">Teď můžete pracovat s hello Pandas DataFrame jako popsané v tématu hello [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="01806-124">Now you can work with hello Pandas DataFrame as covered in hello topic [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="cortana-analytics-process-in-action-example"></a><span data-ttu-id="01806-125">Proces Cortana analýzy v příkladu akce</span><span class="sxs-lookup"><span data-stu-id="01806-125">Cortana Analytics Process in Action Example</span></span>
<span data-ttu-id="01806-126">Příklad začátku do konce návod hello Cortana procesu analýzy použití veřejné datové sady, najdete v části [hello proces vědecké účely Team dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="01806-126">For an end-to-end walkthrough example of hello Cortana Analytics Process using a public dataset, see [hello Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

