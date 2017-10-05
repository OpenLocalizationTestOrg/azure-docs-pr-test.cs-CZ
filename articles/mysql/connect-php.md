---
title: "Připojení k Azure Database for MySQL z PHP | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód PHP, který můžete použít k připojení a dotazování dat z databáze Azure Database for MySQL."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: 59da1ab9e76685d7ed0c4415ef99578c982e956c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-database-for-mysql-use-php-to-connect-and-query-data"></a><span data-ttu-id="0443e-103">Azure Database for MySQL: Připojení a dotazování dat pomocí PHP</span><span class="sxs-lookup"><span data-stu-id="0443e-103">Azure Database for MySQL: Use PHP to connect and query data</span></span>
<span data-ttu-id="0443e-104">Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí aplikace v [PHP](http://php.net/manual/intro-whatis.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="0443e-105">Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="0443e-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="0443e-106">V tomto článku se předpokládá, že máte zkušenosti s vývojem pomocí PHP, ale teprve začínáte pracovat se službou Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="0443e-106">This article assumes you are familiar with development using PHP, but that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0443e-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0443e-107">Prerequisites</span></span>
<span data-ttu-id="0443e-108">Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="0443e-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="0443e-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0443e-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="0443e-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0443e-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="0443e-111">Instalace PHP</span><span class="sxs-lookup"><span data-stu-id="0443e-111">Install PHP</span></span>
<span data-ttu-id="0443e-112">Nainstalujte PHP na vlastní server nebo vytvořte [webovou aplikaci](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) Azure, která zahrnuje PHP.</span><span class="sxs-lookup"><span data-stu-id="0443e-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="macos"></a><span data-ttu-id="0443e-113">MacOS</span><span class="sxs-lookup"><span data-stu-id="0443e-113">MacOS</span></span>
- <span data-ttu-id="0443e-114">Stáhněte [verzi PHP 7.1.4](http://php.net/downloads.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-114">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="0443e-115">Nainstalujte PHP a další konfiguraci vyhledejte v [příručce k PHP](http://php.net/manual/install.macosx.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-115">Install PHP and refer to the [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="0443e-116">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="0443e-116">Linux (Ubuntu)</span></span>
- <span data-ttu-id="0443e-117">Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://php.net/downloads.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-117">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="0443e-118">Nainstalujte PHP a další konfiguraci vyhledejte v [příručce k PHP](http://php.net/manual/install.unix.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-118">Install PHP and refer to the [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>

### <a name="windows"></a><span data-ttu-id="0443e-119">Windows</span><span class="sxs-lookup"><span data-stu-id="0443e-119">Windows</span></span>
- <span data-ttu-id="0443e-120">Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://windows.php.net/download#php-7.1).</span><span class="sxs-lookup"><span data-stu-id="0443e-120">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="0443e-121">Nainstalujte PHP a další konfiguraci vyhledejte v [příručce k PHP](http://php.net/manual/install.windows.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-121">Install PHP and refer to the [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="0443e-122">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="0443e-122">Get connection information</span></span>
<span data-ttu-id="0443e-123">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL.</span><span class="sxs-lookup"><span data-stu-id="0443e-123">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="0443e-124">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0443e-124">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="0443e-125">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0443e-125">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0443e-126">V levém podokně klikněte na **Všechny prostředky** a potom vyhledejte server, který jste vytvořili (například **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="0443e-126">In the left pane, click **All resources**, and then search for the server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="0443e-127">Klikněte na název serveru.</span><span class="sxs-lookup"><span data-stu-id="0443e-127">Click the server name.</span></span>
4. <span data-ttu-id="0443e-128">Vyberte stránku **Vlastnosti** serveru.</span><span class="sxs-lookup"><span data-stu-id="0443e-128">Select the server's **Properties** page.</span></span> <span data-ttu-id="0443e-129">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="0443e-129">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="0443e-130">![Název serveru Azure Database for MySQL](./media/connect-php/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="0443e-130">![Azure Database for MySQL server name](./media/connect-php/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="0443e-131">Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby obnovit heslo.</span><span class="sxs-lookup"><span data-stu-id="0443e-131">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="0443e-132">Připojení a vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="0443e-132">Connect and create a table</span></span>
<span data-ttu-id="0443e-133">Pomocí následujícího kódu se připojte a vytvořte tabulku s využitím příkazu **CREATE TABLE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="0443e-133">Use the following code to connect and create a table using **CREATE TABLE** SQL statement.</span></span> 

<span data-ttu-id="0443e-134">Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP.</span><span class="sxs-lookup"><span data-stu-id="0443e-134">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="0443e-135">Kód volá metody [mysqli_init](http://php.net/manual/mysqli.init.php) a [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) pro připojení k MySQL.</span><span class="sxs-lookup"><span data-stu-id="0443e-135">The code call methods [mysqli_init](http://php.net/manual/mysqli.init.php) and [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) to connect to MySQL.</span></span> <span data-ttu-id="0443e-136">Potom volá metodu [mysqli_query](http://php.net/manual/mysqli.query.php) pro spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="0443e-136">Then it calls method [mysqli_query](http://php.net/manual/mysqli.query.php) to run the query.</span></span> <span data-ttu-id="0443e-137">Potom volá metodu [mysqli_close](http://php.net/manual/mysqli.close.php) pro ukončení připojení.</span><span class="sxs-lookup"><span data-stu-id="0443e-137">Then it calls method [mysqli_close](http://php.net/manual/mysqli.close.php) to close the connection.</span></span>

<span data-ttu-id="0443e-138">Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="0443e-138">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
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

//Close the connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a><span data-ttu-id="0443e-139">Vložení dat</span><span class="sxs-lookup"><span data-stu-id="0443e-139">Insert data</span></span>
<span data-ttu-id="0443e-140">Pomocí následujícího kódu se připojte a vložte data s využitím příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="0443e-140">Use the following code to connect and insert data using an **INSERT** SQL statement.</span></span>

<span data-ttu-id="0443e-141">Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP.</span><span class="sxs-lookup"><span data-stu-id="0443e-141">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="0443e-142">Kód využívá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) k vytvoření připraveného příkazu Insert a potom vytvoří vazbu parametrů pro každou vloženou hodnotu sloupce pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-142">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared insert statement, then binds the parameters for each inserted column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="0443e-143">Kód spustí tento příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a potom tento příkaz zavře pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-143">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="0443e-144">Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="0443e-144">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
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

// Close the connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a><span data-ttu-id="0443e-145">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="0443e-145">Read data</span></span>
<span data-ttu-id="0443e-146">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="0443e-146">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span>  <span data-ttu-id="0443e-147">Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP.</span><span class="sxs-lookup"><span data-stu-id="0443e-147">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="0443e-148">Kód používá metodu [mysqli_query](http://php.net/manual/mysqli.query.php) k provedení dotazu sql query a metodu [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) k načtení výsledných řádků.</span><span class="sxs-lookup"><span data-stu-id="0443e-148">The code uses method [mysqli_query](http://php.net/manual/mysqli.query.php) perform the sql query, and uses [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) method to fetch the resulting rows.</span></span>

<span data-ttu-id="0443e-149">Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="0443e-149">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a><span data-ttu-id="0443e-150">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="0443e-150">Update data</span></span>
<span data-ttu-id="0443e-151">Pomocí následujícího kódu se připojte a aktualizujte data s využitím příkazu **UPDATE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="0443e-151">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="0443e-152">Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP.</span><span class="sxs-lookup"><span data-stu-id="0443e-152">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="0443e-153">Kód využívá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) k vytvoření připraveného příkazu Update a potom vytvoří vazbu parametrů pro každou aktualizovanou hodnotu sloupce pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-153">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared update statement, then binds the parameters for each updated column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="0443e-154">Kód spustí tento příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a potom tento příkaz zavře pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-154">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="0443e-155">Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="0443e-155">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a><span data-ttu-id="0443e-156">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="0443e-156">Delete data</span></span>
<span data-ttu-id="0443e-157">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="0443e-157">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="0443e-158">Tento kód využívá třídu **rozšíření MySQL Improved** (mysqli), která je zahrnutá v PHP.</span><span class="sxs-lookup"><span data-stu-id="0443e-158">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="0443e-159">Kód využívá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) k vytvoření připraveného příkazu Delete a potom vytvoří vazbu parametrů pro klauzuli Where v tomto příkazu pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-159">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared delete statement, then binds the parameters for the where clause in the statement using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="0443e-160">Kód spustí tento příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a potom tento příkaz zavře pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="0443e-160">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="0443e-161">Parametry host (hostitel), username (uživatelské jméno), password (heslo) a db_name (název databáze) nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="0443e-161">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a><span data-ttu-id="0443e-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0443e-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0443e-163">Vytvoření webové aplikace PHP a MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="0443e-163">Build a PHP and MySQL web app in Azure</span></span>](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
