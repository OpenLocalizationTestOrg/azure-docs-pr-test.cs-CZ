---
title: "aaaUse Azure IoT Hub přímé metody (.NET/.NET) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub přímé metody. Použití zařízení Azure IoT hello SDK pro .NET tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: dkshir
ms.openlocfilehash: d4fa093a99558ec6faf294c2583a14a722b9ac03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnet"></a>Používat přímé metody (.NET/.NET)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

V tomto kurzu jsme jsou probíhající toodevelop dva konzoly aplikace .NET:

* **CallMethodOnDevice**, back-end aplikace, která volá metodu v aplikaci simulovaného zařízení hello a zobrazí hello odpovědi.
* **SimulateDeviceMethods**, konzolovou aplikaci, která simuluje zařízení připojení tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odpoví toohello metodu s názvem hello cloudem.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.
> 
> 

toocomplete tohoto kurzu potřebujete:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Pokud chcete identitu zařízení hello toocreate prostřednictvím kódu programu místo, najdete v hello hello odpovídající části [připojení simulovaného zařízení tooyour IoT hub pomocí rozhraní .NET] [ lnk-device-identity-csharp] článku.


## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci .NET, která odpovídá tooa metodu s názvem řešení hello zpět end.

1. V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **SimulateDeviceMethods**.
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]
    
1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **SimulateDeviceMethods** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**. Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello zařízení připojovací řetězec, který jste si poznamenali v předchozím oddílu hello.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. Přidejte následující metodu přímé hello tooimplement na zařízení hello hello:

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written toolog.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. Nakonec přidejte následující kód toohello hello **hlavní** metoda tooopen hello připojení tooyour IoT hub a inicializovat hello metoda naslouchací proces:
   
        try
        {
            Console.WriteLine("Connecting toohub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter tooexit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove hello "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. V hello Průzkumníka řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a potom vyberte hello **SimulateDeviceMethods** projekt v rozevírací nabídce hello.        

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Volání metody přímé na zařízení
V této části vytvoříte konzolovou aplikaci .NET, která volá metodu v aplikaci hello simulované zařízení a potom zobrazí hello odpovědi.

1. V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu. Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější. Název projektu hello **CallMethodOnDevice**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createserviceapp]
2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **CallMethodOnDevice** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
3. V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Okno Správce balíčků NuGet][img-servicenuget]

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

1. V hello Průzkumníka řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a potom vyberte hello **CallMethodOnDevice** projekt v rozevírací nabídce hello.

## <a name="run-hello-applications"></a>Spouštění aplikací hello
Nyní je připraven toorun hello aplikace.

1. Spuštění aplikace zařízení .NET hello **SimulateDeviceMethods**. By se měl spustit přijímá metoda volání ze služby IoT Hub: 

    ![Spusťte aplikaci zařízení][img-deviceapprun]
1. Teď je hello zařízení připojeno a čekání volání metod, spustit hello .NET **CallMethodOnDevice** metody hello tooinvoke aplikace v aplikaci simulovaného zařízení hello. Měli byste vidět odpovědi zařízení hello napsané v konzole hello.
   
    ![Spusťte aplikaci aplikační služby][img-serviceapprun]
1. Metoda toohello Hello zařízení pak reaguje tiskem tuto zprávu:
   
    ![Přímá metoda volána na zařízení hello][img-directmethodinvoked]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci tooreact toomethods vyvolané hello cloudu. Můžete také vytvořit aplikaci, která volá metody na hello zařízení a zobrazí hello odezvu hello zařízení. 

toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:

* [Začínáme s centrem IoT]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

<!-- Images. -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-direct-methods/device-app-nuget.png
[img-createserviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-service-app.png
[img-servicenuget]: ./media/iot-hub-csharp-csharp-direct-methods/service-app-nuget.png
[img-deviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-device-app.png
[img-serviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-service-app.png
[img-directmethodinvoked]: ./media/iot-hub-csharp-csharp-direct-methods/direct-method-invoked.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[Začínáme s centrem IoT]: iot-hub-node-node-getstarted.md
