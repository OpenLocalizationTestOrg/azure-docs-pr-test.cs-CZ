---
<span data-ttu-id="833cf-101">Title: aaa "Azure Analysis Services kurz Lekce 7: vytvoření klíčové ukazatele výkonu | Microsoft Docs"Popis: Popisuje, jak hello toocreate klíčových ukazatelů výkonu v kurzu projektu Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="833cf-101">title: aaa"Azure Analysis Services tutorial lesson 7: Create Key Performance Indicators | Microsoft Docs" description: Describes how toocreate Key Performance Indicators in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="833cf-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="833cf-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="833cf-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="833cf-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-7-create-key-performance-indicators"></a><span data-ttu-id="833cf-104">Lekce 7: Vytvoření klíčových ukazatelů výkonu</span><span class="sxs-lookup"><span data-stu-id="833cf-104">Lesson 7: Create Key Performance Indicators</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="833cf-105">V této lekci vytvoříte klíčové ukazatele výkonu (KPI).</span><span class="sxs-lookup"><span data-stu-id="833cf-105">In this lesson, you create Key Performance Indicators (KPIs).</span></span> <span data-ttu-id="833cf-106">Klíčové ukazatele výkonu představují používané toogauge výkonu definované hodnoty *základní* míry, proti *cíl* hodnotu také definovat míru, nebo absolutní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="833cf-106">KPIs are used toogauge performance of a value defined by a *Base* measure, against a *Target* value also defined by a measure, or by an absolute value.</span></span> <span data-ttu-id="833cf-107">V sestavách klientské aplikace, nabízejí klíčových ukazatelů výkonu obchodní specialisty toounderstand rychlý a snadný způsob shrnutí obchodních úspěch nebo tooidentify trendů.</span><span class="sxs-lookup"><span data-stu-id="833cf-107">In reporting client applications, KPIs can provide business professionals a quick and easy way toounderstand a summary of business success or tooidentify trends.</span></span> <span data-ttu-id="833cf-108">Další, najdete v části toolearn [klíčové ukazatele výkonu.](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)</span><span class="sxs-lookup"><span data-stu-id="833cf-108">toolearn more, see [KPIs](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)</span></span>
  
<span data-ttu-id="833cf-109">Odhadovaný čas toocomplete této lekci: **15 minut**</span><span class="sxs-lookup"><span data-stu-id="833cf-109">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="833cf-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="833cf-110">Prerequisites</span></span>  
<span data-ttu-id="833cf-111">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="833cf-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="833cf-112">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 6: vytvoření míry](../tutorials/aas-lesson-6-create-measures.md).</span><span class="sxs-lookup"><span data-stu-id="833cf-112">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 6: Create measures](../tutorials/aas-lesson-6-create-measures.md).</span></span>   
  
## <a name="create-key-performance-indicators"></a><span data-ttu-id="833cf-113">Vytvoření klíčových ukazatelů výkonu</span><span class="sxs-lookup"><span data-stu-id="833cf-113">Create Key Performance Indicators</span></span>  
  
#### <a name="toocreate-an-internetcurrentquartersalesperformance-kpi"></a><span data-ttu-id="833cf-114">toocreate InternetCurrentQuarterSalesPerformance klíčového ukazatele výkonu</span><span class="sxs-lookup"><span data-stu-id="833cf-114">toocreate an InternetCurrentQuarterSalesPerformance KPI</span></span>  
  
1.  <span data-ttu-id="833cf-115">V Návrháři hello model, klikněte na tlačítko hello **FactInternetSales** tabulky.</span><span class="sxs-lookup"><span data-stu-id="833cf-115">In hello model designer, click hello **FactInternetSales** table.</span></span>  
  
2.  <span data-ttu-id="833cf-116">V mřížce měr hello klikněte na tlačítko prázdné buňky.</span><span class="sxs-lookup"><span data-stu-id="833cf-116">In hello measure grid, click an empty cell.</span></span>  
  
