---
title: "Funkce aaaCreate pro data v systému SQL Server pomocí SQL a Python | Microsoft Docs"
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
ms.openlocfilehash: 223352edb0377a159333e7528ad03c43902e6f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a><span data-ttu-id="7819c-103">Vytvoření funkcí pro data v SQL Serveru pomocí jazyka SQL a Pythonu</span><span class="sxs-lookup"><span data-stu-id="7819c-103">Create features for data in SQL Server using SQL and Python</span></span>
<span data-ttu-id="7819c-104">Tento dokument ukazuje, jak funkce toogenerate data uložená v virtuální počítač SQL Server na platformě Azure, který pomůže algoritmy další efektivněji z dat hello.</span><span class="sxs-lookup"><span data-stu-id="7819c-104">This document shows how toogenerate features for data stored in a SQL Server VM on Azure that help algorithms learn more efficiently from hello data.</span></span> <span data-ttu-id="7819c-105">Tento krok můžete provést pomocí SQL nebo pomocí programovacího jazyka jako Python, které jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="7819c-105">This can be done by using SQL or by using a programming language like Python, both of which are demonstrated here.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="7819c-106">To **nabídky** odkazy tootopics, které popisují, jak funkce toocreate pro data v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="7819c-106">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="7819c-107">Tato úloha je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="7819c-107">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

> [!NOTE]
> <span data-ttu-id="7819c-108">Praktické příkladu lze najít hello [datovou sadu NYC taxíkem](http://www.andresmh.com/nyctaxitrips/) a toohello IPNB s názvem [NYC Data wrangling pomocí poznámkového bloku IPython a SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) pro návod začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="7819c-108">For a practical example, you can consult hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="7819c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7819c-109">Prerequisites</span></span>
<span data-ttu-id="7819c-110">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="7819c-110">This article assumes that you have:</span></span>

* <span data-ttu-id="7819c-111">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7819c-111">Created an Azure storage account.</span></span> <span data-ttu-id="7819c-112">Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="7819c-112">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="7819c-113">Vaše data uložena v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7819c-113">Stored your data in SQL Server.</span></span> <span data-ttu-id="7819c-114">Pokud máte není, najdete v části [přesunout data tooan Azure SQL Database pro Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) pokyny, jak toomove hello dat existuje.</span><span class="sxs-lookup"><span data-stu-id="7819c-114">If you have not, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) for instructions on how toomove hello data there.</span></span>

## <span data-ttu-id="7819c-115"><a name="sql-featuregen"></a>Funkce generování pomocí SQL</span><span class="sxs-lookup"><span data-stu-id="7819c-115"><a name="sql-featuregen"></a>Feature Generation with SQL</span></span>
<span data-ttu-id="7819c-116">V této části popisují jsme způsoby generování funkcí s použitím SQL:</span><span class="sxs-lookup"><span data-stu-id="7819c-116">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="7819c-117">Počet na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="7819c-117">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="7819c-118">Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="7819c-118">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="7819c-119">Zavedením hello funkce z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="7819c-119">Rolling out hello features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="7819c-120">Po vygenerování další funkce, můžete je přidat jako sloupce toohello existující tabulky nebo vytvořit novou tabulku s hello další funkce a primární klíč, který lze spojit s původní tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="7819c-120">Once you generate additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, that can be joined with hello original table.</span></span>
> 
> 

### <span data-ttu-id="7819c-121"><a name="sql-countfeature"></a>Počet na základě funkce generování</span><span class="sxs-lookup"><span data-stu-id="7819c-121"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="7819c-122">Tento dokument ukazuje dva způsoby generování funkce count.</span><span class="sxs-lookup"><span data-stu-id="7819c-122">This document demonstrates two ways of generating count features.</span></span> <span data-ttu-id="7819c-123">První metoda Hello používá podmíněného sum a druhý používá metoda hello hello 'where' klauzule.</span><span class="sxs-lookup"><span data-stu-id="7819c-123">hello first method uses conditional sum and hello second method uses hello 'where\` clause.</span></span> <span data-ttu-id="7819c-124">Potom tyto lze spojit s hello původní tabulky (s použitím sloupců primárních klíčů) toohave počet funkcí společně se původní data hello.</span><span class="sxs-lookup"><span data-stu-id="7819c-124">These can then be joined with hello original table (using primary key columns) toohave count features alongside hello original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <span data-ttu-id="7819c-125"><a name="sql-binningfeature"></a>Přihrádkování funkce generování</span><span class="sxs-lookup"><span data-stu-id="7819c-125"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="7819c-126">Hello následující příklad ukazuje, jak toogenerate binned funkce pomocí přihrádkování (s použitím 5 přihrádek) číselné sloupce, který lze použít jako funkce místo:</span><span class="sxs-lookup"><span data-stu-id="7819c-126">hello following example shows how toogenerate binned features by binning (using 5 bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="7819c-127"><a name="sql-featurerollout"></a>Zavedením hello funkce z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="7819c-127"><a name="sql-featurerollout"></a>Rolling out hello features from a single column</span></span>
<span data-ttu-id="7819c-128">V této části ukážeme, jak tooroll-out jeden sloupec v tabulce další funkce toogenerate.</span><span class="sxs-lookup"><span data-stu-id="7819c-128">In this section, we demonstrate how tooroll-out a single column in a table toogenerate additional features.</span></span> <span data-ttu-id="7819c-129">Hello příklad předpokládá, že se v tabulce hello, ze kterého se pokoušíte toogenerate funkce sloupec zeměpisné šířky nebo délky.</span><span class="sxs-lookup"><span data-stu-id="7819c-129">hello example assumes that there is a latitude or longitude column in hello table from which you are trying toogenerate features.</span></span>

