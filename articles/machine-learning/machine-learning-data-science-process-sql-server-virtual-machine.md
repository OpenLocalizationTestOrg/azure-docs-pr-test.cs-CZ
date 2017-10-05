---
title: "Prozkoumejte data virtuálního počítače s SQL serverem v Azure | Microsoft Docs"
description: "Zkoumat data a funkce generování ve virtuálním počítači systému SQL Server v Azure"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: 
ms.assetid: 3949fb2c-ffab-49fb-908d-27d5e42f743b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 16fabb29bdc8ec770efd843e18e9016e338a8f4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="d991d-103"><a name="heading"></a>Zpracování dat v virtuálního počítače systému SQL Server v Azure</span><span class="sxs-lookup"><span data-stu-id="d991d-103"><a name="heading"></a>Process Data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="d991d-104">Tento dokument popisuje, jak procházet data a vygenerovat funkcí pro data uložená ve virtuálním počítači serveru SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="d991d-104">This document covers how to explore data and generate features for data stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="d991d-105">Tento krok můžete provést pomocí dat wrangling pomocí SQL nebo pomocí programovacího jazyka jako Python.</span><span class="sxs-lookup"><span data-stu-id="d991d-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

> [!NOTE]
> <span data-ttu-id="d991d-106">Ukázkové příkazy SQL v tomto dokumentu předpokládají, že data jsou v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d991d-106">The sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="d991d-107">Pokud tomu tak není, podívejte se na proces mapování cloudu dat vědecké účely se dozvíte, jak pro přesun dat do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d991d-107">If it isn't, refer to the cloud data science process map to learn how to move your data to SQL Server.</span></span>
> 
> 

## <span data-ttu-id="d991d-108"><a name="SQL"></a>Pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="d991d-108"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="d991d-109">Jsme popisují následující úlohy wrangling dat v této části pomocí SQL:</span><span class="sxs-lookup"><span data-stu-id="d991d-109">We describe the following data wrangling tasks in this section using SQL:</span></span>

