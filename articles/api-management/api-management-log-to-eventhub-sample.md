---
title: "Sledování rozhraní API pomocí Azure API Management, Event Hubs a Runscope | Microsoft Docs"
description: "Ukázkovou aplikaci ukázka zásad protokolu eventhub připojování Azure API Management, Azure Event Hubs a Runscope pro protokol HTTP, protokolování a monitorování"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 70ee752c5639c90f77dde104ce85eec0a1062300
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a><span data-ttu-id="f4d5d-103">Sledovat vaše rozhraní API s Azure API Management, Event Hubs a Runscope</span><span class="sxs-lookup"><span data-stu-id="f4d5d-103">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>
<span data-ttu-id="f4d5d-104">[Služba API Management](api-management-key-concepts.md) poskytuje mnoho možností pro zlepšení zpracování požadavky HTTP odeslané na rozhraní API HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-104">The [API Management service](api-management-key-concepts.md) provides many capabilities to enhance the processing of HTTP requests sent to your HTTP API.</span></span> <span data-ttu-id="f4d5d-105">Existenci požadavky a odpovědi jsou však přechodný.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-105">However, the existence of the requests and responses are transient.</span></span> <span data-ttu-id="f4d5d-106">Zadání požadavku a ven prochází přes službu API Management na váš back-end rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-106">The request is made and it flows through the API Management service to your backend API.</span></span> <span data-ttu-id="f4d5d-107">Rozhraní API zpracuje požadavek a odpověď toků zpátky pomocí rozhraní API příjemci.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-107">Your API processes the request and a response flows back through to the API consumer.</span></span> <span data-ttu-id="f4d5d-108">Služba API Management udržuje některých důležitých statistik o rozhraní API pro zobrazení v řídicím panelu portálu vydavatele, ale i mimo, že podrobnosti jsou pryč.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-108">The API Management service keeps some important statistics about the APIs for display in the Publisher portal dashboard, but beyond that, the details are gone.</span></span>

