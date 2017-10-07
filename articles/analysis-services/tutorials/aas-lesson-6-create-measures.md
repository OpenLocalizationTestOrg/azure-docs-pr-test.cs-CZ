---
<span data-ttu-id="fd6cb-101">Title: aaa "Azure Analysis Services kurz lekce 6: vytvoření míry | Microsoft Docs"Popis: Popisuje, jak toocreate měří v kurzu projektu hello Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-101">title: aaa"Azure Analysis Services tutorial lesson 6: Create measures | Microsoft Docs" description: Describes how toocreate measures in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="fd6cb-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="fd6cb-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="fd6cb-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 01/06/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="fd6cb-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---
# <a name="lesson-6-create-measures"></a><span data-ttu-id="fd6cb-104">Lekce 6: Vytvoření měr</span><span class="sxs-lookup"><span data-stu-id="fd6cb-104">Lesson 6: Create measures</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="fd6cb-105">V této lekci vytvoříte míry toobe součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-105">In this lesson, you create measures toobe included in your model.</span></span> <span data-ttu-id="fd6cb-106">Podobně jako toohello počítaného sloupce, které jste vytvořili, míry je výpočet vytvořili pomocí vzorce DAX.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-106">Similar toohello calculated columns you created, a measure is a calculation created by using a DAX formula.</span></span> <span data-ttu-id="fd6cb-107">Na rozdíl od počítaných sloupců se ale míry vyhodnocují na základě uživatelem vybraného *filtru*.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-107">However, unlike calculated columns, measures are evaluated based on a user selected *filter*.</span></span> <span data-ttu-id="fd6cb-108">Například konkrétní sloupec nebo průřez přidat pole toohello popisky řádků v kontingenční tabulce.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-108">For example, a particular column or slicer added toohello Row Labels field in a PivotTable.</span></span> <span data-ttu-id="fd6cb-109">Hodnotu pro každou buňku v hello filtru se pak počítá hello použít míru.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-109">A value for each cell in hello filter is then calculated by hello applied measure.</span></span> <span data-ttu-id="fd6cb-110">Míry jsou efektivní, flexibilní výpočty, které chcete tooinclude v téměř všechny tabulkové modely tooperform dynamické výpočty v číselná data.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-110">Measures are powerful, flexible calculations that you want tooinclude in almost all tabular models tooperform dynamic calculations on numerical data.</span></span> <span data-ttu-id="fd6cb-111">Další, najdete v části toolearn [míry](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="fd6cb-111">toolearn more, see [Measures](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).</span></span>
  
<span data-ttu-id="fd6cb-112">toocreate míry, použijte hello *mřížce měr*.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-112">toocreate measures, you use hello *Measure Grid*.</span></span> <span data-ttu-id="fd6cb-113">Ve výchozím nastavení má každá tabulka prázdnou mřížku měr. Obvykle ale nevytváříte míry pro každou tabulku.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-113">By default, each table has an empty measure grid; however, you typically do not create measures for every table.</span></span> <span data-ttu-id="fd6cb-114">níže uvedená tabulka v Návrháři hello model v zobrazení dat se zobrazí Hello měr mřížky.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-114">hello measure grid appears below a table in hello model designer when in Data View.</span></span> <span data-ttu-id="fd6cb-115">toohide nebo zobrazit mřížky měr hello pro tabulku, klikněte na tlačítko hello **tabulky** nabídce a pak klikněte na tlačítko **zobrazit mřížky měr**.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-115">toohide or show hello measure grid for a table, click hello **Table** menu, and then click **Show Measure Grid**.</span></span>  
  
