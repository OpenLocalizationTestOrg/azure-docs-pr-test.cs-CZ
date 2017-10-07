---
title: "aaaConnect tooAzure brány pomocí Intel NUC IoT Suite | Microsoft Docs"
description: "Použijte hello Kit komerční brány Microsoft IoT a hello předkonfigurovaného řešení vzdáleného monitorování. Použít hello Azure IoT hraniční brány tooenable řešení vzdáleného sledování SensorTag toohello tooconnect zařízení, odesílání telemetrie toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 6f98ee3c1e2311a8644da9d72d40e671e7cbcf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a>Připojení vaší brány Azure IoT Edge toohello předkonfigurovanému řešení vzdáleného monitorování a odesílat telemetrická data z SensorTag

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Tento kurz ukazuje, jak toouse data teploty a vlhkosti toosend Azure IoT Edge z SensorTag toohello vzdálené monitorování zařízení předkonfigurované řešení. Hello SensorTag připojí toohello Intel NUC bránu pomocí Bluetooth. kurz Hello používá:

- Azure IoT Edge tooimplement ukázka brány.
- vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.

## <a name="overview"></a>Přehled

V tomto kurzu dokončení hello následující kroky:

- Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure. Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.
- Nastavte vaše Intel NUC brány zařízení toocommunicate s počítačem a hello řešení vzdáleného monitorování.
- Nastavit Intel NUC brány tooreceive telemetrie ze zařízení SensorTag a odešlete ji toohello vzdálené řídicí panel monitorování.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[Texas Instruments SensorTag lit][lnk-sensortag]. V tomto kurzu načte ze zařízení SensorTag hello telemetrická data přes Bluetooth.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure. nasazení Hello odráží architektura skutečné enterprise. tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním. Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a>Konfigurace připojení k Bluetooth

Konfigurovat Bluetooth v zařízení SensorTag tooconnect pro hello Intel NUC tooenable hello a odesílat telemetrická data.

### <a name="find-hello-mac-address-of-hello-sensortag"></a>Zjištění adresy MAC hello hello SensorTag

1. V prostředí hello hello Intel NUC spusťte následující příkaz toounblock hello Bluetooth služby hello:

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. Hello spusťte následující příkazy toostart hello Bluetooth službu na hello Intel NUC a zadejte hello Bluetooth prostředí:

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Spusťte následující příkaz toopower na řadiči Bluetooth hello hello:

    ```bash
    power on
    ```

    Pokud je řadič hello na, zobrazí se zpráva **změna power v úspěšné**.

1. Spusťte následující příkaz tooscan pro nedaleko zařízeními Bluetooth hello:

    ```bash
    scan on
    ```

1. Power hello stiskněte tlačítko na hello SensorTag toomake je zjistitelný. Hello zelená DIODU bliká.

1. Když se zobrazí zpráva v prostředí hello kontroleru hello zjistila hello SensorTag, poznamenejte si hello adresa MAC zařízení hello. Hello adresa MAC vypadá jako **A0:E6:F8:B5:F6:00**. Adresa MAC hello později v kurzu hello musíte při konfiguraci brány hello.

1. Spusťte následující příkaz tooturn vypnout kontrolu Bluetooth hello:

    ```bash
    scan off
    ```

1. Spusťte následující příkaz tooverify, že se můžete připojit zařízení SensorTag toohello hello:

    ```bash
    connect <SensorTag MAC address>
    ```

    Je-li úspěšně připojit hello prostředí ukazuje uvítací zprávu **úspěšné připojení** a zobrazí informace o zařízení SensorTag hello. Pokud se nemůžete připojit, zkontrolujte hello SensorTag je pořád zapnutý.

1. Teď můžete odpojit od hello SensorTag a ukončete prostředí Bluetooth hello spuštěním hello následující příkazy:

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a>Vytvořit vlastní modul IoT Edge hello

