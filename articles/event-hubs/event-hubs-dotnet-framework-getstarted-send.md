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
# <a name="send-events-tooazure-event-hubs-using-hello-net-framework"></a><span data-ttu-id="6f77d-103">Odesílání událostí tooAzure Event Hubs pomocí hello rozhraní .NET Framework</span><span class="sxs-lookup"><span data-stu-id="6f77d-103">Send events tooAzure Event Hubs using hello .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="6f77d-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="6f77d-104">Introduction</span></span>

<span data-ttu-id="6f77d-105">Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací.</span><span class="sxs-lookup"><span data-stu-id="6f77d-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="6f77d-106">Po shromažďovat data do centra událostí, můžete ukládat data hello pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele analýzu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="6f77d-106">After you collect data into Event Hubs, you can store hello data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="6f77d-107">Tato schopnost shromažďovat a zpracovávat rozsáhlé událostí je klíčovou komponentou moderních aplikačních architektur, například hello Internet věcí (IoT).</span><span class="sxs-lookup"><span data-stu-id="6f77d-107">This large-scale event collection and processing capability is a key component of modern application architectures including hello Internet of Things (IoT).</span></span>

<span data-ttu-id="6f77d-108">Tento kurz ukazuje, jak toouse hello [portál Azure](https://portal.azure.com) toocreate centra událostí.</span><span class="sxs-lookup"><span data-stu-id="6f77d-108">This tutorial shows how toouse hello [Azure portal](https://portal.azure.com) toocreate an event hub.</span></span> <span data-ttu-id="6f77d-109">Také ukazuje, jak hello toosend události tooan centra událostí pomocí konzolové aplikace napsané v C# pomocí rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6f77d-109">It also shows how toosend events tooan event hub using a console application written in C# using hello .NET Framework.</span></span> <span data-ttu-id="6f77d-110">tooreceive událostí pomocí hello rozhraní .NET Framework, najdete v části hello [přijímat události pomocí hello rozhraní .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) článek, nebo klikněte na příslušný přijímající jazyk hello v levé tabulce hello obsahu.</span><span class="sxs-lookup"><span data-stu-id="6f77d-110">tooreceive events using hello .NET Framework, see hello [Receive events using hello .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) article, or click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="6f77d-111">toocomplete tohoto kurzu budete potřebovat hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="6f77d-111">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="6f77d-112">[Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="6f77d-112">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="6f77d-113">snímky obrazovky Hello v tomto kurzu použít Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6f77d-113">hello screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="6f77d-114">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="6f77d-114">An active Azure account.</span></span> <span data-ttu-id="6f77d-115">Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut.</span><span class="sxs-lookup"><span data-stu-id="6f77d-115">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="6f77d-116">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6f77d-116">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="6f77d-117">Vytvoření oboru názvů Event Hubs a centra událostí</span><span class="sxs-lookup"><span data-stu-id="6f77d-117">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="6f77d-118">prvním krokem Hello je toouse hello [portál Azure](https://portal.azure.com) toocreate a obor názvů zadejte Event Hubs a získání přihlašovacích údajů pro správu aplikace musí toocommunicate s centrem událostí hello hello.</span><span class="sxs-lookup"><span data-stu-id="6f77d-118">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace of type Event Hubs, and obtain hello management credentials your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="6f77d-119">toocreate obor názvů a centra událostí, postupujte podle postupu hello v [v tomto článku](event-hubs-create.md), pak pokračujte hello následující kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6f77d-119">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), then proceed with hello following steps in this tutorial.</span></span>

## <a name="create-a-sender-console-application"></a><span data-ttu-id="6f77d-120">Vytvoření konzolové aplikace Odesílatel</span><span class="sxs-lookup"><span data-stu-id="6f77d-120">Create a sender console application</span></span>

<span data-ttu-id="6f77d-121">V této části napíšete konzolovou aplikaci systému Windows, který odesílá centra událostí tooyour události.</span><span class="sxs-lookup"><span data-stu-id="6f77d-121">In this section, you'll write a Windows console app that sends events tooyour event hub.</span></span>

1. <span data-ttu-id="6f77d-122">V sadě Visual Studio vytvořte nový projekt aplikace Visual C# plocha pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="6f77d-122">In Visual Studio, create a new Visual C# Desktop App project using hello **Console Application** project template.</span></span> <span data-ttu-id="6f77d-123">Název projektu hello **odesílatele**.</span><span class="sxs-lookup"><span data-stu-id="6f77d-123">Name hello project **Sender**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. <span data-ttu-id="6f77d-124">V Průzkumníku řešení klikněte pravým tlačítkem na hello **odesílatele** projektu a pak klikněte na **spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="6f77d-124">In Solution Explorer, right-click hello **Sender** project, and then click **Manage NuGet Packages for Solution**.</span></span> 
3. <span data-ttu-id="6f77d-125">Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="6f77d-125">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="6f77d-126">Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="6f77d-126">Click **Install**, and accept hello terms of use.</span></span> 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    <span data-ttu-id="6f77d-127">Visual Studio stáhne, nainstaluje a přidá odkaz toohello [balíček NuGet knihovny Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="6f77d-127">Visual Studio downloads, installs, and adds a reference toohello [Azure Service Bus library NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span></span>
4. <span data-ttu-id="6f77d-128">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="6f77d-128">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. <span data-ttu-id="6f77d-129">Přidejte následující pole toohello hello **Program** třída, nahraďte zástupný symbol hodnoty hello s hello název centra událostí hello jste vytvořili v předchozí části hello a hello úrovni oboru názvů připojovacího řetězce jste si předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="6f77d-129">Add hello following fields toohello **Program** class, substituting hello placeholder values with hello name of hello event hub you created in hello previous section, and hello namespace-level connection string you saved previously.</span></span>
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. <span data-ttu-id="6f77d-130">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="6f77d-130">Add hello following method toohello **Program** class:</span></span>
   
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
   
  <span data-ttu-id="6f77d-131">Tato metoda neustále odesílá události tooyour centra událostí se zpožděním 200 ms.</span><span class="sxs-lookup"><span data-stu-id="6f77d-131">This method continuously sends events tooyour event hub with a 200-ms delay.</span></span>
7. <span data-ttu-id="6f77d-132">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="6f77d-132">Finally, add hello following lines toohello **Main** method:</span></span>
   
  ```csharp
  Console.WriteLine("Press Ctrl-C toostop hello sender process");
  Console.WriteLine("Press Enter toostart now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. <span data-ttu-id="6f77d-133">Spuštění programu hello a ujistěte se, že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="6f77d-133">Run hello program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="6f77d-134">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="6f77d-134">Congratulations!</span></span> <span data-ttu-id="6f77d-135">Nyní jste odeslali centra událostí tooan zprávy.</span><span class="sxs-lookup"><span data-stu-id="6f77d-135">You have now sent messages tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f77d-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f77d-136">Next steps</span></span>
<span data-ttu-id="6f77d-137">Teď, sestavili jste funkční aplikaci, která vytvoří Centrum událostí a odesílá data, můžete přesunout na toohello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="6f77d-137">Now that you've built a working application that creates an event hub and sends data, you can move on toohello following scenarios:</span></span>

* [<span data-ttu-id="6f77d-138">Přijímat události pomocí hello Event Processor Host</span><span class="sxs-lookup"><span data-stu-id="6f77d-138">Receive events using hello Event Processor Host</span></span>](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [<span data-ttu-id="6f77d-139">Referenční informace ke třídě EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="6f77d-139">Event Processor Host reference</span></span>](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [<span data-ttu-id="6f77d-140">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="6f77d-140">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

