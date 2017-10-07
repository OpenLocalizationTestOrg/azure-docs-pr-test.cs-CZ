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
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a>Míra, omezení pro zprávy SMS, e-mailů a webhooku příspěvcích
Omezení rychlosti je pozastavení oznámení, která nastane po příliš mnoho oznámení se odesílají tooa konkrétní telefonní číslo nebo e-mailovou adresu. Omezení rychlosti zajistí výstrahy spravovat a možné použít.

Hello pravidla pro SMS a e-mailu jsou hello stejné. Prahová hodnota omezení míry Hello je:

 - **SMS**: 10 zpráv za hodinu.
 - **E-mailu**: 100 zpráv za hodinu.

## <a name="rate-limit-rules"></a>Míra limit pravidla
- Konkrétní telefonní číslo nebo e-mailu je míra při přijetí více zpráv, než prahová hodnota hello umožňuje omezené.
- Telefonní číslo nebo e-mailu může být součástí akce skupin napříč mnoho odběrů. Omezení rychlosti platí ve všech předplatných. Jakmile je dosaženo prahové hodnoty hello, platí i v případě, že jsou zpráv odesílány z více předplatných.  
- Pokud telefonní číslo nebo e-mailu je míra omezené, další oznámení odesláno toocommunicate hello omezení rychlosti. Hello oznámení, že stavy při hello míra omezení vyprší platnost.

## <a name="rate-limit-of-webhooks"></a>Míra limit webhooky ##
Neexistuje žádné kurz omezení nastavené pro webhooky.

## <a name="next-steps"></a>Další kroky ##
* Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).
* Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak tooreceive výstrahy.  
* Zjistěte, jak příliš[Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).
