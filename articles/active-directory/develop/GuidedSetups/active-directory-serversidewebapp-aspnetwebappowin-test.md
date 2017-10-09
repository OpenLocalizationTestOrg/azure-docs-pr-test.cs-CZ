---
title: "aaaAzure AD v2 ASP.NET Web Server Začínáme - Test | Microsoft Docs"
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
ms.openlocfilehash: 99c7525b9146605142180962fc2a61b3c953c064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="ecb03-103">Otestujte svůj kód</span><span class="sxs-lookup"><span data-stu-id="ecb03-103">Test your code</span></span>

<span data-ttu-id="ecb03-104">Stiskněte klávesu `F5` toorun projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ecb03-104">Press `F5` toorun your project in Visual Studio.</span></span> <span data-ttu-id="ecb03-105">Hello prohlížeč a směrovat je příliš*http://localhost: {port}* kde uvidíte hello *přihlásit pomocí Microsoft* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ecb03-105">hello browser will open and direct you too*http://localhost:{port}* where you’ll see hello *Sign in with Microsoft* button.</span></span> <span data-ttu-id="ecb03-106">Pokračujte a klikněte na něj toosign v.</span><span class="sxs-lookup"><span data-stu-id="ecb03-106">Go ahead and click it toosign in.</span></span>

<span data-ttu-id="ecb03-107">Až budete připravené tootest, použijte pracovní nebo školní (Azure Active Directory) nebo osobní (live.com, outlook.com) toosign v účtu.</span><span class="sxs-lookup"><span data-stu-id="ecb03-107">When you're ready tootest, use a work or school (Azure Active Directory), or a personal (live.com, outlook.com) account toosign in.</span></span> 

