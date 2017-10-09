---
<span data-ttu-id="6808e-101">Title: aaa "Azure Analysis Services kurz lekce 10: vytvoření oddílů | Microsoft Docs"Popis: Popisuje, jak toocreate oddíly v kurzu projektu hello Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6808e-101">title: aaa"Azure Analysis Services tutorial lesson 10: Create partitions | Microsoft Docs" description: Describes how toocreate partitions in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="6808e-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="6808e-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="6808e-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="6808e-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-10-create-partitions"></a><span data-ttu-id="6808e-104">Lekce 10: Vytvoření oddílů</span><span class="sxs-lookup"><span data-stu-id="6808e-104">Lesson 10: Create partitions</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="6808e-105">V této lekci vytvoříte tabulka FactInternetSales hello toodivide oddíly do menších logických částí, které mohou být zpracován (Aktualizovat) bez ohledu na ostatní oddíly.</span><span class="sxs-lookup"><span data-stu-id="6808e-105">In this lesson, you create partitions toodivide hello FactInternetSales table into smaller logical parts that can be processed (refreshed) independent of other partitions.</span></span> <span data-ttu-id="6808e-106">Ve výchozím nastavení má každá tabulka, které můžete zahrnout do modelu jeden oddíl, který zahrnuje všechny hello tabulka sloupce a řádky.</span><span class="sxs-lookup"><span data-stu-id="6808e-106">By default, every table you include in your model has one partition, which includes all hello table’s columns and rows.</span></span> <span data-ttu-id="6808e-107">Pro tabulka FactInternetSales hello chceme toodivide hello dat po letech; jeden oddíl pro každou tabulku hello pět let.</span><span class="sxs-lookup"><span data-stu-id="6808e-107">For hello FactInternetSales table, we want toodivide hello data by year; one partition for each of hello table’s five years.</span></span> <span data-ttu-id="6808e-108">Jednotlivé oddíly pak bude možné zpracovávat nezávisle na sobě.</span><span class="sxs-lookup"><span data-stu-id="6808e-108">Each partition can then be processed independently.</span></span> <span data-ttu-id="6808e-109">Další, najdete v části toolearn [oddíly](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="6808e-109">toolearn more, see [Partitions](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span></span> 
  
<span data-ttu-id="6808e-110">Odhadovaný čas toocomplete této lekci: **15 minut**</span><span class="sxs-lookup"><span data-stu-id="6808e-110">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="6808e-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6808e-111">Prerequisites</span></span>  
<span data-ttu-id="6808e-112">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6808e-112">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="6808e-113">Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 9: vytvoření hierarchie](../tutorials/aas-lesson-9-create-hierarchies.md).</span><span class="sxs-lookup"><span data-stu-id="6808e-113">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 9: Create Hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>  
  
## <a name="create-partitions"></a><span data-ttu-id="6808e-114">Vytvoření oddílů</span><span class="sxs-lookup"><span data-stu-id="6808e-114">Create partitions</span></span>  
  
#### <a name="toocreate-partitions-in-hello-factinternetsales-table"></a><span data-ttu-id="6808e-115">toocreate oddílů v tabulce FactInternetSales hello</span><span class="sxs-lookup"><span data-stu-id="6808e-115">toocreate partitions in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="6808e-116">V Průzkumníku tabelárních modelů rozbalte **Tabulky** a potom klikněte pravým tlačítkem na **FactInternetSales** > **Oddíly**.</span><span class="sxs-lookup"><span data-stu-id="6808e-116">In Tabular Model Explorer, expand **Tables**, and then right-click **FactInternetSales** > **Partitions**.</span></span>  
  
2.  <span data-ttu-id="6808e-117">Ve Správci oddílů, klikněte na tlačítko **kopie**a potom změňte název hello příliš**FactInternetSales2010**.</span><span class="sxs-lookup"><span data-stu-id="6808e-117">In Partition Manager, click **Copy**, and then change hello name too**FactInternetSales2010**.</span></span>
  
    <span data-ttu-id="6808e-118">Protože chcete pouze řádky, hello oddílu tooinclude v určitém období, pro hello roku 2010, musíte upravit hello výrazu dotazu.</span><span class="sxs-lookup"><span data-stu-id="6808e-118">Because you want hello partition tooinclude only those rows within a certain period, for hello year 2010, you must modify hello query expression.</span></span>
  
4.  <span data-ttu-id="6808e-119">Klikněte na tlačítko **návrhu** tooopen dotaz Editor a potom klikněte na hello **FactInternetSales2010** dotazu.</span><span class="sxs-lookup"><span data-stu-id="6808e-119">Click **Design** tooopen Query Editor, and then click hello **FactInternetSales2010** query.</span></span>

5.  <span data-ttu-id="6808e-120">Ve verzi preview, klikněte na tlačítko hello v hello šipka dolů **OrderDate** záhlaví sloupce a pak klikněte na tlačítko **datum a čas filtry** > **mezi**.</span><span class="sxs-lookup"><span data-stu-id="6808e-120">In preview, click hello down arrow in hello **OrderDate** column heading, and then click **Date/Time Filters** > **Between**.</span></span>

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  <span data-ttu-id="6808e-122">V hello Filtr řádků dialogové okno v **zobrazit tyto řádky: OrderDate**, nechte **je po nebo je rovno**a potom zadejte do pole pro datum hello **1. 1. 2010**.</span><span class="sxs-lookup"><span data-stu-id="6808e-122">In hello Filter Rows dialog box, in **Show rows where: OrderDate**, leave **is after or equal to**, and then in hello date field, enter **1/1/2010**.</span></span> <span data-ttu-id="6808e-123">Nechte hello **a** operátor vybraná, pak vyberte **před**, potom zadejte do pole pro datum hello, **1/1/2011**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6808e-123">Leave hello **And** operator selected, then select **is before**, then in hello date field, enter **1/1/2011**, and then click **OK**.</span></span>

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    <span data-ttu-id="6808e-125">Všimněte si, že v editoru dotazů se v části POUŽITÝ POSTUP zobrazí další krok s názvem Filtrované řádky.</span><span class="sxs-lookup"><span data-stu-id="6808e-125">Notice in Query Editor, in APPLIED STEPS, you see another step named Filtered Rows.</span></span> <span data-ttu-id="6808e-126">Tento filtr je tooselect pouze data objednávky z 2010.</span><span class="sxs-lookup"><span data-stu-id="6808e-126">This filter is tooselect only order dates from 2010.</span></span>

8.  <span data-ttu-id="6808e-127">Klikněte na **Importovat**.</span><span class="sxs-lookup"><span data-stu-id="6808e-127">Click **Import**.</span></span>

    <span data-ttu-id="6808e-128">Ve Správci oddílů, Všimněte si hello dotazů výraz teď obsahuje další klauzuli filtrované řádky.</span><span class="sxs-lookup"><span data-stu-id="6808e-128">In Partition Manager, notice hello query expression now has an additional Filtered Rows clause.</span></span>

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    <span data-ttu-id="6808e-130">Tento příkaz určuje, že tento oddíl obsahovat pouze hello data v těchto řádcích hello OrderDate je-li hello kalendářním roce 2010 jako zadaný v klauzuli hello filtrované řádky.</span><span class="sxs-lookup"><span data-stu-id="6808e-130">This statement specifies this partition should include only hello data in those rows where hello OrderDate is in hello 2010 calendar year as specified in hello filtered rows clause.</span></span>  
  
  
#### <a name="toocreate-a-partition-for-hello-2011-year"></a><span data-ttu-id="6808e-131">toocreate oddíl pro hello roku 2011</span><span class="sxs-lookup"><span data-stu-id="6808e-131">toocreate a partition for hello 2011 year</span></span>  
  
1.  <span data-ttu-id="6808e-132">V seznamu hello oddíly, klikněte na tlačítko hello **FactInternetSales2010** oddílu, kterou jste vytvořili a potom klikněte na **kopie**.</span><span class="sxs-lookup"><span data-stu-id="6808e-132">In hello partitions list, click hello **FactInternetSales2010** partition you created, and then click **Copy**.</span></span>  <span data-ttu-id="6808e-133">Změňte název oddílu hello příliš**FactInternetSales2011**.</span><span class="sxs-lookup"><span data-stu-id="6808e-133">Change hello partition name too**FactInternetSales2011**.</span></span> 

    <span data-ttu-id="6808e-134">Není nutné toouse Editor dotazů toocreate nové klauzule filtrované řádky.</span><span class="sxs-lookup"><span data-stu-id="6808e-134">You do not need toouse Query Editor toocreate a new filtered rows clause.</span></span> <span data-ttu-id="6808e-135">Vzhledem k tomu, že vytvoříte kopii hello dotazu pro 2010, je vše, co potřebujete toodo, změňte mírné v hello dotazu pro 2011.</span><span class="sxs-lookup"><span data-stu-id="6808e-135">Because you created a copy of hello query for 2010, all you need toodo is make a slight change in hello query for 2011.</span></span>
  
2.  <span data-ttu-id="6808e-136">V **výrazu dotazu**, v pořadí pro tento oddíl tooinclude pouze řádky pro hello rok 2011, nahraďte hello let v klauzuli hello filtrované řádky s **2011** a **2012**, jako jsou:</span><span class="sxs-lookup"><span data-stu-id="6808e-136">In **Query Expression**, in-order for this partition tooinclude only those rows for hello 2011 year, replace hello years in hello Filtered Rows clause with **2011** and **2012**, respectively, like:</span></span>  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="toocreate-partitions-for-2012-2013-and-2014"></a><span data-ttu-id="6808e-137">pro 2012, 2013 a 2014 toocreate oddíly.</span><span class="sxs-lookup"><span data-stu-id="6808e-137">toocreate partitions for 2012, 2013, and 2014.</span></span>  
  
- <span data-ttu-id="6808e-138">Postupujte podle předchozích kroků hello, vytváření oddílů pro 2012, 2013 a 2014, změna hello let v hello filtrované řádky klauzule tooinclude pouze řádky pro tohoto roku.</span><span class="sxs-lookup"><span data-stu-id="6808e-138">Follow hello previous steps, creating partitions for 2012, 2013, and 2014, changing hello years in hello Filtered Rows clause tooinclude only rows for that year.</span></span> 
  

## <a name="delete-hello-factinternetsales-partition"></a><span data-ttu-id="6808e-139">Odstranit oddíl FactInternetSales hello</span><span class="sxs-lookup"><span data-stu-id="6808e-139">Delete hello FactInternetSales partition</span></span>
<span data-ttu-id="6808e-140">Nyní když máte oddíly pro každý rok, můžete odstranit hello FactInternetSales oddíl. Při výběru zpracovat všechny při zpracování oddílů, brání překrývají.</span><span class="sxs-lookup"><span data-stu-id="6808e-140">Now that you have partitions for each year, you can delete hello FactInternetSales partition; preventing overlap when choosing Process all when processing partitions.</span></span>

#### <a name="toodelete-hello-factinternetsales-partition"></a><span data-ttu-id="6808e-141">hello toodelete FactInternetSales oddílu</span><span class="sxs-lookup"><span data-stu-id="6808e-141">toodelete hello FactInternetSales partition</span></span>
-  <span data-ttu-id="6808e-142">Klikněte na možnost FactInternetSales oddílu hello a pak klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6808e-142">Click hello FactInternetSales partition, and then click **Delete**.</span></span>



## <a name="process-partitions"></a><span data-ttu-id="6808e-143">Zpracování oddílů</span><span class="sxs-lookup"><span data-stu-id="6808e-143">Process partitions</span></span>  
<span data-ttu-id="6808e-144">Ve Správci oddílů, Všimněte si hello **poslední zpracovat** sloupec pro každou hello nové oddíly, které jste vytvořili, zobrazuje tyto oddíly nikdy byly zpracovány.</span><span class="sxs-lookup"><span data-stu-id="6808e-144">In Partition Manager, notice hello **Last Processed** column for each of hello new partitions you created shows these partitions have never been processed.</span></span> <span data-ttu-id="6808e-145">Při vytváření oddílů, byste měli spustit proces oddíly nebo zpracovat tabulku operaci toorefresh hello data v těchto oddílech.</span><span class="sxs-lookup"><span data-stu-id="6808e-145">When you create partitions, you should run a Process Partitions or Process Table operation toorefresh hello data in those partitions.</span></span>  
  
#### <a name="tooprocess-hello-factinternetsales-partitions"></a><span data-ttu-id="6808e-146">tooprocess hello FactInternetSales oddíly</span><span class="sxs-lookup"><span data-stu-id="6808e-146">tooprocess hello FactInternetSales partitions</span></span>  
  
1.  <span data-ttu-id="6808e-147">Klikněte na tlačítko **OK** tooclose Správce oddílů.</span><span class="sxs-lookup"><span data-stu-id="6808e-147">Click **OK** tooclose Partition Manager.</span></span>  
  
2.  <span data-ttu-id="6808e-148">Klikněte na tlačítko hello **FactInternetSales** tabulky a pak klikněte na hello **modelu** nabídky > **proces** > **proces oddíly**.</span><span class="sxs-lookup"><span data-stu-id="6808e-148">Click hello **FactInternetSales** table, then click hello **Model** menu > **Process** > **Process Partitions**.</span></span>  
  
3.  <span data-ttu-id="6808e-149">V dialogovém okně proces oddíly hello, ověřte **režimu** je nastaven příliš**výchozí proces**.</span><span class="sxs-lookup"><span data-stu-id="6808e-149">In hello Process Partitions dialog box, verify **Mode** is set too**Process Default**.</span></span>  
  
4.  <span data-ttu-id="6808e-150">Zaškrtněte políčko hello v hello **proces** sloupec pro každou hello pět oddíly jste vytvořili a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6808e-150">Select hello checkbox in hello **Process** column for each of hello five partitions you created, and then click **OK**.</span></span>  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    <span data-ttu-id="6808e-152">Pokud se zobrazí výzva pro pověření k zosobnění, zadejte uživatelské jméno systému Windows hello a heslo, které jste zadali v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="6808e-152">If you're prompted for Impersonation credentials, enter hello Windows user name and password you specified in Lesson 2.</span></span>  
  
    <span data-ttu-id="6808e-153">Hello **zpracování dat** se zobrazí dialogové okno a zobrazí podrobnosti o procesu pro každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="6808e-153">hello **Data Processing** dialog box appears and displays process details for each partition.</span></span> <span data-ttu-id="6808e-154">Všimněte si, že pro každý oddíl se přenáší jiný počet řádků.</span><span class="sxs-lookup"><span data-stu-id="6808e-154">Notice that a different number of rows for each partition are transferred.</span></span> <span data-ttu-id="6808e-155">Každý oddíl obsahuje pouze řádky hello roku uvedený v hello klauzule WHERE v příkazu SQL hello.</span><span class="sxs-lookup"><span data-stu-id="6808e-155">Each partition includes only those rows for hello year specified in hello WHERE clause in hello SQL Statement.</span></span> <span data-ttu-id="6808e-156">Po dokončení zpracování pokračujte a zavřete dialogové okno zpracování dat hello.</span><span class="sxs-lookup"><span data-stu-id="6808e-156">When processing is finished, go ahead and close hello Data Processing dialog box.</span></span>  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a><span data-ttu-id="6808e-158">Co dále?</span><span class="sxs-lookup"><span data-stu-id="6808e-158">What's next?</span></span>
<span data-ttu-id="6808e-159">Další lekce přejděte toohello: [lekce 11: vytvoření role](../tutorials/aas-lesson-11-create-roles.md).</span><span class="sxs-lookup"><span data-stu-id="6808e-159">Go toohello next lesson: [Lesson 11: Create Roles](../tutorials/aas-lesson-11-create-roles.md).</span></span> 
