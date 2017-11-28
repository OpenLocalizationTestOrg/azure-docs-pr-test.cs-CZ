---
title: "Připojení k Azure Database for PostgreSQL z Node.js | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorek kódu Node.js, který můžete použít k připojení a dotazování dat ze služby Azure Database for PostgreSQL."
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
ms.openlocfilehash: f6c98833c73b70bcf1f8ca53596a34f09807b276
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-nodejs-to-connect-and-query-data"></a><span data-ttu-id="5ab56-103">Azure Database for PostgreSQL: Použití Node.js k připojení a dotazování dat</span><span class="sxs-lookup"><span data-stu-id="5ab56-103">Azure Database for PostgreSQL: Use Node.js to connect and query data</span></span>
<span data-ttu-id="5ab56-104">Tento rychlý start předvádí, jak se připojit k databázi Azure pro používání PostgreSQL [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="5ab56-104">This quickstart demonstrates how to connect to an Azure Database for PostgreSQL using [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="5ab56-105">Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi.</span><span class="sxs-lookup"><span data-stu-id="5ab56-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="5ab56-106">Kroky v tomto článku předpokládají, že máte zkušenosti s vývojem pomocí Node.js a teprve začínáte pracovat se službou Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-106">The steps in this article assume that you are familiar with developing using Node.js, and that you are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ab56-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5ab56-107">Prerequisites</span></span>
<span data-ttu-id="5ab56-108">Tento rychlý start využívá jako výchozí bod prostředky vytvořené v některém z těchto průvodců:</span><span class="sxs-lookup"><span data-stu-id="5ab56-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="5ab56-109">Vytvoření databáze – portál</span><span class="sxs-lookup"><span data-stu-id="5ab56-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="5ab56-110">Vytvoření databáze – rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5ab56-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="5ab56-111">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="5ab56-111">You also need to:</span></span>
- <span data-ttu-id="5ab56-112">Instalovat [Node.js](https://nodejs.org)</span><span class="sxs-lookup"><span data-stu-id="5ab56-112">Install [Node.js](https://nodejs.org)</span></span>

## <a name="install-pg-client"></a><span data-ttu-id="5ab56-113">Instalace klienta pg</span><span class="sxs-lookup"><span data-stu-id="5ab56-113">Install pg client</span></span>
<span data-ttu-id="5ab56-114">Nainstalujte [pg](https://www.npmjs.com/package/pg), což je PostgreSQL klienta pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="5ab56-114">Install [pg](https://www.npmjs.com/package/pg), which is a PostgreSQL client for Node.js.</span></span>

<span data-ttu-id="5ab56-115">Abyste to mohli udělat, spusťte správce balíčků pro uzly (npm) pro JavaScript z příkazového řádku, abyste instalovali klienta pg.</span><span class="sxs-lookup"><span data-stu-id="5ab56-115">To do so, run the node package manager (npm) for JavaScript from your command line to install the pg client.</span></span>
```bash
npm install pg
```

<span data-ttu-id="5ab56-116">Ověřte instalaci tak, že vypíšete nainstalované balíčky.</span><span class="sxs-lookup"><span data-stu-id="5ab56-116">Verify the installation by listing the packages installed.</span></span>
```bash
npm list
```

## <a name="get-connection-information"></a><span data-ttu-id="5ab56-117">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="5ab56-117">Get connection information</span></span>
<span data-ttu-id="5ab56-118">Získejte informace o připojení potřebné pro připojení ke službě Azure Database for PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-118">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="5ab56-119">Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="5ab56-119">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="5ab56-120">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5ab56-120">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="5ab56-121">Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **všechny prostředky** a vyhledávání pro server, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="5ab56-121">From the left-hand menu in Azure portal, click **All resources** and search for the server you just created.</span></span>
3. <span data-ttu-id="5ab56-122">Klikněte na název serveru.</span><span class="sxs-lookup"><span data-stu-id="5ab56-122">Click the server name.</span></span>
4. <span data-ttu-id="5ab56-123">Vyberte stránku **Přehled** serveru.</span><span class="sxs-lookup"><span data-stu-id="5ab56-123">Select the server's **Overview** page.</span></span> <span data-ttu-id="5ab56-124">Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.</span><span class="sxs-lookup"><span data-stu-id="5ab56-124">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="5ab56-125">![Azure Database for PostgreSQL – přihlašovací jméno správce serveru](./media/connect-nodejs/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="5ab56-125">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-nodejs/1-connection-string.png)</span></span>
5. <span data-ttu-id="5ab56-126">Pokud zapomenete přihlašovací údaje k serveru, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="5ab56-126">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="running-the-javascript-code-in-nodejs"></a><span data-ttu-id="5ab56-127">Spuštění kódu jazyka JavaScript v Node.js</span><span class="sxs-lookup"><span data-stu-id="5ab56-127">Running the JavaScript code in Node.js</span></span>
<span data-ttu-id="5ab56-128">Node.js můžete spouštět z prostředí Bash nebo příkazového řádku Windows tak, že zadáte `node` a potom interaktivně spustíte příklad kódu jazyka JavaScript tak, že ho zkopírujete a vložíte na příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="5ab56-128">You may launch Node.js from the bash shell or windows command prompt by typing `node`, then run the example JavaScript code interactively by copy and pasting it onto the prompt.</span></span> <span data-ttu-id="5ab56-129">Případně můžete kód jazyka JavaScript uložit do textového souboru a provést příkaz `node filename.js`, kdy název souboru se bude shodovat s parametrem, který ho spouští.</span><span class="sxs-lookup"><span data-stu-id="5ab56-129">Alternatively, you may save the JavaScript code into a text file and launch `node filename.js` with the file name as a parameter to run it.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="5ab56-130">Připojení, vytvoření tabulky a vložení dat</span><span class="sxs-lookup"><span data-stu-id="5ab56-130">Connect, create table, and insert data</span></span>
<span data-ttu-id="5ab56-131">Použijte následující kód k připojení a načtení dat pomocí příkazů **CREATE TABLE** a **INSERT INTO** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-131">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>
<span data-ttu-id="5ab56-132">Objekt [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) slouží k vytvoření rozhraní pro server PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-132">The [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used to interface with the PostgreSQL server.</span></span> <span data-ttu-id="5ab56-133">Funkce [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) se používá k navázání připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="5ab56-133">The [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used to establish the connection to the server.</span></span> <span data-ttu-id="5ab56-134">Funkce [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) se používá k provedení dotazu SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-134">The [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used to execute the SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="5ab56-135">Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="5ab56-135">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

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

## <a name="read-data"></a><span data-ttu-id="5ab56-136">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="5ab56-136">Read data</span></span>
<span data-ttu-id="5ab56-137">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-137">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="5ab56-138">Objekt [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) slouží k vytvoření rozhraní pro server PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-138">The [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used to interface with the PostgreSQL server.</span></span> <span data-ttu-id="5ab56-139">Funkce [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) se používá k navázání připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="5ab56-139">The [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used to establish the connection to the server.</span></span> <span data-ttu-id="5ab56-140">Funkce [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) se používá k provedení dotazu SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-140">The [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used to execute the SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="5ab56-141">Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="5ab56-141">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span> 

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
  
    console.log(`Running query to PostgreSQL server: ${config.host}`);

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

## <a name="update-data"></a><span data-ttu-id="5ab56-142">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="5ab56-142">Update data</span></span>
<span data-ttu-id="5ab56-143">Použijte následující kód k připojení a čtení dat pomocí příkazu **UPDATE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-143">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="5ab56-144">Objekt [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) slouží k vytvoření rozhraní pro server PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-144">The [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used to interface with the PostgreSQL server.</span></span> <span data-ttu-id="5ab56-145">Funkce [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) se používá k navázání připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="5ab56-145">The [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used to establish the connection to the server.</span></span> <span data-ttu-id="5ab56-146">Funkce [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) se používá k provedení dotazu SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-146">The [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used to execute the SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="5ab56-147">Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="5ab56-147">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span> 

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

## <a name="delete-data"></a><span data-ttu-id="5ab56-148">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="5ab56-148">Delete data</span></span>
<span data-ttu-id="5ab56-149">Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-149">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="5ab56-150">Objekt [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) slouží k vytvoření rozhraní pro server PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-150">The [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used to interface with the PostgreSQL server.</span></span> <span data-ttu-id="5ab56-151">Funkce [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) se používá k navázání připojení k serveru.</span><span class="sxs-lookup"><span data-stu-id="5ab56-151">The [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used to establish the connection to the server.</span></span> <span data-ttu-id="5ab56-152">Funkce [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) se používá k provedení dotazu SQL na databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5ab56-152">The [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used to execute the SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="5ab56-153">Nahraďte parametry host (hostitel), dbname (název databáze), user (uživatel) a password (heslo) hodnotami, které jste zadali při vytváření serveru a databáze.</span><span class="sxs-lookup"><span data-stu-id="5ab56-153">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="5ab56-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5ab56-154">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="5ab56-155">Migrace vaší databáze pomocí exportu a importu</span><span class="sxs-lookup"><span data-stu-id="5ab56-155">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
