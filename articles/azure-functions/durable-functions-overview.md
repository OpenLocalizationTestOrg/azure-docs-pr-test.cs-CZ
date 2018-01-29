---
title: "Trvanlivý Přehled funkce – Azure (preview)"
description: "Úvod do rozšíření trvanlivý funkce pro Azure Functions."
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
ms.openlocfilehash: f1def2a43edee58bc8b5a33880e206130a1b4687
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="durable-functions-overview-preview"></a>Trvanlivý přehled funkcí (preview)

*Trvanlivý funkce* je rozšířením [Azure Functions](functions-overview.md) a [Azure WebJobs](../app-service/web-sites-create-web-jobs.md) který umožňuje zapisovat stavová funkce v prostředí bez serveru. Rozšíření spravuje stav, kontrolní body a restartování za vás.

Rozšíření umožňuje definovat stavová pracovních postupů v nový typ volaná funkce *orchestrator funkce*. Tady jsou některé z výhod funkce orchestrator:

* Pracovní postupy definují v kódu. Žádná schémata JSON nebo návrháři, je potřeba.
* Další funkce můžou volat synchronně a asynchronně. Výstup z vyvolání funkce lze uložit na místní proměnné.
* Budou automaticky kontrolní bod jejich probíhá vždy, když funkce čeká. Místní stavu dojde ke ztrátě nikdy, pokud proces recykluje nebo virtuální počítač se restartuje.

> [!NOTE]
> Trvanlivý funkce je ve verzi preview a je rozšíření rozšířené pro funkce Azure, která není vhodná pro všechny aplikace. Zbývající část tohoto článku předpokládá, že máte silné znalost [Azure Functions](functions-overview.md) koncepty a problémů součástí vývoj aplikací bez serveru.

Případem primárního použití pro funkce trvanlivý je zjednodušení spolupráce komplexní, stavová problémy s aplikacemi bez serveru. Následující části popisují některé vzory Typická aplikace, které můžete využít trvanlivý funkce.

## <a name="pattern-1-function-chaining"></a>Vzor #1: Funkce řetězení

*Funkce řetězení* odkazuje na vzor provádění pořadí funkcí v určitém pořadí. Výstup jednu funkci často potřebuje má být použita pro vstup jinou funkci.

![Diagram řetězení – funkce](media/durable-functions-overview/function-chaining.png)

Trvanlivý funkce umožňuje implementovat tento vzor výstižně v kódu.

```cs
public static async Task<object> Run(DurableOrchestrationContext ctx)
{
    try
    {
        var x = await ctx.CallActivityAsync<object>("F1");
        var y = await ctx.CallActivityAsync<object>("F2", x);
        var z = await ctx.CallActivityAsync<object>("F3", y);
        return  await ctx.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // error handling/compensation goes here
    }
}
```

Hodnoty "F1", "F2", "F3" a "F4" jsou názvy jiných funkcí v aplikaci funkce. Tok řízení je implementovaná pomocí normální imperativní kódování konstrukce. To znamená kód provede shora dolů a může zahrnovat existující sémantiku toku řízení jazyk, jako jsou podmínky a smyčky.  Chyba při zpracování logiky můžou být součástí try/catch/finally – bloky.

`ctx` Parametr ([DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)) poskytuje metody pro vyvolání jiných funkcí podle názvu, předávání parametrů a vrátí výstup funkce. Pokaždé, když kód zavolá metodu `await`, rozhraní trvanlivý funkce *kontrolní body* průběh aktuální instance funkce. Pokud proces nebo virtuální počítač recykluje v polovině prostřednictvím provádění, instance funkce obnoví z předchozí `await` volání. Další informace o toto chování restartovat později.

## <a name="pattern-2-fan-outfan-in"></a>Vzor #2: Fan-odesílacího/fan-v

*FAN odesílacího/fan v* odkazuje na vzor provádění více funkcí paralelně a pak čeká na dokončení všech.  Některé pracovní agregace se často provádí na výsledky vrácené funkcí.

