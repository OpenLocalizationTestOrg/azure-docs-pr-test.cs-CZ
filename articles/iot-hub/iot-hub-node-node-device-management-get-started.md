---
title: "aaaGet spuštění se správou zařízení Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak toouse tooinitiate správy zařízení IoT Hub vzdáleném zařízení restartovat. Používáte hello Azure IoT SDK pro Node.js tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: juanpere
ms.openlocfilehash: 5dd1878e71231850fb95f4170b823f1e86c3ee83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-node"></a><span data-ttu-id="af73c-104">Začínáme se správou zařízení (uzel)</span><span class="sxs-lookup"><span data-stu-id="af73c-104">Get started with device management (Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="af73c-105">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="af73c-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="af73c-106">Použijte hello portálu toocreate Azure IoT Hub a vytvoření identity zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="af73c-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="af73c-107">Vytvoření aplikace simulovaného zařízení, která obsahuje přímý metodu, která restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="af73c-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="af73c-108">Přímé metody jsou vyvolány z cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="af73c-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="af73c-109">Vytvořte konzolovou aplikaci softwaru Node.js, která volá metodu přímé hello restartování v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="af73c-109">Create a Node.js console app that calls hello reboot direct method in hello simulated device app through your IoT hub.</span></span>

<span data-ttu-id="af73c-110">Na konci hello tohoto kurzu máte dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="af73c-110">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="af73c-111">**dmpatterns_getstarted_device.js**, který připojí tooyour IoT hub s dříve, vytvořenou identitou zařízení hello obdrží přímá metoda restartování, simuluje restartu fyzické a sestavy hello čas pro poslední restartování hello.</span><span class="sxs-lookup"><span data-stu-id="af73c-111">**dmpatterns_getstarted_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports hello time for hello last reboot.</span></span>

<span data-ttu-id="af73c-112">**dmpatterns_getstarted_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení hello, zobrazí hello odpovědi a zobrazí hello aktualizovat hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="af73c-112">**dmpatterns_getstarted_service.js**, which calls a direct method in hello simulated device app, displays hello response, and displays hello updated reported properties.</span></span>

<span data-ttu-id="af73c-113">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="af73c-113">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="af73c-114">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="af73c-114">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="af73c-115">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="af73c-115">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="af73c-116">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="af73c-116">An active Azure account.</span></span> <span data-ttu-id="af73c-117">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="af73c-117">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="af73c-118">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="af73c-118">Create a simulated device app</span></span>
<span data-ttu-id="af73c-119">V této části</span><span class="sxs-lookup"><span data-stu-id="af73c-119">In this section, you will</span></span>

* <span data-ttu-id="af73c-120">Vytvořte konzolovou aplikaci Node.js, která odpovídá tooa přímá metoda volá hello cloudu</span><span class="sxs-lookup"><span data-stu-id="af73c-120">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="af73c-121">Aktivační události restartu simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="af73c-121">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="af73c-122">Použití hello hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a při jejich poslední restartovat</span><span class="sxs-lookup"><span data-stu-id="af73c-122">Use hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted</span></span>

