---
title: "Azure AD v2 rozhraní ASP.NET Web Server získávání spuštěno – testování | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 00cb963e85111274c36c3a84489894811ad2dabd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a><span data-ttu-id="6048e-103">Otestujte svůj kód</span><span class="sxs-lookup"><span data-stu-id="6048e-103">Test your code</span></span>

<span data-ttu-id="6048e-104">Stiskněte klávesu `F5` spustit projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6048e-104">Press `F5` to run your project in Visual Studio.</span></span> <span data-ttu-id="6048e-105">Prohlížeč a přesměruje na *http://localhost: {port}* kde se zobrazí *přihlásit pomocí Microsoft* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6048e-105">The browser will open and direct you to *http://localhost:{port}* where you’ll see the *Sign in with Microsoft* button.</span></span> <span data-ttu-id="6048e-106">Pokračujte a klikněte na něj pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6048e-106">Go ahead and click it to sign in.</span></span>

<span data-ttu-id="6048e-107">Až budete připraveni k testování, použijte pracovní nebo školní (Azure Active Directory) nebo osobní (live.com, outlook.com) účet pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6048e-107">When you're ready to test, use a work or school (Azure Active Directory), or a personal (live.com, outlook.com) account to sign in.</span></span> 

![Přihlaste se pomocí okna prohlížeče Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Přihlaste se pomocí okna prohlížeče Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a><span data-ttu-id="6048e-110">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="6048e-110">Expected results</span></span>
<span data-ttu-id="6048e-111">Po přihlášení se uživatel přesměruje na domovskou stránku vašeho webu, což je adresa URL HTTPS zadaná v informace o registraci aplikace v portálu pro registraci aplikace společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6048e-111">After sign-in, the user is redirected to the home page of your web site which is the HTTPS URL specified in your application registration information in the Microsoft Application Registration Portal.</span></span> <span data-ttu-id="6048e-112">Tato stránka se teď zobrazuje *Hello {uživatele}* a odkaz na odhlášení a na odkaz zobrazíte deklaracích identity uživatele –, což je odkaz na kontroler autorizovat vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="6048e-112">This page now shows *Hello {User}* and a link to sign-out, and a link to see the user’s claims – which is a link to the Authorize controller created earlier.</span></span>

### <a name="see-users-claims"></a><span data-ttu-id="6048e-113">V tématu deklarací identity uživatele</span><span class="sxs-lookup"><span data-stu-id="6048e-113">See user's claims</span></span>
<span data-ttu-id="6048e-114">Vyberte hypertextový odkaz zobrazíte deklaracích identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="6048e-114">Select the hyperlink to see the user's claims.</span></span> <span data-ttu-id="6048e-115">To vás řadiče a zobrazení, která je k dispozici pro uživatele, kteří se ověřují.</span><span class="sxs-lookup"><span data-stu-id="6048e-115">This leads you to the controller and view that is only available to users that are authenticated.</span></span>

#### <a name="expected-results"></a><span data-ttu-id="6048e-116">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="6048e-116">Expected results</span></span>
 <span data-ttu-id="6048e-117">Měli byste vidět tabulku obsahující základní vlastnosti pro přihlášeného uživatele:</span><span class="sxs-lookup"><span data-stu-id="6048e-117">You should see a table containing the basic properties of the logged user:</span></span>

| <span data-ttu-id="6048e-118">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6048e-118">Property</span></span> | <span data-ttu-id="6048e-119">Hodnota</span><span class="sxs-lookup"><span data-stu-id="6048e-119">Value</span></span> | <span data-ttu-id="6048e-120">Popis</span><span class="sxs-lookup"><span data-stu-id="6048e-120">Description</span></span>|
|---|---|---|
| <span data-ttu-id="6048e-121">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="6048e-121">Name</span></span> | <span data-ttu-id="6048e-122">{Úplné uživatelské jméno}</span><span class="sxs-lookup"><span data-stu-id="6048e-122">{User Full Name}</span></span> | <span data-ttu-id="6048e-123">Jméno a příjmení uživatele</span><span class="sxs-lookup"><span data-stu-id="6048e-123">The user’s first and last name</span></span>
|<span data-ttu-id="6048e-124">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="6048e-124">Username</span></span> | <span>user@domain.com</span>| <span data-ttu-id="6048e-125">Uživatelské jméno sloužící k identifikaci pro přihlášeného uživatele</span><span class="sxs-lookup"><span data-stu-id="6048e-125">The username used to identify the logged user</span></span>
| <span data-ttu-id="6048e-126">Předmět</span><span class="sxs-lookup"><span data-stu-id="6048e-126">Subject</span></span>| <span data-ttu-id="6048e-127">{Subjektu}</span><span class="sxs-lookup"><span data-stu-id="6048e-127">{Subject}</span></span>|<span data-ttu-id="6048e-128">Řetězec k jednoznačné identifikaci přihlášení uživatele na webu</span><span class="sxs-lookup"><span data-stu-id="6048e-128">A string to uniquely identify the user logon across the web</span></span>|
| <span data-ttu-id="6048e-129">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="6048e-129">Tenant ID</span></span>| <span data-ttu-id="6048e-130">{Guid}</span><span class="sxs-lookup"><span data-stu-id="6048e-130">{Guid}</span></span>| <span data-ttu-id="6048e-131">A *guid* jednoznačně představující uživatele organizaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6048e-131">A *guid* to uniquely represent the user’s Azure Active Directory organization.</span></span>|

<span data-ttu-id="6048e-132">Kromě toho se zobrazí tabulku včetně všech deklarací identity uživatele zahrnuta v žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="6048e-132">In addition, you will see a table including all user claims included in authentication request.</span></span> <span data-ttu-id="6048e-133">Seznam všech deklarací identity do Id tokenu a vysvětlení najdete v tématu to [článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "seznam deklarací identity v Id tokenu").</span><span class="sxs-lookup"><span data-stu-id="6048e-133">For a list of all claims in an Id Token and explanation please see this [article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "List of Claims in Id Token").</span></span>


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a><span data-ttu-id="6048e-134">Test přístupu k metodě, která má *[Authorize]* atribut (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6048e-134">Test accessing a method that has an *[Authorize]* attribute (Optional)</span></span>
<span data-ttu-id="6048e-135">V tomto kroku budete testovat přístup k řadiči ověřený jako anonymní uživatel:</span><span class="sxs-lookup"><span data-stu-id="6048e-135">In this step, you will test accessing the Authenticated controller as an anonymous user:</span></span><br/>
<span data-ttu-id="6048e-136">Vyberte propojení na odhlášení uživatele a dokončete proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6048e-136">Select the link to sign-out the user and complete the sign-out process.</span></span><br/>
<span data-ttu-id="6048e-137">Teď v prohlížeči zadejte http://localhost: {port} / ověřený přístup k řadiči, která je chráněná pomocí `[Authorize]` atribut</span><span class="sxs-lookup"><span data-stu-id="6048e-137">Now in your browser, type http://localhost:{port}/authenticated to access your controller that is protected with the `[Authorize]` attribute</span></span>

#### <a name="expected-results"></a><span data-ttu-id="6048e-138">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="6048e-138">Expected results</span></span>
<span data-ttu-id="6048e-139">Měli byste obdržet řádku by bylo potřeba ověření zobrazíte zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6048e-139">You should receive the prompt requiring you to authenticate to see the view.</span></span>

## <a name="additional-information"></a><span data-ttu-id="6048e-140">Další informace</span><span class="sxs-lookup"><span data-stu-id="6048e-140">Additional information</span></span>

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a><span data-ttu-id="6048e-141">Chránit celý web</span><span class="sxs-lookup"><span data-stu-id="6048e-141">Protect your entire web site</span></span>
<span data-ttu-id="6048e-142">Chcete-li chránit celý web, přidejte `AuthorizeAttribute` k `GlobalFilters` v `Global.asax` `Application_Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="6048e-142">To protect your entire web site, add the `AuthorizeAttribute` to `GlobalFilters` in `Global.asax` `Application_Start` method:</span></span>

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> <span data-ttu-id="6048e-143">**Jak chcete zabránit uživatelům ve pouze jedna organizace k přihlášení do aplikace**</span><span class="sxs-lookup"><span data-stu-id="6048e-143">**How to restrict users from only one organization to sign in to your application**</span></span>

> <span data-ttu-id="6048e-144">Ve výchozím nastavení osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty v jakémkoli společnosti nebo organizace, která má integrované s Azure Active Directory můžete přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="6048e-144">By default, personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory can sign-in to your application.</span></span> 

> <span data-ttu-id="6048e-145">Pokud chcete, aby aplikace tak, aby přijímal přihlášení z pouze jedna organizace Azure Active Directory, nahraďte `Tenant` parametr v *web.config* z `Common` k názvu klienta organizace – příklad, *contoso.onmicrosoft.com*.</span><span class="sxs-lookup"><span data-stu-id="6048e-145">If you want your application to accept sign-ins from only one Azure Active Directory organization, replace the `Tenant` parameter in *web.config* from `Common` to the tenant name of the organization – example, *contoso.onmicrosoft.com*.</span></span> <span data-ttu-id="6048e-146">Potom změnit `ValidateIssuer` argument ve vaší *třídy pro spuštění OWIN* k `true`.</span><span class="sxs-lookup"><span data-stu-id="6048e-146">After that, change the `ValidateIssuer` argument in your *OWIN Startup class* to `true`.</span></span>

> <span data-ttu-id="6048e-147">Chcete-li povolit uživatelům jenom seznamu konkrétní organizací, nastavte `ValidateIssuer` na hodnotu true a použít `ValidIssuers` parametr pro zadání seznamu organizací.</span><span class="sxs-lookup"><span data-stu-id="6048e-147">To allow users from only a list of specific organizations, set `ValidateIssuer` to true and use the `ValidIssuers` parameter to specify a list of organizations.</span></span>

> <span data-ttu-id="6048e-148">Další možností je implementovat vlastní metodu k ověření pomocí parametru IssuerValidator vystavitele.</span><span class="sxs-lookup"><span data-stu-id="6048e-148">Another option is to implement a custom method to validate the issuers using IssuerValidator parameter.</span></span> <span data-ttu-id="6048e-149">Další informace o `TokenValidationParameters`, najdete v tématu [to](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "článku na webu MSDN parametry tokenvalidationparameters") článku na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="6048e-149">For more information about `TokenValidationParameters`, please see [this](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN article") MSDN article.</span></span>

