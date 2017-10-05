---
title: "Connect Raspberry PI (C) k Azure IoT - lekci 1: získání nástroje (macOS) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro platformy v systému macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "vývoj pro IOT, iot software, internet věcí software, nainstalujte git v systému mac, gulp, spusťte instalaci uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fc6bd2c8-a847-4bf5-818f-6f7f9a6835ee
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 64db77040ef3482f213bd622320fdb5df243fa93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a><span data-ttu-id="f6940-104">Získání nástrojů (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="f6940-104">Get the tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f6940-105">Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="f6940-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="f6940-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="f6940-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="f6940-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="f6940-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="f6940-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="f6940-108">What you will do</span></span>
<span data-ttu-id="f6940-109">Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro váš malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="f6940-109">Download the development tools and the software for the first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="f6940-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f6940-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f6940-111">I když programovací jazyk Hlavní logika je C, Node.js nástroje se používají v poznatky zjišťovat zařízení a sestavení a nasazení ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f6940-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to discover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f6940-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="f6940-112">What you will learn</span></span>
<span data-ttu-id="f6940-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="f6940-113">In this article, you will learn:</span></span>

* <span data-ttu-id="f6940-114">Jak nainstalovat Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="f6940-114">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="f6940-115">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="f6940-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="f6940-116">Ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="f6940-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="f6940-117">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="f6940-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="f6940-118">Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.</span><span class="sxs-lookup"><span data-stu-id="f6940-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="f6940-119">Minimální požadovaná verze Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="f6940-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="f6940-120">[NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="f6940-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f6940-121">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="f6940-121">What you need</span></span>
<span data-ttu-id="f6940-122">Pro dokončení této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="f6940-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="f6940-123">Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.</span><span class="sxs-lookup"><span data-stu-id="f6940-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="f6940-124">Mac, který běží v systému macOS Yosemite (10.10) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f6940-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="f6940-125">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="f6940-125">Install Git and Node.js</span></span>
<span data-ttu-id="f6940-126">Chcete-li nainstalovat Git a Node.js, použijte [Homebrew](http://brew.sh) balíček nástroj pro správu pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="f6940-126">To install Git and Node.js, use the [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="f6940-127">Nainstalujte Homebrew.</span><span class="sxs-lookup"><span data-stu-id="f6940-127">Install Homebrew.</span></span> <span data-ttu-id="f6940-128">Pokud jste již nainstalovali Homebrew, přejděte ke kroku 2.</span><span class="sxs-lookup"><span data-stu-id="f6940-128">If you've already installed Homebrew, go to step 2.</span></span>
   
   1. <span data-ttu-id="f6940-129">Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` otevřete terminál.</span><span class="sxs-lookup"><span data-stu-id="f6940-129">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="f6940-130">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f6940-130">Run the following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="f6940-131">Nainstalujte Git a Node.js spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f6940-131">Install Git and Node.js by running the following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="f6940-132">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="f6940-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="f6940-133">Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro vaše platformy.</span><span class="sxs-lookup"><span data-stu-id="f6940-133">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to your Pi.</span></span> <span data-ttu-id="f6940-134">Použití [zařízení zjišťování-rozhraní příkazového řádku](https://github.com/Azure/device-discovery-cli) k načtení sítě informací o zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="f6940-134">Use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="f6940-135">Nainstalujte `gulp` a `device-discovery-cli` spuštěním následujícího příkazu v terminálu:</span><span class="sxs-lookup"><span data-stu-id="f6940-135">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="f6940-136">Pokud máte problémy instalace Node.js a tyto další vývoj nástrojů v systému macOS, přečtěte si téma [Průvodce odstraňováním potíží s](iot-hub-raspberry-pi-kit-c-troubleshooting.md) pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="f6940-136">If you experience issues installing Node.js and these additional development tools on macOS, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="f6940-137">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f6940-137">Install Visual Studio Code</span></span>
<span data-ttu-id="f6940-138">[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f6940-138">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="f6940-139">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="f6940-139">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="f6940-140">Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="f6940-140">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="f6940-141">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f6940-141">Summary</span></span>
<span data-ttu-id="f6940-142">Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f6940-142">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="f6940-143">Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na pí.</span><span class="sxs-lookup"><span data-stu-id="f6940-143">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6940-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6940-144">Next steps</span></span>
[<span data-ttu-id="f6940-145">Vytvoření a nasazení aplikace blink</span><span class="sxs-lookup"><span data-stu-id="f6940-145">Create and deploy the blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

