---
title: AAA "Azure Analysis Services Adventure Works kurzu | Microsoft Docs"
description: "Zavádí hello společnosti Adventure Works kurz pro Azure Analysis Services"
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
ms.openlocfilehash: 2df8b3ab4e8c4ffbe0086418d60fd2e2abd35e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a><span data-ttu-id="c4336-103">Azure Analysis Services – Kurz Adventure Works</span><span class="sxs-lookup"><span data-stu-id="c4336-103">Azure Analysis Services - Adventure Works tutorial</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="c4336-104">Tento kurz obsahuje lekce toocreate a nasazovat s využitím tabulkový model na úroveň kompatibility hello 1400 [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).</span><span class="sxs-lookup"><span data-stu-id="c4336-104">This tutorial provides lessons on how toocreate and deploy a tabular model at hello 1400 compatibility level by using [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).</span></span>  

<span data-ttu-id="c4336-105">Pokud jste nové služby tooAnalysis a tabulkové modelování, dokončení tohoto kurzu je nejrychlejší způsob, jak toolearn hello jak toocreate a nasazení základní tabulkový model.</span><span class="sxs-lookup"><span data-stu-id="c4336-105">If you're new tooAnalysis Services and tabular modeling, completing this tutorial is hello quickest way toolearn how toocreate and deploy a basic tabular model.</span></span> <span data-ttu-id="c4336-106">Jakmile máte hello požadavky na místě, má mezi dvěma toocomplete hodin toothree trvat.</span><span class="sxs-lookup"><span data-stu-id="c4336-106">Once you have hello prerequisites in-place, it should take between two toothree hours toocomplete.</span></span>  
  
## <a name="what-you-learn"></a><span data-ttu-id="c4336-107">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c4336-107">What you learn</span></span>   
  
-   <span data-ttu-id="c4336-108">Jak toocreate nový tabulkový model projektu na hello **úroveň kompatibility 1400** v sadě SSDT.</span><span class="sxs-lookup"><span data-stu-id="c4336-108">How toocreate a new tabular model project at hello **1400 compatibility level** in SSDT.</span></span>
  
-   <span data-ttu-id="c4336-109">Jak tooimport data z relační databáze do projektu tabulkový model.</span><span class="sxs-lookup"><span data-stu-id="c4336-109">How tooimport data from a relational database into a tabular model project.</span></span>  
  
-   <span data-ttu-id="c4336-110">Jak toocreate a spravovat vztahy mezi tabulkami v modelu hello.</span><span class="sxs-lookup"><span data-stu-id="c4336-110">How toocreate and manage relationships between tables in hello model.</span></span>  
  
-   <span data-ttu-id="c4336-111">Jak toocreate počítané sloupce, míry a klíčové ukazatele výkonu, které pomáhají uživatelům analyzovat kritické obchodní metriky.</span><span class="sxs-lookup"><span data-stu-id="c4336-111">How toocreate calculated columns, measures, and Key Performance Indicators that help users analyze critical business metrics.</span></span>  
  
-   <span data-ttu-id="c4336-112">Jak toocreate a spravovat perspektivy a hierarchií, které pomáhají uživatelům snadno procházet data modelu tím, že poskytuje firmy a hlediska specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4336-112">How toocreate and manage perspectives and hierarchies that help users more easily browse model data by providing business and application-specific viewpoints.</span></span>  
  
-   <span data-ttu-id="c4336-113">Jak toocreate oddíly které rozdělují data tabulky do menších logických částí, které lze zpracovat nezávisle z jiných oddílů.</span><span class="sxs-lookup"><span data-stu-id="c4336-113">How toocreate partitions that divide table data into smaller logical parts that can be processed independent from other partitions.</span></span>  
  
-   <span data-ttu-id="c4336-114">Jak toosecure model objektů a dat po vytvoření se členy uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="c4336-114">How toosecure model objects and data by creating roles with user members.</span></span>  
  
