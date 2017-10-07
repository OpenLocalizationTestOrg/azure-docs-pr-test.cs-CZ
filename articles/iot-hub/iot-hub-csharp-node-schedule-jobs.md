---
title: "úlohy aaaSchedule službou Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Jak tooschedule Azure IoT Hub úlohy tooinvoke přímá metoda na několika zařízeních. Použití zařízení Azure IoT hello SDK pro Node.js tooimplement hello simulované zařízení aplikace a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement úlohu služby toorun aplikace hello."
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
ms.openlocfilehash: f6148b67129dde4580bfe9ccceafd6400fbc5976
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a><span data-ttu-id="ba99c-104">Úlohy plán a všesměrového vysílání (.NET/Node.js)</span><span class="sxs-lookup"><span data-stu-id="ba99c-104">Schedule and broadcast jobs (.NET/Node.js)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="ba99c-105">Pomocí Azure IoT Hub tooschedule a sledování úloh, které aktualizují miliony zařízení.</span><span class="sxs-lookup"><span data-stu-id="ba99c-105">Use Azure IoT Hub tooschedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="ba99c-106">Pomocí úloh:</span><span class="sxs-lookup"><span data-stu-id="ba99c-106">Use jobs to:</span></span>

* <span data-ttu-id="ba99c-107">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="ba99c-107">Update desired properties</span></span>
* <span data-ttu-id="ba99c-108">Aktualizace značky</span><span class="sxs-lookup"><span data-stu-id="ba99c-108">Update tags</span></span>
* <span data-ttu-id="ba99c-109">Vyvolání metody přímé</span><span class="sxs-lookup"><span data-stu-id="ba99c-109">Invoke direct methods</span></span>

<span data-ttu-id="ba99c-110">Úlohu zabalí jednu z těchto akcí a sleduje hello vůči sadu zařízení, která je definována dotazem twin zařízení.</span><span class="sxs-lookup"><span data-stu-id="ba99c-110">A job wraps one of these actions and tracks hello execution against a set of devices that is defined by a device twin query.</span></span> <span data-ttu-id="ba99c-111">Back-end aplikačním můžete například použít úlohy tooinvoke přímá metoda na 10 000 zařízení, která restartuje hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="ba99c-111">For example, a back-end app can use a job tooinvoke a direct method on 10,000 devices that reboots hello devices.</span></span> <span data-ttu-id="ba99c-112">Zadejte hello sadu zařízení s dotazem twin zařízení a naplánovat úlohu toorun hello na datum v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="ba99c-112">You specify hello set of devices with a device twin query and schedule hello job toorun at a future time.</span></span> <span data-ttu-id="ba99c-113">sleduje průběh úlohy Hello jako každý hello zařízení obdrží a provést přímý metodu hello restartování.</span><span class="sxs-lookup"><span data-stu-id="ba99c-113">hello job tracks progress as each of hello devices receive and execute hello reboot direct method.</span></span>

<span data-ttu-id="ba99c-114">v tématu toolearn Další informace o každé z těchto funkcí:</span><span class="sxs-lookup"><span data-stu-id="ba99c-114">toolearn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="ba99c-115">Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení] [ lnk-get-started-twin] a [kurz: jak dvojče zařízení toouse vlastnosti][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="ba99c-115">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How toouse device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="ba99c-116">Přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody] [ lnk-dev-methods] a [kurz: použijte přímý metody][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="ba99c-116">Direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: Use direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="ba99c-117">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="ba99c-117">This tutorial shows you how to:</span></span>

* <span data-ttu-id="ba99c-118">Vytvoření aplikace zařízení, která implementuje přímé metodu s názvem **lockDoor** , lze volat back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-118">Create a device app that implements a direct method called **lockDoor** that can be called by hello back-end app.</span></span> <span data-ttu-id="ba99c-119">aplikace Hello zařízení také obdrží požadovanou vlastnost změny z back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-119">hello device app also receives desired property changes from hello back-end app.</span></span>
* <span data-ttu-id="ba99c-120">Vytvořit aplikaci back-end, která vytvoří úlohy toocall hello **lockDoor** přímá metoda na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="ba99c-120">Create a back-end app that creates a job toocall hello **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="ba99c-121">Jiná úloha odešle požadovanou vlastnost aktualizací toomultiple zařízení.</span><span class="sxs-lookup"><span data-stu-id="ba99c-121">Another job sends desired property updates toomultiple devices.</span></span>

<span data-ttu-id="ba99c-122">Na konci hello tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):</span><span class="sxs-lookup"><span data-stu-id="ba99c-122">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="ba99c-123">**simDevice.js** , připojí tooyour IoT hub, implementuje hello **lockDoor** přímá metoda a zpracovává potřeby změny vlastností.</span><span class="sxs-lookup"><span data-stu-id="ba99c-123">**simDevice.js** that connects tooyour IoT hub, implements hello **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="ba99c-124">**ScheduleJob** hello toocall úlohy, který používá **lockDoor** přímá metoda a aktualizace hello dvojče zařízení požadovaných vlastností na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="ba99c-124">**ScheduleJob** that uses jobs toocall hello **lockDoor** direct method and update hello device twin desired properties on multiple devices.</span></span>

