---
title: "Diagnostika v trvanlivý funkce – Azure"
description: "Zjistěte, jak diagnostikovat problémy s příponou trvanlivý funkce pro Azure Functions."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 5ebab8660dfe21984e1a7f9a1cb925aea60de213
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="diagnostics-in-durable-functions-azure-functions"></a>Diagnostika trvanlivý funkcí (Azure Functions)

Existuje několik možností pro diagnostiku problémů s [trvanlivý funkce](durable-functions-overview.md). Některé z těchto možností jsou stejné pro běžné funkce a některé z nich jsou jedinečné pro odolná funkce.

## <a name="application-insights"></a>Application Insights

[Application Insights](../application-insights/app-insights-overview.md) je doporučeným způsobem, jak se diagnostika a sledování v Azure Functions. Totéž platí i pro odolná funkce. Přehled o tom, jak využít Application Insights ve vaší aplikaci funkce najdete v tématu [monitorování Azure Functions](functions-monitoring.md).

Trvanlivý rozšíření funkce Azure také vysílá *sledování událostí* které umožňují sledovat spuštění začátku do konce orchestration. Tyto naleznete a dotazovat pomocí [Application Insights Analytics](../application-insights/app-insights-analytics.md) nástroj na webu Azure portal.

### <a name="tracking-data"></a>Sledování dat

Všechny události životního cyklu instance orchestration způsobí vygenerování události sledování k zápisu do **trasování** kolekce ve službě Application Insights. Tato událost obsahuje **customDimensions** datovou část s několika poli.  Názvy polí jsou všechny přidá jako předpona s `prop__`.

