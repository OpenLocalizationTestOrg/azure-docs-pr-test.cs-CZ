---
title: "Připojení tooAzure databáze pro PostgreSQL z Node.js | Microsoft Docs"
description: "Tento rychlý start poskytuje ukázka kódu Node.js můžete použít tooconnect a zadávat dotazy na data z databáze Azure pro PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 06/23/2017
ms.openlocfilehash: 9b269d72068ecc24bcf3fb447a2efeda512c698c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-nodejs-tooconnect-and-query-data"></a>Azure databázi PostgreSQL: použití Node.js tooconnect a dotazování dat
Tento rychlý start předvádí jak tooconnect tooan Azure databázi PostgreSQL pomocí [Node.js](https://nodejs.org/). Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello. Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí Node.js, a že jste novou tooworking s Azure databáze PostgreSQL.

## <a name="prerequisites"></a>Požadavky
Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](quickstart-create-server-database-azure-cli.md)

Budete také muset:
- Instalovat [Node.js](https://nodejs.org)

## <a name="install-pg-client"></a>Instalace klienta pg
Nainstalujte [pg](https://www.npmjs.com/package/pg), což je PostgreSQL klienta pro Node.js.

toodo tedy spustit hello uzel Správce balíčků (npm) pro jazyk JavaScript z příkazového řádku tooinstall hello pg klienta.
```bash
npm install pg
```

Ověření instalace hello tak, že uvedete hello balíčky nainstalované.
```bash
npm list
```

## <a name="get-connection-information"></a>Získání informací o připojení
Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL. Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, kterou jste právě vytvořili.
3. Klikněte na název serveru hello.
4. Vyberte hello serveru **přehled** stránky. Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.
 ![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-nodejs/1-connection-string.png)
5. Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.

## <a name="running-hello-javascript-code-in-nodejs"></a>Spuštění kódu JavaScript hello v Node.js
Node.js hello bash prostředí nebo windows příkazovém řádku může spustit zadáním `node`, spustit kód jazyka JavaScript příklad hello interaktivně kopírování a vkládání do hello řádku. Alternativně může uložit hello kódu jazyka JavaScript do textového souboru a spuštění `node filename.js` s názvem souboru hello jako parametr toorun ho.

## <a name="connect-create-table-and-insert-data"></a>Připojení, vytvoření tabulky a vložení dat
Použití hello následující kód tooconnect a načtení dat pomocí hello **CREATE TABLE** a **INSERT INTO** příkazů SQL.
Hello [pg. Klient](https://github.com/brianc/node-postgres/wiki/Client) objekt je použité toointerface hello PostgreSQL serveru. Hello [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funkce je použité tooestablish hello připojení toohello server. Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL. 

Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DROP TABLE IF EXISTS inventory;
        CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
        INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
        INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
        INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    `;

    client
        .query(query)
        .then(() => {
            console.log('Table created successfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
        });
}
```

## <a name="read-data"></a>Čtení dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL. Hello [pg. Klient](https://github.com/brianc/node-postgres/wiki/Client) objekt je použité toointerface hello PostgreSQL serveru. Hello [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funkce je použité tooestablish hello připojení toohello server. Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL. 

Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else { queryDatabase(); }
});

function queryDatabase() {
  
    console.log(`Running query tooPostgreSQL server: ${config.host}`);

    const query = 'SELECT * FROM inventory;';

    client.query(query)
        .then(res => {
            const rows = res.rows;

            rows.map(row => {
                console.log(`Read: ${JSON.stringify(row)}`);
            });

            process.exit();
        })
        .catch(err => {
            console.log(err);
        });
}
```

## <a name="update-data"></a>Aktualizace dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **aktualizace** příkaz jazyka SQL. Hello [pg. Klient](https://github.com/brianc/node-postgres/wiki/Client) objekt je použité toointerface hello PostgreSQL serveru. Hello [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funkce je použité tooestablish hello připojení toohello server. Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL. 

Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        UPDATE inventory 
        SET quantity= 1000 WHERE name='banana';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Update completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="delete-data"></a>Odstranění dat
Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL. Hello [pg. Klient](https://github.com/brianc/node-postgres/wiki/Client) objekt je použité toointerface hello PostgreSQL serveru. Hello [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funkce je použité tooestablish hello připojení toohello server. Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL. 

Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry. 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) {
        throw err;
    } else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DELETE FROM inventory 
        WHERE name = 'apple';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Delete completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)
