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
# <a name="get-started-with-device-twins-netnet"></a><span data-ttu-id="36f88-104">Začínáme s dvojčata zařízení (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="36f88-104">Get started with device twins (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="36f88-105">Na konci hello tohoto kurzu budete mít tyto aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="36f88-105">At hello end of this tutorial, you will have these .NET console apps:</span></span>

* <span data-ttu-id="36f88-106">**CreateDeviceIdentity**, aplikace .NET, která vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="36f88-106">**CreateDeviceIdentity**, a .NET app which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="36f88-107">**AddTagsAndQuery**, aplikace .NET back-end, které přidá značky a dotazuje dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="36f88-107">**AddTagsAndQuery**, a .NET back-end app which adds tags and queries device twins.</span></span>
* <span data-ttu-id="36f88-108">**ReportConnectivity**, aplikace .NET zařízení, která simuluje zařízení, která se připojuje tooyour IoT hub s dříve vytvořenou identitou zařízení hello a sestav stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="36f88-108">**ReportConnectivity**, a .NET device app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="36f88-109">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="36f88-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="36f88-110">toocomplete tohoto kurzu potřebujete následující hello:</span><span class="sxs-lookup"><span data-stu-id="36f88-110">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="36f88-111">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="36f88-111">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="36f88-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="36f88-112">An active Azure account.</span></span> <span data-ttu-id="36f88-113">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="36f88-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="36f88-114">Pokud chcete identitu zařízení hello toocreate prostřednictvím kódu programu místo, najdete v hello hello odpovídající části [připojení simulovaného zařízení tooyour IoT hub pomocí rozhraní .NET] [ lnk-device-identity-csharp] článku.</span><span class="sxs-lookup"><span data-stu-id="36f88-114">If you want toocreate hello device identity programmatically instead, read hello corresponding section in hello [Connect your simulated device tooyour IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="36f88-115">Vytvoření aplikace hello service</span><span class="sxs-lookup"><span data-stu-id="36f88-115">Create hello service app</span></span>
<span data-ttu-id="36f88-116">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#), přidá umístění metadat toohello dvojče zařízení spojené s **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="36f88-116">In this section, you create a .NET console app (using C#) that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="36f88-117">Následně dotazů uložená ve výběru hello zařízení nachází v hello hello IoT hub nám hello dvojčata zařízení a pak hello ta, která hlášené mobilní připojení.</span><span class="sxs-lookup"><span data-stu-id="36f88-117">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="36f88-118">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="36f88-118">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="36f88-119">Název projektu hello **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="36f88-119">Name hello project **AddTagsAndQuery**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. <span data-ttu-id="36f88-121">V Průzkumníku řešení klikněte pravým tlačítkem na hello **AddTagsAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="36f88-121">In Solution Explorer, right-click hello **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="36f88-122">V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="36f88-122">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="36f88-123">Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-123">Select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="36f88-124">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="36f88-124">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="36f88-126">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="36f88-126">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="36f88-127">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="36f88-127">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="36f88-128">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-128">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="36f88-129">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="36f88-129">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="36f88-130">Hello **RegistryManager** třída zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-130">hello **RegistryManager** class exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="36f88-131">Předchozí kód Hello nejprve inicializuje hello **registryManager** objektu, pak načte hello dvojče zařízení pro **myDeviceId**a nakonec aktualizuje jeho značky hello požadovaných informací o umístění.</span><span class="sxs-lookup"><span data-stu-id="36f88-131">hello previous code first initializes hello **registryManager** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="36f88-132">Po aktualizaci, se provede dva dotazy: hello první vybere pouze dvojčata zařízení hello zařízení umístěných v hello **Redmond43** zařízení a hello druhý refines hello dotazu tooselect pouze hello zařízení, která jsou také připojené prostřednictvím mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="36f88-132">After updating, it executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="36f88-133">Všimněte si, že předchozí kód hello při vytváření hello **dotazu** objektu, určuje maximální počet vrácených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="36f88-133">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="36f88-134">Hello **dotazu** objekt obsahuje **HasMoreResults** vlastnost typu boolean, které můžete použít tooinvoke hello **GetNextAsTwinAsync** metody tooretrieve více než jednou. všechny výsledky.</span><span class="sxs-lookup"><span data-stu-id="36f88-134">hello **query** object contains a **HasMoreResults** boolean property that you can use tooinvoke hello **GetNextAsTwinAsync** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="36f88-135">Volána metoda **GetNextAsJson** je k dispozici pro výsledky, které není dvojčata zařízení, například výsledky dotazů agregace.</span><span class="sxs-lookup"><span data-stu-id="36f88-135">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="36f88-136">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="36f88-136">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="36f88-137">V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **AddTagsAndQuery** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="36f88-137">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="36f88-138">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-138">Build hello solution.</span></span>
1. <span data-ttu-id="36f88-139">Tuto aplikaci spustit kliknutím pravým tlačítkem na hello **AddTagsAndQuery** projekt a výběrem **ladění**, za nímž následují **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="36f88-139">Run this application by right-clicking on hello **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="36f88-140">Měli byste vidět jeden zařízení ve výsledcích hello hello dotazu žádostí pro všechna zařízení umístěné v **Redmond43** a jeden pro hello dotaz, který omezuje hello výsledků toodevices, použít mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="36f88-140">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![Výsledky dotazu v okně][img-addtagapp]

