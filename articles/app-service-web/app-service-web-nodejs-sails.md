---
title: "Nasazení webové aplikace Sails.js do služby Azure App Service | Microsoft Docs"
description: "Informace o nasazení aplikace Node.js Azure App Service. V tomto kurzu se dozvíte, jak nasadit webové aplikace Sails.js."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 8877ddc8-1476-45ae-9e7f-3c75917b4564
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: e36fc5f5273f98c3cca91973db9910f32443ec7c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a><span data-ttu-id="a4dc7-104">Nasazení webové aplikace Sails.js do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a4dc7-104">Deploy a Sails.js web app to Azure App Service</span></span>
<span data-ttu-id="a4dc7-105">V tomto kurzu se dozvíte, jak nasadit aplikace Sails.js do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-105">This tutorial shows you how to deploy a Sails.js app to Azure App Service.</span></span> <span data-ttu-id="a4dc7-106">V procesu můžete glean některé obecné znalosti o tom, jak nakonfigurovat aplikace Node.js ke spuštění ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-106">In the process, you can glean some general knowledge on how to configure your Node.js app to run in App Service.</span></span>

<span data-ttu-id="a4dc7-107">Zde se dozvíte, mají užitečné dovedností:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-107">Here, you will learn useful skills like:</span></span>

* <span data-ttu-id="a4dc7-108">Konfigurace aplikace Sails.js spustit ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-108">Configure a Sails.js app run in App Service.</span></span>
* <span data-ttu-id="a4dc7-109">Nasazení aplikace do služby App Service z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-109">Deploy an app to App Service from the command line.</span></span>
* <span data-ttu-id="a4dc7-110">Čtení stderr a stdout protokoly a vyřešte všechny problémy nasazení.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-110">Read stderr and stdout logs to troubleshoot any deployment issues.</span></span>
* <span data-ttu-id="a4dc7-111">Uložení proměnné prostředí mimo zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-111">Store environment variables outside of source control.</span></span>
* <span data-ttu-id="a4dc7-112">Přístup k proměnným prostředí Azure z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-112">Access Azure environment variables from your app.</span></span>
* <span data-ttu-id="a4dc7-113">Připojení k databázi (MongoDB).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-113">Connect to a database (MongoDB).</span></span>

<span data-ttu-id="a4dc7-114">Měli byste mít praktické znalosti Sails.js.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-114">You should have working knowledge of Sails.js.</span></span> <span data-ttu-id="a4dc7-115">V tomto kurzu není určeno k vám pomohou s problémy související se spouštěním Sail.js obecně.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-115">This tutorial is not intended to help you with issues related to running Sail.js in general.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="a4dc7-116">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="a4dc7-116">CLI versions to complete the task</span></span>