![Diagram FAN odesílacího/fan v](media/durable-functions-overview/fan-out-fan-in.png)

S normální funkce ventilační lze provést tak, že funkce Odeslat více zpráv do fronty. Zpět v ventilační je však mnohem víc náročné. Nutné napsat kód pro můžete sledovat, kdy funkce aktivované protokolem fronty ukončení a uložení výstupy funkce. Trvanlivý funkce rozšíření zpracovává tento vzor kódem poměrně jednoduché.

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    var parallelTasks = new List<Task<int>>();
 
    // get a list of N work items to process in parallel
    object[] workBatch = await ctx.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = ctx.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }
 
    await Task.WhenAll(parallelTasks);
 
    // aggregate all N outputs and send result to F3
    int sum = parallelTasks.Sum(t => t.Result);
    await ctx.CallActivityAsync("F3", sum);
}
```

Pracovní fan-out je distribuován do více instancí funkce `F2`, a práce je sledován pomocí dynamického seznamu úloh. .NET `Task.WhenAll` rozhraní API se nazývá čekání na všechny volané funkce ukončíte. Pak se `F2`funkce výstupy se agregují ze seznamu úkolů dynamické a na předaný `F3` funkce.

Automatické vytváření kontrolních bodů, které se odehrává na `await` volání na `Task.WhenAll` zajistí všechny havárie nebo restartování polovině prostřednictvím nevyžaduje restartování všech dokončeno úlohy.

## <a name="pattern-3-async-http-apis"></a>Vzor #3: Rozhraní API HTTP asynchronní

Třetí vzor se točí kolem problém koordinace stav dlouhotrvající operace s externími klienty. Běžný způsob implementace tohoto vzoru spočívá v použití akce dlouho běžící aktivovány volání protokolu HTTP a pak přesměruje klienta na stav koncového bodu, který se může dotazovat na další po dokončení operace.

![Diagram rozhraní API HTTP](media/durable-functions-overview/async-http-api.png)

Trvanlivý funkce poskytuje integrované rozhraní API, která zjednodušit kód, který můžete psát pro interakci s jednotlivými spuštěními funkce dlouho běžící. [Ukázky](durable-functions-install.md) zobrazit jednoduchý příkaz REST, který můžete použít ke spuštění nové instance funkce produktu orchestrator. Po spuštění instance rozšíření zpřístupní webhook rozhraní API HTTP takový dotaz stav funkce produktu orchestrator. Následující příklad ukazuje REST příkazy k spuštění orchestrator a dotaz na stav. Některé podrobnosti jsou pro přehlednost vynechání z příkladu.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2017-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2017-03-16T21:20:57Z", ...}
```

Vzhledem k tomu, že stav je spravovaná službou trvanlivý funkce modulu runtime, nemáte implementovat vlastní stav sledování mechanismus.

I když rozšíření trvanlivý funkce má integrovanou webhooky pro správu dlouho běžící orchestrations, můžete implementovat tento vzor sami pomocí vlastní funkce aktivace (třeba HTTP, fronty nebo Centrum událostí) a `orchestrationClient` vazby. Můžete například použít zprávu fronty k aktivaci ukončení.  Nebo můžete použít aktivační procedury HTTP chráněné službou Azure Active Directory zásad ověřování namísto integrované webhooků, který pomocí generovaného klíče pro ověřování. 

