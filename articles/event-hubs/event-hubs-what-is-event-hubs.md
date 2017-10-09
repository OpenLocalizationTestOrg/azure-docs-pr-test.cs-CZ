---
title: "aaaWhat je Azure Event Hubs a proč ji používat | Microsoft Docs"
description: "Přehled a úvod tooAzure Event Hubs - cloudového škálovatelného přijímání telemetrie z webů, aplikací a zařízení"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm; babanisa
ms.openlocfilehash: f6199a2e5bee8506f529b6f561234d79f9c8d465
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-event-hubs"></a><span data-ttu-id="912b1-103">Co je služba Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="912b1-103">What is Event Hubs?</span></span>

<span data-ttu-id="912b1-104">Azure Event Hubs je vysoce škálovatelná platforma pro streamování dat a služba pro ingestování událostí, která je schopná přijmout a zpracovat miliony událostí za sekundu.</span><span class="sxs-lookup"><span data-stu-id="912b1-104">Azure Event Hubs is a highly scalable data streaming platform and event ingestion service capable of receiving and processing millions of events per second.</span></span> <span data-ttu-id="912b1-105">Služba Event Hubs dokáže zpracovávat a ukládat události, data nebo telemetrické údaje produkované distribuovaným softwarem a zařízeními.</span><span class="sxs-lookup"><span data-stu-id="912b1-105">Event Hubs can process and store events, data, or telemetry produced by distributed software and devices.</span></span> <span data-ttu-id="912b1-106">Data se odešlou tooan centra událostí, lze je transformovat a uložit pomocí jakékoli zprostředkovatele datové analýzy v reálném čase nebo adaptérů pro dávkování či ukládání.</span><span class="sxs-lookup"><span data-stu-id="912b1-106">Data sent tooan event hub can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="912b1-107">S hello možnost tooprovide [publikování a odebírání](https://msdn.microsoft.com/library/aa560414.aspx) s nízkou latencí a v masivním měřítku, Event Hubs slouží jako hello "na doběhu" pro velká Data.</span><span class="sxs-lookup"><span data-stu-id="912b1-107">With hello ability tooprovide [publish-subscribe capabilities](https://msdn.microsoft.com/library/aa560414.aspx) with low latency and at massive scale, Event Hubs serves as hello "on ramp" for Big Data.</span></span>

## <a name="why-use-event-hubs"></a><span data-ttu-id="912b1-108">Proč používat Event Hubs</span><span class="sxs-lookup"><span data-stu-id="912b1-108">Why use Event Hubs?</span></span>

<span data-ttu-id="912b1-109">Možnosti zpracování telemetrických údajů a událostí, které služba Event Hubs nabízí, jsou zvláště užitečné pro:</span><span class="sxs-lookup"><span data-stu-id="912b1-109">Event Hubs event and telemetry handling capabilities make it especially useful for:</span></span>

* <span data-ttu-id="912b1-110">instrumentaci aplikací</span><span class="sxs-lookup"><span data-stu-id="912b1-110">Application instrumentation</span></span>
* <span data-ttu-id="912b1-111">zpracování činnosti nebo pracovního postupu koncového uživatele</span><span class="sxs-lookup"><span data-stu-id="912b1-111">User experience or workflow processing</span></span>
* <span data-ttu-id="912b1-112">scénáře související s internetem věcí (IoT)</span><span class="sxs-lookup"><span data-stu-id="912b1-112">Internet of Things (IoT) scenarios</span></span>

<span data-ttu-id="912b1-113">Služba Event Hubs například umožňuje sledování chování v mobilních aplikacích, informace o datových přenosech z webových farem, zachycení událostí v hrách na konzolách nebo shromažďování telemetrických údajů z průmyslových strojů, připojených vozidel či jiných zařízení.</span><span class="sxs-lookup"><span data-stu-id="912b1-113">For example, Event Hubs enables behavior tracking in mobile apps, traffic information from web farms, in-game event capture in console games, or telemetry collected from industrial machines, connected vehicles, or other devices.</span></span>

## <a name="azure-event-hubs-overview"></a><span data-ttu-id="912b1-114">Přehled služby Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="912b1-114">Azure Event Hubs overview</span></span>

<span data-ttu-id="912b1-115">Vítejte v architekturách řešení hraje Služba Event Hubs běžně roli je hello "přední dveře" pro kanál událostí, často říká *přijímač událostí*.</span><span class="sxs-lookup"><span data-stu-id="912b1-115">hello common role that Event Hubs plays in solution architectures is hello "front door" for an event pipeline, often called an *event ingestor*.</span></span> <span data-ttu-id="912b1-116">Přijímač událostí je komponenta nebo služba, která se nachází mezi zdroji událostí a příjemci událostí toodecouple hello produkce datového proudu událostí od spotřeby těchto události hello.</span><span class="sxs-lookup"><span data-stu-id="912b1-116">An event ingestor is a component or service that sits between event publishers and event consumers toodecouple hello production of an event stream from hello consumption of those events.</span></span> <span data-ttu-id="912b1-117">Hello následující obrázek znázorňuje tuto architekturu:</span><span class="sxs-lookup"><span data-stu-id="912b1-117">hello following figure depicts this architecture:</span></span>

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

<span data-ttu-id="912b1-119">Služba Event Hubs poskytuje možnost zpracovávat datové proudy zpráv. Její charakteristiky ji ale odlišují od tradičních podnikových způsobů zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="912b1-119">Event Hubs provides message stream handling capability but has characteristics that are different from traditional enterprise messaging.</span></span> <span data-ttu-id="912b1-120">Možnosti služby Event Hubs jsou vycházejí ze scénářů zpracování událostí a vysoké propustnosti.</span><span class="sxs-lookup"><span data-stu-id="912b1-120">Event Hubs capabilities are built around high throughput and event processing scenarios.</span></span> <span data-ttu-id="912b1-121">Takto se liší od služby Event Hubs [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) zasílání zpráv a neimplementuje některé možnosti hello, které jsou k dispozici pro [zasílání zpráv Service Bus](/azure/service-bus-messaging/) entitami, jako je například témata.</span><span class="sxs-lookup"><span data-stu-id="912b1-121">As such, Event Hubs is different from [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) messaging, and does not implement some of hello capabilities that are available for [Service Bus messaging](/azure/service-bus-messaging/) entities, such as topics.</span></span>

## <a name="event-hubs-features"></a><span data-ttu-id="912b1-122">Funkce Event Hubs</span><span class="sxs-lookup"><span data-stu-id="912b1-122">Event Hubs features</span></span>

<span data-ttu-id="912b1-123">Centra událostí obsahuje hello následující klíčové prvky:</span><span class="sxs-lookup"><span data-stu-id="912b1-123">Event Hubs contains hello following key elements:</span></span>

- <span data-ttu-id="912b1-124">[**Producenti/zdroje událostí**](event-hubs-features.md#event-publishers): Entita, která odesílá data tooan události rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="912b1-124">[**Event producers/publishers**](event-hubs-features.md#event-publishers): An entity that sends data tooan event hub.</span></span> <span data-ttu-id="912b1-125">Událost se publikuje prostřednictvím AMQP 1.0 nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="912b1-125">An event is published via AMQP 1.0 or HTTPS.</span></span>
- <span data-ttu-id="912b1-126">[**Zaznamenat**](event-hubs-features.md#capture): umožňuje toocapture Event Hubs, streamování dat a uloží jej v účtu úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="912b1-126">[**Capture**](event-hubs-features.md#capture): Enables you toocapture Event Hubs streaming data and store it in an Azure Blob storage account.</span></span>
- <span data-ttu-id="912b1-127">[**Oddíly**](event-hubs-features.md#partitions): hello umožňuje každý příjemce tooonly číst z konkrétní podmnožinu nebo oddíl datového proudu událostí.</span><span class="sxs-lookup"><span data-stu-id="912b1-127">[**Partitions**](event-hubs-features.md#partitions): Enables each consumer tooonly read a specific subset, or partition, of hello event stream.</span></span>
- <span data-ttu-id="912b1-128">[**Tokeny SAS**](event-hubs-features.md#sas-tokens): identifikuje a ověřuje vydavatel události hello.</span><span class="sxs-lookup"><span data-stu-id="912b1-128">[**SAS tokens**](event-hubs-features.md#sas-tokens): Identifies and authenticates hello event publisher.</span></span>
- <span data-ttu-id="912b1-129">[**Příjemci událostí:**](event-hubs-features.md#event-consumers) Entita, která čte data událostí z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="912b1-129">[**Event consumers**](event-hubs-features.md#event-consumers): An entity that reads event data from an event hub.</span></span> <span data-ttu-id="912b1-130">Příjemci událostí se připojují prostřednictvím protokolu AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="912b1-130">Event consumers connect via AMQP 1.0.</span></span> 
- <span data-ttu-id="912b1-131">[**Skupiny příjemců**](event-hubs-features.md#consumer-groups): poskytuje každý více využívání aplikace s oddělená zobrazení datového proudu událostí hello, povolení těchto příjemci tooact nezávisle.</span><span class="sxs-lookup"><span data-stu-id="912b1-131">[**Consumer groups**](event-hubs-features.md#consumer-groups): Provides each multiple consuming application with a separate view of hello event stream, enabling those consumers tooact independently.</span></span>
- <span data-ttu-id="912b1-132">[**Jednotky propustnosti:**](event-hubs-features.md#capacity) Předem zakoupené jednotky kapacity.</span><span class="sxs-lookup"><span data-stu-id="912b1-132">[**Throughput units**](event-hubs-features.md#capacity): Pre-purchased units of capacity.</span></span> <span data-ttu-id="912b1-133">Škálování jednoho oddílu dovoluje maximálně jednu jednotku propustnosti.</span><span class="sxs-lookup"><span data-stu-id="912b1-133">A single partition has a maximum scale of 1 throughput unit.</span></span>

<span data-ttu-id="912b1-134">Technické podrobnosti o těchto a dalších funkcí služby Event Hubs najdete v tématu hello [přehled funkcí služby Event Hubs](event-hubs-features.md).</span><span class="sxs-lookup"><span data-stu-id="912b1-134">For technical details about these and other Event Hubs features, see hello [Event Hubs features overview](event-hubs-features.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="912b1-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="912b1-135">Next steps</span></span>

<span data-ttu-id="912b1-136">Podrobné informace o cenách služby Event Hubs naleznete v článku o [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="912b1-136">For detailed Event Hubs pricing information, see [Event Hubs Pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span>

<span data-ttu-id="912b1-137">Další informace o službě Event Hubs najdete na adrese hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="912b1-137">For more information about Event Hubs, visit hello following links:</span></span>

* <span data-ttu-id="912b1-138">Úvodní [Kurz služby Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)</span><span class="sxs-lookup"><span data-stu-id="912b1-138">Get started with an [Event Hubs tutorial](event-hubs-dotnet-standard-getstarted-send.md)</span></span>
* [<span data-ttu-id="912b1-139">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="912b1-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)
* [<span data-ttu-id="912b1-140">Ukázkové aplikace, které používají službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="912b1-140">Sample applications that use Event Hubs</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

