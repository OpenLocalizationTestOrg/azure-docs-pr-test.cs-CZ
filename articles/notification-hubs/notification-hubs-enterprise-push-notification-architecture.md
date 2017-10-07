---
title: "aaaNotification centra – architektura Push Enterprise"
description: "Pokyny k používání Azure Notification Hubs v podnikovém prostředí"
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c3afb83de1ba0882bf99e10f38cca40cb42d07a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-push-architectural-guidance"></a>Doprovodné materiály k architektuře nabízení v podnicích
Podniky jsou dnes postupně přesunutí směrem vytváření mobilních aplikací buď pro své koncové uživatele (externí) nebo pro zaměstnance hello (interní). Mají existující back-end systémy zavedené je jej sálové počítače nebo některé obchodní aplikace, které musí být integrován do hello Architektura mobilní aplikace. Tento průvodce bude komunikovat o tom, jak nejlepší toodo této integrace doporučujeme možných řešení toocommon scénáře.

Časté požadavek je pro odesílání nabízených oznámení uživateli toohello prostřednictvím jejich mobilních aplikací při výskytu události týkající se v systémech hello back-end. Například Banka zákazníkům, kteří používají hello bank bankovní aplikace na svém zařízení iPhone chce toobe upozorněni, když MD přišla nad určitou míru z svůj účet nebo kde zaměstnanci z finančního oddělení, který má aplikace schválení na nároky na jeho Windows Phone chce toobe scénáře intranetu upozorněni, když mu získá žádost o schválení.

Hello bankovní účet nebo zpracování schválení je pravděpodobně toobe v některé back-end systému, které musí spustit uživatel toohello push. Může být více takové back-end systémy, které musí všechny sestavení hello stejný druh logiku tooimplement nabízené, když událost se aktivuje oznámení. složitost Hello zde spočívá v integraci několik systémů back-end společně s jeden nabízené systému, kde hello koncoví uživatelé mohou mít odběru oznámení toodifferent a i může být více mobilních aplikací, například v případě hello intranetu mobilních aplikací kde jeden mobilní aplikace může být vhodné tooreceive oznámení z více takového systému back-end. Hello back-end systémy vědět, nebo potřebují tooknow nabízené sémantiku/technologie, aby se běžné řešení tradičně byl toointroduce komponentu, který posuzuje hello back-end systémy pro všechny události, které vás zajímají a je odpovědná za zasílání zpráv nabízené hello toohello klienta.
Zde se věnuje ještě lepší řešení pomocí Azure Service Bus - model téma/odběr, který se sníží složitost hello při vytváření škálovatelné řešení hello.

Tady je hello obecné architektuře řešení hello (zobecněný s více mobilních aplikací, ale platí po jenom jedna mobilní aplikace)

## <a name="architecture"></a>Architektura
![][1]

část klíče Hello do tohoto diagramu, architektury je Azure Service Bus, která poskytuje témata nebo odběry programovací model (Další informace o jeho v [Service Bus Pub nebo Sub programování]). Hello příjemce, který v tomto případě je mobilního back-endu hello (obvykle [Azure Mobile Service], která zahájí nabízené mobilní aplikace toohello) nejsou doručovány zprávy přímo z hello back-end systémy, ale místo toho máme zprostředkující abstraktní vrstvu poskytované [Azure Service Bus] což umožňuje mobilního back-endu tooreceive zprávy z jednoho nebo více systémů back-end. Téma sběrnice musí toobe vytvořené pro každou z hello back-end systémy například účet oddělení lidských zdrojů, Finance, které jsou v podstatě "témata", které vás zajímají, která zahájí toobe zprávy odeslané jako nabízené oznámení. Hello back-end systémy bude odesílat zprávy toothese témata. Back-end Mobile se mohou přihlásit tooone nebo více těchto témat vytvořením odběru služby Service Bus. To bude opravňují hello mobilního back-endu tooreceive oznámení z back-end systému odpovídající hello. Mobilní back-end pokračuje toolisten pro zprávy o svých předplatných a ihned po doručení zprávy změní zpět a odešle ji jako centra oznámení tooits oznámení. Centra oznámení potom nakonec přináší hello zpráva toohello mobilní aplikace. Klíčové komponenty hello toosummarize máme:

1. Back-end systémy (systémy LoB nebo starší)
   * Vytvoří téma sběrnice
   * Odešle zprávu
2. Mobilní back-end
   * Vytvoří předplatné služby
   * Přijme zprávu (z back-end systému)
   * Odešle oznámení tooclients (prostřednictvím centra oznámení Azure)
3. Mobilní aplikace
   * Přijímá a zobrazovat oznámení

### <a name="benefits"></a>Výhody:
1. Hello oddělení mezi hello příjemce (mobilní aplikaci nebo službě prostřednictvím centra oznámení) a odesílatele (back-end systémy) umožňuje další back-end systémy, kterou je prováděna integrace s minimální změny.
2. Také díky tomu hello scénář více mobilních aplikací, které je možné tooreceive události z jednoho nebo více systémů back-end.  

## <a name="sample"></a>Ukázka:
### <a name="prerequisites"></a>Požadavky
Musíte provést následující kurzy toofamiliarize s koncepty hello, jakož i běžné kroky pro vytvoření a konfigurace hello:

