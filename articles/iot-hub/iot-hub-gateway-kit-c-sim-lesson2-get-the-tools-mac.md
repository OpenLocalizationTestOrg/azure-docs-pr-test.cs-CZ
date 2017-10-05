---
title: "Simulované zařízení & brány Azure IoT - lekci 2: získání nástroje (macOS) | Microsoft Docs"
description: "Instalace nástrojů v počítači Mac, vytvoření služby IoT hub a registraci zařízení ve službě IoT hub."
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
ms.openlocfilehash: d86332816130de7a6951a74ceb215c8ce476d5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos"></a><span data-ttu-id="5de0c-104">Získat nástroje (macOS)</span><span class="sxs-lookup"><span data-stu-id="5de0c-104">Get the tools (macOS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5de0c-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="5de0c-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="5de0c-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="5de0c-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="5de0c-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="5de0c-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="5de0c-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="5de0c-108">What you will do</span></span>

- <span data-ttu-id="5de0c-109">Nainstalujte Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="5de0c-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="5de0c-110">Nainstalujte rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="5de0c-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="5de0c-111">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5de0c-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5de0c-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="5de0c-112">What you will learn</span></span>

<span data-ttu-id="5de0c-113">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="5de0c-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="5de0c-114">Postup instalace [Git](https://git-scm.com/) a [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="5de0c-114">How to install [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="5de0c-115">Git je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="5de0c-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="5de0c-116">Ukázkové aplikace pro tento účel jsou uloženy na Git.</span><span class="sxs-lookup"><span data-stu-id="5de0c-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="5de0c-117">Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="5de0c-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="5de0c-118">Jak používat [NPM](https://www.npmjs.com/) instalace nástrojů pro vývoj Node.js.</span><span class="sxs-lookup"><span data-stu-id="5de0c-118">How to use [NPM](https://www.npmjs.com/) to install Node.js development tools.</span></span>
  - <span data-ttu-id="5de0c-119">Minimální požadovaná verze Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="5de0c-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="5de0c-120">NPM je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="5de0c-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="5de0c-121">Postup instalace Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5de0c-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="5de0c-122">Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="5de0c-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="5de0c-123">Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.</span><span class="sxs-lookup"><span data-stu-id="5de0c-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="5de0c-124">Postup instalace Python.</span><span class="sxs-lookup"><span data-stu-id="5de0c-124">How to install Python.</span></span>
  - <span data-ttu-id="5de0c-125">Python je často používaný vysoké úrovně, pro obecné účely, interpretovaný a dynamické programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="5de0c-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="5de0c-126">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5de0c-126">How to install the Azure CLI.</span></span>
  - <span data-ttu-id="5de0c-127">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5de0c-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="5de0c-128">Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5de0c-128">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="5de0c-129">Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5de0c-129">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5de0c-130">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="5de0c-130">What you need</span></span>

- <span data-ttu-id="5de0c-131">Připojení k Internetu kvůli stažení nástrojů a softwaru.</span><span class="sxs-lookup"><span data-stu-id="5de0c-131">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="5de0c-132">Počítač Mac se systémem OS X Yosemite (10.10) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5de0c-132">A Mac computer that’s running OS X Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="5de0c-133">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="5de0c-133">Install Git and Node.js</span></span>

<span data-ttu-id="5de0c-134">K instalaci Git a Node.js, použijte nástroj pro správu balíček Homebrew pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="5de0c-134">To install Git and Node.js, use the Homebrew package management utility by following these steps:</span></span>

1. <span data-ttu-id="5de0c-135">[Stáhněte si](http://brew.sh/) a nainstalujte Homebrew.</span><span class="sxs-lookup"><span data-stu-id="5de0c-135">[Download](http://brew.sh/) and install Homebrew.</span></span> <span data-ttu-id="5de0c-136">Pokud jste již nainstalovali Homebrew, přejděte ke kroku 2.</span><span class="sxs-lookup"><span data-stu-id="5de0c-136">If you’ve already installed Homebrew, go to step 2.</span></span>
   1. <span data-ttu-id="5de0c-137">Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` otevřete terminál.</span><span class="sxs-lookup"><span data-stu-id="5de0c-137">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="5de0c-138">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5de0c-138">Run the following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. <span data-ttu-id="5de0c-139">Nainstalujte Git a Node.js spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5de0c-139">Install Git and Node.js by running the following command:</span></span>

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="5de0c-140">Instalace nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="5de0c-140">Install Node.js development tools</span></span>

<span data-ttu-id="5de0c-141">Používáte [gulp.js](http://gulpjs.com/) k automatizaci nasazení a spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="5de0c-141">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="5de0c-142">Pokud chcete nainstalovat gulp, spusťte následující příkaz v terminálu:</span><span class="sxs-lookup"><span data-stu-id="5de0c-142">To install gulp, run the following command in the terminal:</span></span>

```bash
npm install -g gulp
```

<span data-ttu-id="5de0c-143">Pokud máte problémy s instalací, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-sim-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="5de0c-143">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-sim-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="5de0c-144">Uzel, NPM a Gulp jsou nutné ke spuštění skripty pro automatizaci vyvinuté v Node.js.</span><span class="sxs-lookup"><span data-stu-id="5de0c-144">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="5de0c-145">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="5de0c-145">Install Python</span></span>

<span data-ttu-id="5de0c-146">I když Mac OS X se dodává s Python 2.7, doporučujeme nainstalovat Python prostřednictvím Homebrew.</span><span class="sxs-lookup"><span data-stu-id="5de0c-146">Although Mac OS X comes with Python 2.7, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="5de0c-147">V tématu [instalaci jazyka Python v systému Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="5de0c-147">See [Installing Python on Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="5de0c-148">Instalace Python a pip spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5de0c-148">Install Python and pip by running the following command:</span></span>

```bash
brew install python
```

## <a name="install-the-azure-cli"></a><span data-ttu-id="5de0c-149">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5de0c-149">Install the Azure CLI</span></span>

<span data-ttu-id="5de0c-150">Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5de0c-150">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="5de0c-151">Spusťte následující příkazy v terminálu:</span><span class="sxs-lookup"><span data-stu-id="5de0c-151">Run the following commands in the terminal:</span></span>
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   <span data-ttu-id="5de0c-152">Instalace může trvat 5 minut.</span><span class="sxs-lookup"><span data-stu-id="5de0c-152">The installation might take 5 minutes.</span></span>

2. <span data-ttu-id="5de0c-153">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5de0c-153">Verify the installation by running the following command:</span></span>
   ```bash
   az iot -h
   ```
   <span data-ttu-id="5de0c-154">Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="5de0c-154">You should see the following output if the installation is successful.</span></span>

   ![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="5de0c-156">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5de0c-156">Install Visual Studio Code</span></span>

<span data-ttu-id="5de0c-157">Visual Studio Code použijete později v tomto kurzu upravit konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="5de0c-157">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="5de0c-158">[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5de0c-158">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="5de0c-159">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5de0c-159">Summary</span></span>

<span data-ttu-id="5de0c-160">V počítači Mac jste nainstalovali všechny požadované nástroje a softwaru.</span><span class="sxs-lookup"><span data-stu-id="5de0c-160">You’ve installed all the required tools and software on your Mac computer.</span></span> <span data-ttu-id="5de0c-161">Svůj další úkol je použití rozhraní příkazového řádku Azure k vytvoření služby IoT hub a registraci zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5de0c-161">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5de0c-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5de0c-162">Next steps</span></span>
[<span data-ttu-id="5de0c-163">Vytvoření služby IoT hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="5de0c-163">Create an IoT hub and register Device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
