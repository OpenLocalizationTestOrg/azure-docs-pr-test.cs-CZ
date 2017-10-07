---
title: "aaaSending nabízená oznámení pomocí Azure Notification Hubs a Node.js"
description: "Zjistěte, jak toouse Notification Hubs toosend nabízená oznámení z aplikace Node.js."
keywords: "nabízená oznámení, nabízené notifications,node.js nabízet, ios push"
services: notification-hubs
documentationcenter: nodejs
author: ysxu
manager: erikre
editor: 
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/25/2016
ms.author: yuaxu
ms.openlocfilehash: 151d224fa6dd07e4acdc3a4887c4e95ee03168c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Odesílání nabízených oznámení pomocí Azure Notification Hubs a Node.js
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Přehled
> [!IMPORTANT]
> toocomplete tento kurz, musíte mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).
> 
> 

Tento průvodce vám ukáže, jak toosend nabízená oznámení pomocí hello Azure Notification Hubs přímo z aplikace Node.js. 

Hello pokryté scénáře zahrnují, odesílání nabízených oznámení tooapplications na hello následující platformy:

* Android
* iOS
* telefon se systémem Windows
* Univerzální platforma pro Windows 

Další informace o notification hubs najdete v tématu hello [další kroky](#next) části.

## <a name="what-are-notification-hubs"></a>Co jsou Notification Hubs?
Azure Notification Hubs poskytuje snadno použitelnou, více platformami, škálovatelné infrastruktury pro odesílání nabízených oznámení toomobile zařízení. Podrobnosti o hello služby infrastruktury, najdete v části hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) stránky.

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js
Hello první krok v tomto kurzu je vytvoření nové prázdné aplikace Node.js. Pokyny pro vytvoření aplikace Node.js, naleznete v části [vytvořit a nasadit tooAzure aplikace Node.js webu][nodejswebsite], [Node.js Cloudová služba] [ Node.js Cloud Service] pomocí prostředí Windows PowerShell nebo [webu pomocí služby WebMatrix].

