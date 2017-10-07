---
title: "aaaAzure IoT Hub přímé metody (uzel) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub přímé metody. SDK služby Azure IoT hello se používá pro Node.js tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ea9c73ca-7778-4e38-a8f1-0bee9d142f04
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12300ba451816fec1f80163b633f6b6e411d9e5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a><span data-ttu-id="839ec-104">Použití přímé metody na zařízení IoT s Node.js</span><span class="sxs-lookup"><span data-stu-id="839ec-104">Use direct methods on your IoT device with Node.js</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="839ec-105">Na konci hello tohoto kurzu máte dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="839ec-105">At hello end of this tutorial, you have two Node.js console apps:</span></span>

* <span data-ttu-id="839ec-106">**CallMethodOnDevice.js**, která volá metodu v aplikaci simulovaného zařízení hello a zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="839ec-106">**CallMethodOnDevice.js**, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="839ec-107">**SimulatedDevice.js**, který připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello a reaguje toohello metodu s názvem hello cloudem.</span><span class="sxs-lookup"><span data-stu-id="839ec-107">**SimulatedDevice.js**, which connects tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="839ec-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="839ec-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="839ec-109">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="839ec-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="839ec-110">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="839ec-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="839ec-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="839ec-111">An active Azure account.</span></span> <span data-ttu-id="839ec-112">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="839ec-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="839ec-113">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="839ec-113">Create a simulated device app</span></span>
<span data-ttu-id="839ec-114">V této části vytvoříte konzolovou aplikaci Node.js, která odpovídá tooa metodu s názvem hello cloudem.</span><span class="sxs-lookup"><span data-stu-id="839ec-114">In this section, you create a Node.js console app that responds tooa method called by hello cloud.</span></span>