![Přihlaste se pomocí okna prohlížeče Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Přihlaste se pomocí okna prohlížeče Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a><span data-ttu-id="ecb03-110">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="ecb03-110">Expected results</span></span>
<span data-ttu-id="ecb03-111">Po přihlášení uživatel hello je přesměrovaného toohello domovskou stránku vašeho webu, který je hello adresy URL HTTPS zadaná v informace o registraci aplikace v portálu pro registraci aplikace Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="ecb03-111">After sign-in, hello user is redirected toohello home page of your web site which is hello HTTPS URL specified in your application registration information in hello Microsoft Application Registration Portal.</span></span> <span data-ttu-id="ecb03-112">Tato stránka se teď zobrazuje *Hello {uživatele}* a na odkaz toosign na více systémů a odkaz toosee hello deklarace identity uživatele – které je řadič odkaz toohello autorizovat vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ecb03-112">This page now shows *Hello {User}* and a link toosign-out, and a link toosee hello user’s claims – which is a link toohello Authorize controller created earlier.</span></span>

### <a name="see-users-claims"></a><span data-ttu-id="ecb03-113">V tématu deklarací identity uživatele</span><span class="sxs-lookup"><span data-stu-id="ecb03-113">See user's claims</span></span>
<span data-ttu-id="ecb03-114">Vyberte hello hyperlink toosee hello deklarací identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="ecb03-114">Select hello hyperlink toosee hello user's claims.</span></span> <span data-ttu-id="ecb03-115">To vás toohello řadiče a zobrazení, která je k dispozici toousers, který se ověřují jenom.</span><span class="sxs-lookup"><span data-stu-id="ecb03-115">This leads you toohello controller and view that is only available toousers that are authenticated.</span></span>

#### <a name="expected-results"></a><span data-ttu-id="ecb03-116">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="ecb03-116">Expected results</span></span>
 <span data-ttu-id="ecb03-117">Měli byste vidět tabulku obsahující základní vlastnosti hello hello přihlášení uživatele:</span><span class="sxs-lookup"><span data-stu-id="ecb03-117">You should see a table containing hello basic properties of hello logged user:</span></span>

| <span data-ttu-id="ecb03-118">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ecb03-118">Property</span></span> | <span data-ttu-id="ecb03-119">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ecb03-119">Value</span></span> | <span data-ttu-id="ecb03-120">Popis</span><span class="sxs-lookup"><span data-stu-id="ecb03-120">Description</span></span>|
|---|---|---|
| <span data-ttu-id="ecb03-121">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ecb03-121">Name</span></span> | <span data-ttu-id="ecb03-122">{Úplné uživatelské jméno}</span><span class="sxs-lookup"><span data-stu-id="ecb03-122">{User Full Name}</span></span> | <span data-ttu-id="ecb03-123">Hello uživatelské jméno a příjmení</span><span class="sxs-lookup"><span data-stu-id="ecb03-123">hello user’s first and last name</span></span>
|<span data-ttu-id="ecb03-124">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="ecb03-124">Username</span></span> | <span>user@domain.com</span>| <span data-ttu-id="ecb03-125">uživatelské jméno Hello používá tooidentify hello přihlášení uživatele</span><span class="sxs-lookup"><span data-stu-id="ecb03-125">hello username used tooidentify hello logged user</span></span>
| <span data-ttu-id="ecb03-126">Předmět</span><span class="sxs-lookup"><span data-stu-id="ecb03-126">Subject</span></span>| <span data-ttu-id="ecb03-127">{Subjektu}</span><span class="sxs-lookup"><span data-stu-id="ecb03-127">{Subject}</span></span>|<span data-ttu-id="ecb03-128">Řetězec toouniquely identifikovat hello přihlášení uživatele na hello web</span><span class="sxs-lookup"><span data-stu-id="ecb03-128">A string toouniquely identify hello user logon across hello web</span></span>|
| <span data-ttu-id="ecb03-129">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="ecb03-129">Tenant ID</span></span>| <span data-ttu-id="ecb03-130">{Guid}</span><span class="sxs-lookup"><span data-stu-id="ecb03-130">{Guid}</span></span>| <span data-ttu-id="ecb03-131">A *guid* toouniquely představují hello uživatele organizaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ecb03-131">A *guid* toouniquely represent hello user’s Azure Active Directory organization.</span></span>|

<span data-ttu-id="ecb03-132">Kromě toho se zobrazí tabulku včetně všech deklarací identity uživatele zahrnuta v žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="ecb03-132">In addition, you will see a table including all user claims included in authentication request.</span></span> <span data-ttu-id="ecb03-133">Seznam všech deklarací identity do Id tokenu a vysvětlení najdete v tématu to [článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "seznam deklarací identity v Id tokenu").</span><span class="sxs-lookup"><span data-stu-id="ecb03-133">For a list of all claims in an Id Token and explanation please see this [article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "List of Claims in Id Token").</span></span>


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a><span data-ttu-id="ecb03-134">Test přístupu k metodě, která má *[Authorize]* atribut (volitelné)</span><span class="sxs-lookup"><span data-stu-id="ecb03-134">Test accessing a method that has an *[Authorize]* attribute (Optional)</span></span>
<span data-ttu-id="ecb03-135">V tomto kroku budete testovat přístupem řadiče hello ověřený jako anonymní uživatel:</span><span class="sxs-lookup"><span data-stu-id="ecb03-135">In this step, you will test accessing hello Authenticated controller as an anonymous user:</span></span><br/>
<span data-ttu-id="ecb03-136">Vyberte hello propojit na více systémů toosign hello uživatele a dokončení hello proces přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ecb03-136">Select hello link toosign-out hello user and complete hello sign-out process.</span></span><br/>
<span data-ttu-id="ecb03-137">Teď v prohlížeči zadejte http://localhost: {port} / ověření tooaccess řadiči, která je chráněná pomocí hello `[Authorize]` atribut</span><span class="sxs-lookup"><span data-stu-id="ecb03-137">Now in your browser, type http://localhost:{port}/authenticated tooaccess your controller that is protected with hello `[Authorize]` attribute</span></span>

#### <a name="expected-results"></a><span data-ttu-id="ecb03-138">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="ecb03-138">Expected results</span></span>
<span data-ttu-id="ecb03-139">Měli byste obdržet hello řádku, které vyžadují tooauthenticate toosee hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ecb03-139">You should receive hello prompt requiring you tooauthenticate toosee hello view.</span></span>

## <a name="additional-information"></a><span data-ttu-id="ecb03-140">Další informace</span><span class="sxs-lookup"><span data-stu-id="ecb03-140">Additional information</span></span>

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a><span data-ttu-id="ecb03-141">Chránit celý web</span><span class="sxs-lookup"><span data-stu-id="ecb03-141">Protect your entire web site</span></span>
<span data-ttu-id="ecb03-142">tooprotect celý web, přidejte hello `AuthorizeAttribute` příliš`GlobalFilters` v `Global.asax` `Application_Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="ecb03-142">tooprotect your entire web site, add hello `AuthorizeAttribute` too`GlobalFilters` in `Global.asax` `Application_Start` method:</span></span>

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> <span data-ttu-id="ecb03-143">**Jak uživatelé toorestrict z toosign pouze jedna organizace v aplikaci tooyour**</span><span class="sxs-lookup"><span data-stu-id="ecb03-143">**How toorestrict users from only one organization toosign in tooyour application**</span></span>

> <span data-ttu-id="ecb03-144">Ve výchozím nastavení, osobní účty (včetně outlook.com, live.com a dalších), jakož i pracovním a školním účtům v jakémkoli společnosti nebo organizace, která má integrované s Azure Active Directory můžete přihlásit tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="ecb03-144">By default, personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory can sign-in tooyour application.</span></span> 

> <span data-ttu-id="ecb03-145">Pokud chcete, aby vaše aplikace tooaccept přihlášení z pouze jedna organizace Azure Active Directory, nahraďte hello `Tenant` parametr v *web.config* z `Common` název klienta toohello hello organizace – například *contoso.onmicrosoft.com*. Potom změnit hello `ValidateIssuer` argument ve vaší *třídy pro spuštění OWIN* příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="ecb03-145">If you want your application tooaccept sign-ins from only one Azure Active Directory organization, replace hello `Tenant` parameter in *web.config* from `Common` toohello tenant name of hello organization – example, *contoso.onmicrosoft.com*. After that, change hello `ValidateIssuer` argument in your *OWIN Startup class* too`true`.</span></span>

> <span data-ttu-id="ecb03-146">nastavit tooallow uživatele ze seznamu pouze konkrétní organizací `ValidateIssuer` tootrue a používání hello `ValidIssuers` parametr toospecify seznam organizace.</span><span class="sxs-lookup"><span data-stu-id="ecb03-146">tooallow users from only a list of specific organizations, set `ValidateIssuer` tootrue and use hello `ValidIssuers` parameter toospecify a list of organizations.</span></span>

> <span data-ttu-id="ecb03-147">Další možností je vlastní metoda tooimplement vystavitelů hello toovalidate pomocí parametru IssuerValidator.</span><span class="sxs-lookup"><span data-stu-id="ecb03-147">Another option is tooimplement a custom method toovalidate hello issuers using IssuerValidator parameter.</span></span> <span data-ttu-id="ecb03-148">Další informace o `TokenValidationParameters`, najdete v tématu [to](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "článku na webu MSDN parametry tokenvalidationparameters") článku na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="ecb03-148">For more information about `TokenValidationParameters`, please see [this](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN article") MSDN article.</span></span>

