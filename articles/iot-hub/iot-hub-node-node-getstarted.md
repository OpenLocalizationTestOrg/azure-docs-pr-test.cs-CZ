---
title: "aaaGet začít s Azure IoT Hub (uzel) | Microsoft Docs"
description: "Zjistěte, jak toosend zařízení cloud zprávy tooAzure IoT Hub pro Node.js pomocí sady SDK služby IoT. Vytvoření simulovaného zařízení a služby aplikace tooregister zařízení, odesílání zpráv a čtení zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0747895365f2359a9c38ea1e85a5881d6efec0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-node"></a><span data-ttu-id="22963-104">Připojení simulovaného zařízení tooyour IoT hub pomocí uzlu</span><span class="sxs-lookup"><span data-stu-id="22963-104">Connect your simulated device tooyour IoT hub using Node</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="22963-105">Na konci hello tohoto kurzu máte tři aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="22963-105">At hello end of this tutorial, you have three Node.js console apps:</span></span>

* <span data-ttu-id="22963-106">**CreateDeviceIdentity.js**, který vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="22963-106">**CreateDeviceIdentity.js**, which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="22963-107">**ReadDeviceToCloudMessages.js**, který zobrazuje hello telemetrické zprávy odesílané aplikace simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="22963-107">**ReadDeviceToCloudMessages.js**, which displays hello telemetry sent by your simulated device app.</span></span>
* <span data-ttu-id="22963-108">**SimulatedDevice.js**, který připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odešle zprávu telemetrie každý druhý pomocí hello MQTT protokolu.</span><span class="sxs-lookup"><span data-stu-id="22963-108">**SimulatedDevice.js**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="22963-109">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="22963-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="22963-110">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="22963-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="22963-111">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="22963-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="22963-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="22963-112">An active Azure account.</span></span> <span data-ttu-id="22963-113">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="22963-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="22963-114">Nyní jste vytvořili svůj IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="22963-114">You have now created your IoT hub.</span></span> <span data-ttu-id="22963-115">Máte hello název hostitele služby IoT Hub a hello připojovací řetězec služby IoT Hub, je nutné, aby toocomplete hello zbytek tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="22963-115">You have hello IoT Hub host name and hello IoT Hub connection string that you need toocomplete hello rest of this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="22963-116">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="22963-116">Create a device identity</span></span>
<span data-ttu-id="22963-117">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která vytvoří identitu zařízení v registru identit hello ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="22963-117">In this section, you create a Node.js console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="22963-118">Zařízení lze připojit pouze tooIoT rozbočovače, pokud má záznam v registru identit hello.</span><span class="sxs-lookup"><span data-stu-id="22963-118">A device can only connect tooIoT hub if it has an entry in hello identity registry.</span></span> <span data-ttu-id="22963-119">Další informace najdete v tématu hello **registru identit** části hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="22963-119">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="22963-120">Při spuštění této konzolové aplikace vygeneruje jedinečné ID zařízení a klíč, že vaše zařízení použít tooidentify samotné při odešle zařízení cloud zprávy tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="22963-120">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="22963-121">Vytvořte novou prázdnou složku s názvem **createdeviceidentity**.</span><span class="sxs-lookup"><span data-stu-id="22963-121">Create a new empty folder called **createdeviceidentity**.</span></span> <span data-ttu-id="22963-122">V hello **createdeviceidentity** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="22963-122">In hello **createdeviceidentity** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="22963-123">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="22963-123">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="22963-124">Na příkazovém řádku v hello **createdeviceidentity** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku sady SDK služby:</span><span class="sxs-lookup"><span data-stu-id="22963-124">At your command prompt in hello **createdeviceidentity** folder, run hello following command tooinstall hello **azure-iothub** Service SDK package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="22963-125">Pomocí textového editoru, vytvořte **CreateDeviceIdentity.js** souboru v hello **createdeviceidentity** složky.</span><span class="sxs-lookup"><span data-stu-id="22963-125">Using a text editor, create a **CreateDeviceIdentity.js** file in hello **createdeviceidentity** folder.</span></span>
4. <span data-ttu-id="22963-126">Přidejte následující hello `require` příkaz při spuštění hello hello **CreateDeviceIdentity.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="22963-126">Add hello following `require` statement at hello start of hello **CreateDeviceIdentity.js** file:</span></span>
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. <span data-ttu-id="22963-127">Přidejte následující kód toohello hello **CreateDeviceIdentity.js** a nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, které jste vytvořili v předchozí části hello:</span><span class="sxs-lookup"><span data-stu-id="22963-127">Add hello following code toohello **CreateDeviceIdentity.js** file and replace hello placeholder value with hello IoT Hub connection string for hello hub you created in hello previous section:</span></span> 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="22963-128">Přidejte následující kód toocreate definice zařízení v registru identit hello ve službě IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="22963-128">Add hello following code toocreate a device definition in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="22963-129">Tento kód vytvoří zařízení, pokud ID zařízení hello neexistuje v registru identit hello, v opačném případě vrátí klíč hello hello existující zařízení:</span><span class="sxs-lookup"><span data-stu-id="22963-129">This code creates a device if hello device ID does not exist in hello identity registry, otherwise it returns hello key of hello existing device:</span></span>
   
    ```
    var device = {
      deviceId: 'myFirstNodeDevice'
    }
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });
   
    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

7. <span data-ttu-id="22963-130">Soubor **CreateDeviceIdentity.js** uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="22963-130">Save and close **CreateDeviceIdentity.js** file.</span></span>
8. <span data-ttu-id="22963-131">toorun hello **createdeviceidentity** aplikace, spustit následující příkaz na příkazovém řádku hello ve složce createdeviceidentity hello hello:</span><span class="sxs-lookup"><span data-stu-id="22963-131">toorun hello **createdeviceidentity** application, execute hello following command at hello command prompt in hello createdeviceidentity folder:</span></span>
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. <span data-ttu-id="22963-132">Poznamenejte si hello **ID zařízení** a **klíč zařízení**.</span><span class="sxs-lookup"><span data-stu-id="22963-132">Make a note of hello **Device ID** and **Device key**.</span></span> <span data-ttu-id="22963-133">Budete potřebovat tyto hodnoty později při vytváření aplikace, která se připojuje tooIoT rozbočovače jako zařízení.</span><span class="sxs-lookup"><span data-stu-id="22963-133">You need these values later when you create an application that connects tooIoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="22963-134">Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="22963-134">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="22963-135">Ukládá ID a klíče toouse zařízení jako zabezpečovací přihlašovací údaje a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením.</span><span class="sxs-lookup"><span data-stu-id="22963-135">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="22963-136">Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště pro konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="22963-136">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="22963-137">Další informace najdete v tématu hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="22963-137">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<a id="D2C_node"></a>
## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="22963-138">Příjem zpráv typu zařízení-cloud</span><span class="sxs-lookup"><span data-stu-id="22963-138">Receive device-to-cloud messages</span></span>
<span data-ttu-id="22963-139">V této části vytvoříte konzolovou aplikaci Node.js, která čte zprávy typu zařízení-cloud ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="22963-139">In this section, you create a Node.js console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="22963-140">IoT hub zpřístupní [Event Hubs][lnk-event-hubs-overview]-koncový bod kompatibilní tooenable jste tooread zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="22963-140">An IoT hub exposes an [Event Hubs][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="22963-141">věcí tookeep jednoduchý, v tomto kurzu vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení.</span><span class="sxs-lookup"><span data-stu-id="22963-141">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="22963-142">Hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurz ukazuje, jak tooprocess zařízení cloud zpráv ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="22963-142">hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how tooprocess device-to-cloud messages at scale.</span></span> <span data-ttu-id="22963-143">Hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurz obsahuje další informace o tom, jak tooprocess zpráv ze služby Event Hubs a je použít toohello koncové body kompatibilní s centrem událostí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="22963-143">hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how tooprocess messages from Event Hubs and is applicable toohello IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="22963-144">Hello koncový bod kompatibilní s centrem událostí pro čtení zpráv typu zařízení cloud vždy používá protokol AMQP hello.</span><span class="sxs-lookup"><span data-stu-id="22963-144">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>
> 
> 

1. <span data-ttu-id="22963-145">Vytvořte prázdnou složku s názvem **readdevicetocloudmessages**.</span><span class="sxs-lookup"><span data-stu-id="22963-145">Create an empty folder called **readdevicetocloudmessages**.</span></span> <span data-ttu-id="22963-146">V hello **readdevicetocloudmessages** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="22963-146">In hello **readdevicetocloudmessages** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="22963-147">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="22963-147">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="22963-148">Na příkazovém řádku v hello **readdevicetocloudmessages** složky, spusťte následující příkaz tooinstall hello hello **centra událostí azure** balíčku:</span><span class="sxs-lookup"><span data-stu-id="22963-148">At your command prompt in hello **readdevicetocloudmessages** folder, run hello following command tooinstall hello **azure-event-hubs** package:</span></span>
   
    ```
    npm install azure-event-hubs --save
    ```
3. <span data-ttu-id="22963-149">Pomocí textového editoru, vytvořte **ReadDeviceToCloudMessages.js** souboru v hello **readdevicetocloudmessages** složky.</span><span class="sxs-lookup"><span data-stu-id="22963-149">Using a text editor, create a **ReadDeviceToCloudMessages.js** file in hello **readdevicetocloudmessages** folder.</span></span>
4. <span data-ttu-id="22963-150">Přidejte následující hello `require` příkazy v hello začátek hello **ReadDeviceToCloudMessages.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="22963-150">Add hello following `require` statements at hello start of hello **ReadDeviceToCloudMessages.js** file:</span></span>
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. <span data-ttu-id="22963-151">Přidejte následující deklarace proměnné hello a nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro vaše centrum:</span><span class="sxs-lookup"><span data-stu-id="22963-151">Add hello following variable declaration and replace hello placeholder value with hello IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. <span data-ttu-id="22963-152">Přidejte následující dvě funkce, které vytisknout výstup konzoly toohello hello:</span><span class="sxs-lookup"><span data-stu-id="22963-152">Add hello following two functions that print output toohello console:</span></span>
   
    ```
    var printError = function (err) {
      console.log(err.message);
    };
   
    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```
7. <span data-ttu-id="22963-153">Přidejte následující kód toocreate hello hello **EventHubClient**, otevřete hello připojení tooyour IoT Hub a vytvoření příjemce pro každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="22963-153">Add hello following code toocreate hello **EventHubClient**, open hello connection tooyour IoT Hub, and create a receiver for each partition.</span></span> <span data-ttu-id="22963-154">Tato aplikace používá filtr, při vytváření přijímače tak, aby hello přijímač četl pouze zprávy odeslané tooIoT Hub až po spuštění hello příjemce.</span><span class="sxs-lookup"><span data-stu-id="22963-154">This application uses a filter when it creates a receiver so that hello receiver only reads messages sent tooIoT Hub after hello receiver starts running.</span></span> <span data-ttu-id="22963-155">Tento filtr je užitečné v testovacím prostředí, takže se zobrazí právě hello aktuální sadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="22963-155">This filter is useful in a test environment so you see just hello current set of messages.</span></span> <span data-ttu-id="22963-156">V produkčním prostředí měli kód zpracovávat všechny zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="22963-156">In a production environment, your code should make sure that it processes all hello messages.</span></span> <span data-ttu-id="22963-157">Další informace najdete v tématu hello [jak tooprocess zpráv typu zařízení cloud IoT Hub] [ lnk-process-d2c-tutorial] kurzu:</span><span class="sxs-lookup"><span data-stu-id="22963-157">For more information, see hello [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial:</span></span>
   
    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```
