---
title: "ověřování aaaAdd na Apache Cordova s Mobile Apps | Microsoft Docs"
description: "Zjistěte, jak toouse Mobile Apps v Azure App Service tooauthenticate uživatele vaší aplikace Apache Cordova prostřednictvím řady různých zprostředkovatelů identity, včetně Google, Facebook, Twitter a společnosti Microsoft."
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 61a05c5ac67fd0f0bc4c9d6920954a9b464a0a8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-apache-cordova-app"></a><span data-ttu-id="aa642-103">Přidat aplikaci Apache Cordova tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="aa642-103">Add authentication tooyour Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="aa642-104">Souhrn</span><span class="sxs-lookup"><span data-stu-id="aa642-104">Summary</span></span>
<span data-ttu-id="aa642-105">V tomto kurzu přidáte projekt rychlého startu todolist toohello ověřování na Apache Cordova pomocí zprostředkovatele identity podporované.</span><span class="sxs-lookup"><span data-stu-id="aa642-105">In this tutorial, you add authentication toohello todolist quickstart project on Apache Cordova using a supported identity provider.</span></span> <span data-ttu-id="aa642-106">V tomto kurzu vychází z hello [Začínáme s Mobile Apps] kurz, který je třeba nejprve provést.</span><span class="sxs-lookup"><span data-stu-id="aa642-106">This tutorial is based on hello [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="aa642-107"><a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte hello služby App Service</span><span class="sxs-lookup"><span data-stu-id="aa642-107"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[<span data-ttu-id="aa642-108">Podívejte se na video zobrazující podobný postup.</span><span class="sxs-lookup"><span data-stu-id="aa642-108">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <span data-ttu-id="aa642-109"><a name="permissions"></a>Omezte oprávnění tooauthenticated</span><span class="sxs-lookup"><span data-stu-id="aa642-109"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="aa642-110">Nyní můžete ověřit, že back-end tooyour anonymní přístup je zakázané.</span><span class="sxs-lookup"><span data-stu-id="aa642-110">Now, you can verify that anonymous access tooyour backend has been disabled.</span></span> <span data-ttu-id="aa642-111">V sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="aa642-111">In Visual Studio:</span></span>

* <span data-ttu-id="aa642-112">Otevřete hello projekt, který jste vytvořili, když jste dokončili kurz hello [Začínáme s Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="aa642-112">Open hello project that you created when you completed hello tutorial [Get started with Mobile Apps].</span></span>
* <span data-ttu-id="aa642-113">Spusťte aplikaci v hello **emulátor Google Android**.</span><span class="sxs-lookup"><span data-stu-id="aa642-113">Run your application in hello **Google Android Emulator**.</span></span>
* <span data-ttu-id="aa642-114">Ověřte, zda neočekávaného selhání připojení je zobrazen po spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="aa642-114">Verify that an Unexpected Connection Failure is shown after hello app starts.</span></span>

<span data-ttu-id="aa642-115">Potom aktualizujte uživatele tooauthenticate aplikace hello před vyžádáním prostředky z back-end mobilní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="aa642-115">Next, update hello app tooauthenticate users before requesting resources from hello Mobile App backend.</span></span>

## <span data-ttu-id="aa642-116"><a name="add-authentication"></a>Přidat aplikaci toohello ověřování</span><span class="sxs-lookup"><span data-stu-id="aa642-116"><a name="add-authentication"></a>Add authentication toohello app</span></span>
1. <span data-ttu-id="aa642-117">Otevřete projekt v **Visual Studio**, pak otevřete hello `www/index.html` soubor pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="aa642-117">Open your project in **Visual Studio**, then open hello `www/index.html` file for editing.</span></span>
2. <span data-ttu-id="aa642-118">Vyhledejte hello `Content-Security-Policy` značka meta v části head hello.</span><span class="sxs-lookup"><span data-stu-id="aa642-118">Locate hello `Content-Security-Policy` meta tag in hello head section.</span></span>  <span data-ttu-id="aa642-119">Přidáte hostitele hello OAuth toohello seznamu povolených zdrojů.</span><span class="sxs-lookup"><span data-stu-id="aa642-119">Add hello OAuth host toohello list of allowed sources.</span></span>

   | <span data-ttu-id="aa642-120">Poskytovatel</span><span class="sxs-lookup"><span data-stu-id="aa642-120">Provider</span></span> | <span data-ttu-id="aa642-121">Název poskytovatele sady SDK</span><span class="sxs-lookup"><span data-stu-id="aa642-121">SDK Provider Name</span></span> | <span data-ttu-id="aa642-122">OAuth hostitele</span><span class="sxs-lookup"><span data-stu-id="aa642-122">OAuth Host</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="aa642-123">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aa642-123">Azure Active Directory</span></span> | <span data-ttu-id="aa642-124">aad</span><span class="sxs-lookup"><span data-stu-id="aa642-124">aad</span></span> | <span data-ttu-id="aa642-125">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="aa642-125">https://login.microsoftonline.com</span></span> |
   | <span data-ttu-id="aa642-126">Facebook</span><span class="sxs-lookup"><span data-stu-id="aa642-126">Facebook</span></span> | <span data-ttu-id="aa642-127">Facebook</span><span class="sxs-lookup"><span data-stu-id="aa642-127">facebook</span></span> | <span data-ttu-id="aa642-128">https://www.Facebook.com</span><span class="sxs-lookup"><span data-stu-id="aa642-128">https://www.facebook.com</span></span> |
   | <span data-ttu-id="aa642-129">Google</span><span class="sxs-lookup"><span data-stu-id="aa642-129">Google</span></span> | <span data-ttu-id="aa642-130">Google</span><span class="sxs-lookup"><span data-stu-id="aa642-130">google</span></span> | <span data-ttu-id="aa642-131">https://accounts.Google.com</span><span class="sxs-lookup"><span data-stu-id="aa642-131">https://accounts.google.com</span></span> |
   | <span data-ttu-id="aa642-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="aa642-132">Microsoft</span></span> | <span data-ttu-id="aa642-133">MicrosoftAccount</span><span class="sxs-lookup"><span data-stu-id="aa642-133">microsoftaccount</span></span> | <span data-ttu-id="aa642-134">https://Login.live.com</span><span class="sxs-lookup"><span data-stu-id="aa642-134">https://login.live.com</span></span> |
   | <span data-ttu-id="aa642-135">Twitter</span><span class="sxs-lookup"><span data-stu-id="aa642-135">Twitter</span></span> | <span data-ttu-id="aa642-136">Služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="aa642-136">twitter</span></span> | <span data-ttu-id="aa642-137">https://API.Twitter.com</span><span class="sxs-lookup"><span data-stu-id="aa642-137">https://api.twitter.com</span></span> |

    <span data-ttu-id="aa642-138">Příklad obsah-Security-Policy (implementována pro Azure Active Directory) je následující:</span><span class="sxs-lookup"><span data-stu-id="aa642-138">An example Content-Security-Policy (implemented for Azure Active Directory) is as follows:</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    <span data-ttu-id="aa642-139">Nahraďte `https://login.microsoftonline.com` s hostitelem OAuth hello z hello předcházející tabulce.</span><span class="sxs-lookup"><span data-stu-id="aa642-139">Replace `https://login.microsoftonline.com` with hello OAuth host from hello preceding table.</span></span>  <span data-ttu-id="aa642-140">Další informace o značka meta hello zásady zabezpečení obsahu najdete v tématu hello [zásady zabezpečení obsahu dokumentaci].</span><span class="sxs-lookup"><span data-stu-id="aa642-140">For more information about hello content-security-policy meta tag, see hello [Content-Security-Policy documentation].</span></span>

    <span data-ttu-id="aa642-141">Někteří poskytovatelé ověřování nevyžadují změny zásad zabezpečení obsahu při použití na příslušné mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="aa642-141">Some authentication providers do not require Content-Security-Policy changes when used on appropriate mobile devices.</span></span>  <span data-ttu-id="aa642-142">Například žádné změny zásad zabezpečení obsahu jsou požadovaná při použití ověřování Google na zařízení s Androidem.</span><span class="sxs-lookup"><span data-stu-id="aa642-142">For example, no Content-Security-Policy changes are required when using Google authentication on an Android device.</span></span>

3. <span data-ttu-id="aa642-143">Otevřete hello `www/js/index.js` souboru pro úpravy, vyhledejte hello `onDeviceReady()` metoda, a v části Vytvoření klienta hello kód přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="aa642-143">Open hello `www/js/index.js` file for editing, locate hello `onDeviceReady()` method, and under hello client  creation code add hello following code:</span></span>

        // Login toohello service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    <span data-ttu-id="aa642-144">Tento kód nahradí hello existující kód, který vytvoří odkaz na tabulku hello a aktualizuje hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="aa642-144">This code replaces hello existing code that creates hello table reference and refreshes hello UI.</span></span>

    <span data-ttu-id="aa642-145">Metoda login() Hello začíná hello zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="aa642-145">hello login() method starts authentication with hello provider.</span></span> <span data-ttu-id="aa642-146">Metoda login() Hello je asynchronní funkce vracející JavaScript Promise.</span><span class="sxs-lookup"><span data-stu-id="aa642-146">hello login() method is an async function that returns a JavaScript Promise.</span></span>  <span data-ttu-id="aa642-147">Hello zbytek inicializace hello je umístěn uvnitř hello promise odpovědi tak, aby nebude provedena, dokud se nedokončí Metoda login() hello.</span><span class="sxs-lookup"><span data-stu-id="aa642-147">hello rest of hello initialization is placed inside hello promise response so that it is not executed until hello login() method completes.</span></span>

4. <span data-ttu-id="aa642-148">V hello kód, který jste právě přidali, nahraďte `SDK_Provider_Name` s názvem hello zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="aa642-148">In hello code that you just added, replace `SDK_Provider_Name` with hello name of your login provider.</span></span> <span data-ttu-id="aa642-149">Například pro Azure Active Directory, použijte `client.login('aad')`.</span><span class="sxs-lookup"><span data-stu-id="aa642-149">For example, for Azure Active Directory, use `client.login('aad')`.</span></span>
5. <span data-ttu-id="aa642-150">Spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="aa642-150">Run your project.</span></span>  <span data-ttu-id="aa642-151">Po dokončení inicializace hello projektu aplikace zobrazuje hello OAuth přihlašovací stránku pro hello vybrali zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="aa642-151">When hello project has finished initializing, your application shows hello OAuth login page for hello chosen authentication provider.</span></span>

## <span data-ttu-id="aa642-152"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa642-152"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="aa642-153">Další informace [o ověřování] službou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="aa642-153">Learn more [About Authentication] with Azure App Service.</span></span>
* <span data-ttu-id="aa642-154">Pokračovat hello kurzu přidáním [nabízená oznámení] tooyour aplikace Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="aa642-154">Continue hello tutorial by adding [Push Notifications] tooyour Apache Cordova app.</span></span>

<span data-ttu-id="aa642-155">Zjistěte, jak toouse hello sady SDK.</span><span class="sxs-lookup"><span data-stu-id="aa642-155">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="aa642-156">[Apache Cordova SDK]</span><span class="sxs-lookup"><span data-stu-id="aa642-156">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="aa642-157">[ASP.NET Server SDK]</span><span class="sxs-lookup"><span data-stu-id="aa642-157">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="aa642-158">[Node.js Server SDK]</span><span class="sxs-lookup"><span data-stu-id="aa642-158">[Node.js Server SDK]</span></span>

<!-- URLs. -->
[Začínáme s Mobile Apps]: app-service-mobile-cordova-get-started.md
[zásady zabezpečení obsahu dokumentaci]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[nabízená oznámení]: app-service-mobile-cordova-get-started-push.md
[o ověřování]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
