---
title: "Připojení k Azure IoT - lekci 2 Intel Edison (uzel): Azure nástroje (macOS) | Microsoft Docs"
description: "Instalace v systému macOS Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 8a2a8031-b1a6-4219-b17d-2825550c35e1
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 16705d54e90ee1482822986ca8c581a192e8702f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="ff270-104">Získat nástroje Azure (systému macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="ff270-104">Get Azure tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="ff270-105">[Windows 7 a novější][windows]</span><span class="sxs-lookup"><span data-stu-id="ff270-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="ff270-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="ff270-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="ff270-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="ff270-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="ff270-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="ff270-108">What you will do</span></span>
<span data-ttu-id="ff270-109">Nainstalujte rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="ff270-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="ff270-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="ff270-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="ff270-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="ff270-111">What you will learn</span></span>
<span data-ttu-id="ff270-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="ff270-112">In this article, you will learn:</span></span>
* <span data-ttu-id="ff270-113">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ff270-113">How to install Azure CLI.</span></span>
* <span data-ttu-id="ff270-114">Postup přidání podskupině IoT rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ff270-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="ff270-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="ff270-115">What you need</span></span>
* <span data-ttu-id="ff270-116">Mac s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="ff270-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="ff270-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ff270-117">An active Azure subscription.</span></span> <span data-ttu-id="ff270-118">Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="ff270-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="ff270-119">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="ff270-119">Install Python</span></span>
<span data-ttu-id="ff270-120">I když systému macOS dodává s Python 2.7 z pole, doporučujeme nainstalovat Python prostřednictvím Homebrew.</span><span class="sxs-lookup"><span data-stu-id="ff270-120">Although macOS comes with Python 2.7 out of the box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="ff270-121">V tématu [Python instalace v systému macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="ff270-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="ff270-122">Instalace Python a pip spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="ff270-122">Install Python and pip by running the following command:</span></span>

```bash
brew install python
```

## <a name="install-the-azure-cli"></a><span data-ttu-id="ff270-123">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ff270-123">Install the Azure CLI</span></span>
<span data-ttu-id="ff270-124">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ff270-124">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="ff270-125">Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ff270-125">You work directly from your command line to provision and manage resources.</span></span> 

<span data-ttu-id="ff270-126">Pokud chcete nainstalovat nejnovější rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ff270-126">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="ff270-127">Spusťte následující příkazy v okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="ff270-127">Run the following commands in a terminal window.</span></span> <span data-ttu-id="ff270-128">Může trvat pět minut, chcete-li nainstalovat rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ff270-128">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="ff270-129">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ff270-129">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="ff270-130">Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="ff270-130">You should see the following output if the installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a><span data-ttu-id="ff270-132">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ff270-132">Summary</span></span>
<span data-ttu-id="ff270-133">Instalaci rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ff270-133">You've installed the Azure CLI.</span></span> <span data-ttu-id="ff270-134">Svůj další úkol je vytvoření Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ff270-134">Your next task is to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff270-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff270-135">Next steps</span></span>
<span data-ttu-id="ff270-136">[Vytvoření služby IoT hub a zaregistrujte Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="ff270-136">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-mac.md
