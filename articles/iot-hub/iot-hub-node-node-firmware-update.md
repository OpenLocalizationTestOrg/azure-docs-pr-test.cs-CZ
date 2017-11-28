---
title: "Aktualizace firmwaru zařízení s Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak používat správu zařízení v Azure IoT Hub zahájíte aktualizaci firmwaru zařízení. Použijete k implementaci aplikace simulovaného zařízení a aplikaci služby, která spustí aktualizaci firmwaru SDK služby Azure IoT pro Node.js."
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
ms.openlocfilehash: 350cf1cbec8847d1bbf29814435502af6f098e54
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-nodenode"></a><span data-ttu-id="1951a-104">Použití správy zařízení za účelem zahájení aktualizaci firmwaru zařízení (uzel nebo uzel)</span><span class="sxs-lookup"><span data-stu-id="1951a-104">Use device management to initiate a device firmware update (Node/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="1951a-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="1951a-105">Introduction</span></span>
<span data-ttu-id="1951a-106">V [Začínáme se správou zařízení] [ lnk-dm-getstarted] kurz, jste viděli, jak používat [dvojče zařízení] [ lnk-devtwin] a [přímé metody ] [ lnk-c2dmethod] primitiv vzdáleně restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="1951a-106">In the [Get started with device management][lnk-dm-getstarted] tutorial, you saw how to use the [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives to remotely reboot a device.</span></span> <span data-ttu-id="1951a-107">Tento kurz používá stejné primitiv IoT Hub a poskytuje pokyny a ukazuje, jak provést aktualizaci firmwaru simulované začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="1951a-107">This tutorial uses the same IoT Hub primitives and provides guidance and shows you how to do an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="1951a-108">Tento vzor slouží k implementaci aktualizace firmwaru pro vzorovou Intel Edison zařízení.</span><span class="sxs-lookup"><span data-stu-id="1951a-108">This pattern is used in the firmware update implementation for the Intel Edison device sample.</span></span>

<span data-ttu-id="1951a-109">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="1951a-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="1951a-110">Vytvořte konzolovou aplikaci softwaru Node.js, která volá metodu firmwareUpdate přímé v aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1951a-110">Create a Node.js console app that calls the firmwareUpdate direct method in the simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="1951a-111">Vytvoření aplikace simulovaného zařízení, která implementuje **firmwareUpdate** přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="1951a-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="1951a-112">Tato metoda inicializuje více fáze procesu, který čeká na stažení bitové kopie firmwaru, stáhne bitovou kopii firmware a nakonec platí bitovou kopii firmwaru.</span><span class="sxs-lookup"><span data-stu-id="1951a-112">This method initiates a multi-stage process that waits to download the firmware image, downloads the firmware image, and finally applies the firmware image.</span></span> <span data-ttu-id="1951a-113">Během aktualizace v každé fázi používá zařízení hlášené vlastnosti hlásit průběh.</span><span class="sxs-lookup"><span data-stu-id="1951a-113">During each stage of the update, the device uses the reported properties to report on progress.</span></span>

<span data-ttu-id="1951a-114">Na konci tohoto kurzu máte dvě aplikace konzoly Node.js:</span><span class="sxs-lookup"><span data-stu-id="1951a-114">At the end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="1951a-115">**dmpatterns_fwupdate_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení, zobrazí odpověď a pravidelně (každých 500ms) zobrazí aktualizovaná hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1951a-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in the simulated device app, displays the response, and periodically (every 500ms) displays the updated reported properties.</span></span>

<span data-ttu-id="1951a-116">**dmpatterns_fwupdate_device.js**, který se připojí ke službě IoT hub s identitou zařízení vytvořenou dříve, dostane přímá metoda firmwareUpdate, spustí prostřednictvím více stavu procesu k simulaci, včetně aktualizace firmwaru: čekání bitové kopie stažení, stahování novou bitovou kopii a nakonec použitím bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="1951a-116">**dmpatterns_fwupdate_device.js**, which connects to your IoT hub with the device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process to simulate a firmware update including: waiting for the image download, downloading the new image, and finally applying the image.</span></span>

<span data-ttu-id="1951a-117">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="1951a-117">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="1951a-118">Node.js verze 0.12.x nebo novější,</span><span class="sxs-lookup"><span data-stu-id="1951a-118">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="1951a-119">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje postup instalace Node.js pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="1951a-119">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="1951a-120">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1951a-120">An active Azure account.</span></span> <span data-ttu-id="1951a-121">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="1951a-121">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="1951a-122">Postupujte podle [Začínáme se správou zařízení](iot-hub-node-node-device-management-get-started.md) článek k vytvoření služby IoT hub a získat připojovací řetězec služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1951a-122">Follow the [Get started with device management](iot-hub-node-node-device-management-get-started.md) article to create your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a><span data-ttu-id="1951a-123">Spustit aktualizaci vzdálené firmwaru v zařízení s přímá metoda</span><span class="sxs-lookup"><span data-stu-id="1951a-123">Trigger a remote firmware update on the device using a direct method</span></span>
<span data-ttu-id="1951a-124">V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje aktualizace vzdálené firmwaru v zařízení.</span><span class="sxs-lookup"><span data-stu-id="1951a-124">In this section, you create a Node.js console app that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="1951a-125">Přímá metoda používá k zahájení aktualizace a aplikace používá zařízení twin dotazy a pravidelně získat stav aktualizace active firmware.</span><span class="sxs-lookup"><span data-stu-id="1951a-125">The app uses a direct method to initiate the update and uses device twin queries to periodically get the status of the active firmware update.</span></span>

1. <span data-ttu-id="1951a-126">Vytvořit prázdnou složku s názvem **triggerfwupdateondevice**.</span><span class="sxs-lookup"><span data-stu-id="1951a-126">Create an empty folder called **triggerfwupdateondevice**.</span></span>  <span data-ttu-id="1951a-127">V **triggerfwupdateondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="1951a-127">In the **triggerfwupdateondevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="1951a-128">Přijměte všechny výchozí hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1951a-128">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="1951a-129">Na příkazovém řádku v **triggerfwupdateondevice** složky, spusťte následující příkaz k instalaci **azure-iot-hub** a **azure-iot zařízení mqtt** zařízení SDK balíčky:</span><span class="sxs-lookup"><span data-stu-id="1951a-129">At your command prompt in the **triggerfwupdateondevice** folder, run the following command to install the **azure-iot-hub** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="1951a-130">Pomocí textového editoru, vytvořte **dmpatterns_getstarted_service.js** v soubor **triggerfwupdateondevice** složky.</span><span class="sxs-lookup"><span data-stu-id="1951a-130">Using a text editor, create a **dmpatterns_getstarted_service.js** file in the **triggerfwupdateondevice** folder.</span></span>
4. <span data-ttu-id="1951a-131">Přidejte následující příkazy na začátku "vyžadovat" **dmpatterns_getstarted_service.js** souboru:</span><span class="sxs-lookup"><span data-stu-id="1951a-131">Add the following 'require' statements at the start of the **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="1951a-132">Přidejte následující deklarace proměnných a nahraďte zástupné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1951a-132">Add the following variable declarations and replace the placeholder values:</span></span>
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. <span data-ttu-id="1951a-133">Přidejte následující funkci k vyhledání a zobrazení hodnotu firmwareUpdate jsou uvedeny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1951a-133">Add the following function to find and display the value of the firmwareUpdate reported property.</span></span>
   
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
7. <span data-ttu-id="1951a-134">Přidejte následující funkci k vyvolání metody firmwareUpdate restartování cílového zařízení:</span><span class="sxs-lookup"><span data-stu-id="1951a-134">Add the following function to invoke the firmwareUpdate method to reboot the target device:</span></span>
   
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
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```
8. <span data-ttu-id="1951a-135">Nakonec přidejte následující funkci k kód pro spuštění pořadí aktualizací firmwaru a spusťte pravidelně zobrazuje hlášené vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1951a-135">Finally, Add the following function to code to start the firmware update sequence and start periodically showing the reported properties:</span></span>
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. <span data-ttu-id="1951a-136">Uložte a zavřete **dmpatterns_fwupdate_service.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="1951a-136">Save and close the **dmpatterns_fwupdate_service.js** file.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a><span data-ttu-id="1951a-137">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="1951a-137">Run the apps</span></span>
<span data-ttu-id="1951a-138">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="1951a-138">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="1951a-139">Na příkazovém řádku v **manageddevice** složky, spusťte následující příkaz, aby začal přijímat přímá metoda restartování.</span><span class="sxs-lookup"><span data-stu-id="1951a-139">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="1951a-140">Na příkazovém řádku v **triggerfwupdateondevice** složky, spusťte následující příkaz k aktivační události restartovat vzdálený počítač a dotaz pro dvojče zařízení najít poslední čas restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="1951a-140">At the command prompt in the **triggerfwupdateondevice** folder, run the following command to trigger the remote reboot and query for the device twin to find the last reboot time.</span></span>
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. <span data-ttu-id="1951a-141">Zobrazí zařízení odpověď na přímá metoda v konzole.</span><span class="sxs-lookup"><span data-stu-id="1951a-141">You see the device response to the direct method in the console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1951a-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1951a-142">Next steps</span></span>
<span data-ttu-id="1951a-143">V tomto kurzu přímá metoda používá k aktivaci aktualizace vzdálené firmwaru v zařízení a umožňuje sledovat průběh aktualizace firmwaru hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1951a-143">In this tutorial, you used a direct method to trigger a remote firmware update on a device and used the reported properties to follow the progress of the firmware update.</span></span>

<span data-ttu-id="1951a-144">Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="1951a-144">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
