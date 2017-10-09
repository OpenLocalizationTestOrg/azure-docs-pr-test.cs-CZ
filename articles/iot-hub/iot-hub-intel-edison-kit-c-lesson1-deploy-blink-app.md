---
title: "Connect Intel Edison (C) tooAzure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování hello ukázkové C aplikace z webu GitHub a spusťte gulp toodeploy této aplikace tooyour Intel Edison panelu. Tato ukázková aplikace bliká hello DIODU připojené toohello Tabule každé dvě sekundy."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vedla projekty, arduino vedla blikání, arduino vedla blikání kód, program blikání arduino, příklad arduino blikání"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: b02dfd3f-28fd-4b52-8775-eb0eaf74d707
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: fa84fae812dd742a2ad4997a5e213c8e40e6fcf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Vytvoření a nasazení aplikace hello blikání
## <a name="what-you-will-do"></a>Co provedete
Klonování hello ukázkové C aplikace z webu GitHub a používat hello gulp nástroj toodeploy hello ukázkové aplikace tooIntel Edison. Ukázková aplikace Hello bliká hello DIODU připojené toohello Tabule každé dvě sekundy. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
* Jak toodeploy a spuštění hello ukázkové aplikace na Edison.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili hello následující operace:

* [Konfigurace zařízení][configure-your-device]
* [Získat nástroje hello][get-the-tools]

## <a name="open-hello-sample-application"></a>Ukázkové aplikace otevřete hello
tooopen hello ukázkové aplikace, postupujte takto:

1. Klonování úložiště ukázkové hello z Githubu spuštěním hello následující příkaz:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

soubor Hello v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toocontrol hello Indikátor.

### <a name="install-application-dependencies"></a>Instalace závislostí aplikací
Nainstalujte hello knihovny a z ostatních modulů, které potřebujete pro ukázkovou aplikaci hello spuštěním hello následující příkaz:

```bash
npm install
```

## <a name="configure-hello-device-connection"></a>Nakonfigurujte připojení zařízení hello
tooconfigure hello připojení zařízení, postupujte takto:

1. Generovat soubor konfigurace zařízení hello spuštěním hello následující příkaz:

   ```bash
   gulp init
   ```

   konfigurační soubor Hello `config-edison.json` obsahuje uživatelská pověření hello použijete toolog v tooEdison. tooavoid hello úniku přihlašovacích údajů uživatele, je generována hello konfigurační soubor v podsložce hello `.iot-hub-getting-started` hello domovské složky v počítači.

2. Otevřete soubor konfigurace zařízení hello v Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. Nahraďte zástupný symbol hello `[device hostname or IP address]` a `[device password]` s hello IP adresu a heslo, které jste označili v předchozí lekci.

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

Blahopřejeme! Úspěšně jste vytvořili hello první ukázkovou aplikaci pro Edison.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
### <a name="install-hello-azure-iot-hub-sdk-on-edison"></a>Instalace sady SDK Azure IoT Hub hello na Edison
Instalace sady SDK Azure IoT Hub hello na Edison spuštěním hello následující příkaz:

```bash
gulp install-tools
```

Tato úloha může trvat dlouhou dobu toocomplete, v závislosti na připojení k síti. Ho potřebovat toobe spustit jenom jednou pro jeden Edison.

### <a name="deploy-and-run-hello-sample-app"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello spuštěním hello následující příkaz:

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a>Ověřte hello aplikace funguje
Hello ukázkové aplikaci se po hello DIODU bliká pro 20krát automaticky ukončí. Pokud nevidíte hello DIODU blikat, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.

![Indikátor blikat][led-blinking]

## <a name="summary"></a>Souhrn
Nainstalujete hello požadované nástroje toowork s Edison a nasazení ukázkové aplikace tooEdison tooblink hello Indikátor. Teď můžete vytvořit, nasadit a spouštět jiný ukázkovou aplikaci, která se připojuje Edison tooAzure toosend IoT Hub a příjem zpráv.

## <a name="next-steps"></a>Další kroky
[Získat hello Azure nástroje][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
