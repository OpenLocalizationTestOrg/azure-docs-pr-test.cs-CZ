---
title: "Architektura Push Notification Hubs – Enterprise"
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
ms.openlocfilehash: ae7c1c9644ecfe7fe4ad6e332cc0683a3b5df22f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enterprise-push-architectural-guidance"></a><span data-ttu-id="6657b-103">Doprovodné materiály k architektuře nabízení v podnicích</span><span class="sxs-lookup"><span data-stu-id="6657b-103">Enterprise push architectural guidance</span></span>
<span data-ttu-id="6657b-104">Podniky jsou dnes postupně přesunutí směrem vytváření mobilních aplikací buď pro své koncové uživatele (externí) nebo pro zaměstnance (interní).</span><span class="sxs-lookup"><span data-stu-id="6657b-104">Enterprises today are gradually moving towards creating mobile applications for either their end users (external) or for the employees (internal).</span></span> <span data-ttu-id="6657b-105">Mají existující back-end systémy zavedené je jej sálové počítače nebo některé obchodní aplikace, které musí být integrován do architektury mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="6657b-105">They have existing backend systems in place be it mainframes or some LoB applications which must be integrated into the mobile application architecture.</span></span> <span data-ttu-id="6657b-106">Tato příručka se mluvit o tom, jak vhodné provést této integrace doporučujeme možná řešení pro běžné scénáře.</span><span class="sxs-lookup"><span data-stu-id="6657b-106">This guide will talk about how best to do this integration recommending possible solution to common scenarios.</span></span>

<span data-ttu-id="6657b-107">Časté požadavek je pro odesílání nabízených oznámení uživatelům prostřednictvím jejich mobilních aplikací při výskytu události týkající se v systémech back-end.</span><span class="sxs-lookup"><span data-stu-id="6657b-107">A frequent requirement is for sending push notification to the users through their mobile application when an event of interest occurs in the backend systems.</span></span> <span data-ttu-id="6657b-108">Například</span><span class="sxs-lookup"><span data-stu-id="6657b-108">E.g.</span></span> <span data-ttu-id="6657b-109">Banka zákazníkovi, který má tato banka bankovní aplikace na svém zařízení iPhone chce být upozorněni, když MD přišla nad určitou míru z svůj účet nebo scénářem intranetu, kde zaměstnanci z finančního oddělení, který má aplikace schválení na nároky na jeho Windows Phone chce být upozorněni, když mu získá žádost o schválení.</span><span class="sxs-lookup"><span data-stu-id="6657b-109">a bank customer who has the bank's banking app on her iPhone wants to be notified when a debit is made above a certain amount from her account or an intranet scenario where an employee from finance department who has a budget approval app on his Windows Phone wants to be notified when he gets an approval request.</span></span>

<span data-ttu-id="6657b-110">Bankovní účet nebo zpracování schválení je pravděpodobné, v některých systém back-end, který musí inicializovat oznámení pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="6657b-110">The bank account or approval processing is likely to be done in some backend system which must initiate a push to the user.</span></span> <span data-ttu-id="6657b-111">Může existovat více takové back-end systémy, které musíte sestavit stejný druh logiku pro implementaci nabízená oznámení se aktivuje událost.</span><span class="sxs-lookup"><span data-stu-id="6657b-111">There may be multiple such backend systems which must all build the same kind of logic to implement push when an event triggers a notification.</span></span> <span data-ttu-id="6657b-112">Složitost zde spočívá v integraci několik systémů back-end společně s jeden nabízené systému, kde koncoví uživatelé mohou mít předplatné jiný oznámení a může i více mobilních aplikací, například v případě mobilních aplikací na intranetu ve může jeden mobilních aplikací chcete oznámení dostávat, více takového systému back-end.</span><span class="sxs-lookup"><span data-stu-id="6657b-112">The complexity here lies in integrating several backend systems together with a single push system where the end users may have subscribed to different notifications and there may even be multiple mobile applications e.g. in the case of intranet mobile apps where one mobile application may want to receive notifications from multiple such backend systems.</span></span> <span data-ttu-id="6657b-113">Back-end systémy neznáte nebo potřebujete vědět nabízené sémantiku/technologie tak běžné řešení tradičně byl zavádět komponentu, který posuzuje systémy back-end pro všechny události, které vás zajímají a je odpovědná za zasílání zpráv nabízené klientovi.</span><span class="sxs-lookup"><span data-stu-id="6657b-113">The backend systems do not know or need to know of push semantics/technology so a common solution here traditionally has been to introduce a component which polls the backend systems for any events of interest and is responsible for sending the push messages to the client.</span></span>
<span data-ttu-id="6657b-114">Zde se věnuje ještě lepší řešení pomocí Azure Service Bus - model téma/odběr, který se sníží složitost při vytváření škálovatelné řešení.</span><span class="sxs-lookup"><span data-stu-id="6657b-114">Here we will talk about an even better solution using Azure Service Bus - Topic/Subscription model which will reduce the complexity while making the solution scalable.</span></span>

