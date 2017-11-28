---
title: aaaAzure integraci sady Android SDK Mobile Engagement
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
ms.openlocfilehash: c57132ff49cf8c335627a72b37f9b78529e84f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-adm-with-engagement"></a><span data-ttu-id="14b55-103">Jak tooIntegrate ADM s zapojení</span><span class="sxs-lookup"><span data-stu-id="14b55-103">How tooIntegrate ADM with Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="14b55-104">Postupujte podle hello integrace postup popsaný v hello jak tooIntegrate Engagement v systému Android dokumentu před těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="14b55-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="14b55-105">Tento dokument je užitečný jenom v případě, že již integrovanou hello Reach modulu a plán toopush Amazon zařízení.</span><span class="sxs-lookup"><span data-stu-id="14b55-105">This document is useful only if you already integrated hello Reach module and plan toopush Amazon devices.</span></span> <span data-ttu-id="14b55-106">kampaně Reach toointegrate ve vaší aplikaci, přečtěte si nejprve jak tooIntegrate Engagement Reach v systému Android.</span><span class="sxs-lookup"><span data-stu-id="14b55-106">toointegrate Reach campaigns in your application, please read first How tooIntegrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="14b55-107">Úvod</span><span class="sxs-lookup"><span data-stu-id="14b55-107">Introduction</span></span>
<span data-ttu-id="14b55-108">Integrace ADM umožňuje vaší aplikace toobe nabídnutých při cílení na zařízení se systémem Android Amazon.</span><span class="sxs-lookup"><span data-stu-id="14b55-108">Integrating ADM allows your application toobe pushed when targeting Amazon Android devices.</span></span>

<span data-ttu-id="14b55-109">Datové části ADM nabídnutých toohello SDK vždy obsahovat hello `azme` klíče v hello datový objekt.</span><span class="sxs-lookup"><span data-stu-id="14b55-109">ADM payloads pushed toohello SDK always contain hello `azme` key in hello data object.</span></span> <span data-ttu-id="14b55-110">Proto pokud používáte ADM pro jiný účel ve vaší aplikaci, můžete filtrovat podle klíči nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="14b55-110">Thus if you use ADM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="14b55-111">Pouze Amazon Kindle zařízení se systémem Android 4.0.3 nebo vyšší jsou podporovány Amazon Device Messaging; Můžete však integrovat tento kód bezpečně na jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="14b55-111">Only Amazon Kindle devices running Android 4.0.3 or above are supported by Amazon Device Messaging; however, you can integrate this code safely on other devices.</span></span>
> 
> 

## <a name="sign-up-tooadm"></a><span data-ttu-id="14b55-112">Zaregistrujte si tooADM</span><span class="sxs-lookup"><span data-stu-id="14b55-112">Sign up tooADM</span></span>
<span data-ttu-id="14b55-113">Pokud dosud neučinili, musíte povolit ADM na vašem účtu Amazon.</span><span class="sxs-lookup"><span data-stu-id="14b55-113">If not already done, you must enable ADM on your Amazon account.</span></span>

