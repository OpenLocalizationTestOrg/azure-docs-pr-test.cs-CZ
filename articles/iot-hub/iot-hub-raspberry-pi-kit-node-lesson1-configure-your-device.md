---
title: "Připojení k Azure IoT - lekci 1 malin platformy (uzel): Konfigurace zařízení | Microsoft Docs"
description: Configure Raspberry Pi 3 for first-time use and install the Raspbian OS, a free operating system that is optimized for the Raspberry Pi hardware.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "instalace raspbian, raspbian stažení způsob pro instalaci raspbian, raspbian instalační program, instalace raspbian Malinová platformy, malinová pí instalace operačního systému, malinová pí sd karty instalace, malin pí připojení, připojení k Malinová pi, malinová pí připojení"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 43f7c2cf-f1a5-4dd5-93f0-7e546c6dc91e
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b848c48157a2310f0eb1d6398f8b9aaa4395d47f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="31f9d-104">Konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="31f9d-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="31f9d-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="31f9d-105">What you will do</span></span>
<span data-ttu-id="31f9d-106">Nakonfigurujte platformy pro první použití a nainstalujte Raspbian operačního systému.</span><span class="sxs-lookup"><span data-stu-id="31f9d-106">Configure Pi for first-time use and install the Raspbian operating system.</span></span> <span data-ttu-id="31f9d-107">Raspbian je volné operačního systému, která je optimalizovaná pro hardware malin platformy.</span><span class="sxs-lookup"><span data-stu-id="31f9d-107">Raspbian is a free operating system that is optimized for the Raspberry Pi hardware.</span></span> <span data-ttu-id="31f9d-108">Pokud máte potíže, můžete hledat řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="31f9d-108">If you have any problems, you can seek solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="31f9d-109">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="31f9d-109">What you will learn</span></span>
<span data-ttu-id="31f9d-110">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="31f9d-110">In this article, you will learn:</span></span>

* <span data-ttu-id="31f9d-111">Postup instalace Raspbian v pí.</span><span class="sxs-lookup"><span data-stu-id="31f9d-111">How to install Raspbian on Pi.</span></span>
* <span data-ttu-id="31f9d-112">Jak power až pí pomocí kabelu USB.</span><span class="sxs-lookup"><span data-stu-id="31f9d-112">How to power up Pi by using a USB cable.</span></span>
* <span data-ttu-id="31f9d-113">Postup připojení platformy k síti pomocí kabelu Ethernet nebo bezdrátové sítě.</span><span class="sxs-lookup"><span data-stu-id="31f9d-113">How to connect Pi to the network by using an Ethernet cable or wireless network.</span></span>
* <span data-ttu-id="31f9d-114">Postup přidání DIODU do breadboard a připojte ho k pí.</span><span class="sxs-lookup"><span data-stu-id="31f9d-114">How to add an LED to the breadboard and connect it to Pi.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="31f9d-115">Co budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="31f9d-115">What you will need</span></span>
<span data-ttu-id="31f9d-116">Pro dokončení této operace, musíte z vaší malin pí 3 Starter Kit následujících částí:</span><span class="sxs-lookup"><span data-stu-id="31f9d-116">To complete this operation, you need the following parts from your Raspberry Pi 3 Starter Kit:</span></span>

* <span data-ttu-id="31f9d-117">Panel malin pí 3</span><span class="sxs-lookup"><span data-stu-id="31f9d-117">The Raspberry Pi 3 board</span></span>
* <span data-ttu-id="31f9d-118">Karta microSD 16 GB</span><span class="sxs-lookup"><span data-stu-id="31f9d-118">The 16-GB microSD card</span></span>
* <span data-ttu-id="31f9d-119">2 amp napájení 5 volt s 6 stopy malých kabelu USB</span><span class="sxs-lookup"><span data-stu-id="31f9d-119">The 5-volt 2-amp power supply with the 6-foot micro USB cable</span></span>
* <span data-ttu-id="31f9d-120">Breadboard</span><span class="sxs-lookup"><span data-stu-id="31f9d-120">The breadboard</span></span>
* <span data-ttu-id="31f9d-121">Konektor vodičům</span><span class="sxs-lookup"><span data-stu-id="31f9d-121">Connector wires</span></span>
* <span data-ttu-id="31f9d-122">560 ohm odpor</span><span class="sxs-lookup"><span data-stu-id="31f9d-122">A 560-ohm resistor</span></span>
* <span data-ttu-id="31f9d-123">Rozptýlený DIODU 10 mm</span><span class="sxs-lookup"><span data-stu-id="31f9d-123">A diffused 10-mm LED</span></span>
* <span data-ttu-id="31f9d-124">Kabel Ethernet</span><span class="sxs-lookup"><span data-stu-id="31f9d-124">The Ethernet cable</span></span>

