---
title: "Výstrahy chování aaaSMS ve skupinách akce | Microsoft Docs"
description: "Formát zprávy SMS a odpovídá toounsubscribe zprávy tooSMS, obnovit předplatné nebo požádat o pomoc."
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
ms.openlocfilehash: 3cd09b1903e3472f6402f62b74409d97e7e7ea97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a><span data-ttu-id="dd57d-103">SMS výstrahy chování ve skupinách, akce</span><span class="sxs-lookup"><span data-stu-id="dd57d-103">SMS Alert Behavior in Action Groups</span></span>
## <a name="overview"></a><span data-ttu-id="dd57d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="dd57d-104">Overview</span></span> ##
<span data-ttu-id="dd57d-105">Skupiny akcí povolit tooconfigure seznam příjemců.</span><span class="sxs-lookup"><span data-stu-id="dd57d-105">Action groups enable you tooconfigure a list of receivers.</span></span> <span data-ttu-id="dd57d-106">Tyto skupiny můžete využít pak při definování aktivity protokolu výstrah; zajištění, že skupinu konkrétní akce zasláno oznámení, když se aktivuje výstraha hello aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="dd57d-106">These groups can then be leveraged when defining activity log alerts; ensuring that a particular action group is notified when hello activity log alert is triggered.</span></span> <span data-ttu-id="dd57d-107">Jeden z hello výstrahy mechanismy podporována je SMS; výstrahy Hello podporovat jednosměrnou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="dd57d-107">One of hello alerting mechanisms supported is SMS; hello alerts support bi-directional communication.</span></span> <span data-ttu-id="dd57d-108">Uživatel může reagovat tooan výstrahu:</span><span class="sxs-lookup"><span data-stu-id="dd57d-108">A user can respond tooan alert to:</span></span>

- <span data-ttu-id="dd57d-109">**Odběr upozornění:** uživatele můžete zrušit odběr všechny výstrahy služby SMS pro všechny skupiny akce nebo skupinu singulární akce.</span><span class="sxs-lookup"><span data-stu-id="dd57d-109">**Unsubscribe from alerts:** A user can unsubscribe from all SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="dd57d-110">**Obnovit předplatné tooalerts:** uživatele můžete obnovit předplatné tooall SMS výstrahy pro všechny skupiny akce nebo skupinu singulární akce.</span><span class="sxs-lookup"><span data-stu-id="dd57d-110">**Resubscribe tooalerts:** A user can resubscribe tooall SMS alerts for all action groups, or a singular action group.</span></span>  
- <span data-ttu-id="dd57d-111">**Požádat o pomoc:** uživatel může požádat o další informace o hello SMS.</span><span class="sxs-lookup"><span data-stu-id="dd57d-111">**Request help:** A user can ask for more information on hello SMS.</span></span> <span data-ttu-id="dd57d-112">Budou přesměrovaného toothis článku</span><span class="sxs-lookup"><span data-stu-id="dd57d-112">They will be redirected toothis article</span></span>

<span data-ttu-id="dd57d-113">Tento článek se zabývá hello chování hello SMS výstrah a hello odpovědi akce hello uživatele můžete provádět na základě hello národního prostředí uživatele hello:</span><span class="sxs-lookup"><span data-stu-id="dd57d-113">This article covers hello behavior of hello SMS alerts and hello response actions hello user can take based on hello locale of hello user:</span></span>

