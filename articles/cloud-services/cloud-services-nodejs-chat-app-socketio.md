---
title: "Aplikace Node.js pomocí Socket.io | Microsoft Docs"
description: "Další informace o použití socket.io v aplikaci node.js hostované v Azure."
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
ms.openlocfilehash: a85d4348a13b79b5b7542362de9956aa3398375a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a><span data-ttu-id="aef62-103">Sestavení aplikace Chat v Node.js se Socket.IO na cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="aef62-103">Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service</span></span>
<span data-ttu-id="aef62-104">Socket.IO poskytuje v reálném čase komunikace mezi mezi node.js serverem a klienty.</span><span class="sxs-lookup"><span data-stu-id="aef62-104">Socket.IO provides realtime communication between between your node.js server and clients.</span></span> <span data-ttu-id="aef62-105">Tento kurz vás provede hostování soketu. Vstupně-výstupních operací na základě chatovací aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="aef62-105">This tutorial will walk you through hosting a socket.IO based chat application on Azure.</span></span> <span data-ttu-id="aef62-106">Další informace o Socket.IO najdete v tématu <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="aef62-106">For more information on Socket.IO, see <http://socket.io/>.</span></span>

<span data-ttu-id="aef62-107">Zde je snímek obrazovky dokončené aplikace:</span><span class="sxs-lookup"><span data-stu-id="aef62-107">A screenshot of the completed application is below:</span></span>

![Okno prohlížeče zobrazující služby hostované v Azure][completed-app]  

## <a name="prerequisites"></a><span data-ttu-id="aef62-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="aef62-109">Prerequisites</span></span>
<span data-ttu-id="aef62-110">Ujistěte se, že jsou nainstalovány následující produkty a verze úspěšné dokončení v příkladu v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="aef62-110">Ensure that the following products and versions are installed to successfully complete the example in this article:</span></span>

