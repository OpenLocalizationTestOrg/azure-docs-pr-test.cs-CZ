---
title: "aaaReport napříč instancemi cloudu databází (vodorovné rozdělení do oddílů) | Microsoft Docs"
description: "jak toouse mezi databáze databázové dotazy"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: c81ef5e3-41e9-4fd2-8631-868f2e168147
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: mlandzic
ms.openlocfilehash: e34f398f8d408cffd91a70fc2cfbda73daec3550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="64952-103">Sestavy napříč instancemi cloudu databází (preview)</span><span class="sxs-lookup"><span data-stu-id="64952-103">Report across scaled-out cloud databases (preview)</span></span>
<span data-ttu-id="64952-104">Můžete vytvořit sestavy z několika databází Azure SQL z bodu pomocí jednoho připojení [elastické dotazu](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="64952-104">You can create reports from multiple Azure SQL databases from a single connection point using an [elastic query](sql-database-elastic-query-overview.md).</span></span> <span data-ttu-id="64952-105">Hello databáze musí mít oddíly vodorovně (také označované jako "horizontálně dělené").</span><span class="sxs-lookup"><span data-stu-id="64952-105">hello databases must be horizontally partitioned (also known as "sharded").</span></span>

<span data-ttu-id="64952-106">Pokud máte existující databázi, přečtěte si téma [migraci stávající databáze, databáze na více systémů tooscaled](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="64952-106">If you have an existing database, see [Migrating existing databases tooscaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>

<span data-ttu-id="64952-107">objekty SQL hello toounderstand potřeby tooquery najdete v tématu [dotazu mezi databázemi vodorovně oddílů](sql-database-elastic-query-horizontal-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="64952-107">toounderstand hello SQL objects needed tooquery, see [Query across horizontally partitioned databases](sql-database-elastic-query-horizontal-partitioning.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64952-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="64952-108">Prerequisites</span></span>
<span data-ttu-id="64952-109">Stažení a spuštění hello [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="64952-109">Download and run hello [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a><span data-ttu-id="64952-110">Vytvoření mapy horizontálního oddílu manager pomocí hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="64952-110">Create a shard map manager using hello sample app</span></span>
<span data-ttu-id="64952-111">Zde vytvoříte mapu horizontálního oddílu manager spolu s několika horizontálních oddílů, za nímž následuje vložení dat do hello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="64952-111">Here you will create a shard map manager along with several shards, followed by insertion of data into hello shards.</span></span> <span data-ttu-id="64952-112">Pokud jste tooalready nastavili horizontálních oddílů s horizontálně dělená data v nich, můžete přeskočit následující kroky hello a přesunout toohello další části.</span><span class="sxs-lookup"><span data-stu-id="64952-112">If you happen tooalready have shards setup with sharded data in them, you can skip hello following steps and move toohello next section.</span></span>

1. <span data-ttu-id="64952-113">Sestavení a spuštění hello **Začínáme s nástroje elastické databáze** ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="64952-113">Build and run hello **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="64952-114">Postupujte podle kroků hello až do kroku 7 v části hello [stažení a spuštění ukázkové aplikace hello](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="64952-114">Follow hello steps until step 7 in hello section [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="64952-115">Na konci hello tohoto kroku 7 zobrazí se hello následující příkazový řádek:</span><span class="sxs-lookup"><span data-stu-id="64952-115">At hello end of Step 7, you will see hello following command prompt:</span></span>

    ![příkazový řádek][1]
2. <span data-ttu-id="64952-117">V příkazovém okně hello, zadejte "1" a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="64952-117">In hello command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="64952-118">To vytvoří hello horizontálního oddílu mapa správce a přidá serverové toohello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="64952-118">This creates hello shard map manager, and adds two shards toohello server.</span></span> <span data-ttu-id="64952-119">Potom zadejte "3" a stiskněte klávesu **Enter**; zopakuje akci hello čtyřikrát.</span><span class="sxs-lookup"><span data-stu-id="64952-119">Then type "3" and press **Enter**; repeat hello action four times.</span></span> <span data-ttu-id="64952-120">Vloží řádky ukázková data ve vašem horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="64952-120">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="64952-121">Hello [portál Azure](https://portal.azure.com) by měl zobrazit tři nové databáze na serveru:</span><span class="sxs-lookup"><span data-stu-id="64952-121">hello [Azure portal](https://portal.azure.com) should show three new databases in your server:</span></span>

   ![Visual Studio potvrzení][2]

   <span data-ttu-id="64952-123">Mezidatabázové dotazy v tomto okamžiku jsou podporovány prostřednictvím knihovny klienta hello elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="64952-123">At this point, cross-database queries are supported through hello Elastic Database client libraries.</span></span> <span data-ttu-id="64952-124">Například v příkazovém okně hello použijte možnost 4.</span><span class="sxs-lookup"><span data-stu-id="64952-124">For example, use option 4 in hello command window.</span></span> <span data-ttu-id="64952-125">Hello výsledků dotazu víc horizontálních jsou vždy **UNION ALL** výsledků hello ze všech horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="64952-125">hello results from a multi-shard query are always a **UNION ALL** of hello results from all shards.</span></span>

   <span data-ttu-id="64952-126">V další části hello jsme vytvořit koncový bod ukázkové databáze podporující bohatší dotazování hello dat napříč horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="64952-126">In hello next section, we create a sample database endpoint that supports richer querying of hello data across shards.</span></span>

## <a name="create-an-elastic-query-database"></a><span data-ttu-id="64952-127">Vytvoření dotazu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="64952-127">Create an elastic query database</span></span>
1. <span data-ttu-id="64952-128">Otevřete hello [portál Azure](https://portal.azure.com) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="64952-128">Open hello [Azure portal](https://portal.azure.com) and log in.</span></span>
2. <span data-ttu-id="64952-129">Vytvořit novou databázi Azure SQL v hello stejný server jako vašeho nastavení horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="64952-129">Create a new Azure SQL database in hello same server as your shard setup.</span></span> <span data-ttu-id="64952-130">Název databáze hello "ElasticDBQuery."</span><span class="sxs-lookup"><span data-stu-id="64952-130">Name hello database "ElasticDBQuery."</span></span>

    ![Portál Azure a cenovou úroveň][3]

    > [!NOTE]
    > <span data-ttu-id="64952-132">můžete použít existující databázi.</span><span class="sxs-lookup"><span data-stu-id="64952-132">you can use an existing database.</span></span> <span data-ttu-id="64952-133">Pokud můžete tak učinit, nesmí být jedna z hello horizontálních oddílů, které chcete tooexecute své dotazy na.</span><span class="sxs-lookup"><span data-stu-id="64952-133">If you can do so, it must not be one of hello shards that you would like tooexecute your queries on.</span></span> <span data-ttu-id="64952-134">Tato databáze se použije pro vytvoření hello objekty metadata pro dotaz elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="64952-134">This database will be used for creating hello metadata objects for an elastic database query.</span></span>
    >

## <a name="create-database-objects"></a><span data-ttu-id="64952-135">Vytvoření databázových objektů</span><span class="sxs-lookup"><span data-stu-id="64952-135">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="64952-136">Hlavní klíč databáze obor a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="64952-136">Database-scoped master key and credentials</span></span>
<span data-ttu-id="64952-137">Toto jsou použité tooconnect toohello horizontálního oddílu mapa správce a hello horizontálních oddílů:</span><span class="sxs-lookup"><span data-stu-id="64952-137">These are used tooconnect toohello shard map manager and hello shards:</span></span>

1. <span data-ttu-id="64952-138">Spusťte aplikaci SQL Server Management Studio nebo SQL Server Data Tools v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="64952-138">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="64952-139">Připojit databáze tooElasticDBQuery a spusťte následující příkazy T-SQL hello:</span><span class="sxs-lookup"><span data-stu-id="64952-139">Connect tooElasticDBQuery database and execute hello following T-SQL commands:</span></span>

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    <span data-ttu-id="64952-140">"username" a "password" by měl být hello stejné jako informace o přihlášení se používají v kroku 6 v [stažení a spuštění ukázkové aplikace hello](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) v [Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="64952-140">"username" and "password" should be hello same as login information used in step 6 of [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) in [Getting started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="64952-141">Externích zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="64952-141">External data sources</span></span>
<span data-ttu-id="64952-142">toocreate externího zdroje dat, spusťte následující příkaz v databázi ElasticDBQuery hello hello:</span><span class="sxs-lookup"><span data-stu-id="64952-142">toocreate an external data source, execute hello following command on hello ElasticDBQuery database:</span></span>

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 <span data-ttu-id="64952-143">"CustomerIDShardMap" je název hello hello horizontálního oddílu mapy, pokud jste vytvořili hello horizontálního oddílu mapy a mapovat horizontálního oddílu manager pomocí Ukázka nástroje elastické databáze hello.</span><span class="sxs-lookup"><span data-stu-id="64952-143">"CustomerIDShardMap" is hello name of hello shard map, if you created hello shard map and shard map manager using hello elastic database tools sample.</span></span> <span data-ttu-id="64952-144">Ale pokud jste použili vlastní instalace pro tuto ukázku, pak jej by měl být hello horizontálního oddílu mapy název, které jste zvolili v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="64952-144">However, if you used your custom setup for this sample, then it should be hello shard map name you chose in your application.</span></span>

### <a name="external-tables"></a><span data-ttu-id="64952-145">Externí tabulky</span><span class="sxs-lookup"><span data-stu-id="64952-145">External tables</span></span>
<span data-ttu-id="64952-146">Vytvořte externí tabulku, která odpovídá tabulku zákazníků hello na horizontálních oddílů hello tak, že spustíte následující příkaz v databázi ElasticDBQuery hello:</span><span class="sxs-lookup"><span data-stu-id="64952-146">Create an external table that matches hello Customers table on hello shards by executing hello following command on ElasticDBQuery database:</span></span>

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="64952-147">Spuštění ukázkového dotazu T-SQL elastické databáze</span><span class="sxs-lookup"><span data-stu-id="64952-147">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="64952-148">Jakmile definujete zdroj externích dat a externí tabulky teď můžete použít úplnou T-SQL na externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="64952-148">Once you have defined your external data source and your external tables you can now use full T-SQL over your external tables.</span></span>

<span data-ttu-id="64952-149">Spusťte tento dotaz na databázi ElasticDBQuery hello:</span><span class="sxs-lookup"><span data-stu-id="64952-149">Execute this query on hello ElasticDBQuery database:</span></span>

    select count(CustomerId) from [dbo].[Customers]

<span data-ttu-id="64952-150">Si všimnete, že dotaz hello agreguje výsledky ze všech hello horizontálních oddílů a poskytuje následující výstup hello:</span><span class="sxs-lookup"><span data-stu-id="64952-150">You will notice that hello query aggregates results from all hello shards and gives hello following output:</span></span>

![Podrobnosti o výstupu][4]

## <a name="import-elastic-database-query-results-tooexcel"></a><span data-ttu-id="64952-152">Import tooExcel výsledky dotazu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="64952-152">Import elastic database query results tooExcel</span></span>
 <span data-ttu-id="64952-153">Můžete importovat hello výsledky ze souboru aplikace Excel tooan dotazu.</span><span class="sxs-lookup"><span data-stu-id="64952-153">You can import hello results from of a query tooan Excel file.</span></span>

1. <span data-ttu-id="64952-154">Spusťte aplikaci Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="64952-154">Launch Excel 2013.</span></span>
2. <span data-ttu-id="64952-155">Přejděte toohello **Data** pásu karet.</span><span class="sxs-lookup"><span data-stu-id="64952-155">Navigate toohello **Data** ribbon.</span></span>
3. <span data-ttu-id="64952-156">Klikněte na tlačítko **z jiných zdrojů** a klikněte na tlačítko **z SQL serveru**.</span><span class="sxs-lookup"><span data-stu-id="64952-156">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Importu pro aplikaci Excel z jiných zdrojů][5]
4. <span data-ttu-id="64952-158">V hello **Průvodce datovým připojením** zadejte název a přihlašovací údaje serveru hello.</span><span class="sxs-lookup"><span data-stu-id="64952-158">In hello **Data Connection Wizard** type hello server name and login credentials.</span></span> <span data-ttu-id="64952-159">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="64952-159">Then click **Next**.</span></span>
5. <span data-ttu-id="64952-160">V dialogovém okně hello **hello vyberte databázi, která obsahuje hello data, která chcete**, vyberte hello **ElasticDBQuery** databáze.</span><span class="sxs-lookup"><span data-stu-id="64952-160">In hello dialog box **Select hello database that contains hello data you want**, select hello **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="64952-161">Vyberte hello **zákazníci** tabulky v zobrazení seznamu hello a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="64952-161">Select hello **Customers** table in hello list view and click **Next**.</span></span> <span data-ttu-id="64952-162">Pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="64952-162">Then click **Finish**.</span></span>
7. <span data-ttu-id="64952-163">V hello **importovat Data** formuláři v části **vyberte požadovaný způsob tooview tato data v sešitu**, vyberte **tabulky** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="64952-163">In hello **Import Data** form, under **Select how you want tooview this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="64952-164">Všechny řádky z hello **zákazníci** tabulky, uložené v různých horizontálních oddílů naplnit hello Excelovém listu.</span><span class="sxs-lookup"><span data-stu-id="64952-164">All hello rows from **Customers** table, stored in different shards populate hello Excel sheet.</span></span>

<span data-ttu-id="64952-165">Teď můžete použít funkce vizualizace výkonné dat v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="64952-165">You can now use Excel’s powerful data visualization functions.</span></span> <span data-ttu-id="64952-166">Můžete použít hello připojovací řetězec s názvem serveru, název databáze a pověření tooconnect vaše data a BI integrace nástrojů toohello elastické dotaz do databáze.</span><span class="sxs-lookup"><span data-stu-id="64952-166">You can use hello connection string with your server name, database name and credentials tooconnect your BI and data integration tools toohello elastic query database.</span></span> <span data-ttu-id="64952-167">Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="64952-167">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="64952-168">Může odkazovat toohello elastické dotaz do databáze a externí tabulky stejně jako všechny ostatní databáze systému SQL Server a zda byste připojili toowith vaše nástroje tabulek systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="64952-168">You can refer toohello elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect toowith your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="64952-169">Náklady</span><span class="sxs-lookup"><span data-stu-id="64952-169">Cost</span></span>
<span data-ttu-id="64952-170">Není k dispozici pro použití funkce hello elastické databáze dotazu bez dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="64952-170">There is no additional charge for using hello Elastic Database Query feature.</span></span>

<span data-ttu-id="64952-171">Informace o cenách najdete v části [podrobnosti o cenách na SQL databázi](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="64952-171">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="64952-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64952-172">Next steps</span></span>

* <span data-ttu-id="64952-173">Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="64952-173">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="64952-174">Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="64952-174">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="64952-175">Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="64952-175">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="64952-176">Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="64952-176">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="64952-177">V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="64952-177">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
