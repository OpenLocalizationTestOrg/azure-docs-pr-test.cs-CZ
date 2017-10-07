---
title: "aaaESP8266 toocloud - připojení prolnutí HUZZAH ESP8266 tooAzure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte Adafruit prolnutí HUZZAH ESP8266 tooAzure IoT Hub pro něj toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.openlocfilehash: 44fd47232488948d21c7aa71bdd865397e41e63e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-tooazure-iot-hub-in-hello-cloud"></a>Připojit Adafruit prolnutí HUZZAH ESP8266 tooAzure IoT Hub v cloudu hello

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Připojení mezi DHT22, prolnutí HUZZAH ESP8266 a IoT Hub](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Co dělat


Připojte Adafruit prolnutí HUZZAH ESP8266 tooan IoT hub, který vytvoříte. Pak spusťte ukázkovou aplikaci na ESP8266 toocollect hello teploty a vlhkosti data z DHT22 senzoru. Nakonec odeslat hello senzor data tooyour IoT hub.

> [!NOTE]
> Pokud používáte jiné ESP8266 panely, můžete stále postupujte podle těchto kroků tooconnect ho tooyour IoT hub. V závislosti na panelu hello ESP8266 používáte, může být nutné tooreconfigure hello `LED_PIN`. Například pokud používáte ESP8266 z AI Thinker, můžete změnit z `0` příliš`2`. Nemáte sady ještě? Získat z hello [webu Azure](http://azure.com/iotstarterkits).




## <a name="what-you-learn"></a>Co se naučíte

* Jak toocreate služby IoT hub a registrovat zařízení pro prolnutí HUZZAH ESP8266
* Jak tooconnect ESP8266 HUZZAH prolnutí s hello senzor a počítač
* Jak data snímačů toocollect spuštěním ukázkovou aplikaci na prolnutí HUZZAH ESP8266
* Jak toosend hello senzor data tooyour IoT hub

## <a name="what-you-need"></a>Co potřebujete

![součásti potřebné pro kurz hello](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete tuto operaci je třeba hello následujících částí z vaší prolnutí HUZZAH ESP8266 Starter Kit:

* Hello prolnutí HUZZAH ESP8266 panelu
* Micro USB tooType kabelu A USB

Musíte taky hello následujících věcí pro vývojové prostředí:

* Aktivní předplatné Azure. Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.
* Mac nebo počítači se systémem Windows nebo Ubuntu.
* Bezdrátové sítě pro prolnutí HUZZAH ESP8266 tooconnect k.
* Nástroj pro konfiguraci internetové připojení toodownload hello.
* [Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 nebo novější. Starší verze nepodporují s knihovnou AzureIoT hello.

Hello následující položky jsou volitelné v případě, že nemáte senzoru. Máte také možnost hello použití dat snímačů simulované.

* Senzor teploty a vlhkosti Adafruit DHT22
* Breadboard
* Vodičům můstek M/M


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-hello-sensor-and-your-computer"></a>Připojit prolnutí HUZZAH ESP8266 s hello senzor a počítače
V této části připojit hello senzorů tooyour panelu. Potom připojíte počítač tooyour zařízení pro další použití.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-huzzah-esp8266"></a>Připojit DHT22 teploty a vlhkosti senzor tooFeather HUZZAH ESP8266

Použijte hello breadboard a můstek vodičům toomake hello připojení následujícím způsobem. Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.

![odkaz na připojení](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


Senzor kód PIN použijte následující kabeláž hello:


| Spuštění (senzor)           | End (panelu)           | Kabel barev   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 31F)            | 3v (připnout 58H)           | Red kabel     |
| DATA (Pin 32F)           | GPIO 2 (Pin 46A)       | Modrý kabel    |
| ZEM (Pin 34F)            | ZEM (PIn 56I)          | Začernit kabel   |

Další informace najdete v tématu [Adafruit DHT22 senzor instalace](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) a [uspořádání Adafruit prolnutí HUZZAH Esp8266 kolíků](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).



Teď vaše ESP8266 Huzzah prolnutí by měly být připojené s pracovní senzoru.

![Spojte se s prolnutí Huzzah DHT22](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-tooyour-computer"></a>Připojte počítač tooyour prolnutí HUZZAH ESP8266

Jak ukazuje následující, použijte hello Micro USB tooType A USB kabel tooconnect prolnutí HUZZAH ESP8266 tooyour počítač.

![Připojte počítač tooyour prolnutí Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Přidání oprávnění sériového portu (pouze Ubuntu)


Pokud používáte Ubuntu, ujistěte se, že máte oprávnění toooperate hello na hello USB port z prolnutí HUZZAH ESP8266. oprávnění tooadd sériového portu, postupujte takto:


1. Spusťte následující příkazy v terminálu hello:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Zobrazí jednu z následujících výstupy hello:

   * CRW-rw---1 kořenové uucp xxxxxxxx
   * CRW-rw---xxxxxxxx síti službou 1 root

   Ve výstupu hello, Všimněte si, že `uucp` nebo `dialout` je název vlastníka skupiny hello hello portu USB.

1. Přidáte skupinu toohello uživatelů hello spuštěním hello následující příkaz:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`je název skupiny vlastníka hello, kterou jste získali v předchozím kroku hello. `<username>`je Ubuntu uživatelské jméno.

1. Odhlaste se od Ubuntu a znovu se přihlaste pro změnu tooappear hello.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Shromažďování dat snímačů a odeslat tooyour IoT hub

V této části nasazení a spuštění ukázkové aplikace na prolnutí HUZZAH ESP8266. ukázkové aplikace Hello bliká hello DIODU na prolnutí HUZZAH ESP8266 a odešle hello teploty a vlhkosti data shromážděná z hello DHT22 senzor tooyour služby IoT hub.

### <a name="get-hello-sample-application-from-github"></a>Získat hello ukázkovou aplikaci z webu GitHub

Ukázková aplikace Hello jsou hostované na Githubu. Klonování úložiště hello ukázka, která obsahuje ukázkovou aplikaci hello z Githubu. tooclone hello Ukázka úložiště, postupujte takto:

1. Otevřete příkazový řádek nebo okno terminálu.
1. Přejděte tooa složku, kam chcete hello ukázkové aplikace toobe uložené.
1. Spusťte následující příkaz hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

Instalujte balíček hello pro prolnutí HUZZAH ESP8266 v hello Arduino IDE:

1. Otevřete složku hello se uloží hello ukázkovou aplikaci.
1. Otevřete soubor app.ino hello ve složce aplikace hello v hello Arduino IDE.

   ![Otevřete hello ukázkovou aplikaci v Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. V hello Arduino IDE, klikněte na **soubor** > **Předvolby**.
1. V hello **Předvolby** dialogovém okně klikněte na další toohello ikonu hello **další adresy URL Manager panely** pole.
1. V místním okně hello, zadejte následující adresu URL hello a pak klikněte na tlačítko **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Adresa url balíčku bodu tooa v Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. V hello **předvoleb** dialogové okno, klikněte na tlačítko **OK**.
1. Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a poté vyhledejte esp8266.

   Panely Manager znamená, že je nainstalována ESP8266 verzí 2.2.0 nebo novější.

   ![je nainstalovaný balíček esp8266 Hello](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. Klikněte na tlačítko **nástroje** > **Tabule** > **Adafruit HUZZAH ESP8266**.

### <a name="install-necessary-libraries"></a>Instalace nezbytné knihovny

1. V hello Arduino IDE, klikněte na **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.
1. Vyhledejte následující názvy knihoven jeden po druhém hello. Pro každou knihovnu, která zjistíte, klikněte na tlačítko **nainstalovat**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Nemáte skutečné senzor DHT22?

Hello ukázkovou aplikaci můžete simulovat teploty a vlhkosti dat v případě, že nemáte skutečné DHT22 senzoru. tooset hello ukázkové aplikace toouse simulated dat, postupujte takto:

1. Otevřete hello `config.h` souboru v hello `app` složky.
1. Vyhledejte následující řádek kódu hello a změňte hodnotu hello z `false` příliš`true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurace hello ukázková aplikace toouse simulated data](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. Uložte soubor hello s `Control-s`.

### <a name="deploy-hello-sample-application-toofeather-huzzah-esp8266"></a>Nasazení hello ukázkové aplikace tooFeather HUZZAH ESP8266

1. V hello Arduino IDE, klikněte na **nástroj** > **Port**a potom klikněte na hello sériového portu pro prolnutí HUZZAH ESP8266.
1. Klikněte na tlačítko **nákresu** > **nahrát** toobuild a nasadit hello ukázkové aplikace tooFeather HUZZAH ESP8266.

### <a name="enter-your-credentials"></a>Zadejte přihlašovací údaje.

Po úspěšném dokončení nahrávání hello postupujte podle těchto kroků tooenter vaše přihlašovací údaje:

1. V hello Arduino IDE, klikněte na **nástroje** > **sériové monitorování**.
1. V okně hello sériové sledování Všimněte si, rozevírací seznamy hello dva v pravém dolním rohu, hello.
1. Vyberte **žádný řádek ukončování** pro hello levém rozevíracího seznamu.
1. Vyberte **115200 přenosová** pro hello právo rozevíracího seznamu.
1. Hello vstupní pole umístěné v hello horní části okna sériové sledování hello, zadejte následující informace, pokud se zobrazí výzva tooprovide hello a potom klikněte na **odeslat**.
   * SSID sítě Wi-Fi
   * Heslo Wi-Fi
   * Řetězec připojení zařízení

> [!Note]
> Hello pověření informace jsou uloženy v hello EEPROM prolnutí HUZZAH ESP8266. Pokud kliknutí na tlačítko resetovat hello na hello prolnutí HUZZAH ESP8266 panelu ukázkové aplikace hello dotáže se tooerase hello informace. Zadejte `Y` toohave hello informace vymazat. Zobrazí se výzva tooprovide hello informace ještě jednou.

### <a name="verify-hello-sample-application-is-running-successfully"></a>Zkontrolujte, zda hello ukázkové aplikace je úspěšně spuštěna.

Pokud se zobrazí následující hello výstup z okna hello sériové sledování a hello blikat DIODU na prolnutí HUZZAH ESP8266, hello ukázkové aplikace je úspěšně spuštěna.

![Závěrečný výstup v integrovaném vývojovém prostředí Arduino](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Další kroky

Máte úspěšně připojen prolnutí HUZZAH ESP8266 tooyour IoT hub a odeslat hello zachytit senzor data tooyour IoT hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

