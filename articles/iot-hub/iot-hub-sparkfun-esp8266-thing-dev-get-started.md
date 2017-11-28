---
title: "aaaESP8266 toocloud - tooAzure připojit Sparkfun ESP8266 věc Dev IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte Sparkfun ESP8266 věc Dev tooAzure IoT Hub pro něj toosend data toohello Azure Cloudová platforma v tomto kurzu."
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
ms.openlocfilehash: 19b249df23b6df516634853521c6d532f51014da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="79cfb-103">Připojit Sparkfun ESP8266 věc Dev tooAzure IoT Hub v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="79cfb-103">Connect Sparkfun ESP8266 Thing Dev tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![připojení mezi DHT22, co vývojářů a IoT Hub](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a><span data-ttu-id="79cfb-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="79cfb-105">What you will do</span></span>

<span data-ttu-id="79cfb-106">Připojte Sparkfun ESP8266 věc Dev tooan IoT hub, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="79cfb-106">Connect Sparkfun ESP8266 Thing Dev tooan IoT hub you will create.</span></span> <span data-ttu-id="79cfb-107">Pak spusťte ukázkovou aplikaci na ESP8266 toocollect teploty a vlhkosti data z DHT22 senzoru.</span><span class="sxs-lookup"><span data-stu-id="79cfb-107">Then run a sample application on ESP8266 toocollect temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="79cfb-108">Nakonec odešlete hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79cfb-108">Finally, send hello sensor data tooyour IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="79cfb-109">Pokud používáte jiné ESP8266 panely, můžete stále postupujte podle těchto kroků tooconnect ho tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79cfb-109">If you are using other ESP8266 boards, you can still follow these steps tooconnect it tooyour IoT hub.</span></span> <span data-ttu-id="79cfb-110">V závislosti na hello ESP8266 Tabule, kterou používáte, může být nutné tooreconfigure hello `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="79cfb-110">Depending on hello ESP8266 board you are using, you may need tooreconfigure hello `LED_PIN`.</span></span> <span data-ttu-id="79cfb-111">Například pokud používáte ESP8266 z AI Thinker, můžete změnit z `0` příliš`2`.</span><span class="sxs-lookup"><span data-stu-id="79cfb-111">For example, if you are using ESP8266 from AI-Thinker, you may change it from `0` too`2`.</span></span> <span data-ttu-id="79cfb-112">Ještě není hotová kit?: klikněte na tlačítko [sem](http://azure.com/iotstarterkits)</span><span class="sxs-lookup"><span data-stu-id="79cfb-112">Don't have a kit yet?: Click [here](http://azure.com/iotstarterkits)</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="79cfb-113">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="79cfb-113">What you will learn</span></span>

* <span data-ttu-id="79cfb-114">Jak toocreate služby IoT hub a registrovat zařízení za účelem věcí</span><span class="sxs-lookup"><span data-stu-id="79cfb-114">How toocreate an IoT hub and register a device for Thing Dev.</span></span>
* <span data-ttu-id="79cfb-115">Jak tooconnect věc Dev s hello senzor a váš počítač.</span><span class="sxs-lookup"><span data-stu-id="79cfb-115">How tooconnect Thing Dev with hello sensor and your computer.</span></span>
* <span data-ttu-id="79cfb-116">Jak data snímačů toocollect spuštěním ukázkovou aplikaci na věc účelem</span><span class="sxs-lookup"><span data-stu-id="79cfb-116">How toocollect sensor data by running a sample application on Thing Dev.</span></span>
* <span data-ttu-id="79cfb-117">Jak toosend hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79cfb-117">How toosend hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="79cfb-118">Co budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="79cfb-118">What you will need</span></span>

![součásti potřebné pro kurz hello](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="79cfb-120">toocomplete tuto operaci je třeba hello následujících částí z vaší věc Dev Starter Kit:</span><span class="sxs-lookup"><span data-stu-id="79cfb-120">toocomplete this operation, you need hello following parts from your Thing Dev Starter Kit:</span></span>

* <span data-ttu-id="79cfb-121">Hello Tabule Sparkfun ESP8266 věc vývojářů.</span><span class="sxs-lookup"><span data-stu-id="79cfb-121">hello Sparkfun ESP8266 Thing Dev board.</span></span>
* <span data-ttu-id="79cfb-122">Micro USB tooType kabelu A USB.</span><span class="sxs-lookup"><span data-stu-id="79cfb-122">A Micro USB tooType A USB cable.</span></span>

<span data-ttu-id="79cfb-123">Potřebujete taky následující hello vývojové prostředí:</span><span class="sxs-lookup"><span data-stu-id="79cfb-123">You also need hello following for your development environment:</span></span>

* <span data-ttu-id="79cfb-124">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="79cfb-124">An active Azure subscription.</span></span> <span data-ttu-id="79cfb-125">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="79cfb-125">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="79cfb-126">Mac nebo počítači se systémem Windows nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="79cfb-126">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="79cfb-127">Bezdrátové sítě pro tooconnect Sparkfun ESP8266 věc Dev k.</span><span class="sxs-lookup"><span data-stu-id="79cfb-127">Wireless network for Sparkfun ESP8266 Thing Dev tooconnect to.</span></span>
* <span data-ttu-id="79cfb-128">Nástroj pro konfiguraci internetové připojení toodownload hello.</span><span class="sxs-lookup"><span data-stu-id="79cfb-128">Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="79cfb-129">[Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 (nebo novější), dřívější verze nebudou pracovat s knihovnou AzureIoT hello.</span><span class="sxs-lookup"><span data-stu-id="79cfb-129">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 (or newer), earlier versions will not work with hello AzureIoT library.</span></span>

<span data-ttu-id="79cfb-130">Hello následující položky jsou volitelné v případě, že nemáte senzoru.</span><span class="sxs-lookup"><span data-stu-id="79cfb-130">hello following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="79cfb-131">Máte také možnost hello použití dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="79cfb-131">You also have hello option of using simulated sensor data.</span></span>

* <span data-ttu-id="79cfb-132">Adafruit DHT22 teploty a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="79cfb-132">An Adafruit DHT22 temperature and humidity sensor.</span></span>
* <span data-ttu-id="79cfb-133">Breadboard.</span><span class="sxs-lookup"><span data-stu-id="79cfb-133">A breadboard.</span></span>
* <span data-ttu-id="79cfb-134">Vodičům můstek M/M.</span><span class="sxs-lookup"><span data-stu-id="79cfb-134">M/M jumper wires.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-hello-sensor-and-your-computer"></a><span data-ttu-id="79cfb-135">Připojit ESP8266 věc Dev s hello senzor a počítače</span><span class="sxs-lookup"><span data-stu-id="79cfb-135">Connect ESP8266 Thing Dev with hello sensor and your computer</span></span>

### <a name="connect-a-dht22-temperature-and-humidity-sensor-tooesp8266-thing-dev"></a><span data-ttu-id="79cfb-136">Senzor teploty a vlhkosti DHT22 připojit tooESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="79cfb-136">Connect a DHT22 temperature and humidity sensor tooESP8266 Thing Dev</span></span>

<span data-ttu-id="79cfb-137">Použijte hello breadboard a můstek vodičům toomake hello připojení následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="79cfb-137">Use hello breadboard and jumper wires toomake hello connection as follows.</span></span> <span data-ttu-id="79cfb-138">Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.</span><span class="sxs-lookup"><span data-stu-id="79cfb-138">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![odkaz na připojení](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

<span data-ttu-id="79cfb-140">Pro kód PIN senzor použijeme následující kabeláž hello:</span><span class="sxs-lookup"><span data-stu-id="79cfb-140">For sensor pins, we will use hello following wiring:</span></span>

| <span data-ttu-id="79cfb-141">Spuštění (senzor)</span><span class="sxs-lookup"><span data-stu-id="79cfb-141">Start (Sensor)</span></span>           | <span data-ttu-id="79cfb-142">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="79cfb-142">End (Board)</span></span>           | <span data-ttu-id="79cfb-143">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="79cfb-143">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="79cfb-144">VDD (Pin 27F)</span><span class="sxs-lookup"><span data-stu-id="79cfb-144">VDD (Pin 27F)</span></span>            | <span data-ttu-id="79cfb-145">3v (8A PIN kódu)</span><span class="sxs-lookup"><span data-stu-id="79cfb-145">3V (Pin 8A)</span></span>           | <span data-ttu-id="79cfb-146">Red kabel</span><span class="sxs-lookup"><span data-stu-id="79cfb-146">Red cable</span></span>     |
| <span data-ttu-id="79cfb-147">DATA (Pin 28F)</span><span class="sxs-lookup"><span data-stu-id="79cfb-147">DATA (Pin 28F)</span></span>           | <span data-ttu-id="79cfb-148">GPIO 2 (9A PIN kódu)</span><span class="sxs-lookup"><span data-stu-id="79cfb-148">GPIO 2 (Pin 9A)</span></span>       | <span data-ttu-id="79cfb-149">Bílé kabel</span><span class="sxs-lookup"><span data-stu-id="79cfb-149">White cable</span></span>    |
| <span data-ttu-id="79cfb-150">ZEM (Pin 30F)</span><span class="sxs-lookup"><span data-stu-id="79cfb-150">GND (Pin 30F)</span></span>            | <span data-ttu-id="79cfb-151">ZEM (7J kód Pin)</span><span class="sxs-lookup"><span data-stu-id="79cfb-151">GND (Pin 7J)</span></span>          | <span data-ttu-id="79cfb-152">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="79cfb-152">Black cable</span></span>   |


- <span data-ttu-id="79cfb-153">Další informace najdete v tématu: [DHT22 senzor instalace](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) a [specifikace Sparkfun ESP8266 věc vývojářů](https://www.sparkfun.com/products/13711)</span><span class="sxs-lookup"><span data-stu-id="79cfb-153">For more information, see: [DHT22 sensor setup](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) and [Sparkfun ESP8266 Thing Dev specification](https://www.sparkfun.com/products/13711)</span></span>

<span data-ttu-id="79cfb-154">Teď vaše Sparkfun ESP8266 věc Dev by měly být připojené s pracovní senzoru.</span><span class="sxs-lookup"><span data-stu-id="79cfb-154">Now your Sparkfun ESP8266 Thing Dev should be connected with a working sensor.</span></span>

![připojit dht22 s ESP8266 věc vývojářů](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-tooyour-computer"></a><span data-ttu-id="79cfb-156">Připojte počítač tooyour Sparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="79cfb-156">Connect Sparkfun ESP8266 Thing Dev tooyour computer</span></span>

<span data-ttu-id="79cfb-157">Použijte hello Micro USB tooType A USB kabel tooconnect Sparkfun ESP8266 věc Dev tooyour počítači následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="79cfb-157">Use hello Micro USB tooType A USB cable tooconnect Sparkfun ESP8266 Thing Dev tooyour computer as follows.</span></span>

![Připojte počítač tooyour huzzah prolnutí](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a><span data-ttu-id="79cfb-159">Přidání oprávnění sériového portu – pouze Ubuntu</span><span class="sxs-lookup"><span data-stu-id="79cfb-159">Add serial port permissions – Ubuntu only</span></span>

<span data-ttu-id="79cfb-160">Pokud používáte Ubuntu, zkontrolujte, zda že má uživatel normální hello oprávnění toooperate na hello USB port z Sparkfun ESP8266 věc účelem</span><span class="sxs-lookup"><span data-stu-id="79cfb-160">If you use Ubuntu, make sure a normal user has hello permissions toooperate on hello USB port of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="79cfb-161">tooadd sériového portu oprávnění pro běžné uživatele, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="79cfb-161">tooadd serial port permissions for a normal user, follow these steps:</span></span>

1. <span data-ttu-id="79cfb-162">Spusťte následující příkazy v terminálu hello:</span><span class="sxs-lookup"><span data-stu-id="79cfb-162">Run hello following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="79cfb-163">Zobrazí jednu z následujících výstupy hello:</span><span class="sxs-lookup"><span data-stu-id="79cfb-163">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="79cfb-164">CRW-rw---1 kořenové uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="79cfb-164">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="79cfb-165">CRW-rw---xxxxxxxx síti službou 1 root</span><span class="sxs-lookup"><span data-stu-id="79cfb-165">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="79cfb-166">Ve výstupu hello, Všimněte si `uucp` nebo `dialout` tedy hello vlastníka název skupiny hello portu USB.</span><span class="sxs-lookup"><span data-stu-id="79cfb-166">In hello output, notice `uucp` or `dialout` that is hello group owner name of hello USB port.</span></span>

1. <span data-ttu-id="79cfb-167">Přidáte skupinu toohello uživatelů hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="79cfb-167">Add hello user toohello group by running hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="79cfb-168">`<group-owner-name>`je název skupiny vlastníka hello, kterou jste získali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="79cfb-168">`<group-owner-name>` is hello group owner name you obtained in hello previous step.</span></span> <span data-ttu-id="79cfb-169">`<username>`je Ubuntu uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="79cfb-169">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="79cfb-170">Odhlaste Ubuntu a přihlaste se znovu pro hello změnit tootake efekt.</span><span class="sxs-lookup"><span data-stu-id="79cfb-170">Log out Ubuntu and log in it again for hello change tootake effect.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="79cfb-171">Shromažďování dat snímačů a odeslat tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="79cfb-171">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="79cfb-172">V této části můžete nasazení a spuštění ukázkové aplikace na Sparkfun ESP8266 věc účelem</span><span class="sxs-lookup"><span data-stu-id="79cfb-172">In this section, you deploy and run a sample application on Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="79cfb-173">Hello ukázkovou aplikaci bliká hello DIODU na Sparkfun ESP8266 věc vývojářů a odešle hello teploty a vlhkosti data shromážděná z hello DHT22 senzor tooyour služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79cfb-173">hello sample application blinks hello LED on Sparkfun ESP8266 Thing Dev and sends hello temperature and humidity data collected from hello DHT22 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github"></a><span data-ttu-id="79cfb-174">Získat hello ukázkovou aplikaci z webu GitHub</span><span class="sxs-lookup"><span data-stu-id="79cfb-174">Get hello sample application from GitHub</span></span>

<span data-ttu-id="79cfb-175">Ukázková aplikace Hello jsou hostované na Githubu.</span><span class="sxs-lookup"><span data-stu-id="79cfb-175">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="79cfb-176">Klonování úložiště hello ukázka, která obsahuje ukázkovou aplikaci hello z Githubu.</span><span class="sxs-lookup"><span data-stu-id="79cfb-176">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="79cfb-177">tooclone hello Ukázka úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="79cfb-177">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="79cfb-178">Otevřete příkazový řádek nebo okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="79cfb-178">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="79cfb-179">Přejděte tooa složku, kam chcete hello ukázkové aplikace toobe uložené.</span><span class="sxs-lookup"><span data-stu-id="79cfb-179">Go tooa folder where you want hello sample application toobe stored.</span></span>
1. <span data-ttu-id="79cfb-180">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="79cfb-180">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

<span data-ttu-id="79cfb-181">Instalovat balíček hello pro vývojáře věc ESP8266 Sparkfun v integrovaném vývojovém prostředí Arduino:</span><span class="sxs-lookup"><span data-stu-id="79cfb-181">Install hello package for Sparkfun ESP8266 Thing Dev in Arduino IDE:</span></span>

1. <span data-ttu-id="79cfb-182">Otevřete složku hello se uloží hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="79cfb-182">Open hello folder where hello sample application is stored.</span></span>
1. <span data-ttu-id="79cfb-183">Otevřete soubor app.ino hello ve složce aplikace hello v Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="79cfb-183">Open hello app.ino file in hello app folder in Arduino IDE.</span></span>

   ![Otevřete hello ukázkovou aplikaci v arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="79cfb-185">V hello Arduino IDE, klikněte na **soubor** > **Předvolby**.</span><span class="sxs-lookup"><span data-stu-id="79cfb-185">In hello Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="79cfb-186">V hello **Předvolby** dialogové okno pole, klikněte na tlačítko Další toohello ikonu hello **další adresy URL Manager panely** textové pole.</span><span class="sxs-lookup"><span data-stu-id="79cfb-186">In hello **Preferences** dialog box, click hello icon next toohello **Additional Boards Manager URLs** text box.</span></span>
1. <span data-ttu-id="79cfb-187">V místním okně hello, zadejte následující adresu URL hello a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="79cfb-187">In hello pop-up window, enter hello following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Adresa url balíčku bodu tooa v arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="79cfb-189">V hello **předvoleb** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="79cfb-189">In hello **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="79cfb-190">Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a poté vyhledejte esp8266.</span><span class="sxs-lookup"><span data-stu-id="79cfb-190">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>
   <span data-ttu-id="79cfb-191">Musí být nainstalován ESP8266 verzí 2.2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="79cfb-191">ESP8266 with a version of 2.2.0 or later should be installed.</span></span>

   ![je nainstalovaný balíček esp8266 Hello](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="79cfb-193">Klikněte na tlačítko **nástroje** > **Tabule** > **Sparkfun ESP8266 věc Dev**.</span><span class="sxs-lookup"><span data-stu-id="79cfb-193">Click **Tools** > **Board** > **Sparkfun ESP8266 Thing Dev**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="79cfb-194">Instalace nezbytné knihovny</span><span class="sxs-lookup"><span data-stu-id="79cfb-194">Install necessary libraries</span></span>

1. <span data-ttu-id="79cfb-195">V hello Arduino IDE, klikněte na **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.</span><span class="sxs-lookup"><span data-stu-id="79cfb-195">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="79cfb-196">Vyhledejte následující názvy knihoven jeden po druhém hello.</span><span class="sxs-lookup"><span data-stu-id="79cfb-196">Search for hello following library names one by one.</span></span> <span data-ttu-id="79cfb-197">Pro každou hello knihovně zjistíte, klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="79cfb-197">For each of hello library you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="79cfb-198">Nemáte skutečné senzor DHT22?</span><span class="sxs-lookup"><span data-stu-id="79cfb-198">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="79cfb-199">Hello ukázkovou aplikaci můžete simulovat teploty a vlhkosti dat v případě, že nemáte skutečné DHT22 senzoru.</span><span class="sxs-lookup"><span data-stu-id="79cfb-199">hello sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="79cfb-200">data pro toouse simulated tooenable hello ukázkové aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="79cfb-200">tooenable hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="79cfb-201">Otevřete hello `config.h` souboru v hello `app` složky.</span><span class="sxs-lookup"><span data-stu-id="79cfb-201">Open hello `config.h` file in hello `app` folder.</span></span>
1. <span data-ttu-id="79cfb-202">Vyhledejte následující řádek kódu hello a změňte hodnotu hello z `false` příliš`true`:</span><span class="sxs-lookup"><span data-stu-id="79cfb-202">Locate hello following line of code and change hello value from `false` too`true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurace hello ukázková aplikace toouse simulated data](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. <span data-ttu-id="79cfb-204">Uložit s `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="79cfb-204">Save with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toosparkfun-esp8266-thing-dev"></a><span data-ttu-id="79cfb-205">Nasazení hello ukázkové aplikace tooSparkfun ESP8266 věc vývojářů</span><span class="sxs-lookup"><span data-stu-id="79cfb-205">Deploy hello sample application tooSparkfun ESP8266 Thing Dev</span></span>

1. <span data-ttu-id="79cfb-206">V hello Arduino IDE, klikněte na **nástroj** > **Port**a potom klikněte na sériového portu hello za účelem co ESP8266 Sparkfun</span><span class="sxs-lookup"><span data-stu-id="79cfb-206">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Sparkfun ESP8266 Thing Dev.</span></span>
1. <span data-ttu-id="79cfb-207">Klikněte na tlačítko **nákresu** > **nahrát** toobuild a nasadit hello ukázkové aplikace tooSparkfun ESP8266 věc účelem</span><span class="sxs-lookup"><span data-stu-id="79cfb-207">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooSparkfun ESP8266 Thing Dev.</span></span>

> [!Note]
> <span data-ttu-id="79cfb-208">Pokud používáte systému macOS by mohli pravděpodobně zobrazit následující zprávy při odesílání hello.</span><span class="sxs-lookup"><span data-stu-id="79cfb-208">If you are using macOS you could probably see hello following messages during uploading.</span></span> <span data-ttu-id="79cfb-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span><span class="sxs-lookup"><span data-stu-id="79cfb-209">`warning: espcomm_sync failed`,`error: espcomm_open failed`.</span></span> <span data-ttu-id="79cfb-210">Otevřete okno vaší ternimal a dokončit následující akce toosolve tento problém.</span><span class="sxs-lookup"><span data-stu-id="79cfb-210">Please open your ternimal window and finish below actions toosolve this issue.</span></span>
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a><span data-ttu-id="79cfb-211">Zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="79cfb-211">Enter your credentials</span></span>

<span data-ttu-id="79cfb-212">Po úspěšném dokončení nahrávání hello postupujte podle kroků tooenter hello vaše přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="79cfb-212">After hello upload completes successfully, follow hello steps tooenter your credentials:</span></span>

1. <span data-ttu-id="79cfb-213">V hello Arduino IDE, klikněte na **nástroje** > **sériové monitorování**.</span><span class="sxs-lookup"><span data-stu-id="79cfb-213">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="79cfb-214">V okně hello sériové sledování Všimněte si, rozevírací seznamy hello dva na hello dolního rohu.</span><span class="sxs-lookup"><span data-stu-id="79cfb-214">In hello serial monitor window, notice hello two drop-down lists on hello bottom right corner.</span></span>
1. <span data-ttu-id="79cfb-215">Vyberte **žádný řádek ukončování** pro hello levém rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="79cfb-215">Select **No line ending** for hello left drop-down list.</span></span>
1. <span data-ttu-id="79cfb-216">Vyberte **115200 přenosová** pro hello právo rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="79cfb-216">Select **115200 baud** for hello right drop-down list.</span></span>
1. <span data-ttu-id="79cfb-217">Hello vstupní pole umístěné v hello horní části okna sériové sledování hello, zadejte následující informace, pokud se zobrazí výzva tooprovide hello a potom klikněte na **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="79cfb-217">In hello input box located at hello top of hello serial monitor window, enter hello following information if you are asked tooprovide them, and then click **Send**.</span></span>
   * <span data-ttu-id="79cfb-218">SSID sítě Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="79cfb-218">Wi-Fi SSID</span></span>
   * <span data-ttu-id="79cfb-219">Heslo Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="79cfb-219">Wi-Fi password</span></span>
   * <span data-ttu-id="79cfb-220">Řetězec připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="79cfb-220">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="79cfb-221">Hello pověření informace jsou uloženy v hello EEPROM z Sparkfun ESP8266 věc účelem</span><span class="sxs-lookup"><span data-stu-id="79cfb-221">hello credential information is stored in hello EEPROM of Sparkfun ESP8266 Thing Dev.</span></span> <span data-ttu-id="79cfb-222">Pokud klepnete na tlačítko Obnovit hello na hello Tabule Sparkfun ESP8266 věc vývojářů, hello ukázkovou aplikaci požádá, pokud chcete, aby tooerase hello informace.</span><span class="sxs-lookup"><span data-stu-id="79cfb-222">If you click hello reset button on hello Sparkfun ESP8266 Thing Dev board, hello sample application asks you if you want tooerase hello information.</span></span> <span data-ttu-id="79cfb-223">Zadejte `Y` toohave hello informace vymazat a jste vyzváni tooprovide hello informace znovu.</span><span class="sxs-lookup"><span data-stu-id="79cfb-223">Enter `Y` toohave hello information erased and you are asked tooprovide hello information again.</span></span>

### <a name="verify-hello-sample-application-is-running-successfully"></a><span data-ttu-id="79cfb-224">Zkontrolujte, zda hello ukázkové aplikace je úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="79cfb-224">Verify hello sample application is running successfully</span></span>

<span data-ttu-id="79cfb-225">Pokud se zobrazí následující hello výstup z okna hello sériové sledování a hello blikat DIODU na Sparkfun ESP8266 věc vývojářů, hello ukázkové aplikace je úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="79cfb-225">If you see hello following output from hello serial monitor window and hello blinking LED on Sparkfun ESP8266 Thing Dev, hello sample application is running successfully.</span></span>

![závěrečný výstup v integrovaném vývojovém prostředí arduino](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="79cfb-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79cfb-227">Next steps</span></span>

<span data-ttu-id="79cfb-228">Úspěšně jste připojené vývojářů věc ESP8266 Sparkfun tooyour IoT hub a odeslaných hello zachytit senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="79cfb-228">You have successfully connected a Sparkfun ESP8266 Thing Dev tooyour IoT hub and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
