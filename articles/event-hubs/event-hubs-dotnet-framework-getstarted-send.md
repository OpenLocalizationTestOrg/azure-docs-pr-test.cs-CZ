---
title: "aaaSend události tooAzure Event Hubs pomocí rozhraní .NET Framework hello | Microsoft Docs"
description: "Začínáme odesílání událostí tooEvent Hubs pomocí hello rozhraní .NET Framework"
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
ms.openlocfilehash: 05514546a6094096e4a3c800db058190076de80a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-hello-net-framework"></a>Odesílání událostí tooAzure Event Hubs pomocí hello rozhraní .NET Framework

## <a name="introduction"></a>Úvod

Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací. Po shromažďovat data do centra událostí, můžete ukládat data hello pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele analýzu v reálném čase. Tato schopnost shromažďovat a zpracovávat rozsáhlé událostí je klíčovou komponentou moderních aplikačních architektur, například hello Internet věcí (IoT).

Tento kurz ukazuje, jak toouse hello [portál Azure](https://portal.azure.com) toocreate centra událostí. Také ukazuje, jak hello toosend události tooan centra událostí pomocí konzolové aplikace napsané v C# pomocí rozhraní .NET Framework. tooreceive událostí pomocí hello rozhraní .NET Framework, najdete v části hello [přijímat události pomocí hello rozhraní .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) článek, nebo klikněte na příslušný přijímající jazyk hello v levé tabulce hello obsahu.

toocomplete tohoto kurzu budete potřebovat hello následující požadavky:

* [Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com). snímky obrazovky Hello v tomto kurzu použít Visual Studio 2017.
* Aktivní účet Azure. Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Vytvoření oboru názvů Event Hubs a centra událostí

prvním krokem Hello je toouse hello [portál Azure](https://portal.azure.com) toocreate a obor názvů zadejte Event Hubs a získání přihlašovacích údajů pro správu aplikace musí toocommunicate s centrem událostí hello hello. toocreate obor názvů a centra událostí, postupujte podle postupu hello v [v tomto článku](event-hubs-create.md), pak pokračujte hello následující kroky v tomto kurzu.

## <a name="create-a-sender-console-application"></a>Vytvoření konzolové aplikace Odesílatel

V této části napíšete konzolovou aplikaci systému Windows, který odesílá centra událostí tooyour události.

1. V sadě Visual Studio vytvořte nový projekt aplikace Visual C# plocha pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **odesílatele**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **odesílatele** projektu a pak klikněte na **spravovat balíčky NuGet pro řešení**. 
3. Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello. 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    Visual Studio stáhne, nainstaluje a přidá odkaz toohello [balíček NuGet knihovny Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus).
4. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. Přidejte následující pole toohello hello **Program** třída, nahraďte zástupný symbol hodnoty hello s hello název centra událostí hello jste vytvořili v předchozí části hello a hello úrovni oboru názvů připojovacího řetězce jste si předtím uložili.
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. Přidejte následující metodu toohello hello **Program** třídy:
   
  ```csharp
  static void SendingRandomMessages()
  {
      var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
      while (true)
      {
          try
          {
              var message = Guid.NewGuid().ToString();
              Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
              eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
          }
          catch (Exception exception)
          {
              Console.ForegroundColor = ConsoleColor.Red;
              Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
              Console.ResetColor();
          }
   
          Thread.Sleep(200);
      }
  }
  ```
   
  Tato metoda neustále odesílá události tooyour centra událostí se zpožděním 200 ms.
7. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
  ```csharp
  Console.WriteLine("Press Ctrl-C toostop hello sender process");
  Console.WriteLine("Press Enter toostart now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. Spuštění programu hello a ujistěte se, že nejsou žádné chyby.
  
Blahopřejeme! Nyní jste odeslali centra událostí tooan zprávy.

## <a name="next-steps"></a>Další kroky
Teď, sestavili jste funkční aplikaci, která vytvoří Centrum událostí a odesílá data, můžete přesunout na toohello následující scénáře:

* [Přijímat události pomocí hello Event Processor Host](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [Referenční informace ke třídě EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

