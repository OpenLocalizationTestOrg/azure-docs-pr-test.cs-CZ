---
title: "aaaExplore data virtuálního počítače s SQL serverem v Azure | Microsoft Docs"
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
ms.openlocfilehash: 67f4b058b0f6557ee15fd42795c918d68f1a9871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="df7a1-103"><a name="heading"></a>Zpracování dat v virtuálního počítače systému SQL Server v Azure</span><span class="sxs-lookup"><span data-stu-id="df7a1-103"><a name="heading"></a>Process Data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="df7a1-104">Tento dokument popisuje jak tooexplore data a vytvářet funkce pro data uložená ve virtuálním počítači serveru SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="df7a1-104">This document covers how tooexplore data and generate features for data stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="df7a1-105">Tento krok můžete provést pomocí dat wrangling pomocí SQL nebo pomocí programovacího jazyka jako Python.</span><span class="sxs-lookup"><span data-stu-id="df7a1-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

> [!NOTE]
> <span data-ttu-id="df7a1-106">Hello vzorové příkazy SQL v tomto dokumentu předpokládají, že data jsou v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="df7a1-106">hello sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="df7a1-107">Pokud tomu tak není, podívejte toohello cloudu datové vědy proces mapy toolearn jak toomove vaše data tooSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="df7a1-107">If it isn't, refer toohello cloud data science process map toolearn how toomove your data tooSQL Server.</span></span>
> 
> 

## <span data-ttu-id="df7a1-108"><a name="SQL"></a>Pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="df7a1-108"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="df7a1-109">Jsme popisují hello následující úlohy wrangling dat v této části pomocí SQL:</span><span class="sxs-lookup"><span data-stu-id="df7a1-109">We describe hello following data wrangling tasks in this section using SQL:</span></span>

