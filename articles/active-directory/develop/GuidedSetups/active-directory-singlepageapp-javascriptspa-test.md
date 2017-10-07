---
title: "aaaAzure AD v2 JS SPA na základě nastavení – Test | Microsoft Docs"
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
ms.openlocfilehash: b2339431a070b5c4ad4058e6c1a9b19b83c84c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="e6d34-103">Otestujte svůj kód</span><span class="sxs-lookup"><span data-stu-id="e6d34-103">Test your code</span></span>

> ### <a name="testing-with-visual-studio"></a><span data-ttu-id="e6d34-104">Testování v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d34-104">Testing with Visual Studio</span></span>
> <span data-ttu-id="e6d34-105">Pokud používáte Visual Studio, stiskněte klávesu `F5` toorun projektu: hello prohlížeč otevře a přesměruje je příliš*http://localhost: {port}* kde uvidíte hello *volání Microsoft Graph API* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e6d34-105">If you are using Visual Studio, press `F5` toorun your project: hello browser opens and directs you too*http://localhost:{port}* where you see hello *Call Microsoft Graph API* button.</span></span>

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a><span data-ttu-id="e6d34-106">Testování v Pythonu nebo jiný webový server</span><span class="sxs-lookup"><span data-stu-id="e6d34-106">Testing with Python or another web server</span></span>
> <span data-ttu-id="e6d34-107">Pokud nepoužíváte Visual Studio, ujistěte se, váš webový server je spuštěna a je nakonfigurovaný toolisten tooa TCP port podle hello složku, která obsahuje vaše *index.html* souboru.</span><span class="sxs-lookup"><span data-stu-id="e6d34-107">If you are not using Visual Studio, make sure your web server is started and it is configured toolisten tooa TCP port based on hello folder containing your *index.html* file.</span></span> <span data-ttu-id="e6d34-108">Pro jazyk Python, můžete zahájit naslouchání portu toohello spuštěním hello v příkazu hello výzvy a Terminálový, ze složky aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="e6d34-108">For Python, you can start listening toohello port by running hello in hello command prompt/ terminal, from hello app's folder:</span></span>
> 
> ```bash
> python -m http.server 8080
> ```
>  <span data-ttu-id="e6d34-109">Potom otevřete hello prohlížeč a zadejte *adrese http://localhost: 8080* nebo *http://localhost: {port}* – kde hello *port* odpovídá toohello port, který je váš webový server naslouchání.</span><span class="sxs-lookup"><span data-stu-id="e6d34-109">Then, open hello browser and type *http://localhost:8080* or *http://localhost:{port}* - where hello *port* corresponds toohello port that your web server is listening to.</span></span> <span data-ttu-id="e6d34-110">Měli byste vidět hello obsah vaší stránka index.html s hello *volání Microsoft Graph API* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e6d34-110">You should see hello contents of your index.html page with hello *Call Microsoft Graph API* button.</span></span>

## <a name="test-your-application"></a><span data-ttu-id="e6d34-111">Testování vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="e6d34-111">Test your application</span></span>

<span data-ttu-id="e6d34-112">Po hello prohlížeč načte vaše *index.html*, klikněte na tlačítko hello *volání Microsoft Graph API* tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e6d34-112">After hello browser loads your *index.html*, click hello *Call Microsoft Graph API* button.</span></span> <span data-ttu-id="e6d34-113">Pokud je to hello poprvé, přesměrování prohlížeče hello je toohello Microsoft Azure Active Directory v2 koncovému bodu, kde se zobrazí výzva toosign v.</span><span class="sxs-lookup"><span data-stu-id="e6d34-113">If this is hello first time, hello browser redirects you toohello Microsoft Azure Active Directory v2 endpoint, where you are  prompted toosign in.</span></span>
 
![Snímek obrazovky](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a><span data-ttu-id="e6d34-115">Vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="e6d34-115">Consent</span></span>
<span data-ttu-id="e6d34-116">Hello velmi při prvním přihlášení tooyour aplikace se zobrazí souhlasu obrazovky podobné toohello následující, kde je nutné tooaccept:</span><span class="sxs-lookup"><span data-stu-id="e6d34-116">hello very first time you sign in tooyour application, you are presented with a consent screen similar toohello following, where you need tooaccept:</span></span>

 ![Souhlas obrazovky](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a><span data-ttu-id="e6d34-118">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="e6d34-118">Expected results</span></span>
<span data-ttu-id="e6d34-119">Měli byste vidět informace o profilu uživatele vrácený hello odpovědi volání Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="e6d34-119">You should see user profile information returned by hello Microsoft Graph API call response.</span></span>
 
 ![Výsledky](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

<span data-ttu-id="e6d34-121">Můžete také zobrazit základní informace o tokenu hello získali v hello *tokenu přístupu* a *ID tokenu deklarací* polí.</span><span class="sxs-lookup"><span data-stu-id="e6d34-121">You also see basic information about hello token acquired in hello *Access Token* and *ID Token Claims* boxes.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="e6d34-122">Další informace o oborech a přidělená oprávnění</span><span class="sxs-lookup"><span data-stu-id="e6d34-122">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="e6d34-123">Hello Microsoft Graph API vyžaduje hello `user.read` obor tooread hello uživatelský profil.</span><span class="sxs-lookup"><span data-stu-id="e6d34-123">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="e6d34-124">Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována.</span><span class="sxs-lookup"><span data-stu-id="e6d34-124">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="e6d34-125">Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory.</span><span class="sxs-lookup"><span data-stu-id="e6d34-125">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="e6d34-126">Například pro Microsoft Graph hello oboru `Calendars.Read` je kalendářích požadované toolist hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="e6d34-126">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="e6d34-127">V pořadí tooaccess hello kalendáře uživatele v kontextu hello aplikace, musíte tooadd hello `Calendars.Read` delegovaná oprávnění toohello registrace aplikace na informace a poté přidejte hello `Calendars.Read` toohello oboru `acquireTokenSilent` volání.</span><span class="sxs-lookup"><span data-stu-id="e6d34-127">In order tooaccess hello user’s calendar in hello context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilent` call.</span></span> <span data-ttu-id="e6d34-128">zvýšit číslo hello oborů, může být Hello uživatel vyzván pro další souhlas všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e6d34-128">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<span data-ttu-id="e6d34-129">Pokud rozhraní API back-end nevyžaduje obor (nedoporučuje se), můžete použít hello `clientId` jako obor hello v hello `acquireTokenSilent` nebo `acquireTokenRedirect` volání.</span><span class="sxs-lookup"><span data-stu-id="e6d34-129">If a backend API does not require a scope (not recommended), you can use hello `clientId` as hello scope in hello `acquireTokenSilent` and/or `acquireTokenRedirect` calls.</span></span>

<!--end-collapse-->
