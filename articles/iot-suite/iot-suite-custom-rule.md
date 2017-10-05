---
title: "Vytvoření vlastního pravidla v sadě Azure IoT Suite | Microsoft Docs"
description: "Postup vytvoření vlastního pravidla v sadě IoT Suite předkonfigurované řešení."
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
ms.openlocfilehash: d58c27234ea05a82aaa3e8d72f70c1449980df09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="178c9-103">Vytvoření vlastního pravidla v předkonfigurovaného řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="178c9-103">Create a custom rule in the remote monitoring preconfigured solution</span></span>

## <a name="introduction"></a><span data-ttu-id="178c9-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="178c9-104">Introduction</span></span>

<span data-ttu-id="178c9-105">V předkonfigurovaných řešení, můžete nakonfigurovat [pravidla, která spouští při telemetrie hodnota zařízení dosáhne určité prahové hodnoty][lnk-builtin-rule].</span><span class="sxs-lookup"><span data-stu-id="178c9-105">In the preconfigured solutions, you can configure [rules that trigger when a telemetry value for a device reaches a specific threshold][lnk-builtin-rule].</span></span> <span data-ttu-id="178c9-106">[Použití dynamické telemetrie s vzdálené monitorování předkonfigurované řešení] [ lnk-dynamic-telemetry] popisuje, jak můžete přidat vlastní telemetrii hodnoty, jako například *ExternalTemperature* do řešení.</span><span class="sxs-lookup"><span data-stu-id="178c9-106">[Use dynamic telemetry with the remote monitoring preconfigured solution][lnk-dynamic-telemetry] describes how you can add custom telemetry values, such as *ExternalTemperature* to your solution.</span></span> <span data-ttu-id="178c9-107">Tento článek ukazuje, jak vytvořit vlastní pravidlo pro dynamické telemetrie typy ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="178c9-107">This article shows you how to create custom rule for dynamic telemetry types in your solution.</span></span>

<span data-ttu-id="178c9-108">Tento kurz používá jednoduchý Node.js simulované zařízení k vygenerování dynamických telemetrie k odeslání do back-end předkonfigurovaných řešení.</span><span class="sxs-lookup"><span data-stu-id="178c9-108">This tutorial uses a simple Node.js simulated device to generate dynamic telemetry to send to the preconfigured solution back end.</span></span> <span data-ttu-id="178c9-109">Potom přidáte vlastní pravidla v **RemoteMonitoring** řešení sady Visual Studio a nasadit tento vlastní back-end do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="178c9-109">You then add custom rules in the **RemoteMonitoring** Visual Studio solution and deploy this customized back end to your Azure subscription.</span></span>

<span data-ttu-id="178c9-110">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="178c9-110">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="178c9-111">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="178c9-111">An active Azure subscription.</span></span> <span data-ttu-id="178c9-112">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="178c9-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="178c9-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="178c9-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="178c9-114">[Node.js] [ lnk-node] verze 0.12.x nebo novější k vytvoření simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="178c9-114">[Node.js][lnk-node] version 0.12.x or later to create a simulated device.</span></span>
* <span data-ttu-id="178c9-115">K úpravě předkonfigurované řešení Visual Studio 2015 nebo Visual Studio 2017 končit nové pravidel.</span><span class="sxs-lookup"><span data-stu-id="178c9-115">Visual Studio 2015 or Visual Studio 2017 to modify the preconfigured solution back end with your new rules.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

<span data-ttu-id="178c9-116">Poznamenejte si název řešení, které jste zvolili pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="178c9-116">Make a note of the solution name you chose for your deployment.</span></span> <span data-ttu-id="178c9-117">Je třeba tento název řešení později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="178c9-117">You need this solution name later in this tutorial.</span></span>

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

<span data-ttu-id="178c9-118">Po ověření, že posílá můžete zastavit konzolovou aplikaci softwaru Node.js **ExternalTemperature** telemetrie s předkonfigurovaným řešením.</span><span class="sxs-lookup"><span data-stu-id="178c9-118">You can stop the Node.js console app when you have verified that it is sending **ExternalTemperature** telemetry to the preconfigured solution.</span></span> <span data-ttu-id="178c9-119">V okně konzoly zachovat otevřít, protože je znovu spustit tuto konzolovou aplikaci softwaru Node.js po do řešení přidat vlastní pravidlo.</span><span class="sxs-lookup"><span data-stu-id="178c9-119">Keep the console window open because you run this Node.js console app again after you add the custom rule to the solution.</span></span>

## <a name="rule-storage-locations"></a><span data-ttu-id="178c9-120">Pravidlo umístění úložiště</span><span class="sxs-lookup"><span data-stu-id="178c9-120">Rule storage locations</span></span>

