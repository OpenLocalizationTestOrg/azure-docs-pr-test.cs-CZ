---
title: 'Connect Intel Edison (C) tooAzure IoT - Lekce 4: Blink hello DIODU | Microsoft Docs'
description: "Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ovládací prvek vedla s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 9826c55a-0e24-4296-ae54-29b7fe66436a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c51acb42aa297ca91cfe76d7b0361ad95e2fb2e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Změnit hello zapnout a vypnout chování hello DIODU
## <a name="what-you-will-do"></a>Co provedete
Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
Použijte další funkce toochange hello DIODU je zapnout a vypnout chování.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili [spusťte ukázkovou aplikaci v cloudu tooreceive Intel Edison toodevice zprávy][receive-cloud-to-device-messages].

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
     mraa_gpio_write(context, 1);
   }

   static void turnOffLED()
   {
     mraa_gpio_write(context, 0);
   }
   ```

   ![soubor Main.c s přidané funkce](media/iot-hub-intel-edison-lessons/lesson4/updated_app_c.png)

3. Přidejte následující podmínky před hello hello `else if` blok hello `receiveMessageCallback` funkce:

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

   ![Soubor Gulpfile.js s přidané – funkce][gulpfile]
5. V hello `sendMessage` fungovat, nahraďte hello řádku `var message = buildMessage(sentMessageCount);` s hello nový řádek ukazuje hello následující fragment kódu:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Uložte všechny změny hello.

### <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na Edison spuštěním hello následující příkaz:

```bash
gulp deploy && gulp run
```

Měli byste vidět hello DIODU zapněte na 2 sekundy a poté vypněte pro jiné dvou sekund. poslední zprávu "stop" Hello zastaví hello spuštění ukázkové aplikace.

![zapnout a vypnout][on-and-off]

Blahopřejeme! Úspěšně jste přizpůsobili hello zpráv, které jsou odeslány tooEdison ze služby IoT hub.

### <a name="summary"></a>Souhrn
V této volitelné části ukážeme, jak toocustomize zpráv tak, že hello ukázkovou aplikaci může řídit hello zapnout a vypnout chování hello DIODU jiným způsobem.

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_c.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_c.png
