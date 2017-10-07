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
# <a name="enterprise-push-architectural-guidance"></a><span data-ttu-id="0c6b9-103">Doprovodné materiály k architektuře nabízení v podnicích</span><span class="sxs-lookup"><span data-stu-id="0c6b9-103">Enterprise push architectural guidance</span></span>
<span data-ttu-id="0c6b9-104">Podniky jsou dnes postupně přesunutí směrem vytváření mobilních aplikací buď pro své koncové uživatele (externí) nebo pro zaměstnance hello (interní).</span><span class="sxs-lookup"><span data-stu-id="0c6b9-104">Enterprises today are gradually moving towards creating mobile applications for either their end users (external) or for hello employees (internal).</span></span> <span data-ttu-id="0c6b9-105">Mají existující back-end systémy zavedené je jej sálové počítače nebo některé obchodní aplikace, které musí být integrován do hello Architektura mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-105">They have existing backend systems in place be it mainframes or some LoB applications which must be integrated into hello mobile application architecture.</span></span> <span data-ttu-id="0c6b9-106">Tento průvodce bude komunikovat o tom, jak nejlepší toodo této integrace doporučujeme možných řešení toocommon scénáře.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-106">This guide will talk about how best toodo this integration recommending possible solution toocommon scenarios.</span></span>

<span data-ttu-id="0c6b9-107">Časté požadavek je pro odesílání nabízených oznámení uživateli toohello prostřednictvím jejich mobilních aplikací při výskytu události týkající se v systémech hello back-end.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-107">A frequent requirement is for sending push notification toohello users through their mobile application when an event of interest occurs in hello backend systems.</span></span> <span data-ttu-id="0c6b9-108">Například</span><span class="sxs-lookup"><span data-stu-id="0c6b9-108">E.g.</span></span> <span data-ttu-id="0c6b9-109">Banka zákazníkům, kteří používají hello bank bankovní aplikace na svém zařízení iPhone chce toobe upozorněni, když MD přišla nad určitou míru z svůj účet nebo kde zaměstnanci z finančního oddělení, který má aplikace schválení na nároky na jeho Windows Phone chce toobe scénáře intranetu upozorněni, když mu získá žádost o schválení.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-109">a bank customer who has hello bank's banking app on her iPhone wants toobe notified when a debit is made above a certain amount from her account or an intranet scenario where an employee from finance department who has a budget approval app on his Windows Phone wants toobe notified when he gets an approval request.</span></span>

<span data-ttu-id="0c6b9-110">Hello bankovní účet nebo zpracování schválení je pravděpodobně toobe v některé back-end systému, které musí spustit uživatel toohello push.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-110">hello bank account or approval processing is likely toobe done in some backend system which must initiate a push toohello user.</span></span> <span data-ttu-id="0c6b9-111">Může být více takové back-end systémy, které musí všechny sestavení hello stejný druh logiku tooimplement nabízené, když událost se aktivuje oznámení.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-111">There may be multiple such backend systems which must all build hello same kind of logic tooimplement push when an event triggers a notification.</span></span> <span data-ttu-id="0c6b9-112">složitost Hello zde spočívá v integraci několik systémů back-end společně s jeden nabízené systému, kde hello koncoví uživatelé mohou mít odběru oznámení toodifferent a i může být více mobilních aplikací, například v případě hello intranetu mobilních aplikací kde jeden mobilní aplikace může být vhodné tooreceive oznámení z více takového systému back-end.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-112">hello complexity here lies in integrating several backend systems together with a single push system where hello end users may have subscribed toodifferent notifications and there may even be multiple mobile applications e.g. in hello case of intranet mobile apps where one mobile application may want tooreceive notifications from multiple such backend systems.</span></span> <span data-ttu-id="0c6b9-113">Hello back-end systémy vědět, nebo potřebují tooknow nabízené sémantiku/technologie, aby se běžné řešení tradičně byl toointroduce komponentu, který posuzuje hello back-end systémy pro všechny události, které vás zajímají a je odpovědná za zasílání zpráv nabízené hello toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-113">hello backend systems do not know or need tooknow of push semantics/technology so a common solution here traditionally has been toointroduce a component which polls hello backend systems for any events of interest and is responsible for sending hello push messages toohello client.</span></span>
<span data-ttu-id="0c6b9-114">Zde se věnuje ještě lepší řešení pomocí Azure Service Bus - model téma/odběr, který se sníží složitost hello při vytváření škálovatelné řešení hello.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-114">Here we will talk about an even better solution using Azure Service Bus - Topic/Subscription model which will reduce hello complexity while making hello solution scalable.</span></span>

