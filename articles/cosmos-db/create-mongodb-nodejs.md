---
title: "aaaConnect tooAzure aplikace MongoDB Cosmos DB pomocí Node.js | Microsoft Docs"
description: "Zjistěte, jak tooconnect existující aplikace Node.js MongoDB tooAzure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/19/2017
ms.author: mimig
ms.openlocfilehash: 4bc4f17a31d8c18d1ce5e3f002462f4d48eeb1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-migrate-an-existing-nodejs-mongodb-web-app"></a><span data-ttu-id="64a0b-103">Služba Azure Cosmos DB: Migrace stávající webové aplikace MongoDB s podporou Node.js</span><span class="sxs-lookup"><span data-stu-id="64a0b-103">Azure Cosmos DB: Migrate an existing Node.js MongoDB web app</span></span> 

<span data-ttu-id="64a0b-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="64a0b-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="64a0b-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="64a0b-106">Tento rychlý start předvádí jak toouse existující [MongoDB](mongodb-introduction.md) aplikace napsané v Node.js a připojte ho tooyour Azure Cosmos DB databáze, která podporuje připojení klienta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-106">This quickstart demonstrates how toouse an existing [MongoDB](mongodb-introduction.md) app written in Node.js and connect it tooyour Azure Cosmos DB database, which supports MongoDB client connections.</span></span> <span data-ttu-id="64a0b-107">Jinými slovy aplikace Node.js pouze ví, že se počítač připojuje tooa databáze pomocí rozhraní API MongoDB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-107">In other words, your Node.js application only knows that it's connecting tooa database using MongoDB APIs.</span></span> <span data-ttu-id="64a0b-108">Je transparentní toohello aplikace, která hello dat je uložen v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-108">It is transparent toohello application that hello data is stored in Azure Cosmos DB.</span></span>