<span data-ttu-id="178c9-121">Informace o pravidlech je uchován v dvou umístěních:</span><span class="sxs-lookup"><span data-stu-id="178c9-121">Information about rules is persisted in two locations:</span></span>

* <span data-ttu-id="178c9-122">**DeviceRulesNormalizedTable** tabulce – v této tabulce jsou uloženy normalizovaný odkaz z pravidel definovaných pomocí portálu řešení.</span><span class="sxs-lookup"><span data-stu-id="178c9-122">**DeviceRulesNormalizedTable** table – This table stores a normalized reference to the rules defined by the solution portal.</span></span> <span data-ttu-id="178c9-123">Když na portálu řešení zobrazuje pravidla zařízení, vyžádá si tuto tabulku definice pravidla.</span><span class="sxs-lookup"><span data-stu-id="178c9-123">When the solution portal displays device rules, it queries this table for the rule definitions.</span></span>
* <span data-ttu-id="178c9-124">**DeviceRules** objektů blob – tento objekt blob ukládá všechna pravidla pro všechny registrované zařízení a je definován jako odkaz na vstup do úlohy Azure Stream Analytics definována.</span><span class="sxs-lookup"><span data-stu-id="178c9-124">**DeviceRules** blob – This blob stores all the rules defined for all registered devices and is defined as a reference input to the Azure Stream Analytics jobs.</span></span>
 
<span data-ttu-id="178c9-125">Při aktualizaci existující pravidlo nebo definovat nové pravidlo na portálu řešení, tabulky a objektů blob jsou aktualizovány tak, aby odrážely změny.</span><span class="sxs-lookup"><span data-stu-id="178c9-125">When you update an existing rule or define a new rule in the solution portal, both the table and blob are updated to reflect the changes.</span></span> <span data-ttu-id="178c9-126">Definice pravidla zobrazí na portálu pochází z tabulky úložiště a definice pravidla odkazuje úlohy Stream Analytics pocházejí z objektu blob.</span><span class="sxs-lookup"><span data-stu-id="178c9-126">The rule definition displayed in the portal comes from the table store, and the rule definition referenced by the Stream Analytics jobs comes from the blob.</span></span> 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a><span data-ttu-id="178c9-127">Aktualizace řešení RemoteMonitoring Visual Studio</span><span class="sxs-lookup"><span data-stu-id="178c9-127">Update the RemoteMonitoring Visual Studio solution</span></span>

<span data-ttu-id="178c9-128">Následující kroky vám ukážou, jak upravit řešení nástroje RemoteMonitoring Visual Studio zahrnout nové pravidlo, které používá **ExternalTemperature** telemetrická data odesílaná ze simulovaného zařízení:</span><span class="sxs-lookup"><span data-stu-id="178c9-128">The following steps show you how to modify the RemoteMonitoring Visual Studio solution to include a new rule that uses the **ExternalTemperature** telemetry sent from the simulated device:</span></span>

1. <span data-ttu-id="178c9-129">Pokud jste tak již neučinili, klonovat **azure-iot-remote-monitoring** úložiště do vhodného umístění na místním počítači pomocí následujícího příkazu Git:</span><span class="sxs-lookup"><span data-stu-id="178c9-129">If you have not already done so, clone the **azure-iot-remote-monitoring** repository to a suitable location on your local machine using the following Git command:</span></span>

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. <span data-ttu-id="178c9-130">V sadě Visual Studio otevřete soubor RemoteMonitoring.sln z vaší místní kopii **azure-iot-remote-monitoring** úložiště.</span><span class="sxs-lookup"><span data-stu-id="178c9-130">In Visual Studio, open the RemoteMonitoring.sln file from your local copy of the **azure-iot-remote-monitoring** repository.</span></span>

3. <span data-ttu-id="178c9-131">Otevřete soubor Infrastructure\Models\DeviceRuleBlobEntity.cs a přidejte **ExternalTemperature** vlastnost následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="178c9-131">Open the file Infrastructure\Models\DeviceRuleBlobEntity.cs and add an **ExternalTemperature** property as follows:</span></span>

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. <span data-ttu-id="178c9-132">Ve stejném souboru, přidejte **ExternalTemperatureRuleOutput** vlastnost následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="178c9-132">In the same file, add an **ExternalTemperatureRuleOutput** property as follows:</span></span>

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. <span data-ttu-id="178c9-133">Otevřete soubor Infrastructure\Models\DeviceRuleDataFields.cs a přidejte následující **ExternalTemperature** vlastnost po existující **vlhkosti** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="178c9-133">Open the file Infrastructure\Models\DeviceRuleDataFields.cs and add the following **ExternalTemperature** property after the existing **Humidity** property:</span></span>

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. <span data-ttu-id="178c9-134">Ve stejném souboru, aktualizovat **_availableDataFields** tak, aby zahrnoval **ExternalTemperature** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="178c9-134">In the same file, update the **_availableDataFields** method to include **ExternalTemperature** as follows:</span></span>

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. <span data-ttu-id="178c9-135">Otevřete soubor Infrastructure\Repository\DeviceRulesRepository.cs a upravit **BuildBlobEntityListFromTableRows** metoda následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="178c9-135">Open the file Infrastructure\Repository\DeviceRulesRepository.cs and modify the **BuildBlobEntityListFromTableRows** method as follows:</span></span>

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

