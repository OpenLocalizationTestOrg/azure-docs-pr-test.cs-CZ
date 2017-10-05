---
title: "Kurz služby Azure Analysis Services – Doplňková lekce: Řádky podrobností | Dokumentace Microsoftu"
description: "Popisuje, jak vytvořit výraz řádků podrobností v kurzu služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: fde5cd9a9efc3a13e731a91962ced5c086a72355
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="supplemental-lesson---detail-rows"></a><span data-ttu-id="87608-103">Doplňková lekce – Řádky podrobností</span><span class="sxs-lookup"><span data-stu-id="87608-103">Supplemental lesson - Detail Rows</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="87608-104">V této doplňkové lekci použijete Editor DAX k definici vlastního výrazu řádků podrobností.</span><span class="sxs-lookup"><span data-stu-id="87608-104">In this supplemental lesson, you use the DAX Editor to define a custom Detail Rows Expression.</span></span> <span data-ttu-id="87608-105">Výraz řádků podrobností je vlastnost míry, která koncovým uživatelům poskytuje další informace o agregovaných výsledcích míry.</span><span class="sxs-lookup"><span data-stu-id="87608-105">A Detail Rows Expression is a property on a measure, providing end-users more information about the aggregated results of a measure.</span></span> 
  
<span data-ttu-id="87608-106">Odhadovaný čas dokončení této lekce: **10 minut**</span><span class="sxs-lookup"><span data-stu-id="87608-106">Estimated time to complete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="87608-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87608-107">Prerequisites</span></span>  
<span data-ttu-id="87608-108">Toto téma doplňkové lekce je součástí kurzu tabulkového modelování.</span><span class="sxs-lookup"><span data-stu-id="87608-108">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="87608-109">Než začnete provádět úkoly v této doplňkové lekci, měli byste mít dokončené všechny předchozí lekce nebo mít dokončený ukázkový projekt modelu Adventure Works Internet Sales.</span><span class="sxs-lookup"><span data-stu-id="87608-109">Before performing the tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span>  
  
## <a name="what-do-we-need-to-solve"></a><span data-ttu-id="87608-110">Co je potřeba vyřešit?</span><span class="sxs-lookup"><span data-stu-id="87608-110">What do we need to solve?</span></span>
<span data-ttu-id="87608-111">Než přidáme výraz řádků podrobností, podívejme se na podrobnosti naší míry InternetTotalSales.</span><span class="sxs-lookup"><span data-stu-id="87608-111">Let's look at the details of our InternetTotalSales measure, before adding a Detail Rows Expression.</span></span>

1.  <span data-ttu-id="87608-112">V sadě SSDT klikněte na nabídku **Model** > **Analyzovat v aplikaci Excel**, otevřete Excel a vytvořte prázdnou kontingenční tabulku.</span><span class="sxs-lookup"><span data-stu-id="87608-112">In SSDT, click the **Model** menu > **Analyze in Excel** to open Excel and create a blank PivotTable.</span></span>
  
2.  <span data-ttu-id="87608-113">V části **Pole kontingenční tabulky** přidejte míru **InternetTotalSales** z tabulky FactInternetSales do **Hodnoty**, **CalendarYear** z tabulky DimDate do **Sloupce** a **EnglishCountryRegionName** do **Řádky**.</span><span class="sxs-lookup"><span data-stu-id="87608-113">In **PivotTable Fields**, add the **InternetTotalSales** measure from the FactInternetSales table to **Values**, **CalendarYear** from the DimDate table to **Columns**, and **EnglishCountryRegionName** to **Rows**.</span></span> <span data-ttu-id="87608-114">Naše kontingenční tabulka nám teď poskytuje výsledky míry InternetTotalSales podle oblastí a roku.</span><span class="sxs-lookup"><span data-stu-id="87608-114">Our PivotTable now gives us aggregated results from the InternetTotalSales measure by regions and year.</span></span> 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. <span data-ttu-id="87608-116">V kontingenční tabulce poklikejte na agregovanou hodnotu pro rok a název oblasti.</span><span class="sxs-lookup"><span data-stu-id="87608-116">In the PivotTable, double-click an aggregated value for a year and a region name.</span></span> <span data-ttu-id="87608-117">My jsme poklikali na hodnotu pro Austrálii a rok 2014.</span><span class="sxs-lookup"><span data-stu-id="87608-117">Here we double-clicked the value for Australia and the year 2014.</span></span> <span data-ttu-id="87608-118">Otevře se nový list, který obsahuje data. Tato data ale nejsou moc užitečná.</span><span class="sxs-lookup"><span data-stu-id="87608-118">A new sheet opens containing data, but not useful data.</span></span>

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
<span data-ttu-id="87608-120">Rádi bychom viděli tabulku obsahující sloupce a řádky dat přispívající k agregovanému výsledku naší míry InternetTotalSales.</span><span class="sxs-lookup"><span data-stu-id="87608-120">What we would like to see here is a table containing columns and rows of data that contribute to the aggregated result of our InternetTotalSales measure.</span></span> <span data-ttu-id="87608-121">Za tímto účelem můžeme přidat výraz řádků podrobností jako vlastnost míry.</span><span class="sxs-lookup"><span data-stu-id="87608-121">To do that, we can add a Detail Rows Expression as a property of the measure.</span></span>

