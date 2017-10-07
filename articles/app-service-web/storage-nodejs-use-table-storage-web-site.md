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
# <a name="nodejs-web-app-using-hello-azure-table-service"></a><span data-ttu-id="04d8c-103">Webové aplikace Node.js pomocí hello služby Azure Table</span><span class="sxs-lookup"><span data-stu-id="04d8c-103">Node.js web app using hello Azure Table Service</span></span>
## <a name="overview"></a><span data-ttu-id="04d8c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="04d8c-104">Overview</span></span>
<span data-ttu-id="04d8c-105">Tento kurz ukazuje, jak služby Table toouse poskytované data toostore a přístup k Azure Data správy od [uzlu] aplikace hostována v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-105">This tutorial shows you how toouse Table service provided by Azure Data Management toostore and access data from a [node] application hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="04d8c-106">V tomto kurzu se předpokládá, že máte zkušenosti s pomocí uzlu a [Git].</span><span class="sxs-lookup"><span data-stu-id="04d8c-106">This tutorial assumes that you have some prior experience using node and [Git].</span></span>

<span data-ttu-id="04d8c-107">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="04d8c-107">You will learn:</span></span>

* <span data-ttu-id="04d8c-108">Jak toouse npm (uzel Správce balíčků) tooinstall hello uzlu moduly</span><span class="sxs-lookup"><span data-stu-id="04d8c-108">How toouse npm (node package manager) tooinstall hello node modules</span></span>
* <span data-ttu-id="04d8c-109">Jak toowork s hello služby Azure Table</span><span class="sxs-lookup"><span data-stu-id="04d8c-109">How toowork with hello Azure Table service</span></span>
* <span data-ttu-id="04d8c-110">Jak toouse hello rozhraní příkazového řádku Azure toocreate webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-110">How toouse hello Azure CLI toocreate a web app.</span></span>

<span data-ttu-id="04d8c-111">Podle tohoto kurzu vytvoříte jednoduchou webovou aplikaci "seznam úkolů", která umožňuje vytváření, získávat a dokončovat úkoly.</span><span class="sxs-lookup"><span data-stu-id="04d8c-111">By following this tutorial, you will build a simple web-based "to-do list" application that allows creating, retrieving and completing tasks.</span></span> <span data-ttu-id="04d8c-112">Hello úlohy jsou uloženy v hello služby Table.</span><span class="sxs-lookup"><span data-stu-id="04d8c-112">hello tasks are stored in hello Table service.</span></span>

<span data-ttu-id="04d8c-113">Zde je hello dokončit aplikace:</span><span class="sxs-lookup"><span data-stu-id="04d8c-113">Here is hello completed application:</span></span>

![Webová stránka zobrazující prázdný tasklist][node-table-finished]

> [!NOTE]
> <span data-ttu-id="04d8c-115">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="04d8c-115">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="04d8c-116">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="04d8c-116">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="04d8c-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="04d8c-117">Prerequisites</span></span>
<span data-ttu-id="04d8c-118">Než budete postupovat hello pokyny v tomto článku, zajistěte, abyste měli hello nainstalované tyto položky:</span><span class="sxs-lookup"><span data-stu-id="04d8c-118">Before following hello instructions in this article, ensure that you have hello following installed:</span></span>

* <span data-ttu-id="04d8c-119">[uzlu] verze 0.10.24 nebo vyšší</span><span class="sxs-lookup"><span data-stu-id="04d8c-119">[node] version 0.10.24 or higher</span></span>
* <span data-ttu-id="04d8c-120">[Git]</span><span class="sxs-lookup"><span data-stu-id="04d8c-120">[Git]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="create-a-storage-account"></a><span data-ttu-id="04d8c-121">vytvořit účet úložiště</span><span class="sxs-lookup"><span data-stu-id="04d8c-121">Create a storage account</span></span>
<span data-ttu-id="04d8c-122">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="04d8c-122">Create an Azure storage account.</span></span> <span data-ttu-id="04d8c-123">Hello aplikace bude používat položkami seznamu úkolů hello toostore tento účet.</span><span class="sxs-lookup"><span data-stu-id="04d8c-123">hello app will use this account toostore hello to-do items.</span></span>

1. <span data-ttu-id="04d8c-124">Přihlaste se k hello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="04d8c-124">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="04d8c-125">Klikněte na tlačítko hello **nový** ikonu na hello spodní levé hello portál, pak klikněte na tlačítko **Data + úložiště** > **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-125">Click hello **New** icon on hello bottom left of hello portal, then click **Data + Storage** > **Storage**.</span></span> <span data-ttu-id="04d8c-126">Zadejte jedinečný název účtu úložiště hello a vytvořte novou [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) pro ni.</span><span class="sxs-lookup"><span data-stu-id="04d8c-126">Give hello storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Tlačítko Nový](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)
   
    <span data-ttu-id="04d8c-128">Po vytvoření účtu úložiště hello hello **oznámení** tlačítko bude flash zelená **úspěch** a se otevře okno účtu úložiště hello tooshow patří toohello nový prostředek skupiny vytvořit.</span><span class="sxs-lookup"><span data-stu-id="04d8c-128">When hello storage account has been created, hello **Notifications** button will flash a green **SUCCESS** and hello storage account's blade is open tooshow that it belongs toohello new resource group you created.</span></span>
