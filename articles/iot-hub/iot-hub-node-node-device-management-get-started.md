---
title: "Začínáme se správou zařízení Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak používat k zahájení restartu zařízení vzdálenou správou zařízení IoT Hub. K implementaci aplikace simulovaného zařízení, která zahrnuje přímá metoda a aplikační služby, která volá metodu přímé používáte Azure IoT SDK pro Node.js."
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
ms.openlocfilehash: 332a3e62cb1ef75e2c6dd5562ee799465c401128
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-device-management-node"></a><span data-ttu-id="5a61f-104">Začínáme se správou zařízení (uzel)</span><span class="sxs-lookup"><span data-stu-id="5a61f-104">Get started with device management (Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="5a61f-105">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="5a61f-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="5a61f-106">Použití portálu Azure k vytvoření služby IoT Hub a vytvoření identity zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5a61f-106">Use the Azure portal to create an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="5a61f-107">Vytvoření aplikace simulovaného zařízení, která obsahuje přímý metodu, která restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a61f-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="5a61f-108">Přímé metody jsou vyvolány z cloudu.</span><span class="sxs-lookup"><span data-stu-id="5a61f-108">Direct methods are invoked from the cloud.</span></span>
* <span data-ttu-id="5a61f-109">Vytvořte konzolovou aplikaci softwaru Node.js, která volá metodu restartování přímé v aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5a61f-109">Create a Node.js console app that calls the reboot direct method in the simulated device app through your IoT hub.</span></span>

<span data-ttu-id="5a61f-110">Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="5a61f-110">At the end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="5a61f-111">**dmpatterns_getstarted_device.js**, který připojí ke službě IoT hub s identitou zařízení vytvořenou dříve, obdrží přímá metoda restartování, simuluje restartu fyzické a sestavy čas posledního restartování.</span><span class="sxs-lookup"><span data-stu-id="5a61f-111">**dmpatterns_getstarted_device.js**, which connects to your IoT hub with the device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports the time for the last reboot.</span></span>

<span data-ttu-id="5a61f-112">**dmpatterns_getstarted_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení, zobrazí odpověď a zobrazí aktualizovaná hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5a61f-112">**dmpatterns_getstarted_service.js**, which calls a direct method in the simulated device app, displays the response, and displays the updated reported properties.</span></span>

<span data-ttu-id="5a61f-113">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="5a61f-113">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="5a61f-114">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="5a61f-114">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="5a61f-115">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje postup instalace Node.js pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="5a61f-115">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="5a61f-116">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="5a61f-116">An active Azure account.</span></span> <span data-ttu-id="5a61f-117">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="5a61f-117">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="5a61f-118">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="5a61f-118">Create a simulated device app</span></span>
<span data-ttu-id="5a61f-119">V této části</span><span class="sxs-lookup"><span data-stu-id="5a61f-119">In this section, you will</span></span>

* <span data-ttu-id="5a61f-120">Vytvoříte konzolovou aplikaci Node.js, která bude reagovat na přímou metodu volanou cloudem.</span><span class="sxs-lookup"><span data-stu-id="5a61f-120">Create a Node.js console app that responds to a direct method called by the cloud</span></span>
* <span data-ttu-id="5a61f-121">Aktivační události restartu simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="5a61f-121">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="5a61f-122">Pomocí hlášen vlastnostech povolit dotazy na twin zařízení k identifikaci zařízení, a když trvají restartovat</span><span class="sxs-lookup"><span data-stu-id="5a61f-122">Use the reported properties to enable device twin queries to identify devices and when they last rebooted</span></span>

