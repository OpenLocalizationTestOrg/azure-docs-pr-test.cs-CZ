---
title: "aaaGet spuštění se správou zařízení Azure IoT Hub (.NET nebo uzel) | Microsoft Docs"
description: "Jak tooinitiate správy zařízení Azure IoT Hub toouse vzdáleném zařízení restartovat. Použití zařízení Azure IoT hello SDK pro Node.js tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a hello sady SDK služby Azure IoT pro rozhraní .NET tooimplement aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2016
ms.author: juanpere
ms.openlocfilehash: ea6d50dfac1c222d7836e3bf5503c6c9b8c89dfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-netnode"></a>Začínáme se správou zařízení (.NET nebo uzel)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

V tomto kurzu získáte informace o následujících postupech:

* Použijte hello portálu toocreate Azure IoT Hub a vytvoření identity zařízení ve službě IoT hub.
* Vytvoření aplikace simulovaného zařízení, která obsahuje přímý metodu, která restartování zařízení. Přímé metody jsou vyvolány z cloudu hello.
* Vytvoření konzolové aplikace .NET, která volá metodu přímé hello restartování v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.

Na konci hello tohoto kurzu máte zařízení konzolovou aplikaci Node.js a back-end konzolové aplikace .NET (C#):

**dmpatterns_getstarted_device.js**, který připojí tooyour IoT hub s dříve, vytvořenou identitou zařízení hello obdrží přímá metoda restartování, simuluje restartu fyzické a sestavy hello čas pro poslední restartování hello.

**TriggerReboot**, která volá metodu přímé v aplikaci simulovaného zařízení hello, zobrazí hello odpovědi a zobrazí hello aktualizovat hlášené vlastnosti.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Node.js verze 0.12.x nebo novější, <br/>  [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Aktivační události restartu vzdálené na zařízení hello pomocí přímá metoda
V této části vytvoříte konzolové aplikace .NET (pomocí jazyka C#) iniciované vzdálené restartování na zařízení pomocí přímého metody. aplikace Hello používá zařízení twin dotazy toodiscover hello čas posledního restartování pro toto zařízení.

1. V sadě Visual Studio, přidejte nové řešení Visual C# Windows klasický desktopový projekt tooa pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu. Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější. Název projektu hello **TriggerReboot**.

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][img-createapp]

2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **TriggerReboot** projektu a pak klikněte na **spravovat balíčky NuGet**.
3. V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.

    ![Okno Správce balíčků NuGet][img-servicenuget]
4. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello a hello cílové zařízení.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. Přidejte následující metodu toohello hello **Program** třídy.  Tento kód získá hello dvojče zařízení pro hello restartování zařízení a výstupy hello hlášené vlastnosti.
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. Přidejte následující metodu toohello hello **Program** třídy.  Tento kód zahájí hello restartování na zařízení hello přímé metodou.

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
8. Vytvoření řešení hello.

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části

* Vytvořte konzolovou aplikaci Node.js, která odpovídá tooa přímá metoda volá hello cloudu
* Aktivační události restartu simulovaného zařízení
* Použití hello hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a při jejich poslední restartovat

1. Vytvořit novou prázdnou složku s názvem **manageddevice**.  V hello **manageddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.  Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **manageddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte novou **dmpatterns_getstarted_device.js** souboru v hello **manageddevice** složky.
4. Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_getstarted_device.js** souboru:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance.  Nahraďte hello připojovacího řetězce připojovacím řetězcem zařízení.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Přidejte následující funkce tooimplement hello přímá metoda na zařízení hello hello
   
    ```
    var onReboot = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report hello reboot before hello physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. Přidejte následující hello kód tooopen hello připojení tooyour IoT hub a spustit hello přímá metoda naslouchací proces:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Uložte a zavřete hello **dmpatterns_getstarted_device.js** souboru.
   
> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].


## <a name="run-hello-apps"></a>Spuštění aplikace hello
Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku hello v hello **manageddevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Spuštění hello C# konzolovou aplikaci **TriggerReboot**. Klikněte pravým tlačítkem na hello **TriggerReboot** projekt, vyberte **ladění**a potom vyberte **spustit novou instanci**.

3. Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/