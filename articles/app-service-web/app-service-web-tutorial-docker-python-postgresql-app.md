---
title: "aaaBuild Docker Pythonu a PostgreSQL webové aplikace v Azure | Microsoft Docs"
description: "Zjistěte, jak tooget Docker Python aplikace v Azure funguje s připojení tooa PostgreSQL databáze."
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
editor: 
ms.assetid: 2bada123-ef18-44e5-be71-e16323b20466
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: e594ef9ec8c04ef2bf725e5f998691f3fb8cf815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Vytvoření webové aplikace Docker Python a PostgreSQL v Azure

Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. Tento kurz ukazuje, jak toocreate základní Python Docker webové aplikace v Azure. Budete připojit databázi PostgreSQL tooa této aplikace. Když jste hotovi, budete mít k aplikaci Python Flask systémem v rámci kontejner Docker [Azure App Service Web Apps](app-service-web-overview.md).

![Docker Python Flask aplikace v Azure App Service](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

V systému macOS můžete provést následující postup hello. Pokyny pro Linux a Windows jsou stejné hello ve většině případů, ale hello rozdíly nejsou popsané v tomto kurzu.
 
## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

1. [Nainstalovat Git](https://git-scm.com/).
1. [Nainstalovat Python](https://www.python.org/downloads/).
1. [Nainstalujte a spusťte PostgreSQL](https://www.postgresql.org/download/)
1. [Nainstalujte Docker Community Edition](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testování místní instalaci PostgreSQL a vytvořit databázi

Otevřete okno terminálu hello a spusťte `psql postgres` tooconnect tooyour místní PostgreSQL server.

```bash
psql postgres
```

Pokud připojení úspěšné, je databáze PostgreSQL spuštěna. Pokud ne, ujistěte se, zda je spuštěná místní databázi PostgresQL podle následujících kroků hello v [stáhne - PostgreSQL základní distribuční](https://www.postgresql.org/download/).

Vytvoření databáze názvem *eventregistration* a nastavení uživatele samostatné databáze s názvem *manager* heslem *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```
Typ *\q* tooexit hello PostgreSQL klienta. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Vytvořit místní aplikace Python Flask

V tomto kroku nastavíte místní projekt Python Flask hello.

### <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Hello otevřete okno terminálu, a `CD` tooa pracovní adresář.  

Hello spusťte následující příkazy tooclone hello Ukázka úložiště a přejděte toohello *0,1 initialapp* verzi.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Tato ukázka úložiště obsahuje [Flask](http://flask.pocoo.org/) aplikace. 

### <a name="run-hello-application"></a>Spuštění aplikace hello

> [!NOTE] 
> Později tento proces zjednodušit podle budovy toouse kontejner Docker s hello produkční databázi.

Instalace hello požadované balíčky a spuštění aplikace hello.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Když je aplikace hello úplným načtením, uvidíte něco podobné toohello následující zprávou:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

Přejděte toohttp://127.0.0.1:5000 v prohlížeči. Klikněte na tlačítko **zaregistrovat!** a vytvoření zkušebního uživatele.

![Aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

Hello Flask ukázkové aplikace ukládá data uživatele v databázi hello. Pokud jste úspěšné při registraci uživatele, aplikace je zápis dat toohello místní databázi PostgreSQL.

server Flask hello toostop v kdykoli, zadejte Ctrl + C hello terminálu. 

## <a name="create-a-production-postgresql-database"></a>Vytvořit databázi PostgreSQL výroby

V tomto kroku vytvoříte databázi PostgreSQL v Azure. Pokud je vaše aplikace nasazené tooAzure, použije tuto databázi cloudu.

### <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Jste nyní probíhající toouse hello Azure CLI 2.0 toocreate hello prostředky potřebné toohost aplikace Python ve službě Azure App Service.  Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů. 

```azurecli
az login 
``` 
   
### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s hello [vytvořit skupinu az](/cli/azure/group#create). 

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

Hello následující příklad vytvoří skupinu prostředků v oblasti západní USA hello:

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

Použití hello [míst seznamu služby App Service az](/cli/azure/appservice#list-locations) rozhraní příkazového řádku Azure příkaz toolist dostupná umístění.

### <a name="create-an-azure-database-for-postgresql-server"></a>Vytvoření serveru Azure Database for PostgreSQL

Vytvoření serveru PostgreSQL s hello [az postgres server vytvořit](/cli/azure/documentdb#create) příkaz.

V hello následující příkaz, nahraďte název jedinečný server hello  *\<postgresql_name >* zástupný symbol a uživatelské jméno pro hello  *\<admin_username >* zástupný symbol . název serveru Hello se používá jako součást váš koncový bod PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), takže název hello musí toobe jedinečné ve všech serverech v Azure. Hello uživatelské jméno je hello počáteční databáze správce uživatelského účtu. Jste výzvami toopick heslo pro tohoto uživatele.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

Při vytvoření hello Azure databáze pro PostgreSQL server je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-hello-azure-database-for-postgresql-server"></a>Vytvořte pravidlo brány firewall pro hello Azure databáze PostgreSQL serveru

Spusťte následující příkaz příkazového řádku Azure CLI, tooallow databázi toohello access ze všech IP adres hello.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

Hello rozhraní příkazového řádku Azure potvrdí hello vytvoření pravidla brány firewall se výstup podobný toohello následující ukázka:

```json
{
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

## <a name="connect-your-python-flask-application-toohello-database"></a>Připojit databáze toohello aplikace Python Flask

V tomto kroku připojíte vaší Python Flask ukázkové aplikace toohello Azure databáze PostgreSQL serveru, který jste vytvořili.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Vytvořit prázdnou databázi a nastavení nového uživatele databáze aplikace

Vytvořte uživatele databáze s tooa jedné databáze access jenom. Tyto přihlašovací údaje tooavoid poskytnutí hello aplikační úplný přístup toohello server budete používat.

Připojte databáze toohello (se zobrazí výzva k zadání hesla správce).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Vytvoření databáze hello a uživatele z hello PostgreSQL rozhraní příkazového řádku.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```

Typ *\q* tooexit hello PostgreSQL klienta.

### <a name="test-hello-application-locally-against-hello-azure-postgresql-database"></a>Testování aplikace hello místně na databázi Azure PostgreSQL hello 

Návratem teď toohello *aplikace* složky hello klonovat úložiště Github, můžete spustit aplikace Python Flask hello aktualizací proměnné prostředí hello databáze.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Když je aplikace hello úplným načtením, uvidíte něco podobné toohello následující zprávou:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

Přejděte toohttp://127.0.0.1:5000 v prohlížeči. Klikněte na tlačítko **zaregistrovat!** a vytvořit testovací registrace. Databáze toohello data jsou nyní zápisu v Azure.

![Aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-hello-application-from-a-docker-container"></a>Spuštění aplikace hello z kontejner Docker

Sestavení bitové kopie kontejner Docker hello.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker zobrazí potvrzení tohoto kontejneru hello it byl úspěšně vytvořen.

```bash
Successfully built 7548f983a36b
```

Přidání databáze prostředí proměnné tooan prostředí proměnné souboru *db.env*. aplikace Hello připojí produkční databázi PostgreSQL toohello v Azure.

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

Spuštění aplikace hello z v rámci kontejner Docker hello. Hello následující příkaz určuje hello souboru proměnných prostředí a mapuje hello výchozí Flask port 5000 toolocal port 5000.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

výstup Hello je podobné toowhat, které jste viděli dříve. Migrace počáteční databáze hello však již nepotřebuje toobe provést a proto se přeskočí.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C tooquit)
```

Hello databáze již obsahuje hello registrace, které jste vytvořili dříve.

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-hello-docker-container-tooa-container-registry"></a>Nahrát hello Docker kontejneru tooa kontejneru registru

V tomto kroku nahrát hello Docker kontejneru tooa kontejneru registru. Budete používat Azure kontejneru registru, ale můžete také použít další oblíbených těch, jako je například Docker Hub.

### <a name="create-an-azure-container-registry"></a>Vytvoření služby Azure Container Registry

V následující příkaz toocreate registru kontejneru hello nahraďte  *\<registry_name >* s názvem registru jedinečný kontejner Azure podle svého výběru.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Výstup
```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-hello-registry-credentials-for-pushing-and-pulling-docker-images"></a>Načíst hello registru pověření pro vkládání a stahování imagí Dockeru

přihlašovací údaje tooshow registru, nejprve povolte režim správce.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Zobrazí dvě hesla. Poznamenejte si hello uživatelské jméno a heslo první hello.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-tooazure-container-registry"></a>Nahrát váš tooAzure kontejner Docker registru kontejneru

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-hello-docker-python-flask-application-tooazure"></a>Nasazení tooAzure aplikace Docker Python Flask hello

V tomto kroku nasadíte vaší Docker na základě kontejneru Python Flask aplikace tooAzure služby App Service.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

Vytvořte plán služby App Service s hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) příkaz. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Hello následující příklad vytvoří plán aplikační služby se systémem Linux s názvem *myAppServicePlan* pomocí hello S1 cenové úrovně:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Při vytvoření hello plán služby App Service je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:

```json 
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "linux",
  "location": "West US",
  "maximumNumberOfWorkers": 10,
  "name": "myAppServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "S",
    "locations": null,
    "name": "S1",
    "size": "S1",
    "skuCapacity": null,
    "tier": "Standard"
  },
  "status": "Ready",
  "subscription": "00000000-0000-0000-0000-000000000000",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
``` 

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

Vytvoření webové aplikace v hello *myAppServicePlan* plán služby App Service se hello [az webapp vytvořit](/cli/azure/webapp#create) příkaz. 

Hello webové aplikace poskytuje můžete hostování místo toodeploy kódu a poskytuje adresu URL pro vás tooview hello nasazené aplikace. Použití toocreate hello webové aplikace. 

Následující příkaz a nahraďte v hello hello  *\<app_name >* zástupný symbol s jedinečným názvem aplikace. Tento název je součástí hello výchozí adresa URL pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure App Service. 

```azurecli
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

### <a name="configure-hello-database-environment-variables"></a>Nakonfigurujte proměnné prostředí databáze hello

V kurzu hello jste definovali databázi PostgreSQL tooyour tooconnect proměnné prostředí.

Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí hello [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config#set) příkaz. 

Hello následující příklad určuje hello podrobnosti připojení databáze jako nastavení aplikace. Používá také hello *PORT* proměnné toomap PORT 5000 z provozu kontejner Docker tooreceive HTTP na portu 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Konfigurace nasazení kontejner Docker 

Služby App Service může automaticky stáhnout a spusťte kontejner Docker.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Vždy, když se aktualizovat kontejner Docker hello nebo změnit nastavení hello, restartujte aplikace hello. Restartování zajišťuje, že všechna nastavení použita a nejnovější kontejneru hello pocházejí z registru hello.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-toohello-azure-web-app"></a>Procházet toohello webové aplikace Azure 

Procházet toohello nasadit webovou aplikaci pomocí webového prohlížeče. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> webové aplikace Hello využívá delší tooload, protože hello kontejneru má toobe stáhnou a spustí po změně konfigurace kontejneru hello.

Zobrazí dříve zaregistrovaný hosté, uložené v předchozím kroku hello toohello Azure produkční databázi.

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Blahopřejeme!** Používáte Docker aplikace Python Flask na základě kontejneru ve službě Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Aktualizace datový model a znovu ho zaveďte

V tomto kroku přidáte hello počet registrace události tooeach účastníci aktualizací hello hosta modelu.

Podívejte se na hello *0,2 Migrace* verzi s hello následující příkaz git:

```bash
git checkout tags/0.2-migration
```

Tato verze už provedených hello tooviews potřebné změny, řadiče a modelu. Zahrnuje také migrace databáze generované prostřednictvím *destilační přístroj* (`flask db migrate`). Zobrazí všechny změny provedené pomocí následujícího příkazu git hello:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Otestujte provedené změny místně

Spusťte následující příkazy tootest hello změny místně používají server flask hello.

Mac / Linux:
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Přejděte v prohlížeči tooview hello změny toohttp://127.0.0.1:5000. Vytvořte testovací registrace.

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-tooazure"></a>Publikování změn tooAzure

Sestavení hello novou bitovou kopii docker, poslat ho toohello kontejneru registru a restartujte aplikaci hello.

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Přejděte tooyour webové aplikace Azure a znovu vyzkoušet nové funkce hello. Vytvořte další registrace události.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask aplikace v Azure App Service](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte toohello [portál Azure](https://portal.azure.com) toosee hello webovou aplikaci jste vytvořili.

V levé nabídce hello, klikněte na **App Services**, pak klikněte na název hello Azure webové aplikace.

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-docker-python-postgresql-app/app-resource.png)

Ve výchozím nastavení, hello portál zobrazuje vaší webové aplikace **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Hello karty na levé straně stránky hello hello zobrazit stránky hello jinou konfiguraci, které můžete otevřít.

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Další kroky

Posunutí toohello další kurz toolearn jak toomap vlastní DNS název tooyour webové aplikace.

> [!div class="nextstepaction"] 
> [Mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md)
