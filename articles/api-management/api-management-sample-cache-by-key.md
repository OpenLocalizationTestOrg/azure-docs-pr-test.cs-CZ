---
title: "aaaCustom ukládání do mezipaměti ve službě Azure API Management"
description: "Zjistěte, jak toocache položky podle klíče ve službě Azure API Management"
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
ms.openlocfilehash: 681380743c8c96af5d0a8e25948a8c0663e9fd35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-caching-in-azure-api-management"></a><span data-ttu-id="e03a7-103">Vlastní ukládání do mezipaměti ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="e03a7-103">Custom caching in Azure API Management</span></span>
<span data-ttu-id="e03a7-104">Služba Azure API Management má integrovanou podporu pro [ukládání do mezipaměti odpovědi HTTP](api-management-howto-cache.md) pomocí adresy URL prostředku hello jako klíč hello.</span><span class="sxs-lookup"><span data-stu-id="e03a7-104">Azure API Management service has built-in support for [HTTP response caching](api-management-howto-cache.md) using hello resource URL as hello key.</span></span> <span data-ttu-id="e03a7-105">Hello klíč mohou být upravena hlavičky žádosti pomocí hello `vary-by` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e03a7-105">hello key can be modified by request headers using hello `vary-by` properties.</span></span> <span data-ttu-id="e03a7-106">To je užitečné pro ukládání do mezipaměti celé odpovědi protokolu HTTP (neboli reprezentace), ale někdy je užitečné toojust mezipaměti část reprezentace.</span><span class="sxs-lookup"><span data-stu-id="e03a7-106">This is useful for caching entire HTTP responses (aka representations), but sometimes it is useful toojust cache a portion of a representation.</span></span> <span data-ttu-id="e03a7-107">Hello nové [hodnota mezipaměti vyhledávání](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) a [hodnota úložiště mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) zásady poskytují možnost hello toostore a načtení libovolný kusy data přímo z definice zásady.</span><span class="sxs-lookup"><span data-stu-id="e03a7-107">hello new [cache-lookup-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) and [cache-store-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) policies provide hello ability toostore and retrieve arbitrary pieces of data from within policy definitions.</span></span> <span data-ttu-id="e03a7-108">Tato možnost také přidá hodnotu toohello dříve zavedená [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zásad vzhledem k tomu, že teď můžete mezipaměti odpovědí z externích služeb.</span><span class="sxs-lookup"><span data-stu-id="e03a7-108">This ability also adds value toohello previously introduced [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy because you can now cache responses from external services.</span></span>

## <a name="architecture"></a><span data-ttu-id="e03a7-109">Architektura</span><span class="sxs-lookup"><span data-stu-id="e03a7-109">Architecture</span></span>
<span data-ttu-id="e03a7-110">Služba API Management používá datové mezipaměti sdíleného za klienta tak, aby při změně měřítka toomultiple jednotky budete stále dostávat přístup toohello stejná data jako do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e03a7-110">API Management service uses a shared per-tenant data cache so that, as you scale up toomultiple units you will still get access toohello same cached data.</span></span> <span data-ttu-id="e03a7-111">Při práci s více oblast nasazení existují však nezávislé mezipaměti v každém z oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="e03a7-111">However, when working with a multi-region deployment there are independent caches within each of hello regions.</span></span> <span data-ttu-id="e03a7-112">Kvůli toothis, je důležité toonot považovat hello mezipaměti jako úložiště dat, kde je jediný zdroj hello některé část informace.</span><span class="sxs-lookup"><span data-stu-id="e03a7-112">Due toothis, it is important toonot treat hello cache as a data store, where it is hello only source of some piece of information.</span></span> <span data-ttu-id="e03a7-113">Pokud nebyla a později se rozhodli tootake výhod nasazení s více oblast hello, zákazníkům, kteří cestují, může ztratit přístup k datům toothat do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e03a7-113">If you did, and later decided tootake advantage of hello multi-region deployment, then customers with users that travel may lose access toothat cached data.</span></span>

