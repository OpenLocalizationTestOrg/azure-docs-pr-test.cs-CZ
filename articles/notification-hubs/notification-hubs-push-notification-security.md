---
title: "aaaSecurity pro centra oznámení"
description: "Toto téma vysvětluje zabezpečení služby Azure notification hubs."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f59ad4594c2c0a2e2b22ab0b6d6bad53825a4dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security"></a>Zabezpečení
## <a name="overview"></a>Přehled
Toto téma popisuje model zabezpečení hello Azure Notification hubs. Protože Notification Hubs entity služby sběrnice, implementace hello stejný model zabezpečení jako Service Bus. Další informace najdete v tématu hello [ověřování sběrnice služby](https://msdn.microsoft.com/library/azure/dn155925.aspx) témata.

## <a name="shared-access-signature-security-sas"></a>Zabezpečení sdílený přístupový podpis (SAS)
Notification Hubs implementuje zabezpečení na úrovni entit schéma volá SAS (sdíleného přístupového podpisu). Toto schéma umožňuje zasílání zpráv entity toodeclare až too12 autorizační pravidla v popisu, která udělují oprávnění na dané entity.

Každé pravidlo obsahuje název, hodnotu klíče (sdílený tajný klíč) a sada práv, jak je popsáno v části hello "Deklarací identity zabezpečení." Při vytváření centra oznámení, se automaticky vytvoří dvě pravidla: jednu s naslouchání práv (hello používá aplikace klienta) a jeden se všemi právy k (které hello používá back-end aplikace).

Při provádění správy registrace z klientské aplikace, pokud informace hello odeslána prostřednictvím oznámení není citlivé (například počasí aktualizace), běžné tooaccess způsob, jak Centrum oznámení je toogive hello hodnoty klíče toohello přístup jen naslouchání pravidlo hello klientské aplikace a toogive hello hodnoty klíče hello pravidlo úplný přístup toohello back-end aplikace.

Není doporučeno vložit hodnotu klíče hello klientské aplikace pro Windows Store. Způsob tooavoid, vkládání hodnota klíče hello je toohave hello klientskou aplikaci jejich načtení z back-end aplikace hello při spuštění.

Je důležité toounderstand, který hello klíč s přístupem k naslouchání umožňuje klientům aplikace tooregister žádné značky. Pokud aplikace třeba omezit registrace toospecific značky toospecific klientů (například když značky představují ID uživatele), musíte provést back-end aplikace hello registrace. Další informace najdete v tématu registrace správy. Všimněte si, že tímto způsobem hello klientské aplikace nebudou mít přímý přístup tooNotification rozbočovače.

## <a name="security-claims"></a>Deklarací identity zabezpečení
Podobně jako tooother entity, jsou povolené operace centra oznámení pro tři deklarací identity zabezpečení: naslouchání, odeslání a spravovat.

| Deklarovat | Popis | Povolené operace |
| --- | --- | --- |
| Naslouchání |Vytvořit nebo aktualizovat, čtení a odstranění jednoho registrace |Vytvořit nebo aktualizovat registraci<br><br>Registrace pro čtení<br><br>Číst všechny registrace pro popisovač<br><br>Odstranit registrace |
| Odeslat |Odeslání zprávy centra oznámení toohello |Odeslat zprávu |
| Spravovat |CRUDs v Notification Hubs (včetně aktualizace systému PNS přihlašovacích údajů a zabezpečení klíče) a čtení registrace podle značky |Vytvoření, aktualizace nebo pro čtení nebo odstranění notification hubs<br><br>Číst registrace podle značky |

Centra oznámení přijímat deklarace identity udělena tokeny řízení přístupu Microsoft Azure a tokeny podpis vygeneroval s sdílených klíčů nakonfigurovat přímo na hello centra oznámení.