## <a name="rebuild-and-redeploy-the-solution"></a><span data-ttu-id="178c9-136">Znovu sestavili a nasadili řešení.</span><span class="sxs-lookup"><span data-stu-id="178c9-136">Rebuild and redeploy the solution.</span></span>

<span data-ttu-id="178c9-137">Teď můžete nasadit aktualizované řešení k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="178c9-137">You can now deploy the updated solution to your Azure subscription.</span></span>

1. <span data-ttu-id="178c9-138">Otevřete příkazový řádek se zvýšenými oprávněními a přejděte do kořenového adresáře místní kopii úložiště azure-iot-remote-monitoring.</span><span class="sxs-lookup"><span data-stu-id="178c9-138">Open an elevated command prompt and navigate to the root of your local copy of the azure-iot-remote-monitoring repository.</span></span>

2. <span data-ttu-id="178c9-139">K nasazení řešení aktualizované, spusťte následující příkaz nahraďte **{název nasazení}** s názvem vašeho nasazení předkonfigurované řešení, které jste si poznamenali dříve:</span><span class="sxs-lookup"><span data-stu-id="178c9-139">To deploy your updated solution, run the following command substituting **{deployment name}** with the name of your preconfigured solution deployment that you noted previously:</span></span>

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a><span data-ttu-id="178c9-140">Aktualizace úlohy služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="178c9-140">Update the Stream Analytics job</span></span>

<span data-ttu-id="178c9-141">Po dokončení nasazení můžete aktualizovat úlohu služby Stream Analytics používat nové definice pravidla.</span><span class="sxs-lookup"><span data-stu-id="178c9-141">When the deployment is complete, you can update the Stream Analytics job to use the new rule definitions.</span></span>

1. <span data-ttu-id="178c9-142">Na portálu Azure přejděte do skupiny prostředků, která obsahuje vaše prostředky předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="178c9-142">In the Azure portal, navigate to the resource group that contains your preconfigured solution resources.</span></span> <span data-ttu-id="178c9-143">Tato skupina prostředků má stejný název, který jste zadali pro řešení během nasazení.</span><span class="sxs-lookup"><span data-stu-id="178c9-143">This resource group has the same name you specified for the solution during the deployment.</span></span>

2. <span data-ttu-id="178c9-144">Přejděte na název nasazení {} – úloha Stream Analytics pravidla.</span><span class="sxs-lookup"><span data-stu-id="178c9-144">Navigate to the {deployment name}-Rules Stream Analytics job.</span></span> 

3. <span data-ttu-id="178c9-145">Klikněte na tlačítko **Zastavit** zastavit úlohu služby Stream Analytics spuštění.</span><span class="sxs-lookup"><span data-stu-id="178c9-145">Click **Stop** to stop the Stream Analytics job from running.</span></span> <span data-ttu-id="178c9-146">(Je nutné počkat úlohu streamování zastavit před úpravou dotazu).</span><span class="sxs-lookup"><span data-stu-id="178c9-146">(You must wait for the streaming job to stop before you can edit the query).</span></span>

4. <span data-ttu-id="178c9-147">Klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="178c9-147">Click **Query**.</span></span> <span data-ttu-id="178c9-148">Upravit dotaz tak, aby zahrnují **vyberte** příkaz pro **ExternalTemperature**.</span><span class="sxs-lookup"><span data-stu-id="178c9-148">Edit the query to include the **SELECT** statement for **ExternalTemperature**.</span></span> <span data-ttu-id="178c9-149">Následující příklad ukazuje dokončení dotazu s novým **vyberte** příkaz:</span><span class="sxs-lookup"><span data-stu-id="178c9-149">The following sample shows the complete query with the new **SELECT** statement:</span></span>

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

5. <span data-ttu-id="178c9-150">Klikněte na tlačítko **Uložit** změnit aktualizované pravidlo dotazu.</span><span class="sxs-lookup"><span data-stu-id="178c9-150">Click **Save** to change the updated rule query.</span></span>

