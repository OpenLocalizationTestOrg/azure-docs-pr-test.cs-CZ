---
title: "aaaCreate chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service"
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
ms.openlocfilehash: 3bd7867ccc297dc0a21c7a00cc9db06358877f5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a><span data-ttu-id="93876-103">Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="93876-103">Create a Node.js chat application with Socket.IO in Azure App Service</span></span>
<span data-ttu-id="93876-104">Socket.IO poskytuje v reálném čase komunikaci mezi serverem node.js a klienty, kteří používají objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="93876-104">Socket.IO provides real-time communication between your node.js server and clients using WebSockets.</span></span> <span data-ttu-id="93876-105">Podporuje také záložní tooother přenosů (například dlouhým dotazováním), které pracují s starší prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="93876-105">It also supports fallback tooother transports (such as long polling) that work with older browsers.</span></span> <span data-ttu-id="93876-106">Tento kurz vás provede procesem hostování Socket.IO na základě chatovací aplikace jako webové aplikace Azure a ukazují, jak tooscale hello aplikace pomocí [Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="93876-106">This tutorial will walk you through hosting a Socket.IO based chat application as an Azure web app, and show you how tooscale hello application using [Azure Redis Cache].</span></span> <span data-ttu-id="93876-107">Další informace o Socket.IO najdete v tématu <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="93876-107">For more information on Socket.IO, see <http://socket.io/>.</span></span>

> [!NOTE]
> <span data-ttu-id="93876-108">Hello postupy v této úloze platí příliš[App Service Web Apps]; pro cloudové služby, najdete v části [sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service].</span><span class="sxs-lookup"><span data-stu-id="93876-108">hello procedures in this task apply too[App Service Web Apps]; for Cloud Services, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>
> 
> 

## <a name="download-hello-chat-example"></a><span data-ttu-id="93876-109">Stáhněte si příklad chat hello</span><span class="sxs-lookup"><span data-stu-id="93876-109">Download hello chat example</span></span>
<span data-ttu-id="93876-110">Pro tento projekt použijeme hello chat příklad z hello [úložiště Socket.IO GitHub].</span><span class="sxs-lookup"><span data-stu-id="93876-110">For this project, we will use hello chat example from hello [Socket.IO GitHub repository].</span></span> <span data-ttu-id="93876-111">Proveďte následující kroky toodownload hello ukázka hello a přidejte ho toohello projektu, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="93876-111">Perform hello following steps toodownload hello example and add it toohello project you previously created.</span></span>

1. <span data-ttu-id="93876-112">Stáhněte si [ZIP nebo GZ archivovat verze] hello Socket.IO projektu (verze 1.3.5 byl použit pro tento dokument)</span><span class="sxs-lookup"><span data-stu-id="93876-112">Download a [ZIP or GZ archived release] of hello Socket.IO project (version 1.3.5 was used for this document)</span></span>
2. <span data-ttu-id="93876-113">Extrahování hello archivu a zkopírujte hello **příklady\\chat** directory tooa nové umístění.</span><span class="sxs-lookup"><span data-stu-id="93876-113">Extract hello archive and copy hello **examples\\chat** directory tooa new location.</span></span> <span data-ttu-id="93876-114">Například  **\\uzlu\\chat**.</span><span class="sxs-lookup"><span data-stu-id="93876-114">For example, **\\node\\chat**.</span></span>

## <a name="modify-appjs-and-install-modules"></a><span data-ttu-id="93876-115">Úprava souboru app.js a instalace modulů</span><span class="sxs-lookup"><span data-stu-id="93876-115">Modify app.js and install modules</span></span>
1. <span data-ttu-id="93876-116">Přejmenujte hello **index.js** souboru příliš**app.js**.</span><span class="sxs-lookup"><span data-stu-id="93876-116">Rename hello **index.js** file too**app.js**.</span></span> <span data-ttu-id="93876-117">To umožňuje Azure toodetect, že se jedná o aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="93876-117">This allows Azure toodetect that this is a Node.js application.</span></span>
2. <span data-ttu-id="93876-118">Otevřete hello **app.js** soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="93876-118">Open hello **app.js** file in a text editor.</span></span> <span data-ttu-id="93876-119">Změna hello řádek obsahující `var io = require('../..')(server);` jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="93876-119">Change hello line containing `var io = require('../..')(server);` as shown below:</span></span>
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. <span data-ttu-id="93876-120">Otevřete hello **package.json** souboru a přidejte odkaz toosocket.io pod `dependencies`, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="93876-120">Open hello **package.json** file and add a reference toosocket.io under `dependencies`, as shown below:</span></span>
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. <span data-ttu-id="93876-121">Z příkazového řádku hello, změnit toohello  **\\uzlu\\chat** adresáře a použijte tooinstall hello moduly npm vyžaduje této aplikace:</span><span class="sxs-lookup"><span data-stu-id="93876-121">From hello command-line, change toohello **\\node\\chat** directory and use npm tooinstall hello modules required by this application:</span></span>
   
        npm install
   
    <span data-ttu-id="93876-122">Moduly hello nainstaluje do složky s názvem **node_modules**.</span><span class="sxs-lookup"><span data-stu-id="93876-122">This will install hello modules into a subfolder named **node_modules**.</span></span>

## <a name="create-an-azure-web-app"></a><span data-ttu-id="93876-123">Vytvoření webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="93876-123">Create an Azure Web App</span></span>
<span data-ttu-id="93876-124">Postupujte podle těchto kroků toocreate webové aplikace Azure, povolit publikování Git a potom povolit podporu protokolu WebSocket pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93876-124">Follow these steps toocreate an Azure web app, enable Git publishing, and then enable WebSocket support for hello web app.</span></span>

> [!NOTE]
> <span data-ttu-id="93876-125">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="93876-125">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="93876-126">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="93876-126">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="93876-127">Podrobnosti najdete v článku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Bezplatná zkušební verze Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="93876-127">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

1. <span data-ttu-id="93876-128">Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI) a připojit tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="93876-128">Install hello Azure Command-Line Interface (Azure CLI) and connect tooyour Azure subscription.</span></span> <span data-ttu-id="93876-129">V tématu [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="93876-129">See [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="93876-130">Pokud je vaše první čas nastavení úložiště v Azure, musíte toocreate přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="93876-130">If this is your first time setting up a repository in Azure, you need toocreate login credentials.</span></span> <span data-ttu-id="93876-131">Z hello rozhraní příkazového řádku Azure zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="93876-131">From hello Azure CLI, enter hello following command:</span></span>
   
        azure site deployment user set [username] [password]
3. <span data-ttu-id="93876-132">Změnit toohello  **\\node\chat** adresář a použití hello následující příkaz toocreate nové webové aplikace Azure a místní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="93876-132">Change toohello **\\node\chat** directory and use hello following command toocreate a new Azure web app and a local Git repository.</span></span> <span data-ttu-id="93876-133">Tento příkaz vytvoří také Git vzdálené s názvem "azure".</span><span class="sxs-lookup"><span data-stu-id="93876-133">This command also creates a Git remote named 'azure'.</span></span>
   
        azure site create mysitename --git
   
    <span data-ttu-id="93876-134">'Mysitename, je potřeba nahradit jedinečný název pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93876-134">You must replace 'mysitename' with a unique name for your web app.</span></span>
4. <span data-ttu-id="93876-135">Potvrďte hello existující soubory toohello místní úložiště pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="93876-135">Commit hello existing files toohello local repository by using hello following commands:</span></span>
   
        git add .
        git commit -m "Initial commit"
5. <span data-ttu-id="93876-136">Push hello soubory toohello Azure Web Apps úložiště s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="93876-136">Push hello files toohello Azure Web Apps repository with hello following command:</span></span>
   
        git push azure master
   
    <span data-ttu-id="93876-137">Po zobrazení výzvy zadejte přihlašovací údaje z kroku 2.</span><span class="sxs-lookup"><span data-stu-id="93876-137">When prompted, enter your credentials from step 2.</span></span> <span data-ttu-id="93876-138">Zobrazí se stavové zprávy a jako moduly importují na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="93876-138">You will receive status messages as modules are imported on hello server.</span></span> <span data-ttu-id="93876-139">Po dokončení tohoto procesu se bude hostovat aplikace hello na vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="93876-139">Once this process has completed, hello application will be hosted on your Azure web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="93876-140">Během instalace modulu, může dojít k chybám, "hello importované projekt... nebyla nalezena".</span><span class="sxs-lookup"><span data-stu-id="93876-140">During module installation, you may notice errors that 'hello imported project ... was not found'.</span></span> <span data-ttu-id="93876-141">Ty můžete bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="93876-141">These can safely be ignored.</span></span>
   > 
   > 
6. <span data-ttu-id="93876-142">Socket.IO používá, které nejsou povolené ve výchozím nastavení v Azure.</span><span class="sxs-lookup"><span data-stu-id="93876-142">Socket.IO uses WebSockets, which are not enabled by default on Azure.</span></span> <span data-ttu-id="93876-143">tooenable webové sokety, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="93876-143">tooenable web sockets, use hello following command:</span></span>
   
        azure site set -w
   
    <span data-ttu-id="93876-144">Pokud se zobrazí výzva, zadejte název hello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="93876-144">If prompted, enter hello name of hello web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="93876-145">Hello příkaz bude "azure lokality set -w" pracovní pouze s verzí 0.7.4 nebo vyšší hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="93876-145">hello 'azure site set -w' command will work only with version 0.7.4 or higher of hello Azure Command-Line Interface.</span></span> <span data-ttu-id="93876-146">Můžete také povolit podporu protokolu WebSocket pomocí hello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="93876-146">You can also enable WebSocket support using hello [Azure Portal](https://portal.azure.com).</span></span>
   > 
   > <span data-ttu-id="93876-147">pomocí technologie WebSockets tooenable hello portálu Azure, klikněte na tlačítko hello webové aplikace v okně webové aplikace hello, klikněte na **všechna nastavení** > **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="93876-147">tooenable WebSockets using hello Azure Portal, click hello web app from hello Web Apps blade, click **All settings** > **Application settings**.</span></span> <span data-ttu-id="93876-148">V části **webové sokety**, klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="93876-148">Under **Web Sockets**, click **On**.</span></span> <span data-ttu-id="93876-149">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="93876-149">Then click **Save**.</span></span>
   > 
   > 
7. <span data-ttu-id="93876-150">tooview hello webové aplikace na Azure, použijte hello následující příkaz toolaunch webový prohlížeč a přejděte toohello hostované webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="93876-150">tooview hello web app on Azure, use hello following command toolaunch your web browser and navigate toohello hosted web app:</span></span>
   
        azure site browse

<span data-ttu-id="93876-151">Vaše aplikace je nyní spuštěna v Azure a může přenášet zprávy chat mezi různé klienty pomocí Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="93876-151">Your app is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

## <a name="scale-out"></a><span data-ttu-id="93876-152">Horizontální navýšení kapacity</span><span class="sxs-lookup"><span data-stu-id="93876-152">Scale out</span></span>
<span data-ttu-id="93876-153">Socket.IO aplikace lze škálovat pomocí **adaptér** toodistribute zprávy a události mezi více instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="93876-153">Socket.IO applications can be scaled out by using an **adapter** toodistribute messages and events between multiple application instances.</span></span> <span data-ttu-id="93876-154">Když jsou k dispozici několik adaptérů, hello [socket.io redis] adaptéru lze snadno použít s Azure Redis Cache funkce hello.</span><span class="sxs-lookup"><span data-stu-id="93876-154">While there are several adapters available, hello [socket.io-redis] adapter can be easily used with hello Azure Redis Cache feature.</span></span>

> [!NOTE]
> <span data-ttu-id="93876-155">Dodatečný požadavek pro škálování Socket.IO řešení je podpora pro trvalé relace.</span><span class="sxs-lookup"><span data-stu-id="93876-155">An additional requirement for scaling out a Socket.IO solution is support for sticky sessions.</span></span> <span data-ttu-id="93876-156">Trvalé relace jsou povolené ve výchozím nastavení pro webové aplikace Azure prostřednictvím směrování žádostí na Azure.</span><span class="sxs-lookup"><span data-stu-id="93876-156">Sticky sessions are enabled by default for Azure Web Apps through Azure Request Routing.</span></span> <span data-ttu-id="93876-157">Další informace najdete v tématu [spřažení instanci v Azure weby].</span><span class="sxs-lookup"><span data-stu-id="93876-157">For more information, see [Instance Affinity in Azure Web Sites].</span></span>
> 
> 

### <a name="create-a-redis-cache"></a><span data-ttu-id="93876-158">Vytvoření mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="93876-158">Create a Redis cache</span></span>
<span data-ttu-id="93876-159">Proveďte kroky hello v [vytvoření mezipaměti ve službě Azure Redis Cache] toocreate novou mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="93876-159">Perform hello steps in [Create a cache in Azure Redis Cache] toocreate a new cache.</span></span>

> [!NOTE]
> <span data-ttu-id="93876-160">Uložit hello **název hostitele** a **primární klíč** pro mezipaměť, jak to bude potřeba v hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="93876-160">Save hello **Host name** and **Primary key** for your cache, as these will be needed in hello next steps.</span></span>
> 
> 

### <a name="add-hello-redis-and-socketio-redis-modules"></a><span data-ttu-id="93876-161">Přidat hello redis a moduly socket.io redis</span><span class="sxs-lookup"><span data-stu-id="93876-161">Add hello redis and socket.io-redis modules</span></span>
1. <span data-ttu-id="93876-162">Z příkazového řádku, změňte toohello  **\\uzlu\\chat** hello adresáře a použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="93876-162">From a command-line, change toohello **\\node\\chat** directory and use hello following command.</span></span>
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > <span data-ttu-id="93876-163">Hello verze uvedené v tomto příkazu jsou použité při testování v tomto článku verze hello.</span><span class="sxs-lookup"><span data-stu-id="93876-163">hello versions specified in this command are hello versions used when testing this article.</span></span>
   > 
   > 
2. <span data-ttu-id="93876-164">Upravit hello **app.js** souboru tooadd hello následující řádků ihned po`var io = require('socket.io')(server);`</span><span class="sxs-lookup"><span data-stu-id="93876-164">Modify hello **app.js** file tooadd hello following lines immediately after `var io = require('socket.io')(server);`</span></span>
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    <span data-ttu-id="93876-165">Nahraďte **redishostname** a **rediskey** s názvem hostitele hello a klíč pro vaše mezipaměť Redis.</span><span class="sxs-lookup"><span data-stu-id="93876-165">Replace **redishostname** and **rediskey** with hello host name and key for your Redis cache.</span></span>
   
    <span data-ttu-id="93876-166">Tato akce vytvoří publikování a přihlášení k odběru toohello klienta Redis cache, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="93876-166">This will create a publish and subscribe client toohello Redis cache created previously.</span></span> <span data-ttu-id="93876-167">Hello klientům jsou pak použít společně s hello adaptér tooconfigure Socket.IO toouse hello Redis cache pro předávání zpráv a událostí mezi instancemi aplikace</span><span class="sxs-lookup"><span data-stu-id="93876-167">hello clients are then used with hello adapter tooconfigure Socket.IO toouse hello Redis cache for passing messages and events between instances of your application</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="93876-168">Při hello **socket.io redis** adaptér může komunikovat přímo tooRedis, aktuální verze hello nepodporuje hello ověřování, který Azure Redis cache.</span><span class="sxs-lookup"><span data-stu-id="93876-168">While hello **socket.io-redis** adapter can communicate directly tooRedis, hello current version does not support hello authentication required by Azure Redis cache.</span></span> <span data-ttu-id="93876-169">Takže hello počátečního připojení se vytvoří pomocí hello **redis** modulu, pak klient hello je předán toohello **socket.io redis** adaptéru.</span><span class="sxs-lookup"><span data-stu-id="93876-169">So hello initial connection is created using hello **redis** module, then hello client is passed toohello **socket.io-redis** adapter.</span></span>
   > 
   > <span data-ttu-id="93876-170">Zatímco Azure Redis Cache podporuje zabezpečené připojení pomocí portu 6380, hello moduly používané v tomto příkladu nepodporují zabezpečené připojení od 7/14/2014.</span><span class="sxs-lookup"><span data-stu-id="93876-170">While Azure Redis Cache supports secure connections using port 6380, hello modules used in this example do not support secure connections as of 7/14/2014.</span></span> <span data-ttu-id="93876-171">Hello výše kód používá hello výchozím nezabezpečená port 6379.</span><span class="sxs-lookup"><span data-stu-id="93876-171">hello above code uses hello default, unsecure port of 6379.</span></span>
   > 
   > 
3. <span data-ttu-id="93876-172">Uložit hello upravit **app.js**</span><span class="sxs-lookup"><span data-stu-id="93876-172">Save hello modified **app.js**</span></span>

### <a name="commit-changes-and-redeploy"></a><span data-ttu-id="93876-173">Potvrzení změn a znovu nasaďte</span><span class="sxs-lookup"><span data-stu-id="93876-173">Commit changes and redeploy</span></span>
<span data-ttu-id="93876-174">Z příkazového řádku v hello hello  **\\uzlu\\chat** adresáře, použijte následující příkazy toocommit změny hello a znovu nasaďte aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="93876-174">From hello command-line in hello **\\node\\chat** directory, use hello following commands toocommit changes and redeploy hello application.</span></span>

    git add .
    git commit -m "implementing scale out"
    git push azure master

<span data-ttu-id="93876-175">Jakmile se nabídne hello změny toohello serveru, je možné škálovat vaší lokality ve více instancích pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="93876-175">Once hello changes have been pushed toohello server, you can scale your site across multiple instances by using hello following command.</span></span>

    azure site scale instances --instances #

<span data-ttu-id="93876-176">Kde  **#**  je hello počet instancí toocreate.</span><span class="sxs-lookup"><span data-stu-id="93876-176">Where **#** is hello number of instances toocreate.</span></span>

<span data-ttu-id="93876-177">Tooyour webové aplikace se můžete připojit z více tooverify prohlížeče nebo počítače, klienti tooall správně odesílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="93876-177">You can connect tooyour web app from multiple browsers or computers tooverify that messages are correctly sent tooall clients.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="93876-178">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="93876-178">Troubleshooting</span></span>
### <a name="connection-limits"></a><span data-ttu-id="93876-179">Omezení počtu připojení</span><span class="sxs-lookup"><span data-stu-id="93876-179">Connection limits</span></span>
<span data-ttu-id="93876-180">Azure Web Apps je k dispozici v několika SKU, které určují hello prostředky k dispozici tooyour lokality.</span><span class="sxs-lookup"><span data-stu-id="93876-180">Azure Web Apps is available in multiple SKUs, which determine hello resources available tooyour site.</span></span> <span data-ttu-id="93876-181">To zahrnuje hello počet povolených připojení protokolu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="93876-181">This includes hello number of allowed WebSocket connections.</span></span> <span data-ttu-id="93876-182">Další informace najdete v tématu hello [stránce s cenami webové aplikace].</span><span class="sxs-lookup"><span data-stu-id="93876-182">For more information, see hello [Web Apps Pricing page].</span></span>

### <a name="messages-arent-being-sent-using-websockets"></a><span data-ttu-id="93876-183">Nejsou odesílány zprávy pomocí technologie WebSockets</span><span class="sxs-lookup"><span data-stu-id="93876-183">Messages aren't being sent using WebSockets</span></span>
<span data-ttu-id="93876-184">Pokud klientský prohlížeč zachovat návratem zpět toolong dotazování místo použití technologie WebSockets, může být způsobeno jedním z následujících hello.</span><span class="sxs-lookup"><span data-stu-id="93876-184">If client browsers keep falling back toolong polling instead of using WebSockets, it may be because of one of hello following.</span></span>

* <span data-ttu-id="93876-185">**Pokuste se omezit přenos toojust hello objekty WebSockets**</span><span class="sxs-lookup"><span data-stu-id="93876-185">**Try limiting hello transport toojust WebSockets**</span></span>
  
    <span data-ttu-id="93876-186">Aby Socket.IO toouse Websocket jako hello zasílání zpráv na úrovni přenosu musí podporovat hello serverů i klientů objekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="93876-186">In order for Socket.IO toouse WebSockets as hello messaging transport, both hello server and client must support WebSockets.</span></span> <span data-ttu-id="93876-187">Pokud jeden nebo hello jiných není, bude Socket.IO vyjednávat jiná přenosu, například dlouhým dotazováním.</span><span class="sxs-lookup"><span data-stu-id="93876-187">If one or hello other does not, Socket.IO will negotiate another transport, such as long polling.</span></span> <span data-ttu-id="93876-188">je výchozím seznamu Hello přenosů používá Socket.IO ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span><span class="sxs-lookup"><span data-stu-id="93876-188">hello default list of transports used by Socket.IO is ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span></span> <span data-ttu-id="93876-189">Můžete vynutit jeho použití tooonly objekty WebSockets přidáním hello následující kód toohello **app.js** po hello řádek obsahující soubor `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="93876-189">You can force it tooonly use WebSockets by adding hello following code toohello **app.js** file, after hello line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > <span data-ttu-id="93876-190">Poznámka: starší prohlížeče, které nepodporují objekty WebSockets nebude možné tooconnect toohello lokality při hello výše kód je aktivní, omezuje jenom tooWebSockets komunikace.</span><span class="sxs-lookup"><span data-stu-id="93876-190">Note that older browsers that do not support WebSockets will not be able tooconnect toohello site while hello above code is active, as it restricts communication tooWebSockets only.</span></span>
  > 
  > 
* <span data-ttu-id="93876-191">**Používat protokol SSL**</span><span class="sxs-lookup"><span data-stu-id="93876-191">**Use SSL**</span></span>
  
    <span data-ttu-id="93876-192">Technologie WebSockets spoléhá na některé nižší úrovně použít hlavičky protokolu HTTP, jako je například hello **Upgrade** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="93876-192">WebSockets relies on some lesser used HTTP headers, such as hello **Upgrade** header.</span></span> <span data-ttu-id="93876-193">Některé zprostředkující síťová zařízení, jako jsou webové proxy servery, může odebrat tyto hlavičky.</span><span class="sxs-lookup"><span data-stu-id="93876-193">Some intermediate network devices, such as web proxies, may remove these headers.</span></span> <span data-ttu-id="93876-194">tooavoid tento problém, můžete vytvořit připojení protokolu WebSocket hello přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="93876-194">tooavoid this problem, you can establish hello WebSocket connection over SSL.</span></span>
  
    <span data-ttu-id="93876-195">Snadný způsob tooaccomplish to tooconfigure Socket.IO je příliš`match origin protocol`.</span><span class="sxs-lookup"><span data-stu-id="93876-195">An easy way tooaccomplish this is tooconfigure Socket.IO too`match origin protocol`.</span></span> <span data-ttu-id="93876-196">To se dá pokyn Socket.IO toosecure Websocket komunikace hello stejný jako hello pocházející protokolu HTTP nebo HTTPS žádosti pro webovou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="93876-196">This instructs Socket.IO toosecure WebSockets communication hello same as hello originating HTTP/HTTPS request for hello web page.</span></span> <span data-ttu-id="93876-197">Pokud prohlížeč používá toovisit adresy URL HTTPS vašeho webu, nebude další komunikace protokolu WebSocket pomocí Socket.IO zabezpečené přes protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="93876-197">If a browser uses an HTTPS URL toovisit your website, subsequent WebSocket communications through Socket.IO will be secured over SSL.</span></span>
  
    <span data-ttu-id="93876-198">toomodify tento příklad tooenable tuto konfiguraci, přidejte následující kód toohello hello **app.js** souboru po hello řádek obsahující `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="93876-198">toomodify this example tooenable this configuration, add hello following code toohello **app.js** file after hello line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* <span data-ttu-id="93876-199">**Ověřte nastavení v souboru web.config**</span><span class="sxs-lookup"><span data-stu-id="93876-199">**Verify web.config settings**</span></span>
  
    <span data-ttu-id="93876-200">Použití Azure webové aplikace, které jsou hostiteli aplikací Node.js hello **web.config** souboru tooroute příchozí požadavky aplikace Node.js toohello.</span><span class="sxs-lookup"><span data-stu-id="93876-200">Azure web apps that host Node.js applications use hello **web.config** file tooroute incoming requests toohello Node.js application.</span></span> <span data-ttu-id="93876-201">Pro objekty WebSockets toofunction správně s aplikacemi Node.js, hello **web.config** musí obsahovat následující položku hello.</span><span class="sxs-lookup"><span data-stu-id="93876-201">For WebSockets toofunction correctly with Node.js applications, hello **web.config** must contain hello following entry.</span></span>
  
        <webSocket enabled="false"/>
  
    <span data-ttu-id="93876-202">To zakáže hello IIS Websocket modul, který obsahuje vlastní implementace technologie WebSockets a je v konfliktu s Node.js konkrétní protokolu WebSocket moduly, například Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="93876-202">This disables hello IIS WebSockets module, which includes its own implementation of WebSockets and conflicts with Node.js specific WebSocket modules such as Socket.IO.</span></span> <span data-ttu-id="93876-203">Pokud tento řádek není přítomen nebo je nastaven příliš`true`, může to být hello důvod, který přenos protokolu WebSocket hello nefunguje pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93876-203">If this line is not present, or is set too`true`, this may be hello reason that hello WebSocket transport is not working for your application.</span></span>
  
    <span data-ttu-id="93876-204">Za normálních okolností aplikací Node.js nezahrnují **web.config** souboru, takže weby Azure bude automaticky generovat za aplikací Node.js při jejich nasazení.</span><span class="sxs-lookup"><span data-stu-id="93876-204">Normally, Node.js applications do not include a **web.config** file, so Azure Websites will automatically generate one for Node.js applications when they are deployed.</span></span> <span data-ttu-id="93876-205">Vzhledem k tomu, že tento soubor je automaticky generovaný serverem hello, musíte použít hello FTP nebo FTPS adresa URL pro váš web tooview tento soubor.</span><span class="sxs-lookup"><span data-stu-id="93876-205">Since this file is automatically generated on hello server, you must use hello FTP or FTPS URL for your website tooview this file.</span></span> <span data-ttu-id="93876-206">Můžete najít hello FTP a FTPS adresy URL pro svůj web portálu classic hello výběrem webové aplikace a pak hello **řídicí panel** odkaz.</span><span class="sxs-lookup"><span data-stu-id="93876-206">You can find hello FTP and FTPS URLs for your site in hello classic portal by selecting your web app, and then hello **Dashboard** link.</span></span> <span data-ttu-id="93876-207">Hello adresy URL se zobrazí v hello **rychlého přehledu** části.</span><span class="sxs-lookup"><span data-stu-id="93876-207">hello URLs are displayed in hello **quick glance** section.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="93876-208">Hello **web.config** souboru se vygeneruje pouze weby Azure, pokud vaše aplikace neposkytuje jeden.</span><span class="sxs-lookup"><span data-stu-id="93876-208">hello **web.config** file is only generated by Azure Websites if your application does not provide one.</span></span> <span data-ttu-id="93876-209">Pokud jste zadali **web.config** soubor v kořenovém hello projektu aplikace, použije se ve službě Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="93876-209">If you provide a **web.config** file in hello root of your application project, it will be used by Azure Web Apps.</span></span>
  > 
  > 
  
    <span data-ttu-id="93876-210">Pokud není k dispozici položka hello, nebo je nastavena hodnota tooa `true`, pak byste měli vytvořit **web.config** v kořenovém adresáři aplikace Node.js hello a zadejte hodnotu `false`.</span><span class="sxs-lookup"><span data-stu-id="93876-210">If hello entry is not present, or is set tooa value of `true`, then you should create a **web.config** in hello root of your Node.js application and specify a value of `false`.</span></span>  <span data-ttu-id="93876-211">Pro referenci hello níže je výchozí **web.config** pro aplikaci, která používá **app.js** jako hello vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="93876-211">For reference, hello below is a default **web.config** for an application that uses **app.js** as hello entry point.</span></span>
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used toorun node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that hello server.js file is a node.js web app toobe handled by hello iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether hello incoming URL matches a physical file in hello /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped toohello node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using hello following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes toorestart hello server
                * node_env: will be propagated toonode as NODE_ENV environment variable
                * debuggingEnabled - controls whether hello built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    <span data-ttu-id="93876-212">Pokud vaše aplikace používá vstupní bod jiné než **app.js**, musí nahraďte všechny výskyty **app.js** s hello opravte vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="93876-212">If your application uses an entry point other than **app.js**, you must replace all occurrences of **app.js** with hello correct entry point.</span></span> <span data-ttu-id="93876-213">Například nahrazení **app.js** s **server.js**.</span><span class="sxs-lookup"><span data-stu-id="93876-213">For example, replacing **app.js** with **server.js**.</span></span>

> [!NOTE]
> <span data-ttu-id="93876-214">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service], kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="93876-214">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="93876-215">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="93876-215">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="93876-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93876-216">Next steps</span></span>
<span data-ttu-id="93876-217">V tomto kurzu jste se dozvěděli, jak toocreate chatovací aplikace hostované ve webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="93876-217">In this tutorial you learned how toocreate a chat application hosted in an Azure web app.</span></span> <span data-ttu-id="93876-218">Můžete také hostovat tuto aplikaci jako cloudová služba Azure.</span><span class="sxs-lookup"><span data-stu-id="93876-218">You can also host this application as an Azure Cloud Service.</span></span> <span data-ttu-id="93876-219">Postup pro tooaccomplish, najdete v tématu [sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service].</span><span class="sxs-lookup"><span data-stu-id="93876-219">For steps on how tooaccomplish this, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>

<span data-ttu-id="93876-220">Další informace najdete v tématu taky hello [středisku pro vývojáře Node.js].</span><span class="sxs-lookup"><span data-stu-id="93876-220">For more information, see also hello [Node.js Developer Center].</span></span>

## <a name="whats-changed"></a><span data-ttu-id="93876-221">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="93876-221">What's changed</span></span>
* <span data-ttu-id="93876-222">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure].</span><span class="sxs-lookup"><span data-stu-id="93876-222">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services].</span></span>

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[stránce s cenami webové aplikace]: http://go.microsoft.com/fwlink/?LinkId=511643
[sestavení aplikace Chat v Node.js se Socket.IO ve službě Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure hello Azure CLI]: ../cli-install-nodejs.md
[Azure App Service a její vliv na stávající služby Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[středisku pro vývojáře Node.js]: /develop/nodejs/
[vyzkoušet službu App Service]: https://azure.microsoft.com/try/app-service/
[spřažení instanci v Azure weby]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[vytvoření mezipaměti ve službě Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io redis]: https://github.com/socketio/socket.io-redis
[úložiště Socket.IO GitHub]: https://github.com/socketio/socket.io
[ZIP nebo GZ archivovat verze]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
