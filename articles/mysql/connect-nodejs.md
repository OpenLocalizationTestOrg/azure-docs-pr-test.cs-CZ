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
# <a name="azure-database-for-mysql-use-nodejs-tooconnect-and-query-data"></a>Azure databáze pro databázi MySQL: použití Node.js tooconnect a dotazování dat
Tento rychlý start předvádí jak tooconnect tooan Azure databázi MySQL pomocí [Node.js](https://nodejs.org/) z platformy Windows, Mac a Ubuntu Linux. Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí Node.js, a že jste novou tooworking s Azure Database pro databázi MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Budete také muset:
- Nainstalujte hello [Node.js](https://nodejs.org) modulu runtime.
- Nainstalujte [mysql2](https://www.npmjs.com/package/mysql2) balíček tooconnect tooMySQL z aplikace Node.js hello. 

## <a name="install-nodejs-and-hello-mysql-connector"></a>Nainstalujte si Node.js a hello MySQL konektoru
V závislosti na platformě postupujte podle hello odpovídající pokyny tooinstall Node.js. Použít npm tooinstall hello mysql2 balíček a jeho závislosti do složky projektu.

### <a name="windows"></a>**Windows**
1. Navštivte hello [Node.js stáhne stránky](https://nodejs.org/en/download/) a vyberte požadovanou možnost Instalační služby systému Windows.
2. Vytvořte místní složku projektu, například `nodejsmysql`. 
3. Spusťte hello příkazového řádku a cd do složky hello projektu, například`cd c:\nodejsmysql\`
4. Spusťte hello NPM nástroj tooinstall hello mysql2 knihovny do složky projektu hello.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. Ověření instalace hello kontrolou hello `npm list` textu pro výstup `mysql2@1.3.5`.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
1. Hello spusťte následující příkazy tooinstall **Node.js** a **npm** hello Správce balíčků pro Node.js.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Spusťte následující příkazy toomake hello složky projektu `mysqlnodejs` a hello mysql2 balíček nainstalovat do této složky.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. Ověření instalace hello kontrolou npm seznamu výstupní text pro `mysql2@1.3.5`.

### <a name="mac-os"></a>**Mac OS**
1. Zadejte následující příkazy tooinstall hello **brew**, Správce balíčků snadno použitelné pro Mac OS X a **Node.js**.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Spusťte následující příkazy toomake hello složky projektu `mysqlnodejs` a hello mysql2 balíček nainstalovat do této složky.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. Ověření instalace hello kontrolou hello `npm list` textu pro výstup `mysql2@1.3.6`. Hello číslo verze se může lišit podle jsou vydávány nové opravy.

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello Azure Database pro databázi MySQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V levém podokně hello, klikněte na **všechny prostředky**a poté vyhledejte hello serveru, které jste vytvořili (například **myserver4demo**).
3. Klikněte na název serveru hello **myserver4demo**.
4. Vyberte hello serveru **vlastnosti** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-nodejs/1_server-properties-name-login.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="running-hello-javascript-code-in-nodejs"></a>Spuštění kódu JavaScript hello v Node.js
1. Vložte kód JavaScript hello do textových souborů a uložte do složky projektu se soubor .js rozšíření, jako je například C:\nodejsmysql\createtable.js nebo /home/username/nodejsmysql/createtable.js
2. Spusťte příkazový řádek hello nebo bash prostředí. Změňte adresář na složku vašeho projektu pomocí příkazu `cd nodejsmysql`.
3. aplikace hello toorun, zadejte příkaz hello uzel a potom podle názvu souboru text hello, jako například `node createtable.js`.
4. V systému Windows Pokud aplikace hello uzel není v cestě proměnné prostředí, může být nutné toouse hello úplná cesta toolaunch hello uzel aplikace, jako například`"C:\Program Files\nodejs\node.exe" createtable.js`

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použití hello následující kód tooconnect a načtení dat pomocí hello **CREATE TABLE** a **INSERT INTO** příkazů SQL.

Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité toointerface hello MySQL serveru. Hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) funkce je použité tooestablish hello připojení toohello server. Hello [query()](https://github.com/mysqljs/mysql#performing-queries) funkce je použité tooexecute hello dotaz SQL na databázi MySQL. 

Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.

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

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. 

Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité toointerface hello MySQL serveru. Hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité tooestablish hello připojení toohello server. Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metoda je použité tooexecute hello dotaz SQL na databázi MySQL. pole výsledky Hello je použité toohold hello výsledky dotazu hello.

Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.

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

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **aktualizace** příkaz jazyka SQL. 

Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité toointerface hello MySQL serveru. Hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité tooestablish hello připojení toohello server. Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metoda je použité tooexecute hello dotaz SQL na databázi MySQL. 

Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.

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

## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL. 

Hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité toointerface hello MySQL serveru. Hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoda je použité tooestablish hello připojení toohello server. Hello [query()](https://github.com/mysqljs/mysql#performing-queries) metoda je použité tooexecute hello dotaz SQL na databázi MySQL. 

Nahraďte hello `host`, `user`, `password`, a `database` parametry s hello hodnoty, kterou jste zadali při vytvoření hello server a databáze.

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

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./concepts-migrate-import-export.md)
