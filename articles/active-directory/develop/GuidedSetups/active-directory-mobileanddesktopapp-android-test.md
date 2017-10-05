---
title: "Azure AD v2 Android získávání spuštěno – testování | Microsoft Docs"
description: "Jak získat přístupový token a volání rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nebo Microsoft Graph API aplikace pro Android"
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
ms.openlocfilehash: 6df64f4820f8409bd8897d5ac24f81bffeeef102
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a><span data-ttu-id="b140b-103">Otestujte svůj kód</span><span class="sxs-lookup"><span data-stu-id="b140b-103">Test your code</span></span>

1. <span data-ttu-id="b140b-104">Nasazení kódu do vašeho zařízení nebo emulátor.</span><span class="sxs-lookup"><span data-stu-id="b140b-104">Deploy your code to your device/emulator.</span></span>
2. <span data-ttu-id="b140b-105">Až budete připraveni k testování, používáte pro přihlášení Microsoft Azure Active Directory (účet organizace) nebo účet Account Microsoft (live.com, outlook.com).</span><span class="sxs-lookup"><span data-stu-id="b140b-105">When you're ready to test, use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account to sign in.</span></span> 

<span data-ttu-id="b140b-106">![Snímek obrazovky](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span><span class="sxs-lookup"><span data-stu-id="b140b-106">![Sample screen shot](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span></span><br/><br/><span data-ttu-id="b140b-107">
![Přihlášení](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span><span class="sxs-lookup"><span data-stu-id="b140b-107">
![Sign-in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span></span>

### <a name="consent"></a><span data-ttu-id="b140b-108">Vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="b140b-108">Consent</span></span>
<span data-ttu-id="b140b-109">Při prvním přihlášení uživatele k vaší aplikaci, se zobrazí obrazovka podobná souhlasu s níže, kde se musí explicitně přijmout:</span><span class="sxs-lookup"><span data-stu-id="b140b-109">The first time a user signs in to your application, they will be presented with a consent screen similar to the below, where they need to explicitly accept:</span></span> 

![Vyjádřit souhlas.](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a><span data-ttu-id="b140b-111">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="b140b-111">Expected results</span></span>
<span data-ttu-id="b140b-112">Měli byste vidět výsledky volání Microsoft Graph API, mi na použije k získání uživatelský profil - https://graph.microsoft.com/v1.0/me koncový bod.</span><span class="sxs-lookup"><span data-stu-id="b140b-112">You should see the results of a call to Microsoft Graph API ‘me’ endpoint used to to obtain the user profile - https://graph.microsoft.com/v1.0/me.</span></span> <span data-ttu-id="b140b-113">Seznam běžných Microsoft Graph koncové body, najdete v tématu to [článku](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span><span class="sxs-lookup"><span data-stu-id="b140b-113">For a list of common Microsoft Graph endpoints, please see this [article](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="b140b-114">Další informace o oborech a přidělená oprávnění</span><span class="sxs-lookup"><span data-stu-id="b140b-114">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="b140b-115">Vyžaduje rozhraní Microsoft Graph API `user.read` obory a čtení profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="b140b-115">The Microsoft Graph API requires the `user.read` scope to read the user's profile.</span></span> <span data-ttu-id="b140b-116">Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována.</span><span class="sxs-lookup"><span data-stu-id="b140b-116">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="b140b-117">Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory.</span><span class="sxs-lookup"><span data-stu-id="b140b-117">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="b140b-118">Například pro Microsoft Graph oboru `Calendars.Read` je potřeba seznamu kalendářích uživatele.</span><span class="sxs-lookup"><span data-stu-id="b140b-118">For example, for Microsoft Graph, the scope `Calendars.Read` is required to list the user’s calendars.</span></span> <span data-ttu-id="b140b-119">Chcete-li získat přístup k kalendáře uživatele v kontextu aplikace, je nutné přidat `Calendars.Read` delegovaná oprávnění k registraci aplikace informace a poté přidejte `Calendars.Read` obor na `acquireTokenSilentAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="b140b-119">In order to access the user’s calendar in a context of an application, you need to add the `Calendars.Read` delegated permission to the application registration’s information and then add the `Calendars.Read` scope to the `acquireTokenSilentAsync` call.</span></span> <span data-ttu-id="b140b-120">Zvýšit počet oborů, může být uživatel vyzván pro další souhlas všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b140b-120">The user may be prompted for additional consents as you increase the number of scopes.</span></span>

<!--end-collapse-->
