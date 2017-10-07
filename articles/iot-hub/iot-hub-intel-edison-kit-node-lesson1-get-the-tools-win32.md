---
title: "Připojit Intel Edison (uzel) tooAzure IoT - lekci 1: získání nástroje (Windows) | Microsoft Docs"
description: "Stáhněte a nainstalujte hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Edison ve Windows 7 a novějších verzích."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git v systému windows, instalace systému windows js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 4164b5a1-5a42-4d8a-9ff6-441e79fcc936
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 933cc585d1b8b0236d76452f5c449ae9f2f3987b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a>Získat nástroje hello (Windows 7 nebo novější)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější][windows]
> * [Ubuntu 16.04][ubuntu]
> * [systému macOS 10.10][macos]

## <a name="what-you-will-do"></a>Co provedete
Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro Intel Edison. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak tooinstall Git a Node.js.
  * [Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem. Hello ukázkovou aplikaci pro tento článek je uložený na Git.
  * [Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.
* Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.
  * požadavky na minimální verzi Hello Node.js je 4.5 LTS.
  * [NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.

## <a name="what-you-need"></a>Co potřebujete

toocomplete této operace, budete potřebovat:

* Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.
* Počítač, který je spuštěn systém Windows.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js

Klikněte na tlačítko hello najdete pod odkazy níže toodownload a nainstalujte Git a LTS Node.js pro systém Windows.

* [Získat Git pro Windows](https://git-scm.com/download/win/)
* [Získat Node.js LTS pro Windows](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a>Instalace dalších nástrojů pro vývoj Node.js

Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooEdison.

Spusťte příkazový řádek jako správce. Nainstalujte `gulp` spuštěním hello následující příkaz:

```cmd
npm install -g gulp
```

Pokud máte problémy s instalací Node.js a tyto další nástroje pro vývoj Node.js do počítače, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.

## <a name="summary"></a>Souhrn

Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci. Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na Edison.

## <a name="next-steps"></a>Další kroky

[Vytvoření a nasazení aplikace hello blikání][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
