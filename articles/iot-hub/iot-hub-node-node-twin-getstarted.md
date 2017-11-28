---
title: "aaaGet začít s dvojčata zařízení Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak tooadd dvojčata zařízení Azure IoT Hub toouse značky a pak použít dotaz služby IoT Hub. Použijte pro aplikaci simulovaného zařízení hello tooimplement Node.js hello SDK služby Azure IoT a aplikační služby, které přidá značky hello a spustí hello dotazu IoT Hub."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: 314c88e4-cce1-441c-b75a-d2e08e39ae7d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.openlocfilehash: d60b8c3de85e9285e496b86e27d4ee31a0554a1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-node"></a><span data-ttu-id="bee71-104">Začínáme s dvojčata zařízení (uzel)</span><span class="sxs-lookup"><span data-stu-id="bee71-104">Get started with device twins (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="bee71-105">Na konci hello tohoto kurzu budete mít dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="bee71-105">At hello end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="bee71-106">**AddTagsAndQuery.js**, aplikace back-end Node.js, které přidá značky a dotazuje dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="bee71-106">**AddTagsAndQuery.js**, a Node.js back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="bee71-107">**TwinSimulatedDevice.js**, aplikace Node.js, která simuluje zařízení, která se připojuje tooyour IoT hub s dříve vytvořenou identitou zařízení hello a sestav stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="bee71-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="bee71-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="bee71-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="bee71-109">toocomplete tohoto kurzu potřebujete následující hello:</span><span class="sxs-lookup"><span data-stu-id="bee71-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="bee71-110">Node.js verze 0.10.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bee71-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="bee71-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="bee71-111">An active Azure account.</span></span> <span data-ttu-id="bee71-112">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="bee71-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a><span data-ttu-id="bee71-113">Vytvoření aplikace hello service</span><span class="sxs-lookup"><span data-stu-id="bee71-113">Create hello service app</span></span>
<span data-ttu-id="bee71-114">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která přidává umístění metadat toohello dvojče zařízení spojené s **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="bee71-114">In this section, you create a Node.js console app that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="bee71-115">Následně dotazů uložená ve výběru hello zařízení nachází v hello hello IoT hub nám hello dvojčata zařízení a pak hello ta, která podávají zprávy mobilní připojení.</span><span class="sxs-lookup"><span data-stu-id="bee71-115">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that are reporting a cellular connection.</span></span>

1. <span data-ttu-id="bee71-116">Vytvořit novou prázdnou složku s názvem **addtagsandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="bee71-116">Create a new empty folder called **addtagsandqueryapp**.</span></span> <span data-ttu-id="bee71-117">V hello **addtagsandqueryapp** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="bee71-117">In hello **addtagsandqueryapp** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="bee71-118">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="bee71-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="bee71-119">Na příkazovém řádku v hello **addtagsandqueryapp** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku:</span><span class="sxs-lookup"><span data-stu-id="bee71-119">At your command prompt in hello **addtagsandqueryapp** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="bee71-120">Pomocí textového editoru, vytvořte novou **AddTagsAndQuery.js** souboru v hello **addtagsandqueryapp** složky.</span><span class="sxs-lookup"><span data-stu-id="bee71-120">Using a text editor, create a new **AddTagsAndQuery.js** file in hello **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="bee71-121">Přidejte následující kód toohello hello **AddTagsAndQuery.js** souboru a nahradit hello **{iot hub, připojovací řetězec}** zástupný symbol hello jste zkopírovali, když vytvoříte Centrum IoT Hub připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="bee71-121">Add hello following code toohello **AddTagsAndQuery.js** file, and substitute hello **{iot hub connection string}** placeholder with hello IoT Hub connection string you copied when you created your hub:</span></span>
   
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
   
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   
    <span data-ttu-id="bee71-122">Hello **registru** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello.</span><span class="sxs-lookup"><span data-stu-id="bee71-122">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="bee71-123">Předchozí kód Hello nejprve inicializuje hello **registru** objektu, pak načte hello dvojče zařízení pro **myDeviceId**a nakonec aktualizuje jeho značky hello požadovaných informací o umístění.</span><span class="sxs-lookup"><span data-stu-id="bee71-123">hello previous code first initializes hello **Registry** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="bee71-124">Po hello aktualizace hello značky ho volání hello **queryTwins** funkce.</span><span class="sxs-lookup"><span data-stu-id="bee71-124">After hello updating hello tags it calls hello **queryTwins** function.</span></span>
5. <span data-ttu-id="bee71-125">Přidejte následující kód na konci hello hello **AddTagsAndQuery.js** tooimplement hello **queryTwins** funkce:</span><span class="sxs-lookup"><span data-stu-id="bee71-125">Add hello following code at hello end of  **AddTagsAndQuery.js** tooimplement hello **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
   
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   
    <span data-ttu-id="bee71-126">Předchozí kód Hello provede dva dotazy: hello první vybere pouze dvojčata zařízení hello zařízení umístěných v hello **Redmond43** zařízení a hello druhý refines hello dotazu tooselect pouze hello zařízení, která jsou také připojené prostřednictvím mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="bee71-126">hello previous code executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="bee71-127">Všimněte si, že předchozí kód hello při vytváření hello **dotazu** objektu, určuje maximální počet vrácených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="bee71-127">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="bee71-128">Hello **dotazu** objekt obsahuje **hasMoreResults** vlastnost typu boolean, které můžete použít tooinvoke hello **nextAsTwin** metody několikrát tooretrieve všechny výsledky.</span><span class="sxs-lookup"><span data-stu-id="bee71-128">hello **query** object contains a **hasMoreResults** boolean property that you can use tooinvoke hello **nextAsTwin** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="bee71-129">Volána metoda **Další** je k dispozici pro výsledky, které nejsou například dvojčata zařízení výsledky dotazů agregace.</span><span class="sxs-lookup"><span data-stu-id="bee71-129">A method called **next** is available for results that are not device twins for example, results of aggregation queries.</span></span>