1. [<span data-ttu-id="d991d-110">Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="d991d-110">Data Exploration</span></span>](#sql-dataexploration)
2. [<span data-ttu-id="d991d-111">Funkce generování</span><span class="sxs-lookup"><span data-stu-id="d991d-111">Feature Generation</span></span>](#sql-featuregen)

### <span data-ttu-id="d991d-112"><a name="sql-dataexploration"></a>Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="d991d-112"><a name="sql-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="d991d-113">Tady jsou několik ukázkové skripty SQL, které lze použít k prozkoumání datová úložiště v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d991d-113">Here are a few sample SQL scripts that can be used to explore data stores in SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="d991d-114">Praktické příklad, můžete použít [datovou sadu NYC taxíkem](http://www.andresmh.com/nyctaxitrips/) a odkazovat na IPNB s názvem [NYC Data wrangling pomocí poznámkového bloku IPython a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro návod začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="d991d-114">For a practical example, you can use the [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer to the IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

1. <span data-ttu-id="d991d-115">Získat počet připomínky za den</span><span class="sxs-lookup"><span data-stu-id="d991d-115">Get the count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="d991d-116">Získat úrovně ve sloupci kategorií</span><span class="sxs-lookup"><span data-stu-id="d991d-116">Get the levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="d991d-117">Získat počet úrovní v kombinaci dvou kategorií sloupců</span><span class="sxs-lookup"><span data-stu-id="d991d-117">Get the number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="d991d-118">Získat distribuce pro číselné sloupce</span><span class="sxs-lookup"><span data-stu-id="d991d-118">Get the distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <span data-ttu-id="d991d-119"><a name="sql-featuregen"></a>Funkce generování</span><span class="sxs-lookup"><span data-stu-id="d991d-119"><a name="sql-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="d991d-120">V této části popisují jsme způsoby generování funkcí s použitím SQL:</span><span class="sxs-lookup"><span data-stu-id="d991d-120">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="d991d-121">Počet na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="d991d-121">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="d991d-122">Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="d991d-122">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="d991d-123">Zavedením funkce z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="d991d-123">Rolling out the features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="d991d-124">Po vygenerování další funkce, můžete je přidat jako sloupce do existující tabulky nebo vytvořit novou tabulku s další funkce a primární klíč, který lze spojit s původní tabulky.</span><span class="sxs-lookup"><span data-stu-id="d991d-124">Once you generate additional features, you can either add them as columns to the existing table or create a new table with the additional features and primary key, that can be joined with the original table.</span></span> 
> 
> 

### <span data-ttu-id="d991d-125"><a name="sql-countfeature"></a>Počet na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="d991d-125"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="d991d-126">Následující příklady ukazují dva způsoby generování funkce count.</span><span class="sxs-lookup"><span data-stu-id="d991d-126">The following examples demonstrate two ways of generating count features.</span></span> <span data-ttu-id="d991d-127">První metoda používá podmíněného sum a druhé metody klauzuli 'where'.</span><span class="sxs-lookup"><span data-stu-id="d991d-127">The first method uses conditional sum and the second method uses the 'where' clause.</span></span> <span data-ttu-id="d991d-128">Tyto je pak možné připojit s původní tabulky (s použitím sloupců primárních klíčů) tak, aby měl funkce počet souběžně s původní data.</span><span class="sxs-lookup"><span data-stu-id="d991d-128">These can then be joined with the original table (using primary key columns) to have count features alongside the original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <span data-ttu-id="d991d-129"><a name="sql-binningfeature"></a>Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="d991d-129"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="d991d-130">Následující příklad ukazuje, jak vygenerujte binned funkce přihrádkování (pomocí pět přihrádek) číselné sloupce, který lze použít jako funkce místo:</span><span class="sxs-lookup"><span data-stu-id="d991d-130">The following example shows how to generate binned features by binning (using five bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="d991d-131"><a name="sql-featurerollout"></a>Zavedením funkce z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="d991d-131"><a name="sql-featurerollout"></a>Rolling out the features from a single column</span></span>
<span data-ttu-id="d991d-132">V této části ukážeme, jak k zavedení jeden sloupec v tabulce ke generování dalších funkcí.</span><span class="sxs-lookup"><span data-stu-id="d991d-132">In this section, we demonstrate how to roll out a single column in a table to generate additional features.</span></span> <span data-ttu-id="d991d-133">Příklad předpokládá, že je v tabulce, ze kterého chcete generovat funkce sloupec zeměpisné šířky nebo délky.</span><span class="sxs-lookup"><span data-stu-id="d991d-133">The example assumes that there is a latitude or longitude column in the table from which you are trying to generate features.</span></span>

<span data-ttu-id="d991d-134">Zde je stručný úvod do na data o umístění zeměpisnou šířku a délku (ze zásobníku se zdroji [postupy: měření přesnost zeměpisnou šířku a délku?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)).</span><span class="sxs-lookup"><span data-stu-id="d991d-134">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow [How to measure the accuracy of latitude and longitude?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)).</span></span> <span data-ttu-id="d991d-135">To je užitečné zjistit před featurizing pole umístění:</span><span class="sxs-lookup"><span data-stu-id="d991d-135">This is useful to understand before featurizing the location field:</span></span>

* <span data-ttu-id="d991d-136">Přihlašovací informuje nám jestli jsme jsou severní nebo – Jih, východ nebo – západ na celém světě.</span><span class="sxs-lookup"><span data-stu-id="d991d-136">The sign tells us whether we are north or south, east or west on the globe.</span></span>
* <span data-ttu-id="d991d-137">Nenulové hodnoty stovky číslice víme, že používáme zeměpisné délky, není zeměpisnou šířku!</span><span class="sxs-lookup"><span data-stu-id="d991d-137">A nonzero hundreds digit tells us that we're using longitude, not latitude!</span></span>
* <span data-ttu-id="d991d-138">Desítkami číslice dává pozice do asi 1000 kilometrech.</span><span class="sxs-lookup"><span data-stu-id="d991d-138">The tens digit gives a position to about 1,000 kilometers.</span></span> <span data-ttu-id="d991d-139">Nabízí nám užitečné informace o jaké kontinentě nebo oceánu jsme na.</span><span class="sxs-lookup"><span data-stu-id="d991d-139">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="d991d-140">Jednotky číslice (jeden decimal stupeň) poskytuje na pozici až 111 kilometrech (60 mílové, o 69 miles).</span><span class="sxs-lookup"><span data-stu-id="d991d-140">The units digit (one decimal degree) gives a position up to 111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="d991d-141">Je nám říct zhruba jaké velké státu nebo země, které jsme jsou v.</span><span class="sxs-lookup"><span data-stu-id="d991d-141">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="d991d-142">Na jedno desetinné místo je vhodné až 11.1 km: je možné rozlišit pozici jedno velké město z sousedních velké města.</span><span class="sxs-lookup"><span data-stu-id="d991d-142">The first decimal place is worth up to 11.1 km: it can distinguish the position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="d991d-143">Dvě desetinná místa je vhodné až 1.1 km: ho jeden vesnice nezávislá na další.</span><span class="sxs-lookup"><span data-stu-id="d991d-143">The second decimal place is worth up to 1.1 km: it can separate one village from the next.</span></span>
* <span data-ttu-id="d991d-144">Může zjistit velké zemědělských pole či institucionální univerzity vhodné až 110 m: je na tři desetinná místa.</span><span class="sxs-lookup"><span data-stu-id="d991d-144">The third decimal place is worth up to 110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="d991d-145">Může zjistit parcela čtvrtého desetinného místa je vhodné m: až 11.</span><span class="sxs-lookup"><span data-stu-id="d991d-145">The fourth decimal place is worth up to 11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="d991d-146">Je srovnatelná typické přesnost neopravené GPS jednotky s bez narušení.</span><span class="sxs-lookup"><span data-stu-id="d991d-146">It is comparable to the typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="d991d-147">Páté desetinné místo je vhodné až 1.1 m: že stromy ho odlišuje od sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="d991d-147">The fifth decimal place is worth up to 1.1 m: it distinguishes trees from each other.</span></span> <span data-ttu-id="d991d-148">Přesnost do této úrovně s komerční GPS jednotky lze dosáhnout pouze s rozdílovou oprava.</span><span class="sxs-lookup"><span data-stu-id="d991d-148">Accuracy to this level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="d991d-149">Šesté desetinné místo je vhodné až 0,11 m: že tu můžete použít pro vytvoření rozložení struktury podrobně pro návrh krajiny, vytváření cest.</span><span class="sxs-lookup"><span data-stu-id="d991d-149">The sixth decimal place is worth up to 0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="d991d-150">Mělo by být víc než dost vhodný pro sledování pohybu glaciers a řek.</span><span class="sxs-lookup"><span data-stu-id="d991d-150">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="d991d-151">Toho lze dosáhnout pomocí painstaking míry s GPS, jako je například differentially opravené GPS.</span><span class="sxs-lookup"><span data-stu-id="d991d-151">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="d991d-152">Informace o umístění může být featurized následujícím způsobem oddělení oblast, umístění a informace o městě.</span><span class="sxs-lookup"><span data-stu-id="d991d-152">The location information can be featurized as follows, separating out region, location, and city information.</span></span> <span data-ttu-id="d991d-153">Všimněte si, že byste také zavolat koncový bod REST například rozhraní API map Bing k dispozici na [vyhledat umístění bodem](https://msdn.microsoft.com/library/ff701710.aspx) získat informace o oblasti nebo oblasti.</span><span class="sxs-lookup"><span data-stu-id="d991d-153">Note that you can also call a REST end point such as Bing Maps API available at [Find a Location by Point](https://msdn.microsoft.com/library/ff701710.aspx) to get the region/district information.</span></span>

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

<span data-ttu-id="d991d-154">Tyto funkce na základě polohy další slouží ke generování dalších počet funkcí, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="d991d-154">These location-based features can be further used to generate additional count features as described earlier.</span></span> 

> [!TIP]
> <span data-ttu-id="d991d-155">Prostřednictvím kódu programu můžete vložit záznamů pomocí vámi zvolený jazyk.</span><span class="sxs-lookup"><span data-stu-id="d991d-155">You can programmatically insert the records using your language of choice.</span></span> <span data-ttu-id="d991d-156">Budete muset vložit data v blocích pro zlepšení efektivity zápisu (příklad toho, jak to provést pomocí pyodbc, naleznete v části [ukázka A HelloWorld pro přístup k SQL Server s pythonem](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)).</span><span class="sxs-lookup"><span data-stu-id="d991d-156">You may need to insert the data in chunks to improve write efficiency (for an example of how to do this using pyodbc, see [A HelloWorld sample to access SQLServer with python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)).</span></span> <span data-ttu-id="d991d-157">Další alternativou je k vkládání dat v databázi pomocí [nástroj BCP](https://msdn.microsoft.com/library/ms162802.aspx).</span><span class="sxs-lookup"><span data-stu-id="d991d-157">Another alternative is to insert data in the database using the [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx).</span></span>
> 
> 

### <span data-ttu-id="d991d-158"><a name="sql-aml"></a>Připojení k Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d991d-158"><a name="sql-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="d991d-159">Nově vygenerovaný funkce můžete přidat jako sloupec do existující tabulky nebo ukládat v nové tabulce a spojena s původní tabulky pro machine learning.</span><span class="sxs-lookup"><span data-stu-id="d991d-159">The newly generated feature can be added as a column to an existing table or stored in a new table and joined with the original table for machine learning.</span></span> <span data-ttu-id="d991d-160">Funkce můžete generovat ani přistupovat, pokud už vytvořili, pomocí [importovat Data] [ import-data] modulu v Azure Machine Learning, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="d991d-160">Features can be generated or accessed if already created, using the [Import Data][import-data] module in Azure Machine Learning as shown below:</span></span>

![azureml čtečky][1] 

## <span data-ttu-id="d991d-162"><a name="python"></a>Pomocí programovacího jazyka jako Python</span><span class="sxs-lookup"><span data-stu-id="d991d-162"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="d991d-163">Používá Python a zkoumat data funkce generovat, když jsou data v systému SQL Server je podobná zpracování dat v Azure blob, které se používá Python, jak je uvedeno v [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="d991d-163">Using Python to explore data and generate features when the data is in SQL Server is similar to processing data in Azure blob using Python as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="d991d-164">Data musí být načtená z databáze do rámečku pandas data a pak můžete další zpracování.</span><span class="sxs-lookup"><span data-stu-id="d991d-164">The data needs to be loaded from the database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="d991d-165">Jsme dokumentů proces připojení k databázi a načítání dat do rámečku dat v této části.</span><span class="sxs-lookup"><span data-stu-id="d991d-165">We document the process of connecting to the database and loading the data into the data frame in this section.</span></span>

<span data-ttu-id="d991d-166">Následující formátu řetězce připojení slouží k připojení k databázi systému SQL Server z Pythonu pomocí pyodbc (servername nahraďte, dbname, uživatelské jméno a heslo s konkrétními hodnotami):</span><span class="sxs-lookup"><span data-stu-id="d991d-166">The following connection string format can be used to connect to a SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="d991d-167">[Pandas knihovny](http://pandas.pydata.org/) v Pythonu poskytuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python.</span><span class="sxs-lookup"><span data-stu-id="d991d-167">The [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="d991d-168">Následující kód čte vráceny výsledky z databáze SQL serveru do rámečku Pandas dat:</span><span class="sxs-lookup"><span data-stu-id="d991d-168">The code below reads the results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="d991d-169">Teď můžete pracovat s rámečkem data Pandas jako popsané v článku [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="d991d-169">Now you can work with the Pandas data frame as covered in the article [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="azure-data-science-in-action-example"></a><span data-ttu-id="d991d-170">Vědecké zpracování dat Azure v příkladu akce</span><span class="sxs-lookup"><span data-stu-id="d991d-170">Azure Data Science in Action Example</span></span>
<span data-ttu-id="d991d-171">Příklad začátku do konce návod procesu vědecké účely dat Azure pomocí veřejné datové sady, naleznete v části [proces vědecké účely dat Azure v akci](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="d991d-171">For an end-to-end walkthrough example of the Azure Data Science Process using a public dataset, see [Azure Data Science Process in Action](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

