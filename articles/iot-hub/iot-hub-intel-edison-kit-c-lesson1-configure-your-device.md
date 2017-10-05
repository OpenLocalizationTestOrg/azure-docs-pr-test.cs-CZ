---
title: "Connect Intel Edison (C) k Azure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte Intel Edison pro první použití."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino nastavit připojení k počítači, instalační program arduino, arduino panelu arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: bb8aa45b-d3ff-4438-b9d6-a9725a45ade1
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0c92d9f7ba63b0a0929ff95599fd757ea425ef1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-intel-edison"></a><span data-ttu-id="957dd-104">Konfigurace vaší Edison Intel</span><span class="sxs-lookup"><span data-stu-id="957dd-104">Configure your Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="957dd-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="957dd-105">What you will do</span></span>
<span data-ttu-id="957dd-106">Nakonfigurujte Intel Edison pro první použití sestavte Tabule, je napájený a instalace nástroje pro konfiguraci na vaší ploše operační systém určený k flash Edison na firmwaru, nastavte jeho heslo a připojte ho k Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="957dd-106">Configure Intel Edison for first-time use by assembling the board, powering it up and installing configuration tool to your desktop OS to flash Edison's firmware, set its password and connect it to Wi-Fi.</span></span> <span data-ttu-id="957dd-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="957dd-107">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="957dd-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="957dd-108">What you will learn</span></span>
<span data-ttu-id="957dd-109">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="957dd-109">In this article, you will learn:</span></span>

* <span data-ttu-id="957dd-110">Jak sestavit Edison panelu a spotřeby nahoru.</span><span class="sxs-lookup"><span data-stu-id="957dd-110">How to assemble Edison board and power it up.</span></span>
* <span data-ttu-id="957dd-111">Jak flash Edison na firmwaru, nastavte heslo a připojení Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="957dd-111">How to flash Edison's firmware, set password and connect Wi-Fi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="957dd-112">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="957dd-112">What you need</span></span>
<span data-ttu-id="957dd-113">Pro dokončení této operace, musíte z vaší Intel Edison Starter Kit následujících částí:</span><span class="sxs-lookup"><span data-stu-id="957dd-113">To complete this operation, you need the following parts from your Intel Edison Starter Kit:</span></span>