* **hubName**: název rozbočovače úloh, ve kterém běží váš orchestrations.
* **appName**: název aplikace funkce. To je užitečné, pokud máte víc aplikací funkce sdílení na stejnou instanci Application Insights.
* **slotName**: [nasazovací slot](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/) v aktuální aplikaci funkce běží. To je užitečné, pokud využíváte nasazovací sloty na verzi vašeho orchestrations.
* **%{FunctionName/**: název funkce orchestrator nebo aktivity.
* **functiontype –**: typ funkce, jako například **Orchestrator** nebo **aktivity**.
* **identifikátor instanceId**: jedinečné ID instance orchestration.
* **Stav**: stav spuštění cyklu instance. Platné hodnoty patří:
    * **Naplánované**: funkce byla naplánována na spuštění, ale nebyla spuštěna, ještě spuštěna.
    * **Spuštění**: funkce byl spuštěn, ale nebylo dosud očekávaná nebo byla dokončena.
    * **Očekávaná**: orchestrator má naplánované některé pracovní a čeká na její dokončení.
    * **Naslouchání**: orchestrator naslouchá pro oznámení o externí události.
    * **Dokončit**: funkce byla úspěšně dokončena.
    * **Se nezdařilo**: funkce došlo k chybě.
* **důvod**: další data přidružená k události sledování. Například pokud instance čeká na oznámení o události externí, toto pole označuje název události, kterou se čeká na. Funkce se nezdařila, bude obsahovat podrobnosti o chybě.
* **isReplay**: přehrány logickou hodnotu udávající, zda je sledování událostí pro spuštění.
* **extensionVersion**: verzi rozšíření trvanlivý úloh. To je zvláště důležitá data při zasílání zpráv o chybách možné v rozšíření. Dlouho běžící instance může hlásit několik verzí, pokud dojde k aktualizaci, když je spuštěná. 

Podrobností sledování dat do služby Application Insights vygenerované se dá nakonfigurovat v `logger` části `host.json` souboru.

```json
{
    "logger": {
        "categoryFilter": {
            "categoryLevels": {
                "Host.Triggers.DurableTask": "Information"
            }
        }
    }
}
```

Ve výchozím nastavení jsou všechny sledování události vygenerované. Objem dat může snížit nastavení `Host.Triggers.DurableTask` k `"Warning"` nebo `"Error"` v takovém případě sledování událostí bude pouze být vygenerované pro výjimečné situace.

> [!WARNING]
> Ve výchozím nastavení je telemetrie Application Insights vzorkovat modulu runtime Azure Functions tak, aby se zabránilo generování dat příliš často. To může způsobit informace o sledování být ztraceny, pokud v krátké době dojde k mnoha události životního cyklu. [Monitorování funkce Azure článku](functions-monitoring.md#configure-sampling) vysvětluje, jak pro konfiguraci tohoto chování.

### <a name="single-instance-query"></a>Jedna instance dotazu

Následující dotaz zobrazí sledování historických dat pro jednu instanci [Hello pořadí](durable-functions-sequence.md) funkce orchestration. Zapsané pomocí [Application Insights dotazu jazyka (AIQL)](https://docs.loganalytics.io/docs/Language-Reference). Odfiltruje opětovného přehrání provádění tak pouze *logické* provádění cesta se zobrazí.

```AIQL
let targetInstanceId = "bf71335b26564016a93860491aa50c7f";
let start = datetime(2017-09-29T00:00:00);
traces
| where timestamp > start and timestamp < start + 30m
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = customDimensions["prop__functionName"]
| extend instanceId = customDimensions["prop__instanceId"]
| extend state = customDimensions["prop__state"]
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| where isReplay == false
| where instanceId == targetInstanceId
| project timestamp, functionName, state, instanceId, appName = cloud_RoleName
```
Výsledkem je seznam sledování události, které se zobrazí cesta provádění orchestration, včetně funkcí, všechny aktivity.

![Application Insights dotazu](media/durable-functions-diagnostics/app-insights-single-instance-query.png)

> [!NOTE]
> Některé z těchto sledování událostí může být mimo pořadí z důvodu nedostatku přesnost v `timestamp` sloupce. To je sledována v Githubu jako [vydání #71](https://github.com/Azure/azure-functions-durable-extension/issues/71).

### <a name="instance-summary-query"></a>Souhrn dotazu instance

Následující dotaz zobrazí stav všech instancí orchestration, které byly spuštěny v zadaném časovém rozmezí.

```AIQL
let start = datetime(2017-09-30T04:30:00);
traces
| where timestamp > start and timestamp < start + 1h
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = tostring(customDimensions["prop__functionName"])
| extend instanceId = tostring(customDimensions["prop__instanceId"])
| extend state = tostring(customDimensions["prop__state"])
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend output = tostring(customDimensions["prop__output"])
| where isReplay == false
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```
Výsledkem je seznam instance ID a jejich aktuální stav modulu runtime.

![Application Insights dotazu](media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Protokolování

Je důležité mějte orchestrator opětovného přehrání chování při zápisu protokoly přímo z funkce produktu orchestrator. Zvažte například následující funkce orchestrator:

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```

Výsledná data protokolu bude vypadat podobně jako následující:

```txt
Calling F1.
Calling F1.
Calling F2.
Calling F1.
Calling F2.
Calling F3.
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> Mějte na paměti, že když chcete-li být volání F1, F2 a F3 deklarací identity v protokolech, tyto funkce jsou pouze *ve skutečnosti* volána při prvním se vyskytují. Následující volání, které dojít během opětovného přehrání se přeskočí a výstupy jsou odesílal na logice orchestrator.

Pokud budete chtít přihlásit pouze provádění opětovného přehrání, podmíněného výrazu může zapsat do protokolu pouze v případě `IsReplaying` je `false`. Vezměte v úvahu v předchozím příkladu, ale tentokrát s opakování kontroly.

```cs
public static async Task Run(
    DurableOrchestrationContext ctx,
    TraceWriter log)
{
    if (!ctx.IsReplaying) log.Info("Calling F1.");
    await ctx.CallActivityAsync("F1");
    if (!ctx.IsReplaying) log.Info("Calling F2.");
    await ctx.CallActivityAsync("F2");
    if (!ctx.IsReplaying) log.Info("Calling F3");
    await ctx.CallActivityAsync("F3");
    log.Info("Done!");
}
```
Díky této změně protokolu výstup vypadá takto:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

## <a name="debugging"></a>Ladění

Azure Functions podporuje ladění kódu funkce přímo a které podporují stejné představuje dál na trvanlivý funkce, jestli běží v Azure nebo místně. Existuje však několik chování znát při ladění:

* **Opakování**: funkce Orchestrator pravidelně opakování při přijetí nové vstupy. To znamená jedné *logické* provádění orchestrator funkce může způsobit stiskne stejné zarážek vícekrát, zvlášť pokud je nastaveno již v rané fázi v kód funkce.
* **Await**: vždy, když `await` je došlo, dává řízení zpět do dispečera trvanlivý Framework úloh. Pokud je to poprvé a konkrétní `await` byl došlo, přidružené úloha je *nikdy* byl obnoven. Protože úloha nikdy obnoví, krokování *přes* await (F10 v sadě Visual Studio) není ve skutečnosti možné. Krokování s přes funguje pouze, když je právě přehrány úlohu.
* **Časové limity pro zasílání zpráv**: trvanlivý funkce interně používá zprávy fronty disku spouštění orchestrator funkce a funkce aktivity. V prostředí s více virtuálních počítačů rozdělení do ladění pro dlouhou dobu způsobit jiným virtuálním Počítačem na vyzvednutí zpráva, což vede k provádění duplicitní. Toto chování pro aktivační událost regulární fronty funkce také existuje, ale je důležité, tak, aby odkazoval v tomto kontextu, protože fronty jsou podrobností implementace.

> [!TIP]
> Při nastavení zarážek, pokud chcete pouze rozdělit na provedení bez opakování, můžete nastavit podmíněné zarážky tohoto konce pouze v případě `IsReplaying` je `false`.

## <a name="storage"></a>Úložiště

Ve výchozím nastavení ukládá trvanlivý funkce stavu ve službě Azure Storage. To znamená, že si můžete prohlédnout stav vaší orchestrations pomocí nástrojů, jako [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Snímek Azure Storage Explorer obrazovky](media/durable-functions-diagnostics/storage-explorer.png)

To je užitečné pro ladění, protože uvidíte přesně jaké stav orchestration může být v. Zprávy do front můžete také prověřit, se dozvíte, jaké pracovní čeká na vyřízení (nebo zablokuje v některých případech).

> [!WARNING]
> I když je vhodné zobrazit historii spouštění ve službě table storage, neberte některé závislé na této tabulce. Se může změnit při zpracovaní rozšíření trvanlivý funkce.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Další informace o použití trvanlivý časovače](durable-functions-timers.md)