---
title: "Pro delegování uživatele registrace a produktu předplatného"
description: "Zjistěte, jak delegovat uživatele registrace a produktu předplatné třetí straně ve službě Azure API Management."
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
ms.openlocfilehash: 2637ab6405f2d4ea1da84981295a144874dfa4f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a><span data-ttu-id="240ab-103">Pro delegování uživatele registrace a produktu předplatného</span><span class="sxs-lookup"><span data-stu-id="240ab-103">How to delegate user registration and product subscription</span></span>
<span data-ttu-id="240ab-104">Delegování umožňuje použít existující web pro zpracování vývojáře sign v nebo registrace-množství a předplatné produkty oproti pomocí integrované funkce v portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="240ab-104">Delegation allows you to use your existing website for handling developer sign-in/sign-up and subscription to products as opposed to using the built-in functionality in the developer portal.</span></span> <span data-ttu-id="240ab-105">To umožňuje webu do vlastní data uživatele a provést ověření z těchto kroků vlastní způsobem.</span><span class="sxs-lookup"><span data-stu-id="240ab-105">This enables your website to own the user data and perform the validation of these steps in a custom way.</span></span>

## <span data-ttu-id="240ab-106"><a name="delegate-signin-up"></a>Delegování vývojáře přihlášení a odhlášení</span><span class="sxs-lookup"><span data-stu-id="240ab-106"><a name="delegate-signin-up"> </a>Delegating developer sign-in and sign-up</span></span>
<span data-ttu-id="240ab-107">Delegovat vývojáře přihlášení a odhlášení na existující web budete muset vytvořit koncový bod speciální delegování na váš web, který funguje jako vstupní bod pro takové žádosti inicializována z portálu pro vývojáře API Management.</span><span class="sxs-lookup"><span data-stu-id="240ab-107">To delegate developer sign-in and sign-up to your existing website you will need to create a special delegation endpoint on your site that acts as the entry-point for any such request initiated from the API Management developer portal.</span></span>

<span data-ttu-id="240ab-108">Poslední pracovní postup bude takto:</span><span class="sxs-lookup"><span data-stu-id="240ab-108">The final workflow will be as follows:</span></span>

1. <span data-ttu-id="240ab-109">Vývojáře klikne na odkaz přihlášení nebo registraci na portálu pro vývojáře API Management</span><span class="sxs-lookup"><span data-stu-id="240ab-109">Developer clicks on the sign-in or sign-up link at the API Management developer portal</span></span>
2. <span data-ttu-id="240ab-110">Prohlížeč je přesměrován na koncový bod delegování</span><span class="sxs-lookup"><span data-stu-id="240ab-110">Browser is redirected to the delegation endpoint</span></span>
3. <span data-ttu-id="240ab-111">Koncový bod delegování na oplátku přesměruje na nebo uvede požadavek uživatele pro přihlášení nebo registraci uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="240ab-111">Delegation endpoint in return redirects to or presents UI asking user to sign-in or sign-up</span></span>
4. <span data-ttu-id="240ab-112">V případě úspěchu je uživatel přesměrován zpět na stránky portálu API Management vývojáře, které se spouští z</span><span class="sxs-lookup"><span data-stu-id="240ab-112">On success, the user is redirected back to the API Management developer portal page they started from</span></span>

<span data-ttu-id="240ab-113">Pokud chcete začít, můžeme první nastavení API Management pro směrování požadavků prostřednictvím delegování koncový bod.</span><span class="sxs-lookup"><span data-stu-id="240ab-113">To begin, let's first set-up API Management to route requests via your delegation endpoint.</span></span> <span data-ttu-id="240ab-114">Na portál vydavatele služby API Management, klikněte na **zabezpečení** a klikněte **delegování** kartě.</span><span class="sxs-lookup"><span data-stu-id="240ab-114">In the API Management publisher portal, click on **Security** and then click the **Delegation** tab.</span></span> <span data-ttu-id="240ab-115">Klikněte na zaškrtávací políčko Povolit delegáta přihlášení a registrace.</span><span class="sxs-lookup"><span data-stu-id="240ab-115">Click the checkbox to enable 'Delegate sign-in & sign-up'.</span></span>

