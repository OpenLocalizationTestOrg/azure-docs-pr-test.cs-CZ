---
title: aaaUse PHP tooquery Azure SQL Database | Microsoft Docs
description: "Toto téma ukazuje, jak toouse PHP toocreate program, který se připojuje tooan Azure SQL Database a dotazování pomocí příkazů Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a 22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: hero-article
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 5fc49dcc42ab07cc1bec554be39bdf08dbd6f75e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-php-tooquery-an-azure-sql-database"></a><span data-ttu-id="b8386-103">Použít PHP tooquery Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="b8386-103">Use PHP tooquery an Azure SQL database</span></span>

<span data-ttu-id="b8386-104">Tento úvodní kurz ukazuje, jak toouse [PHP](http://php.net/manual/en/intro-whatis.php) toocreate tooan tooconnect programu Azure SQL databáze a používat data tooquery příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8386-104">This quick start tutorial demonstrates how toouse [PHP](http://php.net/manual/en/intro-whatis.php) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8386-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8386-105">Prerequisites</span></span>

<span data-ttu-id="b8386-106">toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="b8386-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="b8386-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="b8386-107">An Azure SQL database.</span></span> <span data-ttu-id="b8386-108">Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="b8386-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="b8386-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="b8386-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="b8386-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b8386-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="b8386-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8386-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="b8386-112">A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.</span><span class="sxs-lookup"><span data-stu-id="b8386-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="b8386-113">Máte nainstalovaný jazyk PHP a související software pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="b8386-113">You have installed PHP and related software for your operating system.</span></span>

    - <span data-ttu-id="b8386-114">**Systému MacOS**: Nainstalujte Homebrew a PHP, nainstalujte ovladač ODBC hello a SQLCMD a pak nainstalujte hello ovladače PHP pro SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b8386-114">**MacOS**: Install Homebrew and PHP, install hello ODBC driver and SQLCMD, and then install hello PHP Driver for SQL Server.</span></span> <span data-ttu-id="b8386-115">Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span><span class="sxs-lookup"><span data-stu-id="b8386-115">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span></span>
    - <span data-ttu-id="b8386-116">**Ubuntu**: instalace PHP a dalších požadované balíčky a pak instalaci hello ovladače PHP pro SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b8386-116">**Ubuntu**:  Install PHP and other required packages, and then install hello PHP Driver for SQL Server.</span></span> <span data-ttu-id="b8386-117">Viz [kroky 1.2 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="b8386-117">See [Steps 1.2 and 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span></span>
    - <span data-ttu-id="b8386-118">**Windows**: Nainstalujte hello nejnovější verzi PHP pro službu IIS Express, hello nejnovější verzi Drivers společnosti Microsoft pro systém SQL Server v IIS Express, Chocolatey, ovladač ODBC hello a SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="b8386-118">**Windows**: Install hello newest version of PHP for IIS Express, hello newest version of Microsoft Drivers for SQL Server in IIS Express, Chocolatey, hello ODBC driver, and SQLCMD.</span></span> <span data-ttu-id="b8386-119">Viz [kroky 1.2 a 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span><span class="sxs-lookup"><span data-stu-id="b8386-119">See [Steps 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="b8386-120">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="b8386-120">SQL server connection information</span></span>

<span data-ttu-id="b8386-121">Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="b8386-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="b8386-122">Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="b8386-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="b8386-123">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b8386-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b8386-124">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="b8386-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="b8386-125">Na hello **přehled** stránky pro vaši databázi hello zkontrolujte plně kvalifikovaný název serveru, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="b8386-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="b8386-126">Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost.</span><span class="sxs-lookup"><span data-stu-id="b8386-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="b8386-128">Pokud jste zapomněli vaše přihlašovací údaje serveru, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název nebo, v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="b8386-128">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>     
    
## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="b8386-129">Vložení kódu tooquery SQL database</span><span class="sxs-lookup"><span data-stu-id="b8386-129">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="b8386-130">V oblíbeném textovém editoru vytvořte nový soubor **sqltest.php**.</span><span class="sxs-lookup"><span data-stu-id="b8386-130">In your favorite text editor, create a new file, **sqltest.php**.</span></span>  

2. <span data-ttu-id="b8386-131">Nahraďte obsah hello hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="b8386-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes hello connection
   $conn = sqlsrv_connect($serverName, $connectionOptions);
   $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";
   $getResults= sqlsrv_query($conn, $tsql);
   echo ("Reading data from table" . PHP_EOL);
   if ($getResults == FALSE)
       echo (sqlsrv_errors());
   while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
   }
   sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-hello-code"></a><span data-ttu-id="b8386-132">Spuštění kódu hello</span><span class="sxs-lookup"><span data-stu-id="b8386-132">Run hello code</span></span>

1. <span data-ttu-id="b8386-133">Hello příkazového řádku spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="b8386-133">At hello command prompt, run hello following commands:</span></span>

   ```php
   php sqltest.php
   ```

2. <span data-ttu-id="b8386-134">Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b8386-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8386-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8386-135">Next steps</span></span>
- [<span data-ttu-id="b8386-136">Návrh první databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="b8386-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="b8386-137">Ovladače Microsoft PHP pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="b8386-137">Microsoft PHP Drivers for SQL Server</span></span>](https://github.com/Microsoft/msphpsql/)
- [<span data-ttu-id="b8386-138">Hlášení problémů nebo kladení dotazů</span><span class="sxs-lookup"><span data-stu-id="b8386-138">Report issues or ask questions</span></span>](https://github.com/Microsoft/msphpsql/issues)
