---
title: "Azure AD v2 iOS Začínáme - Test | Microsoft Docs"
description: "Jak aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 4a88096d2b0a23708acdbc1798eac528599b4f71
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a><span data-ttu-id="c4b2f-103">Test dotazování Microsoft Graph API z vaší aplikace iOS</span><span class="sxs-lookup"><span data-stu-id="c4b2f-103">Test querying the Microsoft Graph API from your iOS application</span></span>

<span data-ttu-id="c4b2f-104">Stiskněte klávesu `Command`  +  `R` spustí kód v simulátoru.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-104">Press `Command` + `R` to run the code in the simulator.</span></span>

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

<span data-ttu-id="c4b2f-106">Až budete připraveni k testování, klepněte na *'volání Microsoft Graph API,* a zobrazí se výzva k zadání uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-106">When you're ready to test, tap *‘Call Microsoft Graph API’* and you will be prompted to type your username and password.</span></span>

### <a name="consent"></a><span data-ttu-id="c4b2f-107">Vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-107">Consent</span></span>
<span data-ttu-id="c4b2f-108">Při prvním přihlášení do aplikace, zobrazí obrazovka podobná souhlasu s níže, kde je potřeba explicitně přijmout:</span><span class="sxs-lookup"><span data-stu-id="c4b2f-108">The first time you sign in to your application, you will be presented with a consent screen similar to the below, where you need to explicitly accept:</span></span>

![Souhlas obrazovky](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="c4b2f-110">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="c4b2f-110">Expected results</span></span>
<span data-ttu-id="c4b2f-111">Informace o profilu uživatele vrácený Microsoft Graph API volat v byste měli vidět *protokolování* části.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-111">You should see user profile information returned by the Microsoft Graph API call in the *Logging* section.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="c4b2f-112">Další informace o oborech a přidělená oprávnění</span><span class="sxs-lookup"><span data-stu-id="c4b2f-112">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="c4b2f-113">Vyžaduje rozhraní Microsoft Graph API `user.read` obory a čtení profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-113">The Microsoft Graph API requires the `user.read` scope to read the user's profile.</span></span> <span data-ttu-id="c4b2f-114">Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-114">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="c4b2f-115">Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-115">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="c4b2f-116">Například pro Microsoft Graph oboru `Calendars.Read` je potřeba seznamu kalendářích uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-116">For example, for Microsoft Graph, the scope `Calendars.Read` is required to list the user’s calendars.</span></span> <span data-ttu-id="c4b2f-117">Chcete-li získat přístup k kalendáře uživatele v kontextu aplikace, je nutné přidat `Calendars.Read` delegovaná oprávnění k registraci aplikace informace a poté přidejte `Calendars.Read` obor na `acquireTokenSilent` volání.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-117">In order to access the user’s calendar in a context of an application, you need to add the `Calendars.Read` delegated permission to the application registration’s information and then add the `Calendars.Read` scope to the `acquireTokenSilent` call.</span></span> <span data-ttu-id="c4b2f-118">Zvýšit počet oborů, může být uživatel vyzván pro další souhlas všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c4b2f-118">The user may be prompted for additional consents as you increase the number of scopes.</span></span>

<!--end-collapse-->



