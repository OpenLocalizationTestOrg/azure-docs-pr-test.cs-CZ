---
title: "Intel Edison do cloudu (C) - Edison Intel připojit ke službě Azure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak nastavit a Intel Edison připojit ke službě Azure IoT Hub pro Intel Edison k odesílání dat do Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot intel edison, intel edison iot hub, intel edison odesílat data do cloudu, intel edison do cloudu"
ms.assetid: 4885fa2c-c2ee-4253-b37f-ccd55f92b006
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/17/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: edbdbe0230f742cd7228f04a4a83c9bd567527e8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-intel-edison-to-azure-iot-hub-c"></a><span data-ttu-id="2fddd-104">Připojení k Azure IoT Hub (C) Intel Edison</span><span class="sxs-lookup"><span data-stu-id="2fddd-104">Connect Intel Edison to Azure IoT Hub (C)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="2fddd-105">V tomto kurzu zahájíte učení základní informace o práci s Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="2fddd-105">In this tutorial, you begin by learning the basics of working with Intel Edison.</span></span> <span data-ttu-id="2fddd-106">Pak zjistíte, jak bezproblémově připojení zařízení do cloudu pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="2fddd-106">You then learn how to seamlessly connect your devices to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

<span data-ttu-id="2fddd-107">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="2fddd-107">Don't have a kit yet?</span></span> <span data-ttu-id="2fddd-108">Spustit [sem](https://azure.microsoft.com/develop/iot/starter-kits)</span><span class="sxs-lookup"><span data-stu-id="2fddd-108">Start [here](https://azure.microsoft.com/develop/iot/starter-kits)</span></span>

## <a name="what-you-do"></a><span data-ttu-id="2fddd-109">Co dělat</span><span class="sxs-lookup"><span data-stu-id="2fddd-109">What you do</span></span>

* <span data-ttu-id="2fddd-110">Instalační program Intel Edison a a Groove moduly.</span><span class="sxs-lookup"><span data-stu-id="2fddd-110">Setup Intel Edison and and Grove modules.</span></span>
* <span data-ttu-id="2fddd-111">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2fddd-111">Create an IoT hub.</span></span>
* <span data-ttu-id="2fddd-112">Registrovat zařízení pro Edison ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2fddd-112">Register a device for Edison in your IoT hub.</span></span>
* <span data-ttu-id="2fddd-113">Spuštění ukázkové aplikace na Edison k odesílání dat snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2fddd-113">Run a sample application on Edison to send sensor data to your IoT hub.</span></span>

<span data-ttu-id="2fddd-114">Intel Edison připojte ke službě IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="2fddd-114">Connect Intel Edison to an IoT hub that you create.</span></span> <span data-ttu-id="2fddd-115">Pak spusťte ukázkovou aplikaci na Edison ke shromažďování dat teploty a vlhkosti z teplotní snímač Groove.</span><span class="sxs-lookup"><span data-stu-id="2fddd-115">Then you run a sample application on Edison to collect temperature and humidity data from a Grove temperature sensor.</span></span> <span data-ttu-id="2fddd-116">Nakonec odeslat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2fddd-116">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="2fddd-117">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="2fddd-117">What you learn</span></span>

* <span data-ttu-id="2fddd-118">Postup vytvoření služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="2fddd-118">How to create an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="2fddd-119">Postup připojení Edison s teplotní snímač Groove.</span><span class="sxs-lookup"><span data-stu-id="2fddd-119">How to connect Edison with a Grove temperature sensor.</span></span>
* <span data-ttu-id="2fddd-120">Postup shromažďování dat snímačů spuštěním ukázkovou aplikaci na Edison.</span><span class="sxs-lookup"><span data-stu-id="2fddd-120">How to collect sensor data by running a sample application on Edison.</span></span>
* <span data-ttu-id="2fddd-121">Jak odesílat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2fddd-121">How to send sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2fddd-122">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="2fddd-122">What you need</span></span>

![Co potřebujete](media/iot-hub-intel-edison-kit-c-get-started/0_kit.png)

