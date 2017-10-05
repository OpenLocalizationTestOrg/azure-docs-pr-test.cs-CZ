---
title: "Začínáme s Azure IoT Hub dvojčata zařízení (.NET/.NET) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub přidat značky a pak použijte dotaz služby IoT Hub. Použití zařízení Azure IoT sady SDK pro .NET pro implementaci aplikaci simulovaného zařízení a sady SDK pro .NET k implementaci aplikační služby, které přidá značky a spustí dotaz IoT Hub služby Azure IoT."
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
ms.openlocfilehash: 6073d594117e69676b753a1e3af25fffa3583a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-device-twins-netnet"></a><span data-ttu-id="2351d-104">Začínáme s dvojčata zařízení (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="2351d-104">Get started with device twins (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="2351d-105">Na konci tohoto kurzu budete mít tyto aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="2351d-105">At the end of this tutorial, you will have these .NET console apps:</span></span>

* <span data-ttu-id="2351d-106">**CreateDeviceIdentity**, aplikace .NET, která vytvoří identitu zařízení a přiřazený bezpečnostní klíč k připojení aplikace simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="2351d-106">**CreateDeviceIdentity**, a .NET app which creates a device identity and associated security key to connect your simulated device app.</span></span>
* <span data-ttu-id="2351d-107">**AddTagsAndQuery**, aplikace .NET back-end, které přidá značky a dotazuje dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="2351d-107">**AddTagsAndQuery**, a .NET back-end app which adds tags and queries device twins.</span></span>
* <span data-ttu-id="2351d-108">**ReportConnectivity**, aplikace .NET zařízení, která simuluje zařízení, která se připojuje ke službě IoT hub s dříve vytvořenou identitou zařízení a sestav stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="2351d-108">**ReportConnectivity**, a .NET device app which simulates a device that connects to your IoT hub with the device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="2351d-109">Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.</span><span class="sxs-lookup"><span data-stu-id="2351d-109">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="2351d-110">K dokončení tohoto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="2351d-110">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="2351d-111">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2351d-111">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="2351d-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="2351d-112">An active Azure account.</span></span> <span data-ttu-id="2351d-113">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="2351d-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="2351d-114">Pokud chcete vytvořit identitu zařízení programově místo, najdete v příslušné části [připojení simulovaného zařízení do služby IoT hub pomocí rozhraní .NET] [ lnk-device-identity-csharp] článku.</span><span class="sxs-lookup"><span data-stu-id="2351d-114">If you want to create the device identity programmatically instead, read the corresponding section in the [Connect your simulated device to your IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="2351d-115">Vytvořit aplikaci aplikační služby</span><span class="sxs-lookup"><span data-stu-id="2351d-115">Create the service app</span></span>
<span data-ttu-id="2351d-116">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#), přidá do dvojče zařízení přidružená metadata umístění **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="2351d-116">In this section, you create a .NET console app (using C#) that adds location metadata to the device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="2351d-117">Následně se dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub, výběrem zařízení nachází v USA a ty, které hlášené mobilní připojení.</span><span class="sxs-lookup"><span data-stu-id="2351d-117">It then queries the device twins stored in the IoT hub selecting the devices located in the US, and then the ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="2351d-118">V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2351d-118">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="2351d-119">Název projektu **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="2351d-119">Name the project **AddTagsAndQuery**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. <span data-ttu-id="2351d-121">V Průzkumníku řešení klikněte pravým tlačítkem myši **AddTagsAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet... **.</span><span class="sxs-lookup"><span data-stu-id="2351d-121">In Solution Explorer, right-click the **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="2351d-122">V **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="2351d-122">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="2351d-123">Vyberte **nainstalovat** k instalaci **Microsoft.Azure.Devices** balíček a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="2351d-123">Select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="2351d-124">Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="2351d-124">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="2351d-126">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="2351d-126">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="2351d-127">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="2351d-127">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="2351d-128">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="2351d-128">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="2351d-129">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="2351d-129">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="2351d-130">**RegistryManager** třída poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze služby.</span><span class="sxs-lookup"><span data-stu-id="2351d-130">The **RegistryManager** class exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="2351d-131">Předchozí kód nejprve inicializuje **registryManager** objekt a potom načte dvojče zařízení pro **myDeviceId**a nakonec jeho značky aktualizuje informace o požadované umístění.</span><span class="sxs-lookup"><span data-stu-id="2351d-131">The previous code first initializes the **registryManager** object, then retrieves the device twin for **myDeviceId**, and finally updates its tags with the desired location information.</span></span>
   
    <span data-ttu-id="2351d-132">Po aktualizaci, se provede dva dotazy: první vybere pouze dvojčata zařízení nachází v zařízení **Redmond43** závodu a druhý zpřesnění dotazu a vyberte pouze zařízení, která jsou také připojené přes mobilní síť.</span><span class="sxs-lookup"><span data-stu-id="2351d-132">After updating, it executes two queries: the first selects only the device twins of devices located in the **Redmond43** plant, and the second refines the query to select only the devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="2351d-133">Všimněte si, že předchozí kód, když vytváří **dotazu** objektu, určuje maximální počet vrácených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="2351d-133">Note that the previous code, when it creates the **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="2351d-134">**Dotazu** objekt obsahuje **HasMoreResults** vlastnost typu boolean, který můžete použít k vyvolání **GetNextAsTwinAsync** metody několikrát načíst všechny výsledky.</span><span class="sxs-lookup"><span data-stu-id="2351d-134">The **query** object contains a **HasMoreResults** boolean property that you can use to invoke the **GetNextAsTwinAsync** methods multiple times to retrieve all results.</span></span> <span data-ttu-id="2351d-135">Volána metoda **GetNextAsJson** je k dispozici pro výsledky, které není dvojčata zařízení, například výsledky dotazů agregace.</span><span class="sxs-lookup"><span data-stu-id="2351d-135">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="2351d-136">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="2351d-136">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. <span data-ttu-id="2351d-137">V Průzkumníku řešení otevřete **nastavit projekty po spuštění... ** a zajistěte, aby **akce** pro **AddTagsAndQuery** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="2351d-137">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="2351d-138">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="2351d-138">Build the solution.</span></span>
