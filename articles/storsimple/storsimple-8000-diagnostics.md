---
title: "Nástroj diagnostiky pro zařízení StorSimple 8000 Poradce při potížích | Microsoft Docs"
description: "Popisuje režimy zařízení StorSimple a vysvětluje, jak pomocí prostředí Windows PowerShell pro StorSimple ke změně režimu zařízení."
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
ms.openlocfilehash: 8fae7bb357f8e5e8eff249edfe3a2aaafe04283c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-storsimple-diagnostics-tool-to-troubleshoot-8000-series-device-issues"></a><span data-ttu-id="3c859-103">Nástroj diagnostiky StorSimple potíží pomocí 8000 řady zařízení</span><span class="sxs-lookup"><span data-stu-id="3c859-103">Use the StorSimple Diagnostics Tool to troubleshoot 8000 series device issues</span></span>

## <a name="overview"></a><span data-ttu-id="3c859-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3c859-104">Overview</span></span>

<span data-ttu-id="3c859-105">Nástroj diagnostiky StorSimple diagnostikuje problémy související s stav součásti systému, výkonu, sítě a hardwaru pro zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-105">The StorSimple Diagnostics tool diagnoses issues related to system, performance, network, and hardware component health for a StorSimple device.</span></span> <span data-ttu-id="3c859-106">Nástroj diagnostiky lze použít v různých situacích.</span><span class="sxs-lookup"><span data-stu-id="3c859-106">The diagnostics tool can be used in various scenarios.</span></span> <span data-ttu-id="3c859-107">Mezi tyto scénáře patří úlohy plánování, nasazení zařízení StorSimple, vyhodnocovat síťové prostředí a určení výkon provozní zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-107">These scenarios include workload planning, deploying a StorSimple device, assessing the network environment, and determining the performance of an operational device.</span></span> <span data-ttu-id="3c859-108">Tento článek obsahuje přehled nástroje pro diagnostiku a popisuje, jak nástroj lze použít s zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-108">This article provides an overview of the diagnostics tool and describes how the tool can be used with a StorSimple device.</span></span>

<span data-ttu-id="3c859-109">Nástroj diagnostiky je primárně určený pro řadu zařízení StorSimple 8000 místní (8100 a 8600).</span><span class="sxs-lookup"><span data-stu-id="3c859-109">The diagnostics tool is primarily intended for StorSimple 8000 series on-premises devices (8100 and 8600).</span></span>

## <a name="run-diagnostics-tool"></a><span data-ttu-id="3c859-110">Spustit diagnostické nástroje</span><span class="sxs-lookup"><span data-stu-id="3c859-110">Run diagnostics tool</span></span>

<span data-ttu-id="3c859-111">Tento nástroj můžete spustit pomocí rozhraní Windows PowerShell zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-111">This tool can be run via the Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="3c859-112">Existují dva způsoby pro přístup k místní rozhraní zařízení:</span><span class="sxs-lookup"><span data-stu-id="3c859-112">There are two ways to access the local interface of your device:</span></span>