* <span data-ttu-id="2fddd-124">Panel Intel Edison</span><span class="sxs-lookup"><span data-stu-id="2fddd-124">The Intel Edison board</span></span>
* <span data-ttu-id="2fddd-125">Panel Arduino rozšíření</span><span class="sxs-lookup"><span data-stu-id="2fddd-125">Arduino expansion board</span></span>
* <span data-ttu-id="2fddd-126">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2fddd-126">An active Azure subscription.</span></span> <span data-ttu-id="2fddd-127">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="2fddd-127">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="2fddd-128">Mac nebo počítači se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="2fddd-128">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="2fddd-129">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="2fddd-129">An Internet connection.</span></span>
* <span data-ttu-id="2fddd-130">Micro B kabelem USB typ A</span><span class="sxs-lookup"><span data-stu-id="2fddd-130">A Micro B to Type A USB cable</span></span>
* <span data-ttu-id="2fddd-131">Zdroj napájení přímo aktuální (DC).</span><span class="sxs-lookup"><span data-stu-id="2fddd-131">A direct current (DC) power supply.</span></span> <span data-ttu-id="2fddd-132">Zdroj napájení by měl být hodnocení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2fddd-132">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="2fddd-133">ŘADIČ DOMÉNY 7 15V</span><span class="sxs-lookup"><span data-stu-id="2fddd-133">7-15V DC</span></span>
  - <span data-ttu-id="2fddd-134">Alespoň 1500mA</span><span class="sxs-lookup"><span data-stu-id="2fddd-134">At least 1500mA</span></span>
  - <span data-ttu-id="2fddd-135">PIN kód center nebo vnitřní by měla být kladná pólu napájení</span><span class="sxs-lookup"><span data-stu-id="2fddd-135">The center/inner pin should be the positive pole of the power supply</span></span>

<span data-ttu-id="2fddd-136">Následující položky jsou volitelné:</span><span class="sxs-lookup"><span data-stu-id="2fddd-136">The following items are optional:</span></span>

* <span data-ttu-id="2fddd-137">Groove základní štítu V2</span><span class="sxs-lookup"><span data-stu-id="2fddd-137">Grove Base Shield V2</span></span>
* <span data-ttu-id="2fddd-138">Groove - teplotní snímač</span><span class="sxs-lookup"><span data-stu-id="2fddd-138">Grove - Temperature Sensor</span></span>
* <span data-ttu-id="2fddd-139">Kabel Groove</span><span class="sxs-lookup"><span data-stu-id="2fddd-139">Grove Cable</span></span>
* <span data-ttu-id="2fddd-140">Jako mezeru mezi řádky nebo šrouby, zahrnout v balení, včetně dvou šrouby připevnit modulu Tabule rozšíření a čtyři sady šrouby a plastové vymezovače.</span><span class="sxs-lookup"><span data-stu-id="2fddd-140">Any spacer bars or screws included in the packaging, including two screws to fasten the module to the expansion board and four sets of screws and plastic spacers.</span></span>

> [!NOTE] 
<span data-ttu-id="2fddd-141">Tyto položky jsou volitelné, protože data snímačů simulated podporu ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="2fddd-141">These items are optional because the code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a><span data-ttu-id="2fddd-142">Instalační program Intel Edison</span><span class="sxs-lookup"><span data-stu-id="2fddd-142">Setup Intel Edison</span></span>

### <a name="assemble-your-board"></a><span data-ttu-id="2fddd-143">Sestavte panel</span><span class="sxs-lookup"><span data-stu-id="2fddd-143">Assemble your board</span></span>

<span data-ttu-id="2fddd-144">Tato část obsahuje kroky pro připojení k vaší karty rozšíření modulu Intel® Edison.</span><span class="sxs-lookup"><span data-stu-id="2fddd-144">This section contains steps to attach your Intel® Edison module to your expansion board.</span></span>

1. <span data-ttu-id="2fddd-145">Místní modul Intel® Edison v rámci bílé osnovy na vaší kartě rozšíření zarovnání díry na modul s šrouby na kartě rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2fddd-145">Place the Intel® Edison module within the white outline on your expansion board, lining up the holes on the module with the screws on the expansion board.</span></span>

2. <span data-ttu-id="2fddd-146">Klepnout na modul pod slova `What will you make?` dokud si myslíte, že během.</span><span class="sxs-lookup"><span data-stu-id="2fddd-146">Press down on the module just below the words `What will you make?` until you feel a snap.</span></span>

   ![Sestavte Tabule 2](media/iot-hub-intel-edison-kit-c-get-started/1_assemble_board2.jpg)

