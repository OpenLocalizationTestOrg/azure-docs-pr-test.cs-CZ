---
title: "aaaMonitor rozhraní API pomocí Azure API Management, Event Hubs a Runscope | Microsoft Docs"
description: "Ukázkovou aplikaci ukázka zásad protokolu eventhub hello připojování Azure API Management, Azure Event Hubs a Runscope pro protokol HTTP, protokolování a monitorování"
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
ms.openlocfilehash: 7456a2436f3a2d7b815b70b65fca9481d39c5fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a><span data-ttu-id="092f5-103">Sledovat vaše rozhraní API s Azure API Management, Event Hubs a Runscope</span><span class="sxs-lookup"><span data-stu-id="092f5-103">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>
<span data-ttu-id="092f5-104">Hello [služba API Management](api-management-key-concepts.md) poskytuje tooenhance možnosti mnoho hello zpracování HTTP požadavky odeslané tooyour rozhraní API HTTP.</span><span class="sxs-lookup"><span data-stu-id="092f5-104">hello [API Management service](api-management-key-concepts.md) provides many capabilities tooenhance hello processing of HTTP requests sent tooyour HTTP API.</span></span> <span data-ttu-id="092f5-105">Ale hello existenci hello požadavky a odpovědi jsou přechodný.</span><span class="sxs-lookup"><span data-stu-id="092f5-105">However, hello existence of hello requests and responses are transient.</span></span> <span data-ttu-id="092f5-106">Hello požadavku a ven prochází přes hello API Management service tooyour back-end rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-106">hello request is made and it flows through hello API Management service tooyour backend API.</span></span> <span data-ttu-id="092f5-107">Rozhraní API zpracovává hello požadavek a odpověď zpět prochází příjemce toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-107">Your API processes hello request and a response flows back through toohello API consumer.</span></span> <span data-ttu-id="092f5-108">Hello služba API Management udržuje některých důležitých statistik o hello rozhraní API pro zobrazení v hello řídicí panel portálu vydavatele, ale nad rámec, hello podrobnosti jsou pryč.</span><span class="sxs-lookup"><span data-stu-id="092f5-108">hello API Management service keeps some important statistics about hello APIs for display in hello Publisher portal dashboard, but beyond that, hello details are gone.</span></span>

