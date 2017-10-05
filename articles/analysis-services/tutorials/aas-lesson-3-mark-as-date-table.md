---
title: "Kurz služby Azure Analysis Services – Lekce 3: Označení jako tabulky kalendářních dat | Dokumentace Microsoftu"
description: "Popisuje, jak označit tabulku kalendářních dat v projektu Kurz služby Azure Analysis Services."
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
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: c62f2726fef5219155a08b70c61162c914600d1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-3-mark-as-date-table"></a><span data-ttu-id="6654e-103">Lekce 3: Označení jako tabulky kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="6654e-103">Lesson 3: Mark as Date Table</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="6654e-104">V Lekci 2: Získání dat jste naimportovali tabulku dimenzí s názvem DimDate.</span><span class="sxs-lookup"><span data-stu-id="6654e-104">In Lesson 2: Get data, you imported a dimension table named DimDate.</span></span> <span data-ttu-id="6654e-105">Přestože ve vašem modelu je tato tabulka pojmenovaná DimDate, je možné ji také označit jako *tabulku kalendářních dat*, protože obsahuje údaje o datu a čase.</span><span class="sxs-lookup"><span data-stu-id="6654e-105">While in your model this table is named DimDate, it can also be known as a *Date table*, in that it contains date and time data.</span></span>  
  
<span data-ttu-id="6654e-106">Při každém použití funkcí časového měřítka DAX, třeba při pozdějším vytvoření měření, musíte zadat vlastnosti, které zahrnují *tabulku kalendářních dat* a jedinečný identifikátor – *sloupec Date* v této tabulce.</span><span class="sxs-lookup"><span data-stu-id="6654e-106">Whenever you use DAX time-intelligence functions, like when you create measures later, you must specify properties which include a *Date table* and a unique identifier *Date column* in that table.</span></span>
  
<span data-ttu-id="6654e-107">V této lekci označíte tabulku DimDate jako *tabulku kalendářních dat* a sloupec Date (v tabulce kalendářních dat) jako *sloupec Date* (jedinečný identifikátor).</span><span class="sxs-lookup"><span data-stu-id="6654e-107">In this lesson, you mark the DimDate table as the *Date table* and the Date column (in the Date table) as the *Date column* (unique identifier).</span></span>  

<span data-ttu-id="6654e-108">Než označíte tabulku a sloupec kalendářních dat, je vhodná doba provést drobnou údržbu, která zajistí lepší srozumitelnost vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="6654e-108">Before you mark the date table and date column, it's a good time to do a little housekeeping to make your model easier to understand.</span></span> <span data-ttu-id="6654e-109">Všimněte si, že tabulka DimDate obsahuje sloupec s názvem **FullDateAlternateKey**.</span><span class="sxs-lookup"><span data-stu-id="6654e-109">Notice in the DimDate table a column named **FullDateAlternateKey**.</span></span> <span data-ttu-id="6654e-110">Tento sloupec obsahuje jeden řádek pro každý den každého kalendářního roce zahrnutého v tabulce.</span><span class="sxs-lookup"><span data-stu-id="6654e-110">This column contains one row for every day in each calendar year included in the table.</span></span> <span data-ttu-id="6654e-111">Tento sloupec používáte v řadě vzorců měření a v sestavách.</span><span class="sxs-lookup"><span data-stu-id="6654e-111">You use this column a lot in measure formulas and in reports.</span></span> <span data-ttu-id="6654e-112">Ale FullDateAlternateKey skutečně není dobrý identifikátor pro tento sloupec.</span><span class="sxs-lookup"><span data-stu-id="6654e-112">But, FullDateAlternateKey isn't really a good identifier for this column.</span></span> <span data-ttu-id="6654e-113">Přejmenujete ho na **Date** a usnadníte tak jeho identifikaci a použití ve vzorcích.</span><span class="sxs-lookup"><span data-stu-id="6654e-113">You rename it to **Date**, making it easier to identify and include in formulas.</span></span> <span data-ttu-id="6654e-114">Je vhodné přejmenovat objekty, jako jsou tabulky a sloupce, aby se usnadnila jejich identifikace v SSDT a klientských aplikacích pro vytváření sestav, jako je Power BI a Excel.</span><span class="sxs-lookup"><span data-stu-id="6654e-114">Whenever possible, it's a good idea to rename objects like tables and columns to make them easier to identify in SSDT and client reporting applications like Power BI and Excel.</span></span> 
  
