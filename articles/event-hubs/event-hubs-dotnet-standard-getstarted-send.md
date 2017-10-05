---
title: "Odesílání událostí do centra událostí Azure pomocí rozhraní .NET Standard | Microsoft Docs"
description: "Začínáme odesílání událostí do centra událostí v rozhraní .NET Standard"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: 8af9d70965c1c9ad8c49b7d2bb04244fc207058d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a><span data-ttu-id="5475e-103">Začínáme s Azure Event Hubs v rozhraní .NET standardní zasílání zpráv</span><span class="sxs-lookup"><span data-stu-id="5475e-103">Get started sending messages to Azure Event Hubs in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="5475e-104">Tato ukázka je dostupná na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span><span class="sxs-lookup"><span data-stu-id="5475e-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span></span>

<span data-ttu-id="5475e-105">Tento kurz ukazuje, jak psát aplikace konzoly .NET Core odeslaná sadu zpráv do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5475e-105">This tutorial shows how to write a .NET Core console application that sends a set of messages to an event hub.</span></span> <span data-ttu-id="5475e-106">Můžete spustit [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) řešení jako-, nahrazuje `EhConnectionString` a `EhEntityPath` řetězce hodnotami centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5475e-106">You can run the [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) solution as-is, replacing the `EhConnectionString` and `EhEntityPath` strings with your event hub values.</span></span> <span data-ttu-id="5475e-107">Nebo můžete provést kroky v tomto kurzu k vytvoření vlastní.</span><span class="sxs-lookup"><span data-stu-id="5475e-107">Or you can follow the steps in this tutorial to create your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5475e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5475e-108">Prerequisites</span></span>

* <span data-ttu-id="5475e-109">[Sadu Microsoft Visual Studio 2015 nebo 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="5475e-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="5475e-110">Příklady v tento kurz použijte Visual Studio 2017, ale Visual Studio 2015 je také podporována.</span><span class="sxs-lookup"><span data-stu-id="5475e-110">The examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="5475e-111">[.NET core Visual Studio 2015 nebo 2017 nástroje](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="5475e-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="5475e-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5475e-112">An Azure subscription.</span></span>
* <span data-ttu-id="5475e-113">Na obor názvů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5475e-113">An event hub namespace.</span></span>

<span data-ttu-id="5475e-114">K odesílání zpráv do centra událostí, budeme používat Visual Studio k zápisu konzolovou aplikaci C#.</span><span class="sxs-lookup"><span data-stu-id="5475e-114">To send messages to an event hub, we will use Visual Studio to write a C# console application.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="5475e-115">Vytvoření oboru názvů Event Hubs a centra událostí</span><span class="sxs-lookup"><span data-stu-id="5475e-115">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="5475e-116">Prvním krokem je použití [portál Azure](https://portal.azure.com) vytvořit obor názvů pro typ rozbočovače události a získat přihlašovací údaje správy, které aplikace potřebuje komunikovat s centrem událostí.</span><span class="sxs-lookup"><span data-stu-id="5475e-116">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace for the event hub type, and obtain the management credentials that your application needs to communicate with the event hub.</span></span> <span data-ttu-id="5475e-117">Pokud chcete vytvořit obor názvů a centra událostí, postupujte podle pokynů v [v tomto článku](event-hubs-create.md)a poté pokračujte podle následujících pokynů.</span><span class="sxs-lookup"><span data-stu-id="5475e-117">To create a namespace and an event hub, follow the procedure in [this article](event-hubs-create.md), and then proceed with the following steps.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="5475e-118">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="5475e-118">Create a console application</span></span>

<span data-ttu-id="5475e-119">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5475e-119">Start Visual Studio.</span></span> <span data-ttu-id="5475e-120">V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="5475e-120">From the **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="5475e-121">Vytvoření aplikace konzoly .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5475e-121">Create a .NET Core console application.</span></span>

![Nový projekt][1]

## <a name="add-the-event-hubs-nuget-package"></a><span data-ttu-id="5475e-123">Přidejte balíček NuGet centra událostí</span><span class="sxs-lookup"><span data-stu-id="5475e-123">Add the Event Hubs NuGet package</span></span>

<span data-ttu-id="5475e-124">Přidat [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET standardní knihovny balíček NuGet do projektu pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="5475e-124">Add the [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard library NuGet package to your project by following these steps:</span></span> 

1. <span data-ttu-id="5475e-125">Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5475e-125">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="5475e-126">Klikněte na tlačítko **Procházet** kartu a potom vyhledejte "Microsoft.Azure.EventHubs" a vyberte **Microsoft.Azure.EventHubs** balíčku.</span><span class="sxs-lookup"><span data-stu-id="5475e-126">Click the **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select the **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="5475e-127">Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5475e-127">Click **Install** to complete the installation, then close this dialog box.</span></span>

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a><span data-ttu-id="5475e-128">Napsat kód, který odesílání zpráv do centra událostí</span><span class="sxs-lookup"><span data-stu-id="5475e-128">Write some code to send messages to the event hub</span></span>

1. <span data-ttu-id="5475e-129">Do horní části souboru Program.cs přidejte následující příkazy `using`.</span><span class="sxs-lookup"><span data-stu-id="5475e-129">Add the following `using` statements to the top of the Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="5475e-130">Přidejte konstanty k `Program` třídu pro Event Hubs připojovací řetězec a entity cesta (název centra jednotlivých událostí).</span><span class="sxs-lookup"><span data-stu-id="5475e-130">Add constants to the `Program` class for the Event Hubs connection string and entity path (individual event hub name).</span></span> <span data-ttu-id="5475e-131">Nahraďte zástupné symboly v závorkách správné hodnoty, které byly získány při vytváření centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5475e-131">Replace the placeholders in brackets with the proper values that were obtained when creating the event hub.</span></span>

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. <span data-ttu-id="5475e-132">Přidat novou metodu s názvem `MainAsync` k `Program` třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5475e-132">Add a new method named `MainAsync` to the `Program` class, as follows:</span></span>

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. <span data-ttu-id="5475e-133">Přidat novou metodu s názvem `SendMessagesToEventHub` k `Program` třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5475e-133">Add a new method named `SendMessagesToEventHub` to the `Program` class, as follows:</span></span>

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. <span data-ttu-id="5475e-134">Přidejte následující kód, který `Main` metoda v `Program` třídy.</span><span class="sxs-lookup"><span data-stu-id="5475e-134">Add the following code to the `Main` method in the `Program` class.</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   <span data-ttu-id="5475e-135">Soubor Program.cs by měl vypadat takhle.</span><span class="sxs-lookup"><span data-stu-id="5475e-135">Here is what your Program.cs should look like.</span></span>

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. <span data-ttu-id="5475e-136">Spusťte program a zkontrolujte, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="5475e-136">Run the program, and ensure that there are no errors.</span></span>

<span data-ttu-id="5475e-137">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="5475e-137">Congratulations!</span></span> <span data-ttu-id="5475e-138">Nyní jste odeslali zprávy do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5475e-138">You have now sent messages to an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5475e-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5475e-139">Next steps</span></span>
<span data-ttu-id="5475e-140">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="5475e-140">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="5475e-141">Přijímat události ze služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5475e-141">Receive events from Event Hubs</span></span>](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [<span data-ttu-id="5475e-142">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5475e-142">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="5475e-143">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="5475e-143">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="5475e-144">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5475e-144">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
