---
title: "Simulované zařízení & brány Azure IoT - lekci 2: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Instalace nástroje a software na hostiteli počítače se spuštěným systémem Ubuntu, vytvoření služby IoT hub a registraci zařízení ve službě IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, iot Cloudová služba pro internet věcí softwaru, azure cli, nainstalujte git na ubuntu, gulp spustit, nainstalujte ubuntu js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cf673154-ce67-4ed7-a9f7-2440301c6270
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 349daf5c3206f7fc20662beebd16928624142a56
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a>Získání nástrojů (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete

- Nainstalujte Git, Node.js, Gulp, Python.
- Nainstalujte rozhraní příkazového řádku Azure (Azure CLI). 

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).
## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak nainstalovat Git a Node.js.
  - Git je systém správy verzí distribuované s otevřeným zdrojem. Ukázkové aplikace pro tento účel jsou uloženy na Git.
  - Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.
- Postup instalace nástroje pro vývoj Node.js pomocí NPM.
  - Minimální požadovaná verze Node.js je 4.5 LTS.
  - NPM je jedním z vybraných manažerů balíčku pro Node.js.
- Postup instalace Visual Studio Code.
  - Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS. Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.
- Postup instalace rozhraní příkazového řádku Azure
  - Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.
- Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Připojení k Internetu kvůli stažení nástrojů a softwaru.
- Počítač, který používá Ubuntu 16.04 nebo novější.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js

Chcete-li nainstalovat Git a Node.js, postupujte takto:

1. Stiskněte klávesu `Ctrl + Alt + T` otevřete terminál.
2. Spusťte následující příkazy:

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a>Instalace nástrojů pro vývoj Node.js

Používáte [gulp.js](http://gulpjs.com/) k automatizaci nasazení a spouštění skriptů.

Pokud chcete nainstalovat gulp, spusťte následující příkaz v terminálu:

```bash
sudo npm install -g gulp
```

Pokud máte problémy s instalací, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-sim-troubleshooting.md) pro řešení běžných potíží.

> [!Note]
> Uzel, NPM a Gulp jsou nutné ke spuštění skripty pro automatizaci vyvinuté v Node.js.

## <a name="install-the-azure-cli"></a>Instalace rozhraní příkazového řádku Azure CLI

Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v terminálu:

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   Instalace může trvat 5 minut.

2. Ověřte instalaci tak, že spustíte následující příkaz:

   ```bash
   az iot -h
   ```
Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.
![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)

### <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

Visual Studio Code použijete později v tomto kurzu upravit konfigurační soubory.

[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.

## <a name="summary"></a>Souhrn

Všechny požadované nástroje a softwaru jste nainstalovali v hostitelském počítači. Svůj další úkol je použití rozhraní příkazového řádku Azure k vytvoření služby IoT hub a registraci zařízení ve službě IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT Hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