```cs
// HTTP-triggered function to start a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    TraceWriter log)
{
    // Function name comes from the request URL.
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);
    
    log.Info($"Started orchestration with ID = '{instanceId}'.");
    
    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parametr je hodnota z `orchestrationClient` výstup vazby, která je součástí rozšíření trvanlivý funkce. Poskytuje metody pro počáteční, odeslání události, ukončení a dotazuje se na nový nebo existující orchestrator funkce instancí. V předchozím příkladu protokolu HTTP aktivované – funkce přebírá `functionName` hodnotu z příchozí adresy URL a předává, které hodnoty na [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). Toto rozhraní API vazby vrátí odpověď obsahující `Location` hlavičky a další informace o instanci, která můžete později použije pro vyhledávání registrace stavu spuštěna instance nebo ho ukončit.

## <a name="pattern-4-stateful-singletons"></a>Vzor #4: Stavová jednotlivých prvků

Většina funkcí mít explicitní počáteční a koncové a nemáte komunikovat přímo s zdroje externí událostí. Ale podporují orchestrations [stavová singleton](durable-functions-singletons.md) vzor, který vám umožní se chovat jako spolehlivého [aktéři](https://en.wikipedia.org/wiki/Actor_model) v distribuovaných počítačů.

Následující diagram znázorňuje funkci, která běží v nekonečné smyčce při zpracování událostí přijatých z externích zdrojů.

![Diagram stavová singleton](media/durable-functions-overview/stateful-singleton.png)

Přestože trvanlivý funkce není implementace objektu actor modelu, funkce orchestrator mají mnoho společných vlastností s stejné runtime. Například jsou dlouho běžící (může být nekonečná), stateful, spolehlivé, jedním podprocesem, transparentní pro umístění a globálně adresovatelné. Díky tomu funkce orchestrator užitečné pro "objektu actor"-jako scénáře.

Běžné funkce jsou bezstavové a není proto vhodný implementace vzoru stavová singleton. Však rozšíření trvanlivý funkce usnadňuje vzoru stavová singleton trivial relativně k implementaci. Následující kód je jednoduchý orchestrator funkce, která implementuje čítače.

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    int counterState = ctx.GetInput<int>();

    string operation = await ctx.WaitForExternalEvent<string>("operation");
    if (operation == "incr")
    {
        counterState++;
    }
    else if (operation == "decr")
    {
        counterState--;
    }

    ctx.ContinueAsNew(counterState);
}
```

Tento kód je může popisují jako "eternal orchestration" &mdash; tedy ten, který spustí a nikdy se ukončí. Se provede následující kroky:

* Začíná vstupní hodnoty v `counterState`.
* Volá počká po neomezenou dobu pro zprávu `operation`.
* Provádí některé logiku a aktualizovat stav místní.
* "Restartuje" samotné voláním `ctx.ContinueAsNew`.
* Čeká znovu po neomezenou dobu pro další operace.

## <a name="pattern-5-human-interaction"></a>Vzor #5: Zásahem ze strany

Velký počet procesů zahrnovat nějaký druh zásahem ze strany obsluhy. Složité věc o zahrnující člověka v automatizovaného procesu je, že uživatelé nejsou vždy jako vysoce dostupný a dobře reagovaly jako cloudové služby. Automatizované procesy musíte povolit pro tuto a jejich často to udělat pomocí časových limitů a kompenzace logiku.

Příkladem obchodní proces, který zahrnuje zásahem ze strany je proces schvalování. Například může být schválení od vedoucího požadované pro sestavu výdajů, které překročí určitou. Pokud správce nebude schvalovat do 72 hodin (možná, že se na dovolenou), eskalace proces se spustí pro získání schválení od jiného uživatele (možná manažera manager).

![Diagram zásahem ze strany](media/durable-functions-overview/approval.png)

