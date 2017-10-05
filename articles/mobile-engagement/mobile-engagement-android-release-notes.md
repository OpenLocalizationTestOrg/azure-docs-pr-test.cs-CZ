---
title: Integraci sady Azure Mobile Engagement Android SDK
description: "Nejnovější aktualizace a postupy pro Android SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: c179c39a43da0aa35e945acceacbf27fe8e328f3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="release-notes"></a><span data-ttu-id="aaff1-103">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="aaff1-103">Release notes</span></span>

## <a name="431-07172017"></a><span data-ttu-id="aaff1-104">4.3.1 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="aaff1-104">4.3.1 (07/17/2017)</span></span>
* <span data-ttu-id="aaff1-105">Opravte havárii, která se může občas nastat při volání metody `EngagementAgentUtils.isInDedicatedEngagementProcess`, které používá také `EngagementApplication` třídy.</span><span class="sxs-lookup"><span data-stu-id="aaff1-105">Fix a crash that could rarely happen when calling `EngagementAgentUtils.isInDedicatedEngagementProcess`, which is also used by the `EngagementApplication` class.</span></span>

## <a name="430-06272017"></a><span data-ttu-id="aaff1-106">4.3.0 (06/27/2017)</span><span class="sxs-lookup"><span data-stu-id="aaff1-106">4.3.0 (06/27/2017)</span></span>
* <span data-ttu-id="aaff1-107">8 podpora pro Android (předchozí verze sady SDK nebudou fungovat na Android 8).</span><span class="sxs-lookup"><span data-stu-id="aaff1-107">Android 8 support (previous versions of the SDK will not work on Android 8).</span></span>
* <span data-ttu-id="aaff1-108">Žádné další závislosti na podporu knihovny.</span><span class="sxs-lookup"><span data-stu-id="aaff1-108">No more dependency on support library.</span></span>
* <span data-ttu-id="aaff1-109">Odebrat `EngagementFragmentActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="aaff1-109">Remove `EngagementFragmentActivity` class.</span></span>
* <span data-ttu-id="aaff1-110">Z důvodu [omezení provádění pozadí](https://developer.android.com/preview/features/background.html) na Android 8 může zpoždění protokoly v pozadí, dokud uživatel pracuje se zařízení, to bude mít dopad na kampaň nabízených **doručené** a  **Systémové oznámení zobrazené** statistiky je zpožděno. Pokud je zařízení v režimu spánku (oznámení bude i nadále zobrazovat, bude prstence a zavibrovat v reálném čase bez problémů).</span><span class="sxs-lookup"><span data-stu-id="aaff1-110">Due to [Background Execution Limits](https://developer.android.com/preview/features/background.html) on Android 8, logs in background might be delayed until the user interacts with the device, this will have an impact on Push Campaign **Delivered** and **System notification displayed** statistics being delayed if the device was sleeping (the notification will still be displayed, will ring and vibrate in real time without issues).</span></span>
* <span data-ttu-id="aaff1-111">Z důvodu [omezení umístění pozadí](https://developer.android.com/preview/features/background-location-limits.html), reálném čase na Android 8 nebude často aktualizovat umístění pozadí.</span><span class="sxs-lookup"><span data-stu-id="aaff1-111">Due to [Background Location Limits](https://developer.android.com/preview/features/background-location-limits.html), the real time location in background will not be updated frequently on Android 8.</span></span>

## <a name="424-03302017"></a><span data-ttu-id="aaff1-112">4.2.4 (03/30/2017)</span><span class="sxs-lookup"><span data-stu-id="aaff1-112">4.2.4 (03/30/2017)</span></span>
* <span data-ttu-id="aaff1-113">Vyřešte oznámení v aplikaci barvy textu na Android 7, aby byla stejná jako starší verze Android.</span><span class="sxs-lookup"><span data-stu-id="aaff1-113">Fix in-app notification text colors on Android 7 to be the same as older Android versions.</span></span>

## <a name="423-08102016"></a><span data-ttu-id="aaff1-114">4.2.3 (08/10/2016)</span><span class="sxs-lookup"><span data-stu-id="aaff1-114">4.2.3 (08/10/2016)</span></span>
* <span data-ttu-id="aaff1-115">Žádné další zámek Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="aaff1-115">No more WIFI lock.</span></span>
* <span data-ttu-id="aaff1-116">Opravte zablokování při volání metody getDeviceId před init (počínaje 4.2.0 chyb).</span><span class="sxs-lookup"><span data-stu-id="aaff1-116">Fix a deadlock when calling getDeviceId before init (bug introduced in 4.2.0).</span></span>

## <a name="422-05172016"></a><span data-ttu-id="aaff1-117">4.2.2 (05/17/2016)</span><span class="sxs-lookup"><span data-stu-id="aaff1-117">4.2.2 (05/17/2016)</span></span>
* <span data-ttu-id="aaff1-118">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="aaff1-118">Stability improvements.</span></span>

## <a name="421-05102016"></a><span data-ttu-id="aaff1-119">4.2.1 (05/10/2016)</span><span class="sxs-lookup"><span data-stu-id="aaff1-119">4.2.1 (05/10/2016)</span></span>
* <span data-ttu-id="aaff1-120">Zabezpečení: zakážete webové zobrazení místního souboru přístup.</span><span class="sxs-lookup"><span data-stu-id="aaff1-120">Security: disable web view local file access.</span></span>
* <span data-ttu-id="aaff1-121">Zabezpečení: odeberte `EngagementPreferenceActivity` třídu, která rozšiřuje zastaralé a nezabezpečená `PreferenceActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="aaff1-121">Security: remove `EngagementPreferenceActivity` class that extends obsolete and unsecure `PreferenceActivity` class.</span></span>
* <span data-ttu-id="aaff1-122">Zabezpečení: reach aktivity jsou nyní zdokumentované používat `exported="false"`, tento příznak lze také v předchozích verzích sady SDK.</span><span class="sxs-lookup"><span data-stu-id="aaff1-122">Security: reach activities are now documented to use `exported="false"`, this flag can also be used in previous SDK versions.</span></span>

