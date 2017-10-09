---
title: "iOS v2 aaaAzure AD Začínáme - Test | Microsoft Docs"
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
ms.openlocfilehash: 98c73eddabf9664feb19ac6878e9d7315b9aa79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="test-querying-hello-microsoft-graph-api-from-your-ios-application"></a><span data-ttu-id="848e0-103">Test dotazování hello Microsoft Graph API z vaší aplikace iOS</span><span class="sxs-lookup"><span data-stu-id="848e0-103">Test querying hello Microsoft Graph API from your iOS application</span></span>

<span data-ttu-id="848e0-104">Stiskněte klávesu `Command`  +  `R` toorun hello kód v simulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="848e0-104">Press `Command` + `R` toorun hello code in hello simulator.</span></span>

![Snímek obrazovky](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

<span data-ttu-id="848e0-106">Pokud jste připravené tootest, klepněte na *'volání Microsoft Graph API,* a bude výzvami tootype svoje uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="848e0-106">When you're ready tootest, tap *‘Call Microsoft Graph API’* and you will be prompted tootype your username and password.</span></span>

### <a name="consent"></a><span data-ttu-id="848e0-107">Vyjádřit souhlas.</span><span class="sxs-lookup"><span data-stu-id="848e0-107">Consent</span></span>
<span data-ttu-id="848e0-108">Hello při prvním přihlášení tooyour aplikace, zobrazí se souhlasu obrazovky podobné toohello níže, kde je nutné přijmout tooexplicitly:</span><span class="sxs-lookup"><span data-stu-id="848e0-108">hello first time you sign in tooyour application, you will be presented with a consent screen similar toohello below, where you need tooexplicitly accept:</span></span>

![Souhlas obrazovky](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a><span data-ttu-id="848e0-110">Očekávané výsledky</span><span class="sxs-lookup"><span data-stu-id="848e0-110">Expected results</span></span>
<span data-ttu-id="848e0-111">Měli byste vidět informace o profilu uživatele vrácený hello Microsoft Graph API volání v hello *protokolování* části.</span><span class="sxs-lookup"><span data-stu-id="848e0-111">You should see user profile information returned by hello Microsoft Graph API call in hello *Logging* section.</span></span>

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a><span data-ttu-id="848e0-112">Další informace o oborech a přidělená oprávnění</span><span class="sxs-lookup"><span data-stu-id="848e0-112">More information about scopes and delegated permissions</span></span>

<span data-ttu-id="848e0-113">Hello Microsoft Graph API vyžaduje hello `user.read` obor tooread hello uživatelský profil.</span><span class="sxs-lookup"><span data-stu-id="848e0-113">hello Microsoft Graph API requires hello `user.read` scope tooread hello user's profile.</span></span> <span data-ttu-id="848e0-114">Tento obor je automaticky přidán ve výchozím nastavení každá aplikace na náš portál registrace registrována.</span><span class="sxs-lookup"><span data-stu-id="848e0-114">This scope is automatically added by default in every application being registered on our registration portal.</span></span> <span data-ttu-id="848e0-115">Některé rozhraní API pro Microsoft Graph i vlastní rozhraní API pro back-end serveru může vyžadovat další obory.</span><span class="sxs-lookup"><span data-stu-id="848e0-115">Some other APIs for Microsoft Graph as well as custom APIs for your backend server may require additional scopes.</span></span> <span data-ttu-id="848e0-116">Například pro Microsoft Graph hello oboru `Calendars.Read` je kalendářích požadované toolist hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="848e0-116">For example, for Microsoft Graph, hello scope `Calendars.Read` is required toolist hello user’s calendars.</span></span> <span data-ttu-id="848e0-117">V pořadí tooaccess hello kalendáře uživatele v kontextu aplikace, musíte tooadd hello `Calendars.Read` delegovaná oprávnění toohello registrace aplikace na informace a poté přidejte hello `Calendars.Read` toohello oboru `acquireTokenSilent` volání.</span><span class="sxs-lookup"><span data-stu-id="848e0-117">In order tooaccess hello user’s calendar in a context of an application, you need tooadd hello `Calendars.Read` delegated permission toohello application registration’s information and then add hello `Calendars.Read` scope toohello `acquireTokenSilent` call.</span></span> <span data-ttu-id="848e0-118">zvýšit číslo hello oborů, může být Hello uživatel vyzván pro další souhlas všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="848e0-118">hello user may be prompted for additional consents as you increase hello number of scopes.</span></span>

<!--end-collapse-->



