---
title: "Pokročilé omezování požadavků pomocí Azure API Management"
description: "Zjistěte, jak vytvořit a použít flexibilní kvóty a míra omezení zásad Azure API Management."
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 35375e599891a9443a91c4c3a8657e8c9c48c7b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a><span data-ttu-id="9ff40-103">Pokročilé omezování požadavků pomocí Azure API Management</span><span class="sxs-lookup"><span data-stu-id="9ff40-103">Advanced request throttling with Azure API Management</span></span>
<span data-ttu-id="9ff40-104">Schopnost omezení příchozí požadavky je klíčovou roli služby Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="9ff40-104">Being able to throttle incoming requests is a key role of Azure API Management.</span></span> <span data-ttu-id="9ff40-105">Buď kontrolou rychlost žádostí a celkový počet požadavků/přenášená data správy rozhraní API umožňuje zprostředkovatelé rozhraní API do příslušných rozhraní API umožňuje chránit proti zneužití a vytvořit hodnotu pro různé úrovně rozhraní API produktu.</span><span class="sxs-lookup"><span data-stu-id="9ff40-105">Either by controlling the rate of requests or the total requests/data transferred, API Management allows API providers to protect their APIs from abuse and create value for different API product tiers.</span></span>

## <a name="product-based-throttling"></a><span data-ttu-id="9ff40-106">Omezování na základě produktu</span><span class="sxs-lookup"><span data-stu-id="9ff40-106">Product based throttling</span></span>
<span data-ttu-id="9ff40-107">Aktuální rychlost omezení možnosti se omezená na se obor pro určitý odběr produktu (v podstatě klíč), definované v portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="9ff40-107">To date, the rate throttling capabilities have been limited to being scoped to a particular Product subscription (essentially a key), defined in the API Management publisher portal.</span></span> <span data-ttu-id="9ff40-108">To je užitečné pro poskytovatele rozhraní API za účelem použití omezení na vývojáři, kteří zaregistrovali používat své rozhraní API, ale jeho nepomůže, například v omezení jednotlivých koncoví uživatelé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9ff40-108">This is useful for the API provider to apply limits on the developers who have signed up to use their API, however, it does not help, for example, in throttling individual end-users of the API.</span></span> <span data-ttu-id="9ff40-109">Je možné, že pro jednotné uživatelské aplikace pro vývojáře spotřebovat celý kvóty a pak zabránit ostatním zákazníkům vývojáři mohli k používání aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff40-109">It is possible that for single user of the developer's application to consume the entire quota and then prevent other customers of the developer from being able to use the application.</span></span> <span data-ttu-id="9ff40-110">Několik zákazníci, kteří mohou vytvořit velký objem požadavků může také omezit přístup k příležitostně uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ff40-110">Also, several customers who might generate a high volume of requests may limit access to occasional users.</span></span>

