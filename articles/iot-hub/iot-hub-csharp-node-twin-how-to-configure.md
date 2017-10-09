---
title: "Vlastnosti twin zařízení Azure IoT Hub aaaUse (.NET nebo uzel) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub dvojčata zařízení tooconfigure zařízení. Používáte zařízení Azure IoT hello SDK pro Node.js tooimplement aplikace simulovaného zařízení a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která upraví konfiguraci zařízení pomocí dvojče zařízení."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
ms.openlocfilehash: 840a1b2e45f4763131299577583aa89015dcdd1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices"></a>Použití zařízení tooconfigure požadované vlastnosti
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na konci hello tohoto kurzu budete mít dvě aplikace konzoly:

* **SimulateDeviceConfiguration.js**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a sestavy hello stav procesu aktualizace simulované konfigurace.
* **SetDesiredConfigurationAndQuery**, back-end aplikace .NET, která nastaví hello požadovaných konfigurací na zařízení a dotazů hello proces aktualizace konfigurace.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.
> 
> 

toocomplete tohoto kurzu potřebujete následující hello:

* Visual Studio 2015 nebo Visual Studio 2017.
* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].

Pokud jste postupovali podle hello [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**. V takovém případě můžete přeskočit toohello [aplikaci simulovaného zařízení vytvořit hello] [ lnk-how-to-configure-createapp] části.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení hello
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje tooyour hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a v procesu aktualizace konfigurace hello simulated hlásí aktualizace.

1. Vytvořit novou prázdnou složku s názvem **simulatedeviceconfiguration**. V hello **simulatedeviceconfiguration** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello.
   
    ```
    npm init
    ```
1. Na příkazovém řádku v hello **simulatedeviceconfiguration** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** a **azure-iot zařízení mqtt**balíčky:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Pomocí textového editoru, vytvořte novou **SimulateDeviceConfiguration.js** souboru v hello **simulatedeviceconfiguration** složky.
1. Přidejte následující kód toohello hello **SimulateDeviceConfiguration.js** souboru a nahradit hello **{zařízení připojovací řetězec}** zástupný symbol hello zařízení připojovací řetězec jste zkopírovali, kdy jste Vytvořit hello **myDeviceId** identitu zařízení:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    Hello **klienta** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení z hello zařízení. Tento kód inicializuje hello **klienta** objektu, načte hello dvojče zařízení pro **myDeviceId**a potom připojí obslužnou rutinu pro aktualizaci hello na *potřeby vlastnosti*. Obslužná rutina Hello ověřuje, že se požadavku na změnu skutečné konfigurace tak, že porovnáte hello configIds, pak vyvolá metodu, která spustí hello změně konfigurace.
   
    Všimněte si, že pro hello zájmu jednoduchost, tento kód používá výchozí pevně pro počáteční konfiguraci hello. Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.
   
   > [!IMPORTANT]
   > Události změny požadované vlastnosti jsou vždy vygenerované jednou na připojení zařízení. Zajistěte, aby se skutečné změnu v hodnotě hello toocheck požadovaných vlastností před provedením jakékoli akce.
   > 
   > 
1. Přidejte následující metody před hello hello `client.open()` volání:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    Hello **initConfigChange** metoda aktualizace hello hlášené vlastnosti objektu twin hello místní zařízení s konfigurací hello aktualizovat požadavku a nastaví stav hello příliš**čekající**, pak aktualizací hello dvojče zařízení ve službě hello. Po úspěšné aktualizaci dvojče zařízení hello, simuluje dlouhotrvající proces, který ukončí v provádění hello **completeConfigChange**. Tato metoda aktualizace hello místní hlášené vlastnosti nastavení stavu hello příliš**úspěch** a odebírání hello **pendingConfig** objektu. Pak aktualizuje hello dvojče zařízení ve službě hello.
   
    Hlášen, toosave šířky pásma, jsou aktualizovány vlastnosti zadáním změnit pouze vlastnosti toobe hello (s názvem **oprava** v hello výše kódu), místo nahrazení hello celý dokument.
   
   > [!NOTE]
   > V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace. Některé procesy aktualizace konfigurace může být schopný tooaccommodate změn konfigurace cílového spuštěného hello aktualizace, některé může mít tooqueue je a některé by mohly odmítněte s chybový stav. Ujistěte se, že tooconsider hello požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku hello před zahájením hello změně konfigurace.
   > 
   > 
1. Spuštění aplikace hello zařízení:
   
        node SimulateDeviceConfiguration.js
   
    Měli byste vidět zprávu hello `retrieved device twin`. Zachovat hello aplikace spuštěna.

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
   
            try
            {
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
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
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
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **SetDesiredConfigurationAndQuery** je projekt **spustit**. Vytvoření řešení hello.
1. S **SimulateDeviceConfiguration.js** spuštěna, spusťte aplikaci .NET hello pomocí sady Visual Studio **F5** a měli byste vidět hello změnu z hlášené konfigurace **úspěch** příliš**čekající** příliš**úspěch** znova s novou aktivní hello poslat frekvenci pět minut místo 24 hodin.

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
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-csharp-node-twin-how-to-configure.md#create-the-simulated-device-app
