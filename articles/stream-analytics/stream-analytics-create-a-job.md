---
title: "aaaHow toocreate zpracování úlohy analýzy datového pro Stream Analytics | Microsoft Docs"
description: "Vytvoření úlohy zpracování dat analytics pro Stream Analytics | učení segmentu cesty."
keywords: "zpracování analýzy dat"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: d4a3c89d8862d59688d06a1719b063efa2ab1c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-data-analytics-processing-job-for-stream-analytics"></a>Jak toocreate zpracování analýzy data úlohy pro Stream Analytics
Hello nejvyšší úrovně prostředků v Azure Stream Analytics je úlohy Stream Analytics.  Se skládá z jednoho nebo více vstupních zdrojů dat, dotaz vyjadřující hello transformaci dat a jeden nebo více cílů výstup, zapsaných do výsledků. Společně tyto povolit hello tooperform data analýzy chování uživatelů zpracování dat scénáře.

toostart pomocí Stream Analytics, začněte tím, že vytváření nové úlohy Stream Analytics.  Všimněte si, že tato akce nemá žádné fakturace dopady až po zahájení úlohy hello.

1. Přihlaste se na hello online [portál Azure classic](http://manage.windowsazure.com) nebo hello [portál Azure](https://portal.azure.com/).
2. Portálu hello: **klikněte na tlačítko Nový**, pak klikněte na tlačítko **datové služby** nebo **Data Analytics** v závislosti na portál a pak klikněte na tlačítko **Azure Stream Analytics** nebo **Stream Analytics** a potom **rychle vytvořit**.
   
   ![Průvodce úlohy zpracování analýzy dat](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Vytvoření analýzy dat, zpracování úlohy](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. Zadejte požadovanou konfiguraci úlohy služby Stream Analytics hello hello.
   
   * V hello **název úlohy** zadejte úloha Stream Analytics název tooidentify hello. Když hello **název úlohy** byl ověřen, v poli Název úlohy hello se zobrazí zelená značka zaškrtnutí. Hello **název úlohy** může obsahovat pouze alfanumerické znaky a hello '-' znak a musí být v rozmezí 3 až 63 znaků.
   * Použití **oblast** v hello portál Azure nebo **umístění** v hello Azure portálu toospecify hello zeměpisného umístění, kam chcete toorun hello úlohy.
   * Pokud pomocí hello portálu Azure, vyberte nebo vytvořte toouse účtu úložiště jako hello **místní monitorování účtu úložiště**. Tento účet úložiště je použité toostore monitorování data pro všechny úlohy služby Stream Analytics, které jsou spuštěné v této oblasti.
   * Pokud pomocí hello portálu Azure, zadejte nový nebo existující **skupiny prostředků** toohold související prostředky pro vaši aplikaci.
4. Po nakonfigurování hello nové možnosti úlohy Stream Analytics, klikněte na tlačítko **vytvořit úlohy Stream Analytics**. Může trvat několik minut, než toobe úlohy Stream Analytics hello vytvořili. toocheck hello stav, můžete sledovat průběh hello v centru oznámení hello.
   
   ![Analýza dat, zpracování centrem oznámení úlohy](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Vytvoření úlohy úlohy Azure portálu Data analytics zpracování](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. Nová úloha Hello se zobrazí stav **vytvořená**. Všimněte si, že hello **spustit** tlačítko k dispozici. Před zahájením hello úlohy konfigurace hello úlohy vstup, dotaz a výstup.
   
   ![Úlohy zpracování analýzy dat stavu](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portálu analýzy dat zpracování stavu úlohy](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

