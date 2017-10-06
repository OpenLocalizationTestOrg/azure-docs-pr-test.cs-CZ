---
title: "dynamické telemetrie aaaUse | Microsoft Docs"
description: "Postupujte podle tohoto kurzu toolearn jak toouse dynamické telemetrie s hello Azure IoT Suite vzdálené monitorování předkonfigurované řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 06cb2a370b67b4950efdfa4c7d906ac92106f4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-dynamic-telemetry-with-hello-remote-monitoring-preconfigured-solution"></a>Použití dynamické telemetrie s hello předkonfigurovanému řešení vzdáleného monitorování

Dynamické telemetrie umožňuje vám toovisualize všechny telemetrická data odesílaná toohello předkonfigurovanému řešení vzdáleného monitorování. Hello simulované zařízení, která nasazení s hello předkonfigurované řešení odesílat telemetrii teploty a vlhkosti, který můžete vizualizovat na řídicím panelu hello. Pokud upravíte existující Simulovaná zařízení, vytvořte nové simulované zařízení nebo připojení fyzických zařízení toohello předkonfigurované řešení můžete odeslat další hodnoty telemetrie například externí teplotu hello, ot. / min nebo větru. Potom můžete vizualizovat tuto další telemetrii na řídicím panelu hello.

Tento kurz používá jednoduchý simulované zařízení Node.js můžete snadno upravit tooexperiment s dynamické telemetrií.

toocomplete tohoto kurzu budete potřebovat:

* Aktivní předplatné Azure. Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].
* [Node.js] [ lnk-node] verze 0.12.x nebo novější.

Tento kurz v jakémkoliv operačním systému, jako je například systému Windows nebo Linux, kde můžete nainstalovat Node.js.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Přidání typu telemetrie

dalším krokem Hello je tooreplace hello telemetrii vygenerovanou hello Node.js simulované zařízení s novou sadu hodnot:

1. Zastavení hello Node.js simulované zařízení tak, že zadáte **Ctrl + C** v příkazovém řádku nebo prostředí.
2. V souboru remote_monitoring.js hello uvidíte hello základní data hodnoty pro existující teploty hello vlhkosti a externí teplotní telemetrie. Přidejte hodnotu základní data pro **rpm** následujícím způsobem:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Simulovaná zařízení Node.js Hello používá hello **generateRandomIncrement** v hello remote_monitoring.js soubor tooadd toohello náhodných přírůstek fungovat základní datových hodnot. Náhodně přeskupit hello **rpm** hodnota přidáním řádek kódu po existující randomizations hello:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Přidejte hello nové rpm hodnota toohello JSON datové části hello zařízení odesílá tooIoT rozbočovače:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Spusťte hello Node.js simulované zařízení pomocí hello následující příkaz:

    `node remote_monitoring.js`

6. Sledujte hello nový RPM telemetrie typ, který zobrazí v grafu hello hello řídicím panelu:

![Přidat řídicí panel toohello ot. / min][image3]

> [!NOTE]
> Můžete třeba toodisable a pak povolte zařízení Node.js hello na hello **zařízení** stránky v změn hello toosee hello řídicí panel okamžitě.

## <a name="customize-hello-dashboard-display"></a>Přizpůsobení zobrazení řídicího panelu hello

Hello **informace o zařízení** zprávy může obsahovat metadata o hello telemetrie může posílat hello zařízení tooIoT rozbočovače. Tato metadata můžete určit typy hello telemetrie, které odesílá hello zařízení. Upravit hello **deviceMetaData** hodnota v hello remote_monitoring.js soubor tooinclude **Telemetrie** definice následující hello **příkazy** definice. Hello následující fragment kódu ukazuje hello **příkazy** definice (být jisti tooadd `,` po hello **příkazy** definice):

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> řešení vzdáleného monitorování Hello používá definice metadat hello toocompare velká a malá písmena shodu s daty v datovém proudu telemetrických dat hello.


Přidání **Telemetrie** definice, jak je znázorněno v hello předchozím fragmentu kódu nezmění chování hello hello řídicího panelu. Však může také obsahovat hello metadata **DisplayName** atribut toocustomize hello zobrazení v řídicím panelu hello. Aktualizace hello **Telemetrie** definice metadat, jak je znázorněno v následujícím fragmentu kódu hello:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Hello následující snímek obrazovky ukazuje, jak tuto změnu upraví legendy grafu hello na řídicím panelu hello:

![Přizpůsobení legendy grafu hello][image4]

> [!NOTE]
> Můžete třeba toodisable a pak povolte zařízení Node.js hello na hello **zařízení** stránky v změn hello toosee hello řídicí panel okamžitě.

## <a name="filter-hello-telemetry-types"></a>Filtrovat hello telemetrie typy

Ve výchozím nastavení hello graf na řídicí panel hello znázorňuje všechny datové řady v datovém proudu telemetrických dat hello. Můžete použít hello **informace o zařízení** metadata toosuppress hello zobrazení telemetrie konkrétní typy v grafu hello. 

Graf hello toomake zobrazit pouze teploty a vlhkosti telemetrie, vynechejte **ExternalTemperature** z hello **informace o zařízení** **Telemetrie** metadata následujícím způsobem:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

Hello **venku teploty** už nebude zobrazovat v grafu hello:

![Filtr hello telemetrii na řídicím panelu hello][image5]

Tato změna ovlivňuje pouze zobrazení grafu hello. Hello **ExternalTemperature** hodnoty data jsou stále uloženy a k dispozici pro zpracování všech back-end.

> [!NOTE]
> Můžete třeba toodisable a pak povolte zařízení Node.js hello na hello **zařízení** stránky v změn hello toosee hello řídicí panel okamžitě.

## <a name="handle-errors"></a>Zpracování chyb

Pro toodisplay datového proudu data v grafu hello jeho **typ** v hello **informace o zařízení** metadata musí odpovídat datovému typu hello hello telemetrie hodnot. Například pokud hello metadata Určuje, že hello **typ** vlhkosti dat je **int** a **dvojité** se nachází v datový proud telemetrie hello pak nemá telemetrie vlhkosti hello nejsou zobrazeny v grafu hello. Ale hello **vlhkosti** hodnoty jsou stále uloženy a k dispozici pro zpracování všech back-end.

## <a name="next-steps"></a>Další kroky

Teď, když už víte, jak toouse dynamické telemetrických dat, další informace o tom, jak hello předkonfigurovaných řešení použít informace o zařízení: [informace metadat zařízení v hello vzdálené monitorování předkonfigurované řešení] [ lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
