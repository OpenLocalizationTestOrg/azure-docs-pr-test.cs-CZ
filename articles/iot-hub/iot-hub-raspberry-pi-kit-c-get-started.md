---
title: "aaaRaspberry pí toocloud (C) - tooAzure připojit malin platformy IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte tooAzure malin platformy IoT Hub pro platformy malin toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot Malinová pi, malinová platformy iot hub, malinová platformy odesílání dat toocloud Malinová platformy toocloud"
ms.assetid: 68c0e730-1dc8-4e26-ac6b-573b217b302d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/12/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05086890458e196d7fdc87a53fcabb9386245d6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-c"></a>Připojit malin pí tooAzure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

V tomto kurzu zahájíte učení hello základy práce s malin platformy, na kterém běží Raspbian. Pak zjistíte, jak tooseamlessly propojit své cloudové toohello zařízení pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md). Jádro IoT Windows 10 ukázek najdete toohello [Centrum vývojářů pro Windows](http://www.windowsondevices.com/).

Nemáte sady ještě? Zkuste [online simulátoru malin pí](iot-hub-raspberry-pi-web-simulator-get-started.md). Nebo zakoupení nové kit [zde](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Co dělat

* Vytvoření služby IoT hub.
* Registrovat zařízení pro platformy ve službě IoT hub.
* Instalační program Malinová pí.
* Spuštění ukázkové aplikace na platformy toosend senzor data tooyour IoT hub.

Připojte malin pí tooan IoT hub, který vytvoříte. Pak spusťte ukázkovou aplikaci pro platformy toocollect teploty a vlhkosti data z BME280 senzoru. Nakonec odeslat hello senzor data tooyour IoT hub.

## <a name="what-you-learn"></a>Co se naučíte

* Jak toocreate služby Azure IoT hub a získat nový připojovací řetězec zařízení.
* Jak tooconnect pí s BME280 senzoru.
* Jak data snímačů toocollect spuštěním ukázkovou aplikaci na pí.
* Jak toosend senzor data tooyour IoT hub.

## <a name="what-you-need"></a>Co potřebujete

![Co potřebujete](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* Hello malin pí 2 nebo 3 pí malin panelu.
* Aktivní předplatné Azure. Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.
* Monitorování, USB klávesnice a myši připojujících tooPi.
* Mac nebo počítači se systémem Windows nebo Linux.
* Připojení k Internetu.
* 16 GB nebo vyšší microSD karta.
* USB SD adaptér nebo microSD karta tooburn hello image operačního systému na kartě microSD hello.
* 5 volt 2 amp napájení s hello 6 stopy malých kabel USB.

Hello následující položky jsou volitelné:

* Sestavený Adafruit BME280 teploty, naléhavost a vlhkosti senzoru.
* Breadboard.
* Vodičům můstek 6 F/M.
* Rozptýlený DIODU 10 mm.


> [!NOTE] 
Tyto položky jsou volitelné, protože podpora ukázkový kód hello simulated data snímačů.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a>Instalační program Malinová platformy

### <a name="install-hello-raspbian-operating-system-for-pi"></a>Nainstalujte hello Raspbian operační systém pro platformy

Připravte hello microSD karta pro instalaci bitové kopie Raspbian hello.

1. Stáhněte si Raspbian.
   1. [Stáhnout Raspbian Klára plochy](https://www.raspberrypi.org/downloads/raspbian/) (soubor .zip hello).
   1. Extrahujte hello Raspbian image tooa složky v počítači.
1. Nainstalujte Raspbian toohello microSD karta.
   1. [Stáhněte a nainstalujte nástroj hello Etcher SD karty zapisovací jednotka](https://etcher.io/).
   1. Spusťte Etcher a vyberte bitovou kopii hello Raspbian, které jste extrahovali v kroku 1.
   1. Vyberte jednotku karty microSD hello. Všimněte si, že Etcher může jste již vybrali správnou jednotku hello.
   1. Klikněte na Flash tooinstall Raspbian toohello microSD karta.
   1. Po dokončení instalace odeberte hello microSD karta z vašeho počítače. Je bezpečné tooremove hello microSD karta přímo protože Etcher automaticky vysune nebo odpojí hello microSD karta po dokončení.
   1. Karta microSD hello vložte do pí.

### <a name="enable-ssh-and-spi"></a>Povolit SSH a SPI

1. Připojit pí toohello monitoru, klávesnice a myši, spusťte platformy a znovu přihlásili Raspbian pomocí `pi` jako hello uživatelské jméno a `raspberry` jako hello heslo.
1. Klikněte na tlačítko hello Malinová ikonu > **Předvolby** > **malin pí konfigurace**.

   ![Hello Raspbian předvolby nabídky](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Na hello **rozhraní** nastavte **SPI** a **SSH** příliš**povolit**a potom klikněte na **OK**. Pokud nemáte fyzické senzory a chcete data snímačů toouse simulated, tento krok je volitelný.

   ![Povolit SPI a SSH na Malinová platformy](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
tooenable SSH a SPI, můžete najít další dokumenty odkaz na [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) a [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-hello-sensor-toopi"></a>Připojit tooPi senzor hello

Použijte hello breadboard a můstek vodičům tooconnect DIODU a BME280 tooPi následujícím způsobem. Pokud nemáte hello senzor [tuto část přeskočte](#connect-pi-to-the-network).

![Hello malin platformy a senzor připojení](media/iot-hub-raspberry-pi-kit-c-get-started/3_raspberry-pi-sensor-connection.png)

Hello BME280 senzor teploty a vlhkosti data můžete shromažďovat. A hello DIODU bude blink – Pokud je komunikace mezi zařízením a hello cloudu. 

Senzor kód PIN použijte následující kabeláž hello:

| Spuštění (senzor & DIODU)     | End (panelu)            | Kabel barev   |
| -----------------------  | ---------------------- | ------------: |
| Indikátor VDD (Pin 5G)         | GPIO 4 (Pin 7)         | Bílé kabel   |
| Indikátor zem (Pin 6G)         | ZEM (Pin 6)            | Začernit kabel   |
| VDD (Pin 18F)            | 3.3v opravná (Pin 17)      | Bílé kabel   |
| ZEM (Pin 20F)            | ZEM (Pin 20)           | Začernit kabel   |
| SCK (Pin 21F)            | SPI0 SCLK (Pin 23)     | Oranžové kabel  |
| SDO (Pin 22F)            | SPI0 MISO (Pin 21)     | Žlutý kabel  |
| SDI (Pin 23F)            | SPI0 MOSI (Pin 19)     | Zelená kabel   |
| CS (Pin 24F)             | SPI0 CS (Pin 24)       | Modrý kabel    |

Klikněte na tlačítko tooview [malin pí 2 a 3 mapování kódu Pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) pro vaši informaci.

Po úspěšném připojení BME280 tooyour malin Pi, mělo by být jako níže bitové kopie.

![Připojené platformy a BME280](media/iot-hub-raspberry-pi-kit-c-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a>Připojit síť toohello platformy

Zapněte pí pomocí kabelu USB malých hello a hello napájení. Použití hello Ethernet kabel tooconnect pí tooyour drátové sítě nebo postupujte podle hello [pokyny z hello malin platformy Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect pí tooyour bezdrátové sítě. Po vaší pí byl úspěšně připojeno toohello sítě, je nutné tootake poznámku o hello [IP adresa vašeho čísla pí](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Připojené toowired sítě](media/iot-hub-raspberry-pi-kit-c-get-started/5_power-on-pi.jpg)


## <a name="run-a-sample-application-on-pi"></a>Spuštění ukázkové aplikace na platformy

### <a name="install-hello-prerequisite-packages"></a>Instalace požadovaných součástí balíčků hello

1. Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooyour malin pí hello.
   
   **Uživatelé s Windows**
   1. Stáhněte a nainstalujte [PuTTY](http://www.putty.org/) pro systém Windows. 
   1. Zkopírujte hello IP adresa vaší oddílu pí do hello hostitele název (nebo IP adresu) a vyberte jako typ hello připojení SSH.
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac a Ubuntu uživatelů**
   
   Použijte integrovaného klienta SSH hello na Ubuntu nebo systému macOS. Může být nutné toorun `ssh pi@<ip address of pi>` tooconnect platformy prostřednictvím SSH.
   > [!NOTE] 
   výchozí uživatelské jméno Hello `pi` , a je heslo hello `raspberry`.

1. Instalace hello požadované balíčky pro hello Microsoft sady SDK zařízení IoT Azure pro C a Cmake spuštěním hello následující příkazy:

   ```bash
   grep -q -F 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
   grep -q -F 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
   sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FDA6A393E4C2257F
   sudo apt-get update
   sudo apt-get install -y azure-iot-sdk-c-dev cmake libcurl4-openssl-dev git-core
   git clone git://git.drogon.net/wiringPi
   cd ./wiringPi
   ./build
   ```


### <a name="configure-hello-sample-application"></a>Konfigurace hello ukázkové aplikace

1. Klonování hello ukázkovou aplikaci spuštěním hello následující příkaz:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app
   ```
1. Otevřete konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   cd iot-hub-c-raspberrypi-client-app
   nano config.h
   ```

   ![Konfigurační soubor](media/iot-hub-raspberry-pi-kit-c-get-started/6_config-file.png)

   Existují dvě makra v tomto souboru můžete configurate. Hello nejprve jeden je `INTERVAL`, která definuje hello časový interval (v milisekundách) mezi dvě zprávy, které odesílají toocloud. Hello druhá `SIMULATED_DATA`, což je logickou hodnotu pro jestli toouse simulated data snímačů, nebo ne.

   Pokud jste **nemají hello senzor**, nastavte hello `SIMULATED_DATA` hodnota příliš`1` toomake hello ukázkovou aplikaci vytváření a používání dat snímačů simulované.

1. Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.

### <a name="build-and-run-hello-sample-application"></a>Sestavení a spuštění ukázkové aplikace hello

1. Vytvoření ukázkové aplikace hello spuštěním hello následující příkaz:

   ```bash
   cmake . && make
   ```
   ![Sestavení výstupu](media/iot-hub-raspberry-pi-kit-c-get-started/7_build-output.png)

1. Spuštění ukázkové aplikace hello spuštěním hello následující příkaz:

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Zkontrolujte, zda jste způsobené kopírováním a vkládáním hello zařízení připojovací řetězec do jednoduchých uvozovek a být hello.


Měli byste vidět, že hello následující výstup, že zobrazuje hello senzor dat a hello zpráv, které jsou odeslány tooyour IoT hub.

![Výstup – data snímačů odeslaný tooyour malin platformy IoT hub](media/iot-hub-raspberry-pi-kit-c-get-started/8_run-output.png)

## <a name="next-steps"></a>Další kroky

Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub. toosee hello zprávy, že vaše platformy malin odeslal tooyour IoT hub nebo odesílání zprávy tooyour malin platformy v rozhraní příkazového řádku, najdete v části hello [cloud zařízení spravovat zasílání zpráv s iothub-explorer kurzu](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
