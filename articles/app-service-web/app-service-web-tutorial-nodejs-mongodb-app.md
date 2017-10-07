---
title: aaaBuild webovou aplikaci Node.js a MongoDB v Azure | Microsoft Docs
description: "Zjistěte, jak tooget aplikace Node.js v Azure funguje, s tooa připojení Cosmos DB databáze s připojovacím řetězcem MongoDB."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 532251c51ed6f8513e6e366393e889b67a85e5b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a>Vytvoření webové aplikace Node.js a MongoDB v Azure

Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. Tento kurz ukazuje, jak toocreate Node.js webové aplikace v Azure a připojte ho tooa databázi MongoDB. Když jste hotovi, budete mít střední aplikace (MongoDB, Express, AngularJS a Node.js) spuštěná v [Azure App Service](app-service-web-overview.md). Pro jednoduchost, hello ukázková aplikace používá hello [MEAN.js webová architektura](http://meanjs.org/).

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Získáte informace:

> [!div class="checklist"]
> * Vytvořit databázi MongoDB v Azure
> * Připojit tooMongoDB aplikace Node.js
> * Nasazení aplikace tooAzure hello
> * Aktualizovat hello datový model a znovu nasaďte aplikace hello
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikace hello v hello portálu Azure

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

1. [Nainstalovat Git](https://git-scm.com/).
1. [Nainstalovat Node.js a NPM](https://nodejs.org/).
1. [Nainstalujte Gulp.js](http://gulpjs.com/) (požadavku [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))
1. [Nainstalujte a spusťte MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="test-local-mongodb"></a>Test místní MongoDB

Hello otevřete okno terminálu a `cd` toohello `bin` adresář instalace MongoDB. Všechny příkazy hello toto okno terminálu toorun můžete použít v tomto kurzu.

Spustit `mongo` v hello terminálu tooconnect tooyour místního serveru MongoDB.

```bash
mongo
```

Pokud připojení úspěšné, pak databázi MongoDB je již spuštěna. Pokud ne, ujistěte se, zda je spuštěná místní databázi MongoDB pomocí následujících kroků hello v [nainstalujte MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/). Často je nainstalovaná MongoDB, ale stále potřebujete toostart ho spuštěním `mongod`. 

Po dokončení testování vaší databázi MongoDB, zadejte `Ctrl+C` v terminálu hello. 

## <a name="create-local-nodejs-app"></a>Vytvořit místní aplikace Node.js

V tomto kroku nastavíte místní projekt Node.js hello.

### <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

V okně terminálu hello `cd` tooa pracovní adresář.  

Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

Tato ukázka úložiště obsahuje kopii hello [MEAN.js úložiště](https://github.com/meanjs/mean). Je upravený toorun v App Service (Další informace najdete v tématu hello MEAN.js úložiště [souboru README](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).

### <a name="run-hello-application"></a>Spuštění aplikace hello

Spusťte následující příkazy tooinstall hello požadované balíčky hello a spustit aplikaci hello.

```bash
cd meanjs
npm install
npm start
```

Když je aplikace hello úplným načtením, uvidíte něco podobné toohello následující zprávou:

```
--
MEAN.JS - Development Environment

Environment:     development
Server:          http://0.0.0.0:3000
Database:        mongodb://localhost/mean-dev
App version:     0.5.0
MEAN.JS version: 0.5.0
--
```

Přejděte toohttp://localhost:3000 v prohlížeči. Klikněte na tlačítko **zaregistrovat** v hello horní nabídce a vytvoření zkušebního uživatele. 

Hello MEAN.js ukázkové aplikace ukládá data uživatele v databázi hello. Pokud jste při vytváření uživatele a přihlášení úspěšné, je vaše aplikace zápis dat toohello místní databázi MongoDB.

![MEAN.js připojí úspěšně tooMongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

Vyberte **správce > Správa článků** tooadd některé články.

stiskněte klávesu Node.js kdykoli toostop `Ctrl+C` v terminálu hello. 

## <a name="create-production-mongodb"></a>Vytvořit produkční MongoDB

V tomto kroku vytvoříte databázi MongoDB v Azure. Pokud je vaše aplikace nasazené tooAzure, používá tato databáze cloudu.

Pro MongoDB, tento kurz používá [Azure Cosmos DB](/azure/documentdb/). Cosmos DB podporuje připojení klienta MongoDB.

### <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Hello Azure CLI 2.0 toocreate hello prostředky potřebné toohost budete používat aplikace v Azure. Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

Hello následující příklad vytvoří skupinu prostředků v oblasti západní Evropa hello.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Použití hello [míst seznamu služby App Service az](/cli/azure/appservice#list-locations) rozhraní příkazového řádku Azure příkaz toolist dostupná umístění. 

### <a name="create-a-cosmos-db-account"></a>Vytvoření účtu Cosmos DB

Vytvořte účet Cosmos DB s hello [vytvořit az cosmosdb](/cli/azure/cosmosdb#create) příkaz.

Následující příkaz a nahraďte v hello jedinečný název databáze Cosmos hello  *\<cosmosdb_name >* zástupný symbol. Tento název se používá jako součást hello hello Cosmos DB koncového bodu, `https://<cosmosdb_name>.documents.azure.com/`, takže název hello musí toobe jedinečný mezi všechny Cosmos DB účty v Azure. Hello název musí obsahovat jenom malá písmena, číslice a znak hello pomlčku (-) a musí být v rozmezí 3 až 50 znaků.

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

Hello *– druhu MongoDB* parametr povolí připojení klientů MongoDB.

Při vytvoření hello Cosmos DB účet je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:

```json
{
  "consistencyPolicy":
  {
    "defaultConsistencyLevel": "Session",
    "maxIntervalInSeconds": 5,
    "maxStalenessPrefix": 100
  },
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.com:443/",
  "failoverPolicies": 
  ...
  < Output truncated for readability >
}
```

## <a name="connect-app-tooproduction-mongodb"></a>Připojení aplikace tooproduction MongoDB

V tomto kroku připojíte MEAN.js ukázkové aplikace toohello Cosmos DB databázi, kterou jste právě vytvořili, pomocí připojovacího řetězce MongoDB. 

### <a name="retrieve-hello-database-key"></a>Načíst klíč databáze hello

tooconnect toohello Cosmos DB databáze, je nutné klíč databáze hello. Použití hello [az cosmosdb seznamu klíčů](/cli/azure/cosmosdb#list-keys) příkaz tooretrieve hello primární klíč.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

Hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace:

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Zkopírujte hodnotu hello `primaryMasterKey`. Je třeba tyto informace v dalším kroku hello.

<a name="devconfig"></a>
### <a name="configure-hello-connection-string-in-your-nodejs-application"></a>Konfigurace v aplikaci Node.js hello připojovací řetězec

Ve svém úložišti MEAN.js otevřete _config/env/production.js_.

V hello `db` objektu, aktualizujte hodnotu hello `uri`:

* Nahraďte hello dva  *\<cosmosdb_name >* zástupných symbolů nahraďte názvem databáze Cosmos DB.
* Nahraďte hello  *\<primary_master_key >* zástupný text klíčem hello jste zkopírovali v předchozím kroku hello.

Hello následující kód ukazuje hello `db` objektu:

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

Hello `ssl=true` možnost je povinná, protože [Cosmos DB vyžaduje SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 

Uložte provedené změny.

### <a name="test-hello-application-in-production-mode"></a>Testovací aplikace hello v produkčním režimu 

Spusťte následující příkaz toominify a sady skriptů pro produkční prostředí hello hello. Tento proces generuje soubory hello vyžaduje hello produkčního prostředí.

```bash
gulp prod
```

Spuštění hello následující příkaz toouse hello připojovací řetězec jste nakonfigurovali v _config/env/production.js_.

```bash
NODE_ENV=production node server.js
```

`NODE_ENV=production`Nastaví proměnné prostředí hello, která říká službě Node.js toorun hello produkčního prostředí.  `node server.js`Spustí hello Node.js server s `server.js` v kořenovém úložišti. Toto je, jak aplikace Node.js je načten do platformy Azure. 

Pokud aplikace hello je načtena, zkontrolujte toomake se, že je spuštěna v provozním prostředí hello:

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

Přejděte toohttp://localhost:8443 v prohlížeči. Klikněte na tlačítko **zaregistrovat** v hello horní nabídce a vytvoření zkušebního uživatele. Pokud jste vytváření uživatele a přihlášení úspěšné, pak aplikace zapisuje data toohello Cosmos DB databáze v Azure. 

V terminálu hello, zastavte Node.js zadáním `Ctrl+C`. 

## <a name="deploy-app-tooazure"></a>Nasazení aplikace tooAzure

V tomto kroku nasadíte tooAzure vaše připojení MongoDB Node.js aplikace služby App Service.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

Vytvořte plán služby App Service s hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) příkaz. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Hello následující příklad vytvoří plán služby App Service s názvem _myAppServicePlan_ pomocí hello **volné** cenové úrovně:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Při vytvoření hello plán služby App Service je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:

```json 
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
```

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

Vytvoření webové aplikace v hello `myAppServicePlan` plán služby App Service se hello [az webapp vytvořit](/cli/azure/webapp#create) příkaz. 

Hello webové aplikace poskytuje můžete hostování místo toodeploy kódu a poskytuje adresu URL pro vás tooview hello nasazené aplikace. Použití toocreate hello webové aplikace. 

Následující příkaz a nahraďte v hello hello  *\<app_name >* zástupný symbol s jedinečným názvem aplikace. Tento název se používá jako součást hello hello výchozí adresa URL pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure App Service. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Po vytvoření webové aplikace hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-an-environment-variable"></a>Nakonfigurujte proměnné prostředí

Výše v hello kurz, je pevně zakódované hello připojovací řetězec databáze v _config/env/production.js_. V souladu s nejlepším způsobem zabezpečení chcete tookeep tyto důvěrné osobní údaje z úložiště Git. Pro vaši aplikaci běžící v Azure budete používat proměnné prostředí.

Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí hello [aktualizovat az webapp konfigurace appsettings](/cli/azure/webapp/config/appsettings#update) příkaz. 

Hello následující příklad konfiguruje `MONGODB_URI` nastavení aplikace v Azure webové aplikace. Nahraďte hello  *\<app_name >*,  *\<cosmosdb_name >*, a  *\<primary_master_key >* zástupné symboly.

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

V kódu Node.js, je přístup k nastavení této aplikace s `process.env.MONGODB_URI`, stejně, jako by přístup všechny proměnné prostředí. 

Teď vrátit zpět too_config/env/production.js_ vaše změny se hello následující příkaz:

```bash
git checkout -- .
```

Otevřete _config/env/production.js_ znovu. Všimněte si, že hello výchozí MEAN.js aplikace je již nakonfigurované toouse hello `MONGODB_URI` proměnné prostředí, který jste vytvořili.

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a>Konfigurace nasazení místního gitu 

Použití hello [nastavený uživatel nasazení webapp az](/cli/azure/webapp/deployment/user#set) příkaz toocreate přihlašovací údaje pro nasazení.

Můžete nasadit aplikace tooAzure služby App Service různými způsoby, včetně FTP, Git místní, GitHub, Visual Studio Team Services a BitBucket. Pro místní Git a FTP, je nutné toohave uživatele nasazení nakonfigurovali na serveru tooauthenticate hello nasazení. Tento uživatel nasazení je úrovni účtu a se liší od účtu předplatného Azure. Potřebujete jenom tooconfigure tohoto uživatele nasazení jednou.

Následující příkaz a nahraďte v hello  *\<uživatelské jméno >* a  *\<heslo >* s nové uživatelské jméno a heslo. Hello uživatelské jméno musí být jedinečný. Hello heslo musí obsahovat alespoň osm znaků, se dvěma hello následující tři prvky: písmena, symboly a čísla. Pokud dojde ` 'Conflict'. Details: 409` chyby, uživatelské jméno změnit hello. Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

Záznam hello uživatelské jméno a heslo pro použití v dalších krocích při nasazení aplikace hello.

Použití hello [az webapp nasazení zdroj konfigurace místní git](/cli/azure/webapp/deployment/source#config-local-git) příkaz tooconfigure místní Git přístup toohello webové aplikace Azure. 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

Pokud je nakonfigurován uživatel nasazení hello, hello rozhraní příkazového řádku Azure zobrazuje hello URL nasazení pro Azure webové aplikace v hello následující formát:

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

Kopírovat výstup z terminálu hello hello se použije v dalším kroku hello. 

### <a name="push-tooazure-from-git"></a>Z Git push tooAzure

Přidejte místní úložiště Git Azure vzdálené tooyour. 

```bash
git remote add azure <paste_copied_url_here> 
```

Push toohello Azure vzdálené toodeploy aplikace Node.js. Jste vyzváni k hello heslo, které jste zadali dříve v rámci vytváření hello hello nasazení uživatele. 

```bash
git push azure master
```

Během nasazení Azure App Service komunikuje s Gitem jejím průběhu.

```bash
Counting objects: 5, done.
Delta compression using up too4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

Můžete si všimnout, že proces nasazení hello spouští [Gulp](http://gulpjs.com/) po `npm install`. Služby App Service nespouští Gulp nebo Grunt úloh během nasazení, tak toto úložiště ukázka má dva další soubory v jeho kořenový adresář tooenable ho: 

- _.Deployment_ – tento soubor informuje služby App Service toorun `bash deploy.sh` jako hello vlastní nasazení skriptu.
- _Deploy.SH_ -hello vlastní nasazení skriptu. Při kontrole souboru hello je se zobrazí, že běží `gulp prod` po `npm install` a `bower install`. 

Tento přístup tooadd můžete použít všechny krok tooyour nasazení na základě Git. Pokud restartujete Azure webové aplikace v libovolném bodě, služby App Service není spusťte znovu tyto úlohy automatizace.

### <a name="browse-toohello-azure-web-app"></a>Procházet toohello webové aplikace Azure 

Procházet toohello nasadit webovou aplikaci pomocí webového prohlížeče. 

```bash 
http://<app_name>.azurewebsites.net 
``` 

Klikněte na tlačítko **zaregistrovat** v hello horní nabídce a vytvořte fiktivní uživatele. 

Pokud jste úspěšné a aplikace hello automaticky přihlásí toohello vytvoří uživatele a potom MEAN.js aplikace v Azure má databázi MongoDB (Cosmos DB) toohello připojení. 

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Vyberte **správce > Správa článků** tooadd některé články. 

**Blahopřejeme!** Používáte datové aplikace Node.js ve službě Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Aktualizace datový model a znovu ho zaveďte

V tomto kroku změnit hello `article` dat modelu a publikovat tooAzure vaše změny.

### <a name="update-hello-data-model"></a>Aktualizace hello datový model

Otevřete _modules/articles/server/models/article.server.model.js_.

V `ArticleSchema`, přidejte `String` typu s názvem `comment`. Když jste hotovi, schéma kódu by měla vypadat například takto:

```javascript
var ArticleSchema = new Schema({
  ...,
  user: {
    type: Schema.ObjectId,
    ref: 'User'
  },
  comment: {
    type: String,
    default: '',
    trim: true
  }
});
```

### <a name="update-hello-articles-code"></a>Aktualizujte kód články hello

Aktualizovat zbytek hello vaše `articles` code toouse `comment`.

Je pět souborů, které budete potřebovat toomodify: hello serveru řadiče a zobrazení hello čtyři klientů. 

Otevřete _modules/articles/server/controllers/articles.server.controller.js_.

V hello `update` fungovat, přidejte přiřazení pro `article.comment`. Hello následující kód ukazuje hello Dokončit `update` funkce:

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

Otevřete _modules/articles/client/views/view-article.client.view.html_.

Nad hello ukončovací `</section>` značky, přidejte následující řádek toodisplay hello `comment` společně s hello zbytek hello článku dat:

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

Otevřete _modules/articles/client/views/list-articles.client.view.html_.

Nad hello ukončovací `</a>` značky, přidejte následující řádek toodisplay hello `comment` společně s hello zbytek hello článku dat:

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

Otevřete _modules/articles/client/views/admin/list-articles.client.view.html_.

Uvnitř hello `<div class="list-group">` elementu a nad hello ukončovací `</a>` značky, přidejte následující řádek toodisplay hello `comment` společně s hello zbytek hello článku dat:

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

Otevřete _modules/articles/client/views/admin/form-article.client.view.html_.

Najde hello `<div class="form-group">` elementu, který obsahuje tlačítko pro odeslání hello, jež vypadá takto:

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

Nad tuto značku, přidejte další `<div class="form-group">` element, který umožňuje uživatelům upravit hello `comment` pole. Nového elementu by měl vypadat takto:

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a>Otestujte provedené změny místně

Uložte všechny provedené změny.

Vyzkoušejte změny v produkčním režimu znovu.

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> Nezapomeňte, že vaše _config/env/production.js_ byl vrácen a hello `MONGODB_URI` proměnná prostředí je nastavit pouze v Azure web app a ne na místním počítači. Pokud se podíváte na hello konfiguračního souboru, zjistíte, že hello produkční konfigurace výchozí toouse místní databázi MongoDB. Tím je zajištěno, že nemáte touch provozních dat při testování změn kódu místně.

Přejděte příliš`http://localhost:8443` v prohlížeči a ujistěte se, že jste přihlášení.

Vyberte **správce > Správa článků**, pak výběrem hello přidat článek  **+**  tlačítko.

Zobrazí hello nové `Comment` nyní textové pole.

![Pole tooArticles přidané komentář](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

V terminálu hello, zastavte Node.js zadáním `Ctrl+C`. 

### <a name="publish-changes-tooazure"></a>Publikování změn tooAzure

Potvrdit změny v úložišti Git a potom push tooAzure změny kódu hello.

```bash
git commit -am "added article comment"
git push azure master
```

Jednou hello `git push` dokončení, přejděte tooyour webové aplikace Azure a vyzkoušet nové funkce hello.

![Model a databáze změny publikovány tooAzure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

Pokud jste dříve přidali všechny články, je stále můžete vidíte. Stávající data v databázi vaší Cosmos není ztraceny. Navíc schéma data aktualizace toohello a svoje existující data zůstanou zachovány.

## <a name="stream-diagnostic-logs"></a>Diagnostické protokoly datového proudu 

Při spuštění vaší aplikace Node.js ve službě Azure App Service můžete získat hello konzoly protokoly vytvoření kanálu tooyour terminálu. Tímto způsobem můžete získat hello stejné diagnostické zprávy toohelp ladění chyb aplikace.

toostart protokolu streamování, použijte hello [az webapp protokolu poškozené databáze za](/cli/azure/webapp/log#tail) příkaz.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

Po zahájení vysílání datového proudu protokolu, aktualizujte Azure webové aplikace v prohlížeči tooget hello některé webový provoz. Nyní uvidíte, protokoly konzoly přesměruje tooyour terminálu.

Zastavení protokolu streamování kdykoli zadáním `Ctrl+C`. 

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte toohello [portál Azure](https://portal.azure.com) toosee hello webovou aplikaci jste vytvořili.

V levé nabídce hello, klikněte na **App Services**, pak klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

Ve výchozím nastavení, hello portál zobrazuje vaší webové aplikace **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Hello karty na levé straně stránky hello hello zobrazit stránky hello jinou konfiguraci, které můžete otevřít.

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Další kroky

Co jste se naučili:

> [!div class="checklist"]
> * Vytvořit databázi MongoDB v Azure
> * Připojit tooMongoDB aplikace Node.js
> * Nasazení aplikace tooAzure hello
> * Aktualizovat hello datový model a znovu nasaďte aplikace hello
> * Datový proud protokolů z Azure tooyour terminálu
> * Spravovat aplikace hello v hello portálu Azure

Posunutí toohello další kurz toolearn jak toomap vlastní DNS název tooyour webové aplikace.

> [!div class="nextstepaction"] 
> [Mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md)
