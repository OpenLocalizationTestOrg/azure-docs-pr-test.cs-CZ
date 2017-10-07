---
title: "aaaNode.js webovou aplikaci pomocí hello služby Azure Table"
description: "V tomto kurzu se naučíte, jak toouse hello Azure Table služby toostore data z aplikace Node.js, který je hostován v Azure App Service Web Apps."
tags: azure-portal
services: app-service\web, storage
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 029e6f46-f586-4309-adbf-71c7b8d537d4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: f6e08335b4c7f62f7b3994287edd586860cb7135
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-app-using-hello-azure-table-service"></a>Webové aplikace Node.js pomocí hello služby Azure Table
## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak služby Table toouse poskytované data toostore a přístup k Azure Data správy od [uzlu] aplikace hostována v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace. V tomto kurzu se předpokládá, že máte zkušenosti s pomocí uzlu a [Git].

Co se dozvíte:

* Jak toouse npm (uzel Správce balíčků) tooinstall hello uzlu moduly
* Jak toowork s hello služby Azure Table
* Jak toouse hello rozhraní příkazového řádku Azure toocreate webové aplikace.

Podle tohoto kurzu vytvoříte jednoduchou webovou aplikaci "seznam úkolů", která umožňuje vytváření, získávat a dokončovat úkoly. Hello úlohy jsou uloženy v hello služby Table.

Zde je hello dokončit aplikace:

![Webová stránka zobrazující prázdný tasklist][node-table-finished]

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="prerequisites"></a>Požadavky
Než budete postupovat hello pokyny v tomto článku, zajistěte, abyste měli hello nainstalované tyto položky:

* [uzlu] verze 0.10.24 nebo vyšší
* [Git]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
Vytvoření účtu úložiště Azure Hello aplikace bude používat položkami seznamu úkolů hello toostore tento účet.

1. Přihlaste se k hello [portálu Azure](https://portal.azure.com/).
2. Klikněte na tlačítko hello **nový** ikonu na hello spodní levé hello portál, pak klikněte na tlačítko **Data + úložiště** > **úložiště**. Zadejte jedinečný název účtu úložiště hello a vytvořte novou [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) pro ni.
   
      ![Tlačítko Nový](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)
   
    Po vytvoření účtu úložiště hello hello **oznámení** tlačítko bude flash zelená **úspěch** a se otevře okno účtu úložiště hello tooshow patří toohello nový prostředek skupiny vytvořit.
3. V okně účtu úložiště hello, klikněte na tlačítko **nastavení** > **klíče**. Kopírování hello primární přístupový klíč toohello schránky.
   
    ![Přístupový klíč][portal-storage-access-keys]

## <a name="install-modules-and-generate-scaffolding"></a>Instalace modulů a generovat generování uživatelského rozhraní
V této části vytvoříte novou aplikaci uzlu a npm tooadd modulu balíčky. Pro tuto aplikaci použijete hello [Express] a [Azure] moduly. modul expresního Hello poskytuje Model View Controller rozhraní pro uzel, při hello moduly Azure poskytuje služby Table toohello připojení.

### <a name="install-express-and-generate-scaffolding"></a>Expresní instalace a generovat generování uživatelského rozhraní
1. Z příkazového řádku hello, vytvořte nový adresář s názvem **tasklist** a directory toothat přepínače.  
2. Zadejte následující příkaz tooinstall hello Express modulu hello.
   
        npm install express-generator@4.2.0 -g
   
    V závislosti na hello operační systém budete pravděpodobně potřebovat tooput příkazu "sudo" před hello příkaz:
   
        sudo npm install express-generator@4.2.0 -g
   
    Zobrazí výstup Hello podobné toohello následující ukázka:
   
        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)
   
   > [!NOTE]
   > Hello "-g, parametr globálně nainstaluje modul hello. Tímto způsobem, můžeme použít **express** toogenerate webové aplikace generování bez nutnosti tootype v další informace o cestě.
   > 
   > 
