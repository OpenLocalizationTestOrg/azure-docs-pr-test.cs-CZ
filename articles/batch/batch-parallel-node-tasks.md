---
title: "aaaRun úlohy v paralelní toouse výpočetní prostředky efektivně - Azure Batch | Microsoft Docs"
description: "Zvýšení efektivity a snížení nákladů pomocí méně výpočetních uzlů a spuštění souběžných úkolů na každém uzlu ve fondu Azure Batch"
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05df4b7d8e0bc595168a97faa231b7c90fe81980
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-tasks-concurrently-toomaximize-usage-of-batch-compute-nodes"></a>Spuštění úlohy souběžně toomaximize využití Batch výpočetních uzlů 

Při spuštění více než jeden úkol současně na každém výpočetním uzlu ve fondu Azure Batch, můžete zvýšit využití prostředků na menší počet uzlů ve fondu hello. Pro některé úlohy to může způsobit kratší časy úloh a nižší náklady.

Při některých scénářích těžit z vyhradit všechny uzlu prostředky tooa jeden úkol, několik situací využívat povolení více úloh tooshare tyto prostředky:

* **Minimalizace přenos dat** úkoly, které mají mít tooshare data. V tomto scénáři můžete výrazně snížit poplatky za přenos dat tak, že zkopírujete sdílených dat tooa menší počet uzlů a provádění úloh paralelně na každém uzlu. To platí hlavně Pokud uzel zkopírovaný tooeach toobe hello dat je třeba přenést mezi zeměpisné oblasti.
* **Tím se maximalizuje využití paměti** při úlohy vyžadovat velké množství paměti, ale pouze během krátkodobě dobu a v proměnné dobu během provádění. Můžete využívat méně, ale větší, výpočetní uzly s více paměti tooefficiently zpracovat takové špičky. Tyto uzly by mít více úloh s paralelně na jednotlivých uzlech spuštěné, ale každý úkol by využívat bohaté paměti hello uzly v různých časech.
* **Zmírnění číslo omezení uzlu** při komunikaci mezi uzly vyžádáním v rámci fondu. Fondy, které jsou nakonfigurované pro komunikaci mezi uzly v současné době jsou omezené too50 výpočetní uzly. Pokud každý uzel v takové fondu je možné tooexecute paralelních úkolů, můžete současně spustit větší počet úloh.
* **Replikace místní výpočetního clusteru**, například když je nejprve přesunout tooAzure výpočetního prostředí. Pokud vaše aktuální řešení místní provede několik úkolů na výpočetním uzlu, můžete zvýšit maximální počet hello uzlu úloh toomore úzce zrcadlení pro tuto konfiguraci.

## <a name="example-scenario"></a>Příklad scénáře
Jako tooillustrate příklad hello výhod provádění paralelních úkolů, Řekněme, že aplikace úkolů má požadavky na procesor a paměť tak, aby [standardní\_D1](../cloud-services/cloud-services-sizes-specs.md) postačí uzly. Ale v pořadí úloh hello toofinish v čase hello vyžaduje, jsou potřebné 1000 tyto uzly.

Místo použití standardní\_D1 uzly, které mají 1 jádro procesoru, můžete použít [standardní\_D14](../cloud-services/cloud-services-sizes-specs.md) uzlů, které mají 16 jader a povolit spouštění paralelních úloh. Proto *16 časy méně uzly* může – místo 1000 uzly jen 63 by bylo zapotřebí. Kromě toho pokud aplikace velké soubory nebo referenční data jsou vyžadovány pro každý uzel, doba trvání úlohy a efektivitu znovu lepší vzhledem k tomu, že hello data je zkopírovaný tooonly 16 uzlů.

## <a name="enable-parallel-task-execution"></a>Povolit provedení paralelní úlohy
Výpočetní uzly pro provedení paralelní úlohy nakonfigurujete na úrovni fondu hello. Pomocí knihovny Batch .NET hello, nastavte hello [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vlastnost při vytváření fondu. Pokud používáte hello Batch REST API, nastavte hello [maxTasksPerNode] [ rest_addpool] element v textu žádosti hello během vytváření fondu.

Azure Batch vám umožní tooset maximální úkolů na uzel nahoru toofour časy (4 x) hello počet jader na uzel. Například pokud hello fond je nakonfigurován s uzly velikosti "Velké" (4 jádra), pak `maxTasksPerNode` může být nastaven too16. Podrobnosti hello počtu jader pro každý uzel velikostí hello najdete v tématu [velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md). Další informace o omezení služby najdete v tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md).

> [!TIP]
> Být jisti tootake do účtu hello `maxTasksPerNode` hodnota při vytváření [vzorec škálování] [ enable_autoscaling] fondu. Například vzorec, jehož výsledkem `$RunningTasks` může mít vliv výrazně zvýšit úkolů na uzlu. V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](batch-automatic-scaling.md) Další informace.
>
>

