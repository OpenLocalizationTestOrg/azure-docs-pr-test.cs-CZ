---
title: "Připojení k Azure IoT - lekci 2 malin platformy (uzel): získat nástroje (Windows) | Microsoft Docs"
description: "Ve Windows 7 a novějších verzích nainstalujte Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT cloudové služby, azure cli"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: acfa13e3-6d2c-4e68-9a77-1cbc2cf3f9c1
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c7b927997b738f7a80b71c06d4dff2278dc7c64e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="2c80b-104">Získat nástroje Azure (Windows 7 a novější)</span><span class="sxs-lookup"><span data-stu-id="2c80b-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c80b-105">Windows 7 a novější</span><span class="sxs-lookup"><span data-stu-id="2c80b-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="2c80b-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="2c80b-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="2c80b-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="2c80b-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="2c80b-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="2c80b-108">What you will do</span></span>
<span data-ttu-id="2c80b-109">Instalace jazyka Python a rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="2c80b-109">Install Python and the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="2c80b-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="2c80b-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="2c80b-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="2c80b-111">What you will learn</span></span>
<span data-ttu-id="2c80b-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="2c80b-112">In this article, you will learn:</span></span>
* <span data-ttu-id="2c80b-113">Postup instalace Python.</span><span class="sxs-lookup"><span data-stu-id="2c80b-113">How to install Python.</span></span>
* <span data-ttu-id="2c80b-114">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2c80b-114">How to install the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2c80b-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="2c80b-115">What you need</span></span>
* <span data-ttu-id="2c80b-116">Počítač Windows s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="2c80b-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="2c80b-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2c80b-117">An active Azure subscription.</span></span> <span data-ttu-id="2c80b-118">Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="2c80b-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="2c80b-119">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="2c80b-119">Install Python</span></span>
<span data-ttu-id="2c80b-120">[Instalaci Pythonu](https://www.python.org/downloads/) na počítači s Windows.</span><span class="sxs-lookup"><span data-stu-id="2c80b-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="2c80b-121">Můžete nainstalovat Python 2.7, 3.4 nebo 3.5.</span><span class="sxs-lookup"><span data-stu-id="2c80b-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="2c80b-122">V tomto kurzu vychází z Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="2c80b-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="2c80b-123">Pokud jste již nainstalovali Python, přejděte k další části a nainstalujte rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2c80b-123">If you've already installed Python, go to the next section and install the Azure CLI.</span></span>

<span data-ttu-id="2c80b-124">Musíte taky přidat cestu složky, kde jsou v systému nainstalovány python.exe a pip.exe `PATH` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c80b-124">You also need to add the path of the folders where python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="2c80b-125">Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="2c80b-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="2c80b-126">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2c80b-126">Install the Azure CLI</span></span>
<span data-ttu-id="2c80b-127">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2c80b-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="2c80b-128">Můžete pracovat přímo z vašeho příkazového řádku a zřizovat a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="2c80b-128">You work directly from your command-line to provision and manage resources.</span></span>

<span data-ttu-id="2c80b-129">Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2c80b-129">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="2c80b-130">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="2c80b-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="2c80b-131">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2c80b-131">Run the following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="2c80b-132">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2c80b-132">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="2c80b-133">Pokud je instalace úspěšná, zobrazí se následující výstup.</span><span class="sxs-lookup"><span data-stu-id="2c80b-133">You see the following output if the installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="2c80b-135">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2c80b-135">Summary</span></span>
<span data-ttu-id="2c80b-136">Instalaci rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2c80b-136">You've installed the Azure CLI.</span></span> <span data-ttu-id="2c80b-137">Svůj další úkol k vytvoření Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2c80b-137">Your next task to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c80b-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c80b-138">Next steps</span></span>
[<span data-ttu-id="2c80b-139">Vytvoření služby IoT hub a zaregistrujte malin pí 3</span><span class="sxs-lookup"><span data-stu-id="2c80b-139">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