* <span data-ttu-id="aef62-111">Nainstalovat [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="aef62-111">Install [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span></span>
* <span data-ttu-id="aef62-112">Instalovat [Node.js](https://nodejs.org/download/)</span><span class="sxs-lookup"><span data-stu-id="aef62-112">Install [Node.js](https://nodejs.org/download/)</span></span>
* <span data-ttu-id="aef62-113">Nainstalujte [Python verze 2.7.10](https://www.python.org/)</span><span class="sxs-lookup"><span data-stu-id="aef62-113">Install [Python version 2.7.10](https://www.python.org/)</span></span>

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="aef62-114">Vytvoření projektu cloudové služby</span><span class="sxs-lookup"><span data-stu-id="aef62-114">Create a Cloud Service Project</span></span>
<span data-ttu-id="aef62-115">Následující postup vytvořit projekt cloudové služby, který bude hostitelem Socket.IO aplikace.</span><span class="sxs-lookup"><span data-stu-id="aef62-115">The following steps create the cloud service project that will host the Socket.IO application.</span></span>

1. <span data-ttu-id="aef62-116">Z **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="aef62-116">From the **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="aef62-117">Nakonec klikněte pravým tlačítkem na **prostředí Windows PowerShell** a vyberte **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="aef62-117">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Azure PowerShell – ikona][powershell-menu]
2. <span data-ttu-id="aef62-119">Vytvořte adresář s názvem **c:\\uzlu**.</span><span class="sxs-lookup"><span data-stu-id="aef62-119">Create a directory called **c:\\node**.</span></span> 
   
        PS C:\> md node
3. <span data-ttu-id="aef62-120">Přejděte do adresáře **c:\\uzlu** adresáře</span><span class="sxs-lookup"><span data-stu-id="aef62-120">Change directories to the **c:\\node** directory</span></span>
   
        PS C:\> cd node
4. <span data-ttu-id="aef62-121">Zadejte následující příkazy k vytvoření nové řešení s názvem **chatapp** a roli pracovního procesu s názvem **WorkerRole1**:</span><span class="sxs-lookup"><span data-stu-id="aef62-121">Enter the following commands to create a new solution named **chatapp** and a worker role named **WorkerRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    <span data-ttu-id="aef62-122">Zobrazí se následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="aef62-122">You will see the following response:</span></span>
   
    ![Výstup nové azureservice a přidat azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a><span data-ttu-id="aef62-124">Stáhněte si příklad chatu</span><span class="sxs-lookup"><span data-stu-id="aef62-124">Download the Chat Example</span></span>
<span data-ttu-id="aef62-125">Pro tento projekt použijeme příklad chat z [úložiště Socket.IO GitHub].</span><span class="sxs-lookup"><span data-stu-id="aef62-125">For this project, we will use the chat example from the [Socket.IO GitHub repository].</span></span> <span data-ttu-id="aef62-126">Proveďte následující kroky ke stažení v příkladu a přidat jej do projektu, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="aef62-126">Perform the following steps to download the example and add it to the project you previously created.</span></span>

1. <span data-ttu-id="aef62-127">Vytvořit místní kopii úložiště pomocí **klon** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aef62-127">Create a local copy of the repository by using the **Clone** button.</span></span> <span data-ttu-id="aef62-128">Můžete také používat **ZIP** tlačítko Stáhnout projektu.</span><span class="sxs-lookup"><span data-stu-id="aef62-128">You may also use the **ZIP** button to download the project.</span></span>
   
   ![Okno prohlížeče zobrazení https://github.com/LearnBoost/socket.io/tree/master/examples/chat, zvýrazněnou ikonou stáhnout ZIP][chat-example-view]
2. <span data-ttu-id="aef62-130">Až přijedete do přejděte struktura adresářů místní úložiště **příklady\\chat** adresáře.</span><span class="sxs-lookup"><span data-stu-id="aef62-130">Navigate the directory structure of the local repository until you arrive at the **examples\\chat** directory.</span></span> <span data-ttu-id="aef62-131">Kopírovat obsah tohoto adresáře na **C:\\uzlu\\chatapp\\WorkerRole1** adresář vytvořený.</span><span class="sxs-lookup"><span data-stu-id="aef62-131">Copy the contents of this directory to the **C:\\node\\chatapp\\WorkerRole1** directory created earlier.</span></span>
   
   ![Průzkumník, zobrazení obsahu v příkladech\\directory chat extrahovat z archivu][chat-contents]
   
   <span data-ttu-id="aef62-133">Zvýrazněné položky v výše uvedený snímek obrazovky jsou souborům zkopírovaným ze **příklady\\chat** adresáře</span><span class="sxs-lookup"><span data-stu-id="aef62-133">The highlighted items in the screenshot above are the files copied from the **examples\\chat** directory</span></span>
3. <span data-ttu-id="aef62-134">V **C:\\uzlu\\chatapp\\WorkerRole1** adresáře, odstraňte **server.js** souboru a přejmenujte **app.js** do souboru **server.js**.</span><span class="sxs-lookup"><span data-stu-id="aef62-134">In the **C:\\node\\chatapp\\WorkerRole1** directory, delete the **server.js** file, and then rename the **app.js** file to **server.js**.</span></span> <span data-ttu-id="aef62-135">Odebere výchozí **server.js** soubor vytvořený dříve **přidat AzureNodeWorkerRole** rutiny a nahradí soubor aplikace z příkladu konverzace.</span><span class="sxs-lookup"><span data-stu-id="aef62-135">This removes the default **server.js** file created previously by the **Add-AzureNodeWorkerRole** cmdlet and replaces it with the application file from the chat example.</span></span>

### <a name="modify-serverjs-and-install-modules"></a><span data-ttu-id="aef62-136">Upravit Server.js a nainstalujte moduly</span><span class="sxs-lookup"><span data-stu-id="aef62-136">Modify Server.js and Install Modules</span></span>
<span data-ttu-id="aef62-137">Před testováním aplikace v Azure emulátoru, jsme musí provést některé menší změny.</span><span class="sxs-lookup"><span data-stu-id="aef62-137">Before testing the application in the Azure emulator, we must make some minor modifications.</span></span> <span data-ttu-id="aef62-138">Proveďte následující kroky v souboru server.js:</span><span class="sxs-lookup"><span data-stu-id="aef62-138">Perform the following steps to the server.js file:</span></span>

1. <span data-ttu-id="aef62-139">Otevřete **server.js** souborů v sadě Visual Studio nebo libovolného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="aef62-139">Open the **server.js** file in Visual Studio or any text editor.</span></span>
2. <span data-ttu-id="aef62-140">Najít **modulu závislosti** části na začátku server.js a změňte řádek obsahující **sio = require('.. //.. lib//Socket.IO')** k **sio = require('socket.io')** jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="aef62-140">Find the **Module dependencies** section at the beginning of server.js and change the line containing **sio = require('..//..//lib//socket.io')** to **sio = require('socket.io')** as shown below:</span></span>
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. <span data-ttu-id="aef62-141">Aby aplikace naslouchá na správný port, otevřete server.js v poznámkovém bloku nebo svém oblíbeném editoru a potom změňte následující řádek tak, že nahradíte **3000** s **process.env.port** jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="aef62-141">To ensure the application listens on the correct port, open server.js in Notepad or your favorite editor, and then change the following line by replacing **3000** with **process.env.port** as shown below:</span></span>
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

<span data-ttu-id="aef62-142">Po uložení změn do **server.js**, použijte následující postup k instalaci požadované moduly a otestujte aplikaci v emulátoru Azure:</span><span class="sxs-lookup"><span data-stu-id="aef62-142">After saving the changes to **server.js**, use the following steps to install required modules, and then test the application in the Azure emulator:</span></span>

1. <span data-ttu-id="aef62-143">Pomocí **prostředí Azure PowerShell**, přejděte do adresáře **C:\\uzlu\\chatapp\\WorkerRole1** adresáře a použijte následující příkaz pro instalaci modulů Tato aplikace vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="aef62-143">Using **Azure PowerShell**, change directories to the **C:\\node\\chatapp\\WorkerRole1** directory and use the following command to install the modules required by this application:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   <span data-ttu-id="aef62-144">Nainstaluje uvedených v souboru package.json modulů.</span><span class="sxs-lookup"><span data-stu-id="aef62-144">This will install the modules listed in the package.json file.</span></span> <span data-ttu-id="aef62-145">Po dokončení příkazu, byste měli vidět výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="aef62-145">After the command completes, you should see output similar to the following:</span></span>
   
   ![Příkaz instalovat výstup npm][The-output-of-the-npm-install-command]
2. <span data-ttu-id="aef62-147">Vzhledem k tomu, že v tomto příkladu byla původně součástí úložiště Socket.IO GitHub a knihovně Socket.IO přímo odkazuje relativní cestu, nebyl v souboru package.json, odkazuje Socket.IO, proto ji musíte nainstalovat po vydání následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="aef62-147">Since this example was originally a part of the Socket.IO GitHub repository, and directly referenced the Socket.IO library by relative path, Socket.IO was not referenced in the package.json file, so we must install it by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a><span data-ttu-id="aef62-148">Testování a nasazení</span><span class="sxs-lookup"><span data-stu-id="aef62-148">Test and Deploy</span></span>
1. <span data-ttu-id="aef62-149">Spusťte emulátor po vydání následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="aef62-149">Launch the emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > <span data-ttu-id="aef62-150">Pokud narazíte na potíže se spuštěním emulátoru, např.: spuštění AzureEmulator: došlo k neočekávané chybě.</span><span class="sxs-lookup"><span data-stu-id="aef62-150">If you encounter issues with launching emulator, eg.: Start-AzureEmulator : An unexpected failure occurred.</span></span>  <span data-ttu-id="aef62-151">Podrobnosti: Došlo k neočekávané chybě objekt komunikace, System.ServiceModel.Channels.ServiceChannel, nelze použít pro komunikaci protože je v chybném stavu.</span><span class="sxs-lookup"><span data-stu-id="aef62-151">Details: Encountered an unexpected error The communication object,  System.ServiceModel.Channels.ServiceChannel, cannot be used for communication because it is in the Faulted state.</span></span>
   
      <span data-ttu-id="aef62-152">Přeinstalujte AzureAuthoringTools v 2.7.1 a AzureComputeEmulator v 2.7 – Ujistěte se, že odpovídá této verze.</span><span class="sxs-lookup"><span data-stu-id="aef62-152">reinstall AzureAuthoringTools v 2.7.1 and AzureComputeEmulator v 2.7 - make sure that version matches.</span></span>
   >
   >


2. <span data-ttu-id="aef62-153">Otevřete prohlížeč a přejděte do **http://127.0.0.1**.</span><span class="sxs-lookup"><span data-stu-id="aef62-153">Open a browser and navigate to **http://127.0.0.1**.</span></span>
3. <span data-ttu-id="aef62-154">Když se otevře okno prohlížeče, zadejte přezdívku a potom stiskněte klávesu enter.</span><span class="sxs-lookup"><span data-stu-id="aef62-154">When the browser window opens, enter a nickname and then hit enter.</span></span>
   <span data-ttu-id="aef62-155">To vám umožní k odeslání zprávy jako konkrétní přezdívka.</span><span class="sxs-lookup"><span data-stu-id="aef62-155">This will allow you to post messages as a specific nickname.</span></span> <span data-ttu-id="aef62-156">K testování funkčnosti více uživatelů, spusťte další okna prohlížeče pomocí stejné adresy URL a zadejte jiné přezdívky.</span><span class="sxs-lookup"><span data-stu-id="aef62-156">To test multi-user functionality, open additional browser windows using the same URL and enter different nicknames.</span></span>
   
   ![Zobrazení chatovací zprávy z uživatel1 a uživatel2 dvě okna prohlížeče](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. <span data-ttu-id="aef62-158">Po testování aplikace, zastavte emulátoru vydání následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="aef62-158">After testing the application, stop the emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. <span data-ttu-id="aef62-159">Chcete-li nasadit aplikaci do Azure, použijte **Publish-AzureServiceProject** rutiny.</span><span class="sxs-lookup"><span data-stu-id="aef62-159">To deploy the application to Azure, use the **Publish-AzureServiceProject** cmdlet.</span></span> <span data-ttu-id="aef62-160">Například:</span><span class="sxs-lookup"><span data-stu-id="aef62-160">For example:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > <span data-ttu-id="aef62-161">Nezapomeňte použít jedinečný název, jinak se proces publikování nezdaří.</span><span class="sxs-lookup"><span data-stu-id="aef62-161">Be sure to use a unique name, otherwise the publish process will fail.</span></span> <span data-ttu-id="aef62-162">Po dokončení nasazení se v prohlížeči otevřete a přejděte do nasazené služby.</span><span class="sxs-lookup"><span data-stu-id="aef62-162">After the deployment has completed, the browser will open and navigate to the deployed service.</span></span>
   > 
   > <span data-ttu-id="aef62-163">Pokud se zobrazí chyba oznamující, že název zadané předplatné neexistuje v profilu publikování importované, musíte stáhnout a naimportovat profil publikování pro vaše předplatné před nasazením do Azure.</span><span class="sxs-lookup"><span data-stu-id="aef62-163">If you receive an error stating that the provided subscription name doesn't exist in the imported publish profile, you must download and import the publishing profile for your subscription before deploying to Azure.</span></span> <span data-ttu-id="aef62-164">Najdete v článku **nasazení aplikace do Azure** části [sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="aef62-164">See the **Deploying the Application to Azure** section of [Build and deploy a Node.js application to an Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 
   
   ![Okno prohlížeče zobrazující služby hostované v Azure][completed-app]
   
   > [!NOTE]
   > <span data-ttu-id="aef62-166">Pokud se zobrazí chyba oznamující, že název zadané předplatné neexistuje v profilu publikování importované, musíte stáhnout a naimportovat profil publikování pro vaše předplatné před nasazením do Azure.</span><span class="sxs-lookup"><span data-stu-id="aef62-166">If you receive an error stating that the provided subscription name doesn't exist in the imported publish profile, you must download and import the publishing profile for your subscription before deploying to Azure.</span></span> <span data-ttu-id="aef62-167">Najdete v článku **nasazení aplikace do Azure** části [sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="aef62-167">See the **Deploying the Application to Azure** section of [Build and deploy a Node.js application to an Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 

<span data-ttu-id="aef62-168">Vaše aplikace je nyní spuštěna v Azure a může přenášet zprávy chat mezi různé klienty pomocí Socket.IO.</span><span class="sxs-lookup"><span data-stu-id="aef62-168">Your application is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

> [!NOTE]
> <span data-ttu-id="aef62-169">Pro jednoduchost Tato ukázka je omezený na chatování mezi uživatele připojené ke stejné instanci.</span><span class="sxs-lookup"><span data-stu-id="aef62-169">For simplicity, this sample is limited to chatting between users connected to the same instance.</span></span> <span data-ttu-id="aef62-170">To znamená, že pokud cloudové službě vytvoří dvě instance role pracovního procesu, uživatele bude moci pouze chat s ostatními připojení na stejnou instanci role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="aef62-170">This means that if the cloud service creates two worker role instances, users will only be able to chat with others connected to the same worker role instance.</span></span> <span data-ttu-id="aef62-171">Škálování aplikace pro práci s více instancí role, můžete použít technologie, jako je Service Bus ke sdílení ukládání stavu Socket.IO napříč instancemi.</span><span class="sxs-lookup"><span data-stu-id="aef62-171">To scale the application to work with multiple role instances, you could use a technology like Service Bus to share the Socket.IO store state across instances.</span></span> <span data-ttu-id="aef62-172">Příklady najdete v tématu ukázky využití fronty služby Service Bus a témat v [Azure SDK pro Node.js Githubu úložiště](https://github.com/WindowsAzure/azure-sdk-for-node).</span><span class="sxs-lookup"><span data-stu-id="aef62-172">For examples, see the Service Bus Queues and Topics usage samples in the [Azure SDK for Node.js GitHub repository](https://github.com/WindowsAzure/azure-sdk-for-node).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="aef62-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aef62-173">Next steps</span></span>
<span data-ttu-id="aef62-174">V tomto kurzu jste zjistili, jak vytvořit základní chatovací aplikace hostované na cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="aef62-174">In this tutorial you learned how to create a basic chat application hosted in an Azure Cloud Service.</span></span> <span data-ttu-id="aef62-175">Zjistěte, jak hostovat tuto aplikaci na webu Azure, najdete v tématu [sestavení aplikace Chat v Node.js se Socket.IO na webovou stránku Azure][chatwebsite].</span><span class="sxs-lookup"><span data-stu-id="aef62-175">To learn how to host this application in an Azure Website, see [Build a Node.js Chat Application with Socket.IO on an Azure Web Site][chatwebsite].</span></span>

<span data-ttu-id="aef62-176">Další informace naleznete také [středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="aef62-176">For more information, see also the [Node.js Developer Center](/develop/nodejs/).</span></span>

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
<span data-ttu-id="aef62-177">[úložiště Socket.IO GitHub]: https://github.com/LearnBoost/socket.io/tree/0.9.14</span><span class="sxs-lookup"><span data-stu-id="aef62-177">[Socket.IO GitHub repository]: https://github.com/LearnBoost/socket.io/tree/0.9.14</span></span>
[Azure Considerations]: #windowsazureconsiderations
[Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


