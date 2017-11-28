---
<span data-ttu-id="d8cc5-101">Title: aaa "Azure Analysis Services kurz lekce 9: vytvořte hierarchie | Microsoft Docs"Popis: služby: documentationcenter služby analysis services: '' Autor: minewiskan manager: erikre editor: '' značky:"</span><span class="sxs-lookup"><span data-stu-id="d8cc5-101">title: aaa"Azure Analysis Services tutorial lesson 9: Create hierarchies | Microsoft Docs" description: services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="d8cc5-102">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="d8cc5-102">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-9-create-hierarchies"></a><span data-ttu-id="d8cc5-103">Lekce 9: Vytvoření hierarchií</span><span class="sxs-lookup"><span data-stu-id="d8cc5-103">Lesson 9: Create hierarchies</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="d8cc5-104">V této lekci vytvoříte hierarchie.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-104">In this lesson, you create hierarchies.</span></span> <span data-ttu-id="d8cc5-105">Hierarchie jsou skupiny sloupců uspořádané do úrovní, například hierarchie Geografie může mít podúrovně pro zemi, stát, kraj a město.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-105">Hierarchies are groups of columns arranged in levels; for example, a Geography hierarchy might have sublevels for Country, State, County, and City.</span></span> <span data-ttu-id="d8cc5-106">Hierarchie můžete zobrazit oddělený od ostatních sloupců v generování sestav klienta aplikace seznam polí, snadněji je pro klienta toonavigate uživatelů a zahrnout do sestavy.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-106">Hierarchies can appear separate from other columns in a reporting client application field list, making them easier for client users toonavigate and include in a report.</span></span> <span data-ttu-id="d8cc5-107">Další, najdete v části toolearn [hierarchie](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)</span><span class="sxs-lookup"><span data-stu-id="d8cc5-107">toolearn more, see [Hierarchies](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)</span></span>
  
<span data-ttu-id="d8cc5-108">hierarchie toocreate použít Návrháře hello modelu v *zobrazení diagramu*.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-108">toocreate hierarchies, use hello model designer in *Diagram View*.</span></span> <span data-ttu-id="d8cc5-109">Vytváření a správa hierarchií se v zobrazení dat nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-109">Creating and managing hierarchies is not supported in Data View.</span></span>  
  
<span data-ttu-id="d8cc5-110">Odhadovaný čas toocomplete této lekci: **20 minut**</span><span class="sxs-lookup"><span data-stu-id="d8cc5-110">Estimated time toocomplete this lesson: **20 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="d8cc5-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8cc5-111">Prerequisites</span></span>  
<span data-ttu-id="d8cc5-112">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-112">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="d8cc5-113">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 8: vytvoření perspektivy](../tutorials/aas-lesson-8-create-perspectives.md).</span><span class="sxs-lookup"><span data-stu-id="d8cc5-113">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 8: Create perspectives](../tutorials/aas-lesson-8-create-perspectives.md).</span></span>  
  
## <a name="create-hierarchies"></a><span data-ttu-id="d8cc5-114">Vytvoření hierarchií</span><span class="sxs-lookup"><span data-stu-id="d8cc5-114">Create hierarchies</span></span>  
  
#### <a name="toocreate-a-category-hierarchy-in-hello-dimproduct-table"></a><span data-ttu-id="d8cc5-115">toocreate hierarchie kategorií v tabulce DimProduct hello</span><span class="sxs-lookup"><span data-stu-id="d8cc5-115">toocreate a Category hierarchy in hello DimProduct table</span></span>  
  
1.  <span data-ttu-id="d8cc5-116">V Návrháři modelu hello (zobrazení diagramu), klikněte pravým tlačítkem na hello **DimProduct** tabulky > **vytvořit hierarchii**.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-116">In hello model designer (diagram view), right-click hello **DimProduct** table > **Create Hierarchy**.</span></span> <span data-ttu-id="d8cc5-117">Nové hierarchie se zobrazí v dolní části hello okna hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-117">A new hierarchy appears at hello bottom of hello table window.</span></span> <span data-ttu-id="d8cc5-118">Přejmenujte hello hierarchie **kategorie**.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-118">Rename hello hierarchy **Category**.</span></span>  
  
2.  <span data-ttu-id="d8cc5-119">Klikněte a přetáhněte hello **ProductCategoryName** nové sloupce toohello **kategorie** hierarchie.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-119">Click and drag hello **ProductCategoryName** column toohello new **Category** hierarchy.</span></span>  
  
3.  <span data-ttu-id="d8cc5-120">V hello **kategorie** hierarchie, klikněte pravým tlačítkem na hello **ProductCategoryName** > **přejmenovat**a pak zadejte **kategorie**.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-120">In hello **Category** hierarchy, right-click hello **ProductCategoryName** > **Rename**, and then type **Category**.</span></span>  
  
    > [!NOTE]  
    > <span data-ttu-id="d8cc5-121">Přejmenování sloupce v hierarchii nepřejmenuje sloupce v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-121">Renaming a column in a hierarchy does not rename that column in hello table.</span></span> <span data-ttu-id="d8cc5-122">Sloupec v hierarchii je právě reprezentace hello sloupec v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-122">A column in a hierarchy is just a representation of hello column in hello table.</span></span>  
  
