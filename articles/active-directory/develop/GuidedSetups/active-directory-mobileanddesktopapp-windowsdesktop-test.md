---
title: "aaaAzure AD v2 Windows Desktop Začínáme - Test | Microsoft Docs"
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
ms.openlocfilehash: 0ae9612e1585c54a3fe35ba9d18f92554099b2c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="b20d1-103">Otestujte svůj kód</span><span class="sxs-lookup"><span data-stu-id="b20d1-103">Test your code</span></span>

<span data-ttu-id="b20d1-104">V pořadí tootest vaší aplikace, stiskněte klávesu `F5` toorun projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b20d1-104">In order tootest your application, press `F5` toorun your project in Visual Studio.</span></span> <span data-ttu-id="b20d1-105">Hlavní okno by měl zobrazit:</span><span class="sxs-lookup"><span data-stu-id="b20d1-105">Your Main Window should appear:</span></span>

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

<span data-ttu-id="b20d1-107">Když jste připravené tootest, klikněte na tlačítko *volání Microsoft Graph API* a používat službu Microsoft Azure Active Directory (účet organizace) nebo účet toosign Account Microsoft (live.com, outlook.com) v.</span><span class="sxs-lookup"><span data-stu-id="b20d1-107">When you're ready tootest, click *Call Microsoft Graph API* and use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account toosign in.</span></span> <span data-ttu-id="b20d1-108">Ho je hello poprvé, zobrazí se okno s dotazem, toosign uživatele v:</span><span class="sxs-lookup"><span data-stu-id="b20d1-108">It it is hello first time, you will see a window asking user toosign in:</span></span>

![Přihlášení](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a><span data-ttu-id="b20d1-110">Vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="b20d1-110">Consent</span></span>
<span data-ttu-id="b20d1-111">Hello při prvním přihlášení tooyour aplikace, zobrazí se souhlasu obrazovky podobné toohello níže, kde je nutné přijmout tooexplicitly:</span><span class="sxs-lookup"><span data-stu-id="b20d1-111">hello first time you sign in tooyour application, you will be presented with a consent screen similar toohello below, where you need tooexplicitly accept:</span></span>

![Souhlas obrazovky](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="b20d1-113">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="b20d1-113">Expected results</span></span>
<span data-ttu-id="b20d1-114">Měli byste vidět informace o profilu uživatele vrácený hello Microsoft Graph API volání na úvodní obrazovka výsledky volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b20d1-114">You should see user profile information returned by hello Microsoft Graph API call on hello API Call Results screen.</span></span>

<span data-ttu-id="b20d1-115">Měli byste taky vidět základní informace o tokenu hello získaných prostřednictvím `AcquireTokenAsync` nebo `AcquireTokenSilentAsync` hello informace o tokenu pole:</span><span class="sxs-lookup"><span data-stu-id="b20d1-115">You  should also see basic information about hello token acquired via `AcquireTokenAsync` or `AcquireTokenSilentAsync` in hello Token Info box:</span></span>

|<span data-ttu-id="b20d1-116">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b20d1-116">Property</span></span>  |<span data-ttu-id="b20d1-117">Formát</span><span class="sxs-lookup"><span data-stu-id="b20d1-117">Format</span></span>  |<span data-ttu-id="b20d1-118">Popis</span><span class="sxs-lookup"><span data-stu-id="b20d1-118">Description</span></span> |
|---------|---------|---------|
|<span data-ttu-id="b20d1-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b20d1-119">Name</span></span> | <span data-ttu-id="b20d1-120">{Úplné uživatelské jméno}</span><span class="sxs-lookup"><span data-stu-id="b20d1-120">{User Full name}</span></span> |<span data-ttu-id="b20d1-121">Hello uživatelské jméno a příjmení</span><span class="sxs-lookup"><span data-stu-id="b20d1-121">hello user’s first and last name</span></span>|
|<span data-ttu-id="b20d1-122">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="b20d1-122">Username</span></span> |<span>user@domain.com</span> |<span data-ttu-id="b20d1-123">uživatelské jméno Hello používá tooidentify hello uživatele</span><span class="sxs-lookup"><span data-stu-id="b20d1-123">hello username used tooidentify hello user</span></span>|
|<span data-ttu-id="b20d1-124">Platnost tokenu vyprší</span><span class="sxs-lookup"><span data-stu-id="b20d1-124">Token Expires</span></span> |<span data-ttu-id="b20d1-125">{{Data a času}</span><span class="sxs-lookup"><span data-stu-id="b20d1-125">{DateTime}</span></span>         |<span data-ttu-id="b20d1-126">Hello čas, na které hello vyprší platnost tokenu.</span><span class="sxs-lookup"><span data-stu-id="b20d1-126">hello time on which hello token expires.</span></span> <span data-ttu-id="b20d1-127">MSAL bude prodloužit datum vypršení platnosti hello vám obnovením hello token, pokud je to nezbytné</span><span class="sxs-lookup"><span data-stu-id="b20d1-127">MSAL will extend hello expiration date for you by renewing hello token when necessary</span></span>|
|<span data-ttu-id="b20d1-128">Přístupový token</span><span class="sxs-lookup"><span data-stu-id="b20d1-128">Access token</span></span> |<span data-ttu-id="b20d1-129">{Řetězec}</span><span class="sxs-lookup"><span data-stu-id="b20d1-129">{String}</span></span>         |<span data-ttu-id="b20d1-130">řetězec tokenu Hello odeslány, odešle tooHTTP požadavky, které vyžadují hlavičku autorizace</span><span class="sxs-lookup"><span data-stu-id="b20d1-130">hello token string sent that will be sent tooHTTP requests that require an authorization header</span></span>|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="b20d1-131">Další informace o oborech a přidělená oprávnění</span><span class="sxs-lookup"><span data-stu-id="b20d1-131">More information about scopes and delegated permissions</span></span>
<span data-ttu-id="b20d1-132">Rozhraní Graph API vyžaduje hello `user.read` obor tooread uživatelský profil.</span><span class="sxs-lookup"><span data-stu-id="b20d1-132">Graph API requires hello `user.read` scope tooread user profile.</span></span> <span data-ttu-id="b20d1-133">Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována.</span><span class="sxs-lookup"><span data-stu-id="b20d1-133">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="b20d1-134">Některé jiné rozhraní Graph API a také vlastní rozhraní API pro back-end serveru vyžadovat další obory.</span><span class="sxs-lookup"><span data-stu-id="b20d1-134">Some other Graph APIs as well as custom APIs for your backend server require additional scopes.</span></span> <span data-ttu-id="b20d1-135">Například pro graf `Calendars.Read` je kalendářích požadované toolist uživatele.</span><span class="sxs-lookup"><span data-stu-id="b20d1-135">For example, for Graph, `Calendars.Read` is required toolist user’s calendars.</span></span> <span data-ttu-id="b20d1-136">V pořadí tooaccess hello kalendáře uživatele v kontextu aplikace, musíte tooadd `Calendars.Read` delegovat informace o registraci aplikace a pak přidejte `Calendars.Read` toohello `AcquireTokenAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="b20d1-136">In order tooaccess hello user’s calendar in a context of an application, you need tooadd `Calendars.Read` delegated application registration’s information and then add `Calendars.Read` toohello `AcquireTokenAsync` call.</span></span> <span data-ttu-id="b20d1-137">Zvýšit číslo hello oborů, může být uživatel vyzván pro další souhlas všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b20d1-137">User may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->



