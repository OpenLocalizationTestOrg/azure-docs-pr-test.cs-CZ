---
title: "aaaCustomizing předkonfigurovanými řešeními | Microsoft Docs"
description: "Obsahuje pokyny k jak toocustomize hello Azure IoT Suite předkonfigurovaných řešení."
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
ms.openlocfilehash: 1a8573f5ac6ed944c44459df495446f15174d513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-a-preconfigured-solution"></a><span data-ttu-id="60970-103">Přizpůsobení předkonfigurovaného řešení</span><span class="sxs-lookup"><span data-stu-id="60970-103">Customize a preconfigured solution</span></span>

<span data-ttu-id="60970-104">Hello předkonfigurované řešení součástí hello Azure IoT Suite ukázka hello služby v rámci hello suite pracovní společně toodeliver-komplexní řešení.</span><span class="sxs-lookup"><span data-stu-id="60970-104">hello preconfigured solutions provided with hello Azure IoT Suite demonstrate hello services within hello suite working together toodeliver an end-to-end solution.</span></span> <span data-ttu-id="60970-105">V tento počáteční bod existují různé míst, ve kterých můžete rozšířit a přizpůsobit hello řešení pro konkrétní scénáře.</span><span class="sxs-lookup"><span data-stu-id="60970-105">From this starting point, there are various places in which you can extend and customize hello solution for specific scenarios.</span></span> <span data-ttu-id="60970-106">Hello následující části popisují tyto společné body přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="60970-106">hello following sections describe these common customization points.</span></span>

## <a name="find-hello-source-code"></a><span data-ttu-id="60970-107">Najít hello zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="60970-107">Find hello source code</span></span>

<span data-ttu-id="60970-108">zdrojový kód Hello hello předkonfigurované řešení je k dispozici na Githubu v hello následující úložiště:</span><span class="sxs-lookup"><span data-stu-id="60970-108">hello source code for hello preconfigured solutions is available on GitHub in hello following repositories:</span></span>

* <span data-ttu-id="60970-109">Vzdálené monitorování: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span><span class="sxs-lookup"><span data-stu-id="60970-109">Remote Monitoring: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span></span>
* <span data-ttu-id="60970-110">Prediktivní údržby: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span><span class="sxs-lookup"><span data-stu-id="60970-110">Predictive Maintenance: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span></span>
* <span data-ttu-id="60970-111">Připojené factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span><span class="sxs-lookup"><span data-stu-id="60970-111">Connected factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span></span>

<span data-ttu-id="60970-112">zdrojový kód Hello hello předkonfigurované řešení je zadaný toodemonstrate hello vzory a postupy používané funkce začátku do konce hello tooimplement řešení IoT pomocí sady Azure IoT Suite.</span><span class="sxs-lookup"><span data-stu-id="60970-112">hello source code for hello preconfigured solutions is provided toodemonstrate hello patterns and practices used tooimplement hello end-to-end functionality of an IoT solution using Azure IoT Suite.</span></span> <span data-ttu-id="60970-113">Můžete najít další informace o toobuild a nasadit řešení hello v úložišť GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="60970-113">You can find more information about how toobuild and deploy hello solutions in hello GitHub repositories.</span></span>

## <a name="change-hello-preconfigured-rules"></a><span data-ttu-id="60970-114">Změna pravidel hello předkonfigurované</span><span class="sxs-lookup"><span data-stu-id="60970-114">Change hello preconfigured rules</span></span>

<span data-ttu-id="60970-115">Hello řešení vzdáleného monitorování obsahuje tři [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) úlohy toohandle informace o zařízení, telemetrie a logiku pravidla v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="60970-115">hello remote monitoring solution includes three [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) jobs toohandle device information, telemetry, and rules logic in hello solution.</span></span>

