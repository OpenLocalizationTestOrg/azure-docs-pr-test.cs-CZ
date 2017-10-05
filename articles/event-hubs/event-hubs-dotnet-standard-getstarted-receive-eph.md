---
title: "Přijímat události z Azure Event Hubs pomocí rozhraní .NET Standard | Microsoft Docs"
description: "Začínáme příjem zpráv pomocí knihovny EventProcessorHost ve standardní rozhraní .NET"
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
ms.openlocfilehash: cc62792dad0284f9514664795fdfb32e94a85943
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a><span data-ttu-id="9088a-103">Začínáme v rozhraní .NET standardní přijímání zpráv pomocí třídy Eventprocessorhost</span><span class="sxs-lookup"><span data-stu-id="9088a-103">Get started receiving messages with the Event Processor Host in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="9088a-104">Tato ukázka je dostupná na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span><span class="sxs-lookup"><span data-stu-id="9088a-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span></span>

<span data-ttu-id="9088a-105">Tento kurz ukazuje, jak psát aplikace konzoly .NET Core, která přijímá zprávy z centra událostí pomocí **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="9088a-105">This tutorial shows how to write a .NET Core console application that receives messages from an event hub by using **EventProcessorHost**.</span></span> <span data-ttu-id="9088a-106">Můžete spustit [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) řešení jako-se, řetězce nahrazení hodnoty události rozbočovače a úložiště účtu.</span><span class="sxs-lookup"><span data-stu-id="9088a-106">You can run the [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) solution as-is, replacing the strings with your event hub and storage account values.</span></span> <span data-ttu-id="9088a-107">Nebo můžete provést kroky v tomto kurzu k vytvoření vlastní.</span><span class="sxs-lookup"><span data-stu-id="9088a-107">Or you can follow the steps in this tutorial to create your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9088a-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9088a-108">Prerequisites</span></span>

