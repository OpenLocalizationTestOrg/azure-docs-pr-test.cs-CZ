---
title: "aaaOverview Statistika řady čas Azure | Microsoft Docs"
description: "Úvod tooAzure časové řady vhled, novou službu k analýze dat řady čas a řešení IoT"
keywords: 
services: tsi
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: 
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: omravi
ms.openlocfilehash: 8c022bf1fae88eddab3dbebc7bb36cc15a785172
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-time-series-insights"></a>Co je Azure Time Series Insights

Statistiky Azure řady čas je spravovaná Cloudová služba se úložiště, analýzy a vizualizace komponent, které snadno tooingest, ukládat, zkoumat a analyzovat až miliardy událostí současně. Time Series Insights poskytuje globální přehled o datech a umožňuje rychle ověřit vaše řešení IoT a vyhnout se nákladným prostojům zařízení, protože pomáhá odhalovat skryté trendy a anomálie a provádět analýzy hlavních příčin téměř v reálném čase. Statistika časové řady ingestuje časové řady data z zprostředkovatelé událostí (např. služby IoT Hubs nebo Event Hubs), indexy hello dat a IT vyřadí dat na základě zásad uchovávání konfigurovat. Uživatelé využívají data hello buď prostřednictvím intuitivního UX nebo REST API dotazu.

![Přehled služby Time Series Insights](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Primární scénáře

* Monitorování a ověření řešení IoT během několika minut.
* Vizualizace a analýza dat IoT ve velkém měřítku.
* Urychlení analýzy hlavních příčin a detekce anomálií.
* Vytvoření globálního přehledu o více zařízeních, přístrojích a datech.

## <a name="capabilities-and-benefits"></a>Funkce a výhody

* **Snadno tooget spuštění**: Statistika řady čas Azure vyžaduje bez přípravy počáteční dat a je velmi rychlé. Připojte toobillions událostí ve službě Azure IoT Hub nebo Centrum událostí v minutách. Po připojení, vizualizovat a interakci s dat snímačů v sekundách tooquickly ověření vašich vlastních IoT řešení. Časové řady Insights je snadno toouse; můžete pracovat s daty bez nutnosti napsat jediný řádek kódu.  Neexistuje žádné nové jazykové toolearn; Časové řady statistika poskytuje podrobné, volné dotazu prostor pro pokročilé uživatele a bod a klikněte na zkoumání pro všechny.

* **V blízkosti přehledy v reálném čase**: Statistika časové řady může ingestovanou stovky milionů senzor událostí za den, s latencí jednu minutu, takže umožňují rychle reagovat toochanges. Time Series Insights pomáhá získat podrobný přehled o datech senzorů tím, že pomáhá rychle zjišťovat trendy a anomálie, provádět komplexní analýzy hlavních příčin a vyhnout se nákladným prostojům. Povolením cross korelace mezi v reálném čase a historická data časové řady přehledy vám pomůže odemknutí skrytá trendů v datech hello.

* **Sestavování vlastních řešení:** Zahrňte data Azure Time Series Insights do svých stávajících aplikací nebo vytvořte nová vlastní řešení s využitím rozhraní Time Series Insights REST API. Vytvoření a sdílení přizpůsobit zobrazení, že můžete sdílet pro ostatní tooexplore vaše zjišťování.

* **Škálovatelnost**: čas řady Insights je navrženou toosupport IoT ve velkém měřítku. Ve verzi Preview můžete ho příjem příchozích dat z 1 milion too100 miliony událostí za den, s dobou uchování výchozí span 31 dní. Můžete vizualizovat a analyzovat živé datové proudy téměř v reálném čase spolu s obrovskými objemy historických dat. Klouzavý dopředu, zvýší příjem příchozích dat a uchování sazby tooaccommodate někdy vyvíjející celopodnikového rozsahu.

## <a name="time-series-insights-glossary"></a>Glosář Time Series Insights

* **Prostředí:** Prostředí je prostředek Azure s kapacitou úložiště a příchozího přenosu dat.  Zákazníci zřídit prostředí prostřednictvím hello portál Azure s jejich požadovanou kapacitou.
* **Zdroj událostí:** Zdroj událostí je odvozený od zprostředkovatele událostí, jako je například služba Azure Event Hubs.  Časové řady Insights se připojuje přímo tooEvent zdrojů, příjem hello datový proud bez psaní jakéhokoli kódu. V současné době Time Series Insights podporuje Azure Event Hubs a Azure IoT Hubs.
* **Referenční data**: Statistika časové řady poskytuje uživatelům hello možnost toojoin časových řad dat se referenční data.  Referenčními daty můžou být metadata o zařízeních nebo jiná statická data, která se mění poměrně málo často. Statistika časové řady připojí hello referenční data s datové proudy, umožňuje uživatelům toovisualize a analyzovat tato data v téměř v reálném čase.
