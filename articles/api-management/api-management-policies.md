---
title: "zásady služby API Management aaaAzure | Microsoft Docs"
description: "Další informace o hello zásady, které jsou k dispozici pro použití v Azure API Management."
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
ms.openlocfilehash: 1c468ff37d73359f1dd694b91e20c2ca04f8934e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-policies"></a><span data-ttu-id="c5b89-103">Zásady služby API Management</span><span class="sxs-lookup"><span data-stu-id="c5b89-103">API Management policies</span></span>
<span data-ttu-id="c5b89-104">Tato část obsahuje odkaz pro hello následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c5b89-104">This section provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="c5b89-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="c5b89-105">For information on adding and configuring policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
  
 <span data-ttu-id="c5b89-106">Zásady jsou vynikající funkcí hello systému, který povolí hello vydavatele toochange hello chování hello rozhraní API prostřednictvím konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c5b89-106">Policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="c5b89-107">Zásady představují kolekci příkazů, které se postupně provádí na hello požadavku nebo odezvy z rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c5b89-107">Policies are a collection of Statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="c5b89-108">Oblíbené příkazy patří převod formátu XML tooJSON a četnosti omezení toorestrict hello množství příchozích volání od vývojáře.</span><span class="sxs-lookup"><span data-stu-id="c5b89-108">Popular Statements include format conversion from XML tooJSON and call rate limiting toorestrict hello amount of incoming calls from a developer.</span></span> <span data-ttu-id="c5b89-109">Mnoho další zásady jsou k dispozici předinstalované hello.</span><span class="sxs-lookup"><span data-stu-id="c5b89-109">Many more policies are available out of hello box.</span></span>  
  
 <span data-ttu-id="c5b89-110">Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných hello zásad služby API Management, pokud hello zásady neurčí jinak.</span><span class="sxs-lookup"><span data-stu-id="c5b89-110">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="c5b89-111">Některé zásady, jako je například hello [řízení toku](api-management-advanced-policies.md#choose) a [nastavená proměnná](api-management-advanced-policies.md#set-variable) jsou založené na výrazech zásad.</span><span class="sxs-lookup"><span data-stu-id="c5b89-111">Some policies such as hello [Control flow](api-management-advanced-policies.md#choose) and [Set variable](api-management-advanced-policies.md#set-variable) policies are based on policy expressions.</span></span> <span data-ttu-id="c5b89-112">Další informace najdete v tématu [pokročilé zásady](api-management-advanced-policies.md#AdvancedPolicies) a [výrazy zásad](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="c5b89-112">For more information, see [Advanced policies](api-management-advanced-policies.md#AdvancedPolicies) and [Policy expressions](api-management-policy-expressions.md).</span></span>  
  
##  <span data-ttu-id="c5b89-113"><a name="ProxyPolicies"></a>Zásady</span><span class="sxs-lookup"><span data-stu-id="c5b89-113"><a name="ProxyPolicies"></a> Policies</span></span>  
  
-   [<span data-ttu-id="c5b89-114">Zásady omezení přístupu</span><span class="sxs-lookup"><span data-stu-id="c5b89-114">Access restriction policies</span></span>](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
  
    -   <span data-ttu-id="c5b89-115">[Kontrola HTTP záhlaví](api-management-access-restriction-policies.md#CheckHTTPHeader) -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="c5b89-115">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
    -   <span data-ttu-id="c5b89-116">[Omezení četnosti volání podle předplatného](api-management-access-restriction-policies.md#LimitCallRate) -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="c5b89-116">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="c5b89-117">[Omezení četnosti volání podle klíče](api-management-access-restriction-policies.md#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="c5b89-117">[Limit call rate by key](api-management-access-restriction-policies.md#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="c5b89-118">[Omezit volající IP adresy](api-management-access-restriction-policies.md#RestrictCallerIPs) -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="c5b89-118">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
    -   <span data-ttu-id="c5b89-119">[Nastavení kvóty využití podle předplatného](api-management-access-restriction-policies.md#SetUsageQuota) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="c5b89-119">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="c5b89-120">[Nastavení kvóty využití podle klíče](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="c5b89-120">[Set usage quota by key](api-management-access-restriction-policies.md#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="c5b89-121">[Ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="c5b89-121">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
-   [<span data-ttu-id="c5b89-122">Pokročilé zásady</span><span class="sxs-lookup"><span data-stu-id="c5b89-122">Advanced policies</span></span>](api-management-advanced-policies.md#AdvancedPolicies)  
  
    -   <span data-ttu-id="c5b89-123">[Řízení toku](api-management-advanced-policies.md#choose) – podmíněně platí příkazy zásad podle hello vyhodnocení výrazy logických hodnot.</span><span class="sxs-lookup"><span data-stu-id="c5b89-123">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on hello evaluation of Boolean expressions.</span></span>  
  
    -   <span data-ttu-id="c5b89-124">[Předat dál žádost](api-management-advanced-policies.md#ForwardRequest) -předává hello požadavek toohello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="c5b89-124">[Forward request](api-management-advanced-policies.md#ForwardRequest) - Forwards hello request toohello backend service.</span></span>  
  
    -   <span data-ttu-id="c5b89-125">[Protokolu tooEvent rozbočovače](api-management-advanced-policies.md#log-to-eventhub) -odešle zprávy v hello zadaný formát tooa cíl zprávy definované entity protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="c5b89-125">[Log tooEvent Hub](api-management-advanced-policies.md#log-to-eventhub) - Sends messages in hello specified format tooa message target defined by a Logger entity.</span></span>  
  
    -   <span data-ttu-id="c5b89-126">[Opakujte](api-management-advanced-policies.md#Retry) -provádění opakování hello uzavřené příkazy zásad, pokud a dokud je splněna podmínka hello.</span><span class="sxs-lookup"><span data-stu-id="c5b89-126">[Retry](api-management-advanced-policies.md#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="c5b89-127">Spuštění se bude opakovat v hello zadaným časovým intervalům a až toohello zadaný počet opakování.</span><span class="sxs-lookup"><span data-stu-id="c5b89-127">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>  
  
    -   <span data-ttu-id="c5b89-128">[Vrátí odpověď](api-management-advanced-policies.md#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí hello zadané odpovědi přímo toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="c5b89-128">[Return response](api-management-advanced-policies.md#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span>  
  
    -   <span data-ttu-id="c5b89-129">[Odeslání žádosti o jednorázové jednoduché](api-management-advanced-policies.md#SendOneWayRequest) -odešle žádost toohello zadané adresy URL bez čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="c5b89-129">[Send one way request](api-management-advanced-policies.md#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>  
  
    -   <span data-ttu-id="c5b89-130">[Odeslání požadavku](api-management-advanced-policies.md#SendRequest) -odešle žádost toohello zadaná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="c5b89-130">[Send request](api-management-advanced-policies.md#SendRequest) - Sends a request toohello specified URL.</span></span>  
  
    -   <span data-ttu-id="c5b89-131">[Nastavená proměnná](api-management-advanced-policies.md#set-variable) -zachovat hodnotu do proměnné s názvem kontext pro pozdější přístup.</span><span class="sxs-lookup"><span data-stu-id="c5b89-131">[Set variable](api-management-advanced-policies.md#set-variable) - Persist a value in a named context variable for later access.</span></span>  
  
    -   <span data-ttu-id="c5b89-132">[Nastaví metodu požadavku](api-management-advanced-policies.md#SetRequestMethod) -vám umožní toochange hello HTTP metoda pro žádost.</span><span class="sxs-lookup"><span data-stu-id="c5b89-132">[Set request method](api-management-advanced-policies.md#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>  
  
    -   <span data-ttu-id="c5b89-133">[Nastavit stavový kód](api-management-advanced-policies.md#SetStatus) -toohello kód stavu hello HTTP změny zadané hodnotě.</span><span class="sxs-lookup"><span data-stu-id="c5b89-133">[Set status code](api-management-advanced-policies.md#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>  
  
    -   <span data-ttu-id="c5b89-134">[Trasování](api-management-advanced-policies.md#Trace) -přidá řetězec do hello [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup.</span><span class="sxs-lookup"><span data-stu-id="c5b89-134">[Trace](api-management-advanced-policies.md#Trace) - Adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
    -   <span data-ttu-id="c5b89-135">[Počkejte](api-management-advanced-policies.md#Wait) -čeká pro uzavřené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), nebo [řízení toku](api-management-advanced-policies.md#choose) zásady toocomplete než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c5b89-135">[Wait](api-management-advanced-policies.md#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies toocomplete before proceeding.</span></span>  
  
-   [<span data-ttu-id="c5b89-136">Zásady ověřování</span><span class="sxs-lookup"><span data-stu-id="c5b89-136">Authentication policies</span></span>](api-management-authentication-policies.md#AuthenticationPolicies)  
  
    -   <span data-ttu-id="c5b89-137">[Ověřování Basic](api-management-authentication-policies.md#Basic) -ověření pomocí back-end službu pomocí základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="c5b89-137">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
    -   <span data-ttu-id="c5b89-138">[Ověřování pomocí certifikátu klienta](api-management-authentication-policies.md#ClientCertificate) -ověření pomocí back-end službu pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="c5b89-138">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
-   [<span data-ttu-id="c5b89-139">Zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="c5b89-139">Caching policies</span></span>](api-management-caching-policies.md#CachingPolicies)  
  
    -   <span data-ttu-id="c5b89-140">[Získat z mezipaměti](api-management-caching-policies.md#GetFromCache) -provést mezipaměti vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c5b89-140">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached response when available.</span></span>  
  
    -   <span data-ttu-id="c5b89-141">[Uložení toocache](api-management-caching-policies.md#StoreToCache) -mezipamětí odpověď podle toohello uvedená konfigurace mezipaměti ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="c5b89-141">[Store toocache](api-management-caching-policies.md#StoreToCache) - Caches response according toohello specified cache control configuration.</span></span>  
  
    -   <span data-ttu-id="c5b89-142">[Získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="c5b89-142">[Get value from cache](api-management-caching-policies.md#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="c5b89-143">[Uložení v mezipaměti hodnoty](api-management-caching-policies.md#StoreToCacheByKey) -uložit položku hello mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="c5b89-143">[Store value in cache](api-management-caching-policies.md#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>  
  
    -   <span data-ttu-id="c5b89-144">[Odebrat hodnotu z mezipaměti](api-management-caching-policies.md#RemoveCacheByKey) -odebrání položky v mezipaměti hello podle klíče.</span><span class="sxs-lookup"><span data-stu-id="c5b89-144">[Remove value from cache](api-management-caching-policies.md#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>  
  
-   [<span data-ttu-id="c5b89-145">Mezidoménové zásady</span><span class="sxs-lookup"><span data-stu-id="c5b89-145">Cross domain policies</span></span>](api-management-cross-domain-policies.md#CrossDomainPolicies)  
  
    -   <span data-ttu-id="c5b89-146">[Povolit volání mezi doménami](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -zpřístupní hello rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="c5b89-146">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
    -   <span data-ttu-id="c5b89-147">[CORS](api-management-cross-domain-policies.md#CORS) -přidá sdílení prostředků různých původů (CORS) podporoval tooan operaci nebo volání rozhraní API tooallow napříč doménami založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="c5b89-147">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
    -   <span data-ttu-id="c5b89-148">[JSONP](api-management-cross-domain-policies.md#JSONP) – přidá JSON s odsazení (JSONP) operaci tooan podpory nebo rozhraní API tooallow napříč doménami volá z klientů JavaScript založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c5b89-148">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
-   [<span data-ttu-id="c5b89-149">Zásady transformace</span><span class="sxs-lookup"><span data-stu-id="c5b89-149">Transformation policies</span></span>](api-management-transformation-policies.md#TransformationPolicies)  
  
    -   <span data-ttu-id="c5b89-150">[Převést JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) – převede požadavku nebo odpovědi textu z JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="c5b89-150">[Convert JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON tooXML.</span></span>  
  
    -   <span data-ttu-id="c5b89-151">[Převod XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – převede požadavku nebo odpovědi textu z XML tooJSON.</span><span class="sxs-lookup"><span data-stu-id="c5b89-151">[Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML tooJSON.</span></span>  
  
    -   <span data-ttu-id="c5b89-152">[Najít a nahradit řetězec v textu](api-management-transformation-policies.md#Findandreplacestringinbody) – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="c5b89-152">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
    -   <span data-ttu-id="c5b89-153">[Maskování adresy URL v obsahu](api-management-transformation-policies.md#MaskURLSContent) -znovu zapíše (masky) odkazy v odpovědi hello body tak, aby ukazovaly toohello ekvivalentní propojení prostřednictvím brány hello.</span><span class="sxs-lookup"><span data-stu-id="c5b89-153">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>  
  
    -   <span data-ttu-id="c5b89-154">[Nastavení back-end služby](api-management-transformation-policies.md#SetBackendService) -změny hello back-end službu pro příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="c5b89-154">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes hello backend service for an incoming request.</span></span>  
  
    -   <span data-ttu-id="c5b89-155">[Tělo nastavit](api-management-transformation-policies.md#SetBody) -nastaví hello text zprávy pro příchozí a odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="c5b89-155">[Set body](api-management-transformation-policies.md#SetBody) - Sets hello message body for incoming and outgoing requests.</span></span>  
  
    -   <span data-ttu-id="c5b89-156">[Set – hlavička protokolu HTTP](api-management-transformation-policies.md#SetHTTPheader) – přiřadí hodnota tooan existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.</span><span class="sxs-lookup"><span data-stu-id="c5b89-156">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
    -   <span data-ttu-id="c5b89-157">[Nastavte parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.</span><span class="sxs-lookup"><span data-stu-id="c5b89-157">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
    -   <span data-ttu-id="c5b89-158">[Přepsání adresy URL](api-management-transformation-policies.md#RewriteURL) – převede adresu URL požadavku z formuláře toohello jeho veřejné formuláře očekávanou hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="c5b89-158">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form toohello form expected by hello web service.</span></span>  
  
    -   <span data-ttu-id="c5b89-159">[Transformace XML pomocí transformaci XSLT](api-management-transformation-policies.md#XSLTransform) -vztahuje k transformaci tooXML XSL v textu požadavku nebo odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="c5b89-159">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="c5b89-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5b89-160">Next steps</span></span>
<span data-ttu-id="c5b89-161">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="c5b89-161">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
