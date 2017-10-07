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
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a><span data-ttu-id="f5b17-103">Vytvoření webové aplikace Docker Python a PostgreSQL v Azure</span><span class="sxs-lookup"><span data-stu-id="f5b17-103">Build a Docker Python and PostgreSQL web app in Azure</span></span>

<span data-ttu-id="f5b17-104">Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby.</span><span class="sxs-lookup"><span data-stu-id="f5b17-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="f5b17-105">Tento kurz ukazuje, jak toocreate základní Python Docker webové aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b17-105">This tutorial shows how toocreate a basic Docker Python web app in Azure.</span></span> <span data-ttu-id="f5b17-106">Budete připojit databázi PostgreSQL tooa této aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-106">You'll connect this app tooa PostgreSQL database.</span></span> <span data-ttu-id="f5b17-107">Když jste hotovi, budete mít k aplikaci Python Flask systémem v rámci kontejner Docker [Azure App Service Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5b17-107">When you're done, you'll have a Python Flask application running within a Docker container on [Azure App Service Web Apps](app-service-web-overview.md).</span></span>

![Docker Python Flask aplikace v Azure App Service](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

<span data-ttu-id="f5b17-109">V systému macOS můžete provést následující postup hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-109">You can follow hello steps below on macOS.</span></span> <span data-ttu-id="f5b17-110">Pokyny pro Linux a Windows jsou stejné hello ve většině případů, ale hello rozdíly nejsou popsané v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f5b17-110">Linux and Windows instructions are hello same in most cases, but hello differences are not detailed in this tutorial.</span></span>
 
## <a name="prerequisites"></a><span data-ttu-id="f5b17-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f5b17-111">Prerequisites</span></span>

<span data-ttu-id="f5b17-112">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="f5b17-112">toocomplete this tutorial:</span></span>

1. <span data-ttu-id="f5b17-113">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="f5b17-113">[Install Git](https://git-scm.com/)</span></span>
1. <span data-ttu-id="f5b17-114">[Nainstalovat Python](https://www.python.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f5b17-114">[Install Python](https://www.python.org/downloads/)</span></span>
1. [<span data-ttu-id="f5b17-115">Nainstalujte a spusťte PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="f5b17-115">Install and run PostgreSQL</span></span>](https://www.postgresql.org/download/)
1. [<span data-ttu-id="f5b17-116">Nainstalujte Docker Community Edition</span><span class="sxs-lookup"><span data-stu-id="f5b17-116">Install Docker Community Edition</span></span>](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f5b17-117">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f5b17-117">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f5b17-118">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="f5b17-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="f5b17-119">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f5b17-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-postgresql-installation-and-create-a-database"></a><span data-ttu-id="f5b17-120">Testování místní instalaci PostgreSQL a vytvořit databázi</span><span class="sxs-lookup"><span data-stu-id="f5b17-120">Test local PostgreSQL installation and create a database</span></span>

<span data-ttu-id="f5b17-121">Otevřete okno terminálu hello a spusťte `psql postgres` tooconnect tooyour místní PostgreSQL server.</span><span class="sxs-lookup"><span data-stu-id="f5b17-121">Open hello terminal window and run `psql postgres` tooconnect tooyour local PostgreSQL server.</span></span>

```bash
psql postgres
```

<span data-ttu-id="f5b17-122">Pokud připojení úspěšné, je databáze PostgreSQL spuštěna.</span><span class="sxs-lookup"><span data-stu-id="f5b17-122">If your connection is successful, your PostgreSQL database is running.</span></span> <span data-ttu-id="f5b17-123">Pokud ne, ujistěte se, zda je spuštěná místní databázi PostgresQL podle následujících kroků hello v [stáhne - PostgreSQL základní distribuční](https://www.postgresql.org/download/).</span><span class="sxs-lookup"><span data-stu-id="f5b17-123">If not, make sure that your local PostgresQL database is started by following hello steps at [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).</span></span>

<span data-ttu-id="f5b17-124">Vytvoření databáze názvem *eventregistration* a nastavení uživatele samostatné databáze s názvem *manager* heslem *supersecretpass*.</span><span class="sxs-lookup"><span data-stu-id="f5b17-124">Create a database called *eventregistration* and set up a separate database user named *manager* with password *supersecretpass*.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```
<span data-ttu-id="f5b17-125">Typ *\q* tooexit hello PostgreSQL klienta.</span><span class="sxs-lookup"><span data-stu-id="f5b17-125">Type *\q* tooexit hello PostgreSQL client.</span></span> 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a><span data-ttu-id="f5b17-126">Vytvořit místní aplikace Python Flask</span><span class="sxs-lookup"><span data-stu-id="f5b17-126">Create local Python Flask application</span></span>

<span data-ttu-id="f5b17-127">V tomto kroku nastavíte místní projekt Python Flask hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-127">In this step, you set up hello local Python Flask project.</span></span>

### <a name="clone-hello-sample-application"></a><span data-ttu-id="f5b17-128">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="f5b17-128">Clone hello sample application</span></span>

<span data-ttu-id="f5b17-129">Hello otevřete okno terminálu, a `CD` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="f5b17-129">Open hello terminal window, and `CD` tooa working directory.</span></span>  

<span data-ttu-id="f5b17-130">Hello spusťte následující příkazy tooclone hello Ukázka úložiště a přejděte toohello *0,1 initialapp* verzi.</span><span class="sxs-lookup"><span data-stu-id="f5b17-130">Run hello following commands tooclone hello sample repository and go toohello *0.1-initialapp* release.</span></span>

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

<span data-ttu-id="f5b17-131">Tato ukázka úložiště obsahuje [Flask](http://flask.pocoo.org/) aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-131">This sample repository contains a [Flask](http://flask.pocoo.org/) application.</span></span> 

### <a name="run-hello-application"></a><span data-ttu-id="f5b17-132">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="f5b17-132">Run hello application</span></span>

> [!NOTE] 
> <span data-ttu-id="f5b17-133">Později tento proces zjednodušit podle budovy toouse kontejner Docker s hello produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="f5b17-133">In a later step you simplify this process by building a Docker container toouse with hello production database.</span></span>

<span data-ttu-id="f5b17-134">Instalace hello požadované balíčky a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-134">Install hello required packages and start hello application.</span></span>

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="f5b17-135">Když je aplikace hello úplným načtením, uvidíte něco podobné toohello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="f5b17-135">When hello app is fully loaded, you see something similar toohello following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="f5b17-136">Přejděte toohttp://127.0.0.1:5000 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f5b17-136">Navigate toohttp://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="f5b17-137">Klikněte na tlačítko **zaregistrovat!**</span><span class="sxs-lookup"><span data-stu-id="f5b17-137">Click **Register!**</span></span> <span data-ttu-id="f5b17-138">a vytvoření zkušebního uživatele.</span><span class="sxs-lookup"><span data-stu-id="f5b17-138">and create a test user.</span></span>

![Aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

<span data-ttu-id="f5b17-140">Hello Flask ukázkové aplikace ukládá data uživatele v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-140">hello Flask sample application stores user data in hello database.</span></span> <span data-ttu-id="f5b17-141">Pokud jste úspěšné při registraci uživatele, aplikace je zápis dat toohello místní databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f5b17-141">If you are successful at registering a user, your app is writing data toohello local PostgreSQL database.</span></span>

<span data-ttu-id="f5b17-142">server Flask hello toostop v kdykoli, zadejte Ctrl + C hello terminálu.</span><span class="sxs-lookup"><span data-stu-id="f5b17-142">toostop hello Flask server at anytime, type Ctrl+C in hello terminal.</span></span> 

## <a name="create-a-production-postgresql-database"></a><span data-ttu-id="f5b17-143">Vytvořit databázi PostgreSQL výroby</span><span class="sxs-lookup"><span data-stu-id="f5b17-143">Create a production PostgreSQL database</span></span>

<span data-ttu-id="f5b17-144">V tomto kroku vytvoříte databázi PostgreSQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b17-144">In this step, you create a PostgreSQL database in Azure.</span></span> <span data-ttu-id="f5b17-145">Pokud je vaše aplikace nasazené tooAzure, použije tuto databázi cloudu.</span><span class="sxs-lookup"><span data-stu-id="f5b17-145">When your app is deployed tooAzure, it will use this cloud database.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="f5b17-146">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="f5b17-146">Log in tooAzure</span></span>

<span data-ttu-id="f5b17-147">Jste nyní probíhající toouse hello Azure CLI 2.0 toocreate hello prostředky potřebné toohost aplikace Python ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f5b17-147">You are now going toouse hello Azure CLI 2.0 toocreate hello resources needed toohost your Python application in Azure App Service.</span></span>  <span data-ttu-id="f5b17-148">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="f5b17-148">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="f5b17-149">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f5b17-149">Create a resource group</span></span>

<span data-ttu-id="f5b17-150">Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s hello [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="f5b17-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create).</span></span> 

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="f5b17-151">Hello následující příklad vytvoří skupinu prostředků v oblasti západní USA hello:</span><span class="sxs-lookup"><span data-stu-id="f5b17-151">hello following example creates a resource group in hello West US region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

<span data-ttu-id="f5b17-152">Použití hello [míst seznamu služby App Service az](/cli/azure/appservice#list-locations) rozhraní příkazového řádku Azure příkaz toolist dostupná umístění.</span><span class="sxs-lookup"><span data-stu-id="f5b17-152">Use hello [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command toolist available locations.</span></span>

### <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="f5b17-153">Vytvoření serveru Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="f5b17-153">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="f5b17-154">Vytvoření serveru PostgreSQL s hello [az postgres server vytvořit](/cli/azure/documentdb#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5b17-154">Create a PostgreSQL server with hello [az postgres server create](/cli/azure/documentdb#create) command.</span></span>

<span data-ttu-id="f5b17-155">V hello následující příkaz, nahraďte název jedinečný server hello  *\<postgresql_name >* zástupný symbol a uživatelské jméno pro hello  *\<admin_username >* zástupný symbol .</span><span class="sxs-lookup"><span data-stu-id="f5b17-155">In hello following command, substitute a unique server name for hello *\<postgresql_name>* placeholder and a user name for hello *\<admin_username>* placeholder.</span></span> <span data-ttu-id="f5b17-156">název serveru Hello se používá jako součást váš koncový bod PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), takže název hello musí toobe jedinečné ve všech serverech v Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b17-156">hello server name is used as part of your PostgreSQL endpoint (`https://<postgresql_name>.postgres.database.azure.com`), so hello name needs toobe unique across all servers in Azure.</span></span> <span data-ttu-id="f5b17-157">Hello uživatelské jméno je hello počáteční databáze správce uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="f5b17-157">hello user name is for hello initial database admin user account.</span></span> <span data-ttu-id="f5b17-158">Jste výzvami toopick heslo pro tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="f5b17-158">You are prompted toopick a password for this user.</span></span>

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

<span data-ttu-id="f5b17-159">Při vytvoření hello Azure databáze pro PostgreSQL server je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f5b17-159">When hello Azure Database for PostgreSQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="create-a-firewall-rule-for-hello-azure-database-for-postgresql-server"></a><span data-ttu-id="f5b17-160">Vytvořte pravidlo brány firewall pro hello Azure databáze PostgreSQL serveru</span><span class="sxs-lookup"><span data-stu-id="f5b17-160">Create a firewall rule for hello Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="f5b17-161">Spusťte následující příkaz příkazového řádku Azure CLI, tooallow databázi toohello access ze všech IP adres hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-161">Run hello following Azure CLI command tooallow access toohello database from all IP addresses.</span></span>

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

<span data-ttu-id="f5b17-162">Hello rozhraní příkazového řádku Azure potvrdí hello vytvoření pravidla brány firewall se výstup podobný toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f5b17-162">hello Azure CLI confirms hello firewall rule creation with output similar toohello following example:</span></span>

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

## <a name="connect-your-python-flask-application-toohello-database"></a><span data-ttu-id="f5b17-163">Připojit databáze toohello aplikace Python Flask</span><span class="sxs-lookup"><span data-stu-id="f5b17-163">Connect your Python Flask application toohello database</span></span>

<span data-ttu-id="f5b17-164">V tomto kroku připojíte vaší Python Flask ukázkové aplikace toohello Azure databáze PostgreSQL serveru, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f5b17-164">In this step, you connect your Python Flask sample application toohello Azure Database for PostgreSQL server you created.</span></span>

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a><span data-ttu-id="f5b17-165">Vytvořit prázdnou databázi a nastavení nového uživatele databáze aplikace</span><span class="sxs-lookup"><span data-stu-id="f5b17-165">Create an empty database and set up a new database application user</span></span>

<span data-ttu-id="f5b17-166">Vytvořte uživatele databáze s tooa jedné databáze access jenom.</span><span class="sxs-lookup"><span data-stu-id="f5b17-166">Create a database user with access tooa single database only.</span></span> <span data-ttu-id="f5b17-167">Tyto přihlašovací údaje tooavoid poskytnutí hello aplikační úplný přístup toohello server budete používat.</span><span class="sxs-lookup"><span data-stu-id="f5b17-167">You'll use these credentials tooavoid giving hello application full access toohello server.</span></span>

<span data-ttu-id="f5b17-168">Připojte databáze toohello (se zobrazí výzva k zadání hesla správce).</span><span class="sxs-lookup"><span data-stu-id="f5b17-168">Connect toohello database (you're prompted for your admin password).</span></span>

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

<span data-ttu-id="f5b17-169">Vytvoření databáze hello a uživatele z hello PostgreSQL rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f5b17-169">Create hello database and user from hello PostgreSQL CLI.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```

<span data-ttu-id="f5b17-170">Typ *\q* tooexit hello PostgreSQL klienta.</span><span class="sxs-lookup"><span data-stu-id="f5b17-170">Type *\q* tooexit hello PostgreSQL client.</span></span>

### <a name="test-hello-application-locally-against-hello-azure-postgresql-database"></a><span data-ttu-id="f5b17-171">Testování aplikace hello místně na databázi Azure PostgreSQL hello</span><span class="sxs-lookup"><span data-stu-id="f5b17-171">Test hello application locally against hello Azure PostgreSQL database</span></span> 

<span data-ttu-id="f5b17-172">Návratem teď toohello *aplikace* složky hello klonovat úložiště Github, můžete spustit aplikace Python Flask hello aktualizací proměnné prostředí hello databáze.</span><span class="sxs-lookup"><span data-stu-id="f5b17-172">Going back now toohello *app* folder of hello cloned Github repository, you can run hello Python Flask application by updating hello database environment variables.</span></span>

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="f5b17-173">Když je aplikace hello úplným načtením, uvidíte něco podobné toohello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="f5b17-173">When hello app is fully loaded, you see something similar toohello following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="f5b17-174">Přejděte toohttp://127.0.0.1:5000 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f5b17-174">Navigate toohttp://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="f5b17-175">Klikněte na tlačítko **zaregistrovat!**</span><span class="sxs-lookup"><span data-stu-id="f5b17-175">Click **Register!**</span></span> <span data-ttu-id="f5b17-176">a vytvořit testovací registrace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-176">and create a test registration.</span></span> <span data-ttu-id="f5b17-177">Databáze toohello data jsou nyní zápisu v Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b17-177">You are now writing data toohello database in Azure.</span></span>

![Aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-hello-application-from-a-docker-container"></a><span data-ttu-id="f5b17-179">Spuštění aplikace hello z kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="f5b17-179">Running hello application from a Docker Container</span></span>

<span data-ttu-id="f5b17-180">Sestavení bitové kopie kontejner Docker hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-180">Build hello Docker container image.</span></span>

```bash
cd ..
docker build -t flask-postgresql-sample .
```

<span data-ttu-id="f5b17-181">Docker zobrazí potvrzení tohoto kontejneru hello it byl úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="f5b17-181">Docker displays a confirmation that it successfully created hello container.</span></span>

```bash
Successfully built 7548f983a36b
```

<span data-ttu-id="f5b17-182">Přidání databáze prostředí proměnné tooan prostředí proměnné souboru *db.env*.</span><span class="sxs-lookup"><span data-stu-id="f5b17-182">Add database environment variables tooan environment variable file *db.env*.</span></span> <span data-ttu-id="f5b17-183">aplikace Hello připojí produkční databázi PostgreSQL toohello v Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b17-183">hello app will connect toohello PostgreSQL production database in Azure.</span></span>

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

<span data-ttu-id="f5b17-184">Spuštění aplikace hello z v rámci kontejner Docker hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-184">Run hello app from within hello Docker container.</span></span> <span data-ttu-id="f5b17-185">Hello následující příkaz určuje hello souboru proměnných prostředí a mapuje hello výchozí Flask port 5000 toolocal port 5000.</span><span class="sxs-lookup"><span data-stu-id="f5b17-185">hello following command specifies hello environment variable file and maps hello default Flask port 5000 toolocal port 5000.</span></span>

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

<span data-ttu-id="f5b17-186">výstup Hello je podobné toowhat, které jste viděli dříve.</span><span class="sxs-lookup"><span data-stu-id="f5b17-186">hello output is similar toowhat you saw earlier.</span></span> <span data-ttu-id="f5b17-187">Migrace počáteční databáze hello však již nepotřebuje toobe provést a proto se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="f5b17-187">However, hello initial database migration no longer needs toobe performed and therefore is skipped.</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="f5b17-188">Hello databáze již obsahuje hello registrace, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="f5b17-188">hello database already contains hello registration you created previously.</span></span>

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-hello-docker-container-tooa-container-registry"></a><span data-ttu-id="f5b17-190">Nahrát hello Docker kontejneru tooa kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="f5b17-190">Upload hello Docker container tooa container registry</span></span>

<span data-ttu-id="f5b17-191">V tomto kroku nahrát hello Docker kontejneru tooa kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="f5b17-191">In this step, you upload hello Docker container tooa container registry.</span></span> <span data-ttu-id="f5b17-192">Budete používat Azure kontejneru registru, ale můžete také použít další oblíbených těch, jako je například Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="f5b17-192">You'll use Azure Container Registry, but you could also use other popular ones such as Docker Hub.</span></span>

### <a name="create-an-azure-container-registry"></a><span data-ttu-id="f5b17-193">Vytvoření služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="f5b17-193">Create an Azure Container Registry</span></span>

<span data-ttu-id="f5b17-194">V následující příkaz toocreate registru kontejneru hello nahraďte  *\<registry_name >* s názvem registru jedinečný kontejner Azure podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="f5b17-194">In hello following command toocreate a container registry replace *\<registry_name>* with a unique Azure container registry name of your choice.</span></span>

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

<span data-ttu-id="f5b17-195">Výstup</span><span class="sxs-lookup"><span data-stu-id="f5b17-195">Output</span></span>
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

### <a name="retrieve-hello-registry-credentials-for-pushing-and-pulling-docker-images"></a><span data-ttu-id="f5b17-196">Načíst hello registru pověření pro vkládání a stahování imagí Dockeru</span><span class="sxs-lookup"><span data-stu-id="f5b17-196">Retrieve hello registry credentials for pushing and pulling Docker images</span></span>

<span data-ttu-id="f5b17-197">přihlašovací údaje tooshow registru, nejprve povolte režim správce.</span><span class="sxs-lookup"><span data-stu-id="f5b17-197">tooshow registry credentials, enable admin mode first.</span></span>

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

<span data-ttu-id="f5b17-198">Zobrazí dvě hesla.</span><span class="sxs-lookup"><span data-stu-id="f5b17-198">You see two passwords.</span></span> <span data-ttu-id="f5b17-199">Poznamenejte si hello uživatelské jméno a heslo první hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-199">Make note of hello user name and hello first password.</span></span>

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

### <a name="upload-your-docker-container-tooazure-container-registry"></a><span data-ttu-id="f5b17-200">Nahrát váš tooAzure kontejner Docker registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="f5b17-200">Upload your Docker container tooAzure Container Registry</span></span>

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-hello-docker-python-flask-application-tooazure"></a><span data-ttu-id="f5b17-201">Nasazení tooAzure aplikace Docker Python Flask hello</span><span class="sxs-lookup"><span data-stu-id="f5b17-201">Deploy hello Docker Python Flask application tooAzure</span></span>

<span data-ttu-id="f5b17-202">V tomto kroku nasadíte vaší Docker na základě kontejneru Python Flask aplikace tooAzure služby App Service.</span><span class="sxs-lookup"><span data-stu-id="f5b17-202">In this step, you deploy your Docker container-based Python Flask application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="f5b17-203">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="f5b17-203">Create an App Service plan</span></span>

<span data-ttu-id="f5b17-204">Vytvořte plán služby App Service s hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5b17-204">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="f5b17-205">Hello následující příklad vytvoří plán aplikační služby se systémem Linux s názvem *myAppServicePlan* pomocí hello S1 cenové úrovně:</span><span class="sxs-lookup"><span data-stu-id="f5b17-205">hello following example creates a Linux-based App Service plan named *myAppServicePlan* using hello S1 pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

<span data-ttu-id="f5b17-206">Při vytvoření hello plán služby App Service je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f5b17-206">When hello App Service plan is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="create-a-web-app"></a><span data-ttu-id="f5b17-207">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f5b17-207">Create a web app</span></span>

<span data-ttu-id="f5b17-208">Vytvoření webové aplikace v hello *myAppServicePlan* plán služby App Service se hello [az webapp vytvořit](/cli/azure/webapp#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5b17-208">Create a web app in hello *myAppServicePlan* App Service plan with hello [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="f5b17-209">Hello webové aplikace poskytuje můžete hostování místo toodeploy kódu a poskytuje adresu URL pro vás tooview hello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-209">hello web app gives you a hosting space toodeploy your code and provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="f5b17-210">Použití toocreate hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-210">Use  toocreate hello web app.</span></span> 

<span data-ttu-id="f5b17-211">Následující příkaz a nahraďte v hello hello  *\<app_name >* zástupný symbol s jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-211">In hello following command, replace hello *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="f5b17-212">Tento název je součástí hello výchozí adresa URL pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f5b17-212">This name is part of hello default URL for hello web app, so hello name needs toobe unique across all apps in Azure App Service.</span></span> 

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="f5b17-213">Po vytvoření webové aplikace hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f5b17-213">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-hello-database-environment-variables"></a><span data-ttu-id="f5b17-214">Nakonfigurujte proměnné prostředí databáze hello</span><span class="sxs-lookup"><span data-stu-id="f5b17-214">Configure hello database environment variables</span></span>

<span data-ttu-id="f5b17-215">V kurzu hello jste definovali databázi PostgreSQL tooyour tooconnect proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f5b17-215">Earlier in hello tutorial, you defined environment variables tooconnect tooyour PostgreSQL database.</span></span>

<span data-ttu-id="f5b17-216">Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí hello [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5b17-216">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings set](/cli/azure/webapp/config#set) command.</span></span> 

<span data-ttu-id="f5b17-217">Hello následující příklad určuje hello podrobnosti připojení databáze jako nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-217">hello following example specifies hello database connection details as app settings.</span></span> <span data-ttu-id="f5b17-218">Používá také hello *PORT* proměnné toomap PORT 5000 z provozu kontejner Docker tooreceive HTTP na portu 80.</span><span class="sxs-lookup"><span data-stu-id="f5b17-218">It also uses hello *PORT* variable toomap PORT 5000 from your Docker Container tooreceive HTTP traffic on PORT 80.</span></span>

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a><span data-ttu-id="f5b17-219">Konfigurace nasazení kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="f5b17-219">Configure Docker container deployment</span></span> 

<span data-ttu-id="f5b17-220">Služby App Service může automaticky stáhnout a spusťte kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="f5b17-220">AppService can automatically download and run a Docker container.</span></span>

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

<span data-ttu-id="f5b17-221">Vždy, když se aktualizovat kontejner Docker hello nebo změnit nastavení hello, restartujte aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-221">Whenever you update hello Docker container or change hello settings, restart hello app.</span></span> <span data-ttu-id="f5b17-222">Restartování zajišťuje, že všechna nastavení použita a nejnovější kontejneru hello pocházejí z registru hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-222">Restarting ensures that all settings are applied and hello latest container is pulled from hello registry.</span></span>

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="f5b17-223">Procházet toohello webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="f5b17-223">Browse toohello Azure web app</span></span> 

<span data-ttu-id="f5b17-224">Procházet toohello nasadit webovou aplikaci pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f5b17-224">Browse toohello deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> <span data-ttu-id="f5b17-225">webové aplikace Hello využívá delší tooload, protože hello kontejneru má toobe stáhnou a spustí po změně konfigurace kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-225">hello web app takes longer tooload because hello container has toobe downloaded and started after hello container configuration is changed.</span></span>

<span data-ttu-id="f5b17-226">Zobrazí dříve zaregistrovaný hosté, uložené v předchozím kroku hello toohello Azure produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="f5b17-226">You see previously registered guests that were saved toohello Azure production database in hello previous step.</span></span>

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-app-deployed.png)

<span data-ttu-id="f5b17-228">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="f5b17-228">**Congratulations!**</span></span> <span data-ttu-id="f5b17-229">Používáte Docker aplikace Python Flask na základě kontejneru ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f5b17-229">You're running a Docker container-based Python Flask app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="f5b17-230">Aktualizace datový model a znovu ho zaveďte</span><span class="sxs-lookup"><span data-stu-id="f5b17-230">Update data model and redeploy</span></span>

<span data-ttu-id="f5b17-231">V tomto kroku přidáte hello počet registrace události tooeach účastníci aktualizací hello hosta modelu.</span><span class="sxs-lookup"><span data-stu-id="f5b17-231">In this step, you add hello number of attendees tooeach event registration by updating hello Guest model.</span></span>

<span data-ttu-id="f5b17-232">Podívejte se na hello *0,2 Migrace* verzi s hello následující příkaz git:</span><span class="sxs-lookup"><span data-stu-id="f5b17-232">Check out hello *0.2-migration* release with hello following git command:</span></span>

```bash
git checkout tags/0.2-migration
```

<span data-ttu-id="f5b17-233">Tato verze už provedených hello tooviews potřebné změny, řadiče a modelu.</span><span class="sxs-lookup"><span data-stu-id="f5b17-233">This release already made hello necessary changes tooviews, controllers, and model.</span></span> <span data-ttu-id="f5b17-234">Zahrnuje také migrace databáze generované prostřednictvím *destilační přístroj* (`flask db migrate`).</span><span class="sxs-lookup"><span data-stu-id="f5b17-234">It also includes a database migration generated via *alembic* (`flask db migrate`).</span></span> <span data-ttu-id="f5b17-235">Zobrazí všechny změny provedené pomocí následujícího příkazu git hello:</span><span class="sxs-lookup"><span data-stu-id="f5b17-235">You can see all changes made via hello following git command:</span></span>

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="f5b17-236">Otestujte provedené změny místně</span><span class="sxs-lookup"><span data-stu-id="f5b17-236">Test your changes locally</span></span>

<span data-ttu-id="f5b17-237">Spusťte následující příkazy tootest hello změny místně používají server flask hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-237">Run hello following commands tootest your changes locally by running hello flask server.</span></span>

<span data-ttu-id="f5b17-238">Mac / Linux:</span><span class="sxs-lookup"><span data-stu-id="f5b17-238">Mac / Linux:</span></span>
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="f5b17-239">Přejděte v prohlížeči tooview hello změny toohttp://127.0.0.1:5000.</span><span class="sxs-lookup"><span data-stu-id="f5b17-239">Navigate toohttp://127.0.0.1:5000 in your browser tooview hello changes.</span></span> <span data-ttu-id="f5b17-240">Vytvořte testovací registrace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-240">Create a test registration.</span></span>

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-tooazure"></a><span data-ttu-id="f5b17-242">Publikování změn tooAzure</span><span class="sxs-lookup"><span data-stu-id="f5b17-242">Publish changes tooAzure</span></span>

<span data-ttu-id="f5b17-243">Sestavení hello novou bitovou kopii docker, poslat ho toohello kontejneru registru a restartujte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-243">Build hello new docker image, push it toohello container registry, and restart hello app.</span></span>

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

<span data-ttu-id="f5b17-244">Přejděte tooyour webové aplikace Azure a znovu vyzkoušet nové funkce hello.</span><span class="sxs-lookup"><span data-stu-id="f5b17-244">Navigate tooyour Azure web app and try out hello new functionality again.</span></span> <span data-ttu-id="f5b17-245">Vytvořte další registrace události.</span><span class="sxs-lookup"><span data-stu-id="f5b17-245">Create another event registration.</span></span>

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask aplikace v Azure App Service](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="f5b17-247">Správa Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f5b17-247">Manage your Azure web app</span></span>

<span data-ttu-id="f5b17-248">Přejděte toohello [portál Azure](https://portal.azure.com) toosee hello webovou aplikaci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f5b17-248">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span>

<span data-ttu-id="f5b17-249">V levé nabídce hello, klikněte na **App Services**, pak klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-249">From hello left menu, click **App Services**, then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-docker-python-postgresql-app/app-resource.png)

<span data-ttu-id="f5b17-251">Ve výchozím nastavení, hello portál zobrazuje vaší webové aplikace **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="f5b17-251">By default, hello portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="f5b17-252">Tato stránka poskytuje přehled, jak si vaše aplikace stojí.</span><span class="sxs-lookup"><span data-stu-id="f5b17-252">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="f5b17-253">Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="f5b17-253">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="f5b17-254">Hello karty na levé straně stránky hello hello zobrazit stránky hello jinou konfiguraci, které můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="f5b17-254">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span>

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a><span data-ttu-id="f5b17-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5b17-256">Next steps</span></span>

<span data-ttu-id="f5b17-257">Posunutí toohello další kurz toolearn jak toomap vlastní DNS název tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b17-257">Advance toohello next tutorial toolearn how toomap a custom DNS name tooyour web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="f5b17-258">Mapovat existující vlastní DNS název tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="f5b17-258">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
