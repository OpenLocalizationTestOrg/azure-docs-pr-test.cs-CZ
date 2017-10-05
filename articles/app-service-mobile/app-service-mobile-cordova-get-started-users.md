---
title: "Přidání ověřování na Apache Cordova s Mobile Apps | Microsoft Docs"
description: "Naučte se používat Mobile Apps v Azure App Service ověřovat uživatele vaší aplikace Apache Cordova prostřednictvím řady různých zprostředkovatelů identity, včetně Google, Facebook, Twitter a společnosti Microsoft."
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
ms.openlocfilehash: b7362b7f26859de541f792e714502851d74c98e5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-apache-cordova-app"></a><span data-ttu-id="a26f6-103">Přidání ověřování do vaší aplikace Apache Cordova</span><span class="sxs-lookup"><span data-stu-id="a26f6-103">Add authentication to your Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="a26f6-104">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a26f6-104">Summary</span></span>
<span data-ttu-id="a26f6-105">V tomto kurzu přidání ověřování do seznamu úkolů projektu pro rychlý start na Apache Cordova pomocí zprostředkovatele identity podporované.</span><span class="sxs-lookup"><span data-stu-id="a26f6-105">In this tutorial, you add authentication to the todolist quickstart project on Apache Cordova using a supported identity provider.</span></span> <span data-ttu-id="a26f6-106">V tomto kurzu vychází z [Začínáme s Mobile Apps] kurz, který je třeba nejprve provést.</span><span class="sxs-lookup"><span data-stu-id="a26f6-106">This tutorial is based on the [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="a26f6-107"><a name="register"></a>Registrace aplikace pro ověřování a nakonfigurovat App Service</span><span class="sxs-lookup"><span data-stu-id="a26f6-107"><a name="register"></a>Register your app for authentication and configure the App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[<span data-ttu-id="a26f6-108">Podívejte se na video zobrazující podobný postup.</span><span class="sxs-lookup"><span data-stu-id="a26f6-108">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <span data-ttu-id="a26f6-109"><a name="permissions"></a>Omezit oprávnění k ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="a26f6-109"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="a26f6-110">Nyní můžete ověřit, že byl zakázán anonymní přístup k vaší back-end.</span><span class="sxs-lookup"><span data-stu-id="a26f6-110">Now, you can verify that anonymous access to your backend has been disabled.</span></span> <span data-ttu-id="a26f6-111">V sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a26f6-111">In Visual Studio:</span></span>

* <span data-ttu-id="a26f6-112">Otevřete projekt, který jste vytvořili, když jste dokončili kurz [Začínáme s Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="a26f6-112">Open the project that you created when you completed the tutorial [Get started with Mobile Apps].</span></span>
* <span data-ttu-id="a26f6-113">Spusťte aplikaci **emulátor Google Android**.</span><span class="sxs-lookup"><span data-stu-id="a26f6-113">Run your application in the **Google Android Emulator**.</span></span>
* <span data-ttu-id="a26f6-114">Ověřte, že k neočekávané chybě připojení se zobrazí po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a26f6-114">Verify that an Unexpected Connection Failure is shown after the app starts.</span></span>

<span data-ttu-id="a26f6-115">Potom aktualizujte aplikace k ověření uživatelů před vyžádáním prostředky z back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="a26f6-115">Next, update the app to authenticate users before requesting resources from the Mobile App backend.</span></span>

## <span data-ttu-id="a26f6-116"><a name="add-authentication"></a>Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="a26f6-116"><a name="add-authentication"></a>Add authentication to the app</span></span>
1. <span data-ttu-id="a26f6-117">Otevřete projekt v **Visual Studio**, pak otevřete `www/index.html` soubor pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="a26f6-117">Open your project in **Visual Studio**, then open the `www/index.html` file for editing.</span></span>
2. <span data-ttu-id="a26f6-118">Vyhledejte `Content-Security-Policy` značka meta v části head.</span><span class="sxs-lookup"><span data-stu-id="a26f6-118">Locate the `Content-Security-Policy` meta tag in the head section.</span></span>  <span data-ttu-id="a26f6-119">Přidejte hostitele OAuth do seznamu povolených zdrojů.</span><span class="sxs-lookup"><span data-stu-id="a26f6-119">Add the OAuth host to the list of allowed sources.</span></span>

   | <span data-ttu-id="a26f6-120">Poskytovatel</span><span class="sxs-lookup"><span data-stu-id="a26f6-120">Provider</span></span> | <span data-ttu-id="a26f6-121">Název poskytovatele sady SDK</span><span class="sxs-lookup"><span data-stu-id="a26f6-121">SDK Provider Name</span></span> | <span data-ttu-id="a26f6-122">OAuth hostitele</span><span class="sxs-lookup"><span data-stu-id="a26f6-122">OAuth Host</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="a26f6-123">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a26f6-123">Azure Active Directory</span></span> | <span data-ttu-id="a26f6-124">aad</span><span class="sxs-lookup"><span data-stu-id="a26f6-124">aad</span></span> | <span data-ttu-id="a26f6-125">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a26f6-125">https://login.microsoftonline.com</span></span> |
   | <span data-ttu-id="a26f6-126">Facebook</span><span class="sxs-lookup"><span data-stu-id="a26f6-126">Facebook</span></span> | <span data-ttu-id="a26f6-127">Facebook</span><span class="sxs-lookup"><span data-stu-id="a26f6-127">facebook</span></span> | <span data-ttu-id="a26f6-128">https://www.Facebook.com</span><span class="sxs-lookup"><span data-stu-id="a26f6-128">https://www.facebook.com</span></span> |
   | <span data-ttu-id="a26f6-129">Google</span><span class="sxs-lookup"><span data-stu-id="a26f6-129">Google</span></span> | <span data-ttu-id="a26f6-130">Google</span><span class="sxs-lookup"><span data-stu-id="a26f6-130">google</span></span> | <span data-ttu-id="a26f6-131">https://accounts.Google.com</span><span class="sxs-lookup"><span data-stu-id="a26f6-131">https://accounts.google.com</span></span> |
   | <span data-ttu-id="a26f6-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="a26f6-132">Microsoft</span></span> | <span data-ttu-id="a26f6-133">MicrosoftAccount</span><span class="sxs-lookup"><span data-stu-id="a26f6-133">microsoftaccount</span></span> | <span data-ttu-id="a26f6-134">https://Login.live.com</span><span class="sxs-lookup"><span data-stu-id="a26f6-134">https://login.live.com</span></span> |
   | <span data-ttu-id="a26f6-135">Twitter</span><span class="sxs-lookup"><span data-stu-id="a26f6-135">Twitter</span></span> | <span data-ttu-id="a26f6-136">Služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="a26f6-136">twitter</span></span> | <span data-ttu-id="a26f6-137">https://API.Twitter.com</span><span class="sxs-lookup"><span data-stu-id="a26f6-137">https://api.twitter.com</span></span> |

    <span data-ttu-id="a26f6-138">Příklad obsah-Security-Policy (implementována pro Azure Active Directory) je následující:</span><span class="sxs-lookup"><span data-stu-id="a26f6-138">An example Content-Security-Policy (implemented for Azure Active Directory) is as follows:</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    <span data-ttu-id="a26f6-139">Nahraďte `https://login.microsoftonline.com` s hostitelem OAuth z výše uvedené tabulce.</span><span class="sxs-lookup"><span data-stu-id="a26f6-139">Replace `https://login.microsoftonline.com` with the OAuth host from the preceding table.</span></span>  <span data-ttu-id="a26f6-140">Další informace o metaznačku zásady zabezpečení obsahu najdete v tématu [zásady zabezpečení obsahu dokumentaci].</span><span class="sxs-lookup"><span data-stu-id="a26f6-140">For more information about the content-security-policy meta tag, see the [Content-Security-Policy documentation].</span></span>

    <span data-ttu-id="a26f6-141">Někteří poskytovatelé ověřování nevyžadují změny zásad zabezpečení obsahu při použití na příslušné mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="a26f6-141">Some authentication providers do not require Content-Security-Policy changes when used on appropriate mobile devices.</span></span>  <span data-ttu-id="a26f6-142">Například žádné změny zásad zabezpečení obsahu jsou požadovaná při použití ověřování Google na zařízení s Androidem.</span><span class="sxs-lookup"><span data-stu-id="a26f6-142">For example, no Content-Security-Policy changes are required when using Google authentication on an Android device.</span></span>

3. <span data-ttu-id="a26f6-143">Otevřete `www/js/index.js` souboru pro úpravy, vyhledejte `onDeviceReady()` metoda, a v části Vytvoření klienta kód přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="a26f6-143">Open the `www/js/index.js` file for editing, locate the `onDeviceReady()` method, and under the client  creation code add the following code:</span></span>

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    <span data-ttu-id="a26f6-144">Tento kód nahradí stávající kód, který vytvoří odkaz na tabulku a aktualizuje uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a26f6-144">This code replaces the existing code that creates the table reference and refreshes the UI.</span></span>

    <span data-ttu-id="a26f6-145">Metoda login() začíná zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="a26f6-145">The login() method starts authentication with the provider.</span></span> <span data-ttu-id="a26f6-146">Metoda login() je asynchronní funkce vracející JavaScript Promise.</span><span class="sxs-lookup"><span data-stu-id="a26f6-146">The login() method is an async function that returns a JavaScript Promise.</span></span>  <span data-ttu-id="a26f6-147">Zbytek inicializace je umístěn uvnitř promise odpověď, a tedy není spuštěn až do dokončení metodu login().</span><span class="sxs-lookup"><span data-stu-id="a26f6-147">The rest of the initialization is placed inside the promise response so that it is not executed until the login() method completes.</span></span>

4. <span data-ttu-id="a26f6-148">V kódu, který jste právě přidali, nahraďte `SDK_Provider_Name` s názvem vaší zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a26f6-148">In the code that you just added, replace `SDK_Provider_Name` with the name of your login provider.</span></span> <span data-ttu-id="a26f6-149">Například pro Azure Active Directory, použijte `client.login('aad')`.</span><span class="sxs-lookup"><span data-stu-id="a26f6-149">For example, for Azure Active Directory, use `client.login('aad')`.</span></span>
5. <span data-ttu-id="a26f6-150">Spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="a26f6-150">Run your project.</span></span>  <span data-ttu-id="a26f6-151">Po dokončení inicializace projekt aplikace zobrazuje stránku pro přihlášení OAuth zprostředkovatele zvolené ověřování.</span><span class="sxs-lookup"><span data-stu-id="a26f6-151">When the project has finished initializing, your application shows the OAuth login page for the chosen authentication provider.</span></span>

## <span data-ttu-id="a26f6-152"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="a26f6-152"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="a26f6-153">Další informace [o ověřování] službou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a26f6-153">Learn more [About Authentication] with Azure App Service.</span></span>
* <span data-ttu-id="a26f6-154">Pokračovat v kurzu přidáním [nabízená oznámení] do vaší aplikace Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="a26f6-154">Continue the tutorial by adding [Push Notifications] to your Apache Cordova app.</span></span>

<span data-ttu-id="a26f6-155">Zjistěte, jak používat sady SDK.</span><span class="sxs-lookup"><span data-stu-id="a26f6-155">Learn how to use the SDKs.</span></span>

* <span data-ttu-id="a26f6-156">[Apache Cordova SDK]</span><span class="sxs-lookup"><span data-stu-id="a26f6-156">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="a26f6-157">[ASP.NET Server SDK]</span><span class="sxs-lookup"><span data-stu-id="a26f6-157">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="a26f6-158">[Node.js Server SDK]</span><span class="sxs-lookup"><span data-stu-id="a26f6-158">[Node.js Server SDK]</span></span>

<!-- URLs. -->
<span data-ttu-id="a26f6-159">[Začínáme s Mobile Apps]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="a26f6-159">[Get started with Mobile Apps]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="a26f6-160">[zásady zabezpečení obsahu dokumentaci]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html</span><span class="sxs-lookup"><span data-stu-id="a26f6-160">[Content-Security-Policy documentation]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html</span></span>
<span data-ttu-id="a26f6-161">[nabízená oznámení]: app-service-mobile-cordova-get-started-push.md</span><span class="sxs-lookup"><span data-stu-id="a26f6-161">[Push Notifications]: app-service-mobile-cordova-get-started-push.md</span></span>
<span data-ttu-id="a26f6-162">[o ověřování]: app-service-mobile-auth.md</span><span class="sxs-lookup"><span data-stu-id="a26f6-162">[About Authentication]: app-service-mobile-auth.md</span></span>
<span data-ttu-id="a26f6-163">[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md</span><span class="sxs-lookup"><span data-stu-id="a26f6-163">[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md</span></span>
<span data-ttu-id="a26f6-164">[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md</span><span class="sxs-lookup"><span data-stu-id="a26f6-164">[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md</span></span>
<span data-ttu-id="a26f6-165">[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md</span><span class="sxs-lookup"><span data-stu-id="a26f6-165">[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md</span></span>
