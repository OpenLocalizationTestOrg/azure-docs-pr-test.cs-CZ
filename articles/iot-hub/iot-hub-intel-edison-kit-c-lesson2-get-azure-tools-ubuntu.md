---
title: "Connect Intel Edison (C) k Azure IoT - lekci 2: nástroje Azure (Ubuntu) | Microsoft Docs"
description: "Nainstalujte na Ubuntu Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 2463cb8e-5758-4d72-af98-62520d41f2f7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 897ab63af85a1f830ed49084ce7c3e74c84a4cc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="b8027-104">Získat nástroje Azure (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="b8027-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="b8027-105">[Windows 7 a novější][windows]</span><span class="sxs-lookup"><span data-stu-id="b8027-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="b8027-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="b8027-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="b8027-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="b8027-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="b8027-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="b8027-108">What you will do</span></span>
<span data-ttu-id="b8027-109">Nainstalujte rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="b8027-109">Install the Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="b8027-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="b8027-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="b8027-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="b8027-111">What you will learn</span></span>
<span data-ttu-id="b8027-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="b8027-112">In this article, you will learn:</span></span>
* <span data-ttu-id="b8027-113">Postup instalace rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b8027-113">How to install the Azure CLI.</span></span>
* <span data-ttu-id="b8027-114">Postup přidání podskupině IoT rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b8027-114">How to add an IoT subgroup of the Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b8027-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="b8027-115">What you need</span></span>
* <span data-ttu-id="b8027-116">Počítač s Ubuntu s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="b8027-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="b8027-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b8027-117">An active Azure subscription.</span></span> <span data-ttu-id="b8027-118">Pokud účet nemáte, můžete vytvořit [Bezplatný zkušební účet](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="b8027-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="b8027-119">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b8027-119">Install the Azure CLI</span></span>
<span data-ttu-id="b8027-120">Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure, umožňuje pracovat přímo z příkazového řádku pro zřizování a správu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8027-120">The Azure CLI provides a multiplatform command-line experience for Azure, enabling you to work directly from your command line to provision and manage resources.</span></span>

<span data-ttu-id="b8027-121">Pokud chcete nainstalovat nejnovější rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b8027-121">To install the latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="b8027-122">Spusťte následující příkazy v okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="b8027-122">Run the following commands in a terminal window.</span></span> <span data-ttu-id="b8027-123">Může trvat pět minut, chcete-li nainstalovat rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b8027-123">It might take five minutes to install the Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="b8027-124">Ověřte instalaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b8027-124">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="b8027-125">Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.</span><span class="sxs-lookup"><span data-stu-id="b8027-125">You should see the following output if the installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="b8027-127">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b8027-127">Summary</span></span>
<span data-ttu-id="b8027-128">Instalaci rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b8027-128">You've installed the Azure CLI.</span></span> <span data-ttu-id="b8027-129">Svůj další úkol je vytvoření vaší Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b8027-129">Your next task is to create your Azure IoT hub and device identity using the Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8027-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8027-130">Next steps</span></span>
<span data-ttu-id="b8027-131">[Vytvoření služby IoT hub a zaregistrujte Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="b8027-131">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
