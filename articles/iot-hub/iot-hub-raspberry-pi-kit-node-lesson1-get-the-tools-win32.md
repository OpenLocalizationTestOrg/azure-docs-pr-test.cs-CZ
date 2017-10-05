---
title: "Připojení k Azure IoT - lekci 1 malin platformy (uzel): získat nástroje (Windows) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro platformy Windows 7 a novější verze."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vývoj pro IOT, iot softwaru, internet věcí softwaru, nainstalovat git v systému windows, gulp spustit, uzel js windows nainstalovat, nainstalujte npm v systému windows, nainstalujte python v systému windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b3d88e17-97cc-4f23-85fd-a688fc228eb8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 24c58e006bbef9bbc1fcd626a0f8f6bcac063f05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a><span data-ttu-id="fa1c8-104">Získat nástroje (Windows 7 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="fa1c8-104">Get the tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="fa1c8-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="fa1c8-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="fa1c8-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="fa1c8-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="fa1c8-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="fa1c8-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="fa1c8-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="fa1c8-108">What you will do</span></span>
<span data-ttu-id="fa1c8-109">Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-109">Download the development tools and the software for the first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="fa1c8-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="fa1c8-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="fa1c8-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="fa1c8-111">What you will learn</span></span>
<span data-ttu-id="fa1c8-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="fa1c8-112">In this article, you will learn:</span></span>

* <span data-ttu-id="fa1c8-113">Jak nainstalovat Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-113">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="fa1c8-114">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="fa1c8-115">Ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="fa1c8-116">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="fa1c8-117">Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="fa1c8-118">Požadavek na minimální verzi Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-118">The minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="fa1c8-119">[NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="fa1c8-120">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="fa1c8-120">What you need</span></span>
<span data-ttu-id="fa1c8-121">Pro dokončení této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="fa1c8-121">To complete this operation, you will need:</span></span>

* <span data-ttu-id="fa1c8-122">Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="fa1c8-123">Počítač, který je spuštěn systém Windows.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="fa1c8-124">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="fa1c8-124">Install Git and Node.js</span></span>
<span data-ttu-id="fa1c8-125">Kliknutím na následující odkazy stáhnout a nainstalovat Git a LTS Node.js pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-125">Click the following links to download and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="fa1c8-126">Získat Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="fa1c8-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="fa1c8-127">Získat Node.js LTS pro Windows</span><span class="sxs-lookup"><span data-stu-id="fa1c8-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="fa1c8-128">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="fa1c8-128">Install additional Node.js development tools</span></span>
<span data-ttu-id="fa1c8-129">Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro platformy.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-129">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="fa1c8-130">Můžete také použít [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) k načtení sítě informací o zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-130">You also use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="fa1c8-131">Spusťte příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="fa1c8-132">Nainstalujte `gulp` a `device-discovery-cli` tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fa1c8-132">Install `gulp` and `device-discovery-cli` by running the following command:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="fa1c8-133">Pokud máte problémy s instalací Node.js a tyto další nástroje pro vývoj Node.js do počítače, přečtěte si téma [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-node-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="fa1c8-134">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fa1c8-134">Install Visual Studio Code</span></span>
<span data-ttu-id="fa1c8-135">[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="fa1c8-136">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="fa1c8-137">Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-137">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="fa1c8-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="fa1c8-138">Summary</span></span>
<span data-ttu-id="fa1c8-139">Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-139">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="fa1c8-140">Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na pí.</span><span class="sxs-lookup"><span data-stu-id="fa1c8-140">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa1c8-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa1c8-141">Next steps</span></span>
[<span data-ttu-id="fa1c8-142">Vytvoření a nasazení ukázkové aplikace blikání</span><span class="sxs-lookup"><span data-stu-id="fa1c8-142">Create and deploy the blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

