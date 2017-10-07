---
title: "Vlastnosti twin zařízení Azure IoT Hub aaaUse (.NET/.NET) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub dvojčata zařízení tooconfigure zařízení. Používáte zařízení Azure IoT hello SDK pro .NET tooimplement aplikace simulovaného zařízení a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která upraví konfiguraci zařízení pomocí dvojče zařízení."
services: iot-hub
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dkshir
ms.openlocfilehash: 486436d29abfd5158c253adc5abf5935e0e1fdba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices"></a>Použití zařízení tooconfigure požadované vlastnosti
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na konci hello tohoto kurzu budete mít dvě aplikace konzoly .NET:

* **SimulateDeviceConfiguration**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a sestavy hello stav procesu aktualizace simulované konfigurace.
* **SetDesiredConfigurationAndQuery**, back-end aplikace, která nastaví hello požadovaných konfigurací na zařízení a dotazů hello proces aktualizace konfigurace.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.
> 
> 

toocomplete tohoto kurzu potřebujete následující hello:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].

Pokud jste postupovali podle hello [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**. V takovém případě můžete přeskočit toohello [aplikaci simulovaného zařízení vytvořit hello] [ lnk-how-to-configure-createapp] části.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení hello
V této části vytvoříte konzolovou aplikaci .NET, která připojí tooyour hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a v procesu aktualizace konfigurace hello simulated hlásí aktualizace.

1. V sadě Visual Studio vytvořte nový projekt Visual C# Windows klasický desktopový pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **SimulateDeviceConfiguration**.
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]

1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **SimulateDeviceConfiguration** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**. Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello zařízení připojovací řetězec, který jste si poznamenali v předchozím oddílu hello.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. Přidejte následující metodu toohello hello **Program** třídy:
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting toohub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    Hello **klienta** objekt poskytuje všechny metody hello vyžadují toointeract s dvojčata zařízení z hello zařízení. Hello výše, uvedeném kódu inicializuje hello **klienta** objektu a potom načte hello dvojče zařízení pro **myDeviceId**.

