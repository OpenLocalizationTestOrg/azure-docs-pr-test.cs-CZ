---
title: "Kurz Node.js k rozhraní DocumentDB API pro službu Azure Cosmos DB | Dokumentace Microsoftu"
description: "Kurz Node.js, v rámci kterého se vytvoří služba Cosmos DB pomocí rozhraní DocumentDB API."
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
ms.openlocfilehash: 6510e0270bb2efa252a2b2ad40014c5d26b74a81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="nodejs-tutorial-use-the-documentdb-api-in-azure-cosmos-db-to-create-a-nodejs-console-application"></a><span data-ttu-id="590a1-104">Kurz k Node.js: Vytvořte konzolovou aplikaci Node.js pomocí DocumentDB rozhraní API v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="590a1-104">Node.js tutorial: Use the DocumentDB API in Azure Cosmos DB to create a Node.js console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="590a1-105">.NET</span><span class="sxs-lookup"><span data-stu-id="590a1-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="590a1-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="590a1-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="590a1-107">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="590a1-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="590a1-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="590a1-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="590a1-109">Java</span><span class="sxs-lookup"><span data-stu-id="590a1-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="590a1-110">C++</span><span class="sxs-lookup"><span data-stu-id="590a1-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="590a1-111">Vítejte v kurzu Node.js pro sadu Azure Cosmos DB Node.js SDK!</span><span class="sxs-lookup"><span data-stu-id="590a1-111">Welcome to the Node.js tutorial for the Azure Cosmos DB Node.js SDK!</span></span> <span data-ttu-id="590a1-112">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="590a1-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="590a1-113">Budeme se zabývat těmito tématy:</span><span class="sxs-lookup"><span data-stu-id="590a1-113">We'll cover:</span></span>

* <span data-ttu-id="590a1-114">Vytvoření účtu služby Azure Cosmos DB a připojení k němu</span><span class="sxs-lookup"><span data-stu-id="590a1-114">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="590a1-115">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="590a1-115">Setting up your application</span></span>
* <span data-ttu-id="590a1-116">Vytvoření databáze Node</span><span class="sxs-lookup"><span data-stu-id="590a1-116">Creating a node database</span></span>
* <span data-ttu-id="590a1-117">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="590a1-117">Creating a collection</span></span>
* <span data-ttu-id="590a1-118">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="590a1-118">Creating JSON documents</span></span>
* <span data-ttu-id="590a1-119">Dotazování na kolekci</span><span class="sxs-lookup"><span data-stu-id="590a1-119">Querying the collection</span></span>
* <span data-ttu-id="590a1-120">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="590a1-120">Replacing a document</span></span>
* <span data-ttu-id="590a1-121">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="590a1-121">Deleting a document</span></span>
* <span data-ttu-id="590a1-122">Odstranění databáze Node</span><span class="sxs-lookup"><span data-stu-id="590a1-122">Deleting the node database</span></span>

