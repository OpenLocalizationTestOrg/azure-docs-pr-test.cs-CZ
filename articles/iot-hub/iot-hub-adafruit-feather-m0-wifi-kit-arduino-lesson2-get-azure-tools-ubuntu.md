---
title: "Připojit Arduino tooAzure IoT - lekci 2: nástroje Azure (Ubuntu) | Microsoft Docs"
description: "Nainstalujte na Ubuntu Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: bb5cb602-7292-4772-ac90-c0b52ebc8340
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7eb9c891a6340fee018894883583022d740ecb6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a><span data-ttu-id="6f4b8-104">Získat nástroje Azure (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="6f4b8-104">Get Azure tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="6f4b8-105">[Windows 7 nebo novější][windows]</span><span class="sxs-lookup"><span data-stu-id="6f4b8-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="6f4b8-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="6f4b8-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="6f4b8-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="6f4b8-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="6f4b8-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="6f4b8-108">What you will do</span></span>

<span data-ttu-id="6f4b8-109">Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="6f4b8-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="6f4b8-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) pro panel Adafruit prolnutí M0 Wi-Fi Arduino.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) for your Adafruit Feather M0 WiFi Arduino board.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6f4b8-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="6f4b8-111">What you will learn</span></span>
<span data-ttu-id="6f4b8-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="6f4b8-112">In this article, you will learn:</span></span>
* <span data-ttu-id="6f4b8-113">Jak tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-113">How tooinstall hello Azure CLI.</span></span>
* <span data-ttu-id="6f4b8-114">Jak tooadd podskupině hello rozhraní příkazového řádku Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6f4b8-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="6f4b8-115">What you need</span></span>
* <span data-ttu-id="6f4b8-116">Počítač s Ubuntu s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-116">An Ubuntu computer with an Internet connection.</span></span>
* <span data-ttu-id="6f4b8-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-117">An active Azure subscription.</span></span> <span data-ttu-id="6f4b8-118">Pokud účet nemáte, můžete vytvořit [Bezplatný zkušební účet](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-118">If you don't have an account, you can create a [free trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="6f4b8-119">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6f4b8-119">Install hello Azure CLI</span></span>
<span data-ttu-id="6f4b8-120">Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure, takže se budete toowork přímo z vašeho tooprovision příkazového řádku a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-120">hello Azure CLI provides a multiplatform command-line experience for Azure, enabling you toowork directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="6f4b8-121">tooinstall hello nejnovější rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="6f4b8-121">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="6f4b8-122">Spusťte následující příkazy v okno terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-122">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="6f4b8-123">Může trvat pět minut tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-123">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="6f4b8-124">Hello instalaci ověřte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6f4b8-124">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="6f4b8-125">Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-125">You should see hello following output if hello installation is successful.</span></span>

![Výstup, který označuje úspěch][output]

## <a name="summary"></a><span data-ttu-id="6f4b8-127">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6f4b8-127">Summary</span></span>
<span data-ttu-id="6f4b8-128">Jste nainstalovali hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-128">You've installed hello Azure CLI.</span></span> <span data-ttu-id="6f4b8-129">Svůj další úkol je toocreate Azure IoT hub a pomocí identity zařízení hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6f4b8-129">Your next task is toocreate your Azure IoT hub and device identity using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f4b8-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f4b8-130">Next steps</span></span>
<span data-ttu-id="6f4b8-131">[Vytvoření služby IoT hub a zaregistrujte Arduino panel][create-your-iot-hub-and-register-your-arduino-board]</span><span class="sxs-lookup"><span data-stu-id="6f4b8-131">[Create your IoT hub and register your Arduino board][create-your-iot-hub-and-register-your-arduino-board]</span></span>
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_ubuntu.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md