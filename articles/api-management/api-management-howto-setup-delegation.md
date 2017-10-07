---
title: "předplatné aaaHow toodelegate uživatele registrace a produktu"
description: "Zjistěte, jak toodelegate uživatele registrace a produktu předplatné tooa třetí strany ve službě Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 406648db2d2f37c4dcda466294726d331cc0551b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodelegate-user-registration-and-product-subscription"></a><span data-ttu-id="9eba1-103">Jak toodelegate uživatele registrace a produktu předplatného</span><span class="sxs-lookup"><span data-stu-id="9eba1-103">How toodelegate user registration and product subscription</span></span>
<span data-ttu-id="9eba1-104">Delegování vám umožní toouse vaší existující web pro vývojáře sign v nebo registrace-množství a předplatné tooproducts jako zpracování byl proti toousing hello integrovanou funkci v portálu pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="9eba1-104">Delegation allows you toouse your existing website for handling developer sign-in/sign-up and subscription tooproducts as opposed toousing hello built-in functionality in hello developer portal.</span></span> <span data-ttu-id="9eba1-105">To umožňuje webu tooown hello uživatelská data a provést ověření hello z těchto kroků vlastní způsobem.</span><span class="sxs-lookup"><span data-stu-id="9eba1-105">This enables your website tooown hello user data and perform hello validation of these steps in a custom way.</span></span>

## <span data-ttu-id="9eba1-106"><a name="delegate-signin-up"></a>Delegování vývojáře přihlášení a odhlášení</span><span class="sxs-lookup"><span data-stu-id="9eba1-106"><a name="delegate-signin-up"> </a>Delegating developer sign-in and sign-up</span></span>
<span data-ttu-id="9eba1-107">toodelegate přihlášení a odhlášení tooyour existujícího webu pro vývojáře budete potřebovat toocreate koncový bod speciální delegování na váš web, který slouží jako hello vstupní bod pro takové žádosti inicializována z portálu pro vývojáře hello API Management.</span><span class="sxs-lookup"><span data-stu-id="9eba1-107">toodelegate developer sign-in and sign-up tooyour existing website you will need toocreate a special delegation endpoint on your site that acts as hello entry-point for any such request initiated from hello API Management developer portal.</span></span>

<span data-ttu-id="9eba1-108">Hello konečné pracovní postup bude takto:</span><span class="sxs-lookup"><span data-stu-id="9eba1-108">hello final workflow will be as follows:</span></span>

1. <span data-ttu-id="9eba1-109">Vývojář klikne na odkaz přihlášení nebo registraci hello v hello portál pro vývojáře API Management</span><span class="sxs-lookup"><span data-stu-id="9eba1-109">Developer clicks on hello sign-in or sign-up link at hello API Management developer portal</span></span>
2. <span data-ttu-id="9eba1-110">Prohlížeč je koncový bod přesměrovaného toohello delegování</span><span class="sxs-lookup"><span data-stu-id="9eba1-110">Browser is redirected toohello delegation endpoint</span></span>
3. <span data-ttu-id="9eba1-111">Koncový bod delegování na oplátku přesměruje tooor uvede uživatelského rozhraní dotazem uživatel toosign-in nebo registrace</span><span class="sxs-lookup"><span data-stu-id="9eba1-111">Delegation endpoint in return redirects tooor presents UI asking user toosign-in or sign-up</span></span>
4. <span data-ttu-id="9eba1-112">V případě úspěchu je uživatel hello přesměrovaného back toohello API Management vývojáře stránky portálu, který se spouští z</span><span class="sxs-lookup"><span data-stu-id="9eba1-112">On success, hello user is redirected back toohello API Management developer portal page they started from</span></span>

<span data-ttu-id="9eba1-113">toobegin, můžeme první tooroute API Management nastavení požadavků prostřednictvím delegování koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9eba1-113">toobegin, let's first set-up API Management tooroute requests via your delegation endpoint.</span></span> <span data-ttu-id="9eba1-114">Na portálu vydavatele hello API Management, klikněte na **zabezpečení** a pak klikněte na tlačítko hello **delegování** kartě. Klikněte na zaškrtávací políčko tooenable hello delegáta přihlášení a registrace.</span><span class="sxs-lookup"><span data-stu-id="9eba1-114">In hello API Management publisher portal, click on **Security** and then click hello **Delegation** tab. Click hello checkbox tooenable 'Delegate sign-in & sign-up'.</span></span>

