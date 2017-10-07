---
<span data-ttu-id="a65e2-101">Title: aaa "Azure Analysis Services kurz Lekce 5: vytvoření počítané sloupce | Microsoft Docs"Popis: Popisuje způsob výpočtu toocreate sloupců v kurzu projekt hello Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="a65e2-101">title: aaa"Azure Analysis Services tutorial lesson 5: Create calculated columns | Microsoft Docs" description: Describes how toocreate calculated columns in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="a65e2-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="a65e2-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="a65e2-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 01/06/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="a65e2-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---
# <a name="lesson-5-create-calculated-columns"></a><span data-ttu-id="a65e2-104">Lekce 5: Vytvoření počítaných sloupců</span><span class="sxs-lookup"><span data-stu-id="a65e2-104">Lesson 5: Create calculated columns</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="a65e2-105">V této lekci vytvoříte v modelu data přidáním počítaných sloupců.</span><span class="sxs-lookup"><span data-stu-id="a65e2-105">In this lesson, you create data in your model by adding calculated columns.</span></span> <span data-ttu-id="a65e2-106">Můžete vytvořit počítané sloupce (jako vlastní sloupce) při použití načíst Data, pomocí hello editoru dotazů, nebo později v hello modelu návrháře jako jste to zde.</span><span class="sxs-lookup"><span data-stu-id="a65e2-106">You can create calculated columns (as custom columns) when using Get Data, by using hello Query Editor, or later in hello model designer like you do here.</span></span> <span data-ttu-id="a65e2-107">Další, najdete v části toolearn [počítaných sloupců](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).</span><span class="sxs-lookup"><span data-stu-id="a65e2-107">toolearn more, see [Calculated columns](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).</span></span>
  
<span data-ttu-id="a65e2-108">Vytvoříte pět nových počítaných sloupců ve třech různých tabulkách.</span><span class="sxs-lookup"><span data-stu-id="a65e2-108">You create five new calculated columns in three different tables.</span></span> <span data-ttu-id="a65e2-109">Postup Hello je mírně odlišný pro každý úkol zobrazující existuje několik způsobů toocreate sloupců, přejmenujte je a umístit je na různých místech v tabulce.</span><span class="sxs-lookup"><span data-stu-id="a65e2-109">hello steps are slightly different for each task showing there are several ways toocreate columns, rename them, and place them in various locations in a table.</span></span>  

<span data-ttu-id="a65e2-110">V této lekci také poprvé použijete jazyk DAX (Data Analysis Expressions).</span><span class="sxs-lookup"><span data-stu-id="a65e2-110">This lesson is also where you first use Data Analysis Expressions (DAX).</span></span> <span data-ttu-id="a65e2-111">DAX je speciální jazyk pro vytváření vysoce přizpůsobitelných výrazů se vzorci pro tabelární modely.</span><span class="sxs-lookup"><span data-stu-id="a65e2-111">DAX is a special language for creating highly customizable formula expressions for tabular models.</span></span> <span data-ttu-id="a65e2-112">V tomto kurzu použijete toocreate počítaného sloupce, míry a role filtry jazyka DAX.</span><span class="sxs-lookup"><span data-stu-id="a65e2-112">In this tutorial, you use DAX toocreate calculated columns, measures, and role filters.</span></span> <span data-ttu-id="a65e2-113">Další, najdete v části toolearn [jazyka DAX v tabulkové modely](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="a65e2-113">toolearn more, see [DAX in tabular models](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular).</span></span> 
  
<span data-ttu-id="a65e2-114">Odhadovaný čas toocomplete této lekci: **15 minut**</span><span class="sxs-lookup"><span data-stu-id="a65e2-114">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="a65e2-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a65e2-115">Prerequisites</span></span>  
<span data-ttu-id="a65e2-116">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="a65e2-116">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="a65e2-117">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [Lekce 4: vytvoření vztahů](../tutorials/aas-lesson-4-create-relationships.md).</span><span class="sxs-lookup"><span data-stu-id="a65e2-117">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 4: Create relationships](../tutorials/aas-lesson-4-create-relationships.md).</span></span> 
  
