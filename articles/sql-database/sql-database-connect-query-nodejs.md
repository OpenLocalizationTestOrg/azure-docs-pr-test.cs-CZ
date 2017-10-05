---
title: "Použití Node.js k dotazování služby Azure SQL Database | Dokumentace Microsoftu"
description: "Toto téma vám ukáže, jak pomocí Node.js vytvořit program, který se připojí ke službě Azure SQL Database a bude ji dotazovat s použitím příkazů jazyka Transact-SQL."
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
ms.openlocfilehash: 1907a95df9132c059d7985b6d5cd913536bf3403
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-nodejs-to-query-an-azure-sql-database"></a><span data-ttu-id="86d05-103">Použití Node.js k dotazování databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="86d05-103">Use Node.js to query an Azure SQL database</span></span>

<span data-ttu-id="86d05-104">Tento rychlý úvodní kurz ukazuje použití [Node.js](https://nodejs.org/en/) k vytvoření programu pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.</span><span class="sxs-lookup"><span data-stu-id="86d05-104">This quick start tutorial demonstrates how to use [Node.js](https://nodejs.org/en/) to create a program to connect to an Azure SQL database and use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86d05-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="86d05-105">Prerequisites</span></span>

<span data-ttu-id="86d05-106">Abyste mohli absolvovat tento rychlý úvodní kurz, ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="86d05-106">To complete this quick start tutorial, make sure you have the following:</span></span>

- <span data-ttu-id="86d05-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="86d05-107">An Azure SQL database.</span></span> <span data-ttu-id="86d05-108">Tento rychlý start používá prostředky vytvořené v některém z těchto rychlých startů:</span><span class="sxs-lookup"><span data-stu-id="86d05-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="86d05-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="86d05-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="86d05-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="86d05-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="86d05-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="86d05-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="86d05-112">[Pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro veřejnou IP adresu počítače, který používáte pro tento rychlý úvodní kurz.</span><span class="sxs-lookup"><span data-stu-id="86d05-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="86d05-113">Máte nainstalované Node.js a související software pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="86d05-113">You have installed Node.js and related software for your operating system.</span></span>
    - <span data-ttu-id="86d05-114">**MacOS:** Nainstalujte Homebrew a Node.js a potom nainstalujte ovladač ODBC a nástroj SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="86d05-114">**MacOS**: Install Homebrew and Node.js, and then install the ODBC driver and SQLCMD.</span></span> <span data-ttu-id="86d05-115">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span><span class="sxs-lookup"><span data-stu-id="86d05-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span></span>
    - <span data-ttu-id="86d05-116">**Ubuntu:** Nainstalujte Node.js a potom nainstalujte ovladač ODBC a nástroj SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="86d05-116">**Ubuntu**: Install Node.js, and then install the ODBC driver and SQLCMD.</span></span> <span data-ttu-id="86d05-117">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="86d05-117">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .</span></span>
    - <span data-ttu-id="86d05-118">**Windows:** Nainstalujte Chocolatey a Node.js a potom nainstalujte ovladač ODBC a nástroj SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="86d05-118">**Windows**: Install Chocolatey and Node.js, and then install the ODBC driver and SQL CMD.</span></span> <span data-ttu-id="86d05-119">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span><span class="sxs-lookup"><span data-stu-id="86d05-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="86d05-120">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="86d05-120">SQL server connection information</span></span>

<span data-ttu-id="86d05-121">Získejte informace o připojení potřebné pro připojení k databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="86d05-121">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="86d05-122">V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="86d05-122">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="86d05-123">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="86d05-123">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="86d05-124">V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="86d05-124">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="86d05-125">Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="86d05-125">On the **Overview** page for your database, review the fully qualified server name as shown in the following image.</span></span> <span data-ttu-id="86d05-126">Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**.</span><span class="sxs-lookup"><span data-stu-id="86d05-126">You can hover over the server name to bring up the **Click to copy** option.</span></span> 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="86d05-128">Pokud jste zapomněli přihlašovací informace pro váš server Azure SQL Database, přejděte na stránku serveru SQL Database, abyste zobrazili jméno správce serveru a v případě potřeby resetovali heslo.</span><span class="sxs-lookup"><span data-stu-id="86d05-128">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86d05-129">Musíte mít nastavené pravidlo brány firewall pro veřejnou IP adresu počítače, na kterém provádíte tento kurz.</span><span class="sxs-lookup"><span data-stu-id="86d05-129">You must have a firewall rule in place for the public IP address of the computer on which you perform this tutorial.</span></span> <span data-ttu-id="86d05-130">Pokud jste na jiném počítači nebo máte jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí webu Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="86d05-130">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using the Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="create-a-nodejs-project"></a><span data-ttu-id="86d05-131">Vytvoření projektu Node.js</span><span class="sxs-lookup"><span data-stu-id="86d05-131">Create a Node.js project</span></span>

<span data-ttu-id="86d05-132">Otevřete příkazový řádek a vytvořte složku *sqltest*.</span><span class="sxs-lookup"><span data-stu-id="86d05-132">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="86d05-133">Přejděte do složky, kterou jste vytvořili, a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="86d05-133">Navigate to the folder you created and run the following command:</span></span>

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="86d05-134">Vložení kódu pro dotazování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="86d05-134">Insert code to query SQL database</span></span>

1. <span data-ttu-id="86d05-135">Ve vývojovém prostředí nebo oblíbeném textovém editoru vytvořte nový soubor **sqltest.js**.</span><span class="sxs-lookup"><span data-stu-id="86d05-135">In your development environment or favorite text editor, create a new file, **sqltest.js**.</span></span>

2. <span data-ttu-id="86d05-136">Nahraďte jeho obsah následujícím kódem a přidejte odpovídající hodnoty pro váš server, databázi, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="86d05-136">Replace the contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection to database
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

   // Attempt to connect and execute queries if connection goes through
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
      { console.log('Reading rows from the Table...');

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

## <a name="run-the-code"></a><span data-ttu-id="86d05-137">Spuštění kódu</span><span class="sxs-lookup"><span data-stu-id="86d05-137">Run the code</span></span>

1. <span data-ttu-id="86d05-138">V příkazovém řádku spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="86d05-138">At the command prompt, run the following commands:</span></span>

   ```js
   node sqltest.js
   ```

2. <span data-ttu-id="86d05-139">Ověřte, že se vrátilo prvních 20 řádků, a potom zavřete okno aplikace.</span><span class="sxs-lookup"><span data-stu-id="86d05-139">Verify that the top 20 rows are returned and then close the application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86d05-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86d05-140">Next steps</span></span>

- <span data-ttu-id="86d05-141">Informace o [ovladači Microsoft Node.js pro SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span><span class="sxs-lookup"><span data-stu-id="86d05-141">Learn about the [Microsoft Node.js Driver for SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span></span>
- <span data-ttu-id="86d05-142">Informace o [připojení k databázi SQL Azure a jejím dotazování pomocí .NET Core](sql-database-connect-query-dotnet-core.md) v systému Windows, Linux nebo macOS</span><span class="sxs-lookup"><span data-stu-id="86d05-142">Learn how to [connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="86d05-143">Informace o tom, [jak začít s .NET Core v systému Windows, Linux nebo macOS pomocí příkazového řádku](/dotnet/core/tutorials/using-with-xplat-cli)</span><span class="sxs-lookup"><span data-stu-id="86d05-143">Learn about [Getting started with .NET Core on Windows/Linux/macOS using the command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="86d05-144">Informace o [návrhu první databáze SQL Azure pomocí aplikace SSMS](sql-database-design-first-database.md) nebo [návrhu první databáze SQL Azure pomocí .NET](sql-database-design-first-database-csharp.md)</span><span class="sxs-lookup"><span data-stu-id="86d05-144">Learn how to [Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="86d05-145">Informace o [připojení a dotazování pomocí aplikace SSMS](sql-database-connect-query-ssms.md)</span><span class="sxs-lookup"><span data-stu-id="86d05-145">Learn how to [Connect and query with SSMS](sql-database-connect-query-ssms.md)</span></span>
- <span data-ttu-id="86d05-146">Informace o [připojení a dotazování pomocí Visual Studio Code](sql-database-connect-query-vscode.md)</span><span class="sxs-lookup"><span data-stu-id="86d05-146">Learn how to [Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>