![Stránku delegování][api-management-delegation-signin-up]

* <span data-ttu-id="9eba1-116">Rozhodněte, co bude hello URL speciální delegování koncový bod se a zadejte ho v hello **adresu URL koncového bodu delegování** pole.</span><span class="sxs-lookup"><span data-stu-id="9eba1-116">Decide what hello URL of your special delegation endpoint will be and enter it in hello **Delegation endpoint URL** field.</span></span> 
* <span data-ttu-id="9eba1-117">V rámci hello **delegování ověřovací klíč** pole zadejte tajný klíč, který bude použité toocompute tooyou podpis zadaný pro ověření tooensure, který hello žádost skutečně pochází z Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="9eba1-117">Within hello **Delegation authentication key** field enter a secret that will be used toocompute a signature provided tooyou for verification tooensure that hello request is indeed coming from Azure API Management.</span></span> <span data-ttu-id="9eba1-118">Můžete kliknout na hello **generovat** tlačítko toohave stav rozhraní API náhodně Generovat klíč pro vás.</span><span class="sxs-lookup"><span data-stu-id="9eba1-118">You can click hello **generate** button toohave API Managemnet randomly generate a key for you.</span></span>

<span data-ttu-id="9eba1-119">Nyní je třeba toocreate hello **koncový bod delegování**.</span><span class="sxs-lookup"><span data-stu-id="9eba1-119">Now you need toocreate hello **delegation endpoint**.</span></span> <span data-ttu-id="9eba1-120">Tooperform má několik akcí:</span><span class="sxs-lookup"><span data-stu-id="9eba1-120">It has tooperform a number of actions:</span></span>

1. <span data-ttu-id="9eba1-121">V následující formulář hello zobrazit žádost:</span><span class="sxs-lookup"><span data-stu-id="9eba1-121">Receive a request in hello following form:</span></span>
   
   > <span data-ttu-id="9eba1-122">*http://www.yourwebsite.com/apimdelegation?Operation=SignIn&returnUrl= {URL zdrojové stránky} & salt = {řetězec} & sig = {řetězec}*</span><span class="sxs-lookup"><span data-stu-id="9eba1-122">*http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl={URL of source page}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="9eba1-123">Parametry dotazu pro případ hello přihlášení / registrace:</span><span class="sxs-lookup"><span data-stu-id="9eba1-123">Query parameters for hello sign-in / sign-up case:</span></span>
   
   * <span data-ttu-id="9eba1-124">**operace**: Určuje, jaký typ žádosti o delegování je – lze pouze **přihlášení** v tomto případě</span><span class="sxs-lookup"><span data-stu-id="9eba1-124">**operation**: identifies what type of delegation request it is - it can only be **SignIn** in this case</span></span>
   * <span data-ttu-id="9eba1-125">**returnUrl**: hello URL hello stránky, kde uživatel hello použil odkaz přihlášení nebo registraci</span><span class="sxs-lookup"><span data-stu-id="9eba1-125">**returnUrl**: hello URL of hello page where hello user clicked on a sign-in or sign-up link</span></span>
   * <span data-ttu-id="9eba1-126">**řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení</span><span class="sxs-lookup"><span data-stu-id="9eba1-126">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="9eba1-127">**SIG**: toobe hash počítaný zabezpečení použít pro porovnání tooyour vlastní Vypočítaný algoritmus hash</span><span class="sxs-lookup"><span data-stu-id="9eba1-127">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>
