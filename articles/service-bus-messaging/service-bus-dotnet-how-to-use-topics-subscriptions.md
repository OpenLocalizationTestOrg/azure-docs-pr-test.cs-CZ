---
title: "Začínáme s tématy a odběry služby Azure Service Bus | Dokumentace Microsoftu"
description: "Napíšeme aplikaci konzoly v jazyce C# využívající témata a odběry zasílání zpráv služby Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.openlocfilehash: 9401ada519f600b0d2817f06a396e16607a24129
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-service-bus-topics"></a><span data-ttu-id="5364b-103">Začínáme s tématy služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="5364b-103">Get started with Service Bus topics</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="5364b-104">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="5364b-104">What will be accomplished</span></span>

<span data-ttu-id="5364b-105">Tento kurz se zabývá následujícími kroky:</span><span class="sxs-lookup"><span data-stu-id="5364b-105">This tutorial covers the following steps:</span></span>

1. <span data-ttu-id="5364b-106">Pomocí webu Azure Portal vytvoříme obor názvů služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="5364b-106">Create a Service Bus namespace, using the Azure portal.</span></span>
2. <span data-ttu-id="5364b-107">Pomocí webu Azure Portal vytvoříme téma služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="5364b-107">Create a Service Bus topic, using the Azure portal.</span></span>
3. <span data-ttu-id="5364b-108">Pomocí webu Azure Portal vytvoříme k tomuto tématu odběr služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="5364b-108">Create a Service Bus subscription to that topic, using the Azure portal.</span></span>
4. <span data-ttu-id="5364b-109">Napíšeme aplikaci konzoly pro odeslání zprávy do tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="5364b-109">Write a console application to send a message to the topic.</span></span>
5. <span data-ttu-id="5364b-110">Napíšeme aplikaci konzoly pro příjem této zprávy z odběru.</span><span class="sxs-lookup"><span data-stu-id="5364b-110">Write a console application to receive that message from the subscription.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5364b-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5364b-111">Prerequisites</span></span>

