---
title: "aaaAzure Mobile Engagement iOS SDK poznámky k verzi | Microsoft Docs"
description: "Nejnovější aktualizace a postupy pro iOS SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: ae29d200ebb1784357b29edbd1f66b71df0778cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a><span data-ttu-id="e7bcf-103">Poznámky k verzi sady Azure Mobile Engagement iOS SDK</span><span class="sxs-lookup"><span data-stu-id="e7bcf-103">Azure Mobile Engagement iOS SDK release notes</span></span>

## <a name="410-07172017"></a><span data-ttu-id="e7bcf-104">4.1.0 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-104">4.1.0 (07/17/2017)</span></span>
* <span data-ttu-id="e7bcf-105">Odznaky s pevnou vymazat v pozadí.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-105">Fixed badges cleared in background.</span></span>
* <span data-ttu-id="e7bcf-106">Opravené upozornění na XCode 9 o volání rozhraní API není z hlavní fronty.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-106">Fixed warnings on XCode 9 about APIs not called in main queue.</span></span>
* <span data-ttu-id="e7bcf-107">Vyřešený nevracení paměti v hlasování Reach.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-107">Fixed a memory leak in Reach polls.</span></span>
* <span data-ttu-id="e7bcf-108">Podpora pro iOS vyřadit 6.X.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-108">Dropped support for iOS 6.X.</span></span> <span data-ttu-id="e7bcf-109">Od této verze hello nasazení cílové aplikace musí být minimálně iOS 7.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-109">Starting from this version hello deployment target of your application must be at least iOS 7.</span></span>

## <a name="401-12132016"></a><span data-ttu-id="e7bcf-110">4.0.1 (12/13/2016)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-110">4.0.1 (12/13/2016)</span></span>
* <span data-ttu-id="e7bcf-111">Vylepšení protokolu doručení v pozadí.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-111">Improved log delivery in background.</span></span>

## <a name="400-09122016"></a><span data-ttu-id="e7bcf-112">4.0.0 (09/12/2016)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-112">4.0.0 (09/12/2016)</span></span>
* <span data-ttu-id="e7bcf-113">Opravené oznámení není reakcí na zařízeních s iOS 10.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-113">Fixed notification not actioned on iOS 10 devices.</span></span>
* <span data-ttu-id="e7bcf-114">Přestat používat XCode 7.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-114">Deprecate XCode 7.</span></span>

## <a name="324-06302016"></a><span data-ttu-id="e7bcf-115">3.2.4 (06/30/2016)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-115">3.2.4 (06/30/2016)</span></span>
* <span data-ttu-id="e7bcf-116">Opravené agregace mezi technické protokoly a další protokoly.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-116">Fixed aggregation between technical logs and other logs.</span></span>

## <a name="323-06072016"></a><span data-ttu-id="e7bcf-117">3.2.3 (06/07/2016)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-117">3.2.3 (06/07/2016)</span></span>
* <span data-ttu-id="e7bcf-118">Chyby opravené hello kde zpětnou vazbu o doručení není hlášena, když je aplikace hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-118">Fixed hello bug where delivery feedback is not reported when app is in hello background.</span></span>
* <span data-ttu-id="e7bcf-119">Optimalizované hello odesílání technické protokolů.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-119">Optimized hello sending of technical logs.</span></span>

## <a name="322-04072016"></a><span data-ttu-id="e7bcf-120">3.2.2 (04/07/2016)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-120">3.2.2 (04/07/2016)</span></span>
* <span data-ttu-id="e7bcf-121">Opravené chyby na zrušení požadavku HTTP, což vede někdy toocrash.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-121">Fixed bug on HTTP request cancellation which sometimes leads toocrash.</span></span>

## <a name="321-12112015"></a><span data-ttu-id="e7bcf-122">3.2.1 (12/11/2015)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-122">3.2.1 (12/11/2015)</span></span>
* <span data-ttu-id="e7bcf-123">Opravené hello zpoždění při aktivaci novou instanci aplikace podle oznámení s přímých odkazů</span><span class="sxs-lookup"><span data-stu-id="e7bcf-123">Fixed hello delay when a new app instance is triggered by a notification with deep links</span></span>

