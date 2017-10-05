---
title: "Malinová pí do cloudu (Node.js) - pí malin připojit ke službě Azure IoT Hub | Microsoft Docs"
description: "Pi malin připojte ke službě Azure IoT Hub malin pí k odesílání dat do cloudu Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure iot Malinová pi, malinová platformy iot hub, malinová pí odesílání dat do cloudu, malinová pí do cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.openlocfilehash: 956ed5ab0ed38ddebd978b35eb54bc96567f0d57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a><span data-ttu-id="9fc98-104">Připojení ke službě Azure IoT Hub (Node.js) Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="9fc98-104">Connect Raspberry Pi to Azure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="9fc98-105">V tomto kurzu zahájíte učení základní informace o práci s malin platformy, na kterém běží Raspbian.</span><span class="sxs-lookup"><span data-stu-id="9fc98-105">In this tutorial, you begin by learning the basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="9fc98-106">Pak zjistíte, jak bezproblémově připojení zařízení do cloudu pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="9fc98-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="9fc98-107">Ukázky jádro IoT Windows 10, najdete [Centrum vývojářů pro Windows](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="9fc98-107">For Windows 10 IoT Core samples, go to the [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="9fc98-108">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="9fc98-108">Don't have a kit yet?</span></span> <span data-ttu-id="9fc98-109">Zkuste [malin pí 3 emulátoru](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/).</span><span class="sxs-lookup"><span data-stu-id="9fc98-109">Try the [Raspberry Pi 3 emulator](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/).</span></span> <span data-ttu-id="9fc98-110">Nebo zakoupení nové kit [zde](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="9fc98-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="9fc98-111">Co dělat</span><span class="sxs-lookup"><span data-stu-id="9fc98-111">What you do</span></span>

* <span data-ttu-id="9fc98-112">Instalační program Malinová pí.</span><span class="sxs-lookup"><span data-stu-id="9fc98-112">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="9fc98-113">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9fc98-113">Create an IoT hub.</span></span>
* <span data-ttu-id="9fc98-114">Registrovat zařízení pro platformy ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9fc98-114">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="9fc98-115">Spuštění ukázkové aplikace na platformy k odesílání dat snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9fc98-115">Run a sample application on Pi to send sensor data to your IoT hub.</span></span>

<span data-ttu-id="9fc98-116">Pi malin připojení do služby IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="9fc98-116">Connect Raspberry Pi to an IoT hub that you create.</span></span> <span data-ttu-id="9fc98-117">Pak spusťte ukázkovou aplikaci na platformy ke shromažďování dat teploty a vlhkosti ze BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="9fc98-117">Then you run a sample application on Pi to collect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="9fc98-118">Nakonec odeslat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9fc98-118">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="9fc98-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="9fc98-119">What you learn</span></span>

* <span data-ttu-id="9fc98-120">Postup vytvoření služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="9fc98-120">How to create an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="9fc98-121">Postup připojení pí s BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="9fc98-121">How to connect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="9fc98-122">Postup shromažďování dat snímačů spuštěním ukázkovou aplikaci na pí.</span><span class="sxs-lookup"><span data-stu-id="9fc98-122">How to collect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="9fc98-123">Jak odesílat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9fc98-123">How to send sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="9fc98-124">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="9fc98-124">What you need</span></span>

![Co potřebujete](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* <span data-ttu-id="9fc98-126">Malin pí 2 nebo 3 pí malin panelu.</span><span class="sxs-lookup"><span data-stu-id="9fc98-126">The Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="9fc98-127">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9fc98-127">An active Azure subscription.</span></span> <span data-ttu-id="9fc98-128">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="9fc98-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="9fc98-129">Monitorování, USB klávesnice a myši připojujících se k pí.</span><span class="sxs-lookup"><span data-stu-id="9fc98-129">A monitor, a USB keyboard, and mouse that connect to Pi.</span></span>
* <span data-ttu-id="9fc98-130">Mac nebo počítači se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="9fc98-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="9fc98-131">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="9fc98-131">An Internet connection.</span></span>
* <span data-ttu-id="9fc98-132">16 GB nebo vyšší microSD karta.</span><span class="sxs-lookup"><span data-stu-id="9fc98-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="9fc98-133">USB-adaptér nebo microSD karta SD vypálíte image operačního systému na kartě microSD.</span><span class="sxs-lookup"><span data-stu-id="9fc98-133">A USB-SD adapter or microSD card to burn the operating system image onto the microSD card.</span></span>
* <span data-ttu-id="9fc98-134">5 volt 2 amp napájení s 6 stopy malých kabel USB.</span><span class="sxs-lookup"><span data-stu-id="9fc98-134">A 5-volt 2-amp power supply with the 6-foot micro USB cable.</span></span>

<span data-ttu-id="9fc98-135">Následující položky jsou volitelné:</span><span class="sxs-lookup"><span data-stu-id="9fc98-135">The following items are optional:</span></span>

* <span data-ttu-id="9fc98-136">Sestavený Adafruit BME280 teploty, naléhavost a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="9fc98-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="9fc98-137">Breadboard.</span><span class="sxs-lookup"><span data-stu-id="9fc98-137">A breadboard.</span></span>
* <span data-ttu-id="9fc98-138">Vodičům můstek 6 F/M.</span><span class="sxs-lookup"><span data-stu-id="9fc98-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="9fc98-139">Rozptýlený DIODU 10 mm.</span><span class="sxs-lookup"><span data-stu-id="9fc98-139">A diffused 10-mm LED.</span></span>

  > [!NOTE] 
  <span data-ttu-id="9fc98-140">Tyto položky jsou volitelné, protože data snímačů simulated podporu ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="9fc98-140">These items are optional because the code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="9fc98-141">Instalační program Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="9fc98-141">Setup Raspberry Pi</span></span>

### <a name="install-the-raspbian-operating-system-for-pi"></a><span data-ttu-id="9fc98-142">Instalace operačního systému Raspbian pí</span><span class="sxs-lookup"><span data-stu-id="9fc98-142">Install the Raspbian operating system for Pi</span></span>

<span data-ttu-id="9fc98-143">Připravte karty microSD pro instalaci bitové kopie Raspbian.</span><span class="sxs-lookup"><span data-stu-id="9fc98-143">Prepare the microSD card for installation of the Raspbian image.</span></span>

1. <span data-ttu-id="9fc98-144">Stáhněte si Raspbian.</span><span class="sxs-lookup"><span data-stu-id="9fc98-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="9fc98-145">[Stáhnout Raspbian Klára s pixelů](https://www.raspberrypi.org/downloads/raspbian/) (soubor .zip).</span><span class="sxs-lookup"><span data-stu-id="9fc98-145">[Download Raspbian Jessie with Pixel](https://www.raspberrypi.org/downloads/raspbian/) (the .zip file).</span></span>
   1. <span data-ttu-id="9fc98-146">Extrahujte Raspbian image do složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="9fc98-146">Extract the Raspbian image to a folder on your computer.</span></span>
1. <span data-ttu-id="9fc98-147">Nainstalujte Raspbian microSD karta.</span><span class="sxs-lookup"><span data-stu-id="9fc98-147">Install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="9fc98-148">[Stáhněte a nainstalujte nástroj Etcher SD karty zapisovací jednotka](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="9fc98-148">[Download and install the Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="9fc98-149">Spusťte Etcher a vyberte Raspbian bitovou kopii, která jste extrahovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="9fc98-149">Run Etcher and select the Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="9fc98-150">Vyberte jednotku microSD karta.</span><span class="sxs-lookup"><span data-stu-id="9fc98-150">Select the microSD card drive.</span></span> <span data-ttu-id="9fc98-151">Všimněte si, že Etcher může jste již vybrali správnou jednotku.</span><span class="sxs-lookup"><span data-stu-id="9fc98-151">Note that Etcher may have already selected the correct drive.</span></span>
   1. <span data-ttu-id="9fc98-152">Klikněte na tlačítko nainstalovat Raspbian do karty microSD Flash.</span><span class="sxs-lookup"><span data-stu-id="9fc98-152">Click Flash to install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="9fc98-153">Karta microSD odeberte z počítače, po dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="9fc98-153">Remove the microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="9fc98-154">Je bezpečné karty microSD přímo odebrat, protože Etcher automaticky vysune nebo odpojí microSD karta po dokončení.</span><span class="sxs-lookup"><span data-stu-id="9fc98-154">It's safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.</span></span>
   1. <span data-ttu-id="9fc98-155">Karta microSD vložte do pí.</span><span class="sxs-lookup"><span data-stu-id="9fc98-155">Insert the microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="9fc98-156">Povolit SSH a I2C</span><span class="sxs-lookup"><span data-stu-id="9fc98-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="9fc98-157">Připojit k monitoru, klávesnice a myši platformy, spusťte platformy a znovu přihlásili Raspbian pomocí `pi` jako uživatelské jméno a `raspberry` jako heslo.</span><span class="sxs-lookup"><span data-stu-id="9fc98-157">Connect Pi to the monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as the user name and `raspberry` as the password.</span></span>
1. <span data-ttu-id="9fc98-158">Klikněte na ikonu Malinová > **Předvolby** > **malin pí konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="9fc98-158">Click the Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![V nabídce Raspbian předvolby](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="9fc98-160">Na **rozhraní** nastavte **I2C** a **SSH** k **povolit**a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9fc98-160">On the **Interfaces** tab, set **I2C** and **SSH** to **Enable**, and then click **OK**.</span></span> <span data-ttu-id="9fc98-161">Pokud nemáte fyzické senzory a chcete použít data simulované snímačů, tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="9fc98-161">If you don't have physical sensors and want to use simulated sensor data, this step is optional.</span></span>

   ![Povolit I2C a SSH na Malinová platformy](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="9fc98-163">SSH a I2C povolit, můžete najít další dokumenty odkaz na [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) a [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span><span class="sxs-lookup"><span data-stu-id="9fc98-163">To enable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span></span>

### <a name="connect-the-sensor-to-pi"></a><span data-ttu-id="9fc98-164">Připojit senzoru PI</span><span class="sxs-lookup"><span data-stu-id="9fc98-164">Connect the sensor to Pi</span></span>

<span data-ttu-id="9fc98-165">Pomocí vedení breadboard a můstek pro připojení DIODU a BME280 PI následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="9fc98-165">Use the breadboard and jumper wires to connect an LED and a BME280 to Pi as follows.</span></span> <span data-ttu-id="9fc98-166">Pokud nemáte senzoru, tuto část přeskočte.</span><span class="sxs-lookup"><span data-stu-id="9fc98-166">If you don’t have the sensor, skip this section.</span></span>

![Připojení malin platformy a senzor](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="9fc98-168">BME280 senzoru teploty a vlhkosti data můžete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="9fc98-168">The BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="9fc98-169">A DIODU bude blink – Pokud je komunikace mezi zařízením a cloudem.</span><span class="sxs-lookup"><span data-stu-id="9fc98-169">And the LED will blink if there is a communication between device and the cloud.</span></span> 

<span data-ttu-id="9fc98-170">Senzor kód PIN použijte následující kabeláž:</span><span class="sxs-lookup"><span data-stu-id="9fc98-170">For sensor pins, use the following wiring:</span></span>

| <span data-ttu-id="9fc98-171">Spuštění (senzor & DIODU)</span><span class="sxs-lookup"><span data-stu-id="9fc98-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="9fc98-172">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="9fc98-172">End (Board)</span></span>            | <span data-ttu-id="9fc98-173">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="9fc98-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="9fc98-174">VDD (Pin 5G)</span><span class="sxs-lookup"><span data-stu-id="9fc98-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="9fc98-175">3.3v opravná (Pin 1)</span><span class="sxs-lookup"><span data-stu-id="9fc98-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="9fc98-176">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="9fc98-176">White cable</span></span>   |
| <span data-ttu-id="9fc98-177">ZEM (Pin 7G)</span><span class="sxs-lookup"><span data-stu-id="9fc98-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="9fc98-178">ZEM (Pin 6)</span><span class="sxs-lookup"><span data-stu-id="9fc98-178">GND (Pin 6)</span></span>            | <span data-ttu-id="9fc98-179">Hnědá kabel</span><span class="sxs-lookup"><span data-stu-id="9fc98-179">Brown cable</span></span>   |
| <span data-ttu-id="9fc98-180">SCK (Pin 8G)</span><span class="sxs-lookup"><span data-stu-id="9fc98-180">SCK (Pin 8G)</span></span>             | <span data-ttu-id="9fc98-181">I2C1 SDA (PIN kódu 3)</span><span class="sxs-lookup"><span data-stu-id="9fc98-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="9fc98-182">Oranžové kabel</span><span class="sxs-lookup"><span data-stu-id="9fc98-182">Orange cable</span></span>  |
| <span data-ttu-id="9fc98-183">SDI (Pin 10G)</span><span class="sxs-lookup"><span data-stu-id="9fc98-183">SDI (Pin 10G)</span></span>            | <span data-ttu-id="9fc98-184">I2C1 SCL (Pin 5)</span><span class="sxs-lookup"><span data-stu-id="9fc98-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="9fc98-185">Red kabel</span><span class="sxs-lookup"><span data-stu-id="9fc98-185">Red cable</span></span>     |
| <span data-ttu-id="9fc98-186">Indikátor VDD (Pin 18F)</span><span class="sxs-lookup"><span data-stu-id="9fc98-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="9fc98-187">GPIO 24 (Pin 18)</span><span class="sxs-lookup"><span data-stu-id="9fc98-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="9fc98-188">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="9fc98-188">White cable</span></span>   |
| <span data-ttu-id="9fc98-189">Indikátor zem (Pin 17F)</span><span class="sxs-lookup"><span data-stu-id="9fc98-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="9fc98-190">ZEM (Pin 20)</span><span class="sxs-lookup"><span data-stu-id="9fc98-190">GND (Pin 20)</span></span>           | <span data-ttu-id="9fc98-191">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="9fc98-191">Black cable</span></span>   |

<span data-ttu-id="9fc98-192">Kliknutím zobrazíte [malin pí 2 a 3 mapování kódu Pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) pro vaši informaci.</span><span class="sxs-lookup"><span data-stu-id="9fc98-192">Click to view [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="9fc98-193">Po připojení BME280 úspěšně vaší malin PI, mělo by být jako níže bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="9fc98-193">After you've successfully connected BME280 to your Raspberry Pi, it should be like below image.</span></span>

![Připojené platformy a BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a><span data-ttu-id="9fc98-195">Připojení k síti platformy</span><span class="sxs-lookup"><span data-stu-id="9fc98-195">Connect Pi to the network</span></span>

<span data-ttu-id="9fc98-196">Zapněte pí pomocí kabelu USB micro a napájení.</span><span class="sxs-lookup"><span data-stu-id="9fc98-196">Turn on Pi by using the micro USB cable and the power supply.</span></span> <span data-ttu-id="9fc98-197">Pomocí kabelu Ethernet připojit platformy k drátové síti nebo postupujte podle [pokyny z Foundation pí malin](https://www.raspberrypi.org/learning/software-guide/wifi/) pro připojení k bezdrátové síti pí.</span><span class="sxs-lookup"><span data-stu-id="9fc98-197">Use the Ethernet cable to connect Pi to your wired network or follow the [instructions from the Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) to connect Pi to your wireless network.</span></span> <span data-ttu-id="9fc98-198">Po vaší platformy se úspěšně připojil k síti, budete muset poznamenejte [IP adresa vašeho čísla pí](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="9fc98-198">After your Pi has been successfully connected to the network, you need to take a note of the [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Připojení k drátové síti](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="9fc98-200">Ujistěte se, že platformy je připojený ke stejné síti jako počítače.</span><span class="sxs-lookup"><span data-stu-id="9fc98-200">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="9fc98-201">Například pokud je počítač připojen k bezdrátové síti a platformy je připojeno k drátové síti, nemusíte to vidět IP adresu ve výstupu devdisco.</span><span class="sxs-lookup"><span data-stu-id="9fc98-201">For example, if your computer is connected to a wireless network while Pi is connected to a wired network, you might not see the IP address in the devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="9fc98-202">Spuštění ukázkové aplikace na platformy</span><span class="sxs-lookup"><span data-stu-id="9fc98-202">Run a sample application on Pi</span></span>

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a><span data-ttu-id="9fc98-203">Naklonujte ukázkovou aplikaci a nainstalujte požadované balíčky</span><span class="sxs-lookup"><span data-stu-id="9fc98-203">Clone sample application and install the prerequisite packages</span></span>

1. <span data-ttu-id="9fc98-204">Použijte jeden z následujících klientů SSH z hostitelského počítače pro připojení k vaší malin platformy.</span><span class="sxs-lookup"><span data-stu-id="9fc98-204">Use one of the following SSH clients from your host computer to connect to your Raspberry Pi.</span></span>
    - <span data-ttu-id="9fc98-205">[PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="9fc98-205">[PuTTY](http://www.putty.org/) for Windows.</span></span> <span data-ttu-id="9fc98-206">Je třeba IP adresa vašeho čísla pí připojit pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="9fc98-206">You need the IP address of your Pi to connect it via SSH.</span></span>
    - <span data-ttu-id="9fc98-207">Integrovaného klienta SSH na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="9fc98-207">The built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="9fc98-208">Stačí spustit `ssh pi@<ip address of pi>` připojit platformy prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="9fc98-208">You might need run `ssh pi@<ip address of pi>` to connect Pi via SSH.</span></span>

   > [!NOTE] 
   <span data-ttu-id="9fc98-209">Výchozí uživatelské jméno `pi` , a heslo je `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="9fc98-209">The default username is `pi` , and the password is `raspberry`.</span></span>

1. <span data-ttu-id="9fc98-210">Nainstalujte Node.js a NPM pro vaše platformy.</span><span class="sxs-lookup"><span data-stu-id="9fc98-210">Install Node.js and NPM to your Pi.</span></span>
   
   <span data-ttu-id="9fc98-211">Nejdřív byste měli zkontrolovat vaše verze Node.js pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fc98-211">First you should check your Node.js version with the following command.</span></span> 
   
   ```bash
   node -v
   ```

   <span data-ttu-id="9fc98-212">Pokud verze je nižší než 4.x nebo není žádná Node.js na vaše Pi, spusťte následující příkaz pro instalaci nebo aktualizaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="9fc98-212">If the version is lower than 4.x or there is no Node.js on your Pi, then run the following command to install or update Node.js.</span></span>

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. <span data-ttu-id="9fc98-213">Naklonujte ukázkovou aplikaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9fc98-213">Clone the sample application by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. <span data-ttu-id="9fc98-214">Nainstalujte všechny balíčky podle následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="9fc98-214">Install all packages by the following command.</span></span> <span data-ttu-id="9fc98-215">Obsahuje zařízení Azure IoT SDK, BME280 senzor knihovny a knihovna vzájemné propojení pí.</span><span class="sxs-lookup"><span data-stu-id="9fc98-215">It includes Azure IoT device SDK, BME280 Sensor library and Wiring Pi library.</span></span>

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   <span data-ttu-id="9fc98-216">Může trvat několik minut na dokončení této instalace proces denpening na připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="9fc98-216">It might take several minutes to finish this installation process denpening on your network connection.</span></span>

### <a name="configure-the-sample-application"></a><span data-ttu-id="9fc98-217">Nakonfigurujte ukázkovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="9fc98-217">Configure the sample application</span></span>

1. <span data-ttu-id="9fc98-218">Otevřete konfigurační soubor spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="9fc98-218">Open the config file by running the following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![Konfigurační soubor](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   <span data-ttu-id="9fc98-220">Existují dvě položky v tomto souboru můžete configurate.</span><span class="sxs-lookup"><span data-stu-id="9fc98-220">There are two items in this file you can configurate.</span></span> <span data-ttu-id="9fc98-221">První z nich je `interval`, která definuje časový interval mezi dvě zprávy, které odesílají do cloudu.</span><span class="sxs-lookup"><span data-stu-id="9fc98-221">The first one is `interval`, which defines the time interval between two messages that send to cloud.</span></span> <span data-ttu-id="9fc98-222">Druhý `simulatedData`, což je logickou hodnotu pro jestli se má používat data simulované snímačů, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="9fc98-222">The second one `simulatedData`,which is a Boolean value for whether to use simulated sensor data or not.</span></span>

   <span data-ttu-id="9fc98-223">Pokud jste **nemají senzoru**, nastavte `simulatedData` hodnotu `true` aby ukázkovou aplikaci, vytváření a používání dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="9fc98-223">If you **don't have the sensor**, set the `simulatedData` value to `true` to make the sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="9fc98-224">Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="9fc98-224">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="run-the-sample-application"></a><span data-ttu-id="9fc98-225">Spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="9fc98-225">Run the sample application</span></span>

1. <span data-ttu-id="9fc98-226">Spuštění ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9fc98-226">Run the sample application by running the following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="9fc98-227">Zkontrolujte, zda jste způsobené kopírováním a vkládáním zařízení připojovací řetězec do jednoduchých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="9fc98-227">Make sure you copy-paste the device connection string into the single quotes.</span></span>


<span data-ttu-id="9fc98-228">Měli byste vidět následující výstup, který popisuje data snímačů a zprávy, které se odesílají do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9fc98-228">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

![Výstup – data snímačů odeslaný malin pí do služby IoT hub](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="9fc98-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9fc98-230">Next steps</span></span>

<span data-ttu-id="9fc98-231">Spustíte ukázkovou aplikaci pro shromažďování dat snímačů a odeslat do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9fc98-231">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]