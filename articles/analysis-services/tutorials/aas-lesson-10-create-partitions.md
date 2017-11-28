---
title: "Kurz služby Azure Analysis Services – Lekce 10: Vytvoření oddílů | Dokumentace Microsoftu"
description: "Popisuje, jak vytvořit oddíly v projektu Kurz služby Azure Analysis Services."
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
ms.openlocfilehash: df74d9cbdcf4916c24955e491767589e72389155
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-10-create-partitions"></a><span data-ttu-id="803fa-103">Lekce 10: Vytvoření oddílů</span><span class="sxs-lookup"><span data-stu-id="803fa-103">Lesson 10: Create partitions</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="803fa-104">V této lekci vytvoříte oddíly pro rozdělení tabulky FactInternetSales na menší logické jednotky s možností zpracování (aktualizace) nezávisle na ostatních oddílech.</span><span class="sxs-lookup"><span data-stu-id="803fa-104">In this lesson, you create partitions to divide the FactInternetSales table into smaller logical parts that can be processed (refreshed) independent of other partitions.</span></span> <span data-ttu-id="803fa-105">Ve výchozím nastavení má každá tabulka zahrnutá do modelu jeden oddíl, který obsahuje všechny sloupce a řádky tabulky.</span><span class="sxs-lookup"><span data-stu-id="803fa-105">By default, every table you include in your model has one partition, which includes all the table’s columns and rows.</span></span> <span data-ttu-id="803fa-106">Pro tabulku FactInternetSales chceme data rozdělit podle roku – jeden oddíl pro každých pět let v tabulce.</span><span class="sxs-lookup"><span data-stu-id="803fa-106">For the FactInternetSales table, we want to divide the data by year; one partition for each of the table’s five years.</span></span> <span data-ttu-id="803fa-107">Jednotlivé oddíly pak bude možné zpracovávat nezávisle na sobě.</span><span class="sxs-lookup"><span data-stu-id="803fa-107">Each partition can then be processed independently.</span></span> <span data-ttu-id="803fa-108">Další informace najdete v tématu [Oddíly](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="803fa-108">To learn more, see [Partitions](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span></span> 
  
<span data-ttu-id="803fa-109">Odhadovaný čas dokončení této lekce: **15 minut**</span><span class="sxs-lookup"><span data-stu-id="803fa-109">Estimated time to complete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="803fa-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="803fa-110">Prerequisites</span></span>  
<span data-ttu-id="803fa-111">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="803fa-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="803fa-112">Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 9: Vytvoření hierarchií](../tutorials/aas-lesson-9-create-hierarchies.md).</span><span class="sxs-lookup"><span data-stu-id="803fa-112">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 9: Create Hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>  
  
## <a name="create-partitions"></a><span data-ttu-id="803fa-113">Vytvoření oddílů</span><span class="sxs-lookup"><span data-stu-id="803fa-113">Create partitions</span></span>  
  
#### <a name="to-create-partitions-in-the-factinternetsales-table"></a><span data-ttu-id="803fa-114">Vytvoření oddílů v tabulce FactInternetSales</span><span class="sxs-lookup"><span data-stu-id="803fa-114">To create partitions in the FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="803fa-115">V Průzkumníku tabelárních modelů rozbalte **Tabulky** a potom klikněte pravým tlačítkem na **FactInternetSales** > **Oddíly**.</span><span class="sxs-lookup"><span data-stu-id="803fa-115">In Tabular Model Explorer, expand **Tables**, and then right-click **FactInternetSales** > **Partitions**.</span></span>  
  
2.  <span data-ttu-id="803fa-116">Ve správci oddílů klikněte na **Kopírovat** a potom změňte název na **FactInternetSales2010**.</span><span class="sxs-lookup"><span data-stu-id="803fa-116">In Partition Manager, click **Copy**, and then change the name to **FactInternetSales2010**.</span></span>
  
    <span data-ttu-id="803fa-117">Protože chcete, aby oddíl obsahoval pouze řádky z určitého období (rok 2010), musíte upravit výraz dotazu.</span><span class="sxs-lookup"><span data-stu-id="803fa-117">Because you want the partition to include only those rows within a certain period, for the year 2010, you must modify the query expression.</span></span>
  
