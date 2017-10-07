---
title: "Připojit malin platformy (uzel) tooAzure IoT - Lekce 4: úpravě aplikace | Microsoft Docs"
description: "Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ovládací prvek vedla pomocí Malinová pi, malinová platformy vyvolala ovládací prvek, malinová pí řízení vedla"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 3b42a4ad-0197-42f6-8ca9-04c883e879e8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 99b542fcb8639add0f5a0f7a49dd8abd0e224a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-on-and-off-behavior-of-hello-led"></a>Změnit hello zapnout a vypnout chování hello DIODU
## <a name="what-you-will-do"></a>Co provedete
Přizpůsobte hello zprávy toochange hello DIODU je zapnout a vypnout chování. Pokud máte potíže, hledají řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
Použijte další Node.js funkce toochange hello DIODU je zapnout a vypnout chování.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili [spuštění ukázkové aplikace na malin pí tooreceive zprávy typu cloud zařízení](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="add-nodejs-functions"></a>Přidání funkce Node.js
1. Otevřete hello ukázkovou aplikaci v sadě Visual Studio kódu spuštěním hello následující příkazy:
   
   ```bash
   cd Lesson4
   code .
   ```
2. Otevřete hello `app.js` souboru a poté přidejte následující funkce na konci hello hello:
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![Souboru app.js s přidané funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. Přidejte následující podmínky před hello výchozí nastavení v bloku případy switch hello hello hello `receiveMessageCallback` funkce:
   
   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
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
   
   ![Soubor Gulpfile.js s přidané – funkce](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)
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

![Ukázkovou aplikaci s zapnout a vypnout zprávy](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Blahopřejeme! Úspěšně jste přizpůsobili hello zpráv, které jsou odeslány tooPi ze služby IoT hub.

### <a name="summary"></a>Souhrn
V této volitelné části ukážeme, jak toocustomize zpráv tak, že hello ukázkovou aplikaci může řídit hello zapnout a vypnout chování hello DIODU jiným způsobem.

