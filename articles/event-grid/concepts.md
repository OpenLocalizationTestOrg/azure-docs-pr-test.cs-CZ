---
title: "Koncepty aaaAzure událostí mřížky"
description: "Popisuje mřížky událostí Azure a jeho koncepty. Definuje několik klíčových součástí událostí mřížky."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/10/2017
ms.author: babanisa
ms.openlocfilehash: bb86381330fd2d6d2769167ec1953f0d5c28af85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="concepts-in-azure-event-grid"></a>Koncepty v mřížce Azure událostí

hlavní koncepty Hello v mřížce událostí Azure jsou:

## <a name="events"></a>Události

Událost je nejmenší velikost hello informace popisující plně něco kterým došlo v systému hello.  Každý událost má běžné informace, jako jsou: Zdroj události hello, čas hello událostí trvalo místní a jedinečný identifikátor.  Každé události má také konkrétní informace, které jsou pouze relevantní toohello konkrétní události. Například událost o nový soubor vytváří ve službě Azure Storage obsahuje podrobnosti o hello souboru, jako je hodnota lastTimeModified hello. Nebo události o virtuální počítač restartování obsahuje název hello hello virtuálního počítače a hello důvod pro restartování.

## <a name="event-sourcespublishers"></a>Zdroje nebo zdroje událostí

Zdroje událostí je, kde se stane hello událostí. Azure Storage je například hello zdroj události pro objekt blob vytvoření události. Prostředky infrastruktury Azure virtuálního počítače je hello zdroj události pro události se virtuální počítač. Zdroje událostí jsou zodpovědní za publikování události tooEvent mřížky.

## <a name="topics"></a>Témata

Vydavatelé zařadit do témata události. Hello téma obsahuje koncový bod kde hello vydavatele odesílá události. toorespond toocertain typy událostí, Odběratelé, kteří rozhodněte, které toosubscribe témata na. Témata také poskytují schématu události, aby odběratelé můžete zjistit, jak tooconsume hello události správně.

Témata týkající se systému jsou předdefinované témata poskytovaném službami Azure. Vlastní témata jsou aplikace a témata třetích stran.

## <a name="event-subscriptions"></a>Odběry událostí

Předplatné dá pokyn událostí mřížky, na které události se na téma má zájem o přijetí odběratele.  Předplatné také obsahuje informace o události by měl způsob doručení toohello odběratele.

## <a name="event-handlers"></a>Obslužné rutiny událostí

Obslužné rutiny události z hlediska mřížce událostí je hello místě, kde hello událost je odeslána. Obslužná rutina Hello nastaví některé další událost hello tooprocess akce.  Událost mřížky podporuje více typů odběratele. V závislosti na typu hello odběratele následuje událostí mřížky různé mechanismy tooguarantee hello doručení hello události.  Pro obslužné rutiny událostí webhooku protokolu HTTP, je hello událostí opakovat, dokud hello obslužná rutina vrátí stavový kód `200 – OK`. Pro fronty Azure Storage jsou události hello opakovat, dokud hello fronty služby je možné toosuccessfully proces hello zpráv nabízené do fronty hello.

## <a name="filters"></a>Filtry

Při přihlášení k odběru tooa téma, můžete filtrovat hello události, které se odesílají toohello koncový bod. Můžete filtrovat podle typu události nebo vzor subjektu. Další informace najdete v tématu [schématu odběru událostí mřížky](subscription-creation-schema.md).

## <a name="security"></a>Zabezpečení

Událost poskytuje zabezpečení pro přihlášení k odběru tootopics a publikování témata. Při přihlášení k odběru, musí mít odpovídající oprávnění na hello prostředků nebo téma. Při publikování, musíte mít tokenu SAS nebo ověření pomocí klíče pro téma hello. Další informace najdete v tématu [mřížky událostí zabezpečení a ověřování](security-authentication.md).

## <a name="failed-delivery"></a>Neúspěšné doručení

Když mřížky událostí nelze potvrdit, že událost byla přijata bodem hello odběratele, redelivers hello událostí. Další informace najdete v tématu [doručení zpráv událostí mřížky a zkuste to znovu](delivery-and-retry.md).

## <a name="next-steps"></a>Další kroky

* TooEvent Úvod mřížky, najdete v části [o mřížky událostí](overview.md).
* tooquickly začít používat mřížky událostí najdete v tématu [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).