* <span data-ttu-id="957dd-114">Intel® Edison modulu</span><span class="sxs-lookup"><span data-stu-id="957dd-114">Intel® Edison module</span></span>
* <span data-ttu-id="957dd-115">Panel Arduino rozšíření</span><span class="sxs-lookup"><span data-stu-id="957dd-115">Arduino expansion board</span></span>
* <span data-ttu-id="957dd-116">Jako mezeru mezi řádky nebo šrouby, zahrnout v balení, včetně dvou šrouby připevnit modulu Tabule rozšíření a čtyři sady šrouby a plastové vymezovače.</span><span class="sxs-lookup"><span data-stu-id="957dd-116">Any spacer bars or screws included in the packaging, including two screws to fasten the module to the expansion board and four sets of screws and plastic spacers.</span></span>
* <span data-ttu-id="957dd-117">Micro B kabelem USB typ A</span><span class="sxs-lookup"><span data-stu-id="957dd-117">A Micro B to Type A USB cable</span></span>
* <span data-ttu-id="957dd-118">Zdroj napájení přímo aktuální (DC).</span><span class="sxs-lookup"><span data-stu-id="957dd-118">A direct current (DC) power supply.</span></span> <span data-ttu-id="957dd-119">Zdroj napájení by měl být hodnocení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="957dd-119">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="957dd-120">ŘADIČ DOMÉNY 7 15V</span><span class="sxs-lookup"><span data-stu-id="957dd-120">7-15V DC</span></span>
  - <span data-ttu-id="957dd-121">Alespoň 1500mA</span><span class="sxs-lookup"><span data-stu-id="957dd-121">At least 1500mA</span></span>
  - <span data-ttu-id="957dd-122">PIN kód center nebo vnitřní by měla být kladná pólu napájení</span><span class="sxs-lookup"><span data-stu-id="957dd-122">The center/inner pin should be the positive pole of the power supply</span></span>

  ![Co v Startovní sady](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

<span data-ttu-id="957dd-124">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="957dd-124">You also need:</span></span>

* <span data-ttu-id="957dd-125">Počítač se systémem Windows, Mac nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="957dd-125">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="957dd-126">Bezdrátové připojení pro Edison pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="957dd-126">A wireless connection for Edison to connect to.</span></span>
* <span data-ttu-id="957dd-127">Připojení k Internetu kvůli stahování nástroje Konfigurace.</span><span class="sxs-lookup"><span data-stu-id="957dd-127">An Internet connection to download configuration tool.</span></span>

## <a name="assemble-your-board"></a><span data-ttu-id="957dd-128">Sestavte panel</span><span class="sxs-lookup"><span data-stu-id="957dd-128">Assemble your board</span></span>

<span data-ttu-id="957dd-129">Tato část obsahuje kroky pro připojení k vaší karty rozšíření modulu Intel® Edison.</span><span class="sxs-lookup"><span data-stu-id="957dd-129">This section contains steps to attach your Intel® Edison module to your expansion board.</span></span>

1. <span data-ttu-id="957dd-130">Místní modul Intel® Edison v rámci bílé osnovy na vaší kartě rozšíření zarovnání díry na modul s šrouby na kartě rozšíření.</span><span class="sxs-lookup"><span data-stu-id="957dd-130">Place the Intel® Edison module within the white outline on your expansion board, lining up the holes on the module with the screws on the expansion board.</span></span>

2. <span data-ttu-id="957dd-131">Klepnout na modul pod slova `What will you make?` dokud si myslíte, že během.</span><span class="sxs-lookup"><span data-stu-id="957dd-131">Press down on the module just below the words `What will you make?` until you feel a snap.</span></span>

   ![Sestavte Tabule 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. <span data-ttu-id="957dd-133">Dva šestnáctkových matice (součástí balíčku) použijte k zabezpečení modulu Tabule rozšíření.</span><span class="sxs-lookup"><span data-stu-id="957dd-133">Use the two hex nuts (included in the package) to secure the module to the expansion board.</span></span>

   ![Sestavte Tabule 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. <span data-ttu-id="957dd-135">Vložte šroubu v jednom z díry čtyři rohu na kartě rozšíření.</span><span class="sxs-lookup"><span data-stu-id="957dd-135">Insert a screw in one of the four corner holes on the expansion board.</span></span> <span data-ttu-id="957dd-136">Otočením a posílit mezi bílé plastové nosníků do šroubek.</span><span class="sxs-lookup"><span data-stu-id="957dd-136">Twist and tighten one of the white plastic spacers onto the screw.</span></span>

   ![Sestavte Tabule 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. <span data-ttu-id="957dd-138">Pro další tři rohu nosníků opakujte.</span><span class="sxs-lookup"><span data-stu-id="957dd-138">Repeat for the other three corner spacers.</span></span>

   ![Sestavte Tabule 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

<span data-ttu-id="957dd-140">Nyní je několik panel.</span><span class="sxs-lookup"><span data-stu-id="957dd-140">Now your board is assembled.</span></span>

   ![sestavený panelu](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a><span data-ttu-id="957dd-142">Napájení až Edison</span><span class="sxs-lookup"><span data-stu-id="957dd-142">Power up Edison</span></span>

1. <span data-ttu-id="957dd-143">Zařaďte napájení.</span><span class="sxs-lookup"><span data-stu-id="957dd-143">Plug in the power supply.</span></span>

   ![Zařadit napájení](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. <span data-ttu-id="957dd-145">Zelená DIODU (s názvem bez přípony DS1 na panelu rozšíření Arduino *), by měla zůstat po a light.</span><span class="sxs-lookup"><span data-stu-id="957dd-145">A green LED(labeled DS1 on the Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="957dd-146">Počkejte, než jedna minuta pro panel na dokončení spuštění.</span><span class="sxs-lookup"><span data-stu-id="957dd-146">Wait one minute for the board to finish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="957dd-147">Pokud nemáte ke zdroji napájení řadiče domény, může stále spotřeby Tabule přes USB port.</span><span class="sxs-lookup"><span data-stu-id="957dd-147">If you do not have a DC power supply, you can still power the board through a USB port.</span></span> <span data-ttu-id="957dd-148">V tématu `Connect Edison to your computer` podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="957dd-148">See `Connect Edison to your computer` section for details.</span></span> <span data-ttu-id="957dd-149">Pohánějící panel tímto způsobem může způsobit nepředvídatelné chování z vaší karty, zejména v případě, že pomocí sítě Wi-Fi nebo řídí motory.</span><span class="sxs-lookup"><span data-stu-id="957dd-149">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

## <a name="connect-edison-to-your-computer"></a><span data-ttu-id="957dd-150">Připojte k počítači Edison</span><span class="sxs-lookup"><span data-stu-id="957dd-150">Connect Edison to your computer</span></span>

1. <span data-ttu-id="957dd-151">Přepněte dolů mikrospínače směrem dva malých USB porty, takže Edison je v režimu zařízení.</span><span class="sxs-lookup"><span data-stu-id="957dd-151">Toggle down the microswitch towards the two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="957dd-152">Rozdíly mezi zařízení režim a režim hostitele, prosím odkazovat [zde](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="957dd-152">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Přepněte dolů mikrospínače](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. <span data-ttu-id="957dd-154">Připojte kabel USB malých nejvyšší malých portu USB.</span><span class="sxs-lookup"><span data-stu-id="957dd-154">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Horní malých portu USB](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. <span data-ttu-id="957dd-156">Druhém konci kabelu USB připojte k počítači.</span><span class="sxs-lookup"><span data-stu-id="957dd-156">Plug the other end of USB cable into your computer.</span></span>

   ![Počítač USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. <span data-ttu-id="957dd-158">Budete vědět, že panel úplné inicializace když se počítač připojí na nový disk (podobně jako vkládání SD karty do počítače).</span><span class="sxs-lookup"><span data-stu-id="957dd-158">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-the-configuration-tool"></a><span data-ttu-id="957dd-159">Stáhněte a spusťte nástroj Konfigurace</span><span class="sxs-lookup"><span data-stu-id="957dd-159">Download and run the configuration tool</span></span>
<span data-ttu-id="957dd-160">Získat nejnovější nástroje Konfigurace z [tento odkaz](https://software.intel.com/en-us/iot/hardware/edison/downloads) uvedené v části `Installers` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="957dd-160">Get the latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under the `Installers` heading.</span></span> <span data-ttu-id="957dd-161">Spusťte nástroj a postupujte podle jeho na obrazovce pokyny, kde je potřeba klepnutím na tlačítko Další</span><span class="sxs-lookup"><span data-stu-id="957dd-161">Execute the tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="957dd-162">Flash firmwaru</span><span class="sxs-lookup"><span data-stu-id="957dd-162">Flash firmware</span></span>
1. <span data-ttu-id="957dd-163">Na `Set up options` klikněte na tlačítko `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="957dd-163">On the `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="957dd-164">Vyberte bitovou kopii na flash na panel jedním z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="957dd-164">Select the image to flash onto your board by doing one of the following:</span></span>
   - <span data-ttu-id="957dd-165">Chcete-li stáhnout a flash panel s nejnovější bitové kopie firmwaru, které jsou k dispozici z Intel, vyberte `Download the latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="957dd-165">To download and flash your board with the latest firmware image available from Intel, select `Download the latest image version xxxx`.</span></span>
   - <span data-ttu-id="957dd-166">Chcete-li flash panel s bitovou kopií již uložení v počítači, vyberte `Select the local image`.</span><span class="sxs-lookup"><span data-stu-id="957dd-166">To flash your board with an image you already have saved on your computer, select `Select the local image`.</span></span> <span data-ttu-id="957dd-167">Vyhledejte a vyberte bitovou kopii, kterou chcete flash pro panel.</span><span class="sxs-lookup"><span data-stu-id="957dd-167">Browse to and select the image you want to flash to your board.</span></span>
3. <span data-ttu-id="957dd-168">Nástroj instalační program se pokusí o flash panel.</span><span class="sxs-lookup"><span data-stu-id="957dd-168">The setup tool will attempt to flash your board.</span></span> <span data-ttu-id="957dd-169">Celý proces blikající může trvat až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="957dd-169">The entire flashing process may take up to 10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="957dd-170">Nastavit heslo</span><span class="sxs-lookup"><span data-stu-id="957dd-170">Set password</span></span>
1. <span data-ttu-id="957dd-171">Na `Set up options` klikněte na tlačítko `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="957dd-171">On the `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="957dd-172">Můžete nastavit vlastní název pro panel Intel® Edison.</span><span class="sxs-lookup"><span data-stu-id="957dd-172">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="957dd-173">Tato položka je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="957dd-173">This is optional.</span></span>
3. <span data-ttu-id="957dd-174">Zadejte heslo pro panel a pak klikněte na `Set password`.</span><span class="sxs-lookup"><span data-stu-id="957dd-174">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="957dd-175">Známku výpadku heslo, které se později používá.</span><span class="sxs-lookup"><span data-stu-id="957dd-175">Mark down the password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="957dd-176">Připojení Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="957dd-176">Connect Wi-Fi</span></span>
1. <span data-ttu-id="957dd-177">Na `Set up options` klikněte na tlačítko `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="957dd-177">On the `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="957dd-178">Počkejte, až na jednu minutu jako počítač hledá dostupných sítí Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="957dd-178">Wait up to one minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="957dd-179">Z `Detected Networks` rozevíracího seznamu vyberte vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="957dd-179">From the `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="957dd-180">Z `Security` rozevíracího seznamu vyberte typ zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="957dd-180">From the `Security` drop-down list, select the network's security type.</span></span>
4. <span data-ttu-id="957dd-181">Poskytnout vaše údaje přihlašovací jméno a heslo a pak klikněte na `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="957dd-181">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="957dd-182">Známku výpadku IP adresy, která se později používá.</span><span class="sxs-lookup"><span data-stu-id="957dd-182">Mark down the IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="957dd-183">Ujistěte se, že Edison je připojený ke stejné síti jako počítače.</span><span class="sxs-lookup"><span data-stu-id="957dd-183">Make sure that Edison is connected to the same network as your computer.</span></span> <span data-ttu-id="957dd-184">Váš počítač připojí k vaší Edison pomocí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="957dd-184">Your computer connects to your Edison by using the IP address.</span></span>

<span data-ttu-id="957dd-185">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="957dd-185">Congratulations!</span></span> <span data-ttu-id="957dd-186">Úspěšně jste nakonfigurovali Edison.</span><span class="sxs-lookup"><span data-stu-id="957dd-186">You've successfully configured Edison.</span></span>

## <a name="summary"></a><span data-ttu-id="957dd-187">Souhrn</span><span class="sxs-lookup"><span data-stu-id="957dd-187">Summary</span></span>
<span data-ttu-id="957dd-188">V tomto článku jsme zjistili, jak sestavit Tabule Edison, flash jeho firmwaru, instalační program heslo a připojte ho k Wi-Fi pomocí nástroje Konfigurace.</span><span class="sxs-lookup"><span data-stu-id="957dd-188">In this article, you’ve learned how to assemble the Edison board, flash its firmware, setup password and connect it to Wi-Fi by using configuration tool.</span></span> <span data-ttu-id="957dd-189">Všimněte si, že DIODU není ještě light nahoru.</span><span class="sxs-lookup"><span data-stu-id="957dd-189">Note that the LED doesn't yet light up.</span></span> <span data-ttu-id="957dd-190">Dalším krokem je instalace nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na Edison.</span><span class="sxs-lookup"><span data-stu-id="957dd-190">The next task is to install the necessary tools and software in preparation for running a sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="957dd-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="957dd-191">Next steps</span></span>
<span data-ttu-id="957dd-192">[Získat nástroje][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="957dd-192">[Get the tools][get-the-tools]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md