<span data-ttu-id="60970-116">Hello tři stream analytics úloh a jejich syntaxe jsou popsány podrobněji v hello [vzdálené monitorování návod pro předkonfigurované řešení](iot-suite-remote-monitoring-sample-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="60970-116">hello three stream analytics jobs and their syntax are described in depth in hello [Remote monitoring preconfigured solution walkthrough](iot-suite-remote-monitoring-sample-walkthrough.md).</span></span> 

<span data-ttu-id="60970-117">Tyto úlohy můžete upravit přímo tooalter hello logiku, nebo přidejte logiku tooyour konkrétní scénáře.</span><span class="sxs-lookup"><span data-stu-id="60970-117">You can edit these jobs directly tooalter hello logic, or add logic specific tooyour scenario.</span></span> <span data-ttu-id="60970-118">Můžete najít hello úlohy Stream Analytics následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="60970-118">You can find hello Stream Analytics jobs as follows:</span></span>

1. <span data-ttu-id="60970-119">Přejděte příliš[portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="60970-119">Go too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="60970-120">Přejděte toohello skupinu prostředků s hello stejný název jako řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="60970-120">Navigate toohello resource group with hello same name as your IoT solution.</span></span> 
3. <span data-ttu-id="60970-121">Vyberte úlohy Azure Stream Analytics hello chcete toomodify.</span><span class="sxs-lookup"><span data-stu-id="60970-121">Select hello Azure Stream Analytics job you'd like toomodify.</span></span> 
4. <span data-ttu-id="60970-122">Úlohu zastavení hello výběrem **Zastavit** v hello sadu příkazů.</span><span class="sxs-lookup"><span data-stu-id="60970-122">Stop hello job by selecting **Stop** in hello set of commands.</span></span> 
5. <span data-ttu-id="60970-123">Upravte hello vstupy, dotazů a výstupy.</span><span class="sxs-lookup"><span data-stu-id="60970-123">Edit hello inputs, query, and outputs.</span></span>
   
    <span data-ttu-id="60970-124">Jednoduchých úprav je toochange hello dotazu pro hello **pravidla** toouse úlohy **"<"** místo **">"**.</span><span class="sxs-lookup"><span data-stu-id="60970-124">A simple modification is toochange hello query for hello **Rules** job toouse a **"<"** instead of a **">"**.</span></span> <span data-ttu-id="60970-125">portál řešení Hello stále zobrazuje **">"** při upravit pravidlo, ale Všimněte si, jak je z důvodu změn toohello v hello základní úlohy obráceně hello chování.</span><span class="sxs-lookup"><span data-stu-id="60970-125">hello solution portal still shows **">"** when you edit a rule, but notice how hello behavior is flipped due toohello change in hello underlying job.</span></span>
6. <span data-ttu-id="60970-126">Spuštění úlohy hello</span><span class="sxs-lookup"><span data-stu-id="60970-126">Start hello job</span></span>

> [!NOTE]
> <span data-ttu-id="60970-127">vzdálené monitorování řídicí panel Hello závisí na konkrétní data, tak změnu hello úloh může způsobit toofail hello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="60970-127">hello remote monitoring dashboard depends on specific data, so altering hello jobs can cause hello dashboard toofail.</span></span>

## <a name="add-your-own-rules"></a><span data-ttu-id="60970-128">Přidání vlastních pravidel</span><span class="sxs-lookup"><span data-stu-id="60970-128">Add your own rules</span></span>

<span data-ttu-id="60970-129">Kromě toho toochanging hello předkonfigurované úlohy Azure Stream Analytics, můžete použít nové úlohy Azure portálu tooadd hello nebo přidat nové úlohy tooexisting dotazy.</span><span class="sxs-lookup"><span data-stu-id="60970-129">In addition toochanging hello preconfigured Azure Stream Analytics jobs, you can use hello Azure portal tooadd new jobs or add new queries tooexisting jobs.</span></span>

## <a name="customize-devices"></a><span data-ttu-id="60970-130">Vlastní nastavení zařízení</span><span class="sxs-lookup"><span data-stu-id="60970-130">Customize devices</span></span>

<span data-ttu-id="60970-131">Mezi nejběžnější aktivity rozšíření hello ve spolupráci s konkrétní tooyour scénář zařízení.</span><span class="sxs-lookup"><span data-stu-id="60970-131">One of hello most common extension activities is working with devices specific tooyour scenario.</span></span> <span data-ttu-id="60970-132">Existuje několik metod pro práci s zařízení.</span><span class="sxs-lookup"><span data-stu-id="60970-132">There are several methods for working with devices.</span></span> <span data-ttu-id="60970-133">Tyto metody zahrnují změny simulované zařízení toomatch váš scénář, nebo pomocí hello [sady SDK zařízení IoT] [ IoT Device SDK] tooconnect řešení toohello fyzického zařízení.</span><span class="sxs-lookup"><span data-stu-id="60970-133">These methods include altering a simulated device toomatch your scenario, or using hello [IoT Device SDK][IoT Device SDK] tooconnect your physical device toohello solution.</span></span>

<span data-ttu-id="60970-134">Podrobný průvodce tooadding zařízeních, najdete v části hello [Iot Suite připojení zařízení](iot-suite-connecting-devices.md) článek a hello [vzdálené monitorování C SDK – ukázka](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span><span class="sxs-lookup"><span data-stu-id="60970-134">For a step-by-step guide tooadding devices, see hello [Iot Suite Connecting Devices](iot-suite-connecting-devices.md) article and hello [remote monitoring C SDK Sample](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span></span> <span data-ttu-id="60970-135">Tato ukázka je navrženou toowork s hello předkonfigurovanému řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="60970-135">This sample is designed toowork with hello remote monitoring preconfigured solution.</span></span>

### <a name="create-your-own-simulated-device"></a><span data-ttu-id="60970-136">Vytvoření simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="60970-136">Create your own simulated device</span></span>

<span data-ttu-id="60970-137">Součástí hello [pro vzdálené monitorování řešení zdrojového kódu](https://github.com/Azure/azure-iot-remote-monitoring), je simulátoru .NET.</span><span class="sxs-lookup"><span data-stu-id="60970-137">Included in hello [remote monitoring solution source code](https://github.com/Azure/azure-iot-remote-monitoring), is a .NET simulator.</span></span> <span data-ttu-id="60970-138">Tato simulátoru je hello jeden zřízený jako součást řešení hello a můžete změnu toosend rozdílná metadata, telemetrických dat a odpovědět, toodifferent příkazy a metody.</span><span class="sxs-lookup"><span data-stu-id="60970-138">This simulator is hello one provisioned as part of hello solution and you can alter it toosend different metadata, telemetry, and respond toodifferent commands and methods.</span></span>

<span data-ttu-id="60970-139">předkonfigurované simulátoru Hello v hello předkonfigurovanému řešení vzdáleného monitorování simuluje chladič zařízení, který vysílá telemetrická data teploty a vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="60970-139">hello preconfigured simulator in hello remote monitoring preconfigured solution simulates a cooler device that emits temperature and humidity telemetry.</span></span> <span data-ttu-id="60970-140">Můžete upravit hello simulátoru v hello [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projektu, když jste forked úložiště GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="60970-140">You can modify hello simulator in hello [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) project when you've forked hello GitHub repository.</span></span>

### <a name="available-locations-for-simulated-devices"></a><span data-ttu-id="60970-141">K dispozici umístění pro simulované zařízení</span><span class="sxs-lookup"><span data-stu-id="60970-141">Available locations for simulated devices</span></span>

<span data-ttu-id="60970-142">Sada Hello výchozí umístění je v Praze/Redmond, Washington, USA.</span><span class="sxs-lookup"><span data-stu-id="60970-142">hello default set of locations is in Seattle/Redmond, Washington, United States of America.</span></span> <span data-ttu-id="60970-143">Můžete změnit tato místa v [SampleDeviceFactory.cs][lnk-sample-device-factory].</span><span class="sxs-lookup"><span data-stu-id="60970-143">You can change these locations in [SampleDeviceFactory.cs][lnk-sample-device-factory].</span></span>

### <a name="add-a-desired-property-update-handler-toohello-simulator"></a><span data-ttu-id="60970-144">Přidat simulátoru toohello obslužná rutina aktualizace požadované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="60970-144">Add a desired property update handler toohello simulator</span></span>

<span data-ttu-id="60970-145">Můžete nastavit hodnotu pro požadovanou vlastnost pro zařízení na portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="60970-145">You can set a value for a desired property for a device in hello solution portal.</span></span> <span data-ttu-id="60970-146">Pokud zařízení hello načte hodnotu vlastnosti hello potřeby je hello odpovědností hello zařízení toohandle hello vlastnost žádosti o změnu.</span><span class="sxs-lookup"><span data-stu-id="60970-146">It is hello responsibility of hello device toohandle hello property change request when hello device retrieves hello desired property value.</span></span> <span data-ttu-id="60970-147">Podpora tooadd pro změnu hodnoty vlastnosti prostřednictvím požadovanou vlastnost, je nutné tooadd simulátoru toohello obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="60970-147">tooadd support for a property value change through a desired property, you need tooadd a handler toohello simulator.</span></span>

<span data-ttu-id="60970-148">Simulátor Hello obsahuje obslužné rutiny pro hello **SetPointTemp** a **TelemetryInterval** vlastnosti, které lze aktualizovat nastavením požadované hodnoty v portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="60970-148">hello simulator contains handlers for hello **SetPointTemp** and **TelemetryInterval** properties that you can update by setting desired values in hello solution portal.</span></span>

<span data-ttu-id="60970-149">Hello následující příklad ukazuje hello obslužné rutiny pro hello **SetPointTemp** potřeby vlastnost hello **CoolerDevice** třídy:</span><span class="sxs-lookup"><span data-stu-id="60970-149">hello following example shows hello handler for hello **SetPointTemp** desired property in hello **CoolerDevice** class:</span></span>

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

<span data-ttu-id="60970-150">Tato metoda hello telemetrie bod teploty a pak sestavy hello změnit back tooIoT rozbočovače nastavením hlášené vlastnosti aktualizace.</span><span class="sxs-lookup"><span data-stu-id="60970-150">This method updates hello telemetry point temperature and then reports hello change back tooIoT Hub by setting a reported property.</span></span>

<span data-ttu-id="60970-151">Pomocí následující vzor hello v předchozím příkladu hello můžete přidat svoje vlastní obslužné rutiny pro vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="60970-151">You can add your own handlers for your own properties by following hello pattern in hello preceding example.</span></span>

<span data-ttu-id="60970-152">Je třeba také svázat obslužná rutina toohello hello požadovanou vlastnost, jak ukazuje následující příklad z hello hello **CoolerDevice** konstruktor:</span><span class="sxs-lookup"><span data-stu-id="60970-152">You must also bind hello desired property toohello handler as shown in hello following example from hello **CoolerDevice** constructor:</span></span>

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

<span data-ttu-id="60970-153">Všimněte si, že **SetPointTempPropertyName** konstanta definovaný jako "Config.SetPointTemp".</span><span class="sxs-lookup"><span data-stu-id="60970-153">Note that **SetPointTempPropertyName** is a constant defined as "Config.SetPointTemp".</span></span>

### <a name="add-support-for-a-new-method-toohello-simulator"></a><span data-ttu-id="60970-154">Přidat podporu pro nové simulátoru toohello – metoda</span><span class="sxs-lookup"><span data-stu-id="60970-154">Add support for a new method toohello simulator</span></span>

<span data-ttu-id="60970-155">Můžete přizpůsobit hello simulátoru tooadd podporu pro nové [– metoda (přímá metoda)][lnk-direct-methods].</span><span class="sxs-lookup"><span data-stu-id="60970-155">You can customize hello simulator tooadd support for a new [method (direct method)][lnk-direct-methods].</span></span> <span data-ttu-id="60970-156">Existují dva klíče kroky:</span><span class="sxs-lookup"><span data-stu-id="60970-156">There are two key steps required:</span></span>

- <span data-ttu-id="60970-157">Simulátor Hello musíte upozornit centra IoT hello hello předkonfigurované řešení s podrobnostmi metody hello.</span><span class="sxs-lookup"><span data-stu-id="60970-157">hello simulator must notify hello IoT hub in hello preconfigured solution with details of hello method.</span></span>
- <span data-ttu-id="60970-158">Simulátor Hello musí obsahovat volání metody hello toohandle kód při vyvolání z hello **podrobnosti o zařízení** panely v Průzkumníku řešení hello nebo prostřednictvím úlohu.</span><span class="sxs-lookup"><span data-stu-id="60970-158">hello simulator must include code toohandle hello method call when you invoke it from hello **Device details** panel in hello solution explorer or through a job.</span></span>

<span data-ttu-id="60970-159">Hello vzdálené monitorování předkonfigurované řešení používá *hlášené vlastnosti* toosend podrobnosti o podporovaných metod tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="60970-159">hello remote monitoring preconfigured solution uses *reported properties* toosend details of supported methods tooIoT hub.</span></span> <span data-ttu-id="60970-160">back-end Hello řešení udržuje seznam všech metod hello podporovaných každé zařízení společně s historie volání metod.</span><span class="sxs-lookup"><span data-stu-id="60970-160">hello solution back end maintains a list of all hello methods supported by each device along with a history of method invocations.</span></span> <span data-ttu-id="60970-161">Můžete zobrazit tyto informace o zařízeních a volat metody na portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="60970-161">You can view this information about devices and invoke methods in hello solution portal.</span></span>

<span data-ttu-id="60970-162">toonotify hello Centrum IoT hub, zařízení podporuje metodu hello zařízení musíte přidat podrobnosti o hello metoda toohello **SupportedMethods** uzel v hello hlášené vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="60970-162">toonotify hello IoT hub that a device supports a method, hello device must add details of hello method toohello **SupportedMethods** node in hello reported properties:</span></span>

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

<span data-ttu-id="60970-163">Hello podpis metody má hello následující formát: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span><span class="sxs-lookup"><span data-stu-id="60970-163">hello method signature has hello following format: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span></span> <span data-ttu-id="60970-164">Například toospecify hello **InitiateFirmwareUpdate** metoda očekává parametr řetězec s názvem **FwPackageURI**, použijte následující podpis metody hello:</span><span class="sxs-lookup"><span data-stu-id="60970-164">For example, toospecify hello **InitiateFirmwareUpdate** method expects a string parameter named **FwPackageURI**, use hello following method signature:</span></span>

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

<span data-ttu-id="60970-165">Seznam typů podporovaných parametrů najdete v tématu hello **CommandTypes** třídy v projektu OMI hello.</span><span class="sxs-lookup"><span data-stu-id="60970-165">For a list of supported parameter types, see hello **CommandTypes** class in hello Infrastructure project.</span></span>

<span data-ttu-id="60970-166">toodelete metodu, nastavení hello metoda podpis příliš`null` v hello hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="60970-166">toodelete a method, set hello method signature too`null` in hello reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="60970-167">Hello back-end řešení aktualizuje pouze informace o podporovaných metod při přijetí *informace o zařízení* zpráv ze zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="60970-167">hello solution back end only updates information about supported methods when it receives a *device information* message from hello device.</span></span>

<span data-ttu-id="60970-168">Následující ukázka kódu z hello Hello **SampleDeviceFactory** třídy v hello běžné projektu ukazuje, jak tooadd seznam metoda toohello z **SupportedMethods** v hello hlášené vlastnosti poslal hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="60970-168">hello following code sample from hello **SampleDeviceFactory** class in hello Common project shows how tooadd a method toohello list of **SupportedMethods** in hello reported properties sent by hello device:</span></span>

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' toospecifiy hello URI of hello firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

<span data-ttu-id="60970-169">Tento fragment kódu přidá podrobnosti o hello **InitiateFirmwareUpdate** metoda včetně toodisplay textu hello portál řešení a podrobnosti o hello požadované parametry metody.</span><span class="sxs-lookup"><span data-stu-id="60970-169">This code snippet adds details of hello **InitiateFirmwareUpdate** method including text toodisplay in hello solution portal and details of hello required method parameters.</span></span>

<span data-ttu-id="60970-170">Simulátor Hello odešle hlášené vlastnosti, včetně hello seznam podporovaných metod tooIoT centra, když se spustí simulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="60970-170">hello simulator sends reported properties, including hello list of supported methods, tooIoT Hub when hello simulator starts.</span></span>

<span data-ttu-id="60970-171">Přidejte kód simulátoru toohello obslužné rutiny pro každou metodu, kterou podporuje.</span><span class="sxs-lookup"><span data-stu-id="60970-171">Add a handler toohello simulator code for each method it supports.</span></span> <span data-ttu-id="60970-172">Zobrazí se stávající obslužné rutiny hello v hello **CoolerDevice** – třída v projektu Simulator.WebJob hello.</span><span class="sxs-lookup"><span data-stu-id="60970-172">You can see hello existing handlers in hello **CoolerDevice** class in hello Simulator.WebJob project.</span></span> <span data-ttu-id="60970-173">Hello následující příklad ukazuje hello obslužné rutiny pro **InitiateFirmwareUpdate** metoda:</span><span class="sxs-lookup"><span data-stu-id="60970-173">hello following example shows hello handler for **InitiateFirmwareUpdate** method:</span></span>

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

<span data-ttu-id="60970-174">Metoda obslužná rutina názvy musí začínat `On` následuje název hello hello metody.</span><span class="sxs-lookup"><span data-stu-id="60970-174">Method handler names must start with `On` followed by hello name of hello method.</span></span> <span data-ttu-id="60970-175">Hello **methodRequest** parametr obsahuje žádné parametry předané pomocí volání metody hello z back-end hello řešení.</span><span class="sxs-lookup"><span data-stu-id="60970-175">hello **methodRequest** parameter contains any parameters passed with hello method invocation from hello solution back end.</span></span> <span data-ttu-id="60970-176">Hello návratová hodnota musí být typu **úloh&lt;MethodResponse&gt;**.</span><span class="sxs-lookup"><span data-stu-id="60970-176">hello return value must be of type **Task&lt;MethodResponse&gt;**.</span></span> <span data-ttu-id="60970-177">Hello **BuildMethodResponse** nástroj metoda vám pomůže vytvořit hello návratovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="60970-177">hello **BuildMethodResponse** utility method helps you create hello return value.</span></span>

<span data-ttu-id="60970-178">Uvnitř hello metoda obslužnou rutinu vám může:</span><span class="sxs-lookup"><span data-stu-id="60970-178">Inside hello method handler, you could:</span></span>

- <span data-ttu-id="60970-179">Spustí asynchronní úlohu.</span><span class="sxs-lookup"><span data-stu-id="60970-179">Start an asynchronous task.</span></span>
- <span data-ttu-id="60970-180">Načíst požadované vlastnosti z hello *dvojče zařízení* IoT hub.</span><span class="sxs-lookup"><span data-stu-id="60970-180">Retrieve desired properties from hello *device twin* in IoT Hub.</span></span>
- <span data-ttu-id="60970-181">Aktualizovat vlastnosti jediné hlášené pomocí hello **SetReportedPropertyAsync** metoda v hello **CoolerDevice** třídy.</span><span class="sxs-lookup"><span data-stu-id="60970-181">Update a single reported property using hello **SetReportedPropertyAsync** method in hello **CoolerDevice** class.</span></span>
- <span data-ttu-id="60970-182">Aktualizovat více hlášené vlastnosti tak, že vytvoříte **TwinCollection** instance a volání hello **Transport.UpdateReportedPropertiesAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="60970-182">Update multiple reported properties by creating a **TwinCollection** instance and calling hello **Transport.UpdateReportedPropertiesAsync** method.</span></span>

<span data-ttu-id="60970-183">Hello předchozí příklad aktualizace firmwaru provádí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60970-183">hello preceding firmware update example performs hello following steps:</span></span>

- <span data-ttu-id="60970-184">Kontroly hello zařízení je možné tooaccept žádost o aktualizaci firmwaru hello.</span><span class="sxs-lookup"><span data-stu-id="60970-184">Checks hello device is able tooaccept hello firmware update request.</span></span>
- <span data-ttu-id="60970-185">Asynchronně spustí operace aktualizace firmwaru hello a obnoví hello telemetrie při dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="60970-185">Asynchronously initiates hello firmware update operation and resets hello telemetry when hello operation is complete.</span></span>
- <span data-ttu-id="60970-186">Hned vrátí hello "FirmwareUpdate přijata" zpráva tooindicate hello žádost byla přijata hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="60970-186">Immediately returns hello "FirmwareUpdate accepted" message tooindicate hello request was accepted by hello device.</span></span>

### <a name="build-and-use-your-own-physical-device"></a><span data-ttu-id="60970-187">Vytvoření a použití si vlastní zařízení (fyzické)</span><span class="sxs-lookup"><span data-stu-id="60970-187">Build and use your own (physical) device</span></span>

<span data-ttu-id="60970-188">Hello [SDK služby Azure IoT](https://github.com/Azure/azure-iot-sdks) poskytují knihovny pro připojení (jazyky a operační systémy) celou řadu typů zařízení do řešení IoT.</span><span class="sxs-lookup"><span data-stu-id="60970-188">hello [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) provide libraries for connecting numerous device types (languages and operating systems) into IoT solutions.</span></span>

## <a name="modify-dashboard-limits"></a><span data-ttu-id="60970-189">Upravit omezení řídicí panel</span><span class="sxs-lookup"><span data-stu-id="60970-189">Modify dashboard limits</span></span>

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a><span data-ttu-id="60970-190">Počet zařízení, které jsou zobrazeny v rozevírací nabídce řídicí panel</span><span class="sxs-lookup"><span data-stu-id="60970-190">Number of devices displayed in dashboard dropdown</span></span>

<span data-ttu-id="60970-191">Hello výchozí hodnota je 200.</span><span class="sxs-lookup"><span data-stu-id="60970-191">hello default is 200.</span></span> <span data-ttu-id="60970-192">Můžete změnit toto číslo v [DashboardController.cs][lnk-dashboard-controller].</span><span class="sxs-lookup"><span data-stu-id="60970-192">You can change this number in [DashboardController.cs][lnk-dashboard-controller].</span></span>

### <a name="number-of-pins-toodisplay-in-bing-map-control"></a><span data-ttu-id="60970-193">Počet kódů PIN toodisplay v ovládacím prvku mapy Bing</span><span class="sxs-lookup"><span data-stu-id="60970-193">Number of pins toodisplay in Bing Map control</span></span>

<span data-ttu-id="60970-194">Hello výchozí hodnota je 200.</span><span class="sxs-lookup"><span data-stu-id="60970-194">hello default is 200.</span></span> <span data-ttu-id="60970-195">Můžete změnit toto číslo v [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span><span class="sxs-lookup"><span data-stu-id="60970-195">You can change this number in [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span></span>

### <a name="time-period-of-telemetry-graph"></a><span data-ttu-id="60970-196">Časové období telemetrických dat grafu</span><span class="sxs-lookup"><span data-stu-id="60970-196">Time period of telemetry graph</span></span>

<span data-ttu-id="60970-197">Hello výchozí hodnota je 10 minut.</span><span class="sxs-lookup"><span data-stu-id="60970-197">hello default is 10 minutes.</span></span> <span data-ttu-id="60970-198">Můžete změnit tuto hodnotu v [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span><span class="sxs-lookup"><span data-stu-id="60970-198">You can change this value in [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span></span>

## <a name="manually-set-up-application-roles"></a><span data-ttu-id="60970-199">Ručně nastavit aplikační role</span><span class="sxs-lookup"><span data-stu-id="60970-199">Manually set up application roles</span></span>

<span data-ttu-id="60970-200">Hello následující postup popisuje, jak tooadd **správce** a **jen pro čtení** aplikace role tooa předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="60970-200">hello following procedure describes how tooadd **Admin** and **ReadOnly** application roles tooa preconfigured solution.</span></span> <span data-ttu-id="60970-201">Předkonfigurovaná řešení, které jsou už zřízené z webu azureiotsuite.com hello obsahují hello **správce** a **jen pro čtení** role.</span><span class="sxs-lookup"><span data-stu-id="60970-201">Note that preconfigured solutions provisioned from hello azureiotsuite.com site already include hello **Admin** and **ReadOnly** roles.</span></span>

<span data-ttu-id="60970-202">Členové hello **jen pro čtení** role můžete zobrazit seznam zařízení hello a hello řídicího panelu, ale nejsou povoleny tooadd zařízení, změny atributů zařízení nebo odesílání příkazů.</span><span class="sxs-lookup"><span data-stu-id="60970-202">Members of hello **ReadOnly** role can see hello dashboard and hello device list, but are not allowed tooadd devices, change device attributes, or send commands.</span></span>  <span data-ttu-id="60970-203">Členové hello **správce** role mají plný přístup tooall hello funkci v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="60970-203">Members of hello **Admin** role have full access tooall hello functionality in hello solution.</span></span>

1. <span data-ttu-id="60970-204">Přejděte toohello [portál Azure classic][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="60970-204">Go toohello [Azure classic portal][lnk-classic-portal].</span></span>
2. <span data-ttu-id="60970-205">Vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="60970-205">Select **Active Directory**.</span></span>
3. <span data-ttu-id="60970-206">Klikněte na název hello hello AAD klienta, který jste použili při zřízení řešení.</span><span class="sxs-lookup"><span data-stu-id="60970-206">Click hello name of hello AAD tenant you used when you provisioned your solution.</span></span>
4. <span data-ttu-id="60970-207">Klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="60970-207">Click **Applications**.</span></span>
5. <span data-ttu-id="60970-208">Klikněte na název hello hello aplikace, která odpovídá názvu vašeho předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="60970-208">Click hello name of hello application that matches your preconfigured solution name.</span></span> <span data-ttu-id="60970-209">Pokud nevidíte aplikaci v seznamu hello, vyberte **aplikace Moje společnost vlastní** v hello **zobrazit** rozevíracího seznamu a klikněte na tlačítko hello zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="60970-209">If you don't see your application in hello list, select **Applications my company owns** in hello **Show** dropdown and click hello check mark.</span></span>
6. <span data-ttu-id="60970-210">V dolní části hello hello stránky, klikněte na tlačítko **spravovat Manifest** a potom **stáhnout Manifest**.</span><span class="sxs-lookup"><span data-stu-id="60970-210">At hello bottom of hello page, click **Manage Manifest** and then **Download Manifest**.</span></span>
7. <span data-ttu-id="60970-211">Tento postup stáhne .json souboru tooyour místním počítači.</span><span class="sxs-lookup"><span data-stu-id="60970-211">This procedure downloads a .json file tooyour local machine.</span></span> <span data-ttu-id="60970-212">Umožňuje otevřete tento soubor pro úpravy v textovém editoru podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="60970-212">Open this file for editing in a text editor of your choice.</span></span>
8. <span data-ttu-id="60970-213">Na třetí řádek hello hello .json souboru se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="60970-213">On hello third line of hello .json file, you can see:</span></span>

   ```json
   "appRoles" : [],
   ```
   <span data-ttu-id="60970-214">Nahraďte tento řádek hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="60970-214">Replace this line with hello following code:</span></span>

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access toohello application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access toodevice information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. <span data-ttu-id="60970-215">Uložte soubor .json aktualizované hello (můžete přepsat existující soubor hello).</span><span class="sxs-lookup"><span data-stu-id="60970-215">Save hello updated .json file (you can overwrite hello existing file).</span></span>
10. <span data-ttu-id="60970-216">V hello portál Azure classic, v dolní části hello hello stránky, vyberte **spravovat Manifest** pak **nahrát Manifest** soubor .json hello tooupload jste uložili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="60970-216">In hello Azure classic portal, at hello bottom of hello page, select **Manage Manifest** then **Upload Manifest** tooupload hello .json file you saved in hello previous step.</span></span>
11. <span data-ttu-id="60970-217">Nyní jste přidali hello **správce** a **jen pro čtení** role tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="60970-217">You have now added hello **Admin** and **ReadOnly** roles tooyour application.</span></span>
12. <span data-ttu-id="60970-218">tooassign jednu z těchto rolí uživatele tooa ve vašem adresáři, najdete v části [oprávnění na webu azureiotsuite.com hello][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="60970-218">tooassign one of these roles tooa user in your directory, see [Permissions on hello azureiotsuite.com site][lnk-permissions].</span></span>

## <a name="feedback"></a><span data-ttu-id="60970-219">Váš názor</span><span class="sxs-lookup"><span data-stu-id="60970-219">Feedback</span></span>

<span data-ttu-id="60970-220">Máte přizpůsobení chcete toosee zahrnuté v tomto dokumentu?</span><span class="sxs-lookup"><span data-stu-id="60970-220">Do you have a customization you'd like toosee covered in this document?</span></span> <span data-ttu-id="60970-221">Přidání funkce návrhy příliš[User Voice](https://feedback.azure.com/forums/321918-azure-iot), nebo komentář k tomuto článku.</span><span class="sxs-lookup"><span data-stu-id="60970-221">Add feature suggestions too[User Voice](https://feedback.azure.com/forums/321918-azure-iot), or comment on this article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="60970-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60970-222">Next steps</span></span>

<span data-ttu-id="60970-223">toolearn Další informace o hello možností pro přizpůsobení hello předkonfigurované řešení, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="60970-223">toolearn more about hello options for customizing hello preconfigured solutions, see:</span></span>

* <span data-ttu-id="60970-224">[Připojení aplikace logiky tooyour Azure IoT Suite vzdálené monitorování předkonfigurované řešení][lnk-logicapp]</span><span class="sxs-lookup"><span data-stu-id="60970-224">[Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapp]</span></span>
* <span data-ttu-id="60970-225">[Použití dynamické telemetrie s hello předkonfigurovanému řešení vzdáleného monitorování][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="60970-225">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="60970-226">[Informace metadat zařízení v hello předkonfigurovanému řešení vzdáleného monitorování][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="60970-226">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo]</span></span>
* <span data-ttu-id="60970-227">[Přizpůsobit způsob hello připojen objekt pro vytváření řešení zobrazí data z vašich serverů OPC UA][lnk-cf-customize]</span><span class="sxs-lookup"><span data-stu-id="60970-227">[Customize how hello connected factory solution displays data from your OPC UA servers][lnk-cf-customize]</span></span>

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