* <span data-ttu-id="9088a-109">[Sadu Microsoft Visual Studio 2015 nebo 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="9088a-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="9088a-110">Příklady v tento kurz použijte Visual Studio 2017, ale Visual Studio 2015 je také podporována.</span><span class="sxs-lookup"><span data-stu-id="9088a-110">The examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="9088a-111">[.NET core Visual Studio 2015 nebo 2017 nástroje](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="9088a-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="9088a-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9088a-112">An Azure subscription.</span></span>
* <span data-ttu-id="9088a-113">Na obor názvů Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="9088a-113">An Azure Event Hubs namespace.</span></span>
* <span data-ttu-id="9088a-114">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9088a-114">An Azure storage account.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="9088a-115">Vytvoření oboru názvů Event Hubs a centra událostí</span><span class="sxs-lookup"><span data-stu-id="9088a-115">Create an Event Hubs namespace and an event hub</span></span>  

<span data-ttu-id="9088a-116">Prvním krokem je použití [portál Azure](https://portal.azure.com) vytvořit obor názvů pro daný typ služby Event Hubs a získat přihlašovací údaje správy, které aplikace potřebuje komunikovat s centrem událostí.</span><span class="sxs-lookup"><span data-stu-id="9088a-116">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace for the Event Hubs type, and obtain the management credentials that your application needs to communicate with the event hub.</span></span> <span data-ttu-id="9088a-117">Pokud chcete vytvořit obor názvů a event hub, postupujte podle pokynů v [v tomto článku](event-hubs-create.md)a poté pokračujte podle následujících pokynů.</span><span class="sxs-lookup"><span data-stu-id="9088a-117">To create a namespace and event hub, follow the procedure in [this article](event-hubs-create.md), and then proceed with the following steps.</span></span>  

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="9088a-118">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="9088a-118">Create an Azure storage account</span></span>  

1. <span data-ttu-id="9088a-119">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9088a-119">Sign in to the [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="9088a-120">V levém navigačním podokně portálu klikněte na **nový**, klikněte na tlačítko **úložiště**a potom klikněte na **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="9088a-120">In the left navigation pane of the portal, click **New**, click **Storage**, and then click **Storage Account**.</span></span>  
3. <span data-ttu-id="9088a-121">Vyplňte pole v okně účtu úložiště a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9088a-121">Complete the fields in the storage account blade, and then click **Create**.</span></span>

    ![Vytvořit účet úložiště][1]

4. <span data-ttu-id="9088a-123">Jakmile se zobrazí **úspěšné nasazení** zprávy, klikněte na název nového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9088a-123">After you see the **Deployments Succeeded** message, click the name of the new storage account.</span></span> <span data-ttu-id="9088a-124">V **Essentials** okně klikněte na tlačítko **objekty BLOB**.</span><span class="sxs-lookup"><span data-stu-id="9088a-124">In the **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="9088a-125">Když **služba objektů Blob** okno otevře, klikněte na tlačítko **+ kontejner** v horní části.</span><span class="sxs-lookup"><span data-stu-id="9088a-125">When the **Blob service** blade opens, click **+ Container** at the top.</span></span> <span data-ttu-id="9088a-126">Zadejte název kontejneru a pak zavřete **služba objektů Blob** okno.</span><span class="sxs-lookup"><span data-stu-id="9088a-126">Give the container a name, and then close the **Blob service** blade.</span></span>  
5. <span data-ttu-id="9088a-127">Klikněte na tlačítko **přístupové klíče** v levém okně a zkopírujte název kontejneru úložiště, účet úložiště a hodnota **key1**.</span><span class="sxs-lookup"><span data-stu-id="9088a-127">Click **Access keys** in the left blade and copy the name of the storage container, the storage account, and the value of **key1**.</span></span> <span data-ttu-id="9088a-128">Uložte tyto hodnoty do programu Poznámkový blok nebo některé dočasné umístění.</span><span class="sxs-lookup"><span data-stu-id="9088a-128">Save these values to Notepad or some other temporary location.</span></span>  

## <a name="create-a-console-application"></a><span data-ttu-id="9088a-129">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="9088a-129">Create a console application</span></span>

<span data-ttu-id="9088a-130">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9088a-130">Start Visual Studio.</span></span> <span data-ttu-id="9088a-131">V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="9088a-131">From the **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="9088a-132">Vytvoření aplikace konzoly .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9088a-132">Create a .NET Core console application.</span></span>

![Nový projekt][2]

## <a name="add-the-event-hubs-nuget-package"></a><span data-ttu-id="9088a-134">Přidejte balíček NuGet centra událostí</span><span class="sxs-lookup"><span data-stu-id="9088a-134">Add the Event Hubs NuGet package</span></span>

<span data-ttu-id="9088a-135">Přidat [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) a [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET standardní knihovna NuGet balíčky do projektu pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9088a-135">Add the [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) and [`Microsoft.Azure.EventHubs.Processor`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard library NuGet packages to your project by following these steps:</span></span> 

1. <span data-ttu-id="9088a-136">Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9088a-136">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="9088a-137">Klikněte na tlačítko **Procházet** kartu a potom vyhledejte "Microsoft.Azure.EventHubs" a vyberte **Microsoft.Azure.EventHubs** balíčku.</span><span class="sxs-lookup"><span data-stu-id="9088a-137">Click the **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select the **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="9088a-138">Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9088a-138">Click **Install** to complete the installation, then close this dialog box.</span></span>
3. <span data-ttu-id="9088a-139">Opakujte kroky 1 a 2 a nainstalujte **Microsoft.Azure.EventHubs.Processor** balíčku.</span><span class="sxs-lookup"><span data-stu-id="9088a-139">Repeat steps 1 and 2, and install the **Microsoft.Azure.EventHubs.Processor** package.</span></span>

## <a name="implement-the-ieventprocessor-interface"></a><span data-ttu-id="9088a-140">Implementace rozhraní IEventProcessor</span><span class="sxs-lookup"><span data-stu-id="9088a-140">Implement the IEventProcessor interface</span></span>

1. <span data-ttu-id="9088a-141">V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="9088a-141">In Solution Explorer, right-click the project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="9088a-142">Pojmenujte novou třídu **SimpleEventProcessor**.</span><span class="sxs-lookup"><span data-stu-id="9088a-142">Name the new class **SimpleEventProcessor**.</span></span>

2. <span data-ttu-id="9088a-143">Otevřete na začátek souboru SimpleEventProcessor.cs a přidejte následující `using` příkazů do horní části souboru.</span><span class="sxs-lookup"><span data-stu-id="9088a-143">Open the SimpleEventProcessor.cs file and add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. <span data-ttu-id="9088a-144">Implementace `IEventProcessor` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9088a-144">Implement the `IEventProcessor` interface.</span></span> <span data-ttu-id="9088a-145">Nahradí celý obsah `SimpleEventProcessor` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="9088a-145">Replace the entire contents of the `SimpleEventProcessor` class with the following code:</span></span>

    ```csharp
    public class SimpleEventProcessor : IEventProcessor
    {
        public Task CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
            return Task.CompletedTask;
        }

        public Task OpenAsync(PartitionContext context)
        {
            Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
            return Task.CompletedTask;
        }

        public Task ProcessErrorAsync(PartitionContext context, Exception error)
        {
            Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
            return Task.CompletedTask;
        }

        public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (var eventData in messages)
            {
                var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
                Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
            }

            return context.CheckpointAsync();
        }
    }
    ```

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a><span data-ttu-id="9088a-146">Napíše metoda hlavní konzoly, která používá třídu SimpleEventProcessor pro příjem zpráv</span><span class="sxs-lookup"><span data-stu-id="9088a-146">Write a main console method that uses the SimpleEventProcessor class to receive messages</span></span>

1. <span data-ttu-id="9088a-147">Do horní části souboru Program.cs přidejte následující příkazy `using`.</span><span class="sxs-lookup"><span data-stu-id="9088a-147">Add the following `using` statements to the top of the Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="9088a-148">Přidejte konstanty k `Program` třídu pro událost rozbočovače připojovací řetězec, název centra událostí, název kontejneru účtu úložiště, název účtu úložiště a klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9088a-148">Add constants to the `Program` class for the event hub connection string, event hub name, storage account container name, storage account name, and storage account key.</span></span> <span data-ttu-id="9088a-149">Přidejte následující kód, jejich příslušné hodnoty nahraďte zástupné symboly.</span><span class="sxs-lookup"><span data-stu-id="9088a-149">Add the following code, replacing the placeholders with their corresponding values.</span></span>

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. <span data-ttu-id="9088a-150">Přidat novou metodu s názvem `MainAsync` k `Program` třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9088a-150">Add a new method named `MainAsync` to the `Program` class, as follows:</span></span>

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EhEntityPath,
            PartitionReceiver.DefaultConsumerGroupName,
            EhConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. <span data-ttu-id="9088a-151">Přidejte následující řádek kódu `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="9088a-151">Add the following line of code to the `Main` method:</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    <span data-ttu-id="9088a-152">Soubor Program.cs by měl vypadat takhle:</span><span class="sxs-lookup"><span data-stu-id="9088a-152">Here is what your Program.cs file should look like:</span></span>

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. <span data-ttu-id="9088a-153">Spusťte program a zkontrolujte, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="9088a-153">Run the program, and ensure that there are no errors.</span></span>

<span data-ttu-id="9088a-154">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="9088a-154">Congratulations!</span></span> <span data-ttu-id="9088a-155">Nyní máte přijímá zprávy z centra událostí pomocí Event Processor Host.</span><span class="sxs-lookup"><span data-stu-id="9088a-155">You have now received messages from an event hub by using the Event Processor Host.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9088a-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9088a-156">Next steps</span></span>
<span data-ttu-id="9088a-157">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="9088a-157">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="9088a-158">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="9088a-158">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="9088a-159">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="9088a-159">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="9088a-160">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="9088a-160">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
