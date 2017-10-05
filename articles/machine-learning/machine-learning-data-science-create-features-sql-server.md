---
title: "Vytvoření funkce pro data v systému SQL Server pomocí SQL a Python | Microsoft Docs"
description: "Zpracování dat z SQL Azure"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: 
ms.assetid: bf1f4a6c-7711-4456-beb7-35fdccd46a44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;fashah;garye
ms.openlocfilehash: f0ac2799e2d8f18b2dd5b633555bfca08a44ba27
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a><span data-ttu-id="75fa3-103">Vytvoření funkcí pro data v SQL Serveru pomocí jazyka SQL a Pythonu</span><span class="sxs-lookup"><span data-stu-id="75fa3-103">Create features for data in SQL Server using SQL and Python</span></span>
<span data-ttu-id="75fa3-104">Tento dokument ukazuje, jak vygenerovat funkcí pro data uložená v virtuální počítač SQL Server na platformě Azure, který pomůže algoritmy efektivněji dozvědět se od data.</span><span class="sxs-lookup"><span data-stu-id="75fa3-104">This document shows how to generate features for data stored in a SQL Server VM on Azure that help algorithms learn more efficiently from the data.</span></span> <span data-ttu-id="75fa3-105">Tento krok můžete provést pomocí SQL nebo pomocí programovacího jazyka jako Python, které jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="75fa3-105">This can be done by using SQL or by using a programming language like Python, both of which are demonstrated here.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="75fa3-106">To **nabídky** odkazy na témata, které popisují, jak vytvořit funkce pro data v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="75fa3-106">This **menu** links to topics that describe how to create features for data in various environments.</span></span> <span data-ttu-id="75fa3-107">Tato úloha je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="75fa3-107">This task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

