---
title: "aaaESP8266 toocloud - připojení prolnutí HUZZAH ESP8266 tooAzure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte Adafruit prolnutí HUZZAH ESP8266 tooAzure IoT Hub pro něj toosend data toohello Azure Cloudová platforma v tomto kurzu."
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
ms.openlocfilehash: 44fd47232488948d21c7aa71bdd865397e41e63e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="18d79-103">Připojit Adafruit prolnutí HUZZAH ESP8266 tooAzure IoT Hub v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="18d79-103">Connect Adafruit Feather HUZZAH ESP8266 tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Připojení mezi DHT22, prolnutí HUZZAH ESP8266 a IoT Hub](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a><span data-ttu-id="18d79-105">Co dělat</span><span class="sxs-lookup"><span data-stu-id="18d79-105">What you do</span></span>


<span data-ttu-id="18d79-106">Připojte Adafruit prolnutí HUZZAH ESP8266 tooan IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="18d79-106">Connect Adafruit Feather HUZZAH ESP8266 tooan IoT hub that you create.</span></span> <span data-ttu-id="18d79-107">Pak spusťte ukázkovou aplikaci na ESP8266 toocollect hello teploty a vlhkosti data z DHT22 senzoru.</span><span class="sxs-lookup"><span data-stu-id="18d79-107">Then you run a sample application on ESP8266 toocollect hello temperature and humidity data from a DHT22 sensor.</span></span> <span data-ttu-id="18d79-108">Nakonec odeslat hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="18d79-108">Finally, you send hello sensor data tooyour IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="18d79-109">Pokud používáte jiné ESP8266 panely, můžete stále postupujte podle těchto kroků tooconnect ho tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="18d79-109">If you're using other ESP8266 boards, you can still follow these steps tooconnect it tooyour IoT hub.</span></span> <span data-ttu-id="18d79-110">V závislosti na panelu hello ESP8266 používáte, může být nutné tooreconfigure hello `LED_PIN`.</span><span class="sxs-lookup"><span data-stu-id="18d79-110">Depending on hello ESP8266 board you're using, you might need tooreconfigure hello `LED_PIN`.</span></span> <span data-ttu-id="18d79-111">Například pokud používáte ESP8266 z AI Thinker, můžete změnit z `0` příliš`2`.</span><span class="sxs-lookup"><span data-stu-id="18d79-111">For example, if you're using ESP8266 from AI-Thinker, you might change it from `0` too`2`.</span></span> <span data-ttu-id="18d79-112">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="18d79-112">Don't have a kit yet?</span></span> <span data-ttu-id="18d79-113">Získat z hello [webu Azure](http://azure.com/iotstarterkits).</span><span class="sxs-lookup"><span data-stu-id="18d79-113">Get it from hello [Azure website](http://azure.com/iotstarterkits).</span></span>




## <a name="what-you-learn"></a><span data-ttu-id="18d79-114">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="18d79-114">What you learn</span></span>

* <span data-ttu-id="18d79-115">Jak toocreate služby IoT hub a registrovat zařízení pro prolnutí HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="18d79-115">How toocreate an IoT hub and register a device for Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="18d79-116">Jak tooconnect ESP8266 HUZZAH prolnutí s hello senzor a počítač</span><span class="sxs-lookup"><span data-stu-id="18d79-116">How tooconnect Feather HUZZAH ESP8266 with hello sensor and your computer</span></span>
* <span data-ttu-id="18d79-117">Jak data snímačů toocollect spuštěním ukázkovou aplikaci na prolnutí HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="18d79-117">How toocollect sensor data by running a sample application on Feather HUZZAH ESP8266</span></span>
* <span data-ttu-id="18d79-118">Jak toosend hello senzor data tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="18d79-118">How toosend hello sensor data tooyour IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="18d79-119">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="18d79-119">What you need</span></span>

![součásti potřebné pro kurz hello](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

<span data-ttu-id="18d79-121">toocomplete tuto operaci je třeba hello následujících částí z vaší prolnutí HUZZAH ESP8266 Starter Kit:</span><span class="sxs-lookup"><span data-stu-id="18d79-121">toocomplete this operation, you need hello following parts from your Feather HUZZAH ESP8266 Starter Kit:</span></span>

* <span data-ttu-id="18d79-122">Hello prolnutí HUZZAH ESP8266 panelu</span><span class="sxs-lookup"><span data-stu-id="18d79-122">hello Feather HUZZAH ESP8266 board</span></span>
* <span data-ttu-id="18d79-123">Micro USB tooType kabelu A USB</span><span class="sxs-lookup"><span data-stu-id="18d79-123">A Micro USB tooType A USB cable</span></span>

<span data-ttu-id="18d79-124">Musíte taky hello následujících věcí pro vývojové prostředí:</span><span class="sxs-lookup"><span data-stu-id="18d79-124">You also need hello following things for your development environment:</span></span>

* <span data-ttu-id="18d79-125">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="18d79-125">An active Azure subscription.</span></span> <span data-ttu-id="18d79-126">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="18d79-126">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="18d79-127">Mac nebo počítači se systémem Windows nebo Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="18d79-127">Mac or PC that is running Windows or Ubuntu.</span></span>
* <span data-ttu-id="18d79-128">Bezdrátové sítě pro prolnutí HUZZAH ESP8266 tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="18d79-128">Wireless network for Feather HUZZAH ESP8266 tooconnect to.</span></span>
* <span data-ttu-id="18d79-129">Nástroj pro konfiguraci internetové připojení toodownload hello.</span><span class="sxs-lookup"><span data-stu-id="18d79-129">Internet connection toodownload hello configuration tool.</span></span>
* <span data-ttu-id="18d79-130">[Arduino IDE](https://www.arduino.cc/en/main/software) verze 1.6.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="18d79-130">[Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 or later.</span></span> <span data-ttu-id="18d79-131">Starší verze nepodporují s knihovnou AzureIoT hello.</span><span class="sxs-lookup"><span data-stu-id="18d79-131">Earlier versions don't work with hello AzureIoT library.</span></span>

<span data-ttu-id="18d79-132">Hello následující položky jsou volitelné v případě, že nemáte senzoru.</span><span class="sxs-lookup"><span data-stu-id="18d79-132">hello following items are optional in case you don’t have a sensor.</span></span> <span data-ttu-id="18d79-133">Máte také možnost hello použití dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="18d79-133">You also have hello option of using simulated sensor data.</span></span>

* <span data-ttu-id="18d79-134">Senzor teploty a vlhkosti Adafruit DHT22</span><span class="sxs-lookup"><span data-stu-id="18d79-134">An Adafruit DHT22 temperature and humidity sensor</span></span>
* <span data-ttu-id="18d79-135">Breadboard</span><span class="sxs-lookup"><span data-stu-id="18d79-135">A breadboard</span></span>
* <span data-ttu-id="18d79-136">Vodičům můstek M/M</span><span class="sxs-lookup"><span data-stu-id="18d79-136">M/M jumper wires</span></span>


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-hello-sensor-and-your-computer"></a><span data-ttu-id="18d79-137">Připojit prolnutí HUZZAH ESP8266 s hello senzor a počítače</span><span class="sxs-lookup"><span data-stu-id="18d79-137">Connect Feather HUZZAH ESP8266 with hello sensor and your computer</span></span>
<span data-ttu-id="18d79-138">V této části připojit hello senzorů tooyour panelu.</span><span class="sxs-lookup"><span data-stu-id="18d79-138">In this section, you connect hello sensors tooyour board.</span></span> <span data-ttu-id="18d79-139">Potom připojíte počítač tooyour zařízení pro další použití.</span><span class="sxs-lookup"><span data-stu-id="18d79-139">Then you plug in your device tooyour computer for further use.</span></span>
### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-huzzah-esp8266"></a><span data-ttu-id="18d79-140">Připojit DHT22 teploty a vlhkosti senzor tooFeather HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="18d79-140">Connect a DHT22 temperature and humidity sensor tooFeather HUZZAH ESP8266</span></span>

<span data-ttu-id="18d79-141">Použijte hello breadboard a můstek vodičům toomake hello připojení následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="18d79-141">Use hello breadboard and jumper wires toomake hello connection as follows.</span></span> <span data-ttu-id="18d79-142">Pokud nemáte senzoru, tuto část přeskočte, protože data snímačů simulované můžete použít místo.</span><span class="sxs-lookup"><span data-stu-id="18d79-142">If you don’t have a sensor, skip this section because you can use simulated sensor data instead.</span></span>

![odkaz na připojení](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


<span data-ttu-id="18d79-144">Senzor kód PIN použijte následující kabeláž hello:</span><span class="sxs-lookup"><span data-stu-id="18d79-144">For sensor pins, use hello following wiring:</span></span>


| <span data-ttu-id="18d79-145">Spuštění (senzor)</span><span class="sxs-lookup"><span data-stu-id="18d79-145">Start (Sensor)</span></span>           | <span data-ttu-id="18d79-146">End (panelu)</span><span class="sxs-lookup"><span data-stu-id="18d79-146">End (Board)</span></span>           | <span data-ttu-id="18d79-147">Kabel barev</span><span class="sxs-lookup"><span data-stu-id="18d79-147">Cable Color</span></span>   |
| -----------------------  | ---------------------- | ------------: |
| <span data-ttu-id="18d79-148">VDD (Pin 31F)</span><span class="sxs-lookup"><span data-stu-id="18d79-148">VDD (Pin 31F)</span></span>            | <span data-ttu-id="18d79-149">3v (připnout 58H)</span><span class="sxs-lookup"><span data-stu-id="18d79-149">3V (Pin 58H)</span></span>           | <span data-ttu-id="18d79-150">Red kabel</span><span class="sxs-lookup"><span data-stu-id="18d79-150">Red cable</span></span>     |
| <span data-ttu-id="18d79-151">DATA (Pin 32F)</span><span class="sxs-lookup"><span data-stu-id="18d79-151">DATA (Pin 32F)</span></span>           | <span data-ttu-id="18d79-152">GPIO 2 (Pin 46A)</span><span class="sxs-lookup"><span data-stu-id="18d79-152">GPIO 2 (Pin 46A)</span></span>       | <span data-ttu-id="18d79-153">Modrý kabel</span><span class="sxs-lookup"><span data-stu-id="18d79-153">Blue cable</span></span>    |
| <span data-ttu-id="18d79-154">ZEM (Pin 34F)</span><span class="sxs-lookup"><span data-stu-id="18d79-154">GND (Pin 34F)</span></span>            | <span data-ttu-id="18d79-155">ZEM (PIn 56I)</span><span class="sxs-lookup"><span data-stu-id="18d79-155">GND (PIn 56I)</span></span>          | <span data-ttu-id="18d79-156">Začernit kabel</span><span class="sxs-lookup"><span data-stu-id="18d79-156">Black cable</span></span>   |

<span data-ttu-id="18d79-157">Další informace najdete v tématu [Adafruit DHT22 senzor instalace](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) a [uspořádání Adafruit prolnutí HUZZAH Esp8266 kolíků](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span><span class="sxs-lookup"><span data-stu-id="18d79-157">For more information, see [Adafruit DHT22 sensor setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) and [Adafruit Feather HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).</span></span>



<span data-ttu-id="18d79-158">Teď vaše ESP8266 Huzzah prolnutí by měly být připojené s pracovní senzoru.</span><span class="sxs-lookup"><span data-stu-id="18d79-158">Now your Feather Huzzah ESP8266 should be connected with a working sensor.</span></span>

![Spojte se s prolnutí Huzzah DHT22](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-tooyour-computer"></a><span data-ttu-id="18d79-160">Připojte počítač tooyour prolnutí HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="18d79-160">Connect Feather HUZZAH ESP8266 tooyour computer</span></span>

<span data-ttu-id="18d79-161">Jak ukazuje následující, použijte hello Micro USB tooType A USB kabel tooconnect prolnutí HUZZAH ESP8266 tooyour počítač.</span><span class="sxs-lookup"><span data-stu-id="18d79-161">As shown next, use hello Micro USB tooType A USB cable tooconnect Feather HUZZAH ESP8266 tooyour computer.</span></span>

![Připojte počítač tooyour prolnutí Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a><span data-ttu-id="18d79-163">Přidání oprávnění sériového portu (pouze Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="18d79-163">Add serial port permissions (Ubuntu only)</span></span>


<span data-ttu-id="18d79-164">Pokud používáte Ubuntu, ujistěte se, že máte oprávnění toooperate hello na hello USB port z prolnutí HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="18d79-164">If you use Ubuntu, make sure you have hello permissions toooperate on hello USB port of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="18d79-165">oprávnění tooadd sériového portu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="18d79-165">tooadd serial port permissions, follow these steps:</span></span>


1. <span data-ttu-id="18d79-166">Spusťte následující příkazy v terminálu hello:</span><span class="sxs-lookup"><span data-stu-id="18d79-166">Run hello following commands at a terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="18d79-167">Zobrazí jednu z následujících výstupy hello:</span><span class="sxs-lookup"><span data-stu-id="18d79-167">You get one of hello following outputs:</span></span>

   * <span data-ttu-id="18d79-168">CRW-rw---1 kořenové uucp xxxxxxxx</span><span class="sxs-lookup"><span data-stu-id="18d79-168">crw-rw---- 1 root uucp xxxxxxxx</span></span>
   * <span data-ttu-id="18d79-169">CRW-rw---xxxxxxxx síti službou 1 root</span><span class="sxs-lookup"><span data-stu-id="18d79-169">crw-rw---- 1 root dialout xxxxxxxx</span></span>

   <span data-ttu-id="18d79-170">Ve výstupu hello, Všimněte si, že `uucp` nebo `dialout` je název vlastníka skupiny hello hello portu USB.</span><span class="sxs-lookup"><span data-stu-id="18d79-170">In hello output, notice that `uucp` or `dialout` is hello group owner name of hello USB port.</span></span>

1. <span data-ttu-id="18d79-171">Přidáte skupinu toohello uživatelů hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="18d79-171">Add hello user toohello group by running hello following command:</span></span>

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   <span data-ttu-id="18d79-172">`<group-owner-name>`je název skupiny vlastníka hello, kterou jste získali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="18d79-172">`<group-owner-name>` is hello group owner name you obtained in hello previous step.</span></span> <span data-ttu-id="18d79-173">`<username>`je Ubuntu uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="18d79-173">`<username>` is your Ubuntu user name.</span></span>

1. <span data-ttu-id="18d79-174">Odhlaste se od Ubuntu a znovu se přihlaste pro změnu tooappear hello.</span><span class="sxs-lookup"><span data-stu-id="18d79-174">Sign out of Ubuntu, and then sign in again for hello change tooappear.</span></span>

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a><span data-ttu-id="18d79-175">Shromažďování dat snímačů a odeslat tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="18d79-175">Collect sensor data and send it tooyour IoT hub</span></span>

<span data-ttu-id="18d79-176">V této části nasazení a spuštění ukázkové aplikace na prolnutí HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="18d79-176">In this section, you deploy and run a sample application on Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="18d79-177">ukázkové aplikace Hello bliká hello DIODU na prolnutí HUZZAH ESP8266 a odešle hello teploty a vlhkosti data shromážděná z hello DHT22 senzor tooyour služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="18d79-177">hello sample application blinks hello LED on Feather HUZZAH ESP8266, and sends hello temperature and humidity data collected from hello DHT22 sensor tooyour IoT hub.</span></span>

### <a name="get-hello-sample-application-from-github"></a><span data-ttu-id="18d79-178">Získat hello ukázkovou aplikaci z webu GitHub</span><span class="sxs-lookup"><span data-stu-id="18d79-178">Get hello sample application from GitHub</span></span>

<span data-ttu-id="18d79-179">Ukázková aplikace Hello jsou hostované na Githubu.</span><span class="sxs-lookup"><span data-stu-id="18d79-179">hello sample application is hosted on GitHub.</span></span> <span data-ttu-id="18d79-180">Klonování úložiště hello ukázka, která obsahuje ukázkovou aplikaci hello z Githubu.</span><span class="sxs-lookup"><span data-stu-id="18d79-180">Clone hello sample repository that contains hello sample application from GitHub.</span></span> <span data-ttu-id="18d79-181">tooclone hello Ukázka úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="18d79-181">tooclone hello sample repository, follow these steps:</span></span>

1. <span data-ttu-id="18d79-182">Otevřete příkazový řádek nebo okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="18d79-182">Open a command prompt or a terminal window.</span></span>
1. <span data-ttu-id="18d79-183">Přejděte tooa složku, kam chcete hello ukázkové aplikace toobe uložené.</span><span class="sxs-lookup"><span data-stu-id="18d79-183">Go tooa folder where you want hello sample application toobe stored.</span></span>
1. <span data-ttu-id="18d79-184">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="18d79-184">Run hello following command:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

<span data-ttu-id="18d79-185">Instalujte balíček hello pro prolnutí HUZZAH ESP8266 v hello Arduino IDE:</span><span class="sxs-lookup"><span data-stu-id="18d79-185">Install hello package for Feather HUZZAH ESP8266 in hello Arduino IDE:</span></span>

1. <span data-ttu-id="18d79-186">Otevřete složku hello se uloží hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="18d79-186">Open hello folder where hello sample application is stored.</span></span>
1. <span data-ttu-id="18d79-187">Otevřete soubor app.ino hello ve složce aplikace hello v hello Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="18d79-187">Open hello app.ino file in hello app folder in hello Arduino IDE.</span></span>

   ![Otevřete hello ukázkovou aplikaci v Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. <span data-ttu-id="18d79-189">V hello Arduino IDE, klikněte na **soubor** > **Předvolby**.</span><span class="sxs-lookup"><span data-stu-id="18d79-189">In hello Arduino IDE, click **File** > **Preferences**.</span></span>
1. <span data-ttu-id="18d79-190">V hello **Předvolby** dialogovém okně klikněte na další toohello ikonu hello **další adresy URL Manager panely** pole.</span><span class="sxs-lookup"><span data-stu-id="18d79-190">In hello **Preferences** dialog box, click hello icon next toohello **Additional Boards Manager URLs** box.</span></span>
1. <span data-ttu-id="18d79-191">V místním okně hello, zadejte následující adresu URL hello a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="18d79-191">In hello pop-up window, enter hello following URL, and then click **OK**.</span></span>

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Adresa url balíčku bodu tooa v Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. <span data-ttu-id="18d79-193">V hello **předvoleb** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="18d79-193">In hello **Preference** dialog box, click **OK**.</span></span>
1. <span data-ttu-id="18d79-194">Klikněte na tlačítko **nástroje** > **Tabule** > **Manager panely**a poté vyhledejte esp8266.</span><span class="sxs-lookup"><span data-stu-id="18d79-194">Click **Tools** > **Board** > **Boards Manager**, and then search for esp8266.</span></span>

   <span data-ttu-id="18d79-195">Panely Manager znamená, že je nainstalována ESP8266 verzí 2.2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="18d79-195">Boards Manager indicates that ESP8266 with a version of 2.2.0 or later is installed.</span></span>

   ![je nainstalovaný balíček esp8266 Hello](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. <span data-ttu-id="18d79-197">Klikněte na tlačítko **nástroje** > **Tabule** > **Adafruit HUZZAH ESP8266**.</span><span class="sxs-lookup"><span data-stu-id="18d79-197">Click **Tools** > **Board** > **Adafruit HUZZAH ESP8266**.</span></span>

### <a name="install-necessary-libraries"></a><span data-ttu-id="18d79-198">Instalace nezbytné knihovny</span><span class="sxs-lookup"><span data-stu-id="18d79-198">Install necessary libraries</span></span>

1. <span data-ttu-id="18d79-199">V hello Arduino IDE, klikněte na **nákresu** > **zahrnují knihovny** > **spravovat knihovny**.</span><span class="sxs-lookup"><span data-stu-id="18d79-199">In hello Arduino IDE, click **Sketch** > **Include Library** > **Manage Libraries**.</span></span>
1. <span data-ttu-id="18d79-200">Vyhledejte následující názvy knihoven jeden po druhém hello.</span><span class="sxs-lookup"><span data-stu-id="18d79-200">Search for hello following library names one by one.</span></span> <span data-ttu-id="18d79-201">Pro každou knihovnu, která zjistíte, klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="18d79-201">For each  library that you find, click **Install**.</span></span>
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a><span data-ttu-id="18d79-202">Nemáte skutečné senzor DHT22?</span><span class="sxs-lookup"><span data-stu-id="18d79-202">Don’t have a real DHT22 sensor?</span></span>

<span data-ttu-id="18d79-203">Hello ukázkovou aplikaci můžete simulovat teploty a vlhkosti dat v případě, že nemáte skutečné DHT22 senzoru.</span><span class="sxs-lookup"><span data-stu-id="18d79-203">hello sample application can simulate temperature and humidity data in case you don’t have a real DHT22 sensor.</span></span> <span data-ttu-id="18d79-204">tooset hello ukázkové aplikace toouse simulated dat, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="18d79-204">tooset up hello sample application toouse simulated data, follow these steps:</span></span>

1. <span data-ttu-id="18d79-205">Otevřete hello `config.h` souboru v hello `app` složky.</span><span class="sxs-lookup"><span data-stu-id="18d79-205">Open hello `config.h` file in hello `app` folder.</span></span>
1. <span data-ttu-id="18d79-206">Vyhledejte následující řádek kódu hello a změňte hodnotu hello z `false` příliš`true`:</span><span class="sxs-lookup"><span data-stu-id="18d79-206">Locate hello following line of code and change hello value from `false` too`true`:</span></span>
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurace hello ukázková aplikace toouse simulated data](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. <span data-ttu-id="18d79-208">Uložte soubor hello s `Control-s`.</span><span class="sxs-lookup"><span data-stu-id="18d79-208">Save hello file with `Control-s`.</span></span>

### <a name="deploy-hello-sample-application-toofeather-huzzah-esp8266"></a><span data-ttu-id="18d79-209">Nasazení hello ukázkové aplikace tooFeather HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="18d79-209">Deploy hello sample application tooFeather HUZZAH ESP8266</span></span>

1. <span data-ttu-id="18d79-210">V hello Arduino IDE, klikněte na **nástroj** > **Port**a potom klikněte na hello sériového portu pro prolnutí HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="18d79-210">In hello Arduino IDE, click **Tool** > **Port**, and then click hello serial port for Feather HUZZAH ESP8266.</span></span>
1. <span data-ttu-id="18d79-211">Klikněte na tlačítko **nákresu** > **nahrát** toobuild a nasadit hello ukázkové aplikace tooFeather HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="18d79-211">Click **Sketch** > **Upload** toobuild and deploy hello sample application tooFeather HUZZAH ESP8266.</span></span>

### <a name="enter-your-credentials"></a><span data-ttu-id="18d79-212">Zadejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="18d79-212">Enter your credentials</span></span>

<span data-ttu-id="18d79-213">Po úspěšném dokončení nahrávání hello postupujte podle těchto kroků tooenter vaše přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="18d79-213">After hello upload completes successfully, follow these steps tooenter your credentials:</span></span>

1. <span data-ttu-id="18d79-214">V hello Arduino IDE, klikněte na **nástroje** > **sériové monitorování**.</span><span class="sxs-lookup"><span data-stu-id="18d79-214">In hello Arduino IDE, click **Tools** > **Serial Monitor**.</span></span>
1. <span data-ttu-id="18d79-215">V okně hello sériové sledování Všimněte si, rozevírací seznamy hello dva v pravém dolním rohu, hello.</span><span class="sxs-lookup"><span data-stu-id="18d79-215">In hello serial monitor window, notice hello two drop-down lists in hello lower-right corner.</span></span>
1. <span data-ttu-id="18d79-216">Vyberte **žádný řádek ukončování** pro hello levém rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="18d79-216">Select **No line ending** for hello left drop-down list.</span></span>
1. <span data-ttu-id="18d79-217">Vyberte **115200 přenosová** pro hello právo rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="18d79-217">Select **115200 baud** for hello right drop-down list.</span></span>
1. <span data-ttu-id="18d79-218">Hello vstupní pole umístěné v hello horní části okna sériové sledování hello, zadejte následující informace, pokud se zobrazí výzva tooprovide hello a potom klikněte na **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="18d79-218">In hello input box located at hello top of hello serial monitor window, enter hello following information if you are asked tooprovide them, and then click **Send**.</span></span>
   * <span data-ttu-id="18d79-219">SSID sítě Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="18d79-219">Wi-Fi SSID</span></span>
   * <span data-ttu-id="18d79-220">Heslo Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="18d79-220">Wi-Fi password</span></span>
   * <span data-ttu-id="18d79-221">Řetězec připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="18d79-221">Device connection string</span></span>

> [!Note]
> <span data-ttu-id="18d79-222">Hello pověření informace jsou uloženy v hello EEPROM prolnutí HUZZAH ESP8266.</span><span class="sxs-lookup"><span data-stu-id="18d79-222">hello credential information is stored in hello EEPROM of Feather HUZZAH ESP8266.</span></span> <span data-ttu-id="18d79-223">Pokud kliknutí na tlačítko resetovat hello na hello prolnutí HUZZAH ESP8266 panelu ukázkové aplikace hello dotáže se tooerase hello informace.</span><span class="sxs-lookup"><span data-stu-id="18d79-223">If you click hello reset button on hello Feather HUZZAH ESP8266 board, hello sample application asks if you want tooerase hello information.</span></span> <span data-ttu-id="18d79-224">Zadejte `Y` toohave hello informace vymazat.</span><span class="sxs-lookup"><span data-stu-id="18d79-224">Enter `Y` toohave hello information erased.</span></span> <span data-ttu-id="18d79-225">Zobrazí se výzva tooprovide hello informace ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="18d79-225">You are asked tooprovide hello information a second time.</span></span>

### <a name="verify-hello-sample-application-is-running-successfully"></a><span data-ttu-id="18d79-226">Zkontrolujte, zda hello ukázkové aplikace je úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="18d79-226">Verify hello sample application is running successfully</span></span>

<span data-ttu-id="18d79-227">Pokud se zobrazí následující hello výstup z okna hello sériové sledování a hello blikat DIODU na prolnutí HUZZAH ESP8266, hello ukázkové aplikace je úspěšně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="18d79-227">If you see hello following output from hello serial monitor window and hello blinking LED on Feather HUZZAH ESP8266, hello sample application is running successfully.</span></span>

![Závěrečný výstup v integrovaném vývojovém prostředí Arduino](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a><span data-ttu-id="18d79-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18d79-229">Next steps</span></span>

<span data-ttu-id="18d79-230">Máte úspěšně připojen prolnutí HUZZAH ESP8266 tooyour IoT hub a odeslat hello zachytit senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="18d79-230">You have successfully connected a Feather HUZZAH ESP8266 tooyour IoT hub, and sent hello captured sensor data tooyour IoT hub.</span></span> 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

