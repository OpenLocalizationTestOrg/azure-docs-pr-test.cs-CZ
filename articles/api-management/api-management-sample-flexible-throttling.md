---
title: "aaaAdvanced omezování požadavků pomocí Azure API Management"
description: "Zjistěte, jak toocreate a použít flexibilní kvóty a míra omezení zásad Azure API Management."
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
ms.openlocfilehash: ac87f83118a37bd587fddf044e5c2d6fc2af9031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a><span data-ttu-id="2964d-103">Pokročilé omezování požadavků pomocí Azure API Management</span><span class="sxs-lookup"><span data-stu-id="2964d-103">Advanced request throttling with Azure API Management</span></span>
<span data-ttu-id="2964d-104">Je možné toothrottle příchozí požadavky je klíčovou roli služby Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="2964d-104">Being able toothrottle incoming requests is a key role of Azure API Management.</span></span> <span data-ttu-id="2964d-105">Buď řízení hello míra požadavků nebo data celkový počet požadavků hello přenesen, umožňuje rozhraní API správy rozhraní API zprostředkovatelů tooprotect příslušných rozhraní API z zneužití a vytvořit hodnotu pro různé úrovně rozhraní API produktu.</span><span class="sxs-lookup"><span data-stu-id="2964d-105">Either by controlling hello rate of requests or hello total requests/data transferred, API Management allows API providers tooprotect their APIs from abuse and create value for different API product tiers.</span></span>

## <a name="product-based-throttling"></a><span data-ttu-id="2964d-106">Omezování na základě produktu</span><span class="sxs-lookup"><span data-stu-id="2964d-106">Product based throttling</span></span>
<span data-ttu-id="2964d-107">toodate hello míra možnosti omezování byly omezené toobeing obor tooa určitý produkt odběr (v podstatě klíč), definované v hello portál vydavatele API Management.</span><span class="sxs-lookup"><span data-stu-id="2964d-107">toodate, hello rate throttling capabilities have been limited toobeing scoped tooa particular Product subscription (essentially a key), defined in hello API Management publisher portal.</span></span> <span data-ttu-id="2964d-108">To je užitečné pro hello rozhraní API poskytovatele tooapply omezení hello vývojáře, kteří zaregistrovali toouse jejich rozhraní API, ale jeho nepomůže, například v omezení jednotlivých koncoví uživatelé hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2964d-108">This is useful for hello API provider tooapply limits on hello developers who have signed up toouse their API, however, it does not help, for example, in throttling individual end-users of hello API.</span></span> <span data-ttu-id="2964d-109">Je možné, že pro jednoho uživatele tooconsume aplikace hello vývojáře hello celý kvóty a pak zabránit ostatním zákazníkům hello vývojáře aplikací mít toouse hello.</span><span class="sxs-lookup"><span data-stu-id="2964d-109">It is possible that for single user of hello developer's application tooconsume hello entire quota and then prevent other customers of hello developer from being able toouse hello application.</span></span> <span data-ttu-id="2964d-110">Navíc několik zákazníci, kteří mohou vytvořit k velkému počtu požadavků může omezit přístup toooccasional uživatele.</span><span class="sxs-lookup"><span data-stu-id="2964d-110">Also, several customers who might generate a high volume of requests may limit access toooccasional users.</span></span>