<span data-ttu-id="6657b-115">Tady je obecné architektuře řešení (zobecněný s více mobilních aplikací, ale platí po jenom jedna mobilní aplikace)</span><span class="sxs-lookup"><span data-stu-id="6657b-115">Here is the general architecture of the solution (generalized with multiple mobile apps but equally applicable when there is only one mobile app)</span></span>

## <a name="architecture"></a><span data-ttu-id="6657b-116">Architektura</span><span class="sxs-lookup"><span data-stu-id="6657b-116">Architecture</span></span>
![][1]

<span data-ttu-id="6657b-117">Klíčovou do tohoto diagramu, architektury je Azure Service Bus, která poskytuje témata nebo odběry programovací model (Další informace o jeho v [Service Bus Pub nebo Sub programování]).</span><span class="sxs-lookup"><span data-stu-id="6657b-117">The key piece in this architectural diagram is Azure Service Bus which provides a topics/subscriptions programming model (more on it at [Service Bus Pub/Sub programming]).</span></span> <span data-ttu-id="6657b-118">Příjemce, který v tomto případě je mobilního back-endu (obvykle [Azure Mobile Service], která zahájí oznámení na mobilní aplikace) nejsou doručovány zprávy přímo z back-end systémy, ale místo toho máme zprostředkující abstraktní vrstvu poskytované [Azure Service Bus] což umožňuje mobilní back-end pro příjem zpráv z jednoho nebo více systémů back-end.</span><span class="sxs-lookup"><span data-stu-id="6657b-118">The receiver, which in this case, is the Mobile backend (typically [Azure Mobile Service], which will initiate a push to the mobile apps) does not receive messages directly from the backend systems but instead we have an intermediate abstraction layer provided by [Azure Service Bus] which enables mobile backend to receive messages from one or more backend systems.</span></span> <span data-ttu-id="6657b-119">Téma sběrnice je potřeba vytvořit pro každou z back-end systémy například účet oddělení lidských zdrojů, Finance, které jsou v podstatě "témata", které vás zajímají, která zahájí zpráv k odeslání jako nabízené oznámení.</span><span class="sxs-lookup"><span data-stu-id="6657b-119">A Service Bus Topic needs to be created for each of the backend systems e.g. Account, HR, Finance which are basically "topics" of interest which will initiate messages to be sent as push notification.</span></span> <span data-ttu-id="6657b-120">Back-end systémy bude odesílat zprávy na tato témata.</span><span class="sxs-lookup"><span data-stu-id="6657b-120">The backend systems will send messages to these topics.</span></span> <span data-ttu-id="6657b-121">Back-end Mobile se mohou přihlásit na jeden nebo více těchto témata vytvořením odběru služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6657b-121">A Mobile Backend can subscribe to one or more such topics by creating a Service Bus subscription.</span></span> <span data-ttu-id="6657b-122">To bude získat mobilní back-end pro příjem oznámení z back-end serveru odpovídající oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6657b-122">This will entitle the mobile backend to receive a notification from the corresponding backend system.</span></span> <span data-ttu-id="6657b-123">Mobilní back-end i nadále přijímat zprávy o svých předplatných a ihned po doručení zprávy změní zpět a odešle ji jako oznámení jeho centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="6657b-123">Mobile backend continues to listen for messages on their subscriptions and as soon as a message arrives, it turns back and sends it as notification to its notification hub.</span></span> <span data-ttu-id="6657b-124">Centra oznámení potom nakonec doručení zprávy do mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="6657b-124">Notification hubs then eventually delivers the message to the mobile app.</span></span> <span data-ttu-id="6657b-125">Proto to Shrneme klíčové komponenty, jsme provedli následující:</span><span class="sxs-lookup"><span data-stu-id="6657b-125">So to summarize the key components, we have:</span></span>

