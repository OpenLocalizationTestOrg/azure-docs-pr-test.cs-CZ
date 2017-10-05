---
title: Obsah Windows Universal SDK aplikace
description: "Další informace o obsahu sady Windows SDK pro univerzální aplikace pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b28d525ab16487b963772e23fdecd11f94dcabd1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-sdk-content"></a><span data-ttu-id="10062-103">Obsah Windows Universal SDK aplikace</span><span class="sxs-lookup"><span data-stu-id="10062-103">Windows Universal Apps SDK content</span></span>
<span data-ttu-id="10062-104">Tento dokument uvádí a popisuje obsah nasadit pomocí sady SDK v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="10062-104">This document lists and describes the content deployed by the SDK in your application.</span></span>

## <a name="the-resources-folder"></a><span data-ttu-id="10062-105">`/Resources` Složky</span><span class="sxs-lookup"><span data-stu-id="10062-105">The `/Resources` folder</span></span>
<span data-ttu-id="10062-106">Tato složka obsahuje všechny prostředky, které potřebuje Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="10062-106">This folder contains all the resources that Mobile Engagement needs.</span></span> <span data-ttu-id="10062-107">Můžete také upravit, aby vyhovovaly vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="10062-107">You can also customize them to fit your app.</span></span>

* <span data-ttu-id="10062-108">`EngagementConfiguration.xml`Konfigurační soubor: Mobile Engagement, to je, kde můžete upravit nastavení Mobile Engagementu (Mobile Engagement připojovací řetězec, sestava havárií...).</span><span class="sxs-lookup"><span data-stu-id="10062-108">`EngagementConfiguration.xml` : The Mobile Engagement's configuration file, this is where you can customize Mobile Engagement settings (Mobile Engagement connection string, report crash...).</span></span>

### <a name="html-folder"></a><span data-ttu-id="10062-109">Složka/HTML</span><span class="sxs-lookup"><span data-stu-id="10062-109">/html folder</span></span>
* <span data-ttu-id="10062-110">`EngagementNotification.html`: `Notification` Webové zobrazení html návrh hlaviček v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="10062-110">`EngagementNotification.html` : The `Notification` web view html design for in-app banners.</span></span>
* <span data-ttu-id="10062-111">`EngagementAnnouncement.html`: `Announcement` Webové zobrazení html návrhu pro vkládaná zobrazení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="10062-111">`EngagementAnnouncement.html` : The `Announcement` web view html design for in-app interstitial views.</span></span>

### <a name="images-folder"></a><span data-ttu-id="10062-112">/Images složky</span><span class="sxs-lookup"><span data-stu-id="10062-112">/images folder</span></span>
* <span data-ttu-id="10062-113">`EngagementIconNotification.png`Ikona: brand zobrazí vlevo od oznámení, nahraďte tento vaší brand ikonou.</span><span class="sxs-lookup"><span data-stu-id="10062-113">`EngagementIconNotification.png` : The brand icon displayed at the left of a notification, replace this one by your brand icon.</span></span>
* <span data-ttu-id="10062-114">`EngagementIconOk.png`: `Ok` Ikona reach stránky obsahu pro tlačítko akce nebo ověření.</span><span class="sxs-lookup"><span data-stu-id="10062-114">`EngagementIconOk.png` : The `Ok` icon of the reach content pages for the action or validation button.</span></span>
* <span data-ttu-id="10062-115">`EngagementIconNOK.png`: `NOK` Ikony používané při ověřování tlačítko stránky obsahu reach je zakázané.</span><span class="sxs-lookup"><span data-stu-id="10062-115">`EngagementIconNOK.png` : The `NOK` icon used when the validation button of the reach content pages is disabled.</span></span>
* <span data-ttu-id="10062-116">`EngagementIconClose.png`: `Close` Ikona reach oznámení a obsah pro tlačítko Zavřít.</span><span class="sxs-lookup"><span data-stu-id="10062-116">`EngagementIconClose.png` : The `Close` icon of the reach notifications and contents for the dismiss button.</span></span>

### <a name="overlay-folder"></a><span data-ttu-id="10062-117">/Overlay složky</span><span class="sxs-lookup"><span data-stu-id="10062-117">/overlay folder</span></span>
* <span data-ttu-id="10062-118">`EngagementPageOverlay.cs`: Překrytí stránce zodpovědný za přidání zapojení dosáhnout uživatelského rozhraní v aplikaci na jeho podřízené.</span><span class="sxs-lookup"><span data-stu-id="10062-118">`EngagementPageOverlay.cs` : The overlay page responsible for adding the Engagement reach in-app UI to its child.</span></span>

