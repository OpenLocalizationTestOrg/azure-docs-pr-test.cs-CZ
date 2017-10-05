---
title: "Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service"
description: "Kurz ukazuje, jak pomocí socket.io ve webové aplikaci node.js hostované v Azure."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c4c4af36-3ecf-4619-b586-ca90d53ce35b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: e1aa539e1134884261ea7464bfda6d14815618d4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a><span data-ttu-id="a0ba0-103">Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a0ba0-103">Create a Node.js chat application with Socket.IO in Azure App Service</span></span>
<span data-ttu-id="a0ba0-104">Socket.IO poskytuje v reálném čase komunikaci mezi serverem node.js a klienty, kteří používají objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-104">Socket.IO provides real-time communication between your node.js server and clients using WebSockets.</span></span> <span data-ttu-id="a0ba0-105">Také podporuje přechod na jiné přenosů (například dlouhým dotazováním), které pracují s starší prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-105">It also supports fallback to other transports (such as long polling) that work with older browsers.</span></span> <span data-ttu-id="a0ba0-106">Tento kurz vás provede procesem hostování Socket.IO na základě chatovací aplikace jako webové aplikace Azure a ukazují, jak můžete škálovat aplikace pomocí [Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="a0ba0-106">This tutorial will walk you through hosting a Socket.IO based chat application as an Azure web app, and show you how to scale the application using [Azure Redis Cache].</span></span> <span data-ttu-id="a0ba0-107">Další informace o Socket.IO najdete v tématu <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-107">For more information on Socket.IO, see <http://socket.io/>.</span></span>

> [!NOTE]
> <span data-ttu-id="a0ba0-108">Postupy v této úloze platí pro [App Service Web Apps]; pro cloudové služby, najdete v části [sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service].</span><span class="sxs-lookup"><span data-stu-id="a0ba0-108">The procedures in this task apply to [App Service Web Apps]; for Cloud Services, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>
> 
> 

## <a name="download-the-chat-example"></a><span data-ttu-id="a0ba0-109">Stáhněte si příklad chatu</span><span class="sxs-lookup"><span data-stu-id="a0ba0-109">Download the chat example</span></span>
<span data-ttu-id="a0ba0-110">Pro tento projekt použijeme příklad chat z [úložiště Socket.IO GitHub].</span><span class="sxs-lookup"><span data-stu-id="a0ba0-110">For this project, we will use the chat example from the [Socket.IO GitHub repository].</span></span> <span data-ttu-id="a0ba0-111">Proveďte následující kroky ke stažení v příkladu a přidat jej do projektu, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-111">Perform the following steps to download the example and add it to the project you previously created.</span></span>

1. <span data-ttu-id="a0ba0-112">Stáhněte si [ZIP nebo GZ archivovat verze] Socket.IO projektu (verze 1.3.5 byl použit pro tento dokument)</span><span class="sxs-lookup"><span data-stu-id="a0ba0-112">Download a [ZIP or GZ archived release] of the Socket.IO project (version 1.3.5 was used for this document)</span></span>
2. <span data-ttu-id="a0ba0-113">Extrahujte archivní a zkopírujte **příklady\\chat** adresář do nového umístění.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-113">Extract the archive and copy the **examples\\chat** directory to a new location.</span></span> <span data-ttu-id="a0ba0-114">Například  **\\uzlu\\chat**.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-114">For example, **\\node\\chat**.</span></span>

## <a name="modify-appjs-and-install-modules"></a><span data-ttu-id="a0ba0-115">Úprava souboru app.js a instalace modulů</span><span class="sxs-lookup"><span data-stu-id="a0ba0-115">Modify app.js and install modules</span></span>
1. <span data-ttu-id="a0ba0-116">Přejmenujte **index.js** do souboru **app.js**.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-116">Rename the **index.js** file to **app.js**.</span></span> <span data-ttu-id="a0ba0-117">To umožňuje Azure a zjistit, že se jedná o aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-117">This allows Azure to detect that this is a Node.js application.</span></span>
2. <span data-ttu-id="a0ba0-118">Otevřete **app.js** soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-118">Open the **app.js** file in a text editor.</span></span> <span data-ttu-id="a0ba0-119">Změnit na řádek obsahující `var io = require('../..')(server);` jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="a0ba0-119">Change the line containing `var io = require('../..')(server);` as shown below:</span></span>
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. <span data-ttu-id="a0ba0-120">Otevřete **package.json** souboru a přidejte odkaz na socket.io pod `dependencies`, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="a0ba0-120">Open the **package.json** file and add a reference to socket.io under `dependencies`, as shown below:</span></span>
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. <span data-ttu-id="a0ba0-121">Z příkazového řádku, změňte  **\\uzlu\\chat** adresáře a použijte npm pro instalaci modulů, které vyžadují tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a0ba0-121">From the command-line, change to the **\\node\\chat** directory and use npm to install the modules required by this application:</span></span>
   
        npm install
   
    <span data-ttu-id="a0ba0-122">Moduly nainstaluje do složky s názvem **node_modules**.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-122">This will install the modules into a subfolder named **node_modules**.</span></span>

## <a name="create-an-azure-web-app"></a><span data-ttu-id="a0ba0-123">Vytvoření webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="a0ba0-123">Create an Azure Web App</span></span>
<span data-ttu-id="a0ba0-124">Postupujte podle těchto kroků k vytvoření webové aplikace Azure, povolit publikování Git a potom povolit podporu protokolu WebSocket pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-124">Follow these steps to create an Azure web app, enable Git publishing, and then enable WebSocket support for the web app.</span></span>

> [!NOTE]
> <span data-ttu-id="a0ba0-125">K dokončení tohoto kurzu potřebujete mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-125">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="a0ba0-126">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-126">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a0ba0-127">Podrobnosti najdete v článku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Bezplatná zkušební verze Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-127">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

1. <span data-ttu-id="a0ba0-128">Instalace rozhraní příkazového řádku Azure (Azure CLI) a připojte se k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-128">Install the Azure Command-Line Interface (Azure CLI) and connect to your Azure subscription.</span></span> <span data-ttu-id="a0ba0-129">V tématu [instalace a konfigurace rozhraní příkazového řádku Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a0ba0-129">See [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="a0ba0-130">Pokud je vaše první čas nastavení úložiště v Azure, budete muset vytvořit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-130">If this is your first time setting up a repository in Azure, you need to create login credentials.</span></span> <span data-ttu-id="a0ba0-131">Z příkazového řádku Azure zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a0ba0-131">From the Azure CLI, enter the following command:</span></span>
   
        azure site deployment user set [username] [password]
3. <span data-ttu-id="a0ba0-132">Změnit na  **\\node\chat** adresáře a použijte následující příkaz k vytvoření nové Azure web app a místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-132">Change to the **\\node\chat** directory and use the following command to create a new Azure web app and a local Git repository.</span></span> <span data-ttu-id="a0ba0-133">Tento příkaz vytvoří také Git vzdálené s názvem "azure".</span><span class="sxs-lookup"><span data-stu-id="a0ba0-133">This command also creates a Git remote named 'azure'.</span></span>
   
        azure site create mysitename --git
   
    <span data-ttu-id="a0ba0-134">'Mysitename, je potřeba nahradit jedinečný název pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-134">You must replace 'mysitename' with a unique name for your web app.</span></span>
4. <span data-ttu-id="a0ba0-135">Potvrďte existující soubory v místním úložišti pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="a0ba0-135">Commit the existing files to the local repository by using the following commands:</span></span>
   
        git add .
        git commit -m "Initial commit"
5. <span data-ttu-id="a0ba0-136">Push soubory do úložiště Azure Web Apps pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a0ba0-136">Push the files to the Azure Web Apps repository with the following command:</span></span>
   
        git push azure master
   
    <span data-ttu-id="a0ba0-137">Po zobrazení výzvy zadejte přihlašovací údaje z kroku 2.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-137">When prompted, enter your credentials from step 2.</span></span> <span data-ttu-id="a0ba0-138">Zobrazí se stavové zprávy a jako moduly importují na serveru.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-138">You will receive status messages as modules are imported on the server.</span></span> <span data-ttu-id="a0ba0-139">Po dokončení tohoto procesu se bude hostovat aplikaci na vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-139">Once this process has completed, the application will be hosted on your Azure web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a0ba0-140">Během instalace modulu, může dojít k chybám, se importované projekt... nebyla nalezena ".</span><span class="sxs-lookup"><span data-stu-id="a0ba0-140">During module installation, you may notice errors that 'The imported project ... was not found'.</span></span> <span data-ttu-id="a0ba0-141">Ty můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-141">These can safely be ignored.</span></span>
   > 
   > 
6. <span data-ttu-id="a0ba0-142">Socket.IO používá, které nejsou povolené ve výchozím nastavení v Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-142">Socket.IO uses WebSockets, which are not enabled by default on Azure.</span></span> <span data-ttu-id="a0ba0-143">Pokud chcete povolit webové sokety, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a0ba0-143">To enable web sockets, use the following command:</span></span>
   
        azure site set -w
   
    <span data-ttu-id="a0ba0-144">Pokud se zobrazí výzva, zadejte název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-144">If prompted, enter the name of the web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a0ba0-145">Příkaz se "azure lokality set -w", pracovní pouze s verzí 0.7.4 nebo vyšší z rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-145">The 'azure site set -w' command will work only with version 0.7.4 or higher of the Azure Command-Line Interface.</span></span> <span data-ttu-id="a0ba0-146">Můžete také povolit podpory protokolu WebSocket pomocí [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a0ba0-146">You can also enable WebSocket support using the [Azure Portal](https://portal.azure.com).</span></span>
   > 
   > <span data-ttu-id="a0ba0-147">Chcete-li povolit Websocket pomocí portálu Azure, klikněte na webovou aplikaci v okně webové aplikace, klikněte na **všechna nastavení** > **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-147">To enable WebSockets using the Azure Portal, click the web app from the Web Apps blade, click **All settings** > **Application settings**.</span></span> <span data-ttu-id="a0ba0-148">V části **webové sokety**, klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-148">Under **Web Sockets**, click **On**.</span></span> <span data-ttu-id="a0ba0-149">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-149">Then click **Save**.</span></span>
   > 
   > 
7. <span data-ttu-id="a0ba0-150">Chcete-li zobrazit webové aplikace v Azure, použijte následující příkaz Spustit webový prohlížeč a přejít na hostované webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="a0ba0-150">To view the web app on Azure, use the following command to launch your web browser and navigate to the hosted web app:</span></span>
   
        azure site browse

<span data-ttu-id="a0ba0-151">Vaše aplikace je nyní spuštěna v Azure a může přenášet zprávy chat mezi různé klienty pomocí Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-151">Your app is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

## <a name="scale-out"></a><span data-ttu-id="a0ba0-152">Horizontální navýšení kapacity</span><span class="sxs-lookup"><span data-stu-id="a0ba0-152">Scale out</span></span>
<span data-ttu-id="a0ba0-153">Socket.IO aplikace lze škálovat pomocí **adaptér** distribuovat zprávy a události mezi více instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-153">Socket.IO applications can be scaled out by using an **adapter** to distribute messages and events between multiple application instances.</span></span> <span data-ttu-id="a0ba0-154">Když jsou k dispozici několik adaptérů [socket.io redis] adaptéru lze snadno použít s funkcí Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-154">While there are several adapters available, the [socket.io-redis] adapter can be easily used with the Azure Redis Cache feature.</span></span>

> [!NOTE]
> <span data-ttu-id="a0ba0-155">Dodatečný požadavek pro škálování Socket.IO řešení je podpora pro trvalé relace.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-155">An additional requirement for scaling out a Socket.IO solution is support for sticky sessions.</span></span> <span data-ttu-id="a0ba0-156">Trvalé relace jsou povolené ve výchozím nastavení pro webové aplikace Azure prostřednictvím směrování žádostí na Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-156">Sticky sessions are enabled by default for Azure Web Apps through Azure Request Routing.</span></span> <span data-ttu-id="a0ba0-157">Další informace najdete v tématu [spřažení instanci v Azure weby].</span><span class="sxs-lookup"><span data-stu-id="a0ba0-157">For more information, see [Instance Affinity in Azure Web Sites].</span></span>
> 
> 

### <a name="create-a-redis-cache"></a><span data-ttu-id="a0ba0-158">Vytvoření mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="a0ba0-158">Create a Redis cache</span></span>
<span data-ttu-id="a0ba0-159">Proveďte kroky v [vytvoření mezipaměti ve službě Azure Redis Cache] k vytvoření nové mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-159">Perform the steps in [Create a cache in Azure Redis Cache] to create a new cache.</span></span>

> [!NOTE]
> <span data-ttu-id="a0ba0-160">Uložit **název hostitele** a **primární klíč** pro mezipaměť, jako to bude potřeba v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-160">Save the **Host name** and **Primary key** for your cache, as these will be needed in the next steps.</span></span>
> 
> 

### <a name="add-the-redis-and-socketio-redis-modules"></a><span data-ttu-id="a0ba0-161">Přidejte redis a moduly socket.io redis</span><span class="sxs-lookup"><span data-stu-id="a0ba0-161">Add the redis and socket.io-redis modules</span></span>
1. <span data-ttu-id="a0ba0-162">Z příkazového řádku, změňte  **\\uzlu\\chat** adresáře a použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-162">From a command-line, change to the **\\node\\chat** directory and use the following command.</span></span>
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > <span data-ttu-id="a0ba0-163">Verze zadané v tomto příkazu jsou použité při testování v tomto článku verze.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-163">The versions specified in this command are the versions used when testing this article.</span></span>
   > 
   > 
2. <span data-ttu-id="a0ba0-164">Změnit **app.js** souboru přidejte následující řádků ihned po`var io = require('socket.io')(server);`</span><span class="sxs-lookup"><span data-stu-id="a0ba0-164">Modify the **app.js** file to add the following lines immediately after `var io = require('socket.io')(server);`</span></span>
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    <span data-ttu-id="a0ba0-165">Nahraďte **redishostname** a **rediskey** s názvem hostitele a klíč pro vaše mezipaměť Redis.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-165">Replace **redishostname** and **rediskey** with the host name and key for your Redis cache.</span></span>
   
    <span data-ttu-id="a0ba0-166">Tato akce vytvoří publikování a přihlášení k odběru klienta do mezipaměti Redis vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-166">This will create a publish and subscribe client to the Redis cache created previously.</span></span> <span data-ttu-id="a0ba0-167">Klienti se pak používají s adaptérem ke konfiguraci Socket.IO použití Redis cache pro předávání zpráv a událostí mezi instancemi aplikace</span><span class="sxs-lookup"><span data-stu-id="a0ba0-167">The clients are then used with the adapter to configure Socket.IO to use the Redis cache for passing messages and events between instances of your application</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a0ba0-168">Když **socket.io redis** adaptér může komunikovat přímo k Redis, aktuální verze nepodporuje ověřování vyžaduje Azure Redis cache.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-168">While the **socket.io-redis** adapter can communicate directly to Redis, the current version does not support the authentication required by Azure Redis cache.</span></span> <span data-ttu-id="a0ba0-169">Tak vytvoření počátečního připojení se vytvoří pomocí **redis** modul a potom klientovi předána **socket.io redis** adaptér.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-169">So the initial connection is created using the **redis** module, then the client is passed to the **socket.io-redis** adapter.</span></span>
   > 
   > <span data-ttu-id="a0ba0-170">Zatímco Azure Redis Cache podporuje zabezpečené připojení pomocí portu 6380, moduly používané v tomto příkladu nepodporují zabezpečené připojení od 7/14/2014.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-170">While Azure Redis Cache supports secure connections using port 6380, the modules used in this example do not support secure connections as of 7/14/2014.</span></span> <span data-ttu-id="a0ba0-171">Výše uvedený kód používá výchozí, nezabezpečená port 6379.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-171">The above code uses the default, unsecure port of 6379.</span></span>
   > 
   > 
3. <span data-ttu-id="a0ba0-172">Uložit upravenou **app.js**</span><span class="sxs-lookup"><span data-stu-id="a0ba0-172">Save the modified **app.js**</span></span>

### <a name="commit-changes-and-redeploy"></a><span data-ttu-id="a0ba0-173">Potvrzení změn a znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="a0ba0-173">Commit changes and redeploy</span></span>
<span data-ttu-id="a0ba0-174">Z příkazového řádku v  **\\uzlu\\chat** adresáře, použijte následující příkazy k potvrzení změn a znovu nasaďte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-174">From the command-line in the **\\node\\chat** directory, use the following commands to commit changes and redeploy the application.</span></span>

    git add .
    git commit -m "implementing scale out"
    git push azure master

<span data-ttu-id="a0ba0-175">Jakmile se nabídne změny na server, je možné škálovat vaší lokality ve více instancích pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-175">Once the changes have been pushed to the server, you can scale your site across multiple instances by using the following command.</span></span>

    azure site scale instances --instances #

<span data-ttu-id="a0ba0-176">Kde  **#**  je počet instancí pro vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-176">Where **#** is the number of instances to create.</span></span>

<span data-ttu-id="a0ba0-177">Do webové aplikace se můžete připojit z více prohlížečů nebo počítačů, ověřte, že jsou správně odeslány zprávy pro všechny klienty.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-177">You can connect to your web app from multiple browsers or computers to verify that messages are correctly sent to all clients.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a0ba0-178">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a0ba0-178">Troubleshooting</span></span>
### <a name="connection-limits"></a><span data-ttu-id="a0ba0-179">Omezení počtu připojení</span><span class="sxs-lookup"><span data-stu-id="a0ba0-179">Connection limits</span></span>
<span data-ttu-id="a0ba0-180">Azure Web Apps je k dispozici v několika SKU, které určují prostředky, které jsou k dispozici pro váš web.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-180">Azure Web Apps is available in multiple SKUs, which determine the resources available to your site.</span></span> <span data-ttu-id="a0ba0-181">To zahrnuje počet povolených připojení protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-181">This includes the number of allowed WebSocket connections.</span></span> <span data-ttu-id="a0ba0-182">Další informace najdete v tématu [stránce s cenami webové aplikace].</span><span class="sxs-lookup"><span data-stu-id="a0ba0-182">For more information, see the [Web Apps Pricing page].</span></span>

### <a name="messages-arent-being-sent-using-websockets"></a><span data-ttu-id="a0ba0-183">Nejsou odesílány zprávy pomocí technologie WebSockets</span><span class="sxs-lookup"><span data-stu-id="a0ba0-183">Messages aren't being sent using WebSockets</span></span>
<span data-ttu-id="a0ba0-184">Pokud klientský prohlížeč zachovat návratem zpět k dlouhé dotazování místo použití technologie WebSockets, může to být způsobeno jedním z následujících.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-184">If client browsers keep falling back to long polling instead of using WebSockets, it may be because of one of the following.</span></span>

* <span data-ttu-id="a0ba0-185">**Pokuste se omezit přenosu, který je právě objekty WebSockets**</span><span class="sxs-lookup"><span data-stu-id="a0ba0-185">**Try limiting the transport to just WebSockets**</span></span>
  
    <span data-ttu-id="a0ba0-186">Aby Socket.IO na používat jako zasílání zpráv přenosový protokol Websocket musí podporovat serverem a klientem objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-186">In order for Socket.IO to use WebSockets as the messaging transport, both the server and client must support WebSockets.</span></span> <span data-ttu-id="a0ba0-187">Pokud jeden z nich není, bude Socket.IO vyjednávat jiná přenosu, například dlouhým dotazováním.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-187">If one or the other does not, Socket.IO will negotiate another transport, such as long polling.</span></span> <span data-ttu-id="a0ba0-188">Je výchozím seznamu přenosů používá Socket.IO ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-188">The default list of transports used by Socket.IO is ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span></span> <span data-ttu-id="a0ba0-189">Můžete vynutit, aby používat pouze objekty WebSockets přidáním následující kód, který **app.js** po řádku obsahující soubor `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-189">You can force it to only use WebSockets by adding the following code to the **app.js** file, after the line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > <span data-ttu-id="a0ba0-190">Všimněte si, že starší prohlížeče, které nepodporují objekty WebSockets nebudou moci připojit k webu, zatímco ve výše uvedeném kódu je aktivní, omezuje komunikaci pouze pro objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-190">Note that older browsers that do not support WebSockets will not be able to connect to the site while the above code is active, as it restricts communication to WebSockets only.</span></span>
  > 
  > 
* <span data-ttu-id="a0ba0-191">**Používat protokol SSL**</span><span class="sxs-lookup"><span data-stu-id="a0ba0-191">**Use SSL**</span></span>
  
    <span data-ttu-id="a0ba0-192">Technologie WebSockets spoléhá na některé nižší úrovně použít hlavičky protokolu HTTP, například **Upgrade** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-192">WebSockets relies on some lesser used HTTP headers, such as the **Upgrade** header.</span></span> <span data-ttu-id="a0ba0-193">Některé zprostředkující síťová zařízení, jako jsou webové proxy servery, může odebrat tyto hlavičky.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-193">Some intermediate network devices, such as web proxies, may remove these headers.</span></span> <span data-ttu-id="a0ba0-194">Chcete-li se tomuto problému vyhnout, můžete vytvořit připojení protokolu WebSocket přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-194">To avoid this problem, you can establish the WebSocket connection over SSL.</span></span>
  
    <span data-ttu-id="a0ba0-195">Snadný způsob, jak tomu je konfigurace Socket.IO na `match origin protocol`.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-195">An easy way to accomplish this is to configure Socket.IO to `match origin protocol`.</span></span> <span data-ttu-id="a0ba0-196">To se dá pokyn Socket.IO pro zabezpečení komunikace objekty WebSockets stejný jako původní žádosti protokolu HTTP nebo HTTPS pro webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-196">This instructs Socket.IO to secure WebSockets communication the same as the originating HTTP/HTTPS request for the web page.</span></span> <span data-ttu-id="a0ba0-197">Pokud prohlížeč používá adresu URL HTTPS navštíví váš web, nebude další komunikace protokolu WebSocket pomocí Socket.IO zabezpečené přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-197">If a browser uses an HTTPS URL to visit your website, subsequent WebSocket communications through Socket.IO will be secured over SSL.</span></span>
  
    <span data-ttu-id="a0ba0-198">Tento příklad, chcete-li povolit tuto konfiguraci upravit, přidejte následující kód, který **app.js** souboru po řádku obsahující `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-198">To modify this example to enable this configuration, add the following code to the **app.js** file after the line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* <span data-ttu-id="a0ba0-199">**Ověřte nastavení v souboru web.config**</span><span class="sxs-lookup"><span data-stu-id="a0ba0-199">**Verify web.config settings**</span></span>
  
    <span data-ttu-id="a0ba0-200">Službě Azure web apps, které jsou hostiteli pomocí aplikace Node.js **web.config** souboru směrovat příchozí požadavky na aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-200">Azure web apps that host Node.js applications use the **web.config** file to route incoming requests to the Node.js application.</span></span> <span data-ttu-id="a0ba0-201">Pro objekty WebSockets fungovat správně s aplikací Node.js **web.config** musí obsahovat následující položku.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-201">For WebSockets to function correctly with Node.js applications, the **web.config** must contain the following entry.</span></span>
  
        <webSocket enabled="false"/>
  
    <span data-ttu-id="a0ba0-202">To zakáže IIS Websocket modul, který obsahuje vlastní implementace technologie WebSockets a je v konfliktu s Node.js konkrétní protokolu WebSocket moduly, například Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-202">This disables the IIS WebSockets module, which includes its own implementation of WebSockets and conflicts with Node.js specific WebSocket modules such as Socket.IO.</span></span> <span data-ttu-id="a0ba0-203">Pokud tento řádek není přítomen nebo je nastaven na `true`, může to být z důvodu, který přenos protokolu WebSocket nefunguje pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-203">If this line is not present, or is set to `true`, this may be the reason that the WebSocket transport is not working for your application.</span></span>
  
    <span data-ttu-id="a0ba0-204">Za normálních okolností aplikací Node.js nezahrnují **web.config** souboru, takže weby Azure bude automaticky generovat za aplikací Node.js při jejich nasazení.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-204">Normally, Node.js applications do not include a **web.config** file, so Azure Websites will automatically generate one for Node.js applications when they are deployed.</span></span> <span data-ttu-id="a0ba0-205">Vzhledem k tomu, že tento soubor je automaticky generován na serveru, musíte použít protokol FTP nebo FTPS adresu URL pro váš web k zobrazení tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-205">Since this file is automatically generated on the server, you must use the FTP or FTPS URL for your website to view this file.</span></span> <span data-ttu-id="a0ba0-206">FTP a FTPS adresy URL můžete najít pro svůj web na portálu classic výběrem webové aplikace a potom **řídicí panel** odkaz.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-206">You can find the FTP and FTPS URLs for your site in the classic portal by selecting your web app, and then the **Dashboard** link.</span></span> <span data-ttu-id="a0ba0-207">Adresy URL se zobrazí v **rychlého přehledu** části.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-207">The URLs are displayed in the **quick glance** section.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="a0ba0-208">**Web.config** souboru se vygeneruje pouze weby Azure, pokud vaše aplikace neposkytuje jeden.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-208">The **web.config** file is only generated by Azure Websites if your application does not provide one.</span></span> <span data-ttu-id="a0ba0-209">Pokud jste zadali **web.config** soubor v kořenu projektu aplikace, použije se ve službě Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-209">If you provide a **web.config** file in the root of your application project, it will be used by Azure Web Apps.</span></span>
  > 
  > 
  
    <span data-ttu-id="a0ba0-210">Pokud položka není k dispozici nebo je nastaven na hodnotu `true`, pak byste měli vytvořit **web.config** v kořenovém aplikace Node.js a zadejte hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-210">If the entry is not present, or is set to a value of `true`, then you should create a **web.config** in the root of your Node.js application and specify a value of `false`.</span></span>  <span data-ttu-id="a0ba0-211">Pro srovnání dole je výchozí **web.config** pro aplikaci, která používá **app.js** jako vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-211">For reference, the below is a default **web.config** for an application that uses **app.js** as the entry point.</span></span>
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    <span data-ttu-id="a0ba0-212">Pokud vaše aplikace používá vstupní bod jiné než **app.js**, musí nahraďte všechny výskyty **app.js** s správný vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-212">If your application uses an entry point other than **app.js**, you must replace all occurrences of **app.js** with the correct entry point.</span></span> <span data-ttu-id="a0ba0-213">Například nahrazení **app.js** s **server.js**.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-213">For example, replacing **app.js** with **server.js**.</span></span>

> [!NOTE]
> <span data-ttu-id="a0ba0-214">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service], kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-214">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="a0ba0-215">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-215">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a0ba0-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0ba0-216">Next steps</span></span>
<span data-ttu-id="a0ba0-217">V tomto kurzu jste zjistili, jak k vytvoření chatovací aplikace hostované na webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-217">In this tutorial you learned how to create a chat application hosted in an Azure web app.</span></span> <span data-ttu-id="a0ba0-218">Můžete také hostovat tuto aplikaci jako cloudová služba Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ba0-218">You can also host this application as an Azure Cloud Service.</span></span> <span data-ttu-id="a0ba0-219">Pokyny o tom, jak to provést, najdete v části [sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service].</span><span class="sxs-lookup"><span data-stu-id="a0ba0-219">For steps on how to accomplish this, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>

<span data-ttu-id="a0ba0-220">Další informace naleznete také [středisku pro vývojáře Node.js].</span><span class="sxs-lookup"><span data-stu-id="a0ba0-220">For more information, see also the [Node.js Developer Center].</span></span>

## <a name="whats-changed"></a><span data-ttu-id="a0ba0-221">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="a0ba0-221">What's changed</span></span>
* <span data-ttu-id="a0ba0-222">Průvodce změnou z webů na službu App Service najdete v tématu: [Azure App Service a její vliv na stávající služby Azure].</span><span class="sxs-lookup"><span data-stu-id="a0ba0-222">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services].</span></span>

<!-- URL List -->

<span data-ttu-id="a0ba0-223">[Azure Redis Cache]: /documentation/services/redis-cache/</span><span class="sxs-lookup"><span data-stu-id="a0ba0-223">[Azure Redis Cache]: /documentation/services/redis-cache/</span></span>
<span data-ttu-id="a0ba0-224">[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714</span><span class="sxs-lookup"><span data-stu-id="a0ba0-224">[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714</span></span>
<span data-ttu-id="a0ba0-225">[stránce s cenami webové aplikace]: http://go.microsoft.com/fwlink/?LinkId=511643</span><span class="sxs-lookup"><span data-stu-id="a0ba0-225">[Web Apps Pricing page]: http://go.microsoft.com/fwlink/?LinkId=511643</span></span>
<span data-ttu-id="a0ba0-226">[sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md</span><span class="sxs-lookup"><span data-stu-id="a0ba0-226">[Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md</span></span>
[Install and Configure the Azure CLI]: ../cli-install-nodejs.md
<span data-ttu-id="a0ba0-227">[Azure App Service a její vliv na stávající služby Azure]: http://go.microsoft.com/fwlink/?LinkId=529714</span><span class="sxs-lookup"><span data-stu-id="a0ba0-227">[Azure App Service and Its Impact on Existing Azure Services]: http://go.microsoft.com/fwlink/?LinkId=529714</span></span>
<span data-ttu-id="a0ba0-228">[středisku pro vývojáře Node.js]: /develop/nodejs/</span><span class="sxs-lookup"><span data-stu-id="a0ba0-228">[Node.js Developer Center]: /develop/nodejs/</span></span>
<span data-ttu-id="a0ba0-229">[možnosti vyzkoušet si App Service]: https://azure.microsoft.com/try/app-service/</span><span class="sxs-lookup"><span data-stu-id="a0ba0-229">[Try App Service]: https://azure.microsoft.com/try/app-service/</span></span>
<span data-ttu-id="a0ba0-230">[spřažení instanci v Azure weby]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/</span><span class="sxs-lookup"><span data-stu-id="a0ba0-230">[Instance Affinity in Azure Web Sites]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/</span></span>
<span data-ttu-id="a0ba0-231">[vytvoření mezipaměti ve službě Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md</span><span class="sxs-lookup"><span data-stu-id="a0ba0-231">[Create a cache in Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md</span></span>

<span data-ttu-id="a0ba0-232">[socket.io redis]: https://github.com/socketio/socket.io-redis</span><span class="sxs-lookup"><span data-stu-id="a0ba0-232">[socket.io-redis]: https://github.com/socketio/socket.io-redis</span></span>
<span data-ttu-id="a0ba0-233">[úložiště Socket.IO GitHub]: https://github.com/socketio/socket.io</span><span class="sxs-lookup"><span data-stu-id="a0ba0-233">[Socket.IO GitHub repository]: https://github.com/socketio/socket.io</span></span>
<span data-ttu-id="a0ba0-234">[ZIP nebo GZ archivovat verze]: https://github.com/socketio/socket.io/releases</span><span class="sxs-lookup"><span data-stu-id="a0ba0-234">[ZIP or GZ archived release]: https://github.com/socketio/socket.io/releases</span></span>

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