## <a name="create-calculated-columns"></a><span data-ttu-id="a65e2-118">Vytvoření počítaných sloupců</span><span class="sxs-lookup"><span data-stu-id="a65e2-118">Create calculated columns</span></span>  
  
#### <a name="create-a-monthcalendar-calculated-column-in-hello-dimdate-table"></a><span data-ttu-id="a65e2-119">Vytvoření MonthCalendar počítaného sloupce v tabulce DimDate hello</span><span class="sxs-lookup"><span data-stu-id="a65e2-119">Create a MonthCalendar calculated column in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="a65e2-120">Klikněte na tlačítko hello **modelu** nabídky > **zobrazení modelu** > **zobrazení dat**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-120">Click hello **Model** menu > **Model View** > **Data View**.</span></span>  
  
    <span data-ttu-id="a65e2-121">Počítané sloupce lze vytvořit pouze pomocí návrháře hello model dat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a65e2-121">Calculated columns can only be created by using hello model designer in Data View.</span></span>  
  
2.  <span data-ttu-id="a65e2-122">V Návrháři hello model, klikněte na tlačítko hello **DimDate** tabulky (karta).</span><span class="sxs-lookup"><span data-stu-id="a65e2-122">In hello model designer, click hello **DimDate** table (tab).</span></span>  
  
3.  <span data-ttu-id="a65e2-123">Klikněte pravým tlačítkem na hello **CalendarQuarter** záhlaví sloupce a pak klikněte na tlačítko **vložit sloupec**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-123">Right-click hello **CalendarQuarter** column header, and then click **Insert Column**.</span></span>  
  
    <span data-ttu-id="a65e2-124">Nový sloupec s názvem **počítaného sloupce 1** je vložené toohello nalevo od hello **čtvrtletí kalendáře** sloupce.</span><span class="sxs-lookup"><span data-stu-id="a65e2-124">A new column named **Calculated Column 1** is inserted toohello left of hello **Calendar Quarter** column.</span></span>  
  
4.  <span data-ttu-id="a65e2-125">V řádku vzorců hello výše hello tabulky, zadejte následující vzorec DAX hello: automatického dokončování pomáhá zadáte hello plně kvalifikované názvy sloupců a tabulek a seznamů hello funkce, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a65e2-125">In hello formula bar above hello table, type hello following DAX formula: AutoComplete helps you type hello fully qualified names of columns and tables, and lists hello functions that are available.</span></span>  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    <span data-ttu-id="a65e2-126">Hodnoty jsou pak vyplněný pro všechny řádky hello v počítaném sloupci hello.</span><span class="sxs-lookup"><span data-stu-id="a65e2-126">Values are then populated for all hello rows in hello calculated column.</span></span> <span data-ttu-id="a65e2-127">Pokud přejděte dolů prostřednictvím tabulky hello zobrazí řádky může mít různé hodnoty pro tento sloupec na základě dat hello v jednotlivých řádcích.</span><span class="sxs-lookup"><span data-stu-id="a65e2-127">If you scroll down through hello table, you see rows can have different values for this column, based on hello data in each row.</span></span>    
  
5.  <span data-ttu-id="a65e2-128">Přejmenujte tento sloupec příliš**MonthCalendar**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-128">Rename this column too**MonthCalendar**.</span></span> 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
<span data-ttu-id="a65e2-130">počítaný sloupec Hello MonthCalendar poskytuje řazení název pro měsíc.</span><span class="sxs-lookup"><span data-stu-id="a65e2-130">hello MonthCalendar calculated column provides a sortable name for Month.</span></span>  
  
#### <a name="create-a-dayofweek-calculated-column-in-hello-dimdate-table"></a><span data-ttu-id="a65e2-131">Vytvoření DayOfWeek počítaného sloupce v tabulce DimDate hello</span><span class="sxs-lookup"><span data-stu-id="a65e2-131">Create a DayOfWeek calculated column in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="a65e2-132">S hello **DimDate** tabulky stále aktivní, klikněte na tlačítko hello **sloupec** nabídce a pak klikněte na tlačítko **přidat sloupec**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-132">With hello **DimDate** table still active, click hello **Column** menu, and then click **Add Column**.</span></span>  
  
