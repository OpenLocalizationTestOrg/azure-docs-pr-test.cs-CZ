---
title: "Začínáme s dotazy mezidatabázové (vertikální dělení) | Microsoft Docs"
description: "pomocí dotazu elastické databáze s svisle na oddíly databáze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: e5b44b10-c432-4f96-b20e-08615ff4d5dd
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: torsteng
ms.openlocfilehash: 17158c4960e9ba9251524659c90af9aec1316774
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a><span data-ttu-id="abd79-103">Začínáme s dotazy mezidatabázové (vertikální dělení) (preview)</span><span class="sxs-lookup"><span data-stu-id="abd79-103">Get started with cross-database queries (vertical partitioning) (preview)</span></span>
<span data-ttu-id="abd79-104">Dotaz elastické databáze (preview) pro databázi SQL Azure umožňuje spouštět dotazy T-SQL, které jsou rozmístěny v několika databází pomocí jednoho připojení bodu.</span><span class="sxs-lookup"><span data-stu-id="abd79-104">Elastic database query (preview) for Azure SQL Database allows you to run T-SQL queries that span multiple databases using a single connection point.</span></span> <span data-ttu-id="abd79-105">Toto téma se týká [svisle na oddíly databáze](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="abd79-105">This topic applies to [vertically partitioned databases](sql-database-elastic-query-vertical-partitioning.md).</span></span>  

<span data-ttu-id="abd79-106">Po dokončení bude: Zjistěte, jak konfigurovat a používat Azure SQL Database provádět dotazy, které jsou rozmístěny v několika související databází.</span><span class="sxs-lookup"><span data-stu-id="abd79-106">When completed, you will: learn how to configure and use an Azure SQL Database to perform queries that span multiple related databases.</span></span> 

<span data-ttu-id="abd79-107">Další informace o funkci dotazu elastické databáze, najdete v tématu [přehled dotazu elastické databáze Azure SQL Database](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="abd79-107">For more information about the elastic database query feature, please see  [Azure SQL Database elastic database query overview](sql-database-elastic-query-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="abd79-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="abd79-108">Prerequisites</span></span>

<span data-ttu-id="abd79-109">Musíte mít oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="abd79-109">You must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="abd79-110">Toto oprávnění je součástí oprávnění ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="abd79-110">This permission is included with the ALTER DATABASE permission.</span></span> <span data-ttu-id="abd79-111">K odkazování na podkladový zdroj dat jsou potřeba oprávnění ALTER ANY EXTERNAL DATA SOURCE.</span><span class="sxs-lookup"><span data-stu-id="abd79-111">ALTER ANY EXTERNAL DATA SOURCE permissions are needed to refer to the underlying data source.</span></span>

## <a name="create-the-sample-databases"></a><span data-ttu-id="abd79-112">Vytvoření ukázkové databáze</span><span class="sxs-lookup"><span data-stu-id="abd79-112">Create the sample databases</span></span>
<span data-ttu-id="abd79-113">Abyste mohli začít, potřebujeme vytvořit dvě databáze, **zákazníci** a **objednávky**, buď na stejný nebo jiný logického serveru.</span><span class="sxs-lookup"><span data-stu-id="abd79-113">To start with, we need to create two databases, **Customers** and **Orders**, either in the same or different logical servers.</span></span>   

<span data-ttu-id="abd79-114">Spusťte tyto dotazy na **objednávky** databáze slouží k vytvoření **OrderInformation** tabulky a vstup ukázková data.</span><span class="sxs-lookup"><span data-stu-id="abd79-114">Execute the following queries on the **Orders** database to create the **OrderInformation** table and input the sample data.</span></span> 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

<span data-ttu-id="abd79-115">Nyní, spustit následující dotaz na **zákazníci** databáze slouží k vytvoření **CustomerInformation** tabulky a vstup ukázková data.</span><span class="sxs-lookup"><span data-stu-id="abd79-115">Now, execute following query on the **Customers** database to create the **CustomerInformation** table and input the sample data.</span></span> 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a><span data-ttu-id="abd79-116">Vytvoření databázových objektů</span><span class="sxs-lookup"><span data-stu-id="abd79-116">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="abd79-117">Hlavní klíč a přihlašovací údaje na obor definovaný databází</span><span class="sxs-lookup"><span data-stu-id="abd79-117">Database scoped master key and credentials</span></span>
1. <span data-ttu-id="abd79-118">Spusťte aplikaci SQL Server Management Studio nebo SQL Server Data Tools v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="abd79-118">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="abd79-119">Připojení k databázi objednávek a spuštěním následujících příkazů T-SQL:</span><span class="sxs-lookup"><span data-stu-id="abd79-119">Connect to the Orders database and execute the following T-SQL commands:</span></span>
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    <span data-ttu-id="abd79-120">"Username" a "password" by měl být uživatelské jméno a heslo použité k přihlášení do databáze zákazníků.</span><span class="sxs-lookup"><span data-stu-id="abd79-120">The "username" and "password" should be the username and password used to login into the Customers database.</span></span>
    <span data-ttu-id="abd79-121">Ověřování pomocí služby Azure Active Directory s elastické dotazy není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="abd79-121">Authentication using Azure Active Directory with elastic queries is not currently supported.</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="abd79-122">Externích zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="abd79-122">External data sources</span></span>
<span data-ttu-id="abd79-123">Pokud chcete vytvořit externího zdroje dat, spusťte následující příkaz v databázi objednávky:</span><span class="sxs-lookup"><span data-stu-id="abd79-123">To create an external data source, execute the following command on the Orders database:</span></span> 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a><span data-ttu-id="abd79-124">Externí tabulky</span><span class="sxs-lookup"><span data-stu-id="abd79-124">External tables</span></span>
<span data-ttu-id="abd79-125">Vytvoření externí tabulky v databázi objednávky, který odpovídá definici tabulky CustomerInformation:</span><span class="sxs-lookup"><span data-stu-id="abd79-125">Create an external table on the Orders database, which matches the definition of the CustomerInformation table:</span></span>

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="abd79-126">Spuštění ukázkového dotazu T-SQL elastické databáze</span><span class="sxs-lookup"><span data-stu-id="abd79-126">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="abd79-127">Jakmile definujete zdroj externích dat a externí tabulky teď můžete T-SQL pro dotaz na externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="abd79-127">Once you have defined your external data source and your external tables you can now use T-SQL to query your external tables.</span></span> <span data-ttu-id="abd79-128">Spusťte tento dotaz na databázi objednávky:</span><span class="sxs-lookup"><span data-stu-id="abd79-128">Execute this query on the Orders database:</span></span> 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a><span data-ttu-id="abd79-129">Náklady</span><span class="sxs-lookup"><span data-stu-id="abd79-129">Cost</span></span>
<span data-ttu-id="abd79-130">V současné době funkce dotazu elastické databáze je zahrnut do náklady na vaší databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="abd79-130">Currently, the elastic database query feature is included into the cost of your Azure SQL Database.</span></span>  

<span data-ttu-id="abd79-131">Informace o cenách najdete v části [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database).</span><span class="sxs-lookup"><span data-stu-id="abd79-131">For pricing information see [SQL Database Pricing](https://azure.microsoft.com/pricing/details/sql-database).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="abd79-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abd79-132">Next steps</span></span>

* <span data-ttu-id="abd79-133">Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="abd79-133">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="abd79-134">Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="abd79-134">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="abd79-135">Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="abd79-135">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="abd79-136">Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="abd79-136">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="abd79-137">V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="abd79-137">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>