---
title: aaaWeb aplikace s table storage (Node.js) | Microsoft Docs
description: "Kurz, který je založený na hello webové aplikace s Express kurzu přidáním služby Azure Storage a hello Azure modulu."
services: cloud-services, storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: e90959a2-4cb2-4b19-9bfb-aede15b18b1c
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 4eba16f09f8b69cbc135d097e6ca71e08b33733c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-application-using-storage"></a>Webové aplikace Node.js pomocí úložiště
## <a name="overview"></a>Přehled
V tomto kurzu rozšíříte hello aplikace vytvořené v [webové aplikace Node.js pomocí Express] kurz pomocí hello Microsoft Azure klientské knihovny pro Node.js toowork se službami pro správu dat. Toocreate vaše aplikace bude rozšíření webových úloh list aplikace můžete nasadit tooAzure. Seznam úkolů Hello umožňuje uživateli načíst úlohy, přidat nové úkoly a úkoly označit jako dokončená.

položky úkolů Hello jsou uložené ve službě Azure Storage. Úložiště Azure poskytuje úložiště nestrukturovaných dat, které je odolné proti chybám a vysoce dostupné. Úložiště Azure obsahuje několik datové struktury, kam můžete ukládat a přistupovat k datům a mohou využívat služby úložiště hello z hello rozhraní API, které jsou součástí hello Azure SDK pro Node.js nebo přes rozhraní REST API. Další informace najdete v tématu [ukládání a přístup k datům v Azure].

V tomto kurzu se předpokládá, že jste dokončili hello [webové aplikace Node.js] a [Node.js s Express][webové aplikace Node.js pomocí Express] kurzy.

Co se dozvíte:

* Jak toowork s hello modulu Jade šablon
* Jak toowork službou pro správu dat na Azure

Zde je snímek obrazovky aplikace hello dokončena:

