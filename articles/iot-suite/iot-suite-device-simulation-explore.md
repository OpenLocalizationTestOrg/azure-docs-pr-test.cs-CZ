---
title: "Začínáme s řešením zařízení simulace - Azure | Microsoft Docs"
description: "Simulace řešení IoT Suite je nástroj, který lze použít pro vývoj a testování řešení IoT. Služba simulace je samostatný nabídky, můžete použít ve spojení s další předkonfigurovaných řešení nebo použít s vlastní řešení."
services: iot device simulation
suite: iot-suite
author: troyhopwood
manager: timlt
ms.author: troyhop
ms.service: iot-suite
ms.date: 12/15/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 25b9d1c7debe0d98a87b7d0b47dea6ab6f36c8af
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="device-simulation-walkthrough"></a>Návod simulace zařízení

Simulace zařízení Azure IoT je nástroj, který lze použít pro vývoj a testování řešení IoT. Simulace zařízení je samostatný nabídky, které můžete ve spojení s další předkonfigurovaných řešení nebo vlastní řešení.

Tento kurz vás provede procesem některé funkce simulace zařízení. Se dozvíte, jak funguje a můžete ji použít k testování své vlastní řešení IoT.

V tomto kurzu se naučíte:

>[!div class="checklist"]
> * Konfigurace simulaci
> * Spuštění a zastavení simulace
> * Zobrazení telemetrie metriky

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu potřebujete nasazené instanci simulace zařízení Azure IoT ve vašem předplatném Azure.

Pokud jste nenasadili simulace zařízení ještě by se měla Dokončit [nasazení simulace zařízení Azure IoT](iot-suite-device-simulation-explore.md) kurzu.

## <a name="configuring-device-simulation"></a>Konfigurace simulace zařízení

