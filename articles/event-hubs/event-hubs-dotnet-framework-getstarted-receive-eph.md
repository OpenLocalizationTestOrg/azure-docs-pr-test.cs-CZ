---
title: "hello aaaReceive události z Azure Event Hubs pomocí rozhraní .NET Framework | Microsoft Docs"
description: "Postupujte podle tohoto kurzu tooreceive události z Azure Event Hubs pomocí hello rozhraní .NET Framework."
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
ms.openlocfilehash: a88c3feeacfd3de9622dbb86e25222e861750204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-azure-event-hubs-using-hello-net-framework"></a><span data-ttu-id="cd0d9-103">Přijímat události z Azure Event Hubs pomocí hello rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="cd0d9-103">Receive events from Azure Event Hubs using hello .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="cd0d9-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="cd0d9-104">Introduction</span></span>

<span data-ttu-id="cd0d9-105">Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="cd0d9-106">Po shromažďovat data do centra událostí, můžete ukládat data hello pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele analýzu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-106">After you collect data into Event Hubs, you can store hello data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="cd0d9-107">Tato schopnost shromažďovat a zpracovávat rozsáhlé událostí je klíčovou komponentou moderních aplikačních architektur, například hello Internet věcí (IoT).</span><span class="sxs-lookup"><span data-stu-id="cd0d9-107">This large-scale event collection and processing capability is a key component of modern application architectures including hello Internet of Things (IoT).</span></span>

<span data-ttu-id="cd0d9-108">Tento kurz ukazuje, jak toowrite rozhraní .NET Framework Konzolová aplikace, která přijímá zprávy z centra událostí pomocí hello  **[Event Processor Host][EventProcessorHost]**.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-108">This tutorial shows how toowrite a .NET Framework console application that receives messages from an event hub using hello **[Event Processor Host][EventProcessorHost]**.</span></span> <span data-ttu-id="cd0d9-109">toosend událostí pomocí hello rozhraní .NET Framework, najdete v části hello [odesílat události tooAzure Event Hubs pomocí hello rozhraní .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) článek, nebo klikněte na příslušný odesílání jazyk hello v levé tabulce hello obsahu.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-109">toosend events using hello .NET Framework, see hello [Send events tooAzure Event Hubs using hello .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) article, or click hello appropriate sending language in hello left-hand table of contents.</span></span>

<span data-ttu-id="cd0d9-110">Hello [Event Processor Host] [ EventProcessorHost] je třída rozhraní .NET, která zjednodušuje přijímání události ze služby event hubs tím, že spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby event hubs.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-110">hello [Event Processor Host][EventProcessorHost] is a .NET class that simplifies receiving events from event hubs by managing persistent checkpoints and parallel receives from those event hubs.</span></span> <span data-ttu-id="cd0d9-111">Pomocí hello [Event Processor Host][Event Processor Host], i když jsou hostované v různých uzlech můžete události rozdělit mezi několik příjemců.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-111">Using hello [Event Processor Host][Event Processor Host], you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="cd0d9-112">Tento příklad ukazuje, jak toouse hello [Event Processor Host] [ EventProcessorHost] pro jednoho příjemce.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-112">This example shows how toouse hello [Event Processor Host][EventProcessorHost] for a single receiver.</span></span> <span data-ttu-id="cd0d9-113">Hello [horizontální navýšení kapacity zpracování událostí] [ Scale out Event Processing with Event Hubs] ukázkové ukazuje, jak toouse hello [Event Processor Host] [ EventProcessorHost] s několika příjemců.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-113">hello [Scale out event processing][Scale out Event Processing with Event Hubs] sample shows how toouse hello [Event Processor Host][EventProcessorHost] with multiple receivers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd0d9-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd0d9-114">Prerequisites</span></span>