<span data-ttu-id="f4d5d-109">Pomocí [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [zásad](api-management-howto-policies.md) ve službě API Management můžete odesílat žádné informace z požadavku a odpovědi na [centra událostí Azure](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="f4d5d-109">By using the [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [policy](api-management-howto-policies.md) in the API Management service you can send any details from the request and response to an [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span> <span data-ttu-id="f4d5d-110">Existuje mnoho různých důvodů, proč můžete chtít generovat události z protokolu HTTP zprávy odesílané do vašeho rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-110">There are a variety of reasons why you may want to generate events from HTTP messages being sent to your APIs.</span></span> <span data-ttu-id="f4d5d-111">Mezi příklady patří záznam pro audit aktualizací, analýzy využití, výstrahy výjimek a integrace 3. stran.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-111">Some examples include audit trail of updates, usage analytics, exception alerting and 3rd party integrations.</span></span>   

<span data-ttu-id="f4d5d-112">Tento článek ukazuje, jak k zaznamenání celé zprávy požadavku a odpovědi protokolu HTTP, odešle do centra událostí a pak tuto zprávu třetí straně služba, která poskytuje HTTP protokolování a monitorování služeb předávání.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-112">This article demonstrates how to capture the entire HTTP request and response message, send it to an Event Hub and then relay that message to a third party service that provides HTTP logging and monitoring services.</span></span>

## <a name="why-send-from-api-management-service"></a><span data-ttu-id="f4d5d-113">Proč odeslat z služby API Management?</span><span class="sxs-lookup"><span data-stu-id="f4d5d-113">Why Send From API Management Service?</span></span>
<span data-ttu-id="f4d5d-114">Je možné zapisovat middleware HTTP, který můžete připojit k rozhraní HTTP API k zaznamenání požadavky a odpovědi HTTP a kanálu je do protokolování a monitorování systémů.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-114">It is possible to write HTTP middleware that can plug into HTTP API frameworks to capture HTTP requests and responses and feed them into logging and monitoring systems.</span></span> <span data-ttu-id="f4d5d-115">Nevýhodou tento přístup je HTTP middleware musí být integrovaná do rozhraní API back-end a platformou rozhraní API se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-115">The downside to this approach is the HTTP middleware needs to be integrated into the backend API and must match the platform of the API.</span></span> <span data-ttu-id="f4d5d-116">Pokud máte více rozhraní API, musíte nasadit každé z nich middleware.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-116">If there are multiple APIs then each one must deploy the middleware.</span></span> <span data-ttu-id="f4d5d-117">Často existuje několik důvodů, proč nelze aktualizovat back-end rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-117">Often there are reasons why backend APIs cannot be updated.</span></span>

<span data-ttu-id="f4d5d-118">Pomocí služby Azure API Management integrovat s infrastrukturou protokolování poskytuje centralizovaný a nezávislé na platformě řešení.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-118">Using the Azure API Management service to integrate with logging infrastructure provides a centralized and platform-independent solution.</span></span> <span data-ttu-id="f4d5d-119">Je také škálovatelná částečně kvůli a [geografická replikace](api-management-howto-deploy-multi-region.md) možnosti služby Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-119">It is also scalable, in part due to the [geo-replication](api-management-howto-deploy-multi-region.md) capabilities of Azure API Management.</span></span>

## <a name="why-send-to-an-azure-event-hub"></a><span data-ttu-id="f4d5d-120">Proč odeslat do centra událostí Azure?</span><span class="sxs-lookup"><span data-stu-id="f4d5d-120">Why send to an Azure Event Hub?</span></span>
<span data-ttu-id="f4d5d-121">Je možné logicky požádat, proč vytvořit zásadu, která je specifická pro Azure Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="f4d5d-121">It is a reasonable to ask, why create a policy that is specific to Azure Event Hubs?</span></span> <span data-ttu-id="f4d5d-122">Existuje mnoho různých místech, kde může chcete protokolu Moje žádosti.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-122">There are many different places where I might want to log my requests.</span></span> <span data-ttu-id="f4d5d-123">Proč právě neodesílal žádosti přímo do konečného umístění?</span><span class="sxs-lookup"><span data-stu-id="f4d5d-123">Why not just send the requests directly to the final destination?</span></span>  <span data-ttu-id="f4d5d-124">To je možnost.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-124">That is an option.</span></span> <span data-ttu-id="f4d5d-125">Při provádění protokolování požadavky od služby API management, je však nutné vzít v úvahu, jak protokolování zpráv ovlivní výkon rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-125">However, when making logging requests from an API management service, it is necessary to consider how logging messages will impact the performance of the API.</span></span> <span data-ttu-id="f4d5d-126">Postupná nárůst zatížení lze zpracovávat zvýšením dostupných instancí komponent systému nebo přímým geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-126">Gradual increases in load can be handled by increasing available instances of system components or by taking advantage of geo-replication.</span></span> <span data-ttu-id="f4d5d-127">Krátký špičky v provozu však může způsobit žádosti o výrazně odloží Pokud požadavky na infrastrukturu protokolování spustit zpomalit zatížení.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-127">However, short spikes in traffic can cause requests to be significantly delayed if requests to logging infrastructure start to slow under load.</span></span>

<span data-ttu-id="f4d5d-128">Azure Event Hubs je určena pro příjem příchozích dat obrovské objemy dat, s kapacitou pro plánování práce s daleko vyšší počet událostí, než počet požadavků HTTP většina proces rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-128">The Azure Event Hubs is designed to ingress huge volumes of data, with capacity for dealing with a far higher number of events than the number of HTTP requests most APIs process.</span></span> <span data-ttu-id="f4d5d-129">Centra událostí funguje jako sofistikované vyrovnávací paměti mezi služby API management a infrastruktury, který bude ukládat a zpracovávat zprávy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-129">The Event Hub acts as a kind of sophisticated buffer between your API management service and the infrastructure that will store and process the messages.</span></span> <span data-ttu-id="f4d5d-130">Tím se zajistí, že nebude z důvodu protokolování infrastruktury sníží výkon vašich rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-130">This ensures that your API performance will not suffer due to the logging infrastructure.</span></span>  

<span data-ttu-id="f4d5d-131">Jakmile data byla předána do centra událostí je je trvalá a bude čekat centra událostí příjemci zpracovat.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-131">Once the data has been passed to an Event Hub it is persisted and will wait for Event Hub consumers to process it.</span></span> <span data-ttu-id="f4d5d-132">Centra událostí nezáleží na tom, jak bude zpracována, ho záleží jenom tak, že budou úspěšně doručovat zprávy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-132">The Event Hub does not care how it will be processed, it just cares about making sure the message will be successfully delivered.</span></span>     

<span data-ttu-id="f4d5d-133">Služba Event Hubs mít možnost datového proudu událostí do několika skupin uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-133">Event Hubs have the ability to stream events to multiple consumer groups.</span></span> <span data-ttu-id="f4d5d-134">To umožňuje událostí ke zpracování úplně jiné systémy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-134">This allows events to be processed by completely different systems.</span></span> <span data-ttu-id="f4d5d-135">To umožňuje podporovat mnoho scénáře integrace bez uvedení Přidání zpoždění na zpracování požadavku rozhraní API v rámci služby API Management, jako je třeba vytvořit pouze jednu událost.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-135">This enables supporting many integration scenarios without putting addition delays on the processing of the API request within the API Management service as only one event needs to be generated.</span></span>

## <a name="a-policy-to-send-applicationhttp-messages"></a><span data-ttu-id="f4d5d-136">Zásada pro odesílání zpráv application/http</span><span class="sxs-lookup"><span data-stu-id="f4d5d-136">A policy to send application/http messages</span></span>
<span data-ttu-id="f4d5d-137">Centra událostí přijme data událostí jako jednoduchý řetězec.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-137">An Event Hub accepts event data as a simple string.</span></span> <span data-ttu-id="f4d5d-138">Obsah tento řetězec je zcela na vás.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-138">The contents of that string are completely up to you.</span></span> <span data-ttu-id="f4d5d-139">Abyste mohli zabalit požadavku HTTP a odesílat do centra událostí potřebujeme se naformátovat řetězec s informacemi o požadavku nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-139">To be able to package up an HTTP request and send it off to Event Hubs we need to format the string with the request or response information.</span></span> <span data-ttu-id="f4d5d-140">V situacích, jako to pokud je existující formát můžeme opakovaně a potom jsme nemusí mít k zápisu vlastní analýze kódu.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-140">In situations like this, if there is an existing format we can reuse, then we may not have to write our own parsing code.</span></span> <span data-ttu-id="f4d5d-141">Původně I považována za použití [HAR](http://www.softwareishard.com/blog/har-12-spec/) pro odesílání požadavků a odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-141">Initially I considered using the [HAR](http://www.softwareishard.com/blog/har-12-spec/) for sending HTTP requests and responses.</span></span> <span data-ttu-id="f4d5d-142">Tento formát je však optimalizovaná pro ukládání pořadí požadavků HTTP ve formátu JSON na základě.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-142">However, this format is optimized for storing a sequence of HTTP requests in a JSON based format.</span></span> <span data-ttu-id="f4d5d-143">Obsahuje povinné prvky, které přidány nepotřebné složitější scénář předávání zpráv HTTP prostřednictvím sítě.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-143">It contained a number of mandatory elements that added unnecessary complexity for the scenario of passing the HTTP message over the wire.</span></span>  

<span data-ttu-id="f4d5d-144">Alternativní možnost byla používat `application/http` typ média, jak je popsáno v specifikace protokolu HTTP [RFC 7230](http://tools.ietf.org/html/rfc7230).</span><span class="sxs-lookup"><span data-stu-id="f4d5d-144">An alternative option was to use the `application/http` media type as described in the HTTP specification [RFC 7230](http://tools.ietf.org/html/rfc7230).</span></span> <span data-ttu-id="f4d5d-145">Tento typ média používá přesný stejný formát, který se používá ve skutečnosti odesílat zprávy HTTP prostřednictvím sítě, ale celá zpráva může být v těle další požadavek HTTP put.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-145">This media type uses the exact same format that is used to actually send HTTP messages over the wire, but the entire message can be put in the body of another HTTP request.</span></span> <span data-ttu-id="f4d5d-146">V našem případě jsme právě budete používat text jako našem zpráv k odeslání do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-146">In our case we are just going to use the body as our message to send to Event Hubs.</span></span> <span data-ttu-id="f4d5d-147">Pohodlně, je analyzátor, který již existuje v [Microsoft ASP.NET Web API 2.2 klienta](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) knihovny, které můžete tento formát analyzovat a převádět je do nativního `HttpRequestMessage` a `HttpResponseMessage` objekty.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-147">Conveniently, there is a parser that exists in [Microsoft ASP.NET Web API 2.2 Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) libraries that can parse this format and convert it into the native `HttpRequestMessage` and `HttpResponseMessage` objects.</span></span>

<span data-ttu-id="f4d5d-148">Abyste mohli vytvořit tuto zprávu, potřebujeme, abyste mohli využívat jazyka C# na základě [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx) v Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-148">To be able to create this message we need to take advantage of C# based [Policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management.</span></span> <span data-ttu-id="f4d5d-149">Zde je zásady, které odešle zprávu požadavku HTTP k Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-149">Here is the policy which sends a HTTP request message to Azure Event Hubs.</span></span>

```xml
<log-to-eventhub logger-id="conferencelogger" partition-id="0">
@{
   var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                               context.Request.Method,
                                               context.Request.Url.Path + context.Request.Url.QueryString);

   var body = context.Request.Body?.As<string>(true);
   if (body != null && body.Length > 1024)
   {
       body = body.Substring(0, 1024);
   }

   var headers = context.Request.Headers
                          .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                          .ToArray<string>();

   var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

   return "request:"   + context.Variables["message-id"] + "\n"
                       + requestLine + headerString + "\r\n" + body;
}
</log-to-eventhub>
```

### <a name="policy-declaration"></a><span data-ttu-id="f4d5d-150">Zásady deklarace</span><span class="sxs-lookup"><span data-stu-id="f4d5d-150">Policy declaration</span></span>
<span data-ttu-id="f4d5d-151">Existuje několik věcí konkrétní důležité zmínit, o tomto výrazu zásad.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-151">There a few particular things worth mentioning about this policy expression.</span></span> <span data-ttu-id="f4d5d-152">Zásady protokolu eventhub má atribut volá protokolovač id, které odkazuje na název protokolovacího nástroje, který byl vytvořen v rámci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-152">The log-to-eventhub policy has an attribute called logger-id which refers to the name of logger that has been created within the API Management service.</span></span> <span data-ttu-id="f4d5d-153">Podrobnosti o tom, jak nastavit protokolovač centra událostí ve službě API Management najdete v dokumentu [jak do protokolu událostí Azure Event Hubs ve službě Azure API Management](api-management-howto-log-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="f4d5d-153">The details of how to setup an Event Hub logger in the API Management service can be found in the document [How to log events to Azure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md).</span></span> <span data-ttu-id="f4d5d-154">Druhý atribut je volitelný parametr, který se dá pokyn centra událostí, které k uložení zpráv v oddílu.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-154">The second attribute is an optional parameter that instructs Event Hubs which partition to store the message in.</span></span> <span data-ttu-id="f4d5d-155">Služba Event Hubs slouží k povolení scalabilty a vyžadují minimálně dva oddíly.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-155">Event Hubs use partitions to enable scalabilty and require a minimum of two.</span></span> <span data-ttu-id="f4d5d-156">Seřazené doručení zpráv z jenom záruku, že se v rámci oddílu.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-156">The ordered delivery of messages is only guaranteed within a partition.</span></span> <span data-ttu-id="f4d5d-157">Pokud jsme vyzvat centra událostí, ve kterém oddílu umístit zprávu, použije algoritmus kruhového dotazování distribuovat zátěž.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-157">If we do not instruct Event Hub in which partition to place the message, it will use a round-robin algorithm to distribute the load.</span></span> <span data-ttu-id="f4d5d-158">Však mohou způsobit některé z našich zprávy, které mají být zpracovány mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-158">However, that may cause some of our messages to be processed out of order.</span></span>  

### <a name="partitions"></a><span data-ttu-id="f4d5d-159">Oddíly</span><span class="sxs-lookup"><span data-stu-id="f4d5d-159">Partitions</span></span>
<span data-ttu-id="f4d5d-160">Aby našem zpráv se dodávají k příjemce v pořadí a využívat možnosti distribuce zatížení oddílů, se rozhodli poslat jeden oddíl a zpráv odpovědí HTTP na druhý oddíl zprávy požadavků HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-160">To ensure our messages are delivered to consumers in order and take advantage of the load distribution capability of partitions, I chose to send HTTP request messages to one partition and HTTP response messages to a second partition.</span></span> <span data-ttu-id="f4d5d-161">Tím bude zajištěno jako distribučního i zatížení a jsme může zaručit, že všechny požadavky se budou v pořadí a všechny odpovědi se budou v pořadí.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-161">This will ensure an even load distribution and we can guarantee that all requests will be consumed in order and all responses will be consumed in order.</span></span> <span data-ttu-id="f4d5d-162">Je možné pro odpověď, který se má používat před odpovídající požadavku, ale jako, který se nejedná o problém, protože máme jiný mechanismus pro korelace žádosti odpovědí a víme, že požadavky vždy dřívější než odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-162">It is possible for a response to be consumed before the corresponding request, but as that is not a problem as we have a different mechanism for correlating requests to responses and we know that requests always come before responses.</span></span>

### <a name="http-payloads"></a><span data-ttu-id="f4d5d-163">Datové části HTTP</span><span class="sxs-lookup"><span data-stu-id="f4d5d-163">HTTP payloads</span></span>
<span data-ttu-id="f4d5d-164">Po sestavení `requestLine` jsme zkontrolujte, pokud by se zkrátila textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-164">After building the `requestLine` we check to see if the request body should be truncated.</span></span> <span data-ttu-id="f4d5d-165">Text žádosti se zkrátí na pouze 1024.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-165">The request body is truncated to only 1024.</span></span> <span data-ttu-id="f4d5d-166">To může být vyšší, ale jednotlivé zprávy centra událostí jsou omezeny na 256KB, takže je pravděpodobné, že některé zprávy HTTP subjekty se nevejdou do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-166">This could be increased, however individual Event Hub messages are limited to 256KB, so it is likely that some HTTP message bodies will not fit in a single message.</span></span> <span data-ttu-id="f4d5d-167">Při provádění modulu protokolování a analýza významné množství informací může být odvozen od právě řádek požadavku HTTP a hlavičky.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-167">When doing logging and analytics a significant amount of information can be derived from just the HTTP request line and headers.</span></span> <span data-ttu-id="f4d5d-168">Také mnoho žádostí o rozhraní API vrátit pouze malé subjekty a stejně tak ztrátě informační hodnotu zkrácením velké těla poměrně minimální oproti snížení přenos, zpracování a náklady na úložiště, které chcete zachovat veškerý obsah textu.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-168">Also, many API requests only return small bodies and so the loss of information value by truncating large bodies is fairly minimal in comparison to the reduction in transfer, processing and storage costs to keep all body contents.</span></span> <span data-ttu-id="f4d5d-169">Poslední OneNote o zpracování textu je, že musíme předat `true` do As<string>() metoda vzhledem k tomu, že jsme čtou text obsahu, avšak byla také chcete back-end rozhraní API, abyste mohli ke čtení textu.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-169">One final note about processing the body is that we need to pass `true` to the As<string>() method because we are reading the body contents, but was also want the backend API to be able to read the body.</span></span> <span data-ttu-id="f4d5d-170">Předáním hodnotu PRAVDA, aby tato metoda způsobit jsme text do uložených do vyrovnávací paměti, takže lze číst ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-170">By passing true to this method we cause the body to be buffered so that it can be read a second time.</span></span> <span data-ttu-id="f4d5d-171">To je důležité si uvědomit Pokud máte rozhraní API, který nemá odesílání velmi velké soubory nebo používá dlouhým dotazováním.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-171">This is important to be aware of if you have an API that does uploading of very large files or uses long polling.</span></span> <span data-ttu-id="f4d5d-172">V těchto případech je vyhýbat se vůbec čtení textu.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-172">In these cases it would be best to avoid reading the body at all.</span></span>   

### <a name="http-headers"></a><span data-ttu-id="f4d5d-173">Hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="f4d5d-173">HTTP headers</span></span>
<span data-ttu-id="f4d5d-174">Hlavičky protokolu HTTP můžete jednoduše přenosu do formátu zprávy ve formátu pár klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-174">HTTP Headers can be simply transferred over into the message format in a simple key/value pair format.</span></span> <span data-ttu-id="f4d5d-175">Jsme se rozhodli nepoužijí určitá citlivé pole zabezpečení, aby se zabránilo zbytečně úniku přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-175">We have chosen to strip out certain security sensitive fields, to avoid unnecessarily leaking credential information.</span></span> <span data-ttu-id="f4d5d-176">Není pravděpodobné, že klíče rozhraní API a jiná pověření budou použita pro účely analýzy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-176">It is unlikely that API keys and other credentials would be used for analytics purposes.</span></span> <span data-ttu-id="f4d5d-177">Pokud nám chcete provést analýzu na uživatele a konkrétní produkt používají pak jsme může dojít, která z `context` objektu a přidat ke zprávě.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-177">If we wish to do analysis on the user and the particular product they are using then we could get that from the `context` object and add that to the message.</span></span>     

### <a name="message-metadata"></a><span data-ttu-id="f4d5d-178">Zpráva metadat</span><span class="sxs-lookup"><span data-stu-id="f4d5d-178">Message Metadata</span></span>
<span data-ttu-id="f4d5d-179">Při vytváření dokončení zpráva k odeslání do centra událostí, první řádek není ve skutečnosti součástí `application/http` zprávy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-179">When building the complete message to send to the event hub, the first line is not actually part of the `application/http` message.</span></span> <span data-ttu-id="f4d5d-180">První řádek je tvořený, zda je daná zpráva požadavku nebo odpovědi zprávu a id zprávy, který se používá ke koordinaci požadavků na odpovědi na dalších metadat.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-180">The first line is additional metadata consisting of whether the message is a request or response message and a message id which is used to correlate requests to responses.</span></span> <span data-ttu-id="f4d5d-181">Id zprávy je vytvořená pomocí jiné zásady, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f4d5d-181">The message id is created by using another policy that looks like this:</span></span>

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

<span data-ttu-id="f4d5d-182">Jsme může mít vytvořil zprávu požadavku, který uložené v proměnné, dokud odpověď se vrátí a jednoduše odeslaný žádosti a odpovědi jako do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-182">We could have created the request message, stored that in a variable until the response was returned and then simply sent the request and response as a single message.</span></span> <span data-ttu-id="f4d5d-183">Ale odesílání žádostí a odpovědí nezávisle a použitím id zprávy ke korelaci dvou, se nám získat o něco větší flexibilitu v velikost zprávy, umožňuje využít výhod více oddílů, zatímco zachování pořadí zpráv a požadavek se zobrazí v našem protokolování řídicí panel dříve.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-183">However, by sending the request and response independently and using a message id to correlate the two, we get a bit more flexibility in the message size, the ability to take advantage of multiple partitions whilst maintaining message order and the request will appear in our logging dashboard sooner.</span></span> <span data-ttu-id="f4d5d-184">Je také možné některých scénářích, kdy platnou odpověď se nikdy neodesílá do centra událostí, pravděpodobně z důvodu chyby závažná žádost ve službě API Management, ale stále pomůžeme záznam požadavku.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-184">There also may be some scenarios where a valid response is never sent to the event hub, possibly due to a fatal request error in the API Management service, but we still will have a record of the request.</span></span>

<span data-ttu-id="f4d5d-185">Zásadu odeslat zprávu odpovědi HTTP velmi podobná na žádost a tak konfiguraci dokončení zásad vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f4d5d-185">The policy to send the response HTTP message looks very similar to the request and so the complete policy configuration looks like this:</span></span>

```xml
<policies>
  <inbound>
      <set-variable name="message-id" value="@(Guid.NewGuid())" />
      <log-to-eventhub logger-id="conferencelogger" partition-id="0">
      @{
          var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                      context.Request.Method,
                                                      context.Request.Url.Path + context.Request.Url.QueryString);

          var body = context.Request.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Request.Headers
                               .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                               .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                               .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "request:"   + context.Variables["message-id"] + "\n"
                              + requestLine + headerString + "\r\n" + body;
      }
  </log-to-eventhub>
  </inbound>
  <backend>
      <forward-request follow-redirects="true" />
  </backend>
  <outbound>
      <log-to-eventhub logger-id="conferencelogger" partition-id="1">
      @{
          var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                              context.Response.StatusCode,
                                              context.Response.StatusReason);

          var body = context.Response.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Response.Headers
                                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                          .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "response:"  + context.Variables["message-id"] + "\n"
                              + statusLine + headerString + "\r\n" + body;
     }
  </log-to-eventhub>
  </outbound>
</policies>
```

<span data-ttu-id="f4d5d-186">`set-variable` Zásad vytvoří hodnotu, která je přístupný pro oba `log-to-eventhub` zásad v `<inbound>` části a `<outbound>` části.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-186">The `set-variable` policy creates a value that is accessible by both the `log-to-eventhub` policy in the `<inbound>` section and the `<outbound>` section.</span></span>  

## <a name="receiving-events-from-event-hubs"></a><span data-ttu-id="f4d5d-187">Přijímání události ze služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f4d5d-187">Receiving events from Event Hubs</span></span>
<span data-ttu-id="f4d5d-188">Přijetí události z centra událostí Azure pomocí [protokolu AMQP](http://www.amqp.org/).</span><span class="sxs-lookup"><span data-stu-id="f4d5d-188">Events from Azure Event Hub are received using the [AMQP protocol](http://www.amqp.org/).</span></span> <span data-ttu-id="f4d5d-189">Tým Microsoft Service Bus provedli klientské knihovny, které jsou k dispozici pro usnadnění náročné události.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-189">The Microsoft Service Bus team have made client libraries available to make the consuming events easier.</span></span> <span data-ttu-id="f4d5d-190">Existují dva různé přístupy, které jsou podporovány, jednu, která má být *přímý příjemce* a druhý je použití `EventProcessorHost` třídy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-190">There are two different approaches supported, one is being a *Direct Consumer* and the other is using the `EventProcessorHost` class.</span></span> <span data-ttu-id="f4d5d-191">Příklady tyto dva přístupy lze nalézt v [Průvodce programováním centra událostí](../event-hubs/event-hubs-programming-guide.md).</span><span class="sxs-lookup"><span data-stu-id="f4d5d-191">Examples of these two approaches can be found in the [Event Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).</span></span> <span data-ttu-id="f4d5d-192">Je zkrácený rozdíly, `Direct Consumer` umožňuje úplnou kontrolu a `EventProcessorHost` nepodporuje některé úkoly vložení pro ale díky předem určité domněnky o tom, jak bude zpracovávat události.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-192">The short version of the differences is, `Direct Consumer` gives you complete control and the `EventProcessorHost` does some of the plumbing work for you but makes certain assumptions about how you will process those events.</span></span>  

### <a name="eventprocessorhost"></a><span data-ttu-id="f4d5d-193">EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="f4d5d-193">EventProcessorHost</span></span>
<span data-ttu-id="f4d5d-194">V této ukázce používáme `EventProcessorHost` pro jednoduchost, ale může není nejlepší volbou pro tento konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-194">In this sample, we will use the `EventProcessorHost` for simplicity, however it may not the best choice for this particular scenario.</span></span> <span data-ttu-id="f4d5d-195">`EventProcessorHost`nemá náročné práce tak, že nemusíte si dělat starosti o problémy v rámci třídy procesoru určitá událost dělení na vlákna.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-195">`EventProcessorHost` does the hard work of making sure you don't have to worry about threading issues within a particular event processor class.</span></span> <span data-ttu-id="f4d5d-196">Přesto však v tomto scénáři jsou jednoduše převod zprávy do jiného formátu a předání podél do jiné služby pomocí asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-196">However, in our scenario, we are simply converting the message to another format and passing it along to another service using an async method.</span></span> <span data-ttu-id="f4d5d-197">Není nutné pro aktualizaci sdíleného stavu a proto žádné riziko problémy dělení na vlákna.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-197">There is no need for updating shared state and therefore no risk of threading issues.</span></span> <span data-ttu-id="f4d5d-198">Pro většinu scénářů `EventProcessorHost` je pravděpodobně nejlepší volbou a je určitě jednodušší možnost.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-198">For most scenarios, `EventProcessorHost` is probably the best choice and it is certainly the easier option.</span></span>     

### <a name="ieventprocessor"></a><span data-ttu-id="f4d5d-199">IEventProcessor</span><span class="sxs-lookup"><span data-stu-id="f4d5d-199">IEventProcessor</span></span>
<span data-ttu-id="f4d5d-200">Při použití centrální koncept `EventProcessorHost` je vytvoření implementace `IEventProcessor` rozhraní, které obsahuje metodu `ProcessEventAsync`.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-200">The central concept when using `EventProcessorHost` is to create a an implementation of the `IEventProcessor` interface which contains the method `ProcessEventAsync`.</span></span> <span data-ttu-id="f4d5d-201">Zobrazí se zde je zásadní podpora této metody:</span><span class="sxs-lookup"><span data-stu-id="f4d5d-201">The essence of that method is shown here:</span></span>

```c#
async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
{

   foreach (EventData eventData in messages)
   {
       _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

       try
       {
           var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
           await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
       }
       catch (Exception ex)
       {
           _Logger.LogError(ex.Message);
       }
   }
    ... checkpointing code snipped ...
}
```

<span data-ttu-id="f4d5d-202">Seznam objektů EventData se předávají do metody a jsme iterace v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-202">A list of EventData objects are passed into the method and we iterate over that list.</span></span> <span data-ttu-id="f4d5d-203">Počet bajtů jednotlivých metod jsou analyzovány do HttpMessage objektu a tento objekt je předána instanci IHttpMessageProcessor.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-203">The bytes of each method are parsed into a HttpMessage object and that object is passed to an instance of IHttpMessageProcessor.</span></span>

### <a name="httpmessage"></a><span data-ttu-id="f4d5d-204">HttpMessage</span><span class="sxs-lookup"><span data-stu-id="f4d5d-204">HttpMessage</span></span>
<span data-ttu-id="f4d5d-205">`HttpMessage` Instance obsahuje tři druhy dat:</span><span class="sxs-lookup"><span data-stu-id="f4d5d-205">The `HttpMessage` instance contains three pieces of data:</span></span>

```c#
public class HttpMessage
{
   public Guid MessageId { get; set; }
   public bool IsRequest { get; set; }
   public HttpRequestMessage HttpRequestMessage { get; set; }
   public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

<span data-ttu-id="f4d5d-206">`HttpMessage` Instance obsahuje `MessageId` identifikátor GUID, který umožňuje nám se připojit k odpovídající odpověď HTTP a logickou hodnotu, která označuje, jestli objekt obsahuje instanci objektu HttpRequestMessage a objekt HttpResponseMessage požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-206">The `HttpMessage` instance contains a `MessageId` GUID that allows us to connect the HTTP request to the corresponding HTTP response and a boolean value that identifies if the object contains an instance of a HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="f4d5d-207">Pomocí předdefinovaných ve třídách HTTP z `System.Net.Http`, bylo možné využívat výhod `application/http` analýza kódu, který je součástí `System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-207">By using the built in HTTP classes from `System.Net.Http`, I was able to take advantage of the `application/http` parsing code that is included in `System.Net.Http.Formatting`.</span></span>  

### <a name="ihttpmessageprocessor"></a><span data-ttu-id="f4d5d-208">IHttpMessageProcessor</span><span class="sxs-lookup"><span data-stu-id="f4d5d-208">IHttpMessageProcessor</span></span>
<span data-ttu-id="f4d5d-209">`HttpMessage` Instance je předán implementace `IHttpMessageProcessor` což je rozhraní po vytvoření oddělit přijetí a interpretace události z centra událostí Azure a vlastní zpracování ho.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-209">The `HttpMessage` instance is then forwarded to implementation of `IHttpMessageProcessor` which is an interface I created to decouple the receiving and interpretation of the event from Azure Event Hub and the actual processing of it.</span></span>

## <a name="forwarding-the-http-message"></a><span data-ttu-id="f4d5d-210">Předávání zpráv protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="f4d5d-210">Forwarding the HTTP message</span></span>
<span data-ttu-id="f4d5d-211">Tato ukázka rozhodli je zajímavé nabízená požadavku HTTP přes [Runscope](http://www.runscope.com).</span><span class="sxs-lookup"><span data-stu-id="f4d5d-211">For this sample, I decided it would be interesting to push the HTTP Request over to [Runscope](http://www.runscope.com).</span></span> <span data-ttu-id="f4d5d-212">Runscope je cloudové služby, který se specializuje na protokolu HTTP, ladění, protokolování a monitorování.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-212">Runscope is a cloud based service that specializes in HTTP debugging, logging and monitoring.</span></span> <span data-ttu-id="f4d5d-213">Mají volné vrstvy, takže je snadné a zkuste to a umožňuje nám najdete v části požadavky HTTP v reálném čase předávaných mezi naše služba API Management.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-213">They have a free tier, so it is easy to try and it allows us to see the HTTP requests in real-time flowing through our API Management service.</span></span>

<span data-ttu-id="f4d5d-214">`IHttpMessageProcessor` Implementace vypadá to,</span><span class="sxs-lookup"><span data-stu-id="f4d5d-214">The `IHttpMessageProcessor` implementation looks like this,</span></span>

```c#
public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
{
   private HttpClient _HttpClient;
   private ILogger _Logger;
   private string _BucketKey;
   public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
   {
       _HttpClient = httpClient;
       var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
       _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
       _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
       _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
       _Logger = logger;
   }

   public async Task ProcessHttpMessage(HttpMessage message)
   {
       var runscopeMessage = new RunscopeMessage()
       {
           UniqueIdentifier = message.MessageId
       };

       if (message.IsRequest)
       {
           _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
           runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
       }
       else
       {
           _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
           runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
       }

       var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
       messagesLink.BucketKey = _BucketKey;
       messagesLink.RunscopeMessage = runscopeMessage;
       var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
       _Logger.LogDebug("Request sent to Runscope");
   }
}
```

<span data-ttu-id="f4d5d-215">Bylo možné využívat [existující klientské knihovny pro Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) který usnadňuje nabízené `HttpRequestMessage` a `HttpResponseMessage` instance až do své služby.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-215">I was able to take advantage of an [existing client library for Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) that makes it easy to push `HttpRequestMessage` and `HttpResponseMessage` instances up into their service.</span></span> <span data-ttu-id="f4d5d-216">Chcete-li získat přístup k rozhraní API Runscope budete potřebovat účet a klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-216">In order to access the Runscope API you will need an account and an API Key.</span></span> <span data-ttu-id="f4d5d-217">Pokyny pro získání klíč rozhraní API naleznete v [vytvoření aplikace API Runscope přístup](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) záznam dění na monitoru.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-217">Instructions for getting an API key can be found in the [Creating Applications to Access Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) screencast.</span></span>

## <a name="complete-sample"></a><span data-ttu-id="f4d5d-218">Ucelenou ukázku</span><span class="sxs-lookup"><span data-stu-id="f4d5d-218">Complete sample</span></span>
<span data-ttu-id="f4d5d-219">[Zdrojový kód](https://github.com/darrelmiller/ApimEventProcessor) a testy pro ukázce na Githubu.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-219">The [source code](https://github.com/darrelmiller/ApimEventProcessor) and tests for the sample are on GitHub.</span></span> <span data-ttu-id="f4d5d-220">Budete potřebovat [služby API Management](api-management-get-started.md), [připojeného centra událostí](api-management-howto-log-event-hubs.md)a [účet úložiště](../storage/common/storage-create-storage-account.md) ke spuštění ukázky sami.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-220">You will need an [API Management Service](api-management-get-started.md), [a connected Event Hub](api-management-howto-log-event-hubs.md), and a [Storage Account](../storage/common/storage-create-storage-account.md) to run the sample for yourself.</span></span>   

<span data-ttu-id="f4d5d-221">Ukázka je stejně jednoduché konzolovou aplikaci, která naslouchá pro události pocházející z centra událostí, je do převede `HttpRequestMessage` a `HttpResponseMessage` objekty a předává je na rozhraní API Runscope.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-221">The sample is just a simple Console application that listens for events coming from Event Hub, converts them into a `HttpRequestMessage` and `HttpResponseMessage` objects and then forwards them on to the Runscope API.</span></span>

<span data-ttu-id="f4d5d-222">Na následujícím obrázku animovaný se zobrazí žádost o odkazy na rozhraní API v portálu pro vývojáře, konzolové aplikace zobrazuje zprávy přijaté, zpracován a předávat a pak požadavku a odpovědi zobrazovat na inspector Runscope provoz.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-222">In the following animated image, you can see a request being made to an API in the Developer Portal, the Console application showing the message being received, processed and forwarded and then the request and response showing up in the Runscope Traffic inspector.</span></span>

![Ukázka požadavku předávaná Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a><span data-ttu-id="f4d5d-224">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f4d5d-224">Summary</span></span>
<span data-ttu-id="f4d5d-225">Služba Azure API Management poskytuje ideální místo pro zachycení provozu HTTP cestách do a z vašich rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-225">Azure API Management service provides an ideal place to capture the HTTP traffic travelling to and from your APIs.</span></span> <span data-ttu-id="f4d5d-226">Azure Event Hubs je vysoce škálovatelnou a nenákladné řešení pro zaznamenání tento přenos a vložené ho do sekundární zpracování dat pro protokolování, sledování a další pokročilé analýzy.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-226">Azure Event Hubs is a highly scalable, low cost solution for capturing that traffic and feeding it into secondary processing systems for logging, monitoring and other sophisticated analytics.</span></span> <span data-ttu-id="f4d5d-227">Připojení k monitorování systémů jako Runscope je jednoduchý jako několik desítek řádků kódu 3. stran provoz.</span><span class="sxs-lookup"><span data-stu-id="f4d5d-227">Connecting to 3rd party traffic monitoring systems like Runscope is a simple as a few dozen lines of code.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4d5d-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4d5d-228">Next steps</span></span>
* <span data-ttu-id="f4d5d-229">Další informace o Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f4d5d-229">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="f4d5d-230">Začínáme s Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f4d5d-230">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="f4d5d-231">Přijímat zprávy pomocí třídy EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="f4d5d-231">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="f4d5d-232">Průvodce programováním pro službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f4d5d-232">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="f4d5d-233">Další informace o integraci API Management a služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f4d5d-233">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="f4d5d-234">Jak zapisovat do protokolu událostí Azure Event Hubs ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="f4d5d-234">How to log events to Azure Event Hubs in Azure API Management</span></span>](api-management-howto-log-event-hubs.md)
  * [<span data-ttu-id="f4d5d-235">Odkaz na entitu protokolovacího nástroje</span><span class="sxs-lookup"><span data-stu-id="f4d5d-235">Logger entity reference</span></span>](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [<span data-ttu-id="f4d5d-236">referenční informace o protokolu eventhub zásad</span><span class="sxs-lookup"><span data-stu-id="f4d5d-236">log-to-eventhub policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
