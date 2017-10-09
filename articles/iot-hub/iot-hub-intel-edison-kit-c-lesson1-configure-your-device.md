---
title: "Connect Intel Edison (C) tooAzure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte Intel Edison pro první použití."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino nastavit připojení arduino toopc, instalační program arduino, arduino panelu"
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
ms.openlocfilehash: 5e06e06f1fcea02086e95742804f82cfcb8e265f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-intel-edison"></a><span data-ttu-id="6b877-104">Konfigurace vaší Edison Intel</span><span class="sxs-lookup"><span data-stu-id="6b877-104">Configure your Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="6b877-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="6b877-105">What you will do</span></span>
<span data-ttu-id="6b877-106">Nakonfigurujte Intel Edison pro první použití sestavte hello Tabule, napájený ji a instalaci nástroje configuration tooyour plochy OS tooflash Edison na firmwaru, nastavte jeho heslo a připojte ho tooWi-Fi.</span><span class="sxs-lookup"><span data-stu-id="6b877-106">Configure Intel Edison for first-time use by assembling hello board, powering it up and installing configuration tool tooyour desktop OS tooflash Edison's firmware, set its password and connect it tooWi-Fi.</span></span> <span data-ttu-id="6b877-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="6b877-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6b877-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="6b877-108">What you will learn</span></span>
<span data-ttu-id="6b877-109">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="6b877-109">In this article, you will learn:</span></span>

* <span data-ttu-id="6b877-110">Jak tooassemble Edison board a zapněte nahoru.</span><span class="sxs-lookup"><span data-stu-id="6b877-110">How tooassemble Edison board and power it up.</span></span>
* <span data-ttu-id="6b877-111">Tooflash Edison firmwaru, jak nastavit heslo a připojení Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="6b877-111">How tooflash Edison's firmware, set password and connect Wi-Fi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6b877-112">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="6b877-112">What you need</span></span>
<span data-ttu-id="6b877-113">toocomplete tuto operaci je třeba hello následujících částí z vaší Intel Edison Starter Kit:</span><span class="sxs-lookup"><span data-stu-id="6b877-113">toocomplete this operation, you need hello following parts from your Intel Edison Starter Kit:</span></span>

