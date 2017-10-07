---
title: "Vlastnosti twin zařízení Azure IoT Hub aaaUse (uzel) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub dvojčata zařízení tooconfigure zařízení. Aplikace simulovaného zařízení a aplikace služby, která upraví konfiguraci zařízení pomocí dvojče zařízení používáte pro Node.js tooimplement hello sady SDK služby Azure IoT."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: elioda
ms.openlocfilehash: 7ebfe2dfa0876bf04fdbaceae55db76456523e8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices-node"></a>Použití požadovaného vlastnosti tooconfigure zařízení (uzel)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na konci hello tohoto kurzu budete mít dvě aplikace konzoly Node.js:

* **SimulateDeviceConfiguration.js**, aplikaci simulovaného zařízení, která čeká na aktualizace požadované konfigurace a sestavy hello stav procesu aktualizace simulované konfigurace.
* **SetDesiredConfigurationAndQuery.js**, konfigurace v zařízení požadovaného aplikace back-end Node.js, která nastaví hello a dotazy hello proces aktualizace konfigurace.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.
> 
> 

toocomplete tohoto kurzu potřebujete následující hello:

* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

Pokud jste postupovali podle hello [začít pracovat s dvojčata zařízení] [ lnk-twin-tutorial] kurzu již máte služby IoT hub a identitu zařízení, která je volána **myDeviceId**; a můžete přejít toohello [ Vytvoření aplikace simulovaného zařízení hello] [ lnk-how-to-configure-createapp] části.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení hello
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která se připojuje tooyour hub jako **myDeviceId**, čeká na aktualizace požadované konfigurace a v procesu aktualizace konfigurace hello simulated hlásí aktualizace.

1. Vytvořit novou prázdnou složku s názvem **simulatedeviceconfiguration**. V hello **simulatedeviceconfiguration** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **simulatedeviceconfiguration** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device**, a **azure-iot zařízení mqtt**balíčku:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte novou **SimulateDeviceConfiguration.js** souboru v hello **simulatedeviceconfiguration** složky.
4. Přidejte následující kód toohello hello **SimulateDeviceConfiguration.js** souboru a nahradit hello **{zařízení připojovací řetězec}** zástupný symbol hello zařízení připojovací řetězec jste zkopírovali, kdy jste Vytvořit hello **myDeviceId** identitu zařízení:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    Hello **klienta** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení z hello zařízení. Hello předchozí kód po jeho inicializuje hello **klienta** objektu, načte hello dvojče zařízení pro **myDeviceId**a připojí obslužnou rutinu pro aktualizaci hello na požadované vlastnosti. Obslužná rutina Hello ověřuje, že se požadavku na změnu skutečné konfigurace tak, že porovnáte hello configIds, pak vyvolá metodu, která spustí hello změně konfigurace.
   
    Všimněte si, že pro hello zájmu jednoduchost, předchozí kód hello používá výchozí pevně pro počáteční konfiguraci hello. Skutečné aplikace by pravděpodobně načíst tuto konfiguraci z místního úložiště.
   
   > [!IMPORTANT]
   > Události změny požadované vlastnosti jsou vždy jednou vygenerované při připojení zařízení, ujistěte se, že se skutečné změnu v hodnotě hello toocheck požadovaných vlastností před provedením jakékoli akce.
   > 
   > 
5. Přidejte následující metody před hello hello `client.open()` volání:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    Hello **initConfigChange** metoda aktualizace ohlášeny objekt twin hello místní zařízení se žádost o aktualizaci konfigurace hello vlastnosti a nastaví hello stav příliš**čekající**, pak aktualizací hello zařízení Twin hello služby. Po úspěšné aktualizaci dvojče zařízení hello, simuluje dlouhotrvající proces, který ukončí v provádění hello **completeConfigChange**. Tato metoda aktualizace hello místní zařízení twin je hlášené vlastnosti nastavení stavu hello příliš**úspěch** a odebírání hello **pendingConfig** objektu. Pak aktualizuje hello dvojče zařízení ve službě hello.
   
    Hlášen, toosave šířky pásma, jsou aktualizovány vlastnosti zadáním změnit pouze vlastnosti toobe hello (s názvem **oprava** v hello výše kódu), místo nahrazení hello celý dokument.
   
   > [!NOTE]
   > V tomto kurzu není simulovat žádné chování pro aktualizace souběžných konfigurace. Některé procesy aktualizace konfigurace může být schopný tooaccommodate změn konfigurace cílového spuštěného hello aktualizace, jiné mohou mít tooqueue je a dalších může odmítněte s chybový stav. Ujistěte se, že tooconsider hello požadované chování pro vaše konkrétní konfiguraci a přidejte odpovídající logiku hello před zahájením hello změně konfigurace.
   > 
   > 
