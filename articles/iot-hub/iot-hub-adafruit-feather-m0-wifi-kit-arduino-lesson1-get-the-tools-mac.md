---
title: "Připojit Arduino tooAzure IoT - lekci 1: získání nástroje (macOS) | Microsoft Docs"
description: "Stáhněte a nainstalujte v systému macOS hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Adafruit prolnutí M0 Wi-Fi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git v systému mac, instalace uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 0262f3dd-0259-4eb0-962f-9fb534f8359e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5fd306fecd7259fb8f1e99d76282a1e464c4d4f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a>Získat nástroje hello (systému macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější][windows]
> * [Ubuntu 16.04][ubuntu]
> * [systému macOS 10.10][macos]

## <a name="what-you-will-do"></a>Co provedete

Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro panel Adafruit prolnutí M0 Wi-Fi Arduino. 

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

> [!NOTE]
> Arduino sice hello programovací jazyk Hlavní logika hello Node.js nástroje se používají v hello lekce toobuild a nasazení ukázkové aplikace.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak tooinstall Git a Node.js.
  * [Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem. Hello ukázkovou aplikaci pro tento článek je uložený na Git.
  * [Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.
* Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.
  * minimální požadovaná verze Node.js Hello je 4.5 LTS.
  * [NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.

## <a name="what-you-need"></a>Co potřebujete
toocomplete této operace, budete potřebovat:
* Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.
* Mac, který běží v systému macOS Yosemite (10.10) nebo novější.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js
tooinstall Git a Node.js, použijte hello [Homebrew](http://brew.sh) balíček nástroj pro správu pomocí následujících kroků:

1. Nainstalujte Homebrew. Pokud jste již nainstalovali Homebrew, přejděte toostep 2.

   1. Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` tooopen terminál.
   2. Spusťte následující příkaz hello:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. Nainstalujte Git a Node.js spuštěním hello následující příkaz:

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a>Instalace dalších nástrojů pro vývoj Node.js
Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooyour Arduino panelu.

Nainstalujte `gulp`, `device-discovery-cli` tak, že spustíte následující příkaz v terminálu hello hello:

```bash
sudo npm install -g gulp device-discovery-cli
```

Pokud máte problémy instalace Node.js a tyto další vývoj nástrojů v systému macOS, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code
[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.

## <a name="summary"></a>Souhrn
Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci. Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na panel Arduino.

## <a name="next-steps"></a>Další kroky
[Vytvoření a nasazení aplikace hello blikání][create-and-deploy-the-blink-application]
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md