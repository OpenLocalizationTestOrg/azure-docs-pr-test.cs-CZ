---
title: "Connect Arduino (C) tooAzure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte Adafruit prolnutí M0 Wi-Fi pro první použití."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino nastavit připojení arduino toopc, instalační program arduino, arduino panelu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: f5b334f0-a148-41aa-b374-ce7b9f5b305a
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 30b764e8ff6221995456283a226e79f064b2d74e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="4219a-104">Konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="4219a-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="4219a-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="4219a-105">What you will do</span></span>
<span data-ttu-id="4219a-106">Sestavte hello Tabule, napájený ho nakonfigurujte Adafruit prolnutí M0 Wi-Fi Arduino panel pro první použití.</span><span class="sxs-lookup"><span data-stu-id="4219a-106">Configure your Adafruit Feather M0 WiFi Arduino board for first-time use by assembling hello board, powering it up.</span></span> <span data-ttu-id="4219a-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="4219a-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4219a-108">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="4219a-108">What you need</span></span>
<span data-ttu-id="4219a-109">toocomplete tuto operaci je třeba hello vaší Adafruit prolnutí M0 Wi-Fi Starter Kit následujících částí:</span><span class="sxs-lookup"><span data-stu-id="4219a-109">toocomplete this operation, you need hello following parts for your Adafruit Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="4219a-110">Hello Tabule Adafruit prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="4219a-110">hello Adafruit Feather M0 WiFi board</span></span>
* <span data-ttu-id="4219a-111">Micro B tooType kabelu A USB</span><span class="sxs-lookup"><span data-stu-id="4219a-111">A Micro B tooType A USB cable</span></span>

![Kit][kit]

<span data-ttu-id="4219a-113">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="4219a-113">You also need:</span></span>

* <span data-ttu-id="4219a-114">Počítač se systémem Windows, Mac nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="4219a-114">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="4219a-115">Bezdrátové připojení pro vaše tooconnect Arduino Tabule pro.</span><span class="sxs-lookup"><span data-stu-id="4219a-115">A wireless connection for your Arduino board tooconnect to.</span></span>
* <span data-ttu-id="4219a-116">Internetu toodownload konfigurační nástroj pro připojení.</span><span class="sxs-lookup"><span data-stu-id="4219a-116">An Internet connection toodownload configuration tool.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4219a-117">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="4219a-117">What you will learn</span></span>
<span data-ttu-id="4219a-118">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="4219a-118">In this article, you will learn:</span></span>

* <span data-ttu-id="4219a-119">Jak tooassemble Arduino panelu a power odpovídající hello následující lekci.</span><span class="sxs-lookup"><span data-stu-id="4219a-119">How tooassemble your Arduino board and power it up for hello following lessons.</span></span>
* <span data-ttu-id="4219a-120">Jak tooadd oprávnění Ubuntu sériového portu.</span><span class="sxs-lookup"><span data-stu-id="4219a-120">How tooadd serial port permissions on Ubuntu.</span></span>

## <a name="connect-your-arduino-board-tooyour-computer"></a><span data-ttu-id="4219a-121">Připojení počítače Arduino panelu tooyour</span><span class="sxs-lookup"><span data-stu-id="4219a-121">Connect your Arduino board tooyour computer</span></span>

1. <span data-ttu-id="4219a-122">Připojte kabel USB malých hello portu USB horním malých hello.</span><span class="sxs-lookup"><span data-stu-id="4219a-122">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Horní malých portu USB][top-micro-usb-port]

2. <span data-ttu-id="4219a-124">Moduly hello druhém konci kabelu USB k počítači.</span><span class="sxs-lookup"><span data-stu-id="4219a-124">Plug hello other end of USB cable into your computer.</span></span>

   ![Počítač USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a><span data-ttu-id="4219a-126">Přidání oprávnění sériového portu v Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4219a-126">Add serial port permissions on Ubuntu</span></span>

<span data-ttu-id="4219a-127">Pokud používáte Windows nebo systému macOS, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="4219a-127">You can skip this section if you use Windows or macOS.</span></span> <span data-ttu-id="4219a-128">Ubuntu musíte hello následující kroky toomake se, že má uživatel normální linux hello hello oprávnění toooperate na portu USB hello Arduino panel.</span><span class="sxs-lookup"><span data-stu-id="4219a-128">For Ubuntu, you need hello following steps toomake sure hello normal linux user has hello permissions toooperate on hello USB port of your Arduino board.</span></span>

1. <span data-ttu-id="4219a-129">Nyní jako normální uživatelské z terminálu:</span><span class="sxs-lookup"><span data-stu-id="4219a-129">Now as normal user from terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="4219a-130">Zobrazí se něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="4219a-130">You will get something like:</span></span>

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   <span data-ttu-id="4219a-131">Hello "0" může být odlišný počet nebo více položek, může být vrácena.</span><span class="sxs-lookup"><span data-stu-id="4219a-131">hello "0" might be a different number, or multiple entries might be returned.</span></span> <span data-ttu-id="4219a-132">V první data případu hello hello potřebujeme je `uucp`, v hello druhou je `dialout`, což je vlastník skupiny hello hello souboru.</span><span class="sxs-lookup"><span data-stu-id="4219a-132">In hello first case hello data we need is `uucp`, in hello second is `dialout`, which is hello group owner of hello file.</span></span>

2. <span data-ttu-id="4219a-133">Přidáte skupinu uživatelů toohello toohello:</span><span class="sxs-lookup"><span data-stu-id="4219a-133">Add user toohello toohello group:</span></span>

   ```bash
   sudo usermod -a -G group-name username
   ```

   <span data-ttu-id="4219a-134">Kde `group-name` se nenašla v prvním kroku hello hello data a `username` je vaše uživatelské jméno linux.</span><span class="sxs-lookup"><span data-stu-id="4219a-134">Where `group-name` is hello data found in hello first step, and `username` is your linux user name.</span></span>

3. <span data-ttu-id="4219a-135">Budete znovu potřebovat toolog v a pro tuto změnu tootake účinek a hello dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="4219a-135">You will need toolog out and in again for this change tootake effect and complete hello setup.</span></span>

## <a name="summary"></a><span data-ttu-id="4219a-136">Souhrn</span><span class="sxs-lookup"><span data-stu-id="4219a-136">Summary</span></span>
<span data-ttu-id="4219a-137">V tomto článku, když jste se naučili jak tooconfigure Arduino panel.</span><span class="sxs-lookup"><span data-stu-id="4219a-137">In this article, you’ve learned how tooconfigure your Arduino board.</span></span> <span data-ttu-id="4219a-138">Další úlohou Hello je tooinstall hello nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na vaší kartě Arduino.</span><span class="sxs-lookup"><span data-stu-id="4219a-138">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on your Arduino board.</span></span>

![Je připravená hardwaru][hardware-is-ready]

## <a name="next-steps"></a><span data-ttu-id="4219a-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4219a-140">Next steps</span></span>
<span data-ttu-id="4219a-141">[Získat nástroje hello][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="4219a-141">[Get hello tools][get-the-tools]</span></span>
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md