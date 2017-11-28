---
title: "Upřesňující konfigurace pro Azure Mobile Engagement Android SDK"
description: "Popisuje možnosti pokročilé konfigurace, včetně Android Manifest s Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 37d2c09a-86fa-473d-8987-c7e35a0eb3e8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 0301f71c76872714aa1bf727a6c21dd7a63db036
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="ec5cc-103">Upřesňující konfigurace pro Azure Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="ec5cc-103">Advanced configuration for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ec5cc-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="ec5cc-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="ec5cc-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="ec5cc-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="ec5cc-106">iOS</span><span class="sxs-lookup"><span data-stu-id="ec5cc-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="ec5cc-107">Android</span><span class="sxs-lookup"><span data-stu-id="ec5cc-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
>
>

<span data-ttu-id="ec5cc-108">Tento postup popisuje, jak nakonfigurovat různé možnosti konfigurace aplikací pro Azure Mobile Engagement Android.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-108">This procedure describes how to configure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec5cc-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec5cc-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a><span data-ttu-id="ec5cc-110">Požadavky na oprávnění</span><span class="sxs-lookup"><span data-stu-id="ec5cc-110">Permission Requirements</span></span>
<span data-ttu-id="ec5cc-111">Některé možnosti vyžadují specifické oprávnění, které jsou zde uvedeny pro referenční a v řádku v příslušné funkce.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-111">Some options require specific permissions, all of which are listed here for reference, and in-line in the specific feature.</span></span> <span data-ttu-id="ec5cc-112">Přidejte oprávnění do souboru AndroidManifest.xml vašeho projektu těsně před nebo po `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-112">Add these permissions to the AndroidManifest.xml of your project immediately before or after the `<application>` tag.</span></span>

<span data-ttu-id="ec5cc-113">Kód oprávnění vyžaduje, aby vypadala jako následující, kde vyplňte příslušná oprávnění z tabulky, který následuje.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-113">The permission code needs to look like the following, where you fill in the appropriate permission from the table that follows.</span></span>

    <uses-permission android:name="android.permission.[specific permission]"/>


| <span data-ttu-id="ec5cc-114">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="ec5cc-114">Permission</span></span> | <span data-ttu-id="ec5cc-115">Při použití</span><span class="sxs-lookup"><span data-stu-id="ec5cc-115">When used</span></span> |
| --- | --- |
| <span data-ttu-id="ec5cc-116">INTERNET</span><span class="sxs-lookup"><span data-stu-id="ec5cc-116">INTERNET</span></span> |<span data-ttu-id="ec5cc-117">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-117">Required.</span></span> <span data-ttu-id="ec5cc-118">Pro základní vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="ec5cc-118">For basic reporting</span></span> |
| <span data-ttu-id="ec5cc-119">ACCESS_NETWORK_STATE</span><span class="sxs-lookup"><span data-stu-id="ec5cc-119">ACCESS_NETWORK_STATE</span></span> |<span data-ttu-id="ec5cc-120">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-120">Required.</span></span> <span data-ttu-id="ec5cc-121">Pro základní vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="ec5cc-121">For basic reporting</span></span> |
| <span data-ttu-id="ec5cc-122">RECEIVE_BOOT_COMPLETED</span><span class="sxs-lookup"><span data-stu-id="ec5cc-122">RECEIVE_BOOT_COMPLETED</span></span> |<span data-ttu-id="ec5cc-123">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-123">Required.</span></span> <span data-ttu-id="ec5cc-124">Objeví centru oznámení po restartování zařízení</span><span class="sxs-lookup"><span data-stu-id="ec5cc-124">To show up the notifications center after device reboot</span></span> |
| <span data-ttu-id="ec5cc-125">WAKE_LOCK</span><span class="sxs-lookup"><span data-stu-id="ec5cc-125">WAKE_LOCK</span></span> |<span data-ttu-id="ec5cc-126">Nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-126">Recommended.</span></span> <span data-ttu-id="ec5cc-127">Umožňuje shromažďování dat při použití Wi-Fi nebo při vypnuté obrazovky</span><span class="sxs-lookup"><span data-stu-id="ec5cc-127">Enables collecting data when using WiFi or when screen is off</span></span> |
| <span data-ttu-id="ec5cc-128">ZAVIBROVAT</span><span class="sxs-lookup"><span data-stu-id="ec5cc-128">VIBRATE</span></span> |<span data-ttu-id="ec5cc-129">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-129">Optional.</span></span> <span data-ttu-id="ec5cc-130">Umožňuje vibracím při přijetí oznámení</span><span class="sxs-lookup"><span data-stu-id="ec5cc-130">Enables vibration when notifications are received</span></span> |
| <span data-ttu-id="ec5cc-131">DOWNLOAD_WITHOUT_NOTIFICATION</span><span class="sxs-lookup"><span data-stu-id="ec5cc-131">DOWNLOAD_WITHOUT_NOTIFICATION</span></span> |<span data-ttu-id="ec5cc-132">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-132">Optional.</span></span> <span data-ttu-id="ec5cc-133">Umožňuje oznámení Android velký obrázek</span><span class="sxs-lookup"><span data-stu-id="ec5cc-133">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="ec5cc-134">WRITE_EXTERNAL_STORAGE</span><span class="sxs-lookup"><span data-stu-id="ec5cc-134">WRITE_EXTERNAL_STORAGE</span></span> |<span data-ttu-id="ec5cc-135">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-135">Optional.</span></span> <span data-ttu-id="ec5cc-136">Umožňuje oznámení Android velký obrázek</span><span class="sxs-lookup"><span data-stu-id="ec5cc-136">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="ec5cc-137">ACCESS_COARSE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="ec5cc-137">ACCESS_COARSE_LOCATION</span></span> |<span data-ttu-id="ec5cc-138">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-138">Optional.</span></span> <span data-ttu-id="ec5cc-139">Umožňuje hlášení polohy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="ec5cc-139">Enables Real-time location reporting</span></span> |
| <span data-ttu-id="ec5cc-140">ACCESS_FINE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="ec5cc-140">ACCESS_FINE_LOCATION</span></span> |<span data-ttu-id="ec5cc-141">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-141">Optional.</span></span> <span data-ttu-id="ec5cc-142">Umožňuje vytváření sestav na základě GPS umístění</span><span class="sxs-lookup"><span data-stu-id="ec5cc-142">Enables GPS-based location reporting</span></span> |

