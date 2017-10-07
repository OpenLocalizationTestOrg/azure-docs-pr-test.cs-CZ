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
# <a name="use-direct-methods-netnet"></a><span data-ttu-id="0e13a-104">Používat přímé metody (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="0e13a-104">Use direct methods (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="0e13a-105">V tomto kurzu jsme jsou probíhající toodevelop dva konzoly aplikace .NET:</span><span class="sxs-lookup"><span data-stu-id="0e13a-105">In this tutorial, we are going toodevelop two .NET console apps:</span></span>

* <span data-ttu-id="0e13a-106">**CallMethodOnDevice**, back-end aplikace, která volá metodu v aplikaci simulovaného zařízení hello a zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0e13a-106">**CallMethodOnDevice**, a back-end app, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="0e13a-107">**SimulateDeviceMethods**, konzolovou aplikaci, která simuluje zařízení připojení tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odpoví toohello metodu s názvem hello cloudem.</span><span class="sxs-lookup"><span data-stu-id="0e13a-107">**SimulateDeviceMethods**, a console app which simulates a device connecting tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="0e13a-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="0e13a-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="0e13a-109">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="0e13a-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="0e13a-110">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0e13a-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="0e13a-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="0e13a-111">An active Azure account.</span></span> <span data-ttu-id="0e13a-112">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="0e13a-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="0e13a-113">Pokud chcete identitu zařízení hello toocreate prostřednictvím kódu programu místo, najdete v hello hello odpovídající části [připojení simulovaného zařízení tooyour IoT hub pomocí rozhraní .NET] [ lnk-device-identity-csharp] článku.</span><span class="sxs-lookup"><span data-stu-id="0e13a-113">If you want toocreate hello device identity programmatically instead, read hello corresponding section in hello [Connect your simulated device tooyour IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>


## <a name="create-a-simulated-device-app"></a><span data-ttu-id="0e13a-114">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="0e13a-114">Create a simulated device app</span></span>
<span data-ttu-id="0e13a-115">V této části vytvoříte konzolovou aplikaci .NET, která odpovídá tooa metodu s názvem řešení hello zpět end.</span><span class="sxs-lookup"><span data-stu-id="0e13a-115">In this section, you create a .NET console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="0e13a-116">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="0e13a-116">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="0e13a-117">Název projektu hello **SimulateDeviceMethods**.</span><span class="sxs-lookup"><span data-stu-id="0e13a-117">Name hello project **SimulateDeviceMethods**.</span></span>
   
    ![Novou aplikaci Visual C# klasické zařízení][img-createdeviceapp]
    
1. <span data-ttu-id="0e13a-119">V Průzkumníku řešení klikněte pravým tlačítkem na hello **SimulateDeviceMethods** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="0e13a-119">In Solution Explorer, right-click hello **SimulateDeviceMethods** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="0e13a-120">V hello **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="0e13a-120">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="0e13a-121">Vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-121">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="0e13a-122">Tento postup stáhne, nainstaluje a přidá odkaz toohello [zařízení Azure IoT SDK] [ lnk-nuget-client-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="0e13a-122">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![Správce balíčků NuGet okno klientské aplikace][img-clientnuget]
1. <span data-ttu-id="0e13a-124">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="0e13a-124">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. <span data-ttu-id="0e13a-125">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="0e13a-125">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="0e13a-126">Nahraďte hodnotu zástupného symbolu hello hello zařízení připojovací řetězec, který jste si poznamenali v předchozím oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-126">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="0e13a-127">Přidejte následující metodu přímé hello tooimplement na zařízení hello hello:</span><span class="sxs-lookup"><span data-stu-id="0e13a-127">Add hello following tooimplement hello direct method on hello device:</span></span>

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written toolog.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. <span data-ttu-id="0e13a-128">Nakonec přidejte následující kód toohello hello **hlavní** metoda tooopen hello připojení tooyour IoT hub a inicializovat hello metoda naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="0e13a-128">Finally, add hello following code toohello **Main** method tooopen hello connection tooyour IoT hub and initialize hello method listener:</span></span>
   
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
        
1. <span data-ttu-id="0e13a-129">V hello Průzkumníka řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a potom vyberte hello **SimulateDeviceMethods** projekt v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-129">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **SimulateDeviceMethods** project in hello dropdown menu.</span></span>        

> [!NOTE]
> <span data-ttu-id="0e13a-130">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="0e13a-130">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="0e13a-131">V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="0e13a-131">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="0e13a-132">Volání metody přímé na zařízení</span><span class="sxs-lookup"><span data-stu-id="0e13a-132">Call a direct method on a device</span></span>
<span data-ttu-id="0e13a-133">V této části vytvoříte konzolovou aplikaci .NET, která volá metodu v aplikaci hello simulované zařízení a potom zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0e13a-133">In this section, you create a .NET console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="0e13a-134">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="0e13a-134">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="0e13a-135">Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0e13a-135">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="0e13a-136">Název projektu hello **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="0e13a-136">Name hello project **CallMethodOnDevice**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createserviceapp]
2. <span data-ttu-id="0e13a-138">V Průzkumníku řešení klikněte pravým tlačítkem na hello **CallMethodOnDevice** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="0e13a-138">In Solution Explorer, right-click hello **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="0e13a-139">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-139">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="0e13a-140">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="0e13a-140">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][img-servicenuget]

4. <span data-ttu-id="0e13a-142">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="0e13a-142">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="0e13a-143">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="0e13a-143">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="0e13a-144">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-144">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="0e13a-145">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="0e13a-145">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="0e13a-146">Tato metoda volá metodu přímé s názvem `writeLine` na hello `myDeviceId` zařízení.</span><span class="sxs-lookup"><span data-stu-id="0e13a-146">This method invokes a direct method with name `writeLine` on hello `myDeviceId` device.</span></span> <span data-ttu-id="0e13a-147">Pak zapíše odpověď hello poskytované hello zařízení v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-147">Then, it writes hello response provided by hello device on hello console.</span></span> <span data-ttu-id="0e13a-148">Všimněte si, jak je možné toospecify hodnotu časového limitu pro toorespond hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="0e13a-148">Note how it is possible toospecify a timeout value for hello device toorespond.</span></span>
7. <span data-ttu-id="0e13a-149">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="0e13a-149">Finally, add hello following lines toohello **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="0e13a-150">V hello Průzkumníka řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a potom vyberte hello **CallMethodOnDevice** projekt v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-150">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **CallMethodOnDevice** project in hello dropdown menu.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="0e13a-151">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="0e13a-151">Run hello applications</span></span>
<span data-ttu-id="0e13a-152">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e13a-152">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="0e13a-153">Spuštění aplikace zařízení .NET hello **SimulateDeviceMethods**.</span><span class="sxs-lookup"><span data-stu-id="0e13a-153">Run hello .NET device app **SimulateDeviceMethods**.</span></span> <span data-ttu-id="0e13a-154">By se měl spustit přijímá metoda volání ze služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="0e13a-154">It should start listening for method calls from your IoT Hub:</span></span> 

    ![Spusťte aplikaci zařízení][img-deviceapprun]
1. <span data-ttu-id="0e13a-156">Teď je hello zařízení připojeno a čekání volání metod, spustit hello .NET **CallMethodOnDevice** metody hello tooinvoke aplikace v aplikaci simulovaného zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-156">Now that hello device is connected and waiting for method invocations, run hello .NET **CallMethodOnDevice** app tooinvoke hello method in hello simulated device app.</span></span> <span data-ttu-id="0e13a-157">Měli byste vidět odpovědi zařízení hello napsané v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-157">You should see hello device response written in hello console.</span></span>
   
    ![Spusťte aplikaci aplikační služby][img-serviceapprun]
1. <span data-ttu-id="0e13a-159">Metoda toohello Hello zařízení pak reaguje tiskem tuto zprávu:</span><span class="sxs-lookup"><span data-stu-id="0e13a-159">hello device then reacts toohello method by printing this message:</span></span>
   
    ![Přímá metoda volána na zařízení hello][img-directmethodinvoked]

## <a name="next-steps"></a><span data-ttu-id="0e13a-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e13a-161">Next steps</span></span>
<span data-ttu-id="0e13a-162">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="0e13a-162">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="0e13a-163">Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci tooreact toomethods vyvolané hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="0e13a-163">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="0e13a-164">Můžete také vytvořit aplikaci, která volá metody na hello zařízení a zobrazí hello odezvu hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="0e13a-164">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="0e13a-165">toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:</span><span class="sxs-lookup"><span data-stu-id="0e13a-165">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="0e13a-166">[Začínáme s centrem IoT]</span><span class="sxs-lookup"><span data-stu-id="0e13a-166">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="0e13a-167">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="0e13a-167">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="0e13a-168">toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="0e13a-168">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