<span data-ttu-id="64a0b-109">Po dokončení budete mít ve službě [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) spuštěnou aplikaci MEAN (MongoDB, Express, AngularJS a Node.js).</span><span class="sxs-lookup"><span data-stu-id="64a0b-109">When you are done, you will have a MEAN application (MongoDB, Express, AngularJS, and Node.js) running on [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> 

![Aplikace MEAN.js spuštěná v rámci služby Azure App Service](./media/create-mongodb-nodejs/meanjs-in-azure.png)


[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="64a0b-111">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="64a0b-111">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="64a0b-112">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="64a0b-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="64a0b-113">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="64a0b-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="64a0b-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="64a0b-114">Prerequisites</span></span> 
<span data-ttu-id="64a0b-115">Kromě toho tooAzure rozhraní příkazového řádku, musíte [Node.js](https://nodejs.org/) a [Git](http://www.git-scm.com/downloads) nainstalovány místně toorun `npm` a `git` příkazy.</span><span class="sxs-lookup"><span data-stu-id="64a0b-115">In addition tooAzure CLI, you need [Node.js](https://nodejs.org/) and [Git](http://www.git-scm.com/downloads) installed locally toorun `npm` and `git` commands.</span></span>

<span data-ttu-id="64a0b-116">Měli byste mít praktickou znalost Node.js.</span><span class="sxs-lookup"><span data-stu-id="64a0b-116">You should have working knowledge of Node.js.</span></span> <span data-ttu-id="64a0b-117">Tento rychlý start není určený toohelp k vývoji aplikací Node.js obecně.</span><span class="sxs-lookup"><span data-stu-id="64a0b-117">This quickstart is not intended toohelp you with developing Node.js applications in general.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="64a0b-118">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="64a0b-118">Clone hello sample application</span></span>

<span data-ttu-id="64a0b-119">Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="64a0b-119">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

<span data-ttu-id="64a0b-120">Spusťte následující příkazy tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-120">Run hello following commands tooclone hello sample repository.</span></span> <span data-ttu-id="64a0b-121">Tato ukázka úložiště obsahuje výchozí hello [MEAN.js](http://meanjs.org/) aplikace.</span><span class="sxs-lookup"><span data-stu-id="64a0b-121">This sample repository contains hello default [MEAN.js](http://meanjs.org/) application.</span></span> 

```bash
git clone https://github.com/prashanthmadi/mean
```

## <a name="run-hello-application"></a><span data-ttu-id="64a0b-122">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="64a0b-122">Run hello application</span></span>

<span data-ttu-id="64a0b-123">Instalace hello požadované balíčky a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-123">Install hello required packages and start hello application.</span></span>

```bash
cd mean
npm install
npm start
```

## <a name="log-in-tooazure"></a><span data-ttu-id="64a0b-124">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="64a0b-124">Log in tooAzure</span></span>

<span data-ttu-id="64a0b-125">Pokud používáte nainstalovaný Azure CLI, přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.</span><span class="sxs-lookup"><span data-stu-id="64a0b-125">If you are using an installed Azure CLI, log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> <span data-ttu-id="64a0b-126">Pokud používáte hello prostředí cloudu Azure, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="64a0b-126">You can skip this step if you're using hello Azure Cloud Shell.</span></span>

```azurecli
az login 
``` 
   
## <a name="add-hello-azure-cosmos-db-module"></a><span data-ttu-id="64a0b-127">Přidat modul Azure Cosmos DB hello</span><span class="sxs-lookup"><span data-stu-id="64a0b-127">Add hello Azure Cosmos DB module</span></span>

<span data-ttu-id="64a0b-128">Pokud používáte nainstalované rozhraní příkazového řádku Azure, zkontrolujte toosee Pokud hello `cosmosdb` spuštěním hello je již nainstalována součást `az` příkaz.</span><span class="sxs-lookup"><span data-stu-id="64a0b-128">If you are using an installed Azure CLI, check toosee if hello `cosmosdb` component is already installed by running hello `az` command.</span></span> <span data-ttu-id="64a0b-129">Pokud `cosmosdb` je v seznamu příkazů, základní text hello, pokračovat toohello další příkaz.</span><span class="sxs-lookup"><span data-stu-id="64a0b-129">If `cosmosdb` is in hello list of base commands, proceed toohello next command.</span></span> <span data-ttu-id="64a0b-130">Pokud používáte hello prostředí cloudu Azure, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="64a0b-130">You can skip this step if you're using hello Azure Cloud Shell.</span></span>

<span data-ttu-id="64a0b-131">Pokud `cosmosdb` není v seznamu příkazů, základní text hello, přeinstalujte [Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="64a0b-131">If `cosmosdb` is not in hello list of base commands, reinstall [Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="64a0b-132">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="64a0b-132">Create a resource group</span></span>

<span data-ttu-id="64a0b-133">Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s hello [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="64a0b-133">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="64a0b-134">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="64a0b-134">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="64a0b-135">Hello následující příklad vytvoří skupinu prostředků v oblasti západní Evropa hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-135">hello following example creates a resource group in hello West Europe region.</span></span> <span data-ttu-id="64a0b-136">Vyberte jedinečný název pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-136">Choose a unique name for hello resource group.</span></span>

<span data-ttu-id="64a0b-137">Pokud používáte prostředí cloudu Azure, klikněte na tlačítko **zkuste ho**, postupujte podle pokynů na obrazovce toologin hello a pak zkopírujte hello příkazu do příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-137">If you are using Azure Cloud Shell, click **Try It**, follow hello onscreen prompts toologin, then copy hello command into hello command prompt.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="64a0b-138">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="64a0b-138">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="64a0b-139">Vytvoření účtu Azure Cosmos DB s hello [vytvořit az cosmosdb](/cli/azure/cosmosdb#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="64a0b-139">Create an Azure Cosmos DB account with hello [az cosmosdb create](/cli/azure/cosmosdb#create) command.</span></span>

<span data-ttu-id="64a0b-140">V hello následující příkaz, nahraďte prosím vlastní jedinečný název účtu Azure Cosmos DB, kde uvidíte hello `<cosmosdb-name>` zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="64a0b-140">In hello following command, please substitute your own unique Azure Cosmos DB account name where you see hello `<cosmosdb-name>` placeholder.</span></span> <span data-ttu-id="64a0b-141">Tento jedinečný název se použije jako součást váš koncový bod Azure Cosmos DB (`https://<cosmosdb-name>.documents.azure.com/`), takže název hello musí toobe jedinečný mezi všechny účty Azure Cosmos DB v Azure.</span><span class="sxs-lookup"><span data-stu-id="64a0b-141">This unique name will be used as part of your Azure Cosmos DB endpoint (`https://<cosmosdb-name>.documents.azure.com/`), so hello name needs toobe unique across all Azure Cosmos DB accounts in Azure.</span></span> 

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

<span data-ttu-id="64a0b-142">Hello `--kind MongoDB` parametr povolí připojení klientů MongoDB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-142">hello `--kind MongoDB` parameter enables MongoDB client connections.</span></span>

<span data-ttu-id="64a0b-143">Při vytvoření účtu Azure Cosmos DB hello hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace.</span><span class="sxs-lookup"><span data-stu-id="64a0b-143">When hello Azure Cosmos DB account is created, hello Azure CLI shows information similar toohello following example.</span></span> 

> [!NOTE]
> <span data-ttu-id="64a0b-144">Tento příklad používá JSON jako hello rozhraní příkazového řádku Azure výstupní formát, což je výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-144">This example uses JSON as hello Azure CLI output format, which is hello default.</span></span> <span data-ttu-id="64a0b-145">toouse jiné výstup formátu najdete v tématu [výstup formátů pro příkazy Azure CLI 2.0](https://docs.microsoft.com/cli/azure/format-output-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="64a0b-145">toouse another output format, see [Output formats for Azure CLI 2.0 commands](https://docs.microsoft.com/cli/azure/format-output-azure-cli).</span></span>

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb-name>.documents.azure.com:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb-name>",
  "kind": "MongoDB",
  "location": "West Europe",
  "name": "<cosmosdb-name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

## <a name="connect-your-nodejs-application-toohello-database"></a><span data-ttu-id="64a0b-146">Připojit databáze toohello aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="64a0b-146">Connect your Node.js application toohello database</span></span>

<span data-ttu-id="64a0b-147">V tomto kroku připojíte MEAN.js ukázkové aplikace tooan Azure Cosmos DB databázi, kterou jste právě vytvořili, pomocí připojovacího řetězce MongoDB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-147">In this step, you connect your MEAN.js sample application tooan Azure Cosmos DB database you just created, using a MongoDB connection string.</span></span> 

<a name="devconfig"></a>
## <a name="configure-hello-connection-string-in-your-nodejs-application"></a><span data-ttu-id="64a0b-148">Konfigurace v aplikaci Node.js hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="64a0b-148">Configure hello connection string in your Node.js application</span></span>

<span data-ttu-id="64a0b-149">V úložišti MEAN.js otevřete `config/env/local-development.js`.</span><span class="sxs-lookup"><span data-stu-id="64a0b-149">In your MEAN.js repository, open `config/env/local-development.js`.</span></span>

<span data-ttu-id="64a0b-150">Nahraďte hello obsah tohoto souboru hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="64a0b-150">Replace hello content of this file with hello following code.</span></span> <span data-ttu-id="64a0b-151">Ujistěte se, tooalso nahradit hello dva `<cosmosdb-name>` zástupné texty názvem svého účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-151">Be sure tooalso replace hello two `<cosmosdb-name>` placeholders with your Azure Cosmos DB account name.</span></span>

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

## <a name="retrieve-hello-key"></a><span data-ttu-id="64a0b-152">Načíst klíč hello</span><span class="sxs-lookup"><span data-stu-id="64a0b-152">Retrieve hello key</span></span>

<span data-ttu-id="64a0b-153">Pořadí tooconnect tooan Azure Cosmos DB databáze je nutné klíč databáze hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-153">In order tooconnect tooan Azure Cosmos DB database, you need hello database key.</span></span> <span data-ttu-id="64a0b-154">Použití hello [az cosmosdb seznamu klíčů](/cli/azure/cosmosdb#list-keys) příkaz tooretrieve hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="64a0b-154">Use hello [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) command tooretrieve hello primary key.</span></span>

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb-name> --resource-group myResourceGroup --query "primaryMasterKey"
```

<span data-ttu-id="64a0b-155">Hello rozhraní příkazového řádku Azure výstupy informace podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="64a0b-155">hello Azure CLI outputs information similar toohello following example.</span></span> 

```json
"RUayjYjixJDWG5xTqIiXjC..."
```

<span data-ttu-id="64a0b-156">Zkopírujte hodnotu hello `primaryMasterKey`.</span><span class="sxs-lookup"><span data-stu-id="64a0b-156">Copy hello value of `primaryMasterKey`.</span></span> <span data-ttu-id="64a0b-157">Vložit přes hello `<primary_master_key>` v `local-development.js`.</span><span class="sxs-lookup"><span data-stu-id="64a0b-157">Paste this over hello  `<primary_master_key>` in `local-development.js`.</span></span>

<span data-ttu-id="64a0b-158">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="64a0b-158">Save your changes.</span></span>

### <a name="run-hello-application-again"></a><span data-ttu-id="64a0b-159">Spusťte aplikaci hello znovu.</span><span class="sxs-lookup"><span data-stu-id="64a0b-159">Run hello application again.</span></span>

<span data-ttu-id="64a0b-160">Spusťte `npm start` znovu.</span><span class="sxs-lookup"><span data-stu-id="64a0b-160">Run `npm start` again.</span></span> 

```bash
npm start
```

<span data-ttu-id="64a0b-161">Zprávy konzoly by měl nyní zjistíte, že prostředí vývoj hello je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="64a0b-161">A console message should now tell you that hello development environment is up and running.</span></span> 

<span data-ttu-id="64a0b-162">Přejděte příliš`http://localhost:3000` v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="64a0b-162">Navigate too`http://localhost:3000` in a browser.</span></span> <span data-ttu-id="64a0b-163">Klikněte na tlačítko **zaregistrovat** v horní nabídce a zkuste to toocreate hello dvě fiktivní uživatele.</span><span class="sxs-lookup"><span data-stu-id="64a0b-163">Click **Sign Up** in hello top menu and try toocreate two dummy users.</span></span> 

<span data-ttu-id="64a0b-164">Hello MEAN.js ukázkové aplikace ukládá data uživatele v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-164">hello MEAN.js sample application stores user data in hello database.</span></span> <span data-ttu-id="64a0b-165">Pokud jste úspěšné a do automaticky přihlásí MEAN.js hello vytvořený uživatel a funkčnost připojení k databázi Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="64a0b-165">If you are successful and MEAN.js automatically signs into hello created user, then your Azure Cosmos DB connection is working.</span></span> 

![MEAN.js připojí úspěšně tooMongoDB](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## <a name="view-data-in-data-explorer"></a><span data-ttu-id="64a0b-167">Zobrazení dat v Průzkumníku dat</span><span class="sxs-lookup"><span data-stu-id="64a0b-167">View data in Data Explorer</span></span>

<span data-ttu-id="64a0b-168">Data uložená pomocí Azure DB Cosmos je k dispozici tooview, dotazů a spuštění obchodní logiky na v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="64a0b-168">Data stored by an Azure Cosmos DB is available tooview, query, and run business-logic on in hello Azure portal.</span></span>

<span data-ttu-id="64a0b-169">tooview, dotazování a pracovat s daty uživatele hello vytvořili v předchozím kroku hello přihlášení toohello [portál Azure](https://portal.azure.com) ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="64a0b-169">tooview, query, and work with hello user data created in hello previous step, login toohello [Azure portal](https://portal.azure.com) in your web browser.</span></span>

<span data-ttu-id="64a0b-170">Hello nejvyšší vyhledávacího pole zadejte Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-170">In hello top Search box, type Azure Cosmos DB.</span></span> <span data-ttu-id="64a0b-171">Po otevření okna účtu služby Cosmos DB vyberte účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-171">When your Cosmos DB account blade opens, select your Cosmos DB account.</span></span> <span data-ttu-id="64a0b-172">V levé navigační hello klikněte na tlačítko Průzkumníku dat.</span><span class="sxs-lookup"><span data-stu-id="64a0b-172">In hello left navigation, click Data Explorer.</span></span> <span data-ttu-id="64a0b-173">Rozbalte v podokně Kolekce hello kolekce a pak můžete zobrazit dokumenty hello hello kolekce, hello dotaz na data a i vytvořit a spustit uložené procedury, triggery a UDF.</span><span class="sxs-lookup"><span data-stu-id="64a0b-173">Expand your collection in hello Collections pane, and then you can view hello documents in hello collection, query hello data, and even create and run stored procedures, triggers, and UDFs.</span></span> 

![Průzkumník dat v hello portálu Azure](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)


## <a name="deploy-hello-nodejs-application-tooazure"></a><span data-ttu-id="64a0b-175">Nasazení tooAzure aplikace Node.js hello</span><span class="sxs-lookup"><span data-stu-id="64a0b-175">Deploy hello Node.js application tooAzure</span></span>

<span data-ttu-id="64a0b-176">V tomto kroku nasadíte vaší tooAzure aplikace Node.js připojené MongoDB Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-176">In this step, you deploy your MongoDB-connected Node.js application tooAzure Cosmos DB.</span></span>

<span data-ttu-id="64a0b-177">Může mít k tomu, že hello konfigurační soubor, který jste změnili dříve je pro hello vývojové prostředí (`/config/env/local-development.js`).</span><span class="sxs-lookup"><span data-stu-id="64a0b-177">You may have noticed that hello configuration file that you changed earlier is for hello development environment (`/config/env/local-development.js`).</span></span> <span data-ttu-id="64a0b-178">Při nasazení vaší aplikace tooApp služby, se spustí v provozním prostředí hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="64a0b-178">When you deploy your application tooApp Service, it will run in hello production environment by default.</span></span> <span data-ttu-id="64a0b-179">Proto nyní, budete potřebovat toomake hello stejné změnit toohello příslušného konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="64a0b-179">So now, you need toomake hello same change toohello respective configuration file.</span></span>

<span data-ttu-id="64a0b-180">V úložišti MEAN.js otevřete `config/env/production.js`.</span><span class="sxs-lookup"><span data-stu-id="64a0b-180">In your MEAN.js repository, open `config/env/production.js`.</span></span>

<span data-ttu-id="64a0b-181">V hello `db` objektu, nahraďte hodnotu hello `uri` jako zobrazit v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="64a0b-181">In hello `db` object, replace hello value of `uri` as show in hello following example.</span></span> <span data-ttu-id="64a0b-182">Být jisti tooreplace hello zástupné symboly jako před.</span><span class="sxs-lookup"><span data-stu-id="64a0b-182">Be sure tooreplace hello placeholders as before.</span></span>

```javascript
'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean?ssl=true&sslverifycertificate=false',
```

> [!NOTE] 
> <span data-ttu-id="64a0b-183">Hello `ssl=true` možnost je důležité, protože [Azure Cosmos DB vyžaduje SSL](connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="64a0b-183">hello `ssl=true` option is important because [Azure Cosmos DB requires SSL](connect-mongodb-account.md#connection-string-requirements).</span></span> 
>
>

<span data-ttu-id="64a0b-184">V terminálu hello potvrďte všechny změny do Git.</span><span class="sxs-lookup"><span data-stu-id="64a0b-184">In hello terminal, commit all your changes into Git.</span></span> <span data-ttu-id="64a0b-185">Můžete zkopírovat i toorun příkazy dohromady.</span><span class="sxs-lookup"><span data-stu-id="64a0b-185">You can copy both commands toorun them together.</span></span>

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## <a name="clean-up-resources"></a><span data-ttu-id="64a0b-186">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="64a0b-186">Clean up resources</span></span>

<span data-ttu-id="64a0b-187">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64a0b-187">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="64a0b-188">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="64a0b-188">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="64a0b-189">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="64a0b-189">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64a0b-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64a0b-190">Next steps</span></span>

<span data-ttu-id="64a0b-191">V tento rychlý start, když jste se naučili jak toocreate Azure DB Cosmos účtu a vytvořte kolekci MongoDB pomocí Průzkumníku dat hello.</span><span class="sxs-lookup"><span data-stu-id="64a0b-191">In this quickstart, you've learned how toocreate an Azure Cosmos DB account and create a MongoDB collection using hello Data Explorer.</span></span> <span data-ttu-id="64a0b-192">Nyní můžete migrovat data tooAzure vaše MongoDB Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="64a0b-192">You can now migrate your MongoDB data tooAzure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="64a0b-193">Importování dat MongoDB do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="64a0b-193">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)
