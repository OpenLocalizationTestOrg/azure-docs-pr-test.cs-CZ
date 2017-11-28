---
title: aaaUse Node.js tooquery Azure SQL Database | Microsoft Docs
description: "Toto téma ukazuje, jak toouse Node.js toocreate program, který se připojuje tooan Azure SQL Database a dotazování pomocí příkazů Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 3870130a486c218eafeb9cf792a4275de7fd6551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-nodejs-tooquery-an-azure-sql-database"></a><span data-ttu-id="a8ba0-103">Pomocí Node.js tooquery Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="a8ba0-103">Use Node.js tooquery an Azure SQL database</span></span>

<span data-ttu-id="a8ba0-104">Tento úvodní kurz ukazuje, jak toouse [Node.js](https://nodejs.org/en/) toocreate tooan tooconnect programu Azure SQL databáze a používat data tooquery příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-104">This quick start tutorial demonstrates how toouse [Node.js](https://nodejs.org/en/) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8ba0-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a8ba0-105">Prerequisites</span></span>

<span data-ttu-id="a8ba0-106">toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="a8ba0-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="a8ba0-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-107">An Azure SQL database.</span></span> <span data-ttu-id="a8ba0-108">Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="a8ba0-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="a8ba0-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="a8ba0-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="a8ba0-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="a8ba0-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="a8ba0-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8ba0-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="a8ba0-112">A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="a8ba0-113">Máte nainstalované Node.js a související software pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-113">You have installed Node.js and related software for your operating system.</span></span>
    - <span data-ttu-id="a8ba0-114">**Systému MacOS**: Nainstalujte Homebrew a Node.js a potom nainstalujte ovladač ODBC hello a SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-114">**MacOS**: Install Homebrew and Node.js, and then install hello ODBC driver and SQLCMD.</span></span> <span data-ttu-id="a8ba0-115">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span><span class="sxs-lookup"><span data-stu-id="a8ba0-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span></span>
    - <span data-ttu-id="a8ba0-116">**Ubuntu**: Instalace softwaru Node.js a pak nainstalujte ovladač ODBC hello a SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-116">**Ubuntu**: Install Node.js, and then install hello ODBC driver and SQLCMD.</span></span> <span data-ttu-id="a8ba0-117">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="a8ba0-117">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .</span></span>
    - <span data-ttu-id="a8ba0-118">**Windows**: Nainstalujte Chocolatey a Node.js a potom nainstalujte ovladač ODBC hello a SQL CMD.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-118">**Windows**: Install Chocolatey and Node.js, and then install hello ODBC driver and SQL CMD.</span></span> <span data-ttu-id="a8ba0-119">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span><span class="sxs-lookup"><span data-stu-id="a8ba0-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="a8ba0-120">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="a8ba0-120">SQL server connection information</span></span>

<span data-ttu-id="a8ba0-121">Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="a8ba0-122">Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="a8ba0-123">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a8ba0-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a8ba0-124">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="a8ba0-125">Na hello **přehled** stránky pro vaši databázi hello zkontrolujte plně kvalifikovaný název serveru, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="a8ba0-126">Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span> 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="a8ba0-128">Pokud jste zapomněli hello přihlašovací informace pro váš server databáze SQL Azure, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název a, v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-128">If you have forgotten hello login information for your Azure SQL Database server, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8ba0-129">Pravidlo brány firewall musí mít zavedené hello veřejných IP adres hello počítače, na kterém provádíte tento kurz.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-129">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="a8ba0-130">Pokud jsou v jiném počítači nebo mít jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí portálu Azure hello](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="a8ba0-130">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="create-a-nodejs-project"></a><span data-ttu-id="a8ba0-131">Vytvoření projektu Node.js</span><span class="sxs-lookup"><span data-stu-id="a8ba0-131">Create a Node.js project</span></span>

<span data-ttu-id="a8ba0-132">Otevřete příkazový řádek a vytvořte složku *sqltest*.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-132">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="a8ba0-133">Přejděte toohello složky můžete vytvořit a spustit hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8ba0-133">Navigate toohello folder you created and run hello following command:</span></span>

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="a8ba0-134">Vložení kódu tooquery SQL database</span><span class="sxs-lookup"><span data-stu-id="a8ba0-134">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="a8ba0-135">Ve vývojovém prostředí nebo oblíbeném textovém editoru vytvořte nový soubor **sqltest.js**.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-135">In your development environment or favorite text editor, create a new file, **sqltest.js**.</span></span>

2. <span data-ttu-id="a8ba0-136">Nahraďte obsah hello hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-136">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection toodatabase
   var config = 
      {
        userName: 'someuser', // update me
        password: 'somepassword', // update me
        server: 'edmacasqlserver.database.windows.net', // update me
        options: 
           {
              database: 'somedb' //update me
              , encrypt: true
           }
      }
   var connection = new Connection(config);

   // Attempt tooconnect and execute queries if connection goes through
   connection.on('connect', function(err) 
      {
        if (err) 
          {
             console.log(err)
          }
       else
          {
              queryDatabase()
          }
      }
    );

   function queryDatabase()
      { console.log('Reading rows from hello Table...');

          // Read all rows from table
        request = new Request(
             "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
                function(err, rowCount, rows) 
                   {
                       console.log(rowCount + ' row(s) returned');
                       process.exit();
                   }
               );
    
        request.on('row', function(columns) {
           columns.forEach(function(column) {
               console.log("%s\t%s", column.metadata.colName, column.value);
            });
                });
        connection.execSql(request);
      }
```

## <a name="run-hello-code"></a><span data-ttu-id="a8ba0-137">Spuštění kódu hello</span><span class="sxs-lookup"><span data-stu-id="a8ba0-137">Run hello code</span></span>

1. <span data-ttu-id="a8ba0-138">Hello příkazového řádku spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="a8ba0-138">At hello command prompt, run hello following commands:</span></span>

   ```js
   node sqltest.js
   ```

2. <span data-ttu-id="a8ba0-139">Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-139">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8ba0-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8ba0-140">Next steps</span></span>

- <span data-ttu-id="a8ba0-141">Další informace o hello [Microsoft Driver Node.js pro SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span><span class="sxs-lookup"><span data-stu-id="a8ba0-141">Learn about hello [Microsoft Node.js Driver for SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span></span>
- <span data-ttu-id="a8ba0-142">Zjistěte, jak příliš[připojení a dotazování Azure SQL database pomocí .NET core](sql-database-connect-query-dotnet-core.md) v systému Windows nebo Linux/systému macOS.</span><span class="sxs-lookup"><span data-stu-id="a8ba0-142">Learn how too[connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="a8ba0-143">Další informace o [Začínáme s .NET Core v systému Windows nebo Linux/macOS hello příkazového řádku](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="a8ba0-143">Learn about [Getting started with .NET Core on Windows/Linux/macOS using hello command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="a8ba0-144">Zjistěte, jak příliš[navrhnout první databáze Azure SQL pomocí aplikace SSMS](sql-database-design-first-database.md) nebo [navrhnout první databáze Azure SQL pomocí rozhraní .NET](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="a8ba0-144">Learn how too[Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="a8ba0-145">Zjistěte, jak příliš[připojit a zadávat dotazy pomocí SSMS](sql-database-connect-query-ssms.md)</span><span class="sxs-lookup"><span data-stu-id="a8ba0-145">Learn how too[Connect and query with SSMS](sql-database-connect-query-ssms.md)</span></span>
- <span data-ttu-id="a8ba0-146">Zjistěte, jak příliš[připojit a zadávat dotazy s Visual Studio Code](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="a8ba0-146">Learn how too[Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>