<span data-ttu-id="fd6cb-116">Míra můžete vytvořit tak, že kliknete na prázdné buňky v mřížce měr hello a poté zadat vzorec jazyka DAX do řádku vzorců hello.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-116">You can create a measure by clicking an empty cell in hello measure grid, and then typing a DAX formula in hello formula bar.</span></span> <span data-ttu-id="fd6cb-117">Když klikněte ENTER toocomplete hello vzorec, hello měr, pak se zobrazí v buňce hello.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-117">When you click ENTER toocomplete hello formula, hello measure then appears in hello cell.</span></span> <span data-ttu-id="fd6cb-118">Můžete vytvořit také pomocí kliknutím na sloupec standardní agregační funkce míry, a potom kliknutím na tlačítko AutoSum hello (**∑**) na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-118">You can also create measures using a standard aggregation function by clicking a column, and then clicking hello AutoSum button (**∑**) on hello toolbar.</span></span> <span data-ttu-id="fd6cb-119">Míry vytvořený pomocí funkce AutoSum hello objeví v buňky v mřížce měr hello přímo pod hello sloupec, ale lze přesunout.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-119">Measures created using hello AutoSum feature appear in hello measure grid cell directly beneath hello column, but can be moved.</span></span>  
  
<span data-ttu-id="fd6cb-120">V této lekci vytvoříte míry zadáním obou vzorec jazyka DAX do řádku vzorců hello a pomocí funkce AutoSum hello.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-120">In this lesson, you create measures by both entering a DAX formula in hello formula bar, and by using hello AutoSum feature.</span></span>  
  
<span data-ttu-id="fd6cb-121">Odhadovaný čas toocomplete této lekci: **30 minut**</span><span class="sxs-lookup"><span data-stu-id="fd6cb-121">Estimated time toocomplete this lesson: **30 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="fd6cb-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fd6cb-122">Prerequisites</span></span>  
<span data-ttu-id="fd6cb-123">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-123">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="fd6cb-124">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [Lekce 5: vytvoření počítané sloupce](../tutorials/aas-lesson-5-create-calculated-columns.md).</span><span class="sxs-lookup"><span data-stu-id="fd6cb-124">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 5: Create calculated columns](../tutorials/aas-lesson-5-create-calculated-columns.md).</span></span>  
  
## <a name="create-measures"></a><span data-ttu-id="fd6cb-125">Vytvoření měr</span><span class="sxs-lookup"><span data-stu-id="fd6cb-125">Create measures</span></span>  
  
#### <a name="toocreate-a-dayscurrentquartertodate-measure-in-hello-dimdate-table"></a><span data-ttu-id="fd6cb-126">toocreate DaysCurrentQuarterToDate měr v tabulce DimDate hello</span><span class="sxs-lookup"><span data-stu-id="fd6cb-126">toocreate a DaysCurrentQuarterToDate measure in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="fd6cb-127">V Návrháři hello model, klikněte na tlačítko hello **DimDate** tabulky.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-127">In hello model designer, click hello **DimDate** table.</span></span>  
  
2.  <span data-ttu-id="fd6cb-128">V mřížce měr hello klikněte na tlačítko hello levého horního prázdné buňky.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-128">In hello measure grid, click hello top-left empty cell.</span></span>  
  
3.  <span data-ttu-id="fd6cb-129">V řádku vzorců hello zadejte hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="fd6cb-129">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    <span data-ttu-id="fd6cb-130">Všimněte si hello levého horního buňka teď obsahuje název míry, **DaysCurrentQuarterToDate**, za nímž následují hello výsledek **92**.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-130">Notice hello top-left cell now contains a measure name, **DaysCurrentQuarterToDate**, followed by hello result, **92**.</span></span>
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    <span data-ttu-id="fd6cb-132">Na rozdíl od počítané sloupce se vzorci měr můžete zadat hello název míry, následovaným dvojtečkou, následuje hello vzorce výraz.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-132">Unlike calculated columns, with measure formulas you can type hello measure name, followed by a colon, followed by hello formula expression.</span></span>

  
#### <a name="toocreate-a-daysincurrentquarter-measure-in-hello-dimdate-table"></a><span data-ttu-id="fd6cb-133">toocreate DaysInCurrentQuarter měr v tabulce DimDate hello</span><span class="sxs-lookup"><span data-stu-id="fd6cb-133">toocreate a DaysInCurrentQuarter measure in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="fd6cb-134">S hello **DimDate** tabulky v Návrháři hello modelu, v mřížce měr hello stále aktivní, klikněte na tlačítko hello prázdné buňky pod hello měr, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-134">With hello **DimDate** table still active in hello model designer, in hello measure grid, click hello empty cell below hello measure you created.</span></span>  
  
