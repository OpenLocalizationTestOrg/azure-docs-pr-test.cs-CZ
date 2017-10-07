---
title: "aaaDiagnose a řešení problémů | Microsoft Docs"
description: "Tento kurz se zaměřuje na tom, jak toodiagnose a řešení problémů ve vašem prostředí Statistika časové řady"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.openlocfilehash: 00893d4bec497f5f8bf7093be5b96f1844446d13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnostika a řešení problémů ve vašem prostředí Statistika časové řady

## <a name="i-dont-see-my-data"></a>Data se nezobrazí
Tady jsou některé důvody, proč nemusí zobrazit vaše data ve vašem prostředí v hello [portálu Statistika řady čas Azure](https://insights.timeseries.azure.com).

### <a name="your-event-source-doesnt-have-data-in-json-format"></a>Váš zdroj událostí nemá data ve formátu JSON
Statistiky Azure řady čas podporuje pouze data JSON ještě dnes. Ukázky JSON naleznete v části [podporované JSON tvarů](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-you-registered-your-event-source-you-didnt-provide-hello-key-that-has-hello-required-permission"></a>Při registraci zdroje událostí neposkytli hello klíč, který má oprávnění požadované hello
* Centrum IoT, musíte tooprovide hello klíč, který má **služba připojit** oprávnění.

   ![Povolení pro připojení služby IoT hub](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Jak je znázorněno v hello předcházející bitové kopie, buď hello zásad **iothubowner** a **služby** by pracovat, protože mají **služba připojit** oprávnění.
* Centra událostí, musíte tooprovide hello klíč, který má **naslouchání** oprávnění.

   ![Oprávnění naslouchání centra událostí](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Jak je znázorněno v hello předcházející bitové kopie, buď hello zásad **číst** a **spravovat** by pracovat, protože mají **naslouchání** oprávnění.

### <a name="hello-provided-consumer-group-is-not-exclusive-tootime-series-insights"></a>Hello za předpokladu, že skupina uživatelů není výhradní tooTime řady statistiky
Na centrum IoT nebo centra událostí během registrace jsme vyžadovat skupiny příjemců hello toospecify, který se má použít pro čtení vaše data. Tato skupina uživatelů nesmí sdílet. Když ho sdílíte, základní centra událostí hello automaticky odpojí jeden hello čtečky náhodně.

## <a name="i-see-my-data-but-theres-a-lag"></a>Vidět data, ale existuje prodleva
Tady jsou z důvodů, proč může se zobrazit částečná data ve vašem prostředí v hello [portálu Statistika časové řady](https://insights.timeseries.azure.com).

### <a name="your-environment-is-getting-throttled"></a>Prostředí je získávání omezen.
Hello limit omezení vynuceno podle typů SKU a kapacity hello prostředí. Všechny zdroje událostí v prostředí hello sdílet tento kapacitu. Pokud hello zdroj události pro vaše Centrum IoT nebo Centrum událostí je předání dat nad rámec hello vynucuje omezení, zobrazí omezení a prodleva.

Hello následující diagram znázorňuje časové řady Statistika prostředí, které má SKU S1 a kapacitou 3. Můžete ho příjem příchozích dat 3 miliony událostí za den.

![Aktuální kapacita SKU prostředí](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Předpokládejme, že toto prostředí je příjem zpráv z centra událostí míru příchozího hello hello následující diagram znázorňuje:

![Příklad příchozího rychlost pro centra událostí](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Jak je vidět v diagramu hello, denní míra příchozího hello je ~ 67,000 zprávy. Tato míra překládá zhruba too46 zprávy každou minutu. Pokud každá zpráva centra událostí plochou tooa jedna událost časové řady statistiky, toto prostředí uvidí žádné omezení. Pokud každá zpráva Centrum událostí je plochou too100 Statistika řady čas události, pak 4,600 události by měl konzumaci každou minutu. S1 SKU prostředí, které má kapacitou 3 můžete pouze 2100 události příchozího každou minutu (1 milion událostí za den = 700 události za minutu ve 3 jednotky = 2100 události za minutu). Proto se zobrazí prodleva kvůli toothrottling. 

Do hloubky pochopili, jak vyrovnání logiku funguje, najdete v části [podporované JSON tvarů](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Doporučené kroky
toofix hello prodleva, zvýšení hello kapacita SKU vašeho prostředí. Další informace najdete v tématu [jak tooscale prostředí časové řady Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a>Jste předání historických dat a způsobuje pomalé příjem příchozích dat
Pokud se připojujete existujícího zdroje událostí, je pravděpodobné, že Centrum IoT nebo Centrum událostí už obsahuje data v ní. Proto hello prostředí spustí, stahování dat z hello začátku dobu uchování zpráv zdroj události hello. 

Toto chování je hello výchozí chování a nelze přepsat. Můžete použít omezení, a může chvíli trvat toocatch až na příjem historická data.

#### <a name="recommended-steps"></a>Doporučené kroky
toofix hello prodleva, hello proveďte následující kroky:
1. Zvýšit hello SKU kapacity toohello maximální povolenou hodnotu (v tomto případě 10). Po hello kapacity vzroste, spustí se proces příjem příchozích dat hello, zachytávání až mnohem rychlejší. Můžete vizualizovat, jak rychle jste jste zachytávání prostřednictvím hello dostupnosti grafu v hello [portálu Statistika časové řady](https://insights.timeseries.azure.com). Budou se vám účtovat hello zvýšit kapacitu.
2. Po hello je prodleva zachycena, snížit hello SKU kapacity back tooyour normální příchozí míra.

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Zdroj události *název vlastnosti časové razítko* nastavení nebude fungovat.
Ujistěte se, že hello název a hodnotu odpovídat toohello následující pravidla:
* Název vlastnosti Hello časové razítko je _malá a velká písmena_.
* Hodnota vlastnosti časové razítko Hello pocházející ze zdroje událostí, jako řetězec formátu JSON by měl mít formát hello _rrrr-MM-ddTHH. FFFFFFFK_. Příkladem takových řetězec je "2008-04-12T12:53Z".
