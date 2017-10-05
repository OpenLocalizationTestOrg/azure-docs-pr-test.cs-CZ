---
title: "Kurz služby Azure Analysis Services – Lekce 2: Získání dat | Dokumentace Microsoftu"
description: "Popisuje, jak získat a importovat data v projektu Kurz služby Azure Analysis Services."
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
ms.openlocfilehash: e77de4b9a74b528fa8a7ce86424fc14628b2cacc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-2-get-data"></a><span data-ttu-id="06fb9-103">Lekce 2: Získání dat</span><span class="sxs-lookup"><span data-stu-id="06fb9-103">Lesson 2: Get data</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="06fb9-104">V této lekci se pomocí funkce Získání dat v SSDT připojíte k ukázkové databázi AdventureWorksDW2014, vyberete data, zobrazíte jejich náhled, použijete filtr a potom je naimportujete do pracovního prostoru modelu.</span><span class="sxs-lookup"><span data-stu-id="06fb9-104">In this lesson, you use Get Data in SSDT to connect to the AdventureWorksDW2014 sample database, select data, preview and filter, and then import into your model workspace.</span></span>  
  
<span data-ttu-id="06fb9-105">Pomocí funkce Získání dat můžete importovat data z celé řady zdrojů: Azure SQL Database, Oracle, Sybase, kanál OData, Teradata, soubory a další.</span><span class="sxs-lookup"><span data-stu-id="06fb9-105">By using Get Data, you can import data from a wide variety of sources: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, files and more.</span></span> <span data-ttu-id="06fb9-106">Data umožňují také dotazy pomocí výrazu se vzorci Power Query M.</span><span class="sxs-lookup"><span data-stu-id="06fb9-106">Data can also be queried using a Power Query M formula expression.</span></span>
  
