---
title: "Přidání aktivační událost opakování v aplikace logiky | Microsoft Docs"
description: "Přehled aktivační událost opakování a způsobu jeho použití s aplikací Azure logiku."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: fe558958c316c8dba42163e277ae01451f712e5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-recurrence-trigger"></a>Začínáme s aktivační událost opakování
Pomocí aktivační událost opakování, můžete vytvořit výkonné pracovní postupy v cloudu.

Můžete například provést následující věci:

* Naplánujte pracovní postup spustit uloženou proceduru SQL každý den.
* Během posledního týdne o určitých hashtag e-mailu souhrn všech tweetů.

Chcete-li začít používat aktivační událost opakování v aplikaci logiky, přečtěte si téma [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Použít aktivační událost opakování
Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu, který je definován v aplikaci logiky. [Další informace o aktivační události](connectors-overview.md).

Tady je v sekvenci příklad toho, jak nastavit opakování aktivační událost v aplikaci logiky:

1. Přidat **opakování** aktivační událost jako první krok v aplikaci logiky.
2. Zadejte parametry pro interval opakování.

Aplikace logiky spustí spustit po každé časové období.

![Trigger HTTP](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Podrobnosti o aktivační události
Aktivační událost opakování má následující vlastnosti, které můžete konfigurovat.

Vyvolá aplikace logiky po zadaném časovém intervalu.
A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Frekvence * |frekvence |Jednotka času: `Second`, `Minute`, `Hour`, `Day`, nebo `Year`. |
| Interval * |Interval |Interval dané frekvenci opakování. |
| Časové pásmo |Časové pásmo |Pokud je čas spuštění zadaný bez časový posun, použije se toto časové pásmo. |
| Čas spuštění |startTime |Čas zahájení v [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). |

<br>

## <a name="next-steps"></a>Další kroky
Teď vyzkoušet platformu a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Ostatní konektory k dispozici v aplikace logiky můžete prozkoumat pohledem na našem [rozhraní API seznamu](apis-list.md).

