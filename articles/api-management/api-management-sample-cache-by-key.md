---
title: "Vlastní ukládání do mezipaměti ve službě Azure API Management"
description: "Zjistěte, jak položky do mezipaměti podle klíče ve službě Azure API Management"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: f5d5f44e34fbcd122a10be0ca5b3001760c4c64d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="custom-caching-in-azure-api-management"></a><span data-ttu-id="258ac-103">Vlastní ukládání do mezipaměti ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="258ac-103">Custom caching in Azure API Management</span></span>
<span data-ttu-id="258ac-104">Služba Azure API Management má integrovanou podporu pro [ukládání do mezipaměti odpovědi HTTP](api-management-howto-cache.md) pomocí adresy URL prostředku jako klíč.</span><span class="sxs-lookup"><span data-stu-id="258ac-104">Azure API Management service has built-in support for [HTTP response caching](api-management-howto-cache.md) using the resource URL as the key.</span></span> <span data-ttu-id="258ac-105">Klíč mohou být upravena hlaviček požadavků pomocí `vary-by` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="258ac-105">The key can be modified by request headers using the `vary-by` properties.</span></span> <span data-ttu-id="258ac-106">To je užitečné pro ukládání do mezipaměti celé odpovědi protokolu HTTP (neboli reprezentace), ale někdy je vhodné do mezipaměti jenom část reprezentace.</span><span class="sxs-lookup"><span data-stu-id="258ac-106">This is useful for caching entire HTTP responses (aka representations), but sometimes it is useful to just cache a portion of a representation.</span></span> <span data-ttu-id="258ac-107">Nové [hodnota mezipaměti vyhledávání](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) a [hodnota úložiště mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) zásady poskytují možnost ukládání a načítání libovolný kusy data přímo z definice zásady.</span><span class="sxs-lookup"><span data-stu-id="258ac-107">The new [cache-lookup-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) and [cache-store-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) policies provide the ability to store and retrieve arbitrary pieces of data from within policy definitions.</span></span> <span data-ttu-id="258ac-108">Tato možnost také přidá hodnotu dříve přináší [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zásad vzhledem k tomu, že teď můžete mezipaměti odpovědí z externích služeb.</span><span class="sxs-lookup"><span data-stu-id="258ac-108">This ability also adds value to the previously introduced [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy because you can now cache responses from external services.</span></span>

## <a name="architecture"></a><span data-ttu-id="258ac-109">Architektura</span><span class="sxs-lookup"><span data-stu-id="258ac-109">Architecture</span></span>
<span data-ttu-id="258ac-110">Služba API Management používá datové mezipaměti sdíleného za klienta tak, aby při změně měřítka až víc jednotek služby budete stále dostávat přístup k stejná data jako do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="258ac-110">API Management service uses a shared per-tenant data cache so that, as you scale up to multiple units you will still get access to the same cached data.</span></span> <span data-ttu-id="258ac-111">Při práci s více oblast nasazení existují však nezávislé mezipaměti v každém z oblastí.</span><span class="sxs-lookup"><span data-stu-id="258ac-111">However, when working with a multi-region deployment there are independent caches within each of the regions.</span></span> <span data-ttu-id="258ac-112">Z toho důvodu je důležité nebude pracovat s mezipaměti jako úložiště dat, kde je jediný zdroj některé část informace.</span><span class="sxs-lookup"><span data-stu-id="258ac-112">Due to this, it is important to not treat the cache as a data store, where it is the only source of some piece of information.</span></span> <span data-ttu-id="258ac-113">Pokud nebyla a později se rozhodli využít výhod nasazení s více oblastí, potom zákazníkům, kteří cestují mohou ztratit přístup ke tohoto data uložená v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="258ac-113">If you did, and later decided to take advantage of the multi-region deployment, then customers with users that travel may lose access to that cached data.</span></span>

## <a name="fragment-caching"></a><span data-ttu-id="258ac-114">Fragment ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="258ac-114">Fragment caching</span></span>
<span data-ttu-id="258ac-115">Existují určité případy, kde odpovědí nevrátila obsahovat některé část dat, která je nákladná, chcete-li zjistit a ještě stále aktuální pro přiměřeně krátké doby.</span><span class="sxs-lookup"><span data-stu-id="258ac-115">There are certain cases where responses being returned contain some portion of data that is expensive to determine and yet remains fresh for a reasonable amount of time.</span></span> <span data-ttu-id="258ac-116">Jako příklad zvažte službu vytvořené letecká společnost, která obsahuje informace týkající se rezervace letu, stav letu, atd. Pokud je uživatel členem programu body airlines, mají by také informace týkající se jejich aktuální stav a shromážděných za vzdálenost.</span><span class="sxs-lookup"><span data-stu-id="258ac-116">As an example, consider a service built by an airline that provides information relating flight reservations, flight status, etc. If the user is a member of the airlines points program, they would also have information relating to their current status and mileage accumulated.</span></span> <span data-ttu-id="258ac-117">Tyto informace související s uživatelem se pravděpodobně uloží do jiného systému, ale může být žádoucí chcete zahrnout do odpovědi vrátil o letu stav a rezervace.</span><span class="sxs-lookup"><span data-stu-id="258ac-117">This user-related information might be stored in a different system, but it may be desirable to include it in responses returned about flight status and reservations.</span></span> <span data-ttu-id="258ac-118">To lze provést pomocí procesu nazývaného fragment ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="258ac-118">This can be done using a process called fragment caching.</span></span> <span data-ttu-id="258ac-119">Primární reprezentace lze vrátit ze zdrojového serveru pomocí nějaký druh tokenu k označení, kde má být vložen informace související s uživatelem.</span><span class="sxs-lookup"><span data-stu-id="258ac-119">The primary representation can be returned from the origin server using some kind of token to indicate where the user-related information is to be inserted.</span></span> 

<span data-ttu-id="258ac-120">Vezměte v úvahu následující JSON odpověď z back-end rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="258ac-120">Consider the following JSON response from a backend API.</span></span>

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

<span data-ttu-id="258ac-121">A sekundární prostředek na úrovni `/userprofile/{userid}` jako,</span><span class="sxs-lookup"><span data-stu-id="258ac-121">And secondary resource at `/userprofile/{userid}` that looks like,</span></span>

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

<span data-ttu-id="258ac-122">Aby bylo možné zjistit informace o příslušné uživatele chcete zahrnout, je potřeba zjistit, kdo je koncový uživatel.</span><span class="sxs-lookup"><span data-stu-id="258ac-122">In order to determine the appropriate user information to include, we need to identify who the end user is.</span></span> <span data-ttu-id="258ac-123">Tento mechanismus je závisí na implementaci.</span><span class="sxs-lookup"><span data-stu-id="258ac-123">This mechanism is implementation dependent.</span></span> <span data-ttu-id="258ac-124">Jako příklad používám `Subject` z deklarací identity `JWT` tokenu.</span><span class="sxs-lookup"><span data-stu-id="258ac-124">As an example, I am using the `Subject` claim of a `JWT` token.</span></span> 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

<span data-ttu-id="258ac-125">Nemůžeme Uložit `enduserid` hodnotu v kontextu proměnné pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="258ac-125">We store this `enduserid` value in a context variable for later use.</span></span> <span data-ttu-id="258ac-126">Dalším krokem je určit, pokud se na předchozí požadavek již načíst informace o uživateli a uloženy v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="258ac-126">The next step is to determine if a previous request has already retrieved the user information and stored it in the cache.</span></span> <span data-ttu-id="258ac-127">Pro tento používáme `cache-lookup-value` zásad.</span><span class="sxs-lookup"><span data-stu-id="258ac-127">For this we use the `cache-lookup-value` policy.</span></span>

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

<span data-ttu-id="258ac-128">Pokud neexistuje žádná položka v mezipaměti, která odpovídá hodnotě klíče a ne `userprofile` vytvoří kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="258ac-128">If there is no entry in the cache that corresponds to the key value, then no `userprofile` context variable will be created.</span></span> <span data-ttu-id="258ac-129">Ověření, zda úspěchu při použití vyhledávání `choose` řízení toku zásad.</span><span class="sxs-lookup"><span data-stu-id="258ac-129">We check the success of the lookup using the `choose` control flow policy.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

<span data-ttu-id="258ac-130">Pokud `userprofile` kontextové proměnné neexistuje, pak bude nutné provést požadavek HTTP se jej načíst.</span><span class="sxs-lookup"><span data-stu-id="258ac-130">If the `userprofile` context variable doesn’t exist, then we are going to have to make an HTTP request to retrieve it.</span></span>

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points to the profile for the current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="258ac-131">Používáme `enduserid` konstruovat adresu URL pro prostředek profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="258ac-131">We use the `enduserid` to construct the URL to the user profile resource.</span></span> <span data-ttu-id="258ac-132">Jakmile jsme odpověď, jsme základní text z odpovědi pro vyžádání obsahu a uložte ho zpátky do kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="258ac-132">Once we have the response, we can pull the body text out of the response and store it back into a context variable.</span></span>

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

<span data-ttu-id="258ac-133">Abyste se vyhnuli nám nutnosti provádět tento požadavek HTTP znovu, pokud stejný uživatel zadá další žádost, jsme můžete uložit do mezipaměti uživatelský profil.</span><span class="sxs-lookup"><span data-stu-id="258ac-133">To avoid us having to make this HTTP request again, when the same user makes another request, we can store the user profile in the cache.</span></span>

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

<span data-ttu-id="258ac-134">Uložíme hodnota v mezipaměti pomocí přesně stejný klíč, který jsme původně pokus o její načtení.</span><span class="sxs-lookup"><span data-stu-id="258ac-134">We store the value in the cache using the exact same key that we originally attempted to retrieve it with.</span></span> <span data-ttu-id="258ac-135">Dobu, která jsme zvolit uložení hodnota by měla být založena na jak často jsou informace o změny a jak odolný vůči chybám uživatele na aktuální informace.</span><span class="sxs-lookup"><span data-stu-id="258ac-135">The duration that we choose to store the value should be based on how often the information changes and how tolerant users are to out of date information.</span></span> 

<span data-ttu-id="258ac-136">Je důležité si uvědomit, že načítání z mezipaměti je stále mimo proces, požadavku sítě a potenciálně můžete přesto přidat desítkami milisekund na žádost.</span><span class="sxs-lookup"><span data-stu-id="258ac-136">It is important to realize that retrieving from the cache is still an out-of-process, network request and potentially can still add tens of milliseconds to the request.</span></span> <span data-ttu-id="258ac-137">Výhody pocházet při určování, že výrazně delší, než je z důvodu museli databáze dotazy nebo agregační informace z více back EndY přijímá informace o profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="258ac-137">The benefits come when determining the user profile information takes significantly longer than that due to needing to do database queries or aggregate information from multiple back-ends.</span></span>

<span data-ttu-id="258ac-138">Je posledním krokem v procesu aktualizace vrácený odpovědi pomocí našich informace o profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="258ac-138">The final step in the process is to update the returned response with our user profile information.</span></span>

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

<span data-ttu-id="258ac-139">Se rozhodli použít uvozovky jako součást tokenu, aby i v případě, že nahradit neprobíhá, odpověď byla stále platný kód JSON.</span><span class="sxs-lookup"><span data-stu-id="258ac-139">I chose to include the quotation marks as part of the token so that even when the replace doesn’t occur, the response was still valid JSON.</span></span> <span data-ttu-id="258ac-140">To se primárně usnadnění ladění.</span><span class="sxs-lookup"><span data-stu-id="258ac-140">This was primarily to make debugging easier.</span></span>

<span data-ttu-id="258ac-141">Jakmile se společně sloučit všechny tyto kroky, konečný výsledek je zásadu, která vypadá jako následující.</span><span class="sxs-lookup"><span data-stu-id="258ac-141">Once you combine all these steps together, the end result is a policy that looks like the following one.</span></span>

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in the cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request to get user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points to the profile for the current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

<span data-ttu-id="258ac-142">Tento přístup ukládání do mezipaměti se primárně používá v weby kde HTML tvoří na straně serveru, aby je možné vykreslit jako jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="258ac-142">This caching approach is primarily used in web sites where HTML is composed on the server side so that it can be rendered as a single page.</span></span> <span data-ttu-id="258ac-143">Však může také být užitečné v rozhraní API, kde klienti nemohou provádět klienta na straně ukládání do mezipaměti protokolu HTTP nebo je žádoucí nechcete uvést tuto odpovědnost na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="258ac-143">However, it can also be useful in APIs where clients cannot do client side HTTP caching or it is desirable not to put that responsibility on the client.</span></span>

<span data-ttu-id="258ac-144">Tento stejný druh ukládání do mezipaměti fragment lze také provést na webových serverech back-end pomocí ukládání do mezipaměti serveru Redis, ale pomocí služby pro správu rozhraní API pro tuto práci je užitečné, když v mezipaměti fragmenty pocházejí z různých back EndY než primární odpovědi.</span><span class="sxs-lookup"><span data-stu-id="258ac-144">This same kind of fragment caching can also be done on the backend web servers using a Redis caching server, however, using the API Management service to perform this work is useful when the cached fragments are coming from different back-ends than the primary responses.</span></span>

## <a name="transparent-versioning"></a><span data-ttu-id="258ac-145">Transparentní Správa verzí</span><span class="sxs-lookup"><span data-stu-id="258ac-145">Transparent versioning</span></span>
<span data-ttu-id="258ac-146">Je obvyklé, více verzí jinou implementaci rozhraní API podporovaná v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="258ac-146">It is common practice for multiple different implementation versions of an API to be supported at any one time.</span></span> <span data-ttu-id="258ac-147">Toto je pravděpodobně pro podporu různých prostředích, jako je vývojářů, testovací, produkční atd., nebo může být pro podporu starší verze rozhraní API pro příjemce rozhraní API pro migraci na novější verze.</span><span class="sxs-lookup"><span data-stu-id="258ac-147">This is perhaps to support different environments, like dev, test, production, etc, or it may be to support older versions of the API to give time for API consumers to migrate to newer versions.</span></span> 

<span data-ttu-id="258ac-148">Jeden ze způsobů zpracování toto místo aby vývojáři klienta Změna adresy URL z `/v1/customers` k `/v2/customers` je uložit v data profilu v klientu kterou verzi rozhraní API aktuálně chtějí používat a volání URL odpovídající back-end.</span><span class="sxs-lookup"><span data-stu-id="258ac-148">One approach to handling this instead of requiring client developers to change the URLs from `/v1/customers` to `/v2/customers` is to store in the consumer’s profile data which version of the API they currently wish to use and call the appropriate backend URL.</span></span> <span data-ttu-id="258ac-149">Aby bylo možné zjistit adresu URL back-end správné volání pro konkrétního klienta, je nutné dotaz některé konfigurační data.</span><span class="sxs-lookup"><span data-stu-id="258ac-149">In order to determine the correct backend URL to call for a particular client, it is necessary to query some configuration data.</span></span> <span data-ttu-id="258ac-150">Pomocí ukládání do mezipaměti tato konfigurační data, jsme minimalizovat snížení výkonu to toto vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="258ac-150">By caching this configuration data, we can minimize the performance penalty of doing this lookup.</span></span>

<span data-ttu-id="258ac-151">Prvním krokem je určit identifikátor slouží ke konfiguraci požadovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="258ac-151">The first step is to determine the identifier used to configure the desired version.</span></span> <span data-ttu-id="258ac-152">V tomto příkladu se rozhodli verzi pro kód product key předplatného.</span><span class="sxs-lookup"><span data-stu-id="258ac-152">In this example, I chose to associate the version to the product subscription key.</span></span> 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

<span data-ttu-id="258ac-153">Potom provedeme vyhledávání v mezipaměti zobrazíte, když jsme již načtení verze požadované klienta.</span><span class="sxs-lookup"><span data-stu-id="258ac-153">We then do a cache lookup to see if we already have retrieved the desired client version.</span></span>

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

<span data-ttu-id="258ac-154">Potom jsme zkontrolujte Pokud nebyly nalezeny jej v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="258ac-154">Then we check to see if we did not find it in the cache.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

<span data-ttu-id="258ac-155">Pokud jsme nebyla pak jsme přejděte a jejich načtení.</span><span class="sxs-lookup"><span data-stu-id="258ac-155">If we didn’t then we go and retrieve it.</span></span>

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="258ac-156">Rozbalte text odpovědi text z odpovědi.</span><span class="sxs-lookup"><span data-stu-id="258ac-156">Extract the response body text from the response.</span></span>

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

<span data-ttu-id="258ac-157">Uložte ji zpět do mezipaměti pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="258ac-157">Store it back in the cache for future use.</span></span>

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

<span data-ttu-id="258ac-158">A nakonec Aktualizujte adresu URL back-end vyberte verzi požadované klientem služby.</span><span class="sxs-lookup"><span data-stu-id="258ac-158">And finally update the back-end URL to select the version of the service desired by the client.</span></span>

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

<span data-ttu-id="258ac-159">Zásady úplně je následující.</span><span class="sxs-lookup"><span data-stu-id="258ac-159">The completely policy is as follows.</span></span>

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in the cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

<span data-ttu-id="258ac-160">Povolení rozhraní API příjemci transparentně řídit, které back-end verze je přistupuje klienty, aniž by museli aktualizovat a znovu nasaďte klienty je elegantní řešení, které řeší mnoho aspekty správy verzí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="258ac-160">Enabling API consumers to transparently control which backend version is being accessed by clients without having to update and redeploy clients is a elegant solution that addresses many API versioning concerns.</span></span>

## <a name="tenant-isolation"></a><span data-ttu-id="258ac-161">Izolaci klientů</span><span class="sxs-lookup"><span data-stu-id="258ac-161">Tenant Isolation</span></span>
<span data-ttu-id="258ac-162">U rozsáhlejších, víceklientské nasazení některé společnosti vytvořit samostatné skupiny klientů na odlišné nasazení hardwaru back-end.</span><span class="sxs-lookup"><span data-stu-id="258ac-162">In larger, multi-tenant deployments some companies create separate groups of tenants on distinct deployments of backend hardware.</span></span> <span data-ttu-id="258ac-163">Tím se minimalizují počet zákazníci, kteří jsou ovlivněný problémem hardwaru na back-end.</span><span class="sxs-lookup"><span data-stu-id="258ac-163">This minimizes the number of customers who are impacted by a hardware issue on the backend.</span></span> <span data-ttu-id="258ac-164">Umožňuje také nová verze softwaru nasazen ve fázích.</span><span class="sxs-lookup"><span data-stu-id="258ac-164">It also enables new software versions to be rolled out in stages.</span></span> <span data-ttu-id="258ac-165">Tato architektura back-end v ideálním případě by měl být transparentní k příjemce rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="258ac-165">Ideally this backend architecture should be transparent to API consumers.</span></span> <span data-ttu-id="258ac-166">Toho lze dosáhnout podobným způsobem transparentní Správa verzí aplikace je založena na stejné techniky manipulace s adresu URL back-end pomocí konfigurace stav pro každý klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="258ac-166">This can be achieved in a similar way to transparent versioning because it is based on the same technique of manipulating the backend URL using configuration state per API key.</span></span>  

<span data-ttu-id="258ac-167">Místo vrácení upřednostňované verzi rozhraní API pro každý klíč předplatného, by vrátit identifikátor, který se týká klienta do skupiny přiřazené hardwaru.</span><span class="sxs-lookup"><span data-stu-id="258ac-167">Instead of returning a preferred version of the API for each subscription key, you would return an identifier that relates a tenant to the assigned hardware group.</span></span> <span data-ttu-id="258ac-168">Tento identifikátor slouží k vytvoření adresy URL vhodné back-end.</span><span class="sxs-lookup"><span data-stu-id="258ac-168">That identifier can be used to construct the appropriate backend URL.</span></span>

## <a name="summary"></a><span data-ttu-id="258ac-169">Souhrn</span><span class="sxs-lookup"><span data-stu-id="258ac-169">Summary</span></span>
<span data-ttu-id="258ac-170">Volný pomocí Azure API management mezipaměti pro ukládání libovolného typu dat umožňuje efektivní přístup k datům konfigurace, které mohou ovlivnit způsob zpracování příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="258ac-170">The freedom to use the Azure API management cache for storing any kind of data enables efficient access to configuration data that can affect the way an inbound request is processed.</span></span> <span data-ttu-id="258ac-171">Je také slouží k ukládání dat fragmenty, které můžete posílení odpovědi, vrácená z rozhraní API back-end.</span><span class="sxs-lookup"><span data-stu-id="258ac-171">It can also be used to store data fragments that can augment responses, returned from a backend API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="258ac-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="258ac-172">Next steps</span></span>
<span data-ttu-id="258ac-173">Prosím sdělit svůj názor v služby disqus pro toto téma Pokud existují další scénáře, které tyto zásady jste povolili pro vás, nebo pokud existují scénáře, kterou chcete dosáhnout, ale není myslíte, že je nyní možné.</span><span class="sxs-lookup"><span data-stu-id="258ac-173">Please give us your feedback in the Disqus thread for this topic if there are other scenarios that these policies have enabled for you, or if there are scenarios you would like to achieve but do not feel are currently possible.</span></span>

