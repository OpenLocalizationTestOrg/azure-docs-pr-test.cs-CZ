---
title: "aktualizace aaaConnect tooAzure malin platformy IoT Suite pomocí C toosupport firmware | Microsoft Docs"
description: "Použijte hello Microsoft Azure IoT Starter Kit pro hello malin pí 3 a Azure IoT Suite. Použijte C tooconnect vaše řešení vzdáleného sledování toohello malin platformy, odesílat telemetrická data ze senzorů toohello cloudu a provést aktualizaci firmwaru vzdálené."
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
ms.openlocfilehash: 36d39c6d754ddb025fd3f6b74d7795ed907b754c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a>Připojit vaše řešení vzdáleného sledování malin pí 3 toohello a povolit vzdálenou firmware aktualizace pomocí jazyka C

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Tento kurz ukazuje, jak toouse hello Microsoft Azure IoT Starter Kit pro malin pí 3 na:

* Vývoj čtečku teploty a vlhkosti, který může komunikovat s hello cloudu.
* Povolit a provádět vzdálenou firmware aktualizace tooupdate hello klientskou aplikaci na hello malin pí.

kurz Hello používá:

* Raspbian operačního systému, programovací jazyk hello C a hello sady SDK pro aplikaci Microsoft Azure IoT pro C tooimplement ukázka zařízení.
* vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.

## <a name="overview"></a>Přehled

V tomto kurzu dokončení hello následující kroky:

* Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure. Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.
* Nastavte vaše zařízení a senzory toocommunicate s počítačem a hello řešení vzdáleného sledování.
* Aktualizujte hello ukázka zařízení kódu tooconnect toohello řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení hello.
* Pomocí klienta aplikace hello kód tooupdate hello ukázka zařízení.

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure. nasazení Hello odráží architektura skutečné enterprise. tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním. Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a>Stáhnout a nakonfigurovat ukázka hello

Nyní můžete stáhnout a nakonfigurovat hello vzdálené monitorování klientské aplikace na vaše malin pí.

### <a name="clone-hello-repositories"></a>Klonování úložiště hello

Pokud jste tak ještě neučinili, požadované klon hello hello úložiště tak, že spustíte následující příkazy na vaše platformy:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a>Aktualizovat hello zařízení připojovací řetězec

Otevřete hello vzorový konfigurační soubor v hello **nano** editor pomocí hello následující příkaz:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Hello zástupné hodnoty nahraďte hello ID a IoT Hub informace o zařízení jste vytvořili a uložili při spuštění hello tohoto kurzu.

Až skončíte, hello obsah souboru deviceinfo hello by měl vypadat podobně jako hello následující ukázka:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).

## <a name="build-hello-sample"></a>Vytvoření ukázkové hello

Pokud jste tak již neučinili, nainstalujte hello požadované balíčky pro hello Microsoft sady SDK zařízení IoT Azure pro jazyk C tak, že spustíte následující příkazy v terminálu na hello malin pí hello:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Teď můžete sestavit hello ukázkové řešení na hello malin platformy:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

Ukázka programu hello teď můžete spustit na hello malin pí. Zadejte příkaz hello:

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

Hello následující ukázkový výstup je příklad výstupu hello, uvidíte hello příkazového řádku na hello malin platformy:

![Výstup z aplikace Malinová platformy][img-raspberry-output]

Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. Na řídicím panelu řešení hello, klikněte na tlačítko **zařízení** toovisit hello **zařízení** stránky. Vyberte platformy vaší malin v hello **seznam zařízení**. Zvolte **metody**:

    ![Seznam zařízení na řídicím panelu][img-list-devices]

1. Na hello **vyvolání metody** vyberte **InitiateFirmwareUpdate** v hello **metoda** rozevíracího seznamu.

1. V hello **FWPackageURI** zadejte **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**. Tento soubor archivu obsahuje hello implementace 2.0 verzi firmwaru hello.

1. Zvolte **InvokeMethod**. aplikace Hello na hello malin pí odešle řídicí panel řešení back toohello potvrzení. Pak spustí proces aktualizace firmwaru hello stažením hello novou verzi firmwaru hello:

    ![Zobrazit historii – metoda][img-method-history]

## <a name="observe-hello-firmware-update-process"></a>Sledovat proces aktualizace firmwaru hello

Můžete sledovat proces aktualizace firmwaru hello při jeho spuštění v zařízení hello a zobrazením hello hlášené vlastnosti v řídicí panel řešení hello:

1. Průběh hello v procesu aktualizace hello můžete zobrazit na hello malin platformy:

    ![Ukázat průběh aktualizace][img-update-progress]

    > [!NOTE]
    > vzdálené monitorování aplikace Hello restartuje bezobslužně po dokončení aktualizace hello. Použijte příkaz hello `ps -ef` tooverify je spuštěná. Pokud chcete proces hello tooterminate, použijte hello `kill` s id procesu hello.

1. Stav hello hello aktualizace firmwaru, můžete zobrazit podle hello zařízení na portálu řešení hello. Hello následující snímek obrazovky ukazuje stav hello a doba trvání u každé fáze procesu aktualizace hello a nová verze firmwaru hello:

    ![Zobrazit stav úlohy][img-job-status]

    Pokud přejdete zpět toohello řídicího panelu, můžete ověřit, že hello zařízení stále odesílá telemetrii následující aktualizaci firmwaru hello.

> [!WARNING]
> Pokud necháte hello vzdálené monitorování řešení, které jsou spuštěné v účtu Azure, se vám účtuje hello, když ji spustí. Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config]. Po dokončení používat, odstraňte hello předkonfigurované řešení z účtu Azure.

## <a name="next-steps"></a>Další kroky

Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md