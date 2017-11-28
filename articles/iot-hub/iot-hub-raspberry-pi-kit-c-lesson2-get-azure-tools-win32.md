---
title: "Connect Raspberry PI (C) tooAzure IoT - lekci 2: nástroje Azure (Windows) | Microsoft Docs"
description: "Ve Windows 7 a novějších verzích nainstalujte Python a hello rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT cloudové služby, azure cli"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a3c083b5-0d76-46af-bc77-2ad7d8aadc1e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1819d61fafbee6ac42a1bea5c16437cd8bf43af9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="48cb9-104">Získat nástroje Azure (Windows 7 a novější)</span><span class="sxs-lookup"><span data-stu-id="48cb9-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="48cb9-105">Windows 7 a novější</span><span class="sxs-lookup"><span data-stu-id="48cb9-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="48cb9-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="48cb9-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="48cb9-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="48cb9-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="48cb9-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="48cb9-108">What you will do</span></span>
<span data-ttu-id="48cb9-109">Nainstalujte Python a hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="48cb9-109">Install Python and hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="48cb9-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="48cb9-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="48cb9-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="48cb9-111">What you will learn</span></span>
<span data-ttu-id="48cb9-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="48cb9-112">In this article, you will learn:</span></span>
* <span data-ttu-id="48cb9-113">Jak tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="48cb9-113">How tooinstall Python.</span></span>
* <span data-ttu-id="48cb9-114">Jak tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="48cb9-114">How tooinstall hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="48cb9-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="48cb9-115">What you need</span></span>
* <span data-ttu-id="48cb9-116">Počítač Windows s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="48cb9-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="48cb9-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="48cb9-117">An active Azure subscription.</span></span> <span data-ttu-id="48cb9-118">Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="48cb9-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="48cb9-119">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="48cb9-119">Install Python</span></span>
<span data-ttu-id="48cb9-120">[Instalaci Pythonu](https://www.python.org/downloads/) na počítači s Windows.</span><span class="sxs-lookup"><span data-stu-id="48cb9-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="48cb9-121">Můžete nainstalovat Python 2.7, 3.4 nebo 3.5.</span><span class="sxs-lookup"><span data-stu-id="48cb9-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="48cb9-122">V tomto kurzu vychází z Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="48cb9-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="48cb9-123">Pokud jste již nainstalovali Python, přejděte toohello další části a nainstalujte hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="48cb9-123">If you've already installed Python, go toohello next section and install hello Azure CLI.</span></span>

<span data-ttu-id="48cb9-124">Musíte taky tooadd hello cesta hello složek, kdy jsou python.exe a pip.exe systému nainstalovaná toohello `PATH` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="48cb9-124">You also need tooadd hello path of hello folders where python.exe and pip.exe are installed toohello system `PATH` environment variable.</span></span> <span data-ttu-id="48cb9-125">Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="48cb9-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="48cb9-126">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="48cb9-126">Install hello Azure CLI</span></span>
<span data-ttu-id="48cb9-127">Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="48cb9-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="48cb9-128">Práce přímo z vašeho tooprovision příkazového řádku a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="48cb9-128">You work directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="48cb9-129">tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="48cb9-129">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="48cb9-130">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="48cb9-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="48cb9-131">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="48cb9-131">Run hello following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="48cb9-132">Hello instalaci ověřte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="48cb9-132">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="48cb9-133">Uvidíte, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="48cb9-133">You see hello following output if hello installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="48cb9-135">Souhrn</span><span class="sxs-lookup"><span data-stu-id="48cb9-135">Summary</span></span>
<span data-ttu-id="48cb9-136">Jste nainstalovali hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="48cb9-136">You've installed hello Azure CLI.</span></span> <span data-ttu-id="48cb9-137">Vaše další úloh toocreate Azure IoT hub a zařízení identity pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="48cb9-137">Your next task toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48cb9-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48cb9-138">Next steps</span></span>
[<span data-ttu-id="48cb9-139">Vytvoření služby IoT hub a zaregistrujte malin pí 3</span><span class="sxs-lookup"><span data-stu-id="48cb9-139">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

