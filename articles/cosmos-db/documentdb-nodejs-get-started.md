---
title: "kurz aaaNode.js pro hello DocumentDB rozhraní API pro Azure Cosmos DB | Microsoft Docs"
description: "Kurz Node.js, která vytvoří Cosmos DB s hello DocumentDB rozhraní API."
keywords: "kurz node.js, databáze uzlů"
services: cosmos-db
documentationcenter: node.js
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: 14d52110-1dce-4ac0-9dd9-f936afccd550
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: article
ms.date: 08/14/2017
ms.author: anhoh
ms.openlocfilehash: fce244c6a5f321608e82ca51a2c987e84b98bffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-tutorial-use-hello-documentdb-api-in-azure-cosmos-db-toocreate-a-nodejs-console-application"></a><span data-ttu-id="2172f-104">Kurz k Node.js: použití hello DocumentDB rozhraní API v Azure Cosmos DB toocreate konzolovou aplikaci Node.js</span><span class="sxs-lookup"><span data-stu-id="2172f-104">Node.js tutorial: Use hello DocumentDB API in Azure Cosmos DB toocreate a Node.js console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2172f-105">.NET</span><span class="sxs-lookup"><span data-stu-id="2172f-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="2172f-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="2172f-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="2172f-107">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="2172f-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="2172f-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="2172f-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="2172f-109">Java</span><span class="sxs-lookup"><span data-stu-id="2172f-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="2172f-110">C++</span><span class="sxs-lookup"><span data-stu-id="2172f-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="2172f-111">Vítejte v kurzu Node.js toohello pro hello Azure Cosmos DB Node.js SDK!</span><span class="sxs-lookup"><span data-stu-id="2172f-111">Welcome toohello Node.js tutorial for hello Azure Cosmos DB Node.js SDK!</span></span> <span data-ttu-id="2172f-112">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="2172f-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="2172f-113">Budeme se zabývat těmito tématy:</span><span class="sxs-lookup"><span data-stu-id="2172f-113">We'll cover:</span></span>

* <span data-ttu-id="2172f-114">Vytvoření a připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="2172f-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="2172f-115">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="2172f-115">Setting up your application</span></span>
* <span data-ttu-id="2172f-116">Vytvoření databáze Node</span><span class="sxs-lookup"><span data-stu-id="2172f-116">Creating a node database</span></span>
* <span data-ttu-id="2172f-117">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="2172f-117">Creating a collection</span></span>
* <span data-ttu-id="2172f-118">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="2172f-118">Creating JSON documents</span></span>
* <span data-ttu-id="2172f-119">Dotazování na kolekci hello</span><span class="sxs-lookup"><span data-stu-id="2172f-119">Querying hello collection</span></span>
* <span data-ttu-id="2172f-120">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="2172f-120">Replacing a document</span></span>
* <span data-ttu-id="2172f-121">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="2172f-121">Deleting a document</span></span>
* <span data-ttu-id="2172f-122">Odstranění databáze node hello</span><span class="sxs-lookup"><span data-stu-id="2172f-122">Deleting hello node database</span></span>