Můžete nakonfigurovat a spustit simulaci zařízení zcela od v řídicím panelu. Otevřete řídicí panel ze sady IoT Suite [zřízení řešení](https://www.azureiotsuite.com/) stránky. Klikněte na tlačítko **spusťte** v rámci vaší nové zařízení simulace nasazení.

### <a name="target-iot-hub"></a>Cíl služby IoT Hub

Simulace zařízení můžete použít s předem zřízená Centrum IoT nebo jiných IoT hub:

![Cíl služby IoT Hub](media/iot-suite-device-simulation-explore/targethub.png)

> [!NOTE]
> Možnost použít předem zřízená IoT Hub je k dispozici, pokud se rozhodnete vytvořit novou službu IoT Hub, když jste nasadili simulace zařízení pouze. Pokud nemáte Centrum IoT, můžete vždy vytvořit novou z [portál Azure](https://portal.azure.com).

Pokud chcete zacílit na konkrétní Centrum IoT, zadejte **iothubowner** připojovací řetězec. Tento připojovací řetězec z můžete získat [portál Azure](https://portal.azure.com):

1. Na stránce konfigurace služby IoT Hub v portálu Azure klikněte na tlačítko **zásady sdíleného přístupu**.
1. Klikněte na tlačítko **iothubowner**.
1. Zkopírujte buď primární nebo sekundární klíč.
1. Vložte tuto hodnotu do textového pole pod cíl IoT Hub.

![Cíl služby IoT hub](media/iot-suite-device-simulation-explore/connectionstring.png)

### <a name="device-model"></a>Model zařízení

Model zařízení můžete zvolit typ zařízení k simulaci. Můžete si vybrat modely předem nakonfigurovaná zařízení nebo definovat seznam senzorů pro model vlastní zařízení:

![Model zařízení](media/iot-suite-device-simulation-explore/devicemodel.png)

#### <a name="pre-configured-device-models"></a>Modely předem nakonfigurovaná zařízení

Simulace zařízení poskytuje tři modely předem nakonfigurovaného zařízení. Modely zařízení pro dochlazovače, silech a dodávky jsou k dispozici.

Modely předem nakonfigurovaná zařízení obsahují více senzory s frekvencí předem určené telemetrie. Nelze přizpůsobit četnost telemetrie pro tato zařízení.

Následující tabulka uvádí seznam konfigurace pro každý model předem nakonfigurovaná zařízení:

| Model zařízení | Senzor | Jednotka | Frekvence telemetrie
| -------------| ------ | -----| --------------------|
| Chladič | vlhkosti | % | 5 sekund |
| | pressure | psig | 5 sekund |
| | Teplotní | F | 5 sekund |
| Hodnocení | Floor | | 5 sekund |
| | vibrace | mm | 5 sekund |
| | Teplotní | F | 5 sekund |
| Vůz | Zeměpisná šířka | | 3 sekund |
| | Zeměpisná délka | | 3 sekund |
| | Rychlost | mph | 5 sekund |
| | cargotemperature | F | 5 sekund |

#### <a name="custom-device-model"></a>Model vlastní zařízení

Modely vlastní zařízení umožňují snímače modelu, které přesněji představují vlastní zařízení. Vlastní zařízení může mít až 10 vlastních senzorů.

Když vyberete typ modelu vlastní zařízení, můžete přidat nové senzorů kliknutím **+ přidat senzor**.

![Přidání senzorů](media/iot-suite-device-simulation-explore/customsensors.png)

Vlastní senzorů mít následující vlastnosti:

| Pole | Popis |
| ----- | ----------- |
| Název senzoru | Popisný název senzoru, jako **teploty** nebo **rychlost**. |
| Chování | Chování povolit telemetrická data lišit od jednu zprávu na další k simulaci skutečná data. **Přírůstek** zvyšuje hodnotu jednou v každé zprávě odeslané začínající na minimální hodnotu. Po splnění maximální hodnota je pak spustí přes znovu na minimální hodnotu. **Snížení** se chová stejně jako **přírůstek** ale počty. **Náhodných** chování generuje náhodná hodnota mezi hodnotu minimální a maximální hodnoty. |
| Minimální hodnota | Nejnižší číslo reprezentující přijatelný rozsah. |
| Hodnota maximálního počtu | Největší číslo reprezentující přijatelný rozsah. |
| Jednotka | Jednotky měření senzoru například ° F nebo MPH. |

### <a name="number-of-devices"></a>Počet mobilních zařízení

Simulace zařízení aktuálně umožňuje simulovat až 1 000 zařízení.

![Počet mobilních zařízení](media/iot-suite-device-simulation-explore/numberofdevices.png)

### <a name="telemetry-frequency"></a>Frekvence telemetrie

Frekvence telemetrie umožňuje určit, jak často odesílat data do služby IoT hub Simulovaná zařízení. Může odesílat data jako často jako každých 10 sekund nebo jako zřídka každých jako 99 hodin, 59 minut a 59 sekund.

![Frekvence telemetrie](media/iot-suite-device-simulation-explore/frequency.png)

> [!NOTE]
> Pokud používáte služby IoT hub než předem zřízená IoT hub, měli byste pro službu IoT hub cíl zvážit omezení zpráv. Například pokud máte 1000 Simulovaná zařízení odesílat telemetrii každých 10 sekund do centra S1 dosáhnete limitu telemetrie pro Centrum jenom přes hodinu.

### <a name="simulation-duration"></a>Doba trvání simulace

Můžete spustit simulaci konkrétní dobu nebo spustit, dokud jej nezastavíte explicitně. Když vyberete určitý časový úsek, můžete od 10 až 99 hodiny, 59 minut a sekund 59 žádné doba trvání.

![Doba trvání simulace](media/iot-suite-device-simulation-explore/duration.png)

### <a name="start-and-stop-the-simulation"></a>Spuštění a zastavení simulace

Po přidání všech potřebných konfiguračních dat do formuláře, **Start simulace** tlačítko je k dispozici. Chcete-li spustit simulaci, klikněte na toto tlačítko.

![Spustit simulaci](media/iot-suite-device-simulation-explore/start.png)

Pokud jste zadali určitou dobu pro simulaci, pak je automaticky zastavena po dosažení čas. Můžete vždy simulaci lze zastavit již v rané fázi kliknutím **zastavení simulace.**

Pokud jste zvolili pro simulaci běžela bez omezení, pak se spustí, dokud nekliknete na tlačítko **zastavení simulace**. Okno můžete zavřít prohlížeč a vraťte se na stránku simulace zařízení k zastavení simulace kdykoli.

![Zastavení simulace](media/iot-suite-device-simulation-explore/stop.png)

> [!NOTE]
> Současně lze spustit pouze jeden simulace. Před zahájením nové simulace, je potřeba zastavit aktuálně spuštěné simulace.
