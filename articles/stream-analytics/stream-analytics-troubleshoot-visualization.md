---
title: "aaaVisualize úloh služby Stream Analytics a odstraňování potíží | Microsoft Docs"
description: "Zjistěte, jak toovisualize úloha Stream Analytics kanálu pro samoobslužné řešení problémů pomocí funkce diagram hello diagnostiky."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 8a6715be601fdc47b8d9caf4112da161dad22618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Vizualizace a řešení potíží s úlohy Stream Analytics
V Stream Analytics stejně jako u jiných technologií cloudové řešení potíží s je někdy potřebné toolook do proč úlohu nevytváří hello očekávaný výstup (nebo žádný výstup k tomuto účelu). Myslete na to Stream Analytics poskytuje možnost hello pro vizualizaci úlohu streamování. Toto je také užitečné jako nástroj pro modelování a má hello straně benefit pro ty, které vyžaduje dokumentaci práci.

V hello vizualizaci jsou viditelné a také hello dotazu spouštěna vstupy hello panelu a pak nakonfigurovat všechny výstupy hello. Problémy s připojením nebo konfigurace se může stát více zřejmá a také může být užitečné toosee vizuální reprezentace vaší konfigurace.

## <a name="using-hello-diagnosis-diagram-tool"></a>Pomocí nástroje diagram diagnostiku hello
hello tooaccess hello tento vizualizér, jednoduše klikněte na tlačítko "Diagnostiku diagram" v okně "Nastavení" hello úlohy Stream Analytics hello.

![Stream-Analytics-troubleshoot-Visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Veškerý vstup a výstup je barevně tooindicate hello aktuální stav této součásti, jak je uvedeno níže.

![Stream-Analytics-troubleshoot-Visualization-Input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Pokud chce uživatel hello toolook na zprostředkující kroky toounderstand hello datového toku vzorům dotazů uvnitř úlohy, nástroj pro vizualizaci hello poskytuje zobrazení obsahuje rozpis hello hello dotazu do jeho součásti kroky a pořadí toku hello. Kliknutím na každého kroku dotazu se zobrazí odpovídající části hello v dotazu úpravy podokně podle pokynů. 

![Stream-Analytics-troubleshoot-Visualization-Intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

