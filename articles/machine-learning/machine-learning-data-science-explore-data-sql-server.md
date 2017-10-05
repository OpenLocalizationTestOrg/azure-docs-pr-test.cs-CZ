---
title: "Zkoumat data v virtuálního počítače systému SQL Server na platformě Azure | Microsoft Docs"
description: "Jak chcete dynamicky prozkoumávat data, která je uložená ve virtuálním počítači serveru SQL v Azure."
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
ms.openlocfilehash: a2be21ef15b9209db1e97150e0297558fa69a7be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a><span data-ttu-id="3ecec-103">Zkoumání dat na virtuálním počítači s SQL Serverem v Azure</span><span class="sxs-lookup"><span data-stu-id="3ecec-103">Explore data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="3ecec-104">Tento dokument popisuje, jak chcete dynamicky prozkoumávat data, která je uložená ve virtuálním počítači serveru SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="3ecec-104">This document covers how to explore data that is stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="3ecec-105">Tento krok můžete provést pomocí dat wrangling pomocí SQL nebo pomocí programovacího jazyka jako Python.</span><span class="sxs-lookup"><span data-stu-id="3ecec-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

<span data-ttu-id="3ecec-106">Následující **nabídky** odkazy na témata, které popisují, jak používat nástroje a prozkoumejte data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="3ecec-106">The following **menu** links to topics that describe how to use tools to explore data from various storage environments.</span></span> <span data-ttu-id="3ecec-107">Tato úloha je krok v procesu Cortana Analytics (CAP).</span><span class="sxs-lookup"><span data-stu-id="3ecec-107">This task is a step in the Cortana Analytics Process (CAP).</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> <span data-ttu-id="3ecec-108">Ukázkové příkazy SQL v tomto dokumentu předpokládají, že data jsou v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3ecec-108">The sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="3ecec-109">Pokud tomu tak není, podívejte se na proces mapování cloudu dat vědecké účely se dozvíte, jak pro přesun dat do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3ecec-109">If it isn't, refer to the cloud data science process map to learn how to move your data to SQL Server.</span></span>
> 
> 

## <span data-ttu-id="3ecec-110"><a name="sql-dataexploration"></a>Prozkoumejte data SQL pomocí skriptů SQL</span><span class="sxs-lookup"><span data-stu-id="3ecec-110"><a name="sql-dataexploration"></a>Explore SQL data with SQL scripts</span></span>
<span data-ttu-id="3ecec-111">Tady jsou několik ukázkové skripty SQL, které lze použít k prozkoumání datová úložiště v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3ecec-111">Here are a few sample SQL scripts that can be used to explore data stores in SQL Server.</span></span>

1. <span data-ttu-id="3ecec-112">Získat počet připomínky za den</span><span class="sxs-lookup"><span data-stu-id="3ecec-112">Get the count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="3ecec-113">Získat úrovně ve sloupci kategorií</span><span class="sxs-lookup"><span data-stu-id="3ecec-113">Get the levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="3ecec-114">Získat počet úrovní v kombinaci dvou kategorií sloupců</span><span class="sxs-lookup"><span data-stu-id="3ecec-114">Get the number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="3ecec-115">Získat distribuce pro číselné sloupce</span><span class="sxs-lookup"><span data-stu-id="3ecec-115">Get the distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> <span data-ttu-id="3ecec-116">Praktické příklad, můžete použít [datovou sadu NYC taxíkem](http://www.andresmh.com/nyctaxitrips/) a odkazovat na IPNB s názvem [NYC Data wrangling pomocí poznámkového bloku IPython a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro návod začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="3ecec-116">For a practical example, you can use the [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer to the IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <span data-ttu-id="3ecec-117"><a name="python"></a>Prozkoumejte data SQL s Pythonem</span><span class="sxs-lookup"><span data-stu-id="3ecec-117"><a name="python"></a>Explore SQL data with Python</span></span>
<span data-ttu-id="3ecec-118">Používá Python a zkoumat data funkce generovat, když jsou data v systému SQL Server je podobná zpracování dat v Azure blob pomocí Python, jak je uvedeno v [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="3ecec-118">Using Python to explore data and generate features when the data is in SQL Server is similar to processing data in Azure blob using Python, as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="3ecec-119">Data musí být vždycky načtená z databáze do pandas DataFrame a pak můžete další zpracování.</span><span class="sxs-lookup"><span data-stu-id="3ecec-119">The data needs to be loaded from the database into a pandas DataFrame and then can be processed further.</span></span> <span data-ttu-id="3ecec-120">Jsme dokumentů proces připojení k databázi a načítání dat do DataFrame v této části.</span><span class="sxs-lookup"><span data-stu-id="3ecec-120">We document the process of connecting to the database and loading the data into the DataFrame in this section.</span></span>

<span data-ttu-id="3ecec-121">Následující formátu řetězce připojení slouží k připojení k databázi systému SQL Server z Pythonu pomocí pyodbc (servername nahraďte, dbname, uživatelské jméno a heslo s konkrétními hodnotami):</span><span class="sxs-lookup"><span data-stu-id="3ecec-121">The following connection string format can be used to connect to a SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="3ecec-122">[Pandas knihovny](http://pandas.pydata.org/) v Pythonu poskytuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python.</span><span class="sxs-lookup"><span data-stu-id="3ecec-122">The [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="3ecec-123">Následující kód čte vráceny výsledky z databáze SQL serveru do rámečku Pandas dat:</span><span class="sxs-lookup"><span data-stu-id="3ecec-123">The following code reads the results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="3ecec-124">Teď můžete pracovat s Pandas DataFrame jako popsané v tématu [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="3ecec-124">Now you can work with the Pandas DataFrame as covered in the topic [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="cortana-analytics-process-in-action-example"></a><span data-ttu-id="3ecec-125">Proces Cortana analýzy v příkladu akce</span><span class="sxs-lookup"><span data-stu-id="3ecec-125">Cortana Analytics Process in Action Example</span></span>
<span data-ttu-id="3ecec-126">Příklad začátku do konce návod o procesu analýzy Cortana použití veřejné datové sady, naleznete v části [Team datové vědy procesu v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="3ecec-126">For an end-to-end walkthrough example of the Cortana Analytics Process using a public dataset, see [The Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

