---
title: "Plánování úloh službou Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Popisuje, jak naplánovat úlohu služby Azure IoT Hub pro vyvolání přímé metody na několika zařízeních. Zařízení Azure IoT SDK pro Node.js použijete k implementaci aplikace simulovaného zařízení a sady SDK pro .NET k implementaci aplikace service chcete spustit úlohu služby Azure IoT."
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
ms.date: 07/10/2017
ms.author: juanpere
ms.openlocfilehash: a8f4f34aa99c4a9966957cac213ec9170de80a46
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a><span data-ttu-id="fbe0c-104">Úlohy plán a všesměrového vysílání (.NET/Node.js)</span><span class="sxs-lookup"><span data-stu-id="fbe0c-104">Schedule and broadcast jobs (.NET/Node.js)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="fbe0c-105">K plánování a sledování úloh, které aktualizují miliony zařízení pomocí služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-105">Use Azure IoT Hub to schedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="fbe0c-106">Pomocí úloh:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-106">Use jobs to:</span></span>

* <span data-ttu-id="fbe0c-107">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="fbe0c-107">Update desired properties</span></span>
* <span data-ttu-id="fbe0c-108">Aktualizace značky</span><span class="sxs-lookup"><span data-stu-id="fbe0c-108">Update tags</span></span>
* <span data-ttu-id="fbe0c-109">Vyvolání metody přímé</span><span class="sxs-lookup"><span data-stu-id="fbe0c-109">Invoke direct methods</span></span>

<span data-ttu-id="fbe0c-110">Úloha zabalí jednu z těchto akcí a sleduje provádění na sadu zařízení, která je definována dotazem twin zařízení.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-110">A job wraps one of these actions and tracks the execution against a set of devices that is defined by a device twin query.</span></span> <span data-ttu-id="fbe0c-111">Například můžete použít back-end aplikačním úlohu k vyvolání přímé metodu na 10 000 zařízení, která restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-111">For example, a back-end app can use a job to invoke a direct method on 10,000 devices that reboots the devices.</span></span> <span data-ttu-id="fbe0c-112">Zadejte sadu zařízení s dotazem twin zařízení a úlohu naplánovat na spuštění v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-112">You specify the set of devices with a device twin query and schedule the job to run at a future time.</span></span> <span data-ttu-id="fbe0c-113">Průběh úlohy sleduje jako každé zařízení obdrží a provést přímý metodu restartování.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-113">The job tracks progress as each of the devices receive and execute the reboot direct method.</span></span>

<span data-ttu-id="fbe0c-114">Další informace o každém z těchto funkcí najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-114">To learn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="fbe0c-115">Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení] [ lnk-get-started-twin] a [kurz: použití dvojici vlastností zařízení][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="fbe0c-115">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How to use device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="fbe0c-116">Přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody] [ lnk-dev-methods] a [kurz: použijte přímý metody][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="fbe0c-116">Direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: Use direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="fbe0c-117">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-117">This tutorial shows you how to:</span></span>

* <span data-ttu-id="fbe0c-118">Vytvoření aplikace zařízení, která implementuje přímé metodu s názvem **lockDoor** , lze volat back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-118">Create a device app that implements a direct method called **lockDoor** that can be called by the back-end app.</span></span> <span data-ttu-id="fbe0c-119">Aplikace zařízení také přijme požadovanou vlastnost změny z back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-119">The device app also receives desired property changes from the back-end app.</span></span>
* <span data-ttu-id="fbe0c-120">Vytvořit aplikaci back-end, která vytvoří úlohu k volání **lockDoor** přímá metoda na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-120">Create a back-end app that creates a job to call the **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="fbe0c-121">Jiná úloha odešle aktualizace požadované vlastností do více zařízení.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-121">Another job sends desired property updates to multiple devices.</span></span>

<span data-ttu-id="fbe0c-122">Na konci tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):</span><span class="sxs-lookup"><span data-stu-id="fbe0c-122">At the end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="fbe0c-123">**simDevice.js** která se připojuje ke službě IoT hub, implementuje **lockDoor** přímá metoda a zpracovává potřeby změny vlastností.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-123">**simDevice.js** that connects to your IoT hub, implements the **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="fbe0c-124">**ScheduleJob** používající úlohy k volání **lockDoor** přímá metoda a aktualizovat vlastnosti twin požadovaného zařízení na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-124">**ScheduleJob** that uses jobs to call the **lockDoor** direct method and update the device twin desired properties on multiple devices.</span></span>

