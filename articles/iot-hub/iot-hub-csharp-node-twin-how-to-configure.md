---
title: "Vlastnosti twin zařízení Azure IoT Hub aaaUse (.NET nebo uzel) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub dvojčata zařízení tooconfigure zařízení. Používáte zařízení Azure IoT hello SDK pro Node.js tooimplement aplikace simulovaného zařízení a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která upraví konfiguraci zařízení pomocí dvojče zařízení."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
ms.openlocfilehash: 840a1b2e45f4763131299577583aa89015dcdd1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices"></a><span data-ttu-id="72e83-104">Použití zařízení tooconfigure požadované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="72e83-104">Use desired properties tooconfigure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="72e83-105">Na konci hello tohoto kurzu budete mít dvě aplikace konzoly:</span><span class="sxs-lookup"><span data-stu-id="72e83-105">At hello end of this tutorial, you will have two console apps:</span></span>

* <span data-ttu-id="72e83-106">**SimulateDeviceConfiguration.js**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a sestavy hello stav procesu aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="72e83-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="72e83-107">**SetDesiredConfigurationAndQuery**, back-end aplikace .NET, která nastaví hello požadovaných konfigurací na zařízení a dotazů hello proces aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="72e83-107">**SetDesiredConfigurationAndQuery**, a .NET back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="72e83-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="72e83-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="72e83-109">toocomplete tohoto kurzu potřebujete následující hello:</span><span class="sxs-lookup"><span data-stu-id="72e83-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="72e83-110">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="72e83-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="72e83-111">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="72e83-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="72e83-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="72e83-112">An active Azure account.</span></span> <span data-ttu-id="72e83-113">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="72e83-113">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="72e83-114">Pokud jste postupovali podle hello [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="72e83-114">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="72e83-115">V takovém případě můžete přeskočit toohello [aplikaci simulovaného zařízení vytvořit hello] [ lnk-how-to-configure-createapp] části.</span><span class="sxs-lookup"><span data-stu-id="72e83-115">In that case, you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="72e83-116">Vytvoření aplikace simulovaného zařízení hello</span><span class="sxs-lookup"><span data-stu-id="72e83-116">Create hello simulated device app</span></span>
<span data-ttu-id="72e83-117">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje tooyour hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a v procesu aktualizace konfigurace hello simulated hlásí aktualizace.</span><span class="sxs-lookup"><span data-stu-id="72e83-117">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="72e83-118">Vytvořit novou prázdnou složku s názvem **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="72e83-118">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="72e83-119">V hello **simulatedeviceconfiguration** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-119">In hello **simulatedeviceconfiguration** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="72e83-120">Přijměte všechny výchozí hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-120">Accept all hello defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="72e83-121">Na příkazovém řádku v hello **simulatedeviceconfiguration** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** a **azure-iot zařízení mqtt**balíčky:</span><span class="sxs-lookup"><span data-stu-id="72e83-121">At your command prompt in hello **simulatedeviceconfiguration** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="72e83-122">Pomocí textového editoru, vytvořte novou **SimulateDeviceConfiguration.js** souboru v hello **simulatedeviceconfiguration** složky.</span><span class="sxs-lookup"><span data-stu-id="72e83-122">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in hello **simulatedeviceconfiguration** folder.</span></span>
1. <span data-ttu-id="72e83-123">Přidejte následující kód toohello hello **SimulateDeviceConfiguration.js** souboru a nahradit hello **{zařízení připojovací řetězec}** zástupný symbol hello zařízení připojovací řetězec jste zkopírovali, kdy jste Vytvořit hello **myDeviceId** identitu zařízení:</span><span class="sxs-lookup"><span data-stu-id="72e83-123">Add hello following code toohello **SimulateDeviceConfiguration.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    <span data-ttu-id="72e83-124">Hello **klienta** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="72e83-124">hello **Client** object exposes all hello methods required toointeract with device twins from hello device.</span></span> <span data-ttu-id="72e83-125">Tento kód inicializuje hello **klienta** objektu, načte hello dvojče zařízení pro **myDeviceId**a potom připojí obslužnou rutinu pro aktualizaci hello na *potřeby vlastnosti*.</span><span class="sxs-lookup"><span data-stu-id="72e83-125">This code initializes hello **Client** object, retrieves hello device twin for **myDeviceId**, and then attaches a handler for hello update on *desired properties*.</span></span> <span data-ttu-id="72e83-126">Obslužná rutina Hello ověřuje, že se požadavku na změnu skutečné konfigurace tak, že porovnáte hello configIds, pak vyvolá metodu, která spustí hello změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="72e83-126">hello handler verifies that there is an actual configuration change request by comparing hello configIds, then invokes a method that starts hello configuration change.</span></span>
   
    <span data-ttu-id="72e83-127">Všimněte si, že pro hello zájmu jednoduchost, tento kód používá výchozí pevně pro počáteční konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-127">Note that for hello sake of simplicity, this code uses a hard-coded default for hello initial configuration.</span></span> <span data-ttu-id="72e83-128">Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="72e83-128">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="72e83-129">Události změny požadované vlastnosti jsou vždy vygenerované jednou na připojení zařízení.</span><span class="sxs-lookup"><span data-stu-id="72e83-129">Desired property change events are always emitted once at device connection.</span></span> <span data-ttu-id="72e83-130">Zajistěte, aby se skutečné změnu v hodnotě hello toocheck požadovaných vlastností před provedením jakékoli akce.</span><span class="sxs-lookup"><span data-stu-id="72e83-130">Make sure toocheck that there is an actual change in hello desired properties before performing any action.</span></span>
   > 
   > 
