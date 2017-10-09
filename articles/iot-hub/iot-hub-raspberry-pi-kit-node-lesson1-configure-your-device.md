---
title: "Připojit malin platformy (uzel) tooAzure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte malin pí 3 pro první použití a nainstalujte hello Raspbian OS, volné operační systém, který je optimalizovaná pro hello malin pí hardwaru."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "instalace raspbian, raspbian stažení způsob tooinstall raspbian raspbian instalační program, malinová platformy instalace raspbian Malinová pí instalace operačního systému, malinová pí sd karty instalace, malinová pí připojení, připojit tooraspberry pi, malinová pí připojení"
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
ms.openlocfilehash: 504a4d2a3f29717f955530812442cce2a78a6448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="053cb-104">Konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="053cb-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="053cb-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="053cb-105">What you will do</span></span>
<span data-ttu-id="053cb-106">Nakonfigurujte platformy pro první použití a nainstalujte hello Raspbian operačního systému.</span><span class="sxs-lookup"><span data-stu-id="053cb-106">Configure Pi for first-time use and install hello Raspbian operating system.</span></span> <span data-ttu-id="053cb-107">Raspbian je volné operační systém, která je optimalizovaná pro hello malin pí hardwaru.</span><span class="sxs-lookup"><span data-stu-id="053cb-107">Raspbian is a free operating system that is optimized for hello Raspberry Pi hardware.</span></span> <span data-ttu-id="053cb-108">Pokud máte potíže, můžete hledat řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="053cb-108">If you have any problems, you can seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="053cb-109">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="053cb-109">What you will learn</span></span>
<span data-ttu-id="053cb-110">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="053cb-110">In this article, you will learn:</span></span>

* <span data-ttu-id="053cb-111">Jak tooinstall Raspbian na pí.</span><span class="sxs-lookup"><span data-stu-id="053cb-111">How tooinstall Raspbian on Pi.</span></span>
* <span data-ttu-id="053cb-112">Jak toopower až pí pomocí kabelu USB.</span><span class="sxs-lookup"><span data-stu-id="053cb-112">How toopower up Pi by using a USB cable.</span></span>
* <span data-ttu-id="053cb-113">Jak tooconnect pí toohello sítě pomocí kabelu Ethernet nebo bezdrátové sítě.</span><span class="sxs-lookup"><span data-stu-id="053cb-113">How tooconnect Pi toohello network by using an Ethernet cable or wireless network.</span></span>
* <span data-ttu-id="053cb-114">Jak tooadd toohello DIODU breadboard a připojte ho tooPi.</span><span class="sxs-lookup"><span data-stu-id="053cb-114">How tooadd an LED toohello breadboard and connect it tooPi.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="053cb-115">Co budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="053cb-115">What you will need</span></span>
<span data-ttu-id="053cb-116">toocomplete tuto operaci je třeba hello následujících částí z vaší malin pí 3 Starter Kit:</span><span class="sxs-lookup"><span data-stu-id="053cb-116">toocomplete this operation, you need hello following parts from your Raspberry Pi 3 Starter Kit:</span></span>

* <span data-ttu-id="053cb-117">Hello Tabule malin pí 3</span><span class="sxs-lookup"><span data-stu-id="053cb-117">hello Raspberry Pi 3 board</span></span>
* <span data-ttu-id="053cb-118">Karta microSD 16 GB Hello</span><span class="sxs-lookup"><span data-stu-id="053cb-118">hello 16-GB microSD card</span></span>
* <span data-ttu-id="053cb-119">Hello 5 volt 2 amp napájení pomocí kabelu USB malých 6 stopy hello</span><span class="sxs-lookup"><span data-stu-id="053cb-119">hello 5-volt 2-amp power supply with hello 6-foot micro USB cable</span></span>
* <span data-ttu-id="053cb-120">Hello breadboard</span><span class="sxs-lookup"><span data-stu-id="053cb-120">hello breadboard</span></span>
* <span data-ttu-id="053cb-121">Konektor vodičům</span><span class="sxs-lookup"><span data-stu-id="053cb-121">Connector wires</span></span>
* <span data-ttu-id="053cb-122">560 ohm odpor</span><span class="sxs-lookup"><span data-stu-id="053cb-122">A 560-ohm resistor</span></span>
* <span data-ttu-id="053cb-123">Rozptýlený DIODU 10 mm</span><span class="sxs-lookup"><span data-stu-id="053cb-123">A diffused 10-mm LED</span></span>
* <span data-ttu-id="053cb-124">Hello kabel Ethernet</span><span class="sxs-lookup"><span data-stu-id="053cb-124">hello Ethernet cable</span></span>

