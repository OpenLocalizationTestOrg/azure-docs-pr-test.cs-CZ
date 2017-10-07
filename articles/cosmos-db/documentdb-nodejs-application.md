---
title: "aaaBuild webové aplikace Node.js pro Azure Cosmos DB | Microsoft Docs"
description: "Tento kurz Node.js popisuje, jak toouse data toostore a přístup k databázi Cosmos Microsoft Azure z webové aplikace Node.js Express hostované na webech Azure."
keywords: "Vývoj aplikací, databázový kurz výuka node.js, kurz k node.js"
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 9da9e63b-e76a-434e-96dd-195ce2699ef3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: mimig
ms.openlocfilehash: 31194dccf37eef69d2219b0d8328a88d434f79b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="32396-104"><a name="_Toc395783175"></a>Sestavení webové aplikace Node.js využívající službu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="32396-104"><a name="_Toc395783175"></a>Build a Node.js web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="32396-105">.NET</span><span class="sxs-lookup"><span data-stu-id="32396-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="32396-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="32396-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="32396-107">Java</span><span class="sxs-lookup"><span data-stu-id="32396-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="32396-108">Python</span><span class="sxs-lookup"><span data-stu-id="32396-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="32396-109">Tento kurz Node.js popisuje, jak toouse Azure Cosmos DB a hello DocumentDB API toostore a přístupová data z aplikace Node.js Express hostované na webech Azure.</span><span class="sxs-lookup"><span data-stu-id="32396-109">This Node.js tutorial shows you how toouse Azure Cosmos DB and hello DocumentDB API toostore and access data from a Node.js Express application hosted on Azure Websites.</span></span> <span data-ttu-id="32396-110">Vytvoříte jednoduchou webovou aplikaci pro správu úkolů, aplikaci seznamu úkolů, která umožní vytvářet, získávat a dokončovat úkoly.</span><span class="sxs-lookup"><span data-stu-id="32396-110">You build a simple web-based task-management application, a ToDo app, that allows creating, retrieving, and completing tasks.</span></span> <span data-ttu-id="32396-111">Hello úlohy jsou uloženy jako dokumenty JSON v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="32396-111">hello tasks are stored as JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="32396-112">Tento kurz vás provede procesem vytvoření hello a nasazení aplikace hello a vysvětluje, co se děje v každé fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="32396-112">This tutorial walks you through hello creation and deployment of hello app and explains what's happening in each snippet.</span></span>

