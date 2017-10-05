---
title: "Začínáme s Azure IoT Hub dvojčata zařízení (.NET nebo uzel) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub přidat značky a pak použijte dotaz služby IoT Hub. Použití zařízení Azure IoT SDK pro Node.js implementovat aplikaci simulovaného zařízení a sady SDK pro .NET k implementaci aplikační služby, které přidá značky a spustí dotaz IoT Hub služby Azure IoT."
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
ms.openlocfilehash: 07797b9159c9b926e9eb47d8864c63048951931a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-device-twins-netnode"></a><span data-ttu-id="3cb88-104">Začínáme s dvojčata zařízení (.NET nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="3cb88-104">Get started with device twins (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="3cb88-105">Na konci tohoto kurzu budete mít .NET a konzolovou aplikaci softwaru Node.js:</span><span class="sxs-lookup"><span data-stu-id="3cb88-105">At the end of this tutorial, you will have a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="3cb88-106">**AddTagsAndQuery.sln**, aplikace .NET back-end, které přidá značky a dotazuje dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="3cb88-106">**AddTagsAndQuery.sln**, a .NET back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="3cb88-107">**TwinSimulatedDevice.js**, aplikace Node.js, která simuluje zařízení, která se připojuje ke službě IoT hub s dříve vytvořenou identitou zařízení a sestav stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="3cb88-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects to your IoT hub with the device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="3cb88-108">Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.</span><span class="sxs-lookup"><span data-stu-id="3cb88-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="3cb88-109">K dokončení tohoto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="3cb88-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="3cb88-110">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3cb88-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="3cb88-111">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3cb88-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="3cb88-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3cb88-112">An active Azure account.</span></span> <span data-ttu-id="3cb88-113">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="3cb88-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-service-app"></a><span data-ttu-id="3cb88-114">Vytvořit aplikaci aplikační služby</span><span class="sxs-lookup"><span data-stu-id="3cb88-114">Create the service app</span></span>
<span data-ttu-id="3cb88-115">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#), přidá do dvojče zařízení přidružená metadata umístění **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="3cb88-115">In this section, you create a .NET console app (using C#) that adds location metadata to the device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="3cb88-116">Následně se dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub, výběrem zařízení nachází v USA a ty, které hlášené mobilní připojení.</span><span class="sxs-lookup"><span data-stu-id="3cb88-116">It then queries the device twins stored in the IoT hub selecting the devices located in the US, and then the ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="3cb88-117">V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3cb88-117">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="3cb88-118">Název projektu **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="3cb88-118">Name the project **AddTagsAndQuery**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. <span data-ttu-id="3cb88-120">V Průzkumníku řešení klikněte pravým tlačítkem myši **AddTagsAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="3cb88-120">In Solution Explorer, right-click the **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="3cb88-121">V **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="3cb88-121">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="3cb88-122">Vyberte **nainstalovat** k instalaci **Microsoft.Azure.Devices** balíček a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="3cb88-122">Select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="3cb88-123">Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="3cb88-123">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="3cb88-125">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="3cb88-125">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="3cb88-126">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="3cb88-126">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="3cb88-127">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="3cb88-127">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="3cb88-128">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="3cb88-128">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="3cb88-129">**RegistryManager** třída poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze služby.</span><span class="sxs-lookup"><span data-stu-id="3cb88-129">The **RegistryManager** class exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="3cb88-130">Předchozí kód nejprve inicializuje **registryManager** objekt a potom načte dvojče zařízení pro **myDeviceId**a nakonec jeho značky aktualizuje informace o požadované umístění.</span><span class="sxs-lookup"><span data-stu-id="3cb88-130">The previous code first initializes the **registryManager** object, then retrieves the device twin for **myDeviceId**, and finally updates its tags with the desired location information.</span></span>
   
    <span data-ttu-id="3cb88-131">Po aktualizaci, se provede dva dotazy: první vybere pouze dvojčata zařízení nachází v zařízení **Redmond43** závodu a druhý zpřesnění dotazu a vyberte pouze zařízení, která jsou také připojené přes mobilní síť.</span><span class="sxs-lookup"><span data-stu-id="3cb88-131">After updating, it executes two queries: the first selects only the device twins of devices located in the **Redmond43** plant, and the second refines the query to select only the devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="3cb88-132">Všimněte si, že předchozí kód, když vytváří **dotazu** objektu, určuje maximální počet vrácených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="3cb88-132">Note that the previous code, when it creates the **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="3cb88-133">**Dotazu** objekt obsahuje **HasMoreResults** vlastnost typu boolean, který můžete použít k vyvolání **GetNextAsTwinAsync** metody několikrát načíst všechny výsledky.</span><span class="sxs-lookup"><span data-stu-id="3cb88-133">The **query** object contains a **HasMoreResults** boolean property that you can use to invoke the **GetNextAsTwinAsync** methods multiple times to retrieve all results.</span></span> <span data-ttu-id="3cb88-134">Volána metoda **GetNextAsJson** je k dispozici pro výsledky, které není dvojčata zařízení, například výsledky dotazů agregace.</span><span class="sxs-lookup"><span data-stu-id="3cb88-134">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="3cb88-135">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="3cb88-135">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. <span data-ttu-id="3cb88-136">V Průzkumníku řešení otevřete **nastavit projekty po spuštění...**  a zajistěte, aby **akce** pro **AddTagsAndQuery** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="3cb88-136">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="3cb88-137">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="3cb88-137">Build the solution.</span></span>
