---
title: "SMS výstrahy chování ve skupinách akce | Microsoft Docs"
description: "Formát zprávy SMS a odpovídat na zprávy SMS k odhlášení odběru, obnovit předplatné nebo požádat o pomoc."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 3e4eca174209eeb9cbce1d45111d1e5cc30af8b0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a><span data-ttu-id="cd44e-103">SMS výstrahy chování ve skupinách, akce</span><span class="sxs-lookup"><span data-stu-id="cd44e-103">SMS Alert Behavior in Action Groups</span></span>
## <a name="overview"></a><span data-ttu-id="cd44e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="cd44e-104">Overview</span></span> ##
<span data-ttu-id="cd44e-105">Akce skupiny umožňují konfigurovat seznam příjemců.</span><span class="sxs-lookup"><span data-stu-id="cd44e-105">Action groups enable you to configure a list of receivers.</span></span> <span data-ttu-id="cd44e-106">Tyto skupiny můžete využít pak při definování aktivity protokolu výstrah; zajištění, že konkrétní akce skupiny zasláno oznámení, když se aktivuje výstraha aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="cd44e-106">These groups can then be leveraged when defining activity log alerts; ensuring that a particular action group is notified when the activity log alert is triggered.</span></span> <span data-ttu-id="cd44e-107">Jeden z výstrahy mechanismů podporována je SMS; výstrahy podporovat jednosměrnou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="cd44e-107">One of the alerting mechanisms supported is SMS; the alerts support bi-directional communication.</span></span> <span data-ttu-id="cd44e-108">Uživatel může reagovat na výstrahu:</span><span class="sxs-lookup"><span data-stu-id="cd44e-108">A user can respond to an alert to:</span></span>

- <span data-ttu-id="cd44e-109">**Odběr upozornění:** uživatele můžete zrušit odběr všechny výstrahy služby SMS pro všechny skupiny akce nebo skupinu singulární akce.</span><span class="sxs-lookup"><span data-stu-id="cd44e-109">**Unsubscribe from alerts:** A user can unsubscribe from all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="cd44e-110">**Obnovit předplatné výstrah:** uživatele můžete obnovit předplatné na všechny výstrahy služby SMS pro všechny skupiny akce nebo skupinu singulární akce.</span><span class="sxs-lookup"><span data-stu-id="cd44e-110">**Resubscribe to alerts:** A user can resubscribe to all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="cd44e-111">**Požádat o pomoc:** uživatel může požádat o další informace o serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="cd44e-111">**Request help:** A user can ask for more information on the SMS.</span></span> <span data-ttu-id="cd44e-112">Bude přesměrován na tento článek</span><span class="sxs-lookup"><span data-stu-id="cd44e-112">They will be redirected to this article</span></span>

<span data-ttu-id="cd44e-113">Tento článek se zabývá chování výstrahy SMS a akce odpovědi uživatele můžete provádět na základě národního prostředí uživatele:</span><span class="sxs-lookup"><span data-stu-id="cd44e-113">This article covers the behavior of the SMS alerts and the response actions the user can take based on the locale of the user:</span></span>

