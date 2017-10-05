---
title: "Vytvoření webové aplikace Node.js a MongoDB v Azure | Microsoft Docs"
description: "Další informace o získání aplikace Node.js v Azure, funguje s připojením k databázi Cosmos DB s připojovacím řetězcem MongoDB."
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
ms.openlocfilehash: 3b309382be8cdf8d48b396207fd482a5dc5ed934
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a>Vytvoření webové aplikace Node.js a MongoDB v Azure

Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. Tento kurz ukazuje, jak vytvořit webovou aplikaci Node.js v Azure a připojte ho k databázi MongoDB. Když jste hotovi, budete mít střední aplikace (MongoDB, Express, AngularJS a Node.js) spuštěná v [Azure App Service](app-service-web-overview.md). Pro jednoduchost, ukázková aplikace používá [MEAN.js webová architektura](http://meanjs.org/).

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Získáte informace:

> [!div class="checklist"]
> * Vytvořit databázi MongoDB v Azure
> * Připojení aplikace Node.js pro MongoDB
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikaci na portálu Azure

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

1. [Nainstalovat Git](https://git-scm.com/).
1. [Nainstalovat Node.js a NPM](https://nodejs.org/).
1. [Nainstalujte Gulp.js](http://gulpjs.com/) (požadavku [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))
1. [Nainstalujte a spusťte MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="test-local-mongodb"></a>Test místní MongoDB

Otevřete okno terminálu a `cd` k `bin` adresář instalace MongoDB. Chcete-li spustit všechny příkazy v tomto kurzu můžete toto okno terminálu.

Spustit `mongo` v terminálu pro připojení k místní server MongoDB.

```bash
mongo
```

Pokud připojení úspěšné, pak databázi MongoDB je již spuštěna. Pokud ne, ujistěte se, zda je spuštěná místní databázi MongoDB podle kroků v [nainstalujte MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/). Často je nainstalovaná MongoDB, ale potřebujete spusťte ji spuštěním `mongod`. 

Po dokončení testování vaší databázi MongoDB, zadejte `Ctrl+C` v terminálu. 

## <a name="create-local-nodejs-app"></a>Vytvořit místní aplikace Node.js

V tomto kroku nastavíte místní projekt Node.js.

### <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

V okně terminálu `cd` do pracovního adresáře.  

Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

Tato ukázka úložiště obsahuje kopii [MEAN.js úložiště](https://github.com/meanjs/mean). Je upravit pro spouštění v App Service (Další informace najdete v tématu úložiště MEAN.js [souboru README](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).

### <a name="run-the-application"></a>Spuštění aplikace

Spusťte následující příkazy pro instalaci požadovaných balíčků a spusťte aplikaci.

```bash
cd meanjs
npm install
npm start
```

Po úplným načtením aplikace se zobrazí podobná následující zpráva:

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

Přejděte na http://localhost: 3000 v prohlížeči. Klikněte na tlačítko **zaregistrovat** v horní nabídce a vytvoření zkušebního uživatele. 

Ukázková aplikace MEAN.js ukládá data uživatelů v databázi. Pokud jste při vytváření uživatele a přihlášení úspěšné, pak aplikace je zápis dat do místní databáze MongoDB.

![Aplikace MEAN.js se úspěšně připojí k databázi MongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

Vyberte **správce > Správa článků** přidat některé články.

Kdykoli zastavit Node.js, stiskněte klávesu `Ctrl+C` v terminálu. 

## <a name="create-production-mongodb"></a>Vytvořit produkční MongoDB

V tomto kroku vytvoříte databázi MongoDB v Azure. Po nasazení aplikace do Azure se používá tato databáze cloudu.

Pro MongoDB, tento kurz používá [Azure Cosmos DB](/azure/documentdb/). Cosmos DB podporuje připojení klienta MongoDB.

### <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Pomocí rozhraní Azure CLI 2.0 vytvoříte prostředky potřebné k hostování vaší aplikace v Azure. Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

Následující příklad vytvoří skupinu prostředků pro oblast Západní Evropa.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Použití [az služby App Service seznamu umístění](/cli/azure/appservice#list-locations) příkaz rozhraní příkazového řádku Azure k seznamu dostupných umístění. 

### <a name="create-a-cosmos-db-account"></a>Vytvoření účtu Cosmos DB

Vytvoření účtu Cosmos DB s [vytvořit az cosmosdb](/cli/azure/cosmosdb#create) příkaz.

V následujícím příkazu nahraďte jedinečný název databáze Cosmos  *\<cosmosdb_name >* zástupný symbol. Tento název se používá jako součást Cosmos DB koncového bodu, `https://<cosmosdb_name>.documents.azure.com/`, takže název musí být jedinečný v rámci všech Cosmos DB účty v Azure. Název musí obsahovat jenom malá písmena, číslice a pomlčky (-) a musí být v rozmezí 3 až 50 znaků.

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

*– Druhu MongoDB* parametr povolí připojení klientů MongoDB.

Při vytvoření účtu Cosmos DB rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:

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

## <a name="connect-app-to-production-mongodb"></a>Připojení aplikace do produkčního prostředí MongoDB

V tomto kroku připojíte MEAN.js ukázkovou aplikaci do databáze Cosmos databáze, kterou jste právě vytvořili, pomocí připojovacího řetězce MongoDB. 

### <a name="retrieve-the-database-key"></a>Načíst klíč databáze

Pro připojení k databázi Cosmos DB, musíte klíč databáze. Pro načtení primárního klíče použijte příkaz [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys).

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

Rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Zkopírujte hodnotu `primaryMasterKey`. Tyto informace budete potřebovat v dalším kroku.

<a name="devconfig"></a>
### <a name="configure-the-connection-string-in-your-nodejs-application"></a>Konfigurace připojovacího řetězce v aplikaci Node.js

Ve svém úložišti MEAN.js otevřete _config/env/production.js_.

V `db` objektu, aktualizujte hodnotu `uri`:

* Nahraďte dva  *\<cosmosdb_name >* zástupných symbolů nahraďte názvem databáze Cosmos DB.
* Nahraďte  *\<primary_master_key >* zástupný text klíčem, který jste zkopírovali v předchozím kroku.

Následující kód ukazuje `db` objektu:

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

`ssl=true` Možnost je povinná, protože [Cosmos DB vyžaduje SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 

Uložte provedené změny.

### <a name="test-the-application-in-production-mode"></a>Testování aplikace v provozním režimu 

Spusťte následující příkaz k minifikaci a sady skriptů pro produkční prostředí. Tento proces generuje soubory potřebné v provozním prostředí.

```bash
gulp prod
```

Pomocí následujícího příkazu použít připojovací řetězec, který jste nakonfigurovali v _config/env/production.js_.

```bash
NODE_ENV=production node server.js
```

`NODE_ENV=production`Nastaví proměnné prostředí, která sděluje Node.js ke spuštění v produkčním prostředí.  `node server.js`Spustí server Node.js s `server.js` v kořenovém úložišti. Toto je, jak aplikace Node.js je načten do platformy Azure. 

Když aplikace je načtena, ujistěte se, zda je spuštěna v provozním prostředí:

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

Přejděte do http://localhost:8443 v prohlížeči. Klikněte na tlačítko **zaregistrovat** v horní nabídce a vytvoření zkušebního uživatele. Pokud jste vytváření uživatele a přihlášení úspěšné, pak aplikace je zápis dat do databáze Cosmos DB v Azure. 

V terminálu, zastavte Node.js zadáním `Ctrl+C`. 

## <a name="deploy-app-to-azure"></a>Nasazení aplikace do Azure

V tomto kroku nasadíte aplikace Node.js MongoDB připojení do služby Azure App Service.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Následující příklad vytvoří plán služby App Service s názvem _myAppServicePlan_ pomocí **volné** cenové úrovně:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Když je vytvořen plán služby App Service, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:

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

Vytvoření webové aplikace v `myAppServicePlan` plán služby App Service pomocí [az webapp vytvořit](/cli/azure/webapp#create) příkaz. 

Webová aplikace získáte hostování místa k nasazení kódu a poskytuje adresu URL zobrazení nasazené aplikace. Použijte k vytvoření webové aplikace. 

V následujícím příkazu nahraďte  *\<app_name >* zástupný symbol s jedinečným názvem aplikace. Tento název se používá jako součást výchozí adresa URL pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure App Service. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Po vytvoření webové aplikace se v rozhraní příkazového řádku Azure CLI zobrazí podobné informace jako v následujícím příkladu: 

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

V tomto kurzu jste pevně zakódované připojení k databázi řetězce v _config/env/production.js_. V souladu s nejlepším způsobem zabezpečení, které chcete uchovat tyto důvěrné osobní údaje z úložiště Git. Pro vaši aplikaci běžící v Azure budete používat proměnné prostředí.

Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí [aktualizovat az webapp konfigurace appsettings](/cli/azure/webapp/config/appsettings#update) příkaz. 

Následující příklad konfiguruje `MONGODB_URI` nastavení aplikace v Azure webové aplikace. Nahraďte  *\<app_name >*,  *\<cosmosdb_name >*, a  *\<primary_master_key >* zástupné symboly.

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

V kódu Node.js, je přístup k nastavení této aplikace s `process.env.MONGODB_URI`, stejně, jako by přístup všechny proměnné prostředí. 

Teď vrátit zpět všechny změny _config/env/production.js_ pomocí následujícího příkazu:

```bash
git checkout -- .
```

Otevřete _config/env/production.js_ znovu. Všimněte si, že výchozí MEAN.js aplikace je již nakonfigurován pro použití `MONGODB_URI` proměnné prostředí, který jste vytvořili.

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a>Konfigurace nasazení místního gitu 

Použití [nastavený uživatel nasazení webapp az](/cli/azure/webapp/deployment/user#set) příkazu vytvořit přihlašovací údaje pro nasazení.

Můžete nasadit aplikace do služby Azure App Service různými způsoby, včetně FTP, Git místní, GitHub, Visual Studio Team Services a BitBucket. Pro místní Git a FTP je potřeba mít nasazení uživateli nakonfigurovanému na serveru k ověření nasazení. Tento uživatel nasazení je úrovni účtu a se liší od účtu předplatného Azure. Potřebujete jenom jednou konfiguraci tohoto nasazení uživatele.

V následujícím příkazu nahraďte hodnoty *\<user-name>* a *\<password>* novým uživatelským jménem a heslem. Uživatelské jméno musí být jedinečné. Heslo musí obsahovat aspoň osm znaků a musí v něm být použity minimálně dva z následujících typů znaků: písmena, čísla a symboly. Pokud se zobrazí chyba ` 'Conflict'. Details: 409`, změňte uživatelské jméno. Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

Záznam uživatelského jména a hesla pro použití v dalších krocích při nasazení aplikace.

Použití [az webapp nasazení zdroj konfigurace místní git](/cli/azure/webapp/deployment/source#config-local-git) příkaz, který nakonfiguruje místní Git přístup k webové aplikace Azure. 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

Pokud uživatel nasazení je nakonfigurován, rozhraní příkazového řádku Azure zobrazuje adresu URL nasazení pro Azure webové aplikace v následujícím formátu:

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

Zkopírujte výstup z terminálu, jako se použije v dalším kroku. 

### <a name="push-to-azure-from-git"></a>Přenos z Gitu do Azure

Přidejte vzdálené úložiště Azure do místního úložiště Gitu. 

```bash
git remote add azure <paste_copied_url_here> 
```

Doručte do Azure vzdálené nasazení aplikace Node.js. Zobrazí se výzva k zadání hesla, které jste zadali dříve v rámci vytváření nasazení uživatele. 

```bash
git push azure master
```

Během nasazení Azure App Service komunikuje s Gitem jejím průběhu.

```bash
Counting objects: 5, done.
Delta compression using up to 4 threads.
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
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

Můžete si všimnout, že proces nasazení spouští [Gulp](http://gulpjs.com/) po `npm install`. Služby App Service nespustí Gulp nebo Grunt úloh během nasazení, takže toto úložiště ukázka má dva další soubory v jeho kořenový adresář povolit: 

- _.Deployment_ – tento soubor informuje služby App Service ke spuštění `bash deploy.sh` jako vlastní nasazení skriptu.
- _Deploy.SH_ -skript vlastní nasazení. Při kontrole souboru je se zobrazí, že běží `gulp prod` po `npm install` a `bower install`. 

Tento postup můžete použít k přidání jakéhokoli kroku k nasazení na základě Git. Pokud restartujete Azure webové aplikace v libovolném bodě, služby App Service není spusťte znovu tyto úlohy automatizace.

### <a name="browse-to-the-azure-web-app"></a>Přejděte do webové aplikace Azure 

Přejděte do nasazené webové aplikace pomocí webového prohlížeče. 

```bash 
http://<app_name>.azurewebsites.net 
``` 

Klikněte na tlačítko **zaregistrovat** v horní nabídce a vytvořte fiktivní uživatele. 

Pokud jste úspěšné a aplikace automaticky přihlásí do vytvořeného uživatele, MEAN.js aplikace v Azure má připojení k databázi MongoDB (Cosmos databáze). 

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Vyberte **správce > Správa článků** přidat některé články. 

**Blahopřejeme!** Používáte datové aplikace Node.js ve službě Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Aktualizace datový model a znovu ho zaveďte

V tomto kroku, můžete změnit `article` dat modelu a publikovat změny do Azure.

### <a name="update-the-data-model"></a>Aktualizovat data modelu

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

### <a name="update-the-articles-code"></a>Aktualizujte kód články

Aktualizovat zbytek vaší `articles` kódu pro použití `comment`.

Je pět souborů, budete muset upravit: řadičem serveru a zobrazení čtyři klientů. 

Otevřete _modules/articles/server/controllers/articles.server.controller.js_.

V `update` fungovat, přidejte přiřazení pro `article.comment`. Následující kód ukazuje dokončené `update` funkce:

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

Nad uzavírací `</section>` značky, přidejte následující řádek zobrazíte `comment` spolu s ostatními data článku:

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

Otevřete _modules/articles/client/views/list-articles.client.view.html_.

Nad uzavírací `</a>` značky, přidejte následující řádek zobrazíte `comment` spolu s ostatními data článku:

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

Otevřete _modules/articles/client/views/admin/list-articles.client.view.html_.

Uvnitř `<div class="list-group">` elementu a nad uzavírací `</a>` značky, přidejte následující řádek zobrazíte `comment` spolu s ostatními data článku:

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

Otevřete _modules/articles/client/views/admin/form-article.client.view.html_.

Najít `<div class="form-group">` elementu, který obsahuje tlačítko pro odeslání, který vypadá podobně jako tento:

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

Nad tuto značku, přidejte další `<div class="form-group">` element, který umožňuje uživatelům upravit `comment` pole. Nového elementu by měl vypadat takto:

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
> Nezapomeňte, že vaše _config/env/production.js_ obnovila a `MONGODB_URI` – proměnná prostředí je nastavit pouze v Azure web app a ne na místním počítači. Pokud se podíváte na konfigurační soubor, zjistíte, že konfigurace produkční výchozí použít místní databázi MongoDB. Tím je zajištěno, že nemáte touch provozních dat při testování změn kódu místně.

Přejděte na `http://localhost:8443` v prohlížeči a ujistěte se, že jste přihlášení.

Vyberte **správce > Správa článků**, pak výběrem přidat článek  **+**  tlačítko.

Zobrazí nové `Comment` nyní textové pole.

![Přidání komentářů pole článků](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

V terminálu, zastavte Node.js zadáním `Ctrl+C`. 

### <a name="publish-changes-to-azure"></a>Publikování změn do Azure

Potvrdit změny v úložišti Git a potom odešlete změny kódu do Azure.

```bash
git commit -am "added article comment"
git push azure master
```

Jednou `git push` je dokončení, přejděte do vaší webové aplikace Azure a vyzkoušet nové funkce.

![Model a databáze změny, které jsou publikovány do služby Azure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

Pokud jste dříve přidali všechny články, je stále můžete vidíte. Stávající data v databázi vaší Cosmos není ztraceny. Navíc vaše aktualizace schématu dat a svoje existující data zůstanou zachovány.

## <a name="stream-diagnostic-logs"></a>Diagnostické protokoly datového proudu 

Při spuštění vaší aplikace Node.js ve službě Azure App Service můžete získat protokoly konzoly přesměruje do terminálu. Tímto způsobem můžete získat stejné diagnostické zprávy pomoci při ladění chyb aplikace.

Spusťte protokolu streamování pomocí [az webapp protokolu poškozené databáze za](/cli/azure/webapp/log#tail) příkaz.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

Po zahájení vysílání datového proudu protokolu, aktualizujte Azure webové aplikace v prohlížeči získat některé webový provoz. Zobrazí protokoly konzoly přesměruje do terminálu.

Zastavení protokolu streamování kdykoli zadáním `Ctrl+C`. 

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte na [portál Azure](https://portal.azure.com) zobrazíte webové aplikace, které jste vytvořili.

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

Ve výchozím nastavení, zobrazí na portálu vaší webové aplikace **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Karty na levé straně stránky zobrazí stránek jinou konfiguraci, že můžete otevřít.

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Další kroky

Co jste se naučili:

> [!div class="checklist"]
> * Vytvořit databázi MongoDB v Azure
> * Připojení aplikace Node.js pro MongoDB
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Datový proud protokolů z Azure terminálu
> * Spravovat aplikaci na portálu Azure

Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do vaší webové aplikace.

> [!div class="nextstepaction"] 
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](app-service-web-tutorial-custom-domain.md)