<span data-ttu-id="2172f-123">Nemáte čas?</span><span class="sxs-lookup"><span data-stu-id="2172f-123">Don't have time?</span></span> <span data-ttu-id="2172f-124">Nevadí!</span><span class="sxs-lookup"><span data-stu-id="2172f-124">Don't worry!</span></span> <span data-ttu-id="2172f-125">Hello úplné řešení je k dispozici na [Githubu](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="2172f-125">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span> <span data-ttu-id="2172f-126">V tématu [získání úplného řešení hello](#GetSolution) pro rychlé pokyny.</span><span class="sxs-lookup"><span data-stu-id="2172f-126">See [Get hello complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="2172f-127">Po dokončení kurzu Node.js hello, prosím použijte hello hlasovací tlačítka v hello horní a dolní části této stránky toogive nám zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="2172f-127">After you've completed hello Node.js tutorial, please use hello voting buttons at hello top and bottom of this page toogive us feedback.</span></span> <span data-ttu-id="2172f-128">Pokud byste nám chtěli toocontact přímo, cítíte volné tooinclude e-mailovou adresou v komentářích.</span><span class="sxs-lookup"><span data-stu-id="2172f-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="2172f-129">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="2172f-129">Now let's get started!</span></span>

## <a name="prerequisites-for-hello-nodejs-tutorial"></a><span data-ttu-id="2172f-130">Předpoklady pro kurz k Node.js hello</span><span class="sxs-lookup"><span data-stu-id="2172f-130">Prerequisites for hello Node.js tutorial</span></span>
<span data-ttu-id="2172f-131">Přesvědčte se, že máte následující hello:</span><span class="sxs-lookup"><span data-stu-id="2172f-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="2172f-132">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="2172f-132">An active Azure account.</span></span> <span data-ttu-id="2172f-133">Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2172f-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
    * <span data-ttu-id="2172f-134">Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2172f-134">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="2172f-135">[Node.js](https://nodejs.org/) verze 0.10.29 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="2172f-135">[Node.js](https://nodejs.org/) version v0.10.29 or higher.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="2172f-136">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2172f-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="2172f-137">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2172f-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="2172f-138">Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[nastavení aplikace Node.js](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="2172f-138">If you already have an account you want toouse, you can skip ahead too[Set up your Node.js application](#SetupNode).</span></span> <span data-ttu-id="2172f-139">Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení aplikace Node.js](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="2172f-139">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Node.js application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="2172f-140"><a id="SetupNode"></a>Krok 2: Nastavení aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="2172f-140"><a id="SetupNode"></a>Step 2: Set up your Node.js application</span></span>
1. <span data-ttu-id="2172f-141">Otevřete svůj oblíbený terminál.</span><span class="sxs-lookup"><span data-stu-id="2172f-141">Open your favorite terminal.</span></span>
2. <span data-ttu-id="2172f-142">Vyhledejte hello složku nebo adresář, kam chcete toosave aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="2172f-142">Locate hello folder or directory where you'd like toosave your Node.js application.</span></span>
3. <span data-ttu-id="2172f-143">Vytvořte dva prázdné Javascriptové soubory s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2172f-143">Create two empty JavaScript files with hello following commands:</span></span>
   * <span data-ttu-id="2172f-144">Windows:</span><span class="sxs-lookup"><span data-stu-id="2172f-144">Windows:</span></span>
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * <span data-ttu-id="2172f-145">Linux nebo OS X:</span><span class="sxs-lookup"><span data-stu-id="2172f-145">Linux/OS X:</span></span>
     * ```touch app.js```
     * ```touch config.js```
4. <span data-ttu-id="2172f-146">Nainstalujte pomocí npm modul documentdb hello.</span><span class="sxs-lookup"><span data-stu-id="2172f-146">Install hello documentdb module via npm.</span></span> <span data-ttu-id="2172f-147">Hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2172f-147">Use hello following command:</span></span>
   * ```npm install documentdb --save```

<span data-ttu-id="2172f-148">Výborně!</span><span class="sxs-lookup"><span data-stu-id="2172f-148">Great!</span></span> <span data-ttu-id="2172f-149">Teď když jste dokončili nastavování, napišme nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="2172f-149">Now that you've finished setting up, let's start writing some code.</span></span>

## <span data-ttu-id="2172f-150"><a id="Config"></a>Krok 3: Nastavení konfigurací aplikace</span><span class="sxs-lookup"><span data-stu-id="2172f-150"><a id="Config"></a>Step 3: Set your app's configurations</span></span>
<span data-ttu-id="2172f-151">Ve svém oblíbeném textovém editoru otevřete ```config.js```.</span><span class="sxs-lookup"><span data-stu-id="2172f-151">Open ```config.js``` in your favorite text editor.</span></span>

<span data-ttu-id="2172f-152">Potom, kopírování a vložení hello fragment kódu níže a nastavte vlastnosti ```config.endpoint``` a ```config.primaryKey``` tooyour Azure Cosmos DB koncový bod uri a primary key.</span><span class="sxs-lookup"><span data-stu-id="2172f-152">Then, copy and paste hello code snippet below and set properties ```config.endpoint``` and ```config.primaryKey``` tooyour Azure Cosmos DB endpoint uri and primary key.</span></span> <span data-ttu-id="2172f-153">Obě tyto konfigurace lze nalézt v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2172f-153">Both these configurations can be found in hello [Azure portal](https://portal.azure.com).</span></span>

![Kurz k Node.js – snímek obrazovky portálu Azure znázorňující účet Azure Cosmos DB s AKTIVNÍM centrem hello hello zvýrazněným tlačítkem klíče hello v okně účtu Azure Cosmos DB hello a hello identifikátor URI, primární a sekundární klíč zvýrazněnými hodnotami v hello Okna klíče – databáze Node][keys]

    // ADD THIS PART tooYOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

<span data-ttu-id="2172f-155">Zkopírujte a vložte hello ```database id```, ```collection id```, a ```JSON documents``` tooyour ```config``` objekt níže kde nastavíte vaše ```config.endpoint``` a ```config.authKey``` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2172f-155">Copy and paste hello ```database id```, ```collection id```, and ```JSON documents``` tooyour ```config``` object below where you set your ```config.endpoint``` and ```config.authKey``` properties.</span></span> <span data-ttu-id="2172f-156">Pokud již máte data, které byste chtěli toostore v databázi, můžete použít Azure Cosmos DB [nástroj pro migraci dat](import-data.md) místo přidávání definic dokumentů hello.</span><span class="sxs-lookup"><span data-stu-id="2172f-156">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) rather than adding hello document definitions.</span></span>

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART tooYOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


<span data-ttu-id="2172f-157">Hello databáze, kolekce a definice dokumentů bude fungovat jako vaše Azure DB Cosmos ```database id```, ```collection id```a data dokumentů.</span><span class="sxs-lookup"><span data-stu-id="2172f-157">hello database, collection, and document definitions will act as your Azure Cosmos DB ```database id```, ```collection id```, and documents' data.</span></span>

<span data-ttu-id="2172f-158">Nakonec exportujte vaše ```config``` objektu, takže můžete použít v rámci hello ```app.js``` souboru.</span><span class="sxs-lookup"><span data-stu-id="2172f-158">Finally, export your ```config``` object, so that you can reference it within hello ```app.js``` file.</span></span>

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART tooYOUR CODE
    module.exports = config;

## <span data-ttu-id="2172f-159"><a id="Connect"></a>Krok 4: Připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="2172f-159"><a id="Connect"></a> Step 4: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="2172f-160">Otevřete prázdný ```app.js``` soubor v textovém editoru hello.</span><span class="sxs-lookup"><span data-stu-id="2172f-160">Open your empty ```app.js``` file in hello text editor.</span></span> <span data-ttu-id="2172f-161">Zkopírujte a vložte kód hello pod tooimport hello ```documentdb``` modulu a nově vytvořený ```config``` modulu.</span><span class="sxs-lookup"><span data-stu-id="2172f-161">Copy and paste hello code below tooimport hello ```documentdb``` module and your newly created ```config``` module.</span></span>

    // ADD THIS PART tooYOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

<span data-ttu-id="2172f-162">Zkopírujte a vložte hello kód toouse hello uložili dřív ```config.endpoint``` a ```config.primaryKey``` toocreate nové instance DocumentClient.</span><span class="sxs-lookup"><span data-stu-id="2172f-162">Copy and paste hello code toouse hello previously saved ```config.endpoint``` and ```config.primaryKey``` toocreate a new DocumentClient.</span></span>

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART tooYOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

<span data-ttu-id="2172f-163">Teď, když máte hello kód tooinitialize hello Azure Cosmos DB klienta, Podívejme se na práci s prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2172f-163">Now that you have hello code tooinitialize hello Azure Cosmos DB client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <a name="step-5-create-a-node-database"></a><span data-ttu-id="2172f-164">Krok 5: Vytvoření databáze Node</span><span class="sxs-lookup"><span data-stu-id="2172f-164">Step 5: Create a Node database</span></span>
<span data-ttu-id="2172f-165">Zkopírujte a vložte kód hello pod tooset hello stav protokolu HTTP pro nebyl nalezen, url databáze hello a adresa url kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="2172f-165">Copy and paste hello code below tooset hello HTTP status for Not Found, hello database url, and hello collection url.</span></span> <span data-ttu-id="2172f-166">Tyto adresy URL se, jak bude klient Azure Cosmos DB hello najít hello správné databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="2172f-166">These urls are how hello Azure Cosmos DB client will find hello right database and collection.</span></span>

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART tooYOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

<span data-ttu-id="2172f-167">A [databáze](documentdb-resources.md#databases) lze vytvořit pomocí hello [metodu createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funkce hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="2172f-167">A [database](documentdb-resources.md#databases) can be created by using hello [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="2172f-168">Databáze je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi hello.</span><span class="sxs-lookup"><span data-stu-id="2172f-168">A database is hello logical container of document storage partitioned across collections.</span></span>

<span data-ttu-id="2172f-169">Zkopírujte a vložte hello **getDatabase** funkci pro vytvoření nové databáze v souboru app.js hello s hello ```id``` zadaný v hello ```config``` objektu.</span><span class="sxs-lookup"><span data-stu-id="2172f-169">Copy and paste hello **getDatabase** function for creating your new database in hello app.js file with hello ```id``` specified in hello ```config``` object.</span></span> <span data-ttu-id="2172f-170">Hello funkce zkontroluje, jestli hello databáze s hello stejné ```FamilyRegistry``` id již neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2172f-170">hello function will check if hello database with hello same ```FamilyRegistry``` id does not already exist.</span></span> <span data-ttu-id="2172f-171">Pokud ano, místo vytvoření nové databáze se vrátí databáze s tímto ID.</span><span class="sxs-lookup"><span data-stu-id="2172f-171">If it does exist, we'll return that database instead of creating a new one.</span></span>

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART tooYOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

<span data-ttu-id="2172f-172">Zkopírujte a vložte následující kód hello, kde nastavíte hello **getDatabase** funkce tooadd hello pomocné funkce **ukončete** ta vypíše hello závěrečnou zprávu a volání hello příliš**getDatabase** funkce.</span><span class="sxs-lookup"><span data-stu-id="2172f-172">Copy and paste hello code below where you set hello **getDatabase** function tooadd hello helper function **exit** that will print hello exit message and hello call too**getDatabase** function.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key tooexit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="2172f-173">V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="2172f-173">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="2172f-174">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2172f-174">Congratulations!</span></span> <span data-ttu-id="2172f-175">Úspěšně jste vytvořili databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2172f-175">You have successfully created an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="2172f-176"><a id="CreateColl"></a>Krok 6: Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="2172f-176"><a id="CreateColl"></a>Step 6: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="2172f-177">**CreateDocumentCollectionAsync** vytvoří novou kolekci, za kterou se hradí poplatky.</span><span class="sxs-lookup"><span data-stu-id="2172f-177">**CreateDocumentCollectionAsync** will create a new collection, which has pricing implications.</span></span> <span data-ttu-id="2172f-178">Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="2172f-178">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="2172f-179">A [kolekce](documentdb-resources.md#collections) lze vytvořit pomocí hello [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funkce hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="2172f-179">A [collection](documentdb-resources.md#collections) can be created by using hello [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="2172f-180">Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2172f-180">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="2172f-181">Zkopírujte a vložte hello **getCollection** funkce pod hello **getDatabase** fungovat v toocreate souboru app.js hello nové kolekce s hello ```id``` zadaný v hello ```config```objektu.</span><span class="sxs-lookup"><span data-stu-id="2172f-181">Copy and paste hello **getCollection** function underneath hello **getDatabase** function in hello app.js file toocreate your new collection with hello ```id``` specified in hello ```config``` object.</span></span> <span data-ttu-id="2172f-182">Opět zkontrolujeme, toomake opravdu kolekce se hello stejné ```FamilyCollection``` id již neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2172f-182">Again, we'll check toomake sure a collection with hello same ```FamilyCollection``` id does not already exist.</span></span> <span data-ttu-id="2172f-183">Pokud existuje, místo vytvoření nové kolekce se vrátí kolekce s tímto ID.</span><span class="sxs-lookup"><span data-stu-id="2172f-183">If it does exist, we'll return that collection instead of creating a new one.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

<span data-ttu-id="2172f-184">Zkopírujte a vložte kód hello pod hello volání příliš**getDatabase** tooexecute hello **getCollection** funkce.</span><span class="sxs-lookup"><span data-stu-id="2172f-184">Copy and paste hello code below hello call too**getDatabase** tooexecute hello **getCollection** function.</span></span>

    getDatabase()

    // ADD THIS PART tooYOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="2172f-185">V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="2172f-185">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="2172f-186">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2172f-186">Congratulations!</span></span> <span data-ttu-id="2172f-187">Úspěšně jste vytvořili kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2172f-187">You have successfully created an Azure Cosmos DB collection.</span></span>

## <span data-ttu-id="2172f-188"><a id="CreateDoc"></a>Krok 7: Vytvoření dokumentu</span><span class="sxs-lookup"><span data-stu-id="2172f-188"><a id="CreateDoc"></a>Step 7: Create a document</span></span>
<span data-ttu-id="2172f-189">A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funkce hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="2172f-189">A [document](documentdb-resources.md#documents) can be created by using hello [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="2172f-190">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="2172f-190">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="2172f-191">Nyní můžete do služby Azure Cosmos DB vložit dokument.</span><span class="sxs-lookup"><span data-stu-id="2172f-191">You can now insert a document into Azure Cosmos DB.</span></span>

<span data-ttu-id="2172f-192">Zkopírujte a vložte hello **getFamilyDocument** funkce pod hello **getCollection** funkce pro vytváření hello dokumenty obsahující data JSON hello uložené v hello ```config``` objektu.</span><span class="sxs-lookup"><span data-stu-id="2172f-192">Copy and paste hello **getFamilyDocument** function underneath hello **getCollection** function for creating hello documents containing hello JSON data saved in hello ```config``` object.</span></span> <span data-ttu-id="2172f-193">Opět zkontrolujeme toomake opravdu dokument s hello stejným id už neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2172f-193">Again, we'll check toomake sure a document with hello same id does not already exist.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

<span data-ttu-id="2172f-194">Zkopírujte a vložte kód hello pod hello volání příliš**getCollection** tooexecute hello **getFamilyDocument** funkce.</span><span class="sxs-lookup"><span data-stu-id="2172f-194">Copy and paste hello code below hello call too**getCollection** tooexecute hello **getFamilyDocument** function.</span></span>

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="2172f-195">V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="2172f-195">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="2172f-196">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2172f-196">Congratulations!</span></span> <span data-ttu-id="2172f-197">Úspěšně jste vytvořili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2172f-197">You have successfully created an Azure Cosmos DB document.</span></span>

![Kurz k Node.js – Diagram ilustrující hierarchický vztah hello mezi hello účet, hello databáze, kolekce hello a dokumenty hello – uzlu databáze](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <span data-ttu-id="2172f-199"><a id="Query"></a>Krok 8: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2172f-199"><a id="Query"></a>Step 8: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="2172f-200">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="2172f-200">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="2172f-201">Hello následující vzorový kód ukazuje dotaz, který můžete spustit na hello dokumenty v kolekci.</span><span class="sxs-lookup"><span data-stu-id="2172f-201">hello following sample code shows a query that you can run against hello documents in your collection.</span></span>

<span data-ttu-id="2172f-202">Zkopírujte a vložte hello **queryCollection** funkce pod hello **getFamilyDocument** funkce v souboru app.js hello.</span><span class="sxs-lookup"><span data-stu-id="2172f-202">Copy and paste hello **queryCollection** function underneath hello **getFamilyDocument** function in hello app.js file.</span></span> <span data-ttu-id="2172f-203">Azure Cosmos DB podporuje dotazy podobné jazyku SQL, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="2172f-203">Azure Cosmos DB supports SQL-like queries as shown below.</span></span> <span data-ttu-id="2172f-204">Další informace o vytváření komplexních dotazů, podívejte se na hello [Query Playground](https://www.documentdb.com/sql/demo) a hello [dokumentaci k dotazům](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="2172f-204">For more information on building complex queries, check out hello [Query Playground](https://www.documentdb.com/sql/demo) and hello [query documentation](documentdb-sql-query.md).</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


<span data-ttu-id="2172f-205">Hello následující diagram znázorňuje, jak se hello Azure Cosmos DB SQL dotazu, že se volá syntaxe proti kolekci hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2172f-205">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created.</span></span>

![Kurz k Node.js – Diagram ilustrující hello obor a význam dotazu hello – uzlu databáze](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

<span data-ttu-id="2172f-207">Hello [FROM](documentdb-sql-query.md#FromClause) – klíčové slovo je volitelný hello dotazu, protože Azure Cosmos DB dotazy jsou již vymezená tooa jedinou kolekci.</span><span class="sxs-lookup"><span data-stu-id="2172f-207">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="2172f-208">Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte.</span><span class="sxs-lookup"><span data-stu-id="2172f-208">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="2172f-209">Azure Cosmos DB odvodí, že rodiny, root nebo název proměnné hello jste vybrali, odkaz na aktuální kolekci hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2172f-209">Azure Cosmos DB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

<span data-ttu-id="2172f-210">Zkopírujte a vložte kód hello pod hello volání příliš**getFamilyDocument** tooexecute hello **queryCollection** funkce.</span><span class="sxs-lookup"><span data-stu-id="2172f-210">Copy and paste hello code below hello call too**getFamilyDocument** tooexecute hello **queryCollection** function.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART tooYOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="2172f-211">V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="2172f-211">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="2172f-212">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2172f-212">Congratulations!</span></span> <span data-ttu-id="2172f-213">Úspěšně jste provedli dotaz na dokumenty Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2172f-213">You have successfully queried Azure Cosmos DB documents.</span></span>

## <span data-ttu-id="2172f-214"><a id="ReplaceDocument"></a>Krok 9: Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="2172f-214"><a id="ReplaceDocument"></a>Step 9: Replace a document</span></span>
<span data-ttu-id="2172f-215">Azure Cosmos DB podporuje nahrazování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="2172f-215">Azure Cosmos DB supports replacing JSON documents.</span></span>

<span data-ttu-id="2172f-216">Zkopírujte a vložte hello **replaceFamilyDocument** funkce pod hello **queryCollection** funkce v souboru app.js hello.</span><span class="sxs-lookup"><span data-stu-id="2172f-216">Copy and paste hello **replaceFamilyDocument** function underneath hello **queryCollection** function in hello app.js file.</span></span>

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

<span data-ttu-id="2172f-217">Zkopírujte a vložte kód hello pod hello volání příliš**queryCollection** tooexecute hello **replaceDocument** funkce.</span><span class="sxs-lookup"><span data-stu-id="2172f-217">Copy and paste hello code below hello call too**queryCollection** tooexecute hello **replaceDocument** function.</span></span> <span data-ttu-id="2172f-218">Navíc přidat hello kódu toocall **queryCollection** znovu tooverify hello dokument úspěšně změnil.</span><span class="sxs-lookup"><span data-stu-id="2172f-218">Also, add hello code toocall **queryCollection** again tooverify that hello document had successfully changed.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="2172f-219">V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="2172f-219">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="2172f-220">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2172f-220">Congratulations!</span></span> <span data-ttu-id="2172f-221">Úspěšně jste nahradili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2172f-221">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="2172f-222"><a id="DeleteDocument"></a>Krok 10: Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="2172f-222"><a id="DeleteDocument"></a>Step 10: Delete a document</span></span>
<span data-ttu-id="2172f-223">Azure Cosmos DB podporuje odstraňování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="2172f-223">Azure Cosmos DB supports deleting JSON documents.</span></span>

<span data-ttu-id="2172f-224">Zkopírujte a vložte hello **deleteFamilyDocument** funkce pod hello **replaceFamilyDocument** funkce.</span><span class="sxs-lookup"><span data-stu-id="2172f-224">Copy and paste hello **deleteFamilyDocument** function underneath hello **replaceFamilyDocument** function.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART tooYOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

<span data-ttu-id="2172f-225">Zkopírujte a vložte kód hello pod hello volání toohello druhý **queryCollection** tooexecute hello **deleteDocument** funkce.</span><span class="sxs-lookup"><span data-stu-id="2172f-225">Copy and paste hello code below hello call toohello second **queryCollection** tooexecute hello **deleteDocument** function.</span></span>

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="2172f-226">V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="2172f-226">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="2172f-227">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2172f-227">Congratulations!</span></span> <span data-ttu-id="2172f-228">Úspěšně jste odstranili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2172f-228">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="2172f-229"><a id="DeleteDatabase"></a>Krok 11: Odstranění databáze Node hello</span><span class="sxs-lookup"><span data-stu-id="2172f-229"><a id="DeleteDatabase"></a>Step 11: Delete hello Node database</span></span>
<span data-ttu-id="2172f-230">Odstraňování hello vytvořené databáze dojde k odebrání hello databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="2172f-230">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="2172f-231">Zkopírujte a vložte hello **čištění** funkce pod hello **deleteFamilyDocument** funkce tooremove hello databázi a všechny podřízené prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="2172f-231">Copy and paste hello **cleanup** function underneath hello **deleteFamilyDocument** function tooremove hello database and all hello children resources.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART tooYOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

<span data-ttu-id="2172f-232">Zkopírujte a vložte kód hello pod hello volání příliš**deleteFamilyDocument** tooexecute hello **čištění** funkce.</span><span class="sxs-lookup"><span data-stu-id="2172f-232">Copy and paste hello code below hello call too**deleteFamilyDocument** tooexecute hello **cleanup** function.</span></span>

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART tooYOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <span data-ttu-id="2172f-233"><a id="Run"></a>Krok 12: Spuštění celé aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="2172f-233"><a id="Run"></a>Step 12: Run your Node.js application all together!</span></span>
<span data-ttu-id="2172f-234">Jako celek by pořadí hello volání funkcí by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2172f-234">Altogether, hello sequence for calling your functions should look like this:</span></span>

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="2172f-235">V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="2172f-235">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="2172f-236">Měli byste vidět hello výstup počáteční aplikace.</span><span class="sxs-lookup"><span data-stu-id="2172f-236">You should see hello output of your get started app.</span></span> <span data-ttu-id="2172f-237">výstup Hello by měl odpovídat ukázkovému textu hello níže.</span><span class="sxs-lookup"><span data-stu-id="2172f-237">hello output should match hello example text below.</span></span>

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key tooexit

<span data-ttu-id="2172f-238">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2172f-238">Congratulations!</span></span> <span data-ttu-id="2172f-239">Jste vytvořili jste jste dokončili kurz k Node.js hello a máte vaší první aplikace Azure Cosmos DB konzoly!</span><span class="sxs-lookup"><span data-stu-id="2172f-239">You've created you've completed hello Node.js tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="2172f-240"><a id="GetSolution"></a>Získat hello kompletní řešení kurzu k Node.js</span><span class="sxs-lookup"><span data-stu-id="2172f-240"><a id="GetSolution"></a>Get hello complete Node.js tutorial solution</span></span>
<span data-ttu-id="2172f-241">Pokud nebyly čas toocomplete hello kroky v tomto kurzu, nebo jenom toodownload hello kódu můžete získat z [Githubu](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="2172f-241">If you didn't have time toocomplete hello steps in this tutorial, or just want toodownload hello code, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span>

<span data-ttu-id="2172f-242">toorun hello řešení GetStarted, které obsahuje všechny ukázky hello v tomto článku, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="2172f-242">toorun hello GetStarted solution that contains all hello samples in this article, you will need hello following:</span></span>

* <span data-ttu-id="2172f-243">[Účet služby Azure Cosmos DB][create-account]</span><span class="sxs-lookup"><span data-stu-id="2172f-243">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="2172f-244">Hello [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) řešení, které jsou dostupné na Githubu.</span><span class="sxs-lookup"><span data-stu-id="2172f-244">hello [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="2172f-245">Nainstalujte hello **documentdb** pomocí npm modul.</span><span class="sxs-lookup"><span data-stu-id="2172f-245">Install hello **documentdb** module via npm.</span></span> <span data-ttu-id="2172f-246">Hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2172f-246">Use hello following command:</span></span>

* ```npm install documentdb --save```

<span data-ttu-id="2172f-247">Vedle hello ```config.js``` souboru, hodnoty config.endpoint a config.authKey hello aktualizace, jak je popsáno v [krok 3: nastavení konfigurací aplikace](#Config).</span><span class="sxs-lookup"><span data-stu-id="2172f-247">Next, in hello ```config.js``` file, update hello config.endpoint and config.authKey values as described in [Step 3: Set your app's configurations](#Config).</span></span> 

<span data-ttu-id="2172f-248">V terminálu vyhledejte vaše ```app.js``` souboru a spusťte příkaz hello: ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="2172f-248">Then in your terminal, locate your ```app.js``` file and run hello command: ```node app.js```.</span></span>

<span data-ttu-id="2172f-249">A to je vše, stačí sestavit a máte hotovo.</span><span class="sxs-lookup"><span data-stu-id="2172f-249">That's it, build it and you're on your way!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2172f-250">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2172f-250">Next steps</span></span>
* <span data-ttu-id="2172f-251">Hledáte složitější ukázku Node.js?</span><span class="sxs-lookup"><span data-stu-id="2172f-251">Want a more complex Node.js sample?</span></span> <span data-ttu-id="2172f-252">Viz [Sestavení webové aplikace Node.js využívající službu Azure Cosmos DB](documentdb-nodejs-application.md).</span><span class="sxs-lookup"><span data-stu-id="2172f-252">See [Build a Node.js web application using Azure Cosmos DB](documentdb-nodejs-application.md).</span></span>
* <span data-ttu-id="2172f-253">Zjistěte, jak příliš[monitorovat účet Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="2172f-253">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="2172f-254">Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="2172f-254">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="2172f-255">Další informace o programovacím modelu hello hello vývoj části hello [stránce dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="2172f-255">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
