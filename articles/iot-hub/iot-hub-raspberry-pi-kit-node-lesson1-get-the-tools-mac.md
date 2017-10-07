---
title: "Připojit malin platformy (uzel) tooAzure IoT - lekci 1: získání nástroje (macOS) | Microsoft Docs"
description: "Stáhněte a nainstalujte v systému macOS hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro platformy."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vývoj pro IOT, iot softwaru, pro internet věcí software, nainstalujte python mac, nainstalovat git v systému mac, gulp spustit, nainstalujte uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2ea6d211-c0e8-4ade-ac69-d1c2261d78c4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 382b066cb7ece7ffdeb22b162b725727b22e5fac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="53250-104">Získat nástroje hello (systému macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="53250-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="53250-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="53250-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="53250-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="53250-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="53250-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="53250-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="53250-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="53250-108">What you will do</span></span>
<span data-ttu-id="53250-109">Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro váš malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="53250-109">Download hello development tools and hello software for hello first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="53250-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="53250-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="53250-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="53250-111">What you will learn</span></span>
<span data-ttu-id="53250-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="53250-112">In this article, you will learn:</span></span>

* <span data-ttu-id="53250-113">Jak tooinstall Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="53250-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="53250-114">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="53250-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="53250-115">Hello ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="53250-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="53250-116">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="53250-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="53250-117">Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="53250-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="53250-118">minimální požadovaná verze Node.js Hello je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="53250-118">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="53250-119">[NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="53250-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="53250-120">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="53250-120">What you need</span></span>
<span data-ttu-id="53250-121">toocomplete této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="53250-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="53250-122">Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.</span><span class="sxs-lookup"><span data-stu-id="53250-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="53250-123">Mac, který běží v systému macOS Yosemite (10.10) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="53250-123">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="53250-124">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="53250-124">Install Git and Node.js</span></span>
<span data-ttu-id="53250-125">tooinstall Git a Node.js, použijte hello [Homebrew](http://brew.sh) balíček nástroj pro správu pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="53250-125">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="53250-126">Nainstalujte Homebrew.</span><span class="sxs-lookup"><span data-stu-id="53250-126">Install Homebrew.</span></span> <span data-ttu-id="53250-127">Pokud jste již nainstalovali Homebrew, přejděte toostep 2.</span><span class="sxs-lookup"><span data-stu-id="53250-127">If you've already installed Homebrew, go toostep 2.</span></span>
   
   1. <span data-ttu-id="53250-128">Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` tooopen terminál.</span><span class="sxs-lookup"><span data-stu-id="53250-128">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="53250-129">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="53250-129">Run hello following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="53250-130">Nainstalujte Git a Node.js spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="53250-130">Install Git and Node.js by running hello following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="53250-131">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="53250-131">Install additional Node.js development tools</span></span>
<span data-ttu-id="53250-132">Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooPi.</span><span class="sxs-lookup"><span data-stu-id="53250-132">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="53250-133">Použití hello [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) tooretrieve sítě informace o zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="53250-133">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="53250-134">Nainstalujte `gulp` a `device-discovery-cli` tak, že spustíte následující příkaz v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="53250-134">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="53250-135">Pokud máte problémy instalace Node.js a tyto další vývoj nástrojů v systému macOS, přečtěte si téma hello [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-node-troubleshooting.md) řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="53250-135">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="53250-136">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="53250-136">Install Visual Studio Code</span></span>
<span data-ttu-id="53250-137">[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="53250-137">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="53250-138">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="53250-138">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="53250-139">Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="53250-139">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="53250-140">Souhrn</span><span class="sxs-lookup"><span data-stu-id="53250-140">Summary</span></span>
<span data-ttu-id="53250-141">Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="53250-141">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="53250-142">Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na pí.</span><span class="sxs-lookup"><span data-stu-id="53250-142">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53250-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53250-143">Next steps</span></span>
[<span data-ttu-id="53250-144">Vytvoření a nasazení ukázkové aplikace hello blikání</span><span class="sxs-lookup"><span data-stu-id="53250-144">Create and deploy hello blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

