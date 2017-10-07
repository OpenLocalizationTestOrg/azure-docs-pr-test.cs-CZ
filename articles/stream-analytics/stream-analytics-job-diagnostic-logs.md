---
title: "diagnostické protokoly aaaTroubleshoot Azure Stream Analytics | Microsoft Docs"
description: "Zjistěte, jak tooanalyze diagnostické protokoly ze služby Stream Analytics úloh ve službě Microsoft Azure."
keywords: 
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
ms.openlocfilehash: 600fce73636dd137f8f3a137f1d77b59b4a88801
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Řešení potíží s Azure Stream Analytics s použitím protokolů diagnostiky

V některých případech úlohu služby Azure Stream Analytics neočekávaně zastaví zpracování. Je důležité toobe možné tootroubleshoot tento druh události. Hello události může být způsobena výsledek neočekávaný dotaz, toodevices připojení nebo neočekávané služby výpadku. Hello protokolů diagnostiky v Stream Analytics vám může pomoct identifikovat hello příčinu problémů při jejich výskytu a zkrátit čas obnovení.

## <a name="log-types"></a>Typy protokolu

Stream Analytics poskytuje dva typy protokolů: 
* [Protokoly aktivity](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (je pořád zapnutý). Protokoly aktivity poskytují přehled o operace provedené na úlohách.
* [Diagnostické protokoly](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (Konfigurovat). Diagnostické protokoly nabízejí širší přehled vše, co se stane s úlohou. Diagnostické protokoly spuštění při vytvoření úlohy hello a koncové při odstranění úlohy hello. Při aktualizaci hello úlohy, a když je spuštěná pokrývaly události.

> [!NOTE]
> Můžete vytvořit služby, jako je Azure Storage, Azure Event Hubs a Azure Log Analytics tooanalyze neodpovídající data. Budou se vám účtovat podle hello cenový model pro tyto služby.
>

## <a name="turn-on-diagnostics-logs"></a>Zapnout diagnostické protokoly

Diagnostické protokoly jsou **vypnout** ve výchozím nastavení. tooturn na protokolů diagnostiky, proveďte tyto kroky:

1.  Přihlaste se toohello portál Azure a přejděte toohello streamování okno úlohy. V části **monitorování**, vyberte **protokolů diagnostiky**.

    ![Protokoly toodiagnostics navigačním okně](./media/stream-analytics-job-diagnostic-logs/image1.png)  

2.  Vyberte **zapněte diagnostiku**.

    ![Zapnout diagnostické protokoly](./media/stream-analytics-job-diagnostic-logs/image2.png)

3.  Na hello **nastavení diagnostiky** stránky, pro **stav**, vyberte **na**.

    ![Změnit stav protokolů diagnostiky](./media/stream-analytics-job-diagnostic-logs/image3.png)

4.  Hello archivace cíl (účet úložiště, centra událostí, analýzy protokolů), které chcete nastavte. Potom vyberte kategorie hello protokolů, které chcete toocollect (spuštění, vytváření obsahu). 

5.  Uložte novou konfiguraci diagnostiky hello.

Konfigurace diagnostiky Hello trvá asi 10 minut tootake vliv. Potom hello protokoly spuštění zobrazovaných v hello nakonfigurované archivace cíl (uvidíte tyto na hello **protokolů diagnostiky** stránky):

![Okno navigační toodiagnostics protokoly - archivace cíle](./media/stream-analytics-job-diagnostic-logs/image4.png)

Další informace o konfiguraci diagnostiky najdete v tématu [shromažďovat a využívat diagnostická data z vašich prostředků Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>Kategorie protokolu diagnostiky

V současné době jsme zaznamenat dvě kategorie protokolů diagnostiky:

* **Vytváření**. Záznamy protokolu událostí, které jsou související toojob vytváření operations: vytvoření úlohy, přidávání a odstraňování vstupy a výstupy, přidávání a aktualizaci hello dotazu, spuštění a zastavení hello úlohy.
* **Provádění**. Zachycuje události, ke kterým došlo během provádění úlohy:
    * K chybám připojení
    * Chyby zpracování dat, včetně:
        * Události, které nevyhovují toohello dotaz definice (neodpovídající pole typů a hodnot, chybějící pole atd.)
        * Chyby vyhodnocení výrazu
    * Ostatní události a chyby

## <a name="diagnostics-logs-schema"></a>Diagnostické protokoly schématu

Všechny protokoly se ukládají ve formátu JSON. Každá položka má následující běžné polí s řetězcem hello:

Name (Název) | Popis
------- | -------
time | Časové razítko (ve formátu UTC) hello protokolu.
resourceId | ID prostředku hello, který hello operace proběhly v, velkými písmeny. Obsahuje ID předplatného hello hello skupinu prostředků a název úlohy hello. Například   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB**.
category | Kategorie, buď protokolu **provádění** nebo **vytváření**.
operationName | Název hello operace, která je zaznamenána. Například **odesílat události: zápis výstupu SQL selhání toomysqloutput**.
status | Stav operace hello. Například **se nezdařilo** nebo **úspěšné**.
úroveň | Úroveň protokolu. Například **chyba**, **upozornění**, nebo **informační**.
properties | Podrobnosti konkrétní položky protokolu serializován jako řetězec formátu JSON. Další informace najdete v tématu hello následující části.

### <a name="execution-log-properties-schema"></a>Spuštění protokolu vlastnosti schématu

Protokoly spouštění mít informace o událostech, ke kterým došlo během provádění úlohy Stream Analytics. Hello schéma vlastnosti se liší v závislosti na typu hello události. V současné době máme hello následující typy protokoly spouštění:

### <a name="data-errors"></a>Data chyb

Všechny chyby, ke které dochází při hello úlohy je zpracování dat je v této kategorii protokolů. Tyto protokoly nejčastěji se vytvoří během čtení, data serializace a operace zápisu. Tyto protokoly neobsahují chyby připojení. K chybám připojení jsou považovány za obecné události.

Name (Název) | Popis
------- | -------
Zdroj | Název úlohy hello vstup nebo výstup, kde došlo k chybě hello.
Zpráva | Zpráva přidružená hello chyby.
Typ | Typ chyby. Například **DataConversionError**, **CsvParserError**, nebo **ServiceBusPropertyColumnMissingError**.
Data | Obsahuje data, která je užitečné tooaccurately vyhledejte zdroj hello hello chyby. Předmět tootruncation, v závislosti na velikosti.

V závislosti na hello **operationName** hodnotu chyb dat mají hello následující schéma:
* **Serializace událostí**. Serializace událostí, ke kterým došlo během operace čtení událostí. K nim dojde, když hello dat hello vstup nesplňuje hello dotaz schématu pro jednu z následujících důvodů:
    * *Neshoda typu během události (de) serializovat*: identifikuje hello pole, která je příčinou chyby hello.
    * *Nelze přečíst událost, neplatný serializace*: uvádí informace o hello umístění hello vstupních dat, kde došlo k chybě hello. Obsahuje název objektu blob pro vstup objektu blob, posun a ukázkových dat hello.
* **Odesílání událostí**. Odesílání událostí, ke kterým došlo během operace zápisu. Identifikují hello vysílání datového proudu událostí, který způsobil chybu hello.

### <a name="generic-events"></a>Obecné události

Obecné události zahrnují nic jiného.

Name (Název) | Popis
-------- | --------
Chyba | (volitelné) Informace o chybě. Obvykle se jedná o informace o výjimce, pokud je k dispozici.
Zpráva| Zprávy protokolu.
Typ | Typ zprávy. Mapuje kategorizaci toointernal chyb. Například **JobValidationError** nebo **BlobOutputAdapterInitializationFailure**.
ID korelace | [Identifikátor GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) který jednoznačně identifikuje hello provádění úlohy. Všechny položky protokolu spuštění z hello čas hello úlohy spustí, dokud je úloha pozastavena hello mít hello stejné **ID korelace** hodnotu.

## <a name="next-steps"></a>Další kroky

* [Úvod tooStream Analytics](stream-analytics-introduction.md)
* [Začínáme s Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka jazyka dotazů Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics správu odkazu k REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
