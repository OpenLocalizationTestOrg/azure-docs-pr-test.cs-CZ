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
# <a name="use-device-management-tooinitiate-a-device-firmware-update-nodenode"></a><span data-ttu-id="8a967-104">Použití zařízení správu tooinitiate aktualizaci firmwaru zařízení (uzel nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="8a967-104">Use device management tooinitiate a device firmware update (Node/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="8a967-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="8a967-105">Introduction</span></span>
<span data-ttu-id="8a967-106">V hello [Začínáme se správou zařízení] [ lnk-dm-getstarted] kurzu jste viděli, jak toouse hello [dvojče zařízení] [ lnk-devtwin] a [přímé metody] [ lnk-c2dmethod] tooremotely primitiv restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a967-106">In hello [Get started with device management][lnk-dm-getstarted] tutorial, you saw how toouse hello [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives tooremotely reboot a device.</span></span> <span data-ttu-id="8a967-107">Tento kurz používá hello stejné primitiv IoT Hub a poskytuje pokyny a ukazuje, jak toodo začátku do konce simulated aktualizaci firmwaru.</span><span class="sxs-lookup"><span data-stu-id="8a967-107">This tutorial uses hello same IoT Hub primitives and provides guidance and shows you how toodo an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="8a967-108">Tento vzor se používá v implementaci aktualizace firmwaru hello pro ukázku zařízení Intel Edison hello.</span><span class="sxs-lookup"><span data-stu-id="8a967-108">This pattern is used in hello firmware update implementation for hello Intel Edison device sample.</span></span>

<span data-ttu-id="8a967-109">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="8a967-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="8a967-110">Vytvořte konzolovou aplikaci softwaru Node.js, která volá metodu přímé firmwareUpdate hello v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8a967-110">Create a Node.js console app that calls hello firmwareUpdate direct method in hello simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="8a967-111">Vytvoření aplikace simulovaného zařízení, která implementuje **firmwareUpdate** přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="8a967-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="8a967-112">Tato metoda inicializuje více fáze procesu, který čeká toodownload hello firmware image, soubory ke stažení bitové kopie hello firmware a nakonec platí hello firmware image.</span><span class="sxs-lookup"><span data-stu-id="8a967-112">This method initiates a multi-stage process that waits toodownload hello firmware image, downloads hello firmware image, and finally applies hello firmware image.</span></span> <span data-ttu-id="8a967-113">Během každé fáze hello aktualizace hello zařízení používá hello ohlášeny průběh tooreport vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8a967-113">During each stage of hello update, hello device uses hello reported properties tooreport on progress.</span></span>

<span data-ttu-id="8a967-114">Na konci hello tohoto kurzu máte dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="8a967-114">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="8a967-115">**dmpatterns_fwupdate_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení hello, zobrazí hello odpovědi a pravidelně (každých 500ms) zobrazí hello aktualizovat hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8a967-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in hello simulated device app, displays hello response, and periodically (every 500ms) displays hello updated reported properties.</span></span>

<span data-ttu-id="8a967-116">**dmpatterns_fwupdate_device.js**, který připojí tooyour IoT hub s dříve, vytvořenou identitou zařízení hello dostane přímá metoda firmwareUpdate, spustí prostřednictvím toosimulate více stavu procesu, včetně aktualizace firmwaru: Čekání na hello Stáhnout bitovou kopii, stahování hello novou bitovou kopii a nakonec použití hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="8a967-116">**dmpatterns_fwupdate_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process toosimulate a firmware update including: waiting for hello image download, downloading hello new image, and finally applying hello image.</span></span>

<span data-ttu-id="8a967-117">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="8a967-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="8a967-118">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="8a967-118">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="8a967-119">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="8a967-119">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="8a967-120">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8a967-120">An active Azure account.</span></span> <span data-ttu-id="8a967-121">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="8a967-121">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="8a967-122">Postupujte podle hello [Začínáme se správou zařízení](iot-hub-node-node-device-management-get-started.md) článek toocreate služby IoT hub a získat připojovací řetězec služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8a967-122">Follow hello [Get started with device management](iot-hub-node-node-device-management-get-started.md) article toocreate your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a><span data-ttu-id="8a967-123">Aktivovat aktualizaci firmwaru vzdálené na zařízení hello pomocí přímá metoda</span><span class="sxs-lookup"><span data-stu-id="8a967-123">Trigger a remote firmware update on hello device using a direct method</span></span>
<span data-ttu-id="8a967-124">V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje aktualizace vzdálené firmwaru v zařízení.</span><span class="sxs-lookup"><span data-stu-id="8a967-124">In this section, you create a Node.js console app that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="8a967-125">aplikace Hello používá s přímá metoda tooinitiate hello aktualizací a používá zařízení twin dotazy tooperiodically získat stav hello hello active firmware aktualizace.</span><span class="sxs-lookup"><span data-stu-id="8a967-125">hello app uses a direct method tooinitiate hello update and uses device twin queries tooperiodically get hello status of hello active firmware update.</span></span>

