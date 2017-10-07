---
title: "dotazy efektivní seznamu aaaDesign – Azure Batch | Microsoft Docs"
description: "Zvyšuje výkon filtrování vašich dotazů při žádosti o Batch prostředkům, jako jsou fondy, úlohy, úlohy a výpočetní uzly."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 08/02/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7e554119ec9d0e9e8007ccfb1ca80fe142a5e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-queries-toolist-batch-resources-efficiently"></a>Vytváření dotazů toolist prostředky Batch efektivně

Zde dozvíte, jak tooincrease výkon aplikace Azure Batch snížením hello množství dat, která je vrácena službou hello po dotazu úlohy, úkoly a výpočetní uzly s hello [Batch .NET] [ api_net] knihovny.

Téměř všechny aplikace Batch nutné tooperform nějaký typ monitorování nebo jiné operace, který se dotazuje služby Batch hello, často v pravidelných intervalech. Toodetermine například, zda jsou všechny ve frontě úloh zbývající v rámci úlohy, musíte získat data na každý úkol v úloze hello. Stav hello toodetermine uzly ve fondu, musíte získat data na každý uzel ve fondu hello. Tento článek vysvětluje, jak tooexecute například dotazy v hello nejefektivnějším způsobem.

> [!NOTE]
> Hello služba Batch poskytuje speciální podporu rozhraní API pro běžné scénáře hello počítání úkoly v úloze. Místo použití seznamu dotazu pro tyto, můžete volat hello [získat počty úloh] [ rest_get_task_counts] operaci. Počet úloh Get Určuje, kolik úlohy čekají na vyřízení, spuštění nebo dokončení, a mít kolik úlohy byla úspěšná nebo neúspěšná. Spočítá počet úloh GET je efektivnější než seznamu dotazu. Další informace najdete v tématu [počet úloh pro úlohu podle stavu (Preview)](batch-get-task-counts.md). 
>
> Hello získat počty úloh operaci starší než 2017-06-01.5.1 není k dispozici ve verzích služby Batch. Pokud používáte starší verzi hello služby, potom použijte úlohy toocount dotaz seznam v úlohu.
>
> 

## <a name="meet-hello-detaillevel"></a>Splňovat hello DetailLevel
V produkční aplikaci služby Batch můžete v hello tisíc čísel entity jako úlohy, úlohy a výpočetní uzly. Pokud budete požadovat informace o těchto prostředků, potenciálně velkého množství dat musí "křížová přenosová hello" z aplikace tooyour služby Batch hello na každý dotaz. Omezením hello počet položek a typu informací, které je vrácených dotazem můžete zvýšit rychlost hello své dotazy a proto hello výkon aplikace.

To [Batch .NET] [ api_net] seznamy fragmentu kódu rozhraní API *každých* úlohu, která je spojená s úlohou, spolu s *všechny* hello vlastností jednotlivých úloha:

```csharp
// Get a collection of all of hello tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Mnohem efektivnější seznamu dotazu, můžete však provést použitím tooyour dotazu "úroveň podrobností". To provedete zadáním [ODATADetailLevel] [ odata] objektu toohello [JobOperations.ListTasks] [ net_list_tasks] metoda. Tento fragment kódu vrátí pouze hello ID, příkazový řádek a výpočetní uzel informace vlastnosti dokončených úloh:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties tooreturn
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply hello ODATADetailLevel toohello ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

V tomto ukázkovém scénáři, pokud existují tisíce úkolů v úloze hello hello výsledky z dotazu druhý hello obvykle bude vrácena jako mnohem rychlejší než hello první. Další informace o používání ODATADetailLevel při seznamu položek s hello Batch .NET API je součástí [pod](#efficient-querying-in-batch-net).

> [!IMPORTANT]
> Důrazně doporučujeme, aby vám *vždy* dodávky seznam ODATADetailLevel tooyour objektů .NET API volá tooensure maximální efektivity a výkonu vaší aplikace. Zadáním úroveň podrobností můžete pomoct toolower dávky služby odezvy, zvýšit využití sítě a minimalizovat využití paměti v klientských aplikacích.
> 
> 

## <a name="filter-select-and-expand"></a>Filtrovat, vyberte a rozbalte
Hello [Batch .NET] [ api_net] a [Batch REST] [ api_rest] rozhraní API nabízejí možnost tooreduce hello obou hello počet položek, které jsou vráceny v seznamu, a také hello objem informací, které se vrátí pro každou. To uděláte tak, že zadáte **filtru**, **vyberte**, a **rozbalte řetězce** při provádění dotazů seznamu.

### <a name="filter"></a>Filtr
řetězec filtru Hello je výraz, který snižuje hello počet položek, které jsou vráceny. Například seznam pouze hello spuštěných úloh pro úlohu nebo seznamu pouze výpočetní uzly, které jsou připravené toorun úlohy.

* řetězec filtru Hello se skládá z jednoho nebo více výrazů s výrazem, který obsahuje název vlastnosti, operátor a hodnotu. Hello vlastnosti, které lze zadat jsou konkrétní tooeach typu entity, který dotazování, jako jsou hello operátory, které jsou podporovány pro každou vlastnost.
* Více výrazů lze spojovat pomocí logických operátorů hello `and` a `or`.
* Tento příklad filtrování seznamů řetězec jen hello spuštění úlohy "vykreslení": `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Vyberte
Vyberte řetězec Hello omezuje hello hodnoty vlastností, které se vrátí pro každou položku. Zadejte seznam názvů vlastností a pouze hodnoty těchto vlastností jsou vráceny hello položek ve výsledcích dotazů hello.