1. <span data-ttu-id="2351d-139">Kliknutím pravým tlačítkem na spuštění této aplikace **AddTagsAndQuery** projekt a výběrem **ladění**, za nímž následují **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="2351d-139">Run this application by right-clicking on the **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="2351d-140">Měli byste vidět jedno zařízení ve výsledcích pro dotaz s dotazem pro všechna zařízení umístěné v **Redmond43** a jeden pro dotaz, který omezuje výsledky na zařízení, která používají mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="2351d-140">You should see one device in the results for the query asking for all devices located in **Redmond43** and none for the query that restricts the results to devices that use a cellular network.</span></span>
   
    ![Výsledky dotazu v okně][img-addtagapp]

<span data-ttu-id="2351d-142">V další části můžete vytvořit aplikaci zařízení, která hlásí informace o připojení a změní výsledek dotazu v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="2351d-142">In the next section, you create a device app that reports the connectivity information and changes the result of the query in the previous section.</span></span>

## <a name="create-the-device-app"></a><span data-ttu-id="2351d-143">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="2351d-143">Create the device app</span></span>
<span data-ttu-id="2351d-144">V této části vytvoříte konzolovou aplikaci .NET, která se připojuje k vaší hub jako **myDeviceId**a pak aktualizuje jeho hlášené vlastnosti tak, aby obsahovala informace, že je připojený pomocí mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="2351d-144">In this section, you create a .NET console app that connects to your hub as **myDeviceId**, and then updates its reported properties to contain the information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="2351d-145">V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2351d-145">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="2351d-146">Název projektu **ReportConnectivity**.</span><span class="sxs-lookup"><span data-stu-id="2351d-146">Name the project **ReportConnectivity**.</span></span>
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]
    