## <a name="custom-key-based-throttling"></a><span data-ttu-id="2964d-111">Vlastní klíč na základě omezení</span><span class="sxs-lookup"><span data-stu-id="2964d-111">Custom key based throttling</span></span>
<span data-ttu-id="2964d-112">Hello nové [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady poskytují ovládacího prvku výrazně flexibilnější tootraffic řešení.</span><span class="sxs-lookup"><span data-stu-id="2964d-112">hello new [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies provide a significantly more flexible solution tootraffic control.</span></span> <span data-ttu-id="2964d-113">Tyto nové zásady umožňují toodefine výrazy tooidentify hello klíče, které budou použité tootrack využití provozu.</span><span class="sxs-lookup"><span data-stu-id="2964d-113">These new policies allow you toodefine expressions tooidentify hello keys that will be used tootrack traffic usage.</span></span> <span data-ttu-id="2964d-114">Hello tak, jak to funguje je nejsnažší zobrazené na příkladu.</span><span class="sxs-lookup"><span data-stu-id="2964d-114">hello way this works is easiest illustrated with an example.</span></span> 

## <a name="ip-address-throttling"></a><span data-ttu-id="2964d-115">Omezení IP adres</span><span class="sxs-lookup"><span data-stu-id="2964d-115">IP Address throttling</span></span>
<span data-ttu-id="2964d-116">Hello následující zásady omezení jednoho klienta IP adresu tooonly 10 volání každou minutu, celkem 1 000 000 volání a 10 000 kB šířky pásma za měsíc.</span><span class="sxs-lookup"><span data-stu-id="2964d-116">hello following policies restrict a single client IP address tooonly 10 calls every minute, with a total of 1,000,000 calls and 10,000 kilobytes of bandwidth per month.</span></span> 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

<span data-ttu-id="2964d-117">Pokud všechny klienty v Internetu hello použili jedinečnou IP adresu, může se jednat o účinný způsob omezení využití podle uživatele.</span><span class="sxs-lookup"><span data-stu-id="2964d-117">If all clients on hello Internet used a unique IP address, this might be an effective way of limiting usage by user.</span></span> <span data-ttu-id="2964d-118">Je však velmi pravděpodobné, že více uživatelů se jednu veřejnou IP adresu z důvodu toothem přístupem hello Internet prostřednictvím zařízení NAT pro sdílení.</span><span class="sxs-lookup"><span data-stu-id="2964d-118">However, it is quite likely that multiple users will sharing a single public IP address due toothem accessing hello Internet via a NAT device.</span></span> <span data-ttu-id="2964d-119">Bez ohledu na to, pro rozhraní API umožňující přístup bez ověřování hello `IpAddress` může být nejlepší možnost hello.</span><span class="sxs-lookup"><span data-stu-id="2964d-119">Despite this, for APIs that allow unauthenticated access hello `IpAddress` might be hello best option.</span></span>

## <a name="user-identity-throttling"></a><span data-ttu-id="2964d-120">Omezení identity uživatele</span><span class="sxs-lookup"><span data-stu-id="2964d-120">User identity throttling</span></span>
<span data-ttu-id="2964d-121">Pokud koncový uživatel je ověřen a omezení klíč lze generovat na základě informací, která jednoznačně identifikuje, který uživatel.</span><span class="sxs-lookup"><span data-stu-id="2964d-121">If an end user is authenticated then a throttling key can be generated based on information that uniquely identifies an that user.</span></span>

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

<span data-ttu-id="2964d-122">V tomto příkladu jsme extrahovat hello autorizační hlavičky, převeďte ho příliš`JWT` objektu a použít hello subjektu hello tokenu tooidentify hello uživatele a použít jej v hello míru omezení klíč.</span><span class="sxs-lookup"><span data-stu-id="2964d-122">In this example we extract hello Authorization header, convert it too`JWT` object and use hello subject of hello token tooidentify hello user and use that as hello rate limiting key.</span></span> <span data-ttu-id="2964d-123">Pokud je identita uživatele hello je uložen v hello `JWT` jako jeden z hello další deklarace identity pak hodnota může použije na příslušné místo.</span><span class="sxs-lookup"><span data-stu-id="2964d-123">If hello user identity is stored in hello `JWT` as one of hello other claims then that value could be used in its place.</span></span>

## <a name="combined-policies"></a><span data-ttu-id="2964d-124">Kombinovaná zásady</span><span class="sxs-lookup"><span data-stu-id="2964d-124">Combined policies</span></span>
<span data-ttu-id="2964d-125">Přestože hello nové omezení zásady poskytují větší možnosti než hello existující omezení zásad, je stále hodnota kombinace obou možností.</span><span class="sxs-lookup"><span data-stu-id="2964d-125">Although hello new throttling policies provide more control than hello existing throttling policies, there is still value combining both capabilities.</span></span> <span data-ttu-id="2964d-126">Omezování klíč předplatného produktu ([omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) je skvělým způsobem, tooenable monetizing rozhraní API účtováním podle úrovně využití.</span><span class="sxs-lookup"><span data-stu-id="2964d-126">Throttling by product subscription key ([Limit call rate by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is a great way tooenable monetizing of an API by charging based on usage levels.</span></span> <span data-ttu-id="2964d-127">Hello přesnější možnosti řízení způsobená možné toothrottle uživatelem je doplňkové a zabrání dlouhodobější snížení kvality hello prostředí jiného chování jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="2964d-127">hello finer grained control of being able toothrottle by user is complementary and prevents one user's behavior from degrading hello experience of another.</span></span> 

## <a name="client-driven-throttling"></a><span data-ttu-id="2964d-128">Řízené omezení klienta</span><span class="sxs-lookup"><span data-stu-id="2964d-128">Client driven throttling</span></span>
<span data-ttu-id="2964d-129">Když hello omezení klíče definovaná pomocí [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx), pak je hello rozhraní API poskytovatele, který je výběr, jak má obor hello omezení.</span><span class="sxs-lookup"><span data-stu-id="2964d-129">When hello throttling key is defined using a [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx), then it is hello API provider that is choosing how hello throttling is scoped.</span></span> <span data-ttu-id="2964d-130">Však může být vhodné vývojář toocontrol jak ohodnotili omezit vlastní zákazníků.</span><span class="sxs-lookup"><span data-stu-id="2964d-130">However, a developer might want toocontrol how they rate limit their own customers.</span></span> <span data-ttu-id="2964d-131">To může být povolený poskytovatelem rozhraní API hello zavedením vlastní hlavičky tooallow hello vývojář na klienta aplikace toocommunicate hello klíče toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2964d-131">This could be enabled by hello API provider by introducing a custom header tooallow hello developer's client application toocommunicate hello key toohello API.</span></span>

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

<span data-ttu-id="2964d-132">To umožňuje hello vývojáře klienta aplikace toochoose jak chtějí toocreate hello míru omezení klíč.</span><span class="sxs-lookup"><span data-stu-id="2964d-132">This enables hello developer's client application toochoose how they want toocreate hello rate limiting key.</span></span> <span data-ttu-id="2964d-133">Pomocí jenom trocha vynalézavosti vývojář klienta vytvářet své vlastní míry vrstev přidělením sady toousers klíče a otáčení hello použití klíče.</span><span class="sxs-lookup"><span data-stu-id="2964d-133">With a little bit of ingenuity a client developer could create their own rate tiers by allocating sets of keys toousers and rotating hello key usage.</span></span>

## <a name="summary"></a><span data-ttu-id="2964d-134">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2964d-134">Summary</span></span>
<span data-ttu-id="2964d-135">Azure API Management nabízí rychlost a uvozovky omezení tooboth chránit a přidejte hodnotu tooyour rozhraní API služby.</span><span class="sxs-lookup"><span data-stu-id="2964d-135">Azure API Management provides rate and quote throttling tooboth protect and add value tooyour API service.</span></span> <span data-ttu-id="2964d-136">Hello nové omezení zásad pomocí vlastní oboru pravidla povolit, že je lepší kontrolu nad tooenable tyto zásady podrobných aplikace ještě lepší toobuild zákazníků.</span><span class="sxs-lookup"><span data-stu-id="2964d-136">hello new throttling policies with custom scoping rules allow you finer grained control over those policies tooenable your customers toobuild even better applications.</span></span> <span data-ttu-id="2964d-137">Hello příklady v tomto článku ukazují použití hello tyto nové zásady ve výrobním míru omezení klíče s klientských IP adres, klient vygeneruje hodnoty a identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="2964d-137">hello examples in this article demonstrate hello use of these new policies by manufacturing rate limiting keys with client IP addresses, user identity, and client generated values.</span></span> <span data-ttu-id="2964d-138">Existují však mnoho dalších částí uvítací zprávu, která by bylo možné použít jako uživatelský agent, fragmenty cestu adresy URL, velikost zprávy.</span><span class="sxs-lookup"><span data-stu-id="2964d-138">However, there are many other parts of hello message that could be used such as user agent, URL path fragments, message size.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2964d-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2964d-139">Next steps</span></span>
<span data-ttu-id="2964d-140">Prosím sdělte svůj názor v hello vlákna služby Disqus pro toto téma.</span><span class="sxs-lookup"><span data-stu-id="2964d-140">Please give us your feedback in hello Disqus thread for this topic.</span></span> <span data-ttu-id="2964d-141">Je skvělým toohear o jiné potenciální klíče hodnoty, které byly logické výběru v vaše scénáře.</span><span class="sxs-lookup"><span data-stu-id="2964d-141">It would be great toohear about other potential key values that have been a logical choice in your scenarios.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="2964d-142">Podívejte se na video s přehledem těchto zásad</span><span class="sxs-lookup"><span data-stu-id="2964d-142">Watch a video overview of these policies</span></span>
<span data-ttu-id="2964d-143">Další informace o hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady popsaná v tomto článku, podívejte se prosím hello následující video.</span><span class="sxs-lookup"><span data-stu-id="2964d-143">For more information on hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies covered in this article, please watch hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

