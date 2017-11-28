---
title: "úlohy aaaSchedule službou Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak tooschedule Azure IoT Hub úlohy tooinvoke přímá metoda na několika zařízeních. SDK služby Azure IoT hello se používá pro Node.js tooimplement hello simulované zařízení aplikace a úlohy hello toorun aplikace služby."
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
ms.openlocfilehash: be293362447fbcddaa3433b66f208f22545fe0c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a><span data-ttu-id="a793a-104">Úlohy plán a všesměrového vysílání (uzel)</span><span class="sxs-lookup"><span data-stu-id="a793a-104">Schedule and broadcast jobs (Node)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="a793a-105">Azure IoT Hub je plně spravovaná služba, která umožňuje back-end aplikačním toocreate a sledování úloh, které naplánovat a aktualizovat miliony zařízení.</span><span class="sxs-lookup"><span data-stu-id="a793a-105">Azure IoT Hub is a fully managed service that enables a back-end app toocreate and track jobs that schedule and update millions of devices.</span></span>  <span data-ttu-id="a793a-106">Úlohy lze použít pro hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="a793a-106">Jobs can be used for hello following actions:</span></span>

* <span data-ttu-id="a793a-107">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="a793a-107">Update desired properties</span></span>
* <span data-ttu-id="a793a-108">Aktualizace značky</span><span class="sxs-lookup"><span data-stu-id="a793a-108">Update tags</span></span>
* <span data-ttu-id="a793a-109">Vyvolání metody přímé</span><span class="sxs-lookup"><span data-stu-id="a793a-109">Invoke direct methods</span></span>

<span data-ttu-id="a793a-110">Koncepčně úlohu zabalí jednu z těchto akcí a sleduje hello průběh provádění na skupiny zařízení, který je definován v dotazu twin zařízení.</span><span class="sxs-lookup"><span data-stu-id="a793a-110">Conceptually, a job wraps one of these actions and tracks hello progress of execution against a set of devices, which is defined by a device twin query.</span></span>  <span data-ttu-id="a793a-111">Například back-end aplikace můžete použít tooinvoke úlohy metoda restartu na 10 000 zařízení, určeného zařízení twin dotazu a naplánovány na datum v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="a793a-111">For example, a back-end app can use a job tooinvoke a reboot method on 10,000 devices, specified by a device twin query and scheduled at a future time.</span></span>  <span data-ttu-id="a793a-112">Aplikace pak můžete sledovat průběh každé z těchto zařízení přijímat a provést metodu restartování hello.</span><span class="sxs-lookup"><span data-stu-id="a793a-112">That application can then track progress as each of those devices receive and execute hello reboot method.</span></span>

<span data-ttu-id="a793a-113">Další informace o každém z těchto funkcí v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="a793a-113">Learn more about each of these capabilities in these articles:</span></span>

