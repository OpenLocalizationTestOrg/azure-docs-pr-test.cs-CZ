---
title: "aktivační události opakování hello aaaAdd do aplikace logiky | Microsoft Docs"
description: "Přehled hello opakování aktivační události a jak toouse se službou Azure logiku aplikace."
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
ms.openlocfilehash: e7c625c382a88a1e7cdfff4ddc0caf55727232bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-recurrence-trigger"></a>Začínáme s aktivační událost opakování hello
Pomocí aktivační hello opakování, můžete vytvořit výkonné pracovní postupy v cloudu hello.

Můžete například provést následující věci:

* Plánování pracovního postupu toorun uloženou proceduru SQL každý den.
* E-mailu souhrn všech tweetů v rámci hello minulého týdne o určitých hashtag.

tooget spuštěna v aktivační události opakování hello aplikace logiky, najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Použít aktivační událost opakování
Aktivační událost je událost, která lze použít toostart hello workflow, který je definován v aplikaci logiky. [Další informace o aktivační události](connectors-overview.md).

Tady je příklad posloupnosti jak aktivovat tooset až opakování v aplikaci logiky:

1. Přidat hello **opakování** aktivační událost jako první krok hello v aplikaci logiky.
2. Vyplňte hello parametry pro interval opakování hello.

aplikace logiky Hello spustí spustit po každé časové období.

![Trigger HTTP](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Podrobnosti o aktivační události
aktivační událost Hello opakování má následující vlastnosti, které můžete konfigurovat hello.

Vyvolá aplikace logiky po zadaném časovém intervalu.
A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Frekvence * |frequency |jednotka času Hello: `Second`, `Minute`, `Hour`, `Day`, nebo `Year`. |
| Interval * |interval |interval Hello hello zadané frekvence opakování hello. |
| Časové pásmo |Časové pásmo |Pokud je čas spuštění zadaný bez časový posun, použije se toto časové pásmo. |
| Čas spuštění |startTime |Hello počáteční čas v [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). |

<br>

## <a name="next-steps"></a>Další kroky
Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Můžete prozkoumat hello dalších dostupných konektorů v aplikacích logiky pohledem na našem [rozhraní API seznamu](apis-list.md).

