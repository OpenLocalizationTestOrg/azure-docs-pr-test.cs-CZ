---
title: "aaaDeploy Sails.js webové aplikace tooAzure služby App Service | Microsoft Docs"
description: "Zjistěte, jak toodeploy aplikace Node.js Azure App Service. Tento kurz ukazuje, jak toodeploy Sails.js webové aplikace."
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
ms.openlocfilehash: f5b2518b9c87c040845f7268763862be8c15e83e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-sailsjs-web-app-tooazure-app-service"></a><span data-ttu-id="83d43-104">Nasazení aplikace tooAzure Sails.js webové služby App Service</span><span class="sxs-lookup"><span data-stu-id="83d43-104">Deploy a Sails.js web app tooAzure App Service</span></span>
<span data-ttu-id="83d43-105">Tento kurz ukazuje, jak toodeploy tooAzure Sails.js aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="83d43-105">This tutorial shows you how toodeploy a Sails.js app tooAzure App Service.</span></span> <span data-ttu-id="83d43-106">V procesu hello můžete glean některé obecné znalosti o tom, tooconfigure vaše toorun aplikace Node.js ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="83d43-106">In hello process, you can glean some general knowledge on how tooconfigure your Node.js app toorun in App Service.</span></span>

<span data-ttu-id="83d43-107">Zde se dozvíte, mají užitečné dovedností:</span><span class="sxs-lookup"><span data-stu-id="83d43-107">Here, you will learn useful skills like:</span></span>

* <span data-ttu-id="83d43-108">Konfigurace aplikace Sails.js spustit ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="83d43-108">Configure a Sails.js app run in App Service.</span></span>
* <span data-ttu-id="83d43-109">Nasaďte aplikaci tooApp služby z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="83d43-109">Deploy an app tooApp Service from hello command line.</span></span>
* <span data-ttu-id="83d43-110">Přečtěte si protokoly tootroubleshoot stderr a stdout žádné problémy při nasazení.</span><span class="sxs-lookup"><span data-stu-id="83d43-110">Read stderr and stdout logs tootroubleshoot any deployment issues.</span></span>
* <span data-ttu-id="83d43-111">Uložení proměnné prostředí mimo zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="83d43-111">Store environment variables outside of source control.</span></span>
* <span data-ttu-id="83d43-112">Přístup k proměnným prostředí Azure z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="83d43-112">Access Azure environment variables from your app.</span></span>
* <span data-ttu-id="83d43-113">Připojte databáze tooa (MongoDB).</span><span class="sxs-lookup"><span data-stu-id="83d43-113">Connect tooa database (MongoDB).</span></span>

