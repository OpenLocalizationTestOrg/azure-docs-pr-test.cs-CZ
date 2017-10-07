---
title: "aaaRaspberry pí toocloud (Node.js) – tooAzure připojit malin platformy IoT Hub | Microsoft Docs"
description: "Připojte tooAzure malin platformy IoT Hub pro platformy malin toosend data toohello cloudu Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure iot Malinová pi, malinová platformy iot hub, malinová platformy odesílání dat toocloud Malinová platformy toocloud"
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
ms.openlocfilehash: 07bc66983c427eab8118be18d91abb25deb03ad3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-nodejs"></a><span data-ttu-id="94844-104">Připojit tooAzure malin platformy IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="94844-104">Connect Raspberry Pi tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="94844-105">V tomto kurzu zahájíte učení hello základy práce s malin platformy, na kterém běží Raspbian.</span><span class="sxs-lookup"><span data-stu-id="94844-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="94844-106">Pak zjistíte, jak tooseamlessly propojit své cloudové toohello zařízení pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="94844-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="94844-107">Jádro IoT Windows 10 ukázek najdete toohello [Centrum vývojářů pro Windows](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="94844-107">For Windows 10 IoT Core samples, go toohello [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="94844-108">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="94844-108">Don't have a kit yet?</span></span> <span data-ttu-id="94844-109">Zkuste hello [malin pí 3 emulátoru](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/).</span><span class="sxs-lookup"><span data-stu-id="94844-109">Try hello [Raspberry Pi 3 emulator](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/).</span></span> <span data-ttu-id="94844-110">Nebo zakoupení nové kit [zde](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="94844-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="94844-111">Co dělat</span><span class="sxs-lookup"><span data-stu-id="94844-111">What you do</span></span>

* <span data-ttu-id="94844-112">Instalační program Malinová pí.</span><span class="sxs-lookup"><span data-stu-id="94844-112">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="94844-113">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="94844-113">Create an IoT hub.</span></span>
* <span data-ttu-id="94844-114">Registrovat zařízení pro platformy ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="94844-114">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="94844-115">Spuštění ukázkové aplikace na platformy toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="94844-115">Run a sample application on Pi toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="94844-116">Připojte malin pí tooan IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="94844-116">Connect Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="94844-117">Pak spusťte ukázkovou aplikaci pro platformy toocollect teploty a vlhkosti data z BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="94844-117">Then you run a sample application on Pi toocollect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="94844-118">Nakonec odeslat hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="94844-118">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="94844-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="94844-119">What you learn</span></span>

* <span data-ttu-id="94844-120">Jak toocreate služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="94844-120">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="94844-121">Jak tooconnect pí s BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="94844-121">How tooconnect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="94844-122">Jak data snímačů toocollect spuštěním ukázkovou aplikaci na pí.</span><span class="sxs-lookup"><span data-stu-id="94844-122">How toocollect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="94844-123">Jak toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="94844-123">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="94844-124">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="94844-124">What you need</span></span>

