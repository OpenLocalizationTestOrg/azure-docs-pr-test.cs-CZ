---
title: "Integrace se službou Azure Mobile Engagement Web SDK | Microsoft Docs"
description: "Nejnovější aktualizace a postupy pro Web Azure Mobile Engagement SDK"
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
ms.openlocfilehash: 7d8eaa180e277741a583522ee62d68f5247b92bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a><span data-ttu-id="20d78-103">Integrovat Azure Mobile Engagement ve webové aplikaci</span><span class="sxs-lookup"><span data-stu-id="20d78-103">Integrate Azure Mobile Engagement in a web application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="20d78-104">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="20d78-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="20d78-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="20d78-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="20d78-106">iOS</span><span class="sxs-lookup"><span data-stu-id="20d78-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="20d78-107">Android</span><span class="sxs-lookup"><span data-stu-id="20d78-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="20d78-108">V tomto článku jsou popsány nejjednodušší způsob, jak aktivovat analýzy a monitorování funkcí v Azure Mobile Engagement ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="20d78-108">The procedures in this article describe the simplest way to activate the analytics and monitoring functions in Azure Mobile Engagement in your web application.</span></span>

<span data-ttu-id="20d78-109">Postupujte podle kroků k aktivaci protokolu sestavy, které jsou potřebné k výpočtu všechny statistické údaje o uživatelích, relacích, aktivity, dojde k chybě a technicals.</span><span class="sxs-lookup"><span data-stu-id="20d78-109">Follow the steps to activate the log reports that are needed to compute all statistics about users, sessions, activities, crashes, and technicals.</span></span> <span data-ttu-id="20d78-110">Statistics závislé aplikace, jako jsou události a chyby, úlohy je nutné aktivovat sestavy protokolu ručně pomocí rozhraní API služby Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="20d78-110">For application-dependent statistics, such as events, errors, and jobs, you must activate log reports manually by using the Azure Mobile Engagement API.</span></span> <span data-ttu-id="20d78-111">Další informace najdete další [jak používat rozšířené Mobile Engagement označování rozhraní API ve webové aplikaci](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="20d78-111">For more information, learn [how to use the advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="20d78-112">Úvod</span><span class="sxs-lookup"><span data-stu-id="20d78-112">Introduction</span></span>
<span data-ttu-id="20d78-113">[Stažení na webu Azure Mobile Engagement SDK](http://aka.ms/P7b453).</span><span class="sxs-lookup"><span data-stu-id="20d78-113">[Download the Azure Mobile Engagement Web SDK](http://aka.ms/P7b453).</span></span>
<span data-ttu-id="20d78-114">Mobile Engagement Web SDK je dodávána jako jeden soubor JavaScript, azure-engagement.js, které je nutné zahrnout v každé stránce webu či webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="20d78-114">The Mobile Engagement Web SDK is shipped as a single JavaScript file, azure-engagement.js, which you have to include in each page of your site or web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="20d78-115">Před spuštěním tohoto skriptu, je nutné spustit fragment skriptu nebo kód, který zapisovat konfigurace Mobile Engagementu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="20d78-115">Before you run this script, you must run a script or code snippet that you write to configure Mobile Engagement for your application.</span></span>
> 
> 

## <a name="browser-compatibility"></a><span data-ttu-id="20d78-116">Kompatibilita s prohlížeči</span><span class="sxs-lookup"><span data-stu-id="20d78-116">Browser compatibility</span></span>
<span data-ttu-id="20d78-117">Mobile Engagement Web SDK používá nativní JSON kódování a dekódování kromě požadavky AJAX mezi doménami (spoléhat na specifikace W3C CORS).</span><span class="sxs-lookup"><span data-stu-id="20d78-117">The Mobile Engagement Web SDK uses native JSON encoding and decoding, in addition to cross-domain AJAX requests (relying on the W3C CORS specification).</span></span> <span data-ttu-id="20d78-118">Je kompatibilní s následujících prohlížečů:</span><span class="sxs-lookup"><span data-stu-id="20d78-118">It's compatible with the following browsers:</span></span>

* <span data-ttu-id="20d78-119">Microsoft Edge 12 +</span><span class="sxs-lookup"><span data-stu-id="20d78-119">Microsoft Edge 12+</span></span>
* <span data-ttu-id="20d78-120">Internet Explorer 10 +</span><span class="sxs-lookup"><span data-stu-id="20d78-120">Internet Explorer 10+</span></span>
* <span data-ttu-id="20d78-121">Firefox 3.5 +</span><span class="sxs-lookup"><span data-stu-id="20d78-121">Firefox 3.5+</span></span>
* <span data-ttu-id="20d78-122">Chrome 4 +</span><span class="sxs-lookup"><span data-stu-id="20d78-122">Chrome 4+</span></span>
* <span data-ttu-id="20d78-123">Safari 6 +</span><span class="sxs-lookup"><span data-stu-id="20d78-123">Safari 6+</span></span>
* <span data-ttu-id="20d78-124">Opera 12 +</span><span class="sxs-lookup"><span data-stu-id="20d78-124">Opera 12+</span></span>

## <a name="configure-mobile-engagement"></a><span data-ttu-id="20d78-125">Konfigurace Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="20d78-125">Configure Mobile Engagement</span></span>
<span data-ttu-id="20d78-126">Napsat skript, který vytvoří globální konfiguraci `azureEngagement` objekt jazyka JavaScript, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="20d78-126">Write a script that creates a global `azureEngagement` JavaScript object, as in the following example.</span></span> <span data-ttu-id="20d78-127">Vzhledem k tomu, že váš web může mít násobky stránky, tento příklad předpokládá, že tento skript je zahrnuta v každé stránce.</span><span class="sxs-lookup"><span data-stu-id="20d78-127">Because your site might have multiples pages, this example assumes that this script is included in every page.</span></span> <span data-ttu-id="20d78-128">V tomto příkladu je objekt jazyka JavaScript s názvem `azure-engagement-conf.js`.</span><span class="sxs-lookup"><span data-stu-id="20d78-128">In this example, the JavaScript object is named `azure-engagement-conf.js`.</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

<span data-ttu-id="20d78-129">`connectionString` Hodnotu pro vaše aplikace se zobrazí na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="20d78-129">The `connectionString` value for your application is displayed in the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="20d78-130">`appVersionName`a `appVersionCode` jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="20d78-130">`appVersionName` and `appVersionCode` are optional.</span></span> <span data-ttu-id="20d78-131">Doporučujeme však, že jste je nakonfigurovat tak, aby analytics může zpracovat informace o verzi.</span><span class="sxs-lookup"><span data-stu-id="20d78-131">However, we recommend that you configure them so that analytics can process version information.</span></span>
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a><span data-ttu-id="20d78-132">Zahrnout Mobile Engagement skripty na svých stránkách</span><span class="sxs-lookup"><span data-stu-id="20d78-132">Include Mobile Engagement scripts in your pages</span></span>
<span data-ttu-id="20d78-133">Přidáte skripty Mobile Engagement na stránky v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="20d78-133">Add Mobile Engagement scripts to your pages in one of the following ways:</span></span>

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

<span data-ttu-id="20d78-134">Nebo to:</span><span class="sxs-lookup"><span data-stu-id="20d78-134">Or this:</span></span>

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a><span data-ttu-id="20d78-135">Alias</span><span class="sxs-lookup"><span data-stu-id="20d78-135">Alias</span></span>
<span data-ttu-id="20d78-136">Po načtení skriptu SDK webové služby Mobile Engagement, vytvoří **engagement** alias pro přístup k rozhraní API sady SDK.</span><span class="sxs-lookup"><span data-stu-id="20d78-136">After the Mobile Engagement Web SDK script is loaded, it creates the **engagement** alias to access the SDK APIs.</span></span> <span data-ttu-id="20d78-137">Tento alias nelze použít k definování konfigurace sady SDK.</span><span class="sxs-lookup"><span data-stu-id="20d78-137">You cannot use this alias to define the SDK configuration.</span></span> <span data-ttu-id="20d78-138">Tento alias se používá jako odkaz v této dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="20d78-138">This alias is used as a reference in this documentation.</span></span>

<span data-ttu-id="20d78-139">Všimněte si, že pokud výchozí alias je v konfliktu s jinou – globální proměnná ze stránky, můžete upravit ji v konfiguraci takto před načtením Mobile Engagement SDK webové:</span><span class="sxs-lookup"><span data-stu-id="20d78-139">Note that if the default alias conflicts with another global variable from your page, you can redefine it in the configuration as follows before you load the Mobile Engagement Web SDK:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a><span data-ttu-id="20d78-140">Vytváření základních sestav</span><span class="sxs-lookup"><span data-stu-id="20d78-140">Basic reporting</span></span>
<span data-ttu-id="20d78-141">Základní vytváření sestav v Mobile Engagementu popisuje statistické údaje, relace úrovni, třeba o uživatelích, relacích, aktivity a dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="20d78-141">Basic reporting in Mobile Engagement covers session-level statistics, such as statistics about users, sessions, activities, and crashes.</span></span>

### <a name="session-tracking"></a><span data-ttu-id="20d78-142">Sledování relace</span><span class="sxs-lookup"><span data-stu-id="20d78-142">Session tracking</span></span>
<span data-ttu-id="20d78-143">Mobile Engagement relace je rozdělené do posloupnost aktivit, identifikována názvem.</span><span class="sxs-lookup"><span data-stu-id="20d78-143">A Mobile Engagement session is divided into a sequence of activities, each identified by a name.</span></span>

<span data-ttu-id="20d78-144">V klasického webu doporučujeme je deklarovat jiné aktivitě na každé stránce vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="20d78-144">In a classic website, we recommend that you declare a different activity on each page of your site.</span></span> <span data-ttu-id="20d78-145">Pro web nebo webovou aplikaci, ve kterém se aktuální stránku nikdy změní můžete chtít sledovat aktivity v menším měřítku, jako třeba v rámci dané stránky.</span><span class="sxs-lookup"><span data-stu-id="20d78-145">For a website or web application in which the current page never changes, you might want to track the activities on a smaller scale, such as within the page.</span></span>

<span data-ttu-id="20d78-146">V obou případech spustit nebo změnit aktuální aktivity uživatelů, volání `engagement.agent.startActivity` funkce.</span><span class="sxs-lookup"><span data-stu-id="20d78-146">Either way, to start or change the current user activity, call the `engagement.agent.startActivity` function.</span></span> <span data-ttu-id="20d78-147">Například:</span><span class="sxs-lookup"><span data-stu-id="20d78-147">For example:</span></span>

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

<span data-ttu-id="20d78-148">Mobile Engagement server automaticky ukončí otevřít relaci do tří minut po zavření stránky aplikace.</span><span class="sxs-lookup"><span data-stu-id="20d78-148">The Mobile Engagement server automatically ends an open session within three minutes after the application page is closed.</span></span>

<span data-ttu-id="20d78-149">Alternativně můžete ukončit relaci ručně voláním `engagement.agent.endActivity`.</span><span class="sxs-lookup"><span data-stu-id="20d78-149">Alternatively, you can end a session manually by calling `engagement.agent.endActivity`.</span></span> <span data-ttu-id="20d78-150">Toto nastaví aktuální aktivity uživatelů na "Nečinnosti."</span><span class="sxs-lookup"><span data-stu-id="20d78-150">This sets the current user activity to 'Idle.'</span></span>  <span data-ttu-id="20d78-151">Relace se ukončí 10 sekund později, pokud nové volání na `engagement.agent.startActivity` obnoví relace.</span><span class="sxs-lookup"><span data-stu-id="20d78-151">The session will end 10 seconds later unless a new call to `engagement.agent.startActivity` resumes the session.</span></span>

<span data-ttu-id="20d78-152">Prodlevu 10 sekund do objektu globální zapojení, můžete nakonfigurovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="20d78-152">You can configure the 10-second delay in the global engagement object, as follows:</span></span>

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [!NOTE]
> <span data-ttu-id="20d78-153">Nemůžete použít `engagement.agent.endActivity` v `onunload` zpětného volání vzhledem k tomu, že nemůžete provádět volání AJAX v této fázi.</span><span class="sxs-lookup"><span data-stu-id="20d78-153">You cannot use `engagement.agent.endActivity` in the `onunload` callback because you cannot make AJAX calls at this stage.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="20d78-154">Rozšířená tvorba sestav</span><span class="sxs-lookup"><span data-stu-id="20d78-154">Advanced reporting</span></span>
<span data-ttu-id="20d78-155">Případně pokud chcete ohlásit události specifické pro aplikace, chyb a úlohy, budete muset použít rozhraní API služby Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="20d78-155">Optionally, if you want to report application-specific events, errors, and jobs, you need to use the Mobile Engagement API.</span></span> <span data-ttu-id="20d78-156">Získat přístup k rozhraní API Mobile Engagement prostřednictvím `engagement.agent` objektu.</span><span class="sxs-lookup"><span data-stu-id="20d78-156">You access the Mobile Engagement API through the `engagement.agent` object.</span></span>

<span data-ttu-id="20d78-157">Přístup ke všem rozšířené možnosti v Mobile Engagementu v rozhraní API Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="20d78-157">You can access all of the advanced capabilities in Mobile Engagement in the Mobile Engagement API.</span></span> <span data-ttu-id="20d78-158">Rozhraní API je podrobně popsaná v článku [jak používat rozšířené Mobile Engagement označování rozhraní API ve webové aplikaci](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="20d78-158">The API is detailed in the article [How to use the advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="customize-the-urls-used-for-ajax-calls"></a><span data-ttu-id="20d78-159">Přizpůsobení adresy URL použít pro volání AJAX</span><span class="sxs-lookup"><span data-stu-id="20d78-159">Customize the URLs used for AJAX calls</span></span>
<span data-ttu-id="20d78-160">Můžete přizpůsobit adresy URL, které používá Mobile Engagement Web SDK.</span><span class="sxs-lookup"><span data-stu-id="20d78-160">You can customize URLs that the Mobile Engagement Web SDK uses.</span></span> <span data-ttu-id="20d78-161">Například se znovu definovat adresu URL protokolu (SDK koncový bod pro protokolování), může potlačit konfiguraci takto:</span><span class="sxs-lookup"><span data-stu-id="20d78-161">For example, to redefine the log URL (the SDK endpoint for logging), you can override the configuration like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

<span data-ttu-id="20d78-162">Pokud adresa URL funkce vrátí řetězec, který začíná `/`, `//`, `http://`, nebo `https://`, výchozí schéma se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="20d78-162">If your URL functions return a string that begins with `/`, `//`, `http://`, or `https://`, the default scheme is not used.</span></span> <span data-ttu-id="20d78-163">Ve výchozím nastavení `https://` schéma se používá pro tyto adresy URL.</span><span class="sxs-lookup"><span data-stu-id="20d78-163">By default, the `https://` scheme is used for those URLs.</span></span> <span data-ttu-id="20d78-164">Pokud chcete přizpůsobit výchozí schéma, přepište konfiguraci, například takto:</span><span class="sxs-lookup"><span data-stu-id="20d78-164">If you want to customize the default scheme, override the configuration, like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