## <a name="usacanada-sms-behavior"></a><span data-ttu-id="dd57d-114">Chování USA nebo Kanady SMS</span><span class="sxs-lookup"><span data-stu-id="dd57d-114">USA/Canada SMS behavior</span></span>
### <a name="receiving-an-sms-alert"></a><span data-ttu-id="dd57d-115">Příjem oznámení SMS</span><span class="sxs-lookup"><span data-stu-id="dd57d-115">Receiving an SMS Alert</span></span>
<span data-ttu-id="dd57d-116">SMS příjemce, který je nakonfigurovaný jako součást skupiny akce, obdrží serveru služby SMS při aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="dd57d-116">An SMS receiver, who is configured as part of an action group, will receive an SMS when an alert fires.</span></span> <span data-ttu-id="dd57d-117">Hello SMS vyplní hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="dd57d-117">hello SMS will carry hello following information:</span></span>
* <span data-ttu-id="dd57d-118">Shortname hello akce skupiny byla odeslána tato výstraha</span><span class="sxs-lookup"><span data-stu-id="dd57d-118">Shortname of hello action group this alert was sent to</span></span>
- <span data-ttu-id="dd57d-119">Název výstrahy hello</span><span class="sxs-lookup"><span data-stu-id="dd57d-119">Title of hello alert</span></span>

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a><span data-ttu-id="dd57d-120">Odhlášení z SMS výstrahy pro jednu skupinu akce</span><span class="sxs-lookup"><span data-stu-id="dd57d-120">Unsubscribing from SMS alerts for one action group</span></span>
<span data-ttu-id="dd57d-121">Uživatele můžete zrušit odběr služby SMS pro výstrahy pro jednu akci skupinu podle toohello shortcode odpovídá 20873 s hello klíčová slova: "Zakázat &lt;Shortname akce skupiny&gt;".</span><span class="sxs-lookup"><span data-stu-id="dd57d-121">A user can unsubscribe from SMS for alerts for one action group by responding toohello shortcode 20873 with hello keywords: “DISABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="dd57d-122">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dd57d-122">Ex.</span></span> <span data-ttu-id="dd57d-123">Hodně toounsubscribe z výstrah pro skupinu akce s shortname hello "Azure", uživatel by odesílat shortcode toohello zprávy služby SMS 20873, která uvádí "Zakázat Azure"</span><span class="sxs-lookup"><span data-stu-id="dd57d-123">A user wishing toounsubscribe from alerts for an action group with hello shortname “Azure”, would send an SMS toohello shortcode 20873 that says “DISABLE Azure”</span></span>

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a><span data-ttu-id="dd57d-124">Odhlášení z SMS výstrahy pro všechny skupiny akce</span><span class="sxs-lookup"><span data-stu-id="dd57d-124">Unsubscribing from SMS alerts for all action groups</span></span>
<span data-ttu-id="dd57d-125">Uživatele můžete zrušit odběr všechny výstrahy služby SMS pro všechny skupiny akce podle toohello shortcode odpovídá 20873 s žádným z hello následující klíčová slova:</span><span class="sxs-lookup"><span data-stu-id="dd57d-125">A user can unsubscribe from all SMS alerts for all action groups by responding toohello shortcode 20873 with any of hello following keywords:</span></span>
* <span data-ttu-id="dd57d-126">STOP</span><span class="sxs-lookup"><span data-stu-id="dd57d-126">STOP</span></span>

<span data-ttu-id="dd57d-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dd57d-127">Ex.</span></span> <span data-ttu-id="dd57d-128">Uživatel hodně toounsubscribe z všechny výstrahy služby SMS pro všechny skupiny akce by odesílat shortcode toohello zprávy služby SMS 20873, která uvádí "STOP"</span><span class="sxs-lookup"><span data-stu-id="dd57d-128">A user wishing toounsubscribe from all SMS alerts for all action groups, would send an SMS toohello shortcode 20873 that says “STOP”</span></span>

>[!NOTE]
><span data-ttu-id="dd57d-129">Pokud uživatel odhlásil(a) odběr z výstrah SMS, ale se pak přidá novou skupinu akce tooa; BUDE přijímat výstrahy služby SMS pro tuto novou skupinu akce ale zůstávají odhlásit ze všech skupin, předchozí akce.</span><span class="sxs-lookup"><span data-stu-id="dd57d-129">If a user has unsubscribed from SMS alerts, but is then added tooa new action group; they WILL receive SMS alerts for that new action group, but remain unsubscribed from all previous action groups.</span></span>
>
>

### <a name="resubscribing-toosms-alerts-for-one-action-group"></a><span data-ttu-id="dd57d-130">Resubscribing tooSMS výstrahy pro jednu skupinu akce</span><span class="sxs-lookup"><span data-stu-id="dd57d-130">Resubscribing tooSMS alerts for one action group</span></span>
<span data-ttu-id="dd57d-131">Uživatele můžete obnovit předplatné tooSMS pro výstrahy pro jednu akci skupinu podle toohello shortcode odpovídá 20873 s hello klíčová slova: "Povolit &lt;Shortname akce skupiny&gt;".</span><span class="sxs-lookup"><span data-stu-id="dd57d-131">A user can resubscribe tooSMS for alerts for one action group by responding toohello shortcode 20873 with hello keywords: “ENABLE &lt;Shortname of action group&gt;”.</span></span>