1. <span data-ttu-id="3cb88-138">Kliknutím pravým tlačítkem na spuštění této aplikace **AddTagsAndQuery** projekt a výběrem **ladění**, za nímž následují **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="3cb88-138">Run this application by right-clicking on the **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="3cb88-139">Měli byste vidět jedno zařízení ve výsledcích pro dotaz s dotazem pro všechna zařízení umístěné v **Redmond43** a jeden pro dotaz, který omezuje výsledky na zařízení, která používají mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="3cb88-139">You should see one device in the results for the query asking for all devices located in **Redmond43** and none for the query that restricts the results to devices that use a cellular network.</span></span>
   
    ![Výsledky dotazu v okně][img-addtagapp]

<span data-ttu-id="3cb88-141">V další části můžete vytvořit aplikaci zařízení, která hlásí informace o připojení a změní výsledek dotazu v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="3cb88-141">In the next section, you create a device app that reports the connectivity information and changes the result of the query in the previous section.</span></span>

## <a name="create-the-device-app"></a><span data-ttu-id="3cb88-142">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="3cb88-142">Create the device app</span></span>
<span data-ttu-id="3cb88-143">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje k vaší hub jako **myDeviceId**a pak aktualizuje jeho hlášené vlastnosti tak, aby obsahovala informace, že je připojený pomocí mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="3cb88-143">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, and then updates its reported properties to contain the information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="3cb88-144">Vytvořit novou prázdnou složku s názvem **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="3cb88-144">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="3cb88-145">V **reportconnectivity** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="3cb88-145">In the **reportconnectivity** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="3cb88-146">Přijměte všechny výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3cb88-146">Accept all the defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="3cb88-147">Na příkazovém řádku v **reportconnectivity** složky, spusťte následující příkaz k instalaci **azure-iot-device**, a **azure-iot zařízení mqtt** balíčku:</span><span class="sxs-lookup"><span data-stu-id="3cb88-147">At your command prompt in the **reportconnectivity** folder, run the following command to install the **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="3cb88-148">Pomocí textového editoru, vytvořte novou **ReportConnectivity.js** v soubor **reportconnectivity** složky.</span><span class="sxs-lookup"><span data-stu-id="3cb88-148">Using a text editor, create a new **ReportConnectivity.js** file in the **reportconnectivity** folder.</span></span>
1. <span data-ttu-id="3cb88-149">Přidejte následující kód, který **ReportConnectivity.js** souboru a nahraďte zástupný symbol pro řetězec připojení zařízení s jedním jste zkopírovali, když jste vytvořili **myDeviceId** identitu zařízení:</span><span class="sxs-lookup"><span data-stu-id="3cb88-149">Add the following code to the **ReportConnectivity.js** file, and substitute the placeholder for device connection string with the one you copied when you created the **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="3cb88-150">**Klienta** objekt poskytuje všechny metody vyžadovat interakci s dvojčata zařízení ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="3cb88-150">The **Client** object exposes all the methods you require to interact with device twins from the device.</span></span> <span data-ttu-id="3cb88-151">Předchozí kód, jakmile ji inicializuje **klienta** objektu, načte dvojče zařízení pro **myDeviceId** a aktualizuje jeho hlášené vlastnost s informacemi o připojení.</span><span class="sxs-lookup"><span data-stu-id="3cb88-151">The previous code, after it initializes the **Client** object, retrieves the device twin for **myDeviceId** and updates its reported property with the connectivity information.</span></span>
1. <span data-ttu-id="3cb88-152">Spuštění aplikace zařízení</span><span class="sxs-lookup"><span data-stu-id="3cb88-152">Run the device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="3cb88-153">Měli byste vidět zprávu `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="3cb88-153">You should see the message `twin state reported`.</span></span>
1. <span data-ttu-id="3cb88-154">Teď, když je zařízení hlášené jeho informace o připojení k se má zobrazit v obou dotazy.</span><span class="sxs-lookup"><span data-stu-id="3cb88-154">Now that the device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="3cb88-155">Spustit .NET **AddTagsAndQuery** aplikaci znovu spustit dotazy.</span><span class="sxs-lookup"><span data-stu-id="3cb88-155">Run the .NET **AddTagsAndQuery** app to run the queries again.</span></span> <span data-ttu-id="3cb88-156">Tentokrát **myDeviceId** by se měla objevit v obou výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="3cb88-156">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][img-addtagapp2]

## <a name="next-steps"></a><span data-ttu-id="3cb88-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3cb88-157">Next steps</span></span>
<span data-ttu-id="3cb88-158">V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="3cb88-158">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="3cb88-159">Přidat zařízení metadat jako značky z back-end aplikace a zapsal aplikace simulovaného zařízení do sestavy informace o připojení k zařízení v dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="3cb88-159">You added device metadata as tags from a back-end app, and wrote a simulated device app to report device connectivity information in the device twin.</span></span> <span data-ttu-id="3cb88-160">Také jste zjistili, jak dotazovat tyto informace pomocí dotazu jazyka SQL jako IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="3cb88-160">You also learned how to query this information using the SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="3cb88-161">Použijte v následujících zdrojích informací další postup:</span><span class="sxs-lookup"><span data-stu-id="3cb88-161">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="3cb88-162">odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="3cb88-162">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="3cb88-163">Konfigurace zařízení pomocí dvojče zařízení požadované vlastnosti s [použití požadovaného vlastnosti pro konfiguraci zařízení] [ lnk-twin-how-to-configure] kurzu</span><span class="sxs-lookup"><span data-stu-id="3cb88-163">configure devices using device twin's desired properties with the [Use desired properties to configure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="3cb88-164">s kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="3cb88-164">control devices interactively (such as turning on a fan from a user-controlled app) with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

