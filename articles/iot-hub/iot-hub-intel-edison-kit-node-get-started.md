---
title: "aaaIntel Edison toocloud (Node.js) – připojit Edison Intel tooAzure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte Intel Edison tooAzure IoT Hub pro Intel Edison toosend data toohello Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot intel edison, intel edison iot hub, intel edison odesílat data toocloud, intel edison toocloud"
ms.assetid: a7c9cf2d-c102-41b0-aa45-41285c6877eb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/15/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfc3387efc532b4b83f0626a9cf61d12c2952af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-intel-edison-tooazure-iot-hub-nodejs"></a><span data-ttu-id="149a5-104">Připojit Intel Edison tooAzure IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="149a5-104">Connect Intel Edison tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="149a5-105">V tomto kurzu zahájíte učení hello základy práce s Intel Edison.</span><span class="sxs-lookup"><span data-stu-id="149a5-105">In this tutorial, you begin by learning hello basics of working with Intel Edison.</span></span> <span data-ttu-id="149a5-106">Pak zjistíte, jak tooseamlessly propojit své cloudové toohello zařízení pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="149a5-106">You then learn how tooseamlessly connect your devices toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span>

<span data-ttu-id="149a5-107">Nemáte sady ještě?</span><span class="sxs-lookup"><span data-stu-id="149a5-107">Don't have a kit yet?</span></span> <span data-ttu-id="149a5-108">Spustit [sem](https://azure.microsoft.com/develop/iot/starter-kits)</span><span class="sxs-lookup"><span data-stu-id="149a5-108">Start [here](https://azure.microsoft.com/develop/iot/starter-kits)</span></span>

## <a name="what-you-do"></a><span data-ttu-id="149a5-109">Co dělat</span><span class="sxs-lookup"><span data-stu-id="149a5-109">What you do</span></span>

* <span data-ttu-id="149a5-110">Instalační program Intel Edison a a Groove moduly.</span><span class="sxs-lookup"><span data-stu-id="149a5-110">Setup Intel Edison and and Grove modules.</span></span>
* <span data-ttu-id="149a5-111">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="149a5-111">Create an IoT hub.</span></span>
* <span data-ttu-id="149a5-112">Registrovat zařízení pro Edison ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="149a5-112">Register a device for Edison in your IoT hub.</span></span>
* <span data-ttu-id="149a5-113">Spuštění ukázkové aplikace na Edison toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="149a5-113">Run a sample application on Edison toosend sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="149a5-114">Připojte Intel Edison tooan IoT hub, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="149a5-114">Connect Intel Edison tooan IoT hub that you create.</span></span> <span data-ttu-id="149a5-115">Potom spustíte ukázkovou aplikaci na Edison toocollect teploty a vlhkosti data z teplotní snímač Groove.</span><span class="sxs-lookup"><span data-stu-id="149a5-115">Then you run a sample application on Edison toocollect temperature and humidity data from a Grove temperature sensor.</span></span> <span data-ttu-id="149a5-116">Nakonec odeslat hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="149a5-116">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="149a5-117">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="149a5-117">What you learn</span></span>

* <span data-ttu-id="149a5-118">Jak toocreate služby Azure IoT hub a získat nový připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="149a5-118">How toocreate an Azure IoT hub and get your new device connection string.</span></span>
* <span data-ttu-id="149a5-119">Jak tooconnect Edison s teplotní snímač Groove.</span><span class="sxs-lookup"><span data-stu-id="149a5-119">How tooconnect Edison with a Grove temperature sensor.</span></span>
* <span data-ttu-id="149a5-120">Jak data snímačů toocollect spuštěním ukázkovou aplikaci na Edison.</span><span class="sxs-lookup"><span data-stu-id="149a5-120">How toocollect sensor data by running a sample application on Edison.</span></span>
* <span data-ttu-id="149a5-121">Jak toosend senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="149a5-121">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="149a5-122">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="149a5-122">What you need</span></span>

