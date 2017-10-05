---
title: "Připojení k Azure IoT - lekci 1 Intel Edison (uzel): získat nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro Edison na Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git na ubuntu, ubuntu js uzlu instalace"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 9ab5b161-7ec5-41a6-9c5f-4456e4882752
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 74c5f06c2b12d140814bfb75125d60b83addf70c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="f285c-104">Získání nástrojů (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="f285c-104">Get the tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="f285c-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="f285c-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="f285c-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="f285c-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="f285c-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="f285c-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="f285c-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="f285c-108">What you will do</span></span>
<span data-ttu-id="f285c-109">Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro váš Edison Intel.</span><span class="sxs-lookup"><span data-stu-id="f285c-109">Download the development tools and the software for the first sample application for your Intel Edison.</span></span> <span data-ttu-id="f285c-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="f285c-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f285c-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="f285c-111">What you will learn</span></span>
<span data-ttu-id="f285c-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="f285c-112">In this article, you will learn:</span></span>

* <span data-ttu-id="f285c-113">Jak nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="f285c-113">How to install Git and Node.js</span></span>
  * <span data-ttu-id="f285c-114">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="f285c-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="f285c-115">Ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="f285c-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="f285c-116">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="f285c-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="f285c-117">Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.</span><span class="sxs-lookup"><span data-stu-id="f285c-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="f285c-118">Minimální požadovaná verze Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="f285c-118">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="f285c-119">[NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="f285c-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f285c-120">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="f285c-120">What you need</span></span>
<span data-ttu-id="f285c-121">Pro dokončení této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="f285c-121">To complete this operation, you will need:</span></span>
* <span data-ttu-id="f285c-122">Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.</span><span class="sxs-lookup"><span data-stu-id="f285c-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="f285c-123">Počítač, který používá Ubuntu 16.04 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f285c-123">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="f285c-124">Nainstalovat Git, Node.js a NPM</span><span class="sxs-lookup"><span data-stu-id="f285c-124">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="f285c-125">Použijte klávesovou zkratku `Ctrl + Alt + T` otevřete terminál a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f285c-125">Use the keyboard shortcut `Ctrl + Alt + T` to open a terminal and run the following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="f285c-126">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="f285c-126">Install additional Node.js development tools</span></span>
<span data-ttu-id="f285c-127">Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro Edison.</span><span class="sxs-lookup"><span data-stu-id="f285c-127">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Edison.</span></span>

<span data-ttu-id="f285c-128">Nainstalujte `gulp` spuštěním následujícího příkazu v terminálu:</span><span class="sxs-lookup"><span data-stu-id="f285c-128">Install `gulp` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="f285c-129">Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma [Průvodce odstraňováním potíží s] [ troubleshooting] pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="f285c-129">If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="f285c-130">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f285c-130">Install Visual Studio Code</span></span>
<span data-ttu-id="f285c-131">[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f285c-131">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="f285c-132">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="f285c-132">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="f285c-133">Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="f285c-133">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="f285c-134">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f285c-134">Summary</span></span>
<span data-ttu-id="f285c-135">Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f285c-135">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="f285c-136">Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na Edison.</span><span class="sxs-lookup"><span data-stu-id="f285c-136">The next task is to create, deploy, and run the sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f285c-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f285c-137">Next steps</span></span>
<span data-ttu-id="f285c-138">[Vytvoření a nasazení ukázkové aplikace blikání][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="f285c-138">[Create and deploy the blink sample application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
