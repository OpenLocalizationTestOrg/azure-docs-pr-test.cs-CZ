---
title: "Připojení k Azure IoT - lekci 1 Intel Edison (uzel): získat nástroje (Windows) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro Edison ve Windows 7 a novějších verzích."
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
ms.openlocfilehash: 35b7ae24f8616848eaa6affccb96630603b823b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a>Získat nástroje (Windows 7 nebo novější)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější][windows]
> * [Ubuntu 16.04][ubuntu]
> * [systému macOS 10.10][macos]

## <a name="what-you-will-do"></a>Co provedete
Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro Intel Edison. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak nainstalovat Git a Node.js.
  * [Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem. Ukázkovou aplikaci pro tento článek je uložený na Git.
  * [Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.
* Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.
  * Požadavek na minimální verzi Node.js je 4.5 LTS.
  * [NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.

## <a name="what-you-need"></a>Co potřebujete

Pro dokončení této operace, budete potřebovat:

* Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.
* Počítač, který je spuštěn systém Windows.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js

Kliknutím na odkazy níže stáhnout a nainstalovat Git a LTS Node.js pro systém Windows.

* [Získat Git pro Windows](https://git-scm.com/download/win/)
* [Získat Node.js LTS pro Windows](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a>Instalace dalších nástrojů pro vývoj Node.js

Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro Edison.

Spusťte příkazový řádek jako správce. Nainstalujte `gulp` spuštěním následujícího příkazu:

```cmd
npm install -g gulp
```

Pokud máte problémy s instalací Node.js a tyto další nástroje pro vývoj Node.js do počítače, přečtěte si téma [Průvodce odstraňováním potíží s] [ troubleshooting] pro řešení běžných potíží.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.

## <a name="summary"></a>Souhrn

Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci. Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na Edison.

## <a name="next-steps"></a>Další kroky

[Vytvoření a nasazení aplikace blikání][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