![Co v Startovní sady](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

<span data-ttu-id="053cb-126">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="053cb-126">You also need:</span></span>

* <span data-ttu-id="053cb-127">Pevné nebo bezdrátové připojení pro platformy tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="053cb-127">A wired or wireless connection for Pi tooconnect to.</span></span>
* <span data-ttu-id="053cb-128">USB SD adaptér nebo miniSD karty tooburn hello image operačního systému na kartě microSD hello.</span><span class="sxs-lookup"><span data-stu-id="053cb-128">A USB-SD adapter or miniSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="053cb-129">Počítač se systémem Windows, Mac nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="053cb-129">A computer running Windows, Mac, or Linux.</span></span> <span data-ttu-id="053cb-130">počítač Hello je použité tooinstall Raspbian na kartě microSD hello.</span><span class="sxs-lookup"><span data-stu-id="053cb-130">hello computer is used tooinstall Raspbian on hello microSD card.</span></span>
* <span data-ttu-id="053cb-131">Toodownload připojení Internetu hello nezbytné nástroje a software.</span><span class="sxs-lookup"><span data-stu-id="053cb-131">An Internet connection toodownload hello necessary tools and software.</span></span>

## <a name="install-raspbian-on-hello-microsd-card"></a><span data-ttu-id="053cb-132">Nainstalujte Raspbian na kartě microSD hello</span><span class="sxs-lookup"><span data-stu-id="053cb-132">Install Raspbian on hello microSD card</span></span>
<span data-ttu-id="053cb-133">Připravte hello microSD karta pro instalaci bitové kopie Raspbian hello.</span><span class="sxs-lookup"><span data-stu-id="053cb-133">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="053cb-134">Stáhněte si Raspbian.</span><span class="sxs-lookup"><span data-stu-id="053cb-134">Download Raspbian.</span></span>
   1. <span data-ttu-id="053cb-135">[Stáhněte si](https://www.raspberrypi.org/downloads/raspbian/) hello soubor .zip pro Raspbian Klára s pixelů.</span><span class="sxs-lookup"><span data-stu-id="053cb-135">[Download](https://www.raspberrypi.org/downloads/raspbian/) hello .zip file for Raspbian Jessie with Pixel.</span></span>
   2. <span data-ttu-id="053cb-136">Extrahujte hello Raspbian image tooa složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="053cb-136">Extract hello Raspbian image tooa folder on your computer.</span></span>
2. <span data-ttu-id="053cb-137">Nainstalujte Raspbian toohello microSD karta.</span><span class="sxs-lookup"><span data-stu-id="053cb-137">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="053cb-138">[Stáhněte si](https://www.etcher.io) a nainstalujte nástroj hello Etcher SD karty zapisovací jednotka.</span><span class="sxs-lookup"><span data-stu-id="053cb-138">[Download](https://www.etcher.io) and install hello Etcher SD card burner utility.</span></span>
   2. <span data-ttu-id="053cb-139">Spusťte Etcher a vyberte bitovou kopii hello Raspbian, které jste extrahovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="053cb-139">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   3. <span data-ttu-id="053cb-140">Vyberte jednotku karty microSD hello.</span><span class="sxs-lookup"><span data-stu-id="053cb-140">Select hello microSD card drive.</span></span>
      <span data-ttu-id="053cb-141">Všimněte si, že Etcher může jste již vybrali správnou jednotku hello.</span><span class="sxs-lookup"><span data-stu-id="053cb-141">Note that Etcher may have already selected hello correct drive.</span></span>
   4. <span data-ttu-id="053cb-142">Klikněte na tlačítko **Flash** tooinstall Raspbian toohello microSD karta.</span><span class="sxs-lookup"><span data-stu-id="053cb-142">Click **Flash** tooinstall Raspbian toohello microSD card.</span></span>
   5. <span data-ttu-id="053cb-143">Po dokončení instalace odeberte hello microSD karta z vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="053cb-143">Remove hello microSD card from your computer when installation is complete.</span></span>
      <span data-ttu-id="053cb-144">Je bezpečné tooremove hello microSD karta přímo protože Etcher automaticky vysune nebo odpojí hello microSD karta po dokončení.</span><span class="sxs-lookup"><span data-stu-id="053cb-144">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   6. <span data-ttu-id="053cb-145">Karta microSD hello vložte do pí.</span><span class="sxs-lookup"><span data-stu-id="053cb-145">Insert hello microSD card into Pi.</span></span>

![Vložit hello SD karty](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a><span data-ttu-id="053cb-147">Zapnout platformy</span><span class="sxs-lookup"><span data-stu-id="053cb-147">Turn on Pi</span></span>
<span data-ttu-id="053cb-148">Zapněte pí pomocí kabelu USB malých hello a hello napájení.</span><span class="sxs-lookup"><span data-stu-id="053cb-148">Turn on Pi by using hello micro USB cable and hello power supply.</span></span>

![Zapnout](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> <span data-ttu-id="053cb-150">Je důležité toouse hello napájení v hello kit, která je aspoň 2A toomake se, že vaše malin má dostatek power toowork správně.</span><span class="sxs-lookup"><span data-stu-id="053cb-150">It is important toouse hello power supply in hello kit that is at least 2A toomake sure that your Raspberry has enough power toowork correctly.</span></span>

## <a name="enable-ssh"></a><span data-ttu-id="053cb-151">Povolit SSH</span><span class="sxs-lookup"><span data-stu-id="053cb-151">Enable SSH</span></span>
<span data-ttu-id="053cb-152">Od verze hello verze v listopadu 2016 má Raspbian hello SSH serveru ve výchozím nastavení zakázaná.</span><span class="sxs-lookup"><span data-stu-id="053cb-152">As of hello November 2016 release, Raspbian has hello SSH server disabled by default.</span></span> <span data-ttu-id="053cb-153">Je třeba tooenable ji ručně.</span><span class="sxs-lookup"><span data-stu-id="053cb-153">You need tooenable it manually.</span></span> <span data-ttu-id="053cb-154">Může odkazovat toohello [oficiální pokyny](https://www.raspberrypi.org/documentation/remote-access/ssh/) nebo připojit monitorování a přejděte příliš**Předvolby -> konfigurace platformy malin** tooenable SSH.</span><span class="sxs-lookup"><span data-stu-id="053cb-154">You can refer toohello [official instructions](https://www.raspberrypi.org/documentation/remote-access/ssh/) or connect a monitor and go too**Preferences -> Raspberry Pi Configuration** tooenable SSH.</span></span>

## <a name="connect-raspberry-pi-3-toohello-network"></a><span data-ttu-id="053cb-155">Připojit síť toohello malin pí 3</span><span class="sxs-lookup"><span data-stu-id="053cb-155">Connect Raspberry Pi 3 toohello network</span></span>
<span data-ttu-id="053cb-156">Pi tooa drátové síti nebo tooa bezdrátové síti se můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="053cb-156">You can connect Pi tooa wired network or tooa wireless network.</span></span> <span data-ttu-id="053cb-157">Ujistěte se, že platformy je připojený toohello stejné sítě jako váš počítač.</span><span class="sxs-lookup"><span data-stu-id="053cb-157">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="053cb-158">Například se můžete připojit toohello Pi, stejný přepínač, že je počítač připojen k.</span><span class="sxs-lookup"><span data-stu-id="053cb-158">For example, you can connect Pi toohello same switch that your computer is connected to.</span></span>

### <a name="connect-tooa-wired-network"></a><span data-ttu-id="053cb-159">Připojit tooa drátové síti</span><span class="sxs-lookup"><span data-stu-id="053cb-159">Connect tooa wired network</span></span>
<span data-ttu-id="053cb-160">Použijte hello Ethernet kabel tooconnect pí tooyour drátové síti.</span><span class="sxs-lookup"><span data-stu-id="053cb-160">Use hello Ethernet cable tooconnect Pi tooyour wired network.</span></span> <span data-ttu-id="053cb-161">Hello dvě LED na platformy zapnout, pokud hello připojení.</span><span class="sxs-lookup"><span data-stu-id="053cb-161">hello two LEDs on Pi turn on if hello connection is established.</span></span>

![Připojení pomocí kabelu Ethernet](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-tooa-wireless-network"></a><span data-ttu-id="053cb-163">Připojit tooa bezdrátové sítě</span><span class="sxs-lookup"><span data-stu-id="053cb-163">Connect tooa wireless network</span></span>
<span data-ttu-id="053cb-164">Postupujte podle hello [pokyny](https://www.raspberrypi.org/learning/software-guide/wifi/) z hello malin platformy Foundation tooconnect pí tooyour bezdrátové sítě.</span><span class="sxs-lookup"><span data-stu-id="053cb-164">Follow hello [instructions](https://www.raspberrypi.org/learning/software-guide/wifi/) from hello Raspberry Pi Foundation tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="053cb-165">Tyto pokyny vyžadují toofirst připojit monitorování a tooPi klávesnice.</span><span class="sxs-lookup"><span data-stu-id="053cb-165">These instructions require you toofirst connect a monitor and a keyboard tooPi.</span></span>

## <a name="connect-hello-led-toopi"></a><span data-ttu-id="053cb-166">Připojit hello DIODU tooPi</span><span class="sxs-lookup"><span data-stu-id="053cb-166">Connect hello LED tooPi</span></span>
<span data-ttu-id="053cb-167">toocomplete tento úkol, použijte hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello vodičům konektor, hello DIODU a hello odpor.</span><span class="sxs-lookup"><span data-stu-id="053cb-167">toocomplete this task, use hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello connector wires, hello LED, and hello resistor.</span></span> <span data-ttu-id="053cb-168">Připojte je toohello [pro obecné účely vstupu a výstupu](https://www.raspberrypi.org/documentation/usage/gpio/) porty (GPIO) čísla pí.</span><span class="sxs-lookup"><span data-stu-id="053cb-168">Connect them toohello [general-purpose input/output](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) ports of Pi.</span></span>

![Breadboard DIODU a odpor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. <span data-ttu-id="053cb-170">Připojit hello kratší rameno hello DIODU příliš**GPIO zem (Pin 6)**.</span><span class="sxs-lookup"><span data-stu-id="053cb-170">Connect hello shorter leg of hello LED too**GPIO GND (Pin 6)**.</span></span>
2. <span data-ttu-id="053cb-171">Připojte hello delší rameno hello DIODU tooone rameno odpor hello.</span><span class="sxs-lookup"><span data-stu-id="053cb-171">Connect hello longer leg of hello LED tooone leg of hello resistor.</span></span>
3. <span data-ttu-id="053cb-172">Připojit hello jiných rameno hello odpor příliš**GPIO 4 (Pin 7)**.</span><span class="sxs-lookup"><span data-stu-id="053cb-172">Connect hello other leg of hello resistor too**GPIO 4 (Pin 7)**.</span></span>

<span data-ttu-id="053cb-173">Všimněte si, že hello DIODU polarita je důležité.</span><span class="sxs-lookup"><span data-stu-id="053cb-173">Note that hello LED polarity is important.</span></span> <span data-ttu-id="053cb-174">Toto nastavení polarita se běžně označuje jako aktivní nízká.</span><span class="sxs-lookup"><span data-stu-id="053cb-174">This polarity setting is commonly known as Active Low.</span></span>

![Uspořádání kolíků](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

<span data-ttu-id="053cb-176">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="053cb-176">Congratulations!</span></span> <span data-ttu-id="053cb-177">Úspěšně jste nakonfigurovali pí.</span><span class="sxs-lookup"><span data-stu-id="053cb-177">You've successfully configured Pi.</span></span>

## <a name="summary"></a><span data-ttu-id="053cb-178">Souhrn</span><span class="sxs-lookup"><span data-stu-id="053cb-178">Summary</span></span>
<span data-ttu-id="053cb-179">V tomto článku, když jste se naučili jak tooconfigure pí Raspbian připojování sítě tooa Pi, instalace a připojením tooPi DIODU.</span><span class="sxs-lookup"><span data-stu-id="053cb-179">In this article, you’ve learned how tooconfigure Pi by installing Raspbian, connecting Pi tooa network, and connecting an LED tooPi.</span></span> <span data-ttu-id="053cb-180">Všimněte si, že hello DIODU není ještě light nahoru.</span><span class="sxs-lookup"><span data-stu-id="053cb-180">Note that hello LED doesn't yet light up.</span></span> <span data-ttu-id="053cb-181">Další úlohou Hello je tooinstall hello nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na platformy.</span><span class="sxs-lookup"><span data-stu-id="053cb-181">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on Pi.</span></span>

![Je připravená hardwaru](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a><span data-ttu-id="053cb-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="053cb-183">Next steps</span></span>
[<span data-ttu-id="053cb-184">Získat nástroje hello</span><span class="sxs-lookup"><span data-stu-id="053cb-184">Get hello tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

