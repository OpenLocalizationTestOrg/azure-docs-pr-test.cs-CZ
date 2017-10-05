---
title: "SSMS: Připojení a dotazování dat ve službě Azure SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak se připojit k SQL Database na Azure pomocí služby SQL Server Management Studio (SSMS). Potom spustíte příkazy jazyka Transact-SQL (T-SQL) k dotazování a úpravě dat."
metacanonical: 
keywords: "Připojení k SQL Database, SQL Server Management Studio"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 2835a72fc90d1fd39af73c6907648908e5d9fdeb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a><span data-ttu-id="07ae9-105">Azure SQL Database: Připojení a dotazování dat pomocí aplikace SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="07ae9-105">Azure SQL Database: Use SQL Server Management Studio to connect and query data</span></span>

<span data-ttu-id="07ae9-106">[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) je integrované prostředí pro správu jakékoliv infrastruktury SQL, od SQL Serveru po SQL Database pro Microsoft Windows.</span><span class="sxs-lookup"><span data-stu-id="07ae9-106">[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) is an integrated environment for managing any SQL infrastructure, from SQL Server to SQL Database for Microsoft Windows.</span></span> <span data-ttu-id="07ae9-107">Tento rychlý start ukazuje použití SSMS k připojení k Azure SQL Database a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="07ae9-107">This quick start demonstrates how to use SSMS to connect to an Azure SQL database, and then use Transact-SQL statements to query, insert, update, and delete data in the database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="07ae9-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07ae9-108">Prerequisites</span></span>

<span data-ttu-id="07ae9-109">Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:</span><span class="sxs-lookup"><span data-stu-id="07ae9-109">This quick start uses as its starting point the resources created in one of these quick starts:</span></span>

