---
title: "aaaGet začít s Azure Service Bus témat a odběrů | Microsoft Docs"
description: "Napíšeme aplikaci konzoly v jazyce C# využívající témata a odběry zasílání zpráv služby Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.openlocfilehash: 619d602599d97ecff2ded0681a383b19f1a8b7ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-topics"></a>Začínáme s tématy služby Service Bus

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme

Tento kurz se zabývá hello následující kroky:

1. Vytvořte obor názvů Service Bus pomocí hello portálu Azure.
2. Vytvoření tématu Service Bus, pomocí hello portálu Azure.
3. Vytvořte téma toothat předplatné služby Service Bus pomocí hello portálu Azure.
4. Zápis na konzole aplikace toosend téma toohello zprávy.
5. Zápis na konzole aplikace tooreceive zprávy z předplatného hello.

## <a name="prerequisites"></a>Požadavky

1. [Visual Studio 2015 nebo vyšší](http://www.visualstudio.com). Hello příklady v tomto kurzu použít Visual Studio 2017.
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Vytvoření oboru názvů pomocí hello portálu Azure

Pokud jste již vytvořili obor názvů zasílání zpráv Service Bus, přeskočit toohello [vytvořit téma pomocí portálu Azure hello](#2-create-a-topic-using-the-azure-portal) části.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-hello-azure-portal"></a>2. Vytvoří téma pomocí hello portálu Azure

1. Přihlaste se toohello [portál Azure][azure-portal].
2. V levém navigačním podokně hello hello portálu, klikněte na **Service Bus** (Pokud nevidíte **Service Bus**, klikněte na tlačítko **další služby**).
3. Klikněte na tlačítko hello obor názvů, ve kterém chcete toocreate hello tématu. Zobrazí se okno Přehled Hello obor názvů:
   
    ![Vytvoření tématu][createtopic1]
4. V hello **oboru názvů Service Bus** okně klikněte na tlačítko **témata**, pak klikněte na tlačítko **přidat tématu**.
   
    ![Výběr témat][createtopic2]
5. Zadejte název pro téma hello a zrušte zaškrtnutí políčka hello **povolit vytváření oddílů** možnost. Nechte hello jiné možnosti s jejich výchozí hodnoty.
   
    ![Vyberte Nový][createtopic3]
6. Hello dolní části okna hello, klikněte na **vytvořit**.

## <a name="3-create-a-subscription-toohello-topic"></a>3. Vytvoří téma toohello předplatného

1. V podokně portálu prostředky hello klikněte hello obor názvů, který jste vytvořili v kroku 1 a pak klikněte na název hello téma, které jste vytvořili v kroku 2.
2. Top hello hello přehled podokna klikněte na tlačítko hello plus přihlásit další příliš**předplatné** tooadd téma toothis předplatné.

    ![Vytvoření odběru][createtopic4]

3. Zadejte název pro předplatné hello. Nechte hello jiné možnosti s jejich výchozí hodnoty.

## <a name="4-send-messages-toohello-topic"></a>4. Odeslání zprávy toohello tématu

toosend zprávy toohello tématu jsme zápisu konzolovou aplikaci C# pomocí sady Visual Studio.

### <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Spusťte sadu Visual Studio a vytvořte nový projekt **Aplikace konzoly (.NET Framework)**.

### <a name="add-hello-service-bus-nuget-package"></a>Přidání balíčku Service Bus NuGet hello

1. Klikněte pravým tlačítkem hello nově vytvořený projekt a vyberte **spravovat balíčky NuGet**.
2. Klikněte na tlačítko hello **Procházet** kartě, vyhledejte **Microsoft Azure Service Bus**a potom vyberte hello **WindowsAzure.ServiceBus** položky. Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete toto dialogové okno.
   
    ![Výběr balíčku NuGet][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-topic"></a>Zápis některé kód toosend téma toohello zpráv

1. Přidejte následující hello `using` toohello příkaz na začátku souboru Program.cs hello.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Přidejte následující kód toohello hello `Main` metoda. Sada hello `connectionString` proměnné toohello připojovací řetězec, který jste získali při vytváření názvů hello a nastavte `topicName` toohello název, který jste použili při vytváření tématu hello.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
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

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Spuštění programu hello a zkontrolujte hello portálu Azure: klikněte na název hello témat v oboru názvů hello **přehled** okno. téma Hello **Essentials** zobrazí se okno. V uvedených téměř hello dolní části okna hello hello odběry, Všimněte si, že hello **počet zpráv** hodnota pro každé předplatné by teď měly být 1. Pokaždé, když provedete spuštění aplikace sender hello bez načítání zpráv hello (jak je popsáno v další části hello), tato hodnota se zvýší o 1. Všimněte si, že hello aktuální velikost hello tématu přírůstcích hello **aktuální** hodnota na hello **Essentials** okno pokaždé, když aplikace hello přidá zprávu toohello téma/odběr.
   
      ![Velikost zpráv][topic-message]

## <a name="5-receive-messages-from-hello-subscription"></a>5. Příjem zpráv z odběru hello

1. tooreceive uvítací zprávu nebo zpráv, které jste právě zaslali, vytvořte novou konzolovou aplikaci a přidejte odkaz na balíček Service Bus NuGet toohello, podobně jako toohello předchozí odesílatele aplikaci.
2. Přidejte následující hello `using` toohello příkaz na začátku souboru Program.cs hello.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Přidejte následující kód toohello hello `Main` metoda. Sada hello `connectionString` proměnné toohello připojovací řetězec jste získali při vytváření názvů hello a nastavte `topicName` toohello název, který jste použili při vytváření tématu hello.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. Spuštění programu hello a znovu zkontrolujte hello portálu. Všimněte si, že hello **počet zpráv** a **aktuální** jsou hodnoty 0.
   
    ![Délka tématu][topic-message-receive]

Blahopřejeme! Právě jste vytvořili téma a odběr, odeslali zprávu a přijali ji.

## <a name="next-steps"></a>Další kroky

Podívejte se na naše [úložiště GitHub se ukázky](https://github.com/Azure/azure-service-bus/tree/master/samples) který předvedli hello pokročilejší funkce zasílání zpráv Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
