---
title: "aaaCreate vlastní pravidla v sadě Azure IoT Suite | Microsoft Docs"
description: "Jak toocreate vlastní pravidlo v prostředí IoT Suite předkonfigurované řešení."
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
ms.openlocfilehash: 6c5bb2ca54f3f17b99ad482e727c8e9fa28d7fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-rule-in-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="e7308-103">Vytvoření vlastního pravidla v hello předkonfigurovanému řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="e7308-103">Create a custom rule in hello remote monitoring preconfigured solution</span></span>

## <a name="introduction"></a><span data-ttu-id="e7308-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="e7308-104">Introduction</span></span>

<span data-ttu-id="e7308-105">V případě hello předkonfigurované řešení, můžete nakonfigurovat [pravidla, která spouští při telemetrie hodnota zařízení dosáhne určité prahové hodnoty][lnk-builtin-rule].</span><span class="sxs-lookup"><span data-stu-id="e7308-105">In hello preconfigured solutions, you can configure [rules that trigger when a telemetry value for a device reaches a specific threshold][lnk-builtin-rule].</span></span> <span data-ttu-id="e7308-106">[Použití dynamické telemetrie s hello předkonfigurovanému řešení vzdáleného monitorování] [ lnk-dynamic-telemetry] popisuje, jak můžete přidat vlastní telemetrii hodnoty, jako například *ExternalTemperature* tooyour řešení.</span><span class="sxs-lookup"><span data-stu-id="e7308-106">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic-telemetry] describes how you can add custom telemetry values, such as *ExternalTemperature* tooyour solution.</span></span> <span data-ttu-id="e7308-107">Tento článek ukazuje, jak toocreate vlastní pravidlo pro dynamické telemetrie typy ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="e7308-107">This article shows you how toocreate custom rule for dynamic telemetry types in your solution.</span></span>

<span data-ttu-id="e7308-108">Tento kurz používá jednoduchý Node.js simulované zařízení toogenerate dynamické telemetrie toosend toohello předkonfigurované řešení back-end.</span><span class="sxs-lookup"><span data-stu-id="e7308-108">This tutorial uses a simple Node.js simulated device toogenerate dynamic telemetry toosend toohello preconfigured solution back end.</span></span> <span data-ttu-id="e7308-109">Pak přidejte vlastní pravidla ve hello **RemoteMonitoring** řešení sady Visual Studio a nasadit tento vlastní back-end tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="e7308-109">You then add custom rules in hello **RemoteMonitoring** Visual Studio solution and deploy this customized back end tooyour Azure subscription.</span></span>

<span data-ttu-id="e7308-110">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="e7308-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="e7308-111">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e7308-111">An active Azure subscription.</span></span> <span data-ttu-id="e7308-112">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="e7308-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="e7308-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="e7308-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="e7308-114">[Node.js] [ lnk-node] verze 0.12.x nebo novější toocreate simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="e7308-114">[Node.js][lnk-node] version 0.12.x or later toocreate a simulated device.</span></span>
* <span data-ttu-id="e7308-115">Visual Studio 2015 nebo Visual Studio 2017 toomodify hello předkonfigurované back-end řešení s nové pravidel.</span><span class="sxs-lookup"><span data-stu-id="e7308-115">Visual Studio 2015 or Visual Studio 2017 toomodify hello preconfigured solution back end with your new rules.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

<span data-ttu-id="e7308-116">Poznamenejte si název hello řešení, které jste zvolili pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="e7308-116">Make a note of hello solution name you chose for your deployment.</span></span> <span data-ttu-id="e7308-117">Je třeba tento název řešení později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e7308-117">You need this solution name later in this tutorial.</span></span>

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

<span data-ttu-id="e7308-118">Po ověření, že posílá můžete zastavit konzolovou aplikaci softwaru Node.js hello **ExternalTemperature** telemetrie toohello předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="e7308-118">You can stop hello Node.js console app when you have verified that it is sending **ExternalTemperature** telemetry toohello preconfigured solution.</span></span> <span data-ttu-id="e7308-119">Vzhledem k tomu, že spustíte tuto konzolovou aplikaci softwaru Node.js znovu po přidání hello vlastní pravidlo toohello řešení, nechte okno konzoly hello otevřené.</span><span class="sxs-lookup"><span data-stu-id="e7308-119">Keep hello console window open because you run this Node.js console app again after you add hello custom rule toohello solution.</span></span>

## <a name="rule-storage-locations"></a><span data-ttu-id="e7308-120">Pravidlo umístění úložiště</span><span class="sxs-lookup"><span data-stu-id="e7308-120">Rule storage locations</span></span>

<span data-ttu-id="e7308-121">Informace o pravidlech je uchován v dvou umístěních:</span><span class="sxs-lookup"><span data-stu-id="e7308-121">Information about rules is persisted in two locations:</span></span>

* <span data-ttu-id="e7308-122">**DeviceRulesNormalizedTable** tabulce – v této tabulce jsou uloženy normalizovaný odkazovat toohello pravidla definované portál řešení hello.</span><span class="sxs-lookup"><span data-stu-id="e7308-122">**DeviceRulesNormalizedTable** table – This table stores a normalized reference toohello rules defined by hello solution portal.</span></span> <span data-ttu-id="e7308-123">Když hello portál řešení zobrazí pravidla zařízení, vyžádá si tuto tabulku hello Definice pravidla.</span><span class="sxs-lookup"><span data-stu-id="e7308-123">When hello solution portal displays device rules, it queries this table for hello rule definitions.</span></span>
* <span data-ttu-id="e7308-124">**DeviceRules** objektů blob – tento objekt blob ukládá všechna pravidla hello definovaná pro všechny registrované zařízení a je definován jako úlohy Azure Stream Analytics vstupní toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="e7308-124">**DeviceRules** blob – This blob stores all hello rules defined for all registered devices and is defined as a reference input toohello Azure Stream Analytics jobs.</span></span>
 
<span data-ttu-id="e7308-125">Při aktualizaci existující pravidlo nebo definovat nové pravidlo hello řešení portálu, hello tabulky a objektů blob jsou aktualizované tooreflect hello změny.</span><span class="sxs-lookup"><span data-stu-id="e7308-125">When you update an existing rule or define a new rule in hello solution portal, both hello table and blob are updated tooreflect hello changes.</span></span> <span data-ttu-id="e7308-126">pravidlo Hello definice zobrazení na portálu hello pocházejí z tabulky úložiště hello a hello pravidlo, že definice odkazuje úlohy Stream Analytics hello pochází z objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="e7308-126">hello rule definition displayed in hello portal comes from hello table store, and hello rule definition referenced by hello Stream Analytics jobs comes from hello blob.</span></span> 

## <a name="update-hello-remotemonitoring-visual-studio-solution"></a><span data-ttu-id="e7308-127">Aktualizovat řešení sady Visual Studio RemoteMonitoring hello</span><span class="sxs-lookup"><span data-stu-id="e7308-127">Update hello RemoteMonitoring Visual Studio solution</span></span>

<span data-ttu-id="e7308-128">Hello následující kroky vám ukážou, jak toomodify hello tooinclude řešení sady Visual Studio RemoteMonitoring nové pravidlo, které používá hello **ExternalTemperature** telemetrická data odesílaná ze simulovaného zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="e7308-128">hello following steps show you how toomodify hello RemoteMonitoring Visual Studio solution tooinclude a new rule that uses hello **ExternalTemperature** telemetry sent from hello simulated device:</span></span>

1. <span data-ttu-id="e7308-129">Pokud jste tak již neučinili, klonování hello **azure-iot-remote-monitoring** úložiště tooa vhodný umístění na místním počítači pomocí následujícího příkazu Git hello:</span><span class="sxs-lookup"><span data-stu-id="e7308-129">If you have not already done so, clone hello **azure-iot-remote-monitoring** repository tooa suitable location on your local machine using hello following Git command:</span></span>

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. <span data-ttu-id="e7308-130">V sadě Visual Studio otevřete soubor RemoteMonitoring.sln hello z vaší místní kopii hello **azure-iot-remote-monitoring** úložiště.</span><span class="sxs-lookup"><span data-stu-id="e7308-130">In Visual Studio, open hello RemoteMonitoring.sln file from your local copy of hello **azure-iot-remote-monitoring** repository.</span></span>

3. <span data-ttu-id="e7308-131">Otevřete soubor hello Infrastructure\Models\DeviceRuleBlobEntity.cs a přidejte **ExternalTemperature** vlastnost následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e7308-131">Open hello file Infrastructure\Models\DeviceRuleBlobEntity.cs and add an **ExternalTemperature** property as follows:</span></span>

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. <span data-ttu-id="e7308-132">V hello stejný soubor, přidejte **ExternalTemperatureRuleOutput** vlastnost následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e7308-132">In hello same file, add an **ExternalTemperatureRuleOutput** property as follows:</span></span>

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. <span data-ttu-id="e7308-133">Otevřete soubor hello Infrastructure\Models\DeviceRuleDataFields.cs a přidejte následující hello **ExternalTemperature** vlastnost po hello existující **vlhkosti** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e7308-133">Open hello file Infrastructure\Models\DeviceRuleDataFields.cs and add hello following **ExternalTemperature** property after hello existing **Humidity** property:</span></span>

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. <span data-ttu-id="e7308-134">V hello stejného souboru, aktualizujte hello **_availableDataFields** metoda tooinclude **ExternalTemperature** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e7308-134">In hello same file, update hello **_availableDataFields** method tooinclude **ExternalTemperature** as follows:</span></span>

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. <span data-ttu-id="e7308-135">Otevřete soubor hello Infrastructure\Repository\DeviceRulesRepository.cs a upravit hello **BuildBlobEntityListFromTableRows** metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e7308-135">Open hello file Infrastructure\Repository\DeviceRulesRepository.cs and modify hello **BuildBlobEntityListFromTableRows** method as follows:</span></span>

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-hello-solution"></a><span data-ttu-id="e7308-136">Znovu sestavili a nasadili hello řešení.</span><span class="sxs-lookup"><span data-stu-id="e7308-136">Rebuild and redeploy hello solution.</span></span>