8. <span data-ttu-id="22963-158">Uložte a zavřete hello **ReadDeviceToCloudMessages.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="22963-158">Save and close hello **ReadDeviceToCloudMessages.js** file.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="22963-159">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="22963-159">Create a simulated device app</span></span>
<span data-ttu-id="22963-160">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která simuluje zařízení odesílající zprávy typu zařízení cloud tooan IoT hub.</span><span class="sxs-lookup"><span data-stu-id="22963-160">In this section, you create a Node.js console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="22963-161">Vytvořte prázdnou složku s názvem **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="22963-161">Create an empty folder called **simulateddevice**.</span></span> <span data-ttu-id="22963-162">V hello **simulateddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="22963-162">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="22963-163">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="22963-163">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="22963-164">Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:</span><span class="sxs-lookup"><span data-stu-id="22963-164">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="22963-165">Pomocí textového editoru, vytvořte **SimulatedDevice.js** souboru v hello **simulateddevice** složky.</span><span class="sxs-lookup"><span data-stu-id="22963-165">Using a text editor, create a **SimulatedDevice.js** file in hello **simulateddevice** folder.</span></span>
4. <span data-ttu-id="22963-166">Přidejte následující hello `require` příkazy v hello začátek hello **SimulatedDevice.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="22963-166">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. <span data-ttu-id="22963-167">Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance.</span><span class="sxs-lookup"><span data-stu-id="22963-167">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="22963-168">Nahraďte **{youriothostname}** s názvem hello hello IoT hub vytvořili hello *vytvoření služby IoT Hub* části.</span><span class="sxs-lookup"><span data-stu-id="22963-168">Replace **{youriothostname}** with hello name of hello IoT hub you created hello *Create an IoT Hub* section.</span></span> <span data-ttu-id="22963-169">Nahraďte **{yourdevicekey}** s hodnotou klíče hello zařízení jste vygenerovali v hello *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="22963-169">Replace **{yourdevicekey}** with hello device key value you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. <span data-ttu-id="22963-170">Přidejte následující funkce toodisplay výstup z aplikace hello hello:</span><span class="sxs-lookup"><span data-stu-id="22963-170">Add hello following function toodisplay output from hello application:</span></span>
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. <span data-ttu-id="22963-171">Vytvořte zpětné volání a použijte hello **setInterval** funkce každou sekundu toosend zpráva tooyour IoT hub:</span><span class="sxs-lookup"><span data-stu-id="22963-171">Create a callback and use hello **setInterval** function toosend a message tooyour IoT hub every second:</span></span>
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
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
8. <span data-ttu-id="22963-172">Otevřete hello připojení tooyour IoT Hub a začněte posílat zprávy:</span><span class="sxs-lookup"><span data-stu-id="22963-172">Open hello connection tooyour IoT Hub and start sending messages:</span></span>
   
    ```
    client.open(connectCallback);
    ```