<span data-ttu-id="7819c-130">Zde je stručný úvod do na data o umístění zeměpisnou šířku a délku (ze zásobníku se zdroji `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span><span class="sxs-lookup"><span data-stu-id="7819c-130">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span></span> <span data-ttu-id="7819c-131">To je užitečné toounderstand před featurizing hello umístění pole:</span><span class="sxs-lookup"><span data-stu-id="7819c-131">This is useful toounderstand before featurizing hello location field:</span></span>

* <span data-ttu-id="7819c-132">Hello přihlašovací nám oznamuje, zda jsme jsou severně nebo Jih, východ nebo – západ na celém světě hello.</span><span class="sxs-lookup"><span data-stu-id="7819c-132">hello sign tells us whether we are north or south, east or west on hello globe.</span></span>
* <span data-ttu-id="7819c-133">Nenulové hodnoty stovky číslice informuje nám používáme zeměpisné délky, není zeměpisnou šířku!</span><span class="sxs-lookup"><span data-stu-id="7819c-133">A nonzero hundreds digit tells us we're using longitude, not latitude!</span></span>
* <span data-ttu-id="7819c-134">Hello desítkami číslice dává tooabout pozice 1 000 kilometrech.</span><span class="sxs-lookup"><span data-stu-id="7819c-134">hello tens digit gives a position tooabout 1,000 kilometers.</span></span> <span data-ttu-id="7819c-135">Nabízí nám užitečné informace o jaké kontinentě nebo oceánu jsme na.</span><span class="sxs-lookup"><span data-stu-id="7819c-135">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="7819c-136">Hello jednotky číslice (jeden decimal stupeň) poskytuje pozici nahoru too111 kilometrech (60 mílové, o 69 miles).</span><span class="sxs-lookup"><span data-stu-id="7819c-136">hello units digit (one decimal degree) gives a position up too111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="7819c-137">Je nám říct zhruba jaké velké státu nebo země, které jsme jsou v.</span><span class="sxs-lookup"><span data-stu-id="7819c-137">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="7819c-138">první desetinné místo Hello je vhodné si too11.1 km: je možné rozlišit hello pozici jedno velké město z sousedních velké města.</span><span class="sxs-lookup"><span data-stu-id="7819c-138">hello first decimal place is worth up too11.1 km: it can distinguish hello position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="7819c-139">Hello dvě desetinná místa je vhodné si too1.1 km: ho můžete oddělit jeden vesnice vedle z hello.</span><span class="sxs-lookup"><span data-stu-id="7819c-139">hello second decimal place is worth up too1.1 km: it can separate one village from hello next.</span></span>
* <span data-ttu-id="7819c-140">Hello třetí desetinné místo je vhodné si too110 m: že poznáte velké zemědělských pole nebo institucionální kanceláře.</span><span class="sxs-lookup"><span data-stu-id="7819c-140">hello third decimal place is worth up too110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="7819c-141">Hello čtvrtého desetinného místa je vhodné si too11 m: že poznáte parcela.</span><span class="sxs-lookup"><span data-stu-id="7819c-141">hello fourth decimal place is worth up too11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="7819c-142">Je porovnatelný z hlediska toohello typické přesnost neopravené GPS jednotky s bez narušení.</span><span class="sxs-lookup"><span data-stu-id="7819c-142">It is comparable toohello typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="7819c-143">Hello páté desetinné místo je vhodné si too1.1 m: rozlišit stromy od sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="7819c-143">hello fifth decimal place is worth up too1.1 m: it distinguish trees from each other.</span></span> <span data-ttu-id="7819c-144">Přesnost úroveň toothis komerční GPS jednotky lze dosáhnout pouze s rozdílovou oprava.</span><span class="sxs-lookup"><span data-stu-id="7819c-144">Accuracy toothis level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="7819c-145">Hello šesté desetinné místo je vhodné si too0.11 m: tu můžete použít pro vytvoření rozložení struktury podrobně pro návrh krajiny, vytváření cest.</span><span class="sxs-lookup"><span data-stu-id="7819c-145">hello sixth decimal place is worth up too0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="7819c-146">Mělo by být víc než dost vhodný pro sledování pohybu glaciers a řek.</span><span class="sxs-lookup"><span data-stu-id="7819c-146">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="7819c-147">Toho lze dosáhnout pomocí painstaking míry s GPS, jako je například differentially opravené GPS.</span><span class="sxs-lookup"><span data-stu-id="7819c-147">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="7819c-148">informace o umístění Hello můžete lze featurized následujícím způsobem, oddělení oblast, umístění a informace o městě.</span><span class="sxs-lookup"><span data-stu-id="7819c-148">hello location information can can be featurized as follows, separating out region, location and city information.</span></span> <span data-ttu-id="7819c-149">Všimněte si, že jednou můžete také zavolat koncový bod REST například rozhraní API map Bing k dispozici na `https://msdn.microsoft.com/library/ff701710.aspx` tooget hello oblasti nebo oblastní informace.</span><span class="sxs-lookup"><span data-stu-id="7819c-149">Note that once can also call a REST end point such as Bing Maps API available at `https://msdn.microsoft.com/library/ff701710.aspx` tooget hello region/district information.</span></span>

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

<span data-ttu-id="7819c-150">Hello výše umístění na základě funkce mohou být další používá funkce další počet toogenerate, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="7819c-150">hello above location based features can be further used toogenerate additional count features as described earlier.</span></span>

> [!TIP]
> <span data-ttu-id="7819c-151">Můžete vložit prostřednictvím kódu programu hello záznamů pomocí vámi zvolený jazyk.</span><span class="sxs-lookup"><span data-stu-id="7819c-151">You can programmatically insert hello records using your language of choice.</span></span> <span data-ttu-id="7819c-152">Potřebujete tooinsert hello data v bloky tooimprove zápisu efektivitu [podívejte se na příklad hello toodo tento pomocí pyodbc zde](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span><span class="sxs-lookup"><span data-stu-id="7819c-152">You may need tooinsert hello data in chunks tooimprove write efficiency [Check out hello example of how toodo this using pyodbc here](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span></span>
> <span data-ttu-id="7819c-153">Další alternativou je tooinsert data v databázi pomocí hello [nástroj BCP](https://msdn.microsoft.com/library/ms162802.aspx)</span><span class="sxs-lookup"><span data-stu-id="7819c-153">Another alternative is tooinsert data in hello database using [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx)</span></span>
> 
> 

### <span data-ttu-id="7819c-154"><a name="sql-aml"></a>Připojení tooAzure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7819c-154"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="7819c-155">Funkce Hello nově vygenerované můžete přidat jako sloupec existující tabulky tooan nebo ukládat v nové tabulce a propojit s hello původní tabulky pro machine learning.</span><span class="sxs-lookup"><span data-stu-id="7819c-155">hello newly generated feature can be added as a column tooan existing table or stored in a new table and joined with hello original table for machine learning.</span></span> <span data-ttu-id="7819c-156">Funkce můžete generovat ani přistupovat, pokud už vytvořili, pomocí hello [importovat Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modulu v Azure ML, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="7819c-156">Features can be generated or accessed if already created, using hello [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module in Azure ML as shown below:</span></span>

![azureml čtečky](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <span data-ttu-id="7819c-158"><a name="python"></a>Pomocí programovacího jazyka jako Python</span><span class="sxs-lookup"><span data-stu-id="7819c-158"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="7819c-159">Pomocí funkce toogenerate Python, jakmile hello dat v systému SQL Server je podobná tooprocessing data v Azure blob, které se používá Python, jak je uvedeno v [procesu Azure Blob data v můžete data vědecké účely prostředí](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="7819c-159">Using Python toogenerate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python as documented in [Process Azure Blob data in you data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="7819c-160">Hello data musí toobe načíst z databáze hello do rámečku pandas data a pak můžete další zpracování.</span><span class="sxs-lookup"><span data-stu-id="7819c-160">hello data needs toobe loaded from hello database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="7819c-161">Jsme dokumentů hello proces připojení toohello databázi a načítání dat hello do rámečku hello dat v této části.</span><span class="sxs-lookup"><span data-stu-id="7819c-161">We document hello process of connecting toohello database and loading hello data into hello data frame in this section.</span></span>

<span data-ttu-id="7819c-162">Hello následující formátu řetězce připojení může být databáze systému SQL Server používané tooconnect tooa z Pythonu pomocí pyodbc (servername nahraďte, dbname, uživatelské jméno a heslo s konkrétními hodnotami):</span><span class="sxs-lookup"><span data-stu-id="7819c-162">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="7819c-163">Hello [Pandas knihovny](http://pandas.pydata.org/) v Pythonu poskytuje bohatou sadu datové struktury a nástrojů pro analýzu dat pro manipulaci s daty pro programování Python.</span><span class="sxs-lookup"><span data-stu-id="7819c-163">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="7819c-164">Následující kód Hello čte vráceny výsledky hello do rámečku Pandas data z databáze SQL serveru:</span><span class="sxs-lookup"><span data-stu-id="7819c-164">hello code below reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="7819c-165">Teď můžete pracovat s rámečkem data Pandas hello jako zahrnutých v tématech [vytvořit funkcí pro data úložiště objektů blob v Azure pomocí Panda](machine-learning-data-science-create-features-blob.md).</span><span class="sxs-lookup"><span data-stu-id="7819c-165">Now you can work with hello Pandas data frame as covered in topics [Create features for Azure blob storage data using Panda](machine-learning-data-science-create-features-blob.md).</span></span>

