---
title: "ESP8266 do cloudu - připojit ke službě Azure IoT Hub prolnutí HUZZAH ESP8266 | Microsoft Docs"
description: "Zjistěte, jak nastavit a Adafruit prolnutí HUZZAH ESP8266 připojit ke službě Azure IoT Hub pro něj k odesílání dat do Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.openlocfilehash: 6a450579c848fe6030a328ddf410f139baae2324
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="0209c-103">Adafruit prolnutí HUZZAH ESP8266 připojit ke službě Azure IoT Hub v cloudu</span><span class="sxs-lookup"><span data-stu-id="0209c-103">Connect Adafruit Feather HUZZAH ESP8266 to Azure IoT Hub in the cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Připojení mezi DHT22, prolnutí HUZZAH ESP8266 a IoT Hub](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a><span data-ttu-id="0209c-105">Co dělat</span><span class="sxs-lookup"><span data-stu-id="0209c-105">What you do</span></span>


<span data-ttu-id="0209c-106">Připojte Adafruit prolnutí HUZZAH ESP8266 do služby IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="0209c-106">Connect Adafruit Feather HUZZAH ESP8266 to an IoT hub that you create.</span></span> <span data-ttu-id="0209c-107">Pak spusťte ukázkovou aplikaci na ESP8266 ke shromažďování dat teploty a vlhkosti ze DHT22 senzoru.</span><span class="sxs-lookup"><span data-stu-id="0209c-107">Then you run a sample application on ESP8266 to collect the temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="0209c-108">Nakonec odeslat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0209c-108">Finally, you send the sensor data to your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="0209c-109">Pokud používáte jiné ESP8266 panely, můžete stále následujícím postupem pro připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0209c-109">If you're using other ESP8266 boards, you can still follow these steps to connect it to your IoT hub.</span></span> <span data-ttu-id="0209c-110">V závislosti na ESP8266 panelu, který používáte, možná budete muset překonfigurovat `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="0209c-110">Depending on the ESP8266 board you're using, you might need to reconfigure the `LED_PIN`.</span></span> <span data-ttu-id="0209c-111">Například pokud používáte ESP8266 z AI Thinker, můžete změnit z `0` k `2`.</span><span class="sxs-lookup"><span data-stu-id="0209c-111">For example, if you're using ESP8266 from AI-Thinker, you might change it from `0` to `2`.</span></span> <span data-ttu-id="0209c-112">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="0209c-112">Don't have a kit yet?</span></span> <span data-ttu-id="0209c-113">Získat z [webu Azure](http://azure.com/iotstarterkits).</span><span class="sxs-lookup"><span data-stu-id="0209c-113">Get it from the [Azure website](http://azure.com/iotstarterkits).</span></span>




## <a name="what-you-learn"></a><span data-ttu-id="0209c-114">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="0209c-114">What you learn</span></span>

* <span data-ttu-id="0209c-115">Postup vytvoření služby IoT hub a registrovat zařízení pro prolnutí HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="0209c-115">How to create an IoT hub and register a device for Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="0209c-116">Postup připojení prolnutí HUZZAH ESP8266 s senzoru a počítač</span><span class="sxs-lookup"><span data-stu-id="0209c-116">How to connect Feather HUZZAH ESP8266 with the sensor and your computer</span></span>
* <span data-ttu-id="0209c-117">Postup shromažďování dat snímačů spuštěním ukázkovou aplikaci na prolnutí HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="0209c-117">How to collect sensor data by running a sample application on Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="0209c-118">Postup odesílání dat snímačů do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="0209c-118">How to send the sensor data to your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0209c-119">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="0209c-119">What you need</span></span>

![součásti potřebné pro tento kurz](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="0209c-121">Pro dokončení této operace, musíte z vaší prolnutí HUZZAH ESP8266 Starter Kit následujících částí:</span><span class="sxs-lookup"><span data-stu-id="0209c-121">To complete this operation, you need the following parts from your Feather HUZZAH ESP8266 Starter Kit:</span></span>

* <span data-ttu-id="0209c-122">Prolnutí HUZZAH ESP8266 panelu</span><span class="sxs-lookup"><span data-stu-id="0209c-122">The Feather HUZZAH ESP8266 board</span></span>
* <span data-ttu-id="0209c-123">Micro USB na kabelu USB typ A</span><span class="sxs-lookup"><span data-stu-id="0209c-123">A Micro USB to Type A USB cable</span></span>

<span data-ttu-id="0209c-124">Vývojové prostředí musíte taky následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="0209c-124">You also need the following things for your development environment:</span></span>

* <span data-ttu-id="0209c-125">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="0209c-125">An active Azure subscription.</span></span> <span data-ttu-id="0209c-126">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="0209c-126">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="0209c-127">Mac nebo počítači se systémem Windows nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="0209c-127">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="0209c-128">Bezdrátové sítě pro prolnutí HUZZAH ESP8266 pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="0209c-128">Wireless network for Feather HUZZAH ESP8266 to connect to.</span></span>
* <span data-ttu-id="0209c-129">Připojení k Internetu stahovat nástroj konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0209c-129">Internet connection to download the configuration tool.</span></span>
* <span data-ttu-id="0209c-130">[Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0209c-130">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="0209c-131">Starší verze nepodporují s knihovnou AzureIoT.</span><span class="sxs-lookup"><span data-stu-id="0209c-131">Earlier versions don't work with the AzureIoT library.</span></span>

<span data-ttu-id="0209c-132">Následující položky jsou volitelné, v případě, že nemáte senzoru.</span><span class="sxs-lookup"><span data-stu-id="0209c-132">The following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="0209c-133">Máte také možnost používat data snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="0209c-133">You also have the option of using simulated sensor data.</span></span>

* <span data-ttu-id="0209c-134">Senzor teploty a vlhkosti Adafruit DHT22</span><span class="sxs-lookup"><span data-stu-id="0209c-134">An Adafruit DHT22 temperature and humidity sensor</span></span>
* <span data-ttu-id="0209c-135">Breadboard</span><span class="sxs-lookup"><span data-stu-id="0209c-135">A breadboard</span></span>
* <span data-ttu-id="0209c-136">Vodičům můstek M/M</span><span class="sxs-lookup"><span data-stu-id="0209c-136">M/M jumper wires</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a><span data-ttu-id="0209c-137">Připojit prolnutí HUZZAH ESP8266 s senzoru a počítače</span><span class="sxs-lookup"><span data-stu-id="0209c-137">Connect Feather HUZZAH ESP8266 with the sensor and your computer</span></span>
<span data-ttu-id="0209c-138">V této části se připojíte k vaší karty snímače.</span><span class="sxs-lookup"><span data-stu-id="0209c-138">In this section, you connect the sensors to your board.</span></span> <span data-ttu-id="0209c-139">Potom můžete připojení tohoto zařízení do počítače pro další použití.</span><span class="sxs-lookup"><span data-stu-id="0209c-139">Then you plug in your device to your computer for further use.</span></span>
### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a><span data-ttu-id="0209c-140">Senzor teploty a vlhkosti DHT22 připojit k prolnutí HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="0209c-140">Connect a DHT22 temperature and humidity sensor to Feather HUZZAH ESP8266</span></span>

<span data-ttu-id="0209c-141">Pomocí vedení breadboard a můstek k připojení následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0209c-141">Use the breadboard and jumper wires to make the connection as follows.</span></span> <span data-ttu-id="0209c-142">Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.</span><span class="sxs-lookup"><span data-stu-id="0209c-142">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![odkaz na připojení](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


<span data-ttu-id="0209c-144">Senzor kód PIN použijte následující kabeláž:</span><span class="sxs-lookup"><span data-stu-id="0209c-144">For sensor pins, use the following wiring:</span></span>


| <span data-ttu-id="0209c-145">Spuštění (senzor)</span><span class="sxs-lookup"><span data-stu-id="0209c-145">Start (Sensor)</span></span>           | <span data-ttu-id="0209c-146">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="0209c-146">End (Board)</span></span>           | <span data-ttu-id="0209c-147">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="0209c-147">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="0209c-148">VDD (Pin 31F)</span><span class="sxs-lookup"><span data-stu-id="0209c-148">VDD (Pin 31F)</span></span>            | <span data-ttu-id="0209c-149">3v (připnout 58H)</span><span class="sxs-lookup"><span data-stu-id="0209c-149">3V (Pin 58H)</span></span>           | <span data-ttu-id="0209c-150">Red kabel</span><span class="sxs-lookup"><span data-stu-id="0209c-150">Red cable</span></span>     |
| <span data-ttu-id="0209c-151">DATA (Pin 32F)</span><span class="sxs-lookup"><span data-stu-id="0209c-151">DATA (Pin 32F)</span></span>           | <span data-ttu-id="0209c-152">GPIO 2 (Pin 46A)</span><span class="sxs-lookup"><span data-stu-id="0209c-152">GPIO 2 (Pin 46A)</span></span>       | <span data-ttu-id="0209c-153">Modrý kabel</span><span class="sxs-lookup"><span data-stu-id="0209c-153">Blue cable</span></span>    |
| <span data-ttu-id="0209c-154">ZEM (Pin 34F)</span><span class="sxs-lookup"><span data-stu-id="0209c-154">GND (Pin 34F)</span></span>            | <span data-ttu-id="0209c-155">ZEM (PIn 56I)</span><span class="sxs-lookup"><span data-stu-id="0209c-155">GND (PIn 56I)</span></span>          | <span data-ttu-id="0209c-156">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="0209c-156">Black cable</span></span>   |

<span data-ttu-id="0209c-157">Další informace najdete v tématu [Adafruit DHT22 senzor instalace](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) a [uspořádání Adafruit prolnutí HUZZAH Esp8266 kolíků](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span><span class="sxs-lookup"><span data-stu-id="0209c-157">For more information, see [Adafruit DHT22 sensor setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) and [Adafruit Feather HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span></span>



<span data-ttu-id="0209c-158">Teď vaše ESP8266 Huzzah prolnutí by měly být připojené s pracovní senzoru.</span><span class="sxs-lookup"><span data-stu-id="0209c-158">Now your Feather Huzzah ESP8266 should be connected with a working sensor.</span></span>

![Spojte se s prolnutí Huzzah DHT22](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a><span data-ttu-id="0209c-160">Prolnutí HUZZAH ESP8266 připojte k počítači</span><span class="sxs-lookup"><span data-stu-id="0209c-160">Connect Feather HUZZAH ESP8266 to your computer</span></span>

<span data-ttu-id="0209c-161">Jak ukazuje následující, prolnutí HUZZAH ESP8266 připojte k počítači pomocí USB Micro kabelem USB typ A.</span><span class="sxs-lookup"><span data-stu-id="0209c-161">As shown next, use the Micro USB to Type A USB cable to connect Feather HUZZAH ESP8266 to your computer.</span></span>

![Prolnutí Huzzah připojte k počítači](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="0209c-163">Přidání oprávnění sériového portu (pouze Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="0209c-163">Add serial port permissions (Ubuntu only)</span></span>


<span data-ttu-id="0209c-164">Pokud používáte Ubuntu, zkontrolujte, zda že máte oprávnění k provozu na USB port z prolnutí HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="0209c-164">If you use Ubuntu, make sure you have the permissions to operate on the USB port of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="0209c-165">Pokud chcete přidat oprávnění sériového portu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0209c-165">To add serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="0209c-166">V terminálu, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0209c-166">Run the following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="0209c-167">Zobrazí jednu z následujících výstupy:</span><span class="sxs-lookup"><span data-stu-id="0209c-167">You get one of the following outputs:</span></span>

   * <span data-ttu-id="0209c-168">CRW-rw---1 kořenové uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="0209c-168">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="0209c-169">CRW-rw---xxxxxxxx síti službou 1 root</span><span class="sxs-lookup"><span data-stu-id="0209c-169">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="0209c-170">Ve výstupu, Všimněte si, že `uucp` nebo `dialout` je název skupiny vlastníka portu USB.</span><span class="sxs-lookup"><span data-stu-id="0209c-170">In the output, notice that `uucp` or `dialout` is the group owner name of the USB port.</span></span>

1. <span data-ttu-id="0209c-171">Přidejte uživatele do skupiny tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0209c-171">Add the user to the group by running the following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="0209c-172">`<group-owner-name>`je název vlastníka skupiny, kterou jste získali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="0209c-172">`<group-owner-name>` is the group owner name you obtained in the previous step.</span></span> <span data-ttu-id="0209c-173">`<username>`je Ubuntu uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="0209c-173">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="0209c-174">Odhlaste se od Ubuntu a znovu se přihlaste změna zobrazí.</span><span class="sxs-lookup"><span data-stu-id="0209c-174">Sign out of Ubuntu, and then sign in again for the change to appear.</span></span>

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a><span data-ttu-id="0209c-175">Shromažďování dat snímačů a odeslat do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="0209c-175">Collect sensor data and send it to your IoT hub</span></span>

<span data-ttu-id="0209c-176">V této části nasazení a spuštění ukázkové aplikace na prolnutí HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="0209c-176">In this section, you deploy and run a sample application on Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="0209c-177">Ukázkovou aplikaci bliká DIODU na prolnutí HUZZAH ESP8266 a odešle teploty a vlhkosti data shromážděná z senzoru DHT22 do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0209c-177">The sample application blinks the LED on Feather HUZZAH ESP8266, and sends the temperature and humidity data collected from the DHT22 sensor to your IoT hub.</span></span>

### <a name="get-the-sample-application-from-github"></a><span data-ttu-id="0209c-178">Získat ukázkovou aplikaci z webu GitHub</span><span class="sxs-lookup"><span data-stu-id="0209c-178">Get the sample application from GitHub</span></span>

<span data-ttu-id="0209c-179">Ukázkové aplikace jsou hostované na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0209c-179">The sample application is hosted on GitHub.</span></span> <span data-ttu-id="0209c-180">Naklonujte úložiště ukázka, která obsahuje ukázkovou aplikaci z webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="0209c-180">Clone the sample repository that contains the sample application from GitHub.</span></span> <span data-ttu-id="0209c-181">Klonovat úložiště ukázkové, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0209c-181">To clone the sample repository, follow these steps:</span></span>

1. <span data-ttu-id="0209c-182">Otevřete příkazový řádek nebo okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="0209c-182">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="0209c-183">Přejděte do složky, kam chcete ukázkovou aplikaci k uložení.</span><span class="sxs-lookup"><span data-stu-id="0209c-183">Go to a folder where you want the sample application to be stored.</span></span>
1. <span data-ttu-id="0209c-184">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0209c-184">Run the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

<span data-ttu-id="0209c-185">Instalace balíčku pro prolnutí HUZZAH ESP8266 v prostředí IDE Arduino:</span><span class="sxs-lookup"><span data-stu-id="0209c-185">Install the package for Feather HUZZAH ESP8266 in the Arduino IDE:</span></span>

1. <span data-ttu-id="0209c-186">Otevřete složku, kde je uložen ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0209c-186">Open the folder where the sample application is stored.</span></span>
1. <span data-ttu-id="0209c-187">Otevřete soubor app.ino ve složce aplikace v prostředí IDE Arduino.</span><span class="sxs-lookup"><span data-stu-id="0209c-187">Open the app.ino file in the app folder in the Arduino IDE.</span></span>

   ![Otevřete ukázkovou aplikaci v Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="0209c-189">V prostředí IDE Arduino, klikněte na tlačítko **soubor** > **Předvolby**.</span><span class="sxs-lookup"><span data-stu-id="0209c-189">In the Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="0209c-190">V **Předvolby** dialogové okno pole, klikněte na ikonu vedle **další adresy URL Manager panely** pole.</span><span class="sxs-lookup"><span data-stu-id="0209c-190">In the **Preferences** dialog box, click the icon next to the **Additional Boards Manager URLs** box.</span></span>
1. <span data-ttu-id="0209c-191">V místním okně, zadejte následující adresu URL a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0209c-191">In the pop-up window, enter the following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Přejděte na adresu url balíčku v Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="0209c-193">V **předvoleb** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0209c-193">In the **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="0209c-194">Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a poté vyhledejte esp8266.</span><span class="sxs-lookup"><span data-stu-id="0209c-194">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>

   <span data-ttu-id="0209c-195">Panely Manager znamená, že je nainstalována ESP8266 verzí 2.2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0209c-195">Boards Manager indicates that ESP8266 with a version of 2.2.0 or later is installed.</span></span>

   ![instalaci balíčku esp8266](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="0209c-197">Klikněte na tlačítko **nástroje** > **Tabule** > **Adafruit HUZZAH ESP8266**.</span><span class="sxs-lookup"><span data-stu-id="0209c-197">Click **Tools** > **Board** > **Adafruit HUZZAH ESP8266**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="0209c-198">Instalace nezbytné knihovny</span><span class="sxs-lookup"><span data-stu-id="0209c-198">Install necessary libraries</span></span>

1. <span data-ttu-id="0209c-199">V prostředí IDE Arduino, klikněte na tlačítko **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.</span><span class="sxs-lookup"><span data-stu-id="0209c-199">In the Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="0209c-200">Vyhledejte následující knihovny názvy po jednom.</span><span class="sxs-lookup"><span data-stu-id="0209c-200">Search for the following library names one by one.</span></span> <span data-ttu-id="0209c-201">Pro každou knihovnu, která zjistíte, klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="0209c-201">For each  library that you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="0209c-202">Nemáte skutečné senzor DHT22?</span><span class="sxs-lookup"><span data-stu-id="0209c-202">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="0209c-203">Ukázkové aplikace můžete simulovat teploty a vlhkosti dat v případě, že nemáte skutečné DHT22 senzoru.</span><span class="sxs-lookup"><span data-stu-id="0209c-203">The sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="0209c-204">Pokud chcete nastavit ukázkovou aplikaci používat simulované dat, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0209c-204">To set up the sample application to use simulated data, follow these steps:</span></span>

1. <span data-ttu-id="0209c-205">Otevřete `config.h` v soubor `app` složky.</span><span class="sxs-lookup"><span data-stu-id="0209c-205">Open the `config.h` file in the `app` folder.</span></span>
1. <span data-ttu-id="0209c-206">Vyhledejte následující řádek kódu a změňte hodnotu z `false` k `true`:</span><span class="sxs-lookup"><span data-stu-id="0209c-206">Locate the following line of code and change the value from `false` to `true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Nakonfigurujte ukázkovou aplikaci používat simulované dat](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. <span data-ttu-id="0209c-208">Uložte soubor s `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="0209c-208">Save the file with `Control-s`.</span></span>

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a><span data-ttu-id="0209c-209">Nasadit ukázkovou aplikaci pro prolnutí HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="0209c-209">Deploy the sample application to Feather HUZZAH ESP8266</span></span>

1. <span data-ttu-id="0209c-210">V prostředí IDE Arduino, klikněte na tlačítko **nástroj** > **Port**a potom klikněte na sériového portu pro prolnutí HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="0209c-210">In the Arduino IDE, click **Tool** > **Port**, and then click the serial port for Feather HUZZAH ESP8266.</span></span>
1. <span data-ttu-id="0209c-211">Klikněte na tlačítko **nákresu** > **nahrát** k vytváření a nasazování ukázkovou aplikaci pro prolnutí HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="0209c-211">Click **Sketch** > **Upload** to build and deploy the sample application to Feather HUZZAH ESP8266.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="0209c-212">Zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0209c-212">Enter your credentials</span></span>

<span data-ttu-id="0209c-213">Po úspěšném dokončení nahrávání, zadejte své přihlašovací údaje takto:</span><span class="sxs-lookup"><span data-stu-id="0209c-213">After the upload completes successfully, follow these steps to enter your credentials:</span></span>

1. <span data-ttu-id="0209c-214">V prostředí IDE Arduino, klikněte na tlačítko **nástroje** > **sériové monitorování**.</span><span class="sxs-lookup"><span data-stu-id="0209c-214">In the Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="0209c-215">V okně serial sledování Všimněte si dvě rozevíracích seznamů v pravém dolním rohu.</span><span class="sxs-lookup"><span data-stu-id="0209c-215">In the serial monitor window, notice the two drop-down lists in the lower-right corner.</span></span>
1. <span data-ttu-id="0209c-216">Vyberte **žádný řádek ukončování** pro levý rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0209c-216">Select **No line ending** for the left drop-down list.</span></span>
1. <span data-ttu-id="0209c-217">Vyberte **115200 přenosová** pro právo rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0209c-217">Select **115200 baud** for the right drop-down list.</span></span>
1. <span data-ttu-id="0209c-218">Do vstupního pole umístěné v horní části okna sériové sledování, zadejte následující informace, pokud se zobrazí výzva k poskytování je a pak klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="0209c-218">In the input box located at the top of the serial monitor window, enter the following information if you are asked to provide them, and then click **Send**.</span></span>
   * <span data-ttu-id="0209c-219">SSID sítě Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="0209c-219">Wi-Fi SSID</span></span>
   * <span data-ttu-id="0209c-220">Heslo Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="0209c-220">Wi-Fi password</span></span>
   * <span data-ttu-id="0209c-221">Řetězec připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="0209c-221">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="0209c-222">Informace o přihlašovacích údajích uložený v ESP8266 EEPROM z prolnutí HUZZAH.</span><span class="sxs-lookup"><span data-stu-id="0209c-222">The credential information is stored in the EEPROM of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="0209c-223">Pokud kliknete na tlačítko Obnovit na panelu prolnutí HUZZAH ESP8266, ukázkové aplikace zobrazí, pokud chcete vymazat informace.</span><span class="sxs-lookup"><span data-stu-id="0209c-223">If you click the reset button on the Feather HUZZAH ESP8266 board, the sample application asks if you want to erase the information.</span></span> <span data-ttu-id="0209c-224">Zadejte `Y` k dispozici informace vymazat.</span><span class="sxs-lookup"><span data-stu-id="0209c-224">Enter `Y` to have the information erased.</span></span> <span data-ttu-id="0209c-225">Jste vyzváni k zadání informací ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="0209c-225">You are asked to provide the information a second time.</span></span>

### <a name="verify-the-sample-application-is-running-successfully"></a><span data-ttu-id="0209c-226">Zkontrolujte, zda že ukázkové aplikace je úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="0209c-226">Verify the sample application is running successfully</span></span>

<span data-ttu-id="0209c-227">Pokud se zobrazí následující výstup z okna sériové monitorování a blikající LED na prolnutí HUZZAH ESP8266, ukázkové aplikace je úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="0209c-227">If you see the following output from the serial monitor window and the blinking LED on Feather HUZZAH ESP8266, the sample application is running successfully.</span></span>

![Závěrečný výstup v integrovaném vývojovém prostředí Arduino](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="0209c-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0209c-229">Next steps</span></span>

<span data-ttu-id="0209c-230">Máte úspěšně připojen ESP8266 HUZZAH prolnutí do služby IoT hub a data zaznamenaná senzor odeslané do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0209c-230">You have successfully connected a Feather HUZZAH ESP8266 to your IoT hub, and sent the captured sensor data to your IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

