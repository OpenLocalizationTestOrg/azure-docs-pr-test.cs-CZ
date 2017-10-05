---
title: "Vytvoření webové aplikace Docker Python a PostgreSQL v Azure | Microsoft Docs"
description: "Další informace o získání a Docker Python aplikaci v Azure, funguje s připojením k databázi PostgreSQL."
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
ms.openlocfilehash: e70f85a1eb4a6e1a81e0ca4fae228ca97deca6fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a><span data-ttu-id="669f3-103">Vytvoření webové aplikace Docker Python a PostgreSQL v Azure</span><span class="sxs-lookup"><span data-stu-id="669f3-103">Build a Docker Python and PostgreSQL web app in Azure</span></span>

<span data-ttu-id="669f3-104">Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby.</span><span class="sxs-lookup"><span data-stu-id="669f3-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="669f3-105">Tento kurz ukazuje, jak vytvořit základní webovou aplikaci Docker Python v Azure.</span><span class="sxs-lookup"><span data-stu-id="669f3-105">This tutorial shows how to create a basic Docker Python web app in Azure.</span></span> <span data-ttu-id="669f3-106">Tuto aplikaci budete připojit k databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="669f3-106">You'll connect this app to a PostgreSQL database.</span></span> <span data-ttu-id="669f3-107">Když jste hotovi, budete mít k aplikaci Python Flask systémem v rámci kontejner Docker [Azure App Service Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="669f3-107">When you're done, you'll have a Python Flask application running within a Docker container on [Azure App Service Web Apps](app-service-web-overview.md).</span></span>

