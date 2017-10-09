---
title: "aaaRaspberry pí toocloud (Python) - tooAzure připojit malin platformy IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte tooAzure malin platformy IoT Hub pro platformy malin toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot Malinová pi, malinová platformy iot hub, malinová platformy odesílání dat toocloud Malinová platformy toocloud"
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/31/2017
ms.author: xshi
ms.openlocfilehash: 86f5c91ab9dd4e23c563437827fb7d2d06916d2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-python"></a><span data-ttu-id="efe00-104">Připojit tooAzure malin platformy IoT Hub (Python)</span><span class="sxs-lookup"><span data-stu-id="efe00-104">Connect Raspberry Pi tooAzure IoT Hub (Python)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="efe00-105">V tomto kurzu zahájíte učení hello základy práce s malin platformy, na kterém běží Raspbian.</span><span class="sxs-lookup"><span data-stu-id="efe00-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi that's running Raspbian.</span></span> <span data-ttu-id="efe00-106">Pak zjistíte, jak tooseamlessly propojit své cloudové toohello zařízení pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="efe00-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> <span data-ttu-id="efe00-107">Jádro IoT Windows 10 ukázek najdete toohello [Centrum vývojářů pro Windows](http://www.windowsondevices.com/).</span><span class="sxs-lookup"><span data-stu-id="efe00-107">For Windows 10 IoT Core samples, go toohello [Windows Dev Center](http://www.windowsondevices.com/).</span></span>

<span data-ttu-id="efe00-108">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="efe00-108">Don't have a kit yet?</span></span> <span data-ttu-id="efe00-109">Zkuste [online simulátoru malin pí](iot-hub-raspberry-pi-web-simulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="efe00-109">Try [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md).</span></span> <span data-ttu-id="efe00-110">Nebo zakoupení nové kit [zde](https://azure.microsoft.com/develop/iot/starter-kits).</span><span class="sxs-lookup"><span data-stu-id="efe00-110">Or buy a new kit [here](https://azure.microsoft.com/develop/iot/starter-kits).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="efe00-111">Co dělat</span><span class="sxs-lookup"><span data-stu-id="efe00-111">What you do</span></span>

* <span data-ttu-id="efe00-112">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="efe00-112">Create an IoT hub.</span></span>
* <span data-ttu-id="efe00-113">Registrovat zařízení pro platformy ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="efe00-113">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="efe00-114">Instalační program Malinová pí.</span><span class="sxs-lookup"><span data-stu-id="efe00-114">Setup Raspberry Pi.</span></span>
* <span data-ttu-id="efe00-115">Spuštění ukázkové aplikace na platformy toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="efe00-115">Run a sample application on Pi toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="efe00-116">Připojte malin pí tooan IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="efe00-116">Connect Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="efe00-117">Pak spusťte ukázkovou aplikaci pro platformy toocollect teploty a vlhkosti data z BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="efe00-117">Then you run a sample application on Pi toocollect temperature and humidity data from a BME280 sensor.</span></span> <span data-ttu-id="efe00-118">Nakonec odeslat hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="efe00-118">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="efe00-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="efe00-119">What you learn</span></span>

* <span data-ttu-id="efe00-120">Jak toocreate služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="efe00-120">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="efe00-121">Jak tooconnect pí s BME280 senzoru.</span><span class="sxs-lookup"><span data-stu-id="efe00-121">How tooconnect Pi with a BME280 sensor.</span></span>
* <span data-ttu-id="efe00-122">Jak data snímačů toocollect spuštěním ukázkovou aplikaci na pí.</span><span class="sxs-lookup"><span data-stu-id="efe00-122">How toocollect sensor data by running a sample application on Pi.</span></span>
* <span data-ttu-id="efe00-123">Jak toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="efe00-123">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="efe00-124">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="efe00-124">What you need</span></span>

