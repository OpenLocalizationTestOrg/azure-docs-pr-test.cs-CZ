---
title: "Jak používat JavaScript SDK pro Azure Mobile Apps"
description: "Postup použití v Azure Mobile Apps"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 0c4b4de560d70592f5bbdee28b56a7686b5689f4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a><span data-ttu-id="0d841-103">Používání knihovny JavaScript klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="0d841-103">How to Use the JavaScript client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="0d841-104">Tato příručka je určena můžete provádět běžné scénáře s využitím nejnovější [JavaScript SDK pro Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="0d841-104">This guide teaches you to perform common scenarios using the latest [JavaScript SDK for Azure Mobile Apps].</span></span> <span data-ttu-id="0d841-105">Pokud jste ještě Azure Mobile Apps, nejprve dokončit [Azure Mobile Apps rychlý Start] vytvoření back-end a vytvořte tabulku.</span><span class="sxs-lookup"><span data-stu-id="0d841-105">If you are new to Azure Mobile Apps, first complete [Azure Mobile Apps Quick Start] to create a backend and create a table.</span></span> <span data-ttu-id="0d841-106">V této příručce se zaměříme na používání mobilního back-end v HTML/JavaScript webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="0d841-106">In this guide, we focus on using the mobile backend in HTML/JavaScript Web applications.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="0d841-107">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="0d841-107">Supported platforms</span></span>
<span data-ttu-id="0d841-108">Omezená podpora prohlížečů na aktuální a poslední verze hlavní prohlížeče: Google Chrome, Microsoft Edge, Microsoft Internet Explorer a Mozilla Firefox.</span><span class="sxs-lookup"><span data-stu-id="0d841-108">We limit browser support to the current and last versions of the major browsers:  Google Chrome, Microsoft Edge, Microsoft Internet Explorer, and Mozilla Firefox.</span></span>  <span data-ttu-id="0d841-109">Očekáváme, že sada SDK pro funkci v libovolného relativně moderní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0d841-109">We expect the SDK to function with any relatively modern browser.</span></span>

<span data-ttu-id="0d841-110">Balíček je distribuován jako univerzální modul JavaScript, tak, aby podporuje globals, AMD, a formátuje CommonJS.</span><span class="sxs-lookup"><span data-stu-id="0d841-110">The package is distributed as a Universal JavaScript Module, so it supports globals, AMD, and CommonJS formats.</span></span>

## <span data-ttu-id="0d841-111"><a name="Setup"></a>Instalační program a požadavky</span><span class="sxs-lookup"><span data-stu-id="0d841-111"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="0d841-112">Tato příručka předpokládá, že jste vytvořili back-end s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="0d841-112">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="0d841-113">Tato příručka předpokládá, že tabulka má stejné schéma jako tabulky v těchto kurzech.</span><span class="sxs-lookup"><span data-stu-id="0d841-113">This guide assumes that the table has the same schema as the tables in those tutorials.</span></span>

<span data-ttu-id="0d841-114">Instalace Azure Mobile Apps JavaScript SDK, můžete to udělat pomocí `npm` příkaz:</span><span class="sxs-lookup"><span data-stu-id="0d841-114">Installing the Azure Mobile Apps JavaScript SDK can be done via the `npm` command:</span></span>

```
npm install azure-mobile-apps-client --save
```

<span data-ttu-id="0d841-115">Knihovny můžete použít také jako modul ES2015 v rámci CommonJS prostředí, například Browserify a Webpack a jako knihovnu AMD.</span><span class="sxs-lookup"><span data-stu-id="0d841-115">The library can also be used as an ES2015 module, within CommonJS environments such as Browserify and Webpack and as an AMD library.</span></span>  <span data-ttu-id="0d841-116">Například:</span><span class="sxs-lookup"><span data-stu-id="0d841-116">For example:</span></span>

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

<span data-ttu-id="0d841-117">Předem připravené verzi sady SDK můžete také stáhnout přímo z našich CDN:</span><span class="sxs-lookup"><span data-stu-id="0d841-117">You can also use a pre-built version of the SDK by downloading directly from our CDN:</span></span>

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="0d841-118"><a name="auth"></a>Postupy: ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="0d841-118"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="0d841-119">Azure App Service podporuje ověřování a autorizaci uživatelů aplikace pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account a Twitter.</span><span class="sxs-lookup"><span data-stu-id="0d841-119">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="0d841-120">Můžete nastavit oprávnění pro tabulky, pokud chcete omezit přístup pro určité operace pouze ověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d841-120">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="0d841-121">Můžete také použít identitu ověřeného uživatele k implementaci autorizační pravidla v skripty serveru.</span><span class="sxs-lookup"><span data-stu-id="0d841-121">You can also use the identity of authenticated users to implement authorization rules in server scripts.</span></span> <span data-ttu-id="0d841-122">Další informace najdete v tématu [Začínáme s ověřováním] kurzu.</span><span class="sxs-lookup"><span data-stu-id="0d841-122">For more information, see the [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="0d841-123">Jsou podporovány dva ověřování toky: serveru a klienta tok.</span><span class="sxs-lookup"><span data-stu-id="0d841-123">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="0d841-124">Vývojový server poskytuje nejjednodušší zkušeností ověřování, jako je závislé na poskytovatele webové ověřování rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0d841-124">The server flow provides the simplest authentication experience, as it relies on the provider's web authentication interface.</span></span> <span data-ttu-id="0d841-125">Tok klienta umožňuje hlubší integrace s funkcemi konkrétní zařízení, jako-jednotné přihlášení jako přitom spoléhá na specifický pro zprostředkovatele sady SDK.</span><span class="sxs-lookup"><span data-stu-id="0d841-125">The client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="0d841-126"><a name="configure-external-redirect-urls"></a>Postupy: Konfigurace služby Mobile App pro adresy URL pro externí přesměrování.</span><span class="sxs-lookup"><span data-stu-id="0d841-126"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="0d841-127">Několik typů aplikací JavaScript pomocí funkce zpětné smyčky pro zpracování uživatelského rozhraní OAuth toky.</span><span class="sxs-lookup"><span data-stu-id="0d841-127">Several types of JavaScript applications use a loopback capability to handle OAuth UI flows.</span></span>  <span data-ttu-id="0d841-128">Tyto funkce patří:</span><span class="sxs-lookup"><span data-stu-id="0d841-128">These capabilities include:</span></span>

* <span data-ttu-id="0d841-129">Spuštění služby místně</span><span class="sxs-lookup"><span data-stu-id="0d841-129">Running your service locally</span></span>
* <span data-ttu-id="0d841-130">Pomocí rozhraní iontových načtěte za provozu</span><span class="sxs-lookup"><span data-stu-id="0d841-130">Using Live Reload with the Ionic Framework</span></span>
* <span data-ttu-id="0d841-131">Přesměrování na službu App Service pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="0d841-131">Redirecting to App Service for authentication.</span></span>

<span data-ttu-id="0d841-132">Spuštěn místně může způsobit problémy, protože ve výchozím nastavení, ověřování služby App Service je nakonfigurovaná pouze pro povolení přístupu z váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d841-132">Running locally can cause problems because, by default, App Service authentication is only configured to allow access from your Mobile App backend.</span></span> <span data-ttu-id="0d841-133">Chcete-li změnit nastavení služby App Service povolení ověřování při místním spuštění serveru použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d841-133">Use the following steps to change the App Service settings to enable authentication when running the server locally:</span></span>

1. <span data-ttu-id="0d841-134">Přihlaste se k portálu [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="0d841-134">Log in to the [Azure portal]</span></span>
2. <span data-ttu-id="0d841-135">Přejděte na váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d841-135">Navigate to your Mobile App backend.</span></span>
3. <span data-ttu-id="0d841-136">Vyberte **Průzkumníka prostředků** v **nástroje pro vývoj** nabídky.</span><span class="sxs-lookup"><span data-stu-id="0d841-136">Select **Resource explorer** in the **DEVELOPMENT TOOLS** menu.</span></span>
4. <span data-ttu-id="0d841-137">Klikněte na tlačítko **přejděte** otevřete Průzkumníka prostředků pro váš back-end mobilní aplikace v nové záložky nebo okno.</span><span class="sxs-lookup"><span data-stu-id="0d841-137">Click **Go** to open the resource explorer for your Mobile App backend in a new tab or window.</span></span>
5. <span data-ttu-id="0d841-138">Rozbalte **konfigurace** > **authsettings** uzel pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d841-138">Expand the **config** > **authsettings** node for your app.</span></span>
6. <span data-ttu-id="0d841-139">Klikněte **upravit** tlačítko povolte úpravy prostředku.</span><span class="sxs-lookup"><span data-stu-id="0d841-139">Click the **Edit** button to enable editing of the resource.</span></span>
7. <span data-ttu-id="0d841-140">Najít **allowedExternalRedirectUrls** element, který by měl mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="0d841-140">Find the **allowedExternalRedirectUrls** element, which should be null.</span></span> <span data-ttu-id="0d841-141">Přidejte vaší adresy URL do pole:</span><span class="sxs-lookup"><span data-stu-id="0d841-141">Add your URLs in an array:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="0d841-142">Nahraďte adresy URL v pole adresy URL služby, který v tomto příkladu je `http://localhost:3000` pro místní službu ukázka Node.js.</span><span class="sxs-lookup"><span data-stu-id="0d841-142">Replace the URLs in the array with the URLs of your service, which in this example is `http://localhost:3000` for the local Node.js sample service.</span></span> <span data-ttu-id="0d841-143">Můžete také použít `http://localhost:4400` pro službu Ripple nebo některé jiné adresy URL, v závislosti na konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d841-143">You could also use `http://localhost:4400` for the Ripple service or some other URL, depending on how your app is configured.</span></span>
8. <span data-ttu-id="0d841-144">V horní části stránky klikněte na tlačítko **pro čtení a zápis**, pak klikněte na tlačítko **PUT** uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="0d841-144">At the top of the page, click **Read/Write**, then click **PUT** to save your updates.</span></span>

<span data-ttu-id="0d841-145">Musíte taky přidat stejné adresy URL zpětné smyčky do nastavení CORS seznamu povolených IP adres:</span><span class="sxs-lookup"><span data-stu-id="0d841-145">You also need to add the same loopback URLs to the CORS whitelist settings:</span></span>

1. <span data-ttu-id="0d841-146">Přejděte zpět [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="0d841-146">Navigate back to the [Azure portal].</span></span>
2. <span data-ttu-id="0d841-147">Přejděte na váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d841-147">Navigate to your Mobile App backend.</span></span>
3. <span data-ttu-id="0d841-148">Klikněte na tlačítko **CORS** v **rozhraní API** nabídky.</span><span class="sxs-lookup"><span data-stu-id="0d841-148">Click **CORS** in the **API** menu.</span></span>
4. <span data-ttu-id="0d841-149">Zadejte jednotlivé adresy URL v prázdném **povolené zdroje** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0d841-149">Enter each URL in the empty **Allowed Origins** text box.</span></span>  <span data-ttu-id="0d841-150">Vytvoří se nové textové pole.</span><span class="sxs-lookup"><span data-stu-id="0d841-150">A new text box is created.</span></span>
5. <span data-ttu-id="0d841-151">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="0d841-151">Click **SAVE**</span></span>

<span data-ttu-id="0d841-152">Po aktualizaci back-end, bude moci použít nové adresy URL zpětné smyčky ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d841-152">After the backend updates, you will be able to use the new loopback URLs in your app.</span></span>

<!-- URLs. -->
<span data-ttu-id="0d841-153">[Azure Mobile Apps rychlý Start]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="0d841-153">[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="0d841-154">[Začínáme s ověřováním]: app-service-mobile-cordova-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="0d841-154">[Get started with authentication]: app-service-mobile-cordova-get-started-users.md</span></span>
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

<span data-ttu-id="0d841-155">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="0d841-155">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="0d841-156">[JavaScript SDK pro Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client</span><span class="sxs-lookup"><span data-stu-id="0d841-156">[JavaScript SDK for Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client</span></span>
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