![Co potřebujete](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* <span data-ttu-id="94844-126">Hello malin pí 2 nebo 3 pí malin panelu.</span><span class="sxs-lookup"><span data-stu-id="94844-126">hello Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="94844-127">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="94844-127">An active Azure subscription.</span></span> <span data-ttu-id="94844-128">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="94844-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="94844-129">Monitorování, USB klávesnice a myši připojujících tooPi.</span><span class="sxs-lookup"><span data-stu-id="94844-129">A monitor, a USB keyboard, and mouse that connect tooPi.</span></span>
* <span data-ttu-id="94844-130">Mac nebo počítači se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="94844-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="94844-131">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="94844-131">An Internet connection.</span></span>
* <span data-ttu-id="94844-132">16 GB nebo vyšší microSD karta.</span><span class="sxs-lookup"><span data-stu-id="94844-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="94844-133">USB SD adaptér nebo microSD karta tooburn hello image operačního systému na kartě microSD hello.</span><span class="sxs-lookup"><span data-stu-id="94844-133">A USB-SD adapter or microSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="94844-134">5 volt 2 amp napájení s hello 6 stopy malých kabel USB.</span><span class="sxs-lookup"><span data-stu-id="94844-134">A 5-volt 2-amp power supply with hello 6-foot micro USB cable.</span></span>

<span data-ttu-id="94844-135">Hello následující položky jsou volitelné:</span><span class="sxs-lookup"><span data-stu-id="94844-135">hello following items are optional:</span></span>

* <span data-ttu-id="94844-136">Sestavený Adafruit BME280 teploty, naléhavost a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="94844-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="94844-137">Breadboard.</span><span class="sxs-lookup"><span data-stu-id="94844-137">A breadboard.</span></span>
* <span data-ttu-id="94844-138">Vodičům můstek 6 F/M.</span><span class="sxs-lookup"><span data-stu-id="94844-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="94844-139">Rozptýlený DIODU 10 mm.</span><span class="sxs-lookup"><span data-stu-id="94844-139">A diffused 10-mm LED.</span></span>

  > [!NOTE] 
  <span data-ttu-id="94844-140">Tyto položky jsou volitelné, protože podpora ukázkový kód hello simulated data snímačů.</span><span class="sxs-lookup"><span data-stu-id="94844-140">These items are optional because hello code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a><span data-ttu-id="94844-141">Instalační program Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="94844-141">Setup Raspberry Pi</span></span>

### <a name="install-hello-raspbian-operating-system-for-pi"></a><span data-ttu-id="94844-142">Nainstalujte hello Raspbian operační systém pro platformy</span><span class="sxs-lookup"><span data-stu-id="94844-142">Install hello Raspbian operating system for Pi</span></span>

<span data-ttu-id="94844-143">Připravte hello microSD karta pro instalaci bitové kopie Raspbian hello.</span><span class="sxs-lookup"><span data-stu-id="94844-143">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="94844-144">Stáhněte si Raspbian.</span><span class="sxs-lookup"><span data-stu-id="94844-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="94844-145">[Stáhnout Raspbian Klára s pixelů](https://www.raspberrypi.org/downloads/raspbian/) (soubor .zip hello).</span><span class="sxs-lookup"><span data-stu-id="94844-145">[Download Raspbian Jessie with Pixel](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip file).</span></span>
   1. <span data-ttu-id="94844-146">Extrahujte hello Raspbian image tooa složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="94844-146">Extract hello Raspbian image tooa folder on your computer.</span></span>
1. <span data-ttu-id="94844-147">Nainstalujte Raspbian toohello microSD karta.</span><span class="sxs-lookup"><span data-stu-id="94844-147">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="94844-148">[Stáhněte a nainstalujte nástroj hello Etcher SD karty zapisovací jednotka](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="94844-148">[Download and install hello Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="94844-149">Spusťte Etcher a vyberte bitovou kopii hello Raspbian, které jste extrahovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="94844-149">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="94844-150">Vyberte jednotku karty microSD hello.</span><span class="sxs-lookup"><span data-stu-id="94844-150">Select hello microSD card drive.</span></span> <span data-ttu-id="94844-151">Všimněte si, že Etcher může jste již vybrali správnou jednotku hello.</span><span class="sxs-lookup"><span data-stu-id="94844-151">Note that Etcher may have already selected hello correct drive.</span></span>
   1. <span data-ttu-id="94844-152">Klikněte na Flash tooinstall Raspbian toohello microSD karta.</span><span class="sxs-lookup"><span data-stu-id="94844-152">Click Flash tooinstall Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="94844-153">Po dokončení instalace odeberte hello microSD karta z vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="94844-153">Remove hello microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="94844-154">Je bezpečné tooremove hello microSD karta přímo protože Etcher automaticky vysune nebo odpojí hello microSD karta po dokončení.</span><span class="sxs-lookup"><span data-stu-id="94844-154">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   1. <span data-ttu-id="94844-155">Karta microSD hello vložte do pí.</span><span class="sxs-lookup"><span data-stu-id="94844-155">Insert hello microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="94844-156">Povolit SSH a I2C</span><span class="sxs-lookup"><span data-stu-id="94844-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="94844-157">Připojit pí toohello monitoru, klávesnice a myši, spusťte platformy a znovu přihlásili Raspbian pomocí `pi` jako hello uživatelské jméno a `raspberry` jako hello heslo.</span><span class="sxs-lookup"><span data-stu-id="94844-157">Connect Pi toohello monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as hello user name and `raspberry` as hello password.</span></span>
1. <span data-ttu-id="94844-158">Klikněte na tlačítko hello Malinová ikonu > **Předvolby** > **malin pí konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="94844-158">Click hello Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![Hello Raspbian předvolby nabídky](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="94844-160">Na hello **rozhraní** nastavte **I2C** a **SSH** příliš**povolit**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="94844-160">On hello **Interfaces** tab, set **I2C** and **SSH** too**Enable**, and then click **OK**.</span></span> <span data-ttu-id="94844-161">Pokud nemáte fyzické senzory a chcete data snímačů toouse simulated, tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="94844-161">If you don't have physical sensors and want toouse simulated sensor data, this step is optional.</span></span>

   ![Povolit I2C a SSH na Malinová platformy](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="94844-163">tooenable SSH a I2C, můžete najít další dokumenty odkaz na [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) a [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span><span class="sxs-lookup"><span data-stu-id="94844-163">tooenable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).</span></span>

### <a name="connect-hello-sensor-toopi"></a><span data-ttu-id="94844-164">Připojit tooPi senzor hello</span><span class="sxs-lookup"><span data-stu-id="94844-164">Connect hello sensor tooPi</span></span>

<span data-ttu-id="94844-165">Použijte hello breadboard a můstek vodičům tooconnect DIODU a BME280 tooPi následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="94844-165">Use hello breadboard and jumper wires tooconnect an LED and a BME280 tooPi as follows.</span></span> <span data-ttu-id="94844-166">Pokud nemáte hello senzor, tuto část přeskočte.</span><span class="sxs-lookup"><span data-stu-id="94844-166">If you don’t have hello sensor, skip this section.</span></span>

![Hello malin platformy a senzor připojení](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="94844-168">Hello BME280 senzor teploty a vlhkosti data můžete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="94844-168">hello BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="94844-169">A hello DIODU bude blink – Pokud je komunikace mezi zařízením a hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="94844-169">And hello LED will blink if there is a communication between device and hello cloud.</span></span> 

<span data-ttu-id="94844-170">Senzor kód PIN použijte následující kabeláž hello:</span><span class="sxs-lookup"><span data-stu-id="94844-170">For sensor pins, use hello following wiring:</span></span>

| <span data-ttu-id="94844-171">Spuštění (senzor & DIODU)</span><span class="sxs-lookup"><span data-stu-id="94844-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="94844-172">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="94844-172">End (Board)</span></span>            | <span data-ttu-id="94844-173">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="94844-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="94844-174">VDD (Pin 5G)</span><span class="sxs-lookup"><span data-stu-id="94844-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="94844-175">3.3v opravná (Pin 1)</span><span class="sxs-lookup"><span data-stu-id="94844-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="94844-176">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="94844-176">White cable</span></span>   |
| <span data-ttu-id="94844-177">ZEM (Pin 7G)</span><span class="sxs-lookup"><span data-stu-id="94844-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="94844-178">ZEM (Pin 6)</span><span class="sxs-lookup"><span data-stu-id="94844-178">GND (Pin 6)</span></span>            | <span data-ttu-id="94844-179">Hnědá kabel</span><span class="sxs-lookup"><span data-stu-id="94844-179">Brown cable</span></span>   |
| <span data-ttu-id="94844-180">SCK (Pin 8G)</span><span class="sxs-lookup"><span data-stu-id="94844-180">SCK (Pin 8G)</span></span>             | <span data-ttu-id="94844-181">I2C1 SDA (PIN kódu 3)</span><span class="sxs-lookup"><span data-stu-id="94844-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="94844-182">Oranžové kabel</span><span class="sxs-lookup"><span data-stu-id="94844-182">Orange cable</span></span>  |
| <span data-ttu-id="94844-183">SDI (Pin 10G)</span><span class="sxs-lookup"><span data-stu-id="94844-183">SDI (Pin 10G)</span></span>            | <span data-ttu-id="94844-184">I2C1 SCL (Pin 5)</span><span class="sxs-lookup"><span data-stu-id="94844-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="94844-185">Red kabel</span><span class="sxs-lookup"><span data-stu-id="94844-185">Red cable</span></span>     |
| <span data-ttu-id="94844-186">Indikátor VDD (Pin 18F)</span><span class="sxs-lookup"><span data-stu-id="94844-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="94844-187">GPIO 24 (Pin 18)</span><span class="sxs-lookup"><span data-stu-id="94844-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="94844-188">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="94844-188">White cable</span></span>   |
| <span data-ttu-id="94844-189">Indikátor zem (Pin 17F)</span><span class="sxs-lookup"><span data-stu-id="94844-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="94844-190">ZEM (Pin 20)</span><span class="sxs-lookup"><span data-stu-id="94844-190">GND (Pin 20)</span></span>           | <span data-ttu-id="94844-191">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="94844-191">Black cable</span></span>   |

<span data-ttu-id="94844-192">Klikněte na tlačítko tooview [malin pí 2 a 3 mapování kódu Pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) pro vaši informaci.</span><span class="sxs-lookup"><span data-stu-id="94844-192">Click tooview [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="94844-193">Po úspěšném připojení BME280 tooyour malin Pi, mělo by být jako níže bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="94844-193">After you've successfully connected BME280 tooyour Raspberry Pi, it should be like below image.</span></span>

![Připojené platformy a BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a><span data-ttu-id="94844-195">Připojit síť toohello platformy</span><span class="sxs-lookup"><span data-stu-id="94844-195">Connect Pi toohello network</span></span>

<span data-ttu-id="94844-196">Zapněte pí pomocí kabelu USB malých hello a hello napájení.</span><span class="sxs-lookup"><span data-stu-id="94844-196">Turn on Pi by using hello micro USB cable and hello power supply.</span></span> <span data-ttu-id="94844-197">Použití hello Ethernet kabel tooconnect pí tooyour drátové sítě nebo postupujte podle hello [pokyny z hello malin platformy Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect pí tooyour bezdrátové sítě.</span><span class="sxs-lookup"><span data-stu-id="94844-197">Use hello Ethernet cable tooconnect Pi tooyour wired network or follow hello [instructions from hello Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="94844-198">Po vaší pí byl úspěšně připojeno toohello sítě, je nutné tootake poznámku o hello [IP adresa vašeho čísla pí](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="94844-198">After your Pi has been successfully connected toohello network, you need tootake a note of hello [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Připojené toowired sítě](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="94844-200">Ujistěte se, že platformy je připojený toohello stejné sítě jako váš počítač.</span><span class="sxs-lookup"><span data-stu-id="94844-200">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="94844-201">Například pokud je počítač připojený tooa bezdrátové sítě připojené tooa drátové síti při instalaci platformy, nemusíte to vidět hello IP adresu ve výstupu devdisco hello.</span><span class="sxs-lookup"><span data-stu-id="94844-201">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="94844-202">Spuštění ukázkové aplikace na platformy</span><span class="sxs-lookup"><span data-stu-id="94844-202">Run a sample application on Pi</span></span>

### <a name="clone-sample-application-and-install-hello-prerequisite-packages"></a><span data-ttu-id="94844-203">Naklonujte ukázkovou aplikaci a nainstalujte požadované balíčky hello</span><span class="sxs-lookup"><span data-stu-id="94844-203">Clone sample application and install hello prerequisite packages</span></span>

1. <span data-ttu-id="94844-204">Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooyour malin pí hello.</span><span class="sxs-lookup"><span data-stu-id="94844-204">Use one of hello following SSH clients from your host computer tooconnect tooyour Raspberry Pi.</span></span>
    - <span data-ttu-id="94844-205">[PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="94844-205">[PuTTY](http://www.putty.org/) for Windows.</span></span> <span data-ttu-id="94844-206">Je třeba hello IP adresu vašeho tooconnect pí pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="94844-206">You need hello IP address of your Pi tooconnect it via SSH.</span></span>
    - <span data-ttu-id="94844-207">Hello integrovaného klienta SSH na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="94844-207">hello built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="94844-208">Stačí spustit `ssh pi@<ip address of pi>` tooconnect platformy prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="94844-208">You might need run `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span></span>

   > [!NOTE] 
   <span data-ttu-id="94844-209">výchozí uživatelské jméno Hello `pi` , a je heslo hello `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="94844-209">hello default username is `pi` , and hello password is `raspberry`.</span></span>

1. <span data-ttu-id="94844-210">Nainstalujte Node.js a NPM tooyour pí.</span><span class="sxs-lookup"><span data-stu-id="94844-210">Install Node.js and NPM tooyour Pi.</span></span>
   
   <span data-ttu-id="94844-211">Nejdřív byste měli zkontrolovat vaše verze Node.js s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="94844-211">First you should check your Node.js version with hello following command.</span></span> 
   
   ```bash
   node -v
   ```

   <span data-ttu-id="94844-212">Pokud verze hello je nižší než 4.x nebo není žádná Node.js na vaše Pi, spusťte následující příkaz tooinstall hello nebo aktualizovat Node.js.</span><span class="sxs-lookup"><span data-stu-id="94844-212">If hello version is lower than 4.x or there is no Node.js on your Pi, then run hello following command tooinstall or update Node.js.</span></span>

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. <span data-ttu-id="94844-213">Klonování hello ukázkovou aplikaci spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="94844-213">Clone hello sample application by running hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. <span data-ttu-id="94844-214">Nainstalujte všechny balíčky podle hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="94844-214">Install all packages by hello following command.</span></span> <span data-ttu-id="94844-215">Obsahuje zařízení Azure IoT SDK, BME280 senzor knihovny a knihovna vzájemné propojení pí.</span><span class="sxs-lookup"><span data-stu-id="94844-215">It includes Azure IoT device SDK, BME280 Sensor library and Wiring Pi library.</span></span>

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   <span data-ttu-id="94844-216">Tento proces denpening instalace může trvat několik minut toofinish na připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="94844-216">It might take several minutes toofinish this installation process denpening on your network connection.</span></span>

### <a name="configure-hello-sample-application"></a><span data-ttu-id="94844-217">Konfigurace hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="94844-217">Configure hello sample application</span></span>

1. <span data-ttu-id="94844-218">Otevřete konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="94844-218">Open hello config file by running hello following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![Konfigurační soubor](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   <span data-ttu-id="94844-220">Existují dvě položky v tomto souboru můžete configurate.</span><span class="sxs-lookup"><span data-stu-id="94844-220">There are two items in this file you can configurate.</span></span> <span data-ttu-id="94844-221">Hello nejprve jeden je `interval`, která definuje hello časový interval mezi dvě zprávy, které odesílají toocloud.</span><span class="sxs-lookup"><span data-stu-id="94844-221">hello first one is `interval`, which defines hello time interval between two messages that send toocloud.</span></span> <span data-ttu-id="94844-222">Hello druhá `simulatedData`, což je logickou hodnotu pro jestli toouse simulated data snímačů, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="94844-222">hello second one `simulatedData`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="94844-223">Pokud jste **nemají hello senzor**, nastavte hello `simulatedData` hodnota příliš`true` toomake hello ukázkovou aplikaci vytváření a používání dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="94844-223">If you **don't have hello sensor**, set hello `simulatedData` value too`true` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="94844-224">Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="94844-224">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="run-hello-sample-application"></a><span data-ttu-id="94844-225">Spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="94844-225">Run hello sample application</span></span>

1. <span data-ttu-id="94844-226">Spuštění ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="94844-226">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="94844-227">Zkontrolujte, zda jste způsobené kopírováním a vkládáním hello zařízení připojovací řetězec do jednoduchých uvozovek a být hello.</span><span class="sxs-lookup"><span data-stu-id="94844-227">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>


<span data-ttu-id="94844-228">Měli byste vidět, že hello následující výstup, že zobrazuje hello senzor dat a hello zpráv, které jsou odeslány tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="94844-228">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Výstup – data snímačů odeslaný tooyour malin platformy IoT hub](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a><span data-ttu-id="94844-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94844-230">Next steps</span></span>

<span data-ttu-id="94844-231">Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="94844-231">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]