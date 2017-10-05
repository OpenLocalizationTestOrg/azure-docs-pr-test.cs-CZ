---
title: "Azure AD v2 Windows Desktop získávání spuštěno – testování | Microsoft Docs"
description: "Jak aplikace Windows Desktop .NET (XAML) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
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
ms.openlocfilehash: 972cc48057c13271d725b0c973c3ccf651ad27c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a><span data-ttu-id="f3c48-103">Otestujte svůj kód</span><span class="sxs-lookup"><span data-stu-id="f3c48-103">Test your code</span></span>

<span data-ttu-id="f3c48-104">Chcete-li otestovat aplikaci, stiskněte klávesu `F5` spustit projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f3c48-104">In order to test your application, press `F5` to run your project in Visual Studio.</span></span> <span data-ttu-id="f3c48-105">Hlavní okno by měl zobrazit:</span><span class="sxs-lookup"><span data-stu-id="f3c48-105">Your Main Window should appear:</span></span>

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

<span data-ttu-id="f3c48-107">Až budete připraveni k testování, klikněte na tlačítko *volání Microsoft Graph API* a přihlaste se pomocí Microsoft Azure Active Directory (účet organizace) nebo účet Account Microsoft (live.com, outlook.com).</span><span class="sxs-lookup"><span data-stu-id="f3c48-107">When you're ready to test, click *Call Microsoft Graph API* and use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account to sign in.</span></span> <span data-ttu-id="f3c48-108">Ho je poprvé, zobrazí se okno s výzvou, uživatel k přihlášení:</span><span class="sxs-lookup"><span data-stu-id="f3c48-108">It it is the first time, you will see a window asking user to sign in:</span></span>

![Přihlášení](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a><span data-ttu-id="f3c48-110">Vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="f3c48-110">Consent</span></span>
<span data-ttu-id="f3c48-111">Při prvním přihlášení do aplikace, zobrazí obrazovka podobná souhlasu s níže, kde je potřeba explicitně přijmout:</span><span class="sxs-lookup"><span data-stu-id="f3c48-111">The first time you sign in to your application, you will be presented with a consent screen similar to the below, where you need to explicitly accept:</span></span>

![Souhlas obrazovky](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="f3c48-113">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="f3c48-113">Expected results</span></span>
<span data-ttu-id="f3c48-114">Měli byste vidět informace o profilu uživatele vrácený Microsoft Graph API volání na obrazovce výsledky volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f3c48-114">You should see user profile information returned by the Microsoft Graph API call on the API Call Results screen.</span></span>

<span data-ttu-id="f3c48-115">Měli byste taky vidět základní informace o tokenu získaných prostřednictvím `AcquireTokenAsync` nebo `AcquireTokenSilentAsync` informace o tokenu pole:</span><span class="sxs-lookup"><span data-stu-id="f3c48-115">You  should also see basic information about the token acquired via `AcquireTokenAsync` or `AcquireTokenSilentAsync` in the Token Info box:</span></span>

|<span data-ttu-id="f3c48-116">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f3c48-116">Property</span></span>  |<span data-ttu-id="f3c48-117">Formát</span><span class="sxs-lookup"><span data-stu-id="f3c48-117">Format</span></span>  |<span data-ttu-id="f3c48-118">Popis</span><span class="sxs-lookup"><span data-stu-id="f3c48-118">Description</span></span> |
|---------|---------|---------|
|<span data-ttu-id="f3c48-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f3c48-119">Name</span></span> | <span data-ttu-id="f3c48-120">{Úplné uživatelské jméno}</span><span class="sxs-lookup"><span data-stu-id="f3c48-120">{User Full name}</span></span> |<span data-ttu-id="f3c48-121">Jméno a příjmení uživatele</span><span class="sxs-lookup"><span data-stu-id="f3c48-121">The user’s first and last name</span></span>|
|<span data-ttu-id="f3c48-122">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="f3c48-122">Username</span></span> |<span>user@domain.com</span> |<span data-ttu-id="f3c48-123">Uživatelské jméno používá k identifikaci uživatele</span><span class="sxs-lookup"><span data-stu-id="f3c48-123">The username used to identify the user</span></span>|
|<span data-ttu-id="f3c48-124">Platnost tokenu vyprší</span><span class="sxs-lookup"><span data-stu-id="f3c48-124">Token Expires</span></span> |<span data-ttu-id="f3c48-125">{{Data a času}</span><span class="sxs-lookup"><span data-stu-id="f3c48-125">{DateTime}</span></span>         |<span data-ttu-id="f3c48-126">Čas, kdy vyprší platnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="f3c48-126">The time on which the token expires.</span></span> <span data-ttu-id="f3c48-127">MSAL bude prodloužit datum vypršení platnosti pro vás obnovením token, pokud je to nezbytné</span><span class="sxs-lookup"><span data-stu-id="f3c48-127">MSAL will extend the expiration date for you by renewing the token when necessary</span></span>|
|<span data-ttu-id="f3c48-128">Přístupový token</span><span class="sxs-lookup"><span data-stu-id="f3c48-128">Access token</span></span> |<span data-ttu-id="f3c48-129">{Řetězec}</span><span class="sxs-lookup"><span data-stu-id="f3c48-129">{String}</span></span>         |<span data-ttu-id="f3c48-130">Řetězec tokenu odeslány, budou odeslány na požadavky HTTP, které vyžadují hlavičku autorizace</span><span class="sxs-lookup"><span data-stu-id="f3c48-130">The token string sent that will be sent to HTTP requests that require an authorization header</span></span>|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="f3c48-131">Další informace o oborech a přidělená oprávnění</span><span class="sxs-lookup"><span data-stu-id="f3c48-131">More information about scopes and delegated permissions</span></span>
<span data-ttu-id="f3c48-132">Rozhraní Graph API vyžaduje `user.read` obory a čtení profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f3c48-132">Graph API requires the `user.read` scope to read user profile.</span></span> <span data-ttu-id="f3c48-133">Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována.</span><span class="sxs-lookup"><span data-stu-id="f3c48-133">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="f3c48-134">Některé jiné rozhraní Graph API a také vlastní rozhraní API pro back-end serveru vyžadovat další obory.</span><span class="sxs-lookup"><span data-stu-id="f3c48-134">Some other Graph APIs as well as custom APIs for your backend server require additional scopes.</span></span> <span data-ttu-id="f3c48-135">Například pro graf `Calendars.Read` je vyžadován ke kalendářům uživatele seznamu.</span><span class="sxs-lookup"><span data-stu-id="f3c48-135">For example, for Graph, `Calendars.Read` is required to list user’s calendars.</span></span> <span data-ttu-id="f3c48-136">Chcete-li získat přístup k kalendáře uživatele v kontextu aplikace, budete muset přidat `Calendars.Read` delegovat informace o registraci aplikace a pak přidejte `Calendars.Read` k `AcquireTokenAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="f3c48-136">In order to access the user’s calendar in a context of an application, you need to add `Calendars.Read` delegated application registration’s information and then add `Calendars.Read` to the `AcquireTokenAsync` call.</span></span> <span data-ttu-id="f3c48-137">Zvýšit počet oborů, může být uživatel vyzván pro další souhlas všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f3c48-137">User may be prompted for additional consents as you increase the number of scopes.</span></span>

<!--end-collapse-->



