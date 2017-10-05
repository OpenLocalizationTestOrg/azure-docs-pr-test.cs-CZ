---
title: "Jak používat modul plug-in Apache Cordova pro Azure Mobile Apps"
description: "Jak používat modul plug-in Apache Cordova pro Azure Mobile Apps"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: ebf0e911eeada0e529f908dd3e3430c94edae763
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a><span data-ttu-id="41167-103">Používání klientské knihovny pro Apache Cordova pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="41167-103">How to use Apache Cordova client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="41167-104">Tato příručka je určena můžete provádět běžné scénáře s využitím nejnovější [modul plug-in pro Apache Cordova pro Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="41167-104">This guide teaches you to perform common scenarios using the latest [Apache Cordova Plugin for Azure Mobile Apps].</span></span> <span data-ttu-id="41167-105">Pokud jste ještě Azure Mobile Apps, nejprve dokončit [Azure Mobile Apps rychlý Start] vytvořte back-end, vytvořit tabulku a stáhněte si předem připravené projekt Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="41167-105">If you are new to Azure Mobile Apps, first complete [Azure Mobile Apps Quick Start] to create a backend, create a table, and download a pre-built Apache Cordova project.</span></span> <span data-ttu-id="41167-106">V této příručce se zaměříme na modulu plug-in na straně klienta Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="41167-106">In this guide, we focus on the client-side Apache Cordova Plugin.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="41167-107">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="41167-107">Supported platforms</span></span>
<span data-ttu-id="41167-108">Tato sada SDK podporuje v6.0.0 Apache Cordova a později na iOS, Android a Windows zařízení.</span><span class="sxs-lookup"><span data-stu-id="41167-108">This SDK supports Apache Cordova v6.0.0 and later on iOS, Android, and Windows devices.</span></span>  <span data-ttu-id="41167-109">Podpora platformy je následující:</span><span class="sxs-lookup"><span data-stu-id="41167-109">The platform support is as follows:</span></span>

* <span data-ttu-id="41167-110">Rozhraní API systému Android 19 – 24 (KitKat prostřednictvím cukrovinkách typu nugát).</span><span class="sxs-lookup"><span data-stu-id="41167-110">Android API 19-24 (KitKat through Nougat).</span></span>
* <span data-ttu-id="41167-111">iOS verze 8.0 a novější.</span><span class="sxs-lookup"><span data-stu-id="41167-111">iOS versions 8.0 and later.</span></span>
* <span data-ttu-id="41167-112">Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="41167-112">Windows Phone 8.1.</span></span>
* <span data-ttu-id="41167-113">Univerzální platformy Windows.</span><span class="sxs-lookup"><span data-stu-id="41167-113">Universal Windows Platform.</span></span>

## <span data-ttu-id="41167-114"><a name="Setup"></a>Instalační program a požadavky</span><span class="sxs-lookup"><span data-stu-id="41167-114"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="41167-115">Tato příručka předpokládá, že jste vytvořili back-end s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="41167-115">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="41167-116">Tato příručka předpokládá, že tabulka má stejné schéma jako tabulky v těchto kurzech.</span><span class="sxs-lookup"><span data-stu-id="41167-116">This guide assumes that the table has the same schema as the tables in those tutorials.</span></span> <span data-ttu-id="41167-117">Tato příručka také předpokládá, že jste přidali Cordovu Apache do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="41167-117">This guide also assumes that you have added the Apache Cordova Plugin to your code.</span></span>  <span data-ttu-id="41167-118">Pokud jste tak dosud neučinili, můžete přidat modul plug-in Apache Cordova na projekt na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="41167-118">If you have not done so, you may add the Apache Cordova plugin to your project on the command line:</span></span>

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="41167-119">Další informace o vytváření [vaší první aplikace Apache Cordova], najdete v jejich dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="41167-119">For more information on creating [your first Apache Cordova app], see their documentation.</span></span>

## <span data-ttu-id="41167-120"><a name="ionic"></a>Nastavení aplikace iontových v2</span><span class="sxs-lookup"><span data-stu-id="41167-120"><a name="ionic"></a>Setting up an Ionic v2 app</span></span>

<span data-ttu-id="41167-121">O správné konfiguraci projektu iontových v2, nejprve vytvořit základní aplikaci a přidejte Cordova plugin:</span><span class="sxs-lookup"><span data-stu-id="41167-121">To properly configure an Ionic v2 project, first create a basic app and add the Cordova plugin:</span></span>

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

<span data-ttu-id="41167-122">Přidejte následující řádky do `app.component.ts` k vytvoření objektu klienta:</span><span class="sxs-lookup"><span data-stu-id="41167-122">Add the following lines to `app.component.ts` to create the client object:</span></span>

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

<span data-ttu-id="41167-123">Teď můžete sestavit a spustit projekt v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="41167-123">You can now build and run the project in the browser:</span></span>

```
ionic platform add browser
ionic run browser
```

<span data-ttu-id="41167-124">Modul plug-in Azure Mobile Apps Cordova podporuje obě iontových v1 a v2 aplikace.</span><span class="sxs-lookup"><span data-stu-id="41167-124">The Azure Mobile Apps Cordova plugin supports both Ionic v1 and v2 apps.</span></span>  <span data-ttu-id="41167-125">Jenom aplikace iontových v2 vyžadovat další deklarace pro `WindowsAzure` objektu.</span><span class="sxs-lookup"><span data-stu-id="41167-125">Only the Ionic v2 apps require the additional declaration for the `WindowsAzure` object.</span></span>

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="41167-126"><a name="auth"></a>Postupy: ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="41167-126"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="41167-127">Azure App Service podporuje ověřování a autorizaci uživatelů aplikace pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account a Twitter.</span><span class="sxs-lookup"><span data-stu-id="41167-127">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="41167-128">Můžete nastavit oprávnění pro tabulky, pokud chcete omezit přístup pro určité operace pouze ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="41167-128">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="41167-129">Můžete také použít identitu ověřeného uživatele k implementaci autorizační pravidla v skripty serveru.</span><span class="sxs-lookup"><span data-stu-id="41167-129">You can also use the identity of authenticated users to implement authorization rules in server scripts.</span></span> <span data-ttu-id="41167-130">Další informace najdete v tématu [Začínáme s ověřováním] kurzu.</span><span class="sxs-lookup"><span data-stu-id="41167-130">For more information, see the [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="41167-131">Při použití ověřování v aplikaci Apache Cordova, musí být k dispozici následující moduly plug-in Cordova:</span><span class="sxs-lookup"><span data-stu-id="41167-131">When using authentication in an Apache Cordova app, the following Cordova plugins must be available:</span></span>

* <span data-ttu-id="41167-132">[cordova-plugin-device]</span><span class="sxs-lookup"><span data-stu-id="41167-132">[cordova-plugin-device]</span></span>
* <span data-ttu-id="41167-133">[cordova-plugin-inappbrowser]</span><span class="sxs-lookup"><span data-stu-id="41167-133">[cordova-plugin-inappbrowser]</span></span>

<span data-ttu-id="41167-134">Jsou podporovány dva ověřování toky: serveru a klienta tok.</span><span class="sxs-lookup"><span data-stu-id="41167-134">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="41167-135">Vývojový server poskytuje nejjednodušší zkušeností ověřování, jako je závislé na poskytovatele webové ověřování rozhraní.</span><span class="sxs-lookup"><span data-stu-id="41167-135">The server flow provides the simplest authentication experience, as it relies on the provider's web authentication interface.</span></span> <span data-ttu-id="41167-136">Tok klienta umožňuje hlubší integrace s funkcemi konkrétní zařízení, jako-jednotné přihlášení jako přitom spoléhá na konkrétní zařízení specifické pro poskytovatele sady SDK.</span><span class="sxs-lookup"><span data-stu-id="41167-136">The client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific device-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="41167-137"><a name="configure-external-redirect-urls"></a>Postupy: Konfigurace služby Mobile App pro adresy URL pro externí přesměrování.</span><span class="sxs-lookup"><span data-stu-id="41167-137"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="41167-138">Několik typů aplikací Apache Cordova pomocí funkce zpětné smyčky pro zpracování uživatelského rozhraní OAuth toky.</span><span class="sxs-lookup"><span data-stu-id="41167-138">Several types of Apache Cordova applications use a loopback capability to handle OAuth UI flows.</span></span>  <span data-ttu-id="41167-139">Toky OAuth uživatelského rozhraní na místním hostiteli způsobit problémy, protože ověřovací služby pouze věděl, kam může využívat vaše služba ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="41167-139">OAuth UI flows on localhost cause problems since the authentication service only knows how to utilize your service by default.</span></span>  <span data-ttu-id="41167-140">Příklady problematické toky OAuth uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="41167-140">Examples of problematic OAuth UI flows include:</span></span>

* <span data-ttu-id="41167-141">Ripple emulatoru.</span><span class="sxs-lookup"><span data-stu-id="41167-141">The Ripple emulator.</span></span>
* <span data-ttu-id="41167-142">Živé opětovného načtení s Ionic.</span><span class="sxs-lookup"><span data-stu-id="41167-142">Live Reload with Ionic.</span></span>
* <span data-ttu-id="41167-143">Místní spuštění mobilního back-endu</span><span class="sxs-lookup"><span data-stu-id="41167-143">Running the mobile backend locally</span></span>
* <span data-ttu-id="41167-144">Spuštění mobilní back-end v různých App Service Azure než jeden poskytnete ověřování.</span><span class="sxs-lookup"><span data-stu-id="41167-144">Running the mobile backend in a different Azure App Service than the one providing authentication.</span></span>

<span data-ttu-id="41167-145">Postupujte podle těchto pokynů můžete přidat místní nastavení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="41167-145">Follow these instructions to add your local settings to the configuration:</span></span>

1. <span data-ttu-id="41167-146">Přihlaste se k portálu [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="41167-146">Log in to the [Azure portal]</span></span>
2. <span data-ttu-id="41167-147">Vyberte **všechny prostředky** nebo **App Services** pak klikněte na název vaší mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="41167-147">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="41167-148">Klikněte na tlačítko **nástroje**</span><span class="sxs-lookup"><span data-stu-id="41167-148">Click **Tools**</span></span>
4. <span data-ttu-id="41167-149">Klikněte na tlačítko **Průzkumníka prostředků** v nabídce dodržovat klikněte **přejděte**.</span><span class="sxs-lookup"><span data-stu-id="41167-149">Click **Resource explorer** in the OBSERVE menu, then click **Go**.</span></span>  <span data-ttu-id="41167-150">Otevře se nové okno nebo kartu.</span><span class="sxs-lookup"><span data-stu-id="41167-150">A new window or tab opens.</span></span>
5. <span data-ttu-id="41167-151">Rozbalte **konfigurace**, **authsettings** uzly pro svůj web v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="41167-151">Expand the **config**, **authsettings** nodes for your site in the left-hand navigation.</span></span>
6. <span data-ttu-id="41167-152">Klikněte na tlačítko **upravit**</span><span class="sxs-lookup"><span data-stu-id="41167-152">Click **Edit**</span></span>
7. <span data-ttu-id="41167-153">Vyhledejte element "allowedExternalRedirectUrls".</span><span class="sxs-lookup"><span data-stu-id="41167-153">Look for the "allowedExternalRedirectUrls" element.</span></span>  <span data-ttu-id="41167-154">Může být nastaven na hodnotu null nebo pole hodnot.</span><span class="sxs-lookup"><span data-stu-id="41167-154">It may be set to null or an array of values.</span></span>  <span data-ttu-id="41167-155">Změňte hodnotu na následující hodnotu:</span><span class="sxs-lookup"><span data-stu-id="41167-155">Change the value to the following value:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="41167-156">Nahraďte adresy URL adresy URL služby.</span><span class="sxs-lookup"><span data-stu-id="41167-156">Replace the URLs with the URLs of your service.</span></span>  <span data-ttu-id="41167-157">Mezi příklady patří http://localhost ": 3000" (pro službu ukázka Node.js) nebo "http://localhost:4400" (pro službu Ripple).</span><span class="sxs-lookup"><span data-stu-id="41167-157">Examples include "http://localhost:3000" (for the Node.js sample service), or "http://localhost:4400" (for the Ripple service).</span></span>  <span data-ttu-id="41167-158">Ale tyto adresy URL jsou příklady - vaší situaci, včetně služeb uvedených v příkladech se můžou lišit.</span><span class="sxs-lookup"><span data-stu-id="41167-158">However, these URLs are examples - your situation, including for the services mentioned in the examples, may be different.</span></span>
8. <span data-ttu-id="41167-159">Klikněte **pro čtení a zápis** tlačítko v pravém horním rohu obrazovky.</span><span class="sxs-lookup"><span data-stu-id="41167-159">Click the **Read/Write** button in the top-right corner of the screen.</span></span>
9. <span data-ttu-id="41167-160">Klikněte na tlačítko se zeleným **PUT** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="41167-160">Click the green **PUT** button.</span></span>

<span data-ttu-id="41167-161">Nastavení se ukládají v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="41167-161">The settings are saved at this point.</span></span>  <span data-ttu-id="41167-162">Nezavírejte okno prohlížeče, dokud nebudou dokončeny nastavení ukládání.</span><span class="sxs-lookup"><span data-stu-id="41167-162">Do not close the browser window until the settings have finished saving.</span></span>
<span data-ttu-id="41167-163">Tyto adresy URL zpětné smyčky je také možné přidáte do nastavení CORS pro App Service:</span><span class="sxs-lookup"><span data-stu-id="41167-163">Also add these loopback URLs to the CORS settings for your App Service:</span></span>

1. <span data-ttu-id="41167-164">Přihlaste se k portálu [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="41167-164">Log in to the [Azure portal]</span></span>
2. <span data-ttu-id="41167-165">Vyberte **všechny prostředky** nebo **App Services** pak klikněte na název vaší mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="41167-165">Select **All resources** or **App Services** then click the name of your Mobile App.</span></span>
3. <span data-ttu-id="41167-166">Automaticky se otevře v okně nastavení.</span><span class="sxs-lookup"><span data-stu-id="41167-166">The Settings blade opens automatically.</span></span>  <span data-ttu-id="41167-167">Pokud není, klikněte na tlačítko **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="41167-167">If it doesn't, click **All Settings**.</span></span>
4. <span data-ttu-id="41167-168">Klikněte na tlačítko **CORS** v nabídce rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="41167-168">Click **CORS** under the API menu.</span></span>
5. <span data-ttu-id="41167-169">Zadejte adresu URL, kterou chcete přidat do pole zadat a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="41167-169">Enter the URL that you wish to add in the box provided and press Enter.</span></span>
6. <span data-ttu-id="41167-170">Zadejte další adresy URL, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="41167-170">Enter additional URLs as needed.</span></span>
7. <span data-ttu-id="41167-171">Klikněte na tlačítko **Uložit** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="41167-171">Click **Save** to save the settings.</span></span>

<span data-ttu-id="41167-172">Jak dlouho trvá přibližně 10 až 15 sekund pro nová nastavení vstoupila v platnost.</span><span class="sxs-lookup"><span data-stu-id="41167-172">It takes approximately 10-15 seconds for the new settings to take effect.</span></span>

## <span data-ttu-id="41167-173"><a name="register-for-push"></a>Postupy: zaregistrovat pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="41167-173"><a name="register-for-push"></a>How to: Register for push notifications</span></span>
<span data-ttu-id="41167-174">Nainstalujte [phonegap-plugin nabízené] ke zpracování nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="41167-174">Install the [phonegap-plugin-push] to handle push notifications.</span></span>  <span data-ttu-id="41167-175">Tento modul plug-in lze snadno přidat pomocí `cordova plugin add` příkazu na příkazovém řádku nebo prostřednictvím Git instalační program modulu plug-in v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="41167-175">This plugin can be easily added using the `cordova plugin add` command on the command line, or via the Git plugin installer within Visual Studio.</span></span>  <span data-ttu-id="41167-176">Následující kód do vaší aplikace Apache Cordova registruje zařízení pro nabízená oznámení:</span><span class="sxs-lookup"><span data-stu-id="41167-176">The following code in your Apache Cordova app registers your device for push notifications:</span></span>

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

<span data-ttu-id="41167-177">Pomocí sady SDK centra oznámení k odesílání nabízených oznámení ze serveru.</span><span class="sxs-lookup"><span data-stu-id="41167-177">Use the Notification Hubs SDK to send push notifications from the server.</span></span>  <span data-ttu-id="41167-178">Nikdy odeslat nabízená oznámení přímo z klientů.</span><span class="sxs-lookup"><span data-stu-id="41167-178">Never send push notifications directly from clients.</span></span> <span data-ttu-id="41167-179">Díky tomu mohou využít ke spuštění útoku DoS proti centra oznámení nebo systém PNS.</span><span class="sxs-lookup"><span data-stu-id="41167-179">Doing so could be used to trigger a denial of service attack against Notification Hubs or the PNS.</span></span>  <span data-ttu-id="41167-180">Systém PNS může zakázat provozu v důsledku takové útoky.</span><span class="sxs-lookup"><span data-stu-id="41167-180">The PNS could ban your traffic as a result of such attacks.</span></span>

## <a name="more-information"></a><span data-ttu-id="41167-181">Další informace</span><span class="sxs-lookup"><span data-stu-id="41167-181">More information</span></span>

<span data-ttu-id="41167-182">Můžete najít podrobnosti podrobné rozhraní API v našem [dokumentaci k rozhraní API](http://azure.github.io/azure-mobile-apps-js-client/).</span><span class="sxs-lookup"><span data-stu-id="41167-182">You can find detailed API details in our [API documentation](http://azure.github.io/azure-mobile-apps-js-client/).</span></span>

<!-- URLs. -->
<span data-ttu-id="41167-183">[Azure Portal]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="41167-183">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="41167-184">[Azure Mobile Apps rychlý Start]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="41167-184">[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="41167-185">[Začínáme s ověřováním]: app-service-mobile-cordova-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="41167-185">[Get started with authentication]: app-service-mobile-cordova-get-started-users.md</span></span>
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

<span data-ttu-id="41167-186">[modul plug-in pro Apache Cordova pro Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps</span><span class="sxs-lookup"><span data-stu-id="41167-186">[Apache Cordova Plugin for Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps</span></span>
<span data-ttu-id="41167-187">[vaší první aplikace Apache Cordova]: http://cordova.apache.org/#getstarted</span><span class="sxs-lookup"><span data-stu-id="41167-187">[your first Apache Cordova app]: http://cordova.apache.org/#getstarted</span></span>
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
<span data-ttu-id="41167-188">[phonegap-plugin nabízené]: https://www.npmjs.com/package/phonegap-plugin-push</span><span class="sxs-lookup"><span data-stu-id="41167-188">[phonegap-plugin-push]: https://www.npmjs.com/package/phonegap-plugin-push</span></span>
<span data-ttu-id="41167-189">[cordova-plugin-device]: https://www.npmjs.com/package/cordova-plugin-device</span><span class="sxs-lookup"><span data-stu-id="41167-189">[cordova-plugin-device]: https://www.npmjs.com/package/cordova-plugin-device</span></span>
<span data-ttu-id="41167-190">[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser</span><span class="sxs-lookup"><span data-stu-id="41167-190">[cordova-plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser</span></span>
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
