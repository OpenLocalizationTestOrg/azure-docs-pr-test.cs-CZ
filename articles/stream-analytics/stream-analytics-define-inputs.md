---
title: "Datové připojení: Data stream vstupy z datového proudu událostí | Microsoft Docs"
description: "Informace o nastavení data připojení tooStream Analytics názvem \"vstupy\". Vstupy zahrnout datový proud z událostí a také referenční data."
keywords: "datový proud, datové připojení, datového proudu událostí"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 8155823c-9dd8-4a6b-8393-34452d299b68
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/05/2017
ms.author: samacha
ms.openlocfilehash: be2008f159061c5c9be9d0314c27fa67193e3269
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-connection-learn-about-data-stream-inputs-from-events-toostream-analytics"></a>Datové připojení: Další informace o data vstupy datového proudu z události tooStream Analytics
Hello úlohy Stream Analytics tooa datového připojení je datový proud událostí ze zdroje dat, což je úloha hello odkazované tooas *vstupní*. Stream Analytics obsahuje prvotřídní integrace s datového proudu zdrojů dat Azure, včetně [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/), a [úložiště objektů Azure Blob](https://azure.microsoft.com/services/storage/blobs/). Tyto vstupních zdrojů může být z hello stejné předplatné jako váš úlohy analytics nebo z jiného předplatného.

## <a name="data-input-types-data-stream-and-reference-data"></a>Vstupní typy dat: Data streamu a referenčních dat
Jako data odesílána tooa zdroj dat, má spotřebovávají úlohy služby Stream Analytics hello a zpracovány v reálném čase. Vstupy se dál dělí na dva typy: stream vstupy a referenční data vstupy dat.

### <a name="data-stream-inputs"></a>Vstupy datový proud dat
Datový proud je už bez vazby pořadí událostí v čase. Úlohy Stream Analytics musí obsahovat alespoň jeden vstup datového streamu. Služba Event Hubs, IoT Hub a úložiště objektů Blob jsou podporovány jako vstupní zdroje dat datového proudu. Centra událostí jsou použité toocollect streamů událostí z více zařízení a služeb. Tyto datové proudy mohou zahrnovat sociálních médií o aktivitách, informace o uložených obchodní nebo dat ze senzorů. Centra IoT jsou optimalizované toocollect data z připojených zařízení v scénáře Internetu věcí (IoT).  Úložiště objektů BLOB můžete použít jako vstupní zdroj pro příjem dat hromadné jako datový proud, jako jsou například soubory protokolu.  

### <a name="reference-data"></a>Referenční data
Stream Analytics podporuje také označuje jako vstup *referenční data*. Toto je pomocná data, která je buď statickou nebo který změny pomalu. Se obvykle používá k provedení korelaci a vyhledávání. Například můžete spojit data hello data datového proudu vstupní toodata v hello referenční data, tak, jak můžete provést toolook připojení k SQL statických hodnot. Azure Blob storage je aktuálně podporuje jenom hello vstupní zdroj pro referenční data. Referenční data zdroje BLOB jsou omezené velikost too100 MB.

jak zjistit, toocreate referenční data vstupy, toolearn [použití referenčních dat](stream-analytics-use-reference-data.md).  

## <a name="create-data-stream-input-from-event-hubs"></a>Vytvoření vstupní datový proud dat ze služby Event Hubs

Azure Event Hubs poskytuje vysoce škálovatelné publikování a odběru ingestors událostí. Centra událostí můžete shromáždit miliony událostí za sekundu, takže umožňuje zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello. Služba Event Hubs a Stream Analytics společně poskytují-komplexní řešení pro analýzu v reálném čase – Event Hubs umožňují kanálu události do Azure v reálném čase a úlohy Stream Analytics může zpracovat události v reálném čase. Například může odesílat klikne na web, odečty snímačů nebo online protokolu události tooEvent rozbočovače. Potom můžete vytvořit toouse úlohy Stream Analytics Event Hubs jako vstupní datové proudy hello v reálném čase filtrování, agregace a korelace.

Hello výchozí časové razítko události pocházející ze služby Event Hubs v Stream Analytics je hello časové razítko, které hello událostí byly přijaty ve hello centra událostí, což je `EventEnqueuedUtcTime`. tooprocess hello data jako datový proud použití časového razítka v datové části události hello, je nutné použít hello [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) – klíčové slovo.

### <a name="consumer-groups"></a>Skupiny příjemců
Vstupní toohave každý Stream Analytics události rozbočovače byste měli nakonfigurovat vlastní skupiny příjemců. Pokud úloha obsahuje vlastní spojení nebo pokud obsahuje více vstupů, může číst některé vstup za více než jeden čtečky. Situace má dopad na hello počet čtenářů v skupinu jednoho příjemce. tooavoid přesahující hello Event Hubs limitu pěti čtečky za skupiny příjemců na oddíl, je nejlepší postup toodesignate příjemce skupiny pro každou úlohu služby Stream Analytics. Je také maximální počet 20 skupiny příjemců za centra událostí. Další informace najdete v tématu [Průvodce programováním centra událostí](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-an-event-hub-as-a-data-stream-input"></a>Konfigurace centra událostí jako vstupní datový proud
Hello následující tabulka popisuje každou vlastnost v hello **nové vstup** okno v hello portálu Azure při konfiguraci centra událostí jako vstup.

| Vlastnost | Popis |
| --- | --- |
| **Vstupní alias** |Popisný název, který používáte v tooreference dotazu úlohy hello tento vstup. |
| **Obor názvů sběrnice** |Názvů Azure Service Bus, který je kontejner sady entit pro zasílání zpráv. Při vytváření nového centra událostí vytvořit taky obor názvů sběrnice. |
| **Název centra událostí** |Název Hello toouse centra událostí hello jako vstup. |
| **Název zásady centra událostí** |Hello zásady sdíleného přístupu, která poskytuje přístup k Centru událostí toohello. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. |
| **Skupina uživatelů centra událostí** (volitelné) |data tooingest Hello příjemce skupiny toouse z centra událostí hello. Pokud není zadána žádná skupina příjemců, úloha Stream Analytics hello používá hello výchozí skupina příjemců. Doporučujeme vám, že používáte skupinu odlišné příjemce pro každou úlohu služby Stream Analytics. |
| **Formát serializace událostí** |Hello formát serializace (JSON, CSV nebo Avro) hello příchozí datový proud. |
| **Kódování** | Znakové sady UTF-8 je aktuálně hello podporuje jenom kódování formátu. |

Pokud vaše data pochází z centra událostí, máte přístup toohello následující pole metadat v dotazu Stream Analytics:

| Vlastnost | Popis |
| --- | --- |
| **EventProcessedUtcTime** |Hello datum a čas, že událost hello zpracování Stream Analytics. |
| **EventEnqueuedUtcTime** |Hello datum a čas, že událost hello přijala Event Hubs. |
| **ID oddílu** |ID Hello nule oddílu pro vstupní adaptér hello. |

Například pomocí těchto polí, které můžete vytvořit dotaz, jako hello následující ukázka:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-data-stream-input-from-iot-hub"></a>Vytvoření vstupní datový proud dat ze služby IoT Hub
Azure Iot Hub je vysoce škálovatelná publikování a odběru na přijímač událostí optimalizovaných pro scénáře IoT.

Hello výchozí časové razítko události pocházející ze služby IoT hub v Stream Analytics je hello časové razítko, které hello událostí byly přijaty ve hello IoT hub, což je `EventEnqueuedUtcTime`. tooprocess hello data jako datový proud použití časového razítka v datové části události hello, je nutné použít hello [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) – klíčové slovo.

> [!NOTE]
> Pouze zprávy odeslané s `DeviceClient` vlastnost může být zpracována.
> 
> 

### <a name="consumer-groups"></a>Skupiny příjemců
Každý Stream Analytics IoT hub vstupní toohave byste měli nakonfigurovat vlastní skupiny příjemců. Pokud úloha obsahuje vlastní spojení nebo pokud obsahuje více vstupů, může číst některé vstup za více než jeden čtečky. Situace má dopad na hello počet čtenářů v skupinu jednoho příjemce. tooavoid přesahující hello Azure IoT Hub limitu pěti čtečky za skupiny příjemců na oddíl, je nejlepší postup toodesignate příjemce skupiny pro každou úlohu služby Stream Analytics.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Konfigurace služby IoT hub jako vstupní datový proud
Hello následující tabulka popisuje každou vlastnost v hello **nové vstup** okno v hello portálu Azure při konfiguraci služby IoT hub jako vstup.

| Vlastnost | Popis |
| --- | --- |
| **Vstupní alias** |Popisný název, který používáte v tooreference dotazu úlohy hello tento vstup.|
| **Centrum IoT** |Název Hello hello IoT hub toouse jako vstup. |
| **Koncový bod** |Hello koncový bod centra IoT hello.|
| **Název zásady sdíleného přístupu** |Hello sdílené zásady přístupu, který poskytuje přístup toohello IoT hub. Každá zásada sdíleného přístupu má název, že je nastavená oprávnění a přístupové klíče. |
| **Sdílený přístupový klíč zásad** |sdílený přístupový klíč Hello používá tooauthorize přístup toohello IoT hub. |
| **Skupiny příjemců** (volitelné) |data tooingest Hello příjemce skupiny toouse ze služby IoT hub hello. Pokud není zadána žádná skupina příjemců, úloha Stream Analytics používá hello výchozí skupina příjemců. Doporučujeme vám, že používáte skupinu jiný příjemce pro každou úlohu služby Stream Analytics. |
| **Formát serializace událostí** |Hello formát serializace (JSON, CSV nebo Avro) hello příchozí datový proud. |
| **Kódování** |Znakové sady UTF-8 je aktuálně hello podporuje jenom kódování formátu. |

Vaše data vycházejí ze služby IoT hub, máte přístup toohello následující pole metadat v dotazu Stream Analytics:

| Vlastnost | Popis |
| --- | --- |
| **EventProcessedUtcTime** |Hello datum a čas, že událost hello byl zpracován. |
| **EventEnqueuedUtcTime** |Hello datum a čas, že událost hello byl přijat službou hello IoT hub. |
| **ID oddílu** |ID Hello nule oddílu pro vstupní adaptér hello. |
| **IoTHub.MessageId** | ID, které se používá toocorrelate obousměrné komunikace ve IoT hub. |
| **IoTHub.CorrelationId** |ID, které se používá v odpovědi na zprávy a zpětnou vazbu týkající se služby IoT hub. |
| **IoTHub.ConnectionDeviceId** |ID ověření Hello používá toosend tuto zprávu. Tato hodnota je označený na servicebound zprávy službou hello IoT hub. |
| **IoTHub.ConnectionDeviceGenerationId** |ID generování Hello hello ověření zařízení, která byla použité toosend tuto zprávu. Tato hodnota je označený na servicebound zprávy službou hello IoT hub. |
| **IoTHub.EnqueuedTime** |Hello čas, kdy byla zpráva hello přijal službou hello IoT hub. |
| **IoTHub.StreamId** |Vlastní události vlastnost přidal hello odesílatele zařízení. |


## <a name="create-data-stream-input-from-blob-storage"></a>Vytvoření vstupní datový proud dat z úložiště objektů Blob
Pro scénáře s velkým množstvím nestrukturovaných dat toostore v cloudu hello úložiště objektů Azure Blob nabízí cenově výhodné a škálovatelné řešení. Data v úložišti objektů Blob se obvykle považuje dat v klidovém stavu. Však může být zpracována jako datový proud podle Stream Analytics. Typický scénář v nástroji vstupy úložiště objektů Blob s Stream Analytics je zpracování protokolu. V tomto scénáři zachycení telemetrická data ze systému a musí toobe analyzovat a zpracovaná tooextract smysluplný data.

Hello výchozí časové razítko událostí úložiště objektů Blob v Stream Analytics není hello časové razítko, které hello blob poslední změny, což je `BlobLastModifiedUtcTime`. tooprocess hello data jako datový proud použití časového razítka v datové části události hello, je nutné použít hello [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) – klíčové slovo.

Sdílený svazek clusteru naformátovaný vstupy *vyžadují* toodefine řádek záhlaví pole pro hello datovou sadu. Kromě toho všechna pole záhlaví řádků musí být jedinečný.

> [!NOTE]
> Stream Analytics nepodporuje přidávání obsahu tooan existující objekt blob. Objekt blob služby Stream Analytics se zobrazí pouze jednou a všechny změny, ke kterým došlo v objektu blob hello po hello úlohy má číst hello data nejsou předmětem zpracování. Osvědčeným postupem je tooupload všechna data hello jednou a není přidat úložišti objektů blob toothat události.
> 

### <a name="configure-blob-storage-as-a-data-stream-input"></a>Konfigurace úložiště objektů Blob jako vstupní datový proud

Hello následující tabulka popisuje každou vlastnost v hello **nové vstup** okno v hello portálu Azure při konfiguraci úložiště objektů Blob jako vstup.

| Vlastnost | Popis |
| --- | --- |
| **Vstupní alias** | Popisný název, který používáte v tooreference dotazu úlohy hello tento vstup. |
| **Účet úložiště** | Hello název účtu úložiště hello, kde jsou umístěny soubory hello objektů blob. |
| **Klíče účtu úložiště.** | Hello tajný klíč přidružený účet úložiště hello. |
| **Kontejner** | Hello kontejner pro vstup hello objektů blob. Kontejnery poskytují možnost logického seskupování pro objekty BLOB uložené v hello služby Microsoft Azure Blob. Při nahrávání toohello objektu blob služby Azure Blob storage, je nutné zadat kontejner pro tento objekt blob. |
| **Vzorek cesty** (volitelné) | Cesta k souboru Hello používá objekty BLOB hello toolocate v rámci zadaného kontejneru hello. V cestě hello, můžete zadat jednu nebo více instancí následujících tří proměnných hello: `{date}`, `{time}`, nebo`{partition}`<br/><br/>Příklad 1:`cluster1/logs/{date}/{time}/{partition}`<br/><br/>Příklad 2:`cluster1/logs/{date}`<br/><br/>Hello `*` znak není povolená hodnota pro předponu cesta hello. Jediné platné <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">objektů blob v Azure znaků</a> jsou povoleny. |
| **Formát data** (volitelné) | Pokud použijete proměnnou hello datum v cestě hello, hello formát data, ve které hello soubory jsou uspořádány. Příklad:`YYYY/MM/DD` |
| **Formát času** (volitelné) |  Pokud použijete proměnnou hello čas v cestě hello, hello formát času, ve které hello soubory jsou uspořádány. Aktuálně podporuje jenom hello je `HH`. |
| **Formát serializace událostí** | Hello formát serializace (JSON, CSV nebo Avro) pro příchozí datové proudy. |
| **Kódování** | Pro sdílený svazek clusteru a JSON je UTF-8 aktuálně hello podporuje jenom kódování formátu. |

Vaše data pocházejí z zdroje úložiště objektů Blob, máte přístup toohello následující pole metadat v dotazu Stream Analytics:

| Vlastnost | Popis |
| --- | --- |
| **BlobName** |Hello název vstupního objektu blob hello, který hello událost pochází. |
| **EventProcessedUtcTime** |Hello datum a čas, že událost hello zpracování Stream Analytics. |
| **BlobLastModifiedUtcTime** |Hello datum a čas tomuto objektu blob hello bylo naposledy změněno. |
| **ID oddílu** |ID Hello nule oddílu pro vstupní adaptér hello. |

Například pomocí těchto polí, které můžete vytvořit dotaz, jako hello následující ukázka:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````

## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
Když jste se naučili o možnosti připojení dat v Azure pro úlohy Stream Analytics. toolearn Další informace o Stream Analytics, najdete v části:

* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
