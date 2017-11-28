---
title: "aplikace aaaNode.js rozhraní API v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak toocreate rozhraní Node.js RESTful API a nasadit aplikace tooan rozhraní API v Azure App Service."
services: app-service\api
documentationcenter: node
author: bradygaster
manager: erikre
editor: 
ms.assetid: a820e400-06af-4852-8627-12b3db4a8e70
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: get-started-article
ms.date: 06/13/2017
ms.author: rachelap
ms.openlocfilehash: 3b3229c1453b6ca4d06bef26f476e92afda4e244
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-tooan-api-app-in-azure"></a><span data-ttu-id="75dd4-103">Sestavení rozhraní Node.js RESTful API a nasadit aplikace tooan rozhraní API v Azure</span><span class="sxs-lookup"><span data-stu-id="75dd4-103">Build a Node.js RESTful API and deploy it tooan API app in Azure</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="75dd4-104">Tento rychlý start zobrazuje jak toocreate rozhraní REST API, napsané pomocí Node.js [Express](http://expressjs.com/)pomocí [Swagger](http://swagger.io/) definice a nasazením jako [aplikace API](app-service-api-apps-why-best-platform.md) v Azure.</span><span class="sxs-lookup"><span data-stu-id="75dd4-104">This quickstart shows how toocreate a REST API, written with Node.js [Express](http://expressjs.com/), using a [Swagger](http://swagger.io/) definition and deploying it as an [API app](app-service-api-apps-why-best-platform.md) on Azure.</span></span> <span data-ttu-id="75dd4-105">Vytvoření aplikace hello pomocí nástroje příkazového řádku, nakonfigurujte prostředky s hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)a nasazení aplikace hello pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="75dd4-105">You create hello app using command-line tools, configure resources with hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and deploy hello app using Git.</span></span>  <span data-ttu-id="75dd4-106">Po dokončení budete mít funkční ukázkové rozhraní REST API, které běží na Azure.</span><span class="sxs-lookup"><span data-stu-id="75dd4-106">When you've finished, you have a working sample REST API running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75dd4-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="75dd4-107">Prerequisites</span></span>

