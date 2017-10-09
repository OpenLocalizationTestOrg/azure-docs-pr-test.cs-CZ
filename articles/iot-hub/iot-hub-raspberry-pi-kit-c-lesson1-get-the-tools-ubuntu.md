---
title: "Connect Raspberry PI (C) tooAzure IoT - lekci 1: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte na Ubuntu hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro platformy."
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
ms.openlocfilehash: 794928b5da63521cb0a72cb54256f2ad9724ec84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a>Získat nástroje hello (Ubuntu 16.04)

> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete
Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro váš malin pí 3. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

> [!NOTE]
> C sice hello programovací jazyk Hlavní logika hello Node.js nástroje se používají v hello lekce toodiscover zařízení a sestavení a nasazení ukázkové aplikace.

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
Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooPi. Použití hello [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) tooretrieve sítě informace o zařízení IoT.

Nainstalujte `gulp` a `device-discovery-cli` tak, že spustíte následující příkaz v terminálu hello hello:

```bash
sudo npm install -g device-discovery-cli gulp
```

Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma hello [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) řešení toocommon problémů.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code
[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.

## <a name="summary"></a>Souhrn
Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci. Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na pí.

## <a name="next-steps"></a>Další kroky
[Vytvoření a nasazení aplikace hello blikání](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

