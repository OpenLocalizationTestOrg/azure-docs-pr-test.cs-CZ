---
title: "Malinová pí do cloudu (C) - pí malin připojit ke službě Azure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak nastavit a připojení k Azure IoT Hub malin pí k odesílání dat do Azure Cloudová platforma v tomto kurzu malin pí."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot Malinová pi, malinová platformy iot hub, malinová pí odesílání dat do cloudu, malinová pí do cloudu"
ms.assetid: 68c0e730-1dc8-4e26-ac6b-573b217b302d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/12/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b8fda17a8d1d1796d5299e3aba4b0fd5e719a4c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-c"></a><span data-ttu-id="ab8e7-104">Připojení k Azure IoT Hub (C) Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="ab8e7-104">Connect Raspberry Pi to Azure IoT Hub (C)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="ab8e7-105">V tomto kurzu zahájíte učení základní informace o práci s malin platformy, na kterém běží Raspbian.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-105">In this tutorial, you begin by learning the basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="ab8e7-106">Pak zjistíte, jak bezproblémově připojení zařízení do cloudu pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="ab8e7-107">Ukázky jádro IoT Windows 10, najdete [Centrum vývojářů pro Windows](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-107">For Windows 10 IoT Core samples, go to the [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="ab8e7-108">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="ab8e7-108">Don't have a kit yet?</span></span> <span data-ttu-id="ab8e7-109">Zkuste [online simulátoru malin pí](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="ab8e7-110">Nebo zakoupení nové kit [zde](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="ab8e7-111">Co dělat</span><span class="sxs-lookup"><span data-stu-id="ab8e7-111">What you do</span></span>

* <span data-ttu-id="ab8e7-112">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-112">Create an IoT hub.</span></span>
* <span data-ttu-id="ab8e7-113">Registrovat zařízení pro platformy ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="ab8e7-114">Instalační program Malinová pí.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="ab8e7-115">Spuštění ukázkové aplikace na platformy k odesílání dat snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-115">Run a sample application on Pi to send sensor data to your IoT hub.</span></span>

<span data-ttu-id="ab8e7-116">Pi malin připojení do služby IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-116">Connect Raspberry Pi to an IoT hub that you create.</span></span> <span data-ttu-id="ab8e7-117">Pak spusťte ukázkovou aplikaci na platformy ke shromažďování dat teploty a vlhkosti ze BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-117">Then you run a sample application on Pi to collect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="ab8e7-118">Nakonec odeslat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-118">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="ab8e7-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="ab8e7-119">What you learn</span></span>

* <span data-ttu-id="ab8e7-120">Postup vytvoření služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-120">How to create an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="ab8e7-121">Postup připojení pí s BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-121">How to connect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="ab8e7-122">Postup shromažďování dat snímačů spuštěním ukázkovou aplikaci na pí.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-122">How to collect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="ab8e7-123">Jak odesílat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-123">How to send sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="ab8e7-124">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="ab8e7-124">What you need</span></span>

![Co potřebujete](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* <span data-ttu-id="ab8e7-126">Malin pí 2 nebo 3 pí malin panelu.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-126">The Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="ab8e7-127">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-127">An active Azure subscription.</span></span> <span data-ttu-id="ab8e7-128">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="ab8e7-129">Monitorování, USB klávesnice a myši připojujících se k pí.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-129">A monitor, a USB keyboard, and mouse that connect to Pi.</span></span>
* <span data-ttu-id="ab8e7-130">Mac nebo počítači se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="ab8e7-131">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-131">An Internet connection.</span></span>
* <span data-ttu-id="ab8e7-132">16 GB nebo vyšší microSD karta.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="ab8e7-133">USB-adaptér nebo microSD karta SD vypálíte image operačního systému na kartě microSD.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-133">A USB-SD adapter or microSD card to burn the operating system image onto the microSD card.</span></span>
* <span data-ttu-id="ab8e7-134">5 volt 2 amp napájení s 6 stopy malých kabel USB.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-134">A 5-volt 2-amp power supply with the 6-foot micro USB cable.</span></span>

<span data-ttu-id="ab8e7-135">Následující položky jsou volitelné:</span><span class="sxs-lookup"><span data-stu-id="ab8e7-135">The following items are optional:</span></span>

* <span data-ttu-id="ab8e7-136">Sestavený Adafruit BME280 teploty, naléhavost a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="ab8e7-137">Breadboard.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-137">A breadboard.</span></span>
* <span data-ttu-id="ab8e7-138">Vodičům můstek 6 F/M.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="ab8e7-139">Rozptýlený DIODU 10 mm.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="ab8e7-140">Tyto položky jsou volitelné, protože data snímačů simulated podporu ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-140">These items are optional because the code sample support simulated sensor data.</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="ab8e7-141">Instalační program Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="ab8e7-141">Setup Raspberry Pi</span></span>

### <a name="install-the-raspbian-operating-system-for-pi"></a><span data-ttu-id="ab8e7-142">Instalace operačního systému Raspbian pí</span><span class="sxs-lookup"><span data-stu-id="ab8e7-142">Install the Raspbian operating system for Pi</span></span>

<span data-ttu-id="ab8e7-143">Připravte karty microSD pro instalaci bitové kopie Raspbian.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-143">Prepare the microSD card for installation of the Raspbian image.</span></span>

1. <span data-ttu-id="ab8e7-144">Stáhněte si Raspbian.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="ab8e7-145">[Stáhnout Raspbian Klára plochy](https://www.raspberrypi.org/downloads/raspbian/) (soubor .zip).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (the .zip file).</span></span>
   1. <span data-ttu-id="ab8e7-146">Extrahujte Raspbian image do složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-146">Extract the Raspbian image to a folder on your computer.</span></span>
1. <span data-ttu-id="ab8e7-147">Nainstalujte Raspbian microSD karta.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-147">Install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="ab8e7-148">[Stáhněte a nainstalujte nástroj Etcher SD karty zapisovací jednotka](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-148">[Download and install the Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="ab8e7-149">Spusťte Etcher a vyberte Raspbian bitovou kopii, která jste extrahovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-149">Run Etcher and select the Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="ab8e7-150">Vyberte jednotku microSD karta.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-150">Select the microSD card drive.</span></span> <span data-ttu-id="ab8e7-151">Všimněte si, že Etcher může jste již vybrali správnou jednotku.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-151">Note that Etcher may have already selected the correct drive.</span></span>
   1. <span data-ttu-id="ab8e7-152">Klikněte na tlačítko nainstalovat Raspbian do karty microSD Flash.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-152">Click Flash to install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="ab8e7-153">Karta microSD odeberte z počítače, po dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-153">Remove the microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="ab8e7-154">Je bezpečné karty microSD přímo odebrat, protože Etcher automaticky vysune nebo odpojí microSD karta po dokončení.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-154">It's safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.</span></span>
   1. <span data-ttu-id="ab8e7-155">Karta microSD vložte do pí.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-155">Insert the microSD card into Pi.</span></span>

### <a name="enable-ssh-and-spi"></a><span data-ttu-id="ab8e7-156">Povolit SSH a SPI</span><span class="sxs-lookup"><span data-stu-id="ab8e7-156">Enable SSH and SPI</span></span>

1. <span data-ttu-id="ab8e7-157">Připojit k monitoru, klávesnice a myši platformy, spusťte platformy a znovu přihlásili Raspbian pomocí `pi` jako uživatelské jméno a `raspberry` jako heslo.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-157">Connect Pi to the monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as the user name and `raspberry` as the password.</span></span>
1. <span data-ttu-id="ab8e7-158">Klikněte na ikonu Malinová > **Předvolby** > **malin pí konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-158">Click the Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![V nabídce Raspbian předvolby](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="ab8e7-160">Na **rozhraní** nastavte **SPI** a **SSH** k **povolit**a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-160">On the **Interfaces** tab, set **SPI** and **SSH** to **Enable**, and then click **OK**.</span></span> <span data-ttu-id="ab8e7-161">Pokud nemáte fyzické senzory a chcete použít data simulované snímačů, tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-161">If you don't have physical sensors and want to use simulated sensor data, this step is optional.</span></span>

   ![Povolit SPI a SSH na Malinová platformy](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="ab8e7-163">SSH a SPI povolit, můžete najít další dokumenty odkaz na [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) a [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-163">To enable SSH and SPI, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span></span>

### <a name="connect-the-sensor-to-pi"></a><span data-ttu-id="ab8e7-164">Připojit senzoru PI</span><span class="sxs-lookup"><span data-stu-id="ab8e7-164">Connect the sensor to Pi</span></span>

<span data-ttu-id="ab8e7-165">Pomocí vedení breadboard a můstek pro připojení DIODU a BME280 PI následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-165">Use the breadboard and jumper wires to connect an LED and a BME280 to Pi as follows.</span></span> <span data-ttu-id="ab8e7-166">Pokud nemáte senzoru, [tuto část přeskočte](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-166">If you don’t have the sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![Připojení malin platformy a senzor](media/iot-hub-raspberry-pi-kit-c-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="ab8e7-168">BME280 senzoru teploty a vlhkosti data můžete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-168">The BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="ab8e7-169">A DIODU bude blink – Pokud je komunikace mezi zařízením a cloudem.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-169">And the LED will blink if there is a communication between device and the cloud.</span></span> 

<span data-ttu-id="ab8e7-170">Senzor kód PIN použijte následující kabeláž:</span><span class="sxs-lookup"><span data-stu-id="ab8e7-170">For sensor pins, use the following wiring:</span></span>

| <span data-ttu-id="ab8e7-171">Spuštění (senzor & DIODU)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="ab8e7-172">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-172">End (Board)</span></span>            | <span data-ttu-id="ab8e7-173">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="ab8e7-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="ab8e7-174">Indikátor VDD (Pin 5G)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-174">LED VDD (Pin 5G)</span></span>         | <span data-ttu-id="ab8e7-175">GPIO 4 (Pin 7)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-175">GPIO 4 (Pin 7)</span></span>         | <span data-ttu-id="ab8e7-176">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="ab8e7-176">White cable</span></span>   |
| <span data-ttu-id="ab8e7-177">Indikátor zem (Pin 6G)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-177">LED GND (Pin 6G)</span></span>         | <span data-ttu-id="ab8e7-178">ZEM (Pin 6)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-178">GND (Pin 6)</span></span>            | <span data-ttu-id="ab8e7-179">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="ab8e7-179">Black cable</span></span>   |
| <span data-ttu-id="ab8e7-180">VDD (Pin 18F)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-180">VDD (Pin 18F)</span></span>            | <span data-ttu-id="ab8e7-181">3.3v opravná (Pin 17)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-181">3.3V PWR (Pin 17)</span></span>      | <span data-ttu-id="ab8e7-182">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="ab8e7-182">White cable</span></span>   |
| <span data-ttu-id="ab8e7-183">ZEM (Pin 20F)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-183">GND (Pin 20F)</span></span>            | <span data-ttu-id="ab8e7-184">ZEM (Pin 20)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-184">GND (Pin 20)</span></span>           | <span data-ttu-id="ab8e7-185">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="ab8e7-185">Black cable</span></span>   |
| <span data-ttu-id="ab8e7-186">SCK (Pin 21F)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-186">SCK (Pin 21F)</span></span>            | <span data-ttu-id="ab8e7-187">SPI0 SCLK (Pin 23)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-187">SPI0 SCLK (Pin 23)</span></span>     | <span data-ttu-id="ab8e7-188">Oranžové kabel</span><span class="sxs-lookup"><span data-stu-id="ab8e7-188">Orange cable</span></span>  |
| <span data-ttu-id="ab8e7-189">SDO (Pin 22F)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-189">SDO (Pin 22F)</span></span>            | <span data-ttu-id="ab8e7-190">SPI0 MISO (Pin 21)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-190">SPI0 MISO (Pin 21)</span></span>     | <span data-ttu-id="ab8e7-191">Žlutý kabel</span><span class="sxs-lookup"><span data-stu-id="ab8e7-191">Yellow cable</span></span>  |
| <span data-ttu-id="ab8e7-192">SDI (Pin 23F)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-192">SDI (Pin 23F)</span></span>            | <span data-ttu-id="ab8e7-193">SPI0 MOSI (Pin 19)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-193">SPI0 MOSI (Pin 19)</span></span>     | <span data-ttu-id="ab8e7-194">Zelená kabel</span><span class="sxs-lookup"><span data-stu-id="ab8e7-194">Green cable</span></span>   |
| <span data-ttu-id="ab8e7-195">CS (Pin 24F)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-195">CS (Pin 24F)</span></span>             | <span data-ttu-id="ab8e7-196">SPI0 CS (Pin 24)</span><span class="sxs-lookup"><span data-stu-id="ab8e7-196">SPI0 CS (Pin 24)</span></span>       | <span data-ttu-id="ab8e7-197">Modrý kabel</span><span class="sxs-lookup"><span data-stu-id="ab8e7-197">Blue cable</span></span>    |

<span data-ttu-id="ab8e7-198">Kliknutím zobrazíte [malin pí 2 a 3 mapování kódu Pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) pro vaši informaci.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-198">Click to view [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="ab8e7-199">Po připojení BME280 úspěšně vaší malin PI, mělo by být jako níže bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-199">After you've successfully connected BME280 to your Raspberry Pi, it should be like below image.</span></span>

![Připojené platformy a BME280](media/iot-hub-raspberry-pi-kit-c-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a><span data-ttu-id="ab8e7-201">Připojení k síti platformy</span><span class="sxs-lookup"><span data-stu-id="ab8e7-201">Connect Pi to the network</span></span>

<span data-ttu-id="ab8e7-202">Zapněte pí pomocí kabelu USB micro a napájení.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-202">Turn on Pi by using the micro USB cable and the power supply.</span></span> <span data-ttu-id="ab8e7-203">Pomocí kabelu Ethernet připojit platformy k drátové síti nebo postupujte podle [pokyny z Foundation pí malin](https://www.raspberrypi.org/learning/software-guide/wifi/) pro připojení k bezdrátové síti pí.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-203">Use the Ethernet cable to connect Pi to your wired network or follow the [instructions from the Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) to connect Pi to your wireless network.</span></span> <span data-ttu-id="ab8e7-204">Po vaší platformy se úspěšně připojil k síti, budete muset poznamenejte [IP adresa vašeho čísla pí](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-204">After your Pi has been successfully connected to the network, you need to take a note of the [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Připojení k drátové síti](media/iot-hub-raspberry-pi-kit-c-get-started/5_power-on-pi.jpg)


## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="ab8e7-206">Spuštění ukázkové aplikace na platformy</span><span class="sxs-lookup"><span data-stu-id="ab8e7-206">Run a sample application on Pi</span></span>

### <a name="install-the-prerequisite-packages"></a><span data-ttu-id="ab8e7-207">Instalaci požadovaných balíčků</span><span class="sxs-lookup"><span data-stu-id="ab8e7-207">Install the prerequisite packages</span></span>

1. <span data-ttu-id="ab8e7-208">Použijte jeden z následujících klientů SSH z hostitelského počítače pro připojení k vaší malin platformy.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-208">Use one of the following SSH clients from your host computer to connect to your Raspberry Pi.</span></span>
   
   <span data-ttu-id="ab8e7-209">**Uživatelé s Windows**</span><span class="sxs-lookup"><span data-stu-id="ab8e7-209">**Windows Users**</span></span>
   1. <span data-ttu-id="ab8e7-210">Stáhněte a nainstalujte [PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-210">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="ab8e7-211">Zkopírujte IP adresu vašeho pí do název hostitele (nebo IP adresu) oddílu a vyberte jako typ připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-211">Copy the IP address of your Pi into the Host name (or IP address) section and select SSH as the connection type.</span></span>
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   <span data-ttu-id="ab8e7-213">**Mac a Ubuntu uživatelů**</span><span class="sxs-lookup"><span data-stu-id="ab8e7-213">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="ab8e7-214">Použijte integrovaného klienta SSH na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-214">Use the built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="ab8e7-215">Možná budete muset spustit `ssh pi@<ip address of pi>` připojit platformy prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-215">You might need to run `ssh pi@<ip address of pi>` to connect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="ab8e7-216">Výchozí uživatelské jméno `pi` , a heslo je `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-216">The default username is `pi` , and the password is `raspberry`.</span></span>

1. <span data-ttu-id="ab8e7-217">Instalaci požadovaných balíčků pro Microsoft Azure IoT zařízení SDK pro jazyk C a Cmake spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="ab8e7-217">Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C and Cmake by running the following commands:</span></span>

   ```bash
   grep -q -F 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
   grep -q -F 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
   sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FDA6A393E4C2257F
   sudo apt-get update
   sudo apt-get install -y azure-iot-sdk-c-dev cmake libcurl4-openssl-dev git-core
   git clone git://git.drogon.net/wiringPi
   cd ./wiringPi
   ./build
   ```


### <a name="configure-the-sample-application"></a><span data-ttu-id="ab8e7-218">Nakonfigurujte ukázkovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="ab8e7-218">Configure the sample application</span></span>

1. <span data-ttu-id="ab8e7-219">Naklonujte ukázkovou aplikaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab8e7-219">Clone the sample application by running the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app
   ```
1. <span data-ttu-id="ab8e7-220">Otevřete konfigurační soubor spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="ab8e7-220">Open the config file by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-raspberrypi-client-app
   nano config.h
   ```

   ![Konfigurační soubor](media/iot-hub-raspberry-pi-kit-c-get-started/6_config-file.png)

   <span data-ttu-id="ab8e7-222">Existují dvě makra v tomto souboru můžete configurate.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="ab8e7-223">První z nich je `INTERVAL`, která definuje časový interval (v milisekundách) mezi dvě zprávy, které odesílají do cloudu.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-223">The first one is `INTERVAL`, which defines the time interval (in milliseconds) between two messages that send to cloud.</span></span> <span data-ttu-id="ab8e7-224">Druhý `SIMULATED_DATA`, což je logickou hodnotu pro jestli se má používat data simulované snímačů, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-224">The second one `SIMULATED_DATA`,which is a Boolean value for whether to use simulated sensor data or not.</span></span>

   <span data-ttu-id="ab8e7-225">Pokud jste **nemají senzoru**, nastavte `SIMULATED_DATA` hodnotu `1` aby ukázkovou aplikaci, vytváření a používání dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-225">If you **don't have the sensor**, set the `SIMULATED_DATA` value to `1` to make the sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="ab8e7-226">Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="build-and-run-the-sample-application"></a><span data-ttu-id="ab8e7-227">Sestavení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="ab8e7-227">Build and run the sample application</span></span>

1. <span data-ttu-id="ab8e7-228">Vytvoření ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab8e7-228">Build the sample application by running the following command:</span></span>

   ```bash
   cmake . && make
   ```
   ![Sestavení výstupu](media/iot-hub-raspberry-pi-kit-c-get-started/7_build-output.png)

1. <span data-ttu-id="ab8e7-230">Spuštění ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ab8e7-230">Run the sample application by running the following command:</span></span>

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   <span data-ttu-id="ab8e7-231">Zkontrolujte, zda jste způsobené kopírováním a vkládáním zařízení připojovací řetězec do jednoduchých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-231">Make sure you copy-paste the device connection string into the single quotes.</span></span>


<span data-ttu-id="ab8e7-232">Měli byste vidět následující výstup, který popisuje data snímačů a zprávy, které se odesílají do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-232">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

![Výstup – data snímačů odeslaný malin pí do služby IoT hub](media/iot-hub-raspberry-pi-kit-c-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="ab8e7-234">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab8e7-234">Next steps</span></span>

<span data-ttu-id="ab8e7-235">Spustíte ukázkovou aplikaci pro shromažďování dat snímačů a odeslat do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ab8e7-235">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span> <span data-ttu-id="ab8e7-236">Zprávy, které vaše platformy malin odeslal na IoT hub nebo odesílání zprávy pro vaše platformy malin v rozhraní příkazového řádku najdete v sekci [cloud zařízení spravovat zasílání zpráv s iothub-explorer kurzu](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="ab8e7-236">To see the messages that your Raspberry Pi has sent to your IoT hub or send messages to your Raspberry Pi in a command line interface, see the [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
