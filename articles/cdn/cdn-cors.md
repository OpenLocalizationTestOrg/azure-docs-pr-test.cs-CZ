---
title: "Pomocí Azure CDN CORS | Microsoft Docs"
description: "Další informace o použití Azure Content Delivery Network (CDN) k s sdílení prostředků různých původů (CORS)."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 7070397f6e69b21add75bad8220f0b8ebe36d266
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-cdn-with-cors"></a><span data-ttu-id="e5ce5-103">Azure CDN pomocí CORS</span><span class="sxs-lookup"><span data-stu-id="e5ce5-103">Using Azure CDN with CORS</span></span>
## <a name="what-is-cors"></a><span data-ttu-id="e5ce5-104">Co je CORS?</span><span class="sxs-lookup"><span data-stu-id="e5ce5-104">What is CORS?</span></span>
<span data-ttu-id="e5ce5-105">CORS (mezi sdílení prostředků původu) je funkce protokolu HTTP, která umožňuje webové aplikace spuštěna v rámci jednoho domény přístup k prostředkům v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-105">CORS (Cross Origin Resource Sharing) is an HTTP feature that enables a web application running under one domain to access resources in another domain.</span></span> <span data-ttu-id="e5ce5-106">Chcete-li omezit možnost útoky skriptování mezi weby, všechny moderní prohlížeče implementovat omezení zabezpečení, označuje jako [stejného původu zásad](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span><span class="sxs-lookup"><span data-stu-id="e5ce5-106">In order to reduce the possibility of cross-site scripting attacks, all modern web browsers implement a security restriction known as [same-origin policy](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span></span>  <span data-ttu-id="e5ce5-107">Zabrání se tak na webové stránce z volání rozhraní API v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-107">This prevents a web page from calling APIs in a different domain.</span></span>  <span data-ttu-id="e5ce5-108">CORS poskytuje zabezpečení způsob, jak povolit jeden počátek (doménu původu) k volání rozhraní API v jiný počátek.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-108">CORS provides a secure way to allow one origin (the origin domain) to call APIs in another origin.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="e5ce5-109">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="e5ce5-109">How it works</span></span>
<span data-ttu-id="e5ce5-110">Existují dva typy požadavků CORS *jednoduchých požadavků* a *komplexní požadavky.*</span><span class="sxs-lookup"><span data-stu-id="e5ce5-110">There are two types of CORS requests, *simple requests* and *complex requests.*</span></span>

### <a name="for-simple-requests"></a><span data-ttu-id="e5ce5-111">Pro jednoduché požadavky:</span><span class="sxs-lookup"><span data-stu-id="e5ce5-111">For simple requests:</span></span>

1. <span data-ttu-id="e5ce5-112">Prohlížeč odešle požadavek CORS s další **původu** hlavičku požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-112">The browser sends the CORS request with an additional **Origin** HTTP request header.</span></span> <span data-ttu-id="e5ce5-113">Hodnotu této hlavičky je původ, který obsluhuje nadřazená stránka, která je definována jako kombinace *protokol,* *domény,* a *portu.*</span><span class="sxs-lookup"><span data-stu-id="e5ce5-113">The value of this header is the origin that served the parent page, which is defined as the combination of *protocol,* *domain,* and *port.*</span></span>  <span data-ttu-id="e5ce5-114">Když stránku z https://www.contoso.com pokusí o přístup k datům uživatele v původu fabrikam.com, bude odesláno následující hlavičky žádosti na fabrikam.com:</span><span class="sxs-lookup"><span data-stu-id="e5ce5-114">When a page from https://www.contoso.com attempts to access a user's data in the fabrikam.com origin, the following request header would be sent to fabrikam.com:</span></span>

   `Origin: https://www.contoso.com`

