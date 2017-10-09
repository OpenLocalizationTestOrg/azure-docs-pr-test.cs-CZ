---
title: "Simulované zařízení & brány Azure IoT - lekci 2: získání nástroje (macOS) | Microsoft Docs"
description: "Instalace nástrojů v počítači Mac, vytvoření služby IoT hub a registraci zařízení ve hello IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, iot cloudové služby internet věcí softwaru, rozhraní příkazového řádku azure, nainstalujte mac python, nainstalovat git v systému mac, gulp, spusťte instalaci uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 42f9d186-e20c-4ef9-98cc-71d39e058b06
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 391d60f3cbb209698cae53098efed360ac0f5fad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos"></a>Získat nástroje hello (macOS)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete

- Nainstalujte Git, Node.js, Gulp, Python.
- Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI). 

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak tooinstall [Git](https://git-scm.com/) a [Node.js](https://nodejs.org/en/).
  - Git je systém správy verzí distribuované s otevřeným zdrojem. Ukázková aplikace Hello této lekce jsou uloženy na Git.
  - Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.
- Jak toouse [NPM](https://www.npmjs.com/) tooinstall nástroje pro vývoj Node.js.
  - minimální požadovaná verze Node.js Hello je 4.5 LTS.
  - NPM je jedním z hello správce balíčku pro Node.js.
- Jak tooinstall Visual Studio Code.
  - Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS. Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.
- Jak tooinstall Python.
  - Python je často používaný vysoké úrovně, pro obecné účely, interpretovaný a dynamické programovací jazyk.
- Jak tooinstall hello rozhraní příkazového řádku Azure.
  - Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Práce přímo z příkazového řádku tooprovision a spravovat prostředky.
- Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Toodownload připojení Internetu hello nástrojů a softwaru.
- Počítač Mac se systémem OS X Yosemite (10.10) nebo novější.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js

tooinstall Git a Node.js, použijte nástroj pro správu hello Homebrew balíček pomocí následujících kroků:

1. [Stáhněte si](http://brew.sh/) a nainstalujte Homebrew. Pokud jste již nainstalovali Homebrew, přejděte toostep 2.
   1. Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` tooopen terminál.
   2. Spusťte následující příkaz hello:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. Nainstalujte Git a Node.js spuštěním hello následující příkaz:

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a>Instalace nástrojů pro vývoj Node.js

Používáte [gulp.js](http://gulpjs.com/) tooautomate nasazení a spouštění skriptů.

gulp tooinstall, spusťte následující příkaz v terminálu hello hello:

```bash
npm install -g gulp
```

Pokud máte problémy s instalací hello, najdete v části hello [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-sim-troubleshooting.md) řešení toocommon problémů.

> [!Note]
> Uzel, NPM a Gulp jsou skripty pro automatizaci požadované toorun vyvinuté v Node.js.

## <a name="install-python"></a>Instalace jazyka Python

I když Mac OS X se dodává s Python 2.7, doporučujeme nainstalovat Python prostřednictvím Homebrew. V tématu [instalaci jazyka Python v systému Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).

Instalace jazyka Python a pip spuštěním hello následující příkaz:

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a>Nainstalujte hello rozhraní příkazového řádku Azure

tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v terminálu hello hello:
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   Hello instalace může trvat 5 minut.

2. Hello instalaci ověřte spuštěním hello následující příkaz:
   ```bash
   az iot -h
   ```
   Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.

   ![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

Visual Studio Code použijete později v hello kurz tooedit konfigurační soubory.

[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code.

## <a name="summary"></a>Souhrn

V počítači Mac jste nainstalovali všechny požadované hello nástrojů a softwaru. Svůj další úkol je toouse hello rozhraní příkazového řádku Azure toocreate služby IoT hub a registraci zařízení ve službě IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