6. Spuštění aplikace hello zařízení:
   
        node SimulateDeviceConfiguration.js
   
    Měli byste vidět zprávu hello `retrieved device twin`. Zachovat hello aplikace spuštěna.

## <a name="create-hello-service-app"></a>Vytvoření aplikace hello service
V této části vytvoříte konzolovou aplikaci softwaru Node.js hello této aktualizace *potřeby vlastnosti* na dvojče zařízení hello přidružené **myDeviceId** s nový objekt konfigurace telemetrie. Pak dotazuje dvojčata zařízení hello uložené ve hello IoT hub a ukazuje hello rozdíl mezi hello potřeby a který ohlásil konfigurace hello zařízení.

1. Vytvořit novou prázdnou složku s názvem **setdesiredandqueryapp**. V hello **setdesiredandqueryapp** složky, vytvořte nový soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **setdesiredandqueryapp** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku:
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. Pomocí textového editoru, vytvořte novou **SetDesiredAndQuery.js** souboru v hello **addtagsandqueryapp** složky.
4. Přidejte následující kód toohello hello **SetDesiredAndQuery.js** souboru a nahradit hello **{iot hub, připojovací řetězec}** zástupný symbol hello jste zkopírovali, když vytvoříte Centrum IoT Hub připojovací řetězec :
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    Hello **registru** objekt zpřístupní všechny hello metody požadované toointeract s dvojčata zařízení ze služby hello. Hello předchozí kód po jeho inicializuje hello **registru** objektu, načte hello dvojče zařízení pro **myDeviceId**a aktualizuje jeho požadované vlastnosti nový objekt konfigurace telemetrie. Poté zavolá hello **queryTwins** funkce událostí 10 sekund.

    > [!IMPORTANT]
    > Tato aplikace se dotazuje služby IoT Hub pro ilustraci každých 10 sekund. Použití dotazuje sestavy zobrazující se uživatelům toogenerate napříč mnoho zařízení a ne toodetect změny. Pokud vaše řešení vyžaduje v reálném čase oznámení o události zařízení, použijte [twin oznámení][lnk-twin-notifications].
    > 
    >.

1. Přidejte následující kód těsně před hello hello `registry.getDeviceTwin()` volání tooimplement hello **queryTwins** funkce:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    Předchozí kód dotazy Hello hello dvojčata zařízení, které jsou uložené ve hello IoT hub a výtisků hello potřeby a hlášené telemetrie konfigurace. Odkazovat toohello [IoT Hub dotazovací jazyk] [ lnk-query] toolearn jak toogenerate bohaté sestavy na všech zařízeních.
2. S **SimulateDeviceConfiguration.js** spuštěná, spusťte aplikaci hello se:
   
        node SetDesiredAndQuery.js 5m
   
    Měli byste vidět hello změnu z hlášené konfigurace **úspěch** příliš**čekající** příliš**úspěch** znova s novou aktivní hello poslat frekvenci pět minut místo 24 hodiny.
   
   > [!IMPORTANT]
   > Není zpoždění až minutu tooa mezi hello zařízení sestavy operace a výsledek dotazu hello. Toto je tooenable hello dotazu infrastruktury toowork ve velmi velkém rozsahu. tooretrieve konzistentní zobrazení twin jedno zařízení použít hello **getDeviceTwin** metoda v hello **registru** třídy.
   > 
   > 

## <a name="next-steps"></a>Další kroky
V tomto kurzu nastavíte požadované konfigurace jako *potřeby vlastnosti* z back-end aplikace a napsali toodetect aplikace simulovaného zařízení, která změnit a simulace aktualizace vícekrokový proces reporting její stav jako  *hlášené vlastnosti* toohello dvojče zařízení.

Použití hello následující toolearn prostředky jak pro:

* odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub] [ lnk-iothub-getstarted] kurzu
* naplánovat nebo provádět operace u velkých sad zařízení najdete v části hello [plán a všesměrového vysílání úlohy] [ lnk-schedule-jobs] kurzu.
* kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele), s hello [použít přímé metody] [ lnk-methods-tutorial] kurzu.

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
