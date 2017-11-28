---
title: "Připojit tooAzure databáze pro databázi MySQL z Pythonu | Microsoft Docs"
description: "Tento rychlý start poskytuje že několik Python code ukázky tooconnect a dotaz data z databáze Azure můžete použít pro databázi MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: python
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: 9df5211adcab886a502fd138347aed8fb587cd5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-python-tooconnect-and-query-data"></a><span data-ttu-id="148f6-103">Azure databáze pro databázi MySQL: použití Python tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="148f6-103">Azure Database for MySQL: Use Python tooconnect and query data</span></span>
<span data-ttu-id="148f6-104">Tento rychlý start předvádí jak toouse [Python](https://python.org) tooconnect tooan Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-104">This quickstart demonstrates how toouse [Python](https://python.org) tooconnect tooan Azure Database for MySQL.</span></span> <span data-ttu-id="148f6-105">Používá tooquery příkazy SQL, vložení, aktualizace a odstranění dat v databázi hello z platformy Mac OS, Ubuntu Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="148f6-105">It uses SQL statements tooquery, insert, update, and delete data in hello database from Mac OS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="148f6-106">Hello kroky v tomto článku Předpokládejme, že jsou obeznámeni s vývojem pomocí Pythonu a jsou nové tooworking s Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-106">hello steps in this article assume that you are familiar with developing using Python and are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="148f6-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="148f6-107">Prerequisites</span></span>
<span data-ttu-id="148f6-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="148f6-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="148f6-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="148f6-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="148f6-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="148f6-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-hello-mysql-connector"></a><span data-ttu-id="148f6-111">Instalace jazyka Python a hello MySQL konektoru</span><span class="sxs-lookup"><span data-stu-id="148f6-111">Install Python and hello MySQL connector</span></span>
<span data-ttu-id="148f6-112">Nainstalujte [Python](https://www.python.org/downloads/) a hello [MySQL konektor pro jazyk Python](https://dev.mysql.com/downloads/connector/python/) na vlastním počítači.</span><span class="sxs-lookup"><span data-stu-id="148f6-112">Install [Python](https://www.python.org/downloads/) and hello [MySQL connector for Python](https://dev.mysql.com/downloads/connector/python/) on your own machine.</span></span> <span data-ttu-id="148f6-113">V závislosti na platformě postupujte podle kroků hello:</span><span class="sxs-lookup"><span data-stu-id="148f6-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="148f6-114">Windows</span><span class="sxs-lookup"><span data-stu-id="148f6-114">Windows</span></span>
1. <span data-ttu-id="148f6-115">Z webu [python.org](https://www.python.org/downloads/windows/) stáhněte a nainstalujte Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="148f6-115">Download and Install Python 2.7 from [python.org](https://www.python.org/downloads/windows/).</span></span> 
2. <span data-ttu-id="148f6-116">Zkontrolujte hello Python instalaci spuštěním hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="148f6-116">Check hello Python installation by launching hello command prompt.</span></span> <span data-ttu-id="148f6-117">Spusťte příkaz hello `C:\python27\python.exe -V` pomocí hello velká písmena V přepínači toosee hello číslo verze.</span><span class="sxs-lookup"><span data-stu-id="148f6-117">Run hello command `C:\python27\python.exe -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="148f6-118">Instalace konektoru hello Python pro databázi MySQL z [mysql.com](https://dev.mysql.com/downloads/connector/python/) odpovídající tooyour verzi jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="148f6-118">Install hello Python connector for MySQL from [mysql.com](https://dev.mysql.com/downloads/connector/python/) corresponding tooyour version of Python.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="148f6-119">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="148f6-119">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="148f6-120">V systému Linux (Ubuntu) je obvykle Python nainstalována jako součást instalace výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-120">In Linux (Ubuntu), Python is typically installed as part of hello default installation.</span></span>
2. <span data-ttu-id="148f6-121">Zkontrolujte hello Python instalaci spuštěním prostředí bash hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-121">Check hello Python installation by launching hello bash shell.</span></span> <span data-ttu-id="148f6-122">Spusťte příkaz hello `python -V` pomocí hello velká písmena V přepínači toosee hello číslo verze.</span><span class="sxs-lookup"><span data-stu-id="148f6-122">Run hello command `python -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="148f6-123">Zkontrolujte hello PIP instalaci spuštěním hello `pip show pip -V` příkaz číslo verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-123">Check hello PIP installation by running hello `pip show pip -V` command toosee hello version number.</span></span> 
4. <span data-ttu-id="148f6-124">PIP může být součástí některých verzí Pythonu.</span><span class="sxs-lookup"><span data-stu-id="148f6-124">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="148f6-125">Pokud není nainstalovaná PIP, nainstalujete hello [PIP] (https://pip.pypa.io/en/stable/installing/) balíčku, spuštěním příkazu `sudo apt-get install python-pip`.</span><span class="sxs-lookup"><span data-stu-id="148f6-125">If PIP is not installed, you may install hello [PIP] (https://pip.pypa.io/en/stable/installing/) package, by running command `sudo apt-get install python-pip`.</span></span>
5. <span data-ttu-id="148f6-126">Aktualizace PIP toohello nejnovější verzi, spuštěním hello `pip install -U pip` příkaz.</span><span class="sxs-lookup"><span data-stu-id="148f6-126">Update PIP toohello latest version, by running hello `pip install -U pip` command.</span></span>
6. <span data-ttu-id="148f6-127">Instalace konektoru hello MySQL pro Python a jeho závislosti pomocí příkazu PIP hello:</span><span class="sxs-lookup"><span data-stu-id="148f6-127">Install hello MySQL connector for Python, and its dependencies by using hello PIP command:</span></span>

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a><span data-ttu-id="148f6-128">MacOS</span><span class="sxs-lookup"><span data-stu-id="148f6-128">MacOS</span></span>
1. <span data-ttu-id="148f6-129">V systému Mac OS je obvykle Python nainstalována jako součást instalace operačního systému výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-129">In Mac OS, Python is typically installed as part of hello default OS installation.</span></span>
2. <span data-ttu-id="148f6-130">Zkontrolujte hello Python instalaci spuštěním prostředí bash hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-130">Check hello Python installation by launching hello bash shell.</span></span> <span data-ttu-id="148f6-131">Spusťte příkaz hello `python -V` pomocí hello velká písmena V přepínači toosee hello číslo verze.</span><span class="sxs-lookup"><span data-stu-id="148f6-131">Run hello command `python -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="148f6-132">Zkontrolujte hello PIP instalaci spuštěním hello `pip show pip -V` příkaz číslo verze toosee hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-132">Check hello PIP installation by running hello `pip show pip -V` command toosee hello version number.</span></span>
4. <span data-ttu-id="148f6-133">PIP může být součástí některých verzí Pythonu.</span><span class="sxs-lookup"><span data-stu-id="148f6-133">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="148f6-134">Pokud není nainstalovaná PIP, nainstalujete hello [PIP](https://pip.pypa.io/en/stable/installing/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="148f6-134">If PIP is not installed, you may install hello [PIP](https://pip.pypa.io/en/stable/installing/) package.</span></span>
5. <span data-ttu-id="148f6-135">Aktualizace PIP toohello nejnovější verzi, spuštěním hello `pip install -U pip` příkaz.</span><span class="sxs-lookup"><span data-stu-id="148f6-135">Update PIP toohello latest version, by running hello `pip install -U pip` command.</span></span>
6. <span data-ttu-id="148f6-136">Instalace konektoru hello MySQL pro Python a jeho závislosti pomocí příkazu PIP hello:</span><span class="sxs-lookup"><span data-stu-id="148f6-136">Install hello MySQL connector for Python, and its dependencies by using hello PIP command:</span></span>

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a><span data-ttu-id="148f6-137">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="148f6-137">Get connection information</span></span>
<span data-ttu-id="148f6-138">Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-138">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="148f6-139">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="148f6-139">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="148f6-140">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="148f6-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="148f6-141">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru můžete mít pokrčené, jako například **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="148f6-141">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="148f6-142">Klikněte na název serveru hello **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="148f6-142">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="148f6-143">Vyberte hello serveru **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="148f6-143">Select hello server's **Properties** page.</span></span> <span data-ttu-id="148f6-144">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="148f6-144">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="148f6-145">![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-python/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="148f6-145">![Azure Database for MySQL - Server Admin Login](./media/connect-python/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="148f6-146">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-146">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>
   

## <a name="run-python-code"></a><span data-ttu-id="148f6-147">Spuštění kódu Pythonu</span><span class="sxs-lookup"><span data-stu-id="148f6-147">Run Python Code</span></span>
- <span data-ttu-id="148f6-148">Vložte kód hello do textového souboru a hello soubor uložte do složky projektu se soubor .py rozšíření, jako je například C:\pythonmysql\createtable.py nebo /home/username/pythonmysql/createtable.py</span><span class="sxs-lookup"><span data-stu-id="148f6-148">Paste hello code into a text file, and save hello file into a project folder with file extension .py, such as C:\pythonmysql\createtable.py or /home/username/pythonmysql/createtable.py</span></span>
- <span data-ttu-id="148f6-149">Kód hello toorun, spusťte příkazový řádek hello nebo bash prostředí.</span><span class="sxs-lookup"><span data-stu-id="148f6-149">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="148f6-150">Změňte adresář na složku vašeho projektu pomocí příkazu `cd pythonmysql`.</span><span class="sxs-lookup"><span data-stu-id="148f6-150">Change directory into your project folder `cd pythonmysql`.</span></span> <span data-ttu-id="148f6-151">Zadejte příkaz python hello následuje název souboru hello `python createtable.py` toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="148f6-151">Then type hello python command followed by hello file name `python createtable.py` toorun hello application.</span></span> <span data-ttu-id="148f6-152">Na hello operačního systému Windows Pokud není nalezen python.exe, můžete poskytnout hello úplná cesta toohello spustitelný soubor, nebo přidat cestu Python hello do proměnné prostředí path hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-152">On hello Windows OS, if python.exe is not found, you may provide hello full path toohello executable, or add hello Python path into hello path environment variable.</span></span> `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="148f6-153">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="148f6-153">Connect, create table, and insert data</span></span>
<span data-ttu-id="148f6-154">Použití hello následující kód serveru toohello tooconnect, vytvořit tabulku a načtení dat pomocí hello **vložit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-154">Use hello following code tooconnect toohello server, create a table, and load hello data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="148f6-155">V kódu hello hello mysql.connector knihovny importu.</span><span class="sxs-lookup"><span data-stu-id="148f6-155">In hello code, hello mysql.connector library is imported.</span></span> <span data-ttu-id="148f6-156">Hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funkce je použité tooconnect tooAzure databáze pro databázi MySQL pomocí hello [připojení argumenty](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) v kolekci konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-156">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="148f6-157">Kód Hello používá kurzor na hello připojení, a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoda provádí hello dotaz SQL na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-157">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="148f6-158">Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="148f6-158">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Drop previous table of same name if one exists
  cursor.execute("DROP TABLE IF EXISTS inventory;")
  print("Finished dropping table (if existed).")

  # Create table
  cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
  print("Finished creating table.")

  # Insert some data into table
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
  print("Inserted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="read-data"></a><span data-ttu-id="148f6-159">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="148f6-159">Read data</span></span>
<span data-ttu-id="148f6-160">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-160">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="148f6-161">V kódu hello hello mysql.connector knihovny importu.</span><span class="sxs-lookup"><span data-stu-id="148f6-161">In hello code, hello mysql.connector library is imported.</span></span> <span data-ttu-id="148f6-162">Hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funkce je použité tooconnect tooAzure databáze pro databázi MySQL pomocí hello [připojení argumenty](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) v kolekci konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-162">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="148f6-163">Kód Hello používá kurzor na hello připojení, a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoda provádí hello příkazu SQL na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-163">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL statement against MySQL database.</span></span> <span data-ttu-id="148f6-164">řádky dat Hello se načítají pomocí hello [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) metoda.</span><span class="sxs-lookup"><span data-stu-id="148f6-164">hello data rows are read using hello [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) method.</span></span> <span data-ttu-id="148f6-165">Hello sadu výsledků dotazu je udržována v řádku, kolekce a iterator je použité tooloop přes hello řádků.</span><span class="sxs-lookup"><span data-stu-id="148f6-165">hello result set is kept in a collection row and a for iterator is used tooloop over hello rows.</span></span>

<span data-ttu-id="148f6-166">Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="148f6-166">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Read data
  cursor.execute("SELECT * FROM inventory;")
  rows = cursor.fetchall()
  print("Read",cursor.rowcount,"row(s) of data.")

  # Print all rows
  for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="update-data"></a><span data-ttu-id="148f6-167">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="148f6-167">Update data</span></span>
<span data-ttu-id="148f6-168">Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-168">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="148f6-169">V kódu hello hello mysql.connector knihovny importu.</span><span class="sxs-lookup"><span data-stu-id="148f6-169">In hello code, hello mysql.connector library is imported.</span></span>  <span data-ttu-id="148f6-170">Hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funkce je použité tooconnect tooAzure databáze pro databázi MySQL pomocí hello [připojení argumenty](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) v kolekci konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-170">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="148f6-171">Kód Hello používá kurzor na hello připojení, a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoda provádí hello příkazu SQL na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-171">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL statement against MySQL database.</span></span> 

<span data-ttu-id="148f6-172">Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="148f6-172">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in hello table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a><span data-ttu-id="148f6-173">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="148f6-173">Delete data</span></span>
<span data-ttu-id="148f6-174">Použití hello následující kód tooconnect a odebrat pomocí data **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-174">Use hello following code tooconnect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="148f6-175">V kódu hello hello mysql.connector knihovny importu.</span><span class="sxs-lookup"><span data-stu-id="148f6-175">In hello code, hello mysql.connector library is imported.</span></span>  <span data-ttu-id="148f6-176">Hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funkce je použité tooconnect tooAzure databáze pro databázi MySQL pomocí hello [připojení argumenty](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) v kolekci konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="148f6-176">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="148f6-177">Kód Hello používá kurzor na hello připojení, a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoda provádí hello dotaz SQL na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="148f6-177">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="148f6-178">Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="148f6-178">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established.")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in hello table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a><span data-ttu-id="148f6-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="148f6-179">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="148f6-180">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="148f6-180">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
