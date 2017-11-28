---
title: "aaaNode.js aplikace pomocí Socket.io | Microsoft Docs"
description: "Zjistěte, jak se socket.io toouse v aplikaci node.js hostované v Azure."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 7f9435e0-7732-4aa1-a4df-ea0e894b847f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 47c6c4a748938959315b880340f41f31faab4ea9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a><span data-ttu-id="04163-103">Sestavení aplikace Chat v Node.js se Socket.IO na cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="04163-103">Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service</span></span>
<span data-ttu-id="04163-104">Socket.IO poskytuje v reálném čase komunikace mezi mezi node.js serverem a klienty.</span><span class="sxs-lookup"><span data-stu-id="04163-104">Socket.IO provides realtime communication between between your node.js server and clients.</span></span> <span data-ttu-id="04163-105">Tento kurz vás provede hostování soketu. Vstupně-výstupních operací na základě chatovací aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="04163-105">This tutorial will walk you through hosting a socket.IO based chat application on Azure.</span></span> <span data-ttu-id="04163-106">Další informace o Socket.IO najdete v tématu <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="04163-106">For more information on Socket.IO, see <http://socket.io/>.</span></span>

<span data-ttu-id="04163-107">Zde je snímek obrazovky aplikace hello dokončena:</span><span class="sxs-lookup"><span data-stu-id="04163-107">A screenshot of hello completed application is below:</span></span>

![Okno prohlížeče zobrazující hello služby hostované v Azure][completed-app]  

## <a name="prerequisites"></a><span data-ttu-id="04163-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="04163-109">Prerequisites</span></span>
<span data-ttu-id="04163-110">Ujistěte se, že hello následující produkty a verze jsou nainstalované toosuccessfully hello kompletní příklad v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="04163-110">Ensure that hello following products and versions are installed toosuccessfully complete hello example in this article:</span></span>

