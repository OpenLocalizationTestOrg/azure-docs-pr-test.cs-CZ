---
title: "Azure AD v2 JS SPA na základě nastavení – Test | Microsoft Docs"
description: "Jak může aplikace JavaScript SPA volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: c888760ab311e8ac08b1e625bb837f91047db645
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
## <a name="test-your-code"></a><span data-ttu-id="9abd8-103">Otestujte svůj kód</span><span class="sxs-lookup"><span data-stu-id="9abd8-103">Test your code</span></span>

> ### <a name="testing-with-visual-studio"></a><span data-ttu-id="9abd8-104">Testování v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9abd8-104">Testing with Visual Studio</span></span>
> <span data-ttu-id="9abd8-105">Pokud používáte Visual Studio, stiskněte klávesu `F5` ke spuštění projektu: Otevře se prohlížeč a přesměruje vám *http://localhost: {port}* kde uvidíte *volání Microsoft Graph API* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9abd8-105">If you are using Visual Studio, press `F5` to run your project: the browser opens and directs you to *http://localhost:{port}* where you see the *Call Microsoft Graph API* button.</span></span>

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a><span data-ttu-id="9abd8-106">Testování v Pythonu nebo jiný webový server</span><span class="sxs-lookup"><span data-stu-id="9abd8-106">Testing with Python or another web server</span></span>
> <span data-ttu-id="9abd8-107">Pokud nepoužíváte Visual Studio, ujistěte se, váš webový server je spuštěna a je nakonfigurován pro naslouchání na portu TCP založené na složku, která obsahuje vaše *index.html* souboru.</span><span class="sxs-lookup"><span data-stu-id="9abd8-107">If you are not using Visual Studio, make sure your web server is started and it is configured to listen to a TCP port based on the folder containing your *index.html* file.</span></span> <span data-ttu-id="9abd8-108">Pro jazyk Python, můžete spustit naslouchání portu spuštěním v příkazu výzvy a Terminálový, ze složky aplikace:</span><span class="sxs-lookup"><span data-stu-id="9abd8-108">For Python, you can start listening to the port by running the in the command prompt/ terminal, from the app's folder:</span></span>
> 
> ```bash
> python -m http.server 8080
> ```
>  <span data-ttu-id="9abd8-109">Potom otevřete prohlížeč a zadejte *adrese http://localhost: 8080* nebo *http://localhost: {port}* – kde *port* odpovídá port, který váš webový server naslouchá.</span><span class="sxs-lookup"><span data-stu-id="9abd8-109">Then, open the browser and type *http://localhost:8080* or *http://localhost:{port}* - where the *port* corresponds to the port that your web server is listening to.</span></span> <span data-ttu-id="9abd8-110">Měli byste vidět obsah vaší stránka index.html se *volání Microsoft Graph API* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9abd8-110">You should see the contents of your index.html page with the *Call Microsoft Graph API* button.</span></span>

## <a name="test-your-application"></a><span data-ttu-id="9abd8-111">Testování vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="9abd8-111">Test your application</span></span>

<span data-ttu-id="9abd8-112">Po prohlížeč načte vaše *index.html*, klikněte *volání Microsoft Graph API* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9abd8-112">After the browser loads your *index.html*, click the *Call Microsoft Graph API* button.</span></span> <span data-ttu-id="9abd8-113">Pokud je poprvé, prohlížeč vás přesměruje na koncový bod v2 Microsoft Azure Active Directory, kde se zobrazí výzva k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9abd8-113">If this is the first time, the browser redirects you to the Microsoft Azure Active Directory v2 endpoint, where you are  prompted to sign in.</span></span>
 
![Snímek obrazovky](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a><span data-ttu-id="9abd8-115">Vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="9abd8-115">Consent</span></span>
<span data-ttu-id="9abd8-116">Velmi při prvním přihlášení do aplikace, se zobrazí obrazovky souhlasu podobná následující příkaz, kde je nutné přijmout:</span><span class="sxs-lookup"><span data-stu-id="9abd8-116">The very first time you sign in to your application, you are presented with a consent screen similar to the following, where you need to accept:</span></span>

 ![Souhlas obrazovky](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a><span data-ttu-id="9abd8-118">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="9abd8-118">Expected results</span></span>
<span data-ttu-id="9abd8-119">Měli byste vidět informace o profilu uživatele vrácený odpovědi volání Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="9abd8-119">You should see user profile information returned by the Microsoft Graph API call response.</span></span>
 
 ![Výsledky](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

<span data-ttu-id="9abd8-121">Můžete také zobrazit základní informace o tokenu získali v *tokenu přístupu* a *ID tokenu deklarací* polí.</span><span class="sxs-lookup"><span data-stu-id="9abd8-121">You also see basic information about the token acquired in the *Access Token* and *ID Token Claims* boxes.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="9abd8-122">Další informace o oborech a přidělená oprávnění</span><span class="sxs-lookup"><span data-stu-id="9abd8-122">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="9abd8-123">Vyžaduje rozhraní Microsoft Graph API `user.read` obory a čtení profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="9abd8-123">The Microsoft Graph API requires the `user.read` scope to read the user's profile.</span></span> <span data-ttu-id="9abd8-124">Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována.</span><span class="sxs-lookup"><span data-stu-id="9abd8-124">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="9abd8-125">Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory.</span><span class="sxs-lookup"><span data-stu-id="9abd8-125">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="9abd8-126">Například pro Microsoft Graph oboru `Calendars.Read` je potřeba seznamu kalendářích uživatele.</span><span class="sxs-lookup"><span data-stu-id="9abd8-126">For example, for Microsoft Graph, the scope `Calendars.Read` is required to list the user’s calendars.</span></span> <span data-ttu-id="9abd8-127">Chcete-li získat přístup k kalendáře uživatele v rámci aplikace, je nutné přidat `Calendars.Read` delegovaná oprávnění k registraci aplikace informace a poté přidejte `Calendars.Read` obor na `acquireTokenSilent` volání.</span><span class="sxs-lookup"><span data-stu-id="9abd8-127">In order to access the user’s calendar in the context of an application, you need to add the `Calendars.Read` delegated permission to the application registration’s information and then add the `Calendars.Read` scope to the `acquireTokenSilent` call.</span></span> <span data-ttu-id="9abd8-128">Zvýšit počet oborů, může být uživatel vyzván pro další souhlas všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9abd8-128">The user may be prompted for additional consents as you increase the number of scopes.</span></span>

<span data-ttu-id="9abd8-129">Pokud rozhraní API back-end nevyžaduje obor (nedoporučuje se), můžete použít `clientId` jako obor v `acquireTokenSilent` nebo `acquireTokenRedirect` volání.</span><span class="sxs-lookup"><span data-stu-id="9abd8-129">If a backend API does not require a scope (not recommended), you can use the `clientId` as the scope in the `acquireTokenSilent` and/or `acquireTokenRedirect` calls.</span></span>

<!--end-collapse-->
