---
title: "aaaAdd zpoždění při aplikace logiky | Microsoft Docs"
description: "Přehled hello zpoždění a prodlevu – dokud akce a jak toouse je s Azure logiku aplikace."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e5bc9d639adbddc01ee0f6a4c68716f586d4344a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-delay-and-delay-until-actions"></a>Začínáme s hello zpoždění a prodlevu – dokud akce
Pomocí hello zpoždění a "zpoždění – dokud" akce, můžete dokončit scénáře pracovního postupu.

Můžete například provést následující věci:

* Počkejte, dokud toosend den v týdnu stav aktualizace prostřednictvím e-mailu.
* Pracovní postup hello zpoždění až volání protokolu HTTP má čas toofinish před obnovení a načítání hello výsledek.

tooget spuštění pomocí hello zpoždění akce v aplikaci logiky, aplikaci najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-delay-actions"></a>Pomocí akcí zpoždění hello
Akce je operace, která se provádí v pracovním postupu hello, která je definována v aplikaci logiky. [Další informace o akcích](connectors-overview.md).

Tady je příklad posloupnosti jak toouse zpoždění krok v aplikaci logiky:

1. Po přidání aktivační událost, klikněte na tlačítko **nový krok** tooadd akce.
2. Vyhledejte **zpoždění** toobring až hello zpoždění akce. V tomto příkladu jsme vyberte **zpoždění**.
   
    ![Zpoždění akce](./media/connectors-native-delay/using-action-1.png)
3. Dokončete všechny hello akce vlastnosti tooconfigure hello zpoždění.
   
    ![Konfigurace zpoždění](./media/connectors-native-delay/using-action-2.png)
4. Klikněte na tlačítko **Uložit** toopublish a aktivovat aplikaci logiky hello.

## <a name="action-details"></a>Podrobnosti akce
aktivační událost Hello opakování má následující vlastnosti, které lze nakonfigurovat hello.

### <a name="delay-action"></a>Zpoždění akce
Tato akce hello zpoždění spuštění pro určitý časový interval.
A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Počet * |Počet |Hello počet jednotek toodelay čas |
| Jednotka * |jednotka |jednotka času Hello: `Second`, `Minute`, `Hour`, nebo`Day` |

<br>

### <a name="delay-until-action"></a>Zpoždění – dokud akce
Tato akce zpozdí hello běžet, dokud zadané datum a čas.
A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Rok * |časové razítko |Hello roku toodelay dokud (GMT) |
| Měsíc * |časové razítko |Hello toodelay měsíce až (GMT) |
| Den * |časové razítko |Hello den toodelay dokud (GMT) |

<br>

## <a name="next-steps"></a>Další kroky
Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Můžete prozkoumat hello dalších dostupných konektorů v aplikacích logiky pohledem na našem [rozhraní API seznamu](apis-list.md).