![Snímek obrazovky aplikace seznam úkolů vytvořené v tomto kurzu Node.js hello](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

<span data-ttu-id="32396-114">Nemáte čas toocomplete hello kurzu a právě má tooget hello kompletního řešení?</span><span class="sxs-lookup"><span data-stu-id="32396-114">Don't have time toocomplete hello tutorial and just want tooget hello complete solution?</span></span> <span data-ttu-id="32396-115">Nejedná se o problém, můžete získat hello celé ukázkové řešení z [Githubu][GitHub].</span><span class="sxs-lookup"><span data-stu-id="32396-115">Not a problem, you can get hello complete sample solution from [GitHub][GitHub].</span></span> <span data-ttu-id="32396-116">Jen pro čtení hello [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) souboru pokyny, jak toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="32396-116">Just read hello [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) file for instructions on how toorun hello app.</span></span>

## <span data-ttu-id="32396-117"><a name="_Toc395783176"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="32396-117"><a name="_Toc395783176"></a>Prerequisites</span></span>
> [!TIP]
> <span data-ttu-id="32396-118">V tomto kurzu Node.js se předpokládá, že již máte zkušenosti s používáním Node.js a Webů Azure.</span><span class="sxs-lookup"><span data-stu-id="32396-118">This Node.js tutorial assumes that you have some prior experience using Node.js and Azure Websites.</span></span>
> 
> 

<span data-ttu-id="32396-119">Před následující hello pokyny v tomto článku, se ujistěte, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="32396-119">Before following hello instructions in this article, you should ensure that you have hello following:</span></span>

* <span data-ttu-id="32396-120">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="32396-120">An active Azure account.</span></span> <span data-ttu-id="32396-121">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="32396-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="32396-122">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="32396-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

   <span data-ttu-id="32396-123">NEBO</span><span class="sxs-lookup"><span data-stu-id="32396-123">OR</span></span>

   <span data-ttu-id="32396-124">Místní instalace hello [emulátoru DB Cosmos Azure](local-emulator.md) (jenom Windows).</span><span class="sxs-lookup"><span data-stu-id="32396-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md) (Windows only).</span></span>
* <span data-ttu-id="32396-125">[Node.js][Node.js] verze 0.10.29 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="32396-125">[Node.js][Node.js] version v0.10.29 or higher.</span></span>
* <span data-ttu-id="32396-126">[Generátor Express](http://www.expressjs.com/starter/generator.html) (lze nainstalovat prostřednictvím `npm install express-generator -g`)</span><span class="sxs-lookup"><span data-stu-id="32396-126">[Express generator](http://www.expressjs.com/starter/generator.html) (you can install this via `npm install express-generator -g`)</span></span>
* <span data-ttu-id="32396-127">[Git][Git]</span><span class="sxs-lookup"><span data-stu-id="32396-127">[Git][Git].</span></span>

## <span data-ttu-id="32396-128"><a name="_Toc395637761"></a>Krok 1: Vytvoření účtu databáze Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="32396-128"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="32396-129">Začněme vytvořením účtu služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="32396-129">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="32396-130">Pokud již máte účet, nebo pokud používáte hello emulátoru DB Cosmos Azure pro účely tohoto kurzu, můžete přeskočit příliš[krok 2: vytvoření nové aplikace Node.js](#_Toc395783178).</span><span class="sxs-lookup"><span data-stu-id="32396-130">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create a new Node.js application](#_Toc395783178).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="32396-131"><a name="_Toc395783178"></a>Krok 2: Vytvoření nové aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="32396-131"><a name="_Toc395783178"></a>Step 2: Create a new Node.js application</span></span>
<span data-ttu-id="32396-132">Nyní naučíme toocreate na základní projekt Hello World Node.js pomocí hello [Express](http://expressjs.com/) framework.</span><span class="sxs-lookup"><span data-stu-id="32396-132">Now let's learn toocreate a basic Hello World Node.js project using hello [Express](http://expressjs.com/) framework.</span></span>

1. <span data-ttu-id="32396-133">Otevřete svůj oblíbený terminál, jako je například příkazového řádku Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="32396-133">Open your favorite terminal, such as hello Node.js command prompt.</span></span>
2. <span data-ttu-id="32396-134">Přejděte toohello adresář, ve kterém chcete toostore hello novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="32396-134">Navigate toohello directory in which you'd like toostore hello new application.</span></span>
3. <span data-ttu-id="32396-135">Použít toogenerate generátor express hello nové aplikace volá **todo**.</span><span class="sxs-lookup"><span data-stu-id="32396-135">Use hello express generator toogenerate a new application called **todo**.</span></span>
   
        express todo
4. <span data-ttu-id="32396-136">Otevřete nový adresář **todo** a nainstalujte závislosti.</span><span class="sxs-lookup"><span data-stu-id="32396-136">Open your new **todo** directory and install dependencies.</span></span>
   
        cd todo
        npm install
5. <span data-ttu-id="32396-137">Spusťte novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="32396-137">Run your new application.</span></span>
   
        npm start
6. <span data-ttu-id="32396-138">Můžete zobrazit novou aplikaci, přejděte v prohlížeči příliš[http://localhost: 3000](http://localhost:3000).</span><span class="sxs-lookup"><span data-stu-id="32396-138">You can view your new application by navigating your browser too[http://localhost:3000](http://localhost:3000).</span></span>
   
    ![Výuka Node.js – snímek obrazovky aplikace Hello, World v okně prohlížeče hello](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    <span data-ttu-id="32396-140">Potom toostop hello aplikace, stiskněte kombinaci kláves CTRL + C v hello okno terminálu a pak klikněte na **y** tooterminate hello dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="32396-140">Then, toostop hello application, press CTRL+C in hello terminal window and then click **y** tooterminate hello batch job.</span></span>

## <span data-ttu-id="32396-141"><a name="_Toc395783179"></a>Krok 3: Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="32396-141"><a name="_Toc395783179"></a>Step 3: Install additional modules</span></span>
<span data-ttu-id="32396-142">Hello **package.json** souboru je jedním z hello soubory vytvořené v kořenovém hello hello projektu.</span><span class="sxs-lookup"><span data-stu-id="32396-142">hello **package.json** file is one of hello files created in hello root of hello project.</span></span> <span data-ttu-id="32396-143">Tento soubor obsahuje seznam dalších modulů, které aplikace Node.js vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="32396-143">This file contains a list of additional modules that are required for your Node.js application.</span></span> <span data-ttu-id="32396-144">Později při nasazení této aplikace tooAzure weby, tento soubor je použité toodetermine toobe které moduly potřeba nainstalovat na Azure toosupport vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="32396-144">Later, when you deploy this application tooAzure Websites, this file is used toodetermine which modules need toobe installed on Azure toosupport your application.</span></span> <span data-ttu-id="32396-145">Stále potřebujeme tooinstall dva další balíčky pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="32396-145">We still need tooinstall two more packages for this tutorial.</span></span>

1. <span data-ttu-id="32396-146">Zpět v hello terminálu, nainstalujte hello **asynchronní** pomocí npm modul.</span><span class="sxs-lookup"><span data-stu-id="32396-146">Back in hello terminal, install hello **async** module via npm.</span></span>
   
        npm install async --save
2. <span data-ttu-id="32396-147">Nainstalujte hello **documentdb** pomocí npm modul.</span><span class="sxs-lookup"><span data-stu-id="32396-147">Install hello **documentdb** module via npm.</span></span> <span data-ttu-id="32396-148">Toto je hello modulu, kde se stane všechny magic hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="32396-148">This is hello module where all hello Azure Cosmos DB magic happens.</span></span>
   
        npm install documentdb --save
3. <span data-ttu-id="32396-149">Rychlou kontrolu hello **package.json** souboru hello aplikace by měl zobrazit hello dalších modulů.</span><span class="sxs-lookup"><span data-stu-id="32396-149">A quick check of hello **package.json** file of hello application should show hello additional modules.</span></span> <span data-ttu-id="32396-150">Tento soubor bude informuje Azure, které balíčky toodownload a nainstalovat při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="32396-150">This file will tell Azure which packages toodownload and install when running your application.</span></span> <span data-ttu-id="32396-151">Měl by vypadat hello příklad.</span><span class="sxs-lookup"><span data-stu-id="32396-151">It should resemble hello example below.</span></span>
   
        {
          "name": "todo",
          "version": "0.0.0",
          "private": true,
          "scripts": {
            "start": "node ./bin/www"
          },
          "dependencies": {
            "async": "^2.1.4",
            "body-parser": "~1.15.2",
            "cookie-parser": "~1.4.3",
            "debug": "~2.2.0",
            "documentdb": "^1.10.0",
            "express": "~4.14.0",
            "jade": "~1.11.0",
            "morgan": "~1.7.0",
            "serve-favicon": "~2.3.0"
          }
        }
   
    <span data-ttu-id="32396-152">To říká uzlu (a později Azure), že vaše aplikace závisí na těchto dalších modulech.</span><span class="sxs-lookup"><span data-stu-id="32396-152">This tells Node (and Azure later) that your application depends on these additional modules.</span></span>

## <span data-ttu-id="32396-153"><a name="_Toc395783180"></a>Krok 4: Využití služby Azure Cosmos DB hello v aplikaci node</span><span class="sxs-lookup"><span data-stu-id="32396-153"><a name="_Toc395783180"></a>Step 4: Using hello Azure Cosmos DB service in a node application</span></span>
<span data-ttu-id="32396-154">Který se stará o všechny hello počáteční instalace a konfigurace – nyní Pojďme get dolů toowhy jsme tu, na kterém jsou toowrite některé programování s využitím Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="32396-154">That takes care of all hello initial setup and configuration, now let’s get down toowhy we’re here, and that’s toowrite some code using Azure Cosmos DB.</span></span>

### <a name="create-hello-model"></a><span data-ttu-id="32396-155">Vytvoření modelu hello</span><span class="sxs-lookup"><span data-stu-id="32396-155">Create hello model</span></span>
1. <span data-ttu-id="32396-156">V adresáři projektu hello, vytvořte nový adresář s názvem **modely** v hello stejného adresáře jako soubor package.json hello.</span><span class="sxs-lookup"><span data-stu-id="32396-156">In hello project directory, create a new directory named **models** in hello same directory as hello package.json file.</span></span>
2. <span data-ttu-id="32396-157">V hello **modely** adresáře, vytvořte nový soubor s názvem **taskDao.js**.</span><span class="sxs-lookup"><span data-stu-id="32396-157">In hello **models** directory, create a new file named **taskDao.js**.</span></span> <span data-ttu-id="32396-158">Tento soubor bude obsahovat hello model pro úkoly hello vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="32396-158">This file will contain hello model for hello tasks created by our application.</span></span>
3. <span data-ttu-id="32396-159">V hello stejné **modely** adresáře, vytvořte další nový soubor s názvem **docdbUtils.js**.</span><span class="sxs-lookup"><span data-stu-id="32396-159">In hello same **models** directory, create another new file named **docdbUtils.js**.</span></span> <span data-ttu-id="32396-160">V tomto souboru bude obsažen užitečný, opakovaně použitelný kód, který budeme používat v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="32396-160">This file will contain some useful, reusable, code that we will use throughout our application.</span></span> 
4. <span data-ttu-id="32396-161">Kopírování hello následující kód v příliš**docdbUtils.js**</span><span class="sxs-lookup"><span data-stu-id="32396-161">Copy hello following code in too**docdbUtils.js**</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
   
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
   
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
   
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
   
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };               
   
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
   
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
   
        module.exports = DocDBUtils;
   
5. <span data-ttu-id="32396-162">Uložte a zavřete hello **docdbUtils.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="32396-162">Save and close hello **docdbUtils.js** file.</span></span>
6. <span data-ttu-id="32396-163">Na začátku hello hello **taskDao.js** soubor, přidejte následující kód tooreference hello hello **DocumentDBClient** a hello **docdbUtils.js** jsme vytvořili výše:</span><span class="sxs-lookup"><span data-stu-id="32396-163">At hello beginning of hello **taskDao.js** file, add hello following code tooreference hello **DocumentDBClient** and hello **docdbUtils.js** we created above:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. <span data-ttu-id="32396-164">Dále přidáte kód toodefine a export objektu Task hello.</span><span class="sxs-lookup"><span data-stu-id="32396-164">Next, you will add code toodefine and export hello Task object.</span></span> <span data-ttu-id="32396-165">Toto je zodpovědná za inicializaci objektu Task a nastavení hello databáze a kolekce dokumentů budeme používat.</span><span class="sxs-lookup"><span data-stu-id="32396-165">This is responsible for initializing our Task object and setting up hello Database and Document Collection we will use.</span></span>
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. <span data-ttu-id="32396-166">Dále přidejte následující kód toodefine další metody na objektu Task hello hello ty umožňují pracovat s daty uloženými v databázi Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="32396-166">Next, add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in Azure Cosmos DB.</span></span>
   
        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
   
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
   
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
   
            find: function (querySpec, callback) {
                var self = this;
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results);
                    }
                });
            },
   
            addItem: function (item, callback) {
                var self = this;
   
                item.date = Date.now();
                item.completed = false;
   
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, doc);
                    }
                });
            },
   
            updateItem: function (itemId, callback) {
                var self = this;
   
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        doc.completed = true;
   
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
   
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
   
            getItem: function (itemId, callback) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };
