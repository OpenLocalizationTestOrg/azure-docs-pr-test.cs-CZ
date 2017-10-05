---
title: "Odeslání událostí do Azure Event Hubs pomocí rozhraní .NET Framework | Dokumentace Microsoftu"
description: "Začínáme s odesíláním událostí do služby Event Hubs pomocí rozhraní .NET Framework"
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
ms.openlocfilehash: 4eb0e7bcc14722010121c2a5945509d6ed736f4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="send-events-to-azure-event-hubs-using-the-net-framework"></a><span data-ttu-id="a42f7-103">Odeslání událostí do Azure Event Hubs pomocí rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="a42f7-103">Send events to Azure Event Hubs using the .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="a42f7-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="a42f7-104">Introduction</span></span>

<span data-ttu-id="a42f7-105">Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací.</span><span class="sxs-lookup"><span data-stu-id="a42f7-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="a42f7-106">Data, která shromáždíte pomocí služby Event Hubs, můžete uložit pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele datové analýzy v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="a42f7-106">After you collect data into Event Hubs, you can store the data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="a42f7-107">Schopnost shromažďovat a zpracovávat velké množství událostí je klíčovou komponentou moderních aplikačních architektur, například internetu věcí (Internet of Things – IoT).</span><span class="sxs-lookup"><span data-stu-id="a42f7-107">This large-scale event collection and processing capability is a key component of modern application architectures including the Internet of Things (IoT).</span></span>