## <a name="420-03112016"></a><span data-ttu-id="aaff1-123">4.2.0 (03/11/2016)</span><span class="sxs-lookup"><span data-stu-id="aaff1-123">4.2.0 (03/11/2016)</span></span>
* <span data-ttu-id="aaff1-124">Sada SDK je nyní licencováno v rámci MIT.</span><span class="sxs-lookup"><span data-stu-id="aaff1-124">The SDK is now licensed under MIT.</span></span>
* <span data-ttu-id="aaff1-125">Povolí použití identifikátoru vlastní zařízení během inicializace SDK.</span><span class="sxs-lookup"><span data-stu-id="aaff1-125">Allow specifying a custom device identifier at SDK initialization time.</span></span>

## <a name="415-02012016"></a><span data-ttu-id="aaff1-126">4.1.5 (02/01/2016)</span><span class="sxs-lookup"><span data-stu-id="aaff1-126">4.1.5 (02/01/2016)</span></span>
* <span data-ttu-id="aaff1-127">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="aaff1-127">Stability improvements.</span></span>

## <a name="414-01262016"></a><span data-ttu-id="aaff1-128">4.1.4 (01/26/2016)</span><span class="sxs-lookup"><span data-stu-id="aaff1-128">4.1.4 (01/26/2016)</span></span>
* <span data-ttu-id="aaff1-129">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="aaff1-129">Stability improvements.</span></span>

## <a name="413-1292015"></a><span data-ttu-id="aaff1-130">4.1.3 (12/9/2015)</span><span class="sxs-lookup"><span data-stu-id="aaff1-130">4.1.3 (12/9/2015)</span></span>
* <span data-ttu-id="aaff1-131">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="aaff1-131">Stability improvements.</span></span>

## <a name="412-11252015"></a><span data-ttu-id="aaff1-132">4.1.2 (11/25/2015)</span><span class="sxs-lookup"><span data-stu-id="aaff1-132">4.1.2 (11/25/2015)</span></span>
* <span data-ttu-id="aaff1-133">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="aaff1-133">Stability improvements.</span></span>

## <a name="411-11042015"></a><span data-ttu-id="aaff1-134">4.1.1 (11/04/2015)</span><span class="sxs-lookup"><span data-stu-id="aaff1-134">4.1.1 (11/04/2015)</span></span>
* <span data-ttu-id="aaff1-135">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="aaff1-135">Stability improvements.</span></span>

