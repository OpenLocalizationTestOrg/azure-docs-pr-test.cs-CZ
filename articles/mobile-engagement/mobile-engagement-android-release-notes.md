---
title: aaaAzure integraci sady Android SDK Mobile Engagement
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
ms.openlocfilehash: 16b098198674c49567d720d0c01d984cb763ed8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes"></a><span data-ttu-id="feadf-103">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="feadf-103">Release notes</span></span>

## <a name="431-07172017"></a><span data-ttu-id="feadf-104">4.3.1 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="feadf-104">4.3.1 (07/17/2017)</span></span>
* <span data-ttu-id="feadf-105">Opravte havárii, která se může občas nastat při volání metody `EngagementAgentUtils.isInDedicatedEngagementProcess`, které používá také hello `EngagementApplication` třídy.</span><span class="sxs-lookup"><span data-stu-id="feadf-105">Fix a crash that could rarely happen when calling `EngagementAgentUtils.isInDedicatedEngagementProcess`, which is also used by hello `EngagementApplication` class.</span></span>

## <a name="430-06272017"></a><span data-ttu-id="feadf-106">4.3.0 (06/27/2017)</span><span class="sxs-lookup"><span data-stu-id="feadf-106">4.3.0 (06/27/2017)</span></span>
* <span data-ttu-id="feadf-107">8 podpora pro Android (předchozí verze hello SDK nebude fungovat v systému Android 8).</span><span class="sxs-lookup"><span data-stu-id="feadf-107">Android 8 support (previous versions of hello SDK will not work on Android 8).</span></span>
* <span data-ttu-id="feadf-108">Žádné další závislosti na podporu knihovny.</span><span class="sxs-lookup"><span data-stu-id="feadf-108">No more dependency on support library.</span></span>
* <span data-ttu-id="feadf-109">Odebrat `EngagementFragmentActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="feadf-109">Remove `EngagementFragmentActivity` class.</span></span>
* <span data-ttu-id="feadf-110">Kvůli příliš[omezení provádění pozadí](https://developer.android.com/preview/features/background.html) na Android 8 může zpoždění protokoly v pozadí, dokud hello uživatel pracuje s hello zařízení, to bude mít dopad na kampaň nabízených **doručené** a **Systémové oznámení zobrazené** statistiky je zpožděno. Pokud byl hello zařízení v režimu spánku (hello oznámení bude i nadále zobrazovat, bude prstence a zavibrovat v reálném čase bez problémů).</span><span class="sxs-lookup"><span data-stu-id="feadf-110">Due too[Background Execution Limits](https://developer.android.com/preview/features/background.html) on Android 8, logs in background might be delayed until hello user interacts with hello device, this will have an impact on Push Campaign **Delivered** and **System notification displayed** statistics being delayed if hello device was sleeping (hello notification will still be displayed, will ring and vibrate in real time without issues).</span></span>
* <span data-ttu-id="feadf-111">Kvůli příliš[omezení umístění pozadí](https://developer.android.com/preview/features/background-location-limits.html), hello reálném čase umístění pozadí se nebude často aktualizovat na Android 8.</span><span class="sxs-lookup"><span data-stu-id="feadf-111">Due too[Background Location Limits](https://developer.android.com/preview/features/background-location-limits.html), hello real time location in background will not be updated frequently on Android 8.</span></span>

## <a name="424-03302017"></a><span data-ttu-id="feadf-112">4.2.4 (03/30/2017)</span><span class="sxs-lookup"><span data-stu-id="feadf-112">4.2.4 (03/30/2017)</span></span>
* <span data-ttu-id="feadf-113">Opravte barvy textu na Android 7 toobe hello stejné jako starší verze Android oznámení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="feadf-113">Fix in-app notification text colors on Android 7 toobe hello same as older Android versions.</span></span>

## <a name="423-08102016"></a><span data-ttu-id="feadf-114">4.2.3 (08/10/2016)</span><span class="sxs-lookup"><span data-stu-id="feadf-114">4.2.3 (08/10/2016)</span></span>
* <span data-ttu-id="feadf-115">Žádné další zámek Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="feadf-115">No more WIFI lock.</span></span>
* <span data-ttu-id="feadf-116">Opravte zablokování při volání metody getDeviceId před init (počínaje 4.2.0 chyb).</span><span class="sxs-lookup"><span data-stu-id="feadf-116">Fix a deadlock when calling getDeviceId before init (bug introduced in 4.2.0).</span></span>

## <a name="422-05172016"></a><span data-ttu-id="feadf-117">4.2.2 (05/17/2016)</span><span class="sxs-lookup"><span data-stu-id="feadf-117">4.2.2 (05/17/2016)</span></span>
* <span data-ttu-id="feadf-118">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="feadf-118">Stability improvements.</span></span>

## <a name="421-05102016"></a><span data-ttu-id="feadf-119">4.2.1 (05/10/2016)</span><span class="sxs-lookup"><span data-stu-id="feadf-119">4.2.1 (05/10/2016)</span></span>
* <span data-ttu-id="feadf-120">Zabezpečení: zakážete webové zobrazení místního souboru přístup.</span><span class="sxs-lookup"><span data-stu-id="feadf-120">Security: disable web view local file access.</span></span>
* <span data-ttu-id="feadf-121">Zabezpečení: odeberte `EngagementPreferenceActivity` třídu, která rozšiřuje zastaralé a nezabezpečená `PreferenceActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="feadf-121">Security: remove `EngagementPreferenceActivity` class that extends obsolete and unsecure `PreferenceActivity` class.</span></span>
* <span data-ttu-id="feadf-122">Zabezpečení: reach aktivity jsou nyní zdokumentované toouse `exported="false"`, tento příznak lze také v předchozích verzích sady SDK.</span><span class="sxs-lookup"><span data-stu-id="feadf-122">Security: reach activities are now documented toouse `exported="false"`, this flag can also be used in previous SDK versions.</span></span>

