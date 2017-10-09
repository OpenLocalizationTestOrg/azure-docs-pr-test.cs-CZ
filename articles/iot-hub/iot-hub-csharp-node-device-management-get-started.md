---
title: "aaaGet spuštění se správou zařízení Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Jak tooinitiate správy zařízení Azure IoT Hub toouse vzdáleném zařízení restartovat. Použití zařízení Azure IoT hello SDK pro Node.js tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která volá metodu přímé hello."
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
ms.openlocfilehash: ea6d50dfac1c222d7836e3bf5503c6c9b8c89dfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-netnode"></a><span data-ttu-id="12117-104">Začínáme se správou zařízení (.NET nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="12117-104">Get started with device management (.NET/Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="12117-105">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="12117-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="12117-106">Použijte hello portálu toocreate Azure IoT Hub a vytvoření identity zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="12117-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="12117-107">Vytvoření aplikace simulovaného zařízení, která obsahuje přímý metodu, která restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="12117-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="12117-108">Přímé metody jsou vyvolány z cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="12117-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="12117-109">Vytvoření konzolové aplikace .NET, která volá metodu přímé hello restartování v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="12117-109">Create a .NET console app that calls hello reboot direct method in hello simulated device app through your IoT hub.</span></span>

<span data-ttu-id="12117-110">Na konci hello tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):</span><span class="sxs-lookup"><span data-stu-id="12117-110">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="12117-111">**dmpatterns_getstarted_device.js**, který připojí tooyour IoT hub s dříve, vytvořenou identitou zařízení hello obdrží přímá metoda restartování, simuluje restartu fyzické a sestavy hello čas pro poslední restartování hello.</span><span class="sxs-lookup"><span data-stu-id="12117-111">**dmpatterns_getstarted_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports hello time for hello last reboot.</span></span>

<span data-ttu-id="12117-112">**TriggerReboot**, která volá metodu přímé v aplikaci simulovaného zařízení hello, zobrazí hello odpovědi a zobrazí hello aktualizovat hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12117-112">**TriggerReboot**, which calls a direct method in hello simulated device app, displays hello response, and displays hello updated reported properties.</span></span>

<span data-ttu-id="12117-113">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="12117-113">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="12117-114">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="12117-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="12117-115">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="12117-115">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="12117-116">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="12117-116">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="12117-117">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="12117-117">An active Azure account.</span></span> <span data-ttu-id="12117-118">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="12117-118">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="12117-119">Aktivační události restartu vzdálené na zařízení hello pomocí přímá metoda</span><span class="sxs-lookup"><span data-stu-id="12117-119">Trigger a remote reboot on hello device using a direct method</span></span>
<span data-ttu-id="12117-120">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#) iniciované vzdálené restartování na zařízení pomocí přímého metody.</span><span class="sxs-lookup"><span data-stu-id="12117-120">In this section, you create a .NET console app (using C#) that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="12117-121">aplikace Hello používá zařízení twin dotazy toodiscover hello čas posledního restartování pro toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="12117-121">hello app uses device twin queries toodiscover hello last reboot time for that device.</span></span>

1. <span data-ttu-id="12117-122">V sadě Visual Studio, přidejte nové řešení Visual C# Windows klasický desktopový projekt tooa pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="12117-122">In Visual Studio, add a Visual C# Windows Classic Desktop project tooa new solution by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="12117-123">Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="12117-123">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="12117-124">Název projektu hello **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="12117-124">Name hello project **TriggerReboot**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

