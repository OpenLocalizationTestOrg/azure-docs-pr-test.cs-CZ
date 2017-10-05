---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 2: získání nástroje (Windows) | Microsoft Docs"
description: "Instalace nástroje a software na hostiteli počítače se spuštěným systémem Windows, vytvoření služby IoT hub a registraci zařízení ve službě IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, iot Cloudová služba pro internet věcí softwaru, azure cli, nainstalovat git v systému windows, gulp spustit, nainstalovat windows js uzlu, nainstalujte npm v systému windows, nainstalujte python v systému windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 18ae6ee4-574a-4d5f-9838-ca2a78165628
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0d8ba03df63d0b8657a9e275fc636e806c66b683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-and-later"></a><span data-ttu-id="85640-104">Získat nástroje (Windows 7 a novější)</span><span class="sxs-lookup"><span data-stu-id="85640-104">Get the tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="85640-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="85640-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="85640-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="85640-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="85640-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="85640-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="85640-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="85640-108">What you will do</span></span>

- <span data-ttu-id="85640-109">Nainstalujte Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="85640-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="85640-110">Nainstalujte rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="85640-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="85640-111">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="85640-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="85640-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="85640-112">What you will learn</span></span>

<span data-ttu-id="85640-113">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="85640-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="85640-114">Postup instalace [Git](https://git-scm.com/) a [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="85640-114">How to install [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="85640-115">Git je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="85640-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="85640-116">Ukázkové aplikace pro tento účel jsou uloženy na Git.</span><span class="sxs-lookup"><span data-stu-id="85640-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="85640-117">Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="85640-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="85640-118">Jak používat [NPM](https://www.npmjs.com/) instalace nástrojů pro vývoj Node.js.</span><span class="sxs-lookup"><span data-stu-id="85640-118">How to use [NPM](https://www.npmjs.com/) to install Node.js development tools.</span></span>
  - <span data-ttu-id="85640-119">Minimální požadovaná verze Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="85640-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="85640-120">NPM je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="85640-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="85640-121">Postup instalace Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="85640-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="85640-122">Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="85640-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="85640-123">Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.</span><span class="sxs-lookup"><span data-stu-id="85640-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="85640-124">Postup instalace Python.</span><span class="sxs-lookup"><span data-stu-id="85640-124">How to install Python.</span></span>
  - <span data-ttu-id="85640-125">Python je často používaný vysoké úrovně, pro obecné účely, interpretovaný a dynamické programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="85640-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="85640-126">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="85640-126">How to install the Azure CLI.</span></span>
  - <span data-ttu-id="85640-127">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="85640-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="85640-128">Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.</span><span class="sxs-lookup"><span data-stu-id="85640-128">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="85640-129">Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="85640-129">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="85640-130">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="85640-130">What you need</span></span>

- <span data-ttu-id="85640-131">Připojení k Internetu kvůli stažení nástrojů a softwaru.</span><span class="sxs-lookup"><span data-stu-id="85640-131">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="85640-132">Počítač se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="85640-132">A Windows computer.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="85640-133">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="85640-133">Install Git and Node.js</span></span>

<span data-ttu-id="85640-134">Kliknutím na následující odkazy stáhnout a nainstalovat Git a LTS Node.js pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="85640-134">Click the following links to download and install Git and Node.js LTS for Windows.</span></span>

- [<span data-ttu-id="85640-135">Získat Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="85640-135">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
- [<span data-ttu-id="85640-136">Získat Node.js LTS pro Windows</span><span class="sxs-lookup"><span data-stu-id="85640-136">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="85640-137">Instalace nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="85640-137">Install Node.js development tools</span></span>

<span data-ttu-id="85640-138">Používáte [gulp.js](http://gulpjs.com/) k automatizaci nasazení a spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="85640-138">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="85640-139">Stiskněte klávesu `Windows + R`, typ `cmd` a stiskněte klávesu `Enter` otevřete okno příkazového řádku a potom spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="85640-139">Press `Windows + R`, type `cmd` and press `Enter` to open a Command Prompt window, and then run the following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="85640-140">Pokud máte problémy s instalací, najdete v článku [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="85640-140">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="85640-141">Uzel, NPM a Gulp jsou nutné ke spuštění skripty pro automatizaci vyvinuté v Node.js.</span><span class="sxs-lookup"><span data-stu-id="85640-141">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="85640-142">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="85640-142">Install Python</span></span>

<span data-ttu-id="85640-143">Můžete zvolit z Python 2.7, 3.4 nebo 3.5.</span><span class="sxs-lookup"><span data-stu-id="85640-143">You can choose from Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="85640-144">V tomto kurzu používáme Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="85640-144">In this tutorial, we use Python 2.7.</span></span> <span data-ttu-id="85640-145">Pokud jste již nainstalovali python, přejděte k další části.</span><span class="sxs-lookup"><span data-stu-id="85640-145">If you've already installed python, go to the next section.</span></span>

[<span data-ttu-id="85640-146">Získat jazyk Python pro systém Windows</span><span class="sxs-lookup"><span data-stu-id="85640-146">Get Python for Windows</span></span>](https://www.python.org/downloads/)

<span data-ttu-id="85640-147">Musíte taky přidat cestu složky, kde jsou v systému nainstalovány Python.exe a pip.exe `PATH` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="85640-147">You also need to add the path of the folders where Python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="85640-148">Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="85640-148">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="85640-149">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="85640-149">Install the Azure CLI</span></span>

<span data-ttu-id="85640-150">Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="85640-150">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="85640-151">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="85640-151">Open a Command Prompt window as an administrator.</span></span>

2. <span data-ttu-id="85640-152">Nainstalujte rozhraní příkazového řádku Azure spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="85640-152">Install the Azure CLI by running the following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="85640-153">Instalace může trvat 5 minut.</span><span class="sxs-lookup"><span data-stu-id="85640-153">The installation might take 5 minutes.</span></span>

3. <span data-ttu-id="85640-154">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="85640-154">Verify the installation by running the following command:</span></span>

   ```cmd
   az iot -h
   ```

   <span data-ttu-id="85640-155">Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="85640-155">You should see the following output if the installation is successful.</span></span>

   ![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="85640-157">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="85640-157">Install Visual Studio Code</span></span>

<span data-ttu-id="85640-158">Visual Studio Code použijete později v tomto kurzu upravit konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="85640-158">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="85640-159">[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="85640-159">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="85640-160">Souhrn</span><span class="sxs-lookup"><span data-stu-id="85640-160">Summary</span></span>

<span data-ttu-id="85640-161">Všechny požadované nástroje a softwaru jste nainstalovali v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="85640-161">You've installed all the required tools and software on your host computer.</span></span> <span data-ttu-id="85640-162">Svůj další úkol je použití rozhraní příkazového řádku Azure k vytvoření služby IoT hub a registraci zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="85640-162">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85640-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85640-163">Next steps</span></span>
[<span data-ttu-id="85640-164">Vytvoření služby IoT Hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="85640-164">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
