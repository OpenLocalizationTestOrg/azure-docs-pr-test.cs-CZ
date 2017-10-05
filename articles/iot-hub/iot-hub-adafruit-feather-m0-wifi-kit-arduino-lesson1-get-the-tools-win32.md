---
title: "Připojení k Azure IoT - lekci 1 Arduino: získat nástroje (Windows) | Microsoft Docs"
description: "Stáhněte a nainstalujte nezbytné nástroje a software pro první ukázkovou aplikaci pro Adafruit prolnutí M0 Wi-Fi ve Windows 7 a novějších verzích."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git v systému windows, instalace systému windows js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9cfb8cd2-eafb-4ba2-b23e-d94e114ff3a6
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5d27c016c4a74e31455e676b3c3070a8e262b21f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a><span data-ttu-id="cb358-104">Získat nástroje (Windows 7 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="cb358-104">Get the tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="cb358-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="cb358-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="cb358-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="cb358-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="cb358-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="cb358-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="cb358-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="cb358-108">What you will do</span></span>

<span data-ttu-id="cb358-109">Stažení nástroje pro vývoj a software pro první ukázkovou aplikaci pro panel Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="cb358-109">Download the development tools and the software for the first sample application for your Adafruit Feather M0 WiFi Arduino board.</span></span>

<span data-ttu-id="cb358-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="cb358-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="cb358-111">I když programovací jazyk Hlavní logika je Arduino, Node.js nástroje se používají v poznatky sestavení a nasazení ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb358-111">Although the programming language of the main logic is Arduino, Node.js tools are used in the lessons to build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="cb358-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="cb358-112">What you will learn</span></span>
<span data-ttu-id="cb358-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="cb358-113">In this article, you will learn:</span></span>

* <span data-ttu-id="cb358-114">Jak nainstalovat Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="cb358-114">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="cb358-115">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="cb358-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="cb358-116">Ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="cb358-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="cb358-117">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="cb358-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="cb358-118">Postup instalace dalších nástrojů pro vývoj Node.js pomocí NPM.</span><span class="sxs-lookup"><span data-stu-id="cb358-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="cb358-119">Požadavek na minimální verzi Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="cb358-119">The minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="cb358-120">[NPM](https://www.npmjs.com) je jedním z vybraných manažerů balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="cb358-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="cb358-121">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="cb358-121">What you need</span></span>

<span data-ttu-id="cb358-122">Pro dokončení této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="cb358-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="cb358-123">Připojení k Internetu kvůli stahování nástroje pro vývoj a softwaru.</span><span class="sxs-lookup"><span data-stu-id="cb358-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="cb358-124">Počítač, který je spuštěn systém Windows.</span><span class="sxs-lookup"><span data-stu-id="cb358-124">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="cb358-125">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="cb358-125">Install Git and Node.js</span></span>

<span data-ttu-id="cb358-126">Kliknutím na odkazy níže stáhnout a nainstalovat Git a LTS Node.js pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="cb358-126">Click the links below to download and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="cb358-127">Získat Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="cb358-127">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="cb358-128">Získat Node.js LTS pro Windows</span><span class="sxs-lookup"><span data-stu-id="cb358-128">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="cb358-129">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="cb358-129">Install additional Node.js development tools</span></span>

<span data-ttu-id="cb358-130">Použití [gulp.js](http://gulpjs.com) k automatizaci nasazení ukázkové aplikace pro panel Arduino.</span><span class="sxs-lookup"><span data-stu-id="cb358-130">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to your Arduino board.</span></span>

<span data-ttu-id="cb358-131">Spusťte příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="cb358-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="cb358-132">Nainstalujte `gulp`, `device-discovery-cli` spuštěním následujícího příkazu v terminálu:</span><span class="sxs-lookup"><span data-stu-id="cb358-132">Install `gulp`, `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
npm install -g gulp device-discovery-cli
```

<span data-ttu-id="cb358-133">Pokud máte problémy s instalací Node.js a tyto další nástroje pro vývoj Node.js do počítače, přečtěte si téma [Průvodce odstraňováním potíží s] [ troubleshooting] pro řešení běžných potíží.</span><span class="sxs-lookup"><span data-stu-id="cb358-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="cb358-134">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cb358-134">Install Visual Studio Code</span></span>

<span data-ttu-id="cb358-135">[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="cb358-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="cb358-136">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="cb358-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="cb358-137">Použití tohoto editoru později v tomto kurzu upravte ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="cb358-137">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="cb358-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cb358-138">Summary</span></span>

<span data-ttu-id="cb358-139">Jste nainstalovali nástroje pro vývoj vyžaduje a software pro první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb358-139">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="cb358-140">Dalším krokem je vytvoření, nasazení a spuštění ukázkové aplikace na panel Arduino.</span><span class="sxs-lookup"><span data-stu-id="cb358-140">The next task is to create, deploy, and run the sample application on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb358-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb358-141">Next steps</span></span>

<span data-ttu-id="cb358-142">[Vytvoření a nasazení ukázkové aplikace blikání][create-and-deploy-the-blink-sample-application]</span><span class="sxs-lookup"><span data-stu-id="cb358-142">[Create and deploy the blink sample application][create-and-deploy-the-blink-sample-application]</span></span>
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-sample-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md