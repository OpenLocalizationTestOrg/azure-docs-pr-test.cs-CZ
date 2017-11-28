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
# <a name="use-desired-properties-tooconfigure-devices"></a><span data-ttu-id="f880a-104">Použití zařízení tooconfigure požadované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f880a-104">Use desired properties tooconfigure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="f880a-105">Na konci hello tohoto kurzu budete mít dvě aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="f880a-105">At hello end of this tutorial, you will have two .NET console apps:</span></span>

* <span data-ttu-id="f880a-106">**SimulateDeviceConfiguration**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a sestavy hello stav procesu aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f880a-106">**SimulateDeviceConfiguration**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="f880a-107">**SetDesiredConfigurationAndQuery**, back-end aplikace, která nastaví hello požadovaných konfigurací na zařízení a dotazů hello proces aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f880a-107">**SetDesiredConfigurationAndQuery**, a back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="f880a-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="f880a-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="f880a-109">toocomplete tohoto kurzu potřebujete následující hello:</span><span class="sxs-lookup"><span data-stu-id="f880a-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="f880a-110">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="f880a-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="f880a-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f880a-111">An active Azure account.</span></span> <span data-ttu-id="f880a-112">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="f880a-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="f880a-113">Pokud jste postupovali podle hello [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="f880a-113">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="f880a-114">V takovém případě můžete přeskočit toohello [aplikaci simulovaného zařízení vytvořit hello] [ lnk-how-to-configure-createapp] části.</span><span class="sxs-lookup"><span data-stu-id="f880a-114">In that case, you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="f880a-115">Vytvoření aplikace simulovaného zařízení hello</span><span class="sxs-lookup"><span data-stu-id="f880a-115">Create hello simulated device app</span></span>
<span data-ttu-id="f880a-116">V této části vytvoříte konzolovou aplikaci .NET, která připojí tooyour hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a v procesu aktualizace konfigurace hello simulated hlásí aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f880a-116">In this section, you create a .NET console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="f880a-117">V sadě Visual Studio vytvořte nový projekt Visual C# Windows klasický desktopový pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="f880a-117">In Visual Studio, create a new Visual C# Windows Classic Desktop project by using hello **Console Application** project template.</span></span> <span data-ttu-id="f880a-118">Název projektu hello **SimulateDeviceConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="f880a-118">Name hello project **SimulateDeviceConfiguration**.</span></span>
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]

1. <span data-ttu-id="f880a-120">V Průzkumníku řešení klikněte pravým tlačítkem na hello **SimulateDeviceConfiguration** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="f880a-120">In Solution Explorer, right-click hello **SimulateDeviceConfiguration** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="f880a-121">V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="f880a-121">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="f880a-122">Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-122">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="f880a-123">Tento postup stáhne, nainstaluje a přidá odkaz toohello [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="f880a-123">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. <span data-ttu-id="f880a-125">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="f880a-125">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="f880a-126">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="f880a-126">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="f880a-127">Nahraďte hodnotu zástupného symbolu hello hello zařízení připojovací řetězec, který jste si poznamenali v předchozím oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-127">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. <span data-ttu-id="f880a-128">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="f880a-128">Add hello following method toohello **Program** class:</span></span>
 
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
    <span data-ttu-id="f880a-129">Hello **klienta** objekt poskytuje všechny metody hello vyžadují toointeract s dvojčata zařízení z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="f880a-129">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="f880a-130">Hello výše, uvedeném kódu inicializuje hello **klienta** objektu a potom načte hello dvojče zařízení pro **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="f880a-130">hello code shown above, initializes hello **Client** object, and then retrieves hello device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="f880a-131">Přidejte následující metodu toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="f880a-131">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="f880a-132">Tato metoda nastaví počáteční hodnoty hello telemetrie na místním zařízení hello a poté aktualizace hello dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="f880a-132">This method sets hello initial values of telemetry on hello local device and then updates hello device twin.</span></span>

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

1. <span data-ttu-id="f880a-133">Přidejte následující metodu toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="f880a-133">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="f880a-134">Toto je zpětného volání, která zjistí změnu *potřeby vlastnosti* v dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-134">This is a callback which will detect a change in *desired properties* in hello device twin.</span></span>

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

    <span data-ttu-id="f880a-135">Tato metoda aktualizace hello hlášené vlastnosti objektu twin hello místní zařízení s konfigurací hello aktualizovat požadavku a nastaví stav hello příliš**čekající**, pak aktualizací hello dvojče zařízení ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-135">This method updates hello reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="f880a-136">Po úspěšné aktualizaci dvojče zařízení hello, dokončení změně konfigurace hello voláním metody hello `CompleteConfigChange` popsané v další bod hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-136">After successfully updating hello device twin, it completes hello config change by calling hello method `CompleteConfigChange` described in hello next point.</span></span>

1. <span data-ttu-id="f880a-137">Přidejte následující metodu toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="f880a-137">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="f880a-138">Tato metoda simuluje resetování zařízení a potom aktualizace hello místní hlášené vlastnosti nastavení stavu hello příliš**úspěch** a odebere hello **pendingConfig** element.</span><span class="sxs-lookup"><span data-stu-id="f880a-138">This method simulates a device reset, then updates hello local reported properties setting hello status too**Success** and removes hello **pendingConfig** element.</span></span> <span data-ttu-id="f880a-139">Pak aktualizuje hello dvojče zařízení ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-139">It then updates hello device twin on hello service.</span></span> 

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