* <span data-ttu-id="6b877-114">Intel® Edison modulu</span><span class="sxs-lookup"><span data-stu-id="6b877-114">Intel® Edison module</span></span>
* <span data-ttu-id="6b877-115">Panel Arduino rozšíření</span><span class="sxs-lookup"><span data-stu-id="6b877-115">Arduino expansion board</span></span>
* <span data-ttu-id="6b877-116">Jako mezeru mezi řádky nebo šrouby součástí hello balení, včetně dvěma šrouby toofasten hello modulu toohello rozšíření panelu a čtyři sady šrouby a plastové vymezovače.</span><span class="sxs-lookup"><span data-stu-id="6b877-116">Any spacer bars or screws included in hello packaging, including two screws toofasten hello module toohello expansion board and four sets of screws and plastic spacers.</span></span>
* <span data-ttu-id="6b877-117">Micro B tooType kabelu A USB</span><span class="sxs-lookup"><span data-stu-id="6b877-117">A Micro B tooType A USB cable</span></span>
* <span data-ttu-id="6b877-118">Zdroj napájení přímo aktuální (DC).</span><span class="sxs-lookup"><span data-stu-id="6b877-118">A direct current (DC) power supply.</span></span> <span data-ttu-id="6b877-119">Zdroj napájení by měl být hodnocení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6b877-119">Your power supply should be rated as follows:</span></span>
  - <span data-ttu-id="6b877-120">ŘADIČ DOMÉNY 7 15V</span><span class="sxs-lookup"><span data-stu-id="6b877-120">7-15V DC</span></span>
  - <span data-ttu-id="6b877-121">Alespoň 1500mA</span><span class="sxs-lookup"><span data-stu-id="6b877-121">At least 1500mA</span></span>
  - <span data-ttu-id="6b877-122">Hello center nebo vnitřní pin musí být kladná pólu hello hello napájení</span><span class="sxs-lookup"><span data-stu-id="6b877-122">hello center/inner pin should be hello positive pole of hello power supply</span></span>

  ![Co v Startovní sady](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

<span data-ttu-id="6b877-124">Budete také muset:</span><span class="sxs-lookup"><span data-stu-id="6b877-124">You also need:</span></span>

* <span data-ttu-id="6b877-125">Počítač se systémem Windows, Mac nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="6b877-125">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="6b877-126">Bezdrátové připojení pro Edison tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="6b877-126">A wireless connection for Edison tooconnect to.</span></span>
* <span data-ttu-id="6b877-127">Internetu toodownload konfigurační nástroj pro připojení.</span><span class="sxs-lookup"><span data-stu-id="6b877-127">An Internet connection toodownload configuration tool.</span></span>

## <a name="assemble-your-board"></a><span data-ttu-id="6b877-128">Sestavte panel</span><span class="sxs-lookup"><span data-stu-id="6b877-128">Assemble your board</span></span>

<span data-ttu-id="6b877-129">Tato část obsahuje kroky tooattach panel Intel® Edison modulu tooyour rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6b877-129">This section contains steps tooattach your Intel® Edison module tooyour expansion board.</span></span>

1. <span data-ttu-id="6b877-130">Místní hello Intel® Edison modul v rámci hello bílé outline na vaší kartě rozšíření zarovnání hello díry v modulu hello s hello šrouby na panelu rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="6b877-130">Place hello Intel® Edison module within hello white outline on your expansion board, lining up hello holes on hello module with hello screws on hello expansion board.</span></span>

2. <span data-ttu-id="6b877-131">Klepnout na hello modulu pod hello slova `What will you make?` dokud si myslíte, že během.</span><span class="sxs-lookup"><span data-stu-id="6b877-131">Press down on hello module just below hello words `What will you make?` until you feel a snap.</span></span>

   ![Sestavte Tabule 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. <span data-ttu-id="6b877-133">Použijte hello dva šestnáctkových matice (zahrnutý v balíčku hello) toosecure hello modulu toohello rozšíření panelu.</span><span class="sxs-lookup"><span data-stu-id="6b877-133">Use hello two hex nuts (included in hello package) toosecure hello module toohello expansion board.</span></span>

   ![Sestavte Tabule 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. <span data-ttu-id="6b877-135">Vložte šroubu v jednom z hello čtyři díry rohu na panelu rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="6b877-135">Insert a screw in one of hello four corner holes on hello expansion board.</span></span> <span data-ttu-id="6b877-136">Otočením a posílit mezi hello bílé plastové nosníků do šroubovacím hello.</span><span class="sxs-lookup"><span data-stu-id="6b877-136">Twist and tighten one of hello white plastic spacers onto hello screw.</span></span>

   ![Sestavte Tabule 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. <span data-ttu-id="6b877-138">Tento postup opakujte pro hello nosníků další tři rohu.</span><span class="sxs-lookup"><span data-stu-id="6b877-138">Repeat for hello other three corner spacers.</span></span>

   ![Sestavte Tabule 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

<span data-ttu-id="6b877-140">Nyní je několik panel.</span><span class="sxs-lookup"><span data-stu-id="6b877-140">Now your board is assembled.</span></span>

   ![sestavený panelu](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a><span data-ttu-id="6b877-142">Napájení až Edison</span><span class="sxs-lookup"><span data-stu-id="6b877-142">Power up Edison</span></span>

1. <span data-ttu-id="6b877-143">Zařaďte hello napájení.</span><span class="sxs-lookup"><span data-stu-id="6b877-143">Plug in hello power supply.</span></span>

   ![Zařadit napájení](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. <span data-ttu-id="6b877-145">Zelená DIODU (s názvem bez přípony DS1 na hello Tabule rozšíření Arduino *), by měla zůstat po a light.</span><span class="sxs-lookup"><span data-stu-id="6b877-145">A green LED(labeled DS1 on hello Arduino* expansion board) should light up and stay lit.</span></span>

3. <span data-ttu-id="6b877-146">Počkejte jednu minutu toofinish Tabule hello dalším spuštění.</span><span class="sxs-lookup"><span data-stu-id="6b877-146">Wait one minute for hello board toofinish booting up.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6b877-147">Pokud nemáte ke zdroji napájení řadiče domény, můžete stále panelu power hello přes USB port.</span><span class="sxs-lookup"><span data-stu-id="6b877-147">If you do not have a DC power supply, you can still power hello board through a USB port.</span></span> <span data-ttu-id="6b877-148">V tématu `Connect Edison tooyour computer` podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6b877-148">See `Connect Edison tooyour computer` section for details.</span></span> <span data-ttu-id="6b877-149">Pohánějící panel tímto způsobem může způsobit nepředvídatelné chování z vaší karty, zejména v případě, že pomocí sítě Wi-Fi nebo řídí motory.</span><span class="sxs-lookup"><span data-stu-id="6b877-149">Powering your board in this fashion may result in unpredictable behavior from your board, especially when using Wi-Fi or driving motors.</span></span>

## <a name="connect-edison-tooyour-computer"></a><span data-ttu-id="6b877-150">Připojte počítač tooyour Edison</span><span class="sxs-lookup"><span data-stu-id="6b877-150">Connect Edison tooyour computer</span></span>

1. <span data-ttu-id="6b877-151">Přepnutí dolů hello mikrospínače směrem hello dva malých USB porty, takže Edison je v režimu zařízení.</span><span class="sxs-lookup"><span data-stu-id="6b877-151">Toggle down hello microswitch towards hello two micro USB ports, so that Edison is in device mode.</span></span> <span data-ttu-id="6b877-152">Rozdíly mezi zařízení režim a režim hostitele, prosím odkazovat [zde](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span><span class="sxs-lookup"><span data-stu-id="6b877-152">For differences between device mode and host mode, please reference [here](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).</span></span>

   ![Přepněte dolů mikrospínače hello](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. <span data-ttu-id="6b877-154">Připojte kabel USB malých hello portu USB horním malých hello.</span><span class="sxs-lookup"><span data-stu-id="6b877-154">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Horní malých portu USB](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. <span data-ttu-id="6b877-156">Moduly hello druhém konci kabelu USB k počítači.</span><span class="sxs-lookup"><span data-stu-id="6b877-156">Plug hello other end of USB cable into your computer.</span></span>

   ![Počítač USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. <span data-ttu-id="6b877-158">Budete vědět, že panel úplné inicializace když se počítač připojí na nový disk (podobně jako vkládání SD karty do počítače).</span><span class="sxs-lookup"><span data-stu-id="6b877-158">You will know that your board is fully initialized when your computer mounts a new drive (much like inserting a SD card into your computer).</span></span>

## <a name="download-and-run-hello-configuration-tool"></a><span data-ttu-id="6b877-159">Stáhněte a spusťte konfigurační nástroj hello</span><span class="sxs-lookup"><span data-stu-id="6b877-159">Download and run hello configuration tool</span></span>
<span data-ttu-id="6b877-160">Získat nejnovější nástroje Konfigurace hello z [tento odkaz](https://software.intel.com/en-us/iot/hardware/edison/downloads) uvedené v části hello `Installers` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="6b877-160">Get hello latest configuration tool from [this link](https://software.intel.com/en-us/iot/hardware/edison/downloads) listed under hello `Installers` heading.</span></span> <span data-ttu-id="6b877-161">Spusťte nástroj hello a postupujte podle jeho na obrazovce pokyny, kde je potřeba klepnutím na tlačítko Další</span><span class="sxs-lookup"><span data-stu-id="6b877-161">Execute hello tool and follow its on-screen instructions, clicking Next where needed</span></span>

### <a name="flash-firmware"></a><span data-ttu-id="6b877-162">Flash firmwaru</span><span class="sxs-lookup"><span data-stu-id="6b877-162">Flash firmware</span></span>
1. <span data-ttu-id="6b877-163">Na hello `Set up options` klikněte na tlačítko `Flash Firmware`.</span><span class="sxs-lookup"><span data-stu-id="6b877-163">On hello `Set up options` page, click `Flash Firmware`.</span></span>
2. <span data-ttu-id="6b877-164">Vyberte bitovou kopii tooflash hello na panel jedním z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="6b877-164">Select hello image tooflash onto your board by doing one of hello following:</span></span>
   - <span data-ttu-id="6b877-165">toodownload a flash panel s hello nejnovější firmware bitové kopie k dispozici z Intel, vyberte `Download hello latest image version xxxx`.</span><span class="sxs-lookup"><span data-stu-id="6b877-165">toodownload and flash your board with hello latest firmware image available from Intel, select `Download hello latest image version xxxx`.</span></span>
   - <span data-ttu-id="6b877-166">Vyberte panel s bitovou kopií již uložení v počítači, tooflash `Select hello local image`.</span><span class="sxs-lookup"><span data-stu-id="6b877-166">tooflash your board with an image you already have saved on your computer, select `Select hello local image`.</span></span> <span data-ttu-id="6b877-167">Procházejte tooand hello vyberte bitovou kopii tooflash tooyour panelu.</span><span class="sxs-lookup"><span data-stu-id="6b877-167">Browse tooand select hello image you want tooflash tooyour board.</span></span>
3. <span data-ttu-id="6b877-168">Instalační program nástroje Hello pokusí tooflash panel.</span><span class="sxs-lookup"><span data-stu-id="6b877-168">hello setup tool will attempt tooflash your board.</span></span> <span data-ttu-id="6b877-169">celý proces blikající Hello může trvat až too10 minut.</span><span class="sxs-lookup"><span data-stu-id="6b877-169">hello entire flashing process may take up too10 minutes.</span></span>

### <a name="set-password"></a><span data-ttu-id="6b877-170">Nastavit heslo</span><span class="sxs-lookup"><span data-stu-id="6b877-170">Set password</span></span>
1. <span data-ttu-id="6b877-171">Na hello `Set up options` klikněte na tlačítko `Enable Security`.</span><span class="sxs-lookup"><span data-stu-id="6b877-171">On hello `Set up options` page, click `Enable Security`.</span></span>
2. <span data-ttu-id="6b877-172">Můžete nastavit vlastní název pro panel Intel® Edison.</span><span class="sxs-lookup"><span data-stu-id="6b877-172">You can set a custom name for your Intel® Edison board.</span></span> <span data-ttu-id="6b877-173">Tato položka je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="6b877-173">This is optional.</span></span>
3. <span data-ttu-id="6b877-174">Zadejte heslo pro panel a pak klikněte na `Set password`.</span><span class="sxs-lookup"><span data-stu-id="6b877-174">Type a password for your board, then click `Set password`.</span></span>
4. <span data-ttu-id="6b877-175">Známku výpadku hello hesla, které se později používá.</span><span class="sxs-lookup"><span data-stu-id="6b877-175">Mark down hello password, which is used later.</span></span>

### <a name="connect-wi-fi"></a><span data-ttu-id="6b877-176">Připojení Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="6b877-176">Connect Wi-Fi</span></span>
1. <span data-ttu-id="6b877-177">Na hello `Set up options` klikněte na tlačítko `Connect Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="6b877-177">On hello `Set up options` page, click `Connect Wi-Fi`.</span></span> <span data-ttu-id="6b877-178">Počkejte, až minutu tooone jako kontrolách počítači k dispozici sítě Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="6b877-178">Wait up tooone minute as your computer scans for available Wi-Fi networks.</span></span>
2. <span data-ttu-id="6b877-179">Z hello `Detected Networks` rozevíracího seznamu vyberte vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="6b877-179">From hello `Detected Networks` drop-down list, select your network.</span></span>
3. <span data-ttu-id="6b877-180">Z hello `Security` rozevíracího seznamu, vyberte hello sítě typ zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6b877-180">From hello `Security` drop-down list, select hello network's security type.</span></span>
4. <span data-ttu-id="6b877-181">Poskytnout vaše údaje přihlašovací jméno a heslo a pak klikněte na `Configure Wi-Fi`.</span><span class="sxs-lookup"><span data-stu-id="6b877-181">Provide your login and password information, then click `Configure Wi-Fi`.</span></span>
5. <span data-ttu-id="6b877-182">Známku výpadku hello IP adresy, která se později používá.</span><span class="sxs-lookup"><span data-stu-id="6b877-182">Mark down hello IP address, which is used later.</span></span>

> [!NOTE]
> <span data-ttu-id="6b877-183">Ujistěte se, že Edison je připojený toohello stejné sítě jako váš počítač.</span><span class="sxs-lookup"><span data-stu-id="6b877-183">Make sure that Edison is connected toohello same network as your computer.</span></span> <span data-ttu-id="6b877-184">Váš počítač připojí tooyour Edison pomocí hello IP adresy.</span><span class="sxs-lookup"><span data-stu-id="6b877-184">Your computer connects tooyour Edison by using hello IP address.</span></span>

<span data-ttu-id="6b877-185">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="6b877-185">Congratulations!</span></span> <span data-ttu-id="6b877-186">Úspěšně jste nakonfigurovali Edison.</span><span class="sxs-lookup"><span data-stu-id="6b877-186">You've successfully configured Edison.</span></span>

## <a name="summary"></a><span data-ttu-id="6b877-187">Souhrn</span><span class="sxs-lookup"><span data-stu-id="6b877-187">Summary</span></span>
<span data-ttu-id="6b877-188">V tomto článku když jste se naučili jak tooassemble hello Tabule Edison, flash jeho firmwaru, instalační program heslo a připojte ho pomocí nástroje Konfigurace tooWi-Fi.</span><span class="sxs-lookup"><span data-stu-id="6b877-188">In this article, you’ve learned how tooassemble hello Edison board, flash its firmware, setup password and connect it tooWi-Fi by using configuration tool.</span></span> <span data-ttu-id="6b877-189">Všimněte si, že hello DIODU není ještě light nahoru.</span><span class="sxs-lookup"><span data-stu-id="6b877-189">Note that hello LED doesn't yet light up.</span></span> <span data-ttu-id="6b877-190">Další úlohou Hello je tooinstall hello nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na Edison.</span><span class="sxs-lookup"><span data-stu-id="6b877-190">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b877-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b877-191">Next steps</span></span>
<span data-ttu-id="6b877-192">[Získat nástroje hello][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="6b877-192">[Get hello tools][get-the-tools]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md