3. toocreate hello generování uživatelského rozhraní pro aplikace hello zadejte hello **express** příkaz:
   
        express
   
    Hello výstup tohoto příkazu se zobrazí podobné toohello následující ukázka:
   
           create : .
           create : ./package.json
           create : ./app.js
           create : ./public
           create : ./public/images
           create : ./routes
           create : ./routes/index.js
           create : ./routes/users.js
           create : ./public/stylesheets
           create : ./public/stylesheets/style.css
           create : ./views
           create : ./views/index.jade
           create : ./views/layout.jade
           create : ./views/error.jade
           create : ./public/javascripts
           create : ./bin
           create : ./bin/www
   
           install dependencies:
             $ cd . && npm install
   
           run hello app:
             $ DEBUG=my-application ./bin/www
   
    Nyní máte několik nových adresářů a souborů v hello **tasklist** adresáře.

### <a name="install-additional-modules"></a>Instalace dalších modulů
Jeden z hello soubory, které **express** vytvoří je **package.json**. Tento soubor obsahuje seznam modulu závislosti. Později když nasazujete aplikace tooApp hello Service Web Apps, tento soubor Určuje, které moduly musí toobe nainstalován v Azure.

Z hello příkazového řádku, zadejte následující příkaz tooinstall hello moduly popsané v hello hello **package.json** souboru. Může být nutné toouse "sudo".

    npm install

Hello výstup tohoto příkazu se zobrazí podobné toohello následující ukázka:

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


Potom zadejte následující příkaz tooinstall hello hello [azure], [uzlu uuid], [nconf] a [asynchronní] moduly:

    npm install azure-storage node-uuid async nconf --save

Hello **– Uložit** příznak přidává položky pro tyto moduly toohello **package.json** souboru.

Hello výstup tohoto příkazu se zobrazí podobné toohello následující ukázka:

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-hello-application"></a>Vytvoření aplikace hello
Nyní jsme připravené toobuild hello aplikace.

### <a name="create-a-model"></a>Vytvoření modelu
A *modelu* je objekt, který představuje data hello ve vaší aplikaci. Pro aplikace hello pouze model hello je objekt úlohy, která představuje položku v seznamu úkolů hello. Úlohy budou mít hello následující pole:

* Klíč oddílu
* RowKey
* název (string)
* kategorie (string)
* dokončené (Boolean)

**PartitionKey** a **RowKey** používají hello služby Table jako klíče tabulky. Další informace najdete v tématu [Principy hello služby Table datový model](https://msdn.microsoft.com/library/azure/dd179338.aspx).

1. V hello **tasklist** adresáře, vytvořte nový adresář s názvem **modely**.
2. V hello **modely** adresáře, vytvořte nový soubor s názvem **task.js**. Tento soubor bude obsahovat hello model pro úkoly hello vytvořené v aplikaci.
3. Na začátku hello hello **task.js** soubor, přidejte následující kód tooreference požadované knihovny hello:
   
        var azure = require('azure-storage');
          var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;
4. Přidejte následující hello kód toodefine a export objektu Task hello. Tento objekt je zodpovědná za připojení toohello tabulky.
   
          module.exports = Task;
   
        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };
5. Přidejte následující kód toodefine další metody na objektu Task hello hello ty umožňují pracovat s daty uloženými v tabulce hello:
   
        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(this.tableName, query, null, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },
   
          addItem: function(item, callback) {
            self = this;
            // use entityGenerator tooset types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };
            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },
   
          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }
6. Uložte a zavřete hello **task.js** souboru.

### <a name="create-a-controller"></a>Vytvořit řadič
A *řadič* zpracovává požadavky HTTP a vykreslí hello HTML odpovědi.

1. V hello **tasklist a směruje** adresáře, vytvořte nový soubor s názvem **tasklist.js** a otevřete v textovém editoru.
2. Přidejte následující kód příliš hello**tasklist.js**. Tento kód načte azure a asynchronních hello moduly, které jsou používány **tasklist.js**. Také definuje hello **TaskList** funkce, která se předá instance hello **úloh** definovaného dříve:
   
        var azure = require('azure-storage');
        var async = require('async');
   
        module.exports = TaskList;
