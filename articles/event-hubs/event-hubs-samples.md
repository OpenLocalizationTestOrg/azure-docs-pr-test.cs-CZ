---
title: "Ukázek Azure Event Hubs | Microsoft Docs"
description: "Ukázek Azure Event Hubs"
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
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: ae9fbd97a1747d8f14c561f247a0973bb11fd039
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-hubs-samples"></a><span data-ttu-id="7ad54-103">Ukázky centra událostí</span><span class="sxs-lookup"><span data-stu-id="7ad54-103">Event Hubs samples</span></span> 

<span data-ttu-id="7ad54-104">Sada ukázek Azure Event Hubs ukazuje klíčové funkce v [Azure Event Hubs](/azure/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="7ad54-104">The set of Azure Event Hubs samples demonstrates key features in [Azure Event Hubs](/azure/event-hubs/).</span></span> <span data-ttu-id="7ad54-105">Tento článek rozděluje a popisuje, k dispozici, s odkazy na všechny ukázky.</span><span class="sxs-lookup"><span data-stu-id="7ad54-105">This article categorizes and describes the samples available, with links to each.</span></span>

<span data-ttu-id="7ad54-106">V době psaní tohoto textu Event Hubs ukázky umístěny v několika různých místech:</span><span class="sxs-lookup"><span data-stu-id="7ad54-106">At the time of this writing, Event Hubs samples are located in several different places:</span></span>

- [<span data-ttu-id="7ad54-107">Ukázky kódu vývojáře MSDN</span><span class="sxs-lookup"><span data-stu-id="7ad54-107">MSDN developer code samples</span></span>](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [<span data-ttu-id="7ad54-108">GitHub</span><span class="sxs-lookup"><span data-stu-id="7ad54-108">GitHub</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)

<span data-ttu-id="7ad54-109">Další informace o různých verzích rozhraní .NET Framework najdete v tématu [architektury a cíle](/dotnet/articles/standard/frameworks).</span><span class="sxs-lookup"><span data-stu-id="7ad54-109">For more information about different versions of the .NET Framework, see [Frameworks and Targets](/dotnet/articles/standard/frameworks).</span></span>

<span data-ttu-id="7ad54-110">Další ukázky bude přidán v čase, tak zkontrolujte, vraťte se sem často aktualizací.</span><span class="sxs-lookup"><span data-stu-id="7ad54-110">More samples will be added over time, so check back here frequently for updates.</span></span>

## <a name="net-standard"></a><span data-ttu-id="7ad54-111">Standardní rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7ad54-111">.NET Standard</span></span>

<span data-ttu-id="7ad54-112">Následující ukázky ukazují, jak odesílat a přijímat události pomocí [klienta služby Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) pro [.NET standardní knihovna](/dotnet/articles/standard/library).</span><span class="sxs-lookup"><span data-stu-id="7ad54-112">The following samples demonstrate how to send and receive events using the [Event Hubs client](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) for the [.NET Standard library](/dotnet/articles/standard/library).</span></span>

### <a name="send-events"></a><span data-ttu-id="7ad54-113">Odesílání událostí</span><span class="sxs-lookup"><span data-stu-id="7ad54-113">Send events</span></span> 

<span data-ttu-id="7ad54-114">[Začínáme odesílání](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) příklad ukazuje, jak psát aplikace konzoly .NET Core, která zasílá události do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="7ad54-114">The [Get started sending](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) sample shows how to write a .NET Core console application that sends events to an event hub.</span></span>

### <a name="receive-events"></a><span data-ttu-id="7ad54-115">Příjem událostí</span><span class="sxs-lookup"><span data-stu-id="7ad54-115">Receive events</span></span> 

<span data-ttu-id="7ad54-116">[Začít pracovat s Event Processor Host přijetí](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) ukázka je aplikace konzoly .NET Core, která přijímá zprávy z centra událostí pomocí Event Processor Host.</span><span class="sxs-lookup"><span data-stu-id="7ad54-116">The [Get started receiving with the Event Processor Host](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) sample is a .NET Core console application that receives messages from an event hub using the Event Processor Host.</span></span>

## <a name="net-framework"></a><span data-ttu-id="7ad54-117">Rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="7ad54-117">.NET Framework</span></span>   

<span data-ttu-id="7ad54-118">Tyto ukázky demonstrují různým funkcím služby Azure Event Hubs, cílení [rozhraní .NET Framework – knihovna](/dotnet/framework/index).</span><span class="sxs-lookup"><span data-stu-id="7ad54-118">These samples demonstrate various other features of Azure Event Hubs, targeting the [.NET Framework library](/dotnet/framework/index).</span></span>
 
### <a name="notify-users-of-events-received"></a><span data-ttu-id="7ad54-119">Upozorněte uživatele událostí přijatých</span><span class="sxs-lookup"><span data-stu-id="7ad54-119">Notify users of events received</span></span>

<span data-ttu-id="7ad54-120">[AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) ukázka upozorní uživatele z dat přijatých ze senzorů nebo jinými systémy.</span><span class="sxs-lookup"><span data-stu-id="7ad54-120">The [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) sample notifies users of data received from sensors or other systems.</span></span>

### <a name="get-started-with-event-hubs"></a><span data-ttu-id="7ad54-121">Začínáme se službou Event Hubs</span><span class="sxs-lookup"><span data-stu-id="7ad54-121">Get started with Event Hubs</span></span> 

<span data-ttu-id="7ad54-122">[Event Hubs Začínáme](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) příklad ukazuje základní možnosti služby Event Hubs, například vytvoření centra událostí, odesílání událostí do centra událostí a využívat událostí pomocí [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) .</span><span class="sxs-lookup"><span data-stu-id="7ad54-122">The [Event Hubs Getting Started](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) sample demonstrates the basic capabilities of Event Hubs, such as how to create an event hub, send events to an event hub, and consume events using the [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>

### <a name="scale-out-event-processing"></a><span data-ttu-id="7ad54-123">Horizontální navýšení kapacity zpracování událostí</span><span class="sxs-lookup"><span data-stu-id="7ad54-123">Scale out event processing</span></span> 

<span data-ttu-id="7ad54-124">[Horizontální navýšení kapacity zpracování událostí](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) příklad ukazuje způsob použití [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) distribuovat zatížení spotřeby Event Hubs datového proudu.</span><span class="sxs-lookup"><span data-stu-id="7ad54-124">The [Scale out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) sample demonstrates how to use the [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) to distribute the workload of Event Hubs stream consumption.</span></span> <span data-ttu-id="7ad54-125">Ukazuje, jak implementovat **EventProcessor** a **EventProcessorFactory** objektů ke správě datového proudu událostí.</span><span class="sxs-lookup"><span data-stu-id="7ad54-125">It shows how to implement the **EventProcessor** and **EventProcessorFactory** objects to manage the event stream.</span></span> 

###  <a name="pull-data-from-sql-into-an-event-hub"></a><span data-ttu-id="7ad54-126">Získání dat z SQL do centra událostí</span><span class="sxs-lookup"><span data-stu-id="7ad54-126">Pull data from SQL into an event hub</span></span>

<span data-ttu-id="7ad54-127">[Dat stahování SQL](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) příklad ukazuje postup vyžádá data z tabulky SQL a poslat ho do centra událostí, použít jako vstup v příjem dat analytických aplikací.</span><span class="sxs-lookup"><span data-stu-id="7ad54-127">The [Pulling SQL data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) sample shows how to pull data from a SQL table and push it to an event hub, to use as an input in downstream analytical applications.</span></span>

### <a name="pull-web-data-into-an-event-hub"></a><span data-ttu-id="7ad54-128">Získání dat webové do centra událostí</span><span class="sxs-lookup"><span data-stu-id="7ad54-128">Pull web data into an event hub</span></span> 

<span data-ttu-id="7ad54-129">[Importovat data z webu](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) příklad ukazuje, jak načítat data z veřejné informační kanály (například ministerstva dopravy na provoz informace informační kanál) a poslat ho do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="7ad54-129">The [Import data from the web](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) sample shows how to pull data from public feeds (such as the Department of Transportation's traffic information feed) and push it to an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ad54-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ad54-130">Next steps</span></span>

<span data-ttu-id="7ad54-131">Další informace o rozhraní .NET Framework verze když přejdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="7ad54-131">Learn more about .NET Framework versions by visiting the following links:</span></span>

- [<span data-ttu-id="7ad54-132">Rozhraní a cíle</span><span class="sxs-lookup"><span data-stu-id="7ad54-132">Frameworks and targets</span></span>](/dotnet/articles/standard/frameworks)
- [<span data-ttu-id="7ad54-133">Rozhraní .NET framework 4.6 a 4.5</span><span class="sxs-lookup"><span data-stu-id="7ad54-133">.NET Framework 4.6 and 4.5</span></span>](/dotnet/framework/index)

<span data-ttu-id="7ad54-134">Se více o službě Event Hubs v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="7ad54-134">You can learn more about Event Hubs in the following articles:</span></span>

- [<span data-ttu-id="7ad54-135">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="7ad54-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="7ad54-136">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="7ad54-136">Create an event hub</span></span>](event-hubs-create.md)
- [<span data-ttu-id="7ad54-137">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="7ad54-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)