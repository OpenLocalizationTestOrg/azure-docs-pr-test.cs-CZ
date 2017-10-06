---
title: "dynamické telemetrie aaaUse | Microsoft Docs"
description: "Postupujte podle tohoto kurzu toolearn jak toouse dynamické telemetrie s hello Azure IoT Suite vzdálené monitorování předkonfigurované řešení."
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
ms.openlocfilehash: 06cb2a370b67b4950efdfa4c7d906ac92106f4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-dynamic-telemetry-with-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="1250f-103">Použití dynamické telemetrie s hello předkonfigurovanému řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="1250f-103">Use dynamic telemetry with hello remote monitoring preconfigured solution</span></span>

<span data-ttu-id="1250f-104">Dynamické telemetrie umožňuje vám toovisualize všechny telemetrická data odesílaná toohello předkonfigurovanému řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="1250f-104">Dynamic telemetry enables you toovisualize any telemetry sent toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="1250f-105">Hello simulované zařízení, která nasazení s hello předkonfigurované řešení odesílat telemetrii teploty a vlhkosti, který můžete vizualizovat na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="1250f-105">hello simulated devices that deploy with hello preconfigured solution send temperature and humidity telemetry, which you can visualize on hello dashboard.</span></span> <span data-ttu-id="1250f-106">Pokud upravíte existující Simulovaná zařízení, vytvořte nové simulované zařízení nebo připojení fyzických zařízení toohello předkonfigurované řešení můžete odeslat další hodnoty telemetrie například externí teplotu hello, ot. / min nebo větru.</span><span class="sxs-lookup"><span data-stu-id="1250f-106">If you customize existing simulated devices, create new simulated devices, or connect physical devices toohello preconfigured solution you can send other telemetry values such as hello external temperature, RPM, or windspeed.</span></span> <span data-ttu-id="1250f-107">Potom můžete vizualizovat tuto další telemetrii na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="1250f-107">You can then visualize this additional telemetry on hello dashboard.</span></span>

<span data-ttu-id="1250f-108">Tento kurz používá jednoduchý simulované zařízení Node.js můžete snadno upravit tooexperiment s dynamické telemetrií.</span><span class="sxs-lookup"><span data-stu-id="1250f-108">This tutorial uses a simple Node.js simulated device that you can easily modify tooexperiment with dynamic telemetry.</span></span>

<span data-ttu-id="1250f-109">toocomplete tohoto kurzu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="1250f-109">toocomplete this tutorial, you’ll need:</span></span>

* <span data-ttu-id="1250f-110">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="1250f-110">An active Azure subscription.</span></span> <span data-ttu-id="1250f-111">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="1250f-111">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="1250f-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="1250f-112">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="1250f-113">[Node.js] [ lnk-node] verze 0.12.x nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1250f-113">[Node.js][lnk-node] version 0.12.x or later.</span></span>

<span data-ttu-id="1250f-114">Tento kurz v jakémkoliv operačním systému, jako je například systému Windows nebo Linux, kde můžete nainstalovat Node.js.</span><span class="sxs-lookup"><span data-stu-id="1250f-114">You can complete this tutorial on any operating system, such as Windows or Linux, where you can install Node.js.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a><span data-ttu-id="1250f-115">Přidání typu telemetrie</span><span class="sxs-lookup"><span data-stu-id="1250f-115">Add a telemetry type</span></span>

<span data-ttu-id="1250f-116">dalším krokem Hello je tooreplace hello telemetrii vygenerovanou hello Node.js simulované zařízení s novou sadu hodnot:</span><span class="sxs-lookup"><span data-stu-id="1250f-116">hello next step is tooreplace hello telemetry generated by hello Node.js simulated device with a new set of values:</span></span>

1. <span data-ttu-id="1250f-117">Zastavení hello Node.js simulované zařízení tak, že zadáte **Ctrl + C** v příkazovém řádku nebo prostředí.</span><span class="sxs-lookup"><span data-stu-id="1250f-117">Stop hello Node.js simulated device by typing **Ctrl+C** in your command prompt or shell.</span></span>
2. <span data-ttu-id="1250f-118">V souboru remote_monitoring.js hello uvidíte hello základní data hodnoty pro existující teploty hello vlhkosti a externí teplotní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1250f-118">In hello remote_monitoring.js file, you can see hello base data values for hello existing temperature, humidity, and external temperature telemetry.</span></span> <span data-ttu-id="1250f-119">Přidejte hodnotu základní data pro **rpm** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1250f-119">Add a base data value for **rpm** as follows:</span></span>

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. <span data-ttu-id="1250f-120">Simulovaná zařízení Node.js Hello používá hello **generateRandomIncrement** v hello remote_monitoring.js soubor tooadd toohello náhodných přírůstek fungovat základní datových hodnot.</span><span class="sxs-lookup"><span data-stu-id="1250f-120">hello Node.js simulated device uses hello **generateRandomIncrement** function in hello remote_monitoring.js file tooadd a random increment toohello base data values.</span></span> <span data-ttu-id="1250f-121">Náhodně přeskupit hello **rpm** hodnota přidáním řádek kódu po existující randomizations hello:</span><span class="sxs-lookup"><span data-stu-id="1250f-121">Randomize hello **rpm** value by adding a line of code after hello existing randomizations as follows:</span></span>

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. <span data-ttu-id="1250f-122">Přidejte hello nové rpm hodnota toohello JSON datové části hello zařízení odesílá tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="1250f-122">Add hello new rpm value toohello JSON payload hello device sends tooIoT Hub:</span></span>

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. <span data-ttu-id="1250f-123">Spusťte hello Node.js simulované zařízení pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1250f-123">Run hello Node.js simulated device using hello following command:</span></span>

    `node remote_monitoring.js`

6. <span data-ttu-id="1250f-124">Sledujte hello nový RPM telemetrie typ, který zobrazí v grafu hello hello řídicím panelu:</span><span class="sxs-lookup"><span data-stu-id="1250f-124">Observe hello new RPM telemetry type that displays on hello chart in hello dashboard:</span></span>

![Přidat řídicí panel toohello ot. / min][image3]

> [!NOTE]
> <span data-ttu-id="1250f-126">Můžete třeba toodisable a pak povolte zařízení Node.js hello na hello **zařízení** stránky v změn hello toosee hello řídicí panel okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1250f-126">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="customize-hello-dashboard-display"></a><span data-ttu-id="1250f-127">Přizpůsobení zobrazení řídicího panelu hello</span><span class="sxs-lookup"><span data-stu-id="1250f-127">Customize hello dashboard display</span></span>

<span data-ttu-id="1250f-128">Hello **informace o zařízení** zprávy může obsahovat metadata o hello telemetrie může posílat hello zařízení tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="1250f-128">hello **Device-Info** message can include metadata about hello telemetry hello device can send tooIoT Hub.</span></span> <span data-ttu-id="1250f-129">Tato metadata můžete určit typy hello telemetrie, které odesílá hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="1250f-129">This metadata can specify hello telemetry types hello device sends.</span></span> <span data-ttu-id="1250f-130">Upravit hello **deviceMetaData** hodnota v hello remote_monitoring.js soubor tooinclude **Telemetrie** definice následující hello **příkazy** definice.</span><span class="sxs-lookup"><span data-stu-id="1250f-130">Modify hello **deviceMetaData** value in hello remote_monitoring.js file tooinclude a **Telemetry** definition following hello **Commands** definition.</span></span> <span data-ttu-id="1250f-131">Hello následující fragment kódu ukazuje hello **příkazy** definice (být jisti tooadd `,` po hello **příkazy** definice):</span><span class="sxs-lookup"><span data-stu-id="1250f-131">hello following code snippet shows hello **Commands** definition (be sure tooadd a `,` after hello **Commands** definition):</span></span>

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
> <span data-ttu-id="1250f-132">řešení vzdáleného monitorování Hello používá definice metadat hello toocompare velká a malá písmena shodu s daty v datovém proudu telemetrických dat hello.</span><span class="sxs-lookup"><span data-stu-id="1250f-132">hello remote monitoring solution uses a case-insensitive match toocompare hello metadata definition with data in hello telemetry stream.</span></span>


<span data-ttu-id="1250f-133">Přidání **Telemetrie** definice, jak je znázorněno v hello předchozím fragmentu kódu nezmění chování hello hello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="1250f-133">Adding a **Telemetry** definition as shown in hello preceding code snippet does not change hello behavior of hello dashboard.</span></span> <span data-ttu-id="1250f-134">Však může také obsahovat hello metadata **DisplayName** atribut toocustomize hello zobrazení v řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="1250f-134">However, hello metadata can also include a **DisplayName** attribute toocustomize hello display in hello dashboard.</span></span> <span data-ttu-id="1250f-135">Aktualizace hello **Telemetrie** definice metadat, jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="1250f-135">Update hello **Telemetry** metadata definition as shown in hello following snippet:</span></span>

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

<span data-ttu-id="1250f-136">Hello následující snímek obrazovky ukazuje, jak tuto změnu upraví legendy grafu hello na řídicím panelu hello:</span><span class="sxs-lookup"><span data-stu-id="1250f-136">hello following screenshot shows how this change modifies hello chart legend on hello dashboard:</span></span>

![Přizpůsobení legendy grafu hello][image4]

> [!NOTE]
> <span data-ttu-id="1250f-138">Můžete třeba toodisable a pak povolte zařízení Node.js hello na hello **zařízení** stránky v změn hello toosee hello řídicí panel okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1250f-138">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="filter-hello-telemetry-types"></a><span data-ttu-id="1250f-139">Filtrovat hello telemetrie typy</span><span class="sxs-lookup"><span data-stu-id="1250f-139">Filter hello telemetry types</span></span>

<span data-ttu-id="1250f-140">Ve výchozím nastavení hello graf na řídicí panel hello znázorňuje všechny datové řady v datovém proudu telemetrických dat hello.</span><span class="sxs-lookup"><span data-stu-id="1250f-140">By default, hello chart on hello dashboard shows every data series in hello telemetry stream.</span></span> <span data-ttu-id="1250f-141">Můžete použít hello **informace o zařízení** metadata toosuppress hello zobrazení telemetrie konkrétní typy v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="1250f-141">You can use hello **Device-Info** metadata toosuppress hello display of specific telemetry types on hello chart.</span></span> 

<span data-ttu-id="1250f-142">Graf hello toomake zobrazit pouze teploty a vlhkosti telemetrie, vynechejte **ExternalTemperature** z hello **informace o zařízení** **Telemetrie** metadata následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1250f-142">toomake hello chart show only Temperature and Humidity telemetry, omit **ExternalTemperature** from hello **Device-Info** **Telemetry** metadata as follows:</span></span>

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

<span data-ttu-id="1250f-143">Hello **venku teploty** už nebude zobrazovat v grafu hello:</span><span class="sxs-lookup"><span data-stu-id="1250f-143">hello **Outdoor Temperature** no longer displays on hello chart:</span></span>

![Filtr hello telemetrii na řídicím panelu hello][image5]

<span data-ttu-id="1250f-145">Tato změna ovlivňuje pouze zobrazení grafu hello.</span><span class="sxs-lookup"><span data-stu-id="1250f-145">This change only affects hello chart display.</span></span> <span data-ttu-id="1250f-146">Hello **ExternalTemperature** hodnoty data jsou stále uloženy a k dispozici pro zpracování všech back-end.</span><span class="sxs-lookup"><span data-stu-id="1250f-146">hello **ExternalTemperature** data values are still stored and made available for any backend processing.</span></span>

> [!NOTE]
> <span data-ttu-id="1250f-147">Můžete třeba toodisable a pak povolte zařízení Node.js hello na hello **zařízení** stránky v změn hello toosee hello řídicí panel okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1250f-147">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="1250f-148">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="1250f-148">Handle errors</span></span>

<span data-ttu-id="1250f-149">Pro toodisplay datového proudu data v grafu hello jeho **typ** v hello **informace o zařízení** metadata musí odpovídat datovému typu hello hello telemetrie hodnot.</span><span class="sxs-lookup"><span data-stu-id="1250f-149">For a data stream toodisplay on hello chart, its **Type** in hello **Device-Info** metadata must match hello data type of hello telemetry values.</span></span> <span data-ttu-id="1250f-150">Například pokud hello metadata Určuje, že hello **typ** vlhkosti dat je **int** a **dvojité** se nachází v datový proud telemetrie hello pak nemá telemetrie vlhkosti hello nejsou zobrazeny v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="1250f-150">For example, if hello metadata specifies that hello **Type** of humidity data is **int** and a **double** is found in hello telemetry stream then hello humidity telemetry does not display on hello chart.</span></span> <span data-ttu-id="1250f-151">Ale hello **vlhkosti** hodnoty jsou stále uloženy a k dispozici pro zpracování všech back-end.</span><span class="sxs-lookup"><span data-stu-id="1250f-151">However, hello **Humidity** values are still stored and made available for any back-end processing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1250f-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1250f-152">Next steps</span></span>

<span data-ttu-id="1250f-153">Teď, když už víte, jak toouse dynamické telemetrických dat, další informace o tom, jak hello předkonfigurovaných řešení použít informace o zařízení: [informace metadat zařízení v hello vzdálené monitorování předkonfigurované řešení] [ lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="1250f-153">Now that you've seen how toouse dynamic telemetry, you can learn more about how hello preconfigured solutions use device information: [Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