1. <span data-ttu-id="8a967-126">Vytvořit prázdnou složku s názvem **triggerfwupdateondevice**.</span><span class="sxs-lookup"><span data-stu-id="8a967-126">Create an empty folder called **triggerfwupdateondevice**.</span></span>  <span data-ttu-id="8a967-127">V hello **triggerfwupdateondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="8a967-127">In hello **triggerfwupdateondevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="8a967-128">Přijměte všechny výchozí hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="8a967-128">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="8a967-129">Na příkazovém řádku v hello **triggerfwupdateondevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-hub** a **azure-iot zařízení mqtt** zařízení Balíčky sady SDK:</span><span class="sxs-lookup"><span data-stu-id="8a967-129">At your command prompt in hello **triggerfwupdateondevice** folder, run hello following command tooinstall hello **azure-iot-hub** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="8a967-130">Pomocí textového editoru, vytvořte **dmpatterns_getstarted_service.js** souboru v hello **triggerfwupdateondevice** složky.</span><span class="sxs-lookup"><span data-stu-id="8a967-130">Using a text editor, create a **dmpatterns_getstarted_service.js** file in hello **triggerfwupdateondevice** folder.</span></span>
4. <span data-ttu-id="8a967-131">Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_getstarted_service.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="8a967-131">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="8a967-132">Přidejte následující deklarace proměnných hello a nahraďte zástupné hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="8a967-132">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. <span data-ttu-id="8a967-133">Přidejte následující hello funkce toofind a zobrazit hodnotu hello hello firmwareUpdate hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8a967-133">Add hello following function toofind and display hello value of hello firmwareUpdate reported property.</span></span>
   
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
7. <span data-ttu-id="8a967-134">Přidejte následující funkce tooinvoke hello firmwareUpdate metoda tooreboot hello cílové zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="8a967-134">Add hello following function tooinvoke hello firmwareUpdate method tooreboot hello target device:</span></span>
   
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
8. <span data-ttu-id="8a967-135">Nakonec přidat hello následující funkce toocode toostart hello firmware aktualizace pořadí a začne pravidelně zobrazovat hello hlášené vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8a967-135">Finally, Add hello following function toocode toostart hello firmware update sequence and start periodically showing hello reported properties:</span></span>
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. <span data-ttu-id="8a967-136">Uložte a zavřete hello **dmpatterns_fwupdate_service.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="8a967-136">Save and close hello **dmpatterns_fwupdate_service.js** file.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a><span data-ttu-id="8a967-137">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8a967-137">Run hello apps</span></span>
<span data-ttu-id="8a967-138">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a967-138">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="8a967-139">Na příkazovém řádku hello v hello **manageddevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.</span><span class="sxs-lookup"><span data-stu-id="8a967-139">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="8a967-140">Na příkazovém řádku hello v hello **triggerfwupdateondevice** složky, spusťte následující příkaz tootrigger hello vzdálené hello restartujte počítač a dotaz hello zařízení twin toofind hello restartovat poslední čas.</span><span class="sxs-lookup"><span data-stu-id="8a967-140">At hello command prompt in hello **triggerfwupdateondevice** folder, run hello following command tootrigger hello remote reboot and query for hello device twin toofind hello last reboot time.</span></span>
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. <span data-ttu-id="8a967-141">Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="8a967-141">You see hello device response toohello direct method in hello console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a967-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a967-142">Next steps</span></span>
<span data-ttu-id="8a967-143">V tomto kurzu jste použili tootrigger přímá metoda vzdálené aktualizace firmwaru v zařízení a používané hello hlášené vlastnosti toofollow hello průběh aktualizace firmwaru hello.</span><span class="sxs-lookup"><span data-stu-id="8a967-143">In this tutorial, you used a direct method tootrigger a remote firmware update on a device and used hello reported properties toofollow hello progress of hello firmware update.</span></span>

<span data-ttu-id="8a967-144">toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="8a967-144">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
