---
title: "aaaAdd ověřování v systému Android s Mobile Apps | Microsoft Docs"
description: "Zjistěte, jak toouse hello funkce Mobile Apps služby Azure App Service tooauthenticate uživatelů vaší aplikace Android prostřednictvím řady různých zprostředkovatelů identity, včetně Google, Facebook, Twitter a Microsoft."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 01f608f996c931c643790ed2778df11cf590c903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-android-app"></a><span data-ttu-id="011aa-103">Přidat ověřování tooyour aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="011aa-103">Add authentication tooyour Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="011aa-104">Souhrn</span><span class="sxs-lookup"><span data-stu-id="011aa-104">Summary</span></span>
<span data-ttu-id="011aa-105">V tomto kurzu přidáte toohello ověřování, seznamu úkolů projektu typu rychlý start v systému Android pomocí zprostředkovatele identity podporované.</span><span class="sxs-lookup"><span data-stu-id="011aa-105">In this tutorial, you add authentication toohello todolist quickstart project on Android by using a supported identity provider.</span></span> <span data-ttu-id="011aa-106">V tomto kurzu vychází z hello [Začínáme s Mobile Apps] kurz, který je třeba nejprve provést.</span><span class="sxs-lookup"><span data-stu-id="011aa-106">This tutorial is based on hello [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="011aa-107"><a name="register"></a>Registrace aplikace pro ověřování a konfigurace Azure App Service</span><span class="sxs-lookup"><span data-stu-id="011aa-107"><a name="register"></a>Register your app for authentication and configure Azure App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="011aa-108"><a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="011aa-108"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="011aa-109">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="011aa-109">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="011aa-110">To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="011aa-110">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="011aa-111">V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="011aa-111">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="011aa-112">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="011aa-112">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="011aa-113">Mělo by být jedinečný tooyour mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="011aa-113">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="011aa-114">tooenable hello přesměrování na straně serveru hello:</span><span class="sxs-lookup"><span data-stu-id="011aa-114">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="011aa-115">V [portál Azure] hello vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="011aa-115">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="011aa-116">Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="011aa-116">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="011aa-117">V hello **povoleno externí adres URL pro přesměrování**, zadejte `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="011aa-117">In hello **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="011aa-118">Hello _appname_ v tento řetězec je hello schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="011aa-118">hello _appname_ in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="011aa-119">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="011aa-119">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="011aa-120">Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="011aa-120">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="011aa-121">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="011aa-121">Click **OK**.</span></span>

5. <span data-ttu-id="011aa-122">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="011aa-122">Click **Save**.</span></span>

## <span data-ttu-id="011aa-123"><a name="permissions"></a>Omezte oprávnění tooauthenticated</span><span class="sxs-lookup"><span data-stu-id="011aa-123"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* <span data-ttu-id="011aa-124">V nástroji Android Studio otevřete projekt hello jste dokončili s hello kurzu [Začínáme s Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="011aa-124">In Android Studio, open hello project you completed with hello tutorial [Get started with Mobile Apps].</span></span> <span data-ttu-id="011aa-125">Z hello **spustit** nabídky, klikněte na tlačítko **spuštění aplikace**a ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="011aa-125">From hello **Run** menu, click **Run app**, and verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span>

     <span data-ttu-id="011aa-126">Tato výjimka se stane, protože pokusy aplikace hello tooaccess hello zpět ukončení jako neověřený uživatel, ale hello *TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="011aa-126">This exception happens because hello app attempts tooaccess hello back end as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="011aa-127">Potom aktualizujte uživatele tooauthenticate aplikace hello před vyžádáním prostředky z hello končit Mobile Apps zpět.</span><span class="sxs-lookup"><span data-stu-id="011aa-127">Next, you update hello app tooauthenticate users before requesting resources from hello Mobile Apps back end.</span></span> 

## <a name="add-authentication-toohello-app"></a><span data-ttu-id="011aa-128">Přidat aplikaci toohello ověřování</span><span class="sxs-lookup"><span data-stu-id="011aa-128">Add authentication toohello app</span></span>
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <span data-ttu-id="011aa-129"><a name="cache-tokens"></a>Tokeny ověřování mezipaměti na klientovi hello</span><span class="sxs-lookup"><span data-stu-id="011aa-129"><a name="cache-tokens"></a>Cache authentication tokens on hello client</span></span>
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="011aa-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="011aa-130">Next steps</span></span>
<span data-ttu-id="011aa-131">Teď, když jste dokončili kurz základní ověřování, zvažte, budete pokračovat v tooone hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="011aa-131">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="011aa-132">[Přidat nabízená oznámení tooyour Android aplikaci](app-service-mobile-android-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="011aa-132">[Add push notifications tooyour Android app](app-service-mobile-android-get-started-push.md).</span></span>
  <span data-ttu-id="011aa-133">Zjistěte, jak tooconfigure Mobile Apps zpět ukončení toouse Azure notification hubs toosend nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="011aa-133">Learn how tooconfigure your Mobile Apps back end toouse Azure notification hubs toosend push notifications.</span></span>
* <span data-ttu-id="011aa-134">[Zapnutí offline synchronizace pro svoji aplikaci pro Android](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="011aa-134">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="011aa-135">Zjistěte, jak tooadd offline podporovat tooyour aplikaci pomocí back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="011aa-135">Learn how tooadd offline support tooyour app by using a Mobile Apps back end.</span></span> <span data-ttu-id="011aa-136">S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="011aa-136">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions tooauthenticated users]: #permissions
[Add authentication toohello app]: #add-authentication
[Store authentication tokens on hello client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Začínáme s Mobile Apps]: app-service-mobile-android-get-started.md