![Docker Python Flask aplikace v Azure App Service](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

<span data-ttu-id="669f3-109">Postupujte podle těchto kroků v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="669f3-109">You can follow the steps below on macOS.</span></span> <span data-ttu-id="669f3-110">Pokyny pro Linux a Windows jsou stejné ve většině případů, ale rozdíly nejsou popsané v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="669f3-110">Linux and Windows instructions are the same in most cases, but the differences are not detailed in this tutorial.</span></span>
 
## <a name="prerequisites"></a><span data-ttu-id="669f3-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="669f3-111">Prerequisites</span></span>

<span data-ttu-id="669f3-112">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="669f3-112">To complete this tutorial:</span></span>

1. <span data-ttu-id="669f3-113">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="669f3-113">[Install Git](https://git-scm.com/)</span></span>
1. <span data-ttu-id="669f3-114">[Nainstalovat Python](https://www.python.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="669f3-114">[Install Python](https://www.python.org/downloads/)</span></span>
1. [<span data-ttu-id="669f3-115">Nainstalujte a spusťte PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="669f3-115">Install and run PostgreSQL</span></span>](https://www.postgresql.org/download/)
1. [<span data-ttu-id="669f3-116">Nainstalujte Docker Community Edition</span><span class="sxs-lookup"><span data-stu-id="669f3-116">Install Docker Community Edition</span></span>](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="669f3-117">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="669f3-117">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="669f3-118">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="669f3-118">Run `az --version` to find the version.</span></span> <span data-ttu-id="669f3-119">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="669f3-119">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-postgresql-installation-and-create-a-database"></a><span data-ttu-id="669f3-120">Testování místní instalaci PostgreSQL a vytvořit databázi</span><span class="sxs-lookup"><span data-stu-id="669f3-120">Test local PostgreSQL installation and create a database</span></span>

<span data-ttu-id="669f3-121">Otevřete okno terminálu a spusťte `psql postgres` pro připojení k místní server PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="669f3-121">Open the terminal window and run `psql postgres` to connect to your local PostgreSQL server.</span></span>

```bash
psql postgres
```

<span data-ttu-id="669f3-122">Pokud připojení úspěšné, je databáze PostgreSQL spuštěna.</span><span class="sxs-lookup"><span data-stu-id="669f3-122">If your connection is successful, your PostgreSQL database is running.</span></span> <span data-ttu-id="669f3-123">Pokud ne, ujistěte se, zda je spuštěná místní databázi PostgresQL podle kroků v [stáhne - PostgreSQL základní distribuční](https://www.postgresql.org/download/).</span><span class="sxs-lookup"><span data-stu-id="669f3-123">If not, make sure that your local PostgresQL database is started by following the steps at [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).</span></span>

<span data-ttu-id="669f3-124">Vytvoření databáze názvem *eventregistration* a nastavení uživatele samostatné databáze s názvem *manager* heslem *supersecretpass*.</span><span class="sxs-lookup"><span data-stu-id="669f3-124">Create a database called *eventregistration* and set up a separate database user named *manager* with password *supersecretpass*.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
<span data-ttu-id="669f3-125">Typ *\q* ukončíte klienta PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="669f3-125">Type *\q* to exit the PostgreSQL client.</span></span> 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a><span data-ttu-id="669f3-126">Vytvořit místní aplikace Python Flask</span><span class="sxs-lookup"><span data-stu-id="669f3-126">Create local Python Flask application</span></span>

<span data-ttu-id="669f3-127">V tomto kroku nastavíte místní projekt Python Flask.</span><span class="sxs-lookup"><span data-stu-id="669f3-127">In this step, you set up the local Python Flask project.</span></span>

### <a name="clone-the-sample-application"></a><span data-ttu-id="669f3-128">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="669f3-128">Clone the sample application</span></span>

<span data-ttu-id="669f3-129">Otevřete okno terminálu a `CD` do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="669f3-129">Open the terminal window, and `CD` to a working directory.</span></span>  

<span data-ttu-id="669f3-130">Spusťte následující příkazy a klonovat úložiště v ukázkové přejít na *0,1 initialapp* verzi.</span><span class="sxs-lookup"><span data-stu-id="669f3-130">Run the following commands to clone the sample repository and go to the *0.1-initialapp* release.</span></span>

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

<span data-ttu-id="669f3-131">Tato ukázka úložiště obsahuje [Flask](http://flask.pocoo.org/) aplikace.</span><span class="sxs-lookup"><span data-stu-id="669f3-131">This sample repository contains a [Flask](http://flask.pocoo.org/) application.</span></span> 

### <a name="run-the-application"></a><span data-ttu-id="669f3-132">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="669f3-132">Run the application</span></span>

> [!NOTE] 
> <span data-ttu-id="669f3-133">Později tento proces zjednodušit podle budovy kontejner Docker pro použití s produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="669f3-133">In a later step you simplify this process by building a Docker container to use with the production database.</span></span>

<span data-ttu-id="669f3-134">Nainstalujte požadované balíčky a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="669f3-134">Install the required packages and start the application.</span></span>

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="669f3-135">Po úplným načtením aplikace se zobrazí podobná následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="669f3-135">When the app is fully loaded, you see something similar to the following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

<span data-ttu-id="669f3-136">Přejděte do http://127.0.0.1:5000 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="669f3-136">Navigate to http://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="669f3-137">Klikněte na tlačítko **zaregistrovat!**</span><span class="sxs-lookup"><span data-stu-id="669f3-137">Click **Register!**</span></span> <span data-ttu-id="669f3-138">a vytvoření zkušebního uživatele.</span><span class="sxs-lookup"><span data-stu-id="669f3-138">and create a test user.</span></span>

![Aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

<span data-ttu-id="669f3-140">Ukázkovou aplikaci Flask ukládá data uživatele v databázi.</span><span class="sxs-lookup"><span data-stu-id="669f3-140">The Flask sample application stores user data in the database.</span></span> <span data-ttu-id="669f3-141">Pokud jste úspěšné při registraci uživatele, je vaše aplikace zápisu dat do místní databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="669f3-141">If you are successful at registering a user, your app is writing data to the local PostgreSQL database.</span></span>

<span data-ttu-id="669f3-142">Kdykoli zastavit Flask server kdykoli, zadejte Ctrl + C v terminálu.</span><span class="sxs-lookup"><span data-stu-id="669f3-142">To stop the Flask server at anytime, type Ctrl+C in the terminal.</span></span> 

## <a name="create-a-production-postgresql-database"></a><span data-ttu-id="669f3-143">Vytvořit databázi PostgreSQL výroby</span><span class="sxs-lookup"><span data-stu-id="669f3-143">Create a production PostgreSQL database</span></span>

<span data-ttu-id="669f3-144">V tomto kroku vytvoříte databázi PostgreSQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="669f3-144">In this step, you create a PostgreSQL database in Azure.</span></span> <span data-ttu-id="669f3-145">Při nasazení aplikace do Azure, použije tuto databázi cloudu.</span><span class="sxs-lookup"><span data-stu-id="669f3-145">When your app is deployed to Azure, it will use this cloud database.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="669f3-146">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="669f3-146">Log in to Azure</span></span>

<span data-ttu-id="669f3-147">Nyní se chystáte použít 2.0 rozhraní příkazového řádku Azure k vytvoření prostředky potřebné k hostování vaší aplikace Python ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="669f3-147">You are now going to use the Azure CLI 2.0 to create the resources needed to host your Python application in Azure App Service.</span></span>  <span data-ttu-id="669f3-148">Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="669f3-148">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> 

```azurecli
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="669f3-149">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="669f3-149">Create a resource group</span></span>

<span data-ttu-id="669f3-150">Vytvořte pomocí příkazu [az group create](/cli/azure/group#create) [skupinu prostředků](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="669f3-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with the [az group create](/cli/azure/group#create).</span></span> 

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="669f3-151">Následující příklad vytvoří skupinu prostředků v oblasti západní USA:</span><span class="sxs-lookup"><span data-stu-id="669f3-151">The following example creates a resource group in the West US region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

<span data-ttu-id="669f3-152">Použití [az služby App Service seznamu umístění](/cli/azure/appservice#list-locations) příkaz rozhraní příkazového řádku Azure k seznamu dostupných umístění.</span><span class="sxs-lookup"><span data-stu-id="669f3-152">Use the [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command to list available locations.</span></span>

### <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="669f3-153">Vytvoření serveru Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="669f3-153">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="669f3-154">Vytvoření serveru PostgreSQL s [az postgres server vytvořit](/cli/azure/documentdb#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="669f3-154">Create a PostgreSQL server with the [az postgres server create](/cli/azure/documentdb#create) command.</span></span>

<span data-ttu-id="669f3-155">V následujícím příkazu nahraďte název jedinečný serveru  *\<postgresql_name >* zástupný symbol a uživatel název  *\<admin_username >* zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="669f3-155">In the following command, substitute a unique server name for the *\<postgresql_name>* placeholder and a user name for the *\<admin_username>* placeholder.</span></span> <span data-ttu-id="669f3-156">Název serveru slouží jako součást váš koncový bod PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), takže název musí být jedinečný v rámci všech serverech v Azure.</span><span class="sxs-lookup"><span data-stu-id="669f3-156">The server name is used as part of your PostgreSQL endpoint (`https://<postgresql_name>.postgres.database.azure.com`), so the name needs to be unique across all servers in Azure.</span></span> <span data-ttu-id="669f3-157">Uživatelské jméno je pro uživatelský účet správce počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="669f3-157">The user name is for the initial database admin user account.</span></span> <span data-ttu-id="669f3-158">Zobrazí se výzva k vyberte heslo pro tohoto uživatele.</span><span class="sxs-lookup"><span data-stu-id="669f3-158">You are prompted to pick a password for this user.</span></span>

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

<span data-ttu-id="669f3-159">Při vytvoření databáze Azure PostgreSQL server pro rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="669f3-159">When the Azure Database for PostgreSQL server is created, the Azure CLI shows information similar to the following example:</span></span>

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

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a><span data-ttu-id="669f3-160">Vytvoření pravidla firewallu pro databázi Azure pro PostgreSQL serveru</span><span class="sxs-lookup"><span data-stu-id="669f3-160">Create a firewall rule for the Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="669f3-161">Spusťte následující příkaz rozhraní příkazového řádku Azure pro povolení přístupu k databázi z všechny IP adresy.</span><span class="sxs-lookup"><span data-stu-id="669f3-161">Run the following Azure CLI command to allow access to the database from all IP addresses.</span></span>

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

<span data-ttu-id="669f3-162">Rozhraní příkazového řádku Azure potvrdí vytvoření pravidla brány firewall se výstup podobný v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="669f3-162">The Azure CLI confirms the firewall rule creation with output similar to the following example:</span></span>

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

## <a name="connect-your-python-flask-application-to-the-database"></a><span data-ttu-id="669f3-163">Připojení k databázi aplikace Python Flask</span><span class="sxs-lookup"><span data-stu-id="669f3-163">Connect your Python Flask application to the database</span></span>

<span data-ttu-id="669f3-164">V tomto kroku připojíte vaši ukázkovou aplikaci Python Flask pro Azure databázi PostgreSQL serveru, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="669f3-164">In this step, you connect your Python Flask sample application to the Azure Database for PostgreSQL server you created.</span></span>

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a><span data-ttu-id="669f3-165">Vytvořit prázdnou databázi a nastavení nového uživatele databáze aplikace</span><span class="sxs-lookup"><span data-stu-id="669f3-165">Create an empty database and set up a new database application user</span></span>

<span data-ttu-id="669f3-166">Vytvořte uživatele databáze s přístupem k jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="669f3-166">Create a database user with access to a single database only.</span></span> <span data-ttu-id="669f3-167">Tyto přihlašovací údaje použijete k neudělujte aplikace úplný přístup k serveru.</span><span class="sxs-lookup"><span data-stu-id="669f3-167">You'll use these credentials to avoid giving the application full access to the server.</span></span>

<span data-ttu-id="669f3-168">Připojení k databázi (se zobrazí výzva k zadání hesla správce).</span><span class="sxs-lookup"><span data-stu-id="669f3-168">Connect to the database (you're prompted for your admin password).</span></span>

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

<span data-ttu-id="669f3-169">Vytvoření databáze a uživatele z příkazového řádku PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="669f3-169">Create the database and user from the PostgreSQL CLI.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

<span data-ttu-id="669f3-170">Typ *\q* ukončíte klienta PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="669f3-170">Type *\q* to exit the PostgreSQL client.</span></span>

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a><span data-ttu-id="669f3-171">Testování aplikace místně v databázi Azure PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="669f3-171">Test the application locally against the Azure PostgreSQL database</span></span> 

<span data-ttu-id="669f3-172">Po návratu teď do *aplikace* složky klonovaný úložiště Github, můžete spustit aplikace Python Flask aktualizací proměnné prostředí databáze.</span><span class="sxs-lookup"><span data-stu-id="669f3-172">Going back now to the *app* folder of the cloned Github repository, you can run the Python Flask application by updating the database environment variables.</span></span>

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="669f3-173">Po úplným načtením aplikace se zobrazí podobná následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="669f3-173">When the app is fully loaded, you see something similar to the following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

<span data-ttu-id="669f3-174">Přejděte do http://127.0.0.1:5000 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="669f3-174">Navigate to http://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="669f3-175">Klikněte na tlačítko **zaregistrovat!**</span><span class="sxs-lookup"><span data-stu-id="669f3-175">Click **Register!**</span></span> <span data-ttu-id="669f3-176">a vytvořit testovací registrace.</span><span class="sxs-lookup"><span data-stu-id="669f3-176">and create a test registration.</span></span> <span data-ttu-id="669f3-177">Jsou nyní zápis dat do databáze v Azure.</span><span class="sxs-lookup"><span data-stu-id="669f3-177">You are now writing data to the database in Azure.</span></span>

![Aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a><span data-ttu-id="669f3-179">Spuštění aplikace z kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="669f3-179">Running the application from a Docker Container</span></span>

<span data-ttu-id="669f3-180">Sestavení Docker kontejneru bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="669f3-180">Build the Docker container image.</span></span>

```bash
cd ..
docker build -t flask-postgresql-sample .
```

<span data-ttu-id="669f3-181">Docker zobrazí potvrzení, že úspěšně vytvořil kontejneru.</span><span class="sxs-lookup"><span data-stu-id="669f3-181">Docker displays a confirmation that it successfully created the container.</span></span>

```bash
Successfully built 7548f983a36b
```

<span data-ttu-id="669f3-182">Přidání proměnné prostředí databáze do souboru proměnné prostředí *db.env*.</span><span class="sxs-lookup"><span data-stu-id="669f3-182">Add database environment variables to an environment variable file *db.env*.</span></span> <span data-ttu-id="669f3-183">Aplikace se připojí k produkční databázi PostgreSQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="669f3-183">The app will connect to the PostgreSQL production database in Azure.</span></span>

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

<span data-ttu-id="669f3-184">Spusťte aplikaci v rámci kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="669f3-184">Run the app from within the Docker container.</span></span> <span data-ttu-id="669f3-185">Následující příkaz určuje soubor proměnných prostředí a Flask výchozí port 5000 se mapuje na místní port 5000.</span><span class="sxs-lookup"><span data-stu-id="669f3-185">The following command specifies the environment variable file and maps the default Flask port 5000 to local port 5000.</span></span>

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

<span data-ttu-id="669f3-186">Výstup se podobá jste viděli dříve.</span><span class="sxs-lookup"><span data-stu-id="669f3-186">The output is similar to what you saw earlier.</span></span> <span data-ttu-id="669f3-187">Ale migrace počáteční databáze už je nutné provést a proto bude přeskočena.</span><span class="sxs-lookup"><span data-stu-id="669f3-187">However, the initial database migration no longer needs to be performed and therefore is skipped.</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

<span data-ttu-id="669f3-188">Databáze již obsahuje registrace, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="669f3-188">The database already contains the registration you created previously.</span></span>

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a><span data-ttu-id="669f3-190">Odešlete do kontejneru registru kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="669f3-190">Upload the Docker container to a container registry</span></span>

<span data-ttu-id="669f3-191">V tomto kroku nahrajte kontejner Docker do registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="669f3-191">In this step, you upload the Docker container to a container registry.</span></span> <span data-ttu-id="669f3-192">Budete používat Azure kontejneru registru, ale můžete také použít další oblíbených těch, jako je například Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="669f3-192">You'll use Azure Container Registry, but you could also use other popular ones such as Docker Hub.</span></span>

### <a name="create-an-azure-container-registry"></a><span data-ttu-id="669f3-193">Vytvoření služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="669f3-193">Create an Azure Container Registry</span></span>

<span data-ttu-id="669f3-194">V následujícím příkazu k vytvoření kontejneru registru nahradit  *\<registry_name >* s názvem registru jedinečný kontejner Azure podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="669f3-194">In the following command to create a container registry replace *\<registry_name>* with a unique Azure container registry name of your choice.</span></span>

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

<span data-ttu-id="669f3-195">Výstup</span><span class="sxs-lookup"><span data-stu-id="669f3-195">Output</span></span>
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

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a><span data-ttu-id="669f3-196">Načtení registru přihlašovací údaje pro vkládání a stahování imagí Dockeru</span><span class="sxs-lookup"><span data-stu-id="669f3-196">Retrieve the registry credentials for pushing and pulling Docker images</span></span>

<span data-ttu-id="669f3-197">Zobrazíte registru přihlašovací údaje, nejprve povolte režim správce.</span><span class="sxs-lookup"><span data-stu-id="669f3-197">To show registry credentials, enable admin mode first.</span></span>

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

<span data-ttu-id="669f3-198">Zobrazí dvě hesla.</span><span class="sxs-lookup"><span data-stu-id="669f3-198">You see two passwords.</span></span> <span data-ttu-id="669f3-199">Poznamenejte si uživatelské jméno a heslo první.</span><span class="sxs-lookup"><span data-stu-id="669f3-199">Make note of the user name and the first password.</span></span>

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

### <a name="upload-your-docker-container-to-azure-container-registry"></a><span data-ttu-id="669f3-200">Nahrát váš kontejner Docker do registru kontejner Azure</span><span class="sxs-lookup"><span data-stu-id="669f3-200">Upload your Docker container to Azure Container Registry</span></span>

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a><span data-ttu-id="669f3-201">Nasaďte aplikaci Docker Python Flask do Azure</span><span class="sxs-lookup"><span data-stu-id="669f3-201">Deploy the Docker Python Flask application to Azure</span></span>

<span data-ttu-id="669f3-202">V tomto kroku nasadíte Docker na základě kontejneru aplikace Python Flask do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="669f3-202">In this step, you deploy your Docker container-based Python Flask application to Azure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="669f3-203">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="669f3-203">Create an App Service plan</span></span>

<span data-ttu-id="669f3-204">Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="669f3-204">Create an App Service plan with the [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="669f3-205">Následující příklad vytvoří plán aplikační služby se systémem Linux s názvem *myAppServicePlan* pomocí S1 cenová úroveň:</span><span class="sxs-lookup"><span data-stu-id="669f3-205">The following example creates a Linux-based App Service plan named *myAppServicePlan* using the S1 pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

<span data-ttu-id="669f3-206">Když je vytvořen plán služby App Service, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="669f3-206">When the App Service plan is created, the Azure CLI shows information similar to the following example:</span></span>

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

### <a name="create-a-web-app"></a><span data-ttu-id="669f3-207">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="669f3-207">Create a web app</span></span>

<span data-ttu-id="669f3-208">Vytvoření webové aplikace v *myAppServicePlan* plán služby App Service pomocí [az webapp vytvořit](/cli/azure/webapp#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="669f3-208">Create a web app in the *myAppServicePlan* App Service plan with the [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="669f3-209">Webová aplikace získáte hostování místa k nasazení kódu a poskytuje adresu URL zobrazení nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="669f3-209">The web app gives you a hosting space to deploy your code and provides a URL for you to view the deployed application.</span></span> <span data-ttu-id="669f3-210">Použijte k vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="669f3-210">Use  to create the web app.</span></span> 

<span data-ttu-id="669f3-211">V následujícím příkazu nahraďte  *\<app_name >* zástupný symbol s jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="669f3-211">In the following command, replace the *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="669f3-212">Tento název je součástí výchozí adresa URL pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="669f3-212">This name is part of the default URL for the web app, so the name needs to be unique across all apps in Azure App Service.</span></span> 

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="669f3-213">Po vytvoření webové aplikace se v rozhraní příkazového řádku Azure CLI zobrazí podobné informace jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="669f3-213">When the web app has been created, the Azure CLI shows information similar to the following example:</span></span> 

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

### <a name="configure-the-database-environment-variables"></a><span data-ttu-id="669f3-214">Konfigurace databáze proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="669f3-214">Configure the database environment variables</span></span>

<span data-ttu-id="669f3-215">V tomto kurzu definované proměnné prostředí pro připojení k vaší databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="669f3-215">Earlier in the tutorial, you defined environment variables to connect to your PostgreSQL database.</span></span>

<span data-ttu-id="669f3-216">Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="669f3-216">In App Service, you set environment variables as _app settings_ by using the [az webapp config appsettings set](/cli/azure/webapp/config#set) command.</span></span> 

<span data-ttu-id="669f3-217">Následující příklad určuje podrobnosti připojení databáze jako nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="669f3-217">The following example specifies the database connection details as app settings.</span></span> <span data-ttu-id="669f3-218">Používá také *PORT* proměnnou mapu PORT 5000 z vaší kontejner Docker pro příjem provozu HTTP na portu 80.</span><span class="sxs-lookup"><span data-stu-id="669f3-218">It also uses the *PORT* variable to map PORT 5000 from your Docker Container to receive HTTP traffic on PORT 80.</span></span>

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a><span data-ttu-id="669f3-219">Konfigurace nasazení kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="669f3-219">Configure Docker container deployment</span></span> 

<span data-ttu-id="669f3-220">Služby App Service může automaticky stáhnout a spusťte kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="669f3-220">AppService can automatically download and run a Docker container.</span></span>

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

<span data-ttu-id="669f3-221">Kdykoli aktualizovat kontejner Docker nebo změnit nastavení, restartujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="669f3-221">Whenever you update the Docker container or change the settings, restart the app.</span></span> <span data-ttu-id="669f3-222">Restartování zajišťuje, že všechna nastavení použita a kontejneru nejnovější pocházejí z registru.</span><span class="sxs-lookup"><span data-stu-id="669f3-222">Restarting ensures that all settings are applied and the latest container is pulled from the registry.</span></span>

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="669f3-223">Přejděte do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="669f3-223">Browse to the Azure web app</span></span> 

<span data-ttu-id="669f3-224">Přejděte do nasazené webové aplikace pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="669f3-224">Browse to the deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> <span data-ttu-id="669f3-225">Webové aplikace trvá déle, načíst, protože má kontejneru se stáhnou a spustí po změně konfigurace kontejneru.</span><span class="sxs-lookup"><span data-stu-id="669f3-225">The web app takes longer to load because the container has to be downloaded and started after the container configuration is changed.</span></span>

<span data-ttu-id="669f3-226">Zobrazí dříve zaregistrovaný hosté, které byly uloženy do Azure produkční databázi v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="669f3-226">You see previously registered guests that were saved to the Azure production database in the previous step.</span></span>

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-app-deployed.png)

<span data-ttu-id="669f3-228">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="669f3-228">**Congratulations!**</span></span> <span data-ttu-id="669f3-229">Používáte Docker aplikace Python Flask na základě kontejneru ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="669f3-229">You're running a Docker container-based Python Flask app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="669f3-230">Aktualizace datový model a znovu ho zaveďte</span><span class="sxs-lookup"><span data-stu-id="669f3-230">Update data model and redeploy</span></span>

<span data-ttu-id="669f3-231">V tomto kroku přidáte počtu účastníků k registraci jednotlivých událostí aktualizací modelu hosta.</span><span class="sxs-lookup"><span data-stu-id="669f3-231">In this step, you add the number of attendees to each event registration by updating the Guest model.</span></span>

<span data-ttu-id="669f3-232">Podívejte se *0,2 Migrace* verzi pomocí následujícího příkazu git:</span><span class="sxs-lookup"><span data-stu-id="669f3-232">Check out the *0.2-migration* release with the following git command:</span></span>

```bash
git checkout tags/0.2-migration
```

<span data-ttu-id="669f3-233">Tato verze již provedli nezbytné změny zobrazení, řadiče a modelu.</span><span class="sxs-lookup"><span data-stu-id="669f3-233">This release already made the necessary changes to views, controllers, and model.</span></span> <span data-ttu-id="669f3-234">Zahrnuje také migrace databáze generované prostřednictvím *destilační přístroj* (`flask db migrate`).</span><span class="sxs-lookup"><span data-stu-id="669f3-234">It also includes a database migration generated via *alembic* (`flask db migrate`).</span></span> <span data-ttu-id="669f3-235">Zobrazí všechny změny provedené pomocí následujícího příkazu git:</span><span class="sxs-lookup"><span data-stu-id="669f3-235">You can see all changes made via the following git command:</span></span>

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="669f3-236">Otestujte provedené změny místně</span><span class="sxs-lookup"><span data-stu-id="669f3-236">Test your changes locally</span></span>

<span data-ttu-id="669f3-237">Spusťte následující příkazy k testování změny místně spuštěním flask server.</span><span class="sxs-lookup"><span data-stu-id="669f3-237">Run the following commands to test your changes locally by running the flask server.</span></span>

<span data-ttu-id="669f3-238">Mac / Linux:</span><span class="sxs-lookup"><span data-stu-id="669f3-238">Mac / Linux:</span></span>
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="669f3-239">Přejděte do http://127.0.0.1:5000 v prohlížeči a prohlédněte si změny.</span><span class="sxs-lookup"><span data-stu-id="669f3-239">Navigate to http://127.0.0.1:5000 in your browser to view the changes.</span></span> <span data-ttu-id="669f3-240">Vytvořte testovací registrace.</span><span class="sxs-lookup"><span data-stu-id="669f3-240">Create a test registration.</span></span>

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a><span data-ttu-id="669f3-242">Publikování změn do Azure</span><span class="sxs-lookup"><span data-stu-id="669f3-242">Publish changes to Azure</span></span>

<span data-ttu-id="669f3-243">Vytvořit novou bitovou kopii docker, push registru kontejneru a restartujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="669f3-243">Build the new docker image, push it to the container registry, and restart the app.</span></span>

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

<span data-ttu-id="669f3-244">Přejděte do vaší webové aplikace Azure a znovu vyzkoušet nové funkce.</span><span class="sxs-lookup"><span data-stu-id="669f3-244">Navigate to your Azure web app and try out the new functionality again.</span></span> <span data-ttu-id="669f3-245">Vytvořte další registrace události.</span><span class="sxs-lookup"><span data-stu-id="669f3-245">Create another event registration.</span></span>

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask aplikace v Azure App Service](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="669f3-247">Správa Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="669f3-247">Manage your Azure web app</span></span>

<span data-ttu-id="669f3-248">Přejděte na [portál Azure](https://portal.azure.com) zobrazíte webové aplikace, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="669f3-248">Go to the [Azure portal](https://portal.azure.com) to see the web app you created.</span></span>

<span data-ttu-id="669f3-249">V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="669f3-249">From the left menu, click **App Services**, then click the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-docker-python-postgresql-app/app-resource.png)

<span data-ttu-id="669f3-251">Ve výchozím nastavení, zobrazí na portálu vaší webové aplikace **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="669f3-251">By default, the portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="669f3-252">Tato stránka poskytuje přehled, jak si vaše aplikace stojí.</span><span class="sxs-lookup"><span data-stu-id="669f3-252">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="669f3-253">Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="669f3-253">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="669f3-254">Karty na levé straně stránky zobrazí stránek jinou konfiguraci, že můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="669f3-254">The tabs on the left side of the page show the different configuration pages you can open.</span></span>

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a><span data-ttu-id="669f3-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="669f3-256">Next steps</span></span>

<span data-ttu-id="669f3-257">Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="669f3-257">Advance to the next tutorial to learn how to map a custom DNS name to your web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="669f3-258">Mapování existujícího vlastního názvu DNS na Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="669f3-258">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
