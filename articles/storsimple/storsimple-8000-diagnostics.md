---
title: "zařízení StorSimple 8000 tootroubleshoot nástroj aaaDiagnostics | Microsoft Docs"
description: "Popisuje režimy zařízení StorSimple hello a vysvětluje, jak toouse Windows Powershellu pro StorSimple toochange hello režim zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: e8b7fdbc44d2533973b63da841335ba73ba0014b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-diagnostics-tool-tootroubleshoot-8000-series-device-issues"></a><span data-ttu-id="debd9-103">Použít hello StorSimple Diagnostika tootroubleshoot 8000 řady zařízení problémy</span><span class="sxs-lookup"><span data-stu-id="debd9-103">Use hello StorSimple Diagnostics Tool tootroubleshoot 8000 series device issues</span></span>

## <a name="overview"></a><span data-ttu-id="debd9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="debd9-104">Overview</span></span>

<span data-ttu-id="debd9-105">Hello StorSimple Diagnostika diagnostikuje problémy související toosystem, výkon, sítě a stavu součásti hardwaru pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-105">hello StorSimple Diagnostics tool diagnoses issues related toosystem, performance, network, and hardware component health for a StorSimple device.</span></span> <span data-ttu-id="debd9-106">Nástroj diagnostiky Hello lze použít v různých situacích.</span><span class="sxs-lookup"><span data-stu-id="debd9-106">hello diagnostics tool can be used in various scenarios.</span></span> <span data-ttu-id="debd9-107">Mezi tyto scénáře patří úlohy plánování, nasazení zařízení StorSimple, vyhodnocování hello síťové prostředí a určení hello výkon provozní zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-107">These scenarios include workload planning, deploying a StorSimple device, assessing hello network environment, and determining hello performance of an operational device.</span></span> <span data-ttu-id="debd9-108">Tento článek obsahuje přehled hello Diagnostika a popisuje, jak nástroj hello lze použít s zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-108">This article provides an overview of hello diagnostics tool and describes how hello tool can be used with a StorSimple device.</span></span>

<span data-ttu-id="debd9-109">Nástroj diagnostiky Hello je primárně určený pro řadu zařízení StorSimple 8000 místní (8100 a 8600).</span><span class="sxs-lookup"><span data-stu-id="debd9-109">hello diagnostics tool is primarily intended for StorSimple 8000 series on-premises devices (8100 and 8600).</span></span>

## <a name="run-diagnostics-tool"></a><span data-ttu-id="debd9-110">Spustit diagnostické nástroje</span><span class="sxs-lookup"><span data-stu-id="debd9-110">Run diagnostics tool</span></span>

<span data-ttu-id="debd9-111">Tento nástroj můžete spustit pomocí rozhraní Windows PowerShell hello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-111">This tool can be run via hello Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="debd9-112">Existují dva způsoby tooaccess hello místního rozhraní zařízení:</span><span class="sxs-lookup"><span data-stu-id="debd9-112">There are two ways tooaccess hello local interface of your device:</span></span>