3. <span data-ttu-id="04d8c-129">V okně účtu úložiště hello, klikněte na tlačítko **nastavení** > **klíče**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-129">In hello storage account's blade, click **Settings** > **Keys**.</span></span> <span data-ttu-id="04d8c-130">Kopírování hello primární přístupový klíč toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="04d8c-130">Copy hello primary access key toohello clipboard.</span></span>
   
    ![Přístupový klíč][portal-storage-access-keys]

## <a name="install-modules-and-generate-scaffolding"></a><span data-ttu-id="04d8c-132">Instalace modulů a generovat generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="04d8c-132">Install modules and generate scaffolding</span></span>
<span data-ttu-id="04d8c-133">V této části vytvoříte novou aplikaci uzlu a npm tooadd modulu balíčky.</span><span class="sxs-lookup"><span data-stu-id="04d8c-133">In this section you will create a new Node application and use npm tooadd module packages.</span></span> <span data-ttu-id="04d8c-134">Pro tuto aplikaci použijete hello [Express] a [Azure] moduly.</span><span class="sxs-lookup"><span data-stu-id="04d8c-134">For this application you will use hello [Express] and [Azure] modules.</span></span> <span data-ttu-id="04d8c-135">modul expresního Hello poskytuje Model View Controller rozhraní pro uzel, při hello moduly Azure poskytuje služby Table toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="04d8c-135">hello Express module provides a Model View Controller framework for node, while hello Azure modules provides connectivity toohello Table service.</span></span>

### <a name="install-express-and-generate-scaffolding"></a><span data-ttu-id="04d8c-136">Expresní instalace a generovat generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="04d8c-136">Install express and generate scaffolding</span></span>
1. <span data-ttu-id="04d8c-137">Z příkazového řádku hello, vytvořte nový adresář s názvem **tasklist** a directory toothat přepínače.</span><span class="sxs-lookup"><span data-stu-id="04d8c-137">From hello command line, create a new directory named **tasklist** and switch toothat directory.</span></span>  
2. <span data-ttu-id="04d8c-138">Zadejte následující příkaz tooinstall hello Express modulu hello.</span><span class="sxs-lookup"><span data-stu-id="04d8c-138">Enter hello following command tooinstall hello Express module.</span></span>
   
        npm install express-generator@4.2.0 -g
   
    <span data-ttu-id="04d8c-139">V závislosti na hello operační systém budete pravděpodobně potřebovat tooput příkazu "sudo" před hello příkaz:</span><span class="sxs-lookup"><span data-stu-id="04d8c-139">Depending on hello operating system, you may need tooput 'sudo' before hello command:</span></span>
   
        sudo npm install express-generator@4.2.0 -g
   
    <span data-ttu-id="04d8c-140">Zobrazí výstup Hello podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="04d8c-140">hello output appears similar toohello following example:</span></span>
   
        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)
   
   > [!NOTE]
   > <span data-ttu-id="04d8c-141">Hello "-g, parametr globálně nainstaluje modul hello.</span><span class="sxs-lookup"><span data-stu-id="04d8c-141">hello '-g' parameter installs hello module globally.</span></span> <span data-ttu-id="04d8c-142">Tímto způsobem, můžeme použít **express** toogenerate webové aplikace generování bez nutnosti tootype v další informace o cestě.</span><span class="sxs-lookup"><span data-stu-id="04d8c-142">That way, we can use **express** toogenerate web app scaffolding without having tootype in additional path information.</span></span>
   > 
   > 
3. <span data-ttu-id="04d8c-143">toocreate hello generování uživatelského rozhraní pro aplikace hello zadejte hello **express** příkaz:</span><span class="sxs-lookup"><span data-stu-id="04d8c-143">toocreate hello scaffolding for hello application, enter hello **express** command:</span></span>
   
        express
   
    <span data-ttu-id="04d8c-144">Hello výstup tohoto příkazu se zobrazí podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="04d8c-144">hello output of this command appears similar toohello following example:</span></span>
   
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
   
    <span data-ttu-id="04d8c-145">Nyní máte několik nových adresářů a souborů v hello **tasklist** adresáře.</span><span class="sxs-lookup"><span data-stu-id="04d8c-145">You now have several new directories and files in hello **tasklist** directory.</span></span>

### <a name="install-additional-modules"></a><span data-ttu-id="04d8c-146">Instalace dalších modulů</span><span class="sxs-lookup"><span data-stu-id="04d8c-146">Install additional modules</span></span>
<span data-ttu-id="04d8c-147">Jeden z hello soubory, které **express** vytvoří je **package.json**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-147">One of hello files that **express** creates is **package.json**.</span></span> <span data-ttu-id="04d8c-148">Tento soubor obsahuje seznam modulu závislosti.</span><span class="sxs-lookup"><span data-stu-id="04d8c-148">This file contains a list of module dependencies.</span></span> <span data-ttu-id="04d8c-149">Později když nasazujete aplikace tooApp hello Service Web Apps, tento soubor Určuje, které moduly musí toobe nainstalován v Azure.</span><span class="sxs-lookup"><span data-stu-id="04d8c-149">Later, when you deploy hello application tooApp Service Web Apps, this file determines which modules need toobe installed on Azure.</span></span>