1. <span data-ttu-id="72e83-131">Přidejte následující metody před hello hello `client.open()` volání:</span><span class="sxs-lookup"><span data-stu-id="72e83-131">Add hello following methods before hello `client.open()` invocation:</span></span>
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    <span data-ttu-id="72e83-132">Hello **initConfigChange** metoda aktualizace hello hlášené vlastnosti objektu twin hello místní zařízení s konfigurací hello aktualizovat požadavku a nastaví stav hello příliš**čekající**, pak aktualizací hello dvojče zařízení ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-132">hello **initConfigChange** method updates hello reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="72e83-133">Po úspěšné aktualizaci dvojče zařízení hello, simuluje dlouhotrvající proces, který ukončí v provádění hello **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="72e83-133">After successfully updating hello device twin, it simulates a long running process that terminates in hello execution of **completeConfigChange**.</span></span> <span data-ttu-id="72e83-134">Tato metoda aktualizace hello místní hlášené vlastnosti nastavení stavu hello příliš**úspěch** a odebírání hello **pendingConfig** objektu.</span><span class="sxs-lookup"><span data-stu-id="72e83-134">This method updates hello local reported properties setting hello status too**Success** and removing hello **pendingConfig** object.</span></span> <span data-ttu-id="72e83-135">Pak aktualizuje hello dvojče zařízení ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-135">It then updates hello device twin on hello service.</span></span>
   
    <span data-ttu-id="72e83-136">Hlášen, toosave šířky pásma, jsou aktualizovány vlastnosti zadáním změnit pouze vlastnosti toobe hello (s názvem **oprava** v hello výše kódu), místo nahrazení hello celý dokument.</span><span class="sxs-lookup"><span data-stu-id="72e83-136">Note that, toosave bandwidth, reported properties are updated by specifying only hello properties toobe modified (named **patch** in hello above code), instead of replacing hello whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="72e83-137">V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace.</span><span class="sxs-lookup"><span data-stu-id="72e83-137">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="72e83-138">Některé procesy aktualizace konfigurace může být schopný tooaccommodate změn konfigurace cílového spuštěného hello aktualizace, některé může mít tooqueue je a některé by mohly odmítněte s chybový stav.</span><span class="sxs-lookup"><span data-stu-id="72e83-138">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, some might have tooqueue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="72e83-139">Ujistěte se, že tooconsider hello požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku hello před zahájením hello změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="72e83-139">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="72e83-140">Spuštění aplikace hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="72e83-140">Run hello device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="72e83-141">Měli byste vidět zprávu hello `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="72e83-141">You should see hello message `retrieved device twin`.</span></span> <span data-ttu-id="72e83-142">Zachovat hello aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="72e83-142">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="72e83-143">Vytvoření aplikace hello service</span><span class="sxs-lookup"><span data-stu-id="72e83-143">Create hello service app</span></span>
<span data-ttu-id="72e83-144">V této části vytvoříte konzolovou aplikaci .NET této aktualizace hello *potřeby vlastnosti* na dvojče zařízení hello přidružené **myDeviceId** s nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="72e83-144">In this section, you will create a .NET console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="72e83-145">Pak dotazuje dvojčata zařízení hello uložené ve hello IoT hub a ukazuje hello rozdíl mezi hello potřeby a který ohlásil konfigurace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="72e83-145">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="72e83-146">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="72e83-146">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="72e83-147">Název projektu hello **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="72e83-147">Name hello project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. <span data-ttu-id="72e83-149">V Průzkumníku řešení klikněte pravým tlačítkem na hello **SetDesiredConfigurationAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="72e83-149">In Solution Explorer, right-click hello **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="72e83-150">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-150">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="72e83-151">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="72e83-151">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="72e83-153">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="72e83-153">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="72e83-154">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="72e83-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="72e83-155">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-155">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="72e83-156">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="72e83-156">Add hello following method toohello **Program** class:</span></span>
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            try
            {
                while (true)
                {
                    var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                    var results = await query.GetNextAsTwinAsync();
                    foreach (var result in results)
                    {
                        Console.WriteLine("Config report for: {0}", result.DeviceId);
                        Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine();
                    }
                    Thread.Sleep(10000);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
            }
        }
   
    <span data-ttu-id="72e83-157">Hello **registru** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-157">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="72e83-158">Tento kód inicializuje hello **registru** objektu, načte hello dvojče zařízení pro **myDeviceId**a pak aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="72e83-158">This code initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="72e83-159">Poté vyžádá si dvojčata zařízení hello uložené ve hello IoT hub každých 10 sekund a výtisků hello potřeby a hlášené telemetrie konfigurace.</span><span class="sxs-lookup"><span data-stu-id="72e83-159">After that, it queries hello device twins stored in hello IoT hub every 10 seconds, and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="72e83-160">Odkazovat toohello [IoT Hub dotazovací jazyk] [ lnk-query] toolearn jak toogenerate bohaté sestavy na všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="72e83-160">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="72e83-161">Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="72e83-161">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="72e83-162">Použití dotazuje sestavy zobrazující se uživatelům toogenerate napříč mnoho zařízení a ne toodetect změny.</span><span class="sxs-lookup"><span data-stu-id="72e83-162">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="72e83-163">Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="72e83-163">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="72e83-164">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="72e83-164">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. <span data-ttu-id="72e83-165">V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **SetDesiredConfigurationAndQuery** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="72e83-165">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="72e83-166">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-166">Build hello solution.</span></span>
1. <span data-ttu-id="72e83-167">S **SimulateDeviceConfiguration.js** spuštěna, spusťte aplikaci .NET hello pomocí sady Visual Studio **F5** a měli byste vidět hello změnu z hlášené konfigurace **úspěch** příliš**čekající** příliš**úspěch** znova s novou aktivní hello poslat frekvenci pět minut místo 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="72e83-167">With **SimulateDeviceConfiguration.js** running, run hello .NET application from Visual Studio using **F5** and you should see hello reported configuration change from **Success** too**Pending** too**Success** again with hello new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Zařízení byl úspěšně nakonfigurován][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="72e83-169">Není zpoždění až minutu tooa mezi hello zařízení sestavy operace a výsledek dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="72e83-169">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="72e83-170">Toto je tooenable hello dotazu infrastruktury toowork ve velmi velkém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="72e83-170">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="72e83-171">tooretrieve konzistentní zobrazení twin jedno zařízení použít hello **getDeviceTwin** metoda v hello **registru** třídy.</span><span class="sxs-lookup"><span data-stu-id="72e83-171">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="72e83-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72e83-172">Next steps</span></span>
<span data-ttu-id="72e83-173">V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z řešení hello back-end a napsali toodetect aplikace zařízení, která změnit a simulace aktualizace vícekrokový proces reporting její stav prostřednictvím hello hlášené Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="72e83-173">In this tutorial, you set a desired configuration as *desired properties* from hello solution back end, and wrote a device app toodetect that change and simulate a multi-step update process reporting its status through hello reported properties.</span></span>

<span data-ttu-id="72e83-174">Použití hello následující toolearn prostředky jak pro:</span><span class="sxs-lookup"><span data-stu-id="72e83-174">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="72e83-175">odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="72e83-175">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="72e83-176">naplánovat nebo provádět operace u velkých sad zařízení najdete v části hello [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="72e83-176">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="72e83-177">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="72e83-177">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-csharp-node-twin-how-to-configure.md#create-the-simulated-device-app