2. <span data-ttu-id="e5ce5-115">Server odpoví některé z následujících:</span><span class="sxs-lookup"><span data-stu-id="e5ce5-115">The server may respond with any of the following:</span></span>

   * <span data-ttu-id="e5ce5-116">**Access-Control-Allow-Origin** hlavičky v odpovědi označující původ lokality, která je povolena.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-116">An **Access-Control-Allow-Origin** header in its response indicating which origin site is allowed.</span></span> <span data-ttu-id="e5ce5-117">Například:</span><span class="sxs-lookup"><span data-stu-id="e5ce5-117">For example:</span></span>

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * <span data-ttu-id="e5ce5-118">Kód chyby HTTP například 403, není-li server žádost cross-origin po zkontrolování hlavičku zdroje</span><span class="sxs-lookup"><span data-stu-id="e5ce5-118">An HTTP error code such as 403 if the server does not allow the cross-origin request after checking the Origin header</span></span>

   * <span data-ttu-id="e5ce5-119">**Access-Control-Allow-Origin** záhlaví se zástupnými znaky, které umožňuje všechny původy:</span><span class="sxs-lookup"><span data-stu-id="e5ce5-119">An **Access-Control-Allow-Origin** header with a wildcard that allows all origins:</span></span>

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a><span data-ttu-id="e5ce5-120">Pro komplexní požadavky:</span><span class="sxs-lookup"><span data-stu-id="e5ce5-120">For complex requests:</span></span>

<span data-ttu-id="e5ce5-121">Žádost o komplexní je požadavek CORS, kterých se v prohlížeči vyžaduje k odeslání *předběžný požadavek* (tj. předběžné kontroly) před odesláním aktuální požadavek CORS.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-121">A complex request is a CORS request where the browser is required to send a *preflight request* (i.e. a preliminary probe) before sending the actual CORS request.</span></span> <span data-ttu-id="e5ce5-122">Předběžný požadavek požádá oprávnění k serveru, pokud žádost původní CORS můžete pokračovat a je `OPTIONS` požadavek na stejnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-122">The preflight request asks the server permission if the original CORS request can proceed and is an `OPTIONS` request to the same URL.</span></span>