<span data-ttu-id="ec5cc-143">Od verze Android M [některá oprávnění jsou spravovány v době běhu](mobile-engagement-android-location-reporting.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="ec5cc-143">Starting with Android M, [some permissions are managed at run time](mobile-engagement-android-location-reporting.md#android-m-permissions).</span></span>

<span data-ttu-id="ec5cc-144">Pokud už používáte ``ACCESS_FINE_LOCATION``, pak nemusíte použít také ``ACCESS_COARSE_LOCATION``.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-144">If you are already using ``ACCESS_FINE_LOCATION``, then you don't need to also use ``ACCESS_COARSE_LOCATION``.</span></span>

## <a name="android-manifest-configuration-options"></a><span data-ttu-id="ec5cc-145">Možnosti konfigurace pro Android manifestu</span><span class="sxs-lookup"><span data-stu-id="ec5cc-145">Android Manifest configuration options</span></span>
### <a name="crash-report"></a><span data-ttu-id="ec5cc-146">Hlášení o selhání</span><span class="sxs-lookup"><span data-stu-id="ec5cc-146">Crash report</span></span>
<span data-ttu-id="ec5cc-147">Chcete-li zakázat hlášení selhání, přidejte tento kód mezi `<application>` a `</application>` značky:</span><span class="sxs-lookup"><span data-stu-id="ec5cc-147">To disable crash reports, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="ec5cc-148">Prahová hodnota shluku</span><span class="sxs-lookup"><span data-stu-id="ec5cc-148">Burst threshold</span></span>
<span data-ttu-id="ec5cc-149">Ve výchozím nastavení sestavy služby zapojení protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-149">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="ec5cc-150">Pokud se vaše protokoly sestavy aplikací často liší, je lepší ukládat do vyrovnávací paměti protokoly a sestavy je všechny najednou na základní pravidelného času (nazývané "burst režim").</span><span class="sxs-lookup"><span data-stu-id="ec5cc-150">If your application report logs vary frequently, it is better to buffer the logs and to report them all at once on a regular time base (called "burst mode").</span></span> <span data-ttu-id="ec5cc-151">Uděláte to tak, přidejte tento kód mezi `<application>` a `</application>` značky:</span><span class="sxs-lookup"><span data-stu-id="ec5cc-151">To do so, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="ec5cc-152">Režim shluků mírně zvyšuje výdrž baterie, ale má vliv na zapojení monitorování: všechny dobu trvání relace a úlohy jsou zaokrouhleny na shluků prahovou hodnotu (tedy relací a úlohy kratší, než je prahová hodnota shluků nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="ec5cc-152">Burst mode slightly increases the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration are rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="ec5cc-153">Vaše shluků prahová hodnota musí být delší než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="ec5cc-153">Your burst threshold should be no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="ec5cc-154">Časový limit relace</span><span class="sxs-lookup"><span data-stu-id="ec5cc-154">Session timeout</span></span>
 <span data-ttu-id="ec5cc-155">Aktivitu můžete ukončit stisknutím kombinace kláves **Domů** nebo **zpět** klíče, nastavení telefonu nečinnosti nebo přechod do jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-155">You can end an activity by pressing the **Home** or **Back** key, by setting the phone idle or by jumping into another application.</span></span> <span data-ttu-id="ec5cc-156">Ve výchozím nastavení je relace skončila deseti sekund po uplynutí jeho poslední aktivita.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-156">By default, a session is ended ten seconds after the end of its last activity.</span></span> <span data-ttu-id="ec5cc-157">Tím je zabráněno rozdělení relace pokaždé, když uživatel ukončí a vrátí do aplikace rychle, což může dojít při uživatel převezme bitovou kopii, zkontroluje, oznámení atd. Můžete upravit tento parametr.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-157">This avoids a session split each time the user exits and returns to the application quickly, which can happen when the user picks up an image, checks a notification, etc. You may want to modify this parameter.</span></span> <span data-ttu-id="ec5cc-158">Uděláte to tak, přidejte tento kód mezi `<application>` a `</application>` značky:</span><span class="sxs-lookup"><span data-stu-id="ec5cc-158">To do so, add this code between the `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="ec5cc-159">Zakázání zasílání zpráv protokolu</span><span class="sxs-lookup"><span data-stu-id="ec5cc-159">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="ec5cc-160">Pomocí volání metody</span><span class="sxs-lookup"><span data-stu-id="ec5cc-160">Using a method call</span></span>
<span data-ttu-id="ec5cc-161">Pokud chcete Engagement zastavit odesílání protokolů, můžete volat:</span><span class="sxs-lookup"><span data-stu-id="ec5cc-161">If you want Engagement to stop sending logs, you can call:</span></span>

    EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="ec5cc-162">Toto volání je trvalé: používá soubor sdílet předvolby.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-162">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="ec5cc-163">Pokud je při volání této funkce aktivní zapojení, může trvat jednu minutu pro zastavení služby.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-163">If Engagement is active when you call this function, it may take one minute for the service to stop.</span></span> <span data-ttu-id="ec5cc-164">Ale se nespustí službu vůbec při příštím spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-164">However it won't launch the service at all the next time you launch the application.</span></span>

<span data-ttu-id="ec5cc-165">Můžete povolit protokol reporting znovu voláním stejnou funkci s `true`.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-165">You can enable log reporting again by calling the same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="ec5cc-166">Integrace ve vašem vlastním`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="ec5cc-166">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="ec5cc-167">Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-167">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="ec5cc-168">Zapojení používat vaše předvolby soubor (s požadovanou režim) můžete nakonfigurovat `AndroidManifest.xml` soubor s `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="ec5cc-168">You can configure Engagement to use your preferences file (with the desired mode) in the `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="ec5cc-169">`engagement:agent:settings:name` Klíč se používá k definování názvu souboru sdílet předvolby.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-169">The `engagement:agent:settings:name` key is used to define the name of the shared preferences file.</span></span>
* <span data-ttu-id="ec5cc-170">`engagement:agent:settings:mode` Klíč se používá k definování režim souboru sdílet předvolby.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-170">The `engagement:agent:settings:mode` key is used to define the mode of the shared preferences file.</span></span> <span data-ttu-id="ec5cc-171">Používat stejný režim jako ve vašem `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-171">Use the same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="ec5cc-172">Režim musí být předán jako číslo: Pokud používáte kombinaci konstantní příznaky ve vašem kódu, zkontrolujte celkovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-172">The mode must be passed as a number: if you are using a combination of constant flags in your code, check the total value.</span></span>

<span data-ttu-id="ec5cc-173">Zapojení vždy používá `engagement:key` boolean klíč v souboru předvoleb pro správu tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="ec5cc-173">Engagement always uses the `engagement:key` boolean key within the preferences file for managing this setting.</span></span>

<span data-ttu-id="ec5cc-174">Následující příklad `AndroidManifest.xml` zobrazí výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ec5cc-174">The following example of `AndroidManifest.xml` shows the default values:</span></span>

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

<span data-ttu-id="ec5cc-175">Potom můžete přidat `CheckBoxPreference` v vaše předvolby rozložení stejný, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="ec5cc-175">Then you can add a `CheckBoxPreference` in your preference layout like the following one:</span></span>

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
