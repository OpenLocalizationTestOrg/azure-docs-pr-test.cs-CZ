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
# <a name="create-a-custom-rule-in-hello-remote-monitoring-preconfigured-solution"></a>Vytvoření vlastního pravidla v hello předkonfigurovanému řešení vzdáleného monitorování

## <a name="introduction"></a>Úvod

V případě hello předkonfigurované řešení, můžete nakonfigurovat [pravidla, která spouští při telemetrie hodnota zařízení dosáhne určité prahové hodnoty][lnk-builtin-rule]. [Použití dynamické telemetrie s hello předkonfigurovanému řešení vzdáleného monitorování] [ lnk-dynamic-telemetry] popisuje, jak můžete přidat vlastní telemetrii hodnoty, jako například *ExternalTemperature* tooyour řešení. Tento článek ukazuje, jak toocreate vlastní pravidlo pro dynamické telemetrie typy ve vašem řešení.

Tento kurz používá jednoduchý Node.js simulované zařízení toogenerate dynamické telemetrie toosend toohello předkonfigurované řešení back-end. Pak přidejte vlastní pravidla ve hello **RemoteMonitoring** řešení sady Visual Studio a nasadit tento vlastní back-end tooyour předplatného Azure.

toocomplete tohoto kurzu potřebujete:

* Aktivní předplatné Azure. Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk_free_trial].
* [Node.js] [ lnk-node] verze 0.12.x nebo novější toocreate simulované zařízení.
* Visual Studio 2015 nebo Visual Studio 2017 toomodify hello předkonfigurované back-end řešení s nové pravidel.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

Poznamenejte si název hello řešení, které jste zvolili pro vaše nasazení. Je třeba tento název řešení později v tomto kurzu.

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

Po ověření, že posílá můžete zastavit konzolovou aplikaci softwaru Node.js hello **ExternalTemperature** telemetrie toohello předkonfigurované řešení. Vzhledem k tomu, že spustíte tuto konzolovou aplikaci softwaru Node.js znovu po přidání hello vlastní pravidlo toohello řešení, nechte okno konzoly hello otevřené.

## <a name="rule-storage-locations"></a>Pravidlo umístění úložiště

Informace o pravidlech je uchován v dvou umístěních:

* **DeviceRulesNormalizedTable** tabulce – v této tabulce jsou uloženy normalizovaný odkazovat toohello pravidla definované portál řešení hello. Když hello portál řešení zobrazí pravidla zařízení, vyžádá si tuto tabulku hello Definice pravidla.
* **DeviceRules** objektů blob – tento objekt blob ukládá všechna pravidla hello definovaná pro všechny registrované zařízení a je definován jako úlohy Azure Stream Analytics vstupní toohello odkaz.
 
Při aktualizaci existující pravidlo nebo definovat nové pravidlo hello řešení portálu, hello tabulky a objektů blob jsou aktualizované tooreflect hello změny. pravidlo Hello definice zobrazení na portálu hello pocházejí z tabulky úložiště hello a hello pravidlo, že definice odkazuje úlohy Stream Analytics hello pochází z objektu blob hello. 

## <a name="update-hello-remotemonitoring-visual-studio-solution"></a>Aktualizovat řešení sady Visual Studio RemoteMonitoring hello

Hello následující kroky vám ukážou, jak toomodify hello tooinclude řešení sady Visual Studio RemoteMonitoring nové pravidlo, které používá hello **ExternalTemperature** telemetrická data odesílaná ze simulovaného zařízení hello:

1. Pokud jste tak již neučinili, klonování hello **azure-iot-remote-monitoring** úložiště tooa vhodný umístění na místním počítači pomocí následujícího příkazu Git hello:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. V sadě Visual Studio otevřete soubor RemoteMonitoring.sln hello z vaší místní kopii hello **azure-iot-remote-monitoring** úložiště.

