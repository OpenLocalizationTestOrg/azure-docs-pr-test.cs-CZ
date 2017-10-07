---
title: "aaaUnderstanding monitorování úlohy Stream Analytics | Microsoft Docs"
description: "Principy sledování úlohy Stream Analytics"
keywords: "monitorování dotazu"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 36819025c7b2ddbf4b9694522f1b4820407ca5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-toomonitor-queries"></a>Pochopení monitorování úlohy Stream Analytics a jak toomonitor dotazy

## <a name="introduction-hello-monitor-page"></a>Úvod: stránka sledování hello
Hello portálu Azure i surface klíčových metrik, které je možné použít toomonitor a řešení potíží s výkon dotazů a úlohy. toosee tyto metriky procházet úlohy služby Stream Analytics toohello vás zajímá zobrazení metriky pro a zobrazení hello **monitorování** části na stránce Přehled hello.  

![Monitorování odkaz](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

jak je znázorněno, zobrazí se okno Hello:

![Monitorování úlohy řídicí panel](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Metriky, které jsou k dispozici pro Stream Analytics
| Metrika                 | Definice                               |
| ---------------------- | ---------------------------------------- |
| % Využití SU       | Hello využití hello jednotka streamování přiřadit tooa úlohu z karty škálování hello hello úlohy. Tento ukazatel přístup 80 % nebo výše, je vysoká pravděpodobnost, že zpracování událostí může být zpoždění nebo zastavená, provedení průběh. |
| Vstupní události           | Množství dat přijatých hello úlohy služby Stream Analytics v počet událostí. To může být použité toovalidate, že události jsou odesílány toohello vstupní zdroj. |
| Výstup události          | Množství dat odesílaných hello Stream Analytics úlohy toohello výstup cíl, v počet událostí. |
| Události mimo pořadí    | Počet událostí přijatých mimo pořadí, které byly vyřazeny nebo zadané upravené časové razítko podle hello zásady řazení událostí. To může mít vliv hello konfigurace nastavení interval Tolerance pořádku hello. |
| Chyby při převodu dat | Počet chyb při převodu dat způsobené úloha Stream Analytics. |
| Chyby za běhu         | Hello celkový počet chyb, které dojít během provádění úlohy Stream Analytics. |
| Pozdní vstupní události      | Počet událostí přicházejících pozdní ze hello zdroj, který buď byla vyřazena nebo jejich časové razítko bylo změněno, v závislosti na konfiguraci zásady řazení událostí hello hello pozdní interval Tolerance dodání nastavení. |
| Požadavky funkce      | Počet volání toohello funkce Azure Machine Learning (pokud existuje). |
| Požadavky neúspěšné funkce | Počet selhání volání funkce Azure Machine Learning (pokud existuje). |
| Události – funkce        | Počet událostí odeslaných funkce Azure Machine Learning toohello (pokud existuje). |
| Vstupní událost bajtů      | Množství dat přijatých hello úlohy služby Stream Analytics v bajtech. To může být použité toovalidate, že události jsou odesílány toohello vstupní zdroj. |


## <a name="customizing-monitoring-in-hello-azure-portal"></a>Přizpůsobení sledování v hello portálu Azure
Můžete upravit hello typ grafu, metriky zobrazené a čas rozsah v nastavení upravit graf hello. Podrobnosti najdete v tématu [jak tooCustomize monitorování](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Graf sledování času dotazu](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

