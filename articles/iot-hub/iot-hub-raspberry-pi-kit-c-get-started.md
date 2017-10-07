---
title: "aaaRaspberry pí toocloud (C) - tooAzure připojit malin platformy IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte tooAzure malin platformy IoT Hub pro platformy malin toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot Malinová pi, malinová platformy iot hub, malinová platformy odesílání dat toocloud Malinová platformy toocloud"
ms.assetid: 68c0e730-1dc8-4e26-ac6b-573b217b302d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/12/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05086890458e196d7fdc87a53fcabb9386245d6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-c"></a><span data-ttu-id="dfb3f-104">Připojit malin pí tooAzure IoT Hub (C)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-104">Connect Raspberry Pi tooAzure IoT Hub (C)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="dfb3f-105">V tomto kurzu zahájíte učení hello základy práce s malin platformy, na kterém běží Raspbian.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="dfb3f-106">Pak zjistíte, jak tooseamlessly propojit své cloudové toohello zařízení pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="dfb3f-107">Jádro IoT Windows 10 ukázek najdete toohello [Centrum vývojářů pro Windows](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-107">For Windows 10 IoT Core samples, go toohello [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="dfb3f-108">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="dfb3f-108">Don't have a kit yet?</span></span> <span data-ttu-id="dfb3f-109">Zkuste [online simulátoru malin pí](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="dfb3f-110">Nebo zakoupení nové kit [zde](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="dfb3f-111">Co dělat</span><span class="sxs-lookup"><span data-stu-id="dfb3f-111">What you do</span></span>

* <span data-ttu-id="dfb3f-112">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-112">Create an IoT hub.</span></span>
* <span data-ttu-id="dfb3f-113">Registrovat zařízení pro platformy ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="dfb3f-114">Instalační program Malinová pí.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="dfb3f-115">Spuštění ukázkové aplikace na platformy toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-115">Run a sample application on Pi toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="dfb3f-116">Připojte malin pí tooan IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-116">Connect Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="dfb3f-117">Pak spusťte ukázkovou aplikaci pro platformy toocollect teploty a vlhkosti data z BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-117">Then you run a sample application on Pi toocollect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="dfb3f-118">Nakonec odeslat hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-118">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="dfb3f-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="dfb3f-119">What you learn</span></span>

* <span data-ttu-id="dfb3f-120">Jak toocreate služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-120">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="dfb3f-121">Jak tooconnect pí s BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-121">How tooconnect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="dfb3f-122">Jak data snímačů toocollect spuštěním ukázkovou aplikaci na pí.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-122">How toocollect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="dfb3f-123">Jak toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-123">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="dfb3f-124">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="dfb3f-124">What you need</span></span>

![Co potřebujete](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* <span data-ttu-id="dfb3f-126">Hello malin pí 2 nebo 3 pí malin panelu.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-126">hello Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="dfb3f-127">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-127">An active Azure subscription.</span></span> <span data-ttu-id="dfb3f-128">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="dfb3f-129">Monitorování, USB klávesnice a myši připojujících tooPi.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-129">A monitor, a USB keyboard, and mouse that connect tooPi.</span></span>
* <span data-ttu-id="dfb3f-130">Mac nebo počítači se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="dfb3f-131">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-131">An Internet connection.</span></span>
* <span data-ttu-id="dfb3f-132">16 GB nebo vyšší microSD karta.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="dfb3f-133">USB SD adaptér nebo microSD karta tooburn hello image operačního systému na kartě microSD hello.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-133">A USB-SD adapter or microSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="dfb3f-134">5 volt 2 amp napájení s hello 6 stopy malých kabel USB.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-134">A 5-volt 2-amp power supply with hello 6-foot micro USB cable.</span></span>

<span data-ttu-id="dfb3f-135">Hello následující položky jsou volitelné:</span><span class="sxs-lookup"><span data-stu-id="dfb3f-135">hello following items are optional:</span></span>

* <span data-ttu-id="dfb3f-136">Sestavený Adafruit BME280 teploty, naléhavost a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="dfb3f-137">Breadboard.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-137">A breadboard.</span></span>
* <span data-ttu-id="dfb3f-138">Vodičům můstek 6 F/M.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="dfb3f-139">Rozptýlený DIODU 10 mm.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="dfb3f-140">Tyto položky jsou volitelné, protože podpora ukázkový kód hello simulated data snímačů.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-140">These items are optional because hello code sample support simulated sensor data.</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="dfb3f-141">Instalační program Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="dfb3f-141">Setup Raspberry Pi</span></span>

### <a name="install-hello-raspbian-operating-system-for-pi"></a><span data-ttu-id="dfb3f-142">Nainstalujte hello Raspbian operační systém pro platformy</span><span class="sxs-lookup"><span data-stu-id="dfb3f-142">Install hello Raspbian operating system for Pi</span></span>

<span data-ttu-id="dfb3f-143">Připravte hello microSD karta pro instalaci bitové kopie Raspbian hello.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-143">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="dfb3f-144">Stáhněte si Raspbian.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="dfb3f-145">[Stáhnout Raspbian Klára plochy](https://www.raspberrypi.org/downloads/raspbian/) (soubor .zip hello).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip file).</span></span>
   1. <span data-ttu-id="dfb3f-146">Extrahujte hello Raspbian image tooa složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-146">Extract hello Raspbian image tooa folder on your computer.</span></span>
1. <span data-ttu-id="dfb3f-147">Nainstalujte Raspbian toohello microSD karta.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-147">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="dfb3f-148">[Stáhněte a nainstalujte nástroj hello Etcher SD karty zapisovací jednotka](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-148">[Download and install hello Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="dfb3f-149">Spusťte Etcher a vyberte bitovou kopii hello Raspbian, které jste extrahovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-149">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="dfb3f-150">Vyberte jednotku karty microSD hello.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-150">Select hello microSD card drive.</span></span> <span data-ttu-id="dfb3f-151">Všimněte si, že Etcher může jste již vybrali správnou jednotku hello.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-151">Note that Etcher may have already selected hello correct drive.</span></span>
   1. <span data-ttu-id="dfb3f-152">Klikněte na Flash tooinstall Raspbian toohello microSD karta.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-152">Click Flash tooinstall Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="dfb3f-153">Po dokončení instalace odeberte hello microSD karta z vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-153">Remove hello microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="dfb3f-154">Je bezpečné tooremove hello microSD karta přímo protože Etcher automaticky vysune nebo odpojí hello microSD karta po dokončení.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-154">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   1. <span data-ttu-id="dfb3f-155">Karta microSD hello vložte do pí.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-155">Insert hello microSD card into Pi.</span></span>

### <a name="enable-ssh-and-spi"></a><span data-ttu-id="dfb3f-156">Povolit SSH a SPI</span><span class="sxs-lookup"><span data-stu-id="dfb3f-156">Enable SSH and SPI</span></span>

1. <span data-ttu-id="dfb3f-157">Připojit pí toohello monitoru, klávesnice a myši, spusťte platformy a znovu přihlásili Raspbian pomocí `pi` jako hello uživatelské jméno a `raspberry` jako hello heslo.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-157">Connect Pi toohello monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as hello user name and `raspberry` as hello password.</span></span>
1. <span data-ttu-id="dfb3f-158">Klikněte na tlačítko hello Malinová ikonu > **Předvolby** > **malin pí konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-158">Click hello Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![Hello Raspbian předvolby nabídky](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="dfb3f-160">Na hello **rozhraní** nastavte **SPI** a **SSH** příliš**povolit**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-160">On hello **Interfaces** tab, set **SPI** and **SSH** too**Enable**, and then click **OK**.</span></span> <span data-ttu-id="dfb3f-161">Pokud nemáte fyzické senzory a chcete data snímačů toouse simulated, tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-161">If you don't have physical sensors and want toouse simulated sensor data, this step is optional.</span></span>

   ![Povolit SPI a SSH na Malinová platformy](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="dfb3f-163">tooenable SSH a SPI, můžete najít další dokumenty odkaz na [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) a [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-163">tooenable SSH and SPI, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span></span>

### <a name="connect-hello-sensor-toopi"></a><span data-ttu-id="dfb3f-164">Připojit tooPi senzor hello</span><span class="sxs-lookup"><span data-stu-id="dfb3f-164">Connect hello sensor tooPi</span></span>

<span data-ttu-id="dfb3f-165">Použijte hello breadboard a můstek vodičům tooconnect DIODU a BME280 tooPi následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-165">Use hello breadboard and jumper wires tooconnect an LED and a BME280 tooPi as follows.</span></span> <span data-ttu-id="dfb3f-166">Pokud nemáte hello senzor [tuto část přeskočte](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-166">If you don’t have hello sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![Hello malin platformy a senzor připojení](media/iot-hub-raspberry-pi-kit-c-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="dfb3f-168">Hello BME280 senzor teploty a vlhkosti data můžete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-168">hello BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="dfb3f-169">A hello DIODU bude blink – Pokud je komunikace mezi zařízením a hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-169">And hello LED will blink if there is a communication between device and hello cloud.</span></span> 

<span data-ttu-id="dfb3f-170">Senzor kód PIN použijte následující kabeláž hello:</span><span class="sxs-lookup"><span data-stu-id="dfb3f-170">For sensor pins, use hello following wiring:</span></span>

| <span data-ttu-id="dfb3f-171">Spuštění (senzor & DIODU)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="dfb3f-172">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-172">End (Board)</span></span>            | <span data-ttu-id="dfb3f-173">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="dfb3f-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="dfb3f-174">Indikátor VDD (Pin 5G)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-174">LED VDD (Pin 5G)</span></span>         | <span data-ttu-id="dfb3f-175">GPIO 4 (Pin 7)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-175">GPIO 4 (Pin 7)</span></span>         | <span data-ttu-id="dfb3f-176">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="dfb3f-176">White cable</span></span>   |
| <span data-ttu-id="dfb3f-177">Indikátor zem (Pin 6G)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-177">LED GND (Pin 6G)</span></span>         | <span data-ttu-id="dfb3f-178">ZEM (Pin 6)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-178">GND (Pin 6)</span></span>            | <span data-ttu-id="dfb3f-179">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="dfb3f-179">Black cable</span></span>   |
| <span data-ttu-id="dfb3f-180">VDD (Pin 18F)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-180">VDD (Pin 18F)</span></span>            | <span data-ttu-id="dfb3f-181">3.3v opravná (Pin 17)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-181">3.3V PWR (Pin 17)</span></span>      | <span data-ttu-id="dfb3f-182">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="dfb3f-182">White cable</span></span>   |
| <span data-ttu-id="dfb3f-183">ZEM (Pin 20F)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-183">GND (Pin 20F)</span></span>            | <span data-ttu-id="dfb3f-184">ZEM (Pin 20)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-184">GND (Pin 20)</span></span>           | <span data-ttu-id="dfb3f-185">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="dfb3f-185">Black cable</span></span>   |
| <span data-ttu-id="dfb3f-186">SCK (Pin 21F)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-186">SCK (Pin 21F)</span></span>            | <span data-ttu-id="dfb3f-187">SPI0 SCLK (Pin 23)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-187">SPI0 SCLK (Pin 23)</span></span>     | <span data-ttu-id="dfb3f-188">Oranžové kabel</span><span class="sxs-lookup"><span data-stu-id="dfb3f-188">Orange cable</span></span>  |
| <span data-ttu-id="dfb3f-189">SDO (Pin 22F)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-189">SDO (Pin 22F)</span></span>            | <span data-ttu-id="dfb3f-190">SPI0 MISO (Pin 21)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-190">SPI0 MISO (Pin 21)</span></span>     | <span data-ttu-id="dfb3f-191">Žlutý kabel</span><span class="sxs-lookup"><span data-stu-id="dfb3f-191">Yellow cable</span></span>  |
| <span data-ttu-id="dfb3f-192">SDI (Pin 23F)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-192">SDI (Pin 23F)</span></span>            | <span data-ttu-id="dfb3f-193">SPI0 MOSI (Pin 19)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-193">SPI0 MOSI (Pin 19)</span></span>     | <span data-ttu-id="dfb3f-194">Zelená kabel</span><span class="sxs-lookup"><span data-stu-id="dfb3f-194">Green cable</span></span>   |
| <span data-ttu-id="dfb3f-195">CS (Pin 24F)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-195">CS (Pin 24F)</span></span>             | <span data-ttu-id="dfb3f-196">SPI0 CS (Pin 24)</span><span class="sxs-lookup"><span data-stu-id="dfb3f-196">SPI0 CS (Pin 24)</span></span>       | <span data-ttu-id="dfb3f-197">Modrý kabel</span><span class="sxs-lookup"><span data-stu-id="dfb3f-197">Blue cable</span></span>    |

<span data-ttu-id="dfb3f-198">Klikněte na tlačítko tooview [malin pí 2 a 3 mapování kódu Pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) pro vaši informaci.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-198">Click tooview [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="dfb3f-199">Po úspěšném připojení BME280 tooyour malin Pi, mělo by být jako níže bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-199">After you've successfully connected BME280 tooyour Raspberry Pi, it should be like below image.</span></span>

![Připojené platformy a BME280](media/iot-hub-raspberry-pi-kit-c-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a><span data-ttu-id="dfb3f-201">Připojit síť toohello platformy</span><span class="sxs-lookup"><span data-stu-id="dfb3f-201">Connect Pi toohello network</span></span>

<span data-ttu-id="dfb3f-202">Zapněte pí pomocí kabelu USB malých hello a hello napájení.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-202">Turn on Pi by using hello micro USB cable and hello power supply.</span></span> <span data-ttu-id="dfb3f-203">Použití hello Ethernet kabel tooconnect pí tooyour drátové sítě nebo postupujte podle hello [pokyny z hello malin platformy Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect pí tooyour bezdrátové sítě.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-203">Use hello Ethernet cable tooconnect Pi tooyour wired network or follow hello [instructions from hello Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="dfb3f-204">Po vaší pí byl úspěšně připojeno toohello sítě, je nutné tootake poznámku o hello [IP adresa vašeho čísla pí](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-204">After your Pi has been successfully connected toohello network, you need tootake a note of hello [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Připojené toowired sítě](media/iot-hub-raspberry-pi-kit-c-get-started/5_power-on-pi.jpg)


## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="dfb3f-206">Spuštění ukázkové aplikace na platformy</span><span class="sxs-lookup"><span data-stu-id="dfb3f-206">Run a sample application on Pi</span></span>

### <a name="install-hello-prerequisite-packages"></a><span data-ttu-id="dfb3f-207">Instalace požadovaných součástí balíčků hello</span><span class="sxs-lookup"><span data-stu-id="dfb3f-207">Install hello prerequisite packages</span></span>

1. <span data-ttu-id="dfb3f-208">Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooyour malin pí hello.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-208">Use one of hello following SSH clients from your host computer tooconnect tooyour Raspberry Pi.</span></span>
   
   <span data-ttu-id="dfb3f-209">**Uživatelé s Windows**</span><span class="sxs-lookup"><span data-stu-id="dfb3f-209">**Windows Users**</span></span>
   1. <span data-ttu-id="dfb3f-210">Stáhněte a nainstalujte [PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-210">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="dfb3f-211">Zkopírujte hello IP adresa vaší oddílu pí do hello hostitele název (nebo IP adresu) a vyberte jako typ hello připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-211">Copy hello IP address of your Pi into hello Host name (or IP address) section and select SSH as hello connection type.</span></span>
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   <span data-ttu-id="dfb3f-213">**Mac a Ubuntu uživatelů**</span><span class="sxs-lookup"><span data-stu-id="dfb3f-213">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="dfb3f-214">Použijte integrovaného klienta SSH hello na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-214">Use hello built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="dfb3f-215">Může být nutné toorun `ssh pi@<ip address of pi>` tooconnect platformy prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-215">You might need toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="dfb3f-216">výchozí uživatelské jméno Hello `pi` , a je heslo hello `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-216">hello default username is `pi` , and hello password is `raspberry`.</span></span>

1. <span data-ttu-id="dfb3f-217">Instalace hello požadované balíčky pro hello Microsoft sady SDK zařízení IoT Azure pro C a Cmake spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="dfb3f-217">Install hello prerequisite packages for hello Microsoft Azure IoT Device SDK for C and Cmake by running hello following commands:</span></span>

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


### <a name="configure-hello-sample-application"></a><span data-ttu-id="dfb3f-218">Konfigurace hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="dfb3f-218">Configure hello sample application</span></span>

1. <span data-ttu-id="dfb3f-219">Klonování hello ukázkovou aplikaci spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dfb3f-219">Clone hello sample application by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app
   ```
1. <span data-ttu-id="dfb3f-220">Otevřete konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="dfb3f-220">Open hello config file by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-raspberrypi-client-app
   nano config.h
   ```

   ![Konfigurační soubor](media/iot-hub-raspberry-pi-kit-c-get-started/6_config-file.png)

   <span data-ttu-id="dfb3f-222">Existují dvě makra v tomto souboru můžete configurate.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="dfb3f-223">Hello nejprve jeden je `INTERVAL`, která definuje hello časový interval (v milisekundách) mezi dvě zprávy, které odesílají toocloud.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-223">hello first one is `INTERVAL`, which defines hello time interval (in milliseconds) between two messages that send toocloud.</span></span> <span data-ttu-id="dfb3f-224">Hello druhá `SIMULATED_DATA`, což je logickou hodnotu pro jestli toouse simulated data snímačů, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-224">hello second one `SIMULATED_DATA`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="dfb3f-225">Pokud jste **nemají hello senzor**, nastavte hello `SIMULATED_DATA` hodnota příliš`1` toomake hello ukázkovou aplikaci vytváření a používání dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-225">If you **don't have hello sensor**, set hello `SIMULATED_DATA` value too`1` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="dfb3f-226">Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="build-and-run-hello-sample-application"></a><span data-ttu-id="dfb3f-227">Sestavení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="dfb3f-227">Build and run hello sample application</span></span>

1. <span data-ttu-id="dfb3f-228">Vytvoření ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dfb3f-228">Build hello sample application by running hello following command:</span></span>

   ```bash
   cmake . && make
   ```
   ![Sestavení výstupu](media/iot-hub-raspberry-pi-kit-c-get-started/7_build-output.png)

1. <span data-ttu-id="dfb3f-230">Spuštění ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dfb3f-230">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   <span data-ttu-id="dfb3f-231">Zkontrolujte, zda jste způsobené kopírováním a vkládáním hello zařízení připojovací řetězec do jednoduchých uvozovek a být hello.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-231">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>


<span data-ttu-id="dfb3f-232">Měli byste vidět, že hello následující výstup, že zobrazuje hello senzor dat a hello zpráv, které jsou odeslány tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-232">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Výstup – data snímačů odeslaný tooyour malin platformy IoT hub](media/iot-hub-raspberry-pi-kit-c-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="dfb3f-234">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dfb3f-234">Next steps</span></span>

<span data-ttu-id="dfb3f-235">Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dfb3f-235">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span> <span data-ttu-id="dfb3f-236">toosee hello zprávy, že vaše platformy malin odeslal tooyour IoT hub nebo odesílání zprávy tooyour malin platformy v rozhraní příkazového řádku, najdete v části hello [cloud zařízení spravovat zasílání zpráv s iothub-explorer kurzu](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="dfb3f-236">toosee hello messages that your Raspberry Pi has sent tooyour IoT hub or send messages tooyour Raspberry Pi in a command line interface, see hello [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
