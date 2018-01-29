---
title: "Připojení k Azure Database for MySQL z Node.js | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete několik vzorových kódů Node.js, které můžete použít k připojení a dotazování dat ze služby Azure Database for MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 2f18016614b229273aa4d661991149be949ce238
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-nodejs-to-connect-and-query-data"></a>Azure Database for MySQL: Připojení a dotazování dat pomocí Node.js
Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí [Node.js](https://nodejs.org/) z platforem Windows, Ubuntu Linux a Mac. Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi. Toto téma předpokládá, že máte zkušenosti s vývojem pomocí Node.js a teprve začínáte pracovat se službou Azure Database for MySQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Budete také muset:
- Nainstalovat modul runtime [Node.js](https://nodejs.org).
- Nainstalovat balíček [mysql2](https://www.npmjs.com/package/mysql2) pro připojení k MySQL z aplikace Node.js. 

## <a name="install-nodejs-and-the-mysql-connector"></a>Instalace Node.js a konektoru MySQL
V závislosti na vaší platformě nainstalujte Node.js podle pokynů v příslušné části. Pomocí npm nainstalujte balíček mysql2 a jeho závislosti do složky vašeho projektu.

### <a name="windows"></a>**Windows**
1. Přejděte na [stránku pro stažení Node.js](https://nodejs.org/en/download/) a vyberte požadovanou možnost Instalační služby systému Windows.
2. Vytvořte místní složku projektu, například `nodejsmysql`. 
3. Spusťte příkazový řádek a přejděte do složky projektu, například `cd c:\nodejsmysql\`.
4. Spusťte nástroj NPM a nainstalujte knihovnu mysql2 do složky projektu.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. Ověřte instalaci kontrolou, že text výstupu příkazu `npm list` obsahuje `mysql2@1.3.5`.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
1. Spuštěním následujících příkazů nainstalujte **Node.js** a **npm** – správce balíčků pro Node.js.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Spuštěním následujících příkazů vytvořte složku projektu `mysqlnodejs` a nainstalujte do ní balíček mysql2.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. Ověřte instalaci kontrolou, že text výstupu příkazu npm list obsahuje `mysql2@1.3.5`.

### <a name="mac-os"></a>**Mac OS**
1. Zadejte následující příkazy, abyste nainstalovali **brew**, snadno použitelného správce balíčků pro Mac OS X a **Node.js**.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Spuštěním následujících příkazů vytvořte složku projektu `mysqlnodejs` a nainstalujte do ní balíček mysql2.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. Ověřte instalaci kontrolou, že text výstupu příkazu `npm list` obsahuje `mysql2@1.3.6`. Číslo verze se může lišit v závislosti na vydávání nových oprav.

## <a name="get-connection-information"></a>Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for MySQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V levém podokně klikněte na **Všechny prostředky** a potom vyhledejte server, který jste vytvořili (například **myserver4demo**).
3. Klikněte na název serveru **myserver4demo**.
4. Vyberte stránku **Vlastnosti** serveru a potom si poznamenejte **Název serveru** a **Přihlašovací jméno správce serveru**.
 ![Azure Database for MySQL – přihlašovací jméno správce serveru](./media/connect-nodejs/1_server-properties-name-login.png)
5. Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby obnovit heslo.

## <a name="running-the-javascript-code-in-nodejs"></a>Spuštění kódu jazyka JavaScript v Node.js
1. Vložte kód jazyka JavaScript do textového souboru a ten uložte s příponou .js do složky projektu (například C:\nodejsmysql\createtable.js nebo /home/username/nodejsmysql/createtable.js).
2. Spusťte příkazový řádek nebo prostředí Bash a příkazem `cd nodejsmysql` změňte adresář na složku vašeho projektu.
3. Pokud chcete aplikaci spustit, zadejte příkaz node následovaný názvem souboru, například `node createtable.js`.
4. Pokud v systému Windows není aplikace Node ve vaší proměnné prostředí PATH, možná bude nutné ke spuštění aplikace Node použít úplnou cestu, například `"C:\Program Files\nodejs\node.exe" createtable.js`.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Pomocí následujícího kódu se připojte a nahrajte data s využitím příkazů **CREATE TABLE** a **INSERT INTO** jazyka SQL.

Metoda [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) slouží k vytvoření rozhraní pro server MySQL. Funkce [connect()](https://github.com/mysqljs/mysql#establishing-connections) se používá k navázání připojení k serveru. Funkce [query()](https://github.com/mysqljs/mysql#performing-queries) se používá k provedení příkazu jazyka SQL na databázi MySQL. 

Parametry `host`, `user`, `password` a `database` nahraďte hodnotami, které jste zadali při vytváření serveru a databáze.

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
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL. 

Metoda [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) slouží k vytvoření rozhraní pro server MySQL. Metoda [connect()](https://github.com/mysqljs/mysql#establishing-connections) se používá k navázání připojení k serveru. Metoda [query()](https://github.com/mysqljs/mysql#performing-queries) se používá k provedení příkazu jazyka SQL na databázi MySQL. Pole results slouží k ukládání výsledků dotazu.

Parametry `host`, `user`, `password` a `database` nahraďte hodnotami, které jste zadali při vytváření serveru a databáze.

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
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **UPDATE** jazyka SQL. 

Metoda [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) slouží k vytvoření rozhraní pro server MySQL. Metoda [connect()](https://github.com/mysqljs/mysql#establishing-connections) se používá k navázání připojení k serveru. Metoda [query()](https://github.com/mysqljs/mysql#performing-queries) se používá k provedení příkazu jazyka SQL na databázi MySQL. 

Parametry `host`, `user`, `password` a `database` nahraďte hodnotami, které jste zadali při vytváření serveru a databáze.

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
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL. 

Metoda [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) slouží k vytvoření rozhraní pro server MySQL. Metoda [connect()](https://github.com/mysqljs/mysql#establishing-connections) se používá k navázání připojení k serveru. Metoda [query()](https://github.com/mysqljs/mysql#performing-queries) se používá k provedení příkazu jazyka SQL na databázi MySQL. 

Parametry `host`, `user`, `password` a `database` nahraďte hodnotami, které jste zadali při vytváření serveru a databáze.

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
