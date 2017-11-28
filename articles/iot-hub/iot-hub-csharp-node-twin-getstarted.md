---
title: "aaaGet začít s dvojčata zařízení Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Jak tooadd dvojčata zařízení Azure IoT Hub toouse značky a pak použít dotaz služby IoT Hub. Použití zařízení Azure IoT hello SDK pro Node.js tooimplement hello simulované zařízení aplikaci a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, které přidá značky hello a spustí hello dotazu IoT Hub."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: elioda
ms.openlocfilehash: 1cec082ebddc19c9b87998a5fd0159d32b07acd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnode"></a><span data-ttu-id="21858-104">Začínáme s dvojčata zařízení (.NET nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="21858-104">Get started with device twins (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="21858-105">Na konci hello tohoto kurzu budete mít .NET a konzolovou aplikaci softwaru Node.js:</span><span class="sxs-lookup"><span data-stu-id="21858-105">At hello end of this tutorial, you will have a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="21858-106">**AddTagsAndQuery.sln**, aplikace .NET back-end, které přidá značky a dotazuje dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="21858-106">**AddTagsAndQuery.sln**, a .NET back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="21858-107">**TwinSimulatedDevice.js**, aplikace Node.js, která simuluje zařízení, která se připojuje tooyour IoT hub s dříve vytvořenou identitou zařízení hello a sestav stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="21858-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="21858-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="21858-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="21858-109">toocomplete tohoto kurzu potřebujete následující hello:</span><span class="sxs-lookup"><span data-stu-id="21858-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="21858-110">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="21858-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="21858-111">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="21858-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="21858-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="21858-112">An active Azure account.</span></span> <span data-ttu-id="21858-113">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="21858-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a><span data-ttu-id="21858-114">Vytvoření aplikace hello service</span><span class="sxs-lookup"><span data-stu-id="21858-114">Create hello service app</span></span>
<span data-ttu-id="21858-115">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#), přidá umístění metadat toohello dvojče zařízení spojené s **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="21858-115">In this section, you create a .NET console app (using C#) that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="21858-116">Následně dotazů uložená ve výběru hello zařízení nachází v hello hello IoT hub nám hello dvojčata zařízení a pak hello ta, která hlášené mobilní připojení.</span><span class="sxs-lookup"><span data-stu-id="21858-116">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="21858-117">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="21858-117">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="21858-118">Název projektu hello **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="21858-118">Name hello project **AddTagsAndQuery**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. <span data-ttu-id="21858-120">V Průzkumníku řešení klikněte pravým tlačítkem na hello **AddTagsAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="21858-120">In Solution Explorer, right-click hello **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="21858-121">V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="21858-121">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="21858-122">Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="21858-122">Select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="21858-123">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="21858-123">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="21858-125">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="21858-125">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="21858-126">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="21858-126">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="21858-127">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="21858-127">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="21858-128">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="21858-128">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="21858-129">Hello **RegistryManager** třída zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello.</span><span class="sxs-lookup"><span data-stu-id="21858-129">hello **RegistryManager** class exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="21858-130">Předchozí kód Hello nejprve inicializuje hello **registryManager** objektu, pak načte hello dvojče zařízení pro **myDeviceId**a nakonec aktualizuje jeho značky hello požadovaných informací o umístění.</span><span class="sxs-lookup"><span data-stu-id="21858-130">hello previous code first initializes hello **registryManager** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="21858-131">Po aktualizaci, se provede dva dotazy: hello první vybere pouze dvojčata zařízení hello zařízení umístěných v hello **Redmond43** zařízení a hello druhý refines hello dotazu tooselect pouze hello zařízení, která jsou také připojené prostřednictvím mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="21858-131">After updating, it executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="21858-132">Všimněte si, že předchozí kód hello při vytváření hello **dotazu** objektu, určuje maximální počet vrácených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="21858-132">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="21858-133">Hello **dotazu** objekt obsahuje **HasMoreResults** vlastnost typu boolean, které můžete použít tooinvoke hello **GetNextAsTwinAsync** metody tooretrieve více než jednou. všechny výsledky.</span><span class="sxs-lookup"><span data-stu-id="21858-133">hello **query** object contains a **HasMoreResults** boolean property that you can use tooinvoke hello **GetNextAsTwinAsync** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="21858-134">Volána metoda **GetNextAsJson** je k dispozici pro výsledky, které není dvojčata zařízení, například výsledky dotazů agregace.</span><span class="sxs-lookup"><span data-stu-id="21858-134">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="21858-135">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="21858-135">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="21858-136">V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **AddTagsAndQuery** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="21858-136">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="21858-137">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="21858-137">Build hello solution.</span></span>
1. <span data-ttu-id="21858-138">Tuto aplikaci spustit kliknutím pravým tlačítkem na hello **AddTagsAndQuery** projekt a výběrem **ladění**, za nímž následují **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="21858-138">Run this application by right-clicking on hello **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="21858-139">Měli byste vidět jeden zařízení ve výsledcích hello hello dotazu žádostí pro všechna zařízení umístěné v **Redmond43** a jeden pro hello dotaz, který omezuje hello výsledků toodevices, použít mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="21858-139">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![Výsledky dotazu v okně][img-addtagapp]