2.  <span data-ttu-id="a65e2-133">V řádku vzorců hello zadejte hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="a65e2-133">In hello formula bar, type hello following formula:</span></span>  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    <span data-ttu-id="a65e2-134">Pokud jste dokončili vytváření hello vzorec, stiskněte klávesu ENTER.</span><span class="sxs-lookup"><span data-stu-id="a65e2-134">When you've finished building hello formula, press ENTER.</span></span> <span data-ttu-id="a65e2-135">nový sloupec Hello se přidá toohello nejvíce vpravo hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="a65e2-135">hello new column is added toohello far right of hello table.</span></span>  
  
3.  <span data-ttu-id="a65e2-136">Přejmenovat sloupec hello příliš**DayOfWeek**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-136">Rename hello column too**DayOfWeek**.</span></span>  
  
4.  <span data-ttu-id="a65e2-137">Kliknutím na záhlaví sloupce hello a poté přetáhněte hello sloupce mezi hello **EnglishDayNameOfWeek** sloupce a hello **DayNumberOfMonth** sloupce.</span><span class="sxs-lookup"><span data-stu-id="a65e2-137">Click hello column heading, and then drag hello column between hello **EnglishDayNameOfWeek** column and hello **DayNumberOfMonth** column.</span></span>  
  
    > [!TIP]  
    > <span data-ttu-id="a65e2-138">Přesunutí sloupců v tabulce je snazší toonavigate.</span><span class="sxs-lookup"><span data-stu-id="a65e2-138">Moving columns in your table makes it easier toonavigate.</span></span>  
  
<span data-ttu-id="a65e2-139">počítaný sloupec Hello DayOfWeek poskytuje řazení název hello den v týdnu.</span><span class="sxs-lookup"><span data-stu-id="a65e2-139">hello DayOfWeek calculated column provides a sortable name for hello day of week.</span></span>  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-hello-dimproduct-table"></a><span data-ttu-id="a65e2-140">Vytvoření ProductSubcategoryName počítaného sloupce v tabulce DimProduct hello</span><span class="sxs-lookup"><span data-stu-id="a65e2-140">Create a ProductSubcategoryName calculated column in hello DimProduct table</span></span>  
  
  
1.  <span data-ttu-id="a65e2-141">V hello **DimProduct** tabulky, posuňte toohello daleko vpravo od hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="a65e2-141">In hello **DimProduct** table, scroll toohello far right of hello table.</span></span> <span data-ttu-id="a65e2-142">Všimněte si hello nejvíce vpravo sloupec nazýval **přidat sloupec** (kurzívou), kliknutím na záhlaví sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="a65e2-142">Notice hello right-most column is named **Add Column** (italicized), click hello column heading.</span></span>  
  
2.  <span data-ttu-id="a65e2-143">V řádku vzorců hello zadejte hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="a65e2-143">In hello formula bar, type hello following formula:</span></span>  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  <span data-ttu-id="a65e2-144">Přejmenovat sloupec hello příliš**ProductSubcategoryName**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-144">Rename hello column too**ProductSubcategoryName**.</span></span>  
  
<span data-ttu-id="a65e2-145">počítaný sloupec ProductSubcategoryName Hello je použité toocreate hierarchie v tabulce DimProduct hello, které zahrnují data z hello EnglishProductSubcategoryName sloupec v tabulce DimProductSubcategory hello.</span><span class="sxs-lookup"><span data-stu-id="a65e2-145">hello ProductSubcategoryName calculated column is used toocreate a hierarchy in hello DimProduct table, which includes data from hello EnglishProductSubcategoryName column in hello DimProductSubcategory table.</span></span> <span data-ttu-id="a65e2-146">Hierarchie nemůžou zahrnovat víc než jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="a65e2-146">Hierarchies cannot span more than one table.</span></span> <span data-ttu-id="a65e2-147">Hierarchie vytvoříte později v lekci 9.</span><span class="sxs-lookup"><span data-stu-id="a65e2-147">You create hierarchies later in Lesson 9.</span></span>  
  
