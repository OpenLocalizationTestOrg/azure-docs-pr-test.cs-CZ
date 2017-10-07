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
# <a name="schedule-and-broadcast-jobs-netnodejs"></a>Úlohy plán a všesměrového vysílání (.NET/Node.js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Pomocí Azure IoT Hub tooschedule a sledování úloh, které aktualizují miliony zařízení. Pomocí úloh:

* Aktualizace požadovaných vlastností
* Aktualizace značky
* Vyvolání metody přímé

Úlohu zabalí jednu z těchto akcí a sleduje hello vůči sadu zařízení, která je definována dotazem twin zařízení. Back-end aplikačním můžete například použít úlohy tooinvoke přímá metoda na 10 000 zařízení, která restartuje hello zařízení. Zadejte hello sadu zařízení s dotazem twin zařízení a naplánovat úlohu toorun hello na datum v budoucnosti. sleduje průběh úlohy Hello jako každý hello zařízení obdrží a provést přímý metodu hello restartování.

v tématu toolearn Další informace o každé z těchto funkcí:

* Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení] [ lnk-get-started-twin] a [kurz: jak dvojče zařízení toouse vlastnosti][lnk-twin-props]
* Přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody] [ lnk-dev-methods] a [kurz: použijte přímý metody][lnk-c2d-methods]

V tomto kurzu získáte informace o následujících postupech:

* Vytvoření aplikace zařízení, která implementuje přímé metodu s názvem **lockDoor** , lze volat back-end aplikace hello. aplikace Hello zařízení také obdrží požadovanou vlastnost změny z back-end aplikace hello.
* Vytvořit aplikaci back-end, která vytvoří úlohy toocall hello **lockDoor** přímá metoda na několika zařízeních. Jiná úloha odešle požadovanou vlastnost aktualizací toomultiple zařízení.

Na konci hello tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):

**simDevice.js** , připojí tooyour IoT hub, implementuje hello **lockDoor** přímá metoda a zpracovává potřeby změny vlastností.

**ScheduleJob** hello toocall úlohy, který používá **lockDoor** přímá metoda a aktualizace hello dvojče zařízení požadovaných vlastností na několika zařízeních.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Node.js verze 0.12.x nebo novější. článek Hello [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.
* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Plán úlohy pro volání přímá metoda a odesílání twin aktualizace zařízení

V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#) používající úlohy toocall hello **lockDoor** přímá metoda a odesílat požadovanou vlastnost aktualizace toomultiple zařízení.

1. V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **ScheduleJob**.

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ScheduleJob** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello. Tento krok stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.

    ![Okno Správce balíčků NuGet][img-servicenuget]
1. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Přidejte následující hello `using` příkaz, pokud ještě není přítomný v příkazech výchozí hello.

    ```csharp
    using System.Threading.Tasks;
    ```

1. Přidejte následující pole toohello hello **Program** třídy. Nahraďte zástupný symbol hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. Přidejte následující metodu toohello hello **Program** třídy:

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

1. Přidejte následující metodu toohello hello **Program** třídy:

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

1. Přidejte následující metodu toohello hello **Program** třídy:

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

1. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:

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

1. V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **ScheduleJob** je projekt **spustit**. Vytvoření řešení hello.

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení

V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje tooa přímá metoda volá hello cloudu, což aktivuje restartu simulované zařízení a hello používá hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a při jejich poslední restartován.

1. Vytvořit novou prázdnou složku s názvem **simDevice**.  V hello **simDevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.  Přijměte všechny výchozí hodnoty hello:

    ```cmd/sh
    npm init
    ```

1. Na příkazovém řádku v hello **simDevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** a **azure-iot zařízení mqtt** balíčky:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Pomocí textového editoru, vytvořte novou **simDevice.js** souboru v hello **simDevice** složky.

1. Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **simDevice.js** souboru:

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance. Ujistěte se, že tooreplace hello zástupné hodnoty odpovídající tooyour spuštěním instalačního programu.

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Přidejte následující funkce toohandle hello hello **lockDoor** metoda.

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

1. Přidejte následující kód tooregister hello obslužné rutiny pro hello hello **lockDoor** metoda.

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

1. Uložte a zavřete hello **simDevice.js** souboru.

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].

## <a name="run-hello-apps"></a>Spuštění aplikace hello

Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku hello v hello **simDevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.

    ```cmd/sh
    node simDevice.js
    ```

1. Spuštění hello C# konzolovou aplikaci **ScheduleJob** kliknutím pravým tlačítkem na hello **ScheduleJob** projekt, pak výběrem **ladění** a **spustit novou instanci**.

1. Zobrazí výstup hello ze zařízení i back-end aplikace.

    ![Spuštění aplikace hello tooschedule úloh][img-schedulejobs]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste použili úlohy tooschedule zařízení tooa přímá metoda a hello aktualizace vlastností dvojče zařízení hello.

Začínáme se službou IoT Hub a zařízení vzory správy, jako je vzdálené přes aktualizaci firmwaru letecké hello, přečtěte si toocontinue [kurz: jak toodo firmwarem aktualizovat][lnk-fwupdate].

najdete v části Začínáme se službou IoT Hub, toocontinue [Začínáme se službou IoT Edge][lnk-iot-edge].

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
