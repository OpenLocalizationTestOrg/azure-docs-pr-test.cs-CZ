---
title: "Connect Raspberry PI (C) k Azure IoT - lekci 1: získání nástroje (Windows) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro platformy Windows 7 a novější verze."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot softwaru, internet věcí softwaru, nainstalovat git v systému windows, nainstalujte windows js uzlu, nainstalujte npm v systému windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: bd765ddd-65b7-4241-a391-dc77cb3af1c0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0e58975f4411f97223b2c4374bdd746fe6628c42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a>Získat nástroje (Windows 7 nebo novější)

> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete
Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro malin pí 3. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

> [!NOTE]
> I když programovací jazyk Hlavní logika je C, Node.js nástroje se používají v poznatky zjišťovat zařízení a sestavení a nasazení ukázkové aplikace.

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

Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro platformy. Použití [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) k načtení sítě informací o zařízení IoT.

Spusťte příkazový řádek jako správce. Nainstalujte `gulp` a `device-discovery-cli` tak, že spustíte následující příkaz:

```bash
npm install -g device-discovery-cli gulp
```

Pokud máte problémy s instalací Node.js a tyto další nástroje pro vývoj Node.js do počítače, přečtěte si téma [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) pro řešení běžných potíží.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.

## <a name="summary"></a>Souhrn

Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci. Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na pí.

## <a name="next-steps"></a>Další kroky

[Vytvoření a nasazení aplikace blink](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)
