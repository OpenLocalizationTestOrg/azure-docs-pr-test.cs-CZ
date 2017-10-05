---
title: "Začínáme se správou zařízení Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Jak používat k zahájení restartu zařízení vzdálenou správou zařízení Azure IoT Hub. Implementace aplikaci ze simulovaného zařízení, která zahrnuje přímá metoda a sady SDK pro .NET k implementaci aplikační služby, která volá metodu přímé služby Azure IoT pomocí zařízení Azure IoT SDK pro Node.js."
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
ms.date: 11/17/2016
ms.author: juanpere
ms.openlocfilehash: d97fc5493570985f94c23032c870628d6a089dcd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-device-management-netnode"></a><span data-ttu-id="febdc-104">Začínáme se správou zařízení (.NET nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="febdc-104">Get started with device management (.NET/Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="febdc-105">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="febdc-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="febdc-106">Použití portálu Azure k vytvoření služby IoT Hub a vytvoření identity zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="febdc-106">Use the Azure portal to create an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="febdc-107">Vytvoření aplikace simulovaného zařízení, která obsahuje přímý metodu, která restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="febdc-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="febdc-108">Přímé metody jsou vyvolány z cloudu.</span><span class="sxs-lookup"><span data-stu-id="febdc-108">Direct methods are invoked from the cloud.</span></span>
* <span data-ttu-id="febdc-109">Vytvoření konzolové aplikace .NET, která volá metodu restartování přímé v aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="febdc-109">Create a .NET console app that calls the reboot direct method in the simulated device app through your IoT hub.</span></span>

<span data-ttu-id="febdc-110">Na konci tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):</span><span class="sxs-lookup"><span data-stu-id="febdc-110">At the end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="febdc-111">**dmpatterns_getstarted_device.js**, který připojí ke službě IoT hub s identitou zařízení vytvořenou dříve, obdrží přímá metoda restartování, simuluje restartu fyzické a sestavy čas posledního restartování.</span><span class="sxs-lookup"><span data-stu-id="febdc-111">**dmpatterns_getstarted_device.js**, which connects to your IoT hub with the device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports the time for the last reboot.</span></span>

<span data-ttu-id="febdc-112">**TriggerReboot**, která volá metodu přímé v aplikaci simulovaného zařízení, zobrazí odpověď a zobrazí aktualizovaná hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="febdc-112">**TriggerReboot**, which calls a direct method in the simulated device app, displays the response, and displays the updated reported properties.</span></span>

<span data-ttu-id="febdc-113">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="febdc-113">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="febdc-114">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="febdc-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="febdc-115">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="febdc-115">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="febdc-116">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje postup instalace Node.js pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="febdc-116">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="febdc-117">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="febdc-117">An active Azure account.</span></span> <span data-ttu-id="febdc-118">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="febdc-118">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a><span data-ttu-id="febdc-119">Aktivační události restartu vzdálené v zařízení s přímá metoda</span><span class="sxs-lookup"><span data-stu-id="febdc-119">Trigger a remote reboot on the device using a direct method</span></span>
<span data-ttu-id="febdc-120">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#) iniciované vzdálené restartování na zařízení pomocí přímého metody.</span><span class="sxs-lookup"><span data-stu-id="febdc-120">In this section, you create a .NET console app (using C#) that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="febdc-121">Aplikace používá dotazy twin zařízení Pokud chcete zjistit, čas posledního restartování pro toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="febdc-121">The app uses device twin queries to discover the last reboot time for that device.</span></span>

1. <span data-ttu-id="febdc-122">V sadě Visual Studio přidejte k novému řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="febdc-122">In Visual Studio, add a Visual C# Windows Classic Desktop project to a new solution by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="febdc-123">Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="febdc-123">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="febdc-124">Název projektu **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="febdc-124">Name the project **TriggerReboot**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

2. <span data-ttu-id="febdc-126">V Průzkumníku řešení klikněte pravým tlačítkem myši **TriggerReboot** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="febdc-126">In Solution Explorer, right-click the **TriggerReboot** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="febdc-127">V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání.</span><span class="sxs-lookup"><span data-stu-id="febdc-127">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="febdc-128">Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="febdc-128">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Okno Správce balíčků NuGet][img-servicenuget]
4. <span data-ttu-id="febdc-130">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="febdc-130">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. <span data-ttu-id="febdc-131">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="febdc-131">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="febdc-132">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro rozbočovače, který jste vytvořili v předchozí části a cílové zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="febdc-132">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section and the target device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. <span data-ttu-id="febdc-133">Přidejte následující metodu do **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="febdc-133">Add the following method to the **Program** class.</span></span>  <span data-ttu-id="febdc-134">Tento kód získá dvojče zařízení pro restartování zařízení a výstupy hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="febdc-134">This code gets the device twin for the rebooting device and outputs the reported properties.</span></span>
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. <span data-ttu-id="febdc-135">Přidejte následující metodu do **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="febdc-135">Add the following method to the **Program** class.</span></span>  <span data-ttu-id="febdc-136">Tento kód zahájí restartování v zařízení s přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="febdc-136">This code initiates the reboot on the device using a direct method.</span></span>

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. <span data-ttu-id="febdc-137">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="febdc-137">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
8. <span data-ttu-id="febdc-138">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="febdc-138">Build the solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="febdc-139">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="febdc-139">Create a simulated device app</span></span>
<span data-ttu-id="febdc-140">V této části</span><span class="sxs-lookup"><span data-stu-id="febdc-140">In this section, you will</span></span>

