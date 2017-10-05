---
title: "Použití Azure IoT Hub zařízení dvojici vlastností (uzel) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub pro konfiguraci zařízení. K implementaci aplikace simulovaného zařízení a aplikace služby, která upraví konfiguraci zařízení pomocí dvojče zařízení používáte SDK služby Azure IoT pro Node.js."
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
ms.openlocfilehash: 771106ce7b00a5231d9929e4b5ea34aefe693597
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-desired-properties-to-configure-devices-node"></a><span data-ttu-id="7d993-104">Použijte požadované vlastnosti pro konfiguraci zařízení (uzel)</span><span class="sxs-lookup"><span data-stu-id="7d993-104">Use desired properties to configure devices (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="7d993-105">Na konci tohoto kurzu budete mít dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="7d993-105">At the end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="7d993-106">**SimulateDeviceConfiguration.js**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a oznámí stav procesu aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d993-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports the status of a simulated configuration update process.</span></span>
* <span data-ttu-id="7d993-107">**SetDesiredConfigurationAndQuery.js**, aplikace back-end Node.js, která na zařízení nastaví požadované konfigurace a dotazuje proces aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d993-107">**SetDesiredConfigurationAndQuery.js**, a Node.js back-end app, which sets the desired configuration on a device and queries the configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="7d993-108">Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.</span><span class="sxs-lookup"><span data-stu-id="7d993-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="7d993-109">K dokončení tohoto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="7d993-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="7d993-110">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7d993-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="7d993-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="7d993-111">An active Azure account.</span></span> <span data-ttu-id="7d993-112">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="7d993-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="7d993-113">Pokud jste postupovali podle [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**; a můžete přejít k [Vytvoření aplikace simulovaného zařízení] [ lnk-how-to-configure-createapp] části.</span><span class="sxs-lookup"><span data-stu-id="7d993-113">If you followed the [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**; and you can skip to the [Create the simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a><span data-ttu-id="7d993-114">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="7d993-114">Create the simulated device app</span></span>
<span data-ttu-id="7d993-115">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje k vaší hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a hlásí aktualizace na proces aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d993-115">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, waits for a desired configuration update and then reports updates on the simulated configuration update process.</span></span>

1. <span data-ttu-id="7d993-116">Vytvořit novou prázdnou složku s názvem **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="7d993-116">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="7d993-117">V **simulatedeviceconfiguration** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="7d993-117">In the **simulatedeviceconfiguration** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="7d993-118">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="7d993-118">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="7d993-119">Na příkazovém řádku v **simulatedeviceconfiguration** složky, spusťte následující příkaz k instalaci **azure-iot-device**, a **azure-iot zařízení mqtt** balíček:</span><span class="sxs-lookup"><span data-stu-id="7d993-119">At your command prompt in the **simulatedeviceconfiguration** folder, run the following command to install the **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="7d993-120">Pomocí textového editoru, vytvořte novou **SimulateDeviceConfiguration.js** v soubor **simulatedeviceconfiguration** složky.</span><span class="sxs-lookup"><span data-stu-id="7d993-120">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in the **simulatedeviceconfiguration** folder.</span></span>
4. <span data-ttu-id="7d993-121">Přidejte následující kód, který **SimulateDeviceConfiguration.js** souboru a nahraďte **{zařízení připojovací řetězec}** zástupného symbolu připojovacím řetězcem zařízení jste zkopírovali, když jste vytvořili **myDeviceId** identitu zařízení:</span><span class="sxs-lookup"><span data-stu-id="7d993-121">Add the following code to the **SimulateDeviceConfiguration.js** file, and substitute the **{device connection string}** placeholder with the device connection string you copied when you created the **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="7d993-122">**Klienta** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="7d993-122">The **Client** object exposes all the methods required to interact with device twins from the device.</span></span> <span data-ttu-id="7d993-123">Předchozí kód, jakmile ji inicializuje **klienta** objektu, načte dvojče zařízení pro **myDeviceId**a připojí obslužnou rutinu pro aktualizaci na požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7d993-123">The previous code, after it initializes the **Client** object, retrieves the device twin for **myDeviceId**, and attaches a handler for the update on desired properties.</span></span> <span data-ttu-id="7d993-124">Obslužná rutina ověřuje, že se požadavku na změnu skutečné konfigurace tak, že porovnáte configIds, pak vyvolá metodu, která spustí tuto změnu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d993-124">The handler verifies that there is an actual configuration change request by comparing the configIds, then invokes a method that starts the configuration change.</span></span>
   
    <span data-ttu-id="7d993-125">Všimněte si, že z důvodu zjednodušení předchozí kód používá výchozí pevně pro počáteční konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="7d993-125">Note that for the sake of simplicity, the previous code uses a hard-coded default for the inital configuration.</span></span> <span data-ttu-id="7d993-126">Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="7d993-126">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="7d993-127">Události změny požadované vlastnosti jsou vždy jednou vygenerované při připojení zařízení, nezapomeňte si zkontrolujte, zda je skutečný změnit ve vlastnostech požadované před provedením jakékoli akce.</span><span class="sxs-lookup"><span data-stu-id="7d993-127">Desired property change events are always emitted once at device connection, make sure to check that there is an actual change in the desired properties before performing any action.</span></span>
   > 
   > 