-   <span data-ttu-id="c4336-115">Jak toodeploy tabulkový model tooan **Azure Analysis Services** server nebo místní server SQL Server 2017 Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="c4336-115">How toodeploy a tabular model tooan **Azure Analysis Services** server or an on-premises SQL Server 2017 Analysis Services server.</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="c4336-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c4336-116">Prerequisites</span></span>  
<span data-ttu-id="c4336-117">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="c4336-117">toocomplete this tutorial, you need:</span></span>  
  
-   <span data-ttu-id="c4336-118">Azure Analysis Services nebo SQL Server 2017 Analysis Services instanci toodeploy váš model.</span><span class="sxs-lookup"><span data-stu-id="c4336-118">An Azure Analysis Services or SQL Server 2017 Analysis Services instance toodeploy your model to.</span></span> <span data-ttu-id="c4336-119">Zaregistrujte si bezplatnou [zkušební verzi Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) a [vytvořte server](../analysis-services-create-server.md).</span><span class="sxs-lookup"><span data-stu-id="c4336-119">Sign up for a free [Azure Analysis Services trial](https://azure.microsoft.com/services/analysis-services/) and [create a server](../analysis-services-create-server.md).</span></span> <span data-ttu-id="c4336-120">Nebo si zaregistrujte a stáhněte verzi [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp).</span><span class="sxs-lookup"><span data-stu-id="c4336-120">Or, sign up and download [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp).</span></span> 

-   <span data-ttu-id="c4336-121">Datový sklad SQL Server nebo Azure SQL Data Warehouse s hello [AdventureWorksDW2014 ukázkové databáze](http://go.microsoft.com/fwlink/?LinkID=335807).</span><span class="sxs-lookup"><span data-stu-id="c4336-121">A SQL Server Data Warehouse or Azure SQL Data Warehouse with hello [AdventureWorksDW2014 sample database](http://go.microsoft.com/fwlink/?LinkID=335807).</span></span> <span data-ttu-id="c4336-122">Tato ukázková databáze obsahuje nezbytné toocomplete hello data v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c4336-122">This sample database includes hello data necessary toocomplete this tutorial.</span></span> <span data-ttu-id="c4336-123">Stáhněte si [bezplatnou edici SQL Serveru](https://www.microsoft.com/sql-server/sql-server-downloads).</span><span class="sxs-lookup"><span data-stu-id="c4336-123">Download [SQL Server free editions](https://www.microsoft.com/sql-server/sql-server-downloads).</span></span> <span data-ttu-id="c4336-124">Nebo si zaregistrujte bezplatnou [zkušební verzi Azure SQL Database](https://azure.microsoft.com/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="c4336-124">Or, sign up for a free [Azure SQL Database trial](https://azure.microsoft.com/services/sql-database/).</span></span> 

    <span data-ttu-id="c4336-125">**Důležité:** Pokud instalace hello ukázkové databáze na serveru SQL na místě a nasazení serveru Azure Analysis Services tooan modelu, [místní brána dat](../analysis-services-gateway.md) je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="c4336-125">**Important:** If you install hello sample database on an on-premises SQL Server, and you deploy your model tooan Azure Analysis Services server, an [On-premises data gateway](../analysis-services-gateway.md) is required.</span></span>

-   <span data-ttu-id="c4336-126">nejnovější verze Hello [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4336-126">hello latest version of [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).</span></span>

-   <span data-ttu-id="c4336-127">nejnovější verze Hello [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="c4336-127">hello latest version of [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>    

-   <span data-ttu-id="c4336-128">Klientskou aplikaci, jako je [Power BI Desktop](https://powerbi.microsoft.com/desktop/) nebo Excel.</span><span class="sxs-lookup"><span data-stu-id="c4336-128">A client application such as [Power BI Desktop](https://powerbi.microsoft.com/desktop/) or Excel.</span></span> 

## <a name="scenario"></a><span data-ttu-id="c4336-129">Scénář</span><span class="sxs-lookup"><span data-stu-id="c4336-129">Scenario</span></span>  
<span data-ttu-id="c4336-130">Tento kurz je založený na fiktivní společnosti Adventure Works Cycles.</span><span class="sxs-lookup"><span data-stu-id="c4336-130">This tutorial is based on Adventure Works Cycles, a fictitious company.</span></span> <span data-ttu-id="c4336-131">Společnosti Adventure Works je velké mezinárodní výrobní společnost, která vytvoří a distribuuje jízdních kol a části, příslušenství toocommercial trhů v Severní Americe, Evropy a Asie.</span><span class="sxs-lookup"><span data-stu-id="c4336-131">Adventure Works is a large, multinational manufacturing company that produces and distributes bicycles, parts, and accessories toocommercial markets in North America, Europe, and Asia.</span></span> <span data-ttu-id="c4336-132">Hello společnosti využívá 500 pracovních procesů.</span><span class="sxs-lookup"><span data-stu-id="c4336-132">hello company employs 500 workers.</span></span> <span data-ttu-id="c4336-133">Kromě toho Adventure Works zaměstnává několik regionálních prodejních týmů na všech základních trzích.</span><span class="sxs-lookup"><span data-stu-id="c4336-133">Additionally, Adventure Works employs several regional sales teams throughout its market base.</span></span> <span data-ttu-id="c4336-134">Projekt je toocreate tabulkový model pro tooanalyze prodeje a marketingu uživatelů Internetu prodejní data v databázi AdventureWorksDW hello.</span><span class="sxs-lookup"><span data-stu-id="c4336-134">Your project is toocreate a tabular model for sales and marketing users tooanalyze Internet sales data in hello AdventureWorksDW database.</span></span>  
  
<span data-ttu-id="c4336-135">toocomplete hello kurz, musíte dokončit různé lekce.</span><span class="sxs-lookup"><span data-stu-id="c4336-135">toocomplete hello tutorial, you must complete various lessons.</span></span> <span data-ttu-id="c4336-136">Každá lekce zahrnuje úkoly.</span><span class="sxs-lookup"><span data-stu-id="c4336-136">In each lesson, there are tasks.</span></span> <span data-ttu-id="c4336-137">Dokončení každý úkol v pořadí, je nutné pro dokončení lekce hello.</span><span class="sxs-lookup"><span data-stu-id="c4336-137">Completing each task in order is necessary for completing hello lesson.</span></span> <span data-ttu-id="c4336-138">Některé lekce obsahují několik úkolů, jejichž výsledek je podobný, ale způsob jejich dokončení se mírně liší.</span><span class="sxs-lookup"><span data-stu-id="c4336-138">While in a particular lesson there may be several tasks that accomplish a similar outcome, but how you complete each task is slightly different.</span></span> <span data-ttu-id="c4336-139">Tato metoda zobrazí často je více než jedním ze způsobů toocomplete úlohu a toochallenge naučili jste se s použitím znalostí jste v předchozím lekce a úlohy.</span><span class="sxs-lookup"><span data-stu-id="c4336-139">This method shows there is often more than one way toocomplete a task, and toochallenge you by using skills you've learned in previous lessons and tasks.</span></span>  
  
<span data-ttu-id="c4336-140">účelem Hello lekce hello je tooguide vytvářením základní tabulkový model pomocí mnoho funkcí hello zahrnutých v sadě SSDT.</span><span class="sxs-lookup"><span data-stu-id="c4336-140">hello purpose of hello lessons is tooguide you through authoring a basic tabular model by using many of hello features included in SSDT.</span></span> <span data-ttu-id="c4336-141">Protože každé lekce navazuje na předchozí lekci hello, musíte provést hello lekce v pořadí.</span><span class="sxs-lookup"><span data-stu-id="c4336-141">Because each lesson builds upon hello previous lesson, you should complete hello lessons in order.</span></span>
  
<span data-ttu-id="c4336-142">V tomto kurzu neposkytuje poznatky o správu serveru na portálu Azure, Správa serveru nebo v databázi pomocí aplikace SSMS, nebo pomocí klienta aplikace data modelu toobrowse.</span><span class="sxs-lookup"><span data-stu-id="c4336-142">This tutorial does not provide lessons about managing a server in Azure portal, managing a server or database by using SSMS, or using a client application toobrowse model data.</span></span> 


## <a name="lessons"></a><span data-ttu-id="c4336-143">Lekce</span><span class="sxs-lookup"><span data-stu-id="c4336-143">Lessons</span></span>  
<span data-ttu-id="c4336-144">Tento kurz obsahuje následující lekce hello:</span><span class="sxs-lookup"><span data-stu-id="c4336-144">This tutorial includes hello following lessons:</span></span>  
  
|<span data-ttu-id="c4336-145">Lekce</span><span class="sxs-lookup"><span data-stu-id="c4336-145">Lesson</span></span>|<span data-ttu-id="c4336-146">Odhadovaná doba toocomplete</span><span class="sxs-lookup"><span data-stu-id="c4336-146">Estimated time toocomplete</span></span>|  
|----------|------------------------------|  
|[<span data-ttu-id="c4336-147">1 – Vytvoření nového projektu s tabelárním modelem</span><span class="sxs-lookup"><span data-stu-id="c4336-147">1 - Create a new tabular model project</span></span>](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|<span data-ttu-id="c4336-148">10 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-148">10 minutes</span></span>|  
|[<span data-ttu-id="c4336-149">2 – Získání dat</span><span class="sxs-lookup"><span data-stu-id="c4336-149">2 - Get data</span></span>](../tutorials/aas-lesson-2-get-data.md)|<span data-ttu-id="c4336-150">10 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-150">10 minutes</span></span>|  
|[<span data-ttu-id="c4336-151">3 – Označení jako datové tabulky</span><span class="sxs-lookup"><span data-stu-id="c4336-151">3 - Mark as Date Table</span></span>](../tutorials/aas-lesson-3-mark-as-date-table.md)|<span data-ttu-id="c4336-152">3 minuty</span><span class="sxs-lookup"><span data-stu-id="c4336-152">3 minutes</span></span>|  
|[<span data-ttu-id="c4336-153">4 – Vytvoření relací</span><span class="sxs-lookup"><span data-stu-id="c4336-153">4 - Create relationships</span></span>](../tutorials/aas-lesson-4-create-relationships.md)|<span data-ttu-id="c4336-154">10 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-154">10 minutes</span></span>|  
|[<span data-ttu-id="c4336-155">5 – Vytvoření počítaných sloupců</span><span class="sxs-lookup"><span data-stu-id="c4336-155">5 - Create calculated columns</span></span>](../tutorials/aas-lesson-5-create-calculated-columns.md)|<span data-ttu-id="c4336-156">15 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-156">15 minutes</span></span>|
|[<span data-ttu-id="c4336-157">6 – Vytvoření měr</span><span class="sxs-lookup"><span data-stu-id="c4336-157">6 - Create measures</span></span>](../tutorials/aas-lesson-6-create-measures.md)|<span data-ttu-id="c4336-158">30 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-158">30 minutes</span></span>|  
|[<span data-ttu-id="c4336-159">7 – Vytvoření klíčových ukazatelů výkonu (KPI)</span><span class="sxs-lookup"><span data-stu-id="c4336-159">7 - Create Key Performance Indicators (KPI)</span></span>](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|<span data-ttu-id="c4336-160">15 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-160">15 minutes</span></span>|  
|[<span data-ttu-id="c4336-161">8 – Vytvoření perspektiv</span><span class="sxs-lookup"><span data-stu-id="c4336-161">8 - Create perspectives</span></span>](../tutorials/aas-lesson-8-create-perspectives.md)|<span data-ttu-id="c4336-162">5 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-162">5 minutes</span></span>|  
|[<span data-ttu-id="c4336-163">9 – Vytvoření hierarchií</span><span class="sxs-lookup"><span data-stu-id="c4336-163">9 - Create hierarchies</span></span>](../tutorials/aas-lesson-9-create-hierarchies.md)|<span data-ttu-id="c4336-164">20 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-164">20 minutes</span></span>|  
|[<span data-ttu-id="c4336-165">10 – Vytvoření oddílů</span><span class="sxs-lookup"><span data-stu-id="c4336-165">10 - Create partitions</span></span>](../tutorials/aas-lesson-10-create-partitions.md)|<span data-ttu-id="c4336-166">15 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-166">15 minutes</span></span>|  
|[<span data-ttu-id="c4336-167">11 – Vytvoření rolí</span><span class="sxs-lookup"><span data-stu-id="c4336-167">11 - Create roles</span></span>](../tutorials/aas-lesson-11-create-roles.md)|<span data-ttu-id="c4336-168">15 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-168">15 minutes</span></span>|  
|[<span data-ttu-id="c4336-169">12 – Analýza v aplikaci Excel</span><span class="sxs-lookup"><span data-stu-id="c4336-169">12 - Analyze in Excel</span></span>](../tutorials/aas-lesson-12-analyze-in-excel.md)|<span data-ttu-id="c4336-170">5 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-170">5 minutes</span></span>| 
|[<span data-ttu-id="c4336-171">13 – Nasazení</span><span class="sxs-lookup"><span data-stu-id="c4336-171">13 - Deploy</span></span>](../tutorials/aas-lesson-13-deploy.md)|<span data-ttu-id="c4336-172">5 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-172">5 minutes</span></span>|  
  
## <a name="supplemental-lessons"></a><span data-ttu-id="c4336-173">Doplňkové lekce</span><span class="sxs-lookup"><span data-stu-id="c4336-173">Supplemental lessons</span></span>  
<span data-ttu-id="c4336-174">Tyto poznatky získají nejsou požadované toocomplete hello kurzu, ale může být užitečné pro lepší pochopení pokročilé tabulkový model. vytváření funkcí.</span><span class="sxs-lookup"><span data-stu-id="c4336-174">These lessons are not required toocomplete hello tutorial, but can be helpful in better understanding advanced tabular model authoring features.</span></span>  
  
|<span data-ttu-id="c4336-175">Lekce</span><span class="sxs-lookup"><span data-stu-id="c4336-175">Lesson</span></span>|<span data-ttu-id="c4336-176">Odhadovaná doba toocomplete</span><span class="sxs-lookup"><span data-stu-id="c4336-176">Estimated time toocomplete</span></span>|  
|----------|------------------------------|  
|[<span data-ttu-id="c4336-177">Řádky podrobností</span><span class="sxs-lookup"><span data-stu-id="c4336-177">Detail Rows</span></span>](../tutorials/aas-supplemental-lesson-detail-rows.md)|<span data-ttu-id="c4336-178">10 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-178">10 minutes</span></span>|
|[<span data-ttu-id="c4336-179">Dynamické zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c4336-179">Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)|<span data-ttu-id="c4336-180">30 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-180">30 minutes</span></span>|
|[<span data-ttu-id="c4336-181">Nepravidelné hierarchie</span><span class="sxs-lookup"><span data-stu-id="c4336-181">Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|<span data-ttu-id="c4336-182">20 minut</span><span class="sxs-lookup"><span data-stu-id="c4336-182">20 minutes</span></span>| 

  
## <a name="next-steps"></a><span data-ttu-id="c4336-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4336-183">Next steps</span></span>  
<span data-ttu-id="c4336-184">tooget začít, najdete v části [lekci 1: vytvoření nového projektu tabulkový Model](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span><span class="sxs-lookup"><span data-stu-id="c4336-184">tooget started, see [Lesson 1: Create a New Tabular Model Project](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span></span>  
  
  
  

