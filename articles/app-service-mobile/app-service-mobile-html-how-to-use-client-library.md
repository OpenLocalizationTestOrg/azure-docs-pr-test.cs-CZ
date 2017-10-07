---
title: aaaHow tooUse hello JavaScript SDK pro Azure Mobile Apps
description: Jak v tooUse pro Azure Mobile Apps
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
ms.openlocfilehash: 3fcbb0c5bd6918a285bdafa1946ba0bd47bb21b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-javascript-client-library-for-azure-mobile-apps"></a><span data-ttu-id="2a1a0-103">Jak tooUse hello knihovny JavaScript klienta pro Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="2a1a0-103">How tooUse hello JavaScript client library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="2a1a0-104">Tato příručka je určena tooperform běžné scénáře s využitím hello nejnovější [JavaScript SDK pro Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="2a1a0-104">This guide teaches you tooperform common scenarios using hello latest [JavaScript SDK for Azure Mobile Apps].</span></span> <span data-ttu-id="2a1a0-105">Pokud jste nový tooAzure Mobile Apps, nejprve dokončit [Azure Mobile Apps rychlý Start] toocreate back-end a vytvořte tabulku.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-105">If you are new tooAzure Mobile Apps, first complete [Azure Mobile Apps Quick Start] toocreate a backend and create a table.</span></span> <span data-ttu-id="2a1a0-106">V této příručce se zaměříme na používání mobilního back-endu hello v HTML/JavaScript webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-106">In this guide, we focus on using hello mobile backend in HTML/JavaScript Web applications.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="2a1a0-107">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="2a1a0-107">Supported platforms</span></span>
<span data-ttu-id="2a1a0-108">Jsme omezit aktuální toohello podpora prohlížeče a poslední verze hello hlavní prohlížeče: Google Chrome, Microsoft Edge, Microsoft Internet Explorer a Mozilla Firefox.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-108">We limit browser support toohello current and last versions of hello major browsers:  Google Chrome, Microsoft Edge, Microsoft Internet Explorer, and Mozilla Firefox.</span></span>  <span data-ttu-id="2a1a0-109">Očekáváme, že toofunction SDK hello pomocí libovolného relativně moderní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-109">We expect hello SDK toofunction with any relatively modern browser.</span></span>

<span data-ttu-id="2a1a0-110">Hello balíček je distribuován jako univerzální modul JavaScript, tak, aby podporuje globals, AMD, a formátuje CommonJS.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-110">hello package is distributed as a Universal JavaScript Module, so it supports globals, AMD, and CommonJS formats.</span></span>

## <span data-ttu-id="2a1a0-111"><a name="Setup"></a>Instalační program a požadavky</span><span class="sxs-lookup"><span data-stu-id="2a1a0-111"><a name="Setup"></a>Setup and prerequisites</span></span>
<span data-ttu-id="2a1a0-112">Tato příručka předpokládá, že jste vytvořili back-end s tabulkou.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-112">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="2a1a0-113">Tato příručka předpokládá, že tabulka hello má hello stejného schématu jako hello tabulky v těchto kurzech.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-113">This guide assumes that hello table has hello same schema as hello tables in those tutorials.</span></span>

<span data-ttu-id="2a1a0-114">Instalace hello Azure Mobile Apps JavaScript SDK, můžete to udělat pomocí hello `npm` příkaz:</span><span class="sxs-lookup"><span data-stu-id="2a1a0-114">Installing hello Azure Mobile Apps JavaScript SDK can be done via hello `npm` command:</span></span>

```
npm install azure-mobile-apps-client --save
```

<span data-ttu-id="2a1a0-115">Hello knihovně mohou sloužit také jako modul ES2015 v rámci CommonJS prostředí, například Browserify a Webpack a jako AMD knihovny.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-115">hello library can also be used as an ES2015 module, within CommonJS environments such as Browserify and Webpack and as an AMD library.</span></span>  <span data-ttu-id="2a1a0-116">Například:</span><span class="sxs-lookup"><span data-stu-id="2a1a0-116">For example:</span></span>

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

