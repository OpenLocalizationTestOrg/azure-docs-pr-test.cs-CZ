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
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a><span data-ttu-id="3c45b-103">Vytvoření webové aplikace Node.js a MongoDB v Azure</span><span class="sxs-lookup"><span data-stu-id="3c45b-103">Build a Node.js and MongoDB web app in Azure</span></span>

<span data-ttu-id="3c45b-104">Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby.</span><span class="sxs-lookup"><span data-stu-id="3c45b-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="3c45b-105">Tento kurz ukazuje, jak toocreate Node.js webové aplikace v Azure a připojte ho tooa databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-105">This tutorial shows how toocreate a Node.js web app in Azure and connect it tooa MongoDB database.</span></span> <span data-ttu-id="3c45b-106">Když jste hotovi, budete mít střední aplikace (MongoDB, Express, AngularJS a Node.js) spuštěná v [Azure App Service](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3c45b-106">When you're done, you'll have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running in [Azure App Service](app-service-web-overview.md).</span></span> <span data-ttu-id="3c45b-107">Pro jednoduchost, hello ukázková aplikace používá hello [MEAN.js webová architektura](http://meanjs.org/).</span><span class="sxs-lookup"><span data-stu-id="3c45b-107">For simplicity, hello sample application uses hello [MEAN.js web framework](http://meanjs.org/).</span></span>

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="3c45b-109">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="3c45b-109">What you'll learn:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c45b-110">Vytvořit databázi MongoDB v Azure</span><span class="sxs-lookup"><span data-stu-id="3c45b-110">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="3c45b-111">Připojit tooMongoDB aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="3c45b-111">Connect a Node.js app tooMongoDB</span></span>
> * <span data-ttu-id="3c45b-112">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="3c45b-112">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="3c45b-113">Aktualizovat hello datový model a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3c45b-113">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="3c45b-114">Diagnostické protokoly datového proudu z Azure</span><span class="sxs-lookup"><span data-stu-id="3c45b-114">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="3c45b-115">Spravovat aplikace hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3c45b-115">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c45b-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c45b-116">Prerequisites</span></span>

<span data-ttu-id="3c45b-117">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="3c45b-117">toocomplete this tutorial:</span></span>

1. <span data-ttu-id="3c45b-118">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="3c45b-118">[Install Git](https://git-scm.com/)</span></span>
1. <span data-ttu-id="3c45b-119">[Nainstalovat Node.js a NPM](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="3c45b-119">[Install Node.js and NPM](https://nodejs.org/)</span></span>
1. <span data-ttu-id="3c45b-120">[Nainstalujte Gulp.js](http://gulpjs.com/) (požadavku [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span><span class="sxs-lookup"><span data-stu-id="3c45b-120">[Install Gulp.js](http://gulpjs.com/) (required by [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))</span></span>
1. [<span data-ttu-id="3c45b-121">Nainstalujte a spusťte MongoDB Community Edition</span><span class="sxs-lookup"><span data-stu-id="3c45b-121">Install and run MongoDB Community Edition</span></span>](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3c45b-122">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3c45b-122">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="3c45b-123">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="3c45b-123">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3c45b-124">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3c45b-124">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-mongodb"></a><span data-ttu-id="3c45b-125">Test místní MongoDB</span><span class="sxs-lookup"><span data-stu-id="3c45b-125">Test local MongoDB</span></span>

<span data-ttu-id="3c45b-126">Hello otevřete okno terminálu a `cd` toohello `bin` adresář instalace MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-126">Open hello terminal window and `cd` toohello `bin` directory of your MongoDB installation.</span></span> <span data-ttu-id="3c45b-127">Všechny příkazy hello toto okno terminálu toorun můžete použít v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-127">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

<span data-ttu-id="3c45b-128">Spustit `mongo` v hello terminálu tooconnect tooyour místního serveru MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-128">Run `mongo` in hello terminal tooconnect tooyour local MongoDB server.</span></span>

```bash
mongo
```

<span data-ttu-id="3c45b-129">Pokud připojení úspěšné, pak databázi MongoDB je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="3c45b-129">If your connection is successful, then your MongoDB database is already running.</span></span> <span data-ttu-id="3c45b-130">Pokud ne, ujistěte se, zda je spuštěná místní databázi MongoDB pomocí následujících kroků hello v [nainstalujte MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span><span class="sxs-lookup"><span data-stu-id="3c45b-130">If not, make sure that your local MongoDB database is started by following hello steps at [Install MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/).</span></span> <span data-ttu-id="3c45b-131">Často je nainstalovaná MongoDB, ale stále potřebujete toostart ho spuštěním `mongod`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-131">Often, MongoDB is installed, but you still need toostart it by running `mongod`.</span></span> 

<span data-ttu-id="3c45b-132">Po dokončení testování vaší databázi MongoDB, zadejte `Ctrl+C` v terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-132">When you're done testing your MongoDB database, type `Ctrl+C` in hello terminal.</span></span> 

## <a name="create-local-nodejs-app"></a><span data-ttu-id="3c45b-133">Vytvořit místní aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="3c45b-133">Create local Node.js app</span></span>

<span data-ttu-id="3c45b-134">V tomto kroku nastavíte místní projekt Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-134">In this step, you set up hello local Node.js project.</span></span>

### <a name="clone-hello-sample-application"></a><span data-ttu-id="3c45b-135">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="3c45b-135">Clone hello sample application</span></span>

<span data-ttu-id="3c45b-136">V okně terminálu hello `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="3c45b-136">In hello terminal window, `cd` tooa working directory.</span></span>  

<span data-ttu-id="3c45b-137">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-137">Run hello following command tooclone hello sample repository.</span></span> 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

<span data-ttu-id="3c45b-138">Tato ukázka úložiště obsahuje kopii hello [MEAN.js úložiště](https://github.com/meanjs/mean).</span><span class="sxs-lookup"><span data-stu-id="3c45b-138">This sample repository contains a copy of hello [MEAN.js repository](https://github.com/meanjs/mean).</span></span> <span data-ttu-id="3c45b-139">Je upravený toorun v App Service (Další informace najdete v tématu hello MEAN.js úložiště [souboru README](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span><span class="sxs-lookup"><span data-stu-id="3c45b-139">It is modified toorun on App Service (for more information, see hello MEAN.js repository [README file](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).</span></span>

### <a name="run-hello-application"></a><span data-ttu-id="3c45b-140">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3c45b-140">Run hello application</span></span>

<span data-ttu-id="3c45b-141">Spusťte následující příkazy tooinstall hello požadované balíčky hello a spustit aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-141">Run hello following commands tooinstall hello required packages and start hello application.</span></span>

```bash
cd meanjs
npm install
npm start
```

<span data-ttu-id="3c45b-142">Když je aplikace hello úplným načtením, uvidíte něco podobné toohello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="3c45b-142">When hello app is fully loaded, you see something similar toohello following message:</span></span>

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

<span data-ttu-id="3c45b-143">Přejděte toohttp://localhost:3000 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3c45b-143">Navigate toohttp://localhost:3000 in a browser.</span></span> <span data-ttu-id="3c45b-144">Klikněte na tlačítko **zaregistrovat** v hello horní nabídce a vytvoření zkušebního uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c45b-144">Click **Sign Up** in hello top menu and create a test user.</span></span> 

<span data-ttu-id="3c45b-145">Hello MEAN.js ukázkové aplikace ukládá data uživatele v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-145">hello MEAN.js sample application stores user data in hello database.</span></span> <span data-ttu-id="3c45b-146">Pokud jste při vytváření uživatele a přihlášení úspěšné, je vaše aplikace zápis dat toohello místní databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-146">If you are successful at creating a user and signing in, then your app is writing data toohello local MongoDB database.</span></span>

![MEAN.js připojí úspěšně tooMongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

<span data-ttu-id="3c45b-148">Vyberte **správce > Správa článků** tooadd některé články.</span><span class="sxs-lookup"><span data-stu-id="3c45b-148">Select **Admin > Manage Articles** tooadd some articles.</span></span>

<span data-ttu-id="3c45b-149">stiskněte klávesu Node.js kdykoli toostop `Ctrl+C` v terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-149">toostop Node.js at any time, press `Ctrl+C` in hello terminal.</span></span> 

## <a name="create-production-mongodb"></a><span data-ttu-id="3c45b-150">Vytvořit produkční MongoDB</span><span class="sxs-lookup"><span data-stu-id="3c45b-150">Create production MongoDB</span></span>

<span data-ttu-id="3c45b-151">V tomto kroku vytvoříte databázi MongoDB v Azure.</span><span class="sxs-lookup"><span data-stu-id="3c45b-151">In this step, you create a MongoDB database in Azure.</span></span> <span data-ttu-id="3c45b-152">Pokud je vaše aplikace nasazené tooAzure, používá tato databáze cloudu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-152">When your app is deployed tooAzure, it uses this cloud database.</span></span>

<span data-ttu-id="3c45b-153">Pro MongoDB, tento kurz používá [Azure Cosmos DB](/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="3c45b-153">For MongoDB, this tutorial uses [Azure Cosmos DB](/azure/documentdb/).</span></span> <span data-ttu-id="3c45b-154">Cosmos DB podporuje připojení klienta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-154">Cosmos DB supports MongoDB client connections.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="3c45b-155">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="3c45b-155">Log in tooAzure</span></span>

<span data-ttu-id="3c45b-156">Hello Azure CLI 2.0 toocreate hello prostředky potřebné toohost budete používat aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="3c45b-156">You'll use hello Azure CLI 2.0 toocreate hello resources needed toohost your app in Azure.</span></span> <span data-ttu-id="3c45b-157">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="3c45b-157">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="3c45b-158">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3c45b-158">Create a resource group</span></span>

<span data-ttu-id="3c45b-159">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c45b-159">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="3c45b-160">Hello následující příklad vytvoří skupinu prostředků v oblasti západní Evropa hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-160">hello following example creates a resource group in hello West Europe region.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

<span data-ttu-id="3c45b-161">Použití hello [míst seznamu služby App Service az](/cli/azure/appservice#list-locations) rozhraní příkazového řádku Azure příkaz toolist dostupná umístění.</span><span class="sxs-lookup"><span data-stu-id="3c45b-161">Use hello [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command toolist available locations.</span></span> 

### <a name="create-a-cosmos-db-account"></a><span data-ttu-id="3c45b-162">Vytvoření účtu Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3c45b-162">Create a Cosmos DB account</span></span>

<span data-ttu-id="3c45b-163">Vytvořte účet Cosmos DB s hello [vytvořit az cosmosdb](/cli/azure/cosmosdb#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c45b-163">Create a Cosmos DB account with hello [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="3c45b-164">Následující příkaz a nahraďte v hello jedinečný název databáze Cosmos hello  *\<cosmosdb_name >* zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="3c45b-164">In hello following command, substitute a unique Cosmos DB name for hello *\<cosmosdb_name>* placeholder.</span></span> <span data-ttu-id="3c45b-165">Tento název se používá jako součást hello hello Cosmos DB koncového bodu, `https://<cosmosdb_name>.documents.azure.com/`, takže název hello musí toobe jedinečný mezi všechny Cosmos DB účty v Azure.</span><span class="sxs-lookup"><span data-stu-id="3c45b-165">This name is used as hello part of hello Cosmos DB endpoint, `https://<cosmosdb_name>.documents.azure.com/`, so hello name needs toobe unique across all Cosmos DB accounts in Azure.</span></span> <span data-ttu-id="3c45b-166">Hello název musí obsahovat jenom malá písmena, číslice a znak hello pomlčku (-) a musí být v rozmezí 3 až 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="3c45b-166">hello name must contain only lowercase letters, numbers, and hello hyphen (-) character, and must be between 3 and 50 characters long.</span></span>

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

<span data-ttu-id="3c45b-167">Hello *– druhu MongoDB* parametr povolí připojení klientů MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-167">hello *--kind MongoDB* parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="3c45b-168">Při vytvoření hello Cosmos DB účet je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3c45b-168">When hello Cosmos DB account is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

## <a name="connect-app-tooproduction-mongodb"></a><span data-ttu-id="3c45b-169">Připojení aplikace tooproduction MongoDB</span><span class="sxs-lookup"><span data-stu-id="3c45b-169">Connect app tooproduction MongoDB</span></span>

<span data-ttu-id="3c45b-170">V tomto kroku připojíte MEAN.js ukázkové aplikace toohello Cosmos DB databázi, kterou jste právě vytvořili, pomocí připojovacího řetězce MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-170">In this step, you connect your MEAN.js sample application toohello Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

### <a name="retrieve-hello-database-key"></a><span data-ttu-id="3c45b-171">Načíst klíč databáze hello</span><span class="sxs-lookup"><span data-stu-id="3c45b-171">Retrieve hello database key</span></span>

<span data-ttu-id="3c45b-172">tooconnect toohello Cosmos DB databáze, je nutné klíč databáze hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-172">tooconnect toohello Cosmos DB database, you need hello database key.</span></span> <span data-ttu-id="3c45b-173">Použití hello [az cosmosdb seznamu klíčů](/cli/azure/cosmosdb#list-keys) příkaz tooretrieve hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="3c45b-173">Use hello [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command tooretrieve hello primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

<span data-ttu-id="3c45b-174">Hello rozhraní příkazového řádku Azure ukazuje následující příklad podobné toohello informace:</span><span class="sxs-lookup"><span data-stu-id="3c45b-174">hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Zkopírujte hodnotu hello `primaryMasterKey`. <span data-ttu-id="3c45b-176">Je třeba tyto informace v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-176">You need this information in hello next step.</span></span>

<a name="devconfig"></a>
### <a name="configure-hello-connection-string-in-your-nodejs-application"></a><span data-ttu-id="3c45b-177">Konfigurace v aplikaci Node.js hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3c45b-177">Configure hello connection string in your Node.js application</span></span>

<span data-ttu-id="3c45b-178">Ve svém úložišti MEAN.js otevřete _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-178">In your MEAN.js repository, open _config/env/production.js_.</span></span>

<span data-ttu-id="3c45b-179">V hello `db` objektu, aktualizujte hodnotu hello `uri`:</span><span class="sxs-lookup"><span data-stu-id="3c45b-179">In hello `db` object, update hello value of `uri`:</span></span>

* <span data-ttu-id="3c45b-180">Nahraďte hello dva  *\<cosmosdb_name >* zástupných symbolů nahraďte názvem databáze Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-180">Replace hello two *\<cosmosdb_name>* placeholders with your Cosmos DB database name.</span></span>
* <span data-ttu-id="3c45b-181">Nahraďte hello  *\<primary_master_key >* zástupný text klíčem hello jste zkopírovali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-181">Replace hello *\<primary_master_key>* placeholder with hello key you copied in hello previous step.</span></span>

<span data-ttu-id="3c45b-182">Hello následující kód ukazuje hello `db` objektu:</span><span class="sxs-lookup"><span data-stu-id="3c45b-182">hello following code shows hello `db` object:</span></span>

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

<span data-ttu-id="3c45b-183">Hello `ssl=true` možnost je povinná, protože [Cosmos DB vyžaduje SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="3c45b-183">hello `ssl=true` option is required because [Cosmos DB requires SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 

<span data-ttu-id="3c45b-184">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="3c45b-184">Save your changes.</span></span>

### <a name="test-hello-application-in-production-mode"></a><span data-ttu-id="3c45b-185">Testovací aplikace hello v produkčním režimu</span><span class="sxs-lookup"><span data-stu-id="3c45b-185">Test hello application in production mode</span></span> 

<span data-ttu-id="3c45b-186">Spusťte následující příkaz toominify a sady skriptů pro produkční prostředí hello hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-186">Run hello following command toominify and bundle scripts for hello production environment.</span></span> <span data-ttu-id="3c45b-187">Tento proces generuje soubory hello vyžaduje hello produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c45b-187">This process generates hello files needed by hello production environment.</span></span>

```bash
gulp prod
```

<span data-ttu-id="3c45b-188">Spuštění hello následující příkaz toouse hello připojovací řetězec jste nakonfigurovali v _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-188">Run hello following command toouse hello connection string you configured in _config/env/production.js_.</span></span>

```bash
NODE_ENV=production node server.js
```

<span data-ttu-id="3c45b-189">`NODE_ENV=production`Nastaví proměnné prostředí hello, která říká službě Node.js toorun hello produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c45b-189">`NODE_ENV=production` sets hello environment variable that tells Node.js toorun in hello production environment.</span></span>  <span data-ttu-id="3c45b-190">`node server.js`Spustí hello Node.js server s `server.js` v kořenovém úložišti.</span><span class="sxs-lookup"><span data-stu-id="3c45b-190">`node server.js` starts hello Node.js server with `server.js` in your repository root.</span></span> <span data-ttu-id="3c45b-191">Toto je, jak aplikace Node.js je načten do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="3c45b-191">This is how your Node.js application is loaded in Azure.</span></span> 

<span data-ttu-id="3c45b-192">Pokud aplikace hello je načtena, zkontrolujte toomake se, že je spuštěna v provozním prostředí hello:</span><span class="sxs-lookup"><span data-stu-id="3c45b-192">When hello app is loaded, check toomake sure that it's running in hello production environment:</span></span>

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

<span data-ttu-id="3c45b-193">Přejděte toohttp://localhost:8443 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3c45b-193">Navigate toohttp://localhost:8443 in a browser.</span></span> <span data-ttu-id="3c45b-194">Klikněte na tlačítko **zaregistrovat** v hello horní nabídce a vytvoření zkušebního uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c45b-194">Click **Sign Up** in hello top menu and create a test user.</span></span> <span data-ttu-id="3c45b-195">Pokud jste vytváření uživatele a přihlášení úspěšné, pak aplikace zapisuje data toohello Cosmos DB databáze v Azure.</span><span class="sxs-lookup"><span data-stu-id="3c45b-195">If you are successful creating a user and signing in, then your app is writing data toohello Cosmos DB database in Azure.</span></span> 

<span data-ttu-id="3c45b-196">V terminálu hello, zastavte Node.js zadáním `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-196">In hello terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

## <a name="deploy-app-tooazure"></a><span data-ttu-id="3c45b-197">Nasazení aplikace tooAzure</span><span class="sxs-lookup"><span data-stu-id="3c45b-197">Deploy app tooAzure</span></span>

<span data-ttu-id="3c45b-198">V tomto kroku nasadíte tooAzure vaše připojení MongoDB Node.js aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3c45b-198">In this step, you deploy your MongoDB-connected Node.js application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="3c45b-199">Vytvoření plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="3c45b-199">Create an App Service plan</span></span>

<span data-ttu-id="3c45b-200">Vytvořte plán služby App Service s hello [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c45b-200">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="3c45b-201">Hello následující příklad vytvoří plán služby App Service s názvem _myAppServicePlan_ pomocí hello **volné** cenové úrovně:</span><span class="sxs-lookup"><span data-stu-id="3c45b-201">hello following example creates an App Service plan named _myAppServicePlan_ using hello **FREE** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="3c45b-202">Při vytvoření hello plán služby App Service je hello rozhraní příkazového řádku Azure obsahuje informace o podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3c45b-202">When hello App Service plan is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="create-a-web-app"></a><span data-ttu-id="3c45b-203">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3c45b-203">Create a web app</span></span>

<span data-ttu-id="3c45b-204">Vytvoření webové aplikace v hello `myAppServicePlan` plán služby App Service se hello [az webapp vytvořit](/cli/azure/webapp#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c45b-204">Create a web app in hello `myAppServicePlan` App Service plan with hello [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="3c45b-205">Hello webové aplikace poskytuje můžete hostování místo toodeploy kódu a poskytuje adresu URL pro vás tooview hello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c45b-205">hello web app gives you a hosting space toodeploy your code and provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="3c45b-206">Použití toocreate hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c45b-206">Use  toocreate hello web app.</span></span> 

<span data-ttu-id="3c45b-207">Následující příkaz a nahraďte v hello hello  *\<app_name >* zástupný symbol s jedinečným názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c45b-207">In hello following command, replace hello *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="3c45b-208">Tento název se používá jako součást hello hello výchozí adresa URL pro webovou aplikaci hello, takže název hello musí toobe jedinečný mezi všechny aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3c45b-208">This name is used as hello part of hello default URL for hello web app, so hello name needs toobe unique across all apps in Azure App Service.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="3c45b-209">Po vytvoření webové aplikace hello hello rozhraní příkazového řádku Azure zobrazuje informace podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3c45b-209">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-an-environment-variable"></a><span data-ttu-id="3c45b-210">Nakonfigurujte proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="3c45b-210">Configure an environment variable</span></span>

<span data-ttu-id="3c45b-211">Výše v hello kurz, je pevně zakódované hello připojovací řetězec databáze v _config/env/production.js_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-211">Earlier in hello tutorial, you hardcoded hello database connection string in _config/env/production.js_.</span></span> <span data-ttu-id="3c45b-212">V souladu s nejlepším způsobem zabezpečení chcete tookeep tyto důvěrné osobní údaje z úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="3c45b-212">In keeping with security best practice, you want tookeep this sensitive data out of your Git repository.</span></span> <span data-ttu-id="3c45b-213">Pro vaši aplikaci běžící v Azure budete používat proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c45b-213">For your app running in Azure, you'll use an environment variable instead.</span></span>

<span data-ttu-id="3c45b-214">Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí hello [aktualizovat az webapp konfigurace appsettings](/cli/azure/webapp/config/appsettings#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c45b-214">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings update](/cli/azure/webapp/config/appsettings#update) command.</span></span> 

<span data-ttu-id="3c45b-215">Hello následující příklad konfiguruje `MONGODB_URI` nastavení aplikace v Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c45b-215">hello following example configures a `MONGODB_URI` app setting in your Azure web app.</span></span> <span data-ttu-id="3c45b-216">Nahraďte hello  *\<app_name >*,  *\<cosmosdb_name >*, a  *\<primary_master_key >* zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="3c45b-216">Replace hello *\<app_name>*, *\<cosmosdb_name>*, and *\<primary_master_key>* placeholders.</span></span>

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

<span data-ttu-id="3c45b-217">V kódu Node.js, je přístup k nastavení této aplikace s `process.env.MONGODB_URI`, stejně, jako by přístup všechny proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c45b-217">In Node.js code, you access this app setting with `process.env.MONGODB_URI`, just like you would access any environment variable.</span></span> 

<span data-ttu-id="3c45b-218">Teď vrátit zpět too_config/env/production.js_ vaše změny se hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3c45b-218">Now, undo your changes too_config/env/production.js_ with hello following command:</span></span>

```bash
git checkout -- .
```

<span data-ttu-id="3c45b-219">Otevřete _config/env/production.js_ znovu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-219">Open _config/env/production.js_ again.</span></span> <span data-ttu-id="3c45b-220">Všimněte si, že hello výchozí MEAN.js aplikace je již nakonfigurované toouse hello `MONGODB_URI` proměnné prostředí, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3c45b-220">Note that hello default MEAN.js app is already configured toouse hello `MONGODB_URI` environment variable that you created.</span></span>

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a><span data-ttu-id="3c45b-221">Konfigurace nasazení místního gitu</span><span class="sxs-lookup"><span data-stu-id="3c45b-221">Configure local git deployment</span></span> 

<span data-ttu-id="3c45b-222">Použití hello [nastavený uživatel nasazení webapp az](/cli/azure/webapp/deployment/user#set) příkaz toocreate přihlašovací údaje pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="3c45b-222">Use hello [az webapp deployment user set](/cli/azure/webapp/deployment/user#set) command toocreate credentials for deployment.</span></span>

<span data-ttu-id="3c45b-223">Můžete nasadit aplikace tooAzure služby App Service různými způsoby, včetně FTP, Git místní, GitHub, Visual Studio Team Services a BitBucket.</span><span class="sxs-lookup"><span data-stu-id="3c45b-223">You can deploy your application tooAzure App Service in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="3c45b-224">Pro místní Git a FTP, je nutné toohave uživatele nasazení nakonfigurovali na serveru tooauthenticate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="3c45b-224">For FTP and local Git, it is necessary toohave a deployment user configured on hello server tooauthenticate your deployment.</span></span> <span data-ttu-id="3c45b-225">Tento uživatel nasazení je úrovni účtu a se liší od účtu předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="3c45b-225">This deployment user is account-level and is different from your Azure subscription account.</span></span> <span data-ttu-id="3c45b-226">Potřebujete jenom tooconfigure tohoto uživatele nasazení jednou.</span><span class="sxs-lookup"><span data-stu-id="3c45b-226">You only need tooconfigure this deployment user once.</span></span>

<span data-ttu-id="3c45b-227">Následující příkaz a nahraďte v hello  *\<uživatelské jméno >* a  *\<heslo >* s nové uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="3c45b-227">In hello following command, replace *\<user-name>* and *\<password>* with a new user name and password.</span></span> <span data-ttu-id="3c45b-228">Hello uživatelské jméno musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="3c45b-228">hello user name must be unique.</span></span> <span data-ttu-id="3c45b-229">Hello heslo musí obsahovat alespoň osm znaků, se dvěma hello následující tři prvky: písmena, symboly a čísla.</span><span class="sxs-lookup"><span data-stu-id="3c45b-229">hello password must be at least eight characters long, with two of hello following three elements:  letters, numbers, symbols.</span></span> <span data-ttu-id="3c45b-230">Pokud dojde ` 'Conflict'. Details: 409` chyby, uživatelské jméno změnit hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-230">If you get a ` 'Conflict'. Details: 409` error, change hello username.</span></span> <span data-ttu-id="3c45b-231">Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.</span><span class="sxs-lookup"><span data-stu-id="3c45b-231">If you get a ` 'Bad Request'. Details: 400` error, use a stronger password.</span></span>

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

<span data-ttu-id="3c45b-232">Záznam hello uživatelské jméno a heslo pro použití v dalších krocích při nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-232">Record hello user name and password for use in later steps when you deploy hello app.</span></span>

<span data-ttu-id="3c45b-233">Použití hello [az webapp nasazení zdroj konfigurace místní git](/cli/azure/webapp/deployment/source#config-local-git) příkaz tooconfigure místní Git přístup toohello webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="3c45b-233">Use hello [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) command tooconfigure local Git access toohello Azure web app.</span></span> 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

<span data-ttu-id="3c45b-234">Pokud je nakonfigurován uživatel nasazení hello, hello rozhraní příkazového řádku Azure zobrazuje hello URL nasazení pro Azure webové aplikace v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="3c45b-234">When hello deployment user is configured, hello Azure CLI shows hello deployment URL for your Azure web app in hello following format:</span></span>

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

<span data-ttu-id="3c45b-235">Kopírovat výstup z terminálu hello hello se použije v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-235">Copy hello output from hello terminal, as it will be used in hello next step.</span></span> 

### <a name="push-tooazure-from-git"></a><span data-ttu-id="3c45b-236">Z Git push tooAzure</span><span class="sxs-lookup"><span data-stu-id="3c45b-236">Push tooAzure from Git</span></span>

<span data-ttu-id="3c45b-237">Přidejte místní úložiště Git Azure vzdálené tooyour.</span><span class="sxs-lookup"><span data-stu-id="3c45b-237">Add an Azure remote tooyour local Git repository.</span></span> 

```bash
git remote add azure <paste_copied_url_here> 
```

<span data-ttu-id="3c45b-238">Push toohello Azure vzdálené toodeploy aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="3c45b-238">Push toohello Azure remote toodeploy your Node.js application.</span></span> <span data-ttu-id="3c45b-239">Jste vyzváni k hello heslo, které jste zadali dříve v rámci vytváření hello hello nasazení uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c45b-239">You will be prompted for hello password you supplied earlier as part of hello creation of hello deployment user.</span></span> 

```bash
git push azure master
```

<span data-ttu-id="3c45b-240">Během nasazení Azure App Service komunikuje s Gitem jejím průběhu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-240">During deployment, Azure App Service communicates its progress with Git.</span></span>

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

<span data-ttu-id="3c45b-241">Můžete si všimnout, že proces nasazení hello spouští [Gulp](http://gulpjs.com/) po `npm install`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-241">You may notice that hello deployment process runs [Gulp](http://gulpjs.com/) after `npm install`.</span></span> <span data-ttu-id="3c45b-242">Služby App Service nespouští Gulp nebo Grunt úloh během nasazení, tak toto úložiště ukázka má dva další soubory v jeho kořenový adresář tooenable ho:</span><span class="sxs-lookup"><span data-stu-id="3c45b-242">App Service does not run Gulp or Grunt tasks during deployment, so this sample repository has two additional files in its root directory tooenable it:</span></span> 

- <span data-ttu-id="3c45b-243">_.Deployment_ – tento soubor informuje služby App Service toorun `bash deploy.sh` jako hello vlastní nasazení skriptu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-243">_.deployment_ - This file tells App Service toorun `bash deploy.sh` as hello custom deployment script.</span></span>
- <span data-ttu-id="3c45b-244">_Deploy.SH_ -hello vlastní nasazení skriptu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-244">_deploy.sh_ - hello custom deployment script.</span></span> <span data-ttu-id="3c45b-245">Při kontrole souboru hello je se zobrazí, že běží `gulp prod` po `npm install` a `bower install`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-245">If you review hello file, you will see that it runs `gulp prod` after `npm install` and `bower install`.</span></span> 

<span data-ttu-id="3c45b-246">Tento přístup tooadd můžete použít všechny krok tooyour nasazení na základě Git.</span><span class="sxs-lookup"><span data-stu-id="3c45b-246">You can use this approach tooadd any step tooyour Git-based deployment.</span></span> <span data-ttu-id="3c45b-247">Pokud restartujete Azure webové aplikace v libovolném bodě, služby App Service není spusťte znovu tyto úlohy automatizace.</span><span class="sxs-lookup"><span data-stu-id="3c45b-247">If you restart your Azure web app at any point, App Service doesn't rerun these automation tasks.</span></span>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="3c45b-248">Procházet toohello webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="3c45b-248">Browse toohello Azure web app</span></span> 

<span data-ttu-id="3c45b-249">Procházet toohello nasadit webovou aplikaci pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3c45b-249">Browse toohello deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
``` 

<span data-ttu-id="3c45b-250">Klikněte na tlačítko **zaregistrovat** v hello horní nabídce a vytvořte fiktivní uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c45b-250">Click **Sign Up** in hello top menu and create a dummy user.</span></span> 

<span data-ttu-id="3c45b-251">Pokud jste úspěšné a aplikace hello automaticky přihlásí toohello vytvoří uživatele a potom MEAN.js aplikace v Azure má databázi MongoDB (Cosmos DB) toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="3c45b-251">If you are successful and hello app automatically signs in toohello created user, then your MEAN.js app in Azure has connectivity toohello MongoDB (Cosmos DB) database.</span></span> 

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

<span data-ttu-id="3c45b-253">Vyberte **správce > Správa článků** tooadd některé články.</span><span class="sxs-lookup"><span data-stu-id="3c45b-253">Select **Admin > Manage Articles** tooadd some articles.</span></span> 

<span data-ttu-id="3c45b-254">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="3c45b-254">**Congratulations!**</span></span> <span data-ttu-id="3c45b-255">Používáte datové aplikace Node.js ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3c45b-255">You're running a data-driven Node.js app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="3c45b-256">Aktualizace datový model a znovu ho zaveďte</span><span class="sxs-lookup"><span data-stu-id="3c45b-256">Update data model and redeploy</span></span>

<span data-ttu-id="3c45b-257">V tomto kroku změnit hello `article` dat modelu a publikovat tooAzure vaše změny.</span><span class="sxs-lookup"><span data-stu-id="3c45b-257">In this step, you change hello `article` data model and publish your change tooAzure.</span></span>

### <a name="update-hello-data-model"></a><span data-ttu-id="3c45b-258">Aktualizace hello datový model</span><span class="sxs-lookup"><span data-stu-id="3c45b-258">Update hello data model</span></span>

<span data-ttu-id="3c45b-259">Otevřete _modules/articles/server/models/article.server.model.js_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-259">Open _modules/articles/server/models/article.server.model.js_.</span></span>

<span data-ttu-id="3c45b-260">V `ArticleSchema`, přidejte `String` typu s názvem `comment`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-260">In `ArticleSchema`, add a `String` type called `comment`.</span></span> <span data-ttu-id="3c45b-261">Když jste hotovi, schéma kódu by měla vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="3c45b-261">When you're done, your schema code should look like this:</span></span>

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

### <a name="update-hello-articles-code"></a><span data-ttu-id="3c45b-262">Aktualizujte kód články hello</span><span class="sxs-lookup"><span data-stu-id="3c45b-262">Update hello articles code</span></span>

<span data-ttu-id="3c45b-263">Aktualizovat zbytek hello vaše `articles` code toouse `comment`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-263">Update hello rest of your `articles` code toouse `comment`.</span></span>

<span data-ttu-id="3c45b-264">Je pět souborů, které budete potřebovat toomodify: hello serveru řadiče a zobrazení hello čtyři klientů.</span><span class="sxs-lookup"><span data-stu-id="3c45b-264">There are five files you need toomodify: hello server controller and hello four client views.</span></span> 

<span data-ttu-id="3c45b-265">Otevřete _modules/articles/server/controllers/articles.server.controller.js_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-265">Open _modules/articles/server/controllers/articles.server.controller.js_.</span></span>

<span data-ttu-id="3c45b-266">V hello `update` fungovat, přidejte přiřazení pro `article.comment`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-266">In hello `update` function, add an assignment for `article.comment`.</span></span> <span data-ttu-id="3c45b-267">Hello následující kód ukazuje hello Dokončit `update` funkce:</span><span class="sxs-lookup"><span data-stu-id="3c45b-267">hello following code shows hello completed `update` function:</span></span>

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

<span data-ttu-id="3c45b-268">Otevřete _modules/articles/client/views/view-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-268">Open _modules/articles/client/views/view-article.client.view.html_.</span></span>

<span data-ttu-id="3c45b-269">Nad hello ukončovací `</section>` značky, přidejte následující řádek toodisplay hello `comment` společně s hello zbytek hello článku dat:</span><span class="sxs-lookup"><span data-stu-id="3c45b-269">Just above hello closing `</section>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

<span data-ttu-id="3c45b-270">Otevřete _modules/articles/client/views/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-270">Open _modules/articles/client/views/list-articles.client.view.html_.</span></span>

<span data-ttu-id="3c45b-271">Nad hello ukončovací `</a>` značky, přidejte následující řádek toodisplay hello `comment` společně s hello zbytek hello článku dat:</span><span class="sxs-lookup"><span data-stu-id="3c45b-271">Just above hello closing `</a>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

<span data-ttu-id="3c45b-272">Otevřete _modules/articles/client/views/admin/list-articles.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-272">Open _modules/articles/client/views/admin/list-articles.client.view.html_.</span></span>

<span data-ttu-id="3c45b-273">Uvnitř hello `<div class="list-group">` elementu a nad hello ukončovací `</a>` značky, přidejte následující řádek toodisplay hello `comment` společně s hello zbytek hello článku dat:</span><span class="sxs-lookup"><span data-stu-id="3c45b-273">Inside hello `<div class="list-group">` element and just above hello closing `</a>` tag, add hello following line toodisplay `comment` along with hello rest of hello article data:</span></span>

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

<span data-ttu-id="3c45b-274">Otevřete _modules/articles/client/views/admin/form-article.client.view.html_.</span><span class="sxs-lookup"><span data-stu-id="3c45b-274">Open _modules/articles/client/views/admin/form-article.client.view.html_.</span></span>

<span data-ttu-id="3c45b-275">Najde hello `<div class="form-group">` elementu, který obsahuje tlačítko pro odeslání hello, jež vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="3c45b-275">Find hello `<div class="form-group">` element that contains hello submit button, which looks like this:</span></span>

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

<span data-ttu-id="3c45b-276">Nad tuto značku, přidejte další `<div class="form-group">` element, který umožňuje uživatelům upravit hello `comment` pole.</span><span class="sxs-lookup"><span data-stu-id="3c45b-276">Just above this tag, add another `<div class="form-group">` element that lets people edit hello `comment` field.</span></span> <span data-ttu-id="3c45b-277">Nového elementu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="3c45b-277">Your new element should look like this:</span></span>

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="3c45b-278">Otestujte provedené změny místně</span><span class="sxs-lookup"><span data-stu-id="3c45b-278">Test your changes locally</span></span>

<span data-ttu-id="3c45b-279">Uložte všechny provedené změny.</span><span class="sxs-lookup"><span data-stu-id="3c45b-279">Save all your changes.</span></span>

<span data-ttu-id="3c45b-280">Vyzkoušejte změny v produkčním režimu znovu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-280">Test your changes in production mode again.</span></span>

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> <span data-ttu-id="3c45b-281">Nezapomeňte, že vaše _config/env/production.js_ byl vrácen a hello `MONGODB_URI` proměnná prostředí je nastavit pouze v Azure web app a ne na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="3c45b-281">Remember that your _config/env/production.js_ has been reverted, and hello `MONGODB_URI` environment variable is only set in your Azure web app and not on your local machine.</span></span> <span data-ttu-id="3c45b-282">Pokud se podíváte na hello konfiguračního souboru, zjistíte, že hello produkční konfigurace výchozí toouse místní databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3c45b-282">If you look at hello config file, you find that hello production configuration defaults toouse a local MongoDB database.</span></span> <span data-ttu-id="3c45b-283">Tím je zajištěno, že nemáte touch provozních dat při testování změn kódu místně.</span><span class="sxs-lookup"><span data-stu-id="3c45b-283">This makes sure that you don't touch production data when you test your code changes locally.</span></span>

<span data-ttu-id="3c45b-284">Přejděte příliš`http://localhost:8443` v prohlížeči a ujistěte se, že jste přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3c45b-284">Navigate too`http://localhost:8443` in a browser and make sure that you're signed in.</span></span>

<span data-ttu-id="3c45b-285">Vyberte **správce > Správa článků**, pak výběrem hello přidat článek  **+**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c45b-285">Select **Admin > Manage Articles**, then add an article by selecting hello **+** button.</span></span>

<span data-ttu-id="3c45b-286">Zobrazí hello nové `Comment` nyní textové pole.</span><span class="sxs-lookup"><span data-stu-id="3c45b-286">You see hello new `Comment` textbox now.</span></span>

![Pole tooArticles přidané komentář](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

<span data-ttu-id="3c45b-288">V terminálu hello, zastavte Node.js zadáním `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-288">In hello terminal, stop Node.js by typing `Ctrl+C`.</span></span> 

### <a name="publish-changes-tooazure"></a><span data-ttu-id="3c45b-289">Publikování změn tooAzure</span><span class="sxs-lookup"><span data-stu-id="3c45b-289">Publish changes tooAzure</span></span>

<span data-ttu-id="3c45b-290">Potvrdit změny v úložišti Git a potom push tooAzure změny kódu hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-290">Commit your changes in Git, then push hello code changes tooAzure.</span></span>

```bash
git commit -am "added article comment"
git push azure master
```

<span data-ttu-id="3c45b-291">Jednou hello `git push` dokončení, přejděte tooyour webové aplikace Azure a vyzkoušet nové funkce hello.</span><span class="sxs-lookup"><span data-stu-id="3c45b-291">Once hello `git push` is complete, navigate tooyour Azure web app and try out hello new functionality.</span></span>

![Model a databáze změny publikovány tooAzure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

<span data-ttu-id="3c45b-293">Pokud jste dříve přidali všechny články, je stále můžete vidíte.</span><span class="sxs-lookup"><span data-stu-id="3c45b-293">If you added any articles earlier, you still can see them.</span></span> <span data-ttu-id="3c45b-294">Stávající data v databázi vaší Cosmos není ztraceny.</span><span class="sxs-lookup"><span data-stu-id="3c45b-294">Existing data in your Cosmos DB is not lost.</span></span> <span data-ttu-id="3c45b-295">Navíc schéma data aktualizace toohello a svoje existující data zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="3c45b-295">Also, your updates toohello data schema and leaves your existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="3c45b-296">Diagnostické protokoly datového proudu</span><span class="sxs-lookup"><span data-stu-id="3c45b-296">Stream diagnostic logs</span></span> 

<span data-ttu-id="3c45b-297">Při spuštění vaší aplikace Node.js ve službě Azure App Service můžete získat hello konzoly protokoly vytvoření kanálu tooyour terminálu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-297">While your Node.js application runs in Azure App Service, you can get hello console logs piped tooyour terminal.</span></span> <span data-ttu-id="3c45b-298">Tímto způsobem můžete získat hello stejné diagnostické zprávy toohelp ladění chyb aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c45b-298">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="3c45b-299">toostart protokolu streamování, použijte hello [az webapp protokolu poškozené databáze za](/cli/azure/webapp/log#tail) příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c45b-299">toostart log streaming, use hello [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

<span data-ttu-id="3c45b-300">Po zahájení vysílání datového proudu protokolu, aktualizujte Azure webové aplikace v prohlížeči tooget hello některé webový provoz.</span><span class="sxs-lookup"><span data-stu-id="3c45b-300">Once log streaming has started, refresh your Azure web app in hello browser tooget some web traffic.</span></span> <span data-ttu-id="3c45b-301">Nyní uvidíte, protokoly konzoly přesměruje tooyour terminálu.</span><span class="sxs-lookup"><span data-stu-id="3c45b-301">You now see console logs piped tooyour terminal.</span></span>

<span data-ttu-id="3c45b-302">Zastavení protokolu streamování kdykoli zadáním `Ctrl+C`.</span><span class="sxs-lookup"><span data-stu-id="3c45b-302">Stop log streaming at any time by typing `Ctrl+C`.</span></span> 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="3c45b-303">Správa Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3c45b-303">Manage your Azure web app</span></span>

<span data-ttu-id="3c45b-304">Přejděte toohello [portál Azure](https://portal.azure.com) toosee hello webovou aplikaci jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3c45b-304">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span>

<span data-ttu-id="3c45b-305">V levé nabídce hello, klikněte na **App Services**, pak klikněte na název hello Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c45b-305">From hello left menu, click **App Services**, then click hello name of your Azure web app.</span></span>

![Portálu tooAzure webové aplikace](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

<span data-ttu-id="3c45b-307">Ve výchozím nastavení, hello portál zobrazuje vaší webové aplikace **přehled** stránky.</span><span class="sxs-lookup"><span data-stu-id="3c45b-307">By default, hello portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="3c45b-308">Tato stránka poskytuje přehled, jak si vaše aplikace stojí.</span><span class="sxs-lookup"><span data-stu-id="3c45b-308">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="3c45b-309">Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.</span><span class="sxs-lookup"><span data-stu-id="3c45b-309">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="3c45b-310">Hello karty na levé straně stránky hello hello zobrazit stránky hello jinou konfiguraci, které můžete otevřít.</span><span class="sxs-lookup"><span data-stu-id="3c45b-310">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span>

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a><span data-ttu-id="3c45b-312">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c45b-312">Next steps</span></span>

<span data-ttu-id="3c45b-313">Co jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="3c45b-313">What you learned:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c45b-314">Vytvořit databázi MongoDB v Azure</span><span class="sxs-lookup"><span data-stu-id="3c45b-314">Create a MongoDB database in Azure</span></span>
> * <span data-ttu-id="3c45b-315">Připojit tooMongoDB aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="3c45b-315">Connect a Node.js app tooMongoDB</span></span>
> * <span data-ttu-id="3c45b-316">Nasazení aplikace tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="3c45b-316">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="3c45b-317">Aktualizovat hello datový model a znovu nasaďte aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3c45b-317">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="3c45b-318">Datový proud protokolů z Azure tooyour terminálu</span><span class="sxs-lookup"><span data-stu-id="3c45b-318">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="3c45b-319">Spravovat aplikace hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3c45b-319">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="3c45b-320">Posunutí toohello další kurz toolearn jak toomap vlastní DNS název tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c45b-320">Advance toohello next tutorial toolearn how toomap a custom DNS name tooyour web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="3c45b-321">Mapovat existující vlastní DNS název tooAzure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3c45b-321">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