<span data-ttu-id="04d8c-150">Z hello příkazového řádku, zadejte následující příkaz tooinstall hello moduly popsané v hello hello **package.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-150">From hello command-line, enter hello following command tooinstall hello modules described in hello **package.json** file.</span></span> <span data-ttu-id="04d8c-151">Může být nutné toouse "sudo".</span><span class="sxs-lookup"><span data-stu-id="04d8c-151">You may need toouse 'sudo'.</span></span>

    npm install

<span data-ttu-id="04d8c-152">Hello výstup tohoto příkazu se zobrazí podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="04d8c-152">hello output of this command appears similar toohello following example:</span></span>

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


<span data-ttu-id="04d8c-153">Potom zadejte následující příkaz tooinstall hello hello [azure], [uzlu uuid], [nconf] a [asynchronní] moduly:</span><span class="sxs-lookup"><span data-stu-id="04d8c-153">Next, enter hello following command tooinstall hello [azure], [node-uuid], [nconf] and [async] modules:</span></span>

    npm install azure-storage node-uuid async nconf --save

<span data-ttu-id="04d8c-154">Hello **– Uložit** příznak přidává položky pro tyto moduly toohello **package.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-154">hello **--save** flag adds entries for these modules toohello **package.json** file.</span></span>

<span data-ttu-id="04d8c-155">Hello výstup tohoto příkazu se zobrazí podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="04d8c-155">hello output of this command appears similar toohello following example:</span></span>

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## <a name="create-hello-application"></a><span data-ttu-id="04d8c-156">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="04d8c-156">Create hello application</span></span>
<span data-ttu-id="04d8c-157">Nyní jsme připravené toobuild hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-157">Now we're ready toobuild hello application.</span></span>

### <a name="create-a-model"></a><span data-ttu-id="04d8c-158">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="04d8c-158">Create a model</span></span>
<span data-ttu-id="04d8c-159">A *modelu* je objekt, který představuje data hello ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04d8c-159">A *model* is an object that represents hello data in your application.</span></span> <span data-ttu-id="04d8c-160">Pro aplikace hello pouze model hello je objekt úlohy, která představuje položku v seznamu úkolů hello.</span><span class="sxs-lookup"><span data-stu-id="04d8c-160">For hello application, hello only model is a task object, which represents an item in hello to-do list.</span></span> <span data-ttu-id="04d8c-161">Úlohy budou mít hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="04d8c-161">Tasks will have hello following fields:</span></span>

* <span data-ttu-id="04d8c-162">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="04d8c-162">PartitionKey</span></span>
* <span data-ttu-id="04d8c-163">RowKey</span><span class="sxs-lookup"><span data-stu-id="04d8c-163">RowKey</span></span>
* <span data-ttu-id="04d8c-164">název (string)</span><span class="sxs-lookup"><span data-stu-id="04d8c-164">name (string)</span></span>
* <span data-ttu-id="04d8c-165">kategorie (string)</span><span class="sxs-lookup"><span data-stu-id="04d8c-165">category (string)</span></span>
* <span data-ttu-id="04d8c-166">dokončené (Boolean)</span><span class="sxs-lookup"><span data-stu-id="04d8c-166">completed (Boolean)</span></span>

