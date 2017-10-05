---
title: "Připojení k Azure IoT - Lekce 4 Malinová Pi (C): úpravě aplikace | Microsoft Docs"
description: "Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ovládací prvek vedla pomocí Malinová pi, malinová platformy vyvolala ovládací prvek, malinová pí řízení vedla"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 0201b8ed-d5e6-4445-9a4d-1305003d1eff
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b1e441b20e161f4a03d4c2c300b21aca4fedb2a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>Změnit zapnuto a vypnuto chování Indikátor
## <a name="what-you-will-do"></a>Co provedete
Přizpůsobení zprávy a pokuste se změnit LED zapnout a vypnout chování. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
Na další funkce Node.js můžete změnit LED zapnout a vypnout chování.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili [spuštění ukázkové aplikace na malin platformy pro příjem cloudu na zařízení zprávy](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>Přidání funkce do main.c a gulpfile.js
1. Otevřete ukázkovou aplikaci v sadě Visual Studio kódu spuštěním následujících příkazů:

   ```bash
   cd Lesson4
   code .
   ```
2. Otevřete `main.c` souboru a poté přidejte následující funkce po blinkLED() funkce:

   ```c
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![soubor Main.c s přidané funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_c.png)
3. Přidejte následující podmínky před výchozí nastavení v `if` blokování z `receiveMessageCallback` funkce:

   ```c
   else if (0 == strcmp((const char*)value, "\"on\""))
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\""))
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
   }
   ```

   ![Soubor Gulpfile.js s přidané – funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile_c.png)
5. V `sendMessage` fungovat, nahraďte řádku `var message = buildMessage(sentMessageCount);` s novým řádkem uvedené v následující fragment kódu:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Změny uložte.

### <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace na platformy spuštěním následujícího příkazu:

```bash
gulp deploy && gulp run
```

Měli byste vidět DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund. Poslední zprávu "stop" zastaví spuštění ukázkové aplikace.

![Ukázkovou aplikaci s zapnout a vypnout zprávy](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off_c.png)

Blahopřejeme! Úspěšně jste přizpůsobili zprávy, které se odesílají do pí ze služby IoT hub.

### <a name="summary"></a>Souhrn
Této volitelné části ukazuje, jak přizpůsobit zpráv tak, aby ukázkovou aplikaci může řídit zapnuto a vypnuto chování Indikátor jiným způsobem.