<span data-ttu-id="0c6b9-115">Tady je hello obecné architektuře řešení hello (zobecněný s více mobilních aplikací, ale platí po jenom jedna mobilní aplikace)</span><span class="sxs-lookup"><span data-stu-id="0c6b9-115">Here is hello general architecture of hello solution (generalized with multiple mobile apps but equally applicable when there is only one mobile app)</span></span>

## <a name="architecture"></a><span data-ttu-id="0c6b9-116">Architektura</span><span class="sxs-lookup"><span data-stu-id="0c6b9-116">Architecture</span></span>
![][1]

<span data-ttu-id="0c6b9-117">část klíče Hello do tohoto diagramu, architektury je Azure Service Bus, která poskytuje témata nebo odběry programovací model (Další informace o jeho v [Service Bus Pub nebo Sub programování]).</span><span class="sxs-lookup"><span data-stu-id="0c6b9-117">hello key piece in this architectural diagram is Azure Service Bus which provides a topics/subscriptions programming model (more on it at [Service Bus Pub/Sub programming]).</span></span> <span data-ttu-id="0c6b9-118">Hello příjemce, který v tomto případě je mobilního back-endu hello (obvykle [Azure Mobile Service], která zahájí nabízené mobilní aplikace toohello) nejsou doručovány zprávy přímo z hello back-end systémy, ale místo toho máme zprostředkující abstraktní vrstvu poskytované [Azure Service Bus] což umožňuje mobilního back-endu tooreceive zprávy z jednoho nebo více systémů back-end.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-118">hello receiver, which in this case, is hello Mobile backend (typically [Azure Mobile Service], which will initiate a push toohello mobile apps) does not receive messages directly from hello backend systems but instead we have an intermediate abstraction layer provided by [Azure Service Bus] which enables mobile backend tooreceive messages from one or more backend systems.</span></span> <span data-ttu-id="0c6b9-119">Téma sběrnice musí toobe vytvořené pro každou z hello back-end systémy například účet oddělení lidských zdrojů, Finance, které jsou v podstatě "témata", které vás zajímají, která zahájí toobe zprávy odeslané jako nabízené oznámení.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-119">A Service Bus Topic needs toobe created for each of hello backend systems e.g. Account, HR, Finance which are basically "topics" of interest which will initiate messages toobe sent as push notification.</span></span> <span data-ttu-id="0c6b9-120">Hello back-end systémy bude odesílat zprávy toothese témata.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-120">hello backend systems will send messages toothese topics.</span></span> <span data-ttu-id="0c6b9-121">Back-end Mobile se mohou přihlásit tooone nebo více těchto témat vytvořením odběru služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-121">A Mobile Backend can subscribe tooone or more such topics by creating a Service Bus subscription.</span></span> <span data-ttu-id="0c6b9-122">To bude opravňují hello mobilního back-endu tooreceive oznámení z back-end systému odpovídající hello.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-122">This will entitle hello mobile backend tooreceive a notification from hello corresponding backend system.</span></span> <span data-ttu-id="0c6b9-123">Mobilní back-end pokračuje toolisten pro zprávy o svých předplatných a ihned po doručení zprávy změní zpět a odešle ji jako centra oznámení tooits oznámení.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-123">Mobile backend continues toolisten for messages on their subscriptions and as soon as a message arrives, it turns back and sends it as notification tooits notification hub.</span></span> <span data-ttu-id="0c6b9-124">Centra oznámení potom nakonec přináší hello zpráva toohello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-124">Notification hubs then eventually delivers hello message toohello mobile app.</span></span> <span data-ttu-id="0c6b9-125">Klíčové komponenty hello toosummarize máme:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-125">So toosummarize hello key components, we have:</span></span>

1. <span data-ttu-id="0c6b9-126">Back-end systémy (systémy LoB nebo starší)</span><span class="sxs-lookup"><span data-stu-id="0c6b9-126">Backend systems (LoB/Legacy systems)</span></span>
   * <span data-ttu-id="0c6b9-127">Vytvoří téma sběrnice</span><span class="sxs-lookup"><span data-stu-id="0c6b9-127">Creates Service Bus Topic</span></span>
   * <span data-ttu-id="0c6b9-128">Odešle zprávu</span><span class="sxs-lookup"><span data-stu-id="0c6b9-128">Sends Message</span></span>
