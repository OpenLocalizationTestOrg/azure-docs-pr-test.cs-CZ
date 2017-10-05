---
title: "Plánování úloh službou Azure IoT Hub (uzel) | Microsoft Docs"
description: "Popisuje, jak naplánovat úlohu služby Azure IoT Hub pro vyvolání přímé metody na několika zařízeních. SDK služby Azure IoT pro Node.js použijete k implementaci aplikace simulovaného zařízení a aplikační služby, který chcete spustit úlohu."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: 42e594dc6a8a8be619b5652bf8e44cf883650489
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a><span data-ttu-id="3a339-104">Úlohy plán a všesměrového vysílání (uzel)</span><span class="sxs-lookup"><span data-stu-id="3a339-104">Schedule and broadcast jobs (Node)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="3a339-105">Azure IoT Hub je plně spravovaná služba, která umožňuje back-end aplikačním k vytvoření a sledování úloh, které naplánovat a aktualizovat miliony zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a339-105">Azure IoT Hub is a fully managed service that enables a back-end app to create and track jobs that schedule and update millions of devices.</span></span>  <span data-ttu-id="3a339-106">Úlohy lze použít pro následující akce:</span><span class="sxs-lookup"><span data-stu-id="3a339-106">Jobs can be used for the following actions:</span></span>

* <span data-ttu-id="3a339-107">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="3a339-107">Update desired properties</span></span>
* <span data-ttu-id="3a339-108">Aktualizace značky</span><span class="sxs-lookup"><span data-stu-id="3a339-108">Update tags</span></span>
* <span data-ttu-id="3a339-109">Vyvolání metody přímé</span><span class="sxs-lookup"><span data-stu-id="3a339-109">Invoke direct methods</span></span>

<span data-ttu-id="3a339-110">Úloha koncepčně, zabalí jednu z těchto akcí a sleduje průběh provádění na skupiny zařízení, který je definován v dotazu twin zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a339-110">Conceptually, a job wraps one of these actions and tracks the progress of execution against a set of devices, which is defined by a device twin query.</span></span>  <span data-ttu-id="3a339-111">Back-end aplikace můžete například použít úlohu pro vyvolání metody restartu na 10 000 zařízení, určeného zařízení twin dotazu a naplánovány na datum v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="3a339-111">For example, a back-end app can use a job to invoke a reboot method on 10,000 devices, specified by a device twin query and scheduled at a future time.</span></span>  <span data-ttu-id="3a339-112">Aplikace pak můžete sledovat průběh každé z těchto zařízení přijímat a provést metodu restartování.</span><span class="sxs-lookup"><span data-stu-id="3a339-112">That application can then track progress as each of those devices receive and execute the reboot method.</span></span>

<span data-ttu-id="3a339-113">Další informace o každém z těchto funkcí v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="3a339-113">Learn more about each of these capabilities in these articles:</span></span>

