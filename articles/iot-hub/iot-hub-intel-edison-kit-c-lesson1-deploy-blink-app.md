---
title: "Connect Intel Edison (C) k Azure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování C ukázkovou aplikaci z webu GitHub a spusťte gulp k nasazení této aplikace na panel Intel Edison. Tato ukázková aplikace bliká DIODU připojené k panelu každé dvě sekundy."
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
ms.openlocfilehash: c45ff5f41bdbc78da8532ffdcaaeec15c695f531
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>Vytvoření a nasazení aplikace blink
## <a name="what-you-will-do"></a>Co provedete
Klonování C ukázkovou aplikaci z webu GitHub a pomocí nástroje gulp nasadit ukázkovou aplikaci pro Intel Edison. Ukázkovou aplikaci bliká DIODU připojené k panelu každé dvě sekundy. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
* Postup nasazení a spuštění ukázkové aplikace na Edison.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili následující operace:

* [Konfigurace zařízení][configure-your-device]
* [Získat nástroje][get-the-tools]

## <a name="open-the-sample-application"></a>Otevřete ukázkové aplikace
Chcete-li otevřít ukázkové aplikace, postupujte takto:

1. Naklonujte úložiště ukázkové z Githubu spuštěním následujícího příkazu:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

Soubor v `app` podsložky je klíče zdrojového souboru, který obsahuje kód, který ovládacího prvku Indikátor.

### <a name="install-application-dependencies"></a>Instalace závislostí aplikací
Nainstalujte v knihovnách a dalších modulů, které potřebujete pro ukázkovou aplikaci tak, že spustíte následující příkaz:

```bash
npm install
```

## <a name="configure-the-device-connection"></a>Nakonfigurujte připojení zařízení
Konfigurace připojení zařízení, postupujte takto:

1. Generujte konfiguračního souboru zařízení tak, že spustíte následující příkaz:

   ```bash
   gulp init
   ```

   Konfigurační soubor `config-edison.json` obsahuje uživatelská pověření, které používáte k přihlášení do Edison. Aby se zabránilo úniku přihlašovacích údajů uživatele, je generována konfiguračního souboru v podsložce `.iot-hub-getting-started` domovské složky v počítači.

2. Otevřete konfigurační soubor zařízení v aplikaci Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. Nahraďte zástupný symbol `[device hostname or IP address]` a `[device password]` se IP adresa a heslo, které jste označili v předchozí lekci.

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

Blahopřejeme! Úspěšně jste vytvořili první ukázkovou aplikaci pro Edison.

## <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
### <a name="install-the-azure-iot-hub-sdk-on-edison"></a>Nainstalujte službu Azure IoT Hub SDK Edison
Instalace sady SDK Azure IoT Hub na Edison spuštěním následujícího příkazu:

```bash
gulp install-tools
```

Tato úloha může trvat dlouhou dobu v závislosti na připojení k síti. Musí být spuštěn pouze jednou pro jeden Edison.

### <a name="deploy-and-run-the-sample-app"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace tak, že spustíte následující příkaz:

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a>Ověřte aplikace funguje
Ukázkovou aplikaci se po bliká Indikátor pro 20krát automaticky ukončí. Pokud nevidíte DIODU blikat, najdete v článku [Průvodce odstraňováním potíží s] [ troubleshooting] pro řešení běžných potíží.

![Indikátor blikat][led-blinking]

## <a name="summary"></a>Souhrn
Nainstalujete požadované nástroje pro práci s Edison a nasadit ukázkovou aplikaci pro Edison blikání Indikátor. Teď můžete vytvořit, nasadit a spustit jinou ukázkovou aplikaci, která Edison se připojuje ke službě Azure IoT Hub odesílat a přijímat zprávy.

## <a name="next-steps"></a>Další kroky
[Získat nástroje Azure][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