<span data-ttu-id="fbe0c-125">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-125">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="fbe0c-126">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-126">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="fbe0c-127">Node.js verze 0.12.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-127">Node.js version 0.12.x or later.</span></span> <span data-ttu-id="fbe0c-128">Článek [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje postup instalace Node.js pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-128">The article [Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="fbe0c-129">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-129">An active Azure account.</span></span> <span data-ttu-id="fbe0c-130">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="fbe0c-130">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a><span data-ttu-id="fbe0c-131">Plán úlohy pro volání přímá metoda a odesílání twin aktualizace zařízení</span><span class="sxs-lookup"><span data-stu-id="fbe0c-131">Schedule jobs for calling a direct method and sending device twin updates</span></span>

<span data-ttu-id="fbe0c-132">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#) používající úlohy k volání **lockDoor** přímá metoda a odesílání aktualizací požadovanou vlastnost na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-132">In this section, you create a .NET console app (using C#) that uses jobs to call the **lockDoor** direct method and send desired property updates to multiple devices.</span></span>

1. <span data-ttu-id="fbe0c-133">V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-133">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="fbe0c-134">Název projektu **ScheduleJob**.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-134">Name the project **ScheduleJob**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

1. <span data-ttu-id="fbe0c-136">V Průzkumníku řešení klikněte pravým tlačítkem myši **ScheduleJob** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="fbe0c-136">In Solution Explorer, right-click the **ScheduleJob** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="fbe0c-137">V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-137">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="fbe0c-138">Tento krok stáhne, nainstaluje a přidá odkaz na [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-138">This step downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="fbe0c-140">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-140">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. <span data-ttu-id="fbe0c-141">Přidejte následující `using` příkaz, pokud ještě není přítomný v příkazech výchozí.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-141">Add the following `using` statement if not already present in the default statements.</span></span>

    ```csharp
    using System.Threading.Tasks;
    ```

1. <span data-ttu-id="fbe0c-142">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-142">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="fbe0c-143">Nahraďte zástupný symbol s připojovací řetězec služby IoT Hub pro rozbočovače, který jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-143">Replace the placeholder with the IoT Hub connection string for the hub that you created in the previous section.</span></span>

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. <span data-ttu-id="fbe0c-144">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-144">Add the following method to the **Program** class:</span></span>

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. <span data-ttu-id="fbe0c-145">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-145">Add the following method to the **Program** class:</span></span>

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("lockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            "deviceId='myDeviceId'",
            directMethod,
            DateTime.Now,
            10);

        Console.WriteLine("Started Method Job");
    }
    ```

1. <span data-ttu-id="fbe0c-146">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-146">Add the following method to the **Program** class:</span></span>

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        var twin = new Twin();
        twin.Properties.Desired["Building"] = "43";
        twin.Properties.Desired["Floor"] = "3";
        twin.ETag = "*";

        JobResponse result = await jobClient.ScheduleTwinUpdateAsync(jobId,
            "deviceId='myDeviceId'",
            twin,
            DateTime.Now,
            10);

        Console.WriteLine("Started Twin Update Job");
    }
    ```

1. <span data-ttu-id="fbe0c-147">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-147">Finally, add the following lines to the **Main** method:</span></span>

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

1. <span data-ttu-id="fbe0c-148">V Průzkumníku řešení otevřete **nastavit projekty po spuštění...**  a zajistěte, aby **akce** pro **ScheduleJob** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-148">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **ScheduleJob** project is **Start**.</span></span> <span data-ttu-id="fbe0c-149">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-149">Build the solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="fbe0c-150">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="fbe0c-150">Create a simulated device app</span></span>

<span data-ttu-id="fbe0c-151">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje na přímé metodu s názvem cloudem, který aktivuje restartu simulované zařízení a používá hlášené vlastnosti povolit zařízení twin dotazy k identifikaci zařízení, a při jejich poslední restartován.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-151">In this section, you create a Node.js console app that responds to a direct method called by the cloud, which triggers a simulated device reboot and uses the reported properties to enable device twin queries to identify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="fbe0c-152">Vytvořit novou prázdnou složku s názvem **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-152">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="fbe0c-153">V **simDevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-153">In the **simDevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="fbe0c-154">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-154">Accept all the defaults:</span></span>

    ```cmd/sh
    npm init
    ```

