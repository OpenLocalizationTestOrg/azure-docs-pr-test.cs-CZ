---
title: "Connect Intel Edison (C) tooAzure IoT - lekci 1: získání nástroje (Windows) | Microsoft Docs"
description: "Stáhněte a nainstalujte hello nezbytné nástroje a software pro hello první ukázkovou aplikaci pro Edison ve Windows 7 a novějších verzích."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Nástroje pro vývoj arduino, vývoj iot, iot software, internet věcí software, nainstalujte git v systému windows, instalace systému windows js uzlu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 7d29a358-544d-4657-a504-5ed9b79c2925
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 64d8684ffcb858845de02276a11cf2b2e5c701a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="d090e-104">Získat nástroje hello (Windows 7 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="d090e-104">Get hello tools (Windows 7 or later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="d090e-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="d090e-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="d090e-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="d090e-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="d090e-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="d090e-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="d090e-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="d090e-108">What you will do</span></span>
<span data-ttu-id="d090e-109">Stažení nástroje pro vývoj hello a hello softwaru pro hello první ukázkovou aplikaci pro Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="d090e-109">Download hello development tools and hello software for hello first sample application for Intel Edison.</span></span> <span data-ttu-id="d090e-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="d090e-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="d090e-111">C sice hello programovací jazyk Hlavní logika hello Node.js nástroje se používají v hello lekce toobuild a nasazení ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d090e-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toobuild and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="d090e-112">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="d090e-112">What you will learn</span></span>
<span data-ttu-id="d090e-113">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="d090e-113">In this article, you will learn:</span></span>

* <span data-ttu-id="d090e-114">Jak tooinstall Git a Node.js.</span><span class="sxs-lookup"><span data-stu-id="d090e-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="d090e-115">[Git](https://git-scm.com) je systém správy verzí distribuované s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="d090e-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="d090e-116">Hello ukázkovou aplikaci pro tento článek je uložený na Git.</span><span class="sxs-lookup"><span data-stu-id="d090e-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="d090e-117">[Node.js](https://nodejs.org/en/) je JavaScript runtime s ekosystém bohaté balíčku.</span><span class="sxs-lookup"><span data-stu-id="d090e-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="d090e-118">Jak toouse NPM tooinstall další Node.js nástroje pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="d090e-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="d090e-119">požadavky na minimální verzi Hello Node.js je 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="d090e-119">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="d090e-120">[NPM](https://www.npmjs.com) je jedním z hello správce balíčku pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="d090e-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="d090e-121">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="d090e-121">What you need</span></span>

<span data-ttu-id="d090e-122">toocomplete této operace, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="d090e-122">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="d090e-123">Toodownload připojení Internetu hello nástroje pro vývoj a hello softwaru.</span><span class="sxs-lookup"><span data-stu-id="d090e-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="d090e-124">Počítač, který je spuštěn systém Windows.</span><span class="sxs-lookup"><span data-stu-id="d090e-124">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="d090e-125">Nainstalovat Git a Node.js</span><span class="sxs-lookup"><span data-stu-id="d090e-125">Install Git and Node.js</span></span>

<span data-ttu-id="d090e-126">Klikněte na tlačítko hello najdete pod odkazy níže toodownload a nainstalujte Git a LTS Node.js pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="d090e-126">Click hello links below toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="d090e-127">Získat Git pro Windows</span><span class="sxs-lookup"><span data-stu-id="d090e-127">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="d090e-128">Získat Node.js LTS pro Windows</span><span class="sxs-lookup"><span data-stu-id="d090e-128">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="d090e-129">Instalace dalších nástrojů pro vývoj Node.js</span><span class="sxs-lookup"><span data-stu-id="d090e-129">Install additional Node.js development tools</span></span>

<span data-ttu-id="d090e-130">Použití [gulp.js](http://gulpjs.com) tooautomate hello nasazení hello ukázkové aplikace tooEdison.</span><span class="sxs-lookup"><span data-stu-id="d090e-130">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="d090e-131">Spusťte příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="d090e-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="d090e-132">Nainstalujte `gulp` spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d090e-132">Install `gulp` by running hello following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="d090e-133">Pokud máte problémy s instalací Node.js a tyto další nástroje pro vývoj Node.js do počítače, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting] řešení toocommon problémů.</span><span class="sxs-lookup"><span data-stu-id="d090e-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="d090e-134">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d090e-134">Install Visual Studio Code</span></span>

<span data-ttu-id="d090e-135">[Stáhněte si](https://code.visualstudio.com/docs/setup/windows) a nainstalujte Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d090e-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="d090e-136">Visual Studio Code je editor lightweight, ale výkonnou zdrojového kódu pro Windows, Linux a systému macOS.</span><span class="sxs-lookup"><span data-stu-id="d090e-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="d090e-137">Použití tohoto editoru později v hello kurz tooedit hello ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="d090e-137">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="d090e-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d090e-138">Summary</span></span>

<span data-ttu-id="d090e-139">Jste nainstalovali nástroje pro vývoj hello vyžaduje a software pro hello první ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d090e-139">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="d090e-140">Další úlohou Hello je toocreate, nasazení a spuštění ukázkové aplikace hello na Edison.</span><span class="sxs-lookup"><span data-stu-id="d090e-140">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d090e-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d090e-141">Next steps</span></span>

<span data-ttu-id="d090e-142">[Vytvoření a nasazení aplikace hello blikání][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="d090e-142">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
