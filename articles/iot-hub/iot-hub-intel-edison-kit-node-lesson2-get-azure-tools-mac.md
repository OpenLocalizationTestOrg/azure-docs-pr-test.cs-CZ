---
title: "Připojit Intel Edison (uzel) tooAzure IoT - lekci 2: nástroje Azure (macOS) | Microsoft Docs"
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
ms.openlocfilehash: e33b3c9ee8f06f1c6175457f98a0600dd945cf1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a><span data-ttu-id="50d63-104">Získat nástroje Azure (systému macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="50d63-104">Get Azure tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="50d63-105">[Windows 7 a novější][windows]</span><span class="sxs-lookup"><span data-stu-id="50d63-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="50d63-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="50d63-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="50d63-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="50d63-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="50d63-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="50d63-108">What you will do</span></span>
<span data-ttu-id="50d63-109">Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="50d63-109">Install hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="50d63-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="50d63-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="50d63-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="50d63-111">What you will learn</span></span>
<span data-ttu-id="50d63-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="50d63-112">In this article, you will learn:</span></span>
* <span data-ttu-id="50d63-113">Jak tooinstall rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="50d63-113">How tooinstall Azure CLI.</span></span>
* <span data-ttu-id="50d63-114">Jak tooadd podskupině hello rozhraní příkazového řádku Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="50d63-114">How tooadd an IoT subgroup of hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="50d63-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="50d63-115">What you need</span></span>
* <span data-ttu-id="50d63-116">Mac s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="50d63-116">A Mac with an Internet connection.</span></span>
* <span data-ttu-id="50d63-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="50d63-117">An active Azure subscription.</span></span> <span data-ttu-id="50d63-118">Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="50d63-118">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="50d63-119">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="50d63-119">Install Python</span></span>
<span data-ttu-id="50d63-120">I když systému macOS dodává s Python 2.7 předinstalované hello, doporučujeme nainstalovat Python prostřednictvím Homebrew.</span><span class="sxs-lookup"><span data-stu-id="50d63-120">Although macOS comes with Python 2.7 out of hello box, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="50d63-121">V tématu [Python instalace v systému macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="50d63-121">See [Installing Python on macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="50d63-122">Instalace jazyka Python a pip spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="50d63-122">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="50d63-123">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="50d63-123">Install hello Azure CLI</span></span>
<span data-ttu-id="50d63-124">Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="50d63-124">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="50d63-125">Práce přímo z vašeho tooprovision příkazového řádku a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="50d63-125">You work directly from your command line tooprovision and manage resources.</span></span> 

<span data-ttu-id="50d63-126">tooinstall hello nejnovější rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="50d63-126">tooinstall hello latest Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="50d63-127">Spusťte následující příkazy v okno terminálu hello.</span><span class="sxs-lookup"><span data-stu-id="50d63-127">Run hello following commands in a terminal window.</span></span> <span data-ttu-id="50d63-128">Může trvat pět minut tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="50d63-128">It might take five minutes tooinstall hello Azure CLI.</span></span>

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. <span data-ttu-id="50d63-129">Hello instalaci ověřte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="50d63-129">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```

<span data-ttu-id="50d63-130">Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="50d63-130">You should see hello following output if hello installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a><span data-ttu-id="50d63-132">Souhrn</span><span class="sxs-lookup"><span data-stu-id="50d63-132">Summary</span></span>
<span data-ttu-id="50d63-133">Jste nainstalovali hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="50d63-133">You've installed hello Azure CLI.</span></span> <span data-ttu-id="50d63-134">Svůj další úkol je toocreate hello svou identitu Azure IoT hub a zařízení pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="50d63-134">Your next task is toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50d63-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50d63-135">Next steps</span></span>
<span data-ttu-id="50d63-136">[Vytvoření služby IoT hub a zaregistrujte Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="50d63-136">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-mac.md
