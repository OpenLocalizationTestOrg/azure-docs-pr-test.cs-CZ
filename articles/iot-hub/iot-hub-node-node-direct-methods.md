---
title: "Azure IoT Hub přímé metody (uzel) | Microsoft Docs"
description: "Jak používat Azure IoT Hub přímé metody. K implementaci aplikace simulovaného zařízení, která zahrnuje přímá metoda a aplikační služby, která volá metodu přímé použijete SDK služby Azure IoT pro Node.js."
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
ms.openlocfilehash: 83725c3ae3fd3807f2469be888e270ba078a8972
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a><span data-ttu-id="8a6d4-104">Použití přímé metody na zařízení IoT s Node.js</span><span class="sxs-lookup"><span data-stu-id="8a6d4-104">Use direct methods on your IoT device with Node.js</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="8a6d4-105">Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-105">At the end of this tutorial, you have two Node.js console apps:</span></span>

* <span data-ttu-id="8a6d4-106">**CallMethodOnDevice.js**, která volá metodu v aplikaci simulovaného zařízení a zobrazí odpověď.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-106">**CallMethodOnDevice.js**, which calls a method in the simulated device app and displays the response.</span></span>
* <span data-ttu-id="8a6d4-107">**SimulatedDevice.js**, který se připojuje ke službě IoT hub s dříve vytvořenou identitou zařízení a reaguje na metodu volá cloudu.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-107">**SimulatedDevice.js**, which connects to your IoT hub with the device identity created earlier, and responds to the method called by the cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="8a6d4-108">Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="8a6d4-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both applications to run on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="8a6d4-109">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-109">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="8a6d4-110">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="8a6d4-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-111">An active Azure account.</span></span> <span data-ttu-id="8a6d4-112">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="8a6d4-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="8a6d4-113">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="8a6d4-113">Create a simulated device app</span></span>
<span data-ttu-id="8a6d4-114">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje na metodu s názvem cloudem.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-114">In this section, you create a Node.js console app that responds to a method called by the cloud.</span></span>

1. <span data-ttu-id="8a6d4-115">Vytvořte novou prázdnou složku s názvem **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-115">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="8a6d4-116">Ve složce **simulateddevice** vytvořte soubor package.json pomocí následujícího příkazu v příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-116">In the **simulateddevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="8a6d4-117">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-117">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="8a6d4-118">V příkazovém řádku ve složce **simulateddevice** spusťte následující příkaz k instalaci balíčku sady SDK pro zařízení **azure-iot-device** a balíčku **azure-iot-device-amqp**:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-118">At your command prompt in the **simulateddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="8a6d4-119">Pomocí textového editoru, vytvořte nový soubor **SimulatedDevice.js** ve složce **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-119">Using a text editor, create a new **SimulatedDevice.js** file in the **simulateddevice** folder.</span></span>
4. <span data-ttu-id="8a6d4-120">Na začátek souboru **SimulatedDevice.js** přidejte následující příkazy `require`:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-120">Add the following `require` statements at the start of the **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="8a6d4-121">Přidat **connectionString** proměnné a použít ho k vytvoření **DeviceClient** instance.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-121">Add a **connectionString** variable and use it to create a **DeviceClient** instance.</span></span> <span data-ttu-id="8a6d4-122">Nahraďte **{zařízení připojovací řetězec}** s připojením zařízení vygenerovanými v řetězci *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-122">Replace **{device connection string}** with the device connection string you generated in the *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="8a6d4-123">Přidejte následující funkci implementovat metodu na zařízení:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-123">Add the following function to implement the method on the device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="8a6d4-124">Otevřete připojení do služby IoT hub a spusťte inicializovat naslouchací proces metoda:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-124">Open the connection to your IoT hub and start initialize the method listener:</span></span>
   
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
8. <span data-ttu-id="8a6d4-125">Soubor **SimulatedDevice.js** uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-125">Save and close the **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="8a6d4-126">Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-126">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="8a6d4-127">V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="8a6d4-127">In production code, you should implement retry policies (such as connection retry), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-method-on-a-device"></a><span data-ttu-id="8a6d4-128">Volání metody na zařízení</span><span class="sxs-lookup"><span data-stu-id="8a6d4-128">Call a method on a device</span></span>
<span data-ttu-id="8a6d4-129">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která volá metodu v aplikaci simulovaného zařízení a potom zobrazí odpověď.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-129">In this section, you create a Node.js console app that calls a method in the simulated device app and then displays the response.</span></span>