9. <span data-ttu-id="32396-167">Uložte a zavřete hello **taskDao.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="32396-167">Save and close hello **taskDao.js** file.</span></span> 

### <a name="create-hello-controller"></a><span data-ttu-id="32396-168">Vytvořit řadič hello</span><span class="sxs-lookup"><span data-stu-id="32396-168">Create hello controller</span></span>
1. <span data-ttu-id="32396-169">V hello **trasy** adresáři projektu vytvořte nový soubor s názvem **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="32396-169">In hello **routes** directory of your project, create a new file named **tasklist.js**.</span></span> 
2. <span data-ttu-id="32396-170">Přidejte následující kód příliš hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="32396-170">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="32396-171">Tento kód načte hello moduly DocumentDBClient a async, které jsou používány **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="32396-171">This loads hello DocumentDBClient and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="32396-172">To také definovat hello **TaskList** funkce, která se předá instance hello **úloh** definovaného dříve:</span><span class="sxs-lookup"><span data-stu-id="32396-172">This also defined hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. <span data-ttu-id="32396-173">Pokračovat v přidávání toohello **tasklist.js** souboru přidáním hello metody použité příliš**showTasks, addTask**, a **Completetask**:</span><span class="sxs-lookup"><span data-stu-id="32396-173">Continue adding toohello **tasklist.js** file by adding hello methods used too**showTasks, addTask**, and **completeTasks**:</span></span>
   
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
   
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
   
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
   
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
   
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
   
                    res.redirect('/');
                });
            },
   
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
   
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };
4. <span data-ttu-id="32396-174">Uložte a zavřete hello **tasklist.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="32396-174">Save and close hello **tasklist.js** file.</span></span>

