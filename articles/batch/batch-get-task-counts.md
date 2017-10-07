---
title: "aaaMonitor průběh úlohy roven úlohy podle stavu – Azure Batch | Microsoft Docs"
description: "Monitorování průběhu hello úlohy voláním operace získání úkolů počty hello toocount úlohy pro úlohu. Můžete získat počet aktivní, spuštěné a dokončené úlohy a úlohy, které mají byla úspěšná nebo neúspěšná."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/02/2017
ms.author: tamram
ms.openlocfilehash: 03957d8a3d678bf44587f3bc7f988a76885c2af0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="count-tasks-by-state-toomonitor-a-jobs-progress-preview"></a>Počet úloh ve stavu toomonitor průběh úlohy (Preview)

Azure Batch poskytuje efektivní způsob toomonitor hello průběh úlohy při jeho spuštění její úkoly. Můžete volat hello [získat počty úloh] [ rest_get_task_counts] toofind operaci na tom, kolik úlohy jsou ve stavu aktivní, spuštěné nebo pokud byla dokončena a kolik mají byla úspěšná nebo neúspěšná. Roven hello počet úloh v každém stavu, můžete snadno zobrazit úlohy hello průběh tooa uživatele nebo zjistit neočekávané zpoždění nebo chybám, které mohou ovlivnit hello úlohy.

> [!IMPORTANT]
> Hello operace získání počty úloh je aktuálně ve verzi preview a dosud nejsou k dispozici v Azure Government, Čína Azure a Azure v Německu. 
>
>

## <a name="how-tasks-are-counted"></a>Jak se úlohy počítají

Hello získat počty úloh operaci počty úlohy podle stavu, následujícím způsobem:

- Úloha se počítá jako **active** Pokud je ve frontě a musí umožňovat toorun, ale není aktuálně přiřazen tooa výpočetním uzlu. Úloha se také počítá jako **active** Pokud závisí na nadřazený úkol, který zatím není dokončený. Další informace o závislosti úkolů najdete v tématu [vytvoření závislosti úkolů toorun úlohy, které jsou závislé na dalších úkolech,](batch-task-dependencies.md). 
- Úloha se počítá jako **systémem** když byl přiřazen tooa výpočetním uzlu, ale zatím není dokončený. Úloha se počítá jako **systémem** při její stav je buď `preparing` nebo `running`, jak je indikován hello [získat informace o úkolu] [ rest_get_task] operaci.
- Úloha se počítá jako **Dokončit** Pokud již není vhodné toorun. Úloha se počítají jako **Dokončit** má obvykle buď bylo dokončeno úspěšně, nebo je neúspěšně dokončené a také již vyčerpán limitu opakování. 

Hello získat počty úloh operace také sestavy, kolik úlohy mají byla úspěšná nebo neúspěšná. Batch Určuje, jestli má úloha byla úspěšná nebo neúspěšná kontrolou hello **výsledek** vlastnost hello [executionInfo] [https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task#executionInfo] Vlastnost:

    - Úloha se počítá jako **úspěšné** Jestliže hello výsledek provádění úlohy `success`.
    - Úloha se počítá jako **se nezdařilo** Jestliže hello výsledek provádění úlohy `failure`.

Další informace o stavů úkolů najdete v tématu [získat informace o úkolu][rest_get_task].

Hello následující ukázka kódu .NET ukazuje, jak tooretrieve úloh počty podle stavu: 

```csharp
var taskCounts = await batchClient.JobOperations.GetTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
Console.WriteLine("ValidationStatus: {0}", taskCounts.ValidationStatus);
```

> [!NOTE]
> Můžete použít podobný Princip pro REST a jiné podporované jazyky, které počty tooget úloh pro úlohu. 
> 
> 

## <a name="consistency-checking-for-task-counts"></a>Pro počtů úloha kontroly konzistence

Služba Batch Hello agreguje počty úloh ve shromažďování dat z více částí asynchronní distribuovaného systému. tooensure tento úkol počty jsou správné, Batch poskytuje další ověřování pro stav počty podle provádění kontroly konzistence pro více součástí systému hello. Batch provádí tyto kontroly konzistence, dokud nejsou méně než 200 000 úkoly v úloze hello. V případě pravděpodobně hello, že kontrola konzistence hello zjistí chyby opraví Batch hello výsledek operace hello získat úlohy počítá na základě výsledků kontroly konzistence hello hello. Hello Kontrola konzistence je tooensure navíc měr, který zákazníky, kteří využívají hello získat počty úloh operace získání hello náležité informace, které potřebují pro své řešení.

Hello **validationStatus** vlastnost v odpovědi hello udává, zda dávky byla provedena kontrola konzistence hello. Pokud Batch nebylo možné toocheck stavu počty proti hello skutečné stavy uchovávat v hello systému, potom hello **validationStatus** vlastnost je nastavena příliš`unvalidated`. Z důvodů výkonu Batch nebude provádět kontrolu konzistence hello hello úlohy obsahuje-li více než 200 000 úkoly, takže hello **validationStatus** vlastnost může být nastaven příliš`unvalidated` v tomto případě. Ale počet úkolů hello není nutně nesprávný v takovém případě jako i ke ztrátě dat velmi omezená je vysoce nepravděpodobné. 

Když se úloha změní stav, zpracuje hello agregace kanálu hello změně v rámci několik sekund. Hello získat počty úloh operaci odráží počty úloh hello aktualizovat v rámci této doby. Nicméně pokud hello agregace kanálu nezdařených přístupů k zjištění změny ve stavu úloh, pak změna není zaregistrovaný až další ověření průchodu hello. Během této doby počty úloh mohou být mírně nepřesné kvůli toohello zmeškaných událostí, ale jsou opravena při průchodu další ověření hello.

## <a name="best-practices-for-counting-a-jobs-tasks"></a>Osvědčené postupy pro počítání úlohy

Volání operace získání úkolů počty hello je hello nejúčinnější způsob, jak tooreturn základní počet úlohy podle stavu. Pokud používáte verzi služby Batch 2017-06-01.5.1, doporučujeme zápis nebo aktualizaci vaší kód toouse získat počty úloh.

Hello získat počty úloh operaci starší než 2017-06-01.5.1 není k dispozici ve verzích služby Batch. Pokud používáte starší verzi hello služby, potom použijte úlohy toocount dotaz seznam v úlohu. Další informace najdete v tématu [toolist prostředky Batch efektivně vytvářet dotazy](batch-efficient-list-queries.md).

## <a name="next-steps"></a>Další kroky

* V tématu hello [přehled funkcí Batch](batch-api-basics.md) toolearn Další informace o konceptech služby Batch a funkce. Hello článek popisuje hello primární prostředky služby Batch například fondy, výpočetní uzly, úlohy a úkoly a nabízí přehled funkcí služby hello.
* Další informace hello základy vývoje aplikací s povolenými Batch pomocí hello [klientské knihovny Batch .NET](batch-dotnet-get-started.md) nebo [Python](batch-python-tutorial.md). Tyto články úvodní vás provede funkční aplikaci, která používá tooexecute služby Batch hello zatížení v několika výpočetních uzlech.


[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_task]: https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task
[rest_list_tasks]: https://docs.microsoft.com/rest/api/batchservice/list-the-tasks-associated-with-a-job
