---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 2: získání nástroje (macOS) | Microsoft Docs"
description: "Instalace nástrojů v počítači Mac, vytvoření služby IoT hub a registraci zařízení ve službě IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, iot cloudové služby internet věcí softwaru, rozhraní příkazového řádku azure, nainstalujte mac python, nainstalovat git v systému mac, gulp, spusťte instalaci uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 94e538ef-9857-4023-9c3c-e92a0be7c395
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 07bc5f2cf6542273c334371b77520c674c5d2f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos"></a>Získat nástroje (MacOS)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete

- Nainstalujte Git, Node.js, Gulp, Python.
- Nainstalujte rozhraní příkazového řádku Azure (Azure CLI). 

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Postup instalace [Git](https://git-scm.com/) a [Node.js](https://nodejs.org/en/).
  - Git je systém správy verzí distribuované s otevřeným zdrojem. Ukázkové aplikace pro tento účel jsou uloženy na Git.
  - Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.
- Jak používat [NPM](https://www.npmjs.com/) instalace nástrojů pro vývoj Node.js.
  - Minimální požadovaná verze Node.js je 4.5 LTS.
  - NPM je jedním z vybraných manažerů balíčku pro Node.js.
- Postup instalace Visual Studio Code.
  - Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS. Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.
- Postup instalace Python.
  - Python je často používaný vysoké úrovně, pro obecné účely, interpretovaný a dynamické programovací jazyk.
- Postup instalace rozhraní příkazového řádku Azure.
  - Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.
- Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Připojení k Internetu kvůli stažení nástrojů a softwaru.
- Počítač Mac se systémem OS X Yosemite (10.10) nebo novější.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js

K instalaci Git a Node.js, použijte nástroj pro správu balíček Homebrew pomocí následujících kroků:

1. [Stáhněte si](http://brew.sh/) a nainstalujte Homebrew. Pokud jste již nainstalovali Homebrew, přejděte ke kroku 2.
   1. Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` otevřete terminál.
   2. Spusťte následující příkaz:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. Nainstalujte Git a Node.js spuštěním následujícího příkazu:

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a>Instalace nástrojů pro vývoj Node.js

Používáte [gulp.js](http://gulpjs.com/) k automatizaci nasazení a spouštění skriptů.

Pokud chcete nainstalovat gulp, spusťte následující příkaz v terminálu:

```bash
npm install -g gulp
```

Pokud máte problémy s instalací, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-troubleshooting.md) pro řešení běžných potíží.

> [!Note]
> Uzel, NPM a Gulp jsou nutné ke spuštění skripty pro automatizaci vyvinuté v Node.js.

## <a name="install-python"></a>Instalace jazyka Python

I když Mac OS X se dodává s Python 2.7, doporučujeme nainstalovat Python prostřednictvím Homebrew. V tématu [instalaci jazyka Python v systému Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).

Instalace Python a pip spuštěním následujícího příkazu:

```bash
brew install python
```

## <a name="install-the-azure-cli"></a>Instalace rozhraní příkazového řádku Azure CLI

Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v terminálu:
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   Instalace může trvat 5 minut.

2. Ověřte instalaci tak, že spustíte následující příkaz:
   ```bash
   az iot -h
   ```
   Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.

   ![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

Visual Studio Code použijete později v tomto kurzu upravit konfigurační soubory.

[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code.

## <a name="summary"></a>Souhrn

V počítači Mac jste nainstalovali všechny požadované nástroje a softwaru. Svůj další úkol je použití rozhraní příkazového řádku Azure k vytvoření služby IoT hub a registraci zařízení ve službě IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a registrace zařízení](iot-hub-gateway-kit-c-lesson2-register-device.md)