1. <span data-ttu-id="5a61f-123">Vytvořte prázdnou složku s názvem **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="5a61f-123">Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="5a61f-124">Ve složce **manageddevice** vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="5a61f-124">In the **manageddevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="5a61f-125">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5a61f-125">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="5a61f-126">Na příkazovém řádku v **manageddevice** složky, spusťte následující příkaz k instalaci **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíčku:</span><span class="sxs-lookup"><span data-stu-id="5a61f-126">At your command prompt in the **manageddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="5a61f-127">Pomocí textového editoru, vytvořte **dmpatterns_getstarted_device.js** v soubor **manageddevice** složky.</span><span class="sxs-lookup"><span data-stu-id="5a61f-127">Using a text editor, create a **dmpatterns_getstarted_device.js** file in the **manageddevice** folder.</span></span>
4. <span data-ttu-id="5a61f-128">Přidejte následující příkazy na začátku "vyžadovat" **dmpatterns_getstarted_device.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="5a61f-128">Add the following 'require' statements at the start of the **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="5a61f-129">Přidejte proměnnou **connectionString** a použijte ji k vytvoření instance **klienta**.</span><span class="sxs-lookup"><span data-stu-id="5a61f-129">Add a **connectionString** variable and use it to create a **Client** instance.</span></span>  <span data-ttu-id="5a61f-130">Nahraďte připojovacího řetězce připojovacím řetězcem zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a61f-130">Replace the connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="5a61f-131">Přidejte následující funkci implementovat metodu přímé na zařízení</span><span class="sxs-lookup"><span data-stu-id="5a61f-131">Add the following function to implement the direct method on the device</span></span>
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
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
7. <span data-ttu-id="5a61f-132">Otevření připojení do služby IoT hub a spuštění nástroje pro sledování přímá metoda:</span><span class="sxs-lookup"><span data-stu-id="5a61f-132">Open the connection to your IoT hub and start the direct method listener:</span></span>
   
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
8. <span data-ttu-id="5a61f-133">Uložte a zavřete **dmpatterns_getstarted_device.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="5a61f-133">Save and close the **dmpatterns_getstarted_device.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="5a61f-134">Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="5a61f-134">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="5a61f-135">V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="5a61f-135">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a><span data-ttu-id="5a61f-136">Aktivační události restartu vzdálené v zařízení s přímá metoda</span><span class="sxs-lookup"><span data-stu-id="5a61f-136">Trigger a remote reboot on the device using a direct method</span></span>
<span data-ttu-id="5a61f-137">V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje vzdálené restartování na zařízení pomocí přímého metody.</span><span class="sxs-lookup"><span data-stu-id="5a61f-137">In this section, you create a Node.js console app that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="5a61f-138">Aplikace používá dotazy twin zařízení Pokud chcete zjistit, čas posledního restartování pro toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a61f-138">The app uses device twin queries to discover the last reboot time for that device.</span></span>

1. <span data-ttu-id="5a61f-139">Vytvořit prázdnou složku s názvem **triggerrebootondevice**.</span><span class="sxs-lookup"><span data-stu-id="5a61f-139">Create an empty folder called **triggerrebootondevice**.</span></span>  <span data-ttu-id="5a61f-140">V **triggerrebootondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="5a61f-140">In the **triggerrebootondevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="5a61f-141">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5a61f-141">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="5a61f-142">Na příkazovém řádku v **triggerrebootondevice** složky, spusťte následující příkaz k instalaci **azure-iothub** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíčku:</span><span class="sxs-lookup"><span data-stu-id="5a61f-142">At your command prompt in the **triggerrebootondevice** folder, run the following command to install the **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="5a61f-143">Pomocí textového editoru, vytvořte **dmpatterns_getstarted_service.js** v soubor **triggerrebootondevice** složky.</span><span class="sxs-lookup"><span data-stu-id="5a61f-143">Using a text editor, create a **dmpatterns_getstarted_service.js** file in the **triggerrebootondevice** folder.</span></span>
4. <span data-ttu-id="5a61f-144">Přidejte následující příkazy na začátku "vyžadovat" **dmpatterns_getstarted_service.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="5a61f-144">Add the following 'require' statements at the start of the **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="5a61f-145">Přidejte následující deklarace proměnných a nahraďte zástupné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5a61f-145">Add the following variable declarations and replace the placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. <span data-ttu-id="5a61f-146">Přidejte následující funkci k vyvolání metody zařízení restartování cílového zařízení:</span><span class="sxs-lookup"><span data-stu-id="5a61f-146">Add the following function to invoke the device method to reboot the target device:</span></span>
   
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
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```
7. <span data-ttu-id="5a61f-147">Přidejte následující funkci k dotazování pro zařízení a získat čas posledního restartování:</span><span class="sxs-lookup"><span data-stu-id="5a61f-147">Add the following function to query for the device and get the last reboot time:</span></span>
   
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
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
8. <span data-ttu-id="5a61f-148">Přidejte následující kód k volání funkce, které aktivovat přímá metoda restartování a dotazů pro poslední restartování:</span><span class="sxs-lookup"><span data-stu-id="5a61f-148">Add the following code to call the functions that trigger the reboot direct method and query for the last reboot time:</span></span>
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. <span data-ttu-id="5a61f-149">Uložte a zavřete **dmpatterns_getstarted_service.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="5a61f-149">Save and close the **dmpatterns_getstarted_service.js** file.</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="5a61f-150">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="5a61f-150">Run the apps</span></span>
<span data-ttu-id="5a61f-151">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="5a61f-151">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="5a61f-152">Na příkazovém řádku v **manageddevice** složky, spusťte následující příkaz, aby začal přijímat přímá metoda restartování.</span><span class="sxs-lookup"><span data-stu-id="5a61f-152">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="5a61f-153">Na příkazovém řádku v **triggerrebootondevice** složky, spusťte následující příkaz k aktivační události restartovat vzdálený počítač a dotaz pro dvojče zařízení najít poslední čas restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="5a61f-153">At the command prompt in the **triggerrebootondevice** folder, run the following command to trigger the remote reboot and query for the device twin to find the last reboot time.</span></span>
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. <span data-ttu-id="5a61f-154">Zobrazí zařízení odpověď na přímá metoda v konzole.</span><span class="sxs-lookup"><span data-stu-id="5a61f-154">You see the device response to the direct method in the console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