<span data-ttu-id="092f5-109">Pomocí hello [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [zásad](api-management-howto-policies.md) v hello služba API Management můžete odesílat žádné informace z hello žádostí a odpovědí tooan [centra událostí Azure](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="092f5-109">By using hello [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [policy](api-management-howto-policies.md) in hello API Management service you can send any details from hello request and response tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span> <span data-ttu-id="092f5-110">Existuje mnoho různých důvodů, proč může být vhodné toogenerate události z protokolu HTTP zprávy odesílané tooyour rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-110">There are a variety of reasons why you may want toogenerate events from HTTP messages being sent tooyour APIs.</span></span> <span data-ttu-id="092f5-111">Mezi příklady patří záznam pro audit aktualizací, analýzy využití, výstrahy výjimek a integrace 3. stran.</span><span class="sxs-lookup"><span data-stu-id="092f5-111">Some examples include audit trail of updates, usage analytics, exception alerting and 3rd party integrations.</span></span>   

<span data-ttu-id="092f5-112">Tento článek ukazuje, jak toocapture hello celý žádosti a odpovědi zprávy HTTP, odešle tooan centra událostí a pak tuto zprávu tooa třetích stran služba, která poskytuje HTTP protokolování a monitorování služeb předávání.</span><span class="sxs-lookup"><span data-stu-id="092f5-112">This article demonstrates how toocapture hello entire HTTP request and response message, send it tooan Event Hub and then relay that message tooa third party service that provides HTTP logging and monitoring services.</span></span>

## <a name="why-send-from-api-management-service"></a><span data-ttu-id="092f5-113">Proč odeslat z služby API Management?</span><span class="sxs-lookup"><span data-stu-id="092f5-113">Why Send From API Management Service?</span></span>
<span data-ttu-id="092f5-114">Je možné toowrite HTTP middleware, který můžete připojit k rozhraní API HTTP architektury toocapture požadavky a odpovědi HTTP a kanálu je do protokolování a monitorování systémů.</span><span class="sxs-lookup"><span data-stu-id="092f5-114">It is possible toowrite HTTP middleware that can plug into HTTP API frameworks toocapture HTTP requests and responses and feed them into logging and monitoring systems.</span></span> <span data-ttu-id="092f5-115">Hello nevýhodou toothis přístup je hello HTTP middleware musí toobe integrována do back-end hello rozhraní API a hello platforma hello rozhraní API se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="092f5-115">hello downside toothis approach is hello HTTP middleware needs toobe integrated into hello backend API and must match hello platform of hello API.</span></span> <span data-ttu-id="092f5-116">Pokud máte více rozhraní API, musíte nasadit každé z nich hello middleware.</span><span class="sxs-lookup"><span data-stu-id="092f5-116">If there are multiple APIs then each one must deploy hello middleware.</span></span> <span data-ttu-id="092f5-117">Často existuje několik důvodů, proč nelze aktualizovat back-end rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-117">Often there are reasons why backend APIs cannot be updated.</span></span>

<span data-ttu-id="092f5-118">Pomocí protokolování infrastruktury toointegrate služby Azure API Management hello poskytuje centralizovaný a nezávislé na platformě řešení.</span><span class="sxs-lookup"><span data-stu-id="092f5-118">Using hello Azure API Management service toointegrate with logging infrastructure provides a centralized and platform-independent solution.</span></span> <span data-ttu-id="092f5-119">Je také škálovatelná částečně kvůli toohello [geografická replikace](api-management-howto-deploy-multi-region.md) možnosti služby Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="092f5-119">It is also scalable, in part due toohello [geo-replication](api-management-howto-deploy-multi-region.md) capabilities of Azure API Management.</span></span>

## <a name="why-send-tooan-azure-event-hub"></a><span data-ttu-id="092f5-120">Proč odeslat tooan centra událostí Azure?</span><span class="sxs-lookup"><span data-stu-id="092f5-120">Why send tooan Azure Event Hub?</span></span>
<span data-ttu-id="092f5-121">Je možné logicky tooask, proč vytvořit zásadu, která je konkrétní tooAzure Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="092f5-121">It is a reasonable tooask, why create a policy that is specific tooAzure Event Hubs?</span></span> <span data-ttu-id="092f5-122">Existuje mnoho různých místech, kde může chci toolog Moje žádosti.</span><span class="sxs-lookup"><span data-stu-id="092f5-122">There are many different places where I might want toolog my requests.</span></span> <span data-ttu-id="092f5-123">Proč není právě odesílání hello požadavky přímo konečným cílem toohello?</span><span class="sxs-lookup"><span data-stu-id="092f5-123">Why not just send hello requests directly toohello final destination?</span></span>  <span data-ttu-id="092f5-124">To je možnost.</span><span class="sxs-lookup"><span data-stu-id="092f5-124">That is an option.</span></span> <span data-ttu-id="092f5-125">Při provádění protokolování požadavky od služby API management, je však nutné tooconsider jak protokolování zpráv ovlivní výkon hello hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-125">However, when making logging requests from an API management service, it is necessary tooconsider how logging messages will impact hello performance of hello API.</span></span> <span data-ttu-id="092f5-126">Postupná nárůst zatížení lze zpracovávat zvýšením dostupných instancí komponent systému nebo přímým geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="092f5-126">Gradual increases in load can be handled by increasing available instances of system components or by taking advantage of geo-replication.</span></span> <span data-ttu-id="092f5-127">Krátký špičky v provozu však může způsobit požadavky toobe výrazně zpožděno. Pokud požadavky toologging infrastruktury spustit tooslow zatížení.</span><span class="sxs-lookup"><span data-stu-id="092f5-127">However, short spikes in traffic can cause requests toobe significantly delayed if requests toologging infrastructure start tooslow under load.</span></span>

<span data-ttu-id="092f5-128">Hello Azure Event Hubs je navrženou tooingress obrovské objemy dat, s kapacitou pro práci s daleko vyšší počet událostí, než hello počet HTTP požadavků většina proces rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-128">hello Azure Event Hubs is designed tooingress huge volumes of data, with capacity for dealing with a far higher number of events than hello number of HTTP requests most APIs process.</span></span> <span data-ttu-id="092f5-129">Hello centra událostí funguje jako sofistikované vyrovnávací paměti mezi rozhraní API služby a hello infrastrukturu správy, který bude ukládat a zpracovávat hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="092f5-129">hello Event Hub acts as a kind of sophisticated buffer between your API management service and hello infrastructure that will store and process hello messages.</span></span> <span data-ttu-id="092f5-130">Tím se zajistí, že nebude kvůli toohello protokolování infrastruktury sníží výkon vašich rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-130">This ensures that your API performance will not suffer due toohello logging infrastructure.</span></span>  

<span data-ttu-id="092f5-131">Jakmile hello dat byl předán tooan centra událostí je trvalé a bude čekat centra událostí příjemci tooprocess ho.</span><span class="sxs-lookup"><span data-stu-id="092f5-131">Once hello data has been passed tooan Event Hub it is persisted and will wait for Event Hub consumers tooprocess it.</span></span> <span data-ttu-id="092f5-132">Hello centra událostí nezáleží na tom, jak bude zpracována, ho záleží jenom tak, že úspěšně doručen uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="092f5-132">hello Event Hub does not care how it will be processed, it just cares about making sure hello message will be successfully delivered.</span></span>     

<span data-ttu-id="092f5-133">Služba Event Hubs mít hello možnost toostream události toomultiple skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="092f5-133">Event Hubs have hello ability toostream events toomultiple consumer groups.</span></span> <span data-ttu-id="092f5-134">To umožňuje toobe události, které jsou zpracovávány úplně jiné systémy.</span><span class="sxs-lookup"><span data-stu-id="092f5-134">This allows events toobe processed by completely different systems.</span></span> <span data-ttu-id="092f5-135">To umožňuje podporovat mnoho scénáře integrace bez uvedení Přidání zpoždění na hello zpracování požadavku rozhraní API hello v rámci služby API Management hello toobe generované stačit jenom jednu událost.</span><span class="sxs-lookup"><span data-stu-id="092f5-135">This enables supporting many integration scenarios without putting addition delays on hello processing of hello API request within hello API Management service as only one event needs toobe generated.</span></span>

## <a name="a-policy-toosend-applicationhttp-messages"></a><span data-ttu-id="092f5-136">Zprávy aplikace/http toosend zásad</span><span class="sxs-lookup"><span data-stu-id="092f5-136">A policy toosend application/http messages</span></span>
<span data-ttu-id="092f5-137">Centra událostí přijme data událostí jako jednoduchý řetězec.</span><span class="sxs-lookup"><span data-stu-id="092f5-137">An Event Hub accepts event data as a simple string.</span></span> <span data-ttu-id="092f5-138">obsah Hello tento řetězec je zcela až tooyou.</span><span class="sxs-lookup"><span data-stu-id="092f5-138">hello contents of that string are completely up tooyou.</span></span> <span data-ttu-id="092f5-139">možnost toopackage toobe až požadavku HTTP a odešlete ji vypnout tooEvent centra potřebujeme tooformat hello řetězec s informacemi o hello požadavku nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="092f5-139">toobe able toopackage up an HTTP request and send it off tooEvent Hubs we need tooformat hello string with hello request or response information.</span></span> <span data-ttu-id="092f5-140">V situacích, jako to zda je existujícího formátu, který můžeme opakovaně, pak nemusí je k dispozici toowrite vlastní analýza kódu.</span><span class="sxs-lookup"><span data-stu-id="092f5-140">In situations like this, if there is an existing format we can reuse, then we may not have toowrite our own parsing code.</span></span> <span data-ttu-id="092f5-141">Původně I považována za použití hello [HAR](http://www.softwareishard.com/blog/har-12-spec/) pro odesílání požadavků a odpovědí HTTP.</span><span class="sxs-lookup"><span data-stu-id="092f5-141">Initially I considered using hello [HAR](http://www.softwareishard.com/blog/har-12-spec/) for sending HTTP requests and responses.</span></span> <span data-ttu-id="092f5-142">Tento formát je však optimalizovaná pro ukládání pořadí požadavků HTTP ve formátu JSON na základě.</span><span class="sxs-lookup"><span data-stu-id="092f5-142">However, this format is optimized for storing a sequence of HTTP requests in a JSON based format.</span></span> <span data-ttu-id="092f5-143">Obsahuje povinné prvky, které přidány nepotřebné složitější scénář hello předávání zpráv hello HTTP přes přenosu hello.</span><span class="sxs-lookup"><span data-stu-id="092f5-143">It contained a number of mandatory elements that added unnecessary complexity for hello scenario of passing hello HTTP message over hello wire.</span></span>  

<span data-ttu-id="092f5-144">Alternativní možnost byla toouse hello `application/http` typ média, jak je popsáno ve specifikaci hello HTTP [RFC 7230](http://tools.ietf.org/html/rfc7230).</span><span class="sxs-lookup"><span data-stu-id="092f5-144">An alternative option was toouse hello `application/http` media type as described in hello HTTP specification [RFC 7230](http://tools.ietf.org/html/rfc7230).</span></span> <span data-ttu-id="092f5-145">Tento typ média používá hello přesně stejný formát tedy použité tooactually odesílat zprávy pomocí protokolu HTTP přes přenosu hello, ale můžou být přepnuté hello celé zprávy v textu hello jiné požadavku protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="092f5-145">This media type uses hello exact same format that is used tooactually send HTTP messages over hello wire, but hello entire message can be put in hello body of another HTTP request.</span></span> <span data-ttu-id="092f5-146">V našem případě právě přidáme textu hello toouse jako našem zpráv toosend tooEvent rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="092f5-146">In our case we are just going toouse hello body as our message toosend tooEvent Hubs.</span></span> <span data-ttu-id="092f5-147">Pohodlně, je analyzátor, který již existuje v [Microsoft ASP.NET Web API 2.2 klienta](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) knihovny, které můžete tento formát analyzovat a převádět je do nativní hello `HttpRequestMessage` a `HttpResponseMessage` objekty.</span><span class="sxs-lookup"><span data-stu-id="092f5-147">Conveniently, there is a parser that exists in [Microsoft ASP.NET Web API 2.2 Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) libraries that can parse this format and convert it into hello native `HttpRequestMessage` and `HttpResponseMessage` objects.</span></span>

<span data-ttu-id="092f5-148">možnost toocreate toobe tuto zprávu potřebujeme tootake výhody jazyka C# na základě [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx) v Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="092f5-148">toobe able toocreate this message we need tootake advantage of C# based [Policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management.</span></span> <span data-ttu-id="092f5-149">Zde je hello zásad, který odesílá tooAzure zpráva požadavku HTTP Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="092f5-149">Here is hello policy which sends a HTTP request message tooAzure Event Hubs.</span></span>

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

### <a name="policy-declaration"></a><span data-ttu-id="092f5-150">Zásady deklarace</span><span class="sxs-lookup"><span data-stu-id="092f5-150">Policy declaration</span></span>
<span data-ttu-id="092f5-151">Existuje několik věcí konkrétní důležité zmínit, o tomto výrazu zásad.</span><span class="sxs-lookup"><span data-stu-id="092f5-151">There a few particular things worth mentioning about this policy expression.</span></span> <span data-ttu-id="092f5-152">zásady protokolu eventhub Hello má atribut volá protokolovač id, které odkazuje toohello název protokolovacího nástroje, který byl vytvořen v rámci hello služba API Management.</span><span class="sxs-lookup"><span data-stu-id="092f5-152">hello log-to-eventhub policy has an attribute called logger-id which refers toohello name of logger that has been created within hello API Management service.</span></span> <span data-ttu-id="092f5-153">Podrobnosti o tom, jak toosetup protokolovač centra událostí ve službě API Management hello naleznete v dokumentu hello Hello [jak toolog události tooAzure Event Hubs ve službě Azure API Management](api-management-howto-log-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="092f5-153">hello details of how toosetup an Event Hub logger in hello API Management service can be found in hello document [How toolog events tooAzure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md).</span></span> <span data-ttu-id="092f5-154">druhý atribut Hello je volitelný parametr, který se dá pokyn centra událostí, které oddílu toostore uvítací zprávu v.</span><span class="sxs-lookup"><span data-stu-id="092f5-154">hello second attribute is an optional parameter that instructs Event Hubs which partition toostore hello message in.</span></span> <span data-ttu-id="092f5-155">Služba Event Hubs scalabilty tooenable oddíly a vyžadují minimálně dva.</span><span class="sxs-lookup"><span data-stu-id="092f5-155">Event Hubs use partitions tooenable scalabilty and require a minimum of two.</span></span> <span data-ttu-id="092f5-156">Hello seřazené doručení zpráv z jenom záruku, že se v rámci oddílu.</span><span class="sxs-lookup"><span data-stu-id="092f5-156">hello ordered delivery of messages is only guaranteed within a partition.</span></span> <span data-ttu-id="092f5-157">Pokud v které oddílu tooplace uvítací zprávu jsme vyzvat centra událostí, použije zatížení pomocí kruhového dotazování algoritmus toodistribute hello.</span><span class="sxs-lookup"><span data-stu-id="092f5-157">If we do not instruct Event Hub in which partition tooplace hello message, it will use a round-robin algorithm toodistribute hello load.</span></span> <span data-ttu-id="092f5-158">Však mohou způsobit některé z našich toobe zpráv zpracovaných mimo pořadí.</span><span class="sxs-lookup"><span data-stu-id="092f5-158">However, that may cause some of our messages toobe processed out of order.</span></span>  

### <a name="partitions"></a><span data-ttu-id="092f5-159">Oddíly</span><span class="sxs-lookup"><span data-stu-id="092f5-159">Partitions</span></span>
<span data-ttu-id="092f5-160">tooensure našem zpráv tooconsumers jsou dodávány v určitém pořadí a využívat možnosti distribuce zatížení hello oddílů, vybrali jste toosend HTTP žádosti o zprávy tooone oddíl a HTTP odpovědi zprávy tooa druhý oddíl.</span><span class="sxs-lookup"><span data-stu-id="092f5-160">tooensure our messages are delivered tooconsumers in order and take advantage of hello load distribution capability of partitions, I chose toosend HTTP request messages tooone partition and HTTP response messages tooa second partition.</span></span> <span data-ttu-id="092f5-161">Tím bude zajištěno jako distribučního i zatížení a jsme může zaručit, že všechny požadavky se budou v pořadí a všechny odpovědi se budou v pořadí.</span><span class="sxs-lookup"><span data-stu-id="092f5-161">This will ensure an even load distribution and we can guarantee that all requests will be consumed in order and all responses will be consumed in order.</span></span> <span data-ttu-id="092f5-162">Je možné, a odpověď toobe spotřebované před hello odpovídající požadavku, ale jako, který se nejedná o problém, protože máme jiný mechanismus pro korelace požadavky tooresponses a víme, že požadavky vždy dřívější než odpovědi.</span><span class="sxs-lookup"><span data-stu-id="092f5-162">It is possible for a response toobe consumed before hello corresponding request, but as that is not a problem as we have a different mechanism for correlating requests tooresponses and we know that requests always come before responses.</span></span>

### <a name="http-payloads"></a><span data-ttu-id="092f5-163">Datové části HTTP</span><span class="sxs-lookup"><span data-stu-id="092f5-163">HTTP payloads</span></span>
<span data-ttu-id="092f5-164">Po sestavení hello `requestLine` toosee jsme zkontrolujte, jestli by se zkrátila hello textu žádosti.</span><span class="sxs-lookup"><span data-stu-id="092f5-164">After building hello `requestLine` we check toosee if hello request body should be truncated.</span></span> <span data-ttu-id="092f5-165">text žádosti Hello je oříznuta tooonly 1024.</span><span class="sxs-lookup"><span data-stu-id="092f5-165">hello request body is truncated tooonly 1024.</span></span> <span data-ttu-id="092f5-166">To může být vyšší, ale jednotlivé zprávy centra událostí jsou omezené too256KB, takže je pravděpodobné, že některé zprávy HTTP subjekty se nevejdou do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="092f5-166">This could be increased, however individual Event Hub messages are limited too256KB, so it is likely that some HTTP message bodies will not fit in a single message.</span></span> <span data-ttu-id="092f5-167">Při provádění modulu protokolování a analýza významné množství informací může být odvozen od právě hello řádek požadavku HTTP a hlavičky.</span><span class="sxs-lookup"><span data-stu-id="092f5-167">When doing logging and analytics a significant amount of information can be derived from just hello HTTP request line and headers.</span></span> <span data-ttu-id="092f5-168">Navíc mnoho žádostí o rozhraní API vrátit pouze malé těla a tak ztrátě hello informační hodnotu zkrácením velké těla je poměrně minimální v porovnání toohello snížení přenos, zpracování a ukládání stojí tookeep veškerý obsah textu.</span><span class="sxs-lookup"><span data-stu-id="092f5-168">Also, many API requests only return small bodies and so hello loss of information value by truncating large bodies is fairly minimal in comparison toohello reduction in transfer, processing and storage costs tookeep all body contents.</span></span> <span data-ttu-id="092f5-169">Poslední OneNote o zpracování textu hello je, že potřebujeme toopass `true` toohello jako<string>– metoda () vzhledem k tomu, že jsme čtete obsah textu hello, avšak byla také chcete hello back-end rozhraní API toobe možné tooread hello textu.</span><span class="sxs-lookup"><span data-stu-id="092f5-169">One final note about processing hello body is that we need toopass `true` toohello As<string>() method because we are reading hello body contents, but was also want hello backend API toobe able tooread hello body.</span></span> <span data-ttu-id="092f5-170">Předáním true toothis metoda jsme způsobit toobe textu hello uložená do vyrovnávací paměti tak, aby ho mohou číst ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="092f5-170">By passing true toothis method we cause hello body toobe buffered so that it can be read a second time.</span></span> <span data-ttu-id="092f5-171">To je důležité vědět, pokud máte rozhraní API, který nemá odesílání velmi velké soubory nebo používá dlouhým dotazováním toobe.</span><span class="sxs-lookup"><span data-stu-id="092f5-171">This is important toobe aware of if you have an API that does uploading of very large files or uses long polling.</span></span> <span data-ttu-id="092f5-172">V těchto případech je nejlepší tooavoid čtení textu hello vůbec.</span><span class="sxs-lookup"><span data-stu-id="092f5-172">In these cases it would be best tooavoid reading hello body at all.</span></span>   

### <a name="http-headers"></a><span data-ttu-id="092f5-173">Hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="092f5-173">HTTP headers</span></span>
<span data-ttu-id="092f5-174">Hlavičky protokolu HTTP můžete jednoduše přenosu do formátu zprávy hello ve formátu pár klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="092f5-174">HTTP Headers can be simply transferred over into hello message format in a simple key/value pair format.</span></span> <span data-ttu-id="092f5-175">Rozhodli jsme toostrip na určité zabezpečení polí, tooavoid zbytečně úniku přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="092f5-175">We have chosen toostrip out certain security sensitive fields, tooavoid unnecessarily leaking credential information.</span></span> <span data-ttu-id="092f5-176">Není pravděpodobné, že klíče rozhraní API a jiná pověření budou použita pro účely analýzy.</span><span class="sxs-lookup"><span data-stu-id="092f5-176">It is unlikely that API keys and other credentials would be used for analytics purposes.</span></span> <span data-ttu-id="092f5-177">Pokud nám chcete toodo analýzy na hello uživatele a konkrétní produkt hello používají a které nám může získat z hello `context` objektu a přidejte toohello zprávy.</span><span class="sxs-lookup"><span data-stu-id="092f5-177">If we wish toodo analysis on hello user and hello particular product they are using then we could get that from hello `context` object and add that toohello message.</span></span>     

### <a name="message-metadata"></a><span data-ttu-id="092f5-178">Zpráva metadat</span><span class="sxs-lookup"><span data-stu-id="092f5-178">Message Metadata</span></span>
<span data-ttu-id="092f5-179">Při vytváření centra událostí hello dokončení zpráva toosend toohello, hello první řádek není ve skutečnosti součástí hello `application/http` zprávy.</span><span class="sxs-lookup"><span data-stu-id="092f5-179">When building hello complete message toosend toohello event hub, hello first line is not actually part of hello `application/http` message.</span></span> <span data-ttu-id="092f5-180">id zprávy, která je použité toocorrelate požadavky tooresponses Hello první řádek je další metadata, který se skládá z jestli hello zpráva je zprávou požadavku nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="092f5-180">hello first line is additional metadata consisting of whether hello message is a request or response message and a message id which is used toocorrelate requests tooresponses.</span></span> <span data-ttu-id="092f5-181">id zprávy Hello je vytvořená pomocí jiné zásady, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="092f5-181">hello message id is created by using another policy that looks like this:</span></span>

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

<span data-ttu-id="092f5-182">Může jsme vytvořili uvítací zprávu požadavku, uložené, do proměnné, dokud hello odpověď se vrátí a jednoduše odeslaný hello žádosti a odpovědi jako do jedné zprávy.</span><span class="sxs-lookup"><span data-stu-id="092f5-182">We could have created hello request message, stored that in a variable until hello response was returned and then simply sent hello request and response as a single message.</span></span> <span data-ttu-id="092f5-183">Odesílání hello žádostí a odpovědí nezávisle a použitím zprávu id toocorrelate však hello dva, se nám získat trochu další flexibilitu při hello velikost zprávy, hello možnost tootake využívat více oddílů a přitom zpráva pořadí a hello žádost se zobrazí v našich řídicí panel protokolování dříve.</span><span class="sxs-lookup"><span data-stu-id="092f5-183">However, by sending hello request and response independently and using a message id toocorrelate hello two, we get a bit more flexibility in hello message size, hello ability tootake advantage of multiple partitions whilst maintaining message order and hello request will appear in our logging dashboard sooner.</span></span> <span data-ttu-id="092f5-184">Také může být některých scénářích, kdy platnou odpověď, nikdy neodesílají toohello centra událostí, pravděpodobně z důvodu chyby závažná žádost tooa ve službě API Management hello, ale stále pomůžeme záznam hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="092f5-184">There also may be some scenarios where a valid response is never sent toohello event hub, possibly due tooa fatal request error in hello API Management service, but we still will have a record of hello request.</span></span>

<span data-ttu-id="092f5-185">zpráva odpovědi HTTP toosend hello Hello zásad vypadá velmi podobné toohello žádosti a aby hello dokončit konfiguraci zásad vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="092f5-185">hello policy toosend hello response HTTP message looks very similar toohello request and so hello complete policy configuration looks like this:</span></span>

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

<span data-ttu-id="092f5-186">Hello `set-variable` zásad vytvoří hodnotu, která je přístupná pomocí obou hello `log-to-eventhub` zásad v hello `<inbound>` části a hello `<outbound>` části.</span><span class="sxs-lookup"><span data-stu-id="092f5-186">hello `set-variable` policy creates a value that is accessible by both hello `log-to-eventhub` policy in hello `<inbound>` section and hello `<outbound>` section.</span></span>  

## <a name="receiving-events-from-event-hubs"></a><span data-ttu-id="092f5-187">Přijímání události ze služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="092f5-187">Receiving events from Event Hubs</span></span>
<span data-ttu-id="092f5-188">Přijetí události z centra událostí Azure pomocí hello [protokolu AMQP](http://www.amqp.org/).</span><span class="sxs-lookup"><span data-stu-id="092f5-188">Events from Azure Event Hub are received using hello [AMQP protocol](http://www.amqp.org/).</span></span> <span data-ttu-id="092f5-189">Tým Microsoft Service Bus Hello provedli klienta knihovny k dispozici toomake hello využívání události jednodušší.</span><span class="sxs-lookup"><span data-stu-id="092f5-189">hello Microsoft Service Bus team have made client libraries available toomake hello consuming events easier.</span></span> <span data-ttu-id="092f5-190">Existují dva různé přístupy, které jsou podporovány, jednu, která má být *přímý příjemce* a hello jiných používá hello `EventProcessorHost` třídy.</span><span class="sxs-lookup"><span data-stu-id="092f5-190">There are two different approaches supported, one is being a *Direct Consumer* and hello other is using hello `EventProcessorHost` class.</span></span> <span data-ttu-id="092f5-191">Příklady tyto dva přístupy lze nalézt v hello [Průvodce programováním centra událostí](../event-hubs/event-hubs-programming-guide.md).</span><span class="sxs-lookup"><span data-stu-id="092f5-191">Examples of these two approaches can be found in hello [Event Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).</span></span> <span data-ttu-id="092f5-192">Hello zkrácený rozdíly hello se `Direct Consumer` vám poskytuje úplnou kontrolu a hello `EventProcessorHost` nepodporuje některé hello vložení práce pro ale díky předem určité domněnky o tom, jak bude zpracovávat události.</span><span class="sxs-lookup"><span data-stu-id="092f5-192">hello short version of hello differences is, `Direct Consumer` gives you complete control and hello `EventProcessorHost` does some of hello plumbing work for you but makes certain assumptions about how you will process those events.</span></span>  

### <a name="eventprocessorhost"></a><span data-ttu-id="092f5-193">EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="092f5-193">EventProcessorHost</span></span>
<span data-ttu-id="092f5-194">V této ukázce použijeme hello `EventProcessorHost` pro jednoduchost, ale jeho nemusí hello nejlepší volbou pro tento konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="092f5-194">In this sample, we will use hello `EventProcessorHost` for simplicity, however it may not hello best choice for this particular scenario.</span></span> <span data-ttu-id="092f5-195">`EventProcessorHost`Ujistěte se, že nemáte tooworry o problémy v rámci třídy procesoru určitá událost dělení na vlákna náročné práce hello.</span><span class="sxs-lookup"><span data-stu-id="092f5-195">`EventProcessorHost` does hello hard work of making sure you don't have tooworry about threading issues within a particular event processor class.</span></span> <span data-ttu-id="092f5-196">Přesto však v tomto scénáři jsou jednoduše převádění formát tooanother zprávy hello a předání podél tooanother služby pomocí asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="092f5-196">However, in our scenario, we are simply converting hello message tooanother format and passing it along tooanother service using an async method.</span></span> <span data-ttu-id="092f5-197">Není nutné pro aktualizaci sdíleného stavu a proto žádné riziko problémy dělení na vlákna.</span><span class="sxs-lookup"><span data-stu-id="092f5-197">There is no need for updating shared state and therefore no risk of threading issues.</span></span> <span data-ttu-id="092f5-198">Pro většinu scénářů `EventProcessorHost` je pravděpodobně nejlepší volbou hello a je určitě jednodušší možnost hello.</span><span class="sxs-lookup"><span data-stu-id="092f5-198">For most scenarios, `EventProcessorHost` is probably hello best choice and it is certainly hello easier option.</span></span>     

### <a name="ieventprocessor"></a><span data-ttu-id="092f5-199">IEventProcessor</span><span class="sxs-lookup"><span data-stu-id="092f5-199">IEventProcessor</span></span>
<span data-ttu-id="092f5-200">centrální koncept Hello při použití `EventProcessorHost` je toocreate implementaci hello `IEventProcessor` rozhraní, které obsahuje metodu hello `ProcessEventAsync`.</span><span class="sxs-lookup"><span data-stu-id="092f5-200">hello central concept when using `EventProcessorHost` is toocreate a an implementation of hello `IEventProcessor` interface which contains hello method `ProcessEventAsync`.</span></span> <span data-ttu-id="092f5-201">Zobrazí se zde zásadní podpora Hello této metody:</span><span class="sxs-lookup"><span data-stu-id="092f5-201">hello essence of that method is shown here:</span></span>

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

<span data-ttu-id="092f5-202">Seznam objektů EventData se předávají do metody hello a jsme iterace v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="092f5-202">A list of EventData objects are passed into hello method and we iterate over that list.</span></span> <span data-ttu-id="092f5-203">Hello bajtů jednotlivých metod jsou analyzovány do HttpMessage objektu a tento objekt je předána tooan instanci IHttpMessageProcessor.</span><span class="sxs-lookup"><span data-stu-id="092f5-203">hello bytes of each method are parsed into a HttpMessage object and that object is passed tooan instance of IHttpMessageProcessor.</span></span>

### <a name="httpmessage"></a><span data-ttu-id="092f5-204">HttpMessage</span><span class="sxs-lookup"><span data-stu-id="092f5-204">HttpMessage</span></span>
<span data-ttu-id="092f5-205">Hello `HttpMessage` instance obsahuje tři druhy dat:</span><span class="sxs-lookup"><span data-stu-id="092f5-205">hello `HttpMessage` instance contains three pieces of data:</span></span>

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

<span data-ttu-id="092f5-206">Hello `HttpMessage` instance obsahuje `MessageId` identifikátor GUID, který umožňuje nám tooconnect hello HTTP žádost toohello odpovídající odpovědi HTTP a logickou hodnotu hodnotu, která označuje, jestli objekt hello obsahuje instanci třídy HttpRequestMessage a Objekt HttpResponseMessage.</span><span class="sxs-lookup"><span data-stu-id="092f5-206">hello `HttpMessage` instance contains a `MessageId` GUID that allows us tooconnect hello HTTP request toohello corresponding HTTP response and a boolean value that identifies if hello object contains an instance of a HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="092f5-207">Pomocí hello součástí třídy HTTP z `System.Net.Http`, bylo možné tootake výhod hello `application/http` analýza kódu, který je součástí `System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="092f5-207">By using hello built in HTTP classes from `System.Net.Http`, I was able tootake advantage of hello `application/http` parsing code that is included in `System.Net.Http.Formatting`.</span></span>  

### <a name="ihttpmessageprocessor"></a><span data-ttu-id="092f5-208">IHttpMessageProcessor</span><span class="sxs-lookup"><span data-stu-id="092f5-208">IHttpMessageProcessor</span></span>
<span data-ttu-id="092f5-209">Hello `HttpMessage` instance, předá tooimplementation z `IHttpMessageProcessor` což je rozhraní vytvořené toodecouple hello přijetí a výklad hello událostí z centra událostí Azure a hello skutečné jeho zpracování.</span><span class="sxs-lookup"><span data-stu-id="092f5-209">hello `HttpMessage` instance is then forwarded tooimplementation of `IHttpMessageProcessor` which is an interface I created toodecouple hello receiving and interpretation of hello event from Azure Event Hub and hello actual processing of it.</span></span>

## <a name="forwarding-hello-http-message"></a><span data-ttu-id="092f5-210">Předávání zpráv HTTP hello</span><span class="sxs-lookup"><span data-stu-id="092f5-210">Forwarding hello HTTP message</span></span>
<span data-ttu-id="092f5-211">Tato ukázka rozhodli je zajímavé toopush hello požadavku HTTP přes příliš[Runscope](http://www.runscope.com).</span><span class="sxs-lookup"><span data-stu-id="092f5-211">For this sample, I decided it would be interesting toopush hello HTTP Request over too[Runscope](http://www.runscope.com).</span></span> <span data-ttu-id="092f5-212">Runscope je cloudové služby, který se specializuje na protokolu HTTP, ladění, protokolování a monitorování.</span><span class="sxs-lookup"><span data-stu-id="092f5-212">Runscope is a cloud based service that specializes in HTTP debugging, logging and monitoring.</span></span> <span data-ttu-id="092f5-213">Mají volné vrstvy, takže je snadno tootry a umožňuje nám požadavků HTTP hello toosee v reálném čase předávaných mezi naše služba API Management.</span><span class="sxs-lookup"><span data-stu-id="092f5-213">They have a free tier, so it is easy tootry and it allows us toosee hello HTTP requests in real-time flowing through our API Management service.</span></span>

<span data-ttu-id="092f5-214">Hello `IHttpMessageProcessor` implementace vypadá to,</span><span class="sxs-lookup"><span data-stu-id="092f5-214">hello `IHttpMessageProcessor` implementation looks like this,</span></span>

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
       _Logger.LogDebug("Request sent tooRunscope");
   }
}
```

<span data-ttu-id="092f5-215">Bylo možné tootake výhod [existující klientské knihovny pro Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) umožňující snadno toopush `HttpRequestMessage` a `HttpResponseMessage` instance až do své služby.</span><span class="sxs-lookup"><span data-stu-id="092f5-215">I was able tootake advantage of an [existing client library for Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) that makes it easy toopush `HttpRequestMessage` and `HttpResponseMessage` instances up into their service.</span></span> <span data-ttu-id="092f5-216">V pořadí hello tooaccess Runscope API budete potřebovat účet a klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-216">In order tooaccess hello Runscope API you will need an account and an API Key.</span></span> <span data-ttu-id="092f5-217">Pokyny pro získání klíč rozhraní API naleznete v hello [tooAccess vytvoření aplikace Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) záznam dění na monitoru.</span><span class="sxs-lookup"><span data-stu-id="092f5-217">Instructions for getting an API key can be found in hello [Creating Applications tooAccess Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) screencast.</span></span>

## <a name="complete-sample"></a><span data-ttu-id="092f5-218">Ucelenou ukázku</span><span class="sxs-lookup"><span data-stu-id="092f5-218">Complete sample</span></span>
<span data-ttu-id="092f5-219">Hello [zdrojový kód](https://github.com/darrelmiller/ApimEventProcessor) a testy pro ukázku hello na Githubu.</span><span class="sxs-lookup"><span data-stu-id="092f5-219">hello [source code](https://github.com/darrelmiller/ApimEventProcessor) and tests for hello sample are on GitHub.</span></span> <span data-ttu-id="092f5-220">Budete potřebovat [služby API Management](api-management-get-started.md), [připojeného centra událostí](api-management-howto-log-event-hubs.md)a [účet úložiště](../storage/common/storage-create-storage-account.md) toorun hello ukázka sami.</span><span class="sxs-lookup"><span data-stu-id="092f5-220">You will need an [API Management Service](api-management-get-started.md), [a connected Event Hub](api-management-howto-log-event-hubs.md), and a [Storage Account](../storage/common/storage-create-storage-account.md) toorun hello sample for yourself.</span></span>   

<span data-ttu-id="092f5-221">Hello ukázka je stejně jednoduché konzolovou aplikaci, která naslouchá pro události pocházející z centra událostí, je do převede `HttpRequestMessage` a `HttpResponseMessage` objekty a předává je na toohello Runscope rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-221">hello sample is just a simple Console application that listens for events coming from Event Hub, converts them into a `HttpRequestMessage` and `HttpResponseMessage` objects and then forwards them on toohello Runscope API.</span></span>

<span data-ttu-id="092f5-222">V hello následující animovaný bitové kopie můžete zobrazit žádost o prováděné tooan rozhraní API v hello portál pro vývojáře, hello konzole aplikace zobrazuje hello zpráva přijímání, zpracování a předávat a pak hello žádostí a odpovědí zobrazovat na hello Runscope provoz Nástroj Inspector.</span><span class="sxs-lookup"><span data-stu-id="092f5-222">In hello following animated image, you can see a request being made tooan API in hello Developer Portal, hello Console application showing hello message being received, processed and forwarded and then hello request and response showing up in hello Runscope Traffic inspector.</span></span>

![Ukázka požadavku předávaná tooRunscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a><span data-ttu-id="092f5-224">Souhrn</span><span class="sxs-lookup"><span data-stu-id="092f5-224">Summary</span></span>
<span data-ttu-id="092f5-225">Služba Azure API Management poskytuje provoz hello HTTP toocapture ideální místo cestách tooand z vašich rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="092f5-225">Azure API Management service provides an ideal place toocapture hello HTTP traffic travelling tooand from your APIs.</span></span> <span data-ttu-id="092f5-226">Azure Event Hubs je vysoce škálovatelnou a nenákladné řešení pro zaznamenání tento přenos a vložené ho do sekundární zpracování dat pro protokolování, sledování a další pokročilé analýzy.</span><span class="sxs-lookup"><span data-stu-id="092f5-226">Azure Event Hubs is a highly scalable, low cost solution for capturing that traffic and feeding it into secondary processing systems for logging, monitoring and other sophisticated analytics.</span></span> <span data-ttu-id="092f5-227">Připojení too3rd strany provoz monitorování systémů jako Runscope je jednoduchý jako několik desítek řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="092f5-227">Connecting too3rd party traffic monitoring systems like Runscope is a simple as a few dozen lines of code.</span></span>

## <a name="next-steps"></a><span data-ttu-id="092f5-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="092f5-228">Next steps</span></span>
* <span data-ttu-id="092f5-229">Další informace o Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="092f5-229">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="092f5-230">Začínáme s Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="092f5-230">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="092f5-231">Přijímat zprávy pomocí třídy EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="092f5-231">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="092f5-232">Průvodce programováním pro službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="092f5-232">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="092f5-233">Další informace o integraci API Management a služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="092f5-233">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="092f5-234">Jak toolog události tooAzure Event Hubs ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="092f5-234">How toolog events tooAzure Event Hubs in Azure API Management</span></span>](api-management-howto-log-event-hubs.md)
  * [<span data-ttu-id="092f5-235">Odkaz na entitu protokolovacího nástroje</span><span class="sxs-lookup"><span data-stu-id="092f5-235">Logger entity reference</span></span>](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [<span data-ttu-id="092f5-236">referenční informace o protokolu eventhub zásad</span><span class="sxs-lookup"><span data-stu-id="092f5-236">log-to-eventhub policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
