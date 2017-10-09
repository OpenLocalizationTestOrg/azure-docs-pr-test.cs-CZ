---
title: "Connect Raspberry PI (C) tooAzure IoT - lekci 1: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Stáhněte a nainstalujte na Ubuntu hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro platformy."
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
ms.openlocfilehash: 794928b5da63521cb0a72cb54256f2ad9724ec84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="6b395-104">Získat nástroje hello (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="6b395-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6b395-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="6b395-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="6b395-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="6b395-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="6b395-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="6b395-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="6b395-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="6b395-108">What you will do</span></span>
<span data-ttu-id="6b395-109">Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro váš malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="6b395-109">Download hello development tools and hello software for hello first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="6b395-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="6b395-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6b395-111">C sice hello programovací jazyk Hlavní logika hello Node.js nástroje se používají v hello lekce toodiscover zařízení a sestavení a nasazení ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b395-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toodiscover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6b395-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="6b395-112">What you will learn</span></span>
<span data-ttu-id="6b395-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="6b395-113">In this article, you will learn:</span></span>

* <span data-ttu-id="6b395-114">Jak tooinstall Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="6b395-114">How tooinstall Git and Node.js</span></span>
  * <span data-ttu-id="6b395-115">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="6b395-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="6b395-116">Hello ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="6b395-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="6b395-117">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="6b395-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="6b395-118">Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="6b395-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="6b395-119">minimální požadovaná verze Node.js Hello je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="6b395-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="6b395-120">[NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="6b395-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6b395-121">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="6b395-121">What you need</span></span>
<span data-ttu-id="6b395-122">toocomplete této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="6b395-122">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="6b395-123">Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.</span><span class="sxs-lookup"><span data-stu-id="6b395-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="6b395-124">Počítač, který používá Ubuntu 16.04 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6b395-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="6b395-125">Nainstalovat Git, Node.js a NPM</span><span class="sxs-lookup"><span data-stu-id="6b395-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="6b395-126">Použití hello klávesové zkratky `Ctrl + Alt + T` tooopen hello terminálu a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6b395-126">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="6b395-127">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="6b395-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="6b395-128">Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooPi.</span><span class="sxs-lookup"><span data-stu-id="6b395-128">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="6b395-129">Použití hello [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) tooretrieve sítě informace o zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="6b395-129">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="6b395-130">Nainstalujte `gulp` a `device-discovery-cli` tak, že spustíte následující příkaz v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="6b395-130">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="6b395-131">Pokud máte problémy instalace Node.js a tyto další vývojové nástroje na Ubuntu, přečtěte si téma hello [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="6b395-131">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="6b395-132">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6b395-132">Install Visual Studio Code</span></span>
<span data-ttu-id="6b395-133">[Stáhněte si](https://code.visualstudio.com/docs/setup/linux) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6b395-133">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="6b395-134">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="6b395-134">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="6b395-135">Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="6b395-135">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="6b395-136">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6b395-136">Summary</span></span>
<span data-ttu-id="6b395-137">Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6b395-137">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="6b395-138">Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na pí.</span><span class="sxs-lookup"><span data-stu-id="6b395-138">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b395-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b395-139">Next steps</span></span>
[<span data-ttu-id="6b395-140">Vytvoření a nasazení aplikace hello blikání</span><span class="sxs-lookup"><span data-stu-id="6b395-140">Create and deploy hello blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