3. <span data-ttu-id="2fddd-148">Dva šestnáctkových matice (součástí balíčku) použijte k zabezpečení modulu Tabule rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2fddd-148">Use the two hex nuts (included in the package) to secure the module to the expansion board.</span></span>

   ![Sestavte Tabule 3](media/iot-hub-intel-edison-kit-c-get-started/2_assemble_board3.jpg)

4. <span data-ttu-id="2fddd-150">Vložte šroubu v jednom z díry čtyři rohu na kartě rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2fddd-150">Insert a screw in one of the four corner holes on the expansion board.</span></span> <span data-ttu-id="2fddd-151">Otočením a posílit mezi bílé plastové nosníků do šroubek.</span><span class="sxs-lookup"><span data-stu-id="2fddd-151">Twist and tighten one of the white plastic spacers onto the screw.</span></span>

   ![Sestavte Tabule 4](media/iot-hub-intel-edison-kit-c-get-started/3_assemble_board4.jpg)

5. <span data-ttu-id="2fddd-153">Pro další tři rohu nosníků opakujte.</span><span class="sxs-lookup"><span data-stu-id="2fddd-153">Repeat for the other three corner spacers.</span></span>

   ![Sestavte Tabule 5](media/iot-hub-intel-edison-kit-c-get-started/4_assemble_board5.jpg)