1. Přidejte následující metodu toohello hello **Program** třídy. Tato metoda nastaví počáteční hodnoty hello telemetrie na místním zařízení hello a poté aktualizace hello dvojče zařízení.

        public static async void InitTelemetry()
        {
            try
            {
                Console.WriteLine("Report initial telemetry config:");
                TwinCollection telemetryConfig = new TwinCollection();
                
                telemetryConfig["configId"] = "0";
                telemetryConfig["sendFrequency"] = "24h";
                reportedProperties["telemetryConfig"] = telemetryConfig;
                Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Přidejte následující metodu toohello hello **Program** třídy. Toto je zpětného volání, která zjistí změnu *potřeby vlastnosti* v dvojče zařízení hello.

        private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            try
            {
                Console.WriteLine("Desired property change:");
                Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                var currentTelemetryConfig = reportedProperties["telemetryConfig"];
                var desiredTelemetryConfig = desiredProperties["telemetryConfig"];

                if ((desiredTelemetryConfig != null) && (desiredTelemetryConfig["configId"] != currentTelemetryConfig["configId"]))
                {
                    Console.WriteLine("\nInitiating config change");
                    currentTelemetryConfig["status"] = "Pending";
                    currentTelemetryConfig["pendingConfig"] = desiredTelemetryConfig;

                    await Client.UpdateReportedPropertiesAsync(reportedProperties);

                    CompleteConfigChange();
                }
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    Tato metoda aktualizace hello hlášené vlastnosti objektu twin hello místní zařízení s konfigurací hello aktualizovat požadavku a nastaví stav hello příliš**čekající**, pak aktualizací hello dvojče zařízení ve službě hello. Po úspěšné aktualizaci dvojče zařízení hello, dokončení změně konfigurace hello voláním metody hello `CompleteConfigChange` popsané v další bod hello.

1. Přidejte následující metodu toohello hello **Program** třídy. Tato metoda simuluje resetování zařízení a potom aktualizace hello místní hlášené vlastnosti nastavení stavu hello příliš**úspěch** a odebere hello **pendingConfig** element. Pak aktualizuje hello dvojče zařízení ve službě hello. 

        public static async void CompleteConfigChange()
        {
            try
            {
                var currentTelemetryConfig = reportedProperties["telemetryConfig"];

                Console.WriteLine("\nSimulating device reset");
                await Task.Delay(30000); 

                Console.WriteLine("\nCompleting config change");
                currentTelemetryConfig["configId"] = currentTelemetryConfig["pendingConfig"]["configId"];
                currentTelemetryConfig["sendFrequency"] = currentTelemetryConfig["pendingConfig"]["sendFrequency"];
                currentTelemetryConfig["status"] = "Success";
                currentTelemetryConfig["pendingConfig"] = null;

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
                Console.WriteLine("Config change complete \nPress any key tooexit.");
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:

        try
        {
            InitClient();
            InitTelemetry();

            Console.WriteLine("Wait for desired telemetry...");
            Client.SetDesiredPropertyUpdateCallback(OnDesiredPropertyChanged, null).Wait();
            Console.ReadKey();
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }

   > [!NOTE]
   > V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace. Některé procesy aktualizace konfigurace může být schopný tooaccommodate změn konfigurace cílového spuštěného hello aktualizace, některé může mít tooqueue je a některé by mohly odmítněte s chybový stav. Ujistěte se, že tooconsider hello požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku hello před zahájením hello změně konfigurace.
   > 
   > 
1. Vytvoření hello řešení a pak spusťte aplikaci zařízení hello ze sady Visual Studio kliknutím **F5**. V konzole výstup hello měli byste vidět zprávy hello indikující, že je načítání hello dvojče zařízení simulovaného zařízení, nastavení hello telemetrie a čekání na změnu požadované vlastnosti. Zachovat hello aplikace spuštěna.

## <a name="create-hello-service-app"></a>Vytvoření aplikace hello service
V této části vytvoříte konzolovou aplikaci .NET této aktualizace hello *potřeby vlastnosti* na dvojče zařízení hello přidružené **myDeviceId** s nový objekt konfigurace telemetrie. Pak dotazuje dvojčata zařízení hello uložené ve hello IoT hub a ukazuje hello rozdíl mezi hello potřeby a který ohlásil konfigurace hello zařízení.

1. V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **SetDesiredConfigurationAndQuery**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **SetDesiredConfigurationAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Přidejte následující metodu toohello hello **Program** třídy:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }
   
    Hello **registru** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello. Tento kód inicializuje hello **registru** objektu, načte hello dvojče zařízení pro **myDeviceId**a pak aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.
    Poté vyžádá si dvojčata zařízení hello uložené ve hello IoT hub každých 10 sekund a výtisků hello potřeby a hlášené telemetrie konfigurace. Odkazovat toohello [IoT Hub dotazovací jazyk] [ lnk-query] toolearn jak toogenerate bohaté sestavy na všech zařízeních.
   
   > [!IMPORTANT]
   > Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund. Použití dotazuje sestavy zobrazující se uživatelům toogenerate napříč mnoho zařízení a ne toodetect změny. Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].
   > 
   > 
1. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **SetDesiredConfigurationAndQuery** je projekt **spustit**. Vytvoření řešení hello.
1. S **SimulateDeviceConfiguration** zařízení aplikace spuštěná, spusťte hello aplikace service pomocí sady Visual Studio **F5**. Měli byste vidět hello změnu z hlášené konfigurace **čekající** příliš**úspěch** s nové aktivní hello odeslat frekvenci pět minut místo 24 hodin.

 ![Zařízení byl úspěšně nakonfigurován][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Není zpoždění až minutu tooa mezi hello zařízení sestavy operace a výsledek dotazu hello. Toto je tooenable hello dotazu infrastruktury toowork ve velmi velkém rozsahu. tooretrieve konzistentní zobrazení twin jedno zařízení použít hello **getDeviceTwin** metoda v hello **registru** třídy.
   > 
   > 

## <a name="next-steps"></a>Další kroky
V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z řešení hello back-end a napsali toodetect aplikace zařízení, která změnit a simulace aktualizace vícekrokový proces reporting její stav prostřednictvím hello hlášené Vlastnosti.

Použití hello následující toolearn prostředky jak pro:

* odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu
* naplánovat nebo provádět operace u velkých sad zařízení najdete v části hello [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.
* kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/devicesdknuget.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app
