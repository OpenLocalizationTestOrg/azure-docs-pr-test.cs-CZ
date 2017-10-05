---
title: "Příjem událostí z Azure Event Hubs pomocí rozhraní .NET Framework | Dokumentace Microsoftu"
description: "V tomto kurzu se naučíte přijímat události z Azure Event Hubs pomocí rozhraní .NET Framework."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/12/2017
ms.author: sethm
ms.openlocfilehash: 581af52b3264b1a7c57315c65d5385a372308c27
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a><span data-ttu-id="6b425-103">Příjem událostí z Azure Event Hubs pomocí rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="6b425-103">Receive events from Azure Event Hubs using the .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="6b425-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="6b425-104">Introduction</span></span>

<span data-ttu-id="6b425-105">Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací.</span><span class="sxs-lookup"><span data-stu-id="6b425-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="6b425-106">Data, která shromáždíte pomocí služby Event Hubs, můžete uložit pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele datové analýzy v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="6b425-106">After you collect data into Event Hubs, you can store the data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="6b425-107">Schopnost shromažďovat a zpracovávat velké množství událostí je klíčovou komponentou moderních aplikačních architektur, například internetu věcí (Internet of Things – IoT).</span><span class="sxs-lookup"><span data-stu-id="6b425-107">This large-scale event collection and processing capability is a key component of modern application architectures including the Internet of Things (IoT).</span></span>

<span data-ttu-id="6b425-108">Tento kurz ukazuje, jak psát aplikace konzoly rozhraní .NET Framework, která přijímá zprávy z centra událostí pomocí třídy **[Event Processor Host][EventProcessorHost]**.</span><span class="sxs-lookup"><span data-stu-id="6b425-108">This tutorial shows how to write a .NET Framework console application that receives messages from an event hub using the **[Event Processor Host][EventProcessorHost]**.</span></span> <span data-ttu-id="6b425-109">Pokud chcete odesílat události pomocí rozhraní .NET Framework, přečtěte si článek [Odesílání událostí do Azure Event Hubs pomocí rozhraní .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) nebo klikněte na příslušný odesílající jazyk v obsahu vlevo.</span><span class="sxs-lookup"><span data-stu-id="6b425-109">To send events using the .NET Framework, see the [Send events to Azure Event Hubs using the .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) article, or click the appropriate sending language in the left-hand table of contents.</span></span>

<span data-ttu-id="6b425-110">[Event Processor Host][EventProcessorHost] je třída rozhraní .NET, která zjednodušuje přijímání událostí z center událostí tím, že spravuje trvalé kontrolní body a paralelní příjmy z těchto center událostí.</span><span class="sxs-lookup"><span data-stu-id="6b425-110">The [Event Processor Host][EventProcessorHost] is a .NET class that simplifies receiving events from event hubs by managing persistent checkpoints and parallel receives from those event hubs.</span></span> <span data-ttu-id="6b425-111">Pomocí třídy [Event Processor Host][Event Processor Host] můžete události rozdělit mezi několik příjemců, i když jsou hostované v různých uzlech.</span><span class="sxs-lookup"><span data-stu-id="6b425-111">Using the [Event Processor Host][Event Processor Host], you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="6b425-112">Tento příklad ukazuje způsob použití třídy [Event Processor Host][EventProcessorHost] pro jednoho příjemce.</span><span class="sxs-lookup"><span data-stu-id="6b425-112">This example shows how to use the [Event Processor Host][EventProcessorHost] for a single receiver.</span></span> <span data-ttu-id="6b425-113">Ukázka metody [Horizontální navýšení kapacity zpracování událostí][Scale out Event Processing with Event Hubs] znázorňuje způsob použití třídy [Event Processor Host][EventProcessorHost] v případě několika příjemců.</span><span class="sxs-lookup"><span data-stu-id="6b425-113">The [Scale out event processing][Scale out Event Processing with Event Hubs] sample shows how to use the [Event Processor Host][EventProcessorHost] with multiple receivers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b425-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6b425-114">Prerequisites</span></span>

