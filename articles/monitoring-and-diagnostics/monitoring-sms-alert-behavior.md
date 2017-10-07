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
# <a name="sms-alert-behavior-in-action-groups"></a>SMS výstrahy chování ve skupinách, akce
## <a name="overview"></a>Přehled ##
Skupiny akcí povolit tooconfigure seznam příjemců. Tyto skupiny můžete využít pak při definování aktivity protokolu výstrah; zajištění, že skupinu konkrétní akce zasláno oznámení, když se aktivuje výstraha hello aktivity protokolu. Jeden z hello výstrahy mechanismy podporována je SMS; výstrahy Hello podporovat jednosměrnou komunikaci. Uživatel může reagovat tooan výstrahu:

- **Odběr upozornění:** uživatele můžete zrušit odběr všechny výstrahy služby SMS pro všechny skupiny akce nebo skupinu singulární akce.  
- **Obnovit předplatné tooalerts:** uživatele můžete obnovit předplatné tooall SMS výstrahy pro všechny skupiny akce nebo skupinu singulární akce.  
- **Požádat o pomoc:** uživatel může požádat o další informace o hello SMS. Budou přesměrovaného toothis článku

Tento článek se zabývá hello chování hello SMS výstrah a hello odpovědi akce hello uživatele můžete provádět na základě hello národního prostředí uživatele hello:

## <a name="usacanada-sms-behavior"></a>Chování USA nebo Kanady SMS
### <a name="receiving-an-sms-alert"></a>Příjem oznámení SMS
SMS příjemce, který je nakonfigurovaný jako součást skupiny akce, obdrží serveru služby SMS při aktivuje výstrahu. Hello SMS vyplní hello následující informace:
* Shortname hello akce skupiny byla odeslána tato výstraha
- Název výstrahy hello

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a>Odhlášení z SMS výstrahy pro jednu skupinu akce
Uživatele můžete zrušit odběr služby SMS pro výstrahy pro jednu akci skupinu podle toohello shortcode odpovídá 20873 s hello klíčová slova: "Zakázat &lt;Shortname akce skupiny&gt;".

Příklad: Hodně toounsubscribe z výstrah pro skupinu akce s shortname hello "Azure", uživatel by odesílat shortcode toohello zprávy služby SMS 20873, která uvádí "Zakázat Azure"

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a>Odhlášení z SMS výstrahy pro všechny skupiny akce
Uživatele můžete zrušit odběr všechny výstrahy služby SMS pro všechny skupiny akce podle toohello shortcode odpovídá 20873 s žádným z hello následující klíčová slova:
* STOP

Příklad: Uživatel hodně toounsubscribe z všechny výstrahy služby SMS pro všechny skupiny akce by odesílat shortcode toohello zprávy služby SMS 20873, která uvádí "STOP"

>[!NOTE]
>Pokud uživatel odhlásil(a) odběr z výstrah SMS, ale se pak přidá novou skupinu akce tooa; BUDE přijímat výstrahy služby SMS pro tuto novou skupinu akce ale zůstávají odhlásit ze všech skupin, předchozí akce.
>
>

### <a name="resubscribing-toosms-alerts-for-one-action-group"></a>Resubscribing tooSMS výstrahy pro jednu skupinu akce
Uživatele můžete obnovit předplatné tooSMS pro výstrahy pro jednu akci skupinu podle toohello shortcode odpovídá 20873 s hello klíčová slova: "Povolit &lt;Shortname akce skupiny&gt;".

Příklad: Hodně tooresubscribe tooalerts pro skupinu akce s shortname hello "Azure", uživatel by odesílat shortcode toohello zprávy služby SMS 20873, která uvádí "Povolit Azure"

### <a name="resubscribing-toosms-alerts-for-all-action-groups"></a>Resubscribing tooSMS výstrahy pro všechny skupiny akce
Uživatele můžete obnovit předplatné tooall SMS pro výstrahy pro všechny skupiny akce odpovídá toohello shortcode 20873 s žádným hello následující klíčová slova:

* SPUŠTĚNÍ

Příklad: Uživatel hodně toounsubscribe z všechny výstrahy služby SMS pro všechny skupiny akce by odesílat shortcode toohello zprávy služby SMS 20873, která uvádí "START"

### <a name="requesting-help-via-sms"></a>Žádost o nápovědu prostřednictvím serveru SMS
Další informace o hello SMS můžete požádat uživatele, že obdržely podle odpovídá toohello shortcode 20873 s žádným z hello následující klíčová slova:
* POMOC

Odpověď zašle toohello uživatele s článku toothis na odkaz.

## <a name="next-steps"></a>Další kroky
Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md) a zjistěte, jak tooget výstrahy  
Další informace o [omezení rychlosti SMS](monitoring-alerts-rate-limiting.md)  
Další informace o [skupiny akcí](monitoring-action-groups.md)