## <a name="420-03112016"></a><span data-ttu-id="feadf-123">4.2.0 (03/11/2016)</span><span class="sxs-lookup"><span data-stu-id="feadf-123">4.2.0 (03/11/2016)</span></span>
* <span data-ttu-id="feadf-124">Hello SDK je nyní licencováno v rámci MIT.</span><span class="sxs-lookup"><span data-stu-id="feadf-124">hello SDK is now licensed under MIT.</span></span>
* <span data-ttu-id="feadf-125">Povolí použití identifikátoru vlastní zařízení během inicializace SDK.</span><span class="sxs-lookup"><span data-stu-id="feadf-125">Allow specifying a custom device identifier at SDK initialization time.</span></span>

## <a name="415-02012016"></a><span data-ttu-id="feadf-126">4.1.5 (02/01/2016)</span><span class="sxs-lookup"><span data-stu-id="feadf-126">4.1.5 (02/01/2016)</span></span>
* <span data-ttu-id="feadf-127">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="feadf-127">Stability improvements.</span></span>

## <a name="414-01262016"></a><span data-ttu-id="feadf-128">4.1.4 (01/26/2016)</span><span class="sxs-lookup"><span data-stu-id="feadf-128">4.1.4 (01/26/2016)</span></span>
* <span data-ttu-id="feadf-129">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="feadf-129">Stability improvements.</span></span>

## <a name="413-1292015"></a><span data-ttu-id="feadf-130">4.1.3 (12/9/2015)</span><span class="sxs-lookup"><span data-stu-id="feadf-130">4.1.3 (12/9/2015)</span></span>
* <span data-ttu-id="feadf-131">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="feadf-131">Stability improvements.</span></span>

## <a name="412-11252015"></a><span data-ttu-id="feadf-132">4.1.2 (11/25/2015)</span><span class="sxs-lookup"><span data-stu-id="feadf-132">4.1.2 (11/25/2015)</span></span>
* <span data-ttu-id="feadf-133">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="feadf-133">Stability improvements.</span></span>

## <a name="411-11042015"></a><span data-ttu-id="feadf-134">4.1.1 (11/04/2015)</span><span class="sxs-lookup"><span data-stu-id="feadf-134">4.1.1 (11/04/2015)</span></span>
* <span data-ttu-id="feadf-135">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="feadf-135">Stability improvements.</span></span>