5. <span data-ttu-id="7d993-128">Přidejte následující metody před `client.open()` volání:</span><span class="sxs-lookup"><span data-stu-id="7d993-128">Add the following methods before the `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="7d993-129">**InitConfigChange** metoda aktualizuje hlášené vlastnosti objektu twin místní zařízení žádost o aktualizaci konfigurace a nastaví stav na **čekající**, pak aktualizuje dvojče zařízení na Služba.</span><span class="sxs-lookup"><span data-stu-id="7d993-129">The **initConfigChange** method updates reported properties on the local device twin object with the configuration update request and sets the status to **Pending**, then updates the device twin on the service.</span></span> <span data-ttu-id="7d993-130">Po úspěšné aktualizaci dvojče zařízení, simuluje dlouhotrvající proces, který ukončí za běhu **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="7d993-130">After successfully updating the device twin, it simulates a long running process that terminates in the execution of **completeConfigChange**.</span></span> <span data-ttu-id="7d993-131">Tato metoda aktualizace dvojče místní zařízení hlášené vlastnosti nastavení stavu na **úspěch** a odebírání **pendingConfig** objektu.</span><span class="sxs-lookup"><span data-stu-id="7d993-131">This method updates the local device twin's reported properties setting the status to **Success** and removing the **pendingConfig** object.</span></span> <span data-ttu-id="7d993-132">Pak aktualizuje dvojče zařízení ve službě.</span><span class="sxs-lookup"><span data-stu-id="7d993-132">It then updates the device twin on the service.</span></span>
   
    <span data-ttu-id="7d993-133">Hlášen uložit šířky pásma, která jsou aktualizovány vlastnosti tak, že zadáte pouze vlastnosti upravovat (s názvem **oprava** ve výše uvedeném kódu), místo nahrazení celý dokument.</span><span class="sxs-lookup"><span data-stu-id="7d993-133">Note that, to save bandwidth, reported properties are updated by specifying only the properties to be modified (named **patch** in the above code), instead of replacing the whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7d993-134">V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d993-134">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="7d993-135">Některé procesy aktualizace konfigurace může být schopna přijmout změny konfigurace cílového aktualizace běží, ostatní může mít do fronty je a ostatní může odmítnout s chybový stav.</span><span class="sxs-lookup"><span data-stu-id="7d993-135">Some configuration update processes might be able to accommodate changes of target configuration while the update is running, others might have to queue them, and others could reject them with an error condition.</span></span> <span data-ttu-id="7d993-136">Zajistěte, aby vzít v úvahu požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku před zahájením změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d993-136">Make sure to consider the desired behavior for your specific configuration process, and add the appropriate logic before initiating the configuration change.</span></span>
   > 
   > 
6. <span data-ttu-id="7d993-137">Spusťte aplikaci zařízení:</span><span class="sxs-lookup"><span data-stu-id="7d993-137">Run the device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="7d993-138">Měli byste vidět zprávu `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="7d993-138">You should see the message `retrieved device twin`.</span></span> <span data-ttu-id="7d993-139">Nechte aplikaci spuštěnou.</span><span class="sxs-lookup"><span data-stu-id="7d993-139">Keep the app running.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="7d993-140">Vytvořit aplikaci aplikační služby</span><span class="sxs-lookup"><span data-stu-id="7d993-140">Create the service app</span></span>
<span data-ttu-id="7d993-141">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která aktualizuje *potřeby vlastnosti* na dvojče zařízení spojené s **myDeviceId** s nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="7d993-141">In this section, you will create a Node.js console app that updates the *desired properties* on the device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="7d993-142">Pak dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub a ukazuje rozdíl mezi požadované a oznámená konfigurace zařízení.</span><span class="sxs-lookup"><span data-stu-id="7d993-142">It then queries the device twins stored in the IoT hub and shows the difference between the desired and reported configurations of the device.</span></span>