![Stránku delegování][api-management-delegation-signin-up]

* <span data-ttu-id="240ab-117">Rozhodněte, jaké adresu URL vašeho koncového bodu speciální delegování bude a zadejte ho v **adresu URL koncového bodu delegování** pole.</span><span class="sxs-lookup"><span data-stu-id="240ab-117">Decide what the URL of your special delegation endpoint will be and enter it in the **Delegation endpoint URL** field.</span></span> 
* <span data-ttu-id="240ab-118">V rámci **delegování ověřovací klíč** pole zadejte tajný klíč, který se použije k výpočtu podpisu, které jste pro kontroluje, zda požadavek pochází skutečně z Azure API Management získali.</span><span class="sxs-lookup"><span data-stu-id="240ab-118">Within the **Delegation authentication key** field enter a secret that will be used to compute a signature provided to you for verification to ensure that the request is indeed coming from Azure API Management.</span></span> <span data-ttu-id="240ab-119">Můžete kliknout na **generovat** tlačítko tak, aby měl stav rozhraní API náhodně Generovat klíč pro vás.</span><span class="sxs-lookup"><span data-stu-id="240ab-119">You can click the **generate** button to have API Managemnet randomly generate a key for you.</span></span>

<span data-ttu-id="240ab-120">Teď je potřeba vytvořit **koncový bod delegování**.</span><span class="sxs-lookup"><span data-stu-id="240ab-120">Now you need to create the **delegation endpoint**.</span></span> <span data-ttu-id="240ab-121">Je třeba provést několik akcí:</span><span class="sxs-lookup"><span data-stu-id="240ab-121">It has to perform a number of actions:</span></span>

1. <span data-ttu-id="240ab-122">Zobrazit žádost o následující tvar:</span><span class="sxs-lookup"><span data-stu-id="240ab-122">Receive a request in the following form:</span></span>
   
   > <span data-ttu-id="240ab-123">*http://www.yourwebsite.com/apimdelegation?Operation=SignIn&returnUrl= {URL zdrojové stránky} & salt = {řetězec} & sig = {řetězec}*</span><span class="sxs-lookup"><span data-stu-id="240ab-123">*http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl={URL of source page}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="240ab-124">Parametry dotazu pro případ přihlášení / registrace:</span><span class="sxs-lookup"><span data-stu-id="240ab-124">Query parameters for the sign-in / sign-up case:</span></span>
   
   * <span data-ttu-id="240ab-125">**operace**: Určuje, jaký typ žádosti o delegování je – lze pouze **přihlášení** v tomto případě</span><span class="sxs-lookup"><span data-stu-id="240ab-125">**operation**: identifies what type of delegation request it is - it can only be **SignIn** in this case</span></span>
   * <span data-ttu-id="240ab-126">**returnUrl**: adresa URL stránky, kde uživatel klepl na přihlášení nebo registraci propojení</span><span class="sxs-lookup"><span data-stu-id="240ab-126">**returnUrl**: the URL of the page where the user clicked on a sign-in or sign-up link</span></span>
   * <span data-ttu-id="240ab-127">**řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení</span><span class="sxs-lookup"><span data-stu-id="240ab-127">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="240ab-128">**SIG**: hodnotu hash počítaný zabezpečení, který se má použít pro porovnání se vlastní Vypočítaný algoritmus hash</span><span class="sxs-lookup"><span data-stu-id="240ab-128">**sig**: a computed security hash to be used for comparison to your own computed hash</span></span>
