---
title: "Connect Intel Edison (C) tooAzure IoT - lekci 1: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Edison na Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git na ubuntu, ubuntu js uzlu instalace"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 4c7b8e04-b892-459b-8b03-85bcaff2465c
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1ebd599def4e8bb33d891517cc76bc2fcdc3c35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="a4922-104">Získat nástroje hello (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="a4922-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="a4922-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="a4922-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="a4922-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="a4922-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="a4922-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="a4922-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="a4922-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="a4922-108">What you will do</span></span>
<span data-ttu-id="a4922-109">Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro váš Edison Intel.</span><span class="sxs-lookup"><span data-stu-id="a4922-109">Download hello development tools and hello software for hello first sample application for your Intel Edison.</span></span> <span data-ttu-id="a4922-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="a4922-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="a4922-111">C sice hello programovací jazyk Hlavní logika hello Node.js nástroje se používají v hello lekce toobuild a nasazení ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4922-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toobuild and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a4922-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="a4922-112">What you will learn</span></span>
<span data-ttu-id="a4922-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="a4922-113">In this article, you will learn:</span></span>

* <span data-ttu-id="a4922-114">Jak tooinstall Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="a4922-114">How tooinstall Git and Node.js</span></span>
  * <span data-ttu-id="a4922-115">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="a4922-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="a4922-116">Hello ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="a4922-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="a4922-117">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="a4922-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="a4922-118">Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="a4922-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="a4922-119">minimální požadovaná verze Node.js Hello je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="a4922-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="a4922-120">[NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="a4922-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a4922-121">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="a4922-121">What you need</span></span>
<span data-ttu-id="a4922-122">toocomplete této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="a4922-122">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="a4922-123">Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.</span><span class="sxs-lookup"><span data-stu-id="a4922-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="a4922-124">Počítač, který používá Ubuntu 16.04 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a4922-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="a4922-125">Nainstalovat Git, Node.js a NPM</span><span class="sxs-lookup"><span data-stu-id="a4922-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="a4922-126">Použití hello klávesové zkratky `Ctrl + Alt + T` tooopen hello terminálu a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="a4922-126">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="a4922-127">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="a4922-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="a4922-128">Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooEdison.</span><span class="sxs-lookup"><span data-stu-id="a4922-128">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="a4922-129">Nainstalujte `gulp` tak, že spustíte následující příkaz v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="a4922-129">Install `gulp` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="a4922-130">Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="a4922-130">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="a4922-131">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4922-131">Install Visual Studio Code</span></span>
<span data-ttu-id="a4922-132">[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a4922-132">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="a4922-133">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="a4922-133">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="a4922-134">Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="a4922-134">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="a4922-135">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a4922-135">Summary</span></span>
<span data-ttu-id="a4922-136">Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4922-136">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="a4922-137">Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na Edison.</span><span class="sxs-lookup"><span data-stu-id="a4922-137">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4922-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a4922-138">Next steps</span></span>
<span data-ttu-id="a4922-139">[Vytvoření a nasazení ukázkové aplikace hello blikání][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="a4922-139">[Create and deploy hello blink sample application][create-and-deploy-the-blink-application]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