<span data-ttu-id="2a1a0-117">Můžete také předem připravené verzi hello SDK stáhnout přímo z našich CDN:</span><span class="sxs-lookup"><span data-stu-id="2a1a0-117">You can also use a pre-built version of hello SDK by downloading directly from our CDN:</span></span>

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <span data-ttu-id="2a1a0-118"><a name="auth"></a>Postupy: ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="2a1a0-118"><a name="auth"></a>How to: Authenticate users</span></span>
<span data-ttu-id="2a1a0-119">Azure App Service podporuje ověřování a autorizaci uživatelů aplikace pomocí různých zprostředkovatelů externí identity: Facebook, Google, Microsoft Account a Twitter.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-119">Azure App Service supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, and Twitter.</span></span> <span data-ttu-id="2a1a0-120">Oprávnění můžete nastavit na tabulky toorestrict přístup pro určité operace tooonly ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-120">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="2a1a0-121">Můžete taky hello identity ověřené uživatele tooimplement autorizačních pravidel ve skriptech serveru.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-121">You can also use hello identity of authenticated users tooimplement authorization rules in server scripts.</span></span> <span data-ttu-id="2a1a0-122">Další informace najdete v tématu hello [Začínáme s ověřováním] kurzu.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-122">For more information, see hello [Get started with authentication] tutorial.</span></span>

<span data-ttu-id="2a1a0-123">Jsou podporovány dva ověřování toky: serveru a klienta tok.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-123">Two authentication flows are supported: a server flow and a client flow.</span></span>  <span data-ttu-id="2a1a0-124">Vývojový server Hello poskytuje hello nejjednodušší ověřování, jako je závislé na hello zprostředkovatele webového ověření rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-124">hello server flow provides hello simplest authentication experience, as it relies on hello provider's web authentication interface.</span></span> <span data-ttu-id="2a1a0-125">Hello tok klienta umožňuje hlubší integrace s funkcemi konkrétní zařízení, jako-jednotné přihlášení jako přitom spoléhá na specifický pro zprostředkovatele sady SDK.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-125">hello client flow allows for deeper integration with device-specific capabilities such as single-sign-on as it relies on provider-specific SDKs.</span></span>

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <span data-ttu-id="2a1a0-126"><a name="configure-external-redirect-urls"></a>Postupy: Konfigurace služby Mobile App pro adresy URL pro externí přesměrování.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-126"><a name="configure-external-redirect-urls"></a>How to: Configure your Mobile App Service for external redirect URLs.</span></span>
<span data-ttu-id="2a1a0-127">Několik typů aplikací JavaScript pomocí funkce toohandle zpětné smyčky, kterou toků OAuth uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-127">Several types of JavaScript applications use a loopback capability toohandle OAuth UI flows.</span></span>  <span data-ttu-id="2a1a0-128">Tyto funkce patří:</span><span class="sxs-lookup"><span data-stu-id="2a1a0-128">These capabilities include:</span></span>

* <span data-ttu-id="2a1a0-129">Spuštění služby místně</span><span class="sxs-lookup"><span data-stu-id="2a1a0-129">Running your service locally</span></span>
* <span data-ttu-id="2a1a0-130">Načtěte za provozu pomocí hello iontových Framework</span><span class="sxs-lookup"><span data-stu-id="2a1a0-130">Using Live Reload with hello Ionic Framework</span></span>
* <span data-ttu-id="2a1a0-131">Přesměrování tooApp služby pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-131">Redirecting tooApp Service for authentication.</span></span>

<span data-ttu-id="2a1a0-132">Spuštěn místně může způsobit problémy, protože ve výchozím nastavení, aplikační služby ověřování je jenom nakonfigurován tooallow přístup z váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-132">Running locally can cause problems because, by default, App Service authentication is only configured tooallow access from your Mobile App backend.</span></span> <span data-ttu-id="2a1a0-133">Použijte následující postup toochange hello ověřování tooenable nastavení služby App Service při místním spuštění hello server hello:</span><span class="sxs-lookup"><span data-stu-id="2a1a0-133">Use hello following steps toochange hello App Service settings tooenable authentication when running hello server locally:</span></span>