<span data-ttu-id="14b55-114">Postup Hello je podrobně popsán v: [ <https://developer.amazon.com/sdk/adm/credentials.html>].</span><span class="sxs-lookup"><span data-stu-id="14b55-114">hello procedure is detailed at: [<https://developer.amazon.com/sdk/adm/credentials.html>].</span></span>

<span data-ttu-id="14b55-115">Po dokončení postupu hello se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="14b55-115">Upon completing hello procedure, you get:</span></span>

* <span data-ttu-id="14b55-116">OAuth přihlašovací údaje (ID klienta a tajný klíč klienta) pro zapojení toobe možné toopush zařízení.</span><span class="sxs-lookup"><span data-stu-id="14b55-116">OAuth credentials (a Client ID and a Client Secret) for Engagement toobe able toopush your devices.</span></span>
* <span data-ttu-id="14b55-117">Klíč rozhraní API, která musí být integrovaná do aplikace.</span><span class="sxs-lookup"><span data-stu-id="14b55-117">An API Key that must be integrated into your application.</span></span>

## <a name="sdk-integration"></a><span data-ttu-id="14b55-118">Integrace sady SDK</span><span class="sxs-lookup"><span data-stu-id="14b55-118">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="14b55-119">Správa registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="14b55-119">Managing device registrations</span></span>
<span data-ttu-id="14b55-120">Každé zařízení musí odeslat příkaz toohello registrace ADM servery, v opačném případě nelze získat přístup.</span><span class="sxs-lookup"><span data-stu-id="14b55-120">Each device must send a registration command toohello ADM servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="14b55-121">Pokud už používáte hello [klientské knihovny ADM]a už máte [integrované ADM] můžete přímo přejít tooandroid-sdk-adm přijímat.</span><span class="sxs-lookup"><span data-stu-id="14b55-121">If you already use hello [ADM client library], and already have [integrated ADM] you can directly go tooandroid-sdk-adm-receive.</span></span>

<span data-ttu-id="14b55-122">Pokud nebyly integrované ADM ještě Engagement má jednodušší způsob tooenable ho v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="14b55-122">If you have not integrated ADM yet, Engagement has a simpler way tooenable it in your application:</span></span>

<span data-ttu-id="14b55-123">Upravit vaše `AndroidManifest.xml` souboru:</span><span class="sxs-lookup"><span data-stu-id="14b55-123">Edit your `AndroidManifest.xml` file:</span></span>

* <span data-ttu-id="14b55-124">Přidejte hello názvů Amazon, hello soubor by měl začínat takto:</span><span class="sxs-lookup"><span data-stu-id="14b55-124">Add hello Amazon namespace, hello file should begin like this:</span></span>
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* <span data-ttu-id="14b55-125">Uvnitř hello `<application/>` značky, přidejte v této části:</span><span class="sxs-lookup"><span data-stu-id="14b55-125">Inside hello `<application/>` tag, add this section:</span></span>
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* <span data-ttu-id="14b55-126">Po přidání značka hello amazon, může mít chyby sestavení, pokud je váš cíl sestavení projektu menší než Android 2.1.</span><span class="sxs-lookup"><span data-stu-id="14b55-126">After adding hello amazon tag, you may have a build error if your Project Build Target is below Android 2.1.</span></span> <span data-ttu-id="14b55-127">Máte toouse **Android 2.1 +** sestavení cíl (nemusíte si dělat starosti, můžete mít dál `minSdkVersion` nastavit too4).</span><span class="sxs-lookup"><span data-stu-id="14b55-127">You have toouse an **Android 2.1+** build target (don't worry, you can still have a `minSdkVersion` set too4).</span></span>
* <span data-ttu-id="14b55-128">Integrovat hello klíč rozhraní API ADM jako prostředek podle [tento postup].</span><span class="sxs-lookup"><span data-stu-id="14b55-128">Integrate hello ADM API Key as an asset by following [this procedure].</span></span>

<span data-ttu-id="14b55-129">Potom postupujte podle pokynů hello hello další části.</span><span class="sxs-lookup"><span data-stu-id="14b55-129">Then follow hello instructions of hello next sections.</span></span>

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="14b55-130">Komunikaci registrace id toohello Engagement nabízené služby a přijímat oznámení</span><span class="sxs-lookup"><span data-stu-id="14b55-130">Communicate registration id toohello Engagement Push service and receive notifications</span></span>
<span data-ttu-id="14b55-131">V pořadí toocommunicate hello id registrace toohello zařízení hello Engagement nabízené služby a jeho oznámení dostávat, přidat následující tooyour hello `AndroidManifest.xml` souboru uvnitř hello `<application/>` značky (i když používáte ADM bez zapojení):</span><span class="sxs-lookup"><span data-stu-id="14b55-131">In order toocommunicate hello registration id of hello device toohello Engagement Push service and receive its notifications, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag (even if you use ADM without Engagement):</span></span>

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

<span data-ttu-id="14b55-132">Ujistěte se, máte následující oprávnění v hello vaše `AndroidManifest.xml` (před hello `</application>` značka).</span><span class="sxs-lookup"><span data-stu-id="14b55-132">Ensure you have hello following permissions in your `AndroidManifest.xml` (before hello `</application>` tag).</span></span>

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a><span data-ttu-id="14b55-133">Přihlašovací údaje OAuth grant zapojení</span><span class="sxs-lookup"><span data-stu-id="14b55-133">Grant Engagement OAuth credentials</span></span>
<span data-ttu-id="14b55-134">Odešlete vaše přihlašovací údaje OAuth (ID klienta a tajný klíč klienta) na portálu Engagement.</span><span class="sxs-lookup"><span data-stu-id="14b55-134">Submit your OAuth Credentials (Client ID and Client Secret) in Engagement Portal.</span></span>

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[klientské knihovny ADM]:https://developer.amazon.com/sdk/adm/setup.html
[integrované ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[tento postup]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
