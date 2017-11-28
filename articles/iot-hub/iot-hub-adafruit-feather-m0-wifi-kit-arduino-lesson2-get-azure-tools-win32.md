---
title: "Připojení k Azure IoT - lekci 2 Arduino: nástroje Azure (Windows) | Microsoft Docs"
description: "Ve Windows 7 a novějších verzích nainstalujte Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 70dfff14-4be1-468d-9919-e40f4bead308
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1f121d9f22f8a7c8582df4d2e62191cec364da46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="f67b1-104">Získat nástroje Azure (Windows 7 a novější)</span><span class="sxs-lookup"><span data-stu-id="f67b1-104">Get Azure tools (Windows 7 and later)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="f67b1-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="f67b1-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="f67b1-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="f67b1-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="f67b1-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="f67b1-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="f67b1-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="f67b1-108">What you will do</span></span>

<span data-ttu-id="f67b1-109">Instalace jazyka Python a rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="f67b1-109">Install Python and the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="f67b1-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) pro panel Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="f67b1-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f67b1-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="f67b1-111">What you will learn</span></span>
<span data-ttu-id="f67b1-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="f67b1-112">In this article, you will learn:</span></span>
* <span data-ttu-id="f67b1-113">Postup instalace Python.</span><span class="sxs-lookup"><span data-stu-id="f67b1-113">How to install Python.</span></span>
* <span data-ttu-id="f67b1-114">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f67b1-114">How to install the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f67b1-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="f67b1-115">What you need</span></span>
* <span data-ttu-id="f67b1-116">Počítač Windows s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="f67b1-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="f67b1-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="f67b1-117">An active Azure subscription.</span></span> <span data-ttu-id="f67b1-118">Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="f67b1-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="f67b1-119">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="f67b1-119">Install Python</span></span>
<span data-ttu-id="f67b1-120">[Instalaci Pythonu](https://www.python.org/downloads/) na počítači s Windows.</span><span class="sxs-lookup"><span data-stu-id="f67b1-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="f67b1-121">Můžete nainstalovat Python 2.7, 3.4 nebo 3.5.</span><span class="sxs-lookup"><span data-stu-id="f67b1-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="f67b1-122">V tomto kurzu vychází z Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="f67b1-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="f67b1-123">Pokud jste již nainstalovali Python, přejděte k další části a nainstalujte rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f67b1-123">If you've already installed Python, go to the next section and install the Azure CLI.</span></span>

<span data-ttu-id="f67b1-124">Musíte taky přidat cestu složky, kde jsou v systému nainstalovány python.exe a pip.exe `PATH` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="f67b1-124">You also need to add the path of the folders where python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="f67b1-125">Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="f67b1-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="f67b1-126">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f67b1-126">Install the Azure CLI</span></span>
<span data-ttu-id="f67b1-127">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f67b1-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="f67b1-128">Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f67b1-128">You work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="f67b1-129">Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f67b1-129">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="f67b1-130">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="f67b1-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="f67b1-131">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f67b1-131">Run the following commands:</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="f67b1-132">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f67b1-132">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="f67b1-133">Pokud je instalace úspěšná, zobrazí se následující výstup.</span><span class="sxs-lookup"><span data-stu-id="f67b1-133">You see the following output if the installation is successful.</span></span>

![Výstup, který označuje úspěch][output]

## <a name="summary"></a><span data-ttu-id="f67b1-135">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f67b1-135">Summary</span></span>
<span data-ttu-id="f67b1-136">Instalaci rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f67b1-136">You've installed the Azure CLI.</span></span> <span data-ttu-id="f67b1-137">Svůj další úkol k vytvoření Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f67b1-137">Your next task to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f67b1-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f67b1-138">Next steps</span></span>
<span data-ttu-id="f67b1-139">[Vytvoření služby IoT hub a zaregistrujte Arduino panel][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="f67b1-139">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_win.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md