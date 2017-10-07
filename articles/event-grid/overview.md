---
title: "Přehled událostí mřížky aaaAzure"
description: "Popisuje mřížky událostí Azure a jeho koncepty."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 95dce22e9335df88e81b134143a6c14994c26b8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-event-grid"></a><span data-ttu-id="6b92d-103">Úvod tooAzure událostí mřížky</span><span class="sxs-lookup"><span data-stu-id="6b92d-103">An introduction tooAzure Event Grid</span></span>

<span data-ttu-id="6b92d-104">Azure mřížky událostí vám umožní tooeasily sestavení aplikace pomocí architektury na základě událostí.</span><span class="sxs-lookup"><span data-stu-id="6b92d-104">Azure Event Grid allows you tooeasily build applications with event-based architectures.</span></span> <span data-ttu-id="6b92d-105">Můžete vybrat hello prostředků Azure chcete vytvořit toosubscribe k a poskytnout hello obslužná rutina události nebo Webhooku koncový bod toosend hello události.</span><span class="sxs-lookup"><span data-stu-id="6b92d-105">You select hello Azure resource you would like toosubscribe to, and give hello event handler or WebHook endpoint toosend hello event to.</span></span> <span data-ttu-id="6b92d-106">Událost mřížky má integrovanou podporu pro události pocházející ze služby Azure, jako jsou skupiny prostředků a úložiště objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="6b92d-106">Event Grid has built-in support for events coming from Azure services, like storage blobs and resource groups.</span></span> <span data-ttu-id="6b92d-107">Událost mřížky má také vlastní podporu pro aplikace a událostí třetích stran, pomocí vlastní témata a vlastní webhooky.</span><span class="sxs-lookup"><span data-stu-id="6b92d-107">Event Grid also has custom support for application and third-party events, using custom topics and custom webhooks.</span></span> 

<span data-ttu-id="6b92d-108">Můžete použít koncových bodů toodifferent určité události tooroute filtry, vícesměrového vysílání toomultiple koncových bodů a ujistěte se, že události jsou spolehlivě doručovány.</span><span class="sxs-lookup"><span data-stu-id="6b92d-108">You can use filters tooroute specific events toodifferent endpoints, multicast toomultiple endpoints, and make sure your events are reliably delivered.</span></span> <span data-ttu-id="6b92d-109">Událost mřížky také obsahuje vestavěnou podporou pro třetí strany a vlastní události.</span><span class="sxs-lookup"><span data-stu-id="6b92d-109">Event Grid also has built in support for custom and third-party events.</span></span>

<span data-ttu-id="6b92d-110">Pro verzi preview hello událostí mřížky podporuje **westus2** a **westcentralus** umístění.</span><span class="sxs-lookup"><span data-stu-id="6b92d-110">For hello preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span> <span data-ttu-id="6b92d-111">Přidá jiných oblastí.</span><span class="sxs-lookup"><span data-stu-id="6b92d-111">Other regions will be added.</span></span>

