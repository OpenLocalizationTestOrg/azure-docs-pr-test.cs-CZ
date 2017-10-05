---
title: "Připojení k Azure Database for PostgreSQL z Pythonu | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód Pythonu, který můžete použít k připojení a dotazování dat ze služby Azure Database for PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: python
ms.topic: quickstart
ms.date: 08/15/2017
ms.openlocfilehash: d682d94143fb9fd5e2c2a578c3cb0dcfa101462c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-python-to-connect-and-query-data"></a><span data-ttu-id="364d8-103">Azure Database for PostgreSQL: Použití Pythonu k připojení a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="364d8-103">Azure Database for PostgreSQL: Use Python to connect and query data</span></span>
<span data-ttu-id="364d8-104">Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for PostgreSQL pomocí [Pythonu](https://python.org).</span><span class="sxs-lookup"><span data-stu-id="364d8-104">This quickstart demonstrates how to use [Python](https://python.org) to connect to an Azure Database for PostgreSQL.</span></span> <span data-ttu-id="364d8-105">Předvádí také použití příkazů jazyka SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem macOS, Ubuntu Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="364d8-105">It also demonstrates how to use SQL statements to query, insert, update, and delete data in the database from macOS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="364d8-106">Kroky v tomto článku předpokládají, že máte zkušenosti s vývojem pomocí Pythonu a teprve začínáte pracovat se službou Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="364d8-106">The steps in this article assume that you are familiar with developing using Python and are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="364d8-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="364d8-107">Prerequisites</span></span>
<span data-ttu-id="364d8-108">Tento rychlý start využívá jako výchozí bod prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="364d8-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="364d8-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="364d8-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="364d8-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="364d8-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="364d8-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="364d8-111">You also need:</span></span>
- <span data-ttu-id="364d8-112">Nainstalovat [Python](https://www.python.org/downloads/)</span><span class="sxs-lookup"><span data-stu-id="364d8-112">[python](https://www.python.org/downloads/) installed</span></span>
- <span data-ttu-id="364d8-113">Nainstalovat balíček [pip](https://pip.pypa.io/en/stable/installing/) (pokud pracujete s binárními soubory Pythonu 2 >= 2.7.9 nebo Pythonu 3 >= 3.4 stažené z webu [python.org](https://python.org), balíček pip už máte nainstalovaný).</span><span class="sxs-lookup"><span data-stu-id="364d8-113">[pip](https://pip.pypa.io/en/stable/installing/) package installed (pip is already installed if you're working with Python 2 >=2.7.9 or Python 3 >=3.4 binaries downloaded from [python.org](https://python.org).</span></span>

## <a name="install-the-python-connection-libraries-for-postgresql"></a><span data-ttu-id="364d8-114">Instalace knihoven připojení Pythonu pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="364d8-114">Install the Python connection libraries for PostgreSQL</span></span>
<span data-ttu-id="364d8-115">Nainstalujte balíček [psycopg2](http://initd.org/psycopg/docs/install.html) umožňující připojení a dotazování databáze.</span><span class="sxs-lookup"><span data-stu-id="364d8-115">Install the [psycopg2](http://initd.org/psycopg/docs/install.html) package, which enables you to connect and query the database.</span></span> <span data-ttu-id="364d8-116">Balíček psycopg2 je [dostupný na webu PyPI](https://pypi.python.org/pypi/psycopg2/) ve formě balíčků [wheel](http://pythonwheels.com/) pro většinu běžných platforem (Linux, OSX, Windows).</span><span class="sxs-lookup"><span data-stu-id="364d8-116">psycopg2 is [available on PyPI](https://pypi.python.org/pypi/psycopg2/) in the form of [wheel](http://pythonwheels.com/) packages for the most common platforms (Linux, OSX, Windows).</span></span> <span data-ttu-id="364d8-117">Pomocí příkazu pip install získejte binární verzi modulu včetně všech závislostí.</span><span class="sxs-lookup"><span data-stu-id="364d8-117">Use pip install to get the binary version of the module including all the dependencies.</span></span>

1. <span data-ttu-id="364d8-118">Na vlastním počítači spusťte rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="364d8-118">On your own computer, launch a command-line interface:</span></span>
    - <span data-ttu-id="364d8-119">V Linuxu spusťte prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="364d8-119">On Linux, launch the Bash shell.</span></span>
    - <span data-ttu-id="364d8-120">V systému macOS spusťte Terminál.</span><span class="sxs-lookup"><span data-stu-id="364d8-120">On macOS, launch the Terminal.</span></span>
    - <span data-ttu-id="364d8-121">Ve Windows spusťte Příkazový řádek z nabídky Start.</span><span class="sxs-lookup"><span data-stu-id="364d8-121">On Windows, launch the Command Prompt from the Start Menu.</span></span>
2. <span data-ttu-id="364d8-122">Ujistěte se, že používáte nejaktuálnější verzi pip, spuštěním příkazu jako:</span><span class="sxs-lookup"><span data-stu-id="364d8-122">Ensure that you are using the most current version of pip by running a command such as:</span></span>
    ```cmd
    pip install -U pip
    ```

3. <span data-ttu-id="364d8-123">Spuštěním následujícího příkazu nainstalujte balíček psycopg2:</span><span class="sxs-lookup"><span data-stu-id="364d8-123">Run the following command to install the psycopg2 package:</span></span>
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a><span data-ttu-id="364d8-124">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="364d8-124">Get connection information</span></span>
<span data-ttu-id="364d8-125">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="364d8-125">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="364d8-126">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="364d8-126">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="364d8-127">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="364d8-127">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="364d8-128">V nabídce na levé straně webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte **mypgserver-20170401** (právě vytvořený server).</span><span class="sxs-lookup"><span data-stu-id="364d8-128">From the left-hand menu in Azure portal, click **All resources** and search for **mypgserver-20170401** (the server you just created).</span></span>
3. <span data-ttu-id="364d8-129">Klikněte na název serveru **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="364d8-129">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="364d8-130">Vyberte stránku **Přehled** serveru a potom si poznamenejte **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="364d8-130">Select the server's **Overview** page, and then make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="364d8-131">![Azure Database for PostgreSQL – přihlašovací jméno správce serveru](./media/connect-python/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="364d8-131">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-python/1-connection-string.png)</span></span>
5. <span data-ttu-id="364d8-132">Pokud zapomenete přihlašovací údaje k serveru, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="364d8-132">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="how-to-run-python-code"></a><span data-ttu-id="364d8-133">Spuštění kódu Pythonu</span><span class="sxs-lookup"><span data-stu-id="364d8-133">How to run Python code</span></span>
<span data-ttu-id="364d8-134">Toto téma obsahuje celkem čtyři vzorové kódy, z nichž každý provádí konkrétní funkci.</span><span class="sxs-lookup"><span data-stu-id="364d8-134">This topic contains a total of four code samples, each of which performs a specific function.</span></span> <span data-ttu-id="364d8-135">Následující pokyny uvádějí, jak vytvořit textový soubor, vložit do něj blok kódu a pak ho uložit, abyste ho mohli spustit později.</span><span class="sxs-lookup"><span data-stu-id="364d8-135">The following instructions indicate how to create a text file, insert a code block, and then save the file so that you can run it later.</span></span> <span data-ttu-id="364d8-136">Nezapomeňte vytvořit čtyři samostatné soubory – pro každý blok kódu jeden.</span><span class="sxs-lookup"><span data-stu-id="364d8-136">Be sure to create four separate files, one for each code block.</span></span>

- <span data-ttu-id="364d8-137">Pomocí oblíbeného textového editoru vytvořte nový soubor.</span><span class="sxs-lookup"><span data-stu-id="364d8-137">Using your favorite text editor, create a new file.</span></span>
- <span data-ttu-id="364d8-138">Zkopírujte a vložte do textového souboru jeden ze vzorových kódů v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="364d8-138">Copy and paste one of the code samples in the following sections into the text file.</span></span> <span data-ttu-id="364d8-139">Nahraďte parametry **host** (hostitel), **dbname** (název databáze), **user** (uživatel) a **password** (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="364d8-139">Replace the **host**, **dbname**, **user**, and **password** parameters with the values that you specified when you created the server and database.</span></span>
- <span data-ttu-id="364d8-140">Uložte soubor s příponou .py (třeba postgres.py) do složky vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="364d8-140">Save the file with the .py extension (for example postgres.py) into your project folder.</span></span> <span data-ttu-id="364d8-141">Pokud používáte operační systém Windows, nezapomeňte při ukládání souboru vybrat kódování UTF-8.</span><span class="sxs-lookup"><span data-stu-id="364d8-141">If you are running the Windows OS, be sure to select UTF-8 encoding when saving the file.</span></span> 
- <span data-ttu-id="364d8-142">Spusťte Příkazový řádek nebo prostředí Bash a změňte adresář na složku vašeho projektu, například `cd postgres`.</span><span class="sxs-lookup"><span data-stu-id="364d8-142">Launch the Command Prompt or Bash shell and then change the directory to your project folder, for example `cd postgres`.</span></span>
-  <span data-ttu-id="364d8-143">Pokud chcete spustit kód, zadejte příkaz Python následovaný názvem souboru, například `Python postgres.py`.</span><span class="sxs-lookup"><span data-stu-id="364d8-143">To run the code, type the Python command followed by the file name, for example `Python postgres.py`.</span></span>

> [!NOTE]
> <span data-ttu-id="364d8-144">Od Pythonu verze 3 se při spouštění následujících bloků kódu může zobrazit chyba `SyntaxError: Missing parentheses in call to 'print'`.</span><span class="sxs-lookup"><span data-stu-id="364d8-144">Starting in Python version 3, you may see the error `SyntaxError: Missing parentheses in call to 'print'` when running the following code blocks.</span></span> <span data-ttu-id="364d8-145">Pokud k tomu dojde, nahraďte všechna volání příkazu `print "string"` za volání funkce použitím závorek, například `print("string")`.</span><span class="sxs-lookup"><span data-stu-id="364d8-145">If that happens, replace each call to the command `print "string"` with a function call using parenthesis, such as `print("string")`.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="364d8-146">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="364d8-146">Connect, create table, and insert data</span></span>
<span data-ttu-id="364d8-147">Pomocí následujícího kódu se připojte a načtěte data s využitím funkce [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) a příkazu **INSERT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="364d8-147">Use the following code to connect and load the data using [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) function with **INSERT** SQL statement.</span></span> <span data-ttu-id="364d8-148">Funkce [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) se používá k provedení dotazu SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="364d8-148">The [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function is used to execute the SQL query against PostgreSQL database.</span></span> <span data-ttu-id="364d8-149">Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="364d8-149">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print "Finished dropping table (if existed)"

# Create table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print "Finished creating table"

# Insert some data into table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print "Inserted 3 rows of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

<span data-ttu-id="364d8-150">Po úspěšném spuštění kódu se zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="364d8-150">After the code runs successfully, the output appears as follows:</span></span>

![Výstup příkazového řádku](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a><span data-ttu-id="364d8-152">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="364d8-152">Read data</span></span>
<span data-ttu-id="364d8-153">Pomocí následujícího kódu přečtěte data vložená pomocí funkce [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) a příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="364d8-153">Use the following code to read the data inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **SELECT** SQL statement.</span></span> <span data-ttu-id="364d8-154">Tato funkce přijme dotaz a vrátí sadu výsledků, u které můžete provést iteraci pomocí funkce [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).</span><span class="sxs-lookup"><span data-stu-id="364d8-154">This function accepts a query and returns a result set that can be iterated over with the use of [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).</span></span> <span data-ttu-id="364d8-155">Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="364d8-155">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print "Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2]))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a><span data-ttu-id="364d8-156">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="364d8-156">Update data</span></span>
<span data-ttu-id="364d8-157">Pomocí následujícího kódu aktualizujte řádek inventáře, který jste předtím vložili pomocí funkce [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) a příkazu **UPDATE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="364d8-157">Use the following code to update the inventory row that you previously inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **UPDATE** SQL statement.</span></span> <span data-ttu-id="364d8-158">Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="364d8-158">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a><span data-ttu-id="364d8-159">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="364d8-159">Delete data</span></span>
<span data-ttu-id="364d8-160">Pomocí následujícího kódu odstraňte položku inventáře, kterou jste předtím vložili pomocí funkce [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) a příkazu **DELETE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="364d8-160">Use the following code to delete an inventory item that you previously inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **DELETE** SQL statement.</span></span> <span data-ttu-id="364d8-161">Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="364d8-161">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print "Deleted 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a><span data-ttu-id="364d8-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="364d8-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="364d8-163">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="364d8-163">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
