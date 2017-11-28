---
title: "aaaWindows obsah univerzální aplikace sady SDK"
description: "Další informace o obsahu hello hello Windows Universal SDK aplikací pro Azure Mobile Engagement"
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
ms.openlocfilehash: a8013d2433c0be62d737c8bc6e8360ed79bbe532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-content"></a><span data-ttu-id="d2a35-103">Obsah Windows Universal SDK aplikace</span><span class="sxs-lookup"><span data-stu-id="d2a35-103">Windows Universal Apps SDK content</span></span>
<span data-ttu-id="d2a35-104">Tento dokument uvádí a popisuje obsah hello nasadit pomocí hello SDK v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d2a35-104">This document lists and describes hello content deployed by hello SDK in your application.</span></span>

## <a name="hello-resources-folder"></a><span data-ttu-id="d2a35-105">Hello `/Resources` složky</span><span class="sxs-lookup"><span data-stu-id="d2a35-105">hello `/Resources` folder</span></span>
<span data-ttu-id="d2a35-106">Tato složka obsahuje všechny hello prostředky, které potřebuje Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="d2a35-106">This folder contains all hello resources that Mobile Engagement needs.</span></span> <span data-ttu-id="d2a35-107">Můžete také upravit je toofit vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d2a35-107">You can also customize them toofit your app.</span></span>

* <span data-ttu-id="d2a35-108">`EngagementConfiguration.xml`: hello Mobile Engagement konfigurační soubor, to je, kde můžete upravit nastavení Mobile Engagementu (Mobile Engagement připojovací řetězec, sestava havárií...).</span><span class="sxs-lookup"><span data-stu-id="d2a35-108">`EngagementConfiguration.xml` : hello Mobile Engagement's configuration file, this is where you can customize Mobile Engagement settings (Mobile Engagement connection string, report crash...).</span></span>

### <a name="html-folder"></a><span data-ttu-id="d2a35-109">Složka/HTML</span><span class="sxs-lookup"><span data-stu-id="d2a35-109">/html folder</span></span>
* <span data-ttu-id="d2a35-110">`EngagementNotification.html`: hello `Notification` webové zobrazení html návrh hlaviček v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d2a35-110">`EngagementNotification.html` : hello `Notification` web view html design for in-app banners.</span></span>
* <span data-ttu-id="d2a35-111">`EngagementAnnouncement.html`: hello `Announcement` webové zobrazení html návrhu pro vkládaná zobrazení v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d2a35-111">`EngagementAnnouncement.html` : hello `Announcement` web view html design for in-app interstitial views.</span></span>

### <a name="images-folder"></a><span data-ttu-id="d2a35-112">/Images složky</span><span class="sxs-lookup"><span data-stu-id="d2a35-112">/images folder</span></span>
* <span data-ttu-id="d2a35-113">`EngagementIconNotification.png`: left ikonu značky hello v hello zobrazí oznámení, nahraďte tento vaší brand ikonou.</span><span class="sxs-lookup"><span data-stu-id="d2a35-113">`EngagementIconNotification.png` : hello brand icon displayed at hello left of a notification, replace this one by your brand icon.</span></span>
* <span data-ttu-id="d2a35-114">`EngagementIconOk.png`: hello `Ok` ikonu hello reach stránek s obsahem pro tlačítko akce nebo ověření hello.</span><span class="sxs-lookup"><span data-stu-id="d2a35-114">`EngagementIconOk.png` : hello `Ok` icon of hello reach content pages for hello action or validation button.</span></span>
* <span data-ttu-id="d2a35-115">`EngagementIconNOK.png`: hello `NOK` ikony používané při ověřování tlačítko hello stránek s obsahem reach hello je zakázané.</span><span class="sxs-lookup"><span data-stu-id="d2a35-115">`EngagementIconNOK.png` : hello `NOK` icon used when hello validation button of hello reach content pages is disabled.</span></span>
* <span data-ttu-id="d2a35-116">`EngagementIconClose.png`: hello `Close` ikonu hello dosáhnout oznámení a obsah pro hello zavřít tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2a35-116">`EngagementIconClose.png` : hello `Close` icon of hello reach notifications and contents for hello dismiss button.</span></span>

### <a name="overlay-folder"></a><span data-ttu-id="d2a35-117">/Overlay složky</span><span class="sxs-lookup"><span data-stu-id="d2a35-117">/overlay folder</span></span>
* <span data-ttu-id="d2a35-118">`EngagementPageOverlay.cs`: stránka překrytí hello zodpovědný za přidání hello Engagement reach podřízené tooits uživatelského rozhraní v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d2a35-118">`EngagementPageOverlay.cs` : hello overlay page responsible for adding hello Engagement reach in-app UI tooits child.</span></span>