* Vyberte řetězec Hello se skládá z seznam názvů vlastností oddělených čárkami. Můžete zadat jakýkoli hello vlastnosti pro typ entity hello, který se dotazuje.
* Tento příklad vyberte řetězec Určuje, že má být vrácen pouze tři hodnoty vlastností pro každou úlohu: `id, state, stateTransitionTime`.

### <a name="expand"></a>Rozbalit
Hello rozbalte položku řetězec snižuje hello počet volání rozhraní API, které jsou požadované tooobtain určité informace. Použijete-li řetězec rozbalte, nelze získat další informace o jednotlivých položkách na základě jednoho volání rozhraní API. Místo první získání hello seznamu entity, pak se žádajícího informace pro každou položku v seznamu hello využít řetězec rozbalte tooobtain hello stejné informace v jediném volání rozhraní API. Menší volání rozhraní API znamená vyšší výkon.

* Podobné toohello vyberte řetězec, hello rozbalte řetězec ovládací prvky, jestli některá data je zahrnuta v seznamu výsledků dotazu.
* Hello rozbalte položku řetězec je podporována pouze v případě, že se používá v seznam úloh, plány úloh, úlohy a fondy. V současné době podporuje pouze statistické informace.
* Když je zadán žádný vyberte řetězec vyžadují se všechny vlastnosti, rozbalte hello řetězec *musí* být použité tooget statistické informace. Pokud vyberte řetězec je použité tooobtain podmnožinu vlastností `stats` lze zadat v řetězci hello vyberte a rozbalte hello řetězec nemusí toobe zadán.
* Tento příklad rozbalte položku řetězec Určuje, zda má být vrácen statistické informace pro každou položku v seznamu hello: `stats`.

