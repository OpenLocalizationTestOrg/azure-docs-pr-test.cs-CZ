---
title: "aaaSend události tooAzure Event Hubs pomocí .NET Standard | Microsoft Docs"
description: "Začínáme odesílání tooEvent centra událostí v rozhraní .NET Standard"
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
ms.openlocfilehash: caa9747a8a72aa8e7aea1348a116f6e4b406460e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-sending-messages-tooazure-event-hubs-in-net-standard"></a><span data-ttu-id="5e429-103">Začínáme odesílání zpráv v rozhraní .NET standardní tooAzure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5e429-103">Get started sending messages tooAzure Event Hubs in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="5e429-104">Tato ukázka je dostupná na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span><span class="sxs-lookup"><span data-stu-id="5e429-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span></span>

<span data-ttu-id="5e429-105">Tento kurz ukazuje, jak toowrite konzolovou aplikaci .NET Core, která odesílá zprávy tooan centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5e429-105">This tutorial shows how toowrite a .NET Core console application that sends a set of messages tooan event hub.</span></span> <span data-ttu-id="5e429-106">Můžete spustit hello [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) řešení jako-, nahrazuje hello `EhConnectionString` a `EhEntityPath` řetězce hodnotami centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5e429-106">You can run hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) solution as-is, replacing hello `EhConnectionString` and `EhEntityPath` strings with your event hub values.</span></span> <span data-ttu-id="5e429-107">Nebo můžete provést hello kroky tento kurz toocreate vlastní.</span><span class="sxs-lookup"><span data-stu-id="5e429-107">Or you can follow hello steps in this tutorial toocreate your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e429-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5e429-108">Prerequisites</span></span>

