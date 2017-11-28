---
<span data-ttu-id="66188-101">Title: aaa "kurz dodatečné lekce Azure Analysis Services: řádky s podrobnostmi | Microsoft Docs"Popis: Popisuje, jak toocreate a výraz řádky podrobností v hello kurz služby Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="66188-101">title: aaa"Azure Analysis Services tutorial supplemental lesson: Detail Rows | Microsoft Docs" description: Describes how toocreate a Detail Rows Expression in hello Azure Analysis Services tutorial.</span></span>
<span data-ttu-id="66188-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="66188-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="66188-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="66188-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="supplemental-lesson---detail-rows"></a><span data-ttu-id="66188-104">Doplňková lekce – Řádky podrobností</span><span class="sxs-lookup"><span data-stu-id="66188-104">Supplemental lesson - Detail Rows</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="66188-105">V této další lekci použijete hello toodefine DAX Editor vlastního výrazu řádky podrobností.</span><span class="sxs-lookup"><span data-stu-id="66188-105">In this supplemental lesson, you use hello DAX Editor toodefine a custom Detail Rows Expression.</span></span> <span data-ttu-id="66188-106">Výraz řádky podrobností je vlastnost na míru, poskytuje koncovým uživatelům další informace o výsledcích hello agregovat míry.</span><span class="sxs-lookup"><span data-stu-id="66188-106">A Detail Rows Expression is a property on a measure, providing end-users more information about hello aggregated results of a measure.</span></span> 
  
<span data-ttu-id="66188-107">Odhadovaný čas toocomplete této lekci: **10 minut**</span><span class="sxs-lookup"><span data-stu-id="66188-107">Estimated time toocomplete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="66188-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="66188-108">Prerequisites</span></span>  
<span data-ttu-id="66188-109">Toto téma doplňkové lekce je součástí kurzu tabulkového modelování.</span><span class="sxs-lookup"><span data-stu-id="66188-109">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="66188-110">Před provedením úlohy hello v této další lekci, by byly dokončeny všechny předchozí lekce nebo máte dokončený projekt modelu ukázkové společnosti Adventure Works Internet prodej.</span><span class="sxs-lookup"><span data-stu-id="66188-110">Before performing hello tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span>  
  
## <a name="what-do-we-need-toosolve"></a><span data-ttu-id="66188-111">Co dělat, potřebujeme toosolve?</span><span class="sxs-lookup"><span data-stu-id="66188-111">What do we need toosolve?</span></span>
<span data-ttu-id="66188-112">Podívejme se na podrobnosti hello míry naše InternetTotalSales před přidáním výraz řádky podrobností.</span><span class="sxs-lookup"><span data-stu-id="66188-112">Let's look at hello details of our InternetTotalSales measure, before adding a Detail Rows Expression.</span></span>

1.  <span data-ttu-id="66188-113">V sadě SSDT, klikněte na tlačítko hello **modelu** nabídky > **analyzovat v aplikaci Excel** tooopen aplikace Excel a vytvořit prázdnou kontingenční tabulku.</span><span class="sxs-lookup"><span data-stu-id="66188-113">In SSDT, click hello **Model** menu > **Analyze in Excel** tooopen Excel and create a blank PivotTable.</span></span>
  
2.  <span data-ttu-id="66188-114">V **pole kontingenční tabulky**, přidejte hello **InternetTotalSales** měření z tabulka FactInternetSales hello příliš**hodnoty**, **CalendarYear**z hello DimDate tabulky příliš**sloupce**, a **EnglishCountryRegionName** příliš**řádky**.</span><span class="sxs-lookup"><span data-stu-id="66188-114">In **PivotTable Fields**, add hello **InternetTotalSales** measure from hello FactInternetSales table too**Values**, **CalendarYear** from hello DimDate table too**Columns**, and **EnglishCountryRegionName** too**Rows**.</span></span> <span data-ttu-id="66188-115">Naše kontingenční tabulky nyní umožňuje nám agregované výsledky z měr InternetTotalSales hello oblasti a roku.</span><span class="sxs-lookup"><span data-stu-id="66188-115">Our PivotTable now gives us aggregated results from hello InternetTotalSales measure by regions and year.</span></span> 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. <span data-ttu-id="66188-117">V hello kontingenční tabulky klikněte dvakrát na agregované hodnoty pro název oblasti a roce.</span><span class="sxs-lookup"><span data-stu-id="66188-117">In hello PivotTable, double-click an aggregated value for a year and a region name.</span></span> <span data-ttu-id="66188-118">Zde jsme dvakrát kliknuli hello hodnotu pro Austrálii a hello rok 2014.</span><span class="sxs-lookup"><span data-stu-id="66188-118">Here we double-clicked hello value for Australia and hello year 2014.</span></span> <span data-ttu-id="66188-119">Otevře se nový list, který obsahuje data. Tato data ale nejsou moc užitečná.</span><span class="sxs-lookup"><span data-stu-id="66188-119">A new sheet opens containing data, but not useful data.</span></span>

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
<span data-ttu-id="66188-121">Co jsme chcete toosee tady je tabulku obsahující sloupce a řádky dat, která přispívat toohello agregován výsledek naše InternetTotalSales míry.</span><span class="sxs-lookup"><span data-stu-id="66188-121">What we would like toosee here is a table containing columns and rows of data that contribute toohello aggregated result of our InternetTotalSales measure.</span></span> <span data-ttu-id="66188-122">toodo, že jsme přidat výraz řádky podrobností jako vlastnost míry hello.</span><span class="sxs-lookup"><span data-stu-id="66188-122">toodo that, we can add a Detail Rows Expression as a property of hello measure.</span></span>

