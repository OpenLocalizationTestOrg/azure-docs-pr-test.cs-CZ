---
title: Konfigurace aaaAdvanced pro Azure Mobile Engagement Android SDK
description: "Popisuje hello rozšířených možnostech konfigurace, včetně hello Android Manifest s Azure Mobile Engagement Android SDK"
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
ms.openlocfilehash: 757abf362021fd018f444cae6305524623e77062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a><span data-ttu-id="18b77-103">Upřesňující konfigurace pro Azure Mobile Engagement Android SDK</span><span class="sxs-lookup"><span data-stu-id="18b77-103">Advanced configuration for Azure Mobile Engagement Android SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="18b77-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="18b77-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-configuration.md)
> * [<span data-ttu-id="18b77-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="18b77-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="18b77-106">iOS</span><span class="sxs-lookup"><span data-stu-id="18b77-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="18b77-107">Android</span><span class="sxs-lookup"><span data-stu-id="18b77-107">Android</span></span>](mobile-engagement-android-advanced-configuration.md)
>
>

<span data-ttu-id="18b77-108">Tento postup popisuje, jak tooconfigure různé možnosti konfigurace aplikací pro Azure Mobile Engagement Android.</span><span class="sxs-lookup"><span data-stu-id="18b77-108">This procedure describes how tooconfigure various configuration options for Azure Mobile Engagement Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18b77-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="18b77-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a><span data-ttu-id="18b77-110">Požadavky na oprávnění</span><span class="sxs-lookup"><span data-stu-id="18b77-110">Permission Requirements</span></span>
<span data-ttu-id="18b77-111">Některé možnosti vyžadují specifické oprávnění, které jsou zde uvedeny pro referenční a v řádku v hello příslušné funkce.</span><span class="sxs-lookup"><span data-stu-id="18b77-111">Some options require specific permissions, all of which are listed here for reference, and in-line in hello specific feature.</span></span> <span data-ttu-id="18b77-112">Přidat tyto oprávnění toohello AndroidManifest.xml vašeho projektu těsně před nebo po hello `<application>` značky.</span><span class="sxs-lookup"><span data-stu-id="18b77-112">Add these permissions toohello AndroidManifest.xml of your project immediately before or after hello `<application>` tag.</span></span>

<span data-ttu-id="18b77-113">Kód oprávnění Hello musí toolook jako hello následující, kde vyplňte příslušná oprávnění hello z hello tabulky, který následuje.</span><span class="sxs-lookup"><span data-stu-id="18b77-113">hello permission code needs toolook like hello following, where you fill in hello appropriate permission from hello table that follows.</span></span>

    <uses-permission android:name="android.permission.[specific permission]"/>


| <span data-ttu-id="18b77-114">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="18b77-114">Permission</span></span> | <span data-ttu-id="18b77-115">Při použití</span><span class="sxs-lookup"><span data-stu-id="18b77-115">When used</span></span> |
| --- | --- |
| <span data-ttu-id="18b77-116">INTERNET</span><span class="sxs-lookup"><span data-stu-id="18b77-116">INTERNET</span></span> |<span data-ttu-id="18b77-117">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="18b77-117">Required.</span></span> <span data-ttu-id="18b77-118">Pro základní vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="18b77-118">For basic reporting</span></span> |
| <span data-ttu-id="18b77-119">ACCESS_NETWORK_STATE</span><span class="sxs-lookup"><span data-stu-id="18b77-119">ACCESS_NETWORK_STATE</span></span> |<span data-ttu-id="18b77-120">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="18b77-120">Required.</span></span> <span data-ttu-id="18b77-121">Pro základní vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="18b77-121">For basic reporting</span></span> |
| <span data-ttu-id="18b77-122">RECEIVE_BOOT_COMPLETED</span><span class="sxs-lookup"><span data-stu-id="18b77-122">RECEIVE_BOOT_COMPLETED</span></span> |<span data-ttu-id="18b77-123">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="18b77-123">Required.</span></span> <span data-ttu-id="18b77-124">tooshow do center oznámení hello po restartování zařízení</span><span class="sxs-lookup"><span data-stu-id="18b77-124">tooshow up hello notifications center after device reboot</span></span> |
| <span data-ttu-id="18b77-125">WAKE_LOCK</span><span class="sxs-lookup"><span data-stu-id="18b77-125">WAKE_LOCK</span></span> |<span data-ttu-id="18b77-126">Nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="18b77-126">Recommended.</span></span> <span data-ttu-id="18b77-127">Umožňuje shromažďování dat při použití Wi-Fi nebo při vypnuté obrazovky</span><span class="sxs-lookup"><span data-stu-id="18b77-127">Enables collecting data when using WiFi or when screen is off</span></span> |
| <span data-ttu-id="18b77-128">ZAVIBROVAT</span><span class="sxs-lookup"><span data-stu-id="18b77-128">VIBRATE</span></span> |<span data-ttu-id="18b77-129">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="18b77-129">Optional.</span></span> <span data-ttu-id="18b77-130">Umožňuje vibracím při přijetí oznámení</span><span class="sxs-lookup"><span data-stu-id="18b77-130">Enables vibration when notifications are received</span></span> |
| <span data-ttu-id="18b77-131">DOWNLOAD_WITHOUT_NOTIFICATION</span><span class="sxs-lookup"><span data-stu-id="18b77-131">DOWNLOAD_WITHOUT_NOTIFICATION</span></span> |<span data-ttu-id="18b77-132">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="18b77-132">Optional.</span></span> <span data-ttu-id="18b77-133">Umožňuje oznámení Android velký obrázek</span><span class="sxs-lookup"><span data-stu-id="18b77-133">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="18b77-134">WRITE_EXTERNAL_STORAGE</span><span class="sxs-lookup"><span data-stu-id="18b77-134">WRITE_EXTERNAL_STORAGE</span></span> |<span data-ttu-id="18b77-135">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="18b77-135">Optional.</span></span> <span data-ttu-id="18b77-136">Umožňuje oznámení Android velký obrázek</span><span class="sxs-lookup"><span data-stu-id="18b77-136">Enables Android Big Picture Notification</span></span> |
| <span data-ttu-id="18b77-137">ACCESS_COARSE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="18b77-137">ACCESS_COARSE_LOCATION</span></span> |<span data-ttu-id="18b77-138">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="18b77-138">Optional.</span></span> <span data-ttu-id="18b77-139">Umožňuje hlášení polohy v reálném čase</span><span class="sxs-lookup"><span data-stu-id="18b77-139">Enables Real-time location reporting</span></span> |
| <span data-ttu-id="18b77-140">ACCESS_FINE_LOCATION</span><span class="sxs-lookup"><span data-stu-id="18b77-140">ACCESS_FINE_LOCATION</span></span> |<span data-ttu-id="18b77-141">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="18b77-141">Optional.</span></span> <span data-ttu-id="18b77-142">Umožňuje vytváření sestav na základě GPS umístění</span><span class="sxs-lookup"><span data-stu-id="18b77-142">Enables GPS-based location reporting</span></span> |

<span data-ttu-id="18b77-143">Od verze Android M [některá oprávnění jsou spravovány v době běhu](mobile-engagement-android-location-reporting.md#android-m-permissions).</span><span class="sxs-lookup"><span data-stu-id="18b77-143">Starting with Android M, [some permissions are managed at run time](mobile-engagement-android-location-reporting.md#android-m-permissions).</span></span>

<span data-ttu-id="18b77-144">Pokud už používáte ``ACCESS_FINE_LOCATION``, pak nepotřebujete tooalso použít ``ACCESS_COARSE_LOCATION``.</span><span class="sxs-lookup"><span data-stu-id="18b77-144">If you are already using ``ACCESS_FINE_LOCATION``, then you don't need tooalso use ``ACCESS_COARSE_LOCATION``.</span></span>

## <a name="android-manifest-configuration-options"></a><span data-ttu-id="18b77-145">Možnosti konfigurace pro Android manifestu</span><span class="sxs-lookup"><span data-stu-id="18b77-145">Android Manifest configuration options</span></span>
### <a name="crash-report"></a><span data-ttu-id="18b77-146">Hlášení o selhání</span><span class="sxs-lookup"><span data-stu-id="18b77-146">Crash report</span></span>
<span data-ttu-id="18b77-147">toodisable hlášení selhání, přidejte tento kód mezi hello `<application>` a `</application>` značky:</span><span class="sxs-lookup"><span data-stu-id="18b77-147">toodisable crash reports, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a><span data-ttu-id="18b77-148">Prahová hodnota shluku</span><span class="sxs-lookup"><span data-stu-id="18b77-148">Burst threshold</span></span>
<span data-ttu-id="18b77-149">Ve výchozím nastavení sestavy služby zapojení hello protokolů v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="18b77-149">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="18b77-150">Pokud se vaše protokoly sestavy aplikací často liší, je lepší toobuffer hello protokoly a tooreport je všechny najednou na pravidelné základní časové (nazývané "burst režim").</span><span class="sxs-lookup"><span data-stu-id="18b77-150">If your application report logs vary frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (called "burst mode").</span></span> <span data-ttu-id="18b77-151">toodo Ano, přidat tento kód mezi hello `<application>` a `</application>` značky:</span><span class="sxs-lookup"><span data-stu-id="18b77-151">toodo so, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

<span data-ttu-id="18b77-152">Shluků režimu mírně zvyšuje hello z baterie, ale má vliv na hello monitorování Engagement: všechny dobu trvání relace a úlohy jsou prahová hodnota shluků zaokrouhlené toohello (tedy relací a úlohy kratší, než je prahová hodnota shluků hello nemusí být zobrazeny).</span><span class="sxs-lookup"><span data-stu-id="18b77-152">Burst mode slightly increases hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration are rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="18b77-153">Vaše shluků prahová hodnota musí být delší než 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="18b77-153">Your burst threshold should be no longer than 30000 (30s).</span></span>

### <a name="session-timeout"></a><span data-ttu-id="18b77-154">Časový limit relace</span><span class="sxs-lookup"><span data-stu-id="18b77-154">Session timeout</span></span>
 <span data-ttu-id="18b77-155">Aktivitu můžete ukončit pomocí klávesy hello **Domů** nebo **zpět** klíče, nastavení hello phone nečinnosti nebo přechod do jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="18b77-155">You can end an activity by pressing hello **Home** or **Back** key, by setting hello phone idle or by jumping into another application.</span></span> <span data-ttu-id="18b77-156">Ve výchozím nastavení je relace skončila deseti sekund po skončení hello jeho poslední aktivita.</span><span class="sxs-lookup"><span data-stu-id="18b77-156">By default, a session is ended ten seconds after hello end of its last activity.</span></span> <span data-ttu-id="18b77-157">Tím je zabráněno rozdělení relace každý uživatel hello čas ukončení a rychle, vrátí toohello aplikace, které může dojít při hello uživatele převezme bitovou kopii, zkontroluje, oznámení atd. Můžete chtít toomodify tento parametr.</span><span class="sxs-lookup"><span data-stu-id="18b77-157">This avoids a session split each time hello user exits and returns toohello application quickly, which can happen when hello user picks up an image, checks a notification, etc. You may want toomodify this parameter.</span></span> <span data-ttu-id="18b77-158">toodo Ano, přidat tento kód mezi hello `<application>` a `</application>` značky:</span><span class="sxs-lookup"><span data-stu-id="18b77-158">toodo so, add this code between hello `<application>` and `</application>` tags:</span></span>

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a><span data-ttu-id="18b77-159">Zakázání zasílání zpráv protokolu</span><span class="sxs-lookup"><span data-stu-id="18b77-159">Disable log reporting</span></span>
### <a name="using-a-method-call"></a><span data-ttu-id="18b77-160">Pomocí volání metody</span><span class="sxs-lookup"><span data-stu-id="18b77-160">Using a method call</span></span>
<span data-ttu-id="18b77-161">Pokud chcete Engagement toostop odesílání protokolů, můžete volat:</span><span class="sxs-lookup"><span data-stu-id="18b77-161">If you want Engagement toostop sending logs, you can call:</span></span>

    EngagementAgent.getInstance(context).setEnabled(false);

<span data-ttu-id="18b77-162">Toto volání je trvalé: používá soubor sdílet předvolby.</span><span class="sxs-lookup"><span data-stu-id="18b77-162">This call is persistent: it uses a shared preferences file.</span></span>

<span data-ttu-id="18b77-163">Pokud je při volání této funkce aktivní zapojení, může trvat jednu minutu po dobu toostop služby hello.</span><span class="sxs-lookup"><span data-stu-id="18b77-163">If Engagement is active when you call this function, it may take one minute for hello service toostop.</span></span> <span data-ttu-id="18b77-164">Ale nespustí hello služby všechny hello příštím spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="18b77-164">However it won't launch hello service at all hello next time you launch hello application.</span></span>

<span data-ttu-id="18b77-165">Můžete povolit protokol reporting znovu voláním hello stejné fungovat s `true`.</span><span class="sxs-lookup"><span data-stu-id="18b77-165">You can enable log reporting again by calling hello same function with `true`.</span></span>

### <a name="integration-in-your-own-preferenceactivity"></a><span data-ttu-id="18b77-166">Integrace ve vašem vlastním`PreferenceActivity`</span><span class="sxs-lookup"><span data-stu-id="18b77-166">Integration in your own `PreferenceActivity`</span></span>
<span data-ttu-id="18b77-167">Namísto volání této funkce, můžete také integrovat toto nastavení přímo do existující `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="18b77-167">Instead of calling this function, you can also integrate this setting directly in your existing `PreferenceActivity`.</span></span>

<span data-ttu-id="18b77-168">Zapojení toouse vaše předvolby soubor (s požadovanou režim hello) můžete nakonfigurovat v hello `AndroidManifest.xml` soubor s `application meta-data`:</span><span class="sxs-lookup"><span data-stu-id="18b77-168">You can configure Engagement toouse your preferences file (with hello desired mode) in hello `AndroidManifest.xml` file with `application meta-data`:</span></span>

* <span data-ttu-id="18b77-169">Hello `engagement:agent:settings:name` klíč je použité toodefine hello název souboru sdílet předvolby hello.</span><span class="sxs-lookup"><span data-stu-id="18b77-169">hello `engagement:agent:settings:name` key is used toodefine hello name of hello shared preferences file.</span></span>
* <span data-ttu-id="18b77-170">Hello `engagement:agent:settings:mode` klíč je použité toodefine hello režim souboru sdílet předvolby hello.</span><span class="sxs-lookup"><span data-stu-id="18b77-170">hello `engagement:agent:settings:mode` key is used toodefine hello mode of hello shared preferences file.</span></span> <span data-ttu-id="18b77-171">Použití hello stejné jako v režimu vaše `PreferenceActivity`.</span><span class="sxs-lookup"><span data-stu-id="18b77-171">Use hello same mode as in your `PreferenceActivity`.</span></span> <span data-ttu-id="18b77-172">Hello režimu, musí být předán jako číslo: Pokud používáte kombinaci konstantní příznaky ve vašem kódu, zkontrolujte hello celkové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="18b77-172">hello mode must be passed as a number: if you are using a combination of constant flags in your code, check hello total value.</span></span>

<span data-ttu-id="18b77-173">Zapojení vždy používá hello `engagement:key` boolean klíč v souboru předvoleb hello ke správě tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="18b77-173">Engagement always uses hello `engagement:key` boolean key within hello preferences file for managing this setting.</span></span>

<span data-ttu-id="18b77-174">Následující příklad Hello `AndroidManifest.xml` ukazuje hello výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="18b77-174">hello following example of `AndroidManifest.xml` shows hello default values:</span></span>

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

<span data-ttu-id="18b77-175">Potom můžete přidat `CheckBoxPreference` v vaše předvolby rozložení jako hello jeden následující:</span><span class="sxs-lookup"><span data-stu-id="18b77-175">Then you can add a `CheckBoxPreference` in your preference layout like hello following one:</span></span>

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
