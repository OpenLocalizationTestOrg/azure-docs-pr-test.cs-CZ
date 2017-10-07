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
# <a name="receive-events-from-azure-event-hubs-using-hello-net-framework"></a>Přijímat události z Azure Event Hubs pomocí hello rozhraní .NET Framework

## <a name="introduction"></a>Úvod

Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací. Po shromažďovat data do centra událostí, můžete ukládat data hello pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele analýzu v reálném čase. Tato schopnost shromažďovat a zpracovávat rozsáhlé událostí je klíčovou komponentou moderních aplikačních architektur, například hello Internet věcí (IoT).

Tento kurz ukazuje, jak toowrite rozhraní .NET Framework Konzolová aplikace, která přijímá zprávy z centra událostí pomocí hello  **[Event Processor Host][EventProcessorHost]**. toosend událostí pomocí hello rozhraní .NET Framework, najdete v části hello [odesílat události tooAzure Event Hubs pomocí hello rozhraní .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) článek, nebo klikněte na příslušný odesílání jazyk hello v levé tabulce hello obsahu.

Hello [Event Processor Host] [ EventProcessorHost] je třída rozhraní .NET, která zjednodušuje přijímání události ze služby event hubs tím, že spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby event hubs. Pomocí hello [Event Processor Host][Event Processor Host], i když jsou hostované v různých uzlech můžete události rozdělit mezi několik příjemců. Tento příklad ukazuje, jak toouse hello [Event Processor Host] [ EventProcessorHost] pro jednoho příjemce. Hello [horizontální navýšení kapacity zpracování událostí] [ Scale out Event Processing with Event Hubs] ukázkové ukazuje, jak toouse hello [Event Processor Host] [ EventProcessorHost] s několika příjemců.

## <a name="prerequisites"></a>Požadavky

toocomplete tohoto kurzu budete potřebovat hello následující požadavky:

* [Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com). snímky obrazovky Hello v tomto kurzu použít Visual Studio 2017.
* Aktivní účet Azure. Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Vytvoření oboru názvů Event Hubs a centra událostí

prvním krokem Hello je toouse hello [portál Azure](https://portal.azure.com) toocreate a obor názvů zadejte Event Hubs a získání přihlašovacích údajů pro správu aplikace musí toocommunicate s centrem událostí hello hello. toocreate obor názvů a centra událostí, postupujte podle postupu hello v [v tomto článku](event-hubs-create.md), pak pokračujte hello následující kroky v tomto kurzu.

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu služby Azure Storage

toouse hello [Event Processor Host][EventProcessorHost], musíte mít [účet úložiště Azure][Azure Storage account]:

1. Přihlaste se toohello [portál Azure][Azure portal]a klikněte na tlačítko **nový** v hello levém horním rohu úvodní obrazovka.
2. Klikněte na **Storage** a poté klikněte na **Účet úložiště**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. V hello **vytvořit účet úložiště** okno, zadejte název pro účet úložiště hello. Vyberte příslušné předplatné Azure, skupinu prostředků a umístění v který toocreate hello prostředek. Poté klikněte na **Vytvořit**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. V seznamu hello účtů úložiště klikněte na tlačítko hello nově vytvořený účet úložiště.
5. V okně účtu úložiště hello, klikněte na tlačítko **přístupové klíče**. Zkopírujte hodnotu hello **key1** toouse později v tomto kurzu.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a>Vytvoření konzolové aplikace Příjemce

1. V sadě Visual Studio vytvořte nový projekt aplikace Visual C# plocha pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **příjemce**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **příjemce** projektu a pak klikněte na **spravovat balíčky NuGet pro řešení**.
3. Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio stáhne, nainstaluje a přidá odkaz toohello [centra událostí Azure Service Bus - balíček NuGet třídy EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), se všemi jeho závislostmi.
4. Klikněte pravým tlačítkem na hello **příjemce** projektu, klikněte na tlačítko **přidat**a potom klikněte na **třída**. Pojmenujte novou třídu hello **SimpleEventProcessor**a potom klikněte na **přidat** toocreate hello třídy.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. Přidejte následující příkazy hello horní části souboru SimpleEventProcessor.cs hello hello:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  Potom nahraďte hello následující kód pro hello textu hello třídy:
    
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
    
  Tato třída volá hello **EventProcessorHost** tooprocess událostí přijatých z centra událostí hello. Hello `SimpleEventProcessor` třída používá metodu stopky tooperiodically volání hello kontrolního bodu na hello **EventProcessorHost** kontextu. Zpracování zajistí, že pokud je restartován hello příjemce, ztratí více než pět minut práce potřebné ke zpracování.
6. V hello **programu** třídy, přidejte následující hello `using` příkaz hello horní části souboru hello:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  Potom nahraďte hello `Main` metoda v hello `Program` třídy s hello následující kód, nahraďte název centra událostí hello a připojení hello úrovni oboru názvů řetězce, který jste předtím uložili, a hello účet úložiště a klíč, který jste zkopírovali v hello předchozí části. 
    
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

7. Spuštění programu hello a ujistěte se, že nejsou žádné chyby.
  
Blahopřejeme! Obdrželi jste nyní zprávy z centra událostí pomocí hello Event Processor Host.


> [!NOTE]
> Tento kurz používá jednu instanci třídy [EventProcessorHost][EventProcessorHost]. tooincrease propustnost, doporučujeme spustit více instancí [EventProcessorHost][EventProcessorHost], jak je znázorněno v hello [měřítkem kapacity zpracování událostí] [měřítkem kapacity zpracování událostí] ukázka. V takových případech hello různých instancí automaticky koordinují mezi sebou tooload vyrovnávání hello přijatých událostí. Pokud chcete, aby více příjemců tooeach proces *všechny* hello události, je nutné použít hello **ConsumerGroup** koncept. Když přijímáte události z různých počítačů, může to být užitečné toospecify názvy pro [EventProcessorHost] [ EventProcessorHost] instancí na základě u počítačů hello (nebo rolí), ve kterých jsou nasazené. Další informace o těchto tématech najdete v tématu hello [Přehled služby Event Hubs] [ Event Hubs overview] a hello [Průvodce programováním pro službu Event Hubs] [ Event Hubs Programming Guide] témata.
> 
> 

## <a name="next-steps"></a>Další kroky

Teď, sestavili jste funkční aplikaci, která vytvoří Centrum událostí a odesílá a přijímá data, další informace získáte hello následující odkazy:

* [Event Processor Host][Event Processor Host]
* [Přehled služby Event Hubs][Event Hubs overview]
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

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
