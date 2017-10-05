---
title: "Vizualizace dat SQL Data Warehouse pomocí Power BI – Microsoft Azure"
description: "Vizualizace dat SQL Data Warehouse pomocí Power BI"
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a41393730143b14e91318a61858d989fff3786c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-data-with-power-bi"></a><span data-ttu-id="5e972-103">Vizualizace dat pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="5e972-103">Visualize data with Power BI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e972-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="5e972-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="5e972-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5e972-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="5e972-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e972-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="5e972-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="5e972-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="5e972-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="5e972-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="5e972-109">V tomto kurzu si ukážeme, jak se pomocí Power BI připojit k SQL Data Warehouse a vytvořit pár základních vizualizací.</span><span class="sxs-lookup"><span data-stu-id="5e972-109">This tutorial shows you how to use Power BI to connect to SQL Data Warehouse and create a few basic visualizations.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="5e972-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5e972-110">Prerequisites</span></span>
<span data-ttu-id="5e972-111">Pro jednotlivé kroky v tomto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="5e972-111">To step through this tutorial, you need:</span></span>

* <span data-ttu-id="5e972-112">SQL Data Warehouse s předem načtenou databází AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="5e972-112">A SQL Data Warehouse pre-loaded with the AdventureWorksDW database.</span></span> <span data-ttu-id="5e972-113">Ke zřízení této konfigurace použijte článek [Vytvoření SQL Data Warehouse][Create a SQL Data Warehouse] a zvolte načtení ukázkových dat.</span><span class="sxs-lookup"><span data-stu-id="5e972-113">To provision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose to load the sample data.</span></span> <span data-ttu-id="5e972-114">Pokud už Data Warehouse máte, ale nemáte ukázková data, můžete [ukázková data načíst ručně][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="5e972-114">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-connect-to-your-database"></a><span data-ttu-id="5e972-115">1. Připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="5e972-115">1. Connect to your database</span></span>
<span data-ttu-id="5e972-116">Pokud chcete otevřít Power BI a připojit se ke své databázi AdventureWorksDW, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5e972-116">To open Power BI and connect to your AdventureWorksDW database:</span></span>

1. <span data-ttu-id="5e972-117">Přihlaste se k webu [Azure Portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="5e972-117">Sign into the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="5e972-118">Klikněte na **Databáze SQL** a zvolte databázi AdventureWorks služby SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5e972-118">Click **SQL databases** and choose your AdventureWorks SQL Data Warehouse database.</span></span>
   
    ![Vyhledání databáze][1]
3. <span data-ttu-id="5e972-120">Klikněte na tlačítko Otevřít v Power BI.</span><span class="sxs-lookup"><span data-stu-id="5e972-120">Click the 'Open in Power BI' button.</span></span>
   
    ![Tlačítko Power BI][2]
4. <span data-ttu-id="5e972-122">Teď by se vám měla zobrazit stránka pro připojení k SQL Data Warehouse s webovou adresou vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="5e972-122">You should now see the SQL Data Warehouse connection page displaying your database web address.</span></span> <span data-ttu-id="5e972-123">Klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="5e972-123">Click next.</span></span>
   
    ![Připojení Power BI][3]
5. <span data-ttu-id="5e972-125">Zadejte své uživatelské jméno a heslo pro SQL Server Azure a budete plně připojení k vaší databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5e972-125">Enter your Azure SQL server username and password and you will be fully connected to your SQL Data Warehouse database.</span></span>
   
    ![Přihlášení k Power BI][4]
6. <span data-ttu-id="5e972-127">Až se zaregistrujete v Power BI, klikněte v levém okně na datovou sadu AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="5e972-127">Once you have signed into Power BI, click the AdventureWorksDW dataset on the left blade.</span></span> <span data-ttu-id="5e972-128">Tím se otevře databáze.</span><span class="sxs-lookup"><span data-stu-id="5e972-128">This will open the database.</span></span>
   
    ![Otevření databáze AdventureWorksDW v Power BI][5]

## <a name="2-create-a-report"></a><span data-ttu-id="5e972-130">2. Vytvoření sestavy</span><span class="sxs-lookup"><span data-stu-id="5e972-130">2. Create a report</span></span>
<span data-ttu-id="5e972-131">Teď můžete pomocí Power BI analyzovat ukázková data databáze AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="5e972-131">You are now ready to use Power BI to analyze your AdventureWorksDW sample data.</span></span> <span data-ttu-id="5e972-132">K provedení analýzy má databáze AdventureWorksDW zobrazení s názvem AggregateSales.</span><span class="sxs-lookup"><span data-stu-id="5e972-132">To perform the analysis, AdventureWorksDW has a view called AggregateSales.</span></span> <span data-ttu-id="5e972-133">Toto zobrazení obsahuje několik klíčových metrik pro analýzu prodeje společnosti.</span><span class="sxs-lookup"><span data-stu-id="5e972-133">This view contains a few of the key metrics for analyzing the sales of the company.</span></span>

1. <span data-ttu-id="5e972-134">Pokud budete chtít vytvořit mapu částek prodeje podle PSČ, klikněte v podokně polí napravo na zobrazení AggregateSales a tím ho rozbalte.</span><span class="sxs-lookup"><span data-stu-id="5e972-134">To create a map of sales amount according to postal code, in the right-hand fields pane, click the AggregateSales view to expand it.</span></span> <span data-ttu-id="5e972-135">Kliknutím vyberte sloupce PostalCode a SalesAmount.</span><span class="sxs-lookup"><span data-stu-id="5e972-135">Click the PostalCode and SalesAmount columns to select them.</span></span>
   
    ![Výběr sloupce AggregateSales v Power BI][6]
   
    <span data-ttu-id="5e972-137">Power BI automaticky rozpozná, že se jedná o zeměpisné údaje, a umístí vám je na mapu.</span><span class="sxs-lookup"><span data-stu-id="5e972-137">Power BI automatically recognizes this is geographic data and put it in a map for you.</span></span>
   
    ![Mapa Power BI][7]
2. <span data-ttu-id="5e972-139">Tento krok vytvoří pruhový graf, který zobrazí částku prodeje na příjem zákazníka.</span><span class="sxs-lookup"><span data-stu-id="5e972-139">This step creates a bar graph that shows amount of sales per customer income.</span></span> <span data-ttu-id="5e972-140">Toto můžete vytvořit v rozbaleném zobrazení AggregateSales.</span><span class="sxs-lookup"><span data-stu-id="5e972-140">To create this go to the expanded AggregateSales view.</span></span> <span data-ttu-id="5e972-141">Klikněte na pole SalesAmount.</span><span class="sxs-lookup"><span data-stu-id="5e972-141">Click the SalesAmount field.</span></span> <span data-ttu-id="5e972-142">Přetáhněte pole Customer Income (Příjem zákazníka) nalevo do části Axis (Osa).</span><span class="sxs-lookup"><span data-stu-id="5e972-142">Drag the Customer Income field to the left and drop it into Axis.</span></span>
   
    ![Výběr osy v Power BI][8]
   
    <span data-ttu-id="5e972-144">Pruhový graf jsme přesunuli vlevo.</span><span class="sxs-lookup"><span data-stu-id="5e972-144">We moved the bar chart over the left.</span></span>
   
    ![Pruhový graf v Power BI][9]
3. <span data-ttu-id="5e972-146">Tento krok vytvoří spojnicový graf zobrazující prodejní částku k datu objednávky.</span><span class="sxs-lookup"><span data-stu-id="5e972-146">This step creates a line chart that shows sales amount per order date.</span></span> <span data-ttu-id="5e972-147">Toto můžete vytvořit v rozbaleném zobrazení AggregateSales.</span><span class="sxs-lookup"><span data-stu-id="5e972-147">To create this go to the expanded AggregateSales view.</span></span> <span data-ttu-id="5e972-148">Klikněte na SalesAmount a OrderDate.</span><span class="sxs-lookup"><span data-stu-id="5e972-148">Click SalesAmount and OrderDate.</span></span> <span data-ttu-id="5e972-149">Ve sloupci Visualizations (Vizualizace) klikněte na ikonu spojnicového grafu (je to první ikona na druhém řádku pod vizualizacemi).</span><span class="sxs-lookup"><span data-stu-id="5e972-149">In the Visualizations column click the Line Chart icon; this is the first icon in the second line under visualizations.</span></span>
   
    ![Výběr spojnicového grafu v Power BI][10]
   
    <span data-ttu-id="5e972-151">Teď máte sestavu zobrazující tři různé vizualizace dat.</span><span class="sxs-lookup"><span data-stu-id="5e972-151">You now have a report that shows three different visualizations of the data.</span></span>
   
    ![Spojnicový graf v Power BI][11]

<span data-ttu-id="5e972-153">Kdykoli můžete rozdělanou práci uložit tak, že kliknete na **Soubor** a vyberete **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5e972-153">You can save your progress at any time by clicking **File** and selecting **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e972-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e972-154">Next steps</span></span>
<span data-ttu-id="5e972-155">Vyzkoušeli jste si tedy práci s ukázkovými daty a teď se podívejte, jak na [vývoj][develop], [načítání][load] nebo [migraci][migrate].</span><span class="sxs-lookup"><span data-stu-id="5e972-155">Now that we've given you some time to warm up with the sample data, see how to [develop][develop], [load][load], or [migrate][migrate].</span></span> <span data-ttu-id="5e972-156">Nebo se podívejte na [web Power BI][Power BI website].</span><span class="sxs-lookup"><span data-stu-id="5e972-156">Or take a look at the [Power BI website][Power BI website].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