1. <span data-ttu-id="fbe0c-155">Na příkazovém řádku v **simDevice** složky, spusťte následující příkaz k instalaci **azure-iot-device** a **azure-iot zařízení mqtt** balíčky:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-155">At your command prompt in the **simDevice** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="fbe0c-156">Pomocí textového editoru, vytvořte novou **simDevice.js** v soubor **simDevice** složky.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-156">Using a text editor, create a new **simDevice.js** file in the **simDevice** folder.</span></span>

1. <span data-ttu-id="fbe0c-157">Přidejte následující příkazy na začátku "vyžadovat" **simDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="fbe0c-157">Add the following 'require' statements at the start of the **simDevice.js** file:</span></span>

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. <span data-ttu-id="fbe0c-158">Přidejte proměnnou **connectionString** a použijte ji k vytvoření instance **klienta**.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-158">Add a **connectionString** variable and use it to create a **Client** instance.</span></span> <span data-ttu-id="fbe0c-159">Ujistěte se, zda jste nahradili zástupné symboly hodnotami, které jsou vhodné pro vaši instalaci.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-159">Make sure to replace the placeholders with values appropriate to your setup.</span></span>

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="fbe0c-160">Přidejte následující funkci pro zpracování **lockDoor** metoda.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-160">Add the following function to handle the **lockDoor** method.</span></span>

    ```nodejs
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

1. <span data-ttu-id="fbe0c-161">Přidejte následující kód pro obslužnou rutinu pro registraci **lockDoor** metoda.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-161">Add the following code to register the handler for the **lockDoor** method.</span></span>

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. <span data-ttu-id="fbe0c-162">Uložte a zavřete **simDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-162">Save and close the **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="fbe0c-163">Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-163">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="fbe0c-164">V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-164">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="fbe0c-165">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="fbe0c-165">Run the apps</span></span>

<span data-ttu-id="fbe0c-166">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-166">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="fbe0c-167">Na příkazovém řádku v **simDevice** složky, spusťte následující příkaz, aby začal přijímat přímá metoda restartování.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-167">At the command prompt in the **simDevice** folder, run the following command to begin listening for the reboot direct method.</span></span>

    ```cmd/sh
    node simDevice.js
    ```

1. <span data-ttu-id="fbe0c-168">Spusťte konzolovou aplikaci C# **ScheduleJob** kliknutím pravým tlačítkem na **ScheduleJob** projekt, pak výběrem **ladění** a **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-168">Run the C# console app **ScheduleJob** by right-clicking on the **ScheduleJob** project, then selecting **Debug** and **Start new instance**.</span></span>

1. <span data-ttu-id="fbe0c-169">Zobrazí se výstup ze zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-169">You see the output from both device and back-end apps.</span></span>

    ![Spuštění aplikace při plánování úloh][img-schedulejobs]

## <a name="next-steps"></a><span data-ttu-id="fbe0c-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fbe0c-171">Next steps</span></span>

<span data-ttu-id="fbe0c-172">V tomto kurzu jste použili úlohu při plánování přímá metoda zařízení a aktualizaci vlastností dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="fbe0c-172">In this tutorial, you used a job to schedule a direct method to a device and the update of the device twin's properties.</span></span>

<span data-ttu-id="fbe0c-173">Chcete-li pokračovat, Začínáme se službou IoT Hub a vzory správy zařízení, jako je vzdálené přes aktualizaci firmwaru letecké, přečtěte si [kurz: jak to provést aktualizaci firmwaru][lnk-fwupdate].</span><span class="sxs-lookup"><span data-stu-id="fbe0c-173">To continue getting started with IoT Hub and device management patterns such as remote over the air firmware update, read [Tutorial: How to do a firmware update][lnk-fwupdate].</span></span>

<span data-ttu-id="fbe0c-174">Chcete-li pokračovat, Začínáme se službou IoT Hub, najdete v části [Začínáme se službou IoT Edge][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="fbe0c-174">To continue getting started with IoT Hub, see [Getting started with IoT Edge][lnk-iot-edge].</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-node-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
