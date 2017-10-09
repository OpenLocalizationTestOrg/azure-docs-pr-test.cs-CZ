---
title: "aaaHow toostart streamování úloh v Stream Analytics | Microsoft Docs"
description: "Jak spustit úlohu streamování v Azure Stream Analytics | učení segmentu cesty."
keywords: "Úloha streamování"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 67aa14860c38cbd0535d0ec4f23729445d0185c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-streaming-job-in-azure-stream-analytics"></a>Jak toorun streamování úlohy Azure Stream Analytics
Při vstupu úlohy, dotaz a výstup všech nebyly zadány, že můžete spustit úlohy služby Stream Analytics hello.

toostart vaše úlohy:

1. V portálu Azure Classic hello z řídicího panelu úloha hello, klikněte na **spustit** v hello dolní části stránky hello.
   
   ![Spuštění úlohy tlačítko](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   V hello portálu Azure, klikněte na **spustit** v horní části hello stránky úlohy.
   
   ![Úloha spuštění Azure portálu tlačítko](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Zadejte **spustit výstup** toodetermine hodnotu, pokud se tato úloha spustí vytváření výstupu. Hello výchozí nastavení pro úlohy, které nebyly dříve spuštěna je **čas spuštění úlohy**, což znamená, že úlohy hello okamžitě začne zpracování dat. Můžete také zadat **vlastní** čas v hello minulosti (pro použití historických dat) nebo budoucí hello (toodelay zpracování dokud datum v budoucnosti). Pro možnost hello případech, kdy úloha má dříve spuštění a zastavení, **naposledy Zastaveno** je k dispozici v pořadí tooresume hello úlohu z hello čas poslední výstup a nedošlo ke ztrátě dat..  
   
   ![Počáteční čas úlohy streamování](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portálu spustit úlohu streamování čas](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Potvrďte výběr. Stav úlohy Hello změní příliš*počáteční* a krátce přesune příliš*systémem* po zahájení úlohy hello. Můžete sledovat průběh hello hello **spustit** operace v hello **centra oznámení**:
   
   ![průběh úlohy streamování](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Portál Azure průběh úlohy streamování](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