## <a name="usacanada-sms-behavior"></a><span data-ttu-id="cd44e-114">Chování USA nebo Kanady SMS</span><span class="sxs-lookup"><span data-stu-id="cd44e-114">USA/Canada SMS behavior</span></span>
### <a name="receiving-an-sms-alert"></a><span data-ttu-id="cd44e-115">Příjem oznámení SMS</span><span class="sxs-lookup"><span data-stu-id="cd44e-115">Receiving an SMS Alert</span></span>
<span data-ttu-id="cd44e-116">SMS příjemce, který je nakonfigurovaný jako součást skupiny akce, obdrží serveru služby SMS při aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="cd44e-116">An SMS receiver, who is configured as part of an action group, will receive an SMS when an alert fires.</span></span> <span data-ttu-id="cd44e-117">Serveru SMS, bude mít následující informace:</span><span class="sxs-lookup"><span data-stu-id="cd44e-117">The SMS will carry the following information:</span></span>
* <span data-ttu-id="cd44e-118">Shortname skupiny akce, které tato výstraha byla odeslána</span><span class="sxs-lookup"><span data-stu-id="cd44e-118">Shortname of the action group this alert was sent to</span></span>
- <span data-ttu-id="cd44e-119">Název výstrahy</span><span class="sxs-lookup"><span data-stu-id="cd44e-119">Title of the alert</span></span>

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a><span data-ttu-id="cd44e-120">Odhlášení z SMS výstrahy pro jednu skupinu akce</span><span class="sxs-lookup"><span data-stu-id="cd44e-120">Unsubscribing from SMS alerts for one action group</span></span>
<span data-ttu-id="cd44e-121">Uživatele můžete zrušit odběr služby SMS pro výstrahy pro jednu akci skupinu podle neodpovídá na požadavky shortcode 20873 s klíčovými slovy: "Zakázat &lt;Shortname akce skupiny&gt;".</span><span class="sxs-lookup"><span data-stu-id="cd44e-121">A user can unsubscribe from SMS for alerts for one action group by responding to the shortcode 20873 with the keywords: “DISABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="cd44e-122">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cd44e-122">Ex.</span></span> <span data-ttu-id="cd44e-123">Uživatel chtějí odhlásit z výstrah pro skupinu akce s shortname "Azure" by odeslat zprávu SMS shortcode 20873 s informacemi o tom "Zakázat Azure"</span><span class="sxs-lookup"><span data-stu-id="cd44e-123">A user wishing to unsubscribe from alerts for an action group with the shortname “Azure”, would send an SMS to the shortcode 20873 that says “DISABLE Azure”</span></span>

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a><span data-ttu-id="cd44e-124">Odhlášení z SMS výstrahy pro všechny skupiny akce</span><span class="sxs-lookup"><span data-stu-id="cd44e-124">Unsubscribing from SMS alerts for all action groups</span></span>
<span data-ttu-id="cd44e-125">Uživatele můžete zrušit odběr všechny výstrahy služby SMS pro všechny skupiny akce odpovědi na shortcode 20873 s žádným z následujících klíčových slov:</span><span class="sxs-lookup"><span data-stu-id="cd44e-125">A user can unsubscribe from all SMS alerts for all action groups by responding to the shortcode 20873 with any of the following keywords:</span></span>
* <span data-ttu-id="cd44e-126">STOP</span><span class="sxs-lookup"><span data-stu-id="cd44e-126">STOP</span></span>

<span data-ttu-id="cd44e-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cd44e-127">Ex.</span></span> <span data-ttu-id="cd44e-128">Uživatel chtějí odběr všech upozornění služby SMS pro všechny skupiny akce by odeslat zprávu SMS shortcode 20873 s informacemi o tom "STOP"</span><span class="sxs-lookup"><span data-stu-id="cd44e-128">A user wishing to unsubscribe from all SMS alerts for all action groups, would send an SMS to the shortcode 20873 that says “STOP”</span></span>

>[!NOTE]
><span data-ttu-id="cd44e-129">Pokud uživatel odhlásil(a) odběr z výstrah služby SMS, ale se pak přidá do nové skupiny akce; BUDE přijímat výstrahy služby SMS pro tuto novou skupinu akce ale zůstávají odhlásit ze všech skupin, předchozí akce.</span><span class="sxs-lookup"><span data-stu-id="cd44e-129">If a user has unsubscribed from SMS alerts, but is then added to a new action group; they WILL receive SMS alerts for that new action group, but remain unsubscribed from all previous action groups.</span></span>
>
>