### <a name="add-configjs"></a><span data-ttu-id="32396-175">Přidání souboru config.js</span><span class="sxs-lookup"><span data-stu-id="32396-175">Add config.js</span></span>
1. <span data-ttu-id="32396-176">V adresáři projektu vytvořte nový soubor s názvem **config.js**.</span><span class="sxs-lookup"><span data-stu-id="32396-176">In your project directory create a new file named **config.js**.</span></span>
2. <span data-ttu-id="32396-177">Přidejte následující hello příliš**config.js**.</span><span class="sxs-lookup"><span data-stu-id="32396-177">Add hello following too**config.js**.</span></span> <span data-ttu-id="32396-178">Tento kód definuje nastavení konfigurace a hodnoty, které aplikace potřebuje.</span><span class="sxs-lookup"><span data-stu-id="32396-178">This defines configuration settings and values needed for our application.</span></span>
   
        var config = {}
   
        config.host = process.env.HOST || "[hello URI value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[hello PRIMARY KEY value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. <span data-ttu-id="32396-179">V hello **config.js** souboru aktualizace hello hodnoty HOST a AUTH_KEY hello hodnoty zjištěné v okně hello klíčů účtu Azure Cosmos DB na hello [portálu Microsoft Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="32396-179">In hello **config.js** file, update hello values of HOST and AUTH_KEY using hello values found in hello Keys blade of your Azure Cosmos DB account on hello [Microsoft Azure portal](https://portal.azure.com).</span></span>
