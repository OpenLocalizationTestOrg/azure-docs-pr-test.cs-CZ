---
title: "Connect Arduino (C) k Azure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte Adafruit prolnutí M0 Wi-Fi pro první použití."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino nastavit připojení k počítači, instalační program arduino, arduino panelu arduino"
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
ms.openlocfilehash: 9e319292e5d30dea7e45857e435825861aad1c84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="27b88-104">Konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="27b88-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="27b88-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="27b88-105">What you will do</span></span>
<span data-ttu-id="27b88-106">Sestavte Tabule, napájený ho nakonfigurujte Adafruit prolnutí M0 Wi-Fi Arduino panel pro první použití.</span><span class="sxs-lookup"><span data-stu-id="27b88-106">Configure your Adafruit Feather M0 WiFi Arduino board for first-time use by assembling the board, powering it up.</span></span> <span data-ttu-id="27b88-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="27b88-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-you-need"></a><span data-ttu-id="27b88-108">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="27b88-108">What you need</span></span>
<span data-ttu-id="27b88-109">Pro dokončení této operace, potřebujete pro vaše Adafruit prolnutí M0 Wi-Fi Starter Kit následujících částí:</span><span class="sxs-lookup"><span data-stu-id="27b88-109">To complete this operation, you need the following parts for your Adafruit Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="27b88-110">Panel Adafruit prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="27b88-110">The Adafruit Feather M0 WiFi board</span></span>
* <span data-ttu-id="27b88-111">Micro B kabelem USB typ A</span><span class="sxs-lookup"><span data-stu-id="27b88-111">A Micro B to Type A USB cable</span></span>

![Kit][kit]

<span data-ttu-id="27b88-113">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="27b88-113">You also need:</span></span>

* <span data-ttu-id="27b88-114">Počítač se systémem Windows, Mac nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="27b88-114">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="27b88-115">Bezdrátové připojení pro panel Arduino pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="27b88-115">A wireless connection for your Arduino board to connect to.</span></span>
* <span data-ttu-id="27b88-116">Připojení k Internetu kvůli stahování nástroje Konfigurace.</span><span class="sxs-lookup"><span data-stu-id="27b88-116">An Internet connection to download configuration tool.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="27b88-117">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="27b88-117">What you will learn</span></span>
<span data-ttu-id="27b88-118">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="27b88-118">In this article, you will learn:</span></span>

* <span data-ttu-id="27b88-119">Jak sestavit panel Arduino a spotřeby pro následující lekce.</span><span class="sxs-lookup"><span data-stu-id="27b88-119">How to assemble your Arduino board and power it up for the following lessons.</span></span>
* <span data-ttu-id="27b88-120">Postup přidání oprávnění sériového portu v Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="27b88-120">How to add serial port permissions on Ubuntu.</span></span>

## <a name="connect-your-arduino-board-to-your-computer"></a><span data-ttu-id="27b88-121">Panel Arduino připojte k počítači</span><span class="sxs-lookup"><span data-stu-id="27b88-121">Connect your Arduino board to your computer</span></span>

1. <span data-ttu-id="27b88-122">Připojte kabel USB malých nejvyšší malých portu USB.</span><span class="sxs-lookup"><span data-stu-id="27b88-122">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Horní malých portu USB][top-micro-usb-port]

2. <span data-ttu-id="27b88-124">Druhém konci kabelu USB připojte k počítači.</span><span class="sxs-lookup"><span data-stu-id="27b88-124">Plug the other end of USB cable into your computer.</span></span>

   ![Počítač USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a><span data-ttu-id="27b88-126">Přidání oprávnění sériového portu v Ubuntu</span><span class="sxs-lookup"><span data-stu-id="27b88-126">Add serial port permissions on Ubuntu</span></span>

<span data-ttu-id="27b88-127">Pokud používáte Windows nebo systému macOS, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="27b88-127">You can skip this section if you use Windows or macOS.</span></span> <span data-ttu-id="27b88-128">Ubuntu musíte následujících kroků zkontrolujte, zda že má uživatel normální linux oprávnění provoz na portu USB Arduino panel.</span><span class="sxs-lookup"><span data-stu-id="27b88-128">For Ubuntu, you need the following steps to make sure the normal linux user has the permissions to operate on the USB port of your Arduino board.</span></span>

1. <span data-ttu-id="27b88-129">Nyní jako normální uživatelské z terminálu:</span><span class="sxs-lookup"><span data-stu-id="27b88-129">Now as normal user from terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="27b88-130">Zobrazí se něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="27b88-130">You will get something like:</span></span>

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   <span data-ttu-id="27b88-131">"0" může být odlišný počet nebo více položek, může být vrácena.</span><span class="sxs-lookup"><span data-stu-id="27b88-131">The "0" might be a different number, or multiple entries might be returned.</span></span> <span data-ttu-id="27b88-132">V prvním případě dat, potřebujeme je `uucp`, za sekundu je `dialout`, který je vlastníkem skupiny souboru.</span><span class="sxs-lookup"><span data-stu-id="27b88-132">In the first case the data we need is `uucp`, in the second is `dialout`, which is the group owner of the file.</span></span>

2. <span data-ttu-id="27b88-133">Přidejte uživatele do do skupiny:</span><span class="sxs-lookup"><span data-stu-id="27b88-133">Add user to the to the group:</span></span>

   ```bash
   sudo usermod -a -G group-name username
   ```

   <span data-ttu-id="27b88-134">Kde `group-name` se data uvedená v prvním kroku, a `username` je vaše uživatelské jméno linux.</span><span class="sxs-lookup"><span data-stu-id="27b88-134">Where `group-name` is the data found in the first step, and `username` is your linux user name.</span></span>

3. <span data-ttu-id="27b88-135">Musíte se přihlásit v a znovu tato změna se projeví a dokončete instalaci.</span><span class="sxs-lookup"><span data-stu-id="27b88-135">You will need to log out and in again for this change to take effect and complete the setup.</span></span>

## <a name="summary"></a><span data-ttu-id="27b88-136">Souhrn</span><span class="sxs-lookup"><span data-stu-id="27b88-136">Summary</span></span>
<span data-ttu-id="27b88-137">V tomto článku jsme zjistili, jak nakonfigurovat Arduino panel.</span><span class="sxs-lookup"><span data-stu-id="27b88-137">In this article, you’ve learned how to configure your Arduino board.</span></span> <span data-ttu-id="27b88-138">Dalším krokem je instalace nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na vaší kartě Arduino.</span><span class="sxs-lookup"><span data-stu-id="27b88-138">The next task is to install the necessary tools and software in preparation for running a sample application on your Arduino board.</span></span>

![Je připravená hardwaru][hardware-is-ready]

## <a name="next-steps"></a><span data-ttu-id="27b88-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27b88-140">Next steps</span></span>
<span data-ttu-id="27b88-141">[Získat nástroje][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="27b88-141">[Get the tools][get-the-tools]</span></span>
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md