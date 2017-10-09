---
title: "testování dotazů aaaAzure Stream Analytics | Microsoft Docs"
description: "Jak tootest své dotazy v úlohy Stream Analytics."
keywords: "Test dotazu, řešení potíží s dotazu"
documentation center: 
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
ms.openlocfilehash: 3b141d98332fdc170e696e181c8446796a86f78e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-azure-stream-analytics-queries-in-hello-azure-portal"></a>Testování dotazů Azure Stream Analytics v hello portálu Azure

Pomocí služby Azure Stream Analytics můžete otestovat dotazy v hello portál Azure bez nutnosti toostart nebo zastavení úlohy.

## <a name="test-hello-input"></a>Test hello vstup

1. Klikněte pravým tlačítkem na vstupy tootest s ukázkovými daty vstupní a potom vyberte **nahrát ukázková data ze souboru**.

    ![Stream analytics dotaz editor testu dotazu](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Po dokončení nahrávání hello klikněte na tlačítko **Test** tootest tento dotaz hello ukázková data, která jste zadali.

    ![Dotaz služby Stream analytics editor testu ukázková data](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

Hello výstup tohoto dotazu se zobrazí v prohlížeči hello s výsledky odkaz ke stažení, budete chtít výstup testu hello toosave pro pozdější použití. Teď můžete snadno a interaktivně upravte dotaz a otestovat ji opakovaně toosee jak výstup hello změny.

![Stream Analytics query editor ukázkový výstup](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

S více výstupů použitých v dotazu můžete zobrazit výsledky hello pro obě výstupy samostatně a snadno přepínat mezi nimi.

Jakmile budete spokojeni s hello výsledky zobrazené v prohlížeči hello, můžete uložit dotazu, spusťte úlohu a nechat zpracovat události bez chyby.

## <a name="get-help"></a>Podpora

Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky

* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
