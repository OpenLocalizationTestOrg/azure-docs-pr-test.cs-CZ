---
title: "Simulované zařízení & brány Azure IoT - lekci 2: získání nástroje (macOS) | Microsoft Docs"
description: "Instalace nástrojů v počítači Mac, vytvoření služby IoT hub a registraci zařízení ve hello IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, iot cloudové služby internet věcí softwaru, rozhraní příkazového řádku azure, nainstalujte mac python, nainstalovat git v systému mac, gulp, spusťte instalaci uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 42f9d186-e20c-4ef9-98cc-71d39e058b06
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 391d60f3cbb209698cae53098efed360ac0f5fad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos"></a><span data-ttu-id="8e0f9-104">Získat nástroje hello (macOS)</span><span class="sxs-lookup"><span data-stu-id="8e0f9-104">Get hello tools (macOS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8e0f9-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="8e0f9-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="8e0f9-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="8e0f9-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="8e0f9-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="8e0f9-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="8e0f9-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="8e0f9-108">What you will do</span></span>

- <span data-ttu-id="8e0f9-109">Nainstalujte Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="8e0f9-110">Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="8e0f9-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="8e0f9-111">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="8e0f9-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="8e0f9-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="8e0f9-112">What you will learn</span></span>

<span data-ttu-id="8e0f9-113">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="8e0f9-114">Jak tooinstall [Git](https://git-scm.com/) a [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="8e0f9-114">How tooinstall [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="8e0f9-115">Git je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="8e0f9-116">Ukázková aplikace Hello této lekce jsou uloženy na Git.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="8e0f9-117">Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="8e0f9-118">Jak toouse [NPM](https://www.npmjs.com/) tooinstall nástroje pro vývoj Node.js.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-118">How toouse [NPM](https://www.npmjs.com/) tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="8e0f9-119">minimální požadovaná verze Node.js Hello je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="8e0f9-120">NPM je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="8e0f9-121">Jak tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="8e0f9-122">Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="8e0f9-123">Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="8e0f9-124">Jak tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-124">How tooinstall Python.</span></span>
  - <span data-ttu-id="8e0f9-125">Python je často používaný vysoké úrovně, pro obecné účely, interpretovaný a dynamické programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="8e0f9-126">Jak tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-126">How tooinstall hello Azure CLI.</span></span>
  - <span data-ttu-id="8e0f9-127">Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="8e0f9-128">Práce přímo z příkazového řádku tooprovision a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-128">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="8e0f9-129">Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-129">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8e0f9-130">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="8e0f9-130">What you need</span></span>

- <span data-ttu-id="8e0f9-131">Toodownload připojení Internetu hello nástrojů a softwaru.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-131">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="8e0f9-132">Počítač Mac se systémem OS X Yosemite (10.10) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-132">A Mac computer that’s running OS X Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="8e0f9-133">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="8e0f9-133">Install Git and Node.js</span></span>

<span data-ttu-id="8e0f9-134">tooinstall Git a Node.js, použijte nástroj pro správu hello Homebrew balíček pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-134">tooinstall Git and Node.js, use hello Homebrew package management utility by following these steps:</span></span>

1. <span data-ttu-id="8e0f9-135">[Stáhněte si](http://brew.sh/) a nainstalujte Homebrew.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-135">[Download](http://brew.sh/) and install Homebrew.</span></span> <span data-ttu-id="8e0f9-136">Pokud jste již nainstalovali Homebrew, přejděte toostep 2.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-136">If you’ve already installed Homebrew, go toostep 2.</span></span>
   1. <span data-ttu-id="8e0f9-137">Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` tooopen terminál.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-137">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="8e0f9-138">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-138">Run hello following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. <span data-ttu-id="8e0f9-139">Nainstalujte Git a Node.js spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-139">Install Git and Node.js by running hello following command:</span></span>

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="8e0f9-140">Instalace nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="8e0f9-140">Install Node.js development tools</span></span>

<span data-ttu-id="8e0f9-141">Používáte [gulp.js](http://gulpjs.com/) tooautomate nasazení a spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-141">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="8e0f9-142">gulp tooinstall, spusťte následující příkaz v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-142">tooinstall gulp, run hello following command in hello terminal:</span></span>

```bash
npm install -g gulp
```

<span data-ttu-id="8e0f9-143">Pokud máte problémy s instalací hello, najdete v části hello [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-sim-troubleshooting.md) řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-143">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-sim-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="8e0f9-144">Uzel, NPM a Gulp jsou skripty pro automatizaci požadované toorun vyvinuté v Node.js.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-144">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="8e0f9-145">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="8e0f9-145">Install Python</span></span>

<span data-ttu-id="8e0f9-146">I když Mac OS X se dodává s Python 2.7, doporučujeme nainstalovat Python prostřednictvím Homebrew.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-146">Although Mac OS X comes with Python 2.7, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="8e0f9-147">V tématu [instalaci jazyka Python v systému Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="8e0f9-147">See [Installing Python on Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="8e0f9-148">Instalace jazyka Python a pip spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-148">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="8e0f9-149">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8e0f9-149">Install hello Azure CLI</span></span>

<span data-ttu-id="8e0f9-150">tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-150">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="8e0f9-151">Spusťte následující příkazy v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-151">Run hello following commands in hello terminal:</span></span>
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   <span data-ttu-id="8e0f9-152">Hello instalace může trvat 5 minut.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-152">hello installation might take 5 minutes.</span></span>

2. <span data-ttu-id="8e0f9-153">Hello instalaci ověřte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8e0f9-153">Verify hello installation by running hello following command:</span></span>
   ```bash
   az iot -h
   ```
   <span data-ttu-id="8e0f9-154">Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-154">You should see hello following output if hello installation is successful.</span></span>

   ![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="8e0f9-156">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e0f9-156">Install Visual Studio Code</span></span>

<span data-ttu-id="8e0f9-157">Visual Studio Code použijete později v hello kurz tooedit konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-157">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="8e0f9-158">[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-158">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="8e0f9-159">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8e0f9-159">Summary</span></span>

<span data-ttu-id="8e0f9-160">V počítači Mac jste nainstalovali všechny požadované hello nástrojů a softwaru.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-160">You’ve installed all hello required tools and software on your Mac computer.</span></span> <span data-ttu-id="8e0f9-161">Svůj další úkol je toouse hello rozhraní příkazového řádku Azure toocreate služby IoT hub a registraci zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8e0f9-161">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e0f9-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e0f9-162">Next steps</span></span>
[<span data-ttu-id="8e0f9-163">Vytvoření služby IoT hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="8e0f9-163">Create an IoT hub and register Device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