## <a name="custom-key-based-throttling"></a><span data-ttu-id="9ff40-111">Vlastní klíč na základě omezení</span><span class="sxs-lookup"><span data-stu-id="9ff40-111">Custom key based throttling</span></span>
<span data-ttu-id="9ff40-112">Nové [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady poskytují výrazně flexibilnější řešení pro řízení provozu.</span><span class="sxs-lookup"><span data-stu-id="9ff40-112">The new [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies provide a significantly more flexible solution to traffic control.</span></span> <span data-ttu-id="9ff40-113">Tyto nové zásady umožňují definovat výrazy k identifikaci klíče, které se používají ke sledování využití provozu.</span><span class="sxs-lookup"><span data-stu-id="9ff40-113">These new policies allow you to define expressions to identify the keys that will be used to track traffic usage.</span></span> <span data-ttu-id="9ff40-114">Způsob, jakým tento postup funguje je nejsnažší zobrazené na příkladu.</span><span class="sxs-lookup"><span data-stu-id="9ff40-114">The way this works is easiest illustrated with an example.</span></span> 

## <a name="ip-address-throttling"></a><span data-ttu-id="9ff40-115">Omezení IP adres</span><span class="sxs-lookup"><span data-stu-id="9ff40-115">IP Address throttling</span></span>
<span data-ttu-id="9ff40-116">Tyto zásady omezení na IP adresu pro jednoho klienta k jenom 10 volání každou minutu, s celkem 1 000 000 volání a 10 000 kB šířky pásma za měsíc.</span><span class="sxs-lookup"><span data-stu-id="9ff40-116">The following policies restrict a single client IP address to only 10 calls every minute, with a total of 1,000,000 calls and 10,000 kilobytes of bandwidth per month.</span></span> 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

<span data-ttu-id="9ff40-117">Pokud všichni klienti na Internetu použili jedinečnou IP adresu, může se jednat o účinný způsob omezení využití podle uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ff40-117">If all clients on the Internet used a unique IP address, this might be an effective way of limiting usage by user.</span></span> <span data-ttu-id="9ff40-118">Je však velmi pravděpodobné, že více uživatelů se jednu veřejnou IP adresu z důvodu je přístup k Internetu prostřednictvím zařízení NAT pro sdílení.</span><span class="sxs-lookup"><span data-stu-id="9ff40-118">However, it is quite likely that multiple users will sharing a single public IP address due to them accessing the Internet via a NAT device.</span></span> <span data-ttu-id="9ff40-119">Bez ohledu na to, pro rozhraní API umožňující přístup bez ověřování `IpAddress` může být vhodné.</span><span class="sxs-lookup"><span data-stu-id="9ff40-119">Despite this, for APIs that allow unauthenticated access the `IpAddress` might be the best option.</span></span>

## <a name="user-identity-throttling"></a><span data-ttu-id="9ff40-120">Omezení identity uživatele</span><span class="sxs-lookup"><span data-stu-id="9ff40-120">User identity throttling</span></span>
<span data-ttu-id="9ff40-121">Pokud koncový uživatel je ověřen a omezení klíč lze generovat na základě informací, která jednoznačně identifikuje, který uživatel.</span><span class="sxs-lookup"><span data-stu-id="9ff40-121">If an end user is authenticated then a throttling key can be generated based on information that uniquely identifies an that user.</span></span>

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

<span data-ttu-id="9ff40-122">V tomto příkladu jsme extrahovat autorizační hlavičky, převeďte ho na `JWT` objektu a použít k identifikaci uživatele a použít je jako míru omezení klíč subjektu tokenu.</span><span class="sxs-lookup"><span data-stu-id="9ff40-122">In this example we extract the Authorization header, convert it to `JWT` object and use the subject of the token to identify the user and use that as the rate limiting key.</span></span> <span data-ttu-id="9ff40-123">Pokud je identita uživatele je uložen v `JWT` jako jednu z dalších deklarací pak hodnota může použije na příslušné místo.</span><span class="sxs-lookup"><span data-stu-id="9ff40-123">If the user identity is stored in the `JWT` as one of the other claims then that value could be used in its place.</span></span>

## <a name="combined-policies"></a><span data-ttu-id="9ff40-124">Kombinovaná zásady</span><span class="sxs-lookup"><span data-stu-id="9ff40-124">Combined policies</span></span>
<span data-ttu-id="9ff40-125">I když nové omezení zásady poskytují větší možnosti než existující zásady omezení, je stále hodnota kombinace obou možností.</span><span class="sxs-lookup"><span data-stu-id="9ff40-125">Although the new throttling policies provide more control than the existing throttling policies, there is still value combining both capabilities.</span></span> <span data-ttu-id="9ff40-126">Omezování klíč předplatného produktu ([omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) je skvělým způsobem, jak povolit monetizing účtováním podle úrovně využití rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9ff40-126">Throttling by product subscription key ([Limit call rate by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is a great way to enable monetizing of an API by charging based on usage levels.</span></span> <span data-ttu-id="9ff40-127">Přesnější možnosti řízení moci omezení uživatelem je doplňkové a zabrání docházelo k omezení prostředí jiného chování jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ff40-127">The finer grained control of being able to throttle by user is complementary and prevents one user's behavior from degrading the experience of another.</span></span> 

## <a name="client-driven-throttling"></a><span data-ttu-id="9ff40-128">Řízené omezení klienta</span><span class="sxs-lookup"><span data-stu-id="9ff40-128">Client driven throttling</span></span>
<span data-ttu-id="9ff40-129">Když je omezení klíče definovaná pomocí [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx), pak je rozhraní API zprostředkovatele, který je výběr, jak je vymezen omezení.</span><span class="sxs-lookup"><span data-stu-id="9ff40-129">When the throttling key is defined using a [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx), then it is the API provider that is choosing how the throttling is scoped.</span></span> <span data-ttu-id="9ff40-130">Nicméně vývojář možné řídit, jak se omezení přenosové rychlosti vlastní zákazníků.</span><span class="sxs-lookup"><span data-stu-id="9ff40-130">However, a developer might want to control how they rate limit their own customers.</span></span> <span data-ttu-id="9ff40-131">To může lze povolit zprostředkovatelem rozhraní API zavedením vlastní hlavičku pro vývojáře klientská aplikace komunikovat klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9ff40-131">This could be enabled by the API provider by introducing a custom header to allow the developer's client application to communicate the key to the API.</span></span>

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

<span data-ttu-id="9ff40-132">To umožňuje vývojáře pro klientské aplikace zvolte, jak se mají vytvořit nejpomalejší klíč.</span><span class="sxs-lookup"><span data-stu-id="9ff40-132">This enables the developer's client application to choose how they want to create the rate limiting key.</span></span> <span data-ttu-id="9ff40-133">Pomocí jenom trocha vynalézavosti vývojář klienta vytvářet své vlastní míry vrstev přidělením sady klíčů pro uživatele a otáčení použití klíče.</span><span class="sxs-lookup"><span data-stu-id="9ff40-133">With a little bit of ingenuity a client developer could create their own rate tiers by allocating sets of keys to users and rotating the key usage.</span></span>

## <a name="summary"></a><span data-ttu-id="9ff40-134">Souhrn</span><span class="sxs-lookup"><span data-stu-id="9ff40-134">Summary</span></span>
<span data-ttu-id="9ff40-135">Azure API Management nabízí rychlost a uvozovky, omezení pro ochranu i přidejte hodnotu do vašeho rozhraní API služby.</span><span class="sxs-lookup"><span data-stu-id="9ff40-135">Azure API Management provides rate and quote throttling to both protect and add value to your API service.</span></span> <span data-ttu-id="9ff40-136">Nové zásady omezení s vlastní oboru pravidla povolit tyto zásady pro vaše zákazníkům umožňují vytvářet ještě lepší aplikací přesnější možnosti řízení.</span><span class="sxs-lookup"><span data-stu-id="9ff40-136">The new throttling policies with custom scoping rules allow you finer grained control over those policies to enable your customers to build even better applications.</span></span> <span data-ttu-id="9ff40-137">V příkladech v tomto článku ukazují použití tyto nové zásady ve výrobním míru omezení klíče s klientských IP adres, klient vygeneruje hodnoty a identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ff40-137">The examples in this article demonstrate the use of these new policies by manufacturing rate limiting keys with client IP addresses, user identity, and client generated values.</span></span> <span data-ttu-id="9ff40-138">Existují však mnoho dalších částí zprávy, která by bylo možné použít jako uživatelský agent, fragmenty cestu adresy URL, velikost zprávy.</span><span class="sxs-lookup"><span data-stu-id="9ff40-138">However, there are many other parts of the message that could be used such as user agent, URL path fragments, message size.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ff40-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ff40-139">Next steps</span></span>
<span data-ttu-id="9ff40-140">Prosím sdělte svůj názor v služby disqus pro toto téma.</span><span class="sxs-lookup"><span data-stu-id="9ff40-140">Please give us your feedback in the Disqus thread for this topic.</span></span> <span data-ttu-id="9ff40-141">Budeme velmi uslyšíme jiné potenciální klíče hodnoty, které byly logické výběru v vaše scénáře.</span><span class="sxs-lookup"><span data-stu-id="9ff40-141">It would be great to hear about other potential key values that have been a logical choice in your scenarios.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="9ff40-142">Podívejte se na video s přehledem těchto zásad</span><span class="sxs-lookup"><span data-stu-id="9ff40-142">Watch a video overview of these policies</span></span>
<span data-ttu-id="9ff40-143">Další informace o [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady popsaná v tomto článku prosím v následujícím videu.</span><span class="sxs-lookup"><span data-stu-id="9ff40-143">For more information on the [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies covered in this article, please watch the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

