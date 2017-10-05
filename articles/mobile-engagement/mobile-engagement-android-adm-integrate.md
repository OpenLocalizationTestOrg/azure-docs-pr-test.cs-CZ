---
title: Integraci sady Azure Mobile Engagement Android SDK
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a7d719ec-67b3-4be3-9d7f-0b61a57fe978
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 43987962ea2b7b825b88643d18b4db65f1f1670e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-adm-with-engagement"></a><span data-ttu-id="081eb-103">Postup pro integraci ADM zapojení</span><span class="sxs-lookup"><span data-stu-id="081eb-103">How to Integrate ADM with Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="081eb-104">Postupujte podle integrace postup popsaný v tom, jak integrovat Engagement Android dokumentu před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="081eb-104">You must follow the integration procedure described in the How to Integrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="081eb-105">Tento dokument je užitečný jenom v případě, že jste již integrované modul kampaně Reach a plán tak, aby nabízel Amazon zařízení.</span><span class="sxs-lookup"><span data-stu-id="081eb-105">This document is useful only if you already integrated the Reach module and plan to push Amazon devices.</span></span> <span data-ttu-id="081eb-106">Chcete-li integrovat kampaně Reach ve vaší aplikaci, přečtěte si nejprve postupy integrovat Engagement Reach v systému Android.</span><span class="sxs-lookup"><span data-stu-id="081eb-106">To integrate Reach campaigns in your application, please read first How to Integrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="081eb-107">Úvod</span><span class="sxs-lookup"><span data-stu-id="081eb-107">Introduction</span></span>
<span data-ttu-id="081eb-108">Integrace ADM umožňuje poslat při cílení na zařízení se systémem Android Amazon vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="081eb-108">Integrating ADM allows your application to be pushed when targeting Amazon Android devices.</span></span>

<span data-ttu-id="081eb-109">Datové části ADM nabídnutých do sady SDK vždy obsahovat `azme` klíče v datový objekt.</span><span class="sxs-lookup"><span data-stu-id="081eb-109">ADM payloads pushed to the SDK always contain the `azme` key in the data object.</span></span> <span data-ttu-id="081eb-110">Proto pokud používáte ADM pro jiný účel ve vaší aplikaci, můžete filtrovat podle klíči nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="081eb-110">Thus if you use ADM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="081eb-111">Pouze Amazon Kindle zařízení se systémem Android 4.0.3 nebo vyšší jsou podporovány Amazon Device Messaging; Můžete však integrovat tento kód bezpečně na jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="081eb-111">Only Amazon Kindle devices running Android 4.0.3 or above are supported by Amazon Device Messaging; however, you can integrate this code safely on other devices.</span></span>
> 
> 

## <a name="sign-up-to-adm"></a><span data-ttu-id="081eb-112">Zaregistrujte si ADM</span><span class="sxs-lookup"><span data-stu-id="081eb-112">Sign up to ADM</span></span>
<span data-ttu-id="081eb-113">Pokud dosud neučinili, musíte povolit ADM na vašem účtu Amazon.</span><span class="sxs-lookup"><span data-stu-id="081eb-113">If not already done, you must enable ADM on your Amazon account.</span></span>

