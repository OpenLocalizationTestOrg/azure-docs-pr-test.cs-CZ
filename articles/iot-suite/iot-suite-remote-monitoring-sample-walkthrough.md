---
title: "aaaRemote monitorování předkonfigurované řešení návod | Microsoft Docs"
description: "Popis hello Azure IoT předkonfigurovaného řešení vzdáleného monitorování a jeho architektura."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 57a336bd94938c2b9ee5d3456ea8e45446cf3d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a><span data-ttu-id="2edb7-103">Návod pro předkonfigurované řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="2edb7-103">Remote monitoring preconfigured solution walkthrough</span></span>

<span data-ttu-id="2edb7-104">Hello vzdáleného sledování IoT Suite [předkonfigurované řešení] [ lnk-preconfigured-solutions] je implementace pro kompletní monitorování řešení pro více počítačů spuštěných ve vzdálených umístěních.</span><span class="sxs-lookup"><span data-stu-id="2edb7-104">hello IoT Suite remote monitoring [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end monitoring solution for multiple machines running in remote locations.</span></span> <span data-ttu-id="2edb7-105">Hello řešení kombinuje klíčové služby Azure tooprovide obecnou implementaci hello podnikový scénář.</span><span class="sxs-lookup"><span data-stu-id="2edb7-105">hello solution combines key Azure services tooprovide a generic implementation of hello business scenario.</span></span> <span data-ttu-id="2edb7-106">Hello řešení můžete použít jako výchozí bod pro vlastní implementaci a [přizpůsobit] [ lnk-customize] ho toomeet specifických podnikových požadavků.</span><span class="sxs-lookup"><span data-stu-id="2edb7-106">You can use hello solution as a starting point for your own implementation and [customize][lnk-customize] it toomeet your own specific business requirements.</span></span>

<span data-ttu-id="2edb7-107">Tento článek vás provede procesem některé klíčové prvky hello tooenable řešení vzdáleného monitorování hello toounderstand jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="2edb7-107">This article walks you through some of hello key elements of hello remote monitoring solution tooenable you toounderstand how it works.</span></span> <span data-ttu-id="2edb7-108">Díky tomu budete moct:</span><span class="sxs-lookup"><span data-stu-id="2edb7-108">This knowledge helps you to:</span></span>

* <span data-ttu-id="2edb7-109">Řešení problémů v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-109">Troubleshoot issues in hello solution.</span></span>
* <span data-ttu-id="2edb7-110">Plánování jak toocustomize toohello řešení toomeet vaše konkrétní požadavky.</span><span class="sxs-lookup"><span data-stu-id="2edb7-110">Plan how toocustomize toohello solution toomeet your own specific requirements.</span></span> 
* <span data-ttu-id="2edb7-111">Navrhněte vlastní řešení IoT, které používá služby Azure.</span><span class="sxs-lookup"><span data-stu-id="2edb7-111">Design your own IoT solution that uses Azure services.</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="2edb7-112">Logická architektura</span><span class="sxs-lookup"><span data-stu-id="2edb7-112">Logical architecture</span></span>

<span data-ttu-id="2edb7-113">Hello následující diagram popisuje logické součásti hello hello předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="2edb7-113">hello following diagram outlines hello logical components of hello preconfigured solution:</span></span>

![Logická architektura](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a><span data-ttu-id="2edb7-115">Simulovaná zařízení</span><span class="sxs-lookup"><span data-stu-id="2edb7-115">Simulated devices</span></span>

<span data-ttu-id="2edb7-116">V hello předkonfigurované řešení hello simulované zařízení představuje chladicí zařízení (například budově nebo budovy klimatizační jednotku).</span><span class="sxs-lookup"><span data-stu-id="2edb7-116">In hello preconfigured solution, hello simulated device represents a cooling device (such as a building air conditioner or facility air handling unit).</span></span> <span data-ttu-id="2edb7-117">Když nasadíte hello předkonfigurované řešení, také automaticky zřizovat čtyři Simulovaná zařízení, které běží v [webové úlohy Azure][lnk-webjobs].</span><span class="sxs-lookup"><span data-stu-id="2edb7-117">When you deploy hello preconfigured solution, you also automatically provision four simulated devices that run in an [Azure WebJob][lnk-webjobs].</span></span> <span data-ttu-id="2edb7-118">Hello simulované zařízení usnadní vám tooexplore hello chování hello řešení bez nutnosti toodeploy hello žádné fyzické zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-118">hello simulated devices make it easy for you tooexplore hello behavior of hello solution without hello need toodeploy any physical devices.</span></span> <span data-ttu-id="2edb7-119">toodeploy skutečných fyzické zařízení, najdete v části hello [připojit vaše zařízení toohello předkonfigurovanému řešení vzdáleného monitorování] [ lnk-connect-rm] kurzu.</span><span class="sxs-lookup"><span data-stu-id="2edb7-119">toodeploy a real physical device, see hello [Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

### <a name="device-to-cloud-messages"></a><span data-ttu-id="2edb7-120">Zprávy typu zařízení-cloud</span><span class="sxs-lookup"><span data-stu-id="2edb7-120">Device-to-cloud messages</span></span>

<span data-ttu-id="2edb7-121">Každé simulované zařízení můžete odeslat následující typy tooIoT zprávu centra hello:</span><span class="sxs-lookup"><span data-stu-id="2edb7-121">Each simulated device can send hello following message types tooIoT Hub:</span></span>

| <span data-ttu-id="2edb7-122">Zpráva</span><span class="sxs-lookup"><span data-stu-id="2edb7-122">Message</span></span> | <span data-ttu-id="2edb7-123">Popis</span><span class="sxs-lookup"><span data-stu-id="2edb7-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2edb7-124">Spuštění</span><span class="sxs-lookup"><span data-stu-id="2edb7-124">Startup</span></span> |<span data-ttu-id="2edb7-125">Když se spustí hello zařízení, odešle **informace o zařízení** zprávu obsahující informace o samotné toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="2edb7-125">When hello device starts, it sends a **device-info** message containing information about itself toohello back end.</span></span> <span data-ttu-id="2edb7-126">Tato data zahrnují id zařízení hello a seznam příkazů hello a metody hello zařízení podporuje.</span><span class="sxs-lookup"><span data-stu-id="2edb7-126">This data includes hello device id and a list of hello commands and methods hello device supports.</span></span> |
| <span data-ttu-id="2edb7-127">Přítomnost</span><span class="sxs-lookup"><span data-stu-id="2edb7-127">Presence</span></span> |<span data-ttu-id="2edb7-128">Zařízení odesílá **přítomnosti** zprávy tooreport, zda zařízení hello rozpozná hello přítomnost senzoru.</span><span class="sxs-lookup"><span data-stu-id="2edb7-128">A device periodically sends a **presence** message tooreport whether hello device can sense hello presence of a sensor.</span></span> |
| <span data-ttu-id="2edb7-129">Telemetrická data</span><span class="sxs-lookup"><span data-stu-id="2edb7-129">Telemetry</span></span> |<span data-ttu-id="2edb7-130">Zařízení odesílá **telemetrie** zprávu, která hlásí simulované hodnoty pro hello teploty a vlhkosti shromážděných z hello zařízení je simulované senzorů.</span><span class="sxs-lookup"><span data-stu-id="2edb7-130">A device periodically sends a **telemetry** message that reports simulated values for hello temperature and humidity collected from hello device's simulated sensors.</span></span> |

> [!NOTE]
> <span data-ttu-id="2edb7-131">Hello řešení ukládá hello seznam příkazů hello zařízením v databázi Cosmos DB a není v dvojče zařízení hello podporován.</span><span class="sxs-lookup"><span data-stu-id="2edb7-131">hello solution stores hello list of commands supported by hello device in a Cosmos DB database and not in hello device twin.</span></span>

### <a name="properties-and-device-twins"></a><span data-ttu-id="2edb7-132">Vlastnosti a dvojčata zařízení</span><span class="sxs-lookup"><span data-stu-id="2edb7-132">Properties and device twins</span></span>

<span data-ttu-id="2edb7-133">Hello Simulovaná zařízení odesílají hello následující zařízení vlastnosti toohello [twin] [ lnk-device-twins] hello IoT hub jako *hlášené vlastnosti*.</span><span class="sxs-lookup"><span data-stu-id="2edb7-133">hello simulated devices send hello following device properties toohello [twin][lnk-device-twins] in hello IoT hub as *reported properties*.</span></span> <span data-ttu-id="2edb7-134">Hello zasílá zařízení hlášené vlastnosti při spuštění a v odpovědi tooa **změny stavu zařízení** příkaz nebo metoda.</span><span class="sxs-lookup"><span data-stu-id="2edb7-134">hello device sends reported properties at startup and in response tooa **Change Device State** command or method.</span></span>

| <span data-ttu-id="2edb7-135">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2edb7-135">Property</span></span> | <span data-ttu-id="2edb7-136">Účel</span><span class="sxs-lookup"><span data-stu-id="2edb7-136">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="2edb7-137">Config.TelemetryInterval</span><span class="sxs-lookup"><span data-stu-id="2edb7-137">Config.TelemetryInterval</span></span> | <span data-ttu-id="2edb7-138">Frekvence (v sekundách) hello zařízení odesílá telemetrii</span><span class="sxs-lookup"><span data-stu-id="2edb7-138">Frequency (seconds) hello device sends telemetry</span></span> |
| <span data-ttu-id="2edb7-139">Config.TemperatureMeanValue</span><span class="sxs-lookup"><span data-stu-id="2edb7-139">Config.TemperatureMeanValue</span></span> | <span data-ttu-id="2edb7-140">Určuje střední hodnotu hello hello simulované teplotní telemetrie</span><span class="sxs-lookup"><span data-stu-id="2edb7-140">Specifies hello mean value for hello simulated temperature telemetry</span></span> |
| <span data-ttu-id="2edb7-141">Device.DeviceID</span><span class="sxs-lookup"><span data-stu-id="2edb7-141">Device.DeviceID</span></span> |<span data-ttu-id="2edb7-142">ID, které je zadáno nebo přiřazeno při vytvoření zařízení v řešení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-142">Id that is either provided or assigned when a device is created in hello solution</span></span> |
| <span data-ttu-id="2edb7-143">Device.DeviceState</span><span class="sxs-lookup"><span data-stu-id="2edb7-143">Device.DeviceState</span></span> | <span data-ttu-id="2edb7-144">Stav zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-144">State reported by hello device</span></span> |
| <span data-ttu-id="2edb7-145">Device.CreatedTime</span><span class="sxs-lookup"><span data-stu-id="2edb7-145">Device.CreatedTime</span></span> |<span data-ttu-id="2edb7-146">Čas hello zařízení byl vytvořen v řešení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-146">Time hello device was created in hello solution</span></span> |
| <span data-ttu-id="2edb7-147">Device.StartupTime</span><span class="sxs-lookup"><span data-stu-id="2edb7-147">Device.StartupTime</span></span> |<span data-ttu-id="2edb7-148">Čas hello zařízení byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="2edb7-148">Time hello device was started</span></span> |
| <span data-ttu-id="2edb7-149">Device.LastDesiredPropertyChange</span><span class="sxs-lookup"><span data-stu-id="2edb7-149">Device.LastDesiredPropertyChange</span></span> |<span data-ttu-id="2edb7-150">Změňte číslo verze Hello hello poslední požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2edb7-150">hello version number of hello last desired property change</span></span> |
| <span data-ttu-id="2edb7-151">Device.Location.Latitude</span><span class="sxs-lookup"><span data-stu-id="2edb7-151">Device.Location.Latitude</span></span> |<span data-ttu-id="2edb7-152">Zeměpisná šířka umístění zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-152">Latitude location of hello device</span></span> |
| <span data-ttu-id="2edb7-153">Device.Location.Longitude</span><span class="sxs-lookup"><span data-stu-id="2edb7-153">Device.Location.Longitude</span></span> |<span data-ttu-id="2edb7-154">Zeměpisná délka umístění zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-154">Longitude location of hello device</span></span> |
| <span data-ttu-id="2edb7-155">System.Manufacturer</span><span class="sxs-lookup"><span data-stu-id="2edb7-155">System.Manufacturer</span></span> |<span data-ttu-id="2edb7-156">Výrobce zařízení</span><span class="sxs-lookup"><span data-stu-id="2edb7-156">Device manufacturer</span></span> |
| <span data-ttu-id="2edb7-157">System.ModelNumber</span><span class="sxs-lookup"><span data-stu-id="2edb7-157">System.ModelNumber</span></span> |<span data-ttu-id="2edb7-158">Číslo modelu zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-158">Model number of hello device</span></span> |
| <span data-ttu-id="2edb7-159">System.SerialNumber</span><span class="sxs-lookup"><span data-stu-id="2edb7-159">System.SerialNumber</span></span> |<span data-ttu-id="2edb7-160">Sériové číslo zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-160">Serial number of hello device</span></span> |
| <span data-ttu-id="2edb7-161">System.FirmwareVersion</span><span class="sxs-lookup"><span data-stu-id="2edb7-161">System.FirmwareVersion</span></span> |<span data-ttu-id="2edb7-162">Aktuální verzi firmwaru v zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-162">Current version of firmware on hello device</span></span> |
| <span data-ttu-id="2edb7-163">System.Platform</span><span class="sxs-lookup"><span data-stu-id="2edb7-163">System.Platform</span></span> |<span data-ttu-id="2edb7-164">Architektura platformy zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-164">Platform architecture of hello device</span></span> |
| <span data-ttu-id="2edb7-165">System.Processor</span><span class="sxs-lookup"><span data-stu-id="2edb7-165">System.Processor</span></span> |<span data-ttu-id="2edb7-166">Procesor spuštěné hello zařízení</span><span class="sxs-lookup"><span data-stu-id="2edb7-166">Processor running hello device</span></span> |
| <span data-ttu-id="2edb7-167">System.InstalledRAM</span><span class="sxs-lookup"><span data-stu-id="2edb7-167">System.InstalledRAM</span></span> |<span data-ttu-id="2edb7-168">Velikost paměti RAM nainstalované v zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-168">Amount of RAM installed on hello device</span></span> |

<span data-ttu-id="2edb7-169">Hello simulátor doplňuje pro tyto vlastnosti v simulovaném zařízení vzorové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2edb7-169">hello simulator seeds these properties in simulated devices with sample values.</span></span> <span data-ttu-id="2edb7-170">Pokaždé, když hello simulátor inicializuje simulované zařízení, hello zařízení hlásí hello předem definovaná metadata tooIoT rozbočovače jako hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2edb7-170">Each time hello simulator initializes a simulated device, hello device reports hello pre-defined metadata tooIoT Hub as reported properties.</span></span> <span data-ttu-id="2edb7-171">Hlášené vlastnosti lze aktualizovat pouze pomocí hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-171">Reported properties can only be updated by hello device.</span></span> <span data-ttu-id="2edb7-172">toochange hlášené vlastnost, nastavte požadovanou vlastnost portálu řešení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-172">toochange a reported property, you set a desired property in solution portal.</span></span> <span data-ttu-id="2edb7-173">Je zodpovědností hello hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="2edb7-173">It is hello responsibility of hello device to:</span></span>

1. <span data-ttu-id="2edb7-174">Pravidelně načíst ze služby IoT hub hello požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2edb7-174">Periodically retrieve desired properties from hello IoT hub.</span></span>
2. <span data-ttu-id="2edb7-175">Hodnotou vlastnosti hello potřeby aktualizaci konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2edb7-175">Update its configuration with hello desired property value.</span></span>
3. <span data-ttu-id="2edb7-176">Odešlete hello novou hodnotu back toohello hub jako hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2edb7-176">Send hello new value back toohello hub as a reported property.</span></span>

<span data-ttu-id="2edb7-177">Z řídicího panelu řešení hello, můžete použít *potřeby vlastnosti* tooset vlastnosti na zařízení s použitím hello [dvojče zařízení][lnk-device-twins].</span><span class="sxs-lookup"><span data-stu-id="2edb7-177">From hello solution dashboard, you can use *desired properties* tooset properties on a device by using hello [device twin][lnk-device-twins].</span></span> <span data-ttu-id="2edb7-178">Obvykle zařízení čte hodnotu požadované vlastnosti z tooupdate hello rozbočovače, jeho vnitřní stav a sestavy hello změní zpět jako hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2edb7-178">Typically, a device reads a desired property value from hello hub tooupdate its internal state and report hello change back as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="2edb7-179">Hello kód simulované zařízení používá jenom hello **Desired.Config.TemperatureMeanValue** a **Desired.Config.TelemetryInterval** požadované vlastnosti tooupdate hello hlášené vlastnosti odeslána zpět tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2edb7-179">hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties sent back tooIoT Hub.</span></span> <span data-ttu-id="2edb7-180">V hello simulované zařízení jsou ignorovány všechny ostatní žádosti o změnu požadovanou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2edb7-180">All other desired property change requests are ignored in hello simulated device.</span></span>

### <a name="methods"></a><span data-ttu-id="2edb7-181">Metody</span><span class="sxs-lookup"><span data-stu-id="2edb7-181">Methods</span></span>

<span data-ttu-id="2edb7-182">Hello Simulovaná zařízení mohou zpracovávat následující metody hello ([přímé metody][lnk-direct-methods]) volat z portálu řešení hello prostřednictvím hello IoT hub:</span><span class="sxs-lookup"><span data-stu-id="2edb7-182">hello simulated devices can handle hello following methods ([direct methods][lnk-direct-methods]) invoked from hello solution portal through hello IoT hub:</span></span>

| <span data-ttu-id="2edb7-183">Metoda</span><span class="sxs-lookup"><span data-stu-id="2edb7-183">Method</span></span> | <span data-ttu-id="2edb7-184">Popis</span><span class="sxs-lookup"><span data-stu-id="2edb7-184">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2edb7-185">InitiateFirmwareUpdate</span><span class="sxs-lookup"><span data-stu-id="2edb7-185">InitiateFirmwareUpdate</span></span> |<span data-ttu-id="2edb7-186">Dá pokyn hello zařízení tooperform aktualizaci firmwaru</span><span class="sxs-lookup"><span data-stu-id="2edb7-186">Instructs hello device tooperform a firmware update</span></span> |
| <span data-ttu-id="2edb7-187">Restartování</span><span class="sxs-lookup"><span data-stu-id="2edb7-187">Reboot</span></span> |<span data-ttu-id="2edb7-188">Dá pokyn tooreboot zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-188">Instructs hello device tooreboot</span></span> |
| <span data-ttu-id="2edb7-189">FactoryReset</span><span class="sxs-lookup"><span data-stu-id="2edb7-189">FactoryReset</span></span> |<span data-ttu-id="2edb7-190">Dá pokyn tooperform hello zařízení resetovat výrobní nastavení</span><span class="sxs-lookup"><span data-stu-id="2edb7-190">Instructs hello device tooperform a factory reset</span></span> |

<span data-ttu-id="2edb7-191">Některé metody použijte hlášené vlastnosti tooreport na průběh.</span><span class="sxs-lookup"><span data-stu-id="2edb7-191">Some methods use reported properties tooreport on progress.</span></span> <span data-ttu-id="2edb7-192">Například hello **InitiateFirmwareUpdate** metoda simuluje spuštěné hello aktualizaci asynchronně na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-192">For example, hello **InitiateFirmwareUpdate** method simulates running hello update asynchronously on hello device.</span></span> <span data-ttu-id="2edb7-193">Hello metoda vrátí okamžitě na zařízení hello pomocí řídicího panelu řešení toohello zatímco hello asynchronní úkol stále toosend aktualizací stavu zpět hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2edb7-193">hello method returns immediately on hello device, while hello asynchronous task continues toosend status updates back toohello solution dashboard using reported properties.</span></span>

### <a name="commands"></a><span data-ttu-id="2edb7-194">Příkazy</span><span class="sxs-lookup"><span data-stu-id="2edb7-194">Commands</span></span>

<span data-ttu-id="2edb7-195">Hello simulované zařízení mohou zpracovávat následující příkazy (zpráv typu cloud zařízení), odeslané ze hello portál řešení prostřednictvím centra IoT hello hello:</span><span class="sxs-lookup"><span data-stu-id="2edb7-195">hello simulated devices can handle hello following commands (cloud-to-device messages) sent from hello solution portal through hello IoT hub:</span></span>

| <span data-ttu-id="2edb7-196">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2edb7-196">Command</span></span> | <span data-ttu-id="2edb7-197">Popis</span><span class="sxs-lookup"><span data-stu-id="2edb7-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2edb7-198">PingDevice</span><span class="sxs-lookup"><span data-stu-id="2edb7-198">PingDevice</span></span> |<span data-ttu-id="2edb7-199">Odešle *ping* toocheck toohello zařízení je zachování připojení</span><span class="sxs-lookup"><span data-stu-id="2edb7-199">Sends a *ping* toohello device toocheck it is alive</span></span> |
| <span data-ttu-id="2edb7-200">StartTelemetry</span><span class="sxs-lookup"><span data-stu-id="2edb7-200">StartTelemetry</span></span> |<span data-ttu-id="2edb7-201">Spustí hello zařízení odesílat telemetrii</span><span class="sxs-lookup"><span data-stu-id="2edb7-201">Starts hello device sending telemetry</span></span> |
| <span data-ttu-id="2edb7-202">StopTelemetry</span><span class="sxs-lookup"><span data-stu-id="2edb7-202">StopTelemetry</span></span> |<span data-ttu-id="2edb7-203">Zastaví odesílání telemetrických dat zařízením hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-203">Stops hello device from sending telemetry</span></span> |
| <span data-ttu-id="2edb7-204">ChangeSetPointTemp</span><span class="sxs-lookup"><span data-stu-id="2edb7-204">ChangeSetPointTemp</span></span> |<span data-ttu-id="2edb7-205">Hodnotu bodu hello změny kolem které hello se vytvářejí náhodná data</span><span class="sxs-lookup"><span data-stu-id="2edb7-205">Changes hello set point value around which hello random data is generated</span></span> |
| <span data-ttu-id="2edb7-206">DiagnosticTelemetry</span><span class="sxs-lookup"><span data-stu-id="2edb7-206">DiagnosticTelemetry</span></span> |<span data-ttu-id="2edb7-207">Aktivační události hello toosend simulátoru zařízení další telemetrické hodnoty (externalTemp)</span><span class="sxs-lookup"><span data-stu-id="2edb7-207">Triggers hello device simulator toosend an additional telemetry value (externalTemp)</span></span> |
| <span data-ttu-id="2edb7-208">ChangeDeviceState</span><span class="sxs-lookup"><span data-stu-id="2edb7-208">ChangeDeviceState</span></span> |<span data-ttu-id="2edb7-209">Změní rozšířené stavové vlastnosti zařízení hello a odešle zprávu s informacemi o hello ze zařízení hello</span><span class="sxs-lookup"><span data-stu-id="2edb7-209">Changes an extended state property for hello device and sends hello device info message from hello device</span></span> |

> [!NOTE]
> <span data-ttu-id="2edb7-210">Porovnání těchto příkazů (zpráv typu cloud-zařízení) a metod (přímých metod) najdete v [doprovodných materiálech ke komunikaci typu cloud-zařízení][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="2edb7-210">For a comparison of these commands (cloud-to-device messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>

## <a name="iot-hub"></a><span data-ttu-id="2edb7-211">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="2edb7-211">IoT Hub</span></span>

<span data-ttu-id="2edb7-212">Hello [služby IoT hub] [ lnk-iothub] ingestuje dat odesílaných ze zařízení hello do cloudu hello a udělá z něj k dispozici toohello úlohy Azure Stream Analytics (ASA).</span><span class="sxs-lookup"><span data-stu-id="2edb7-212">hello [IoT hub][lnk-iothub] ingests data sent from hello devices into hello cloud and makes it available toohello Azure Stream Analytics (ASA) jobs.</span></span> <span data-ttu-id="2edb7-213">Každá úloha datového proudu ASA používá samostatné služby IoT Hub příjemce skupiny tooread hello proud zpráv ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-213">Each stream ASA job uses a separate IoT Hub consumer group tooread hello stream of messages from your devices.</span></span>

<span data-ttu-id="2edb7-214">centra IoT řešení hello Hello také:</span><span class="sxs-lookup"><span data-stu-id="2edb7-214">hello IoT hub in hello solution also:</span></span>

- <span data-ttu-id="2edb7-215">Udržuje registru identit, která ukládá hello ID a ověřovací klíče všech hello zařízení povolené tooconnect toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="2edb7-215">Maintains an identity registry that stores hello ids and authentication keys of all hello devices permitted tooconnect toohello portal.</span></span> <span data-ttu-id="2edb7-216">Můžete povolit nebo zakázat zařízení prostřednictvím registru identit hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-216">You can enable and disable devices through hello identity registry.</span></span>
- <span data-ttu-id="2edb7-217">Odešle příkazy tooyour zařízení jménem portál řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-217">Sends commands tooyour devices on behalf of hello solution portal.</span></span>
- <span data-ttu-id="2edb7-218">Vyvolá metody v zařízeních jménem portál řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-218">Invokes methods on your devices on behalf of hello solution portal.</span></span>
- <span data-ttu-id="2edb7-219">Udržuje dvojčata zařízení pro všechna registrovaná zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-219">Maintains device twins for all registered devices.</span></span> <span data-ttu-id="2edb7-220">Dvojče zařízení uloží hodnoty vlastností hello hlášených zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-220">A device twin stores hello property values reported by a device.</span></span> <span data-ttu-id="2edb7-221">Dvojče zařízení také ukládá nastaveny hello řešení portálu, tooretrieve hello zařízení, když se připojí další požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2edb7-221">A device twin also stores desired properties, set in hello solution portal, for hello device tooretrieve when it next connects.</span></span>
- <span data-ttu-id="2edb7-222">Plány úloh tooset vlastnosti pro více zařízení nebo volat metody na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="2edb7-222">Schedules jobs tooset properties for multiple devices or invoke methods on multiple devices.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="2edb7-223">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="2edb7-223">Azure Stream Analytics</span></span>

<span data-ttu-id="2edb7-224">V řešení, vzdáleného sledování hello [Azure Stream Analytics] [ lnk-asa] (ASA) odešle zprávu zařízení zprávy přijaté službou IoT hub tooother back-end hello komponenty pro zpracování nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="2edb7-224">In hello remote monitoring solution, [Azure Stream Analytics][lnk-asa] (ASA) dispatches device messages received by hello IoT hub tooother back-end components for processing or storage.</span></span> <span data-ttu-id="2edb7-225">Různé úlohy ASA provést požadované funkce na základě obsahu hello hello zpráv.</span><span class="sxs-lookup"><span data-stu-id="2edb7-225">Different ASA jobs perform specific functions based on hello content of hello messages.</span></span>

<span data-ttu-id="2edb7-226">**Úloha 1: Informace o zařízení** filtry zprávy z hello příchozího datového proudu zprávy s informacemi o zařízení a odešle je tooan koncového bodu centra událostí.</span><span class="sxs-lookup"><span data-stu-id="2edb7-226">**Job 1: Device Info** filters device information messages from hello incoming message stream and sends them tooan Event Hub endpoint.</span></span> <span data-ttu-id="2edb7-227">Zařízení odesílá zprávy s informacemi o zařízení při spuštění a v odpovědi tooa **SendDeviceInfo** příkaz.</span><span class="sxs-lookup"><span data-stu-id="2edb7-227">A device sends device information messages at startup and in response tooa **SendDeviceInfo** command.</span></span> <span data-ttu-id="2edb7-228">Tato úloha používá následující tooidentify definice dotazu hello **informace o zařízení** zprávy:</span><span class="sxs-lookup"><span data-stu-id="2edb7-228">This job uses hello following query definition tooidentify **device-info** messages:</span></span>

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

<span data-ttu-id="2edb7-229">Tuto úlohu odešle svůj výstupní tooan centra událostí pro další zpracování.</span><span class="sxs-lookup"><span data-stu-id="2edb7-229">This job sends its output tooan Event Hub for further processing.</span></span>

<span data-ttu-id="2edb7-230">**Úloha 2: Pravidla** vyhodnotí příchozí telemetrická data o teplotě a vlhkosti v porovnání s mezními hodnotami na daném zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-230">**Job 2: Rules** evaluates incoming temperature and humidity telemetry values against per-device thresholds.</span></span> <span data-ttu-id="2edb7-231">Mezní hodnoty se nastavují v editoru pravidel hello v portálu řešení hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2edb7-231">Threshold values are set in hello rules editor available in hello solution portal.</span></span> <span data-ttu-id="2edb7-232">Každý pár zařízení/hodnota se ukládá pomocí časového razítka v objektu blob, který služba Stream Analytics načítá jako **referenční data**.</span><span class="sxs-lookup"><span data-stu-id="2edb7-232">Each device/value pair is stored by timestamp in a blob which Stream Analytics reads in as **Reference Data**.</span></span> <span data-ttu-id="2edb7-233">Hello úloha porovná všechny neprázdnou hodnotu proti hello nastavenou prahovou hodnotu pro hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-233">hello job compares any non-empty value against hello set threshold for hello device.</span></span> <span data-ttu-id="2edb7-234">Pokud překročí hello ' >' podmínky výstupy úlohy hello **alarmů** událost, která označuje, že prahová hodnota hello je překročena a poskytuje hello zařízení, hodnotu a hodnoty časového razítka.</span><span class="sxs-lookup"><span data-stu-id="2edb7-234">If it exceeds hello '>' condition, hello job outputs an **alarm** event that indicates that hello threshold is exceeded and provides hello device, value, and timestamp values.</span></span> <span data-ttu-id="2edb7-235">Tato úloha používá následující dotaz definice tooidentify telemetrické zprávy, které spustí alarm hello:</span><span class="sxs-lookup"><span data-stu-id="2edb7-235">This job uses hello following query definition tooidentify telemetry messages that should trigger an alarm:</span></span>

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

<span data-ttu-id="2edb7-236">Hello úloha odešle svůj výstupní tooan centra událostí pro další zpracování a ukládá podrobnosti o jednotlivých výstrah tooblob úložiště z kde hello portál řešení může číst informace o výstrahách hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-236">hello job sends its output tooan Event Hub for further processing and saves details of each alert tooblob storage from where hello solution portal can read hello alert information.</span></span>

<span data-ttu-id="2edb7-237">**Úloha 3: Telemetrická** funguje na hello příchozí zařízení datový proud telemetrie dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="2edb7-237">**Job 3: Telemetry** operates on hello incoming device telemetry stream in two ways.</span></span> <span data-ttu-id="2edb7-238">Hello nejprve odeslání všech telemetrických zpráv z úložiště objektů blob toopersistent hello zařízení pro dlouhodobé uložení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-238">hello first sends all telemetry messages from hello devices toopersistent blob storage for long-term storage.</span></span> <span data-ttu-id="2edb7-239">Hello druhý vypočítá vlhkosti průměrné, minimální a maximální hodnoty přes posuvné okno pět minut a odešle tato data tooblob úložiště.</span><span class="sxs-lookup"><span data-stu-id="2edb7-239">hello second computes average, minimum, and maximum humidity values over a five-minute sliding window and sends this data tooblob storage.</span></span> <span data-ttu-id="2edb7-240">portál řešení Hello čte hello telemetrická data z objektu blob úložiště toopopulate hello grafy.</span><span class="sxs-lookup"><span data-stu-id="2edb7-240">hello solution portal reads hello telemetry data from blob storage toopopulate hello charts.</span></span> <span data-ttu-id="2edb7-241">Tato úloha používá následující definici dotazu hello:</span><span class="sxs-lookup"><span data-stu-id="2edb7-241">This job uses hello following query definition:</span></span>

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a><span data-ttu-id="2edb7-242">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="2edb7-242">Event Hubs</span></span>

<span data-ttu-id="2edb7-243">Hello **informace o zařízení** a **pravidla** úlohy ASA výstup jejich data tooEvent centra tooreliably dál na toohello **procesor událostí** spuštěné v hello webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="2edb7-243">hello **device info** and **rules** ASA jobs output their data tooEvent Hubs tooreliably forward on toohello **Event Processor** running in hello WebJob.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="2edb7-244">Úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="2edb7-244">Azure storage</span></span>

<span data-ttu-id="2edb7-245">Hello řešení používá Azure blob storage toopersist všechny hello nezpracovaná a souhrnné telemetrická data ze zařízení hello v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-245">hello solution uses Azure blob storage toopersist all hello raw and summarized telemetry data from hello devices in hello solution.</span></span> <span data-ttu-id="2edb7-246">portál Hello čte hello telemetrická data z objektu blob úložiště toopopulate hello grafy.</span><span class="sxs-lookup"><span data-stu-id="2edb7-246">hello portal reads hello telemetry data from blob storage toopopulate hello charts.</span></span> <span data-ttu-id="2edb7-247">toodisplay výstrahy, portál řešení hello čte hello data z úložiště objektů blob této záznamy při telemetrie překročila hodnoty hello nakonfigurované prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2edb7-247">toodisplay alerts, hello solution portal reads hello data from blob storage that records when telemetry values exceeded hello configured threshold values.</span></span> <span data-ttu-id="2edb7-248">řešení Hello také používá objekt blob úložiště toorecord hello prahové hodnoty, které se nastavují v portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-248">hello solution also uses blob storage toorecord hello threshold values you set in hello solution portal.</span></span>

## <a name="webjobs"></a><span data-ttu-id="2edb7-249">Webové úlohy</span><span class="sxs-lookup"><span data-stu-id="2edb7-249">WebJobs</span></span>

<span data-ttu-id="2edb7-250">Kromě toho simulátorů zařízení hello toohosting, hello webové úlohy v řešení hello také hostitele hello **procesor událostí** spuštěné v Azure webová úloha, která zpracovává odezvy na příkazy.</span><span class="sxs-lookup"><span data-stu-id="2edb7-250">In addition toohosting hello device simulators, hello WebJobs in hello solution also host hello **Event Processor** running in an Azure WebJob that handles command responses.</span></span> <span data-ttu-id="2edb7-251">Používá příkaz odpovědi zprávy tooupdate hello zařízení historie příkazů (uložené v databázi Cosmos DB hello).</span><span class="sxs-lookup"><span data-stu-id="2edb7-251">It uses command response messages tooupdate hello device command history (stored in hello Cosmos DB database).</span></span>

## <a name="cosmos-db"></a><span data-ttu-id="2edb7-252">Databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="2edb7-252">Cosmos DB</span></span>

<span data-ttu-id="2edb7-253">řešení Hello používá informace o databázi Cosmos databáze toostore o hello zařízení připojených toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-253">hello solution uses a Cosmos DB database toostore information about hello devices connected toohello solution.</span></span> <span data-ttu-id="2edb7-254">Tyto informace zahrnují hello historie příkazů toodevices odeslaný portál řešení hello a metod volat z portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-254">This information includes hello history of commands sent toodevices from hello solution portal and of methods invoked from hello solution portal.</span></span>

## <a name="solution-portal"></a><span data-ttu-id="2edb7-255">Portál řešení</span><span class="sxs-lookup"><span data-stu-id="2edb7-255">Solution portal</span></span>

<span data-ttu-id="2edb7-256">portál řešení Hello je nasazen jako součást hello předkonfigurované řešení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2edb7-256">hello solution portal is a web app deployed as part of hello preconfigured solution.</span></span> <span data-ttu-id="2edb7-257">Hello klíče stránky portálu řešení hello jsou hello řídicí panel a seznam zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-257">hello key pages in hello solution portal are hello dashboard and hello device list.</span></span>

### <a name="dashboard"></a><span data-ttu-id="2edb7-258">Řídicí panel</span><span class="sxs-lookup"><span data-stu-id="2edb7-258">Dashboard</span></span>

<span data-ttu-id="2edb7-259">Tato stránka ve hello webové aplikaci používá ovládací prvky PowerBI jazyce javascript (viz [úložiště vizuálních prvků PowerBI](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetrická data ze zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="2edb7-259">This page in hello web app uses PowerBI javascript controls (See [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetry data from hello devices.</span></span> <span data-ttu-id="2edb7-260">řešení Hello používá hello ASA telemetrie úlohy toowrite hello telemetrická data tooblob úložiště.</span><span class="sxs-lookup"><span data-stu-id="2edb7-260">hello solution uses hello ASA telemetry job toowrite hello telemetry data tooblob storage.</span></span>

### <a name="device-list"></a><span data-ttu-id="2edb7-261">Seznam zařízení</span><span class="sxs-lookup"><span data-stu-id="2edb7-261">Device list</span></span>

<span data-ttu-id="2edb7-262">Z této stránky portálu řešení hello můžete:</span><span class="sxs-lookup"><span data-stu-id="2edb7-262">From this page in hello solution portal you can:</span></span>

* <span data-ttu-id="2edb7-263">Zřízení nového zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-263">Provision a new device.</span></span> <span data-ttu-id="2edb7-264">Tato akce nastaví hello jedinečné id zařízení a generuje hello ověřovací klíč.</span><span class="sxs-lookup"><span data-stu-id="2edb7-264">This action sets hello unique device id and generates hello authentication key.</span></span> <span data-ttu-id="2edb7-265">Zapíše informace o hello zařízení tooboth hello registru identit služby IoT Hub a hello specifické řešení Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="2edb7-265">It writes information about hello device tooboth hello IoT Hub identity registry and hello solution-specific Cosmos DB database.</span></span>
* <span data-ttu-id="2edb7-266">Správa vlastností zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-266">Manage device properties.</span></span> <span data-ttu-id="2edb7-267">Tato akce zahrnuje zobrazení existujících vlastností a aktualizaci novými vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="2edb7-267">This action includes viewing existing properties and updating with new properties.</span></span>
* <span data-ttu-id="2edb7-268">Odešlete příkazy tooa zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-268">Send commands tooa device.</span></span>
* <span data-ttu-id="2edb7-269">Zobrazit historii příkazů hello pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-269">View hello command history for a device.</span></span>
* <span data-ttu-id="2edb7-270">Povolení a zákaz zařízení.</span><span class="sxs-lookup"><span data-stu-id="2edb7-270">Enable and disable devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2edb7-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2edb7-271">Next steps</span></span>

<span data-ttu-id="2edb7-272">Hello následující příspěvky blogu TechNet poskytují další podrobnosti o hello předkonfigurovanému řešení vzdáleného monitorování:</span><span class="sxs-lookup"><span data-stu-id="2edb7-272">hello following TechNet blog posts provide more detail about hello remote monitoring preconfigured solution:</span></span>

* [<span data-ttu-id="2edb7-273">IoT Suite - pod hello pokličkou - vzdálené monitorování</span><span class="sxs-lookup"><span data-stu-id="2edb7-273">IoT Suite - Under hello Hood - Remote Monitoring</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [<span data-ttu-id="2edb7-274">Sada IoT Suite - Vzdálené monitorování - Přidávání skutečných a simulovaných zařízení</span><span class="sxs-lookup"><span data-stu-id="2edb7-274">IoT Suite - Remote Monitoring - Adding Live and Simulated Devices</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

<span data-ttu-id="2edb7-275">Budete-li pokračovat, Začínáme se službou IoT Suite načtením hello následující články:</span><span class="sxs-lookup"><span data-stu-id="2edb7-275">You can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="2edb7-276">[Připojit vaše zařízení toohello předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="2edb7-276">[Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="2edb7-277">[Oprávnění na webu azureiotsuite.com hello][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="2edb7-277">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
