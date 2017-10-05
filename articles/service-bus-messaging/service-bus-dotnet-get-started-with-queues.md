---
title: "Začínáme s frontami služby Azure Service Bus | Dokumentace Microsoftu"
description: "Napíšeme konzolovou aplikaci C# využívající fronty zasílání zpráv služby Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/26/2017
ms.author: sethm
ms.openlocfilehash: 99a377db6341d90d263b98e14227db61dd9beabd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-service-bus-queues"></a><span data-ttu-id="33c4e-103">Začínáme s frontami služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="33c4e-103">Get started with Service Bus queues</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="33c4e-104">Co všechno zvládneme</span><span class="sxs-lookup"><span data-stu-id="33c4e-104">What will be accomplished</span></span>
<span data-ttu-id="33c4e-105">Tento kurz se zabývá následujícími kroky:</span><span class="sxs-lookup"><span data-stu-id="33c4e-105">This tutorial covers the following steps:</span></span>

1. <span data-ttu-id="33c4e-106">Pomocí webu Azure Portal vytvoříme obor názvů služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="33c4e-106">Create a Service Bus namespace, using the Azure portal.</span></span>
2. <span data-ttu-id="33c4e-107">Pomocí webu Azure Portal vytvoříme frontu služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="33c4e-107">Create a Service Bus queue, using the Azure portal.</span></span>
3. <span data-ttu-id="33c4e-108">Napíšeme konzolovou aplikaci pro odeslání zprávy.</span><span class="sxs-lookup"><span data-stu-id="33c4e-108">Write a console application to send a message.</span></span>
4. <span data-ttu-id="33c4e-109">Napíšeme konzolovou aplikaci pro příjem zpráv odeslaných v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="33c4e-109">Write a console application to receive the messages sent in the previous step.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33c4e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="33c4e-110">Prerequisites</span></span>
1. <span data-ttu-id="33c4e-111">[Visual Studio 2015 nebo vyšší](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="33c4e-111">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="33c4e-112">V příkladech v tomto kurzu se používá sada Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="33c4e-112">The examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="33c4e-113">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="33c4e-113">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a><span data-ttu-id="33c4e-114">1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="33c4e-114">1. Create a namespace using the Azure portal</span></span>
<span data-ttu-id="33c4e-115">Pokud už máte vytvořený obor názvů pro zasílání zpráv služby Service Bus, přejděte k části [Vytvoření fronty pomocí webu Azure Portal](#2-create-a-queue-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="33c4e-115">If you've already created a Service Bus Messaging namespace, jump to the [Create a queue using the Azure portal](#2-create-a-queue-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a><span data-ttu-id="33c4e-116">2. Vytvoření fronty pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="33c4e-116">2. Create a queue using the Azure portal</span></span>
<span data-ttu-id="33c4e-117">Pokud už máte frontu služby Service Bus vytvořenou, přejděte k části [Zasílání zpráv do fronty](#3-send-messages-to-the-queue).</span><span class="sxs-lookup"><span data-stu-id="33c4e-117">If you have already created a Service Bus queue, jump to the [Send messages to the queue](#3-send-messages-to-the-queue) section.</span></span>

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a><span data-ttu-id="33c4e-118">3. Zasílání zpráv do fronty</span><span class="sxs-lookup"><span data-stu-id="33c4e-118">3. Send messages to the queue</span></span>
<span data-ttu-id="33c4e-119">Abychom mohli do fronty odesílat zprávy, napíšeme v sadě Visual Studio konzolovou aplikaci v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="33c4e-119">To send messages to the queue, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="33c4e-120">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="33c4e-120">Create a console application</span></span>

<span data-ttu-id="33c4e-121">Spusťte sadu Visual Studio a vytvořte nový projekt **Aplikace konzoly (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="33c4e-121">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-the-service-bus-nuget-package"></a><span data-ttu-id="33c4e-122">Přidání balíčku Service Bus NuGet</span><span class="sxs-lookup"><span data-stu-id="33c4e-122">Add the Service Bus NuGet package</span></span>
1. <span data-ttu-id="33c4e-123">Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="33c4e-123">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="33c4e-124">Klikněte na kartu **Procházet**, vyhledejte **Microsoft Azure Service Bus** a pak vyberte položku **WindowsAzure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="33c4e-124">Click the **Browse** tab, search for **Microsoft Azure Service Bus**, and then select the **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="33c4e-125">Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="33c4e-125">Click **Install** to complete the installation, then close this dialog box.</span></span>
   
    ![Výběr balíčku NuGet][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a><span data-ttu-id="33c4e-127">Napsání kódu pro zaslání zprávy do fronty</span><span class="sxs-lookup"><span data-stu-id="33c4e-127">Write some code to send a message to the queue</span></span>
1. <span data-ttu-id="33c4e-128">Na začátek souboru Program.cs přidejte následující příkaz `using`.</span><span class="sxs-lookup"><span data-stu-id="33c4e-128">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="33c4e-129">Do metody `Main` přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="33c4e-129">Add the following code to the `Main` method.</span></span> <span data-ttu-id="33c4e-130">Nastavte proměnnou `connectionString` na připojovací řetězec, který jste získali při vytváření oboru názvů, a proměnnou `queueName` nastavte na název, který jste použili při vytváření fronty.</span><span class="sxs-lookup"><span data-stu-id="33c4e-130">Set the `connectionString` variable to the connection string that you obtained when creating the namespace, and set `queueName` to the queue name that you used when creating the queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="33c4e-131">Soubor Program.cs by měl vypadat takhle.</span><span class="sxs-lookup"><span data-stu-id="33c4e-131">Here is what your Program.cs file should look like.</span></span>
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="33c4e-132">Spusťte program a podívejte se na web Azure Portal: klikněte na název vaší fronty v okně **Přehled** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="33c4e-132">Run the program, and check the Azure portal: click the name of your queue in the namespace **Overview** blade.</span></span> <span data-ttu-id="33c4e-133">Zobrazí se okno **Základy** fronty.</span><span class="sxs-lookup"><span data-stu-id="33c4e-133">The queue **Essentials** blade is displayed.</span></span> <span data-ttu-id="33c4e-134">Všimněte si, že hodnota **Počet aktivních zpráv** by měla nyní být 1.</span><span class="sxs-lookup"><span data-stu-id="33c4e-134">Notice that the **Active Message Count** value should now be 1.</span></span> <span data-ttu-id="33c4e-135">Pokaždé, když spustíte aplikaci odesílatele bez načtení zpráv, se tato hodnota zvýší o 1.</span><span class="sxs-lookup"><span data-stu-id="33c4e-135">Each time you run the sender application without retrieving the messages, this value increases by 1.</span></span> <span data-ttu-id="33c4e-136">Všimněte si také, že se pokaždé, když aplikace přidá zprávu do fronty, zvětší aktuální velikost fronty.</span><span class="sxs-lookup"><span data-stu-id="33c4e-136">Also note that the current size of the queue increments each time the app adds a message to the queue.</span></span>
   
      ![Velikost zpráv][queue-message]

## <a name="4-receive-messages-from-the-queue"></a><span data-ttu-id="33c4e-138">4. Přijetí zpráv z fronty</span><span class="sxs-lookup"><span data-stu-id="33c4e-138">4. Receive messages from the queue</span></span>

1. <span data-ttu-id="33c4e-139">Pokud chcete přijímat zprávy, které jste právě odeslali, vytvořte novou konzolovou aplikaci a přidejte odkaz na balíček NuGet služby Service Bus, podobně jako předtím u aplikace odesílatele.</span><span class="sxs-lookup"><span data-stu-id="33c4e-139">To receive the messages you just sent, create a new console application and add a reference to the Service Bus NuGet package, similar to the previous sender application.</span></span>
2. <span data-ttu-id="33c4e-140">Na začátek souboru Program.cs přidejte následující příkaz `using`.</span><span class="sxs-lookup"><span data-stu-id="33c4e-140">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="33c4e-141">Do metody `Main` přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="33c4e-141">Add the following code to the `Main` method.</span></span> <span data-ttu-id="33c4e-142">Nastavte proměnnou `connectionString` na připojovací řetězec, který jste získali při vytváření oboru názvů, a proměnnou `queueName` nastavte na název, který jste použili při vytváření fronty.</span><span class="sxs-lookup"><span data-stu-id="33c4e-142">Set the `connectionString` variable to the connection string that was obtained when creating the namespace, and set `queueName` to the queue name that you used when creating the queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="33c4e-143">Soubor Program.cs by měl vypadat takhle:</span><span class="sxs-lookup"><span data-stu-id="33c4e-143">Here is what your Program.cs file should look like:</span></span>
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
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
4. <span data-ttu-id="33c4e-144">Spusťte program a znovu se podívejte na portál.</span><span class="sxs-lookup"><span data-stu-id="33c4e-144">Run the program, and check the portal again.</span></span> <span data-ttu-id="33c4e-145">Všimněte si, že hodnoty **Počet aktivních zpráv** a **Aktuální** jsou nyní 0.</span><span class="sxs-lookup"><span data-stu-id="33c4e-145">Notice that the **Active Message Count** and **Current** values are now 0.</span></span>
   
    ![Délka fronty][queue-message-receive]

<span data-ttu-id="33c4e-147">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="33c4e-147">Congratulations!</span></span> <span data-ttu-id="33c4e-148">Vytvořili jste frontu, zaslali jste zprávu a přijali jste zprávu.</span><span class="sxs-lookup"><span data-stu-id="33c4e-148">You have now created a queue, sent a message, and received a message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33c4e-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33c4e-149">Next steps</span></span>

<span data-ttu-id="33c4e-150">Podívejte se na naše [úložiště GitHub s ukázkami](https://github.com/Azure/azure-service-bus/tree/master/samples), které předvádějí některé pokročilejší funkce zasílání zpráv služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="33c4e-150">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of the more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
