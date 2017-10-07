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
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Sledovat vaše rozhraní API s Azure API Management, Event Hubs a Runscope
Hello [služba API Management](api-management-key-concepts.md) poskytuje tooenhance možnosti mnoho hello zpracování HTTP požadavky odeslané tooyour rozhraní API HTTP. Ale hello existenci hello požadavky a odpovědi jsou přechodný. Hello požadavku a ven prochází přes hello API Management service tooyour back-end rozhraní API. Rozhraní API zpracovává hello požadavek a odpověď zpět prochází příjemce toohello rozhraní API. Hello služba API Management udržuje některých důležitých statistik o hello rozhraní API pro zobrazení v hello řídicí panel portálu vydavatele, ale nad rámec, hello podrobnosti jsou pryč.

Pomocí hello [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [zásad](api-management-howto-policies.md) v hello služba API Management můžete odesílat žádné informace z hello žádostí a odpovědí tooan [centra událostí Azure](../event-hubs/event-hubs-what-is-event-hubs.md). Existuje mnoho různých důvodů, proč může být vhodné toogenerate události z protokolu HTTP zprávy odesílané tooyour rozhraní API. Mezi příklady patří záznam pro audit aktualizací, analýzy využití, výstrahy výjimek a integrace 3. stran.   

Tento článek ukazuje, jak toocapture hello celý žádosti a odpovědi zprávy HTTP, odešle tooan centra událostí a pak tuto zprávu tooa třetích stran služba, která poskytuje HTTP protokolování a monitorování služeb předávání.

## <a name="why-send-from-api-management-service"></a>Proč odeslat z služby API Management?
Je možné toowrite HTTP middleware, který můžete připojit k rozhraní API HTTP architektury toocapture požadavky a odpovědi HTTP a kanálu je do protokolování a monitorování systémů. Hello nevýhodou toothis přístup je hello HTTP middleware musí toobe integrována do back-end hello rozhraní API a hello platforma hello rozhraní API se musí shodovat. Pokud máte více rozhraní API, musíte nasadit každé z nich hello middleware. Často existuje několik důvodů, proč nelze aktualizovat back-end rozhraní API.

Pomocí protokolování infrastruktury toointegrate služby Azure API Management hello poskytuje centralizovaný a nezávislé na platformě řešení. Je také škálovatelná částečně kvůli toohello [geografická replikace](api-management-howto-deploy-multi-region.md) možnosti služby Azure API Management.

## <a name="why-send-tooan-azure-event-hub"></a>Proč odeslat tooan centra událostí Azure?
Je možné logicky tooask, proč vytvořit zásadu, která je konkrétní tooAzure Event Hubs? Existuje mnoho různých místech, kde může chci toolog Moje žádosti. Proč není právě odesílání hello požadavky přímo konečným cílem toohello?  To je možnost. Při provádění protokolování požadavky od služby API management, je však nutné tooconsider jak protokolování zpráv ovlivní výkon hello hello rozhraní API. Postupná nárůst zatížení lze zpracovávat zvýšením dostupných instancí komponent systému nebo přímým geografická replikace. Krátký špičky v provozu však může způsobit požadavky toobe výrazně zpožděno. Pokud požadavky toologging infrastruktury spustit tooslow zatížení.

Hello Azure Event Hubs je navrženou tooingress obrovské objemy dat, s kapacitou pro práci s daleko vyšší počet událostí, než hello počet HTTP požadavků většina proces rozhraní API. Hello centra událostí funguje jako sofistikované vyrovnávací paměti mezi rozhraní API služby a hello infrastrukturu správy, který bude ukládat a zpracovávat hello zprávy. Tím se zajistí, že nebude kvůli toohello protokolování infrastruktury sníží výkon vašich rozhraní API.  

Jakmile hello dat byl předán tooan centra událostí je trvalé a bude čekat centra událostí příjemci tooprocess ho. Hello centra událostí nezáleží na tom, jak bude zpracována, ho záleží jenom tak, že úspěšně doručen uvítací zprávu.     

Služba Event Hubs mít hello možnost toostream události toomultiple skupiny příjemců. To umožňuje toobe události, které jsou zpracovávány úplně jiné systémy. To umožňuje podporovat mnoho scénáře integrace bez uvedení Přidání zpoždění na hello zpracování požadavku rozhraní API hello v rámci služby API Management hello toobe generované stačit jenom jednu událost.

## <a name="a-policy-toosend-applicationhttp-messages"></a>Zprávy aplikace/http toosend zásad
Centra událostí přijme data událostí jako jednoduchý řetězec. obsah Hello tento řetězec je zcela až tooyou. možnost toopackage toobe až požadavku HTTP a odešlete ji vypnout tooEvent centra potřebujeme tooformat hello řetězec s informacemi o hello požadavku nebo odpovědi. V situacích, jako to zda je existujícího formátu, který můžeme opakovaně, pak nemusí je k dispozici toowrite vlastní analýza kódu. Původně I považována za použití hello [HAR](http://www.softwareishard.com/blog/har-12-spec/) pro odesílání požadavků a odpovědí HTTP. Tento formát je však optimalizovaná pro ukládání pořadí požadavků HTTP ve formátu JSON na základě. Obsahuje povinné prvky, které přidány nepotřebné složitější scénář hello předávání zpráv hello HTTP přes přenosu hello.  

Alternativní možnost byla toouse hello `application/http` typ média, jak je popsáno ve specifikaci hello HTTP [RFC 7230](http://tools.ietf.org/html/rfc7230). Tento typ média používá hello přesně stejný formát tedy použité tooactually odesílat zprávy pomocí protokolu HTTP přes přenosu hello, ale můžou být přepnuté hello celé zprávy v textu hello jiné požadavku protokolu HTTP. V našem případě právě přidáme textu hello toouse jako našem zpráv toosend tooEvent rozbočovače. Pohodlně, je analyzátor, který již existuje v [Microsoft ASP.NET Web API 2.2 klienta](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) knihovny, které můžete tento formát analyzovat a převádět je do nativní hello `HttpRequestMessage` a `HttpResponseMessage` objekty.

možnost toocreate toobe tuto zprávu potřebujeme tootake výhody jazyka C# na základě [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx) v Azure API Management. Zde je hello zásad, který odesílá tooAzure zpráva požadavku HTTP Event Hubs.

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

### <a name="policy-declaration"></a>Zásady deklarace
Existuje několik věcí konkrétní důležité zmínit, o tomto výrazu zásad. zásady protokolu eventhub Hello má atribut volá protokolovač id, které odkazuje toohello název protokolovacího nástroje, který byl vytvořen v rámci hello služba API Management. Podrobnosti o tom, jak toosetup protokolovač centra událostí ve službě API Management hello naleznete v dokumentu hello Hello [jak toolog události tooAzure Event Hubs ve službě Azure API Management](api-management-howto-log-event-hubs.md). druhý atribut Hello je volitelný parametr, který se dá pokyn centra událostí, které oddílu toostore uvítací zprávu v. Služba Event Hubs scalabilty tooenable oddíly a vyžadují minimálně dva. Hello seřazené doručení zpráv z jenom záruku, že se v rámci oddílu. Pokud v které oddílu tooplace uvítací zprávu jsme vyzvat centra událostí, použije zatížení pomocí kruhového dotazování algoritmus toodistribute hello. Však mohou způsobit některé z našich toobe zpráv zpracovaných mimo pořadí.  

### <a name="partitions"></a>Oddíly
tooensure našem zpráv tooconsumers jsou dodávány v určitém pořadí a využívat možnosti distribuce zatížení hello oddílů, vybrali jste toosend HTTP žádosti o zprávy tooone oddíl a HTTP odpovědi zprávy tooa druhý oddíl. Tím bude zajištěno jako distribučního i zatížení a jsme může zaručit, že všechny požadavky se budou v pořadí a všechny odpovědi se budou v pořadí. Je možné, a odpověď toobe spotřebované před hello odpovídající požadavku, ale jako, který se nejedná o problém, protože máme jiný mechanismus pro korelace požadavky tooresponses a víme, že požadavky vždy dřívější než odpovědi.

### <a name="http-payloads"></a>Datové části HTTP
Po sestavení hello `requestLine` toosee jsme zkontrolujte, jestli by se zkrátila hello textu žádosti. text žádosti Hello je oříznuta tooonly 1024. To může být vyšší, ale jednotlivé zprávy centra událostí jsou omezené too256KB, takže je pravděpodobné, že některé zprávy HTTP subjekty se nevejdou do jedné zprávy. Při provádění modulu protokolování a analýza významné množství informací může být odvozen od právě hello řádek požadavku HTTP a hlavičky. Navíc mnoho žádostí o rozhraní API vrátit pouze malé těla a tak ztrátě hello informační hodnotu zkrácením velké těla je poměrně minimální v porovnání toohello snížení přenos, zpracování a ukládání stojí tookeep veškerý obsah textu. Poslední OneNote o zpracování textu hello je, že potřebujeme toopass `true` toohello jako<string>– metoda () vzhledem k tomu, že jsme čtete obsah textu hello, avšak byla také chcete hello back-end rozhraní API toobe možné tooread hello textu. Předáním true toothis metoda jsme způsobit toobe textu hello uložená do vyrovnávací paměti tak, aby ho mohou číst ještě jednou. To je důležité vědět, pokud máte rozhraní API, který nemá odesílání velmi velké soubory nebo používá dlouhým dotazováním toobe. V těchto případech je nejlepší tooavoid čtení textu hello vůbec.   

### <a name="http-headers"></a>Hlavičky protokolu HTTP
Hlavičky protokolu HTTP můžete jednoduše přenosu do formátu zprávy hello ve formátu pár klíč hodnota. Rozhodli jsme toostrip na určité zabezpečení polí, tooavoid zbytečně úniku přihlašovacích údajů. Není pravděpodobné, že klíče rozhraní API a jiná pověření budou použita pro účely analýzy. Pokud nám chcete toodo analýzy na hello uživatele a konkrétní produkt hello používají a které nám může získat z hello `context` objektu a přidejte toohello zprávy.     

### <a name="message-metadata"></a>Zpráva metadat
Při vytváření centra událostí hello dokončení zpráva toosend toohello, hello první řádek není ve skutečnosti součástí hello `application/http` zprávy. id zprávy, která je použité toocorrelate požadavky tooresponses Hello první řádek je další metadata, který se skládá z jestli hello zpráva je zprávou požadavku nebo odpovědi. id zprávy Hello je vytvořená pomocí jiné zásady, které vypadá takto:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Může jsme vytvořili uvítací zprávu požadavku, uložené, do proměnné, dokud hello odpověď se vrátí a jednoduše odeslaný hello žádosti a odpovědi jako do jedné zprávy. Odesílání hello žádostí a odpovědí nezávisle a použitím zprávu id toocorrelate však hello dva, se nám získat trochu další flexibilitu při hello velikost zprávy, hello možnost tootake využívat více oddílů a přitom zpráva pořadí a hello žádost se zobrazí v našich řídicí panel protokolování dříve. Také může být některých scénářích, kdy platnou odpověď, nikdy neodesílají toohello centra událostí, pravděpodobně z důvodu chyby závažná žádost tooa ve službě API Management hello, ale stále pomůžeme záznam hello požadavku.

zpráva odpovědi HTTP toosend hello Hello zásad vypadá velmi podobné toohello žádosti a aby hello dokončit konfiguraci zásad vypadá takto:

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

Hello `set-variable` zásad vytvoří hodnotu, která je přístupná pomocí obou hello `log-to-eventhub` zásad v hello `<inbound>` části a hello `<outbound>` části.  

## <a name="receiving-events-from-event-hubs"></a>Přijímání události ze služby Event Hubs
Přijetí události z centra událostí Azure pomocí hello [protokolu AMQP](http://www.amqp.org/). Tým Microsoft Service Bus Hello provedli klienta knihovny k dispozici toomake hello využívání události jednodušší. Existují dva různé přístupy, které jsou podporovány, jednu, která má být *přímý příjemce* a hello jiných používá hello `EventProcessorHost` třídy. Příklady tyto dva přístupy lze nalézt v hello [Průvodce programováním centra událostí](../event-hubs/event-hubs-programming-guide.md). Hello zkrácený rozdíly hello se `Direct Consumer` vám poskytuje úplnou kontrolu a hello `EventProcessorHost` nepodporuje některé hello vložení práce pro ale díky předem určité domněnky o tom, jak bude zpracovávat události.  

### <a name="eventprocessorhost"></a>EventProcessorHost
V této ukázce použijeme hello `EventProcessorHost` pro jednoduchost, ale jeho nemusí hello nejlepší volbou pro tento konkrétní scénář. `EventProcessorHost`Ujistěte se, že nemáte tooworry o problémy v rámci třídy procesoru určitá událost dělení na vlákna náročné práce hello. Přesto však v tomto scénáři jsou jednoduše převádění formát tooanother zprávy hello a předání podél tooanother služby pomocí asynchronní metody. Není nutné pro aktualizaci sdíleného stavu a proto žádné riziko problémy dělení na vlákna. Pro většinu scénářů `EventProcessorHost` je pravděpodobně nejlepší volbou hello a je určitě jednodušší možnost hello.     

### <a name="ieventprocessor"></a>IEventProcessor
centrální koncept Hello při použití `EventProcessorHost` je toocreate implementaci hello `IEventProcessor` rozhraní, které obsahuje metodu hello `ProcessEventAsync`. Zobrazí se zde zásadní podpora Hello této metody:

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

Seznam objektů EventData se předávají do metody hello a jsme iterace v tomto seznamu. Hello bajtů jednotlivých metod jsou analyzovány do HttpMessage objektu a tento objekt je předána tooan instanci IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
Hello `HttpMessage` instance obsahuje tři druhy dat:

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

Hello `HttpMessage` instance obsahuje `MessageId` identifikátor GUID, který umožňuje nám tooconnect hello HTTP žádost toohello odpovídající odpovědi HTTP a logickou hodnotu hodnotu, která označuje, jestli objekt hello obsahuje instanci třídy HttpRequestMessage a Objekt HttpResponseMessage. Pomocí hello součástí třídy HTTP z `System.Net.Http`, bylo možné tootake výhod hello `application/http` analýza kódu, který je součástí `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
Hello `HttpMessage` instance, předá tooimplementation z `IHttpMessageProcessor` což je rozhraní vytvořené toodecouple hello přijetí a výklad hello událostí z centra událostí Azure a hello skutečné jeho zpracování.

## <a name="forwarding-hello-http-message"></a>Předávání zpráv HTTP hello
Tato ukázka rozhodli je zajímavé toopush hello požadavku HTTP přes příliš[Runscope](http://www.runscope.com). Runscope je cloudové služby, který se specializuje na protokolu HTTP, ladění, protokolování a monitorování. Mají volné vrstvy, takže je snadno tootry a umožňuje nám požadavků HTTP hello toosee v reálném čase předávaných mezi naše služba API Management.

Hello `IHttpMessageProcessor` implementace vypadá to,

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

Bylo možné tootake výhod [existující klientské knihovny pro Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) umožňující snadno toopush `HttpRequestMessage` a `HttpResponseMessage` instance až do své služby. V pořadí hello tooaccess Runscope API budete potřebovat účet a klíč rozhraní API. Pokyny pro získání klíč rozhraní API naleznete v hello [tooAccess vytvoření aplikace Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) záznam dění na monitoru.

## <a name="complete-sample"></a>Ucelenou ukázku
Hello [zdrojový kód](https://github.com/darrelmiller/ApimEventProcessor) a testy pro ukázku hello na Githubu. Budete potřebovat [služby API Management](api-management-get-started.md), [připojeného centra událostí](api-management-howto-log-event-hubs.md)a [účet úložiště](../storage/common/storage-create-storage-account.md) toorun hello ukázka sami.   

Hello ukázka je stejně jednoduché konzolovou aplikaci, která naslouchá pro události pocházející z centra událostí, je do převede `HttpRequestMessage` a `HttpResponseMessage` objekty a předává je na toohello Runscope rozhraní API.

V hello následující animovaný bitové kopie můžete zobrazit žádost o prováděné tooan rozhraní API v hello portál pro vývojáře, hello konzole aplikace zobrazuje hello zpráva přijímání, zpracování a předávat a pak hello žádostí a odpovědí zobrazovat na hello Runscope provoz Nástroj Inspector.

![Ukázka požadavku předávaná tooRunscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Souhrn
Služba Azure API Management poskytuje provoz hello HTTP toocapture ideální místo cestách tooand z vašich rozhraní API. Azure Event Hubs je vysoce škálovatelnou a nenákladné řešení pro zaznamenání tento přenos a vložené ho do sekundární zpracování dat pro protokolování, sledování a další pokročilé analýzy. Připojení too3rd strany provoz monitorování systémů jako Runscope je jednoduchý jako několik desítek řádků kódu.

## <a name="next-steps"></a>Další kroky
* Další informace o Azure Event Hubs
  * [Začínáme s Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Přijímat zprávy pomocí třídy EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Průvodce programováním pro službu Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Další informace o integraci API Management a služby Event Hubs
  * [Jak toolog události tooAzure Event Hubs ve službě Azure API Management](api-management-howto-log-event-hubs.md)
  * [Odkaz na entitu protokolovacího nástroje](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [referenční informace o protokolu eventhub zásad](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