<span data-ttu-id="04d8c-167">**PartitionKey** a **RowKey** používají hello služby Table jako klíče tabulky.</span><span class="sxs-lookup"><span data-stu-id="04d8c-167">**PartitionKey** and **RowKey** are used by hello Table Service as table keys.</span></span> <span data-ttu-id="04d8c-168">Další informace najdete v tématu [Principy hello služby Table datový model](https://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="04d8c-168">For more information, see [Understanding hello Table Service data model](https://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

1. <span data-ttu-id="04d8c-169">V hello **tasklist** adresáře, vytvořte nový adresář s názvem **modely**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-169">In hello **tasklist** directory, create a new directory named **models**.</span></span>
2. <span data-ttu-id="04d8c-170">V hello **modely** adresáře, vytvořte nový soubor s názvem **task.js**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-170">In hello **models** directory, create a new file named **task.js**.</span></span> <span data-ttu-id="04d8c-171">Tento soubor bude obsahovat hello model pro úkoly hello vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04d8c-171">This file will contain hello model for hello tasks created by your application.</span></span>
3. <span data-ttu-id="04d8c-172">Na začátku hello hello **task.js** soubor, přidejte následující kód tooreference požadované knihovny hello:</span><span class="sxs-lookup"><span data-stu-id="04d8c-172">At hello beginning of hello **task.js** file, add hello following code tooreference required libraries:</span></span>
   
        var azure = require('azure-storage');
          var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;
4. <span data-ttu-id="04d8c-173">Přidejte následující hello kód toodefine a export objektu Task hello.</span><span class="sxs-lookup"><span data-stu-id="04d8c-173">Add hello following code toodefine and export hello Task object.</span></span> <span data-ttu-id="04d8c-174">Tento objekt je zodpovědná za připojení toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="04d8c-174">This object is responsible for connecting toohello table.</span></span>
   
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
5. <span data-ttu-id="04d8c-175">Přidejte následující kód toodefine další metody na objektu Task hello hello ty umožňují pracovat s daty uloženými v tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="04d8c-175">Add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in hello table:</span></span>
   
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
6. <span data-ttu-id="04d8c-176">Uložte a zavřete hello **task.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-176">Save and close hello **task.js** file.</span></span>

### <a name="create-a-controller"></a><span data-ttu-id="04d8c-177">Vytvořit řadič</span><span class="sxs-lookup"><span data-stu-id="04d8c-177">Create a controller</span></span>
<span data-ttu-id="04d8c-178">A *řadič* zpracovává požadavky HTTP a vykreslí hello HTML odpovědi.</span><span class="sxs-lookup"><span data-stu-id="04d8c-178">A *controller* handles HTTP requests and renders hello HTML response.</span></span>

1. <span data-ttu-id="04d8c-179">V hello **tasklist a směruje** adresáře, vytvořte nový soubor s názvem **tasklist.js** a otevřete v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-179">In hello **tasklist/routes** directory, create a new file named **tasklist.js** and open it in a text editor.</span></span>
2. <span data-ttu-id="04d8c-180">Přidejte následující kód příliš hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-180">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="04d8c-181">Tento kód načte azure a asynchronních hello moduly, které jsou používány **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-181">This loads hello azure and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="04d8c-182">Také definuje hello **TaskList** funkce, která se předá instance hello **úloh** definovaného dříve:</span><span class="sxs-lookup"><span data-stu-id="04d8c-182">This also defines hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>
   
        var azure = require('azure-storage');
        var async = require('async');
   
        module.exports = TaskList;
3. <span data-ttu-id="04d8c-183">Definování **TaskList** objektu.</span><span class="sxs-lookup"><span data-stu-id="04d8c-183">Define a **TaskList** object.</span></span>
   
        function TaskList(task) {
          this.task = task;
        }
4. <span data-ttu-id="04d8c-184">Přidejte následující metody příliš hello**TaskList**:</span><span class="sxs-lookup"><span data-stu-id="04d8c-184">Add hello following methods too**TaskList**:</span></span>
   
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

### <a name="modify-appjs"></a><span data-ttu-id="04d8c-185">Úprava souboru app.js</span><span class="sxs-lookup"><span data-stu-id="04d8c-185">Modify app.js</span></span>
1. <span data-ttu-id="04d8c-186">Z hello **tasklist** adresář, otevřete hello **app.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-186">From hello **tasklist** directory, open hello **app.js** file.</span></span> <span data-ttu-id="04d8c-187">Tento soubor byl vytvořen dříve spuštěním hello **express** příkaz.</span><span class="sxs-lookup"><span data-stu-id="04d8c-187">This file was created earlier by running hello **express** command.</span></span>
2. <span data-ttu-id="04d8c-188">Od začátku hello hello souboru, přidejte následující tooload hello azure modulu, název tabulky hello sady, klíč oddílu a nastavení hello úložiště přihlašovacích údajů používaných v tomto příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="04d8c-188">At hello beginning of hello file, add hello following tooload hello azure module, set hello table name, partition key, and set hello storage credentials used by this example:</span></span>
   
        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");
   
   > [!NOTE]
   > <span data-ttu-id="04d8c-189">nconf načte hodnoty konfigurace hello z proměnné prostředí nebo hello **config.json** souboru, který vytvoříme později.</span><span class="sxs-lookup"><span data-stu-id="04d8c-189">nconf will load hello configuration values from either environment variables or hello **config.json** file, which we will create later.</span></span>
   > 
   > 
3. <span data-ttu-id="04d8c-190">V souboru app.js hello, posuňte se dolů toowhere zobrazí hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="04d8c-190">In hello app.js file, scroll down toowhere you see hello following line:</span></span>
   
        app.use('/', routes);
        app.use('/users', users);
   
    <span data-ttu-id="04d8c-191">Nahraďte hello výše řádky kódu hello vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="04d8c-191">Replace hello above lines with hello code shown below.</span></span> <span data-ttu-id="04d8c-192">To bude inicializovat instanci <strong>úloh</strong> s účtem úložiště tooyour připojení.</span><span class="sxs-lookup"><span data-stu-id="04d8c-192">This will initialize an instance of <strong>Task</strong> with a connection tooyour storage account.</span></span> <span data-ttu-id="04d8c-193">To je předán toohello <strong>TaskList</strong>, který použije ji toocommunicate hello služby Table:</span><span class="sxs-lookup"><span data-stu-id="04d8c-193">This is passed toohello <strong>TaskList</strong>, which will use it toocommunicate with hello Table service:</span></span>
   
        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
4. <span data-ttu-id="04d8c-194">Uložit hello **app.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-194">Save hello **app.js** file.</span></span>

### <a name="modify-hello-index-view"></a><span data-ttu-id="04d8c-195">Upravit zobrazení indexu hello</span><span class="sxs-lookup"><span data-stu-id="04d8c-195">Modify hello index view</span></span>
1. <span data-ttu-id="04d8c-196">Otevřete hello **tasklist/views/index.jade** soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-196">Open hello **tasklist/views/index.jade** file in a text editor.</span></span>
2. <span data-ttu-id="04d8c-197">Nahraďte hello celý obsah souboru hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="04d8c-197">Replace hello entire contents of hello file with hello following code.</span></span> <span data-ttu-id="04d8c-198">Definuje zobrazení zobrazí existující úlohy, která obsahuje formulář pro přidání nové úlohy a označit stávajících jako dokončenou.</span><span class="sxs-lookup"><span data-stu-id="04d8c-198">This defines a view that displays existing tasks and includes a form for adding new tasks and marking existing ones as completed.</span></span>
   
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
3. <span data-ttu-id="04d8c-199">Uložte a zavřete **index.jade** souboru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-199">Save and close **index.jade** file.</span></span>

### <a name="modify-hello-global-layout"></a><span data-ttu-id="04d8c-200">Upravit globální rozložení hello</span><span class="sxs-lookup"><span data-stu-id="04d8c-200">Modify hello global layout</span></span>
<span data-ttu-id="04d8c-201">Hello **layout.jade** souboru v hello **zobrazení** adresář je globální šablona pro ostatní **.jade** soubory.</span><span class="sxs-lookup"><span data-stu-id="04d8c-201">hello **layout.jade** file in hello **views** directory is a global template for other **.jade** files.</span></span> <span data-ttu-id="04d8c-202">V tomto kroku jej upravíte toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), což je sada nástrojů, který umožňuje snadno toodesign dobrý vypadající webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-202">In this step you will modify it toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking web app.</span></span>

