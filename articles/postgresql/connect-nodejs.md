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
# <a name="azure-database-for-postgresql-use-nodejs-tooconnect-and-query-data"></a><span data-ttu-id="f676a-103">Azure databázi PostgreSQL: použití Node.js tooconnect a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="f676a-103">Azure Database for PostgreSQL: Use Node.js tooconnect and query data</span></span>
<span data-ttu-id="f676a-104">Tento rychlý start předvádí jak tooconnect tooan Azure databázi PostgreSQL pomocí [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="f676a-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="f676a-105">Zobrazuje jak toouse tooquery příkazy SQL, vložit, aktualizovat a odstranit data v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="f676a-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="f676a-106">Hello postup v tomto článku předpokládá, že jste obeznámeni s vývojem pomocí Node.js, a že jste novou tooworking s Azure databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-106">hello steps in this article assume that you are familiar with developing using Node.js, and that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f676a-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f676a-107">Prerequisites</span></span>
<span data-ttu-id="f676a-108">Tento rychlý start využívá prostředky hello vytvořené v některém z těchto průvodcích se dozvíte jako výchozí bod:</span><span class="sxs-lookup"><span data-stu-id="f676a-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="f676a-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="f676a-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="f676a-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f676a-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="f676a-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="f676a-111">You also need to:</span></span>
- <span data-ttu-id="f676a-112">Instalovat [Node.js](https://nodejs.org)</span><span class="sxs-lookup"><span data-stu-id="f676a-112">Install [Node.js](https://nodejs.org)</span></span>

## <a name="install-pg-client"></a><span data-ttu-id="f676a-113">Instalace klienta pg</span><span class="sxs-lookup"><span data-stu-id="f676a-113">Install pg client</span></span>
<span data-ttu-id="f676a-114">Nainstalujte [pg](https://www.npmjs.com/package/pg), což je PostgreSQL klienta pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="f676a-114">Install [pg](https://www.npmjs.com/package/pg), which is a PostgreSQL client for Node.js.</span></span>

<span data-ttu-id="f676a-115">toodo tedy spustit hello uzel Správce balíčků (npm) pro jazyk JavaScript z příkazového řádku tooinstall hello pg klienta.</span><span class="sxs-lookup"><span data-stu-id="f676a-115">toodo so, run hello node package manager (npm) for JavaScript from your command line tooinstall hello pg client.</span></span>
```bash
npm install pg
```

<span data-ttu-id="f676a-116">Ověření instalace hello tak, že uvedete hello balíčky nainstalované.</span><span class="sxs-lookup"><span data-stu-id="f676a-116">Verify hello installation by listing hello packages installed.</span></span>
```bash
npm list
```

## <a name="get-connection-information"></a><span data-ttu-id="f676a-117">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="f676a-117">Get connection information</span></span>
<span data-ttu-id="f676a-118">Získáte hello připojení informace potřebné tooconnect toohello databáze Azure pro PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-118">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="f676a-119">Musíte hello serveru plně kvalifikovaný název a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f676a-119">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="f676a-120">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f676a-120">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f676a-121">Hello levé nabídce na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledejte hello serveru, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f676a-121">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you just created.</span></span>
3. <span data-ttu-id="f676a-122">Klikněte na název serveru hello.</span><span class="sxs-lookup"><span data-stu-id="f676a-122">Click hello server name.</span></span>
4. <span data-ttu-id="f676a-123">Vyberte hello serveru **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="f676a-123">Select hello server's **Overview** page.</span></span> <span data-ttu-id="f676a-124">Poznamenejte si hello **název serveru** a **přihlašovací jméno pro Server správce**.</span><span class="sxs-lookup"><span data-stu-id="f676a-124">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="f676a-125">![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-nodejs/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="f676a-125">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-nodejs/1-connection-string.png)</span></span>
5. <span data-ttu-id="f676a-126">Pokud zapomenete vaše přihlašovací údaje serveru, přejděte toohello **přehled** stránka tooview hello serveru správce přihlašovací jméno a v případě potřeby obnovit heslo hello.</span><span class="sxs-lookup"><span data-stu-id="f676a-126">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="running-hello-javascript-code-in-nodejs"></a><span data-ttu-id="f676a-127">Spuštění kódu JavaScript hello v Node.js</span><span class="sxs-lookup"><span data-stu-id="f676a-127">Running hello JavaScript code in Node.js</span></span>
<span data-ttu-id="f676a-128">Node.js hello bash prostředí nebo windows příkazovém řádku může spustit zadáním `node`, spustit kód jazyka JavaScript příklad hello interaktivně kopírování a vkládání do hello řádku.</span><span class="sxs-lookup"><span data-stu-id="f676a-128">You may launch Node.js from hello bash shell or windows command prompt by typing `node`, then run hello example JavaScript code interactively by copy and pasting it onto hello prompt.</span></span> <span data-ttu-id="f676a-129">Alternativně může uložit hello kódu jazyka JavaScript do textového souboru a spuštění `node filename.js` s názvem souboru hello jako parametr toorun ho.</span><span class="sxs-lookup"><span data-stu-id="f676a-129">Alternatively, you may save hello JavaScript code into a text file and launch `node filename.js` with hello file name as a parameter toorun it.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="f676a-130">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="f676a-130">Connect, create table, and insert data</span></span>
<span data-ttu-id="f676a-131">Použití hello následující kód tooconnect a načtení dat pomocí hello **CREATE TABLE** a **INSERT INTO** příkazů SQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-131">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>
<span data-ttu-id="f676a-132">Hello [pg. Klient](https://github.com/brianc/node-postgres/wiki/Client) objekt je použité toointerface hello PostgreSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="f676a-132">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="f676a-133">Hello [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funkce je použité tooestablish hello připojení toohello server.</span><span class="sxs-lookup"><span data-stu-id="f676a-133">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="f676a-134">Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-134">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="f676a-135">Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="f676a-135">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="read-data"></a><span data-ttu-id="f676a-136">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="f676a-136">Read data</span></span>
<span data-ttu-id="f676a-137">Použití hello následující kód tooconnect a čtení dat pomocí hello **vyberte** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-137">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="f676a-138">Hello [pg. Klient](https://github.com/brianc/node-postgres/wiki/Client) objekt je použité toointerface hello PostgreSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="f676a-138">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="f676a-139">Hello [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funkce je použité tooestablish hello připojení toohello server.</span><span class="sxs-lookup"><span data-stu-id="f676a-139">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="f676a-140">Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-140">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="f676a-141">Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="f676a-141">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="f676a-142">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="f676a-142">Update data</span></span>
<span data-ttu-id="f676a-143">Použití hello následující kód tooconnect a čtení dat pomocí hello **aktualizace** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-143">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="f676a-144">Hello [pg. Klient](https://github.com/brianc/node-postgres/wiki/Client) objekt je použité toointerface hello PostgreSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="f676a-144">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="f676a-145">Hello [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funkce je použité tooestablish hello připojení toohello server.</span><span class="sxs-lookup"><span data-stu-id="f676a-145">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="f676a-146">Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-146">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="f676a-147">Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="f676a-147">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

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

## <a name="delete-data"></a><span data-ttu-id="f676a-148">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="f676a-148">Delete data</span></span>
<span data-ttu-id="f676a-149">Použití hello následující kód tooconnect a čtení dat pomocí hello **odstranit** příkaz jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-149">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="f676a-150">Hello [pg. Klient](https://github.com/brianc/node-postgres/wiki/Client) objekt je použité toointerface hello PostgreSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="f676a-150">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="f676a-151">Hello [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funkce je použité tooestablish hello připojení toohello server.</span><span class="sxs-lookup"><span data-stu-id="f676a-151">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="f676a-152">Hello [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funkce je použité tooexecute hello dotaz SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f676a-152">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="f676a-153">Nahraďte hello hodnoty, kterou jste zadali při vytvoření hello server a databáze hello hostitele, dbname, uživatele a heslo parametry.</span><span class="sxs-lookup"><span data-stu-id="f676a-153">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="f676a-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f676a-154">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="f676a-155">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="f676a-155">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
