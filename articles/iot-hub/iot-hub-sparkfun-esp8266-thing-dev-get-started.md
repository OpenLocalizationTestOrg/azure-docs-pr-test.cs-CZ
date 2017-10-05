---
title: "ESP8266 do cloudu - připojení Sparkfun ESP8266 věc Dev do služby Azure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak nastavit a připojení k Azure IoT Hub pro něj k odesílání dat do Azure Cloudová platforma v tomto kurzu Sparkfun ESP8266 věc vývojářů."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 557f0cdf375b345e0dbe0526f5a5bd3c050dec38
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="1ae8e-103">Připojení k Azure IoT Hub v cloudu Sparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="1ae8e-103">Connect Sparkfun ESP8266 Thing Dev to Azure IoT Hub in the cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![připojení mezi DHT22, co vývojářů a IoT Hub](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a><span data-ttu-id="1ae8e-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="1ae8e-105">What you will do</span></span>

<span data-ttu-id="1ae8e-106">Sparkfun ESP8266 věc vývojářů připojte do služby IoT hub, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-106">Connect Sparkfun ESP8266 Thing Dev to an IoT hub you will create.</span></span> <span data-ttu-id="1ae8e-107">Pak spusťte ukázkovou aplikaci na ESP8266 ke shromažďování dat teploty a vlhkosti ze DHT22 senzoru.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-107">Then run a sample application on ESP8266 to collect temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="1ae8e-108">Nakonec odešlete data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-108">Finally, send the sensor data to your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="1ae8e-109">Pokud používáte jiné ESP8266 panely, můžete stále následujícím postupem pro připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-109">If you are using other ESP8266 boards, you can still follow these steps to connect it to your IoT hub.</span></span> <span data-ttu-id="1ae8e-110">V závislosti na ESP8266 Tabule, kterou používáte, bude pravděpodobně třeba překonfigurovat `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-110">Depending on the ESP8266 board you are using, you may need to reconfigure the `LED_PIN`.</span></span> <span data-ttu-id="1ae8e-111">Například pokud používáte ESP8266 z AI Thinker, můžete změnit z `0` k `2`.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-111">For example, if you are using ESP8266 from AI-Thinker, you may change it from `0` to `2`.</span></span> <span data-ttu-id="1ae8e-112">Ještě není hotová kit?: klikněte na tlačítko [sem](http://azure.com/iotstarterkits)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-112">Don't have a kit yet?: Click [here](http://azure.com/iotstarterkits)</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="1ae8e-113">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="1ae8e-113">What you will learn</span></span>

* <span data-ttu-id="1ae8e-114">Postup vytvoření služby IoT hub a registrovat zařízení za účelem věcí</span><span class="sxs-lookup"><span data-stu-id="1ae8e-114">How to create an IoT hub and register a device for Thing Dev.</span></span>
* <span data-ttu-id="1ae8e-115">Postup připojení věc Dev s senzoru a váš počítač.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-115">How to connect Thing Dev with the sensor and your computer.</span></span>
* <span data-ttu-id="1ae8e-116">Postup shromažďování dat snímačů spuštěním ukázkovou aplikaci na věc účelem</span><span class="sxs-lookup"><span data-stu-id="1ae8e-116">How to collect sensor data by running a sample application on Thing Dev.</span></span>
* <span data-ttu-id="1ae8e-117">Jak odesílat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-117">How to send the sensor data to your IoT hub.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="1ae8e-118">Co budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="1ae8e-118">What you will need</span></span>

![součásti potřebné pro tento kurz](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="1ae8e-120">Pro dokončení této operace, musíte z vaší věc Dev Starter Kit následujících částí:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-120">To complete this operation, you need the following parts from your Thing Dev Starter Kit:</span></span>

* <span data-ttu-id="1ae8e-121">Panel Sparkfun ESP8266 věc vývojářů.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-121">The Sparkfun ESP8266 Thing Dev board.</span></span>
* <span data-ttu-id="1ae8e-122">Micro USB na kabelu USB typ A.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-122">A Micro USB to Type A USB cable.</span></span>

<span data-ttu-id="1ae8e-123">Vývojové prostředí musíte také následující:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-123">You also need the following for your development environment:</span></span>

* <span data-ttu-id="1ae8e-124">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-124">An active Azure subscription.</span></span> <span data-ttu-id="1ae8e-125">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-125">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="1ae8e-126">Mac nebo počítači se systémem Windows nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-126">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="1ae8e-127">Bezdrátové sítě pro Sparkfun ESP8266 věc vývojářů pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-127">Wireless network for Sparkfun ESP8266 Thing Dev to connect to.</span></span>
* <span data-ttu-id="1ae8e-128">Připojení k Internetu stahovat nástroj konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-128">Internet connection to download the configuration tool.</span></span>
* <span data-ttu-id="1ae8e-129">[Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 (nebo novější), dřívější verze nebudou pracovat s knihovnou AzureIoT.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-129">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 (or newer), earlier versions will not work with the AzureIoT library.</span></span>

<span data-ttu-id="1ae8e-130">Následující položky jsou volitelné, v případě, že nemáte senzoru.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-130">The following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="1ae8e-131">Máte také možnost používat data snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-131">You also have the option of using simulated sensor data.</span></span>

* <span data-ttu-id="1ae8e-132">Adafruit DHT22 teploty a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-132">An Adafruit DHT22 temperature and humidity sensor.</span></span>
* <span data-ttu-id="1ae8e-133">Breadboard.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-133">A breadboard.</span></span>
* <span data-ttu-id="1ae8e-134">Vodičům můstek M/M.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-134">M/M jumper wires.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-the-sensor-and-your-computer"></a><span data-ttu-id="1ae8e-135">Připojit ESP8266 věc Dev s senzoru a počítače</span><span class="sxs-lookup"><span data-stu-id="1ae8e-135">Connect ESP8266 Thing Dev with the sensor and your computer</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-esp8266-thing-dev"></a><span data-ttu-id="1ae8e-136">Senzor teploty a vlhkosti DHT22 připojit k ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="1ae8e-136">Connect a DHT22 temperature and humidity sensor to ESP8266 Thing Dev</span></span>

<span data-ttu-id="1ae8e-137">Pomocí vedení breadboard a můstek k připojení následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-137">Use the breadboard and jumper wires to make the connection as follows.</span></span> <span data-ttu-id="1ae8e-138">Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-138">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![odkaz na připojení](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

<span data-ttu-id="1ae8e-140">Pro kód PIN senzor použijeme následující kabeláž:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-140">For sensor pins, we will use the following wiring:</span></span>

| <span data-ttu-id="1ae8e-141">Spuštění (senzor)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-141">Start (Sensor)</span></span>           | <span data-ttu-id="1ae8e-142">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-142">End (Board)</span></span>           | <span data-ttu-id="1ae8e-143">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="1ae8e-143">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="1ae8e-144">VDD (Pin 27F)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-144">VDD (Pin 27F)</span></span>            | <span data-ttu-id="1ae8e-145">3v (8A PIN kódu)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-145">3V (Pin 8A)</span></span>           | <span data-ttu-id="1ae8e-146">Red kabel</span><span class="sxs-lookup"><span data-stu-id="1ae8e-146">Red cable</span></span>     |
| <span data-ttu-id="1ae8e-147">DATA (Pin 28F)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-147">DATA (Pin 28F)</span></span>           | <span data-ttu-id="1ae8e-148">GPIO 2 (9A PIN kódu)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-148">GPIO 2 (Pin 9A)</span></span>       | <span data-ttu-id="1ae8e-149">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="1ae8e-149">White cable</span></span>    |
| <span data-ttu-id="1ae8e-150">ZEM (Pin 30F)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-150">GND (Pin 30F)</span></span>            | <span data-ttu-id="1ae8e-151">ZEM (7J kód Pin)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-151">GND (Pin 7J)</span></span>          | <span data-ttu-id="1ae8e-152">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="1ae8e-152">Black cable</span></span>   |


- <span data-ttu-id="1ae8e-153">Další informace najdete v tématu: [DHT22 senzor instalace](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) a [specifikace Sparkfun ESP8266 věc vývojářů](https://www.sparkfun.com/products/13711)</span><span class="sxs-lookup"><span data-stu-id="1ae8e-153">For more information, see: [DHT22 sensor setup](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) and [Sparkfun ESP8266 Thing Dev specification](https://www.sparkfun.com/products/13711)</span></span>

<span data-ttu-id="1ae8e-154">Teď vaše Sparkfun ESP8266 věc Dev by měly být připojené s pracovní senzoru.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-154">Now your Sparkfun ESP8266 Thing Dev should be connected with a working sensor.</span></span>

![připojit dht22 s ESP8266 věc vývojářů](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-to-your-computer"></a><span data-ttu-id="1ae8e-156">Připojte k počítači Sparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="1ae8e-156">Connect Sparkfun ESP8266 Thing Dev to your computer</span></span>

<span data-ttu-id="1ae8e-157">USB Micro kabelem USB typ A slouží k připojení Sparkfun ESP8266 věc vývojářů v počítači následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-157">Use the Micro USB to Type A USB cable to connect Sparkfun ESP8266 Thing Dev to your computer as follows.</span></span>

![prolnutí huzzah připojte k počítači](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a><span data-ttu-id="1ae8e-159">Přidání oprávnění sériového portu – pouze Ubuntu</span><span class="sxs-lookup"><span data-stu-id="1ae8e-159">Add serial port permissions – Ubuntu only</span></span>

<span data-ttu-id="1ae8e-160">Pokud používáte Ubuntu, zkontrolujte, zda že normální uživatel má oprávnění k provozu na USB port z Sparkfun ESP8266 věc účelem</span><span class="sxs-lookup"><span data-stu-id="1ae8e-160">If you use Ubuntu, make sure a normal user has the permissions to operate on the USB port of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="1ae8e-161">Pokud chcete přidat oprávnění sériového portu pro běžné uživatele, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-161">To add serial port permissions for a normal user, follow these steps:</span></span>

1. <span data-ttu-id="1ae8e-162">V terminálu, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-162">Run the following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="1ae8e-163">Zobrazí jednu z následujících výstupy:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-163">You get one of the following outputs:</span></span>

   * <span data-ttu-id="1ae8e-164">CRW-rw---1 kořenové uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="1ae8e-164">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="1ae8e-165">CRW-rw---xxxxxxxx síti službou 1 root</span><span class="sxs-lookup"><span data-stu-id="1ae8e-165">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="1ae8e-166">Ve výstupu, Všimněte si `uucp` nebo `dialout` který je vlastníkem název skupiny portu USB.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-166">In the output, notice `uucp` or `dialout` that is the group owner name of the USB port.</span></span>

1. <span data-ttu-id="1ae8e-167">Přidejte uživatele do skupiny tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-167">Add the user to the group by running the following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="1ae8e-168">`<group-owner-name>`je název vlastníka skupiny, kterou jste získali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-168">`<group-owner-name>` is the group owner name you obtained in the previous step.</span></span> <span data-ttu-id="1ae8e-169">`<username>`je Ubuntu uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-169">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="1ae8e-170">Odhlaste se Ubuntu a přihlaste se znovu pro změna se projeví.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-170">Log out Ubuntu and log in it again for the change to take effect.</span></span>

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a><span data-ttu-id="1ae8e-171">Shromažďování dat snímačů a odeslat do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="1ae8e-171">Collect sensor data and send it to your IoT hub</span></span>

<span data-ttu-id="1ae8e-172">V této části můžete nasazení a spuštění ukázkové aplikace na Sparkfun ESP8266 věc účelem</span><span class="sxs-lookup"><span data-stu-id="1ae8e-172">In this section, you deploy and run a sample application on Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="1ae8e-173">Ukázkovou aplikaci bliká DIODU na Sparkfun ESP8266 věc vývojářů a odešle teploty a vlhkosti data shromážděná z senzoru DHT22 do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-173">The sample application blinks the LED on Sparkfun ESP8266 Thing Dev and sends the temperature and humidity data collected from the DHT22 sensor to your IoT hub.</span></span>

### <a name="get-the-sample-application-from-github"></a><span data-ttu-id="1ae8e-174">Získat ukázkovou aplikaci z webu GitHub</span><span class="sxs-lookup"><span data-stu-id="1ae8e-174">Get the sample application from GitHub</span></span>

<span data-ttu-id="1ae8e-175">Ukázkové aplikace jsou hostované na Githubu.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-175">The sample application is hosted on GitHub.</span></span> <span data-ttu-id="1ae8e-176">Naklonujte úložiště ukázka, která obsahuje ukázkovou aplikaci z webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-176">Clone the sample repository that contains the sample application from GitHub.</span></span> <span data-ttu-id="1ae8e-177">Klonovat úložiště ukázkové, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-177">To clone the sample repository, follow these steps:</span></span>

1. <span data-ttu-id="1ae8e-178">Otevřete příkazový řádek nebo okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-178">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="1ae8e-179">Přejděte do složky, kam chcete ukázkovou aplikaci k uložení.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-179">Go to a folder where you want the sample application to be stored.</span></span>
1. <span data-ttu-id="1ae8e-180">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-180">Run the following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

<span data-ttu-id="1ae8e-181">Instalace balíčku pro vývojáře věc ESP8266 Sparkfun v integrovaném vývojovém prostředí Arduino:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-181">Install the package for Sparkfun ESP8266 Thing Dev in Arduino IDE:</span></span>

1. <span data-ttu-id="1ae8e-182">Otevřete složku, kde je uložen ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-182">Open the folder where the sample application is stored.</span></span>
1. <span data-ttu-id="1ae8e-183">Otevřete soubor app.ino ve složce aplikace v integrovaném vývojovém prostředí Arduino.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-183">Open the app.ino file in the app folder in Arduino IDE.</span></span>

   ![Otevřete ukázkovou aplikaci v arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="1ae8e-185">V prostředí IDE Arduino, klikněte na tlačítko **soubor** > **Předvolby**.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-185">In the Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="1ae8e-186">V **Předvolby** dialogové okno pole, klikněte na ikonu vedle **další adresy URL Manager panely** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-186">In the **Preferences** dialog box, click the icon next to the **Additional Boards Manager URLs** text box.</span></span>
1. <span data-ttu-id="1ae8e-187">V místním okně, zadejte následující adresu URL a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-187">In the pop-up window, enter the following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![přejděte na adresu url balíčku v arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="1ae8e-189">V **předvoleb** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-189">In the **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="1ae8e-190">Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a poté vyhledejte esp8266.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-190">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>
   <span data-ttu-id="1ae8e-191">Musí být nainstalován ESP8266 verzí 2.2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-191">ESP8266 with a version of 2.2.0 or later should be installed.</span></span>

   ![instalaci balíčku esp8266](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="1ae8e-193">Klikněte na tlačítko **nástroje** > **Tabule** > **Sparkfun ESP8266 věc Dev**.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-193">Click **Tools** > **Board** > **Sparkfun ESP8266 Thing Dev**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="1ae8e-194">Instalace nezbytné knihovny</span><span class="sxs-lookup"><span data-stu-id="1ae8e-194">Install necessary libraries</span></span>

1. <span data-ttu-id="1ae8e-195">V prostředí IDE Arduino, klikněte na tlačítko **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-195">In the Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="1ae8e-196">Vyhledejte následující knihovny názvy po jednom.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-196">Search for the following library names one by one.</span></span> <span data-ttu-id="1ae8e-197">Pro každou z knihovny zjistíte, klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-197">For each of the library you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="1ae8e-198">Nemáte skutečné senzor DHT22?</span><span class="sxs-lookup"><span data-stu-id="1ae8e-198">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="1ae8e-199">Ukázkové aplikace můžete simulovat teploty a vlhkosti dat v případě, že nemáte skutečné DHT22 senzoru.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-199">The sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="1ae8e-200">Pokud chcete povolit ukázkovou aplikaci pro simulované data použít, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-200">To enable the sample application to use simulated data, follow these steps:</span></span>

1. <span data-ttu-id="1ae8e-201">Otevřete `config.h` v soubor `app` složky.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-201">Open the `config.h` file in the `app` folder.</span></span>
1. <span data-ttu-id="1ae8e-202">Vyhledejte následující řádek kódu a změňte hodnotu z `false` k `true`:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-202">Locate the following line of code and change the value from `false` to `true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Nakonfigurujte ukázkovou aplikaci používat simulované dat](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. <span data-ttu-id="1ae8e-204">Uložit s `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-204">Save with `Control-s`.</span></span>

### <a name="deploy-the-sample-application-to-sparkfun-esp8266-thing-dev"></a><span data-ttu-id="1ae8e-205">Nasadit ukázkovou aplikaci pro Sparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="1ae8e-205">Deploy the sample application to Sparkfun ESP8266 Thing Dev</span></span>

1. <span data-ttu-id="1ae8e-206">V prostředí IDE Arduino, klikněte na tlačítko **nástroj** > **Port**a potom klikněte na sériového portu za účelem co ESP8266 Sparkfun</span><span class="sxs-lookup"><span data-stu-id="1ae8e-206">In the Arduino IDE, click **Tool** > **Port**, and then click the serial port for Sparkfun ESP8266 Thing Dev.</span></span>
1. <span data-ttu-id="1ae8e-207">Klikněte na tlačítko **nákresu** > **nahrát** sestavení a nasazení ukázkové aplikace na Sparkfun ESP8266 věc účelem</span><span class="sxs-lookup"><span data-stu-id="1ae8e-207">Click **Sketch** > **Upload** to build and deploy the sample application to Sparkfun ESP8266 Thing Dev.</span></span>

> [!Note]
> <span data-ttu-id="1ae8e-208">Pokud používáte systému macOS během nahrávání se pravděpodobně může zobrazit následující zprávy.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-208">If you are using macOS you could probably see the following messages during uploading.</span></span> <span data-ttu-id="1ae8e-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span></span> <span data-ttu-id="1ae8e-210">Otevřete okno vaší ternimal a dokončit následující akce pro tento problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-210">Please open your ternimal window and finish below actions to solve this issue.</span></span>
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a><span data-ttu-id="1ae8e-211">Zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-211">Enter your credentials</span></span>

<span data-ttu-id="1ae8e-212">Po úspěšném dokončení nahrávání, postupujte podle kroků k zadání přihlašovacích údajů:</span><span class="sxs-lookup"><span data-stu-id="1ae8e-212">After the upload completes successfully, follow the steps to enter your credentials:</span></span>

1. <span data-ttu-id="1ae8e-213">V prostředí IDE Arduino, klikněte na tlačítko **nástroje** > **sériové monitorování**.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-213">In the Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="1ae8e-214">V okně serial monitorování Všimněte si dva rozevírací seznamy v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-214">In the serial monitor window, notice the two drop-down lists on the bottom right corner.</span></span>
1. <span data-ttu-id="1ae8e-215">Vyberte **žádný řádek ukončování** pro levý rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-215">Select **No line ending** for the left drop-down list.</span></span>
1. <span data-ttu-id="1ae8e-216">Vyberte **115200 přenosová** pro právo rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-216">Select **115200 baud** for the right drop-down list.</span></span>
1. <span data-ttu-id="1ae8e-217">Do vstupního pole umístěné v horní části okna sériové sledování, zadejte následující informace, pokud se zobrazí výzva k poskytování je a pak klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-217">In the input box located at the top of the serial monitor window, enter the following information if you are asked to provide them, and then click **Send**.</span></span>
   * <span data-ttu-id="1ae8e-218">SSID sítě Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="1ae8e-218">Wi-Fi SSID</span></span>
   * <span data-ttu-id="1ae8e-219">Heslo Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="1ae8e-219">Wi-Fi password</span></span>
   * <span data-ttu-id="1ae8e-220">Řetězec připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="1ae8e-220">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="1ae8e-221">Informace o přihlašovacích údajích je uložené v paměti EEPROM ESP8266 věc účelem Sparkfun</span><span class="sxs-lookup"><span data-stu-id="1ae8e-221">The credential information is stored in the EEPROM of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="1ae8e-222">Pokud kliknete na tlačítko Obnovit na panelu Sparkfun ESP8266 věc vývojářů, ukázkové aplikace zobrazí dotaz, pokud chcete vymazat informace.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-222">If you click the reset button on the Sparkfun ESP8266 Thing Dev board, the sample application asks you if you want to erase the information.</span></span> <span data-ttu-id="1ae8e-223">Zadejte `Y` tak, aby měl informace vymazat a budete vyzváni k zadání informace znovu.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-223">Enter `Y` to have the information erased and you are asked to provide the information again.</span></span>

### <a name="verify-the-sample-application-is-running-successfully"></a><span data-ttu-id="1ae8e-224">Zkontrolujte, zda že ukázkové aplikace je úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-224">Verify the sample application is running successfully</span></span>

<span data-ttu-id="1ae8e-225">Pokud se zobrazí následující výstup z okna sériové monitorování a blikající LED na Sparkfun ESP8266 věc vývojářů, ukázkové aplikace je úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-225">If you see the following output from the serial monitor window and the blinking LED on Sparkfun ESP8266 Thing Dev, the sample application is running successfully.</span></span>

![závěrečný výstup v integrovaném vývojovém prostředí arduino](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="1ae8e-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1ae8e-227">Next steps</span></span>

<span data-ttu-id="1ae8e-228">Úspěšně jste připojené Sparkfun ESP8266 věc Dev do služby IoT hub a data zaznamenaná senzor odeslané do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1ae8e-228">You have successfully connected a Sparkfun ESP8266 Thing Dev to your IoT hub and sent the captured sensor data to your IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