<span data-ttu-id="a4dc7-117">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-117">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="a4dc7-118">[Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="a4dc7-118">[Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – our CLI for the classic and resource management deployment models</span></span>
- <span data-ttu-id="a4dc7-119">[Azure CLI 2.0](app-service-web-nodejs-sails.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="a4dc7-119">[Azure CLI 2.0](app-service-web-nodejs-sails.md) - our next generation CLI for the resource management deployment model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4dc7-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a4dc7-120">Prerequisites</span></span>
* [<span data-ttu-id="a4dc7-121">Node.js</span><span class="sxs-lookup"><span data-stu-id="a4dc7-121">Node.js</span></span>](https://nodejs.org/)
* [<span data-ttu-id="a4dc7-122">Sails.js</span><span class="sxs-lookup"><span data-stu-id="a4dc7-122">Sails.js</span></span>](http://sailsjs.org/get-started)
* [<span data-ttu-id="a4dc7-123">Git</span><span class="sxs-lookup"><span data-stu-id="a4dc7-123">Git</span></span>](http://www.git-scm.com/downloads)
* [<span data-ttu-id="a4dc7-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a4dc7-124">Azure CLI 2.0</span></span>](/cli/azure/install-az-cli2)
* <span data-ttu-id="a4dc7-125">Účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-125">A Microsoft Azure account.</span></span> <span data-ttu-id="a4dc7-126">Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-126">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="a4dc7-127">[App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-127">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="a4dc7-128">Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-128">Create a starter app and play with it for up to an hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="step-1-create-and-configure-a-sailsjs-app-locally"></a><span data-ttu-id="a4dc7-129">Krok 1: Vytvoření a konfigurace aplikace Sails.js místně</span><span class="sxs-lookup"><span data-stu-id="a4dc7-129">Step 1: Create and configure a Sails.js app locally</span></span>
<span data-ttu-id="a4dc7-130">Nejprve rychle vytvořte výchozí Sails.js aplikace ve vašem vývojovém prostředí pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-130">First, quickly create a default Sails.js app in your development environment by following these steps:</span></span>

1. <span data-ttu-id="a4dc7-131">Otevřete terminál příkazového řádku podle svého výběru a `CD` do pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-131">Open the command-line terminal of your choice and `CD` to a working directory.</span></span>
2. <span data-ttu-id="a4dc7-132">Vytvoření aplikace Sails.js a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-132">Create a Sails.js app and run it:</span></span>

        sails new <app_name>
        cd <app_name>
        sails lift

    <span data-ttu-id="a4dc7-133">Ujistěte se, že můžete přejít na domovskou stránku výchozí v http://localhost:1377.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-133">Make sure you can navigate to the default home page at http://localhost:1377.</span></span>

1. <span data-ttu-id="a4dc7-134">V dalším kroku povolení protokolování pro Azure.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-134">Next, enable logging for Azure.</span></span> <span data-ttu-id="a4dc7-135">V kořenovém adresáři, vytvořte soubor s názvem `iisnode.yml` a přidejte následující dva řádky:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-135">In your root directory, create a file called `iisnode.yml` and add the following two lines:</span></span>

        loggingEnabled: true
        logDirectory: iisnode

    <span data-ttu-id="a4dc7-136">Je nyní povoleno protokolování [iisnode](https://github.com/tjanczuk/iisnode) serveru, který používá Azure App Service spouští aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-136">Logging is now enabled for the [iisnode](https://github.com/tjanczuk/iisnode) server that Azure App Service uses to run Node.js apps.</span></span> 
    <span data-ttu-id="a4dc7-137">Další informace o tom, jak to funguje, najdete v části [postup ladění webové aplikace Node.js ve službě Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-137">For more information on how this works, see [How to debug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

2. <span data-ttu-id="a4dc7-138">V dalším kroku nakonfigurujte aplikace Sails.js používat proměnné prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-138">Next, configure the Sails.js app to use Azure environment variables.</span></span> <span data-ttu-id="a4dc7-139">Otevřete config/env/production.js konfigurovat provozním prostředí, a nastavte `port` a `hookTimeout`:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-139">Open config/env/production.js to configure your production environment, and set `port` and `hookTimeout`:</span></span>

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    <span data-ttu-id="a4dc7-140">Tato nastavení konfigurace v dokumentaci můžete najít [Sails.js dokumentaci](http://sailsjs.org/documentation/reference/configuration/sails-config).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-140">You can find documentation for these configuration settings in the  [Sails.js Documentation](http://sailsjs.org/documentation/reference/configuration/sails-config).</span></span>

4. <span data-ttu-id="a4dc7-141">V dalším kroku závislé Node.js verze, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-141">Next, hardcode the Node.js version you want to use.</span></span> <span data-ttu-id="a4dc7-142">V souboru package.json, přidejte následující `engines` vlastnost nastavující verze Node.js na takový, který má být.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-142">In package.json, add the following `engines` property to set the Node.js version to one that we want.</span></span>

        "engines": {
            "node": "6.9.1"
        },

5. <span data-ttu-id="a4dc7-143">Nakonec inicializovat úložiště Git a potvrďte svoje soubory.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-143">Finally, initialize a Git repository and commit your files.</span></span> <span data-ttu-id="a4dc7-144">V kořenovém adresáři aplikace (kde package.json je) spusťte následující příkazy Git:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-144">In the application root (where package.json is), run the following Git commands:</span></span>

        git init
        git add .
        git commit -m "<your commit message>"

<span data-ttu-id="a4dc7-145">Váš kód je připravená k nasazení.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-145">Your code is ready to be deployed.</span></span> 

## <a name="step-2-create-an-azure-app-and-deploy-sailsjs"></a><span data-ttu-id="a4dc7-146">Krok 2: Vytvoření aplikace Azure a nasadit Sails.js</span><span class="sxs-lookup"><span data-stu-id="a4dc7-146">Step 2: Create an Azure app and deploy Sails.js</span></span>

<span data-ttu-id="a4dc7-147">V dalším kroku v Azure vytvořit prostředek služby App Service a nasazení aplikace Sails.js do ní.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-147">Next, create the App Service resource in Azure and deploy your Sails.js app to it.</span></span>

1. <span data-ttu-id="a4dc7-148">Přihlaste se k Azure takto proto:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-148">log in to Azure like so:</span></span>

        az login

    <span data-ttu-id="a4dc7-149">Postupujte podle výzvy a pokračujte v přihlášení v prohlížeči pomocí účtu Microsoft, který obsahuje předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-149">Follow the prompt to continue the login in a browser with a Microsoft account that has your Azure subscription.</span></span>

3. <span data-ttu-id="a4dc7-150">Nastavte uživatele nasazení pro App Service.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-150">Set the deployment user for App Service.</span></span> <span data-ttu-id="a4dc7-151">Později pomocí těchto přihlašovacích údajů nasadíte kód.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-151">You will deploy code using these credentials later.</span></span>
   
        az appservice web deployment user set --user-name <username> --password <password>

3. <span data-ttu-id="a4dc7-152">Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s názvem.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-152">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with a name.</span></span> <span data-ttu-id="a4dc7-153">V tomto kurzu Node.js nepotřebujete skutečně potřebujete vědět, co je.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-153">For this Node.js tutorial, you don't really need to know what it is.</span></span>

        az group create --location "<location>" --name my-sailsjs-app-group

    <span data-ttu-id="a4dc7-154">K zobrazení možných hodnot, které se dají použít pro `<location>`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-154">To see what possible values you can use for `<location>`, use the `az appservice list-locations` CLI command.</span></span>

3. <span data-ttu-id="a4dc7-155">Vytvoření "FREE" [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) s názvem.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-155">Create a "FREE" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) with a name.</span></span> <span data-ttu-id="a4dc7-156">V tomto kurzu Node.js právě vědět, že vám nebude nic účtováno pro webové aplikace v tomto plánu.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-156">For this Node.js tutorial, just know that you won't be charged for web apps in this plan.</span></span>

        az appservice plan create --name my-sailsjs-appservice-plan --resource-group my-sailsjs-app-group --sku FREE

4. <span data-ttu-id="a4dc7-157">Vytvořte novou webovou aplikaci s jedinečným názvem ve značce `<app_name>`.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-157">Create a new web app with a unique name in `<app_name>`.</span></span>

        az appservice web create --name <app_name> --resource-group my-sailsjs-app-group --plan my-sailsjs-appservice-plan

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a><span data-ttu-id="a4dc7-158">Krok 3: Konfigurace a nasazení aplikace Sails.js</span><span class="sxs-lookup"><span data-stu-id="a4dc7-158">Step 3: Configure and deploy your Sails.js app</span></span>

1. <span data-ttu-id="a4dc7-159">Ke konfiguraci místního nasazení Gitu pro webovou aplikaci použijete následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-159">Configure local Git deployment for your new web app with the following command:</span></span>

        az appservice web source-control config-local-git --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="a4dc7-160">Získáte výstup JSON podobný tomuto. To znamená, že vzdálené úložiště Git je nakonfigurované:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-160">You will get a JSON output like this, which means that the remote Git repository is configured:</span></span>

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. <span data-ttu-id="a4dc7-161">Přidejte adresu URL v kódu JSON jako vzdálené úložiště Git pro vaše místní úložiště (pro zjednodušení nazvané `azure`).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-161">Add the URL in the JSON as a Git remote for your local repository (called `azure` for simplicity).</span></span>

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. <span data-ttu-id="a4dc7-162">Nasaďte ukázkový kód do vzdáleného úložiště Git s názvem `azure`.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-162">Deploy your sample code to the `azure` Git remote.</span></span> <span data-ttu-id="a4dc7-163">Po zobrazení výzvy použijte přihlašovací údaje nasazení, které jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-163">When prompted, use the deployment credentials you configured earlier.</span></span>

        git push azure master

7. <span data-ttu-id="a4dc7-164">Právě nakonec spusťte živou aplikaci Azure v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-164">Finally, just launch your live Azure app in the browser:</span></span>

        az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="a4dc7-165">Měli byste nyní vidět domovské stránce stejné Sails.js.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-165">You should now see the same Sails.js home page.</span></span>

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a><span data-ttu-id="a4dc7-166">Řešení potíží s nasazením</span><span class="sxs-lookup"><span data-stu-id="a4dc7-166">Troubleshoot your deployment</span></span>
<span data-ttu-id="a4dc7-167">Pokud vaše aplikace Sails.js selže z nějakého důvodu ve službě App Service, najděte protokoly stderr k jeho řešení.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-167">If your Sails.js application fails for some reason in App Service, find the stderr logs to help troubleshoot it.</span></span>
<span data-ttu-id="a4dc7-168">Další informace najdete v tématu [postup ladění webové aplikace Node.js ve službě Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-168">For more information, see [How to debug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>
<span data-ttu-id="a4dc7-169">Pokud aplikace byla úspěšně spuštěna, protokol stdout měl zobrazovat známé zprávu:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-169">If the app has started successfully, the stdout log should show you the familiar message:</span></span>

                   .-..-.
    
       Sails              <|    .-..-.
       v0.12.11            |\
                          /|.\
                         / || \
                       ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
       __---___--___---___--___---___--___
     ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

<span data-ttu-id="a4dc7-170">Můžete řídit členitost protokoly stdout v [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) souboru.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-170">You can control granularity of the stdout logs in the [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) file.</span></span>

## <a name="connect-to-a-database-in-azure"></a><span data-ttu-id="a4dc7-171">Připojení k databázi v Azure</span><span class="sxs-lookup"><span data-stu-id="a4dc7-171">Connect to a database in Azure</span></span>
<span data-ttu-id="a4dc7-172">Pro připojení k databázi v Azure, vytvořte databázi zvoleného v Azure, například Azure SQL Database, MySQL, MongoDB, Azure (Redis) Cache, atd. a použijte příslušné [úložiště adaptér](https://github.com/balderdashy/sails#compatibility) připojení k tomuto.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-172">To connect to a database in Azure, you create the database of your choice in Azure, such as Azure SQL Database, MySQL, MongoDB, Azure (Redis) Cache, etc., and use the corresponding [datastore adapter](https://github.com/balderdashy/sails#compatibility) to connect to it.</span></span> <span data-ttu-id="a4dc7-173">Kroky v této části ukazují, jak se připojit k MongoDB pomocí [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) databáze, který podporuje připojení klienta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-173">The steps in this section show you how to connect to MongoDB by using an [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) database, which can support MongoDB client connections.</span></span>

1. <span data-ttu-id="a4dc7-174">[Vytvoření účtu Cosmos DB s podporou protokolů pro MongoDB](../documentdb/documentdb-create-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-174">[Create a Cosmos DB account with MongoDB protocol support](../documentdb/documentdb-create-mongodb-account.md).</span></span>
2. <span data-ttu-id="a4dc7-175">[Vytvoření kolekce Cosmos databáze a databáze](../documentdb/documentdb-create-collection.md).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-175">[Create a Cosmos DB collection and database](../documentdb/documentdb-create-collection.md).</span></span> <span data-ttu-id="a4dc7-176">Název kolekce není důležité, ale při připojení z Sails.js potřebujete název databáze.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-176">The name of the collection doesn't matter, but you need the name of the database when you connect from Sails.js.</span></span>
3. <span data-ttu-id="a4dc7-177">[Najít informace o připojení pro vaši databázi Cosmos DB](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-177">[Find the connection information for your Cosmos DB database](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).</span></span>
2. <span data-ttu-id="a4dc7-178">Z příkazového řádku terminálu nainstalujte adaptér MongoDB:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-178">From your command-line terminal, install the MongoDB adapter:</span></span>

        npm install sails-mongo --save

3. <span data-ttu-id="a4dc7-179">Otevřete config/connections.js a přidejte následující objekt připojení do seznamu:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-179">Open config/connections.js and add the following connection object to the list:</span></span>

        docDbMongo: {
            adapter: 'sails-mongo',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost,
            port: process.env.dbport,
            database: process.env.dbname,
            ssl: true
        },

    > [!NOTE] 
    > <span data-ttu-id="a4dc7-180">`ssl: true` Možnost je důležité, protože [Cosmos DB vyžaduje](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-180">The `ssl: true` option is important because [Cosmos DB requires it](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 
    >
    >

4. <span data-ttu-id="a4dc7-181">Pro každou proměnnou prostředí (`process.env.*`), je nutné nastavit ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-181">For each environment variable (`process.env.*`), you need to set it in App Service.</span></span> <span data-ttu-id="a4dc7-182">Chcete-li to provést, spusťte následující příkazy z terminálu.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-182">To do this, run the following commands from your terminal.</span></span> <span data-ttu-id="a4dc7-183">Použijte informace o připojení pro vaše DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-183">Use the connection information for your Cosmos DB.</span></span>

        az appservice web config appsettings update --settings dbuser="<database user>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbpassword="<database password>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbhost="<database hostname>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbport="<database port>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbname="<database name>" --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="a4dc7-184">Uvedení nastavení v nastavení aplikace Azure zajistí citlivých dat mimo správy zdrojového kódu (Git).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-184">Putting your settings in Azure app settings keeps sensitive data out of your source control (Git).</span></span> <span data-ttu-id="a4dc7-185">Dále nakonfigurujete vývojového prostředí používat stejné informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-185">Next, you will configure your development environment to use the same connection information.</span></span>
5. <span data-ttu-id="a4dc7-186">Otevřete config/local.js a přidejte následující objekt připojení:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-186">Open config/local.js and add the following connections object:</span></span>

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    <span data-ttu-id="a4dc7-187">Tato konfigurace se přepíše nastavení v souboru config/connections.js pro místní prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-187">This configuration overrides the settings in your config/connections.js file for the local environment.</span></span> <span data-ttu-id="a4dc7-188">Tento soubor je vyloučená, a výchozí .gitignore ve vašem projektu, takže ho nebude uložena v úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-188">This file is excluded by the default .gitignore in your project, so it will not be stored in Git.</span></span> <span data-ttu-id="a4dc7-189">Nyní budete moci připojit k vaší databázi DB Cosmos (MongoDB) z vaší webové aplikace Azure a z místního vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-189">Now, you are able to connect to your Cosmos DB (MongoDB) database both from your Azure web app and from your local development environment.</span></span>
6. <span data-ttu-id="a4dc7-190">Otevřete config/env/production.js konfigurace produkční prostředí a přidejte následující `models` objektu:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-190">Open config/env/production.js to configure your production environment, and add the following `models` object:</span></span>

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. <span data-ttu-id="a4dc7-191">Otevřete config/env/development.js konfigurace vývojového prostředí a přidejte následující `models` objektu:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-191">Open config/env/development.js to configure your development environment, and add the following `models` object:</span></span>

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    <span data-ttu-id="a4dc7-192">`migrate: 'alter'`Umožňuje použít k vytváření a aktualizaci databáze kolekce nebo tabulky snadno funkce migrace databáze.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-192">`migrate: 'alter'` lets you use database migration features to create and update database collections or tables easily.</span></span> <span data-ttu-id="a4dc7-193">Ale `migrate: 'safe'` se používá pro vaše prostředí Azure (produkční), protože Sails.js neumožňuje použití `migrate: 'alter'` v produkčním prostředí (v tématu [Sails.js dokumentace](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-193">However, `migrate: 'safe'` is used for your Azure (production) environment because Sails.js does not allow you to use `migrate: 'alter'` in a production environment (see [Sails.js Documentation](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).</span></span>
8. <span data-ttu-id="a4dc7-194">Z terminálu [generovat](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [plán, podle kterého rozhraní API](http://sailsjs.org/documentation/concepts/blueprints) jako za normálních okolností byste, pak spustili `sails lift` vytvořit databázi s migrací Sails.js databáze.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-194">From the terminal, [generate](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) a Sails.js [blueprint API](http://sailsjs.org/documentation/concepts/blueprints) like you normally would, then run `sails lift` to create the database with Sails.js database migration.</span></span> <span data-ttu-id="a4dc7-195">Například:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-195">For example:</span></span>

         sails generate api mywidget
         sails lift

    <span data-ttu-id="a4dc7-196">`mywidget` Model generované tento příkaz je prázdný, ale nemůžeme můžete ji použít k ukazují, že jsme mají připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-196">The `mywidget` model generated by this command is empty, but we can use it to show that we have database connectivity.</span></span>
    <span data-ttu-id="a4dc7-197">Při spuštění `sails lift`, vytvoří chybějící kolekce a tabulky pro modely vaše aplikace používá.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-197">When you run `sails lift`, it creates the missing collections and tables for the models your app uses.</span></span>
9. <span data-ttu-id="a4dc7-198">Přístup k matrici rozhraní API, které jste právě vytvořili, v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-198">Access the blueprint API you just created in the browser.</span></span> <span data-ttu-id="a4dc7-199">Například:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-199">For example:</span></span>

        http://localhost:1337/mywidget/create

    <span data-ttu-id="a4dc7-200">Rozhraní API by měl vrátit vytvořenou položku vám v okně prohlížeče, což znamená, že vaše kolekce byla úspěšně vytvořena.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-200">The API should return the created entry back to you in the browser window, which means that your collection is created  successfully.</span></span>

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. <span data-ttu-id="a4dc7-201">Nyní odešlete své změny do Azure a přejděte do vaší aplikace a ujistěte se, že stále funguje.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-201">Now, push your changes to Azure, and browse to your app to make sure it still works.</span></span>

         git add .
         git commit -m "<your commit message>"
         git push azure master
         az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

11. <span data-ttu-id="a4dc7-202">Přístup plán, podle kterého rozhraní API Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4dc7-202">Access the blueprint API of your Azure web app.</span></span> <span data-ttu-id="a4dc7-203">Například:</span><span class="sxs-lookup"><span data-stu-id="a4dc7-203">For example:</span></span>

         http://<appname>.azurewebsites.net/mywidget/create

     <span data-ttu-id="a4dc7-204">Pokud rozhraní API vrátí jiné nový záznam, vaší webové aplikace Azure rozhovoru k vaší databázi DB Cosmos (MongoDB).</span><span class="sxs-lookup"><span data-stu-id="a4dc7-204">If the API returns another new entry, then your Azure web app is talking to your Cosmos DB (MongoDB) database.</span></span>

## <a name="more-resources"></a><span data-ttu-id="a4dc7-205">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="a4dc7-205">More resources</span></span>
* [<span data-ttu-id="a4dc7-206">Začínáme s webovými aplikacemi Node.js ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a4dc7-206">Get started with Node.js web apps in Azure App Service</span></span>](app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="a4dc7-207">Používání modulů Node.js s aplikacemi Azure</span><span class="sxs-lookup"><span data-stu-id="a4dc7-207">Using Node.js Modules with Azure applications</span></span>](../nodejs-use-node-modules-azure-apps.md)
