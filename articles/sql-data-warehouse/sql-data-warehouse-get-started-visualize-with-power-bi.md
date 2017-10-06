---
title: "aaaVisualize dat SQL Data Warehouse pomocí Power BI – Microsoft Azure"
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
ms.openlocfilehash: 0425cf5abe7bc001b2a41df4d09bf5f2e42527e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-data-with-power-bi"></a><span data-ttu-id="3c5c1-103">Vizualizace dat pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="3c5c1-103">Visualize data with Power BI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c5c1-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="3c5c1-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="3c5c1-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3c5c1-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="3c5c1-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c5c1-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="3c5c1-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="3c5c1-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="3c5c1-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="3c5c1-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="3c5c1-109">Tento kurz ukazuje, jak toouse Power BI tooconnect tooSQL datového skladu a vytvořit pár základních vizualizací.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-109">This tutorial shows you how toouse Power BI tooconnect tooSQL Data Warehouse and create a few basic visualizations.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="3c5c1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c5c1-110">Prerequisites</span></span>
<span data-ttu-id="3c5c1-111">toostep prostřednictvím tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3c5c1-111">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="3c5c1-112">SQL Data Warehouse předem načtená hello databáze AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-112">A SQL Data Warehouse pre-loaded with hello AdventureWorksDW database.</span></span> <span data-ttu-id="3c5c1-113">tooprovision, najdete v tématu [vytvořit SQL Data Warehouse] [ Create a SQL Data Warehouse] a zvolte tooload hello ukázková data.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-113">tooprovision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose tooload hello sample data.</span></span> <span data-ttu-id="3c5c1-114">Pokud už Data Warehouse máte, ale nemáte ukázková data, můžete [ukázková data načíst ručně][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="3c5c1-114">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-connect-tooyour-database"></a><span data-ttu-id="3c5c1-115">1. Připojit databáze tooyour</span><span class="sxs-lookup"><span data-stu-id="3c5c1-115">1. Connect tooyour database</span></span>
<span data-ttu-id="3c5c1-116">tooopen Power BI a připojte databázi AdventureWorksDW tooyour:</span><span class="sxs-lookup"><span data-stu-id="3c5c1-116">tooopen Power BI and connect tooyour AdventureWorksDW database:</span></span>

1. <span data-ttu-id="3c5c1-117">Přihlaste se k hello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="3c5c1-117">Sign into hello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="3c5c1-118">Klikněte na **Databáze SQL** a zvolte databázi AdventureWorks služby SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-118">Click **SQL databases** and choose your AdventureWorks SQL Data Warehouse database.</span></span>
   
    ![Vyhledání databáze][1]
3. <span data-ttu-id="3c5c1-120">Klikněte na tlačítko Otevřít v Power BI hello.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-120">Click hello 'Open in Power BI' button.</span></span>
   
    ![Tlačítko Power BI][2]
4. <span data-ttu-id="3c5c1-122">Teď byste měli vidět hello SQL Data Warehouse připojení stránky zobrazení webovou adresu vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-122">You should now see hello SQL Data Warehouse connection page displaying your database web address.</span></span> <span data-ttu-id="3c5c1-123">Klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-123">Click next.</span></span>
   
    ![Připojení Power BI][3]
5. <span data-ttu-id="3c5c1-125">Zadejte uživatelské jméno pro server Azure SQL a heslo a budete plně připojené tooyour databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-125">Enter your Azure SQL server username and password and you will be fully connected tooyour SQL Data Warehouse database.</span></span>
   
    ![Přihlášení k Power BI][4]
6. <span data-ttu-id="3c5c1-127">Až se zaregistrujete v Power BI, klikněte na datovou sadu AdventureWorksDW hello v levém okně hello.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-127">Once you have signed into Power BI, click hello AdventureWorksDW dataset on hello left blade.</span></span> <span data-ttu-id="3c5c1-128">Tím se otevře databáze hello.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-128">This will open hello database.</span></span>
   
    ![Otevření databáze AdventureWorksDW v Power BI][5]

## <a name="2-create-a-report"></a><span data-ttu-id="3c5c1-130">2. Vytvoření sestavy</span><span class="sxs-lookup"><span data-stu-id="3c5c1-130">2. Create a report</span></span>
<span data-ttu-id="3c5c1-131">Můžete je nyní připraven toouse Power BI tooanalyze ukázkových dat AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-131">You are now ready toouse Power BI tooanalyze your AdventureWorksDW sample data.</span></span> <span data-ttu-id="3c5c1-132">tooperform hello analýzy má databáze AdventureWorksDW zobrazení s názvem AggregateSales.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-132">tooperform hello analysis, AdventureWorksDW has a view called AggregateSales.</span></span> <span data-ttu-id="3c5c1-133">Toto zobrazení obsahuje několik hello klíčových metrik pro analýzu prodeje společnosti hello hello.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-133">This view contains a few of hello key metrics for analyzing hello sales of hello company.</span></span>