<span data-ttu-id="dd57d-132">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dd57d-132">Ex.</span></span> <span data-ttu-id="dd57d-133">Hodně tooresubscribe tooalerts pro skupinu akce s shortname hello "Azure", uživatel by odesílat shortcode toohello zprávy služby SMS 20873, která uvádí "Povolit Azure"</span><span class="sxs-lookup"><span data-stu-id="dd57d-133">A user wishing tooresubscribe tooalerts for an action group with hello shortname “Azure”, would send an SMS toohello shortcode 20873 that says “ENABLE Azure”</span></span>

### <a name="resubscribing-toosms-alerts-for-all-action-groups"></a><span data-ttu-id="dd57d-134">Resubscribing tooSMS výstrahy pro všechny skupiny akce</span><span class="sxs-lookup"><span data-stu-id="dd57d-134">Resubscribing tooSMS alerts for all action groups</span></span>
<span data-ttu-id="dd57d-135">Uživatele můžete obnovit předplatné tooall SMS pro výstrahy pro všechny skupiny akce odpovídá toohello shortcode 20873 s žádným hello následující klíčová slova:</span><span class="sxs-lookup"><span data-stu-id="dd57d-135">A user can resubscribe tooall SMS for alerts for all action groups by responding toohello shortcode 20873 with any of hello following keywords:</span></span>

* <span data-ttu-id="dd57d-136">SPUŠTĚNÍ</span><span class="sxs-lookup"><span data-stu-id="dd57d-136">START</span></span>

<span data-ttu-id="dd57d-137">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dd57d-137">Ex.</span></span> <span data-ttu-id="dd57d-138">Uživatel hodně toounsubscribe z všechny výstrahy služby SMS pro všechny skupiny akce by odesílat shortcode toohello zprávy služby SMS 20873, která uvádí "START"</span><span class="sxs-lookup"><span data-stu-id="dd57d-138">A user wishing toounsubscribe from all SMS alerts for all action groups, would send an SMS toohello shortcode 20873 that says “START”</span></span>

### <a name="requesting-help-via-sms"></a><span data-ttu-id="dd57d-139">Žádost o nápovědu prostřednictvím serveru SMS</span><span class="sxs-lookup"><span data-stu-id="dd57d-139">Requesting help via SMS</span></span>
<span data-ttu-id="dd57d-140">Další informace o hello SMS můžete požádat uživatele, že obdržely podle odpovídá toohello shortcode 20873 s žádným z hello následující klíčová slova:</span><span class="sxs-lookup"><span data-stu-id="dd57d-140">A user can ask for more information about hello SMS they have received by responding toohello shortcode 20873 with any of hello following keywords:</span></span>
* <span data-ttu-id="dd57d-141">POMOC</span><span class="sxs-lookup"><span data-stu-id="dd57d-141">HELP</span></span>

<span data-ttu-id="dd57d-142">Odpověď zašle toohello uživatele s článku toothis na odkaz.</span><span class="sxs-lookup"><span data-stu-id="dd57d-142">A response will be sent toohello user with a link toothis article.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd57d-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd57d-143">Next Steps</span></span>
<span data-ttu-id="dd57d-144">Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md) a zjistěte, jak tooget výstrahy</span><span class="sxs-lookup"><span data-stu-id="dd57d-144">Get an [overview of activity log alerts](monitoring-overview-alerts.md) and learn how tooget alerted</span></span>  
<span data-ttu-id="dd57d-145">Další informace o [omezení rychlosti SMS](monitoring-alerts-rate-limiting.md)</span><span class="sxs-lookup"><span data-stu-id="dd57d-145">Learn more about [SMS rate limiting](monitoring-alerts-rate-limiting.md)</span></span>  
<span data-ttu-id="dd57d-146">Další informace o [skupiny akcí](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="dd57d-146">Learn more about [action groups](monitoring-action-groups.md)</span></span>
