---
title: "Řešení potíží s Průvodce - SDK Azure Mobile Engagement."
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
ms.openlocfilehash: 4d9d6165deb4bd0c65f1841aa7c457363a1f2865
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a><span data-ttu-id="58de9-103">Průvodci odstraňováním potíží problémů s integrací sady SDK</span><span class="sxs-lookup"><span data-stu-id="58de9-103">Troubleshooting guide for SDK integration issues</span></span>
<span data-ttu-id="58de9-104">Následují možných problémů se můžete setkat s jak Azure Mobile Engagement integruje do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="58de9-104">The following are possible issues you may encounter with how Azure Mobile Engagement integrates into your application.</span></span>

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a><span data-ttu-id="58de9-105">SDK problémů zjištěných selhání v jiné oblasti vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="58de9-105">SDK issues discovered by a failure in another area of your application</span></span>
### <a name="issue"></a><span data-ttu-id="58de9-106">Problém</span><span class="sxs-lookup"><span data-stu-id="58de9-106">Issue</span></span>
* <span data-ttu-id="58de9-107">Chyba kolekce dat uživatelského rozhraní (v analýzy, monitorování, segmentace nebo řídicí panely).</span><span class="sxs-lookup"><span data-stu-id="58de9-107">UI data collection failure (in Analytics, Monitoring, Segmentation, or Dashboards).</span></span>
* <span data-ttu-id="58de9-108">Push selhání (nabízených oznámení nefungují v aplikaci a mimo aplikaci, nebo obojí).</span><span class="sxs-lookup"><span data-stu-id="58de9-108">Push Failures (Pushes don't work in app, out of app, or both).</span></span>
* <span data-ttu-id="58de9-109">Pokročilé funkce selhání (sledování, informace o zeměpisné poloze nebo platformy, které nepodporují konkrétní nabízených oznámení).</span><span class="sxs-lookup"><span data-stu-id="58de9-109">Advanced Feature Failures (Tracking, Geolocation, or platform specific Pushes don’t work).</span></span>
* <span data-ttu-id="58de9-110">Selhání rozhraní API (rozhraní API selhání často bezobslužném režimu bez chybové zprávy).</span><span class="sxs-lookup"><span data-stu-id="58de9-110">API Failures (APIs fail often silently without error messages).</span></span>
* <span data-ttu-id="58de9-111">Selhání služby (žádná z Azure Mobile Engagement funguje pro vaši aplikaci).</span><span class="sxs-lookup"><span data-stu-id="58de9-111">Service Failures (none of Azure Mobile Engagement works for your application).</span></span>

### <a name="causes"></a><span data-ttu-id="58de9-112">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="58de9-112">Causes</span></span>
* <span data-ttu-id="58de9-113">Většiny problémů, které musí být rozpoznány s Azure Mobile Engagement SDK budou zjištěny chybou v aplikaci (například selhání shromažďování dat uživatelského rozhraní, selhání nabízená, chybu upřesňující funkce, selhání rozhraní API, havárie aplikací nebo výpadkem zřejmá služby) .</span><span class="sxs-lookup"><span data-stu-id="58de9-113">Most issues that need to be resolved with the Azure Mobile Engagement SDK will be discovered by a failure in your application (such as a UI data collection failure, push failure, advanced feature failure, API failure, Application crashes, or apparent service outage).</span></span>  
* <span data-ttu-id="58de9-114">Pokud konkrétní funkce Azure Mobile Engagement ve vaší aplikaci před nikdy fungovala, musíte se k dokončení integrace.</span><span class="sxs-lookup"><span data-stu-id="58de9-114">If a particular feature of Azure Mobile Engagement has never worked in your app before, you will need to complete the integration.</span></span> 
* <span data-ttu-id="58de9-115">Pokud konkrétní funkce Azure Mobile Engagement byla práce a byla zastavena, musíte aktualizovat na poslední verzi s Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="58de9-115">If a particular feature of Azure Mobile Engagement was working and stopped, you may need to upgrade to the last version with the Azure Mobile Engagement SDK.</span></span> <span data-ttu-id="58de9-116">Mějte na paměti, že je jinou verzi sady Azure Mobile Engagement SDK pro každou platformu podporovanou nástrojem Azure Mobile Engagement (Android, iOS, Windows a Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="58de9-116">Remember that there is a different version of the Azure Mobile Engagement SDK for each platform supported by Azure Mobile Engagement (Android, iOS, Windows, and Windows Phone).</span></span>

#### <a name="sdk-integration"></a><span data-ttu-id="58de9-117">Integrace sady SDK</span><span class="sxs-lookup"><span data-stu-id="58de9-117">SDK Integration</span></span>
* <span data-ttu-id="58de9-118">Azure Mobile Engagement integrované není správně v sadě SDK (Analytics).</span><span class="sxs-lookup"><span data-stu-id="58de9-118">Azure Mobile Engagement not correctly integrated in SDK (Analytics).</span></span>
* <span data-ttu-id="58de9-119">Dosažení integrované není správně v sadě SDK (v aplikaci a mimo nabízených oznámení aplikace).</span><span class="sxs-lookup"><span data-stu-id="58de9-119">Reach not correctly integrated in SDK (In App and Out of App Pushes).</span></span>
* <span data-ttu-id="58de9-120">Certificate vs PRODUKČNÍMU vypršela platnost nebo není správný. DEV (jenom iOS).</span><span class="sxs-lookup"><span data-stu-id="58de9-120">Certificate expired or incorrect PROD vs. DEV (iOS only).</span></span>
* <span data-ttu-id="58de9-121">GCM nebo ADM integrované není správně v sadě SDK (Android jenom - Service konkrétní nabízených oznámení).</span><span class="sxs-lookup"><span data-stu-id="58de9-121">GCM or ADM not correctly integrated in SDK (Android only - Service Specific Pushes).</span></span>
* <span data-ttu-id="58de9-122">Sledování není správně integrované v sadě SDK (sledování instalace úložiště).</span><span class="sxs-lookup"><span data-stu-id="58de9-122">Tracking not correctly integrated in SDK (Install store tracking).</span></span>
* <span data-ttu-id="58de9-123">Opožděné umístění nebo umístění GPS integrované není správně v sadě SDK (cílení na podle geografického umístění).</span><span class="sxs-lookup"><span data-stu-id="58de9-123">Lazy Location or GPS Location not correctly integrated in SDK (Targeting by geo-location).</span></span>

<span data-ttu-id="58de9-124">**Viz také:**</span><span class="sxs-lookup"><span data-stu-id="58de9-124">**See also:**</span></span>

* <span data-ttu-id="58de9-125">[Dokumentaci k sadě SDK – integrace příručky][Link 5]</span><span class="sxs-lookup"><span data-stu-id="58de9-125">[SDK Documentation - Integration Guides][Link 5]</span></span> 
* <span data-ttu-id="58de9-126">[Průvodce odstraňováním potíží s - Push][Link 23]</span><span class="sxs-lookup"><span data-stu-id="58de9-126">[Troubleshooting Guide - Push][Link 23]</span></span>

#### <a name="sdk-upgrade"></a><span data-ttu-id="58de9-127">Upgrade sady SDK</span><span class="sxs-lookup"><span data-stu-id="58de9-127">SDK Upgrade</span></span>
* <span data-ttu-id="58de9-128">Třeba upgradovat SDK a řešit problémy s starší verze sady SDK (často souvisejících s novější verzí operačního systému zařízení).</span><span class="sxs-lookup"><span data-stu-id="58de9-128">Need to upgrade SDK to resolve issues with older versions of the SDK (often related to newer versions of the device OS).</span></span>
* <span data-ttu-id="58de9-129">Odinstalujte všechny předchozí verze aplikace ze zařízení a znovu nainstalujte nejnovější verzi aplikace znovu zaregistrovat ID zařízení z rozhraní Azure Mobile Engagement k potvrzení, že vaše zařízení používá nejnovější verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="58de9-129">Uninstall all previous versions of your app from your device and reinstall the newest version of your app, the re-register your Device ID from the Azure Mobile Engagement UI to confirm that your device is using the newest version of your app.</span></span>

<span data-ttu-id="58de9-130">**Viz také:**</span><span class="sxs-lookup"><span data-stu-id="58de9-130">**See also:**</span></span>

* [<span data-ttu-id="58de9-131">Dokumentaci k sadě SDK – poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="58de9-131">SDK Documentation - Release Notes</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [<span data-ttu-id="58de9-132">Dokumentaci k sadě SDK - upgradu příručky</span><span class="sxs-lookup"><span data-stu-id="58de9-132">SDK Documentation - Upgrade Guides</span></span>](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a><span data-ttu-id="58de9-133">SDK jiných</span><span class="sxs-lookup"><span data-stu-id="58de9-133">SDK Other</span></span>
* <span data-ttu-id="58de9-134">Azure Mobile Engagement nepracuje (jen Android) může způsobit chyby "AndroidManifest.xml" manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="58de9-134">Errors in Application Manifest "AndroidManifest.xml" can cause Azure Mobile Engagement not to work (Android only).</span></span>
* <span data-ttu-id="58de9-135">Běžné problémy s integraci sady SDK a využití rozhraní API je zaměnit klíč SDK a klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="58de9-135">A common issue with SDK integration and API usage is to confuse the SDK Key and the API Key.</span></span>

<span data-ttu-id="58de9-136">**Viz také:**</span><span class="sxs-lookup"><span data-stu-id="58de9-136">**See also:**</span></span>

* <span data-ttu-id="58de9-137">[Principy – Glosář][Link 6]</span><span class="sxs-lookup"><span data-stu-id="58de9-137">[Concepts - Glossary][Link 6]</span></span>

## <a name="advanced-coding-issues"></a><span data-ttu-id="58de9-138">Pokročilé kódování problémy</span><span class="sxs-lookup"><span data-stu-id="58de9-138">Advanced coding issues</span></span>
### <a name="issue"></a><span data-ttu-id="58de9-139">Problém</span><span class="sxs-lookup"><span data-stu-id="58de9-139">Issue</span></span>
* <span data-ttu-id="58de9-140">Ne přímo související s Azure Mobile Engagement konkrétní kód platformy může způsobit problémy na iOS, Android a Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="58de9-140">Platform specific code not directly related to Azure Mobile Engagement can cause issues on iOS, Android, and Windows Phone.</span></span>

### <a name="causes"></a><span data-ttu-id="58de9-141">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="58de9-141">Causes</span></span>
* <span data-ttu-id="58de9-142">Několik upřesňujících kódování problémy s Azure Mobile Engagement jsou způsobeny konkrétní kód nesprávně napsaný platformy nesouvisí přímo s Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="58de9-142">Many advanced coding issues with Azure Mobile Engagement are caused by improperly written platform specific code not directly related to Azure Mobile Engagement.</span></span> <span data-ttu-id="58de9-143">Musíte se k dokumentaci specifické pro platformy, které vyvíjíte pro kromě dokumentace Azure Mobile Engagement (Android, iOS, Web, systém Windows a Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="58de9-143">You will need to consult documentation specific to the platform you are developing for in addition to Azure Mobile Engagement documentation (Android, iOS, Web, Windows, and Windows Phone).</span></span>
* <span data-ttu-id="58de9-144">Konfigurace není správně "kategorie", zabraňuje propojení z oznámení do jiného umístění uvnitř nebo mimo aplikaci (jen Android).</span><span class="sxs-lookup"><span data-stu-id="58de9-144">Not correctly configuring "categories", prevents linking from a notification to another location either inside or outside of the app (Android only).</span></span> 
* <span data-ttu-id="58de9-145">Není nastavení "UIKit.framework" na "volitelné" ve vašem kódu iOS zobrazí "Symbol nebyla nalezena chyba" nebo dojde k chybě na zařízeních s iOS starší (jenom iOS).</span><span class="sxs-lookup"><span data-stu-id="58de9-145">Not setting "UIKit.framework" to "optional" in your iOS code, shows a "Symbol not found error" and/or crashes on older iOS devices (iOS only).</span></span>
* <span data-ttu-id="58de9-146">Platnost certifikátů nebo není správně pomocí Vývojového nebo produkčnímu verze CERT příčiny problémů nabízené (jenom iOS).</span><span class="sxs-lookup"><span data-stu-id="58de9-146">Expired certificates or not correctly using the DEV or Prod version of the cert, causes push issues (iOS only).</span></span>
* <span data-ttu-id="58de9-147">Jsou systému platformu, která Azure Mobile Engagement nemůže řídit (jako jsou jak funguje produktu system center pro mimo aplikaci nabízených oznámení v Android a iOS) určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="58de9-147">There are some limitations inherent to a platform that Azure Mobile Engagement can't control (like how the system center works for out of app pushes in Android and iOS).</span></span>
* <span data-ttu-id="58de9-148">Azure Mobile Engagement publikuje úplný seznam interní balíčky pro referenci používá pro iOS a Android pomocí Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="58de9-148">Azure Mobile Engagement publishes a full list of the internal packages used by Azure Mobile Engagement for iOS and Android for reference.</span></span> <span data-ttu-id="58de9-149">Mějte na paměti, že některé funkce Azure Mobile Engagement jsou specifické pro platformu (Android, iOS, Web, systém Windows a Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="58de9-149">Keep in mind that some features of Azure Mobile Engagement are specific to the platform (Android, iOS, Web, Windows, and Windows Phone).</span></span>

### <a name="see-also"></a><span data-ttu-id="58de9-150">Viz také</span><span class="sxs-lookup"><span data-stu-id="58de9-150">See also</span></span>
* <span data-ttu-id="58de9-151">[Průvodce odstraňováním potíží s - Push][Link 23]</span><span class="sxs-lookup"><span data-stu-id="58de9-151">[Troubleshooting Guide - Push][Link 23]</span></span> 
* <span data-ttu-id="58de9-152">[Dokumentaci k sadě SDK – poznámky k verzi][Link 5]</span><span class="sxs-lookup"><span data-stu-id="58de9-152">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="58de9-153">[Dokumentaci k sadě SDK - upgradu příručky][Link 5]</span><span class="sxs-lookup"><span data-stu-id="58de9-153">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="application-crashes"></a><span data-ttu-id="58de9-154">Selhání aplikace</span><span class="sxs-lookup"><span data-stu-id="58de9-154">Application crashes</span></span>
### <a name="issue"></a><span data-ttu-id="58de9-155">Problém</span><span class="sxs-lookup"><span data-stu-id="58de9-155">Issue</span></span>
* <span data-ttu-id="58de9-156">Aplikace, dojde k chybě v zařízení koncových uživatelů.</span><span class="sxs-lookup"><span data-stu-id="58de9-156">Your application crashes on the end users' device.</span></span>

### <a name="causes"></a><span data-ttu-id="58de9-157">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="58de9-157">Causes</span></span>
* <span data-ttu-id="58de9-158">Informace o havárii lze zobrazit v *uživatelského rozhraní Analytics* nebo *Analytics rozhraní API*</span><span class="sxs-lookup"><span data-stu-id="58de9-158">Crash information can be viewed in the *Analytics UI* or the *Analytics API*</span></span>
* <span data-ttu-id="58de9-159">Můžete zjistit ID zařízení testovací zařízení a trvat o stejnou akci, která způsobila, že aplikace havárií pro koncové uživatele k identifikaci příčinu vaší havárií.</span><span class="sxs-lookup"><span data-stu-id="58de9-159">You can find the Device ID of your test device and take the same action that caused your application to crash for an end user to help identify the cause of your crash.</span></span>
* <span data-ttu-id="58de9-160">Známé problémy s Azure Mobile Engagement SDK, které způsobí aplikace došlo k chybě, se někdy vyřeší upgrade na nejnovější verzi sady SDK.</span><span class="sxs-lookup"><span data-stu-id="58de9-160">Known issues with the Azure Mobile Engagement SDK that cause applications to crash are sometimes resolved by upgrading to the latest version of the SDK.</span></span> <span data-ttu-id="58de9-161">Ujistěte se, že zkontrolujte poznámky o vaši platformu při zkoumání dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="58de9-161">Make sure to check the release notes about your platform when investigating crashes.</span></span>

### <a name="see-also"></a><span data-ttu-id="58de9-162">Viz také</span><span class="sxs-lookup"><span data-stu-id="58de9-162">See also</span></span>
* <span data-ttu-id="58de9-163">[Dokumentaci k sadě SDK – poznámky k verzi][Link 5]</span><span class="sxs-lookup"><span data-stu-id="58de9-163">[SDK Documentation - Release Notes][Link 5]</span></span>
* <span data-ttu-id="58de9-164">[Dokumentaci k sadě SDK - upgradu příručky][Link 5]</span><span class="sxs-lookup"><span data-stu-id="58de9-164">[SDK Documentation - Upgrade Guides][Link 5]</span></span>

## <a name="app-store-upload-failures"></a><span data-ttu-id="58de9-165">Chyb odesílání App store</span><span class="sxs-lookup"><span data-stu-id="58de9-165">App store upload failures</span></span>
### <a name="issue"></a><span data-ttu-id="58de9-166">Problém</span><span class="sxs-lookup"><span data-stu-id="58de9-166">Issue</span></span>
* <span data-ttu-id="58de9-167">Chyby související s odesílání nejnovější verzi aplikace Apple, Google nebo aplikace pro Windows store.</span><span class="sxs-lookup"><span data-stu-id="58de9-167">Errors related to uploading the latest version of your app to Apple, Google, or the Windows App store.</span></span>

### <a name="causes"></a><span data-ttu-id="58de9-168">Způsobí, že</span><span class="sxs-lookup"><span data-stu-id="58de9-168">Causes</span></span>
* <span data-ttu-id="58de9-169">Aplikace ukládá někdy blokovat aplikacím s určité funkce povolit (např. Apple Storu brání použití IDFV v aplikacích v úložišti a úložišti GooglePlay brání sdílení informací o aplikaci mezi aplikacemi).</span><span class="sxs-lookup"><span data-stu-id="58de9-169">App stores sometimes block apps with certain features enabled (e.g. the Apple Store prevents the use of IDFV in apps in the store and the GooglePlay store prevents the sharing of application information between apps).</span></span> 
* <span data-ttu-id="58de9-170">Zajistěte, aby zkontrolujte poznámky o platformy a aktuální verzi sady SDK, pokud máte potíže při nahrávání aplikace do úložiště.</span><span class="sxs-lookup"><span data-stu-id="58de9-170">Make sure that you check the release notes about your platform and current version of the SDK if you have difficulty uploading an app to the store.</span></span>

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

