---
title: "Použití Azure IoT Hub zařízení dvojici vlastností (.NET/.NET) | Microsoft Docs"
description: "Jak používat dvojčata zařízení Azure IoT Hub pro konfiguraci zařízení. Použití zařízení Azure IoT sady SDK pro .NET k implementaci aplikace simulovaného zařízení a sady SDK pro .NET k implementaci aplikační služby, která upraví konfiguraci zařízení dvojče zařízení pomocí služby Azure IoT."
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
ms.openlocfilehash: 679cda28bf3ce9fb207fe3693a3453b355f1de15
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="use-desired-properties-to-configure-devices"></a><span data-ttu-id="94036-104">Použijte požadované vlastnosti pro konfiguraci zařízení</span><span class="sxs-lookup"><span data-stu-id="94036-104">Use desired properties to configure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="94036-105">Na konci tohoto kurzu budete mít dvě aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="94036-105">At the end of this tutorial, you will have two .NET console apps:</span></span>

* <span data-ttu-id="94036-106">**SimulateDeviceConfiguration**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a oznámí stav procesu aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94036-106">**SimulateDeviceConfiguration**, a simulated device app that waits for a desired configuration update and reports the status of a simulated configuration update process.</span></span>
* <span data-ttu-id="94036-107">**SetDesiredConfigurationAndQuery**, back-end aplikace, která na zařízení nastaví požadované konfigurace a dotazuje proces aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94036-107">**SetDesiredConfigurationAndQuery**, a back-end app, which sets the desired configuration on a device and queries the configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="94036-108">Článek [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.</span><span class="sxs-lookup"><span data-stu-id="94036-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="94036-109">K dokončení tohoto kurzu budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="94036-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="94036-110">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="94036-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="94036-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="94036-111">An active Azure account.</span></span> <span data-ttu-id="94036-112">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="94036-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="94036-113">Pokud jste postupovali podle [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="94036-113">If you followed the [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="94036-114">V takovém případě můžete přeskočit na [vytvoření aplikace simulovaného zařízení] [ lnk-how-to-configure-createapp] části.</span><span class="sxs-lookup"><span data-stu-id="94036-114">In that case, you can skip to the [Create the simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a><span data-ttu-id="94036-115">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="94036-115">Create the simulated device app</span></span>
<span data-ttu-id="94036-116">V této části vytvoříte konzolovou aplikaci .NET, která se připojuje k vaší hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a hlásí aktualizace na proces aktualizace simulované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94036-116">In this section, you create a .NET console app that connects to your hub as **myDeviceId**, waits for a desired configuration update and then reports updates on the simulated configuration update process.</span></span>

1. <span data-ttu-id="94036-117">V sadě Visual Studio vytvořte nový projekt Visual C# Windows klasický desktopový pomocí **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="94036-117">In Visual Studio, create a new Visual C# Windows Classic Desktop project by using the **Console Application** project template.</span></span> <span data-ttu-id="94036-118">Název projektu **SimulateDeviceConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="94036-118">Name the project **SimulateDeviceConfiguration**.</span></span>
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]

1. <span data-ttu-id="94036-120">V Průzkumníku řešení klikněte pravým tlačítkem myši **SimulateDeviceConfiguration** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="94036-120">In Solution Explorer, right-click the **SimulateDeviceConfiguration** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="94036-121">V **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="94036-121">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="94036-122">Vyberte **nainstalovat** k instalaci **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="94036-122">Select **Install** to install the **Microsoft.Azure.Devices.Client** package, and accept the terms of use.</span></span> <span data-ttu-id="94036-123">Tento postup stáhne, nainstaluje a přidá odkaz na [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="94036-123">This procedure downloads, installs, and adds a reference to the [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. <span data-ttu-id="94036-125">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="94036-125">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="94036-126">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="94036-126">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="94036-127">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem zařízení, kterou jste si poznamenali v předchozím oddílu.</span><span class="sxs-lookup"><span data-stu-id="94036-127">Replace the placeholder value with the device connection string that you noted in the previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. <span data-ttu-id="94036-128">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="94036-128">Add the following method to the **Program** class:</span></span>
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    <span data-ttu-id="94036-129">**Klienta** objekt poskytuje všechny metody vyžadovat interakci s dvojčata zařízení ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="94036-129">The **Client** object exposes all the methods you require to interact with device twins from the device.</span></span> <span data-ttu-id="94036-130">Inicializuje výše uvedeném kódu **klienta** objektu a potom načte dvojče zařízení pro **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="94036-130">The code shown above, initializes the **Client** object, and then retrieves the device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="94036-131">Přidejte následující metodu do **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="94036-131">Add the following method to the **Program** class.</span></span> <span data-ttu-id="94036-132">Tato metoda nastaví počáteční hodnoty telemetrie na místním zařízení a pak aktualizuje dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="94036-132">This method sets the initial values of telemetry on the local device and then updates the device twin.</span></span>

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

1. <span data-ttu-id="94036-133">Přidejte následující metodu do **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="94036-133">Add the following method to the **Program** class.</span></span> <span data-ttu-id="94036-134">Toto je zpětného volání, která zjistí změnu *potřeby vlastnosti* v dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="94036-134">This is a callback which will detect a change in *desired properties* in the device twin.</span></span>

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

    <span data-ttu-id="94036-135">Tato metoda aktualizuje hlášené vlastnosti v objektu twin místní zařízení žádost o aktualizaci konfigurace a nastaví stav na **čekající**, pak aktualizuje dvojče zařízení ve službě.</span><span class="sxs-lookup"><span data-stu-id="94036-135">This method updates the reported properties on the local device twin object with the configuration update request and sets the status to **Pending**, then updates the device twin on the service.</span></span> <span data-ttu-id="94036-136">Po úspěšné aktualizaci dvojče zařízení, dokončení změny konfigurace pomocí volání metody `CompleteConfigChange` popsané v další bod.</span><span class="sxs-lookup"><span data-stu-id="94036-136">After successfully updating the device twin, it completes the config change by calling the method `CompleteConfigChange` described in the next point.</span></span>

1. <span data-ttu-id="94036-137">Přidejte následující metodu do **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="94036-137">Add the following method to the **Program** class.</span></span> <span data-ttu-id="94036-138">Tato metoda simuluje resetování zařízení a potom aktualizace místní hlášené vlastnosti nastavení stavu na **úspěch** a odebere **pendingConfig** element.</span><span class="sxs-lookup"><span data-stu-id="94036-138">This method simulates a device reset, then updates the local reported properties setting the status to **Success** and removes the **pendingConfig** element.</span></span> <span data-ttu-id="94036-139">Pak aktualizuje dvojče zařízení ve službě.</span><span class="sxs-lookup"><span data-stu-id="94036-139">It then updates the device twin on the service.</span></span> 

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
                Console.WriteLine("Config change complete \nPress any key to exit.");
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

1. <span data-ttu-id="94036-140">Nakonec přidejte následující řádky, které se **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="94036-140">Finally add the following lines to the **Main** method:</span></span>

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
   > <span data-ttu-id="94036-141">V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94036-141">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="94036-142">Některé procesy aktualizace konfigurace může být schopna přijmout změny konfigurace cílového aktualizace běží, některé může mít do fronty je a některé může odmítnout s chybový stav.</span><span class="sxs-lookup"><span data-stu-id="94036-142">Some configuration update processes might be able to accommodate changes of target configuration while the update is running, some might have to queue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="94036-143">Zajistěte, aby vzít v úvahu požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku před zahájením změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94036-143">Make sure to consider the desired behavior for your specific configuration process, and add the appropriate logic before initiating the configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="94036-144">Sestavte řešení a pak spusťte aplikaci zařízení ze sady Visual Studio kliknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="94036-144">Build the solution and then run the device app from Visual Studio by clicking **F5**.</span></span> <span data-ttu-id="94036-145">V konzole pro výstup měli byste vidět zprávy indikující, že simulovaného zařízení načítá dvojče zařízení, nastavení telemetrie a čekání na změnu požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="94036-145">On the output console, you should see the messages indicating that your simulated device is retrieving the device twin, setting up the telemetry, and waiting for desired property change.</span></span> <span data-ttu-id="94036-146">Nechte aplikaci spuštěnou.</span><span class="sxs-lookup"><span data-stu-id="94036-146">Keep the app running.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="94036-147">Vytvořit aplikaci aplikační služby</span><span class="sxs-lookup"><span data-stu-id="94036-147">Create the service app</span></span>
<span data-ttu-id="94036-148">V této části vytvoříte konzolovou aplikaci .NET, která aktualizuje *potřeby vlastnosti* na dvojče zařízení spojené s **myDeviceId** s nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="94036-148">In this section, you will create a .NET console app that updates the *desired properties* on the device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="94036-149">Pak dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub a ukazuje rozdíl mezi požadované a oznámená konfigurace zařízení.</span><span class="sxs-lookup"><span data-stu-id="94036-149">It then queries the device twins stored in the IoT hub and shows the difference between the desired and reported configurations of the device.</span></span>

1. <span data-ttu-id="94036-150">V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="94036-150">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="94036-151">Název projektu **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="94036-151">Name the project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]
1. <span data-ttu-id="94036-153">V Průzkumníku řešení klikněte pravým tlačítkem myši **SetDesiredConfigurationAndQuery** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="94036-153">In Solution Explorer, right-click the **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="94036-154">V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání.</span><span class="sxs-lookup"><span data-stu-id="94036-154">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="94036-155">Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="94036-155">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="94036-157">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="94036-157">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="94036-158">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="94036-158">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="94036-159">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="94036-159">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="94036-160">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="94036-160">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="94036-161">**Registru** objekt poskytuje všechny metody požadované pro interakci s dvojčata zařízení ze služby.</span><span class="sxs-lookup"><span data-stu-id="94036-161">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="94036-162">Tento kód inicializuje **registru** objektu, načte dvojče zařízení pro **myDeviceId**a pak aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie.</span><span class="sxs-lookup"><span data-stu-id="94036-162">This code initializes the **Registry** object, retrieves the device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="94036-163">Poté se dotazuje dvojčata zařízení, které jsou uložené ve službě IoT hub každých 10 sekund a vytiskne konfigurace požadované a oznámená telemetrie.</span><span class="sxs-lookup"><span data-stu-id="94036-163">After that, it queries the device twins stored in the IoT hub every 10 seconds, and prints the desired and reported telemetry configurations.</span></span> <span data-ttu-id="94036-164">Odkazovat [IoT Hub dotazovací jazyk] [ lnk-query] se dozvíte, jak chcete generovat sestavy o bohaté na všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="94036-164">Refer to the [IoT Hub query language][lnk-query] to learn how to generate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="94036-165">Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="94036-165">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="94036-166">Použijte dotazy na generování sestavy zobrazující se uživatelům prostřednictvím zařízení a ne ke zjištění změny.</span><span class="sxs-lookup"><span data-stu-id="94036-166">Use queries to generate user-facing reports across many devices, and not to detect changes.</span></span> <span data-ttu-id="94036-167">Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="94036-167">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="94036-168">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="94036-168">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. <span data-ttu-id="94036-169">V Průzkumníku řešení otevřete **nastavit projekty po spuštění...**  a zajistěte, aby **akce** pro **SetDesiredConfigurationAndQuery** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="94036-169">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="94036-170">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="94036-170">Build the solution.</span></span>
1. <span data-ttu-id="94036-171">S **SimulateDeviceConfiguration** zařízení aplikace spuštěná, spusťte aplikaci service pomocí sady Visual Studio **F5**.</span><span class="sxs-lookup"><span data-stu-id="94036-171">With **SimulateDeviceConfiguration** device app running, run the service app from Visual Studio using **F5**.</span></span> <span data-ttu-id="94036-172">Měli byste vidět hlášené změnu z konfigurace **čekající** k **úspěch** s nové aktivní odeslat frekvenci pět minut místo 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="94036-172">You should see the reported configuration change from **Pending** to **Success** with the new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Zařízení byl úspěšně nakonfigurován][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="94036-174">Není zpoždění až několik minut mezi operaci sestavy zařízení a výsledek dotazu.</span><span class="sxs-lookup"><span data-stu-id="94036-174">There is a delay of up to a minute between the device report operation and the query result.</span></span> <span data-ttu-id="94036-175">To je umožnit dotazu infrastruktury pro práci na velmi velký rozsah.</span><span class="sxs-lookup"><span data-stu-id="94036-175">This is to enable the query infrastructure to work at very high scale.</span></span> <span data-ttu-id="94036-176">Načtení konzistentní zobrazení používají twin jedno zařízení **getDeviceTwin** metoda v **registru** třídy.</span><span class="sxs-lookup"><span data-stu-id="94036-176">To retrieve consistent views of a single device twin use the **getDeviceTwin** method in the **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="94036-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94036-177">Next steps</span></span>
<span data-ttu-id="94036-178">V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z řešení back-end a napsali aplikace na zařízení ke zjištění této změny a simulovat aktualizace vícekrokový proces reporting její stav prostřednictvím hlášení Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="94036-178">In this tutorial, you set a desired configuration as *desired properties* from the solution back end, and wrote a device app to detect that change and simulate a multi-step update process reporting its status through the reported properties.</span></span>

<span data-ttu-id="94036-179">Použijte v následujících zdrojích informací další postup:</span><span class="sxs-lookup"><span data-stu-id="94036-179">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="94036-180">odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="94036-180">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="94036-181">naplánovat nebo udělat operace u velkých sad zařízení najdete [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="94036-181">schedule or perform operations on large sets of devices see the [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="94036-182">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="94036-182">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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
