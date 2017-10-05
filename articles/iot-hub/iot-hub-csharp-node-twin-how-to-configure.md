---
title: "Použití Azure IoT Hub zařízení dvojici vlastností (.NET nebo uzel) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub pro konfiguraci zařízení. Použití zařízení Azure IoT SDK pro Node.js pro implementaci aplikace simulovaného zařízení a sady SDK pro .NET k implementaci aplikační služby, která upraví konfiguraci zařízení dvojče zařízení pomocí služby Azure IoT."
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
ms.openlocfilehash: 78b4523fa7d0c056f84214429730a5df1bcdcef7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-desired-properties-to-configure-devices"></a><span data-ttu-id="dd8ed-104">Použijte požadované vlastnosti pro konfiguraci zařízení</span><span class="sxs-lookup"><span data-stu-id="dd8ed-104">Use desired properties to configure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="dd8ed-105">Na konci tohoto kurzu budete mít dvě aplikace konzoly:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-105">At the end of this tutorial, you will have two console apps:</span></span>

* <span data-ttu-id="dd8ed-106">**SimulateDeviceConfiguration.js**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a oznámí stav procesu aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports the status of a simulated configuration update process.</span></span>
* <span data-ttu-id="dd8ed-107">**SetDesiredConfigurationAndQuery**, back-end aplikace .NET, která na zařízení nastaví požadované konfigurace a dotazuje proces aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-107">**SetDesiredConfigurationAndQuery**, a .NET back-end app, which sets the desired configuration on a device and queries the configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="dd8ed-108">Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="dd8ed-109">K dokončení tohoto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="dd8ed-110">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="dd8ed-111">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="dd8ed-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-112">An active Azure account.</span></span> <span data-ttu-id="dd8ed-113">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="dd8ed-113">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="dd8ed-114">Pokud jste postupovali podle [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-114">If you followed the [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="dd8ed-115">V takovém případě můžete přeskočit na [vytvoření aplikace simulovaného zařízení] [ lnk-how-to-configure-createapp] části.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-115">In that case, you can skip to the [Create the simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a><span data-ttu-id="dd8ed-116">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="dd8ed-116">Create the simulated device app</span></span>
<span data-ttu-id="dd8ed-117">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje k vaší hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a hlásí aktualizace na proces aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-117">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, waits for a desired configuration update and then reports updates on the simulated configuration update process.</span></span>

1. <span data-ttu-id="dd8ed-118">Vytvořit novou prázdnou složku s názvem **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-118">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="dd8ed-119">V **simulatedeviceconfiguration** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-119">In the **simulatedeviceconfiguration** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="dd8ed-120">Přijměte všechny výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-120">Accept all the defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="dd8ed-121">Na příkazovém řádku v **simulatedeviceconfiguration** složky, spusťte následující příkaz k instalaci **azure-iot-device** a **azure-iot zařízení mqtt** balíčky:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-121">At your command prompt in the **simulatedeviceconfiguration** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="dd8ed-122">Pomocí textového editoru, vytvořte novou **SimulateDeviceConfiguration.js** v soubor **simulatedeviceconfiguration** složky.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-122">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in the **simulatedeviceconfiguration** folder.</span></span>
1. <span data-ttu-id="dd8ed-123">Přidejte následující kód, který **SimulateDeviceConfiguration.js** souboru a nahraďte **{zařízení připojovací řetězec}** zástupného symbolu připojovacím řetězcem zařízení jste zkopírovali, když jste vytvořili **myDeviceId** identitu zařízení:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-123">Add the following code to the **SimulateDeviceConfiguration.js** file, and substitute the **{device connection string}** placeholder with the device connection string you copied when you created the **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="dd8ed-124">**Klienta** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-124">The **Client** object exposes all the methods required to interact with device twins from the device.</span></span> <span data-ttu-id="dd8ed-125">Tento kód inicializuje **klienta** objektu, načte dvojče zařízení pro **myDeviceId**a potom připojí obslužnou rutinu pro aktualizaci na *potřeby vlastnosti*.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-125">This code initializes the **Client** object, retrieves the device twin for **myDeviceId**, and then attaches a handler for the update on *desired properties*.</span></span> <span data-ttu-id="dd8ed-126">Obslužná rutina ověřuje, že se požadavku na změnu skutečné konfigurace tak, že porovnáte configIds, pak vyvolá metodu, která spustí tuto změnu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-126">The handler verifies that there is an actual configuration change request by comparing the configIds, then invokes a method that starts the configuration change.</span></span>
   
    <span data-ttu-id="dd8ed-127">Všimněte si, že z důvodu zjednodušení tento kód používá výchozí pevně pro počáteční konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-127">Note that for the sake of simplicity, this code uses a hard-coded default for the initial configuration.</span></span> <span data-ttu-id="dd8ed-128">Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-128">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="dd8ed-129">Události změny požadované vlastnosti jsou vždy vygenerované jednou na připojení zařízení.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-129">Desired property change events are always emitted once at device connection.</span></span> <span data-ttu-id="dd8ed-130">Ujistěte se, že zkontrolujte, zda je skutečný změnit ve vlastnostech požadované před provedením jakékoli akce.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-130">Make sure to check that there is an actual change in the desired properties before performing any action.</span></span>
   > 
   > 
1. <span data-ttu-id="dd8ed-131">Přidejte následující metody před `client.open()` volání:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-131">Add the following methods before the `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="dd8ed-132">**InitConfigChange** metoda aktualizuje hlášené vlastnosti v objektu twin místní zařízení žádost o aktualizaci konfigurace a nastaví stav na **čekající**, pak aktualizuje dvojče zařízení na Služba.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-132">The **initConfigChange** method updates the reported properties on the local device twin object with the configuration update request and sets the status to **Pending**, then updates the device twin on the service.</span></span> <span data-ttu-id="dd8ed-133">Po úspěšné aktualizaci dvojče zařízení, simuluje dlouhotrvající proces, který ukončí za běhu **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-133">After successfully updating the device twin, it simulates a long running process that terminates in the execution of **completeConfigChange**.</span></span> <span data-ttu-id="dd8ed-134">Tato metoda aktualizace místní hlášené vlastnosti nastavení stavu na **úspěch** a odebírání **pendingConfig** objektu.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-134">This method updates the local reported properties setting the status to **Success** and removing the **pendingConfig** object.</span></span> <span data-ttu-id="dd8ed-135">Pak aktualizuje dvojče zařízení ve službě.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-135">It then updates the device twin on the service.</span></span>
   
    <span data-ttu-id="dd8ed-136">Hlášen uložit šířky pásma, která jsou aktualizovány vlastnosti tak, že zadáte pouze vlastnosti upravovat (s názvem **oprava** ve výše uvedeném kódu), místo nahrazení celý dokument.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-136">Note that, to save bandwidth, reported properties are updated by specifying only the properties to be modified (named **patch** in the above code), instead of replacing the whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="dd8ed-137">V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-137">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="dd8ed-138">Některé procesy aktualizace konfigurace může být schopna přijmout změny konfigurace cílového aktualizace běží, některé může mít do fronty je a některé může odmítnout s chybový stav.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-138">Some configuration update processes might be able to accommodate changes of target configuration while the update is running, some might have to queue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="dd8ed-139">Zajistěte, aby vzít v úvahu požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku před zahájením změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-139">Make sure to consider the desired behavior for your specific configuration process, and add the appropriate logic before initiating the configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="dd8ed-140">Spusťte aplikaci zařízení:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-140">Run the device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="dd8ed-141">Měli byste vidět zprávu `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-141">You should see the message `retrieved device twin`.</span></span> <span data-ttu-id="dd8ed-142">Nechte aplikaci spuštěnou.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-142">Keep the app running.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="dd8ed-143">Vytvořit aplikaci aplikační služby</span><span class="sxs-lookup"><span data-stu-id="dd8ed-143">Create the service app</span></span>
<span data-ttu-id="dd8ed-144">V této části vytvoříte konzolovou aplikaci .NET, která aktualizuje *potřeby vlastnosti* na dvojče zařízení spojené s **myDeviceId** s nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-144">In this section, you will create a .NET console app that updates the *desired properties* on the device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="dd8ed-145">Pak dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub a ukazuje rozdíl mezi požadované a oznámená konfigurace zařízení.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-145">It then queries the device twins stored in the IoT hub and shows the difference between the desired and reported configurations of the device.</span></span>

1. <span data-ttu-id="dd8ed-146">V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-146">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="dd8ed-147">Název projektu **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-147">Name the project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. <span data-ttu-id="dd8ed-149">V Průzkumníku řešení klikněte pravým tlačítkem myši **SetDesiredConfigurationAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="dd8ed-149">In Solution Explorer, right-click the **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="dd8ed-150">V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-150">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="dd8ed-151">Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-151">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="dd8ed-153">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-153">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="dd8ed-154">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-154">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="dd8ed-155">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-155">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="dd8ed-156">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-156">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="dd8ed-157">**Registru** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze služby.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-157">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="dd8ed-158">Tento kód inicializuje **registru** objektu, načte dvojče zařízení pro **myDeviceId**a pak aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-158">This code initializes the **Registry** object, retrieves the device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="dd8ed-159">Poté se dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub každých 10 sekund a vytiskne konfigurace požadované a oznámená telemetrie.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-159">After that, it queries the device twins stored in the IoT hub every 10 seconds, and prints the desired and reported telemetry configurations.</span></span> <span data-ttu-id="dd8ed-160">Odkazovat [IoT Hub dotazovací jazyk] [ lnk-query] se dozvíte, jak chcete generovat sestavy o bohaté na všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-160">Refer to the [IoT Hub query language][lnk-query] to learn how to generate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="dd8ed-161">Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-161">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="dd8ed-162">Použijte dotazy na generování sestavy zobrazující se uživatelům prostřednictvím zařízení a ne ke zjištění změny.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-162">Use queries to generate user-facing reports across many devices, and not to detect changes.</span></span> <span data-ttu-id="dd8ed-163">Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="dd8ed-163">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="dd8ed-164">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-164">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. <span data-ttu-id="dd8ed-165">V Průzkumníku řešení otevřete **nastavit projekty po spuštění...**  a zajistěte, aby **akce** pro **SetDesiredConfigurationAndQuery** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-165">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="dd8ed-166">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-166">Build the solution.</span></span>
1. <span data-ttu-id="dd8ed-167">S **SimulateDeviceConfiguration.js** spuštěná, spusťte aplikaci .NET pomocí sady Visual Studio **F5** a měli byste vidět změnu z hlášené konfigurace **úspěch**k **čekající** k **úspěch** znova s novou aktivní poslat frekvenci pět minut místo 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-167">With **SimulateDeviceConfiguration.js** running, run the .NET application from Visual Studio using **F5** and you should see the reported configuration change from **Success** to **Pending** to **Success** again with the new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Zařízení byl úspěšně nakonfigurován][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="dd8ed-169">Není zpoždění až několik minut mezi operaci sestavy zařízení a výsledek dotazu.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-169">There is a delay of up to a minute between the device report operation and the query result.</span></span> <span data-ttu-id="dd8ed-170">To je umožnit dotazu infrastruktury pro práci na velmi velký rozsah.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-170">This is to enable the query infrastructure to work at very high scale.</span></span> <span data-ttu-id="dd8ed-171">Načtení konzistentní zobrazení používají twin jedno zařízení **getDeviceTwin** metoda v **registru** třídy.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-171">To retrieve consistent views of a single device twin use the **getDeviceTwin** method in the **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="dd8ed-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd8ed-172">Next steps</span></span>
<span data-ttu-id="dd8ed-173">V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z řešení back-end a napsali aplikace na zařízení ke zjištění této změny a simulovat aktualizace vícekrokový proces reporting její stav prostřednictvím hlášení Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-173">In this tutorial, you set a desired configuration as *desired properties* from the solution back end, and wrote a device app to detect that change and simulate a multi-step update process reporting its status through the reported properties.</span></span>

<span data-ttu-id="dd8ed-174">Použijte v následujících zdrojích informací další postup:</span><span class="sxs-lookup"><span data-stu-id="dd8ed-174">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="dd8ed-175">odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="dd8ed-175">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="dd8ed-176">naplánovat nebo udělat operace u velkých sad zařízení najdete [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-176">schedule or perform operations on large sets of devices see the [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="dd8ed-177">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd8ed-177">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