1. <span data-ttu-id="839ec-115">Vytvořte novou prázdnou složku s názvem **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="839ec-115">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="839ec-116">V hello **simulateddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="839ec-116">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="839ec-117">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="839ec-117">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="839ec-118">Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:</span><span class="sxs-lookup"><span data-stu-id="839ec-118">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="839ec-119">Pomocí textového editoru, vytvořte novou **SimulatedDevice.js** souboru v hello **simulateddevice** složky.</span><span class="sxs-lookup"><span data-stu-id="839ec-119">Using a text editor, create a new **SimulatedDevice.js** file in hello **simulateddevice** folder.</span></span>
4. <span data-ttu-id="839ec-120">Přidejte následující hello `require` příkazy v hello začátek hello **SimulatedDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="839ec-120">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="839ec-121">Přidat **connectionString** proměnné a použít ho toocreate **DeviceClient** instance.</span><span class="sxs-lookup"><span data-stu-id="839ec-121">Add a **connectionString** variable and use it toocreate a **DeviceClient** instance.</span></span> <span data-ttu-id="839ec-122">Nahraďte **{zařízení připojovací řetězec}** s řetězcem připojení zařízení hello jste vygenerovali v hello *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="839ec-122">Replace **{device connection string}** with hello device connection string you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="839ec-123">Přidejte následující metodu hello tooimplement funkce v zařízení hello hello:</span><span class="sxs-lookup"><span data-stu-id="839ec-123">Add hello following function tooimplement hello method on hello device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="839ec-124">Otevřete Centrum IoT tooyour hello připojení a spusťte inicializovat hello metoda naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="839ec-124">Open hello connection tooyour IoT hub and start initialize hello method listener:</span></span>
   
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
8. <span data-ttu-id="839ec-125">Uložte a zavřete hello **SimulatedDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="839ec-125">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="839ec-126">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="839ec-126">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="839ec-127">V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="839ec-127">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-method-on-a-device"></a><span data-ttu-id="839ec-128">Volání metody na zařízení</span><span class="sxs-lookup"><span data-stu-id="839ec-128">Call a method on a device</span></span>
<span data-ttu-id="839ec-129">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která volá metodu v aplikaci simulovaného zařízení hello a potom zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="839ec-129">In this section, you create a Node.js console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="839ec-130">Vytvořit novou prázdnou složku s názvem **callmethodondevice**.</span><span class="sxs-lookup"><span data-stu-id="839ec-130">Create a new empty folder called **callmethodondevice**.</span></span> <span data-ttu-id="839ec-131">V hello **callmethodondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="839ec-131">In hello **callmethodondevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="839ec-132">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="839ec-132">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="839ec-133">Na příkazovém řádku v hello **callmethodondevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku:</span><span class="sxs-lookup"><span data-stu-id="839ec-133">At your command prompt in hello **callmethodondevice** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="839ec-134">Pomocí textového editoru, vytvořte **CallMethodOnDevice.js** souboru v hello **callmethodondevice** složky.</span><span class="sxs-lookup"><span data-stu-id="839ec-134">Using a text editor, create a **CallMethodOnDevice.js** file in hello **callmethodondevice** folder.</span></span>
4. <span data-ttu-id="839ec-135">Přidejte následující hello `require` příkazy v hello začátek hello **CallMethodOnDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="839ec-135">Add hello following `require` statements at hello start of hello **CallMethodOnDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="839ec-136">Přidejte následující deklarace proměnné hello a nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro vaše centrum:</span><span class="sxs-lookup"><span data-stu-id="839ec-136">Add hello following variable declaration and replace hello placeholder value with hello IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. <span data-ttu-id="839ec-137">Vytvoření centra IoT tooyour hello hello klienta tooopen připojení.</span><span class="sxs-lookup"><span data-stu-id="839ec-137">Create hello client tooopen hello connection tooyour IoT hub.</span></span>
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. <span data-ttu-id="839ec-138">Přidejte následující funkce tooinvoke hello zařízení metoda tiskových hello zařízení odpovědi toohello konzoly a hello:</span><span class="sxs-lookup"><span data-stu-id="839ec-138">Add hello following function tooinvoke hello device method and print hello device response toohello console:</span></span>
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed tooinvoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. <span data-ttu-id="839ec-139">Uložte a zavřete hello **CallMethodOnDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="839ec-139">Save and close hello **CallMethodOnDevice.js** file.</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="839ec-140">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="839ec-140">Run hello apps</span></span>
<span data-ttu-id="839ec-141">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="839ec-141">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="839ec-142">Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz toostart přijímá metoda volání ze služby IoT Hub hello:</span><span class="sxs-lookup"><span data-stu-id="839ec-142">At a command prompt in hello **simulateddevice** folder, run hello following command toostart listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. <span data-ttu-id="839ec-143">Na příkazovém řádku v hello **callmethodondevice** složky, spusťte následující příkaz toobegin monitorovat služba IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="839ec-143">At a command prompt in hello **callmethodondevice** folder, run hello following command toobegin monitoring your IoT hub:</span></span>
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. <span data-ttu-id="839ec-144">Zobrazí se zařízení hello reagovat toohello metoda tiskem uvítací zprávu a hello aplikace, který označuje hello metoda zobrazení hello odpověď z hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="839ec-144">You will see hello device react toohello method by printing out hello message and hello application which called hello method display hello response from hello device:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="839ec-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="839ec-145">Next steps</span></span>
<span data-ttu-id="839ec-146">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="839ec-146">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="839ec-147">Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci tooreact toomethods vyvolané hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="839ec-147">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="839ec-148">Můžete také vytvořit aplikaci, která volá metody na hello zařízení a zobrazí hello odezvu hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="839ec-148">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="839ec-149">toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:</span><span class="sxs-lookup"><span data-stu-id="839ec-149">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="839ec-150">[Začínáme s centrem IoT]</span><span class="sxs-lookup"><span data-stu-id="839ec-150">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="839ec-151">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="839ec-151">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="839ec-152">toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="839ec-152">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-node-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-node-node-direct-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-node-node-direct-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Začínáme s centrem IoT]: iot-hub-node-node-getstarted.md
