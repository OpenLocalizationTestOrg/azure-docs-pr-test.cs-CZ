---
title: "aktualizace firmwaru aaaDevice službou Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Jak aktualizovat toouse správou zařízení ve službě Azure IoT Hub tooinitiate firmwaru zařízení. Používáte zařízení Azure IoT hello SDK pro Node.js tooimplement aplikace simulovaného zařízení a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která spustí aktualizaci firmwaru hello."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/17/2017
ms.author: juanpere
ms.openlocfilehash: d48899651bcd5d67e577c971be0f9453c8f21592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-netnode"></a><span data-ttu-id="a5b53-104">Použití zařízení správu tooinitiate aktualizaci firmwaru zařízení (.NET nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="a5b53-104">Use device management tooinitiate a device firmware update (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="a5b53-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="a5b53-105">Introduction</span></span>
<span data-ttu-id="a5b53-106">V hello [Začínáme se správou zařízení] [ lnk-dm-getstarted] kurzu jste viděli, jak toouse hello [dvojče zařízení] [ lnk-devtwin] a [přímé metody] [ lnk-c2dmethod] tooremotely primitiv restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="a5b53-106">In hello [Get started with device management][lnk-dm-getstarted] tutorial, you saw how toouse hello [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives tooremotely reboot a device.</span></span> <span data-ttu-id="a5b53-107">Tento kurz používá hello stejné primitiv IoT Hub a popisuje, jak toodo začátku do konce simulated aktualizaci firmwaru.</span><span class="sxs-lookup"><span data-stu-id="a5b53-107">This tutorial uses hello same IoT Hub primitives and shows you how toodo an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="a5b53-108">Tento vzor se používá v implementaci aktualizace firmwaru hello pro hello [malin platformy zařízení implementace ukázka][lnk-rpi-implementation].</span><span class="sxs-lookup"><span data-stu-id="a5b53-108">This pattern is used in hello firmware update implementation for hello [Raspberry Pi device implementation sample][lnk-rpi-implementation].</span></span>

<span data-ttu-id="a5b53-109">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="a5b53-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="a5b53-110">Vytvoření konzolové aplikace .NET, která volá metodu přímé firmwareUpdate hello v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a5b53-110">Create a .NET console app that calls hello firmwareUpdate direct method in hello simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="a5b53-111">Vytvoření aplikace simulovaného zařízení, která implementuje **firmwareUpdate** přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="a5b53-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="a5b53-112">Tato metoda inicializuje více fáze procesu, který čeká toodownload hello firmware image, soubory ke stažení bitové kopie hello firmware a nakonec platí hello firmware image.</span><span class="sxs-lookup"><span data-stu-id="a5b53-112">This method initiates a multi-stage process that waits toodownload hello firmware image, downloads hello firmware image, and finally applies hello firmware image.</span></span> <span data-ttu-id="a5b53-113">Během každé fáze hello aktualizace hello zařízení používá hello ohlášeny průběh tooreport vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a5b53-113">During each stage of hello update, hello device uses hello reported properties tooreport on progress.</span></span>

<span data-ttu-id="a5b53-114">Na konci hello tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):</span><span class="sxs-lookup"><span data-stu-id="a5b53-114">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="a5b53-115">**dmpatterns_fwupdate_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení hello, zobrazí hello odpovědi a pravidelně (každých 500ms) zobrazí hello aktualizovat hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a5b53-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in hello simulated device app, displays hello response, and periodically (every 500ms) displays hello updated reported properties.</span></span>

<span data-ttu-id="a5b53-116">**TriggerFWUpdate**, který připojí tooyour IoT hub s dříve, vytvořenou identitou zařízení hello dostane přímá metoda firmwareUpdate, spustí prostřednictvím toosimulate více stavu procesu, včetně aktualizace firmwaru: Čekání na stažení bitové kopie hello Probíhá stahování hello novou bitovou kopii a bitovou kopii nakonec použití hello.</span><span class="sxs-lookup"><span data-stu-id="a5b53-116">**TriggerFWUpdate**, which connects tooyour IoT hub with hello device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process toosimulate a firmware update including: waiting for hello image download, downloading hello new image, and finally applying hello image.</span></span>