<span data-ttu-id="83d43-114">Měli byste mít praktické znalosti Sails.js.</span><span class="sxs-lookup"><span data-stu-id="83d43-114">You should have working knowledge of Sails.js.</span></span> <span data-ttu-id="83d43-115">V tomto kurzu není určený toohelp jste s problémy související s toorunning Sail.js obecně.</span><span class="sxs-lookup"><span data-stu-id="83d43-115">This tutorial is not intended toohelp you with issues related toorunning Sail.js in general.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="83d43-116">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="83d43-116">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="83d43-117">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="83d43-117">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="83d43-118">[Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely</span><span class="sxs-lookup"><span data-stu-id="83d43-118">[Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span>
- <span data-ttu-id="83d43-119">[Azure CLI 2.0](app-service-web-nodejs-sails.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="83d43-119">[Azure CLI 2.0](app-service-web-nodejs-sails.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83d43-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83d43-120">Prerequisites</span></span>
* [<span data-ttu-id="83d43-121">Node.js</span><span class="sxs-lookup"><span data-stu-id="83d43-121">Node.js</span></span>](https://nodejs.org/)
* [<span data-ttu-id="83d43-122">Sails.js</span><span class="sxs-lookup"><span data-stu-id="83d43-122">Sails.js</span></span>](http://sailsjs.org/get-started)
* [<span data-ttu-id="83d43-123">Git</span><span class="sxs-lookup"><span data-stu-id="83d43-123">Git</span></span>](http://www.git-scm.com/downloads)
* [<span data-ttu-id="83d43-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="83d43-124">Azure CLI 2.0</span></span>](/cli/azure/install-az-cli2)
* <span data-ttu-id="83d43-125">Účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="83d43-125">A Microsoft Azure account.</span></span> <span data-ttu-id="83d43-126">Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="83d43-126">If you don't have an account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

> [!NOTE]
> <span data-ttu-id="83d43-127">[App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="83d43-127">You can [Try App Service](https://azure.microsoft.com/try/app-service/) without an Azure account.</span></span> <span data-ttu-id="83d43-128">Vytvoření aplikace starter- and -play se pro až hodinu tooan – nepožaduje se žádná platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="83d43-128">Create a starter app and play with it for up tooan hour--no credit card required, no commitments.</span></span>
> 
> 

## <a name="step-1-create-and-configure-a-sailsjs-app-locally"></a><span data-ttu-id="83d43-129">Krok 1: Vytvoření a konfigurace aplikace Sails.js místně</span><span class="sxs-lookup"><span data-stu-id="83d43-129">Step 1: Create and configure a Sails.js app locally</span></span>
<span data-ttu-id="83d43-130">Nejprve rychle vytvořte výchozí Sails.js aplikace ve vašem vývojovém prostředí pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="83d43-130">First, quickly create a default Sails.js app in your development environment by following these steps:</span></span>

1. <span data-ttu-id="83d43-131">Otevřete hello terminál příkazového řádku podle svého výběru a `CD` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="83d43-131">Open hello command-line terminal of your choice and `CD` tooa working directory.</span></span>
2. <span data-ttu-id="83d43-132">Vytvoření aplikace Sails.js a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="83d43-132">Create a Sails.js app and run it:</span></span>

        sails new <app_name>
        cd <app_name>
        sails lift

    <span data-ttu-id="83d43-133">Ujistěte se, že přejdete toohello výchozí domovskou stránku v http://localhost:1377.</span><span class="sxs-lookup"><span data-stu-id="83d43-133">Make sure you can navigate toohello default home page at http://localhost:1377.</span></span>

1. <span data-ttu-id="83d43-134">V dalším kroku povolení protokolování pro Azure.</span><span class="sxs-lookup"><span data-stu-id="83d43-134">Next, enable logging for Azure.</span></span> <span data-ttu-id="83d43-135">V kořenovém adresáři, vytvořte soubor s názvem `iisnode.yml` a přidejte následující dva řádky hello:</span><span class="sxs-lookup"><span data-stu-id="83d43-135">In your root directory, create a file called `iisnode.yml` and add hello following two lines:</span></span>

        loggingEnabled: true
        logDirectory: iisnode

    <span data-ttu-id="83d43-136">Hello je nyní povoleno protokolování [iisnode](https://github.com/tjanczuk/iisnode) server, že Azure App Service používá toorun aplikací Node.js.</span><span class="sxs-lookup"><span data-stu-id="83d43-136">Logging is now enabled for hello [iisnode](https://github.com/tjanczuk/iisnode) server that Azure App Service uses toorun Node.js apps.</span></span> 
    <span data-ttu-id="83d43-137">Další informace o tom, jak to funguje, najdete v části [jak toodebug Node.js webové aplikace v Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="83d43-137">For more information on how this works, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

2. <span data-ttu-id="83d43-138">V dalším kroku nakonfigurujte hello Sails.js aplikace toouse prostředí Azure proměnné.</span><span class="sxs-lookup"><span data-stu-id="83d43-138">Next, configure hello Sails.js app toouse Azure environment variables.</span></span> <span data-ttu-id="83d43-139">Otevřete config/env/production.js tooconfigure provozním prostředí a nastavte `port` a `hookTimeout`:</span><span class="sxs-lookup"><span data-stu-id="83d43-139">Open config/env/production.js tooconfigure your production environment, and set `port` and `hookTimeout`:</span></span>

        module.exports = {

            // Use process.env.port toohandle web requests toohello default HTTP port
            port: process.env.port,
            // Increase hooks timout too30 seconds
            // This avoids hello Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    <span data-ttu-id="83d43-140">Tato nastavení konfigurace v dokumentaci můžete najít [Sails.js dokumentaci](http://sailsjs.org/documentation/reference/configuration/sails-config).</span><span class="sxs-lookup"><span data-stu-id="83d43-140">You can find documentation for these configuration settings in the  [Sails.js Documentation](http://sailsjs.org/documentation/reference/configuration/sails-config).</span></span>

4. <span data-ttu-id="83d43-141">Dále používat pevné kódování hello verze Node.js, že chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="83d43-141">Next, hardcode hello Node.js version you want toouse.</span></span> <span data-ttu-id="83d43-142">V souboru package.json, přidejte následující hello `engines` vlastnost tooset hello Node.js verze tooone, který má být.</span><span class="sxs-lookup"><span data-stu-id="83d43-142">In package.json, add hello following `engines` property tooset hello Node.js version tooone that we want.</span></span>

        "engines": {
            "node": "6.9.1"
        },

5. <span data-ttu-id="83d43-143">Nakonec inicializovat úložiště Git a potvrďte svoje soubory.</span><span class="sxs-lookup"><span data-stu-id="83d43-143">Finally, initialize a Git repository and commit your files.</span></span> <span data-ttu-id="83d43-144">V hello kořenový adresář aplikace (kde package.json je), spusťte následující příkazy Gitu hello:</span><span class="sxs-lookup"><span data-stu-id="83d43-144">In hello application root (where package.json is), run hello following Git commands:</span></span>

        git init
        git add .
        git commit -m "<your commit message>"

<span data-ttu-id="83d43-145">Váš kód je připraven toobe nasazení.</span><span class="sxs-lookup"><span data-stu-id="83d43-145">Your code is ready toobe deployed.</span></span> 

## <a name="step-2-create-an-azure-app-and-deploy-sailsjs"></a><span data-ttu-id="83d43-146">Krok 2: Vytvoření aplikace Azure a nasadit Sails.js</span><span class="sxs-lookup"><span data-stu-id="83d43-146">Step 2: Create an Azure app and deploy Sails.js</span></span>

<span data-ttu-id="83d43-147">Dále vytvořte hello prostředek služby App Service v Azure a nasazení vaší aplikace tooit Sails.js.</span><span class="sxs-lookup"><span data-stu-id="83d43-147">Next, create hello App Service resource in Azure and deploy your Sails.js app tooit.</span></span>

1. <span data-ttu-id="83d43-148">přihlášení tooAzure takto:</span><span class="sxs-lookup"><span data-stu-id="83d43-148">log in tooAzure like so:</span></span>

        az login

    <span data-ttu-id="83d43-149">Postupujte podle hello výzva toocontinue hello přihlášení v prohlížeči pomocí účtu Microsoft, který obsahuje předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="83d43-149">Follow hello prompt toocontinue hello login in a browser with a Microsoft account that has your Azure subscription.</span></span>

3. <span data-ttu-id="83d43-150">Nastavit uživatele hello nasazení pro službu App Service.</span><span class="sxs-lookup"><span data-stu-id="83d43-150">Set hello deployment user for App Service.</span></span> <span data-ttu-id="83d43-151">Později pomocí těchto přihlašovacích údajů nasadíte kód.</span><span class="sxs-lookup"><span data-stu-id="83d43-151">You will deploy code using these credentials later.</span></span>
   
        az appservice web deployment user set --user-name <username> --password <password>

3. <span data-ttu-id="83d43-152">Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s názvem.</span><span class="sxs-lookup"><span data-stu-id="83d43-152">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with a name.</span></span> <span data-ttu-id="83d43-153">V tomto kurzu Node.js nepotřebujete skutečně tooknow co je.</span><span class="sxs-lookup"><span data-stu-id="83d43-153">For this Node.js tutorial, you don't really need tooknow what it is.</span></span>

        az group create --location "<location>" --name my-sailsjs-app-group

    <span data-ttu-id="83d43-154">toosee jaké možné hodnoty, které můžete použít pro `<location>`, použijte hello `az appservice list-locations` rozhraní příkazového řádku příkaz.</span><span class="sxs-lookup"><span data-stu-id="83d43-154">toosee what possible values you can use for `<location>`, use hello `az appservice list-locations` CLI command.</span></span>

3. <span data-ttu-id="83d43-155">Vytvoření "FREE" [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) s názvem.</span><span class="sxs-lookup"><span data-stu-id="83d43-155">Create a "FREE" [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) with a name.</span></span> <span data-ttu-id="83d43-156">V tomto kurzu Node.js právě vědět, že vám nebude nic účtováno pro webové aplikace v tomto plánu.</span><span class="sxs-lookup"><span data-stu-id="83d43-156">For this Node.js tutorial, just know that you won't be charged for web apps in this plan.</span></span>

        az appservice plan create --name my-sailsjs-appservice-plan --resource-group my-sailsjs-app-group --sku FREE

4. <span data-ttu-id="83d43-157">Vytvořte novou webovou aplikaci s jedinečným názvem ve značce `<app_name>`.</span><span class="sxs-lookup"><span data-stu-id="83d43-157">Create a new web app with a unique name in `<app_name>`.</span></span>

        az appservice web create --name <app_name> --resource-group my-sailsjs-app-group --plan my-sailsjs-appservice-plan

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a><span data-ttu-id="83d43-158">Krok 3: Konfigurace a nasazení aplikace Sails.js</span><span class="sxs-lookup"><span data-stu-id="83d43-158">Step 3: Configure and deploy your Sails.js app</span></span>

1. <span data-ttu-id="83d43-159">Nakonfigurujte místní nasazení Git pro vaše nová webová aplikace s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="83d43-159">Configure local Git deployment for your new web app with hello following command:</span></span>

        az appservice web source-control config-local-git --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="83d43-160">Zobrazí se výstup JSON tímto způsobem, což znamená, že je nakonfigurovaný vzdáleného úložiště Git této hello:</span><span class="sxs-lookup"><span data-stu-id="83d43-160">You will get a JSON output like this, which means that hello remote Git repository is configured:</span></span>

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. <span data-ttu-id="83d43-161">Adresa URL hello v hello JSON přidat jako vzdálené řízení Git pro místní úložiště (nazývá `azure` pro jednoduchost).</span><span class="sxs-lookup"><span data-stu-id="83d43-161">Add hello URL in hello JSON as a Git remote for your local repository (called `azure` for simplicity).</span></span>

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. <span data-ttu-id="83d43-162">Nasazení vaší ukázkový kód toohello `azure` vzdálené Git.</span><span class="sxs-lookup"><span data-stu-id="83d43-162">Deploy your sample code toohello `azure` Git remote.</span></span> <span data-ttu-id="83d43-163">Pokud budete vyzváni, použijte hello přihlašovací údaje nasazení, které jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="83d43-163">When prompted, use hello deployment credentials you configured earlier.</span></span>

        git push azure master

7. <span data-ttu-id="83d43-164">Právě nakonec spusťte živou aplikaci Azure v prohlížeči hello:</span><span class="sxs-lookup"><span data-stu-id="83d43-164">Finally, just launch your live Azure app in hello browser:</span></span>

        az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="83d43-165">Teď byste měli vidět text hello stejné Sails.js domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="83d43-165">You should now see hello same Sails.js home page.</span></span>

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a><span data-ttu-id="83d43-166">Řešení potíží s nasazením</span><span class="sxs-lookup"><span data-stu-id="83d43-166">Troubleshoot your deployment</span></span>
<span data-ttu-id="83d43-167">Pokud vaše aplikace Sails.js selže z nějakého důvodu ve službě App Service, najít protokoly stderr hello toohelp vyřešte je.</span><span class="sxs-lookup"><span data-stu-id="83d43-167">If your Sails.js application fails for some reason in App Service, find hello stderr logs toohelp troubleshoot it.</span></span>
<span data-ttu-id="83d43-168">Další informace najdete v tématu [jak toodebug Node.js webové aplikace v Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="83d43-168">For more information, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>
<span data-ttu-id="83d43-169">Pokud aplikace hello byla úspěšně spuštěna, hello stdout protokolu by měl zobrazit jste obeznámeni uvítací zprávu:</span><span class="sxs-lookup"><span data-stu-id="83d43-169">If hello app has started successfully, hello stdout log should show you hello familiar message:</span></span>

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
    toosee your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    tooshut down Sails, press <CTRL> + C at any time.

<span data-ttu-id="83d43-170">Můžete řídit členitost protokoly stdout hello v hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) souboru.</span><span class="sxs-lookup"><span data-stu-id="83d43-170">You can control granularity of hello stdout logs in hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) file.</span></span>

## <a name="connect-tooa-database-in-azure"></a><span data-ttu-id="83d43-171">Připojit databáze tooa v Azure</span><span class="sxs-lookup"><span data-stu-id="83d43-171">Connect tooa database in Azure</span></span>
<span data-ttu-id="83d43-172">tooconnect tooa databáze v Azure, můžete vytvořit databázi hello zvoleného v Azure, například Azure SQL Database, MySQL, MongoDB, Azure (Redis) Cache, atd. a pomocí odpovídající hello [úložiště adaptér](https://github.com/balderdashy/sails#compatibility) tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="83d43-172">tooconnect tooa database in Azure, you create hello database of your choice in Azure, such as Azure SQL Database, MySQL, MongoDB, Azure (Redis) Cache, etc., and use hello corresponding [datastore adapter](https://github.com/balderdashy/sails#compatibility) tooconnect tooit.</span></span> <span data-ttu-id="83d43-173">Hello kroky v této části ukazují, jak tooconnect tooMongoDB pomocí [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) databáze, který podporuje připojení klienta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="83d43-173">hello steps in this section show you how tooconnect tooMongoDB by using an [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) database, which can support MongoDB client connections.</span></span>

1. <span data-ttu-id="83d43-174">[Vytvoření účtu Cosmos DB s podporou protokolů pro MongoDB](../documentdb/documentdb-create-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="83d43-174">[Create a Cosmos DB account with MongoDB protocol support](../documentdb/documentdb-create-mongodb-account.md).</span></span>
2. <span data-ttu-id="83d43-175">[Vytvoření kolekce Cosmos databáze a databáze](../documentdb/documentdb-create-collection.md).</span><span class="sxs-lookup"><span data-stu-id="83d43-175">[Create a Cosmos DB collection and database](../documentdb/documentdb-create-collection.md).</span></span> <span data-ttu-id="83d43-176">Název Hello hello kolekce není důležité, ale při připojení z Sails.js bude nutné hello název databáze hello.</span><span class="sxs-lookup"><span data-stu-id="83d43-176">hello name of hello collection doesn't matter, but you need hello name of hello database when you connect from Sails.js.</span></span>
3. <span data-ttu-id="83d43-177">[Najít informace o připojení hello pro vaši databázi Cosmos DB](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="83d43-177">[Find hello connection information for your Cosmos DB database](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).</span></span>
2. <span data-ttu-id="83d43-178">Z příkazového řádku terminálu nainstalujte adaptér MongoDB hello:</span><span class="sxs-lookup"><span data-stu-id="83d43-178">From your command-line terminal, install hello MongoDB adapter:</span></span>

        npm install sails-mongo --save

3. <span data-ttu-id="83d43-179">Otevřete config/connections.js a přidejte následující seznam toohello objekt připojení hello:</span><span class="sxs-lookup"><span data-stu-id="83d43-179">Open config/connections.js and add hello following connection object toohello list:</span></span>

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
    > <span data-ttu-id="83d43-180">Hello `ssl: true` možnost je důležité, protože [Cosmos DB vyžaduje](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span><span class="sxs-lookup"><span data-stu-id="83d43-180">hello `ssl: true` option is important because [Cosmos DB requires it](../cosmos-db/connect-mongodb-account.md#connection-string-requirements).</span></span> 
    >
    >

4. <span data-ttu-id="83d43-181">Pro každou proměnnou prostředí (`process.env.*`), je nutné tooset ji ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="83d43-181">For each environment variable (`process.env.*`), you need tooset it in App Service.</span></span> <span data-ttu-id="83d43-182">toodo se spuštění hello následujících příkazů z terminálu.</span><span class="sxs-lookup"><span data-stu-id="83d43-182">toodo this, run hello following commands from your terminal.</span></span> <span data-ttu-id="83d43-183">Informace o připojení hello použijte pro vaše DB Cosmos.</span><span class="sxs-lookup"><span data-stu-id="83d43-183">Use hello connection information for your Cosmos DB.</span></span>

        az appservice web config appsettings update --settings dbuser="<database user>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbpassword="<database password>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbhost="<database hostname>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbport="<database port>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbname="<database name>" --name <app_name> --resource-group my-sailsjs-app-group

    <span data-ttu-id="83d43-184">Uvedení nastavení v nastavení aplikace Azure zajistí citlivých dat mimo správy zdrojového kódu (Git).</span><span class="sxs-lookup"><span data-stu-id="83d43-184">Putting your settings in Azure app settings keeps sensitive data out of your source control (Git).</span></span> <span data-ttu-id="83d43-185">Dále je nutné nakonfigurovat váš vývojový prostředí toouse hello stejné informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="83d43-185">Next, you will configure your development environment toouse hello same connection information.</span></span>
5. <span data-ttu-id="83d43-186">Otevřete config/local.js a přidejte následující objekt připojení hello:</span><span class="sxs-lookup"><span data-stu-id="83d43-186">Open config/local.js and add hello following connections object:</span></span>

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    <span data-ttu-id="83d43-187">Tato konfigurace se přepíše hello nastavení v souboru config/connections.js pro místní prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="83d43-187">This configuration overrides hello settings in your config/connections.js file for hello local environment.</span></span> <span data-ttu-id="83d43-188">Tento soubor je vyloučený hello výchozí .gitignore ve vašem projektu, takže ho nebude uložena v úložišti Git.</span><span class="sxs-lookup"><span data-stu-id="83d43-188">This file is excluded by hello default .gitignore in your project, so it will not be stored in Git.</span></span> <span data-ttu-id="83d43-189">Teď můžete mít tooconnect tooyour Cosmos DB (MongoDB) databáze je z vaší webové aplikace Azure a z místního vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="83d43-189">Now, you are able tooconnect tooyour Cosmos DB (MongoDB) database both from your Azure web app and from your local development environment.</span></span>
6. <span data-ttu-id="83d43-190">Otevřete config/env/production.js tooconfigure provozním prostředí a přidejte následující hello `models` objektu:</span><span class="sxs-lookup"><span data-stu-id="83d43-190">Open config/env/production.js tooconfigure your production environment, and add hello following `models` object:</span></span>

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. <span data-ttu-id="83d43-191">Otevřete config/env/development.js tooconfigure vývojového prostředí a přidejte následující hello `models` objektu:</span><span class="sxs-lookup"><span data-stu-id="83d43-191">Open config/env/development.js tooconfigure your development environment, and add hello following `models` object:</span></span>

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    <span data-ttu-id="83d43-192">`migrate: 'alter'`Umožňuje použít toocreate funkce migrace databáze a snadno aktualizovat kolekce databáze nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="83d43-192">`migrate: 'alter'` lets you use database migration features toocreate and update database collections or tables easily.</span></span> <span data-ttu-id="83d43-193">Však `migrate: 'safe'` se používá pro vaše prostředí Azure (produkční), protože Sails.js neumožňuje toouse `migrate: 'alter'` v produkčním prostředí (viz [Sails.js dokumentace](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).</span><span class="sxs-lookup"><span data-stu-id="83d43-193">However, `migrate: 'safe'` is used for your Azure (production) environment because Sails.js does not allow you toouse `migrate: 'alter'` in a production environment (see [Sails.js Documentation](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).</span></span>
8. <span data-ttu-id="83d43-194">Z hello terminálu [generovat](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [plán, podle kterého rozhraní API](http://sailsjs.org/documentation/concepts/blueprints) jako za normálních okolností byste, pak spustili `sails lift` k vytvoření databáze hello s migrací Sails.js databáze.</span><span class="sxs-lookup"><span data-stu-id="83d43-194">From hello terminal, [generate](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) a Sails.js [blueprint API](http://sailsjs.org/documentation/concepts/blueprints) like you normally would, then run `sails lift` to create hello database with Sails.js database migration.</span></span> <span data-ttu-id="83d43-195">Například:</span><span class="sxs-lookup"><span data-stu-id="83d43-195">For example:</span></span>

         sails generate api mywidget
         sails lift

    <span data-ttu-id="83d43-196">Hello `mywidget` model generované tento příkaz je prázdný, ale můžeme ho použít tooshow, že máme připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="83d43-196">hello `mywidget` model generated by this command is empty, but we can use it tooshow that we have database connectivity.</span></span>
    <span data-ttu-id="83d43-197">Při spuštění `sails lift`, vytvoří hello chybějící kolekce a tabulky pro hello modely vaše aplikace používá.</span><span class="sxs-lookup"><span data-stu-id="83d43-197">When you run `sails lift`, it creates hello missing collections and tables for hello models your app uses.</span></span>
9. <span data-ttu-id="83d43-198">API plán, podle kterého hello přístupu, kterou jste právě vytvořili, v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="83d43-198">Access hello blueprint API you just created in hello browser.</span></span> <span data-ttu-id="83d43-199">Například:</span><span class="sxs-lookup"><span data-stu-id="83d43-199">For example:</span></span>

        http://localhost:1337/mywidget/create

    <span data-ttu-id="83d43-200">Hello rozhraní API by měl vrátit zpět tooyou hello vytvořit položku v okně prohlížeče hello, což znamená, že vaše kolekce byla úspěšně vytvořena.</span><span class="sxs-lookup"><span data-stu-id="83d43-200">hello API should return hello created entry back tooyou in hello browser window, which means that your collection is created  successfully.</span></span>

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. <span data-ttu-id="83d43-201">Nyní push vaší tooAzure změny a procházet tooyour aplikace toomake se, že stále funguje.</span><span class="sxs-lookup"><span data-stu-id="83d43-201">Now, push your changes tooAzure, and browse tooyour app toomake sure it still works.</span></span>

         git add .
         git commit -m "<your commit message>"
         git push azure master
         az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

11. <span data-ttu-id="83d43-202">Přístup k rozhraní API hello plán, podle kterého Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="83d43-202">Access hello blueprint API of your Azure web app.</span></span> <span data-ttu-id="83d43-203">Například:</span><span class="sxs-lookup"><span data-stu-id="83d43-203">For example:</span></span>

         http://<appname>.azurewebsites.net/mywidget/create

     <span data-ttu-id="83d43-204">Pokud hello rozhraní API vrátí jinou novou položku, je vaše webové aplikace Azure rozhovoru tooyour DB Cosmos (MongoDB) databáze.</span><span class="sxs-lookup"><span data-stu-id="83d43-204">If hello API returns another new entry, then your Azure web app is talking tooyour Cosmos DB (MongoDB) database.</span></span>

## <a name="more-resources"></a><span data-ttu-id="83d43-205">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="83d43-205">More resources</span></span>
* [<span data-ttu-id="83d43-206">Začínáme s webovými aplikacemi Node.js ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="83d43-206">Get started with Node.js web apps in Azure App Service</span></span>](app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="83d43-207">Používání modulů Node.js s aplikacemi Azure</span><span class="sxs-lookup"><span data-stu-id="83d43-207">Using Node.js Modules with Azure applications</span></span>](../nodejs-use-node-modules-azure-apps.md)