4. <span data-ttu-id="32396-180">Uložte a zavřete hello **config.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="32396-180">Save and close hello **config.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="32396-181">Úprava souboru app.js</span><span class="sxs-lookup"><span data-stu-id="32396-181">Modify app.js</span></span>
1. <span data-ttu-id="32396-182">V adresáři projektu hello, otevřete hello **app.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="32396-182">In hello project directory, open hello **app.js** file.</span></span> <span data-ttu-id="32396-183">Tento soubor byl vytvořen dříve při vytvoření webové aplikace Express hello.</span><span class="sxs-lookup"><span data-stu-id="32396-183">This file was created earlier when hello Express web application was created.</span></span>
2. <span data-ttu-id="32396-184">Přidejte následující kód toohello horní části hello **app.js**</span><span class="sxs-lookup"><span data-stu-id="32396-184">Add hello following code toohello top of **app.js**</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. <span data-ttu-id="32396-185">Tento kód definuje hello konfigurační soubor toobe používá a pokračuje hodnoty tooread mimo tento soubor do některé proměnné, které brzy využijeme.</span><span class="sxs-lookup"><span data-stu-id="32396-185">This code defines hello config file toobe used, and proceeds tooread values out of this file into some variables we will use soon.</span></span>
4. <span data-ttu-id="32396-186">Nahraďte hello následující dva řádky v **app.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="32396-186">Replace hello following two lines in **app.js** file:</span></span>
   
        app.use('/', index);
        app.use('/users', users); 
   
      <span data-ttu-id="32396-187">s hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="32396-187">with hello following snippet:</span></span>
   
        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');
