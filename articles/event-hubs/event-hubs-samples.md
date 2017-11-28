---
title: "ukázky služby Event Hubs aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: f01f52e6c13f9e885999a6726143440bbc70446d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-samples"></a><span data-ttu-id="f0254-103">Ukázky centra událostí</span><span class="sxs-lookup"><span data-stu-id="f0254-103">Event Hubs samples</span></span> 

<span data-ttu-id="f0254-104">Hello sady Azure Event Hubs vzorků ukazuje klíčové funkce v [Azure Event Hubs](/azure/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="f0254-104">hello set of Azure Event Hubs samples demonstrates key features in [Azure Event Hubs](/azure/event-hubs/).</span></span> <span data-ttu-id="f0254-105">Tento článek rozděluje a popisuje hello vzorků, které jsou k dispozici, tooeach odkazy.</span><span class="sxs-lookup"><span data-stu-id="f0254-105">This article categorizes and describes hello samples available, with links tooeach.</span></span>

<span data-ttu-id="f0254-106">V době psaní tohoto textu hello Event Hubs ukázky umístěny v několika různých místech:</span><span class="sxs-lookup"><span data-stu-id="f0254-106">At hello time of this writing, Event Hubs samples are located in several different places:</span></span>

- [<span data-ttu-id="f0254-107">Ukázky kódu vývojáře MSDN</span><span class="sxs-lookup"><span data-stu-id="f0254-107">MSDN developer code samples</span></span>](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [<span data-ttu-id="f0254-108">GitHub</span><span class="sxs-lookup"><span data-stu-id="f0254-108">GitHub</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)

<span data-ttu-id="f0254-109">Další informace o různých verzích hello rozhraní .NET Framework najdete v tématu [architektury a cíle](/dotnet/articles/standard/frameworks).</span><span class="sxs-lookup"><span data-stu-id="f0254-109">For more information about different versions of hello .NET Framework, see [Frameworks and Targets](/dotnet/articles/standard/frameworks).</span></span>

<span data-ttu-id="f0254-110">Další ukázky bude přidán v čase, tak zkontrolujte, vraťte se sem často aktualizací.</span><span class="sxs-lookup"><span data-stu-id="f0254-110">More samples will be added over time, so check back here frequently for updates.</span></span>

## <a name="net-standard"></a><span data-ttu-id="f0254-111">Standardní rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="f0254-111">.NET Standard</span></span>

<span data-ttu-id="f0254-112">předvedení technologie Hello následující ukázky jak toosend a příjmu událostí pomocí hello [klienta služby Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) pro hello [.NET standardní knihovna](/dotnet/articles/standard/library).</span><span class="sxs-lookup"><span data-stu-id="f0254-112">hello following samples demonstrate how toosend and receive events using hello [Event Hubs client](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) for hello [.NET Standard library](/dotnet/articles/standard/library).</span></span>

### <a name="send-events"></a><span data-ttu-id="f0254-113">Odesílání událostí</span><span class="sxs-lookup"><span data-stu-id="f0254-113">Send events</span></span> 

<span data-ttu-id="f0254-114">Hello [Začínáme odesílání](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) příklad ukazuje, jak toowrite .NET Core Konzolová aplikace, který odesílá centra událostí tooan události.</span><span class="sxs-lookup"><span data-stu-id="f0254-114">hello [Get started sending](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) sample shows how toowrite a .NET Core console application that sends events tooan event hub.</span></span>

### <a name="receive-events"></a><span data-ttu-id="f0254-115">Příjem událostí</span><span class="sxs-lookup"><span data-stu-id="f0254-115">Receive events</span></span> 

<span data-ttu-id="f0254-116">Hello [začít pracovat s hello Event Processor Host přijetí](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) ukázka je aplikace konzoly .NET Core, která přijímá zprávy z centra událostí pomocí hello Event Processor Host.</span><span class="sxs-lookup"><span data-stu-id="f0254-116">hello [Get started receiving with hello Event Processor Host](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) sample is a .NET Core console application that receives messages from an event hub using hello Event Processor Host.</span></span>

## <a name="net-framework"></a><span data-ttu-id="f0254-117">Rozhraní .NET framework</span><span class="sxs-lookup"><span data-stu-id="f0254-117">.NET Framework</span></span>   

<span data-ttu-id="f0254-118">Tyto ukázky demonstrují různým funkcím služby Azure Event Hubs, cílení hello [rozhraní .NET Framework – knihovna](/dotnet/framework/index).</span><span class="sxs-lookup"><span data-stu-id="f0254-118">These samples demonstrate various other features of Azure Event Hubs, targeting hello [.NET Framework library](/dotnet/framework/index).</span></span>
 
### <a name="notify-users-of-events-received"></a><span data-ttu-id="f0254-119">Upozorněte uživatele událostí přijatých</span><span class="sxs-lookup"><span data-stu-id="f0254-119">Notify users of events received</span></span>

<span data-ttu-id="f0254-120">Hello [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) ukázka upozorní uživatele z dat přijatých ze senzorů nebo jinými systémy.</span><span class="sxs-lookup"><span data-stu-id="f0254-120">hello [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) sample notifies users of data received from sensors or other systems.</span></span>

### <a name="get-started-with-event-hubs"></a><span data-ttu-id="f0254-121">Začínáme se službou Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f0254-121">Get started with Event Hubs</span></span> 

<span data-ttu-id="f0254-122">Hello [Event Hubs Začínáme](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) příklad znázorňuje hello základní možnosti služby Event Hubs, například jak odesílat události centra událostí tooan toocreate centra událostí a využívat událostí pomocí hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span><span class="sxs-lookup"><span data-stu-id="f0254-122">hello [Event Hubs Getting Started](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) sample demonstrates hello basic capabilities of Event Hubs, such as how toocreate an event hub, send events tooan event hub, and consume events using hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>

### <a name="scale-out-event-processing"></a><span data-ttu-id="f0254-123">Horizontální navýšení kapacity zpracování událostí</span><span class="sxs-lookup"><span data-stu-id="f0254-123">Scale out event processing</span></span> 

<span data-ttu-id="f0254-124">Hello [horizontální navýšení kapacity zpracování událostí](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) příklad znázorňuje způsob toouse hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello zatížení spotřeby Event Hubs datového proudu.</span><span class="sxs-lookup"><span data-stu-id="f0254-124">hello [Scale out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) sample demonstrates how toouse hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello workload of Event Hubs stream consumption.</span></span> <span data-ttu-id="f0254-125">Ukazuje, jak tooimplement hello **EventProcessor** a **EventProcessorFactory** datového proudu událostí objekty toomanage hello.</span><span class="sxs-lookup"><span data-stu-id="f0254-125">It shows how tooimplement hello **EventProcessor** and **EventProcessorFactory** objects toomanage hello event stream.</span></span> 

###  <a name="pull-data-from-sql-into-an-event-hub"></a><span data-ttu-id="f0254-126">Získání dat z SQL do centra událostí</span><span class="sxs-lookup"><span data-stu-id="f0254-126">Pull data from SQL into an event hub</span></span>

<span data-ttu-id="f0254-127">Hello [dat stahování SQL](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) příklad ukazuje, jak toopull data z SQL tabulky a poslat ho tooan centra událostí, toouse jako vstup v příjem dat analytických aplikací.</span><span class="sxs-lookup"><span data-stu-id="f0254-127">hello [Pulling SQL data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) sample shows how toopull data from a SQL table and push it tooan event hub, toouse as an input in downstream analytical applications.</span></span>

### <a name="pull-web-data-into-an-event-hub"></a><span data-ttu-id="f0254-128">Získání dat webové do centra událostí</span><span class="sxs-lookup"><span data-stu-id="f0254-128">Pull web data into an event hub</span></span> 

<span data-ttu-id="f0254-129">Hello [umožňuje importovat data z webové hello](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) příklad ukazuje, jak toopull data z veřejné kanály (například hello informace o datových přenosech ministerstvo dopravy na informační kanál) a poslat ho tooan centra událostí.</span><span class="sxs-lookup"><span data-stu-id="f0254-129">hello [Import data from hello web](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) sample shows how toopull data from public feeds (such as hello Department of Transportation's traffic information feed) and push it tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0254-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0254-130">Next steps</span></span>

<span data-ttu-id="f0254-131">Další informace o verzích rozhraní .NET Framework návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="f0254-131">Learn more about .NET Framework versions by visiting hello following links:</span></span>

- [<span data-ttu-id="f0254-132">Rozhraní a cíle</span><span class="sxs-lookup"><span data-stu-id="f0254-132">Frameworks and targets</span></span>](/dotnet/articles/standard/frameworks)
- [<span data-ttu-id="f0254-133">Rozhraní .NET framework 4.6 a 4.5</span><span class="sxs-lookup"><span data-stu-id="f0254-133">.NET Framework 4.6 and 4.5</span></span>](/dotnet/framework/index)

<span data-ttu-id="f0254-134">Další informace o službě Event Hubs v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="f0254-134">You can learn more about Event Hubs in hello following articles:</span></span>

- [<span data-ttu-id="f0254-135">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f0254-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="f0254-136">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="f0254-136">Create an event hub</span></span>](event-hubs-create.md)
- [<span data-ttu-id="f0254-137">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f0254-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)