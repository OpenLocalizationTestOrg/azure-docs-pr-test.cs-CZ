---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 2: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Nainstalovat nástroje hello a hello software na hostiteli počítače se spuštěným systémem Ubuntu, vytvoření služby IoT hub a registraci zařízení ve hello IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, iot Cloudová služba pro internet věcí softwaru, azure cli, nainstalujte git na ubuntu, gulp spustit, nainstalujte ubuntu js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0bac1412-385b-4255-a33f-9d44c35feb3e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c9edca91e791ef914b1920178b66eadd12ae0281
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a>Získat nástroje hello (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete

- Nainstalujte Git, Node.js, Gulp, Python.
- Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI). 

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).
## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak tooinstall Git a Node.js.
  - Git je systém správy verzí distribuované s otevřeným zdrojem. Ukázková aplikace Hello této lekce jsou uloženy na Git.
  - Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.
- Jak toouse NPM tooinstall Node.js nástroje pro vývoj.
  - minimální požadovaná verze Node.js Hello je 4.5 LTS.
  - NPM je jedním z hello správce balíčku pro Node.js.
- Jak tooinstall Visual Studio Code.
  - Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS. Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.
- Jak tooinstall hello rozhraní příkazového řádku Azure
  - Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Práce přímo z příkazového řádku tooprovision a spravovat prostředky.
- Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Toodownload připojení Internetu hello nástrojů a softwaru.
- Počítač, který používá Ubuntu 16.04 nebo novější.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js

tooinstall Git a Node.js, postupujte takto:

1. Stiskněte klávesu `Ctrl + Alt + T` tooopen terminál.
2. Spusťte následující příkazy hello:

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a>Instalace nástrojů pro vývoj Node.js

Používáte [gulp.js](http://gulpjs.com/) tooautomate nasazení a spouštění skriptů.

gulp tooinstall, spusťte následující příkaz v terminálu hello hello:

```bash
sudo npm install -g gulp
```

Pokud máte problémy s instalací hello, najdete v části hello [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-troubleshooting.md) řešení toocommon problémů.

> [!Note]
> Uzel, NPM a Gulp jsou skripty pro automatizaci požadované toorun vyvinuté v Node.js.

## <a name="install-hello-azure-cli"></a>Nainstalujte hello rozhraní příkazového řádku Azure

tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v terminálu hello hello:

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   Hello instalace může trvat 5 minut.

2. Hello instalaci ověřte spuštěním hello následující příkaz:

   ```bash
   az iot -h
   ```
Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.
![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)

### <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

Visual Studio Code použijete později v hello kurz tooedit konfigurační soubory.

[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.

## <a name="summary"></a>Souhrn

Všechny požadované hello nástrojů a softwaru jste nainstalovali v hostitelském počítači. Svůj další úkol je toouse hello rozhraní příkazového řádku Azure toocreate služby IoT hub a registraci zařízení ve službě IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT Hub a registrace zařízení](iot-hub-gateway-kit-c-lesson2-register-device.md)
