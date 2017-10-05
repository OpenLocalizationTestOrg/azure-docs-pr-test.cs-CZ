---
title: "Přizpůsobení předkonfigurovaných řešení | Microsoft Docs"
description: "Poskytuje pokyny o tom, jak přizpůsobit Azure IoT Suite předkonfigurované řešení."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: bdf4cd89d5ad0392337dfe761108608d506adf18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="customize-a-preconfigured-solution"></a><span data-ttu-id="b58f9-103">Přizpůsobení předkonfigurovaného řešení</span><span class="sxs-lookup"><span data-stu-id="b58f9-103">Customize a preconfigured solution</span></span>

<span data-ttu-id="b58f9-104">Předkonfigurovaná řešení Azure IoT Suite součástí Ukázka služby v rámci sady společně k poskytování-komplexní řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-104">The preconfigured solutions provided with the Azure IoT Suite demonstrate the services within the suite working together to deliver an end-to-end solution.</span></span> <span data-ttu-id="b58f9-105">Z tohoto bodu počáteční jsou různých místech, ve kterých můžete rozšířit a přizpůsobit řešení pro konkrétní scénáře.</span><span class="sxs-lookup"><span data-stu-id="b58f9-105">From this starting point, there are various places in which you can extend and customize the solution for specific scenarios.</span></span> <span data-ttu-id="b58f9-106">Následující části popisují tyto společné body přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-106">The following sections describe these common customization points.</span></span>

## <a name="find-the-source-code"></a><span data-ttu-id="b58f9-107">Vyhledání zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="b58f9-107">Find the source code</span></span>

<span data-ttu-id="b58f9-108">Zdrojový kód pro předkonfigurované řešení je k dispozici na Githubu v následující úložiště:</span><span class="sxs-lookup"><span data-stu-id="b58f9-108">The source code for the preconfigured solutions is available on GitHub in the following repositories:</span></span>

* <span data-ttu-id="b58f9-109">Vzdálené monitorování: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span><span class="sxs-lookup"><span data-stu-id="b58f9-109">Remote Monitoring: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span></span>
* <span data-ttu-id="b58f9-110">Prediktivní údržby: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span><span class="sxs-lookup"><span data-stu-id="b58f9-110">Predictive Maintenance: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span></span>
* <span data-ttu-id="b58f9-111">Připojené factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span><span class="sxs-lookup"><span data-stu-id="b58f9-111">Connected factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span></span>

<span data-ttu-id="b58f9-112">Zdrojový kód pro předkonfigurované řešení je určen k předvedení vzory a postupy, které slouží k implementaci začátku do konce funkcí řešení IoT pomocí sady Azure IoT Suite.</span><span class="sxs-lookup"><span data-stu-id="b58f9-112">The source code for the preconfigured solutions is provided to demonstrate the patterns and practices used to implement the end-to-end functionality of an IoT solution using Azure IoT Suite.</span></span> <span data-ttu-id="b58f9-113">Můžete najít další informace o tom, jak vytvořit a nasadit řešení v úložišť GitHub.</span><span class="sxs-lookup"><span data-stu-id="b58f9-113">You can find more information about how to build and deploy the solutions in the GitHub repositories.</span></span>

## <a name="change-the-preconfigured-rules"></a><span data-ttu-id="b58f9-114">Změnit předkonfigurovaných pravidel</span><span class="sxs-lookup"><span data-stu-id="b58f9-114">Change the preconfigured rules</span></span>

<span data-ttu-id="b58f9-115">Řešení vzdáleného monitorování obsahuje tři [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) úlohy pro zpracování informací o zařízení, telemetrie a logiku pravidla v řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-115">The remote monitoring solution includes three [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) jobs to handle device information, telemetry, and rules logic in the solution.</span></span>