1. <span data-ttu-id="3c5c1-134">toocreate mapu částek prodeje podle kódu toopostal, v podokně polí hello, klikněte na tlačítko hello tooexpand zobrazení AggregateSales ho.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-134">toocreate a map of sales amount according toopostal code, in hello right-hand fields pane, click hello AggregateSales view tooexpand it.</span></span> <span data-ttu-id="3c5c1-135">Klikněte na tlačítko tooselect sloupce PostalCode a SalesAmount hello je.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-135">Click hello PostalCode and SalesAmount columns tooselect them.</span></span>
   
    ![Výběr sloupce AggregateSales v Power BI][6]
   
    <span data-ttu-id="3c5c1-137">Power BI automaticky rozpozná, že se jedná o zeměpisné údaje, a umístí vám je na mapu.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-137">Power BI automatically recognizes this is geographic data and put it in a map for you.</span></span>
   
    ![Mapa Power BI][7]
2. <span data-ttu-id="3c5c1-139">Tento krok vytvoří pruhový graf, který zobrazí částku prodeje na příjem zákazníka.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-139">This step creates a bar graph that shows amount of sales per customer income.</span></span> <span data-ttu-id="3c5c1-140">toocreate tento přejděte toohello rozšířené zobrazení AggregateSales.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-140">toocreate this go toohello expanded AggregateSales view.</span></span> <span data-ttu-id="3c5c1-141">Klikněte na pole SalesAmount hello.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-141">Click hello SalesAmount field.</span></span> <span data-ttu-id="3c5c1-142">Přetáhněte hello příjem zákazníka pole toohello doleva a umístěte jej do osy.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-142">Drag hello Customer Income field toohello left and drop it into Axis.</span></span>
   
    ![Výběr osy v Power BI][8]
   
    <span data-ttu-id="3c5c1-144">Jsme hello pruhový graf nepřesouvají hello doleva.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-144">We moved hello bar chart over hello left.</span></span>
   
    ![Pruhový graf v Power BI][9]
3. <span data-ttu-id="3c5c1-146">Tento krok vytvoří spojnicový graf zobrazující prodejní částku k datu objednávky.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-146">This step creates a line chart that shows sales amount per order date.</span></span> <span data-ttu-id="3c5c1-147">toocreate tento přejděte toohello rozšířené zobrazení AggregateSales.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-147">toocreate this go toohello expanded AggregateSales view.</span></span> <span data-ttu-id="3c5c1-148">Klikněte na SalesAmount a OrderDate.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-148">Click SalesAmount and OrderDate.</span></span> <span data-ttu-id="3c5c1-149">Ve sloupci vizualizace hello klikněte na ikonu spojnicového grafu hello; Toto je první ikona hello hello druhém řádku pod vizualizacemi.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-149">In hello Visualizations column click hello Line Chart icon; this is hello first icon in hello second line under visualizations.</span></span>
   
    ![Výběr spojnicového grafu v Power BI][10]
   
    <span data-ttu-id="3c5c1-151">Teď máte sestavu zobrazující tři různé vizualizace dat hello.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-151">You now have a report that shows three different visualizations of hello data.</span></span>
   
    ![Spojnicový graf v Power BI][11]

<span data-ttu-id="3c5c1-153">Kdykoli můžete rozdělanou práci uložit tak, že kliknete na **Soubor** a vyberete **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3c5c1-153">You can save your progress at any time by clicking **File** and selecting **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c5c1-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c5c1-154">Next steps</span></span>
<span data-ttu-id="3c5c1-155">Teď, když vyzkoušeli jste některé čas toowarm hello ukázková data, najdete v části Jak příliš[vyvíjet][develop], [načíst][load], nebo [ migrace][migrate].</span><span class="sxs-lookup"><span data-stu-id="3c5c1-155">Now that we've given you some time toowarm up with hello sample data, see how too[develop][develop], [load][load], or [migrate][migrate].</span></span> <span data-ttu-id="3c5c1-156">Nebo se podívejte na hello [web Power BI][Power BI website].</span><span class="sxs-lookup"><span data-stu-id="3c5c1-156">Or take a look at hello [Power BI website][Power BI website].</span></span>

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
[connecting tooSQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
