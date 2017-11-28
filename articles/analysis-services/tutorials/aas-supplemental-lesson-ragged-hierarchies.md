---
<span data-ttu-id="ec481-101">Title: aaa "kurz dodatečné lekce Azure Analysis Services: nepravidelné hierarchie s logickými | Microsoft Docs"Popis: Popisuje, jak toofix nepravidelné logickými hierarchií v kurzu hello Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="ec481-101">title: aaa"Azure Analysis Services tutorial supplemental lesson: Ragged hierarchies | Microsoft Docs" description: Describes how toofix ragged hierarchies in hello Azure Analysis Services tutorial.</span></span>
<span data-ttu-id="ec481-102">služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "</span><span class="sxs-lookup"><span data-stu-id="ec481-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="ec481-103">MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="ec481-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="supplemental-lesson---ragged-hierarchies"></a><span data-ttu-id="ec481-104">Doplňková lekce – Nepravidelné hierarchie</span><span class="sxs-lookup"><span data-stu-id="ec481-104">Supplemental lesson - Ragged hierarchies</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="ec481-105">V této doplňkové lekci vyřešíte běžný problém vyskytující se při přesunu do hierarchií, které obsahují prázdné hodnoty (členy) na různých úrovních.</span><span class="sxs-lookup"><span data-stu-id="ec481-105">In this supplemental lesson, you resolve a common problem when pivoting on hierarchies that contain blank values (members) at different levels.</span></span> <span data-ttu-id="ec481-106">Může se to například týkat organizace, ve které vysoce postavenému manažerovi přímo reportují manažeři a jiní zaměstnanci oddělení.</span><span class="sxs-lookup"><span data-stu-id="ec481-106">For example, an organization where a high-level manager has both departmental managers and non-managers as direct reports.</span></span> <span data-ttu-id="ec481-107">Nebo se může jednat o geografické hierarchie skládající se z položek Země, Oblast, Město, kde některá města nemají nadřazený stát nebo kraj, například Washington D. C. nebo Vatikán.</span><span class="sxs-lookup"><span data-stu-id="ec481-107">Or, geographic hierarchies composed of Country-Region-City, where some cities lack a parent State or Province, such as Washington D.C., Vatican City.</span></span> <span data-ttu-id="ec481-108">Hierarchie má prázdné členy, je často descends toodifferent nebo nepravidelné logickými úrovně.</span><span class="sxs-lookup"><span data-stu-id="ec481-108">When a hierarchy has blank members, it often descends toodifferent, or ragged, levels.</span></span>

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

<span data-ttu-id="ec481-110">Tabulkové modely na úrovni kompatibility hello 1400 mají další **skrýt členy** vlastnost pro hierarchie.</span><span class="sxs-lookup"><span data-stu-id="ec481-110">Tabular models at hello 1400 compatibility level have an additional **Hide Members** property for hierarchies.</span></span> <span data-ttu-id="ec481-111">Hello **výchozí** nastavení předpokládá, že neexistují členové, prázdné jakékoli úrovni.</span><span class="sxs-lookup"><span data-stu-id="ec481-111">hello **Default** setting assumes there are no blank members at any level.</span></span> <span data-ttu-id="ec481-112">Hello **skrýt prázdné členy** nastavení vyloučí prázdné členy z hierarchie hello při přidání tooa kontingenční tabulku nebo sestavy.</span><span class="sxs-lookup"><span data-stu-id="ec481-112">hello **Hide blank members** setting excludes blank members from hello hierarchy when added tooa PivotTable or report.</span></span>  
  
<span data-ttu-id="ec481-113">Odhadovaný čas toocomplete této lekci: **20 minut**</span><span class="sxs-lookup"><span data-stu-id="ec481-113">Estimated time toocomplete this lesson: **20 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="ec481-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec481-114">Prerequisites</span></span>  
<span data-ttu-id="ec481-115">Toto téma doplňkové lekce je součástí kurzu tabulkového modelování.</span><span class="sxs-lookup"><span data-stu-id="ec481-115">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="ec481-116">Před provedením úlohy hello v této další lekci, by byly dokončeny všechny předchozí lekce nebo máte dokončený projekt modelu ukázkové společnosti Adventure Works Internet prodej.</span><span class="sxs-lookup"><span data-stu-id="ec481-116">Before performing hello tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span> 

