---
title: "aaaAzure AD v2 Android Začínáme - Test | Microsoft Docs"
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
ms.openlocfilehash: 499f32b46fd44cca0e52179bced49b311135d8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="88afb-103">Otestujte svůj kód</span><span class="sxs-lookup"><span data-stu-id="88afb-103">Test your code</span></span>

1. <span data-ttu-id="88afb-104">Nasaďte vaše zařízení nebo emulátoru tooyour kódu.</span><span class="sxs-lookup"><span data-stu-id="88afb-104">Deploy your code tooyour device/emulator.</span></span>
2. <span data-ttu-id="88afb-105">Až budete připravené tootest, použijte Microsoft Azure Active Directory (účet organizace) nebo účet toosign Account Microsoft (live.com, outlook.com) v.</span><span class="sxs-lookup"><span data-stu-id="88afb-105">When you're ready tootest, use a Microsoft Azure Active Directory (organizational account) or a Microsoft Account (live.com, outlook.com) account toosign in.</span></span> 

<span data-ttu-id="88afb-106">![Snímek obrazovky](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span><span class="sxs-lookup"><span data-stu-id="88afb-106">![Sample screen shot](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
</span></span><br/><br/><span data-ttu-id="88afb-107">
![Přihlášení](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span><span class="sxs-lookup"><span data-stu-id="88afb-107">
![Sign-in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)</span></span>

### <a name="consent"></a><span data-ttu-id="88afb-108">Vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="88afb-108">Consent</span></span>
<span data-ttu-id="88afb-109">Hello poprvé, co se uživatel přihlásí tooyour aplikace, zobrazí se souhlasu obrazovky podobné toohello níže, kde potřebují přijmout tooexplicitly:</span><span class="sxs-lookup"><span data-stu-id="88afb-109">hello first time a user signs in tooyour application, they will be presented with a consent screen similar toohello below, where they need tooexplicitly accept:</span></span> 

![Vyjádřit souhlas.](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a><span data-ttu-id="88afb-111">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="88afb-111">Expected results</span></span>
<span data-ttu-id="88afb-112">Měli byste vidět hello výsledky tooMicrosoft volání rozhraní Graph API, mi používá koncový bod profilu uživatele hello tootooobtain - https://graph.microsoft.com/v1.0/me.</span><span class="sxs-lookup"><span data-stu-id="88afb-112">You should see hello results of a call tooMicrosoft Graph API ‘me’ endpoint used tootooobtain hello user profile - https://graph.microsoft.com/v1.0/me.</span></span> <span data-ttu-id="88afb-113">Seznam běžných Microsoft Graph koncové body, najdete v tématu to [článku](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span><span class="sxs-lookup"><span data-stu-id="88afb-113">For a list of common Microsoft Graph endpoints, please see this [article](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="88afb-114">Další informace o oborech a přidělená oprávnění</span><span class="sxs-lookup"><span data-stu-id="88afb-114">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="88afb-115">Hello Microsoft Graph API vyžaduje hello `user.read` obor tooread hello uživatelský profil.</span><span class="sxs-lookup"><span data-stu-id="88afb-115">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="88afb-116">Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována.</span><span class="sxs-lookup"><span data-stu-id="88afb-116">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="88afb-117">Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory.</span><span class="sxs-lookup"><span data-stu-id="88afb-117">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="88afb-118">Například pro Microsoft Graph hello oboru `Calendars.Read` je kalendářích požadované toolist hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="88afb-118">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="88afb-119">V pořadí tooaccess hello kalendáře uživatele v kontextu aplikace, musíte tooadd hello `Calendars.Read` delegovaná oprávnění toohello registrace aplikace na informace a poté přidejte hello `Calendars.Read` toohello oboru `acquireTokenSilentAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="88afb-119">In order tooaccess hello user’s calendar in a context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilentAsync` call.</span></span> <span data-ttu-id="88afb-120">zvýšit číslo hello oborů, může být Hello uživatel vyzván pro další souhlas všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="88afb-120">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->
