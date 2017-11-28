---
title: "Připojit Arduino tooAzure IoT - lekci 1: získání nástroje (macOS) | Microsoft Docs"
description: "Stáhněte a nainstalujte v systému macOS hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Adafruit prolnutí M0 Wi-Fi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git v systému mac, instalace uzlu js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 0262f3dd-0259-4eb0-962f-9fb534f8359e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5fd306fecd7259fb8f1e99d76282a1e464c4d4f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="cac7e-104">Získat nástroje hello (systému macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="cac7e-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="cac7e-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="cac7e-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="cac7e-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="cac7e-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="cac7e-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="cac7e-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="cac7e-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="cac7e-108">What you will do</span></span>

<span data-ttu-id="cac7e-109">Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro panel Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="cac7e-109">Download hello development tools and hello software for hello first sample application for your Adafruit Feather M0 WiFi Arduino board.</span></span> 

<span data-ttu-id="cac7e-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="cac7e-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="cac7e-111">Arduino sice hello programovací jazyk Hlavní logika hello Node.js nástroje se používají v hello lekce toobuild a nasazení ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cac7e-111">Although hello programming language of hello main logic is Arduino, Node.js tools are used in hello lessons toobuild and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="cac7e-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="cac7e-112">What you will learn</span></span>
<span data-ttu-id="cac7e-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="cac7e-113">In this article, you will learn:</span></span>

* <span data-ttu-id="cac7e-114">Jak tooinstall Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="cac7e-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="cac7e-115">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="cac7e-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="cac7e-116">Hello ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="cac7e-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="cac7e-117">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="cac7e-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="cac7e-118">Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="cac7e-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="cac7e-119">minimální požadovaná verze Node.js Hello je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="cac7e-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="cac7e-120">[NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="cac7e-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="cac7e-121">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="cac7e-121">What you need</span></span>
<span data-ttu-id="cac7e-122">toocomplete této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="cac7e-122">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="cac7e-123">Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.</span><span class="sxs-lookup"><span data-stu-id="cac7e-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="cac7e-124">Mac, který běží v systému macOS Yosemite (10.10) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="cac7e-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="cac7e-125">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="cac7e-125">Install Git and Node.js</span></span>
<span data-ttu-id="cac7e-126">tooinstall Git a Node.js, použijte hello [Homebrew](http://brew.sh) balíček nástroj pro správu pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="cac7e-126">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="cac7e-127">Nainstalujte Homebrew.</span><span class="sxs-lookup"><span data-stu-id="cac7e-127">Install Homebrew.</span></span> <span data-ttu-id="cac7e-128">Pokud jste již nainstalovali Homebrew, přejděte toostep 2.</span><span class="sxs-lookup"><span data-stu-id="cac7e-128">If you've already installed Homebrew, go toostep 2.</span></span>

   1. <span data-ttu-id="cac7e-129">Stiskněte klávesu `Cmd + Space` a zadejte `Terminal` tooopen terminál.</span><span class="sxs-lookup"><span data-stu-id="cac7e-129">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="cac7e-130">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="cac7e-130">Run hello following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="cac7e-131">Nainstalujte Git a Node.js spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cac7e-131">Install Git and Node.js by running hello following command:</span></span>

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="cac7e-132">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="cac7e-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="cac7e-133">Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooyour Arduino panelu.</span><span class="sxs-lookup"><span data-stu-id="cac7e-133">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooyour Arduino board.</span></span>

<span data-ttu-id="cac7e-134">Nainstalujte `gulp`, `device-discovery-cli` tak, že spustíte následující příkaz v terminálu hello hello:</span><span class="sxs-lookup"><span data-stu-id="cac7e-134">Install `gulp`, `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp device-discovery-cli
```

<span data-ttu-id="cac7e-135">Pokud máte problémy instalace Node.js a tyto další vývoj nástrojů v systému macOS, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="cac7e-135">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="cac7e-136">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cac7e-136">Install Visual Studio Code</span></span>
<span data-ttu-id="cac7e-137">[Stáhněte si](https://code.visualstudio.com/docs/setup/osx) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="cac7e-137">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="cac7e-138">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="cac7e-138">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="cac7e-139">Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="cac7e-139">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="cac7e-140">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cac7e-140">Summary</span></span>
<span data-ttu-id="cac7e-141">Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cac7e-141">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="cac7e-142">Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na panel Arduino.</span><span class="sxs-lookup"><span data-stu-id="cac7e-142">hello next task is toocreate, deploy, and run hello sample application on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cac7e-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cac7e-143">Next steps</span></span>
<span data-ttu-id="cac7e-144">[Vytvoření a nasazení aplikace hello blikání][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="cac7e-144">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md