---
title: "Přidání ověřování v systému Android s Mobile Apps | Microsoft Docs"
description: "Další informace o použití funkce Mobile Apps služby Azure App Service ověřovat uživatele vaší aplikace pro Android prostřednictvím řady různých zprostředkovatelů identity, včetně Google, Facebook, Twitter a společnosti Microsoft."
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
ms.openlocfilehash: 81331142aa6110d4e29e6fb30a90ce6e3a853439
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-android-app"></a><span data-ttu-id="a2f44-103">Přidání ověřování do aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="a2f44-103">Add authentication to your Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="a2f44-104">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a2f44-104">Summary</span></span>
<span data-ttu-id="a2f44-105">V tomto kurzu jste přidání ověřování do seznamu úkolů projektu pro rychlý start v systému Android pomocí zprostředkovatele identity podporované.</span><span class="sxs-lookup"><span data-stu-id="a2f44-105">In this tutorial, you add authentication to the todolist quickstart project on Android by using a supported identity provider.</span></span> <span data-ttu-id="a2f44-106">V tomto kurzu vychází z [Začínáme s Mobile Apps] kurz, který je třeba nejprve provést.</span><span class="sxs-lookup"><span data-stu-id="a2f44-106">This tutorial is based on the [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="a2f44-107"><a name="register"></a>Registrace aplikace pro ověřování a konfigurace Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a2f44-107"><a name="register"></a>Register your app for authentication and configure Azure App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="a2f44-108"><a name="redirecturl"></a>Přidání aplikace do adresy URL pro povolené externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="a2f44-108"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="a2f44-109">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a2f44-109">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="a2f44-110">To umožňuje ověřování systému přesměrovat zpět do aplikace po dokončení procesu ověřování.</span><span class="sxs-lookup"><span data-stu-id="a2f44-110">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="a2f44-111">V tomto kurzu používáme schématu adresy URL _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="a2f44-111">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="a2f44-112">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="a2f44-112">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="a2f44-113">Musí být jedinečné pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2f44-113">It should be unique to your mobile application.</span></span> <span data-ttu-id="a2f44-114">Chcete povolit přesměrování na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="a2f44-114">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="a2f44-115">V [portál Azure] vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="a2f44-115">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="a2f44-116">Klikněte **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="a2f44-116">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="a2f44-117">V **povoleno externí adres URL pro přesměrování**, zadejte `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="a2f44-117">In the **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="a2f44-118">_Appname_ v tento řetězec je schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2f44-118">The _appname_ in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="a2f44-119">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="a2f44-119">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="a2f44-120">Měli byste si poznamenat řetězce, který zvolíte, jako je třeba upravit kód mobilní aplikace s schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="a2f44-120">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="a2f44-121">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2f44-121">Click **OK**.</span></span>

5. <span data-ttu-id="a2f44-122">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a2f44-122">Click **Save**.</span></span>

## <span data-ttu-id="a2f44-123"><a name="permissions"></a>Omezit oprávnění k ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="a2f44-123"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* <span data-ttu-id="a2f44-124">V nástroji Android Studio otevřete projekt dokončení kurzu [Začínáme s Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="a2f44-124">In Android Studio, open the project you completed with the tutorial [Get started with Mobile Apps].</span></span> <span data-ttu-id="a2f44-125">Z **spustit** nabídky, klikněte na tlačítko **spuštění aplikace**a ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2f44-125">From the **Run** menu, click **Run app**, and verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span>

     <span data-ttu-id="a2f44-126">Tato výjimka se stane, protože se aplikace pokusí o přístup k back-end jako neověřený uživatel, ale *TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="a2f44-126">This exception happens because the app attempts to access the back end as an unauthenticated user, but the *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="a2f44-127">Potom aktualizujte aplikace k ověření uživatelů před vyžádáním prostředky z back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2f44-127">Next, you update the app to authenticate users before requesting resources from the Mobile Apps back end.</span></span> 

## <a name="add-authentication-to-the-app"></a><span data-ttu-id="a2f44-128">Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="a2f44-128">Add authentication to the app</span></span>
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <span data-ttu-id="a2f44-129"><a name="cache-tokens"></a>Tokeny ověřování mezipaměti na straně klienta</span><span class="sxs-lookup"><span data-stu-id="a2f44-129"><a name="cache-tokens"></a>Cache authentication tokens on the client</span></span>
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="a2f44-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2f44-130">Next steps</span></span>
<span data-ttu-id="a2f44-131">Teď, když jste dokončili kurz základní ověřování, vezměte v úvahu pokračovat na jednu z následujících kurzů:</span><span class="sxs-lookup"><span data-stu-id="a2f44-131">Now that you completed this basic authentication tutorial, consider continuing on to one of the following tutorials:</span></span>

* <span data-ttu-id="a2f44-132">[Přidání nabízených oznámení do vaší aplikace pro Android](app-service-mobile-android-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="a2f44-132">[Add push notifications to your Android app](app-service-mobile-android-get-started-push.md).</span></span>
  <span data-ttu-id="a2f44-133">Naučte se konfigurovat Mobile Apps back-end vašeho používat Azure notification hubs k odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="a2f44-133">Learn how to configure your Mobile Apps back end to use Azure notification hubs to send push notifications.</span></span>
* <span data-ttu-id="a2f44-134">[Zapnutí offline synchronizace pro svoji aplikaci pro Android](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="a2f44-134">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="a2f44-135">Zjistěte, jak přidat podporu offline režimu do aplikace pomocí back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2f44-135">Learn how to add offline support to your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="a2f44-136">S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="a2f44-136">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
<span data-ttu-id="a2f44-137">[Začínáme s Mobile Apps]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="a2f44-137">[Get started with Mobile Apps]: app-service-mobile-android-get-started.md</span></span>
