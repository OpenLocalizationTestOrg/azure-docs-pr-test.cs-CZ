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
# <a name="get-started-sending-messages-tooazure-event-hubs-in-net-standard"></a>Začínáme odesílání zpráv v rozhraní .NET standardní tooAzure Event Hubs

> [!NOTE]
> Tato ukázka je dostupná na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Tento kurz ukazuje, jak toowrite konzolovou aplikaci .NET Core, která odesílá zprávy tooan centra událostí. Můžete spustit hello [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) řešení jako-, nahrazuje hello `EhConnectionString` a `EhEntityPath` řetězce hodnotami centra událostí. Nebo můžete provést hello kroky tento kurz toocreate vlastní.

## <a name="prerequisites"></a>Požadavky

* [Sadu Microsoft Visual Studio 2015 nebo 2017](http://www.visualstudio.com). Hello příklady v tomto kurzu Visual Studio 2017, ale Visual Studio 2015 je také podporována.
* [.NET core Visual Studio 2015 nebo 2017 nástroje](http://www.microsoft.com/net/core).
* Předplatné Azure.
* Na obor názvů centra událostí.

centra událostí tooan toosend zprávy, budeme používat Visual Studio toowrite konzolovou aplikaci C#.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Vytvoření oboru názvů Event Hubs a centra událostí

prvním krokem Hello je toouse hello [portál Azure](https://portal.azure.com) toocreate obor názvů pro typ rozbočovače hello události a získání přihlašovacích údajů pro správu, aplikace musí toocommunicate s centrem událostí hello hello. toocreate obor názvů a centra událostí, postupujte podle postupu hello v [v tomto článku](event-hubs-create.md)a poté pokračovat hello následující kroky.

## <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Spusťte Visual Studio. Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**. Vytvoření aplikace konzoly .NET Core.

![Nový projekt][1]

## <a name="add-hello-event-hubs-nuget-package"></a>Přidání balíčku NuGet centra událostí hello

Přidat hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard NuGet balíček tooyour projektu knihovny pomocí následujících kroků: 

1. Klikněte pravým tlačítkem hello nově vytvořený projekt a vyberte **spravovat balíčky NuGet**.
2. Klikněte na tlačítko hello **Procházet** kartu a potom vyhledejte "Microsoft.Azure.EventHubs" a vyberte hello **Microsoft.Azure.EventHubs** balíčku. Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete toto dialogové okno.

## <a name="write-some-code-toosend-messages-toohello-event-hub"></a>Zápis centra událostí toohello některé kód toosend zprávy

1. Přidejte následující hello `using` toohello příkazy na začátku souboru Program.cs hello.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Přidat konstanty toohello `Program` třídu pro hello Event Hubs, připojovací řetězec a entity cesta (název rozbočovače jednotlivých událostí). Nahraďte zástupné symboly hello v závorkách hello správné hodnoty, které byly získány při vytváření centra událostí hello.

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Přidat novou metodu s názvem `MainAsync` toohello `Program` třídy následujícím způsobem:

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

4. Přidat novou metodu s názvem `SendMessagesToEventHub` toohello `Program` třídy následujícím způsobem:

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

5. Přidejte následující kód toohello hello `Main` metoda v hello `Program` třídy.

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Soubor Program.cs by měl vypadat takhle.

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

6. Spuštění programu hello a ujistěte se, že nejsou žádné chyby.

Blahopřejeme! Nyní jste odeslali centra událostí tooan zprávy.

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přijímat události ze služby Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