<span data-ttu-id="e7308-137">Teď můžete nasadit řešení tooyour hello aktualizovat předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e7308-137">You can now deploy hello updated solution tooyour Azure subscription.</span></span>

1. <span data-ttu-id="e7308-138">Otevřete příkazový řádek se zvýšenými oprávněními a přejděte toohello kořenovém místní kopii úložiště azure-iot-remote-monitoring hello.</span><span class="sxs-lookup"><span data-stu-id="e7308-138">Open an elevated command prompt and navigate toohello root of your local copy of hello azure-iot-remote-monitoring repository.</span></span>

2. <span data-ttu-id="e7308-139">toodeploy řešení aktualizované, spusťte následující příkaz nahraďte hello **{název nasazení}** s názvem hello vašeho nasazení předkonfigurované řešení, které jste si poznamenali dříve:</span><span class="sxs-lookup"><span data-stu-id="e7308-139">toodeploy your updated solution, run hello following command substituting **{deployment name}** with hello name of your preconfigured solution deployment that you noted previously:</span></span>

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-hello-stream-analytics-job"></a><span data-ttu-id="e7308-140">Úloha Stream Analytics hello aktualizace</span><span class="sxs-lookup"><span data-stu-id="e7308-140">Update hello Stream Analytics job</span></span>

<span data-ttu-id="e7308-141">Po dokončení nasazení hello můžete aktualizovat hello Stream Analytics toouse hello nové pravidel definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="e7308-141">When hello deployment is complete, you can update hello Stream Analytics job toouse hello new rule definitions.</span></span>

1. <span data-ttu-id="e7308-142">V hello portálu Azure přejděte toohello skupinu prostředků, která obsahuje vaše prostředky předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="e7308-142">In hello Azure portal, navigate toohello resource group that contains your preconfigured solution resources.</span></span> <span data-ttu-id="e7308-143">Tato skupina prostředků má stejný název, kterou jste zadali pro hello hello řešení během nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="e7308-143">This resource group has hello same name you specified for hello solution during hello deployment.</span></span>

2. <span data-ttu-id="e7308-144">Přejděte toohello {název nasazení}-úlohy Stream Analytics pravidla.</span><span class="sxs-lookup"><span data-stu-id="e7308-144">Navigate toohello {deployment name}-Rules Stream Analytics job.</span></span> 

3. <span data-ttu-id="e7308-145">Klikněte na tlačítko **Zastavit** úlohy služby Stream Analytics hello toostop spuštění.</span><span class="sxs-lookup"><span data-stu-id="e7308-145">Click **Stop** toostop hello Stream Analytics job from running.</span></span> <span data-ttu-id="e7308-146">(Je nutné počkat hello streamování úlohy toostop před úpravou dotazu hello).</span><span class="sxs-lookup"><span data-stu-id="e7308-146">(You must wait for hello streaming job toostop before you can edit hello query).</span></span>