1. <span data-ttu-id="8a6d4-130">Vytvořit novou prázdnou složku s názvem **callmethodondevice**.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-130">Create a new empty folder called **callmethodondevice**.</span></span> <span data-ttu-id="8a6d4-131">V **callmethodondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-131">In the **callmethodondevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="8a6d4-132">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-132">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="8a6d4-133">Na příkazovém řádku v **callmethodondevice** složky, spusťte následující příkaz k instalaci **azure-iothub** balíčku:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-133">At your command prompt in the **callmethodondevice** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="8a6d4-134">Pomocí textového editoru, vytvořte **CallMethodOnDevice.js** v soubor **callmethodondevice** složky.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-134">Using a text editor, create a **CallMethodOnDevice.js** file in the **callmethodondevice** folder.</span></span>
4. <span data-ttu-id="8a6d4-135">Přidejte následující `require` příkazy na začátku **CallMethodOnDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-135">Add the following `require` statements at the start of the **CallMethodOnDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="8a6d4-136">Přidejte následující deklaraci proměnné a nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro vaši službu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-136">Add the following variable declaration and replace the placeholder value with the IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. <span data-ttu-id="8a6d4-137">Vytvoření klienta k otevření připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-137">Create the client to open the connection to your IoT hub.</span></span>
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. <span data-ttu-id="8a6d4-138">Přidejte následující funkci k vyvolání metody zařízení a vytisknout odpověď zařízení do konzoly:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-138">Add the following function to invoke the device method and print the device response to the console:</span></span>
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. <span data-ttu-id="8a6d4-139">Uložte a zavřete **CallMethodOnDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-139">Save and close the **CallMethodOnDevice.js** file.</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="8a6d4-140">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="8a6d4-140">Run the apps</span></span>
<span data-ttu-id="8a6d4-141">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-141">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="8a6d4-142">Na příkazovém řádku v **simulateddevice** složky, spusťte následující příkaz, který zahájit naslouchání pro volání metod ze služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-142">At a command prompt in the **simulateddevice** folder, run the following command to start listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. <span data-ttu-id="8a6d4-143">Na příkazovém řádku v **callmethodondevice** složky, spusťte následující příkaz ke spuštění monitorování služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-143">At a command prompt in the **callmethodondevice** folder, run the following command to begin monitoring your IoT hub:</span></span>
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. <span data-ttu-id="8a6d4-144">Zobrazí se zařízení reagovat na metodu tiskem, zprávu a aplikace, který označuje zobrazení metoda odpověď ze zařízení:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-144">You will see the device react to the method by printing out the message and the application which called the method display the response from the device:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="8a6d4-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a6d4-145">Next steps</span></span>
<span data-ttu-id="8a6d4-146">V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-146">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="8a6d4-147">Povolit aplikaci simulovaného zařízení reagování na metody vyvolané cloudu použijete tuto identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-147">You used this device identity to enable the simulated device app to react to methods invoked by the cloud.</span></span> <span data-ttu-id="8a6d4-148">Můžete také vytvořit aplikaci, která volá metody na zařízení a zobrazí odpověď ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-148">You also created an app that invokes methods on the device and displays the response from the device.</span></span> 

<span data-ttu-id="8a6d4-149">Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:</span><span class="sxs-lookup"><span data-stu-id="8a6d4-149">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="8a6d4-150">[Začínáme s centrem IoT]</span><span class="sxs-lookup"><span data-stu-id="8a6d4-150">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="8a6d4-151">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="8a6d4-151">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="8a6d4-152">Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="8a6d4-152">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