3. Definování **TaskList** objektu.
   
        function TaskList(task) {
          this.task = task;
        }
4. Přidejte následující metody příliš hello**TaskList**:
   
        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = new azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },
   
          addTask: function(req,res) {
            var self = this;
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },
   
          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

### <a name="modify-appjs"></a>Úprava souboru app.js
1. Z hello **tasklist** adresář, otevřete hello **app.js** souboru. Tento soubor byl vytvořen dříve spuštěním hello **express** příkaz.
2. Od začátku hello hello souboru, přidejte následující tooload hello azure modulu, název tabulky hello sady, klíč oddílu a nastavení hello úložiště přihlašovacích údajů používaných v tomto příkladu hello:
   
        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");
   
   > [!NOTE]
   > nconf načte hodnoty konfigurace hello z proměnné prostředí nebo hello **config.json** souboru, který vytvoříme později.
   > 
   > 
3. V souboru app.js hello, posuňte se dolů toowhere zobrazí hello následující řádek:
   
        app.use('/', routes);
        app.use('/users', users);
   
    Nahraďte hello výše řádky kódu hello vidíte níže. To bude inicializovat instanci <strong>úloh</strong> s účtem úložiště tooyour připojení. To je předán toohello <strong>TaskList</strong>, který použije ji toocommunicate hello služby Table:
   
        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
4. Uložit hello **app.js** souboru.

### <a name="modify-hello-index-view"></a>Upravit zobrazení indexu hello
1. Otevřete hello **tasklist/views/index.jade** soubor v textovém editoru.
2. Nahraďte hello celý obsah souboru hello hello následující kód. Definuje zobrazení zobrazí existující úlohy, která obsahuje formulář pro přidání nové úlohy a označit stávajících jako dokončenou.
   
        extends layout
   
        block content
          h1= title
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
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name:
            input(name="item[name]", type="textbox")
            label Item Category:
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item
3. Uložte a zavřete **index.jade** souboru.

### <a name="modify-hello-global-layout"></a>Upravit globální rozložení hello
Hello **layout.jade** souboru v hello **zobrazení** adresář je globální šablona pro ostatní **.jade** soubory. V tomto kroku jej upravíte toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), což je sada nástrojů, který umožňuje snadno toodesign dobrý vypadající webové aplikace.