## <a name="410-08252015"></a><span data-ttu-id="aaff1-136">4.1.0 (08/25/2015)</span><span class="sxs-lookup"><span data-stu-id="aaff1-136">4.1.0 (08/25/2015)</span></span>
* <span data-ttu-id="aaff1-137">Zpracujte nový model oprávnění pro Android M.</span><span class="sxs-lookup"><span data-stu-id="aaff1-137">Handle new permission model for Android M.</span></span>
* <span data-ttu-id="aaff1-138">Můžete teď nakonfigurovat funkce umístění za běhu místo použití `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="aaff1-138">Can now configure location features at runtime instead of using  `AndroidManifest.xml`.</span></span>
* <span data-ttu-id="aaff1-139">Oprava chyby oprávnění: Pokud používáte `ACCESS_FINE_LOCATION`, pak `ACCESS_COARSE_LOCATION` už není potřeba.</span><span class="sxs-lookup"><span data-stu-id="aaff1-139">Fix a permission bug: if you use `ACCESS_FINE_LOCATION`, then `ACCESS_COARSE_LOCATION` is not needed anymore.</span></span>
* <span data-ttu-id="aaff1-140">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="aaff1-140">Stability improvements.</span></span>

## <a name="400-07062015"></a><span data-ttu-id="aaff1-141">4.0.0 (07/06/2015)</span><span class="sxs-lookup"><span data-stu-id="aaff1-141">4.0.0 (07/06/2015)</span></span>
* <span data-ttu-id="aaff1-142">Provedení analytickými funkcemi a nabízenými spolehlivější interní protokol změn.</span><span class="sxs-lookup"><span data-stu-id="aaff1-142">Internal protocol changes to make analytics and push more reliable.</span></span>
* <span data-ttu-id="aaff1-143">Nativního nabízení (GCM/ADM) se teď také používá pro oznámení v aplikaci, musíte nakonfigurovat přihlašovací údaje nativního nabízení pro jakýkoli typ nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="aaff1-143">Native push (GCM/ADM) is now also used for in app notifications so you must configure the native push credentials for any type of push campaign.</span></span>
* <span data-ttu-id="aaff1-144">Opravte velký obrázek oznámení: byly zobrazené pouze 10s po se instaluje.</span><span class="sxs-lookup"><span data-stu-id="aaff1-144">Fix big picture notification: they were displayed only 10s after being pushed.</span></span>
* <span data-ttu-id="aaff1-145">Oprava chyby ve webovém zobrazení: Kliknutím na odkaz se také provádí výchozí adresa URL akce.</span><span class="sxs-lookup"><span data-stu-id="aaff1-145">Fix a bug in web view: clicking on a link was also executing the default action URL.</span></span>
* <span data-ttu-id="aaff1-146">Opravte výjimečných havárie související se správou místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="aaff1-146">Fix a rare crash related to local storage management.</span></span>
* <span data-ttu-id="aaff1-147">Opravte dynamické konfigurace řetězec správy.</span><span class="sxs-lookup"><span data-stu-id="aaff1-147">Fix dynamic configuration string management.</span></span>
* <span data-ttu-id="aaff1-148">Aktualizujte smlouvy EULA.</span><span class="sxs-lookup"><span data-stu-id="aaff1-148">Update EULA.</span></span>

## <a name="300-02172015"></a><span data-ttu-id="aaff1-149">3.0.0 (02/17/2015)</span><span class="sxs-lookup"><span data-stu-id="aaff1-149">3.0.0 (02/17/2015)</span></span>
* <span data-ttu-id="aaff1-150">Počáteční verzi Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="aaff1-150">Initial Release of Azure Mobile Engagement</span></span>
* <span data-ttu-id="aaff1-151">Konfigurace appId je nahrazena konfiguraci řetězec připojení.</span><span class="sxs-lookup"><span data-stu-id="aaff1-151">appId configuration is replaced by a connection string configuration.</span></span>
* <span data-ttu-id="aaff1-152">Odebrat rozhraní API odesílat a přijímat libovolné protokolu XMPP zprávy z libovolné protokolu XMPP entit.</span><span class="sxs-lookup"><span data-stu-id="aaff1-152">Removed API to send and receive arbitrary XMPP messages from arbitrary XMPP entities.</span></span>
* <span data-ttu-id="aaff1-153">Odebrat rozhraní API pro odesílání a přijímání zpráv mezi zařízeními.</span><span class="sxs-lookup"><span data-stu-id="aaff1-153">Removed API to send and receive messages between devices.</span></span>
* <span data-ttu-id="aaff1-154">Vylepšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aaff1-154">Security improvements.</span></span>
* <span data-ttu-id="aaff1-155">Sledování Google Play a SmartAd odebrat.</span><span class="sxs-lookup"><span data-stu-id="aaff1-155">Google Play and SmartAd tracking removed.</span></span>

