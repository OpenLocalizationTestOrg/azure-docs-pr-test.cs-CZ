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
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a><span data-ttu-id="3639a-103">Vytvoření webové aplikace Node.js a MongoDB v Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-103">Build a Node.js and MongoDB web app in Azure</span></span>

<span data-ttu-id="3639a-104">Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby.</span><span class="sxs-lookup"><span data-stu-id="3639a-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="3639a-105">Tento kurz ukazuje, jak vytvořit webovou aplikaci Node.js v Azure a připojte ho k databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3639a-105">This tutorial shows how to create a Node.js web app in Azure and connect it to a MongoDB database.</span></span> <span data-ttu-id="3639a-106">Když jste hotovi, budete mít střední aplikace (MongoDB, Express, AngularJS a Node.js) spuštěná v [Azure App Service](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3639a-106">When you're done, you'll have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running in [Azure App Service](app-service-web-overview.md).</span></span> <span data-ttu-id="3639a-107">Pro jednoduchost, ukázková aplikace používá [MEAN.js webová architektura](http://meanjs.org/).</span><span class="sxs-lookup"><span data-stu-id="3639a-107">For simplicity, the sample application uses the [MEAN.js web framework](http://meanjs.org/).</span></span>

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="3639a-109">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="3639a-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3639a-110">Vytvořit databázi MongoDB v Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-110">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="3639a-111">Připojení aplikace Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="3639a-111">Connect a Node.js app to MongoDB</span></span>
> * <span data-ttu-id="3639a-112">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-112">Deploy the app to Azure</span></span>
> * <span data-ttu-id="3639a-113">Aktualizovat datový model a aplikaci znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="3639a-113">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="3639a-114">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-114">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="3639a-115">Spravovat aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-115">Manage the app in the Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3639a-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3639a-116">Prerequisites</span></span>

<span data-ttu-id="3639a-117">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3639a-117">To complete this tutorial:</span></span>

1. <span data-ttu-id="3639a-118">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="3639a-118">[Install Git](https://git-scm.com/)</span></span>
1. <span data-ttu-id="3639a-119">[Nainstalovat Node.js a NPM](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="3639a-119">[Install Node.js and NPM](https://nodejs.org/)</span></span>
1. <span data-ttu-id="3639a-120">[Nainstalujte Gulp.js](http://gulpjs.com/) (požadavku [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span><span class="sxs-lookup"><span data-stu-id="3639a-120">[Install Gulp.js](http://gulpjs.com/) (required by [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span></span>
1. [<span data-ttu-id="3639a-121">Nainstalujte a spusťte MongoDB Community Edition</span><span class="sxs-lookup"><span data-stu-id="3639a-121">Install and run MongoDB Community Edition</span></span>](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3639a-122">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3639a-122">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="3639a-123">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="3639a-123">Run `az --version` to find the version.</span></span> <span data-ttu-id="3639a-124">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3639a-124">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-mongodb"></a><span data-ttu-id="3639a-125">Test místní MongoDB</span><span class="sxs-lookup"><span data-stu-id="3639a-125">Test local MongoDB</span></span>

<span data-ttu-id="3639a-126">Otevřete okno terminálu a `cd` k `bin` adresář instalace MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3639a-126">Open the terminal window and `cd` to the `bin` directory of your MongoDB installation.</span></span> <span data-ttu-id="3639a-127">Chcete-li spustit všechny příkazy v tomto kurzu můžete toto okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="3639a-127">You can use this terminal window to run all the commands in this tutorial.</span></span>

<span data-ttu-id="3639a-128">Spustit `mongo` v terminálu pro připojení k místní server MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3639a-128">Run `mongo` in the terminal to connect to your local MongoDB server.</span></span>

```bash
mongo
```

<span data-ttu-id="3639a-129">Pokud připojení úspěšné, pak databázi MongoDB je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="3639a-129">If your connection is successful, then your MongoDB database is already running.</span></span> <span data-ttu-id="3639a-130">Pokud ne, ujistěte se, zda je spuštěná místní databázi MongoDB podle kroků v [nainstalujte MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span><span class="sxs-lookup"><span data-stu-id="3639a-130">If not, make sure that your local MongoDB database is started by following the steps at [Install MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span></span> <span data-ttu-id="3639a-131">Často je nainstalovaná MongoDB, ale potřebujete spusťte ji spuštěním `mongod`.</span><span class="sxs-lookup"><span data-stu-id="3639a-131">Often, MongoDB is installed, but you still need to start it by running `mongod`.</span></span> 

<span data-ttu-id="3639a-132">Po dokončení testování vaší databázi MongoDB, zadejte `Ctrl+C` v terminálu.</span><span class="sxs-lookup"><span data-stu-id="3639a-132">When you're done testing your MongoDB database, type `Ctrl+C` in the terminal.</span></span> 

## <a name="create-local-nodejs-app"></a><span data-ttu-id="3639a-133">Vytvořit místní aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="3639a-133">Create local Node.js app</span></span>

<span data-ttu-id="3639a-134">V tomto kroku nastavíte místní projekt Node.js.</span><span class="sxs-lookup"><span data-stu-id="3639a-134">In this step, you set up the local Node.js project.</span></span>

### <a name="clone-the-sample-application"></a><span data-ttu-id="3639a-135">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="3639a-135">Clone the sample application</span></span>

<span data-ttu-id="3639a-136">V okně terminálu `cd` do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="3639a-136">In the terminal window, `cd` to a working directory.</span></span>  

<span data-ttu-id="3639a-137">Ukázkové úložiště naklonujete spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="3639a-137">Run the following command to clone the sample repository.</span></span> 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

<span data-ttu-id="3639a-138">Tato ukázka úložiště obsahuje kopii [MEAN.js úložiště](https://github.com/meanjs/mean).</span><span class="sxs-lookup"><span data-stu-id="3639a-138">This sample repository contains a copy of the [MEAN.js repository](https://github.com/meanjs/mean).</span></span> <span data-ttu-id="3639a-139">Je upravit pro spouštění v App Service (Další informace najdete v tématu úložiště MEAN.js [souboru README](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span><span class="sxs-lookup"><span data-stu-id="3639a-139">It is modified to run on App Service (for more information, see the MEAN.js repository [README file](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span></span>

### <a name="run-the-application"></a><span data-ttu-id="3639a-140">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="3639a-140">Run the application</span></span>

<span data-ttu-id="3639a-141">Spusťte následující příkazy pro instalaci požadovaných balíčků a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3639a-141">Run the following commands to install the required packages and start the application.</span></span>

```bash
cd meanjs
npm install
npm start
```

<span data-ttu-id="3639a-142">Po úplným načtením aplikace se zobrazí podobná následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="3639a-142">When the app is fully loaded, you see something similar to the following message:</span></span>

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

<span data-ttu-id="3639a-143">Přejděte na http://localhost: 3000 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3639a-143">Navigate to http://localhost:3000 in a browser.</span></span> <span data-ttu-id="3639a-144">Klikněte na tlačítko **zaregistrovat** v horní nabídce a vytvoření zkušebního uživatele.</span><span class="sxs-lookup"><span data-stu-id="3639a-144">Click **Sign Up** in the top menu and create a test user.</span></span> 

<span data-ttu-id="3639a-145">Ukázková aplikace MEAN.js ukládá data uživatelů v databázi.</span><span class="sxs-lookup"><span data-stu-id="3639a-145">The MEAN.js sample application stores user data in the database.</span></span> <span data-ttu-id="3639a-146">Pokud jste při vytváření uživatele a přihlášení úspěšné, pak aplikace je zápis dat do místní databáze MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3639a-146">If you are successful at creating a user and signing in, then your app is writing data to the local MongoDB database.</span></span>

![Aplikace MEAN.js se úspěšně připojí k databázi MongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

<span data-ttu-id="3639a-148">Vyberte **správce > Správa článků** přidat některé články.</span><span class="sxs-lookup"><span data-stu-id="3639a-148">Select **Admin > Manage Articles** to add some articles.</span></span>

<span data-ttu-id="3639a-149">Kdykoli zastavit Node.js, stiskněte klávesu `Ctrl+C` v terminálu.</span><span class="sxs-lookup"><span data-stu-id="3639a-149">To stop Node.js at any time, press `Ctrl+C` in the terminal.</span></span> 

## <a name="create-production-mongodb"></a><span data-ttu-id="3639a-150">Vytvořit produkční MongoDB</span><span class="sxs-lookup"><span data-stu-id="3639a-150">Create production MongoDB</span></span>

<span data-ttu-id="3639a-151">V tomto kroku vytvoříte databázi MongoDB v Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-151">In this step, you create a MongoDB database in Azure.</span></span> <span data-ttu-id="3639a-152">Po nasazení aplikace do Azure se používá tato databáze cloudu.</span><span class="sxs-lookup"><span data-stu-id="3639a-152">When your app is deployed to Azure, it uses this cloud database.</span></span>

<span data-ttu-id="3639a-153">Pro MongoDB, tento kurz používá [Azure Cosmos DB](/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="3639a-153">For MongoDB, this tutorial uses [Azure Cosmos DB](/azure/documentdb/).</span></span> <span data-ttu-id="3639a-154">Cosmos DB podporuje připojení klienta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3639a-154">Cosmos DB supports MongoDB client connections.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="3639a-155">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-155">Log in to Azure</span></span>

<span data-ttu-id="3639a-156">Pomocí rozhraní Azure CLI 2.0 vytvoříte prostředky potřebné k hostování vaší aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-156">You'll use the Azure CLI 2.0 to create the resources needed to host your app in Azure.</span></span> <span data-ttu-id="3639a-157">Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="3639a-157">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span>

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="3639a-158">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3639a-158">Create a resource group</span></span>

<span data-ttu-id="3639a-159">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3639a-159">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="3639a-160">Následující příklad vytvoří skupinu prostředků pro oblast Západní Evropa.</span><span class="sxs-lookup"><span data-stu-id="3639a-160">The following example creates a resource group in the West Europe region.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

<span data-ttu-id="3639a-161">Použití [az služby App Service seznamu umístění](/cli/azure/appservice#list-locations) příkaz rozhraní příkazového řádku Azure k seznamu dostupných umístění.</span><span class="sxs-lookup"><span data-stu-id="3639a-161">Use the [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command to list available locations.</span></span> 

### <a name="create-a-cosmos-db-account"></a><span data-ttu-id="3639a-162">Vytvoření účtu Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3639a-162">Create a Cosmos DB account</span></span>

<span data-ttu-id="3639a-163">Vytvoření účtu Cosmos DB s [vytvořit az cosmosdb](/cli/azure/cosmosdb#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3639a-163">Create a Cosmos DB account with the [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="3639a-164">V následujícím příkazu nahraďte jedinečný název databáze Cosmos  *\<cosmosdb_name >* zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="3639a-164">In the following command, substitute a unique Cosmos DB name for the *\<cosmosdb_name>* placeholder.</span></span> <span data-ttu-id="3639a-165">Tento název se používá jako součást Cosmos DB koncového bodu, `https://<cosmosdb_name>.documents.azure.com/`, takže název musí být jedinečný v rámci všech Cosmos DB účty v Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-165">This name is used as the part of the Cosmos DB endpoint, `https://<cosmosdb_name>.documents.azure.com/`, so the name needs to be unique across all Cosmos DB accounts in Azure.</span></span> <span data-ttu-id="3639a-166">Název musí obsahovat jenom malá písmena, číslice a pomlčky (-) a musí být v rozmezí 3 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="3639a-166">The name must contain only lowercase letters, numbers, and the hyphen (-) character, and must be between 3 and 50 characters long.</span></span>

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

<span data-ttu-id="3639a-167">*– Druhu MongoDB* parametr povolí připojení klientů MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3639a-167">The *--kind MongoDB* parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="3639a-168">Při vytvoření účtu Cosmos DB rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3639a-168">When the Cosmos DB account is created, the Azure CLI shows information similar to the following example:</span></span>

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

## <a name="connect-app-to-production-mongodb"></a><span data-ttu-id="3639a-169">Připojení aplikace do produkčního prostředí MongoDB</span><span class="sxs-lookup"><span data-stu-id="3639a-169">Connect app to production MongoDB</span></span>

<span data-ttu-id="3639a-170">V tomto kroku připojíte MEAN.js ukázkovou aplikaci do databáze Cosmos databáze, kterou jste právě vytvořili, pomocí připojovacího řetězce MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3639a-170">In this step, you connect your MEAN.js sample application to the Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

### <a name="retrieve-the-database-key"></a><span data-ttu-id="3639a-171">Načíst klíč databáze</span><span class="sxs-lookup"><span data-stu-id="3639a-171">Retrieve the database key</span></span>

<span data-ttu-id="3639a-172">Pro připojení k databázi Cosmos DB, musíte klíč databáze.</span><span class="sxs-lookup"><span data-stu-id="3639a-172">To connect to the Cosmos DB database, you need the database key.</span></span> <span data-ttu-id="3639a-173">Pro načtení primárního klíče použijte příkaz [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys).</span><span class="sxs-lookup"><span data-stu-id="3639a-173">Use the [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command to retrieve the primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

<span data-ttu-id="3639a-174">Rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3639a-174">The Azure CLI shows information similar to the following example:</span></span>

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Zkopírujte hodnotu `primaryMasterKey`. <span data-ttu-id="3639a-176">Tyto informace budete potřebovat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="3639a-176">You need this information in the next step.</span></span>

<a name="devconfig"></a>
### <a name="configure-the-connection-string-in-your-nodejs-application"></a><span data-ttu-id="3639a-177">Konfigurace připojovacího řetězce v aplikaci Node.js</span><span class="sxs-lookup"><span data-stu-id="3639a-177">Configure the connection string in your Node.js application</span></span>

<span data-ttu-id="3639a-178">Ve svém úložišti MEAN.js otevřete _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="3639a-178">In your MEAN.js repository, open _config/env/production.js_.</span></span>

<span data-ttu-id="3639a-179">V `db` objektu, aktualizujte hodnotu `uri`:</span><span class="sxs-lookup"><span data-stu-id="3639a-179">In the `db` object, update the value of `uri`:</span></span>

* <span data-ttu-id="3639a-180">Nahraďte dva  *\<cosmosdb_name >* zástupných symbolů nahraďte názvem databáze Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3639a-180">Replace the two *\<cosmosdb_name>* placeholders with your Cosmos DB database name.</span></span>
* <span data-ttu-id="3639a-181">Nahraďte  *\<primary_master_key >* zástupný text klíčem, který jste zkopírovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3639a-181">Replace the *\<primary_master_key>* placeholder with the key you copied in the previous step.</span></span>

<span data-ttu-id="3639a-182">Následující kód ukazuje `db` objektu:</span><span class="sxs-lookup"><span data-stu-id="3639a-182">The following code shows the `db` object:</span></span>

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

<span data-ttu-id="3639a-183">`ssl=true` Možnost je povinná, protože [Cosmos DB vyžaduje SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="3639a-183">The `ssl=true` option is required because [Cosmos DB requires SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 

<span data-ttu-id="3639a-184">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="3639a-184">Save your changes.</span></span>

### <a name="test-the-application-in-production-mode"></a><span data-ttu-id="3639a-185">Testování aplikace v provozním režimu</span><span class="sxs-lookup"><span data-stu-id="3639a-185">Test the application in production mode</span></span> 

<span data-ttu-id="3639a-186">Spusťte následující příkaz k minifikaci a sady skriptů pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="3639a-186">Run the following command to minify and bundle scripts for the production environment.</span></span> <span data-ttu-id="3639a-187">Tento proces generuje soubory potřebné v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3639a-187">This process generates the files needed by the production environment.</span></span>

```bash
gulp prod
```

<span data-ttu-id="3639a-188">Pomocí následujícího příkazu použít připojovací řetězec, který jste nakonfigurovali v _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="3639a-188">Run the following command to use the connection string you configured in _config/env/production.js_.</span></span>

```bash
NODE_ENV=production node server.js
```

<span data-ttu-id="3639a-189">`NODE_ENV=production`Nastaví proměnné prostředí, která sděluje Node.js ke spuštění v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3639a-189">`NODE_ENV=production` sets the environment variable that tells Node.js to run in the production environment.</span></span>  <span data-ttu-id="3639a-190">`node server.js`Spustí server Node.js s `server.js` v kořenovém úložišti.</span><span class="sxs-lookup"><span data-stu-id="3639a-190">`node server.js` starts the Node.js server with `server.js` in your repository root.</span></span> <span data-ttu-id="3639a-191">Toto je, jak aplikace Node.js je načten do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-191">This is how your Node.js application is loaded in Azure.</span></span> 

<span data-ttu-id="3639a-192">Když aplikace je načtena, ujistěte se, zda je spuštěna v provozním prostředí:</span><span class="sxs-lookup"><span data-stu-id="3639a-192">When the app is loaded, check to make sure that it's running in the production environment:</span></span>

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

<span data-ttu-id="3639a-193">Přejděte do http://localhost:8443 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3639a-193">Navigate to http://localhost:8443 in a browser.</span></span> <span data-ttu-id="3639a-194">Klikněte na tlačítko **zaregistrovat** v horní nabídce a vytvoření zkušebního uživatele.</span><span class="sxs-lookup"><span data-stu-id="3639a-194">Click **Sign Up** in the top menu and create a test user.</span></span> <span data-ttu-id="3639a-195">Pokud jste vytváření uživatele a přihlášení úspěšné, pak aplikace je zápis dat do databáze Cosmos DB v Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-195">If you are successful creating a user and signing in, then your app is writing data to the Cosmos DB database in Azure.</span></span> 

<span data-ttu-id="3639a-196">V terminálu, zastavte Node.js zadáním `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="3639a-196">In the terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

## <a name="deploy-app-to-azure"></a><span data-ttu-id="3639a-197">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-197">Deploy app to Azure</span></span>

<span data-ttu-id="3639a-198">V tomto kroku nasadíte aplikace Node.js MongoDB připojení do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3639a-198">In this step, you deploy your MongoDB-connected Node.js application to Azure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="3639a-199">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="3639a-199">Create an App Service plan</span></span>

<span data-ttu-id="3639a-200">Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3639a-200">Create an App Service plan with the [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="3639a-201">Následující příklad vytvoří plán služby App Service s názvem _myAppServicePlan_ pomocí **volné** cenové úrovně:</span><span class="sxs-lookup"><span data-stu-id="3639a-201">The following example creates an App Service plan named _myAppServicePlan_ using the **FREE** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="3639a-202">Když je vytvořen plán služby App Service, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3639a-202">When the App Service plan is created, the Azure CLI shows information similar to the following example:</span></span>

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

### <a name="create-a-web-app"></a><span data-ttu-id="3639a-203">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3639a-203">Create a web app</span></span>

<span data-ttu-id="3639a-204">Vytvoření webové aplikace v `myAppServicePlan` plán služby App Service pomocí [az webapp vytvořit](/cli/azure/webapp#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3639a-204">Create a web app in the `myAppServicePlan` App Service plan with the [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="3639a-205">Webová aplikace získáte hostování místa k nasazení kódu a poskytuje adresu URL zobrazení nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="3639a-205">The web app gives you a hosting space to deploy your code and provides a URL for you to view the deployed application.</span></span> <span data-ttu-id="3639a-206">Použijte k vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3639a-206">Use  to create the web app.</span></span> 

<span data-ttu-id="3639a-207">V následujícím příkazu nahraďte  *\<app_name >* zástupný symbol s jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="3639a-207">In the following command, replace the *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="3639a-208">Tento název se používá jako součást výchozí adresa URL pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3639a-208">This name is used as the part of the default URL for the web app, so the name needs to be unique across all apps in Azure App Service.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="3639a-209">Po vytvoření webové aplikace se v rozhraní příkazového řádku Azure CLI zobrazí podobné informace jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="3639a-209">When the web app has been created, the Azure CLI shows information similar to the following example:</span></span> 

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

### <a name="configure-an-environment-variable"></a><span data-ttu-id="3639a-210">Nakonfigurujte proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="3639a-210">Configure an environment variable</span></span>

<span data-ttu-id="3639a-211">V tomto kurzu jste pevně zakódované připojení k databázi řetězce v _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="3639a-211">Earlier in the tutorial, you hardcoded the database connection string in _config/env/production.js_.</span></span> <span data-ttu-id="3639a-212">V souladu s nejlepším způsobem zabezpečení, které chcete uchovat tyto důvěrné osobní údaje z úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="3639a-212">In keeping with security best practice, you want to keep this sensitive data out of your Git repository.</span></span> <span data-ttu-id="3639a-213">Pro vaši aplikaci běžící v Azure budete používat proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3639a-213">For your app running in Azure, you'll use an environment variable instead.</span></span>

<span data-ttu-id="3639a-214">Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí [aktualizovat az webapp konfigurace appsettings](/cli/azure/webapp/config/appsettings#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3639a-214">In App Service, you set environment variables as _app settings_ by using the [az webapp config appsettings update](/cli/azure/webapp/config/appsettings#update) command.</span></span> 

<span data-ttu-id="3639a-215">Následující příklad konfiguruje `MONGODB_URI` nastavení aplikace v Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3639a-215">The following example configures a `MONGODB_URI` app setting in your Azure web app.</span></span> <span data-ttu-id="3639a-216">Nahraďte  *\<app_name >*,  *\<cosmosdb_name >*, a  *\<primary_master_key >* zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="3639a-216">Replace the *\<app_name>*, *\<cosmosdb_name>*, and *\<primary_master_key>* placeholders.</span></span>

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

<span data-ttu-id="3639a-217">V kódu Node.js, je přístup k nastavení této aplikace s `process.env.MONGODB_URI`, stejně, jako by přístup všechny proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3639a-217">In Node.js code, you access this app setting with `process.env.MONGODB_URI`, just like you would access any environment variable.</span></span> 

<span data-ttu-id="3639a-218">Teď vrátit zpět všechny změny _config/env/production.js_ pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="3639a-218">Now, undo your changes to _config/env/production.js_ with the following command:</span></span>

```bash
git checkout -- .
```

<span data-ttu-id="3639a-219">Otevřete _config/env/production.js_ znovu.</span><span class="sxs-lookup"><span data-stu-id="3639a-219">Open _config/env/production.js_ again.</span></span> <span data-ttu-id="3639a-220">Všimněte si, že výchozí MEAN.js aplikace je již nakonfigurován pro použití `MONGODB_URI` proměnné prostředí, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3639a-220">Note that the default MEAN.js app is already configured to use the `MONGODB_URI` environment variable that you created.</span></span>

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a><span data-ttu-id="3639a-221">Konfigurace nasazení místního gitu</span><span class="sxs-lookup"><span data-stu-id="3639a-221">Configure local git deployment</span></span> 

<span data-ttu-id="3639a-222">Použití [nastavený uživatel nasazení webapp az](/cli/azure/webapp/deployment/user#set) příkazu vytvořit přihlašovací údaje pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="3639a-222">Use the [az webapp deployment user set](/cli/azure/webapp/deployment/user#set) command to create credentials for deployment.</span></span>

<span data-ttu-id="3639a-223">Můžete nasadit aplikace do služby Azure App Service různými způsoby, včetně FTP, Git místní, GitHub, Visual Studio Team Services a BitBucket.</span><span class="sxs-lookup"><span data-stu-id="3639a-223">You can deploy your application to Azure App Service in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="3639a-224">Pro místní Git a FTP je potřeba mít nasazení uživateli nakonfigurovanému na serveru k ověření nasazení.</span><span class="sxs-lookup"><span data-stu-id="3639a-224">For FTP and local Git, it is necessary to have a deployment user configured on the server to authenticate your deployment.</span></span> <span data-ttu-id="3639a-225">Tento uživatel nasazení je úrovni účtu a se liší od účtu předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-225">This deployment user is account-level and is different from your Azure subscription account.</span></span> <span data-ttu-id="3639a-226">Potřebujete jenom jednou konfiguraci tohoto nasazení uživatele.</span><span class="sxs-lookup"><span data-stu-id="3639a-226">You only need to configure this deployment user once.</span></span>

<span data-ttu-id="3639a-227">V následujícím příkazu nahraďte hodnoty *\<user-name>* a *\<password>* novým uživatelským jménem a heslem.</span><span class="sxs-lookup"><span data-stu-id="3639a-227">In the following command, replace *\<user-name>* and *\<password>* with a new user name and password.</span></span> <span data-ttu-id="3639a-228">Uživatelské jméno musí být jedinečné.</span><span class="sxs-lookup"><span data-stu-id="3639a-228">The user name must be unique.</span></span> <span data-ttu-id="3639a-229">Heslo musí obsahovat aspoň osm znaků a musí v něm být použity minimálně dva z následujících typů znaků: písmena, čísla a symboly.</span><span class="sxs-lookup"><span data-stu-id="3639a-229">The password must be at least eight characters long, with two of the following three elements:  letters, numbers, symbols.</span></span> <span data-ttu-id="3639a-230">Pokud se zobrazí chyba ` 'Conflict'. Details: 409`, změňte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3639a-230">If you get a ` 'Conflict'. Details: 409` error, change the username.</span></span> <span data-ttu-id="3639a-231">Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.</span><span class="sxs-lookup"><span data-stu-id="3639a-231">If you get a ` 'Bad Request'. Details: 400` error, use a stronger password.</span></span>

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

<span data-ttu-id="3639a-232">Záznam uživatelského jména a hesla pro použití v dalších krocích při nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3639a-232">Record the user name and password for use in later steps when you deploy the app.</span></span>

<span data-ttu-id="3639a-233">Použití [az webapp nasazení zdroj konfigurace místní git](/cli/azure/webapp/deployment/source#config-local-git) příkaz, který nakonfiguruje místní Git přístup k webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-233">Use the [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command to configure local Git access to the Azure web app.</span></span> 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

<span data-ttu-id="3639a-234">Pokud uživatel nasazení je nakonfigurován, rozhraní příkazového řádku Azure zobrazuje adresu URL nasazení pro Azure webové aplikace v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="3639a-234">When the deployment user is configured, the Azure CLI shows the deployment URL for your Azure web app in the following format:</span></span>

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

<span data-ttu-id="3639a-235">Zkopírujte výstup z terminálu, jako se použije v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="3639a-235">Copy the output from the terminal, as it will be used in the next step.</span></span> 

### <a name="push-to-azure-from-git"></a><span data-ttu-id="3639a-236">Přenos z Gitu do Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-236">Push to Azure from Git</span></span>

<span data-ttu-id="3639a-237">Přidejte vzdálené úložiště Azure do místního úložiště Gitu.</span><span class="sxs-lookup"><span data-stu-id="3639a-237">Add an Azure remote to your local Git repository.</span></span> 

```bash
git remote add azure <paste_copied_url_here> 
```

<span data-ttu-id="3639a-238">Doručte do Azure vzdálené nasazení aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="3639a-238">Push to the Azure remote to deploy your Node.js application.</span></span> <span data-ttu-id="3639a-239">Zobrazí se výzva k zadání hesla, které jste zadali dříve v rámci vytváření nasazení uživatele.</span><span class="sxs-lookup"><span data-stu-id="3639a-239">You will be prompted for the password you supplied earlier as part of the creation of the deployment user.</span></span> 

```bash
git push azure master
```

<span data-ttu-id="3639a-240">Během nasazení Azure App Service komunikuje s Gitem jejím průběhu.</span><span class="sxs-lookup"><span data-stu-id="3639a-240">During deployment, Azure App Service communicates its progress with Git.</span></span>

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

<span data-ttu-id="3639a-241">Můžete si všimnout, že proces nasazení spouští [Gulp](http://gulpjs.com/) po `npm install`.</span><span class="sxs-lookup"><span data-stu-id="3639a-241">You may notice that the deployment process runs [Gulp](http://gulpjs.com/) after `npm install`.</span></span> <span data-ttu-id="3639a-242">Služby App Service nespustí Gulp nebo Grunt úloh během nasazení, takže toto úložiště ukázka má dva další soubory v jeho kořenový adresář povolit:</span><span class="sxs-lookup"><span data-stu-id="3639a-242">App Service does not run Gulp or Grunt tasks during deployment, so this sample repository has two additional files in its root directory to enable it:</span></span> 

- <span data-ttu-id="3639a-243">_.Deployment_ – tento soubor informuje služby App Service ke spuštění `bash deploy.sh` jako vlastní nasazení skriptu.</span><span class="sxs-lookup"><span data-stu-id="3639a-243">_.deployment_ - This file tells App Service to run `bash deploy.sh` as the custom deployment script.</span></span>
- <span data-ttu-id="3639a-244">_Deploy.SH_ -skript vlastní nasazení.</span><span class="sxs-lookup"><span data-stu-id="3639a-244">_deploy.sh_ - The custom deployment script.</span></span> <span data-ttu-id="3639a-245">Při kontrole souboru je se zobrazí, že běží `gulp prod` po `npm install` a `bower install`.</span><span class="sxs-lookup"><span data-stu-id="3639a-245">If you review the file, you will see that it runs `gulp prod` after `npm install` and `bower install`.</span></span> 

<span data-ttu-id="3639a-246">Tento postup můžete použít k přidání jakéhokoli kroku k nasazení na základě Git.</span><span class="sxs-lookup"><span data-stu-id="3639a-246">You can use this approach to add any step to your Git-based deployment.</span></span> <span data-ttu-id="3639a-247">Pokud restartujete Azure webové aplikace v libovolném bodě, služby App Service není spusťte znovu tyto úlohy automatizace.</span><span class="sxs-lookup"><span data-stu-id="3639a-247">If you restart your Azure web app at any point, App Service doesn't rerun these automation tasks.</span></span>

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="3639a-248">Přejděte do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-248">Browse to the Azure web app</span></span> 

<span data-ttu-id="3639a-249">Přejděte do nasazené webové aplikace pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3639a-249">Browse to the deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
``` 

<span data-ttu-id="3639a-250">Klikněte na tlačítko **zaregistrovat** v horní nabídce a vytvořte fiktivní uživatele.</span><span class="sxs-lookup"><span data-stu-id="3639a-250">Click **Sign Up** in the top menu and create a dummy user.</span></span> 

<span data-ttu-id="3639a-251">Pokud jste úspěšné a aplikace automaticky přihlásí do vytvořeného uživatele, MEAN.js aplikace v Azure má připojení k databázi MongoDB (Cosmos databáze).</span><span class="sxs-lookup"><span data-stu-id="3639a-251">If you are successful and the app automatically signs in to the created user, then your MEAN.js app in Azure has connectivity to the MongoDB (Cosmos DB) database.</span></span> 

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="3639a-253">Vyberte **správce > Správa článků** přidat některé články.</span><span class="sxs-lookup"><span data-stu-id="3639a-253">Select **Admin > Manage Articles** to add some articles.</span></span> 

<span data-ttu-id="3639a-254">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="3639a-254">**Congratulations!**</span></span> <span data-ttu-id="3639a-255">Používáte datové aplikace Node.js ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3639a-255">You're running a data-driven Node.js app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="3639a-256">Aktualizace datový model a znovu ho zaveďte</span><span class="sxs-lookup"><span data-stu-id="3639a-256">Update data model and redeploy</span></span>

<span data-ttu-id="3639a-257">V tomto kroku, můžete změnit `article` dat modelu a publikovat změny do Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-257">In this step, you change the `article` data model and publish your change to Azure.</span></span>

### <a name="update-the-data-model"></a><span data-ttu-id="3639a-258">Aktualizovat data modelu</span><span class="sxs-lookup"><span data-stu-id="3639a-258">Update the data model</span></span>

<span data-ttu-id="3639a-259">Otevřete _modules/articles/server/models/article.server.model.js_.</span><span class="sxs-lookup"><span data-stu-id="3639a-259">Open _modules/articles/server/models/article.server.model.js_.</span></span>

<span data-ttu-id="3639a-260">V `ArticleSchema`, přidejte `String` typu s názvem `comment`.</span><span class="sxs-lookup"><span data-stu-id="3639a-260">In `ArticleSchema`, add a `String` type called `comment`.</span></span> <span data-ttu-id="3639a-261">Když jste hotovi, schéma kódu by měla vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="3639a-261">When you're done, your schema code should look like this:</span></span>

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

### <a name="update-the-articles-code"></a><span data-ttu-id="3639a-262">Aktualizujte kód články</span><span class="sxs-lookup"><span data-stu-id="3639a-262">Update the articles code</span></span>

<span data-ttu-id="3639a-263">Aktualizovat zbytek vaší `articles` kódu pro použití `comment`.</span><span class="sxs-lookup"><span data-stu-id="3639a-263">Update the rest of your `articles` code to use `comment`.</span></span>

<span data-ttu-id="3639a-264">Je pět souborů, budete muset upravit: řadičem serveru a zobrazení čtyři klientů.</span><span class="sxs-lookup"><span data-stu-id="3639a-264">There are five files you need to modify: the server controller and the four client views.</span></span> 

<span data-ttu-id="3639a-265">Otevřete _modules/articles/server/controllers/articles.server.controller.js_.</span><span class="sxs-lookup"><span data-stu-id="3639a-265">Open _modules/articles/server/controllers/articles.server.controller.js_.</span></span>

<span data-ttu-id="3639a-266">V `update` fungovat, přidejte přiřazení pro `article.comment`.</span><span class="sxs-lookup"><span data-stu-id="3639a-266">In the `update` function, add an assignment for `article.comment`.</span></span> <span data-ttu-id="3639a-267">Následující kód ukazuje dokončené `update` funkce:</span><span class="sxs-lookup"><span data-stu-id="3639a-267">The following code shows the completed `update` function:</span></span>

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

<span data-ttu-id="3639a-268">Otevřete _modules/articles/client/views/view-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="3639a-268">Open _modules/articles/client/views/view-article.client.view.html_.</span></span>

<span data-ttu-id="3639a-269">Nad uzavírací `</section>` značky, přidejte následující řádek zobrazíte `comment` spolu s ostatními data článku:</span><span class="sxs-lookup"><span data-stu-id="3639a-269">Just above the closing `</section>` tag, add the following line to display `comment` along with the rest of the article data:</span></span>

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

<span data-ttu-id="3639a-270">Otevřete _modules/articles/client/views/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="3639a-270">Open _modules/articles/client/views/list-articles.client.view.html_.</span></span>

<span data-ttu-id="3639a-271">Nad uzavírací `</a>` značky, přidejte následující řádek zobrazíte `comment` spolu s ostatními data článku:</span><span class="sxs-lookup"><span data-stu-id="3639a-271">Just above the closing `</a>` tag, add the following line to display `comment` along with the rest of the article data:</span></span>

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

<span data-ttu-id="3639a-272">Otevřete _modules/articles/client/views/admin/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="3639a-272">Open _modules/articles/client/views/admin/list-articles.client.view.html_.</span></span>

<span data-ttu-id="3639a-273">Uvnitř `<div class="list-group">` elementu a nad uzavírací `</a>` značky, přidejte následující řádek zobrazíte `comment` spolu s ostatními data článku:</span><span class="sxs-lookup"><span data-stu-id="3639a-273">Inside the `<div class="list-group">` element and just above the closing `</a>` tag, add the following line to display `comment` along with the rest of the article data:</span></span>

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

<span data-ttu-id="3639a-274">Otevřete _modules/articles/client/views/admin/form-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="3639a-274">Open _modules/articles/client/views/admin/form-article.client.view.html_.</span></span>

<span data-ttu-id="3639a-275">Najít `<div class="form-group">` elementu, který obsahuje tlačítko pro odeslání, který vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="3639a-275">Find the `<div class="form-group">` element that contains the submit button, which looks like this:</span></span>

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

<span data-ttu-id="3639a-276">Nad tuto značku, přidejte další `<div class="form-group">` element, který umožňuje uživatelům upravit `comment` pole.</span><span class="sxs-lookup"><span data-stu-id="3639a-276">Just above this tag, add another `<div class="form-group">` element that lets people edit the `comment` field.</span></span> <span data-ttu-id="3639a-277">Nového elementu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="3639a-277">Your new element should look like this:</span></span>

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="3639a-278">Otestujte provedené změny místně</span><span class="sxs-lookup"><span data-stu-id="3639a-278">Test your changes locally</span></span>

<span data-ttu-id="3639a-279">Uložte všechny provedené změny.</span><span class="sxs-lookup"><span data-stu-id="3639a-279">Save all your changes.</span></span>

<span data-ttu-id="3639a-280">Vyzkoušejte změny v produkčním režimu znovu.</span><span class="sxs-lookup"><span data-stu-id="3639a-280">Test your changes in production mode again.</span></span>

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> <span data-ttu-id="3639a-281">Nezapomeňte, že vaše _config/env/production.js_ obnovila a `MONGODB_URI` – proměnná prostředí je nastavit pouze v Azure web app a ne na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="3639a-281">Remember that your _config/env/production.js_ has been reverted, and the `MONGODB_URI` environment variable is only set in your Azure web app and not on your local machine.</span></span> <span data-ttu-id="3639a-282">Pokud se podíváte na konfigurační soubor, zjistíte, že konfigurace produkční výchozí použít místní databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3639a-282">If you look at the config file, you find that the production configuration defaults to use a local MongoDB database.</span></span> <span data-ttu-id="3639a-283">Tím je zajištěno, že nemáte touch provozních dat při testování změn kódu místně.</span><span class="sxs-lookup"><span data-stu-id="3639a-283">This makes sure that you don't touch production data when you test your code changes locally.</span></span>

<span data-ttu-id="3639a-284">Přejděte na `http://localhost:8443` v prohlížeči a ujistěte se, že jste přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3639a-284">Navigate to `http://localhost:8443` in a browser and make sure that you're signed in.</span></span>

<span data-ttu-id="3639a-285">Vyberte **správce > Správa článků**, pak výběrem přidat článek  **+**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3639a-285">Select **Admin > Manage Articles**, then add an article by selecting the **+** button.</span></span>

<span data-ttu-id="3639a-286">Zobrazí nové `Comment` nyní textové pole.</span><span class="sxs-lookup"><span data-stu-id="3639a-286">You see the new `Comment` textbox now.</span></span>

![Přidání komentářů pole článků](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

<span data-ttu-id="3639a-288">V terminálu, zastavte Node.js zadáním `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="3639a-288">In the terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

### <a name="publish-changes-to-azure"></a><span data-ttu-id="3639a-289">Publikování změn do Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-289">Publish changes to Azure</span></span>

<span data-ttu-id="3639a-290">Potvrdit změny v úložišti Git a potom odešlete změny kódu do Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-290">Commit your changes in Git, then push the code changes to Azure.</span></span>

```bash
git commit -am "added article comment"
git push azure master
```

<span data-ttu-id="3639a-291">Jednou `git push` je dokončení, přejděte do vaší webové aplikace Azure a vyzkoušet nové funkce.</span><span class="sxs-lookup"><span data-stu-id="3639a-291">Once the `git push` is complete, navigate to your Azure web app and try out the new functionality.</span></span>

![Model a databáze změny, které jsou publikovány do služby Azure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

<span data-ttu-id="3639a-293">Pokud jste dříve přidali všechny články, je stále můžete vidíte.</span><span class="sxs-lookup"><span data-stu-id="3639a-293">If you added any articles earlier, you still can see them.</span></span> <span data-ttu-id="3639a-294">Stávající data v databázi vaší Cosmos není ztraceny.</span><span class="sxs-lookup"><span data-stu-id="3639a-294">Existing data in your Cosmos DB is not lost.</span></span> <span data-ttu-id="3639a-295">Navíc vaše aktualizace schématu dat a svoje existující data zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="3639a-295">Also, your updates to the data schema and leaves your existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="3639a-296">Diagnostické protokoly datového proudu</span><span class="sxs-lookup"><span data-stu-id="3639a-296">Stream diagnostic logs</span></span> 

<span data-ttu-id="3639a-297">Při spuštění vaší aplikace Node.js ve službě Azure App Service můžete získat protokoly konzoly přesměruje do terminálu.</span><span class="sxs-lookup"><span data-stu-id="3639a-297">While your Node.js application runs in Azure App Service, you can get the console logs piped to your terminal.</span></span> <span data-ttu-id="3639a-298">Tímto způsobem můžete získat stejné diagnostické zprávy pomoci při ladění chyb aplikace.</span><span class="sxs-lookup"><span data-stu-id="3639a-298">That way, you can get the same diagnostic messages to help you debug application errors.</span></span>

<span data-ttu-id="3639a-299">Spusťte protokolu streamování pomocí [az webapp protokolu poškozené databáze za](/cli/azure/webapp/log#tail) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3639a-299">To start log streaming, use the [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

<span data-ttu-id="3639a-300">Po zahájení vysílání datového proudu protokolu, aktualizujte Azure webové aplikace v prohlížeči získat některé webový provoz.</span><span class="sxs-lookup"><span data-stu-id="3639a-300">Once log streaming has started, refresh your Azure web app in the browser to get some web traffic.</span></span> <span data-ttu-id="3639a-301">Zobrazí protokoly konzoly přesměruje do terminálu.</span><span class="sxs-lookup"><span data-stu-id="3639a-301">You now see console logs piped to your terminal.</span></span>

<span data-ttu-id="3639a-302">Zastavení protokolu streamování kdykoli zadáním `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="3639a-302">Stop log streaming at any time by typing `Ctrl+C`.</span></span> 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="3639a-303">Správa Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3639a-303">Manage your Azure web app</span></span>

<span data-ttu-id="3639a-304">Přejděte na [portál Azure](https://portal.azure.com) zobrazíte webové aplikace, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3639a-304">Go to the [Azure portal](https://portal.azure.com) to see the web app you created.</span></span>

<span data-ttu-id="3639a-305">V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="3639a-305">From the left menu, click **App Services**, then click the name of your Azure web app.</span></span>

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

<span data-ttu-id="3639a-307">Ve výchozím nastavení, zobrazí na portálu vaší webové aplikace **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="3639a-307">By default, the portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="3639a-308">Tato stránka poskytuje přehled, jak si vaše aplikace stojí.</span><span class="sxs-lookup"><span data-stu-id="3639a-308">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="3639a-309">Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="3639a-309">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="3639a-310">Karty na levé straně stránky zobrazí stránek jinou konfiguraci, že můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="3639a-310">The tabs on the left side of the page show the different configuration pages you can open.</span></span>

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a><span data-ttu-id="3639a-312">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3639a-312">Next steps</span></span>

<span data-ttu-id="3639a-313">Co jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="3639a-313">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3639a-314">Vytvořit databázi MongoDB v Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-314">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="3639a-315">Připojení aplikace Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="3639a-315">Connect a Node.js app to MongoDB</span></span>
> * <span data-ttu-id="3639a-316">Nasazení aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-316">Deploy the app to Azure</span></span>
> * <span data-ttu-id="3639a-317">Aktualizovat datový model a aplikaci znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="3639a-317">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="3639a-318">Datový proud protokolů z Azure terminálu</span><span class="sxs-lookup"><span data-stu-id="3639a-318">Stream logs from Azure to your terminal</span></span>
> * <span data-ttu-id="3639a-319">Spravovat aplikaci na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3639a-319">Manage the app in the Azure portal</span></span>

<span data-ttu-id="3639a-320">Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3639a-320">Advance to the next tutorial to learn how to map a custom DNS name to your web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="3639a-321">Mapování existujícího vlastního názvu DNS na Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="3639a-321">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