<span data-ttu-id="2fddd-155">Nyní je několik panel.</span><span class="sxs-lookup"><span data-stu-id="2fddd-155">Now your board is assembled.</span></span>

   ![sestavený panelu](media/iot-hub-intel-edison-kit-c-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a><span data-ttu-id="2fddd-157">Připojit štítu základní Groove a teplotní snímač.</span><span class="sxs-lookup"><span data-stu-id="2fddd-157">Connect the Grove Base Shield and the temperature sensor</span></span>

1. <span data-ttu-id="2fddd-158">Místní štítu základní Groove k vaší karty.</span><span class="sxs-lookup"><span data-stu-id="2fddd-158">Place the Grove Base Shield on to your board.</span></span> <span data-ttu-id="2fddd-159">Ujistěte se, že všechny kódy PIN jsou úzce připojovány panel.</span><span class="sxs-lookup"><span data-stu-id="2fddd-159">Make sure all pins are tightly plugged into your board.</span></span>
   
   ![Základní štítu Groove](media/iot-hub-intel-edison-kit-c-get-started/6_grove_base_sheild.jpg)

2. <span data-ttu-id="2fddd-161">Použít pro připojení Groove teplotní snímač do štítu základní Groove Groove kabel **A0** portu.</span><span class="sxs-lookup"><span data-stu-id="2fddd-161">Use Grove Cable to connect Grove temperature sensor onto the Grove Base Shield **A0** port.</span></span>

   ![Připojení k teplotní snímač](media/iot-hub-intel-edison-kit-c-get-started/7_temperature_sensor.jpg)
   
   ![Edison a senzor připojení](media/iot-hub-intel-edison-kit-c-get-started/16_edion_sensor.png)

<span data-ttu-id="2fddd-164">Vaše senzor je nyní připraven.</span><span class="sxs-lookup"><span data-stu-id="2fddd-164">Now your sensor is ready.</span></span>

### <a name="power-up-edison"></a><span data-ttu-id="2fddd-165">Napájení až Edison</span><span class="sxs-lookup"><span data-stu-id="2fddd-165">Power up Edison</span></span>

1. <span data-ttu-id="2fddd-166">Zařaďte napájení.</span><span class="sxs-lookup"><span data-stu-id="2fddd-166">Plug in the power supply.</span></span>

   ![Zařadit napájení](media/iot-hub-intel-edison-kit-c-get-started/8_plug_power.jpg)

2. <span data-ttu-id="2fddd-168">Zelená DIODU (s názvem bez přípony DS1 na panelu rozšíření Arduino *), by měla zůstat po a light.</span><span class="sxs-lookup"><span data-stu-id="2fddd-168">A green LED(labeled DS1 on the Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="2fddd-169">Počkejte, než jedna minuta pro panel na dokončení spuštění.</span><span class="sxs-lookup"><span data-stu-id="2fddd-169">Wait one minute for the board to finish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2fddd-170">Pokud nemáte ke zdroji napájení řadiče domény, může stále spotřeby Tabule přes USB port.</span><span class="sxs-lookup"><span data-stu-id="2fddd-170">If you do not have a DC power supply, you can still power the board through a USB port.</span></span> <span data-ttu-id="2fddd-171">V tématu `Connect Edison to your computer` podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2fddd-171">See `Connect Edison to your computer` section for details.</span></span> <span data-ttu-id="2fddd-172">Pohánějící panel tímto způsobem může způsobit nepředvídatelné chování z vaší karty, zejména v případě, že pomocí sítě Wi-Fi nebo řídí motory.</span><span class="sxs-lookup"><span data-stu-id="2fddd-172">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

### <a name="connect-edison-to-your-computer"></a><span data-ttu-id="2fddd-173">Připojte k počítači Edison</span><span class="sxs-lookup"><span data-stu-id="2fddd-173">Connect Edison to your computer</span></span>

1. <span data-ttu-id="2fddd-174">Přepněte dolů mikrospínače směrem dva malých USB porty, takže Edison je v režimu zařízení.</span><span class="sxs-lookup"><span data-stu-id="2fddd-174">Toggle down the microswitch towards the two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="2fddd-175">Rozdíly mezi zařízení režim a režim hostitele, prosím odkazovat [zde](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="2fddd-175">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Přepněte dolů mikrospínače](media/iot-hub-intel-edison-kit-c-get-started/9_toggle_down_microswitch.jpg)

2. <span data-ttu-id="2fddd-177">Připojte kabel USB malých nejvyšší malých portu USB.</span><span class="sxs-lookup"><span data-stu-id="2fddd-177">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Horní malých portu USB](media/iot-hub-intel-edison-kit-c-get-started/10_top_usbport.jpg)

3. <span data-ttu-id="2fddd-179">Druhém konci kabelu USB připojte k počítači.</span><span class="sxs-lookup"><span data-stu-id="2fddd-179">Plug the other end of USB cable into your computer.</span></span>

   ![Počítač USB](media/iot-hub-intel-edison-kit-c-get-started/11_computer_usb.jpg)

4. <span data-ttu-id="2fddd-181">Budete vědět, že panel úplné inicializace když se počítač připojí na nový disk (podobně jako vkládání SD karty do počítače).</span><span class="sxs-lookup"><span data-stu-id="2fddd-181">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-the-configuration-tool"></a><span data-ttu-id="2fddd-182">Stáhněte a spusťte nástroj Konfigurace</span><span class="sxs-lookup"><span data-stu-id="2fddd-182">Download and run the configuration tool</span></span>
<span data-ttu-id="2fddd-183">Získat nejnovější nástroje Konfigurace z [tento odkaz](https://software.intel.com/en-us/iot/hardware/edison/downloads) uvedené v části `Installers` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="2fddd-183">Get the latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under the `Installers` heading.</span></span> <span data-ttu-id="2fddd-184">Spusťte nástroj a postupujte podle jeho na obrazovce pokyny, kde je potřeba klepnutím na tlačítko Další</span><span class="sxs-lookup"><span data-stu-id="2fddd-184">Execute the tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="2fddd-185">Flash firmwaru</span><span class="sxs-lookup"><span data-stu-id="2fddd-185">Flash firmware</span></span>
1. <span data-ttu-id="2fddd-186">Na `Set up options` klikněte na tlačítko `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="2fddd-186">On the `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="2fddd-187">Vyberte bitovou kopii na flash na panel jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2fddd-187">Select the image to flash onto your board by doing one of the following:</span></span>
   - <span data-ttu-id="2fddd-188">Chcete-li stáhnout a flash panel s nejnovější bitové kopie firmwaru, které jsou k dispozici z Intel, vyberte `Download the latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="2fddd-188">To download and flash your board with the latest firmware image available from Intel, select `Download the latest image version xxxx`.</span></span>
   - <span data-ttu-id="2fddd-189">Chcete-li flash panel s bitovou kopií již uložení v počítači, vyberte `Select the local image`.</span><span class="sxs-lookup"><span data-stu-id="2fddd-189">To flash your board with an image you already have saved on your computer, select `Select the local image`.</span></span> <span data-ttu-id="2fddd-190">Vyhledejte a vyberte bitovou kopii, kterou chcete flash pro panel.</span><span class="sxs-lookup"><span data-stu-id="2fddd-190">Browse to and select the image you want to flash to your board.</span></span>
3. <span data-ttu-id="2fddd-191">Nástroj instalační program se pokusí o flash panel.</span><span class="sxs-lookup"><span data-stu-id="2fddd-191">The setup tool will attempt to flash your board.</span></span> <span data-ttu-id="2fddd-192">Celý proces blikající může trvat až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="2fddd-192">The entire flashing process may take up to 10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="2fddd-193">Nastavit heslo</span><span class="sxs-lookup"><span data-stu-id="2fddd-193">Set password</span></span>
1. <span data-ttu-id="2fddd-194">Na `Set up options` klikněte na tlačítko `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="2fddd-194">On the `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="2fddd-195">Můžete nastavit vlastní název pro panel Intel® Edison.</span><span class="sxs-lookup"><span data-stu-id="2fddd-195">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="2fddd-196">Tato položka je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="2fddd-196">This is optional.</span></span>
3. <span data-ttu-id="2fddd-197">Zadejte heslo pro panel a pak klikněte na `Set password`.</span><span class="sxs-lookup"><span data-stu-id="2fddd-197">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="2fddd-198">Známku výpadku heslo, které se později používá.</span><span class="sxs-lookup"><span data-stu-id="2fddd-198">Mark down the password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="2fddd-199">Připojení Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="2fddd-199">Connect Wi-Fi</span></span>
1. <span data-ttu-id="2fddd-200">Na `Set up options` klikněte na tlačítko `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="2fddd-200">On the `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="2fddd-201">Počkejte, až na jednu minutu jako počítač hledá dostupných sítí Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="2fddd-201">Wait up to one minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="2fddd-202">Z `Detected Networks` rozevíracího seznamu vyberte vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="2fddd-202">From the `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="2fddd-203">Z `Security` rozevíracího seznamu vyberte typ zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="2fddd-203">From the `Security` drop-down list, select the network's security type.</span></span>
4. <span data-ttu-id="2fddd-204">Poskytnout vaše údaje přihlašovací jméno a heslo a pak klikněte na `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="2fddd-204">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="2fddd-205">Známku výpadku IP adresy, která se později používá.</span><span class="sxs-lookup"><span data-stu-id="2fddd-205">Mark down the IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="2fddd-206">Ujistěte se, že Edison je připojený ke stejné síti jako počítače.</span><span class="sxs-lookup"><span data-stu-id="2fddd-206">Make sure that Edison is connected to the same network as your computer.</span></span> <span data-ttu-id="2fddd-207">Váš počítač připojí k vaší Edison pomocí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="2fddd-207">Your computer connects to your Edison by using the IP address.</span></span>

   ![Připojení k teplotní snímač](media/iot-hub-intel-edison-kit-c-get-started/12_configuration_tool.png)

<span data-ttu-id="2fddd-209">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2fddd-209">Congratulations!</span></span> <span data-ttu-id="2fddd-210">Úspěšně jste nakonfigurovali Edison.</span><span class="sxs-lookup"><span data-stu-id="2fddd-210">You've successfully configured Edison.</span></span>

## <a name="run-a-sample-application-on-intel-edison"></a><span data-ttu-id="2fddd-211">Spuštění ukázkové aplikace na Intel Edison</span><span class="sxs-lookup"><span data-stu-id="2fddd-211">Run a sample application on Intel Edison</span></span>

### <a name="prepare-the-azure-iot-device-sdk"></a><span data-ttu-id="2fddd-212">Připravte zařízení Azure IoT SDK</span><span class="sxs-lookup"><span data-stu-id="2fddd-212">Prepare the Azure IoT Device SDK</span></span>

1. <span data-ttu-id="2fddd-213">Použijte jednu z následujících klientů SSH z hostitelského počítače pro připojení k vaší Edison Intel.</span><span class="sxs-lookup"><span data-stu-id="2fddd-213">Use one of the following SSH clients from your host computer to connect to your Intel Edison.</span></span> <span data-ttu-id="2fddd-214">IP adresa je z nástroje pro konfiguraci a heslo je ten, který jste nastavili v tohoto nástroje.</span><span class="sxs-lookup"><span data-stu-id="2fddd-214">The IP address is from the configuration tool and the password is the one you've set in that tool.</span></span>
    - <span data-ttu-id="2fddd-215">[PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="2fddd-215">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="2fddd-216">Integrovaného klienta SSH na Ubuntu nebo systému macOS (Spustit `ssh root@"the IP address"`).</span><span class="sxs-lookup"><span data-stu-id="2fddd-216">The built-in SSH client on Ubuntu or macOS (run `ssh root@"the IP address"`).</span></span>

2. <span data-ttu-id="2fddd-217">Klonování vzorku klientskou aplikaci do vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="2fddd-217">Clone the sample client app to your device.</span></span> 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-edison-client-app.git
   ```

3. <span data-ttu-id="2fddd-218">Pak přejděte do složky úložiště, spusťte následující příkaz k sestavení sady SDK Azure IoT</span><span class="sxs-lookup"><span data-stu-id="2fddd-218">Then navigate to the repo folder to run the following command to build Azure IoT SDK</span></span>

   ```bash
   cd iot-hub-c-intel-edison-client-app
   sed -i -e 's/\r$//' buildSDK.sh
   chmod 755 buildSDK.sh
   ./buildSDK.sh
   ```

### <a name="configure-the-sample-application"></a><span data-ttu-id="2fddd-219">Nakonfigurujte ukázkovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="2fddd-219">Configure the sample application</span></span>

1. <span data-ttu-id="2fddd-220">Otevřete konfigurační soubor spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="2fddd-220">Open the config file by running the following commands:</span></span>

   ```bash
   nano config.h
   ```

   ![Konfigurační soubor](media/iot-hub-intel-edison-kit-c-get-started/13_configure_file.png)

   <span data-ttu-id="2fddd-222">Existují dvě makra v tomto souboru můžete configurate.</span><span class="sxs-lookup"><span data-stu-id="2fddd-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="2fddd-223">První z nich je `INTERVAL`, která definuje časový interval mezi dvě zprávy, které odesílají do cloudu.</span><span class="sxs-lookup"><span data-stu-id="2fddd-223">The first one is `INTERVAL`, which defines the time interval between two messages that send to cloud.</span></span> <span data-ttu-id="2fddd-224">Druhý `SIMULATED_DATA`, což je logickou hodnotu pro jestli se má používat data simulované snímačů, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="2fddd-224">The second one `SIMULATED_DATA`,which is a Boolean value for whether to use simulated sensor data or not.</span></span>

   <span data-ttu-id="2fddd-225">Pokud jste **nemají senzoru**, nastavte `SIMULATED_DATA` hodnotu `1` aby ukázkovou aplikaci, vytváření a používání dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="2fddd-225">If you **don't have the sensor**, set the `SIMULATED_DATA` value to `1` to make the sample application create and use simulated sensor data.</span></span>

2. <span data-ttu-id="2fddd-226">Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="2fddd-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>

### <a name="build-and-run-the-sample-application"></a><span data-ttu-id="2fddd-227">Sestavení a spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="2fddd-227">Build and run the sample application</span></span>

1. <span data-ttu-id="2fddd-228">Vytvoření ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2fddd-228">Build the sample application by running the following command:</span></span>

   ```bash
   cmake . && make
   ```
   ![Sestavení výstupu](media/iot-hub-intel-edison-kit-c-get-started/14_build_output.png)

1. <span data-ttu-id="2fddd-230">Spuštění ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2fddd-230">Run the sample application by running the following command:</span></span>

   ```bash
   sudo ./app '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="2fddd-231">Zkontrolujte, zda jste způsobené kopírováním a vkládáním zařízení připojovací řetězec do jednoduchých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="2fddd-231">Make sure you copy-paste the device connection string into the single quotes.</span></span>

<span data-ttu-id="2fddd-232">Měli byste vidět následující výstup, který popisuje data snímačů a zprávy, které se odesílají do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2fddd-232">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

![Výstup – data snímačů odeslaný Intel Edison do služby IoT hub](media/iot-hub-intel-edison-kit-c-get-started/15_message_sent.png)

## <a name="next-steps"></a><span data-ttu-id="2fddd-234">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2fddd-234">Next steps</span></span>

<span data-ttu-id="2fddd-235">Spustíte ukázkovou aplikaci pro shromažďování dat snímačů a odeslat do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2fddd-235">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
