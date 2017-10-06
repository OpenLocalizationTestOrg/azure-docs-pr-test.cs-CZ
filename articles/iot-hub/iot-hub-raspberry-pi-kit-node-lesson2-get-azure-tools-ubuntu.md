---
title: "Připojit malin platformy (uzel) tooAzure IoT - lekci 2: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Nainstalujte Python a na Ubuntu hello rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT cloudové služby, azure cli"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2f98923a-5274-4220-87d4-77ac8beb4d0f
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0adf91bef41f502e1333fdcc75cfb2fe912364c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="cdc62-104">Získat nástroje Azure (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="cdc62-104">Get Azure tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cdc62-105">Windows 7 a novější</span><span class="sxs-lookup"><span data-stu-id="cdc62-105">Windows 7 and later</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [<span data-ttu-id="cdc62-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="cdc62-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [<span data-ttu-id="cdc62-107">systému macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="cdc62-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="cdc62-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="cdc62-108">What you will do</span></span>
<span data-ttu-id="cdc62-109">Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="cdc62-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="cdc62-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="cdc62-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="cdc62-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="cdc62-111">What you will learn</span></span>
<span data-ttu-id="cdc62-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="cdc62-112">In this article, you will learn:</span></span>
* <span data-ttu-id="cdc62-113">Jak tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="cdc62-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="cdc62-114">Jak tooadd podskupině hello rozhraní příkazového řádku Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="cdc62-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="cdc62-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="cdc62-115">What you need</span></span>
* <span data-ttu-id="cdc62-116">Počítač s Ubuntu s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="cdc62-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="cdc62-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="cdc62-117">An active Azure subscription.</span></span> <span data-ttu-id="cdc62-118">Pokud účet nemáte, můžete vytvořit [Bezplatný zkušební účet](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="cdc62-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="cdc62-119">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="cdc62-119">Install hello Azure CLI</span></span>
<span data-ttu-id="cdc62-120">Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure, takže se budete toowork přímo z vašeho příkazového řádku tooprovision a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="cdc62-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command-line tooprovision and manage resources.</span></span>

<span data-ttu-id="cdc62-121">tooinstall hello nejnovější rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="cdc62-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="cdc62-122">Spusťte následující příkazy v okno terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="cdc62-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="cdc62-123">Může trvat pět minut tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="cdc62-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="cdc62-124">Hello instalaci ověřte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cdc62-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="cdc62-125">Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="cdc62-125">You should see hello following output if hello installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a><span data-ttu-id="cdc62-127">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cdc62-127">Summary</span></span>
<span data-ttu-id="cdc62-128">Jste nainstalovali hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="cdc62-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="cdc62-129">Svůj další úkol je toocreate Azure IoT hub a pomocí identity zařízení hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="cdc62-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdc62-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cdc62-130">Next steps</span></span>
[<span data-ttu-id="cdc62-131">Vytvoření služby IoT hub a zaregistrujte malin pí 3</span><span class="sxs-lookup"><span data-stu-id="cdc62-131">Create your IoT hub and register Raspberry Pi 3</span></span>](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

