---
title: "M0 toocloud: připojení Wi-Fi M0 prolnutí tooAzure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak tooset až a připojení přes Wi-Fi Adafruit prolnutí M0 tooAzure IoT Hub toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/16/2017
ms.author: xshi
ms.openlocfilehash: 6aabeb961a50ba5d3934f77eb1ccda4af1bf64c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-m0-wifi-tooazure-iot-hub-in-hello-cloud"></a>Připojení přes Wi-Fi Adafruit prolnutí M0 tooAzure IoT Hub v cloudu hello
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Připojení mezi BME280, prolnutí M0 Wi-Fi a IoT Hub](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

V tomto kurzu zahájíte učení hello základy práce s Arduino panel. Pak zjistíte, jak tooseamlessly propojit své cloudové toohello zařízení pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

## <a name="what-you-do"></a>Co dělat

Připojte centra IoT tooan Adafruit prolnutí M0 Wi-Fi, který vytvoříte. Pak spusťte ukázkovou aplikaci na Wi-Fi M0 toocollect hello teploty a vlhkosti dat z BME280. Nakonec odeslat hello senzor data tooyour IoT hub.


## <a name="what-you-learn"></a>Co se naučíte

* Jak toocreate služby IoT hub a registrovat zařízení pro prolnutí M0 Wi-Fi
* Jak tooconnect prolnutí M0 Wi-Fi s hello senzor a počítač
* Jak data snímačů toocollect spuštěním ukázkovou aplikaci na prolnutí M0 Wi-Fi
* Jak toosend hello senzor data tooyour IoT hub

## <a name="what-you-need"></a>Co potřebujete

![součásti potřebné pro kurz hello](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete tuto operaci je třeba hello následujících částí z vaší prolnutí M0 Wi-Fi Starter Kit:

* Hello Tabule prolnutí M0 Wi-Fi
* Micro USB tooType kabelu A USB

Musíte taky hello následujících věcí pro vývojové prostředí:

* Aktivní předplatné Azure. Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.
* Mac nebo počítači se systémem Windows nebo Ubuntu.
* Bezdrátové sítě Wi-Fi M0 prolnutí tooconnect k.
* Internetu toodownload hello konfigurační nástroj pro připojení.
* [Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 nebo novější. Starší verze nepodporují s knihovnou Azure IoT Hub hello.

Pokud nemáte senzoru, hello následující položky jsou volitelné. Máte také možnost hello pomocí simulované senzor dat:

* Senzor teploty a vlhkosti BME280
* Breadboard
* Vodičům můstek M/M

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-hello-sensor-and-your-computer"></a>Připojení Wi-Fi M0 prolnutí s hello senzor a počítače
V této části připojit hello senzorů tooyour panelu. Potom připojíte počítač tooyour zařízení pro další použití.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-m0-wifi"></a>Připojit DHT22 teploty a vlhkosti senzor tooFeather M0 Wi-Fi

Použití hello breadboard a můstek vodičům toomake hello připojení. Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.

![odkaz na připojení](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


Senzor kód PIN použijte následující kabeláž hello:


| Spuštění (senzor)           | End (panelu)            | Kabel barev   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 27A)            | 3v (3A PIN kódu)            | Red kabel     |
| ZEM (Pin 29A)            | ZEM (6A PIN kódu)           | Začernit kabel   |
| SCK (Pin 30A)            | SCK (Pin 12A)          | Žlutý kabel  |
| SDO (Pin 31A)            | MI (Pin 14A)           | Bílé kabel   |
| SDI (Pin 32A)            | M0 (Pin 13A)           | Modrý kabel    |
| CS (Pin 33A)             | GPIO 5 (Pin 15J)       | Oranžové kabel  |

Další informace najdete v tématu [Adafruit BME280 vlhkosti + barometrický tlak + teplotní snímač volnými](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) a [uspořádání kolíků Adafruit prolnutí M0 Wi-Fi](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).



Nyní by měly být připojené vaší prolnutí M0 Wi-Fi s pracovní senzoru.

![Spojte se s prolnutí Huzzah DHT22](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-tooyour-computer"></a>Připojte počítač tooyour prolnutí M0 Wi-Fi

Použijte hello Micro USB tooType A USB kabel tooconnect prolnutí M0 Wi-Fi tooyour počítač, jak je znázorněno:

![Připojte počítač tooyour prolnutí Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Přidání oprávnění sériového portu (pouze Ubuntu)

Pokud používáte Ubuntu, ujistěte se, že máte oprávnění toooperate hello na hello USB port z prolnutí M0 Wi-Fi. oprávnění tooadd sériového portu, postupujte takto:


1. V terminálu spusťte následující příkazy hello:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Zobrazí jednu z následujících výstupy hello:

   * CRW-rw---1 kořenové uucp xxxxxxxx
   * CRW-rw---xxxxxxxx síti službou 1 root

   Ve výstupu hello, Všimněte si, že `uucp` nebo `dialout` je název vlastníka skupiny hello hello portu USB.

2. tooadd hello toohello skupinu uživatelů, spusťte následující příkaz hello:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   V předchozím kroku hello získat název vlastníka skupiny hello `<group-owner-name>`. Uživatelské jméno Ubuntu `<username>`.

3. Pro změnu tooappear hello odhlaste se od Ubuntu a potom se znovu přihlaste.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Shromažďování dat snímačů a odeslat tooyour IoT hub

V této části nasazení a spuštění ukázkové aplikace na prolnutí M0 Wi-Fi. Ukázková aplikace Hello díky hello DIODU blikání na prolnutí M0 Wi-Fi. Pak odešle hello teploty a vlhkosti data shromážděná z hello BME280 senzor tooyour služby IoT hub.

### <a name="get-hello-sample-application-from-github-and-prepare-hello-arduino-ide"></a>Získat hello ukázkovou aplikaci z webu GitHub a příprava hello Arduino IDE

Ukázková aplikace Hello jsou hostované na Githubu. Klonování úložiště hello ukázka, která obsahuje ukázkovou aplikaci hello z Githubu. tooclone hello Ukázka úložiště, postupujte takto:

1. Otevřete příkazový řádek nebo okno terminálu.

2. Přejděte tooa složku, kam chcete hello ukázkové aplikace toobe uložené.
3. Spusťte následující příkaz hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-hello-package-for-feather-m0-wifi-in-hello-arduino-ide"></a>Instalovat balíček hello pro prolnutí M0 Wi-Fi v hello Arduino IDE

1. Otevřete složku hello se uloží hello ukázkovou aplikaci.

2. Otevřete soubor app.ino hello ve složce aplikace hello v hello Arduino IDE.

   ![Otevřete hello ukázkovou aplikaci v Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. Klikněte na tlačítko **soubor** > **Předvolby** (Windows nebo Linuxem) nebo **Arduino** > **Předvolby** (Mac) a zkopírujte a Vložení hello dole na odkaz do hello **další adresy URL Manager panely** možnost hello předvolby Arduino IDE.
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a pak nainstalujte hello `Arduino SAMD Boards` verze `1.6.2` nebo novější. 

1. Potom v hello stejného časového období, nainstalujte `Adafruit SAMD Boards` tooadd hello Tabule soubor definice balíčku.

   ![je nainstalovaný balíček esp8266 Hello](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. Klikněte na tlačítko **nástroje** > **Tabule** > **Adafruit M0 Wi-Fi**.

5. Instalace ovladačů (jenom Windows). Po připojení Wi-Fi M0 prolnutí, může být nutné tooinstall ovladač. Klikněte na tlačítko [hello odkaz ke stažení na webovou stránku hello](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello ovladač Instalační služby. Postupujte podle hello kroky tooinstall hello ovladače, které chcete.

### <a name="install-necessary-libraries"></a>Instalace nezbytné knihovny

1. V hello Arduino IDE, klikněte na **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.

2. Vyhledejte následující názvy knihoven jeden po druhém hello. Pro každou knihovnu, která zjistíte, klikněte na tlačítko **nainstalovat**:

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. Nainstalujte ručně `Adafruit_WINC1500`. Přejděte příliš[tento web](https://github.com/adafruit/Adafruit_WINC1500) a klikněte na tlačítko **klonovat nebo stáhnout** > **stáhnout ZIP**. Potom v IDE vaší Arduino přejděte příliš**nákresu** > **zahrnují knihovny** > **přidat .zip knihovna** a přidejte soubor zip hello.

### <a name="use-hello-sample-application-if-you-dont-have-a-real-bme280-sensor"></a>Pokud nemáte skutečné senzor BME280 použít hello ukázkové aplikace

Pokud nemáte skutečné senzor BME280, hello ukázkovou aplikaci můžete simulovat teploty a vlhkosti data. tooset hello ukázkové aplikace toouse simulated dat, postupujte takto:

1. Otevřete hello `config.h` souboru v hello `app` složky.

2. Vyhledejte následující řádek kódu hello a změňte hodnotu hello z `false` příliš`true`:

   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurace hello ukázková aplikace toouse simulated data](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. Uložte soubor hello s `Control-s`.

### <a name="deploy-hello-sample-application-toofeather-m0-wifi"></a>Nasazení hello ukázkové aplikace tooFeather M0 Wi-Fi

1. V hello Arduino IDE, klikněte na **nástroj** > **Port**a potom klikněte na hello sériového portu pro prolnutí M0 Wi-Fi.

2. Klikněte na tlačítko **nákresu** > **nahrát** toobuild a nasadit hello ukázkové aplikace tooFeather M0 Wi-Fi.

### <a name="enter-your-credentials"></a>Zadejte přihlašovací údaje.

Po úspěšném dokončení nahrávání hello postupujte podle těchto kroků tooenter vaše přihlašovací údaje:

1. V hello Arduino IDE, klikněte na **nástroje** > **sériové monitorování**.

2. V pravém dolním rohu, hello okna hello sériové monitorování, vyberte **žádný řádek ukončování** v rozevíracím seznamu hello na levé straně hello.
3. Vyberte **115200 přenosová** v rozevíracím seznamu hello na hello správné.
4. Hello vstupní pole v horní části hello, zadejte následující informace, pokud jste hello výzva tooprovide a klikněte na **odeslat**:

   * SSID sítě Wi-Fi
   * Heslo Wi-Fi
   * Řetězec připojení zařízení

> [!Note]
> Hello pověření informace jsou uloženy v hello EEPROM z prolnutí M0 Wi-Fi. Pokud klepnete na tlačítko Obnovit hello na hello Tabule prolnutí M0 Wi-Fi, hello ukázkovou aplikaci dotáže se tooerase hello informace. Zadejte `Y` tooerase hello informace. Zobrazí se dotaz tooprovide hello informace ještě jednou.

### <a name="verify-that-hello-sample-application-is-running-successfully"></a>Ověřte, zda text hello ukázkové aplikace úspěšně spuštěna

Pokud se zobrazí následující hello výstup z okna sériové sledování hello a hello blikat DIODU na prolnutí M0 sítě Wi-Fi, hello ukázkové aplikace je spuštěna úspěšně:

![Závěrečný výstup v integrovaném vývojovém prostředí Arduino](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Další kroky

Úspěšně jste připojení Wi-Fi M0 prolnutí tooyour IoT hub a odeslat hello zachytit senzor data tooyour IoT hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

