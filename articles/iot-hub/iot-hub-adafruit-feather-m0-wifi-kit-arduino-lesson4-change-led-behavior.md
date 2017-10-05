---
title: "Připojení k Azure IoT - Lekce 4 Arduino (C): úpravě aplikace | Microsoft Docs"
description: "Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ovládací prvek vedla s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: d7a25430-450e-43c4-a3ed-1eed995b8b7e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5009a0466f2c5689b8ab426049f4c4f02272512b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>Změnit zapnuto a vypnuto chování Indikátor
## <a name="what-you-will-do"></a>Co provedete
Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) pro panel Adafruit prolnutí M0 Wi-Fi Arduino.

## <a name="what-you-will-learn"></a>Co se dozvíte
Na další funkce Arduino můžete změnit LED zapnout a vypnout chování.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili [spuštění ukázkové aplikace na vaší kartě Arduino přijímat cloudu na zařízení zprávy][receive-cloud-to-device-messages].

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>Přidání funkce do main.c a gulpfile.js
1. Otevřete ukázkovou aplikaci v sadě Visual Studio kódu spuštěním následujících příkazů:

   ```bash
   cd Lesson4
   code .
   ```
2. Otevřete `app.ino` souboru a poté přidejte následující funkce po blinkLED() funkce:

   ```arduino
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![soubor App.ino s přidané funkce][app-ino-file]
3. Přidejte následující podmínky před `else if` blokování z `receiveMessageCallback` funkce:

   ```arduino
   else if (strcmp((const char*)value, "\"on\"") == 0)
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\"") == 0)
   {
       turnOffLED();
   }
   ```

   Nyní jste nakonfigurovali ukázkovou aplikaci reagovat na další pokyny prostřednictvím zprávy. Instrukce "na" zapne Indikátor a instrukce "off" vypne Indikátor.
4. Otevřete soubor gulpfile.js a pak přidejte novou funkci před funkce `sendMessage`:

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   };
   ```

   ![Soubor Gulpfile.js s přidané – funkce][gulp-file-js]
5. V `sendMessage` fungovat, nahraďte řádku `var message = buildMessage(sentMessageCount);` s novým řádkem uvedené v následující fragment kódu:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Změny uložte.

### <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace na panel Arduino spuštěním následujícího příkazu:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Měli byste vidět DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund. Poslední zprávu "stop" zastaví spuštění ukázkové aplikace.

![zapnout a vypnout][on-and-off]

Blahopřejeme! Úspěšně jste přizpůsobili zprávy, které se odesílají do vaší karty Arduino ze služby IoT hub.

### <a name="summary"></a>Souhrn
Této volitelné části ukazuje, jak přizpůsobit zpráv tak, aby ukázkovou aplikaci může řídit zapnuto a vypnuto chování Indikátor jiným způsobem.

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png