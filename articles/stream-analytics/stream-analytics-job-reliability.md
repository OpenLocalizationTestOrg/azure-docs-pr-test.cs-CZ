---
title: "Zabránilo přerušení služeb s úlohy Azure Stream Analytics | Microsoft Docs"
description: "Pokyny k provedení Stream Analytics úlohy upgradu odolný."
services: stream-analytics
documentationCenter: 
authors: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 3ac65c93ecb47e93e963dd9869a7af70f73b19c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Zajistit spolehlivost úlohy Stream Analytics během aktualizace služby

Součástí je plně spravovaná služba je hello schopností toointroduce nové služby funkce a vylepšení rychlé tempem. Stream Analytics v důsledku toho může mít aktualizaci služby nasazení na základě týdenní (nebo vyšší). Bez ohledu na to, kolik testování se provádí stále existuje riziko, že existující, spuštěné úlohy je možné ukončit kvůli toohello zavedení chyby. Pro zákazníky, kteří spustí kritické úlohy streamování zpracování těchto rizik potřebovat toobe vyhnout. Zákazníci mechanismus můžete použít tooreduce toto riziko je Azure  **[spárované oblast](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modelu. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Jak oblastí Azure spárované tuto situaci řešit?

Stream Analytics zaručuje, že úlohy v oblasti spárované se aktualizují odděleně. Mezi hello aktualizace potenciální narušující tooidentify chyb a opravit, je výsledkem dostatečný čas mezera.

_S výjimkou hello střed_ (jehož spárované oblast – Jih, Indie, nemá přítomnosti Stream Analytics), nasazení hello aktualizaci tooStream Analytics se nevyskytuje v hello stejný čas v sadě spárované oblasti. Nasazení v několika oblastech **v hello stejnou skupinu** může dojít, **v hello současně**.

článek Hello na  **[dostupnosti a oblastí, spárované](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  má hello nejnovější informace, na kterém jsou spárovat oblasti.

Zákazníci jsou doporučené toodeploy identické úlohy tooboth spárovat oblasti. Kromě toho tooStream Analytics interní možnosti zákazníkům monitorování jsou také doporučujeme úlohy hello toomonitor jako **obě** provozní úlohy. Pokud zalomení identifikovaných toobe výsledku hello aktualizace služby Stream Analytics, eskalovat správně a převzít žádný výstup podřízeného příjemci toohello pořádku úlohy. Eskalace toosupport bude zabránění spárované oblast hello ovlivnění hello nové nasazení a zachovat integritu hello hello spárovat úloh.