6. <span data-ttu-id="178c9-151">Klikněte na tlačítko **spustit** při spuštění úlohy Stream Analytics znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="178c9-151">Click **Start** to start the Stream Analytics job running again.</span></span>

## <a name="add-your-new-rule-in-the-dashboard"></a><span data-ttu-id="178c9-152">Přidat nové pravidlo v řídicím panelu</span><span class="sxs-lookup"><span data-stu-id="178c9-152">Add your new rule in the dashboard</span></span>

<span data-ttu-id="178c9-153">Nyní můžete přidat **ExternalTemperature** pravidlo pro zařízení na řídicím panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="178c9-153">You can now add the **ExternalTemperature** rule to a device in the solution dashboard.</span></span>

1. <span data-ttu-id="178c9-154">Přejděte na portálu řešení.</span><span class="sxs-lookup"><span data-stu-id="178c9-154">Navigate to the solution portal.</span></span>

2. <span data-ttu-id="178c9-155">Přejděte na **zařízení** panelu.</span><span class="sxs-lookup"><span data-stu-id="178c9-155">Navigate to the **Devices** panel.</span></span>

3. <span data-ttu-id="178c9-156">Vyhledejte jste vytvořili vlastní zařízení, které odesílá **ExternalTemperature** telemetrie a na **podrobnosti o zařízení** panelu, klikněte na tlačítko **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="178c9-156">Locate the custom device you created that sends **ExternalTemperature** telemetry and on the **Device Details** panel, click **Add Rule**.</span></span>

4. <span data-ttu-id="178c9-157">Vyberte **ExternalTemperature** v **datové pole**.</span><span class="sxs-lookup"><span data-stu-id="178c9-157">Select **ExternalTemperature** in **Data Field**.</span></span>

5. <span data-ttu-id="178c9-158">Nastavit **prahová hodnota** k 56.</span><span class="sxs-lookup"><span data-stu-id="178c9-158">Set **Threshold** to 56.</span></span> <span data-ttu-id="178c9-159">Pak klikněte na tlačítko **uložit a zobrazit pravidla**.</span><span class="sxs-lookup"><span data-stu-id="178c9-159">Then click **Save and view rules**.</span></span>

6. <span data-ttu-id="178c9-160">Vrátit na řídicí panel k zobrazení historie alarmů.</span><span class="sxs-lookup"><span data-stu-id="178c9-160">Return to the dashboard to view the alarm history.</span></span>

7. <span data-ttu-id="178c9-161">V okně konzoly můžete ponechány otevřené, spusťte konzolovou aplikaci softwaru Node.js, aby se začala odesílat **ExternalTemperature** telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="178c9-161">In the console window you left open, start the Node.js console app to begin sending **ExternalTemperature** telemetry data.</span></span>

8. <span data-ttu-id="178c9-162">Všimněte si, že **historie alarmů** tabulka ukazuje nové výstrahy, když se aktivuje nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="178c9-162">Notice that the **Alarm History** table shows new alarms when the new rule is triggered.</span></span>
 
## <a name="additional-information"></a><span data-ttu-id="178c9-163">Další informace</span><span class="sxs-lookup"><span data-stu-id="178c9-163">Additional information</span></span>

<span data-ttu-id="178c9-164">Změna operátor  **>**  je složitější a překročí podle kroků uvedených v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="178c9-164">Changing the operator **>** is more complex and goes beyond the steps outlined in this tutorial.</span></span> <span data-ttu-id="178c9-165">Zatímco můžete změnit na jakémkoli operátor chcete úlohy Stream Analytics, odrážející tento operátor na portálu řešení je složitější úlohy.</span><span class="sxs-lookup"><span data-stu-id="178c9-165">While you can change the Stream Analytics job to use whatever operator you like, reflecting that operator in the solution portal is a more complex task.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="178c9-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="178c9-166">Next steps</span></span>
<span data-ttu-id="178c9-167">Teď, když už víte, jak vytvořit vlastní pravidla, můžete další informace o předkonfigurovaných řešení:</span><span class="sxs-lookup"><span data-stu-id="178c9-167">Now that you've seen how to create custom rules, you can learn more about the preconfigured solutions:</span></span>

- <span data-ttu-id="178c9-168">[Připojení aplikace logiky k řešení Azure IoT Suite vzdálené monitorování předkonfigurované][lnk-logic-app]</span><span class="sxs-lookup"><span data-stu-id="178c9-168">[Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logic-app]</span></span>
- <span data-ttu-id="178c9-169">[Informace metadat zařízení ve vzdálené monitorování předkonfigurované řešení][lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="178c9-169">[Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md