<span data-ttu-id="6b425-115">Pro absolvování tohoto kurzu musí být splněné následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="6b425-115">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="6b425-116">[Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="6b425-116">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="6b425-117">Pro snímky obrazovky v tomto kurzu se používá Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6b425-117">The screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="6b425-118">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="6b425-118">An active Azure account.</span></span> <span data-ttu-id="6b425-119">Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut.</span><span class="sxs-lookup"><span data-stu-id="6b425-119">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="6b425-120">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6b425-120">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="6b425-121">Vytvoření oboru názvů Event Hubs a centra událostí</span><span class="sxs-lookup"><span data-stu-id="6b425-121">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="6b425-122">Prvním krokem je použití webu [Azure Portal](https://portal.azure.com) k vytvoření oboru názvů typu Event Hubs a získání přihlašovacích údajů pro správu, které vaše aplikace potřebuje ke komunikaci s centrem událostí.</span><span class="sxs-lookup"><span data-stu-id="6b425-122">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace of type Event Hubs, and obtain the management credentials your application needs to communicate with the event hub.</span></span> <span data-ttu-id="6b425-123">Pokud chcete vytvořit obor názvů a centrum událostí, postupujte podle pokynů v [tomto článku](event-hubs-create.md) a pak pokračujte podle následujících pokynů v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6b425-123">To create a namespace and event hub, follow the procedure in [this article](event-hubs-create.md), then proceed with the following steps in this tutorial.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="6b425-124">Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6b425-124">Create an Azure Storage account</span></span>

<span data-ttu-id="6b425-125">Pokud chcete používat třídu [Event Processor Host][EventProcessorHost], musíte mít [Účet služby Azure Storage][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="6b425-125">To use the [Event Processor Host][EventProcessorHost], you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="6b425-126">Přihlaste se na web [Azure Portal][Azure portal] a v levém horním rohu obrazovky klikněte na **Nový**.</span><span class="sxs-lookup"><span data-stu-id="6b425-126">Log on to the [Azure portal][Azure portal], and click **New** at the top left of the screen.</span></span>
2. <span data-ttu-id="6b425-127">Klikněte na **Storage** a poté klikněte na **Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="6b425-127">Click **Storage**, then click **Storage account**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. <span data-ttu-id="6b425-128">V okně **Vytvořit účet úložiště** zadejte název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="6b425-128">In the **Create storage account** blade, type a name for the storage account.</span></span> <span data-ttu-id="6b425-129">Zvolte předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6b425-129">Choose an Azure subscription, resource group, and location in which to create the resource.</span></span> <span data-ttu-id="6b425-130">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6b425-130">Then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. <span data-ttu-id="6b425-131">V seznamu účtů úložiště klikněte na nově vytvořený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6b425-131">In the list of storage accounts, click the newly created storage account.</span></span>
5. <span data-ttu-id="6b425-132">V okně účtu úložiště klikněte na **Přístupové klávesy**.</span><span class="sxs-lookup"><span data-stu-id="6b425-132">In the storage account blade, click **Access keys**.</span></span> <span data-ttu-id="6b425-133">Zkopírujte hodnotu **key1** pro pozdější použití v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6b425-133">Copy the value of **key1** to use later in this tutorial.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a><span data-ttu-id="6b425-134">Vytvoření konzolové aplikace Příjemce</span><span class="sxs-lookup"><span data-stu-id="6b425-134">Create a receiver console application</span></span>

1. <span data-ttu-id="6b425-135">Pomocí šablony projektu **Konzolová aplikace** vytvořte v sadě Visual Studio nový projekt desktopové aplikace Visual C#.</span><span class="sxs-lookup"><span data-stu-id="6b425-135">In Visual Studio, create a new Visual C# Desktop App project using the **Console  Application** project template.</span></span> <span data-ttu-id="6b425-136">Projekt nazvěte **Receiver** (Příjemce).</span><span class="sxs-lookup"><span data-stu-id="6b425-136">Name the project **Receiver**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. <span data-ttu-id="6b425-137">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **Receiver** a potom klikněte na **Spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="6b425-137">In Solution Explorer, right-click the **Receiver** project, and then click **Manage NuGet Packages for Solution**.</span></span>
3. <span data-ttu-id="6b425-138">Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span><span class="sxs-lookup"><span data-stu-id="6b425-138">Click the **Browse** tab, then search for `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span></span> <span data-ttu-id="6b425-139">Klikněte na **Instalovat** a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="6b425-139">Click **Install**, and accept the terms of use.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    <span data-ttu-id="6b425-140">Visual Studio stáhne, nainstaluje a přidá odkaz na [balíček NuGet třídy EventProcessorHost služby Event Hub ve službě Azure Service Bus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) se všemi jeho závislostmi.</span><span class="sxs-lookup"><span data-stu-id="6b425-140">Visual Studio downloads, installs, and adds a reference to the [Azure Service Bus Event Hub - EventProcessorHost NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), with all its dependencies.</span></span>
4. <span data-ttu-id="6b425-141">Klikněte pravým tlačítkem na projekt **Receiver**, **Přidat** a potom na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="6b425-141">Right-click the **Receiver** project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="6b425-142">Pojmenujte novou třídu **SimpleEventProcessor** a potom kliknutím na **Přidat** třídu vytvořte.</span><span class="sxs-lookup"><span data-stu-id="6b425-142">Name the new class **SimpleEventProcessor**, and then click **Add** to create the class.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. <span data-ttu-id="6b425-143">Na začátek souboru SimpleEventProcessor.cs přidejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6b425-143">Add the following statements at the top of the SimpleEventProcessor.cs file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  <span data-ttu-id="6b425-144">Potom nahraďte tělo třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6b425-144">Then, substitute the following code for the body of the class:</span></span>
    
  ```csharp
  class SimpleEventProcessor : IEventProcessor
  {
    Stopwatch checkpointStopWatch;
    
    async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
    
    Task IEventProcessor.OpenAsync(PartitionContext context)
    {
        Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
        this.checkpointStopWatch = new Stopwatch();
        this.checkpointStopWatch.Start();
        return Task.FromResult<object>(null);
    }
    
    async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData eventData in messages)
        {
            string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
            Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                context.Lease.PartitionId, data));
        }
    
        //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
        if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
        {
            await context.CheckpointAsync();
            this.checkpointStopWatch.Restart();
        }
    }
  }
  ```
    
  <span data-ttu-id="6b425-145">Tuto třídu volá třída **EventProcessorHost** kvůli zpracování událostí přijatých z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="6b425-145">This class is called by the **EventProcessorHost** to process events received from the event hub.</span></span> <span data-ttu-id="6b425-146">Třída `SimpleEventProcessor` používá stopky, aby pravidelně volala metodu kontrolního bodu v kontextu třídy **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="6b425-146">The `SimpleEventProcessor` class uses a stopwatch to periodically call the checkpoint method on the **EventProcessorHost** context.</span></span> <span data-ttu-id="6b425-147">Tímto způsobem je zajištěno, že příjemce v případě restartování neztratí víc než pět minut práce potřebné ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="6b425-147">This processing ensures that, if the receiver is restarted, it loses no more than five minutes of processing work.</span></span>
6. <span data-ttu-id="6b425-148">Ve třídě **Program** přidejte na začátek souboru následující příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="6b425-148">In the **Program** class, add the following `using` statement at the top of the file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  <span data-ttu-id="6b425-149">Potom nahraďte metodu `Main` ve třídě `Program` následujícím kódem, kde nahradíte název centra událostí a připojovací řetězec na úrovni oboru názvů, který jste si dříve uložili, a účet úložiště spolu s klíčem, který jste si v předchozích částech zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="6b425-149">Then, replace the `Main` method in the `Program` class with the following code, substituting the event hub name and the namespace-level connection string that you saved previously, and the storage account and key that you copied in the previous sections.</span></span> 
    
  ```csharp
  static void Main(string[] args)
  {
    string eventHubConnectionString = "{Event Hubs namespace connection string}";
    string eventHubName = "{Event Hub name}";
    string storageAccountName = "{storage account name}";
    string storageAccountKey = "{storage account key}";
    string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
    string eventProcessorHostName = Guid.NewGuid().ToString();
    EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
    Console.WriteLine("Registering EventProcessor...");
    var options = new EventProcessorOptions();
    options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
    eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
    Console.WriteLine("Receiving. Press enter key to stop worker.");
    Console.ReadLine();
    eventProcessorHost.UnregisterEventProcessorAsync().Wait();
  }
  ```

7. <span data-ttu-id="6b425-150">Spusťte program a zkontrolujte, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="6b425-150">Run the program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="6b425-151">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="6b425-151">Congratulations!</span></span> <span data-ttu-id="6b425-152">Obdrželi jste nyní zprávy z centra událostí pomocí třídy Event Processor Host.</span><span class="sxs-lookup"><span data-stu-id="6b425-152">You have now received messages from an event hub using the Event Processor Host.</span></span>


> [!NOTE]
> <span data-ttu-id="6b425-153">Tento kurz používá jednu instanci třídy [EventProcessorHost][EventProcessorHost].</span><span class="sxs-lookup"><span data-stu-id="6b425-153">This tutorial uses a single instance of [EventProcessorHost][EventProcessorHost].</span></span> <span data-ttu-id="6b425-154">Pokud chcete zvýšit propustnost, spusťte několik instancí třídy [EventProcessorHost][EventProcessorHost], jak je znázorněno v ukázce metody [Horizontální navýšení kapacity zpracování událostí][Horizontální navýšení kapacity zpracování událostí].</span><span class="sxs-lookup"><span data-stu-id="6b425-154">To increase throughput, it is recommended that you run multiple instances of [EventProcessorHost][EventProcessorHost], as shown in the [Scaled out event processing][Scaled out event processing] sample.</span></span> <span data-ttu-id="6b425-155">V těchto případech se spolu různé instance navzájem automaticky koordinují, aby dokázaly vyrovnávat zatížení přijatých událostí.</span><span class="sxs-lookup"><span data-stu-id="6b425-155">In those cases, the various instances automatically coordinate with each other to load balance the received events.</span></span> <span data-ttu-id="6b425-156">Pokud chcete, aby každý z několika příjemců zpracovával *všechny* události, musíte použít koncept **ConsumerGroup**.</span><span class="sxs-lookup"><span data-stu-id="6b425-156">If you want multiple receivers to each process *all* the events, you must use the **ConsumerGroup** concept.</span></span> <span data-ttu-id="6b425-157">Když přijímáte události z různých počítačů, může být užitečné nazvat instance třídy [EventProcessorHost][EventProcessorHost] podle počítačů (nebo rolí), ve kterých jsou nasazené.</span><span class="sxs-lookup"><span data-stu-id="6b425-157">When receiving events from different machines, it might be useful to specify names for [EventProcessorHost][EventProcessorHost] instances based on the machines (or roles) in which they are deployed.</span></span> <span data-ttu-id="6b425-158">Další informace o těchto tématech najdete v tématech [Přehled služby Event Hubs][Event Hubs overview] a [Průvodce programováním pro službu Event Hubs][Event Hubs Programming Guide].</span><span class="sxs-lookup"><span data-stu-id="6b425-158">For more information about these topics, see the [Event Hubs overview][Event Hubs overview] and the [Event Hubs programming guide][Event Hubs Programming Guide] topics.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6b425-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b425-159">Next steps</span></span>

<span data-ttu-id="6b425-160">Sestavili jste funkční aplikaci, která vytvoří centrum událostí a odesílá i přijímá data. Nyní se můžete dozvědět víc návštěvou následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="6b425-160">Now that you've built a working application that creates an event hub and sends and receives data, you can learn more by visiting the following links:</span></span>

* <span data-ttu-id="6b425-161">[Event Processor Host][Event Processor Host]</span><span class="sxs-lookup"><span data-stu-id="6b425-161">[Event Processor Host][Event Processor Host]</span></span>
* <span data-ttu-id="6b425-162">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="6b425-162">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="6b425-163">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="6b425-163">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com
