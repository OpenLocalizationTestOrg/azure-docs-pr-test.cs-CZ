---
title: 'Connect Arduino (C) tooAzure IoT - Lekce 3: Table storage | Microsoft Docs'
description: "Zprávy typu zařízení cloud hello sledovat, jak jsou napsány tooyour Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu hello, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 386083e0-0dbb-48c0-9ac2-4f8fb4590772
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9fef18bc9e780e78d95f0c643a5f193125130a5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Čtení zprávy uchovávané v úložišti Azure
## <a name="what-you-will-do"></a>Co provedete
Zprávy typu zařízení cloud hello monitorování odeslané z centra IoT Adafruit prolnutí M0 Wi-Fi Arduino Tabule tooyour jako hello zprávy jsou zapsány tooyour Azure Table storage.

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte, jak trvalý toouse hello gulp úloha číst zprávy tooread zpráv ve službě Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete
Před zahájením tohoto procesu se musí úspěšně jste dokončili [spustit hello Azure blikání ukázkovou aplikaci na vaší kartě Arduino][run-blink-application].

## <a name="read-new-messages-from-your-storage-account"></a>Přečtěte si nové zprávy z vašeho účtu úložiště
V předchozím článku hello jste spustili ukázkovou aplikaci na vaší kartě Arduino. Ukázková aplikace Hello odeslané zprávy tooyour Azure IoT hub. Hello zprávy odeslané tooyour IoT hub jsou uloženy do úložiště Azure Table prostřednictvím aplikace Azure funkce hello. Je nutné hello úložiště Azure připojovací řetězec tooread zprávy z Azure Table storage.

zprávy tooread uložené ve službě Azure Table storage, postupujte takto:

1. Spustit hello následující příkazy a získáte hello připojovací řetězec:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   První příkaz Hello načte hello `storage name` , který se používá v hello druhý příkaz tooget hello připojovací řetězec. Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.
2. Konfigurační soubor otevřete hello `config-arduino.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```
3. Nahraďte `[Azure storage connection string]` s řetězcem připojení hello jste získali v kroku 1.
4. Uložit hello `config-arduino.json` souboru.
5. Odesílání zpráv znovu a číst je z Azure Table storage spuštěním hello následující příkaz:

   ```bash
   gulp run --read-storage

   # You can monitor hello serial port by running listen task:
   gulp listen

   # Or you can combine above two gulp tasks into one:
   gulp run --read-storage --listen
   ```

   Hello logiku pro čtení z úložiště tabulek Azure je v hello `azure-table.js` souboru.

   ![spuštění – gulp úložiště pro čtení][gulp-run]

## <a name="summary"></a>Souhrn
Úspěšně jste připojené centra IoT tooyour Tabule Arduino hello cloudu a použít hello blikání ukázkové aplikace toosend zařízení cloud zprávy. Můžete také použít hello Azure funkce aplikace toostore příchozí IoT hub zprávy tooyour Azure Table storage. Můžete teď odesílání zpráv typu cloud zařízení z vaší IoT hub tooyour Arduino panelu.

## <a name="next-steps"></a>Další kroky
[Odesílání zpráv typu cloud zařízení][send-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[run-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md
[gulp-run]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_read_message_arduino.png
[send-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md