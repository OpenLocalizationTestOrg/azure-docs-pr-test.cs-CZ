---
title: "Connect Raspberry PI (C) k Azure IoT - lekci 2: nástroje Azure (Ubuntu) | Microsoft Docs"
description: "Nainstalujte na Ubuntu Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT cloudové služby, azure cli"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a03512f2-fabe-40c5-8505-b93eef8e5bec
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: be1506edf0e63190dbb85a3adb7897bd1cc84d38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="5713a-104">Získat nástroje Azure (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="5713a-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5713a-105">Windows 7 a novější</span><span class="sxs-lookup"><span data-stu-id="5713a-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="5713a-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="5713a-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="5713a-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="5713a-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="5713a-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="5713a-108">What you will do</span></span>
<span data-ttu-id="5713a-109">Nainstalujte rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="5713a-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="5713a-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5713a-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5713a-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="5713a-111">What you will learn</span></span>
<span data-ttu-id="5713a-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="5713a-112">In this article, you will learn:</span></span>
* <span data-ttu-id="5713a-113">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5713a-113">How to install the Azure CLI.</span></span>
* <span data-ttu-id="5713a-114">Postup přidání podskupině IoT rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5713a-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5713a-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="5713a-115">What you need</span></span>
* <span data-ttu-id="5713a-116">Počítač s Ubuntu s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="5713a-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="5713a-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5713a-117">An active Azure subscription.</span></span> <span data-ttu-id="5713a-118">Pokud účet nemáte, můžete vytvořit [Bezplatný zkušební účet](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="5713a-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="5713a-119">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5713a-119">Install the Azure CLI</span></span>
<span data-ttu-id="5713a-120">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure, umožňuje pracovat přímo z příkazového řádku pro zřizování a správu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5713a-120">The Azure CLI provides a multiplatform command-line experience for Azure, enabling you to work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="5713a-121">Pokud chcete nainstalovat nejnovější rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5713a-121">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="5713a-122">Spusťte následující příkazy v okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="5713a-122">Run the following commands in a terminal window.</span></span> <span data-ttu-id="5713a-123">Může trvat pět minut, chcete-li nainstalovat rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5713a-123">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="5713a-124">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5713a-124">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="5713a-125">Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="5713a-125">You should see the following output if the installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="5713a-127">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5713a-127">Summary</span></span>
<span data-ttu-id="5713a-128">Instalaci rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5713a-128">You've installed the Azure CLI.</span></span> <span data-ttu-id="5713a-129">Svůj další úkol je vytvoření vaší Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5713a-129">Your next task is to create your Azure IoT hub and device identity using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5713a-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5713a-130">Next steps</span></span>
[<span data-ttu-id="5713a-131">Vytvoření služby IoT hub a zaregistrujte malin pí 3</span><span class="sxs-lookup"><span data-stu-id="5713a-131">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

