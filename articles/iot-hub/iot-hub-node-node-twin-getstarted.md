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
# <a name="get-started-with-device-twins-node"></a>Začínáme s dvojčata zařízení (uzel)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Na konci hello tohoto kurzu budete mít dvě aplikace konzoly Node.js:

* **AddTagsAndQuery.js**, aplikace back-end Node.js, které přidá značky a dotazuje dvojčata zařízení.
* **TwinSimulatedDevice.js**, aplikace Node.js, která simuluje zařízení, která se připojuje tooyour IoT hub s dříve vytvořenou identitou zařízení hello a sestav stavu připojení.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.
> 
> 

toocomplete tohoto kurzu potřebujete následující hello:

* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a>Vytvoření aplikace hello service
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která přidává umístění metadat toohello dvojče zařízení spojené s **myDeviceId**. Následně dotazů uložená ve výběru hello zařízení nachází v hello hello IoT hub nám hello dvojčata zařízení a pak hello ta, která podávají zprávy mobilní připojení.

1. Vytvořit novou prázdnou složku s názvem **addtagsandqueryapp**. V hello **addtagsandqueryapp** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **addtagsandqueryapp** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku:
   
    ```
    npm install azure-iothub --save
    ```
3. Pomocí textového editoru, vytvořte novou **AddTagsAndQuery.js** souboru v hello **addtagsandqueryapp** složky.
4. Přidejte následující kód toohello hello **AddTagsAndQuery.js** souboru a nahradit hello **{iot hub, připojovací řetězec}** zástupný symbol hello jste zkopírovali, když vytvoříte Centrum IoT Hub připojovací řetězec:
   
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
   
    Hello **registru** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello. Předchozí kód Hello nejprve inicializuje hello **registru** objektu, pak načte hello dvojče zařízení pro **myDeviceId**a nakonec aktualizuje jeho značky hello požadovaných informací o umístění.
   
    Po hello aktualizace hello značky ho volání hello **queryTwins** funkce.
5. Přidejte následující kód na konci hello hello **AddTagsAndQuery.js** tooimplement hello **queryTwins** funkce:
   
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
   
    Předchozí kód Hello provede dva dotazy: hello první vybere pouze dvojčata zařízení hello zařízení umístěných v hello **Redmond43** zařízení a hello druhý refines hello dotazu tooselect pouze hello zařízení, která jsou také připojené prostřednictvím mobilní síti.
   
    Všimněte si, že předchozí kód hello při vytváření hello **dotazu** objektu, určuje maximální počet vrácených dokumentů. Hello **dotazu** objekt obsahuje **hasMoreResults** vlastnost typu boolean, které můžete použít tooinvoke hello **nextAsTwin** metody několikrát tooretrieve všechny výsledky. Volána metoda **Další** je k dispozici pro výsledky, které nejsou například dvojčata zařízení výsledky dotazů agregace.
6. Spuštění aplikace hello se:
   
        node AddTagsAndQuery.js
   
    Měli byste vidět jeden zařízení ve výsledcích hello hello dotazu žádostí pro všechna zařízení umístěné v **Redmond43** a jeden pro hello dotaz, který omezuje hello výsledků toodevices, použít mobilní síti.
   
    ![][1]

V další části hello vytvoříte aplikaci zařízení, která hlásí informace o připojení k hello a změny hello výsledek dotazu hello v předchozí části hello.

## <a name="create-hello-device-app"></a>Vytvoření aplikace hello zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje tooyour hub jako **myDeviceId**a poté aktualizace jeho dvojče zařízení je hlášené vlastnosti toocontain hello informace, že je připojený pomocí mobilní síti.

> [!NOTE]
> V tomto okamžiku jsou přístupné pouze ze zařízení, která se připojují tooIoT rozbočovače dvojčata zařízení pomocí protokolu MQTT hello. Naleznete toohello [MQTT podporu] [ lnk-devguide-mqtt] článku pokyny, jak tooconvert existující zařízení aplikaci toouse MQTT.
> 
> 

1. Vytvořit novou prázdnou složku s názvem **reportconnectivity**. V hello **reportconnectivity** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **reportconnectivity** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device**, a **azure-iot zařízení mqtt** balíčku :
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte novou **ReportConnectivity.js** souboru v hello **reportconnectivity** složky.
4. Přidejte následující kód toohello hello **ReportConnectivity.js** souboru a nahradit hello **{zařízení připojovací řetězec}** zástupný symbol hello zařízení připojovací řetězec jste zkopírovali, když jste vytvořili hello **myDeviceId** identitu zařízení:
   
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
   
    Hello **klienta** objekt poskytuje všechny metody hello vyžadují toointeract s dvojčata zařízení z hello zařízení. Hello předchozí kód po jeho inicializuje hello **klienta** objektu, načte hello dvojče zařízení pro **myDeviceId** a aktualizuje jeho hlášené vlastnost informace o připojení k hello.
5. Spuštění aplikace hello zařízení
   
        node ReportConnectivity.js
   
    Měli byste vidět zprávu hello `twin state reported`.
6. Teď, když hello zařízení hlásí informace o jeho připojení k, mělo by se zobrazit v obou dotazy. Přejděte zpět v hello **addtagsandqueryapp** hello složky a spusťte znovu dotazy:
   
        node AddTagsAndQuery.js
   
    Tentokrát **myDeviceId** by se měla objevit v obou výsledky dotazu.
   
    ![][3]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Přidat zařízení metadat jako značky z back-end aplikace a napsali informace připojení k zařízení simulovaného zařízení aplikaci tooreport v dvojče zařízení hello. Také jste se naučili jak tooquery tyto informace pomocí dotazovacího jazyka pro hello SQL jako IoT Hub.

Použití hello následující toolearn prostředky jak pro:

* odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu
* Konfigurace zařízení požadované vlastnosti dvojče zařízení pomocí hello [použití požadovaného vlastnosti tooconfigure zařízení] [ lnk-twin-how-to-configure] kurzu
* kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.

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
