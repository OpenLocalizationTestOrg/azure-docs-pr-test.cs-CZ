---
title: "aaaHow tooconfigure data vstupů pro úlohy Stream Analytics | Microsoft Docs"
description: "Nakonfigurujte výstupy pro úlohy Stream Analytics | učení segmentu cesty."
keywords: "data výstupu přesun dat"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 3bbea3da-bfce-4af1-a15e-d4b23874034f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/26/2017
ms.author: samacha
ms.openlocfilehash: c5d89e9e9f9211d3e778580c071dd53d56aed9fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-outputs-for-stream-analytics-jobs"></a>Jak se tooconfigure data vstupů pro úlohy Stream Analytics

Úlohy Azure Stream Analytics může být připojené tooone nebo další výstupy dat, které definují existující data podřízený tooan připojení. Úlohu Stream Analytics procesy a transformuje příchozích dat, proud událostí výstupní data je zapsán výstup tooyour úlohy.

Stream Analytics data výstupy může být v reálném čase řídicí panely použité toosource nebo výstrahy, pracovní postupy přesun dat aktivační události nebo jednoduše archivu data pro pozdější zpracování dávky. Stream Analytics obsahuje prvotřídní integrace s několik služeb Azure, které jsou popsány zde podrobně.

tooadd úlohu služby Stream Analytics tooyour výstup:

1. V hello [portál Azure](https://portal.azure.com), spusťte úlohu a klikněte na tlačítko **výstupy** a pak klikněte na **přidat** v zobrazeném okně výstupy hello.
   
    ![Přidejte výstupy](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. Zadejte popisný název pro tento výstup v hello **Alias pro výstup** pole. Tento název dá vaše úloha dotaz později na toorefer toohello výstup.  
   
    Vyplňte hello rest hello vyžaduje připojení vlastnosti tooconnect tooyour výstupu.  Tato pole se liší podle typu výstupu a jsou definovány zde podrobně.  
   
    ![Výběr typu přesunu dat](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. V závislosti na typu výstup hello může být nutné toospecify jak hello data serializovaná nebo formátu. nastavení Hello konkrétní serializace pro každý typ výstupu jsou popsané v tomto poli.
   
    Vyplňte hello rest hello požadované připojení vlastnosti zdroje dat tooconnect tooyour. Tato pole se liší podle typu vstupu a zdroj typu a jsou definovány v podrobně hello [vytvoření úlohy článku](stream-analytics-create-a-job.md).  

> [!Note]
>
> Všechny úlohy přidané toohello výstup elementu musí existovat hello úloha se spustila a události spuštění toku. Například pokud použijete jako výstupu úložiště objektů Blob, úlohy hello nebude vytvořit účet úložiště automaticky. Potřebuje toobe vytvořené uživatelem hello před zahájení úlohy ASA hello.
> 
 

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

