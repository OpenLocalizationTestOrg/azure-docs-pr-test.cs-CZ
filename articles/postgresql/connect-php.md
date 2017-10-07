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
# <a name="azure-database-for-postgresql-use-php-tooconnect-and-query-data"></a><span data-ttu-id="16278-103">Azure databázi PostgreSQL: použití PHP data tooconnect a dotazů</span><span class="sxs-lookup"><span data-stu-id="16278-103">Azure Database for PostgreSQL: Use PHP tooconnect and query data</span></span>
<span data-ttu-id="16278-104">Tento rychlý start předvádí jak tooconnect tooan Azure databázi PostgreSQL pomocí [PHP](http://php.net/manual/intro-whatis.php) aplikace.</span><span class="sxs-lookup"><span data-stu-id="16278-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="16278-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="16278-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="16278-106">Tento článek předpokládá, že jste obeznámeni s vývojem pomocí PHP, ale, že se nový tooworking s Azure databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="16278-106">This article assumes you are familiar with development using PHP, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16278-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="16278-107">Prerequisites</span></span>
<span data-ttu-id="16278-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="16278-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="16278-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="16278-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="16278-110">Vytvoření databáze – rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="16278-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="16278-111">Instalace PHP</span><span class="sxs-lookup"><span data-stu-id="16278-111">Install PHP</span></span>
<span data-ttu-id="16278-112">Nainstalujte PHP na vlastní server nebo vytvořte [webovou aplikaci](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) Azure, která zahrnuje PHP.</span><span class="sxs-lookup"><span data-stu-id="16278-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="windows"></a><span data-ttu-id="16278-113">Windows</span><span class="sxs-lookup"><span data-stu-id="16278-113">Windows</span></span>
- <span data-ttu-id="16278-114">Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://windows.php.net/download#php-7.1).</span><span class="sxs-lookup"><span data-stu-id="16278-114">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="16278-115">Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.windows.php) pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="16278-115">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>
- <span data-ttu-id="16278-116">Kód Hello používá hello **pgsql** – třída (ext/php_pgsql.dll), který je součástí hello instalace PHP.</span><span class="sxs-lookup"><span data-stu-id="16278-116">hello code uses hello **pgsql** class (ext/php_pgsql.dll)  that is included in hello PHP installation.</span></span> 
- <span data-ttu-id="16278-117">Povoleno hello **pgsql** rozšíření úpravou konfiguračního souboru php.ini hello, obvykle umístěné v `C:\Program Files\PHP\v7.1\php.ini`.</span><span class="sxs-lookup"><span data-stu-id="16278-117">Enabled hello **pgsql** extension by editing hello php.ini configuration file, typically located at `C:\Program Files\PHP\v7.1\php.ini`.</span></span> <span data-ttu-id="16278-118">Hello konfigurační soubor by měl obsahovat řádek s textem hello `extension=php_pgsql.so`.</span><span class="sxs-lookup"><span data-stu-id="16278-118">hello configuration file should contain a line with hello text `extension=php_pgsql.so`.</span></span> <span data-ttu-id="16278-119">Pokud ji není zobrazený, přidejte hello text a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="16278-119">If it is not shown, add hello text and save hello file.</span></span> <span data-ttu-id="16278-120">Pokud hello text existuje, ale komentář s předponou středník, zrušte komentář u textu hello odebráním hello středníkem.</span><span class="sxs-lookup"><span data-stu-id="16278-120">If hello text is present, but commented with a semicolon prefix, uncomment hello text by removing hello semicolon.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="16278-121">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="16278-121">Linux (Ubuntu)</span></span>
- <span data-ttu-id="16278-122">Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://php.net/downloads.php).</span><span class="sxs-lookup"><span data-stu-id="16278-122">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span> 
- <span data-ttu-id="16278-123">Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.unix.php) pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="16278-123">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>
- <span data-ttu-id="16278-124">Kód Hello používá hello **pgsql** – třída (php_pgsql.so).</span><span class="sxs-lookup"><span data-stu-id="16278-124">hello code uses hello **pgsql** class (php_pgsql.so).</span></span> <span data-ttu-id="16278-125">Nainstalujte ji spuštěním příkazu `sudo apt-get install php-pgsql`.</span><span class="sxs-lookup"><span data-stu-id="16278-125">Install it by running `sudo apt-get install php-pgsql`.</span></span>
- <span data-ttu-id="16278-126">Povoleno hello **pgsql** rozšíření úpravou hello `/etc/php/7.0/mods-available/pgsql.ini` konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="16278-126">Enabled hello **pgsql** extension by editing hello `/etc/php/7.0/mods-available/pgsql.ini` configuration file.</span></span> <span data-ttu-id="16278-127">Hello konfigurační soubor by měl obsahovat řádek s textem hello `extension=php_pgsql.so`.</span><span class="sxs-lookup"><span data-stu-id="16278-127">hello configuration file should contain a line with hello text `extension=php_pgsql.so`.</span></span> <span data-ttu-id="16278-128">Pokud ji není zobrazený, přidejte hello text a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="16278-128">If it is not shown, add hello text and save hello file.</span></span> <span data-ttu-id="16278-129">Pokud hello text existuje, ale komentář s předponou středník, zrušte komentář u textu hello odebráním hello středníkem.</span><span class="sxs-lookup"><span data-stu-id="16278-129">If hello text is present, but commented with a semicolon prefix, uncomment hello text by removing hello semicolon.</span></span>