<span data-ttu-id="06fb9-107">Odhadovaný čas dokončení této lekce: **10 minut**</span><span class="sxs-lookup"><span data-stu-id="06fb9-107">Estimated time to complete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="06fb9-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="06fb9-108">Prerequisites</span></span>  
<span data-ttu-id="06fb9-109">Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí.</span><span class="sxs-lookup"><span data-stu-id="06fb9-109">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="06fb9-110">Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 1: Vytvoření nového projektu s tabelárním modelem](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span><span class="sxs-lookup"><span data-stu-id="06fb9-110">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 1: Create a new tabular model project](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span></span>  
  
## <a name="create-a-connection"></a><span data-ttu-id="06fb9-111">Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="06fb9-111">Create a connection</span></span>  
  
#### <a name="to-create-a-connection-to-the-adventureworksdw2014-database"></a><span data-ttu-id="06fb9-112">Vytvoření připojení k databázi AdventureWorksDW2014</span><span class="sxs-lookup"><span data-stu-id="06fb9-112">To create a connection to the AdventureWorksDW2014 database</span></span>  
  
1.  <span data-ttu-id="06fb9-113">V Průzkumníku tabelárních modelů klikněte pravým tlačítkem na **Zdroje dat** > **Importovat ze zdroje dat**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-113">In Tabular Model Explorer, right-click **Data Sources** > **Import from Data Source**.</span></span>  
  
    <span data-ttu-id="06fb9-114">Spustí se funkce Získání dat, která vás provede připojením ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="06fb9-114">This launches Get Data, which guides you through connecting to a data source.</span></span> <span data-ttu-id="06fb9-115">Pokud se Průzkumník tabelárních modelů nezobrazuje, v **Průzkumníku řešení** dvakrát klikněte na **Model.bim** a otevřete model v návrháři.</span><span class="sxs-lookup"><span data-stu-id="06fb9-115">If you don't see Tabular Model Explorer, in **Solution Explorer**, double-click **Model.bim** to open the model in the designer.</span></span> 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  <span data-ttu-id="06fb9-117">Ve funkci Získání dat klikněte na **Databáze** > **Databáze SQL Serveru** > **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-117">In Get Data, click **Database** > **SQL Server Database** > **Connect**.</span></span>  
  
3.  <span data-ttu-id="06fb9-118">V dialogovém okně **Databáze SQL Serveru** v části **Server** zadejte název serveru, na který jste nainstalovali databázi AdventureWorksDW2014, a klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-118">In the **SQL Server Database** dialog, in **Server**, type the name of the server where you installed the AdventureWorksDW2014 database, and then click **Connect**.</span></span>  

4.  <span data-ttu-id="06fb9-119">Po zobrazení výzvy k zadání přihlašovacích údajů je potřeba zadat přihlašovací údaje, pomocí kterých se služba Analysis Services připojuje ke zdroji dat při importu a zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="06fb9-119">When prompted to enter credentials, you need to specify the credentials Analysis Services uses to connect to the data source when importing and processing data.</span></span> <span data-ttu-id="06fb9-120">V části **Režim zosobnění** vyberte **Zosobnit účet**, zadejte přihlašovací údaje a klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-120">In **Impersonation Mode**, select **Impersonate Account**, then enter credentials, and then click **Connect**.</span></span> <span data-ttu-id="06fb9-121">Doporučujeme použít účet, u kterého nedochází k vypršení platnosti hesla.</span><span class="sxs-lookup"><span data-stu-id="06fb9-121">It's recommended you use an account where the password doesn't expire.</span></span>

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > <span data-ttu-id="06fb9-123">Nejbezpečnější metodu připojení ke zdroji dat poskytuje použití uživatelského účtu a hesla systému Windows.</span><span class="sxs-lookup"><span data-stu-id="06fb9-123">Using a Windows user account and password provides the most secure method of connecting to a data source.</span></span>
  
5.  <span data-ttu-id="06fb9-124">V části Navigátor vyberte databázi **AdventureWorksDW2014** a klikněte na **OK**. Tím se vytvoří připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="06fb9-124">In Navigator, select the **AdventureWorksDW2014** database, and then click **OK**.This creates the connection to the database.</span></span> 
  
6.  <span data-ttu-id="06fb9-125">V části Navigátor zaškrtněte políčka u následujících tabulek: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** a **FactInternetSales**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-125">In Navigator, select the check box for the following tables: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, and **FactInternetSales**.</span></span>  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
<span data-ttu-id="06fb9-127">Po kliknutí na OK se otevře Editor dotazů.</span><span class="sxs-lookup"><span data-stu-id="06fb9-127">After you click OK, Query Editor opens.</span></span> <span data-ttu-id="06fb9-128">V další části vyberete jenom data, která chcete importovat.</span><span class="sxs-lookup"><span data-stu-id="06fb9-128">In the next section, you select only the data you want to import.</span></span>

  
## <a name="filter-the-table-data"></a><span data-ttu-id="06fb9-129">Filtrování tabulkových dat</span><span class="sxs-lookup"><span data-stu-id="06fb9-129">Filter the table data</span></span>  
<span data-ttu-id="06fb9-130">Tabulky v ukázkové databázi AdventureWorksDW2014 obsahují data, která není nutné zahrnout do modelu.</span><span class="sxs-lookup"><span data-stu-id="06fb9-130">Tables in the AdventureWorksDW2014 sample database have data that isn't necessary to include in your model.</span></span> <span data-ttu-id="06fb9-131">Pokud je to možné, nepotřebná data byste měli vyfiltrovat, abyste ušetřili místo v paměti využívané modelem.</span><span class="sxs-lookup"><span data-stu-id="06fb9-131">When possible, you want to filter out unnecessary data to save in-memory space used by the model.</span></span> <span data-ttu-id="06fb9-132">Vyfiltrujete z tabulek některé ze sloupců, aby se neimportovaly do databáze pracovního prostoru nebo databáze modelu, až bude nasazena.</span><span class="sxs-lookup"><span data-stu-id="06fb9-132">You filter out some of the columns from tables so they're not imported into the workspace database, or the model database after it has been deployed.</span></span> 
  
#### <a name="to-filter-the-table-data-before-importing"></a><span data-ttu-id="06fb9-133">Filtrování tabulkových dat před importem</span><span class="sxs-lookup"><span data-stu-id="06fb9-133">To filter the table data before importing</span></span>  
  
1.  <span data-ttu-id="06fb9-134">V Editoru dotazů vyberte tabulku **DimCustomer**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-134">In Query Editor, select the **DimCustomer** table.</span></span> <span data-ttu-id="06fb9-135">Otevře se zobrazení tabulky DimCustomer ve zdroji dat (vaše ukázková databáze AdventureWorksDWQ2014).</span><span class="sxs-lookup"><span data-stu-id="06fb9-135">A view of the DimCustomer table at the datasource (your AdventureWorksDWQ2014 sample database) appears.</span></span> 
  
2.  <span data-ttu-id="06fb9-136">Vyberte (Ctrl + kliknutí) sloupce **SpanishEducation**, **FrenchEducation**, **SpanishOccupation** a **FrenchOccupation**, klikněte pravým tlačítkem a potom klikněte na **Odebrat sloupce**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-136">Multi-select (Ctrl + click) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**, then right-click, and then click **Remove Columns**.</span></span> 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    <span data-ttu-id="06fb9-138">Vzhledem k tomu, že tyto sloupce nejsou pro analýzu prodejů přes internet relevantní, není nutné je importovat.</span><span class="sxs-lookup"><span data-stu-id="06fb9-138">Since the values for these columns are not relevant to Internet sales analysis, there is no need to import these columns.</span></span> <span data-ttu-id="06fb9-139">Díky odstranění nepotřebných sloupců bude váš model menší a efektivnější.</span><span class="sxs-lookup"><span data-stu-id="06fb9-139">Eliminating unnecessary columns makes your model smaller and more efficient.</span></span>  
  
4.  <span data-ttu-id="06fb9-140">Filtrujte zbývající tabulky odebráním následujících sloupců v každé z nich:</span><span class="sxs-lookup"><span data-stu-id="06fb9-140">Filter the remaining tables by removing the following columns in each table:</span></span>  
    
    <span data-ttu-id="06fb9-141">**DimDate**</span><span class="sxs-lookup"><span data-stu-id="06fb9-141">**DimDate**</span></span>
    
      |<span data-ttu-id="06fb9-142">Sloupec</span><span class="sxs-lookup"><span data-stu-id="06fb9-142">Column</span></span>|  
      |--------|  
      |<span data-ttu-id="06fb9-143">DateKey</span><span class="sxs-lookup"><span data-stu-id="06fb9-143">DateKey</span></span>|  
      |<span data-ttu-id="06fb9-144">**SpanishDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="06fb9-144">**SpanishDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="06fb9-145">**FrenchDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="06fb9-145">**FrenchDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="06fb9-146">**SpanishMonthName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-146">**SpanishMonthName**</span></span>|  
      |<span data-ttu-id="06fb9-147">**FrenchMonthName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-147">**FrenchMonthName**</span></span>|  
  
    <span data-ttu-id="06fb9-148">**DimGeography**</span><span class="sxs-lookup"><span data-stu-id="06fb9-148">**DimGeography**</span></span>
  
      |<span data-ttu-id="06fb9-149">Sloupec</span><span class="sxs-lookup"><span data-stu-id="06fb9-149">Column</span></span>|  
      |-------------|  
      |<span data-ttu-id="06fb9-150">**SpanishCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-150">**SpanishCountryRegionName**</span></span>|  
      |<span data-ttu-id="06fb9-151">**FrenchCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-151">**FrenchCountryRegionName**</span></span>|  
      |<span data-ttu-id="06fb9-152">**IpAddressLocator**</span><span class="sxs-lookup"><span data-stu-id="06fb9-152">**IpAddressLocator**</span></span>|  
  
    <span data-ttu-id="06fb9-153">**DimProduct**</span><span class="sxs-lookup"><span data-stu-id="06fb9-153">**DimProduct**</span></span>
  
      |<span data-ttu-id="06fb9-154">Sloupec</span><span class="sxs-lookup"><span data-stu-id="06fb9-154">Column</span></span>|  
      |-----------|  
      |<span data-ttu-id="06fb9-155">**SpanishProductName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-155">**SpanishProductName**</span></span>|  
      |<span data-ttu-id="06fb9-156">**FrenchProductName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-156">**FrenchProductName**</span></span>|  
      |<span data-ttu-id="06fb9-157">**FrenchDescription**</span><span class="sxs-lookup"><span data-stu-id="06fb9-157">**FrenchDescription**</span></span>|  
      |<span data-ttu-id="06fb9-158">**ChineseDescription**</span><span class="sxs-lookup"><span data-stu-id="06fb9-158">**ChineseDescription**</span></span>|  
      |<span data-ttu-id="06fb9-159">**ArabicDescription**</span><span class="sxs-lookup"><span data-stu-id="06fb9-159">**ArabicDescription**</span></span>|  
      |<span data-ttu-id="06fb9-160">**HebrewDescription**</span><span class="sxs-lookup"><span data-stu-id="06fb9-160">**HebrewDescription**</span></span>|  
      |<span data-ttu-id="06fb9-161">**ThaiDescription**</span><span class="sxs-lookup"><span data-stu-id="06fb9-161">**ThaiDescription**</span></span>|  
      |<span data-ttu-id="06fb9-162">**GermanDescription**</span><span class="sxs-lookup"><span data-stu-id="06fb9-162">**GermanDescription**</span></span>|  
      |<span data-ttu-id="06fb9-163">**JapaneseDescription**</span><span class="sxs-lookup"><span data-stu-id="06fb9-163">**JapaneseDescription**</span></span>|  
      |<span data-ttu-id="06fb9-164">**TurkishDescription**</span><span class="sxs-lookup"><span data-stu-id="06fb9-164">**TurkishDescription**</span></span>|  
  
    <span data-ttu-id="06fb9-165">**DimProductCategory**</span><span class="sxs-lookup"><span data-stu-id="06fb9-165">**DimProductCategory**</span></span>
  
      |<span data-ttu-id="06fb9-166">Sloupec</span><span class="sxs-lookup"><span data-stu-id="06fb9-166">Column</span></span>|  
      |--------------------|  
      |<span data-ttu-id="06fb9-167">**SpanishProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-167">**SpanishProductCategoryName**</span></span>|  
      |<span data-ttu-id="06fb9-168">**FrenchProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-168">**FrenchProductCategoryName**</span></span>|  
  
    <span data-ttu-id="06fb9-169">**DimProductSubcategory**</span><span class="sxs-lookup"><span data-stu-id="06fb9-169">**DimProductSubcategory**</span></span>
  
      |<span data-ttu-id="06fb9-170">Sloupec</span><span class="sxs-lookup"><span data-stu-id="06fb9-170">Column</span></span>|  
      |-----------------------|  
      |<span data-ttu-id="06fb9-171">**SpanishProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-171">**SpanishProductSubcategoryName**</span></span>|  
      |<span data-ttu-id="06fb9-172">**FrenchProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="06fb9-172">**FrenchProductSubcategoryName**</span></span>|  
  
    <span data-ttu-id="06fb9-173">**FactInternetSales**</span><span class="sxs-lookup"><span data-stu-id="06fb9-173">**FactInternetSales**</span></span>
  
      |<span data-ttu-id="06fb9-174">Sloupec</span><span class="sxs-lookup"><span data-stu-id="06fb9-174">Column</span></span>|  
      |------------------|  
      |<span data-ttu-id="06fb9-175">**OrderDateKey**</span><span class="sxs-lookup"><span data-stu-id="06fb9-175">**OrderDateKey**</span></span>|  
      |<span data-ttu-id="06fb9-176">**DueDateKey**</span><span class="sxs-lookup"><span data-stu-id="06fb9-176">**DueDateKey**</span></span>|  
      |<span data-ttu-id="06fb9-177">**ShipDateKey**</span><span class="sxs-lookup"><span data-stu-id="06fb9-177">**ShipDateKey**</span></span>|   
  
## <span data-ttu-id="06fb9-178"><a name="Import"></a>Import vybraných tabulek a dat sloupců</span><span class="sxs-lookup"><span data-stu-id="06fb9-178"><a name="Import"></a>Import the selected tables and column data</span></span>  
<span data-ttu-id="06fb9-179">Teď, když jste zobrazili náhled a vyfiltrovali nepotřebná data, můžete importovat zbývající požadovaná data.</span><span class="sxs-lookup"><span data-stu-id="06fb9-179">Now that you've previewed and filtered out unnecessary data, you can import the rest of the data you do want.</span></span> <span data-ttu-id="06fb9-180">Průvodce importuje kromě tabulkových dat také případné relace mezi tabulkami.</span><span class="sxs-lookup"><span data-stu-id="06fb9-180">The wizard imports the table data along with any relationships between tables.</span></span> <span data-ttu-id="06fb9-181">V modelu se vytvoří nové tabulky a sloupce a data, která jste vyfiltrovali, se neimportují.</span><span class="sxs-lookup"><span data-stu-id="06fb9-181">New tables and columns are created in the model and data that you filtered out is not be imported.</span></span>  
  
#### <a name="to-import-the-selected-tables-and-column-data"></a><span data-ttu-id="06fb9-182">Import vybraných tabulek a dat sloupců</span><span class="sxs-lookup"><span data-stu-id="06fb9-182">To import the selected tables and column data</span></span>  
  
1.  <span data-ttu-id="06fb9-183">Zkontrolujte váš výběr.</span><span class="sxs-lookup"><span data-stu-id="06fb9-183">Review your selections.</span></span> <span data-ttu-id="06fb9-184">Pokud vše vypadá v pořádku, klikněte na **Importovat**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-184">If everything looks okay, click **Import**.</span></span> <span data-ttu-id="06fb9-185">V dialogovém okně Zpracování dat se zobrazí stav importování dat ze zdroje dat do databáze pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="06fb9-185">The Data Processing dialog shows the status of data being imported from your datasource into your workspace database.</span></span>
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  <span data-ttu-id="06fb9-187">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-187">Click **Close**.</span></span>  

  
## <a name="save-your-model-project"></a><span data-ttu-id="06fb9-188">Uložení projektu s modelem</span><span class="sxs-lookup"><span data-stu-id="06fb9-188">Save your model project</span></span>  
<span data-ttu-id="06fb9-189">Je důležité projekt s modelem často ukládat.</span><span class="sxs-lookup"><span data-stu-id="06fb9-189">It's important to frequently save your model project.</span></span>  
  
#### <a name="to-save-the-model-project"></a><span data-ttu-id="06fb9-190">Uložení projektu s modelem</span><span class="sxs-lookup"><span data-stu-id="06fb9-190">To save the model project</span></span>  
  
-   <span data-ttu-id="06fb9-191">Klikněte na **Soubor** > **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="06fb9-191">Click **File** > **Save All**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="06fb9-192">Co dále?</span><span class="sxs-lookup"><span data-stu-id="06fb9-192">What's next?</span></span>
<span data-ttu-id="06fb9-193">[Lekce 3: Označení jako tabulky kalendářních dat](../tutorials/aas-lesson-3-mark-as-date-table.md)</span><span class="sxs-lookup"><span data-stu-id="06fb9-193">[Lesson 3: Mark as Date Table](../tutorials/aas-lesson-3-mark-as-date-table.md).</span></span>

  
  