### <a name="resubscribing-to-sms-alerts-for-one-action-group"></a><span data-ttu-id="cd44e-130">Resubscribing SMS výstrah pro jednu skupinu akce</span><span class="sxs-lookup"><span data-stu-id="cd44e-130">Resubscribing to SMS alerts for one action group</span></span>
<span data-ttu-id="cd44e-131">Uživatele můžete obnovit předplatné do služby SMS pro výstrahy pro jednu akci skupinu podle neodpovídá na požadavky shortcode 20873 s klíčová slova: "Povolit &lt;Shortname akce skupiny&gt;".</span><span class="sxs-lookup"><span data-stu-id="cd44e-131">A user can resubscribe to SMS for alerts for one action group by responding to the shortcode 20873 with the keywords: “ENABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="cd44e-132">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cd44e-132">Ex.</span></span> <span data-ttu-id="cd44e-133">Uživatel chtějí obnovit předplatné výstrah pro skupinu akce s shortname "Azure" by odeslat zprávu SMS shortcode 20873 s informacemi o tom "Povolit Azure"</span><span class="sxs-lookup"><span data-stu-id="cd44e-133">A user wishing to resubscribe to alerts for an action group with the shortname “Azure”, would send an SMS to the shortcode 20873 that says “ENABLE Azure”</span></span>

### <a name="resubscribing-to-sms-alerts-for-all-action-groups"></a><span data-ttu-id="cd44e-134">Resubscribing SMS výstrah pro všechny skupiny akce</span><span class="sxs-lookup"><span data-stu-id="cd44e-134">Resubscribing to SMS alerts for all action groups</span></span>
<span data-ttu-id="cd44e-135">Uživatele můžete obnovit předplatné pro všechny služby SMS pro výstrahy pro všechny skupiny akce odpovědi na shortcode 20873 s žádným z následujících klíčových slov:</span><span class="sxs-lookup"><span data-stu-id="cd44e-135">A user can resubscribe to all SMS for alerts for all action groups by responding to the shortcode 20873 with any of the following keywords:</span></span>

* <span data-ttu-id="cd44e-136">SPUŠTĚNÍ</span><span class="sxs-lookup"><span data-stu-id="cd44e-136">START</span></span>

<span data-ttu-id="cd44e-137">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cd44e-137">Ex.</span></span> <span data-ttu-id="cd44e-138">Uživatel chtějí odběr všech upozornění služby SMS pro všechny skupiny akce by odeslat zprávu SMS shortcode 20873 s informacemi o tom "START"</span><span class="sxs-lookup"><span data-stu-id="cd44e-138">A user wishing to unsubscribe from all SMS alerts for all action groups, would send an SMS to the shortcode 20873 that says “START”</span></span>

### <a name="requesting-help-via-sms"></a><span data-ttu-id="cd44e-139">Žádost o nápovědu prostřednictvím serveru SMS</span><span class="sxs-lookup"><span data-stu-id="cd44e-139">Requesting help via SMS</span></span>
<span data-ttu-id="cd44e-140">Uživatel může požádat o další informace o serveru SMS, že obdržely podle neodpovídá na požadavky shortcode 20873 s žádným z následujících klíčových slov:</span><span class="sxs-lookup"><span data-stu-id="cd44e-140">A user can ask for more information about the SMS they have received by responding to the shortcode 20873 with any of the following keywords:</span></span>
* <span data-ttu-id="cd44e-141">POMOC</span><span class="sxs-lookup"><span data-stu-id="cd44e-141">HELP</span></span>

<span data-ttu-id="cd44e-142">Odpověď zašle uživateli se zobrazí odkaz na tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="cd44e-142">A response will be sent to the user with a link to this article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd44e-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd44e-143">Next Steps</span></span>
<span data-ttu-id="cd44e-144">Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md) a zjistěte, jak získat výstrahy</span><span class="sxs-lookup"><span data-stu-id="cd44e-144">Get an [overview of activity log alerts](monitoring-overview-alerts.md) and learn how to get alerted</span></span>  
<span data-ttu-id="cd44e-145">Další informace o [omezení rychlosti SMS](monitoring-alerts-rate-limiting.md)</span><span class="sxs-lookup"><span data-stu-id="cd44e-145">Learn more about [SMS rate limiting](monitoring-alerts-rate-limiting.md)</span></span>  
<span data-ttu-id="cd44e-146">Další informace o [skupiny akcí](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="cd44e-146">Learn more about [action groups](monitoring-action-groups.md)</span></span>