1. <span data-ttu-id="f880a-140">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="f880a-140">Finally add hello following lines toohello **Main** method:</span></span>

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
   > <span data-ttu-id="f880a-141">V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f880a-141">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="f880a-142">Některé procesy aktualizace konfigurace může být schopný tooaccommodate změn konfigurace cílového spuštěného hello aktualizace, některé může mít tooqueue je a některé by mohly odmítněte s chybový stav.</span><span class="sxs-lookup"><span data-stu-id="f880a-142">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, some might have tooqueue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="f880a-143">Ujistěte se, že tooconsider hello požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku hello před zahájením hello změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f880a-143">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="f880a-144">Vytvoření hello řešení a pak spusťte aplikaci zařízení hello ze sady Visual Studio kliknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="f880a-144">Build hello solution and then run hello device app from Visual Studio by clicking **F5**.</span></span> <span data-ttu-id="f880a-145">V konzole výstup hello měli byste vidět zprávy hello indikující, že je načítání hello dvojče zařízení simulovaného zařízení, nastavení hello telemetrie a čekání na změnu požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f880a-145">On hello output console, you should see hello messages indicating that your simulated device is retrieving hello device twin, setting up hello telemetry, and waiting for desired property change.</span></span> <span data-ttu-id="f880a-146">Zachovat hello aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="f880a-146">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="f880a-147">Vytvoření aplikace hello service</span><span class="sxs-lookup"><span data-stu-id="f880a-147">Create hello service app</span></span>
<span data-ttu-id="f880a-148">V této části vytvoříte konzolovou aplikaci .NET této aktualizace hello *potřeby vlastnosti* na dvojče zařízení hello přidružené **myDeviceId** s nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="f880a-148">In this section, you will create a .NET console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="f880a-149">Pak dotazuje dvojčata zařízení hello uložené ve hello IoT hub a ukazuje hello rozdíl mezi hello potřeby a který ohlásil konfigurace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="f880a-149">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="f880a-150">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="f880a-150">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="f880a-151">Název projektu hello **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="f880a-151">Name hello project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. <span data-ttu-id="f880a-153">V Průzkumníku řešení klikněte pravým tlačítkem na hello **SetDesiredConfigurationAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="f880a-153">In Solution Explorer, right-click hello **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="f880a-154">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-154">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="f880a-155">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="f880a-155">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="f880a-157">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="f880a-157">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="f880a-158">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="f880a-158">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="f880a-159">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-159">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="f880a-160">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="f880a-160">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="f880a-161">Hello **registru** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-161">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="f880a-162">Tento kód inicializuje hello **registru** objektu, načte hello dvojče zařízení pro **myDeviceId**a pak aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="f880a-162">This code initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="f880a-163">Poté vyžádá si dvojčata zařízení hello uložené ve hello IoT hub každých 10 sekund a výtisků hello potřeby a hlášené telemetrie konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f880a-163">After that, it queries hello device twins stored in hello IoT hub every 10 seconds, and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="f880a-164">Odkazovat toohello [IoT Hub dotazovací jazyk] [ lnk-query] toolearn jak toogenerate bohaté sestavy na všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="f880a-164">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="f880a-165">Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="f880a-165">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="f880a-166">Použití dotazuje sestavy zobrazující se uživatelům toogenerate napříč mnoho zařízení a ne toodetect změny.</span><span class="sxs-lookup"><span data-stu-id="f880a-166">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="f880a-167">Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="f880a-167">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="f880a-168">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="f880a-168">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. <span data-ttu-id="f880a-169">V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **SetDesiredConfigurationAndQuery** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="f880a-169">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="f880a-170">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-170">Build hello solution.</span></span>
1. <span data-ttu-id="f880a-171">S **SimulateDeviceConfiguration** zařízení aplikace spuštěná, spusťte hello aplikace service pomocí sady Visual Studio **F5**.</span><span class="sxs-lookup"><span data-stu-id="f880a-171">With **SimulateDeviceConfiguration** device app running, run hello service app from Visual Studio using **F5**.</span></span> <span data-ttu-id="f880a-172">Měli byste vidět hello změnu z hlášené konfigurace **čekající** příliš**úspěch** s nové aktivní hello odeslat frekvenci pět minut místo 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="f880a-172">You should see hello reported configuration change from **Pending** too**Success** with hello new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Zařízení byl úspěšně nakonfigurován][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="f880a-174">Není zpoždění až minutu tooa mezi hello zařízení sestavy operace a výsledek dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="f880a-174">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="f880a-175">Toto je tooenable hello dotazu infrastruktury toowork ve velmi velkém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="f880a-175">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="f880a-176">tooretrieve konzistentní zobrazení twin jedno zařízení použít hello **getDeviceTwin** metoda v hello **registru** třídy.</span><span class="sxs-lookup"><span data-stu-id="f880a-176">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="f880a-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f880a-177">Next steps</span></span>
<span data-ttu-id="f880a-178">V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z řešení hello back-end a napsali toodetect aplikace zařízení, která změnit a simulace aktualizace vícekrokový proces reporting její stav prostřednictvím hello hlášené Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f880a-178">In this tutorial, you set a desired configuration as *desired properties* from hello solution back end, and wrote a device app toodetect that change and simulate a multi-step update process reporting its status through hello reported properties.</span></span>

<span data-ttu-id="f880a-179">Použití hello následující toolearn prostředky jak pro:</span><span class="sxs-lookup"><span data-stu-id="f880a-179">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="f880a-180">odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="f880a-180">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="f880a-181">naplánovat nebo provádět operace u velkých sad zařízení najdete v části hello [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="f880a-181">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="f880a-182">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="f880a-182">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
