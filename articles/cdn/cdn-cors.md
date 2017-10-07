---
title: aaaUsing Azure CDN s CORS | Microsoft Docs
description: "Zjistěte, jak toouse hello Azure Content Delivery Network (CDN) toowith sdílení prostředků různých původů (CORS)."
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
ms.openlocfilehash: 6c743b56c32a2d3aacc9a77094cb87d61b95d2f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-cdn-with-cors"></a><span data-ttu-id="0db32-103">Azure CDN pomocí CORS</span><span class="sxs-lookup"><span data-stu-id="0db32-103">Using Azure CDN with CORS</span></span>
## <a name="what-is-cors"></a><span data-ttu-id="0db32-104">Co je CORS?</span><span class="sxs-lookup"><span data-stu-id="0db32-104">What is CORS?</span></span>
<span data-ttu-id="0db32-105">CORS (mezi sdílení prostředků původu) je funkce protokolu HTTP, která umožňuje webová aplikace spuštěna v rámci jednoho prostředkům tooaccess domény v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="0db32-105">CORS (Cross Origin Resource Sharing) is an HTTP feature that enables a web application running under one domain tooaccess resources in another domain.</span></span> <span data-ttu-id="0db32-106">V pořadí tooreduce hello možnost útoky skriptování mezi weby, všechny moderní prohlížeče implementovat omezení zabezpečení známé jako [stejného původu zásad](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span><span class="sxs-lookup"><span data-stu-id="0db32-106">In order tooreduce hello possibility of cross-site scripting attacks, all modern web browsers implement a security restriction known as [same-origin policy](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span></span>  <span data-ttu-id="0db32-107">Zabrání se tak na webové stránce z volání rozhraní API v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="0db32-107">This prevents a web page from calling APIs in a different domain.</span></span>  <span data-ttu-id="0db32-108">CORS poskytuje bezpečný tooallow jeden počátek (doménu původu hello) toocall rozhraní API v jiný počátek.</span><span class="sxs-lookup"><span data-stu-id="0db32-108">CORS provides a secure way tooallow one origin (hello origin domain) toocall APIs in another origin.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="0db32-109">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="0db32-109">How it works</span></span>
<span data-ttu-id="0db32-110">Existují dva typy požadavků CORS *jednoduchých požadavků* a *komplexní požadavky.*</span><span class="sxs-lookup"><span data-stu-id="0db32-110">There are two types of CORS requests, *simple requests* and *complex requests.*</span></span>

### <a name="for-simple-requests"></a><span data-ttu-id="0db32-111">Pro jednoduché požadavky:</span><span class="sxs-lookup"><span data-stu-id="0db32-111">For simple requests:</span></span>

1. <span data-ttu-id="0db32-112">Hello prohlížeč odešle požadavek CORS hello s další **původu** hlavičku požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="0db32-112">hello browser sends hello CORS request with an additional **Origin** HTTP request header.</span></span> <span data-ttu-id="0db32-113">Hello hodnotu této hlavičky je hello původ, který obsluhuje hello nadřazená stránka, která je definována jako kombinace hello *protokol,* *domény,* a *portu.*</span><span class="sxs-lookup"><span data-stu-id="0db32-113">hello value of this header is hello origin that served hello parent page, which is defined as hello combination of *protocol,* *domain,* and *port.*</span></span>  <span data-ttu-id="0db32-114">Když se na stránce z https://www.contoso.com pokusí tooaccess uživatelská data v hello fabrikam.com původu, mají být odeslány toofabrikam.com hello následující hlavičky žádosti:</span><span class="sxs-lookup"><span data-stu-id="0db32-114">When a page from https://www.contoso.com attempts tooaccess a user's data in hello fabrikam.com origin, hello following request header would be sent toofabrikam.com:</span></span>

   `Origin: https://www.contoso.com`

2. <span data-ttu-id="0db32-115">Hello server může reagovat s žádným z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="0db32-115">hello server may respond with any of hello following:</span></span>

   * <span data-ttu-id="0db32-116">**Access-Control-Allow-Origin** hlavičky v odpovědi označující původ lokality, která je povolena.</span><span class="sxs-lookup"><span data-stu-id="0db32-116">An **Access-Control-Allow-Origin** header in its response indicating which origin site is allowed.</span></span> <span data-ttu-id="0db32-117">Například:</span><span class="sxs-lookup"><span data-stu-id="0db32-117">For example:</span></span>

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * <span data-ttu-id="0db32-118">Chyby protokolu HTTP code například 403, není-li hello server hello cross-origin požadavek po zkontrolování hlavičky počátku hello</span><span class="sxs-lookup"><span data-stu-id="0db32-118">An HTTP error code such as 403 if hello server does not allow hello cross-origin request after checking hello Origin header</span></span>

   * <span data-ttu-id="0db32-119">**Access-Control-Allow-Origin** záhlaví se zástupnými znaky, které umožňuje všechny původy:</span><span class="sxs-lookup"><span data-stu-id="0db32-119">An **Access-Control-Allow-Origin** header with a wildcard that allows all origins:</span></span>

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a><span data-ttu-id="0db32-120">Pro komplexní požadavky:</span><span class="sxs-lookup"><span data-stu-id="0db32-120">For complex requests:</span></span>

<span data-ttu-id="0db32-121">Žádost o komplexní je požadavek CORS, kde hello prohlížeče je požadovaná toosend *předběžný požadavek* (tj. předběžné kontroly) před odesláním hello aktuální požadavek CORS.</span><span class="sxs-lookup"><span data-stu-id="0db32-121">A complex request is a CORS request where hello browser is required toosend a *preflight request* (i.e. a preliminary probe) before sending hello actual CORS request.</span></span> <span data-ttu-id="0db32-122">Hello předběžný požadavek požádá oprávnění server hello Pokud hello původní požadavek CORS můžete pokračovat a je `OPTIONS` požadavku toohello stejnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="0db32-122">hello preflight request asks hello server permission if hello original CORS request can proceed and is an `OPTIONS` request toohello same URL.</span></span>

> [!TIP]
> <span data-ttu-id="0db32-123">Další informace o CORS toky a běžné nástrahy zobrazit hello [Průvodce tooCORS pro rozhraní REST API](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span><span class="sxs-lookup"><span data-stu-id="0db32-123">For more details on CORS flows and common pitfalls, view hello [Guide tooCORS for REST APIs](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span></span>
>
>

## <a name="wildcard-or-single-origin-scenarios"></a><span data-ttu-id="0db32-124">Zástupný znak nebo jeden počátek scénáře</span><span class="sxs-lookup"><span data-stu-id="0db32-124">Wildcard or single origin scenarios</span></span>
<span data-ttu-id="0db32-125">CORS v Azure CDN budou automaticky fungovat žádnou další konfiguraci, když hello **Access-Control-Allow-Origin** záhlaví nastavena toowildcard (*) nebo jeden počátek.</span><span class="sxs-lookup"><span data-stu-id="0db32-125">CORS on Azure CDN will work automatically with no additional configuration when hello **Access-Control-Allow-Origin** header is set toowildcard (*) or a single origin.</span></span>  <span data-ttu-id="0db32-126">Hello CDN bude ukládat do mezipaměti první odpověď hello a následné žádosti budou používat hello stejné záhlaví.</span><span class="sxs-lookup"><span data-stu-id="0db32-126">hello CDN will cache hello first response and subsequent requests will use hello same header.</span></span>

<span data-ttu-id="0db32-127">Pokud požadavky již byly provedeny toohello CDN předchozí tooCORS se nastavuje na hello do zdrojového umístění, budete potřebovat toopurge obsah na váš koncový bod obsahu tooreload hello obsahu s hello **Access-Control-Allow-Origin** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="0db32-127">If requests have already been made toohello CDN prior tooCORS being set on hello your origin, you will need toopurge content on your endpoint content tooreload hello content with hello **Access-Control-Allow-Origin** header.</span></span>

## <a name="multiple-origin-scenarios"></a><span data-ttu-id="0db32-128">Více scénářů počátek</span><span class="sxs-lookup"><span data-stu-id="0db32-128">Multiple origin scenarios</span></span>
<span data-ttu-id="0db32-129">Pokud potřebujete tooallow na konkrétní seznam původů toobe povolené pro CORS, získat věcí trochu složitější.</span><span class="sxs-lookup"><span data-stu-id="0db32-129">If you need tooallow a specific list of origins toobe allowed for CORS, things get a little more complicated.</span></span> <span data-ttu-id="0db32-130">Hello potížím dochází, pokud hello CDN ukládá do mezipaměti hello **Access-Control-Allow-Origin** záhlaví pro první zdroj CORS hello.</span><span class="sxs-lookup"><span data-stu-id="0db32-130">hello problem occurs when hello CDN caches hello **Access-Control-Allow-Origin** header for hello first CORS origin.</span></span>  <span data-ttu-id="0db32-131">Pokud jiný zdroj CORS provede následného požadavku, hello CDN bude sloužit hello mezipaměti **Access-Control-Allow-Origin** hlavičky, která nebude odpovídat.</span><span class="sxs-lookup"><span data-stu-id="0db32-131">When a different CORS origin makes a subsequent request, hello CDN will serve hello cached **Access-Control-Allow-Origin** header, which won't match.</span></span>  <span data-ttu-id="0db32-132">Existuje několik způsobů toocorrect to.</span><span class="sxs-lookup"><span data-stu-id="0db32-132">There are several ways toocorrect this.</span></span>

### <a name="azure-cdn-premium-from-verizon"></a><span data-ttu-id="0db32-133">Azure CDN Premium od Verizonu</span><span class="sxs-lookup"><span data-stu-id="0db32-133">Azure CDN Premium from Verizon</span></span>
<span data-ttu-id="0db32-134">Hello tooenable nejlepší způsob, jak jde toouse **Azure CDN Premium od společnosti Verizon**, který zpřístupňuje některé rozšířené funkce.</span><span class="sxs-lookup"><span data-stu-id="0db32-134">hello best way tooenable this is toouse **Azure CDN Premium from Verizon**, which exposes some advanced functionality.</span></span> 

<span data-ttu-id="0db32-135">Budete potřebovat příliš[vytvořit pravidlo](cdn-rules-engine.md) toocheck hello **původu** hlavičky v požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="0db32-135">You'll need too[create a rule](cdn-rules-engine.md) toocheck hello **Origin** header on hello request.</span></span>  <span data-ttu-id="0db32-136">Pokud je platný původu, pravidla nastaví hello **Access-Control-Allow-Origin** hlavička s hello původu uvedených v žádosti o hello.</span><span class="sxs-lookup"><span data-stu-id="0db32-136">If it's a valid origin, your rule will set hello **Access-Control-Allow-Origin** header with hello origin provided in hello request.</span></span>  <span data-ttu-id="0db32-137">Pokud hello počátek zadané v hello **původu** hlavičky není povolena, pravidla měli vynechejte hello **Access-Control-Allow-Origin** záhlaví, což způsobí hello prohlížeče tooreject hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="0db32-137">If hello origin specified in hello **Origin** header is not allowed, your rule should omit hello **Access-Control-Allow-Origin** header which will cause hello browser tooreject hello request.</span></span> 

<span data-ttu-id="0db32-138">Existují dva způsoby toodo to s stroj pravidel hello.</span><span class="sxs-lookup"><span data-stu-id="0db32-138">There are two ways toodo this with hello rules engine.</span></span>  <span data-ttu-id="0db32-139">V obou případech hello **Access-Control-Allow-Origin** hlavička ze souboru hello zdrojový server je zcela ignorován, stroj pravidel hello CDN provádí kompletní správu hello povolené zdroje CORS.</span><span class="sxs-lookup"><span data-stu-id="0db32-139">In both cases, hello **Access-Control-Allow-Origin** header from hello file's origin server is completely ignored, hello CDN's rules engine completely manages hello allowed CORS origins.</span></span>

#### <a name="one-regular-expression-with-all-valid-origins"></a><span data-ttu-id="0db32-140">Jeden regulární výraz s všechny původy platný</span><span class="sxs-lookup"><span data-stu-id="0db32-140">One regular expression with all valid origins</span></span>
<span data-ttu-id="0db32-141">V tomto případě vytvoříte regulární výraz, který obsahuje všechny hello původu chcete tooallow:</span><span class="sxs-lookup"><span data-stu-id="0db32-141">In this case, you'll create a regular expression that includes all of hello origins you want tooallow:</span></span> 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> <span data-ttu-id="0db32-142">**Azure CDN společnosti Verizon** používá [Perl kompatibilní regulární výrazy](http://pcre.org/) jako jeho modul pro regulární výrazy.</span><span class="sxs-lookup"><span data-stu-id="0db32-142">**Azure CDN from Verizon** uses [Perl Compatible Regular Expressions](http://pcre.org/) as its engine for regular expressions.</span></span>  <span data-ttu-id="0db32-143">Můžete použít nástroje, jako je [regulární výrazy 101](https://regex101.com/) toovalidate regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="0db32-143">You can use a tool like [Regular Expressions 101](https://regex101.com/) toovalidate your regular expression.</span></span>  <span data-ttu-id="0db32-144">Všimněte si, že hello "/" znak je platný v regulárních výrazech a nepotřebuje toobe řídicí sekvencí, ale uvozovací znaky tento znak se považuje za osvědčený postup a očekává se některé validátory regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="0db32-144">Note that hello "/" character is valid in regular expressions and doesn't need toobe escaped, however, escaping that character is considered a best practice and is expected by some regex validators.</span></span>
> 
> 

<span data-ttu-id="0db32-145">Pokud regulární výraz hello odpovídá, nahradí pravidla hello **Access-Control-Allow-Origin** záhlaví (pokud existuje) ze zdroje hello s hello původ, který poslal žádost hello.</span><span class="sxs-lookup"><span data-stu-id="0db32-145">If hello regular expression matches, your rule will replace hello **Access-Control-Allow-Origin** header (if any) from hello origin with hello origin that sent hello request.</span></span>  <span data-ttu-id="0db32-146">Můžete také přidat další hlavičky CORS, jako například **přístup – ovládací prvek-Allow-Methods**.</span><span class="sxs-lookup"><span data-stu-id="0db32-146">You can also add additional CORS headers, such as **Access-Control-Allow-Methods**.</span></span>

![Příklad pravidla s regulárním výrazem](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a><span data-ttu-id="0db32-148">Žádost o pravidlo záhlaví pro každý počátek.</span><span class="sxs-lookup"><span data-stu-id="0db32-148">Request header rule for each origin.</span></span>
<span data-ttu-id="0db32-149">Místo regulární výrazy, můžete místo toho vytvořit samostatné pravidlo pro každý původ chcete tooallow pomocí hello **zástupné hlavičky požadavku** [vyhovují podmínce](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="0db32-149">Rather than regular expressions, you can instead create a separate rule for each origin you wish tooallow using hello **Request Header Wildcard** [match condition](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span></span> <span data-ttu-id="0db32-150">Jako regulární výraz metodou hello, modul hello pravidel samostatně nastaví hlavičky CORS hello.</span><span class="sxs-lookup"><span data-stu-id="0db32-150">As with hello regular expression method, hello rules engine alone sets hello CORS headers.</span></span> 

![Příklad pravidla bez regulární výraz](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> <span data-ttu-id="0db32-152">V předchozím příkladu hello, hello použití hello zástupný znak * informuje hello pravidla modul toomatch HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0db32-152">In hello example above, hello use of hello wildcard character * tells hello rules engine toomatch both HTTP and HTTPS.</span></span>
> 
> 

### <a name="azure-cdn-standard"></a><span data-ttu-id="0db32-153">Azure CDN Standard</span><span class="sxs-lookup"><span data-stu-id="0db32-153">Azure CDN Standard</span></span>
<span data-ttu-id="0db32-154">Na Azure CDN Standard profily hello pouze tooallow mechanismus pro více zdroje bez použití hello původu zástupný znak hello je toouse [řetězce dotazu do mezipaměti](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="0db32-154">On Azure CDN Standard profiles, hello only mechanism tooallow for multiple origins without hello use of hello wildcard origin is toouse [query string caching](cdn-query-string.md).</span></span>  <span data-ttu-id="0db32-155">Nastavení řetězec dotazu tooenable potřebovat pro koncový bod CDN hello a potom pomocí řetězce dotazu jedinečné požadavky z každé povolené domény.</span><span class="sxs-lookup"><span data-stu-id="0db32-155">You need tooenable query string setting for hello CDN endpoint and then use a unique query string for requests from each allowed domain.</span></span> <span data-ttu-id="0db32-156">To způsobí hello CDN ukládání do mezipaměti samostatný objekt pro každý jedinečný dotazu řetězec.</span><span class="sxs-lookup"><span data-stu-id="0db32-156">Doing this will result in hello CDN caching a separate object for each unique query string.</span></span> <span data-ttu-id="0db32-157">Tento přístup není ideální, ale, jak ho bude mít za následek více kopií hello stejný soubor v mezipaměti na hello CDN.</span><span class="sxs-lookup"><span data-stu-id="0db32-157">This approach is not ideal, however, as it will result in multiple copies of hello same file cached on hello CDN.</span></span>  