## <a name="320-10082015"></a><span data-ttu-id="e7bcf-124">3.2.0 (10/08/2015)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-124">3.2.0 (10/08/2015)</span></span>
* <span data-ttu-id="e7bcf-125">Povolit Bitcode v toomake SDK hello se pracovat s **Xcode 7**.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-125">Enabled Bitcode in hello SDK toomake it work with **Xcode 7**.</span></span>
* <span data-ttu-id="e7bcf-126">Opravené chyby související s aplikací tooin oznámení.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-126">Fixed bugs related tooin-app notifications.</span></span>
* <span data-ttu-id="e7bcf-127">Oznámení v aplikaci hello provedené v případě nízký stav baterie. a dalších takových scénářů.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-127">Made hello in-app notifications more reliable in case of low battery and other such scenarios.</span></span>
* <span data-ttu-id="e7bcf-128">Odebrat protokoly navíc konzoly generované 3. stran knihovny.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-128">Removed extra console logs generated by 3rd party library.</span></span>

## <a name="310-08262015"></a><span data-ttu-id="e7bcf-129">3.1.0 (08/26/2015)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-129">3.1.0 (08/26/2015)</span></span>
* <span data-ttu-id="e7bcf-130">Opravte chyby kompatibility iOS 9 s knihovnou třetích stran.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-130">Fix iOS 9 compatibility bug with a third party library.</span></span> <span data-ttu-id="e7bcf-131">Že způsobuje dojde k chybě při odesílání dotazuje výsledky, informace o aplikaci nebo doplňující data.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-131">It was causing crashes while sending polls results, application information or extra data.</span></span>

## <a name="300-06192015"></a><span data-ttu-id="e7bcf-132">3.0.0 (06/19/2015)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-132">3.0.0 (06/19/2015)</span></span>
* <span data-ttu-id="e7bcf-133">Mobile Engagement používá bezobslužných nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-133">Mobile Engagement uses Silent Push Notifications.</span></span>
* <span data-ttu-id="e7bcf-134">Podpora pro iOS vyřadit 4.X.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-134">Dropped support for iOS 4.X.</span></span> <span data-ttu-id="e7bcf-135">Od této verze hello nasazení cílové aplikace musí být minimálně iOS 6.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-135">Starting from this version hello deployment target of your application must be at least iOS 6.</span></span>

## <a name="220-05212015"></a><span data-ttu-id="e7bcf-136">2.2.0 (05/21/2015)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-136">2.2.0 (05/21/2015)</span></span>
* <span data-ttu-id="e7bcf-137">id zařízení Hello Mobile Engagement pro zařízení < iOS 6 teď vychází identifikátor GUID generovány v době instalace.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-137">hello Mobile Engagement device id for devices < iOS 6 is now based on a GUID generated at installation time.</span></span>

## <a name="210-04242015"></a><span data-ttu-id="e7bcf-138">2.1.0 (04/24/2015)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-138">2.1.0 (04/24/2015)</span></span>
* <span data-ttu-id="e7bcf-139">Přidání Swift kompatibility.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-139">Added Swift compatibility.</span></span>
* <span data-ttu-id="e7bcf-140">Když kliknete na oznámení, provést hello akci, kterou je adresa URL po otevření aplikace hello vpravo.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-140">When clicking on a notification, hello action URL is now executed right after hello application is opened.</span></span>
* <span data-ttu-id="e7bcf-141">Přidané chybí hlavička souboru v balíčku sady SDK.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-141">Added missing header file in SDK package.</span></span>
* <span data-ttu-id="e7bcf-142">Oprava problému při hello Mobile Engagement havárií zpravodaje, která byla zakázána.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-142">Fixed an issue when hello Mobile Engagement crash reporter was disabled.</span></span>

## <a name="200-02172015"></a><span data-ttu-id="e7bcf-143">2.0.0 (02/17/2015)</span><span class="sxs-lookup"><span data-stu-id="e7bcf-143">2.0.0 (02/17/2015)</span></span>
* <span data-ttu-id="e7bcf-144">Počáteční verzi Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="e7bcf-144">Initial Release of Azure Mobile Engagement</span></span>
* <span data-ttu-id="e7bcf-145">Konfigurace appId/sdkKey je nahrazena konfiguraci řetězec připojení.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-145">appId/sdkKey configuration is replaced by a connection string configuration.</span></span>
* <span data-ttu-id="e7bcf-146">Odebrat toosend rozhraní API a přijímat libovolné protokolu XMPP zprávy z libovolné protokolu XMPP entit.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-146">Removed API toosend and receive arbitrary XMPP messages from arbitrary XMPP entities.</span></span>
* <span data-ttu-id="e7bcf-147">Odebrat toosend rozhraní API a přijímat zprávy mezi zařízeními.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-147">Removed API toosend and receive messages between devices.</span></span>
* <span data-ttu-id="e7bcf-148">Vylepšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-148">Security improvements.</span></span>
* <span data-ttu-id="e7bcf-149">Sledování SmartAd odebrat.</span><span class="sxs-lookup"><span data-stu-id="e7bcf-149">SmartAd tracking removed.</span></span>