6. <span data-ttu-id="bee71-130">Spuštění aplikace hello se:</span><span class="sxs-lookup"><span data-stu-id="bee71-130">Run hello application with:</span></span>
   
        node AddTagsAndQuery.js
   
    <span data-ttu-id="bee71-131">Měli byste vidět jeden zařízení ve výsledcích hello hello dotazu žádostí pro všechna zařízení umístěné v **Redmond43** a jeden pro hello dotaz, který omezuje hello výsledků toodevices, použít mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="bee71-131">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![][1]

<span data-ttu-id="bee71-132">V další části hello vytvoříte aplikaci zařízení, která hlásí informace o připojení k hello a změny hello výsledek dotazu hello v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="bee71-132">In hello next section you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="bee71-133">Vytvoření aplikace hello zařízení</span><span class="sxs-lookup"><span data-stu-id="bee71-133">Create hello device app</span></span>
<span data-ttu-id="bee71-134">V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje tooyour hub jako **myDeviceId**a poté aktualizace jeho dvojče zařízení je hlášené vlastnosti toocontain hello informace, že je připojený pomocí mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="bee71-134">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, and then updates its device twin's reported properties toocontain hello information that it is connected using a cellular network.</span></span>

> [!NOTE]
> <span data-ttu-id="bee71-135">V tomto okamžiku jsou přístupné pouze ze zařízení, která se připojují tooIoT rozbočovače dvojčata zařízení pomocí protokolu MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="bee71-135">At this time, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="bee71-136">Naleznete toohello [MQTT podporu] [ lnk-devguide-mqtt] článku pokyny, jak tooconvert existující zařízení aplikaci toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="bee71-136">Please refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>
> 
> 

1. <span data-ttu-id="bee71-137">Vytvořit novou prázdnou složku s názvem **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="bee71-137">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="bee71-138">V hello **reportconnectivity** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="bee71-138">In hello **reportconnectivity** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="bee71-139">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="bee71-139">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="bee71-140">Na příkazovém řádku v hello **reportconnectivity** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device**, a **azure-iot zařízení mqtt** balíčku :</span><span class="sxs-lookup"><span data-stu-id="bee71-140">At your command prompt in hello **reportconnectivity** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="bee71-141">Pomocí textového editoru, vytvořte novou **ReportConnectivity.js** souboru v hello **reportconnectivity** složky.</span><span class="sxs-lookup"><span data-stu-id="bee71-141">Using a text editor, create a new **ReportConnectivity.js** file in hello **reportconnectivity** folder.</span></span>
4. <span data-ttu-id="bee71-142">Přidejte následující kód toohello hello **ReportConnectivity.js** souboru a nahradit hello **{zařízení připojovací řetězec}** zástupný symbol hello zařízení připojovací řetězec jste zkopírovali, když jste vytvořili hello **myDeviceId** identitu zařízení:</span><span class="sxs-lookup"><span data-stu-id="bee71-142">Add hello following code toohello **ReportConnectivity.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    <span data-ttu-id="bee71-143">Hello **klienta** objekt poskytuje všechny metody hello vyžadují toointeract s dvojčata zařízení z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="bee71-143">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="bee71-144">Hello předchozí kód po jeho inicializuje hello **klienta** objektu, načte hello dvojče zařízení pro **myDeviceId** a aktualizuje jeho hlášené vlastnost informace o připojení k hello.</span><span class="sxs-lookup"><span data-stu-id="bee71-144">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId** and updates its reported property with hello connectivity information.</span></span>
5. <span data-ttu-id="bee71-145">Spuštění aplikace hello zařízení</span><span class="sxs-lookup"><span data-stu-id="bee71-145">Run hello device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="bee71-146">Měli byste vidět zprávu hello `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="bee71-146">You should see hello message `twin state reported`.</span></span>
6. <span data-ttu-id="bee71-147">Teď, když hello zařízení hlásí informace o jeho připojení k, mělo by se zobrazit v obou dotazy.</span><span class="sxs-lookup"><span data-stu-id="bee71-147">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="bee71-148">Přejděte zpět v hello **addtagsandqueryapp** hello složky a spusťte znovu dotazy:</span><span class="sxs-lookup"><span data-stu-id="bee71-148">Go back in hello **addtagsandqueryapp** folder and run hello queries again:</span></span>
   
        node AddTagsAndQuery.js
   
    <span data-ttu-id="bee71-149">Tentokrát **myDeviceId** by se měla objevit v obou výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="bee71-149">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][3]

## <a name="next-steps"></a><span data-ttu-id="bee71-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bee71-150">Next steps</span></span>
<span data-ttu-id="bee71-151">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="bee71-151">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="bee71-152">Přidat zařízení metadat jako značky z back-end aplikace a napsali informace připojení k zařízení simulovaného zařízení aplikaci tooreport v dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="bee71-152">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="bee71-153">Také jste se naučili jak tooquery tyto informace pomocí dotazovacího jazyka pro hello SQL jako IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bee71-153">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="bee71-154">Použití hello následující toolearn prostředky jak pro:</span><span class="sxs-lookup"><span data-stu-id="bee71-154">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="bee71-155">odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu</span><span class="sxs-lookup"><span data-stu-id="bee71-155">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="bee71-156">Konfigurace zařízení požadované vlastnosti dvojče zařízení pomocí hello [použití požadovaného vlastnosti tooconfigure zařízení] [ lnk-twin-how-to-configure] kurzu</span><span class="sxs-lookup"><span data-stu-id="bee71-156">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="bee71-157">kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="bee71-157">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