* <span data-ttu-id="3a339-114">Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení] [ lnk-get-started-twin] a [kurz: použití dvojici vlastností zařízení][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="3a339-114">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How to use device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="3a339-115">přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody] [ lnk-dev-methods] a [kurz: přímé metody][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="3a339-115">direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="3a339-116">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="3a339-116">This tutorial shows you how to:</span></span>

* <span data-ttu-id="3a339-117">Vytvoření aplikace simulovaného zařízení, která má přímá metoda, která umožňuje **lockDoor** které je možné volat v back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="3a339-117">Create a simulated device app that has a direct method which enables **lockDoor** which can be called by the solution back end.</span></span>
* <span data-ttu-id="3a339-118">Vytvořte konzolovou aplikaci softwaru Node.js, která volá **lockDoor** přímá metoda v aplikaci simulovaného zařízení pomocí úlohy a aktualizace požadované vlastnosti pomocí úlohy zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a339-118">Create a Node.js console app that calls the **lockDoor** direct method in the simulated device app using a job and updates the desired properties using a device job.</span></span>

<span data-ttu-id="3a339-119">Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="3a339-119">At the end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="3a339-120">**simDevice.js**, který připojuje ke službě IoT hub s identitou zařízení a přijímá **lockDoor** přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="3a339-120">**simDevice.js**, which connects to your IoT hub with the device identity and receives a **lockDoor** direct method.</span></span>

<span data-ttu-id="3a339-121">**scheduleJobService.js**, která volá metodu přímé v aplikaci simulovaného zařízení a aktualizací dvojče zařízení požadovaných vlastností pomocí úlohy.</span><span class="sxs-lookup"><span data-stu-id="3a339-121">**scheduleJobService.js**, which calls a direct method in the simulated device app and update the device twin's desired properties using a job.</span></span>

<span data-ttu-id="3a339-122">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3a339-122">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="3a339-123">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="3a339-123">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="3a339-124">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje postup instalace Node.js pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="3a339-124">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="3a339-125">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3a339-125">An active Azure account.</span></span> <span data-ttu-id="3a339-126">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="3a339-126">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="3a339-127">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="3a339-127">Create a simulated device app</span></span>
<span data-ttu-id="3a339-128">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje na přímé metodu s názvem cloudem, který aktivuje restartu simulované zařízení a používá hlášené vlastnosti povolit zařízení twin dotazy k identifikaci zařízení, a při jejich poslední restartován.</span><span class="sxs-lookup"><span data-stu-id="3a339-128">In this section, you create a Node.js console app that responds to a direct method called by the cloud, which triggers a simulated device reboot and uses the reported properties to enable device twin queries to identify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="3a339-129">Vytvořit novou prázdnou složku s názvem **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="3a339-129">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="3a339-130">V **simDevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="3a339-130">In the **simDevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="3a339-131">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3a339-131">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="3a339-132">Na příkazovém řádku v **simDevice** složky, spusťte následující příkaz k instalaci **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíčku:</span><span class="sxs-lookup"><span data-stu-id="3a339-132">At your command prompt in the **simDevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="3a339-133">Pomocí textového editoru, vytvořte novou **simDevice.js** v soubor **simDevice** složky.</span><span class="sxs-lookup"><span data-stu-id="3a339-133">Using a text editor, create a new **simDevice.js** file in the **simDevice** folder.</span></span>
4. <span data-ttu-id="3a339-134">Přidejte následující příkazy na začátku "vyžadovat" **simDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="3a339-134">Add the following 'require' statements at the start of the **simDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="3a339-135">Přidejte proměnnou **connectionString** a použijte ji k vytvoření instance **klienta**.</span><span class="sxs-lookup"><span data-stu-id="3a339-135">Add a **connectionString** variable and use it to create a **Client** instance.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="3a339-136">Přidejte následující funkci pro zpracování **lockDoor** metoda.</span><span class="sxs-lookup"><span data-stu-id="3a339-136">Add the following function to handle the **lockDoor** method.</span></span>
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. <span data-ttu-id="3a339-137">Přidejte následující kód pro obslužnou rutinu pro registraci **lockDoor** metoda.</span><span class="sxs-lookup"><span data-stu-id="3a339-137">Add the following code to register the handler for the **lockDoor** method.</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. <span data-ttu-id="3a339-138">Uložte a zavřete **simDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="3a339-138">Save and close the **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="3a339-139">Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="3a339-139">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="3a339-140">V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="3a339-140">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a><span data-ttu-id="3a339-141">Plán úlohy pro volání přímá metoda a aktualizace vlastností dvojče zařízení</span><span class="sxs-lookup"><span data-stu-id="3a339-141">Schedule jobs for calling a direct method and updating a device twin's properties</span></span>
<span data-ttu-id="3a339-142">V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje vzdálené **lockDoor** na zařízení pomocí jiné metody přímé a aktualizovat vlastnosti dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a339-142">In this section, you create a Node.js console app that initiates a remote **lockDoor** on a device using a direct method and update the device twin's properties.</span></span>

1. <span data-ttu-id="3a339-143">Vytvořit novou prázdnou složku s názvem **scheduleJobService**.</span><span class="sxs-lookup"><span data-stu-id="3a339-143">Create a new empty folder called **scheduleJobService**.</span></span>  <span data-ttu-id="3a339-144">V **scheduleJobService** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="3a339-144">In the **scheduleJobService** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="3a339-145">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3a339-145">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="3a339-146">Na příkazovém řádku v **scheduleJobService** složky, spusťte následující příkaz k instalaci **azure-iothub** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíček:</span><span class="sxs-lookup"><span data-stu-id="3a339-146">At your command prompt in the **scheduleJobService** folder, run the following command to install the **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub uuid --save
    ```
3. <span data-ttu-id="3a339-147">Pomocí textového editoru, vytvořte novou **scheduleJobService.js** v soubor **scheduleJobService** složky.</span><span class="sxs-lookup"><span data-stu-id="3a339-147">Using a text editor, create a new **scheduleJobService.js** file in the **scheduleJobService** folder.</span></span>
4. <span data-ttu-id="3a339-148">Přidejte následující příkazy na začátku "vyžadovat" **dmpatterns_gscheduleJobServiceetstarted_service.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="3a339-148">Add the following 'require' statements at the start of the **dmpatterns_gscheduleJobServiceetstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. <span data-ttu-id="3a339-149">Přidejte následující deklarace proměnných a nahraďte zástupné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3a339-149">Add the following variable declarations and replace the placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="3a339-150">Přidejte následující funkce, které se používá k monitorování provádění úlohy:</span><span class="sxs-lookup"><span data-stu-id="3a339-150">Add the following function that will be used to monitor the execution of the job:</span></span>
   
    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```
7. <span data-ttu-id="3a339-151">Přidejte následující kód do naplánovat úlohu, která volá metodu zařízení:</span><span class="sxs-lookup"><span data-stu-id="3a339-151">Add the following code to schedule the job that calls the device method:</span></span>
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable to process method
    };
   
    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
