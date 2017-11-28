---
title: "aaaRate omezení pro SMS, e-mailů a webhooky | Microsoft Docs"
description: "Pochopte, jak Azure omezuje hello počet možných oznámení SMS, e-mailu nebo webhooku ze skupiny pro akce."
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
ms.openlocfilehash: 1cd08a5b982c82bb02e0bf93451aa1fcd9bc34af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a><span data-ttu-id="7760e-103">Míra, omezení pro zprávy SMS, e-mailů a webhooku příspěvcích</span><span class="sxs-lookup"><span data-stu-id="7760e-103">Rate limiting for SMS messages, emails, and webhook posts</span></span>
<span data-ttu-id="7760e-104">Omezení rychlosti je pozastavení oznámení, která nastane po příliš mnoho oznámení se odesílají tooa konkrétní telefonní číslo nebo e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="7760e-104">Rate limiting is a suspension of notifications that occurs when too many notifications are sent tooa particular phone number or email address.</span></span> <span data-ttu-id="7760e-105">Omezení rychlosti zajistí výstrahy spravovat a možné použít.</span><span class="sxs-lookup"><span data-stu-id="7760e-105">Rate limiting ensures that alerts are manageable and actionable.</span></span>

<span data-ttu-id="7760e-106">Hello pravidla pro SMS a e-mailu jsou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="7760e-106">hello rules for SMS and email are hello same.</span></span> <span data-ttu-id="7760e-107">Prahová hodnota omezení míry Hello je:</span><span class="sxs-lookup"><span data-stu-id="7760e-107">hello rate limit threshold is:</span></span>

 - <span data-ttu-id="7760e-108">**SMS**: 10 zpráv za hodinu.</span><span class="sxs-lookup"><span data-stu-id="7760e-108">**SMS**: 10 messages in an hour.</span></span>
 - <span data-ttu-id="7760e-109">**E-mailu**: 100 zpráv za hodinu.</span><span class="sxs-lookup"><span data-stu-id="7760e-109">**Email**: 100 messages in an hour.</span></span>

## <a name="rate-limit-rules"></a><span data-ttu-id="7760e-110">Míra limit pravidla</span><span class="sxs-lookup"><span data-stu-id="7760e-110">Rate limit rules</span></span>
- <span data-ttu-id="7760e-111">Konkrétní telefonní číslo nebo e-mailu je míra při přijetí více zpráv, než prahová hodnota hello umožňuje omezené.</span><span class="sxs-lookup"><span data-stu-id="7760e-111">A particular phone number or email is rate limited when it receives more messages than hello threshold allows.</span></span>
- <span data-ttu-id="7760e-112">Telefonní číslo nebo e-mailu může být součástí akce skupin napříč mnoho odběrů.</span><span class="sxs-lookup"><span data-stu-id="7760e-112">A phone number or email can be part of action groups across many subscriptions.</span></span> <span data-ttu-id="7760e-113">Omezení rychlosti platí ve všech předplatných.</span><span class="sxs-lookup"><span data-stu-id="7760e-113">Rate limiting applies across all subscriptions.</span></span> <span data-ttu-id="7760e-114">Jakmile je dosaženo prahové hodnoty hello, platí i v případě, že jsou zpráv odesílány z více předplatných.</span><span class="sxs-lookup"><span data-stu-id="7760e-114">It applies as soon as hello threshold is reached, even if messages are sent from multiple subscriptions.</span></span>  
- <span data-ttu-id="7760e-115">Pokud telefonní číslo nebo e-mailu je míra omezené, další oznámení odesláno toocommunicate hello omezení rychlosti.</span><span class="sxs-lookup"><span data-stu-id="7760e-115">When a phone number or email is rate limited, an additional notification is sent toocommunicate hello rate limiting.</span></span> <span data-ttu-id="7760e-116">Hello oznámení, že stavy při hello míra omezení vyprší platnost.</span><span class="sxs-lookup"><span data-stu-id="7760e-116">hello notification states when hello rate limiting expires.</span></span>

## <a name="rate-limit-of-webhooks"></a><span data-ttu-id="7760e-117">Míra limit webhooky</span><span class="sxs-lookup"><span data-stu-id="7760e-117">Rate limit of webhooks</span></span> ##
<span data-ttu-id="7760e-118">Neexistuje žádné kurz omezení nastavené pro webhooky.</span><span class="sxs-lookup"><span data-stu-id="7760e-118">There is no rate limiting in place for webhooks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7760e-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7760e-119">Next steps</span></span> ##
* <span data-ttu-id="7760e-120">Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="7760e-120">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>
* <span data-ttu-id="7760e-121">Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak tooreceive výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7760e-121">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span>  
* <span data-ttu-id="7760e-122">Zjistěte, jak příliš[Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="7760e-122">Learn how too[configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