## <a name="distribution-of-tasks"></a>Rozdělení úkolů
Když hello výpočetních uzlů ve fondu, můžete současně provést úlohy, je důležité toospecify způsob toobe úlohy hello rozdělené mezi uzly hello ve fondu hello.

Pomocí hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] vlastnost, můžete určit, že úlohy by se měla přiřadit rovnoměrně mezi všechny uzly ve fondu hello ("rozšíření"). Nebo můžete zadat, aby co nejvíce úkolů by se měla přiřadit tooeach uzlu před úlohy jsou přiřazeny tooanother uzlu ve fondu hello ("dodací").

Jako příklad, jak tato funkce se hodí v situaci, zvažte hello fond [standardní\_D14](../cloud-services/cloud-services-sizes-specs.md) uzly (v předchozím příkladu hello), které je nakonfigurované [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] hodnotu 16. Pokud hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] nastavena [ComputeNodeFillType] [ fill_type] z *Pack*, by maximalizovat využití všech 16 jader každého uzlu a povolit [automatické škálování fondu](batch-automatic-scaling.md) tooprune nepoužívané uzly z fondu hello (uzlů bez přiřazeny žádné úkoly). To snižuje využití prostředků a úsporu nákladů.

## <a name="batch-net-example"></a>Příklad batch .NET
To [Batch .NET] [ api_net] fragment kódu rozhraní API ukazuje toocreate žádost o fond, který obsahuje čtyři uzly velké s maximálně čtyřmi úkolů na uzlu. Určuje úlohu plánování zásad, který naplní každý uzel s úlohy předchozí tooassigning úlohy tooanother uzlu ve fondu hello. Další informace o přidání fondů pomocí hello Batch .NET API najdete v tématu [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Příklad batch REST
To [Batch REST] [ api_rest] rozhraní API fragment kódu ukazuje toocreate žádost o fond, který obsahuje dva uzly velké s maximálně čtyřmi úkolů na uzel. Další informace o přidání fondů pomocí hello REST API najdete v tématu [přidat účet fondu tooan][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> Můžete nastavit hello `maxTasksPerNode` elementu a [MaxTasksPerComputeNode] [ maxtasks_net] vlastnost pouze při vytváření fondu. Nemůže být upraven po fond již byly vytvořeny.
>
>

## <a name="code-sample"></a>Ukázka kódu
Hello [ParallelNodeTasks] [ parallel_tasks_sample] projektu na Githubu ukazuje použití hello hello [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vlastnost.

Tato Konzolová aplikace C# používá hello [Batch .NET] [ api_net] knihovny toocreate fond s jednou nebo několika výpočetních uzlech. Konfigurovat řadu úloh se provede na těchto uzlech toosimulate proměnné zatížení. Výstup z aplikace hello Určuje, které uzly provést každý úkol. aplikace Hello také poskytuje souhrn hello parametry úlohy a doba trvání. Souhrn část Hello hello výstupu ze dvou různých spustí hello ukázkové aplikace se zobrazí pod.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

první spuštění ukázkové aplikace hello Hello ukazuje, že se do jednoho uzlu v hello fondu a výchozí nastavení hello úkolů na uzel, dobu trvání úlohy hello je více než 30 minut.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Hello druhý spuštění hello ukázka ukazuje výrazného poklesu dobu trvání úlohy. To je proto hello fondu je nakonfigurovaná s čtyři úkolů na uzlu, který umožňuje pro paralelní úloha spuštění toocomplete hello úlohu v téměř čtvrtletí hello času.

> [!NOTE]
> Hello dob trvání úlohy v hello souhrny výše nezahrnují čas vytvoření fondu. Každá z výše uvedených hello úloh byla odeslaná toopreviously vytvořit fondy, jejichž výpočetní uzly se hello *nečinnosti* během odesílání stavu.
>
>

## <a name="next-steps"></a>Další kroky
### <a name="batch-explorer-heat-map"></a>Batch Explorer Heat mapa
Hello [Průzkumník Azure Batch][batch_explorer], jeden z hello Azure Batch [ukázkové aplikace][github_samples], obsahuje *Heat mapa* funkce, která poskytuje vizualizaci provedení úlohy. Pokud jste provádění hello [ParallelTasks] [ parallel_tasks_sample] ukázkovou aplikaci, můžete použít hello Heat mapa funkce tooeasily vizualizovat hello provádění paralelních úloh na každém uzlu.

![Batch Explorer Heat mapa][1]

*Batch Explorer Heat mapa zobrazuje fond čtyři uzly se každý uzel aktuálně spuštěných čtyři úlohy*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
