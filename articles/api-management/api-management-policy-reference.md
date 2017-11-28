---
title: "aaaAzure zásady API managementu"
description: "Další informace o dostupných tooconfigure hello zásady API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7ee194f4d6f172bf836c789cbe8ab732e18a7e0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-policy-reference"></a><span data-ttu-id="db3ce-103">Zásady Azure API managementu</span><span class="sxs-lookup"><span data-stu-id="db3ce-103">Azure API Management Policy Reference</span></span>
<span data-ttu-id="db3ce-104">Tato část obsahuje index pro zásady hello v hello [zásady API managementu][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="db3ce-104">This section provides an index for hello policies in hello [API Management policy reference][API Management policy reference].</span></span> <span data-ttu-id="db3ce-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management][Policies in API Management].</span><span class="sxs-lookup"><span data-stu-id="db3ce-105">For information on adding and configuring policies, see [Policies in API Management][Policies in API Management].</span></span>

<span data-ttu-id="db3ce-106">Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných hello zásad služby API Management, pokud hello zásady neurčí jinak.</span><span class="sxs-lookup"><span data-stu-id="db3ce-106">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="db3ce-107">Některé zásady, jako je například hello [řízení toku] [ Control flow] a [nastavená proměnná] [ Set variable] jsou založené na výrazech zásad.</span><span class="sxs-lookup"><span data-stu-id="db3ce-107">Some policies such as hello [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="db3ce-108">Další informace najdete v tématu [pokročilé zásady] [ Advanced policies] a [výrazy zásad][Policy expressions]</span><span class="sxs-lookup"><span data-stu-id="db3ce-108">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions]</span></span>

## <a name="policy-reference-index"></a><span data-ttu-id="db3ce-109">Rejstřík referenčních informací o zásadách</span><span class="sxs-lookup"><span data-stu-id="db3ce-109">Policy reference index</span></span>
* <span data-ttu-id="db3ce-110">[Zásady omezení přístupu][Access restriction policies]</span><span class="sxs-lookup"><span data-stu-id="db3ce-110">[Access restriction policies][Access restriction policies]</span></span>
  * <span data-ttu-id="db3ce-111">[Kontrola HTTP záhlaví] [ Check HTTP header] -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db3ce-111">[Check HTTP header][Check HTTP header] - Enforces existence and/or value of a HTTP Header.</span></span>
  * <span data-ttu-id="db3ce-112">[Omezení četnosti volání podle předplatného] [ Limit call rate by subscription] -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="db3ce-112">[Limit call rate by subscription][Limit call rate by subscription] - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>
  * <span data-ttu-id="db3ce-113">[Omezení četnosti volání podle klíče](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="db3ce-113">[Limit call rate by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>
  * <span data-ttu-id="db3ce-114">[Omezit volající IP adresy] [ Restrict caller IPs] -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="db3ce-114">[Restrict caller IPs][Restrict caller IPs] - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>
  * <span data-ttu-id="db3ce-115">[Nastavení kvóty využití podle předplatného] [ Set usage quota by subscription] -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="db3ce-115">[Set usage quota by subscription][Set usage quota by subscription] - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>
  * <span data-ttu-id="db3ce-116">[Nastavení kvóty využití podle klíče](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="db3ce-116">[Set usage quota by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>
  * <span data-ttu-id="db3ce-117">[Ověření JWT] [ Validate JWT] -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="db3ce-117">[Validate JWT][Validate JWT] - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>
* <span data-ttu-id="db3ce-118">[Rozšířené zásady][Advanced policies]</span><span class="sxs-lookup"><span data-stu-id="db3ce-118">[Advanced policies][Advanced policies]</span></span>
  * <span data-ttu-id="db3ce-119">[Řízení toku] [ Control flow] – podmíněně platí příkazy zásad na základě výsledků hello hello vyhodnocení logická hodnota [výrazy][expressions].</span><span class="sxs-lookup"><span data-stu-id="db3ce-119">[Control flow][Control flow] - Conditionally applies policy statements based on hello results of hello evaluation of Boolean [expressions][expressions].</span></span>
  * <span data-ttu-id="db3ce-120">[Předat dál žádost] [ Forward request] -předává hello požadavek toohello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="db3ce-120">[Forward request][Forward request] - Forwards hello request toohello backend service.</span></span>
  * <span data-ttu-id="db3ce-121">[Protokolu tooEvent rozbočovače] [ Log tooEvent Hub] -odešle zprávy v hello zadaný formát tooa cíl zprávy definované [Protokolovač](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.</span><span class="sxs-lookup"><span data-stu-id="db3ce-121">[Log tooEvent Hub][Log tooEvent Hub] - Sends messages in hello specified format tooa message target defined by a [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.</span></span>
  * <span data-ttu-id="db3ce-122">[Opakujte](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -provádění opakování hello uzavřené příkazy zásad, pokud a dokud je splněna podmínka hello.</span><span class="sxs-lookup"><span data-stu-id="db3ce-122">[Retry](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="db3ce-123">Spuštění se bude opakovat v hello zadaným časovým intervalům a až toohello zadaný počet opakování.</span><span class="sxs-lookup"><span data-stu-id="db3ce-123">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>
  * <span data-ttu-id="db3ce-124">[Vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí hello zadané odpovědi přímo toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="db3ce-124">[Return response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span>
  * <span data-ttu-id="db3ce-125">[Odeslání žádosti o jednorázové jednoduché](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -odešle žádost toohello zadané adresy URL bez čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="db3ce-125">[Send one way request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>
  * <span data-ttu-id="db3ce-126">[Odeslání požadavku](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -odešle žádost toohello zadaná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="db3ce-126">[Send request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - Sends a request toohello specified URL.</span></span>
  * <span data-ttu-id="db3ce-127">[Nastaví metodu požadavku](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -vám umožní toochange hello HTTP metoda pro žádost.</span><span class="sxs-lookup"><span data-stu-id="db3ce-127">[Set request method](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>
  * <span data-ttu-id="db3ce-128">[Nastavte stav](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -toohello kód stavu hello HTTP změny zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="db3ce-128">[Set status](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>
  * <span data-ttu-id="db3ce-129">[Nastavená proměnná] [ Set variable] -zachovat hodnotu v pojmenovaná [kontextu] [ context] proměnná pro pozdější přístup.</span><span class="sxs-lookup"><span data-stu-id="db3ce-129">[Set variable][Set variable] - Persist a value in a named [context][context] variable for later access.</span></span>
  * <span data-ttu-id="db3ce-130">[Trasování](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -přidá řetězec do hello [rozhraní API Inspector](api-management-howto-api-inspector.md) výstup.</span><span class="sxs-lookup"><span data-stu-id="db3ce-130">[Trace](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - Adds a string into hello [API Inspector](api-management-howto-api-inspector.md) output.</span></span>
  * <span data-ttu-id="db3ce-131">[Počkejte](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) – čeká pro závorkách odesílání žádostí, získat hodnotu z mezipaměti, nebo řízení toku zásady toocomplete než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="db3ce-131">[Wait](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - Waits for enclosed Send request, Get value from cache, or Control flow policies toocomplete before proceeding.</span></span>
* <span data-ttu-id="db3ce-132">[Zásady ověřování][Authentication policies]</span><span class="sxs-lookup"><span data-stu-id="db3ce-132">[Authentication policies][Authentication policies]</span></span>
  * <span data-ttu-id="db3ce-133">[Ověřování Basic] [ Authenticate with Basic] -ověření pomocí back-end službu pomocí základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="db3ce-133">[Authenticate with Basic][Authenticate with Basic] - Authenticate with a backend service using Basic authentication.</span></span>
  * <span data-ttu-id="db3ce-134">[Ověřování pomocí certifikátu klienta] [ Authenticate with client certificate] -ověření pomocí back-end službu pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="db3ce-134">[Authenticate with client certificate][Authenticate with client certificate] - Authenticate with a backend service using client certificates.</span></span>
* <span data-ttu-id="db3ce-135">[Zásady ukládání do mezipaměti][Caching policies]</span><span class="sxs-lookup"><span data-stu-id="db3ce-135">[Caching policies][Caching policies]</span></span> 
  * <span data-ttu-id="db3ce-136">[Získat z mezipaměti] [ Get from cache] -provést mezipaměti vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="db3ce-136">[Get from cache][Get from cache] - Perform cache look up and return a valid cached response when available.</span></span>
  * <span data-ttu-id="db3ce-137">[Uložení toocache] [ Store toocache] -mezipamětí odpověď podle toohello uvedená konfigurace mezipaměti ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="db3ce-137">[Store toocache][Store toocache] - Caches response according toohello specified cache control configuration.</span></span>
  * <span data-ttu-id="db3ce-138">[Získat hodnotu z mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="db3ce-138">[Get value from cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>
  * <span data-ttu-id="db3ce-139">[Uložení v mezipaměti hodnoty](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -uložit položku hello mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="db3ce-139">[Store value in cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>
  * <span data-ttu-id="db3ce-140">[Odebrat hodnotu z mezipaměti](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -odebrání položky v mezipaměti hello podle klíče.</span><span class="sxs-lookup"><span data-stu-id="db3ce-140">[Remove value from cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>
* <span data-ttu-id="db3ce-141">[Různé zásady domény][Cross domain policies]</span><span class="sxs-lookup"><span data-stu-id="db3ce-141">[Cross domain policies][Cross domain policies]</span></span> 
  * <span data-ttu-id="db3ce-142">[Povolit volání mezi doménami] [ Allow cross-domain calls] -zpřístupní hello rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="db3ce-142">[Allow cross-domain calls][Allow cross-domain calls] - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>
  * <span data-ttu-id="db3ce-143">[CORS] [ CORS] -přidá sdílení prostředků různých původů (CORS) podporoval tooan operaci nebo volání rozhraní API tooallow napříč doménami založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="db3ce-143">[CORS][CORS] - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>
  * <span data-ttu-id="db3ce-144">[JSONP] [ JSONP] – přidá JSON s odsazení (JSONP) operaci tooan podpory nebo rozhraní API tooallow napříč doménami volá z klientů JavaScript založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="db3ce-144">[JSONP][JSONP] - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>
* <span data-ttu-id="db3ce-145">[Zásad transformace][Transformation policies]</span><span class="sxs-lookup"><span data-stu-id="db3ce-145">[Transformation policies][Transformation policies]</span></span> 
  * <span data-ttu-id="db3ce-146">[Převést JSON tooXML] [ Convert JSON tooXML] – převede požadavku nebo odpovědi textu z JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="db3ce-146">[Convert JSON tooXML][Convert JSON tooXML] - Converts request or response body from JSON tooXML.</span></span>
  * <span data-ttu-id="db3ce-147">[Převod XML tooJSON] [ Convert XML tooJSON] – převede požadavku nebo odpovědi textu z XML tooJSON.</span><span class="sxs-lookup"><span data-stu-id="db3ce-147">[Convert XML tooJSON][Convert XML tooJSON] - Converts request or response body from XML tooJSON.</span></span>
  * <span data-ttu-id="db3ce-148">[Najít a nahradit řetězec v textu] [ Find and replace string in body] – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="db3ce-148">[Find and replace string in body][Find and replace string in body] - Finds a request or response substring and replaces it with a different substring.</span></span>
  * <span data-ttu-id="db3ce-149">[Maskování adresy URL v obsahu] [ Mask URLs in content] -znovu zapíše (masky) odkazy v odpovědi hello body tak, aby ukazovaly toohello ekvivalentní propojení prostřednictvím brány hello.</span><span class="sxs-lookup"><span data-stu-id="db3ce-149">[Mask URLs in content][Mask URLs in content] - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>
  * <span data-ttu-id="db3ce-150">[Nastavení back-end služby] [ Set backend service] -změny hello back-end službu pro příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="db3ce-150">[Set backend service][Set backend service] - Changes hello backend service for an incoming request.</span></span>
  * <span data-ttu-id="db3ce-151">[Tělo nastavit] [ Set body] -nastaví hello text zprávy pro příchozí a odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="db3ce-151">[Set body][Set body] - Sets hello message body for incoming and outgoing requests.</span></span>
  * <span data-ttu-id="db3ce-152">[Set – hlavička protokolu HTTP] [ Set HTTP header] – přiřadí hodnota tooan existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.</span><span class="sxs-lookup"><span data-stu-id="db3ce-152">[Set HTTP header][Set HTTP header] - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>
  * <span data-ttu-id="db3ce-153">[Nastavte parametr řetězce dotazu] [ Set query string parameter] – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.</span><span class="sxs-lookup"><span data-stu-id="db3ce-153">[Set query string parameter][Set query string parameter] - Adds, replaces value of, or deletes request query string parameter.</span></span>
  * <span data-ttu-id="db3ce-154">[Přepsání adresy URL] [ Rewrite URL] – převede adresu URL požadavku z formuláře toohello jeho veřejné formuláře očekávanou hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="db3ce-154">[Rewrite URL][Rewrite URL] - Converts a request URL from its public form toohello form expected by hello web service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db3ce-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db3ce-155">Next steps</span></span>
<span data-ttu-id="db3ce-156">Další informace o výrazů zásad najdete v tématu hello následující videa.</span><span class="sxs-lookup"><span data-stu-id="db3ce-156">For more information on policy expressions, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log tooEvent Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store toocache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON tooXML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML tooJSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx


