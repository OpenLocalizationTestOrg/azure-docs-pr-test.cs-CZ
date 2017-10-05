---
title: "Použití dynamické telemetrie | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak používat dynamické telemetrie s Azure IoT Suite předkonfigurovanému řešení vzdáleného monitorování."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 0114f27f9b8ae76e1170d04ddf66e2c4bf20686a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="347ad-103">Použití dynamické telemetrie s předkonfigurovaného řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="347ad-103">Use dynamic telemetry with the remote monitoring preconfigured solution</span></span>

<span data-ttu-id="347ad-104">Dynamické telemetrie umožňuje vizualizovat všechny telemetrická data odesílaná do předkonfigurovaného řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="347ad-104">Dynamic telemetry enables you to visualize any telemetry sent to the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="347ad-105">Simulovaná zařízení, které nasadit s předkonfigurovaným řešením odesílat telemetrii teploty a vlhkosti, který můžete vizualizovat na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="347ad-105">The simulated devices that deploy with the preconfigured solution send temperature and humidity telemetry, which you can visualize on the dashboard.</span></span> <span data-ttu-id="347ad-106">Je-li přizpůsobit existující Simulovaná zařízení, vytvořte nové simulované zařízení nebo připojení fyzického zařízení s předkonfigurovaným řešením můžete odeslat další hodnoty telemetrie například externí teplotu, ot. / min nebo větru.</span><span class="sxs-lookup"><span data-stu-id="347ad-106">If you customize existing simulated devices, create new simulated devices, or connect physical devices to the preconfigured solution you can send other telemetry values such as the external temperature, RPM, or windspeed.</span></span> <span data-ttu-id="347ad-107">Potom můžete vizualizovat tuto další telemetrii na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="347ad-107">You can then visualize this additional telemetry on the dashboard.</span></span>

<span data-ttu-id="347ad-108">Tento kurz používá jednoduchý Node.js simulovaného zařízení, která můžete snadno upravit a experimentovat s dynamické telemetrie.</span><span class="sxs-lookup"><span data-stu-id="347ad-108">This tutorial uses a simple Node.js simulated device that you can easily modify to experiment with dynamic telemetry.</span></span>

<span data-ttu-id="347ad-109">K dokončení tohoto kurzu, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="347ad-109">To complete this tutorial, you’ll need:</span></span>

* <span data-ttu-id="347ad-110">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="347ad-110">An active Azure subscription.</span></span> <span data-ttu-id="347ad-111">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="347ad-111">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="347ad-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="347ad-112">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="347ad-113">[Node.js] [ lnk-node] verze 0.12.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="347ad-113">[Node.js][lnk-node] version 0.12.x or later.</span></span>

<span data-ttu-id="347ad-114">Tento kurz v jakémkoliv operačním systému, jako je například systému Windows nebo Linux, kde můžete nainstalovat Node.js.</span><span class="sxs-lookup"><span data-stu-id="347ad-114">You can complete this tutorial on any operating system, such as Windows or Linux, where you can install Node.js.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a><span data-ttu-id="347ad-115">Přidání typu telemetrie</span><span class="sxs-lookup"><span data-stu-id="347ad-115">Add a telemetry type</span></span>

<span data-ttu-id="347ad-116">Dalším krokem je nahradit telemetrii vygenerovanou simulované zařízení Node.js s novou sadu hodnot:</span><span class="sxs-lookup"><span data-stu-id="347ad-116">The next step is to replace the telemetry generated by the Node.js simulated device with a new set of values:</span></span>