## <a name="add-a-detail-rows-expression"></a><span data-ttu-id="66188-123">Přidání výrazu řádků podrobností</span><span class="sxs-lookup"><span data-stu-id="66188-123">Add a Detail Rows Expression</span></span>

#### <a name="toocreate-a-detail-rows-expression"></a><span data-ttu-id="66188-124">toocreate výraz řádky podrobností</span><span class="sxs-lookup"><span data-stu-id="66188-124">toocreate a Detail Rows Expression</span></span> 
  
1. <span data-ttu-id="66188-125">V sadě SSDT, v mřížce měr tabulka FactInternetSales hello, klikněte na tlačítko hello **InternetTotalSales** měr.</span><span class="sxs-lookup"><span data-stu-id="66188-125">In SSDT, in hello FactInternetSales table's measure grid, click hello **InternetTotalSales** measure.</span></span> 

2. <span data-ttu-id="66188-126">V **vlastnosti** > **výraz řádky podrobností**, klikněte na tlačítko hello editor tlačítko tooopen hello editoru jazyka DAX.</span><span class="sxs-lookup"><span data-stu-id="66188-126">In **Properties** > **Detail Rows Expression**, click hello editor button tooopen hello DAX Editor.</span></span>

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. <span data-ttu-id="66188-128">V editoru jazyka DAX, zadejte následující výraz hello:</span><span class="sxs-lookup"><span data-stu-id="66188-128">In DAX Editor, enter hello following expression:</span></span>

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    <span data-ttu-id="66188-129">Tento výraz určuje názvy sloupců a měr výsledky z tabulka FactInternetSales hello a související tabulky se vrátí při poklepání výsledek agregované v kontingenční tabulce nebo sestavy.</span><span class="sxs-lookup"><span data-stu-id="66188-129">This expression specifies names, columns, and measure results from hello FactInternetSales table and related tables are returned when a user double-clicks an aggregated result in a PivotTable or report.</span></span>

4. <span data-ttu-id="66188-130">Zpět v aplikaci Excel odstranit list hello vytvořili v kroku 3, pak poklikejte na agregovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="66188-130">Back in Excel, delete hello sheet created in Step 3, then double-click an aggregated value.</span></span> <span data-ttu-id="66188-131">Tentokrát s výraz řádky podrobností vlastnost definovanou pro míru hello, nový list otevře obsahující mnoho užitečnější data.</span><span class="sxs-lookup"><span data-stu-id="66188-131">This time, with a Detail Rows Expression property defined for hello measure, a new sheet opens containing a lot more useful data.</span></span>

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. <span data-ttu-id="66188-133">Znovu nasaďte model.</span><span class="sxs-lookup"><span data-stu-id="66188-133">Redeploy your model.</span></span>

  
## <a name="see-also"></a><span data-ttu-id="66188-134">Viz také</span><span class="sxs-lookup"><span data-stu-id="66188-134">See Also</span></span>  
<span data-ttu-id="66188-135">[Funkce SELECTCOLUMNS (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span><span class="sxs-lookup"><span data-stu-id="66188-135">[SELECTCOLUMNS Function (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span></span>  
[<span data-ttu-id="66188-136">Doplňková lekce – Dynamické zabezpečení</span><span class="sxs-lookup"><span data-stu-id="66188-136">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="66188-137">Doplňková lekce – Nepravidelné hierarchie</span><span class="sxs-lookup"><span data-stu-id="66188-137">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