#### <a name="create-a-productcategoryname-calculated-column-in-hello-dimproduct-table"></a><span data-ttu-id="a65e2-148">Vytvoření ProductCategoryName počítaného sloupce v tabulce DimProduct hello</span><span class="sxs-lookup"><span data-stu-id="a65e2-148">Create a ProductCategoryName calculated column in hello DimProduct table</span></span>  
  
1.  <span data-ttu-id="a65e2-149">S hello **DimProduct** tabulky stále aktivní, klikněte na tlačítko hello **sloupec** nabídce a pak klikněte na tlačítko **přidat sloupec**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-149">With hello **DimProduct** table still active, click hello **Column** menu, and then click **Add Column**.</span></span>  
  
2.  <span data-ttu-id="a65e2-150">V řádku vzorců hello zadejte hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="a65e2-150">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  <span data-ttu-id="a65e2-151">Přejmenovat sloupec hello příliš**ProductCategoryName**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-151">Rename hello column too**ProductCategoryName**.</span></span>  
  
<span data-ttu-id="a65e2-152">počítaný sloupec ProductCategoryName Hello je použité toocreate hierarchie v tabulce DimProduct hello, které zahrnují data z hello EnglishProductCategoryName sloupec v tabulce DimProductCategory hello.</span><span class="sxs-lookup"><span data-stu-id="a65e2-152">hello ProductCategoryName calculated column is used toocreate a hierarchy in hello DimProduct table, which includes data from hello EnglishProductCategoryName column in hello DimProductCategory table.</span></span> <span data-ttu-id="a65e2-153">Hierarchie nemůžou zahrnovat víc než jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="a65e2-153">Hierarchies cannot span more than one table.</span></span>  
  
#### <a name="create-a-margin-calculated-column-in-hello-factinternetsales-table"></a><span data-ttu-id="a65e2-154">Vytvoření okraj počítaného sloupce v tabulce FactInternetSales hello</span><span class="sxs-lookup"><span data-stu-id="a65e2-154">Create a Margin calculated column in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="a65e2-155">V Návrháři hello modelu, vyberte hello **FactInternetSales** tabulky.</span><span class="sxs-lookup"><span data-stu-id="a65e2-155">In hello model designer, select hello **FactInternetSales** table.</span></span>  
  
2.  <span data-ttu-id="a65e2-156">Vytvořit nový počítaný sloupec mezi hello **SalesAmount** sloupce a hello **TaxAmt** sloupce.</span><span class="sxs-lookup"><span data-stu-id="a65e2-156">Create a new calculated column between hello **SalesAmount** column and hello **TaxAmt** column.</span></span>  
  
3.  <span data-ttu-id="a65e2-157">V řádku vzorců hello zadejte hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="a65e2-157">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  <span data-ttu-id="a65e2-158">Přejmenovat sloupec hello příliš**okraj**.</span><span class="sxs-lookup"><span data-stu-id="a65e2-158">Rename hello column too**Margin**.</span></span>  
 
      ![aas-lesson5-newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    <span data-ttu-id="a65e2-160">počítaný sloupec Hello okraj je použité tooanalyze zisku okrajů pro každý prodej.</span><span class="sxs-lookup"><span data-stu-id="a65e2-160">hello Margin calculated column is used tooanalyze profit margins for each sale.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="a65e2-161">Co dále?</span><span class="sxs-lookup"><span data-stu-id="a65e2-161">What's next?</span></span>
<span data-ttu-id="a65e2-162">[Lekce 6: Vytvoření měřítek](../tutorials/aas-lesson-6-create-measures.md)</span><span class="sxs-lookup"><span data-stu-id="a65e2-162">[Lesson 6: Create measures](../tutorials/aas-lesson-6-create-measures.md).</span></span>
  
  
  