![Hello dokončit webové stránky v aplikaci internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Nastavení přihlašovacích údajů úložiště v souboru Web.Config.
tooaccess Azure Storage, musíte toopass úložiště pověření. toodo, využívat nastavení aplikace v souboru web.config.
Tato nastavení budou předány jako tooNode proměnné prostředí, které jsou pak přečte hello Azure SDK.

> [!NOTE]
> Přihlašovací údaje úložiště používají jenom při hello aplikace je nasazená tooAzure. Při spuštění v emulátoru hello, hello aplikace bude používat emulátor úložiště hello.
>
>

Proveďte následující kroky přihlašovacích údajů účtu úložiště tooretrieve hello hello a jejich přidání toohello nastavení v souboru web.config:

1. Pokud již není otevřený, spusťte z hello hello prostředí Azure PowerShell **spustit** nabídky rozšířením **všechny programy, Azure**, klikněte pravým tlačítkem na **prostředí Azure PowerShell**a potom vyberte  **Spustit jako správce**.
2. Změna adresáře toohello složky obsahující aplikaci. Například C:\\uzlu\\tasklist\\WebRole1.
3. Z hello okno Azure Powershell zadejte následující rutinu informace o účtu úložiště tooretrieve hello hello:

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   To umožňuje načíst seznam hello účty úložiště a účtu klíče přidruženého k vaší hostované služby.

   > [!NOTE]
   > Vzhledem k tomu, že hello Azure SDK vytvoří účet úložiště při nasazení služby, by měl z nasazení aplikace v příručkách předchozí hello již existuje účet úložiště.
   >
   >
4. Otevřete hello **ServiceDefinition.csdef** souboru, který obsahuje nastavení hello prostředí, které se použijí při hello aplikace je nasazená tooAzure:

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. Vložení hello následujícího bloku pod **prostředí** elementu, nahraďte {účet úložiště} a {přístupový klíč k ÚLOŽIŠTI} s názvem účtu hello a hello primární klíč pro účet úložiště hello chcete toouse pro nasazení:

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![obsah souboru web.cloud.config Hello](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6. Hello soubor uložte a zavřete poznámkový blok.

### <a name="install-additional-modules"></a>Instalace dalších modulů
1. Hello použijte následující příkaz tooinstall hello [azure], [uzlu uuid], [nconf] a [asynchronní] moduly místně, jak dobře jako toosave položku pro jejich toohello **package.json** souboru:

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  Hello výstup tohoto příkazu by měla vypadat podobně jako toohello následující:

  ```
  node-uuid@1.4.1 node_modules\node-uuid

  nconf@0.6.9 node_modules\nconf
  ├── ini@1.1.0
  ├── async@0.2.9
  └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

  azure-storage@0.1.0 node_modules\azure-storage
  ├── extend@1.2.1
  ├── xmlbuilder@0.4.3
  ├── mime@1.2.11
  ├── underscore@1.4.4
  ├── validator@3.1.0
  ├── node-uuid@1.4.1
  ├── xml2js@0.2.7 (sax@0.5.2)
  └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)
  ```

## <a name="using-hello-table-service-in-a-node-application"></a>Pomocí služby Table hello v aplikaci node
V této části rozšíříte hello základní aplikace vytvořené v hello **express** příkaz přidáním **task.js** soubor, který obsahuje hello model pro úkoly. Také upravíte existující hello **app.js** a vytvořte novou **tasklist.js** soubor, který používá hello model.

### <a name="create-hello-model"></a>Vytvoření modelu hello
1. V hello **WebRole1** adresáře, vytvořte nový adresář s názvem **modely**.
2. V hello **modely** adresáře, vytvořte nový soubor s názvem **task.js**. Tento soubor bude obsahovat hello model pro úkoly hello vytvořené v aplikaci.
3. Na začátku hello hello **task.js** soubor, přidejte následující kód tooreference požadované knihovny hello:

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. Dále přidáte kód toodefine a export objektu Task hello. Tento objekt je zodpovědná za připojení toohello tabulky.

    ```nodejs
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
    ```

5. Dál přidejte následující kód toodefine další metody na objektu Task hello hello ty umožňují pracovat s daty uloženými v tabulce hello:

    ```nodejs
    Task.prototype = {
      find: function(query, callback) {
        self = this;
        self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
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
    ```

6. Uložte a zavřete hello **task.js** souboru.

### <a name="create-hello-controller"></a>Vytvořit řadič hello
1. V hello **WebRole1 a směruje** adresáře, vytvořte nový soubor s názvem **tasklist.js** a otevřete v textovém editoru.
2. Přidejte následující kód příliš hello**tasklist.js**. Tento kód načte azure a asynchronních hello moduly, které jsou používány **tasklist.js**. Také definuje hello **TaskList** funkce, která se předá instance hello **úloh** definovaného dříve:

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. Pokračovat v přidávání toohello **tasklist.js** souboru přidáním hello metody použité příliš**showTasks**, **addTask**, a **Completetask**:

    ```nodejs
    TaskList.prototype = {
      showTasks: function(req, res) {
        self = this;
        var query = azure.TableQuery()
          .where('completed eq ?', false);
        self.task.find(query, function itemsFound(error, items) {
          res.render('index',{title: 'My ToDo List ', tasks: items});
        });
      },

      addTask: function(req,res) {
        var self = this
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
    ```

4. Uložit hello **tasklist.js** souboru.

### <a name="modify-appjs"></a>Úprava souboru app.js
1. V hello **WebRole1** adresář, otevřete hello **app.js** soubor v textovém editoru.
2. Od začátku hello hello souboru, přidejte následující modul hello azure tooload hello a nastavte klíč název a oddíl tabulky hello:

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. V souboru app.js hello, posuňte se dolů toowhere zobrazí hello následující řádek:

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    Nahraďte hello výše řádky kódu hello vidíte níže. To bude inicializovat instanci <strong>úloh</strong> s účtem úložiště tooyour připojení. To je předán toohello <strong>TaskList</strong>, který použije ji toocommunicate hello služby Table:

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. Uložit hello **app.js** souboru.

### <a name="modify-hello-index-view"></a>Upravit zobrazení indexu hello
1. Změna adresáře toohello **zobrazení** adresář a otevřete hello **index.jade** soubor v textovém editoru.
2. Nahraďte obsah hello hello **index.jade** soubor s hello kód níže. Definuje hello zobrazení pro zobrazení stávající úlohy a také formuláře pro přidání nové úlohy a označit stávajících jako dokončenou.

    ```
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
          if tasks != []
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
    ```

3. Uložte a zavřete **index.jade** souboru.

### <a name="modify-hello-global-layout"></a>Upravit globální rozložení hello
Hello **layout.jade** souboru v hello **zobrazení** directory se používá jako globální šablona pro ostatní **.jade** soubory. V tomto kroku jej upravíte toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), což je sada nástrojů, který umožňuje snadno toodesign pěkných webů.

1. Stažení a extrakci souborů hello [Twitter Bootstrap](http://getbootstrap.com/). Kopírování hello **bootstrap.min.css** soubor z hello **bootstrap\\dist\\šablon stylů css** složky toohello **veřejné\\předlohy se styly** adresář tasklist aplikace.
2. Z hello **zobrazení** složku, otevřete hello **layout.jade** ve vaší textového editoru a nahradit hello obsah s hello následující:

    DOCTYPE html html head title = název odkazu (relativní = 'šablony stylů', href='/stylesheets/bootstrap.min.css.) odkaz (relativní = 'šablony stylů', href='/stylesheets/style.css.) body.app nav.navbar.navbar výchozí záhlaví div.navbar a.navbar-brand(href='/') vlastní úkoly blokování obsahu

3. Uložit hello **layout.jade** souboru.

### <a name="running-hello-application-in-hello-emulator"></a>Spuštění hello aplikaci v emulátoru hello
Použijte následující příkaz toostart hello aplikaci v emulátoru hello hello.

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

Hello prohlížeči se otevře a zobrazí hello následující stránky:

![Webové stránkovaného fondu s názvem Moje seznam úkolů v tabulce obsahující pole a úlohy tooadd novou úlohu.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Použití položek tooadd hello formuláře nebo odebrat existující položky, můžete je označit jako dokončená.

## <a name="publishing-hello-application-tooazure"></a>Publikování aplikace tooAzure hello
V okně prostředí Windows PowerShell hello volání hello následující rutiny tooredeploy tooAzure vaší hostované služby.

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

Nahraďte **myuniquename** s jedinečný název pro tuto aplikaci. Nahraďte **datovém centru** s názvem hello datové centrum Azure, jako například **západní USA**.

Po dokončení nasazení hello byste měli vidět a odpovědi podobné toohello následující:

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist tooMicrosoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package toostorage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

Jako předtím protože zadaný hello **– spusťte** možnost hello otevře se prohlížeč a zobrazí vaši aplikaci běžící v Azure po dokončení publikování.

![Okno prohlížeče zobrazující stránku hello Moje seznamu úkolů. Hello URL značí, že stránku hello je teď hostované v Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Zastavení a odstranění vaší aplikace
Po nasazení aplikace, může být vhodné toodisable ji proto můžete vyhnout nákladům nebo sestavení a nasazení dalších aplikací v rámci hello volné zkušební období.

Azure účtuje poplatky za instance webových rolí podle času (v hodinách), který na serveru spotřebují.
Čas serveru se spotřebovává, jakmile vaše aplikace je nasazená, i když nejsou spuštěné instance a jsou ve stavu hello byla zastavena.

Hello následující kroky ukazují, jak toostop a odstranění vaší aplikace.

1. V okně prostředí Windows PowerShell hello zastavte hello nasazení služby vytvořené v předchozí části hello s hello následující rutiny:

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   Zastavování služby hello může trvat několik minut. Pokud je hello služba zastavená, obdržíte zprávu s upozorněním, že se zastavil.

2. toodelete hello služba, volání hello následující rutiny:

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   Po zobrazení výzvy zadejte **Y** toodelete hello služby.

   Odstraněním hello služby může trvat několik minut. Po odstranění hello služby obdržíte zprávu s upozorněním, že se odstranila služba hello.

[webové aplikace Node.js pomocí Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[ukládání a přístup k datům v Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[webové aplikace Node.js]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