Teď můžete sestavit hello vlastní okraje IoT modul, který umožňuje hello brány toosend zprávy toohello řešení vzdáleného monitorování. Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].

Hello zdrojového kódu pro vlastní moduly IoT Edge hello stáhněte z webu GitHub pomocí hello následující příkazy:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Vytvořit vlastní modul IoT Edge hello pomocí hello následující příkazy:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

skriptu buildu Hello umístí do složky sestavení hello hello libsensor2remotemonitoring.so vlastní okraje IoT modul.

## <a name="configure-and-run-hello-iot-edge-gateway"></a>Nakonfigurujte a spusťte hello IoT hraniční brána

Teď můžete konfigurovat hello IoT hraniční brány toosend telemetrie z vaší tooyour zařízení SensorTag pro vzdálené monitorování řídicího panelu. Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].

> [!TIP]
> V tomto kurzu použijete standardní hello `vi` textového editoru na hello Intel NUC. Pokud jste nepoužili `vi` před, musíte provést úvodní kurz, jako například [Unix - hello vi Editor kurzu] [ lnk-vi-tutorial] toofamiliarize sami pomocí tohoto editoru. Alternativně můžete nainstalovat hello přívětivější [nano](https://www.nano-editor.org/) editor pomocí příkazu hello `smart install nano -y`.

Otevřete hello vzorový konfigurační soubor v hello **vi** editor pomocí hello následující příkaz:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

Vyhledejte následující řádky v hello konfiguraci pro modul IoTHub hello hello:

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

Nahraďte zástupný symbol hello hodnoty s hello informace služby IoT Hub vytvořili a uložili v hello spuštění tohoto kurzu. Hodnota Hello IoTHubName vypadá jako **yourrmsolution37e08**, a hodnota hello IoTSuffix je obvykle **azure devices.net**.

Vyhledejte následující řádky v konfiguraci hello hello mapování modulu hello:

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Nahraďte hello **macAddress** zástupný symbol hello adresu MAC vaší SensorTag jste si poznamenali dříve. Nahraďte hello **deviceID** a **deviceKey** zástupné symboly hello ID a klíče pro hello dva zařízení, které jste vytvořili dříve v hello řešení vzdáleného monitorování.

Vyhledejte následující řádky v hello konfiguraci pro modul SensorTag hello hello:

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

Nahraďte hello **zařízení\_mac\_adresu** zástupný symbol hello adresu MAC vaší SensorTag jste si poznamenali dříve.

Uložte provedené změny.

Teď můžete spustit bránu hello pomocí hello následující příkazy:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

Hello IoT hraniční brána se spouští v hello Intel NUC a odesílá telemetrii z hello SensorTag toohello řešení vzdáleného monitorování:

![IoT hraniční brána odesílá telemetrii z hello SensorTag][img-telemetry]

Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.

## <a name="view-hello-telemetry"></a>Zobrazení telemetrie hello

Hello brána teď odesílá telemetrii z hello SensorTag zařízení toohello řešení vzdáleného monitorování. Můžete zobrazit telemetrii hello na řídicí panel řešení hello. Můžete také odeslat příkazy tooyour SensorTag zařízení prostřednictvím brány hello z řídicí panel řešení hello.

- Přejděte toohello řídicí panel řešení.
- Vyberte hello zařízení, které jste nakonfigurovali v hello brány, který představuje hello SensorTag v hello **zařízení tooView** rozevíracího seznamu.
- Hello telemetrie ze zařízení SensorTag hello se zobrazí na řídicím panelu hello.

![Zobrazení telemetrie z zařízení SensorTag hello][img-telemetry-display]

> [!WARNING]
> Pokud necháte hello vzdálené monitorování řešení, které jsou spuštěné v účtu Azure, se vám účtuje hello, když ji spustí. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config]. Po dokončení používat, odstraňte hello předkonfigurované řešení z účtu Azure.


## <a name="next-steps"></a>Další kroky

Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started