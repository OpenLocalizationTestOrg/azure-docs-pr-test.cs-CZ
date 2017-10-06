---
title: "aaaConnect tooAzure brány pomocí Intel NUC IoT Suite | Microsoft Docs"
description: "Použijte hello Kit komerční brány Microsoft IoT a hello předkonfigurovaného řešení vzdáleného monitorování. Použití hello Azure IoT hraniční brány tooconnect toohello řešení vzdáleného monitorování, odešle simulovanou telemetrii toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
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
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 46b545fc21b054191c8f78ace20fc628f839a819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>Připojení vaší brány Azure IoT Edge toohello předkonfigurovanému řešení vzdáleného monitorování a odeslat simulovanou telemetrii

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Tento kurz ukazuje, jak toouse Azure IoT Edge toosimulate teploty a vlhkosti data toosend toohello vzdálené monitorování předkonfigurované řešení. kurz Hello používá:

- Azure IoT Edge tooimplement ukázka brány.
- vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.

## <a name="overview"></a>Přehled

V tomto kurzu dokončení hello následující kroky:

- Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure. Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.
- Nastavte vaše Intel NUC brány zařízení toocommunicate s počítačem a hello řešení vzdáleného monitorování.
- Nakonfigurujte hello IoT hraniční brány toosend simulated telemetrii, kterou můžete zobrazit na řídicí panel řešení hello.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure. nasazení Hello odráží architektura skutečné enterprise. tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním. Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

Opakujte předchozí kroky tooadd hello druhé zařízení pomocí ID zařízení, jako třeba **device02**. Ukázka Hello odešle data ze dvou simulované zařízení v hello brány toohello řešení vzdáleného monitorování.

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

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
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

skriptu buildu Hello umístí do složky sestavení hello hello libsimulator.so vlastní okraje IoT modul.

## <a name="configure-and-run-hello-iot-edge-gateway"></a>Nakonfigurujte a spusťte hello IoT hraniční brána

Teď můžete konfigurovat hello IoT hraniční brány toosend simulovanou telemetrii tooyour vzdálené monitorování řídicího panelu. Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].

> [!TIP]
> V tomto kurzu použijete standardní hello `vi` textového editoru na hello Intel NUC. Pokud jste nepoužili `vi` před, musíte provést úvodní kurz, jako například [Unix - hello vi Editor kurzu] [ lnk-vi-tutorial] toofamiliarize sami pomocí tohoto editoru. Alternativně můžete nainstalovat hello přívětivější [nano](https://www.nano-editor.org/) editor pomocí příkazu hello `smart install nano -y`.

Otevřete hello vzorový konfigurační soubor v hello **vi** editor pomocí hello následující příkaz:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
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
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Nahraďte hello **deviceID** a **deviceKey** zástupné symboly hello ID a klíče pro hello dva zařízení, které jste vytvořili dříve v hello řešení vzdáleného monitorování.

Uložte provedené změny.

Teď můžete spustit hello IoT hraniční bránu pomocí hello následující příkazy:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

Hello brány se spouští v hello Intel NUC a odešle simulovanou telemetrii toohello řešení vzdáleného monitorování:

![IoT hraniční brána generuje simulovanou telemetrii][img-simulated telemetry]

Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.

## <a name="view-hello-telemetry"></a>Zobrazení telemetrie hello

Hello IoT hraniční brána je nyní odesílání simulovanou telemetrii toohello řešení vzdáleného monitorování. Můžete zobrazit telemetrii hello na řídicí panel řešení hello.

- Přejděte toohello řídicí panel řešení.
- Vyberte jednu z hello dva zařízení, které jste nakonfigurovali v hello brány v hello **zařízení tooView** rozevíracího seznamu.
- Hello telemetrie ze zařízení brány hello se zobrazí na řídicím panelu hello.

![Zobrazení telemetrie z hello simulované zařízení brány][img-telemetry-display]

> [!WARNING]
> Pokud necháte hello vzdálené monitorování řešení, které jsou spuštěné v účtu Azure, se vám účtuje hello, když ji spustí. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config]. Po dokončení používat, odstraňte hello předkonfigurované řešení z účtu Azure.

## <a name="next-steps"></a>Další kroky

Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started