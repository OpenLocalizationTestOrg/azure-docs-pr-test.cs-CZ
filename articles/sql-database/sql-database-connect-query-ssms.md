---
title: "SSMS: Připojení a dotazování dat ve službě Azure SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak tooconnect tooSQL databáze v Azure pomocí služby SQL Server Management Studio (SSMS). Potom spusťte příkazy jazyka Transact-SQL (T-SQL) tooquery a upravit data."
metacanonical: 
keywords: "připojit databáze toosql, sql server management studio"
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
ms.openlocfilehash: 769a3a1ecc34800bd345b64e89841f7147b144f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-use-sql-server-management-studio-tooconnect-and-query-data"></a><span data-ttu-id="24d71-105">Azure SQL Database: Pomocí SQL Server Management Studio tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="24d71-105">Azure SQL Database: Use SQL Server Management Studio tooconnect and query data</span></span>

<span data-ttu-id="24d71-106">[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) je integrované prostředí pro správu jakékoliv infrastruktury, SQL, z tooSQL systému SQL Server databáze pro Microsoft Windows.</span><span class="sxs-lookup"><span data-stu-id="24d71-106">[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) is an integrated environment for managing any SQL infrastructure, from SQL Server tooSQL Database for Microsoft Windows.</span></span> <span data-ttu-id="24d71-107">Tento rychlý start předvádí, jak toouse SSMS tooconnect tooan Azure SQL database a použití jazyka Transact-SQL příkazy tooquery, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-107">This quick start demonstrates how toouse SSMS tooconnect tooan Azure SQL database, and then use Transact-SQL statements tooquery, insert, update, and delete data in hello database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="24d71-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="24d71-108">Prerequisites</span></span>

<span data-ttu-id="24d71-109">Tento rychlý start používá jako jeho výchozí prostředky hello bodu vytvořené v jednom z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="24d71-109">This quick start uses as its starting point hello resources created in one of these quick starts:</span></span>