3. Otevřete soubor hello Infrastructure\Models\DeviceRuleBlobEntity.cs a přidejte **ExternalTemperature** vlastnost následujícím způsobem:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. V hello stejný soubor, přidejte **ExternalTemperatureRuleOutput** vlastnost následujícím způsobem:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Otevřete soubor hello Infrastructure\Models\DeviceRuleDataFields.cs a přidejte následující hello **ExternalTemperature** vlastnost po hello existující **vlhkosti** vlastnost:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. V hello stejného souboru, aktualizujte hello **_availableDataFields** metoda tooinclude **ExternalTemperature** následujícím způsobem:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Otevřete soubor hello Infrastructure\Repository\DeviceRulesRepository.cs a upravit hello **BuildBlobEntityListFromTableRows** metoda následujícím způsobem:

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

## <a name="rebuild-and-redeploy-hello-solution"></a>Znovu sestavili a nasadili hello řešení.

Teď můžete nasadit řešení tooyour hello aktualizovat předplatné Azure.

1. Otevřete příkazový řádek se zvýšenými oprávněními a přejděte toohello kořenovém místní kopii úložiště azure-iot-remote-monitoring hello.

2. toodeploy řešení aktualizované, spusťte následující příkaz nahraďte hello **{název nasazení}** s názvem hello vašeho nasazení předkonfigurované řešení, které jste si poznamenali dříve:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-hello-stream-analytics-job"></a>Úloha Stream Analytics hello aktualizace

Po dokončení nasazení hello můžete aktualizovat hello Stream Analytics toouse hello nové pravidel definice úlohy.

1. V hello portálu Azure přejděte toohello skupinu prostředků, která obsahuje vaše prostředky předkonfigurované řešení. Tato skupina prostředků má stejný název, kterou jste zadali pro hello hello řešení během nasazení hello.

2. Přejděte toohello {název nasazení}-úlohy Stream Analytics pravidla. 

3. Klikněte na tlačítko **Zastavit** úlohy služby Stream Analytics hello toostop spuštění. (Je nutné počkat hello streamování úlohy toostop před úpravou dotazu hello).

4. Klikněte na tlačítko **dotazu**. Upravit hello dotazu tooinclude hello **vyberte** příkaz pro **ExternalTemperature**. Hello následující příklad ukazuje hello dokončení dotazu s hello nové **vyberte** příkaz:

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

5. Klikněte na tlačítko **Uložit** toochange hello aktualizovat pravidlo dotazu.

6. Klikněte na tlačítko **spustit** toostart hello Stream Analytics úlohu znovu spustit.

## <a name="add-your-new-rule-in-hello-dashboard"></a>Přidat nové pravidlo v řídicím panelu hello

Nyní můžete přidat hello **ExternalTemperature** pravidlo tooa zařízení na řídicím panelu řešení hello.

1. Přejděte toohello portál řešení.

2. Přejděte toohello **zařízení** panelu.

3. Vyhledejte vlastní zařízení hello jste vytvořili, který odesílá **ExternalTemperature** telemetrie a na hello **podrobnosti o zařízení** panelu, klikněte na tlačítko **přidat pravidlo**.

4. Vyberte **ExternalTemperature** v **datové pole**.

5. Nastavit **prahová hodnota** too56. Pak klikněte na tlačítko **uložit a zobrazit pravidla**.

6. Vrátí historie alarmů toohello řídicí panel tooview hello.

7. V okně konzoly hello je ponechány otevřené spustit odesílání hello Node.js konzole aplikace toobegin **ExternalTemperature** telemetrická data.

8. Všimněte si, že hello **historie alarmů** tabulka ukazuje nové výstrahy, když se aktivuje hello nové pravidlo.
 
## <a name="additional-information"></a>Další informace

Změna hello operátor  **>**  je složitější a překročí hello kroků uvedených v tomto kurzu. Při toouse úlohy Stream Analytics hello můžete změnit ať operátor se vám líbí, odrážející tento operátor portálu řešení hello je složitější úlohy. 

## <a name="next-steps"></a>Další kroky
Teď, když už víte, jak toocreate vlastní pravidla, získáte další informace o hello předkonfigurované řešení:

- [Připojení aplikace logiky tooyour Azure IoT Suite vzdálené monitorování předkonfigurované řešení][lnk-logic-app]
- [Informace metadat zařízení v hello vzdálené monitorování předkonfigurované řešení][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md