2.  <span data-ttu-id="fd6cb-135">V řádku vzorců hello zadejte hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="fd6cb-135">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    <span data-ttu-id="fd6cb-136">Při vytváření porovnání poměr mezi jeden neúplné období a hello předchozího období.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-136">When creating a comparison ratio between one incomplete period and hello previous period.</span></span> <span data-ttu-id="fd6cb-137">Vzorec Hello musí vypočítat hello podíl hello období, kdy a porovnávají ho toohello stejné deklarovaná čistá nebo hrubá v hello předchozího období.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-137">hello formula must calculate hello proportion of hello period that has elapsed and compare it toohello same proportion in hello previous period.</span></span> <span data-ttu-id="fd6cb-138">V tomto případě [DaysCurrentQuarterToDate] / [DaysInCurrentQuarter] poskytuje hello část uplynulý čas v hello aktuální období.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-138">In this case, [DaysCurrentQuarterToDate]/[DaysInCurrentQuarter] gives hello proportion elapsed in hello current period.</span></span>  
  
#### <a name="toocreate-an-internetdistinctcountsalesorder-measure-in-hello-factinternetsales-table"></a><span data-ttu-id="fd6cb-139">toocreate InternetDistinctCountSalesOrder měr v tabulka FactInternetSales hello</span><span class="sxs-lookup"><span data-stu-id="fd6cb-139">toocreate an InternetDistinctCountSalesOrder measure in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="fd6cb-140">Klikněte na tlačítko hello **FactInternetSales** tabulky.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-140">Click hello **FactInternetSales** table.</span></span>   
  
2.  <span data-ttu-id="fd6cb-141">Klikněte na tlačítko hello **SalesOrderNumber** záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-141">Click hello **SalesOrderNumber** column heading.</span></span>  
  
3.  <span data-ttu-id="fd6cb-142">Na panelu nástrojů hello, klikněte na tlačítko Další toohello šipky dolů hello AutoSum (**∑**) tlačítko a potom vyberte **DistinctCount**.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-142">On hello toolbar, click hello down-arrow next toohello AutoSum (**∑**) button, and then select **DistinctCount**.</span></span>  
  
    <span data-ttu-id="fd6cb-143">funkce AutoSum Hello automaticky vytvoří míru pomocí vzorce standardní agregace DistinctCount hello vybrané sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-143">hello AutoSum feature automatically creates a measure for hello selected column using hello DistinctCount standard aggregation formula.</span></span>  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  <span data-ttu-id="fd6cb-145">V mřížce měr hello, klikněte na novou measure hello a potom v hello **vlastnosti** okno v **název míry**, přejmenujte hello míra příliš**InternetDistinctCountSalesOrder**.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-145">In hello measure grid, click hello new measure, and then in hello **Properties** window, in **Measure Name**, rename hello measure too**InternetDistinctCountSalesOrder**.</span></span> 
 
  