## <a name="fragment-caching"></a><span data-ttu-id="e03a7-114">Fragment ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="e03a7-114">Fragment caching</span></span>
<span data-ttu-id="e03a7-115">Existují určité případy, kde odpovědí nevrátila obsahovat některé část dat, která je nákladná toodetermine a ještě stále aktuální pro přiměřené době.</span><span class="sxs-lookup"><span data-stu-id="e03a7-115">There are certain cases where responses being returned contain some portion of data that is expensive toodetermine and yet remains fresh for a reasonable amount of time.</span></span> <span data-ttu-id="e03a7-116">Jako příklad zvažte službu vytvořené letecká společnost, která obsahuje informace týkající se rezervace letu, stav letu, atd. Pokud uživatel hello je členem programu hello airlines body, mají by také informace týkající se aktuální stav tootheir a vzdálenost nahromadění.</span><span class="sxs-lookup"><span data-stu-id="e03a7-116">As an example, consider a service built by an airline that provides information relating flight reservations, flight status, etc. If hello user is a member of hello airlines points program, they would also have information relating tootheir current status and mileage accumulated.</span></span> <span data-ttu-id="e03a7-117">Tyto informace související s uživatelem se pravděpodobně uloží do jiného systému, ale může být žádoucí tooinclude v odpovědi vrátí o letu stav a rezervace.</span><span class="sxs-lookup"><span data-stu-id="e03a7-117">This user-related information might be stored in a different system, but it may be desirable tooinclude it in responses returned about flight status and reservations.</span></span> <span data-ttu-id="e03a7-118">To lze provést pomocí procesu nazývaného fragment ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="e03a7-118">This can be done using a process called fragment caching.</span></span> <span data-ttu-id="e03a7-119">primární reprezentace Hello lze vrátit ze serveru počátek hello pomocí nějaký druh tokenu tooindicate, kde informace týkající se uživatele hello je toobe vložit.</span><span class="sxs-lookup"><span data-stu-id="e03a7-119">hello primary representation can be returned from hello origin server using some kind of token tooindicate where hello user-related information is toobe inserted.</span></span> 

<span data-ttu-id="e03a7-120">Vezměte v úvahu hello následující JSON odpověď z back-end rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e03a7-120">Consider hello following JSON response from a backend API.</span></span>

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

<span data-ttu-id="e03a7-121">A sekundární prostředek na úrovni `/userprofile/{userid}` jako,</span><span class="sxs-lookup"><span data-stu-id="e03a7-121">And secondary resource at `/userprofile/{userid}` that looks like,</span></span>

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

<span data-ttu-id="e03a7-122">V pořadí toodetermine hello odpovídající uživatelské informace tooinclude potřebujeme tooidentify, který je hello koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e03a7-122">In order toodetermine hello appropriate user information tooinclude, we need tooidentify who hello end user is.</span></span> <span data-ttu-id="e03a7-123">Tento mechanismus je závisí na implementaci.</span><span class="sxs-lookup"><span data-stu-id="e03a7-123">This mechanism is implementation dependent.</span></span> <span data-ttu-id="e03a7-124">Jako příklad používám hello `Subject` z deklarací identity `JWT` tokenu.</span><span class="sxs-lookup"><span data-stu-id="e03a7-124">As an example, I am using hello `Subject` claim of a `JWT` token.</span></span> 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

<span data-ttu-id="e03a7-125">Nemůžeme Uložit `enduserid` hodnotu v kontextu proměnné pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="e03a7-125">We store this `enduserid` value in a context variable for later use.</span></span> <span data-ttu-id="e03a7-126">dalším krokem Hello je toodetermine, pokud už na předchozí požadavek načíst informace o uživateli hello a uložené v mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="e03a7-126">hello next step is toodetermine if a previous request has already retrieved hello user information and stored it in hello cache.</span></span> <span data-ttu-id="e03a7-127">Pro tento používáme hello `cache-lookup-value` zásad.</span><span class="sxs-lookup"><span data-stu-id="e03a7-127">For this we use hello `cache-lookup-value` policy.</span></span>

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