> [!TIP]
> <span data-ttu-id="e5ce5-123">Další informace o CORS toky a běžné nástrahy, podívejte se [Průvodce CORS pro rozhraní REST API](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span><span class="sxs-lookup"><span data-stu-id="e5ce5-123">For more details on CORS flows and common pitfalls, view the [Guide to CORS for REST APIs](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span></span>
>
>

## <a name="wildcard-or-single-origin-scenarios"></a><span data-ttu-id="e5ce5-124">Zástupný znak nebo jeden počátek scénáře</span><span class="sxs-lookup"><span data-stu-id="e5ce5-124">Wildcard or single origin scenarios</span></span>
<span data-ttu-id="e5ce5-125">CORS v Azure CDN budou automaticky fungovat s žádná další konfigurace při **Access-Control-Allow-Origin** záhlaví je nastaven na zástupný znak (*) nebo jeden počátek.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-125">CORS on Azure CDN will work automatically with no additional configuration when the **Access-Control-Allow-Origin** header is set to wildcard (*) or a single origin.</span></span>  <span data-ttu-id="e5ce5-126">CDN se první odpověď do mezipaměti a následné žádosti bude používat stejné záhlaví.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-126">The CDN will cache the first response and subsequent requests will use the same header.</span></span>

<span data-ttu-id="e5ce5-127">Pokud již byly provedeny požadavky CDN před CORS se nastavuje na počátku, budete muset vymazat obsah na koncový bod obsah znovu načíst obsah s **Access-Control-Allow-Origin** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-127">If requests have already been made to the CDN prior to CORS being set on the your origin, you will need to purge content on your endpoint content to reload the content with the **Access-Control-Allow-Origin** header.</span></span>

## <a name="multiple-origin-scenarios"></a><span data-ttu-id="e5ce5-128">Více scénářů počátek</span><span class="sxs-lookup"><span data-stu-id="e5ce5-128">Multiple origin scenarios</span></span>
<span data-ttu-id="e5ce5-129">Pokud je potřeba povolit konkrétní seznam původů smí pro CORS, získat věcí trochu složitější.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-129">If you need to allow a specific list of origins to be allowed for CORS, things get a little more complicated.</span></span> <span data-ttu-id="e5ce5-130">Tento problém nastane, když CDN ukládá do mezipaměti **Access-Control-Allow-Origin** záhlaví pro první zdroj CORS.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-130">The problem occurs when the CDN caches the **Access-Control-Allow-Origin** header for the first CORS origin.</span></span>  <span data-ttu-id="e5ce5-131">Pokud jiný zdroj CORS provede následného požadavku, bude sloužit CDN uložená v mezipaměti **Access-Control-Allow-Origin** hlavičky, která nebude odpovídat.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-131">When a different CORS origin makes a subsequent request, the CDN will serve the cached **Access-Control-Allow-Origin** header, which won't match.</span></span>  <span data-ttu-id="e5ce5-132">Napravíte to tím několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-132">There are several ways to correct this.</span></span>

### <a name="azure-cdn-premium-from-verizon"></a><span data-ttu-id="e5ce5-133">Azure CDN Premium od Verizonu</span><span class="sxs-lookup"><span data-stu-id="e5ce5-133">Azure CDN Premium from Verizon</span></span>
<span data-ttu-id="e5ce5-134">Nejlepší způsob, jak povolit, je použití **Azure CDN Premium od společnosti Verizon**, který zpřístupňuje některé rozšířené funkce.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-134">The best way to enable this is to use **Azure CDN Premium from Verizon**, which exposes some advanced functionality.</span></span> 

<span data-ttu-id="e5ce5-135">Budete muset [vytvořit pravidlo](cdn-rules-engine.md) zkontrolujte **původu** hlavičky v požadavku.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-135">You'll need to [create a rule](cdn-rules-engine.md) to check the **Origin** header on the request.</span></span>  <span data-ttu-id="e5ce5-136">Pokud je platný původu, pravidla nastaví **Access-Control-Allow-Origin** hlavička s počátek zadaný v požadavku.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-136">If it's a valid origin, your rule will set the **Access-Control-Allow-Origin** header with the origin provided in the request.</span></span>  <span data-ttu-id="e5ce5-137">Pokud počátek zadané v **původu** hlavičky není povolena, by měl vynechejte pravidla **Access-Control-Allow-Origin** záhlaví, což způsobí, že prohlížeč tak, aby zamítal žádosti.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-137">If the origin specified in the **Origin** header is not allowed, your rule should omit the **Access-Control-Allow-Origin** header which will cause the browser to reject the request.</span></span> 

<span data-ttu-id="e5ce5-138">Existují dva způsoby, jak to provést pomocí stroj pravidel.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-138">There are two ways to do this with the rules engine.</span></span>  <span data-ttu-id="e5ce5-139">V obou případech **Access-Control-Allow-Origin** hlavička ze souboru zdrojový server je zcela ignorován, je CDN pravidla modul provádí kompletní správu povolené zdroje CORS.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-139">In both cases, the **Access-Control-Allow-Origin** header from the file's origin server is completely ignored, the CDN's rules engine completely manages the allowed CORS origins.</span></span>

#### <a name="one-regular-expression-with-all-valid-origins"></a><span data-ttu-id="e5ce5-140">Jeden regulární výraz s všechny původy platný</span><span class="sxs-lookup"><span data-stu-id="e5ce5-140">One regular expression with all valid origins</span></span>
<span data-ttu-id="e5ce5-141">V tomto případě vytvoříte regulární výraz, který obsahuje všechny zdroje, které chcete povolit:</span><span class="sxs-lookup"><span data-stu-id="e5ce5-141">In this case, you'll create a regular expression that includes all of the origins you want to allow:</span></span> 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> <span data-ttu-id="e5ce5-142">**Azure CDN společnosti Verizon** používá [Perl kompatibilní regulární výrazy](http://pcre.org/) jako jeho modul pro regulární výrazy.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-142">**Azure CDN from Verizon** uses [Perl Compatible Regular Expressions](http://pcre.org/) as its engine for regular expressions.</span></span>  <span data-ttu-id="e5ce5-143">Můžete použít nástroje, jako je [regulární výrazy 101](https://regex101.com/) ověření regulárního výrazu.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-143">You can use a tool like [Regular Expressions 101](https://regex101.com/) to validate your regular expression.</span></span>  <span data-ttu-id="e5ce5-144">Všimněte si, že znak "/" je platná v regulárních výrazech a nemusí být uvozené však uvozovací znaky tento znak se považuje za osvědčený postup a očekává se některé validátory regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-144">Note that the "/" character is valid in regular expressions and doesn't need to be escaped, however, escaping that character is considered a best practice and is expected by some regex validators.</span></span>
> 
> 

<span data-ttu-id="e5ce5-145">Pokud odpovídá regulárnímu výrazu, dojde k nahrazení pravidla **Access-Control-Allow-Origin** záhlaví (pokud existuje) z tohoto počátku s původ, který požadavek odeslal.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-145">If the regular expression matches, your rule will replace the **Access-Control-Allow-Origin** header (if any) from the origin with the origin that sent the request.</span></span>  <span data-ttu-id="e5ce5-146">Můžete také přidat další hlavičky CORS, jako například **přístup – ovládací prvek-Allow-Methods**.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-146">You can also add additional CORS headers, such as **Access-Control-Allow-Methods**.</span></span>

![Příklad pravidla s regulárním výrazem](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a><span data-ttu-id="e5ce5-148">Žádost o pravidlo záhlaví pro každý počátek.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-148">Request header rule for each origin.</span></span>
<span data-ttu-id="e5ce5-149">Místo regulární výrazy, můžete místo toho vytvořit samostatné pravidlo pro každý původ chcete povolit používání **zástupné hlavičky požadavku** [vyhovují podmínce](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="e5ce5-149">Rather than regular expressions, you can instead create a separate rule for each origin you wish to allow using the **Request Header Wildcard** [match condition](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span></span> <span data-ttu-id="e5ce5-150">Stejně jako v případě metody regulární výraz modul pravidel samostatně nastaví hlavičky CORS.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-150">As with the regular expression method, the rules engine alone sets the CORS headers.</span></span> 

![Příklad pravidla bez regulární výraz](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> <span data-ttu-id="e5ce5-152">V příkladu výše, použít zástupný znak * informuje stroj pravidel tak, aby odpovídaly HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-152">In the example above, the use of the wildcard character * tells the rules engine to match both HTTP and HTTPS.</span></span>
> 
> 

### <a name="azure-cdn-standard"></a><span data-ttu-id="e5ce5-153">Azure CDN Standard</span><span class="sxs-lookup"><span data-stu-id="e5ce5-153">Azure CDN Standard</span></span>
<span data-ttu-id="e5ce5-154">Na Azure CDN Standard profily, je použití pouze mechanismus povolit pro více zdroje bez použití zástupných znaků původu [řetězce dotazu do mezipaměti](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="e5ce5-154">On Azure CDN Standard profiles, the only mechanism to allow for multiple origins without the use of the wildcard origin is to use [query string caching](cdn-query-string.md).</span></span>  <span data-ttu-id="e5ce5-155">Budete muset povolit řetězec dotazu nastavení pro koncový bod CDN a potom pomocí řetězce dotazu jedinečné požadavky z každé povolené domény.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-155">You need to enable query string setting for the CDN endpoint and then use a unique query string for requests from each allowed domain.</span></span> <span data-ttu-id="e5ce5-156">To způsobí CDN ukládání do mezipaměti samostatný objekt pro každý jedinečný dotazu řetězec.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-156">Doing this will result in the CDN caching a separate object for each unique query string.</span></span> <span data-ttu-id="e5ce5-157">Tento postup však není ideální, protože výsledkem bude více kopií stejného souboru do mezipaměti na CDN.</span><span class="sxs-lookup"><span data-stu-id="e5ce5-157">This approach is not ideal, however, as it will result in multiple copies of the same file cached on the CDN.</span></span>  