![Co v Startovní sady](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

<span data-ttu-id="31f9d-126">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="31f9d-126">You also need:</span></span>

* <span data-ttu-id="31f9d-127">Pevné nebo bezdrátové připojení pro platformy pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="31f9d-127">A wired or wireless connection for Pi to connect to.</span></span>
* <span data-ttu-id="31f9d-128">USB-adaptér nebo miniSD karta SD vypálíte image operačního systému na kartě microSD.</span><span class="sxs-lookup"><span data-stu-id="31f9d-128">A USB-SD adapter or miniSD card to burn the operating system image onto the microSD card.</span></span>
* <span data-ttu-id="31f9d-129">Počítač se systémem Windows, Mac nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="31f9d-129">A computer running Windows, Mac, or Linux.</span></span> <span data-ttu-id="31f9d-130">Počítač se používá k instalaci Raspbian na kartě microSD.</span><span class="sxs-lookup"><span data-stu-id="31f9d-130">The computer is used to install Raspbian on the microSD card.</span></span>
* <span data-ttu-id="31f9d-131">Připojení k Internetu kvůli stahování nezbytné nástroje a software.</span><span class="sxs-lookup"><span data-stu-id="31f9d-131">An Internet connection to download the necessary tools and software.</span></span>

## <a name="install-raspbian-on-the-microsd-card"></a><span data-ttu-id="31f9d-132">Na kartě microSD nainstalovat Raspbian</span><span class="sxs-lookup"><span data-stu-id="31f9d-132">Install Raspbian on the microSD card</span></span>
<span data-ttu-id="31f9d-133">Připravte karty microSD pro instalaci bitové kopie Raspbian.</span><span class="sxs-lookup"><span data-stu-id="31f9d-133">Prepare the microSD card for installation of the Raspbian image.</span></span>

1. <span data-ttu-id="31f9d-134">Stáhněte si Raspbian.</span><span class="sxs-lookup"><span data-stu-id="31f9d-134">Download Raspbian.</span></span>
   1. <span data-ttu-id="31f9d-135">[Stáhněte si](https://www.raspberrypi.org/downloads/raspbian/) soubor .zip pro Raspbian Klára s pixelů.</span><span class="sxs-lookup"><span data-stu-id="31f9d-135">[Download](https://www.raspberrypi.org/downloads/raspbian/) the .zip file for Raspbian Jessie with Pixel.</span></span>
   2. <span data-ttu-id="31f9d-136">Extrahujte Raspbian image do složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="31f9d-136">Extract the Raspbian image to a folder on your computer.</span></span>
2. <span data-ttu-id="31f9d-137">Nainstalujte Raspbian microSD karta.</span><span class="sxs-lookup"><span data-stu-id="31f9d-137">Install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="31f9d-138">[Stáhněte si](https://www.etcher.io) a nainstalujte nástroj zapisovací jednotka Etcher SD karty.</span><span class="sxs-lookup"><span data-stu-id="31f9d-138">[Download](https://www.etcher.io) and install the Etcher SD card burner utility.</span></span>
   2. <span data-ttu-id="31f9d-139">Spusťte Etcher a vyberte Raspbian bitovou kopii, která jste extrahovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="31f9d-139">Run Etcher and select the Raspbian image that you extracted in step 1.</span></span>
   3. <span data-ttu-id="31f9d-140">Vyberte jednotku microSD karta.</span><span class="sxs-lookup"><span data-stu-id="31f9d-140">Select the microSD card drive.</span></span>
      <span data-ttu-id="31f9d-141">Všimněte si, že Etcher může jste již vybrali správnou jednotku.</span><span class="sxs-lookup"><span data-stu-id="31f9d-141">Note that Etcher may have already selected the correct drive.</span></span>
   4. <span data-ttu-id="31f9d-142">Klikněte na tlačítko **Flash** nainstalovat Raspbian do microSD karta.</span><span class="sxs-lookup"><span data-stu-id="31f9d-142">Click **Flash** to install Raspbian to the microSD card.</span></span>
   5. <span data-ttu-id="31f9d-143">Karta microSD odeberte z počítače, po dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="31f9d-143">Remove the microSD card from your computer when installation is complete.</span></span>
      <span data-ttu-id="31f9d-144">Je bezpečné karty microSD přímo odebrat, protože Etcher automaticky vysune nebo odpojí microSD karta po dokončení.</span><span class="sxs-lookup"><span data-stu-id="31f9d-144">It's safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.</span></span>
   6. <span data-ttu-id="31f9d-145">Karta microSD vložte do pí.</span><span class="sxs-lookup"><span data-stu-id="31f9d-145">Insert the microSD card into Pi.</span></span>

![Vložit kartu SD.](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a><span data-ttu-id="31f9d-147">Zapnout platformy</span><span class="sxs-lookup"><span data-stu-id="31f9d-147">Turn on Pi</span></span>
<span data-ttu-id="31f9d-148">Zapněte pí pomocí kabelu USB micro a napájení.</span><span class="sxs-lookup"><span data-stu-id="31f9d-148">Turn on Pi by using the micro USB cable and the power supply.</span></span>

![Zapnout](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> <span data-ttu-id="31f9d-150">Je důležité sady, který je minimálně použít napájení 2A a ujistěte se, že vaše malin má dostatek power fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="31f9d-150">It is important to use the power supply in the kit that is at least 2A to make sure that your Raspberry has enough power to work correctly.</span></span>

## <a name="enable-ssh"></a><span data-ttu-id="31f9d-151">Povolit SSH</span><span class="sxs-lookup"><span data-stu-id="31f9d-151">Enable SSH</span></span>
<span data-ttu-id="31f9d-152">Od verze v listopadu 2016 Raspbian má server SSH ve výchozím nastavení zakázaná.</span><span class="sxs-lookup"><span data-stu-id="31f9d-152">As of the November 2016 release, Raspbian has the SSH server disabled by default.</span></span> <span data-ttu-id="31f9d-153">Budete muset povolit ručně.</span><span class="sxs-lookup"><span data-stu-id="31f9d-153">You need to enable it manually.</span></span> <span data-ttu-id="31f9d-154">Můžete se podívat do [oficiální pokyny](https://www.raspberrypi.org/documentation/remote-access/ssh/) nebo připojit monitorování a přejděte na **Předvolby -> konfigurace platformy malin** povolit SSH.</span><span class="sxs-lookup"><span data-stu-id="31f9d-154">You can refer to the [official instructions](https://www.raspberrypi.org/documentation/remote-access/ssh/) or connect a monitor and go to **Preferences -> Raspberry Pi Configuration** to enable SSH.</span></span>

## <a name="connect-raspberry-pi-3-to-the-network"></a><span data-ttu-id="31f9d-155">Připojení k síti malin pí 3</span><span class="sxs-lookup"><span data-stu-id="31f9d-155">Connect Raspberry Pi 3 to the network</span></span>
<span data-ttu-id="31f9d-156">Pi můžete připojit ke kabelové síti nebo k bezdrátové síti.</span><span class="sxs-lookup"><span data-stu-id="31f9d-156">You can connect Pi to a wired network or to a wireless network.</span></span> <span data-ttu-id="31f9d-157">Ujistěte se, že platformy je připojený ke stejné síti jako počítače.</span><span class="sxs-lookup"><span data-stu-id="31f9d-157">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="31f9d-158">Například můžete pí připojit ke stejnému přepínači, který je počítač připojen k.</span><span class="sxs-lookup"><span data-stu-id="31f9d-158">For example, you can connect Pi to the same switch that your computer is connected to.</span></span>

### <a name="connect-to-a-wired-network"></a><span data-ttu-id="31f9d-159">Připojení k drátové síti</span><span class="sxs-lookup"><span data-stu-id="31f9d-159">Connect to a wired network</span></span>
<span data-ttu-id="31f9d-160">Kabel Ethernet slouží k připojení platformy k drátové síti.</span><span class="sxs-lookup"><span data-stu-id="31f9d-160">Use the Ethernet cable to connect Pi to your wired network.</span></span> <span data-ttu-id="31f9d-161">Dva LED na platformy zapnout, pokud připojení.</span><span class="sxs-lookup"><span data-stu-id="31f9d-161">The two LEDs on Pi turn on if the connection is established.</span></span>

![Připojení pomocí kabelu Ethernet](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-to-a-wireless-network"></a><span data-ttu-id="31f9d-163">Připojit k bezdrátové síti</span><span class="sxs-lookup"><span data-stu-id="31f9d-163">Connect to a wireless network</span></span>
<span data-ttu-id="31f9d-164">Postupujte podle [pokyny](https://www.raspberrypi.org/learning/software-guide/wifi/) z Foundation pí malin pro připojení k bezdrátové síti pí.</span><span class="sxs-lookup"><span data-stu-id="31f9d-164">Follow the [instructions](https://www.raspberrypi.org/learning/software-guide/wifi/) from the Raspberry Pi Foundation to connect Pi to your wireless network.</span></span> <span data-ttu-id="31f9d-165">Tyto pokyny vyžadují, abyste nejdřív připojit k pí monitorování a klávesnice.</span><span class="sxs-lookup"><span data-stu-id="31f9d-165">These instructions require you to first connect a monitor and a keyboard to Pi.</span></span>

## <a name="connect-the-led-to-pi"></a><span data-ttu-id="31f9d-166">Připojení DIODU PI</span><span class="sxs-lookup"><span data-stu-id="31f9d-166">Connect the LED to Pi</span></span>
<span data-ttu-id="31f9d-167">Chcete-li tuto úlohu dokončit, použijte [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), vodičům konektor Indikátor a se odpor.</span><span class="sxs-lookup"><span data-stu-id="31f9d-167">To complete this task, use the [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), the connector wires, the LED, and the resistor.</span></span> <span data-ttu-id="31f9d-168">Připojte je k [pro obecné účely vstupu a výstupu](https://www.raspberrypi.org/documentation/usage/gpio/) porty (GPIO) čísla pí.</span><span class="sxs-lookup"><span data-stu-id="31f9d-168">Connect them to the [general-purpose input/output](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) ports of Pi.</span></span>

![Breadboard DIODU a odpor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. <span data-ttu-id="31f9d-170">Připojit kratší rameno DIODU k **GPIO zem (Pin 6)**.</span><span class="sxs-lookup"><span data-stu-id="31f9d-170">Connect the shorter leg of the LED to **GPIO GND (Pin 6)**.</span></span>
2. <span data-ttu-id="31f9d-171">Připojte k jeden úsek se odpor delší rameno Indikátor.</span><span class="sxs-lookup"><span data-stu-id="31f9d-171">Connect the longer leg of the LED to one leg of the resistor.</span></span>
3. <span data-ttu-id="31f9d-172">Připojit rameno odpor k **GPIO 4 (Pin 7)**.</span><span class="sxs-lookup"><span data-stu-id="31f9d-172">Connect the other leg of the resistor to **GPIO 4 (Pin 7)**.</span></span>

<span data-ttu-id="31f9d-173">Všimněte si, že Indikátor polarita je důležité.</span><span class="sxs-lookup"><span data-stu-id="31f9d-173">Note that the LED polarity is important.</span></span> <span data-ttu-id="31f9d-174">Toto nastavení polarita se běžně označuje jako aktivní nízká.</span><span class="sxs-lookup"><span data-stu-id="31f9d-174">This polarity setting is commonly known as Active Low.</span></span>

![Uspořádání kolíků](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

<span data-ttu-id="31f9d-176">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="31f9d-176">Congratulations!</span></span> <span data-ttu-id="31f9d-177">Úspěšně jste nakonfigurovali pí.</span><span class="sxs-lookup"><span data-stu-id="31f9d-177">You've successfully configured Pi.</span></span>

## <a name="summary"></a><span data-ttu-id="31f9d-178">Souhrn</span><span class="sxs-lookup"><span data-stu-id="31f9d-178">Summary</span></span>
<span data-ttu-id="31f9d-179">V tomto článku jsme zjistili, jak nakonfigurovat tak, že instalace Raspbian, Pi připojení k síti a připojení DIODU PI pí.</span><span class="sxs-lookup"><span data-stu-id="31f9d-179">In this article, you’ve learned how to configure Pi by installing Raspbian, connecting Pi to a network, and connecting an LED to Pi.</span></span> <span data-ttu-id="31f9d-180">Všimněte si, že DIODU není ještě light nahoru.</span><span class="sxs-lookup"><span data-stu-id="31f9d-180">Note that the LED doesn't yet light up.</span></span> <span data-ttu-id="31f9d-181">Dalším krokem je instalace nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na platformy.</span><span class="sxs-lookup"><span data-stu-id="31f9d-181">The next task is to install the necessary tools and software in preparation for running a sample application on Pi.</span></span>

![Je připravená hardwaru](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a><span data-ttu-id="31f9d-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31f9d-183">Next steps</span></span>
[<span data-ttu-id="31f9d-184">Získat nástroje</span><span class="sxs-lookup"><span data-stu-id="31f9d-184">Get the tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

