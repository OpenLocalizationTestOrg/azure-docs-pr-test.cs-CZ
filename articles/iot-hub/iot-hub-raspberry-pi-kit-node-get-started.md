---
title: "aaaRaspberry pí toocloud (Node.js) – tooAzure připojit malin platformy IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte tooAzure malin platformy IoT Hub pro platformy malin toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot Malinová pi, malinová platformy iot hub, malinová platformy odesílání dat toocloud Malinová platformy toocloud"
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 57a15ab3984021a9c18ff0aa1316a4d4c6ebdec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-nodejs"></a>Připojit tooAzure malin platformy IoT Hub (Node.js)

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

![Co potřebujete](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

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

### <a name="enable-ssh-and-i2c"></a>Povolit SSH a I2C

1. Připojit pí toohello monitoru, klávesnice a myši, spusťte platformy a znovu přihlásili Raspbian pomocí `pi` jako hello uživatelské jméno a `raspberry` jako hello heslo.
1. Klikněte na tlačítko hello Malinová ikonu > **Předvolby** > **malin pí konfigurace**.

   ![Hello Raspbian předvolby nabídky](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. Na hello **rozhraní** nastavte **I2C** a **SSH** příliš**povolit**a potom klikněte na **OK**. Pokud nemáte fyzické senzory a chcete data snímačů toouse simulated, tento krok je volitelný.

   ![Povolit I2C a SSH na Malinová platformy](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
tooenable SSH a I2C, můžete najít další dokumenty odkaz na [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) a [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-hello-sensor-toopi"></a>Připojit tooPi senzor hello

Použijte hello breadboard a můstek vodičům tooconnect DIODU a BME280 tooPi následujícím způsobem. Pokud nemáte hello senzor [tuto část přeskočte](#connect-pi-to-the-network).

![Hello malin platformy a senzor připojení](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

Hello BME280 senzor teploty a vlhkosti data můžete shromažďovat. A hello DIODU bude blink – Pokud je komunikace mezi zařízením a hello cloudu. 

Senzor kód PIN použijte následující kabeláž hello:

| Spuštění (senzor & DIODU)     | End (panelu)            | Kabel barev   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 5G)             | 3.3v opravná (Pin 1)       | Bílé kabel   |
| ZEM (Pin 7G)             | ZEM (Pin 6)            | Hnědá kabel   |
| SDI (Pin 10G)            | I2C1 SDA (PIN kódu 3)       | Red kabel     |
| SCK (Pin 8G)             | I2C1 SCL (Pin 5)       | Oranžové kabel  |
| Indikátor VDD (Pin 18F)        | GPIO 24 (Pin 18)       | Bílé kabel   |
| Indikátor zem (Pin 17F)        | ZEM (Pin 20)           | Začernit kabel   |

Klikněte na tlačítko tooview [malin pí 2 a 3 mapování kódu Pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) pro vaši informaci.

Po úspěšném připojení BME280 tooyour malin Pi, mělo by být jako níže bitové kopie.

![Připojené platformy a BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a>Připojit síť toohello platformy

Zapněte pí pomocí kabelu USB malých hello a hello napájení. Použití hello Ethernet kabel tooconnect pí tooyour drátové sítě nebo postupujte podle hello [pokyny z hello malin platformy Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect pí tooyour bezdrátové sítě. Po vaší pí byl úspěšně připojeno toohello sítě, je nutné tootake poznámku o hello [IP adresa vašeho čísla pí](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Připojené toowired sítě](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Ujistěte se, že platformy je připojený toohello stejné sítě jako váš počítač. Například pokud je počítač připojený tooa bezdrátové sítě připojené tooa drátové síti při instalaci platformy, nemusíte to vidět hello IP adresu ve výstupu devdisco hello.

## <a name="run-a-sample-application-on-pi"></a>Spuštění ukázkové aplikace na platformy

### <a name="clone-sample-application-and-install-hello-prerequisite-packages"></a>Naklonujte ukázkovou aplikaci a nainstalujte požadované balíčky hello

1. Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooyour malin pí hello.
   
   **Uživatelé s Windows**
   1. Stáhněte a nainstalujte [PuTTY](http://www.putty.org/) pro systém Windows. 
   1. Zkopírujte hello IP adresa vaší oddílu pí do hello hostitele název (nebo IP adresu) a vyberte jako typ hello připojení SSH.
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac a Ubuntu uživatelů**
   
   Použijte integrovaného klienta SSH hello na Ubuntu nebo systému macOS. Může být nutné toorun `ssh pi@<ip address of pi>` tooconnect platformy prostřednictvím SSH.
   > [!NOTE] 
   výchozí uživatelské jméno Hello `pi` , a je heslo hello `raspberry`.

1. Nainstalujte Node.js a NPM tooyour pí.
   
   Nejdřív byste měli zkontrolovat vaše verze Node.js s hello následující příkaz. 
   
   ```bash
   node -v
   ```

   Pokud verze hello je nižší než 4.x nebo není žádná Node.js na vaše Pi, spusťte následující příkaz tooinstall hello nebo aktualizovat Node.js.

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. Klonování hello ukázkovou aplikaci spuštěním hello následující příkaz:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. Nainstalujte všechny balíčky podle hello následující příkaz. Obsahuje zařízení Azure IoT SDK, BME280 senzor knihovny a knihovna vzájemné propojení pí.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   Může trvat několik minut toofinish tento proces instalace v závislosti na připojení k síti.

### <a name="configure-hello-sample-application"></a>Konfigurace hello ukázkové aplikace

1. Otevřete konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   nano config.json
   ```

   ![Konfigurační soubor](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Existují dvě položky v tomto souboru můžete configurate. Hello nejprve jeden je `interval`, která definuje hello časový interval (v milisekundách) mezi dvě zprávy, které odesílají toocloud. Hello druhá `simulatedData`, což je logickou hodnotu pro jestli toouse simulated data snímačů, nebo ne.

   Pokud jste **nemají hello senzor**, nastavte hello `simulatedData` hodnota příliš`true` toomake hello ukázkovou aplikaci vytváření a používání dat snímačů simulované.

1. Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.

### <a name="run-hello-sample-application"></a>Spuštění ukázkové aplikace hello

Spuštění ukázkové aplikace hello spuštěním hello následující příkaz:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Zkontrolujte, zda jste způsobené kopírováním a vkládáním hello zařízení připojovací řetězec do jednoduchých uvozovek a být hello.


Měli byste vidět, že hello následující výstup, že zobrazuje hello senzor dat a hello zpráv, které jsou odeslány tooyour IoT hub.

![Výstup – data snímačů odeslaný tooyour malin platformy IoT hub](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a>Další kroky

Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub. toosee hello zprávy, že vaše platformy malin odeslal tooyour IoT hub nebo odesílání zprávy tooyour malin platformy v rozhraní příkazového řádku, najdete v části hello [cloud zařízení spravovat zasílání zpráv s iothub-explorer kurzu](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
