---
<span data-ttu-id="a0daf-101">Title: aaa "Azure Analysis Services kurz Lekce 2: získání dat | Microsoft Docs"Popis: Popisuje, jak hello tooget a import dat v kurzu projektu Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="a0daf-101">title: aaa"Azure Analysis Services tutorial lesson 2: Get data | Microsoft Docs" description: Describes how tooget and import data in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="a0daf-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="a0daf-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="a0daf-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 01/06/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="a0daf-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---

# <a name="lesson-2-get-data"></a><span data-ttu-id="a0daf-104">Lekce 2: Získání dat</span><span class="sxs-lookup"><span data-stu-id="a0daf-104">Lesson 2: Get data</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="a0daf-105">V této lekci použijete načíst Data v rozšíření SSDT tooconnect toohello AdventureWorksDW2014 ukázkovou databázi, vyberte data, náhled a filtr a potom importovat do pracovního prostoru modelu.</span><span class="sxs-lookup"><span data-stu-id="a0daf-105">In this lesson, you use Get Data in SSDT tooconnect toohello AdventureWorksDW2014 sample database, select data, preview and filter, and then import into your model workspace.</span></span>  
  
<span data-ttu-id="a0daf-106">Pomocí funkce Získání dat můžete importovat data z celé řady zdrojů: Azure SQL Database, Oracle, Sybase, kanál OData, Teradata, soubory a další.</span><span class="sxs-lookup"><span data-stu-id="a0daf-106">By using Get Data, you can import data from a wide variety of sources: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, files and more.</span></span> <span data-ttu-id="a0daf-107">Data umožňují také dotazy pomocí výrazu se vzorci Power Query M.</span><span class="sxs-lookup"><span data-stu-id="a0daf-107">Data can also be queried using a Power Query M formula expression.</span></span>
  