* <span data-ttu-id="5e429-109">[Sadu Microsoft Visual Studio 2015 nebo 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="5e429-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="5e429-110">Hello příklady v tomto kurzu Visual Studio 2017, ale Visual Studio 2015 je také podporována.</span><span class="sxs-lookup"><span data-stu-id="5e429-110">hello examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="5e429-111">[.NET core Visual Studio 2015 nebo 2017 nástroje](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="5e429-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="5e429-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5e429-112">An Azure subscription.</span></span>
* <span data-ttu-id="5e429-113">Na obor názvů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="5e429-113">An event hub namespace.</span></span>

<span data-ttu-id="5e429-114">centra událostí tooan toosend zprávy, budeme používat Visual Studio toowrite konzolovou aplikaci C#.</span><span class="sxs-lookup"><span data-stu-id="5e429-114">toosend messages tooan event hub, we will use Visual Studio toowrite a C# console application.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="5e429-115">Vytvoření oboru názvů Event Hubs a centra událostí</span><span class="sxs-lookup"><span data-stu-id="5e429-115">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="5e429-116">prvním krokem Hello je toouse hello [portál Azure](https://portal.azure.com) toocreate obor názvů pro typ rozbočovače hello události a získání přihlašovacích údajů pro správu, aplikace musí toocommunicate s centrem událostí hello hello.</span><span class="sxs-lookup"><span data-stu-id="5e429-116">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace for hello event hub type, and obtain hello management credentials that your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="5e429-117">toocreate obor názvů a centra událostí, postupujte podle postupu hello v [v tomto článku](event-hubs-create.md)a poté pokračovat hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="5e429-117">toocreate a namespace and an event hub, follow hello procedure in [this article](event-hubs-create.md), and then proceed with hello following steps.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="5e429-118">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="5e429-118">Create a console application</span></span>

<span data-ttu-id="5e429-119">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e429-119">Start Visual Studio.</span></span> <span data-ttu-id="5e429-120">Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="5e429-120">From hello **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="5e429-121">Vytvoření aplikace konzoly .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e429-121">Create a .NET Core console application.</span></span>

![Nový projekt][1]

## <a name="add-hello-event-hubs-nuget-package"></a><span data-ttu-id="5e429-123">Přidání balíčku NuGet centra událostí hello</span><span class="sxs-lookup"><span data-stu-id="5e429-123">Add hello Event Hubs NuGet package</span></span>

<span data-ttu-id="5e429-124">Přidat hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard NuGet balíček tooyour projektu knihovny pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="5e429-124">Add hello [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard library NuGet package tooyour project by following these steps:</span></span> 

1. <span data-ttu-id="5e429-125">Klikněte pravým tlačítkem hello nově vytvořený projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e429-125">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="5e429-126">Klikněte na tlačítko hello **Procházet** kartu a potom vyhledejte "Microsoft.Azure.EventHubs" a vyberte hello **Microsoft.Azure.EventHubs** balíčku.</span><span class="sxs-lookup"><span data-stu-id="5e429-126">Click hello **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select hello **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="5e429-127">Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete toto dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5e429-127">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>

## <a name="write-some-code-toosend-messages-toohello-event-hub"></a><span data-ttu-id="5e429-128">Zápis centra událostí toohello některé kód toosend zprávy</span><span class="sxs-lookup"><span data-stu-id="5e429-128">Write some code toosend messages toohello event hub</span></span>

1. <span data-ttu-id="5e429-129">Přidejte následující hello `using` toohello příkazy na začátku souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="5e429-129">Add hello following `using` statements toohello top of hello Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="5e429-130">Přidat konstanty toohello `Program` třídu pro hello Event Hubs, připojovací řetězec a entity cesta (název rozbočovače jednotlivých událostí).</span><span class="sxs-lookup"><span data-stu-id="5e429-130">Add constants toohello `Program` class for hello Event Hubs connection string and entity path (individual event hub name).</span></span> <span data-ttu-id="5e429-131">Nahraďte zástupné symboly hello v závorkách hello správné hodnoty, které byly získány při vytváření centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="5e429-131">Replace hello placeholders in brackets with hello proper values that were obtained when creating hello event hub.</span></span>

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. <span data-ttu-id="5e429-132">Přidat novou metodu s názvem `MainAsync` toohello `Program` třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5e429-132">Add a new method named `MainAsync` toohello `Program` class, as follows:</span></span>

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from hello connection string, and sets hello EntityPath.
        // Typically, hello connection string should have hello entity path in it, but for hello sake of this simple scenario
        // we are using hello connection string from hello namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
    }
    ```

4. <span data-ttu-id="5e429-133">Přidat novou metodu s názvem `SendMessagesToEventHub` toohello `Program` třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5e429-133">Add a new method named `SendMessagesToEventHub` toohello `Program` class, as follows:</span></span>

    ```csharp
    // Creates an event hub client and sends 100 messages toohello event hub.
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

5. <span data-ttu-id="5e429-134">Přidejte následující kód toohello hello `Main` metoda v hello `Program` třídy.</span><span class="sxs-lookup"><span data-stu-id="5e429-134">Add hello following code toohello `Main` method in hello `Program` class.</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   <span data-ttu-id="5e429-135">Soubor Program.cs by měl vypadat takhle.</span><span class="sxs-lookup"><span data-stu-id="5e429-135">Here is what your Program.cs should look like.</span></span>

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
                // Creates an EventHubsConnectionStringBuilder object from hello connection string, and sets hello EntityPath.
                // Typically, hello connection string should have hello entity path in it, but for hello sake of this simple scenario
                // we are using hello connection string from hello namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER tooexit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages toohello event hub.
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

6. <span data-ttu-id="5e429-136">Spuštění programu hello a ujistěte se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="5e429-136">Run hello program, and ensure that there are no errors.</span></span>

<span data-ttu-id="5e429-137">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="5e429-137">Congratulations!</span></span> <span data-ttu-id="5e429-138">Nyní jste odeslali centra událostí tooan zprávy.</span><span class="sxs-lookup"><span data-stu-id="5e429-138">You have now sent messages tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e429-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e429-139">Next steps</span></span>
<span data-ttu-id="5e429-140">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="5e429-140">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="5e429-141">Přijímat události ze služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5e429-141">Receive events from Event Hubs</span></span>](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [<span data-ttu-id="5e429-142">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5e429-142">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="5e429-143">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="5e429-143">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="5e429-144">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5e429-144">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