5. <span data-ttu-id="32396-188">Tyto řádky definují novou instanci třídy Naše **TaskDao** objekt se nové připojení tooAzure Cosmos DB (pomocí hodnot hello číst z hello **config.js**), inicializace hello objekt úkolů a svážou akce formuláře toomethods na našem **TaskList** řadiče.</span><span class="sxs-lookup"><span data-stu-id="32396-188">These lines define a new instance of our **TaskDao** object, with a new connection tooAzure Cosmos DB (using hello values read from hello **config.js**), initialize hello task object and then bind form actions toomethods on our **TaskList** controller.</span></span> 
6. <span data-ttu-id="32396-189">Nakonec uložte a zavřete hello **app.js** souboru, jsme téměř hotovi.</span><span class="sxs-lookup"><span data-stu-id="32396-189">Finally, save and close hello **app.js** file, we're just about done.</span></span>

## <span data-ttu-id="32396-190"><a name="_Toc395783181"></a>Krok 5: Vytvoření uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="32396-190"><a name="_Toc395783181"></a>Step 5: Build a user interface</span></span>
<span data-ttu-id="32396-191">Nyní přejdeme naše pozornost toobuilding hello uživatelské rozhraní, uživatel mohl ve skutečnosti spolupracovat s naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="32396-191">Now let’s turn our attention toobuilding hello user interface so a user can actually interact with our application.</span></span> <span data-ttu-id="32396-192">Hello aplikace Express jsme vytvořili používá **Jade** jako zobrazovací modul hello.</span><span class="sxs-lookup"><span data-stu-id="32396-192">hello Express application we created uses **Jade** as hello view engine.</span></span> <span data-ttu-id="32396-193">Další informace o Jade najdete příliš[http://jade-lang.com/](http://jade-lang.com/).</span><span class="sxs-lookup"><span data-stu-id="32396-193">For more information on Jade please refer too[http://jade-lang.com/](http://jade-lang.com/).</span></span>

1. <span data-ttu-id="32396-194">Hello **layout.jade** souboru v hello **zobrazení** directory se používá jako globální šablona pro ostatní **.jade** soubory.</span><span class="sxs-lookup"><span data-stu-id="32396-194">hello **layout.jade** file in hello **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="32396-195">V tomto kroku jej upravíte toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), což je sada nástrojů, který umožňuje snadno toodesign pěkných webů.</span><span class="sxs-lookup"><span data-stu-id="32396-195">In this step you will modify it toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking website.</span></span> 
2. <span data-ttu-id="32396-196">Otevřete hello **layout.jade** soubor nachází ve hello **zobrazení** hello obsah složky a nahradit s hello následující:</span><span class="sxs-lookup"><span data-stu-id="32396-196">Open hello **layout.jade** file found in hello **views** folder and replace hello contents with hello following:</span></span>

    ```
    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body
        nav.navbar.navbar-inverse.navbar-fixed-top
          div.navbar-header
            a.navbar-brand(href='#') My Tasks
        block content
        script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
        script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
    ```

    <span data-ttu-id="32396-197">Tato hodnota informuje efektivně hello **Jade** modul toorender některý kód HTML pro naši aplikaci a vytvoří **bloku** názvem **obsah** kterého můžeme zadat rozložení hello pro náš obsah stránky.</span><span class="sxs-lookup"><span data-stu-id="32396-197">This effectively tells hello **Jade** engine toorender some HTML for our application and creates a **block** called **content** where we can supply hello layout for our content pages.</span></span>

    <span data-ttu-id="32396-198">Uložte a zavřete tento soubor **layout.jade**.</span><span class="sxs-lookup"><span data-stu-id="32396-198">Save and close this **layout.jade** file.</span></span>

