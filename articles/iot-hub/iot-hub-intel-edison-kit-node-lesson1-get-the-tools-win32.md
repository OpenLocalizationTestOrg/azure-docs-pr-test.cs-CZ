---
title: "Připojit Intel Edison (uzel) tooAzure IoT - lekci 1: získání nástroje (Windows) | Microsoft Docs"
description: "Stáhněte a nainstalujte hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Edison ve Windows 7 a novějších verzích."
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
ms.openlocfilehash: 933cc585d1b8b0236d76452f5c449ae9f2f3987b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="bcdf8-104">Získat nástroje hello (Windows 7 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="bcdf8-104">Get hello tools (Windows 7 or later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="bcdf8-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="bcdf8-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="bcdf8-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="bcdf8-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="bcdf8-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="bcdf8-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="bcdf8-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="bcdf8-108">What you will do</span></span>
<span data-ttu-id="bcdf8-109">Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-109">Download hello development tools and hello software for hello first sample application for Intel Edison.</span></span> <span data-ttu-id="bcdf8-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="bcdf8-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="bcdf8-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="bcdf8-111">What you will learn</span></span>
<span data-ttu-id="bcdf8-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="bcdf8-112">In this article, you will learn:</span></span>

* <span data-ttu-id="bcdf8-113">Jak tooinstall Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="bcdf8-114">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="bcdf8-115">Hello ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="bcdf8-116">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="bcdf8-117">Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="bcdf8-118">požadavky na minimální verzi Hello Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-118">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="bcdf8-119">[NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="bcdf8-120">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="bcdf8-120">What you need</span></span>

<span data-ttu-id="bcdf8-121">toocomplete této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="bcdf8-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="bcdf8-122">Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="bcdf8-123">Počítač, který je spuštěn systém Windows.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="bcdf8-124">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="bcdf8-124">Install Git and Node.js</span></span>

<span data-ttu-id="bcdf8-125">Klikněte na tlačítko hello najdete pod odkazy níže toodownload a nainstalujte Git a LTS Node.js pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-125">Click hello links below toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="bcdf8-126">Získat Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="bcdf8-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="bcdf8-127">Získat Node.js LTS pro Windows</span><span class="sxs-lookup"><span data-stu-id="bcdf8-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="bcdf8-128">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="bcdf8-128">Install additional Node.js development tools</span></span>

<span data-ttu-id="bcdf8-129">Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooEdison.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-129">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="bcdf8-130">Spusťte příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-130">Start a command prompt as an administrator.</span></span> <span data-ttu-id="bcdf8-131">Nainstalujte `gulp` spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bcdf8-131">Install `gulp` by running hello following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="bcdf8-132">Pokud máte problémy s instalací Node.js a tyto další nástroje pro vývoj Node.js do počítače, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-132">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="bcdf8-133">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bcdf8-133">Install Visual Studio Code</span></span>

<span data-ttu-id="bcdf8-134">[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-134">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="bcdf8-135">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-135">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="bcdf8-136">Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-136">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="bcdf8-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="bcdf8-137">Summary</span></span>

<span data-ttu-id="bcdf8-138">Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-138">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="bcdf8-139">Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na Edison.</span><span class="sxs-lookup"><span data-stu-id="bcdf8-139">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bcdf8-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bcdf8-140">Next steps</span></span>

<span data-ttu-id="bcdf8-141">[Vytvoření a nasazení aplikace hello blikání][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="bcdf8-141">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
