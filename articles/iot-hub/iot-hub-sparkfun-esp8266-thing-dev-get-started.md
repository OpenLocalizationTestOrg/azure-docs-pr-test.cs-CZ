---
title: "aaaESP8266 toocloud - tooAzure připojit Sparkfun ESP8266 věc Dev IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte Sparkfun ESP8266 věc Dev tooAzure IoT Hub pro něj toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 19b249df23b6df516634853521c6d532f51014da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-tooazure-iot-hub-in-hello-cloud"></a>Připojit Sparkfun ESP8266 věc Dev tooAzure IoT Hub v cloudu hello

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![připojení mezi DHT22, co vývojářů a IoT Hub](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>Co provedete

Připojte Sparkfun ESP8266 věc Dev tooan IoT hub, které vytvoříte. Pak spusťte ukázkovou aplikaci na ESP8266 toocollect teploty a vlhkosti data z DHT22 senzoru. Nakonec odešlete hello senzor data tooyour IoT hub.

> [!NOTE]
> Pokud používáte jiné ESP8266 panely, můžete stále postupujte podle těchto kroků tooconnect ho tooyour IoT hub. V závislosti na hello ESP8266 Tabule, kterou používáte, může být nutné tooreconfigure hello `LED_PIN`. Například pokud používáte ESP8266 z AI Thinker, můžete změnit z `0` příliš`2`. Ještě není hotová kit?: klikněte na tlačítko [sem](http://azure.com/iotstarterkits)

## <a name="what-you-will-learn"></a>Co se dozvíte

* Jak toocreate služby IoT hub a registrovat zařízení za účelem věcí
* Jak tooconnect věc Dev s hello senzor a váš počítač.
* Jak data snímačů toocollect spuštěním ukázkovou aplikaci na věc účelem
* Jak toosend hello senzor data tooyour IoT hub.

## <a name="what-you-will-need"></a>Co budete potřebovat

![součásti potřebné pro kurz hello](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete tuto operaci je třeba hello následujících částí z vaší věc Dev Starter Kit:

* Hello Tabule Sparkfun ESP8266 věc vývojářů.
* Micro USB tooType kabelu A USB.

Potřebujete taky následující hello vývojové prostředí:

* Aktivní předplatné Azure. Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.
* Mac nebo počítači se systémem Windows nebo Ubuntu.
* Bezdrátové sítě pro tooconnect Sparkfun ESP8266 věc Dev k.
* Nástroj pro konfiguraci internetové připojení toodownload hello.
* [Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 (nebo novější), dřívější verze nebudou pracovat s knihovnou AzureIoT hello.

Hello následující položky jsou volitelné v případě, že nemáte senzoru. Máte také možnost hello použití dat snímačů simulované.

* Adafruit DHT22 teploty a vlhkosti senzoru.
* Breadboard.
* Vodičům můstek M/M.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-hello-sensor-and-your-computer"></a>Připojit ESP8266 věc Dev s hello senzor a počítače

### <a name="connect-a-dht22-temperature-and-humidity-sensor-tooesp8266-thing-dev"></a>Senzor teploty a vlhkosti DHT22 připojit tooESP8266 věc vývojářů

Použijte hello breadboard a můstek vodičům toomake hello připojení následujícím způsobem. Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.

![odkaz na připojení](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

Pro kód PIN senzor použijeme následující kabeláž hello:

| Spuštění (senzor)           | End (panelu)           | Kabel barev   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 27F)            | 3v (8A PIN kódu)           | Red kabel     |
| DATA (Pin 28F)           | GPIO 2 (9A PIN kódu)       | Bílé kabel    |
| ZEM (Pin 30F)            | ZEM (7J kód Pin)          | Začernit kabel   |


- Další informace najdete v tématu: [DHT22 senzor instalace](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) a [specifikace Sparkfun ESP8266 věc vývojářů](https://www.sparkfun.com/products/13711)

Teď vaše Sparkfun ESP8266 věc Dev by měly být připojené s pracovní senzoru.

![připojit dht22 s ESP8266 věc vývojářů](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-tooyour-computer"></a>Připojte počítač tooyour Sparkfun ESP8266 věc vývojářů

Použijte hello Micro USB tooType A USB kabel tooconnect Sparkfun ESP8266 věc Dev tooyour počítači následujícím způsobem.

![Připojte počítač tooyour huzzah prolnutí](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Přidání oprávnění sériového portu – pouze Ubuntu

Pokud používáte Ubuntu, zkontrolujte, zda že má uživatel normální hello oprávnění toooperate na hello USB port z Sparkfun ESP8266 věc účelem tooadd sériového portu oprávnění pro běžné uživatele, postupujte takto:

1. Spusťte následující příkazy v terminálu hello:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Zobrazí jednu z následujících výstupy hello:

   * CRW-rw---1 kořenové uucp xxxxxxxx
   * CRW-rw---xxxxxxxx síti službou 1 root

   Ve výstupu hello, Všimněte si `uucp` nebo `dialout` tedy hello vlastníka název skupiny hello portu USB.

1. Přidáte skupinu toohello uživatelů hello spuštěním hello následující příkaz:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`je název skupiny vlastníka hello, kterou jste získali v předchozím kroku hello. `<username>`je Ubuntu uživatelské jméno.

1. Odhlaste Ubuntu a přihlaste se znovu pro hello změnit tootake efekt.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Shromažďování dat snímačů a odeslat tooyour IoT hub

V této části můžete nasazení a spuštění ukázkové aplikace na Sparkfun ESP8266 věc účelem Hello ukázkovou aplikaci bliká hello DIODU na Sparkfun ESP8266 věc vývojářů a odešle hello teploty a vlhkosti data shromážděná z hello DHT22 senzor tooyour služby IoT hub.

### <a name="get-hello-sample-application-from-github"></a>Získat hello ukázkovou aplikaci z webu GitHub

Ukázková aplikace Hello jsou hostované na Githubu. Klonování úložiště hello ukázka, která obsahuje ukázkovou aplikaci hello z Githubu. tooclone hello Ukázka úložiště, postupujte takto:

1. Otevřete příkazový řádek nebo okno terminálu.
1. Přejděte tooa složku, kam chcete hello ukázkové aplikace toobe uložené.
1. Spusťte následující příkaz hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Instalovat balíček hello pro vývojáře věc ESP8266 Sparkfun v integrovaném vývojovém prostředí Arduino:

1. Otevřete složku hello se uloží hello ukázkovou aplikaci.
1. Otevřete soubor app.ino hello ve složce aplikace hello v Arduino IDE.

   ![Otevřete hello ukázkovou aplikaci v arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. V hello Arduino IDE, klikněte na **soubor** > **Předvolby**.
1. V hello **Předvolby** dialogové okno pole, klikněte na tlačítko Další toohello ikonu hello **další adresy URL Manager panely** textové pole.
1. V místním okně hello, zadejte následující adresu URL hello a pak klikněte na tlačítko **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Adresa url balíčku bodu tooa v arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. V hello **předvoleb** dialogové okno, klikněte na tlačítko **OK**.
1. Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a poté vyhledejte esp8266.
   Musí být nainstalován ESP8266 verzí 2.2.0 nebo novější.

   ![je nainstalovaný balíček esp8266 Hello](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Klikněte na tlačítko **nástroje** > **Tabule** > **Sparkfun ESP8266 věc Dev**.

### <a name="install-necessary-libraries"></a>Instalace nezbytné knihovny

1. V hello Arduino IDE, klikněte na **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.
1. Vyhledejte následující názvy knihoven jeden po druhém hello. Pro každou hello knihovně zjistíte, klikněte na tlačítko **nainstalovat**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Nemáte skutečné senzor DHT22?

Hello ukázkovou aplikaci můžete simulovat teploty a vlhkosti dat v případě, že nemáte skutečné DHT22 senzoru. data pro toouse simulated tooenable hello ukázkové aplikace, postupujte takto:

1. Otevřete hello `config.h` souboru v hello `app` složky.
1. Vyhledejte následující řádek kódu hello a změňte hodnotu hello z `false` příliš`true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurace hello ukázková aplikace toouse simulated data](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. Uložit s `Control-s`.

### <a name="deploy-hello-sample-application-toosparkfun-esp8266-thing-dev"></a>Nasazení hello ukázkové aplikace tooSparkfun ESP8266 věc vývojářů

1. V hello Arduino IDE, klikněte na **nástroj** > **Port**a potom klikněte na sériového portu hello za účelem co ESP8266 Sparkfun
1. Klikněte na tlačítko **nákresu** > **nahrát** toobuild a nasadit hello ukázkové aplikace tooSparkfun ESP8266 věc účelem

> [!Note]
> Pokud používáte systému macOS by mohli pravděpodobně zobrazit následující zprávy při odesílání hello. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Otevřete okno vaší ternimal a dokončit následující akce toosolve tento problém.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Zadejte přihlašovací údaje.

Po úspěšném dokončení nahrávání hello postupujte podle kroků tooenter hello vaše přihlašovací údaje:

1. V hello Arduino IDE, klikněte na **nástroje** > **sériové monitorování**.
1. V okně hello sériové sledování Všimněte si, rozevírací seznamy hello dva na hello dolního rohu.
1. Vyberte **žádný řádek ukončování** pro hello levém rozevíracího seznamu.
1. Vyberte **115200 přenosová** pro hello právo rozevíracího seznamu.
1. Hello vstupní pole umístěné v hello horní části okna sériové sledování hello, zadejte následující informace, pokud se zobrazí výzva tooprovide hello a potom klikněte na **odeslat**.
   * SSID sítě Wi-Fi
   * Heslo Wi-Fi
   * Řetězec připojení zařízení

> [!Note]
> Hello pověření informace jsou uloženy v hello EEPROM z Sparkfun ESP8266 věc účelem Pokud klepnete na tlačítko Obnovit hello na hello Tabule Sparkfun ESP8266 věc vývojářů, hello ukázkovou aplikaci požádá, pokud chcete, aby tooerase hello informace. Zadejte `Y` toohave hello informace vymazat a jste vyzváni tooprovide hello informace znovu.

### <a name="verify-hello-sample-application-is-running-successfully"></a>Zkontrolujte, zda hello ukázkové aplikace je úspěšně spuštěna.

Pokud se zobrazí následující hello výstup z okna hello sériové sledování a hello blikat DIODU na Sparkfun ESP8266 věc vývojářů, hello ukázkové aplikace je úspěšně spuštěna.

![závěrečný výstup v integrovaném vývojovém prostředí arduino](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Další kroky

Úspěšně jste připojené vývojářů věc ESP8266 Sparkfun tooyour IoT hub a odeslaných hello zachytit senzor data tooyour IoT hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
