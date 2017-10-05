---
title: "Sestavení webové aplikace Node.js pro Azure Cosmos DB | Microsoft Docs"
description: "Tento kurz Node.js popisuje jak používat Microsoft Azure Cosmos databázi k ukládání a přístup k datům z webové aplikace Node.js Express hostované na webech Azure."
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
ms.openlocfilehash: 1a98509a98bcd2a5de593eb006f905766fe72966
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <span data-ttu-id="17afb-104"><a name="_Toc395783175"></a>Sestavení webové aplikace Node.js využívající službu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="17afb-104"><a name="_Toc395783175"></a>Build a Node.js web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="17afb-105">.NET</span><span class="sxs-lookup"><span data-stu-id="17afb-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="17afb-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="17afb-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="17afb-107">Java</span><span class="sxs-lookup"><span data-stu-id="17afb-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="17afb-108">Python</span><span class="sxs-lookup"><span data-stu-id="17afb-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="17afb-109">Tento kurz Node.js popisuje, jak pomocí Azure Cosmos DB a rozhraní API DocumentDB ukládat a přistupovat k datům z aplikace Node.js Express hostované na webech Azure.</span><span class="sxs-lookup"><span data-stu-id="17afb-109">This Node.js tutorial shows you how to use Azure Cosmos DB and the DocumentDB API to store and access data from a Node.js Express application hosted on Azure Websites.</span></span> <span data-ttu-id="17afb-110">Vytvoříte jednoduchou webovou aplikaci pro správu úkolů, aplikaci seznamu úkolů, která umožní vytvářet, získávat a dokončovat úkoly.</span><span class="sxs-lookup"><span data-stu-id="17afb-110">You build a simple web-based task-management application, a ToDo app, that allows creating, retrieving, and completing tasks.</span></span> <span data-ttu-id="17afb-111">Úkoly se ve službě Azure Cosmos DB ukládají jako dokumenty JSON.</span><span class="sxs-lookup"><span data-stu-id="17afb-111">The tasks are stored as JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="17afb-112">Tento kurz vás provede procesem vytvoření a nasazení aplikace a vysvětlí, co se v každé části děje.</span><span class="sxs-lookup"><span data-stu-id="17afb-112">This tutorial walks you through the creation and deployment of the app and explains what's happening in each snippet.</span></span>

