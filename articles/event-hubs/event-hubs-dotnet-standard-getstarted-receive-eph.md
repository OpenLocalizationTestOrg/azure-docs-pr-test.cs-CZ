---
title: "aaaReceive události z Azure Event Hubs pomocí rozhraní .NET Standard | Microsoft Docs"
description: "Začínáme příjem zpráv s hello EventProcessorHost ve standardní rozhraní .NET"
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
ms.openlocfilehash: c3983f2668ac8f65522e44a1609dfd2eed31b7d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-receiving-messages-with-hello-event-processor-host-in-net-standard"></a><span data-ttu-id="d6ecd-103">Začínáme přijímání zpráv s hello Event Processor Host v .NET Standard</span><span class="sxs-lookup"><span data-stu-id="d6ecd-103">Get started receiving messages with hello Event Processor Host in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="d6ecd-104">Tato ukázka je dostupná na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span><span class="sxs-lookup"><span data-stu-id="d6ecd-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span></span>

<span data-ttu-id="d6ecd-105">Tento kurz ukazuje, jak toowrite .NET Core Konzolová aplikace, která přijímá zprávy z centra událostí pomocí **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-105">This tutorial shows how toowrite a .NET Core console application that receives messages from an event hub by using **EventProcessorHost**.</span></span> <span data-ttu-id="d6ecd-106">Můžete spustit hello [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) řešení jako-se nahrazování řetězců hello s událostí hodnoty účet rozbočovače a úložiště.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-106">You can run hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) solution as-is, replacing hello strings with your event hub and storage account values.</span></span> <span data-ttu-id="d6ecd-107">Nebo můžete provést hello kroky tento kurz toocreate vlastní.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-107">Or you can follow hello steps in this tutorial toocreate your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6ecd-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d6ecd-108">Prerequisites</span></span>

