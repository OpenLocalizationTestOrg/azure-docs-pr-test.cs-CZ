---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 2: získání nástroje (Windows) | Microsoft Docs"
description: "Nainstalovat nástroje hello a hello software na hostiteli počítače se spuštěným systémem Windows, vytvoření služby IoT hub a registraci zařízení ve hello IoT hub."
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
ms.openlocfilehash: 3b30b60a0115413394992061a88dde4cd442ac19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-and-later"></a><span data-ttu-id="79b8f-104">Získat nástroje hello (Windows 7 a novější)</span><span class="sxs-lookup"><span data-stu-id="79b8f-104">Get hello tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="79b8f-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="79b8f-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="79b8f-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="79b8f-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="79b8f-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="79b8f-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="79b8f-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="79b8f-108">What you will do</span></span>

- <span data-ttu-id="79b8f-109">Nainstalujte Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="79b8f-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="79b8f-110">Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="79b8f-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="79b8f-111">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="79b8f-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="79b8f-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="79b8f-112">What you will learn</span></span>

<span data-ttu-id="79b8f-113">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="79b8f-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="79b8f-114">Jak tooinstall [Git](https://git-scm.com/) a [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="79b8f-114">How tooinstall [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="79b8f-115">Git je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="79b8f-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="79b8f-116">Ukázková aplikace Hello této lekce jsou uloženy na Git.</span><span class="sxs-lookup"><span data-stu-id="79b8f-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="79b8f-117">Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="79b8f-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="79b8f-118">Jak toouse [NPM](https://www.npmjs.com/) tooinstall nástroje pro vývoj Node.js.</span><span class="sxs-lookup"><span data-stu-id="79b8f-118">How toouse [NPM](https://www.npmjs.com/) tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="79b8f-119">minimální požadovaná verze Node.js Hello je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="79b8f-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="79b8f-120">NPM je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="79b8f-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="79b8f-121">Jak tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="79b8f-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="79b8f-122">Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="79b8f-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="79b8f-123">Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.</span><span class="sxs-lookup"><span data-stu-id="79b8f-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="79b8f-124">Jak tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="79b8f-124">How tooinstall Python.</span></span>
  - <span data-ttu-id="79b8f-125">Python je často používaný vysoké úrovně, pro obecné účely, interpretovaný a dynamické programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="79b8f-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="79b8f-126">Jak tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="79b8f-126">How tooinstall hello Azure CLI.</span></span>
  - <span data-ttu-id="79b8f-127">Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="79b8f-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="79b8f-128">Práce přímo z příkazového řádku tooprovision a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="79b8f-128">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="79b8f-129">Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79b8f-129">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="79b8f-130">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="79b8f-130">What you need</span></span>

- <span data-ttu-id="79b8f-131">Toodownload připojení Internetu hello nástrojů a softwaru.</span><span class="sxs-lookup"><span data-stu-id="79b8f-131">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="79b8f-132">Počítač se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="79b8f-132">A Windows computer.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="79b8f-133">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="79b8f-133">Install Git and Node.js</span></span>

<span data-ttu-id="79b8f-134">Klikněte na následující odkazy toodownload hello a nainstalujte Git a LTS Node.js pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="79b8f-134">Click hello following links toodownload and install Git and Node.js LTS for Windows.</span></span>

- [<span data-ttu-id="79b8f-135">Získat Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="79b8f-135">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
- [<span data-ttu-id="79b8f-136">Získat Node.js LTS pro Windows</span><span class="sxs-lookup"><span data-stu-id="79b8f-136">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="79b8f-137">Instalace nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="79b8f-137">Install Node.js development tools</span></span>

<span data-ttu-id="79b8f-138">Používáte [gulp.js](http://gulpjs.com/) tooautomate nasazení a spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="79b8f-138">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="79b8f-139">Stiskněte klávesu `Windows + R`, typ `cmd` a stiskněte klávesu `Enter` tooopen okno příkazového řádku, a pak spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="79b8f-139">Press `Windows + R`, type `cmd` and press `Enter` tooopen a Command Prompt window, and then run hello following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="79b8f-140">Pokud máte problémy s instalací hello, najdete v části hello [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-troubleshooting.md) řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="79b8f-140">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="79b8f-141">Uzel, NPM a Gulp jsou skripty pro automatizaci požadované toorun vyvinuté v Node.js.</span><span class="sxs-lookup"><span data-stu-id="79b8f-141">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="79b8f-142">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="79b8f-142">Install Python</span></span>

<span data-ttu-id="79b8f-143">Můžete zvolit z Python 2.7, 3.4 nebo 3.5.</span><span class="sxs-lookup"><span data-stu-id="79b8f-143">You can choose from Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="79b8f-144">V tomto kurzu používáme Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="79b8f-144">In this tutorial, we use Python 2.7.</span></span> <span data-ttu-id="79b8f-145">Pokud jste již nainstalovali python, přejděte toohello další části.</span><span class="sxs-lookup"><span data-stu-id="79b8f-145">If you've already installed python, go toohello next section.</span></span>

[<span data-ttu-id="79b8f-146">Získat jazyk Python pro systém Windows</span><span class="sxs-lookup"><span data-stu-id="79b8f-146">Get Python for Windows</span></span>](https://www.python.org/downloads/)

<span data-ttu-id="79b8f-147">Musíte taky tooadd hello cesta hello složek, kdy jsou Python.exe a pip.exe systému nainstalovaná toohello `PATH` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="79b8f-147">You also need tooadd hello path of hello folders where Python.exe and pip.exe are installed toohello system `PATH` environment variable.</span></span> <span data-ttu-id="79b8f-148">Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="79b8f-148">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="79b8f-149">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="79b8f-149">Install hello Azure CLI</span></span>

<span data-ttu-id="79b8f-150">tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="79b8f-150">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="79b8f-151">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="79b8f-151">Open a Command Prompt window as an administrator.</span></span>

2. <span data-ttu-id="79b8f-152">Nainstalujte rozhraní příkazového řádku Azure hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="79b8f-152">Install hello Azure CLI by running hello following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="79b8f-153">Hello instalace může trvat 5 minut.</span><span class="sxs-lookup"><span data-stu-id="79b8f-153">hello installation might take 5 minutes.</span></span>

3. <span data-ttu-id="79b8f-154">Hello instalaci ověřte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="79b8f-154">Verify hello installation by running hello following command:</span></span>

   ```cmd
   az iot -h
   ```

   <span data-ttu-id="79b8f-155">Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="79b8f-155">You should see hello following output if hello installation is successful.</span></span>

   ![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="79b8f-157">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79b8f-157">Install Visual Studio Code</span></span>

<span data-ttu-id="79b8f-158">Visual Studio Code použijete později v hello kurz tooedit konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="79b8f-158">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="79b8f-159">[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="79b8f-159">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="79b8f-160">Souhrn</span><span class="sxs-lookup"><span data-stu-id="79b8f-160">Summary</span></span>

<span data-ttu-id="79b8f-161">Všechny požadované hello nástrojů a softwaru jste nainstalovali v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="79b8f-161">You've installed all hello required tools and software on your host computer.</span></span> <span data-ttu-id="79b8f-162">Svůj další úkol je toouse hello rozhraní příkazového řádku Azure toocreate služby IoT hub a registraci zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79b8f-162">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79b8f-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79b8f-163">Next steps</span></span>
[<span data-ttu-id="79b8f-164">Vytvoření služby IoT Hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="79b8f-164">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