### <a name="macos"></a><span data-ttu-id="16278-130">MacOS</span><span class="sxs-lookup"><span data-stu-id="16278-130">MacOS</span></span>
- <span data-ttu-id="16278-131">Stáhněte [verzi PHP 7.1.4](http://php.net/downloads.php).</span><span class="sxs-lookup"><span data-stu-id="16278-131">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="16278-132">Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.macosx.php) pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="16278-132">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="16278-133">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="16278-133">Get connection information</span></span>
<span data-ttu-id="16278-134">Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="16278-134">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="16278-135">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="16278-135">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="16278-136">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="16278-136">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="16278-137">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, které jste vytvořili, například **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="16278-137">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="16278-138">Klikněte na název serveru hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="16278-138">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="16278-139">Vyberte hello serveru **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="16278-139">Select hello server's **Overview** page.</span></span> <span data-ttu-id="16278-140">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="16278-140">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="16278-141">![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-php/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="16278-141">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-php/1-connection-string.png)</span></span>
5. <span data-ttu-id="16278-142">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="16278-142">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="16278-143">Připojení a vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="16278-143">Connect and create a table</span></span>
<span data-ttu-id="16278-144">Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL, za nímž následuje **INSERT INTO** SQL příkazy tooadd řádků do tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="16278-144">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="16278-145">Hello kód volání metoda [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="16278-145">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="16278-146">Potom zavolá metodu [pg_query()](http://php.net/manual/en/function.pg-query.php) několikrát toorun několik příkazů a [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello podrobnosti Pokud pokaždé, když došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="16278-146">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) several times toorun several commands, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred each time.</span></span> <span data-ttu-id="16278-147">Potom zavolá metodu [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello připojení.</span><span class="sxs-lookup"><span data-stu-id="16278-147">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="16278-148">Nahraďte hello `$host`, `$database`, `$user`, a `$password` parametry s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="16278-148">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="16278-149">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="16278-149">Read data</span></span>
<span data-ttu-id="16278-150">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="16278-150">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

 <span data-ttu-id="16278-151">Hello kód volání metoda [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="16278-151">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="16278-152">Potom zavolá metodu [pg_query()](http://php.net/manual/en/function.pg-query.php) vyberte příkaz hello toorun, udržování hello výsledky sadě výsledků dotazu, a [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello podrobností, pokud došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="16278-152">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun hello SELECT command, keeping hello results in a result set, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span>  <span data-ttu-id="16278-153">Sada výsledků tooread hello, metoda [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) nazývá ve smyčce, jakmile na řádku a hello řádek jsou načtena data v matici `$row`, s hodnotou jeden datový každý sloupec v pozici každého pole.</span><span class="sxs-lookup"><span data-stu-id="16278-153">tooread hello result set, method [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) is called in a loop, once per row, and hello row data is retrieved in an array `$row`, with one data value per column in each array position.</span></span>  <span data-ttu-id="16278-154">Sada výsledků toofree hello, metoda [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) je volána.</span><span class="sxs-lookup"><span data-stu-id="16278-154">toofree hello result set, method [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) is called.</span></span> <span data-ttu-id="16278-155">Potom zavolá metodu [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello připojení.</span><span class="sxs-lookup"><span data-stu-id="16278-155">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="16278-156">Nahraďte hello `$host`, `$database`, `$user`, a `$password` parametry s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="16278-156">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="16278-157">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="16278-157">Update data</span></span>
<span data-ttu-id="16278-158">Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="16278-158">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="16278-159">Hello kód volání metoda [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="16278-159">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="16278-160">Potom zavolá metodu [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun příkaz, a [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello podrobností, pokud došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="16278-160">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun a command, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span> <span data-ttu-id="16278-161">Potom zavolá metodu [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello připojení.</span><span class="sxs-lookup"><span data-stu-id="16278-161">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="16278-162">Nahraďte hello `$host`, `$database`, `$user`, a `$password` parametry s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="16278-162">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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


## <a name="delete-data"></a><span data-ttu-id="16278-163">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="16278-163">Delete data</span></span>
<span data-ttu-id="16278-164">Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="16278-164">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

 <span data-ttu-id="16278-165">Hello kód volání metoda [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect příliš Azure databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="16278-165">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect too Azure Database for PostgreSQL.</span></span> <span data-ttu-id="16278-166">Potom zavolá metodu [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun příkaz, a [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello podrobností, pokud došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="16278-166">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun a command, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span> <span data-ttu-id="16278-167">Potom zavolá metodu [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello připojení.</span><span class="sxs-lookup"><span data-stu-id="16278-167">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="16278-168">Nahraďte hello `$host`, `$database`, `$user`, a `$password` parametry s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="16278-168">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="16278-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16278-169">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="16278-170">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="16278-170">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