<span data-ttu-id="590a1-123">Nemáte čas?</span><span class="sxs-lookup"><span data-stu-id="590a1-123">Don't have time?</span></span> <span data-ttu-id="590a1-124">Nevadí!</span><span class="sxs-lookup"><span data-stu-id="590a1-124">Don't worry!</span></span> <span data-ttu-id="590a1-125">Úplné řešení je k dispozici na [GitHubu](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="590a1-125">The complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span> <span data-ttu-id="590a1-126">Rychlé pokyny najdete v části [Získání úplného řešení](#GetSolution).</span><span class="sxs-lookup"><span data-stu-id="590a1-126">See [Get the complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="590a1-127">Až tento kurz Node.js dokončíte, řekněte nám prosím svůj názor pomocí hlasovacích tlačítek v horní a dolní části této stránky.</span><span class="sxs-lookup"><span data-stu-id="590a1-127">After you've completed the Node.js tutorial, please use the voting buttons at the top and bottom of this page to give us feedback.</span></span> <span data-ttu-id="590a1-128">Pokud chcete, abychom vás kontaktovali přímo, můžete nám nechat e-mailovou adresu v komentářích.</span><span class="sxs-lookup"><span data-stu-id="590a1-128">If you'd like us to contact you directly, feel free to include your email address in your comments.</span></span>

<span data-ttu-id="590a1-129">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="590a1-129">Now let's get started!</span></span>

## <a name="prerequisites-for-the-nodejs-tutorial"></a><span data-ttu-id="590a1-130">Předpoklady pro kurz k Node.js </span><span class="sxs-lookup"><span data-stu-id="590a1-130">Prerequisites for the Node.js tutorial</span></span>
<span data-ttu-id="590a1-131">Ujistěte se prosím, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="590a1-131">Please make sure you have the following:</span></span>

* <span data-ttu-id="590a1-132">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="590a1-132">An active Azure account.</span></span> <span data-ttu-id="590a1-133">Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="590a1-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
    * <span data-ttu-id="590a1-134">Alternativně můžete pro tento kurz použít [emulátor služby Azure Cosmos DB](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="590a1-134">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="590a1-135">[Node.js](https://nodejs.org/) verze 0.10.29 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="590a1-135">[Node.js](https://nodejs.org/) version v0.10.29 or higher.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="590a1-136">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="590a1-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="590a1-137">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="590a1-138">Pokud již máte účet, který chcete použít, můžete přeskočit na [nastavení aplikace Node.js](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="590a1-138">If you already have an account you want to use, you can skip ahead to [Set up your Node.js application](#SetupNode).</span></span> <span data-ttu-id="590a1-139">Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení aplikace Node.js](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="590a1-139">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Node.js application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="590a1-140"><a id="SetupNode"></a>Krok 2: Nastavení aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="590a1-140"><a id="SetupNode"></a>Step 2: Set up your Node.js application</span></span>
1. <span data-ttu-id="590a1-141">Otevřete svůj oblíbený terminál.</span><span class="sxs-lookup"><span data-stu-id="590a1-141">Open your favorite terminal.</span></span>
2. <span data-ttu-id="590a1-142">Vyhledejte složku nebo adresář, do kterého chcete uložit aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="590a1-142">Locate the folder or directory where you'd like to save your Node.js application.</span></span>
3. <span data-ttu-id="590a1-143">Vytvořte dva prázdné javascriptové soubory, které budou obsahovat následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="590a1-143">Create two empty JavaScript files with the following commands:</span></span>
   * <span data-ttu-id="590a1-144">Windows:</span><span class="sxs-lookup"><span data-stu-id="590a1-144">Windows:</span></span>
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * <span data-ttu-id="590a1-145">Linux nebo OS X:</span><span class="sxs-lookup"><span data-stu-id="590a1-145">Linux/OS X:</span></span>
     * ```touch app.js```
     * ```touch config.js```
4. <span data-ttu-id="590a1-146">Přes npm nainstalujte modul documentdb.</span><span class="sxs-lookup"><span data-stu-id="590a1-146">Install the documentdb module via npm.</span></span> <span data-ttu-id="590a1-147">Použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="590a1-147">Use the following command:</span></span>
   * ```npm install documentdb --save```

<span data-ttu-id="590a1-148">Výborně!</span><span class="sxs-lookup"><span data-stu-id="590a1-148">Great!</span></span> <span data-ttu-id="590a1-149">Teď když jste dokončili nastavování, napišme nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="590a1-149">Now that you've finished setting up, let's start writing some code.</span></span>

## <span data-ttu-id="590a1-150"><a id="Config"></a>Krok 3: Nastavení konfigurací aplikace</span><span class="sxs-lookup"><span data-stu-id="590a1-150"><a id="Config"></a>Step 3: Set your app's configurations</span></span>
<span data-ttu-id="590a1-151">Ve svém oblíbeném textovém editoru otevřete ```config.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-151">Open ```config.js``` in your favorite text editor.</span></span>

<span data-ttu-id="590a1-152">Potom, kopírování a vložte následující fragment kódu a nastavte vlastnosti ```config.endpoint``` a ```config.primaryKey``` pro koncový bod Azure Cosmos DB identifikátor uri a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="590a1-152">Then, copy and paste the code snippet below and set properties ```config.endpoint``` and ```config.primaryKey``` to your Azure Cosmos DB endpoint uri and primary key.</span></span> <span data-ttu-id="590a1-153">Obě tyto konfigurace lze nalézt v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="590a1-153">Both these configurations can be found in the [Azure portal](https://portal.azure.com).</span></span>

![Kurz k Node.js – snímek obrazovky portálu Azure znázorňující účet Azure Cosmos DB s AKTIVNÍM centrem zvýrazní, tlačítkem klíče v okně účtu Azure Cosmos DB a hodnotami URI, primární klíč a sekundární klíč v okně klíče – Databáze node][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

<span data-ttu-id="590a1-155">Zkopírujte ```database id```, ```collection id``` a ```JSON documents``` a vložte je do objektu ```config``` níže, kde nastavíte vlastnosti ```config.endpoint``` a ```config.authKey```.</span><span class="sxs-lookup"><span data-stu-id="590a1-155">Copy and paste the ```database id```, ```collection id```, and ```JSON documents``` to your ```config``` object below where you set your ```config.endpoint``` and ```config.authKey``` properties.</span></span> <span data-ttu-id="590a1-156">Pokud již máte data, která chcete uložit do databáze, můžete místo přidávání definic dokumentů použít [nástroj pro migraci dat](import-data.md) služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-156">If you already have data you'd like to store in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) rather than adding the document definitions.</span></span>

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
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


<span data-ttu-id="590a1-157">Databáze, kolekce a definice dokumentů bude fungovat jako vaše Azure DB Cosmos ```database id```, ```collection id```a data dokumentů.</span><span class="sxs-lookup"><span data-stu-id="590a1-157">The database, collection, and document definitions will act as your Azure Cosmos DB ```database id```, ```collection id```, and documents' data.</span></span>

<span data-ttu-id="590a1-158">Nakonec exportujte objekt ```config```, abyste na něj mohli odkazovat ze souboru ```app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-158">Finally, export your ```config``` object, so that you can reference it within the ```app.js``` file.</span></span>

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

## <span data-ttu-id="590a1-159"><a id="Connect"></a>Krok 4: Připojení k účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="590a1-159"><a id="Connect"></a> Step 4: Connect to an Azure Cosmos DB account</span></span>
<span data-ttu-id="590a1-160">V textovém editoru otevřete prázdný soubor ```app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-160">Open your empty ```app.js``` file in the text editor.</span></span> <span data-ttu-id="590a1-161">Zkopírováním a vložením kódu níže importujte modul ```documentdb``` a nově vytvořený modul ```config```.</span><span class="sxs-lookup"><span data-stu-id="590a1-161">Copy and paste the code below to import the ```documentdb``` module and your newly created ```config``` module.</span></span>

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

<span data-ttu-id="590a1-162">Zkopírujte a vložte kód, který použije dříve uložené ```config.endpoint``` a ```config.primaryKey``` k vytvoření nové instance DocumentClient.</span><span class="sxs-lookup"><span data-stu-id="590a1-162">Copy and paste the code to use the previously saved ```config.endpoint``` and ```config.primaryKey``` to create a new DocumentClient.</span></span>

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

<span data-ttu-id="590a1-163">Teď, když máte kód pro inicializaci klienta Azure Cosmos DB, Podívejme se na práci s prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-163">Now that you have the code to initialize the Azure Cosmos DB client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <a name="step-5-create-a-node-database"></a><span data-ttu-id="590a1-164">Krok 5: Vytvoření databáze Node</span><span class="sxs-lookup"><span data-stu-id="590a1-164">Step 5: Create a Node database</span></span>
<span data-ttu-id="590a1-165">Zkopírujte a vložte kód níže, který nastaví stav HTTP pro odpověď Not Found (Nenalezeno), URL databáze a URL kolekce.</span><span class="sxs-lookup"><span data-stu-id="590a1-165">Copy and paste the code below to set the HTTP status for Not Found, the database url, and the collection url.</span></span> <span data-ttu-id="590a1-166">Tyto adresy URL se, jak bude klient Azure Cosmos DB najít správné databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="590a1-166">These urls are how the Azure Cosmos DB client will find the right database and collection.</span></span>

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

<span data-ttu-id="590a1-167">[Databázi](documentdb-resources.md#databases) je možné vytvořit pomocí funkce [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="590a1-167">A [database](documentdb-resources.md#databases) can be created by using the [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="590a1-168">Databáze je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi.</span><span class="sxs-lookup"><span data-stu-id="590a1-168">A database is the logical container of document storage partitioned across collections.</span></span>

<span data-ttu-id="590a1-169">Pro vytvoření nové databáze v souboru app.js s ```id``` zadaným v objektu ```config``` zkopírujte a vložte funkci **getDatabase**.</span><span class="sxs-lookup"><span data-stu-id="590a1-169">Copy and paste the **getDatabase** function for creating your new database in the app.js file with the ```id``` specified in the ```config``` object.</span></span> <span data-ttu-id="590a1-170">Funkce zkontroluje, jestli již neexistuje databáze se stejným ID ```FamilyRegistry```.</span><span class="sxs-lookup"><span data-stu-id="590a1-170">The function will check if the database with the same ```FamilyRegistry``` id does not already exist.</span></span> <span data-ttu-id="590a1-171">Pokud ano, místo vytvoření nové databáze se vrátí databáze s tímto ID.</span><span class="sxs-lookup"><span data-stu-id="590a1-171">If it does exist, we'll return that database instead of creating a new one.</span></span>

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="590a1-172">Zkopírujte a vložte kód níže, kde za funkci **getDatabase** přidáte pomocnou funkci **exit**. Ta vypíše závěrečnou zprávu a zavolá funkci **getDatabase**.</span><span class="sxs-lookup"><span data-stu-id="590a1-172">Copy and paste the code below where you set the **getDatabase** function to add the helper function **exit** that will print the exit message and the call to **getDatabase** function.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="590a1-173">V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-173">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="590a1-174">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="590a1-174">Congratulations!</span></span> <span data-ttu-id="590a1-175">Úspěšně jste vytvořili databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-175">You have successfully created an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="590a1-176"><a id="CreateColl"></a>Krok 6: Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="590a1-176"><a id="CreateColl"></a>Step 6: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="590a1-177">**CreateDocumentCollectionAsync** vytvoří novou kolekci, za kterou se hradí poplatky.</span><span class="sxs-lookup"><span data-stu-id="590a1-177">**CreateDocumentCollectionAsync** will create a new collection, which has pricing implications.</span></span> <span data-ttu-id="590a1-178">Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="590a1-178">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="590a1-179">[Kolekci](documentdb-resources.md#collections) je možné vytvořit pomocí funkce [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="590a1-179">A [collection](documentdb-resources.md#collections) can be created by using the [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="590a1-180">Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="590a1-180">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="590a1-181">Pro vytvoření nové kolekce s ```id``` zadaným v objektu ```config``` zkopírujte a vložte funkci **getCollection** pod funkci **getDatabase** v souboru app.js.</span><span class="sxs-lookup"><span data-stu-id="590a1-181">Copy and paste the **getCollection** function underneath the **getDatabase** function in the app.js file to create your new collection with the ```id``` specified in the ```config``` object.</span></span> <span data-ttu-id="590a1-182">Opět zkontrolujeme, že kolekce se stejným ID ```FamilyCollection``` ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="590a1-182">Again, we'll check to make sure a collection with the same ```FamilyCollection``` id does not already exist.</span></span> <span data-ttu-id="590a1-183">Pokud existuje, místo vytvoření nové kolekce se vrátí kolekce s tímto ID.</span><span class="sxs-lookup"><span data-stu-id="590a1-183">If it does exist, we'll return that collection instead of creating a new one.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="590a1-184">Zkopírujte a vložte kód pod voláním funkce **getDatabase**, aby se spustila funkce **getCollection**.</span><span class="sxs-lookup"><span data-stu-id="590a1-184">Copy and paste the code below the call to **getDatabase** to execute the **getCollection** function.</span></span>

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="590a1-185">V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-185">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="590a1-186">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="590a1-186">Congratulations!</span></span> <span data-ttu-id="590a1-187">Úspěšně jste vytvořili kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-187">You have successfully created an Azure Cosmos DB collection.</span></span>

## <span data-ttu-id="590a1-188"><a id="CreateDoc"></a>Krok 7: Vytvoření dokumentu</span><span class="sxs-lookup"><span data-stu-id="590a1-188"><a id="CreateDoc"></a>Step 7: Create a document</span></span>
<span data-ttu-id="590a1-189">[Dokument](documentdb-resources.md#documents) je možné vytvořit pomocí metody [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="590a1-189">A [document](documentdb-resources.md#documents) can be created by using the [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="590a1-190">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="590a1-190">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="590a1-191">Nyní můžete do služby Azure Cosmos DB vložit dokument.</span><span class="sxs-lookup"><span data-stu-id="590a1-191">You can now insert a document into Azure Cosmos DB.</span></span>

<span data-ttu-id="590a1-192">Pod funkci **getCollection** zkopírujte a vložte funkci **getFamilyDocument**, která bude vytvářet dokumenty obsahující data JSON uložené v objektu ```config```.</span><span class="sxs-lookup"><span data-stu-id="590a1-192">Copy and paste the **getFamilyDocument** function underneath the **getCollection** function for creating the documents containing the JSON data saved in the ```config``` object.</span></span> <span data-ttu-id="590a1-193">Opět zkontrolujeme, že dokument se stejným ID ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="590a1-193">Again, we'll check to make sure a document with the same id does not already exist.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="590a1-194">Zkopírujte a vložte kód pod voláním funkce **getCollection**, aby se spustila funkce **getFamilyDocument**.</span><span class="sxs-lookup"><span data-stu-id="590a1-194">Copy and paste the code below the call to **getCollection** to execute the **getFamilyDocument** function.</span></span>

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="590a1-195">V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-195">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="590a1-196">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="590a1-196">Congratulations!</span></span> <span data-ttu-id="590a1-197">Úspěšně jste vytvořili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-197">You have successfully created an Azure Cosmos DB document.</span></span>

![Kurz k Node.js – diagram ilustrující hierarchický vztah mezi účtem, databází, kolekcí a dokumenty – databáze Node](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <span data-ttu-id="590a1-199"><a id="Query"></a>Krok 8: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="590a1-199"><a id="Query"></a>Step 8: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="590a1-200">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="590a1-200">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="590a1-201">Následující ukázka kódu obsahuje dotaz, který je možné spustit proti dokumentům v kolekci.</span><span class="sxs-lookup"><span data-stu-id="590a1-201">The following sample code shows a query that you can run against the documents in your collection.</span></span>

<span data-ttu-id="590a1-202">Zkopírujte funkci **queryCollection** a vložte ji pod funkci **getFamilyDocument** v souboru app.js.</span><span class="sxs-lookup"><span data-stu-id="590a1-202">Copy and paste the **queryCollection** function underneath the **getFamilyDocument** function in the app.js file.</span></span> <span data-ttu-id="590a1-203">Azure Cosmos DB podporuje dotazy podobné jazyku SQL, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="590a1-203">Azure Cosmos DB supports SQL-like queries as shown below.</span></span> <span data-ttu-id="590a1-204">Další informace o vytváření komplexních dotazů najdete v [Query Playground](https://www.documentdb.com/sql/demo) a [dokumentaci k dotazům](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="590a1-204">For more information on building complex queries, check out the [Query Playground](https://www.documentdb.com/sql/demo) and the [query documentation](documentdb-sql-query.md).</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
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


<span data-ttu-id="590a1-205">Následující diagram znázorňuje, jak Azure Cosmos DB SQL se volá syntaxe dotazu proti kolekci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="590a1-205">The following diagram illustrates how the Azure Cosmos DB SQL query syntax is called against the collection you created.</span></span>

![Kurz k Node.js – diagram znázorňující obor a význam dotazu – databáze Node](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

<span data-ttu-id="590a1-207">[FROM](documentdb-sql-query.md#FromClause) – klíčové slovo je v dotazu volitelné, protože Azure Cosmos DB dotazy již mají obor nastaven na jedinou kolekci.</span><span class="sxs-lookup"><span data-stu-id="590a1-207">The [FROM](documentdb-sql-query.md#FromClause) keyword is optional in the query because Azure Cosmos DB queries are already scoped to a single collection.</span></span> <span data-ttu-id="590a1-208">Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte.</span><span class="sxs-lookup"><span data-stu-id="590a1-208">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="590a1-209">Azure Cosmos DB bude odvození že Families, root nebo název proměnné, které jste zvolili, odkazují na aktuální kolekci ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="590a1-209">Azure Cosmos DB will infer that Families, root, or the variable name you chose, reference the current collection by default.</span></span>

<span data-ttu-id="590a1-210">Zkopírujte a vložte kód pod voláním funkce **getFamilyDocument**, aby se spustila funkce **queryCollection**.</span><span class="sxs-lookup"><span data-stu-id="590a1-210">Copy and paste the code below the call to **getFamilyDocument** to execute the **queryCollection** function.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="590a1-211">V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-211">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="590a1-212">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="590a1-212">Congratulations!</span></span> <span data-ttu-id="590a1-213">Úspěšně jste provedli dotaz na dokumenty Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-213">You have successfully queried Azure Cosmos DB documents.</span></span>

## <span data-ttu-id="590a1-214"><a id="ReplaceDocument"></a>Krok 9: Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="590a1-214"><a id="ReplaceDocument"></a>Step 9: Replace a document</span></span>
<span data-ttu-id="590a1-215">Azure Cosmos DB podporuje nahrazování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="590a1-215">Azure Cosmos DB supports replacing JSON documents.</span></span>

<span data-ttu-id="590a1-216">Zkopírujte funkci **replaceFamilyDocument** a vložte ji pod funkci **queryCollection** v souboru app.js.</span><span class="sxs-lookup"><span data-stu-id="590a1-216">Copy and paste the **replaceFamilyDocument** function underneath the **queryCollection** function in the app.js file.</span></span>

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="590a1-217">Zkopírujte a vložte kód pod voláním funkce **queryCollection**, aby se spustila funkce **replaceDocument**.</span><span class="sxs-lookup"><span data-stu-id="590a1-217">Copy and paste the code below the call to **queryCollection** to execute the **replaceDocument** function.</span></span> <span data-ttu-id="590a1-218">Také znovu přidejte kód volání **queryCollection**, aby bylo možné ověřit, že se dokument úspěšně změnil.</span><span class="sxs-lookup"><span data-stu-id="590a1-218">Also, add the code to call **queryCollection** again to verify that the document had successfully changed.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="590a1-219">V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-219">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="590a1-220">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="590a1-220">Congratulations!</span></span> <span data-ttu-id="590a1-221">Úspěšně jste nahradili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-221">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="590a1-222"><a id="DeleteDocument"></a>Krok 10: Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="590a1-222"><a id="DeleteDocument"></a>Step 10: Delete a document</span></span>
<span data-ttu-id="590a1-223">Azure Cosmos DB podporuje odstraňování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="590a1-223">Azure Cosmos DB supports deleting JSON documents.</span></span>

<span data-ttu-id="590a1-224">Zkopírujte funkci **deleteFamilyDocument** a vložte ji pod funkci **replaceFamilyDocument**.</span><span class="sxs-lookup"><span data-stu-id="590a1-224">Copy and paste the **deleteFamilyDocument** function underneath the **replaceFamilyDocument** function.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="590a1-225">Zkopírujte a vložte kód pod druhým voláním funkce **queryCollection**, aby se spustila funkce **deleteDocument**.</span><span class="sxs-lookup"><span data-stu-id="590a1-225">Copy and paste the code below the call to the second **queryCollection** to execute the **deleteDocument** function.</span></span>

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="590a1-226">V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-226">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="590a1-227">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="590a1-227">Congratulations!</span></span> <span data-ttu-id="590a1-228">Úspěšně jste odstranili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="590a1-228">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="590a1-229"><a id="DeleteDatabase"></a>Krok 11: Odstranění databáze Node</span><span class="sxs-lookup"><span data-stu-id="590a1-229"><a id="DeleteDatabase"></a>Step 11: Delete the Node database</span></span>
<span data-ttu-id="590a1-230">Odstraněním vytvořené databáze dojde k odstranění databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="590a1-230">Deleting the created database will remove the database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="590a1-231">Zkopírujte a pod funkci **deleteFamilyDocument** vložte funkci **cleanup**, která odebere databázi a všechny podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="590a1-231">Copy and paste the **cleanup** function underneath the **deleteFamilyDocument** function to remove the database and all the children resources.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

<span data-ttu-id="590a1-232">Zkopírujte a vložte kód pod volání funkce **deleteFamilyDocument**, aby se spustila funkce **cleanup**.</span><span class="sxs-lookup"><span data-stu-id="590a1-232">Copy and paste the code below the call to **deleteFamilyDocument** to execute the **cleanup** function.</span></span>

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <span data-ttu-id="590a1-233"><a id="Run"></a>Krok 12: Spuštění celé aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="590a1-233"><a id="Run"></a>Step 12: Run your Node.js application all together!</span></span>
<span data-ttu-id="590a1-234">Jako celek by pořadí volání funkcí mělo vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="590a1-234">Altogether, the sequence for calling your functions should look like this:</span></span>

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

<span data-ttu-id="590a1-235">V terminálu vyhledejte soubor ```app.js``` a spusťte příkaz ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-235">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="590a1-236">Měl by se zobrazit výstup počáteční aplikace.</span><span class="sxs-lookup"><span data-stu-id="590a1-236">You should see the output of your get started app.</span></span> <span data-ttu-id="590a1-237">Výstup by měl odpovídat ukázkovému textu níže.</span><span class="sxs-lookup"><span data-stu-id="590a1-237">The output should match the example text below.</span></span>

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
    Press any key to exit

<span data-ttu-id="590a1-238">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="590a1-238">Congratulations!</span></span> <span data-ttu-id="590a1-239">Dokončili jste kurz Node.js a máte svou první konzolovou aplikaci využívající službu Azure Cosmos DB!</span><span class="sxs-lookup"><span data-stu-id="590a1-239">You've created you've completed the Node.js tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="590a1-240"><a id="GetSolution"></a>Získání úplného řešení kurzu k Node.js</span><span class="sxs-lookup"><span data-stu-id="590a1-240"><a id="GetSolution"></a>Get the complete Node.js tutorial solution</span></span>
<span data-ttu-id="590a1-241">Pokud jste neměli dostatek času k dokončení kroků v tomto kurzu nebo si jenom chcete stáhnout kód, můžete ho získat z [GitHubu](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="590a1-241">If you didn't have time to complete the steps in this tutorial, or just want to download the code, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span>

<span data-ttu-id="590a1-242">Abyste mohli spustit řešení GetStarted, které obsahuje všechny ukázky tohoto článku, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="590a1-242">To run the GetStarted solution that contains all the samples in this article, you will need the following:</span></span>

* <span data-ttu-id="590a1-243">[Účet služby Azure Cosmos DB][create-account]</span><span class="sxs-lookup"><span data-stu-id="590a1-243">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="590a1-244">Řešení [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) dostupné na GitHubu</span><span class="sxs-lookup"><span data-stu-id="590a1-244">The [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="590a1-245">Přes npm nainstalujte modul **documentdb**.</span><span class="sxs-lookup"><span data-stu-id="590a1-245">Install the **documentdb** module via npm.</span></span> <span data-ttu-id="590a1-246">Použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="590a1-246">Use the following command:</span></span>

* ```npm install documentdb --save```

<span data-ttu-id="590a1-247">Dále v souboru ```config.js``` aktualizujte hodnoty config.endpoint a config.authKey, jak je popsáno v části [Krok 3: Nastavení konfigurací aplikace](#Config).</span><span class="sxs-lookup"><span data-stu-id="590a1-247">Next, in the ```config.js``` file, update the config.endpoint and config.authKey values as described in [Step 3: Set your app's configurations](#Config).</span></span> 

<span data-ttu-id="590a1-248">Potom v terminálu vyhledejte soubor ```app.js``` a spusťte příkaz: ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="590a1-248">Then in your terminal, locate your ```app.js``` file and run the command: ```node app.js```.</span></span>

<span data-ttu-id="590a1-249">A to je vše, stačí sestavit a máte hotovo.</span><span class="sxs-lookup"><span data-stu-id="590a1-249">That's it, build it and you're on your way!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="590a1-250">Další kroky</span><span class="sxs-lookup"><span data-stu-id="590a1-250">Next steps</span></span>
* <span data-ttu-id="590a1-251">Hledáte složitější ukázku Node.js?</span><span class="sxs-lookup"><span data-stu-id="590a1-251">Want a more complex Node.js sample?</span></span> <span data-ttu-id="590a1-252">Viz [Sestavení webové aplikace Node.js využívající službu Azure Cosmos DB](documentdb-nodejs-application.md).</span><span class="sxs-lookup"><span data-stu-id="590a1-252">See [Build a Node.js web application using Azure Cosmos DB](documentdb-nodejs-application.md).</span></span>
* <span data-ttu-id="590a1-253">Zjistěte, jak [monitorovat účet služby Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="590a1-253">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="590a1-254">Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="590a1-254">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="590a1-255">Přečtěte si více o tomto programovacím modelu v části Vyvíjejte na [stránce dokumentace ke službě Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="590a1-255">Learn more about the programming model in the Develop section of the [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
