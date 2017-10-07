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
# <a name="use-device-management-tooinitiate-a-device-firmware-update-netnode"></a>Použití zařízení správu tooinitiate aktualizaci firmwaru zařízení (.NET nebo uzel)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Úvod
V hello [Začínáme se správou zařízení] [ lnk-dm-getstarted] kurzu jste viděli, jak toouse hello [dvojče zařízení] [ lnk-devtwin] a [přímé metody] [ lnk-c2dmethod] tooremotely primitiv restartování zařízení. Tento kurz používá hello stejné primitiv IoT Hub a popisuje, jak toodo začátku do konce simulated aktualizaci firmwaru.  Tento vzor se používá v implementaci aktualizace firmwaru hello pro hello [malin platformy zařízení implementace ukázka][lnk-rpi-implementation].

V tomto kurzu získáte informace o následujících postupech:

* Vytvoření konzolové aplikace .NET, která volá metodu přímé firmwareUpdate hello v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.
* Vytvoření aplikace simulovaného zařízení, která implementuje **firmwareUpdate** přímá metoda. Tato metoda inicializuje více fáze procesu, který čeká toodownload hello firmware image, soubory ke stažení bitové kopie hello firmware a nakonec platí hello firmware image. Během každé fáze hello aktualizace hello zařízení používá hello ohlášeny průběh tooreport vlastnosti.

Na konci hello tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):

**dmpatterns_fwupdate_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení hello, zobrazí hello odpovědi a pravidelně (každých 500ms) zobrazí hello aktualizovat hlášené vlastnosti.

**TriggerFWUpdate**, který připojí tooyour IoT hub s dříve, vytvořenou identitou zařízení hello dostane přímá metoda firmwareUpdate, spustí prostřednictvím toosimulate více stavu procesu, včetně aktualizace firmwaru: Čekání na stažení bitové kopie hello Probíhá stahování hello novou bitovou kopii a bitovou kopii nakonec použití hello.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Node.js verze 0.12.x nebo novější, <br/>  [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

Postupujte podle hello [Začínáme se správou zařízení](iot-hub-csharp-node-device-management-get-started.md) článek toocreate služby IoT hub a získat připojovací řetězec služby IoT Hub.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a>Aktivovat aktualizaci firmwaru vzdálené na zařízení hello pomocí přímá metoda
V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#), zahájí aktualizaci vzdálené firmwaru v zařízení. aplikace Hello používá s přímá metoda tooinitiate hello aktualizací a používá zařízení twin dotazy tooperiodically získat stav hello hello active firmware aktualizace.

1. V sadě Visual Studio, přidejte aktuální řešení Visual C# Windows klasický desktopový projekt toohello pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **TriggerFWUpdate**.

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **TriggerFWUpdate** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .
1. V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.

    ![Okno Správce balíčků NuGet][img-servicenuget]
1. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hello více zástupné hodnoty hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello a hello Id vašeho zařízení.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. Přidejte následující metodu toohello hello **Program** třídy:
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. Přidejte následující metodu toohello hello **Program** třídy:

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

1. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
1. V Průzkumníku řešení hello, otevřete hello **nastavit projekty po spuštění...**  a ujistěte se, zda text hello **akce** pro **TriggerFWUpdate** je projekt **spustit**.

1. Vytvoření řešení hello.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a>Spuštění aplikace hello
Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku hello v hello **manageddevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. V sadě Visual Studio, klikněte pravým tlačítkem na hello **TriggerFWUpdate** projectRun toohello C# konzolovou aplikaci, vyberte **ladění** a **spustit novou instanci**.

3. Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.

    ![Firmware aktualizace byla úspěšná.][img-fwupdate]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste použili tootrigger přímá metoda vzdálené aktualizace firmwaru v zařízení a používané hello hlášené vlastnosti toofollow hello průběh aktualizace firmwaru hello.

toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

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