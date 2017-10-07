---
title: aaaAzure integrace sady Mobile Engagement Web SDK | Microsoft Docs
description: "Hello nejnovější aktualizace a postupy pro hello sada SDK webové služby Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
ms.openlocfilehash: 99613b68b615bec4ddcfcc8e4e0133ce9d887bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a><span data-ttu-id="28777-103">Integrovat Azure Mobile Engagement ve webové aplikaci</span><span class="sxs-lookup"><span data-stu-id="28777-103">Integrate Azure Mobile Engagement in a web application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="28777-104">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="28777-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="28777-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="28777-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="28777-106">iOS</span><span class="sxs-lookup"><span data-stu-id="28777-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="28777-107">Android</span><span class="sxs-lookup"><span data-stu-id="28777-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="28777-108">Hello postupy v tomto článku popisují hello nejjednodušší způsob, jak tooactivate hello analýzy a monitorování funkcí v Azure Mobile Engagement ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28777-108">hello procedures in this article describe hello simplest way tooactivate hello analytics and monitoring functions in Azure Mobile Engagement in your web application.</span></span>

<span data-ttu-id="28777-109">Postupujte podle hello kroky tooactivate hello protokolu sestavy, které jsou potřebné toocompute všechny statistické údaje o uživatelích, relacích, aktivity, dojde k chybě a technicals.</span><span class="sxs-lookup"><span data-stu-id="28777-109">Follow hello steps tooactivate hello log reports that are needed toocompute all statistics about users, sessions, activities, crashes, and technicals.</span></span> <span data-ttu-id="28777-110">Statistics závislé aplikace, jako jsou události a chyby, úlohy je nutné aktivovat sestavy protokolu ručně pomocí hello rozhraní API služby Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="28777-110">For application-dependent statistics, such as events, errors, and jobs, you must activate log reports manually by using hello Azure Mobile Engagement API.</span></span> <span data-ttu-id="28777-111">Další informace najdete další [jak toouse hello advanced označování rozhraní API ve webové aplikaci Mobile Engagement](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="28777-111">For more information, learn [how toouse hello advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="28777-112">Úvod</span><span class="sxs-lookup"><span data-stu-id="28777-112">Introduction</span></span>
<span data-ttu-id="28777-113">[Stáhnout hello sada SDK webové služby Azure Mobile Engagement](http://aka.ms/P7b453).</span><span class="sxs-lookup"><span data-stu-id="28777-113">[Download hello Azure Mobile Engagement Web SDK](http://aka.ms/P7b453).</span></span>
<span data-ttu-id="28777-114">Hello Mobile Engagement Web SDK je dodávána jako jeden soubor JavaScript, azure-engagement.js, které jste tooinclude v každé stránce webu či webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="28777-114">hello Mobile Engagement Web SDK is shipped as a single JavaScript file, azure-engagement.js, which you have tooinclude in each page of your site or web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="28777-115">Před spuštěním tohoto skriptu, musíte spustit skript nebo kód fragment kódu, který zápisu tooconfigure Mobile Engagementu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="28777-115">Before you run this script, you must run a script or code snippet that you write tooconfigure Mobile Engagement for your application.</span></span>
> 
> 

## <a name="browser-compatibility"></a><span data-ttu-id="28777-116">Kompatibilita s prohlížeči</span><span class="sxs-lookup"><span data-stu-id="28777-116">Browser compatibility</span></span>
<span data-ttu-id="28777-117">Hello Mobile Engagement Web SDK používá nativní JSON kódování a dekódování kromě požadavky AJAX toocross domény (přijímající na specifikaci W3C CORS hello).</span><span class="sxs-lookup"><span data-stu-id="28777-117">hello Mobile Engagement Web SDK uses native JSON encoding and decoding, in addition toocross-domain AJAX requests (relying on hello W3C CORS specification).</span></span> <span data-ttu-id="28777-118">Je kompatibilní s hello následujících prohlížečů:</span><span class="sxs-lookup"><span data-stu-id="28777-118">It's compatible with hello following browsers:</span></span>

* <span data-ttu-id="28777-119">Microsoft Edge 12 +</span><span class="sxs-lookup"><span data-stu-id="28777-119">Microsoft Edge 12+</span></span>
* <span data-ttu-id="28777-120">Internet Explorer 10 +</span><span class="sxs-lookup"><span data-stu-id="28777-120">Internet Explorer 10+</span></span>
* <span data-ttu-id="28777-121">Firefox 3.5 +</span><span class="sxs-lookup"><span data-stu-id="28777-121">Firefox 3.5+</span></span>
* <span data-ttu-id="28777-122">Chrome 4 +</span><span class="sxs-lookup"><span data-stu-id="28777-122">Chrome 4+</span></span>
* <span data-ttu-id="28777-123">Safari 6 +</span><span class="sxs-lookup"><span data-stu-id="28777-123">Safari 6+</span></span>
* <span data-ttu-id="28777-124">Opera 12 +</span><span class="sxs-lookup"><span data-stu-id="28777-124">Opera 12+</span></span>

## <a name="configure-mobile-engagement"></a><span data-ttu-id="28777-125">Konfigurace Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="28777-125">Configure Mobile Engagement</span></span>
<span data-ttu-id="28777-126">Napsat skript, který vytvoří globální konfiguraci `azureEngagement` objekt jazyka JavaScript, stejně jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="28777-126">Write a script that creates a global `azureEngagement` JavaScript object, as in hello following example.</span></span> <span data-ttu-id="28777-127">Vzhledem k tomu, že váš web může mít násobky stránky, tento příklad předpokládá, že tento skript je zahrnuta v každé stránce.</span><span class="sxs-lookup"><span data-stu-id="28777-127">Because your site might have multiples pages, this example assumes that this script is included in every page.</span></span> <span data-ttu-id="28777-128">V tomto příkladu je název objektu JavaScript hello `azure-engagement-conf.js`.</span><span class="sxs-lookup"><span data-stu-id="28777-128">In this example, hello JavaScript object is named `azure-engagement-conf.js`.</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

<span data-ttu-id="28777-129">Hello `connectionString` hodnotu pro vaše aplikace je zobrazena v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="28777-129">hello `connectionString` value for your application is displayed in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="28777-130">`appVersionName`a `appVersionCode` jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="28777-130">`appVersionName` and `appVersionCode` are optional.</span></span> <span data-ttu-id="28777-131">Doporučujeme však, že jste je nakonfigurovat tak, aby analytics může zpracovat informace o verzi.</span><span class="sxs-lookup"><span data-stu-id="28777-131">However, we recommend that you configure them so that analytics can process version information.</span></span>
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a><span data-ttu-id="28777-132">Zahrnout Mobile Engagement skripty na svých stránkách</span><span class="sxs-lookup"><span data-stu-id="28777-132">Include Mobile Engagement scripts in your pages</span></span>
<span data-ttu-id="28777-133">Přidání Mobile Engagement skripty tooyour stránky v jednom z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="28777-133">Add Mobile Engagement scripts tooyour pages in one of hello following ways:</span></span>

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

<span data-ttu-id="28777-134">Nebo to:</span><span class="sxs-lookup"><span data-stu-id="28777-134">Or this:</span></span>

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a><span data-ttu-id="28777-135">Alias</span><span class="sxs-lookup"><span data-stu-id="28777-135">Alias</span></span>
<span data-ttu-id="28777-136">Po načtení hello Mobile Engagement Web SDK skript vytvoří hello **engagement** alias tooaccess hello rozhraní API sady SDK.</span><span class="sxs-lookup"><span data-stu-id="28777-136">After hello Mobile Engagement Web SDK script is loaded, it creates hello **engagement** alias tooaccess hello SDK APIs.</span></span> <span data-ttu-id="28777-137">Konfigurace sady SDK hello toodefine tento alias nelze použít.</span><span class="sxs-lookup"><span data-stu-id="28777-137">You cannot use this alias toodefine hello SDK configuration.</span></span> <span data-ttu-id="28777-138">Tento alias se používá jako odkaz v této dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="28777-138">This alias is used as a reference in this documentation.</span></span>

<span data-ttu-id="28777-139">Všimněte si, že pokud alias výchozí hello je v konfliktu s jinou – globální proměnná ze stránky, můžete upravit ho v hello konfigurace takto před načtením hello Mobile Engagement Web SDK:</span><span class="sxs-lookup"><span data-stu-id="28777-139">Note that if hello default alias conflicts with another global variable from your page, you can redefine it in hello configuration as follows before you load hello Mobile Engagement Web SDK:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a><span data-ttu-id="28777-140">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="28777-140">Basic reporting</span></span>
<span data-ttu-id="28777-141">Základní vytváření sestav v Mobile Engagementu popisuje statistické údaje, relace úrovni, třeba o uživatelích, relacích, aktivity a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="28777-141">Basic reporting in Mobile Engagement covers session-level statistics, such as statistics about users, sessions, activities, and crashes.</span></span>

### <a name="session-tracking"></a><span data-ttu-id="28777-142">Sledování relace</span><span class="sxs-lookup"><span data-stu-id="28777-142">Session tracking</span></span>
<span data-ttu-id="28777-143">Mobile Engagement relace je rozdělené do posloupnost aktivit, identifikována názvem.</span><span class="sxs-lookup"><span data-stu-id="28777-143">A Mobile Engagement session is divided into a sequence of activities, each identified by a name.</span></span>

<span data-ttu-id="28777-144">V klasického webu doporučujeme je deklarovat jiné aktivitě na každé stránce vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="28777-144">In a classic website, we recommend that you declare a different activity on each page of your site.</span></span> <span data-ttu-id="28777-145">Web nebo webovou aplikaci, ve které hello aktuální stránku nikdy změní můžete tootrack hello aktivity v menším měřítku, jako třeba v rámci stránky hello.</span><span class="sxs-lookup"><span data-stu-id="28777-145">For a website or web application in which hello current page never changes, you might want tootrack hello activities on a smaller scale, such as within hello page.</span></span>

<span data-ttu-id="28777-146">Buď způsobem, toostart nebo změňte hello aktuální aktivity uživatelů, volání hello `engagement.agent.startActivity` funkce.</span><span class="sxs-lookup"><span data-stu-id="28777-146">Either way, toostart or change hello current user activity, call hello `engagement.agent.startActivity` function.</span></span> <span data-ttu-id="28777-147">Například:</span><span class="sxs-lookup"><span data-stu-id="28777-147">For example:</span></span>

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

<span data-ttu-id="28777-148">Mobile Engagement server Hello automaticky ukončí otevřít relaci do tří minut po zavření stránky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="28777-148">hello Mobile Engagement server automatically ends an open session within three minutes after hello application page is closed.</span></span>

<span data-ttu-id="28777-149">Alternativně můžete ukončit relaci ručně voláním `engagement.agent.endActivity`.</span><span class="sxs-lookup"><span data-stu-id="28777-149">Alternatively, you can end a session manually by calling `engagement.agent.endActivity`.</span></span> <span data-ttu-id="28777-150">Toto nastaví hello aktuální uživatel aktivity too'Idle. "</span><span class="sxs-lookup"><span data-stu-id="28777-150">This sets hello current user activity too'Idle.'</span></span>  <span data-ttu-id="28777-151">Hello relace bude ukončena 10 sekund později, pokud novou volání příliš`engagement.agent.startActivity` obnoví hello relace.</span><span class="sxs-lookup"><span data-stu-id="28777-151">hello session will end 10 seconds later unless a new call too`engagement.agent.startActivity` resumes hello session.</span></span>

<span data-ttu-id="28777-152">Prodlevu hello 10 do objektu hello globální zapojení, můžete nakonfigurovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="28777-152">You can configure hello 10-second delay in hello global engagement object, as follows:</span></span>

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end hello session as soon as endActivity is called

> [!NOTE]
> <span data-ttu-id="28777-153">Nemůžete použít `engagement.agent.endActivity` v hello `onunload` zpětného volání vzhledem k tomu, že nemůžete provádět volání AJAX v této fázi.</span><span class="sxs-lookup"><span data-stu-id="28777-153">You cannot use `engagement.agent.endActivity` in hello `onunload` callback because you cannot make AJAX calls at this stage.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="28777-154">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="28777-154">Advanced reporting</span></span>
<span data-ttu-id="28777-155">Volitelně Pokud chcete události specifické pro aplikaci tooreport, chyb a úlohy, je nutné toouse hello Mobile Engagement API.</span><span class="sxs-lookup"><span data-stu-id="28777-155">Optionally, if you want tooreport application-specific events, errors, and jobs, you need toouse hello Mobile Engagement API.</span></span> <span data-ttu-id="28777-156">Přístup k hello Mobile Engagement API prostřednictvím hello `engagement.agent` objektu.</span><span class="sxs-lookup"><span data-stu-id="28777-156">You access hello Mobile Engagement API through hello `engagement.agent` object.</span></span>

<span data-ttu-id="28777-157">Přístup ke všem hello rozšířené možnosti v Mobile Engagementu v hello Mobile Engagement API.</span><span class="sxs-lookup"><span data-stu-id="28777-157">You can access all of hello advanced capabilities in Mobile Engagement in hello Mobile Engagement API.</span></span> <span data-ttu-id="28777-158">Hello rozhraní API je podrobně popsaná v článku hello [jak toouse hello advanced označování rozhraní API ve webové aplikaci Mobile Engagement](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="28777-158">hello API is detailed in hello article [How toouse hello advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="customize-hello-urls-used-for-ajax-calls"></a><span data-ttu-id="28777-159">Přizpůsobení adresy URL hello použít pro volání AJAX</span><span class="sxs-lookup"><span data-stu-id="28777-159">Customize hello URLs used for AJAX calls</span></span>
<span data-ttu-id="28777-160">Adresy URL můžete přizpůsobit této hello, jaký používá SDK webové služby Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="28777-160">You can customize URLs that hello Mobile Engagement Web SDK uses.</span></span> <span data-ttu-id="28777-161">Například tooredefine hello protokolu adresu URL (hello SDK koncový bod pro protokolování), můžete přepsat konfiguraci hello podobné výjimky:</span><span class="sxs-lookup"><span data-stu-id="28777-161">For example, tooredefine hello log URL (hello SDK endpoint for logging), you can override hello configuration like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

<span data-ttu-id="28777-162">Pokud adresa URL funkce vrátí řetězec, který začíná `/`, `//`, `http://`, nebo `https://`, výchozí schéma hello nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="28777-162">If your URL functions return a string that begins with `/`, `//`, `http://`, or `https://`, hello default scheme is not used.</span></span> <span data-ttu-id="28777-163">Ve výchozím nastavení, hello `https://` schéma se používá pro tyto adresy URL.</span><span class="sxs-lookup"><span data-stu-id="28777-163">By default, hello `https://` scheme is used for those URLs.</span></span> <span data-ttu-id="28777-164">Pokud chcete toocustomize hello výchozí schéma, přepište hello konfigurace, například takto:</span><span class="sxs-lookup"><span data-stu-id="28777-164">If you want toocustomize hello default scheme, override hello configuration, like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