![Snímek obrazovky aplikace pro seznam úkolů vytvořené v tomto kurzu k Node.js](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

<span data-ttu-id="17afb-114">Nemáte čas absolvovat celý tento kurz a chcete jen získat celé řešení?</span><span class="sxs-lookup"><span data-stu-id="17afb-114">Don't have time to complete the tutorial and just want to get the complete solution?</span></span> <span data-ttu-id="17afb-115">To není problém, celé ukázkové řešení si můžete stáhnout z [GitHubu][GitHub].</span><span class="sxs-lookup"><span data-stu-id="17afb-115">Not a problem, you can get the complete sample solution from [GitHub][GitHub].</span></span> <span data-ttu-id="17afb-116">Pokud potřebujete pokyny, jak aplikaci spustit, stačí si přečíst soubor [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="17afb-116">Just read the [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) file for instructions on how to run the app.</span></span>

## <span data-ttu-id="17afb-117"><a name="_Toc395783176"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="17afb-117"><a name="_Toc395783176"></a>Prerequisites</span></span>
> [!TIP]
> <span data-ttu-id="17afb-118">V tomto kurzu Node.js se předpokládá, že již máte zkušenosti s používáním Node.js a Webů Azure.</span><span class="sxs-lookup"><span data-stu-id="17afb-118">This Node.js tutorial assumes that you have some prior experience using Node.js and Azure Websites.</span></span>
> 
> 

<span data-ttu-id="17afb-119">Než budete postupovat podle pokynů tohoto článku, měli byste se ujistit, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="17afb-119">Before following the instructions in this article, you should ensure that you have the following:</span></span>

* <span data-ttu-id="17afb-120">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="17afb-120">An active Azure account.</span></span> <span data-ttu-id="17afb-121">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="17afb-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="17afb-122">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="17afb-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

   <span data-ttu-id="17afb-123">NEBO</span><span class="sxs-lookup"><span data-stu-id="17afb-123">OR</span></span>

   <span data-ttu-id="17afb-124">Místní instalaci [emulátoru DB Cosmos Azure](local-emulator.md) (jenom Windows).</span><span class="sxs-lookup"><span data-stu-id="17afb-124">A local installation of the [Azure Cosmos DB Emulator](local-emulator.md) (Windows only).</span></span>
* <span data-ttu-id="17afb-125">[Node.js][Node.js] verze 0.10.29 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="17afb-125">[Node.js][Node.js] version v0.10.29 or higher.</span></span>
* <span data-ttu-id="17afb-126">[Generátor Express](http://www.expressjs.com/starter/generator.html) (lze nainstalovat prostřednictvím `npm install express-generator -g`)</span><span class="sxs-lookup"><span data-stu-id="17afb-126">[Express generator](http://www.expressjs.com/starter/generator.html) (you can install this via `npm install express-generator -g`)</span></span>
* <span data-ttu-id="17afb-127">[Git][Git]</span><span class="sxs-lookup"><span data-stu-id="17afb-127">[Git][Git].</span></span>

## <span data-ttu-id="17afb-128"><a name="_Toc395637761"></a>Krok 1: Vytvoření účtu databáze Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="17afb-128"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="17afb-129">Začněme vytvořením účtu služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="17afb-129">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="17afb-130">Pokud již účet máte nebo pokud používáte pro účely tohoto kurzu emulátor služby Azure Cosmos DB, můžete přeskočit na [Krok 2: Vytvoření nové aplikace Node.js](#_Toc395783178).</span><span class="sxs-lookup"><span data-stu-id="17afb-130">If you already have an account or if you are using the Azure Cosmos DB Emulator for this tutorial, you can skip to [Step 2: Create a new Node.js application](#_Toc395783178).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="17afb-131"><a name="_Toc395783178"></a>Krok 2: Vytvoření nové aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="17afb-131"><a name="_Toc395783178"></a>Step 2: Create a new Node.js application</span></span>
<span data-ttu-id="17afb-132">Nyní se naučíme, jak vytvořit základní projekt Node.js Hello World pomocí rozhraní [Express](http://expressjs.com/).</span><span class="sxs-lookup"><span data-stu-id="17afb-132">Now let's learn to create a basic Hello World Node.js project using the [Express](http://expressjs.com/) framework.</span></span>

1. <span data-ttu-id="17afb-133">Otevřete svůj oblíbený terminál, jako je třeba příkazový řádek Node.js.</span><span class="sxs-lookup"><span data-stu-id="17afb-133">Open your favorite terminal, such as the Node.js command prompt.</span></span>
2. <span data-ttu-id="17afb-134">Přejděte do adresáře, do kterého chcete novou aplikaci uložit.</span><span class="sxs-lookup"><span data-stu-id="17afb-134">Navigate to the directory in which you'd like to store the new application.</span></span>
3. <span data-ttu-id="17afb-135">Pomocí generátoru Express vygenerujte novou aplikaci s názvem **todo**.</span><span class="sxs-lookup"><span data-stu-id="17afb-135">Use the express generator to generate a new application called **todo**.</span></span>
   
        express todo
4. <span data-ttu-id="17afb-136">Otevřete nový adresář **todo** a nainstalujte závislosti.</span><span class="sxs-lookup"><span data-stu-id="17afb-136">Open your new **todo** directory and install dependencies.</span></span>
   
        cd todo
        npm install
5. <span data-ttu-id="17afb-137">Spusťte novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="17afb-137">Run your new application.</span></span>
   
        npm start
6. <span data-ttu-id="17afb-138">Pokud si chcete zobrazit novou aplikaci, přejděte v prohlížeči na adresu [http://localhost:3000](http://localhost:3000).</span><span class="sxs-lookup"><span data-stu-id="17afb-138">You can view your new application by navigating your browser to [http://localhost:3000](http://localhost:3000).</span></span>
   
    ![Výuka Node.js – snímek obrazovky aplikace Hello World v okně prohlížeče](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    <span data-ttu-id="17afb-140">Potom aplikaci zastavte stisknutím kombinace kláves CTRL + C v okně terminálu a ukončete dávkovou úlohu kliknutím na **y**.</span><span class="sxs-lookup"><span data-stu-id="17afb-140">Then, to stop the application, press CTRL+C in the terminal window and then click **y** to terminate the batch job.</span></span>

## <span data-ttu-id="17afb-141"><a name="_Toc395783179"></a>Krok 3: Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="17afb-141"><a name="_Toc395783179"></a>Step 3: Install additional modules</span></span>
<span data-ttu-id="17afb-142">Soubor **package.json** je jedním ze souborů vytvořených v kořenu projektu.</span><span class="sxs-lookup"><span data-stu-id="17afb-142">The **package.json** file is one of the files created in the root of the project.</span></span> <span data-ttu-id="17afb-143">Tento soubor obsahuje seznam dalších modulů, které aplikace Node.js vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="17afb-143">This file contains a list of additional modules that are required for your Node.js application.</span></span> <span data-ttu-id="17afb-144">Později při nasazení této aplikace na weby Azure, tento soubor se používá k určení, které moduly musí být nainstalovaný na Azure pro podporu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="17afb-144">Later, when you deploy this application to Azure Websites, this file is used to determine which modules need to be installed on Azure to support your application.</span></span> <span data-ttu-id="17afb-145">Pro tento kurz je ještě nutné nainstalovat další dva balíčky.</span><span class="sxs-lookup"><span data-stu-id="17afb-145">We still need to install two more packages for this tutorial.</span></span>

1. <span data-ttu-id="17afb-146">Po návratu na terminál nainstalujte pomocí npm modul **async**.</span><span class="sxs-lookup"><span data-stu-id="17afb-146">Back in the terminal, install the **async** module via npm.</span></span>
   
        npm install async --save
2. <span data-ttu-id="17afb-147">Přes npm nainstalujte modul **documentdb**.</span><span class="sxs-lookup"><span data-stu-id="17afb-147">Install the **documentdb** module via npm.</span></span> <span data-ttu-id="17afb-148">To je modul, kde se stane všechny magic Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="17afb-148">This is the module where all the Azure Cosmos DB magic happens.</span></span>
   
        npm install documentdb --save
3. <span data-ttu-id="17afb-149">Rychlý pohled do souboru **package.json** aplikace by měl odhalit další moduly.</span><span class="sxs-lookup"><span data-stu-id="17afb-149">A quick check of the **package.json** file of the application should show the additional modules.</span></span> <span data-ttu-id="17afb-150">Tento soubor informuje Azure, které balíčky stáhnout a nainstalovat při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="17afb-150">This file will tell Azure which packages to download and install when running your application.</span></span> <span data-ttu-id="17afb-151">Měl by vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="17afb-151">It should resemble the example below.</span></span>
   
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
   
    <span data-ttu-id="17afb-152">To říká uzlu (a později Azure), že vaše aplikace závisí na těchto dalších modulech.</span><span class="sxs-lookup"><span data-stu-id="17afb-152">This tells Node (and Azure later) that your application depends on these additional modules.</span></span>

## <span data-ttu-id="17afb-153"><a name="_Toc395783180"></a>Krok 4: Využití služby Azure Cosmos DB v aplikaci Node.js</span><span class="sxs-lookup"><span data-stu-id="17afb-153"><a name="_Toc395783180"></a>Step 4: Using the Azure Cosmos DB service in a node application</span></span>
<span data-ttu-id="17afb-154">Tím je hotovo veškeré počáteční nastavování a konfigurace – nyní se budeme zabývat tím, proč tu jsme, tedy psaním kódu s využitím služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="17afb-154">That takes care of all the initial setup and configuration, now let’s get down to why we’re here, and that’s to write some code using Azure Cosmos DB.</span></span>

### <a name="create-the-model"></a><span data-ttu-id="17afb-155">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="17afb-155">Create the model</span></span>
1. <span data-ttu-id="17afb-156">V adresáři projektu vytvořte nový adresář s názvem **models** ve stejném adresáři jako soubor package.json.</span><span class="sxs-lookup"><span data-stu-id="17afb-156">In the project directory, create a new directory named **models** in the same directory as the package.json file.</span></span>
2. <span data-ttu-id="17afb-157">V adresáři **models** vytvořte nový soubor s názvem **taskDao.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-157">In the **models** directory, create a new file named **taskDao.js**.</span></span> <span data-ttu-id="17afb-158">Tento soubor bude obsahovat model pro úkoly vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="17afb-158">This file will contain the model for the tasks created by our application.</span></span>
3. <span data-ttu-id="17afb-159">Ve stejném adresáři **models** vytvořte jiný soubor s názvem **docdbUtils.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-159">In the same **models** directory, create another new file named **docdbUtils.js**.</span></span> <span data-ttu-id="17afb-160">V tomto souboru bude obsažen užitečný, opakovaně použitelný kód, který budeme používat v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="17afb-160">This file will contain some useful, reusable, code that we will use throughout our application.</span></span> 
4. <span data-ttu-id="17afb-161">Do **docdbUtils.js** zkopírujte následující kód.</span><span class="sxs-lookup"><span data-stu-id="17afb-161">Copy the following code in to **docdbUtils.js**</span></span>
   
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
   
5. <span data-ttu-id="17afb-162">Uložte a zavřete soubor **docdbUtils.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-162">Save and close the **docdbUtils.js** file.</span></span>
6. <span data-ttu-id="17afb-163">Na začátek souboru **taskDao.js** přidejte následující kód, který odkazuje na **DocumentDBClient** a **docdbUtils.js**, které jsme vytvořili výše:</span><span class="sxs-lookup"><span data-stu-id="17afb-163">At the beginning of the **taskDao.js** file, add the following code to reference the **DocumentDBClient** and the **docdbUtils.js** we created above:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. <span data-ttu-id="17afb-164">Dále přidáte kód, který definuje export objektu Task.</span><span class="sxs-lookup"><span data-stu-id="17afb-164">Next, you will add code to define and export the Task object.</span></span> <span data-ttu-id="17afb-165">Ten má na starosti inicializaci objektu Task a nastavení databáze a kolekce dokumentů, které budeme používat.</span><span class="sxs-lookup"><span data-stu-id="17afb-165">This is responsible for initializing our Task object and setting up the Database and Document Collection we will use.</span></span>
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. <span data-ttu-id="17afb-166">Dále přidejte následující kód, který definuje další metody objektu Task. Ty umožňují pracovat s daty uloženými ve službě Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="17afb-166">Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in Azure Cosmos DB.</span></span>
   
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
9. <span data-ttu-id="17afb-167">Uložte a zavřete soubor **taskDao.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-167">Save and close the **taskDao.js** file.</span></span> 

### <a name="create-the-controller"></a><span data-ttu-id="17afb-168">Vytvoření kontroleru</span><span class="sxs-lookup"><span data-stu-id="17afb-168">Create the controller</span></span>
1. <span data-ttu-id="17afb-169">V adresáři **routes** projektu vytvořte nový soubor s názvem **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-169">In the **routes** directory of your project, create a new file named **tasklist.js**.</span></span> 
2. <span data-ttu-id="17afb-170">Do souboru **tasklist.js** přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="17afb-170">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="17afb-171">Tento kód načte moduly DocumentDBClient a async, které budou používány v souboru **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-171">This loads the DocumentDBClient and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="17afb-172">Také v něm je definována funkce **TaskList**, která přijímá instanci objektu **Task** definovaného dříve:</span><span class="sxs-lookup"><span data-stu-id="17afb-172">This also defined the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. <span data-ttu-id="17afb-173">Dále do souboru **tasklist.js** přidejte metody **showTasks, addTask** a **completeTask**:</span><span class="sxs-lookup"><span data-stu-id="17afb-173">Continue adding to the **tasklist.js** file by adding the methods used to **showTasks, addTask**, and **completeTasks**:</span></span>
   
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
4. <span data-ttu-id="17afb-174">Uložte a zavřete soubor **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-174">Save and close the **tasklist.js** file.</span></span>

### <a name="add-configjs"></a><span data-ttu-id="17afb-175">Přidání souboru config.js</span><span class="sxs-lookup"><span data-stu-id="17afb-175">Add config.js</span></span>
1. <span data-ttu-id="17afb-176">V adresáři projektu vytvořte nový soubor s názvem **config.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-176">In your project directory create a new file named **config.js**.</span></span>
2. <span data-ttu-id="17afb-177">Do souboru **config.js** přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="17afb-177">Add the following to **config.js**.</span></span> <span data-ttu-id="17afb-178">Tento kód definuje nastavení konfigurace a hodnoty, které aplikace potřebuje.</span><span class="sxs-lookup"><span data-stu-id="17afb-178">This defines configuration settings and values needed for our application.</span></span>
   
        var config = {}
   
        config.host = process.env.HOST || "[the URI value from the Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. <span data-ttu-id="17afb-179">V souboru **config.js** aktualizujte hodnoty HOST a AUTH_KEY s použitím hodnot, které získáte v okně Klíče účtu služby Azure Cosmos DB na webu [Microsoft Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="17afb-179">In the **config.js** file, update the values of HOST and AUTH_KEY using the values found in the Keys blade of your Azure Cosmos DB account on the [Microsoft Azure portal](https://portal.azure.com).</span></span>
4. <span data-ttu-id="17afb-180">Uložte a zavřete soubor **config.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-180">Save and close the **config.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="17afb-181">Úprava souboru app.js</span><span class="sxs-lookup"><span data-stu-id="17afb-181">Modify app.js</span></span>
1. <span data-ttu-id="17afb-182">V adresáři projektu otevřete soubor **app.js**.</span><span class="sxs-lookup"><span data-stu-id="17afb-182">In the project directory, open the **app.js** file.</span></span> <span data-ttu-id="17afb-183">Tento soubor byl vytvořen již dříve, při vytváření webové aplikace Express.</span><span class="sxs-lookup"><span data-stu-id="17afb-183">This file was created earlier when the Express web application was created.</span></span>
2. <span data-ttu-id="17afb-184">Do horní části souboru **app.js** přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="17afb-184">Add the following code to the top of **app.js**</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. <span data-ttu-id="17afb-185">Tento kód definuje konfigurační soubor, který se má použít, a načte z něj do proměnných hodnoty, které brzy využijeme.</span><span class="sxs-lookup"><span data-stu-id="17afb-185">This code defines the config file to be used, and proceeds to read values out of this file into some variables we will use soon.</span></span>
4. <span data-ttu-id="17afb-186">Nahraďte v souboru **app.js** následující dva řádky:</span><span class="sxs-lookup"><span data-stu-id="17afb-186">Replace the following two lines in **app.js** file:</span></span>
   
        app.use('/', index);
        app.use('/users', users); 
   
      <span data-ttu-id="17afb-187">následujícím fragmentem kódu:</span><span class="sxs-lookup"><span data-stu-id="17afb-187">with the following snippet:</span></span>
   
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
5. <span data-ttu-id="17afb-188">Tyto řádky definují novou instanci objektu **TaskDao** s novým připojením ke službě Azure Cosmos DB (pomocí hodnot přečtených ze souboru **config.js**), inicializují objekt úkolů a svážou akce formuláře s metodami kontroleru **TaskList**.</span><span class="sxs-lookup"><span data-stu-id="17afb-188">These lines define a new instance of our **TaskDao** object, with a new connection to Azure Cosmos DB (using the values read from the **config.js**), initialize the task object and then bind form actions to methods on our **TaskList** controller.</span></span> 
6. <span data-ttu-id="17afb-189">Nakonec soubor **app.js** uložte a zavřete a jsme téměř hotovi.</span><span class="sxs-lookup"><span data-stu-id="17afb-189">Finally, save and close the **app.js** file, we're just about done.</span></span>

## <span data-ttu-id="17afb-190"><a name="_Toc395783181"></a>Krok 5: Vytvoření uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="17afb-190"><a name="_Toc395783181"></a>Step 5: Build a user interface</span></span>
<span data-ttu-id="17afb-191">Nyní se zaměřme na vytvoření uživatelského rozhraní, aby uživatelé mohli s aplikací pracovat.</span><span class="sxs-lookup"><span data-stu-id="17afb-191">Now let’s turn our attention to building the user interface so a user can actually interact with our application.</span></span> <span data-ttu-id="17afb-192">Aplikace Express, kterou jsme vytvořili, používá jako zobrazovací modul **Jade**.</span><span class="sxs-lookup"><span data-stu-id="17afb-192">The Express application we created uses **Jade** as the view engine.</span></span> <span data-ttu-id="17afb-193">Další informace o Jade najdete na stránce [http://jade-lang.com/](http://jade-lang.com/).</span><span class="sxs-lookup"><span data-stu-id="17afb-193">For more information on Jade please refer to [http://jade-lang.com/](http://jade-lang.com/).</span></span>

1. <span data-ttu-id="17afb-194">Soubor **layout.jade** v adresáři **views** slouží jako globální šablona pro ostatní soubory **.jade**.</span><span class="sxs-lookup"><span data-stu-id="17afb-194">The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="17afb-195">V tomto kroku jej upravíte tak, aby používal [Twitter Bootstrap](https://github.com/twbs/bootstrap). To je sada nástrojů, které usnadňují návrh pěkných webů.</span><span class="sxs-lookup"><span data-stu-id="17afb-195">In this step you will modify it to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking website.</span></span> 
2. <span data-ttu-id="17afb-196">Otevřete soubor **layout.jade** umístěný v adresáři **views** a nahraďte jeho obsah tímto:</span><span class="sxs-lookup"><span data-stu-id="17afb-196">Open the **layout.jade** file found in the **views** folder and replace the contents with the following:</span></span>

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

    <span data-ttu-id="17afb-197">Tímto modul **Jade** dostává instrukci vykreslovat pro naši aplikaci některý kód HTML a vytvoří **block** s názvem **content**, do kterého můžeme zadat rozložení stránek s obsahem.</span><span class="sxs-lookup"><span data-stu-id="17afb-197">This effectively tells the **Jade** engine to render some HTML for our application and creates a **block** called **content** where we can supply the layout for our content pages.</span></span>

    <span data-ttu-id="17afb-198">Uložte a zavřete tento soubor **layout.jade**.</span><span class="sxs-lookup"><span data-stu-id="17afb-198">Save and close this **layout.jade** file.</span></span>

3. <span data-ttu-id="17afb-199">Nyní otevřete soubor **index.jade**, který představuje zobrazení používané v naší aplikaci a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="17afb-199">Now open the **index.jade** file, the view that will be used by our application, and replace the content of the file with the following:</span></span>
   
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
   

<span data-ttu-id="17afb-200">Tento kód rozšiřuje rozložení a poskytuje obsah pro zástupný symbol **content**, který jsme viděli v souboru **layout.jade** výše.</span><span class="sxs-lookup"><span data-stu-id="17afb-200">This extends layout, and provides content for the **content** placeholder we saw in the **layout.jade** file earlier.</span></span>
   
<span data-ttu-id="17afb-201">V tomto rozložení jsme vytvořili dva formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="17afb-201">In this layout we created two HTML forms.</span></span>

<span data-ttu-id="17afb-202">První formulář obsahuje tabulku našich dat a tlačítko, které umožňuje aktualizovat položky odesláním informací metodě kontroleru **/completetask**.</span><span class="sxs-lookup"><span data-stu-id="17afb-202">The first form contains a table for our data and a button that allows us to update items by posting to **/completetask** method of our controller.</span></span>
    
<span data-ttu-id="17afb-203">Druhý formulář obsahuje dvě vstupní pole a tlačítko, které umožňuje vytvořit novou položku odesláním informací metodě kontroleru **/addtask**.</span><span class="sxs-lookup"><span data-stu-id="17afb-203">The second form contains two input fields and a button that allows us to create a new item by posting to **/addtask** method of our controller.</span></span>

<span data-ttu-id="17afb-204">To by mělo být vše, co potřebujeme ke zprovoznění aplikace.</span><span class="sxs-lookup"><span data-stu-id="17afb-204">This should be all that we need for our application to work.</span></span>

## <span data-ttu-id="17afb-205"><a name="_Toc395783181"></a>Krok 6: Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="17afb-205"><a name="_Toc395783181"></a>Step 6: Run your application locally</span></span>
1. <span data-ttu-id="17afb-206">Pokud chcete aplikaci otestovat na místním počítači, spusťte aplikaci spuštěním příkazu `npm start` v terminálu a pak aktualizujte stránku prohlížeče [http://localhost:3000](http://localhost:3000).</span><span class="sxs-lookup"><span data-stu-id="17afb-206">To test the application on your local machine, run `npm start` in the terminal to start your application, then refresh your [http://localhost:3000](http://localhost:3000) browser page.</span></span> <span data-ttu-id="17afb-207">Stránka by teď měla vypadat jako na obrázku níže:</span><span class="sxs-lookup"><span data-stu-id="17afb-207">The page should now look like the image below:</span></span>
   
    ![Snímek obrazovky aplikace Seznam úkolů v okně prohlížeče](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > <span data-ttu-id="17afb-209">Pokud se vám zobrazí chyba týkající se odsazení v souboru layout.jade nebo index.jade, ujistěte se, že jsou první dva řádky v obou souborech zarovnané vlevo a nejsou tam žádné mezery.</span><span class="sxs-lookup"><span data-stu-id="17afb-209">If you receive an error about the indent in the layout.jade file or the index.jade file, ensure that the first two lines in both files is left justified, with no spaces.</span></span> <span data-ttu-id="17afb-210">Pokud jsou před prvními dvěma řádky mezery, odeberte je, oba soubory uložte a pak aktualizujte okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="17afb-210">If there are spaces before the first two lines, remove them, save both files, then refresh your browser window.</span></span> 

2. <span data-ttu-id="17afb-211">Pro zadání nového úkolu použijte pole Položka, Název položky a Kategorie a pak klikněte na **Přidat položku**.</span><span class="sxs-lookup"><span data-stu-id="17afb-211">Use the Item, Item Name and Category fields to enter a new task and then click **Add Item**.</span></span> <span data-ttu-id="17afb-212">Tak se ve službě Azure Cosmos DB vytvoří dokument s těmito vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="17afb-212">This creates a document in Azure Cosmos DB with those properties.</span></span> 
3. <span data-ttu-id="17afb-213">Stránka by se měla aktualizovat, aby se v seznamu úkolů zobrazila nově vytvořená položka.</span><span class="sxs-lookup"><span data-stu-id="17afb-213">The page should update to display the newly created item in the ToDo list.</span></span>
   
    ![Snímek obrazovky aplikace s novou položkou v seznamu úkolů](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. <span data-ttu-id="17afb-215">Pokud chcete dokončit úkol, stačí zaškrtnout políčko ve sloupci Complete (Dokončeno) a kliknout na **Update tasks** (Aktualizovat úkoly).</span><span class="sxs-lookup"><span data-stu-id="17afb-215">To complete a task, simply check the checkbox in the Complete column, and then click **Update tasks**.</span></span> <span data-ttu-id="17afb-216">Takto se vámi vytvořený dokument aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="17afb-216">This updates the document you already created.</span></span>

5. <span data-ttu-id="17afb-217">Pokud chcete aplikaci zastavit, stiskněte kombinace kláves CTRL + C v okně terminálu a pak ukončete dávkovou úlohu kliknutím na **y**.</span><span class="sxs-lookup"><span data-stu-id="17afb-217">To stop the application, press CTRL+C in the terminal window and then click **Y** to terminate the batch job.</span></span>

## <span data-ttu-id="17afb-218"><a name="_Toc395783182"></a>Krok 7: Nasazení vývojového projektu aplikace na Azure Websites</span><span class="sxs-lookup"><span data-stu-id="17afb-218"><a name="_Toc395783182"></a>Step 7: Deploy your application development project to Azure Websites</span></span>
1. <span data-ttu-id="17afb-219">Pokud jste tak ještě neučinili, povolte úložiště Git pro Weby Azure.</span><span class="sxs-lookup"><span data-stu-id="17afb-219">If you haven't already, enable a git repository for your Azure Website.</span></span> <span data-ttu-id="17afb-220">Pokyny, jak máte postupovat, najdete v tématu [Místní nasazení přes Git do Azure App Service](../app-service-web/app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="17afb-220">You can find instructions on how to do this in the [Local Git Deployment to Azure App Service](../app-service-web/app-service-deploy-local-git.md) topic.</span></span>
2. <span data-ttu-id="17afb-221">Přidejte svůj web Azure jako vzdálené úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="17afb-221">Add your Azure Website as a git remote.</span></span>
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. <span data-ttu-id="17afb-222">Nasazení provedete odesláním na vzdálené úložiště.</span><span class="sxs-lookup"><span data-stu-id="17afb-222">Deploy by pushing to the remote.</span></span>
   
        git push azure master
4. <span data-ttu-id="17afb-223">Během pár sekund bude git dokončí publikování webové aplikace a spustí prohlížeč, kde uvidíte vaše handiwork běžící v Azure!</span><span class="sxs-lookup"><span data-stu-id="17afb-223">In a few seconds, git will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>

    <span data-ttu-id="17afb-224">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="17afb-224">Congratulations!</span></span> <span data-ttu-id="17afb-225">Právě jste vytvořili svou první webovou aplikaci Node.js Express využívající službu Azure Cosmos DB a publikovali jste ji ve službě Azure Websites.</span><span class="sxs-lookup"><span data-stu-id="17afb-225">You have just built your first Node.js Express Web Application using Azure Cosmos DB and published it to Azure Websites.</span></span>

    <span data-ttu-id="17afb-226">Pokud chcete stáhnout nebo odkazovat na úplnou aplikaci referencí tohoto kurzu, můžete ji stáhnout z [GitHubu][GitHub].</span><span class="sxs-lookup"><span data-stu-id="17afb-226">If you want to download or refer to the complete reference application for this tutorial, it can be downloaded from [GitHub][GitHub].</span></span>

## <span data-ttu-id="17afb-227"><a name="_Toc395637775"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="17afb-227"><a name="_Toc395637775"></a>Next steps</span></span>

* <span data-ttu-id="17afb-228">Chcete testovat škálování a výkon pomocí služby Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="17afb-228">Want to perform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="17afb-229">Viz [Testování výkonu a škálování pomocí služby Azure Cosmos DB](performance-testing.md).</span><span class="sxs-lookup"><span data-stu-id="17afb-229">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="17afb-230">Zjistěte, jak [monitorovat účet služby Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="17afb-230">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="17afb-231">Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="17afb-231">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="17afb-232">Prozkoumejte [dokumentaci ke službě Azure Cosmos DB](https://docs.microsoft.com/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="17afb-232">Explore the [Azure Cosmos DB documentation](https://docs.microsoft.com/azure/documentdb/).</span></span>

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