![Co potřebujete](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* <span data-ttu-id="149a5-124">Hello Intel Edison panelu</span><span class="sxs-lookup"><span data-stu-id="149a5-124">hello Intel Edison board</span></span>
* <span data-ttu-id="149a5-125">Panel Arduino rozšíření</span><span class="sxs-lookup"><span data-stu-id="149a5-125">Arduino expansion board</span></span>
* <span data-ttu-id="149a5-126">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="149a5-126">An active Azure subscription.</span></span> <span data-ttu-id="149a5-127">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="149a5-127">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="149a5-128">Mac nebo počítači se systémem Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="149a5-128">A Mac or a PC that is running Windows or Linux.</span></span>
* <span data-ttu-id="149a5-129">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="149a5-129">An Internet connection.</span></span>
* <span data-ttu-id="149a5-130">Micro B tooType kabelu A USB</span><span class="sxs-lookup"><span data-stu-id="149a5-130">A Micro B tooType A USB cable</span></span>
* <span data-ttu-id="149a5-131">Zdroj napájení přímo aktuální (DC).</span><span class="sxs-lookup"><span data-stu-id="149a5-131">A direct current (DC) power supply.</span></span> <span data-ttu-id="149a5-132">Zdroj napájení by měl být hodnocení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="149a5-132">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="149a5-133">ŘADIČ DOMÉNY 7 15V</span><span class="sxs-lookup"><span data-stu-id="149a5-133">7-15V DC</span></span>
  - <span data-ttu-id="149a5-134">Alespoň 1500mA</span><span class="sxs-lookup"><span data-stu-id="149a5-134">At least 1500mA</span></span>
  - <span data-ttu-id="149a5-135">Hello center nebo vnitřní pin musí být kladná pólu hello hello napájení</span><span class="sxs-lookup"><span data-stu-id="149a5-135">hello center/inner pin should be hello positive pole of hello power supply</span></span>

<span data-ttu-id="149a5-136">Hello následující položky jsou volitelné:</span><span class="sxs-lookup"><span data-stu-id="149a5-136">hello following items are optional:</span></span>

* <span data-ttu-id="149a5-137">Groove základní štítu V2</span><span class="sxs-lookup"><span data-stu-id="149a5-137">Grove Base Shield V2</span></span>
* <span data-ttu-id="149a5-138">Groove - teplotní snímač</span><span class="sxs-lookup"><span data-stu-id="149a5-138">Grove - Temperature Sensor</span></span>
* <span data-ttu-id="149a5-139">Kabel Groove</span><span class="sxs-lookup"><span data-stu-id="149a5-139">Grove Cable</span></span>
* <span data-ttu-id="149a5-140">Jako mezeru mezi řádky nebo šrouby součástí hello balení, včetně dvěma šrouby toofasten hello modulu toohello rozšíření panelu a čtyři sady šrouby a plastové vymezovače.</span><span class="sxs-lookup"><span data-stu-id="149a5-140">Any spacer bars or screws included in hello packaging, including two screws toofasten hello module toohello expansion board and four sets of screws and plastic spacers.</span></span>

> [!NOTE] 
<span data-ttu-id="149a5-141">Tyto položky jsou volitelné, protože podpora ukázkový kód hello simulated data snímačů.</span><span class="sxs-lookup"><span data-stu-id="149a5-141">These items are optional because hello code sample support simulated sensor data.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a><span data-ttu-id="149a5-142">Instalační program Intel Edison</span><span class="sxs-lookup"><span data-stu-id="149a5-142">Setup Intel Edison</span></span>

### <a name="assemble-your-board"></a><span data-ttu-id="149a5-143">Sestavte panel</span><span class="sxs-lookup"><span data-stu-id="149a5-143">Assemble your board</span></span>

<span data-ttu-id="149a5-144">Tato část obsahuje kroky tooattach panel Intel® Edison modulu tooyour rozšíření.</span><span class="sxs-lookup"><span data-stu-id="149a5-144">This section contains steps tooattach your Intel® Edison module tooyour expansion board.</span></span>

1. <span data-ttu-id="149a5-145">Místní hello Intel® Edison modul v rámci hello bílé outline na vaší kartě rozšíření zarovnání hello díry v modulu hello s hello šrouby na panelu rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="149a5-145">Place hello Intel® Edison module within hello white outline on your expansion board, lining up hello holes on hello module with hello screws on hello expansion board.</span></span>

2. <span data-ttu-id="149a5-146">Klepnout na hello modulu pod hello slova `What will you make?` dokud si myslíte, že během.</span><span class="sxs-lookup"><span data-stu-id="149a5-146">Press down on hello module just below hello words `What will you make?` until you feel a snap.</span></span>

   ![Sestavte Tabule 2](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. <span data-ttu-id="149a5-148">Použijte hello dva šestnáctkových matice (zahrnutý v balíčku hello) toosecure hello modulu toohello rozšíření panelu.</span><span class="sxs-lookup"><span data-stu-id="149a5-148">Use hello two hex nuts (included in hello package) toosecure hello module toohello expansion board.</span></span>

   ![Sestavte Tabule 3](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. <span data-ttu-id="149a5-150">Vložte šroubu v jednom z hello čtyři díry rohu na panelu rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="149a5-150">Insert a screw in one of hello four corner holes on hello expansion board.</span></span> <span data-ttu-id="149a5-151">Otočením a posílit mezi hello bílé plastové nosníků do šroubovacím hello.</span><span class="sxs-lookup"><span data-stu-id="149a5-151">Twist and tighten one of hello white plastic spacers onto hello screw.</span></span>

   ![Sestavte Tabule 4](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. <span data-ttu-id="149a5-153">Tento postup opakujte pro hello nosníků další tři rohu.</span><span class="sxs-lookup"><span data-stu-id="149a5-153">Repeat for hello other three corner spacers.</span></span>

   ![Sestavte Tabule 5](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

<span data-ttu-id="149a5-155">Nyní je několik panel.</span><span class="sxs-lookup"><span data-stu-id="149a5-155">Now your board is assembled.</span></span>

   ![sestavený panelu](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-hello-grove-base-shield-and-hello-temperature-sensor"></a><span data-ttu-id="149a5-157">Připojit hello štítu základní Groove a senzor teploty hello</span><span class="sxs-lookup"><span data-stu-id="149a5-157">Connect hello Grove Base Shield and hello temperature sensor</span></span>

1. <span data-ttu-id="149a5-158">Na panelu tooyour umístíte hello štítu základní Groove.</span><span class="sxs-lookup"><span data-stu-id="149a5-158">Place hello Grove Base Shield on tooyour board.</span></span> <span data-ttu-id="149a5-159">Ujistěte se, že všechny kódy PIN jsou úzce připojovány panel.</span><span class="sxs-lookup"><span data-stu-id="149a5-159">Make sure all pins are tightly plugged into your board.</span></span>
   
   ![Základní štítu Groove](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. <span data-ttu-id="149a5-161">Senzor teploty použití Groove kabel tooconnect Groove do hello Groove základní štítu **A0** portu.</span><span class="sxs-lookup"><span data-stu-id="149a5-161">Use Grove Cable tooconnect Grove temperature sensor onto hello Grove Base Shield **A0** port.</span></span>

   ![Připojit tootemperature senzor](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Edison a senzor připojení](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

<span data-ttu-id="149a5-164">Vaše senzor je nyní připraven.</span><span class="sxs-lookup"><span data-stu-id="149a5-164">Now your sensor is ready.</span></span>

### <a name="power-up-edison"></a><span data-ttu-id="149a5-165">Napájení až Edison</span><span class="sxs-lookup"><span data-stu-id="149a5-165">Power up Edison</span></span>

1. <span data-ttu-id="149a5-166">Zařaďte hello napájení.</span><span class="sxs-lookup"><span data-stu-id="149a5-166">Plug in hello power supply.</span></span>

   ![Zařadit napájení](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. <span data-ttu-id="149a5-168">Zelená DIODU (s názvem bez přípony DS1 na hello Tabule rozšíření Arduino *), by měla zůstat po a light.</span><span class="sxs-lookup"><span data-stu-id="149a5-168">A green LED(labeled DS1 on hello Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="149a5-169">Počkejte jednu minutu toofinish Tabule hello dalším spuštění.</span><span class="sxs-lookup"><span data-stu-id="149a5-169">Wait one minute for hello board toofinish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="149a5-170">Pokud nemáte ke zdroji napájení řadiče domény, můžete stále panelu power hello přes USB port.</span><span class="sxs-lookup"><span data-stu-id="149a5-170">If you do not have a DC power supply, you can still power hello board through a USB port.</span></span> <span data-ttu-id="149a5-171">V tématu `Connect Edison tooyour computer` podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="149a5-171">See `Connect Edison tooyour computer` section for details.</span></span> <span data-ttu-id="149a5-172">Pohánějící panel tímto způsobem může způsobit nepředvídatelné chování z vaší karty, zejména v případě, že pomocí sítě Wi-Fi nebo řídí motory.</span><span class="sxs-lookup"><span data-stu-id="149a5-172">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

### <a name="connect-edison-tooyour-computer"></a><span data-ttu-id="149a5-173">Připojte počítač tooyour Edison</span><span class="sxs-lookup"><span data-stu-id="149a5-173">Connect Edison tooyour computer</span></span>

1. <span data-ttu-id="149a5-174">Přepnutí dolů hello mikrospínače směrem hello dva malých USB porty, takže Edison je v režimu zařízení.</span><span class="sxs-lookup"><span data-stu-id="149a5-174">Toggle down hello microswitch towards hello two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="149a5-175">Rozdíly mezi zařízení režim a režim hostitele, prosím odkazovat [zde](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="149a5-175">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Přepněte dolů mikrospínače hello](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. <span data-ttu-id="149a5-177">Připojte kabel USB malých hello portu USB horním malých hello.</span><span class="sxs-lookup"><span data-stu-id="149a5-177">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Horní malých portu USB](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. <span data-ttu-id="149a5-179">Moduly hello druhém konci kabelu USB k počítači.</span><span class="sxs-lookup"><span data-stu-id="149a5-179">Plug hello other end of USB cable into your computer.</span></span>

   ![Počítač USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. <span data-ttu-id="149a5-181">Budete vědět, že panel úplné inicializace když se počítač připojí na nový disk (podobně jako vkládání SD karty do počítače).</span><span class="sxs-lookup"><span data-stu-id="149a5-181">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-hello-configuration-tool"></a><span data-ttu-id="149a5-182">Stáhněte a spusťte konfigurační nástroj hello</span><span class="sxs-lookup"><span data-stu-id="149a5-182">Download and run hello configuration tool</span></span>
<span data-ttu-id="149a5-183">Získat nejnovější nástroje Konfigurace hello z [tento odkaz](https://software.intel.com/en-us/iot/hardware/edison/downloads) uvedené v části hello `Installers` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="149a5-183">Get hello latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under hello `Installers` heading.</span></span> <span data-ttu-id="149a5-184">Spusťte nástroj hello a postupujte podle jeho na obrazovce pokyny, kde je potřeba klepnutím na tlačítko Další</span><span class="sxs-lookup"><span data-stu-id="149a5-184">Execute hello tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="149a5-185">Flash firmwaru</span><span class="sxs-lookup"><span data-stu-id="149a5-185">Flash firmware</span></span>
1. <span data-ttu-id="149a5-186">Na hello `Set up options` klikněte na tlačítko `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="149a5-186">On hello `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="149a5-187">Vyberte bitovou kopii tooflash hello na panel jedním z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="149a5-187">Select hello image tooflash onto your board by doing one of hello following:</span></span>
   - <span data-ttu-id="149a5-188">toodownload a flash panel s hello nejnovější firmware bitové kopie k dispozici z Intel, vyberte `Download hello latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="149a5-188">toodownload and flash your board with hello latest firmware image available from Intel, select `Download hello latest image version xxxx`.</span></span>
   - <span data-ttu-id="149a5-189">Vyberte panel s bitovou kopií již uložení v počítači, tooflash `Select hello local image`.</span><span class="sxs-lookup"><span data-stu-id="149a5-189">tooflash your board with an image you already have saved on your computer, select `Select hello local image`.</span></span> <span data-ttu-id="149a5-190">Procházejte tooand hello vyberte bitovou kopii tooflash tooyour panelu.</span><span class="sxs-lookup"><span data-stu-id="149a5-190">Browse tooand select hello image you want tooflash tooyour board.</span></span>
3. <span data-ttu-id="149a5-191">Instalační program nástroje Hello pokusí tooflash panel.</span><span class="sxs-lookup"><span data-stu-id="149a5-191">hello setup tool will attempt tooflash your board.</span></span> <span data-ttu-id="149a5-192">celý proces blikající Hello může trvat až too10 minut.</span><span class="sxs-lookup"><span data-stu-id="149a5-192">hello entire flashing process may take up too10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="149a5-193">Nastavit heslo</span><span class="sxs-lookup"><span data-stu-id="149a5-193">Set password</span></span>
1. <span data-ttu-id="149a5-194">Na hello `Set up options` klikněte na tlačítko `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="149a5-194">On hello `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="149a5-195">Můžete nastavit vlastní název pro panel Intel® Edison.</span><span class="sxs-lookup"><span data-stu-id="149a5-195">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="149a5-196">Tato položka je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="149a5-196">This is optional.</span></span>
3. <span data-ttu-id="149a5-197">Zadejte heslo pro panel a pak klikněte na `Set password`.</span><span class="sxs-lookup"><span data-stu-id="149a5-197">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="149a5-198">Známku výpadku hello hesla, které se později používá.</span><span class="sxs-lookup"><span data-stu-id="149a5-198">Mark down hello password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="149a5-199">Připojení Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="149a5-199">Connect Wi-Fi</span></span>
1. <span data-ttu-id="149a5-200">Na hello `Set up options` klikněte na tlačítko `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="149a5-200">On hello `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="149a5-201">Počkejte, až minutu tooone jako kontrolách počítači k dispozici sítě Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="149a5-201">Wait up tooone minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="149a5-202">Z hello `Detected Networks` rozevíracího seznamu vyberte vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="149a5-202">From hello `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="149a5-203">Z hello `Security` rozevíracího seznamu, vyberte hello sítě typ zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="149a5-203">From hello `Security` drop-down list, select hello network's security type.</span></span>
4. <span data-ttu-id="149a5-204">Poskytnout vaše údaje přihlašovací jméno a heslo a pak klikněte na `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="149a5-204">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="149a5-205">Známku výpadku hello IP adresy, která se později používá.</span><span class="sxs-lookup"><span data-stu-id="149a5-205">Mark down hello IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="149a5-206">Ujistěte se, že Edison je připojený toohello stejné sítě jako váš počítač.</span><span class="sxs-lookup"><span data-stu-id="149a5-206">Make sure that Edison is connected toohello same network as your computer.</span></span> <span data-ttu-id="149a5-207">Váš počítač připojí tooyour Edison pomocí hello IP adresy.</span><span class="sxs-lookup"><span data-stu-id="149a5-207">Your computer connects tooyour Edison by using hello IP address.</span></span>

   ![Připojit tootemperature senzor](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

<span data-ttu-id="149a5-209">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="149a5-209">Congratulations!</span></span> <span data-ttu-id="149a5-210">Úspěšně jste nakonfigurovali Edison.</span><span class="sxs-lookup"><span data-stu-id="149a5-210">You've successfully configured Edison.</span></span>

## <a name="run-a-sample-application-on-intel-edison"></a><span data-ttu-id="149a5-211">Spuštění ukázkové aplikace na Intel Edison</span><span class="sxs-lookup"><span data-stu-id="149a5-211">Run a sample application on Intel Edison</span></span>

### <a name="prepare-hello-azure-iot-device-sdk"></a><span data-ttu-id="149a5-212">Příprava hello sady SDK zařízení Azure IoT</span><span class="sxs-lookup"><span data-stu-id="149a5-212">Prepare hello Azure IoT Device SDK</span></span>

1. <span data-ttu-id="149a5-213">Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooyour Intel Edison hello.</span><span class="sxs-lookup"><span data-stu-id="149a5-213">Use one of hello following SSH clients from your host computer tooconnect tooyour Intel Edison.</span></span> <span data-ttu-id="149a5-214">Hello IP adresa je z hello konfigurační nástroj a hello heslo je hello jeden, které jste nastavili v tohoto nástroje.</span><span class="sxs-lookup"><span data-stu-id="149a5-214">hello IP address is from hello configuration tool and hello password is hello one you've set in that tool.</span></span>
    - <span data-ttu-id="149a5-215">[PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="149a5-215">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="149a5-216">Hello integrovaného klienta SSH na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="149a5-216">hello built-in SSH client on Ubuntu or macOS.</span></span>

2. <span data-ttu-id="149a5-217">Klon hello ukázka klientské aplikace tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="149a5-217">Clone hello sample client app tooyour device.</span></span> 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. <span data-ttu-id="149a5-218">Pak přejděte toohello úložišti složky toorun hello následující příkaz tooinstall všechny balíčky, může trvat toocomplete několika minut.</span><span class="sxs-lookup"><span data-stu-id="149a5-218">Then navigate toohello repo folder toorun hello following command tooinstall all packages, it may take serval minutes toocomplete.</span></span>
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-hello-sample-application"></a><span data-ttu-id="149a5-219">Nakonfigurujte a spusťte ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="149a5-219">Configure and run hello sample application</span></span>

1. <span data-ttu-id="149a5-220">Otevřete konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="149a5-220">Open hello config file by running hello following commands:</span></span>

   ```bash
   nano config.json
   ```

   ![Konfigurační soubor](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   <span data-ttu-id="149a5-222">Existují dvě makra v tomto souboru můžete configurate.</span><span class="sxs-lookup"><span data-stu-id="149a5-222">There are two macros in this file you can configurate.</span></span> <span data-ttu-id="149a5-223">Hello nejprve jeden je `INTERVAL`, která definuje hello časový interval mezi dvě zprávy, které odesílají toocloud.</span><span class="sxs-lookup"><span data-stu-id="149a5-223">hello first one is `INTERVAL`, which defines hello time interval between two messages that send toocloud.</span></span> <span data-ttu-id="149a5-224">Hello druhá `simulatedData`, což je logickou hodnotu pro jestli toouse simulated data snímačů, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="149a5-224">hello second one `simulatedData`,which is a Boolean value for whether toouse simulated sensor data or not.</span></span>

   <span data-ttu-id="149a5-225">Pokud jste **nemají hello senzor**, nastavte hello `simulatedData` hodnota příliš`true` toomake hello ukázkovou aplikaci vytváření a používání dat snímačů simulované.</span><span class="sxs-lookup"><span data-stu-id="149a5-225">If you **don't have hello sensor**, set hello `simulatedData` value too`true` toomake hello sample application create and use simulated sensor data.</span></span>

1. <span data-ttu-id="149a5-226">Uložte a zavřete stisknutím řízení-O > zadejte > CTRL-X.</span><span class="sxs-lookup"><span data-stu-id="149a5-226">Save and exit by pressing Control-O > Enter > Control-X.</span></span>


1. <span data-ttu-id="149a5-227">Spuštění ukázkové aplikace hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="149a5-227">Run hello sample application by running hello following command:</span></span>

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   <span data-ttu-id="149a5-228">Zkontrolujte, zda jste způsobené kopírováním a vkládáním hello zařízení připojovací řetězec do jednoduchých uvozovek a být hello.</span><span class="sxs-lookup"><span data-stu-id="149a5-228">Make sure you copy-paste hello device connection string into hello single quotes.</span></span>

<span data-ttu-id="149a5-229">Měli byste vidět, že hello následující výstup, že zobrazuje hello senzor dat a hello zpráv, které jsou odeslány tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="149a5-229">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub.</span></span>

![Výstup – senzor dat odesílaných ze služby IoT hub Intel Edison tooyour](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a><span data-ttu-id="149a5-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="149a5-231">Next steps</span></span>

<span data-ttu-id="149a5-232">Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="149a5-232">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
