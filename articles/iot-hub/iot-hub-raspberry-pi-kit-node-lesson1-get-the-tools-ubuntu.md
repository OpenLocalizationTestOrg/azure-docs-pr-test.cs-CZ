---
title: "Připojení k Azure IoT - lekci 1 malin platformy (uzel): získat nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro platformy na Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vývoj pro IOT, iot softwaru, pro internet věcí software, nainstalujte git na ubuntu, gulp spustit, nainstalujte ubuntu js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 4d5e45c0-1db9-4662-a039-99ba26333085
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: de583be0cdce058c83091f421376812e8013d76e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="56bb2-104">Získání nástrojů (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="56bb2-104">Get the tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="56bb2-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="56bb2-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="56bb2-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="56bb2-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="56bb2-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="56bb2-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="56bb2-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="56bb2-108">What you will do</span></span>
<span data-ttu-id="56bb2-109">Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="56bb2-109">Download the development tools and the software for the first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="56bb2-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="56bb2-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="56bb2-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="56bb2-111">What you will learn</span></span>
<span data-ttu-id="56bb2-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="56bb2-112">In this article, you will learn:</span></span>

* <span data-ttu-id="56bb2-113">Jak nainstalovat Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="56bb2-113">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="56bb2-114">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="56bb2-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="56bb2-115">Ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="56bb2-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="56bb2-116">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="56bb2-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="56bb2-117">Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.</span><span class="sxs-lookup"><span data-stu-id="56bb2-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="56bb2-118">Minimální požadovaná verze Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="56bb2-118">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="56bb2-119">[NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="56bb2-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="56bb2-120">Co je potřeba</span><span class="sxs-lookup"><span data-stu-id="56bb2-120">What do you need</span></span>
<span data-ttu-id="56bb2-121">Pro dokončení této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="56bb2-121">To complete this operation, you will need:</span></span>

* <span data-ttu-id="56bb2-122">Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.</span><span class="sxs-lookup"><span data-stu-id="56bb2-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="56bb2-123">Počítač, který používá Ubuntu 16.04 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="56bb2-123">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="56bb2-124">Nainstalovat Git, Node.js a NPM</span><span class="sxs-lookup"><span data-stu-id="56bb2-124">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="56bb2-125">Použijte klávesovou zkratku `Ctrl + Alt + T` otevřete terminál a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="56bb2-125">Use the keyboard shortcut `Ctrl + Alt + T` to open a terminal and run the following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="56bb2-126">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="56bb2-126">Install additional Node.js development tools</span></span>
<span data-ttu-id="56bb2-127">Používáte [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro platformy.</span><span class="sxs-lookup"><span data-stu-id="56bb2-127">You use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="56bb2-128">Můžete také použít [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) k načtení sítě informací o zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="56bb2-128">You also use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="56bb2-129">Nainstalujte `gulp` a `device-discovery-cli` spuštěním následujícího příkazu v terminálu:</span><span class="sxs-lookup"><span data-stu-id="56bb2-129">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="56bb2-130">Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-node-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="56bb2-130">If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="56bb2-131">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="56bb2-131">Install Visual Studio Code</span></span>
<span data-ttu-id="56bb2-132">[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="56bb2-132">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="56bb2-133">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="56bb2-133">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="56bb2-134">Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="56bb2-134">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="56bb2-135">Souhrn</span><span class="sxs-lookup"><span data-stu-id="56bb2-135">Summary</span></span>
<span data-ttu-id="56bb2-136">Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56bb2-136">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="56bb2-137">Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na pí.</span><span class="sxs-lookup"><span data-stu-id="56bb2-137">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="56bb2-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56bb2-138">Next steps</span></span>
[<span data-ttu-id="56bb2-139">Vytvoření a nasazení ukázkové aplikace blikání</span><span class="sxs-lookup"><span data-stu-id="56bb2-139">Create and deploy the blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

