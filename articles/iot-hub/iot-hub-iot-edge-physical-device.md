---
title: "aaaUse fyzického zařízení s hranou Azure IoT | Microsoft Docs"
description: "Jak toouse rozbočovači Texas Instruments SensorTag zařízení toosend data tooan IoT prostřednictvím brány IoT Edge spuštěné na zařízení malin pí 3. Brána Hello vytvořená s využitím Azure IoT okraj."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: andbuc
ms.openlocfilehash: a2385accdbd99012ad094232653ee47d4e5c7839
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-tooforward-device-to-cloud-messages-tooiot-hub"></a>Použití Azure IoT Edge na tooIoT zpráv typu zařízení cloud tooforward malin pí rozbočovače

Tento návod hello [Bluetooth nízkou energie ukázka] [ lnk-ble-samplecode] ukazuje, jak toouse [Azure IoT Edge] [ lnk-sdk] na:

* Předat dál zařízení cloud telemetrie tooIoT centra z fyzického zařízení.
* Směrování příkazů z fyzického zařízení IoT Hub tooa.

Tento návod ilustruje:

* **Architektura**: důležité architektury informace o ukázkové nízkou energie hello Bluetooth.
* **Sestavení a spuštění**: požadované toobuild hello kroky a spusťte hello ukázková.

## <a name="architecture"></a>Architektura

Hello návod ukazuje, jak toobuild a spusťte IoT vstupní brána v pí 3 malin používající Raspbian Linux. Hello brány je sestaven pomocí IoT okraj. Ukázka Hello používá data teploty toocollect zařízení Texas Instruments SensorTag Bluetooth nízká energie (Povolit).

Když spustíte hello IoT hraniční brána je:

* Připojí zařízení SensorTag tooa pomocí protokolu hello Bluetooth nízká energie (Povolit).
* Připojí tooIoT centra pomocí protokolu hello HTTP.
* Předává telemetrie z tooIoT zařízení SensorTag hello rozbočovače.
* Směrování příkazů ze zařízení SensorTag toohello IoT Hub.

Brána Hello obsahuje hello následující moduly hraniční IoT:

* A *zakázat modulu* , rozhraní zakázat zařízení tooreceive teploty daty ze zařízení hello a odesílání příkazů toohello zařízení.
* A *zakázat cloudu toodevice modulu* který překládá JSON zprávy odeslané ze služby IoT Hub do zakázat pokyny hello *zakázat modulu*.
* A *protokoly modulu* který protokoluje všechny brány zprávy tooa místního souboru.
* *Modulem mapování identit* který překládá mezi adresy MAC povolit zařízení a identit zařízení Azure IoT Hub.
* *IoT Hub modulu* které odesílá telemetrická data tooan IoT hub a přijímá příkazy zařízení ze služby IoT hub.
* A *zakázat tiskárny modulu* , interpretuje telemetrická data ze zařízení zakázat hello a vytiskne formátovaných dat toohello konzoly tooenable řešení potíží a ladění.

### <a name="how-data-flows-through-hello-gateway"></a>Tok dat prostřednictvím brány hello

Hello následující Blokový diagram znázorňuje hello telemetrie nahrávání dat toku kanál:

![Kanál brány odesílání telemetrie](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

Hello kroky, které položky telemetrie trvá, cestování z tooIoT zařízení povolit centru jsou:

1. zařízení zakázat Hello generuje ukázku teploty a odešle přes Bluetooth toohello zakázat modulu v bráně hello.
1. modul zakázat Hello obdrží hello ukázka a vydává je toohello zprostředkovatele spolu s hello adresa MAC zařízení hello.
1. modul mapování identit Hello převezme tuto zprávu a používá k interní tabulce tootranslate hello adresa MAC zařízení hello do identity zařízení IoT Hub. Identita zařízení IoT Hub se skládá z ID zařízení a klíč zařízení.
1. modul mapování identit Hello publikuje novou zprávu, která obsahuje hello teploty ukázková data, adresa MAC hello hello zařízení, hello ID zařízení a klíč zařízení hello.
1. Hello modulu služby IoT Hub přijímá tuto novou zprávu (generované modulem mapování identit hello) a vydává je tooIoT rozbočovače.
1. modul protokolovacího nástroje Hello zaznamená všechny zprávy z místního souboru tooa hello zprostředkovatele.

Hello následující Blokový diagram znázorňuje hello zařízení příkaz datovém toku kanálu:

![Zařízení příkaz brány kanálu](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. Hello modulu pravidelně se dotazuje služby IoT Hub hello IoT hub pro nové zprávy příkaz.
1. Když hello IoT Hub modulu přijme zprávu o nový příkaz, tato možnost publikuje ho toohello zprostředkovatele.
1. modul mapování identit Hello převezme zprávou příkazu hello a používá k interní tabulce tootranslate hello IoT Hub zařízení ID tooa zařízení adresa MAC. Pak publikuje novou zprávu, která obsahuje adresu MAC hello hello cílové zařízení v mapě vlastnosti hello hello zprávy.
1. modul zakázat Cloud-zařízení Hello převezme tuto zprávu a překládá do hello správné zakázat instrukce pro modul zakázat hello. Pak publikuje novou zprávu.
1. modul zakázat Hello převezme tuto zprávu a provede hello vstupně-výstupních operací instrukci navázat komunikaci s hello zakázat zařízení.
1. Hello protokoly modulu zaznamená všechny zprávy ze souboru disku tooa hello zprostředkovatele.

## <a name="prerequisites"></a>Požadavky

toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.

> [!NOTE]
> Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].

Musíte klient SSH na vaše tooenable stolní počítače můžete tooremotely hello se příkaz access řádek na hello malin pí.

- Windows nezahrnuje klientem SSH. Doporučujeme používat [PuTTY](http://www.putty.org/).
- Většina Linuxových distribucích a Mac OS, obsahují hello nástroj příkazového řádku SSH. Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

## <a name="prepare-your-hardware"></a>Připravte svůj hardware

Tento kurz předpokládá, že používáte [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) zařízení připojené tooa malin pí 3 systémem Raspbian.

### <a name="install-raspbian"></a>Nainstalujte Raspbian

Můžete použít buď hello následující možnosti tooinstall Raspbian na vašem zařízení malin pí 3.

* tooinstall hello nejnovější verzi Raspbian, použijte hello [NOOBS] [ lnk-noobs] grafické uživatelské rozhraní.
* Ručně [Stáhnout] [ lnk-raspbian] a zapsat nejnovější bitovou kopii hello hello Raspbian operačního systému tooan SD karty.

### <a name="sign-in-and-access-hello-terminal"></a>Přihlaste se a přístup k Terminálové hello

Dvě možnosti tooaccess máte na vaše platformy malin terminálu prostředí:

* Pokud máte klávesnici a sledování připojených tooyour malin platformy, můžete použít grafické uživatelské rozhraní Raspbian tooaccess hello okno terminálu.

* Přístup hello příkazového řádku na vaší malin pí pomocí protokolu SSH ze stolního počítače.

#### <a name="use-a-terminal-window-in-hello-gui"></a>Použijte okno terminálu v hello grafického uživatelského rozhraní

uživatelské jméno jsou Hello výchozí pověření pro Raspbian **pí** a heslo **malin**. Hello hlavním panelu v hello grafického uživatelského rozhraní, můžete spustit hello **Terminálové** nástroj pomocí hello ikonu, která vypadá jako monitorování.

#### <a name="sign-in-with-ssh"></a>Přihlaste se pomocí protokolu SSH

SSH můžete použít pro přístup přes příkazový řádek tooyour malin pí. článek Hello [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje, jak tooconfigure SSH na vaše malin platformy a jak tooconnect z [Windows] [ lnk-ssh-windows] nebo [Operačního systému Linux & Mac][lnk-ssh-linux].

Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.

### <a name="install-bluez-537"></a>Nainstalujte BlueZ 5.37

moduly zakázat Hello komunikovat hardwaru Bluetooth toohello prostřednictvím hello BlueZ zásobníku. Potřebujete 5.37 verzi BlueZ pro hello moduly toowork správně. Tyto pokyny Ujistěte se, že je nainstalována správná verze BlueZ hello.

1. Zastavte hello aktuální démon bluetooth:

    ```sh
    sudo systemctl stop bluetooth
    ```

1. Nainstalujte hello BlueZ závislosti:

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. Stáhněte hello BlueZ zdrojového kódu z bluez.org:

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. Rozbalte hello zdrojového kódu:

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. Změna složky pro nově vytvořený toohello adresáře:

    ```sh
    cd bluez-5.37
    ```

1. Nakonfigurujte hello BlueZ kód toobe vytvořené:

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. Sestavení BlueZ:

    ```sh
    make
    ```

1. Až se to dokončí instalaci BlueZ stavbě:

    ```sh
    sudo make install
    ```

1. Změna konfigurace služby systemd pro bluetooth tak odkazuje toohello nové démon bluetooth v souboru hello `/lib/systemd/system/bluetooth.service`. Nahraďte řádku 'ExecStart' hello hello následující text:

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-toohello-sensortag-device-from-your-raspberry-pi-3-device"></a>Povolit zařízení SensorTag toohello připojení ze zařízení malin pí 3

Před spuštěné ukázkový text hello musíte tooverify, že vaše malin pí 3 se může připojit zařízení SensorTag toohello.

1. Ujistěte se, hello `rfkill` je nainstalován nástroj:

    ```sh
    sudo apt-get install rfkill
    ```

1. Odblokování bluetooth na hello malin pí 3 a zkontrolujte, zda text hello číslo verze **5.37**:

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. spuštění služby bluetooth hello tooenter hello interaktivní bluetooth prostředí a spusťte hello **bluetoothctl** příkaz:

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Zadejte příkaz hello **zapnutí** toopower až hello bluetooth řadiče. příkaz Hello vrátí podobné toohello následující výstup:

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. V prostředí hello interaktivní bluetooth, zadejte příkaz hello **kontrolu** tooscan pro zařízeními bluetooth. příkaz Hello vrátí podobné toohello následující výstup:

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. Zařízení SensorTag hello zjistitelnost stisknutím tlačítka na malé hello (hello, které by měl zelená DIODU flash). Hello malin pí 3 by měl zjistit zařízení SensorTag hello:

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    V tomto příkladu uvidíte, že hello adresu MAC hello zařízení SensorTag **A0:E6:F8:B5:F6:00**.

1. Vypnout kontrolu zadáním hello **kontrolovat vypnout** příkaz:

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. Připojení zařízení SensorTag tooyour pomocí adresu MAC zadáním **připojit \<adresa MAC\>**. Následující ukázkový výstup Hello je zkratka pro přehlednost:

    ```sh
    Attempting tooconnect tooA0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > Můžete vytvořit seznam vlastností GATT hello hello zařízení znovu s použitím hello **atributy seznamu** příkaz.

1. Teď můžete odpojit od hello zařízení pomocí hello **odpojit** příkaz a poté ukončete z prostředí bluetooth hello pomocí hello **ukončení** příkaz:

    ```sh
    Attempting toodisconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Nyní jste připravené toorun hello lit IoT Edge ukázka na vaše malin pí 3.

## <a name="run-hello-iot-edge-ble-sample"></a>Spuštění ukázkové IoT Edge zakázat hello

Ukázka IoT Edge zakázat hello toorun, je třeba toocomplete tři úlohy:

* Nakonfigurujte dva ukázkové zařízení ve službě IoT Hub.
* Sestavení IoT Edge ve vašem zařízení malin pí 3.
* Nakonfigurujte a spusťte ukázkové zakázat hello na vašem zařízení malin pí 3.

V době psaní textu hello podporuje IoT Edge moduly zakázat pouze v brány se systémem Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfigurovat dva ukázkové zařízení ve službě IoT Hub

* [Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure, potřebujete hello název vašeho centra toocomplete tento návod. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* Přidat jednu zařízení s názvem **SensorTag_01** tooyour IoT hub a poznamenejte si jeho id a zařízení klíče. Můžete použít hello [Průzkumníka zařízení nebo iothub-explorer] [ lnk-explorer-tools] nástroje tooadd toto centrum IoT zařízení toohello jste vytvořili v předchozím kroku hello a tooretrieve svůj klíč. Toto zařízení SensorTag toohello zařízení mapy, při konfiguraci brány hello.

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a>Vytvoření Azure IoT Edge ve vaší Malinová pí 3

Nainstalujte závislosti pro hraniční Azure IoT:

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

Použití hello následující příkazy tooclone IoT okraj a všechny jeho submodules tooyour domovského adresáře:

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

Až budete mít úplnou kopii hello IoT Edge úložiště na vaše malin pí 3, můžete vytvořit pomocí hello následující příkaz z hello složku, která obsahuje hello SDK:

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-hello-ble-sample-on-your-raspberry-pi-3"></a>Nakonfigurujte a spusťte ukázkové zakázat hello na vaše malin pí 3

toobootstrap a spuštění hello ukázková, musíte nakonfigurovat každý IoT Edge modul, který se účastní v bráně hello. Tato konfigurace je součástí souboru JSON a je nutné nakonfigurovat pět zúčastněných moduly IoT okraj. Není k dispozici ukázkový soubor JSON v úložišti hello názvem **brány\_sample.json** , můžete použít jako výchozí bod pro vytváření konfiguračního souboru hello. Tento soubor je v hello **ukázky/ble_gateway/src** složky v místní kopii hello IoT Edge úložiště.

Hello následující části popisují, jak je tooedit tato konfigurace soubor pro ukázku zakázat hello a předpokládá, že hello IoT Edge úložiště v hello **/home/pi/iot-edge /** složky na vaše malin pí 3. Pokud je úložiště hello jinde, odpovídajícím způsobem upravte hello cesty.

#### <a name="logger-configuration"></a>Konfigurace protokolovacího nástroje

Za předpokladu, že úložiště brány hello se nachází v hello **/home/pi/iot-edge /** složku, nakonfigurujte hello protokoly modulu takto:

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a>Konfigurace modulu zakázat

Hello Ukázková konfigurace pro zařízení zakázat hello předpokládá zařízením Texas Instruments SensorTag. Jakékoli standardní zakázat zařízení, které může fungovat jako by měla fungovat GATT periferní však můžete potřebovat tooupdate hello GATT vlastnosti ID a data. Přidáte adresu MAC hello zařízení SensorTag:

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

Pokud nepoužíváte zařízení SensorTag, najdete v dokumentaci hello pro vaše zařízení toodetermine zakázat jestli potřebujete tooupdate hello GATT vlastnosti ID a datové hodnoty.

#### <a name="iot-hub-module"></a>Modul služby IoT Hub

Přidáte hello název služby IoT Hub. Hodnota přípony Hello je obvykle **azure devices.net**:

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Konfigurace modulu mapování identit

Přidat adresu MAC hello zařízení SensorTag a hello ID zařízení a klíč hello **SensorTag_01** zařízení, které jste přidali tooyour IoT Hub:

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Konfigurace modulu zakázat tiskárny

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a>Konfigurace modulu BLEC2D

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a>Konfigurace směrování

Hello následující konfigurace zajišťuje hello následující směrování mezi moduly hraniční IoT:

* Hello **Protokolovač** modul přijímá a zaznamenává všechny zprávy.
* Hello **SensorTag** modul odesílá zprávy tooboth hello **mapování** a **zakázat tiskárny** moduly.
* Hello **mapování** modul odesílá zprávy toohello **IoTHub** toobe modulu správně tooyour IoT Hub.
* Hello **IoTHub** modul odesílá zprávy zpět toohello **mapování** modulu.
* Hello **mapování** modul odesílá zprávy toohello **BLEC2D** modulu.
* Hello **BLEC2D** modul odesílá zprávy zpět toohello **Sensortag** modulu.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

Ukázka hello toorun, pass hello cesta toohello JSON konfiguračním souboru jako parametr toohello **lit\_brány** binární. Hello následující příkaz předpokládá, že používáte hello **gateway_sample.json** konfigurační soubor. Provedení tohoto příkazu z hello **iot hranou** složky na hello malin platformy:

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

Může být nutné toopress hello malé tlačítko toomake zařízení SensorTag hello je zjistitelný před spuštěním ukázka hello.

Když spustíte ukázkový text hello, můžete použít hello [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nebo hello [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroj toomonitor hello zprávy hello IoT hraniční brána předává ze zařízení SensorTag hello. Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí hello následující příkaz:

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a>Odesílání zpráv z cloudu do zařízení

Hello zakázat modul rovněž podporuje odesílání příkazů z toohello zařízení IoT Hub. Můžete použít hello [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nebo hello [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroj toosend JSON zprávy Tenhle modul hello zakázat brány předá toohello zakázat zařízení.

Pokud používáte hello Texas Instruments SensorTag zařízení, můžete zapnout hello red DIODU, zelená DIODU nebo bzučák odesláním příkazů ze služby IoT Hub. Před odesláním příkazy ze služby IoT Hub, nejprve odešlete hello následující dvě zprávy JSON v pořadí. Potom můžete odeslat všechny příkazy tooturn hello na hello indikátory nebo bzučák.

1. Resetovat všechny LED a bzučák hello (jejich vypnutím):

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. Nakonfigurujte vstupně-výstupních operací jako 'vzdálené':

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

Nyní můžete odeslat žádný z následujících příkazů tooturn na hello indikátory nebo bzučák na zařízení SensorTag hello hello:

* Zapněte hello red DIODU:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* Zapněte hello zelená DIODU:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* Zapněte bzučák hello:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Další kroky

Pokud chcete toogain rozsáhlejšími znalostmi IoT okraj a experimentovat s příklady kódu, navštivte hello následující kurzy developer a prostředky:

* [Azure IoT Edge][lnk-sdk]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