<span data-ttu-id="ec481-117">Pokud vytvoříte projekt AW Internet prodej hello v rámci kurzu hello modelu ještě neobsahuje data ani hierarchie, které jsou nepravidelné.</span><span class="sxs-lookup"><span data-stu-id="ec481-117">If you've created hello AW Internet Sales project as part of hello tutorial, your model does not yet contain any data or hierarchies that are ragged.</span></span> <span data-ttu-id="ec481-118">toocomplete tento doplňkový lekce, musíte se napřed toocreate hello problém tak, že přidáte některé další tabulky, vytvořte relace, počítané sloupce, míry a nové hierarchie organizace.</span><span class="sxs-lookup"><span data-stu-id="ec481-118">toocomplete this supplemental lesson, you first have toocreate hello problem by adding some additional tables, create relationships, calculated columns, a measure, and a new Organization hierarchy.</span></span> <span data-ttu-id="ec481-119">Tato část zabere přibližně 15 minut.</span><span class="sxs-lookup"><span data-stu-id="ec481-119">That part takes about 15 minutes.</span></span> <span data-ttu-id="ec481-120">Potom můžete získat toosolve ho za několik minut.</span><span class="sxs-lookup"><span data-stu-id="ec481-120">Then, you get toosolve it in just a few minutes.</span></span>  

## <a name="add-tables-and-objects"></a><span data-ttu-id="ec481-121">Přidání tabulek a objektů</span><span class="sxs-lookup"><span data-stu-id="ec481-121">Add tables and objects</span></span>
  
### <a name="tooadd-new-tables-tooyour-model"></a><span data-ttu-id="ec481-122">tooadd nové tabulky tooyour modelu</span><span class="sxs-lookup"><span data-stu-id="ec481-122">tooadd new tables tooyour model</span></span>
  
1.  <span data-ttu-id="ec481-123">V Průzkumníku tabulkových modelů rozbalte položku **Zdroje Dat**, potom klikněte pravým tlačítkem myši na vaše připojení > **Importovat nové tabulky**.</span><span class="sxs-lookup"><span data-stu-id="ec481-123">In Tabular Model Explorer, expand **Data Sources**, then right-click your connection > **Import New Tables**.</span></span>
  
2.  <span data-ttu-id="ec481-124">V Navigátoru vyberte **DimEmployee** a **FactResellerSales** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec481-124">In Navigator, select **DimEmployee** and **FactResellerSales**, and then click **OK**.</span></span>

3.  <span data-ttu-id="ec481-125">V Editoru dotazů klikněte na **Importovat**.</span><span class="sxs-lookup"><span data-stu-id="ec481-125">In Query Editor, click **Import**</span></span>

