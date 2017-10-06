---
title: aaaAndroid integraci sady SDK pro Azure Mobile Engagement
description: Popisuje, jak toointegrate Azure Mobile Engagement SDK do aplikace pro Android
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a91ed04f-f3ce-4692-a6dd-b56a28d7dee8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo;ricksal
ms.openlocfilehash: 0c63bfaf673abbda7ea498390f8282c43e2fb8df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="android-sdk-integration-for-azure-mobile-engagement"></a><span data-ttu-id="7508b-103">Integraci sady Android SDK pro Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="7508b-103">Android SDK Integration for Azure Mobile Engagement</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7508b-104">Univerzální platforma Windows</span><span class="sxs-lookup"><span data-stu-id="7508b-104">Universal Windows</span></span>](mobile-engagement-windows-store-sdk-overview.md)
> * [<span data-ttu-id="7508b-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="7508b-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-sdk-overview.md)
> * [<span data-ttu-id="7508b-106">iOS</span><span class="sxs-lookup"><span data-stu-id="7508b-106">iOS</span></span>](mobile-engagement-ios-sdk-overview.md)
> * [<span data-ttu-id="7508b-107">Android</span><span class="sxs-lookup"><span data-stu-id="7508b-107">Android</span></span>](mobile-engagement-android-sdk-overview.md)
> 
> 

<span data-ttu-id="7508b-108">Tento dokument popisuje hello integrace a konfigurace k dispozici všechny možnosti pro Azure Mobile Engagement Android SDK.</span><span class="sxs-lookup"><span data-stu-id="7508b-108">This document describes all hello integration and configuration options available for Azure Mobile Engagement Android SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7508b-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7508b-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a><span data-ttu-id="7508b-110">Pokročilé funkce</span><span class="sxs-lookup"><span data-stu-id="7508b-110">Advanced Features</span></span>
### <a name="reporting-features"></a><span data-ttu-id="7508b-111">Funkce vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="7508b-111">Reporting Features</span></span>
<span data-ttu-id="7508b-112">Můžete přidat tyto funkce:</span><span class="sxs-lookup"><span data-stu-id="7508b-112">You can add these features:</span></span>