3.  <span data-ttu-id="833cf-117">V řádku vzorců hello výše hello tabulky zadejte hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="833cf-117">In hello formula bar, above hello table, type hello following formula:</span></span> 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=DIVIDE([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    <span data-ttu-id="833cf-118">Tato míra slouží jako základní míru hello pro hello klíčového ukazatele výkonu.</span><span class="sxs-lookup"><span data-stu-id="833cf-118">This measure serves as hello Base measure for hello KPI.</span></span>  
  
4.  <span data-ttu-id="833cf-119">Pravým tlačítkem myši klikněte na **InternetCurrentQuarterSalesPerformance** > **Vytvořit klíčový ukazatel výkonu**.</span><span class="sxs-lookup"><span data-stu-id="833cf-119">Right-click **InternetCurrentQuarterSalesPerformance** > **Create KPI**.</span></span>   
  
5.  <span data-ttu-id="833cf-120">V dialogovém okně klíčového ukazatele výkonu (KPI) hello v **cíl** vyberte **absolutní hodnota**a pak zadejte **1.1**.</span><span class="sxs-lookup"><span data-stu-id="833cf-120">In hello Key Performance Indicator (KPI) dialog box, in **Target** select **Absolute Value**, and then type **1.1**.</span></span>  
  
7.  <span data-ttu-id="833cf-121">V poli hello levý jezdec (nízká), zadejte **1**a potom v posuvníku hello vpravo (vysoká) zadejte **1.07**.</span><span class="sxs-lookup"><span data-stu-id="833cf-121">In hello left (low) slider field, type **1**, and then in hello right (high) slider field, type **1.07**.</span></span>  
  
8.  <span data-ttu-id="833cf-122">V **vyberte ikonu styl**vyberte hello kosočtverec (červený), trojúhelník (žlutý), typ (zelený) ikony v kruhu.</span><span class="sxs-lookup"><span data-stu-id="833cf-122">In **Select Icon Style**, select hello diamond (red), triangle (yellow), circle (green) icon type.</span></span>
  
    ![aas-lesson7-kpi](../tutorials/media/aas-lesson7-kpi.png)
    
    > [!TIP]  
    > <span data-ttu-id="833cf-124">Všimněte si hello rozšíření **popisy** popisek pod hello k dispozici ikona stylů.</span><span class="sxs-lookup"><span data-stu-id="833cf-124">Notice hello expandable **Descriptions** label below hello available icon styles.</span></span> <span data-ttu-id="833cf-125">Použití popisů pro hello různé elementy toomake klíčový ukazatel výkonu je více identifikovatelnými v klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="833cf-125">Use descriptions for hello various KPI elements toomake them more identifiable in client applications.</span></span>  
  
9. <span data-ttu-id="833cf-126">Klikněte na tlačítko **OK** toocomplete hello klíčového ukazatele výkonu.</span><span class="sxs-lookup"><span data-stu-id="833cf-126">Click **OK** toocomplete hello KPI.</span></span>  
  
    <span data-ttu-id="833cf-127">V mřížce měr hello, Všimněte si hello ikonu další toohello **InternetCurrentQuarterSalesPerformance** měr.</span><span class="sxs-lookup"><span data-stu-id="833cf-127">In hello measure grid, notice hello icon next toohello **InternetCurrentQuarterSalesPerformance** measure.</span></span> <span data-ttu-id="833cf-128">Tato ikona označuje, že tato míra slouží jako základní hodnota klíčového ukazatele výkonu.</span><span class="sxs-lookup"><span data-stu-id="833cf-128">This icon indicates that this measure serves as a Base value for a KPI.</span></span>  
  
#### <a name="toocreate-an-internetcurrentquartermarginperformance-kpi"></a><span data-ttu-id="833cf-129">toocreate InternetCurrentQuarterMarginPerformance klíčového ukazatele výkonu</span><span class="sxs-lookup"><span data-stu-id="833cf-129">toocreate an InternetCurrentQuarterMarginPerformance KPI</span></span>  
  
1.  <span data-ttu-id="833cf-130">V mřížce měr hello pro hello **FactInternetSales** tabulky, klikněte na tlačítko prázdné buňky.</span><span class="sxs-lookup"><span data-stu-id="833cf-130">In hello measure grid for hello **FactInternetSales** table, click an empty cell.</span></span>  
  
2.  <span data-ttu-id="833cf-131">V řádku vzorců hello výše hello tabulky zadejte hello následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="833cf-131">In hello formula bar, above hello table, type hello following formula:</span></span>  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  <span data-ttu-id="833cf-132">Pravým tlačítkem myši klikněte na **InternetCurrentQuarterMarginPerformance** > **Vytvořit klíčový ukazatel výkonu**.</span><span class="sxs-lookup"><span data-stu-id="833cf-132">Right-click **InternetCurrentQuarterMarginPerformance** > **Create KPI**.</span></span>  
  
4.  <span data-ttu-id="833cf-133">V dialogovém okně klíčového ukazatele výkonu (KPI) hello v **cíl** vyberte **absolutní hodnota**a pak zadejte **1,25**.</span><span class="sxs-lookup"><span data-stu-id="833cf-133">In hello Key Performance Indicator (KPI) dialog box, in **Target** select **Absolute Value**, and then type **1.25**.</span></span>   
  
5.  <span data-ttu-id="833cf-134">V poli hello levý jezdec (nízká), posuňte dokud hello pole zobrazuje **0,8**, a pak hello snímku pravým pole posuvník (vysoká), dokud pole hello zobrazuje **číslem 1,03**.</span><span class="sxs-lookup"><span data-stu-id="833cf-134">In hello left (low) slider field, slide until hello field displays **0.8**, and then slide hello right (high) slider field, until hello field displays **1.03**.</span></span>  
  
6.  <span data-ttu-id="833cf-135">V **vyberte ikonu styl**, vyberte hello kosočtverec (červený), trojúhelník (žlutý), typ ikony kruhu (zelený) a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="833cf-135">In **Select Icon Style**, select hello diamond (red), triangle (yellow), circle (green) icon type, and then click **OK**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="833cf-136">Co dále?</span><span class="sxs-lookup"><span data-stu-id="833cf-136">What's next?</span></span>
<span data-ttu-id="833cf-137">[Lekce 8: Vytvoření perspektiv](../tutorials/aas-lesson-8-create-perspectives.md)</span><span class="sxs-lookup"><span data-stu-id="833cf-137">[Lesson 8: Create perspectives](../tutorials/aas-lesson-8-create-perspectives.md).</span></span>
  
  
