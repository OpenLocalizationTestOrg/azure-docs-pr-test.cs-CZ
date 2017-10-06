---
title: "aaaGet začít s fronty Azure Service Bus | Microsoft Docs"
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
ms.openlocfilehash: eaa362ab0eabd2427977398c1deab5dc00105ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-queues"></a>Začínáme s frontami služby Service Bus
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme
Tento kurz se zabývá hello následující kroky:

1. Vytvořte obor názvů Service Bus pomocí hello portálu Azure.
2. Vytvoření fronty Service Bus, pomocí hello portálu Azure.
3. Zápis konzoly aplikace toosend zprávy.
4. Zápis konzoly aplikace tooreceive hello zprávy odeslané v předchozím kroku hello.

## <a name="prerequisites"></a>Požadavky
1. [Visual Studio 2015 nebo vyšší](http://www.visualstudio.com). Hello příklady v tomto kurzu použít Visual Studio 2017.
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Vytvoření oboru názvů pomocí hello portálu Azure
Pokud jste již vytvořili obor názvů zasílání zpráv Service Bus, přeskočit toohello [vytvoření fronty pomocí portálu Azure hello](#2-create-a-queue-using-the-azure-portal) části.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-hello-azure-portal"></a>2. Vytvoření fronty pomocí hello portálu Azure
Pokud jste již vytvořili fronty Service Bus, přeskočit toohello [toohello fronty pro odesílání zpráv](#3-send-messages-to-the-queue) části.

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-toohello-queue"></a>3. Odesílat zprávy fronty toohello
Fronta zpráv toohello toosend, jsme zápisu konzolovou aplikaci C# pomocí sady Visual Studio.

### <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Spusťte sadu Visual Studio a vytvořte nový projekt **Aplikace konzoly (.NET Framework)**.

### <a name="add-hello-service-bus-nuget-package"></a>Přidání balíčku Service Bus NuGet hello
1. Klikněte pravým tlačítkem hello nově vytvořený projekt a vyberte **spravovat balíčky NuGet**.
2. Klikněte na tlačítko hello **Procházet** kartě, vyhledejte **Microsoft Azure Service Bus**a potom vyberte hello **WindowsAzure.ServiceBus** položky. Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete toto dialogové okno.
   
    ![Výběr balíčku NuGet][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-queue"></a>Zápis některých kódu toosend toohello fronty zpráv
1. Přidejte následující hello `using` toohello příkaz na začátku souboru Program.cs hello.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Přidejte následující kód toohello hello `Main` metoda. Sada hello `connectionString` proměnné toohello připojovací řetězec, který jste získali při vytváření názvů hello a nastavte `queueName` toohello název fronty, který jste použili při vytvoření fronty hello.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    Soubor Program.cs by měl vypadat takhle.
   
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

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Spuštění programu hello a zkontrolujte hello portálu Azure: klikněte na název hello fronty v oboru názvů hello **přehled** okno. fronty Hello **Essentials** zobrazí se okno. Všimněte si, že hello **počet zpráv aktivní** hodnotu by teď měly být 1. Pokaždé, když spustíte aplikace sender hello bez načítání zpráv hello, tato hodnota se zvýší o 1. Všimněte si, že hello aktuální velikost fronty hello zvýší každé aplikace hello čas také přidá toohello front zpráv.
   
      ![Velikost zpráv][queue-message]

## <a name="4-receive-messages-from-hello-queue"></a>4. Příjem zpráv z fronty hello

1. tooreceive hello zprávy, které jste právě zaslali, vytvořte novou konzolovou aplikaci a přidejte odkaz na balíček Service Bus NuGet toohello, podobně jako toohello předchozí odesílatele aplikaci.
2. Přidejte následující hello `using` toohello příkaz na začátku souboru Program.cs hello.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Přidejte následující kód toohello hello `Main` metoda. Sada hello `connectionString` proměnné toohello připojovací řetězec, který byl získán při vytvoření oboru názvů hello a nastavte `queueName` toohello název fronty, který jste použili při vytvoření fronty hello.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    Soubor Program.cs by měl vypadat takhle:
   
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

          Console.WriteLine("Press ENTER tooexit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. Spuštění programu hello a znovu zkontrolujte hello portálu. Všimněte si, že hello **počet zpráv aktivní** a **aktuální** jsou hodnoty 0.
   
    ![Délka fronty][queue-message-receive]

Blahopřejeme! Vytvořili jste frontu, zaslali jste zprávu a přijali jste zprávu.

## <a name="next-steps"></a>Další kroky

Podívejte se na naše [úložiště GitHub se ukázky](https://github.com/Azure/azure-service-bus/tree/master/samples) který předvedli hello pokročilejší funkce zasílání zpráv Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
