---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 2: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Instalace nástroje a software na hostiteli počítače se spuštěným systémem Ubuntu, vytvoření služby IoT hub a registraci zařízení ve službě IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, iot Cloudová služba pro internet věcí softwaru, azure cli, nainstalujte git na ubuntu, gulp spustit, nainstalujte ubuntu js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0bac1412-385b-4255-a33f-9d44c35feb3e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 234b60e1f8eaff52ce07f54d4d12de2421cc1a52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="28fd1-104">Získání nástrojů (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="28fd1-104">Get the tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="28fd1-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="28fd1-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="28fd1-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="28fd1-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="28fd1-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="28fd1-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="28fd1-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="28fd1-108">What you will do</span></span>

- <span data-ttu-id="28fd1-109">Nainstalujte Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="28fd1-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="28fd1-110">Nainstalujte rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="28fd1-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="28fd1-111">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="28fd1-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>
## <a name="what-you-will-learn"></a><span data-ttu-id="28fd1-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="28fd1-112">What you will learn</span></span>

<span data-ttu-id="28fd1-113">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="28fd1-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="28fd1-114">Jak nainstalovat Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="28fd1-114">How to install Git and Node.js.</span></span>
  - <span data-ttu-id="28fd1-115">Git je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="28fd1-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="28fd1-116">Ukázkové aplikace pro tento účel jsou uloženy na Git.</span><span class="sxs-lookup"><span data-stu-id="28fd1-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="28fd1-117">Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="28fd1-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="28fd1-118">Postup instalace nástroje pro vývoj Node.js pomocí NPM.</span><span class="sxs-lookup"><span data-stu-id="28fd1-118">How to use NPM to install Node.js development tools.</span></span>
  - <span data-ttu-id="28fd1-119">Minimální požadovaná verze Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="28fd1-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="28fd1-120">NPM je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="28fd1-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="28fd1-121">Postup instalace Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="28fd1-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="28fd1-122">Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="28fd1-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="28fd1-123">Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.</span><span class="sxs-lookup"><span data-stu-id="28fd1-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="28fd1-124">Postup instalace rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="28fd1-124">How to install the Azure CLI</span></span>
  - <span data-ttu-id="28fd1-125">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="28fd1-125">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="28fd1-126">Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.</span><span class="sxs-lookup"><span data-stu-id="28fd1-126">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="28fd1-127">Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="28fd1-127">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="28fd1-128">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="28fd1-128">What you need</span></span>

- <span data-ttu-id="28fd1-129">Připojení k Internetu kvůli stažení nástrojů a softwaru.</span><span class="sxs-lookup"><span data-stu-id="28fd1-129">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="28fd1-130">Počítač, který používá Ubuntu 16.04 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="28fd1-130">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="28fd1-131">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="28fd1-131">Install Git and Node.js</span></span>

<span data-ttu-id="28fd1-132">Chcete-li nainstalovat Git a Node.js, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="28fd1-132">To install Git and Node.js, follow these steps:</span></span>

1. <span data-ttu-id="28fd1-133">Stiskněte klávesu `Ctrl + Alt + T` otevřete terminál.</span><span class="sxs-lookup"><span data-stu-id="28fd1-133">Press `Ctrl + Alt + T` to open a terminal.</span></span>
2. <span data-ttu-id="28fd1-134">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="28fd1-134">Run the following commands:</span></span>

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="28fd1-135">Instalace nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="28fd1-135">Install Node.js development tools</span></span>

<span data-ttu-id="28fd1-136">Používáte [gulp.js](http://gulpjs.com/) k automatizaci nasazení a spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="28fd1-136">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="28fd1-137">Pokud chcete nainstalovat gulp, spusťte následující příkaz v terminálu:</span><span class="sxs-lookup"><span data-stu-id="28fd1-137">To install gulp, run the following command in the terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="28fd1-138">Pokud máte problémy s instalací, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="28fd1-138">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="28fd1-139">Uzel, NPM a Gulp jsou nutné ke spuštění skripty pro automatizaci vyvinuté v Node.js.</span><span class="sxs-lookup"><span data-stu-id="28fd1-139">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="28fd1-140">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="28fd1-140">Install the Azure CLI</span></span>

<span data-ttu-id="28fd1-141">Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="28fd1-141">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="28fd1-142">Spusťte následující příkazy v terminálu:</span><span class="sxs-lookup"><span data-stu-id="28fd1-142">Run the following commands in the terminal:</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="28fd1-143">Instalace může trvat 5 minut.</span><span class="sxs-lookup"><span data-stu-id="28fd1-143">The installation might take 5 minutes.</span></span>

2. <span data-ttu-id="28fd1-144">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="28fd1-144">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```
<span data-ttu-id="28fd1-145">Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="28fd1-145">You should see the following output if the installation is successful.</span></span>
<span data-ttu-id="28fd1-146">![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="28fd1-146">![Verify Azure CLI installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span></span>

### <a name="install-visual-studio-code"></a><span data-ttu-id="28fd1-147">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="28fd1-147">Install Visual Studio Code</span></span>

<span data-ttu-id="28fd1-148">Visual Studio Code použijete později v tomto kurzu upravit konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="28fd1-148">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="28fd1-149">[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="28fd1-149">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="28fd1-150">Souhrn</span><span class="sxs-lookup"><span data-stu-id="28fd1-150">Summary</span></span>

<span data-ttu-id="28fd1-151">Všechny požadované nástroje a softwaru jste nainstalovali v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="28fd1-151">You've installed all the required tools and software on your host computer.</span></span> <span data-ttu-id="28fd1-152">Svůj další úkol je použití rozhraní příkazového řádku Azure k vytvoření služby IoT hub a registraci zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="28fd1-152">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28fd1-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28fd1-153">Next steps</span></span>
[<span data-ttu-id="28fd1-154">Vytvoření služby IoT Hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="28fd1-154">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