## <a name="410-08252015"></a><span data-ttu-id="feadf-136">4.1.0 (08/25/2015)</span><span class="sxs-lookup"><span data-stu-id="feadf-136">4.1.0 (08/25/2015)</span></span>
* <span data-ttu-id="feadf-137">Zpracujte nový model oprávnění pro Android M.</span><span class="sxs-lookup"><span data-stu-id="feadf-137">Handle new permission model for Android M.</span></span>
* <span data-ttu-id="feadf-138">Můžete teď nakonfigurovat funkce umístění za běhu místo použití `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="feadf-138">Can now configure location features at runtime instead of using  `AndroidManifest.xml`.</span></span>
* <span data-ttu-id="feadf-139">Oprava chyby oprávnění: Pokud používáte `ACCESS_FINE_LOCATION`, pak `ACCESS_COARSE_LOCATION` už není potřeba.</span><span class="sxs-lookup"><span data-stu-id="feadf-139">Fix a permission bug: if you use `ACCESS_FINE_LOCATION`, then `ACCESS_COARSE_LOCATION` is not needed anymore.</span></span>
* <span data-ttu-id="feadf-140">Zlepšení stability.</span><span class="sxs-lookup"><span data-stu-id="feadf-140">Stability improvements.</span></span>

## <a name="400-07062015"></a><span data-ttu-id="feadf-141">4.0.0 (07/06/2015)</span><span class="sxs-lookup"><span data-stu-id="feadf-141">4.0.0 (07/06/2015)</span></span>
* <span data-ttu-id="feadf-142">Vnitřní změní toomake analýzy a nabízených spolehlivější.</span><span class="sxs-lookup"><span data-stu-id="feadf-142">Internal protocol changes toomake analytics and push more reliable.</span></span>
* <span data-ttu-id="feadf-143">Nativního nabízení (GCM/ADM) se teď také používá pro oznámení v aplikaci, musíte nakonfigurovat přihlašovací údaje nativního nabízení hello pro jakýkoli typ nabízené kampaně.</span><span class="sxs-lookup"><span data-stu-id="feadf-143">Native push (GCM/ADM) is now also used for in app notifications so you must configure hello native push credentials for any type of push campaign.</span></span>
* <span data-ttu-id="feadf-144">Opravte velký obrázek oznámení: byly zobrazené pouze 10s po se instaluje.</span><span class="sxs-lookup"><span data-stu-id="feadf-144">Fix big picture notification: they were displayed only 10s after being pushed.</span></span>
* <span data-ttu-id="feadf-145">Oprava chyby ve webovém zobrazení: Kliknutím na odkaz se také provádí hello výchozí akce adresy URL.</span><span class="sxs-lookup"><span data-stu-id="feadf-145">Fix a bug in web view: clicking on a link was also executing hello default action URL.</span></span>
* <span data-ttu-id="feadf-146">Opravte výjimečných havárií související toolocal správu úložiště.</span><span class="sxs-lookup"><span data-stu-id="feadf-146">Fix a rare crash related toolocal storage management.</span></span>
* <span data-ttu-id="feadf-147">Opravte dynamické konfigurace řetězec správy.</span><span class="sxs-lookup"><span data-stu-id="feadf-147">Fix dynamic configuration string management.</span></span>
* <span data-ttu-id="feadf-148">Aktualizujte smlouvy EULA.</span><span class="sxs-lookup"><span data-stu-id="feadf-148">Update EULA.</span></span>

## <a name="300-02172015"></a><span data-ttu-id="feadf-149">3.0.0 (02/17/2015)</span><span class="sxs-lookup"><span data-stu-id="feadf-149">3.0.0 (02/17/2015)</span></span>
* <span data-ttu-id="feadf-150">Počáteční verzi Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="feadf-150">Initial Release of Azure Mobile Engagement</span></span>
* <span data-ttu-id="feadf-151">Konfigurace appId je nahrazena konfiguraci řetězec připojení.</span><span class="sxs-lookup"><span data-stu-id="feadf-151">appId configuration is replaced by a connection string configuration.</span></span>
* <span data-ttu-id="feadf-152">Odebrat toosend rozhraní API a přijímat libovolné protokolu XMPP zprávy z libovolné protokolu XMPP entit.</span><span class="sxs-lookup"><span data-stu-id="feadf-152">Removed API toosend and receive arbitrary XMPP messages from arbitrary XMPP entities.</span></span>
* <span data-ttu-id="feadf-153">Odebrat toosend rozhraní API a přijímat zprávy mezi zařízeními.</span><span class="sxs-lookup"><span data-stu-id="feadf-153">Removed API toosend and receive messages between devices.</span></span>
* <span data-ttu-id="feadf-154">Vylepšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="feadf-154">Security improvements.</span></span>
* <span data-ttu-id="feadf-155">Sledování Google Play a SmartAd odebrat.</span><span class="sxs-lookup"><span data-stu-id="feadf-155">Google Play and SmartAd tracking removed.</span></span>

