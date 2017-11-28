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
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="497b0-103">Webové aplikace Node.js pomocí úložiště</span><span class="sxs-lookup"><span data-stu-id="497b0-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="497b0-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="497b0-104">Overview</span></span>
<span data-ttu-id="497b0-105">V tomto kurzu rozšíříte hello aplikace vytvořené v [webové aplikace Node.js pomocí Express] kurz pomocí hello Microsoft Azure klientské knihovny pro Node.js toowork se službami pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="497b0-105">In this tutorial, you will extend hello application created in the [Node.js Web Application using Express] tutorial by using hello Microsoft Azure Client Libraries for Node.js toowork with data management services.</span></span> <span data-ttu-id="497b0-106">Toocreate vaše aplikace bude rozšíření webových úloh list aplikace můžete nasadit tooAzure.</span><span class="sxs-lookup"><span data-stu-id="497b0-106">You will extend your application toocreate a web-based task-list application that you can deploy tooAzure.</span></span> <span data-ttu-id="497b0-107">Seznam úkolů Hello umožňuje uživateli načíst úlohy, přidat nové úkoly a úkoly označit jako dokončená.</span><span class="sxs-lookup"><span data-stu-id="497b0-107">hello task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="497b0-108">položky úkolů Hello jsou uložené ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="497b0-108">hello task items are stored in Azure Storage.</span></span> <span data-ttu-id="497b0-109">Úložiště Azure poskytuje úložiště nestrukturovaných dat, které je odolné proti chybám a vysoce dostupné.</span><span class="sxs-lookup"><span data-stu-id="497b0-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="497b0-110">Úložiště Azure obsahuje několik datové struktury, kam můžete ukládat a přistupovat k datům a mohou využívat služby úložiště hello z hello rozhraní API, které jsou součástí hello Azure SDK pro Node.js nebo přes rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="497b0-110">Azure Storage includes several data structures where you can store and access data, and you can leverage hello storage services from hello APIs included in hello Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="497b0-111">Další informace najdete v tématu [ukládání a přístup k datům v Azure].</span><span class="sxs-lookup"><span data-stu-id="497b0-111">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="497b0-112">V tomto kurzu se předpokládá, že jste dokončili hello [webové aplikace Node.js] a [Node.js s Express][webové aplikace Node.js pomocí Express] kurzy.</span><span class="sxs-lookup"><span data-stu-id="497b0-112">This tutorial assumes that you have completed hello [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="497b0-113">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="497b0-113">You will learn:</span></span>

* <span data-ttu-id="497b0-114">Jak toowork s hello modulu Jade šablon</span><span class="sxs-lookup"><span data-stu-id="497b0-114">How toowork with hello Jade template engine</span></span>
* <span data-ttu-id="497b0-115">Jak toowork službou pro správu dat na Azure</span><span class="sxs-lookup"><span data-stu-id="497b0-115">How toowork with Azure Data Management services</span></span>

<span data-ttu-id="497b0-116">Zde je snímek obrazovky aplikace hello dokončena:</span><span class="sxs-lookup"><span data-stu-id="497b0-116">A screenshot of hello completed application is below:</span></span>

