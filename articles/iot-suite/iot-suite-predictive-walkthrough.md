---
title: "návod aaaPredictive údržby | Microsoft Docs"
description: "Návod pro prediktivní údržby Azure IoT hello předkonfigurované řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 900d6351019489a8e2f4b98908364e3bd14975c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Návod pro předkonfigurované řešení prediktivní údržby

Hello předkonfigurované řešení prediktivní údržby je začátku do konce řešení pro podnikový scénář, který bude předpovídat hello bod selhání, na které jsou pravděpodobně toooccur. Toto předkonfigurované řešení můžete aktivně využívat pro různé činnosti, jako je třeba optimalizace údržby. Hello řešení kombinuje klíčové služby sady Azure IoT Suite, jako je například IoT Hub, Stream analytics a [Azure Machine Learning] [ lnk-machine-learning] pracovního prostoru. Tento pracovní prostor obsahuje model, založené na veřejné vzorové datové sady, toopredict hello zbývající dobu životnosti (RUL) leteckého motoru. řešení Hello plně implementuje hello IoT obchodní scénáře jako výchozí bod pro tooplan je a implementovat řešení, které splňuje specifických podnikových požadavků.

## <a name="logical-architecture"></a>Logická architektura

Hello následující diagram popisuje logické součásti hello hello předkonfigurované řešení:

![][img-architecture]

Hello modré položky jsou služby Azure zřízené v hello oblasti, kde jste nasadili hello předkonfigurované řešení. Zobrazí se seznam Hello oblastí, kde můžete nasadit hello předkonfigurované řešení na hello [zřizování stránky][lnk-azureiotsuite].

Hello zelená položka je simulované zařízení, které představuje letecký motor. Další informace o těchto simulovaných zařízeních v následující části hello.

Hello šedé položky představují součásti, které implementují *správy zařízení* možnosti. Hello aktuální verzi hello předkonfigurované řešení prediktivní údržby tyto prostředky neposkytuje. toolearn Další informace o správě zařízení naleznete toohello [předkonfigurovaná řešení vzdáleného sledování][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simulovaná zařízení

V hello předkonfigurované řešení simulované zařízení představuje letecký motor. se dvěma motory, které mapují jednoho letadla tooa zřídí řešení Hello. Každý motor vysílá čtyři typy telemetrických dat: Sensor 9, Sensor 11, Sensor 14 a Sensor 15 zadejte hello data potřebná pro hello Machine Learning modelu toocalculate hello RUL pro modul hello. Každé simulované zařízení posílá hello následující telemetrické zprávy tooIoT rozbočovače:

*Počet cyklů*. Cyklus představuje dokončený let trvající 2 až 10 hodin. Během letu hello je každou půlhodinu zaznamenána telemetrická data.

*Telemetrická data*. V řešení jsou čtyři snímače, které představují vlastnosti motoru. Hello snímače jsou obecně označeny jako Sensor 9, Sensor 11, Sensor 14 a Sensor 15. Tyto čtyři snímače představují telemetrie dostatečná tooobtain užitečné výsledky z modelu RUL hello. model Hello používá v hello předkonfigurované řešení je vytvořený z veřejné datové sady, obsahující data snímačů reálného motoru. Další informace o vytváření hello modelu z původní sady dat hello najdete v tématu hello [Cortana Intelligence Galerie šablona prediktivní údržby][lnk-cortana-analytics].

Hello simulované zařízení mohou zpracovávat následující příkazy, odeslané ze služby IoT hub hello v řešení hello hello:

| Příkaz | Popis |
| --- | --- |
| StartTelemetry |Ovládací prvky hello stav simulace hello.<br/>Spustí hello zařízení odesílat telemetrii |
| StopTelemetry |Ovládací prvky hello stav simulace hello.<br/>Hello zastaví odesílání telemetrických dat ze zařízení |

Služba IoT Hub zajišťuje potvrzení příkazu zařízení.

## <a name="azure-stream-analytics-job"></a>Úlohy služby Azure Stream Analytics

**Úloha: Telemetrie** funguje na hello příchozí datovém proudu telemetrických dat pomocí dvou příkazů:

* Hello první vybere všechny telemetrická ze zařízení hello a odešle tato data tooblob úložiště. Tady jsou vizualizována ve webové aplikaci hello.
* Hello druhý vypočítá průměrnou senzor hodnoty přes dvě minuty posuvné okno a odešle je prostřednictvím tooan centra událostí hello **procesor událostí**.

## <a name="event-processor"></a>Procesor událostí
Hello **eventprocessorhost** běží v úlohu webové služby Azure. Hello **procesor událostí** trvá hello průměrné hodnoty čidel za dokončený cyklus. Pak předá tyto hodnoty tooan rozhraní API, která zveřejňuje trained model toocalculate hello RUL motoru. Hello rozhraní API je zveřejněný prostřednictvím pracovního prostoru Machine Learning, která je zřízená jako součást řešení hello.

## <a name="machine-learning"></a>Machine Learning
součást Machine Learning Hello používá model odvozený od data shromážděná z reálného letecké motory. Můžete přejít pracovní prostor Machine Learning toohello z dlaždice hello na hello [azureiotsuite.com] [ lnk-azureiotsuite] stránky zřízeného řešení. Hello dlaždice je dostupná, když je řešení hello v hello **připraven** stavu.


## <a name="next-steps"></a>Další kroky
Nyní jste viděli hello klíčové komponenty hello předkonfigurované řešení prediktivní údržby, může být vhodné toocustomize ho. Viz [Doprovodné materiály k přizpůsobení předkonfigurovaných řešení][lnk-customize].

Můžete také prozkoumat některé hello další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:

* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]
* [Zabezpečení IoT z hello pozadí][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/