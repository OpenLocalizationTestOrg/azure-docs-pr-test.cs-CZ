---
title: "nastavení protokolování aaaCheck stav a získat upozornění – Azure Logic Apps | Microsoft Docs"
description: "Monitorovat stav a výkon pro aplikace logiky, protokolu diagnostická data a nastavení výstrah"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 81f186e11a669b710f4c06089597eb5a76f7a44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a>Monitorování stavu, nastavit protokolování diagnostiky a zapnutí výstrah pro Azure Logic Apps

Po jste [vytvořte a spusťte aplikaci logiky](../logic-apps/logic-apps-create-a-logic-app.md), můžete zkontrolovat historii spustí, historie aktivační událost, stavu a výkonu. Nastavení pro sledování událostí v reálném čase a bohatší ladění, [protokolování diagnostiky](#azure-diagnostics) pro svou aplikaci logiky. Tímto způsobem můžete [najít a zobrazit události](#find-events), jako jsou aktivační události, spuštění události a události akce. Můžete také použít tuto [diagnostická data s jinými službami](#extend-diagnostic-data), jako jsou služby Azure Storage a Azure Event Hubs. 

nastavit tooget oznámení o selhání nebo jiné možné problémy [výstrahy](#add-azure-alerts). Například můžete vytvořit výstrahu, která zjistí "při více než pět spustí nezdaří za hodinu." Můžete také nastavit monitorování, sledování a protokolování programově pomocí [Azure Diagnostics událostí nastavení a vlastností](#diagnostic-event-properties).

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a>Spustí zobrazení a historii aktivační událost pro svou aplikaci logiky

1. toofind aplikace logiky v hello [portál Azure](https://portal.azure.com), na hello hlavní nabídky Azure, zvolte **další služby**. Hello vyhledávacího pole, vyhledejte "aplikace logiky" a zvolte **Logic apps**.

   ![Najít aplikaci logiky](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   Hello portál Azure obsahuje všechny hello logiku aplikace, které jsou spojeny s předplatným Azure. 

2. Vyberte svou aplikaci logiky, a potom vyberte **přehled**.

   Hello portál Azure zobrazuje hello spustí historie a historie aktivační událost pro svou aplikaci logiky. Například:

   ![Spuštění aplikace logiky historie historie a aktivační události](media/logic-apps-monitor-your-logic-apps/overview.png)

   * **Spustí historie** ukazuje všechny hello spuštění pro svou aplikaci logiky. 
   * **Aktivovat historie** zobrazuje všechny aktivity hello aktivační událost pro svou aplikaci logiky.

   Popis stavu najdete v tématu [řešení aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).

   > [!TIP]
   > Pokud nenajdete hello data, která očekáváte, na panelu nástrojů hello, zvolte **aktualizovat**.

3. tooview hello kroky v části z konkrétní spuštění, **spouští historie**, vyberte, které používají. 

   zobrazení monitorování Hello uvádí každý krok v tom, že spustit. Například:

   ![Akce pro konkrétní spustit](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. Vyberte další podrobnosti o hello spustit, tooget **podrobnosti o spuštění**. Tyto informace shrnuje kroky hello, stav, vstupy a výstupy pro hello spusťte. 

   ![Zvolte "Spustit podrobnosti"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   Například můžete získat hello spustit na **ID korelace**, což může být nutné při použití hello [REST API pro Logic Apps](https://docs.microsoft.com/rest/api/logic).

5. tooget podrobnosti o konkrétní krok, vyberte tento krok. Nyní můžete zkontrolovat podrobnosti, třeba vstupy, výstupy a všechny chyby, které pro tento krok se stalo. Například:

   ![Podrobnosti o krok](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > Všechny podrobnosti runtime a událostí, jsou šifrovány ve hello služba Logic Apps. Budou se dešifrují jenom v případě, že uživatel požádá o tooview tato data. Můžete také ovládat přístup toothese události s [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md).

6. tooget podrobnosti o konkrétní aktivační událost, přejděte zpět toohello **přehled** podokně. V části **aktivovat historie**vyberte hello aktivační událost. Můžete teď zkontrolujte podrobnosti jako vstupy a výstupy, například:

   ![Podrobnosti o aktivační události výstup](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a>Zapněte diagnostiku protokolování pro svou aplikaci logiky

Pro širší ladění s podrobnostmi runtime a události, můžete nastavit protokolování s diagnostiky [Azure Log Analytics](../log-analytics/log-analytics-overview.md). Analýzy protokolů je služba v [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) , sleduje vaše cloudové a místní prostředí toohelp udržování své dostupnosti a výkonu. 

Než začnete, je nutné toohave pracovním prostorem OMS. Další informace [jak toocreate pracovním prostorem OMS](../log-analytics/log-analytics-get-started.md).

1. V hello [portál Azure](https://portal.azure.com), najděte a vyberte svou aplikaci logiky. 

2. V nabídce okno aplikace logiky hello pod **monitorování**, zvolte **diagnostiky** > **nastavení pro diagnostiku**.

   ![Přejděte na nastavení pro diagnostiku tooMonitoring, diagnostiky,](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. V části **nastavení diagnostiky**, zvolte **na**.

   ![Zapnout diagnostické protokoly](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. Nyní vyberte kategorii pracovní prostor a událostí OMS hello pro protokolování, jak je znázorněno:

   1. Vyberte **odeslat tooLog Analytics**. 
   2. V části **analýzy protokolů**, zvolte **konfigurace**. 
   3. V části **pracovních prostorů OMS**, vyberte hello OMS prostoru toouse pro protokolování.
   4. V části **protokolu**, vyberte hello **modul runtime pracovního postupu** kategorie.
   5. Zvolte metriky interval hello.
   6. Až budete hotoví, zvolte **Uložit**.

   ![Vyberte pracovní prostor OMS a dat pro protokolování](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

Teď můžete najít události a dalších dat pro aktivační události, spustit události a události akce.

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a>Najít události a data pro svou aplikaci logiky

toofind a zobrazení událostí v aplikaci logiky, jako jsou aktivační události, spustit události a události akce, postupujte podle těchto kroků.

1. V hello [portál Azure](https://portal.azure.com), zvolte **více služeb**. Vyhledejte "analýzy protokolů" a pak zvolte **analýzy protokolů** jak je vidět tady:

   ![Zvolte "Analýzy protokolů"](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS. 

   ![Vyberte pracovní prostor OMS](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. V části **správy**, zvolte **portálu OMS**.

   ![Zvolte "Portál OMS"](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. Na domovské stránce OMS, zvolte **hledání protokolů**.

   ![Na domovské stránce OMS zvolte "Hledání protokolů"](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   -nebo-

   ![V nabídce OMS hello zvolte "Hledání protokolů"](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. Hello vyhledávacího pole zadejte pole, které chcete toofind a stiskněte klávesu **Enter**. Když začnete psát, ukazuje OMS je možné shody a operací, které můžete použít. 

   Například toofind hello prvních 10 události, které se stalo, zadejte a vyberte tento vyhledávací dotaz: **kategorie = modul runtime pracovního postupu | top 10**

   ![Zadejte hledaný řetězec](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   Další informace o [jak toofind data v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).

6. Na stránce výsledky hello hello levém panelu, zvolte hello časový rámec, které chcete tooview.
Zvolte dotaz přidáním filtru, toorefine **+ přidat**.

   ![Vyberte časový rámec pro výsledky dotazu](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. V části **přidat filtry**, zadejte název filtru hello hello filtru chcete vyhledávat. Vyberte filtr hello a zvolte **+ přidat**.

   Tento příklad používá hello toofind "status" word se nezdařilo události **AzureDiagnostics**.
   Zde hello filtr **status_s** je již vybrána.

   ![Vyberte filtr](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. V levém panelu hello vyberte hello filtru hodnotu, kterou chcete toouse a zvolte **použít**.

   ![Vyberte hodnotu filtru, zvolte "Použít"](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. Nyní se vraťte toohello dotaz, který vytváříte. Dotaz se aktualizuje se vybraný filtr a hodnotou. Předchozí výsledky jsou nyní příliš filtrovány.

   ![Vrátí tooyour dotaz s filtrované výsledky](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. Zvolte dotaz pro budoucí použití, toosave **Uložit**. Další informace [jak toosave dotazu](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Rozšíření jak a kde používáte diagnostických dat s jinými službami

Společně s Azure Log Analytics můžete rozšířit použití aplikace logiky diagnostických dat s jinými službami Azure, například: 

* [Archiv, který Azure Diagnostics protokolů v úložišti Azure](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [TooAzure datový proud protokolů diagnostiky Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Můžete pak monitorování pomocí telemetrie a analýzy z jiných služeb, jako například get v reálném čase [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) a [Power BI](../log-analytics/log-analytics-powerbi.md). Například:

* [Datový proud dat ze služby Event Hubs tooStream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Analýza dat pomocí služby Stream Analytics a vytvořit řídicí panel analýzu v reálném čase v Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Podle hello možnosti, které chcete nastavit, ujistěte se, že jste první [vytvořit účet úložiště Azure](../storage/common/storage-create-storage-account.md) nebo [vytváření centra událostí Azure](../event-hubs/event-hubs-create.md). Potom vyberte možnosti hello místo toosend diagnostických dat:

![Odeslání dat tooAzure úložiště účet nebo události rozbočovače](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> Období uchovávání použít pouze v případě, že zvolíte toouse účet úložiště.

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a>Nastavit výstrahy pro svou aplikaci logiky

konkrétní metriky toomonitor nebo překročil prahové hodnoty pro svou aplikaci logiky nastavit [výstrahy v Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md). Další informace o [metriky v Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md). 

tooset si výstrahy bez [Azure Log Analytics](../log-analytics/log-analytics-overview.md), postupujte podle těchto kroků. Pro pokročilejší kritéria výstrahy a akce [nastavení analýzy protokolů](#azure-diagnostics) příliš.

1. V nabídce okno aplikace logiky hello pod **monitorování**, zvolte **diagnostiky** > **výstrah pravidla** > **přidat upozornění**jak je vidět tady:

   ![Přidání oznámení pro svou aplikaci logiky](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. Na hello **přidání pravidla výstrahy** okně vytvořit upozornění, jak je znázorněno:

   1. V části **prostředků**, vyberte svou aplikaci logiky, pokud není vybrána. 
   2. Zadejte název a popis pro upozornění.
   3. Vyberte **metrika** nebo událostí, které chcete tootrack.
   4. Vyberte **podmínku**, zadejte **prahová hodnota** pro metriku hello a vyberte hello **období** pro monitorování tato metrika.
   5. Vyberte, zda toosend poštovní hello výstrahy. 
   6. Zadejte jiné e-mailové adresy pro odesílání upozornění hello. 
   Také můžete zadat adresu URL webhooku místo toosend hello výstrahy.

   Například toto pravidlo, odešle výstrahu, pokud pět nebo další spustí selhání za hodinu:

   ![Vytvoření metriky pravidlo výstrahy](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> toorun aplikace logiky z výstrahy, můžete zahrnout hello [aktivační událost požadavku](../connectors/connectors-native-reqres.md) v pracovním postupu, která vám umožňuje provádět úlohy, jako tyto příklady:
> 
> * [POST tooSlack](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Odeslat jako text](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Přidat tooa fronty zpráv](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a>Nastavení Azure Diagnostics událostí a podrobnosti

Každý diagnostických událostí obsahuje podrobnosti o aplikaci logiky a tuto událost, například hello stav, čas spuštění, koncový čas a tak dále. tooprogrammatically nastavení monitorování, sledování a protokolování, organizační jednotky můžete použít tyto podrobnosti s hello [REST API pro Azure Logic Apps](https://docs.microsoft.com/rest/api/logic) a hello [REST API pro Azure Diagnostics](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).

Například hello `ActionCompleted` událost má hello `clientTrackingId` a `trackedProperties` vlastnosti, které můžete použít pro sledování a monitorování:

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* `clientTrackingId`: Pokud není zadáno, Azure automaticky vygeneruje ID a korelaci událostí napříč spustit aplikace logiky, včetně všech vnořených pracovních postupů, se nazývají z aplikace logiky hello. Můžete ručně zadat toto ID z aktivační událost pomocí předání `x-ms-client-tracking-id` hlavička s vlastní hodnota ID v požadavek aktivace hello. Můžete použít aktivační událost požadavku, triggeru protokolu HTTP nebo aktivační události webhooku.

* `trackedProperties`: tootrack vstupů nebo výstupů v diagnostická data, můžete přidat tooactions sledovaných vlastnosti v definici aplikace logiky JSON. Sledované vlastnosti můžete sledovat jenom jednu akci vstupy a výstupy, ale můžete použít hello `correlation` vlastnosti události toocorrelate napříč akce v spustit.

  tootrack jednu nebo více vlastností, přidejte hello `trackedProperties` části a hello vlastnosti, které chcete, aby definice toohello akce. Předpokládejme například, že chcete data tootrack jako "ID pořadí" v telemetrie:

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a>Další kroky

* [Vytvoření šablony pro nasazení aplikace logiky a správa verzí](../logic-apps/logic-apps-create-deploy-template.md)
* [Scénáře B2B s Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Monitorování zpráv B2B](../logic-apps/logic-apps-monitor-b2b-message.md)