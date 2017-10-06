---
title: "aaaCreate úloh tooprepare úlohy a dokončení úlohy na výpočetních uzlech - Azure Batch | Microsoft Docs"
description: "Použijte úrovní úlohy přípravy úlohy toominimize data přeneste tooAzure Batch výpočetních uzlů a uvolnění úlohy pro vyčištění uzlu na dokončení úlohy."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fd5fb47ae6700281e63048c49a1241f4e935baba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>Spuštění úlohy přípravy a uvolnění úloh na Batch výpočetních uzlů

 Úlohu služby Azure Batch často vyžaduje určitý způsob instalace předtím, než její úkoly spustí a po úlohy údržby, po dokončení jeho úkolů. Může potřebovat toodownload běžných úkolů vstupní data tooyour výpočetní uzly, nebo odeslání úloh výstupní data tooAzure úložiště po dokončení úlohy hello. Můžete použít **úlohy přípravy** a **úlohy verze** úloh tooperform těchto operací.

## <a name="what-are-job-preparation-and-release-tasks"></a>Co jsou úlohy přípravy a uvolnění úlohy?
Před spuštěním úlohy, hello úkol přípravy úlohy běží na všech výpočetních uzlů naplánované toorun alespoň jeden úkol. Po dokončení úlohy hello hello úkol uvolnění úlohy se spustí na každém uzlu ve fondu hello, která spouští alespoň jeden úkol. Stejně jako u normálních úkoly služby Batch, můžete zadat příkazovém řádku toobe volána, když úkol příprava nebo uvolnění úlohy běží.

Přípravy a uvolnění úlohy nabízejí známé dávkové úlohy funkce jako stažení souboru ([soubory prostředků][net_job_prep_resourcefiles]), provedení, vlastní proměnné prostředí, maximální dobu provádění, počet opakování a dobu uchovávání souboru se zvýšenými oprávněními.

V následujících částech hello, získáte informace jak toouse hello [JobPreparationTask] [ net_job_prep] a [JobReleaseTask] [ net_job_release] v hello nalezeny třídy [Batch .NET] [ api_net] knihovny.

> [!TIP]
> Úkoly přípravy a uvolnění úlohy jsou užitečné zejména v prostředích "sdílené fondu", ve které fond výpočetních uzlů potrvají mezi spuštění úloh a používá řadu úloh.
> 
> 

## <a name="when-toouse-job-preparation-and-release-tasks"></a>Když toouse úlohy přípravy a uvolnění úlohy
Úloha přípravy a uvolnění úloh jsou vhodné pro hello následující situace:

**Stahování dat běžných úloh**

Úlohy batch často vyžadují společnou sadu dat jako vstup pro hello úlohy. Například v denní výpočtů analýza rizik a trhu data jsou specifické pro úlohy, ale běžné tooall úkoly v úloze hello. Tento trhu data, často několika gigabajtů velikost, by měla být stažené tooeach výpočetním uzlu pouze jednou, aby ho můžete používat všechny úlohy, které běží na uzlu hello. Použití **úkol přípravy úlohy** toodownload tento uzel tooeach data před hello provádění úlohy hello je další úlohy.

**Odstranit výstup úlohy a úlohy**

V prostředí "sdílené fondu", kde nejsou vyřazení mezi úlohami fondu výpočetních uzlů, může být nutné toodelete data úlohy mezi spustí. Může být nutné tooconserve místa na disku na uzly hello nebo splňují zásady zabezpečení vaší organizace. Použití **úkol uvolnění úlohy** toodelete data, která se stáhne úkol přípravy úlohy nebo vygenerovaných během provedení úlohy.

**Uchování protokolu**

Můžete chtít tookeep kopii soubory protokolů, které vaše úlohy generují nebo možná soubory se stavem systému generovaných aplikací se nezdařilo. Použití **úkol uvolnění úlohy** v takových případech toocompress a nahrajte tato data tooan [Azure Storage] [ azure_storage] účtu.

> [!TIP]
> Jiný způsob toopersist protokoly a další úlohy a úkolů výstupní data jsou toouse hello [Azure Batch souboru konvence](batch-task-output.md) knihovny.
> 
> 

## <a name="job-preparation-task"></a>Úkol přípravy úlohy
Před spuštěním úlohy služba Batch provádí hello úkol přípravy úlohy na každém výpočetním uzlu, který je toorun naplánovanou úlohu. Ve výchozím nastavení služba Batch hello čeká hello úlohy přípravy úlohy toobe dokončit před spuštěním naplánované tooexecute hello úlohy na uzlu hello. Můžete ale nakonfigurovat hello služby není toowait. Pokud restartování uzlu hello hello úkol přípravy úlohy znovu spustí, ale můžete také zakázat toto chování.

Hello úkol přípravy úlohy je provést pouze u uzlů, které jsou naplánované toorun úlohu. Tím se zabrání hello nepotřebné provádění přípravy úlohy v případě, že uzel není přiřazen úlohu. Tato situace může nastat, pokud je číslo hello úloh pro úlohu menší než hello počet uzlů ve fondu. Také platí, když [provedení souběžné úlohy](batch-parallel-node-tasks.md) je povoleno, což ponechá některé uzly nečinnosti Pokud počet úkolů hello je nižší než hello celkový počet možných souběžných úkolů. Spuštěním není hello úkol přípravy úlohy na nečinných uzlů, strávíte nižší cenu na poplatky za přenos dat.

> [!NOTE]
> [JobPreparationTask] [ net_job_prep_cloudjob] se liší od [CloudPool.StartTask] [ pool_starttask] v tom, že JobPreparationTask provede při spuštění hello každé úlohy, zatímco StartTask spustí, pouze když výpočetního uzlu nejprve spojí fond nebo restartování.
> 
> 

