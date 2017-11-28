---
title: "aaaUse Azure IoT Hub přímé metody (.NET nebo uzel) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub přímé metody. Použití zařízení Azure IoT hello SDK pro Node.js tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: nberdy
ms.openlocfilehash: f566f939be840eb308b00ffa4e05c4e5b3fefb39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnode"></a><span data-ttu-id="26084-104">Používat přímé metody (.NET nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="26084-104">Use direct methods (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="26084-105">V tomto kurzu budeme toodevelop .NET a konzolovou aplikaci softwaru Node.js:</span><span class="sxs-lookup"><span data-stu-id="26084-105">In this tutorial, we are going toodevelop a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="26084-106">**CallMethodOnDevice.sln**, back-end aplikace .NET, která volá metodu v aplikaci simulovaného zařízení hello a zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="26084-106">**CallMethodOnDevice.sln**, a .NET back-end app, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="26084-107">**SimulatedDevice.js**, aplikace Node.js, která simuluje zařízení připojení tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odpoví toohello metodu s názvem hello cloudem.</span><span class="sxs-lookup"><span data-stu-id="26084-107">**SimulatedDevice.js**, a Node.js app, which simulates a device connecting tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="26084-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="26084-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="26084-109">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="26084-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="26084-110">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="26084-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="26084-111">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="26084-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="26084-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="26084-112">An active Azure account.</span></span> <span data-ttu-id="26084-113">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="26084-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="26084-114">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="26084-114">Create a simulated device app</span></span>
<span data-ttu-id="26084-115">V této části vytvoříte konzolovou aplikaci Node.js, která odpovídá tooa metodu s názvem řešení hello zpět end.</span><span class="sxs-lookup"><span data-stu-id="26084-115">In this section, you create a Node.js console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="26084-116">Vytvořte novou prázdnou složku s názvem **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="26084-116">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="26084-117">V hello **simulateddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="26084-117">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="26084-118">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="26084-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="26084-119">Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** a **azure-iot zařízení mqtt** balíčky:</span><span class="sxs-lookup"><span data-stu-id="26084-119">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="26084-120">Pomocí textového editoru, vytvořte soubor v hello **simulateddevice** složku a pojmenujte ji **SimulatedDevice.js**.</span><span class="sxs-lookup"><span data-stu-id="26084-120">Using a text editor, create a file in hello **simulateddevice** folder and name it **SimulatedDevice.js**.</span></span>
4. <span data-ttu-id="26084-121">Přidejte následující hello `require` příkazy v hello začátek hello **SimulatedDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="26084-121">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="26084-122">Přidat **connectionString** proměnné a použít ho toocreate **DeviceClient** instance.</span><span class="sxs-lookup"><span data-stu-id="26084-122">Add a **connectionString** variable and use it toocreate a **DeviceClient** instance.</span></span> <span data-ttu-id="26084-123">Nahraďte **{zařízení připojovací řetězec}** s řetězcem připojení zařízení hello jste vygenerovali v hello *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="26084-123">Replace **{device connection string}** with hello device connection string you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="26084-124">Přidejte následující funkce tooimplement hello přímá metoda na zařízení hello hello:</span><span class="sxs-lookup"><span data-stu-id="26084-124">Add hello following function tooimplement hello direct method on hello device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="26084-125">Otevřete Centrum IoT tooyour hello připojení a inicializovat naslouchací proces metoda hello:</span><span class="sxs-lookup"><span data-stu-id="26084-125">Open hello connection tooyour IoT hub and initialize hello method listener:</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. <span data-ttu-id="26084-126">Uložte a zavřete hello **SimulatedDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="26084-126">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="26084-127">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="26084-127">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="26084-128">V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="26084-128">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="26084-129">Volání metody přímé na zařízení</span><span class="sxs-lookup"><span data-stu-id="26084-129">Call a direct method on a device</span></span>
<span data-ttu-id="26084-130">V této části vytvoříte konzolovou aplikaci .NET, která volá metodu v aplikaci hello simulované zařízení a potom zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="26084-130">In this section, you create a .NET console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="26084-131">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="26084-131">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="26084-132">Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="26084-132">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="26084-133">Název projektu hello **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="26084-133">Name hello project **CallMethodOnDevice**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. <span data-ttu-id="26084-135">V Průzkumníku řešení klikněte pravým tlačítkem na hello **CallMethodOnDevice** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="26084-135">In Solution Explorer, right-click hello **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="26084-136">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="26084-136">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="26084-137">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="26084-137">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][11]

