---
title: "Aktualizace firmwaru zařízení s Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Jak používat správu zařízení v Azure IoT Hub zahájíte aktualizaci firmwaru zařízení. Použití zařízení Azure IoT SDK pro Node.js pro implementaci aplikace simulovaného zařízení a sady SDK pro .NET k implementaci služby aplikaci, která spustí aktualizaci firmwaru služby Azure IoT."
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
ms.openlocfilehash: c2192328a152e955d182c4a07b391c98a5960964
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnode"></a><span data-ttu-id="dd9e1-104">Použití správy zařízení za účelem zahájení aktualizaci firmwaru zařízení (.NET nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="dd9e1-104">Use device management to initiate a device firmware update (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="dd9e1-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="dd9e1-105">Introduction</span></span>
<span data-ttu-id="dd9e1-106">V [Začínáme se správou zařízení] [ lnk-dm-getstarted] kurz, jste viděli, jak používat [dvojče zařízení] [ lnk-devtwin] a [přímé metody ] [ lnk-c2dmethod] primitiv vzdáleně restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-106">In the [Get started with device management][lnk-dm-getstarted] tutorial, you saw how to use the [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives to remotely reboot a device.</span></span> <span data-ttu-id="dd9e1-107">Tento kurz používá stejné primitiv IoT Hub a ukazuje, jak provést aktualizaci firmwaru simulované začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-107">This tutorial uses the same IoT Hub primitives and shows you how to do an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="dd9e1-108">Tento vzor slouží k implementaci firmwaru aktualizace pro [malin platformy zařízení implementace ukázka][lnk-rpi-implementation].</span><span class="sxs-lookup"><span data-stu-id="dd9e1-108">This pattern is used in the firmware update implementation for the [Raspberry Pi device implementation sample][lnk-rpi-implementation].</span></span>

<span data-ttu-id="dd9e1-109">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="dd9e1-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="dd9e1-110">Vytvoření konzolové aplikace .NET, která volá metodu firmwareUpdate přímé v aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-110">Create a .NET console app that calls the firmwareUpdate direct method in the simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="dd9e1-111">Vytvoření aplikace simulovaného zařízení, která implementuje **firmwareUpdate** přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="dd9e1-112">Tato metoda inicializuje více fáze procesu, který čeká na stažení bitové kopie firmwaru, stáhne bitovou kopii firmware a nakonec platí bitovou kopii firmwaru.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-112">This method initiates a multi-stage process that waits to download the firmware image, downloads the firmware image, and finally applies the firmware image.</span></span> <span data-ttu-id="dd9e1-113">Během aktualizace v každé fázi používá zařízení hlášené vlastnosti hlásit průběh.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-113">During each stage of the update, the device uses the reported properties to report on progress.</span></span>

<span data-ttu-id="dd9e1-114">Na konci tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):</span><span class="sxs-lookup"><span data-stu-id="dd9e1-114">At the end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="dd9e1-115">**dmpatterns_fwupdate_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení, zobrazí odpověď a pravidelně (každých 500ms) zobrazí aktualizovaná hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in the simulated device app, displays the response, and periodically (every 500ms) displays the updated reported properties.</span></span>

<span data-ttu-id="dd9e1-116">**TriggerFWUpdate**, který se připojí ke službě IoT hub s identitou zařízení vytvořenou dříve, dostane přímá metoda firmwareUpdate, spustí prostřednictvím více stavu procesu k simulaci, včetně aktualizace firmwaru: Čekání na stažení bitové kopie stahování novou bitovou kopii a nakonec použitím bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-116">**TriggerFWUpdate**, which connects to your IoT hub with the device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process to simulate a firmware update including: waiting for the image download, downloading the new image, and finally applying the image.</span></span>