![Hello dokončit webové stránky v aplikaci internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="497b0-118">Nastavení přihlašovacích údajů úložiště v souboru Web.Config.</span><span class="sxs-lookup"><span data-stu-id="497b0-118">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="497b0-119">tooaccess Azure Storage, musíte toopass úložiště pověření.</span><span class="sxs-lookup"><span data-stu-id="497b0-119">tooaccess Azure Storage, you need toopass in storage credentials.</span></span> <span data-ttu-id="497b0-120">toodo, využívat nastavení aplikace v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="497b0-120">toodo this, you utilize web.config application settings.</span></span>
<span data-ttu-id="497b0-121">Tato nastavení budou předány jako tooNode proměnné prostředí, které jsou pak přečte hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="497b0-121">Those settings will be passed as environment variables tooNode, which are then read by hello Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="497b0-122">Přihlašovací údaje úložiště používají jenom při hello aplikace je nasazená tooAzure.</span><span class="sxs-lookup"><span data-stu-id="497b0-122">Storage credentials are only used when hello application is deployed tooAzure.</span></span> <span data-ttu-id="497b0-123">Při spuštění v emulátoru hello, hello aplikace bude používat emulátor úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="497b0-123">When running in hello emulator, hello application will use hello storage emulator.</span></span>
>
>

<span data-ttu-id="497b0-124">Proveďte následující kroky přihlašovacích údajů účtu úložiště tooretrieve hello hello a jejich přidání toohello nastavení v souboru web.config:</span><span class="sxs-lookup"><span data-stu-id="497b0-124">Perform hello following steps tooretrieve hello storage account credentials and add them toohello web.config settings:</span></span>

1. <span data-ttu-id="497b0-125">Pokud již není otevřený, spusťte z hello hello prostředí Azure PowerShell **spustit** nabídky rozšířením **všechny programy, Azure**, klikněte pravým tlačítkem na **prostředí Azure PowerShell**a potom vyberte  **Spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="497b0-125">If it is not already open, start hello Azure PowerShell from hello **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="497b0-126">Změna adresáře toohello složky obsahující aplikaci.</span><span class="sxs-lookup"><span data-stu-id="497b0-126">Change directories toohello folder containing your application.</span></span> <span data-ttu-id="497b0-127">Například C:\\uzlu\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="497b0-127">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="497b0-128">Z hello okno Azure Powershell zadejte následující rutinu informace o účtu úložiště tooretrieve hello hello:</span><span class="sxs-lookup"><span data-stu-id="497b0-128">From hello Azure Powershell window enter hello following cmdlet tooretrieve hello storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="497b0-129">To umožňuje načíst seznam hello účty úložiště a účtu klíče přidruženého k vaší hostované služby.</span><span class="sxs-lookup"><span data-stu-id="497b0-129">This retrieves hello list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="497b0-130">Vzhledem k tomu, že hello Azure SDK vytvoří účet úložiště při nasazení služby, by měl z nasazení aplikace v příručkách předchozí hello již existuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="497b0-130">Since hello Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in hello previous guides.</span></span>
   >
   >
4. <span data-ttu-id="497b0-131">Otevřete hello **ServiceDefinition.csdef** souboru, který obsahuje nastavení hello prostředí, které se použijí při hello aplikace je nasazená tooAzure:</span><span class="sxs-lookup"><span data-stu-id="497b0-131">Open hello **ServiceDefinition.csdef** file containing hello environment settings that are used when hello application is deployed tooAzure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="497b0-132">Vložení hello následujícího bloku pod **prostředí** elementu, nahraďte {účet úložiště} a {přístupový klíč k ÚLOŽIŠTI} s názvem účtu hello a hello primární klíč pro účet úložiště hello chcete toouse pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="497b0-132">Insert hello following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with hello account name and hello primary key for hello storage account you want toouse for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![obsah souboru web.cloud.config Hello](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6. <span data-ttu-id="497b0-134">Hello soubor uložte a zavřete poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="497b0-134">Save hello file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="497b0-135">Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="497b0-135">Install additional modules</span></span>
1. <span data-ttu-id="497b0-136">Hello použijte následující příkaz tooinstall hello [azure], [uzlu uuid], [nconf] a [asynchronní] moduly místně, jak dobře jako toosave položku pro jejich toohello **package.json** souboru:</span><span class="sxs-lookup"><span data-stu-id="497b0-136">Use hello following command tooinstall hello [azure], [node-uuid], [nconf] and [async] modules locally as well as toosave an entry for them toohello **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="497b0-137">Hello výstup tohoto příkazu by měla vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="497b0-137">hello output of this command should appear similar toohello following:</span></span>

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

## <a name="using-hello-table-service-in-a-node-application"></a><span data-ttu-id="497b0-138">Pomocí služby Table hello v aplikaci node</span><span class="sxs-lookup"><span data-stu-id="497b0-138">Using hello Table service in a node application</span></span>
<span data-ttu-id="497b0-139">V této části rozšíříte hello základní aplikace vytvořené v hello **express** příkaz přidáním **task.js** soubor, který obsahuje hello model pro úkoly.</span><span class="sxs-lookup"><span data-stu-id="497b0-139">In this section you will extend hello basic application created by hello **express** command by adding a **task.js** file which contains hello model for your tasks.</span></span> <span data-ttu-id="497b0-140">Také upravíte existující hello **app.js** a vytvořte novou **tasklist.js** soubor, který používá hello model.</span><span class="sxs-lookup"><span data-stu-id="497b0-140">You will also modify hello existing **app.js** and create a new **tasklist.js** file that uses hello model.</span></span>

### <a name="create-hello-model"></a><span data-ttu-id="497b0-141">Vytvoření modelu hello</span><span class="sxs-lookup"><span data-stu-id="497b0-141">Create hello model</span></span>
1. <span data-ttu-id="497b0-142">V hello **WebRole1** adresáře, vytvořte nový adresář s názvem **modely**.</span><span class="sxs-lookup"><span data-stu-id="497b0-142">In hello **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="497b0-143">V hello **modely** adresáře, vytvořte nový soubor s názvem **task.js**.</span><span class="sxs-lookup"><span data-stu-id="497b0-143">In hello **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="497b0-144">Tento soubor bude obsahovat hello model pro úkoly hello vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="497b0-144">This file will contain hello model for hello tasks created by your application.</span></span>
3. <span data-ttu-id="497b0-145">Na začátku hello hello **task.js** soubor, přidejte následující kód tooreference požadované knihovny hello:</span><span class="sxs-lookup"><span data-stu-id="497b0-145">At hello beginning of hello **task.js** file, add hello following code tooreference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="497b0-146">Dále přidáte kód toodefine a export objektu Task hello.</span><span class="sxs-lookup"><span data-stu-id="497b0-146">Next, you will add code toodefine and export hello Task object.</span></span> <span data-ttu-id="497b0-147">Tento objekt je zodpovědná za připojení toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="497b0-147">This object is responsible for connecting toohello table.</span></span>

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

5. <span data-ttu-id="497b0-148">Dál přidejte následující kód toodefine další metody na objektu Task hello hello ty umožňují pracovat s daty uloženými v tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="497b0-148">Next, add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in hello table:</span></span>

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

6. <span data-ttu-id="497b0-149">Uložte a zavřete hello **task.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="497b0-149">Save and close hello **task.js** file.</span></span>

### <a name="create-hello-controller"></a><span data-ttu-id="497b0-150">Vytvořit řadič hello</span><span class="sxs-lookup"><span data-stu-id="497b0-150">Create hello controller</span></span>
1. <span data-ttu-id="497b0-151">V hello **WebRole1 a směruje** adresáře, vytvořte nový soubor s názvem **tasklist.js** a otevřete v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="497b0-151">In hello **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="497b0-152">Přidejte následující kód příliš hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="497b0-152">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="497b0-153">Tento kód načte azure a asynchronních hello moduly, které jsou používány **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="497b0-153">This loads hello azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="497b0-154">Také definuje hello **TaskList** funkce, která se předá instance hello **úloh** definovaného dříve:</span><span class="sxs-lookup"><span data-stu-id="497b0-154">This also defines hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="497b0-155">Pokračovat v přidávání toohello **tasklist.js** souboru přidáním hello metody použité příliš**showTasks**, **addTask**, a **Completetask**:</span><span class="sxs-lookup"><span data-stu-id="497b0-155">Continue adding toohello **tasklist.js** file by adding hello methods used too**showTasks**, **addTask**, and **completeTasks**:</span></span>

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

4. <span data-ttu-id="497b0-156">Uložit hello **tasklist.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="497b0-156">Save hello **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="497b0-157">Úprava souboru app.js</span><span class="sxs-lookup"><span data-stu-id="497b0-157">Modify app.js</span></span>
1. <span data-ttu-id="497b0-158">V hello **WebRole1** adresář, otevřete hello **app.js** soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="497b0-158">In hello **WebRole1** directory, open hello **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="497b0-159">Od začátku hello hello souboru, přidejte následující modul hello azure tooload hello a nastavte klíč název a oddíl tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="497b0-159">At hello beginning of hello file, add hello following tooload hello azure module and set hello table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="497b0-160">V souboru app.js hello, posuňte se dolů toowhere zobrazí hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="497b0-160">In hello app.js file, scroll down toowhere you see hello following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="497b0-161">Nahraďte hello výše řádky kódu hello vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="497b0-161">Replace hello above lines with hello code shown below.</span></span> <span data-ttu-id="497b0-162">To bude inicializovat instanci <strong>úloh</strong> s účtem úložiště tooyour připojení.</span><span class="sxs-lookup"><span data-stu-id="497b0-162">This will initialize an instance of <strong>Task</strong> with a connection tooyour storage account.</span></span> <span data-ttu-id="497b0-163">To je předán toohello <strong>TaskList</strong>, který použije ji toocommunicate hello služby Table:</span><span class="sxs-lookup"><span data-stu-id="497b0-163">This is passed toohello <strong>TaskList</strong>, which will use it toocommunicate with hello Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="497b0-164">Uložit hello **app.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="497b0-164">Save hello **app.js** file.</span></span>

### <a name="modify-hello-index-view"></a><span data-ttu-id="497b0-165">Upravit zobrazení indexu hello</span><span class="sxs-lookup"><span data-stu-id="497b0-165">Modify hello index view</span></span>
1. <span data-ttu-id="497b0-166">Změna adresáře toohello **zobrazení** adresář a otevřete hello **index.jade** soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="497b0-166">Change directories toohello **views** directory and open hello **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="497b0-167">Nahraďte obsah hello hello **index.jade** soubor s hello kód níže.</span><span class="sxs-lookup"><span data-stu-id="497b0-167">Replace hello contents of hello **index.jade** file with hello code below.</span></span> <span data-ttu-id="497b0-168">Definuje hello zobrazení pro zobrazení stávající úlohy a také formuláře pro přidání nové úlohy a označit stávajících jako dokončenou.</span><span class="sxs-lookup"><span data-stu-id="497b0-168">This defines hello view for displaying existing tasks, as well as a form for adding new tasks and marking existing ones as completed.</span></span>

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

3. <span data-ttu-id="497b0-169">Uložte a zavřete **index.jade** souboru.</span><span class="sxs-lookup"><span data-stu-id="497b0-169">Save and close **index.jade** file.</span></span>

### <a name="modify-hello-global-layout"></a><span data-ttu-id="497b0-170">Upravit globální rozložení hello</span><span class="sxs-lookup"><span data-stu-id="497b0-170">Modify hello global layout</span></span>
<span data-ttu-id="497b0-171">Hello **layout.jade** souboru v hello **zobrazení** directory se používá jako globální šablona pro ostatní **.jade** soubory.</span><span class="sxs-lookup"><span data-stu-id="497b0-171">hello **layout.jade** file in hello **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="497b0-172">V tomto kroku jej upravíte toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), což je sada nástrojů, který umožňuje snadno toodesign pěkných webů.</span><span class="sxs-lookup"><span data-stu-id="497b0-172">In this step you will modify it toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking website.</span></span>

1. <span data-ttu-id="497b0-173">Stažení a extrakci souborů hello [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="497b0-173">Download and extract hello files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="497b0-174">Kopírování hello **bootstrap.min.css** soubor z hello **bootstrap\\dist\\šablon stylů css** složky toohello **veřejné\\předlohy se styly** adresář tasklist aplikace.</span><span class="sxs-lookup"><span data-stu-id="497b0-174">Copy hello **bootstrap.min.css** file from hello **bootstrap\\dist\\css** folder toohello **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="497b0-175">Z hello **zobrazení** složku, otevřete hello **layout.jade** ve vaší textového editoru a nahradit hello obsah s hello následující:</span><span class="sxs-lookup"><span data-stu-id="497b0-175">From hello **views** folder, open hello **layout.jade** in your text editor and replace hello contents with hello following:</span></span>

    <span data-ttu-id="497b0-176">DOCTYPE html html head title = název odkazu (relativní = 'šablony stylů', href='/stylesheets/bootstrap.min.css.) odkaz (relativní = 'šablony stylů', href='/stylesheets/style.css.) body.app nav.navbar.navbar výchozí záhlaví div.navbar a.navbar-brand(href='/') vlastní úkoly blokování obsahu</span><span class="sxs-lookup"><span data-stu-id="497b0-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="497b0-177">Uložit hello **layout.jade** souboru.</span><span class="sxs-lookup"><span data-stu-id="497b0-177">Save hello **layout.jade** file.</span></span>

### <a name="running-hello-application-in-hello-emulator"></a><span data-ttu-id="497b0-178">Spuštění hello aplikaci v emulátoru hello</span><span class="sxs-lookup"><span data-stu-id="497b0-178">Running hello Application in hello Emulator</span></span>
<span data-ttu-id="497b0-179">Použijte následující příkaz toostart hello aplikaci v emulátoru hello hello.</span><span class="sxs-lookup"><span data-stu-id="497b0-179">Use hello following command toostart hello application in hello emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="497b0-180">Hello prohlížeči se otevře a zobrazí hello následující stránky:</span><span class="sxs-lookup"><span data-stu-id="497b0-180">hello browser will open and displays hello following page:</span></span>

![Webové stránkovaného fondu s názvem Moje seznam úkolů v tabulce obsahující pole a úlohy tooadd novou úlohu.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

<span data-ttu-id="497b0-182">Použití položek tooadd hello formuláře nebo odebrat existující položky, můžete je označit jako dokončená.</span><span class="sxs-lookup"><span data-stu-id="497b0-182">Use hello form tooadd items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-hello-application-tooazure"></a><span data-ttu-id="497b0-183">Publikování aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="497b0-183">Publishing hello Application tooAzure</span></span>
<span data-ttu-id="497b0-184">V okně prostředí Windows PowerShell hello volání hello následující rutiny tooredeploy tooAzure vaší hostované služby.</span><span class="sxs-lookup"><span data-stu-id="497b0-184">In hello Windows PowerShell window, call hello following cmdlet tooredeploy your hosted service tooAzure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="497b0-185">Nahraďte **myuniquename** s jedinečný název pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="497b0-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="497b0-186">Nahraďte **datovém centru** s názvem hello datové centrum Azure, jako například **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="497b0-186">Replace **datacentername** with hello name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="497b0-187">Po dokončení nasazení hello byste měli vidět a odpovědi podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="497b0-187">After hello deployment is complete, you should see a response similar toohello following:</span></span>

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

<span data-ttu-id="497b0-188">Jako předtím protože zadaný hello **– spusťte** možnost hello otevře se prohlížeč a zobrazí vaši aplikaci běžící v Azure po dokončení publikování.</span><span class="sxs-lookup"><span data-stu-id="497b0-188">As before, because you specified hello **-launch** option, hello browser opens and displays your application running in Azure when publishing is completed.</span></span>

![Okno prohlížeče zobrazující stránku hello Moje seznamu úkolů.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="497b0-191">Zastavení a odstranění vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="497b0-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="497b0-192">Po nasazení aplikace, může být vhodné toodisable ji proto můžete vyhnout nákladům nebo sestavení a nasazení dalších aplikací v rámci hello volné zkušební období.</span><span class="sxs-lookup"><span data-stu-id="497b0-192">After deploying your application, you may want toodisable it so you can avoid costs or build and deploy other applications within hello free trial time period.</span></span>

<span data-ttu-id="497b0-193">Azure účtuje poplatky za instance webových rolí podle času (v hodinách), který na serveru spotřebují.</span><span class="sxs-lookup"><span data-stu-id="497b0-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="497b0-194">Čas serveru se spotřebovává, jakmile vaše aplikace je nasazená, i když nejsou spuštěné instance a jsou ve stavu hello byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="497b0-194">Server time is consumed once your application is deployed, even if the instances are not running and are in hello stopped state.</span></span>

<span data-ttu-id="497b0-195">Hello následující kroky ukazují, jak toostop a odstranění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="497b0-195">hello following steps show you how toostop and delete your application.</span></span>

1. <span data-ttu-id="497b0-196">V okně prostředí Windows PowerShell hello zastavte hello nasazení služby vytvořené v předchozí části hello s hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="497b0-196">In hello Windows PowerShell window, stop hello service deployment created in hello previous section with hello following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="497b0-197">Zastavování služby hello může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="497b0-197">Stopping hello service may take several minutes.</span></span> <span data-ttu-id="497b0-198">Pokud je hello služba zastavená, obdržíte zprávu s upozorněním, že se zastavil.</span><span class="sxs-lookup"><span data-stu-id="497b0-198">When hello service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="497b0-199">toodelete hello služba, volání hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="497b0-199">toodelete hello service, call hello following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="497b0-200">Po zobrazení výzvy zadejte **Y** toodelete hello služby.</span><span class="sxs-lookup"><span data-stu-id="497b0-200">When prompted, enter **Y** toodelete hello service.</span></span>

   <span data-ttu-id="497b0-201">Odstraněním hello služby může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="497b0-201">Deleting hello service may take several minutes.</span></span> <span data-ttu-id="497b0-202">Po odstranění hello služby obdržíte zprávu s upozorněním, že se odstranila služba hello.</span><span class="sxs-lookup"><span data-stu-id="497b0-202">After hello service has been deleted you receive a message indicating that hello service was deleted.</span></span>

[webové aplikace Node.js pomocí Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[ukládání a přístup k datům v Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[webové aplikace Node.js]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