8. <span data-ttu-id="3a339-152">Přidejte následující kód k naplánování úlohy aktualizace dvojče zařízení:</span><span class="sxs-lookup"><span data-stu-id="3a339-152">Add the following code to schedule the job to update the device twin:</span></span>
   
    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };
   
    var twinJobId = uuid.v4();
   
    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
9. <span data-ttu-id="3a339-153">Uložte a zavřete **scheduleJobService.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="3a339-153">Save and close the **scheduleJobService.js** file.</span></span>

## <a name="run-the-applications"></a><span data-ttu-id="3a339-154">Spuštění aplikací</span><span class="sxs-lookup"><span data-stu-id="3a339-154">Run the applications</span></span>
<span data-ttu-id="3a339-155">Nyní můžete spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a339-155">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="3a339-156">Na příkazovém řádku v **simDevice** složky, spusťte následující příkaz, aby začal přijímat přímá metoda restartování.</span><span class="sxs-lookup"><span data-stu-id="3a339-156">At the command prompt in the **simDevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node simDevice.js
    ```
2. <span data-ttu-id="3a339-157">Na příkazovém řádku v **scheduleJobService** složky, spusťte následující příkaz k aktivaci úlohy, které mají zamknout dveře a aktualizovat twin</span><span class="sxs-lookup"><span data-stu-id="3a339-157">At the command prompt in the **scheduleJobService** folder, run the following command to trigger the jobs to lock the door and update the twin</span></span>
   
    ```
    node scheduleJobService.js
    ```
3. <span data-ttu-id="3a339-158">Zobrazí zařízení odpověď na přímá metoda v konzole.</span><span class="sxs-lookup"><span data-stu-id="3a339-158">You see the device response to the direct method in the console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a339-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a339-159">Next steps</span></span>
<span data-ttu-id="3a339-160">V tomto kurzu jste použili úlohu při plánování přímá metoda zařízení a aktualizaci vlastností dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a339-160">In this tutorial, you used a job to schedule a direct method to a device and the update of the device twin's properties.</span></span>

<span data-ttu-id="3a339-161">Chcete-li pokračovat, Začínáme se službou IoT Hub a vzory správy zařízení, jako je vzdálené přes aktualizaci firmwaru letecké, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="3a339-161">To continue getting started with IoT Hub and device management patterns such as remote over the air firmware update, see:</span></span>

<span data-ttu-id="3a339-162">[Kurz: Jak to provést aktualizaci firmwaru][lnk-fwupdate]</span><span class="sxs-lookup"><span data-stu-id="3a339-162">[Tutorial: How to do a firmware update][lnk-fwupdate]</span></span>

<span data-ttu-id="3a339-163">Chcete-li pokračovat, Začínáme se službou IoT Hub, najdete v části [Začínáme se službou Azure IoT Edge][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="3a339-163">To continue getting started with IoT Hub, see [Getting started with Azure IoT Edge][lnk-iot-edge].</span></span>

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
