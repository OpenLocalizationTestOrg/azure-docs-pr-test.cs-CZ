---
title: "aaaUse referenční data a vyhledávací tabulky v Stream Analytics | Microsoft Docs"
description: "Použití referenčních dat v dotazu Stream Analytics"
keywords: "vyhledávací tabulky, referenční data"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: fb1d18fba920db5e097d0c95d333e8e8390d1589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Pomocí referenční data nebo vyhledávací tabulky v datovém proudu vstupní Stream Analytics
Referenční data (také označované jako vyhledávací tabulky) je omezené datovou sadou, která je statický nebo zpomalení změna ve své podstatě, použít tooperform lookup nebo toocorrelate s datový proud. toomake použití referenčních dat ve vaší úloze Azure Stream Analytics, obvykle použijete [referenční datové připojení](https://msdn.microsoft.com/library/azure/dn949258.aspx) v dotazu. Stream Analytics používá úložiště objektů Blob v Azure jako vrstva hello úložiště pro referenční Data a s odkazem na objekt pro vytváření dat Azure data mohou být transformovaná nebo zkopírovaný tooAzure úložiště objektů Blob, použít jako referenční Data z [libovolný počet založené na cloudu a místní úložiště dat](../data-factory/data-factory-data-movement-activities.md). Referenční data je modelovaná jako pořadí objektů BLOB (definovanou v konfiguraci vstupní hello) ve vzestupném pořadí hello datum a čas zadaný v názvu objektu blob hello. Ho **pouze** podporuje přidání toohello konec hello pořadí pomocí data a času **větší** než jeden určeného hello poslední objektů blob v pořadí hello hello.

Stream Analytics má **limit 100 MB na jednu blob** ale úloh může zpracovat více objektů BLOB odkaz pomocí hello **vzorek cesty** vlastnost.


## <a name="configuring-reference-data"></a>Konfigurace referenční data
tooconfigure referenční data, musíte nejprve toocreate vstup typu **referenční Data**. Následující tabulka Hello vysvětluje každou vlastnost, že budete potřebovat tooprovide při vytváření hello referenčního datového vstupu s jeho popis:


<table>
<tbody>
<tr>
<td>Název vlastnosti</td>
<td>Popis</td>
</tr>
<tr>
<td>Vstupní Alias</td>
<td>Popisný název, který se použije v tooreference dotazu úlohy hello tento vstup.</td>
</tr>
<tr>
<td>Účet úložiště</td>
<td>Hello název účtu úložiště hello, kde se nachází objektů BLOB. Pokud se nachází v hello stejného předplatného jako vaše úlohy Stream Analytics, můžete ji vybrat z rozevíracího seznamu hello.</td>
</tr>
<tr>
<td>Klíče účtu úložiště.</td>
<td>Hello tajný klíč přidružený účet úložiště hello. To získá automaticky vyplněný, když je účet úložiště hello v hello stejnému předplatnému jako úlohu služby Stream Analytics.</td>
</tr>
<tr>
<td>Kontejner úložiště</td>
<td>Kontejnery poskytují možnost logického seskupování pro objekty BLOB uložené v hello služby Microsoft Azure Blob. Při nahrávání toohello objektu blob služby objektů Blob, je nutné zadat kontejner pro tento objekt blob.</td>
</tr>
<tr>
<td>Vzorek cesty</td>
<td>Cesta Hello používá toolocate objektů BLOB v hello zadaného kontejneru. V cestě hello můžete zvolit jednu nebo více instancí hello následující 2 proměnné toospecify:<BR>{date}, {time}<BR>Příklad 1: products/{date}/{time}/product-list.csv<BR>Příklad 2: products/{date}/product-list.csv
</tr>
<tr>
<td>[Nepovinné] formát data</td>
<td>Pokud jste použili {date} v rámci hello vzorek cesty, který jste zadali, můžete vybrat formát data hello, ve kterém jsou uspořádány objektů blob z rozevíracího seznamu hello podporovaných formátů.<BR>Příklad: Rrrr/MM/DD/MM/DD/RRRR, atd.</td>
</tr>
<tr>
<td>Formát času [Nepovinné]</td>
<td>Pokud jste použili {time} v rámci hello vzorek cesty, který jste zadali, můžete vybrat formát času hello, ve kterém jsou uspořádány objektů blob z rozevíracího seznamu hello podporovaných formátů.<BR>Příklad: HH, HH/mm nebo HH mm</td>
</tr>
<tr>
<td>Formát serializace událostí</td>
<td>toomake se, že dotazy fungovaly hello očekávaným způsobem, Stream Analytics musí tooknow, který formát serializace, který používáte pro příchozí streamy. Pro referenční Data hello podporované formáty jsou sdíleného svazku clusteru a JSON.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Znakové sady UTF-8 je hello pouze v současné době podporovaný formát kódování</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generování referenční data podle plánu
Pokud referenční data pomalu změna datové sady, je povolena podpora aktualizace referenční data zadáním vzorek cesty v hello vstupní konfiguraci pomocí hello {date} a {time} nahrazení tokeny. Stream Analytics převezme definice dat hello aktualizovat odkaz na základě vzoru pro tuto cestu. Například vzorec `sample/{date}/{time}/products.csv` s formátem data **"Rrrr-MM-DD"** a formát času z **"HH-mm"** dá pokyn toopick Stream Analytics se objekt blob hello aktualizovat `sample/2015-04-16/17-30/products.csv` v 17:30:00 v dubnu. 16, 2015 časovém pásmu UTC.

> [!NOTE]
> Aktuálně úlohy Stream Analytics vyhledejte aktualizace objektu blob hello jenom v případě, že čas počítače hello přejde toohello čas v název objektu blob hello kódování. Například bude hledat hello úlohy `sample/2015-04-16/17-30/products.csv` také možné, ale ne starší než 5:30 PM 16 duben 2015 UTC Čas zóny. Zruší *nikdy* vyhledejte objekt blob se dříve, než hello naposledy, který je zjištěn kódovaného čas.
> 
> Například Po hello úlohy vyhledá hello blob `sample/2015-04-16/17-30/products.csv` se bude ignorovat všechny soubory s kódovaného data starší než 5:30 PM 16 duben 2015 takže pokud pozdní přicházejících `sample/2015-04-16/17-25/products.csv` objekt blob se vytvoří v hello stejné úlohy hello kontejneru je používat.
> 
> Podobně pokud `sample/2015-04-16/17-30/products.csv` pouze vytváří ve 10:03 16 duben 2015, ale žádný objekt blob s stavu se nachází v kontejneru hello, hello úlohy použije tento soubor spouští ve 10:03 16 duben 2015 a používání referenčních dat. předchozí hello do té doby.
> 
> K výjimce toothis je při hello úlohy musí toore zpracování dat zpět v čase nebo při prvním spuštění úlohy hello. Při spuštění úlohy hello čas hledá poslední vytvořil před hello úlohy počáteční čas zadaný objekt blob v hello. Děje se tak tooensure, který je **neprázdný** referenční datové sady při spuštění úlohy hello. Pokud nelze nalézt jeden, hello úlohy zobrazí hello následující diagnostiky: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) lze použít tooorchestrate hello úlohy vytvoření objektů BLOB hello aktualizovat vyžaduje Stream Analytics tooupdate referenční data definice. Objekt pro vytváření dat je služba pro integraci dat založené na cloudu, která orchestruje a automatizuje hello přesouvání a transformaci dat. Objekt pro vytváření dat podporuje [připojení tooa velký počet cloudové a místní úložiště dat](../data-factory/data-factory-data-movement-activities.md) a snadno přesouvání dat v pravidelných intervalech, který určíte. Další informace a pokyny krok za krokem k jak tooset až objekt pro vytváření dat kanál toogenerate referenční data pro analýzu datového proudu, který aktualizuje podle předdefinovaného plánu, podívejte se na to [Githubu ukázka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tipy k aktualizaci referenční data
1. Přepsání referenční data objektů BLOB nezpůsobí Stream Analytics tooreload hello blob a v některých případech může to způsobit toofail úlohy hello. Hello doporučená způsob toochange referenčních dat je tooadd nového objektu blob pomocí stejného vzoru kontejneru a cestu definované ve vstupu úlohy hello hello a používat data a času **větší** než jeden určeného hello poslední objektů blob v pořadí hello hello.
2. Objekty BLOB referenční data jsou **není** seřazené čas "Poslední změny" hello blob, ale pouze data a času, zadaný v názvu objektu blob hello pomocí hello {date} a {time} náhrady hello.
3. V několika případech úlohu musí vrátit v čase, proto nesmí být referenční data objektů BLOB nemění ani neodstraňují.

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
Byli jste přináší tooStream Analytics, spravované služby pro analýzy z hello Internet věcí datových proudů. toolearn Další informace o této služby najdete v části:

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
