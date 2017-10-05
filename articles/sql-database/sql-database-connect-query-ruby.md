---
title: "Použití Ruby k dotazování služby Azure SQL Database | Dokumentace Microsoftu"
description: "Toto téma vám ukáže, jak pomocí Ruby vytvořit program, který se připojí ke službě Azure SQL Database a bude ji dotazovat s použitím příkazů jazyka Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: carlrab
ms.openlocfilehash: 25ff9a9cfaa5494dbb006c84e235099fe51e6545
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-ruby-to-query-an-azure-sql-database"></a><span data-ttu-id="3f6bd-103">Použití Ruby k dotazování na službu Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3f6bd-103">Use Ruby to query an Azure SQL database</span></span>

<span data-ttu-id="3f6bd-104">Tento rychlý úvodní kurz ukazuje použití [Ruby](https://www.ruby-lang.org) k vytvoření programu pro připojení ke službě Azure SQL Database a použití příkazů jazyka Transact-SQL k dotazování dat.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-104">This quick start tutorial demonstrates how to use [Ruby](https://www.ruby-lang.org) to create a program to connect to an Azure SQL database and use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f6bd-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3f6bd-105">Prerequisites</span></span>

<span data-ttu-id="3f6bd-106">Abyste mohli absolvovat tento rychlý úvodní kurz, ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="3f6bd-106">To complete this quick start tutorial, make sure you have the following prerequisites:</span></span>

- <span data-ttu-id="3f6bd-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-107">An Azure SQL database.</span></span> <span data-ttu-id="3f6bd-108">Tento rychlý start používá prostředky vytvořené v některém z těchto rychlých startů:</span><span class="sxs-lookup"><span data-stu-id="3f6bd-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="3f6bd-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="3f6bd-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="3f6bd-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="3f6bd-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="3f6bd-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f6bd-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="3f6bd-112">[Pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro veřejnou IP adresu počítače, který používáte pro tento rychlý úvodní kurz.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="3f6bd-113">Máte nainstalované Ruby a související software pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-113">You have installed Ruby and related software for your operating system.</span></span>
    - <span data-ttu-id="3f6bd-114">**MacOS:** Nainstalujte Homebrew, nainstalujte rbenv a ruby-build, nainstalujte Ruby a potom nainstalujte FreeTDS.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-114">**MacOS**: Install Homebrew, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="3f6bd-115">Viz [kroky 1.2, 1.3, 1.4 a 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span><span class="sxs-lookup"><span data-stu-id="3f6bd-115">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span></span>
    - <span data-ttu-id="3f6bd-116">**Ubuntu:** Nainstalujte požadavky pro Ruby, nainstalujte rbenv a ruby-build, nainstalujte Ruby a potom nainstalujte FreeTDS.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-116">**Ubuntu**: Install prerequisites for Ruby, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="3f6bd-117">Viz [kroky 1.2, 1.3, 1.4 a 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="3f6bd-117">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="3f6bd-118">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="3f6bd-118">SQL server connection information</span></span>

<span data-ttu-id="3f6bd-119">Získejte informace o připojení potřebné pro připojení k databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-119">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="3f6bd-120">V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-120">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="3f6bd-121">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3f6bd-121">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="3f6bd-122">V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-122">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="3f6bd-123">Na stránce **Přehled** pro vaši databázi zkontrolujte plně kvalifikovaný název serveru.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-123">On the **Overview** page for your database, review the fully qualified server name.</span></span> <span data-ttu-id="3f6bd-124">Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="3f6bd-124">You can hover over the server name to bring up the **Click to copy** option, as shown in the following image:</span></span>

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="3f6bd-126">Pokud jste zapomněli přihlašovací informace pro váš server Azure SQL Database, přejděte na stránku serveru SQL Database, abyste zobrazili jméno správce serveru a v případě potřeby resetovali heslo.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-126">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f6bd-127">Musíte mít nastavené pravidlo brány firewall pro veřejnou IP adresu počítače, na kterém provádíte tento kurz.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-127">You must have a firewall rule in place for the public IP address of the computer on which you perform this tutorial.</span></span> <span data-ttu-id="3f6bd-128">Pokud jste na jiném počítači nebo máte jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí webu Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="3f6bd-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using the Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="3f6bd-129">Vložení kódu pro dotazování databáze SQL</span><span class="sxs-lookup"><span data-stu-id="3f6bd-129">Insert code to query SQL database</span></span>

1. <span data-ttu-id="3f6bd-130">V oblíbeném textovém editoru vytvořte nový soubor **sqltest.rb**.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-130">In your favorite text editor, create a new file, **sqltest.rb**</span></span>

2. <span data-ttu-id="3f6bd-131">Nahraďte jeho obsah následujícím kódem a přidejte odpovídající hodnoty pro váš server, databázi, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-131">Replace the contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="run-the-code"></a><span data-ttu-id="3f6bd-132">Spuštění kódu</span><span class="sxs-lookup"><span data-stu-id="3f6bd-132">Run the code</span></span>

1. <span data-ttu-id="3f6bd-133">V příkazovém řádku spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="3f6bd-133">At the command prompt, run the following commands:</span></span>

   ```bash
   ruby sqltest.rb
   ```

2. <span data-ttu-id="3f6bd-134">Ověřte, že se vrátilo prvních 20 řádků, a potom zavřete okno aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f6bd-134">Verify that the top 20 rows are returned and then close the application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3f6bd-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f6bd-135">Next Steps</span></span>
- [<span data-ttu-id="3f6bd-136">Návrh první databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="3f6bd-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="3f6bd-137">Úložiště GitHub pro TinyTDS</span><span class="sxs-lookup"><span data-stu-id="3f6bd-137">GitHub repository for TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds)
- [<span data-ttu-id="3f6bd-138">Hlášení problémů nebo kladení dotazů ohledně TinyTDS</span><span class="sxs-lookup"><span data-stu-id="3f6bd-138">Report issues or ask questions about TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds/issues)
- [<span data-ttu-id="3f6bd-139">Ovladače Ruby pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="3f6bd-139">Ruby Drivers for SQL Server</span></span>](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
