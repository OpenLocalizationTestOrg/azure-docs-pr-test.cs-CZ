---
title: "aktualizace firmwaru aaaDevice službou Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak aktualizovat toouse správou zařízení ve službě Azure IoT Hub tooinitiate firmwaru zařízení. Aplikace simulovaného zařízení a aplikaci služby, která spustí aktualizaci firmwaru hello použijete pro Node.js tooimplement SDK služby Azure IoT hello."
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
ms.date: 02/06/2017
ms.author: juanpere
ms.openlocfilehash: 99d4b369e7aba334bf713e0c657e6e5d227fb691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-nodenode"></a>Použití zařízení správu tooinitiate aktualizaci firmwaru zařízení (uzel nebo uzel)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Úvod
V hello [Začínáme se správou zařízení] [ lnk-dm-getstarted] kurzu jste viděli, jak toouse hello [dvojče zařízení] [ lnk-devtwin] a [přímé metody] [ lnk-c2dmethod] tooremotely primitiv restartování zařízení. Tento kurz používá hello stejné primitiv IoT Hub a poskytuje pokyny a ukazuje, jak toodo začátku do konce simulated aktualizaci firmwaru.  Tento vzor se používá v implementaci aktualizace firmwaru hello pro ukázku zařízení Intel Edison hello.

V tomto kurzu získáte informace o následujících postupech:

* Vytvořte konzolovou aplikaci softwaru Node.js, která volá metodu přímé firmwareUpdate hello v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.
* Vytvoření aplikace simulovaného zařízení, která implementuje **firmwareUpdate** přímá metoda. Tato metoda inicializuje více fáze procesu, který čeká toodownload hello firmware image, soubory ke stažení bitové kopie hello firmware a nakonec platí hello firmware image. Během každé fáze hello aktualizace hello zařízení používá hello ohlášeny průběh tooreport vlastnosti.

Na konci hello tohoto kurzu máte dvě aplikace konzoly Node.js:

**dmpatterns_fwupdate_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení hello, zobrazí hello odpovědi a pravidelně (každých 500ms) zobrazí hello aktualizovat hlášené vlastnosti.

**dmpatterns_fwupdate_device.js**, který připojí tooyour IoT hub s dříve, vytvořenou identitou zařízení hello dostane přímá metoda firmwareUpdate, spustí prostřednictvím toosimulate více stavu procesu, včetně aktualizace firmwaru: Čekání na hello Stáhnout bitovou kopii, stahování hello novou bitovou kopii a nakonec použití hello bitové kopie.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Node.js verze 0.12.x nebo novější, <br/>  [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

Postupujte podle hello [Začínáme se správou zařízení](iot-hub-node-node-device-management-get-started.md) článek toocreate služby IoT hub a získat připojovací řetězec služby IoT Hub.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a>Aktivovat aktualizaci firmwaru vzdálené na zařízení hello pomocí přímá metoda
V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje aktualizace vzdálené firmwaru v zařízení. aplikace Hello používá s přímá metoda tooinitiate hello aktualizací a používá zařízení twin dotazy tooperiodically získat stav hello hello active firmware aktualizace.

1. Vytvořit prázdnou složku s názvem **triggerfwupdateondevice**.  V hello **triggerfwupdateondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.  Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **triggerfwupdateondevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-hub** a **azure-iot zařízení mqtt** zařízení Balíčky sady SDK:
   
    ```
    npm install azure-iothub --save
    ```
3. Pomocí textového editoru, vytvořte **dmpatterns_getstarted_service.js** souboru v hello **triggerfwupdateondevice** složky.
4. Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_getstarted_service.js** souboru:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Přidejte následující deklarace proměnných hello a nahraďte zástupné hodnoty hello:
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. Přidejte následující hello funkce toofind a zobrazit hodnotu hello hello firmwareUpdate hlášené vlastnost.
   
    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```
7. Přidejte následující funkce tooinvoke hello firmwareUpdate metoda tooreboot hello cílové zařízení hello:
   
    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
   
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
   
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
   
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start hello firmware update on hello device: ' + err.message)
        } 
      });
    };
    ```
8. Nakonec přidat hello následující funkce toocode toostart hello firmware aktualizace pořadí a začne pravidelně zobrazovat hello hlášené vlastnosti:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Uložte a zavřete hello **dmpatterns_fwupdate_service.js** souboru.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a>Spuštění aplikace hello
Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku hello v hello **manageddevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Na příkazovém řádku hello v hello **triggerfwupdateondevice** složky, spusťte následující příkaz tootrigger hello vzdálené hello restartujte počítač a dotaz hello zařízení twin toofind hello restartovat poslední čas.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste použili tootrigger přímá metoda vzdálené aktualizace firmwaru v zařízení a používané hello hlášené vlastnosti toofollow hello průběh aktualizace firmwaru hello.

toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