1. <span data-ttu-id="2351d-148">V Průzkumníku řešení klikněte pravým tlačítkem myši **ReportConnectivity** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet... **.</span><span class="sxs-lookup"><span data-stu-id="2351d-148">In Solution Explorer, right-click the **ReportConnectivity** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="2351d-149">V **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="2351d-149">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="2351d-150">Vyberte **nainstalovat** k instalaci **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="2351d-150">Select **Install** to install the **Microsoft.Azure.Devices.Client** package, and accept the terms of use.</span></span> <span data-ttu-id="2351d-151">Tento postup stáhne, nainstaluje a přidá odkaz na [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="2351d-151">This procedure downloads, installs, and adds a reference to the [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. <span data-ttu-id="2351d-153">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="2351d-153">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="2351d-154">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="2351d-154">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="2351d-155">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem zařízení, kterou jste si poznamenali v předchozím oddílu.</span><span class="sxs-lookup"><span data-stu-id="2351d-155">Replace the placeholder value with the device connection string that you noted in the previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="2351d-156">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="2351d-156">Add the following method to the **Program** class:</span></span>

       public static async void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
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

    <span data-ttu-id="2351d-157">**Klienta** objekt poskytuje všechny metody vyžadovat interakci s dvojčata zařízení ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="2351d-157">The **Client** object exposes all the methods you require to interact with device twins from the device.</span></span> <span data-ttu-id="2351d-158">Inicializuje výše uvedeném kódu **klienta** objektu a potom načte dvojče zařízení pro **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="2351d-158">The code shown above, initializes the **Client** object, and then retrieves the device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="2351d-159">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="2351d-159">Add the following method to the **Program** class:</span></span>
   
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

   <span data-ttu-id="2351d-160">Kód výše aktualizace **myDeviceId**je hlášené vlastnost s informacemi o připojení.</span><span class="sxs-lookup"><span data-stu-id="2351d-160">The code above updates **myDeviceId**'s reported property with the connectivity information.</span></span>

1. <span data-ttu-id="2351d-161">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="2351d-161">Finally, add the following lines to the **Main** method:</span></span>
   
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
       Console.WriteLine("Press Enter to exit.");
       Console.ReadLine();

1. <span data-ttu-id="2351d-162">V Průzkumníku řešení otevřete **nastavit projekty po spuštění... ** a zajistěte, aby **akce** pro **ReportConnectivity** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="2351d-162">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **ReportConnectivity** project is **Start**.</span></span> <span data-ttu-id="2351d-163">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="2351d-163">Build the solution.</span></span>
1. <span data-ttu-id="2351d-164">Kliknutím pravým tlačítkem na spuštění této aplikace **ReportConnectivity** projekt a výběrem **ladění**, za nímž následují **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="2351d-164">Run this application by right-clicking on the **ReportConnectivity** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="2351d-165">Měli vidět, získávání informací o dvojici a pak odešle připojení jako *hlášené vlastnost*.</span><span class="sxs-lookup"><span data-stu-id="2351d-165">You should see it getting the twin information, and then sending connectivity as a *reported property*.</span></span>
   
    ![Spuštění aplikace zařízení pro připojení k sestavě][img-rundeviceapp]
    
    
1. <span data-ttu-id="2351d-167">Teď, když je zařízení hlášené jeho informace o připojení k se má zobrazit v obou dotazy.</span><span class="sxs-lookup"><span data-stu-id="2351d-167">Now that the device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="2351d-168">Spustit .NET **AddTagsAndQuery** aplikaci znovu spustit dotazy.</span><span class="sxs-lookup"><span data-stu-id="2351d-168">Run the .NET **AddTagsAndQuery** app to run the queries again.</span></span> <span data-ttu-id="2351d-169">Tentokrát **myDeviceId** by se měla objevit v obou výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="2351d-169">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![Připojení zařízení úspěšně hlášené][img-tagappsuccess]

## <a name="next-steps"></a><span data-ttu-id="2351d-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2351d-171">Next steps</span></span>
<span data-ttu-id="2351d-172">V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2351d-172">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="2351d-173">Přidat zařízení metadat jako značky z back-end aplikace a zapsal aplikace simulovaného zařízení do sestavy informace o připojení k zařízení v dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="2351d-173">You added device metadata as tags from a back-end app, and wrote a simulated device app to report device connectivity information in the device twin.</span></span> <span data-ttu-id="2351d-174">Také jste zjistili, jak dotazovat tyto informace pomocí dotazu jazyka SQL jako IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2351d-174">You also learned how to query this information using the SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="2351d-175">Použijte v následujících zdrojích informací další postup:</span><span class="sxs-lookup"><span data-stu-id="2351d-175">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="2351d-176">odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="2351d-176">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="2351d-177">Konfigurace zařízení pomocí dvojče zařízení požadované vlastnosti s [použití požadovaného vlastnosti pro konfiguraci zařízení] [ lnk-twin-how-to-configure] kurzu</span><span class="sxs-lookup"><span data-stu-id="2351d-177">configure devices using device twin's desired properties with the [Use desired properties to configure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="2351d-178">s kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="2351d-178">control devices interactively (such as turning on a fan from a user-controlled app) with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

