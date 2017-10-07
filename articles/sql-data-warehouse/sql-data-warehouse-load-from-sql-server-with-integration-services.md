---
title: aaaLoad dat z SQL serveru do Azure SQL Data Warehouse (SSIS) | Microsoft Docs
description: "Ukazuje, jak toocreate toomove data balíčku integrační služby SSIS (SQL Server) ze široké škály data zdroje tooSQL datového skladu."
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
ms.openlocfilehash: bb28a08807a5b07832b85f2f074c2acf912c1dc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a><span data-ttu-id="35cee-103">Načtení dat z SQL serveru do Azure SQL Data Warehouse služby SSIS)</span><span class="sxs-lookup"><span data-stu-id="35cee-103">Load data from SQL Server into Azure SQL Data Warehouse (SSIS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="35cee-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="35cee-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="35cee-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="35cee-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="35cee-106">bcp</span><span class="sxs-lookup"><span data-stu-id="35cee-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="35cee-107">Vytvořte data tooload balíčku integrační služby SSIS (SQL Server) ze serveru SQL Server do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-107">Create a SQL Server Integration Services (SSIS) package tooload data from SQL Server into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="35cee-108">Volitelně můžete změny struktury, transformace a čistí hello data při průchodu hello SSIS datového toku.</span><span class="sxs-lookup"><span data-stu-id="35cee-108">You can optionally restructure, transform, and cleanse hello data as it passes through hello SSIS data flow.</span></span>

<span data-ttu-id="35cee-109">V tomto kurzu provedete následující:</span><span class="sxs-lookup"><span data-stu-id="35cee-109">In this tutorial, you will:</span></span>

* <span data-ttu-id="35cee-110">V sadě Visual Studio vytvořte nový projekt integrační služby.</span><span class="sxs-lookup"><span data-stu-id="35cee-110">Create a new Integration Services project in Visual Studio.</span></span>
* <span data-ttu-id="35cee-111">Připojte toodata zdrojů, včetně systému SQL Server (jako zdroj) a SQL Data Warehouse (jako cíl).</span><span class="sxs-lookup"><span data-stu-id="35cee-111">Connect toodata sources, including SQL Server (as a source) and SQL Data Warehouse (as a destination).</span></span>
* <span data-ttu-id="35cee-112">Návrh služby SSIS balíček, který načte data ze zdroje hello do cílové hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-112">Design an SSIS package that loads data from hello source into hello destination.</span></span>
* <span data-ttu-id="35cee-113">Spuštění hello data hello tooload balíčku služby SSIS.</span><span class="sxs-lookup"><span data-stu-id="35cee-113">Run hello SSIS package tooload hello data.</span></span>

<span data-ttu-id="35cee-114">Tento kurz používá SQL Server jako zdroj dat hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-114">This tutorial uses SQL Server as hello data source.</span></span> <span data-ttu-id="35cee-115">SQL Server může být spuštěn místně nebo v virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="35cee-115">SQL Server could be running on premises or in an Azure virtual machine.</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="35cee-116">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="35cee-116">Basic concepts</span></span>
<span data-ttu-id="35cee-117">balíček Hello je hello jednotky práce v SSIS.</span><span class="sxs-lookup"><span data-stu-id="35cee-117">hello package is hello unit of work in SSIS.</span></span> <span data-ttu-id="35cee-118">Související balíčky jsou seskupené v projektech.</span><span class="sxs-lookup"><span data-stu-id="35cee-118">Related packages are grouped in projects.</span></span> <span data-ttu-id="35cee-119">Vytváření projektů a balíčků návrhu v sadě Visual Studio s SQL Server Data Tools.</span><span class="sxs-lookup"><span data-stu-id="35cee-119">You create projects and design packages in Visual Studio with SQL Server Data Tools.</span></span> <span data-ttu-id="35cee-120">Hello návrhu, že je visual proces, ve kterém můžete přetáhnout myší součásti z hello sady nástrojů toohello návrhovou plochu, je připojení a nastavení jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="35cee-120">hello design process is a visual process in which you drag and drop components from hello Toolbox toohello design surface, connect them, and set their properties.</span></span> <span data-ttu-id="35cee-121">Po dokončení vašeho balíčku, můžete volitelně nasadit tooSQL serveru pro komplexní správu, monitorování a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="35cee-121">After you finish your package, you can optionally deploy it tooSQL Server for comprehensive management, monitoring, and security.</span></span>

## <a name="options-for-loading-data-with-ssis"></a><span data-ttu-id="35cee-122">Možnosti pro načítání dat pomocí služby SSIS</span><span class="sxs-lookup"><span data-stu-id="35cee-122">Options for loading data with SSIS</span></span>
<span data-ttu-id="35cee-123">Integrace služby SSIS (SQL Server) je flexibilní sadu nástrojů, který poskytuje celou řadu možností pro připojení k a načítání dat do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-123">SQL Server Integration Services (SSIS) is a flexible set of tools that provides a variety of options for connecting to, and loading data into, SQL Data Warehouse.</span></span>

1. <span data-ttu-id="35cee-124">Použijte určení NET ADO tooconnect tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="35cee-124">Use an ADO NET Destination tooconnect tooSQL Data Warehouse.</span></span> <span data-ttu-id="35cee-125">Tento kurz používá určení NET ADO, protože má nejmenší počet možností konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-125">This tutorial uses an ADO NET Destination because it has hello fewest configuration options.</span></span>
2. <span data-ttu-id="35cee-126">Použijte určení technologie OLE DB tooconnect tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="35cee-126">Use an OLE DB Destination tooconnect tooSQL Data Warehouse.</span></span> <span data-ttu-id="35cee-127">Tato možnost může poskytovat mírně lepší výkon než hello ADO NET cílový.</span><span class="sxs-lookup"><span data-stu-id="35cee-127">This option may provide slightly better performance than hello ADO NET Destination.</span></span>
3. <span data-ttu-id="35cee-128">Použití hello úloh nahrát objekt Blob Azure toostage hello dat v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="35cee-128">Use hello Azure Blob Upload Task toostage hello data in Azure Blob Storage.</span></span> <span data-ttu-id="35cee-129">Potom pomocí hello SSIS provést SQL úloh toolaunch Polybase skript, který načte hello data do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-129">Then use hello SSIS Execute SQL task toolaunch a Polybase script that loads hello data into SQL Data Warehouse.</span></span> <span data-ttu-id="35cee-130">Tato možnost poskytuje nejlepší výkon hello hello tři možnosti uvedené v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="35cee-130">This option provides hello best performance of hello three options listed here.</span></span> <span data-ttu-id="35cee-131">tooget hello nahrát objekt Blob Azure úloh, stáhnout hello [Microsoft SQL Server 2016 integrační služby Feature Pack pro Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span><span class="sxs-lookup"><span data-stu-id="35cee-131">tooget hello Azure Blob Upload task, download hello [Microsoft SQL Server 2016 Integration Services Feature Pack for Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span></span> <span data-ttu-id="35cee-132">toolearn Další informace o používání funkce Polybase, najdete v části [Průvodce funkcí PolyBase][PolyBase Guide].</span><span class="sxs-lookup"><span data-stu-id="35cee-132">toolearn more about Polybase, see [PolyBase Guide][PolyBase Guide].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="35cee-133">Než začnete</span><span class="sxs-lookup"><span data-stu-id="35cee-133">Before you start</span></span>
<span data-ttu-id="35cee-134">toostep prostřednictvím tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="35cee-134">toostep through this tutorial, you need:</span></span>

1. <span data-ttu-id="35cee-135">**SQL Server Integration Services (služby SSIS)**.</span><span class="sxs-lookup"><span data-stu-id="35cee-135">**SQL Server Integration Services (SSIS)**.</span></span> <span data-ttu-id="35cee-136">Služby SSIS je součástí systému SQL Server a vyžaduje zkušební verze nebo verze na licencovanou verzi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="35cee-136">SSIS is a component of SQL Server and requires an evaluation version or a licensed version of SQL Server.</span></span> <span data-ttu-id="35cee-137">tooget zkušební verze systému SQL Server 2016 ve verzi Preview, najdete v části [SQL serveru hodnocení][SQL Server Evaluations].</span><span class="sxs-lookup"><span data-stu-id="35cee-137">tooget an evaluation version of SQL Server 2016 Preview, see [SQL Server Evaluations][SQL Server Evaluations].</span></span>
2. <span data-ttu-id="35cee-138">Sadu **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="35cee-138">**Visual Studio**.</span></span> <span data-ttu-id="35cee-139">tooget hello bezplatná edice Community Visual Studio najdete v tématu [Visual Studio Community][Visual Studio Community].</span><span class="sxs-lookup"><span data-stu-id="35cee-139">tooget hello free Visual Studio Community Edition, see [Visual Studio Community][Visual Studio Community].</span></span>
3. <span data-ttu-id="35cee-140">**Nástroje SQL Server Data Tools pro sadu Visual Studio (SSDT)**.</span><span class="sxs-lookup"><span data-stu-id="35cee-140">**SQL Server Data Tools for Visual Studio (SSDT)**.</span></span> <span data-ttu-id="35cee-141">tooget SQL Server Data Tools pro sadu Visual Studio, najdete v části [stáhnout SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span><span class="sxs-lookup"><span data-stu-id="35cee-141">tooget SQL Server Data Tools for Visual Studio, see [Download SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span></span>
4. <span data-ttu-id="35cee-142">**Ukázková data**.</span><span class="sxs-lookup"><span data-stu-id="35cee-142">**Sample data**.</span></span> <span data-ttu-id="35cee-143">Tento kurz používá ukázková data uložena v systému SQL Server v ukázkovou databázi AdventureWorks hello, protože hello zdroje dat toobe načíst do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-143">This tutorial uses sample data stored in SQL Server in hello AdventureWorks sample database as hello source data toobe loaded into SQL Data Warehouse.</span></span> <span data-ttu-id="35cee-144">tooget hello ukázkovou databázi AdventureWorks, najdete v části [AdventureWorks 2014 ukázkové databáze][AdventureWorks 2014 Sample Databases].</span><span class="sxs-lookup"><span data-stu-id="35cee-144">tooget hello AdventureWorks sample database, see [AdventureWorks 2014 Sample Databases][AdventureWorks 2014 Sample Databases].</span></span>
5. <span data-ttu-id="35cee-145">**Databáze SQL Data Warehouse a oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="35cee-145">**A SQL Data Warehouse database and permissions**.</span></span> <span data-ttu-id="35cee-146">V tomto kurzu připojí tooa instanci SQL Data Warehouse a načte data do ní.</span><span class="sxs-lookup"><span data-stu-id="35cee-146">This tutorial connects tooa SQL Data Warehouse instance and loads data into it.</span></span> <span data-ttu-id="35cee-147">Máte toohave oprávnění toocreate tabulky a tooload data.</span><span class="sxs-lookup"><span data-stu-id="35cee-147">You have toohave permissions toocreate a table and tooload data.</span></span>
6. <span data-ttu-id="35cee-148">**Pravidlo brány firewall**.</span><span class="sxs-lookup"><span data-stu-id="35cee-148">**A firewall rule**.</span></span> <span data-ttu-id="35cee-149">Máte toocreate pravidlo brány firewall v SQL Data Warehouse s hello IP adresu místního počítače předtím, než můžete nahrát data toohello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-149">You have toocreate a firewall rule on SQL Data Warehouse with hello IP address of your local computer before you can upload data toohello SQL Data Warehouse.</span></span>

## <a name="step-1-create-a-new-integration-services-project"></a><span data-ttu-id="35cee-150">Krok 1: Vytvoření nového projektu integrační služby</span><span class="sxs-lookup"><span data-stu-id="35cee-150">Step 1: Create a new Integration Services project</span></span>
1. <span data-ttu-id="35cee-151">Spusťte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="35cee-151">Launch Visual Studio.</span></span>
2. <span data-ttu-id="35cee-152">Na hello **soubor** nabídce vyberte možnost **nový | Projekt**.</span><span class="sxs-lookup"><span data-stu-id="35cee-152">On hello **File** menu, select **New | Project**.</span></span>
3. <span data-ttu-id="35cee-153">Přejděte toohello **nainstalovaná | Šablony | Business Intelligence | Integrační služby** typy projektů.</span><span class="sxs-lookup"><span data-stu-id="35cee-153">Navigate toohello **Installed | Templates | Business Intelligence | Integration Services** project types.</span></span>
4. <span data-ttu-id="35cee-154">Vyberte **integračních služeb projektu**.</span><span class="sxs-lookup"><span data-stu-id="35cee-154">Select **Integration Services Project**.</span></span> <span data-ttu-id="35cee-155">Zadejte hodnoty pro **název** a **umístění**a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="35cee-155">Provide values for **Name** and **Location**, and then select **OK**.</span></span>

<span data-ttu-id="35cee-156">Visual Studio otevře a vytvoří nový projekt Integration Services (SSIS).</span><span class="sxs-lookup"><span data-stu-id="35cee-156">Visual Studio opens and creates a new Integration Services (SSIS) project.</span></span> <span data-ttu-id="35cee-157">Pak Visual Studio otevře hello designer pro hello jednoho nového SSIS balíčku (Package.dtsx) v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-157">Then Visual Studio opens hello designer for hello single new SSIS package (Package.dtsx) in hello project.</span></span> <span data-ttu-id="35cee-158">Zobrazí následující obrazovka oblasti hello:</span><span class="sxs-lookup"><span data-stu-id="35cee-158">You see hello following screen areas:</span></span>

* <span data-ttu-id="35cee-159">Na levé straně hello hello **sada nástrojů** součásti služby SSIS.</span><span class="sxs-lookup"><span data-stu-id="35cee-159">On hello left, hello **Toolbox** of SSIS components.</span></span>
* <span data-ttu-id="35cee-160">Uprostřed hello hello návrhovou plochu, s několik karet.</span><span class="sxs-lookup"><span data-stu-id="35cee-160">In hello middle, hello design surface, with multiple tabs.</span></span> <span data-ttu-id="35cee-161">Obvykle se používá minimálně hello **tok řízení** a hello **toku dat** karty.</span><span class="sxs-lookup"><span data-stu-id="35cee-161">You typically use at least hello **Control Flow** and hello **Data Flow** tabs.</span></span>
* <span data-ttu-id="35cee-162">Na pravém hello, hello **Průzkumníku řešení** a hello **vlastnosti** podokna.</span><span class="sxs-lookup"><span data-stu-id="35cee-162">On hello right, hello **Solution Explorer** and hello **Properties** panes.</span></span>
  
    ![][01]

## <a name="step-2-create-hello-basic-data-flow"></a><span data-ttu-id="35cee-163">Krok 2: Vytvoření základní datový tok hello</span><span class="sxs-lookup"><span data-stu-id="35cee-163">Step 2: Create hello basic data flow</span></span>
1. <span data-ttu-id="35cee-164">Přetáhněte úlohu toku dat z centra toohello sady nástrojů hello hello návrhové plochy aplikace (na hello **tok řízení** karta).</span><span class="sxs-lookup"><span data-stu-id="35cee-164">Drag a Data Flow Task from hello Toolbox toohello center of hello design surface (on hello **Control Flow** tab).</span></span>
   
    ![][02]
2. <span data-ttu-id="35cee-165">Dvakrát klikněte na hello úlohy toku dat tooswitch toohello toku dat kartě.</span><span class="sxs-lookup"><span data-stu-id="35cee-165">Double-click hello Data Flow Task tooswitch toohello Data Flow tab.</span></span>
3. <span data-ttu-id="35cee-166">Z jiných zdrojů seznamu hello v hello sada nástrojů přetáhněte návrhovou plochu toohello zdroje ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="35cee-166">From hello Other Sources list in hello Toolbox, drag an ADO.NET Source toohello design surface.</span></span> <span data-ttu-id="35cee-167">Adaptérem hello zdroj vybraný, změňte její název příliš**zdroje systému SQL Server** v hello **vlastnosti** podokně.</span><span class="sxs-lookup"><span data-stu-id="35cee-167">With hello source adapter still selected, change its name too**SQL Server source** in hello **Properties** pane.</span></span>
4. <span data-ttu-id="35cee-168">Hello jiné cíle seznamu hello sada nástrojů přetáhněte ADO.NET cílové toohello návrhová plocha pod hello zdroje ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="35cee-168">From hello Other Destinations list in hello Toolbox, drag an ADO.NET Destination toohello design surface under hello ADO.NET Source.</span></span> <span data-ttu-id="35cee-169">Cílový adaptér hello stále vybrán, změnit její název příliš**SQL DW cílové** v hello **vlastnosti** podokně.</span><span class="sxs-lookup"><span data-stu-id="35cee-169">With hello destination adapter still selected, change its name too**SQL DW destination** in hello **Properties** pane.</span></span>
   
    ![][09]

## <a name="step-3-configure-hello-source-adapter"></a><span data-ttu-id="35cee-170">Krok 3: Konfigurace adaptér zdroje hello</span><span class="sxs-lookup"><span data-stu-id="35cee-170">Step 3: Configure hello source adapter</span></span>
1. <span data-ttu-id="35cee-171">Klikněte dvakrát na hello zdroj adaptér tooopen hello **Editor zdroje ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="35cee-171">Double-click hello source adapter tooopen hello **ADO.NET Source Editor**.</span></span>
   
    ![][03]
2. <span data-ttu-id="35cee-172">Na hello **Správce připojení** kartě hello **Editor zdroje ADO.NET**, klikněte na tlačítko hello **nový** tlačítko Další toohello **Správce připojení ADO.NET**seznamu tooopen hello **nakonfigurovat správce připojení ADO.NET** dialogové okno a vytvořte nastavení připojení pro databázi systému SQL Server hello, ze kterého v tomto kurzu načte data.</span><span class="sxs-lookup"><span data-stu-id="35cee-172">On hello **Connection Manager** tab of hello **ADO.NET Source Editor**, click hello **New** button next toohello **ADO.NET connection manager** list tooopen hello **Configure ADO.NET Connection Manager** dialog box and create connection settings for hello SQL Server database from which this tutorial loads data.</span></span>
   
    ![][04]
3. <span data-ttu-id="35cee-173">V hello **nakonfigurovat správce připojení ADO.NET** dialogové okno pole, klikněte na tlačítko hello **nový** tlačítko tooopen hello **Správce připojení** dialogové okno a vytvořte nové datové připojení.</span><span class="sxs-lookup"><span data-stu-id="35cee-173">In hello **Configure ADO.NET Connection Manager** dialog box, click hello **New** button tooopen hello **Connection Manager** dialog box and create a new data connection.</span></span>
   
    ![][05]
4. <span data-ttu-id="35cee-174">V hello **Správce připojení** dialogové okno pole, hello následující věci.</span><span class="sxs-lookup"><span data-stu-id="35cee-174">In hello **Connection Manager** dialog box, do hello following things.</span></span>
   
   1. <span data-ttu-id="35cee-175">Pro **zprostředkovatele**, vyberte hello zprostředkovatele dat SqlClient.</span><span class="sxs-lookup"><span data-stu-id="35cee-175">For **Provider**, select hello SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="35cee-176">Pro **název serveru**, zadejte název serveru SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-176">For **Server name**, enter hello SQL Server name.</span></span>
   3. <span data-ttu-id="35cee-177">V hello **protokolu na serveru toohello** vyberte nebo zadejte informace o ověřování.</span><span class="sxs-lookup"><span data-stu-id="35cee-177">In hello **Log on toohello server** section, select or enter authentication information.</span></span>
   4. <span data-ttu-id="35cee-178">V hello **Connect tooa databáze** vyberte ukázkovou databázi AdventureWorks hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-178">In hello **Connect tooa database** section, select hello AdventureWorks sample database.</span></span>
   5. <span data-ttu-id="35cee-179">Klikněte na tlačítko **testovací připojení**.</span><span class="sxs-lookup"><span data-stu-id="35cee-179">Click **Test Connection**.</span></span>
      
       ![][06]
   6. <span data-ttu-id="35cee-180">V hello dialogové okno sestav hello výsledky testu hello připojení, klikněte na **OK** tooreturn toohello **Správce připojení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35cee-180">In hello dialog box that reports hello results of hello connection test, click **OK** tooreturn toohello **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="35cee-181">V hello **Správce připojení** dialogové okno, klikněte na tlačítko **OK** tooreturn toohello **nakonfigurovat správce připojení ADO.NET** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35cee-181">In hello **Connection Manager** dialog box, click **OK** tooreturn toohello **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="35cee-182">V hello **nakonfigurovat správce připojení ADO.NET** dialogové okno, klikněte na tlačítko **OK** tooreturn toohello **Editor zdroje ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="35cee-182">In hello **Configure ADO.NET Connection Manager** dialog box, click **OK** tooreturn toohello **ADO.NET Source Editor**.</span></span>
6. <span data-ttu-id="35cee-183">V hello **Editor zdroje ADO.NET**, v hello **název hello tabulky nebo zobrazení hello** seznamu, vyberte hello **Sales.SalesOrderDetail** tabulky.</span><span class="sxs-lookup"><span data-stu-id="35cee-183">In hello **ADO.NET Source Editor**, in hello **Name of hello table or hello view** list, select hello **Sales.SalesOrderDetail** table.</span></span>
   
    ![][07]
7. <span data-ttu-id="35cee-184">Klikněte na tlačítko **Preview** toosee hello prvních 200 řádků dat ve zdrojové tabulce hello v hello **náhled výsledků dotazu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35cee-184">Click **Preview** toosee hello first 200 rows of data in hello source table in hello **Preview Query Results** dialog box.</span></span>
   
    ![][08]
8. <span data-ttu-id="35cee-185">V hello **náhled výsledků dotazu** dialogové okno, klikněte na tlačítko **Zavřít** tooreturn toohello **Editor zdroje ADO.NET**.</span><span class="sxs-lookup"><span data-stu-id="35cee-185">In hello **Preview Query Results** dialog box, click **Close** tooreturn toohello **ADO.NET Source Editor**.</span></span>
9. <span data-ttu-id="35cee-186">V hello **Editor zdroje ADO.NET**, klikněte na tlačítko **OK** toofinish konfiguraci zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-186">In hello **ADO.NET Source Editor**, click **OK** toofinish configuring hello data source.</span></span>

## <a name="step-4-connect-hello-source-adapter-toohello-destination-adapter"></a><span data-ttu-id="35cee-187">Krok 4: Připojení hello zdroj adaptér toohello cílový adaptér</span><span class="sxs-lookup"><span data-stu-id="35cee-187">Step 4: Connect hello source adapter toohello destination adapter</span></span>
1. <span data-ttu-id="35cee-188">Vyberte zdroj adaptér hello hello návrhové ploše.</span><span class="sxs-lookup"><span data-stu-id="35cee-188">Select hello source adapter on hello design surface.</span></span>
2. <span data-ttu-id="35cee-189">Vyberte hello blue šipku, která rozšiřuje z adaptéru zdroj hello a přetáhněte ji toohello cílové editor, dokud ho přichytí.</span><span class="sxs-lookup"><span data-stu-id="35cee-189">Select hello blue arrow that extends from hello source adapter and drag it toohello destination editor until it snaps into place.</span></span>
   
    ![][10]
   
    <span data-ttu-id="35cee-190">V typické SSIS balíčku použít několik dalších součástí ze hello sada nástrojů služby SSIS mezi hello zdrojové a cílové toorestructure hello, transformace a čistí data při průchodu hello SSIS datového toku.</span><span class="sxs-lookup"><span data-stu-id="35cee-190">In a typical SSIS package, you use a number of other components from hello SSIS Toolbox in between hello source and hello destination toorestructure, transform, and cleanse your data as it passes through hello SSIS data flow.</span></span> <span data-ttu-id="35cee-191">tookeep co nejjednodušší například jsme se chcete připojit hello zdroje přímo toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="35cee-191">tookeep this example as simple as possible, we’re connecting hello source directly toohello destination.</span></span>

## <a name="step-5-configure-hello-destination-adapter"></a><span data-ttu-id="35cee-192">Krok 5: Konfigurace hello cílový adaptér</span><span class="sxs-lookup"><span data-stu-id="35cee-192">Step 5: Configure hello destination adapter</span></span>
1. <span data-ttu-id="35cee-193">Klikněte dvakrát na hello cílový adaptér tooopen hello **ADO.NET cílové Editor**.</span><span class="sxs-lookup"><span data-stu-id="35cee-193">Double-click hello destination adapter tooopen hello **ADO.NET Destination Editor**.</span></span>
   
    ![][11]
2. <span data-ttu-id="35cee-194">Na hello **Správce připojení** kartě hello **ADO.NET cílové Editor**, klikněte na tlačítko hello **nový** tlačítko Další toohello **Správce připojení**seznamu tooopen hello **nakonfigurovat správce připojení ADO.NET** dialogové okno a vytvořte nastavení připojení pro databázi Azure SQL Data Warehouse hello, do kterého v tomto kurzu načte data.</span><span class="sxs-lookup"><span data-stu-id="35cee-194">On hello **Connection Manager** tab of hello **ADO.NET Destination Editor**, click hello **New** button next toohello **Connection manager** list tooopen hello **Configure ADO.NET Connection Manager** dialog box and create connection settings for hello Azure SQL Data Warehouse database into which this tutorial loads data.</span></span>
3. <span data-ttu-id="35cee-195">V hello **nakonfigurovat správce připojení ADO.NET** dialogové okno pole, klikněte na tlačítko hello **nový** tlačítko tooopen hello **Správce připojení** dialogové okno a vytvořte nové datové připojení.</span><span class="sxs-lookup"><span data-stu-id="35cee-195">In hello **Configure ADO.NET Connection Manager** dialog box, click hello **New** button tooopen hello **Connection Manager** dialog box and create a new data connection.</span></span>
4. <span data-ttu-id="35cee-196">V hello **Správce připojení** dialogové okno pole, hello následující věci.</span><span class="sxs-lookup"><span data-stu-id="35cee-196">In hello **Connection Manager** dialog box, do hello following things.</span></span>
   1. <span data-ttu-id="35cee-197">Pro **zprostředkovatele**, vyberte hello zprostředkovatele dat SqlClient.</span><span class="sxs-lookup"><span data-stu-id="35cee-197">For **Provider**, select hello SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="35cee-198">Pro **název serveru**, zadejte název hello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-198">For **Server name**, enter hello SQL Data Warehouse name.</span></span>
   3. <span data-ttu-id="35cee-199">V hello **protokolu na serveru toohello** vyberte **ověřování použít SQL Server** a zadejte informace o ověřování.</span><span class="sxs-lookup"><span data-stu-id="35cee-199">In hello **Log on toohello server** section, select **Use SQL Server authentication** and enter authentication information.</span></span>
   4. <span data-ttu-id="35cee-200">V hello **Connect tooa databáze** vyberte existující databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-200">In hello **Connect tooa database** section, select an existing SQL Data Warehouse database.</span></span>
   5. <span data-ttu-id="35cee-201">Klikněte na tlačítko **testovací připojení**.</span><span class="sxs-lookup"><span data-stu-id="35cee-201">Click **Test Connection**.</span></span>
   6. <span data-ttu-id="35cee-202">V hello dialogové okno sestav hello výsledky testu hello připojení, klikněte na **OK** tooreturn toohello **Správce připojení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35cee-202">In hello dialog box that reports hello results of hello connection test, click **OK** tooreturn toohello **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="35cee-203">V hello **Správce připojení** dialogové okno, klikněte na tlačítko **OK** tooreturn toohello **nakonfigurovat správce připojení ADO.NET** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35cee-203">In hello **Connection Manager** dialog box, click **OK** tooreturn toohello **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="35cee-204">V hello **nakonfigurovat správce připojení ADO.NET** dialogové okno, klikněte na tlačítko **OK** tooreturn toohello **ADO.NET cílové Editor**.</span><span class="sxs-lookup"><span data-stu-id="35cee-204">In hello **Configure ADO.NET Connection Manager** dialog box, click **OK** tooreturn toohello **ADO.NET Destination Editor**.</span></span>
6. <span data-ttu-id="35cee-205">V hello **ADO.NET cílové Editor**, klikněte na tlačítko **nový** další toohello **použití tabulky nebo zobrazení** seznamu tooopen hello **Create Table** dialogové okno toocreate nové cílové tabulky se seznam sloupců, která odpovídá hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="35cee-205">In hello **ADO.NET Destination Editor**, click **New** next toohello **Use a table or view** list tooopen hello **Create Table** dialog box toocreate a new destination table with a column list that matches hello source table.</span></span>
   
    ![][12a]
7. <span data-ttu-id="35cee-206">V hello **Create Table** dialogové okno pole, hello následující věci.</span><span class="sxs-lookup"><span data-stu-id="35cee-206">In hello **Create Table** dialog box, do hello following things.</span></span>
   
   1. <span data-ttu-id="35cee-207">Změňte název hello hello cílové tabulky příliš**podrobnosti prodejní objednávky**.</span><span class="sxs-lookup"><span data-stu-id="35cee-207">Change hello name of hello destination table too**SalesOrderDetail**.</span></span>
   2. <span data-ttu-id="35cee-208">Odebrat hello **rowguid** sloupce.</span><span class="sxs-lookup"><span data-stu-id="35cee-208">Remove hello **rowguid** column.</span></span> <span data-ttu-id="35cee-209">Hello **uniqueidentifier** datový typ není podporován v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-209">hello **uniqueidentifier** data type is not supported in SQL Data Warehouse.</span></span>
   3. <span data-ttu-id="35cee-210">Změnit datový typ hello hello **LineTotal** sloupec příliš**peníze**.</span><span class="sxs-lookup"><span data-stu-id="35cee-210">Change hello data type of hello **LineTotal** column too**money**.</span></span> <span data-ttu-id="35cee-211">Hello **decimal** datový typ není podporován v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-211">hello **decimal** data type is not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="35cee-212">Informace o podporované datové typy najdete v tématu [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="35cee-212">For info about supported data types, see [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span></span>
      
       ![][12b]
   4. <span data-ttu-id="35cee-213">Klikněte na tlačítko **OK** toocreate hello tabulky a vraťte se toohello **ADO.NET cílové Editor**.</span><span class="sxs-lookup"><span data-stu-id="35cee-213">Click **OK** toocreate hello table and return toohello **ADO.NET Destination Editor**.</span></span>
8. <span data-ttu-id="35cee-214">V hello **ADO.NET cílové Editor**, vyberte hello **mapování** kartě toosee jak sloupce ve zdroji hello jsou namapované toocolumns v cílovém umístění hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-214">In hello **ADO.NET Destination Editor**, select hello **Mappings** tab toosee how columns in hello source are mapped toocolumns in hello destination.</span></span>
   
    ![][13]
9. <span data-ttu-id="35cee-215">Klikněte na tlačítko **OK** toofinish konfiguraci zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-215">Click **OK** toofinish configuring hello data source.</span></span>

## <a name="step-6-run-hello-package-tooload-hello-data"></a><span data-ttu-id="35cee-216">Krok 6: Spuštění data hello tooload balíčku hello</span><span class="sxs-lookup"><span data-stu-id="35cee-216">Step 6: Run hello package tooload hello data</span></span>
<span data-ttu-id="35cee-217">Spuštění hello balíček kliknutím hello **spustit** tlačítka na panelu nástrojů hello nebo výběrem jedné z hello **spustit** možnosti na hello **ladění** nabídky.</span><span class="sxs-lookup"><span data-stu-id="35cee-217">Run hello package by clicking hello **Start** button on hello toolbar or by selecting one of hello **Run** options on hello **Debug** menu.</span></span>

<span data-ttu-id="35cee-218">Jako balíček hello začne toorun, zobrazí žlutý souborů Wheel roztočený tooindicate aktivity, jakož i hello počet řádků, pokud zpracovat.</span><span class="sxs-lookup"><span data-stu-id="35cee-218">As hello package begins toorun, you see yellow spinning wheels tooindicate activity as well as hello number of rows processed so far.</span></span>

![][14]

<span data-ttu-id="35cee-219">Po dokončení spuštění balíčku hello je najdete v části úspěch tooindicate zelené značky zaškrtnutí a také hello celkový počet řádků dat načtených z hello zdroj toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="35cee-219">When hello package has finished running, you see green check marks tooindicate success as well as hello total number of rows of data loaded from hello source toohello destination.</span></span>

![][15]

<span data-ttu-id="35cee-220">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="35cee-220">Congratulations!</span></span> <span data-ttu-id="35cee-221">Úspěšně jste použili službu SQL Server Integration Services tooload data do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="35cee-221">You’ve successfully used SQL Server Integration Services tooload data into Azure SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35cee-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="35cee-222">Next steps</span></span>
* <span data-ttu-id="35cee-223">Další informace o hello SSIS datového toku.</span><span class="sxs-lookup"><span data-stu-id="35cee-223">Learn more about hello SSIS data flow.</span></span> <span data-ttu-id="35cee-224">Začněte zde: [toku dat][Data Flow].</span><span class="sxs-lookup"><span data-stu-id="35cee-224">Start here: [Data Flow][Data Flow].</span></span>
* <span data-ttu-id="35cee-225">Zjistěte, jak toodebug a řešení potíží s vaše právo balíčky v prostředí návrhu hello.</span><span class="sxs-lookup"><span data-stu-id="35cee-225">Learn how toodebug and troubleshoot your packages right in hello design environment.</span></span> <span data-ttu-id="35cee-226">Začněte zde: [řešení potíží s nástrojů pro vývoj balíček][Troubleshooting Tools for Package Development].</span><span class="sxs-lookup"><span data-stu-id="35cee-226">Start here: [Troubleshooting Tools for Package Development][Troubleshooting Tools for Package Development].</span></span>
* <span data-ttu-id="35cee-227">Zjistěte, jak toodeploy vaší služby SSIS projekty a balíčky tooIntegration služby serveru nebo tooanother umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="35cee-227">Learn how toodeploy your SSIS projects and packages tooIntegration Services Server or tooanother storage location.</span></span> <span data-ttu-id="35cee-228">Začněte zde: [projekty nasazení a balíčky][Deployment of Projects and Packages].</span><span class="sxs-lookup"><span data-stu-id="35cee-228">Start here: [Deployment of Projects and Packages][Deployment of Projects and Packages].</span></span>

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
