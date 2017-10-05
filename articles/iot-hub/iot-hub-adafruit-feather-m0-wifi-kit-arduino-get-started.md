---
title: "M0 do cloudu: prolnutí M0 Wi-Fi připojení ke službě Azure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak nastavit a Adafruit prolnutí M0 Wi-Fi připojení ke službě Azure IoT Hub k odesílání dat do Azure Cloudová platforma v tomto kurzu."
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
ms.openlocfilehash: 0dcf6b46a4c6c743c713d24ce7844e801b278dcf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="a015b-103">Připojení ke službě Azure IoT Hub v cloudu Adafruit prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-103">Connect Adafruit Feather M0 WiFi to Azure IoT Hub in the cloud</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Připojení mezi BME280, prolnutí M0 Wi-Fi a IoT Hub](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

<span data-ttu-id="a015b-105">V tomto kurzu zahájíte učení základní informace o práci s Arduino panel.</span><span class="sxs-lookup"><span data-stu-id="a015b-105">In this tutorial, you begin by learning the basics of working with your Arduino board.</span></span> <span data-ttu-id="a015b-106">Pak zjistíte, jak bezproblémově připojení zařízení do cloudu pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="a015b-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="a015b-107">Co dělat</span><span class="sxs-lookup"><span data-stu-id="a015b-107">What you do</span></span>

<span data-ttu-id="a015b-108">Připojte Adafruit prolnutí M0 Wi-Fi do služby IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="a015b-108">Connect Adafruit Feather M0 WiFi to an IoT hub that you create.</span></span> <span data-ttu-id="a015b-109">Pak spusťte ukázkovou aplikaci na M0 Wi-Fi ke shromažďování dat teploty a vlhkosti z BME280.</span><span class="sxs-lookup"><span data-stu-id="a015b-109">Then you run a sample application on M0 WiFi to collect the temperature and humidity data from a BME280.</span></span> <span data-ttu-id="a015b-110">Nakonec odeslat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a015b-110">Finally, you send the sensor data to your IoT hub.</span></span>


## <a name="what-you-learn"></a><span data-ttu-id="a015b-111">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="a015b-111">What you learn</span></span>

* <span data-ttu-id="a015b-112">Postup vytvoření služby IoT hub a registrovat zařízení pro prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-112">How to create an IoT hub and register a device for Feather M0 WiFi</span></span>
* <span data-ttu-id="a015b-113">Postup připojení Wi-Fi M0 prolnutí s senzoru a počítač</span><span class="sxs-lookup"><span data-stu-id="a015b-113">How to connect Feather M0 WiFi with the sensor and your computer</span></span>
* <span data-ttu-id="a015b-114">Postup shromažďování dat snímačů spuštěním ukázkovou aplikaci na prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-114">How to collect sensor data by running a sample application on Feather M0 WiFi</span></span>
* <span data-ttu-id="a015b-115">Postup odesílání dat snímačů do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="a015b-115">How to send the sensor data to your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a015b-116">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="a015b-116">What you need</span></span>

