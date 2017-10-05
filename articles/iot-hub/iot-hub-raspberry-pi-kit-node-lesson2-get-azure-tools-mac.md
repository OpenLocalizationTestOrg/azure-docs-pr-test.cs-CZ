---
title: "Připojení k Azure IoT - lekci 2 malin platformy (uzel): získat nástroje (Ubuntu) | Microsoft Docs"
description: "Instalace v systému macOS Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT cloudové služby, azure cli"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 1814b703-2d81-45db-aff0-eb338c97f120
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 12a8c5b20724e747f3799960a0bd39b82d7fa6b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="e664e-104">Získat nástroje Azure (systému macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="e664e-104">Get Azure tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e664e-105">Windows 7 a novější</span><span class="sxs-lookup"><span data-stu-id="e664e-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="e664e-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="e664e-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="e664e-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="e664e-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="e664e-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="e664e-108">What you will do</span></span>
<span data-ttu-id="e664e-109">Nainstalujte rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="e664e-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="e664e-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e664e-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e664e-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e664e-111">What you will learn</span></span>
<span data-ttu-id="e664e-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="e664e-112">In this article, you will learn:</span></span>
* <span data-ttu-id="e664e-113">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e664e-113">How to install Azure CLI.</span></span>
* <span data-ttu-id="e664e-114">Postup přidání podskupině IoT rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e664e-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e664e-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="e664e-115">What you need</span></span>
* <span data-ttu-id="e664e-116">Mac s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="e664e-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="e664e-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e664e-117">An active Azure subscription.</span></span> <span data-ttu-id="e664e-118">Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="e664e-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="e664e-119">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="e664e-119">Install Python</span></span>
<span data-ttu-id="e664e-120">I když systému macOS dodává s Python 2.7 z pole, doporučujeme nainstalovat Python prostřednictvím Homebrew.</span><span class="sxs-lookup"><span data-stu-id="e664e-120">Although macOS comes with Python 2.7 out of the box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="e664e-121">V tématu [Python instalace v systému macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="e664e-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="e664e-122">Instalace Python a pip spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="e664e-122">Install Python and pip by running the following command:</span></span>

```bash
brew install python
```

## <a name="install-the-azure-cli"></a><span data-ttu-id="e664e-123">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e664e-123">Install the Azure CLI</span></span>
<span data-ttu-id="e664e-124">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e664e-124">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="e664e-125">Můžete pracovat přímo z vašeho příkazového řádku a zřizovat a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="e664e-125">You work directly from your command-line to provision and manage resources.</span></span> 

<span data-ttu-id="e664e-126">Pokud chcete nainstalovat nejnovější rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e664e-126">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="e664e-127">Spusťte následující příkazy v okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="e664e-127">Run the following commands in a terminal window.</span></span> <span data-ttu-id="e664e-128">Může trvat pět minut, chcete-li nainstalovat rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e664e-128">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="e664e-129">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e664e-129">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="e664e-130">Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="e664e-130">You should see the following output if the installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a><span data-ttu-id="e664e-132">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e664e-132">Summary</span></span>
<span data-ttu-id="e664e-133">Instalaci rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e664e-133">You've installed the Azure CLI.</span></span> <span data-ttu-id="e664e-134">Svůj další úkol je vytvoření Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e664e-134">Your next task is to create your Azure IoT hub and device identity by using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e664e-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e664e-135">Next steps</span></span>
[<span data-ttu-id="e664e-136">Vytvoření služby IoT hub a zaregistrujte malin pí 3</span><span class="sxs-lookup"><span data-stu-id="e664e-136">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

