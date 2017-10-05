---
title: "Zpracování – pořadí událostí a zpoždění pomocí služby Azure Stream Analytics | Microsoft Docs"
description: "Informace o fungování služby Stream Analytics s mimo pořadí nebo pozdní události v datových proudů."
keywords: "mimo provoz, pozdní, události"
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
ms.openlocfilehash: a48e48c5fc65d0ffbb7c23f426a4b06dcd568821
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a>Zpracování pořadí událostí služby Azure Stream Analytics

V dočasné datovém proudu událostí každá událost se zaznamená s časem, které se získaly události. Některé podmínky může způsobit streamů událostí příležitostně přijímat některé události v jiném pořadí, než které byly odeslány. Jednoduché opakování TCP nebo i hodiny zkosení mezi zařízením odesílání a příjmu centra událostí může dojít k tomu došlo. "Interpunkce" události také se přidají do datových proudů přijatou událost posunut čas při absenci události doručení. Je potřeba ve scénářích, jako je "Upozornit mě, pokud žádná přihlášení dojít k 3 minuty."

Vstupní datové proudy, které nejsou v pořadí jsou buď:
* Seřadit (a proto **odložené**).
* Upravit v systému podle zásady definované uživatelem.


## <a name="lateness-tolerance"></a>Tolerance zpoždění
Stream Analytics toleruje tyto typy scénáře. Stream Analytics má zpracování pro "na více systémů pořadí" a "pozdní" události. Tyto události zpracovává následujícími způsoby:

* Události, které přicházejí mimo pořadí, ale v rámci sady tolerance **přeuspořádány pomocí časového razítka**.
* Události, které přicházejí později než proti chybám jsou **vyřadit nebo upravena**.
    * **Upraví**: upravený tak, aby se zobrazí určeny na nejnovější přijatelný čas.
    * **Vyřadit**: zahozeny.

![Stream Analytics zpracování událostí](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-the-number-of-out-of-order-events"></a>Snižte počet mimo pořadí událostí

Protože Stream Analytics vztahuje dočasné transformace při zpracování příchozí události (například pro agregací v časových oknech nebo dočasných spojení), Stream Analytics seřadí příchozí události podle pořadí časové razítko.

Pokud klíčové slovo "časové razítko podle" je **není** používá, čas zařazování Azure Event Hubs události se používá ve výchozím nastavení. Služba Event Hubs zaručuje monotonicity časového razítka v každém oddílu centra událostí. Také zaručuje, že události ze všech oddílů se sloučí v pořadí časové razítko. Tyto dvě služby Event Hubs záruky zajistěte žádné události mimo pořadí.

V některých případech je důležité budete moci použít časové razítko odesílatele. V takovém případě je zvolen časové razítko z datová část události pomocí "časového razítka podle." V těchto scénářích může být zavedl jednoho nebo více zdrojů misorder událostí:

* Producenti událostí mít zkosí hodiny. To je běžné, když producenti jsou z různých počítačů, takže mají různé hodiny.
* Dochází ke zpoždění sítě ze zdroje událostí do centra událostí cílový.
* Hodiny zkosí existovat mezi oddílů centra událostí. Stream Analytics nejprve řadí události ze všech oddílů centra událostí podle zařazování čas události. Pak zkontroluje datový proud pro misordered události.

Na kartě konfigurace zobrazí následující výchozí nastavení:

![Stream Analytics se na pořadí zpracování](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

Pokud používáte 0 s jako interval tolerance pořadí se na více systémů, uplatňujete, že jsou všechny události v pořadí vždy. Zadána tři zdroje misordered událostí nepravděpodobné, že se jedná o hodnotu true. 

Povolit Stream Analytics opravit misorder události, můžete zadat interval tolerance nenulové mimo pořadí. Stream Analytics ukládá vyrovnávací paměti události až toto okno a potom je změní pomocí časového razítka, které jste zvolili. Aplikace používá dočasné transformace. Můžete začínat okno 3 sekund a ladit hodnota a snížit počet událostí, které jsou upraveny čas. 

Vedlejším účinkem ukládání do vyrovnávací paměti je, že je výstup **zpožděn stejnou dobu**. Můžete ladit hodnota ke snížení počtu událostí na více systémů pořadí a zachovat nízkou latencí úlohy.

## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Stream Analytics](stream-analytics-introduction.md)
* [Začínáme s Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka jazyka dotazů Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics správu odkazu k REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)