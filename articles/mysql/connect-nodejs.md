---
title: "Připojit tooAzure databáze pro databázi MySQL z Node.js | Microsoft Docs"
description: "Tento rychlý start poskytuje několik ukázky kódu Node.js můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro databázi MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/17/2017
ms.openlocfilehash: ed9a39b5ab003e8216cef1c0f6a95a75c3db8458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-nodejs-tooconnect-and-query-data"></a><span data-ttu-id="a86be-103">Azure databáze pro databázi MySQL: použití Node.js tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="a86be-103">Azure Database for MySQL: Use Node.js tooconnect and query data</span></span>
<span data-ttu-id="a86be-104">Tento rychlý start předvádí jak tooconnect tooan Azure databázi MySQL pomocí [Node.js](https://nodejs.org/) z platformy Windows, Mac a Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="a86be-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using [Node.js](https://nodejs.org/) from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="a86be-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="a86be-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="a86be-106">Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí Node.js, a že jste novou tooworking s Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-106">hello steps in this article assume that you are familiar with developing using Node.js, and that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a86be-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a86be-107">Prerequisites</span></span>
<span data-ttu-id="a86be-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="a86be-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="a86be-109">Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a86be-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="a86be-110">Vytvoření serveru Azure Database for MySQL pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a86be-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="a86be-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="a86be-111">You also need to:</span></span>
- <span data-ttu-id="a86be-112">Nainstalujte hello [Node.js](https://nodejs.org) modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="a86be-112">Install hello [Node.js](https://nodejs.org) runtime.</span></span>
- <span data-ttu-id="a86be-113">Nainstalujte [mysql2](https://www.npmjs.com/package/mysql2) balíček tooconnect tooMySQL z aplikace Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="a86be-113">Install [mysql2](https://www.npmjs.com/package/mysql2) package tooconnect tooMySQL from hello Node.js application.</span></span> 

## <a name="install-nodejs-and-hello-mysql-connector"></a><span data-ttu-id="a86be-114">Nainstalujte si Node.js a hello MySQL konektoru</span><span class="sxs-lookup"><span data-stu-id="a86be-114">Install Node.js and hello MySQL connector</span></span>
<span data-ttu-id="a86be-115">V závislosti na platformě postupujte podle hello odpovídající pokyny tooinstall Node.js.</span><span class="sxs-lookup"><span data-stu-id="a86be-115">Depending on your platform, follow hello appropriate instructions tooinstall Node.js.</span></span> <span data-ttu-id="a86be-116">Použít npm tooinstall hello mysql2 balíček a jeho závislosti do složky projektu.</span><span class="sxs-lookup"><span data-stu-id="a86be-116">Use npm tooinstall hello mysql2 package and its dependencies into your project folder.</span></span>

### <a name="windows"></a><span data-ttu-id="a86be-117">**Windows**</span><span class="sxs-lookup"><span data-stu-id="a86be-117">**Windows**</span></span>
1. <span data-ttu-id="a86be-118">Navštivte hello [Node.js stáhne stránky](https://nodejs.org/en/download/) a vyberte požadovanou možnost Instalační služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a86be-118">Visit hello [Node.js downloads page](https://nodejs.org/en/download/) and select your desired Windows installer option.</span></span>
2. <span data-ttu-id="a86be-119">Vytvořte místní složku projektu, například `nodejsmysql`.</span><span class="sxs-lookup"><span data-stu-id="a86be-119">Make a local project folder such as `nodejsmysql`.</span></span> 
3. <span data-ttu-id="a86be-120">Spusťte hello příkazového řádku a cd do složky hello projektu, například`cd c:\nodejsmysql\`</span><span class="sxs-lookup"><span data-stu-id="a86be-120">Launch hello command prompt and cd into hello project folder, such as `cd c:\nodejsmysql\`</span></span>
4. <span data-ttu-id="a86be-121">Spusťte hello NPM nástroj tooinstall hello mysql2 knihovny do složky projektu hello.</span><span class="sxs-lookup"><span data-stu-id="a86be-121">Run hello NPM tool tooinstall hello mysql2 library into hello project folder.</span></span>

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. <span data-ttu-id="a86be-122">Ověření instalace hello kontrolou hello `npm list` textu pro výstup `mysql2@1.3.5`.</span><span class="sxs-lookup"><span data-stu-id="a86be-122">Verify hello installation by checking hello `npm list` output text for `mysql2@1.3.5`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="a86be-123">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="a86be-123">**Linux (Ubuntu)**</span></span>
1. <span data-ttu-id="a86be-124">Hello spusťte následující příkazy tooinstall **Node.js** a **npm** hello Správce balíčků pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="a86be-124">Run hello following commands tooinstall **Node.js** and **npm** hello package manager for Node.js.</span></span>

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. <span data-ttu-id="a86be-125">Spusťte následující příkazy toomake hello složky projektu `mysqlnodejs` a hello mysql2 balíček nainstalovat do této složky.</span><span class="sxs-lookup"><span data-stu-id="a86be-125">Run hello following commands toomake a project folder `mysqlnodejs` and install hello mysql2 package into that folder.</span></span>

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. <span data-ttu-id="a86be-126">Ověření instalace hello kontrolou npm seznamu výstupní text pro `mysql2@1.3.5`.</span><span class="sxs-lookup"><span data-stu-id="a86be-126">Verify hello installation by checking npm list output text for `mysql2@1.3.5`.</span></span>

### <a name="mac-os"></a><span data-ttu-id="a86be-127">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="a86be-127">**Mac OS**</span></span>
1. <span data-ttu-id="a86be-128">Zadejte následující příkazy tooinstall hello **brew**, Správce balíčků snadno použitelné pro Mac OS X a **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="a86be-128">Enter hello following commands tooinstall **brew**, an easy-to-use package manager for Mac OS X and **Node.js**.</span></span>

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. <span data-ttu-id="a86be-129">Spusťte následující příkazy toomake hello složky projektu `mysqlnodejs` a hello mysql2 balíček nainstalovat do této složky.</span><span class="sxs-lookup"><span data-stu-id="a86be-129">Run hello following commands toomake a project folder `mysqlnodejs` and install hello mysql2 package into that folder.</span></span>

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. <span data-ttu-id="a86be-130">Ověření instalace hello kontrolou hello `npm list` textu pro výstup `mysql2@1.3.6`.</span><span class="sxs-lookup"><span data-stu-id="a86be-130">Verify hello installation by checking hello `npm list` output text for `mysql2@1.3.6`.</span></span> <span data-ttu-id="a86be-131">Hello číslo verze se může lišit podle jsou vydávány nové opravy.</span><span class="sxs-lookup"><span data-stu-id="a86be-131">hello version number may vary as new patches are released.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="a86be-132">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="a86be-132">Get connection information</span></span>
<span data-ttu-id="a86be-133">Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-133">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="a86be-134">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a86be-134">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="a86be-135">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a86be-135">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a86be-136">V levém podokně hello, klikněte na **všechny prostředky**a poté vyhledejte hello serveru, které jste vytvořili (například **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="a86be-136">In hello left pane, click **All resources**, and then search for hello server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="a86be-137">Klikněte na název serveru hello **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="a86be-137">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="a86be-138">Vyberte hello serveru **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="a86be-138">Select hello server's **Properties** page.</span></span> <span data-ttu-id="a86be-139">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="a86be-139">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="a86be-140">![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-nodejs/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="a86be-140">![Azure Database for MySQL - Server Admin Login](./media/connect-nodejs/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="a86be-141">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="a86be-141">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="running-hello-javascript-code-in-nodejs"></a><span data-ttu-id="a86be-142">Spuštění kódu JavaScript hello v Node.js</span><span class="sxs-lookup"><span data-stu-id="a86be-142">Running hello JavaScript code in Node.js</span></span>
1. <span data-ttu-id="a86be-143">Vložte kód JavaScript hello do textových souborů a uložte do složky projektu se soubor .js rozšíření, jako je například C:\nodejsmysql\createtable.js nebo /home/username/nodejsmysql/createtable.js</span><span class="sxs-lookup"><span data-stu-id="a86be-143">Paste hello JavaScript code into text files, and save into a project folder with file extension .js, such as C:\nodejsmysql\createtable.js or /home/username/nodejsmysql/createtable.js</span></span>
2. <span data-ttu-id="a86be-144">Spusťte příkazový řádek hello nebo bash prostředí.</span><span class="sxs-lookup"><span data-stu-id="a86be-144">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="a86be-145">Změňte adresář na složku vašeho projektu pomocí příkazu `cd nodejsmysql`.</span><span class="sxs-lookup"><span data-stu-id="a86be-145">Change directory into your project folder `cd nodejsmysql`.</span></span>
3. <span data-ttu-id="a86be-146">aplikace hello toorun, zadejte příkaz hello uzel a potom podle názvu souboru text hello, jako například `node createtable.js`.</span><span class="sxs-lookup"><span data-stu-id="a86be-146">toorun hello application, type hello node command followed by hello file name, such as `node createtable.js`.</span></span>
4. <span data-ttu-id="a86be-147">V systému Windows Pokud aplikace hello uzel není v cestě proměnné prostředí, může být nutné toouse hello úplná cesta toolaunch hello uzel aplikace, jako například`"C:\Program Files\nodejs\node.exe" createtable.js`</span><span class="sxs-lookup"><span data-stu-id="a86be-147">On Windows, if hello node application is not in your environment variable path, you may need toouse hello full path toolaunch hello node application, such as `"C:\Program Files\nodejs\node.exe" createtable.js`</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="a86be-148">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="a86be-148">Connect, create table, and insert data</span></span>
<span data-ttu-id="a86be-149">Použití hello následující kód tooconnect a načtení dat pomocí hello **CREATE TABLE** a **INSERT INTO** příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-149">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>

<span data-ttu-id="a86be-150">Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité toointerface hello MySQL serveru.</span><span class="sxs-lookup"><span data-stu-id="a86be-150">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="a86be-151">Hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) funkce je použité tooestablish hello připojení toohello server.</span><span class="sxs-lookup"><span data-stu-id="a86be-151">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="a86be-152">Hello [query()](https://github.com/mysqljs/mysql#performing-queries) funkce je použité tooexecute hello dotaz SQL na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-152">hello [query()](https://github.com/mysqljs/mysql#performing-queries) function is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="a86be-153">Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="a86be-153">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }   
});

function queryDatabase(){
       conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
            if (err) throw err; 
            console.log('Dropped inventory table if existed.');
        })
       conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Created inventory table.');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
            else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
        function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.end(function (err) { 
        if (err) throw err;
        else  console.log('Done.') 
        });
};
```

## <a name="read-data"></a><span data-ttu-id="a86be-154">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="a86be-154">Read data</span></span>
<span data-ttu-id="a86be-155">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-155">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="a86be-156">Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité toointerface hello MySQL serveru.</span><span class="sxs-lookup"><span data-stu-id="a86be-156">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="a86be-157">Hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité tooestablish hello připojení toohello server.</span><span class="sxs-lookup"><span data-stu-id="a86be-157">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="a86be-158">Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metoda je použité tooexecute hello dotaz SQL na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-158">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> <span data-ttu-id="a86be-159">pole výsledky Hello je použité toohold hello výsledky dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="a86be-159">hello results array is used toohold hello results of hello query.</span></span>

<span data-ttu-id="a86be-160">Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="a86be-160">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }   
    });

function readData(){
        conn.query('SELECT * FROM inventory', 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Selected ' + results.length + ' row(s).');
                for (i = 0; i < results.length; i++) {
                    console.log('Row: ' + JSON.stringify(results[i]));
                }
                console.log('Done.');
            })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Closing connection.') 
        });
};
```

## <a name="update-data"></a><span data-ttu-id="a86be-161">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="a86be-161">Update data</span></span>
<span data-ttu-id="a86be-162">Použití hello následující kód tooconnect a čtení dat pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-162">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="a86be-163">Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité toointerface hello MySQL serveru.</span><span class="sxs-lookup"><span data-stu-id="a86be-163">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="a86be-164">Hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité tooestablish hello připojení toohello server.</span><span class="sxs-lookup"><span data-stu-id="a86be-164">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="a86be-165">Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metoda je použité tooexecute hello dotaz SQL na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-165">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="a86be-166">Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="a86be-166">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }   
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a><span data-ttu-id="a86be-167">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="a86be-167">Delete data</span></span>
<span data-ttu-id="a86be-168">Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-168">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="a86be-169">Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité toointerface hello MySQL serveru.</span><span class="sxs-lookup"><span data-stu-id="a86be-169">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="a86be-170">Hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité tooestablish hello připojení toohello server.</span><span class="sxs-lookup"><span data-stu-id="a86be-170">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="a86be-171">Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metoda je použité tooexecute hello dotaz SQL na databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="a86be-171">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="a86be-172">Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.</span><span class="sxs-lookup"><span data-stu-id="a86be-172">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }   
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="next-steps"></a><span data-ttu-id="a86be-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a86be-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a86be-174">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="a86be-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
