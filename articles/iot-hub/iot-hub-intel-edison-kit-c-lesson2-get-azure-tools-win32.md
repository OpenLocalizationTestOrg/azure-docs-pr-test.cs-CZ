---
title: "Connect Intel Edison (C) tooAzure IoT - lekci 2: nástroje Azure (Windows) | Microsoft Docs"
description: "Ve Windows 7 a novějších verzích nainstalujte Python a hello rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 1035760e-cdd1-4d99-8003-067e98b29762
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 46f964541c00674500e4f6a072a9a2547b5b24d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a><span data-ttu-id="21ed8-104">Získat nástroje Azure (Windows 7 a novější)</span><span class="sxs-lookup"><span data-stu-id="21ed8-104">Get Azure tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="21ed8-105">[Windows 7 a novější][windows]</span><span class="sxs-lookup"><span data-stu-id="21ed8-105">[Windows 7 and later][windows]</span></span>
> * <span data-ttu-id="21ed8-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="21ed8-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="21ed8-107">[systému macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="21ed8-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="21ed8-108">Co provedete</span><span class="sxs-lookup"><span data-stu-id="21ed8-108">What you will do</span></span>
<span data-ttu-id="21ed8-109">Nainstalujte Python a hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="21ed8-109">Install Python and hello Azure command-line interface (Azure CLI).</span></span> <span data-ttu-id="21ed8-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="21ed8-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="21ed8-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="21ed8-111">What you will learn</span></span>
<span data-ttu-id="21ed8-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="21ed8-112">In this article, you will learn:</span></span>
* <span data-ttu-id="21ed8-113">Jak tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="21ed8-113">How tooinstall Python.</span></span>
* <span data-ttu-id="21ed8-114">Jak tooinstall hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="21ed8-114">How tooinstall hello Azure CLI.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="21ed8-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="21ed8-115">What you need</span></span>
* <span data-ttu-id="21ed8-116">Počítač Windows s připojením k Internetu.</span><span class="sxs-lookup"><span data-stu-id="21ed8-116">A Windows computer with an Internet connection.</span></span>
* <span data-ttu-id="21ed8-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="21ed8-117">An active Azure subscription.</span></span> <span data-ttu-id="21ed8-118">Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="21ed8-118">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>

## <a name="install-python"></a><span data-ttu-id="21ed8-119">Instalace jazyka Python</span><span class="sxs-lookup"><span data-stu-id="21ed8-119">Install Python</span></span>
<span data-ttu-id="21ed8-120">[Instalaci Pythonu](https://www.python.org/downloads/) na počítači s Windows.</span><span class="sxs-lookup"><span data-stu-id="21ed8-120">[Install Python](https://www.python.org/downloads/) on your Windows computer.</span></span> <span data-ttu-id="21ed8-121">Můžete nainstalovat Python 2.7, 3.4 nebo 3.5.</span><span class="sxs-lookup"><span data-stu-id="21ed8-121">You can install Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="21ed8-122">V tomto kurzu vychází z Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="21ed8-122">This tutorial is based on Python 2.7.</span></span> <span data-ttu-id="21ed8-123">Pokud jste již nainstalovali Python, přejděte toohello další části a nainstalujte hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="21ed8-123">If you've already installed Python, go toohello next section and install hello Azure CLI.</span></span>

<span data-ttu-id="21ed8-124">Musíte taky tooadd hello cesta hello složek, kdy jsou python.exe a pip.exe systému nainstalovaná toohello `PATH` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="21ed8-124">You also need tooadd hello path of hello folders where python.exe and pip.exe are installed toohello system `PATH` environment variable.</span></span> <span data-ttu-id="21ed8-125">Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="21ed8-125">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="21ed8-126">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="21ed8-126">Install hello Azure CLI</span></span>
<span data-ttu-id="21ed8-127">Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="21ed8-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="21ed8-128">Práce přímo z vašeho tooprovision příkazového řádku a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="21ed8-128">You work directly from your command line tooprovision and manage resources.</span></span>

<span data-ttu-id="21ed8-129">tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="21ed8-129">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="21ed8-130">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="21ed8-130">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="21ed8-131">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="21ed8-131">Run hello following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. <span data-ttu-id="21ed8-132">Hello instalaci ověřte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="21ed8-132">Verify hello installation by running hello following command:</span></span>

   ```cmd
   az iot -h
   ```

<span data-ttu-id="21ed8-133">Uvidíte, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="21ed8-133">You see hello following output if hello installation is successful.</span></span>

![Výstup, který označuje úspěch](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a><span data-ttu-id="21ed8-135">Souhrn</span><span class="sxs-lookup"><span data-stu-id="21ed8-135">Summary</span></span>
<span data-ttu-id="21ed8-136">Jste nainstalovali hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="21ed8-136">You've installed hello Azure CLI.</span></span> <span data-ttu-id="21ed8-137">Vaše další úloh toocreate Azure IoT hub a zařízení identity pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="21ed8-137">Your next task toocreate your Azure IoT hub and device identity by using hello Azure CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21ed8-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21ed8-138">Next steps</span></span>
<span data-ttu-id="21ed8-139">[Vytvoření služby IoT hub a zaregistrujte Intel Edison][create-your-iot-hub-and-register-intel-edison]</span><span class="sxs-lookup"><span data-stu-id="21ed8-139">[Create your IoT hub and register Intel Edison][create-your-iot-hub-and-register-intel-edison]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