1. <span data-ttu-id="347ad-117">Zastavit simulované zařízení Node.js zadáním **Ctrl + C** v příkazovém řádku nebo prostředí.</span><span class="sxs-lookup"><span data-stu-id="347ad-117">Stop the Node.js simulated device by typing **Ctrl+C** in your command prompt or shell.</span></span>
2. <span data-ttu-id="347ad-118">V souboru remote_monitoring.js uvidíte základní datových hodnot pro existující teploty, vlhkosti a externí teplotní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="347ad-118">In the remote_monitoring.js file, you can see the base data values for the existing temperature, humidity, and external temperature telemetry.</span></span> <span data-ttu-id="347ad-119">Přidejte hodnotu základní data pro **rpm** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="347ad-119">Add a base data value for **rpm** as follows:</span></span>

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. <span data-ttu-id="347ad-120">Node.js simulované zařízení používá **generateRandomIncrement** funkce v souboru remote_monitoring.js přidejte náhodný přírůstek hodnoty základní data.</span><span class="sxs-lookup"><span data-stu-id="347ad-120">The Node.js simulated device uses the **generateRandomIncrement** function in the remote_monitoring.js file to add a random increment to the base data values.</span></span> <span data-ttu-id="347ad-121">Náhodně přeskupit **rpm** hodnota přidáním řádek kódu po existující randomizations následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="347ad-121">Randomize the **rpm** value by adding a line of code after the existing randomizations as follows:</span></span>

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. <span data-ttu-id="347ad-122">Přidejte novou hodnotu rpm do datové části JSON, které zařízení odesílá do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="347ad-122">Add the new rpm value to the JSON payload the device sends to IoT Hub:</span></span>

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. <span data-ttu-id="347ad-123">Spusťte simulované zařízení Node.js pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="347ad-123">Run the Node.js simulated device using the following command:</span></span>

    `node remote_monitoring.js`

6. <span data-ttu-id="347ad-124">Sledujte nový typ telemetrie ot. / min, který se zobrazí v grafu v řídicím panelu:</span><span class="sxs-lookup"><span data-stu-id="347ad-124">Observe the new RPM telemetry type that displays on the chart in the dashboard:</span></span>

![Přidat RPM na řídicí panel][image3]

> [!NOTE]
> <span data-ttu-id="347ad-126">Budete muset zakažte a znovu povolte Node.js zařízení na **zařízení** stránky v řídicím panelu se projeví okamžitě.</span><span class="sxs-lookup"><span data-stu-id="347ad-126">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="customize-the-dashboard-display"></a><span data-ttu-id="347ad-127">Přizpůsobení zobrazení řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="347ad-127">Customize the dashboard display</span></span>

<span data-ttu-id="347ad-128">**Informace o zařízení** zprávy může obsahovat metadata o telemetrie zařízení může odesílat do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="347ad-128">The **Device-Info** message can include metadata about the telemetry the device can send to IoT Hub.</span></span> <span data-ttu-id="347ad-129">Tato metadata můžete určit typy telemetrických dat, které zařízení odesílá.</span><span class="sxs-lookup"><span data-stu-id="347ad-129">This metadata can specify the telemetry types the device sends.</span></span> <span data-ttu-id="347ad-130">Změnit **deviceMetaData** hodnota v souboru remote_monitoring.js **Telemetrie** následující definice **příkazy** definice.</span><span class="sxs-lookup"><span data-stu-id="347ad-130">Modify the **deviceMetaData** value in the remote_monitoring.js file to include a **Telemetry** definition following the **Commands** definition.</span></span> <span data-ttu-id="347ad-131">Následující fragment kódu ukazuje kód **příkazy** definice (Nezapomeňte přidat `,` po **příkazy** definice):</span><span class="sxs-lookup"><span data-stu-id="347ad-131">The following code snippet shows the **Commands** definition (be sure to add a `,` after the **Commands** definition):</span></span>

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> <span data-ttu-id="347ad-132">Pro porovnání definici metadat s daty v datovém proudu telemetrických dat používá řešení vzdáleného monitorování porovnávání.</span><span class="sxs-lookup"><span data-stu-id="347ad-132">The remote monitoring solution uses a case-insensitive match to compare the metadata definition with data in the telemetry stream.</span></span>


<span data-ttu-id="347ad-133">Přidání **Telemetrie** definice, jak je uvedeno v předchozím fragmentu kódu nezmění chování řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="347ad-133">Adding a **Telemetry** definition as shown in the preceding code snippet does not change the behavior of the dashboard.</span></span> <span data-ttu-id="347ad-134">Však může také obsahovat metadata **DisplayName** atribut pro přizpůsobení zobrazení v řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="347ad-134">However, the metadata can also include a **DisplayName** attribute to customize the display in the dashboard.</span></span> <span data-ttu-id="347ad-135">Aktualizace **Telemetrie** definice metadat, jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="347ad-135">Update the **Telemetry** metadata definition as shown in the following snippet:</span></span>

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

<span data-ttu-id="347ad-136">Následující snímek obrazovky ukazuje, jak tuto změnu upraví legendu grafu na řídicím panelu:</span><span class="sxs-lookup"><span data-stu-id="347ad-136">The following screenshot shows how this change modifies the chart legend on the dashboard:</span></span>