<span data-ttu-id="a42f7-108">Díky tomuto kurzu se dozvíte, jak pomocí webu [Azure Portal](https://portal.azure.com) vytvořit centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="a42f7-108">This tutorial shows how to use the [Azure portal](https://portal.azure.com) to create an event hub.</span></span> <span data-ttu-id="a42f7-109">Také ukazuje, jak odesílat události do centra událostí pomocí konzolové aplikace napsané v jazyce C# s použitím rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a42f7-109">It also shows how to send events to an event hub using a console application written in C# using the .NET Framework.</span></span> <span data-ttu-id="a42f7-110">Pokud chcete přijímat události pomocí rozhraní .NET Framework, přečtěte si článek [Příjem událostí pomocí rozhraní .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) nebo klikněte na příslušný přijímající jazyk v obsahu vlevo.</span><span class="sxs-lookup"><span data-stu-id="a42f7-110">To receive events using the .NET Framework, see the [Receive events using the .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) article, or click the appropriate receiving language in the left-hand table of contents.</span></span>

<span data-ttu-id="a42f7-111">Pro absolvování tohoto kurzu musí být splněné následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="a42f7-111">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="a42f7-112">[Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="a42f7-112">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="a42f7-113">Pro snímky obrazovky v tomto kurzu se používá Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a42f7-113">The screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="a42f7-114">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a42f7-114">An active Azure account.</span></span> <span data-ttu-id="a42f7-115">Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut.</span><span class="sxs-lookup"><span data-stu-id="a42f7-115">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="a42f7-116">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="a42f7-116">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="a42f7-117">Vytvoření oboru názvů Event Hubs a centra událostí</span><span class="sxs-lookup"><span data-stu-id="a42f7-117">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="a42f7-118">Prvním krokem je použití webu [Azure Portal](https://portal.azure.com) k vytvoření oboru názvů typu Event Hubs a získání přihlašovacích údajů pro správu, které vaše aplikace potřebuje ke komunikaci s centrem událostí.</span><span class="sxs-lookup"><span data-stu-id="a42f7-118">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace of type Event Hubs, and obtain the management credentials your application needs to communicate with the event hub.</span></span> <span data-ttu-id="a42f7-119">Pokud chcete vytvořit obor názvů a centrum událostí, postupujte podle pokynů v [tomto článku](event-hubs-create.md) a pak pokračujte podle následujících pokynů v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a42f7-119">To create a namespace and event hub, follow the procedure in [this article](event-hubs-create.md), then proceed with the following steps in this tutorial.</span></span>

## <a name="create-a-sender-console-application"></a><span data-ttu-id="a42f7-120">Vytvoření konzolové aplikace Odesílatel</span><span class="sxs-lookup"><span data-stu-id="a42f7-120">Create a sender console application</span></span>

<span data-ttu-id="a42f7-121">V této části napíšete konzolovou aplikaci pro Windows, která zasílá události do vašeho centra událostí.</span><span class="sxs-lookup"><span data-stu-id="a42f7-121">In this section, you'll write a Windows console app that sends events to your event hub.</span></span>

1. <span data-ttu-id="a42f7-122">Pomocí šablony projektu **Konzolová aplikace** vytvořte v sadě Visual Studio nový projekt desktopové aplikace Visual C#.</span><span class="sxs-lookup"><span data-stu-id="a42f7-122">In Visual Studio, create a new Visual C# Desktop App project using the **Console Application** project template.</span></span> <span data-ttu-id="a42f7-123">Projekt pojmenujte **Odesílatel**.</span><span class="sxs-lookup"><span data-stu-id="a42f7-123">Name the project **Sender**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. <span data-ttu-id="a42f7-124">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **Sender** a potom klikněte na **Spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="a42f7-124">In Solution Explorer, right-click the **Sender** project, and then click **Manage NuGet Packages for Solution**.</span></span> 
3. <span data-ttu-id="a42f7-125">Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="a42f7-125">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="a42f7-126">Klikněte na **Instalovat** a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="a42f7-126">Click **Install**, and accept the terms of use.</span></span> 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    <span data-ttu-id="a42f7-127">Visual Studio stáhne a nainstaluje [balíček NuGet knihovny Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="a42f7-127">Visual Studio downloads, installs, and adds a reference to the [Azure Service Bus library NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span></span>
4. <span data-ttu-id="a42f7-128">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="a42f7-128">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. <span data-ttu-id="a42f7-129">K třídě **Program** přidejte následující pole a zástupné hodnoty nahraďte názvem centra událostí, které jste vytvořili v předchozí části, a připojovacím řetězcem na úrovni oboru názvů, který jste si předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="a42f7-129">Add the following fields to the **Program** class, substituting the placeholder values with the name of the event hub you created in the previous section, and the namespace-level connection string you saved previously.</span></span>
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. <span data-ttu-id="a42f7-130">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="a42f7-130">Add the following method to the **Program** class:</span></span>
   
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
   
  <span data-ttu-id="a42f7-131">Tato metoda neustále odesílá události do vašeho centra událostí se zpožděním 200 ms.</span><span class="sxs-lookup"><span data-stu-id="a42f7-131">This method continuously sends events to your event hub with a 200-ms delay.</span></span>
7. <span data-ttu-id="a42f7-132">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="a42f7-132">Finally, add the following lines to the **Main** method:</span></span>
   
  ```csharp
  Console.WriteLine("Press Ctrl-C to stop the sender process");
  Console.WriteLine("Press Enter to start now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. <span data-ttu-id="a42f7-133">Spusťte program a zkontrolujte, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="a42f7-133">Run the program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="a42f7-134">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="a42f7-134">Congratulations!</span></span> <span data-ttu-id="a42f7-135">Nyní jste odeslali zprávy do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="a42f7-135">You have now sent messages to an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a42f7-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a42f7-136">Next steps</span></span>
<span data-ttu-id="a42f7-137">Gratulujeme, sestavili jste funkční aplikaci, která vytvoří centrum událostí a odesílá data. Nyní se můžete podívat na některý z následujících scénářů:</span><span class="sxs-lookup"><span data-stu-id="a42f7-137">Now that you've built a working application that creates an event hub and sends data, you can move on to the following scenarios:</span></span>

* [<span data-ttu-id="a42f7-138">Příjem událostí pomocí třídy EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="a42f7-138">Receive events using the Event Processor Host</span></span>](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [<span data-ttu-id="a42f7-139">Referenční informace ke třídě EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="a42f7-139">Event Processor Host reference</span></span>](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [<span data-ttu-id="a42f7-140">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a42f7-140">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

