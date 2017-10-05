---
title: "Zásady služby Azure API Management | Microsoft Docs"
description: "Další informace o zásady, které jsou k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 1cc460cb-8e67-41aa-bc76-bbafc1892798
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 485dc3a87a81dc67f5144596a30d498293d6b76a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-policies"></a><span data-ttu-id="7f7d3-103">Zásady služby API Management</span><span class="sxs-lookup"><span data-stu-id="7f7d3-103">API Management policies</span></span>
<span data-ttu-id="7f7d3-104">Tato část obsahuje odkaz pro následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-104">This section provides a reference for the following API Management policies.</span></span> <span data-ttu-id="7f7d3-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="7f7d3-105">For information on adding and configuring policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
  
 <span data-ttu-id="7f7d3-106">Zásady jsou vynikající funkcí systému, která vydavatelům umožňuje měnit chování rozhraní API prostřednictvím konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-106">Policies are a powerful capability of the system that allow the publisher to change the behavior of the API through configuration.</span></span> <span data-ttu-id="7f7d3-107">Zásady představují kolekci příkazů, které jsou prováděny postupně v požadavku nebo odezvy z rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-107">Policies are a collection of Statements that are executed sequentially on the request or response of an API.</span></span> <span data-ttu-id="7f7d3-108">Oblíbené příkazy patří převod formátu XML do formátu JSON a četnosti omezení omezit množství příchozích volání od vývojáře.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-108">Popular Statements include format conversion from XML to JSON and call rate limiting to restrict the amount of incoming calls from a developer.</span></span> <span data-ttu-id="7f7d3-109">Jsou dostupné ihned mnoho další zásady.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-109">Many more policies are available out of the box.</span></span>  
  
 <span data-ttu-id="7f7d3-110">Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných zásadách API Management (pokud zásady neurčí jinak).</span><span class="sxs-lookup"><span data-stu-id="7f7d3-110">Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise.</span></span> <span data-ttu-id="7f7d3-111">Některé zásady, například [řízení toku](api-management-advanced-policies.md#choose) a [nastavená proměnná](api-management-advanced-policies.md#set-variable), jsou založené na výrazech zásad.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-111">Some policies such as the [Control flow](api-management-advanced-policies.md#choose) and [Set variable](api-management-advanced-policies.md#set-variable) policies are based on policy expressions.</span></span> <span data-ttu-id="7f7d3-112">Další informace najdete v tématu [pokročilé zásady](api-management-advanced-policies.md#AdvancedPolicies) a [výrazy zásad](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="7f7d3-112">For more information, see [Advanced policies](api-management-advanced-policies.md#AdvancedPolicies) and [Policy expressions](api-management-policy-expressions.md).</span></span>  
  
##  <span data-ttu-id="7f7d3-113"><a name="ProxyPolicies"></a>Zásady</span><span class="sxs-lookup"><span data-stu-id="7f7d3-113"><a name="ProxyPolicies"></a> Policies</span></span>  
  
-   [<span data-ttu-id="7f7d3-114">Zásady omezení přístupu</span><span class="sxs-lookup"><span data-stu-id="7f7d3-114">Access restriction policies</span></span>](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
  
    -   <span data-ttu-id="7f7d3-115">[Kontrola HTTP záhlaví](api-management-access-restriction-policies.md#CheckHTTPHeader) -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-115">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
    -   <span data-ttu-id="7f7d3-116">[Omezení četnosti volání podle předplatného](api-management-access-restriction-policies.md#LimitCallRate) -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-116">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="7f7d3-117">[Omezení četnosti volání podle klíče](api-management-access-restriction-policies.md#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-117">[Limit call rate by key](api-management-access-restriction-policies.md#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="7f7d3-118">[Omezit volající IP adresy](api-management-access-restriction-policies.md#RestrictCallerIPs) -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-118">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
    -   <span data-ttu-id="7f7d3-119">[Nastavení kvóty využití podle předplatného](api-management-access-restriction-policies.md#SetUsageQuota) -umožňuje vynucovat obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-119">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="7f7d3-120">[Nastavení kvóty využití podle klíče](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -umožňuje vynucovat obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-120">[Set usage quota by key](api-management-access-restriction-policies.md#SetUsageQuotaByKey) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="7f7d3-121">[Ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-121">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
-   [<span data-ttu-id="7f7d3-122">Pokročilé zásady</span><span class="sxs-lookup"><span data-stu-id="7f7d3-122">Advanced policies</span></span>](api-management-advanced-policies.md#AdvancedPolicies)  
  
    -   <span data-ttu-id="7f7d3-123">[Řízení toku](api-management-advanced-policies.md#choose) – podmíněně platí příkazy zásad na základě vyhodnocení výrazy logických hodnot.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-123">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on the evaluation of Boolean expressions.</span></span>  
  
    -   <span data-ttu-id="7f7d3-124">[Předat dál žádost](api-management-advanced-policies.md#ForwardRequest) -předá požadavek back-end službu.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-124">[Forward request](api-management-advanced-policies.md#ForwardRequest) - Forwards the request to the backend service.</span></span>  
  
    -   <span data-ttu-id="7f7d3-125">[Protokol do centra událostí](api-management-advanced-policies.md#log-to-eventhub) -odešle zprávy v zadaném formátu do zprávy Cíl definované entity protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-125">[Log to Event Hub](api-management-advanced-policies.md#log-to-eventhub) - Sends messages in the specified format to a message target defined by a Logger entity.</span></span>  
  
    -   <span data-ttu-id="7f7d3-126">[Opakujte](api-management-advanced-policies.md#Retry) -opakování provádění příkazů závorkách zásad, pokud a dokud nebude splněna podmínka.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-126">[Retry](api-management-advanced-policies.md#Retry) - Retries execution of the enclosed policy statements, if and until the condition is met.</span></span> <span data-ttu-id="7f7d3-127">Spuštění se opakovaly zadaným časovým intervalům a až zadaný počet opakování.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-127">Execution will repeat at the specified time intervals and up to the specified retry count.</span></span>  
  
    -   <span data-ttu-id="7f7d3-128">[Vrátí odpověď](api-management-advanced-policies.md#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí zadanou odpověď přímo na volajícího.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-128">[Return response](api-management-advanced-policies.md#ReturnResponse) - Aborts pipeline execution and returns the specified response directly to the caller.</span></span>  
  
    -   <span data-ttu-id="7f7d3-129">[Odeslání žádosti o jednorázové jednoduché](api-management-advanced-policies.md#SendOneWayRequest) -odešle požadavek na zadanou adresu URL bez čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-129">[Send one way request](api-management-advanced-policies.md#SendOneWayRequest) - Sends a request to the specified URL without waiting for a response.</span></span>  
  
    -   <span data-ttu-id="7f7d3-130">[Odeslání požadavku](api-management-advanced-policies.md#SendRequest) -odešle požadavek na zadanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-130">[Send request](api-management-advanced-policies.md#SendRequest) - Sends a request to the specified URL.</span></span>  
  
    -   <span data-ttu-id="7f7d3-131">[Nastavená proměnná](api-management-advanced-policies.md#set-variable) -zachovat hodnotu do proměnné s názvem kontext pro pozdější přístup.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-131">[Set variable](api-management-advanced-policies.md#set-variable) - Persist a value in a named context variable for later access.</span></span>  
  
    -   <span data-ttu-id="7f7d3-132">[Nastaví metodu požadavku](api-management-advanced-policies.md#SetRequestMethod) -vám umožní změnit metodu protokolu HTTP pro žádost.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-132">[Set request method](api-management-advanced-policies.md#SetRequestMethod) - Allows you to change the HTTP method for a request.</span></span>  
  
    -   <span data-ttu-id="7f7d3-133">[Nastavit stavový kód](api-management-advanced-policies.md#SetStatus) -změní stavový kód protokolu HTTP se zadanou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-133">[Set status code](api-management-advanced-policies.md#SetStatus) - Changes the HTTP status code to the specified value.</span></span>  
  
    -   <span data-ttu-id="7f7d3-134">[Trasování](api-management-advanced-policies.md#Trace) -přidá řetězec do [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-134">[Trace](api-management-advanced-policies.md#Trace) - Adds a string into the [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
    -   <span data-ttu-id="7f7d3-135">[Počkejte](api-management-advanced-policies.md#Wait) -čeká pro uzavřené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), nebo [řízení toku](api-management-advanced-policies.md#choose) zásady k dokončení než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-135">[Wait](api-management-advanced-policies.md#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies to complete before proceeding.</span></span>  
  
-   [<span data-ttu-id="7f7d3-136">Zásady ověřování</span><span class="sxs-lookup"><span data-stu-id="7f7d3-136">Authentication policies</span></span>](api-management-authentication-policies.md#AuthenticationPolicies)  
  
    -   <span data-ttu-id="7f7d3-137">[Ověřování Basic](api-management-authentication-policies.md#Basic) -ověření pomocí back-end službu pomocí základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-137">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
    -   <span data-ttu-id="7f7d3-138">[Ověřování pomocí certifikátu klienta](api-management-authentication-policies.md#ClientCertificate) -ověření pomocí back-end službu pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-138">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
-   [<span data-ttu-id="7f7d3-139">Zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="7f7d3-139">Caching policies</span></span>](api-management-caching-policies.md#CachingPolicies)  
  
    -   <span data-ttu-id="7f7d3-140">[Získat z mezipaměti](api-management-caching-policies.md#GetFromCache) -provést mezipaměti vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-140">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached response when available.</span></span>  
  
    -   <span data-ttu-id="7f7d3-141">[Úložiště pro mezipaměť](api-management-caching-policies.md#StoreToCache) -ukládá do mezipaměti odpovědi podle konfigurace zadané mezipaměti ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-141">[Store to cache](api-management-caching-policies.md#StoreToCache) - Caches response according to the specified cache control configuration.</span></span>  
  
    -   <span data-ttu-id="7f7d3-142">[Získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-142">[Get value from cache](api-management-caching-policies.md#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="7f7d3-143">[Uložení v mezipaměti hodnoty](api-management-caching-policies.md#StoreToCacheByKey) -uložit položky do mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-143">[Store value in cache](api-management-caching-policies.md#StoreToCacheByKey) - Store an item in the cache by key.</span></span>  
  
    -   <span data-ttu-id="7f7d3-144">[Odebrat hodnotu z mezipaměti](api-management-caching-policies.md#RemoveCacheByKey) -odebrání položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-144">[Remove value from cache](api-management-caching-policies.md#RemoveCacheByKey) - Remove an item in the cache by key.</span></span>  
  
-   [<span data-ttu-id="7f7d3-145">Mezidoménové zásady</span><span class="sxs-lookup"><span data-stu-id="7f7d3-145">Cross domain policies</span></span>](api-management-cross-domain-policies.md#CrossDomainPolicies)  
  
    -   <span data-ttu-id="7f7d3-146">[Povolit volání mezi doménami](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -zpřístupní rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-146">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
    -   <span data-ttu-id="7f7d3-147">[CORS](api-management-cross-domain-policies.md#CORS) – přidává podporu (CORS), aby operace nebo rozhraní API umožňující volání napříč doménami založené na prohlížeči klientů pro sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-147">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>  
  
    -   <span data-ttu-id="7f7d3-148">[JSONP](api-management-cross-domain-policies.md#JSONP) -přidá JSON s podporou odsazení (JSONP) operace nebo rozhraní API, chcete-li povolit volání mezi doménami z klientů JavaScript založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-148">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
-   [<span data-ttu-id="7f7d3-149">Zásady transformace</span><span class="sxs-lookup"><span data-stu-id="7f7d3-149">Transformation policies</span></span>](api-management-transformation-policies.md#TransformationPolicies)  
  
    -   <span data-ttu-id="7f7d3-150">[Převést JSON do XML](api-management-transformation-policies.md#ConvertJSONtoXML) – převede požadavku nebo odpovědi textu z JSON do XML.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-150">[Convert JSON to XML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON to XML.</span></span>  
  
    -   <span data-ttu-id="7f7d3-151">[Převést na JSON XML](api-management-transformation-policies.md#ConvertXMLtoJSON) – převede požadavku nebo odpovědi textu z XML do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-151">[Convert XML to JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML to JSON.</span></span>  
  
    -   <span data-ttu-id="7f7d3-152">[Najít a nahradit řetězec v textu](api-management-transformation-policies.md#Findandreplacestringinbody) – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-152">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
    -   <span data-ttu-id="7f7d3-153">[Maskování adresy URL v obsahu](api-management-transformation-policies.md#MaskURLSContent) -znovu zapíše (masky) odkazy v odpovědi body tak, aby ukazovaly na ekvivalentní propojení prostřednictvím brány.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-153">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span>  
  
    -   <span data-ttu-id="7f7d3-154">[Nastavení back-end služby](api-management-transformation-policies.md#SetBackendService) -změny službě back-end pro příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-154">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes the backend service for an incoming request.</span></span>  
  
    -   <span data-ttu-id="7f7d3-155">[Tělo nastavit](api-management-transformation-policies.md#SetBody) -nastaví obsah zprávy pro příchozí a odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-155">[Set body](api-management-transformation-policies.md#SetBody) - Sets the message body for incoming and outgoing requests.</span></span>  
  
    -   <span data-ttu-id="7f7d3-156">[Set – hlavička protokolu HTTP](api-management-transformation-policies.md#SetHTTPheader) – přiřazuje hodnotu existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-156">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>  
  
    -   <span data-ttu-id="7f7d3-157">[Nastavte parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-157">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
    -   <span data-ttu-id="7f7d3-158">[Přepsání adresy URL](api-management-transformation-policies.md#RewriteURL) – převede adresu URL požadavku z jeho veřejné formuláře do formuláře očekává webovou službou.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-158">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form to the form expected by the web service.</span></span>  
  
    -   <span data-ttu-id="7f7d3-159">[Transformace XML pomocí transformaci XSLT](api-management-transformation-policies.md#XSLTransform) -použije transformaci XSL na XML v textu požadavku nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7f7d3-159">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation to XML in the request or response body.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="7f7d3-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7f7d3-160">Next steps</span></span>
<span data-ttu-id="7f7d3-161">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="7f7d3-161">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
