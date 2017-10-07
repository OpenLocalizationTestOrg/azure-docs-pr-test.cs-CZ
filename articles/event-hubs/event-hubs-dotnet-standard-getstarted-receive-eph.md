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
# <a name="get-started-receiving-messages-with-hello-event-processor-host-in-net-standard"></a>Začínáme přijímání zpráv s hello Event Processor Host v .NET Standard

> [!NOTE]
> Tato ukázka je dostupná na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).

Tento kurz ukazuje, jak toowrite .NET Core Konzolová aplikace, která přijímá zprávy z centra událostí pomocí **EventProcessorHost**. Můžete spustit hello [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) řešení jako-se nahrazování řetězců hello s událostí hodnoty účet rozbočovače a úložiště. Nebo můžete provést hello kroky tento kurz toocreate vlastní.

## <a name="prerequisites"></a>Požadavky

* [Sadu Microsoft Visual Studio 2015 nebo 2017](http://www.visualstudio.com). Hello příklady v tomto kurzu Visual Studio 2017, ale Visual Studio 2015 je také podporována.
* [.NET core Visual Studio 2015 nebo 2017 nástroje](http://www.microsoft.com/net/core).
* Předplatné Azure.
* Na obor názvů Azure Event Hubs.
* Účet úložiště Azure.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Vytvoření oboru názvů Event Hubs a centra událostí  

prvním krokem Hello je toouse hello [portál Azure](https://portal.azure.com) toocreate obor názvů pro hello Event Hubs zadejte a získání přihlašovacích údajů pro správu, aplikace musí toocommunicate s centrem událostí hello hello. toocreate obor názvů a centra událostí, postupujte podle postupu hello v [v tomto článku](event-hubs-create.md)a poté pokračovat hello následující kroky.  

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure  

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).  
2. V levém navigačním podokně hello hello portálu, klikněte na **nový**, klikněte na tlačítko **úložiště**a potom klikněte na **účet úložiště**.  
3. Vyplňte pole hello v okně účtu úložiště hello a pak klikněte na tlačítko **vytvořit**.

    ![Vytvořit účet úložiště][1]

4. Jakmile ověříte hello **úspěšné nasazení** zprávy, klikněte na název hello hello nový účet úložiště. V hello **Essentials** okně klikněte na tlačítko **objekty BLOB**. Když hello **služba objektů Blob** okno otevře, klikněte na tlačítko **+ kontejner** v horní části hello. Pojmenujte hello kontejner a pak zavřete hello **služba objektů Blob** okno.  
5. Klikněte na tlačítko **přístupové klíče** v hello levém okně a zkopírujte hello názvu kontejneru úložiště hello, účet úložiště hello a hodnota hello **key1**. Uložte tyto hodnoty tooNotepad nebo některé dočasné umístění.  

## <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Spusťte Visual Studio. Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**. Vytvoření aplikace konzoly .NET Core.

![Nový projekt][2]

## <a name="add-hello-event-hubs-nuget-package"></a>Přidání balíčku NuGet centra událostí hello

Přidat hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) a [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard NuGet balíčky tooyour projektu knihovny pomocí následujících kroků: 

1. Klikněte pravým tlačítkem hello nově vytvořený projekt a vyberte **spravovat balíčky NuGet**.
2. Klikněte na tlačítko hello **Procházet** kartu a potom vyhledejte "Microsoft.Azure.EventHubs" a vyberte hello **Microsoft.Azure.EventHubs** balíčku. Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete toto dialogové okno.
3. Opakujte kroky 1 a 2 a nainstalujte hello **Microsoft.Azure.EventHubs.Processor** balíčku.

## <a name="implement-hello-ieventprocessor-interface"></a>Implementovat rozhraní IEventProcessor hello

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello, klikněte na tlačítko **přidat**a potom klikněte na **třída**. Pojmenujte novou třídu hello **SimpleEventProcessor**.

2. Otevřete na začátek souboru SimpleEventProcessor.cs hello a přidejte následující hello `using` toohello příkazy na začátek souboru hello.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implementace hello `IEventProcessor` rozhraní. Nahraďte hello celý obsah hello `SimpleEventProcessor` se hello následující kód:

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

## <a name="write-a-main-console-method-that-uses-hello-simpleeventprocessor-class-tooreceive-messages"></a>Napíše metoda hlavní konzoly, která používá hello SimpleEventProcessor třída tooreceive zprávy

1. Přidejte následující hello `using` toohello příkazy na začátku souboru Program.cs hello.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Přidat konstanty toohello `Program` třídu pro hello event hub připojovací řetězec, název centra událostí, název kontejneru účtu úložiště, název účtu úložiště a klíč účtu úložiště. Přidejte následující kód, jejich příslušné hodnoty nahraďte zástupné symboly hello hello.

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Přidat novou metodu s názvem `MainAsync` toohello `Program` třídy následujícím způsobem:

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

3. Přidejte následující řádek kódu toohello hello `Main` metoda:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Soubor Program.cs by měl vypadat takhle:

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

4. Spuštění programu hello a ujistěte se, že nejsou žádné chyby.

Blahopřejeme! Nyní máte přijímá zprávy z centra událostí pomocí hello Event Processor Host.

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