1. [Service Bus Pub nebo Sub programování] – Tato část popisuje hello podrobnosti o práci s Service Bus témata nebo předplatného, jak toocreate obor názvů toocontain témata nebo předplatného, jak toosend & příjem zpráv z nich.
2. [Kurzu centra oznámení – Windows Universal] – Tato část popisuje, jak tooset až aplikace pro Windows Store a používat Notification Hubs tooregister a pak přijímat oznámení.

### <a name="sample-code"></a>Ukázka kódu
Hello úplný ukázkový kód je k dispozici na [ukázky centra oznámení]. Ho je rozdělená do tří součástí:

1. **EnterprisePushBackendSystem**
   
    a. Tento projekt používá hello *WindowsAzure.ServiceBus* balíček Nuget a je založena na [Service Bus Pub nebo Sub programování].
   
    b. Toto je jednoduchý C# konzole aplikace toosimulate s LoB systémem, který zahájí toobe zpráva hello doručit toohello mobilní aplikace.
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    c. `CreateTopic`je tématu Service Bus hello použité toocreate, kde bude odesílat zprávy.
   
        public static void CreateTopic(string connectionString)
        {
            // Create hello topic if it does not exist already
   
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }
   
    d. `SendMessage`je použité toosend hello zprávy toothis témata sběrnice. Nemůžeme tady jednoduše odesílají sadu náhodné zprávy toohello tématu pravidelně za účelem hello hello ukázky. Obvykle bude systém back-end, který bude odesílat zprávy, když dojde k události.
   
        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
   
            // Sends random messages every 10 seconds toohello topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched tooa different team."
            };
   
            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);
   
                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);
   
                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);
   
                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }
2. **ReceiveAndSendNotification**
   
    a. Tento projekt používá hello *WindowsAzure.ServiceBus* a *Microsoft.Web.WebJobs.Publish* Nuget balíčků a je založena na [Service Bus Pub nebo Sub programování].
   
    b. Toto je jiný C# konzole aplikace, která jsme se spustit jako [webové úlohy Azure] vzhledem k tomu, že má toorun nepřetržitě toolisten pro zprávy od hello LoB nebo back-end systémy. Bude součástí mobilního back-endu.
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    c. `CreateSubscription`předplatné služby Service Bus pro téma hello je použité toocreate, u kterých hello back-end systému bude odesílat zprávy. V závislosti na scénáři hello firmy tato součást bude vytvořit jeden nebo více odběrů toocorresponding témata (například některé může být přijímání zpráv ze systému oddělení lidských zdrojů, některé z finančního systému a tak dále)
   
        static void CreateSubscription(string connectionString)
        {
            // Create hello subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }
   
    d. ReceiveMessageAndSendNotification je použité tooread uvítací zprávu z tématu hello pomocí svého předplatného a je-li hello číst úspěšně vytvořit toohello toobe odeslat oznámení (v ukázkovém scénáři hello Windows nativní informační), která je mobilní aplikace pomocí Azure Notification Hubs.
   
        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize hello Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");
   
            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);
   
            Client.Receive();
   
            // Continuously process messages received from hello subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";
   
                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");
   
                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);
   
                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }
   
    e. Pro publikování jako **webové úlohy**, klikněte pravým tlačítkem na hello řešení v sadě Visual Studio a vyberte **publikovat jako webová úloha**
   
    ![][2]
   
    f. Vyberte svůj profil publikování a vytvoření nového webu Azure, pokud ji už neexistuje, který bude hostitelem této webové úlohy, a až pak máte hello web **publikovat**.
   
    ![][3]
   
    g. Nakonfigurujte hello úlohy "Spouštět nepřetržitě" toobe tak, aby při přihlášení toohello [portálu Azure Classic] měli byste vidět něco podobného jako hello následující:
   
    ![][4]
3. **EnterprisePushMobileApp**
   
    a. Toto je aplikace Windows Store, která bude přijímat oznámení informačního nápisu z hello webová úloha běží jako součást mobilního back-endu a zobrazit ji. To je založené na [kurzu centra oznámení – Windows Universal].  
   
    b. Zajistěte, aby vaše aplikace povoleno tooreceive informační zprávy.
   
    c. Ujistěte se, že hello následující kód registrace Notification Hubs je volána při spuštění aplikace hello (po nahrazení hello *HubName* a *DefaultListenSharedAccessSignature*:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Spuštění ukázkové:
1. Zajistěte, aby vaše webová úloha pracuje správně a naplánováno příliš "spouštět nepřetržitě".
2. Spustit hello **EnterprisePushMobileApp** který se spustí aplikace pro Windows Store hello.
3. Spustit hello **EnterprisePushBackendSystem** konzolovou aplikaci, která bude simulovat hello LoB back-end a spustí odesílání zpráv a měli byste vidět informační zprávy zobrazí jako hello následující:
   
    ![][5]
4. témata sběrnice tooService, které byl monitorován odběry služby Service Bus ve webové úlohy původně odeslaných zpráv Hello. Jakmile byla přijata zpráva, oznámení se vytváří a odesílají toohello mobilní aplikace. Můžete zobrazit prostřednictvím hello webové úlohy protokoly tooconfirm hello zpracování při přechodu toohello protokoly na odkaz v [portálu Azure Classic] pro webové úlohy:
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Ukázky centra oznámení]: https://github.com/Azure/azure-notificationhubs-samples
[Mobilní služby Azure]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub nebo Sub programování]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Webová úloha Azure]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Centra oznámení – kurz pro univerzální aplikace Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Portál Azure Classic]: https://manage.windowsazure.com/
