---
title: "aaaAdd dat vstupu úlohy Stream Analytics tooyour | Microsoft Docs"
description: "Zjistěte, jak toohook nahoru tooyour zdroje dat Stream Analytics úlohy jako streamování vstupní data ze služby Event Hubs nebo odkaz na data z úložiště blogu."
keywords: "data vstup, streamování dat"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: 
ms.assetid: 9e59bd24-2a80-4ecb-b6b2-309a07c70bcd
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 674000268fcdf9bc000af3e2f166cb66f1366922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-tooa-stream-analytics-job"></a>Přidat streamování dat vstup nebo referenční data tooa úloha Stream Analytics
Zjistěte, jak toohook nahoru tooyour zdroje dat Stream Analytics úlohy jako streamování vstupní data ze služby Event Hubs nebo odkaz na data z úložiště objektů Blob.

Úlohy Azure Stream Analytics může být připojené tooone vstupní data nebo informace, z nichž každý definujte připojení tooan existující zdroj dat. Protože data se odesílají toothat zdroj dat, je spotřebovávají úlohy služby Stream Analytics hello a zpracování v reálném čase jako datový proud. Stream Analytics obsahuje prvotřídní integrace s [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [úložiště objektů Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) v rámci i mimo ni hello úlohu odběru.

Tento článek je krok v hello [Stream Analytics studijní](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Vstupní data: streamování dat a referenčních dat
Existují dva odlišné typy vstupů ve Stream Analytics: datové proudy a referenční data.

* **Datové proudy**: úlohy Stream Analytics musí obsahovat alespoň jeden data datového proudu vstupní toobe spotřebované a transformovat úlohou hello. Azure Blob storage a Azure Event Hubs se podporují jako vstupní zdroje dat datového proudu. Azure Event Hubs je použité toocollect streamů událostí z připojených zařízení, služeb a aplikací. Azure Blob storage můžete použít jako vstupní zdroj pro příjem dat hromadné jako datový proud.  
* **Referenční data**: Stream Analytics podporuje druhého typu pomocného vstupní volané referenční data.  Jako názvem na rozdíl od toodata přesouvaných tato data jsou statická nebo zpomalení změna.  Se obvykle používá k provedení look-ups a korelací s toocreate datové proudy data širší datové sady.  Azure Blob storage je aktuálně podporuje jenom hello vstupní zdroj pro referenční data.  

úlohu Stream Analytics vstupní tooyour tooadd:

1. V hello portálu Azure klikněte na **vstupy** a pak klikněte na **přidat vstup** v úloze Stream Analytics.
   
    ![Portál Azure classic – přidat vstup.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    V hello portálu Azure klikněte na tlačítko hello **vstupy** dlaždici v úloze Stream Analytics.  
   
    ![Portál Azure – přidání datového vstupu.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. Zadejte typ hello hello vstupu: buď **datový proud** nebo **referenční data**.
   
    ![Přidání správná data hello vstup, streamování nebo odkaz](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Přidání správná data hello vstup, streamování nebo odkaz](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. Pokud vytváříte vstupní datový proud, zadejte hello typ zdroje pro vstup hello.  Během vytváření referenční Data jako jenom objekt Blob úložiště je podporována v tuto chvíli můžete tento krok přeskočen.
   
    ![Přidat vstup datového streamu dat](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Přidat vstup datového streamu dat](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. Zadejte popisný název pro tento vstupní v hello vstupní Alias pole.  Tento název se použije v dotazu vaše úlohy později na toorefer toohello vstup.
   
    Vyplňte hello rest hello požadované připojení vlastnosti zdroje dat tooconnect tooyour. Tato pole se liší podle typu vstupu a zdroj typu a jsou definovány podrobně [zde](stream-analytics-create-a-job.md).  
   
    ![Přidat vstup datového centra událostí](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. Zadejte nastavení hello serializace pro vstupní data hello:
   
   * aby dotazy fungovaly hello očekávaným způsobem, toomake zadejte hello **formát serializace událostí** příchozích dat.  Formáty podporované serializace jsou JSON, CSV a Avro.
   * Ověřte hello **kódování** pro hello data.  Znakové sady UTF-8 je hello pouze v současné době podporovaný formát kódování.
     
     ![Nastavení serializace dat pro vstupní data hello](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Nastavení serializace dat pro vstupní data hello](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. Po dokončení vytvoření vstupní hello, Stream Analytics ověří, zda se může připojit toohello vstupní zdroj.  Stav hello hello operace testování připojení můžete zobrazit v centru oznámení hello.
   
    ![Testovací připojení hello dat vstupních datových proudů](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![Testovací připojení hello dat vstupních datových proudů](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Získat pomoc s streamování vstupů dat.
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

