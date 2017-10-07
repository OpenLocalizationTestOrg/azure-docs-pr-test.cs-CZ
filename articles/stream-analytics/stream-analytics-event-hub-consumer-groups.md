---
title: "aaaDebug Azure Stream Analytics s příjemců u centra událostí | Microsoft Docs"
description: "Osvědčené postupy pro Event Hubs skupiny uživatelů v úlohy Stream Analytics využívá dotaz."
keywords: "limit centra událostí, skupiny uživatelů"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 89821e6273151de43b5e42d907e547c939e24824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a>Ladění Azure Stream Analytics se příjemců u centra událostí

Azure Event Hubs můžete použít v Azure Stream Analytics tooingest nebo výstupní data z úlohy. Doporučený postup pro použití služby Event Hubs je toouse více skupiny příjemců, škálovatelnost tooensure úlohy. Jedním z důvodů je, že hello počet čtenářů v úloze Stream Analytics hello pro specifický vstup ovlivňuje hello počet čtenářů v skupinu jednoho příjemce. Hello přesné počet příjemců je založen na interní implementace podrobnosti pro logiku hello topologie Škálováním na více systémů. Hello počet příjemců nebude vystavena, externě. Hello počet čtenářů, můžete změnit na čas spuštění úlohy hello nebo během upgradů úlohy.

> [!NOTE]
> Při změně hello počet čtenářů během upgradu úlohy se zapisují protokoly tooaudit přechodný upozornění. Úlohy Stream Analytics automaticky zotavit tyto přechodné problémy.

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Počet čtenářů na oddíl překračuje limit služby Event Hubs pět

Scénáře, ve které hello počet čtenářů na oddíl překračuje limit služby Event Hubs hello pět zahrnout hello následující:

* Více příkazů SELECT: Pokud používáte více příkazů SELECT, které odkazují příliš**stejné** vstupní centra událostí, každý příkaz SELECT způsobí, že nový toobe příjemce vytvořili.
* SJEDNOCENÍ: Pokud používáte spojení, je možné toohave více vstupů, které odkazují toohello **stejné** skupina rozbočovače a příjemce událostí.
* SPOJENÍ sama: Pokud použijete operaci SAMOOBSLUŽNÉ připojení, je možné toorefer toohello **stejné** centra událostí vícekrát.

## <a name="solution"></a>Řešení

Hello následující osvědčené postupy může pomoci zmírnit scénáře, ve které hello počet čtenářů na oddíl překračuje limit služby Event Hubs hello pět.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>Rozdělením více kroků dotazu pomocí klauzule WITH

klauzule WITH Hello určuje je sada výsledků dotazu dočasné s názvem, který lze odkazovat pomocí klauzule FROM v dotazu hello. Klauzule WITH hello definujete v oboru provádění hello jednoho příkazu SELECT.

Například místo tento dotaz:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Pomocí tohoto dotazu:

```
WITH input (
   SELECT * FROM inputEventHub
) as data

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-toodifferent-consumer-groups"></a>Ujistěte se, že vstupy vázat toodifferent skupiny uživatelů

Pro dotazy, ve kterých jsou tři nebo více vstupů připojené toohello stejné služby Event Hubs příjemce skupiny, vytvořit skupiny samostatné příjemců. To vyžaduje hello vytvoření další Stream Analytics vstupy.


## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooStream Analytics](stream-analytics-introduction.md)
* [Začínáme s Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka jazyka dotazů Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics správu odkazu k REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
