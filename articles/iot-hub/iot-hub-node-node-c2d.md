---
title: "zprávy aaaCloud zařízení s Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak toosend cloud zařízení zpráv tooa zařízení ze služby Azure IoT hub pomocí hello Azure IoT sady SDK pro Node.js. Upravte zprávy typu cloud zařízení tooreceive aplikace simulovaného zařízení a změny zpráv back-end aplikace hello toosend cloud zařízení."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 1ccae0cada52193c2abb91504c086cac226e93da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a><span data-ttu-id="f78b2-104">Odesílání zpráv typu cloud zařízení službou IoT Hub (uzel)</span><span class="sxs-lookup"><span data-stu-id="f78b2-104">Send cloud-to-device messages with IoT Hub (Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="f78b2-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="f78b2-105">Introduction</span></span>
<span data-ttu-id="f78b2-106">Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="f78b2-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="f78b2-107">Hello [Začínáme se službou IoT Hub] kurzu se dozvíte, jak zřídit identitu zařízení v ní toocreate služby IoT hub a kódu aplikaci ze simulovaného zařízení, která odesílá zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="f78b2-107">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="f78b2-108">V tomto kurzu vychází [Začínáme se službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="f78b2-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="f78b2-109">Jak ukazuje na:</span><span class="sxs-lookup"><span data-stu-id="f78b2-109">It shows you how to:</span></span>

* <span data-ttu-id="f78b2-110">Z back-end vašeho řešení odesílání zpráv typu cloud zařízení tooa jedno zařízení prostřednictvím služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f78b2-110">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="f78b2-111">Příjem zpráv typu cloud zařízení na zařízení.</span><span class="sxs-lookup"><span data-stu-id="f78b2-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="f78b2-112">Z back-end vašeho řešení, žádosti o potvrzení o doručení (*zpětné vazby*) pro zprávy odeslané tooa zařízení ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f78b2-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="f78b2-113">Další informace o zprávy typu cloud zařízení můžete najít v hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="f78b2-113">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="f78b2-114">Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="f78b2-114">At hello end of this tutorial, you run two Node.js console apps:</span></span>

* <span data-ttu-id="f78b2-115">**SimulatedDevice**, upravenou verzi hello aplikace vytvořená v [Začínáme se službou IoT Hub], který připojí tooyour IoT hub a přijímá zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="f78b2-115">**SimulatedDevice**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="f78b2-116">**SendCloudToDeviceMessage**, který odesílá aplikace simulovaného zařízení toohello zpráv typu cloud zařízení prostřednictvím služby IoT Hub a pak přijímá jeho potvrzení o doručení.</span><span class="sxs-lookup"><span data-stu-id="f78b2-116">**SendCloudToDeviceMessage**, which sends a cloud-to-device message toohello simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="f78b2-117">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky (například C, Javy a JavaScriptu) prostřednictvím SDK pro zařízení Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="f78b2-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="f78b2-118">Podrobné pokyny o tom, jak tooconnect kurzu vaše zařízení toothis kódu a obecně tooAzure IoT Hub, najdete v hello [Azure střediska pro vývojáře IoT].</span><span class="sxs-lookup"><span data-stu-id="f78b2-118">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [Azure IoT Developer Center].</span></span>
> 
> 

<span data-ttu-id="f78b2-119">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="f78b2-119">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="f78b2-120">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f78b2-120">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="f78b2-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f78b2-121">An active Azure account.</span></span> <span data-ttu-id="f78b2-122">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="f78b2-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-simulated-device-app"></a><span data-ttu-id="f78b2-123">Příjem zpráv v aplikaci simulovaného zařízení hello</span><span class="sxs-lookup"><span data-stu-id="f78b2-123">Receive messages in hello simulated device app</span></span>
<span data-ttu-id="f78b2-124">V této části upravíte jste vytvořili v aplikaci simulovaného zařízení hello [Začínáme se službou IoT Hub] tooreceive zprávy typu cloud zařízení ze služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="f78b2-124">In this section, you modify hello simulated device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="f78b2-125">Pomocí textového editoru otevřete soubor SimulatedDevice.js hello.</span><span class="sxs-lookup"><span data-stu-id="f78b2-125">Using a text editor, open hello SimulatedDevice.js file.</span></span>
2. <span data-ttu-id="f78b2-126">Upravit hello **connectCallback** funkce toohandle zprávy odeslané ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f78b2-126">Modify hello **connectCallback** function toohandle messages sent from IoT Hub.</span></span> <span data-ttu-id="f78b2-127">V tomto příkladu hello zařízení vždy vyvolá hello **dokončení** funkce toonotify IoT Hub, že zpracovala uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="f78b2-127">In this example, hello device always invokes hello **complete** function toonotify IoT Hub that it has processed hello message.</span></span> <span data-ttu-id="f78b2-128">Vaše nové verze hello **connectCallback** funkce vypadá hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="f78b2-128">Your new version of hello **connectCallback** function looks like hello following snippet:</span></span>
   
    ```javascript
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it toohello IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);            
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
   > [!NOTE]
   > <span data-ttu-id="f78b2-129">Pokud používáte protokol HTTP místo MQTT nebo AMQP jako hello přenosu, hello **DeviceClient** instance vyhledává zprávy ze služby IoT Hub zřídka (méně než každých 25 minut).</span><span class="sxs-lookup"><span data-stu-id="f78b2-129">If you use HTTP instead of MQTT or AMQP as hello transport, hello **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="f78b2-130">Další informace o hello rozdíly mezi podpora MQTT, AMQP a HTTP a omezení služby IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="f78b2-130">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="f78b2-131">Odeslání zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="f78b2-131">Send a cloud-to-device message</span></span>
<span data-ttu-id="f78b2-132">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která odešle aplikace simulovaného zařízení toohello zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="f78b2-132">In this section, you create a Node.js console app that sends cloud-to-device messages toohello simulated device app.</span></span> <span data-ttu-id="f78b2-133">ID zařízení hello zařízení, které jste přidali v hello potřebovat hello [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="f78b2-133">You need hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="f78b2-134">Můžete také potřebovat hello připojovací řetězec služby IoT Hub pro vaše centrum, které můžete najít v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="f78b2-134">You also need hello IoT Hub connection string for your hub that you can find in hello [Azure portal].</span></span>

1. <span data-ttu-id="f78b2-135">Vytvořit prázdnou složku s názvem **sendcloudtodevicemessage**.</span><span class="sxs-lookup"><span data-stu-id="f78b2-135">Create an empty folder called **sendcloudtodevicemessage**.</span></span> <span data-ttu-id="f78b2-136">V hello **sendcloudtodevicemessage** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="f78b2-136">In hello **sendcloudtodevicemessage** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="f78b2-137">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="f78b2-137">Accept all hello defaults:</span></span>
   
    ```shell
    npm init
    ```
2. <span data-ttu-id="f78b2-138">Na příkazovém řádku v hello **sendcloudtodevicemessage** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku:</span><span class="sxs-lookup"><span data-stu-id="f78b2-138">At your command prompt in hello **sendcloudtodevicemessage** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```shell
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="f78b2-139">Pomocí textového editoru, vytvořte **SendCloudToDeviceMessage.js** souboru v hello **sendcloudtodevicemessage** složky.</span><span class="sxs-lookup"><span data-stu-id="f78b2-139">Using a text editor, create a **SendCloudToDeviceMessage.js** file in hello **sendcloudtodevicemessage** folder.</span></span>
4. <span data-ttu-id="f78b2-140">Přidejte následující hello `require` příkazy v hello začátek hello **SendCloudToDeviceMessage.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="f78b2-140">Add hello following `require` statements at hello start of hello **SendCloudToDeviceMessage.js** file:</span></span>
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. <span data-ttu-id="f78b2-141">Přidejte následující kód příliš hello**SendCloudToDeviceMessage.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="f78b2-141">Add hello following code too**SendCloudToDeviceMessage.js** file.</span></span> <span data-ttu-id="f78b2-142">Nahraďte hodnotu zástupného symbolu hello "{iot hub připojovací řetězec}" hello připojovací řetězec služby IoT Hub pro hello rozbočovače, které jste vytvořili v hello [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="f78b2-142">Replace hello "{iot hub connection string}" placeholder value with hello IoT Hub connection string for hello hub you created in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="f78b2-143">Nahraďte zástupný symbol hello "{zařízení id} s ID zařízení hello hello zařízení, které jste přidali v hello [Začínáme se službou IoT Hub] kurzu:</span><span class="sxs-lookup"><span data-stu-id="f78b2-143">Replace hello "{device id}" placeholder with hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial:</span></span>
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="f78b2-144">Přidejte následující funkce tooprint operaci výsledky toohello konzoly hello:</span><span class="sxs-lookup"><span data-stu-id="f78b2-144">Add hello following function tooprint operation results toohello console:</span></span>
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. <span data-ttu-id="f78b2-145">Přidejte následující funkce tooprint doručení zpětnou vazbu zprávy toohello konzoly hello:</span><span class="sxs-lookup"><span data-stu-id="f78b2-145">Add hello following function tooprint delivery feedback messages toohello console:</span></span>
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. <span data-ttu-id="f78b2-146">Přidejte následující hello kód toosend zařízení tooyour zprávy a zpracování zprávy hello zpětné vazby, pokud zařízení hello uznává uvítací zprávu cloud zařízení:</span><span class="sxs-lookup"><span data-stu-id="f78b2-146">Add hello following code toosend a message tooyour device and handle hello feedback message when hello device acknowledges hello cloud-to-device message:</span></span>
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud toodevice message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. <span data-ttu-id="f78b2-147">Uložte a zavřete **SendCloudToDeviceMessage.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="f78b2-147">Save and close **SendCloudToDeviceMessage.js** file.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="f78b2-148">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="f78b2-148">Run hello applications</span></span>
<span data-ttu-id="f78b2-149">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f78b2-149">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="f78b2-150">Na příkazovém řádku hello v hello **simulateddevice** spusťte hello následující příkaz toosend telemetrie tooIoT rozbočovače a toolisten pro zprávy typu cloud zařízení:</span><span class="sxs-lookup"><span data-stu-id="f78b2-150">At hello command prompt in hello **simulateddevice** folder, run hello following command toosend telemetry tooIoT Hub and toolisten for cloud-to-device messages:</span></span>
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Spusťte aplikaci simulovaného zařízení hello][img-simulated-device]
2. <span data-ttu-id="f78b2-152">Na příkazovém řádku v hello **sendcloudtodevicemessage** složky, spusťte následující příkaz toosend hello zprávy typu cloud zařízení a počkat na zpětnou vazbu hello potvrzení:</span><span class="sxs-lookup"><span data-stu-id="f78b2-152">At a command prompt in hello **sendcloudtodevicemessage** folder, run hello following command toosend a cloud-to-device message and wait for hello acknowledgment feedback:</span></span>
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Spusťte hello aplikace toosend hello cloudu do zařízení příkaz][img-send-command]
   
   > [!NOTE]
   > <span data-ttu-id="f78b2-154">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="f78b2-154">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="f78b2-155">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="f78b2-155">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="f78b2-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f78b2-156">Next steps</span></span>
<span data-ttu-id="f78b2-157">V tomto kurzu jste se naučili jak toosend a příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="f78b2-157">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="f78b2-158">Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="f78b2-158">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="f78b2-159">toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="f78b2-159">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Začínáme se službou IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[Azure střediska pro vývojáře IoT]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[portál Azure]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