1. <span data-ttu-id="2a1a0-134">Přihlaste se toohello [portálu Azure]</span><span class="sxs-lookup"><span data-stu-id="2a1a0-134">Log in toohello [Azure portal]</span></span>
2. <span data-ttu-id="2a1a0-135">Přejděte back-end mobilní aplikace tooyour.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-135">Navigate tooyour Mobile App backend.</span></span>
3. <span data-ttu-id="2a1a0-136">Vyberte **Průzkumníka prostředků** v hello **nástroje pro vývoj** nabídky.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-136">Select **Resource explorer** in hello **DEVELOPMENT TOOLS** menu.</span></span>
4. <span data-ttu-id="2a1a0-137">Klikněte na tlačítko **přejděte** tooopen hello Průzkumníka prostředků pro váš back-end mobilní aplikace v nové záložky nebo okno.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-137">Click **Go** tooopen hello resource explorer for your Mobile App backend in a new tab or window.</span></span>
5. <span data-ttu-id="2a1a0-138">Rozbalte hello **konfigurace** > **authsettings** uzel pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-138">Expand hello **config** > **authsettings** node for your app.</span></span>
6. <span data-ttu-id="2a1a0-139">Klikněte na tlačítko hello **upravit** tlačítko tooenable úpravy hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-139">Click hello **Edit** button tooenable editing of hello resource.</span></span>
7. <span data-ttu-id="2a1a0-140">Najde hello **allowedExternalRedirectUrls** element, který by měl mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-140">Find hello **allowedExternalRedirectUrls** element, which should be null.</span></span> <span data-ttu-id="2a1a0-141">Přidejte vaší adresy URL do pole:</span><span class="sxs-lookup"><span data-stu-id="2a1a0-141">Add your URLs in an array:</span></span>

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    <span data-ttu-id="2a1a0-142">Nahraďte hello adresy URL v poli hello hello adresy URL služby, který v tomto příkladu je `http://localhost:3000` hello místní Node.js Ukázka služby.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-142">Replace hello URLs in hello array with hello URLs of your service, which in this example is `http://localhost:3000` for hello local Node.js sample service.</span></span> <span data-ttu-id="2a1a0-143">Můžete také použít `http://localhost:4400` pro hello Ripple služby nebo některých jiných adresu URL, v závislosti na konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-143">You could also use `http://localhost:4400` for hello Ripple service or some other URL, depending on how your app is configured.</span></span>
8. <span data-ttu-id="2a1a0-144">V horní části hello hello stránky, klikněte na tlačítko **pro čtení a zápis**, pak klikněte na tlačítko **PUT** toosave vaše aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-144">At hello top of hello page, click **Read/Write**, then click **PUT** toosave your updates.</span></span>

<span data-ttu-id="2a1a0-145">Musíte taky tooadd hello stejné zpětné smyčky adresy URL toohello CORS povolených nastavení:</span><span class="sxs-lookup"><span data-stu-id="2a1a0-145">You also need tooadd hello same loopback URLs toohello CORS whitelist settings:</span></span>

1. <span data-ttu-id="2a1a0-146">Přejděte zpět toohello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="2a1a0-146">Navigate back toohello [Azure portal].</span></span>
2. <span data-ttu-id="2a1a0-147">Přejděte back-end mobilní aplikace tooyour.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-147">Navigate tooyour Mobile App backend.</span></span>
3. <span data-ttu-id="2a1a0-148">Klikněte na tlačítko **CORS** v hello **rozhraní API** nabídky.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-148">Click **CORS** in hello **API** menu.</span></span>
4. <span data-ttu-id="2a1a0-149">Zadejte jednotlivé adresy URL v hello prázdný **povolené zdroje** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-149">Enter each URL in hello empty **Allowed Origins** text box.</span></span>  <span data-ttu-id="2a1a0-150">Vytvoří se nové textové pole.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-150">A new text box is created.</span></span>
5. <span data-ttu-id="2a1a0-151">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="2a1a0-151">Click **SAVE**</span></span>

<span data-ttu-id="2a1a0-152">Po aktualizaci hello back-end, bude možné toouse hello nové zpětné smyčky adresy URL ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a1a0-152">After hello backend updates, you will be able toouse hello new loopback URLs in your app.</span></span>

<!-- URLs. -->
[Azure Mobile Apps rychlý Start]: app-service-mobile-cordova-get-started.md
[Začínáme s ověřováním]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[portálu Azure]: https://portal.azure.com/
[JavaScript SDK pro Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
