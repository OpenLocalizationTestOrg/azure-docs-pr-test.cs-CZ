---
title: "Connect Raspberry PI (C) tooAzure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování hello ukázkové C aplikace z webu GitHub a gulp toodeploy Tabule tooyour malin pí 3 této aplikace. Tato ukázková aplikace bliká hello DIODU připojené toohello Tabule každé dvě sekundy."
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
ms.openlocfilehash: e90c3360c4de1873313db19561c781eb21dbf1d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Vytvoření a nasazení aplikace hello blikání
## <a name="what-you-will-do"></a>Co provedete
Klonování hello ukázkovou aplikaci C z Githubu a používat hello gulp nástroj toodeploy hello ukázkové aplikace tooRaspberry pí 3. Ukázková aplikace Hello bliká hello DIODU připojené toohello Tabule každé dvě sekundy. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak toouse hello `device-discover-cli` nástroj tooretrieve sítě informace o pí.
* Jak toodeploy a spuštění hello ukázkové aplikace na pí.
* Jak toodeploy a ladění aplikací spuštěných vzdáleně na pí.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili hello následující operace:

* [Konfigurace zařízení](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [Získat nástroje hello](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-hello-ip-address-and-host-name-of-pi"></a>Získat hello IP adresy a názvu hostitele pí
Otevřete příkazový řádek v systému Windows nebo terminál v systému macOS nebo Ubuntu a spusťte následující příkaz hello:

```bash
devdisco list --eth
```

Měli byste vidět výstupu, který je podobný toohello následující:

![Zjišťování zařízení](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Poznamenejte si hello `IP address` a `hostname` pí. Je třeba tyto informace dále v tomto článku.

> [!NOTE]
> Ujistěte se, že platformy je připojený toohello stejné sítě jako váš počítač. Například pokud je počítač připojený tooa bezdrátové sítě připojené tooa drátové síti při instalaci platformy, nemusíte to vidět hello IP adresu ve výstupu devdisco hello.

## <a name="open-hello-sample-application"></a>Ukázkové aplikace otevřete hello
tooopen hello ukázkové aplikace, postupujte takto:

1. Klonování úložiště ukázkové hello z Githubu spuštěním hello následující příkaz:
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

Hello `main.c` souboru v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toocontrol hello Indikátor.

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
   
   konfigurační soubor Hello `config-raspberrypi.json` obsahuje uživatelská pověření hello použijete toolog v tooPi. tooavoid hello úniku přihlašovacích údajů uživatele, je generována hello konfigurační soubor v podsložce hello `.iot-hub-getting-started` hello domovské složky v počítači.

2. Otevřete soubor konfigurace zařízení hello v Visual Studio Code spuštěním hello následující příkaz:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. Nahraďte zástupný symbol hello `[device hostname or IP address]` s hello IP adresu nebo název hostitele hello, který jste získali dříve "Získat hello IP adresy a názvu hostitele pí."
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> Při připojování tooRaspberry platformy, můžete použít klíč SSH místo uživatelské jméno a heslo. V pořadí toodo, to budete mít klíč pomocí toogenerate hello **ssh-keygen** a **ssh kopie id platformy @\<adresa zařízení\>**.
>
> V systému Windows jsou k dispozici v těchto příkazů **Git Bash**.
>
> V systému MacOS potřebujete toorun **brew nainstalovat ssh kopie id**.
>
> Po nahrání úspěšně hello klíče toohello malin Pi, nahraďte **device_password** s **device_key_path** vlastnost **konfigurace raspberrypi.json**.
>
> Aktualizované řádky by měl vypadat jako níže:
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

Blahopřejeme! Úspěšně jste vytvořili hello první ukázkovou aplikaci pro platformy.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
### <a name="install-hello-azure-iot-hub-sdk-on-pi"></a>Instalace sady SDK Azure IoT Hub hello na platformy
Instalace sady SDK Azure IoT Hub hello na platformy spuštěním hello následující příkaz:

```bash
gulp install-tools
```

Tato úloha může trvat několik minut toocomplete hello při prvním spuštění.

### <a name="deploy-and-run-hello-sample-app"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello spuštěním hello následující příkaz:

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a>Ověřte hello aplikace funguje
Hello ukázkové aplikaci se po hello DIODU bliká pro 20krát automaticky ukončí. Pokud nevidíte hello DIODU blikat, přečtěte si téma hello [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) řešení toocommon problémů.
![Indikátor blikat](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

## <a name="summary"></a>Souhrn
Nainstalujete hello požadované nástroje toowork s platformy a nasazení ukázkové aplikace tooPi tooblink hello Indikátor. Teď můžete vytvořit, nasadit a spusťte jinou ukázkovou aplikaci, která připojí tooAzure platformy IoT Hub toosend a přijímat zprávy.

## <a name="next-steps"></a>Další kroky
[Získat nástroje Azure](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

