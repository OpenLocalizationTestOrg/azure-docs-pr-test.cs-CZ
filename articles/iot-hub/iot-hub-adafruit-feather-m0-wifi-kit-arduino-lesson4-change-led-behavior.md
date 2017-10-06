---
title: "Connect Arduino (C) tooAzure IoT - Lekce 4: úpravě aplikace | Microsoft Docs"
description: "Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování."
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
ms.openlocfilehash: 8cc438650f01ae4335d91c94df6a29e0ffbdc508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Změnit hello zapnout a vypnout chování hello DIODU
## <a name="what-you-will-do"></a>Co provedete
Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) pro panel Adafruit prolnutí M0 Wi-Fi Arduino.

## <a name="what-you-will-learn"></a>Co se dozvíte
Použijte další Arduino funkce toochange hello DIODU je zapnout a vypnout chování.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili [spusťte ukázkovou aplikaci v cloudu Arduino Tabule tooreceive toodevice zprávy][receive-cloud-to-device-messages].

## <a name="add-functions-toomainc-and-gulpfilejs"></a>Přidání funkce toomain.c a gulpfile.js
1. Otevřete hello ukázkovou aplikaci v sadě Visual Studio kódu spuštěním hello následující příkazy:

   ```bash
   cd Lesson4
   code .
   ```
2. Otevřete hello `app.ino` souboru a poté přidejte hello po blinkLED() funkce následující funkce:

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
3. Přidejte následující podmínky před hello hello `else if` blok hello `receiveMessageCallback` funkce:

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

   Nyní jste nakonfigurovali hello ukázkové aplikace toorespond toomore pokyny prostřednictvím zprávy. Hello "na" instrukce zapne hello DIODU a hello "off" instrukce vypne hello Indikátor.
4. Otevřete soubor gulpfile.js hello a pak přidejte novou funkci před hello funkce `sendMessage`:

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
5. V hello `sendMessage` fungovat, nahraďte hello řádku `var message = buildMessage(sentMessageCount);` s hello nový řádek ukazuje hello následující fragment kódu:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Uložte všechny změny hello.

### <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na panel Arduino spuštěním hello následující příkaz:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Měli byste vidět hello DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund. poslední zprávu "stop" Hello zastaví hello spuštění ukázkové aplikace.

![zapnout a vypnout][on-and-off]

Blahopřejeme! Úspěšně jste přizpůsobili hello zpráv, které jsou odeslány tooyour Arduino Tabule ze služby IoT hub.

### <a name="summary"></a>Souhrn
V této volitelné části ukážeme, jak toocustomize zpráv tak, že hello ukázkovou aplikaci může řídit hello zapnout a vypnout chování hello DIODU jiným způsobem.

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png