Tento vzor můžete implementovat pomocí funkce produktu orchestrator. Orchestrator využije [trvanlivý časovače](durable-functions-timers.md) požádat o schválení a zvýšení v případě vypršení časového limitu. By počkejte [externí událostí](durable-functions-external-events.md), který bude oznámení generované některé zásahem ze strany.

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    await ctx.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = ctx.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = ctx.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = ctx.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await ctx.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await ctx.CallActivityAsync("Escalate");
        }
    }
}
```

Trvanlivý časovač se vytvoří voláním `ctx.CreateTimer`. Doručení pomocí `ctx.WaitForExternalEvent`. A `Task.WhenAny` nazývá se můžete rozhodnout, jestli chcete-li postoupit (vypršení časového limitu se stane nejprve) nebo zpracovat schválení (schválení je obdrženy předtím, než časový limit).

## <a name="the-technology"></a>Technologie

Na pozadí rozšíření trvanlivý funkce je postavený na [trvanlivý Framework úloh](https://github.com/Azure/durabletask), knihovny s otevřeným zdrojem na Githubu pro vytváření orchestrations trvanlivý úloh. Jako jak Azure Functions je bez serveru vývoj Azure WebJobs, mnohem trvanlivý Functions je bez serveru vývoj trvanlivý Framework úloh. Trvanlivý úloh Framework se používá výraznou v rámci společnosti Microsoft a mimo také automatizovat klíčové procesy. Je přirozené přizpůsobit pro bez serveru prostředí Azure Functions.

### <a name="event-sourcing-checkpointing-and-replay"></a>Událost sourcing, vytváření kontrolních bodů a opětovného přehrání

Funkce Orchestrator spolehlivě zachovat jejich stavu spuštění pomocí vzoru návrhu cloudu známé jako [Sourcing událostí](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing). Místo ukládání přímo *aktuální* stav orchestration, trvanlivý rozšíření používá k zaznamenání úložišti připojovacím *úplné sérii akcí* provedenou funkce orchestration. To má mnoho výhod, jako třeba vylepšení výkonu, škálovatelnosti a odezvy ve srovnání s "vypsání" stav úplné modulu runtime. Mezi další výhody patří konzistence typu případné poskytuje pro transakční data a údržbu úplných a historie. Záznamy auditu, sami povolení spolehlivé zušlechtěných akcí.

Použití událostí Sourcing toto rozšíření je transparentní. V pozadí `await` operátor v funkci orchestrator vypočítá kontrolu nad vlákno orchestrator zpět do dispečera trvanlivý Framework úloh. Dispečer pak potvrdí všechny nové akce, které funkce orchestrator naplánované (například volání funkcí jeden nebo více podřízených nebo plánování trvanlivý časovač) do úložiště. Tato akce transparentní potvrzení připojí k *historie provádění* instance orchestration. Historie je uložena v tabulce úložiště. Akce zápisu pak přidá zprávy do fronty při plánování samotnou práci. V tomto okamžiku funkce orchestrator může být uvolněn z paměti. Fakturace pro něj zastaví, pokud používáte plánování využívání funkce Azure.  Pokud existuje další práci udělat, je funkce restartování a její stav je znovu vytvořena.

Jakmile funkce orchestration je zadána více práce uděláte (například je přijatá zpráva odpovědi nebo trvanlivý vyprší), orchestrator probudí se znovu a aby bylo možné znovu sestavit místní stav znovu spustí celý funkce od začátku. Pokud během tohoto opětovného přehrání kód pokusí volat funkci (nebo provést jiné asynchronní pracovní), rozhraní trvanlivý úloh zajímají s *historie provádění* aktuální Orchestrace. Pokud zjistí, že už provedena funkce aktivitu a poskytuje některé výsledek, replays výsledku této funkce a kód orchestrator běžet dál. Tento stav bude trvat děje, dokud funkce kód získá do bodu, kde buď dokončení nebo má naplánovanou práci nový asynchronní.

### <a name="orchestrator-code-constraints"></a>Omezení kód produktu Orchestrator

Chování opětovného přehrání vytvoří omezení na typu kód, který může být napsán v funkce produktu orchestrator. Kód produktu orchestrator například musí být deterministický, jak budou přehrány několikrát a musí mít stejný výsledek pokaždé, když. Úplný seznam omezení lze nalézt v [Orchestrator kód omezení](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints) části **vytváření kontrolních bodů a restartujte** článku.

## <a name="language-support"></a>Podpora jazyků

C# se v současné době pouze podporovaných jazycích pro odolná funkce. To zahrnuje orchestrator funkce a funkce aktivity. V budoucnu přidáme podpora pro všechny jazyky, které podporuje Azure Functions. Azure Functions najdete v části [seznam problémů úložiště GitHub](https://github.com/Azure/azure-functions-durable-extension/issues) chcete zobrazit nejnovější stav naše další jazyková podpora práce.

## <a name="monitoring-and-diagnostics"></a>Monitorování a diagnostika

Trvanlivý funkce rozšíření automaticky vysílá sledování strukturovaných dat [Application Insights](functions-monitoring.md) když je funkce aplikace nakonfigurovaná s klíčem instrumentace Application Insights. Tato data sledování můžete použít ke sledování chování a průběh vaší orchestrations.

Tady je příklad vzhled trvanlivý funkce sledování událostí v portálu Application Insights pomocí [Application Insights Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics):

![Výsledky dotazu statistiky aplikace](media/durable-functions-overview/app-insights-1.png)

Existuje mnoho užitečné strukturovaná data zabalit do `customDimensions` pole v každé položky protokolu. Tady je příklad takové položky úplně rozbalit.

![pole customDimensions v aplikaci Statistika dotazu](media/durable-functions-overview/app-insights-2.png)

Z důvodu opětovného přehrání chování dispečera trvanlivý Framework úloh lze očekávat zobrazíte položky redundantní protokolu pro přehraná akce. To může být užitečné pochopit chování opětovného přehrání modulu jádra. [Diagnostiky](durable-functions-diagnostics.md) článek ukazuje ukázkové dotazy, které protokoly opětovného přehrání filtrovat, abyste viděli pouze "v reálném čase" protokoly.

## <a name="storage-and-scalability"></a>Úložiště a škálovatelnost

Rozšíření trvanlivý funkce používá fronty Azure Storage, tabulky a objekty BLOB se zachovat provádění historie stavu a aktivační události funkce provádění. Můžete použít výchozí účet úložiště pro funkce aplikace, nebo můžete nakonfigurovat účet samostatného úložiště. Můžete chtít samostatný účet z důvodu omezení propustnosti úložiště. Orchestrator kód, který můžete psát nemusí (a neměli) komunikovat s entity v tyto účty úložiště. Entity, které jsou spravovány přímo trvanlivý Framework úloh jako podrobností implementace.

Funkce Orchestrator plán aktivita funkce a jejich odpovědi prostřednictvím zprávy vnitřní fronty přijímat. Když funkce aplikace běží v plánu spotřeby funkce Azure, tyto fronty jsou monitorovány pomocí [Azure funkce škálování řadiče](functions-scale.md#how-the-consumption-plan-works) a nový výpočet instance přidány podle potřeby. Při škálovat na více systémů pro víc virtuálních počítačů, funkce orchestrator může spustit v jeden virtuální počítač při spuštění funkce aktivity, který volá na několik různých virtuálních počítačích. Další informace naleznete na chování škálování trvanlivý funkcí v [výkonu a možností škálování](durable-functions-perf-and-scale.md).

Úložiště Table se používá k ukládání historie provádění pro orchestrator účty. Vždy, když instance rehydrates na konkrétním virtuálním počítači, ho načte jeho historie provádění z úložiště tabulek tak, aby ji můžete znovu sestavit stav místní. Jednou z věcí pohodlný o nutnosti historie k dispozici ve službě Table storage je, můžete si je můžete prohlédnout a najdete v historii vaší orchestrations pomocí nástrojů, jako [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Snímek Azure Storage Explorer obrazovky](media/durable-functions-overview/storage-explorer.png)

> [!WARNING]
> I když je snadný a pohodlný zobrazíte historii spouštění ve službě table storage, neberte některé závislé na této tabulce. Se může změnit při zpracovaní rozšíření trvanlivý funkce.

## <a name="known-issues-and-faq"></a>Známé problémy a nejčastější dotazy

Obecně platí, by měly být všechny známé problémy sledovány v [Githubu problémy](https://github.com/Azure/azure-functions-durable-extension/issues) seznamu. Pokud dojde k potížím a nelze najít problém v Githubu, otevřete nový problém a obsahovat podrobný popis problému. I v případě, že chcete stačí položit dotaz, klidně si otevřete potíže Githubu a označit ji jako dotaz.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Pokračujte ve čtení trvanlivý funkce dokumentace](durable-functions-bindings.md)

> [!div class="nextstepaction"]
> [Nainstalujte rozšíření trvanlivý funkce a ukázky](durable-functions-install.md)