2. <span data-ttu-id="12117-126">V Průzkumníku řešení klikněte pravým tlačítkem na hello **TriggerReboot** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="12117-126">In Solution Explorer, right-click hello **TriggerReboot** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="12117-127">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="12117-127">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="12117-128">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="12117-128">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Okno Správce balíčků NuGet][img-servicenuget]
4. <span data-ttu-id="12117-130">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="12117-130">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. <span data-ttu-id="12117-131">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="12117-131">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="12117-132">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello a hello cílové zařízení.</span><span class="sxs-lookup"><span data-stu-id="12117-132">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section and hello target device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. <span data-ttu-id="12117-133">Přidejte následující metodu toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="12117-133">Add hello following method toohello **Program** class.</span></span>  <span data-ttu-id="12117-134">Tento kód získá hello dvojče zařízení pro hello restartování zařízení a výstupy hello hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12117-134">This code gets hello device twin for hello rebooting device and outputs hello reported properties.</span></span>
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. <span data-ttu-id="12117-135">Přidejte následující metodu toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="12117-135">Add hello following method toohello **Program** class.</span></span>  <span data-ttu-id="12117-136">Tento kód zahájí hello restartování na zařízení hello přímé metodou.</span><span class="sxs-lookup"><span data-stu-id="12117-136">This code initiates hello reboot on hello device using a direct method.</span></span>

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. <span data-ttu-id="12117-137">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="12117-137">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
8. <span data-ttu-id="12117-138">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="12117-138">Build hello solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="12117-139">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="12117-139">Create a simulated device app</span></span>
<span data-ttu-id="12117-140">V této části</span><span class="sxs-lookup"><span data-stu-id="12117-140">In this section, you will</span></span>

* <span data-ttu-id="12117-141">Vytvořte konzolovou aplikaci Node.js, která odpovídá tooa přímá metoda volá hello cloudu</span><span class="sxs-lookup"><span data-stu-id="12117-141">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="12117-142">Aktivační události restartu simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="12117-142">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="12117-143">Použití hello hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a při jejich poslední restartovat</span><span class="sxs-lookup"><span data-stu-id="12117-143">Use hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted</span></span>

1. <span data-ttu-id="12117-144">Vytvořit novou prázdnou složku s názvem **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="12117-144">Create a new empty folder called **manageddevice**.</span></span>  <span data-ttu-id="12117-145">V hello **manageddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="12117-145">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="12117-146">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="12117-146">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="12117-147">Na příkazovém řádku v hello **manageddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:</span><span class="sxs-lookup"><span data-stu-id="12117-147">At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="12117-148">Pomocí textového editoru, vytvořte novou **dmpatterns_getstarted_device.js** souboru v hello **manageddevice** složky.</span><span class="sxs-lookup"><span data-stu-id="12117-148">Using a text editor, create a new **dmpatterns_getstarted_device.js** file in hello **manageddevice** folder.</span></span>
4. <span data-ttu-id="12117-149">Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_getstarted_device.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="12117-149">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="12117-150">Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance.</span><span class="sxs-lookup"><span data-stu-id="12117-150">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  <span data-ttu-id="12117-151">Nahraďte hello připojovacího řetězce připojovacím řetězcem zařízení.</span><span class="sxs-lookup"><span data-stu-id="12117-151">Replace hello connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="12117-152">Přidejte následující funkce tooimplement hello přímá metoda na zařízení hello hello</span><span class="sxs-lookup"><span data-stu-id="12117-152">Add hello following function tooimplement hello direct method on hello device</span></span>
   
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
7. <span data-ttu-id="12117-153">Přidejte následující hello kód tooopen hello připojení tooyour IoT hub a spustit hello přímá metoda naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="12117-153">Add hello following code tooopen hello connection tooyour IoT hub and start hello direct method listener:</span></span>
   
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
8. <span data-ttu-id="12117-154">Uložte a zavřete hello **dmpatterns_getstarted_device.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="12117-154">Save and close hello **dmpatterns_getstarted_device.js** file.</span></span>
   
> [!NOTE]
> <span data-ttu-id="12117-155">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="12117-155">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="12117-156">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="12117-156">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>


## <a name="run-hello-apps"></a><span data-ttu-id="12117-157">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="12117-157">Run hello apps</span></span>
<span data-ttu-id="12117-158">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="12117-158">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="12117-159">Na příkazovém řádku hello v hello **manageddevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.</span><span class="sxs-lookup"><span data-stu-id="12117-159">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="12117-160">Spuštění hello C# konzolovou aplikaci **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="12117-160">Run hello C# console app **TriggerReboot**.</span></span> <span data-ttu-id="12117-161">Klikněte pravým tlačítkem na hello **TriggerReboot** projekt, vyberte **ladění**a potom vyberte **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="12117-161">Right-click hello **TriggerReboot** project, select **Debug**, and then select **Start new instance**.</span></span>

3. <span data-ttu-id="12117-162">Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="12117-162">You see hello device response toohello direct method in hello console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/