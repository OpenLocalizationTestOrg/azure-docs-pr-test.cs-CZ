---
title: "Převod aaaData na IoT brány s hranou Azure IoT | Microsoft Docs"
description: "Použijte IoT brány tooconvert hello formát data snímačů pomocí vlastní modul z Azure IoT okraj."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Převod dat IOT brány, transformaci dat brány iot"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: ae94b1f96f36dfcb4f77fadc0ece3cff3d0bba91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a>Použít bránu IoT pro transformaci dat snímačů s hranou Azure IoT

> [!NOTE]
> Než začnete tento kurz, ujistěte se, že jste dokončené hello následující lekce v pořadí:
> * [Nastavení Intel NUC jako brány IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [Použít IoT brány tooconnect věcí toohello cloudu - SensorTag tooAzure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

Jediný účel bránu Iot tooprocess shromažďovaných dat je před odesláním toohello cloudu. Azure IoT Edge zavádí moduly, které můžou být vytvořené a sestavený tooform hello zpracování dat pracovního postupu. Modul přijme nějakou zprávu, provádí některé akce a potom posuňte pro jiné moduly tooprocess.

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak toocreate modulu tooconvert zpráv ze hello SensorTag do jiného formátu.

## <a name="what-you-do"></a>Co dělat

* Vytvoření modulu tooconvert přijaté zprávy do formátu .json hello.
* Zkompilujte hello modulu.
* Přidáte hello modulu toohello zakázat ukázkovou aplikaci z Azure IoT okraj.
* Spuštění ukázkové aplikace hello.

## <a name="what-you-need"></a>Co potřebujete

* Hello následující kurzy dokončit v pořadí:
  * [Nastavení Intel NUC jako brány IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [Použít IoT brány tooconnect věcí toohello cloudu - SensorTag tooAzure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* Klient SSH, který běží v hostitelském počítači. V systému Windows se doporučuje puTTY. Linux a systému macOS již obsahují klientem SSH.
* Hello IP adresa a hello uživatelské jméno a heslo tooaccess hello brány z klienta SSH hello.
* Připojení k Internetu.

## <a name="create-a-module"></a>Vytvoření modulu

1. Na hostitelském počítači hello spusťte hello klient SSH a připojit toohello IoT bránu.
1. Klonování hello zdrojové soubory modulu převod hello z Githubu toohello domovský adresář hello IoT brány spuštěním hello následující příkazy:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   Toto je nativní modul Azure Edge napsané v hello programovacího jazyka C. modul Hello převede hello formát přijaté zprávy hello jeden následující:

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-hello-module"></a>Kompilace hello modulu

modul hello toocompile spustit hello následující příkazy:

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change hello build script runnable
chmod 777 build.sh
# remove hello invalid windows character
sed -i -e "s/\r$//" build.sh
# run hello build shell script
./build.sh
```

Můžete získat `libmy_module.so` souboru po dokončení hello kompilace. Poznamenejte si hello absolutní cesta tohoto souboru.

## <a name="add-hello-module-toohello-ble-sample-application"></a>Přidání hello modulu toohello zakázat ukázkové aplikace

1. Ukázky složky přejděte toohello spuštěním hello následující příkaz:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. Otevřete konfigurační soubor hello spuštěním hello následující příkaz:

   ```bash
   vi ble_gateway.json
   ```

1. Přidat modul vložením hello následující kód toohello `modules` části.

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. Nahraďte `[Your libmy_module.so path]` v kódu hello s hello absolutní cesta hello libmy_module.so' souboru.
1. Nahraďte kód hello v hello `links` oddíl s hello jeden následující:

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. Stiskněte klávesu `ESC`a pak zadejte `:wq` toosave hello souboru.

## <a name="run-hello-sample-application"></a>Spuštění ukázkové aplikace hello

1. Zapnutí hello SensorTag.
1. Nastavit proměnnou prostředí SSL_CERT_FILE hello spuštěním hello následující příkaz:

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. Spusťte hello ukázkovou aplikaci s přidaným modulem hello spuštěním hello následující příkaz:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Další kroky

Úspěšně jste použití hello IoT brány tooconvert hello zprávu od SensorTag do formátu .json hello.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
