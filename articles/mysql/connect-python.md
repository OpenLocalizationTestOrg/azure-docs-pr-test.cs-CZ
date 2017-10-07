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
# <a name="azure-database-for-mysql-use-python-tooconnect-and-query-data"></a>Azure databáze pro databázi MySQL: použití Python tooconnect a dotazování dat
Tento rychlý start předvádí jak toouse [Python](https://python.org) tooconnect tooan Azure Database pro databázi MySQL. Používá tooquery příkazy SQL, vložení, aktualizace a odstranění dat v databázi hello z platformy Mac OS, Ubuntu Linux a Windows. Hello kroky v tomto článku Předpokládejme, že jsou obeznámeni s vývojem pomocí Pythonu a jsou nové tooworking s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-hello-mysql-connector"></a>Instalace jazyka Python a hello MySQL konektoru
Nainstalujte [Python](https://www.python.org/downloads/) a hello [MySQL konektor pro jazyk Python](https://dev.mysql.com/downloads/connector/python/) na vlastním počítači. V závislosti na platformě postupujte podle kroků hello:

### <a name="windows"></a>Windows
1. Z webu [python.org](https://www.python.org/downloads/windows/) stáhněte a nainstalujte Python 2.7. 
2. Zkontrolujte hello Python instalaci spuštěním hello příkazového řádku. Spusťte příkaz hello `C:\python27\python.exe -V` pomocí hello velká písmena V přepínači toosee hello číslo verze.
3. Instalace konektoru hello Python pro databázi MySQL z [mysql.com](https://dev.mysql.com/downloads/connector/python/) odpovídající tooyour verzi jazyka Python.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. V systému Linux (Ubuntu) je obvykle Python nainstalována jako součást instalace výchozí hello.
2. Zkontrolujte hello Python instalaci spuštěním prostředí bash hello. Spusťte příkaz hello `python -V` pomocí hello velká písmena V přepínači toosee hello číslo verze.
3. Zkontrolujte hello PIP instalaci spuštěním hello `pip show pip -V` příkaz číslo verze toosee hello. 
4. PIP může být součástí některých verzí Pythonu. Pokud není nainstalovaná PIP, nainstalujete hello [PIP] (https://pip.pypa.io/en/stable/installing/) balíčku, spuštěním příkazu `sudo apt-get install python-pip`.
5. Aktualizace PIP toohello nejnovější verzi, spuštěním hello `pip install -U pip` příkaz.
6. Instalace konektoru hello MySQL pro Python a jeho závislosti pomocí příkazu PIP hello:

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a>MacOS
1. V systému Mac OS je obvykle Python nainstalována jako součást instalace operačního systému výchozí hello.
2. Zkontrolujte hello Python instalaci spuštěním prostředí bash hello. Spusťte příkaz hello `python -V` pomocí hello velká písmena V přepínači toosee hello číslo verze.
3. Zkontrolujte hello PIP instalaci spuštěním hello `pip show pip -V` příkaz číslo verze toosee hello.
4. PIP může být součástí některých verzí Pythonu. Pokud není nainstalovaná PIP, nainstalujete hello [PIP](https://pip.pypa.io/en/stable/installing/) balíčku.
5. Aktualizace PIP toohello nejnovější verzi, spuštěním hello `pip install -U pip` příkaz.
6. Instalace konektoru hello MySQL pro Python a jeho závislosti pomocí příkazu PIP hello:

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru můžete mít pokrčené, jako například **myserver4demo**.
3. Klikněte na název serveru hello **myserver4demo**.
4. Vyberte hello serveru **vlastnosti** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-python/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.
   

## <a name="run-python-code"></a>Spuštění kódu Pythonu
- Vložte kód hello do textového souboru a hello soubor uložte do složky projektu se soubor .py rozšíření, jako je například C:\pythonmysql\createtable.py nebo /home/username/pythonmysql/createtable.py
- Kód hello toorun, spusťte příkazový řádek hello nebo bash prostředí. Změňte adresář na složku vašeho projektu pomocí příkazu `cd pythonmysql`. Zadejte příkaz python hello následuje název souboru hello `python createtable.py` toorun hello aplikace. Na hello operačního systému Windows Pokud není nalezen python.exe, můžete poskytnout hello úplná cesta toohello spustitelný soubor, nebo přidat cestu Python hello do proměnné prostředí path hello. `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použití hello následující kód serveru toohello tooconnect, vytvořit tabulku a načtení dat pomocí hello **vložit** příkaz jazyka SQL. 

V kódu hello hello mysql.connector knihovny importu. Hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funkce je použité tooconnect tooAzure databáze pro databázi MySQL pomocí hello [připojení argumenty](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) v kolekci konfigurace hello. Kód Hello používá kurzor na hello připojení, a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoda provádí hello dotaz SQL na databázi MySQL. 

Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.

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

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. 

V kódu hello hello mysql.connector knihovny importu. Hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funkce je použité tooconnect tooAzure databáze pro databázi MySQL pomocí hello [připojení argumenty](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) v kolekci konfigurace hello. Kód Hello používá kurzor na hello připojení, a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoda provádí hello příkazu SQL na databázi MySQL. řádky dat Hello se načítají pomocí hello [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) metoda. Hello sadu výsledků dotazu je udržována v řádku, kolekce a iterator je použité tooloop přes hello řádků.

Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.

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

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL. 

V kódu hello hello mysql.connector knihovny importu.  Hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funkce je použité tooconnect tooAzure databáze pro databázi MySQL pomocí hello [připojení argumenty](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) v kolekci konfigurace hello. Kód Hello používá kurzor na hello připojení, a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoda provádí hello příkazu SQL na databázi MySQL. 

Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.

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

## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a odebrat pomocí data **odstranit** příkaz jazyka SQL. 

V kódu hello hello mysql.connector knihovny importu.  Hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funkce je použité tooconnect tooAzure databáze pro databázi MySQL pomocí hello [připojení argumenty](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) v kolekci konfigurace hello. Kód Hello používá kurzor na hello připojení, a [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoda provádí hello dotaz SQL na databázi MySQL. 

Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.

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

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./concepts-migrate-import-export.md)