## <a name="add-a-detail-rows-expression"></a><span data-ttu-id="87608-122">Přidání výrazu řádků podrobností</span><span class="sxs-lookup"><span data-stu-id="87608-122">Add a Detail Rows Expression</span></span>

#### <a name="to-create-a-detail-rows-expression"></a><span data-ttu-id="87608-123">Vytvoření výrazu řádků podrobností</span><span class="sxs-lookup"><span data-stu-id="87608-123">To create a Detail Rows Expression</span></span> 
  
1. <span data-ttu-id="87608-124">V sadě SSDT klikněte v mřížce měr tabulky FactInternetSales na míru **InternetTotalSales**.</span><span class="sxs-lookup"><span data-stu-id="87608-124">In SSDT, in the FactInternetSales table's measure grid, click the **InternetTotalSales** measure.</span></span> 

2. <span data-ttu-id="87608-125">V části **Vlastnosti** > **Výraz řádků podrobností** klikněte na tlačítko editoru a otevřete Editor DAX.</span><span class="sxs-lookup"><span data-stu-id="87608-125">In **Properties** > **Detail Rows Expression**, click the editor button to open the DAX Editor.</span></span>

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. <span data-ttu-id="87608-127">V Editoru DAX zadejte následující výraz:</span><span class="sxs-lookup"><span data-stu-id="87608-127">In DAX Editor, enter the following expression:</span></span>

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

    <span data-ttu-id="87608-128">Tento výraz určuje názvy a sloupce a výsledky měr z tabulky FactInternetSales a souvisejících tabulky se vrátí, když uživatel pokliká na agregovaný výsledek v kontingenční tabulce nebo sestavě.</span><span class="sxs-lookup"><span data-stu-id="87608-128">This expression specifies names, columns, and measure results from the FactInternetSales table and related tables are returned when a user double-clicks an aggregated result in a PivotTable or report.</span></span>

4. <span data-ttu-id="87608-129">Zpět v aplikaci Excel odstraňte list vytvořený v kroku 3, pak poklikejte na agregovanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="87608-129">Back in Excel, delete the sheet created in Step 3, then double-click an aggregated value.</span></span> <span data-ttu-id="87608-130">Teď s nadefinovanou vlastností výrazu řádků vlastností pro míru se otevře nový list obsahující užitečnější data.</span><span class="sxs-lookup"><span data-stu-id="87608-130">This time, with a Detail Rows Expression property defined for the measure, a new sheet opens containing a lot more useful data.</span></span>

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. <span data-ttu-id="87608-132">Znovu nasaďte model.</span><span class="sxs-lookup"><span data-stu-id="87608-132">Redeploy your model.</span></span>

  
## <a name="see-also"></a><span data-ttu-id="87608-133">Viz také</span><span class="sxs-lookup"><span data-stu-id="87608-133">See Also</span></span>  
<span data-ttu-id="87608-134">[Funkce SELECTCOLUMNS (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span><span class="sxs-lookup"><span data-stu-id="87608-134">[SELECTCOLUMNS Function (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span></span>  
[<span data-ttu-id="87608-135">Doplňková lekce – Dynamické zabezpečení</span><span class="sxs-lookup"><span data-stu-id="87608-135">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="87608-136">Doplňková lekce – Nepravidelné hierarchie</span><span class="sxs-lookup"><span data-stu-id="87608-136">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