4.  <span data-ttu-id="803fa-118">Kliknutím na **Návrh** otevřete editor dotazů a klikněte na dotaz **FactInternetSales2010**.</span><span class="sxs-lookup"><span data-stu-id="803fa-118">Click **Design** to open Query Editor, and then click the **FactInternetSales2010** query.</span></span>

5.  <span data-ttu-id="803fa-119">V náhledu klikněte na šipku dolů v záhlaví sloupce **OrderDate** a potom klikněte na **Filtry data a času** > **Mezi**.</span><span class="sxs-lookup"><span data-stu-id="803fa-119">In preview, click the down arrow in the **OrderDate** column heading, and then click **Date/Time Filters** > **Between**.</span></span>

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  <span data-ttu-id="803fa-121">V dialogovém okně Filtrovat řádky v části **Zobrazit řádky, kde: OrderDate** ponechte možnost **je po nebo přesně** a do pole kalendářního data zadejte **1. 1. 2010**.</span><span class="sxs-lookup"><span data-stu-id="803fa-121">In the Filter Rows dialog box, in **Show rows where: OrderDate**, leave **is after or equal to**, and then in the date field, enter **1/1/2010**.</span></span> <span data-ttu-id="803fa-122">Ponechte vybraný operátor **A**, vyberte možnost **je před**, do pole kalendářního data zadejte **1. 1. 2011** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="803fa-122">Leave the **And** operator selected, then select **is before**, then in the date field, enter **1/1/2011**, and then click **OK**.</span></span>

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    <span data-ttu-id="803fa-124">Všimněte si, že v editoru dotazů se v části POUŽITÝ POSTUP zobrazí další krok s názvem Filtrované řádky.</span><span class="sxs-lookup"><span data-stu-id="803fa-124">Notice in Query Editor, in APPLIED STEPS, you see another step named Filtered Rows.</span></span> <span data-ttu-id="803fa-125">Tento filtr slouží k výběru dat objednávek pouze z roku 2010.</span><span class="sxs-lookup"><span data-stu-id="803fa-125">This filter is to select only order dates from 2010.</span></span>

8.  <span data-ttu-id="803fa-126">Klikněte na **Importovat**.</span><span class="sxs-lookup"><span data-stu-id="803fa-126">Click **Import**.</span></span>

    <span data-ttu-id="803fa-127">Ve správci oddílů si všimněte, že výraz dotazu teď obsahuje další klauzuli Filtrované řádky.</span><span class="sxs-lookup"><span data-stu-id="803fa-127">In Partition Manager, notice the query expression now has an additional Filtered Rows clause.</span></span>

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    <span data-ttu-id="803fa-129">Tento příkaz určuje, že by tento oddíl měl zahrnovat pouze data v řádcích, kde hodnota ve sloupci OrderDate spadá do kalendářního roku 2010, jak je uvedeno v klauzuli Filtrované řádky.</span><span class="sxs-lookup"><span data-stu-id="803fa-129">This statement specifies this partition should include only the data in those rows where the OrderDate is in the 2010 calendar year as specified in the filtered rows clause.</span></span>  
  
  
#### <a name="to-create-a-partition-for-the-2011-year"></a><span data-ttu-id="803fa-130">Vytvoření oddílu pro rok 2011</span><span class="sxs-lookup"><span data-stu-id="803fa-130">To create a partition for the 2011 year</span></span>  
  
