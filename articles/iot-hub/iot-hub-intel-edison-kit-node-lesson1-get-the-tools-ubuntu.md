---
title: "Připojit Intel Edison (uzel) tooAzure IoT - lekci 1: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Edison na Ubuntu."
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
ms.openlocfilehash: ad1a48708bd74bcc07d09f105f597f18c3f9d2b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="0df59-104">Získat nástroje hello (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="0df59-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="0df59-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="0df59-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="0df59-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="0df59-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="0df59-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="0df59-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="0df59-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="0df59-108">What you will do</span></span>
<span data-ttu-id="0df59-109">Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro váš Edison Intel.</span><span class="sxs-lookup"><span data-stu-id="0df59-109">Download hello development tools and hello software for hello first sample application for your Intel Edison.</span></span> <span data-ttu-id="0df59-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="0df59-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="0df59-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="0df59-111">What you will learn</span></span>
<span data-ttu-id="0df59-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="0df59-112">In this article, you will learn:</span></span>

* <span data-ttu-id="0df59-113">Jak tooinstall Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="0df59-113">How tooinstall Git and Node.js</span></span>
  * <span data-ttu-id="0df59-114">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="0df59-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="0df59-115">Hello ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="0df59-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="0df59-116">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="0df59-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="0df59-117">Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="0df59-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="0df59-118">minimální požadovaná verze Node.js Hello je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="0df59-118">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="0df59-119">[NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="0df59-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0df59-120">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="0df59-120">What you need</span></span>
<span data-ttu-id="0df59-121">toocomplete této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="0df59-121">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="0df59-122">Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.</span><span class="sxs-lookup"><span data-stu-id="0df59-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="0df59-123">Počítač, který používá Ubuntu 16.04 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0df59-123">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="0df59-124">Nainstalovat Git, Node.js a NPM</span><span class="sxs-lookup"><span data-stu-id="0df59-124">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="0df59-125">Použití hello klávesové zkratky `Ctrl + Alt + T` tooopen hello terminálu a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0df59-125">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="0df59-126">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="0df59-126">Install additional Node.js development tools</span></span>
<span data-ttu-id="0df59-127">Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooEdison.</span><span class="sxs-lookup"><span data-stu-id="0df59-127">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="0df59-128">Nainstalujte `gulp` tak, že spustíte následující příkaz v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="0df59-128">Install `gulp` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="0df59-129">Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="0df59-129">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="0df59-130">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0df59-130">Install Visual Studio Code</span></span>
<span data-ttu-id="0df59-131">[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0df59-131">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="0df59-132">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="0df59-132">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="0df59-133">Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="0df59-133">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="0df59-134">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0df59-134">Summary</span></span>
<span data-ttu-id="0df59-135">Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0df59-135">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="0df59-136">Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na Edison.</span><span class="sxs-lookup"><span data-stu-id="0df59-136">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0df59-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0df59-137">Next steps</span></span>
<span data-ttu-id="0df59-138">[Vytvoření a nasazení ukázkové aplikace hello blikání][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="0df59-138">[Create and deploy hello blink sample application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
