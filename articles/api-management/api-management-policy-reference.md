---
title: "Zásady Azure API managementu"
description: "Další informace o zásadách dostupné pro konfiguraci API Management."
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
ms.openlocfilehash: adc0c4415e10ddd0b4994cecef17f026546e91a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-policy-reference"></a><span data-ttu-id="cca45-103">Zásady Azure API managementu</span><span class="sxs-lookup"><span data-stu-id="cca45-103">Azure API Management Policy Reference</span></span>
<span data-ttu-id="cca45-104">Tato část obsahuje index pro zásady v [zásady API managementu][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="cca45-104">This section provides an index for the policies in the [API Management policy reference][API Management policy reference].</span></span> <span data-ttu-id="cca45-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management][Policies in API Management].</span><span class="sxs-lookup"><span data-stu-id="cca45-105">For information on adding and configuring policies, see [Policies in API Management][Policies in API Management].</span></span>

<span data-ttu-id="cca45-106">Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných zásadách API Management (pokud zásady neurčí jinak).</span><span class="sxs-lookup"><span data-stu-id="cca45-106">Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise.</span></span> <span data-ttu-id="cca45-107">Některé zásady, jako [řízení toku] [ Control flow] a [nastavená proměnná] [ Set variable] jsou založené na výrazech zásad.</span><span class="sxs-lookup"><span data-stu-id="cca45-107">Some policies such as the [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="cca45-108">Další informace najdete v tématu [pokročilé zásady] [ Advanced policies] a [výrazy zásad][Policy expressions]</span><span class="sxs-lookup"><span data-stu-id="cca45-108">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions]</span></span>

## <a name="policy-reference-index"></a><span data-ttu-id="cca45-109">Rejstřík referenčních informací o zásadách</span><span class="sxs-lookup"><span data-stu-id="cca45-109">Policy reference index</span></span>
* <span data-ttu-id="cca45-110">[Zásady omezení přístupu][Access restriction policies]</span><span class="sxs-lookup"><span data-stu-id="cca45-110">[Access restriction policies][Access restriction policies]</span></span>
  * <span data-ttu-id="cca45-111">[Kontrola HTTP záhlaví] [ Check HTTP header] -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="cca45-111">[Check HTTP header][Check HTTP header] - Enforces existence and/or value of a HTTP Header.</span></span>
  * <span data-ttu-id="cca45-112">[Omezení četnosti volání podle předplatného] [ Limit call rate by subscription] -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="cca45-112">[Limit call rate by subscription][Limit call rate by subscription] - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>
  * <span data-ttu-id="cca45-113">[Omezení četnosti volání podle klíče](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="cca45-113">[Limit call rate by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>
  * <span data-ttu-id="cca45-114">[Omezit volající IP adresy] [ Restrict caller IPs] -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="cca45-114">[Restrict caller IPs][Restrict caller IPs] - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>
  * <span data-ttu-id="cca45-115">[Nastavení kvóty využití podle předplatného] [ Set usage quota by subscription] -umožňuje vynucovat obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="cca45-115">[Set usage quota by subscription][Set usage quota by subscription] - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>
  * <span data-ttu-id="cca45-116">[Nastavení kvóty využití podle klíče](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -umožňuje vynucovat obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="cca45-116">[Set usage quota by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>
  * <span data-ttu-id="cca45-117">[Ověření JWT] [ Validate JWT] -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="cca45-117">[Validate JWT][Validate JWT] - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>
* <span data-ttu-id="cca45-118">[Rozšířené zásady][Advanced policies]</span><span class="sxs-lookup"><span data-stu-id="cca45-118">[Advanced policies][Advanced policies]</span></span>
  * <span data-ttu-id="cca45-119">[Řízení toku] [ Control flow] – podmíněně platí příkazy zásad na základě výsledků vyhodnocení logická hodnota [výrazy][expressions].</span><span class="sxs-lookup"><span data-stu-id="cca45-119">[Control flow][Control flow] - Conditionally applies policy statements based on the results of the evaluation of Boolean [expressions][expressions].</span></span>
  * <span data-ttu-id="cca45-120">[Předat dál žádost] [ Forward request] -předá požadavek back-end službu.</span><span class="sxs-lookup"><span data-stu-id="cca45-120">[Forward request][Forward request] - Forwards the request to the backend service.</span></span>
  * <span data-ttu-id="cca45-121">[Protokol do centra událostí] [ Log to Event Hub] -odešle zprávy v zadaném formátu do zprávy Cíl definované [protokolovacího nástroje](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.</span><span class="sxs-lookup"><span data-stu-id="cca45-121">[Log to Event Hub][Log to Event Hub] - Sends messages in the specified format to a message target defined by a [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.</span></span>
  * <span data-ttu-id="cca45-122">[Opakujte](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -opakování provádění příkazů závorkách zásad, pokud a dokud nebude splněna podmínka.</span><span class="sxs-lookup"><span data-stu-id="cca45-122">[Retry](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - Retries execution of the enclosed policy statements, if and until the condition is met.</span></span> <span data-ttu-id="cca45-123">Spuštění se opakovaly zadaným časovým intervalům a až zadaný počet opakování.</span><span class="sxs-lookup"><span data-stu-id="cca45-123">Execution will repeat at the specified time intervals and up to the specified retry count.</span></span>
  * <span data-ttu-id="cca45-124">[Vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí zadanou odpověď přímo na volajícího.</span><span class="sxs-lookup"><span data-stu-id="cca45-124">[Return response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - Aborts pipeline execution and returns the specified response directly to the caller.</span></span>
  * <span data-ttu-id="cca45-125">[Odeslání žádosti o jednorázové jednoduché](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -odešle požadavek na zadanou adresu URL bez čekání na odpověď.</span><span class="sxs-lookup"><span data-stu-id="cca45-125">[Send one way request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - Sends a request to the specified URL without waiting for a response.</span></span>
  * <span data-ttu-id="cca45-126">[Odeslání požadavku](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -odešle požadavek na zadanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="cca45-126">[Send request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - Sends a request to the specified URL.</span></span>
  * <span data-ttu-id="cca45-127">[Nastaví metodu požadavku](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -vám umožní změnit metodu protokolu HTTP pro žádost.</span><span class="sxs-lookup"><span data-stu-id="cca45-127">[Set request method](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - Allows you to change the HTTP method for a request.</span></span>
  * <span data-ttu-id="cca45-128">[Nastavte stav](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -změní stavový kód protokolu HTTP se zadanou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="cca45-128">[Set status](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - Changes the HTTP status code to the specified value.</span></span>
  * <span data-ttu-id="cca45-129">[Nastavená proměnná] [ Set variable] -zachovat hodnotu v pojmenovaná [kontextu] [ context] proměnná pro pozdější přístup.</span><span class="sxs-lookup"><span data-stu-id="cca45-129">[Set variable][Set variable] - Persist a value in a named [context][context] variable for later access.</span></span>
  * <span data-ttu-id="cca45-130">[Trasování](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -přidá řetězec do [rozhraní API Inspector](api-management-howto-api-inspector.md) výstup.</span><span class="sxs-lookup"><span data-stu-id="cca45-130">[Trace](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - Adds a string into the [API Inspector](api-management-howto-api-inspector.md) output.</span></span>
  * <span data-ttu-id="cca45-131">[Počkejte](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) -počká pro závorkách odeslání žádosti o, Get hodnota z mezipaměti, nebo řídit tok zásady k dokončení než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="cca45-131">[Wait](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - Waits for enclosed Send request, Get value from cache, or Control flow policies to complete before proceeding.</span></span>
* <span data-ttu-id="cca45-132">[Zásady ověřování][Authentication policies]</span><span class="sxs-lookup"><span data-stu-id="cca45-132">[Authentication policies][Authentication policies]</span></span>
  * <span data-ttu-id="cca45-133">[Ověřování Basic] [ Authenticate with Basic] -ověření pomocí back-end službu pomocí základního ověřování.</span><span class="sxs-lookup"><span data-stu-id="cca45-133">[Authenticate with Basic][Authenticate with Basic] - Authenticate with a backend service using Basic authentication.</span></span>
  * <span data-ttu-id="cca45-134">[Ověřování pomocí certifikátu klienta] [ Authenticate with client certificate] -ověření pomocí back-end službu pomocí klientských certifikátů.</span><span class="sxs-lookup"><span data-stu-id="cca45-134">[Authenticate with client certificate][Authenticate with client certificate] - Authenticate with a backend service using client certificates.</span></span>
* <span data-ttu-id="cca45-135">[Zásady ukládání do mezipaměti][Caching policies]</span><span class="sxs-lookup"><span data-stu-id="cca45-135">[Caching policies][Caching policies]</span></span> 
  * <span data-ttu-id="cca45-136">[Získat z mezipaměti] [ Get from cache] -provést mezipaměti vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cca45-136">[Get from cache][Get from cache] - Perform cache look up and return a valid cached response when available.</span></span>
  * <span data-ttu-id="cca45-137">[Úložiště pro mezipaměť] [ Store to cache] -ukládá do mezipaměti odpovědi podle konfigurace zadané mezipaměti ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="cca45-137">[Store to cache][Store to cache] - Caches response according to the specified cache control configuration.</span></span>
  * <span data-ttu-id="cca45-138">[Získat hodnotu z mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="cca45-138">[Get value from cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>
  * <span data-ttu-id="cca45-139">[Uložení v mezipaměti hodnoty](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -uložit položky do mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="cca45-139">[Store value in cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Store an item in the cache by key.</span></span>
  * <span data-ttu-id="cca45-140">[Odebrat hodnotu z mezipaměti](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -odebrání položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="cca45-140">[Remove value from cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Remove an item in the cache by key.</span></span>
* <span data-ttu-id="cca45-141">[Různé zásady domény][Cross domain policies]</span><span class="sxs-lookup"><span data-stu-id="cca45-141">[Cross domain policies][Cross domain policies]</span></span> 
  * <span data-ttu-id="cca45-142">[Povolit volání mezi doménami] [ Allow cross-domain calls] -zpřístupní rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="cca45-142">[Allow cross-domain calls][Allow cross-domain calls] - Makes the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>
  * <span data-ttu-id="cca45-143">[CORS] [ CORS] – přidává podporu (CORS), aby operace nebo rozhraní API umožňující volání napříč doménami založené na prohlížeči klientů pro sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="cca45-143">[CORS][CORS] - Adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>
  * <span data-ttu-id="cca45-144">[JSONP] [ JSONP] -přidá JSON s podporou odsazení (JSONP) operace nebo rozhraní API, chcete-li povolit volání mezi doménami z klientů JavaScript založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="cca45-144">[JSONP][JSONP] - Adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span>
* <span data-ttu-id="cca45-145">[Zásad transformace][Transformation policies]</span><span class="sxs-lookup"><span data-stu-id="cca45-145">[Transformation policies][Transformation policies]</span></span> 
  * <span data-ttu-id="cca45-146">[Převést JSON do XML] [ Convert JSON to XML] – převede požadavku nebo odpovědi textu z JSON do XML.</span><span class="sxs-lookup"><span data-stu-id="cca45-146">[Convert JSON to XML][Convert JSON to XML] - Converts request or response body from JSON to XML.</span></span>
  * <span data-ttu-id="cca45-147">[Převést na JSON XML] [ Convert XML to JSON] – převede požadavku nebo odpovědi textu z XML do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="cca45-147">[Convert XML to JSON][Convert XML to JSON] - Converts request or response body from XML to JSON.</span></span>
  * <span data-ttu-id="cca45-148">[Najít a nahradit řetězec v textu] [ Find and replace string in body] – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="cca45-148">[Find and replace string in body][Find and replace string in body] - Finds a request or response substring and replaces it with a different substring.</span></span>
  * <span data-ttu-id="cca45-149">[Maskování adresy URL v obsahu] [ Mask URLs in content] -znovu zapíše (masky) odkazy v odpovědi body tak, aby ukazovaly na ekvivalentní propojení prostřednictvím brány.</span><span class="sxs-lookup"><span data-stu-id="cca45-149">[Mask URLs in content][Mask URLs in content] - Re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span>
  * <span data-ttu-id="cca45-150">[Nastavení back-end služby] [ Set backend service] -změny službě back-end pro příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="cca45-150">[Set backend service][Set backend service] - Changes the backend service for an incoming request.</span></span>
  * <span data-ttu-id="cca45-151">[Tělo nastavit] [ Set body] -nastaví obsah zprávy pro příchozí a odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="cca45-151">[Set body][Set body] - Sets the message body for incoming and outgoing requests.</span></span>
  * <span data-ttu-id="cca45-152">[Set – hlavička protokolu HTTP] [ Set HTTP header] – přiřazuje hodnotu existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.</span><span class="sxs-lookup"><span data-stu-id="cca45-152">[Set HTTP header][Set HTTP header] - Assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>
  * <span data-ttu-id="cca45-153">[Nastavte parametr řetězce dotazu] [ Set query string parameter] – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.</span><span class="sxs-lookup"><span data-stu-id="cca45-153">[Set query string parameter][Set query string parameter] - Adds, replaces value of, or deletes request query string parameter.</span></span>
  * <span data-ttu-id="cca45-154">[Přepsání adresy URL] [ Rewrite URL] – převede adresu URL požadavku z jeho veřejné formuláře do formuláře očekává webovou službou.</span><span class="sxs-lookup"><span data-stu-id="cca45-154">[Rewrite URL][Rewrite URL] - Converts a request URL from its public form to the form expected by the web service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cca45-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cca45-155">Next steps</span></span>
<span data-ttu-id="cca45-156">Další informace o výrazů zásad najdete v následujícím videu.</span><span class="sxs-lookup"><span data-stu-id="cca45-156">For more information on policy expressions, see the following video.</span></span>

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
[Log to Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON to XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML to JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
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


