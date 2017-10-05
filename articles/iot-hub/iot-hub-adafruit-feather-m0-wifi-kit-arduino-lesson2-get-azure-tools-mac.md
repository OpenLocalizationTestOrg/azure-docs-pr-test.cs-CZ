---
title: "Připojení k Azure IoT - lekci 2 Arduino: nástroje Azure (macOS) | Microsoft Docs"
description: "Instalace v systému macOS Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9b719293-01d2-4a2d-9c49-476e67f4816d
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 037f8e3dba542773185fde43a345d5fd487b2d84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="e71bb-104">Získat nástroje Azure (systému macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="e71bb-104">Get Azure tools (macOS 10.10)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="e71bb-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="e71bb-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="e71bb-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="e71bb-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="e71bb-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="e71bb-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e71bb-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="e71bb-108">What you will do</span></span>

<span data-ttu-id="e71bb-109">Nainstalujte rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="e71bb-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="e71bb-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) pro panel Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="e71bb-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e71bb-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e71bb-111">What you will learn</span></span>
<span data-ttu-id="e71bb-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="e71bb-112">In this article, you will learn:</span></span>
* <span data-ttu-id="e71bb-113">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e71bb-113">How to install Azure CLI.</span></span>
* <span data-ttu-id="e71bb-114">Postup přidání podskupině IoT rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e71bb-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e71bb-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="e71bb-115">What you need</span></span>
* <span data-ttu-id="e71bb-116">Mac s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="e71bb-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="e71bb-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e71bb-117">An active Azure subscription.</span></span> <span data-ttu-id="e71bb-118">Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="e71bb-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="e71bb-119">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="e71bb-119">Install Python</span></span>
<span data-ttu-id="e71bb-120">I když systému macOS dodává s Python 2.7 z pole, doporučujeme nainstalovat Python prostřednictvím Homebrew.</span><span class="sxs-lookup"><span data-stu-id="e71bb-120">Although macOS comes with Python 2.7 out of the box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="e71bb-121">V tématu [Python instalace v systému macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="e71bb-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="e71bb-122">Instalace Python a pip spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="e71bb-122">Install Python and pip by running the following command:</span></span>

```bash
brew install python
```

## <a name="install-the-azure-cli"></a><span data-ttu-id="e71bb-123">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e71bb-123">Install the Azure CLI</span></span>
<span data-ttu-id="e71bb-124">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e71bb-124">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="e71bb-125">Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e71bb-125">You work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="e71bb-126">Pokud chcete nainstalovat nejnovější rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e71bb-126">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="e71bb-127">Spusťte následující příkazy v okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="e71bb-127">Run the following commands in a terminal window.</span></span> <span data-ttu-id="e71bb-128">Může trvat pět minut, chcete-li nainstalovat rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e71bb-128">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="e71bb-129">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e71bb-129">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="e71bb-130">Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="e71bb-130">You should see the following output if the installation is successful.</span></span>

![Výstup, který označuje úspěch][output]

## <a name="summary"></a><span data-ttu-id="e71bb-132">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e71bb-132">Summary</span></span>
<span data-ttu-id="e71bb-133">Instalaci rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e71bb-133">You've installed the Azure CLI.</span></span> <span data-ttu-id="e71bb-134">Svůj další úkol je vytvoření Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e71bb-134">Your next task is to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e71bb-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e71bb-135">Next steps</span></span>
<span data-ttu-id="e71bb-136">[Vytvoření služby IoT hub a zaregistrujte Arduino panel][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="e71bb-136">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_osx.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md