4.  <span data-ttu-id="d8cc5-123">Klikněte a přetáhněte hello **ProductSubcategoryName** sloupec toohello **kategorie** hierarchie.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-123">Click and drag hello **ProductSubcategoryName** column toohello **Category** hierarchy.</span></span> <span data-ttu-id="d8cc5-124">Přejmenujte ho na **Subcategory**.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-124">Rename it **Subcategory**.</span></span> 
  
5.  <span data-ttu-id="d8cc5-125">Klikněte pravým tlačítkem na hello **%{ModelName/** sloupce > **přidat toohierarchy**a potom vyberte **kategorie**.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-125">Right-click hello **ModelName** column > **Add toohierarchy**, and then select **Category**.</span></span> <span data-ttu-id="d8cc5-126">Přejmenujte ho na **Model**.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-126">Rename it **Model**.</span></span>

6.  <span data-ttu-id="d8cc5-127">Nakonec přidejte **EnglishProductName** toohello hierarchie kategorií.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-127">Finally, add **EnglishProductName** toohello Category hierarchy.</span></span> <span data-ttu-id="d8cc5-128">Přejmenujte ho na **Product**.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-128">Rename it **Product**.</span></span>  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="toocreate-hierarchies-in-hello-dimdate-table"></a><span data-ttu-id="d8cc5-130">toocreate hierarchií v tabulce DimDate hello</span><span class="sxs-lookup"><span data-stu-id="d8cc5-130">toocreate hierarchies in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="d8cc5-131">V hello **DimDate** tabulky, vytvořte hierarchii s názvem **kalendáře**.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-131">In hello **DimDate** table, create a hierarchy named **Calendar**.</span></span>  
  
3.  <span data-ttu-id="d8cc5-132">Přidejte hello následující sloupce v daném pořadí:</span><span class="sxs-lookup"><span data-stu-id="d8cc5-132">Add hello following columns in-order:</span></span>

    *  <span data-ttu-id="d8cc5-133">CalendarYear</span><span class="sxs-lookup"><span data-stu-id="d8cc5-133">CalendarYear</span></span>
    *  <span data-ttu-id="d8cc5-134">CalendarSemester</span><span class="sxs-lookup"><span data-stu-id="d8cc5-134">CalendarSemester</span></span>
    *  <span data-ttu-id="d8cc5-135">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="d8cc5-135">CalendarQuarter</span></span>
    *  <span data-ttu-id="d8cc5-136">MonthCalendar</span><span class="sxs-lookup"><span data-stu-id="d8cc5-136">MonthCalendar</span></span>
    *  <span data-ttu-id="d8cc5-137">DayNumberOfMonth</span><span class="sxs-lookup"><span data-stu-id="d8cc5-137">DayNumberOfMonth</span></span>
    
4.  <span data-ttu-id="d8cc5-138">V hello **DimDate** tabulky, vytvořte **fiskálním** hierarchie.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-138">In hello **DimDate** table, create a **Fiscal** hierarchy.</span></span> <span data-ttu-id="d8cc5-139">Zahrnout hello následující sloupce v daném pořadí:</span><span class="sxs-lookup"><span data-stu-id="d8cc5-139">Include hello following columns in-order:</span></span>  
  
    *  <span data-ttu-id="d8cc5-140">FiscalYear</span><span class="sxs-lookup"><span data-stu-id="d8cc5-140">FiscalYear</span></span>
    *  <span data-ttu-id="d8cc5-141">FiscalSemester</span><span class="sxs-lookup"><span data-stu-id="d8cc5-141">FiscalSemester</span></span>
    *  <span data-ttu-id="d8cc5-142">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="d8cc5-142">FiscalQuarter</span></span>
    *  <span data-ttu-id="d8cc5-143">MonthCalendar</span><span class="sxs-lookup"><span data-stu-id="d8cc5-143">MonthCalendar</span></span>
    *  <span data-ttu-id="d8cc5-144">DayNumberOfMonth</span><span class="sxs-lookup"><span data-stu-id="d8cc5-144">DayNumberOfMonth</span></span>
  
5.  <span data-ttu-id="d8cc5-145">Nakonec v hello **DimDate** tabulky, vytvořte **ProductionCalendar** hierarchie.</span><span class="sxs-lookup"><span data-stu-id="d8cc5-145">Finally, in hello **DimDate** table, create a **ProductionCalendar** hierarchy.</span></span> <span data-ttu-id="d8cc5-146">Zahrnout hello následující sloupce v daném pořadí:</span><span class="sxs-lookup"><span data-stu-id="d8cc5-146">Include hello following columns in-order:</span></span>  
    *  <span data-ttu-id="d8cc5-147">CalendarYear</span><span class="sxs-lookup"><span data-stu-id="d8cc5-147">CalendarYear</span></span>
    *  <span data-ttu-id="d8cc5-148">WeekNumberOfYear</span><span class="sxs-lookup"><span data-stu-id="d8cc5-148">WeekNumberOfYear</span></span>
    *  <span data-ttu-id="d8cc5-149">DayNumberOfWeek</span><span class="sxs-lookup"><span data-stu-id="d8cc5-149">DayNumberOfWeek</span></span>
  
 ## <a name="whats-next"></a><span data-ttu-id="d8cc5-150">Co dále?</span><span class="sxs-lookup"><span data-stu-id="d8cc5-150">What's next?</span></span>
<span data-ttu-id="d8cc5-151">[Lekce 10: Vytvoření oddílů](../tutorials/aas-lesson-10-create-partitions.md)</span><span class="sxs-lookup"><span data-stu-id="d8cc5-151">[Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span> 
  
  