<span data-ttu-id="cd0d9-115">toocomplete tohoto kurzu budete potřebovat hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="cd0d9-115">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="cd0d9-116">[Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="cd0d9-116">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="cd0d9-117">snímky obrazovky Hello v tomto kurzu použít Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-117">hello screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="cd0d9-118">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-118">An active Azure account.</span></span> <span data-ttu-id="cd0d9-119">Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-119">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="cd0d9-120">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="cd0d9-120">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="cd0d9-121">Vytvoření oboru názvů Event Hubs a centra událostí</span><span class="sxs-lookup"><span data-stu-id="cd0d9-121">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="cd0d9-122">prvním krokem Hello je toouse hello [portál Azure](https://portal.azure.com) toocreate a obor názvů zadejte Event Hubs a získání přihlašovacích údajů pro správu aplikace musí toocommunicate s centrem událostí hello hello.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-122">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace of type Event Hubs, and obtain hello management credentials your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="cd0d9-123">toocreate obor názvů a centra událostí, postupujte podle postupu hello v [v tomto článku](event-hubs-create.md), pak pokračujte hello následující kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-123">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), then proceed with hello following steps in this tutorial.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="cd0d9-124">Vytvoření účtu služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="cd0d9-124">Create an Azure Storage account</span></span>

<span data-ttu-id="cd0d9-125">toouse hello [Event Processor Host][EventProcessorHost], musíte mít [účet úložiště Azure][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="cd0d9-125">toouse hello [Event Processor Host][EventProcessorHost], you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="cd0d9-126">Přihlaste se toohello [portál Azure][Azure portal]a klikněte na tlačítko **nový** v hello levém horním rohu úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-126">Log on toohello [Azure portal][Azure portal], and click **New** at hello top left of hello screen.</span></span>
2. <span data-ttu-id="cd0d9-127">Klikněte na **Storage** a poté klikněte na **Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-127">Click **Storage**, then click **Storage account**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. <span data-ttu-id="cd0d9-128">V hello **vytvořit účet úložiště** okno, zadejte název pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-128">In hello **Create storage account** blade, type a name for hello storage account.</span></span> <span data-ttu-id="cd0d9-129">Vyberte příslušné předplatné Azure, skupinu prostředků a umístění v který toocreate hello prostředek.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-129">Choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> <span data-ttu-id="cd0d9-130">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-130">Then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. <span data-ttu-id="cd0d9-131">V seznamu hello účtů úložiště klikněte na tlačítko hello nově vytvořený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-131">In hello list of storage accounts, click hello newly created storage account.</span></span>
5. <span data-ttu-id="cd0d9-132">V okně účtu úložiště hello, klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-132">In hello storage account blade, click **Access keys**.</span></span> <span data-ttu-id="cd0d9-133">Zkopírujte hodnotu hello **key1** toouse později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-133">Copy hello value of **key1** toouse later in this tutorial.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a><span data-ttu-id="cd0d9-134">Vytvoření konzolové aplikace Příjemce</span><span class="sxs-lookup"><span data-stu-id="cd0d9-134">Create a receiver console application</span></span>

1. <span data-ttu-id="cd0d9-135">V sadě Visual Studio vytvořte nový projekt aplikace Visual C# plocha pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-135">In Visual Studio, create a new Visual C# Desktop App project using hello **Console  Application** project template.</span></span> <span data-ttu-id="cd0d9-136">Název projektu hello **příjemce**.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-136">Name hello project **Receiver**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. <span data-ttu-id="cd0d9-137">V Průzkumníku řešení klikněte pravým tlačítkem na hello **příjemce** projektu a pak klikněte na **spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-137">In Solution Explorer, right-click hello **Receiver** project, and then click **Manage NuGet Packages for Solution**.</span></span>
3. <span data-ttu-id="cd0d9-138">Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-138">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span></span> <span data-ttu-id="cd0d9-139">Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-139">Click **Install**, and accept hello terms of use.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    <span data-ttu-id="cd0d9-140">Visual Studio stáhne, nainstaluje a přidá odkaz toohello [centra událostí Azure Service Bus - balíček NuGet třídy EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), se všemi jeho závislostmi.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-140">Visual Studio downloads, installs, and adds a reference toohello [Azure Service Bus Event Hub - EventProcessorHost NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), with all its dependencies.</span></span>
4. <span data-ttu-id="cd0d9-141">Klikněte pravým tlačítkem na hello **příjemce** projektu, klikněte na tlačítko **přidat**a potom klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-141">Right-click hello **Receiver** project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="cd0d9-142">Pojmenujte novou třídu hello **SimpleEventProcessor**a potom klikněte na **přidat** toocreate hello třídy.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-142">Name hello new class **SimpleEventProcessor**, and then click **Add** toocreate hello class.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. <span data-ttu-id="cd0d9-143">Přidejte následující příkazy hello horní části souboru SimpleEventProcessor.cs hello hello:</span><span class="sxs-lookup"><span data-stu-id="cd0d9-143">Add hello following statements at hello top of hello SimpleEventProcessor.cs file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  <span data-ttu-id="cd0d9-144">Potom nahraďte hello následující kód pro hello textu hello třídy:</span><span class="sxs-lookup"><span data-stu-id="cd0d9-144">Then, substitute hello following code for hello body of hello class:</span></span>
    
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
    
  <span data-ttu-id="cd0d9-145">Tato třída volá hello **EventProcessorHost** tooprocess událostí přijatých z centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-145">This class is called by hello **EventProcessorHost** tooprocess events received from hello event hub.</span></span> <span data-ttu-id="cd0d9-146">Hello `SimpleEventProcessor` třída používá metodu stopky tooperiodically volání hello kontrolního bodu na hello **EventProcessorHost** kontextu.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-146">hello `SimpleEventProcessor` class uses a stopwatch tooperiodically call hello checkpoint method on hello **EventProcessorHost** context.</span></span> <span data-ttu-id="cd0d9-147">Zpracování zajistí, že pokud je restartován hello příjemce, ztratí více než pět minut práce potřebné ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-147">This processing ensures that, if hello receiver is restarted, it loses no more than five minutes of processing work.</span></span>
6. <span data-ttu-id="cd0d9-148">V hello **programu** třídy, přidejte následující hello `using` příkaz hello horní části souboru hello:</span><span class="sxs-lookup"><span data-stu-id="cd0d9-148">In hello **Program** class, add hello following `using` statement at hello top of hello file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  <span data-ttu-id="cd0d9-149">Potom nahraďte hello `Main` metoda v hello `Program` třídy s hello následující kód, nahraďte název centra událostí hello a připojení hello úrovni oboru názvů řetězce, který jste předtím uložili, a hello účet úložiště a klíč, který jste zkopírovali v hello předchozí části.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-149">Then, replace hello `Main` method in hello `Program` class with hello following code, substituting hello event hub name and hello namespace-level connection string that you saved previously, and hello storage account and key that you copied in hello previous sections.</span></span> 
    
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
    
    Console.WriteLine("Receiving. Press enter key toostop worker.");
    Console.ReadLine();
    eventProcessorHost.UnregisterEventProcessorAsync().Wait();
  }
  ```

7. <span data-ttu-id="cd0d9-150">Spuštění programu hello a ujistěte se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-150">Run hello program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="cd0d9-151">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="cd0d9-151">Congratulations!</span></span> <span data-ttu-id="cd0d9-152">Obdrželi jste nyní zprávy z centra událostí pomocí hello Event Processor Host.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-152">You have now received messages from an event hub using hello Event Processor Host.</span></span>


> [!NOTE]
> <span data-ttu-id="cd0d9-153">Tento kurz používá jednu instanci třídy [EventProcessorHost][EventProcessorHost].</span><span class="sxs-lookup"><span data-stu-id="cd0d9-153">This tutorial uses a single instance of [EventProcessorHost][EventProcessorHost].</span></span> <span data-ttu-id="cd0d9-154">tooincrease propustnost, doporučujeme spustit více instancí [EventProcessorHost][EventProcessorHost], jak je znázorněno v hello [měřítkem kapacity zpracování událostí] [měřítkem kapacity zpracování událostí] ukázka.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-154">tooincrease throughput, it is recommended that you run multiple instances of [EventProcessorHost][EventProcessorHost], as shown in hello [Scaled out event processing][Scaled out event processing] sample.</span></span> <span data-ttu-id="cd0d9-155">V takových případech hello různých instancí automaticky koordinují mezi sebou tooload vyrovnávání hello přijatých událostí.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-155">In those cases, hello various instances automatically coordinate with each other tooload balance hello received events.</span></span> <span data-ttu-id="cd0d9-156">Pokud chcete, aby více příjemců tooeach proces *všechny* hello události, je nutné použít hello **ConsumerGroup** koncept.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-156">If you want multiple receivers tooeach process *all* hello events, you must use hello **ConsumerGroup** concept.</span></span> <span data-ttu-id="cd0d9-157">Když přijímáte události z různých počítačů, může to být užitečné toospecify názvy pro [EventProcessorHost] [ EventProcessorHost] instancí na základě u počítačů hello (nebo rolí), ve kterých jsou nasazené.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-157">When receiving events from different machines, it might be useful toospecify names for [EventProcessorHost][EventProcessorHost] instances based on hello machines (or roles) in which they are deployed.</span></span> <span data-ttu-id="cd0d9-158">Další informace o těchto tématech najdete v tématu hello [Přehled služby Event Hubs] [ Event Hubs overview] a hello [Průvodce programováním pro službu Event Hubs] [ Event Hubs Programming Guide] témata.</span><span class="sxs-lookup"><span data-stu-id="cd0d9-158">For more information about these topics, see hello [Event Hubs overview][Event Hubs overview] and hello [Event Hubs programming guide][Event Hubs Programming Guide] topics.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="cd0d9-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd0d9-159">Next steps</span></span>

<span data-ttu-id="cd0d9-160">Teď, sestavili jste funkční aplikaci, která vytvoří Centrum událostí a odesílá a přijímá data, další informace získáte hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="cd0d9-160">Now that you've built a working application that creates an event hub and sends and receives data, you can learn more by visiting hello following links:</span></span>

* <span data-ttu-id="cd0d9-161">[Event Processor Host][Event Processor Host]</span><span class="sxs-lookup"><span data-stu-id="cd0d9-161">[Event Processor Host][Event Processor Host]</span></span>
* <span data-ttu-id="cd0d9-162">[Přehled služby Event Hubs][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="cd0d9-162">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="cd0d9-163">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="cd0d9-163">Event Hubs FAQ</span></span>](event-hubs-faq.md)

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
