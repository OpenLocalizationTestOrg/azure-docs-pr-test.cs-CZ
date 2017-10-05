---
title: "Connect Raspberry PI (C) k Azure IoT - lekci 1: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro platformy na Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot softwaru, pro internet věcí software, nainstalujte git na ubuntu, gulp spustit, nainstalujte ubuntu js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 32cfea00-c254-4cef-8f6f-bbf807eca6b6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 28ebba82e90d91470518cd830c96e6da39d8b9b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="f8628-104">Získání nástrojů (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="f8628-104">Get the tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f8628-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="f8628-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="f8628-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="f8628-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="f8628-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="f8628-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="f8628-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="f8628-108">What you will do</span></span>
<span data-ttu-id="f8628-109">Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro váš malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="f8628-109">Download the development tools and the software for the first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="f8628-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f8628-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f8628-111">I když programovací jazyk Hlavní logika je C, Node.js nástroje se používají v poznatky zjišťovat zařízení a sestavení a nasazení ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f8628-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to discover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f8628-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="f8628-112">What you will learn</span></span>
<span data-ttu-id="f8628-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="f8628-113">In this article, you will learn:</span></span>

* <span data-ttu-id="f8628-114">Jak nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="f8628-114">How to install Git and Node.js</span></span>
  * <span data-ttu-id="f8628-115">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="f8628-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="f8628-116">Ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="f8628-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="f8628-117">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="f8628-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="f8628-118">Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.</span><span class="sxs-lookup"><span data-stu-id="f8628-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="f8628-119">Minimální požadovaná verze Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="f8628-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="f8628-120">[NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="f8628-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f8628-121">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="f8628-121">What you need</span></span>
<span data-ttu-id="f8628-122">Pro dokončení této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="f8628-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="f8628-123">Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.</span><span class="sxs-lookup"><span data-stu-id="f8628-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="f8628-124">Počítač, který používá Ubuntu 16.04 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f8628-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="f8628-125">Nainstalovat Git, Node.js a NPM</span><span class="sxs-lookup"><span data-stu-id="f8628-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="f8628-126">Použijte klávesovou zkratku `Ctrl + Alt + T` otevřete terminál a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f8628-126">Use the keyboard shortcut `Ctrl + Alt + T` to open a terminal and run the following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="f8628-127">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="f8628-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="f8628-128">Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro platformy.</span><span class="sxs-lookup"><span data-stu-id="f8628-128">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="f8628-129">Použití [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) k načtení sítě informací o zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="f8628-129">Use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="f8628-130">Nainstalujte `gulp` a `device-discovery-cli` spuštěním následujícího příkazu v terminálu:</span><span class="sxs-lookup"><span data-stu-id="f8628-130">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="f8628-131">Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="f8628-131">If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="f8628-132">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8628-132">Install Visual Studio Code</span></span>
<span data-ttu-id="f8628-133">[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f8628-133">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="f8628-134">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="f8628-134">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="f8628-135">Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="f8628-135">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="f8628-136">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f8628-136">Summary</span></span>
<span data-ttu-id="f8628-137">Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f8628-137">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="f8628-138">Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na pí.</span><span class="sxs-lookup"><span data-stu-id="f8628-138">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8628-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8628-139">Next steps</span></span>
[<span data-ttu-id="f8628-140">Vytvoření a nasazení aplikace blink</span><span class="sxs-lookup"><span data-stu-id="f8628-140">Create and deploy the blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