1. [<span data-ttu-id="df7a1-110">Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="df7a1-110">Data Exploration</span></span>](#sql-dataexploration)
2. [<span data-ttu-id="df7a1-111">Funkce generování</span><span class="sxs-lookup"><span data-stu-id="df7a1-111">Feature Generation</span></span>](#sql-featuregen)

### <span data-ttu-id="df7a1-112"><a name="sql-dataexploration"></a>Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="df7a1-112"><a name="sql-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="df7a1-113">Tady jsou několik ukázkové skripty SQL, které se dají použít tooexplore datová úložiště v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="df7a1-113">Here are a few sample SQL scripts that can be used tooexplore data stores in SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="df7a1-114">Praktické příklad, můžete použít hello [datovou sadu NYC taxíkem](http://www.andresmh.com/nyctaxitrips/) a toohello IPNB s názvem [NYC Data wrangling pomocí poznámkového bloku IPython a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro návod začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="df7a1-114">For a practical example, you can use hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

1. <span data-ttu-id="df7a1-115">Získat hello počet připomínky za den</span><span class="sxs-lookup"><span data-stu-id="df7a1-115">Get hello count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="df7a1-116">Získat hello úrovně ve sloupci kategorií</span><span class="sxs-lookup"><span data-stu-id="df7a1-116">Get hello levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="df7a1-117">Získat hello počet úrovní v kombinaci dvou kategorií sloupců</span><span class="sxs-lookup"><span data-stu-id="df7a1-117">Get hello number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="df7a1-118">Získat hello distribuce pro číselné sloupce</span><span class="sxs-lookup"><span data-stu-id="df7a1-118">Get hello distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <span data-ttu-id="df7a1-119"><a name="sql-featuregen"></a>Funkce generování</span><span class="sxs-lookup"><span data-stu-id="df7a1-119"><a name="sql-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="df7a1-120">V této části popisují jsme způsoby generování funkcí s použitím SQL:</span><span class="sxs-lookup"><span data-stu-id="df7a1-120">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="df7a1-121">Počet na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="df7a1-121">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="df7a1-122">Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="df7a1-122">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="df7a1-123">Zavedením hello funkce z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="df7a1-123">Rolling out hello features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="df7a1-124">Po vygenerování další funkce, můžete je přidat jako sloupce toohello existující tabulky nebo vytvořit novou tabulku s hello další funkce a primární klíč, který lze spojit s původní tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="df7a1-124">Once you generate additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, that can be joined with hello original table.</span></span> 
> 
> 

### <span data-ttu-id="df7a1-125"><a name="sql-countfeature"></a>Počet na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="df7a1-125"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="df7a1-126">Hello následující příklady znázorňují dva způsoby generování funkce count.</span><span class="sxs-lookup"><span data-stu-id="df7a1-126">hello following examples demonstrate two ways of generating count features.</span></span> <span data-ttu-id="df7a1-127">První metoda Hello používá podmíněného sum a druhý používá metoda hello hello 'where' klauzule.</span><span class="sxs-lookup"><span data-stu-id="df7a1-127">hello first method uses conditional sum and hello second method uses hello 'where' clause.</span></span> <span data-ttu-id="df7a1-128">Potom tyto lze spojit s hello původní tabulky (s použitím sloupců primárních klíčů) toohave počet funkcí společně se původní data hello.</span><span class="sxs-lookup"><span data-stu-id="df7a1-128">These can then be joined with hello original table (using primary key columns) toohave count features alongside hello original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <span data-ttu-id="df7a1-129"><a name="sql-binningfeature"></a>Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="df7a1-129"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="df7a1-130">Hello následující příklad ukazuje, jak toogenerate binned funkce pomocí přihrádkování (pomocí pět přihrádek) číselné sloupce, který lze použít jako funkce místo:</span><span class="sxs-lookup"><span data-stu-id="df7a1-130">hello following example shows how toogenerate binned features by binning (using five bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="df7a1-131"><a name="sql-featurerollout"></a>Zavedením hello funkce z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="df7a1-131"><a name="sql-featurerollout"></a>Rolling out hello features from a single column</span></span>
<span data-ttu-id="df7a1-132">V této části ukážeme, jak tooroll na jeden sloupec v tabulce další funkce toogenerate.</span><span class="sxs-lookup"><span data-stu-id="df7a1-132">In this section, we demonstrate how tooroll out a single column in a table toogenerate additional features.</span></span> <span data-ttu-id="df7a1-133">Hello příklad předpokládá, že se v tabulce hello, ze kterého se pokoušíte toogenerate funkce sloupec zeměpisné šířky nebo délky.</span><span class="sxs-lookup"><span data-stu-id="df7a1-133">hello example assumes that there is a latitude or longitude column in hello table from which you are trying toogenerate features.</span></span>

<span data-ttu-id="df7a1-134">Zde je stručný úvod do na data o umístění zeměpisnou šířku a délku (ze zásobníku se zdroji [jak toomeasure hello přesnost zeměpisnou šířku a délku?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)).</span><span class="sxs-lookup"><span data-stu-id="df7a1-134">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow [How toomeasure hello accuracy of latitude and longitude?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)).</span></span> <span data-ttu-id="df7a1-135">To je užitečné toounderstand před featurizing hello umístění pole:</span><span class="sxs-lookup"><span data-stu-id="df7a1-135">This is useful toounderstand before featurizing hello location field:</span></span>

* <span data-ttu-id="df7a1-136">Hello přihlašovací nám oznamuje, zda jsme jsou severně nebo Jih, východ nebo – západ na celém světě hello.</span><span class="sxs-lookup"><span data-stu-id="df7a1-136">hello sign tells us whether we are north or south, east or west on hello globe.</span></span>
* <span data-ttu-id="df7a1-137">Nenulové hodnoty stovky číslice víme, že používáme zeměpisné délky, není zeměpisnou šířku!</span><span class="sxs-lookup"><span data-stu-id="df7a1-137">A nonzero hundreds digit tells us that we're using longitude, not latitude!</span></span>
* <span data-ttu-id="df7a1-138">Hello desítkami číslice dává tooabout pozice 1 000 kilometrech.</span><span class="sxs-lookup"><span data-stu-id="df7a1-138">hello tens digit gives a position tooabout 1,000 kilometers.</span></span> <span data-ttu-id="df7a1-139">Nabízí nám užitečné informace o jaké kontinentě nebo oceánu jsme na.</span><span class="sxs-lookup"><span data-stu-id="df7a1-139">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="df7a1-140">Hello jednotky číslice (jeden decimal stupeň) poskytuje pozici nahoru too111 kilometrech (60 mílové, o 69 miles).</span><span class="sxs-lookup"><span data-stu-id="df7a1-140">hello units digit (one decimal degree) gives a position up too111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="df7a1-141">Je nám říct zhruba jaké velké státu nebo země, které jsme jsou v.</span><span class="sxs-lookup"><span data-stu-id="df7a1-141">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="df7a1-142">první desetinné místo Hello je vhodné si too11.1 km: je možné rozlišit hello pozici jedno velké město z sousedních velké města.</span><span class="sxs-lookup"><span data-stu-id="df7a1-142">hello first decimal place is worth up too11.1 km: it can distinguish hello position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="df7a1-143">Hello dvě desetinná místa je vhodné si too1.1 km: ho můžete oddělit jeden vesnice vedle z hello.</span><span class="sxs-lookup"><span data-stu-id="df7a1-143">hello second decimal place is worth up too1.1 km: it can separate one village from hello next.</span></span>
* <span data-ttu-id="df7a1-144">Hello třetí desetinné místo je vhodné si too110 m: že poznáte velké zemědělských pole nebo institucionální kanceláře.</span><span class="sxs-lookup"><span data-stu-id="df7a1-144">hello third decimal place is worth up too110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="df7a1-145">Hello čtvrtého desetinného místa je vhodné si too11 m: že poznáte parcela.</span><span class="sxs-lookup"><span data-stu-id="df7a1-145">hello fourth decimal place is worth up too11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="df7a1-146">Je porovnatelný z hlediska toohello typické přesnost neopravené GPS jednotky s bez narušení.</span><span class="sxs-lookup"><span data-stu-id="df7a1-146">It is comparable toohello typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="df7a1-147">Hello páté desetinné místo je vhodné si too1.1 m: že stromy ho odlišuje od sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="df7a1-147">hello fifth decimal place is worth up too1.1 m: it distinguishes trees from each other.</span></span> <span data-ttu-id="df7a1-148">Přesnost úroveň toothis komerční GPS jednotky lze dosáhnout pouze s rozdílovou oprava.</span><span class="sxs-lookup"><span data-stu-id="df7a1-148">Accuracy toothis level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="df7a1-149">Hello šesté desetinné místo je vhodné si too0.11 m: tu můžete použít pro vytvoření rozložení struktury podrobně pro návrh krajiny, vytváření cest.</span><span class="sxs-lookup"><span data-stu-id="df7a1-149">hello sixth decimal place is worth up too0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="df7a1-150">Mělo by být víc než dost vhodný pro sledování pohybu glaciers a řek.</span><span class="sxs-lookup"><span data-stu-id="df7a1-150">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="df7a1-151">Toho lze dosáhnout pomocí painstaking míry s GPS, jako je například differentially opravené GPS.</span><span class="sxs-lookup"><span data-stu-id="df7a1-151">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="df7a1-152">informace o umístění Hello může být featurized následujícím způsobem oddělení oblast, umístění a informace o městě.</span><span class="sxs-lookup"><span data-stu-id="df7a1-152">hello location information can be featurized as follows, separating out region, location, and city information.</span></span> <span data-ttu-id="df7a1-153">Všimněte si, že byste také zavolat koncový bod REST například rozhraní API map Bing k dispozici na [vyhledat umístění bodem](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello oblasti nebo oblastní informace.</span><span class="sxs-lookup"><span data-stu-id="df7a1-153">Note that you can also call a REST end point such as Bing Maps API available at [Find a Location by Point](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello region/district information.</span></span>

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

<span data-ttu-id="df7a1-154">Tyto funkce na základě umístění může být další používané toogenerate počet další funkce, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="df7a1-154">These location-based features can be further used toogenerate additional count features as described earlier.</span></span> 

> [!TIP]
> <span data-ttu-id="df7a1-155">Můžete vložit prostřednictvím kódu programu hello záznamů pomocí vámi zvolený jazyk.</span><span class="sxs-lookup"><span data-stu-id="df7a1-155">You can programmatically insert hello records using your language of choice.</span></span> <span data-ttu-id="df7a1-156">Potřebujete tooinsert hello data v efektivitu zápisu tooimprove bloky dat (pro příklad toodo tento pomocí pyodbc, najdete v tématu [A HelloWorld ukázkové tooaccess SQLServer s pythonem](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)).</span><span class="sxs-lookup"><span data-stu-id="df7a1-156">You may need tooinsert hello data in chunks tooimprove write efficiency (for an example of how toodo this using pyodbc, see [A HelloWorld sample tooaccess SQLServer with python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)).</span></span> <span data-ttu-id="df7a1-157">Další alternativou je tooinsert data v databázi hello pomocí hello [nástroj BCP](https://msdn.microsoft.com/library/ms162802.aspx).</span><span class="sxs-lookup"><span data-stu-id="df7a1-157">Another alternative is tooinsert data in hello database using hello [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx).</span></span>
> 
> 

### <span data-ttu-id="df7a1-158"><a name="sql-aml"></a>Připojení tooAzure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="df7a1-158"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="df7a1-159">Funkce Hello nově vygenerované můžete přidat jako sloupec existující tabulky tooan nebo ukládat v nové tabulce a propojit s hello původní tabulky pro machine learning.</span><span class="sxs-lookup"><span data-stu-id="df7a1-159">hello newly generated feature can be added as a column tooan existing table or stored in a new table and joined with hello original table for machine learning.</span></span> <span data-ttu-id="df7a1-160">Funkce můžete generovat ani přistupovat, pokud už vytvořili, pomocí hello [importovat Data] [ import-data] modulu v Azure Machine Learning, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="df7a1-160">Features can be generated or accessed if already created, using hello [Import Data][import-data] module in Azure Machine Learning as shown below:</span></span>

![azureml čtečky][1] 

## <span data-ttu-id="df7a1-162"><a name="python"></a>Pomocí programovacího jazyka jako Python</span><span class="sxs-lookup"><span data-stu-id="df7a1-162"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="df7a1-163">Pomocí Python tooexplore data a vygenerovat funkce, když hello data jsou v systému SQL Server je podobná tooprocessing data v Azure blob, které se používá Python, jak je uvedeno v [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="df7a1-163">Using Python tooexplore data and generate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="df7a1-164">Hello data musí toobe načíst z databáze hello do rámečku pandas data a pak můžete další zpracování.</span><span class="sxs-lookup"><span data-stu-id="df7a1-164">hello data needs toobe loaded from hello database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="df7a1-165">Jsme dokumentů hello proces připojení toohello databázi a načítání dat hello do rámečku hello dat v této části.</span><span class="sxs-lookup"><span data-stu-id="df7a1-165">We document hello process of connecting toohello database and loading hello data into hello data frame in this section.</span></span>

<span data-ttu-id="df7a1-166">Hello následující formátu řetězce připojení může být databáze systému SQL Server používané tooconnect tooa z Pythonu pomocí pyodbc (servername nahraďte, dbname, uživatelské jméno a heslo s konkrétními hodnotami):</span><span class="sxs-lookup"><span data-stu-id="df7a1-166">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="df7a1-167">Hello [Pandas knihovny](http://pandas.pydata.org/) v Pythonu poskytuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python.</span><span class="sxs-lookup"><span data-stu-id="df7a1-167">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="df7a1-168">Následující kód Hello čte vráceny výsledky hello do rámečku Pandas data z databáze SQL serveru:</span><span class="sxs-lookup"><span data-stu-id="df7a1-168">hello code below reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="df7a1-169">Teď můžete pracovat s rámečkem data Pandas hello jako popsaná v článku hello [procesu Azure Blob dat ve vašem prostředí vědecké účely data](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="df7a1-169">Now you can work with hello Pandas data frame as covered in hello article [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="azure-data-science-in-action-example"></a><span data-ttu-id="df7a1-170">Vědecké zpracování dat Azure v příkladu akce</span><span class="sxs-lookup"><span data-stu-id="df7a1-170">Azure Data Science in Action Example</span></span>
<span data-ttu-id="df7a1-171">Příklad začátku do konce návod hello proces vědecké účely dat Azure pomocí veřejné datové sady, najdete v části [proces vědecké účely dat Azure v akci](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="df7a1-171">For an end-to-end walkthrough example of hello Azure Data Science Process using a public dataset, see [Azure Data Science Process in Action](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

