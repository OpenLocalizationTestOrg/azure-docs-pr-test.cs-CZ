---
title: dotazy toowrite aaaHow v Stream Analytics | Microsoft Docs
description: "Zápis dotazů v Stream Analytics a dotazování dat | učení segmentu cesty."
keywords: "jak toowrite dotazy, dotaz na data, napsat dotaz, zápis dotazů"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: b943c34f10afd2b21789afbd341c471a5f168729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowrite-queries-in-stream-analytics"></a>Jak toowrite dotazy v Stream Analytics
Zápis dotazů pro zpracování logiky v Azure Stream Analytics datového proudu je implementovaný jako "stojící dotaz", který je definován před úloha spustí a u dat provést, protože nedosáhne hello úlohy. transformaci dat Hello je vyjádřenou v jazyce SQL jako dotaz, který je z velké části podmnožinou T-SQL s některými přidat jazyková rozšíření jako [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) použít dočasnou sémantiku tooexpress.

## <a name="writing-queries"></a>Zápis dotazů:
1. V úloze Stream Analytics na portálu pro správu Azure hello, klikněte na tlačítko **dotazu**.
   
    ![Vybrat dotaz](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    V hello portálu Azure, klikněte na **dotazu**.
   
    ![Vyberte Náhled dotazu](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. Mají nové úlohy dotaz toohelp šablony vám pomůžou začít. Hello dotazů šablony provede "předávací" dotaz této projekty všechna pole ze vstupních událostech do výstup hello.  
   
   * Pokud jste definovali alespoň jeden vstup a výstup pro úlohu, můžete nahradit hello zástupný symbol "[YourOutputAlias]" a pole "[YourInputAlias]" hello aliasy hello vstup a výstup nejprve chcete použít. Kromě toho můžete pořád vytvářet a testování dotazu v hello portálu Azure Classic bez definování vstupy a výstupy u úlohy hello.
   * Pokud chcete tooperform větším objemem zpracování než jednoduchý průchozí, můžete upravit definice dotazu hello. tooget začít s pro tvorbu dotazu, prohlédněte si některé běžný dotaz na zaznamenání vzory [zde](stream-analytics-stream-analytics-query-patterns.md).  
   
   ![Dotaz na data okna](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="toovalidate-query-data-is-working"></a>dotaz na data toovalidate funguje:
Můžete otestovat, zda dotaz chová podle očekávání spuštěním v prohlížeči hello přes jeden nebo více místní soubory JSON obsahující testovací data. To nebude spuštění úlohy hello nebo mají vliv na všechny fakturace.

> [!NOTE]
> Aktuálně testování dotazů v prohlížeči není podporována hello portálu Azure.  
> 
> 

1. Ujistěte se, že nejsou žádné chyby v dotazu hello (jinak hello tlačítko Testovat bude zakázáno) a potom klikněte na tlačítko Testovat hello.  
   
   ![Dotaz na data testu](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. Bude výzvami toospecify soubory pro každou z hello vstupů odkazovaná v dotazu hello. V tomto příkladu je ponechán hello šablony dotazu jako-se, takže hello dialogu je dotaz na vstup s názvem "yourinputalias".  
   
   ![Dotaz na Data testu](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. Procházejte tooa testovací soubor. Několik ukázkových souborů, které jsou k dispozici na [githubu](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) a můžete také načíst ukázková data z vlastního datového proudu vstupy data prostřednictvím hello funkce ukázková Data na kartě hello vstupy.  
   
   ![Vstupní dotaz](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. Po zavření dialogu hello, se spustí dotaz přes hello testovací data a zobrazí se hello výsledků v dolní části hello hello dotazu stránky.  
   
   ![Souhrn dotazu](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

