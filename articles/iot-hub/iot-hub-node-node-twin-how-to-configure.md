---
title: "Vlastnosti twin zařízení Azure IoT Hub aaaUse (uzel) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub dvojčata zařízení tooconfigure zařízení. Aplikace simulovaného zařízení a aplikace služby, která upraví konfiguraci zařízení pomocí dvojče zařízení používáte pro Node.js tooimplement hello sady SDK služby Azure IoT."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: elioda
ms.openlocfilehash: 7ebfe2dfa0876bf04fdbaceae55db76456523e8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices-node"></a><span data-ttu-id="815b9-104">Použití požadovaného vlastnosti tooconfigure zařízení (uzel)</span><span class="sxs-lookup"><span data-stu-id="815b9-104">Use desired properties tooconfigure devices (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="815b9-105">Na konci hello tohoto kurzu budete mít dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="815b9-105">At hello end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="815b9-106">**SimulateDeviceConfiguration.js**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a sestavy hello stav procesu aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="815b9-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="815b9-107">**SetDesiredConfigurationAndQuery.js**, konfigurace v zařízení požadovaného aplikace back-end Node.js, která nastaví hello a dotazy hello proces aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="815b9-107">**SetDesiredConfigurationAndQuery.js**, a Node.js back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="815b9-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="815b9-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="815b9-109">toocomplete tohoto kurzu potřebujete následující hello:</span><span class="sxs-lookup"><span data-stu-id="815b9-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="815b9-110">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="815b9-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="815b9-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="815b9-111">An active Azure account.</span></span> <span data-ttu-id="815b9-112">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="815b9-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="815b9-113">Pokud jste postupovali podle hello [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**; a můžete přejít toohello [ Vytvoření aplikace simulovaného zařízení hello] [ lnk-how-to-configure-createapp] části.</span><span class="sxs-lookup"><span data-stu-id="815b9-113">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**; and you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="815b9-114">Vytvoření aplikace simulovaného zařízení hello</span><span class="sxs-lookup"><span data-stu-id="815b9-114">Create hello simulated device app</span></span>
<span data-ttu-id="815b9-115">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje tooyour hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a v procesu aktualizace konfigurace hello simulated hlásí aktualizace.</span><span class="sxs-lookup"><span data-stu-id="815b9-115">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="815b9-116">Vytvořit novou prázdnou složku s názvem **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="815b9-116">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="815b9-117">V hello **simulatedeviceconfiguration** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="815b9-117">In hello **simulatedeviceconfiguration** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="815b9-118">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="815b9-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="815b9-119">Na příkazovém řádku v hello **simulatedeviceconfiguration** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device**, a **azure-iot zařízení mqtt**balíčku:</span><span class="sxs-lookup"><span data-stu-id="815b9-119">At your command prompt in hello **simulatedeviceconfiguration** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="815b9-120">Pomocí textového editoru, vytvořte novou **SimulateDeviceConfiguration.js** souboru v hello **simulatedeviceconfiguration** složky.</span><span class="sxs-lookup"><span data-stu-id="815b9-120">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in hello **simulatedeviceconfiguration** folder.</span></span>
4. <span data-ttu-id="815b9-121">Přidejte následující kód toohello hello **SimulateDeviceConfiguration.js** souboru a nahradit hello **{zařízení připojovací řetězec}** zástupný symbol hello zařízení připojovací řetězec jste zkopírovali, kdy jste Vytvořit hello **myDeviceId** identitu zařízení:</span><span class="sxs-lookup"><span data-stu-id="815b9-121">Add hello following code toohello **SimulateDeviceConfiguration.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="815b9-122">Hello **klienta** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="815b9-122">hello **Client** object exposes all hello methods required toointeract with device twins from hello device.</span></span> <span data-ttu-id="815b9-123">Hello předchozí kód po jeho inicializuje hello **klienta** objektu, načte hello dvojče zařízení pro **myDeviceId**a připojí obslužnou rutinu pro aktualizaci hello na požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="815b9-123">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId**, and attaches a handler for hello update on desired properties.</span></span> <span data-ttu-id="815b9-124">Obslužná rutina Hello ověřuje, že se požadavku na změnu skutečné konfigurace tak, že porovnáte hello configIds, pak vyvolá metodu, která spustí hello změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="815b9-124">hello handler verifies that there is an actual configuration change request by comparing hello configIds, then invokes a method that starts hello configuration change.</span></span>
   
    <span data-ttu-id="815b9-125">Všimněte si, že pro hello zájmu jednoduchost, předchozí kód hello používá výchozí pevně pro počáteční konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="815b9-125">Note that for hello sake of simplicity, hello previous code uses a hard-coded default for hello inital configuration.</span></span> <span data-ttu-id="815b9-126">Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="815b9-126">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="815b9-127">Události změny požadované vlastnosti jsou vždy jednou vygenerované při připojení zařízení, ujistěte se, že se skutečné změnu v hodnotě hello toocheck požadovaných vlastností před provedením jakékoli akce.</span><span class="sxs-lookup"><span data-stu-id="815b9-127">Desired property change events are always emitted once at device connection, make sure toocheck that there is an actual change in hello desired properties before performing any action.</span></span>
   > 
   > 
5. <span data-ttu-id="815b9-128">Přidejte následující metody před hello hello `client.open()` volání:</span><span class="sxs-lookup"><span data-stu-id="815b9-128">Add hello following methods before hello `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="815b9-129">Hello **initConfigChange** metoda aktualizace ohlášeny objekt twin hello místní zařízení se žádost o aktualizaci konfigurace hello vlastnosti a nastaví hello stav příliš**čekající**, pak aktualizací hello zařízení Twin hello služby.</span><span class="sxs-lookup"><span data-stu-id="815b9-129">hello **initConfigChange** method updates reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="815b9-130">Po úspěšné aktualizaci dvojče zařízení hello, simuluje dlouhotrvající proces, který ukončí v provádění hello **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="815b9-130">After successfully updating hello device twin, it simulates a long running process that terminates in hello execution of **completeConfigChange**.</span></span> <span data-ttu-id="815b9-131">Tato metoda aktualizace hello místní zařízení twin je hlášené vlastnosti nastavení stavu hello příliš**úspěch** a odebírání hello **pendingConfig** objektu.</span><span class="sxs-lookup"><span data-stu-id="815b9-131">This method updates hello local device twin's reported properties setting hello status too**Success** and removing hello **pendingConfig** object.</span></span> <span data-ttu-id="815b9-132">Pak aktualizuje hello dvojče zařízení ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="815b9-132">It then updates hello device twin on hello service.</span></span>
   
    <span data-ttu-id="815b9-133">Hlášen, toosave šířky pásma, jsou aktualizovány vlastnosti zadáním změnit pouze vlastnosti toobe hello (s názvem **oprava** v hello výše kódu), místo nahrazení hello celý dokument.</span><span class="sxs-lookup"><span data-stu-id="815b9-133">Note that, toosave bandwidth, reported properties are updated by specifying only hello properties toobe modified (named **patch** in hello above code), instead of replacing hello whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="815b9-134">V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace.</span><span class="sxs-lookup"><span data-stu-id="815b9-134">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="815b9-135">Některé procesy aktualizace konfigurace může být schopný tooaccommodate změn konfigurace cílového spuštěného hello aktualizace, jiné mohou mít tooqueue je a dalších může odmítněte s chybový stav.</span><span class="sxs-lookup"><span data-stu-id="815b9-135">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, others might have tooqueue them, and others could reject them with an error condition.</span></span> <span data-ttu-id="815b9-136">Ujistěte se, že tooconsider hello požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku hello před zahájením hello změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="815b9-136">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
6. <span data-ttu-id="815b9-137">Spuštění aplikace hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="815b9-137">Run hello device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="815b9-138">Měli byste vidět zprávu hello `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="815b9-138">You should see hello message `retrieved device twin`.</span></span> <span data-ttu-id="815b9-139">Zachovat hello aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="815b9-139">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="815b9-140">Vytvoření aplikace hello service</span><span class="sxs-lookup"><span data-stu-id="815b9-140">Create hello service app</span></span>
<span data-ttu-id="815b9-141">V této části vytvoříte konzolovou aplikaci softwaru Node.js hello této aktualizace *potřeby vlastnosti* na dvojče zařízení hello přidružené **myDeviceId** s nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="815b9-141">In this section, you will create a Node.js console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="815b9-142">Pak dotazuje dvojčata zařízení hello uložené ve hello IoT hub a ukazuje hello rozdíl mezi hello potřeby a který ohlásil konfigurace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="815b9-142">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="815b9-143">Vytvořit novou prázdnou složku s názvem **setdesiredandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="815b9-143">Create a new empty folder called **setdesiredandqueryapp**.</span></span> <span data-ttu-id="815b9-144">V hello **setdesiredandqueryapp** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="815b9-144">In hello **setdesiredandqueryapp** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="815b9-145">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="815b9-145">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="815b9-146">Na příkazovém řádku v hello **setdesiredandqueryapp** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku:</span><span class="sxs-lookup"><span data-stu-id="815b9-146">At your command prompt in hello **setdesiredandqueryapp** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. <span data-ttu-id="815b9-147">Pomocí textového editoru, vytvořte novou **SetDesiredAndQuery.js** souboru v hello **addtagsandqueryapp** složky.</span><span class="sxs-lookup"><span data-stu-id="815b9-147">Using a text editor, create a new **SetDesiredAndQuery.js** file in hello **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="815b9-148">Přidejte následující kód toohello hello **SetDesiredAndQuery.js** souboru a nahradit hello **{iot hub, připojovací řetězec}** zástupný symbol hello jste zkopírovali, když vytvoříte Centrum IoT Hub připojovací řetězec :</span><span class="sxs-lookup"><span data-stu-id="815b9-148">Add hello following code toohello **SetDesiredAndQuery.js** file, and substitute hello **{iot hub connection string}** placeholder with hello IoT Hub connection string you copied when you created your hub:</span></span>
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    <span data-ttu-id="815b9-149">Hello **registru** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello.</span><span class="sxs-lookup"><span data-stu-id="815b9-149">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="815b9-150">Hello předchozí kód po jeho inicializuje hello **registru** objektu, načte hello dvojče zařízení pro **myDeviceId**a aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="815b9-150">hello previous code, after it initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and updates its desired properties with a new telemetry configuration object.</span></span> <span data-ttu-id="815b9-151">Poté zavolá hello **queryTwins** funkce událostí 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="815b9-151">After that, it calls hello **queryTwins** function event 10 seconds.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="815b9-152">Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="815b9-152">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="815b9-153">Použití dotazuje sestavy zobrazující se uživatelům toogenerate napříč mnoho zařízení a ne toodetect změny.</span><span class="sxs-lookup"><span data-stu-id="815b9-153">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="815b9-154">Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="815b9-154">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
    > 
    ><span data-ttu-id="815b9-155">.</span><span class="sxs-lookup"><span data-stu-id="815b9-155">.</span></span>

1. <span data-ttu-id="815b9-156">Přidejte následující kód těsně před hello hello `registry.getDeviceTwin()` volání tooimplement hello **queryTwins** funkce:</span><span class="sxs-lookup"><span data-stu-id="815b9-156">Add hello following code right before hello `registry.getDeviceTwin()` invocation tooimplement hello **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    <span data-ttu-id="815b9-157">Předchozí kód dotazy Hello hello dvojčata zařízení, které jsou uložené ve hello IoT hub a výtisků hello potřeby a hlášené telemetrie konfigurace.</span><span class="sxs-lookup"><span data-stu-id="815b9-157">hello previous code queries hello device twins stored in hello IoT hub and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="815b9-158">Odkazovat toohello [IoT Hub dotazovací jazyk] [ lnk-query] toolearn jak toogenerate bohaté sestavy na všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="815b9-158">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
2. <span data-ttu-id="815b9-159">S **SimulateDeviceConfiguration.js** spuštěná, spusťte aplikaci hello se:</span><span class="sxs-lookup"><span data-stu-id="815b9-159">With **SimulateDeviceConfiguration.js** running, run hello application with:</span></span>
   
        node SetDesiredAndQuery.js 5m
   
    <span data-ttu-id="815b9-160">Měli byste vidět hello změnu z hlášené konfigurace **úspěch** příliš**čekající** příliš**úspěch** znova s novou aktivní hello poslat frekvenci pět minut místo 24 hodiny.</span><span class="sxs-lookup"><span data-stu-id="815b9-160">You should see hello reported configuration change from **Success** too**Pending** too**Success** again with hello new active send frequency of five minutes instead of 24 hours.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="815b9-161">Není zpoždění až minutu tooa mezi hello zařízení sestavy operace a výsledek dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="815b9-161">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="815b9-162">Toto je tooenable hello dotazu infrastruktury toowork ve velmi velkém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="815b9-162">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="815b9-163">tooretrieve konzistentní zobrazení twin jedno zařízení použít hello **getDeviceTwin** metoda v hello **registru** třídy.</span><span class="sxs-lookup"><span data-stu-id="815b9-163">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="815b9-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="815b9-164">Next steps</span></span>
<span data-ttu-id="815b9-165">V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z back-end aplikace a napsali toodetect aplikace simulovaného zařízení, která změnit a simulace aktualizace vícekrokový proces reporting její stav jako  *hlášené vlastnosti* toohello dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="815b9-165">In this tutorial, you set a desired configuration as *desired properties* from a back-end app, and wrote a simulated device app toodetect that change and simulate a multi-step update process reporting its status as *reported properties* toohello device twin.</span></span>

<span data-ttu-id="815b9-166">Použití hello následující toolearn prostředky jak pro:</span><span class="sxs-lookup"><span data-stu-id="815b9-166">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="815b9-167">odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="815b9-167">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="815b9-168">naplánovat nebo provádět operace u velkých sad zařízení najdete v části hello [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="815b9-168">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="815b9-169">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="815b9-169">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

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
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
