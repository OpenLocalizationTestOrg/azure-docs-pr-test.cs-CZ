---
title: "aaaGet začít s dvojčata zařízení Azure IoT Hub (.NET/.NET) | Microsoft Docs"
description: "Jak tooadd dvojčata zařízení Azure IoT Hub toouse značky a pak použít dotaz služby IoT Hub. Použití zařízení Azure IoT hello SDK pro aplikace .NET tooimplement hello simulované zařízení a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, které přidá značky hello a spustí hello dotazu IoT Hub."
services: iot-hub
documentationcenter: node
author: dsk-2015
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: dkshir
ms.openlocfilehash: 7fa73ac896c44e79c6522d252cd1515bd6e7bb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnet"></a>Začínáme s dvojčata zařízení (.NET/.NET)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Na konci hello tohoto kurzu budete mít tyto aplikace konzoly .NET:

* **CreateDeviceIdentity**, aplikace .NET, která vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace simulovaného zařízení.
* **AddTagsAndQuery**, aplikace .NET back-end, které přidá značky a dotazuje dvojčata zařízení.
* **ReportConnectivity**, aplikace .NET zařízení, která simuluje zařízení, která se připojuje tooyour IoT hub s dříve vytvořenou identitou zařízení hello a sestav stavu připojení.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.
> 
> 

toocomplete tohoto kurzu potřebujete následující hello:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Pokud chcete identitu zařízení hello toocreate prostřednictvím kódu programu místo, najdete v hello hello odpovídající části [připojení simulovaného zařízení tooyour IoT hub pomocí rozhraní .NET] [ lnk-device-identity-csharp] článku.

## <a name="create-hello-service-app"></a>Vytvoření aplikace hello service
V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#), přidá umístění metadat toohello dvojče zařízení spojené s **myDeviceId**. Následně dotazů uložená ve výběru hello zařízení nachází v hello hello IoT hub nám hello dvojčata zařízení a pak hello ta, která hlášené mobilní připojení.

1. V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **AddTagsAndQuery**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **AddTagsAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices**. Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices;
1. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Přidejte následující metodu toohello hello **Program** třídy:
   
        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }
   
    Hello **RegistryManager** třída zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello. Předchozí kód Hello nejprve inicializuje hello **registryManager** objektu, pak načte hello dvojče zařízení pro **myDeviceId**a nakonec aktualizuje jeho značky hello požadovaných informací o umístění.
   
    Po aktualizaci, se provede dva dotazy: hello první vybere pouze dvojčata zařízení hello zařízení umístěných v hello **Redmond43** zařízení a hello druhý refines hello dotazu tooselect pouze hello zařízení, která jsou také připojené prostřednictvím mobilní síti.
   
    Všimněte si, že předchozí kód hello při vytváření hello **dotazu** objektu, určuje maximální počet vrácených dokumentů. Hello **dotazu** objekt obsahuje **HasMoreResults** vlastnost typu boolean, které můžete použít tooinvoke hello **GetNextAsTwinAsync** metody tooretrieve více než jednou. všechny výsledky. Volána metoda **GetNextAsJson** je k dispozici pro výsledky, které není dvojčata zařízení, například výsledky dotazů agregace.
1. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **AddTagsAndQuery** je projekt **spustit**. Vytvoření řešení hello.
1. Tuto aplikaci spustit kliknutím pravým tlačítkem na hello **AddTagsAndQuery** projekt a výběrem **ladění**, za nímž následují **spustit novou instanci**. Měli byste vidět jeden zařízení ve výsledcích hello hello dotazu žádostí pro všechna zařízení umístěné v **Redmond43** a jeden pro hello dotaz, který omezuje hello výsledků toodevices, použít mobilní síti.
   
    ![Výsledky dotazu v okně][img-addtagapp]

V další části hello můžete vytvořit aplikaci zařízení, která hlásí informace o připojení k hello a změny hello výsledek dotazu hello v předchozí části hello.

## <a name="create-hello-device-app"></a>Vytvoření aplikace hello zařízení
V této části vytvoříte konzolovou aplikaci .NET, která připojí tooyour hub jako **myDeviceId**a pak aktualizuje jeho informace hello hlášené vlastnosti toocontain že je připojený používá mobilní síť.

1. V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **ReportConnectivity**.
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]
    
1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ReportConnectivity** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**. Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello zařízení připojovací řetězec, který jste si poznamenali v předchozím oddílu hello.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. Přidejte následující metodu toohello hello **Program** třídy:

       public static async void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting toohub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
                Console.WriteLine("Retrieving twin");
                await Client.GetTwinAsync();
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    Hello **klienta** objekt poskytuje všechny metody hello vyžadují toointeract s dvojčata zařízení z hello zařízení. Hello výše, uvedeném kódu inicializuje hello **klienta** objektu a potom načte hello dvojče zařízení pro **myDeviceId**.

1. Přidejte následující metodu toohello hello **Program** třídy:
   
        public static async void ReportConnectivity()
        {
            try
            {
                Console.WriteLine("Sending connectivity data as reported property");
                
                TwinCollection reportedProperties, connectivity;
                reportedProperties = new TwinCollection();
                connectivity = new TwinCollection();
                connectivity["type"] = "cellular";
                reportedProperties["connectivity"] = connectivity;
                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

   Hello kód výše aktualizace **myDeviceId**je hlášené vlastnost s informace o připojení k hello.

1. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
       try
       {
            InitClient();
            ReportConnectivity();
       }
       catch (Exception ex)
       {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
       }
       Console.WriteLine("Press Enter tooexit.");
       Console.ReadLine();

1. V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **ReportConnectivity** je projekt **spustit**. Vytvoření řešení hello.
1. Tuto aplikaci spustit kliknutím pravým tlačítkem na hello **ReportConnectivity** projekt a výběrem **ladění**, za nímž následují **spustit novou instanci**. Měli vidět, získávání hello twin informace a pak odešle připojení jako *hlášené vlastnost*.
   
    ![Spustit připojení tooreport aplikace zařízení][img-rundeviceapp]
    
    
1. Teď, když hello zařízení hlásí informace o jeho připojení k, mělo by se zobrazit v obou dotazy. Spuštění rozhraní .NET hello **AddTagsAndQuery** hello toorun aplikace dotazuje znovu. Tentokrát **myDeviceId** by se měla objevit v obou výsledky dotazu.
   
    ![Připojení zařízení úspěšně hlášené][img-tagappsuccess]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Přidat zařízení metadat jako značky z back-end aplikace a napsali informace připojení k zařízení simulovaného zařízení aplikaci tooreport v dvojče zařízení hello. Také jste se naučili jak tooquery tyto informace pomocí dotazovacího jazyka pro hello SQL jako IoT Hub.

Použití hello následující toolearn prostředky jak pro:

* odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu
* Konfigurace zařízení požadované vlastnosti dvojče zařízení pomocí hello [použití požadovaného vlastnosti tooconfigure zařízení] [ lnk-twin-how-to-configure] kurzu
* kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png
[img-rundeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png
[img-tagappsuccess]: media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