- [<span data-ttu-id="24d71-110">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="24d71-110">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
- [<span data-ttu-id="24d71-111">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="24d71-111">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
- [<span data-ttu-id="24d71-112">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="24d71-112">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

<span data-ttu-id="24d71-113">Než začnete, ujistěte se, máte nainstalovanou nejnovější verzi hello [SSMS](https://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="24d71-113">Before you start, make sure you have installed hello newest version of [SSMS](https://msdn.microsoft.com/library/mt238290.aspx).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="24d71-114">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="24d71-114">SQL server connection information</span></span>

<span data-ttu-id="24d71-115">Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="24d71-115">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="24d71-116">Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-116">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="24d71-117">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="24d71-117">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="24d71-118">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="24d71-118">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="24d71-119">Na hello **přehled** pro vaši databázi si prohlédněte hello serveru plně kvalifikovaný název, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-119">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello image below.</span></span> <span data-ttu-id="24d71-120">Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost.</span><span class="sxs-lookup"><span data-stu-id="24d71-120">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>

   ![informace o připojení](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="24d71-122">Pokud jste zapomněli hello přihlašovací informace pro váš server databáze SQL Azure, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název a, v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-122">If you have forgotten hello login information for your Azure SQL Database server, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span> 

## <a name="connect-tooyour-database"></a><span data-ttu-id="24d71-123">Připojit databáze tooyour</span><span class="sxs-lookup"><span data-stu-id="24d71-123">Connect tooyour database</span></span>

<span data-ttu-id="24d71-124">Pomocí SQL Server Management Studio tooestablish serveru Azure SQL Database tooyour připojení.</span><span class="sxs-lookup"><span data-stu-id="24d71-124">Use SQL Server Management Studio tooestablish a connection tooyour Azure SQL Database server.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="24d71-125">Logický server Azure SQL Database naslouchá na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="24d71-125">An Azure SQL Database logical server listens on port 1433.</span></span> <span data-ttu-id="24d71-126">Pokud se pokoušíte tooconnect tooan Azure SQL Database logického serveru z podniková brána firewall, musí být tento port otevřít v hello podniková brána firewall pro připojení je toosuccessfully.</span><span class="sxs-lookup"><span data-stu-id="24d71-126">If you are attempting tooconnect tooan Azure SQL Database logical server from within a corporate firewall, this port must be open in hello corporate firewall for you toosuccessfully connect.</span></span>
>

1. <span data-ttu-id="24d71-127">Otevřete SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="24d71-127">Open SQL Server Management Studio.</span></span>

2. <span data-ttu-id="24d71-128">V hello **připojit tooServer** dialogovém okně zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="24d71-128">In hello **Connect tooServer** dialog box, enter hello following information:</span></span>

   | <span data-ttu-id="24d71-129">Nastavení</span><span class="sxs-lookup"><span data-stu-id="24d71-129">Setting</span></span>       | <span data-ttu-id="24d71-130">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="24d71-130">Suggested value</span></span> | <span data-ttu-id="24d71-131">Popis</span><span class="sxs-lookup"><span data-stu-id="24d71-131">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="24d71-132">**Typ serveru**</span><span class="sxs-lookup"><span data-stu-id="24d71-132">**Server type**</span></span> | <span data-ttu-id="24d71-133">Databázový stroj</span><span class="sxs-lookup"><span data-stu-id="24d71-133">Database engine</span></span> | <span data-ttu-id="24d71-134">Tato hodnota se vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="24d71-134">This value is required.</span></span> |
   | <span data-ttu-id="24d71-135">**Název serveru**</span><span class="sxs-lookup"><span data-stu-id="24d71-135">**Server name**</span></span> | <span data-ttu-id="24d71-136">název plně kvalifikovaný server Hello</span><span class="sxs-lookup"><span data-stu-id="24d71-136">hello fully qualified server name</span></span> | <span data-ttu-id="24d71-137">Hello název by měl být přibližně takto: **mynewserver20170313.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="24d71-137">hello name should be something like this: **mynewserver20170313.database.windows.net**.</span></span> |
   | <span data-ttu-id="24d71-138">**Ověřování**</span><span class="sxs-lookup"><span data-stu-id="24d71-138">**Authentication**</span></span> | <span data-ttu-id="24d71-139">Ověřování SQL Serveru</span><span class="sxs-lookup"><span data-stu-id="24d71-139">SQL Server Authentication</span></span> | <span data-ttu-id="24d71-140">Ověřování systému SQL je typ hello pouze ověřování, který jsme nakonfigurovali v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="24d71-140">SQL Authentication is hello only authentication type that we have configured in this tutorial.</span></span> |
   | <span data-ttu-id="24d71-141">**Přihlášení**</span><span class="sxs-lookup"><span data-stu-id="24d71-141">**Login**</span></span> | <span data-ttu-id="24d71-142">účet správce serveru Hello</span><span class="sxs-lookup"><span data-stu-id="24d71-142">hello server admin account</span></span> | <span data-ttu-id="24d71-143">Toto je hello účet, který jste zadali při vytváření hello server.</span><span class="sxs-lookup"><span data-stu-id="24d71-143">This is hello account that you specified when you created hello server.</span></span> |
   | <span data-ttu-id="24d71-144">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="24d71-144">**Password**</span></span> | <span data-ttu-id="24d71-145">Hello heslo pro váš účet správce serveru</span><span class="sxs-lookup"><span data-stu-id="24d71-145">hello password for your server admin account</span></span> | <span data-ttu-id="24d71-146">Toto je hello heslo, které jste zadali při vytváření hello server.</span><span class="sxs-lookup"><span data-stu-id="24d71-146">This is hello password that you specified when you created hello server.</span></span> |

   ![připojit tooserver](./media/sql-database-connect-query-ssms/connect.png)  

3. <span data-ttu-id="24d71-148">Klikněte na tlačítko **možnosti** v hello **připojit tooserver** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="24d71-148">Click **Options** in hello **Connect tooserver** dialog box.</span></span> <span data-ttu-id="24d71-149">V hello **připojit toodatabase** zadejte **mySampleDatabase** tooconnect toothis databáze.</span><span class="sxs-lookup"><span data-stu-id="24d71-149">In hello **Connect toodatabase** section, enter **mySampleDatabase** tooconnect toothis database.</span></span>

   ![připojit toodb na serveru](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. <span data-ttu-id="24d71-151">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="24d71-151">Click **Connect**.</span></span> <span data-ttu-id="24d71-152">Otevře se okno Průzkumník objektů Hello v aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="24d71-152">hello Object Explorer window opens in SSMS.</span></span> 

   ![připojené tooserver](./media/sql-database-connect-query-ssms/connected.png)  

5. <span data-ttu-id="24d71-154">V Průzkumníku objektů rozbalte **databáze** a potom rozbalte **mySampleDatabase** tooview hello objekty v ukázkové databázi hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-154">In Object Explorer, expand **Databases** and then expand **mySampleDatabase** tooview hello objects in hello sample database.</span></span>

## <a name="query-data"></a><span data-ttu-id="24d71-155">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="24d71-155">Query data</span></span>

<span data-ttu-id="24d71-156">Použití hello následující kód tooquery produktů hello prvních 20 počítačů podle kategorie pomocí hello [vyberte](https://msdn.microsoft.com/library/ms189499.aspx) příkazu Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="24d71-156">Use hello following code tooquery for hello top 20 products by category using hello [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="24d71-157">V Průzkumníku objektů klikněte pravým tlačítkem na **mySampleDatabase** a potom klikněte na **Nový dotaz**.</span><span class="sxs-lookup"><span data-stu-id="24d71-157">In Object Explorer, right-click **mySampleDatabase** and click **New Query**.</span></span> <span data-ttu-id="24d71-158">Otevře okno prázdné dotazu který je připojený tooyour databáze.</span><span class="sxs-lookup"><span data-stu-id="24d71-158">A blank query window opens that is connected tooyour database.</span></span>
2. <span data-ttu-id="24d71-159">V okně dotazu hello zadejte hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="24d71-159">In hello query window, enter hello following query:</span></span>

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. <span data-ttu-id="24d71-160">Na panelu nástrojů hello, klikněte na tlačítko **Execute** tooretrieve data z tabulek produktu a ProductCategory hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-160">On hello toolbar, click **Execute** tooretrieve data from hello Product and ProductCategory tables.</span></span>

    ![query](./media/sql-database-connect-query-ssms/query.png)

## <a name="insert-data"></a><span data-ttu-id="24d71-162">Vložení dat</span><span class="sxs-lookup"><span data-stu-id="24d71-162">Insert data</span></span>

<span data-ttu-id="24d71-163">Použití hello následující kód tooinsert nového produktu do tabulky SalesLT.Product hello pomocí hello [vložit](https://msdn.microsoft.com/library/ms174335.aspx) příkazu Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="24d71-163">Use hello following code tooinsert a new product into hello SalesLT.Product table using hello [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="24d71-164">V okně dotazu hello nahraďte předchozí dotaz hello hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="24d71-164">In hello query window, replace hello previous query with hello following query:</span></span>

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

2. <span data-ttu-id="24d71-165">Na panelu nástrojů hello, klikněte na tlačítko **Execute** tooinsert nový řádek v tabulce produktu hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-165">On hello toolbar, click **Execute**  tooinsert a new row in hello Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a><span data-ttu-id="24d71-166">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="24d71-166">Update data</span></span>

<span data-ttu-id="24d71-167">Použití hello následující kód tooupdate hello nového produktu, zda jste dříve přidali pomocí hello [aktualizace](https://msdn.microsoft.com/library/ms177523.aspx) příkazu Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="24d71-167">Use hello following code tooupdate hello new product that you previously added using hello [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="24d71-168">V okně dotazu hello nahraďte předchozí dotaz hello hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="24d71-168">In hello query window, replace hello previous query with hello following query:</span></span>

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="24d71-169">Na panelu nástrojů hello, klikněte na tlačítko **Execute** tooupdate hello zadaný řádek v tabulce produktu hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-169">On hello toolbar, click **Execute** tooupdate hello specified row in hello Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a><span data-ttu-id="24d71-170">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="24d71-170">Delete data</span></span>

<span data-ttu-id="24d71-171">Použití hello následující kód toodelete hello nového produktu, zda jste dříve přidali pomocí hello [odstranit](https://msdn.microsoft.com/library/ms189835.aspx) příkazu Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="24d71-171">Use hello following code toodelete hello new product that you previously added using hello [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL statement.</span></span>

1. <span data-ttu-id="24d71-172">V okně dotazu hello nahraďte předchozí dotaz hello hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="24d71-172">In hello query window, replace hello previous query with hello following query:</span></span>

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. <span data-ttu-id="24d71-173">Na panelu nástrojů hello, klikněte na tlačítko **Execute** toodelete hello zadaný řádek v tabulce produktu hello.</span><span class="sxs-lookup"><span data-stu-id="24d71-173">On hello toolbar, click **Execute** toodelete hello specified row in hello Product table.</span></span>

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a><span data-ttu-id="24d71-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24d71-174">Next steps</span></span>

- <span data-ttu-id="24d71-175">toolearn o vytváření a správě serverů a databází pomocí jazyka Transact-SQL, najdete v části [Další informace o službě Azure SQL Database serverů a databází](sql-database-servers-databases.md).</span><span class="sxs-lookup"><span data-stu-id="24d71-175">toolearn about creating and managing servers and databases with Transact-SQL, see [Learn about Azure SQL Database servers and databases](sql-database-servers-databases.md).</span></span>
- <span data-ttu-id="24d71-176">Další informace o aplikaci SSMS najdete v tématu [Použití aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span><span class="sxs-lookup"><span data-stu-id="24d71-176">For information about SSMS, see [Use SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span></span>
- <span data-ttu-id="24d71-177">tooconnect a dotazu pomocí kódu pro Visual Studio, najdete v části [připojit a zadávat dotazy s Visual Studio Code](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="24d71-177">tooconnect and query using Visual Studio Code, see [Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>
- <span data-ttu-id="24d71-178">tooconnect a dotaz pomocí rozhraní .NET, najdete v části [připojit a zadávat dotazy pomocí .NET](sql-database-connect-query-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="24d71-178">tooconnect and query using .NET, see [Connect and query with .NET](sql-database-connect-query-dotnet.md).</span></span>
- <span data-ttu-id="24d71-179">tooconnect a dotazem jazyka PHP, najdete v části [připojit a zadávat dotazy s PHP](sql-database-connect-query-php.md).</span><span class="sxs-lookup"><span data-stu-id="24d71-179">tooconnect and query using PHP, see [Connect and query with PHP](sql-database-connect-query-php.md).</span></span>
- <span data-ttu-id="24d71-180">tooconnect a dotaz pomocí Node.js, najdete v části [připojit a zadávat dotazy s Node.js](sql-database-connect-query-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="24d71-180">tooconnect and query using Node.js, see [Connect and query with Node.js](sql-database-connect-query-nodejs.md).</span></span>
- <span data-ttu-id="24d71-181">tooconnect a dotazem jazyka Java, najdete v části [připojit a zadávat dotazy s Javou](sql-database-connect-query-java.md).</span><span class="sxs-lookup"><span data-stu-id="24d71-181">tooconnect and query using Java, see [Connect and query with Java](sql-database-connect-query-java.md).</span></span>
- <span data-ttu-id="24d71-182">tooconnect a dotazem jazyka Python, najdete v části [připojit a zadávat dotazy s Pythonem](sql-database-connect-query-python.md).</span><span class="sxs-lookup"><span data-stu-id="24d71-182">tooconnect and query using Python, see [Connect and query with Python](sql-database-connect-query-python.md).</span></span>
- <span data-ttu-id="24d71-183">tooconnect a dotazu pomocí Ruby, najdete v části [připojit a zadávat dotazy s Ruby](sql-database-connect-query-ruby.md).</span><span class="sxs-lookup"><span data-stu-id="24d71-183">tooconnect and query using Ruby, see [Connect and query with Ruby](sql-database-connect-query-ruby.md).</span></span>