<span data-ttu-id="ba99c-125">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="ba99c-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="ba99c-126">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ba99c-126">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="ba99c-127">Node.js verze 0.12.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ba99c-127">Node.js version 0.12.x or later.</span></span> <span data-ttu-id="ba99c-128">článek Hello [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="ba99c-128">hello article [Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="ba99c-129">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ba99c-129">An active Azure account.</span></span> <span data-ttu-id="ba99c-130">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="ba99c-130">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a><span data-ttu-id="ba99c-131">Plán úlohy pro volání přímá metoda a odesílání twin aktualizace zařízení</span><span class="sxs-lookup"><span data-stu-id="ba99c-131">Schedule jobs for calling a direct method and sending device twin updates</span></span>

<span data-ttu-id="ba99c-132">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#) používající úlohy toocall hello **lockDoor** přímá metoda a odesílat požadovanou vlastnost aktualizace toomultiple zařízení.</span><span class="sxs-lookup"><span data-stu-id="ba99c-132">In this section, you create a .NET console app (using C#) that uses jobs toocall hello **lockDoor** direct method and send desired property updates toomultiple devices.</span></span>

1. <span data-ttu-id="ba99c-133">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="ba99c-133">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="ba99c-134">Název projektu hello **ScheduleJob**.</span><span class="sxs-lookup"><span data-stu-id="ba99c-134">Name hello project **ScheduleJob**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

1. <span data-ttu-id="ba99c-136">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ScheduleJob** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="ba99c-136">In Solution Explorer, right-click hello **ScheduleJob** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="ba99c-137">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-137">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="ba99c-138">Tento krok stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="ba99c-138">This step downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="ba99c-140">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="ba99c-140">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. <span data-ttu-id="ba99c-141">Přidejte následující hello `using` příkaz, pokud ještě není přítomný v příkazech výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-141">Add hello following `using` statement if not already present in hello default statements.</span></span>

    ```csharp
    using System.Threading.Tasks;
    ```

1. <span data-ttu-id="ba99c-142">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="ba99c-142">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="ba99c-143">Nahraďte zástupný symbol hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-143">Replace hello placeholder with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. <span data-ttu-id="ba99c-144">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="ba99c-144">Add hello following method toohello **Program** class:</span></span>

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

1. <span data-ttu-id="ba99c-145">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="ba99c-145">Add hello following method toohello **Program** class:</span></span>

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

1. <span data-ttu-id="ba99c-146">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="ba99c-146">Add hello following method toohello **Program** class:</span></span>

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

