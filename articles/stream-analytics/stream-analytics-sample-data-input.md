---
title: "vstupy vzorkování AAA v Azure Stream Analytics | Microsoft Docs"
description: "Přesně stanovit problémy při řešení potíží s úlohy Stream Analytics."
keywords: "řešení potíží s vstupní, vstupní vzorkování"
documentationcenter: 
services: stream-analytics
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
ms.openlocfilehash: 9637a8664de099eebb8f5654036d2957f4c6b7ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-input-stream-sampling"></a>Vzorkování vstupu stream Azure Stream Analytics

Pomocí Azure Stream Analytics můžete zkusit vstupních událostech, které pocházejí ze souboru a testování dotazů hello portálu bez nutnosti toostart nebo zastavení úlohy.

## <a name="testing-your-query"></a>Testování dotazu

V podokně podrobností úlohy Stream Analytics hello otevřete hello **editor dotazů** tak, že kliknete na název dotazu hello pod **dotazu**. (V našem ukázkový scénář, protože zatím nebyla vytvořena žádná dotazu, klikněte na tlačítko hello **< >** zástupný symbol.)

![editor dotazů Hello Stream Analytics](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

stejně jako tomu bylo v předchozí verzi hello, zobrazí se okno Hello bohatě vybavený editor pro vytvoření dotazu. Teď hello okno byl aktualizován na novou levé podokno, že zobrazuje hello vstupy a výstupy, které jsou používané hello dotazu a definované pro tuto úlohu.

![editor dotazů Stream Analytics Hello vstup a výstupy seznamy](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

Rovněž jsou uvedeny další vstupu a výstupu, které nejsou definovány. Pocházejí ze hello novou dotazu šablonu, kterou spustíte s. Změňte, nebo i zmizí, jak upravit dotaz hello. Můžete bez obav ignorovat je teď.

Klikněte pravým tlačítkem na vstupy tootest s ukázkovými daty vstupní a potom vyberte **nahrát ukázková data ze souboru**.

![Hello Stream Analytics dotazu editor nahrávání ukázková data z soubor – příkaz](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

Po dokončení nahrávání hello klikněte na tlačítko **Test** tootest tento dotaz hello vzorová data, které jste právě zadali.

![tlačítko Testovat editor dotazů Stream Analytics Hello](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

Pokud chcete výstup testu hello toosave pro pozdější použití, hello výstup tohoto dotazu se zobrazí v prohlížeči hello s výsledky stahování toohello a odkaz. Teď můžete snadno a interaktivně upravte dotaz a otestovat ji opakovaně toosee jak výstup hello změny.

![Stream Analytics query editor ukázkový výstup](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

V hello předcházející bitové kopie, byl přidán druhý výstup, nazývá **HighAvgTempOutput**.

Použijete-li několik výstupů v dotazu, můžete zobrazit hello výsledky pro každý výstup samostatně a snadno přepínat mezi nimi.

Jakmile budete spokojeni s výsledky hello, můžete uložit dotazu, spustit úlohu, sledujte a sledovat magické číslo hello Stream Analytics.

## <a name="get-help"></a>Podpora

Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
