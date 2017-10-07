---
title: "Připojit Arduino tooAzure IoT - lekci 1: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte na Ubuntu hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Adafruit prolnutí M0 Wi-Fi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git na ubuntu, ubuntu js uzlu instalace"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 7572f191-420d-41f0-923b-7ea86c0bfa73
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 586f89025d2fa11a31cb782e3789d306ade018a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a>Získat nástroje hello (Ubuntu 16.04)

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

* Jak tooinstall Git a Node.js
  * [Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem. Hello ukázkovou aplikaci pro tento článek je uložený na Git.
  * [Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.
* Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.
  * minimální požadovaná verze Node.js Hello je 4.5 LTS.
  * [NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.

## <a name="what-you-need"></a>Co potřebujete
toocomplete této operace, budete potřebovat:
* Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.
* Počítač, který používá Ubuntu 16.04 nebo novější.

## <a name="install-git-nodejs-and-npm"></a>Nainstalovat Git, Node.js a NPM
Použití hello klávesové zkratky `Ctrl + Alt + T` tooopen hello terminálu a spusťte následující příkazy:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a>Instalace dalších nástrojů pro vývoj Node.js
Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooyour Arduino panelu.

Nainstalujte `gulp`, `device-discovery-cli` tak, že spustíte následující příkaz v terminálu hello hello:

```bash
sudo npm install -g gulp device-discovery-cli
```

Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code
[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.

## <a name="summary"></a>Souhrn
Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci. Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na panel Arduino.

## <a name="next-steps"></a>Další kroky
[Vytvoření a nasazení ukázkové aplikace hello blikání][create-and-deploy-the-blink-sample-application]

<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-sample-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md