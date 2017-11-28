---
title: aaaUse Ruby tooquery Azure SQL Database | Microsoft Docs
description: "Toto téma ukazuje, jak toouse Ruby toocreate program, který se připojuje tooan Azure SQL Database a dotazování pomocí příkazů Transact-SQL."
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
ms.openlocfilehash: 0d4b16b8aacb5e376ab80cbe37569130f2fd52b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-ruby-tooquery-an-azure-sql-database"></a><span data-ttu-id="55686-103">Použití poznámek Ruby tooquery Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="55686-103">Use Ruby tooquery an Azure SQL database</span></span>

<span data-ttu-id="55686-104">Tento úvodní kurz ukazuje, jak toouse [Ruby](https://www.ruby-lang.org) toocreate tooan tooconnect programu Azure SQL databáze a používat data tooquery příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="55686-104">This quick start tutorial demonstrates how toouse [Ruby](https://www.ruby-lang.org) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55686-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="55686-105">Prerequisites</span></span>

<span data-ttu-id="55686-106">toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="55686-106">toocomplete this quick start tutorial, make sure you have hello following prerequisites:</span></span>

- <span data-ttu-id="55686-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="55686-107">An Azure SQL database.</span></span> <span data-ttu-id="55686-108">Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="55686-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="55686-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="55686-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="55686-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="55686-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="55686-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="55686-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="55686-112">A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.</span><span class="sxs-lookup"><span data-stu-id="55686-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="55686-113">Máte nainstalované Ruby a související software pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="55686-113">You have installed Ruby and related software for your operating system.</span></span>
    - <span data-ttu-id="55686-114">**MacOS:** Nainstalujte Homebrew, nainstalujte rbenv a ruby-build, nainstalujte Ruby a potom nainstalujte FreeTDS.</span><span class="sxs-lookup"><span data-stu-id="55686-114">**MacOS**: Install Homebrew, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="55686-115">Viz [kroky 1.2, 1.3, 1.4 a 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span><span class="sxs-lookup"><span data-stu-id="55686-115">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span></span>
    - <span data-ttu-id="55686-116">**Ubuntu:** Nainstalujte požadavky pro Ruby, nainstalujte rbenv a ruby-build, nainstalujte Ruby a potom nainstalujte FreeTDS.</span><span class="sxs-lookup"><span data-stu-id="55686-116">**Ubuntu**: Install prerequisites for Ruby, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="55686-117">Viz [kroky 1.2, 1.3, 1.4 a 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="55686-117">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="55686-118">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="55686-118">SQL server connection information</span></span>

<span data-ttu-id="55686-119">Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="55686-119">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="55686-120">Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="55686-120">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="55686-121">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="55686-121">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="55686-122">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="55686-122">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="55686-123">Na hello **přehled** pro vaši databázi si prohlédněte hello serveru plně kvalifikovaný název.</span><span class="sxs-lookup"><span data-stu-id="55686-123">On hello **Overview** page for your database, review hello fully qualified server name.</span></span> <span data-ttu-id="55686-124">Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="55686-124">You can hover over hello server name toobring up hello **Click toocopy** option, as shown in hello following image:</span></span>

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="55686-126">Pokud jste zapomněli hello přihlašovací informace pro váš server databáze SQL Azure, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název a, v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="55686-126">If you have forgotten hello login information for your Azure SQL Database server, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="55686-127">Pravidlo brány firewall musí mít zavedené hello veřejných IP adres hello počítače, na kterém provádíte tento kurz.</span><span class="sxs-lookup"><span data-stu-id="55686-127">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="55686-128">Pokud jsou v jiném počítači nebo mít jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí portálu Azure hello](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="55686-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="55686-129">Vložení kódu tooquery SQL database</span><span class="sxs-lookup"><span data-stu-id="55686-129">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="55686-130">V oblíbeném textovém editoru vytvořte nový soubor **sqltest.rb**.</span><span class="sxs-lookup"><span data-stu-id="55686-130">In your favorite text editor, create a new file, **sqltest.rb**</span></span>

2. <span data-ttu-id="55686-131">Nahraďte obsah hello hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="55686-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

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

## <a name="run-hello-code"></a><span data-ttu-id="55686-132">Spuštění kódu hello</span><span class="sxs-lookup"><span data-stu-id="55686-132">Run hello code</span></span>

1. <span data-ttu-id="55686-133">Hello příkazového řádku spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="55686-133">At hello command prompt, run hello following commands:</span></span>

   ```bash
   ruby sqltest.rb
   ```

2. <span data-ttu-id="55686-134">Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="55686-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="55686-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55686-135">Next Steps</span></span>
- [<span data-ttu-id="55686-136">Návrh první databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="55686-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="55686-137">Úložiště GitHub pro TinyTDS</span><span class="sxs-lookup"><span data-stu-id="55686-137">GitHub repository for TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds)
- [<span data-ttu-id="55686-138">Hlášení problémů nebo kladení dotazů ohledně TinyTDS</span><span class="sxs-lookup"><span data-stu-id="55686-138">Report issues or ask questions about TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds/issues)
- [<span data-ttu-id="55686-139">Ovladače Ruby pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="55686-139">Ruby Drivers for SQL Server</span></span>](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
