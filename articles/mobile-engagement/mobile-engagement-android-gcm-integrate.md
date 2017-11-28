---
title: aaaAzure integraci sady Android SDK Mobile Engagement
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d72b5014-a22b-4a7f-a470-d2b8145b5b86
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/10/2016
ms.author: piyushjo
ms.openlocfilehash: e81230cbc99a209f2909cc163c4e566df67dc828
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-gcm-with-mobile-engagement"></a><span data-ttu-id="289c3-103">Jak tooIntegrate GCM s Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="289c3-103">How tooIntegrate GCM with Mobile Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="289c3-104">Postupujte podle hello integrace postup popsaný v hello jak tooIntegrate Engagement v systému Android dokumentu před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="289c3-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="289c3-105">Tento dokument je užitečný jenom v případě, že jste již integrované hello dosáhnout modulu a plán toopush Google Play zařízení.</span><span class="sxs-lookup"><span data-stu-id="289c3-105">This document is useful only if you already integrated hello Reach module and plan toopush Google Play devices.</span></span> <span data-ttu-id="289c3-106">kampaně Reach toointegrate ve vaší aplikaci, přečtěte si nejprve jak tooIntegrate Engagement Reach v systému Android.</span><span class="sxs-lookup"><span data-stu-id="289c3-106">toointegrate Reach campaigns in your application, please read first How tooIntegrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="289c3-107">Úvod</span><span class="sxs-lookup"><span data-stu-id="289c3-107">Introduction</span></span>
<span data-ttu-id="289c3-108">Integrace služby GCM umožňuje vaší aplikace toobe poslat.</span><span class="sxs-lookup"><span data-stu-id="289c3-108">Integrating GCM allows your application toobe pushed.</span></span>

<span data-ttu-id="289c3-109">Datové části GCM nabídnutých toohello SDK vždy obsahovat hello `azme` klíče v hello datový objekt.</span><span class="sxs-lookup"><span data-stu-id="289c3-109">GCM payloads pushed toohello SDK always contain hello `azme` key in hello data object.</span></span> <span data-ttu-id="289c3-110">Proto pokud používáte GCM pro jiný účel ve vaší aplikaci, můžete filtrovat podle klíči nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="289c3-110">Thus if you use GCM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="289c3-111">Jenom zařízení se systémem Android 2.2 nebo vyšší, s Google Play nainstalovaná a má Google pozadí připojení povoleno mohou poslat pomocí služby GCM; Můžete však integrovat tento kód bezpečně na nepodporované zařízení (používá právě záměry).</span><span class="sxs-lookup"><span data-stu-id="289c3-111">Only devices running Android 2.2 or above, having Google Play installed and having Google background connection enabled can be pushed using GCM; however, you can integrate this code safely on unsupported devices (it just uses intents).</span></span>
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a><span data-ttu-id="289c3-112">Vytvoření projektu služby GCM (Google Cloud Messaging) s klíčem rozhraní API</span><span class="sxs-lookup"><span data-stu-id="289c3-112">Create a Google Cloud Messaging project with API key</span></span>
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a><span data-ttu-id="289c3-113">Integrace sady SDK</span><span class="sxs-lookup"><span data-stu-id="289c3-113">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="289c3-114">Správa registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="289c3-114">Managing device registrations</span></span>
<span data-ttu-id="289c3-115">Každé zařízení musí odeslat příkaz toohello registrace servery Googlu, jinak nelze získat přístup.</span><span class="sxs-lookup"><span data-stu-id="289c3-115">Each device must send a registration command toohello Google servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="289c3-116">Zařízení můžete také zrušit registraci oznámení GCM (hello zařízení se automaticky registrace, když se odinstaluje aplikace hello).</span><span class="sxs-lookup"><span data-stu-id="289c3-116">A device can also unregister from GCM notifications (hello device is automatically unregistered if hello application is uninstalled).</span></span>

<span data-ttu-id="289c3-117">Pokud nepoužijete [Google Play SDK] nebo jste již Neodesílat hello registrace záměr sami, můžete provést Engagement registrovat zařízení hello automaticky za vás.</span><span class="sxs-lookup"><span data-stu-id="289c3-117">If you don't use [Google Play SDK] or you don't already send hello registration intent yourself, you can make Engagement register hello device automatically for you.</span></span>

<span data-ttu-id="289c3-118">tooenable, přidejte následující tooyour hello `AndroidManifest.xml` souboru uvnitř hello `<application/>` značky:</span><span class="sxs-lookup"><span data-stu-id="289c3-118">tooenable this, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag:</span></span>

            <!-- If only 1 sender, don't forget hello \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="289c3-119">Komunikaci registrace id toohello Engagement nabízené služby a přijímat oznámení</span><span class="sxs-lookup"><span data-stu-id="289c3-119">Communicate registration id toohello Engagement Push service and receive notifications</span></span>
<span data-ttu-id="289c3-120">V pořadí toocommunicate hello id registrace toohello zařízení hello Engagement nabízené služby a jeho oznámení dostávat, přidat následující tooyour hello `AndroidManifest.xml` souboru uvnitř hello `<application/>` značky (i v případě registrace zařízení spravujete sami):</span><span class="sxs-lookup"><span data-stu-id="289c3-120">In order toocommunicate hello registration id of hello device toohello Engagement Push service and receive its notifications, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag (even if you manage device registrations yourself):</span></span>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

<span data-ttu-id="289c3-121">Ujistěte se, máte následující oprávnění v hello vaše `AndroidManifest.xml` (po hello `</application>` značka).</span><span class="sxs-lookup"><span data-stu-id="289c3-121">Ensure you have hello following permissions in your `AndroidManifest.xml` (after hello `</application>` tag).</span></span>

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a><span data-ttu-id="289c3-122">Mobile Engagement udělení přístupu tooyour klíč rozhraní API GCM</span><span class="sxs-lookup"><span data-stu-id="289c3-122">Grant Mobile Engagement access tooyour GCM API Key</span></span>
<span data-ttu-id="289c3-123">Postupujte podle [Tato příručka](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant Mobile Engagement přístup tooyour klíč rozhraní API GCM.</span><span class="sxs-lookup"><span data-stu-id="289c3-123">Follow [this guide](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant Mobile Engagement access tooyour GCM API Key.</span></span>

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