#### <a name="toocreate-additional-measures-in-hello-factinternetsales-table"></a><span data-ttu-id="fd6cb-146">toocreate další míry v tabulka FactInternetSales hello</span><span class="sxs-lookup"><span data-stu-id="fd6cb-146">toocreate additional measures in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="fd6cb-147">Pomocí funkce AutoSum hello vytvořte a pojmenujte hello následující míry:</span><span class="sxs-lookup"><span data-stu-id="fd6cb-147">By using hello AutoSum feature, create and name hello following measures:</span></span>  

    |<span data-ttu-id="fd6cb-148">Sloupec</span><span class="sxs-lookup"><span data-stu-id="fd6cb-148">Column</span></span>|<span data-ttu-id="fd6cb-149">Název míry</span><span class="sxs-lookup"><span data-stu-id="fd6cb-149">Measure name</span></span>|<span data-ttu-id="fd6cb-150">AutoSum (∑)</span><span class="sxs-lookup"><span data-stu-id="fd6cb-150">AutoSum (∑)</span></span>|<span data-ttu-id="fd6cb-151">Vzorec</span><span class="sxs-lookup"><span data-stu-id="fd6cb-151">Formula</span></span>|  
    |----------------|----------|-----------------|-----------|  
    |<span data-ttu-id="fd6cb-152">SalesOrderLineNumber</span><span class="sxs-lookup"><span data-stu-id="fd6cb-152">SalesOrderLineNumber</span></span>|<span data-ttu-id="fd6cb-153">InternetOrderLinesCount</span><span class="sxs-lookup"><span data-stu-id="fd6cb-153">InternetOrderLinesCount</span></span>|<span data-ttu-id="fd6cb-154">Počet</span><span class="sxs-lookup"><span data-stu-id="fd6cb-154">Count</span></span>|<span data-ttu-id="fd6cb-155">=POČET2([SalesOrderLineNumber])</span><span class="sxs-lookup"><span data-stu-id="fd6cb-155">=COUNTA([SalesOrderLineNumber])</span></span>|  
    |<span data-ttu-id="fd6cb-156">OrderQuantity</span><span class="sxs-lookup"><span data-stu-id="fd6cb-156">OrderQuantity</span></span>|<span data-ttu-id="fd6cb-157">InternetTotalUnits</span><span class="sxs-lookup"><span data-stu-id="fd6cb-157">InternetTotalUnits</span></span>|<span data-ttu-id="fd6cb-158">Součet</span><span class="sxs-lookup"><span data-stu-id="fd6cb-158">Sum</span></span>|<span data-ttu-id="fd6cb-159">=SUMA([OrderQuantity])</span><span class="sxs-lookup"><span data-stu-id="fd6cb-159">=SUM([OrderQuantity])</span></span>|  
    |<span data-ttu-id="fd6cb-160">DiscountAmount</span><span class="sxs-lookup"><span data-stu-id="fd6cb-160">DiscountAmount</span></span>|<span data-ttu-id="fd6cb-161">InternetTotalDiscountAmount</span><span class="sxs-lookup"><span data-stu-id="fd6cb-161">InternetTotalDiscountAmount</span></span>|<span data-ttu-id="fd6cb-162">Součet</span><span class="sxs-lookup"><span data-stu-id="fd6cb-162">Sum</span></span>|<span data-ttu-id="fd6cb-163">=SUMA([DiscountAmount])</span><span class="sxs-lookup"><span data-stu-id="fd6cb-163">=SUM([DiscountAmount])</span></span>|  
    |<span data-ttu-id="fd6cb-164">TotalProductCost</span><span class="sxs-lookup"><span data-stu-id="fd6cb-164">TotalProductCost</span></span>|<span data-ttu-id="fd6cb-165">InternetTotalProductCost</span><span class="sxs-lookup"><span data-stu-id="fd6cb-165">InternetTotalProductCost</span></span>|<span data-ttu-id="fd6cb-166">Součet</span><span class="sxs-lookup"><span data-stu-id="fd6cb-166">Sum</span></span>|<span data-ttu-id="fd6cb-167">=SUMA([TotalProductCost])</span><span class="sxs-lookup"><span data-stu-id="fd6cb-167">=SUM([TotalProductCost])</span></span>|  
    |<span data-ttu-id="fd6cb-168">SalesAmount</span><span class="sxs-lookup"><span data-stu-id="fd6cb-168">SalesAmount</span></span>|<span data-ttu-id="fd6cb-169">InternetTotalSales</span><span class="sxs-lookup"><span data-stu-id="fd6cb-169">InternetTotalSales</span></span>|<span data-ttu-id="fd6cb-170">Součet</span><span class="sxs-lookup"><span data-stu-id="fd6cb-170">Sum</span></span>|<span data-ttu-id="fd6cb-171">=SUMA([SalesAmount])</span><span class="sxs-lookup"><span data-stu-id="fd6cb-171">=SUM([SalesAmount])</span></span>|  
    |<span data-ttu-id="fd6cb-172">Margin</span><span class="sxs-lookup"><span data-stu-id="fd6cb-172">Margin</span></span>|<span data-ttu-id="fd6cb-173">InternetTotalMargin</span><span class="sxs-lookup"><span data-stu-id="fd6cb-173">InternetTotalMargin</span></span>|<span data-ttu-id="fd6cb-174">Součet</span><span class="sxs-lookup"><span data-stu-id="fd6cb-174">Sum</span></span>|<span data-ttu-id="fd6cb-175">=SUMA([Margin])</span><span class="sxs-lookup"><span data-stu-id="fd6cb-175">=SUM([Margin])</span></span>|  
    |<span data-ttu-id="fd6cb-176">TaxAmt</span><span class="sxs-lookup"><span data-stu-id="fd6cb-176">TaxAmt</span></span>|<span data-ttu-id="fd6cb-177">InternetTotalTaxAmt</span><span class="sxs-lookup"><span data-stu-id="fd6cb-177">InternetTotalTaxAmt</span></span>|<span data-ttu-id="fd6cb-178">Součet</span><span class="sxs-lookup"><span data-stu-id="fd6cb-178">Sum</span></span>|<span data-ttu-id="fd6cb-179">=SUMA([TaxAmt])</span><span class="sxs-lookup"><span data-stu-id="fd6cb-179">=SUM([TaxAmt])</span></span>|  
    |<span data-ttu-id="fd6cb-180">Freight</span><span class="sxs-lookup"><span data-stu-id="fd6cb-180">Freight</span></span>|<span data-ttu-id="fd6cb-181">InternetTotalFreight</span><span class="sxs-lookup"><span data-stu-id="fd6cb-181">InternetTotalFreight</span></span>|<span data-ttu-id="fd6cb-182">Součet</span><span class="sxs-lookup"><span data-stu-id="fd6cb-182">Sum</span></span>|<span data-ttu-id="fd6cb-183">=SUMA([Freight])</span><span class="sxs-lookup"><span data-stu-id="fd6cb-183">=SUM([Freight])</span></span>|  
  
2.  <span data-ttu-id="fd6cb-184">Kliknutím na vytvořit prázdné buňky v mřížce měr hello a pomocí hello řádku vzorců, a název hello následující měří v pořadí:</span><span class="sxs-lookup"><span data-stu-id="fd6cb-184">By clicking an empty cell in hello measure grid, and by using hello formula bar, create, and name hello following measures in order:</span></span>  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
<span data-ttu-id="fd6cb-185">Míry vytvořené pro tabulka FactInternetSales hello může být důležité finanční údaje použité tooanalyze například prodej, náklady a zisku pro položky definované hello uživatele vybraného filtru.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-185">Measures created for hello FactInternetSales table can be used tooanalyze critical financial data such as sales, costs, and profit margin for items defined by hello user selected filter.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="fd6cb-186">Co dále?</span><span class="sxs-lookup"><span data-stu-id="fd6cb-186">What's next?</span></span>
<span data-ttu-id="fd6cb-187">[Lekce 7: Vytvoření klíčových ukazatelů výkonu](../tutorials/aas-lesson-7-create-key-performance-indicators.md)</span><span class="sxs-lookup"><span data-stu-id="fd6cb-187">[Lesson 7: Create Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span></span>  

  
