---
title: "M0 toocloud: připojení Wi-Fi M0 prolnutí tooAzure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak tooset až a připojení přes Wi-Fi Adafruit prolnutí M0 tooAzure IoT Hub toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/16/2017
ms.author: xshi
ms.openlocfilehash: 6aabeb961a50ba5d3934f77eb1ccda4af1bf64c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-m0-wifi-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="50f24-103">Připojení přes Wi-Fi Adafruit prolnutí M0 tooAzure IoT Hub v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="50f24-103">Connect Adafruit Feather M0 WiFi tooAzure IoT Hub in hello cloud</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Připojení mezi BME280, prolnutí M0 Wi-Fi a IoT Hub](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

<span data-ttu-id="50f24-105">V tomto kurzu zahájíte učení hello základy práce s Arduino panel.</span><span class="sxs-lookup"><span data-stu-id="50f24-105">In this tutorial, you begin by learning hello basics of working with your Arduino board.</span></span> <span data-ttu-id="50f24-106">Pak zjistíte, jak tooseamlessly propojit své cloudové toohello zařízení pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="50f24-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="50f24-107">Co dělat</span><span class="sxs-lookup"><span data-stu-id="50f24-107">What you do</span></span>

<span data-ttu-id="50f24-108">Připojte centra IoT tooan Adafruit prolnutí M0 Wi-Fi, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="50f24-108">Connect Adafruit Feather M0 WiFi tooan IoT hub that you create.</span></span> <span data-ttu-id="50f24-109">Pak spusťte ukázkovou aplikaci na Wi-Fi M0 toocollect hello teploty a vlhkosti dat z BME280.</span><span class="sxs-lookup"><span data-stu-id="50f24-109">Then you run a sample application on M0 WiFi toocollect hello temperature and humidity data from a BME280.</span></span> <span data-ttu-id="50f24-110">Nakonec odeslat hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="50f24-110">Finally, you send hello sensor data tooyour IoT hub.</span></span>


## <a name="what-you-learn"></a><span data-ttu-id="50f24-111">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="50f24-111">What you learn</span></span>

* <span data-ttu-id="50f24-112">Jak toocreate služby IoT hub a registrovat zařízení pro prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="50f24-112">How toocreate an IoT hub and register a device for Feather M0 WiFi</span></span>
* <span data-ttu-id="50f24-113">Jak tooconnect prolnutí M0 Wi-Fi s hello senzor a počítač</span><span class="sxs-lookup"><span data-stu-id="50f24-113">How tooconnect Feather M0 WiFi with hello sensor and your computer</span></span>
* <span data-ttu-id="50f24-114">Jak data snímačů toocollect spuštěním ukázkovou aplikaci na prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="50f24-114">How toocollect sensor data by running a sample application on Feather M0 WiFi</span></span>
* <span data-ttu-id="50f24-115">Jak toosend hello senzor data tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="50f24-115">How toosend hello sensor data tooyour IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="50f24-116">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="50f24-116">What you need</span></span>

![součásti potřebné pro kurz hello](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="50f24-118">toocomplete tuto operaci je třeba hello následujících částí z vaší prolnutí M0 Wi-Fi Starter Kit:</span><span class="sxs-lookup"><span data-stu-id="50f24-118">toocomplete this operation, you need hello following parts from your Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="50f24-119">Hello Tabule prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="50f24-119">hello Feather M0 WiFi board</span></span>
* <span data-ttu-id="50f24-120">Micro USB tooType kabelu A USB</span><span class="sxs-lookup"><span data-stu-id="50f24-120">A Micro USB tooType A USB cable</span></span>

<span data-ttu-id="50f24-121">Musíte taky hello následujících věcí pro vývojové prostředí:</span><span class="sxs-lookup"><span data-stu-id="50f24-121">You also need hello following things for your development environment:</span></span>

* <span data-ttu-id="50f24-122">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="50f24-122">An active Azure subscription.</span></span> <span data-ttu-id="50f24-123">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="50f24-123">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="50f24-124">Mac nebo počítači se systémem Windows nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="50f24-124">A Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="50f24-125">Bezdrátové sítě Wi-Fi M0 prolnutí tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="50f24-125">A wireless network for Feather M0 WiFi tooconnect to.</span></span>
* <span data-ttu-id="50f24-126">Internetu toodownload hello konfigurační nástroj pro připojení.</span><span class="sxs-lookup"><span data-stu-id="50f24-126">An Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="50f24-127">[Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="50f24-127">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="50f24-128">Starší verze nepodporují s knihovnou Azure IoT Hub hello.</span><span class="sxs-lookup"><span data-stu-id="50f24-128">Earlier versions don't work with hello Azure IoT Hub library.</span></span>

<span data-ttu-id="50f24-129">Pokud nemáte senzoru, hello následující položky jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="50f24-129">If you don’t have a sensor, hello following items are optional.</span></span> <span data-ttu-id="50f24-130">Máte také možnost hello pomocí simulované senzor dat:</span><span class="sxs-lookup"><span data-stu-id="50f24-130">You also have hello option of using simulated sensor data:</span></span>

* <span data-ttu-id="50f24-131">Senzor teploty a vlhkosti BME280</span><span class="sxs-lookup"><span data-stu-id="50f24-131">A BME280 temperature and humidity sensor</span></span>
* <span data-ttu-id="50f24-132">Breadboard</span><span class="sxs-lookup"><span data-stu-id="50f24-132">A breadboard</span></span>
* <span data-ttu-id="50f24-133">Vodičům můstek M/M</span><span class="sxs-lookup"><span data-stu-id="50f24-133">M/M jumper wires</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-hello-sensor-and-your-computer"></a><span data-ttu-id="50f24-134">Připojení Wi-Fi M0 prolnutí s hello senzor a počítače</span><span class="sxs-lookup"><span data-stu-id="50f24-134">Connect Feather M0 WiFi with hello sensor and your computer</span></span>
<span data-ttu-id="50f24-135">V této části připojit hello senzorů tooyour panelu.</span><span class="sxs-lookup"><span data-stu-id="50f24-135">In this section, you connect hello sensors tooyour board.</span></span> <span data-ttu-id="50f24-136">Potom připojíte počítač tooyour zařízení pro další použití.</span><span class="sxs-lookup"><span data-stu-id="50f24-136">Then you plug in your device tooyour computer for further use.</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-m0-wifi"></a><span data-ttu-id="50f24-137">Připojit DHT22 teploty a vlhkosti senzor tooFeather M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="50f24-137">Connect a DHT22 temperature and humidity sensor tooFeather M0 WiFi</span></span>

<span data-ttu-id="50f24-138">Použití hello breadboard a můstek vodičům toomake hello připojení.</span><span class="sxs-lookup"><span data-stu-id="50f24-138">Use hello breadboard and jumper wires toomake hello connection.</span></span> <span data-ttu-id="50f24-139">Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.</span><span class="sxs-lookup"><span data-stu-id="50f24-139">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![odkaz na připojení](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


<span data-ttu-id="50f24-141">Senzor kód PIN použijte následující kabeláž hello:</span><span class="sxs-lookup"><span data-stu-id="50f24-141">For sensor pins, use hello following wiring:</span></span>


| <span data-ttu-id="50f24-142">Spuštění (senzor)</span><span class="sxs-lookup"><span data-stu-id="50f24-142">Start (sensor)</span></span>           | <span data-ttu-id="50f24-143">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="50f24-143">End (board)</span></span>            | <span data-ttu-id="50f24-144">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="50f24-144">Cable color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="50f24-145">VDD (Pin 27A)</span><span class="sxs-lookup"><span data-stu-id="50f24-145">VDD (Pin 27A)</span></span>            | <span data-ttu-id="50f24-146">3v (3A PIN kódu)</span><span class="sxs-lookup"><span data-stu-id="50f24-146">3V (Pin 3A)</span></span>            | <span data-ttu-id="50f24-147">Red kabel</span><span class="sxs-lookup"><span data-stu-id="50f24-147">Red cable</span></span>     |
| <span data-ttu-id="50f24-148">ZEM (Pin 29A)</span><span class="sxs-lookup"><span data-stu-id="50f24-148">GND (Pin 29A)</span></span>            | <span data-ttu-id="50f24-149">ZEM (6A PIN kódu)</span><span class="sxs-lookup"><span data-stu-id="50f24-149">GND (Pin 6A)</span></span>           | <span data-ttu-id="50f24-150">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="50f24-150">Black cable</span></span>   |
| <span data-ttu-id="50f24-151">SCK (Pin 30A)</span><span class="sxs-lookup"><span data-stu-id="50f24-151">SCK (Pin 30A)</span></span>            | <span data-ttu-id="50f24-152">SCK (Pin 12A)</span><span class="sxs-lookup"><span data-stu-id="50f24-152">SCK (Pin 12A)</span></span>          | <span data-ttu-id="50f24-153">Žlutý kabel</span><span class="sxs-lookup"><span data-stu-id="50f24-153">Yellow cable</span></span>  |
| <span data-ttu-id="50f24-154">SDO (Pin 31A)</span><span class="sxs-lookup"><span data-stu-id="50f24-154">SDO (Pin 31A)</span></span>            | <span data-ttu-id="50f24-155">MI (Pin 14A)</span><span class="sxs-lookup"><span data-stu-id="50f24-155">MI (Pin 14A)</span></span>           | <span data-ttu-id="50f24-156">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="50f24-156">White cable</span></span>   |
| <span data-ttu-id="50f24-157">SDI (Pin 32A)</span><span class="sxs-lookup"><span data-stu-id="50f24-157">SDI (Pin 32A)</span></span>            | <span data-ttu-id="50f24-158">M0 (Pin 13A)</span><span class="sxs-lookup"><span data-stu-id="50f24-158">M0 (Pin 13A)</span></span>           | <span data-ttu-id="50f24-159">Modrý kabel</span><span class="sxs-lookup"><span data-stu-id="50f24-159">Blue cable</span></span>    |
| <span data-ttu-id="50f24-160">CS (Pin 33A)</span><span class="sxs-lookup"><span data-stu-id="50f24-160">CS (Pin 33A)</span></span>             | <span data-ttu-id="50f24-161">GPIO 5 (Pin 15J)</span><span class="sxs-lookup"><span data-stu-id="50f24-161">GPIO 5 (Pin 15J)</span></span>       | <span data-ttu-id="50f24-162">Oranžové kabel</span><span class="sxs-lookup"><span data-stu-id="50f24-162">Orange cable</span></span>  |

<span data-ttu-id="50f24-163">Další informace najdete v tématu [Adafruit BME280 vlhkosti + barometrický tlak + teplotní snímač volnými](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) a [uspořádání kolíků Adafruit prolnutí M0 Wi-Fi](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span><span class="sxs-lookup"><span data-stu-id="50f24-163">For more information, see [Adafruit BME280 Humidity + Barometric Pressure + Temperature Sensor Breakout](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) and [Adafruit Feather M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span></span>



<span data-ttu-id="50f24-164">Nyní by měly být připojené vaší prolnutí M0 Wi-Fi s pracovní senzoru.</span><span class="sxs-lookup"><span data-stu-id="50f24-164">Now your Feather M0 WiFi should be connected with a working sensor.</span></span>

![Spojte se s prolnutí Huzzah DHT22](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-tooyour-computer"></a><span data-ttu-id="50f24-166">Připojte počítač tooyour prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="50f24-166">Connect Feather M0 WiFi tooyour computer</span></span>

<span data-ttu-id="50f24-167">Použijte hello Micro USB tooType A USB kabel tooconnect prolnutí M0 Wi-Fi tooyour počítač, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="50f24-167">Use hello Micro USB tooType A USB cable tooconnect Feather M0 WiFi tooyour computer, as shown:</span></span>

![Připojte počítač tooyour prolnutí Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="50f24-169">Přidání oprávnění sériového portu (pouze Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="50f24-169">Add serial port permissions (Ubuntu only)</span></span>

<span data-ttu-id="50f24-170">Pokud používáte Ubuntu, ujistěte se, že máte oprávnění toooperate hello na hello USB port z prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="50f24-170">If you use Ubuntu, make sure you have hello permissions toooperate on hello USB port of Feather M0 WiFi.</span></span> <span data-ttu-id="50f24-171">oprávnění tooadd sériového portu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="50f24-171">tooadd serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="50f24-172">V terminálu spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="50f24-172">At a terminal, run hello following commands:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="50f24-173">Zobrazí jednu z následujících výstupy hello:</span><span class="sxs-lookup"><span data-stu-id="50f24-173">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="50f24-174">CRW-rw---1 kořenové uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="50f24-174">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="50f24-175">CRW-rw---xxxxxxxx síti službou 1 root</span><span class="sxs-lookup"><span data-stu-id="50f24-175">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="50f24-176">Ve výstupu hello, Všimněte si, že `uucp` nebo `dialout` je název vlastníka skupiny hello hello portu USB.</span><span class="sxs-lookup"><span data-stu-id="50f24-176">In hello output, notice that `uucp` or `dialout` is hello group owner name of hello USB port.</span></span>

2. <span data-ttu-id="50f24-177">tooadd hello toohello skupinu uživatelů, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="50f24-177">tooadd hello user toohello group, run hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="50f24-178">V předchozím kroku hello získat název vlastníka skupiny hello `<group-owner-name>`.</span><span class="sxs-lookup"><span data-stu-id="50f24-178">In hello previous step, you obtained hello group owner name `<group-owner-name>`.</span></span> <span data-ttu-id="50f24-179">Uživatelské jméno Ubuntu `<username>`.</span><span class="sxs-lookup"><span data-stu-id="50f24-179">Your Ubuntu user name is `<username>`.</span></span>

3. <span data-ttu-id="50f24-180">Pro změnu tooappear hello odhlaste se od Ubuntu a potom se znovu přihlaste.</span><span class="sxs-lookup"><span data-stu-id="50f24-180">For hello change tooappear, sign out of Ubuntu and then sign in again.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="50f24-181">Shromažďování dat snímačů a odeslat tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="50f24-181">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="50f24-182">V této části nasazení a spuštění ukázkové aplikace na prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="50f24-182">In this section, you deploy and run a sample application on Feather M0 WiFi.</span></span> <span data-ttu-id="50f24-183">Ukázková aplikace Hello díky hello DIODU blikání na prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="50f24-183">hello sample application makes hello LED blink on Feather M0 WiFi.</span></span> <span data-ttu-id="50f24-184">Pak odešle hello teploty a vlhkosti data shromážděná z hello BME280 senzor tooyour služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="50f24-184">It then sends hello temperature and humidity data collected from hello BME280 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github-and-prepare-hello-arduino-ide"></a><span data-ttu-id="50f24-185">Získat hello ukázkovou aplikaci z webu GitHub a příprava hello Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="50f24-185">Get hello sample application from GitHub and prepare hello Arduino IDE</span></span>

<span data-ttu-id="50f24-186">Ukázková aplikace Hello jsou hostované na Githubu.</span><span class="sxs-lookup"><span data-stu-id="50f24-186">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="50f24-187">Klonování úložiště hello ukázka, která obsahuje ukázkovou aplikaci hello z Githubu.</span><span class="sxs-lookup"><span data-stu-id="50f24-187">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="50f24-188">tooclone hello Ukázka úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="50f24-188">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="50f24-189">Otevřete příkazový řádek nebo okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="50f24-189">Open a command prompt or a terminal window.</span></span>

2. <span data-ttu-id="50f24-190">Přejděte tooa složku, kam chcete hello ukázkové aplikace toobe uložené.</span><span class="sxs-lookup"><span data-stu-id="50f24-190">Go tooa folder where you want hello sample application toobe stored.</span></span>
3. <span data-ttu-id="50f24-191">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="50f24-191">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-hello-package-for-feather-m0-wifi-in-hello-arduino-ide"></a><span data-ttu-id="50f24-192">Instalovat balíček hello pro prolnutí M0 Wi-Fi v hello Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="50f24-192">Install hello package for Feather M0 WiFi in hello Arduino IDE</span></span>

1. <span data-ttu-id="50f24-193">Otevřete složku hello se uloží hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="50f24-193">Open hello folder where hello sample application is stored.</span></span>

2. <span data-ttu-id="50f24-194">Otevřete soubor app.ino hello ve složce aplikace hello v hello Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="50f24-194">Open hello app.ino file in hello app folder in hello Arduino IDE.</span></span>

   ![Otevřete hello ukázkovou aplikaci v Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. <span data-ttu-id="50f24-196">Klikněte na tlačítko **soubor** > **Předvolby** (Windows nebo Linuxem) nebo **Arduino** > **Předvolby** (Mac) a zkopírujte a Vložení hello dole na odkaz do hello **další adresy URL Manager panely** možnost hello předvolby Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="50f24-196">Click **File** > **Preferences** (Windows/Linux) or **Arduino** > **Preferences** (Mac) and copy and paste hello link below into hello **Additional Boards Manager URLs** option in hello Arduino IDE preferences.</span></span>
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. <span data-ttu-id="50f24-197">Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a pak nainstalujte hello `Arduino SAMD Boards` verze `1.6.2` nebo novější.</span><span class="sxs-lookup"><span data-stu-id="50f24-197">Click **Tools** > **Board** > **Boards Manager**, and then install hello `Arduino SAMD Boards` version `1.6.2` or later.</span></span> 

1. <span data-ttu-id="50f24-198">Potom v hello stejného časového období, nainstalujte `Adafruit SAMD Boards` tooadd hello Tabule soubor definice balíčku.</span><span class="sxs-lookup"><span data-stu-id="50f24-198">Then in hello same window, install `Adafruit SAMD Boards` package tooadd hello board file definitions.</span></span>

   ![je nainstalovaný balíček esp8266 Hello](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. <span data-ttu-id="50f24-200">Klikněte na tlačítko **nástroje** > **Tabule** > **Adafruit M0 Wi-Fi**.</span><span class="sxs-lookup"><span data-stu-id="50f24-200">Click **Tools** > **Board** > **Adafruit M0 WiFi**.</span></span>

5. <span data-ttu-id="50f24-201">Instalace ovladačů (jenom Windows).</span><span class="sxs-lookup"><span data-stu-id="50f24-201">Install drivers (for Windows only).</span></span> <span data-ttu-id="50f24-202">Po připojení Wi-Fi M0 prolnutí, může být nutné tooinstall ovladač.</span><span class="sxs-lookup"><span data-stu-id="50f24-202">When you plug in Feather M0 WiFi, you might need tooinstall a driver.</span></span> <span data-ttu-id="50f24-203">Klikněte na tlačítko [hello odkaz ke stažení na webovou stránku hello](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello ovladač Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="50f24-203">Click [hello download link on hello webpage](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello driver installer.</span></span> <span data-ttu-id="50f24-204">Postupujte podle hello kroky tooinstall hello ovladače, které chcete.</span><span class="sxs-lookup"><span data-stu-id="50f24-204">Follow hello steps tooinstall hello drivers you want.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="50f24-205">Instalace nezbytné knihovny</span><span class="sxs-lookup"><span data-stu-id="50f24-205">Install necessary libraries</span></span>

1. <span data-ttu-id="50f24-206">V hello Arduino IDE, klikněte na **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.</span><span class="sxs-lookup"><span data-stu-id="50f24-206">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>

2. <span data-ttu-id="50f24-207">Vyhledejte následující názvy knihoven jeden po druhém hello.</span><span class="sxs-lookup"><span data-stu-id="50f24-207">Search for hello following library names one by one.</span></span> <span data-ttu-id="50f24-208">Pro každou knihovnu, která zjistíte, klikněte na tlačítko **nainstalovat**:</span><span class="sxs-lookup"><span data-stu-id="50f24-208">For each library that you find, click **Install**:</span></span>

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. <span data-ttu-id="50f24-209">Nainstalujte ručně `Adafruit_WINC1500`.</span><span class="sxs-lookup"><span data-stu-id="50f24-209">Manually install `Adafruit_WINC1500`.</span></span> <span data-ttu-id="50f24-210">Přejděte příliš[tento web](https://github.com/adafruit/Adafruit_WINC1500) a klikněte na tlačítko **klonovat nebo stáhnout** > **stáhnout ZIP**.</span><span class="sxs-lookup"><span data-stu-id="50f24-210">Go too[this website](https://github.com/adafruit/Adafruit_WINC1500) and click **Clone or download** > **Download ZIP**.</span></span> <span data-ttu-id="50f24-211">Potom v IDE vaší Arduino přejděte příliš**nákresu** > **zahrnují knihovny** > **přidat .zip knihovna** a přidejte soubor zip hello.</span><span class="sxs-lookup"><span data-stu-id="50f24-211">Then in your Arduino IDE, go too**Sketch** > **Include Library** > **Add .zip Library** and add hello zip file.</span></span>

### <a name="use-hello-sample-application-if-you-dont-have-a-real-bme280-sensor"></a><span data-ttu-id="50f24-212">Pokud nemáte skutečné senzor BME280 použít hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="50f24-212">Use hello sample application if you don’t have a real BME280 sensor</span></span>

<span data-ttu-id="50f24-213">Pokud nemáte skutečné senzor BME280, hello ukázkovou aplikaci můžete simulovat teploty a vlhkosti data.</span><span class="sxs-lookup"><span data-stu-id="50f24-213">If you don’t have a real BME280 sensor, hello sample application can simulate temperature and humidity data.</span></span> <span data-ttu-id="50f24-214">tooset hello ukázkové aplikace toouse simulated dat, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="50f24-214">tooset up hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="50f24-215">Otevřete hello `config.h` souboru v hello `app` složky.</span><span class="sxs-lookup"><span data-stu-id="50f24-215">Open hello `config.h` file in hello `app` folder.</span></span>

2. <span data-ttu-id="50f24-216">Vyhledejte následující řádek kódu hello a změňte hodnotu hello z `false` příliš`true`:</span><span class="sxs-lookup"><span data-stu-id="50f24-216">Locate hello following line of code and change hello value from `false` too`true`:</span></span>

   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurace hello ukázková aplikace toouse simulated data](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. <span data-ttu-id="50f24-218">Uložte soubor hello s `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="50f24-218">Save hello file with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toofeather-m0-wifi"></a><span data-ttu-id="50f24-219">Nasazení hello ukázkové aplikace tooFeather M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="50f24-219">Deploy hello sample application tooFeather M0 WiFi</span></span>

1. <span data-ttu-id="50f24-220">V hello Arduino IDE, klikněte na **nástroj** > **Port**a potom klikněte na hello sériového portu pro prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="50f24-220">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Feather M0 WiFi.</span></span>

2. <span data-ttu-id="50f24-221">Klikněte na tlačítko **nákresu** > **nahrát** toobuild a nasadit hello ukázkové aplikace tooFeather M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="50f24-221">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooFeather M0 WiFi.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="50f24-222">Zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="50f24-222">Enter your credentials</span></span>

<span data-ttu-id="50f24-223">Po úspěšném dokončení nahrávání hello postupujte podle těchto kroků tooenter vaše přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="50f24-223">After hello upload completes successfully, follow these steps tooenter your credentials:</span></span>

1. <span data-ttu-id="50f24-224">V hello Arduino IDE, klikněte na **nástroje** > **sériové monitorování**.</span><span class="sxs-lookup"><span data-stu-id="50f24-224">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>

2. <span data-ttu-id="50f24-225">V pravém dolním rohu, hello okna hello sériové monitorování, vyberte **žádný řádek ukončování** v rozevíracím seznamu hello na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="50f24-225">In hello lower-right corner of hello serial monitor window, select **No line ending** in hello drop-down list on hello left.</span></span>
3. <span data-ttu-id="50f24-226">Vyberte **115200 přenosová** v rozevíracím seznamu hello na hello správné.</span><span class="sxs-lookup"><span data-stu-id="50f24-226">Select **115200 baud** in hello drop-down list on hello right.</span></span>
4. <span data-ttu-id="50f24-227">Hello vstupní pole v horní části hello, zadejte následující informace, pokud jste hello výzva tooprovide a klikněte na **odeslat**:</span><span class="sxs-lookup"><span data-stu-id="50f24-227">In hello input box at hello top, enter hello following information if you're asked tooprovide it, and click **Send**:</span></span>

   * <span data-ttu-id="50f24-228">SSID sítě Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="50f24-228">Wi-Fi SSID</span></span>
   * <span data-ttu-id="50f24-229">Heslo Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="50f24-229">Wi-Fi password</span></span>
   * <span data-ttu-id="50f24-230">Řetězec připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="50f24-230">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="50f24-231">Hello pověření informace jsou uloženy v hello EEPROM z prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="50f24-231">hello credential information is stored in hello EEPROM of Feather M0 WiFi.</span></span> <span data-ttu-id="50f24-232">Pokud klepnete na tlačítko Obnovit hello na hello Tabule prolnutí M0 Wi-Fi, hello ukázkovou aplikaci dotáže se tooerase hello informace.</span><span class="sxs-lookup"><span data-stu-id="50f24-232">If you click hello reset button on hello Feather M0 WiFi board, hello sample application asks if you want tooerase hello information.</span></span> <span data-ttu-id="50f24-233">Zadejte `Y` tooerase hello informace.</span><span class="sxs-lookup"><span data-stu-id="50f24-233">Enter `Y` tooerase hello information.</span></span> <span data-ttu-id="50f24-234">Zobrazí se dotaz tooprovide hello informace ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="50f24-234">You're asked tooprovide hello information a second time.</span></span>

### <a name="verify-that-hello-sample-application-is-running-successfully"></a><span data-ttu-id="50f24-235">Ověřte, zda text hello ukázkové aplikace úspěšně spuštěna</span><span class="sxs-lookup"><span data-stu-id="50f24-235">Verify that hello sample application is running successfully</span></span>

<span data-ttu-id="50f24-236">Pokud se zobrazí následující hello výstup z okna sériové sledování hello a hello blikat DIODU na prolnutí M0 sítě Wi-Fi, hello ukázkové aplikace je spuštěna úspěšně:</span><span class="sxs-lookup"><span data-stu-id="50f24-236">If you see hello following output from hello serial monitor window and hello blinking LED on Feather M0 WiFi, hello sample application is running successfully:</span></span>

![Závěrečný výstup v integrovaném vývojovém prostředí Arduino](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="50f24-238">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50f24-238">Next steps</span></span>

<span data-ttu-id="50f24-239">Úspěšně jste připojení Wi-Fi M0 prolnutí tooyour IoT hub a odeslat hello zachytit senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="50f24-239">You have successfully connected Feather M0 WiFi tooyour IoT hub and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