* <span data-ttu-id="3c859-113">[Použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="3c859-113">[Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
* <span data-ttu-id="3c859-114">[Vzdálený přístup nástroj prostřednictvím Windows Powershellu pro StorSimple](storsimple-remote-connect.md).</span><span class="sxs-lookup"><span data-stu-id="3c859-114">[Remotely access the tool via the Windows PowerShell for StorSimple](storsimple-remote-connect.md).</span></span>

<span data-ttu-id="3c859-115">V tomto článku předpokládáme, že jste připojení ke konzole sériového portu zařízení prostřednictvím PuTTY.</span><span class="sxs-lookup"><span data-stu-id="3c859-115">In this article, we assume that you have connected to the device serial console via PuTTY.</span></span>

#### <a name="to-run-the-diagnostics-tool"></a><span data-ttu-id="3c859-116">Chcete-li spustit nástroj diagnostiky</span><span class="sxs-lookup"><span data-stu-id="3c859-116">To run the diagnostics tool</span></span>

<span data-ttu-id="3c859-117">Jakmile se připojíte k rozhraní Windows PowerShell zařízení, proveďte následující kroky ke spuštění rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c859-117">Once you have connected to the Windows PowerShell interface of the device, perform the following steps to run the cmdlet.</span></span>
1. <span data-ttu-id="3c859-118">Přihlaste se k konzole sériového portu zařízení podle pokynů v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="3c859-118">Log on to the device serial console by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>

2. <span data-ttu-id="3c859-119">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3c859-119">Type the following command:</span></span>

    `Invoke-HcsDiagnostics`

    <span data-ttu-id="3c859-120">Pokud není zadán parametr oboru, rutina provede všechny diagnostické testy.</span><span class="sxs-lookup"><span data-stu-id="3c859-120">If the scope parameter is not specified, the cmdlet executes all the diagnostic tests.</span></span> <span data-ttu-id="3c859-121">Tyto testy zahrnují systému, stav součásti hardwaru, síť a výkonu.</span><span class="sxs-lookup"><span data-stu-id="3c859-121">These tests include system, hardware component health, network, and performance.</span></span> 
    
    <span data-ttu-id="3c859-122">Pokud chcete spustit pouze konkrétní test, zadejte parametr oboru.</span><span class="sxs-lookup"><span data-stu-id="3c859-122">To run only a specific test, specify the scope parameter.</span></span> <span data-ttu-id="3c859-123">Například spusťte pouze test sítě, zadejte</span><span class="sxs-lookup"><span data-stu-id="3c859-123">For instance, to run only the network test, type</span></span>

    `Invoke-HcsDiagnostics -Scope Network`

3. <span data-ttu-id="3c859-124">Vyberte a zkopírujte výstup z okna PuTTY do textového souboru k další analýze.</span><span class="sxs-lookup"><span data-stu-id="3c859-124">Select and copy the output from the PuTTY window into a text file for further analysis.</span></span>

## <a name="scenarios-to-use-the-diagnostics-tool"></a><span data-ttu-id="3c859-125">Scénáře použití nástroje diagnostiky</span><span class="sxs-lookup"><span data-stu-id="3c859-125">Scenarios to use the diagnostics tool</span></span>

<span data-ttu-id="3c859-126">Použijte nástroj Diagnostika k řešení potíží se stavem sítě, výkon, systém a hardware systému.</span><span class="sxs-lookup"><span data-stu-id="3c859-126">Use the diagnostics tool to troubleshoot the network, performance, system, and hardware health of the system.</span></span> <span data-ttu-id="3c859-127">Tady je několik možných scénářů:</span><span class="sxs-lookup"><span data-stu-id="3c859-127">Here are some possible scenarios:</span></span>

* <span data-ttu-id="3c859-128">**Zařízení do offline režimu** -zařízení řady StorSimple 8000 si je offline.</span><span class="sxs-lookup"><span data-stu-id="3c859-128">**Device offline** - Your StorSimple 8000 series device is offline.</span></span> <span data-ttu-id="3c859-129">Však z rozhraní Windows PowerShell, zdá se, že jsou oběma řadičům spuštěná.</span><span class="sxs-lookup"><span data-stu-id="3c859-129">However, from the Windows PowerShell interface, it seems that both the controllers are up and running.</span></span>
    * <span data-ttu-id="3c859-130">Tento nástroj můžete pak určit stav sítě.</span><span class="sxs-lookup"><span data-stu-id="3c859-130">You can use this tool to then determine the network state.</span></span>
         
         > [!NOTE]
         > <span data-ttu-id="3c859-131">Nepoužívejte tento nástroj pro vyhodnocení nastavení sítě a výkonu v zařízení než registrace (nebo konfigurace prostřednictvím Průvodce instalací).</span><span class="sxs-lookup"><span data-stu-id="3c859-131">Do not use this tool to assess performance and network settings on a device before the registration (or configuring via setup wizard).</span></span> <span data-ttu-id="3c859-132">Platnou IP je přiřazený k zařízení během registrace a Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="3c859-132">A valid IP is assigned to the device during setup wizard and registration.</span></span> <span data-ttu-id="3c859-133">Tuto rutinu můžete spustit na zařízení, který není zaregistrován pro stav hardwaru a systému.</span><span class="sxs-lookup"><span data-stu-id="3c859-133">You can run this cmdlet, on a device that is not registered, for hardware health and system.</span></span> <span data-ttu-id="3c859-134">Používejte parametr oboru, například:</span><span class="sxs-lookup"><span data-stu-id="3c859-134">Use the scope parameter, for example:</span></span>
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* <span data-ttu-id="3c859-135">**Problémy s trvalé zařízení** -dochází k zařízení problémy, které se zdá, že zachovat.</span><span class="sxs-lookup"><span data-stu-id="3c859-135">**Persistent device issues** - You are experiencing device issues that seem to persist.</span></span> <span data-ttu-id="3c859-136">Například registrace se nedaří.</span><span class="sxs-lookup"><span data-stu-id="3c859-136">For instance, registration is failing.</span></span> <span data-ttu-id="3c859-137">Můžete také vykazuje problémy zařízení po zařízení je úspěšně registrovaná a funkční nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="3c859-137">You could also be experiencing device issues after the device is successfully registered and operational for a while.</span></span>
    * <span data-ttu-id="3c859-138">V takovém případě použijte tento nástroj pro předběžné odstraňování před přihlášením žádost o služby s Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="3c859-138">In this case, use this tool for preliminary troubleshooting before you log a service request with Microsoft Support.</span></span> <span data-ttu-id="3c859-139">Doporučujeme, abyste tento nástroj spustit a zachycení výstup tohoto nástroje.</span><span class="sxs-lookup"><span data-stu-id="3c859-139">We recommend that you run this tool and capture the output of this tool.</span></span> <span data-ttu-id="3c859-140">Potom můžete zadat Tento výstup pro podporu, aby urychlit řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="3c859-140">You can then provide this output to Support to expedite troubleshooting.</span></span>
    * <span data-ttu-id="3c859-141">Pokud jsou všechny součásti nebo clusteru selhání hardwaru, je zapotřebí přihlásit v žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="3c859-141">If there are any hardware component or cluster failures, you should log in a Support request.</span></span>

* <span data-ttu-id="3c859-142">**Nedostatek výkonu zařízení** -zařízení StorSimple si je pomalé.</span><span class="sxs-lookup"><span data-stu-id="3c859-142">**Low device performance** - Your StorSimple device is slow.</span></span>
    * <span data-ttu-id="3c859-143">V takovém případě spusťte tuto rutinu s parametrem oboru nastavena na výkon.</span><span class="sxs-lookup"><span data-stu-id="3c859-143">In this case, run this cmdlet with scope parameter set to performance.</span></span> <span data-ttu-id="3c859-144">Analýza výstupu.</span><span class="sxs-lookup"><span data-stu-id="3c859-144">Analyze the output.</span></span> <span data-ttu-id="3c859-145">Získat cloudu latenci pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="3c859-145">You get the cloud read-write latencies.</span></span> <span data-ttu-id="3c859-146">Používat oznámených latencí jako maximální dosažitelný cíl, zohlednit v některé zatížení pro vnitřní zpracování dat a potom nasaďte úlohy v systému.</span><span class="sxs-lookup"><span data-stu-id="3c859-146">Use the reported latencies as maximum achievable target, factor in some overhead for the internal data processing, and then deploy the workloads on the system.</span></span> <span data-ttu-id="3c859-147">Další informace, přejděte na [použít testovací sítě k řešení potíží s výkonem zařízení](#network-test).</span><span class="sxs-lookup"><span data-stu-id="3c859-147">For more information, go to [Use the network test to troubleshoot device performance](#network-test).</span></span>


## <a name="diagnostics-test-and-sample-outputs"></a><span data-ttu-id="3c859-148">Výstupy testů a ukázkové diagnostiky</span><span class="sxs-lookup"><span data-stu-id="3c859-148">Diagnostics test and sample outputs</span></span>

### <a name="hardware-test"></a><span data-ttu-id="3c859-149">Test na hardwaru</span><span class="sxs-lookup"><span data-stu-id="3c859-149">Hardware test</span></span>

<span data-ttu-id="3c859-150">Tento test Určuje stav hardwarové součásti, Seznam USM firmware a firmware disku spuštěny v systému.</span><span class="sxs-lookup"><span data-stu-id="3c859-150">This test determines the status of the hardware components, the USM firmware, and the disk firmware running on your system.</span></span>

* <span data-ttu-id="3c859-151">Hardwarové součásti hlášené jsou tyto součásti, které se nezdařily testu nebo nejsou k dispozici v systému.</span><span class="sxs-lookup"><span data-stu-id="3c859-151">The hardware components reported are those components that failed the test or are not present in the system.</span></span>
* <span data-ttu-id="3c859-152">Seznam USM verzí firmwaru firmwaru a disku jsou hlášené pro řadič 0, 1 řadiče a sdílené součásti v systému.</span><span class="sxs-lookup"><span data-stu-id="3c859-152">The USM firmware and disk firmware versions are reported for the Controller 0, Controller 1, and shared components in your system.</span></span> <span data-ttu-id="3c859-153">Úplný seznam hardwarové součásti přejděte na:</span><span class="sxs-lookup"><span data-stu-id="3c859-153">For a complete list of hardware components, go to:</span></span>

    * [<span data-ttu-id="3c859-154">Součásti v primární skříň</span><span class="sxs-lookup"><span data-stu-id="3c859-154">Components in primary enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [<span data-ttu-id="3c859-155">Součásti v EBOD skříň</span><span class="sxs-lookup"><span data-stu-id="3c859-155">Components in EBOD enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> <span data-ttu-id="3c859-156">Pokud test na hardwaru ohlásí selhání součásti, [protokolu v žádosti o služby se Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="3c859-156">If the hardware test reports failed components, [log in a service request with Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a><span data-ttu-id="3c859-157">Ukázkový výstup spustí na zařízení s 8100 test na hardwaru</span><span class="sxs-lookup"><span data-stu-id="3c859-157">Sample output of hardware test run on an 8100 device</span></span>

<span data-ttu-id="3c859-158">Tady je ukázkový výstup ze zařízení StorSimple 8100.</span><span class="sxs-lookup"><span data-stu-id="3c859-158">Here is a sample output from a StorSimple 8100 device.</span></span> <span data-ttu-id="3c859-159">Skříň EBOD ve model zařízení 8100, není nainstalována.</span><span class="sxs-lookup"><span data-stu-id="3c859-159">In the 8100 model device, the EBOD enclosure is not present.</span></span> <span data-ttu-id="3c859-160">Proto nejsou hlášené komponenty EBOD řadiče.</span><span class="sxs-lookup"><span data-stu-id="3c859-160">Hence, the EBOD controller components are not reported.</span></span>

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

### <a name="system-test"></a><span data-ttu-id="3c859-161">Testovací systém</span><span class="sxs-lookup"><span data-stu-id="3c859-161">System test</span></span>

<span data-ttu-id="3c859-162">Tento test hlásí informace o systému, dostupné aktualizace, informace o clusteru a informace služby pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-162">This test reports the system information, the updates available, the cluster information, and the service information for your device.</span></span>

* <span data-ttu-id="3c859-163">Informace o systému zahrnuje modelu, sériové číslo zařízení, časové pásmo, stav řadiče a verze softwaru podrobné spuštěných v systému.</span><span class="sxs-lookup"><span data-stu-id="3c859-163">The system information includes the model, device serial number, time zone, controller status, and the detailed software version running on the system.</span></span> <span data-ttu-id="3c859-164">Chcete-li pochopit různé parametry systému hlášené jako výstup, přejděte na [interpretace informace o systému](#appendix-interpreting-system-information).</span><span class="sxs-lookup"><span data-stu-id="3c859-164">To understand the various system parameters reported as the output, go to [Interpreting system information](#appendix-interpreting-system-information).</span></span>

* <span data-ttu-id="3c859-165">Dostupnost aktualizace oznámí, zda režimů regular a údržby nejsou k dispozici a jejich názvy přidruženého balíčku.</span><span class="sxs-lookup"><span data-stu-id="3c859-165">The update availability reports whether the regular and maintenance modes are available and their associated package names.</span></span> <span data-ttu-id="3c859-166">Pokud `RegularUpdates` a `MaintenanceModeUpdates` jsou `false`, to znamená, že aktualizace nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3c859-166">If `RegularUpdates` and `MaintenanceModeUpdates` are `false`, this indicates that the updates are not available.</span></span> <span data-ttu-id="3c859-167">Zařízení je aktuální.</span><span class="sxs-lookup"><span data-stu-id="3c859-167">Your device is up-to-date.</span></span>
* <span data-ttu-id="3c859-168">Informace o clusteru obsahuje informace o různých logických součástí všech skupin HCS clusteru a jejich příslušné stavy.</span><span class="sxs-lookup"><span data-stu-id="3c859-168">The cluster information contains the information on various logical components of all the HCS cluster groups and their respective statuses.</span></span> <span data-ttu-id="3c859-169">Pokud uvidíte skupinu offline clusteru v této části sestavy, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="3c859-169">If you see an offline cluster group in this section of the report, [contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="3c859-170">Informace o službě obsahuje názvy a stavy všech HCS a položek konfigurace služby spuštěné na zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-170">The service information includes the names and statuses of all the HCS and CiS services running on your device.</span></span> <span data-ttu-id="3c859-171">Tyto informace jsou užitečné pro systém Microsoft Support při řešení potíží problém zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-171">This information is helpful for the Microsoft Support in troubleshooting the device issue.</span></span>

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a><span data-ttu-id="3c859-172">Ukázkový výstup testu systému spustí na zařízení s 8100</span><span class="sxs-lookup"><span data-stu-id="3c859-172">Sample output of system test run on an 8100 device</span></span>

<span data-ttu-id="3c859-173">Tady je ukázkový výstup testu systému spustí na zařízení s 8100.</span><span class="sxs-lookup"><span data-stu-id="3c859-173">Here is a sample output of the system test run on an 8100 device.</span></span>

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

### <a name="network-test"></a><span data-ttu-id="3c859-174">Test sítě</span><span class="sxs-lookup"><span data-stu-id="3c859-174">Network test</span></span>

<span data-ttu-id="3c859-175">Tento test ověřuje stav síťových rozhraní, porty, DNS a NTP připojení k serveru, SSL certifikátů, přihlašovacích údajů účtu úložiště, připojení k serverům aktualizace, a připojení k proxy serveru webu na zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-175">This test validates the status of the network interfaces, ports, DNS and NTP server connectivity, SSL certificate, storage account credentials, connectivity to the Update servers, and web proxy connectivity on your StorSimple device.</span></span>

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a><span data-ttu-id="3c859-176">Ukázkový výstup sítě testů, pokud je povoleno pouze DATA0</span><span class="sxs-lookup"><span data-stu-id="3c859-176">Sample output of network test when only DATA0 is enabled</span></span>

<span data-ttu-id="3c859-177">Tady je ukázkový výstup zařízení 8100.</span><span class="sxs-lookup"><span data-stu-id="3c859-177">Here is a sample output of the 8100 device.</span></span> <span data-ttu-id="3c859-178">Zobrazí se ve výstupu který:</span><span class="sxs-lookup"><span data-stu-id="3c859-178">You can see in the output that:</span></span>
* <span data-ttu-id="3c859-179">Pouze DATA 0 a síťová rozhraní DATA 1 jsou povolené a nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="3c859-179">Only DATA 0 and DATA 1 network interfaces are enabled and configured.</span></span>
* <span data-ttu-id="3c859-180">DATA 2 až 5 nejsou povolené v portálu.</span><span class="sxs-lookup"><span data-stu-id="3c859-180">DATA 2 - 5 are not enabled in the portal.</span></span>
* <span data-ttu-id="3c859-181">Konfigurace serveru DNS je platný, a zařízení se můžete připojit pomocí serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="3c859-181">The DNS server configuration is valid and the device can connect via the DNS server.</span></span>
* <span data-ttu-id="3c859-182">Připojení k serveru NTP je také v pořádku.</span><span class="sxs-lookup"><span data-stu-id="3c859-182">The NTP server connectivity is also fine.</span></span>
* <span data-ttu-id="3c859-183">Jsou otevřené porty 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="3c859-183">Ports 80 and 443 are open.</span></span> <span data-ttu-id="3c859-184">Port 9354 však je blokován.</span><span class="sxs-lookup"><span data-stu-id="3c859-184">However, port 9354 is blocked.</span></span> <span data-ttu-id="3c859-185">Na základě [požadavky na systém sítě](storsimple-system-requirements.md), je třeba otevřít tento port pro komunikaci se service bus.</span><span class="sxs-lookup"><span data-stu-id="3c859-185">Based on the [system network requirements](storsimple-system-requirements.md), you need to open this port for the service bus communication.</span></span>
* <span data-ttu-id="3c859-186">Certifikáty SSL je platný.</span><span class="sxs-lookup"><span data-stu-id="3c859-186">The SSL certification is valid.</span></span>
* <span data-ttu-id="3c859-187">Zařízení se může připojit k účtu úložiště: _myss8000storageacct_.</span><span class="sxs-lookup"><span data-stu-id="3c859-187">The device can connect to the storage account: _myss8000storageacct_.</span></span>
* <span data-ttu-id="3c859-188">Připojení k serverům aktualizace je platný.</span><span class="sxs-lookup"><span data-stu-id="3c859-188">The connectivity to Update servers is valid.</span></span>
* <span data-ttu-id="3c859-189">Webový proxy server není nakonfigurován na tomto zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-189">The web proxy is not configured on this device.</span></span>

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a><span data-ttu-id="3c859-190">Ukázkový výstup testu sítě, pokud jsou povolené DATA0 a DATA1</span><span class="sxs-lookup"><span data-stu-id="3c859-190">Sample output of network test when DATA0 and DATA1 are enabled</span></span>

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

### <a name="performance-test"></a><span data-ttu-id="3c859-191">Test výkonnosti</span><span class="sxs-lookup"><span data-stu-id="3c859-191">Performance test</span></span>

<span data-ttu-id="3c859-192">Tento test sestavy výkon cloudu prostřednictvím latenci pro čtení a zápis cloudu pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-192">This test reports the cloud performance via the cloud read-write latencies for your device.</span></span> <span data-ttu-id="3c859-193">Tento nástroj slouží k stanovení základní úrovně výkonu cloudu, můžete dosáhnout s StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-193">This tool can be used to establish a baseline of the cloud performance that you can achieve with StorSimple.</span></span> <span data-ttu-id="3c859-194">Nástroj hlásí maximální výkon (nejlepší scénář pro latenci pro čtení a zápis), které můžete získat pro připojení.</span><span class="sxs-lookup"><span data-stu-id="3c859-194">The tool reports the maximum performance (best case scenario for read-write latencies) that you can get for your connection.</span></span>

<span data-ttu-id="3c859-195">Jak nástroj sestavy maximální možná výkonu, můžeme použít oznámených latencí pro čtení a zápis jako cíle při nasazování úlohy.</span><span class="sxs-lookup"><span data-stu-id="3c859-195">As the tool reports the maximum achievable performance, we can use the reported read-write latencies as targets when deploying the workloads.</span></span>

<span data-ttu-id="3c859-196">Test simuluje velikosti objektu blob přidružené typy jiný svazek v zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-196">The test simulates the blob sizes associated with the different volume types on the device.</span></span> <span data-ttu-id="3c859-197">Víceúrovňová Regular a zálohování místně vázaných svazků velikost objektu blob 64 KB.</span><span class="sxs-lookup"><span data-stu-id="3c859-197">Regular tiered and backups of locally pinned volumes use a 64 KB blob size.</span></span> <span data-ttu-id="3c859-198">Vrstvené svazky s možností archivu zaškrtnuta možnost použít velikost dat blob 512 KB.</span><span class="sxs-lookup"><span data-stu-id="3c859-198">Tiered volumes with archive option checked use 512 KB blob data size.</span></span> <span data-ttu-id="3c859-199">Pokud vaše zařízení má vrstvené a místně vázaných svazků konfigurace, pouze test odpovídající do objektu blob 64 KB, velikost dat je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="3c859-199">If your device has tiered and locally pinned volumes configured, only the test corresponding to 64 KB blob data size is run.</span></span>

<span data-ttu-id="3c859-200">Chcete-li tento nástroj používat, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3c859-200">To use this tool, perform the following steps:</span></span>

1.  <span data-ttu-id="3c859-201">Nejprve vytvořte směs vrstvené svazky a vrstvené svazky s archivovaný možnost zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="3c859-201">First, create a mix of tiered volumes and tiered volumes with archived option checked.</span></span> <span data-ttu-id="3c859-202">Tato akce zajistíte, že nástroj spustí testy pro 64 KB a 512 KB velikosti objektu blob.</span><span class="sxs-lookup"><span data-stu-id="3c859-202">This action ensures that the tool runs the tests for both 64 KB and 512 KB blob sizes.</span></span>

2. <span data-ttu-id="3c859-203">Spusťte rutinu po po vytvoření a konfiguraci svazky.</span><span class="sxs-lookup"><span data-stu-id="3c859-203">Run the cmdlet after you have created and configured the volumes.</span></span> <span data-ttu-id="3c859-204">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="3c859-204">Type:</span></span>

    `Invoke-HcsDiagnostics -Scope Performance`

3. <span data-ttu-id="3c859-205">Poznamenejte si latenci pro čtení a zápis hlášené nástroj.</span><span class="sxs-lookup"><span data-stu-id="3c859-205">Make a note of the read-write latencies reported by the tool.</span></span> <span data-ttu-id="3c859-206">Tento test může trvat několik minut, který bude spuštěn před ohlásí výsledky.</span><span class="sxs-lookup"><span data-stu-id="3c859-206">This test can take several minutes to run before it reports the results.</span></span>

4. <span data-ttu-id="3c859-207">Latence připojení jsou pod očekávaný rozsah, pak latence hlášené nástroj lze nastavit jako maximální dosažitelný cíl při nasazování úlohy.</span><span class="sxs-lookup"><span data-stu-id="3c859-207">If the connection latencies are all under the expected range, then the latencies reported by the tool can be used as maximum achievable target when deploying the workloads.</span></span> <span data-ttu-id="3c859-208">Dvoufaktorového v některé zatížení pro vnitřní zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="3c859-208">Factor in some overhead for internal data processing.</span></span>

    <span data-ttu-id="3c859-209">Pokud jsou vysoké latenci pro čtení a zápis hlášené nástroj diagnostiky:</span><span class="sxs-lookup"><span data-stu-id="3c859-209">If the read-write latencies reported by the diagnostics tool are high:</span></span>

    1. <span data-ttu-id="3c859-210">Konfigurace pro služby blob Storage Analytics a analyzovat výstup pochopit latence pro účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3c859-210">Configure Storage Analytics for blob services and analyze the output to understand the latencies for the Azure storage account.</span></span> <span data-ttu-id="3c859-211">Podrobné pokyny najdete v tématu [povolení a konfigurace úložiště analýzy](../storage/common/storage-enable-and-view-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="3c859-211">For detailed instructions, go to [enable and configure Storage Analytics](../storage/common/storage-enable-and-view-metrics.md).</span></span> <span data-ttu-id="3c859-212">Pokud tyto latence jsou také vysokou a porovnatelný z hlediska na čísla, který jste dostali od nástroj diagnostiky StorSimple, budete muset přihlásit žádost o služby s Azure storage.</span><span class="sxs-lookup"><span data-stu-id="3c859-212">If those latencies are also high and comparable to the numbers you received from the StorSimple Diagnostics tool, then you need to log a service request with Azure storage.</span></span>

    2. <span data-ttu-id="3c859-213">Pokud jsou nízkou latenci účet úložiště, obraťte se na správce sítě prozkoumat problémy s latencí ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="3c859-213">If the storage account latencies are low, contact your network administrator to investigate any latency issues in your network.</span></span>

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a><span data-ttu-id="3c859-214">Ukázkový výstup testu výkonnosti spustí na zařízení s 8100</span><span class="sxs-lookup"><span data-stu-id="3c859-214">Sample output of performance test run on an 8100 device</span></span>

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

## <a name="appendix-interpreting-system-information"></a><span data-ttu-id="3c859-215">Dodatek: interpretace informace o systému</span><span class="sxs-lookup"><span data-stu-id="3c859-215">Appendix: interpreting system information</span></span>

<span data-ttu-id="3c859-216">Zde je popisující, co různé parametry prostředí Windows PowerShell v systémové informace o mapování pro tabulku.</span><span class="sxs-lookup"><span data-stu-id="3c859-216">Here is a table describing what the various Windows PowerShell parameters in the system information map to.</span></span> 

| <span data-ttu-id="3c859-217">Parametr prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c859-217">PowerShell Parameter</span></span>    | <span data-ttu-id="3c859-218">Popis</span><span class="sxs-lookup"><span data-stu-id="3c859-218">Description</span></span>  |
|-------------------------|------------------|
| <span data-ttu-id="3c859-219">ID instance</span><span class="sxs-lookup"><span data-stu-id="3c859-219">Instance ID</span></span>             | <span data-ttu-id="3c859-220">Každý řadič má jedinečný identifikátor nebo identifikátor GUID s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="3c859-220">Every controller has a unique identifier or a GUID associated with it.</span></span>|
| <span data-ttu-id="3c859-221">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="3c859-221">Name</span></span>                    | <span data-ttu-id="3c859-222">Popisný název zařízení, jak nakonfigurovat prostřednictvím portálu Azure při nasazení zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-222">The friendly name of the device as configured through the Azure portal during device deployment.</span></span> <span data-ttu-id="3c859-223">Popisný název výchozí je sériové číslo zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-223">The default friendly name is the device serial number.</span></span> |
| <span data-ttu-id="3c859-224">Model</span><span class="sxs-lookup"><span data-stu-id="3c859-224">Model</span></span>                   | <span data-ttu-id="3c859-225">Model zařízení řady StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="3c859-225">The model of your StorSimple 8000 series device.</span></span> <span data-ttu-id="3c859-226">Model může být 8100 nebo 8600.</span><span class="sxs-lookup"><span data-stu-id="3c859-226">The model can be 8100 or 8600.</span></span>|
| <span data-ttu-id="3c859-227">sériové číslo</span><span class="sxs-lookup"><span data-stu-id="3c859-227">SerialNumber</span></span>            | <span data-ttu-id="3c859-228">Sériové číslo zařízení se přiřadí v objektu pro vytváření a 15 znaků.</span><span class="sxs-lookup"><span data-stu-id="3c859-228">The device serial number is assigned at the factory and is 15 characters long.</span></span> <span data-ttu-id="3c859-229">Například 8600 SHX0991003G44HT určuje:</span><span class="sxs-lookup"><span data-stu-id="3c859-229">For instance, 8600-SHX0991003G44HT indicates:</span></span><br> <span data-ttu-id="3c859-230">8600 – je model zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-230">8600 – Is the device model.</span></span><br><span data-ttu-id="3c859-231">TVX – je místo výroby.</span><span class="sxs-lookup"><span data-stu-id="3c859-231">SHX – Is the manufacturing site.</span></span><br> <span data-ttu-id="3c859-232">0991003 - je konkrétní produkt.</span><span class="sxs-lookup"><span data-stu-id="3c859-232">0991003 - Is a specific product.</span></span> <br> <span data-ttu-id="3c859-233">G44HT-, které jsou k vytvoření jedinečné sériová čísla zvýší posledních 5 číslic.</span><span class="sxs-lookup"><span data-stu-id="3c859-233">G44HT- the last 5 digits are incremented to create unique serial numbers.</span></span> <span data-ttu-id="3c859-234">Toto nemusí být sekvenční sada.</span><span class="sxs-lookup"><span data-stu-id="3c859-234">This may not be a sequential set.</span></span>|
| <span data-ttu-id="3c859-235">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="3c859-235">TimeZone</span></span>                | <span data-ttu-id="3c859-236">Časové pásmo zařízení podle konfigurace v portálu Azure během nasazování zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-236">The device time zone as configured in the Azure portal during device deployment.</span></span>|
| <span data-ttu-id="3c859-237">CurrentController</span><span class="sxs-lookup"><span data-stu-id="3c859-237">CurrentController</span></span>       | <span data-ttu-id="3c859-238">Řadiče, které jste připojeni přes rozhraní prostředí Windows PowerShell zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-238">The controller that you are connected to through the Windows PowerShell interface of your StorSimple device.</span></span>|
| <span data-ttu-id="3c859-239">ActiveController</span><span class="sxs-lookup"><span data-stu-id="3c859-239">ActiveController</span></span>        | <span data-ttu-id="3c859-240">Na řadič, který je v zařízení aktivní a je řízení všech síťových a diskových operací.</span><span class="sxs-lookup"><span data-stu-id="3c859-240">The controller that is active on your device and is controlling all the network and disk operations.</span></span> <span data-ttu-id="3c859-241">To může být řadič 0 nebo 1 řadiče.</span><span class="sxs-lookup"><span data-stu-id="3c859-241">This can be Controller 0 or Controller 1.</span></span>  |
| <span data-ttu-id="3c859-242">Controller0Status</span><span class="sxs-lookup"><span data-stu-id="3c859-242">Controller0Status</span></span>       | <span data-ttu-id="3c859-243">Stav řadič 0 na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-243">The status of Controller 0 on your device.</span></span> <span data-ttu-id="3c859-244">Stav řadič může být normálního v režimu obnovení, nebo je nedostupný.</span><span class="sxs-lookup"><span data-stu-id="3c859-244">The controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="3c859-245">Controller1Status</span><span class="sxs-lookup"><span data-stu-id="3c859-245">Controller1Status</span></span>       | <span data-ttu-id="3c859-246">Stav řadič 1 na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-246">The status of Controller 1 on your device.</span></span>  <span data-ttu-id="3c859-247">Stav řadič může být normálního v režimu obnovení, nebo je nedostupný.</span><span class="sxs-lookup"><span data-stu-id="3c859-247">The controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="3c859-248">SystemMode</span><span class="sxs-lookup"><span data-stu-id="3c859-248">SystemMode</span></span>              | <span data-ttu-id="3c859-249">Celkový stav zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-249">The overall status of your StorSimple device.</span></span> <span data-ttu-id="3c859-250">Stav zařízení může být normální údržby, nebo vyřazení (odpovídá deaktivováno na portálu Azure).</span><span class="sxs-lookup"><span data-stu-id="3c859-250">The device status can be normal, maintenance, or decommissioned (corresponds to deactivated in the Azure portal).</span></span>|
| <span data-ttu-id="3c859-251">FriendlySoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="3c859-251">FriendlySoftwareVersion</span></span> | <span data-ttu-id="3c859-252">Popisný řetězec, který odpovídá verzi softwaru zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-252">The friendly string that corresponds to the device software version.</span></span> <span data-ttu-id="3c859-253">U systému, který používá aktualizace 4 verze popisný softwaru bude StorSimple 8000 řady aktualizace 4.0.</span><span class="sxs-lookup"><span data-stu-id="3c859-253">For a system running Update 4, the friendly software version would be StorSimple 8000 Series Update 4.0.</span></span>|
| <span data-ttu-id="3c859-254">HcsSoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="3c859-254">HcsSoftwareVersion</span></span>      | <span data-ttu-id="3c859-255">Verze softwaru HCS spuštěné na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-255">The HCS software version running on your device.</span></span> <span data-ttu-id="3c859-256">Například verze softwaru HCS odpovídající StorSimple 8000 řady aktualizace 4.0 je 6.3.9600.17820.</span><span class="sxs-lookup"><span data-stu-id="3c859-256">For instance, the HCS software version corresponding to StorSimple 8000 Series Update 4.0 is 6.3.9600.17820.</span></span> |
| <span data-ttu-id="3c859-257">ApiVersion</span><span class="sxs-lookup"><span data-stu-id="3c859-257">ApiVersion</span></span>              | <span data-ttu-id="3c859-258">Verze softwaru rozhraní API prostředí PowerShell systému Windows HCS zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-258">The software version of the Windows PowerShell API of the HCS device.</span></span>|
| <span data-ttu-id="3c859-259">VhdVersion</span><span class="sxs-lookup"><span data-stu-id="3c859-259">VhdVersion</span></span>              | <span data-ttu-id="3c859-260">Verze softwaru vytváření bitové kopie, který byl součástí zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-260">The software version of the factory image that the device was shipped with.</span></span> <span data-ttu-id="3c859-261">Pokud zařízení resetujete do výchozího, pak spustí tato verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="3c859-261">If you reset your device to factory defaults, then it runs this software version.</span></span>|
| <span data-ttu-id="3c859-262">OSVersion</span><span class="sxs-lookup"><span data-stu-id="3c859-262">OSVersion</span></span>               | <span data-ttu-id="3c859-263">Verze softwaru operačního systému Windows Server spuštěné na zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-263">The software version of the Windows Server operating system running on the device.</span></span> <span data-ttu-id="3c859-264">Zařízení StorSimple je založena na Windows Server 2012 R2, která odpovídá 6.3.9600.</span><span class="sxs-lookup"><span data-stu-id="3c859-264">The StorSimple device is based off the Windows Server 2012 R2 that corresponds to 6.3.9600.</span></span>|
| <span data-ttu-id="3c859-265">CisAgentVersion</span><span class="sxs-lookup"><span data-stu-id="3c859-265">CisAgentVersion</span></span>         | <span data-ttu-id="3c859-266">Verze pro agenta položek konfigurace pro spuštění v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-266">The version for your Cis agent running on your StorSimple device.</span></span> <span data-ttu-id="3c859-267">Tento agent pomáhá komunikovat se službou StorSimple Manager běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="3c859-267">This agent helps communicate with the StorSimple Manager service running in Azure.</span></span>|
| <span data-ttu-id="3c859-268">MdsAgentVersion</span><span class="sxs-lookup"><span data-stu-id="3c859-268">MdsAgentVersion</span></span>         | <span data-ttu-id="3c859-269">Verze odpovídající Mds agenta spuštěného v zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-269">The version corresponding to the Mds agent running on your StorSimple device.</span></span> <span data-ttu-id="3c859-270">Tento agent přesune data monitorování a Diagnostika služby MDS ().</span><span class="sxs-lookup"><span data-stu-id="3c859-270">This agent moves data to the Monitoring and Diagnostics Service (MDS).</span></span>|
| <span data-ttu-id="3c859-271">Lsisas2Version</span><span class="sxs-lookup"><span data-stu-id="3c859-271">Lsisas2Version</span></span>          | <span data-ttu-id="3c859-272">Verze odpovídající LSI ovladače zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-272">The version corresponding to the LSI drivers on your StorSimple device.</span></span>|
| <span data-ttu-id="3c859-273">Kapacita</span><span class="sxs-lookup"><span data-stu-id="3c859-273">Capacity</span></span>                | <span data-ttu-id="3c859-274">Celkový počet zařízení v bajtech.</span><span class="sxs-lookup"><span data-stu-id="3c859-274">The total capacity of the device in bytes.</span></span>|
| <span data-ttu-id="3c859-275">RemoteManagementMode</span><span class="sxs-lookup"><span data-stu-id="3c859-275">RemoteManagementMode</span></span>    | <span data-ttu-id="3c859-276">Určuje, zda zařízení můžete vzdáleně spravovat přes jeho rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c859-276">Indicates whether the device can be remotely managed via its Windows PowerShell interface.</span></span> |
| <span data-ttu-id="3c859-277">FipsMode</span><span class="sxs-lookup"><span data-stu-id="3c859-277">FipsMode</span></span>                | <span data-ttu-id="3c859-278">Určuje, zda režim Spojených států informace o zpracování Standard FIPS (Federal) povolena na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="3c859-278">Indicates whether the United States Federal Information Processing Standard (FIPS) mode is enabled on your device.</span></span> <span data-ttu-id="3c859-279">Standard FIPS 140 definuje kryptografické algoritmy pro použití schváleno nám Federal government počítačových systémů pro ochranu citlivá data.</span><span class="sxs-lookup"><span data-stu-id="3c859-279">The FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for the protection of sensitive data.</span></span> <span data-ttu-id="3c859-280">Pro zařízení se systémem Update 4 nebo novější je ve výchozím nastavení povolen režim FIPS.</span><span class="sxs-lookup"><span data-stu-id="3c859-280">For devices running Update 4 or later, FIPS mode is enabled by default.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3c859-281">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c859-281">Next steps</span></span>

* <span data-ttu-id="3c859-282">Další informace [syntaxe rutiny Invoke-HcsDiagnostics](https://technet.microsoft.com/library/mt795371.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c859-282">Learn the [syntax of the Invoke-HcsDiagnostics cmdlet](https://technet.microsoft.com/library/mt795371.aspx).</span></span>

* <span data-ttu-id="3c859-283">Další informace o tom, jak [potíží s nasazením](storsimple-troubleshoot-deployment.md) zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3c859-283">Learn more about how to [troubleshoot deployment issues](storsimple-troubleshoot-deployment.md) on your StorSimple device.</span></span>