> [!NOTE]
> Při vytváření žádné hello tři typy řetězce dotazů (filtrování, vyberte a rozbalte možnost), musí zajistěte, aby názvy vlastností hello a případ odpovídaly s jejich protějšky element REST API. Například při práci s hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) třídy, je nutné zadat **stavu** místo **stavu**, i když hello vlastnost .NET [ CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Najdete v části tabulky hello pod pro vlastnost mapování mezi hello .NET a REST API.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Pravidla pro filtr, vyberte a rozbalte řetězce
* Názvy vlastností ve filtru, vyberte a rozbalte řetězce by se měla zobrazit jako ve hello [Batch REST] [ api_rest] rozhraní API – i v případě, že používáte [Batch .NET] [ api_net] nebo jeden z hello jiných SDK služby Batch.
* Všechny názvy vlastností malá a velká písmena, ale hodnoty vlastností se malá a velká písmena.
* Datum a čas řetězců může mít jednu ze dvou formátů a musí předcházet `DateTime`.
  
  * Příklad formátu W3C DTF:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * Příklad formátu RFC 1123:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Logická hodnota řetězce jsou buď `true` nebo `false`.
* Pokud je zadána neplatná vlastnost nebo operátor, `400 (Bad Request)` bude výsledkem chyba.

## <a name="efficient-querying-in-batch-net"></a>Efektivní dotazování v Batch .NET
V rámci hello [Batch .NET] [ api_net] rozhraní API, hello [ODATADetailLevel] [ odata] třída se používá pro zadávání filtru, vyberte a rozbalte řetězce toolist operace. Hello ODataDetailLevel třída má tři vlastnosti veřejné řetězce, které lze zadat v konstruktoru hello nebo nastavit přímo na objekt hello. Pak předáte hello ODataDetailLevel objekt jako parametr toohello různých seznam operací, jako [ListPools][net_list_pools], [ListJobs][net_list_jobs], a [ListTasks][net_list_tasks].

* [ODATADetailLevel][odata].[ FilterClause][odata_filter]: omezit hello počet položek, které jsou vráceny.
* [ODATADetailLevel][odata].[ SelectClause][odata_select]: Zadejte každou položku jsou vráceny hodnot vlastností.
* [ODATADetailLevel][odata].[ ExpandClause][odata_expand]: načtení dat pro všechny položky v jediného rozhraní API volat místo samostatné volání pro každou položku.

Hello následující fragment kódu používá hello Batch .NET API tooefficiently dotazu hello Batch službu pro hello statistiky konkrétní sadu fondů. V tomto scénáři má uživatel Batch hello testovací a produkční fondů. Hello testovací fondu ID mají předponu "test" a hello produkční fondu ID mají předponu "produkčnímu". Ve fragmentu hello *myBatchClient* je správně inicializována instanci hello [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) třídy.

```csharp
// First we need an ODATADetailLevel instance on which tooset hello filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want toopull only hello "test" pools, so we limit hello number of items returned
// by using a FilterClause and specifying that hello pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// toofurther limit hello data that crosses hello wire, configure hello SelectClause to
// limit hello properties that are returned on each CloudPool object tooonly
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify hello ExpandClause so that hello .NET API pulls hello statistics for the
// CloudPools in a single underlying REST API call. Note that we use hello pool's
// REST API element name "stats" here as opposed too"Statistics" as it appears in
// hello .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing hello amount of data that is returned
// by specifying hello detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> Instance [ODATADetailLevel] [ odata] nakonfigurovaný s vyberte a rozbalte klauzule lze také předat tooappropriate metody Get, jako například [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx) , toolimit hello množství dat, která je vrácena.
> 
> 

## <a name="batch-rest-toonet-api-mappings"></a>Batch REST too.NET API mapování
Názvy vlastností ve filtru, vyberte a rozbalte řetězce *musí* odráží jejich protějšky REST API, v názvu i případu. Následující tabulky Hello zadejte mapování mezi hello .NET a REST API protějšky.

### <a name="mappings-for-filter-strings"></a>Mapování pro filtr řetězce
* **Seznam metod rozhraní .NET**: každá z metod .NET API hello v tomto sloupci přijímá [ODATADetailLevel] [ odata] objekt jako parametr.
* **REST seznamu požadavků**: každý REST API stránky propojené tooin tento sloupec obsahuje tabulku, která určuje vlastnosti hello a operace, které jsou povoleny v *filtru* řetězce. Když vytvoříte budou používat tyto operace a názvy vlastností [ODATADetailLevel.FilterClause] [ odata_filter] řetězec.

| Seznam metod rozhraní .NET | REST seznamu požadavků |
| --- | --- |
| [CertificateOperations.ListCertificates][net_list_certs] |[Seznam certifikátů hello na účtu][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[Seznam hello soubory spojené s úlohami][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[Stav seznamu hello hello úlohy přípravy a uvolnění úloh pro úlohu][rest_list_jobprep_status] |
| [JobOperations.ListJobs][net_list_jobs] |[Seznam úloh hello v účtu][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[Seznam souborů hello na uzlu][rest_list_nodefiles] |
| [JobOperations.ListTasks][net_list_tasks] |[Seznam hello úkoly spojené s úlohou][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Plány úloh hello seznamu v účtu][rest_list_job_schedules] |
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] |[Seznam hello úlohy přidružené k plánu úlohy][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Seznam hello výpočetních uzlů ve fondu][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[Seznam hello fondy v účtu][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Mapování pro vyberte řetězce
* **Batch .NET typy**: typy Batch .NET API.
* **Rozhraní REST API entity**: obsahuje jednu nebo více tabulek, které seznam názvů vlastností hello REST API pro typ hello každé stránce v tomto sloupci. Tyto názvy vlastností se používají při vytváření *vyberte* řetězce. Tyto stejné názvy vlastností použijete při vytváření [ODATADetailLevel.SelectClause] [ odata_select] řetězec.

| Typy batch .NET | Rozhraní REST API entity |
| --- | --- |
| [Certifikát][net_cert] |[Získání informací o certifikát][rest_get_cert] |
| [CloudJob][net_job] |[Získání informací o úlohy][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[Získat informace o plánu úlohy][rest_get_schedule] |
| [ComputeNode][net_node] |[Získání informací o uzlu][rest_get_node] |
| [CloudPool][net_pool] |[Získat informace o fondu][rest_get_pool] |
| [CloudTask][net_task] |[Získat informace o úkolu][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Příklad: vytvoření řetězec filtru
Když vytvoříte řetězec filtru pro [ODATADetailLevel.FilterClause][odata_filter], naleznete v tabulce hello výše v části "Mapování pro filtr řetězce" toofind hello REST API dokumentace stránky, která odpovídá operace seznamu toohello chcete tooperform. V první více řádků tabulky hello na této stránce najdete hello filtrování vlastností a jejich podporované operátory. Pokud chcete tooretrieve všechny úlohy, jejichž ukončovací kód procesu je nenulové hodnoty, například tento řádek na [seznamu hello úkoly spojené s úlohou] [ rest_list_tasks] určuje povolené operátory a řetězec příslušné vlastnosti hello:

| Vlastnost | Povolené operace | Typ |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Proto by být hello řetězec filtru pro výpis všech úloh s nenulový ukončovací kód:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Příklad: sestavit vyberte řetězec
tooconstruct [ODATADetailLevel.SelectClause][odata_select], projděte si hello tabulce výše v části "Mapování pro vyberte řetězce" a přejděte na stránku toohello REST API, která odpovídá toohello typ entity, které vytváření seznamu. V první více řádků tabulky hello na této stránce najdete hello volitelný vlastností a jejich podporované operátory. Pokud chcete pouze ID hello tooretrieve a příkazový řádek pro každý úkol v seznamu, například zjistíte, tyto řádky v tabulce použít hello na [získat informace o úkolu][rest_get_task]:

| Vlastnost | Typ | Poznámky |
|:--- |:--- |:--- |
| `id` |`String` |`hello ID of hello task.` |
| `commandLine` |`String` |`hello command line of hello task.` |

poté budou vyberte řetězec Hello, včetně pouze hello ID a příkazového řádku u jednotlivých uvedených úkolů:

`id, commandLine`

## <a name="code-samples"></a>Ukázky kódů
### <a name="efficient-list-queries-code-sample"></a>Ukázka kódu dotazy efektivní seznamu
Podívejte se na hello [EfficientListQueries] [ efficient_query_sample] ukázkového projektu na Githubu toosee jak efektivní dotazování na seznamu může ovlivnit výkon v aplikaci. Tato Konzolová aplikace C# vytvoří a přidá velký počet úloh tooa úlohy. Pak umožňuje více volání toohello [JobOperations.ListTasks] [ net_list_tasks] metoda a předává [ODATADetailLevel] [ odata] objekty, které jsou nakonfigurován s jinou vlastnost hodnoty toovary hello množství dat toobe vrátila. Vyvolá podobné toohello následující výstup:

```
Adding 5000 tasks toojob jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER tooquery tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER toocontinue...
```

Jak je znázorněno v hello procentuálně dobu, můžete výrazně snížit dobu odezvy na dotazy omezením hello vlastnosti a hello počet položek, které jsou vráceny. Tato a Další ukázkové projekty můžete najít v hello [azure-batch-samples] [ github_samples] úložišti na Githubu.

### <a name="batchmetrics-library-and-code-sample"></a>Ukázka BatchMetrics knihovny a kódu
Kromě toho ukázka kódu toohello v EfficientListQueries výše, můžete najít hello [BatchMetrics] [ batch_metrics] projekt v hello [azure-batch-samples] [ github_samples] Úložiště GitHub. Ukázkový projekt Hello BatchMetrics ukazuje, jak tooefficiently monitorovat průběh úlohy Azure Batch pomocí rozhraní Batch API hello.

Hello [BatchMetrics] [ batch_metrics] ukázka zahrnuje projektu knihovny tříd rozhraní .NET, která můžete začlenit do vašich vlastních projektů a jednoduchou příkazového řádku programu tooexercise a ukazují použití hello hello Knihovna.

Hello ukázkovou aplikaci v rámci projektu hello ukazuje hello následující operace:

1. Když vyberete konkrétní atributy v pořadí toodownload jenom hello vlastnosti, které budete potřebovat
2. Filtrování na časy přechod stavu v pořadí toodownload pouze změny od poslední dotaz s hello

Například následující metoda hello se zobrazí v hello knihovně BatchMetrics. Vrátí ODATADetailLevel, která určuje, že pouze hello `id` a `state` vlastnosti by měla být získána hello entit, které jsou předmětem dotazování. Také určuje, že pouze entity, jejichž stav se změnil od hello zadaný `DateTime` parametr má být vrácen.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Další kroky
### <a name="parallel-node-tasks"></a>Uzel paralelní úlohy
[Maximalizovat využití prostředků Azure Batch výpočetní uzel souběžných úloh](batch-parallel-node-tasks.md) je jiný článek související tooBatch výkon aplikace. Některé typy úloh využívat výhod spouštění paralelní úlohy na větší – ale méně – výpočetní uzly. Podívejte se na hello [ukázkový scénář](batch-parallel-node-tasks.md#example-scenario) v článku hello podrobnosti o tento případ.

### <a name="batch-forum"></a>Fórum batch
Hello [fóru služby Azure Batch] [ forum] na webu MSDN je skvělá umístit toodiscuss Batch a klást otázky týkající se služby hello. HEAD na přes pro užitečné "rychlé" příspěvky a při jejich vzniku při sestavování řešení Batch zveřejněte svoje otázky.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job