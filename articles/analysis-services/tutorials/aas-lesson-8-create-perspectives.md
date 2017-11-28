---
title: "AAA \"Azure Analysis Services kurz lekce 8 vytvořit perspektivy | Microsoft Docs\""
description: Popisuje, jak hello toocreate perspektivy v kurzu projektu Azure Analysis Services.
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
ms.openlocfilehash: 25391813e1969ecb22af4d6f9c1ccd8358d812fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="lesson-8-create-perspectives"></a><span data-ttu-id="b7b77-103">Lekce 8: Vytvoření perspektiv</span><span class="sxs-lookup"><span data-stu-id="b7b77-103">Lesson 8: Create perspectives</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="b7b77-104">V této lekci vytvoříte perspektivu Internet Sales.</span><span class="sxs-lookup"><span data-stu-id="b7b77-104">In this lesson, you create an Internet Sales perspective.</span></span> <span data-ttu-id="b7b77-105">Perspektiva definuje zobrazitelnou podmnožinu modelu poskytující zacílené pohledy specifické pro firmu nebo aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b7b77-105">A perspective defines a viewable subset of a model that provides focused, business-specific, or application-specific viewpoints.</span></span> <span data-ttu-id="b7b77-106">Když se uživatel připojí tooa modelu s použitím perspektivu, uvidí jenom tyto objekty modelu (tabulky, sloupce, míry, hierarchie a klíčových ukazatelů výkonu) jako polí definovaných v této perspektivě.</span><span class="sxs-lookup"><span data-stu-id="b7b77-106">When a user connects tooa model by using a perspective, they see only those model objects (tables, columns, measures, hierarchies, and KPIs) as fields defined in that perspective.</span></span> <span data-ttu-id="b7b77-107">Další, najdete v části toolearn [perspektivy](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="b7b77-107">toolearn more, see [Perspectives](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).</span></span>
  
<span data-ttu-id="b7b77-108">Hello perspektivy internetového prodeje, vytvořených v této lekci vyloučí hello DimCustomer tabulky objektu.</span><span class="sxs-lookup"><span data-stu-id="b7b77-108">hello Internet Sales perspective you create in this lesson excludes hello DimCustomer table object.</span></span> <span data-ttu-id="b7b77-109">Když vytvoříte perspektivu, která vyloučí určité objekty z zobrazení, tento objekt se stále existuje v modelu hello.</span><span class="sxs-lookup"><span data-stu-id="b7b77-109">When you create a perspective that excludes certain objects from view, that object still exists in hello model.</span></span> <span data-ttu-id="b7b77-110">Nebudou se ale zobrazovat v seznamu polí klienta sestav.</span><span class="sxs-lookup"><span data-stu-id="b7b77-110">However, it is not visible in a reporting client field list.</span></span> <span data-ttu-id="b7b77-111">Počítané sloupce a míry, ať už jsou do perspektivy zahnuté či nikoli, můžou dál provádět výpočty z dat vyloučeného objektu.</span><span class="sxs-lookup"><span data-stu-id="b7b77-111">Calculated columns and measures either included in a perspective or not can still calculate from object data that is excluded.</span></span>  
  
<span data-ttu-id="b7b77-112">účelem této lekci Hello je toodescribe jak toocreate perspektivy a seznámit se s hello nástroje pro tvorbu tabulkový model.</span><span class="sxs-lookup"><span data-stu-id="b7b77-112">hello purpose of this lesson is toodescribe how toocreate perspectives and become familiar with hello tabular model authoring tools.</span></span> <span data-ttu-id="b7b77-113">Pokud rozbalíte později tento model tooinclude další tabulky, můžete vytvořit další perspektivy toodefine různých hlediska hello modelu, například inventář a organizační jednotky prodej.</span><span class="sxs-lookup"><span data-stu-id="b7b77-113">If you later expand this model tooinclude additional tables, you can create additional perspectives toodefine different viewpoints of hello model, for example, Inventory and Sales.</span></span>  
  
<span data-ttu-id="b7b77-114">Odhadovaný čas toocomplete této lekci: **pět minut**</span><span class="sxs-lookup"><span data-stu-id="b7b77-114">Estimated time toocomplete this lesson: **Five minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="b7b77-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b7b77-115">Prerequisites</span></span>  
<span data-ttu-id="b7b77-116">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="b7b77-116">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="b7b77-117">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [Lekce 7: vytvoření klíčové ukazatele výkonu](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span><span class="sxs-lookup"><span data-stu-id="b7b77-117">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 7: Create Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span></span>  
  
## <a name="create-perspectives"></a><span data-ttu-id="b7b77-118">Vytvoření perspektiv</span><span class="sxs-lookup"><span data-stu-id="b7b77-118">Create perspectives</span></span>  
  
#### <a name="toocreate-an-internet-sales-perspective"></a><span data-ttu-id="b7b77-119">toocreate Internetu prodej perspektivy</span><span class="sxs-lookup"><span data-stu-id="b7b77-119">toocreate an Internet Sales perspective</span></span>  
  
1.  <span data-ttu-id="b7b77-120">Klikněte na tlačítko hello **modelu** nabídky > **perspektivy** > **vytvořit a spravovat**.</span><span class="sxs-lookup"><span data-stu-id="b7b77-120">Click hello **Model** menu > **Perspectives** > **Create and Manage**.</span></span>  
  
2.  <span data-ttu-id="b7b77-121">V hello **perspektivy** dialogové okno, klikněte na tlačítko **nové perspektivy**.</span><span class="sxs-lookup"><span data-stu-id="b7b77-121">In hello **Perspectives** dialog box, click **New Perspective**.</span></span>  
  
3.  <span data-ttu-id="b7b77-122">Klikněte dvakrát na hello **nové perspektivy** záhlaví sloupce a poté změňte **Internet prodej**.</span><span class="sxs-lookup"><span data-stu-id="b7b77-122">Double-click hello **New Perspective** column heading, and then rename **Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="b7b77-123">Vyberte hello všechny tabulky hello *s výjimkou* **DimCustomer**.</span><span class="sxs-lookup"><span data-stu-id="b7b77-123">Select hello all hello tables *except* **DimCustomer**.</span></span>  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    <span data-ttu-id="b7b77-125">V později lekce použijete hello analyzovat v tootest funkce aplikace Excel se tato perspektiva.</span><span class="sxs-lookup"><span data-stu-id="b7b77-125">In a later lesson, you use hello Analyze in Excel feature tootest this perspective.</span></span> <span data-ttu-id="b7b77-126">Hello seznamu polí kontingenční tabulky aplikace Excel obsahuje každou tabulku s výjimkou hello DimCustomer tabulky.</span><span class="sxs-lookup"><span data-stu-id="b7b77-126">hello Excel PivotTable Fields List includes each table except hello DimCustomer table.</span></span>  

## <a name="whats-next"></a><span data-ttu-id="b7b77-127">Co dále?</span><span class="sxs-lookup"><span data-stu-id="b7b77-127">What's next?</span></span>
<span data-ttu-id="b7b77-128">[Lekce 9: Vytvoření hierarchií](../tutorials/aas-lesson-9-create-hierarchies.md)</span><span class="sxs-lookup"><span data-stu-id="b7b77-128">[Lesson 9: Create hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>
  
  
  
  