1. <span data-ttu-id="6657b-126">Back-end systémy (systémy LoB nebo starší)</span><span class="sxs-lookup"><span data-stu-id="6657b-126">Backend systems (LoB/Legacy systems)</span></span>
   * <span data-ttu-id="6657b-127">Vytvoří téma sběrnice</span><span class="sxs-lookup"><span data-stu-id="6657b-127">Creates Service Bus Topic</span></span>
   * <span data-ttu-id="6657b-128">Odešle zprávu</span><span class="sxs-lookup"><span data-stu-id="6657b-128">Sends Message</span></span>
2. <span data-ttu-id="6657b-129">Mobilní back-end</span><span class="sxs-lookup"><span data-stu-id="6657b-129">Mobile backend</span></span>
   * <span data-ttu-id="6657b-130">Vytvoří předplatné služby</span><span class="sxs-lookup"><span data-stu-id="6657b-130">Creates Service Subscription</span></span>
   * <span data-ttu-id="6657b-131">Přijme zprávu (z back-end systému)</span><span class="sxs-lookup"><span data-stu-id="6657b-131">Receives Message (from Backend system)</span></span>
   * <span data-ttu-id="6657b-132">Odešle oznámení klientům (prostřednictvím centra oznámení Azure)</span><span class="sxs-lookup"><span data-stu-id="6657b-132">Sends notification to clients (via Azure Notification Hub)</span></span>
3. <span data-ttu-id="6657b-133">Mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="6657b-133">Mobile Application</span></span>
   * <span data-ttu-id="6657b-134">Přijímá a zobrazovat oznámení</span><span class="sxs-lookup"><span data-stu-id="6657b-134">Receives and display notification</span></span>

### <a name="benefits"></a><span data-ttu-id="6657b-135">Výhody:</span><span class="sxs-lookup"><span data-stu-id="6657b-135">Benefits:</span></span>
1. <span data-ttu-id="6657b-136">Oddělení mezi příjemce (mobilní aplikaci nebo službě prostřednictvím centra oznámení) a odesílatele (back-end systémy) umožňuje další back-end systémy, kterou je prováděna integrace s minimální změny.</span><span class="sxs-lookup"><span data-stu-id="6657b-136">The decoupling between the receiver (mobile app/service via Notification Hub) and sender (backend systems) enables additional backend systems being integrated with minimal change.</span></span>
2. <span data-ttu-id="6657b-137">Také díky tomu scénář více mobilních aplikací, bude možné přijímat události z jednoho nebo více systémů back-end.</span><span class="sxs-lookup"><span data-stu-id="6657b-137">This also makes the scenario of multiple mobile apps being able to receive events from one or more backend systems.</span></span>  

## <a name="sample"></a><span data-ttu-id="6657b-138">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="6657b-138">Sample:</span></span>
### <a name="prerequisites"></a><span data-ttu-id="6657b-139">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6657b-139">Prerequisites</span></span>
<span data-ttu-id="6657b-140">Musíte provést následující kurzy seznamte s koncepty a také běžné vytvoření & kroky konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6657b-140">You should complete the following tutorials to familiarize with the concepts as well as common creation & configuration steps:</span></span>

1. <span data-ttu-id="6657b-141">[Service Bus Pub nebo Sub programování] – Tato část popisuje podrobnosti o práci s Service Bus témata nebo předplatných, postup vytvoření oboru názvů tak, aby obsahovala témata nebo předplatného, jak odesílat a přijímat zprávy z nich.</span><span class="sxs-lookup"><span data-stu-id="6657b-141">[Service Bus Pub/Sub programming] - This explains the details of working with Service Bus Topics/Subscriptions, how to create a namespace to contain topics/subscriptions, how to send & receive messages from them.</span></span>
2. <span data-ttu-id="6657b-142">[Kurzu centra oznámení – Windows Universal] – Tato část popisuje postup nastavení aplikace pro Windows Store a používat Notification Hubs k registraci a pak přijímat oznámení.</span><span class="sxs-lookup"><span data-stu-id="6657b-142">[Notification Hubs - Windows Universal tutorial] - This explains how to set up a Windows Store app and use Notification Hubs to register and then receive notifications.</span></span>

