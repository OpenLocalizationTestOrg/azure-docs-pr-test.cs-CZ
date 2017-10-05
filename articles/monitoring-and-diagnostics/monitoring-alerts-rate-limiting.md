---
title: "Míra, omezení pro SMS, e-mailů a webhooky | Microsoft Docs"
description: "Pochopte, jak Azure omezuje počet možných oznámení SMS, e-mailu nebo webhooku ze skupiny pro akce."
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
ms.openlocfilehash: bde645624ab1860d19ba18470f55845855a7d1fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a><span data-ttu-id="1de0b-103">Míra, omezení pro zprávy SMS, e-mailů a webhooku příspěvcích</span><span class="sxs-lookup"><span data-stu-id="1de0b-103">Rate limiting for SMS messages, emails, and webhook posts</span></span>
<span data-ttu-id="1de0b-104">Omezení rychlosti je pozastavení oznámení, která nastane po příliš mnoho oznámení se odesílají do konkrétní telefonní číslo nebo e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="1de0b-104">Rate limiting is a suspension of notifications that occurs when too many notifications are sent to a particular phone number or email address.</span></span> <span data-ttu-id="1de0b-105">Omezení rychlosti zajistí výstrahy spravovat a možné použít.</span><span class="sxs-lookup"><span data-stu-id="1de0b-105">Rate limiting ensures that alerts are manageable and actionable.</span></span>

<span data-ttu-id="1de0b-106">Pravidla pro SMS a e-mailu jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="1de0b-106">The rules for SMS and email are the same.</span></span> <span data-ttu-id="1de0b-107">Prahová hodnota omezení míry je:</span><span class="sxs-lookup"><span data-stu-id="1de0b-107">The rate limit threshold is:</span></span>

 - <span data-ttu-id="1de0b-108">**SMS**: 10 zpráv za hodinu.</span><span class="sxs-lookup"><span data-stu-id="1de0b-108">**SMS**: 10 messages in an hour.</span></span>
 - <span data-ttu-id="1de0b-109">**E-mailu**: 100 zpráv za hodinu.</span><span class="sxs-lookup"><span data-stu-id="1de0b-109">**Email**: 100 messages in an hour.</span></span>

## <a name="rate-limit-rules"></a><span data-ttu-id="1de0b-110">Míra limit pravidla</span><span class="sxs-lookup"><span data-stu-id="1de0b-110">Rate limit rules</span></span>
- <span data-ttu-id="1de0b-111">Konkrétní telefonní číslo nebo e-mailu je míra při přijetí více zpráv, než prahová hodnota umožňuje omezené.</span><span class="sxs-lookup"><span data-stu-id="1de0b-111">A particular phone number or email is rate limited when it receives more messages than the threshold allows.</span></span>
- <span data-ttu-id="1de0b-112">Telefonní číslo nebo e-mailu může být součástí akce skupin napříč mnoho odběrů.</span><span class="sxs-lookup"><span data-stu-id="1de0b-112">A phone number or email can be part of action groups across many subscriptions.</span></span> <span data-ttu-id="1de0b-113">Omezení rychlosti platí ve všech předplatných.</span><span class="sxs-lookup"><span data-stu-id="1de0b-113">Rate limiting applies across all subscriptions.</span></span> <span data-ttu-id="1de0b-114">Jakmile je dosaženo prahové hodnoty, platí i v případě, že jsou zpráv odesílány z více předplatných.</span><span class="sxs-lookup"><span data-stu-id="1de0b-114">It applies as soon as the threshold is reached, even if messages are sent from multiple subscriptions.</span></span>  
- <span data-ttu-id="1de0b-115">Pokud telefonní číslo nebo e-mailu je míra omezené, další oznámení odesláno komunikovat omezení rychlosti.</span><span class="sxs-lookup"><span data-stu-id="1de0b-115">When a phone number or email is rate limited, an additional notification is sent to communicate the rate limiting.</span></span> <span data-ttu-id="1de0b-116">Stavy oznámení, když vyprší platnost omezení rychlosti.</span><span class="sxs-lookup"><span data-stu-id="1de0b-116">The notification states when the rate limiting expires.</span></span>

## <a name="rate-limit-of-webhooks"></a><span data-ttu-id="1de0b-117">Míra limit webhooky</span><span class="sxs-lookup"><span data-stu-id="1de0b-117">Rate limit of webhooks</span></span> ##
<span data-ttu-id="1de0b-118">Neexistuje žádné kurz omezení nastavené pro webhooky.</span><span class="sxs-lookup"><span data-stu-id="1de0b-118">There is no rate limiting in place for webhooks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1de0b-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1de0b-119">Next steps</span></span> ##
* <span data-ttu-id="1de0b-120">Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="1de0b-120">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>
* <span data-ttu-id="1de0b-121">Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak dostávat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1de0b-121">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how to receive alerts.</span></span>  
* <span data-ttu-id="1de0b-122">Zjistěte, jak [Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="1de0b-122">Learn how to [configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
