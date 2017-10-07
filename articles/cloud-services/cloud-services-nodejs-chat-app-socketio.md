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
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Sestavení aplikace Chat v Node.js se Socket.IO na cloudové služby Azure
Socket.IO poskytuje v reálném čase komunikace mezi mezi node.js serverem a klienty. Tento kurz vás provede hostování soketu. Vstupně-výstupních operací na základě chatovací aplikace v Azure. Další informace o Socket.IO najdete v tématu <http://socket.io/>.

Zde je snímek obrazovky aplikace hello dokončena:

![Okno prohlížeče zobrazující hello služby hostované v Azure][completed-app]  

## <a name="prerequisites"></a>Požadavky
Ujistěte se, že hello následující produkty a verze jsou nainstalované toosuccessfully hello kompletní příklad v tomto článku:

* Nainstalovat [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* Instalovat [Node.js](https://nodejs.org/download/)
* Nainstalujte [Python verze 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Vytvoření projektu cloudové služby
Hello následujících kroků vytvořte projekt hello cloudové služby, který bude hostitelem hello Socket.IO aplikace.

1. Z hello **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**. Nakonec klikněte pravým tlačítkem na **prostředí Windows PowerShell** a vyberte **spustit jako správce**.
   
    ![Azure PowerShell – ikona][powershell-menu]
2. Vytvořte adresář s názvem **c:\\uzlu**. 
   
        PS C:\> md node
3. Změna adresáře toohello **c:\\uzlu** adresáře
   
        PS C:\> cd node
4. Zadejte následující příkazy toocreate nové řešení s názvem hello **chatapp** a roli pracovního procesu s názvem **WorkerRole1**:
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    Zobrazí se následující odpověď hello:
   
    ![výstup Hello hello nové azureservice a přidat azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-hello-chat-example"></a>Stáhnout hello Chat příklad
Pro tento projekt použijeme hello chat příklad z hello [úložiště Socket.IO GitHub]. Proveďte následující kroky toodownload hello ukázka hello a přidejte ho toohello projektu, které jste vytvořili.

1. Vytvořit místní kopii hello úložiště pomocí hello **klon** tlačítko. Můžete také používat hello **ZIP** tlačítko toodownload hello projektu.
   
   ![Zobrazení https://github.com/LearnBoost/socket.io/tree/master/examples/chat, ikonou hello ZIP stažení zvýrazněná okně prohlížeče][chat-example-view]
2. Přejděte struktura adresářů hello hello místní úložiště, dokud přicházejí na hello **příklady\\chat** adresáře. Kopírovat obsah tohoto adresáře toothe hello **C:\\uzlu\\chatapp\\WorkerRole1** adresář vytvořený dříve.
   
   ![Průzkumník, zobrazení obsahu hello hello příklady\\directory chat extrahovat z archivu hello][chat-contents]
   
   Hello zvýrazněných v výše uvedený snímek obrazovky hello jsou položky hello souborům zkopírovaným ze hello **příklady\\chat** adresáře
3. V hello **C:\\uzlu\\chatapp\\WorkerRole1** adresář, odstraňte hello **server.js** souboru a přejmenujte hello **app.js**souboru příliš**server.js**. Odebere hello výchozí **server.js** soubor dříve vytvořený hello **přidat AzureNodeWorkerRole** rutiny a nahradí ji s aplikací hello souboru z hello chat příklad.

### <a name="modify-serverjs-and-install-modules"></a>Upravit Server.js a nainstalujte moduly
Před testování aplikace hello v hello Azure emulátoru jsme musí provést některé menší změny. Proveďte následující kroky souboru server.js toothe hello:

1. Otevřete hello **server.js** souborů v sadě Visual Studio nebo libovolného textového editoru.
2. Najde hello **modulu závislosti** části na začátku hello server.js a změňte hello řádek obsahující **sio = require('.. //.. lib//Socket.IO')** příliš**sio = require('socket.io')** jak je uvedeno níže:
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. tooensure hello aplikace naslouchá na správný port hello, otevřete v poznámkovém bloku nebo svém oblíbeném editoru server.js a poté změňte následující řádek tak, že nahradíte **3000** s **process.env.port** znázorněné níže:
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

Po uložení změn hello příliš**server.js**, použijte následující kroky hello k instalaci požadované moduly a otestujte hello aplikaci v emulátoru Azure:

1. Pomocí **prostředí Azure PowerShell**, změňte adresáře toohello **C:\\uzlu\\chatapp\\WorkerRole1** hello adresáře a použijte následující příkaz tooinstall hello moduly, které vyžadují tuto aplikaci:
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   Nainstaluje uvedených v souboru package.json hello modulů hello. Po dokončení příkazu hello byste měli vidět výstup podobný toothe následující:
   
   ![příkaz instalovat Hello výstup hello npm][The-output-of-the-npm-install-command]
2. Vzhledem k tomu, že v tomto příkladu byla původně součástí hello úložiště Socket.IO GitHub a knihovna Socket.IO hello přímo odkazuje relativní cestu, nebyl v souboru package.json hello, odkazuje Socket.IO, proto ji musíte nainstalovat vydáním hello následující příkaz:
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testování a nasazení
1. Spusťte emulátor hello vydáním hello následující příkaz:
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > Pokud narazíte na potíže se spuštěním emulátoru, např.: spuštění AzureEmulator: došlo k neočekávané chybě.  Podrobnosti: Došlo došlo k neočekávané chybě hello objekt komunikace, System.ServiceModel.Channels.ServiceChannel, nelze použít pro komunikaci protože je v chybném stavu hello.
   
      Přeinstalujte AzureAuthoringTools v 2.7.1 a AzureComputeEmulator v 2.7 – Ujistěte se, že odpovídá této verze.
   >
   >


2. Otevřete prohlížeč a přejděte příliš**http://127.0.0.1**.
3. Když se otevře okno prohlížeče hello, zadejte přezdívku a potom stiskněte klávesu enter.
   To vám umožní zprávy toopost jako konkrétní přezdívka. Funkce tootest více uživatelů, spusťte další okna prohlížeče pomocí stejné adresy URL a zadejte jiné přezdívky.
   
   ![Zobrazení chatovací zprávy z uživatel1 a uživatel2 dvě okna prohlížeče](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. Po testování aplikace hello zastavte hello emulátoru vydání následujícího příkazu:
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. toodeploy hello aplikace tooAzure, použijte **Publish-AzureServiceProject** rutiny. Například:
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > Se stát, že toouse jedinečný název, jinak hello publikování proces se nezdaří. Po dokončení nasazení hello hello prohlížeč a přejděte toohello nasazené služby.
   > 
   > Pokud se zobrazí chybová zpráva, že hello, pokud název odběru neexistuje v hello importovat profil publikování, musíte stáhnout a naimportovat profil publikování hello pro vaše předplatné před nasazením tooAzure. V tématu hello **nasazení tooAzure aplikace hello** části [sestavení a nasazení tooan aplikace Node.js Cloudová služba Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 
   
   ![Okno prohlížeče zobrazující hello služby hostované v Azure][completed-app]
   
   > [!NOTE]
   > Pokud se zobrazí chybová zpráva, že hello, pokud název odběru neexistuje v hello importovat profil publikování, musíte stáhnout a naimportovat profil publikování hello pro vaše předplatné před nasazením tooAzure. V tématu hello **nasazení tooAzure aplikace hello** části [sestavení a nasazení tooan aplikace Node.js Cloudová služba Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 

Vaše aplikace je nyní spuštěna v Azure a může přenášet zprávy chat mezi různé klienty pomocí Socket.IO.

> [!NOTE]
> Pro jednoduchost, tato ukázka je omezená toochatting mezi toohello připojených uživatelů stejnou instanci. To znamená, že pokud hello Cloudová služba vytvoří dvě instance role pracovního procesu, uživatele pouze můžou toochat s ostatními připojené toohello stejnou instanci role pracovního procesu. tooscale toowork aplikace hello s více instancí role, můžete použít technologie jako Service Bus tooshare hello Socket.IO ukládání stavu napříč instancemi. Příklady najdete v tématu hello témata a fronty služby Service Bus ukázky využití v hello [Azure SDK pro Node.js Githubu úložiště](https://github.com/WindowsAzure/azure-sdk-for-node).
> 
> 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli, jak toocreate základní chatovací aplikace hostované ve službě Azure Cloud Service. toolearn jak toohost této aplikace na webu Azure, najdete v části [sestavení aplikace Chat v Node.js se Socket.IO na webovou stránku Azure][chatwebsite].

Další informace najdete v tématu taky hello [středisku pro vývojáře Node.js](/develop/nodejs/).

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


