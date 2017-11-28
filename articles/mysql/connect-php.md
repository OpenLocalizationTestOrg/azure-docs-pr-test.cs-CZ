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
# <a name="azure-database-for-mysql-use-php-tooconnect-and-query-data"></a><span data-ttu-id="a7138-103">Azure databáze pro databázi MySQL: použití PHP data tooconnect a dotazů</span><span class="sxs-lookup"><span data-stu-id="a7138-103">Azure Database for MySQL: Use PHP tooconnect and query data</span></span>
<span data-ttu-id="a7138-104">Tento rychlý start předvádí jak tooconnect tooan Azure databáze pro používání MySQL [PHP](http://php.net/manual/intro-whatis.php) aplikace.</span><span class="sxs-lookup"><span data-stu-id="a7138-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="a7138-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="a7138-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="a7138-106">Tento článek předpokládá, že jste obeznámeni s vývojem pomocí PHP, ale, že se nový tooworking s Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a7138-106">This article assumes you are familiar with development using PHP, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7138-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a7138-107">Prerequisites</span></span>
<span data-ttu-id="a7138-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="a7138-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="a7138-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a7138-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="a7138-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a7138-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="a7138-111">Instalace PHP</span><span class="sxs-lookup"><span data-stu-id="a7138-111">Install PHP</span></span>
<span data-ttu-id="a7138-112">Nainstalujte PHP na vlastní server nebo vytvořte [webovou aplikaci](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) Azure, která zahrnuje PHP.</span><span class="sxs-lookup"><span data-stu-id="a7138-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="macos"></a><span data-ttu-id="a7138-113">MacOS</span><span class="sxs-lookup"><span data-stu-id="a7138-113">MacOS</span></span>
- <span data-ttu-id="a7138-114">Stáhněte [verzi PHP 7.1.4](http://php.net/downloads.php).</span><span class="sxs-lookup"><span data-stu-id="a7138-114">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="a7138-115">Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.macosx.php) pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a7138-115">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="a7138-116">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="a7138-116">Linux (Ubuntu)</span></span>
- <span data-ttu-id="a7138-117">Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://php.net/downloads.php).</span><span class="sxs-lookup"><span data-stu-id="a7138-117">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="a7138-118">Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.unix.php) pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a7138-118">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>

### <a name="windows"></a><span data-ttu-id="a7138-119">Windows</span><span class="sxs-lookup"><span data-stu-id="a7138-119">Windows</span></span>
- <span data-ttu-id="a7138-120">Stáhněte [verzi PHP 7.1.4 Non-Thread Safe (x64)](http://windows.php.net/download#php-7.1).</span><span class="sxs-lookup"><span data-stu-id="a7138-120">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="a7138-121">Instalace PHP a odkazovat toohello [PHP ručně](http://php.net/manual/install.windows.php) pro další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a7138-121">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="a7138-122">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="a7138-122">Get connection information</span></span>
<span data-ttu-id="a7138-123">Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a7138-123">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="a7138-124">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a7138-124">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="a7138-125">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a7138-125">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a7138-126">V levém podokně hello, klikněte na **všechny prostředky**a poté vyhledejte hello serveru, které jste vytvořili (například **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="a7138-126">In hello left pane, click **All resources**, and then search for hello server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="a7138-127">Klikněte na název serveru hello.</span><span class="sxs-lookup"><span data-stu-id="a7138-127">Click hello server name.</span></span>
4. <span data-ttu-id="a7138-128">Vyberte hello serveru **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="a7138-128">Select hello server's **Properties** page.</span></span> <span data-ttu-id="a7138-129">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="a7138-129">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="a7138-130">![Název serveru Azure Database for MySQL](./media/connect-php/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="a7138-130">![Azure Database for MySQL server name](./media/connect-php/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="a7138-131">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="a7138-131">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="a7138-132">Připojení a vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="a7138-132">Connect and create a table</span></span>
<span data-ttu-id="a7138-133">Použití hello následující kód tooconnect a vytvořte tabulku pomocí **CREATE TABLE** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a7138-133">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement.</span></span> 

<span data-ttu-id="a7138-134">Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="a7138-134">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="a7138-135">Hello kód volání metody [mysqli_init](http://php.net/manual/mysqli.init.php) a [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="a7138-135">hello code call methods [mysqli_init](http://php.net/manual/mysqli.init.php) and [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL.</span></span> <span data-ttu-id="a7138-136">Potom zavolá metodu [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="a7138-136">Then it calls method [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello query.</span></span> <span data-ttu-id="a7138-137">Potom zavolá metodu [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello připojení.</span><span class="sxs-lookup"><span data-stu-id="a7138-137">Then it calls method [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello connection.</span></span>

<span data-ttu-id="a7138-138">Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a7138-138">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="insert-data"></a><span data-ttu-id="a7138-139">Vložení dat</span><span class="sxs-lookup"><span data-stu-id="a7138-139">Insert data</span></span>
<span data-ttu-id="a7138-140">Použití hello následující kód tooconnect a vkládání dat pomocí **vložit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a7138-140">Use hello following code tooconnect and insert data using an **INSERT** SQL statement.</span></span>

<span data-ttu-id="a7138-141">Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="a7138-141">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="a7138-142">Hello kód používá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate připravené vložte příkaz a potom hello vazby parametrů pro každou hodnotu vloženého sloupce pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="a7138-142">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared insert statement, then binds hello parameters for each inserted column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="a7138-143">Spustí kód Hello hello příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a později zavře hello příkaz pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="a7138-143">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="a7138-144">Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a7138-144">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="a7138-145">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="a7138-145">Read data</span></span>
<span data-ttu-id="a7138-146">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a7138-146">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span>  <span data-ttu-id="a7138-147">Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="a7138-147">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="a7138-148">Hello kód používá metodu [mysqli_query](http://php.net/manual/mysqli.query.php) provádění dotazu sql hello a používá [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) toofetch metoda hello výsledné řádky.</span><span class="sxs-lookup"><span data-stu-id="a7138-148">hello code uses method [mysqli_query](http://php.net/manual/mysqli.query.php) perform hello sql query, and uses [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) method toofetch hello resulting rows.</span></span>

<span data-ttu-id="a7138-149">Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a7138-149">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="a7138-150">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="a7138-150">Update data</span></span>
<span data-ttu-id="a7138-151">Použití hello následující kód tooconnect a aktualizovat data pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a7138-151">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="a7138-152">Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="a7138-152">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="a7138-153">Hello kód používá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate výpis připravené aktualizace, pak váže hello parametry pro každou hodnotu aktualizované sloupce pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="a7138-153">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared update statement, then binds hello parameters for each updated column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="a7138-154">Spustí kód Hello hello příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a později zavře hello příkaz pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="a7138-154">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="a7138-155">Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a7138-155">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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


## <a name="delete-data"></a><span data-ttu-id="a7138-156">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="a7138-156">Delete data</span></span>
<span data-ttu-id="a7138-157">Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a7138-157">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="a7138-158">Kód Hello používá hello **MySQL Vylepšené rozšíření** (mysqli) třídy zahrnuty v jazyce PHP.</span><span class="sxs-lookup"><span data-stu-id="a7138-158">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="a7138-159">Hello kód používá metodu [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate připravený příkaz delete a potom hello vazby parametrů pro hello kde klauzule v příkazu hello pomocí metody [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="a7138-159">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared delete statement, then binds hello parameters for hello where clause in hello statement using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="a7138-160">Spustí kód Hello hello příkaz pomocí metody [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) a později zavře hello příkaz pomocí metody [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="a7138-160">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="a7138-161">Parametry hostitele, uživatelské jméno, heslo a %{db_name/ hello nahraďte vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a7138-161">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="a7138-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7138-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a7138-163">Vytvoření webové aplikace PHP a MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="a7138-163">Build a PHP and MySQL web app in Azure</span></span>](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