4. <span data-ttu-id="e7308-147">Klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="e7308-147">Click **Query**.</span></span> <span data-ttu-id="e7308-148">Upravit hello dotazu tooinclude hello **vyberte** příkaz pro **ExternalTemperature**.</span><span class="sxs-lookup"><span data-stu-id="e7308-148">Edit hello query tooinclude hello **SELECT** statement for **ExternalTemperature**.</span></span> <span data-ttu-id="e7308-149">Hello následující příklad ukazuje hello dokončení dotazu s hello nové **vyberte** příkaz:</span><span class="sxs-lookup"><span data-stu-id="e7308-149">hello following sample shows hello complete query with hello new **SELECT** statement:</span></span>

    ```
    WITH AlarmsData AS 
    (
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Temperature' as ReadingType,
         Stream.Temperature as Reading,
         Ref.Temperature as Threshold,
         Ref.TemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Humidity' as ReadingType,
         Stream.Humidity as Reading,
         Ref.Humidity as Threshold,
         Ref.HumidityRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. <span data-ttu-id="e7308-150">Klikněte na tlačítko **Uložit** toochange hello aktualizovat pravidlo dotazu.</span><span class="sxs-lookup"><span data-stu-id="e7308-150">Click **Save** toochange hello updated rule query.</span></span>

6. <span data-ttu-id="e7308-151">Klikněte na tlačítko **spustit** toostart hello Stream Analytics úlohu znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="e7308-151">Click **Start** toostart hello Stream Analytics job running again.</span></span>

## <a name="add-your-new-rule-in-hello-dashboard"></a><span data-ttu-id="e7308-152">Přidat nové pravidlo v řídicím panelu hello</span><span class="sxs-lookup"><span data-stu-id="e7308-152">Add your new rule in hello dashboard</span></span>

<span data-ttu-id="e7308-153">Nyní můžete přidat hello **ExternalTemperature** pravidlo tooa zařízení na řídicím panelu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="e7308-153">You can now add hello **ExternalTemperature** rule tooa device in hello solution dashboard.</span></span>

1. <span data-ttu-id="e7308-154">Přejděte toohello portál řešení.</span><span class="sxs-lookup"><span data-stu-id="e7308-154">Navigate toohello solution portal.</span></span>

2. <span data-ttu-id="e7308-155">Přejděte toohello **zařízení** panelu.</span><span class="sxs-lookup"><span data-stu-id="e7308-155">Navigate toohello **Devices** panel.</span></span>

3. <span data-ttu-id="e7308-156">Vyhledejte vlastní zařízení hello jste vytvořili, který odesílá **ExternalTemperature** telemetrie a na hello **podrobnosti o zařízení** panelu, klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="e7308-156">Locate hello custom device you created that sends **ExternalTemperature** telemetry and on hello **Device Details** panel, click **Add Rule**.</span></span>

4. <span data-ttu-id="e7308-157">Vyberte **ExternalTemperature** v **datové pole**.</span><span class="sxs-lookup"><span data-stu-id="e7308-157">Select **ExternalTemperature** in **Data Field**.</span></span>

5. <span data-ttu-id="e7308-158">Nastavit **prahová hodnota** too56.</span><span class="sxs-lookup"><span data-stu-id="e7308-158">Set **Threshold** too56.</span></span> <span data-ttu-id="e7308-159">Pak klikněte na tlačítko **uložit a zobrazit pravidla**.</span><span class="sxs-lookup"><span data-stu-id="e7308-159">Then click **Save and view rules**.</span></span>

6. <span data-ttu-id="e7308-160">Vrátí historie alarmů toohello řídicí panel tooview hello.</span><span class="sxs-lookup"><span data-stu-id="e7308-160">Return toohello dashboard tooview hello alarm history.</span></span>

7. <span data-ttu-id="e7308-161">V okně konzoly hello je ponechány otevřené spustit odesílání hello Node.js konzole aplikace toobegin **ExternalTemperature** telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="e7308-161">In hello console window you left open, start hello Node.js console app toobegin sending **ExternalTemperature** telemetry data.</span></span>

8. <span data-ttu-id="e7308-162">Všimněte si, že hello **historie alarmů** tabulka ukazuje nové výstrahy, když se aktivuje hello nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="e7308-162">Notice that hello **Alarm History** table shows new alarms when hello new rule is triggered.</span></span>
 
## <a name="additional-information"></a><span data-ttu-id="e7308-163">Další informace</span><span class="sxs-lookup"><span data-stu-id="e7308-163">Additional information</span></span>

<span data-ttu-id="e7308-164">Změna hello operátor  **>**  je složitější a překročí hello kroků uvedených v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e7308-164">Changing hello operator **>** is more complex and goes beyond hello steps outlined in this tutorial.</span></span> <span data-ttu-id="e7308-165">Při toouse úlohy Stream Analytics hello můžete změnit ať operátor se vám líbí, odrážející tento operátor portálu řešení hello je složitější úlohy.</span><span class="sxs-lookup"><span data-stu-id="e7308-165">While you can change hello Stream Analytics job toouse whatever operator you like, reflecting that operator in hello solution portal is a more complex task.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e7308-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7308-166">Next steps</span></span>
<span data-ttu-id="e7308-167">Teď, když už víte, jak toocreate vlastní pravidla, získáte další informace o hello předkonfigurované řešení:</span><span class="sxs-lookup"><span data-stu-id="e7308-167">Now that you've seen how toocreate custom rules, you can learn more about hello preconfigured solutions:</span></span>

- <span data-ttu-id="e7308-168">[Připojení aplikace logiky tooyour Azure IoT Suite vzdálené monitorování předkonfigurované řešení][lnk-logic-app]</span><span class="sxs-lookup"><span data-stu-id="e7308-168">[Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logic-app]</span></span>
- <span data-ttu-id="e7308-169">[Informace metadat zařízení v hello vzdálené monitorování předkonfigurované řešení][lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="e7308-169">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md