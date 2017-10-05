---
title: Integraci sady Azure Mobile Engagement Android SDK
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
ms.openlocfilehash: 0282abbf44406cac89c13520bc2a4e375817ed1f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-gcm-with-mobile-engagement"></a><span data-ttu-id="2a128-103">Postup pro integraci služby GCM Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="2a128-103">How to Integrate GCM with Mobile Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2a128-104">Postupujte podle integrace postup popsaný v tom, jak integrovat Engagement Android dokumentu před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="2a128-104">You must follow the integration procedure described in the How to Integrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="2a128-105">Tento dokument je užitečný jenom v případě, že jste již integrované modul kampaně Reach a plán tak, aby nabízel zařízení Google Play.</span><span class="sxs-lookup"><span data-stu-id="2a128-105">This document is useful only if you already integrated the Reach module and plan to push Google Play devices.</span></span> <span data-ttu-id="2a128-106">Chcete-li integrovat kampaně Reach ve vaší aplikaci, přečtěte si nejprve postupy integrovat Engagement Reach v systému Android.</span><span class="sxs-lookup"><span data-stu-id="2a128-106">To integrate Reach campaigns in your application, please read first How to Integrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="2a128-107">Úvod</span><span class="sxs-lookup"><span data-stu-id="2a128-107">Introduction</span></span>
<span data-ttu-id="2a128-108">Integrace služby GCM umožňuje poslat vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a128-108">Integrating GCM allows your application to be pushed.</span></span>

<span data-ttu-id="2a128-109">Datové části GCM nabídnutých do sady SDK vždy obsahovat `azme` klíče v datový objekt.</span><span class="sxs-lookup"><span data-stu-id="2a128-109">GCM payloads pushed to the SDK always contain the `azme` key in the data object.</span></span> <span data-ttu-id="2a128-110">Proto pokud používáte GCM pro jiný účel ve vaší aplikaci, můžete filtrovat podle klíči nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="2a128-110">Thus if you use GCM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a128-111">Jenom zařízení se systémem Android 2.2 nebo vyšší, s Google Play nainstalovaná a má Google pozadí připojení povoleno mohou poslat pomocí služby GCM; Můžete však integrovat tento kód bezpečně na nepodporované zařízení (používá právě záměry).</span><span class="sxs-lookup"><span data-stu-id="2a128-111">Only devices running Android 2.2 or above, having Google Play installed and having Google background connection enabled can be pushed using GCM; however, you can integrate this code safely on unsupported devices (it just uses intents).</span></span>
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a><span data-ttu-id="2a128-112">Vytvoření projektu služby GCM (Google Cloud Messaging) s klíčem rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2a128-112">Create a Google Cloud Messaging project with API key</span></span>
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a><span data-ttu-id="2a128-113">Integrace sady SDK</span><span class="sxs-lookup"><span data-stu-id="2a128-113">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="2a128-114">Správa registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="2a128-114">Managing device registrations</span></span>
<span data-ttu-id="2a128-115">Každé zařízení musí odeslat příkaz registrace na servery Googlu, jinak nelze získat přístup.</span><span class="sxs-lookup"><span data-stu-id="2a128-115">Each device must send a registration command to the Google servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="2a128-116">Zařízení můžete také zrušit registraci oznámení GCM (zařízení je automaticky registrace, pokud odinstalaci aplikace).</span><span class="sxs-lookup"><span data-stu-id="2a128-116">A device can also unregister from GCM notifications (the device is automatically unregistered if the application is uninstalled).</span></span>

<span data-ttu-id="2a128-117">Pokud nepoužijete [Google Play SDK] nebo jste již Neodesílat záměr registrace sami, můžete provést Engagement registraci zařízení automaticky za vás.</span><span class="sxs-lookup"><span data-stu-id="2a128-117">If you don't use [Google Play SDK] or you don't already send the registration intent yourself, you can make Engagement register the device automatically for you.</span></span>

<span data-ttu-id="2a128-118">Chcete-li povolit, přidejte následující příkaz pro vaše `AndroidManifest.xml` souboru uvnitř `<application/>` značky:</span><span class="sxs-lookup"><span data-stu-id="2a128-118">To enable this, add the following to your `AndroidManifest.xml` file, inside the `<application/>` tag:</span></span>

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="2a128-119">Id registrace ke službě Engagement Push komunikovat a přijímat oznámení</span><span class="sxs-lookup"><span data-stu-id="2a128-119">Communicate registration id to the Engagement Push service and receive notifications</span></span>
<span data-ttu-id="2a128-120">Abyste mohli komunikovat id registrace zařízení ke službě Engagement Push a jeho oznámení dostávat, přidejte následující příkaz pro vaše `AndroidManifest.xml` souboru uvnitř `<application/>` značky (i v případě registrace zařízení spravujete sami):</span><span class="sxs-lookup"><span data-stu-id="2a128-120">In order to communicate the registration id of the device to the Engagement Push service and receive its notifications, add the following to your `AndroidManifest.xml` file, inside the `<application/>` tag (even if you manage device registrations yourself):</span></span>

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

<span data-ttu-id="2a128-121">Ujistěte se, máte následující oprávnění ve vaší `AndroidManifest.xml` (po `</application>` značka).</span><span class="sxs-lookup"><span data-stu-id="2a128-121">Ensure you have the following permissions in your `AndroidManifest.xml` (after the `</application>` tag).</span></span>

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a><span data-ttu-id="2a128-122">Udělení přístupu aplikaci Mobile Engagement k vašemu klíči rozhraní API GCM</span><span class="sxs-lookup"><span data-stu-id="2a128-122">Grant Mobile Engagement access to your GCM API Key</span></span>
<span data-ttu-id="2a128-123">Postupujte podle [Tato příručka](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) k udělení přístupu Mobile Engagement k vašemu klíči rozhraní API GCM.</span><span class="sxs-lookup"><span data-stu-id="2a128-123">Follow [this guide](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) to grant Mobile Engagement access to your GCM API Key.</span></span>

<span data-ttu-id="2a128-124">[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start</span><span class="sxs-lookup"><span data-stu-id="2a128-124">[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start</span></span>
