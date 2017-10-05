---
title: "Načtení dat z SQL serveru do Azure SQL Data Warehouse (SSIS) | Microsoft Docs"
description: "Ukazuje, jak vytvořit balíček integrační služby SSIS (SQL Server) pro přesun dat z široké škály zdrojů dat do SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e2c252e9-0828-47c2-a808-e3bea46c134a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/30/2017
ms.author: cakarst;douglasl;barbkess
ms.openlocfilehash: 6c9cebdd715b6997d0633bc725a3945ba9e0c357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a><span data-ttu-id="75914-103">Načtení dat z SQL serveru do Azure SQL Data Warehouse služby SSIS)</span><span class="sxs-lookup"><span data-stu-id="75914-103">Load data from SQL Server into Azure SQL Data Warehouse (SSIS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="75914-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="75914-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="75914-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="75914-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="75914-106">bcp</span><span class="sxs-lookup"><span data-stu-id="75914-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="75914-107">Vytvoření balíčku integrační služby SSIS (SQL Server) k načtení dat z SQL serveru do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-107">Create a SQL Server Integration Services (SSIS) package to load data from SQL Server into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="75914-108">Volitelně můžete změny struktury, transformace a čistí data při průchodu SSIS datového toku.</span><span class="sxs-lookup"><span data-stu-id="75914-108">You can optionally restructure, transform, and cleanse the data as it passes through the SSIS data flow.</span></span>

<span data-ttu-id="75914-109">V tomto kurzu provedete následující:</span><span class="sxs-lookup"><span data-stu-id="75914-109">In this tutorial, you will:</span></span>

* <span data-ttu-id="75914-110">V sadě Visual Studio vytvořte nový projekt integrační služby.</span><span class="sxs-lookup"><span data-stu-id="75914-110">Create a new Integration Services project in Visual Studio.</span></span>
* <span data-ttu-id="75914-111">Připojení ke zdrojům dat, včetně systému SQL Server (jako zdroj) a SQL Data Warehouse (jako cíl).</span><span class="sxs-lookup"><span data-stu-id="75914-111">Connect to data sources, including SQL Server (as a source) and SQL Data Warehouse (as a destination).</span></span>
* <span data-ttu-id="75914-112">Návrh SSIS balíčku, který načte data ze zdrojové do cílové.</span><span class="sxs-lookup"><span data-stu-id="75914-112">Design an SSIS package that loads data from the source into the destination.</span></span>
* <span data-ttu-id="75914-113">Spusťte služby SSIS balíček, který chcete načíst data.</span><span class="sxs-lookup"><span data-stu-id="75914-113">Run the SSIS package to load the data.</span></span>

<span data-ttu-id="75914-114">Tento kurz používá jako zdroj dat systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="75914-114">This tutorial uses SQL Server as the data source.</span></span> <span data-ttu-id="75914-115">SQL Server může být spuštěn místně nebo v virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="75914-115">SQL Server could be running on premises or in an Azure virtual machine.</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="75914-116">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="75914-116">Basic concepts</span></span>
<span data-ttu-id="75914-117">Balíček je jednotka práce v SSIS.</span><span class="sxs-lookup"><span data-stu-id="75914-117">The package is the unit of work in SSIS.</span></span> <span data-ttu-id="75914-118">Související balíčky jsou seskupené v projektech.</span><span class="sxs-lookup"><span data-stu-id="75914-118">Related packages are grouped in projects.</span></span> <span data-ttu-id="75914-119">Vytváření projektů a balíčků návrhu v sadě Visual Studio s SQL Server Data Tools.</span><span class="sxs-lookup"><span data-stu-id="75914-119">You create projects and design packages in Visual Studio with SQL Server Data Tools.</span></span> <span data-ttu-id="75914-120">Proces návrhu je visual proces, ve kterém můžete přetahování součásti z panelu nástrojů na návrhovou plochu, připojte je a nastavte jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="75914-120">The design process is a visual process in which you drag and drop components from the Toolbox to the design surface, connect them, and set their properties.</span></span> <span data-ttu-id="75914-121">Po dokončení vašeho balíčku, můžete volitelně nasadit ho do systému SQL Server pro komplexní správu, monitorování a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="75914-121">After you finish your package, you can optionally deploy it to SQL Server for comprehensive management, monitoring, and security.</span></span>

## <a name="options-for-loading-data-with-ssis"></a><span data-ttu-id="75914-122">Možnosti pro načítání dat pomocí služby SSIS</span><span class="sxs-lookup"><span data-stu-id="75914-122">Options for loading data with SSIS</span></span>
<span data-ttu-id="75914-123">Integrace služby SSIS (SQL Server) je flexibilní sadu nástrojů, který poskytuje celou řadu možností pro připojení k a načítání dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-123">SQL Server Integration Services (SSIS) is a flexible set of tools that provides a variety of options for connecting to, and loading data into, SQL Data Warehouse.</span></span>

1. <span data-ttu-id="75914-124">Použijte určení NET ADO pro připojení k SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-124">Use an ADO NET Destination to connect to SQL Data Warehouse.</span></span> <span data-ttu-id="75914-125">Tento kurz používá určení NET ADO, protože má nejmenší počet možností konfigurace.</span><span class="sxs-lookup"><span data-stu-id="75914-125">This tutorial uses an ADO NET Destination because it has the fewest configuration options.</span></span>
2. <span data-ttu-id="75914-126">Použijte cílové OLE DB pro připojení k SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-126">Use an OLE DB Destination to connect to SQL Data Warehouse.</span></span> <span data-ttu-id="75914-127">Tato možnost může poskytovat mírně lepší výkon než cílový NET ADO.</span><span class="sxs-lookup"><span data-stu-id="75914-127">This option may provide slightly better performance than the ADO NET Destination.</span></span>
3. <span data-ttu-id="75914-128">Příprava data v Azure Blob Storage pomocí úlohy nahrát objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="75914-128">Use the Azure Blob Upload Task to stage the data in Azure Blob Storage.</span></span> <span data-ttu-id="75914-129">Pak spusťte skript Polybase, který načte data do SQL Data Warehouse pomocí úlohy služby SSIS provést SQL.</span><span class="sxs-lookup"><span data-stu-id="75914-129">Then use the SSIS Execute SQL task to launch a Polybase script that loads the data into SQL Data Warehouse.</span></span> <span data-ttu-id="75914-130">Tato možnost poskytuje nejlepší výkon ze tří zde uvedené možnosti.</span><span class="sxs-lookup"><span data-stu-id="75914-130">This option provides the best performance of the three options listed here.</span></span> <span data-ttu-id="75914-131">Chcete-li získat nahrát objekt Blob Azure úloh, stáhnout [Microsoft SQL Server 2016 integrační služby Feature Pack pro Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span><span class="sxs-lookup"><span data-stu-id="75914-131">To get the Azure Blob Upload task, download the [Microsoft SQL Server 2016 Integration Services Feature Pack for Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span></span> <span data-ttu-id="75914-132">Další informace o používání funkce Polybase, najdete v části [Průvodce funkcí PolyBase][PolyBase Guide].</span><span class="sxs-lookup"><span data-stu-id="75914-132">To learn more about Polybase, see [PolyBase Guide][PolyBase Guide].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="75914-133">Než začnete</span><span class="sxs-lookup"><span data-stu-id="75914-133">Before you start</span></span>
<span data-ttu-id="75914-134">Pro jednotlivé kroky v tomto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="75914-134">To step through this tutorial, you need:</span></span>

1. <span data-ttu-id="75914-135">**SQL Server Integration Services (služby SSIS)**.</span><span class="sxs-lookup"><span data-stu-id="75914-135">**SQL Server Integration Services (SSIS)**.</span></span> <span data-ttu-id="75914-136">Služby SSIS je součástí systému SQL Server a vyžaduje zkušební verze nebo verze na licencovanou verzi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="75914-136">SSIS is a component of SQL Server and requires an evaluation version or a licensed version of SQL Server.</span></span> <span data-ttu-id="75914-137">Chcete-li získat zkušební verze systému SQL Server 2016 ve verzi Preview, přečtěte si téma [SQL serveru hodnocení][SQL Server Evaluations].</span><span class="sxs-lookup"><span data-stu-id="75914-137">To get an evaluation version of SQL Server 2016 Preview, see [SQL Server Evaluations][SQL Server Evaluations].</span></span>
2. <span data-ttu-id="75914-138">Sadu **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="75914-138">**Visual Studio**.</span></span> <span data-ttu-id="75914-139">Bezplatná edice Community Visual Studio najdete [Visual Studio Community][Visual Studio Community].</span><span class="sxs-lookup"><span data-stu-id="75914-139">To get the free Visual Studio Community Edition, see [Visual Studio Community][Visual Studio Community].</span></span>
3. <span data-ttu-id="75914-140">**Nástroje SQL Server Data Tools pro sadu Visual Studio (SSDT)**.</span><span class="sxs-lookup"><span data-stu-id="75914-140">**SQL Server Data Tools for Visual Studio (SSDT)**.</span></span> <span data-ttu-id="75914-141">SQL Server Data Tools pro sadu Visual Studio najdete [stáhnout SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span><span class="sxs-lookup"><span data-stu-id="75914-141">To get SQL Server Data Tools for Visual Studio, see [Download SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span></span>
4. <span data-ttu-id="75914-142">**Ukázková data**.</span><span class="sxs-lookup"><span data-stu-id="75914-142">**Sample data**.</span></span> <span data-ttu-id="75914-143">Tento kurz používá ukázková data uložená v systému SQL Server v ukázkové databázi AdventureWorks jako zdroj dat má být načten do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-143">This tutorial uses sample data stored in SQL Server in the AdventureWorks sample database as the source data to be loaded into SQL Data Warehouse.</span></span> <span data-ttu-id="75914-144">Chcete-li získat ukázkovou databázi AdventureWorks, přečtěte si téma [AdventureWorks 2014 ukázkové databáze][AdventureWorks 2014 Sample Databases].</span><span class="sxs-lookup"><span data-stu-id="75914-144">To get the AdventureWorks sample database, see [AdventureWorks 2014 Sample Databases][AdventureWorks 2014 Sample Databases].</span></span>
5. <span data-ttu-id="75914-145">**Databáze SQL Data Warehouse a oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="75914-145">**A SQL Data Warehouse database and permissions**.</span></span> <span data-ttu-id="75914-146">V tomto kurzu se připojí k instanci SQL Data Warehouse a načte data do ní.</span><span class="sxs-lookup"><span data-stu-id="75914-146">This tutorial connects to a SQL Data Warehouse instance and loads data into it.</span></span> <span data-ttu-id="75914-147">Musíte mít oprávnění k vytvoření tabulky a načíst data.</span><span class="sxs-lookup"><span data-stu-id="75914-147">You have to have permissions to create a table and to load data.</span></span>
6. <span data-ttu-id="75914-148">**Pravidlo brány firewall**.</span><span class="sxs-lookup"><span data-stu-id="75914-148">**A firewall rule**.</span></span> <span data-ttu-id="75914-149">Budete muset vytvořit pravidlo brány firewall v SQL Data Warehouse s IP adresou místního počítače, než můžete uložit data do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-149">You have to create a firewall rule on SQL Data Warehouse with the IP address of your local computer before you can upload data to the SQL Data Warehouse.</span></span>

## <a name="step-1-create-a-new-integration-services-project"></a><span data-ttu-id="75914-150">Krok 1: Vytvoření nového projektu integrační služby</span><span class="sxs-lookup"><span data-stu-id="75914-150">Step 1: Create a new Integration Services project</span></span>
1. <span data-ttu-id="75914-151">Spusťte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75914-151">Launch Visual Studio.</span></span>
2. <span data-ttu-id="75914-152">Na **soubor** nabídce vyberte možnost **nový | Projekt**.</span><span class="sxs-lookup"><span data-stu-id="75914-152">On the **File** menu, select **New | Project**.</span></span>
3. <span data-ttu-id="75914-153">Přejděte do **nainstalován | Šablony | Business Intelligence | Integrační služby** typy projektů.</span><span class="sxs-lookup"><span data-stu-id="75914-153">Navigate to the **Installed | Templates | Business Intelligence | Integration Services** project types.</span></span>
4. <span data-ttu-id="75914-154">Vyberte **integračních služeb projektu**.</span><span class="sxs-lookup"><span data-stu-id="75914-154">Select **Integration Services Project**.</span></span> <span data-ttu-id="75914-155">Zadejte hodnoty pro **název** a **umístění**a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="75914-155">Provide values for **Name** and **Location**, and then select **OK**.</span></span>

<span data-ttu-id="75914-156">Visual Studio otevře a vytvoří nový projekt Integration Services (SSIS).</span><span class="sxs-lookup"><span data-stu-id="75914-156">Visual Studio opens and creates a new Integration Services (SSIS) project.</span></span> <span data-ttu-id="75914-157">Pak Visual Studio otevře návrháře jednoho nového balíčku služby SSIS (Package.dtsx) v projektu.</span><span class="sxs-lookup"><span data-stu-id="75914-157">Then Visual Studio opens the designer for the single new SSIS package (Package.dtsx) in the project.</span></span> <span data-ttu-id="75914-158">Zobrazí následující obrazovka oblastí:</span><span class="sxs-lookup"><span data-stu-id="75914-158">You see the following screen areas:</span></span>

* <span data-ttu-id="75914-159">Na levé straně **sada nástrojů** součásti služby SSIS.</span><span class="sxs-lookup"><span data-stu-id="75914-159">On the left, the **Toolbox** of SSIS components.</span></span>
* <span data-ttu-id="75914-160">Uprostřed klikněte na návrhovou plochu, s několik karet.</span><span class="sxs-lookup"><span data-stu-id="75914-160">In the middle, the design surface, with multiple tabs.</span></span> <span data-ttu-id="75914-161">Obvykle se používá minimálně **tok řízení** a **toku dat** karty.</span><span class="sxs-lookup"><span data-stu-id="75914-161">You typically use at least the **Control Flow** and the **Data Flow** tabs.</span></span>
* <span data-ttu-id="75914-162">Na pravé straně **Průzkumníku řešení** a **vlastnosti** podokna.</span><span class="sxs-lookup"><span data-stu-id="75914-162">On the right, the **Solution Explorer** and the **Properties** panes.</span></span>
  
    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a><span data-ttu-id="75914-163">Krok 2: Vytvoření základní datový tok</span><span class="sxs-lookup"><span data-stu-id="75914-163">Step 2: Create the basic data flow</span></span>
1. <span data-ttu-id="75914-164">Přetáhněte úlohu toku dat z panelu nástrojů k centru návrhové ploše (na **tok řízení** karta).</span><span class="sxs-lookup"><span data-stu-id="75914-164">Drag a Data Flow Task from the Toolbox to the center of the design surface (on the **Control Flow** tab).</span></span>
   
    ![][02]
2. <span data-ttu-id="75914-165">Dvakrát klikněte na úlohu toku dat přejděte na kartu toku dat.</span><span class="sxs-lookup"><span data-stu-id="75914-165">Double-click the Data Flow Task to switch to the Data Flow tab.</span></span>
3. <span data-ttu-id="75914-166">Ze seznamu jiných zdrojů v panelu nástrojů přetáhněte na zdroj ADO.NET na plochu návrháře.</span><span class="sxs-lookup"><span data-stu-id="75914-166">From the Other Sources list in the Toolbox, drag an ADO.NET Source to the design surface.</span></span> <span data-ttu-id="75914-167">S adaptérem zdroj vybraný, změňte její název **zdroje systému SQL Server** v **vlastnosti** podokně.</span><span class="sxs-lookup"><span data-stu-id="75914-167">With the source adapter still selected, change its name to **SQL Server source** in the **Properties** pane.</span></span>
4. <span data-ttu-id="75914-168">Z jiných seznamu v panelu nástrojů a přetáhněte na návrhovou plochu pod zdrojem ADO.NET cíl pro technologii ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="75914-168">From the Other Destinations list in the Toolbox, drag an ADO.NET Destination to the design surface under the ADO.NET Source.</span></span> <span data-ttu-id="75914-169">Cílový adaptér, vybraný, změňte její název **SQL DW cílové** v **vlastnosti** podokně.</span><span class="sxs-lookup"><span data-stu-id="75914-169">With the destination adapter still selected, change its name to **SQL DW destination** in the **Properties** pane.</span></span>
   
    ![][09]

## <a name="step-3-configure-the-source-adapter"></a><span data-ttu-id="75914-170">Krok 3: Konfigurace adaptér zdroje</span><span class="sxs-lookup"><span data-stu-id="75914-170">Step 3: Configure the source adapter</span></span>
1. <span data-ttu-id="75914-171">Poklikejte na adaptér zdroj otevřete **Editor zdroje ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="75914-171">Double-click the source adapter to open the **ADO.NET Source Editor**.</span></span>
   
    ![][03]
2. <span data-ttu-id="75914-172">Na **Správce připojení** kartě **Editor zdroje ADO.NET**, klikněte na tlačítko **nový** vedle položky **Správce připojení ADO.NET** seznamu Chcete-li otevřít **nakonfigurovat správce připojení ADO.NET** dialogové okno a vytvořte nastavení připojení pro databázi systému SQL Server, ze kterého v tomto kurzu načte data.</span><span class="sxs-lookup"><span data-stu-id="75914-172">On the **Connection Manager** tab of the **ADO.NET Source Editor**, click the **New** button next to the **ADO.NET connection manager** list to open the **Configure ADO.NET Connection Manager** dialog box and create connection settings for the SQL Server database from which this tutorial loads data.</span></span>
   
    ![][04]
3. <span data-ttu-id="75914-173">V **nakonfigurovat správce připojení ADO.NET** dialogové okno, klikněte na tlačítko **nový** tlačítko Otevřít **Správce připojení** dialogové okno a vytvořte nové datové připojení.</span><span class="sxs-lookup"><span data-stu-id="75914-173">In the **Configure ADO.NET Connection Manager** dialog box, click the **New** button to open the **Connection Manager** dialog box and create a new data connection.</span></span>
   
    ![][05]
4. <span data-ttu-id="75914-174">V **Správce připojení** dialogové okno pole, provádět následující akce.</span><span class="sxs-lookup"><span data-stu-id="75914-174">In the **Connection Manager** dialog box, do the following things.</span></span>
   
   1. <span data-ttu-id="75914-175">Pro **zprostředkovatele**, vyberte poskytovatel dat SqlClient.</span><span class="sxs-lookup"><span data-stu-id="75914-175">For **Provider**, select the SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="75914-176">Pro **název serveru**, zadejte název serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="75914-176">For **Server name**, enter the SQL Server name.</span></span>
   3. <span data-ttu-id="75914-177">V **Přihlaste se k serveru** vyberte nebo zadejte informace o ověřování.</span><span class="sxs-lookup"><span data-stu-id="75914-177">In the **Log on to the server** section, select or enter authentication information.</span></span>
   4. <span data-ttu-id="75914-178">V **připojit k databázi** vyberte ukázkovou databázi AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="75914-178">In the **Connect to a database** section, select the AdventureWorks sample database.</span></span>
   5. <span data-ttu-id="75914-179">Klikněte na tlačítko **testovací připojení**.</span><span class="sxs-lookup"><span data-stu-id="75914-179">Click **Test Connection**.</span></span>
      
       ![][06]
   6. <span data-ttu-id="75914-180">V dialogovém okně, které hlásí výsledky testu připojení, klikněte na **OK** se vrátíte do **Správce připojení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75914-180">In the dialog box that reports the results of the connection test, click **OK** to return to the **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="75914-181">V **Správce připojení** dialogové okno, klikněte na tlačítko **OK** se vrátíte do **nakonfigurovat správce připojení ADO.NET** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75914-181">In the **Connection Manager** dialog box, click **OK** to return to the **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="75914-182">V **nakonfigurovat správce připojení ADO.NET** dialogové okno, klikněte na tlačítko **OK** se vrátíte do **Editor zdroje ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="75914-182">In the **Configure ADO.NET Connection Manager** dialog box, click **OK** to return to the **ADO.NET Source Editor**.</span></span>
6. <span data-ttu-id="75914-183">V **Editor zdroje ADO.NET**v **název tabulky nebo zobrazení** seznamu, vyberte **Sales.SalesOrderDetail** tabulky.</span><span class="sxs-lookup"><span data-stu-id="75914-183">In the **ADO.NET Source Editor**, in the **Name of the table or the view** list, select the **Sales.SalesOrderDetail** table.</span></span>
   
    ![][07]
7. <span data-ttu-id="75914-184">Klikněte na tlačítko **Preview** zobrazíte prvních 200 řádků dat ve zdrojové tabulce v **náhled výsledků dotazu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75914-184">Click **Preview** to see the first 200 rows of data in the source table in the **Preview Query Results** dialog box.</span></span>
   
    ![][08]
8. <span data-ttu-id="75914-185">V **náhled výsledků dotazu** dialogové okno, klikněte na tlačítko **Zavřít** se vrátíte do **Editor zdroje ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="75914-185">In the **Preview Query Results** dialog box, click **Close** to return to the **ADO.NET Source Editor**.</span></span>
9. <span data-ttu-id="75914-186">V **Editor zdroje ADO.NET**, klikněte na tlačítko **OK** konfiguraci zdroje dat dokončíte.</span><span class="sxs-lookup"><span data-stu-id="75914-186">In the **ADO.NET Source Editor**, click **OK** to finish configuring the data source.</span></span>

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a><span data-ttu-id="75914-187">Krok 4: Připojte adaptér zdrojového na cílový adaptér</span><span class="sxs-lookup"><span data-stu-id="75914-187">Step 4: Connect the source adapter to the destination adapter</span></span>
1. <span data-ttu-id="75914-188">Vyberte zdroj adaptéru na návrhovou plochu.</span><span class="sxs-lookup"><span data-stu-id="75914-188">Select the source adapter on the design surface.</span></span>
2. <span data-ttu-id="75914-189">Vyberte modrou šipkou, který rozšiřuje z adaptéru zdroje a přetáhněte jej do editoru cílové, dokud ho přichytí.</span><span class="sxs-lookup"><span data-stu-id="75914-189">Select the blue arrow that extends from the source adapter and drag it to the destination editor until it snaps into place.</span></span>
   
    ![][10]
   
    <span data-ttu-id="75914-190">V typické SSIS balíčku použít několik dalších součásti ze sady nástrojů SSIS mezi zdroj a cíl změny struktury, transformace a čistí data při průchodu tok dat služby SSIS.</span><span class="sxs-lookup"><span data-stu-id="75914-190">In a typical SSIS package, you use a number of other components from the SSIS Toolbox in between the source and the destination to restructure, transform, and cleanse your data as it passes through the SSIS data flow.</span></span> <span data-ttu-id="75914-191">Aby tento příklad co nejjednodušší, jsme připojujete, přímo do cílového umístění zdroje.</span><span class="sxs-lookup"><span data-stu-id="75914-191">To keep this example as simple as possible, we’re connecting the source directly to the destination.</span></span>

## <a name="step-5-configure-the-destination-adapter"></a><span data-ttu-id="75914-192">Krok 5: Konfigurace cílový adaptér</span><span class="sxs-lookup"><span data-stu-id="75914-192">Step 5: Configure the destination adapter</span></span>
1. <span data-ttu-id="75914-193">Poklikejte na adaptér cílové otevřete **ADO.NET cílové Editor**.</span><span class="sxs-lookup"><span data-stu-id="75914-193">Double-click the destination adapter to open the **ADO.NET Destination Editor**.</span></span>
   
    ![][11]
2. <span data-ttu-id="75914-194">Na **Správce připojení** kartě **ADO.NET cílové Editor**, klikněte na tlačítko **nový** vedle položky **Správce připojení** se seznam Otevřete **nakonfigurovat správce připojení ADO.NET** dialogové okno a vytvořte nastavení připojení pro databázi Azure SQL Data Warehouse, do kterého v tomto kurzu načte data.</span><span class="sxs-lookup"><span data-stu-id="75914-194">On the **Connection Manager** tab of the **ADO.NET Destination Editor**, click the **New** button next to the **Connection manager** list to open the **Configure ADO.NET Connection Manager** dialog box and create connection settings for the Azure SQL Data Warehouse database into which this tutorial loads data.</span></span>
3. <span data-ttu-id="75914-195">V **nakonfigurovat správce připojení ADO.NET** dialogové okno, klikněte na tlačítko **nový** tlačítko Otevřít **Správce připojení** dialogové okno a vytvořte nové datové připojení.</span><span class="sxs-lookup"><span data-stu-id="75914-195">In the **Configure ADO.NET Connection Manager** dialog box, click the **New** button to open the **Connection Manager** dialog box and create a new data connection.</span></span>
4. <span data-ttu-id="75914-196">V **Správce připojení** dialogové okno pole, provádět následující akce.</span><span class="sxs-lookup"><span data-stu-id="75914-196">In the **Connection Manager** dialog box, do the following things.</span></span>
   1. <span data-ttu-id="75914-197">Pro **zprostředkovatele**, vyberte poskytovatel dat SqlClient.</span><span class="sxs-lookup"><span data-stu-id="75914-197">For **Provider**, select the SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="75914-198">Pro **název serveru**, zadejte název serveru SQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="75914-198">For **Server name**, enter the SQL Data Warehouse name.</span></span>
   3. <span data-ttu-id="75914-199">V **Přihlaste se k serveru** vyberte **ověřování použít SQL Server** a zadejte informace o ověřování.</span><span class="sxs-lookup"><span data-stu-id="75914-199">In the **Log on to the server** section, select **Use SQL Server authentication** and enter authentication information.</span></span>
   4. <span data-ttu-id="75914-200">V **připojit k databázi** vyberte existující databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-200">In the **Connect to a database** section, select an existing SQL Data Warehouse database.</span></span>
   5. <span data-ttu-id="75914-201">Klikněte na tlačítko **testovací připojení**.</span><span class="sxs-lookup"><span data-stu-id="75914-201">Click **Test Connection**.</span></span>
   6. <span data-ttu-id="75914-202">V dialogovém okně, které hlásí výsledky testu připojení, klikněte na **OK** se vrátíte do **Správce připojení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75914-202">In the dialog box that reports the results of the connection test, click **OK** to return to the **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="75914-203">V **Správce připojení** dialogové okno, klikněte na tlačítko **OK** se vrátíte do **nakonfigurovat správce připojení ADO.NET** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75914-203">In the **Connection Manager** dialog box, click **OK** to return to the **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="75914-204">V **nakonfigurovat správce připojení ADO.NET** dialogové okno, klikněte na tlačítko **OK** se vraťte do **ADO.NET cílové Editor**.</span><span class="sxs-lookup"><span data-stu-id="75914-204">In the **Configure ADO.NET Connection Manager** dialog box, click **OK** to return to the **ADO.NET Destination Editor**.</span></span>
6. <span data-ttu-id="75914-205">V **ADO.NET cílové Editor**, klikněte na tlačítko **nový** vedle **použití tabulky nebo zobrazení** seznamu otevřete **Create Table** dialogové okno vytvořit nové cílové tabulky se seznam sloupců, která odpovídá zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="75914-205">In the **ADO.NET Destination Editor**, click **New** next to the **Use a table or view** list to open the **Create Table** dialog box to create a new destination table with a column list that matches the source table.</span></span>
   
    ![][12a]
7. <span data-ttu-id="75914-206">V **Create Table** dialogové okno pole, provádět následující akce.</span><span class="sxs-lookup"><span data-stu-id="75914-206">In the **Create Table** dialog box, do the following things.</span></span>
   
   1. <span data-ttu-id="75914-207">Změnit název cílové tabulky pro **podrobnosti prodejní objednávky**.</span><span class="sxs-lookup"><span data-stu-id="75914-207">Change the name of the destination table to **SalesOrderDetail**.</span></span>
   2. <span data-ttu-id="75914-208">Odeberte **rowguid** sloupce.</span><span class="sxs-lookup"><span data-stu-id="75914-208">Remove the **rowguid** column.</span></span> <span data-ttu-id="75914-209">**Uniqueidentifier** datový typ není podporován v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-209">The **uniqueidentifier** data type is not supported in SQL Data Warehouse.</span></span>
   3. <span data-ttu-id="75914-210">Změnit datový typ **LineTotal** sloupec, který se **peníze**.</span><span class="sxs-lookup"><span data-stu-id="75914-210">Change the data type of the **LineTotal** column to **money**.</span></span> <span data-ttu-id="75914-211">**Decimal** datový typ není podporován v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-211">The **decimal** data type is not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="75914-212">Informace o podporované datové typy najdete v tématu [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="75914-212">For info about supported data types, see [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span></span>
      
       ![][12b]
   4. <span data-ttu-id="75914-213">Klikněte na tlačítko **OK** pro vytvoření tabulky a návrat na **ADO.NET cílové Editor**.</span><span class="sxs-lookup"><span data-stu-id="75914-213">Click **OK** to create the table and return to the **ADO.NET Destination Editor**.</span></span>
8. <span data-ttu-id="75914-214">V **ADO.NET cílové Editor**, vyberte **mapování** zjistit, jak sloupce ve zdroji jsou mapované na sloupce v cílovém.</span><span class="sxs-lookup"><span data-stu-id="75914-214">In the **ADO.NET Destination Editor**, select the **Mappings** tab to see how columns in the source are mapped to columns in the destination.</span></span>
   
    ![][13]
9. <span data-ttu-id="75914-215">Klikněte na tlačítko **OK** konfiguraci zdroje dat dokončíte.</span><span class="sxs-lookup"><span data-stu-id="75914-215">Click **OK** to finish configuring the data source.</span></span>

## <a name="step-6-run-the-package-to-load-the-data"></a><span data-ttu-id="75914-216">Krok 6: Spuštění balíček, který chcete načíst data</span><span class="sxs-lookup"><span data-stu-id="75914-216">Step 6: Run the package to load the data</span></span>
<span data-ttu-id="75914-217">Spusťte balíček kliknutím **spustit** tlačítka na panelu nástrojů nebo výběrem jedné z **spustit** možnosti na **ladění** nabídky.</span><span class="sxs-lookup"><span data-stu-id="75914-217">Run the package by clicking the **Start** button on the toolbar or by selecting one of the **Run** options on the **Debug** menu.</span></span>

<span data-ttu-id="75914-218">Jako balíček začne spustit, zobrazí žlutý souborů Wheel roztočený udávajících aktivit a také počet řádků, pokud zpracovat.</span><span class="sxs-lookup"><span data-stu-id="75914-218">As the package begins to run, you see yellow spinning wheels to indicate activity as well as the number of rows processed so far.</span></span>

![][14]

<span data-ttu-id="75914-219">Pokud balíček bylo dokončeno, uvidíte zelené značky zaškrtnutí indikující úspěch a také celkový počet řádků dat načtených ze zdrojové do cílové.</span><span class="sxs-lookup"><span data-stu-id="75914-219">When the package has finished running, you see green check marks to indicate success as well as the total number of rows of data loaded from the source to the destination.</span></span>

![][15]

<span data-ttu-id="75914-220">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="75914-220">Congratulations!</span></span> <span data-ttu-id="75914-221">SQL Server Integration Services jste úspěšně použili k načtení dat do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="75914-221">You’ve successfully used SQL Server Integration Services to load data into Azure SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75914-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75914-222">Next steps</span></span>
* <span data-ttu-id="75914-223">Další informace o toku dat SSIS.</span><span class="sxs-lookup"><span data-stu-id="75914-223">Learn more about the SSIS data flow.</span></span> <span data-ttu-id="75914-224">Začněte zde: [toku dat][Data Flow].</span><span class="sxs-lookup"><span data-stu-id="75914-224">Start here: [Data Flow][Data Flow].</span></span>
* <span data-ttu-id="75914-225">Informace o ladění a odstraňování potíží v prostředí návrhu vaší balíčky správné.</span><span class="sxs-lookup"><span data-stu-id="75914-225">Learn how to debug and troubleshoot your packages right in the design environment.</span></span> <span data-ttu-id="75914-226">Začněte zde: [řešení potíží s nástrojů pro vývoj balíček][Troubleshooting Tools for Package Development].</span><span class="sxs-lookup"><span data-stu-id="75914-226">Start here: [Troubleshooting Tools for Package Development][Troubleshooting Tools for Package Development].</span></span>
* <span data-ttu-id="75914-227">Informace o nasazení služby SSIS projekty a balíčky Server integračních služeb nebo do jiného umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="75914-227">Learn how to deploy your SSIS projects and packages to Integration Services Server or to another storage location.</span></span> <span data-ttu-id="75914-228">Začněte zde: [projekty nasazení a balíčky][Deployment of Projects and Packages].</span><span class="sxs-lookup"><span data-stu-id="75914-228">Start here: [Deployment of Projects and Packages][Deployment of Projects and Packages].</span></span>

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Data Flow]: https://msdn.microsoft.com/library/ms140080.aspx
[Troubleshooting Tools for Package Development]: https://msdn.microsoft.com/library/ms137625.aspx
[Deployment of Projects and Packages]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
