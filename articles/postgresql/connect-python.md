---
title: "Připojení tooAzure databáze pro PostgreSQL z Pythonu | Microsoft Docs"
description: "Tento rychlý start poskytuje ukázka kódu Pythonu, můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro PostgreSQL."
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
ms.openlocfilehash: 7d6d9f5424fb39ad8837999d4788b4363c818887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-python-tooconnect-and-query-data"></a>Azure databázi PostgreSQL: použití Python tooconnect a dotazování dat
Tento rychlý start předvádí jak toouse [Python](https://python.org) tooconnect tooan databáze Azure pro PostgreSQL. Také ukazuje, jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello ze systému macOS, Ubuntu Linux a platformy systému Windows. Hello kroky v tomto článku Předpokládejme, že jsou obeznámeni s vývojem pomocí Pythonu a jsou nové tooworking s Azure databáze PostgreSQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](quickstart-create-server-database-azure-cli.md)

Budete také muset:
- Nainstalovat [Python](https://www.python.org/downloads/)
- Nainstalovat balíček [pip](https://pip.pypa.io/en/stable/installing/) (pokud pracujete s binárními soubory Pythonu 2 >= 2.7.9 nebo Pythonu 3 >= 3.4 stažené z webu [python.org](https://python.org), balíček pip už máte nainstalovaný).

## <a name="install-hello-python-connection-libraries-for-postgresql"></a>Instalace knihovny připojení hello Python pro PostgreSQL
Nainstalujte hello [psycopg2](http://initd.org/psycopg/docs/install.html) balíčku, což vám umožní tooconnect a dotaz hello databáze. je psycopg2 [k dispozici na úložiště PyPI](https://pypi.python.org/pypi/psycopg2/) hello tvar [kolečka](http://pythonwheels.com/) balíčky pro hello nejběžnější platformy (Linux, OSX, Windows). Použití pip nainstalovat tooget hello binární verze modulu hello včetně všechny závislosti hello.

1. Na vlastním počítači spusťte rozhraní příkazového řádku:
    - V systému Linux spusťte prostředí Bash hello.
    - V systému macOS spusťte hello terminálu.
    - V systému Windows spusťte příkazový řádek hello z hello nabídce Start.
2. Ujistěte se, že používáte nejnovější verzi pip hello spuštěním příkazu:
    ```cmd
    pip install -U pip
    ```

3. Spusťte následující příkaz tooinstall hello psycopg2 balíček hello:
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte **mypgserver 20170401** (server hello jste právě vytvořili).
3. Klikněte na název serveru hello **mypgserver 20170401**.
4. Vyberte hello serveru **přehled** stránky a poté si poznamenejte hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-python/1-connection-string.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="how-toorun-python-code"></a>Jak toorun kód Python
Toto téma obsahuje celkem čtyři vzorové kódy, z nichž každý provádí konkrétní funkci. Hello následující pokyny znamenat jak toocreate s textovým souborem, Vložit blok kódu a potom uložte soubor hello tak, aby ji můžete spustit později. Být jisti toocreate čtyři samostatné soubory, jeden pro každou blok kódu.

- Pomocí oblíbeného textového editoru vytvořte nový soubor.
- Zkopírujte a vložte jednu z ukázky kódu hello v následující části do textového souboru hello hello. Nahraďte hello **hostitele**, **dbname**, **uživatele**, a **heslo** parametry s hello hodnoty, kterou jste zadali při vytvoření hello Server a databáze.
- Uložte hello soubor s příponou .py hello (například postgres.py) do složky projektu. Pokud používáte hello operačního systému Windows, se, že tooselect kódování UTF-8 při ukládání souboru hello. 
- Spusťte prostředí shell příkazového řádku nebo Bash hello a poté změňte hello tooyour projektu složku, například `cd postgres`.
-  toorun hello kód, typ hello Python příkazu s parametrem hello název souboru, například `Python postgres.py`.

> [!NOTE]
> Počínaje Python verze 3, mohou se zobrazit chyba hello `SyntaxError: Missing parentheses in call too'print'` při spuštění hello následující bloky kódu. Pokud k tomu dojde, nahraďte každý příkaz toohello volání `print "string"` s použitím závorky, jako například volání funkce `print("string")`.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použití hello následující kód tooconnect a načtení dat pomocí hello [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) fungovat s **vložit** příkaz jazyka SQL. Hello [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL. Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.

```Python
import psycopg2

# Update connection string information obtained from hello portal
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

Po hello kód spustí úspěšně, zobrazí se následující výstup hello:

![Výstup příkazového řádku](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooread hello data vložená, pomocí [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungovat s **vyberte** příkaz jazyka SQL. Tato funkce přijímá dotazu a vrátí výsledek nastavit, které lze vstupní přes s použitím hello [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall). Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.

```Python
import psycopg2

# Update connection string information obtained from hello portal
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

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooupdate hello inventáře řádek, který jste vložili dříve pomocí [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungovat s **aktualizace** příkaz jazyka SQL. Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.

```Python
import psycopg2

# Update connection string information obtained from hello portal
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

# Update a data row in hello table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód toodelete skladové položky, kterou jste vložili dříve pomocí [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungovat s **odstranit** příkaz jazyka SQL. Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.

```Python
import psycopg2

# Update connection string information obtained from hello portal
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

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