## <a name="job-release-task"></a>Úkol uvolnění úlohy
Jakmile úloha je označena jako dokončená, hello úkol uvolnění úlohy se spustí na každém uzlu ve fondu hello, která spouští alespoň jeden úkol. Úlohu můžete označit jako vydáním žádost o ukončení. potom nastaví hello stav úlohy příliš Hello služba Batch*ukončení*, ukončí všechny aktivní nebo spuštěné úkoly spojené s úlohou hello a spouští hello úkol uvolnění úlohy. Úloha Hello pak se posouvá toohello *Dokončit* stavu.

> [!NOTE]
> Odstranění úlohy také provede hello úkol uvolnění úlohy. Ale pokud úloha už byla ukončena, hello uvolnění úlohy není spuštěn podruhé Pokud hello úlohy je později odstranit.
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Příprava úlohy a uvolnění úlohy pomocí rozhraní Batch .NET
přiřadit toouse úkol přípravy úlohy [JobPreparationTask] [ net_job_prep] úlohy tooyour objekt [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] vlastnost . Podobně, inicializovat [JobReleaseTask] [ net_job_release] a přiřaďte ho tooyour úlohy [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] vlastnost tooset hello pro úkol uvolnění úlohy.

V tento fragment kódu `myBatchClient` je instance [BatchClient][net_batch_client], a `myPool` je existující fond v rámci hello účtu Batch.

```csharp
// Create hello CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify hello command lines for hello job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign hello job preparation task toohello job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign hello job release task toohello job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Jak už bylo zmíněno dříve, hello uvolnění úlohy se spustí, až se úloha je ukončeno, nebo odstranit. Ukončit úlohu s [JobOperations.TerminateJobAsync][net_job_terminate]. Odstranit úlohu s [JobOperations.DeleteJobAsync][net_job_delete]. Obvykle ukončit nebo odstranit úlohu po dokončení jeho úkolů, nebo když bylo dosaženo časového limitu, který jste definovali.

```csharp
// Terminate hello job toomark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Ukázka kódu na Githubu
úkoly přípravy a uvolnění úlohy toosee v akci, podívejte se na hello [JobPrepRelease] [ job_prep_release_sample] ukázkového projektu na Githubu. Tato Konzolová aplikace hello následující:

1. Vytvoří fond s dvěma uzly "malá".
2. Vytvoří úlohu s přípravy úlohy, verzi a standardní úlohy.
3. Spustí hello úkol přípravy úlohy, které nejprve zapíše hello uzlu ID tooa textový soubor v adresáři "sdílené" uzlu.
4. Spustí úlohu na každém uzlu, který zapíše jeho toohello ID úloh stejné textového souboru.
5. Jakmile jsou všechny úkoly dokončené (nebo je dosaženo časového limitu hello), vytiskne obsah hello každý uzel textového souboru toohello konzoly.
6. Po dokončení úlohy hello spustí hello úlohy verze úloh toodelete hello soubor z uzlu hello.
7. Hello výtisků ukončovací kódy hello přípravy úlohy a uvolnění úlohy pro každý uzel, na kterém provést.
8. Pozastaví spuštění tooallow potvrzení odstranění úlohy a fondu.

Výstup z ukázkové aplikace hello je podobné toohello následující:

```
Attempting toocreate pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob tooreach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER tooexit...
```

> [!NOTE]
> Z důvodu toohello proměnné vytvoření počáteční čas a uzlů v rámci nového fondu (některé uzly jsou připravené pro úlohy než ostatní) může se zobrazit odlišný výstup. Konkrétně protože hello úkoly dokončí rychle, jeden z uzlů hello fondu může spustit všechny hello úlohy. Pokud k tomu dojde, si všimnete, že hello Příprava úlohy a uvolnění úlohy nejsou k dispozici pro hello uzel, který spuštěné žádné úlohy.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-hello-azure-portal"></a>Zkontrolujte úlohu přípravy a uvolnění úlohy v hello portálu Azure
Když spustíte hello ukázkovou aplikaci, můžete použít hello [portál Azure] [ portal] tooview hello vlastnosti hello úlohy a její úkoly nebo stahujte hello sdílené textový soubor, který je upraveném hello úlohy.

Následující snímek obrazovky Hello ukazuje hello **přípravy úlohy okno** v hello portál Azure po spuštění hello ukázkovou aplikaci. Přejděte toohello *JobPrepReleaseSampleJob* vlastnosti po dokončení úkolů (ale před odstraněním úlohy a fondu) a klikněte na tlačítko **přípravných kroků** nebo **uvolnění úlohy** tooview jejich vlastnosti.

![Vlastnosti přípravy úlohy na portálu Azure][1]

## <a name="next-steps"></a>Další kroky
### <a name="application-packages"></a>Balíčky aplikací
V přidání toohello úkol přípravy úlohy, můžete použít také hello [balíčky aplikací](batch-application-packages.md) funkce Batch tooprepare výpočetní uzly pro provedení úlohy. Tato funkce je užitečná zejména při nasazení aplikací, které nevyžadují systémem instalační program, aplikace, které obsahují mnoho (100 +) souborů nebo aplikace, které vyžadují striktní verzí.

### <a name="installing-applications-and-staging-data"></a>Instalace aplikací a pracovní data
Tento příspěvek na fóru MSDN obsahuje přehled několik metod pro spuštěné úkoly přípravy uzlů:

[Instalace aplikací a pracovní data na Batch výpočetních uzlů][forum_post]

Podle některého z členů týmu Azure Batch hello zapisují, popisuje několik technik, které můžete použít toodeploy aplikacím a datům toocompute uzlů.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

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

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