![součásti potřebné pro tento kurz](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="a015b-118">Pro dokončení této operace, musíte z vaší prolnutí M0 Wi-Fi Starter Kit následujících částí:</span><span class="sxs-lookup"><span data-stu-id="a015b-118">To complete this operation, you need the following parts from your Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="a015b-119">Panel prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-119">The Feather M0 WiFi board</span></span>
* <span data-ttu-id="a015b-120">Micro USB na kabelu USB typ A</span><span class="sxs-lookup"><span data-stu-id="a015b-120">A Micro USB to Type A USB cable</span></span>

<span data-ttu-id="a015b-121">Vývojové prostředí musíte taky následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="a015b-121">You also need the following things for your development environment:</span></span>

* <span data-ttu-id="a015b-122">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="a015b-122">An active Azure subscription.</span></span> <span data-ttu-id="a015b-123">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="a015b-123">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="a015b-124">Mac nebo počítači se systémem Windows nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="a015b-124">A Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="a015b-125">Bezdrátové sítě Wi-Fi M0 prolnutí pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="a015b-125">A wireless network for Feather M0 WiFi to connect to.</span></span>
* <span data-ttu-id="a015b-126">Připojení k Internetu kvůli stahování nástroj konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a015b-126">An Internet connection to download the configuration tool.</span></span>
* <span data-ttu-id="a015b-127">[Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a015b-127">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="a015b-128">Starší verze nepodporují s knihovnou Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a015b-128">Earlier versions don't work with the Azure IoT Hub library.</span></span>

<span data-ttu-id="a015b-129">Pokud nemáte senzoru, následující položky jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="a015b-129">If you don’t have a sensor, the following items are optional.</span></span> <span data-ttu-id="a015b-130">Máte také možnost používat data simulované snímače:</span><span class="sxs-lookup"><span data-stu-id="a015b-130">You also have the option of using simulated sensor data:</span></span>

* <span data-ttu-id="a015b-131">Senzor teploty a vlhkosti BME280</span><span class="sxs-lookup"><span data-stu-id="a015b-131">A BME280 temperature and humidity sensor</span></span>
* <span data-ttu-id="a015b-132">Breadboard</span><span class="sxs-lookup"><span data-stu-id="a015b-132">A breadboard</span></span>
* <span data-ttu-id="a015b-133">Vodičům můstek M/M</span><span class="sxs-lookup"><span data-stu-id="a015b-133">M/M jumper wires</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a><span data-ttu-id="a015b-134">Připojení Wi-Fi M0 prolnutí s senzoru a počítač</span><span class="sxs-lookup"><span data-stu-id="a015b-134">Connect Feather M0 WiFi with the sensor and your computer</span></span>
<span data-ttu-id="a015b-135">V této části se připojíte k vaší karty snímače.</span><span class="sxs-lookup"><span data-stu-id="a015b-135">In this section, you connect the sensors to your board.</span></span> <span data-ttu-id="a015b-136">Potom můžete připojení tohoto zařízení do počítače pro další použití.</span><span class="sxs-lookup"><span data-stu-id="a015b-136">Then you plug in your device to your computer for further use.</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a><span data-ttu-id="a015b-137">Senzor teploty a vlhkosti DHT22 připojit k prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-137">Connect a DHT22 temperature and humidity sensor to Feather M0 WiFi</span></span>

<span data-ttu-id="a015b-138">Pomocí vedení breadboard a můstek pro připojení.</span><span class="sxs-lookup"><span data-stu-id="a015b-138">Use the breadboard and jumper wires to make the connection.</span></span> <span data-ttu-id="a015b-139">Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.</span><span class="sxs-lookup"><span data-stu-id="a015b-139">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![odkaz na připojení](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


<span data-ttu-id="a015b-141">Senzor kód PIN použijte následující kabeláž:</span><span class="sxs-lookup"><span data-stu-id="a015b-141">For sensor pins, use the following wiring:</span></span>


| <span data-ttu-id="a015b-142">Spuštění (senzor)</span><span class="sxs-lookup"><span data-stu-id="a015b-142">Start (sensor)</span></span>           | <span data-ttu-id="a015b-143">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="a015b-143">End (board)</span></span>            | <span data-ttu-id="a015b-144">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="a015b-144">Cable color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="a015b-145">VDD (Pin 27A)</span><span class="sxs-lookup"><span data-stu-id="a015b-145">VDD (Pin 27A)</span></span>            | <span data-ttu-id="a015b-146">3v (3A PIN kódu)</span><span class="sxs-lookup"><span data-stu-id="a015b-146">3V (Pin 3A)</span></span>            | <span data-ttu-id="a015b-147">Red kabel</span><span class="sxs-lookup"><span data-stu-id="a015b-147">Red cable</span></span>     |
| <span data-ttu-id="a015b-148">ZEM (Pin 29A)</span><span class="sxs-lookup"><span data-stu-id="a015b-148">GND (Pin 29A)</span></span>            | <span data-ttu-id="a015b-149">ZEM (6A PIN kódu)</span><span class="sxs-lookup"><span data-stu-id="a015b-149">GND (Pin 6A)</span></span>           | <span data-ttu-id="a015b-150">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="a015b-150">Black cable</span></span>   |
| <span data-ttu-id="a015b-151">SCK (Pin 30A)</span><span class="sxs-lookup"><span data-stu-id="a015b-151">SCK (Pin 30A)</span></span>            | <span data-ttu-id="a015b-152">SCK (Pin 12A)</span><span class="sxs-lookup"><span data-stu-id="a015b-152">SCK (Pin 12A)</span></span>          | <span data-ttu-id="a015b-153">Žlutý kabel</span><span class="sxs-lookup"><span data-stu-id="a015b-153">Yellow cable</span></span>  |
| <span data-ttu-id="a015b-154">SDO (Pin 31A)</span><span class="sxs-lookup"><span data-stu-id="a015b-154">SDO (Pin 31A)</span></span>            | <span data-ttu-id="a015b-155">MI (Pin 14A)</span><span class="sxs-lookup"><span data-stu-id="a015b-155">MI (Pin 14A)</span></span>           | <span data-ttu-id="a015b-156">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="a015b-156">White cable</span></span>   |
| <span data-ttu-id="a015b-157">SDI (Pin 32A)</span><span class="sxs-lookup"><span data-stu-id="a015b-157">SDI (Pin 32A)</span></span>            | <span data-ttu-id="a015b-158">M0 (Pin 13A)</span><span class="sxs-lookup"><span data-stu-id="a015b-158">M0 (Pin 13A)</span></span>           | <span data-ttu-id="a015b-159">Modrý kabel</span><span class="sxs-lookup"><span data-stu-id="a015b-159">Blue cable</span></span>    |
| <span data-ttu-id="a015b-160">CS (Pin 33A)</span><span class="sxs-lookup"><span data-stu-id="a015b-160">CS (Pin 33A)</span></span>             | <span data-ttu-id="a015b-161">GPIO 5 (Pin 15J)</span><span class="sxs-lookup"><span data-stu-id="a015b-161">GPIO 5 (Pin 15J)</span></span>       | <span data-ttu-id="a015b-162">Oranžové kabel</span><span class="sxs-lookup"><span data-stu-id="a015b-162">Orange cable</span></span>  |

<span data-ttu-id="a015b-163">Další informace najdete v tématu [Adafruit BME280 vlhkosti + barometrický tlak + teplotní snímač volnými](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) a [uspořádání kolíků Adafruit prolnutí M0 Wi-Fi](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span><span class="sxs-lookup"><span data-stu-id="a015b-163">For more information, see [Adafruit BME280 Humidity + Barometric Pressure + Temperature Sensor Breakout](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) and [Adafruit Feather M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).</span></span>



<span data-ttu-id="a015b-164">Nyní by měly být připojené vaší prolnutí M0 Wi-Fi s pracovní senzoru.</span><span class="sxs-lookup"><span data-stu-id="a015b-164">Now your Feather M0 WiFi should be connected with a working sensor.</span></span>

![Spojte se s prolnutí Huzzah DHT22](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a><span data-ttu-id="a015b-166">Připojte k počítači prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-166">Connect Feather M0 WiFi to your computer</span></span>

<span data-ttu-id="a015b-167">Pro připojení Wi-Fi M0 prolnutí do počítače, použijte Micro USB na kabelu USB typ A jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="a015b-167">Use the Micro USB to Type A USB cable to connect Feather M0 WiFi to your computer, as shown:</span></span>

![Prolnutí Huzzah připojte k počítači](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="a015b-169">Přidání oprávnění sériového portu (pouze Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="a015b-169">Add serial port permissions (Ubuntu only)</span></span>

<span data-ttu-id="a015b-170">Pokud používáte Ubuntu, zkontrolujte, zda že máte oprávnění k provozu na USB port z prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="a015b-170">If you use Ubuntu, make sure you have the permissions to operate on the USB port of Feather M0 WiFi.</span></span> <span data-ttu-id="a015b-171">Pokud chcete přidat oprávnění sériového portu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a015b-171">To add serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="a015b-172">V terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="a015b-172">At a terminal, run the following commands:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="a015b-173">Zobrazí jednu z následujících výstupy:</span><span class="sxs-lookup"><span data-stu-id="a015b-173">You get one of the following outputs:</span></span>

   * <span data-ttu-id="a015b-174">CRW-rw---1 kořenové uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="a015b-174">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="a015b-175">CRW-rw---xxxxxxxx síti službou 1 root</span><span class="sxs-lookup"><span data-stu-id="a015b-175">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="a015b-176">Ve výstupu, Všimněte si, že `uucp` nebo `dialout` je název skupiny vlastníka portu USB.</span><span class="sxs-lookup"><span data-stu-id="a015b-176">In the output, notice that `uucp` or `dialout` is the group owner name of the USB port.</span></span>

2. <span data-ttu-id="a015b-177">Přidejte uživatele do skupiny, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a015b-177">To add the user to the group, run the following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="a015b-178">V předchozím kroku jste získali název vlastníka skupiny `<group-owner-name>`.</span><span class="sxs-lookup"><span data-stu-id="a015b-178">In the previous step, you obtained the group owner name `<group-owner-name>`.</span></span> <span data-ttu-id="a015b-179">Uživatelské jméno Ubuntu `<username>`.</span><span class="sxs-lookup"><span data-stu-id="a015b-179">Your Ubuntu user name is `<username>`.</span></span>

3. <span data-ttu-id="a015b-180">Změna zobrazí, odhlaste se od Ubuntu a potom se znovu přihlaste.</span><span class="sxs-lookup"><span data-stu-id="a015b-180">For the change to appear, sign out of Ubuntu and then sign in again.</span></span>

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a><span data-ttu-id="a015b-181">Shromažďování dat snímačů a odeslat do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="a015b-181">Collect sensor data and send it to your IoT hub</span></span>

<span data-ttu-id="a015b-182">V této části nasazení a spuštění ukázkové aplikace na prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="a015b-182">In this section, you deploy and run a sample application on Feather M0 WiFi.</span></span> <span data-ttu-id="a015b-183">Ukázkové aplikace vytváří blikání DIODU na prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="a015b-183">The sample application makes the LED blink on Feather M0 WiFi.</span></span> <span data-ttu-id="a015b-184">Pak odešle teploty a vlhkosti data shromážděná z senzoru BME280 do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a015b-184">It then sends the temperature and humidity data collected from the BME280 sensor to your IoT hub.</span></span>

### <a name="get-the-sample-application-from-github-and-prepare-the-arduino-ide"></a><span data-ttu-id="a015b-185">Získat ukázkovou aplikaci z webu GitHub a příprava Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="a015b-185">Get the sample application from GitHub and prepare the Arduino IDE</span></span>

<span data-ttu-id="a015b-186">Ukázkové aplikace jsou hostované na Githubu.</span><span class="sxs-lookup"><span data-stu-id="a015b-186">The sample application is hosted on GitHub.</span></span> <span data-ttu-id="a015b-187">Naklonujte úložiště ukázka, která obsahuje ukázkovou aplikaci z webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="a015b-187">Clone the sample repository that contains the sample application from GitHub.</span></span> <span data-ttu-id="a015b-188">Klonovat úložiště ukázkové, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a015b-188">To clone the sample repository, follow these steps:</span></span>

1. <span data-ttu-id="a015b-189">Otevřete příkazový řádek nebo okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="a015b-189">Open a command prompt or a terminal window.</span></span>

2. <span data-ttu-id="a015b-190">Přejděte do složky, kam chcete ukázkovou aplikaci k uložení.</span><span class="sxs-lookup"><span data-stu-id="a015b-190">Go to a folder where you want the sample application to be stored.</span></span>
3. <span data-ttu-id="a015b-191">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a015b-191">Run the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-the-package-for-feather-m0-wifi-in-the-arduino-ide"></a><span data-ttu-id="a015b-192">Instalace balíčku pro prolnutí M0 Wi-Fi v Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="a015b-192">Install the package for Feather M0 WiFi in the Arduino IDE</span></span>

1. <span data-ttu-id="a015b-193">Otevřete složku, kde je uložen ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a015b-193">Open the folder where the sample application is stored.</span></span>

2. <span data-ttu-id="a015b-194">Otevřete soubor app.ino ve složce aplikace v prostředí IDE Arduino.</span><span class="sxs-lookup"><span data-stu-id="a015b-194">Open the app.ino file in the app folder in the Arduino IDE.</span></span>

   ![Otevřete ukázkovou aplikaci v Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. <span data-ttu-id="a015b-196">Klikněte na tlačítko **soubor** > **Předvolby** (Windows nebo Linuxem) nebo **Arduino** > **Předvolby** (Mac) a zkopírujte a vložte odkaz do níže **další adresy URL Manager panely** možnost v předvolbách Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="a015b-196">Click **File** > **Preferences** (Windows/Linux) or **Arduino** > **Preferences** (Mac) and copy and paste the link below into the **Additional Boards Manager URLs** option in the Arduino IDE preferences.</span></span>
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. <span data-ttu-id="a015b-197">Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a pak nainstalujte `Arduino SAMD Boards` verze `1.6.2` nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a015b-197">Click **Tools** > **Board** > **Boards Manager**, and then install the `Arduino SAMD Boards` version `1.6.2` or later.</span></span> 

1. <span data-ttu-id="a015b-198">V rámci stejného časového období, nainstalujte `Adafruit SAMD Boards` balíček, který chcete přidat soubor definice panelu.</span><span class="sxs-lookup"><span data-stu-id="a015b-198">Then in the same window, install `Adafruit SAMD Boards` package to add the board file definitions.</span></span>

   ![instalaci balíčku esp8266](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. <span data-ttu-id="a015b-200">Klikněte na tlačítko **nástroje** > **Tabule** > **Adafruit M0 Wi-Fi**.</span><span class="sxs-lookup"><span data-stu-id="a015b-200">Click **Tools** > **Board** > **Adafruit M0 WiFi**.</span></span>

5. <span data-ttu-id="a015b-201">Instalace ovladačů (jenom Windows).</span><span class="sxs-lookup"><span data-stu-id="a015b-201">Install drivers (for Windows only).</span></span> <span data-ttu-id="a015b-202">Po připojení Wi-Fi M0 prolnutí, možná muset nainstalovat ovladač.</span><span class="sxs-lookup"><span data-stu-id="a015b-202">When you plug in Feather M0 WiFi, you might need to install a driver.</span></span> <span data-ttu-id="a015b-203">Klikněte na tlačítko [odkaz ke stažení na webové stránce](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) pro stažení instalačního programu ovladače.</span><span class="sxs-lookup"><span data-stu-id="a015b-203">Click [the download link on the webpage](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) to download the driver installer.</span></span> <span data-ttu-id="a015b-204">Postupujte podle kroků k instalaci ovladačů, které chcete.</span><span class="sxs-lookup"><span data-stu-id="a015b-204">Follow the steps to install the drivers you want.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="a015b-205">Instalace nezbytné knihovny</span><span class="sxs-lookup"><span data-stu-id="a015b-205">Install necessary libraries</span></span>

1. <span data-ttu-id="a015b-206">V prostředí IDE Arduino, klikněte na tlačítko **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.</span><span class="sxs-lookup"><span data-stu-id="a015b-206">In the Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>

2. <span data-ttu-id="a015b-207">Vyhledejte následující knihovny názvy po jednom.</span><span class="sxs-lookup"><span data-stu-id="a015b-207">Search for the following library names one by one.</span></span> <span data-ttu-id="a015b-208">Pro každou knihovnu, která zjistíte, klikněte na tlačítko **nainstalovat**:</span><span class="sxs-lookup"><span data-stu-id="a015b-208">For each library that you find, click **Install**:</span></span>

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. <span data-ttu-id="a015b-209">Nainstalujte ručně `Adafruit_WINC1500`.</span><span class="sxs-lookup"><span data-stu-id="a015b-209">Manually install `Adafruit_WINC1500`.</span></span> <span data-ttu-id="a015b-210">Přejděte na [tento web](https://github.com/adafruit/Adafruit_WINC1500) a klikněte na tlačítko **klonovat nebo stáhnout** > **stáhnout ZIP**.</span><span class="sxs-lookup"><span data-stu-id="a015b-210">Go to [this website](https://github.com/adafruit/Adafruit_WINC1500) and click **Clone or download** > **Download ZIP**.</span></span> <span data-ttu-id="a015b-211">Potom vaší Arduino IDE, přejděte na **nákresu** > **zahrnují knihovny** > **přidat .zip knihovna** a přidejte soubor zip.</span><span class="sxs-lookup"><span data-stu-id="a015b-211">Then in your Arduino IDE, go to **Sketch** > **Include Library** > **Add .zip Library** and add the zip file.</span></span>

### <a name="use-the-sample-application-if-you-dont-have-a-real-bme280-sensor"></a><span data-ttu-id="a015b-212">Pokud nemáte skutečné senzor BME280 použít ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="a015b-212">Use the sample application if you don’t have a real BME280 sensor</span></span>

<span data-ttu-id="a015b-213">Pokud nemáte skutečné senzor BME280, ukázkové aplikace můžete simulovat teploty a vlhkosti data.</span><span class="sxs-lookup"><span data-stu-id="a015b-213">If you don’t have a real BME280 sensor, the sample application can simulate temperature and humidity data.</span></span> <span data-ttu-id="a015b-214">Pokud chcete nastavit ukázkovou aplikaci používat simulované dat, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a015b-214">To set up the sample application to use simulated data, follow these steps:</span></span>

1. <span data-ttu-id="a015b-215">Otevřete `config.h` v soubor `app` složky.</span><span class="sxs-lookup"><span data-stu-id="a015b-215">Open the `config.h` file in the `app` folder.</span></span>

2. <span data-ttu-id="a015b-216">Vyhledejte následující řádek kódu a změňte hodnotu z `false` k `true`:</span><span class="sxs-lookup"><span data-stu-id="a015b-216">Locate the following line of code and change the value from `false` to `true`:</span></span>

   ```c
   define SIMULATED_DATA true
   ```
   ![Nakonfigurujte ukázkovou aplikaci používat simulované dat](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. <span data-ttu-id="a015b-218">Uložte soubor s `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="a015b-218">Save the file with `Control-s`.</span></span>

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a><span data-ttu-id="a015b-219">Nasadit ukázkovou aplikaci pro prolnutí M0 Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-219">Deploy the sample application to Feather M0 WiFi</span></span>

1. <span data-ttu-id="a015b-220">V prostředí IDE Arduino, klikněte na tlačítko **nástroj** > **Port**a potom klikněte na sériového portu pro prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="a015b-220">In the Arduino IDE, click **Tool** > **Port**, and then click the serial port for Feather M0 WiFi.</span></span>

2. <span data-ttu-id="a015b-221">Klikněte na tlačítko **nákresu** > **nahrát** k vytváření a nasazování ukázkovou aplikaci pro prolnutí M0 Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="a015b-221">Click **Sketch** > **Upload** to build and deploy the sample application to Feather M0 WiFi.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="a015b-222">Zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a015b-222">Enter your credentials</span></span>

<span data-ttu-id="a015b-223">Po úspěšném dokončení nahrávání, zadejte své přihlašovací údaje takto:</span><span class="sxs-lookup"><span data-stu-id="a015b-223">After the upload completes successfully, follow these steps to enter your credentials:</span></span>

1. <span data-ttu-id="a015b-224">V prostředí IDE Arduino, klikněte na tlačítko **nástroje** > **sériové monitorování**.</span><span class="sxs-lookup"><span data-stu-id="a015b-224">In the Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>

2. <span data-ttu-id="a015b-225">V pravém dolním rohu okna sériové monitorování, vyberte **žádný řádek ukončování** v rozevíracím seznamu na levé straně.</span><span class="sxs-lookup"><span data-stu-id="a015b-225">In the lower-right corner of the serial monitor window, select **No line ending** in the drop-down list on the left.</span></span>
3. <span data-ttu-id="a015b-226">Vyberte **115200 přenosová** v rozevíracím seznamu na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="a015b-226">Select **115200 baud** in the drop-down list on the right.</span></span>
4. <span data-ttu-id="a015b-227">Do vstupního pole v horní části, zadejte následující informace, pokud budete vyzváni k tomu a klikněte na tlačítko **odeslat**:</span><span class="sxs-lookup"><span data-stu-id="a015b-227">In the input box at the top, enter the following information if you're asked to provide it, and click **Send**:</span></span>

   * <span data-ttu-id="a015b-228">SSID sítě Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-228">Wi-Fi SSID</span></span>
   * <span data-ttu-id="a015b-229">Heslo Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="a015b-229">Wi-Fi password</span></span>
   * <span data-ttu-id="a015b-230">Řetězec připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="a015b-230">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="a015b-231">Informace o přihlašovacích údajích uložený v EEPROM z prolnutí M0 WiFi.</span><span class="sxs-lookup"><span data-stu-id="a015b-231">The credential information is stored in the EEPROM of Feather M0 WiFi.</span></span> <span data-ttu-id="a015b-232">Pokud kliknete na tlačítko Obnovit na panelu prolnutí M0 Wi-Fi, ukázková aplikace zobrazí, pokud chcete vymazat informace.</span><span class="sxs-lookup"><span data-stu-id="a015b-232">If you click the reset button on the Feather M0 WiFi board, the sample application asks if you want to erase the information.</span></span> <span data-ttu-id="a015b-233">Zadejte `Y` vymazat informace.</span><span class="sxs-lookup"><span data-stu-id="a015b-233">Enter `Y` to erase the information.</span></span> <span data-ttu-id="a015b-234">Budete vyzváni k zadání informací ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="a015b-234">You're asked to provide the information a second time.</span></span>

### <a name="verify-that-the-sample-application-is-running-successfully"></a><span data-ttu-id="a015b-235">Ověřte, zda je ukázková aplikace úspěšně spuštěna</span><span class="sxs-lookup"><span data-stu-id="a015b-235">Verify that the sample application is running successfully</span></span>

<span data-ttu-id="a015b-236">Pokud se zobrazí následující výstup z okna sériové monitorování a blikající LED na prolnutí M0 Wi-Fi, je ukázka aplikace spuštěna úspěšně:</span><span class="sxs-lookup"><span data-stu-id="a015b-236">If you see the following output from the serial monitor window and the blinking LED on Feather M0 WiFi, the sample application is running successfully:</span></span>

![Závěrečný výstup v integrovaném vývojovém prostředí Arduino](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="a015b-238">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a015b-238">Next steps</span></span>

<span data-ttu-id="a015b-239">Úspěšně jste připojení Wi-Fi M0 prolnutí do služby IoT hub a data zaznamenaná senzor odeslané do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a015b-239">You have successfully connected Feather M0 WiFi to your IoT hub and sent the captured sensor data to your IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

