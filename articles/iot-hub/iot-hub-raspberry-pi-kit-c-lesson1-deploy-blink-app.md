---
title: "Malinová Pi (C) připojit k Azure IoT - Lekce 1: nasazení aplikace | Microsoft Docs"
description: "Klonování C ukázkovou aplikaci z webu GitHub a gulp k nasazení této aplikace na panel malin pí 3. Tato ukázková aplikace bliká DIODU připojené k panelu každé dvě sekundy."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Malinová platformy vyvolala blikání, blikání vedla s Malinová platformy"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f601d1e1-2bc3-4cc5-a6b1-0467e5304dcf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 2ae409c6a39521711777ec329d2507a2801cc985
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>Vytvoření a nasazení aplikace blink
## <a name="what-you-will-do"></a>Co provedete
Klonování C ukázkovou aplikaci z webu GitHub a pomocí nástroje gulp nasadit ukázkovou aplikaci pro malin pí 3. Ukázkovou aplikaci bliká DIODU připojené k panelu každé dvě sekundy. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Postup použití `device-discover-cli` nástroj k načtení sítě informace o pí.
* Postup nasazení a spuštění ukázkové aplikace na pí.
* Postup nasazení a ladění aplikací běžících na platformy vzdáleně.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili následující operace:

* [Konfigurace zařízení](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [Získat nástroje](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a>Získat IP adresu a hostitele název platformy
Otevřete příkazový řádek v systému Windows nebo terminál v systému macOS nebo Ubuntu a spusťte následující příkaz:

```bash
devdisco list --eth
```

Měli byste vidět výstup podobný tomuto:

![Zjišťování zařízení](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Poznamenejte si `IP address` a `hostname` pí. Je třeba tyto informace dále v tomto článku.

> [!NOTE]
> Ujistěte se, že platformy je připojený ke stejné síti jako počítače. Například pokud je počítač připojen k bezdrátové síti a platformy je připojeno k drátové síti, nemusíte to vidět IP adresu ve výstupu devdisco.

## <a name="open-the-sample-application"></a>Otevřete ukázkové aplikace
Chcete-li otevřít ukázkové aplikace, postupujte takto:

1. Naklonujte úložiště ukázkové z Githubu spuštěním následujícího příkazu:
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

`main.c` v soubor `app` podsložky je klíče zdrojového souboru, který obsahuje kód, který ovládacího prvku Indikátor.

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
   
   Konfigurační soubor `config-raspberrypi.json` obsahuje uživatelská pověření, které používáte k přihlášení do pí. Aby se zabránilo úniku přihlašovacích údajů uživatele, je generována konfiguračního souboru v podsložce `.iot-hub-getting-started` domovské složky v počítači.

2. Otevřete konfigurační soubor zařízení v aplikaci Visual Studio Code spuštěním následujícího příkazu:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. Nahraďte zástupný symbol `[device hostname or IP address]` se IP adresa nebo název hostitele, který jste získali dříve v "získat IP adresu a hostitele název pí."
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> Při připojování k malin platformy, můžete použít klíč SSH místo uživatelské jméno a heslo. Pokud to chcete provést budete muset vygenerovat klíč pomocí **ssh-keygen** a **ssh kopie id platformy @\<adresa zařízení\>**.
>
> V systému Windows jsou k dispozici v těchto příkazů **Git Bash**.
>
> V systému MacOS, budete muset spustit **brew nainstalovat ssh kopie id**.
>
> Po nahrání úspěšně klíč malin PI, nahraďte **device_password** s **device_key_path** vlastnost **konfigurace raspberrypi.json**.
>
> Aktualizované řádky by měl vypadat jako níže:
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

Blahopřejeme! Úspěšně jste vytvořili první ukázkovou aplikaci pro platformy.

## <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
### <a name="install-the-azure-iot-hub-sdk-on-pi"></a>Nainstalujte službu Azure IoT Hub SDK na platformy
Instalace sady SDK Azure IoT Hub na platformy spuštěním následujícího příkazu:

```bash
gulp install-tools
```

Tato úloha může trvat několik minut na dokončení při prvním spuštění.

### <a name="deploy-and-run-the-sample-app"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace tak, že spustíte následující příkaz:

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a>Ověřte aplikace funguje
Ukázkovou aplikaci se po bliká Indikátor pro 20krát automaticky ukončí. Pokud nevidíte DIODU blikat, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) pro řešení běžných potíží.
![Indikátor blikat](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

## <a name="summary"></a>Souhrn
Nainstalujete požadované nástroje pro práci s platformy a nasadit ukázkovou aplikaci pro platformy blikání Indikátor. Teď můžete vytvořit, nasadit a spustit jinou ukázkovou aplikaci, která připojuje platformy Azure IoT Hub odesílat a přijímat zprávy.

## <a name="next-steps"></a>Další kroky
[Získat nástroje Azure](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