9. <span data-ttu-id="22963-173">Uložte a zavřete hello **SimulatedDevice.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="22963-173">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="22963-174">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="22963-174">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="22963-175">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="22963-175">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="run-hello-apps"></a><span data-ttu-id="22963-176">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="22963-176">Run hello apps</span></span>
<span data-ttu-id="22963-177">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="22963-177">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="22963-178">Na příkazovém řádku v hello **readdevicetocloudmessages** složky, spusťte následující příkaz toobegin monitorovat služba IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="22963-178">At a command prompt in hello **readdevicetocloudmessages** folder, run hello following command toobegin monitoring your IoT hub:</span></span>
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![Zařízení cloud toomonitor zprávy pro Node.js IoT Hub služby aplikace][7]
2. <span data-ttu-id="22963-180">Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz toobegin odesílání telemetrických dat tooyour IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="22963-180">At a command prompt in hello **simulateddevice** folder, run hello following command toobegin sending telemetry data tooyour IoT hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![Node.js IoT Hub zařízení aplikaci toosend zařízení cloud zprávy][8]
3. <span data-ttu-id="22963-182">Hello **využití** dlaždici v hello [portál Azure] [ lnk-portal] ukazuje hello počet zpráv odeslaných toohello služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="22963-182">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>
   
    ![Azure portálu využití dlaždice zobrazuje počet zpráv odeslaných tooIoT rozbočovače][43]

## <a name="next-steps"></a><span data-ttu-id="22963-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22963-184">Next steps</span></span>
<span data-ttu-id="22963-185">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="22963-185">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="22963-186">Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci toosend zpráv typu zařízení cloud toohello Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="22963-186">You used this device identity tooenable hello simulated device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="22963-187">Můžete také vytvořit aplikaci, která zobrazuje hello zprávy přijaté službou hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="22963-187">You also created an app that displays hello messages received by hello IoT hub.</span></span> 

<span data-ttu-id="22963-188">toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:</span><span class="sxs-lookup"><span data-stu-id="22963-188">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="22963-189">[Připojení zařízení][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="22963-189">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="22963-190">[Začínáme se správou zařízení][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="22963-190">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="22963-191">[Začínáme se službou Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="22963-191">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="22963-192">toolearn jak tooextend zpráv IoT řešení a proces zařízení cloud ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="22963-192">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