![Co potřebujete](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* <span data-ttu-id="efe00-126">Hello malin pí 2 nebo 3 pí malin panelu.</span><span class="sxs-lookup"><span data-stu-id="efe00-126">hello Raspberry Pi 2 or Raspberry Pi 3 board.</span></span>
* <span data-ttu-id="efe00-127">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="efe00-127">An active Azure subscription.</span></span> <span data-ttu-id="efe00-128">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="efe00-128">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="efe00-129">Monitorování, USB klávesnice a myši připojujících tooPi.</span><span class="sxs-lookup"><span data-stu-id="efe00-129">A monitor, a USB keyboard, and mouse that connect tooPi.</span></span>
* <span data-ttu-id="efe00-130">Mac nebo počítači se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="efe00-130">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="efe00-131">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="efe00-131">An Internet connection.</span></span>
* <span data-ttu-id="efe00-132">16 GB nebo vyšší microSD karta.</span><span class="sxs-lookup"><span data-stu-id="efe00-132">A 16 GB or above microSD card.</span></span>
* <span data-ttu-id="efe00-133">USB SD adaptér nebo microSD karta tooburn hello image operačního systému na kartě microSD hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-133">A USB-SD adapter or microSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="efe00-134">5 volt 2 amp napájení s hello 6 stopy malých kabel USB.</span><span class="sxs-lookup"><span data-stu-id="efe00-134">A 5-volt 2-amp power supply with hello 6-foot micro USB cable.</span></span>

<span data-ttu-id="efe00-135">Hello následující položky jsou volitelné:</span><span class="sxs-lookup"><span data-stu-id="efe00-135">hello following items are optional:</span></span>

* <span data-ttu-id="efe00-136">Sestavený Adafruit BME280 teploty, naléhavost a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="efe00-136">An assembled Adafruit BME280 temperature, pressure, and humidity sensor.</span></span>
* <span data-ttu-id="efe00-137">Breadboard.</span><span class="sxs-lookup"><span data-stu-id="efe00-137">A breadboard.</span></span>
* <span data-ttu-id="efe00-138">Vodičům můstek 6 F/M.</span><span class="sxs-lookup"><span data-stu-id="efe00-138">6 F/M jumper wires.</span></span>
* <span data-ttu-id="efe00-139">Rozptýlený DIODU 10 mm.</span><span class="sxs-lookup"><span data-stu-id="efe00-139">A diffused 10-mm LED.</span></span>


> [!NOTE] 
<span data-ttu-id="efe00-140">Tyto položky jsou volitelné, protože podpora ukázkový kód hello simulated data snímačů.</span><span class="sxs-lookup"><span data-stu-id="efe00-140">These items are optional because hello code sample support simulated sensor data.</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a><span data-ttu-id="efe00-141">Nastavit malin platformy</span><span class="sxs-lookup"><span data-stu-id="efe00-141">Set up Raspberry Pi</span></span>

### <a name="install-hello-raspbian-operating-system-for-pi"></a><span data-ttu-id="efe00-142">Nainstalujte hello Raspbian operační systém pro platformy</span><span class="sxs-lookup"><span data-stu-id="efe00-142">Install hello Raspbian operating system for Pi</span></span>

<span data-ttu-id="efe00-143">Připravte hello microSD karta pro instalaci bitové kopie Raspbian hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-143">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="efe00-144">Stáhněte si Raspbian.</span><span class="sxs-lookup"><span data-stu-id="efe00-144">Download Raspbian.</span></span>
   1. <span data-ttu-id="efe00-145">[Stáhnout Raspbian Klára plochy](https://www.raspberrypi.org/downloads/raspbian/) (soubor .zip hello).</span><span class="sxs-lookup"><span data-stu-id="efe00-145">[Download Raspbian Jessie with Desktop](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip file).</span></span>
   1. <span data-ttu-id="efe00-146">Extrahujte hello Raspbian image tooa složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="efe00-146">Extract hello Raspbian image tooa folder on your computer.</span></span>
1. <span data-ttu-id="efe00-147">Nainstalujte Raspbian toohello microSD karta.</span><span class="sxs-lookup"><span data-stu-id="efe00-147">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="efe00-148">[Stáhněte a nainstalujte nástroj hello Etcher SD karty zapisovací jednotka](https://etcher.io/).</span><span class="sxs-lookup"><span data-stu-id="efe00-148">[Download and install hello Etcher SD card burner utility](https://etcher.io/).</span></span>
   1. <span data-ttu-id="efe00-149">Spusťte Etcher a vyberte bitovou kopii hello Raspbian, které jste extrahovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="efe00-149">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   1. <span data-ttu-id="efe00-150">Vyberte jednotku karty microSD hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-150">Select hello microSD card drive.</span></span> <span data-ttu-id="efe00-151">Všimněte si, že Etcher může jste již vybrali správnou jednotku hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-151">Note that Etcher may have already selected hello correct drive.</span></span>
   1. <span data-ttu-id="efe00-152">Klikněte na Flash tooinstall Raspbian toohello microSD karta.</span><span class="sxs-lookup"><span data-stu-id="efe00-152">Click Flash tooinstall Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="efe00-153">Po dokončení instalace odeberte hello microSD karta z vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="efe00-153">Remove hello microSD card from your computer when installation is complete.</span></span> <span data-ttu-id="efe00-154">Je bezpečné tooremove hello microSD karta přímo protože Etcher automaticky vysune nebo odpojí hello microSD karta po dokončení.</span><span class="sxs-lookup"><span data-stu-id="efe00-154">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   1. <span data-ttu-id="efe00-155">Karta microSD hello vložte do pí.</span><span class="sxs-lookup"><span data-stu-id="efe00-155">Insert hello microSD card into Pi.</span></span>

### <a name="enable-ssh-and-i2c"></a><span data-ttu-id="efe00-156">Povolit SSH a I2C</span><span class="sxs-lookup"><span data-stu-id="efe00-156">Enable SSH and I2C</span></span>

1. <span data-ttu-id="efe00-157">Připojit pí toohello monitoru, klávesnice a myši, spusťte platformy a znovu přihlásili Raspbian pomocí `pi` jako hello uživatelské jméno a `raspberry` jako hello heslo.</span><span class="sxs-lookup"><span data-stu-id="efe00-157">Connect Pi toohello monitor, keyboard and mouse, start Pi and then log in Raspbian by using `pi` as hello user name and `raspberry` as hello password.</span></span>
1. <span data-ttu-id="efe00-158">Klikněte na tlačítko hello Malinová ikonu > **Předvolby** > **malin pí konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="efe00-158">Click hello Raspberry icon > **Preferences** > **Raspberry Pi Configuration**.</span></span>

   ![Hello Raspbian předvolby nabídky](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. <span data-ttu-id="efe00-160">Na hello **rozhraní** nastavte **I2C** a **SSH** příliš**povolit**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="efe00-160">On hello **Interfaces** tab, set **I2C** and **SSH** too**Enable**, and then click **OK**.</span></span> <span data-ttu-id="efe00-161">Pokud nemáte fyzické senzory a chcete data snímačů toouse simulated, tento krok je volitelný.</span><span class="sxs-lookup"><span data-stu-id="efe00-161">If you don't have physical sensors and want toouse simulated sensor data, this step is optional.</span></span>

   ![Povolit I2C a SSH na Malinová platformy](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
<span data-ttu-id="efe00-163">tooenable SSH a I2C, můžete najít další dokumenty odkaz na [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) a [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span><span class="sxs-lookup"><span data-stu-id="efe00-163">tooenable SSH and I2C, you can find more reference documents on [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) and [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).</span></span>

### <a name="connect-hello-sensor-toopi"></a><span data-ttu-id="efe00-164">Připojit tooPi senzor hello</span><span class="sxs-lookup"><span data-stu-id="efe00-164">Connect hello sensor tooPi</span></span>

<span data-ttu-id="efe00-165">Použijte hello breadboard a můstek vodičům tooconnect DIODU a BME280 tooPi následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="efe00-165">Use hello breadboard and jumper wires tooconnect an LED and a BME280 tooPi as follows.</span></span> <span data-ttu-id="efe00-166">Pokud nemáte hello senzor [tuto část přeskočte](#connect-pi-to-the-network).</span><span class="sxs-lookup"><span data-stu-id="efe00-166">If you don’t have hello sensor, [skip this section](#connect-pi-to-the-network).</span></span>

![Hello malin platformy a senzor připojení](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

<span data-ttu-id="efe00-168">Hello BME280 senzor teploty a vlhkosti data můžete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="efe00-168">hello BME280 sensor can collect temperature and humidity data.</span></span> <span data-ttu-id="efe00-169">A hello DIODU bude blink – Pokud je komunikace mezi zařízením a hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="efe00-169">And hello LED will blink if there is a communication between device and hello cloud.</span></span> 

<span data-ttu-id="efe00-170">Senzor kód PIN použijte následující kabeláž hello:</span><span class="sxs-lookup"><span data-stu-id="efe00-170">For sensor pins, use hello following wiring:</span></span>

| <span data-ttu-id="efe00-171">Spuštění (senzor & DIODU)</span><span class="sxs-lookup"><span data-stu-id="efe00-171">Start (Sensor & LED)</span></span>     | <span data-ttu-id="efe00-172">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="efe00-172">End (Board)</span></span>            | <span data-ttu-id="efe00-173">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="efe00-173">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="efe00-174">VDD (Pin 5G)</span><span class="sxs-lookup"><span data-stu-id="efe00-174">VDD (Pin 5G)</span></span>             | <span data-ttu-id="efe00-175">3.3v opravná (Pin 1)</span><span class="sxs-lookup"><span data-stu-id="efe00-175">3.3V PWR (Pin 1)</span></span>       | <span data-ttu-id="efe00-176">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="efe00-176">White cable</span></span>   |
| <span data-ttu-id="efe00-177">ZEM (Pin 7G)</span><span class="sxs-lookup"><span data-stu-id="efe00-177">GND (Pin 7G)</span></span>             | <span data-ttu-id="efe00-178">ZEM (Pin 6)</span><span class="sxs-lookup"><span data-stu-id="efe00-178">GND (Pin 6)</span></span>            | <span data-ttu-id="efe00-179">Hnědá kabel</span><span class="sxs-lookup"><span data-stu-id="efe00-179">Brown cable</span></span>   |
| <span data-ttu-id="efe00-180">SDI (Pin 10G)</span><span class="sxs-lookup"><span data-stu-id="efe00-180">SDI (Pin 10G)</span></span>            | <span data-ttu-id="efe00-181">I2C1 SDA (PIN kódu 3)</span><span class="sxs-lookup"><span data-stu-id="efe00-181">I2C1 SDA (Pin 3)</span></span>       | <span data-ttu-id="efe00-182">Red kabel</span><span class="sxs-lookup"><span data-stu-id="efe00-182">Red cable</span></span>     |
| <span data-ttu-id="efe00-183">SCK (Pin 8G)</span><span class="sxs-lookup"><span data-stu-id="efe00-183">SCK (Pin 8G)</span></span>             | <span data-ttu-id="efe00-184">I2C1 SCL (Pin 5)</span><span class="sxs-lookup"><span data-stu-id="efe00-184">I2C1 SCL (Pin 5)</span></span>       | <span data-ttu-id="efe00-185">Oranžové kabel</span><span class="sxs-lookup"><span data-stu-id="efe00-185">Orange cable</span></span>  |
| <span data-ttu-id="efe00-186">Indikátor VDD (Pin 18F)</span><span class="sxs-lookup"><span data-stu-id="efe00-186">LED VDD (Pin 18F)</span></span>        | <span data-ttu-id="efe00-187">GPIO 24 (Pin 18)</span><span class="sxs-lookup"><span data-stu-id="efe00-187">GPIO 24 (Pin 18)</span></span>       | <span data-ttu-id="efe00-188">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="efe00-188">White cable</span></span>   |
| <span data-ttu-id="efe00-189">Indikátor zem (Pin 17F)</span><span class="sxs-lookup"><span data-stu-id="efe00-189">LED GND (Pin 17F)</span></span>        | <span data-ttu-id="efe00-190">ZEM (Pin 20)</span><span class="sxs-lookup"><span data-stu-id="efe00-190">GND (Pin 20)</span></span>           | <span data-ttu-id="efe00-191">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="efe00-191">Black cable</span></span>   |

<span data-ttu-id="efe00-192">Klikněte na tlačítko tooview [malin pí 2 a 3 mapování kódu Pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) pro vaši informaci.</span><span class="sxs-lookup"><span data-stu-id="efe00-192">Click tooview [Raspberry Pi 2 & 3 Pin mappings](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) for your reference.</span></span>

<span data-ttu-id="efe00-193">Po úspěšném připojení BME280 tooyour malin Pi, mělo by být jako níže bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="efe00-193">After you've successfully connected BME280 tooyour Raspberry Pi, it should be like below image.</span></span>

![Připojené platformy a BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a><span data-ttu-id="efe00-195">Připojit síť toohello platformy</span><span class="sxs-lookup"><span data-stu-id="efe00-195">Connect Pi toohello network</span></span>

<span data-ttu-id="efe00-196">Zapněte pí pomocí kabelu USB malých hello a hello napájení.</span><span class="sxs-lookup"><span data-stu-id="efe00-196">Turn on Pi by using hello micro USB cable and hello power supply.</span></span> <span data-ttu-id="efe00-197">Použití hello Ethernet kabel tooconnect pí tooyour drátové sítě nebo postupujte podle hello [pokyny z hello malin platformy Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect pí tooyour bezdrátové sítě.</span><span class="sxs-lookup"><span data-stu-id="efe00-197">Use hello Ethernet cable tooconnect Pi tooyour wired network or follow hello [instructions from hello Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="efe00-198">Po vaší pí byl úspěšně připojeno toohello sítě, je nutné tootake poznámku o hello [IP adresa vašeho čísla pí](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span><span class="sxs-lookup"><span data-stu-id="efe00-198">After your Pi has been successfully connected toohello network, you need tootake a note of hello [IP address of your Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).</span></span>

![Připojené toowired sítě](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> <span data-ttu-id="efe00-200">Ujistěte se, že platformy je připojený toohello stejné sítě jako váš počítač.</span><span class="sxs-lookup"><span data-stu-id="efe00-200">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="efe00-201">Například pokud je počítač připojený tooa bezdrátové sítě připojené tooa drátové síti při instalaci platformy, nemusíte to vidět hello IP adresu ve výstupu devdisco hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-201">For example, if your computer is connected tooa wireless network while Pi is connected tooa wired network, you might not see hello IP address in hello devdisco output.</span></span>

## <a name="run-a-sample-application-on-pi"></a><span data-ttu-id="efe00-202">Spuštění ukázkové aplikace na platformy</span><span class="sxs-lookup"><span data-stu-id="efe00-202">Run a sample application on Pi</span></span>

### <a name="install-hello-prerequisite-packages"></a><span data-ttu-id="efe00-203">Instalace požadovaných součástí balíčků hello</span><span class="sxs-lookup"><span data-stu-id="efe00-203">Install hello prerequisite packages</span></span>

<span data-ttu-id="efe00-204">Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooyour malin pí hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-204">Use one of hello following SSH clients from your host computer tooconnect tooyour Raspberry Pi.</span></span>
   
   <span data-ttu-id="efe00-205">**Uživatelé s Windows**</span><span class="sxs-lookup"><span data-stu-id="efe00-205">**Windows Users**</span></span>
   1. <span data-ttu-id="efe00-206">Stáhněte a nainstalujte [PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="efe00-206">Download and install [PuTTY](http://www.putty.org/) for Windows.</span></span> 
   1. <span data-ttu-id="efe00-207">Zkopírujte hello IP adresa vaší oddílu pí do hello hostitele název (nebo IP adresu) a vyberte jako typ hello připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="efe00-207">Copy hello IP address of your Pi into hello Host name (or IP address) section and select SSH as hello connection type.</span></span>
   
   
   <span data-ttu-id="efe00-208">**Mac a Ubuntu uživatelů**</span><span class="sxs-lookup"><span data-stu-id="efe00-208">**Mac and Ubuntu Users**</span></span>
   
   <span data-ttu-id="efe00-209">Použijte integrovaného klienta SSH hello na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="efe00-209">Use hello built-in SSH client on Ubuntu or macOS.</span></span> <span data-ttu-id="efe00-210">Může být nutné toorun `ssh pi@<ip address of pi>` tooconnect platformy prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="efe00-210">You might need toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.</span></span>
   > [!NOTE] 
   <span data-ttu-id="efe00-211">výchozí uživatelské jméno Hello `pi` , a je heslo hello `raspberry`.</span><span class="sxs-lookup"><span data-stu-id="efe00-211">hello default username is `pi` , and hello password is `raspberry`.</span></span>


### <a name="configure-hello-sample-application"></a><span data-ttu-id="efe00-212">Konfigurace hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="efe00-212">Configure hello sample application</span></span>

1. <span data-ttu-id="efe00-213">Klonování hello ukázkovou aplikaci spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="efe00-213">Clone hello sample application by running hello following command:</span></span>

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. <span data-ttu-id="efe00-214">Otevřete konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="efe00-214">Open hello config file by running hello following commands:</span></span>

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   <span data-ttu-id="efe00-215">5 maker v tomto souboru můžete configurate.</span><span class="sxs-lookup"><span data-stu-id="efe00-215">There are 5 macros in this file you can configurate.</span></span> <span data-ttu-id="efe00-216">Hello nejprve jeden je `MESSAGE_TIMESPAN`, která definuje hello časový interval (v milisekundách) mezi dvě zprávy, které odesílají toocloud.</span><span class="sxs-lookup"><span data-stu-id="efe00-216">hello first one is `MESSAGE_TIMESPAN`, which defines hello time interval (in milliseconds) between two messages that send toocloud.</span></span> <span data-ttu-id="efe00-217">Hello druhá `SIMULATED_DATA`, což je logickou hodnotu pro jestli toouse simulated data snímačů, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="efe00-217">hello second one `SIMULATED_DATA`, which is a Boolean value for whether toouse simulated sensor data or not.</span></span> <span data-ttu-id="efe00-218">`I2C_ADDRESS`je hello I2C adresa, která je připojena vaše BME280 snímač.</span><span class="sxs-lookup"><span data-stu-id="efe00-218">`I2C_ADDRESS` is hello I2C address which your BME280 sensor is connected.</span></span> <span data-ttu-id="efe00-219">`GPIO_PIN_ADDRESS`pro vaše DIODU je adresa GPIO hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-219">`GPIO_PIN_ADDRESS` is hello GPIO address for your LED.</span></span> <span data-ttu-id="efe00-220">Hello poslední jeden je `BLINK_TIMESPAN`, která definována hello časový interval, po zapnutí vaší DIODU v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="efe00-220">hello last one is `BLINK_TIMESPAN`, which defined hello timespan when your LED is turned on in milliseconds.</span></span>

   <span data-ttu-id="efe00-221">Pokud jste **nemají hello senzor**, nastavte hello `SIMULATED_DATA` hodnota příliš`True` toomake hello ukázkovou aplikaci vytváření a používání dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="efe00-221">If you **don't have hello sensor**, set hello `SIMULATED_DATA` value too`True` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="efe00-222">Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="efe00-222">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="build-and-run-hello-sample-application"></a><span data-ttu-id="efe00-223">Sestavení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="efe00-223">Build and run hello sample application</span></span>

1. <span data-ttu-id="efe00-224">Sestavení hello ukázkovou aplikaci tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-224">Build hello sample application by running hello following command.</span></span> <span data-ttu-id="efe00-225">Protože hello SDK služby Azure IoT pro jazyk Python – obálky nad hello sady SDK C zařízení Azure IoT, toocompile hello C knihovny je nutné, pokud chcete nebo potřebujete toogenerate hello Python knihovny ze zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="efe00-225">Because hello Azure IoT SDKs for Python are wrappers on top of hello Azure IoT Device C SDK, you will need toocompile hello C libraries if you want or need toogenerate hello Python libraries from source code.</span></span>

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   <span data-ttu-id="efe00-226">Můžete také určit verzi hello chcete spuštěním `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`.</span><span class="sxs-lookup"><span data-stu-id="efe00-226">You can also specify hello version you want by running `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`.</span></span> <span data-ttu-id="efe00-227">Pokud spustíte skript bez parametrů, skript hello automaticky zjistí hello verzi jazyka python nainstalovaný (pořadí hledání 2.7 -> 3.4 -> 3.5).</span><span class="sxs-lookup"><span data-stu-id="efe00-227">If you run script without parameter, hello script will automatically detect hello version of python installed (Search sequence 2.7->3.4->3.5).</span></span> <span data-ttu-id="efe00-228">Ujistěte se, že vaše verze Python zajišťuje konzistentní během vytváření a spouštění.</span><span class="sxs-lookup"><span data-stu-id="efe00-228">Make sure your Python version keeps consistent during building and running.</span></span> 
   
   > [!NOTE] 
   <span data-ttu-id="efe00-229">Na budování hello Python klientské knihovny (iothub_client.so) na Linux zařízení, které mají méně než 1GB paměti RAM, mohou se zobrazit sestavení získávání zablokované ve 98 % při sestavování iothub_client_python.cpp, jak je uvedeno níže `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`.</span><span class="sxs-lookup"><span data-stu-id="efe00-229">On building hello Python client library (iothub_client.so) on Linux devices that have less than 1GB RAM, you may see build getting stuck at 98% while building iothub_client_python.cpp as shown below `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`.</span></span> <span data-ttu-id="efe00-230">Pokud narazíte na potíže, zkontrolujte využití paměti hello hello zařízení pomocí `free -m command` v jiné okno terminálu během této doby.</span><span class="sxs-lookup"><span data-stu-id="efe00-230">If you run into this issue, check hello memory consumption of hello device using `free -m command` in another terminal window during that time.</span></span> <span data-ttu-id="efe00-231">Pokud používáte nedostatek paměti při kompilování souborů iothub_client_python.cpp, může mít tootemporarily zvýšit tooget místa odkládacího souboru hello k dispozici další paměť toosuccessfully sestavení hello Python klientské knihovny SDK zařízení.</span><span class="sxs-lookup"><span data-stu-id="efe00-231">If you are running out of memory while compiling iothub_client_python.cpp file, you may have tootemporarily increase hello swap space tooget more available memory toosuccessfully build hello Python client-side device SDK library.</span></span>
   
1. <span data-ttu-id="efe00-232">Spuštění ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="efe00-232">Run hello sample application by running hello following command:</span></span>

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="efe00-233">Zkontrolujte, zda jste způsobené kopírováním a vkládáním hello zařízení připojovací řetězec do jednoduchých uvozovek a být hello.</span><span class="sxs-lookup"><span data-stu-id="efe00-233">Make sure you copy-paste hello device connection string into hello single quotes.</span></span> <span data-ttu-id="efe00-234">A pokud používáte hello python 3, pak můžete použít příkaz hello `python3 app.py '<your Azure IoT hub device connection string>'`.</span><span class="sxs-lookup"><span data-stu-id="efe00-234">And if you use hello python 3, then you can use hello command `python3 app.py '<your Azure IoT hub device connection string>'`.</span></span>


   <span data-ttu-id="efe00-235">Měli byste vidět, že hello následující výstup, že zobrazuje hello senzor dat a hello zpráv, které jsou odeslány tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="efe00-235">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

   ![Výstup – data snímačů odeslaný tooyour malin platformy IoT hub](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a><span data-ttu-id="efe00-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="efe00-237">Next steps</span></span>

<span data-ttu-id="efe00-238">Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="efe00-238">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span> <span data-ttu-id="efe00-239">toosee hello zprávy, že vaše platformy malin odeslal tooyour IoT hub nebo odesílání zprávy tooyour malin platformy v rozhraní příkazového řádku, najdete v části hello [cloud zařízení spravovat zasílání zpráv s iothub-explorer kurzu](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span><span class="sxs-lookup"><span data-stu-id="efe00-239">toosee hello messages that your Raspberry Pi has sent tooyour IoT hub or send messages tooyour Raspberry Pi in a command line interface, see hello [Manage cloud device messaging with iothub-explorer tutorial](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
