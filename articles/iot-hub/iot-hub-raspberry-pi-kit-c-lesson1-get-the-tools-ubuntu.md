---
title: "Connect Raspberry PI (C) k Azure IoT - lekci 1: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro platformy na Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot softwaru, pro internet věcí software, nainstalujte git na ubuntu, gulp spustit, nainstalujte ubuntu js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 32cfea00-c254-4cef-8f6f-bbf807eca6b6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 28ebba82e90d91470518cd830c96e6da39d8b9b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a>Získání nástrojů (Ubuntu 16.04)

> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete
Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro váš malin pí 3. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

> [!NOTE]
> I když programovací jazyk Hlavní logika je C, Node.js nástroje se používají v poznatky zjišťovat zařízení a sestavení a nasazení ukázkové aplikace.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak nainstalovat Git a Node.js
  * [Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem. Ukázkovou aplikaci pro tento článek je uložený na Git.
  * [Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.
* Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.
  * Minimální požadovaná verze Node.js je 4.5 LTS.
  * [NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.

## <a name="what-you-need"></a>Co potřebujete
Pro dokončení této operace, budete potřebovat:

* Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.
* Počítač, který používá Ubuntu 16.04 nebo novější.

## <a name="install-git-nodejs-and-npm"></a>Nainstalovat Git, Node.js a NPM
Použijte klávesovou zkratku `Ctrl + Alt + T` otevřete terminál a spusťte následující příkazy:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a>Instalace dalších nástrojů pro vývoj Node.js
Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro platformy. Použití [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) k načtení sítě informací o zařízení IoT.

Nainstalujte `gulp` a `device-discovery-cli` spuštěním následujícího příkazu v terminálu:

```bash
sudo npm install -g device-discovery-cli gulp
```

Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) pro řešení běžných potíží.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code
[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.

## <a name="summary"></a>Souhrn
Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci. Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na pí.

## <a name="next-steps"></a>Další kroky
[Vytvoření a nasazení aplikace blink](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

