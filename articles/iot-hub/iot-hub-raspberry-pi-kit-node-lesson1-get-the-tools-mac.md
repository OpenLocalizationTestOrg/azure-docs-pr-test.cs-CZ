---
title: "Připojení k Azure IoT - lekci 1 malin platformy (uzel): získat nástroje (macOS) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro platformy v systému macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vývoj pro IOT, iot softwaru, pro internet věcí software, nainstalujte python mac, nainstalovat git v systému mac, gulp spustit, nainstalujte uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2ea6d211-c0e8-4ade-ac69-d1c2261d78c4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 6c2338baa8e88bab4c4be64568220f53178943cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a>Získání nástrojů (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete
Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro váš malin pí 3. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak nainstalovat Git a Node.js.
  * [Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem. Ukázkovou aplikaci pro tento článek je uložený na Git.
  * [Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.
* Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.
  * Minimální požadovaná verze Node.js je 4.5 LTS.
  * [NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.

## <a name="what-you-need"></a>Co potřebujete
Pro dokončení této operace, budete potřebovat:

* Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.
* Mac, který běží v systému macOS Yosemite (10.10) nebo novější.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js
Chcete-li nainstalovat Git a Node.js, použijte [Homebrew](http://brew.sh) balíček nástroj pro správu pomocí následujících kroků:

1. Nainstalujte Homebrew. Pokud jste již nainstalovali Homebrew, přejděte ke kroku 2.
   
   1. Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` otevřete terminál.
   2. Spusťte následující příkaz:
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. Nainstalujte Git a Node.js spuštěním následujícího příkazu:
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a>Instalace dalších nástrojů pro vývoj Node.js
Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro platformy. Použití [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) k načtení sítě informací o zařízení IoT.

Nainstalujte `gulp` a `device-discovery-cli` spuštěním následujícího příkazu v terminálu:

```bash
npm install -g device-discovery-cli gulp
```

Pokud máte problémy instalace Node.js a tyto další vývoj nástrojů v systému macOS, přečtěte si téma [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-node-troubleshooting.md) pro řešení běžných potíží.

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code
[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code. Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS. Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.

## <a name="summary"></a>Souhrn
Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci. Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na pí.

## <a name="next-steps"></a>Další kroky
[Vytvoření a nasazení ukázkové aplikace blikání](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

