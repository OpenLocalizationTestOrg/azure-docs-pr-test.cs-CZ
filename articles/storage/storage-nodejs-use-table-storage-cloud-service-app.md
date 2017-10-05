---
title: "Webové aplikace s table storage (Node.js) | Microsoft Docs"
description: "Kurz, který je založený na webovou aplikaci s Express kurzu přidáním služby Azure Storage a modulu Azure."
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
ms.openlocfilehash: 5d7ee2f529b5127ee60ec8b4f5acaa49e75ddf39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="nodejs-web-application-using-storage"></a><span data-ttu-id="bc381-103">Webové aplikace Node.js pomocí úložiště</span><span class="sxs-lookup"><span data-stu-id="bc381-103">Node.js Web Application using Storage</span></span>
## <a name="overview"></a><span data-ttu-id="bc381-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="bc381-104">Overview</span></span>
<span data-ttu-id="bc381-105">V tomto kurzu rozšíříte vytvořené v aplikaci [webové aplikace Node.js pomocí Express] kurz pomocí knihovny klienta Microsoft Azure pro platformu Node.js pro práci se službami pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="bc381-105">In this tutorial, you will extend the application created in the [Node.js Web Application using Express] tutorial by using the Microsoft Azure Client Libraries for Node.js to work with data management services.</span></span> <span data-ttu-id="bc381-106">Rozšíříte aplikace k vytvoření aplikace založené na webu – seznam úloh, kterou můžete nasadit do Azure.</span><span class="sxs-lookup"><span data-stu-id="bc381-106">You will extend your application to create a web-based task-list application that you can deploy to Azure.</span></span> <span data-ttu-id="bc381-107">Seznam úloh umožňuje uživateli načíst úlohy, přidat nové úkoly a úkoly označit jako dokončená.</span><span class="sxs-lookup"><span data-stu-id="bc381-107">The task list allows a user to retrieve tasks, add new tasks, and mark tasks as completed.</span></span>

<span data-ttu-id="bc381-108">Položky úkolů jsou uložené ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bc381-108">The task items are stored in Azure Storage.</span></span> <span data-ttu-id="bc381-109">Úložiště Azure poskytuje úložiště nestrukturovaných dat, které je odolné proti chybám a vysoce dostupné.</span><span class="sxs-lookup"><span data-stu-id="bc381-109">Azure Storage provides unstructured data storage that is fault-tolerant and highly available.</span></span> <span data-ttu-id="bc381-110">Úložiště Azure obsahuje několik datové struktury, kam můžete ukládat a přistupovat k datům a mohou využívat služby storage z rozhraní API zahrnutý v sadě Azure SDK pro Node.js nebo přes rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="bc381-110">Azure Storage includes several data structures where you can store and access data, and you can leverage the storage services from the APIs included in the Azure SDK for Node.js or via REST APIs.</span></span> <span data-ttu-id="bc381-111">Další informace najdete v tématu [ukládání a přístup k datům v Azure].</span><span class="sxs-lookup"><span data-stu-id="bc381-111">For more information, see [Storing and Accessing Data in Azure].</span></span>

<span data-ttu-id="bc381-112">V tomto kurzu se předpokládá, že jste dokončili [webové aplikace Node.js] a [Node.js s Express][webové aplikace Node.js pomocí Express] kurzy.</span><span class="sxs-lookup"><span data-stu-id="bc381-112">This tutorial assumes that you have completed the [Node.js Web Application] and [Node.js with Express][Node.js Web Application using Express] tutorials.</span></span>

<span data-ttu-id="bc381-113">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="bc381-113">You will learn:</span></span>

* <span data-ttu-id="bc381-114">Jak pracovat s modulem Jade šablony</span><span class="sxs-lookup"><span data-stu-id="bc381-114">How to work with the Jade template engine</span></span>
* <span data-ttu-id="bc381-115">Jak pracovat s Azure Data Management services</span><span class="sxs-lookup"><span data-stu-id="bc381-115">How to work with Azure Data Management services</span></span>

<span data-ttu-id="bc381-116">Zde je snímek obrazovky dokončené aplikace:</span><span class="sxs-lookup"><span data-stu-id="bc381-116">A screenshot of the completed application is below:</span></span>