<span data-ttu-id="36f88-142">V další části hello můžete vytvořit aplikaci zařízení, která hlásí informace o připojení k hello a změny hello výsledek dotazu hello v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-142">In hello next section, you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="36f88-143">Vytvoření aplikace hello zařízení</span><span class="sxs-lookup"><span data-stu-id="36f88-143">Create hello device app</span></span>
<span data-ttu-id="36f88-144">V této části vytvoříte konzolovou aplikaci .NET, která připojí tooyour hub jako **myDeviceId**a pak aktualizuje jeho informace hello hlášené vlastnosti toocontain že je připojený používá mobilní síť.</span><span class="sxs-lookup"><span data-stu-id="36f88-144">In this section, you create a .NET console app that connects tooyour hub as **myDeviceId**, and then updates its reported properties toocontain hello information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="36f88-145">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="36f88-145">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="36f88-146">Název projektu hello **ReportConnectivity**.</span><span class="sxs-lookup"><span data-stu-id="36f88-146">Name hello project **ReportConnectivity**.</span></span>
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]
    
1. <span data-ttu-id="36f88-148">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ReportConnectivity** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="36f88-148">In Solution Explorer, right-click hello **ReportConnectivity** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="36f88-149">V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="36f88-149">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="36f88-150">Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-150">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="36f88-151">Tento postup stáhne, nainstaluje a přidá odkaz toohello [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="36f88-151">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. <span data-ttu-id="36f88-153">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="36f88-153">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="36f88-154">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="36f88-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="36f88-155">Nahraďte hodnotu zástupného symbolu hello hello zařízení připojovací řetězec, který jste si poznamenali v předchozím oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-155">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="36f88-156">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="36f88-156">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="36f88-157">Hello **klienta** objekt poskytuje všechny metody hello vyžadují toointeract s dvojčata zařízení z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="36f88-157">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="36f88-158">Hello výše, uvedeném kódu inicializuje hello **klienta** objektu a potom načte hello dvojče zařízení pro **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="36f88-158">hello code shown above, initializes hello **Client** object, and then retrieves hello device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="36f88-159">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="36f88-159">Add hello following method toohello **Program** class:</span></span>
   
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

   <span data-ttu-id="36f88-160">Hello kód výše aktualizace **myDeviceId**je hlášené vlastnost s informace o připojení k hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-160">hello code above updates **myDeviceId**'s reported property with hello connectivity information.</span></span>

1. <span data-ttu-id="36f88-161">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="36f88-161">Finally, add hello following lines toohello **Main** method:</span></span>
   
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

1. <span data-ttu-id="36f88-162">V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **ReportConnectivity** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="36f88-162">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **ReportConnectivity** project is **Start**.</span></span> <span data-ttu-id="36f88-163">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-163">Build hello solution.</span></span>
1. <span data-ttu-id="36f88-164">Tuto aplikaci spustit kliknutím pravým tlačítkem na hello **ReportConnectivity** projekt a výběrem **ladění**, za nímž následují **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="36f88-164">Run this application by right-clicking on hello **ReportConnectivity** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="36f88-165">Měli vidět, získávání hello twin informace a pak odešle připojení jako *hlášené vlastnost*.</span><span class="sxs-lookup"><span data-stu-id="36f88-165">You should see it getting hello twin information, and then sending connectivity as a *reported property*.</span></span>
   
    ![Spustit připojení tooreport aplikace zařízení][img-rundeviceapp]
    
    
1. <span data-ttu-id="36f88-167">Teď, když hello zařízení hlásí informace o jeho připojení k, mělo by se zobrazit v obou dotazy.</span><span class="sxs-lookup"><span data-stu-id="36f88-167">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="36f88-168">Spuštění rozhraní .NET hello **AddTagsAndQuery** hello toorun aplikace dotazuje znovu.</span><span class="sxs-lookup"><span data-stu-id="36f88-168">Run hello .NET **AddTagsAndQuery** app toorun hello queries again.</span></span> <span data-ttu-id="36f88-169">Tentokrát **myDeviceId** by se měla objevit v obou výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="36f88-169">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![Připojení zařízení úspěšně hlášené][img-tagappsuccess]

## <a name="next-steps"></a><span data-ttu-id="36f88-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36f88-171">Next steps</span></span>
<span data-ttu-id="36f88-172">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-172">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="36f88-173">Přidat zařízení metadat jako značky z back-end aplikace a napsali informace připojení k zařízení simulovaného zařízení aplikaci tooreport v dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="36f88-173">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="36f88-174">Také jste se naučili jak tooquery tyto informace pomocí dotazovacího jazyka pro hello SQL jako IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="36f88-174">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="36f88-175">Použití hello následující toolearn prostředky jak pro:</span><span class="sxs-lookup"><span data-stu-id="36f88-175">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="36f88-176">odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="36f88-176">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="36f88-177">Konfigurace zařízení požadované vlastnosti dvojče zařízení pomocí hello [použití požadovaného vlastnosti tooconfigure zařízení] [ lnk-twin-how-to-configure] kurzu</span><span class="sxs-lookup"><span data-stu-id="36f88-177">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="36f88-178">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="36f88-178">control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

