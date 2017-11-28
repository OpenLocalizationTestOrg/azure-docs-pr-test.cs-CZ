---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 2: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Nainstalovat nástroje hello a hello software na hostiteli počítače se spuštěným systémem Ubuntu, vytvoření služby IoT hub a registraci zařízení ve hello IoT hub."
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
ms.openlocfilehash: c9edca91e791ef914b1920178b66eadd12ae0281
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="b95ec-104">Získat nástroje hello (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="b95ec-104">Get hello tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b95ec-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="b95ec-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="b95ec-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="b95ec-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="b95ec-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="b95ec-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="b95ec-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="b95ec-108">What you will do</span></span>

- <span data-ttu-id="b95ec-109">Nainstalujte Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="b95ec-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="b95ec-110">Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="b95ec-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="b95ec-111">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="b95ec-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>
## <a name="what-you-will-learn"></a><span data-ttu-id="b95ec-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="b95ec-112">What you will learn</span></span>

<span data-ttu-id="b95ec-113">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="b95ec-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="b95ec-114">Jak tooinstall Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="b95ec-114">How tooinstall Git and Node.js.</span></span>
  - <span data-ttu-id="b95ec-115">Git je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="b95ec-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="b95ec-116">Ukázková aplikace Hello této lekce jsou uloženy na Git.</span><span class="sxs-lookup"><span data-stu-id="b95ec-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="b95ec-117">Platforma Node.js je prostředí JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="b95ec-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="b95ec-118">Jak toouse NPM tooinstall Node.js nástroje pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="b95ec-118">How toouse NPM tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="b95ec-119">minimální požadovaná verze Node.js Hello je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="b95ec-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="b95ec-120">NPM je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="b95ec-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="b95ec-121">Jak tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b95ec-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="b95ec-122">Visual Studio Code je křížové platformy, editoru lightweight, ale výkonnou zdrojového kódu pro systém Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="b95ec-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="b95ec-123">Má podpory pro ladění, vloženému ovládacímu prvku Git, zvýraznění syntaxe, inteligentního doplňování kódu, fragmenty a také refaktoring kódu.</span><span class="sxs-lookup"><span data-stu-id="b95ec-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="b95ec-124">Jak tooinstall hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b95ec-124">How tooinstall hello Azure CLI</span></span>
  - <span data-ttu-id="b95ec-125">Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b95ec-125">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="b95ec-126">Práce přímo z příkazového řádku tooprovision a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="b95ec-126">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="b95ec-127">Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="b95ec-127">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b95ec-128">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="b95ec-128">What you need</span></span>

- <span data-ttu-id="b95ec-129">Toodownload připojení Internetu hello nástrojů a softwaru.</span><span class="sxs-lookup"><span data-stu-id="b95ec-129">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="b95ec-130">Počítač, který používá Ubuntu 16.04 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b95ec-130">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="b95ec-131">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="b95ec-131">Install Git and Node.js</span></span>

<span data-ttu-id="b95ec-132">tooinstall Git a Node.js, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b95ec-132">tooinstall Git and Node.js, follow these steps:</span></span>

1. <span data-ttu-id="b95ec-133">Stiskněte klávesu `Ctrl + Alt + T` tooopen terminál.</span><span class="sxs-lookup"><span data-stu-id="b95ec-133">Press `Ctrl + Alt + T` tooopen a terminal.</span></span>
2. <span data-ttu-id="b95ec-134">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="b95ec-134">Run hello following commands:</span></span>

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="b95ec-135">Instalace nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="b95ec-135">Install Node.js development tools</span></span>

<span data-ttu-id="b95ec-136">Používáte [gulp.js](http://gulpjs.com/) tooautomate nasazení a spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="b95ec-136">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="b95ec-137">gulp tooinstall, spusťte následující příkaz v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="b95ec-137">tooinstall gulp, run hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="b95ec-138">Pokud máte problémy s instalací hello, najdete v části hello [Průvodce odstraňováním potíží s](iot-hub-gateway-kit-c-troubleshooting.md) řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="b95ec-138">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="b95ec-139">Uzel, NPM a Gulp jsou skripty pro automatizaci požadované toorun vyvinuté v Node.js.</span><span class="sxs-lookup"><span data-stu-id="b95ec-139">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="b95ec-140">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b95ec-140">Install hello Azure CLI</span></span>

<span data-ttu-id="b95ec-141">tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b95ec-141">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="b95ec-142">Spusťte následující příkazy v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="b95ec-142">Run hello following commands in hello terminal:</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="b95ec-143">Hello instalace může trvat 5 minut.</span><span class="sxs-lookup"><span data-stu-id="b95ec-143">hello installation might take 5 minutes.</span></span>

2. <span data-ttu-id="b95ec-144">Hello instalaci ověřte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b95ec-144">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```
<span data-ttu-id="b95ec-145">Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="b95ec-145">You should see hello following output if hello installation is successful.</span></span>
<span data-ttu-id="b95ec-146">![Ověření instalace rozhraní příkazového řádku Azure](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="b95ec-146">![Verify Azure CLI installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span></span>

### <a name="install-visual-studio-code"></a><span data-ttu-id="b95ec-147">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b95ec-147">Install Visual Studio Code</span></span>

<span data-ttu-id="b95ec-148">Visual Studio Code použijete později v hello kurz tooedit konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="b95ec-148">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="b95ec-149">[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b95ec-149">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="b95ec-150">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b95ec-150">Summary</span></span>

<span data-ttu-id="b95ec-151">Všechny požadované hello nástrojů a softwaru jste nainstalovali v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="b95ec-151">You've installed all hello required tools and software on your host computer.</span></span> <span data-ttu-id="b95ec-152">Svůj další úkol je toouse hello rozhraní příkazového řádku Azure toocreate služby IoT hub a registraci zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="b95ec-152">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b95ec-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b95ec-153">Next steps</span></span>
[<span data-ttu-id="b95ec-154">Vytvoření služby IoT Hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="b95ec-154">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