![Dokončené webové stránky v aplikaci internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a><span data-ttu-id="bc381-118">Nastavení přihlašovacích údajů úložiště v souboru Web.Config.</span><span class="sxs-lookup"><span data-stu-id="bc381-118">Setting Storage Credentials in Web.Config</span></span>
<span data-ttu-id="bc381-119">Pro přístup k úložišti Azure, musíte předat přihlašovací údaje úložiště.</span><span class="sxs-lookup"><span data-stu-id="bc381-119">To access Azure Storage, you need to pass in storage credentials.</span></span> <span data-ttu-id="bc381-120">K tomu využívat nastavení aplikace v souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="bc381-120">To do this, you utilize web.config application settings.</span></span>
<span data-ttu-id="bc381-121">Tato nastavení budou předány jako proměnné prostředí do uzlu, které jsou pak přečte sadu Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="bc381-121">Those settings will be passed as environment variables to Node, which are then read by the Azure SDK.</span></span>

> [!NOTE]
> <span data-ttu-id="bc381-122">Přihlašovací údaje úložiště používají jenom při nasazení aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="bc381-122">Storage credentials are only used when the application is deployed to Azure.</span></span> <span data-ttu-id="bc381-123">Při spuštění v emulátoru, aplikace bude používat emulátor úložiště.</span><span class="sxs-lookup"><span data-stu-id="bc381-123">When running in the emulator, the application will use the storage emulator.</span></span>
>
>

<span data-ttu-id="bc381-124">Proveďte následující kroky k načtení údaje k účtu úložiště a přidat je do souboru web.config nastavení:</span><span class="sxs-lookup"><span data-stu-id="bc381-124">Perform the following steps to retrieve the storage account credentials and add them to the web.config settings:</span></span>

1. <span data-ttu-id="bc381-125">Pokud již není otevřený, otevřete prostředí Azure PowerShell z **spustit** nabídky rozšířením **všechny programy, Azure**, klikněte pravým tlačítkem na **prostředí Azure PowerShell**a potom vyberte **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="bc381-125">If it is not already open, start the Azure PowerShell from the **Start** menu by expanding **All Programs, Azure**, right-click **Azure PowerShell**, and then select **Run As Administrator**.</span></span>
2. <span data-ttu-id="bc381-126">Změňte adresář na složku obsahující aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc381-126">Change directories to the folder containing your application.</span></span> <span data-ttu-id="bc381-127">Například C:\\uzlu\\tasklist\\WebRole1.</span><span class="sxs-lookup"><span data-stu-id="bc381-127">For example, C:\\node\\tasklist\\WebRole1.</span></span>
3. <span data-ttu-id="bc381-128">V okně Azure Powershell zadejte následující rutiny můžete načíst informace o účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="bc381-128">From the Azure Powershell window enter the following cmdlet to retrieve the storage account information:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   <span data-ttu-id="bc381-129">To umožňuje načíst seznam účtů úložiště a účtu klíče přidruženého k vaší hostované služby.</span><span class="sxs-lookup"><span data-stu-id="bc381-129">This retrieves the list of storage accounts and account keys associated with your hosted service.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bc381-130">Vzhledem k tomu, že sadu Azure SDK vytvoří účet úložiště při nasazení služby, by měl z nasazení aplikace v předchozí příručkách již existuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bc381-130">Since the Azure SDK creates a storage account when you deploy a service, a storage account should already exist from deploying your application in the previous guides.</span></span>
   >
   >
4. <span data-ttu-id="bc381-131">Otevřete **ServiceDefinition.csdef** souboru, který obsahuje nastavení prostředí, které se používají při nasazení aplikace do Azure:</span><span class="sxs-lookup"><span data-stu-id="bc381-131">Open the **ServiceDefinition.csdef** file containing the environment settings that are used when the application is deployed to Azure:</span></span>

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. <span data-ttu-id="bc381-132">Vložte následující blok v části **prostředí** elementu, nahraďte {účet úložiště} a {přístupový klíč k ÚLOŽIŠTI} s názvem účtu a primární klíč pro účet úložiště, kterou chcete použít pro nasazení:</span><span class="sxs-lookup"><span data-stu-id="bc381-132">Insert the following block under **Environment** element, substituting {STORAGE ACCOUNT} and {STORAGE ACCESS KEY} with the account name and the primary key for the storage account you want to use for deployment:</span></span>

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![Obsah souboru web.cloud.config](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6. <span data-ttu-id="bc381-134">Soubor uložte a zavřete poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="bc381-134">Save the file and close notepad.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="bc381-135">Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="bc381-135">Install additional modules</span></span>
1. <span data-ttu-id="bc381-136">Použijte následující příkaz k instalaci [azure,] [uzlu uuid], [nconf] a [asynchronní] moduly místně i tak, aby mohly uložit položku **package.json** souboru:</span><span class="sxs-lookup"><span data-stu-id="bc381-136">Use the following command to install the [azure], [node-uuid], [nconf] and [async] modules locally as well as to save an entry for them to the **package.json** file:</span></span>

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  <span data-ttu-id="bc381-137">Výstup tohoto příkazu by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="bc381-137">The output of this command should appear similar to the following:</span></span>

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

## <a name="using-the-table-service-in-a-node-application"></a><span data-ttu-id="bc381-138">Pomocí služby Table v aplikaci node</span><span class="sxs-lookup"><span data-stu-id="bc381-138">Using the Table service in a node application</span></span>
<span data-ttu-id="bc381-139">V této části rozšíříte základní aplikace vytvořené **express** příkaz přidáním **task.js** soubor, který obsahuje model pro úkoly.</span><span class="sxs-lookup"><span data-stu-id="bc381-139">In this section you will extend the basic application created by the **express** command by adding a **task.js** file which contains the model for your tasks.</span></span> <span data-ttu-id="bc381-140">Také upravíte existující **app.js** a vytvořte novou **tasklist.js** soubor, který používá model.</span><span class="sxs-lookup"><span data-stu-id="bc381-140">You will also modify the existing **app.js** and create a new **tasklist.js** file that uses the model.</span></span>

### <a name="create-the-model"></a><span data-ttu-id="bc381-141">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="bc381-141">Create the model</span></span>
1. <span data-ttu-id="bc381-142">V **WebRole1** adresáře, vytvořte nový adresář s názvem **modely**.</span><span class="sxs-lookup"><span data-stu-id="bc381-142">In the **WebRole1** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="bc381-143">V **modely** adresáře, vytvořte nový soubor s názvem **task.js**.</span><span class="sxs-lookup"><span data-stu-id="bc381-143">In the **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="bc381-144">Tento soubor bude obsahovat model pro úkoly vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc381-144">This file will contain the model for the tasks created by your application.</span></span>
3. <span data-ttu-id="bc381-145">Na začátku **task.js** soubor, přidejte následující kód k odkazování požadované knihovny:</span><span class="sxs-lookup"><span data-stu-id="bc381-145">At the beginning of the **task.js** file, add the following code to reference required libraries:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. <span data-ttu-id="bc381-146">Dále přidáte kód, který definuje export objektu Task.</span><span class="sxs-lookup"><span data-stu-id="bc381-146">Next, you will add code to define and export the Task object.</span></span> <span data-ttu-id="bc381-147">Tento objekt je zodpovědná za připojení do tabulky.</span><span class="sxs-lookup"><span data-stu-id="bc381-147">This object is responsible for connecting to the table.</span></span>

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

5. <span data-ttu-id="bc381-148">Dál přidejte následující kód, který definuje další metody objektu Task ty umožňují pracovat s daty uloženými v tabulce:</span><span class="sxs-lookup"><span data-stu-id="bc381-148">Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in the table:</span></span>

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
        // use entityGenerator to set types
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

6. <span data-ttu-id="bc381-149">Uložte a zavřete **task.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="bc381-149">Save and close the **task.js** file.</span></span>

### <a name="create-the-controller"></a><span data-ttu-id="bc381-150">Vytvoření kontroleru</span><span class="sxs-lookup"><span data-stu-id="bc381-150">Create the controller</span></span>
1. <span data-ttu-id="bc381-151">V **WebRole1 a směruje** adresáře, vytvořte nový soubor s názvem **tasklist.js** a otevřete v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="bc381-151">In the **WebRole1/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="bc381-152">Do souboru **tasklist.js** přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="bc381-152">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="bc381-153">Tento kód načte moduly azure a async, které jsou používány **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="bc381-153">This loads the azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="bc381-154">Také definuje **TaskList** funkce, která se předá instance **úloh** definovaného dříve:</span><span class="sxs-lookup"><span data-stu-id="bc381-154">This also defines the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. <span data-ttu-id="bc381-155">Pokračovat v přidání do **tasklist.js** souboru přidáním metody použité k **showTasks**, **addTask**, a **Completetask**:</span><span class="sxs-lookup"><span data-stu-id="bc381-155">Continue adding to the **tasklist.js** file by adding the methods used to **showTasks**, **addTask**, and **completeTasks**:</span></span>

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

4. <span data-ttu-id="bc381-156">Uložit **tasklist.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="bc381-156">Save the **tasklist.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="bc381-157">Úprava souboru app.js</span><span class="sxs-lookup"><span data-stu-id="bc381-157">Modify app.js</span></span>
1. <span data-ttu-id="bc381-158">V **WebRole1** adresáře, otevřete **app.js** soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="bc381-158">In the **WebRole1** directory, open the **app.js** file in a text editor.</span></span>
2. <span data-ttu-id="bc381-159">Na začátek souboru přidejte následující příkaz pro načtení modulu azure a nastavte název a oddíl klíč tabulky:</span><span class="sxs-lookup"><span data-stu-id="bc381-159">At the beginning of the file, add the following to load the azure module and set the table name and partition key:</span></span>

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. <span data-ttu-id="bc381-160">V souboru app.js posuňte se dolů, kde uvidíte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="bc381-160">In the app.js file, scroll down to where you see the following line:</span></span>

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    <span data-ttu-id="bc381-161">Nahraďte kód vidíte níže uvedených řádků.</span><span class="sxs-lookup"><span data-stu-id="bc381-161">Replace the above lines with the code shown below.</span></span> <span data-ttu-id="bc381-162">To bude inicializovat instanci <strong>úloh</strong> s připojením k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bc381-162">This will initialize an instance of <strong>Task</strong> with a connection to your storage account.</span></span> <span data-ttu-id="bc381-163">To je předán <strong>TaskList</strong>, který bude používat pro komunikaci se službou tabulky:</span><span class="sxs-lookup"><span data-stu-id="bc381-163">This is passed to the <strong>TaskList</strong>, which will use it to communicate with the Table service:</span></span>

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. <span data-ttu-id="bc381-164">Uložit **app.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="bc381-164">Save the **app.js** file.</span></span>

### <a name="modify-the-index-view"></a><span data-ttu-id="bc381-165">Upravit zobrazení indexu</span><span class="sxs-lookup"><span data-stu-id="bc381-165">Modify the index view</span></span>
1. <span data-ttu-id="bc381-166">Přejděte do adresáře **zobrazení** adresář a otevřete **index.jade** soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="bc381-166">Change directories to the **views** directory and open the **index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="bc381-167">Nahraďte obsah **index.jade** souboru pomocí kódu níže.</span><span class="sxs-lookup"><span data-stu-id="bc381-167">Replace the contents of the **index.jade** file with the code below.</span></span> <span data-ttu-id="bc381-168">Definuje zobrazení pro zobrazení stávající úlohy a také formuláře pro přidání nové úlohy a označit stávajících jako dokončenou.</span><span class="sxs-lookup"><span data-stu-id="bc381-168">This defines the view for displaying existing tasks, as well as a form for adding new tasks and marking existing ones as completed.</span></span>

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

3. <span data-ttu-id="bc381-169">Uložte a zavřete **index.jade** souboru.</span><span class="sxs-lookup"><span data-stu-id="bc381-169">Save and close **index.jade** file.</span></span>

### <a name="modify-the-global-layout"></a><span data-ttu-id="bc381-170">Upravit globální rozložení</span><span class="sxs-lookup"><span data-stu-id="bc381-170">Modify the global layout</span></span>
<span data-ttu-id="bc381-171">Soubor **layout.jade** v adresáři **views** slouží jako globální šablona pro ostatní soubory **.jade**.</span><span class="sxs-lookup"><span data-stu-id="bc381-171">The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="bc381-172">V tomto kroku jej upravíte tak, aby používal [Twitter Bootstrap](https://github.com/twbs/bootstrap). To je sada nástrojů, které usnadňují návrh pěkných webů.</span><span class="sxs-lookup"><span data-stu-id="bc381-172">In this step you will modify it to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking website.</span></span>

1. <span data-ttu-id="bc381-173">Stažení a extrakci souborů pro [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="bc381-173">Download and extract the files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="bc381-174">Kopírování **bootstrap.min.css** souboru z **bootstrap\\dist\\šablon stylů css** složku pro **veřejné\\předlohy se styly** adresář tasklist aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc381-174">Copy the **bootstrap.min.css** file from the **bootstrap\\dist\\css** folder to the **public\\stylesheets** directory of your tasklist application.</span></span>
2. <span data-ttu-id="bc381-175">Z **zobrazení** složku, otevřete **layout.jade** v textovém editoru a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="bc381-175">From the **views** folder, open the **layout.jade** in your text editor and replace the contents with the following:</span></span>

    <span data-ttu-id="bc381-176">DOCTYPE html html head title = název odkazu (relativní = 'šablony stylů', href='/stylesheets/bootstrap.min.css.) odkaz (relativní = 'šablony stylů', href='/stylesheets/style.css.) body.app nav.navbar.navbar výchozí záhlaví div.navbar a.navbar-brand(href='/') vlastní úkoly blokování obsahu</span><span class="sxs-lookup"><span data-stu-id="bc381-176">doctype html  html    head      title= title      link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')      link(rel='stylesheet', href='/stylesheets/style.css')    body.app      nav.navbar.navbar-default        div.navbar-header          a.navbar-brand(href='/') My Tasks      block content</span></span>

3. <span data-ttu-id="bc381-177">Uložit **layout.jade** souboru.</span><span class="sxs-lookup"><span data-stu-id="bc381-177">Save the **layout.jade** file.</span></span>

### <a name="running-the-application-in-the-emulator"></a><span data-ttu-id="bc381-178">Spuštění aplikace v emulátoru</span><span class="sxs-lookup"><span data-stu-id="bc381-178">Running the Application in the Emulator</span></span>
<span data-ttu-id="bc381-179">Použijte následující příkaz a spusťte aplikaci v emulátoru.</span><span class="sxs-lookup"><span data-stu-id="bc381-179">Use the following command to start the application in the emulator.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

<span data-ttu-id="bc381-180">V prohlížeči se otevře a zobrazí následující stránka:</span><span class="sxs-lookup"><span data-stu-id="bc381-180">The browser will open and displays the following page:</span></span>

![Webové stránkovaného fondu s názvem Moje seznam úkolů v tabulce obsahující pole pro přidání nové úlohy a úlohy.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

<span data-ttu-id="bc381-182">Formulář slouží k přidání položek nebo odebrat existující položky, můžete je označit jako dokončená.</span><span class="sxs-lookup"><span data-stu-id="bc381-182">Use the form to add items, or remove existing items by marking them as completed.</span></span>

## <a name="publishing-the-application-to-azure"></a><span data-ttu-id="bc381-183">Publikování aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="bc381-183">Publishing the Application to Azure</span></span>
<span data-ttu-id="bc381-184">V okně prostředí Windows PowerShell zavolejte následující rutiny můžete znovu nasadit vaší hostované služby Azure.</span><span class="sxs-lookup"><span data-stu-id="bc381-184">In the Windows PowerShell window, call the following cmdlet to redeploy your hosted service to Azure.</span></span>

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

<span data-ttu-id="bc381-185">Nahraďte **myuniquename** s jedinečný název pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc381-185">Replace **myuniquename** with a unique name for this application.</span></span> <span data-ttu-id="bc381-186">Nahraďte **datovém centru** s názvem datové centrum Azure, jako například **západní USA**.</span><span class="sxs-lookup"><span data-stu-id="bc381-186">Replace **datacentername** with the name of an Azure data center, such as **West US**.</span></span>

<span data-ttu-id="bc381-187">Po dokončení nasazení byste měli vidět odpověď podobná následující:</span><span class="sxs-lookup"><span data-stu-id="bc381-187">After the deployment is complete, you should see a response similar to the following:</span></span>

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

<span data-ttu-id="bc381-188">Jako předtím protože jste zadali **– spusťte** možnost, otevře se prohlížeč a zobrazí vaši aplikaci běžící v Azure po dokončení publikování.</span><span class="sxs-lookup"><span data-stu-id="bc381-188">As before, because you specified the **-launch** option, the browser opens and displays your application running in Azure when publishing is completed.</span></span>

![Okno prohlížeče zobrazující stránku Moje seznamu úloh.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="bc381-191">Zastavení a odstranění vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="bc381-191">Stopping and Deleting Your Application</span></span>
<span data-ttu-id="bc381-192">Po nasazení aplikace, můžete ji zakázat, takže můžete vyhnout nákladům nebo sestavení a nasazení dalších aplikací v rámci bezplatné zkušební období.</span><span class="sxs-lookup"><span data-stu-id="bc381-192">After deploying your application, you may want to disable it so you can avoid costs or build and deploy other applications within the free trial time period.</span></span>

<span data-ttu-id="bc381-193">Azure účtuje poplatky za instance webových rolí podle času (v hodinách), který na serveru spotřebují.</span><span class="sxs-lookup"><span data-stu-id="bc381-193">Azure bills web role instances per hour of server time consumed.</span></span>
<span data-ttu-id="bc381-194">Čas serveru se spotřebovává, jakmile je vaše aplikace nasazena, i když jsou instance zrovna zastaveny a neběží.</span><span class="sxs-lookup"><span data-stu-id="bc381-194">Server time is consumed once your application is deployed, even if the instances are not running and are in the stopped state.</span></span>

<span data-ttu-id="bc381-195">Následující kroky ukazují, jak zastavení a odstranění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc381-195">The following steps show you how to stop and delete your application.</span></span>

1. <span data-ttu-id="bc381-196">V okně prostředí Windows PowerShell pomocí následující rutiny ukončete nasazení služby vytvořené v předchozí části:</span><span class="sxs-lookup"><span data-stu-id="bc381-196">In the Windows PowerShell window, stop the service deployment created in the previous section with the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   <span data-ttu-id="bc381-197">Zastavování služby může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="bc381-197">Stopping the service may take several minutes.</span></span> <span data-ttu-id="bc381-198">Až bude služba zastavená, obdržíte zprávu s oznámením, že se tak stalo.</span><span class="sxs-lookup"><span data-stu-id="bc381-198">When the service is stopped, you receive a message indicating that it has stopped.</span></span>

2. <span data-ttu-id="bc381-199">Pokud chcete službu odstranit, zavolejte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="bc381-199">To delete the service, call the following cmdlet:</span></span>

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   <span data-ttu-id="bc381-200">Po zobrazení výzvy zadejte **Y**, a službu tak odstraňte.</span><span class="sxs-lookup"><span data-stu-id="bc381-200">When prompted, enter **Y** to delete the service.</span></span>

   <span data-ttu-id="bc381-201">Odstraňování služby může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="bc381-201">Deleting the service may take several minutes.</span></span> <span data-ttu-id="bc381-202">Po odstranění služby obdržíte zprávu s oznámením, že se tak stalo.</span><span class="sxs-lookup"><span data-stu-id="bc381-202">After the service has been deleted you receive a message indicating that the service was deleted.</span></span>

<span data-ttu-id="bc381-203">[webové aplikace Node.js pomocí Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/</span><span class="sxs-lookup"><span data-stu-id="bc381-203">[Node.js Web Application using Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/</span></span>
<span data-ttu-id="bc381-204">[ukládání a přístup k datům v Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx</span><span class="sxs-lookup"><span data-stu-id="bc381-204">[Storing and Accessing Data in Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx</span></span>
<span data-ttu-id="bc381-205">[webové aplikace Node.js]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/</span><span class="sxs-lookup"><span data-stu-id="bc381-205">[Node.js Web Application]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/</span></span>