<span data-ttu-id="e03a7-128">Pokud neexistuje žádná položka v mezipaměti hello, která odpovídá hodnota klíče toohello a ne `userprofile` vytvoří kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="e03a7-128">If there is no entry in hello cache that corresponds toohello key value, then no `userprofile` context variable will be created.</span></span> <span data-ttu-id="e03a7-129">Ověření, zda hello úspěšné vyhledání hello pomocí hello `choose` řízení toku zásad.</span><span class="sxs-lookup"><span data-stu-id="e03a7-129">We check hello success of hello lookup using hello `choose` control flow policy.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If hello userprofile context variable doesn’t exist, make an HTTP request tooretrieve it.  -->
    </when>
</choose>
```

<span data-ttu-id="e03a7-130">Pokud hello `userprofile` kontextové proměnné neexistuje, pak jsme tyto probíhající toohave toomake protokolu HTTP žádosti tooretrieve ho.</span><span class="sxs-lookup"><span data-stu-id="e03a7-130">If hello `userprofile` context variable doesn’t exist, then we are going toohave toomake an HTTP request tooretrieve it.</span></span>

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points toohello profile for hello current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="e03a7-131">Používáme hello `enduserid` tooconstruct hello URL toohello uživatelského profilu prostředku.</span><span class="sxs-lookup"><span data-stu-id="e03a7-131">We use hello `enduserid` tooconstruct hello URL toohello user profile resource.</span></span> <span data-ttu-id="e03a7-132">Jakmile jsme hello odpovědi, jsme hello základní text z odpovědi hello pro vyžádání obsahu a uloží jej zpět do kontextové proměnné.</span><span class="sxs-lookup"><span data-stu-id="e03a7-132">Once we have hello response, we can pull hello body text out of hello response and store it back into a context variable.</span></span>

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

<span data-ttu-id="e03a7-133">tooavoid nám s toomake tento požadavek HTTP znovu, pokud hello stejný uživatel zadá další žádost, lze uložit profil uživatele hello v mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="e03a7-133">tooavoid us having toomake this HTTP request again, when hello same user makes another request, we can store hello user profile in hello cache.</span></span>

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

<span data-ttu-id="e03a7-134">Uložíme hello hodnota v mezipaměti hello pomocí hello přesně stejný klíč jsme, že původně pokusil tooretrieve se službou.</span><span class="sxs-lookup"><span data-stu-id="e03a7-134">We store hello value in hello cache using hello exact same key that we originally attempted tooretrieve it with.</span></span> <span data-ttu-id="e03a7-135">Hello dobu, po kterou vybereme možnost toostore hello hodnota by měla být založena na tom, jak často hello informace o změny a jak odolný vůči chybám uživatelé jsou tooout informace o datu.</span><span class="sxs-lookup"><span data-stu-id="e03a7-135">hello duration that we choose toostore hello value should be based on how often hello information changes and how tolerant users are tooout of date information.</span></span> 

<span data-ttu-id="e03a7-136">Je důležité toorealize že načítání z mezipaměti hello je stále mimo proces, požadavku sítě a potenciálně můžete přesto přidat desítkami milisekundách toohello požadavku.</span><span class="sxs-lookup"><span data-stu-id="e03a7-136">It is important toorealize that retrieving from hello cache is still an out-of-process, network request and potentially can still add tens of milliseconds toohello request.</span></span> <span data-ttu-id="e03a7-137">výhody Hello pocházet pokud trvá výrazně delší, než je z důvodu tooneeding toodo databázové dotazy nebo agregační informace z více back EndY určení hello informace o uživatelském profilu.</span><span class="sxs-lookup"><span data-stu-id="e03a7-137">hello benefits come when determining hello user profile information takes significantly longer than that due tooneeding toodo database queries or aggregate information from multiple back-ends.</span></span>

<span data-ttu-id="e03a7-138">Hello posledním krokem v procesu hello je tooupdate hello vrátí odpověď se naše informace o profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="e03a7-138">hello final step in hello process is tooupdate hello returned response with our user profile information.</span></span>

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

<span data-ttu-id="e03a7-139">Vybrali jste tooinclude hello uvozovek jako součást tokenu hello tak, že i když nedojde, nahraďte text hello, hello odpověď byla stále platný kód JSON.</span><span class="sxs-lookup"><span data-stu-id="e03a7-139">I chose tooinclude hello quotation marks as part of hello token so that even when hello replace doesn’t occur, hello response was still valid JSON.</span></span> <span data-ttu-id="e03a7-140">To se primárně toomake snazší ladění.</span><span class="sxs-lookup"><span data-stu-id="e03a7-140">This was primarily toomake debugging easier.</span></span>

<span data-ttu-id="e03a7-141">Jakmile se společně sloučit všechny tyto kroky, je hello konečný výsledek zásadu, která vypadá jako hello následující jeden.</span><span class="sxs-lookup"><span data-stu-id="e03a7-141">Once you combine all these steps together, hello end result is a policy that looks like hello following one.</span></span>

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in hello cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If we don’t find it in hello cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request tooget user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points toohello profile for hello current end-user -->
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

<span data-ttu-id="e03a7-142">Tento přístup ukládání do mezipaměti se primárně používá v weby kde HTML tvoří na straně serveru hello tak, aby je možné vykreslit jako jednu stránku.</span><span class="sxs-lookup"><span data-stu-id="e03a7-142">This caching approach is primarily used in web sites where HTML is composed on hello server side so that it can be rendered as a single page.</span></span> <span data-ttu-id="e03a7-143">Však může také být užitečné v rozhraní API, kde klienti nemohou provádět klienta na straně ukládání do mezipaměti protokolu HTTP nebo je žádoucí není tooput tuto odpovědnost hello klienta.</span><span class="sxs-lookup"><span data-stu-id="e03a7-143">However, it can also be useful in APIs where clients cannot do client side HTTP caching or it is desirable not tooput that responsibility on hello client.</span></span>

<span data-ttu-id="e03a7-144">Tento stejný druh ukládání do mezipaměti fragment provést také v hello back-end webových serverů pomocí ukládání do mezipaměti serveru Redis, ale pomocí tooperform služby API Management hello tato práce je užitečné, když hello mezipaměti fragmenty pocházejí z různých back EndY než hello primární odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e03a7-144">This same kind of fragment caching can also be done on hello backend web servers using a Redis caching server, however, using hello API Management service tooperform this work is useful when hello cached fragments are coming from different back-ends than hello primary responses.</span></span>

## <a name="transparent-versioning"></a><span data-ttu-id="e03a7-145">Transparentní Správa verzí</span><span class="sxs-lookup"><span data-stu-id="e03a7-145">Transparent versioning</span></span>
<span data-ttu-id="e03a7-146">Je obvyklé, více verzí jinou implementaci rozhraní API toobe podporované v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="e03a7-146">It is common practice for multiple different implementation versions of an API toobe supported at any one time.</span></span> <span data-ttu-id="e03a7-147">Toto je možná toosupport různých prostředích, jako je vývojářů, testovací, produkční atd., nebo může být toosupport starší verze času toogive hello rozhraní API pro rozhraní API příjemci toomigrate toonewer verze.</span><span class="sxs-lookup"><span data-stu-id="e03a7-147">This is perhaps toosupport different environments, like dev, test, production, etc, or it may be toosupport older versions of hello API toogive time for API consumers toomigrate toonewer versions.</span></span> 

<span data-ttu-id="e03a7-148">Jeden ze způsobů toohandling tomto místo z nutnosti klienta vývojáři toochange hello adresy z `/v1/customers` příliš`/v2/customers` je toostore v hello příjemce data profilu, kterou verzi hello rozhraní API se v současné době chcete toouse a volání hello odpovídající Adresa URL back-end.</span><span class="sxs-lookup"><span data-stu-id="e03a7-148">One approach toohandling this instead of requiring client developers toochange hello URLs from `/v1/customers` too`/v2/customers` is toostore in hello consumer’s profile data which version of hello API they currently wish toouse and call hello appropriate backend URL.</span></span> <span data-ttu-id="e03a7-149">V aplikaci pořadí toodetermine hello správné back-end adresy URL toocall pro konkrétního klienta, je nutné tooquery některé konfigurační data.</span><span class="sxs-lookup"><span data-stu-id="e03a7-149">In order toodetermine hello correct backend URL toocall for a particular client, it is necessary tooquery some configuration data.</span></span> <span data-ttu-id="e03a7-150">Pomocí ukládání do mezipaměti tato konfigurační data, jsme minimalizovat snížení výkonu hello to toto vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="e03a7-150">By caching this configuration data, we can minimize hello performance penalty of doing this lookup.</span></span>

<span data-ttu-id="e03a7-151">prvním krokem Hello je, že identifikátor toodetermine hello používá tooconfigure hello požadovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="e03a7-151">hello first step is toodetermine hello identifier used tooconfigure hello desired version.</span></span> <span data-ttu-id="e03a7-152">V tomto příkladu vybrali jste klíč předplatného tooassociate hello verze toohello produktu.</span><span class="sxs-lookup"><span data-stu-id="e03a7-152">In this example, I chose tooassociate hello version toohello product subscription key.</span></span> 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

<span data-ttu-id="e03a7-153">Toosee vyhledávání mezipaměti jsme pak dělat, když jsme již byla načtena verze hello požadované klienta.</span><span class="sxs-lookup"><span data-stu-id="e03a7-153">We then do a cache lookup toosee if we already have retrieved hello desired client version.</span></span>

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

<span data-ttu-id="e03a7-154">Potom jsme zkontrolujte toosee, pokud jsme ho nebyl nalezen v mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="e03a7-154">Then we check toosee if we did not find it in hello cache.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

<span data-ttu-id="e03a7-155">Pokud jsme nebyla pak jsme přejděte a jejich načtení.</span><span class="sxs-lookup"><span data-stu-id="e03a7-155">If we didn’t then we go and retrieve it.</span></span>

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

<span data-ttu-id="e03a7-156">Extrahuje hello odpovědi základní text z odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="e03a7-156">Extract hello response body text from hello response.</span></span>

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

<span data-ttu-id="e03a7-157">Uložte ji zpět do hello mezipaměti pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="e03a7-157">Store it back in hello cache for future use.</span></span>

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

<span data-ttu-id="e03a7-158">A nakonec hello back-end adresy URL tooselect hello verze aktualizace požadované klientem hello hello služby.</span><span class="sxs-lookup"><span data-stu-id="e03a7-158">And finally update hello back-end URL tooselect hello version of hello service desired by hello client.</span></span>

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

<span data-ttu-id="e03a7-159">Hello úplně zásady je následující.</span><span class="sxs-lookup"><span data-stu-id="e03a7-159">hello completely policy is as follows.</span></span>

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in hello cache, make a request for it and store it -->
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

<span data-ttu-id="e03a7-160">Povolení rozhraní API příjemci tootransparently řízení které back-end verze je aktuálně používán klientů bez nutnosti tooupdate a znovu ho zaveďte klientů je elegantní řešení, které řeší mnoho aspekty správy verzí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e03a7-160">Enabling API consumers tootransparently control which backend version is being accessed by clients without having tooupdate and redeploy clients is a elegant solution that addresses many API versioning concerns.</span></span>

## <a name="tenant-isolation"></a><span data-ttu-id="e03a7-161">Izolaci klientů</span><span class="sxs-lookup"><span data-stu-id="e03a7-161">Tenant Isolation</span></span>
<span data-ttu-id="e03a7-162">U rozsáhlejších, víceklientské nasazení některé společnosti vytvořit samostatné skupiny klientů na odlišné nasazení hardwaru back-end.</span><span class="sxs-lookup"><span data-stu-id="e03a7-162">In larger, multi-tenant deployments some companies create separate groups of tenants on distinct deployments of backend hardware.</span></span> <span data-ttu-id="e03a7-163">Tím se minimalizují počet hello zákazníci, kteří jsou ovlivněný problémem hardwaru na back-end hello.</span><span class="sxs-lookup"><span data-stu-id="e03a7-163">This minimizes hello number of customers who are impacted by a hardware issue on hello backend.</span></span> <span data-ttu-id="e03a7-164">Umožňuje také nové toobe verze softwaru ve fázích.</span><span class="sxs-lookup"><span data-stu-id="e03a7-164">It also enables new software versions toobe rolled out in stages.</span></span> <span data-ttu-id="e03a7-165">Tato architektura back-end v ideálním případě by měl být transparentní tooAPI příjemci.</span><span class="sxs-lookup"><span data-stu-id="e03a7-165">Ideally this backend architecture should be transparent tooAPI consumers.</span></span> <span data-ttu-id="e03a7-166">Toho lze dosáhnout v podobně jako verze tootransparent způsob aplikace je založena na hello stejným způsobem manipulace s hello URL back-end pomocí konfigurace stav pro každý klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e03a7-166">This can be achieved in a similar way tootransparent versioning because it is based on hello same technique of manipulating hello backend URL using configuration state per API key.</span></span>  

<span data-ttu-id="e03a7-167">Místo vrácení upřednostňované verzi hello rozhraní API pro každý klíč předplatného, by vrátit identifikátor, který má vztah skupinu klienta toohello přiřazené hardwaru.</span><span class="sxs-lookup"><span data-stu-id="e03a7-167">Instead of returning a preferred version of hello API for each subscription key, you would return an identifier that relates a tenant toohello assigned hardware group.</span></span> <span data-ttu-id="e03a7-168">Tento identifikátor mohou být použité tooconstruct hello odpovídající back-end adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e03a7-168">That identifier can be used tooconstruct hello appropriate backend URL.</span></span>

## <a name="summary"></a><span data-ttu-id="e03a7-169">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e03a7-169">Summary</span></span>
<span data-ttu-id="e03a7-170">Hello volnost toouse hello Azure API management mezipaměti pro ukládání libovolného typu dat umožňuje efektivní přístup tooconfiguration data, která může mít vliv na způsob hello zpracování příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="e03a7-170">hello freedom toouse hello Azure API management cache for storing any kind of data enables efficient access tooconfiguration data that can affect hello way an inbound request is processed.</span></span> <span data-ttu-id="e03a7-171">Lze také použít toostore fragmenty dat, které můžete posílení odpovědi, vrácená z rozhraní API back-end.</span><span class="sxs-lookup"><span data-stu-id="e03a7-171">It can also be used toostore data fragments that can augment responses, returned from a backend API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e03a7-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e03a7-172">Next steps</span></span>
<span data-ttu-id="e03a7-173">Prosím sdělte svůj názor v hello vlákna služby Disqus pro toto téma případné další scénáře, tyto zásady mají povolený pro vás, nebo pokud existují scénáře byste chtěli tooachieve, ale není zaregistrované, jsou aktuálně možné.</span><span class="sxs-lookup"><span data-stu-id="e03a7-173">Please give us your feedback in hello Disqus thread for this topic if there are other scenarios that these policies have enabled for you, or if there are scenarios you would like tooachieve but do not feel are currently possible.</span></span>