<span data-ttu-id="a0daf-108">Odhadovaný čas toocomplete této lekci: **10 minut**</span><span class="sxs-lookup"><span data-stu-id="a0daf-108">Estimated time toocomplete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="a0daf-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a0daf-109">Prerequisites</span></span>  
<span data-ttu-id="a0daf-110">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="a0daf-110">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="a0daf-111">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekci 1: vytvoření nového projektu tabulkový model](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span><span class="sxs-lookup"><span data-stu-id="a0daf-111">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 1: Create a new tabular model project](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span></span>  
  
## <a name="create-a-connection"></a><span data-ttu-id="a0daf-112">Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="a0daf-112">Create a connection</span></span>  
  
#### <a name="toocreate-a-connection-toohello-adventureworksdw2014-database"></a><span data-ttu-id="a0daf-113">toocreate databázi toohello AdventureWorksDW2014 připojení</span><span class="sxs-lookup"><span data-stu-id="a0daf-113">toocreate a connection toohello AdventureWorksDW2014 database</span></span>  
  
1.  <span data-ttu-id="a0daf-114">V Průzkumníku tabelárních modelů klikněte pravým tlačítkem na **Zdroje dat** > **Importovat ze zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-114">In Tabular Model Explorer, right-click **Data Sources** > **Import from Data Source**.</span></span>  
  
    <span data-ttu-id="a0daf-115">Spustí se získat Data, která vás provede připojování zdroje dat tooa.</span><span class="sxs-lookup"><span data-stu-id="a0daf-115">This launches Get Data, which guides you through connecting tooa data source.</span></span> <span data-ttu-id="a0daf-116">Pokud nevidíte v tabulkovém modelu Průzkumníku **Průzkumníku řešení**, dvakrát klikněte na **Model.bim** tooopen hello modelu v Návrháři hello.</span><span class="sxs-lookup"><span data-stu-id="a0daf-116">If you don't see Tabular Model Explorer, in **Solution Explorer**, double-click **Model.bim** tooopen hello model in hello designer.</span></span> 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  <span data-ttu-id="a0daf-118">Ve funkci Získání dat klikněte na **Databáze** > **Databáze SQL Serveru** > **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-118">In Get Data, click **Database** > **SQL Server Database** > **Connect**.</span></span>  
  
3.  <span data-ttu-id="a0daf-119">V hello **databáze systému SQL Server** dialogové okno, v **Server**, zadejte název hello hello serveru, kam jste nainstalovali hello AdventureWorksDW2014 databáze a pak klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-119">In hello **SQL Server Database** dialog, in **Server**, type hello name of hello server where you installed hello AdventureWorksDW2014 database, and then click **Connect**.</span></span>  

4.  <span data-ttu-id="a0daf-120">Po zobrazení výzvy tooenter přihlašovací údaje, je nutné toospecify hello pověření služby Analysis Services používá tooconnect toohello zdroj dat při importu a zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="a0daf-120">When prompted tooenter credentials, you need toospecify hello credentials Analysis Services uses tooconnect toohello data source when importing and processing data.</span></span> <span data-ttu-id="a0daf-121">V části **Režim zosobnění** vyberte **Zosobnit účet**, zadejte přihlašovací údaje a klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-121">In **Impersonation Mode**, select **Impersonate Account**, then enter credentials, and then click **Connect**.</span></span> <span data-ttu-id="a0daf-122">Doporučuje se, že používáte účet, kde není hello heslo vyprší.</span><span class="sxs-lookup"><span data-stu-id="a0daf-122">It's recommended you use an account where hello password doesn't expire.</span></span>

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > <span data-ttu-id="a0daf-124">Pomocí uživatelského účtu systému Windows a hesla poskytuje nejbezpečnější metodou hello připojování zdroje dat tooa.</span><span class="sxs-lookup"><span data-stu-id="a0daf-124">Using a Windows user account and password provides hello most secure method of connecting tooa data source.</span></span>
  
5.  <span data-ttu-id="a0daf-125">V tabulce dat, vyberte hello **AdventureWorksDW2014** databáze a potom klikněte na **OK**. Tím se vytvoří databáze toohello hello připojení.</span><span class="sxs-lookup"><span data-stu-id="a0daf-125">In Navigator, select hello **AdventureWorksDW2014** database, and then click **OK**.This creates hello connection toohello database.</span></span> 
  
6.  <span data-ttu-id="a0daf-126">V tabulce dat, vyberte hello zaškrtávací políčko pro hello následujících tabulek: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**,  **DimProductCategory**, **DimProductSubcategory**, a **FactInternetSales**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-126">In Navigator, select hello check box for hello following tables: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, and **FactInternetSales**.</span></span>  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
<span data-ttu-id="a0daf-128">Po kliknutí na OK se otevře Editor dotazů.</span><span class="sxs-lookup"><span data-stu-id="a0daf-128">After you click OK, Query Editor opens.</span></span> <span data-ttu-id="a0daf-129">V další části hello vyberte pouze hello data, která chcete tooimport.</span><span class="sxs-lookup"><span data-stu-id="a0daf-129">In hello next section, you select only hello data you want tooimport.</span></span>

  
## <a name="filter-hello-table-data"></a><span data-ttu-id="a0daf-130">Filtrování dat v tabulce hello</span><span class="sxs-lookup"><span data-stu-id="a0daf-130">Filter hello table data</span></span>  
<span data-ttu-id="a0daf-131">Tabulky v ukázkové databázi AdventureWorksDW2014 hello mají data, která není nutné tooinclude v modelu.</span><span class="sxs-lookup"><span data-stu-id="a0daf-131">Tables in hello AdventureWorksDW2014 sample database have data that isn't necessary tooinclude in your model.</span></span> <span data-ttu-id="a0daf-132">Pokud je to možné, budete chtít toofilter se místo v paměti toosave nepotřebných dat používaný modelem hello.</span><span class="sxs-lookup"><span data-stu-id="a0daf-132">When possible, you want toofilter out unnecessary data toosave in-memory space used by hello model.</span></span> <span data-ttu-id="a0daf-133">Můžete filtrovat některé z hello sloupce z tabulky, aby nejsou importovány do databáze pracovního prostoru hello hello model databáze, nebo po jeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="a0daf-133">You filter out some of hello columns from tables so they're not imported into hello workspace database, or hello model database after it has been deployed.</span></span> 
  
#### <a name="toofilter-hello-table-data-before-importing"></a><span data-ttu-id="a0daf-134">data tabulky hello toofilter před importem</span><span class="sxs-lookup"><span data-stu-id="a0daf-134">toofilter hello table data before importing</span></span>  
  
1.  <span data-ttu-id="a0daf-135">V editoru dotazů vyberte hello **DimCustomer** tabulky.</span><span class="sxs-lookup"><span data-stu-id="a0daf-135">In Query Editor, select hello **DimCustomer** table.</span></span> <span data-ttu-id="a0daf-136">Hello DimCustomer tabulky na zdroj dat hello (ukázkové databázi AdventureWorksDWQ2014) se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="a0daf-136">A view of hello DimCustomer table at hello datasource (your AdventureWorksDWQ2014 sample database) appears.</span></span> 
  
2.  <span data-ttu-id="a0daf-137">Vyberte (Ctrl + kliknutí) sloupce **SpanishEducation**, **FrenchEducation**, **SpanishOccupation** a **FrenchOccupation**, klikněte pravým tlačítkem a potom klikněte na **Odebrat sloupce**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-137">Multi-select (Ctrl + click) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**, then right-click, and then click **Remove Columns**.</span></span> 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    <span data-ttu-id="a0daf-139">Vzhledem k tomu, že hello hodnoty pro tyto sloupce nejsou relevantní tooInternet prodejní analýzy, není tooimport bez nutnosti tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="a0daf-139">Since hello values for these columns are not relevant tooInternet sales analysis, there is no need tooimport these columns.</span></span> <span data-ttu-id="a0daf-140">Díky odstranění nepotřebných sloupců bude váš model menší a efektivnější.</span><span class="sxs-lookup"><span data-stu-id="a0daf-140">Eliminating unnecessary columns makes your model smaller and more efficient.</span></span>  
  
4.  <span data-ttu-id="a0daf-141">Filtrovat hello zbývající tabulky odebráním hello následující sloupce v každé tabulce:</span><span class="sxs-lookup"><span data-stu-id="a0daf-141">Filter hello remaining tables by removing hello following columns in each table:</span></span>  
    
    <span data-ttu-id="a0daf-142">**DimDate**</span><span class="sxs-lookup"><span data-stu-id="a0daf-142">**DimDate**</span></span>
    
      |<span data-ttu-id="a0daf-143">Sloupec</span><span class="sxs-lookup"><span data-stu-id="a0daf-143">Column</span></span>|  
      |--------|  
      |<span data-ttu-id="a0daf-144">DateKey</span><span class="sxs-lookup"><span data-stu-id="a0daf-144">DateKey</span></span>|  
      |<span data-ttu-id="a0daf-145">**SpanishDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="a0daf-145">**SpanishDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="a0daf-146">**FrenchDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="a0daf-146">**FrenchDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="a0daf-147">**SpanishMonthName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-147">**SpanishMonthName**</span></span>|  
      |<span data-ttu-id="a0daf-148">**FrenchMonthName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-148">**FrenchMonthName**</span></span>|  
  
    <span data-ttu-id="a0daf-149">**DimGeography**</span><span class="sxs-lookup"><span data-stu-id="a0daf-149">**DimGeography**</span></span>
  
      |<span data-ttu-id="a0daf-150">Sloupec</span><span class="sxs-lookup"><span data-stu-id="a0daf-150">Column</span></span>|  
      |-------------|  
      |<span data-ttu-id="a0daf-151">**SpanishCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-151">**SpanishCountryRegionName**</span></span>|  
      |<span data-ttu-id="a0daf-152">**FrenchCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-152">**FrenchCountryRegionName**</span></span>|  
      |<span data-ttu-id="a0daf-153">**IpAddressLocator**</span><span class="sxs-lookup"><span data-stu-id="a0daf-153">**IpAddressLocator**</span></span>|  
  
    <span data-ttu-id="a0daf-154">**DimProduct**</span><span class="sxs-lookup"><span data-stu-id="a0daf-154">**DimProduct**</span></span>
  
      |<span data-ttu-id="a0daf-155">Sloupec</span><span class="sxs-lookup"><span data-stu-id="a0daf-155">Column</span></span>|  
      |-----------|  
      |<span data-ttu-id="a0daf-156">**SpanishProductName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-156">**SpanishProductName**</span></span>|  
      |<span data-ttu-id="a0daf-157">**FrenchProductName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-157">**FrenchProductName**</span></span>|  
      |<span data-ttu-id="a0daf-158">**FrenchDescription**</span><span class="sxs-lookup"><span data-stu-id="a0daf-158">**FrenchDescription**</span></span>|  
      |<span data-ttu-id="a0daf-159">**ChineseDescription**</span><span class="sxs-lookup"><span data-stu-id="a0daf-159">**ChineseDescription**</span></span>|  
      |<span data-ttu-id="a0daf-160">**ArabicDescription**</span><span class="sxs-lookup"><span data-stu-id="a0daf-160">**ArabicDescription**</span></span>|  
      |<span data-ttu-id="a0daf-161">**HebrewDescription**</span><span class="sxs-lookup"><span data-stu-id="a0daf-161">**HebrewDescription**</span></span>|  
      |<span data-ttu-id="a0daf-162">**ThaiDescription**</span><span class="sxs-lookup"><span data-stu-id="a0daf-162">**ThaiDescription**</span></span>|  
      |<span data-ttu-id="a0daf-163">**GermanDescription**</span><span class="sxs-lookup"><span data-stu-id="a0daf-163">**GermanDescription**</span></span>|  
      |<span data-ttu-id="a0daf-164">**JapaneseDescription**</span><span class="sxs-lookup"><span data-stu-id="a0daf-164">**JapaneseDescription**</span></span>|  
      |<span data-ttu-id="a0daf-165">**TurkishDescription**</span><span class="sxs-lookup"><span data-stu-id="a0daf-165">**TurkishDescription**</span></span>|  
  
    <span data-ttu-id="a0daf-166">**DimProductCategory**</span><span class="sxs-lookup"><span data-stu-id="a0daf-166">**DimProductCategory**</span></span>
  
      |<span data-ttu-id="a0daf-167">Sloupec</span><span class="sxs-lookup"><span data-stu-id="a0daf-167">Column</span></span>|  
      |--------------------|  
      |<span data-ttu-id="a0daf-168">**SpanishProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-168">**SpanishProductCategoryName**</span></span>|  
      |<span data-ttu-id="a0daf-169">**FrenchProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-169">**FrenchProductCategoryName**</span></span>|  
  
    <span data-ttu-id="a0daf-170">**DimProductSubcategory**</span><span class="sxs-lookup"><span data-stu-id="a0daf-170">**DimProductSubcategory**</span></span>
  
      |<span data-ttu-id="a0daf-171">Sloupec</span><span class="sxs-lookup"><span data-stu-id="a0daf-171">Column</span></span>|  
      |-----------------------|  
      |<span data-ttu-id="a0daf-172">**SpanishProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-172">**SpanishProductSubcategoryName**</span></span>|  
      |<span data-ttu-id="a0daf-173">**FrenchProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="a0daf-173">**FrenchProductSubcategoryName**</span></span>|  
  
    <span data-ttu-id="a0daf-174">**FactInternetSales**</span><span class="sxs-lookup"><span data-stu-id="a0daf-174">**FactInternetSales**</span></span>
  
      |<span data-ttu-id="a0daf-175">Sloupec</span><span class="sxs-lookup"><span data-stu-id="a0daf-175">Column</span></span>|  
      |------------------|  
      |<span data-ttu-id="a0daf-176">**OrderDateKey**</span><span class="sxs-lookup"><span data-stu-id="a0daf-176">**OrderDateKey**</span></span>|  
      |<span data-ttu-id="a0daf-177">**DueDateKey**</span><span class="sxs-lookup"><span data-stu-id="a0daf-177">**DueDateKey**</span></span>|  
      |<span data-ttu-id="a0daf-178">**ShipDateKey**</span><span class="sxs-lookup"><span data-stu-id="a0daf-178">**ShipDateKey**</span></span>|   
  
## <span data-ttu-id="a0daf-179"><a name="Import"></a>Import hello vybrané tabulky a sloupce dat</span><span class="sxs-lookup"><span data-stu-id="a0daf-179"><a name="Import"></a>Import hello selected tables and column data</span></span>  
<span data-ttu-id="a0daf-180">Teď, když jste náhled a odfiltrovat nepotřebná data, můžete importovat hello zbytek hello data, která ho.</span><span class="sxs-lookup"><span data-stu-id="a0daf-180">Now that you've previewed and filtered out unnecessary data, you can import hello rest of hello data you do want.</span></span> <span data-ttu-id="a0daf-181">Hello Průvodce naimportuje data tabulky hello společně s všechny vztahy mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="a0daf-181">hello wizard imports hello table data along with any relationships between tables.</span></span> <span data-ttu-id="a0daf-182">Nové tabulky a sloupce, které jsou vytvořené v modelu hello a data, která můžete odfiltrovat není importován.</span><span class="sxs-lookup"><span data-stu-id="a0daf-182">New tables and columns are created in hello model and data that you filtered out is not be imported.</span></span>  
  
#### <a name="tooimport-hello-selected-tables-and-column-data"></a><span data-ttu-id="a0daf-183">tooimport hello vybrané tabulky a sloupce dat</span><span class="sxs-lookup"><span data-stu-id="a0daf-183">tooimport hello selected tables and column data</span></span>  
  
1.  <span data-ttu-id="a0daf-184">Zkontrolujte váš výběr.</span><span class="sxs-lookup"><span data-stu-id="a0daf-184">Review your selections.</span></span> <span data-ttu-id="a0daf-185">Pokud vše vypadá v pořádku, klikněte na **Importovat**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-185">If everything looks okay, click **Import**.</span></span> <span data-ttu-id="a0daf-186">Dialogové okno Hello zpracování dat se zobrazuje stav hello dat importovaných z zdroj dat do databáze pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="a0daf-186">hello Data Processing dialog shows hello status of data being imported from your datasource into your workspace database.</span></span>
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  <span data-ttu-id="a0daf-188">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-188">Click **Close**.</span></span>  

  
## <a name="save-your-model-project"></a><span data-ttu-id="a0daf-189">Uložení projektu s modelem</span><span class="sxs-lookup"><span data-stu-id="a0daf-189">Save your model project</span></span>  
<span data-ttu-id="a0daf-190">Je důležité toofrequently uložit projektu modelu.</span><span class="sxs-lookup"><span data-stu-id="a0daf-190">It's important toofrequently save your model project.</span></span>  
  
#### <a name="toosave-hello-model-project"></a><span data-ttu-id="a0daf-191">projekt modelu toosave hello</span><span class="sxs-lookup"><span data-stu-id="a0daf-191">toosave hello model project</span></span>  
  
-   <span data-ttu-id="a0daf-192">Klikněte na **Soubor** > **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="a0daf-192">Click **File** > **Save All**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="a0daf-193">Co dále?</span><span class="sxs-lookup"><span data-stu-id="a0daf-193">What's next?</span></span>
<span data-ttu-id="a0daf-194">[Lekce 3: Označení jako tabulky kalendářních dat](../tutorials/aas-lesson-3-mark-as-date-table.md)</span><span class="sxs-lookup"><span data-stu-id="a0daf-194">[Lesson 3: Mark as Date Table](../tutorials/aas-lesson-3-mark-as-date-table.md).</span></span>

  
  