4.  <span data-ttu-id="ec481-126">Vytvořte následující hello [vztahy](../tutorials/aas-lesson-4-create-relationships.md):</span><span class="sxs-lookup"><span data-stu-id="ec481-126">Create hello following [relationships](../tutorials/aas-lesson-4-create-relationships.md):</span></span>

    | <span data-ttu-id="ec481-127">Tabulka 1</span><span class="sxs-lookup"><span data-stu-id="ec481-127">Table 1</span></span>           | <span data-ttu-id="ec481-128">Sloupec</span><span class="sxs-lookup"><span data-stu-id="ec481-128">Column</span></span>       | <span data-ttu-id="ec481-129">Směr filtru</span><span class="sxs-lookup"><span data-stu-id="ec481-129">Filter Direction</span></span>   | <span data-ttu-id="ec481-130">Tabulka 2</span><span class="sxs-lookup"><span data-stu-id="ec481-130">Table 2</span></span>     | <span data-ttu-id="ec481-131">Sloupec</span><span class="sxs-lookup"><span data-stu-id="ec481-131">Column</span></span>      | <span data-ttu-id="ec481-132">Aktivní</span><span class="sxs-lookup"><span data-stu-id="ec481-132">Active</span></span> |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | <span data-ttu-id="ec481-133">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="ec481-133">FactResellerSales</span></span> | <span data-ttu-id="ec481-134">OrderDateKey</span><span class="sxs-lookup"><span data-stu-id="ec481-134">OrderDateKey</span></span> | <span data-ttu-id="ec481-135">Výchozí</span><span class="sxs-lookup"><span data-stu-id="ec481-135">Default</span></span>            | <span data-ttu-id="ec481-136">DimDate</span><span class="sxs-lookup"><span data-stu-id="ec481-136">DimDate</span></span>     | <span data-ttu-id="ec481-137">Datum</span><span class="sxs-lookup"><span data-stu-id="ec481-137">Date</span></span>        | <span data-ttu-id="ec481-138">Ano</span><span class="sxs-lookup"><span data-stu-id="ec481-138">Yes</span></span>    |
    | <span data-ttu-id="ec481-139">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="ec481-139">FactResellerSales</span></span> | <span data-ttu-id="ec481-140">DueDate</span><span class="sxs-lookup"><span data-stu-id="ec481-140">DueDate</span></span>      | <span data-ttu-id="ec481-141">Výchozí</span><span class="sxs-lookup"><span data-stu-id="ec481-141">Default</span></span>            | <span data-ttu-id="ec481-142">DimDate</span><span class="sxs-lookup"><span data-stu-id="ec481-142">DimDate</span></span>     | <span data-ttu-id="ec481-143">Datum</span><span class="sxs-lookup"><span data-stu-id="ec481-143">Date</span></span>        | <span data-ttu-id="ec481-144">Ne</span><span class="sxs-lookup"><span data-stu-id="ec481-144">No</span></span>     |
    | <span data-ttu-id="ec481-145">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="ec481-145">FactResellerSales</span></span> | <span data-ttu-id="ec481-146">ShipDateKey</span><span class="sxs-lookup"><span data-stu-id="ec481-146">ShipDateKey</span></span>  | <span data-ttu-id="ec481-147">Výchozí</span><span class="sxs-lookup"><span data-stu-id="ec481-147">Default</span></span>            | <span data-ttu-id="ec481-148">DimDate</span><span class="sxs-lookup"><span data-stu-id="ec481-148">DimDate</span></span>     | <span data-ttu-id="ec481-149">Datum</span><span class="sxs-lookup"><span data-stu-id="ec481-149">Date</span></span>        | <span data-ttu-id="ec481-150">Ne</span><span class="sxs-lookup"><span data-stu-id="ec481-150">No</span></span>     |
    | <span data-ttu-id="ec481-151">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="ec481-151">FactResellerSales</span></span> | <span data-ttu-id="ec481-152">ProductKey</span><span class="sxs-lookup"><span data-stu-id="ec481-152">ProductKey</span></span>   | <span data-ttu-id="ec481-153">Výchozí</span><span class="sxs-lookup"><span data-stu-id="ec481-153">Default</span></span>            | <span data-ttu-id="ec481-154">DimProduct</span><span class="sxs-lookup"><span data-stu-id="ec481-154">DimProduct</span></span>  | <span data-ttu-id="ec481-155">ProductKey</span><span class="sxs-lookup"><span data-stu-id="ec481-155">ProductKey</span></span>  | <span data-ttu-id="ec481-156">Ano</span><span class="sxs-lookup"><span data-stu-id="ec481-156">Yes</span></span>    |
    | <span data-ttu-id="ec481-157">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="ec481-157">FactResellerSales</span></span> | <span data-ttu-id="ec481-158">EmployeeKey</span><span class="sxs-lookup"><span data-stu-id="ec481-158">EmployeeKey</span></span>  | <span data-ttu-id="ec481-159">tooBoth tabulky</span><span class="sxs-lookup"><span data-stu-id="ec481-159">tooBoth Tables</span></span> | <span data-ttu-id="ec481-160">DimEmployee</span><span class="sxs-lookup"><span data-stu-id="ec481-160">DimEmployee</span></span> | <span data-ttu-id="ec481-161">EmployeeKey</span><span class="sxs-lookup"><span data-stu-id="ec481-161">EmployeeKey</span></span> | <span data-ttu-id="ec481-162">Ano</span><span class="sxs-lookup"><span data-stu-id="ec481-162">Yes</span></span>    |

5. <span data-ttu-id="ec481-163">V hello **DimEmployee** tabulky, vytvořte následující hello [počítaných sloupců](../tutorials/aas-lesson-5-create-calculated-columns.md):</span><span class="sxs-lookup"><span data-stu-id="ec481-163">In hello **DimEmployee** table, create hello following [calculated columns](../tutorials/aas-lesson-5-create-calculated-columns.md):</span></span> 

    <span data-ttu-id="ec481-164">**Cesta**</span><span class="sxs-lookup"><span data-stu-id="ec481-164">**Path**</span></span> 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    <span data-ttu-id="ec481-165">**Úplný název**</span><span class="sxs-lookup"><span data-stu-id="ec481-165">**FullName**</span></span> 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    <span data-ttu-id="ec481-166">**Level1**</span><span class="sxs-lookup"><span data-stu-id="ec481-166">**Level1**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    <span data-ttu-id="ec481-167">**Level2**</span><span class="sxs-lookup"><span data-stu-id="ec481-167">**Level2**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    <span data-ttu-id="ec481-168">**Level3**</span><span class="sxs-lookup"><span data-stu-id="ec481-168">**Level3**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    <span data-ttu-id="ec481-169">**Level4**</span><span class="sxs-lookup"><span data-stu-id="ec481-169">**Level4**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    <span data-ttu-id="ec481-170">**Level5**</span><span class="sxs-lookup"><span data-stu-id="ec481-170">**Level5**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  <span data-ttu-id="ec481-171">V hello **DimEmployee** tabulky, vytvořte [hierarchie](../tutorials/aas-lesson-9-create-hierarchies.md) s názvem **organizace**.</span><span class="sxs-lookup"><span data-stu-id="ec481-171">In hello **DimEmployee** table, create a [hierarchy](../tutorials/aas-lesson-9-create-hierarchies.md) named **Organization**.</span></span> <span data-ttu-id="ec481-172">Přidat hello následující sloupce v daném pořadí: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.</span><span class="sxs-lookup"><span data-stu-id="ec481-172">Add hello following columns in-order: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.</span></span>