<span data-ttu-id="21858-141">V další části hello můžete vytvořit aplikaci zařízení, která hlásí informace o připojení k hello a změny hello výsledek dotazu hello v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="21858-141">In hello next section, you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="21858-142">Vytvoření aplikace hello zařízení</span><span class="sxs-lookup"><span data-stu-id="21858-142">Create hello device app</span></span>
<span data-ttu-id="21858-143">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje tooyour hub jako **myDeviceId**a pak aktualizuje jeho informace hello hlášené vlastnosti toocontain že je připojený používá mobilní síť.</span><span class="sxs-lookup"><span data-stu-id="21858-143">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, and then updates its reported properties toocontain hello information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="21858-144">Vytvořit novou prázdnou složku s názvem **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="21858-144">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="21858-145">V hello **reportconnectivity** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="21858-145">In hello **reportconnectivity** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="21858-146">Přijměte všechny výchozí hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="21858-146">Accept all hello defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="21858-147">Na příkazovém řádku v hello **reportconnectivity** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device**, a **azure-iot zařízení mqtt** balíčku :</span><span class="sxs-lookup"><span data-stu-id="21858-147">At your command prompt in hello **reportconnectivity** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="21858-148">Pomocí textového editoru, vytvořte novou **ReportConnectivity.js** souboru v hello **reportconnectivity** složky.</span><span class="sxs-lookup"><span data-stu-id="21858-148">Using a text editor, create a new **ReportConnectivity.js** file in hello **reportconnectivity** folder.</span></span>
1. <span data-ttu-id="21858-149">Přidejte následující kód toohello hello **ReportConnectivity.js** souboru a nahraďte zástupný symbol hello pro řetězec připojení zařízení s hello jeden jste zkopírovali, když jste vytvořili hello **myDeviceId** zařízení identity:</span><span class="sxs-lookup"><span data-stu-id="21858-149">Add hello following code toohello **ReportConnectivity.js** file, and substitute hello placeholder for device connection string with hello one you copied when you created hello **myDeviceId** device identity:</span></span>
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    <span data-ttu-id="21858-150">Hello **klienta** objekt poskytuje všechny metody hello vyžadují toointeract s dvojčata zařízení z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="21858-150">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="21858-151">Hello předchozí kód po jeho inicializuje hello **klienta** objektu, načte hello dvojče zařízení pro **myDeviceId** a aktualizuje jeho hlášené vlastnost informace o připojení k hello.</span><span class="sxs-lookup"><span data-stu-id="21858-151">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId** and updates its reported property with hello connectivity information.</span></span>
1. <span data-ttu-id="21858-152">Spuštění aplikace hello zařízení</span><span class="sxs-lookup"><span data-stu-id="21858-152">Run hello device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="21858-153">Měli byste vidět zprávu hello `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="21858-153">You should see hello message `twin state reported`.</span></span>
1. <span data-ttu-id="21858-154">Teď, když hello zařízení hlásí informace o jeho připojení k, mělo by se zobrazit v obou dotazy.</span><span class="sxs-lookup"><span data-stu-id="21858-154">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="21858-155">Spuštění rozhraní .NET hello **AddTagsAndQuery** hello toorun aplikace dotazuje znovu.</span><span class="sxs-lookup"><span data-stu-id="21858-155">Run hello .NET **AddTagsAndQuery** app toorun hello queries again.</span></span> <span data-ttu-id="21858-156">Tentokrát **myDeviceId** by se měla objevit v obou výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="21858-156">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][img-addtagapp2]

## <a name="next-steps"></a><span data-ttu-id="21858-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21858-157">Next steps</span></span>
<span data-ttu-id="21858-158">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="21858-158">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="21858-159">Přidat zařízení metadat jako značky z back-end aplikace a napsali informace připojení k zařízení simulovaného zařízení aplikaci tooreport v dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="21858-159">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="21858-160">Také jste se naučili jak tooquery tyto informace pomocí dotazovacího jazyka pro hello SQL jako IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="21858-160">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="21858-161">Použití hello následující toolearn prostředky jak pro:</span><span class="sxs-lookup"><span data-stu-id="21858-161">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="21858-162">odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="21858-162">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="21858-163">Konfigurace zařízení požadované vlastnosti dvojče zařízení pomocí hello [použití požadovaného vlastnosti tooconfigure zařízení] [ lnk-twin-how-to-configure] kurzu</span><span class="sxs-lookup"><span data-stu-id="21858-163">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="21858-164">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="21858-164">control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

