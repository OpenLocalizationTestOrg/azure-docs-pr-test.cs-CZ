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
# <a name="build-a-nodejs-restful-api-and-deploy-it-tooan-api-app-in-azure"></a>Sestavení rozhraní Node.js RESTful API a nasadit aplikace tooan rozhraní API v Azure
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Tento rychlý start zobrazuje jak toocreate rozhraní REST API, napsané pomocí Node.js [Express](http://expressjs.com/)pomocí [Swagger](http://swagger.io/) definice a nasazením jako [aplikace API](app-service-api-apps-why-best-platform.md) v Azure. Vytvoření aplikace hello pomocí nástroje příkazového řádku, nakonfigurujte prostředky s hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)a nasazení aplikace hello pomocí Git.  Po dokončení budete mít funkční ukázkové rozhraní REST API, které běží na Azure.

## <a name="prerequisites"></a>Požadavky

* [Git](https://git-scm.com/)
* [Node.js a NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="prepare-your-environment"></a>Příprava prostředí

1. Okno terminálu spusťte následující příkaz tooclone hello ukázka tooyour místního počítače hello.

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. Změnit toohello adresář, který obsahuje hello ukázkový kód.

    ```bash
    cd app-service-api-node-contact-list
    ```

3. Na místní počítač nainstalujte [Swaggerize](https://www.npmjs.com/package/swaggerize-express). Swaggerize je nástroj, který generuje kód Node.js pro REST API z definice Swaggeru.

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a>Generování kódu Node.js 

V této části kurzu hello modely rozhraní API pracovní postup vývoje ve kterém nejprve vytvoříte Swagger metadata a použít tuto tooscaffold (automaticky generovat) serverový kód pro hello rozhraní API. 

Změňte adresář toohello *spustit* složky, poté spusťte `yo swaggerize`. Swaggerize vytvoří projekt Node.js pro vaše rozhraní API z definici Swaggeru hello v *api.json*.

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

Pokud se Swaggerize zeptá na název projektu, použijte *ContactList*.
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like toocall this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-hello-project-code"></a>Přizpůsobení hello projektu kódu

1. Kopírování hello *lib* složky do hello *ContactList* vytvořené složky `yo swaggerize`, potom změnit adresář, do *ContactList*.

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. Nainstalujte hello `jsonpath` a `swaggerize-ui` moduly NPM. 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. Nahraďte kód hello v hello *handlers/contacts.js* s hello následující kód: 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    Tento kód používá hello JSON data uložená v *lib/contacts.json* obsloužených *lib/contactRepository.js*. Hello nové *contacts.js* kód vrátí všechny kontakty v úložišti hello jako datové části JSON. 

4. Nahraďte kód hello v hello **handlers/contacts/{id}.js** soubor s hello následující kód:

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    Tento kód vám umožní používat proměnné tooreturn cesta pouze hello kontakt s dané ID.

5. Nahraďte kód hello v **server.js** s hello následující kód:

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

    Tento kód provede některé toolet malé změny se pracovat s Azure App Service a zveřejňuje interaktivní webové rozhraní pro vaše rozhraní API.

### <a name="test-hello-api-locally"></a>Test hello místně rozhraní API

1. Spuštění aplikace Node.js hello
    ```bash
    npm start
    ```
    
2. Procházet toohttp://localhost:8000 / kontaktuje tooview hello JSON pro celý seznam kontaktů hello.
   
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

3. Procházet toohttp://localhost:8000/contacts/2 tooview hello kontaktu s `id` dva.
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. Otestovat pomocí na http://localhost: 8000/docs hello Swagger webové rozhraní API hello.
   
    ![Webové rozhraní Swaggeru](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a> Vytvoření aplikace API

V této části použijte hello Azure CLI 2.0 toocreate hello prostředky toohost hello rozhraní API v Azure App Service. 

1.  Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.

    ```azurecli-interactive
    az login
    ```

2. Pokud máte víc předplatných Azure, požadované změny hello výchozí předplatné toohello jeden.

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-hello-api-with-git"></a>Nasadit hello rozhraní API s Gitem

Nasazení aplikace API toohello kód vynucením potvrzení z vaší místní tooAzure úložiště Git služby App Service.

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. Inicializace nové úložiště v hello *ContactList* adresáře. 

    ```bash
    git init .
    ```

3. Vyloučit hello *node_modules* adresář vytvořený npm v předchozím kroku v kurzu hello z Git. Vytvořte novou `.gitignore` v aktuálním adresáři hello soubor a přidejte následující text na nový řádek kdekoli v souboru hello hello.

    ```
    node_modules/
    ```
    Potvrďte hello `node_modules` složky je ignorován s `git status`.

4. Potvrďte hello změny toohello úložišti.
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push tooAzure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-hello-api--in-azure"></a>Test hello rozhraní API v Azure

1. Otevřete prohlížeč toohttp://app_name.azurewebsites.net/contacts. Zobrazí hello stejné JSON vrácena jako když jste provedli hello požadavek místně v kurzu hello.

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

2. Přejděte v prohlížeči, toohello `http://app_name.azurewebsites.net/docs` tootry koncový bod se hello uživatelské rozhraní Swagger spuštěné v Azure.

    ![Uživatelské rozhraní Swaggeru](media/app-service-api-nodejs-api-app/swagger-azure-ui.png)

    Teď můžete nasadit aktualizace toohello ukázkové rozhraní API tooAzure jednoduchým nuceným doručením úložiště Git v Azure toohello potvrzení.

## <a name="clean-up"></a>Vyčištění

tooclean systémové prostředky hello vytvořené v této rychlé spuštění, spusťte následující příkaz rozhraní příkazového řádku Azure hello:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a>Další krok 
> [!div class="nextstepaction"]
> [Využití aplikace API z JavaScriptu pomocí CORS](app-service-api-cors-consume-javascript.md)