> [!NOTE]
> <span data-ttu-id="75fa3-108">Praktické příkladu lze najít [datovou sadu NYC taxíkem](http://www.andresmh.com/nyctaxitrips/) a odkazovat na IPNB s názvem [NYC Data wrangling pomocí poznámkového bloku IPython a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro návod začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="75fa3-108">For a practical example, you can consult the [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer to the IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="75fa3-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="75fa3-109">Prerequisites</span></span>
<span data-ttu-id="75fa3-110">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="75fa3-110">This article assumes that you have:</span></span>

* <span data-ttu-id="75fa3-111">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="75fa3-111">Created an Azure storage account.</span></span> <span data-ttu-id="75fa3-112">Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="75fa3-112">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="75fa3-113">Vaše data uložena v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="75fa3-113">Stored your data in SQL Server.</span></span> <span data-ttu-id="75fa3-114">Pokud máte není, najdete v části [přesun dat do Azure SQL Database pro Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) pokyny o tom, jak přesunout data existuje.</span><span class="sxs-lookup"><span data-stu-id="75fa3-114">If you have not, see [Move data to an Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) for instructions on how to move the data there.</span></span>

## <span data-ttu-id="75fa3-115"><a name="sql-featuregen"></a>Funkce generování pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="75fa3-115"><a name="sql-featuregen"></a>Feature Generation with SQL</span></span>
<span data-ttu-id="75fa3-116">V této části popisují jsme způsoby generování funkcí s použitím SQL:</span><span class="sxs-lookup"><span data-stu-id="75fa3-116">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="75fa3-117">Počet na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="75fa3-117">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="75fa3-118">Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="75fa3-118">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="75fa3-119">Zavedením funkce z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="75fa3-119">Rolling out the features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="75fa3-120">Po vygenerování další funkce, můžete je přidat jako sloupce do existující tabulky nebo vytvořit novou tabulku s další funkce a primární klíč, který lze spojit s původní tabulky.</span><span class="sxs-lookup"><span data-stu-id="75fa3-120">Once you generate additional features, you can either add them as columns to the existing table or create a new table with the additional features and primary key, that can be joined with the original table.</span></span>
> 
> 

### <span data-ttu-id="75fa3-121"><a name="sql-countfeature"></a>Počet na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="75fa3-121"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="75fa3-122">Tento dokument ukazuje dva způsoby generování funkce count.</span><span class="sxs-lookup"><span data-stu-id="75fa3-122">This document demonstrates two ways of generating count features.</span></span> <span data-ttu-id="75fa3-123">První metoda používá podmíněného sum a druhé metody klauzuli 'where'.</span><span class="sxs-lookup"><span data-stu-id="75fa3-123">The first method uses conditional sum and the second method uses the 'where\` clause.</span></span> <span data-ttu-id="75fa3-124">Tyto je pak možné připojit s původní tabulky (s použitím sloupců primárních klíčů) tak, aby měl funkce počet souběžně s původní data.</span><span class="sxs-lookup"><span data-stu-id="75fa3-124">These can then be joined with the original table (using primary key columns) to have count features alongside the original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <span data-ttu-id="75fa3-125"><a name="sql-binningfeature"></a>Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="75fa3-125"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="75fa3-126">Následující příklad ukazuje, jak vygenerujte binned funkce přihrádkování (s použitím 5 přihrádek) číselné sloupce, který lze použít jako funkce místo:</span><span class="sxs-lookup"><span data-stu-id="75fa3-126">The following example shows how to generate binned features by binning (using 5 bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="75fa3-127"><a name="sql-featurerollout"></a>Zavedením funkce z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="75fa3-127"><a name="sql-featurerollout"></a>Rolling out the features from a single column</span></span>
<span data-ttu-id="75fa3-128">V této části ukážeme, jak zavádění jeden sloupec v tabulce ke generování dalších funkcí.</span><span class="sxs-lookup"><span data-stu-id="75fa3-128">In this section, we demonstrate how to roll-out a single column in a table to generate additional features.</span></span> <span data-ttu-id="75fa3-129">Příklad předpokládá, že je v tabulce, ze kterého chcete generovat funkce sloupec zeměpisné šířky nebo délky.</span><span class="sxs-lookup"><span data-stu-id="75fa3-129">The example assumes that there is a latitude or longitude column in the table from which you are trying to generate features.</span></span>

<span data-ttu-id="75fa3-130">Zde je stručný úvod do na data o umístění zeměpisnou šířku a délku (ze zásobníku se zdroji `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span><span class="sxs-lookup"><span data-stu-id="75fa3-130">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span></span> <span data-ttu-id="75fa3-131">To je užitečné zjistit před featurizing pole umístění:</span><span class="sxs-lookup"><span data-stu-id="75fa3-131">This is useful to understand before featurizing the location field:</span></span>

* <span data-ttu-id="75fa3-132">Přihlašovací informuje nám jestli jsme jsou severní nebo – Jih, východ nebo – západ na celém světě.</span><span class="sxs-lookup"><span data-stu-id="75fa3-132">The sign tells us whether we are north or south, east or west on the globe.</span></span>
* <span data-ttu-id="75fa3-133">Nenulové hodnoty stovky číslice informuje nám používáme zeměpisné délky, není zeměpisnou šířku!</span><span class="sxs-lookup"><span data-stu-id="75fa3-133">A nonzero hundreds digit tells us we're using longitude, not latitude!</span></span>
* <span data-ttu-id="75fa3-134">Desítkami číslice dává pozice do asi 1000 kilometrech.</span><span class="sxs-lookup"><span data-stu-id="75fa3-134">The tens digit gives a position to about 1,000 kilometers.</span></span> <span data-ttu-id="75fa3-135">Nabízí nám užitečné informace o jaké kontinentě nebo oceánu jsme na.</span><span class="sxs-lookup"><span data-stu-id="75fa3-135">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="75fa3-136">Jednotky číslice (jeden decimal stupeň) poskytuje na pozici až 111 kilometrech (60 mílové, o 69 miles).</span><span class="sxs-lookup"><span data-stu-id="75fa3-136">The units digit (one decimal degree) gives a position up to 111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="75fa3-137">Je nám říct zhruba jaké velké státu nebo země, které jsme jsou v.</span><span class="sxs-lookup"><span data-stu-id="75fa3-137">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="75fa3-138">Na jedno desetinné místo je vhodné až 11.1 km: je možné rozlišit pozici jedno velké město z sousedních velké města.</span><span class="sxs-lookup"><span data-stu-id="75fa3-138">The first decimal place is worth up to 11.1 km: it can distinguish the position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="75fa3-139">Dvě desetinná místa je vhodné až 1.1 km: ho jeden vesnice nezávislá na další.</span><span class="sxs-lookup"><span data-stu-id="75fa3-139">The second decimal place is worth up to 1.1 km: it can separate one village from the next.</span></span>
* <span data-ttu-id="75fa3-140">Může zjistit velké zemědělských pole či institucionální univerzity vhodné až 110 m: je na tři desetinná místa.</span><span class="sxs-lookup"><span data-stu-id="75fa3-140">The third decimal place is worth up to 110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="75fa3-141">Může zjistit parcela čtvrtého desetinného místa je vhodné m: až 11.</span><span class="sxs-lookup"><span data-stu-id="75fa3-141">The fourth decimal place is worth up to 11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="75fa3-142">Je srovnatelná typické přesnost neopravené GPS jednotky s bez narušení.</span><span class="sxs-lookup"><span data-stu-id="75fa3-142">It is comparable to the typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="75fa3-143">Páté desetinné místo je vhodné až 1.1 m: rozlišit stromy od sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="75fa3-143">The fifth decimal place is worth up to 1.1 m: it distinguish trees from each other.</span></span> <span data-ttu-id="75fa3-144">Přesnost do této úrovně s komerční GPS jednotky lze dosáhnout pouze s rozdílovou oprava.</span><span class="sxs-lookup"><span data-stu-id="75fa3-144">Accuracy to this level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="75fa3-145">Šesté desetinné místo je vhodné až 0,11 m: že tu můžete použít pro vytvoření rozložení struktury podrobně pro návrh krajiny, vytváření cest.</span><span class="sxs-lookup"><span data-stu-id="75fa3-145">The sixth decimal place is worth up to 0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="75fa3-146">Mělo by být víc než dost vhodný pro sledování pohybu glaciers a řek.</span><span class="sxs-lookup"><span data-stu-id="75fa3-146">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="75fa3-147">Toho lze dosáhnout pomocí painstaking míry s GPS, jako je například differentially opravené GPS.</span><span class="sxs-lookup"><span data-stu-id="75fa3-147">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="75fa3-148">Informace o umístění můžete lze featurized následujícím způsobem, oblast, umístění a informace o městě oddělení.</span><span class="sxs-lookup"><span data-stu-id="75fa3-148">The location information can can be featurized as follows, separating out region, location and city information.</span></span> <span data-ttu-id="75fa3-149">Všimněte si, že jednou můžete také zavolat koncový bod REST například rozhraní API map Bing k dispozici na `https://msdn.microsoft.com/library/ff701710.aspx` získat informace o oblasti nebo oblasti.</span><span class="sxs-lookup"><span data-stu-id="75fa3-149">Note that once can also call a REST end point such as Bing Maps API available at `https://msdn.microsoft.com/library/ff701710.aspx` to get the region/district information.</span></span>

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

<span data-ttu-id="75fa3-150">Výše uvedené umístění na základě funkcí další lze vygenerujte počet další funkce, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="75fa3-150">The above location based features can be further used to generate additional count features as described earlier.</span></span>

> [!TIP]
> <span data-ttu-id="75fa3-151">Prostřednictvím kódu programu můžete vložit záznamů pomocí vámi zvolený jazyk.</span><span class="sxs-lookup"><span data-stu-id="75fa3-151">You can programmatically insert the records using your language of choice.</span></span> <span data-ttu-id="75fa3-152">Budete muset vložit data v blocích pro zlepšení efektivity zápisu [podívejte se na příklad, jak to provést pomocí pyodbc zde](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span><span class="sxs-lookup"><span data-stu-id="75fa3-152">You may need to insert the data in chunks to improve write efficiency [Check out the example of how to do this using pyodbc here](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span></span>
> <span data-ttu-id="75fa3-153">Další alternativou je k vkládání dat v databázi pomocí [nástroj BCP](https://msdn.microsoft.com/library/ms162802.aspx)</span><span class="sxs-lookup"><span data-stu-id="75fa3-153">Another alternative is to insert data in the database using [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx)</span></span>
> 
> 

### <span data-ttu-id="75fa3-154"><a name="sql-aml"></a>Připojení k Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="75fa3-154"><a name="sql-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="75fa3-155">Nově vygenerovaný funkce můžete přidat jako sloupec do existující tabulky nebo ukládat v nové tabulce a spojena s původní tabulky pro machine learning.</span><span class="sxs-lookup"><span data-stu-id="75fa3-155">The newly generated feature can be added as a column to an existing table or stored in a new table and joined with the original table for machine learning.</span></span> <span data-ttu-id="75fa3-156">Funkce můžete generovat ani přistupovat, pokud už vytvořili, pomocí [importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modulu v Azure ML, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="75fa3-156">Features can be generated or accessed if already created, using the [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module in Azure ML as shown below:</span></span>

![azureml čtečky](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <span data-ttu-id="75fa3-158"><a name="python"></a>Pomocí programovacího jazyka jako Python</span><span class="sxs-lookup"><span data-stu-id="75fa3-158"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="75fa3-159">Generování funkce, když jsou data v systému SQL Server pomocí Python je podobná zpracování dat v Azure blob, které se používá Python, jak je uvedeno v [procesu Azure Blob data v můžete data vědecké účely prostředí](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="75fa3-159">Using Python to generate features when the data is in SQL Server is similar to processing data in Azure blob using Python as documented in [Process Azure Blob data in you data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="75fa3-160">Data musí být načtená z databáze do rámečku pandas data a pak můžete další zpracování.</span><span class="sxs-lookup"><span data-stu-id="75fa3-160">The data needs to be loaded from the database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="75fa3-161">Jsme dokumentů proces připojení k databázi a načítání dat do rámečku dat v této části.</span><span class="sxs-lookup"><span data-stu-id="75fa3-161">We document the process of connecting to the database and loading the data into the data frame in this section.</span></span>

<span data-ttu-id="75fa3-162">Následující formátu řetězce připojení slouží k připojení k databázi systému SQL Server z Pythonu pomocí pyodbc (servername nahraďte, dbname, uživatelské jméno a heslo s konkrétními hodnotami):</span><span class="sxs-lookup"><span data-stu-id="75fa3-162">The following connection string format can be used to connect to a SQL Server database from Python using pyodbc (replace servername, dbname, username and password with your specific values):</span></span>

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="75fa3-163">[Pandas knihovny](http://pandas.pydata.org/) v Pythonu poskytuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python.</span><span class="sxs-lookup"><span data-stu-id="75fa3-163">The [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="75fa3-164">Následující kód čte vráceny výsledky z databáze SQL serveru do rámečku Pandas dat:</span><span class="sxs-lookup"><span data-stu-id="75fa3-164">The code below reads the results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="75fa3-165">Teď můžete pracovat s rámečkem data Pandas jako zahrnutých v tématech [vytvořit funkcí pro data úložiště objektů blob v Azure pomocí Panda](machine-learning-data-science-create-features-blob.md).</span><span class="sxs-lookup"><span data-stu-id="75fa3-165">Now you can work with the Pandas data frame as covered in topics [Create features for Azure blob storage data using Panda](machine-learning-data-science-create-features-blob.md).</span></span>