1. [<span data-ttu-id="7508b-113">Rozšířené možnosti vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="7508b-113">Advanced reporting options</span></span>](mobile-engagement-android-advanced-reporting.md)
2. [<span data-ttu-id="7508b-114">Možnosti zasílání sestav umístění</span><span class="sxs-lookup"><span data-stu-id="7508b-114">Location Reporting options</span></span>](mobile-engagement-android-location-reporting.md)
3. [<span data-ttu-id="7508b-115">Rozšířené možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="7508b-115">Advanced Configuration options</span></span>](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a><span data-ttu-id="7508b-116">Oznámení:</span><span class="sxs-lookup"><span data-stu-id="7508b-116">Notifications:</span></span>
[<span data-ttu-id="7508b-117">Jak toointegrate Reach (oznámení) v aplikacích pro Android</span><span class="sxs-lookup"><span data-stu-id="7508b-117">How toointegrate Reach (Notifications) in your Android app</span></span>](mobile-engagement-android-integrate-engagement-reach.md)

1. <span data-ttu-id="7508b-118">Google Cloud Messaging (GCM): [jak tooIntegrate GCM s Mobile Engagement](mobile-engagement-android-gcm-integrate.md)</span><span class="sxs-lookup"><span data-stu-id="7508b-118">Google Cloud Messaging (GCM): [How tooIntegrate GCM with Mobile Engagement](mobile-engagement-android-gcm-integrate.md)</span></span>
2. <span data-ttu-id="7508b-119">Zasílání zpráv zařízení Amazon (ADM): [jak tooIntegrate ADM s Mobile Engagement](mobile-engagement-android-adm-integrate.md)</span><span class="sxs-lookup"><span data-stu-id="7508b-119">Amazon Device Messaging (ADM): [How tooIntegrate ADM with Mobile Engagement](mobile-engagement-android-adm-integrate.md)</span></span>

### <a name="tag-plan-implementation"></a><span data-ttu-id="7508b-120">Implementace plánu značky:</span><span class="sxs-lookup"><span data-stu-id="7508b-120">Tag plan implementation:</span></span>
[<span data-ttu-id="7508b-121">Jak toouse hello advanced Mobile Engagement označování rozhraní API v aplikacích pro Android</span><span class="sxs-lookup"><span data-stu-id="7508b-121">How toouse hello advanced Mobile Engagement tagging API in your Android app</span></span>](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a><span data-ttu-id="7508b-122">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="7508b-122">Release notes</span></span>

### <a name="431-07172017"></a><span data-ttu-id="7508b-123">4.3.1 (07/17/2017)</span><span class="sxs-lookup"><span data-stu-id="7508b-123">4.3.1 (07/17/2017)</span></span>
* <span data-ttu-id="7508b-124">Opravte havárii, která se může občas nastat při volání metody `EngagementAgentUtils.isInDedicatedEngagementProcess`, které používá také hello `EngagementApplication` třídy.</span><span class="sxs-lookup"><span data-stu-id="7508b-124">Fix a crash that could rarely happen when calling `EngagementAgentUtils.isInDedicatedEngagementProcess`, which is also used by hello `EngagementApplication` class.</span></span>

### <a name="430-06272017"></a><span data-ttu-id="7508b-125">4.3.0 (06/27/2017)</span><span class="sxs-lookup"><span data-stu-id="7508b-125">4.3.0 (06/27/2017)</span></span>
* <span data-ttu-id="7508b-126">8 podpora pro Android (předchozí verze hello SDK nebude fungovat v systému Android 8).</span><span class="sxs-lookup"><span data-stu-id="7508b-126">Android 8 support (previous versions of hello SDK will not work on Android 8).</span></span>
* <span data-ttu-id="7508b-127">Žádné další závislosti na podporu knihovny.</span><span class="sxs-lookup"><span data-stu-id="7508b-127">No more dependency on support library.</span></span>
* <span data-ttu-id="7508b-128">Odebrat `EngagementFragmentActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="7508b-128">Remove `EngagementFragmentActivity` class.</span></span>
* <span data-ttu-id="7508b-129">Kvůli příliš[omezení provádění pozadí](https://developer.android.com/preview/features/background.html) na Android 8 může zpoždění protokoly v pozadí, dokud hello uživatel pracuje s hello zařízení, to bude mít dopad na kampaň nabízených **doručené** a **Systémové oznámení zobrazené** statistiky je zpožděno. Pokud byl hello zařízení v režimu spánku (hello oznámení bude i nadále zobrazovat, bude prstence a zavibrovat v reálném čase bez problémů).</span><span class="sxs-lookup"><span data-stu-id="7508b-129">Due too[Background Execution Limits](https://developer.android.com/preview/features/background.html) on Android 8, logs in background might be delayed until hello user interacts with hello device, this will have an impact on Push Campaign **Delivered** and **System notification displayed** statistics being delayed if hello device was sleeping (hello notification will still be displayed, will ring and vibrate in real time without issues).</span></span>
* <span data-ttu-id="7508b-130">Kvůli příliš[omezení umístění pozadí](https://developer.android.com/preview/features/background-location-limits.html), hello reálném čase umístění pozadí se nebude často aktualizovat na Android 8.</span><span class="sxs-lookup"><span data-stu-id="7508b-130">Due too[Background Location Limits](https://developer.android.com/preview/features/background-location-limits.html), hello real time location in background will not be updated frequently on Android 8.</span></span>

<span data-ttu-id="7508b-131">Všechny verze najdete v tématu hello [dokončení poznámky k verzi](mobile-engagement-android-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="7508b-131">For all versions, see hello [complete release notes](mobile-engagement-android-release-notes.md).</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="7508b-132">Postupy upgradu</span><span class="sxs-lookup"><span data-stu-id="7508b-132">Upgrade procedures</span></span>
<span data-ttu-id="7508b-133">Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, projděte si [postupy upgradu](mobile-engagement-android-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="7508b-133">If you already have integrated an older version of our SDK into your application, consult [Upgrade Procedures](mobile-engagement-android-upgrade-procedure.md).</span></span>