### <a name="sample-code"></a><span data-ttu-id="6657b-143">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="6657b-143">Sample code</span></span>
<span data-ttu-id="6657b-144">Úplný ukázkový kód je k dispozici na [ukázky centra oznámení].</span><span class="sxs-lookup"><span data-stu-id="6657b-144">The full sample code is available at [Notification Hub Samples].</span></span> <span data-ttu-id="6657b-145">Ho je rozdělená do tří součástí:</span><span class="sxs-lookup"><span data-stu-id="6657b-145">It is split into three components:</span></span>

1. <span data-ttu-id="6657b-146">**EnterprisePushBackendSystem**</span><span class="sxs-lookup"><span data-stu-id="6657b-146">**EnterprisePushBackendSystem**</span></span>
   
    <span data-ttu-id="6657b-147">a.</span><span class="sxs-lookup"><span data-stu-id="6657b-147">a.</span></span> <span data-ttu-id="6657b-148">Používá tento projekt *WindowsAzure.ServiceBus* balíček Nuget a je založena na [Service Bus Pub nebo Sub programování].</span><span class="sxs-lookup"><span data-stu-id="6657b-148">This project uses the *WindowsAzure.ServiceBus* Nuget package and is  based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="6657b-149">b.</span><span class="sxs-lookup"><span data-stu-id="6657b-149">b.</span></span> <span data-ttu-id="6657b-150">Toto je jednoduchá C# konzole aplikace k simulaci s LoB systémem, který zahájí zprávu, která se bude doručen do mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="6657b-150">This is a simple C# console app to simulate an LoB system which initiates the message to be delivered to the mobile app.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    <span data-ttu-id="6657b-151">c.</span><span class="sxs-lookup"><span data-stu-id="6657b-151">c.</span></span> <span data-ttu-id="6657b-152">`CreateTopic`se používá k vytvoření tématu Service Bus kde bude odesílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="6657b-152">`CreateTopic` is used to create the Service Bus topic where we will send messages.</span></span>
   
        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already
   
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }
   
    <span data-ttu-id="6657b-153">d.</span><span class="sxs-lookup"><span data-stu-id="6657b-153">d.</span></span> <span data-ttu-id="6657b-154">`SendMessage`se používá k odeslání zprávy do tohoto tématu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6657b-154">`SendMessage` is used to send the messages to this Service Bus Topic.</span></span> <span data-ttu-id="6657b-155">Zde jsme se jednoduše odeslat sadu náhodné zprávy do tématu pravidelně pro účely ukázky.</span><span class="sxs-lookup"><span data-stu-id="6657b-155">Here we are simply sending a set of random messages to the topic periodically for the purpose of the sample.</span></span> <span data-ttu-id="6657b-156">Obvykle bude systém back-end, který bude odesílat zprávy, když dojde k události.</span><span class="sxs-lookup"><span data-stu-id="6657b-156">Normally there will be a backend system which will send messages when an event occurs.</span></span>
   
        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
   
            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
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
2. <span data-ttu-id="6657b-157">**ReceiveAndSendNotification**</span><span class="sxs-lookup"><span data-stu-id="6657b-157">**ReceiveAndSendNotification**</span></span>
   
    <span data-ttu-id="6657b-158">a.</span><span class="sxs-lookup"><span data-stu-id="6657b-158">a.</span></span> <span data-ttu-id="6657b-159">Používá tento projekt *WindowsAzure.ServiceBus* a *Microsoft.Web.WebJobs.Publish* Nuget balíčků a je založena na [Service Bus Pub nebo Sub programování].</span><span class="sxs-lookup"><span data-stu-id="6657b-159">This project uses the *WindowsAzure.ServiceBus* and *Microsoft.Web.WebJobs.Publish* Nuget packages and is based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="6657b-160">b.</span><span class="sxs-lookup"><span data-stu-id="6657b-160">b.</span></span> <span data-ttu-id="6657b-161">Toto je jiný C# konzole aplikace, která jsme se spustit jako [webové úlohy Azure] vzhledem k tomu, že je spouštět nepřetržitě přijímat zprávy ze systémů LoB nebo back-end.</span><span class="sxs-lookup"><span data-stu-id="6657b-161">This is another C# console app which we will run as an [Azure WebJob] since it has to run continuously to listen for messages from the LoB/backend systems.</span></span> <span data-ttu-id="6657b-162">Bude součástí mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="6657b-162">This will be part of your Mobile backend.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    <span data-ttu-id="6657b-163">c.</span><span class="sxs-lookup"><span data-stu-id="6657b-163">c.</span></span> <span data-ttu-id="6657b-164">`CreateSubscription`slouží k vytvoření odběru služby Service Bus pro téma kde systému back-end bude odesílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="6657b-164">`CreateSubscription` is used to create a Service Bus subscription for the topic where the backend system will send messages.</span></span> <span data-ttu-id="6657b-165">V závislosti na podnikový scénář bude tato součást vytvořit jeden nebo více odběrů na odpovídající témata (například některé může být přijímání zpráv ze systému oddělení lidských zdrojů, některé z finančního systému a tak dále)</span><span class="sxs-lookup"><span data-stu-id="6657b-165">Depending on the business scenario, this component will create one or more subscriptions to corresponding topics (e.g. some may be receiving messages from HR system, some from Finance system, and so on)</span></span>
   
        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }
   
    <span data-ttu-id="6657b-166">d.</span><span class="sxs-lookup"><span data-stu-id="6657b-166">d.</span></span> <span data-ttu-id="6657b-167">ReceiveMessageAndSendNotification se používá k tuto zprávu přečíst v tématu pomocí svého předplatného a pokud čtení úspěšné potom vytvořit oznámení (v ukázkovém scénáři Windows nativní informační) k odeslání do mobilní aplikace pomocí Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="6657b-167">ReceiveMessageAndSendNotification is used to read the message from the topic using its subscription and if the read is successful then craft a notification (in the sample scenario a Windows native toast notification) to be sent to the mobile application using Azure Notification Hubs.</span></span>
   
        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");
   
            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);
   
            Client.Receive();
   
            // Continuously process messages received from the subscription
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
   
    <span data-ttu-id="6657b-168">e.</span><span class="sxs-lookup"><span data-stu-id="6657b-168">e.</span></span> <span data-ttu-id="6657b-169">Pro publikování jako **webové úlohy**, klikněte pravým tlačítkem na řešení v sadě Visual Studio a vyberte **publikovat jako webová úloha**</span><span class="sxs-lookup"><span data-stu-id="6657b-169">For publishing this as a **WebJob**, right click on the solution in Visual Studio and select **Publish as WebJob**</span></span>
   
    ![][2]
   
    <span data-ttu-id="6657b-170">f.</span><span class="sxs-lookup"><span data-stu-id="6657b-170">f.</span></span> <span data-ttu-id="6657b-171">Vyberte svůj profil publikování a vytvoření nového webu Azure, pokud ji už neexistuje, který bude hostitelem této webové úlohy, a až pak máte na webu **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="6657b-171">Select your publishing profile and create a new Azure WebSite if it doesnt exist already which will host this WebJob and once you have the WebSite then **Publish**.</span></span>
   
    ![][3]
   
    <span data-ttu-id="6657b-172">g.</span><span class="sxs-lookup"><span data-stu-id="6657b-172">g.</span></span> <span data-ttu-id="6657b-173">Nakonfigurujte úlohu, která má být "Spouštět nepřetržitě" tak, aby při přihlášení do [portálu Azure Classic] měli byste vidět nějak takto:</span><span class="sxs-lookup"><span data-stu-id="6657b-173">Configure the job to be "Run Continuously" so that when you log in to the [Azure Classic Portal] you should see something like the following:</span></span>
   
    ![][4]
