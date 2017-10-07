---
title: "aaaAzure mřížky události doručení a zkuste to znovu"
description: "Popisuje, jak Azure událostí mřížky doručí události a jak zpracovává nedoručených zpráv."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.openlocfilehash: 874b3bf8892fbf803ef40f29d0ec10eb50150916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-message-delivery-and-retry"></a>Doručení zpráv událostí mřížky a zkuste to znovu 

Tento článek popisuje, jak Azure událostí mřížky zpracovává události, když není potvrdí doručení.

Událost mřížky poskytuje trvanlivý doručení. Zajišťuje každou zprávu alespoň jednou pro každé předplatné. Události se posílají toohello zaregistrován webhooku každého předplatného okamžitě. Webhook, jehož není potvrdil přijetí události do 60 sekund od první doručení hello pokus, události mřížky opakování doručení hello události.

## <a name="message-delivery-status"></a>Stav doručení zpráv

Událost mřížky používá HTTP odpovědi kódy tooacknowledge příjmu událostí. 

### <a name="success-codes"></a>Kódy úspěch

Hello následující kódy odpovědi HTTP znamenat, že událost byla doručena úspěšně tooyour webhooku. Událost mřížky zvažuje doručení dokončení.

- 200 OK
- 202 platných

### <a name="failure-codes"></a>Selhání kódy

Hello následující kódy odpovědi HTTP znamenat, že události doručení pokus se nezdařil. Mřížky událostí se pokusí znovu toosend hello událostí. 

- 400 – Chybný požadavek
- 401 unauthorized
- 404 – Nenalezeno
- 408 časový limit požadavku.
- 414 URI příliš dlouhý
- 500 vnitřní chybu serveru
- 503 Služba nedostupná
- Vypršel časový limit 504 brány

Další kód odpovědi nebo nedostatek odpověď znamená chybu. Událost mřížky opakuje doručení. 

## <a name="retry-intervals"></a>Opakujte intervaly

Událost mřížky používá zásady opakování exponenciálního omezení rychlosti pro odeslání události. Pokud vaše webhooku neodpovídá nebo vrací kód chyby, opakování mřížky události doručení na hello následující plánu:

1. 10 sekund.
2. 30 sekund
3. 1 minuta
4. 5 minut
5. 10 minut
6. 30 minut
7. 1 hodina

Událost mřížky přidá malé náhodné přeskupování tooall opakování intervalech.

## <a name="retry-duration"></a>Opakujte doba trvání

Během hello preview vyprší mřížky událostí Azure všechny události, které nejsou doručeny během dvou hodin. Před zveřejněním tentokrát bude vyšší too24 hodin. 

## <a name="next-steps"></a>Další kroky

* TooEvent Úvod mřížky, najdete v části [o mřížky událostí](overview.md).
* tooquickly začít používat mřížky událostí najdete v tématu [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).