3. <span data-ttu-id="32396-199">Nyní otevřete hello **index.jade** soubor, hello zobrazení, která se použije v naší aplikaci a nahraďte hello obsah souboru hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="32396-199">Now open hello **index.jade** file, hello view that will be used by our application, and replace hello content of hello file with hello following:</span></span>
   
        extends layout
        block content
           h1 #{title}
           br
        
           form(action="/completetask", method="post")
             table.table.table-striped.table-bordered
               tr
                 td Name
                 td Category
                 td Date
                 td Complete
               if (typeof tasks === "undefined")
                 tr
                   td
               else
                 each task in tasks
                   tr
                     td #{task.name}
                     td #{task.category}
                     - var date  = new Date(task.date);
                     - var day   = date.getDate();
                     - var month = date.getMonth() + 1;
                     - var year  = date.getFullYear();
                     td #{month + "/" + day + "/" + year}
                     td
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn.btn-primary(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             .form-group
               label(for="name") Item Name:
               input.form-control(name="name", type="textbox")
             .form-group
               label(for="category") Item Category:
               input.form-control(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   

<span data-ttu-id="32396-200">Tento kód rozšiřuje rozložení a poskytuje obsah pro hello **obsah** zástupný symbol jsme viděli v hello **layout.jade** výše.</span><span class="sxs-lookup"><span data-stu-id="32396-200">This extends layout, and provides content for hello **content** placeholder we saw in hello **layout.jade** file earlier.</span></span>
   
<span data-ttu-id="32396-201">V tomto rozložení jsme vytvořili dva formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="32396-201">In this layout we created two HTML forms.</span></span>

<span data-ttu-id="32396-202">Hello první formulář obsahuje tabulku našich dat a tlačítko, které umožňuje nám tooupdate položky zveřejněním příliš**/completetask** metodě kontroleru.</span><span class="sxs-lookup"><span data-stu-id="32396-202">hello first form contains a table for our data and a button that allows us tooupdate items by posting too**/completetask** method of our controller.</span></span>
    
<span data-ttu-id="32396-203">Hello druhý formulář obsahuje dvě vstupní pole a tlačítko, které umožňuje nám toocreate novou položku zveřejněním příliš**/addtask** metodě kontroleru.</span><span class="sxs-lookup"><span data-stu-id="32396-203">hello second form contains two input fields and a button that allows us toocreate a new item by posting too**/addtask** method of our controller.</span></span>

<span data-ttu-id="32396-204">To by mělo být vše, co potřebujeme ke toowork naše aplikace.</span><span class="sxs-lookup"><span data-stu-id="32396-204">This should be all that we need for our application toowork.</span></span>

## <span data-ttu-id="32396-205"><a name="_Toc395783181"></a>Krok 6: Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="32396-205"><a name="_Toc395783181"></a>Step 6: Run your application locally</span></span>
1. <span data-ttu-id="32396-206">aplikace hello tootest na místním počítači, spusťte `npm start` v terminálu toostart hello vaší aplikace a pak aktualizujte vaše [http://localhost: 3000](http://localhost:3000) stránku prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="32396-206">tootest hello application on your local machine, run `npm start` in hello terminal toostart your application, then refresh your [http://localhost:3000](http://localhost:3000) browser page.</span></span> <span data-ttu-id="32396-207">stránku Hello by teď měl vypadat jako následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="32396-207">hello page should now look like hello image below:</span></span>
   
    ![Snímek obrazovky aplikace seznam úkolů v okně prohlížeče hello](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > <span data-ttu-id="32396-209">Pokud obdržíte chybu o hello odsazení v souboru layout.jade hello nebo hello index.jade, ujistěte se, že první dva řádky hello v oba soubory je ponecháno zarovnán do bloku, bez mezer.</span><span class="sxs-lookup"><span data-stu-id="32396-209">If you receive an error about hello indent in hello layout.jade file or hello index.jade file, ensure that hello first two lines in both files is left justified, with no spaces.</span></span> <span data-ttu-id="32396-210">Pokud existují prostory před hello první dva řádky, je odstranit, uložit oba soubory a pak aktualizujte okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="32396-210">If there are spaces before hello first two lines, remove them, save both files, then refresh your browser window.</span></span> 

2. <span data-ttu-id="32396-211">Použít hello položky, názvů a kategorie položek pole tooenter novou úlohu a potom klikněte na **přidat položku**.</span><span class="sxs-lookup"><span data-stu-id="32396-211">Use hello Item, Item Name and Category fields tooenter a new task and then click **Add Item**.</span></span> <span data-ttu-id="32396-212">Tak se ve službě Azure Cosmos DB vytvoří dokument s těmito vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="32396-212">This creates a document in Azure Cosmos DB with those properties.</span></span> 
3. <span data-ttu-id="32396-213">stránku Hello by měl aktualizovat toodisplay hello nově vytvořený položky v seznamu úkolů hello.</span><span class="sxs-lookup"><span data-stu-id="32396-213">hello page should update toodisplay hello newly created item in hello ToDo list.</span></span>
   
    ![Snímek obrazovky aplikace hello s novou položkou v seznamu úkolů hello](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. <span data-ttu-id="32396-215">toocomplete úlohy, jednoduše zaškrtněte políčko hello ve sloupci hello dokončení a potom klikněte na **aktualizace úloh**.</span><span class="sxs-lookup"><span data-stu-id="32396-215">toocomplete a task, simply check hello checkbox in hello Complete column, and then click **Update tasks**.</span></span> <span data-ttu-id="32396-216">Tento dokument hello aktualizace jste již vytvořili.</span><span class="sxs-lookup"><span data-stu-id="32396-216">This updates hello document you already created.</span></span>

5. <span data-ttu-id="32396-217">aplikace hello toostop, stiskněte kombinaci kláves CTRL + C v hello okno terminálu a pak klikněte na **Y** tooterminate hello dávkovou úlohu.</span><span class="sxs-lookup"><span data-stu-id="32396-217">toostop hello application, press CTRL+C in hello terminal window and then click **Y** tooterminate hello batch job.</span></span>

## <span data-ttu-id="32396-218"><a name="_Toc395783182"></a>Krok 7: Nasazení vaší aplikace tooAzure projektu vývoj webů</span><span class="sxs-lookup"><span data-stu-id="32396-218"><a name="_Toc395783182"></a>Step 7: Deploy your application development project tooAzure Websites</span></span>
1. <span data-ttu-id="32396-219">Pokud jste tak ještě neučinili, povolte úložiště Git pro Weby Azure.</span><span class="sxs-lookup"><span data-stu-id="32396-219">If you haven't already, enable a git repository for your Azure Website.</span></span> <span data-ttu-id="32396-220">Pokyny naleznete v tom toodo v hello [místní nasazení Git tooAzure služby App Service](../app-service-web/app-service-deploy-local-git.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="32396-220">You can find instructions on how toodo this in hello [Local Git Deployment tooAzure App Service](../app-service-web/app-service-deploy-local-git.md) topic.</span></span>
2. <span data-ttu-id="32396-221">Přidejte svůj web Azure jako vzdálené úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="32396-221">Add your Azure Website as a git remote.</span></span>
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. <span data-ttu-id="32396-222">Nasazení provedete odesláním toohello vzdálené.</span><span class="sxs-lookup"><span data-stu-id="32396-222">Deploy by pushing toohello remote.</span></span>
   
        git push azure master
4. <span data-ttu-id="32396-223">Během pár sekund bude git dokončí publikování webové aplikace a spustí prohlížeč, kde uvidíte vaše handiwork běžící v Azure!</span><span class="sxs-lookup"><span data-stu-id="32396-223">In a few seconds, git will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>

    <span data-ttu-id="32396-224">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="32396-224">Congratulations!</span></span> <span data-ttu-id="32396-225">Máte právě vytvořené vaše první webovou aplikaci Node.js Express pomocí Azure Cosmos DB a publikovali jste ji tooAzure weby.</span><span class="sxs-lookup"><span data-stu-id="32396-225">You have just built your first Node.js Express Web Application using Azure Cosmos DB and published it tooAzure Websites.</span></span>

    <span data-ttu-id="32396-226">Chcete-li toodownload nebo odkazovat toohello úplné referenční aplikace pro tento kurz, ho můžete stáhnout z [Githubu][GitHub].</span><span class="sxs-lookup"><span data-stu-id="32396-226">If you want toodownload or refer toohello complete reference application for this tutorial, it can be downloaded from [GitHub][GitHub].</span></span>

## <span data-ttu-id="32396-227"><a name="_Toc395637775"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="32396-227"><a name="_Toc395637775"></a>Next steps</span></span>

* <span data-ttu-id="32396-228">Chcete tooperform škálování a výkon testování pomocí Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="32396-228">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="32396-229">Viz [Testování výkonu a škálování pomocí služby Azure Cosmos DB](performance-testing.md).</span><span class="sxs-lookup"><span data-stu-id="32396-229">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="32396-230">Zjistěte, jak příliš[monitorovat účet Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="32396-230">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="32396-231">Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="32396-231">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="32396-232">Prozkoumejte hello [dokumentace Azure Cosmos DB](https://docs.microsoft.com/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="32396-232">Explore hello [Azure Cosmos DB documentation](https://docs.microsoft.com/azure/documentdb/).</span></span>

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

