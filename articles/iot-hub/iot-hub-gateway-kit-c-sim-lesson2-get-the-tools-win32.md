---
title: "Simulované zařízení & brány Azure IoT - lekci 2: získání nástroje (Windows) | Microsoft Docs"
description: "Nainstalovat nástroje hello a hello software na hostiteli počítače se spuštěným systémem Windows, vytvoření služby IoT hub a registraci zařízení ve hello IoT hub."
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
ms.openlocfilehash: 7ca3c9f11eb232f853fc8fd921b0a49ae37d0184
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-and-later"></a>Získat nástroje hello (Windows 7 a novější)
> [!div class="op_single_selector"]
> * [Windows 7 nebo novější](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete

- Nainstalujte Git, Node.js, Gulp, Python.
- Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI). 

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak tooinstall [Git](https://git-scm.com/) a [Node.js](https://nodejs.org/en/).
  - Git je systém správy verzí distribuované s otevřeným zdrojem. Ukázková aplikace Hello této lekce jsou uloženy na Git.
  - Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.
- Jak toouse [NPM](https://www.npmjs.com/) tooinstall nástroje pro vývoj Node.js.
  - minimální požadovaná verze Node.js Hello je 4.5 LTS.
  - NPM je jedním z hello správce balíčku pro Node.js.
- Jak tooinstall Visual Studio Code.
  - Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS. Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.
- Jak tooinstall Python.
  - Python je často používaný vysoké úrovně, pro obecné účely, interpretovaný a dynamické programovací jazyk.
- Jak tooinstall hello rozhraní příkazového řádku Azure.
  - Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Práce přímo z příkazového řádku tooprovision a spravovat prostředky.
- Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Toodownload připojení Internetu hello nástrojů a softwaru.
- Počítač se systémem Windows.

## <a name="install-git-and-nodejs"></a>Nainstalovat Git a Node.js

Klikněte na následující odkazy toodownload hello a nainstalujte Git a LTS Node.js pro systém Windows.

- [Získat Git pro Windows](https://git-scm.com/download/win/)
- [Získat Node.js LTS pro Windows](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a>Instalace nástrojů pro vývoj Node.js

Používáte [gulp.js](http://gulpjs.com/) tooautomate nasazení a spouštění skriptů.

Stiskněte klávesu `Windows + R`, typ `cmd` a stiskněte klávesu `Enter` tooopen okno příkazového řádku, a pak spusťte hello následující příkaz:

```cmd
npm install -g gulp
```

Pokud máte problémy s instalací hello, najdete v části hello [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-sim-troubleshooting.md) řešení toocommon problémů.

> [!Note]
> Uzel, NPM a Gulp jsou skripty pro automatizaci požadované toorun vyvinuté v Node.js.

## <a name="install-python"></a>Instalace jazyka Python

Můžete zvolit z Python 2.7, 3.4 nebo 3.5. V tomto kurzu používáme Python 2.7. Pokud jste již nainstalovali python, přejděte toohello další části.

[Získat jazyk Python pro systém Windows](https://www.python.org/downloads/)

Musíte taky tooadd hello cesta hello složek, kdy jsou Python.exe a pip.exe systému nainstalovaná toohello `PATH` proměnné prostředí. Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.

## <a name="install-hello-azure-cli"></a>Nainstalujte hello rozhraní příkazového řádku Azure

tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:

1. Otevřete okno příkazového řádku jako správce.

2. Nainstalujte rozhraní příkazového řádku Azure hello spuštěním hello následující příkazy:

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   Hello instalace může trvat 5 minut.

3. Hello instalaci ověřte spuštěním hello následující příkaz:

   ```cmd
   az iot -h
   ```

   Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.

   ![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a>Nainstalovat Visual Studio Code

Visual Studio Code použijete později v hello kurz tooedit konfigurační soubory.

[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.

## <a name="summary"></a>Souhrn

Všechny požadované hello nástrojů a softwaru jste nainstalovali v hostitelském počítači. Svůj další úkol je toouse hello rozhraní příkazového řádku Azure toocreate služby IoT hub a registraci zařízení ve službě IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT Hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