2. <span data-ttu-id="0c6b9-129">Mobilní back-end</span><span class="sxs-lookup"><span data-stu-id="0c6b9-129">Mobile backend</span></span>
   * <span data-ttu-id="0c6b9-130">Vytvoří předplatné služby</span><span class="sxs-lookup"><span data-stu-id="0c6b9-130">Creates Service Subscription</span></span>
   * <span data-ttu-id="0c6b9-131">Přijme zprávu (z back-end systému)</span><span class="sxs-lookup"><span data-stu-id="0c6b9-131">Receives Message (from Backend system)</span></span>
   * <span data-ttu-id="0c6b9-132">Odešle oznámení tooclients (prostřednictvím centra oznámení Azure)</span><span class="sxs-lookup"><span data-stu-id="0c6b9-132">Sends notification tooclients (via Azure Notification Hub)</span></span>
3. <span data-ttu-id="0c6b9-133">Mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="0c6b9-133">Mobile Application</span></span>
   * <span data-ttu-id="0c6b9-134">Přijímá a zobrazovat oznámení</span><span class="sxs-lookup"><span data-stu-id="0c6b9-134">Receives and display notification</span></span>

### <a name="benefits"></a><span data-ttu-id="0c6b9-135">Výhody:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-135">Benefits:</span></span>
1. <span data-ttu-id="0c6b9-136">Hello oddělení mezi hello příjemce (mobilní aplikaci nebo službě prostřednictvím centra oznámení) a odesílatele (back-end systémy) umožňuje další back-end systémy, kterou je prováděna integrace s minimální změny.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-136">hello decoupling between hello receiver (mobile app/service via Notification Hub) and sender (backend systems) enables additional backend systems being integrated with minimal change.</span></span>
2. <span data-ttu-id="0c6b9-137">Také díky tomu hello scénář více mobilních aplikací, které je možné tooreceive události z jednoho nebo více systémů back-end.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-137">This also makes hello scenario of multiple mobile apps being able tooreceive events from one or more backend systems.</span></span>  

## <a name="sample"></a><span data-ttu-id="0c6b9-138">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-138">Sample:</span></span>
### <a name="prerequisites"></a><span data-ttu-id="0c6b9-139">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0c6b9-139">Prerequisites</span></span>
<span data-ttu-id="0c6b9-140">Musíte provést následující kurzy toofamiliarize s koncepty hello, jakož i běžné kroky pro vytvoření a konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-140">You should complete hello following tutorials toofamiliarize with hello concepts as well as common creation & configuration steps:</span></span>

1. <span data-ttu-id="0c6b9-141">[Service Bus Pub nebo Sub programování] – Tato část popisuje hello podrobnosti o práci s Service Bus témata nebo předplatného, jak toocreate obor názvů toocontain témata nebo předplatného, jak toosend & příjem zpráv z nich.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-141">[Service Bus Pub/Sub programming] - This explains hello details of working with Service Bus Topics/Subscriptions, how toocreate a namespace toocontain topics/subscriptions, how toosend & receive messages from them.</span></span>
2. <span data-ttu-id="0c6b9-142">[Kurzu centra oznámení – Windows Universal] – Tato část popisuje, jak tooset až aplikace pro Windows Store a používat Notification Hubs tooregister a pak přijímat oznámení.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-142">[Notification Hubs - Windows Universal tutorial] - This explains how tooset up a Windows Store app and use Notification Hubs tooregister and then receive notifications.</span></span>