![Přizpůsobení legendu grafu][image4]

> [!NOTE]
> <span data-ttu-id="347ad-138">Budete muset zakažte a znovu povolte Node.js zařízení na **zařízení** stránky v řídicím panelu se projeví okamžitě.</span><span class="sxs-lookup"><span data-stu-id="347ad-138">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="filter-the-telemetry-types"></a><span data-ttu-id="347ad-139">Filtroval typy telemetrie</span><span class="sxs-lookup"><span data-stu-id="347ad-139">Filter the telemetry types</span></span>

<span data-ttu-id="347ad-140">Ve výchozím nastavení graf na řídicí panel zobrazuje každé datové řady v datový proud telemetrie.</span><span class="sxs-lookup"><span data-stu-id="347ad-140">By default, the chart on the dashboard shows every data series in the telemetry stream.</span></span> <span data-ttu-id="347ad-141">Můžete použít **informace o zařízení** metadata potlačit zobrazení telemetrie konkrétní typy v grafu.</span><span class="sxs-lookup"><span data-stu-id="347ad-141">You can use the **Device-Info** metadata to suppress the display of specific telemetry types on the chart.</span></span> 

<span data-ttu-id="347ad-142">Chcete-li zobrazit pouze teploty a vlhkosti telemetrie grafu, vynechejte **ExternalTemperature** z **informace o zařízení** **Telemetrie** metadata následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="347ad-142">To make the chart show only Temperature and Humidity telemetry, omit **ExternalTemperature** from the **Device-Info** **Telemetry** metadata as follows:</span></span>

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

<span data-ttu-id="347ad-143">**Venku teploty** už nebude zobrazovat na graf:</span><span class="sxs-lookup"><span data-stu-id="347ad-143">The **Outdoor Temperature** no longer displays on the chart:</span></span>

![Filtrovat telemetrii na řídicím panelu][image5]

<span data-ttu-id="347ad-145">Tato změna ovlivňuje pouze zobrazení grafu.</span><span class="sxs-lookup"><span data-stu-id="347ad-145">This change only affects the chart display.</span></span> <span data-ttu-id="347ad-146">**ExternalTemperature** hodnoty data jsou stále uloženy a k dispozici pro zpracování všech back-end.</span><span class="sxs-lookup"><span data-stu-id="347ad-146">The **ExternalTemperature** data values are still stored and made available for any backend processing.</span></span>

> [!NOTE]
> <span data-ttu-id="347ad-147">Budete muset zakažte a znovu povolte Node.js zařízení na **zařízení** stránky v řídicím panelu se projeví okamžitě.</span><span class="sxs-lookup"><span data-stu-id="347ad-147">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="347ad-148">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="347ad-148">Handle errors</span></span>

<span data-ttu-id="347ad-149">Pro datový proud pro zobrazení v grafu jeho **typ** v **informace o zařízení** metadata musí odpovídat datovému typu hodnot telemetrie.</span><span class="sxs-lookup"><span data-stu-id="347ad-149">For a data stream to display on the chart, its **Type** in the **Device-Info** metadata must match the data type of the telemetry values.</span></span> <span data-ttu-id="347ad-150">Například, pokud metadata Určuje, že **typ** vlhkosti dat je **int** a **dvojité** nenajde v datovém proudu telemetrických dat, pak telemetrie vlhkosti nezobrazí. v grafu.</span><span class="sxs-lookup"><span data-stu-id="347ad-150">For example, if the metadata specifies that the **Type** of humidity data is **int** and a **double** is found in the telemetry stream then the humidity telemetry does not display on the chart.</span></span> <span data-ttu-id="347ad-151">Ale **vlhkosti** hodnoty jsou stále uloženy a k dispozici pro zpracování všech back-end.</span><span class="sxs-lookup"><span data-stu-id="347ad-151">However, the **Humidity** values are still stored and made available for any back-end processing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="347ad-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="347ad-152">Next steps</span></span>

<span data-ttu-id="347ad-153">Teď, když už víte, jak používat dynamické telemetrie, další informace o tom, jak předkonfigurovaná řešení využívají informace o zařízení: [informace metadat zařízení ve vzdálené monitorování předkonfigurované řešení] [ lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="347ad-153">Now that you've seen how to use dynamic telemetry, you can learn more about how the preconfigured solutions use device information: [Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