<span data-ttu-id="081eb-114">Postup je podrobně popsán v: [ <https://developer.amazon.com/sdk/adm/credentials.html>].</span><span class="sxs-lookup"><span data-stu-id="081eb-114">The procedure is detailed at: [<https://developer.amazon.com/sdk/adm/credentials.html>].</span></span>

<span data-ttu-id="081eb-115">Po dokončení procesu, můžete získat:</span><span class="sxs-lookup"><span data-stu-id="081eb-115">Upon completing the procedure, you get:</span></span>

* <span data-ttu-id="081eb-116">Přihlašovací údaje OAuth (ID klienta a tajný klíč klienta) pro zapojení moct push zařízení.</span><span class="sxs-lookup"><span data-stu-id="081eb-116">OAuth credentials (a Client ID and a Client Secret) for Engagement to be able to push your devices.</span></span>
* <span data-ttu-id="081eb-117">Klíč rozhraní API, která musí být integrovaná do aplikace.</span><span class="sxs-lookup"><span data-stu-id="081eb-117">An API Key that must be integrated into your application.</span></span>

## <a name="sdk-integration"></a><span data-ttu-id="081eb-118">Integrace sady SDK</span><span class="sxs-lookup"><span data-stu-id="081eb-118">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="081eb-119">Správa registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="081eb-119">Managing device registrations</span></span>
<span data-ttu-id="081eb-120">Každé zařízení musí odeslat příkaz registrace k serverům ADM, jinak nelze získat přístup.</span><span class="sxs-lookup"><span data-stu-id="081eb-120">Each device must send a registration command to the ADM servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="081eb-121">Pokud už používáte [klientské knihovny ADM]a už máte [integrované ADM] můžete přímo přejít na android-sdk-adm přijímat.</span><span class="sxs-lookup"><span data-stu-id="081eb-121">If you already use the [ADM client library], and already have [integrated ADM] you can directly go to android-sdk-adm-receive.</span></span>

<span data-ttu-id="081eb-122">Pokud ještě nebyly spojili ADM, má Engagement jednodušší způsob, jak povolit ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="081eb-122">If you have not integrated ADM yet, Engagement has a simpler way to enable it in your application:</span></span>

<span data-ttu-id="081eb-123">Upravit vaše `AndroidManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="081eb-123">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="081eb-124">Přidat obor názvů Amazon soubor by měl začínat takto:</span><span class="sxs-lookup"><span data-stu-id="081eb-124">Add the Amazon namespace, the file should begin like this:</span></span>
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* <span data-ttu-id="081eb-125">Uvnitř `<application/>` značky, přidejte v této části:</span><span class="sxs-lookup"><span data-stu-id="081eb-125">Inside the `<application/>` tag, add this section:</span></span>
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* <span data-ttu-id="081eb-126">Po přidání značka amazon, může mít chyby sestavení, pokud je váš cíl sestavení projektu menší než Android 2.1.</span><span class="sxs-lookup"><span data-stu-id="081eb-126">After adding the amazon tag, you may have a build error if your Project Build Target is below Android 2.1.</span></span> <span data-ttu-id="081eb-127">Budete muset použít **Android 2.1 +** sestavení cíl (nemusíte si dělat starosti, můžete mít dál `minSdkVersion` nastavena na 4).</span><span class="sxs-lookup"><span data-stu-id="081eb-127">You have to use an **Android 2.1+** build target (don't worry, you can still have a `minSdkVersion` set to 4).</span></span>
* <span data-ttu-id="081eb-128">Integrovat klíč rozhraní API ADM jako prostředek podle [tento postup].</span><span class="sxs-lookup"><span data-stu-id="081eb-128">Integrate the ADM API Key as an asset by following [this procedure].</span></span>

<span data-ttu-id="081eb-129">Potom postupujte podle pokynů v dalších oddílech.</span><span class="sxs-lookup"><span data-stu-id="081eb-129">Then follow the instructions of the next sections.</span></span>

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="081eb-130">Id registrace ke službě Engagement Push komunikovat a přijímat oznámení</span><span class="sxs-lookup"><span data-stu-id="081eb-130">Communicate registration id to the Engagement Push service and receive notifications</span></span>
<span data-ttu-id="081eb-131">Abyste mohli komunikovat id registrace zařízení ke službě Engagement Push a jeho oznámení dostávat, přidejte následující příkaz pro vaše `AndroidManifest.xml` souboru uvnitř `<application/>` značky (i když používáte ADM bez zapojení):</span><span class="sxs-lookup"><span data-stu-id="081eb-131">In order to communicate the registration id of the device to the Engagement Push service and receive its notifications, add the following to your `AndroidManifest.xml` file, inside the `<application/>` tag (even if you use ADM without Engagement):</span></span>

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

<span data-ttu-id="081eb-132">Ujistěte se, máte následující oprávnění ve vaší `AndroidManifest.xml` (před `</application>` značka).</span><span class="sxs-lookup"><span data-stu-id="081eb-132">Ensure you have the following permissions in your `AndroidManifest.xml` (before the `</application>` tag).</span></span>

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a><span data-ttu-id="081eb-133">Přihlašovací údaje OAuth grant zapojení</span><span class="sxs-lookup"><span data-stu-id="081eb-133">Grant Engagement OAuth credentials</span></span>
<span data-ttu-id="081eb-134">Odešlete vaše přihlašovací údaje OAuth (ID klienta a tajný klíč klienta) na portálu Engagement.</span><span class="sxs-lookup"><span data-stu-id="081eb-134">Submit your OAuth Credentials (Client ID and Client Secret) in Engagement Portal.</span></span>

<span data-ttu-id="081eb-135">[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html</span><span class="sxs-lookup"><span data-stu-id="081eb-135">[<https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html</span></span>
<span data-ttu-id="081eb-136">[klientské knihovny ADM]:https://developer.amazon.com/sdk/adm/setup.html</span><span class="sxs-lookup"><span data-stu-id="081eb-136">[ADM client library]:https://developer.amazon.com/sdk/adm/setup.html</span></span>
<span data-ttu-id="081eb-137">[integrované ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html</span><span class="sxs-lookup"><span data-stu-id="081eb-137">[integrated ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html</span></span>
<span data-ttu-id="081eb-138">[tento postup]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset</span><span class="sxs-lookup"><span data-stu-id="081eb-138">[this procedure]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset</span></span>