<span data-ttu-id="a5b53-117">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="a5b53-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="a5b53-118">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a5b53-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="a5b53-119">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="a5b53-119">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="a5b53-120">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="a5b53-120">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="a5b53-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a5b53-121">An active Azure account.</span></span> <span data-ttu-id="a5b53-122">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="a5b53-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="a5b53-123">Postupujte podle hello [Začínáme se správou zařízení](iot-hub-csharp-node-device-management-get-started.md) článek toocreate služby IoT hub a získat připojovací řetězec služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a5b53-123">Follow hello [Get started with device management](iot-hub-csharp-node-device-management-get-started.md) article toocreate your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a><span data-ttu-id="a5b53-124">Aktivovat aktualizaci firmwaru vzdálené na zařízení hello pomocí přímá metoda</span><span class="sxs-lookup"><span data-stu-id="a5b53-124">Trigger a remote firmware update on hello device using a direct method</span></span>
<span data-ttu-id="a5b53-125">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#), zahájí aktualizaci vzdálené firmwaru v zařízení.</span><span class="sxs-lookup"><span data-stu-id="a5b53-125">In this section, you create a .NET console app (using C#) that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="a5b53-126">aplikace Hello používá s přímá metoda tooinitiate hello aktualizací a používá zařízení twin dotazy tooperiodically získat stav hello hello active firmware aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a5b53-126">hello app uses a direct method tooinitiate hello update and uses device twin queries tooperiodically get hello status of hello active firmware update.</span></span>

1. <span data-ttu-id="a5b53-127">V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="a5b53-127">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="a5b53-128">Název projektu hello **TriggerFWUpdate**.</span><span class="sxs-lookup"><span data-stu-id="a5b53-128">Name hello project **TriggerFWUpdate**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

1. <span data-ttu-id="a5b53-130">V Průzkumníku řešení klikněte pravým tlačítkem na hello **TriggerFWUpdate** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="a5b53-130">In Solution Explorer, right-click hello **TriggerFWUpdate** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="a5b53-131">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="a5b53-131">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="a5b53-132">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="a5b53-132">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="a5b53-134">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="a5b53-134">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. <span data-ttu-id="a5b53-135">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="a5b53-135">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="a5b53-136">Nahraďte hello více zástupné hodnoty hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello a hello Id vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="a5b53-136">Replace hello multiple placeholder values with hello IoT Hub connection string for hello hub that you created in hello previous section and hello Id of your device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. <span data-ttu-id="a5b53-137">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="a5b53-137">Add hello following method toohello **Program** class:</span></span>
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. <span data-ttu-id="a5b53-138">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="a5b53-138">Add hello following method toohello **Program** class:</span></span>

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

1. <span data-ttu-id="a5b53-139">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="a5b53-139">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
1. <span data-ttu-id="a5b53-140">V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **TriggerFWUpdate** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="a5b53-140">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **TriggerFWUpdate** project is **Start**.</span></span>

1. <span data-ttu-id="a5b53-141">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="a5b53-141">Build hello solution.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a><span data-ttu-id="a5b53-142">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a5b53-142">Run hello apps</span></span>
<span data-ttu-id="a5b53-143">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5b53-143">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="a5b53-144">Na příkazovém řádku hello v hello **manageddevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.</span><span class="sxs-lookup"><span data-stu-id="a5b53-144">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="a5b53-145">V sadě Visual Studio, klikněte pravým tlačítkem na hello **TriggerFWUpdate** projectRun toohello C# konzolovou aplikaci, vyberte **ladění** a **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="a5b53-145">In Visual Studio, right-click on hello **TriggerFWUpdate** projectRun toohello C# console app, select **Debug** and **Start new instance**.</span></span>

3. <span data-ttu-id="a5b53-146">Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="a5b53-146">You see hello device response toohello direct method in hello console.</span></span>

    ![Firmware aktualizace byla úspěšná.][img-fwupdate]

## <a name="next-steps"></a><span data-ttu-id="a5b53-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5b53-148">Next steps</span></span>
<span data-ttu-id="a5b53-149">V tomto kurzu jste použili tootrigger přímá metoda vzdálené aktualizace firmwaru v zařízení a používané hello hlášené vlastnosti toofollow hello průběh aktualizace firmwaru hello.</span><span class="sxs-lookup"><span data-stu-id="a5b53-149">In this tutorial, you used a direct method tootrigger a remote firmware update on a device and used hello reported properties toofollow hello progress of hello firmware update.</span></span>

<span data-ttu-id="a5b53-150">toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="a5b53-150">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png
[img-fwupdate]: media/iot-hub-csharp-node-firmware-update/fwupdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/