* <span data-ttu-id="a793a-114">Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení] [ lnk-get-started-twin] a [kurz: jak dvojče zařízení toouse vlastnosti][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="a793a-114">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How toouse device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="a793a-115">přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody] [ lnk-dev-methods] a [kurz: přímé metody][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="a793a-115">direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="a793a-116">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="a793a-116">This tutorial shows you how to:</span></span>

* <span data-ttu-id="a793a-117">Vytvoření aplikace simulovaného zařízení, která má přímá metoda, která umožňuje **lockDoor** které je možné volat v back-end hello řešení.</span><span class="sxs-lookup"><span data-stu-id="a793a-117">Create a simulated device app that has a direct method which enables **lockDoor** which can be called by hello solution back end.</span></span>
* <span data-ttu-id="a793a-118">Vytvořte konzolovou aplikaci softwaru Node.js hello tohoto volání **lockDoor** přímá metoda v aplikaci simulovaného zařízení hello pomocí úlohy a aktualizace hello požadovaných vlastností pomocí úlohy zařízení.</span><span class="sxs-lookup"><span data-stu-id="a793a-118">Create a Node.js console app that calls hello **lockDoor** direct method in hello simulated device app using a job and updates hello desired properties using a device job.</span></span>

<span data-ttu-id="a793a-119">Na konci hello tohoto kurzu máte dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="a793a-119">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="a793a-120">**simDevice.js**, který připojí tooyour IoT hub s identitou zařízení hello a přijímá **lockDoor** přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="a793a-120">**simDevice.js**, which connects tooyour IoT hub with hello device identity and receives a **lockDoor** direct method.</span></span>

<span data-ttu-id="a793a-121">**scheduleJobService.js**, která volá přímá metoda hello simulované zařízení aplikace a aktualizace hello zařízení požadované vlastnosti pro dvojici pomocí úlohy.</span><span class="sxs-lookup"><span data-stu-id="a793a-121">**scheduleJobService.js**, which calls a direct method in hello simulated device app and update hello device twin's desired properties using a job.</span></span>

<span data-ttu-id="a793a-122">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="a793a-122">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="a793a-123">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="a793a-123">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="a793a-124">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="a793a-124">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="a793a-125">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a793a-125">An active Azure account.</span></span> <span data-ttu-id="a793a-126">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="a793a-126">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="a793a-127">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="a793a-127">Create a simulated device app</span></span>
<span data-ttu-id="a793a-128">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje tooa přímá metoda volá hello cloudu, což aktivuje restartu simulované zařízení a hello používá hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a při jejich poslední restartován.</span><span class="sxs-lookup"><span data-stu-id="a793a-128">In this section, you create a Node.js console app that responds tooa direct method called by hello cloud, which triggers a simulated device reboot and uses hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="a793a-129">Vytvořit novou prázdnou složku s názvem **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="a793a-129">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="a793a-130">V hello **simDevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="a793a-130">In hello **simDevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="a793a-131">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="a793a-131">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a793a-132">Na příkazovém řádku v hello **simDevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíček:</span><span class="sxs-lookup"><span data-stu-id="a793a-132">At your command prompt in hello **simDevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="a793a-133">Pomocí textového editoru, vytvořte novou **simDevice.js** souboru v hello **simDevice** složky.</span><span class="sxs-lookup"><span data-stu-id="a793a-133">Using a text editor, create a new **simDevice.js** file in hello **simDevice** folder.</span></span>
4. <span data-ttu-id="a793a-134">Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **simDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="a793a-134">Add hello following 'require' statements at hello start of hello **simDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="a793a-135">Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance.</span><span class="sxs-lookup"><span data-stu-id="a793a-135">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="a793a-136">Přidejte následující funkce toohandle hello hello **lockDoor** metoda.</span><span class="sxs-lookup"><span data-stu-id="a793a-136">Add hello following function toohandle hello **lockDoor** method.</span></span>
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. <span data-ttu-id="a793a-137">Přidejte následující kód tooregister hello obslužné rutiny pro hello hello **lockDoor** metoda.</span><span class="sxs-lookup"><span data-stu-id="a793a-137">Add hello following code tooregister hello handler for hello **lockDoor** method.</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. <span data-ttu-id="a793a-138">Uložte a zavřete hello **simDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="a793a-138">Save and close hello **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="a793a-139">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="a793a-139">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="a793a-140">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="a793a-140">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a><span data-ttu-id="a793a-141">Plán úlohy pro volání přímá metoda a aktualizace vlastností dvojče zařízení</span><span class="sxs-lookup"><span data-stu-id="a793a-141">Schedule jobs for calling a direct method and updating a device twin's properties</span></span>
<span data-ttu-id="a793a-142">V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje vzdálené **lockDoor** na zařízení pomocí vlastností přímé metoda a aktualizace hello dvojče zařízení společnosti.</span><span class="sxs-lookup"><span data-stu-id="a793a-142">In this section, you create a Node.js console app that initiates a remote **lockDoor** on a device using a direct method and update hello device twin's properties.</span></span>

1. <span data-ttu-id="a793a-143">Vytvořit novou prázdnou složku s názvem **scheduleJobService**.</span><span class="sxs-lookup"><span data-stu-id="a793a-143">Create a new empty folder called **scheduleJobService**.</span></span>  <span data-ttu-id="a793a-144">V hello **scheduleJobService** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="a793a-144">In hello **scheduleJobService** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="a793a-145">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="a793a-145">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a793a-146">Na příkazovém řádku v hello **scheduleJobService** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:</span><span class="sxs-lookup"><span data-stu-id="a793a-146">At your command prompt in hello **scheduleJobService** folder, run hello following command tooinstall hello **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub uuid --save
    ```
3. <span data-ttu-id="a793a-147">Pomocí textového editoru, vytvořte novou **scheduleJobService.js** souboru v hello **scheduleJobService** složky.</span><span class="sxs-lookup"><span data-stu-id="a793a-147">Using a text editor, create a new **scheduleJobService.js** file in hello **scheduleJobService** folder.</span></span>
4. <span data-ttu-id="a793a-148">Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_gscheduleJobServiceetstarted_service.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="a793a-148">Add hello following 'require' statements at hello start of hello **dmpatterns_gscheduleJobServiceetstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. <span data-ttu-id="a793a-149">Přidejte následující deklarace proměnných hello a nahraďte zástupné hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="a793a-149">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="a793a-150">Přidejte následující funkce, které budou použité toomonitor hello provádění úlohy hello hello:</span><span class="sxs-lookup"><span data-stu-id="a793a-150">Add hello following function that will be used toomonitor hello execution of hello job:</span></span>
   
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
7. <span data-ttu-id="a793a-151">Přidejte následující kód tooschedule hello úlohu, která volá metodu zařízení hello hello:</span><span class="sxs-lookup"><span data-stu-id="a793a-151">Add hello following code tooschedule hello job that calls hello device method:</span></span>
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable tooprocess method
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
8. <span data-ttu-id="a793a-152">Přidejte následující kód tooschedule hello úlohy tooupdate hello dvojče zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="a793a-152">Add hello following code tooschedule hello job tooupdate hello device twin:</span></span>
   
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
9. <span data-ttu-id="a793a-153">Uložte a zavřete hello **scheduleJobService.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="a793a-153">Save and close hello **scheduleJobService.js** file.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="a793a-154">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="a793a-154">Run hello applications</span></span>
<span data-ttu-id="a793a-155">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a793a-155">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="a793a-156">Na příkazovém řádku hello v hello **simDevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.</span><span class="sxs-lookup"><span data-stu-id="a793a-156">At hello command prompt in hello **simDevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node simDevice.js
    ```
2. <span data-ttu-id="a793a-157">Na příkazovém řádku hello v hello **scheduleJobService** složky, spusťte následující příkaz tootrigger hello úlohy toolock hello dveře a aktualizace hello twin hello</span><span class="sxs-lookup"><span data-stu-id="a793a-157">At hello command prompt in hello **scheduleJobService** folder, run hello following command tootrigger hello jobs toolock hello door and update hello twin</span></span>
   
    ```
    node scheduleJobService.js
    ```
3. <span data-ttu-id="a793a-158">Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="a793a-158">You see hello device response toohello direct method in hello console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a793a-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a793a-159">Next steps</span></span>
<span data-ttu-id="a793a-160">V tomto kurzu jste použili úlohy tooschedule zařízení tooa přímá metoda a hello aktualizace vlastností dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="a793a-160">In this tutorial, you used a job tooschedule a direct method tooa device and hello update of hello device twin's properties.</span></span>

<span data-ttu-id="a793a-161">toocontinue Začínáme se službou IoT Hub a vzory správy zařízení, jako je vzdálené přes hello letecké firmware aktualizace, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="a793a-161">toocontinue getting started with IoT Hub and device management patterns such as remote over hello air firmware update, see:</span></span>

<span data-ttu-id="a793a-162">[Kurz: Jak toodo firmwarem aktualizovat][lnk-fwupdate]</span><span class="sxs-lookup"><span data-stu-id="a793a-162">[Tutorial: How toodo a firmware update][lnk-fwupdate]</span></span>

<span data-ttu-id="a793a-163">najdete v části Začínáme se službou IoT Hub, toocontinue [Začínáme se službou Azure IoT Edge][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="a793a-163">toocontinue getting started with IoT Hub, see [Getting started with Azure IoT Edge][lnk-iot-edge].</span></span>

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