2. <span data-ttu-id="9eba1-128">Ověřte, že tento požadavek hello přichází z Azure API Management (volitelný, ale důrazně doporučujeme pro zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="9eba1-128">Verify that hello request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="9eba1-129">Výpočetní HMAC SHA512 hash řetězec v závislosti na hello **returnUrl** a **řetězce salt** parametrů dotazu ([příklad kódu níže uvedenou]):</span><span class="sxs-lookup"><span data-stu-id="9eba1-129">Compute an HMAC-SHA512 hash of a string based on hello **returnUrl** and **salt** query parameters ([example code provided below]):</span></span>
     
     > <span data-ttu-id="9eba1-130">Metoda HMAC (**řetězce salt** + '\n' + **returnUrl**)</span><span class="sxs-lookup"><span data-stu-id="9eba1-130">HMAC(**salt** + '\n' + **returnUrl**)</span></span>
     > 
     > 
   * <span data-ttu-id="9eba1-131">Porovnat hodnotu toohello výše počítaný hash hello hello **sig** parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="9eba1-131">Compare hello above-computed hash toohello value of hello **sig** query parameter.</span></span> <span data-ttu-id="9eba1-132">Pokud hello dva klíče hash shodují, v dalším kroku toohello přesune, jinak hello požadavek je odepřen.</span><span class="sxs-lookup"><span data-stu-id="9eba1-132">If hello two hashes match, move on toohello next step, otherwise deny hello request.</span></span>
3. <span data-ttu-id="9eba1-133">Ověřte, zda jsou přijímají požadavek na přihlášení v nebo přihlašovací up: hello **operace** parametr dotazu se nastaví příliš "**přihlášení**".</span><span class="sxs-lookup"><span data-stu-id="9eba1-133">Verify that you are receiving a request for sign-in/sign-up: hello **operation** query parameter will be set too"**SignIn**".</span></span>
4. <span data-ttu-id="9eba1-134">K dispozici hello uživatele pomocí uživatelského rozhraní v toosign nebo registrace</span><span class="sxs-lookup"><span data-stu-id="9eba1-134">Present hello user with UI toosign-in or sign-up</span></span>
5. <span data-ttu-id="9eba1-135">Pokud uživatel hello zaregistrovali máte toocreate odpovídající účet je ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="9eba1-135">If hello user is signing-up you have toocreate a corresponding account for them in API Management.</span></span> <span data-ttu-id="9eba1-136">[Vytvoření uživatele] s hello rozhraní API REST API pro správu.</span><span class="sxs-lookup"><span data-stu-id="9eba1-136">[Create a user] with hello API Management REST API.</span></span> <span data-ttu-id="9eba1-137">Když to uděláte, ujistěte se, abyste nastavili toohello ID uživatele hello stejné, který je v úložišti uživatele nebo tooan ID, které můžete sledovat určité.</span><span class="sxs-lookup"><span data-stu-id="9eba1-137">When doing so, ensure that you set hello user ID toohello same it is in your user store or tooan ID that you can keep track of.</span></span>
6. <span data-ttu-id="9eba1-138">Když uživatel hello úspěšně ověřen:</span><span class="sxs-lookup"><span data-stu-id="9eba1-138">When hello user is successfully authenticated:</span></span>
   
   * <span data-ttu-id="9eba1-139">[žádosti o token jednotného přihlašování (SSO)] prostřednictvím hello rozhraní API REST API pro správu</span><span class="sxs-lookup"><span data-stu-id="9eba1-139">[request a single-sign-on (SSO) token] via hello API Management REST API</span></span>
   * <span data-ttu-id="9eba1-140">Připojte toohello parametr dotazu returnUrl jednotné přihlašování adresu URL jste dostali od volání rozhraní API hello výše:</span><span class="sxs-lookup"><span data-stu-id="9eba1-140">append a returnUrl query parameter toohello SSO URL you have received from hello API call above:</span></span>
     
     > <span data-ttu-id="9eba1-141">například https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span><span class="sxs-lookup"><span data-stu-id="9eba1-141">e.g. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span></span> 
     > 
     > 
   * <span data-ttu-id="9eba1-142">přesměrování hello uživatele toohello výše vytvořeného adresy URL</span><span class="sxs-lookup"><span data-stu-id="9eba1-142">redirect hello user toohello above produced URL</span></span>

<span data-ttu-id="9eba1-143">V přidání toohello **přihlášení** operace, můžete také provést správy účtů podle předchozích kroků hello a pomocí některého z hello následující operace.</span><span class="sxs-lookup"><span data-stu-id="9eba1-143">In addition toohello **SignIn** operation, you can also perform account management by following hello previous steps and using one of hello following operations.</span></span>

* <span data-ttu-id="9eba1-144">**Změna hesla byla**</span><span class="sxs-lookup"><span data-stu-id="9eba1-144">**ChangePassword**</span></span>
* <span data-ttu-id="9eba1-145">**ChangeProfile**</span><span class="sxs-lookup"><span data-stu-id="9eba1-145">**ChangeProfile**</span></span>
* <span data-ttu-id="9eba1-146">**CloseAccount**</span><span class="sxs-lookup"><span data-stu-id="9eba1-146">**CloseAccount**</span></span>

<span data-ttu-id="9eba1-147">Je nutné předat hello následující parametry dotazu pro operace správy účtů.</span><span class="sxs-lookup"><span data-stu-id="9eba1-147">You must pass hello following query parameters for account management operations.</span></span>

* <span data-ttu-id="9eba1-148">**operace**: Určuje, jaký typ žádosti o delegování je (Změna hesla byla, ChangeProfile nebo CloseAccount)</span><span class="sxs-lookup"><span data-stu-id="9eba1-148">**operation**: identifies what type of delegation request it is (ChangePassword, ChangeProfile, or CloseAccount)</span></span>
* <span data-ttu-id="9eba1-149">**ID uživatele**: hello id uživatele účtu toomanage hello</span><span class="sxs-lookup"><span data-stu-id="9eba1-149">**userId**: hello user id of hello account toomanage</span></span>
* <span data-ttu-id="9eba1-150">**řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení</span><span class="sxs-lookup"><span data-stu-id="9eba1-150">**salt**: a special salt string used for computing a security hash</span></span>
* <span data-ttu-id="9eba1-151">**SIG**: toobe hash počítaný zabezpečení použít pro porovnání tooyour vlastní Vypočítaný algoritmus hash</span><span class="sxs-lookup"><span data-stu-id="9eba1-151">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>

## <span data-ttu-id="9eba1-152"><a name="delegate-product-subscription"></a>Delegování odběru produktu</span><span class="sxs-lookup"><span data-stu-id="9eba1-152"><a name="delegate-product-subscription"> </a>Delegating product subscription</span></span>
<span data-ttu-id="9eba1-153">Delegování odběru produktů funguje podobně toodelegating uživatele přihlášení /-up.</span><span class="sxs-lookup"><span data-stu-id="9eba1-153">Delegating product subscription works similarly toodelegating user sign-in/-up.</span></span> <span data-ttu-id="9eba1-154">poslední pracovního postupu Hello vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="9eba1-154">hello final workflow would be as follows:</span></span>

1. <span data-ttu-id="9eba1-155">Vývojář vybere produktu v portálu pro vývojáře hello API Management a klikne na tlačítko přihlásit k odběru hello</span><span class="sxs-lookup"><span data-stu-id="9eba1-155">Developer selects a product in hello API Management developer portal and clicks on hello Subscribe button</span></span>
2. <span data-ttu-id="9eba1-156">Prohlížeč je koncový bod přesměrovaného toohello delegování</span><span class="sxs-lookup"><span data-stu-id="9eba1-156">Browser is redirected toohello delegation endpoint</span></span>
3. <span data-ttu-id="9eba1-157">Koncový bod delegování provede kroky požadované produktu předplatné – to je tooyou a může mít za následek přesměrování stránky toorequest tooanother fakturační informace, klást další otázky, nebo jednoduše ukládání informací o hello a které nevyžadují žádné akce uživatele</span><span class="sxs-lookup"><span data-stu-id="9eba1-157">Delegation endpoint performs required product subscription steps - this is up tooyou and may entail redirecting tooanother page toorequest billing information, asking additional questions, or simply storing hello information and not requiring any user action</span></span>

<span data-ttu-id="9eba1-158">Funkce hello tooenable, na hello **delegování** klikněte na stránce **delegovat odběru produktů**.</span><span class="sxs-lookup"><span data-stu-id="9eba1-158">tooenable hello functionality, on hello **Delegation** page click **Delegate product subscription**.</span></span>

<span data-ttu-id="9eba1-159">Potom zkontrolujte, že koncový bod delegování hello provede následující akce hello:</span><span class="sxs-lookup"><span data-stu-id="9eba1-159">Then ensure hello delegation endpoint performs hello following actions:</span></span>

1. <span data-ttu-id="9eba1-160">V následující formulář hello zobrazit žádost:</span><span class="sxs-lookup"><span data-stu-id="9eba1-160">Receive a request in hello following form:</span></span>
   
   > <span data-ttu-id="9eba1-161">*http://www.yourwebsite.com/apimdelegation?Operation= {operaci} & productId = {produktu toosubscribe k} & userId = {uživatele, který vytvořil požadavek} & řetězce salt = {řetězec} & sig = {řetězec}*</span><span class="sxs-lookup"><span data-stu-id="9eba1-161">*http://www.yourwebsite.com/apimdelegation?operation={operation}&productId={product toosubscribe to}&userId={user making request}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="9eba1-162">Parametry dotazu pro případ odběru produktu hello:</span><span class="sxs-lookup"><span data-stu-id="9eba1-162">Query parameters for hello product subscription case:</span></span>
   
   * <span data-ttu-id="9eba1-163">**operace**: Určuje, jaký typ žádosti o delegování je.</span><span class="sxs-lookup"><span data-stu-id="9eba1-163">**operation**: identifies what type of delegation request it is.</span></span> <span data-ttu-id="9eba1-164">Pro odběr produktů požadavky hello platné možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="9eba1-164">For product subscription requests hello valid options are:</span></span>
     * <span data-ttu-id="9eba1-165">"Přihlásit": žádost toosubscribe hello uživatele tooa zadaný produkt zadané ID (viz níže)</span><span class="sxs-lookup"><span data-stu-id="9eba1-165">"Subscribe": a request toosubscribe hello user tooa given product with provided ID (see below)</span></span>
     * <span data-ttu-id="9eba1-166">"Zrušit": žádost toounsubscribe uživatele z produktu</span><span class="sxs-lookup"><span data-stu-id="9eba1-166">"Unsubscribe": a request toounsubscribe a user from a product</span></span>
     * <span data-ttu-id="9eba1-167">"Obnovit": žádost toorenew předplatné (například to může být vypršení platnosti)</span><span class="sxs-lookup"><span data-stu-id="9eba1-167">"Renew": a requst toorenew a subscription (e.g. that may be expiring)</span></span>
   * <span data-ttu-id="9eba1-168">**productId**: toosubscribe na požadovaný hello ID uživatele hello hello produktu</span><span class="sxs-lookup"><span data-stu-id="9eba1-168">**productId**: hello ID of hello product hello user requested toosubscribe to</span></span>
   * <span data-ttu-id="9eba1-169">**ID uživatele**: hello ID hello uživatele, pro kterého hello požadavku</span><span class="sxs-lookup"><span data-stu-id="9eba1-169">**userId**: hello ID of hello user for whom hello request is made</span></span>
   * <span data-ttu-id="9eba1-170">**řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení</span><span class="sxs-lookup"><span data-stu-id="9eba1-170">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="9eba1-171">**SIG**: toobe hash počítaný zabezpečení použít pro porovnání tooyour vlastní Vypočítaný algoritmus hash</span><span class="sxs-lookup"><span data-stu-id="9eba1-171">**sig**: a computed security hash toobe used for comparison tooyour own computed hash</span></span>
2. <span data-ttu-id="9eba1-172">Ověřte, že tento požadavek hello přichází z Azure API Management (volitelný, ale důrazně doporučujeme pro zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="9eba1-172">Verify that hello request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="9eba1-173">Výpočetní SHA512 HMAC řetězce podle hello **productId**, **userId** a **řetězce salt** parametrů dotazu:</span><span class="sxs-lookup"><span data-stu-id="9eba1-173">Compute an HMAC-SHA512 of a string based on hello **productId**, **userId** and **salt** query parameters:</span></span>
     
     > <span data-ttu-id="9eba1-174">Metoda HMAC (**řetězce salt** + '\n' + **productId** + '\n' + **userId**)</span><span class="sxs-lookup"><span data-stu-id="9eba1-174">HMAC(**salt** + '\n' + **productId** + '\n' + **userId**)</span></span>
     > 
     > 
   * <span data-ttu-id="9eba1-175">Porovnat hodnotu toohello výše počítaný hash hello hello **sig** parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="9eba1-175">Compare hello above-computed hash toohello value of hello **sig** query parameter.</span></span> <span data-ttu-id="9eba1-176">Pokud hello dva klíče hash shodují, v dalším kroku toohello přesune, jinak hello požadavek je odepřen.</span><span class="sxs-lookup"><span data-stu-id="9eba1-176">If hello two hashes match, move on toohello next step, otherwise deny hello request.</span></span>
3. <span data-ttu-id="9eba1-177">Proveďte jakékoli produktu zpracování odběru na základě typu hello požadovanou v operaci **operace** – například fakturace, další otázky, atd.</span><span class="sxs-lookup"><span data-stu-id="9eba1-177">Perform any product subscription processing based on hello type of operation requested in **operation** - e.g. billing, further questions, etc.</span></span>
4. <span data-ttu-id="9eba1-178">Na úspěšně přihlášení k odběru produktu toohello hello uživatele na vaší straně, přihlášení k odběru hello uživatele toohello API Management produktu podle [hello volání rozhraní REST API pro odběr produktů].</span><span class="sxs-lookup"><span data-stu-id="9eba1-178">On successfully subscribing hello user toohello product on your side, subscribe hello user toohello API Management product by [calling hello REST API for product subscription].</span></span>

## <span data-ttu-id="9eba1-179"><a name="delegate-example-code"></a> Příklad kódu</span><span class="sxs-lookup"><span data-stu-id="9eba1-179"><a name="delegate-example-code"> </a> Example Code</span></span>
<span data-ttu-id="9eba1-180">Tyto ukázky zobrazit code jak tootake hello *delegování ověřovací klíč*, který je nastaven na obrazovce delegování hello hello portálu vydavatele, toocreate klíčem HMAC, který je pak použit toovalidate hello podpis, prokázání hello platnost hello Předaný returnUrl.</span><span class="sxs-lookup"><span data-stu-id="9eba1-180">These code samples show how tootake hello *delegation validation key*, which is set in hello Delegation screen of hello publisher portal, toocreate a HMAC which is then used toovalidate hello signature, proving hello validity of hello passed returnUrl.</span></span> <span data-ttu-id="9eba1-181">Dobrý den, kterou stejný kód pracuje hello productId a ID uživatele se mírně upraven.</span><span class="sxs-lookup"><span data-stu-id="9eba1-181">hello same code works for hello productId and userId with slight modification.</span></span>

<span data-ttu-id="9eba1-182">**C# kódu toogenerate hash returnUrl**</span><span class="sxs-lookup"><span data-stu-id="9eba1-182">**C# code toogenerate hash of returnUrl**</span></span>

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature toosig query parameter
}
```

<span data-ttu-id="9eba1-183">**Kód toogenerate hash returnUrl NodeJS**</span><span class="sxs-lookup"><span data-stu-id="9eba1-183">**NodeJS code toogenerate hash of returnUrl**</span></span>

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature toosig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a><span data-ttu-id="9eba1-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9eba1-184">Next steps</span></span>
<span data-ttu-id="9eba1-185">Další informace o delegování najdete v tématu hello následující videa.</span><span class="sxs-lookup"><span data-stu-id="9eba1-185">For more information on delegation, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[žádosti o token jednotného přihlašování (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409
[vytvoření uživatele]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[hello volání rozhraní REST API pro odběr produktů]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[příklad kódu níže uvedenou]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
