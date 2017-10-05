---
title: "Simulované zařízení & brány Azure IoT - lekci 2: získání nástroje (Windows) | Microsoft Docs"
description: "Instalace nástroje a software na hostiteli počítače se spuštěným systémem Windows, vytvoření služby IoT hub a registraci zařízení ve službě IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, iot Cloudová služba pro internet věcí softwaru, azure cli, nainstalovat git v systému windows, gulp spustit, nainstalovat windows js uzlu, nainstalujte npm v systému windows, nainstalujte python v systému windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: c16eee4c-8756-454b-82bf-3eb0dd51aa5f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ecedf5ef89677c73c5d9a3e5250eb9f45f6bad32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-and-later"></a>Získat nástroje (Windows 7 a novější)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete

- Nainstalujte Git, Node.js, Gulp, Python.
- Nainstalujte rozhraní příkazového řádku Azure (Azure CLI). 

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Postup instalace [Git](https://git-scm.com/) a [Node.js](https://nodejs.org/en/).
  - Git je systém správy verzí distribuované s otevřeným zdrojem. Ukázkové aplikace pro tento účel jsou uloženy na Git.
  - Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.
- Jak používat [NPM](https://www.npmjs.com/) instalace nástrojů pro vývoj Node.js.
  - Minimální požadovaná verze Node.js je 4.5 LTS.
  - NPM je jedním z vybraných manažerů balíčku pro Node.js.
- Postup instalace Visual Studio Code.
  - Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS. Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.
- Postup instalace Python.
  - Python je často používaný vysoké úrovně, pro obecné účely, interpretovaný a dynamické programovací jazyk.
- Postup instalace rozhraní příkazového řádku Azure.
  - Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.
- Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Připojení k Internetu kvůli stažení nástrojů a softwaru.
- Počítač se systémem Windows.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js

Kliknutím na následující odkazy stáhnout a nainstalovat Git a LTS Node.js pro systém Windows.

- [Získat Git pro Windows](https://git-scm.com/download/win/)
- [Získat Node.js LTS pro Windows](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a>Instalace nástrojů pro vývoj Node.js

Používáte [gulp.js](http://gulpjs.com/) k automatizaci nasazení a spouštění skriptů.

Stiskněte klávesu `Windows + R`, typ `cmd` a stiskněte klávesu `Enter` otevřete okno příkazového řádku a potom spusťte následující příkaz:

```cmd
npm install -g gulp
```

Pokud máte problémy s instalací, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-sim-troubleshooting.md) pro řešení běžných potíží.

> [!Note]
> Uzel, NPM a Gulp jsou nutné ke spuštění skripty pro automatizaci vyvinuté v Node.js.

## <a name="install-python"></a>Instalace jazyka Python

Můžete zvolit z Python 2.7, 3.4 nebo 3.5. V tomto kurzu používáme Python 2.7. Pokud jste již nainstalovali python, přejděte k další části.

[Získat jazyk Python pro systém Windows](https://www.python.org/downloads/)

Musíte taky přidat cestu složky, kde jsou v systému nainstalovány Python.exe a pip.exe `PATH` proměnné prostředí. Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.

## <a name="install-the-azure-cli"></a>Instalace rozhraní příkazového řádku Azure CLI

Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:

1. Otevřete okno příkazového řádku jako správce.

2. Nainstalujte rozhraní příkazového řádku Azure spuštěním následujících příkazů:

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   Instalace může trvat 5 minut.

3. Ověřte instalaci tak, že spustíte následující příkaz:

   ```cmd
   az iot -h
   ```

   Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.

   ![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

Visual Studio Code použijete později v tomto kurzu upravit konfigurační soubory.

[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.

## <a name="summary"></a>Souhrn

Všechny požadované nástroje a softwaru jste nainstalovali v hostitelském počítači. Svůj další úkol je použití rozhraní příkazového řádku Azure k vytvoření služby IoT hub a registraci zařízení ve službě IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT Hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