<span data-ttu-id="dd9e1-117">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="dd9e1-117">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="dd9e1-118">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="dd9e1-119">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="dd9e1-119">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="dd9e1-120">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje postup instalace Node.js pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-120">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="dd9e1-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-121">An active Azure account.</span></span> <span data-ttu-id="dd9e1-122">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="dd9e1-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="dd9e1-123">Postupujte podle [Začínáme se správou zařízení](iot-hub-csharp-node-device-management-get-started.md) článek k vytvoření služby IoT hub a získat připojovací řetězec služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-123">Follow the [Get started with device management](iot-hub-csharp-node-device-management-get-started.md) article to create your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a><span data-ttu-id="dd9e1-124">Spustit aktualizaci vzdálené firmwaru v zařízení s přímá metoda</span><span class="sxs-lookup"><span data-stu-id="dd9e1-124">Trigger a remote firmware update on the device using a direct method</span></span>
<span data-ttu-id="dd9e1-125">V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#), zahájí aktualizaci vzdálené firmwaru v zařízení.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-125">In this section, you create a .NET console app (using C#) that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="dd9e1-126">Přímá metoda používá k zahájení aktualizace a aplikace používá zařízení twin dotazy a pravidelně získat stav aktualizace active firmware.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-126">The app uses a direct method to initiate the update and uses device twin queries to periodically get the status of the active firmware update.</span></span>

1. <span data-ttu-id="dd9e1-127">V sadě Visual Studio přidejte k stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-127">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="dd9e1-128">Název projektu **TriggerFWUpdate**.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-128">Name the project **TriggerFWUpdate**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

1. <span data-ttu-id="dd9e1-130">V Průzkumníku řešení klikněte pravým tlačítkem myši **TriggerFWUpdate** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="dd9e1-130">In Solution Explorer, right-click the **TriggerFWUpdate** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="dd9e1-131">V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-131">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="dd9e1-132">Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-132">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Okno Správce balíčků NuGet][img-servicenuget]
1. <span data-ttu-id="dd9e1-134">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="dd9e1-134">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. <span data-ttu-id="dd9e1-135">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-135">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="dd9e1-136">Více zástupné hodnoty nahraďte připojovací řetězec služby IoT Hub pro rozbočovače, který jste vytvořili v předchozí části a Id vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-136">Replace the multiple placeholder values with the IoT Hub connection string for the hub that you created in the previous section and the Id of your device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. <span data-ttu-id="dd9e1-137">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="dd9e1-137">Add the following method to the **Program** class:</span></span>
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. <span data-ttu-id="dd9e1-138">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="dd9e1-138">Add the following method to the **Program** class:</span></span>

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

1. <span data-ttu-id="dd9e1-139">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="dd9e1-139">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
1. <span data-ttu-id="dd9e1-140">V Průzkumníku řešení otevřete **nastavit projekty po spuštění...**  a zajistěte, aby **akce** pro **TriggerFWUpdate** je projekt **spustit**.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-140">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **TriggerFWUpdate** project is **Start**.</span></span>

1. <span data-ttu-id="dd9e1-141">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-141">Build the solution.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a><span data-ttu-id="dd9e1-142">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="dd9e1-142">Run the apps</span></span>
<span data-ttu-id="dd9e1-143">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-143">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="dd9e1-144">Na příkazovém řádku v **manageddevice** složky, spusťte následující příkaz, aby začal přijímat přímá metoda restartování.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-144">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="dd9e1-145">V sadě Visual Studio, klikněte pravým tlačítkem na **TriggerFWUpdate** projectRun pro C# konzolovou aplikaci, vyberte **ladění** a **spustit novou instanci**.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-145">In Visual Studio, right-click on the **TriggerFWUpdate** projectRun to the C# console app, select **Debug** and **Start new instance**.</span></span>

3. <span data-ttu-id="dd9e1-146">Zobrazí zařízení odpověď na přímá metoda v konzole.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-146">You see the device response to the direct method in the console.</span></span>

    ![Firmware aktualizace byla úspěšná.][img-fwupdate]

## <a name="next-steps"></a><span data-ttu-id="dd9e1-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd9e1-148">Next steps</span></span>
<span data-ttu-id="dd9e1-149">V tomto kurzu přímá metoda používá k aktivaci aktualizace vzdálené firmwaru v zařízení a umožňuje sledovat průběh aktualizace firmwaru hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-149">In this tutorial, you used a direct method to trigger a remote firmware update on a device and used the reported properties to follow the progress of the firmware update.</span></span>

<span data-ttu-id="dd9e1-150">Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="dd9e1-150">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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