3. <span data-ttu-id="6657b-174">**EnterprisePushMobileApp**</span><span class="sxs-lookup"><span data-stu-id="6657b-174">**EnterprisePushMobileApp**</span></span>
   
    <span data-ttu-id="6657b-175">a.</span><span class="sxs-lookup"><span data-stu-id="6657b-175">a.</span></span> <span data-ttu-id="6657b-176">Toto je aplikace Windows Store, která bude přijímat oznámení informačního nápisu z webová úloha spuštěna jako součást mobilního back-endu a zobrazit ji.</span><span class="sxs-lookup"><span data-stu-id="6657b-176">This is a Windows Store application which will receive toast notifications from the WebJob running as part of your Mobile backend and display it.</span></span> <span data-ttu-id="6657b-177">To je založené na [Kurzu centra oznámení – Windows Universal].</span><span class="sxs-lookup"><span data-stu-id="6657b-177">This is based on [Notification Hubs - Windows Universal tutorial].</span></span>  
   
    <span data-ttu-id="6657b-178">b.</span><span class="sxs-lookup"><span data-stu-id="6657b-178">b.</span></span> <span data-ttu-id="6657b-179">Ujistěte se, že aplikace je povoleno přijímat oznámení informačního nápisu.</span><span class="sxs-lookup"><span data-stu-id="6657b-179">Ensure that your application is enabled to receive toast notifications.</span></span>
   
    <span data-ttu-id="6657b-180">c.</span><span class="sxs-lookup"><span data-stu-id="6657b-180">c.</span></span> <span data-ttu-id="6657b-181">Ujistěte se, že následující Notification Hubs registrační kód je volána v aplikaci spuštění (po nahrazení *HubName* a *DefaultListenSharedAccessSignature*:</span><span class="sxs-lookup"><span data-stu-id="6657b-181">Ensure that the following Notification Hubs registration code is being called at the App start up (after replacing the *HubName* and *DefaultListenSharedAccessSignature*:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a><span data-ttu-id="6657b-182">Spuštění ukázkové:</span><span class="sxs-lookup"><span data-stu-id="6657b-182">Running sample:</span></span>
1. <span data-ttu-id="6657b-183">Zajistěte, aby vaše webová úloha pracuje správně a naplánované "Nepřetržitě spustit".</span><span class="sxs-lookup"><span data-stu-id="6657b-183">Ensure that your WebJob is running successfully and scheduled to "Run Continuously".</span></span>
2. <span data-ttu-id="6657b-184">Spustit **EnterprisePushMobileApp** který se spustí aplikace pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="6657b-184">Run the **EnterprisePushMobileApp** which will start the Windows Store app.</span></span>
3. <span data-ttu-id="6657b-185">Spustit **EnterprisePushBackendSystem** konzolovou aplikaci, která bude simulovat LoB back-end a spustí odesílání zpráv a měli byste vidět informační zprávy, které jsou uvedeny jako následující:</span><span class="sxs-lookup"><span data-stu-id="6657b-185">Run the **EnterprisePushBackendSystem** console application which will simulate the LoB backend and will start sending messages and you should see toast notifications appearing like the following:</span></span>
   
    ![][5]
4. <span data-ttu-id="6657b-186">Zprávy byly původně odeslána do témat Service Bus, které byl monitorován odběry služby Service Bus ve webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="6657b-186">The messages were originally sent to Service Bus topics which was being monitored by Service Bus subscriptions in your Web Job.</span></span> <span data-ttu-id="6657b-187">Jakmile byla přijata zpráva, oznámení se vytváří a odesílají do mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="6657b-187">Once a message was received, a notification was created and sent to the mobile app.</span></span> <span data-ttu-id="6657b-188">Můžete zobrazit prostřednictvím protokolů webové úlohy potvrďte zpracování při přechodu na odkaz protokoly v [portálu Azure Classic] pro webové úlohy:</span><span class="sxs-lookup"><span data-stu-id="6657b-188">You can look through the WebJob logs to confirm the processing when you go to the Logs link in [Azure Classic Portal] for your Web Job:</span></span>
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
<span data-ttu-id="6657b-189">[ukázky centra oznámení]: https://github.com/Azure/azure-notificationhubs-samples</span><span class="sxs-lookup"><span data-stu-id="6657b-189">[Notification Hub Samples]: https://github.com/Azure/azure-notificationhubs-samples</span></span>
<span data-ttu-id="6657b-190">[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/</span><span class="sxs-lookup"><span data-stu-id="6657b-190">[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/</span></span>
<span data-ttu-id="6657b-191">[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/</span><span class="sxs-lookup"><span data-stu-id="6657b-191">[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/</span></span>
<span data-ttu-id="6657b-192">[Service Bus Pub nebo Sub programování]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/</span><span class="sxs-lookup"><span data-stu-id="6657b-192">[Service Bus Pub/Sub programming]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/</span></span>
<span data-ttu-id="6657b-193">[webové úlohy Azure]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/</span><span class="sxs-lookup"><span data-stu-id="6657b-193">[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/</span></span>
<span data-ttu-id="6657b-194">[Kurzu centra oznámení – Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span><span class="sxs-lookup"><span data-stu-id="6657b-194">[Notification Hubs - Windows Universal tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span></span>
<span data-ttu-id="6657b-195">[portálu Azure Classic]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="6657b-195">[Azure Classic Portal]: https://manage.windowsazure.com/</span></span>
