---
title: "Použít referenční data a vyhledávací tabulky v Stream Analytics | Microsoft Docs"
description: "Použití referenčních dat v dotazu Stream Analytics"
keywords: "vyhledávací tabulky, referenční data"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 438ec565f3c6e06ab7ec92cf1bbfbdde88f99b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Pomocí referenční data nebo vyhledávací tabulky v datovém proudu vstupní Stream Analytics
Referenční data (také označované jako vyhledávací tabulky) je omezené datovou sadou, která je statická nebo zpomalení změna ve své podstatě, použít k vyhledávání nebo ke korelaci s datového proudu. Chcete-li použít referenčních dat ve vaší úloze Azure Stream Analytics, obvykle použijete [referenční datové připojení](https://msdn.microsoft.com/library/azure/dn949258.aspx) v dotazu. Stream Analytics používá úložiště objektů Blob v Azure jako vrstva úložiště pro referenční Data, a s odkazem na objekt pro vytváření dat Azure data transformovat a zkopíruje do úložiště objektů Blob v Azure, použít jako referenční Data z [libovolný počet založené na cloudu a místní úložiště dat](../data-factory/copy-activity-overview.md). Referenční data je modelovaná jako pořadí objektů BLOB (definovanou v konfiguraci vstupní) ve vzestupném pořadí podle data a času, zadaný v názvu objektu blob. Ho **pouze** podporuje přidání na konec pořadí pomocí data a času **větší** než určenému poslední objektů blob v pořadí.

Stream Analytics má **limit 100 MB na objekt blob** ale úloh může zpracovat více objektů BLOB odkaz pomocí **vzorek cesty** vlastnost.


## <a name="configuring-reference-data"></a>Konfigurace referenční data
Ke konfiguraci referenční data, musíte nejprve vytvořit vstup typu **referenční Data**. Následující tabulka vysvětluje každou vlastnost, která budete muset zadat při vytváření referenční data, zadejte jeho popis:


<table>
<tbody>
<tr>
<td>Název vlastnosti</td>
<td>Popis</td>
</tr>
<tr>
<td>Vstupní Alias</td>
<td>Popisný název, který se použije v dotazu úlohy, které chcete-li tento vstup.</td>
</tr>
<tr>
<td>Účet úložiště</td>
<td>Název účtu úložiště, kde se nachází objektů BLOB. Pokud je ve stejném předplatném jako vaše úlohy Stream Analytics, můžete ji vyberte z rozevíracího seznamu.</td>
</tr>
<tr>
<td>Klíče účtu úložiště.</td>
<td>Tajný klíč přidružený k účtu úložiště. Automaticky načíst to získá hodnotu, pokud je účet úložiště ve stejném předplatném jako vaše úloha Stream Analytics.</td>
</tr>
<tr>
<td>Kontejner úložiště</td>
<td>Kontejnery poskytují možnost logického seskupování pro objekty BLOB uložené ve službě Microsoft Azure Blob. Při nahrávání do objektu blob ve službě Blob, je nutné zadat kontejner pro tento objekt blob.</td>
</tr>
<tr>
<td>Vzorek cesty</td>
<td>Cesta používaná k nalezení objektů BLOB v rámci zadaného kontejneru. V této cestě můžete určit jeden nebo více instancí 2 následující proměnné:<BR>{date}, {time}<BR>Příklad 1: products/{date}/{time}/product-list.csv<BR>Příklad 2: products/{date}/product-list.csv
</tr>
<tr>
<td>[Nepovinné] formát data</td>
<td>Pokud jste použili {date} v rámci vzorek cesty, který jste zadali, můžete vybrat formát data, ve kterém jsou uspořádány objektů blob z rozevíracího seznamu podporovaných formátů.<BR>Příklad: Rrrr/MM/DD/MM/DD/RRRR, atd.</td>
</tr>
<tr>
<td>Formát času [Nepovinné]</td>
<td>Pokud jste použili {time} v rámci vzorek cesty, který jste zadali, můžete vybrat formát času, ve kterém jsou uspořádány objektů blob z rozevíracího seznamu podporovaných formátů.<BR>Příklad: HH, HH/mm nebo HH mm</td>
</tr>
<tr>
<td>Formát serializace událostí</td>
<td>Abyste měli jistotu, že vaše dotazy fungovaly podle očekávání, potřebuje Stream Analytics vědět, který formát serializace používáte pro příchozí streamy. Pro referenční Data jsou podporovány následující formáty sdíleného svazku clusteru a JSON.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Znakové sady UTF-8 v tuto chvíli je jediným podporovaným formátem kódování</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generování referenční data podle plánu
Pokud referenční data pomalu změna datové sady, potom podpora pro obnovení odkaz, který dat je povolené zadáním vzorek cesty v vstupní konfigurace pomocí {date} a {time} nahrazení tokenů. Stream Analytics převezme definice aktualizovaná referenční data na základě vzoru pro tuto cestu. Například vzorec `sample/{date}/{time}/products.csv` s formátem data **"Rrrr-MM-DD"** a formát času z **"HH-mm"** dá pokyn Stream Analytics a pokračovat tam aktualizovaný objekt blob `sample/2015-04-16/17-30/products.csv` v 17:30:00 v dubnu 16 , Časové pásmo UTC 2015.

> [!NOTE]
> Aktuálně úlohy Stream Analytics vyhledejte aktualizace objektu blob jenom v případě, že čas počítače přejde na čas kódovaný v názvu objektu blob. Například bude hledat úlohu `sample/2015-04-16/17-30/products.csv` také možné, ale ne starší než 5:30 PM 16 duben 2015 UTC Čas zóny. Zruší *nikdy* vyhledejte objekt blob se starší než poslední ten, který zjišťuje kódovaného čas.
> 
> Například Jakmile úloha nalezne objekt blob `sample/2015-04-16/17-30/products.csv` se bude ignorovat všechny soubory s kódovaného data starší než 5:30 PM 16 duben 2015 takže pokud pozdní přicházejících `sample/2015-04-16/17-25/products.csv` objekt blob se vytvoří ve stejném kontejneru úlohy je používat.
> 
> Podobně pokud `sample/2015-04-16/17-30/products.csv` pouze vytváří ve 10:03 16 duben 2015, ale žádný objekt blob s stavu se nachází v kontejneru, úloha použije tento soubor spouští ve 10:03 16 duben 2015 a použít předchozí referenční data do té doby.
> 
> Výjimkou je při úlohu potřeba znovu zpracovat data zpět v čase, nebo po první úlohy spustit. Při spuštění úlohy hledá nejnovější blob vytvořeného než určený čas spuštění úlohy. To slouží k zkontrolujte, zda je **neprázdný** referenční datové sady při spuštění úlohy. Pokud nelze nalézt jeden, úloha zobrazí následující Diagnostika: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) můžete použít k orchestrování úlohy vytvoření aktualizované objekty BLOB, potřebuje Stream Analytics, aby aktualizace definic referenční data. Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje přesouvání a transformaci dat. Objekt pro vytváření dat podporuje [připojení velký počet cloudu na základě a místní úložiště dat](../data-factory/copy-activity-overview.md) a snadno přesouvání dat v pravidelných intervalech, který určíte. Další informace a podrobné pokyny o tom, jak nastavit kanál služby Data Factory ke generování referenčních dat pro Stream Analytics, která aktualizuje podle předdefinovaného plánu, podívejte se na to [Githubu ukázka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tipy k aktualizaci referenční data
1. Přepsání referenční data objektů BLOB nezpůsobí Stream Analytics načtením objektu blob a v některých případech může to způsobit selhání úlohy. Doporučeným způsobem, jak změnit referenční data je přidejte do nového objektu blob pomocí stejného vzoru kontejneru a cestu definované ve vstupu úlohy a pomocí data a času **větší** než určenému poslední objektů blob v pořadí.
2. Objekty BLOB referenční data jsou **není** seřadí čas objektu blob "Poslední změny", ale pouze pomocí data a času zadaný v objektu blob název pomocí {date} a {time} nahrazení.
3. V několika případech úlohu musí vrátit v čase, proto nesmí být referenční data objektů BLOB nemění ani neodstraňují.

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
Přečetli jste si úvod do služby Stream Analytics, spravované služby pro analýzy datových proudů z platformy Internet věcí. Další informace o této službě najdete v následujících článcích:

* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
