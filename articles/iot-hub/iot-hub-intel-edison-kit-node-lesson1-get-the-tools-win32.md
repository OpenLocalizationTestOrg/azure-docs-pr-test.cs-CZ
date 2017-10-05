---
title: "Připojení k Azure IoT - lekci 1 Intel Edison (uzel): získat nástroje (Windows) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro Edison ve Windows 7 a novějších verzích."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git v systému windows, instalace systému windows js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 4164b5a1-5a42-4d8a-9ff6-441e79fcc936
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 35b7ae24f8616848eaa6affccb96630603b823b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a><span data-ttu-id="81aee-104">Získat nástroje (Windows 7 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="81aee-104">Get the tools (Windows 7 or later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="81aee-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="81aee-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="81aee-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="81aee-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="81aee-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="81aee-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="81aee-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="81aee-108">What you will do</span></span>
<span data-ttu-id="81aee-109">Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="81aee-109">Download the development tools and the software for the first sample application for Intel Edison.</span></span> <span data-ttu-id="81aee-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="81aee-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="81aee-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="81aee-111">What you will learn</span></span>
<span data-ttu-id="81aee-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="81aee-112">In this article, you will learn:</span></span>

* <span data-ttu-id="81aee-113">Jak nainstalovat Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="81aee-113">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="81aee-114">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="81aee-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="81aee-115">Ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="81aee-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="81aee-116">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="81aee-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="81aee-117">Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.</span><span class="sxs-lookup"><span data-stu-id="81aee-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="81aee-118">Požadavek na minimální verzi Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="81aee-118">The minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="81aee-119">[NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="81aee-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="81aee-120">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="81aee-120">What you need</span></span>

<span data-ttu-id="81aee-121">Pro dokončení této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="81aee-121">To complete this operation, you will need:</span></span>

* <span data-ttu-id="81aee-122">Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.</span><span class="sxs-lookup"><span data-stu-id="81aee-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="81aee-123">Počítač, který je spuštěn systém Windows.</span><span class="sxs-lookup"><span data-stu-id="81aee-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="81aee-124">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="81aee-124">Install Git and Node.js</span></span>

<span data-ttu-id="81aee-125">Kliknutím na odkazy níže stáhnout a nainstalovat Git a LTS Node.js pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="81aee-125">Click the links below to download and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="81aee-126">Získat Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="81aee-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="81aee-127">Získat Node.js LTS pro Windows</span><span class="sxs-lookup"><span data-stu-id="81aee-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="81aee-128">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="81aee-128">Install additional Node.js development tools</span></span>

<span data-ttu-id="81aee-129">Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro Edison.</span><span class="sxs-lookup"><span data-stu-id="81aee-129">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Edison.</span></span>

<span data-ttu-id="81aee-130">Spusťte příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="81aee-130">Start a command prompt as an administrator.</span></span> <span data-ttu-id="81aee-131">Nainstalujte `gulp` spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="81aee-131">Install `gulp` by running the following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="81aee-132">Pokud máte problémy s instalací Node.js a tyto další nástroje pro vývoj Node.js do počítače, přečtěte si téma [Průvodce odstraňováním potíží s] [ troubleshooting] pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="81aee-132">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="81aee-133">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="81aee-133">Install Visual Studio Code</span></span>

<span data-ttu-id="81aee-134">[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="81aee-134">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="81aee-135">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="81aee-135">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="81aee-136">Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="81aee-136">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="81aee-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="81aee-137">Summary</span></span>

<span data-ttu-id="81aee-138">Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81aee-138">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="81aee-139">Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na Edison.</span><span class="sxs-lookup"><span data-stu-id="81aee-139">The next task is to create, deploy, and run the sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81aee-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81aee-140">Next steps</span></span>

<span data-ttu-id="81aee-141">[Vytvoření a nasazení aplikace blikání][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="81aee-141">[Create and deploy the blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