<span data-ttu-id="04d8c-203">Stažení a extrakci souborů hello [Twitter Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="04d8c-203">Download and extract hello files for [Twitter Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="04d8c-204">Kopírování hello **bootstrap.min.css** soubor z hello Bootstrap **šablon stylů css** složky do hello **veřejné nebo předlohy se styly** adresář vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-204">Copy hello **bootstrap.min.css** file from hello Bootstrap **css** folder into hello **public/stylesheets** directory of your application.</span></span>

<span data-ttu-id="04d8c-205">Z hello **zobrazení** složku, otevřete **layout.jade** a nahraďte celý obsah hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="04d8c-205">From hello **views** folder, open **layout.jade** and replace hello entire contents with hello following:</span></span>

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

### <a name="create-a-config-file"></a><span data-ttu-id="04d8c-206">Vytvoření konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="04d8c-206">Create a config file</span></span>
<span data-ttu-id="04d8c-207">toorun hello aplikaci místně do konfiguračního souboru jsme budete uveďte přihlašovací údaje Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="04d8c-207">toorun hello app locally, we'll put Azure Storage credentials into a config file.</span></span> <span data-ttu-id="04d8c-208">Vytvořte soubor s názvem **config.json* * s hello následující JSON:</span><span class="sxs-lookup"><span data-stu-id="04d8c-208">Create a file named **config.json* *with hello following JSON:</span></span>

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

<span data-ttu-id="04d8c-209">Nahraďte **název účtu úložiště** s názvem hello hello úložiště účet jste dříve vytvořili a nahraďte **přístupový klíč k úložišti** s hello primární přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="04d8c-209">Replace **storage account name** with hello name of hello storage account you created earlier, and replace **storage access key** with hello primary access key for your storage account.</span></span> <span data-ttu-id="04d8c-210">Například:</span><span class="sxs-lookup"><span data-stu-id="04d8c-210">For example:</span></span>

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

<span data-ttu-id="04d8c-211">Uložte tento soubor *jeden adresář úroveň vyšší* než hello **tasklist** adresáři, například takto:</span><span class="sxs-lookup"><span data-stu-id="04d8c-211">Save this file *one directory level higher* than hello **tasklist** directory, like this:</span></span>

    parent/
      |-- config.json
      |-- tasklist/

<span data-ttu-id="04d8c-212">Hello Důvodem tohoto postupu je tooavoid kontrola hello konfiguračního souboru do zdrojového kódu, kde mohou být veřejné.</span><span class="sxs-lookup"><span data-stu-id="04d8c-212">hello reason for doing this is tooavoid checking hello config file into source control, where it might become public.</span></span> <span data-ttu-id="04d8c-213">Když jsme nasadit tooAzure aplikace hello, budeme používat proměnné prostředí místo souboru config.</span><span class="sxs-lookup"><span data-stu-id="04d8c-213">When we deploy hello app tooAzure, we will use environment variables instead of a config file.</span></span>

## <a name="run-hello-application-locally"></a><span data-ttu-id="04d8c-214">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="04d8c-214">Run hello application locally</span></span>
<span data-ttu-id="04d8c-215">aplikace hello tootest na místním počítači, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="04d8c-215">tootest hello application on your local machine, perform hello following steps:</span></span>

1. <span data-ttu-id="04d8c-216">Z příkazového řádku hello, změňte adresáře toohello **tasklist** adresáře.</span><span class="sxs-lookup"><span data-stu-id="04d8c-216">From hello command-line, change directories toohello **tasklist** directory.</span></span>
2. <span data-ttu-id="04d8c-217">Použijte následující příkaz toolaunch hello aplikace místně hello:</span><span class="sxs-lookup"><span data-stu-id="04d8c-217">Use hello following command toolaunch hello application locally:</span></span>
   
        npm start
3. <span data-ttu-id="04d8c-218">Otevřete webový prohlížeč a přejděte toohttp://127.0.0.1:3000.</span><span class="sxs-lookup"><span data-stu-id="04d8c-218">Open a web browser and navigate toohttp://127.0.0.1:3000.</span></span>
   
    <span data-ttu-id="04d8c-219">Zobrazí se webová stránka podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="04d8c-219">A web page similar toohello following example appears.</span></span>
   
    ![Prázdný tasklist zobrazení webové stránky][node-table-finished]
4. <span data-ttu-id="04d8c-221">Zadejte název a kategorie toocreate novou položku seznamu úkolů a klikněte na tlačítko **přidat položku**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-221">toocreate a new to-do item, enter a name and category and click **Add Item**.</span></span> 
5. <span data-ttu-id="04d8c-222">toomark úlohu jako dokončení kontroly **Complete** a klikněte na tlačítko **úlohy aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-222">toomark a task as complete, check **Complete** and click **Update Tasks**.</span></span>
   
    ![Obrázek hello novou položku v seznamu hello úloh][node-table-list-items]

<span data-ttu-id="04d8c-224">I když hello aplikace běží místně, je při ukládání dat hello v hello služby Azure Table.</span><span class="sxs-lookup"><span data-stu-id="04d8c-224">Even though hello application is running locally, it is storing hello data in hello Azure Table service.</span></span>

## <a name="deploy-your-application-tooazure"></a><span data-ttu-id="04d8c-225">Nasazení vaší aplikace tooAzure</span><span class="sxs-lookup"><span data-stu-id="04d8c-225">Deploy your application tooAzure</span></span>
<span data-ttu-id="04d8c-226">Hello kroky v této části použijte nástroje příkazového řádku Azure toocreate hello novou webovou aplikaci ve službě App Service a potom pomocí Git toodeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-226">hello steps in this section use hello Azure command-line tools toocreate a new web app in App Service, and then use Git toodeploy your application.</span></span> <span data-ttu-id="04d8c-227">tooperform tyto kroky musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="04d8c-227">tooperform these steps you must have an Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="04d8c-228">Tyto kroky provádět také pomocí hello [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="04d8c-228">These steps can also be performed by using hello [Azure Portal](https://portal.azure.com/).</span></span> <span data-ttu-id="04d8c-229">V tématu [sestavení a nasazení webové aplikace Node.js ve službě Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="04d8c-229">See [Build and deploy a Node.js web app in Azure App Service].</span></span>
> 
> <span data-ttu-id="04d8c-230">Pokud je to hello první webové aplikace, které jste vytvořili, musíte použít portál Azure toodeploy hello této aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-230">If this is hello first web app you have created, you must use hello Azure Portal toodeploy this application.</span></span>
> 
> 

<span data-ttu-id="04d8c-231">tooget spustili, nainstalujte hello [rozhraní příkazového řádku Azure] tak, že zadáte následující příkaz z příkazového řádku hello hello:</span><span class="sxs-lookup"><span data-stu-id="04d8c-231">tooget started, install hello [Azure CLI] by entering hello following command from hello command line:</span></span>

    npm install azure-cli -g

### <a name="import-publishing-settings"></a><span data-ttu-id="04d8c-232">Import nastavení publikování</span><span class="sxs-lookup"><span data-stu-id="04d8c-232">Import publishing settings</span></span>
<span data-ttu-id="04d8c-233">V tomto kroku si stáhnete soubor obsahující informace o vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="04d8c-233">In this step, you will download a file containing information about your subscription.</span></span>

1. <span data-ttu-id="04d8c-234">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="04d8c-234">Enter hello following command:</span></span>
   
        azure login
   
    <span data-ttu-id="04d8c-235">Tento příkaz spustí prohlížeč a přejde toohello stránky pro stažení.</span><span class="sxs-lookup"><span data-stu-id="04d8c-235">This command launches a browser and navigates toohello download page.</span></span> <span data-ttu-id="04d8c-236">Pokud budete vyzváni, přihlaste se pomocí hello účet spojený s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="04d8c-236">If prompted, log in with hello account associated with your Azure subscription.</span></span>
   
    <!-- ![hello download page][download-publishing-settings] -->
   
    <span data-ttu-id="04d8c-237">Stažení souboru Hello začne automaticky. Pokud není, můžete kliknout na odkaz hello od začátku hello hello stránky toomanually stažení hello souboru.</span><span class="sxs-lookup"><span data-stu-id="04d8c-237">hello file download begins automatically; if it does not, you can click hello link at hello beginning of hello page toomanually download hello file.</span></span> <span data-ttu-id="04d8c-238">Hello souboru Poznámka hello cesta k souboru a uložte.</span><span class="sxs-lookup"><span data-stu-id="04d8c-238">Save hello file and note hello file path.</span></span>
2. <span data-ttu-id="04d8c-239">Zadejte následující příkaz tooimport hello nastavení hello:</span><span class="sxs-lookup"><span data-stu-id="04d8c-239">Enter hello following command tooimport hello settings:</span></span>
   
        azure account import <path-to-file>
   
    <span data-ttu-id="04d8c-240">Zadejte název a cesta k souboru hello Dobrý den publikování souboru nastavení, které jste stáhli v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="04d8c-240">Specify hello path and file name of hello publishing settings file you downloaded in hello previous step.</span></span>
3. <span data-ttu-id="04d8c-241">Po importu hello nastavení, odstranit hello soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="04d8c-241">After hello settings are imported, delete hello publish settings file.</span></span> <span data-ttu-id="04d8c-242">Je již nejsou potřebné a obsahuje citlivé informace týkající se vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="04d8c-242">It is no longer needed, and contains sensitive information regarding your Azure subscription.</span></span>

### <a name="create-an-app-service-web-app"></a><span data-ttu-id="04d8c-243">Vytvoření webové aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="04d8c-243">Create an App Service web app</span></span>
1. <span data-ttu-id="04d8c-244">Z příkazového řádku hello, změňte adresáře toohello **tasklist** adresáře.</span><span class="sxs-lookup"><span data-stu-id="04d8c-244">From hello command-line, change directories toohello **tasklist** directory.</span></span>
2. <span data-ttu-id="04d8c-245">Použijte následující příkaz toocreate nové webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="04d8c-245">Use hello following command toocreate a new web app.</span></span>
   
        azure site create --git
   
    <span data-ttu-id="04d8c-246">Zobrazí se výzva pro název webové aplikace hello a umístění.</span><span class="sxs-lookup"><span data-stu-id="04d8c-246">You will be prompted for hello web app name and location.</span></span> <span data-ttu-id="04d8c-247">Zadejte jedinečný název a vyberte hello stejné geografické oblasti jako váš účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="04d8c-247">Provide a unique name and select hello same geographical location as your Azure Storage account.</span></span>
   
    <span data-ttu-id="04d8c-248">Hello `--git` parametr vytvoří úložiště Git v Azure pro tuto webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04d8c-248">hello `--git` parameter creates a Git repository on Azure for this web app.</span></span> <span data-ttu-id="04d8c-249">Pokud žádný neexistuje a přidá také inicializuje úložiště Git v aktuálním adresáři hello [Git vzdálené] s názvem "azure", což je použité toopublish hello aplikace tooAzure.</span><span class="sxs-lookup"><span data-stu-id="04d8c-249">It also initializes a Git repository in hello current directory if none exists, and adds a [Git remote] named 'azure', which is used toopublish hello application tooAzure.</span></span> <span data-ttu-id="04d8c-250">Nakonec se vytvoří **web.config** soubor, který obsahuje nastavení používaná aplikací Azure toohost uzlu.</span><span class="sxs-lookup"><span data-stu-id="04d8c-250">Finally, it creates a **web.config** file, which contains settings used by Azure toohost node applications.</span></span> <span data-ttu-id="04d8c-251">Pokud vynecháte hello `--git` parametr ale hello adresář obsahuje úložiště Git, hello příkaz bude stále vytvořit vzdálené hello "azure".</span><span class="sxs-lookup"><span data-stu-id="04d8c-251">If you omit hello `--git` parameter but hello directory contains a Git repository, hello command will still create hello 'azure' remote.</span></span>
   
    <span data-ttu-id="04d8c-252">Po dokončení tohoto příkazu, zobrazí se výstup podobný toohello následující.</span><span class="sxs-lookup"><span data-stu-id="04d8c-252">Once this command has completed, you will see output similar toohello following.</span></span> <span data-ttu-id="04d8c-253">Všimněte si, že hello řádek začínající **web je vytvořen v** obsahuje hello URL pro webovou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="04d8c-253">Note that hello line beginning with **Website created at** contains hello URL for hello web app.</span></span>
   
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
   > <span data-ttu-id="04d8c-254">Pokud je to hello první webové aplikace App Service pro vaše předplatné, bude pokyn toouse hello portálu Azure toocreate hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-254">If this is hello first App Service web app for your subscription, you will be instructed toouse hello Azure Portal toocreate hello web app.</span></span> <span data-ttu-id="04d8c-255">Další informace najdete v tématu [sestavení a nasazení webové aplikace Node.js ve službě Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="04d8c-255">For more information, see [Build and deploy a Node.js web app in Azure App Service].</span></span>
   > 
   > 

### <a name="set-environment-variables"></a><span data-ttu-id="04d8c-256">Proměnné prostředí sady</span><span class="sxs-lookup"><span data-stu-id="04d8c-256">Set environment variables</span></span>
<span data-ttu-id="04d8c-257">V tomto kroku přidáte konfiguraci prostředí proměnné tooyour webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="04d8c-257">In this step, you will add environment variables tooyour web app configuration on Azure.</span></span>
<span data-ttu-id="04d8c-258">Hello příkazovém řádku zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="04d8c-258">From hello command line, enter hello following:</span></span>

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


<span data-ttu-id="04d8c-259">Nahraďte  **<storage account name>**  s názvem hello hello úložiště účet jste dříve vytvořili a nahraďte  **<storage access key>**  s hello primární přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="04d8c-259">Replace **<storage account name>** with hello name of hello storage account you created earlier, and replace **<storage access key>** with hello primary access key for your storage account.</span></span> <span data-ttu-id="04d8c-260">(Použít hello stejné hodnoty jako hello config.json soubor, který jste vytvořili dříve.)</span><span class="sxs-lookup"><span data-stu-id="04d8c-260">(Use hello same values as hello config.json file that you created earlier.)</span></span>

<span data-ttu-id="04d8c-261">Alternativně můžete nastavit proměnné prostředí v hello [portálu Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="04d8c-261">Alternatively, you can set environment variables in hello [Azure Portal](https://portal.azure.com/):</span></span>

1. <span data-ttu-id="04d8c-262">Otevřete okno hello webovou aplikaci kliknutím **Procházet** > **webové aplikace** > název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-262">Open hello web app's blade by clicking **Browse** > **Web Apps** > your web app name.</span></span>
2. <span data-ttu-id="04d8c-263">V okně vaší webové aplikace, klikněte na tlačítko **všechna nastavení** > **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-263">In your web app's blade, click **All Settings** > **Application Settings**.</span></span>
   
     <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->
3. <span data-ttu-id="04d8c-264">Projděte dolů toohello **nastavení aplikace** a přidejte hello páry klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="04d8c-264">Scroll down toohello **App settings** section and add hello key/value pairs.</span></span>
   
     ![Nastavení aplikace](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)
4. <span data-ttu-id="04d8c-266">Klikněte na **ULOŽIT**.</span><span class="sxs-lookup"><span data-stu-id="04d8c-266">Click **SAVE**.</span></span>

### <a name="publish-hello-application"></a><span data-ttu-id="04d8c-267">Publikování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="04d8c-267">Publish hello application</span></span>
<span data-ttu-id="04d8c-268">aplikace hello toopublish, potvrďte hello kód soubory tooGit a potom odešlete tooazure/hlavní.</span><span class="sxs-lookup"><span data-stu-id="04d8c-268">toopublish hello app, commit hello code files tooGit and then push tooazure/master.</span></span>

1. <span data-ttu-id="04d8c-269">Nastavte přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="04d8c-269">Set your deployment credentials.</span></span>
   
        azure site deployment user set <name> <password>
2. <span data-ttu-id="04d8c-270">Přidejte a potvrďte soubory aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-270">Add and commit your application files.</span></span>
   
        git add .
        git commit -m "adding files"
3. <span data-ttu-id="04d8c-271">Push hello potvrzení toohello webové aplikace App Service:</span><span class="sxs-lookup"><span data-stu-id="04d8c-271">Push hello commit toohello App Service web app:</span></span>
   
        git push azure master
   
    <span data-ttu-id="04d8c-272">Použití **hlavní** jako hello cílovou větev.</span><span class="sxs-lookup"><span data-stu-id="04d8c-272">Use **master** as hello target branch.</span></span> <span data-ttu-id="04d8c-273">Na konci hello hello nasazení zobrazí příkaz podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="04d8c-273">At hello end of hello deployment, you see a statement similar toohello following example:</span></span>
   
        toohttps://username@tabletasklist.azurewebsites.net/TableTasklist.git
          * [new branch]      master -> master
4. <span data-ttu-id="04d8c-274">Po dokončení operace push hello procházet toohello adresa URL webové aplikace dříve vrácený hello `azure create site` příkaz tooview vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="04d8c-274">Once hello push operation has completed, browse toohello web app URL returned previously by hello `azure create site` command tooview your application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04d8c-275">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04d8c-275">Next steps</span></span>
<span data-ttu-id="04d8c-276">Při hello kroky v tomto článku popisují, pomocí informací o toostore hello služby Table, můžete také použít [MongoDB](https://mlab.com/azure/).</span><span class="sxs-lookup"><span data-stu-id="04d8c-276">While hello steps in this article describe using hello Table Service toostore information, you can also use [MongoDB](https://mlab.com/azure/).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="04d8c-277">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="04d8c-277">Additional resources</span></span>
<span data-ttu-id="04d8c-278">[rozhraní příkazového řádku Azure]</span><span class="sxs-lookup"><span data-stu-id="04d8c-278">[Azure CLI]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="04d8c-279">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="04d8c-279">What's changed</span></span>
* <span data-ttu-id="04d8c-280">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="04d8c-280">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
