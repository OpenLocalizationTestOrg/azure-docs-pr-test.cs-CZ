---
title: "Připojit tooAzure databáze pro databázi MySQL z PHP | Microsoft Docs"
description: "Tento rychlý start poskytuje několik ukázky kódu PHP můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro databázi MySQL."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: b928748c473c1aef320ae2183f237b5b845e83f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-php-tooconnect-and-query-data"></a>Azure databáze pro databázi MySQL: použití PHP data tooconnect a dotazů
Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro používání MySQL [PHP](http://php.net/manual/intro-whatis.php) aplikace. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Tento článek předpokládá, že jste obeznámeni s vývojem pomocí PHP, ale, že se nový tooworking s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a>Instalace PHP
Nainstalujte PHP na vlastní server nebo vytvořte [webovou aplikaci](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) Azure, která zahrnuje PHP.

### <a name="macos"></a>MacOS
- Stáhněte [verzi PHP 7.1.4](http://php.net/downloads.php).
- Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.macosx.php) pro další konfiguraci.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://php.net/downloads.php).
- Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.unix.php) pro další konfiguraci.

### <a name="windows"></a>Windows
- Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://windows.php.net/download#php-7.1).
- Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.windows.php) pro další konfiguraci.

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V levém podokně hello, klikněte na **všechny prostředky**a poté vyhledejte hello serveru, které jste vytvořili (například **myserver4demo**).
3. Klikněte na název serveru hello.
4. Vyberte hello serveru **vlastnosti** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Název serveru Azure Database for MySQL](./media/connect-php/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="connect-and-create-a-table"></a>Připojení a vytvoření tabulky
Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL. 

Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP. Hello kód volání metody [mysqli_init](http://php.net/manual/mysqli.init.php) a [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL. Potom zavolá metodu [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello dotazu. Potom zavolá metodu [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello připojení.

Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

// Run hello create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a>Vložení dat
Použití hello následující kód tooconnect a vkládání dat pomocí **vložit** příkaz jazyka SQL.

Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP. Hello kód používá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate připravené vložte příkaz a potom hello vazby parametrů pro každou hodnotu vloženého sloupce pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Spustí kód Hello hello příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a později zavře hello příkaz pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close hello connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.  Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP. Hello kód používá metodu [mysqli_query](http://php.net/manual/mysqli.query.php) provádění dotazu sql hello a používá [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) toofetch metoda hello výsledné řádky.

Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.

Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP. Hello kód používá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate výpis připravené aktualizace, pak váže hello parametry pro každou hodnotu aktualizované sloupce pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Spustí kód Hello hello příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a později zavře hello příkaz pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close hello connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL. 

Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP. Hello kód používá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate připravený příkaz delete a potom hello vazby parametrů pro hello kde klauzule v příkazu hello pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Spustí kód Hello hello příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a později zavře hello příkaz pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Vytvoření webové aplikace PHP a MySQL v Azure](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