4. <span data-ttu-id="26084-139">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="26084-139">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="26084-140">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="26084-140">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="26084-141">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="26084-141">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="26084-142">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="26084-142">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="26084-143">Tato metoda volá metodu přímé s názvem `writeLine` na hello `myDeviceId` zařízení.</span><span class="sxs-lookup"><span data-stu-id="26084-143">This method invokes a direct method with name `writeLine` on hello `myDeviceId` device.</span></span> <span data-ttu-id="26084-144">Pak zapíše odpověď hello poskytované hello zařízení v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="26084-144">Then, it writes hello response provided by hello device on hello console.</span></span> <span data-ttu-id="26084-145">Všimněte si, jak je možné toospecify hodnotu časového limitu pro toorespond hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="26084-145">Note how it is possible toospecify a timeout value for hello device toorespond.</span></span>
7. <span data-ttu-id="26084-146">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="26084-146">Finally, add hello following lines toohello **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

## <a name="run-hello-applications"></a><span data-ttu-id="26084-147">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="26084-147">Run hello applications</span></span>
<span data-ttu-id="26084-148">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="26084-148">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="26084-149">V hello Průzkumníka řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a potom vyberte hello **CallMethodOnDevice** projekt v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="26084-149">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **CallMethodOnDevice** project in hello dropdown menu.</span></span>

2. <span data-ttu-id="26084-150">Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz toostart přijímá metoda volání ze služby IoT Hub hello:</span><span class="sxs-lookup"><span data-stu-id="26084-150">At a command prompt in hello **simulateddevice** folder, run hello following command toostart listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   <span data-ttu-id="26084-151">Počkejte tooopen hello simulované zařízení:![][7]</span><span class="sxs-lookup"><span data-stu-id="26084-151">Wait for hello simulated device tooopen:  ![][7]</span></span>
3. <span data-ttu-id="26084-152">Teď je hello zařízení připojeno a čekání volání metod, spustit hello .NET **CallMethodOnDevice** metody hello tooinvoke aplikace v aplikaci simulovaného zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="26084-152">Now that hello device is connected and waiting for method invocations, run hello .NET **CallMethodOnDevice** app tooinvoke hello method in hello simulated device app.</span></span> <span data-ttu-id="26084-153">Měli byste vidět odpovědi zařízení hello napsané v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="26084-153">You should see hello device response written in hello console.</span></span>
   
    ![][8]
4. <span data-ttu-id="26084-154">Metoda toohello Hello zařízení pak reaguje tiskem tuto zprávu:</span><span class="sxs-lookup"><span data-stu-id="26084-154">hello device then reacts toohello method by printing this message:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="26084-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26084-155">Next steps</span></span>
<span data-ttu-id="26084-156">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="26084-156">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="26084-157">Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci tooreact toomethods vyvolané hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="26084-157">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="26084-158">Můžete také vytvořit aplikaci, která volá metody na hello zařízení a zobrazí hello odezvu hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="26084-158">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="26084-159">toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:</span><span class="sxs-lookup"><span data-stu-id="26084-159">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="26084-160">[Začínáme s centrem IoT]</span><span class="sxs-lookup"><span data-stu-id="26084-160">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="26084-161">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="26084-161">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="26084-162">toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="26084-162">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Začínáme s centrem IoT]: iot-hub-node-node-getstarted.md
