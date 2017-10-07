---
title: "aaaConnect tooAzure databázi PostgreSQL pomocí PHP | Microsoft Docs"
description: "Tento rychlý start poskytuje ukázka kódu PHP můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: php
ms.topic: quickstart
ms.date: 06/29/2017
ms.openlocfilehash: 008505e837e37cb8c7fea3fc164b3446c3580e46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-php-tooconnect-and-query-data"></a>Azure databázi PostgreSQL: použití PHP data tooconnect a dotazů
Tento rychlý start předvádí jak tooconnect tooan Azure databázi PostgreSQL pomocí [PHP](http://php.net/manual/intro-whatis.php) aplikace. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Tento článek předpokládá, že jste obeznámeni s vývojem pomocí PHP, ale, že se nový tooworking s Azure databáze PostgreSQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku Azure](quickstart-create-server-database-azure-cli.md)

## <a name="install-php"></a>Instalace PHP
Nainstalujte PHP na vlastní server nebo vytvořte [webovou aplikaci](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) Azure, která zahrnuje PHP.

### <a name="windows"></a>Windows
- Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://windows.php.net/download#php-7.1).
- Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.windows.php) pro další konfiguraci.
- Kód Hello používá hello **pgsql** – třída (ext/php_pgsql.dll), který je součástí hello instalace PHP. 
- Povoleno hello **pgsql** rozšíření úpravou konfiguračního souboru php.ini hello, obvykle umístěné v `C:\Program Files\PHP\v7.1\php.ini`. Hello konfigurační soubor by měl obsahovat řádek s textem hello `extension=php_pgsql.so`. Pokud ji není zobrazený, přidejte hello text a uložte soubor hello. Pokud hello text existuje, ale komentář s předponou středník, zrušte komentář u textu hello odebráním hello středníkem.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://php.net/downloads.php). 
- Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.unix.php) pro další konfiguraci.
- Kód Hello používá hello **pgsql** – třída (php_pgsql.so). Nainstalujte ji spuštěním příkazu `sudo apt-get install php-pgsql`.
- Povoleno hello **pgsql** rozšíření úpravou hello `/etc/php/7.0/mods-available/pgsql.ini` konfigurační soubor. Hello konfigurační soubor by měl obsahovat řádek s textem hello `extension=php_pgsql.so`. Pokud ji není zobrazený, přidejte hello text a uložte soubor hello. Pokud hello text existuje, ale komentář s předponou středník, zrušte komentář u textu hello odebráním hello středníkem.

### <a name="macos"></a>MacOS
- Stáhněte [verzi PHP 7.1.4](http://php.net/downloads.php).
- Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.macosx.php) pro další konfiguraci.

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **mypgserver 20170401**.
3. Klikněte na název serveru hello **mypgserver 20170401**.
4. Vyberte hello serveru **přehled** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-php/1-connection-string.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="connect-and-create-a-table"></a>Připojení a vytvoření tabulky
Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL, za nímž následuje **INSERT INTO** SQL příkazy tooadd řádků do tabulky hello.

Hello kód volání metoda [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure databázi PostgreSQL. Potom zavolá metodu [pg_query()](http://php.net/manual/en/function.pg-query.php) několikrát toorun několik příkazů a [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello podrobnosti Pokud pokaždé, když došlo k chybě. Potom zavolá metodu [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello připojení.

Nahraďte hello `$host`, `$database`, `$user`, a `$password` parametry s vlastními hodnotami. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password") 
        or die("Failed toocreate connection toodatabase: ". pg_last_error(). "<br/>");
    print "Successfully created connection toodatabase.<br/>";

    // Drop previous table of same name if one exists.
    $query = "DROP TABLE IF EXISTS inventory;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    print "Finished dropping table (if existed).<br/>";

    // Create table.
    $query = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    print "Finished creating table.<br/>";

    // Insert some data into table.
    $name = '\'banana\'';
    $quantity = 150;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($1, $2);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");

    $name = '\'orange\'';
    $quantity = 154;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($name, $quantity);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");

    $name = '\'apple\'';
    $quantity = 100;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($name, $quantity);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error()). "<br/>";

    print "Inserted 3 rows of data.<br/>";

    // Closing connection
    pg_close($connection);
?>
```

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. 

 Hello kód volání metoda [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure databázi PostgreSQL. Potom zavolá metodu [pg_query()](http://php.net/manual/en/function.pg-query.php) vyberte příkaz hello toorun, udržování hello výsledky sadě výsledků dotazu, a [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello podrobností, pokud došlo k chybě.  Sada výsledků tooread hello, metoda [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) nazývá ve smyčce, jakmile na řádku a hello řádek jsou načtena data v matici `$row`, s hodnotou jeden datový každý sloupec v pozici každého pole.  Sada výsledků toofree hello, metoda [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) je volána. Potom zavolá metodu [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello připojení.

Nahraďte hello `$host`, `$database`, `$user`, a `$password` parametry s vlastními hodnotami. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";
    
    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
                or die("Failed toocreate connection toodatabase: ". pg_last_error(). "<br/>");

    print "Successfully created connection toodatabase. <br/>";

    // Perform some SQL queries over hello connection.
    $query = "SELECT * from inventory";
    $result_set = pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    while ($row = pg_fetch_row($result_set))
    {
        print "Data row = ($row[0], $row[1], $row[2]). <br/>";
    }

    // Free result_set
    pg_free_result($result_set);

    // Closing connection
    pg_close($connection);
?>
```

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.

Hello kód volání metoda [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure databázi PostgreSQL. Potom zavolá metodu [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun příkaz, a [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello podrobností, pokud došlo k chybě. Potom zavolá metodu [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello připojení.

Nahraďte hello `$host`, `$database`, `$user`, a `$password` parametry s vlastními hodnotami. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
                or die("Failed toocreate connection toodatabase: ". pg_last_error(). ".<br/>");

    print "Successfully created connection toodatabase. <br/>";

    // Modify some data in table.
    $new_quantity = 200;
    $name = '\'banana\'';
    $query = "UPDATE inventory SET quantity = $new_quantity WHERE name = $name;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). ".<br/>");
    print "Updated 1 row of data. </br>";

    // Closing connection
    pg_close($connection);
?>
```


## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL. 

 Hello kód volání metoda [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect příliš Azure databázi PostgreSQL. Potom zavolá metodu [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun příkaz, a [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello podrobností, pokud došlo k chybě. Potom zavolá metodu [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello připojení.

Nahraďte hello `$host`, `$database`, `$user`, a `$password` parametry s vlastními hodnotami. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
            or die("Failed toocreate connection toodatabase: ". pg_last_error(). ". </br>");

    print "Successfully created connection toodatabase. <br/>";

    // Delete some data from table.
    $name = '\'orange\'';
    $query = "DELETE FROM inventory WHERE name = $name;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). ". <br/>");
    print "Deleted 1 row of data. <br/>";

    // Closing connection
    pg_close($connection);
?>
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