1. <span data-ttu-id="7d993-143">Vytvořit novou prázdnou složku s názvem **setdesiredandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="7d993-143">Create a new empty folder called **setdesiredandqueryapp**.</span></span> <span data-ttu-id="7d993-144">V **setdesiredandqueryapp** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="7d993-144">In the **setdesiredandqueryapp** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="7d993-145">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="7d993-145">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="7d993-146">Na příkazovém řádku v **setdesiredandqueryapp** složky, spusťte následující příkaz k instalaci **azure-iothub** balíčku:</span><span class="sxs-lookup"><span data-stu-id="7d993-146">At your command prompt in the **setdesiredandqueryapp** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. <span data-ttu-id="7d993-147">Pomocí textového editoru, vytvořte novou **SetDesiredAndQuery.js** v soubor **addtagsandqueryapp** složky.</span><span class="sxs-lookup"><span data-stu-id="7d993-147">Using a text editor, create a new **SetDesiredAndQuery.js** file in the **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="7d993-148">Přidejte následující kód, který **SetDesiredAndQuery.js** souboru a nahraďte **{iot hub, připojovací řetězec}** zástupný symbol jste zkopírovali, když vytvoříte Centrum IoT Hub připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="7d993-148">Add the following code to the **SetDesiredAndQuery.js** file, and substitute the **{iot hub connection string}** placeholder with the IoT Hub connection string you copied when you created your hub:</span></span>
   
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

    <span data-ttu-id="7d993-149">**Registru** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze služby.</span><span class="sxs-lookup"><span data-stu-id="7d993-149">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="7d993-150">Předchozí kód, jakmile ji inicializuje **registru** objektu, načte dvojče zařízení pro **myDeviceId**a aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="7d993-150">The previous code, after it initializes the **Registry** object, retrieves the device twin for **myDeviceId**, and updates its desired properties with a new telemetry configuration object.</span></span> <span data-ttu-id="7d993-151">Poté zavolá **queryTwins** funkce událostí 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="7d993-151">After that, it calls the **queryTwins** function event 10 seconds.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7d993-152">Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="7d993-152">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="7d993-153">Použijte dotazy na generování sestavy zobrazující se uživatelům prostřednictvím zařízení a ne ke zjištění změny.</span><span class="sxs-lookup"><span data-stu-id="7d993-153">Use queries to generate user-facing reports across many devices, and not to detect changes.</span></span> <span data-ttu-id="7d993-154">Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="7d993-154">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
    > 
    ><span data-ttu-id="7d993-155">.</span><span class="sxs-lookup"><span data-stu-id="7d993-155">.</span></span>

1. <span data-ttu-id="7d993-156">Přidejte následující kód těsně před `registry.getDeviceTwin()` volání implementovat **queryTwins** funkce:</span><span class="sxs-lookup"><span data-stu-id="7d993-156">Add the following code right before the `registry.getDeviceTwin()` invocation to implement the **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
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
   
    <span data-ttu-id="7d993-157">Předchozí kód dotazy dvojčata zařízení uložené ve službě IoT hub a vytiskne konfigurace požadované a oznámená telemetrie.</span><span class="sxs-lookup"><span data-stu-id="7d993-157">The previous code queries the device twins stored in the IoT hub and prints the desired and reported telemetry configurations.</span></span> <span data-ttu-id="7d993-158">Odkazovat [IoT Hub dotazovací jazyk] [ lnk-query] se dozvíte, jak chcete generovat sestavy o bohaté na všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="7d993-158">Refer to the [IoT Hub query language][lnk-query] to learn how to generate rich reports across all your devices.</span></span>
2. <span data-ttu-id="7d993-159">S **SimulateDeviceConfiguration.js** spuštěná, spusťte aplikaci klávesou:</span><span class="sxs-lookup"><span data-stu-id="7d993-159">With **SimulateDeviceConfiguration.js** running, run the application with:</span></span>
   
        node SetDesiredAndQuery.js 5m
   
    <span data-ttu-id="7d993-160">Byste měli vidět změnu z hlášené konfigurace **úspěch** k **čekající** k **úspěch** znova s novou aktivní poslat frekvenci pět minut místo 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="7d993-160">You should see the reported configuration change from **Success** to **Pending** to **Success** again with the new active send frequency of five minutes instead of 24 hours.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="7d993-161">Není zpoždění až několik minut mezi operaci sestavy zařízení a výsledek dotazu.</span><span class="sxs-lookup"><span data-stu-id="7d993-161">There is a delay of up to a minute between the device report operation and the query result.</span></span> <span data-ttu-id="7d993-162">To je umožnit dotazu infrastruktury pro práci na velmi velký rozsah.</span><span class="sxs-lookup"><span data-stu-id="7d993-162">This is to enable the query infrastructure to work at very high scale.</span></span> <span data-ttu-id="7d993-163">Načtení konzistentní zobrazení používají twin jedno zařízení **getDeviceTwin** metoda v **registru** třídy.</span><span class="sxs-lookup"><span data-stu-id="7d993-163">To retrieve consistent views of a single device twin use the **getDeviceTwin** method in the **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="7d993-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d993-164">Next steps</span></span>
<span data-ttu-id="7d993-165">V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z back-end aplikace a napsali aplikace simulovaného zařízení ke zjištění této změny a simulovat aktualizace vícekrokový proces reporting její stav jako  *hlášené vlastnosti* k dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="7d993-165">In this tutorial, you set a desired configuration as *desired properties* from a back-end app, and wrote a simulated device app to detect that change and simulate a multi-step update process reporting its status as *reported properties* to the device twin.</span></span>

<span data-ttu-id="7d993-166">Použijte v následujících zdrojích informací další postup:</span><span class="sxs-lookup"><span data-stu-id="7d993-166">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="7d993-167">odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="7d993-167">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="7d993-168">naplánovat nebo udělat operace u velkých sad zařízení najdete [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="7d993-168">schedule or perform operations on large sets of devices see the [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="7d993-169">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="7d993-169">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
