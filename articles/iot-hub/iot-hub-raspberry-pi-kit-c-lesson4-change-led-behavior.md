---
title: "Connect Raspberry PI (C) tooAzure IoT - Lekce 4: úpravě aplikace | Microsoft Docs"
description: "Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování."
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
ms.openlocfilehash: f4739c4e9a58b4b0fe964b5c3c81e5918982099f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Změnit hello zapnout a vypnout chování hello DIODU
## <a name="what-you-will-do"></a>Co provedete
Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
Použijte další Node.js funkce toochange hello DIODU je zapnout a vypnout chování.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili [spusťte ukázkovou aplikaci v cloudu tooreceive malin pí toodevice zprávy](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).

## <a name="add-functions-toomainc-and-gulpfilejs"></a>Přidání funkce toomain.c a gulpfile.js
1. Otevřete hello ukázkovou aplikaci v sadě Visual Studio kódu spuštěním hello následující příkazy:

   ```bash
   cd Lesson4
   code .
   ```
2. Otevřete hello `main.c` souboru a poté přidejte hello po blinkLED() funkce následující funkce:

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
3. Přidejte následující podmínky před hello výchozí nastavení v hello hello `if` blok hello `receiveMessageCallback` funkce:

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
   }
   ```

   ![Soubor Gulpfile.js s přidané – funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile_c.png)
5. V hello `sendMessage` fungovat, nahraďte hello řádku `var message = buildMessage(sentMessageCount);` s hello nový řádek ukazuje hello následující fragment kódu:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Uložte všechny změny hello.

### <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na platformy spuštěním hello následující příkaz:

```bash
gulp deploy && gulp run
```

Měli byste vidět hello DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund. poslední zprávu "stop" Hello zastaví hello spuštění ukázkové aplikace.

![Ukázkovou aplikaci s zapnout a vypnout zprávy](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off_c.png)

Blahopřejeme! Úspěšně jste přizpůsobili hello zpráv, které jsou odeslány tooPi ze služby IoT hub.

### <a name="summary"></a>Souhrn
V této volitelné části ukážeme, jak toocustomize zpráv tak, že hello ukázkovou aplikaci může řídit hello zapnout a vypnout chování hello DIODU jiným způsobem.
