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
# <a name="_Toc395783175"></a>Sestavení webové aplikace Node.js využívající službu Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Tento kurz Node.js popisuje, jak toouse Azure Cosmos DB a hello DocumentDB API toostore a přístupová data z aplikace Node.js Express hostované na webech Azure. Vytvoříte jednoduchou webovou aplikaci pro správu úkolů, aplikaci seznamu úkolů, která umožní vytvářet, získávat a dokončovat úkoly. Hello úlohy jsou uloženy jako dokumenty JSON v Azure Cosmos DB. Tento kurz vás provede procesem vytvoření hello a nasazení aplikace hello a vysvětluje, co se děje v každé fragmentu kódu.

![Snímek obrazovky aplikace seznam úkolů vytvořené v tomto kurzu Node.js hello](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

Nemáte čas toocomplete hello kurzu a právě má tooget hello kompletního řešení? Nejedná se o problém, můžete získat hello celé ukázkové řešení z [Githubu][GitHub]. Jen pro čtení hello [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) souboru pokyny, jak toorun hello aplikace.

## <a name="_Toc395783176"></a>Požadavky
> [!TIP]
> V tomto kurzu Node.js se předpokládá, že již máte zkušenosti s používáním Node.js a Webů Azure.
> 
> 

Před následující hello pokyny v tomto článku, se ujistěte, že máte hello následující:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

   NEBO

   Místní instalace hello [emulátoru DB Cosmos Azure](local-emulator.md) (jenom Windows).
* [Node.js][Node.js] verze 0.10.29 nebo vyšší
* [Generátor Express](http://www.expressjs.com/starter/generator.html) (lze nainstalovat prostřednictvím `npm install express-generator -g`)
* [Git][Git]

## <a name="_Toc395637761"></a>Krok 1: Vytvoření účtu databáze Azure Cosmos DB
Začněme vytvořením účtu služby Azure Cosmos DB. Pokud již máte účet, nebo pokud používáte hello emulátoru DB Cosmos Azure pro účely tohoto kurzu, můžete přeskočit příliš[krok 2: vytvoření nové aplikace Node.js](#_Toc395783178).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>Krok 2: Vytvoření nové aplikace Node.js
Nyní naučíme toocreate na základní projekt Hello World Node.js pomocí hello [Express](http://expressjs.com/) framework.

1. Otevřete svůj oblíbený terminál, jako je například příkazového řádku Node.js hello.
2. Přejděte toohello adresář, ve kterém chcete toostore hello novou aplikaci.
3. Použít toogenerate generátor express hello nové aplikace volá **todo**.
   
        express todo
4. Otevřete nový adresář **todo** a nainstalujte závislosti.
   
        cd todo
        npm install
5. Spusťte novou aplikaci.
   
        npm start
6. Můžete zobrazit novou aplikaci, přejděte v prohlížeči příliš[http://localhost: 3000](http://localhost:3000).
   
    ![Výuka Node.js – snímek obrazovky aplikace Hello, World v okně prohlížeče hello](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    Potom toostop hello aplikace, stiskněte kombinaci kláves CTRL + C v hello okno terminálu a pak klikněte na **y** tooterminate hello dávkovou úlohu.

## <a name="_Toc395783179"></a>Krok 3: Instalace dalších modulů
Hello **package.json** souboru je jedním z hello soubory vytvořené v kořenovém hello hello projektu. Tento soubor obsahuje seznam dalších modulů, které aplikace Node.js vyžaduje. Později při nasazení této aplikace tooAzure weby, tento soubor je použité toodetermine toobe které moduly potřeba nainstalovat na Azure toosupport vaší aplikace. Stále potřebujeme tooinstall dva další balíčky pro tento kurz.

1. Zpět v hello terminálu, nainstalujte hello **asynchronní** pomocí npm modul.
   
        npm install async --save
2. Nainstalujte hello **documentdb** pomocí npm modul. Toto je hello modulu, kde se stane všechny magic hello Azure Cosmos DB.
   
        npm install documentdb --save
3. Rychlou kontrolu hello **package.json** souboru hello aplikace by měl zobrazit hello dalších modulů. Tento soubor bude informuje Azure, které balíčky toodownload a nainstalovat při spuštění aplikace. Měl by vypadat hello příklad.
   
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
   
    To říká uzlu (a později Azure), že vaše aplikace závisí na těchto dalších modulech.

## <a name="_Toc395783180"></a>Krok 4: Využití služby Azure Cosmos DB hello v aplikaci node
Který se stará o všechny hello počáteční instalace a konfigurace – nyní Pojďme get dolů toowhy jsme tu, na kterém jsou toowrite některé programování s využitím Azure Cosmos DB.

### <a name="create-hello-model"></a>Vytvoření modelu hello
1. V adresáři projektu hello, vytvořte nový adresář s názvem **modely** v hello stejného adresáře jako soubor package.json hello.
2. V hello **modely** adresáře, vytvořte nový soubor s názvem **taskDao.js**. Tento soubor bude obsahovat hello model pro úkoly hello vytvořené v aplikaci.
3. V hello stejné **modely** adresáře, vytvořte další nový soubor s názvem **docdbUtils.js**. V tomto souboru bude obsažen užitečný, opakovaně použitelný kód, který budeme používat v celé aplikaci. 
4. Kopírování hello následující kód v příliš**docdbUtils.js**
   
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
   
5. Uložte a zavřete hello **docdbUtils.js** souboru.
6. Na začátku hello hello **taskDao.js** soubor, přidejte následující kód tooreference hello hello **DocumentDBClient** a hello **docdbUtils.js** jsme vytvořili výše:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. Dále přidáte kód toodefine a export objektu Task hello. Toto je zodpovědná za inicializaci objektu Task a nastavení hello databáze a kolekce dokumentů budeme používat.
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. Dále přidejte následující kód toodefine další metody na objektu Task hello hello ty umožňují pracovat s daty uloženými v databázi Cosmos Azure.
   
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
9. Uložte a zavřete hello **taskDao.js** souboru. 

### <a name="create-hello-controller"></a>Vytvořit řadič hello
1. V hello **trasy** adresáři projektu vytvořte nový soubor s názvem **tasklist.js**. 
2. Přidejte následující kód příliš hello**tasklist.js**. Tento kód načte hello moduly DocumentDBClient a async, které jsou používány **tasklist.js**. To také definovat hello **TaskList** funkce, která se předá instance hello **úloh** definovaného dříve:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. Pokračovat v přidávání toohello **tasklist.js** souboru přidáním hello metody použité příliš**showTasks, addTask**, a **Completetask**:
   
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
4. Uložte a zavřete hello **tasklist.js** souboru.

### <a name="add-configjs"></a>Přidání souboru config.js
1. V adresáři projektu vytvořte nový soubor s názvem **config.js**.
2. Přidejte následující hello příliš**config.js**. Tento kód definuje nastavení konfigurace a hodnoty, které aplikace potřebuje.
   
        var config = {}
   
        config.host = process.env.HOST || "[hello URI value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[hello PRIMARY KEY value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. V hello **config.js** souboru aktualizace hello hodnoty HOST a AUTH_KEY hello hodnoty zjištěné v okně hello klíčů účtu Azure Cosmos DB na hello [portálu Microsoft Azure](https://portal.azure.com).
4. Uložte a zavřete hello **config.js** souboru.

### <a name="modify-appjs"></a>Úprava souboru app.js
1. V adresáři projektu hello, otevřete hello **app.js** souboru. Tento soubor byl vytvořen dříve při vytvoření webové aplikace Express hello.
2. Přidejte následující kód toohello horní části hello **app.js**
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. Tento kód definuje hello konfigurační soubor toobe používá a pokračuje hodnoty tooread mimo tento soubor do některé proměnné, které brzy využijeme.
4. Nahraďte hello následující dva řádky v **app.js** souboru:
   
        app.use('/', index);
        app.use('/users', users); 
   
      s hello následující fragment kódu:
   
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
5. Tyto řádky definují novou instanci třídy Naše **TaskDao** objekt se nové připojení tooAzure Cosmos DB (pomocí hodnot hello číst z hello **config.js**), inicializace hello objekt úkolů a svážou akce formuláře toomethods na našem **TaskList** řadiče. 
6. Nakonec uložte a zavřete hello **app.js** souboru, jsme téměř hotovi.

## <a name="_Toc395783181"></a>Krok 5: Vytvoření uživatelského rozhraní
Nyní přejdeme naše pozornost toobuilding hello uživatelské rozhraní, uživatel mohl ve skutečnosti spolupracovat s naší aplikace. Hello aplikace Express jsme vytvořili používá **Jade** jako zobrazovací modul hello. Další informace o Jade najdete příliš[http://jade-lang.com/](http://jade-lang.com/).

1. Hello **layout.jade** souboru v hello **zobrazení** directory se používá jako globální šablona pro ostatní **.jade** soubory. V tomto kroku jej upravíte toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), což je sada nástrojů, který umožňuje snadno toodesign pěkných webů. 
2. Otevřete hello **layout.jade** soubor nachází ve hello **zobrazení** hello obsah složky a nahradit s hello následující:

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

    Tato hodnota informuje efektivně hello **Jade** modul toorender některý kód HTML pro naši aplikaci a vytvoří **bloku** názvem **obsah** kterého můžeme zadat rozložení hello pro náš obsah stránky.

    Uložte a zavřete tento soubor **layout.jade**.

3. Nyní otevřete hello **index.jade** soubor, hello zobrazení, která se použije v naší aplikaci a nahraďte hello obsah souboru hello hello následující:
   
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
   

Tento kód rozšiřuje rozložení a poskytuje obsah pro hello **obsah** zástupný symbol jsme viděli v hello **layout.jade** výše.
   
V tomto rozložení jsme vytvořili dva formuláře HTML.

Hello první formulář obsahuje tabulku našich dat a tlačítko, které umožňuje nám tooupdate položky zveřejněním příliš**/completetask** metodě kontroleru.
    
Hello druhý formulář obsahuje dvě vstupní pole a tlačítko, které umožňuje nám toocreate novou položku zveřejněním příliš**/addtask** metodě kontroleru.

To by mělo být vše, co potřebujeme ke toowork naše aplikace.

## <a name="_Toc395783181"></a>Krok 6: Místní spuštění aplikace
1. aplikace hello tootest na místním počítači, spusťte `npm start` v terminálu toostart hello vaší aplikace a pak aktualizujte vaše [http://localhost: 3000](http://localhost:3000) stránku prohlížeče. stránku Hello by teď měl vypadat jako následující obrázek hello:
   
    ![Snímek obrazovky aplikace seznam úkolů v okně prohlížeče hello](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > Pokud obdržíte chybu o hello odsazení v souboru layout.jade hello nebo hello index.jade, ujistěte se, že první dva řádky hello v oba soubory je ponecháno zarovnán do bloku, bez mezer. Pokud existují prostory před hello první dva řádky, je odstranit, uložit oba soubory a pak aktualizujte okno prohlížeče. 

2. Použít hello položky, názvů a kategorie položek pole tooenter novou úlohu a potom klikněte na **přidat položku**. Tak se ve službě Azure Cosmos DB vytvoří dokument s těmito vlastnostmi. 
3. stránku Hello by měl aktualizovat toodisplay hello nově vytvořený položky v seznamu úkolů hello.
   
    ![Snímek obrazovky aplikace hello s novou položkou v seznamu úkolů hello](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. toocomplete úlohy, jednoduše zaškrtněte políčko hello ve sloupci hello dokončení a potom klikněte na **aktualizace úloh**. Tento dokument hello aktualizace jste již vytvořili.

5. aplikace hello toostop, stiskněte kombinaci kláves CTRL + C v hello okno terminálu a pak klikněte na **Y** tooterminate hello dávkovou úlohu.

## <a name="_Toc395783182"></a>Krok 7: Nasazení vaší aplikace tooAzure projektu vývoj webů
1. Pokud jste tak ještě neučinili, povolte úložiště Git pro Weby Azure. Pokyny naleznete v tom toodo v hello [místní nasazení Git tooAzure služby App Service](../app-service-web/app-service-deploy-local-git.md) tématu.
2. Přidejte svůj web Azure jako vzdálené úložiště Git.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Nasazení provedete odesláním toohello vzdálené.
   
        git push azure master
4. Během pár sekund bude git dokončí publikování webové aplikace a spustí prohlížeč, kde uvidíte vaše handiwork běžící v Azure!

    Blahopřejeme! Máte právě vytvořené vaše první webovou aplikaci Node.js Express pomocí Azure Cosmos DB a publikovali jste ji tooAzure weby.

    Chcete-li toodownload nebo odkazovat toohello úplné referenční aplikace pro tento kurz, ho můžete stáhnout z [Githubu][GitHub].

## <a name="_Toc395637775"></a>Další kroky

* Chcete tooperform škálování a výkon testování pomocí Azure Cosmos DB? Viz [Testování výkonu a škálování pomocí služby Azure Cosmos DB](performance-testing.md).
* Zjistěte, jak příliš[monitorovat účet Azure Cosmos DB](monitor-accounts.md).
* Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).
* Prozkoumejte hello [dokumentace Azure Cosmos DB](https://docs.microsoft.com/azure/documentdb/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