<span data-ttu-id="6b92d-112">Tento článek obsahuje přehled Azure událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="6b92d-112">This article provides an overview of Azure Event Grid.</span></span> <span data-ttu-id="6b92d-113">Pokud chcete začít s událostí mřížky tooget, najdete v části [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="6b92d-113">If you want tooget started with Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>

![Funkční model událostí mřížky](./media/overview/event-grid-functional-model.png)

<span data-ttu-id="6b92d-115">Úložiště objektů Blob v současné době není veřejně dostupné jako vydavatel.</span><span class="sxs-lookup"><span data-stu-id="6b92d-115">Currently, Blob Storage is not publicly available as a publisher.</span></span>

## <a name="concepts"></a><span data-ttu-id="6b92d-116">Koncepty</span><span class="sxs-lookup"><span data-stu-id="6b92d-116">Concepts</span></span>

<span data-ttu-id="6b92d-117">V mřížce událostí Azure, která umožňují zprovoznění existují pět koncepty:</span><span class="sxs-lookup"><span data-stu-id="6b92d-117">There are five concepts in Azure Event Grid that let you get going:</span></span>

* <span data-ttu-id="6b92d-118">**Události** -co se stalo.</span><span class="sxs-lookup"><span data-stu-id="6b92d-118">**Events** - What happened.</span></span>
* <span data-ttu-id="6b92d-119">**Zdroje událostí nebo vydavatelů** – tam, kde došlo k události hello.</span><span class="sxs-lookup"><span data-stu-id="6b92d-119">**Event sources/publishers** - Where hello event took place.</span></span>
* <span data-ttu-id="6b92d-120">**Témata** -hello koncový bod, kde vydavatelů odesílat události.</span><span class="sxs-lookup"><span data-stu-id="6b92d-120">**Topics** - hello endpoint where publishers send events.</span></span>
* <span data-ttu-id="6b92d-121">**Odběry událostí** -hello koncového bodu nebo integrované mechanismus tooroute událostí, někdy toomultiple obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b92d-121">**Event subscriptions** - hello endpoint or built-in mechanism tooroute events, sometimes toomultiple handlers.</span></span> <span data-ttu-id="6b92d-122">Odběry jsou také používány obslužné rutiny toointelligently filtru příchozí události.</span><span class="sxs-lookup"><span data-stu-id="6b92d-122">Subscriptions are also used by handlers toointelligently filter incoming events.</span></span>
* <span data-ttu-id="6b92d-123">**Obslužné rutiny událostí** – hello aplikace nebo služby reagující toohello událostí.</span><span class="sxs-lookup"><span data-stu-id="6b92d-123">**Event handlers** - hello app or service reacting toohello event.</span></span>

<span data-ttu-id="6b92d-124">Další informace o těchto postupech najdete v tématu [koncepty v mřížce událostí Azure](concepts.md).</span><span class="sxs-lookup"><span data-stu-id="6b92d-124">For more information about these concepts, see [Concepts in Azure Event Grid](concepts.md).</span></span>

## <a name="capabilities"></a><span data-ttu-id="6b92d-125">Možnosti</span><span class="sxs-lookup"><span data-stu-id="6b92d-125">Capabilities</span></span>

<span data-ttu-id="6b92d-126">Tady jsou některé klíčové funkce Azure událostí mřížky hello:</span><span class="sxs-lookup"><span data-stu-id="6b92d-126">Here are some of hello key features of Azure Event Grid:</span></span>

* <span data-ttu-id="6b92d-127">**Jednoduchost** -bodu a klikněte na tlačítko tooaim události z obslužné rutiny události tooany prostředků Azure nebo koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6b92d-127">**Simplicity** - Point and click tooaim events from your Azure resource tooany event handler or endpoint.</span></span>
* <span data-ttu-id="6b92d-128">**Rozšířené filtrování** -filtru pro událost typu nebo událostí publikování tooensure cesta obslužné rutiny událostí přijímat pouze relevantní události.</span><span class="sxs-lookup"><span data-stu-id="6b92d-128">**Advanced filtering** - Filter on event type or event publish path tooensure event handlers only receive relevant events.</span></span>
* <span data-ttu-id="6b92d-129">**FAN-out** -přihlášení k odběru několika koncové body toohello stejné události toosend zkopíruje z hello událostí tooas mnoha místech podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="6b92d-129">**Fan-out** - Subscribe multiple endpoints toohello same event toosend copies of hello event tooas many places as needed.</span></span>
* <span data-ttu-id="6b92d-130">**Spolehlivost** -využívat znova 24 hodin s exponenciálního omezení rychlosti tooensure doručí události.</span><span class="sxs-lookup"><span data-stu-id="6b92d-130">**Reliability** - Utilize 24-hour retry with exponential backoff tooensure events are delivered.</span></span>
* <span data-ttu-id="6b92d-131">**Platím za událostí** – platíte jenom pro velikost hello použijete událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="6b92d-131">**Pay-per-event** - Pay only for hello amount you use Event Grid.</span></span>
* <span data-ttu-id="6b92d-132">**Vysoká propustnost** -sestavení vysoký počet úloh v mřížce událostí s podporou miliony událostí za sekundu.</span><span class="sxs-lookup"><span data-stu-id="6b92d-132">**High throughput** - Build high-volume workloads on Event Grid with support for millions of events per second.</span></span>
* <span data-ttu-id="6b92d-133">**Předdefinované události** – zprovoznění a rychlé spuštění s integrovanou událostí definovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b92d-133">**Built-in Events** - Get up and running quickly with resource-defined built-in events.</span></span>
* <span data-ttu-id="6b92d-134">**Vlastní události** -použít událost mřížky trasy, filtr a spolehlivě vlastních událostí doručit ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6b92d-134">**Custom Events** - use Event Grid route, filter, and reliably deliver custom events in your app.</span></span>

## <a name="built-in-publisher-and-handler-integration"></a><span data-ttu-id="6b92d-135">Integraci vydavatele a obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="6b92d-135">Built-in publisher and handler integration</span></span>

<span data-ttu-id="6b92d-136">Azure nabízí podporu předdefinované událostí pomocí množství služeb, včetně vydavatele a obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b92d-136">Azure offers built-in event support using numerous services, including both publishers and handlers.</span></span>

### <a name="publishers"></a><span data-ttu-id="6b92d-137">Vydavatelé</span><span class="sxs-lookup"><span data-stu-id="6b92d-137">Publishers</span></span>

<span data-ttu-id="6b92d-138">V současné době hello následující služby Azure mají podporu předdefinované vydavatele pro mřížky událostí:</span><span class="sxs-lookup"><span data-stu-id="6b92d-138">Currently, hello following Azure services have built-in publisher support for event grid:</span></span>

* <span data-ttu-id="6b92d-139">Skupiny prostředků (operace správy)</span><span class="sxs-lookup"><span data-stu-id="6b92d-139">Resource Groups (management operations)</span></span>
* <span data-ttu-id="6b92d-140">Předplatná Azure (operace správy)</span><span class="sxs-lookup"><span data-stu-id="6b92d-140">Azure Subscriptions (management operations)</span></span>
* <span data-ttu-id="6b92d-141">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="6b92d-141">Event Hubs</span></span>
* <span data-ttu-id="6b92d-142">Vlastní témata</span><span class="sxs-lookup"><span data-stu-id="6b92d-142">Custom Topics</span></span>

<span data-ttu-id="6b92d-143">Jinými službami Azure se přidá tohoto roku.</span><span class="sxs-lookup"><span data-stu-id="6b92d-143">Other Azure services will be added this year.</span></span>

### <a name="handlers"></a><span data-ttu-id="6b92d-144">Obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="6b92d-144">Handlers</span></span>

<span data-ttu-id="6b92d-145">V současné době hello následující služby Azure mají podporu předdefinované obslužné rutiny událostí mřížky:</span><span class="sxs-lookup"><span data-stu-id="6b92d-145">Currently, hello following Azure services have built-in handler support for Event Grid:</span></span> 

* <span data-ttu-id="6b92d-146">Funkce Azure</span><span class="sxs-lookup"><span data-stu-id="6b92d-146">Azure Functions</span></span>
* <span data-ttu-id="6b92d-147">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="6b92d-147">Logic Apps</span></span>
* <span data-ttu-id="6b92d-148">Azure Automation</span><span class="sxs-lookup"><span data-stu-id="6b92d-148">Azure Automation</span></span>
* <span data-ttu-id="6b92d-149">Webhooky</span><span class="sxs-lookup"><span data-stu-id="6b92d-149">WebHooks</span></span>

<span data-ttu-id="6b92d-150">Jinými službami Azure se přidá tohoto roku.</span><span class="sxs-lookup"><span data-stu-id="6b92d-150">Other Azure services will be added this year.</span></span>

## <a name="what-can-i-do-with-event-grid"></a><span data-ttu-id="6b92d-151">Co můžete dělat s událostí mřížky?</span><span class="sxs-lookup"><span data-stu-id="6b92d-151">What can I do with Event Grid?</span></span>

<span data-ttu-id="6b92d-152">Azure mřížky událostí poskytuje několik možností, které významně zlepšit bez serveru, ops automatizace a integraci pracovních:</span><span class="sxs-lookup"><span data-stu-id="6b92d-152">Azure Event Grid provides several capabilities that vastly improve serverless, ops automation, and integration work:</span></span> 

### <a name="serverless-application-architectures"></a><span data-ttu-id="6b92d-153">Architektury aplikací bez serveru</span><span class="sxs-lookup"><span data-stu-id="6b92d-153">Serverless application architectures</span></span>

![Aplikaci bez serveru](./media/overview/serverless_web_app.png)

<span data-ttu-id="6b92d-155">Event Grid propojuje zdroje dat a obslužné rutiny událostí.</span><span class="sxs-lookup"><span data-stu-id="6b92d-155">Event Grid connects data sources and event handlers.</span></span> <span data-ttu-id="6b92d-156">Například použijte aktivační událost mřížky tooinstantly analýza bez serveru funkce toorun obrázků pokaždé, když je nové fotografie přidat tooa kontejner úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="6b92d-156">For example, use Event Grid tooinstantly trigger a serverless function toorun image analysis each time a new photo is added tooa blob storage container.</span></span> 

### <a name="ops-automation"></a><span data-ttu-id="6b92d-157">Automatizace OPS</span><span class="sxs-lookup"><span data-stu-id="6b92d-157">Ops Automation</span></span>

![Automatizace operací](./media/overview/Ops_automation.png)

<span data-ttu-id="6b92d-159">Událost mřížky vám umožní toospeed automatizace a zjednodušit vynucení zásad.</span><span class="sxs-lookup"><span data-stu-id="6b92d-159">Event Grid allows you toospeed automation and simplify policy enforcement.</span></span> <span data-ttu-id="6b92d-160">Event Grid může například upozornit službu Azure Automation při vytvoření virtuálního počítače nebo aktivaci služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="6b92d-160">For example, Event Grid can notify Azure Automation when a virtual machine is created, or a SQL Database is spun up.</span></span> <span data-ttu-id="6b92d-161">Tyto události můžete použít tooautomatically zkontrolujte, jestli konfigurace služby jsou kompatibilní, put metadata do nástroje operations, značka virtuálních počítačů nebo soubor pracovních položek.</span><span class="sxs-lookup"><span data-stu-id="6b92d-161">These events can be used tooautomatically check that service configurations are compliant, put metadata into operations tools, tag virtual machines, or file work items.</span></span>

### <a name="application-integration"></a><span data-ttu-id="6b92d-162">Integrace aplikací</span><span class="sxs-lookup"><span data-stu-id="6b92d-162">Application integration</span></span>

![Integrace aplikací](./media/overview/app_integration.png)

<span data-ttu-id="6b92d-164">Event Grid propojuje vaši aplikaci s dalšími službami.</span><span class="sxs-lookup"><span data-stu-id="6b92d-164">Event Grid connects your app with other services.</span></span> <span data-ttu-id="6b92d-165">Například vytvořit vlastní téma toosend vaší aplikace událostí data tooEvent mřížky a využít její spolehlivé doručení advanced směrování a přímá integrace s Azure.</span><span class="sxs-lookup"><span data-stu-id="6b92d-165">For example, create a custom topic toosend your app's event data tooEvent Grid, and take advantage of its reliable delivery, advanced routing, and direct integration with Azure.</span></span> <span data-ttu-id="6b92d-166">Alternativně můžete použít událost mřížky s Logic Apps tooprocess kdekoliv a dat bez nutnosti psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="6b92d-166">Alternatively, you can use Event Grid with Logic Apps tooprocess data anywhere, without writing code.</span></span> 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a><span data-ttu-id="6b92d-167">Jak se liší od jiných služeb integrace se službou Azure mřížky událostí?</span><span class="sxs-lookup"><span data-stu-id="6b92d-167">How is Event Grid different from other Azure integration services?</span></span>

<span data-ttu-id="6b92d-168">Mřížka událostí je eventing propojovacího rozhraní, která umožňuje událostmi, reaktivní programování.</span><span class="sxs-lookup"><span data-stu-id="6b92d-168">Event Grid is an eventing backplane that enables event-driven, reactive programming.</span></span> <span data-ttu-id="6b92d-169">Ho se úzce integruje se službami Azure a může být integrovaná v služeb třetích stran.</span><span class="sxs-lookup"><span data-stu-id="6b92d-169">It is deeply integrated with Azure services and can be integrated with third-party services.</span></span> <span data-ttu-id="6b92d-170">Hello zpráva události obsahuje hello informace, které potřebujete tooreact toochanges služeb a aplikací.</span><span class="sxs-lookup"><span data-stu-id="6b92d-170">hello event message contains hello information you need tooreact toochanges in services and applications.</span></span> <span data-ttu-id="6b92d-171">Mřížky událostí není datovém kanálu a nejsou poskytovány hello skutečný objekt, který byl aktualizován.</span><span class="sxs-lookup"><span data-stu-id="6b92d-171">Event Grid is not a data pipeline, and does not deliver hello actual object that was updated.</span></span>

<span data-ttu-id="6b92d-172">Service Bus je skvěle hodí pro tradiční podnikové aplikace, které vyžadují transakcí, řazení, detekci duplikátů a okamžitou konzistence.</span><span class="sxs-lookup"><span data-stu-id="6b92d-172">Service Bus is well suited for traditional enterprise applications that require transactions, ordering, duplicate detection, and instantaneous consistency.</span></span> <span data-ttu-id="6b92d-173">Mřížka událostí je určených rychlost, škálování, spektra a nízké náklady v reaktivní modelu.</span><span class="sxs-lookup"><span data-stu-id="6b92d-173">Event Grid is designed for speed, scale, breadth, and low cost in a reactive model.</span></span> <span data-ttu-id="6b92d-174">Je architektura skvěle hodí tooserverless.</span><span class="sxs-lookup"><span data-stu-id="6b92d-174">It is well suited tooserverless architecture.</span></span>

<span data-ttu-id="6b92d-175">Událost mřížky doplňuje jinými službami Azure jako Logic Apps a Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="6b92d-175">Event Grid complements other Azure services like Logic Apps and Event Hubs.</span></span> <span data-ttu-id="6b92d-176">Aktivační události mřížky hello logiku aplikace toobegin jejího pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="6b92d-176">Event Grid triggers hello logic app toobegin its workflow.</span></span> <span data-ttu-id="6b92d-177">Služba Event Hubs pracuje s událostí mřížky tím, že vám tooreact tooevents z zaznamenat centra událostí a kanálů pro příchozí a transformaci dat sestavení.</span><span class="sxs-lookup"><span data-stu-id="6b92d-177">Event Hubs works with Event Grid by enabling you tooreact tooevents from Event Hubs Capture, and build data ingress and transformation pipelines.</span></span>

## <a name="how-much-does-event-grid-cost"></a><span data-ttu-id="6b92d-178">Jaké události mřížky náklady?</span><span class="sxs-lookup"><span data-stu-id="6b92d-178">How much does Event Grid cost?</span></span>

<span data-ttu-id="6b92d-179">Azure mřížky událostí používá model tvorby cen platím za událostí, takže platíte jenom se používá.</span><span class="sxs-lookup"><span data-stu-id="6b92d-179">Azure Event Grid uses a pay-per-event pricing model, so you only pay for what you use.</span></span>

<span data-ttu-id="6b92d-180">Událost mřížky stojí 0,60 za mil. operace ($0,30 verzi Preview) a jsou hello nejprve 100 000 operaci za měsíc zdarma.</span><span class="sxs-lookup"><span data-stu-id="6b92d-180">Event Grid costs $0.60 per million operations ($0.30 during preview) and hello first 100,000 operation per month are free.</span></span> <span data-ttu-id="6b92d-181">Operace jsou definovány jako příjem příchozích dat událostí, rozšířená shoda, pokus o doručení a správu volání.</span><span class="sxs-lookup"><span data-stu-id="6b92d-181">Operations are defined as event ingress, advanced match, delivery attempt, and management calls.</span></span>  <span data-ttu-id="6b92d-182">Další podrobnosti naleznete na hello [stránce s cenami](https://azure.microsoft.com/pricing/details/event-grid/).</span><span class="sxs-lookup"><span data-stu-id="6b92d-182">More details can be found on hello [pricing page](https://azure.microsoft.com/pricing/details/event-grid/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b92d-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b92d-183">Next steps</span></span>

* <span data-ttu-id="6b92d-184">[Vytvoření a přihlášení k odběru události toocustom](custom-event-quickstart.md) rovnou a začít posílat svůj vlastní koncový bod tooany vlastních událostí pomocí rychlého startu Azure událostí mřížky hello.</span><span class="sxs-lookup"><span data-stu-id="6b92d-184">[Create and subscribe toocustom events](custom-event-quickstart.md) Jump right in and start sending your own custom events tooany endpoint using hello Azure Event Grid quickstart.</span></span>
* <span data-ttu-id="6b92d-185">[Použití aplikace logiky jako obslužné rutiny události](monitor-virtual-machine-changes-event-grid-logic-app.md) kurz týkající se vytváření aplikace pomocí Logic Apps tooreact tooevents nabídnutých podle událostí mřížky.</span><span class="sxs-lookup"><span data-stu-id="6b92d-185">[Using Logic Apps as an Event Handler](monitor-virtual-machine-changes-event-grid-logic-app.md) A tutorial on building an app using Logic Apps tooreact tooevents pushed by Event Grid.</span></span>
* [<span data-ttu-id="6b92d-186">Odkazu k REST API mřížky událostí</span><span class="sxs-lookup"><span data-stu-id="6b92d-186">Event Grid REST API reference</span></span>](/rest/api/eventgrid)  
  <span data-ttu-id="6b92d-187">Poskytuje další odborné informace o hello mřížky událostí Azure a odkaz pro správě přihlášení k odběru událostí, směrování a filtrování.</span><span class="sxs-lookup"><span data-stu-id="6b92d-187">Provides more technical information about hello Azure Event Grid, and a reference for managing Event Subscriptions, routing, and filtering.</span></span>