* <span data-ttu-id="04163-111">Nainstalovat [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="04163-111">Install [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span></span>
* <span data-ttu-id="04163-112">Instalovat [Node.js](https://nodejs.org/download/)</span><span class="sxs-lookup"><span data-stu-id="04163-112">Install [Node.js](https://nodejs.org/download/)</span></span>
* <span data-ttu-id="04163-113">Nainstalujte [Python verze 2.7.10](https://www.python.org/)</span><span class="sxs-lookup"><span data-stu-id="04163-113">Install [Python version 2.7.10](https://www.python.org/)</span></span>

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="04163-114">Vytvoření projektu cloudové služby</span><span class="sxs-lookup"><span data-stu-id="04163-114">Create a Cloud Service Project</span></span>
<span data-ttu-id="04163-115">Hello následujících kroků vytvořte projekt hello cloudové služby, který bude hostitelem hello Socket.IO aplikace.</span><span class="sxs-lookup"><span data-stu-id="04163-115">hello following steps create hello cloud service project that will host hello Socket.IO application.</span></span>

1. <span data-ttu-id="04163-116">Z hello **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="04163-116">From hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="04163-117">Nakonec klikněte pravým tlačítkem na **prostředí Windows PowerShell** a vyberte **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="04163-117">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Azure PowerShell – ikona][powershell-menu]
2. <span data-ttu-id="04163-119">Vytvořte adresář s názvem **c:\\uzlu**.</span><span class="sxs-lookup"><span data-stu-id="04163-119">Create a directory called **c:\\node**.</span></span> 
   
        PS C:\> md node
3. <span data-ttu-id="04163-120">Změna adresáře toohello **c:\\uzlu** adresáře</span><span class="sxs-lookup"><span data-stu-id="04163-120">Change directories toohello **c:\\node** directory</span></span>
   
        PS C:\> cd node
4. <span data-ttu-id="04163-121">Zadejte následující příkazy toocreate nové řešení s názvem hello **chatapp** a roli pracovního procesu s názvem **WorkerRole1**:</span><span class="sxs-lookup"><span data-stu-id="04163-121">Enter hello following commands toocreate a new solution named **chatapp** and a worker role named **WorkerRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    <span data-ttu-id="04163-122">Zobrazí se následující odpověď hello:</span><span class="sxs-lookup"><span data-stu-id="04163-122">You will see hello following response:</span></span>
   
    ![výstup Hello hello nové azureservice a přidat azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-hello-chat-example"></a><span data-ttu-id="04163-124">Stáhnout hello Chat příklad</span><span class="sxs-lookup"><span data-stu-id="04163-124">Download hello Chat Example</span></span>
<span data-ttu-id="04163-125">Pro tento projekt použijeme hello chat příklad z hello [úložiště Socket.IO GitHub].</span><span class="sxs-lookup"><span data-stu-id="04163-125">For this project, we will use hello chat example from hello [Socket.IO GitHub repository].</span></span> <span data-ttu-id="04163-126">Proveďte následující kroky toodownload hello ukázka hello a přidejte ho toohello projektu, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="04163-126">Perform hello following steps toodownload hello example and add it toohello project you previously created.</span></span>

1. <span data-ttu-id="04163-127">Vytvořit místní kopii hello úložiště pomocí hello **klon** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04163-127">Create a local copy of hello repository by using hello **Clone** button.</span></span> <span data-ttu-id="04163-128">Můžete také používat hello **ZIP** tlačítko toodownload hello projektu.</span><span class="sxs-lookup"><span data-stu-id="04163-128">You may also use hello **ZIP** button toodownload hello project.</span></span>
   
   ![Zobrazení https://github.com/LearnBoost/socket.io/tree/master/examples/chat, ikonou hello ZIP stažení zvýrazněná okně prohlížeče][chat-example-view]
2. <span data-ttu-id="04163-130">Přejděte struktura adresářů hello hello místní úložiště, dokud přicházejí na hello **příklady\\chat** adresáře.</span><span class="sxs-lookup"><span data-stu-id="04163-130">Navigate hello directory structure of hello local repository until you arrive at hello **examples\\chat** directory.</span></span> <span data-ttu-id="04163-131">Kopírovat obsah tohoto adresáře toothe hello **C:\\uzlu\\chatapp\\WorkerRole1** adresář vytvořený dříve.</span><span class="sxs-lookup"><span data-stu-id="04163-131">Copy hello contents of this directory toothe **C:\\node\\chatapp\\WorkerRole1** directory created earlier.</span></span>
   
   ![Průzkumník, zobrazení obsahu hello hello příklady\\directory chat extrahovat z archivu hello][chat-contents]
   
   <span data-ttu-id="04163-133">Hello zvýrazněných v výše uvedený snímek obrazovky hello jsou položky hello souborům zkopírovaným ze hello **příklady\\chat** adresáře</span><span class="sxs-lookup"><span data-stu-id="04163-133">hello highlighted items in hello screenshot above are hello files copied from hello **examples\\chat** directory</span></span>
3. <span data-ttu-id="04163-134">V hello **C:\\uzlu\\chatapp\\WorkerRole1** adresář, odstraňte hello **server.js** souboru a přejmenujte hello **app.js**souboru příliš**server.js**.</span><span class="sxs-lookup"><span data-stu-id="04163-134">In hello **C:\\node\\chatapp\\WorkerRole1** directory, delete hello **server.js** file, and then rename hello **app.js** file too**server.js**.</span></span> <span data-ttu-id="04163-135">Odebere hello výchozí **server.js** soubor dříve vytvořený hello **přidat AzureNodeWorkerRole** rutiny a nahradí ji s aplikací hello souboru z hello chat příklad.</span><span class="sxs-lookup"><span data-stu-id="04163-135">This removes hello default **server.js** file created previously by hello **Add-AzureNodeWorkerRole** cmdlet and replaces it with hello application file from hello chat example.</span></span>

### <a name="modify-serverjs-and-install-modules"></a><span data-ttu-id="04163-136">Upravit Server.js a nainstalujte moduly</span><span class="sxs-lookup"><span data-stu-id="04163-136">Modify Server.js and Install Modules</span></span>
<span data-ttu-id="04163-137">Před testování aplikace hello v hello Azure emulátoru jsme musí provést některé menší změny.</span><span class="sxs-lookup"><span data-stu-id="04163-137">Before testing hello application in hello Azure emulator, we must make some minor modifications.</span></span> <span data-ttu-id="04163-138">Proveďte následující kroky souboru server.js toothe hello:</span><span class="sxs-lookup"><span data-stu-id="04163-138">Perform hello following steps toothe server.js file:</span></span>

1. <span data-ttu-id="04163-139">Otevřete hello **server.js** souborů v sadě Visual Studio nebo libovolného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="04163-139">Open hello **server.js** file in Visual Studio or any text editor.</span></span>
2. <span data-ttu-id="04163-140">Najde hello **modulu závislosti** části na začátku hello server.js a změňte hello řádek obsahující **sio = require('.. //.. lib//Socket.IO')** příliš**sio = require('socket.io')** jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="04163-140">Find hello **Module dependencies** section at hello beginning of server.js and change hello line containing **sio = require('..//..//lib//socket.io')** too**sio = require('socket.io')** as shown below:</span></span>
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. <span data-ttu-id="04163-141">tooensure hello aplikace naslouchá na správný port hello, otevřete v poznámkovém bloku nebo svém oblíbeném editoru server.js a poté změňte následující řádek tak, že nahradíte **3000** s **process.env.port** znázorněné níže:</span><span class="sxs-lookup"><span data-stu-id="04163-141">tooensure hello application listens on hello correct port, open server.js in Notepad or your favorite editor, and then change the following line by replacing **3000** with **process.env.port** as shown below:</span></span>
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

<span data-ttu-id="04163-142">Po uložení změn hello příliš**server.js**, použijte následující kroky hello k instalaci požadované moduly a otestujte hello aplikaci v emulátoru Azure:</span><span class="sxs-lookup"><span data-stu-id="04163-142">After saving hello changes too**server.js**, use hello following steps to install required modules, and then test hello application in the Azure emulator:</span></span>

1. <span data-ttu-id="04163-143">Pomocí **prostředí Azure PowerShell**, změňte adresáře toohello **C:\\uzlu\\chatapp\\WorkerRole1** hello adresáře a použijte následující příkaz tooinstall hello moduly, které vyžadují tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="04163-143">Using **Azure PowerShell**, change directories toohello **C:\\node\\chatapp\\WorkerRole1** directory and use hello following command tooinstall hello modules required by this application:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   <span data-ttu-id="04163-144">Nainstaluje uvedených v souboru package.json hello modulů hello.</span><span class="sxs-lookup"><span data-stu-id="04163-144">This will install hello modules listed in hello package.json file.</span></span> <span data-ttu-id="04163-145">Po dokončení příkazu hello byste měli vidět výstup podobný toothe následující:</span><span class="sxs-lookup"><span data-stu-id="04163-145">After hello command completes, you should see output similar toothe following:</span></span>
   
   ![příkaz instalovat Hello výstup hello npm][The-output-of-the-npm-install-command]
2. <span data-ttu-id="04163-147">Vzhledem k tomu, že v tomto příkladu byla původně součástí hello úložiště Socket.IO GitHub a knihovna Socket.IO hello přímo odkazuje relativní cestu, nebyl v souboru package.json hello, odkazuje Socket.IO, proto ji musíte nainstalovat vydáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="04163-147">Since this example was originally a part of hello Socket.IO GitHub repository, and directly referenced hello Socket.IO library by relative path, Socket.IO was not referenced in hello package.json file, so we must install it by issuing hello following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a><span data-ttu-id="04163-148">Testování a nasazení</span><span class="sxs-lookup"><span data-stu-id="04163-148">Test and Deploy</span></span>
1. <span data-ttu-id="04163-149">Spusťte emulátor hello vydáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="04163-149">Launch hello emulator by issuing hello following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > <span data-ttu-id="04163-150">Pokud narazíte na potíže se spuštěním emulátoru, např.: spuštění AzureEmulator: došlo k neočekávané chybě.</span><span class="sxs-lookup"><span data-stu-id="04163-150">If you encounter issues with launching emulator, eg.: Start-AzureEmulator : An unexpected failure occurred.</span></span>  <span data-ttu-id="04163-151">Podrobnosti: Došlo došlo k neočekávané chybě hello objekt komunikace, System.ServiceModel.Channels.ServiceChannel, nelze použít pro komunikaci protože je v chybném stavu hello.</span><span class="sxs-lookup"><span data-stu-id="04163-151">Details: Encountered an unexpected error hello communication object,  System.ServiceModel.Channels.ServiceChannel, cannot be used for communication because it is in hello Faulted state.</span></span>
   
      <span data-ttu-id="04163-152">Přeinstalujte AzureAuthoringTools v 2.7.1 a AzureComputeEmulator v 2.7 – Ujistěte se, že odpovídá této verze.</span><span class="sxs-lookup"><span data-stu-id="04163-152">reinstall AzureAuthoringTools v 2.7.1 and AzureComputeEmulator v 2.7 - make sure that version matches.</span></span>
   >
   >


2. <span data-ttu-id="04163-153">Otevřete prohlížeč a přejděte příliš**http://127.0.0.1**.</span><span class="sxs-lookup"><span data-stu-id="04163-153">Open a browser and navigate too**http://127.0.0.1**.</span></span>
3. <span data-ttu-id="04163-154">Když se otevře okno prohlížeče hello, zadejte přezdívku a potom stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="04163-154">When hello browser window opens, enter a nickname and then hit enter.</span></span>
   <span data-ttu-id="04163-155">To vám umožní zprávy toopost jako konkrétní přezdívka.</span><span class="sxs-lookup"><span data-stu-id="04163-155">This will allow you toopost messages as a specific nickname.</span></span> <span data-ttu-id="04163-156">Funkce tootest více uživatelů, spusťte další okna prohlížeče pomocí stejné adresy URL a zadejte jiné přezdívky.</span><span class="sxs-lookup"><span data-stu-id="04163-156">tootest multi-user functionality, open additional browser windows using the same URL and enter different nicknames.</span></span>
   
   ![Zobrazení chatovací zprávy z uživatel1 a uživatel2 dvě okna prohlížeče](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. <span data-ttu-id="04163-158">Po testování aplikace hello zastavte hello emulátoru vydání následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="04163-158">After testing hello application, stop hello emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. <span data-ttu-id="04163-159">toodeploy hello aplikace tooAzure, použijte **Publish-AzureServiceProject** rutiny.</span><span class="sxs-lookup"><span data-stu-id="04163-159">toodeploy hello application tooAzure, use the **Publish-AzureServiceProject** cmdlet.</span></span> <span data-ttu-id="04163-160">Například:</span><span class="sxs-lookup"><span data-stu-id="04163-160">For example:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > <span data-ttu-id="04163-161">Se stát, že toouse jedinečný název, jinak hello publikování proces se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="04163-161">Be sure toouse a unique name, otherwise hello publish process will fail.</span></span> <span data-ttu-id="04163-162">Po dokončení nasazení hello hello prohlížeč a přejděte toohello nasazené služby.</span><span class="sxs-lookup"><span data-stu-id="04163-162">After hello deployment has completed, hello browser will open and navigate toohello deployed service.</span></span>
   > 
   > <span data-ttu-id="04163-163">Pokud se zobrazí chybová zpráva, že hello, pokud název odběru neexistuje v hello importovat profil publikování, musíte stáhnout a naimportovat profil publikování hello pro vaše předplatné před nasazením tooAzure.</span><span class="sxs-lookup"><span data-stu-id="04163-163">If you receive an error stating that hello provided subscription name doesn't exist in hello imported publish profile, you must download and import hello publishing profile for your subscription before deploying tooAzure.</span></span> <span data-ttu-id="04163-164">V tématu hello **nasazení tooAzure aplikace hello** části [sestavení a nasazení tooan aplikace Node.js Cloudová služba Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="04163-164">See hello **Deploying hello Application tooAzure** section of [Build and deploy a Node.js application tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 
   
   ![Okno prohlížeče zobrazující hello služby hostované v Azure][completed-app]
   
   > [!NOTE]
   > <span data-ttu-id="04163-166">Pokud se zobrazí chybová zpráva, že hello, pokud název odběru neexistuje v hello importovat profil publikování, musíte stáhnout a naimportovat profil publikování hello pro vaše předplatné před nasazením tooAzure.</span><span class="sxs-lookup"><span data-stu-id="04163-166">If you receive an error stating that hello provided subscription name doesn't exist in hello imported publish profile, you must download and import hello publishing profile for your subscription before deploying tooAzure.</span></span> <span data-ttu-id="04163-167">V tématu hello **nasazení tooAzure aplikace hello** části [sestavení a nasazení tooan aplikace Node.js Cloudová služba Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="04163-167">See hello **Deploying hello Application tooAzure** section of [Build and deploy a Node.js application tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 

<span data-ttu-id="04163-168">Vaše aplikace je nyní spuštěna v Azure a může přenášet zprávy chat mezi různé klienty pomocí Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="04163-168">Your application is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

> [!NOTE]
> <span data-ttu-id="04163-169">Pro jednoduchost, tato ukázka je omezená toochatting mezi toohello připojených uživatelů stejnou instanci.</span><span class="sxs-lookup"><span data-stu-id="04163-169">For simplicity, this sample is limited toochatting between users connected toohello same instance.</span></span> <span data-ttu-id="04163-170">To znamená, že pokud hello Cloudová služba vytvoří dvě instance role pracovního procesu, uživatele pouze můžou toochat s ostatními připojené toohello stejnou instanci role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="04163-170">This means that if hello cloud service creates two worker role instances, users will only be able toochat with others connected toohello same worker role instance.</span></span> <span data-ttu-id="04163-171">tooscale toowork aplikace hello s více instancí role, můžete použít technologie jako Service Bus tooshare hello Socket.IO ukládání stavu napříč instancemi.</span><span class="sxs-lookup"><span data-stu-id="04163-171">tooscale hello application toowork with multiple role instances, you could use a technology like Service Bus tooshare hello Socket.IO store state across instances.</span></span> <span data-ttu-id="04163-172">Příklady najdete v tématu hello témata a fronty služby Service Bus ukázky využití v hello [Azure SDK pro Node.js Githubu úložiště](https://github.com/WindowsAzure/azure-sdk-for-node).</span><span class="sxs-lookup"><span data-stu-id="04163-172">For examples, see hello Service Bus Queues and Topics usage samples in hello [Azure SDK for Node.js GitHub repository](https://github.com/WindowsAzure/azure-sdk-for-node).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="04163-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04163-173">Next steps</span></span>
<span data-ttu-id="04163-174">V tomto kurzu jste se dozvěděli, jak toocreate základní chatovací aplikace hostované ve službě Azure Cloud Service.</span><span class="sxs-lookup"><span data-stu-id="04163-174">In this tutorial you learned how toocreate a basic chat application hosted in an Azure Cloud Service.</span></span> <span data-ttu-id="04163-175">toolearn jak toohost této aplikace na webu Azure, najdete v části [sestavení aplikace Chat v Node.js se Socket.IO na webovou stránku Azure][chatwebsite].</span><span class="sxs-lookup"><span data-stu-id="04163-175">toolearn how toohost this application in an Azure Website, see [Build a Node.js Chat Application with Socket.IO on an Azure Web Site][chatwebsite].</span></span>

<span data-ttu-id="04163-176">Další informace najdete v tématu taky hello [středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="04163-176">For more information, see also hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[úložiště Socket.IO GitHub]: https://github.com/LearnBoost/socket.io/tree/0.9.14
[Azure Considerations]: #windowsazureconsiderations
[Hosting hello Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