* <span data-ttu-id="febdc-141">Vytvoříte konzolovou aplikaci Node.js, která bude reagovat na přímou metodu volanou cloudem.</span><span class="sxs-lookup"><span data-stu-id="febdc-141">Create a Node.js console app that responds to a direct method called by the cloud</span></span>
* <span data-ttu-id="febdc-142">Aktivační události restartu simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="febdc-142">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="febdc-143">Pomocí hlášen vlastnostech povolit dotazy na twin zařízení k identifikaci zařízení, a když trvají restartovat</span><span class="sxs-lookup"><span data-stu-id="febdc-143">Use the reported properties to enable device twin queries to identify devices and when they last rebooted</span></span>

1. <span data-ttu-id="febdc-144">Vytvořit novou prázdnou složku s názvem **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="febdc-144">Create a new empty folder called **manageddevice**.</span></span>  <span data-ttu-id="febdc-145">Ve složce **manageddevice** vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="febdc-145">In the **manageddevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="febdc-146">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="febdc-146">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="febdc-147">Na příkazovém řádku v **manageddevice** složky, spusťte následující příkaz k instalaci **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíčku:</span><span class="sxs-lookup"><span data-stu-id="febdc-147">At your command prompt in the **manageddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="febdc-148">Pomocí textového editoru, vytvořte novou **dmpatterns_getstarted_device.js** v soubor **manageddevice** složky.</span><span class="sxs-lookup"><span data-stu-id="febdc-148">Using a text editor, create a new **dmpatterns_getstarted_device.js** file in the **manageddevice** folder.</span></span>
4. <span data-ttu-id="febdc-149">Přidejte následující příkazy na začátku "vyžadovat" **dmpatterns_getstarted_device.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="febdc-149">Add the following 'require' statements at the start of the **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="febdc-150">Přidejte proměnnou **connectionString** a použijte ji k vytvoření instance **klienta**.</span><span class="sxs-lookup"><span data-stu-id="febdc-150">Add a **connectionString** variable and use it to create a **Client** instance.</span></span>  <span data-ttu-id="febdc-151">Nahraďte připojovacího řetězce připojovacím řetězcem zařízení.</span><span class="sxs-lookup"><span data-stu-id="febdc-151">Replace the connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="febdc-152">Přidejte následující funkci implementovat metodu přímé na zařízení</span><span class="sxs-lookup"><span data-stu-id="febdc-152">Add the following function to implement the direct method on the device</span></span>
   
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
7. <span data-ttu-id="febdc-153">Přidejte následující kód k otevření připojení do služby IoT hub a spustit přímá metoda naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="febdc-153">Add the following code to open the connection to your IoT hub and start the direct method listener:</span></span>
   
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
8. <span data-ttu-id="febdc-154">Uložte a zavřete **dmpatterns_getstarted_device.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="febdc-154">Save and close the **dmpatterns_getstarted_device.js** file.</span></span>
   
> [!NOTE]
> <span data-ttu-id="febdc-155">Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="febdc-155">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="febdc-156">V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="febdc-156">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>


## <a name="run-the-apps"></a><span data-ttu-id="febdc-157">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="febdc-157">Run the apps</span></span>
<span data-ttu-id="febdc-158">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="febdc-158">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="febdc-159">Na příkazovém řádku v **manageddevice** složky, spusťte následující příkaz, aby začal přijímat přímá metoda restartování.</span><span class="sxs-lookup"><span data-stu-id="febdc-159">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="febdc-160">Spusťte konzolovou aplikaci C# **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="febdc-160">Run the C# console app **TriggerReboot**.</span></span> <span data-ttu-id="febdc-161">Klikněte pravým tlačítkem myši **TriggerReboot** projekt, vyberte **ladění**a potom vyberte **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="febdc-161">Right-click the **TriggerReboot** project, select **Debug**, and then select **Start new instance**.</span></span>

3. <span data-ttu-id="febdc-162">Zobrazí zařízení odpověď na přímá metoda v konzole.</span><span class="sxs-lookup"><span data-stu-id="febdc-162">You see the device response to the direct method in the console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/