1.  <span data-ttu-id="803fa-131">V seznamu oddílů klikněte na oddíl **FactInternetSales2010**, který jste vytvořili, a potom klikněte na **Kopírovat**.</span><span class="sxs-lookup"><span data-stu-id="803fa-131">In the partitions list, click the **FactInternetSales2010** partition you created, and then click **Copy**.</span></span>  <span data-ttu-id="803fa-132">Změňte název oddílu na **FactInternetSales2011**.</span><span class="sxs-lookup"><span data-stu-id="803fa-132">Change the partition name to **FactInternetSales2011**.</span></span> 

    <span data-ttu-id="803fa-133">Není nutné použít editor dotazů k vytvoření nové klauzule Filtrované řádky.</span><span class="sxs-lookup"><span data-stu-id="803fa-133">You do not need to use Query Editor to create a new filtered rows clause.</span></span> <span data-ttu-id="803fa-134">Vzhledem k tomu, že jste vytvořili kopii dotazu pro rok 2010, stačí provést drobnou změnu v dotazu pro rok 2011.</span><span class="sxs-lookup"><span data-stu-id="803fa-134">Because you created a copy of the query for 2010, all you need to do is make a slight change in the query for 2011.</span></span>
  
2.  <span data-ttu-id="803fa-135">Aby tento oddíl obsahoval pouze řádky pro rok 2011, ve **výrazu dotazu** nahraďte roky v klauzuli Filtrované řádky za **2011** a **2012**:</span><span class="sxs-lookup"><span data-stu-id="803fa-135">In **Query Expression**, in-order for this partition to include only those rows for the 2011 year, replace the years in the Filtered Rows clause with **2011** and **2012**, respectively, like:</span></span>  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="to-create-partitions-for-2012-2013-and-2014"></a><span data-ttu-id="803fa-136">Vytvoření oddílů pro roky 2012, 2013 a 2014</span><span class="sxs-lookup"><span data-stu-id="803fa-136">To create partitions for 2012, 2013, and 2014.</span></span>  
  
- <span data-ttu-id="803fa-137">Stejným postupem jako v předchozích krocích vytvořte oddíly pro roky 2012, 2013 a 2014 a v klauzuli Filtrované řádky vždy změňte rok tak, aby se zahrnuly pouze řádky pro daný rok.</span><span class="sxs-lookup"><span data-stu-id="803fa-137">Follow the previous steps, creating partitions for 2012, 2013, and 2014, changing the years in the Filtered Rows clause to include only rows for that year.</span></span> 
  

## <a name="delete-the-factinternetsales-partition"></a><span data-ttu-id="803fa-138">Odstranění oddílu FactInternetSales</span><span class="sxs-lookup"><span data-stu-id="803fa-138">Delete the FactInternetSales partition</span></span>
<span data-ttu-id="803fa-139">Když teď máte oddíly pro každý rok, můžete odstranit oddíl FactInternetSales. Zabráníte tím překrývání při výběru možnosti Zpracovat vše v rámci zpracování oddílů.</span><span class="sxs-lookup"><span data-stu-id="803fa-139">Now that you have partitions for each year, you can delete the FactInternetSales partition; preventing overlap when choosing Process all when processing partitions.</span></span>

#### <a name="to-delete-the-factinternetsales-partition"></a><span data-ttu-id="803fa-140">Odstranění oddílu FactInternetSales</span><span class="sxs-lookup"><span data-stu-id="803fa-140">To delete the FactInternetSales partition</span></span>
-  <span data-ttu-id="803fa-141">Klikněte na oddíl FactInternetSales a potom klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="803fa-141">Click the FactInternetSales partition, and then click **Delete**.</span></span>