2. <span data-ttu-id="240ab-129">Ověřte, zda požadavek pochází z Azure API Management, (volitelný, ale důrazně doporučujeme pro zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="240ab-129">Verify that the request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="240ab-130">Výpočetní HMAC SHA512 hash řetězce na základě **returnUrl** a **řetězce salt** parametrů dotazu ([příklad kódu níže uvedenou]):</span><span class="sxs-lookup"><span data-stu-id="240ab-130">Compute an HMAC-SHA512 hash of a string based on the **returnUrl** and **salt** query parameters ([example code provided below]):</span></span>
     
     > <span data-ttu-id="240ab-131">Metoda HMAC (**řetězce salt** + '\n' + **returnUrl**)</span><span class="sxs-lookup"><span data-stu-id="240ab-131">HMAC(**salt** + '\n' + **returnUrl**)</span></span>
     > 
     > 
   * <span data-ttu-id="240ab-132">Porovnat hodnotu výše vypočítanou na hodnotu **sig** parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="240ab-132">Compare the above-computed hash to the value of the **sig** query parameter.</span></span> <span data-ttu-id="240ab-133">Pokud dva klíče hash shodují, přejít k dalšímu kroku, v opačném případě daný požadavek je odepřen.</span><span class="sxs-lookup"><span data-stu-id="240ab-133">If the two hashes match, move on to the next step, otherwise deny the request.</span></span>
3. <span data-ttu-id="240ab-134">Ověřte, zda jsou přijímají požadavek na přihlášení v nebo přihlašovací up: **operace** parametr dotazu bude nastavena na "**přihlášení**".</span><span class="sxs-lookup"><span data-stu-id="240ab-134">Verify that you are receiving a request for sign-in/sign-up: the **operation** query parameter will be set to "**SignIn**".</span></span>
4. <span data-ttu-id="240ab-135">Uživateli zprostředkovali uživatelského rozhraní pro přihlášení nebo registraci</span><span class="sxs-lookup"><span data-stu-id="240ab-135">Present the user with UI to sign-in or sign-up</span></span>
5. <span data-ttu-id="240ab-136">Pokud je uživatel zaregistrovali budete muset vytvořit účet odpovídající pro ně ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="240ab-136">If the user is signing-up you have to create a corresponding account for them in API Management.</span></span> <span data-ttu-id="240ab-137">[Vytvoření uživatele] pomocí rozhraní API REST API pro správu.</span><span class="sxs-lookup"><span data-stu-id="240ab-137">[Create a user] with the API Management REST API.</span></span> <span data-ttu-id="240ab-138">Když to uděláte, zkontrolujte, že je nastavená ID uživatele stejná, který je v úložišti uživatele nebo ID, které můžete sledovat určité.</span><span class="sxs-lookup"><span data-stu-id="240ab-138">When doing so, ensure that you set the user ID to the same it is in your user store or to an ID that you can keep track of.</span></span>
6. <span data-ttu-id="240ab-139">Když je uživatel úspěšně ověřen:</span><span class="sxs-lookup"><span data-stu-id="240ab-139">When the user is successfully authenticated:</span></span>
   
   * <span data-ttu-id="240ab-140">[žádosti o token jednotného přihlašování (SSO)] prostřednictvím rozhraní API REST API pro správu</span><span class="sxs-lookup"><span data-stu-id="240ab-140">[request a single-sign-on (SSO) token] via the API Management REST API</span></span>
   * <span data-ttu-id="240ab-141">parametr dotazu returnUrl připojte k adrese URL jednotné přihlašování jste dostali od volání rozhraní API výše:</span><span class="sxs-lookup"><span data-stu-id="240ab-141">append a returnUrl query parameter to the SSO URL you have received from the API call above:</span></span>
     
     > <span data-ttu-id="240ab-142">například https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span><span class="sxs-lookup"><span data-stu-id="240ab-142">e.g. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url</span></span> 
     > 
     > 
   * <span data-ttu-id="240ab-143">přesměruje uživatele na výše uvedenou adresu URL vytvořené</span><span class="sxs-lookup"><span data-stu-id="240ab-143">redirect the user to the above produced URL</span></span>

<span data-ttu-id="240ab-144">Kromě **přihlášení** operace, můžete také provést správy účtů podle předchozího postupu a pomocí některého z následujících operací.</span><span class="sxs-lookup"><span data-stu-id="240ab-144">In addition to the **SignIn** operation, you can also perform account management by following the previous steps and using one of the following operations.</span></span>

* <span data-ttu-id="240ab-145">**Změna hesla byla**</span><span class="sxs-lookup"><span data-stu-id="240ab-145">**ChangePassword**</span></span>
* <span data-ttu-id="240ab-146">**ChangeProfile**</span><span class="sxs-lookup"><span data-stu-id="240ab-146">**ChangeProfile**</span></span>
* <span data-ttu-id="240ab-147">**CloseAccount**</span><span class="sxs-lookup"><span data-stu-id="240ab-147">**CloseAccount**</span></span>

<span data-ttu-id="240ab-148">Musíte zadat následující parametry dotazu pro operace správy účtů.</span><span class="sxs-lookup"><span data-stu-id="240ab-148">You must pass the following query parameters for account management operations.</span></span>

* <span data-ttu-id="240ab-149">**operace**: Určuje, jaký typ žádosti o delegování je (Změna hesla byla, ChangeProfile nebo CloseAccount)</span><span class="sxs-lookup"><span data-stu-id="240ab-149">**operation**: identifies what type of delegation request it is (ChangePassword, ChangeProfile, or CloseAccount)</span></span>
* <span data-ttu-id="240ab-150">**ID uživatele**: id uživatele účtu pro správu</span><span class="sxs-lookup"><span data-stu-id="240ab-150">**userId**: the user id of the account to manage</span></span>
* <span data-ttu-id="240ab-151">**řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení</span><span class="sxs-lookup"><span data-stu-id="240ab-151">**salt**: a special salt string used for computing a security hash</span></span>
* <span data-ttu-id="240ab-152">**SIG**: hodnotu hash počítaný zabezpečení, který se má použít pro porovnání se vlastní Vypočítaný algoritmus hash</span><span class="sxs-lookup"><span data-stu-id="240ab-152">**sig**: a computed security hash to be used for comparison to your own computed hash</span></span>

## <span data-ttu-id="240ab-153"><a name="delegate-product-subscription"></a>Delegování odběru produktu</span><span class="sxs-lookup"><span data-stu-id="240ab-153"><a name="delegate-product-subscription"> </a>Delegating product subscription</span></span>
<span data-ttu-id="240ab-154">Delegování odběru produktů funguje podobně delegování uživatele přihlášení /-up.</span><span class="sxs-lookup"><span data-stu-id="240ab-154">Delegating product subscription works similarly to delegating user sign-in/-up.</span></span> <span data-ttu-id="240ab-155">Poslední pracovní postup bude následující:</span><span class="sxs-lookup"><span data-stu-id="240ab-155">The final workflow would be as follows:</span></span>

1. <span data-ttu-id="240ab-156">Vývojář vybere produktu v portálu pro vývojáře API Management a klikne na tlačítko přihlásit k odběru</span><span class="sxs-lookup"><span data-stu-id="240ab-156">Developer selects a product in the API Management developer portal and clicks on the Subscribe button</span></span>
2. <span data-ttu-id="240ab-157">Prohlížeč je přesměrován na koncový bod delegování</span><span class="sxs-lookup"><span data-stu-id="240ab-157">Browser is redirected to the delegation endpoint</span></span>
3. <span data-ttu-id="240ab-158">Koncový bod delegování provede kroky požadované produktu předplatné – to je na vás a může mít za následek přesměrování na jinou stránku k požadavku na fakturační informace klást další otázky, nebo jednoduše ukládání informací a které nevyžadují žádné akce uživatele</span><span class="sxs-lookup"><span data-stu-id="240ab-158">Delegation endpoint performs required product subscription steps - this is up to you and may entail redirecting to another page to request billing information, asking additional questions, or simply storing the information and not requiring any user action</span></span>

<span data-ttu-id="240ab-159">K povolení funkcí, na **delegování** klikněte na stránce **delegovat odběru produktů**.</span><span class="sxs-lookup"><span data-stu-id="240ab-159">To enable the functionality, on the **Delegation** page click **Delegate product subscription**.</span></span>

<span data-ttu-id="240ab-160">Potom zkontrolujte, že koncový bod delegování provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="240ab-160">Then ensure the delegation endpoint performs the following actions:</span></span>

1. <span data-ttu-id="240ab-161">Zobrazit žádost o následující tvar:</span><span class="sxs-lookup"><span data-stu-id="240ab-161">Receive a request in the following form:</span></span>
   
   > <span data-ttu-id="240ab-162">*{operaci} http://www.yourwebsite.com/apimdelegation?Operation= & productId = {produktu pro přihlášení k odběru} & userId = {uživatele, který vytvořil požadavek} & řetězce salt = {řetězec} & sig = {řetězec}*</span><span class="sxs-lookup"><span data-stu-id="240ab-162">*http://www.yourwebsite.com/apimdelegation?operation={operation}&productId={product to subscribe to}&userId={user making request}&salt={string}&sig={string}*</span></span>
   > 
   > 
   
    <span data-ttu-id="240ab-163">Parametry dotazu pro případ odběru produktu:</span><span class="sxs-lookup"><span data-stu-id="240ab-163">Query parameters for the product subscription case:</span></span>
   
   * <span data-ttu-id="240ab-164">**operace**: Určuje, jaký typ žádosti o delegování je.</span><span class="sxs-lookup"><span data-stu-id="240ab-164">**operation**: identifies what type of delegation request it is.</span></span> <span data-ttu-id="240ab-165">Pro odběr produktů požadavky platné možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="240ab-165">For product subscription requests the valid options are:</span></span>
     * <span data-ttu-id="240ab-166">"Přihlásit": požadavek na přihlášení k odběru uživateli daný produkt zadané ID (viz níže)</span><span class="sxs-lookup"><span data-stu-id="240ab-166">"Subscribe": a request to subscribe the user to a given product with provided ID (see below)</span></span>
     * <span data-ttu-id="240ab-167">"Zrušit": požadavek na odhlášení uživatele z produktu</span><span class="sxs-lookup"><span data-stu-id="240ab-167">"Unsubscribe": a request to unsubscribe a user from a product</span></span>
     * <span data-ttu-id="240ab-168">"Obnovit": žádost o obnovení předplatného, (například to může být vypršení platnosti)</span><span class="sxs-lookup"><span data-stu-id="240ab-168">"Renew": a requst to renew a subscription (e.g. that may be expiring)</span></span>
   * <span data-ttu-id="240ab-169">**productId**: ID uživatel si vyžádal, přihlaste se k odběru produktu</span><span class="sxs-lookup"><span data-stu-id="240ab-169">**productId**: the ID of the product the user requested to subscribe to</span></span>
   * <span data-ttu-id="240ab-170">**ID uživatele**: ID uživatele, pro kterého je žádosti</span><span class="sxs-lookup"><span data-stu-id="240ab-170">**userId**: the ID of the user for whom the request is made</span></span>
   * <span data-ttu-id="240ab-171">**řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení</span><span class="sxs-lookup"><span data-stu-id="240ab-171">**salt**: a special salt string used for computing a security hash</span></span>
   * <span data-ttu-id="240ab-172">**SIG**: hodnotu hash počítaný zabezpečení, který se má použít pro porovnání se vlastní Vypočítaný algoritmus hash</span><span class="sxs-lookup"><span data-stu-id="240ab-172">**sig**: a computed security hash to be used for comparison to your own computed hash</span></span>
2. <span data-ttu-id="240ab-173">Ověřte, zda požadavek pochází z Azure API Management, (volitelný, ale důrazně doporučujeme pro zabezpečení)</span><span class="sxs-lookup"><span data-stu-id="240ab-173">Verify that the request is coming from Azure API Management (optional, but highly recommended for security)</span></span>
   
   * <span data-ttu-id="240ab-174">Výpočetní SHA512 HMAC řetězce na základě **productId**, **userId** a **řetězce salt** parametrů dotazu:</span><span class="sxs-lookup"><span data-stu-id="240ab-174">Compute an HMAC-SHA512 of a string based on the **productId**, **userId** and **salt** query parameters:</span></span>
     
     > <span data-ttu-id="240ab-175">Metoda HMAC (**řetězce salt** + '\n' + **productId** + '\n' + **userId**)</span><span class="sxs-lookup"><span data-stu-id="240ab-175">HMAC(**salt** + '\n' + **productId** + '\n' + **userId**)</span></span>
     > 
     > 
   * <span data-ttu-id="240ab-176">Porovnat hodnotu výše vypočítanou na hodnotu **sig** parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="240ab-176">Compare the above-computed hash to the value of the **sig** query parameter.</span></span> <span data-ttu-id="240ab-177">Pokud dva klíče hash shodují, přejít k dalšímu kroku, v opačném případě daný požadavek je odepřen.</span><span class="sxs-lookup"><span data-stu-id="240ab-177">If the two hashes match, move on to the next step, otherwise deny the request.</span></span>
3. <span data-ttu-id="240ab-178">Proveďte jakékoli produktu zpracování odběru na základě typu požadovanou v operaci **operace** – například fakturace, další otázky, atd.</span><span class="sxs-lookup"><span data-stu-id="240ab-178">Perform any product subscription processing based on the type of operation requested in **operation** - e.g. billing, further questions, etc.</span></span>
4. <span data-ttu-id="240ab-179">Na přihlášení uživatele k produktu na vaší straně úspěšně odběru, přihlášení k odběru uživateli produktů pro správu rozhraní API pomocí [volání rozhraní REST API pro odběr produktů].</span><span class="sxs-lookup"><span data-stu-id="240ab-179">On successfully subscribing the user to the product on your side, subscribe the user to the API Management product by [calling the REST API for product subscription].</span></span>

## <span data-ttu-id="240ab-180"><a name="delegate-example-code"></a> Příklad kódu</span><span class="sxs-lookup"><span data-stu-id="240ab-180"><a name="delegate-example-code"> </a> Example Code</span></span>
<span data-ttu-id="240ab-181">Tyto ukázky kódu ukazují, jak provést *delegování ověřovací klíč*, který je nastaven na obrazovce delegování portálu vydavatele, chcete-li vytvořit HMAC, které se pak použije k ověření podpisu, prokázání platnost předaný returnUrl .</span><span class="sxs-lookup"><span data-stu-id="240ab-181">These code samples show how to take the *delegation validation key*, which is set in the Delegation screen of the publisher portal, to create a HMAC which is then used to validate the signature, proving the validity of the passed returnUrl.</span></span> <span data-ttu-id="240ab-182">Stejný kód funguje pro productId a ID uživatele se mírně upraven.</span><span class="sxs-lookup"><span data-stu-id="240ab-182">The same code works for the productId and userId with slight modification.</span></span>

<span data-ttu-id="240ab-183">**Kód jazyka C# pro generování hash returnUrl**</span><span class="sxs-lookup"><span data-stu-id="240ab-183">**C# code to generate hash of returnUrl**</span></span>

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
}
```

<span data-ttu-id="240ab-184">**Kód NodeJS ke generování hash returnUrl**</span><span class="sxs-lookup"><span data-stu-id="240ab-184">**NodeJS code to generate hash of returnUrl**</span></span>

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature to sig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a><span data-ttu-id="240ab-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="240ab-185">Next steps</span></span>
<span data-ttu-id="240ab-186">Další informace o delegování najdete v následujícím videu.</span><span class="sxs-lookup"><span data-stu-id="240ab-186">For more information on delegation, see the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
<span data-ttu-id="240ab-187">[žádosti o token jednotného přihlašování (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409</span><span class="sxs-lookup"><span data-stu-id="240ab-187">[request a single-sign-on (SSO) token]: http://go.microsoft.com/fwlink/?LinkId=507409</span></span>
<span data-ttu-id="240ab-188">[vytvoření uživatele]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser</span><span class="sxs-lookup"><span data-stu-id="240ab-188">[create a user]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser</span></span>
<span data-ttu-id="240ab-189">[volání rozhraní REST API pro odběr produktů]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO</span><span class="sxs-lookup"><span data-stu-id="240ab-189">[calling the REST API for product subscription]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO</span></span>
[Next steps]: #next-steps
<span data-ttu-id="240ab-190">[příklad kódu níže uvedenou]: #delegate-example-code</span><span class="sxs-lookup"><span data-stu-id="240ab-190">[example code provided below]: #delegate-example-code</span></span>

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
