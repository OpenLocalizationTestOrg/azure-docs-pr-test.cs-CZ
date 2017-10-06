---
title: "aaaConnect tooAzure malin platformy IoT Suite simulovanou telemetrii pomocí Node.js | Microsoft Docs"
description: "Použijte hello Microsoft Azure IoT Starter Kit pro hello malin pí 3 a Azure IoT Suite. Použít Node.js tooconnect vaše řešení vzdáleného sledování toohello malin Pi, odesílání simulovanou telemetrii toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: f65eeaa6e83fd89cdedae8fa8386a3e9ef8417d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a>Připojit vaše řešení vzdáleného sledování malin pí 3 toohello a odeslat simulovanou telemetrii pomocí Node.js

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Tento kurz ukazuje, jak cloudové toouse hello malin pí 3 toosimulate teploty a vlhkosti data toosend toohello. kurz Hello používá:

- Raspbian OS hello Node.js programovací jazyk a hello Microsoft Azure IoT SDK pro Node.js tooimplement ukázka zařízení.
- vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure. nasazení Hello odráží architektura skutečné enterprise. tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním. Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a>Stáhnout a nakonfigurovat ukázka hello

Nyní můžete stáhnout a nakonfigurovat hello vzdálené monitorování klientské aplikace na vaše malin pí.

### <a name="install-nodejs"></a>Instalovat Node.js

Pokud jste tak ještě neučinili, nainstalujte na vaše platformy malin Node.js. Hello IoT SDK pro Node.js vyžaduje verzi 0.11.5 Node.js nebo novější. Hello následující kroky ukazují, jak tooinstall Node.js v6.10.2 na vaše malin platformy:

1. Použijte následující příkaz tooupdate hello vaší malin platformy:

    ```sh
    sudo apt-get update
    ```

1. Použijte následující příkaz toodownload hello Node.js binární soubory tooyour malin pí hello:

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Použijte následující příkaz tooinstall hello binární soubory hello:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Použijte následující příkaz tooverify jste úspěšně nainstalovali Node.js v6.10.2 hello:

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a>Klonování úložiště hello

Pokud jste tak již neučinili, požadované klon hello hello úložiště tak, že spustíte následující příkazy v terminálu na vaše platformy:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a>Aktualizovat hello zařízení připojovací řetězec

Otevřete hello ukázka zdrojový soubor v hello **nano** editor pomocí hello následující příkaz:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Najděte hello řádek:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Hello zástupné hodnoty nahraďte hello zařízení a informace služby IoT Hub vytvořili a uložili při spuštění hello tohoto kurzu. Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).

## <a name="run-hello-sample"></a>Spuštění ukázkové hello

Hello spusťte následující příkazy tooinstall hello požadované balíčky pro hello ukázku:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

Ukázka programu hello teď můžete spustit na hello malin pí. Zadejte příkaz hello:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Hello následující ukázkový výstup je příklad výstupu hello, uvidíte hello příkazového řádku na hello malin platformy:

![Výstup z aplikace Malinová platformy][img-raspberry-output]

Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Další kroky

Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
