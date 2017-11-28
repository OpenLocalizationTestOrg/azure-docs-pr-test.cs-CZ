---
title: "Co je Azure Event Hubs a proč tuto službu používat | Microsoft Docs"
description: "Přehled a úvod do služby Azure Event Hubs – ingestování telemetrických údajů z webů, aplikací a zařízení v cloudovém měřítku"
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
ms.openlocfilehash: 1a6bf0a0352e6d9e3a22586ac825558d12e1307a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-event-hubs"></a><span data-ttu-id="39af4-103">Co je služba Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="39af4-103">What is Event Hubs?</span></span>

<span data-ttu-id="39af4-104">Azure Event Hubs je vysoce škálovatelná platforma pro streamování dat a služba pro ingestování událostí, která je schopná přijmout a zpracovat miliony událostí za sekundu.</span><span class="sxs-lookup"><span data-stu-id="39af4-104">Azure Event Hubs is a highly scalable data streaming platform and event ingestion service capable of receiving and processing millions of events per second.</span></span> <span data-ttu-id="39af4-105">Služba Event Hubs dokáže zpracovávat a ukládat události, data nebo telemetrické údaje produkované distribuovaným softwarem a zařízeními.</span><span class="sxs-lookup"><span data-stu-id="39af4-105">Event Hubs can process and store events, data, or telemetry produced by distributed software and devices.</span></span> <span data-ttu-id="39af4-106">Data odeslaná do centra událostí je možné transformovat a uložit pomocí libovolného poskytovatele analýz v reálném čase nebo adaptérů pro dávkové zpracování a ukládání.</span><span class="sxs-lookup"><span data-stu-id="39af4-106">Data sent to an event hub can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="39af4-107">Díky [možnosti publikování a odebírání dat](https://msdn.microsoft.com/library/aa560414.aspx) s nízkou latencí a v masivním měřítku slouží služba Event Hubs jako vstupní brána k velkým objemům dat.</span><span class="sxs-lookup"><span data-stu-id="39af4-107">With the ability to provide [publish-subscribe capabilities](https://msdn.microsoft.com/library/aa560414.aspx) with low latency and at massive scale, Event Hubs serves as the "on ramp" for Big Data.</span></span>

## <a name="why-use-event-hubs"></a><span data-ttu-id="39af4-108">Proč používat Event Hubs</span><span class="sxs-lookup"><span data-stu-id="39af4-108">Why use Event Hubs?</span></span>

<span data-ttu-id="39af4-109">Možnosti zpracování telemetrických údajů a událostí, které služba Event Hubs nabízí, jsou zvláště užitečné pro:</span><span class="sxs-lookup"><span data-stu-id="39af4-109">Event Hubs event and telemetry handling capabilities make it especially useful for:</span></span>

* <span data-ttu-id="39af4-110">instrumentaci aplikací</span><span class="sxs-lookup"><span data-stu-id="39af4-110">Application instrumentation</span></span>
* <span data-ttu-id="39af4-111">zpracování činnosti nebo pracovního postupu koncového uživatele</span><span class="sxs-lookup"><span data-stu-id="39af4-111">User experience or workflow processing</span></span>
* <span data-ttu-id="39af4-112">scénáře související s internetem věcí (IoT)</span><span class="sxs-lookup"><span data-stu-id="39af4-112">Internet of Things (IoT) scenarios</span></span>

<span data-ttu-id="39af4-113">Služba Event Hubs například umožňuje sledování chování v mobilních aplikacích, informace o datových přenosech z webových farem, zachycení událostí v hrách na konzolách nebo shromažďování telemetrických údajů z průmyslových strojů, připojených vozidel či jiných zařízení.</span><span class="sxs-lookup"><span data-stu-id="39af4-113">For example, Event Hubs enables behavior tracking in mobile apps, traffic information from web farms, in-game event capture in console games, or telemetry collected from industrial machines, connected vehicles, or other devices.</span></span>

## <a name="azure-event-hubs-overview"></a><span data-ttu-id="39af4-114">Přehled služby Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="39af4-114">Azure Event Hubs overview</span></span>

<span data-ttu-id="39af4-115">V architekturách řešení hraje služba Event Hubs běžně roli „předních dveří“ pro kanál událostí, který se často nazývá *přijímač událostí*.</span><span class="sxs-lookup"><span data-stu-id="39af4-115">The common role that Event Hubs plays in solution architectures is the "front door" for an event pipeline, often called an *event ingestor*.</span></span> <span data-ttu-id="39af4-116">Přijímač událostí je komponenta nebo služba, která se nachází mezi zdroji událostí a příjemci událostí a slouží k oddělení produkce datového proudu událostí od spotřeby těchto událostí.</span><span class="sxs-lookup"><span data-stu-id="39af4-116">An event ingestor is a component or service that sits between event publishers and event consumers to decouple the production of an event stream from the consumption of those events.</span></span> <span data-ttu-id="39af4-117">Následující obrázek znázorňuje tuto architekturu:</span><span class="sxs-lookup"><span data-stu-id="39af4-117">The following figure depicts this architecture:</span></span>

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

<span data-ttu-id="39af4-119">Služba Event Hubs poskytuje možnost zpracovávat datové proudy zpráv. Její charakteristiky ji ale odlišují od tradičních podnikových způsobů zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="39af4-119">Event Hubs provides message stream handling capability but has characteristics that are different from traditional enterprise messaging.</span></span> <span data-ttu-id="39af4-120">Možnosti služby Event Hubs jsou vycházejí ze scénářů zpracování událostí a vysoké propustnosti.</span><span class="sxs-lookup"><span data-stu-id="39af4-120">Event Hubs capabilities are built around high throughput and event processing scenarios.</span></span> <span data-ttu-id="39af4-121">V důsledku toho se služba Event Hubs liší od zasílání zpráv služby [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) a neimplementuje některé možnosti dostupné pro entity [zasílání zpráv Service Bus](/azure/service-bus-messaging/), jako jsou třeba témata.</span><span class="sxs-lookup"><span data-stu-id="39af4-121">As such, Event Hubs is different from [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) messaging, and does not implement some of the capabilities that are available for [Service Bus messaging](/azure/service-bus-messaging/) entities, such as topics.</span></span>

## <a name="event-hubs-features"></a><span data-ttu-id="39af4-122">Funkce Event Hubs</span><span class="sxs-lookup"><span data-stu-id="39af4-122">Event Hubs features</span></span>

<span data-ttu-id="39af4-123">Event Hubs obsahuje následující klíčové prvky:</span><span class="sxs-lookup"><span data-stu-id="39af4-123">Event Hubs contains the following key elements:</span></span>

- <span data-ttu-id="39af4-124">[**Producenti/zdroje událostí:**](event-hubs-features.md#event-publishers) Entita, která odesílá data do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="39af4-124">[**Event producers/publishers**](event-hubs-features.md#event-publishers): An entity that sends data to an event hub.</span></span> <span data-ttu-id="39af4-125">Událost se publikuje prostřednictvím AMQP 1.0 nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="39af4-125">An event is published via AMQP 1.0 or HTTPS.</span></span>
- <span data-ttu-id="39af4-126">[**Zachytávání:**](event-hubs-features.md#capture) Umožňuje zachytit streamovaná data Event Hubs a uložit je v účtu Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="39af4-126">[**Capture**](event-hubs-features.md#capture): Enables you to capture Event Hubs streaming data and store it in an Azure Blob storage account.</span></span>
- <span data-ttu-id="39af4-127">[**Oddíly:**](event-hubs-features.md#partitions) Umožňuje každému příjemci načíst jenom konkrétní podmnožinu, nebo oddíl datového proudu událostí.</span><span class="sxs-lookup"><span data-stu-id="39af4-127">[**Partitions**](event-hubs-features.md#partitions): Enables each consumer to only read a specific subset, or partition, of the event stream.</span></span>
- <span data-ttu-id="39af4-128">[**Tokeny SAS:**](event-hubs-features.md#sas-tokens) Identifikuje a ověřuje zdroj událostí.</span><span class="sxs-lookup"><span data-stu-id="39af4-128">[**SAS tokens**](event-hubs-features.md#sas-tokens): Identifies and authenticates the event publisher.</span></span>
- <span data-ttu-id="39af4-129">[**Příjemci událostí:**](event-hubs-features.md#event-consumers) Entita, která čte data událostí z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="39af4-129">[**Event consumers**](event-hubs-features.md#event-consumers): An entity that reads event data from an event hub.</span></span> <span data-ttu-id="39af4-130">Příjemci událostí se připojují prostřednictvím protokolu AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="39af4-130">Event consumers connect via AMQP 1.0.</span></span> 
- <span data-ttu-id="39af4-131">[**Skupiny příjemců:**](event-hubs-features.md#consumer-groups) Poskytuje každé aplikaci s několika konzumacemi samostatné zobrazení datového proudu událostí a umožňuje, aby tito příjemci jednali nezávisle.</span><span class="sxs-lookup"><span data-stu-id="39af4-131">[**Consumer groups**](event-hubs-features.md#consumer-groups): Provides each multiple consuming application with a separate view of the event stream, enabling those consumers to act independently.</span></span>
- <span data-ttu-id="39af4-132">[**Jednotky propustnosti:**](event-hubs-features.md#capacity) Předem zakoupené jednotky kapacity.</span><span class="sxs-lookup"><span data-stu-id="39af4-132">[**Throughput units**](event-hubs-features.md#capacity): Pre-purchased units of capacity.</span></span> <span data-ttu-id="39af4-133">Škálování jednoho oddílu dovoluje maximálně jednu jednotku propustnosti.</span><span class="sxs-lookup"><span data-stu-id="39af4-133">A single partition has a maximum scale of 1 throughput unit.</span></span>

<span data-ttu-id="39af4-134">Technické podrobnosti o těchto a dalších funkcích služby Event Hubs najdete v [přehledu funkcí Event Hubs](event-hubs-features.md).</span><span class="sxs-lookup"><span data-stu-id="39af4-134">For technical details about these and other Event Hubs features, see the [Event Hubs features overview](event-hubs-features.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="39af4-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39af4-135">Next steps</span></span>

<span data-ttu-id="39af4-136">Podrobné informace o cenách služby Event Hubs naleznete v článku o [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="39af4-136">For detailed Event Hubs pricing information, see [Event Hubs Pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span>

<span data-ttu-id="39af4-137">Další informace o službě Event Hubs naleznete pod těmito odkazy:</span><span class="sxs-lookup"><span data-stu-id="39af4-137">For more information about Event Hubs, visit the following links:</span></span>

* <span data-ttu-id="39af4-138">Úvodní [Kurz služby Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)</span><span class="sxs-lookup"><span data-stu-id="39af4-138">Get started with an [Event Hubs tutorial](event-hubs-dotnet-standard-getstarted-send.md)</span></span>
* [<span data-ttu-id="39af4-139">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="39af4-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)
* [<span data-ttu-id="39af4-140">Ukázkové aplikace, které používají službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="39af4-140">Sample applications that use Event Hubs</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