<span data-ttu-id="b58f9-116">Jsou tři stream analytics úloh a jejich syntaxi jsou popsané v podrobně [vzdálené monitorování návod pro předkonfigurované řešení](iot-suite-remote-monitoring-sample-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="b58f9-116">The three stream analytics jobs and their syntax are described in depth in the [Remote monitoring preconfigured solution walkthrough](iot-suite-remote-monitoring-sample-walkthrough.md).</span></span> 

<span data-ttu-id="b58f9-117">Můžete upravit tyto úlohy přímo ke změně logiku, nebo přidejte logiku, které jsou specifické pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="b58f9-117">You can edit these jobs directly to alter the logic, or add logic specific to your scenario.</span></span> <span data-ttu-id="b58f9-118">Úlohy služby Stream Analytics můžete najít následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b58f9-118">You can find the Stream Analytics jobs as follows:</span></span>

1. <span data-ttu-id="b58f9-119">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b58f9-119">Go to [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b58f9-120">Přejděte do skupiny prostředků se stejným názvem jako řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="b58f9-120">Navigate to the resource group with the same name as your IoT solution.</span></span> 
3. <span data-ttu-id="b58f9-121">Vyberte úlohu Azure Stream Analytics, kterou chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="b58f9-121">Select the Azure Stream Analytics job you'd like to modify.</span></span> 
4. <span data-ttu-id="b58f9-122">Zastavit úlohu tak, že vyberete **Zastavit** v sadu příkazů.</span><span class="sxs-lookup"><span data-stu-id="b58f9-122">Stop the job by selecting **Stop** in the set of commands.</span></span> 
5. <span data-ttu-id="b58f9-123">Upravte vstupy, dotazů a výstupy.</span><span class="sxs-lookup"><span data-stu-id="b58f9-123">Edit the inputs, query, and outputs.</span></span>
   
    <span data-ttu-id="b58f9-124">Jednoduchých úprav je změna dotazu pro **pravidla** úlohu, která má použít **"<"** místo **">"**.</span><span class="sxs-lookup"><span data-stu-id="b58f9-124">A simple modification is to change the query for the **Rules** job to use a **"<"** instead of a **">"**.</span></span> <span data-ttu-id="b58f9-125">Na portálu řešení stále zobrazuje **">"** při upravit pravidlo, ale Všimněte si, jak se chování obráceně z důvodu změny v základní úlohy.</span><span class="sxs-lookup"><span data-stu-id="b58f9-125">The solution portal still shows **">"** when you edit a rule, but notice how the behavior is flipped due to the change in the underlying job.</span></span>
6. <span data-ttu-id="b58f9-126">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="b58f9-126">Start the job</span></span>

> [!NOTE]
> <span data-ttu-id="b58f9-127">Řídicím panelu vzdáleného monitorování závisí na konkrétní data, takže změna úloh může způsobit selhání řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="b58f9-127">The remote monitoring dashboard depends on specific data, so altering the jobs can cause the dashboard to fail.</span></span>

## <a name="add-your-own-rules"></a><span data-ttu-id="b58f9-128">Přidání vlastních pravidel</span><span class="sxs-lookup"><span data-stu-id="b58f9-128">Add your own rules</span></span>

<span data-ttu-id="b58f9-129">Kromě změna předkonfigurované úlohy Azure Stream Analytics, můžete na portálu Azure můžete přidat nové úlohy nebo přidat nové dotazy k stávající úlohy.</span><span class="sxs-lookup"><span data-stu-id="b58f9-129">In addition to changing the preconfigured Azure Stream Analytics jobs, you can use the Azure portal to add new jobs or add new queries to existing jobs.</span></span>

## <a name="customize-devices"></a><span data-ttu-id="b58f9-130">Vlastní nastavení zařízení</span><span class="sxs-lookup"><span data-stu-id="b58f9-130">Customize devices</span></span>

<span data-ttu-id="b58f9-131">Mezi nejběžnější rozšíření aktivity ve spolupráci s zařízení, které jsou specifické pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="b58f9-131">One of the most common extension activities is working with devices specific to your scenario.</span></span> <span data-ttu-id="b58f9-132">Existuje několik metod pro práci s zařízení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-132">There are several methods for working with devices.</span></span> <span data-ttu-id="b58f9-133">Tyto metody zahrnují změny simulované zařízení tak, aby odpovídaly vašemu scénáři, nebo pomocí [sady SDK zařízení IoT] [ IoT Device SDK] připojit fyzického zařízení k řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-133">These methods include altering a simulated device to match your scenario, or using the [IoT Device SDK][IoT Device SDK] to connect your physical device to the solution.</span></span>

<span data-ttu-id="b58f9-134">Podrobný návod k přidávání zařízení, najdete v článku [Iot Suite připojení zařízení](iot-suite-connecting-devices.md) článku a [vzdálené monitorování C SDK – ukázka](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span><span class="sxs-lookup"><span data-stu-id="b58f9-134">For a step-by-step guide to adding devices, see the [Iot Suite Connecting Devices](iot-suite-connecting-devices.md) article and the [remote monitoring C SDK Sample](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span></span> <span data-ttu-id="b58f9-135">Tato ukázka je navržen pro práci s předkonfigurovaného řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="b58f9-135">This sample is designed to work with the remote monitoring preconfigured solution.</span></span>

### <a name="create-your-own-simulated-device"></a><span data-ttu-id="b58f9-136">Vytvoření simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="b58f9-136">Create your own simulated device</span></span>

<span data-ttu-id="b58f9-137">Součástí [pro vzdálené monitorování řešení zdrojového kódu](https://github.com/Azure/azure-iot-remote-monitoring), je simulátoru .NET.</span><span class="sxs-lookup"><span data-stu-id="b58f9-137">Included in the [remote monitoring solution source code](https://github.com/Azure/azure-iot-remote-monitoring), is a .NET simulator.</span></span> <span data-ttu-id="b58f9-138">Tato simulátoru je zřízená jako součást řešení a můžete ho odeslat rozdílná metadata, telemetrických dat, a reagovat na jiné příkazy a metody alter.</span><span class="sxs-lookup"><span data-stu-id="b58f9-138">This simulator is the one provisioned as part of the solution and you can alter it to send different metadata, telemetry, and respond to different commands and methods.</span></span>

<span data-ttu-id="b58f9-139">Simulátoru předem nakonfigurované v předkonfigurovaného řešení vzdáleného monitorování simuluje chladič zařízení, který vysílá telemetrická data teploty a vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="b58f9-139">The preconfigured simulator in the remote monitoring preconfigured solution simulates a cooler device that emits temperature and humidity telemetry.</span></span> <span data-ttu-id="b58f9-140">Můžete upravit v simulátoru [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projektu, když jste forked úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="b58f9-140">You can modify the simulator in the [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) project when you've forked the GitHub repository.</span></span>

### <a name="available-locations-for-simulated-devices"></a><span data-ttu-id="b58f9-141">K dispozici umístění pro simulované zařízení</span><span class="sxs-lookup"><span data-stu-id="b58f9-141">Available locations for simulated devices</span></span>

<span data-ttu-id="b58f9-142">Sada výchozí umístění je v Praze/Redmond, Washington, USA.</span><span class="sxs-lookup"><span data-stu-id="b58f9-142">The default set of locations is in Seattle/Redmond, Washington, United States of America.</span></span> <span data-ttu-id="b58f9-143">Můžete změnit tato místa v [SampleDeviceFactory.cs][lnk-sample-device-factory].</span><span class="sxs-lookup"><span data-stu-id="b58f9-143">You can change these locations in [SampleDeviceFactory.cs][lnk-sample-device-factory].</span></span>

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a><span data-ttu-id="b58f9-144">Přidání obslužné rutiny aktualizace požadované vlastnosti simulátoru</span><span class="sxs-lookup"><span data-stu-id="b58f9-144">Add a desired property update handler to the simulator</span></span>

<span data-ttu-id="b58f9-145">Na portálu řešení můžete nastavit hodnotu pro požadovanou vlastnost pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-145">You can set a value for a desired property for a device in the solution portal.</span></span> <span data-ttu-id="b58f9-146">Je zodpovědností zařízení ke zpracování vlastnosti žádost o změnu, když zařízení načte hodnoty požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b58f9-146">It is the responsibility of the device to handle the property change request when the device retrieves the desired property value.</span></span> <span data-ttu-id="b58f9-147">Chcete-li přidat podporu pro změnu hodnoty vlastnosti prostřednictvím požadované vlastnosti, přidání obslužné rutiny simulátoru.</span><span class="sxs-lookup"><span data-stu-id="b58f9-147">To add support for a property value change through a desired property, you need to add a handler to the simulator.</span></span>

<span data-ttu-id="b58f9-148">Simulátor obsahuje obslužné rutiny pro **SetPointTemp** a **TelemetryInterval** vlastnosti, které lze aktualizovat nastavením požadované hodnoty na portálu řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-148">The simulator contains handlers for the **SetPointTemp** and **TelemetryInterval** properties that you can update by setting desired values in the solution portal.</span></span>

<span data-ttu-id="b58f9-149">Následující příklad ukazuje rutinu **SetPointTemp** potřeby vlastnost **CoolerDevice** – třída:</span><span class="sxs-lookup"><span data-stu-id="b58f9-149">The following example shows the handler for the **SetPointTemp** desired property in the **CoolerDevice** class:</span></span>

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

<span data-ttu-id="b58f9-150">Tato metoda aktualizace teploty bodu telemetrie a hlásí změnu zpět do služby IoT Hub nastavením hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b58f9-150">This method updates the telemetry point temperature and then reports the change back to IoT Hub by setting a reported property.</span></span>

<span data-ttu-id="b58f9-151">Podle vzoru v předchozím příkladu můžete přidat svoje vlastní obslužné rutiny pro vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b58f9-151">You can add your own handlers for your own properties by following the pattern in the preceding example.</span></span>

<span data-ttu-id="b58f9-152">Je třeba také svázat požadovanou vlastnost obslužná rutina jak je znázorněno v následujícím příkladu z **CoolerDevice** konstruktor:</span><span class="sxs-lookup"><span data-stu-id="b58f9-152">You must also bind the desired property to the handler as shown in the following example from the **CoolerDevice** constructor:</span></span>

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

<span data-ttu-id="b58f9-153">Všimněte si, že **SetPointTempPropertyName** konstanta definovaný jako "Config.SetPointTemp".</span><span class="sxs-lookup"><span data-stu-id="b58f9-153">Note that **SetPointTempPropertyName** is a constant defined as "Config.SetPointTemp".</span></span>

### <a name="add-support-for-a-new-method-to-the-simulator"></a><span data-ttu-id="b58f9-154">Přidat podporu pro nové metody pro simulátoru</span><span class="sxs-lookup"><span data-stu-id="b58f9-154">Add support for a new method to the simulator</span></span>

<span data-ttu-id="b58f9-155">Můžete přizpůsobit simulátoru přidat podporu pro nové [– metoda (přímá metoda)][lnk-direct-methods].</span><span class="sxs-lookup"><span data-stu-id="b58f9-155">You can customize the simulator to add support for a new [method (direct method)][lnk-direct-methods].</span></span> <span data-ttu-id="b58f9-156">Existují dva klíče kroky:</span><span class="sxs-lookup"><span data-stu-id="b58f9-156">There are two key steps required:</span></span>

- <span data-ttu-id="b58f9-157">Simulátor musíte upozornit službu IoT hub v předkonfigurovaném řešení s podrobnostmi metody.</span><span class="sxs-lookup"><span data-stu-id="b58f9-157">The simulator must notify the IoT hub in the preconfigured solution with details of the method.</span></span>
- <span data-ttu-id="b58f9-158">Simulátor musí obsahovat kód pro zpracování volání metody, které při vyvolání z **podrobnosti o zařízení** panely v Průzkumníku řešení nebo prostřednictvím úlohu.</span><span class="sxs-lookup"><span data-stu-id="b58f9-158">The simulator must include code to handle the method call when you invoke it from the **Device details** panel in the solution explorer or through a job.</span></span>

<span data-ttu-id="b58f9-159">Vzdálené monitorování předkonfigurované řešení používá *hlášené vlastnosti* Odeslat podrobnosti o podporovaných metod do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="b58f9-159">The remote monitoring preconfigured solution uses *reported properties* to send details of supported methods to IoT hub.</span></span> <span data-ttu-id="b58f9-160">Back-end řešení udržuje seznam všech metod nepodporuje každé zařízení společně s historie volání metod.</span><span class="sxs-lookup"><span data-stu-id="b58f9-160">The solution back end maintains a list of all the methods supported by each device along with a history of method invocations.</span></span> <span data-ttu-id="b58f9-161">Můžete zobrazit tyto informace o zařízeních a volat metody na portálu řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-161">You can view this information about devices and invoke methods in the solution portal.</span></span>

<span data-ttu-id="b58f9-162">Oznámení služby IoT hub, zařízení podporuje metodu, zařízení musí přidat podrobnosti metody, která **SupportedMethods** uzlu ve vlastnostech hlášené:</span><span class="sxs-lookup"><span data-stu-id="b58f9-162">To notify the IoT hub that a device supports a method, the device must add details of the method to the **SupportedMethods** node in the reported properties:</span></span>

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

<span data-ttu-id="b58f9-163">Podpis metody má následující formát: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span><span class="sxs-lookup"><span data-stu-id="b58f9-163">The method signature has the following format: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span></span> <span data-ttu-id="b58f9-164">Například zadejte **InitiateFirmwareUpdate** metoda očekává parametr řetězec s názvem **FwPackageURI**, použijte následující podpis metody:</span><span class="sxs-lookup"><span data-stu-id="b58f9-164">For example, to specify the **InitiateFirmwareUpdate** method expects a string parameter named **FwPackageURI**, use the following method signature:</span></span>

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

<span data-ttu-id="b58f9-165">Seznam typů podporovaných parametrů najdete v tématu **CommandTypes** – třída v projektu infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="b58f9-165">For a list of supported parameter types, see the **CommandTypes** class in the Infrastructure project.</span></span>

<span data-ttu-id="b58f9-166">Chcete-li odstranit metodu, nastavte podpis metody `null` ve vlastnostech hlášené.</span><span class="sxs-lookup"><span data-stu-id="b58f9-166">To delete a method, set the method signature to `null` in the reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="b58f9-167">Back-end řešení aktualizuje informace o podporovaných metod pouze, když obdrží *informace o zařízení* zprávy ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-167">The solution back end only updates information about supported methods when it receives a *device information* message from the device.</span></span>

<span data-ttu-id="b58f9-168">Následující příklad kódu z **SampleDeviceFactory** – třída v běžné projektu ukazuje, jak přidat metodu do seznamu **SupportedMethods** ve vlastnostech hlášené poslal zařízení:</span><span class="sxs-lookup"><span data-stu-id="b58f9-168">The following code sample from the **SampleDeviceFactory** class in the Common project shows how to add a method to the list of **SupportedMethods** in the reported properties sent by the device:</span></span>

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

<span data-ttu-id="b58f9-169">Tento fragment kódu přidá podrobnosti o **InitiateFirmwareUpdate** metoda včetně text, který se zobrazí na portálu řešení a podrobnosti o parametrech požadovaná metoda.</span><span class="sxs-lookup"><span data-stu-id="b58f9-169">This code snippet adds details of the **InitiateFirmwareUpdate** method including text to display in the solution portal and details of the required method parameters.</span></span>

<span data-ttu-id="b58f9-170">Simulátor odešle hlášené vlastnosti, včetně seznamu podporovaných metod do služby IoT Hub při spuštění simulátoru.</span><span class="sxs-lookup"><span data-stu-id="b58f9-170">The simulator sends reported properties, including the list of supported methods, to IoT Hub when the simulator starts.</span></span>

<span data-ttu-id="b58f9-171">Přidání obslužné rutiny kód simulátor pro každou metodu, kterou podporuje.</span><span class="sxs-lookup"><span data-stu-id="b58f9-171">Add a handler to the simulator code for each method it supports.</span></span> <span data-ttu-id="b58f9-172">Zobrazí se stávající obslužné rutiny v **CoolerDevice** – třída v projektu Simulator.WebJob.</span><span class="sxs-lookup"><span data-stu-id="b58f9-172">You can see the existing handlers in the **CoolerDevice** class in the Simulator.WebJob project.</span></span> <span data-ttu-id="b58f9-173">Následující příklad ukazuje rutinu **InitiateFirmwareUpdate** metoda:</span><span class="sxs-lookup"><span data-stu-id="b58f9-173">The following example shows the handler for **InitiateFirmwareUpdate** method:</span></span>

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

<span data-ttu-id="b58f9-174">Metoda obslužná rutina názvy musí začínat `On` následuje název metody.</span><span class="sxs-lookup"><span data-stu-id="b58f9-174">Method handler names must start with `On` followed by the name of the method.</span></span> <span data-ttu-id="b58f9-175">**MethodRequest** parametr obsahuje žádné parametry předané pomocí volání metody z back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-175">The **methodRequest** parameter contains any parameters passed with the method invocation from the solution back end.</span></span> <span data-ttu-id="b58f9-176">Návratová hodnota musí být typu **úloh&lt;MethodResponse&gt;**.</span><span class="sxs-lookup"><span data-stu-id="b58f9-176">The return value must be of type **Task&lt;MethodResponse&gt;**.</span></span> <span data-ttu-id="b58f9-177">**BuildMethodResponse** nástroj metoda vám pomůže vytvořit návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b58f9-177">The **BuildMethodResponse** utility method helps you create the return value.</span></span>

<span data-ttu-id="b58f9-178">Uvnitř obslužné rutiny metoda může takto:</span><span class="sxs-lookup"><span data-stu-id="b58f9-178">Inside the method handler, you could:</span></span>

- <span data-ttu-id="b58f9-179">Spustí asynchronní úlohu.</span><span class="sxs-lookup"><span data-stu-id="b58f9-179">Start an asynchronous task.</span></span>
- <span data-ttu-id="b58f9-180">Načíst požadované vlastnosti z *dvojče zařízení* IoT hub.</span><span class="sxs-lookup"><span data-stu-id="b58f9-180">Retrieve desired properties from the *device twin* in IoT Hub.</span></span>
- <span data-ttu-id="b58f9-181">Aktualizovat vlastnosti jediné hlášené pomocí **SetReportedPropertyAsync** metoda v **CoolerDevice** třídy.</span><span class="sxs-lookup"><span data-stu-id="b58f9-181">Update a single reported property using the **SetReportedPropertyAsync** method in the **CoolerDevice** class.</span></span>
- <span data-ttu-id="b58f9-182">Aktualizovat více hlášené vlastnosti tak, že vytvoříte **TwinCollection** instance a volání **Transport.UpdateReportedPropertiesAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="b58f9-182">Update multiple reported properties by creating a **TwinCollection** instance and calling the **Transport.UpdateReportedPropertiesAsync** method.</span></span>

<span data-ttu-id="b58f9-183">V předchozím příkladu aktualizace firmwaru provede následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b58f9-183">The preceding firmware update example performs the following steps:</span></span>

- <span data-ttu-id="b58f9-184">Kontroluje, zda je zařízení nemůže přijmout žádost o aktualizaci firmwaru.</span><span class="sxs-lookup"><span data-stu-id="b58f9-184">Checks the device is able to accept the firmware update request.</span></span>
- <span data-ttu-id="b58f9-185">Asynchronně spustí operace aktualizace firmwaru a obnoví telemetrii po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="b58f9-185">Asynchronously initiates the firmware update operation and resets the telemetry when the operation is complete.</span></span>
- <span data-ttu-id="b58f9-186">Okamžitě vrátí zprávu "FirmwareUpdate přijata" se indikovat, že žádost byla přijata zařízení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-186">Immediately returns the "FirmwareUpdate accepted" message to indicate the request was accepted by the device.</span></span>

### <a name="build-and-use-your-own-physical-device"></a><span data-ttu-id="b58f9-187">Vytvoření a použití si vlastní zařízení (fyzické)</span><span class="sxs-lookup"><span data-stu-id="b58f9-187">Build and use your own (physical) device</span></span>

<span data-ttu-id="b58f9-188">[SDK služby Azure IoT](https://github.com/Azure/azure-iot-sdks) poskytují knihovny pro připojení (jazyky a operační systémy) celou řadu typů zařízení do řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="b58f9-188">The [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) provide libraries for connecting numerous device types (languages and operating systems) into IoT solutions.</span></span>

## <a name="modify-dashboard-limits"></a><span data-ttu-id="b58f9-189">Upravit omezení řídicí panel</span><span class="sxs-lookup"><span data-stu-id="b58f9-189">Modify dashboard limits</span></span>

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a><span data-ttu-id="b58f9-190">Počet zařízení, které jsou zobrazeny v rozevírací nabídce řídicí panel</span><span class="sxs-lookup"><span data-stu-id="b58f9-190">Number of devices displayed in dashboard dropdown</span></span>

<span data-ttu-id="b58f9-191">Výchozí hodnota je 200.</span><span class="sxs-lookup"><span data-stu-id="b58f9-191">The default is 200.</span></span> <span data-ttu-id="b58f9-192">Můžete změnit toto číslo v [DashboardController.cs][lnk-dashboard-controller].</span><span class="sxs-lookup"><span data-stu-id="b58f9-192">You can change this number in [DashboardController.cs][lnk-dashboard-controller].</span></span>

### <a name="number-of-pins-to-display-in-bing-map-control"></a><span data-ttu-id="b58f9-193">Počet kódů PIN pro zobrazení v ovládacím prvku mapy Bing</span><span class="sxs-lookup"><span data-stu-id="b58f9-193">Number of pins to display in Bing Map control</span></span>

<span data-ttu-id="b58f9-194">Výchozí hodnota je 200.</span><span class="sxs-lookup"><span data-stu-id="b58f9-194">The default is 200.</span></span> <span data-ttu-id="b58f9-195">Můžete změnit toto číslo v [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span><span class="sxs-lookup"><span data-stu-id="b58f9-195">You can change this number in [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span></span>

### <a name="time-period-of-telemetry-graph"></a><span data-ttu-id="b58f9-196">Časové období telemetrických dat grafu</span><span class="sxs-lookup"><span data-stu-id="b58f9-196">Time period of telemetry graph</span></span>

<span data-ttu-id="b58f9-197">Výchozí hodnota je 10 minut.</span><span class="sxs-lookup"><span data-stu-id="b58f9-197">The default is 10 minutes.</span></span> <span data-ttu-id="b58f9-198">Můžete změnit tuto hodnotu v [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span><span class="sxs-lookup"><span data-stu-id="b58f9-198">You can change this value in [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span></span>

## <a name="manually-set-up-application-roles"></a><span data-ttu-id="b58f9-199">Ručně nastavit aplikační role</span><span class="sxs-lookup"><span data-stu-id="b58f9-199">Manually set up application roles</span></span>

<span data-ttu-id="b58f9-200">Následující postup popisuje, jak přidat **správce** a **jen pro čtení** aplikační role pro předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-200">The following procedure describes how to add **Admin** and **ReadOnly** application roles to a preconfigured solution.</span></span> <span data-ttu-id="b58f9-201">Všimněte si, že zahrnují předkonfigurovaných řešení, které jsou už zřízené z webu azureiotsuite.com **správce** a **jen pro čtení** role.</span><span class="sxs-lookup"><span data-stu-id="b58f9-201">Note that preconfigured solutions provisioned from the azureiotsuite.com site already include the **Admin** and **ReadOnly** roles.</span></span>

<span data-ttu-id="b58f9-202">Členové **jen pro čtení** role můžete zobrazit řídicí panel a v seznamu zařízení, ale nejsou povoleny zařízení přidat, změnit atributy zařízení nebo odesílat příkazy.</span><span class="sxs-lookup"><span data-stu-id="b58f9-202">Members of the **ReadOnly** role can see the dashboard and the device list, but are not allowed to add devices, change device attributes, or send commands.</span></span>  <span data-ttu-id="b58f9-203">Členové **správce** role mají plný přístup ke všem funkcím v řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-203">Members of the **Admin** role have full access to all the functionality in the solution.</span></span>

1. <span data-ttu-id="b58f9-204">Přejděte na [portál Azure classic][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="b58f9-204">Go to the [Azure classic portal][lnk-classic-portal].</span></span>
2. <span data-ttu-id="b58f9-205">Vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b58f9-205">Select **Active Directory**.</span></span>
3. <span data-ttu-id="b58f9-206">Klikněte na název klienta AAD, které jste použili při zřizování řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-206">Click the name of the AAD tenant you used when you provisioned your solution.</span></span>
4. <span data-ttu-id="b58f9-207">Klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b58f9-207">Click **Applications**.</span></span>
5. <span data-ttu-id="b58f9-208">Klikněte na název aplikace, která odpovídá názvu vašeho předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="b58f9-208">Click the name of the application that matches your preconfigured solution name.</span></span> <span data-ttu-id="b58f9-209">Pokud nevidíte aplikaci v seznamu, vyberte **aplikace Moje společnost vlastní** v **zobrazit** rozevíracího seznamu a klikněte na označit kontroly.</span><span class="sxs-lookup"><span data-stu-id="b58f9-209">If you don't see your application in the list, select **Applications my company owns** in the **Show** dropdown and click the check mark.</span></span>
6. <span data-ttu-id="b58f9-210">V dolní části stránky klikněte na tlačítko **spravovat Manifest** a potom **stáhnout Manifest**.</span><span class="sxs-lookup"><span data-stu-id="b58f9-210">At the bottom of the page, click **Manage Manifest** and then **Download Manifest**.</span></span>
7. <span data-ttu-id="b58f9-211">Tento postup stáhne soubor .json do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="b58f9-211">This procedure downloads a .json file to your local machine.</span></span> <span data-ttu-id="b58f9-212">Umožňuje otevřete tento soubor pro úpravy v textovém editoru podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="b58f9-212">Open this file for editing in a text editor of your choice.</span></span>
8. <span data-ttu-id="b58f9-213">Na třetí řádek soubor .json se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="b58f9-213">On the third line of the .json file, you can see:</span></span>

   ```json
   "appRoles" : [],
   ```
   <span data-ttu-id="b58f9-214">Nahraďte tento řádek následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="b58f9-214">Replace this line with the following code:</span></span>

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access to the application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access to device information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. <span data-ttu-id="b58f9-215">Uložte soubor aktualizované .json (můžete přepsat existující soubor).</span><span class="sxs-lookup"><span data-stu-id="b58f9-215">Save the updated .json file (you can overwrite the existing file).</span></span>
10. <span data-ttu-id="b58f9-216">V portálu Azure classic, v dolní části stránky, vyberte **spravovat Manifest** pak **nahrát Manifest** nahrát soubor .json, který jste uložili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="b58f9-216">In the Azure classic portal, at the bottom of the page, select **Manage Manifest** then **Upload Manifest** to upload the .json file you saved in the previous step.</span></span>
11. <span data-ttu-id="b58f9-217">Nyní jste přidali **správce** a **jen pro čtení** rolí do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b58f9-217">You have now added the **Admin** and **ReadOnly** roles to your application.</span></span>
12. <span data-ttu-id="b58f9-218">Přiřadit jedna z těchto rolí uživatele ve vašem adresáři, najdete v části [oprávnění na webu azureiotsuite.com][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="b58f9-218">To assign one of these roles to a user in your directory, see [Permissions on the azureiotsuite.com site][lnk-permissions].</span></span>

## <a name="feedback"></a><span data-ttu-id="b58f9-219">Váš názor</span><span class="sxs-lookup"><span data-stu-id="b58f9-219">Feedback</span></span>

<span data-ttu-id="b58f9-220">Máte přizpůsobení chcete najdete v části zahrnuté v tomto dokumentu?</span><span class="sxs-lookup"><span data-stu-id="b58f9-220">Do you have a customization you'd like to see covered in this document?</span></span> <span data-ttu-id="b58f9-221">Přidání funkce návrhy na [User Voice](https://feedback.azure.com/forums/321918-azure-iot), nebo komentář k tomuto článku.</span><span class="sxs-lookup"><span data-stu-id="b58f9-221">Add feature suggestions to [User Voice](https://feedback.azure.com/forums/321918-azure-iot), or comment on this article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b58f9-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b58f9-222">Next steps</span></span>

<span data-ttu-id="b58f9-223">Další informace o možnostech přizpůsobení předkonfigurovaných řešení najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="b58f9-223">To learn more about the options for customizing the preconfigured solutions, see:</span></span>

* <span data-ttu-id="b58f9-224">[Připojení aplikace logiky k řešení Azure IoT Suite vzdálené monitorování předkonfigurované][lnk-logicapp]</span><span class="sxs-lookup"><span data-stu-id="b58f9-224">[Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapp]</span></span>
* <span data-ttu-id="b58f9-225">[Použití dynamické telemetrie s předkonfigurovaného řešení vzdáleného monitorování][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="b58f9-225">[Use dynamic telemetry with the remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="b58f9-226">[Informace metadat zařízení v předkonfigurovaného řešení vzdáleného monitorování][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="b58f9-226">[Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo]</span></span>
* <span data-ttu-id="b58f9-227">[Přizpůsobit způsob, jakým připojené vytváření řešení zobrazí data z vašich serverů OPC UA][lnk-cf-customize]</span><span class="sxs-lookup"><span data-stu-id="b58f9-227">[Customize how the connected factory solution displays data from your OPC UA servers][lnk-cf-customize]</span></span>

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]: iot-suite-connected-factory-customize.md