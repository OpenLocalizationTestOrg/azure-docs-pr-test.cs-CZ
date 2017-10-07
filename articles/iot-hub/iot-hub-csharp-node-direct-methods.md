---
title: "aaaUse Azure IoT Hub přímé metody (.NET nebo uzel) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub přímé metody. Použití zařízení Azure IoT hello SDK pro Node.js tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: nberdy
ms.openlocfilehash: f566f939be840eb308b00ffa4e05c4e5b3fefb39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnode"></a>Používat přímé metody (.NET nebo uzel)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

V tomto kurzu budeme toodevelop .NET a konzolovou aplikaci softwaru Node.js:

* **CallMethodOnDevice.sln**, back-end aplikace .NET, která volá metodu v aplikaci simulovaného zařízení hello a zobrazí hello odpovědi.
* **SimulatedDevice.js**, aplikace Node.js, která simuluje zařízení připojení tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odpoví toohello metodu s názvem hello cloudem.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.
> 
> 

toocomplete tohoto kurzu potřebujete:

* Visual Studio 2015 nebo Visual Studio 2017.
* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci Node.js, která odpovídá tooa metodu s názvem řešení hello zpět end.

1. Vytvořte novou prázdnou složku s názvem **simulateddevice**. V hello **simulateddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** a **azure-iot zařízení mqtt** balíčky:
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte soubor v hello **simulateddevice** složku a pojmenujte ji **SimulatedDevice.js**.
4. Přidejte následující hello `require` příkazy v hello začátek hello **SimulatedDevice.js** souboru:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Přidat **connectionString** proměnné a použít ho toocreate **DeviceClient** instance. Nahraďte **{zařízení připojovací řetězec}** s řetězcem připojení zařízení hello jste vygenerovali v hello *vytvoření identity zařízení* části:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Přidejte následující funkce tooimplement hello přímá metoda na zařízení hello hello:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Otevřete Centrum IoT tooyour hello připojení a inicializovat naslouchací proces metoda hello:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Uložte a zavřete hello **SimulatedDevice.js** souboru.

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Volání metody přímé na zařízení
V této části vytvoříte konzolovou aplikaci .NET, která volá metodu v aplikaci hello simulované zařízení a potom zobrazí hello odpovědi.

1. V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu. Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější. Název projektu hello **CallMethodOnDevice**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **CallMethodOnDevice** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
3. V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Okno Správce balíčků NuGet][11]

4. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Přidejte následující metodu toohello hello **Program** třídy:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Tato metoda volá metodu přímé s názvem `writeLine` na hello `myDeviceId` zařízení. Pak zapíše odpověď hello poskytované hello zařízení v konzole hello. Všimněte si, jak je možné toospecify hodnotu časového limitu pro toorespond hello zařízení.
7. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

## <a name="run-hello-applications"></a>Spouštění aplikací hello
Nyní je připraven toorun hello aplikace.

1. V hello Průzkumníka řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a potom vyberte hello **CallMethodOnDevice** projekt v rozevírací nabídce hello.

2. Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz toostart přijímá metoda volání ze služby IoT Hub hello:
   
    ```
    node SimulatedDevice.js
    ```
   Počkejte tooopen hello simulované zařízení:![][7]
3. Teď je hello zařízení připojeno a čekání volání metod, spustit hello .NET **CallMethodOnDevice** metody hello tooinvoke aplikace v aplikaci simulovaného zařízení hello. Měli byste vidět odpovědi zařízení hello napsané v konzole hello.
   
    ![][8]
4. Metoda toohello Hello zařízení pak reaguje tiskem tuto zprávu:
   
    ![][9]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci tooreact toomethods vyvolané hello cloudu. Můžete také vytvořit aplikaci, která volá metody na hello zařízení a zobrazí hello odezvu hello zařízení. 

toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:

* [Začínáme s centrem IoT]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Začínáme s centrem IoT]: iot-hub-node-node-getstarted.md