* <span data-ttu-id="d6ecd-109">[Sadu Microsoft Visual Studio 2015 nebo 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="d6ecd-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="d6ecd-110">Hello příklady v tomto kurzu Visual Studio 2017, ale Visual Studio 2015 je také podporována.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-110">hello examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="d6ecd-111">[.NET core Visual Studio 2015 nebo 2017 nástroje](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="d6ecd-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="d6ecd-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-112">An Azure subscription.</span></span>
* <span data-ttu-id="d6ecd-113">Na obor názvů Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-113">An Azure Event Hubs namespace.</span></span>
* <span data-ttu-id="d6ecd-114">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-114">An Azure storage account.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="d6ecd-115">Vytvoření oboru názvů Event Hubs a centra událostí</span><span class="sxs-lookup"><span data-stu-id="d6ecd-115">Create an Event Hubs namespace and an event hub</span></span>  

<span data-ttu-id="d6ecd-116">prvním krokem Hello je toouse hello [portál Azure](https://portal.azure.com) toocreate obor názvů pro hello Event Hubs zadejte a získání přihlašovacích údajů pro správu, aplikace musí toocommunicate s centrem událostí hello hello.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-116">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace for hello Event Hubs type, and obtain hello management credentials that your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="d6ecd-117">toocreate obor názvů a centra událostí, postupujte podle postupu hello v [v tomto článku](event-hubs-create.md)a poté pokračovat hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-117">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), and then proceed with hello following steps.</span></span>  

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="d6ecd-118">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="d6ecd-118">Create an Azure storage account</span></span>  

1. <span data-ttu-id="d6ecd-119">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d6ecd-119">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="d6ecd-120">V levém navigačním podokně hello hello portálu, klikněte na **nový**, klikněte na tlačítko **úložiště**a potom klikněte na **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-120">In hello left navigation pane of hello portal, click **New**, click **Storage**, and then click **Storage Account**.</span></span>  
3. <span data-ttu-id="d6ecd-121">Vyplňte pole hello v okně účtu úložiště hello a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-121">Complete hello fields in hello storage account blade, and then click **Create**.</span></span>

    ![Vytvořit účet úložiště][1]

4. <span data-ttu-id="d6ecd-123">Jakmile ověříte hello **úspěšné nasazení** zprávy, klikněte na název hello hello nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-123">After you see hello **Deployments Succeeded** message, click hello name of hello new storage account.</span></span> <span data-ttu-id="d6ecd-124">V hello **Essentials** okně klikněte na tlačítko **objekty BLOB**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-124">In hello **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="d6ecd-125">Když hello **služba objektů Blob** okno otevře, klikněte na tlačítko **+ kontejner** v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-125">When hello **Blob service** blade opens, click **+ Container** at hello top.</span></span> <span data-ttu-id="d6ecd-126">Pojmenujte hello kontejner a pak zavřete hello **služba objektů Blob** okno.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-126">Give hello container a name, and then close hello **Blob service** blade.</span></span>  
5. <span data-ttu-id="d6ecd-127">Klikněte na tlačítko **přístupové klíče** v hello levém okně a zkopírujte hello názvu kontejneru úložiště hello, účet úložiště hello a hodnota hello **key1**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-127">Click **Access keys** in hello left blade and copy hello name of hello storage container, hello storage account, and hello value of **key1**.</span></span> <span data-ttu-id="d6ecd-128">Uložte tyto hodnoty tooNotepad nebo některé dočasné umístění.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-128">Save these values tooNotepad or some other temporary location.</span></span>  

## <a name="create-a-console-application"></a><span data-ttu-id="d6ecd-129">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="d6ecd-129">Create a console application</span></span>

<span data-ttu-id="d6ecd-130">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-130">Start Visual Studio.</span></span> <span data-ttu-id="d6ecd-131">Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-131">From hello **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="d6ecd-132">Vytvoření aplikace konzoly .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-132">Create a .NET Core console application.</span></span>

![Nový projekt][2]

## <a name="add-hello-event-hubs-nuget-package"></a><span data-ttu-id="d6ecd-134">Přidání balíčku NuGet centra událostí hello</span><span class="sxs-lookup"><span data-stu-id="d6ecd-134">Add hello Event Hubs NuGet package</span></span>

<span data-ttu-id="d6ecd-135">Přidat hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) a [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard NuGet balíčky tooyour projektu knihovny pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d6ecd-135">Add hello [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) and [`Microsoft.Azure.EventHubs.Processor`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard library NuGet packages tooyour project by following these steps:</span></span> 

1. <span data-ttu-id="d6ecd-136">Klikněte pravým tlačítkem hello nově vytvořený projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-136">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="d6ecd-137">Klikněte na tlačítko hello **Procházet** kartu a potom vyhledejte "Microsoft.Azure.EventHubs" a vyberte hello **Microsoft.Azure.EventHubs** balíčku.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-137">Click hello **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select hello **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="d6ecd-138">Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete toto dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-138">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
3. <span data-ttu-id="d6ecd-139">Opakujte kroky 1 a 2 a nainstalujte hello **Microsoft.Azure.EventHubs.Processor** balíčku.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-139">Repeat steps 1 and 2, and install hello **Microsoft.Azure.EventHubs.Processor** package.</span></span>

## <a name="implement-hello-ieventprocessor-interface"></a><span data-ttu-id="d6ecd-140">Implementovat rozhraní IEventProcessor hello</span><span class="sxs-lookup"><span data-stu-id="d6ecd-140">Implement hello IEventProcessor interface</span></span>

1. <span data-ttu-id="d6ecd-141">V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello, klikněte na tlačítko **přidat**a potom klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-141">In Solution Explorer, right-click hello project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="d6ecd-142">Pojmenujte novou třídu hello **SimpleEventProcessor**.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-142">Name hello new class **SimpleEventProcessor**.</span></span>

2. <span data-ttu-id="d6ecd-143">Otevřete na začátek souboru SimpleEventProcessor.cs hello a přidejte následující hello `using` toohello příkazy na začátek souboru hello.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-143">Open hello SimpleEventProcessor.cs file and add hello following `using` statements toohello top of hello file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. <span data-ttu-id="d6ecd-144">Implementace hello `IEventProcessor` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-144">Implement hello `IEventProcessor` interface.</span></span> <span data-ttu-id="d6ecd-145">Nahraďte hello celý obsah hello `SimpleEventProcessor` se hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="d6ecd-145">Replace hello entire contents of hello `SimpleEventProcessor` class with hello following code:</span></span>

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

## <a name="write-a-main-console-method-that-uses-hello-simpleeventprocessor-class-tooreceive-messages"></a><span data-ttu-id="d6ecd-146">Napíše metoda hlavní konzoly, která používá hello SimpleEventProcessor třída tooreceive zprávy</span><span class="sxs-lookup"><span data-stu-id="d6ecd-146">Write a main console method that uses hello SimpleEventProcessor class tooreceive messages</span></span>

1. <span data-ttu-id="d6ecd-147">Přidejte následující hello `using` toohello příkazy na začátku souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-147">Add hello following `using` statements toohello top of hello Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="d6ecd-148">Přidat konstanty toohello `Program` třídu pro hello event hub připojovací řetězec, název centra událostí, název kontejneru účtu úložiště, název účtu úložiště a klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-148">Add constants toohello `Program` class for hello event hub connection string, event hub name, storage account container name, storage account name, and storage account key.</span></span> <span data-ttu-id="d6ecd-149">Přidejte následující kód, jejich příslušné hodnoty nahraďte zástupné symboly hello hello.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-149">Add hello following code, replacing hello placeholders with their corresponding values.</span></span>

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. <span data-ttu-id="d6ecd-150">Přidat novou metodu s názvem `MainAsync` toohello `Program` třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d6ecd-150">Add a new method named `MainAsync` toohello `Program` class, as follows:</span></span>

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

        // Registers hello Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER toostop worker.");
        Console.ReadLine();

        // Disposes of hello Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. <span data-ttu-id="d6ecd-151">Přidejte následující řádek kódu toohello hello `Main` metoda:</span><span class="sxs-lookup"><span data-stu-id="d6ecd-151">Add hello following line of code toohello `Main` method:</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    <span data-ttu-id="d6ecd-152">Soubor Program.cs by měl vypadat takhle:</span><span class="sxs-lookup"><span data-stu-id="d6ecd-152">Here is what your Program.cs file should look like:</span></span>

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

                // Registers hello Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER toostop worker.");
                Console.ReadLine();

                // Disposes of hello Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. <span data-ttu-id="d6ecd-153">Spuštění programu hello a ujistěte se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-153">Run hello program, and ensure that there are no errors.</span></span>

<span data-ttu-id="d6ecd-154">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="d6ecd-154">Congratulations!</span></span> <span data-ttu-id="d6ecd-155">Nyní máte přijímá zprávy z centra událostí pomocí hello Event Processor Host.</span><span class="sxs-lookup"><span data-stu-id="d6ecd-155">You have now received messages from an event hub by using hello Event Processor Host.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6ecd-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d6ecd-156">Next steps</span></span>
<span data-ttu-id="d6ecd-157">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="d6ecd-157">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="d6ecd-158">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d6ecd-158">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="d6ecd-159">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="d6ecd-159">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="d6ecd-160">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="d6ecd-160">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
