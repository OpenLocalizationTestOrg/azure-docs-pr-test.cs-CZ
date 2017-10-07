---
title: "aaaAzure Mobile Engagement Průvodce odstraňováním potíží - SDK"
description: "Řešení potíží s problémů s integrací sady SDK v Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: de265cf1-2f88-43ef-8616-156ada5be7b6
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1c082b81d898f4bdb47b8efe6cfbacfd83fe9279
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a><span data-ttu-id="4ee71-103">Průvodci odstraňováním potíží problémů s integrací sady SDK</span><span class="sxs-lookup"><span data-stu-id="4ee71-103">Troubleshooting guide for SDK integration issues</span></span>
<span data-ttu-id="4ee71-104">Hello následují možných problémů se můžete setkat s jak Azure Mobile Engagement integruje do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ee71-104">hello following are possible issues you may encounter with how Azure Mobile Engagement integrates into your application.</span></span>

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a><span data-ttu-id="4ee71-105">SDK problémů zjištěných selhání v jiné oblasti vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ee71-105">SDK issues discovered by a failure in another area of your application</span></span>
### <a name="issue"></a><span data-ttu-id="4ee71-106">Problém</span><span class="sxs-lookup"><span data-stu-id="4ee71-106">Issue</span></span>
* <span data-ttu-id="4ee71-107">Chyba kolekce dat uživatelského rozhraní (v analýzy, monitorování, segmentace nebo řídicí panely).</span><span class="sxs-lookup"><span data-stu-id="4ee71-107">UI data collection failure (in Analytics, Monitoring, Segmentation, or Dashboards).</span></span>
* <span data-ttu-id="4ee71-108">Push selhání (nabízených oznámení nefungují v aplikaci a mimo aplikaci, nebo obojí).</span><span class="sxs-lookup"><span data-stu-id="4ee71-108">Push Failures (Pushes don't work in app, out of app, or both).</span></span>
* <span data-ttu-id="4ee71-109">Pokročilé funkce selhání (sledování, informace o zeměpisné poloze nebo platformy, které nepodporují konkrétní nabízených oznámení).</span><span class="sxs-lookup"><span data-stu-id="4ee71-109">Advanced Feature Failures (Tracking, Geolocation, or platform specific Pushes don’t work).</span></span>
* <span data-ttu-id="4ee71-110">Selhání rozhraní API (rozhraní API selhání často bezobslužném režimu bez chybové zprávy).</span><span class="sxs-lookup"><span data-stu-id="4ee71-110">API Failures (APIs fail often silently without error messages).</span></span>
* <span data-ttu-id="4ee71-111">Selhání služby (žádná z Azure Mobile Engagement funguje pro vaši aplikaci).</span><span class="sxs-lookup"><span data-stu-id="4ee71-111">Service Failures (none of Azure Mobile Engagement works for your application).</span></span>

### <a name="causes"></a><span data-ttu-id="4ee71-112">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="4ee71-112">Causes</span></span>
* <span data-ttu-id="4ee71-113">Většiny problémů, které je třeba toobe vyřešil hello Azure Mobile Engagement SDK budou zjištěny chybou v aplikaci (například selhání shromažďování dat uživatelského rozhraní, selhání nabízená, chybu upřesňující funkce, selhání rozhraní API, havárie aplikací nebo zřejmá služby výpadek).</span><span class="sxs-lookup"><span data-stu-id="4ee71-113">Most issues that need toobe resolved with hello Azure Mobile Engagement SDK will be discovered by a failure in your application (such as a UI data collection failure, push failure, advanced feature failure, API failure, Application crashes, or apparent service outage).</span></span>  
* <span data-ttu-id="4ee71-114">Pokud konkrétní funkce Azure Mobile Engagement nikdy fungovala ve vaší aplikaci před, budete potřebovat toocomplete hello integrace.</span><span class="sxs-lookup"><span data-stu-id="4ee71-114">If a particular feature of Azure Mobile Engagement has never worked in your app before, you will need toocomplete hello integration.</span></span> 
* <span data-ttu-id="4ee71-115">Pokud konkrétní funkce Azure Mobile Engagement byla práce a byla zastavena, může být nutné tooupgrade toohello poslední verze s hello Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="4ee71-115">If a particular feature of Azure Mobile Engagement was working and stopped, you may need tooupgrade toohello last version with hello Azure Mobile Engagement SDK.</span></span> <span data-ttu-id="4ee71-116">Mějte na paměti, že je jinou verzi hello Azure Mobile Engagement SDK pro každou platformu podporovanou nástrojem Azure Mobile Engagement (Android, iOS, Windows a Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="4ee71-116">Remember that there is a different version of hello Azure Mobile Engagement SDK for each platform supported by Azure Mobile Engagement (Android, iOS, Windows, and Windows Phone).</span></span>

#### <a name="sdk-integration"></a><span data-ttu-id="4ee71-117">Integrace sady SDK</span><span class="sxs-lookup"><span data-stu-id="4ee71-117">SDK Integration</span></span>
* <span data-ttu-id="4ee71-118">Azure Mobile Engagement integrované není správně v sadě SDK (Analytics).</span><span class="sxs-lookup"><span data-stu-id="4ee71-118">Azure Mobile Engagement not correctly integrated in SDK (Analytics).</span></span>
* <span data-ttu-id="4ee71-119">Dosažení integrované není správně v sadě SDK (v aplikaci a mimo nabízených oznámení aplikace).</span><span class="sxs-lookup"><span data-stu-id="4ee71-119">Reach not correctly integrated in SDK (In App and Out of App Pushes).</span></span>
* <span data-ttu-id="4ee71-120">Certificate vs PRODUKČNÍMU vypršela platnost nebo není správný. DEV (jenom iOS).</span><span class="sxs-lookup"><span data-stu-id="4ee71-120">Certificate expired or incorrect PROD vs. DEV (iOS only).</span></span>
* <span data-ttu-id="4ee71-121">GCM nebo ADM integrované není správně v sadě SDK (Android jenom - Service konkrétní nabízených oznámení).</span><span class="sxs-lookup"><span data-stu-id="4ee71-121">GCM or ADM not correctly integrated in SDK (Android only - Service Specific Pushes).</span></span>
* <span data-ttu-id="4ee71-122">Sledování není správně integrované v sadě SDK (sledování instalace úložiště).</span><span class="sxs-lookup"><span data-stu-id="4ee71-122">Tracking not correctly integrated in SDK (Install store tracking).</span></span>
* <span data-ttu-id="4ee71-123">Opožděné umístění nebo umístění GPS integrované není správně v sadě SDK (cílení na podle geografického umístění).</span><span class="sxs-lookup"><span data-stu-id="4ee71-123">Lazy Location or GPS Location not correctly integrated in SDK (Targeting by geo-location).</span></span>

<span data-ttu-id="4ee71-124">**Viz také:**</span><span class="sxs-lookup"><span data-stu-id="4ee71-124">**See also:**</span></span>

* <span data-ttu-id="4ee71-125">[Dokumentaci k sadě SDK – integrace příručky][Link 5]</span><span class="sxs-lookup"><span data-stu-id="4ee71-125">[SDK Documentation - Integration Guides][Link 5]</span></span> 
* <span data-ttu-id="4ee71-126">[Průvodce odstraňováním potíží s - Push][Link 23]</span><span class="sxs-lookup"><span data-stu-id="4ee71-126">[Troubleshooting Guide - Push][Link 23]</span></span>

#### <a name="sdk-upgrade"></a><span data-ttu-id="4ee71-127">Upgrade sady SDK</span><span class="sxs-lookup"><span data-stu-id="4ee71-127">SDK Upgrade</span></span>
* <span data-ttu-id="4ee71-128">Třeba tooupgrade SDK tooresolve problémy se staršími verzemi hello SDK (často související toonewer verze operačního systému zařízení hello).</span><span class="sxs-lookup"><span data-stu-id="4ee71-128">Need tooupgrade SDK tooresolve issues with older versions of hello SDK (often related toonewer versions of hello device OS).</span></span>
* <span data-ttu-id="4ee71-129">Odinstalujte všechny předchozí verze aplikace ze zařízení a znovu nainstalujte nejnovější verzi aplikace hello, hello znovu zaregistrovat ID zařízení z tooconfirm hello Azure Mobile Engagement uživatelského rozhraní, že vaše zařízení používá hello nejnovější verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ee71-129">Uninstall all previous versions of your app from your device and reinstall hello newest version of your app, hello re-register your Device ID from hello Azure Mobile Engagement UI tooconfirm that your device is using hello newest version of your app.</span></span>

<span data-ttu-id="4ee71-130">**Viz také:**</span><span class="sxs-lookup"><span data-stu-id="4ee71-130">**See also:**</span></span>

* [<span data-ttu-id="4ee71-131">Dokumentaci k sadě SDK – poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="4ee71-131">SDK Documentation - Release Notes</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [<span data-ttu-id="4ee71-132">Dokumentaci k sadě SDK - upgradu příručky</span><span class="sxs-lookup"><span data-stu-id="4ee71-132">SDK Documentation - Upgrade Guides</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a><span data-ttu-id="4ee71-133">SDK jiných</span><span class="sxs-lookup"><span data-stu-id="4ee71-133">SDK Other</span></span>
* <span data-ttu-id="4ee71-134">Chyby v Application Manifest "AndroidManifest.xml" může způsobit, že Azure Mobile Engagement toowork (jen Android).</span><span class="sxs-lookup"><span data-stu-id="4ee71-134">Errors in Application Manifest "AndroidManifest.xml" can cause Azure Mobile Engagement not toowork (Android only).</span></span>
* <span data-ttu-id="4ee71-135">Běžné problémy s integraci sady SDK a využití rozhraní API je tooconfuse hello klíč SDK a hello klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4ee71-135">A common issue with SDK integration and API usage is tooconfuse hello SDK Key and hello API Key.</span></span>

<span data-ttu-id="4ee71-136">**Viz také:**</span><span class="sxs-lookup"><span data-stu-id="4ee71-136">**See also:**</span></span>

* <span data-ttu-id="4ee71-137">[Principy – Glosář][Link 6]</span><span class="sxs-lookup"><span data-stu-id="4ee71-137">[Concepts - Glossary][Link 6]</span></span>

## <a name="advanced-coding-issues"></a><span data-ttu-id="4ee71-138">Pokročilé kódování problémy</span><span class="sxs-lookup"><span data-stu-id="4ee71-138">Advanced coding issues</span></span>
### <a name="issue"></a><span data-ttu-id="4ee71-139">Problém</span><span class="sxs-lookup"><span data-stu-id="4ee71-139">Issue</span></span>
* <span data-ttu-id="4ee71-140">Kód konkrétní platformy není přímo souvisí s tooAzure Mobile Engagement v iOS, Android a Windows Phone může způsobit problémy.</span><span class="sxs-lookup"><span data-stu-id="4ee71-140">Platform specific code not directly related tooAzure Mobile Engagement can cause issues on iOS, Android, and Windows Phone.</span></span>

### <a name="causes"></a><span data-ttu-id="4ee71-141">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="4ee71-141">Causes</span></span>
* <span data-ttu-id="4ee71-142">Několik upřesňujících kódování problémy s Azure Mobile Engagement se nezdařila z důvodu nesprávně napsaný platformou konkrétního kódu není přímo souvisí s tooAzure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="4ee71-142">Many advanced coding issues with Azure Mobile Engagement are caused by improperly written platform specific code not directly related tooAzure Mobile Engagement.</span></span> <span data-ttu-id="4ee71-143">Budete potřebovat tooconsult dokumentace konkrétní toohello platformy, které vyvíjíte pro kromě tooAzure Mobile Engagement dokumentace (Android, iOS, Web, systém Windows a Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="4ee71-143">You will need tooconsult documentation specific toohello platform you are developing for in addition tooAzure Mobile Engagement documentation (Android, iOS, Web, Windows, and Windows Phone).</span></span>
* <span data-ttu-id="4ee71-144">Konfigurace není správně "kategorie", zabraňuje propojení z umístění tooanother oznámení uvnitř nebo vně aplikace hello (jen Android).</span><span class="sxs-lookup"><span data-stu-id="4ee71-144">Not correctly configuring "categories", prevents linking from a notification tooanother location either inside or outside of hello app (Android only).</span></span> 
* <span data-ttu-id="4ee71-145">Není nastavení "UIKit.framework" příliš "volitelné" ve vašem kódu iOS zobrazí "Symbol nebyla nalezena chyba" nebo dojde k chybě na zařízeních s iOS starší (jenom iOS).</span><span class="sxs-lookup"><span data-stu-id="4ee71-145">Not setting "UIKit.framework" too"optional" in your iOS code, shows a "Symbol not found error" and/or crashes on older iOS devices (iOS only).</span></span>
* <span data-ttu-id="4ee71-146">Platnost certifikátů nebo pomocí není správně hello DEV nebo produkčnímu verze hello cert, nabízené příčiny problémů (jenom iOS).</span><span class="sxs-lookup"><span data-stu-id="4ee71-146">Expired certificates or not correctly using hello DEV or Prod version of hello cert, causes push issues (iOS only).</span></span>
* <span data-ttu-id="4ee71-147">Existují některá omezení vyplývajících tooa platforma, která Azure Mobile Engagement nemůže řídit (jako jsou jak funguje hello system center pro mimo aplikaci nabízených oznámení v Android a iOS).</span><span class="sxs-lookup"><span data-stu-id="4ee71-147">There are some limitations inherent tooa platform that Azure Mobile Engagement can't control (like how hello system center works for out of app pushes in Android and iOS).</span></span>
* <span data-ttu-id="4ee71-148">Azure Mobile Engagement publikuje úplný seznam hello interní balíčky používané Azure Mobile Engagementem pro iOS a Android pro referenci.</span><span class="sxs-lookup"><span data-stu-id="4ee71-148">Azure Mobile Engagement publishes a full list of hello internal packages used by Azure Mobile Engagement for iOS and Android for reference.</span></span> <span data-ttu-id="4ee71-149">Mějte na paměti, že některé funkce Azure Mobile Engagement jsou konkrétní toohello platformy (Android, iOS, Web, systém Windows a Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="4ee71-149">Keep in mind that some features of Azure Mobile Engagement are specific toohello platform (Android, iOS, Web, Windows, and Windows Phone).</span></span>

### <a name="see-also"></a><span data-ttu-id="4ee71-150">Viz také</span><span class="sxs-lookup"><span data-stu-id="4ee71-150">See also</span></span>
* <span data-ttu-id="4ee71-151">[Průvodce odstraňováním potíží s - Push][Link 23]</span><span class="sxs-lookup"><span data-stu-id="4ee71-151">[Troubleshooting Guide - Push][Link 23]</span></span> 
* <span data-ttu-id="4ee71-152">[Dokumentaci k sadě SDK – poznámky k verzi][Link 5]</span><span class="sxs-lookup"><span data-stu-id="4ee71-152">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="4ee71-153">[Dokumentaci k sadě SDK - upgradu příručky][Link 5]</span><span class="sxs-lookup"><span data-stu-id="4ee71-153">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="application-crashes"></a><span data-ttu-id="4ee71-154">Selhání aplikace</span><span class="sxs-lookup"><span data-stu-id="4ee71-154">Application crashes</span></span>
### <a name="issue"></a><span data-ttu-id="4ee71-155">Problém</span><span class="sxs-lookup"><span data-stu-id="4ee71-155">Issue</span></span>
* <span data-ttu-id="4ee71-156">Aplikace na zařízení koncoví uživatelé hello dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4ee71-156">Your application crashes on hello end users' device.</span></span>

### <a name="causes"></a><span data-ttu-id="4ee71-157">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="4ee71-157">Causes</span></span>
* <span data-ttu-id="4ee71-158">Informace o havárii lze zobrazit v hello *uživatelského rozhraní Analytics* nebo hello *Analytics rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="4ee71-158">Crash information can be viewed in hello *Analytics UI* or hello *Analytics API*</span></span>
* <span data-ttu-id="4ee71-159">Můžete najít hello zařízení ID testovací zařízení a proveďte hello stejnou akci, která způsobila, že vaše aplikace toocrash pro toohelp koncový uživatel určit příčinu hello vaší havárie.</span><span class="sxs-lookup"><span data-stu-id="4ee71-159">You can find hello Device ID of your test device and take hello same action that caused your application toocrash for an end user toohelp identify hello cause of your crash.</span></span>
* <span data-ttu-id="4ee71-160">Známé problémy s hello Azure Mobile Engagement SDK, které způsobí toocrash aplikací jsou někdy vyřešit upgradem toohello nejnovější verzi hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4ee71-160">Known issues with hello Azure Mobile Engagement SDK that cause applications toocrash are sometimes resolved by upgrading toohello latest version of hello SDK.</span></span> <span data-ttu-id="4ee71-161">Ujistěte se, že toocheck hello poznámky o vaši platformu při zkoumání dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4ee71-161">Make sure toocheck hello release notes about your platform when investigating crashes.</span></span>

### <a name="see-also"></a><span data-ttu-id="4ee71-162">Viz také</span><span class="sxs-lookup"><span data-stu-id="4ee71-162">See also</span></span>
* <span data-ttu-id="4ee71-163">[Dokumentaci k sadě SDK – poznámky k verzi][Link 5]</span><span class="sxs-lookup"><span data-stu-id="4ee71-163">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="4ee71-164">[Dokumentaci k sadě SDK - upgradu příručky][Link 5]</span><span class="sxs-lookup"><span data-stu-id="4ee71-164">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="app-store-upload-failures"></a><span data-ttu-id="4ee71-165">Chyb odesílání App store</span><span class="sxs-lookup"><span data-stu-id="4ee71-165">App store upload failures</span></span>
### <a name="issue"></a><span data-ttu-id="4ee71-166">Problém</span><span class="sxs-lookup"><span data-stu-id="4ee71-166">Issue</span></span>
* <span data-ttu-id="4ee71-167">Chyby související s toouploading hello nejnovější verzi vaší aplikace tooApple, hello aplikace pro Windows store nebo Google.</span><span class="sxs-lookup"><span data-stu-id="4ee71-167">Errors related toouploading hello latest version of your app tooApple, Google, or hello Windows App store.</span></span>

### <a name="causes"></a><span data-ttu-id="4ee71-168">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="4ee71-168">Causes</span></span>
* <span data-ttu-id="4ee71-169">Aplikace ukládá někdy blokovat aplikacím s určité funkce povolit (například hello Apple Store brání použití hello IDFV v aplikacích v úložišti hello a úložiště GooglePlay hello brání hello sdílení informací o aplikaci mezi aplikacemi).</span><span class="sxs-lookup"><span data-stu-id="4ee71-169">App stores sometimes block apps with certain features enabled (e.g. hello Apple Store prevents hello use of IDFV in apps in hello store and hello GooglePlay store prevents hello sharing of application information between apps).</span></span> 
* <span data-ttu-id="4ee71-170">Zajistěte, aby zkontrolujte poznámky k verzi hello o platformy a aktuální verzi hello SDK, pokud máte potíže při odesílání toohello obchod s aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="4ee71-170">Make sure that you check hello release notes about your platform and current version of hello SDK if you have difficulty uploading an app toohello store.</span></span>

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