1. <span data-ttu-id="5364b-112">[Visual Studio 2015 nebo vyšší](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="5364b-112">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="5364b-113">V příkladech v tomto kurzu se používá sada Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="5364b-113">The examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="5364b-114">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5364b-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a><span data-ttu-id="5364b-115">1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5364b-115">1. Create a namespace using the Azure portal</span></span>

<span data-ttu-id="5364b-116">Pokud už máte vytvořený obor názvů pro zasílání zpráv služby Service Bus, přejděte k části [Vytvoření tématu pomocí webu Azure Portal](#2-create-a-topic-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="5364b-116">If you have already created a Service Bus Messaging namespace, jump to the [Create a topic using the Azure portal](#2-create-a-topic-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a><span data-ttu-id="5364b-117">2. Vytvoření tématu pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5364b-117">2. Create a topic using the Azure portal</span></span>

1. <span data-ttu-id="5364b-118">Přihlaste se k webu [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="5364b-118">Log on to the [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="5364b-119">V levém navigačním podokně portálu klikněte na **Service Bus** (pokud položku **Service Bus** nevidíte, klikněte na **Další služby**).</span><span class="sxs-lookup"><span data-stu-id="5364b-119">In the left navigation pane of the portal, click **Service Bus** (if you don't see **Service Bus**, click **More services**).</span></span>
3. <span data-ttu-id="5364b-120">Klikněte na obor názvů, ve kterém chcete vytvořit téma.</span><span class="sxs-lookup"><span data-stu-id="5364b-120">Click the namespace in which you would like to create the topic.</span></span> <span data-ttu-id="5364b-121">Zobrazí se okno přehledu oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="5364b-121">The namespace overview blade appears:</span></span>
   
    ![Vytvoření tématu][createtopic1]
4. <span data-ttu-id="5364b-123">V okně **Obor názvů služby Service Bus** klikněte na **Témata** a pak na **Přidat téma**.</span><span class="sxs-lookup"><span data-stu-id="5364b-123">In the **Service Bus namespace** blade, click **Topics**, then click **Add topic**.</span></span>
   
    ![Výběr témat][createtopic2]
5. <span data-ttu-id="5364b-125">Zadejte název tématu a zrušte zaškrtnutí možnosti **Povolit dělení**.</span><span class="sxs-lookup"><span data-stu-id="5364b-125">Enter a name for the topic, and uncheck the **Enable partitioning** option.</span></span> <span data-ttu-id="5364b-126">U ostatních možností ponechte jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5364b-126">Leave the other options with their default values.</span></span>
   
    ![Vyberte Nový][createtopic3]
6. <span data-ttu-id="5364b-128">Dole na v okně klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5364b-128">At the bottom of the blade, click **Create**.</span></span>

## <a name="3-create-a-subscription-to-the-topic"></a><span data-ttu-id="5364b-129">3. Vytvoření odběru tématu</span><span class="sxs-lookup"><span data-stu-id="5364b-129">3. Create a subscription to the topic</span></span>

1. <span data-ttu-id="5364b-130">V podokně prostředků na portálu klikněte na obor názvů, který jste vytvořili v kroku 1, a pak klikněte na téma, které jste vytvořili v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="5364b-130">In the portal resources pane, click the namespace you created in step 1, then click name of the topic you created in step 2.</span></span>
2. <span data-ttu-id="5364b-131">V horní části podokna přehledu kliknutím na symbol plus vedle možnosti **Odběr** přidejte odběr tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="5364b-131">A the top of the overview pane, click the plus sign next to **Subscription** to add a subscription to this topic.</span></span>

    ![Vytvoření odběru][createtopic4]

3. <span data-ttu-id="5364b-133">Zadejte název odběru.</span><span class="sxs-lookup"><span data-stu-id="5364b-133">Enter a name for the subscription.</span></span> <span data-ttu-id="5364b-134">U ostatních možností ponechte jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5364b-134">Leave the other options with their default values.</span></span>

## <a name="4-send-messages-to-the-topic"></a><span data-ttu-id="5364b-135">4. Odesílání zpráv do tématu</span><span class="sxs-lookup"><span data-stu-id="5364b-135">4. Send messages to the topic</span></span>

<span data-ttu-id="5364b-136">Pro odesílání zpráv do tématu napíšeme pomocí sady Visual Studio aplikaci konzoly v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="5364b-136">To send messages to the topic, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="5364b-137">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="5364b-137">Create a console application</span></span>

<span data-ttu-id="5364b-138">Spusťte sadu Visual Studio a vytvořte nový projekt **Aplikace konzoly (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="5364b-138">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-the-service-bus-nuget-package"></a><span data-ttu-id="5364b-139">Přidání balíčku Service Bus NuGet</span><span class="sxs-lookup"><span data-stu-id="5364b-139">Add the Service Bus NuGet package</span></span>

1. <span data-ttu-id="5364b-140">Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5364b-140">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="5364b-141">Klikněte na kartu **Procházet**, vyhledejte **Microsoft Azure Service Bus** a pak vyberte položku **WindowsAzure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="5364b-141">Click the **Browse** tab, search for **Microsoft Azure Service Bus**, and then select the **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="5364b-142">Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5364b-142">Click **Install** to complete the installation, then close this dialog box.</span></span>
   
    ![Výběr balíčku NuGet][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-topic"></a><span data-ttu-id="5364b-144">Napsání kódu pro odeslání zprávy do tématu</span><span class="sxs-lookup"><span data-stu-id="5364b-144">Write some code to send a message to the topic</span></span>

1. <span data-ttu-id="5364b-145">Na začátek souboru Program.cs přidejte následující příkaz `using`.</span><span class="sxs-lookup"><span data-stu-id="5364b-145">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="5364b-146">Do metody `Main` přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="5364b-146">Add the following code to the `Main` method.</span></span> <span data-ttu-id="5364b-147">Nastavte proměnnou `connectionString` na připojovací řetězec, který jste získali při vytváření oboru názvů, a proměnnou `topicName` nastavte na název, který jste použili při vytváření tématu.</span><span class="sxs-lookup"><span data-stu-id="5364b-147">Set the `connectionString` variable to the connection string that you obtained when creating the namespace, and set `topicName` to the name that you used when creating the topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="5364b-148">Soubor Program.cs by měl vypadat takhle.</span><span class="sxs-lookup"><span data-stu-id="5364b-148">Here is what your Program.cs file should look like.</span></span>
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="5364b-149">Spusťte program a podívejte se na web Azure Portal: klikněte na název vašeho tématu v okně **Přehled** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5364b-149">Run the program, and check the Azure portal: click the name of your topic in the namespace **Overview** blade.</span></span> <span data-ttu-id="5364b-150">Zobrazí se okno **Základy** tématu.</span><span class="sxs-lookup"><span data-stu-id="5364b-150">The topic **Essentials** blade is displayed.</span></span> <span data-ttu-id="5364b-151">Všimněte si, že v odběrech uvedených v dolní části okna by hodnota **Počet zpráv** měla být u každého odběru 1.</span><span class="sxs-lookup"><span data-stu-id="5364b-151">In the subscription(s) listed near the bottom of the blade, notice that the **Message Count** value for each subscription should now be 1.</span></span> <span data-ttu-id="5364b-152">Pokaždé, když spustíte aplikaci odesílatele bez načtení zpráv (jak je popsáno v další části), se tato hodnota zvýší o 1.</span><span class="sxs-lookup"><span data-stu-id="5364b-152">Each time you run the sender application without retrieving the messages (as described in the next section), this value increases by 1.</span></span> <span data-ttu-id="5364b-153">Všimněte si také, že aktuální velikost tématu navyšuje hodnotu **Aktuální** v okně **Základy** pokaždé, když aplikace do daného tématu nebo odběru přidá zprávu.</span><span class="sxs-lookup"><span data-stu-id="5364b-153">Also note that the current size of the topic increments the **Current** value on the **Essentials** blade each time the app adds a message to the topic/subscription.</span></span>
   
      ![Velikost zpráv][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a><span data-ttu-id="5364b-155">5. Příjem zpráv z odběru</span><span class="sxs-lookup"><span data-stu-id="5364b-155">5. Receive messages from the subscription</span></span>

1. <span data-ttu-id="5364b-156">Pokud chcete přijímat zprávy, které jste právě odeslali, vytvořte novou aplikaci konzoly a přidejte odkaz na balíček NuGet služby Service Bus, podobně jako předtím u aplikace odesílatele.</span><span class="sxs-lookup"><span data-stu-id="5364b-156">To receive the message or messages you just sent, create a new console application and add a reference to the Service Bus NuGet package, similar to the previous sender application.</span></span>
2. <span data-ttu-id="5364b-157">Na začátek souboru Program.cs přidejte následující příkaz `using`.</span><span class="sxs-lookup"><span data-stu-id="5364b-157">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="5364b-158">Do metody `Main` přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="5364b-158">Add the following code to the `Main` method.</span></span> <span data-ttu-id="5364b-159">Nastavte proměnnou `connectionString` na připojovací řetězec, který jste získali při vytváření oboru názvů, a proměnnou `topicName` nastavte na název, který jste použili při vytváření tématu.</span><span class="sxs-lookup"><span data-stu-id="5364b-159">Set the `connectionString` variable to the connection string you obtained when creating the namespace, and set `topicName` to the name that you used when creating the topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="5364b-160">Soubor Program.cs by měl vypadat takhle:</span><span class="sxs-lookup"><span data-stu-id="5364b-160">Here is what your Program.cs file should look like:</span></span>
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. <span data-ttu-id="5364b-161">Spusťte program a znovu se podívejte na portál.</span><span class="sxs-lookup"><span data-stu-id="5364b-161">Run the program, and check the portal again.</span></span> <span data-ttu-id="5364b-162">Všimněte si, že hodnoty **Počet zpráv** a **Aktuální** jsou nyní 0.</span><span class="sxs-lookup"><span data-stu-id="5364b-162">Notice that the **Message Count** and **Current** values are now 0.</span></span>
   
    ![Délka tématu][topic-message-receive]

<span data-ttu-id="5364b-164">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="5364b-164">Congratulations!</span></span> <span data-ttu-id="5364b-165">Právě jste vytvořili téma a odběr, odeslali zprávu a přijali ji.</span><span class="sxs-lookup"><span data-stu-id="5364b-165">You have now created a topic and subscription, sent a message, and received that message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5364b-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5364b-166">Next steps</span></span>

<span data-ttu-id="5364b-167">Podívejte se na naše [úložiště GitHub s ukázkami](https://github.com/Azure/azure-service-bus/tree/master/samples), které předvádějí některé pokročilejší funkce zasílání zpráv služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="5364b-167">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of the more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