* [<span data-ttu-id="75dd4-108">Git</span><span class="sxs-lookup"><span data-stu-id="75dd4-108">Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="75dd4-109">Node.js a NPM</span><span class="sxs-lookup"><span data-stu-id="75dd4-109">Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="75dd4-110">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="75dd4-110">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="75dd4-111">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="75dd4-111">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="75dd4-112">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="75dd4-112">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-your-environment"></a><span data-ttu-id="75dd4-113">Příprava prostředí</span><span class="sxs-lookup"><span data-stu-id="75dd4-113">Prepare your environment</span></span>

1. <span data-ttu-id="75dd4-114">Okno terminálu spusťte následující příkaz tooclone hello ukázka tooyour místního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="75dd4-114">In a terminal window, run hello following command tooclone hello sample tooyour local machine.</span></span>

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. <span data-ttu-id="75dd4-115">Změnit toohello adresář, který obsahuje hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="75dd4-115">Change toohello directory that contains hello sample code.</span></span>

    ```bash
    cd app-service-api-node-contact-list
    ```

3. <span data-ttu-id="75dd4-116">Na místní počítač nainstalujte [Swaggerize](https://www.npmjs.com/package/swaggerize-express).</span><span class="sxs-lookup"><span data-stu-id="75dd4-116">Install [Swaggerize](https://www.npmjs.com/package/swaggerize-express) on your local machine.</span></span> <span data-ttu-id="75dd4-117">Swaggerize je nástroj, který generuje kód Node.js pro REST API z definice Swaggeru.</span><span class="sxs-lookup"><span data-stu-id="75dd4-117">Swaggerize is a tool that generates Node.js code for your REST API from a Swagger definition.</span></span>

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a><span data-ttu-id="75dd4-118">Generování kódu Node.js</span><span class="sxs-lookup"><span data-stu-id="75dd4-118">Generate Node.js code</span></span> 

<span data-ttu-id="75dd4-119">V této části kurzu hello modely rozhraní API pracovní postup vývoje ve kterém nejprve vytvoříte Swagger metadata a použít tuto tooscaffold (automaticky generovat) serverový kód pro hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="75dd4-119">This section of hello tutorial models an API development workflow in which you create Swagger metadata first and use that tooscaffold (auto-generate) server code for hello API.</span></span> 

<span data-ttu-id="75dd4-120">Změňte adresář toohello *spustit* složky, poté spusťte `yo swaggerize`.</span><span class="sxs-lookup"><span data-stu-id="75dd4-120">Change directory toohello *start* folder, then run `yo swaggerize`.</span></span> <span data-ttu-id="75dd4-121">Swaggerize vytvoří projekt Node.js pro vaše rozhraní API z definici Swaggeru hello v *api.json*.</span><span class="sxs-lookup"><span data-stu-id="75dd4-121">Swaggerize creates a Node.js project for your API from hello Swagger definition in *api.json*.</span></span>

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

<span data-ttu-id="75dd4-122">Pokud se Swaggerize zeptá na název projektu, použijte *ContactList*.</span><span class="sxs-lookup"><span data-stu-id="75dd4-122">When Swaggerize asks for a project name, use *ContactList*.</span></span>
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like toocall this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-hello-project-code"></a><span data-ttu-id="75dd4-123">Přizpůsobení hello projektu kódu</span><span class="sxs-lookup"><span data-stu-id="75dd4-123">Customize hello project code</span></span>

1. <span data-ttu-id="75dd4-124">Kopírování hello *lib* složky do hello *ContactList* vytvořené složky `yo swaggerize`, potom změnit adresář, do *ContactList*.</span><span class="sxs-lookup"><span data-stu-id="75dd4-124">Copy hello *lib* folder into hello *ContactList* folder created by `yo swaggerize`, then change directory into *ContactList*.</span></span>

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. <span data-ttu-id="75dd4-125">Nainstalujte hello `jsonpath` a `swaggerize-ui` moduly NPM.</span><span class="sxs-lookup"><span data-stu-id="75dd4-125">Install hello `jsonpath` and `swaggerize-ui` NPM modules.</span></span> 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. <span data-ttu-id="75dd4-126">Nahraďte kód hello v hello *handlers/contacts.js* s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="75dd4-126">Replace hello code in hello *handlers/contacts.js* with hello following code:</span></span> 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    <span data-ttu-id="75dd4-127">Tento kód používá hello JSON data uložená v *lib/contacts.json* obsloužených *lib/contactRepository.js*.</span><span class="sxs-lookup"><span data-stu-id="75dd4-127">This code uses hello JSON data stored in *lib/contacts.json* served by *lib/contactRepository.js*.</span></span> <span data-ttu-id="75dd4-128">Hello nové *contacts.js* kód vrátí všechny kontakty v úložišti hello jako datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="75dd4-128">hello new *contacts.js* code returns all contacts in hello repository as a JSON payload.</span></span> 

4. <span data-ttu-id="75dd4-129">Nahraďte kód hello v hello **handlers/contacts/{id}.js** soubor s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="75dd4-129">Replace hello code in hello **handlers/contacts/{id}.js** file with hello following code:</span></span>

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    <span data-ttu-id="75dd4-130">Tento kód vám umožní používat proměnné tooreturn cesta pouze hello kontakt s dané ID.</span><span class="sxs-lookup"><span data-stu-id="75dd4-130">This code lets you use a path variable tooreturn only hello contact with a given ID.</span></span>

5. <span data-ttu-id="75dd4-131">Nahraďte kód hello v **server.js** s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="75dd4-131">Replace hello code in **server.js** with hello following code:</span></span>

    ```javascript
    'use strict';

    var port = process.env.PORT || 8000; 

    var http = require('http');
    var express = require('express');
    var bodyParser = require('body-parser');
    var swaggerize = require('swaggerize-express');
    var swaggerUi = require('swaggerize-ui'); 
    var path = require('path');
    var fs = require("fs");
    
    fs.existsSync = fs.existsSync || require('path').existsSync;

    var app = express();

    var server = http.createServer(app);

    app.use(bodyParser.json());

    app.use(swaggerize({
        api: path.resolve('./config/swagger.json'),
        handlers: path.resolve('./handlers'),
        docspath: '/swagger' 
    }));

    // change four
    app.use('/docs', swaggerUi({
        docs: '/swagger'  
    }));

    server.listen(port, function () { 
    });
    ```   

    <span data-ttu-id="75dd4-132">Tento kód provede některé toolet malé změny se pracovat s Azure App Service a zveřejňuje interaktivní webové rozhraní pro vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="75dd4-132">This code makes some small changes toolet it work with Azure App Service and exposes an interactive web interface for your API.</span></span>

### <a name="test-hello-api-locally"></a><span data-ttu-id="75dd4-133">Test hello místně rozhraní API</span><span class="sxs-lookup"><span data-stu-id="75dd4-133">Test hello API locally</span></span>

1. <span data-ttu-id="75dd4-134">Spuštění aplikace Node.js hello</span><span class="sxs-lookup"><span data-stu-id="75dd4-134">Start up hello Node.js app</span></span>
    ```bash
    npm start
    ```
    
2. <span data-ttu-id="75dd4-135">Procházet toohttp://localhost:8000 / kontaktuje tooview hello JSON pro celý seznam kontaktů hello.</span><span class="sxs-lookup"><span data-stu-id="75dd4-135">Browse toohttp://localhost:8000/contacts tooview hello JSON for hello entire contact list.</span></span>
   
   ```json
    {
        "id": 1,
        "name": "Barney Poland",
        "email": "barney@contoso.com"
    },
    {
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    },
    {
        "id": 3,
        "name": "Lora Riggs",
        "email": "lora@contoso.com"
    }
   ```

3. <span data-ttu-id="75dd4-136">Procházet toohttp://localhost:8000/contacts/2 tooview hello kontaktu s `id` dva.</span><span class="sxs-lookup"><span data-stu-id="75dd4-136">Browse toohttp://localhost:8000/contacts/2 tooview hello contact with an `id` of two.</span></span>
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. <span data-ttu-id="75dd4-137">Otestovat pomocí na http://localhost: 8000/docs hello Swagger webové rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="75dd4-137">Test hello API using hello Swagger web interface at http://localhost:8000/docs.</span></span>
   
    ![Webové rozhraní Swaggeru](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <span data-ttu-id="75dd4-139"><a id="createapiapp"></a> Vytvoření aplikace API</span><span class="sxs-lookup"><span data-stu-id="75dd4-139"><a id="createapiapp"></a> Create an API App</span></span>

<span data-ttu-id="75dd4-140">V této části použijte hello Azure CLI 2.0 toocreate hello prostředky toohost hello rozhraní API v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="75dd4-140">In this section, you use hello Azure CLI 2.0 toocreate hello resources toohost hello API on Azure App Service.</span></span> 

1.  <span data-ttu-id="75dd4-141">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="75dd4-141">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

    ```azurecli-interactive
    az login
    ```

2. <span data-ttu-id="75dd4-142">Pokud máte víc předplatných Azure, požadované změny hello výchozí předplatné toohello jeden.</span><span class="sxs-lookup"><span data-stu-id="75dd4-142">If you have multiple Azure subscriptions, change hello default subscription toohello desired one.</span></span>

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-hello-api-with-git"></a><span data-ttu-id="75dd4-143">Nasadit hello rozhraní API s Gitem</span><span class="sxs-lookup"><span data-stu-id="75dd4-143">Deploy hello API with Git</span></span>

<span data-ttu-id="75dd4-144">Nasazení aplikace API toohello kód vynucením potvrzení z vaší místní tooAzure úložiště Git služby App Service.</span><span class="sxs-lookup"><span data-stu-id="75dd4-144">Deploy your code toohello API app by pushing commits from your local Git repository tooAzure App Service.</span></span>

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. <span data-ttu-id="75dd4-145">Inicializace nové úložiště v hello *ContactList* adresáře.</span><span class="sxs-lookup"><span data-stu-id="75dd4-145">Initialize a new repo in hello *ContactList* directory.</span></span> 

    ```bash
    git init .
    ```

3. <span data-ttu-id="75dd4-146">Vyloučit hello *node_modules* adresář vytvořený npm v předchozím kroku v kurzu hello z Git.</span><span class="sxs-lookup"><span data-stu-id="75dd4-146">Exclude hello *node_modules* directory created by npm in an earlier step in hello tutorial from Git.</span></span> <span data-ttu-id="75dd4-147">Vytvořte novou `.gitignore` v aktuálním adresáři hello soubor a přidejte následující text na nový řádek kdekoli v souboru hello hello.</span><span class="sxs-lookup"><span data-stu-id="75dd4-147">Create a new `.gitignore` file in hello current directory and add hello following text on a new line anywhere in hello file.</span></span>

    ```
    node_modules/
    ```
    <span data-ttu-id="75dd4-148">Potvrďte hello `node_modules` složky je ignorován s `git status`.</span><span class="sxs-lookup"><span data-stu-id="75dd4-148">Confirm hello `node_modules` folder is being ignored with  `git status`.</span></span>

4. <span data-ttu-id="75dd4-149">Potvrďte hello změny toohello úložišti.</span><span class="sxs-lookup"><span data-stu-id="75dd4-149">Commit hello changes toohello repo.</span></span>
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push tooAzure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-hello-api--in-azure"></a><span data-ttu-id="75dd4-150">Test hello rozhraní API v Azure</span><span class="sxs-lookup"><span data-stu-id="75dd4-150">Test hello API  in Azure</span></span>

1. <span data-ttu-id="75dd4-151">Otevřete prohlížeč toohttp://app_name.azurewebsites.net/contacts.</span><span class="sxs-lookup"><span data-stu-id="75dd4-151">Open a browser toohttp://app_name.azurewebsites.net/contacts.</span></span> <span data-ttu-id="75dd4-152">Zobrazí hello stejné JSON vrácena jako když jste provedli hello požadavek místně v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="75dd4-152">You see hello same JSON returned as when you made hello request locally earlier in hello tutorial.</span></span>

   ```json
   {
       "id": 1,
       "name": "Barney Poland",
       "email": "barney@contoso.com"
   },
   {
       "id": 2,
       "name": "Lacy Barrera",
       "email": "lacy@contoso.com"
   },
   {
       "id": 3,
       "name": "Lora Riggs",
       "email": "lora@contoso.com"
   }
   ```

2. <span data-ttu-id="75dd4-153">Přejděte v prohlížeči, toohello `http://app_name.azurewebsites.net/docs` tootry koncový bod se hello uživatelské rozhraní Swagger spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="75dd4-153">In a browser, go toohello `http://app_name.azurewebsites.net/docs` endpoint tootry out hello Swagger UI running on Azure.</span></span>

    ![Uživatelské rozhraní Swaggeru](media/app-service-api-nodejs-api-app/swagger-azure-ui.png)

    <span data-ttu-id="75dd4-155">Teď můžete nasadit aktualizace toohello ukázkové rozhraní API tooAzure jednoduchým nuceným doručením úložiště Git v Azure toohello potvrzení.</span><span class="sxs-lookup"><span data-stu-id="75dd4-155">You can now deploy updates toohello sample API tooAzure simply by pushing commits toohello Azure Git repository.</span></span>

## <a name="clean-up"></a><span data-ttu-id="75dd4-156">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="75dd4-156">Clean up</span></span>

<span data-ttu-id="75dd4-157">tooclean systémové prostředky hello vytvořené v této rychlé spuštění, spusťte následující příkaz rozhraní příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="75dd4-157">tooclean up hello resources created in this quickstart, run hello following Azure CLI command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a><span data-ttu-id="75dd4-158">Další krok</span><span class="sxs-lookup"><span data-stu-id="75dd4-158">Next step</span></span> 
> [!div class="nextstepaction"]
> [<span data-ttu-id="75dd4-159">Využití aplikace API z JavaScriptu pomocí CORS</span><span class="sxs-lookup"><span data-stu-id="75dd4-159">Consume API apps from JavaScript clients with CORS</span></span>](app-service-api-cors-consume-javascript.md)