Stažení a extrakci souborů hello [Twitter Bootstrap](http://getbootstrap.com/). Kopírování hello **bootstrap.min.css** soubor z hello Bootstrap **šablon stylů css** složky do hello **veřejné nebo předlohy se styly** adresář vaší aplikace.

Z hello **zobrazení** složku, otevřete **layout.jade** a nahraďte celý obsah hello hello následující:

    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body.app
        nav.navbar.navbar-default
          div.navbar-header
          a.navbar-brand(href='/') My Tasks
        block content

### <a name="create-a-config-file"></a>Vytvoření konfiguračního souboru
toorun hello aplikaci místně do konfiguračního souboru jsme budete uveďte přihlašovací údaje Azure Storage. Vytvořte soubor s názvem **config.json* * s hello následující JSON:

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Nahraďte **název účtu úložiště** s názvem hello hello úložiště účet jste dříve vytvořili a nahraďte **přístupový klíč k úložišti** s hello primární přístupový klíč účtu úložiště. Například:

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

Uložte tento soubor *jeden adresář úroveň vyšší* než hello **tasklist** adresáři, například takto:

    parent/
      |-- config.json
      |-- tasklist/

Hello Důvodem tohoto postupu je tooavoid kontrola hello konfiguračního souboru do zdrojového kódu, kde mohou být veřejné. Když jsme nasadit tooAzure aplikace hello, budeme používat proměnné prostředí místo souboru config.

## <a name="run-hello-application-locally"></a>Místní spuštění aplikace hello
aplikace hello tootest na místním počítači, proveďte následující kroky hello:

1. Z příkazového řádku hello, změňte adresáře toohello **tasklist** adresáře.
2. Použijte následující příkaz toolaunch hello aplikace místně hello:
   
        npm start
3. Otevřete webový prohlížeč a přejděte toohttp://127.0.0.1:3000.
   
    Zobrazí se webová stránka podobné toohello následující ukázka.
   
    ![Prázdný tasklist zobrazení webové stránky][node-table-finished]
4. Zadejte název a kategorie toocreate novou položku seznamu úkolů a klikněte na tlačítko **přidat položku**. 
5. toomark úlohu jako dokončení kontroly **Complete** a klikněte na tlačítko **úlohy aktualizace**.
   
    ![Obrázek hello novou položku v seznamu hello úloh][node-table-list-items]

I když hello aplikace běží místně, je při ukládání dat hello v hello služby Azure Table.

## <a name="deploy-your-application-tooazure"></a>Nasazení vaší aplikace tooAzure
Hello kroky v této části použijte nástroje příkazového řádku Azure toocreate hello novou webovou aplikaci ve službě App Service a potom pomocí Git toodeploy vaší aplikace. tooperform tyto kroky musíte mít předplatné Azure.

> [!NOTE]
> Tyto kroky provádět také pomocí hello [portálu Azure](https://portal.azure.com/). V tématu [sestavení a nasazení webové aplikace Node.js ve službě Azure App Service].
> 
> Pokud je to hello první webové aplikace, které jste vytvořili, musíte použít portál Azure toodeploy hello této aplikace.
> 
> 

tooget spustili, nainstalujte hello [rozhraní příkazového řádku Azure] tak, že zadáte následující příkaz z příkazového řádku hello hello:

    npm install azure-cli -g

### <a name="import-publishing-settings"></a>Import nastavení publikování
V tomto kroku si stáhnete soubor obsahující informace o vašem předplatném.

1. Zadejte hello následující příkaz:
   
        azure login
   
    Tento příkaz spustí prohlížeč a přejde toohello stránky pro stažení. Pokud budete vyzváni, přihlaste se pomocí hello účet spojený s předplatným Azure.
   
    <!-- ![hello download page][download-publishing-settings] -->
   
    Stažení souboru Hello začne automaticky. Pokud není, můžete kliknout na odkaz hello od začátku hello hello stránky toomanually stažení hello souboru. Hello souboru Poznámka hello cesta k souboru a uložte.
2. Zadejte následující příkaz tooimport hello nastavení hello:
   
        azure account import <path-to-file>
   
    Zadejte název a cesta k souboru hello Dobrý den publikování souboru nastavení, které jste stáhli v předchozím kroku hello.
3. Po importu hello nastavení, odstranit hello soubor nastavení publikování. Je již nejsou potřebné a obsahuje citlivé informace týkající se vašeho předplatného Azure.

### <a name="create-an-app-service-web-app"></a>Vytvoření webové aplikace služby App Service
1. Z příkazového řádku hello, změňte adresáře toohello **tasklist** adresáře.
2. Použijte následující příkaz toocreate nové webové aplikace hello.
   
        azure site create --git
   
    Zobrazí se výzva pro název webové aplikace hello a umístění. Zadejte jedinečný název a vyberte hello stejné geografické oblasti jako váš účet úložiště Azure.
   
    Hello `--git` parametr vytvoří úložiště Git v Azure pro tuto webovou aplikaci. Pokud žádný neexistuje a přidá také inicializuje úložiště Git v aktuálním adresáři hello [Git vzdálené] s názvem "azure", což je použité toopublish hello aplikace tooAzure. Nakonec se vytvoří **web.config** soubor, který obsahuje nastavení používaná aplikací Azure toohost uzlu. Pokud vynecháte hello `--git` parametr ale hello adresář obsahuje úložiště Git, hello příkaz bude stále vytvořit vzdálené hello "azure".
   
    Po dokončení tohoto příkazu, zobrazí se výstup podobný toohello následující. Všimněte si, že hello řádek začínající **web je vytvořen v** obsahuje hello URL pro webovou aplikaci hello.
   
        info:   Executing command site create
        help:   Need a site name
        Name: TableTasklist
        info:   Using location southcentraluswebspace
        info:   Executing `git init`
        info:   Creating default .gitignore file
        info:   Creating a new web site
        info:   Created web site at  tabletasklist.azurewebsites.net
        info:   Initializing repository
        info:   Repository initialized
        info:   Executing `git remote add azure https://username@tabletasklist.azurewebsites.net/TableTasklist.git`
        info:   site create command OK
   
   > [!NOTE]
   > Pokud je to hello první webové aplikace App Service pro vaše předplatné, bude pokyn toouse hello portálu Azure toocreate hello webové aplikace. Další informace najdete v tématu [sestavení a nasazení webové aplikace Node.js ve službě Azure App Service].
   > 
   > 

### <a name="set-environment-variables"></a>Proměnné prostředí sady
V tomto kroku přidáte konfiguraci prostředí proměnné tooyour webové aplikace v Azure.
Hello příkazovém řádku zadejte následující hello:

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


Nahraďte  **<storage account name>**  s názvem hello hello úložiště účet jste dříve vytvořili a nahraďte  **<storage access key>**  s hello primární přístupový klíč účtu úložiště. (Použít hello stejné hodnoty jako hello config.json soubor, který jste vytvořili dříve.)

Alternativně můžete nastavit proměnné prostředí v hello [portálu Azure](https://portal.azure.com/):

1. Otevřete okno hello webovou aplikaci kliknutím **Procházet** > **webové aplikace** > název vaší webové aplikace.
2. V okně vaší webové aplikace, klikněte na tlačítko **všechna nastavení** > **nastavení aplikace**.
   
     <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->
3. Projděte dolů toohello **nastavení aplikace** a přidejte hello páry klíč/hodnota.
   
     ![Nastavení aplikace](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)
4. Klikněte na **ULOŽIT**.

### <a name="publish-hello-application"></a>Publikování aplikace hello
aplikace hello toopublish, potvrďte hello kód soubory tooGit a potom odešlete tooazure/hlavní.

1. Nastavte přihlašovací údaje nasazení.
   
        azure site deployment user set <name> <password>
2. Přidejte a potvrďte soubory aplikace.
   
        git add .
        git commit -m "adding files"
3. Push hello potvrzení toohello webové aplikace App Service:
   
        git push azure master
   
    Použití **hlavní** jako hello cílovou větev. Na konci hello hello nasazení zobrazí příkaz podobné toohello následující ukázka:
   
        toohttps://username@tabletasklist.azurewebsites.net/TableTasklist.git
          * [new branch]      master -> master
4. Po dokončení operace push hello procházet toohello adresa URL webové aplikace dříve vrácený hello `azure create site` příkaz tooview vaší aplikace.

## <a name="next-steps"></a>Další kroky
Při hello kroky v tomto článku popisují, pomocí informací o toostore hello služby Table, můžete také použít [MongoDB](https://mlab.com/azure/). 

## <a name="additional-resources"></a>Další zdroje
[rozhraní příkazového řádku Azure]

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URLs -->

[sestavení a nasazení webové aplikace Node.js ve službě Azure App Service]: app-service-web-get-started-nodejs.md
[Azure Developer Center]: /develop/nodejs/

[uzlu]: http://nodejs.org
[Git]: http://git-scm.com
[Express]: http://expressjs.com
[for free]: http://windowsazure.com
[Git vzdálené]: http://git-scm.com/docs/git-remote

[rozhraní příkazového řádku Azure]:../cli-install-nodejs.md

[azure]: https://github.com/Azure/azure-sdk-for-node
[uzlu uuid]: https://www.npmjs.com/package/node-uuid
[nconf]: https://www.npmjs.com/package/nconf
[asynchronní]: https://www.npmjs.com/package/async

[Azure Portal]: https://portal.azure.com

[Create and deploy a Node.js application tooan Azure Web Site]: app-service-web-get-started-nodejs.md

<!-- Image References -->

[node-table-finished]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_empty.png
[node-table-list-items]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_list.png
[download-publishing-settings]: ./media/storage-nodejs-use-table-storage-web-site/azure-account-download-cli.png
[portal-storage-access-keys]: ./media/storage-nodejs-use-table-storage-web-site/manage-access-keys.png
[app-settings]: ./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png