* <span data-ttu-id="debd9-113">[Konzole sériového portu toohello zařízení PuTTY tooconnect pro použití](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="debd9-113">[Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
* <span data-ttu-id="debd9-114">[Vzdálený přístup hello nástroj prostřednictvím hello Windows Powershellu pro StorSimple](storsimple-remote-connect.md).</span><span class="sxs-lookup"><span data-stu-id="debd9-114">[Remotely access hello tool via hello Windows PowerShell for StorSimple](storsimple-remote-connect.md).</span></span>

<span data-ttu-id="debd9-115">V tomto článku předpokládáme, že jste připojení toohello konzoly sériového portu zařízení prostřednictvím PuTTY.</span><span class="sxs-lookup"><span data-stu-id="debd9-115">In this article, we assume that you have connected toohello device serial console via PuTTY.</span></span>

#### <a name="toorun-hello-diagnostics-tool"></a><span data-ttu-id="debd9-116">Nástroj diagnostiky toorun hello</span><span class="sxs-lookup"><span data-stu-id="debd9-116">toorun hello diagnostics tool</span></span>

<span data-ttu-id="debd9-117">Po připojení rozhraní Windows PowerShell toohello hello zařízení, proveďte následující kroky toorun hello rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-117">Once you have connected toohello Windows PowerShell interface of hello device, perform hello following steps toorun hello cmdlet.</span></span>
1. <span data-ttu-id="debd9-118">Přihlaste se pomocí následujících kroků hello v konzole sériového portu zařízení toohello [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="debd9-118">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>

2. <span data-ttu-id="debd9-119">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="debd9-119">Type hello following command:</span></span>

    `Invoke-HcsDiagnostics`

    <span data-ttu-id="debd9-120">Pokud není zadán parametr oboru hello, spustí rutina hello hello diagnostické testy.</span><span class="sxs-lookup"><span data-stu-id="debd9-120">If hello scope parameter is not specified, hello cmdlet executes all hello diagnostic tests.</span></span> <span data-ttu-id="debd9-121">Tyto testy zahrnují systému, stav součásti hardwaru, síť a výkonu.</span><span class="sxs-lookup"><span data-stu-id="debd9-121">These tests include system, hardware component health, network, and performance.</span></span> 
    
    <span data-ttu-id="debd9-122">toorun pouze konkrétní test, zadejte parametr oboru hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-122">toorun only a specific test, specify hello scope parameter.</span></span> <span data-ttu-id="debd9-123">Například toorun pouze hello test sítě, typ</span><span class="sxs-lookup"><span data-stu-id="debd9-123">For instance, toorun only hello network test, type</span></span>

    `Invoke-HcsDiagnostics -Scope Network`

3. <span data-ttu-id="debd9-124">Vyberte a zkopírujte výstup hello z hello PuTTY okna do textového souboru k další analýze.</span><span class="sxs-lookup"><span data-stu-id="debd9-124">Select and copy hello output from hello PuTTY window into a text file for further analysis.</span></span>

## <a name="scenarios-toouse-hello-diagnostics-tool"></a><span data-ttu-id="debd9-125">Scénáře toouse hello diagnostického nástroje</span><span class="sxs-lookup"><span data-stu-id="debd9-125">Scenarios toouse hello diagnostics tool</span></span>

<span data-ttu-id="debd9-126">Použijte hello diagnostiky nástroj tootroubleshoot hello sítě, výkon, systém a hardwaru stav hello systému.</span><span class="sxs-lookup"><span data-stu-id="debd9-126">Use hello diagnostics tool tootroubleshoot hello network, performance, system, and hardware health of hello system.</span></span> <span data-ttu-id="debd9-127">Tady je několik možných scénářů:</span><span class="sxs-lookup"><span data-stu-id="debd9-127">Here are some possible scenarios:</span></span>

* <span data-ttu-id="debd9-128">**Zařízení do offline režimu** -zařízení řady StorSimple 8000 si je offline.</span><span class="sxs-lookup"><span data-stu-id="debd9-128">**Device offline** - Your StorSimple 8000 series device is offline.</span></span> <span data-ttu-id="debd9-129">Z rozhraní Windows PowerShell text hello, ale zdá, že jsou oba řadiče hello spuštěná.</span><span class="sxs-lookup"><span data-stu-id="debd9-129">However, from hello Windows PowerShell interface, it seems that both hello controllers are up and running.</span></span>
    * <span data-ttu-id="debd9-130">Tento nástroj můžete použít toothen určení stavu sítě hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-130">You can use this tool toothen determine hello network state.</span></span>
         
         > [!NOTE]
         > <span data-ttu-id="debd9-131">Nepoužívejte tento nástroj tooassess výkonu a nastavení sítě na zařízení než registrace hello (nebo konfigurace prostřednictvím Průvodce instalací).</span><span class="sxs-lookup"><span data-stu-id="debd9-131">Do not use this tool tooassess performance and network settings on a device before hello registration (or configuring via setup wizard).</span></span> <span data-ttu-id="debd9-132">Platnou IP je přiřazen toohello zařízení během registrace a Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="debd9-132">A valid IP is assigned toohello device during setup wizard and registration.</span></span> <span data-ttu-id="debd9-133">Tuto rutinu můžete spustit na zařízení, který není zaregistrován pro stav hardwaru a systému.</span><span class="sxs-lookup"><span data-stu-id="debd9-133">You can run this cmdlet, on a device that is not registered, for hardware health and system.</span></span> <span data-ttu-id="debd9-134">Používejte hello parametr rozsahu, například:</span><span class="sxs-lookup"><span data-stu-id="debd9-134">Use hello scope parameter, for example:</span></span>
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* <span data-ttu-id="debd9-135">**Problémy s trvalé zařízení** -dochází k problémům zařízení, které pravděpodobně toopersist.</span><span class="sxs-lookup"><span data-stu-id="debd9-135">**Persistent device issues** - You are experiencing device issues that seem toopersist.</span></span> <span data-ttu-id="debd9-136">Například registrace se nedaří.</span><span class="sxs-lookup"><span data-stu-id="debd9-136">For instance, registration is failing.</span></span> <span data-ttu-id="debd9-137">Můžete také vykazuje problémy zařízení po hello zařízení je úspěšně registrovaná a funkční nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="debd9-137">You could also be experiencing device issues after hello device is successfully registered and operational for a while.</span></span>
    * <span data-ttu-id="debd9-138">V takovém případě použijte tento nástroj pro předběžné odstraňování před přihlášením žádost o služby s Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="debd9-138">In this case, use this tool for preliminary troubleshooting before you log a service request with Microsoft Support.</span></span> <span data-ttu-id="debd9-139">Doporučujeme vám, že spustíte tento výstup hello nástroj a zachycení tento nástroj.</span><span class="sxs-lookup"><span data-stu-id="debd9-139">We recommend that you run this tool and capture hello output of this tool.</span></span> <span data-ttu-id="debd9-140">Potom můžete zadat Tento výstup tooSupport tooexpedite řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="debd9-140">You can then provide this output tooSupport tooexpedite troubleshooting.</span></span>
    * <span data-ttu-id="debd9-141">Pokud jsou všechny součásti nebo clusteru selhání hardwaru, je zapotřebí přihlásit v žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="debd9-141">If there are any hardware component or cluster failures, you should log in a Support request.</span></span>

* <span data-ttu-id="debd9-142">**Nedostatek výkonu zařízení** -zařízení StorSimple si je pomalé.</span><span class="sxs-lookup"><span data-stu-id="debd9-142">**Low device performance** - Your StorSimple device is slow.</span></span>
    * <span data-ttu-id="debd9-143">V takovém případě spusťte tuto rutinu s tooperformance sadu parametr oboru.</span><span class="sxs-lookup"><span data-stu-id="debd9-143">In this case, run this cmdlet with scope parameter set tooperformance.</span></span> <span data-ttu-id="debd9-144">Analýza výstup hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-144">Analyze hello output.</span></span> <span data-ttu-id="debd9-145">Získat hello cloudu latenci pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="debd9-145">You get hello cloud read-write latencies.</span></span> <span data-ttu-id="debd9-146">Použití hello hlášené zohlednit v některé zatížení pro vnitřní zpracování dat hello latence maximální dosažitelný cíl a pak nasadit hello zatížení systému hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-146">Use hello reported latencies as maximum achievable target, factor in some overhead for hello internal data processing, and then deploy hello workloads on hello system.</span></span> <span data-ttu-id="debd9-147">Další informace, přejděte příliš[použít hello sítě testování tootroubleshoot zařízení výkonu](#network-test).</span><span class="sxs-lookup"><span data-stu-id="debd9-147">For more information, go too[Use hello network test tootroubleshoot device performance](#network-test).</span></span>


## <a name="diagnostics-test-and-sample-outputs"></a><span data-ttu-id="debd9-148">Výstupy testů a ukázkové diagnostiky</span><span class="sxs-lookup"><span data-stu-id="debd9-148">Diagnostics test and sample outputs</span></span>

### <a name="hardware-test"></a><span data-ttu-id="debd9-149">Test na hardwaru</span><span class="sxs-lookup"><span data-stu-id="debd9-149">Hardware test</span></span>

<span data-ttu-id="debd9-150">Tento test Určuje stav hello hello hardwarové součásti, hello Seznam USM firmware a firmware disku hello spuštěny v systému.</span><span class="sxs-lookup"><span data-stu-id="debd9-150">This test determines hello status of hello hardware components, hello USM firmware, and hello disk firmware running on your system.</span></span>

* <span data-ttu-id="debd9-151">hardwarové součásti Hello hlášené jsou tyto součásti se nezdařilo hello testu nebo nejsou k dispozici v systému hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-151">hello hardware components reported are those components that failed hello test or are not present in hello system.</span></span>
* <span data-ttu-id="debd9-152">Hello Seznam USM firmwaru a disku verzí firmwaru jsou hlášené pro hello řadič 0, 1 řadiče a sdílené součásti v systému.</span><span class="sxs-lookup"><span data-stu-id="debd9-152">hello USM firmware and disk firmware versions are reported for hello Controller 0, Controller 1, and shared components in your system.</span></span> <span data-ttu-id="debd9-153">Úplný seznam hardwarové součásti přejděte na:</span><span class="sxs-lookup"><span data-stu-id="debd9-153">For a complete list of hardware components, go to:</span></span>

    * [<span data-ttu-id="debd9-154">Součásti v primární skříň</span><span class="sxs-lookup"><span data-stu-id="debd9-154">Components in primary enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [<span data-ttu-id="debd9-155">Součásti v EBOD skříň</span><span class="sxs-lookup"><span data-stu-id="debd9-155">Components in EBOD enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> <span data-ttu-id="debd9-156">Pokud test na hardwaru hello ohlásí selhání součásti, [protokolu v žádosti o služby se Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="debd9-156">If hello hardware test reports failed components, [log in a service request with Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a><span data-ttu-id="debd9-157">Ukázkový výstup spustí na zařízení s 8100 test na hardwaru</span><span class="sxs-lookup"><span data-stu-id="debd9-157">Sample output of hardware test run on an 8100 device</span></span>

<span data-ttu-id="debd9-158">Tady je ukázkový výstup ze zařízení StorSimple 8100.</span><span class="sxs-lookup"><span data-stu-id="debd9-158">Here is a sample output from a StorSimple 8100 device.</span></span> <span data-ttu-id="debd9-159">V zařízení 8100 modelu hello hello EBOD skříň není nainstalována.</span><span class="sxs-lookup"><span data-stu-id="debd9-159">In hello 8100 model device, hello EBOD enclosure is not present.</span></span> <span data-ttu-id="debd9-160">Proto nejsou hlášené hello EBOD řadiče součásti.</span><span class="sxs-lookup"><span data-stu-id="debd9-160">Hence, hello EBOD controller components are not reported.</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Hardware
Running hardware diagnostics ...
--------------------------------------------------
Hardware components failed or not present
----------------------

           Type           State      Controller           Index     EnclosureId
           ----           -----      ----------           -----     -----------
...rVipResource      NotPresent            None               1            None
...rVipResource      NotPresent            None               2            None
...rVipResource      NotPresent            None               3            None
...rVipResource      NotPresent            None               4            None
...rVipResource      NotPresent            None               5            None
...rVipResource      NotPresent            None               6            None
...rVipResource      NotPresent            None               7            None
...rVipResource      NotPresent            None               8            None
...rVipResource      NotPresent            None               9            None
...rVipResource      NotPresent            None              10            None
...rVipResource      NotPresent            None              11            None

Firmware information
----------------------
TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

--------------------------------------------------
```

### <a name="system-test"></a><span data-ttu-id="debd9-161">Testovací systém</span><span class="sxs-lookup"><span data-stu-id="debd9-161">System test</span></span>

<span data-ttu-id="debd9-162">Tento test sestav hello systémové informace, hello aktualizace k dispozici, informace o clusteru hello a informace o službě hello pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-162">This test reports hello system information, hello updates available, hello cluster information, and hello service information for your device.</span></span>

* <span data-ttu-id="debd9-163">informace o systému Hello zahrnuje hello modelu, sériové číslo zařízení, časové pásmo, stav řadiče a verze podrobné softwaru hello běžící na systému hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-163">hello system information includes hello model, device serial number, time zone, controller status, and hello detailed software version running on hello system.</span></span> <span data-ttu-id="debd9-164">hello toounderstand různé parametry systému hlášené jako výstup hello přejděte příliš[interpretace informace o systému](#appendix-interpreting-system-information).</span><span class="sxs-lookup"><span data-stu-id="debd9-164">toounderstand hello various system parameters reported as hello output, go too[Interpreting system information](#appendix-interpreting-system-information).</span></span>

* <span data-ttu-id="debd9-165">Dostupnost aktualizace Hello ohlásí, zda je k dispozici režimy hello regular a údržby a jejich názvy přidruženého balíčku.</span><span class="sxs-lookup"><span data-stu-id="debd9-165">hello update availability reports whether hello regular and maintenance modes are available and their associated package names.</span></span> <span data-ttu-id="debd9-166">Pokud `RegularUpdates` a `MaintenanceModeUpdates` jsou `false`, znamená to, zda text hello aktualizace nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="debd9-166">If `RegularUpdates` and `MaintenanceModeUpdates` are `false`, this indicates that hello updates are not available.</span></span> <span data-ttu-id="debd9-167">Zařízení je aktuální.</span><span class="sxs-lookup"><span data-stu-id="debd9-167">Your device is up-to-date.</span></span>
* <span data-ttu-id="debd9-168">informace o clusteru Hello obsahuje hello informace o různých logické součásti všechny skupiny clusteru HCS hello a jejich příslušné stavy.</span><span class="sxs-lookup"><span data-stu-id="debd9-168">hello cluster information contains hello information on various logical components of all hello HCS cluster groups and their respective statuses.</span></span> <span data-ttu-id="debd9-169">Pokud uvidíte skupinu offline clusteru v této části sestavy hello [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="debd9-169">If you see an offline cluster group in this section of hello report, [contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="debd9-170">informace o službě Hello zahrnuje hello názvy a stavy všech hello HCS a položek konfigurace služby spuštěné na zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-170">hello service information includes hello names and statuses of all hello HCS and CiS services running on your device.</span></span> <span data-ttu-id="debd9-171">Tyto informace jsou užitečné pro hello Microsoft Support při řešení potíží problém hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-171">This information is helpful for hello Microsoft Support in troubleshooting hello device issue.</span></span>

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a><span data-ttu-id="debd9-172">Ukázkový výstup testu systému spustí na zařízení s 8100</span><span class="sxs-lookup"><span data-stu-id="debd9-172">Sample output of system test run on an 8100 device</span></span>

<span data-ttu-id="debd9-173">Tady je ukázkový výstup testu systému hello spustí na zařízení s 8100.</span><span class="sxs-lookup"><span data-stu-id="debd9-173">Here is a sample output of hello system test run on an 8100 device.</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope System
Running system diagnostics ...
--------------------------------------------------

System information
----------------------
Controller0:

InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                : (UTC-08:00) Pacific Time (US & Canada)
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : Disabled
FipsMode                : Enabled

Controller1:
InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                :
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : HttpsAndHttpEnabled
FipsMode                : Enabled

Update availability
----------------------
RegularUpdates              : False
MaintenanceModeUpdates      : False
RegularUpdatesTitle         : {}
MaintenanceModeUpdatesTitle : {}

Cluster information
----------------------
Name                          State OwnerGroup
----                          ----- ----------
ApplicationHostRLUA           Online HCS Cluster Group
Data0v4                       Online HCS Cluster Group
HCS Vnic Resource             Online HCS Cluster Group
hcs_cloud_connectivity_...    Online HCS Cluster Group
hcs_controller_replacement    Online HCS Cluster Group
hcs_datapath_service          Online HCS Cluster Group
hcs_management_servic         Online HCS Cluster Group
hcs_nvram_service             Online HCS Cluster Group
hcs_passive_datapath          Online HCS Passive Cluster Group
hcs_platform_service          Online HCS Cluster Group
hcs_saas_agent_service        Online HCS Cluster Group
HddDataClusterDisk            Online HCS Cluster Group
HddMgmtClusterDisk            Online HCS Cluster Group
HddReplClusterDisk            Online HCS Cluster Group
iSCSI Target Server           Online HCS Cluster Group
NvramClusterDisk              Online HCS Cluster Group
SSAdminRLUA                   Online HCS Cluster Group
SsdDataClusterDisk            Online HCS Cluster Group
SsdNvramClusterDisk           Online HCS Cluster Group

Service information
----------------------
Name                                          Status DisplayName
----                                          ------ -----------
CiSAgentSvc                                   Stopped CiS Service Agent
hcs_cloud_connectivity_...                    Running hcs_cloud_connectivity...
hcs_controller_replacement                    Running HCS Controller Replace...
hcs_datapath_service                          Running HCS Datapath Service
hcs_management_service                        Running HCS Management Service
hcs_minishell                                 Running hcs_minishell
HCS_NVRAM_Service                             Running HCS NVRAM Service
hcs_passive_datapath                          Stopped HCS Passive Datapath S...
hcs_platform_service                          Running HCS Platform Monitor S...
hcs_saas_agent_service                        Running hcs_saas_agent_service
hcs_startup                                   Stopped hcs_startup
--------------------------------------------------
```

### <a name="network-test"></a><span data-ttu-id="debd9-174">Test sítě</span><span class="sxs-lookup"><span data-stu-id="debd9-174">Network test</span></span>

<span data-ttu-id="debd9-175">Tento test ověřuje stav hello hello síťových rozhraní, porty, DNS a NTP připojení k serveru, SSL certifikátů, přihlašovacích údajů účtu úložiště, servery pro aktualizaci toohello připojení a připojení k proxy serveru webu na zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-175">This test validates hello status of hello network interfaces, ports, DNS and NTP server connectivity, SSL certificate, storage account credentials, connectivity toohello Update servers, and web proxy connectivity on your StorSimple device.</span></span>

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a><span data-ttu-id="debd9-176">Ukázkový výstup sítě testů, pokud je povoleno pouze DATA0</span><span class="sxs-lookup"><span data-stu-id="debd9-176">Sample output of network test when only DATA0 is enabled</span></span>

<span data-ttu-id="debd9-177">Tady je ukázkový výstup hello zařízení 8100.</span><span class="sxs-lookup"><span data-stu-id="debd9-177">Here is a sample output of hello 8100 device.</span></span> <span data-ttu-id="debd9-178">Zobrazí se ve výstupu hello který:</span><span class="sxs-lookup"><span data-stu-id="debd9-178">You can see in hello output that:</span></span>
* <span data-ttu-id="debd9-179">Pouze DATA 0 a síťová rozhraní DATA 1 jsou povolené a nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="debd9-179">Only DATA 0 and DATA 1 network interfaces are enabled and configured.</span></span>
* <span data-ttu-id="debd9-180">DATA 2 až 5 nejsou povolené hello portálu.</span><span class="sxs-lookup"><span data-stu-id="debd9-180">DATA 2 - 5 are not enabled in hello portal.</span></span>
* <span data-ttu-id="debd9-181">Konfigurace serveru DNS Hello je platný a hello zařízení můžete připojit přes hello DNS server.</span><span class="sxs-lookup"><span data-stu-id="debd9-181">hello DNS server configuration is valid and hello device can connect via hello DNS server.</span></span>
* <span data-ttu-id="debd9-182">připojení k serveru NTP Hello je také v pořádku.</span><span class="sxs-lookup"><span data-stu-id="debd9-182">hello NTP server connectivity is also fine.</span></span>
* <span data-ttu-id="debd9-183">Jsou otevřené porty 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="debd9-183">Ports 80 and 443 are open.</span></span> <span data-ttu-id="debd9-184">Port 9354 však je blokován.</span><span class="sxs-lookup"><span data-stu-id="debd9-184">However, port 9354 is blocked.</span></span> <span data-ttu-id="debd9-185">Podle hello [požadavky na systém sítě](storsimple-system-requirements.md), musíte tento port pro tooopen pro hello service bus komunikaci.</span><span class="sxs-lookup"><span data-stu-id="debd9-185">Based on hello [system network requirements](storsimple-system-requirements.md), you need tooopen this port for hello service bus communication.</span></span>
* <span data-ttu-id="debd9-186">certifikáty SSL Hello je platný.</span><span class="sxs-lookup"><span data-stu-id="debd9-186">hello SSL certification is valid.</span></span>
* <span data-ttu-id="debd9-187">Hello zařízení může připojit účet úložiště toohello: _myss8000storageacct_.</span><span class="sxs-lookup"><span data-stu-id="debd9-187">hello device can connect toohello storage account: _myss8000storageacct_.</span></span>
* <span data-ttu-id="debd9-188">servery tooUpdate připojení Hello je platný.</span><span class="sxs-lookup"><span data-stu-id="debd9-188">hello connectivity tooUpdate servers is valid.</span></span>
* <span data-ttu-id="debd9-189">na toto zařízení není nakonfigurován Hello webový proxy server.</span><span class="sxs-lookup"><span data-stu-id="debd9-189">hello web proxy is not configured on this device.</span></span>

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a><span data-ttu-id="debd9-190">Ukázkový výstup testu sítě, pokud jsou povolené DATA0 a DATA1</span><span class="sxs-lookup"><span data-stu-id="debd9-190">Sample output of network test when DATA0 and DATA1 are enabled</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Network
Running network diagnostics ....
--------------------------------------------------
Validating networks .....
Name                Entity              Result              Details
----                ------              ------              -------
Network interface   Data0               Valid
Network interface   Data1               Valid
Network interface   Data2               Not enabled
Network interface   Data3               Not enabled
Network interface   Data4               Not enabled
Network interface   Data5               Not enabled
DNS                 10.222.118.154      Valid
NTP                 time.windows.com    Valid
Port                80                  Open
Port                443                 Open
Port                9354                Blocked
SSL certificate     https://myss8000... Valid
Storage account ... myss8000storageacct Valid
URL                 http://download.... Valid
URL                 http://download.... Valid
Web proxy                               Not enabled         Web proxy is not...
--------------------------------------------------
```

### <a name="performance-test"></a><span data-ttu-id="debd9-191">Test výkonnosti</span><span class="sxs-lookup"><span data-stu-id="debd9-191">Performance test</span></span>

<span data-ttu-id="debd9-192">Tento test hlásí hello cloudu výkonu prostřednictvím latenci pro čtení a zápis hello cloudu pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-192">This test reports hello cloud performance via hello cloud read-write latencies for your device.</span></span> <span data-ttu-id="debd9-193">Tento nástroj se dá použít tooestablish základní úroveň výkonu hello cloudu, který můžete dosáhnout s StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-193">This tool can be used tooestablish a baseline of hello cloud performance that you can achieve with StorSimple.</span></span> <span data-ttu-id="debd9-194">Hello nástroj sestavy hello maximální výkon (nejlepší scénář pro latenci pro čtení a zápis), které můžete získat pro připojení.</span><span class="sxs-lookup"><span data-stu-id="debd9-194">hello tool reports hello maximum performance (best case scenario for read-write latencies) that you can get for your connection.</span></span>

<span data-ttu-id="debd9-195">Jak nástroj hello sestavy hello maximální možná výkonu, můžeme použít hello oznamuje latenci pro čtení a zápis jako cíle při nasazování hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="debd9-195">As hello tool reports hello maximum achievable performance, we can use hello reported read-write latencies as targets when deploying hello workloads.</span></span>

<span data-ttu-id="debd9-196">Hello test simuluje velikosti objektu blob hello přidružené typy hello jiný svazek v zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-196">hello test simulates hello blob sizes associated with hello different volume types on hello device.</span></span> <span data-ttu-id="debd9-197">Víceúrovňová Regular a zálohování místně vázaných svazků velikost objektu blob 64 KB.</span><span class="sxs-lookup"><span data-stu-id="debd9-197">Regular tiered and backups of locally pinned volumes use a 64 KB blob size.</span></span> <span data-ttu-id="debd9-198">Vrstvené svazky s možností archivu zaškrtnuta možnost použít velikost dat blob 512 KB.</span><span class="sxs-lookup"><span data-stu-id="debd9-198">Tiered volumes with archive option checked use 512 KB blob data size.</span></span> <span data-ttu-id="debd9-199">Pokud vaše zařízení má vrstvené a místně vázaných svazků konfigurace, pouze hello testovací odpovídající too64 KB objekt blob, který je velikost dat spustit.</span><span class="sxs-lookup"><span data-stu-id="debd9-199">If your device has tiered and locally pinned volumes configured, only hello test corresponding too64 KB blob data size is run.</span></span>

<span data-ttu-id="debd9-200">toouse tento nástroj, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="debd9-200">toouse this tool, perform hello following steps:</span></span>

1.  <span data-ttu-id="debd9-201">Nejprve vytvořte směs vrstvené svazky a vrstvené svazky s archivovaný možnost zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="debd9-201">First, create a mix of tiered volumes and tiered volumes with archived option checked.</span></span> <span data-ttu-id="debd9-202">Tato akce zajistí, že tento nástroj hello spustí testy hello 64 KB a 512 KB velikosti objektu blob.</span><span class="sxs-lookup"><span data-stu-id="debd9-202">This action ensures that hello tool runs hello tests for both 64 KB and 512 KB blob sizes.</span></span>

2. <span data-ttu-id="debd9-203">Spusťte rutinu hello po po vytvoření a konfiguraci hello svazky.</span><span class="sxs-lookup"><span data-stu-id="debd9-203">Run hello cmdlet after you have created and configured hello volumes.</span></span> <span data-ttu-id="debd9-204">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="debd9-204">Type:</span></span>

    `Invoke-HcsDiagnostics -Scope Performance`

3. <span data-ttu-id="debd9-205">Poznamenejte si latenci pro čtení a zápis hello hlášené nástrojem hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-205">Make a note of hello read-write latencies reported by hello tool.</span></span> <span data-ttu-id="debd9-206">Tento test může trvat několik minut toorun předtím, než se oznámí hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="debd9-206">This test can take several minutes toorun before it reports hello results.</span></span>

4. <span data-ttu-id="debd9-207">Pokud latence připojení hello všechny pod hello očekávaný rozsah a potom hello latence hlášené hello nástroj lze použít jako maximální dosažitelný cíl při nasazování hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="debd9-207">If hello connection latencies are all under hello expected range, then hello latencies reported by hello tool can be used as maximum achievable target when deploying hello workloads.</span></span> <span data-ttu-id="debd9-208">Dvoufaktorového v některé zatížení pro vnitřní zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="debd9-208">Factor in some overhead for internal data processing.</span></span>

    <span data-ttu-id="debd9-209">Pokud latence pro čtení a zápis hello hlášené jsou vysoké hello diagnostického nástroje:</span><span class="sxs-lookup"><span data-stu-id="debd9-209">If hello read-write latencies reported by hello diagnostics tool are high:</span></span>

    1. <span data-ttu-id="debd9-210">Konfigurace pro služby blob Storage Analytics a analyzujte hello výstup toounderstand hello latence pro hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="debd9-210">Configure Storage Analytics for blob services and analyze hello output toounderstand hello latencies for hello Azure storage account.</span></span> <span data-ttu-id="debd9-211">Podrobné pokyny najdete příliš[povolení a konfigurace úložiště analýzy](../storage/common/storage-enable-and-view-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="debd9-211">For detailed instructions, go too[enable and configure Storage Analytics](../storage/common/storage-enable-and-view-metrics.md).</span></span> <span data-ttu-id="debd9-212">Pokud jsou tyto latence také vysoké a porovnatelný z hlediska toohello čísla, který jste dostali od hello StorSimple Diagnostika, musíte toolog žádost o služby s Azure storage.</span><span class="sxs-lookup"><span data-stu-id="debd9-212">If those latencies are also high and comparable toohello numbers you received from hello StorSimple Diagnostics tool, then you need toolog a service request with Azure storage.</span></span>

    2. <span data-ttu-id="debd9-213">Pokud jsou nízkou latenci účet úložiště hello, obraťte se na tooinvestigate správce vaší sítě žádné latence problémy ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="debd9-213">If hello storage account latencies are low, contact your network administrator tooinvestigate any latency issues in your network.</span></span>

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a><span data-ttu-id="debd9-214">Ukázkový výstup testu výkonnosti spustí na zařízení s 8100</span><span class="sxs-lookup"><span data-stu-id="debd9-214">Sample output of performance test run on an 8100 device</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Performance
Running performance diagnostics...
--------------------------------------------------
Cloud performance: writing blobs
Cloud write latency: 194 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: reading blobs..
Cloud read latency: 544 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: writing blobs.
Cloud write latency: 369 ms using credential 'myss8000storageacct', blob size '512KB'
Cloud performance: reading blobs...
Cloud read latency: 4924 ms using credential 'myss8000storageacct', blob size '512KB'
--------------------------------------------------
Controller0>
```

## <a name="appendix-interpreting-system-information"></a><span data-ttu-id="debd9-215">Dodatek: interpretace informace o systému</span><span class="sxs-lookup"><span data-stu-id="debd9-215">Appendix: interpreting system information</span></span>

<span data-ttu-id="debd9-216">Zde je tabulku popisující jaké hello mapovat různé parametry prostředí Windows PowerShell hello systémové informace.</span><span class="sxs-lookup"><span data-stu-id="debd9-216">Here is a table describing what hello various Windows PowerShell parameters in hello system information map to.</span></span> 

| <span data-ttu-id="debd9-217">Parametr prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="debd9-217">PowerShell Parameter</span></span>    | <span data-ttu-id="debd9-218">Popis</span><span class="sxs-lookup"><span data-stu-id="debd9-218">Description</span></span>  |
|-------------------------|------------------|
| <span data-ttu-id="debd9-219">ID instance</span><span class="sxs-lookup"><span data-stu-id="debd9-219">Instance ID</span></span>             | <span data-ttu-id="debd9-220">Každý řadič má jedinečný identifikátor nebo identifikátor GUID s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="debd9-220">Every controller has a unique identifier or a GUID associated with it.</span></span>|
| <span data-ttu-id="debd9-221">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="debd9-221">Name</span></span>                    | <span data-ttu-id="debd9-222">Hello popisný název zařízení hello jako nakonfigurovaný pomocí portálu Azure hello během nasazování zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-222">hello friendly name of hello device as configured through hello Azure portal during device deployment.</span></span> <span data-ttu-id="debd9-223">popisný název výchozí Hello je sériové číslo zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-223">hello default friendly name is hello device serial number.</span></span> |
| <span data-ttu-id="debd9-224">Model</span><span class="sxs-lookup"><span data-stu-id="debd9-224">Model</span></span>                   | <span data-ttu-id="debd9-225">Hello model zařízení řady StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="debd9-225">hello model of your StorSimple 8000 series device.</span></span> <span data-ttu-id="debd9-226">Hello modelu může být 8100 nebo 8600.</span><span class="sxs-lookup"><span data-stu-id="debd9-226">hello model can be 8100 or 8600.</span></span>|
| <span data-ttu-id="debd9-227">sériové číslo</span><span class="sxs-lookup"><span data-stu-id="debd9-227">SerialNumber</span></span>            | <span data-ttu-id="debd9-228">sériové číslo zařízení Hello je přiřazena na hello objekt pro vytváření a 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="debd9-228">hello device serial number is assigned at hello factory and is 15 characters long.</span></span> <span data-ttu-id="debd9-229">Například 8600 SHX0991003G44HT určuje:</span><span class="sxs-lookup"><span data-stu-id="debd9-229">For instance, 8600-SHX0991003G44HT indicates:</span></span><br> <span data-ttu-id="debd9-230">8600 – je model zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-230">8600 – Is hello device model.</span></span><br><span data-ttu-id="debd9-231">TVX – je hello výrobní lokality.</span><span class="sxs-lookup"><span data-stu-id="debd9-231">SHX – Is hello manufacturing site.</span></span><br> <span data-ttu-id="debd9-232">0991003 - je konkrétní produkt.</span><span class="sxs-lookup"><span data-stu-id="debd9-232">0991003 - Is a specific product.</span></span> <br> <span data-ttu-id="debd9-233">Sériová čísla jedinečná toocreate zvýší G44HT-hello jsou posledních 5 číslic.</span><span class="sxs-lookup"><span data-stu-id="debd9-233">G44HT- hello last 5 digits are incremented toocreate unique serial numbers.</span></span> <span data-ttu-id="debd9-234">Toto nemusí být sekvenční sada.</span><span class="sxs-lookup"><span data-stu-id="debd9-234">This may not be a sequential set.</span></span>|
| <span data-ttu-id="debd9-235">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="debd9-235">TimeZone</span></span>                | <span data-ttu-id="debd9-236">Hello zařízení časové pásmo jako nakonfigurovaný v hello portál Azure během nasazování zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-236">hello device time zone as configured in hello Azure portal during device deployment.</span></span>|
| <span data-ttu-id="debd9-237">CurrentController</span><span class="sxs-lookup"><span data-stu-id="debd9-237">CurrentController</span></span>       | <span data-ttu-id="debd9-238">Hello řadiče, že jste rozhraní Windows PowerShell připojené toothrough hello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-238">hello controller that you are connected toothrough hello Windows PowerShell interface of your StorSimple device.</span></span>|
| <span data-ttu-id="debd9-239">ActiveController</span><span class="sxs-lookup"><span data-stu-id="debd9-239">ActiveController</span></span>        | <span data-ttu-id="debd9-240">Hello řadiče, který je aktivní na vašem zařízení a řídí všechny operace hello sítě a disku.</span><span class="sxs-lookup"><span data-stu-id="debd9-240">hello controller that is active on your device and is controlling all hello network and disk operations.</span></span> <span data-ttu-id="debd9-241">To může být řadič 0 nebo 1 řadiče.</span><span class="sxs-lookup"><span data-stu-id="debd9-241">This can be Controller 0 or Controller 1.</span></span>  |
| <span data-ttu-id="debd9-242">Controller0Status</span><span class="sxs-lookup"><span data-stu-id="debd9-242">Controller0Status</span></span>       | <span data-ttu-id="debd9-243">Stav Hello řadič 0 na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-243">hello status of Controller 0 on your device.</span></span> <span data-ttu-id="debd9-244">Stav řadiče Hello může být normálního v režimu obnovení, nebo je nedostupný.</span><span class="sxs-lookup"><span data-stu-id="debd9-244">hello controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="debd9-245">Controller1Status</span><span class="sxs-lookup"><span data-stu-id="debd9-245">Controller1Status</span></span>       | <span data-ttu-id="debd9-246">Hello stav řadič 1 na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-246">hello status of Controller 1 on your device.</span></span>  <span data-ttu-id="debd9-247">Stav řadiče Hello může být normálního v režimu obnovení, nebo je nedostupný.</span><span class="sxs-lookup"><span data-stu-id="debd9-247">hello controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="debd9-248">SystemMode</span><span class="sxs-lookup"><span data-stu-id="debd9-248">SystemMode</span></span>              | <span data-ttu-id="debd9-249">Hello celkového stavu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-249">hello overall status of your StorSimple device.</span></span> <span data-ttu-id="debd9-250">Stav zařízení Hello může být normální údržby, nebo vyřazení (odpovídá toodeactivated v hello portál Azure).</span><span class="sxs-lookup"><span data-stu-id="debd9-250">hello device status can be normal, maintenance, or decommissioned (corresponds toodeactivated in hello Azure portal).</span></span>|
| <span data-ttu-id="debd9-251">FriendlySoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="debd9-251">FriendlySoftwareVersion</span></span> | <span data-ttu-id="debd9-252">Hello popisný řetězec, který odpovídá verzi softwaru toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-252">hello friendly string that corresponds toohello device software version.</span></span> <span data-ttu-id="debd9-253">U systému, který používá aktualizace 4 verze hello popisný softwaru bude StorSimple 8000 řady aktualizace 4.0.</span><span class="sxs-lookup"><span data-stu-id="debd9-253">For a system running Update 4, hello friendly software version would be StorSimple 8000 Series Update 4.0.</span></span>|
| <span data-ttu-id="debd9-254">HcsSoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="debd9-254">HcsSoftwareVersion</span></span>      | <span data-ttu-id="debd9-255">verze softwaru HCS Hello spuštěné na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-255">hello HCS software version running on your device.</span></span> <span data-ttu-id="debd9-256">Například hello HCS software verze odpovídající tooStorSimple 8000 je 6.3.9600.17820 4.0 Update řady.</span><span class="sxs-lookup"><span data-stu-id="debd9-256">For instance, hello HCS software version corresponding tooStorSimple 8000 Series Update 4.0 is 6.3.9600.17820.</span></span> |
| <span data-ttu-id="debd9-257">ApiVersion</span><span class="sxs-lookup"><span data-stu-id="debd9-257">ApiVersion</span></span>              | <span data-ttu-id="debd9-258">verze softwaru Hello hello rozhraní API prostředí PowerShell systému Windows hello HCS zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-258">hello software version of hello Windows PowerShell API of hello HCS device.</span></span>|
| <span data-ttu-id="debd9-259">VhdVersion</span><span class="sxs-lookup"><span data-stu-id="debd9-259">VhdVersion</span></span>              | <span data-ttu-id="debd9-260">verze softwaru Hello hello tovární image, která hello zařízení byl součástí.</span><span class="sxs-lookup"><span data-stu-id="debd9-260">hello software version of hello factory image that hello device was shipped with.</span></span> <span data-ttu-id="debd9-261">Pokud obnovíte výchozí nastavení vašeho zařízení toofactory, pak spustí tato verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="debd9-261">If you reset your device toofactory defaults, then it runs this software version.</span></span>|
| <span data-ttu-id="debd9-262">OSVersion</span><span class="sxs-lookup"><span data-stu-id="debd9-262">OSVersion</span></span>               | <span data-ttu-id="debd9-263">verze softwaru Hello spuštěné na zařízení hello hello operační systém Windows Server.</span><span class="sxs-lookup"><span data-stu-id="debd9-263">hello software version of hello Windows Server operating system running on hello device.</span></span> <span data-ttu-id="debd9-264">zařízení StorSimple Hello je založena na Windows Server 2012 R2, která odpovídá too6.3.9600 hello.</span><span class="sxs-lookup"><span data-stu-id="debd9-264">hello StorSimple device is based off hello Windows Server 2012 R2 that corresponds too6.3.9600.</span></span>|
| <span data-ttu-id="debd9-265">CisAgentVersion</span><span class="sxs-lookup"><span data-stu-id="debd9-265">CisAgentVersion</span></span>         | <span data-ttu-id="debd9-266">Hello verze pro agenta položek konfigurace pro spuštění v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-266">hello version for your Cis agent running on your StorSimple device.</span></span> <span data-ttu-id="debd9-267">Tento agent pomáhá komunikovat se službou StorSimple Manager hello běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="debd9-267">This agent helps communicate with hello StorSimple Manager service running in Azure.</span></span>|
| <span data-ttu-id="debd9-268">MdsAgentVersion</span><span class="sxs-lookup"><span data-stu-id="debd9-268">MdsAgentVersion</span></span>         | <span data-ttu-id="debd9-269">Hello verze odpovídající toohello Mds agenta spuštěného v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-269">hello version corresponding toohello Mds agent running on your StorSimple device.</span></span> <span data-ttu-id="debd9-270">Tento agent přesune data toohello monitorování a Diagnostika služby MDS ().</span><span class="sxs-lookup"><span data-stu-id="debd9-270">This agent moves data toohello Monitoring and Diagnostics Service (MDS).</span></span>|
| <span data-ttu-id="debd9-271">Lsisas2Version</span><span class="sxs-lookup"><span data-stu-id="debd9-271">Lsisas2Version</span></span>          | <span data-ttu-id="debd9-272">Hello verze odpovídající toohello LSI ovladače zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-272">hello version corresponding toohello LSI drivers on your StorSimple device.</span></span>|
| <span data-ttu-id="debd9-273">Kapacita</span><span class="sxs-lookup"><span data-stu-id="debd9-273">Capacity</span></span>                | <span data-ttu-id="debd9-274">Celková kapacita Hello hello zařízení v bajtech.</span><span class="sxs-lookup"><span data-stu-id="debd9-274">hello total capacity of hello device in bytes.</span></span>|
| <span data-ttu-id="debd9-275">RemoteManagementMode</span><span class="sxs-lookup"><span data-stu-id="debd9-275">RemoteManagementMode</span></span>    | <span data-ttu-id="debd9-276">Určuje, zda hello zařízení můžete vzdáleně spravovat přes jeho rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="debd9-276">Indicates whether hello device can be remotely managed via its Windows PowerShell interface.</span></span> |
| <span data-ttu-id="debd9-277">FipsMode</span><span class="sxs-lookup"><span data-stu-id="debd9-277">FipsMode</span></span>                | <span data-ttu-id="debd9-278">Určuje, zda hello Spojených států informace o zpracování Standard FIPS (Federal) režimu povolena na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="debd9-278">Indicates whether hello United States Federal Information Processing Standard (FIPS) mode is enabled on your device.</span></span> <span data-ttu-id="debd9-279">standard Hello FIPS 140 definuje kryptografické algoritmy pro použití schváleno nám Federal government počítačových systémů hello ochranu citlivá data.</span><span class="sxs-lookup"><span data-stu-id="debd9-279">hello FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for hello protection of sensitive data.</span></span> <span data-ttu-id="debd9-280">Pro zařízení se systémem Update 4 nebo novější je ve výchozím nastavení povolen režim FIPS.</span><span class="sxs-lookup"><span data-stu-id="debd9-280">For devices running Update 4 or later, FIPS mode is enabled by default.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="debd9-281">Další kroky</span><span class="sxs-lookup"><span data-stu-id="debd9-281">Next steps</span></span>

* <span data-ttu-id="debd9-282">Další informace hello [syntaxe rutiny hello Invoke-HcsDiagnostics](https://technet.microsoft.com/library/mt795371.aspx).</span><span class="sxs-lookup"><span data-stu-id="debd9-282">Learn hello [syntax of hello Invoke-HcsDiagnostics cmdlet](https://technet.microsoft.com/library/mt795371.aspx).</span></span>

* <span data-ttu-id="debd9-283">Další informace o příliš[potíží s nasazením](storsimple-troubleshoot-deployment.md) zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="debd9-283">Learn more about how too[troubleshoot deployment issues](storsimple-troubleshoot-deployment.md) on your StorSimple device.</span></span>
