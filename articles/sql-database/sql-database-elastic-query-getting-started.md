---
title: "Sestava napříč instancemi cloudu databází (vodorovné rozdělení do oddílů) | Microsoft Docs"
description: "jak používat křížové databáze databázové dotazy"
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
ms.openlocfilehash: 8eb56d44c3a261f6325d4fc91f169d09bf108160
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="6a284-103">Sestavy napříč instancemi cloudu databází (preview)</span><span class="sxs-lookup"><span data-stu-id="6a284-103">Report across scaled-out cloud databases (preview)</span></span>
<span data-ttu-id="6a284-104">Můžete vytvořit sestavy z několika databází Azure SQL z bodu pomocí jednoho připojení [elastické dotazu](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a284-104">You can create reports from multiple Azure SQL databases from a single connection point using an [elastic query](sql-database-elastic-query-overview.md).</span></span> <span data-ttu-id="6a284-105">Databáze musí mít oddíly vodorovně (také označované jako "horizontálně dělené").</span><span class="sxs-lookup"><span data-stu-id="6a284-105">The databases must be horizontally partitioned (also known as "sharded").</span></span>

<span data-ttu-id="6a284-106">Pokud máte existující databázi, přečtěte si téma [migrace existujících databází do databází upraveným](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="6a284-106">If you have an existing database, see [Migrating existing databases to scaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>

<span data-ttu-id="6a284-107">Pochopit objektů SQL potřebné k dotazování, najdete v části [dotazu mezi databázemi vodorovně oddílů](sql-database-elastic-query-horizontal-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="6a284-107">To understand the SQL objects needed to query, see [Query across horizontally partitioned databases](sql-database-elastic-query-horizontal-partitioning.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a284-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a284-108">Prerequisites</span></span>
<span data-ttu-id="6a284-109">Stažení a spuštění [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6a284-109">Download and run the [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-the-sample-app"></a><span data-ttu-id="6a284-110">Vytvoření horizontálního oddílu mapy manager pomocí ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="6a284-110">Create a shard map manager using the sample app</span></span>
<span data-ttu-id="6a284-111">Zde vytvoříte mapu horizontálního oddílu manager spolu s několika horizontálních oddílů, za nímž následuje vložení dat do horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="6a284-111">Here you will create a shard map manager along with several shards, followed by insertion of data into the shards.</span></span> <span data-ttu-id="6a284-112">Pokud jste již má instalační program horizontálních oddílů s horizontálně dělená data v nich, můžete přeskočit následující kroky a přesunout k další části.</span><span class="sxs-lookup"><span data-stu-id="6a284-112">If you happen to already have shards setup with sharded data in them, you can skip the following steps and move to the next section.</span></span>

1. <span data-ttu-id="6a284-113">Sestavení a spuštění **Začínáme s nástroje elastické databáze** ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a284-113">Build and run the **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="6a284-114">Postupujte podle pokynů až do kroku 7 v části [stažení a spuštění ukázkové aplikace](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="6a284-114">Follow the steps until step 7 in the section [Download and run the sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="6a284-115">Na konci tohoto kroku 7 zobrazí se následující příkazový řádek:</span><span class="sxs-lookup"><span data-stu-id="6a284-115">At the end of Step 7, you will see the following command prompt:</span></span>

    ![příkazový řádek][1]
2. <span data-ttu-id="6a284-117">V okně příkazového řádku zadejte "1" a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="6a284-117">In the command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="6a284-118">To vytvoří horizontálního oddílu správce mapy a přidá dva horizontálních oddílů server.</span><span class="sxs-lookup"><span data-stu-id="6a284-118">This creates the shard map manager, and adds two shards to the server.</span></span> <span data-ttu-id="6a284-119">Potom zadejte "3" a stiskněte klávesu **Enter**; čtyřikrát akci opakujte.</span><span class="sxs-lookup"><span data-stu-id="6a284-119">Then type "3" and press **Enter**; repeat the action four times.</span></span> <span data-ttu-id="6a284-120">Vloží řádky ukázková data ve vašem horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="6a284-120">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="6a284-121">[Portál Azure](https://portal.azure.com) by měl zobrazit tři nové databáze na serveru:</span><span class="sxs-lookup"><span data-stu-id="6a284-121">The [Azure portal](https://portal.azure.com) should show three new databases in your server:</span></span>

   ![Visual Studio potvrzení][2]

   <span data-ttu-id="6a284-123">Mezidatabázové dotazy v tomto okamžiku jsou podporovány prostřednictvím knihovny klienta elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="6a284-123">At this point, cross-database queries are supported through the Elastic Database client libraries.</span></span> <span data-ttu-id="6a284-124">Například v příkazovém okně použijte možnost 4.</span><span class="sxs-lookup"><span data-stu-id="6a284-124">For example, use option 4 in the command window.</span></span> <span data-ttu-id="6a284-125">Výsledky z dotazu víc horizontálních jsou vždy **UNION ALL** výsledků ze všech horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="6a284-125">The results from a multi-shard query are always a **UNION ALL** of the results from all shards.</span></span>

   <span data-ttu-id="6a284-126">V další části jsme vytvořit koncový bod ukázkové databáze podporující bohatší dotazování dat napříč horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="6a284-126">In the next section, we create a sample database endpoint that supports richer querying of the data across shards.</span></span>

## <a name="create-an-elastic-query-database"></a><span data-ttu-id="6a284-127">Vytvoření dotazu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="6a284-127">Create an elastic query database</span></span>
1. <span data-ttu-id="6a284-128">Otevřete [portál Azure](https://portal.azure.com) a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="6a284-128">Open the [Azure portal](https://portal.azure.com) and log in.</span></span>
2. <span data-ttu-id="6a284-129">Vytvořte novou databázi Azure SQL na stejném serveru jako vašeho nastavení horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="6a284-129">Create a new Azure SQL database in the same server as your shard setup.</span></span> <span data-ttu-id="6a284-130">Název databáze "ElasticDBQuery."</span><span class="sxs-lookup"><span data-stu-id="6a284-130">Name the database "ElasticDBQuery."</span></span>

    ![Portál Azure a cenovou úroveň][3]

    > [!NOTE]
    > <span data-ttu-id="6a284-132">můžete použít existující databázi.</span><span class="sxs-lookup"><span data-stu-id="6a284-132">you can use an existing database.</span></span> <span data-ttu-id="6a284-133">Pokud můžete tak učinit, nesmí být jeden z horizontálních oddílů, které byste chtěli provést své dotazy.</span><span class="sxs-lookup"><span data-stu-id="6a284-133">If you can do so, it must not be one of the shards that you would like to execute your queries on.</span></span> <span data-ttu-id="6a284-134">Tato databáze se použije pro vytváření objektů metadat pro dotaz elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="6a284-134">This database will be used for creating the metadata objects for an elastic database query.</span></span>
    >

## <a name="create-database-objects"></a><span data-ttu-id="6a284-135">Vytvoření databázových objektů</span><span class="sxs-lookup"><span data-stu-id="6a284-135">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="6a284-136">Hlavní klíč databáze obor a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="6a284-136">Database-scoped master key and credentials</span></span>
<span data-ttu-id="6a284-137">Ty se používají k připojení k správce mapy horizontálního oddílu a horizontálních oddílů:</span><span class="sxs-lookup"><span data-stu-id="6a284-137">These are used to connect to the shard map manager and the shards:</span></span>

1. <span data-ttu-id="6a284-138">Spusťte aplikaci SQL Server Management Studio nebo SQL Server Data Tools v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a284-138">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="6a284-139">Připojení k databázi ElasticDBQuery a spuštěním následujících příkazů T-SQL:</span><span class="sxs-lookup"><span data-stu-id="6a284-139">Connect to ElasticDBQuery database and execute the following T-SQL commands:</span></span>

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    <span data-ttu-id="6a284-140">"username" a "password" by měla být stejná jako informace o přihlášení se používají v kroku 6 v [stažení a spuštění ukázkové aplikace](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) v [Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6a284-140">"username" and "password" should be the same as login information used in step 6 of [Download and run the sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) in [Getting started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="6a284-141">Externích zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="6a284-141">External data sources</span></span>
<span data-ttu-id="6a284-142">Pokud chcete vytvořit externího zdroje dat, spusťte následující příkaz v databázi ElasticDBQuery:</span><span class="sxs-lookup"><span data-stu-id="6a284-142">To create an external data source, execute the following command on the ElasticDBQuery database:</span></span>

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 <span data-ttu-id="6a284-143">"CustomerIDShardMap" je název horizontálního oddílu mapy, pokud jste vytvořili horizontálního oddílu mapy a správce mapy horizontálního oddílu pomocí Ukázka nástroje elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="6a284-143">"CustomerIDShardMap" is the name of the shard map, if you created the shard map and shard map manager using the elastic database tools sample.</span></span> <span data-ttu-id="6a284-144">Ale pokud jste použili vlastní instalace pro tuto ukázku, pak je nutné název mapy horizontálního oddílu, které jste zvolili v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6a284-144">However, if you used your custom setup for this sample, then it should be the shard map name you chose in your application.</span></span>

### <a name="external-tables"></a><span data-ttu-id="6a284-145">Externí tabulky</span><span class="sxs-lookup"><span data-stu-id="6a284-145">External tables</span></span>
<span data-ttu-id="6a284-146">Vytvořte externí tabulku, která odpovídá tabulku zákazníků na horizontálních oddílů spuštěním následujícího příkazu na databázi ElasticDBQuery:</span><span class="sxs-lookup"><span data-stu-id="6a284-146">Create an external table that matches the Customers table on the shards by executing the following command on ElasticDBQuery database:</span></span>

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="6a284-147">Spuštění ukázkového dotazu T-SQL elastické databáze</span><span class="sxs-lookup"><span data-stu-id="6a284-147">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="6a284-148">Jakmile definujete zdroj externích dat a externí tabulky teď můžete použít úplnou T-SQL na externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="6a284-148">Once you have defined your external data source and your external tables you can now use full T-SQL over your external tables.</span></span>

<span data-ttu-id="6a284-149">Spusťte tento dotaz na databázi ElasticDBQuery:</span><span class="sxs-lookup"><span data-stu-id="6a284-149">Execute this query on the ElasticDBQuery database:</span></span>

    select count(CustomerId) from [dbo].[Customers]

<span data-ttu-id="6a284-150">Si všimnete, že dotaz agreguje výsledky ze všech horizontálních oddílů a poskytuje následující výstup:</span><span class="sxs-lookup"><span data-stu-id="6a284-150">You will notice that the query aggregates results from all the shards and gives the following output:</span></span>

![Podrobnosti o výstupu][4]

## <a name="import-elastic-database-query-results-to-excel"></a><span data-ttu-id="6a284-152">Import výsledků dotazu elastické databáze do aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="6a284-152">Import elastic database query results to Excel</span></span>
 <span data-ttu-id="6a284-153">Můžete importovat výsledky z dotazu do souboru aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="6a284-153">You can import the results from of a query to an Excel file.</span></span>

1. <span data-ttu-id="6a284-154">Spusťte aplikaci Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="6a284-154">Launch Excel 2013.</span></span>
2. <span data-ttu-id="6a284-155">Přejděte na **Data** pásu karet.</span><span class="sxs-lookup"><span data-stu-id="6a284-155">Navigate to the **Data** ribbon.</span></span>
3. <span data-ttu-id="6a284-156">Klikněte na tlačítko **z jiných zdrojů** a klikněte na tlačítko **z SQL serveru**.</span><span class="sxs-lookup"><span data-stu-id="6a284-156">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Importu pro aplikaci Excel z jiných zdrojů][5]
4. <span data-ttu-id="6a284-158">V **Průvodce datovým připojením** zadejte název a přihlašovací údaje serveru.</span><span class="sxs-lookup"><span data-stu-id="6a284-158">In the **Data Connection Wizard** type the server name and login credentials.</span></span> <span data-ttu-id="6a284-159">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6a284-159">Then click **Next**.</span></span>
5. <span data-ttu-id="6a284-160">V dialogovém okně **vyberte databáze, která obsahuje data, která chcete**, vyberte **ElasticDBQuery** databáze.</span><span class="sxs-lookup"><span data-stu-id="6a284-160">In the dialog box **Select the database that contains the data you want**, select the **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="6a284-161">Vyberte **zákazníci** tabulky v zobrazení seznamu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6a284-161">Select the **Customers** table in the list view and click **Next**.</span></span> <span data-ttu-id="6a284-162">Pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="6a284-162">Then click **Finish**.</span></span>
7. <span data-ttu-id="6a284-163">V **importovat Data** formuláři v části **vyberte, jak chcete zobrazit tato data v sešitu**, vyberte **tabulky** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a284-163">In the **Import Data** form, under **Select how you want to view this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="6a284-164">Všechny řádky z **zákazníci** tabulky, uložené v různých horizontálních oddílů naplnit listu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="6a284-164">All the rows from **Customers** table, stored in different shards populate the Excel sheet.</span></span>

<span data-ttu-id="6a284-165">Teď můžete použít funkce vizualizace výkonné dat v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="6a284-165">You can now use Excel’s powerful data visualization functions.</span></span> <span data-ttu-id="6a284-166">Připojovací řetězec s názvem serveru, názvu databáze a pověření slouží k připojení k databázi elastické dotazu vaše integrace nástrojů BI a data.</span><span class="sxs-lookup"><span data-stu-id="6a284-166">You can use the connection string with your server name, database name and credentials to connect your BI and data integration tools to the elastic query database.</span></span> <span data-ttu-id="6a284-167">Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="6a284-167">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="6a284-168">Může být elastické dotaz do databáze a externí tabulky stejně jako všechny ostatní databáze systému SQL Server a tabulek systému SQL Server, které by se připojit k vaší nástrojem.</span><span class="sxs-lookup"><span data-stu-id="6a284-168">You can refer to the elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect to with your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="6a284-169">Náklady</span><span class="sxs-lookup"><span data-stu-id="6a284-169">Cost</span></span>
<span data-ttu-id="6a284-170">Není k dispozici pro použití funkce elastické databáze dotazu bez dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="6a284-170">There is no additional charge for using the Elastic Database Query feature.</span></span>

<span data-ttu-id="6a284-171">Informace o cenách najdete v části [podrobnosti o cenách na SQL databázi](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="6a284-171">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a284-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a284-172">Next steps</span></span>

* <span data-ttu-id="6a284-173">Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a284-173">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="6a284-174">Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="6a284-174">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="6a284-175">Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="6a284-175">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="6a284-176">Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="6a284-176">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="6a284-177">V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="6a284-177">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
