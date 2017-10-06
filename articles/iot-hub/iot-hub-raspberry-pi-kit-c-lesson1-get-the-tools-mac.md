---
title: "Connect Raspberry PI (C) tooAzure IoT - lekci 1: získání nástroje (macOS) | Microsoft Docs"
description: "Stáhněte a nainstalujte v systému macOS hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro platformy."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, internet věcí software, nainstalujte git v systému mac, gulp, spusťte instalaci uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fc6bd2c8-a847-4bf5-818f-6f7f9a6835ee
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 68ee44945dd69edcdf61ab3da4c80379c0ef955d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a>Získat nástroje hello (systému macOS 10.10)
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
Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooyour pí. Použití hello [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) tooretrieve sítě informace o zařízení IoT.

Nainstalujte `gulp` a `device-discovery-cli` tak, že spustíte následující příkaz v terminálu hello hello:

```bash
sudo npm install -g device-discovery-cli gulp
```

Pokud máte problémy instalace Node.js a tyto další vývoj nástrojů v systému macOS, přečtěte si téma hello [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) řešení toocommon problémů.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code
[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.

## <a name="summary"></a>Souhrn
Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci. Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na pí.

## <a name="next-steps"></a>Další kroky
[Vytvoření a nasazení aplikace hello blikání](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

