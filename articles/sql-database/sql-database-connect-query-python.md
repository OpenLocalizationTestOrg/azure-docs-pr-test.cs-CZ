---
title: aaaUse Python tooquery Azure SQL Database | Microsoft Docs
description: "Toto téma ukazuje, jak toouse Python toocreate program, který se připojuje tooan Azure SQL Database a dotazování pomocí příkazů Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 6c786e7a61f5ed3e7d2395da59acfeae008e90e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-python-tooquery-an-azure-sql-database"></a><span data-ttu-id="010e1-103">Použít Python tooquery Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="010e1-103">Use Python tooquery an Azure SQL database</span></span>

 <span data-ttu-id="010e1-104">Tento rychlý start předvádí, jak toouse [Python](https://python.org) tooconnect tooan Azure SQL databáze a používat data tooquery příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="010e1-104">This quick start demonstrates how toouse [Python](https://python.org) tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="010e1-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="010e1-105">Prerequisites</span></span>

<span data-ttu-id="010e1-106">toocomplete tento rychlý úvodní kurz, ujistěte se, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="010e1-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="010e1-107">Databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="010e1-107">An Azure SQL database.</span></span> <span data-ttu-id="010e1-108">Tento rychlý start používá hello prostředky vytvořené v jednom z těchto rychlé spuštění:</span><span class="sxs-lookup"><span data-stu-id="010e1-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="010e1-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="010e1-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="010e1-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="010e1-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="010e1-111">Vytvoření databáze – PowerShell</span><span class="sxs-lookup"><span data-stu-id="010e1-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="010e1-112">A [pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro hello veřejnou IP adresu počítače hello použijete pro tento kurz rychlý start.</span><span class="sxs-lookup"><span data-stu-id="010e1-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="010e1-113">Máte nainstalovaný Python a související software pro váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="010e1-113">You have installed Python and related software for your operating system.</span></span>

    - <span data-ttu-id="010e1-114">**Systému MacOS**: nainstalovat Homebrew a Python, nainstalujte ovladač ODBC hello a SQLCMD a potom nainstalujte hello Python ovladač pro systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="010e1-114">**MacOS**: Install Homebrew and Python, install hello ODBC driver and SQLCMD, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="010e1-115">Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).</span><span class="sxs-lookup"><span data-stu-id="010e1-115">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).</span></span>
    - <span data-ttu-id="010e1-116">**Ubuntu**: nainstalovat Python a další požadované balíčky a pak hello instalaci ovladačů Python pro systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="010e1-116">**Ubuntu**:  Install Python and other required packages, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="010e1-117">Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="010e1-117">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).</span></span>
    - <span data-ttu-id="010e1-118">**Windows**: nainstalujte nejnovější verzi jazyka Python (proměnnou prostředí je nyní nakonfigurována pro vás) hello, nainstalujte ovladač ODBC hello a SQLCMD a pak nainstalujte hello Python ovladač pro systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="010e1-118">**Windows**: Install hello newest version of Python (environment variable is now configured for you), install hello ODBC driver and SQLCMD, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="010e1-119">Viz [kroky 1.2, 1.3 a 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/).</span><span class="sxs-lookup"><span data-stu-id="010e1-119">See [Step 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="010e1-120">Informace o připojení k SQL serveru</span><span class="sxs-lookup"><span data-stu-id="010e1-120">SQL server connection information</span></span>

<span data-ttu-id="010e1-121">Získáte hello připojení informace potřebné tooconnect toohello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="010e1-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="010e1-122">Budete potřebovat hello serveru plně kvalifikovaný název, název databáze a přihlašovacích údajů v dalším postupu hello.</span><span class="sxs-lookup"><span data-stu-id="010e1-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="010e1-123">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="010e1-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="010e1-124">Vyberte **databází SQL** z nabídky na levé straně hello a klikněte na tlačítko databáze na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="010e1-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="010e1-125">Na hello **přehled** stránky pro vaši databázi hello zkontrolujte plně kvalifikovaný název serveru, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="010e1-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="010e1-126">Můžete podržet přes toobring název serveru hello až hello **klikněte na tlačítko toocopy** možnost.</span><span class="sxs-lookup"><span data-stu-id="010e1-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="010e1-128">Pokud jste zapomněli vaše přihlašovací údaje serveru, přejděte toohello databáze SQL serveru stránky tooview hello serveru správce název nebo, v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="010e1-128">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>     
    
## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="010e1-129">Vložení kódu tooquery SQL database</span><span class="sxs-lookup"><span data-stu-id="010e1-129">Insert code tooquery SQL database</span></span> 

1. <span data-ttu-id="010e1-130">V oblíbeném textovém editoru vytvořte nový soubor **sqltest.py**.</span><span class="sxs-lookup"><span data-stu-id="010e1-130">In your favorite text editor, create a new file, **sqltest.py**.</span></span>  

2. <span data-ttu-id="010e1-131">Nahraďte obsah hello hello následující kód a přidat hello odpovídající hodnoty pro server, databáze, uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="010e1-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

## <a name="run-hello-code"></a><span data-ttu-id="010e1-132">Spuštění kódu hello</span><span class="sxs-lookup"><span data-stu-id="010e1-132">Run hello code</span></span>

1. <span data-ttu-id="010e1-133">Hello příkazového řádku spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="010e1-133">At hello command prompt, run hello following commands:</span></span>

   ```Python
   python sqltest.py
   ```

2. <span data-ttu-id="010e1-134">Ověřte, že horních řádků 20 hello se vrátí a pak zavřete okno aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="010e1-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="010e1-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="010e1-135">Next steps</span></span>

- [<span data-ttu-id="010e1-136">Návrh první databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="010e1-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="010e1-137">Ovladače Microsoft Python pro SQL Server</span><span class="sxs-lookup"><span data-stu-id="010e1-137">Microsoft Python Drivers for SQL Server</span></span>](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [<span data-ttu-id="010e1-138">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="010e1-138">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/?v=17.23h)

