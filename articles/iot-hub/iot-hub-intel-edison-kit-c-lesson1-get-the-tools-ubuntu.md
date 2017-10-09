---
title: "Connect Intel Edison (C) tooAzure IoT - lekci 1: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Edison na Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git na ubuntu, ubuntu js uzlu instalace"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 4c7b8e04-b892-459b-8b03-85bcaff2465c
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1ebd599def4e8bb33d891517cc76bc2fcdc3c35a
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
Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro váš Edison Intel. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

> [!NOTE]
> C sice hello programovací jazyk Hlavní logika hello Node.js nástroje se používají v hello lekce toobuild a nasazení ukázkové aplikace.

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
Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooEdison.

Nainstalujte `gulp` tak, že spustíte následující příkaz v terminálu hello hello:

```bash
sudo npm install -g gulp
```

Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code
[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.

## <a name="summary"></a>Souhrn
Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci. Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na Edison.

## <a name="next-steps"></a>Další kroky
[Vytvoření a nasazení ukázkové aplikace hello blikání][create-and-deploy-the-blink-application]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
