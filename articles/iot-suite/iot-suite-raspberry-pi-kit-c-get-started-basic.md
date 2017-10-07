---
title: "aaaConnect tooAzure malin platformy IoT Suite pomocí jazyka C skutečné snímači | Microsoft Docs"
description: "Použijte hello Microsoft Azure IoT Starter Kit pro hello malin pí 3 a Azure IoT Suite. Použijte C tooconnect vaše řešení vzdáleného sledování toohello malin platformy, odesílat telemetrická data ze senzorů toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
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
ms.openlocfilehash: 7dac55ae5fde4c6f8e3ea6a7debf9a6822dc07ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a>Připojit vaše řešení vzdáleného sledování malin pí 3 toohello a odesílat telemetrická data z reálného senzor pomocí jazyka C

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Tento kurz ukazuje, jak toouse hello Microsoft Azure IoT Starter Kit pro malin pí 3 toodevelop čtečku teploty a vlhkosti, který může komunikovat s hello cloudu. kurz Hello používá:

- Raspbian operačního systému, programovací jazyk hello C a hello sady SDK pro aplikaci Microsoft Azure IoT pro C tooimplement ukázka zařízení.
- vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.

## <a name="overview"></a>Přehled

V tomto kurzu dokončení hello následující kroky:

- Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure. Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.
- Nastavte vaše zařízení a senzory toocommunicate s počítačem a hello řešení vzdáleného sledování.
- Aktualizujte hello ukázka zařízení kódu tooconnect toohello řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení hello.

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure. nasazení Hello odráží architektura skutečné enterprise. tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním. Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a>Stáhnout a nakonfigurovat ukázka hello

Nyní můžete stáhnout a nakonfigurovat hello vzdálené monitorování klientské aplikace na vaše malin pí.

### <a name="clone-hello-repositories"></a>Klonování úložiště hello

Pokud jste tak již neučinili, požadované klon hello hello úložiště tak, že spustíte následující příkazy v terminálu na vaše platformy:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-hello-device-connection-string"></a>Aktualizovat hello zařízení připojovací řetězec

Otevřete hello ukázka zdrojový soubor v hello **nano** editor pomocí hello následující příkaz:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
```

Vyhledejte hello následující řádky:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Hello zástupné hodnoty nahraďte hello zařízení a informace služby IoT Hub vytvořili a uložili při spuštění hello tohoto kurzu. Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).

## <a name="build-hello-sample"></a>Vytvoření ukázkové hello

Nainstalujte hello požadovaných balíčků hello Microsoft Azure IoT zařízení SDK pro jazyk C tak, že spustíte následující příkazy v terminálu na hello malin pí hello:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Teď můžete sestavit hello aktualizované ukázkové řešení na hello malin platformy:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

Ukázka programu hello teď můžete spustit na hello malin pí. Zadejte příkaz hello:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

Hello následující ukázkový výstup je příklad výstupu hello, uvidíte hello příkazového řádku na hello malin platformy:

![Výstup z aplikace Malinová platformy][img-raspberry-output]

Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Další kroky

Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