### <a name="sample-code"></a><span data-ttu-id="0c6b9-143">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="0c6b9-143">Sample code</span></span>
<span data-ttu-id="0c6b9-144">Hello úplný ukázkový kód je k dispozici na [ukázky centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="0c6b9-144">hello full sample code is available at [Notification Hub Samples].</span></span> <span data-ttu-id="0c6b9-145">Ho je rozdělená do tří součástí:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-145">It is split into three components:</span></span>

1. <span data-ttu-id="0c6b9-146">**EnterprisePushBackendSystem**</span><span class="sxs-lookup"><span data-stu-id="0c6b9-146">**EnterprisePushBackendSystem**</span></span>
   
    <span data-ttu-id="0c6b9-147">a.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-147">a.</span></span> <span data-ttu-id="0c6b9-148">Tento projekt používá hello *WindowsAzure.ServiceBus* balíček Nuget a je založena na [Service Bus Pub nebo Sub programování].</span><span class="sxs-lookup"><span data-stu-id="0c6b9-148">This project uses hello *WindowsAzure.ServiceBus* Nuget package and is  based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="0c6b9-149">b.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-149">b.</span></span> <span data-ttu-id="0c6b9-150">Toto je jednoduchý C# konzole aplikace toosimulate s LoB systémem, který zahájí toobe zpráva hello doručit toohello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-150">This is a simple C# console app toosimulate an LoB system which initiates hello message toobe delivered toohello mobile app.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    <span data-ttu-id="0c6b9-151">c.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-151">c.</span></span> <span data-ttu-id="0c6b9-152">`CreateTopic`je tématu Service Bus hello použité toocreate, kde bude odesílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-152">`CreateTopic` is used toocreate hello Service Bus topic where we will send messages.</span></span>
   
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
   
    <span data-ttu-id="0c6b9-153">d.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-153">d.</span></span> <span data-ttu-id="0c6b9-154">`SendMessage`je použité toosend hello zprávy toothis témata sběrnice.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-154">`SendMessage` is used toosend hello messages toothis Service Bus Topic.</span></span> <span data-ttu-id="0c6b9-155">Nemůžeme tady jednoduše odesílají sadu náhodné zprávy toohello tématu pravidelně za účelem hello hello ukázky.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-155">Here we are simply sending a set of random messages toohello topic periodically for hello purpose of hello sample.</span></span> <span data-ttu-id="0c6b9-156">Obvykle bude systém back-end, který bude odesílat zprávy, když dojde k události.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-156">Normally there will be a backend system which will send messages when an event occurs.</span></span>
   
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
2. <span data-ttu-id="0c6b9-157">**ReceiveAndSendNotification**</span><span class="sxs-lookup"><span data-stu-id="0c6b9-157">**ReceiveAndSendNotification**</span></span>
   
    <span data-ttu-id="0c6b9-158">a.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-158">a.</span></span> <span data-ttu-id="0c6b9-159">Tento projekt používá hello *WindowsAzure.ServiceBus* a *Microsoft.Web.WebJobs.Publish* Nuget balíčků a je založena na [Service Bus Pub nebo Sub programování].</span><span class="sxs-lookup"><span data-stu-id="0c6b9-159">This project uses hello *WindowsAzure.ServiceBus* and *Microsoft.Web.WebJobs.Publish* Nuget packages and is based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="0c6b9-160">b.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-160">b.</span></span> <span data-ttu-id="0c6b9-161">Toto je jiný C# konzole aplikace, která jsme se spustit jako [webové úlohy Azure] vzhledem k tomu, že má toorun nepřetržitě toolisten pro zprávy od hello LoB nebo back-end systémy.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-161">This is another C# console app which we will run as an [Azure WebJob] since it has toorun continuously toolisten for messages from hello LoB/backend systems.</span></span> <span data-ttu-id="0c6b9-162">Bude součástí mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-162">This will be part of your Mobile backend.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    <span data-ttu-id="0c6b9-163">c.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-163">c.</span></span> <span data-ttu-id="0c6b9-164">`CreateSubscription`předplatné služby Service Bus pro téma hello je použité toocreate, u kterých hello back-end systému bude odesílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-164">`CreateSubscription` is used toocreate a Service Bus subscription for hello topic where hello backend system will send messages.</span></span> <span data-ttu-id="0c6b9-165">V závislosti na scénáři hello firmy tato součást bude vytvořit jeden nebo více odběrů toocorresponding témata (například některé může být přijímání zpráv ze systému oddělení lidských zdrojů, některé z finančního systému a tak dále)</span><span class="sxs-lookup"><span data-stu-id="0c6b9-165">Depending on hello business scenario, this component will create one or more subscriptions toocorresponding topics (e.g. some may be receiving messages from HR system, some from Finance system, and so on)</span></span>
   
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
   
    <span data-ttu-id="0c6b9-166">d.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-166">d.</span></span> <span data-ttu-id="0c6b9-167">ReceiveMessageAndSendNotification je použité tooread uvítací zprávu z tématu hello pomocí svého předplatného a je-li hello číst úspěšně vytvořit toohello toobe odeslat oznámení (v ukázkovém scénáři hello Windows nativní informační), která je mobilní aplikace pomocí Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-167">ReceiveMessageAndSendNotification is used tooread hello message from hello topic using its subscription and if hello read is successful then craft a notification (in hello sample scenario a Windows native toast notification) toobe sent toohello mobile application using Azure Notification Hubs.</span></span>
   
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
   
    <span data-ttu-id="0c6b9-168">e.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-168">e.</span></span> <span data-ttu-id="0c6b9-169">Pro publikování jako **webové úlohy**, klikněte pravým tlačítkem na hello řešení v sadě Visual Studio a vyberte **publikovat jako webová úloha**</span><span class="sxs-lookup"><span data-stu-id="0c6b9-169">For publishing this as a **WebJob**, right click on hello solution in Visual Studio and select **Publish as WebJob**</span></span>
   
    ![][2]
   
    <span data-ttu-id="0c6b9-170">f.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-170">f.</span></span> <span data-ttu-id="0c6b9-171">Vyberte svůj profil publikování a vytvoření nového webu Azure, pokud ji už neexistuje, který bude hostitelem této webové úlohy, a až pak máte hello web **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-171">Select your publishing profile and create a new Azure WebSite if it doesnt exist already which will host this WebJob and once you have hello WebSite then **Publish**.</span></span>
   
    ![][3]
   
    <span data-ttu-id="0c6b9-172">g.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-172">g.</span></span> <span data-ttu-id="0c6b9-173">Nakonfigurujte hello úlohy "Spouštět nepřetržitě" toobe tak, aby při přihlášení toohello [portálu Azure Classic] měli byste vidět něco podobného jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-173">Configure hello job toobe "Run Continuously" so that when you log in toohello [Azure Classic Portal] you should see something like hello following:</span></span>
   
    ![][4]
3. <span data-ttu-id="0c6b9-174">**EnterprisePushMobileApp**</span><span class="sxs-lookup"><span data-stu-id="0c6b9-174">**EnterprisePushMobileApp**</span></span>
   
    <span data-ttu-id="0c6b9-175">a.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-175">a.</span></span> <span data-ttu-id="0c6b9-176">Toto je aplikace Windows Store, která bude přijímat oznámení informačního nápisu z hello webová úloha běží jako součást mobilního back-endu a zobrazit ji.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-176">This is a Windows Store application which will receive toast notifications from hello WebJob running as part of your Mobile backend and display it.</span></span> <span data-ttu-id="0c6b9-177">To je založené na [kurzu centra oznámení – Windows Universal].</span><span class="sxs-lookup"><span data-stu-id="0c6b9-177">This is based on [Notification Hubs - Windows Universal tutorial].</span></span>  
   
    <span data-ttu-id="0c6b9-178">b.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-178">b.</span></span> <span data-ttu-id="0c6b9-179">Zajistěte, aby vaše aplikace povoleno tooreceive informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-179">Ensure that your application is enabled tooreceive toast notifications.</span></span>
   
    <span data-ttu-id="0c6b9-180">c.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-180">c.</span></span> <span data-ttu-id="0c6b9-181">Ujistěte se, že hello následující kód registrace Notification Hubs je volána při spuštění aplikace hello (po nahrazení hello *HubName* a *DefaultListenSharedAccessSignature*:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-181">Ensure that hello following Notification Hubs registration code is being called at hello App start up (after replacing hello *HubName* and *DefaultListenSharedAccessSignature*:</span></span>
   
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

### <a name="running-sample"></a><span data-ttu-id="0c6b9-182">Spuštění ukázkové:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-182">Running sample:</span></span>
1. <span data-ttu-id="0c6b9-183">Zajistěte, aby vaše webová úloha pracuje správně a naplánováno příliš "spouštět nepřetržitě".</span><span class="sxs-lookup"><span data-stu-id="0c6b9-183">Ensure that your WebJob is running successfully and scheduled too"Run Continuously".</span></span>
2. <span data-ttu-id="0c6b9-184">Spustit hello **EnterprisePushMobileApp** který se spustí aplikace pro Windows Store hello.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-184">Run hello **EnterprisePushMobileApp** which will start hello Windows Store app.</span></span>
3. <span data-ttu-id="0c6b9-185">Spustit hello **EnterprisePushBackendSystem** konzolovou aplikaci, která bude simulovat hello LoB back-end a spustí odesílání zpráv a měli byste vidět informační zprávy zobrazí jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-185">Run hello **EnterprisePushBackendSystem** console application which will simulate hello LoB backend and will start sending messages and you should see toast notifications appearing like hello following:</span></span>
   
    ![][5]
4. <span data-ttu-id="0c6b9-186">témata sběrnice tooService, které byl monitorován odběry služby Service Bus ve webové úlohy původně odeslaných zpráv Hello.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-186">hello messages were originally sent tooService Bus topics which was being monitored by Service Bus subscriptions in your Web Job.</span></span> <span data-ttu-id="0c6b9-187">Jakmile byla přijata zpráva, oznámení se vytváří a odesílají toohello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c6b9-187">Once a message was received, a notification was created and sent toohello mobile app.</span></span> <span data-ttu-id="0c6b9-188">Můžete zobrazit prostřednictvím hello webové úlohy protokoly tooconfirm hello zpracování při přechodu toohello protokoly na odkaz v [portálu Azure Classic] pro webové úlohy:</span><span class="sxs-lookup"><span data-stu-id="0c6b9-188">You can look through hello WebJob logs tooconfirm hello processing when you go toohello Logs link in [Azure Classic Portal] for your Web Job:</span></span>
   
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
