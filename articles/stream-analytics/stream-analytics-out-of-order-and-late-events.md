---
title: "aaaHandling – pořadí událostí a zpoždění pomocí služby Azure Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 87c028662fbafbf4f72f57f215d017f65bb649f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a>Zpracování pořadí událostí služby Azure Stream Analytics

V dočasné datovém proudu událostí, se zaznamenává všechny události s časem hello tuto událost hello přijetí. Některé podmínky může způsobit streamů událostí toooccasionally přijímat některé události v jiném pořadí, než které byly odeslány. Přenos jednoduché TCP nebo i zkosení hodin mezi hello odesílání zařízení a hello přijetí centra událostí může způsobit, že tento toooccur. "Interpunkce" události také přidají datových proudů událostí tooreceived, tooadvance hello čas v hello absence událostí doručení. Je potřeba ve scénářích, jako je "Upozornit mě, pokud žádná přihlášení dojít k 3 minuty."

Vstupní datové proudy, které nejsou v pořadí jsou buď:
* Seřadit (a proto **odložené**).
* Upraví systémem hello podle zásad tooa zadaného uživatelem.


## <a name="lateness-tolerance"></a>Tolerance zpoždění
Stream Analytics toleruje tyto typy scénáře. Stream Analytics má zpracování pro "na více systémů pořadí" a "pozdní" události. Zpracovává tyto události v hello následující způsoby:

* Události, které dorazí mimo pořadí, ale v rámci hello nastavit tolerance **přeuspořádány pomocí časového razítka**.
* Události, které přicházejí později než proti chybám jsou **vyřadit nebo upravena**.
    * **Upraví**: upravenou tooappear toohave byly přijaty nejnovější přijatelný časový hello.
    * **Vyřadit**: zahozeny.

![Stream Analytics zpracování událostí](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-hello-number-of-out-of-order-events"></a>Snižte počet hello na více systémů pořadí událostí

Protože Stream Analytics vztahuje dočasné transformace při zpracování příchozí události (například pro agregací v časových oknech nebo dočasných spojení), Stream Analytics seřadí příchozí události podle pořadí časové razítko.

Pokud je – klíčové slovo hello "časové razítko podle" **není** používá, čas zařazování události hello Azure Event Hubs se používá ve výchozím nastavení. Služba Event Hubs zaručuje monotonicity hello časového razítka v každém oddílu centra událostí hello. Také zaručuje, že události ze všech oddílů se sloučí v pořadí časové razítko. Tyto dvě služby Event Hubs záruky zajistěte žádné události mimo pořadí.

V některých případech je důležité pro časové razítko je toouse hello odesílatele. V takovém případě je zvolen časové razítko z datová část události hello pomocí "časového razítka podle." V těchto scénářích může být zavedl jednoho nebo více zdrojů misorder událostí:

* Producenti událostí mít zkosí hodiny. To je běžné, když producenti jsou z různých počítačů, takže mají různé hodiny.
* Dochází ke zpoždění sítě ze zdroje hello centra událostí cílové toohello události hello.
* Hodiny zkosí existovat mezi oddílů centra událostí. Stream Analytics nejprve řadí události ze všech oddílů centra událostí podle zařazování čas události. Pak zkontroluje hello datový proud pro misordered události.

Na kartě Konfigurace hello najdete v části hello následující výchozí nastavení:

![Stream Analytics se na pořadí zpracování](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

Pokud používáte 0 s jako interval tolerance na více systémů pořadí hello, uplatňujete, že jsou všechny události v pořadí celou dobu hello. Zadané hello tři zdroje misordered událostí, nepravděpodobné, že se jedná o hodnotu true. 

Stream Analytics toocorrect tooallow misorder události, můžete zadat interval tolerance nenulové mimo pořadí. Stream Analytics vyrovnávací paměti události toothat okna a potom přiobjednávky je pomocí hello časové razítko, které jste zvolili. Aplikace používá dočasné transformace hello. Můžete začít s okno 3 sekundu a ladit hello hodnota tooreduce hello počet událostí, které jsou upraveny čas. 

Vedlejším účinkem ukládání do vyrovnávací paměti hello je, že je výstup hello **zpožděn hello stejné množství času**. Můžete ladit hello hodnota tooreduce hello počet událostí na více systémů pořadí a zachovat nízkou latencí úlohy hello.

## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooStream Analytics](stream-analytics-introduction.md)
* [Začínáme s Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka jazyka dotazů Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics správu odkazu k REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)