1. <span data-ttu-id="ba99c-147">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="ba99c-147">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER toorun hello next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER tooexit.");
    Console.ReadLine();
    ```

1. <span data-ttu-id="ba99c-148">V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **ScheduleJob** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="ba99c-148">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **ScheduleJob** project is **Start**.</span></span> <span data-ttu-id="ba99c-149">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-149">Build hello solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="ba99c-150">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="ba99c-150">Create a simulated device app</span></span>

<span data-ttu-id="ba99c-151">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje tooa přímá metoda volá hello cloudu, což aktivuje restartu simulované zařízení a hello používá hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a při jejich poslední restartován.</span><span class="sxs-lookup"><span data-stu-id="ba99c-151">In this section, you create a Node.js console app that responds tooa direct method called by hello cloud, which triggers a simulated device reboot and uses hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="ba99c-152">Vytvořit novou prázdnou složku s názvem **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="ba99c-152">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="ba99c-153">V hello **simDevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-153">In hello **simDevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="ba99c-154">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="ba99c-154">Accept all hello defaults:</span></span>

    ```cmd/sh
    npm init
    ```

1. <span data-ttu-id="ba99c-155">Na příkazovém řádku v hello **simDevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** a **azure-iot zařízení mqtt** balíčky:</span><span class="sxs-lookup"><span data-stu-id="ba99c-155">At your command prompt in hello **simDevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="ba99c-156">Pomocí textového editoru, vytvořte novou **simDevice.js** souboru v hello **simDevice** složky.</span><span class="sxs-lookup"><span data-stu-id="ba99c-156">Using a text editor, create a new **simDevice.js** file in hello **simDevice** folder.</span></span>

1. <span data-ttu-id="ba99c-157">Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **simDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="ba99c-157">Add hello following 'require' statements at hello start of hello **simDevice.js** file:</span></span>

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. <span data-ttu-id="ba99c-158">Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance.</span><span class="sxs-lookup"><span data-stu-id="ba99c-158">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="ba99c-159">Ujistěte se, že tooreplace hello zástupné hodnoty odpovídající tooyour spuštěním instalačního programu.</span><span class="sxs-lookup"><span data-stu-id="ba99c-159">Make sure tooreplace hello placeholders with values appropriate tooyour setup.</span></span>

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="ba99c-160">Přidejte následující funkce toohandle hello hello **lockDoor** metoda.</span><span class="sxs-lookup"><span data-stu-id="ba99c-160">Add hello following function toohandle hello **lockDoor** method.</span></span>

    ```nodejs
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

1. <span data-ttu-id="ba99c-161">Přidejte následující kód tooregister hello obslužné rutiny pro hello hello **lockDoor** metoda.</span><span class="sxs-lookup"><span data-stu-id="ba99c-161">Add hello following code tooregister hello handler for hello **lockDoor** method.</span></span>

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. <span data-ttu-id="ba99c-162">Uložte a zavřete hello **simDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="ba99c-162">Save and close hello **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="ba99c-163">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="ba99c-163">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="ba99c-164">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="ba99c-164">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="ba99c-165">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="ba99c-165">Run hello apps</span></span>

<span data-ttu-id="ba99c-166">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba99c-166">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="ba99c-167">Na příkazovém řádku hello v hello **simDevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-167">At hello command prompt in hello **simDevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>

    ```cmd/sh
    node simDevice.js
    ```

1. <span data-ttu-id="ba99c-168">Spuštění hello C# konzolovou aplikaci **ScheduleJob** kliknutím pravým tlačítkem na hello **ScheduleJob** projekt, pak výběrem **ladění** a **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="ba99c-168">Run hello C# console app **ScheduleJob** by right-clicking on hello **ScheduleJob** project, then selecting **Debug** and **Start new instance**.</span></span>

1. <span data-ttu-id="ba99c-169">Zobrazí výstup hello ze zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba99c-169">You see hello output from both device and back-end apps.</span></span>

    ![Spuštění aplikace hello tooschedule úloh][img-schedulejobs]

## <a name="next-steps"></a><span data-ttu-id="ba99c-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba99c-171">Next steps</span></span>

<span data-ttu-id="ba99c-172">V tomto kurzu jste použili úlohy tooschedule zařízení tooa přímá metoda a hello aktualizace vlastností dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="ba99c-172">In this tutorial, you used a job tooschedule a direct method tooa device and hello update of hello device twin's properties.</span></span>

<span data-ttu-id="ba99c-173">Začínáme se službou IoT Hub a zařízení vzory správy, jako je vzdálené přes aktualizaci firmwaru letecké hello, přečtěte si toocontinue [kurz: jak toodo firmwarem aktualizovat][lnk-fwupdate].</span><span class="sxs-lookup"><span data-stu-id="ba99c-173">toocontinue getting started with IoT Hub and device management patterns such as remote over hello air firmware update, read [Tutorial: How toodo a firmware update][lnk-fwupdate].</span></span>

<span data-ttu-id="ba99c-174">najdete v části Začínáme se službou IoT Hub, toocontinue [Začínáme se službou IoT Edge][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="ba99c-174">toocontinue getting started with IoT Hub, see [Getting started with IoT Edge][lnk-iot-edge].</span></span>

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