1. <span data-ttu-id="af73c-123">Vytvořte prázdnou složku s názvem **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="af73c-123">Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="af73c-124">V hello **manageddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="af73c-124">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="af73c-125">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="af73c-125">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="af73c-126">Na příkazovém řádku v hello **manageddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:</span><span class="sxs-lookup"><span data-stu-id="af73c-126">At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="af73c-127">Pomocí textového editoru, vytvořte **dmpatterns_getstarted_device.js** souboru v hello **manageddevice** složky.</span><span class="sxs-lookup"><span data-stu-id="af73c-127">Using a text editor, create a **dmpatterns_getstarted_device.js** file in hello **manageddevice** folder.</span></span>
4. <span data-ttu-id="af73c-128">Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_getstarted_device.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="af73c-128">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="af73c-129">Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance.</span><span class="sxs-lookup"><span data-stu-id="af73c-129">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  <span data-ttu-id="af73c-130">Nahraďte hello připojovacího řetězce připojovacím řetězcem zařízení.</span><span class="sxs-lookup"><span data-stu-id="af73c-130">Replace hello connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="af73c-131">Přidejte následující funkce tooimplement hello přímá metoda na zařízení hello hello</span><span class="sxs-lookup"><span data-stu-id="af73c-131">Add hello following function tooimplement hello direct method on hello device</span></span>
   
    ```
    var onReboot = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report hello reboot before hello physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. <span data-ttu-id="af73c-132">Otevřete Centrum IoT tooyour hello připojení a spuštění nástroje pro sledování přímá metoda hello:</span><span class="sxs-lookup"><span data-stu-id="af73c-132">Open hello connection tooyour IoT hub and start hello direct method listener:</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. <span data-ttu-id="af73c-133">Uložte a zavřete hello **dmpatterns_getstarted_device.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="af73c-133">Save and close hello **dmpatterns_getstarted_device.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="af73c-134">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="af73c-134">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="af73c-135">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="af73c-135">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="af73c-136">Aktivační události restartu vzdálené na zařízení hello pomocí přímá metoda</span><span class="sxs-lookup"><span data-stu-id="af73c-136">Trigger a remote reboot on hello device using a direct method</span></span>
<span data-ttu-id="af73c-137">V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje vzdálené restartování na zařízení pomocí přímého metody.</span><span class="sxs-lookup"><span data-stu-id="af73c-137">In this section, you create a Node.js console app that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="af73c-138">aplikace Hello používá zařízení twin dotazy toodiscover hello čas posledního restartování pro toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="af73c-138">hello app uses device twin queries toodiscover hello last reboot time for that device.</span></span>

1. <span data-ttu-id="af73c-139">Vytvořit prázdnou složku s názvem **triggerrebootondevice**.</span><span class="sxs-lookup"><span data-stu-id="af73c-139">Create an empty folder called **triggerrebootondevice**.</span></span>  <span data-ttu-id="af73c-140">V hello **triggerrebootondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="af73c-140">In hello **triggerrebootondevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="af73c-141">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="af73c-141">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="af73c-142">Na příkazovém řádku v hello **triggerrebootondevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíčku:</span><span class="sxs-lookup"><span data-stu-id="af73c-142">At your command prompt in hello **triggerrebootondevice** folder, run hello following command tooinstall hello **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="af73c-143">Pomocí textového editoru, vytvořte **dmpatterns_getstarted_service.js** souboru v hello **triggerrebootondevice** složky.</span><span class="sxs-lookup"><span data-stu-id="af73c-143">Using a text editor, create a **dmpatterns_getstarted_service.js** file in hello **triggerrebootondevice** folder.</span></span>
4. <span data-ttu-id="af73c-144">Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_getstarted_service.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="af73c-144">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="af73c-145">Přidejte následující deklarace proměnných hello a nahraďte zástupné hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="af73c-145">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. <span data-ttu-id="af73c-146">Přidejte následující funkce tooinvoke hello zařízení metoda tooreboot hello cílové zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="af73c-146">Add hello following function tooinvoke hello device method tooreboot hello target device:</span></span>
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked hello device tooreboot.");  
            }
        });
    };
    ```
7. <span data-ttu-id="af73c-147">Přidejte následující hello funkce tooquery hello zařízení a získat hello čas posledního restartování:</span><span class="sxs-lookup"><span data-stu-id="af73c-147">Add hello following function tooquery for hello device and get hello last reboot time:</span></span>
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device tooreport last reboot time.');
        });
    };
    ```
8. <span data-ttu-id="af73c-148">Přidejte následující funkce hello toocall kódu, které aktivují hello hello restartovat přímá metoda a dotaz na hello poslední restartovat čas:</span><span class="sxs-lookup"><span data-stu-id="af73c-148">Add hello following code toocall hello functions that trigger hello reboot direct method and query for hello last reboot time:</span></span>
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. <span data-ttu-id="af73c-149">Uložte a zavřete hello **dmpatterns_getstarted_service.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="af73c-149">Save and close hello **dmpatterns_getstarted_service.js** file.</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="af73c-150">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="af73c-150">Run hello apps</span></span>
<span data-ttu-id="af73c-151">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="af73c-151">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="af73c-152">Na příkazovém řádku hello v hello **manageddevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.</span><span class="sxs-lookup"><span data-stu-id="af73c-152">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="af73c-153">Na příkazovém řádku hello v hello **triggerrebootondevice** složky, spusťte následující příkaz tootrigger hello vzdálené hello restartujte počítač a dotaz hello zařízení twin toofind hello restartovat poslední čas.</span><span class="sxs-lookup"><span data-stu-id="af73c-153">At hello command prompt in hello **triggerrebootondevice** folder, run hello following command tootrigger hello remote reboot and query for hello device twin toofind hello last reboot time.</span></span>
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. <span data-ttu-id="af73c-154">Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="af73c-154">You see hello device response toohello direct method in hello console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