7.  <span data-ttu-id="ec481-173">V hello **FactResellerSales** tabulky, vytvořte následující hello [měr](../tutorials/aas-lesson-6-create-measures.md):</span><span class="sxs-lookup"><span data-stu-id="ec481-173">In hello **FactResellerSales** table, create hello following [measure](../tutorials/aas-lesson-6-create-measures.md):</span></span>

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  <span data-ttu-id="ec481-174">Použití [analyzovat v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen aplikace Excel a automaticky vytvořit kontingenční tabulku.</span><span class="sxs-lookup"><span data-stu-id="ec481-174">Use [Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen Excel and automatically create a PivotTable.</span></span>

9.  <span data-ttu-id="ec481-175">V **pole kontingenční tabulky**, přidejte hello **organizace** hierarchie z hello **DimEmployee** tabulky příliš**řádky**a hello **ResellerTotalSales** míru z hello **FactResellerSales** tabulky příliš**hodnoty**.</span><span class="sxs-lookup"><span data-stu-id="ec481-175">In **PivotTable Fields**, add hello **Organization** hierarchy from hello **DimEmployee** table too**Rows**, and hello **ResellerTotalSales** measure from hello **FactResellerSales**  table too**Values**.</span></span>

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    <span data-ttu-id="ec481-177">Jak vidíte v hello kontingenční tabulky, zobrazí hello hierarchie řádků, které jsou nepravidelné.</span><span class="sxs-lookup"><span data-stu-id="ec481-177">As you can see in hello PivotTable, hello hierarchy displays rows that are ragged.</span></span> <span data-ttu-id="ec481-178">Je tam množství řádků, které zobrazují prázdné členy.</span><span class="sxs-lookup"><span data-stu-id="ec481-178">There are many rows where blank members are shown.</span></span>

## <a name="toofix-hello-ragged-hierarchy-by-setting-hello-hide-members-property"></a><span data-ttu-id="ec481-179">toofix hello nepravidelné hierarchie logickými nastavením vlastnosti hello skrýt členy</span><span class="sxs-lookup"><span data-stu-id="ec481-179">toofix hello ragged hierarchy by setting hello Hide members property</span></span>

1.  <span data-ttu-id="ec481-180">V **Průzkumníku tabulkových modelů** rozbalte **Tabulky** > **DimEmployee** > **Hierarchie** > **Organization**.</span><span class="sxs-lookup"><span data-stu-id="ec481-180">In **Tabular Model Explorer**, expand **Tables** > **DimEmployee** > **Hierarchies** > **Organization**.</span></span>

2.  <span data-ttu-id="ec481-181">V části **Vlastnosti** > **Skrýt členy** vyberte **Skrýt prázdné členy**.</span><span class="sxs-lookup"><span data-stu-id="ec481-181">In **Properties** > **Hide Members**, select **Hide blank members**.</span></span> 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  <span data-ttu-id="ec481-183">Zpět v aplikaci Excel aktualizujte hello kontingenční tabulky.</span><span class="sxs-lookup"><span data-stu-id="ec481-183">Back in Excel, refresh hello PivotTable.</span></span> 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    <span data-ttu-id="ec481-185">Teď to vypadá mnohem líp!</span><span class="sxs-lookup"><span data-stu-id="ec481-185">Now that looks a whole lot better!</span></span>

## <a name="see-also"></a><span data-ttu-id="ec481-186">Viz také</span><span class="sxs-lookup"><span data-stu-id="ec481-186">See Also</span></span>   
[<span data-ttu-id="ec481-187">Lekce 9: Vytvoření hierarchií</span><span class="sxs-lookup"><span data-stu-id="ec481-187">Lesson 9: Create hierarchies</span></span>](../tutorials/aas-lesson-9-create-hierarchies.md)  
[<span data-ttu-id="ec481-188">Doplňková lekce – Dynamické zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ec481-188">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="ec481-189">Doplňková lekce – Řádky podrobností</span><span class="sxs-lookup"><span data-stu-id="ec481-189">Supplemental Lesson - Detail rows</span></span>](../tutorials/aas-supplemental-lesson-detail-rows.md)  