## <a name="process-partitions"></a><span data-ttu-id="803fa-142">Zpracování oddílů</span><span class="sxs-lookup"><span data-stu-id="803fa-142">Process partitions</span></span>  
<span data-ttu-id="803fa-143">Ve správci oddílů si všimněte, že sloupec **Poslední zpracování** pro každý z nově vytvořených oddílů ukazuje, že oddíly nikdy nebyly zpracovány.</span><span class="sxs-lookup"><span data-stu-id="803fa-143">In Partition Manager, notice the **Last Processed** column for each of the new partitions you created shows these partitions have never been processed.</span></span> <span data-ttu-id="803fa-144">Při vytváření oddílů byste měli spustit operaci Zpracování oddílů nebo Zpracování tabulky pro aktualizaci dat v těchto oddílech.</span><span class="sxs-lookup"><span data-stu-id="803fa-144">When you create partitions, you should run a Process Partitions or Process Table operation to refresh the data in those partitions.</span></span>  
  
#### <a name="to-process-the-factinternetsales-partitions"></a><span data-ttu-id="803fa-145">Zpracování oddílů FactInternetSales</span><span class="sxs-lookup"><span data-stu-id="803fa-145">To process the FactInternetSales partitions</span></span>  
  
1.  <span data-ttu-id="803fa-146">Kliknutím na **OK** zavřete správce oddílů.</span><span class="sxs-lookup"><span data-stu-id="803fa-146">Click **OK** to close Partition Manager.</span></span>  
  
2.  <span data-ttu-id="803fa-147">Klikněte na tabulku **FactInternetSales** a potom na nabídku **Model** > **Zpracovat** > **Zpracovat oddíly**.</span><span class="sxs-lookup"><span data-stu-id="803fa-147">Click the **FactInternetSales** table, then click the **Model** menu > **Process** > **Process Partitions**.</span></span>  
  
3.  <span data-ttu-id="803fa-148">V dialogovém okně Zpracovat oddíly zkontrolujte, že je **Režim** nastavený na **Zpracovat výchozí**.</span><span class="sxs-lookup"><span data-stu-id="803fa-148">In the Process Partitions dialog box, verify **Mode** is set to **Process Default**.</span></span>  
  
4.  <span data-ttu-id="803fa-149">Ve sloupci **Zpracovat** zaškrtněte políčko pro každý z pěti oddílů, které jste vytvořili, a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="803fa-149">Select the checkbox in the **Process** column for each of the five partitions you created, and then click **OK**.</span></span>  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    <span data-ttu-id="803fa-151">Pokud se zobrazí výzva k zadání přihlašovacích údajů k zosobnění, zadejte uživatelské jméno a heslo systému Windows, které jste zadali v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="803fa-151">If you're prompted for Impersonation credentials, enter the Windows user name and password you specified in Lesson 2.</span></span>  
  
    <span data-ttu-id="803fa-152">Zobrazí se dialogové okno **Zpracování dat** s podrobnostmi o zpracování pro každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="803fa-152">The **Data Processing** dialog box appears and displays process details for each partition.</span></span> <span data-ttu-id="803fa-153">Všimněte si, že pro každý oddíl se přenáší jiný počet řádků.</span><span class="sxs-lookup"><span data-stu-id="803fa-153">Notice that a different number of rows for each partition are transferred.</span></span> <span data-ttu-id="803fa-154">Každý oddíl obsahuje pouze řádky pro rok zadaný v klauzuli WHERE příkazu jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="803fa-154">Each partition includes only those rows for the year specified in the WHERE clause in the SQL Statement.</span></span> <span data-ttu-id="803fa-155">Po dokončení zpracování zavřete dialogové okno Zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="803fa-155">When processing is finished, go ahead and close the Data Processing dialog box.</span></span>  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a><span data-ttu-id="803fa-157">Co dále?</span><span class="sxs-lookup"><span data-stu-id="803fa-157">What's next?</span></span>
<span data-ttu-id="803fa-158">Přejděte na další lekci: [Lekce 11: Vytvoření rolí](../tutorials/aas-lesson-11-create-roles.md).</span><span class="sxs-lookup"><span data-stu-id="803fa-158">Go to the next lesson: [Lesson 11: Create Roles](../tutorials/aas-lesson-11-create-roles.md).</span></span> 