- [<span data-ttu-id="07ae9-110">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="07ae9-110">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
- [<span data-ttu-id="07ae9-111">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="07ae9-111">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
- [<span data-ttu-id="07ae9-112">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="07ae9-112">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

<span data-ttu-id="07ae9-113">Než začnete, ujistěte se, že máte nainstalovanou nejnovější verzi aplikace [SSMS](https://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="07ae9-113">Before you start, make sure you have installed the newest version of [SSMS](https://msdn.microsoft.com/library/mt238290.aspx).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="07ae9-114">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="07ae9-114">SQL server connection information</span></span>

<span data-ttu-id="07ae9-115">Získejte informace o připojení potřebné pro připojení k databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="07ae9-115">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="07ae9-116">V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="07ae9-116">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="07ae9-117">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="07ae9-117">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="07ae9-118">V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="07ae9-118">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="07ae9-119">Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="07ae9-119">On the **Overview** page for your database, review the fully qualified server name as shown in the image below.</span></span> <span data-ttu-id="07ae9-120">Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**.</span><span class="sxs-lookup"><span data-stu-id="07ae9-120">You can hover over the server name to bring up the **Click to copy** option.</span></span>

   ![informace o připojení](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="07ae9-122">Pokud jste zapomněli přihlašovací informace pro váš server Azure SQL Database, přejděte na stránku serveru SQL Database, abyste zobrazili jméno správce serveru a v případě potřeby obnovili heslo.</span><span class="sxs-lookup"><span data-stu-id="07ae9-122">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span> 

## <a name="connect-to-your-database"></a><span data-ttu-id="07ae9-123">Připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="07ae9-123">Connect to your database</span></span>

<span data-ttu-id="07ae9-124">Pomocí aplikace SQL Server Management Studio navažte připojení k serveru služby Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="07ae9-124">Use SQL Server Management Studio to establish a connection to your Azure SQL Database server.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="07ae9-125">Logický server Azure SQL Database naslouchá na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="07ae9-125">An Azure SQL Database logical server listens on port 1433.</span></span> <span data-ttu-id="07ae9-126">Pokud se pokoušíte připojovat k logickému serveru Azure SQL Database z oblasti za podnikovou bránou firewall, je třeba v podnikové brány firewall otevřít tento port, abyste se mohli úspěšně připojit.</span><span class="sxs-lookup"><span data-stu-id="07ae9-126">If you are attempting to connect to an Azure SQL Database logical server from within a corporate firewall, this port must be open in the corporate firewall for you to successfully connect.</span></span>
>

1. <span data-ttu-id="07ae9-127">Otevřete SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="07ae9-127">Open SQL Server Management Studio.</span></span>

2. <span data-ttu-id="07ae9-128">V dialogovém okně **Připojení k serveru** zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="07ae9-128">In the **Connect to Server** dialog box, enter the following information:</span></span>

   | <span data-ttu-id="07ae9-129">Nastavení</span><span class="sxs-lookup"><span data-stu-id="07ae9-129">Setting</span></span>       | <span data-ttu-id="07ae9-130">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="07ae9-130">Suggested value</span></span> | <span data-ttu-id="07ae9-131">Popis</span><span class="sxs-lookup"><span data-stu-id="07ae9-131">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="07ae9-132">**Typ serveru**</span><span class="sxs-lookup"><span data-stu-id="07ae9-132">**Server type**</span></span> | <span data-ttu-id="07ae9-133">Databázový stroj</span><span class="sxs-lookup"><span data-stu-id="07ae9-133">Database engine</span></span> | <span data-ttu-id="07ae9-134">Tato hodnota se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="07ae9-134">This value is required.</span></span> |
   | <span data-ttu-id="07ae9-135">**Název serveru**</span><span class="sxs-lookup"><span data-stu-id="07ae9-135">**Server name**</span></span> | <span data-ttu-id="07ae9-136">Plně kvalifikovaný název serveru</span><span class="sxs-lookup"><span data-stu-id="07ae9-136">The fully qualified server name</span></span> | <span data-ttu-id="07ae9-137">Název musí vypadat přibližně takto: **mynewserver20170313.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="07ae9-137">The name should be something like this: **mynewserver20170313.database.windows.net**.</span></span> |
   | <span data-ttu-id="07ae9-138">**Ověřování**</span><span class="sxs-lookup"><span data-stu-id="07ae9-138">**Authentication**</span></span> | <span data-ttu-id="07ae9-139">Ověřování SQL Serveru</span><span class="sxs-lookup"><span data-stu-id="07ae9-139">SQL Server Authentication</span></span> | <span data-ttu-id="07ae9-140">Ověřování SQL je jediný typ ověřování, který jsme v tomto kurzu nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="07ae9-140">SQL Authentication is the only authentication type that we have configured in this tutorial.</span></span> |
   | <span data-ttu-id="07ae9-141">**Přihlášení**</span><span class="sxs-lookup"><span data-stu-id="07ae9-141">**Login**</span></span> | <span data-ttu-id="07ae9-142">Účet správce serveru</span><span class="sxs-lookup"><span data-stu-id="07ae9-142">The server admin account</span></span> | <span data-ttu-id="07ae9-143">Jedná se o účet, který jste zadali při vytváření serveru.</span><span class="sxs-lookup"><span data-stu-id="07ae9-143">This is the account that you specified when you created the server.</span></span> |
   | <span data-ttu-id="07ae9-144">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="07ae9-144">**Password**</span></span> | <span data-ttu-id="07ae9-145">Heslo pro účet správce serveru</span><span class="sxs-lookup"><span data-stu-id="07ae9-145">The password for your server admin account</span></span> | <span data-ttu-id="07ae9-146">Jedná se o heslo, které jste zadali při vytváření serveru.</span><span class="sxs-lookup"><span data-stu-id="07ae9-146">This is the password that you specified when you created the server.</span></span> |

   ![Připojení k serveru](./media/sql-database-connect-query-ssms/connect.png)  

3. <span data-ttu-id="07ae9-148">Klikněte na **Možnosti** v dialogovém okně **Připojit k serveru**.</span><span class="sxs-lookup"><span data-stu-id="07ae9-148">Click **Options** in the **Connect to server** dialog box.</span></span> <span data-ttu-id="07ae9-149">V části **Připojit k databázi** zadejte **mySampleDatabase**, abyste se připojili k této databázi.</span><span class="sxs-lookup"><span data-stu-id="07ae9-149">In the **Connect to database** section, enter **mySampleDatabase** to connect to this database.</span></span>

   ![připojení k databázi na serveru](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. <span data-ttu-id="07ae9-151">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="07ae9-151">Click **Connect**.</span></span> <span data-ttu-id="07ae9-152">V aplikaci SSMS se otevře okno Průzkumníka objektů.</span><span class="sxs-lookup"><span data-stu-id="07ae9-152">The Object Explorer window opens in SSMS.</span></span> 

   ![Připojeno k serveru](./media/sql-database-connect-query-ssms/connected.png)  

5. <span data-ttu-id="07ae9-154">V Průzkumníku objektů zobrazte objekty v ukázkové databázi rozbalením **Databáze** a potom **mySampleDatabase**.</span><span class="sxs-lookup"><span data-stu-id="07ae9-154">In Object Explorer, expand **Databases** and then expand **mySampleDatabase** to view the objects in the sample database.</span></span>

## <a name="query-data"></a><span data-ttu-id="07ae9-155">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="07ae9-155">Query data</span></span>

<span data-ttu-id="07ae9-156">Použijte následující kód k zadání dotazu na Top 20 produktů podle kategorie pomocí příkazu jazyka Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx).</span><span class="sxs-lookup"><span data-stu-id="07ae9-156">Use the following code to query for the top 20 products by category using the [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="07ae9-157">V Průzkumníku objektů klikněte pravým tlačítkem na **mySampleDatabase** a potom klikněte na **Nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="07ae9-157">In Object Explorer, right-click **mySampleDatabase** and click **New Query**.</span></span> <span data-ttu-id="07ae9-158">Otevře se prázdné okno dotazu připojené k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="07ae9-158">A blank query window opens that is connected to your database.</span></span>
2. <span data-ttu-id="07ae9-159">Do okna dotazu zadejte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="07ae9-159">In the query window, enter the following query:</span></span>

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. <span data-ttu-id="07ae9-160">Kliknutím na **Provést** na panelu nástrojů načtěte data z tabulek Product a ProductCategory.</span><span class="sxs-lookup"><span data-stu-id="07ae9-160">On the toolbar, click **Execute** to retrieve data from the Product and ProductCategory tables.</span></span>

    ![query](./media/sql-database-connect-query-ssms/query.png)

## <a name="insert-data"></a><span data-ttu-id="07ae9-162">Vložení dat</span><span class="sxs-lookup"><span data-stu-id="07ae9-162">Insert data</span></span>

<span data-ttu-id="07ae9-163">Použijte následující kód k vložení nového produktu do tabulky SalesLT.Product pomocí příkazu jazyka Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).</span><span class="sxs-lookup"><span data-stu-id="07ae9-163">Use the following code to insert a new product into the SalesLT.Product table using the [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="07ae9-164">V okně dotazu nahraďte předchozí dotaz následujícím dotazem:</span><span class="sxs-lookup"><span data-stu-id="07ae9-164">In the query window, replace the previous query with the following query:</span></span>

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. <span data-ttu-id="07ae9-165">Kliknutím na **Provést** na panelu nástrojů vložte nový řádek do tabulky Product.</span><span class="sxs-lookup"><span data-stu-id="07ae9-165">On the toolbar, click **Execute**  to insert a new row in the Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a><span data-ttu-id="07ae9-166">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="07ae9-166">Update data</span></span>

<span data-ttu-id="07ae9-167">Použijte následující kód k aktualizaci nového produktu, který jste přidali dříve, pomocí příkazu jazyka Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).</span><span class="sxs-lookup"><span data-stu-id="07ae9-167">Use the following code to update the new product that you previously added using the [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="07ae9-168">V okně dotazu nahraďte předchozí dotaz následujícím dotazem:</span><span class="sxs-lookup"><span data-stu-id="07ae9-168">In the query window, replace the previous query with the following query:</span></span>

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="07ae9-169">Kliknutím na **Provést** na panelu nástrojů aktualizujte zadaný řádek v tabulce Product.</span><span class="sxs-lookup"><span data-stu-id="07ae9-169">On the toolbar, click **Execute** to update the specified row in the Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a><span data-ttu-id="07ae9-170">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="07ae9-170">Delete data</span></span>

<span data-ttu-id="07ae9-171">Použijte následující kód k odstranění nového produktu, který jste přidali dříve, pomocí příkazu jazyka Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).</span><span class="sxs-lookup"><span data-stu-id="07ae9-171">Use the following code to delete the new product that you previously added using the [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="07ae9-172">V okně dotazu nahraďte předchozí dotaz následujícím dotazem:</span><span class="sxs-lookup"><span data-stu-id="07ae9-172">In the query window, replace the previous query with the following query:</span></span>

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="07ae9-173">Kliknutím na **Provést** na panelu nástrojů odstraňte zadaný řádek z tabulky Product.</span><span class="sxs-lookup"><span data-stu-id="07ae9-173">On the toolbar, click **Execute** to delete the specified row in the Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a><span data-ttu-id="07ae9-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07ae9-174">Next steps</span></span>

- <span data-ttu-id="07ae9-175">Další informace o vytváření a správě serverů a databází pomocí jazyka Transact-SQL najdete v tématu [Další informace o serverech a databázích Azure SQL Database](sql-database-servers-databases.md).</span><span class="sxs-lookup"><span data-stu-id="07ae9-175">To learn about creating and managing servers and databases with Transact-SQL, see [Learn about Azure SQL Database servers and databases](sql-database-servers-databases.md).</span></span>
- <span data-ttu-id="07ae9-176">Další informace o aplikaci SSMS najdete v tématu [Použití aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span><span class="sxs-lookup"><span data-stu-id="07ae9-176">For information about SSMS, see [Use SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span></span>
- <span data-ttu-id="07ae9-177">Informace o připojení a dotazování pomocí Visual Studio Code najdete v tématu [Připojení a dotazování pomocí Visual Studio Code](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="07ae9-177">To connect and query using Visual Studio Code, see [Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>
- <span data-ttu-id="07ae9-178">Informace o připojení a dotazování pomocí .NET najdete v tématu [Připojení a dotazování pomocí .NET](sql-database-connect-query-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="07ae9-178">To connect and query using .NET, see [Connect and query with .NET](sql-database-connect-query-dotnet.md).</span></span>
- <span data-ttu-id="07ae9-179">Informace o připojení a dotazování pomocí PHP najdete v tématu [Připojení a dotazování pomocí PHP](sql-database-connect-query-php.md).</span><span class="sxs-lookup"><span data-stu-id="07ae9-179">To connect and query using PHP, see [Connect and query with PHP](sql-database-connect-query-php.md).</span></span>
- <span data-ttu-id="07ae9-180">Informace o připojení a dotazování pomocí Node.js najdete v tématu [Připojení a dotazování pomocí Node.js](sql-database-connect-query-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="07ae9-180">To connect and query using Node.js, see [Connect and query with Node.js](sql-database-connect-query-nodejs.md).</span></span>
- <span data-ttu-id="07ae9-181">Informace o připojení a dotazování pomocí Javy najdete v tématu [Připojení a dotazování pomocí Javy](sql-database-connect-query-java.md).</span><span class="sxs-lookup"><span data-stu-id="07ae9-181">To connect and query using Java, see [Connect and query with Java](sql-database-connect-query-java.md).</span></span>
- <span data-ttu-id="07ae9-182">Informace o připojení a dotazování pomocí Pythonu najdete v tématu [Připojení a dotazování pomocí Pythonu](sql-database-connect-query-python.md).</span><span class="sxs-lookup"><span data-stu-id="07ae9-182">To connect and query using Python, see [Connect and query with Python](sql-database-connect-query-python.md).</span></span>
- <span data-ttu-id="07ae9-183">Informace o připojení a dotazování pomocí Ruby najdete v tématu [Připojení a dotazování pomocí Ruby](sql-database-connect-query-ruby.md).</span><span class="sxs-lookup"><span data-stu-id="07ae9-183">To connect and query using Ruby, see [Connect and query with Ruby](sql-database-connect-query-ruby.md).</span></span>