## <a name="configure-your-application-toouse-notification-hubs"></a>Konfigurace centra oznámení tooUse vaše aplikace
toouse Azure Notification Hubs, budete potřebovat toodownload a použití hello Node.js [balíček azure](https://www.npmjs.com/package/azure), což zahrnuje integrovanou sadu pomocné knihovny, které komunikují REST služeb hello nabízených oznámení.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Použijte uzel balíček správce (NPM) tooobtain hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Linux) a přejděte toohello složku, kde můžete vytvořit prázdnou aplikaci .
2. Typ **npm nainstalujte azure sb** v příkazovém okně hello.
3. Můžete ručně spustit hello **ls** nebo **dir** tooverify příkaz, **uzlu\_moduly** složka byla vytvořena. V této složce najít hello **azure** balíček, který obsahuje hello knihovny, je nutné tooaccess hello centra oznámení.

> [!NOTE]
> Další informace o instalaci NPM hello oficiální [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 
> 
> 

### <a name="import-hello-module"></a>Import modulu hello
Pomocí textového editoru, přidejte následující toohello horní části hello hello **server.js** souboru aplikace hello:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Nastavit připojení k Azure Notification Hubs
Hello **NotificationHubService** objekt vám umožňuje spolupracovat s centry oznámení. Hello následující kód vytvoří **NotificationHubService** objekt pro rozbočovač nofication hello s názvem **hubname**. Přidat v hello horní části hello **server.js** souboru po hello příkaz tooimport hello azure modul:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Hello připojení **connectionstring** nelze získat hodnotu z hello [portálu Azure] provedením hello následující kroky:

1. V levém navigačním podokně hello, klikněte na **Procházet**.
2. Vyberte **Notification Hubs**a potom najít hello centra chcete toouse pro ukázku hello. Můžete se podívat toohello [Windows Store Začínáme kurzu](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Pokud potřebujete pomoc, vytváření nového centra oznámení.
3. Vyberte **nastavení**.
4. Klikněte na **zásady přístupu**. Zobrazí se oba připojovací řetězce sdílené a úplný přístup.

![Portál Azure – centra oznámení](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> Můžete také načíst hello připojovací řetězec pomocí hello **Get-AzureSbNamespace** rutiny poskytované [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo hello **azure sb obor názvů zobrazit** s Hello [rozhraní příkazového řádku Azure (Azure CLI)](../cli-install-nodejs.md).
> 
> 

## <a name="general-architecture"></a>Obecná architektura
Hello **NotificationHubService** objekt poskytuje hello následující instance objektů pro odesílání nabízených oznámení toospecific zařízení a aplikací:

* **Android** -použít hello **GcmService** objekt, který je k dispozici na **notificationHubService.gcm**
* **iOS** -použít hello **ApnsService** objekt, který je přístupná na **notificationHubService.apns**
* **Windows Phone** -použít hello **MpnsService** objekt, který je k dispozici na **notificationHubService.mpns**
* **Univerzální platformu Windows** -použít hello **WnsService** objekt, který je k dispozici na **notificationHubService.wns**

### <a name="how-to-send-push-notifications-tooandroid-applications"></a>Postupy: odeslání nabízených oznámení tooAndroid aplikace
Hello **GcmService** objekt poskytuje **odeslat** metoda, kterou lze použít toosend nabízená oznámení tooAndroid aplikace. Hello **odeslat** metoda přijímá hello následující parametry:

* **Značky** -hello identifikátor značky. Pokud je k dispozici žádná značka, hello odešle upozornění tooall klientů.
* **Datová část** -hello JSON nebo nezpracovaný řetězec datovou část zprávy.
* **Zpětné volání** -hello funkce zpětného volání.

Další informace o formát datové části hello najdete v tématu hello **datové části** části hello [implementace serveru GCM](http://developer.android.com/google/gcm/server.html#payload) dokumentu.

Hello následující kód používá hello **GcmService** instance vystavené hello **NotificationHubService** toosend nabízených oznámení tooall zaregistrovat klienty.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-tooios-applications"></a>Postupy: odeslání nabízených oznámení tooiOS aplikace
Stejné jako u aplikací pro Android popsané výše, hello **ApnsService** objekt poskytuje **odeslat** metoda, kterou lze použít toosend nabízená oznámení tooiOS aplikace. Hello **odeslat** metoda přijímá hello následující parametry:

* **Značky** -hello identifikátor značky. Pokud je k dispozici žádná značka, hello odešle upozornění tooall klientů.
* **Datová část** – hello JSON dané zprávy nebo string datové části.
* **Zpětné volání** -hello funkce zpětného volání.

Formát datové části hello Další informace naleznete v tématu hello **datová část oznámení** části hello [průvodci místních a nabízených oznámení programování](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentu.

Hello následující kód používá hello **ApnsService** instance vystavené hello **NotificationHubService** toosend výstrahu zprávy tooall klientů:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-toowindows-phone-applications"></a>Postupy: odeslání nabízených oznámení tooWindows telefonní aplikace
Hello **MpnsService** objekt poskytuje **odeslat** metoda, kterou lze použít toosend nabízená oznámení tooWindows telefonní aplikace. Hello **odeslat** metoda přijímá hello následující parametry:

* **Značky** -hello identifikátor značky. Pokud je k dispozici žádná značka, hello odešle upozornění tooall klientů.
* **Datová část** -hello datovou část zprávy XML.
* **TargetName**  -  `toast` pro informační zprávy. `token`pro dlaždici oznámení.
* **NotificationClass** -hello prioritu hello oznámení. V tématu hello **prvky záhlaví HTTP** části hello [nabízená oznámení ze serveru](http://msdn.microsoft.com/library/hh221551.aspx) dokumentu platné hodnoty.
* **Možnosti** – volitelné hlavičky požadavku.
* **Zpětné volání** -hello funkce zpětného volání.

Seznam platný **TargetName**, **NotificationClass** a možnosti hlaviček, podívejte se na hello [nabízená oznámení ze serveru](http://msdn.microsoft.com/library/hh221551.aspx) stránky.

Následující ukázkový kód Hello používá hello **MpnsService** instance vystavené hello **NotificationHubService** toosend nabízených oznámení:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-toouniversal-windows-platform-uwp-applications"></a>Postupy: odeslání nabízených oznámení tooUniversal platformu Windows (UWP) aplikací
Hello **WnsService** objekt poskytuje **odeslat** metoda, kterou lze použít toosend nabízená oznámení tooUniversal platformu Windows aplikace.  Hello **odeslat** metoda přijímá hello následující parametry:

* **Značky** -hello identifikátor značky. Pokud je k dispozici žádná značka, hello odešle upozornění tooall zaregistrovat klienty.
* **Datová část** -hello datovou část zprávy XML.
* **Typ** -hello typ oznámení.
* **Možnosti** – volitelné hlavičky požadavku.
* **Zpětné volání** -hello funkce zpětného volání.

Seznam platné typy a hlavičky požadavku, naleznete v části [nabízená oznámení hlavičkách žádostí a odpovědí služby](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Hello následující kód používá hello **WnsService** instance vystavené hello **NotificationHubService** toosend aplikací UWP tooa nabízená oznámení informační zprávy:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a>Další kroky
výše uvedené fragmenty ukázka Hello umožňují vám tooeasily sestavení služby infrastruktury toodeliver nabízená oznámení tooa širokou škálu zařízení. Teď, když jste se naučili základy používání centra oznámení s node.js hello, postupujte podle těchto odkazů toolearn informace o tom, jak můžete rozšířit tyto další možnosti.

* V tématu hello referenční dokumentace MSDN pro [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
* Navštivte hello [Azure SDK pro uzel] úložišti na Githubu Další ukázky a podrobnosti implementace.

[Azure SDK pro uzel]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain hello Default Management Credentials for hello Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application tooUse Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages tooa Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[Azure Classic Portal]: http://manage.windowsazure.com
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[Webový server pomocí služby WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