<span data-ttu-id="6654e-115">Odhadovaný čas dokončení této lekce: **3 minuty**</span><span class="sxs-lookup"><span data-stu-id="6654e-115">Estimated time to complete this lesson: **Three minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="6654e-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6654e-116">Prerequisites</span></span>  
<span data-ttu-id="6654e-117">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6654e-117">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="6654e-118">Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 2: Získání dat](../tutorials/aas-lesson-2-get-data.md).</span><span class="sxs-lookup"><span data-stu-id="6654e-118">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 2: Get data](../tutorials/aas-lesson-2-get-data.md).</span></span> 

### <a name="to-rename-the-fulldatealternatekey-column"></a><span data-ttu-id="6654e-119">Přejmenování sloupce FullDateAlternateKey</span><span class="sxs-lookup"><span data-stu-id="6654e-119">To rename the FullDateAlternateKey column</span></span>

1.  <span data-ttu-id="6654e-120">V návrháři modelů klikněte na tabulku **DimDate**.</span><span class="sxs-lookup"><span data-stu-id="6654e-120">In the model designer, click the **DimDate** table.</span></span>

2.  <span data-ttu-id="6654e-121">Dvakrát klikněte na hlavičku sloupce **FullDateAlternateKey** a přejmenujte ho na **Date**.</span><span class="sxs-lookup"><span data-stu-id="6654e-121">Double-click the header for the **FullDateAlternateKey** column, and then rename it to **Date**.</span></span>

  
### <a name="to-set-mark-as-date-table"></a><span data-ttu-id="6654e-122">Nastavení Označit jako tabulku kalendářních dat</span><span class="sxs-lookup"><span data-stu-id="6654e-122">To set Mark as Date Table</span></span>  
  
1.  <span data-ttu-id="6654e-123">Vyberte sloupec **Date** a potom v okně **Vlastnosti** v části **Typ dat** ověřte, že je vybraná možnost **Datum**.</span><span class="sxs-lookup"><span data-stu-id="6654e-123">Select the **Date** column, and then in the **Properties** window, under **Data Type**, make sure  **Date** is selected.</span></span>  
  
2.  <span data-ttu-id="6654e-124">Klikněte do nabídky **Tabulka**, potom klikněte na **Datum** a nakonec klikněte na **Označit jako tabulku kalendářních dat**.</span><span class="sxs-lookup"><span data-stu-id="6654e-124">Click the **Table** menu, then click **Date**, and then click **Mark as Date Table**.</span></span>  
  
3.  <span data-ttu-id="6654e-125">V dialogovém okně **Označit jako tabulku kalendářních dat** v seznamu **Datum** vyberte sloupec **Date** jako jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6654e-125">In the **Mark as Date Table** dialog box, in the **Date** listbox, select the **Date** column as the unique identifier.</span></span> <span data-ttu-id="6654e-126">Ve výchozím nastavení je obvykle vybraný.</span><span class="sxs-lookup"><span data-stu-id="6654e-126">It's usually selected by default.</span></span> <span data-ttu-id="6654e-127">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6654e-127">Click **OK**.</span></span> 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a><span data-ttu-id="6654e-129">Co dále?</span><span class="sxs-lookup"><span data-stu-id="6654e-129">What's next?</span></span>
<span data-ttu-id="6654e-130">[Lekce 4: Vytvoření relací](../tutorials/aas-lesson-4-create-relationships.md)</span><span class="sxs-lookup"><span data-stu-id="6654e-130">[Lesson 4: Create relationships](../tutorials/aas-lesson-4-create-relationships.md).</span></span>
  
