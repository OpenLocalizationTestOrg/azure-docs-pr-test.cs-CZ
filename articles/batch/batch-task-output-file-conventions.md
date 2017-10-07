---
title: "aaaPersist úloh a úkolů výstup tooAzure úložiště s knihovnou hello souboru konvence pro platformu .NET – Azure Batch | Microsoft Docs"
description: "Zjistěte, jak se knihovna toouse konvence souboru Azure Batch pro .NET toopersist úlohy Batch a tooAzure výstup úlohy úložiště a zobrazení hello trvalý výstupu v hello portálu Azure."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cf2ac8632a13d32438c1bdcf11b4b9649de1e2b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="persist-job-and-task-data-tooazure-storage-with-hello-batch-file-conventions-library-for-net-toopersist"></a>Zachovat úlohy a úkolů data tooAzure úložiště s knihovnou hello konvence souboru Batch pro .NET toopersist 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Jedním ze způsobů toopersist úloh dat je toouse hello [konvence souboru Azure Batch library pro .NET][nuget_package]. Hello souboru konvence knihovny zjednodušuje proces hello ukládání výstupu úlohy tooAzure datového úložiště a jejich načtení. Můžete použít knihovnu hello názvů souboru v kódu úlohy a klienta &mdash; v kódu úlohy pro zachování soubory a v klientovi kód toolist a jejich načtení. Kód úloh můžete také použít hello knihovně tooretrieve hello výstup nadřazeného úlohy, například v [závislosti úkolů](batch-task-dependencies.md) scénář. 

tooretrieve výstupní soubory s hello souboru konvence knihovny, můžete vyhledat soubory hello pro dané úlohy nebo úkolu je vypsáním podle ID a účel. Nepotřebujete tooknow hello názvy nebo umístění souborů hello. Můžete například použít hello souboru konvence knihovny toolist všechny zprostředkující soubory pro danou úlohu nebo získat soubor preview pro danou úlohu.

> [!TIP]
> Počínaje verzí 2017-05-01, hello rozhraní API služby Batch podporuje zachování výstupní data tooAzure úložiště pro úlohy a úkolech Správce úloh, které běží na fondy, které jsou vytvořené pomocí hello konfigurace virtuálního počítače. Hello rozhraní API služby Batch poskytuje jednoduchý způsob toopersist, výstup z hello kódu, který vytvoří úlohu a slouží jako již alternativní toohello souboru konvence knihovny. Můžete upravit toopersist výstupu Batch klienta aplikace bez nutnosti tooupdate hello aplikace spuštěná vaše úloha. Další informace najdete v tématu [zachovat data tooAzure úloh úložiště s hello rozhraní API služby Batch](batch-task-output-files.md).
> 
> 

## <a name="when-do-i-use-hello-file-conventions-library-toopersist-task-output"></a>Kdy použít výstup úlohy hello souboru konvence knihovny toopersist?

Azure Batch poskytuje více než jedním ze způsobů toopersist výstup úlohy. Hello konvence souboru je nejlepší vhodná toothese scénáře:

- Můžete snadno upravit hello kód aplikace hello, že vaše úloha pracuje toopersist soubory pomocí hello souboru konvence knihovny.
- Chcete data tooAzure toostream úložiště při běhu úlohy hello.
- Chcete data toopersist z fondů vytvořené pomocí konfigurace hello cloudové služby nebo konfigurace virtuálního počítače hello.
- Klientské aplikace nebo dalších úloh v hello úlohy toolocate potřeb a stáhněte soubory výstup úlohy podle ID nebo podle účelu. 
- Chcete tooview výstup úlohy v hello portálu Azure.

Pokud váš scénář se liší od výše uvedené, může být nutné tooconsider jiný přístup. Další informace o dalších možnostech pro zachování výstup úlohy najdete v tématu [zachovat úlohy a úkolů výstupní tooAzure úložiště](batch-task-output.md). 

## <a name="what-is-hello-batch-file-conventions-standard"></a>Co je hello dávkového souboru konvence standardní?

Hello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) obsahuje schéma pojmenování pro hello cílové kontejnerů a objektů blob cesty toowhich jsou zapsány výstupní soubory. Soubory trvalou tooAzure úložiště, které splňovat toohello souboru konvence standardní jsou automaticky dostupné pro zobrazení v hello portálu Azure. portál Hello si je vědoma hello zásady vytváření názvů a proto můžete zobrazit soubory, které splňovat tooit.

Hello souboru konvence knihovna pro .NET automaticky názvy kontejnerů úložiště a úloh výstupní soubory podle toohello souboru konvence standardní. Knihovna souboru konvence Hello také poskytuje metody tooquery výstupní soubory ve službě Azure Storage podle toojob ID, ID úlohy nebo účel.   

Pokud vyvíjíte s jiném jazyce než v rozhraní .NET, můžete implementovat standard souboru konvence hello sami ve vaší aplikaci. Další informace najdete v tématu [o hello dávkového souboru konvence standard](batch-task-output.md#about-the-batch-file-conventions-standard).

## <a name="link-an-azure-storage-account-tooyour-batch-account"></a>Odkaz tooyour účtu Azure Storage účtu Batch

toopersist výstupní data tooAzure úložiště pomocí hello souboru konvence knihovny, je nutné nejprve propojit tooyour účet Azure Storage účtu Batch. Pokud jste tak ještě neučinili, propojení tooyour účet úložiště účtu Batch pomocí hello [portál Azure](https://portal.azure.com):

1. Přejděte tooyour účtu Batch na portálu Azure hello. 
2. V části **nastavení**, vyberte **účet úložiště**.
3. Pokud již nemáte účet úložiště spojené s vaším účtem Batch, klikněte na tlačítko **účet úložiště (None)**.
4. Vyberte účet úložiště, ze seznamu hello pro vaše předplatné. Pro nejlepší výkon, použít účet Azure Storage, který je v hello stejné oblasti jako hello účtu Batch, kde běží vaše úkoly.

## <a name="persist-output-data"></a>Zachovat výstupní data

toopersist úloh a úkolů výstupní data s knihovnou hello konvence soubor, vytvořit kontejner ve službě Azure Storage a pak uložte kontejneru toohello výstup hello. Použití hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage) v vaše úloha kód tooupload hello úloh výstup toohello kontejneru. 

Další informace o práci s kontejnery a objekty BLOB ve službě Azure Storage najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

> [!WARNING]
> Všechny výstupy úlohy a úkolů zachovat pomocí konvence souboru hello knihovně jsou uloženy v hello stejnému kontejneru. Pokud zkuste velkého počtu úkolů toopersist soubory v hello stejný čas, [úložiště omezení](../storage/common/storage-performance-checklist.md#blobs) může být požadováno.
> 
> 

### <a name="create-storage-container"></a>Vytvoření kontejneru úložiště

toopersist úloh výstup tooAzure úložiště, nejprve vytvořte kontejner voláním [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync]. Tato metoda rozšíření přijímá [CloudStorageAccount] [ net_cloudstorageaccount] objekt jako parametr. Vytvoří kontejner s názvem podle toohello souboru konvence standardní, tak, aby jeho obsah je zjistitelný podle hello Azure portal a hello metody načtení, které jsou popsány dále v článku hello.

Obvykle umístit hello kód toocreate kontejner v aplikaci klienta &mdash; hello aplikaci, která vytváří vaše fondy, úlohy a úkoly.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference toohello linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create hello blob storage container for hello outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Výstupy úložiště úlohy

Teď, když jste připravili kontejner ve službě Azure Storage, úlohy můžete uložit výstupní toohello kontejneru pomocí hello [TaskOutputStorage] [ net_taskoutputstorage] nalezena třída v souboru konvence knihovny hello.

Ve vašem kódu úloha nejprve vytvořit [TaskOutputStorage] [ net_taskoutputstorage] objekt a potom po dokončení práce hello úloh volání hello [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] toosave metoda jeho výstup tooAzure úložiště.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code tooprocess data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Hello `kind` parametr hello [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) metoda rozděluje hello trvalé soubory. Existují čtyři předdefinované [TaskOutputKind] [ net_taskoutputkind] typy: `TaskOutput`, `TaskPreview`, `TaskLog`, a `TaskIntermediate.` je možné definovat také vlastní kategorie výstupu.

Tyto typy výstup povolit toospecify výstupy danou úlohu jako trvalý, jaký typ výstupy toolist později při dotazu Batch pro hello. Jinými slovy zobrazením hello výstupy úlohy můžete filtrovat seznam hello na jeden z typů výstup hello. Například "mě hello *preview* výstup pro úlohu *109*." Další informace o výpis a načítání výstupy se zobrazí v [načíst výstup](#retrieve-output) dále v článku hello.

> [!TIP]
> Hello výstupní typ také určuje, kde v hello Azure portal určitého souboru se zobrazí: *TaskOutput*-seřazený podle kategorií soubory se zobrazí v části **úloh výstupní soubory**, a *TaskLog*soubory se zobrazí v části **úkolů protokoly**.
> 
> 

### <a name="store-job-outputs"></a>Výstupy úlohy úložiště

Kromě toho výstupy úlohy toostoring, můžete uložit hello výstupy přidružené k celé úlohy. Například v úloze sloučení hello film vykreslování úlohy můžete zachovat film hello plně se vykresluje jako výstup úlohy. Po dokončení úlohy klientské aplikace můžete seznam načíst hello výstupy úlohy hello a nemá nutné tooquery hello jednotlivé úlohy.

Ukládání výstupu úlohy pomocí volání hello [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] metoda a zadejte hello [JobOutputKind] [ net_joboutputkind] a název souboru:

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Stejně jako u hello **TaskOutputKind** typ pro výstupy úlohy, můžete použít hello [JobOutputKind] [ net_joboutputkind] toocategorize typ úlohy je trvalé soubory. Tento parametr umožňuje toolater dotazu pro určitý typ výstupu (list). Hello **JobOutputKind** typ obsahuje výstup a preview kategorie a podporuje vytváření vlastních kategorií.

### <a name="store-task-logs"></a>Úloha protokoly úložiště

Kromě toho toopersisting toodurable úložiště file, pokud úlohy nebo úkolu dokončení, bude pravděpodobně nutné toopersist soubory, které jsou aktualizovány během provádění hello úlohy &mdash; soubory protokolu nebo `stdout.txt` a `stderr.txt`, např. Pro tento účel, poskytuje knihovna Azure Batch souboru konvence hello hello [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] metoda. S [SaveTrackedAsync][net_savetrackedasync], můžete sledovat soubor tooa aktualizace v uzlu hello (na časový interval, který zadáte) a zachovat tyto aktualizace tooAzure úložiště.

V hello následující fragment kódu, použijeme [SaveTrackedAsync] [ net_savetrackedasync] tooupdate `stdout.txt` ve službě Azure Storage každých 15 sekund během provádění hello hello úlohy:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// hello primary task logic is wrapped in a using statement that sends updates to
// hello stdout.txt blob in Storage every 15 seconds while hello task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code tooprocess data and produce output file(s) */

    // We are tracking hello disk file toosave our standard output, but the
    // node agent may take up too3 seconds tooflush hello stdout stream to
    // disk. So give hello file a moment toocatch up.
     await Task.Delay(stdoutFlushDelay);
}
```

Hello komentář části `Code tooprocess data and produce output file(s)` je zástupný symbol pro hello kód, který by za normálních okolností provedl úlohu. Například můžete mít kód, který stahuje data ze služby Azure Storage a provede transformaci nebo výpočet jeho. Hello důležitou součástí tento fragment kódu je ukázka, jak může obtékat takový kód v `using` bloku tooperiodically aktualizovat soubor s [SaveTrackedAsync][net_savetrackedasync].

agent uzlu Hello je program, který běží na každém uzlu ve fondu hello a poskytuje rozhraní příkazu a řízení hello mezi hello uzlu a služba Batch hello. Hello `Task.Delay` volání je vyžadován na konci hello `using` tooensure bloku, která hello uzlu agenta má čas tooflush hello obsah standard se soubor stdout.txt toohello na uzlu hello. Bez této zpoždění je možné toomiss hello poslední několik sekund výstupu. Tato prodleva nemusí být požadovány pro všechny soubory.

> [!NOTE]
> Když povolíte sledování pomocí souboru **SaveTrackedAsync**, pouze *připojí* toohello sledovaných souboru jsou trvalé tooAzure úložiště. Tuto metodu použijte pouze pro sledování-rotaci souborů protokolu nebo jiné soubory, které jsou zapsány toowith připojovat operations toohello konec souboru hello.
> 
> 

## <a name="retrieve-output-data"></a>Načíst výstupní data

Můžete načíst pomocí knihovny Azure Batch souboru konvence hello trvalou výstupu, můžete udělat úlohy a úlohy-zaměřená na způsobem. Můžete požádat, zda text hello výstup pro daný úloh bez nutnosti tooknow cesty do úložiště Azure, nebo i její název souboru. Místo toho můžete požádat výstupní soubory úlohy nebo úlohy ID.

Hello následující fragment kódu iteruje v rámci úlohy, vytiskne některé informace o hello výstupní soubory pro hello úlohu a potom stáhne jeho soubory z úložiště.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (OutputFileReference output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="view-output-files-in-hello-azure-portal"></a>Zobrazení výstupní soubory v hello portálu Azure

Hello portál Azure zobrazí úloh výstupní soubory a protokoly, které jsou trvalé tooa propojení účtu úložiště Azure pomocí hello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). Můžete implementovat tyto konvence sami v hello jazyk podle vašeho výběru nebo hello souboru konvence knihovny můžete použít v aplikacích .NET.

zobrazení hello tooenable souborů výstup hello portálu, musí splňovat následující požadavky hello:

1. [Propojení účtu Azure Storage](#requirement-linked-storage-account) tooyour účtu Batch.
2. Dodržovat konvence pojmenování toohello předdefinované kontejnery úložiště a souborů při zachování výstupy. Definice hello tyto konvence můžete najít v souboru konvence knihovny hello [README][github_file_conventions_readme]. Pokud používáte hello [Azure Batch souboru konvence] [ nuget_package] toopersist knihovny vaší výstup, vaše soubory jsou nastavené jako trvalé podle toohello souboru konvence standardní.

výstup úlohy tooview soubory a protokoly ve hello portálu Azure, přejděte toohello úloh, jejíž výstup vás zajímá, pak klikněte buď **uložená výstupní soubory** nebo **uložené protokoly**. Tento obrázek ukazuje hello **uložená výstupní soubory** pro hello úlohu s ID "007":

![Výstupy úlohy okno v hello portálu Azure][2]

## <a name="code-sample"></a>Ukázka kódu

Hello [PersistOutputs] [ github_persistoutputs] ukázkový projekt je jedním z hello [ukázky kódu Azure Batch] [ github_samples] na Githubu. Toto řešení sady Visual Studio ukazuje, jak toouse hello Azure Batch souboru konvence knihovny toopersist úloh výstup toodurable úložiště. toorun hello ukázkové, postupujte takto:

1. Hello otevřete projekt v **Visual Studio 2015 nebo novější**.
2. Přidat Batch a Storage **přihlašovací údaje účtu** příliš**AccountSettings.settings** v projektu Microsoft.Azure.Batch.Samples.Common hello.
3. **Sestavení** (ale není spuštěný) hello řešení. Pokud se zobrazí výzva, obnovení všech balíčků NuGet.
4. Použití hello Azure portálu tooupload [balíčku aplikace](batch-application-packages.md) pro **PersistOutputsTask**. Zahrnout hello `PersistOutputsTask.exe` a jeho závislá sestavení v hello ZIP balíčku, ID aplikace hello sady příliš "PersistOutputsTask" a aplikace hello verze balíčku příliš "1.0".
5. **Spustit** (spustit) hello **PersistOutputs** projektu.
6. Když výzvami toochoose hello trvalost technologie toouse pro spuštěné hello ukázku, zadejte **1** toorun hello ukázku pomocí výstup úlohy hello souboru konvence knihovny toopersist. 

## <a name="next-steps"></a>Další kroky

### <a name="get-hello-batch-file-conventions-library-for-net"></a>Získat hello dávkového souboru konvence knihovna pro .NET

Hello konvence souboru Batch library pro .NET je k dispozici na [NuGet][nuget_package]. Hello knihovně rozšiřuje hello [CloudJob] [ net_cloudjob] a [CloudTask] [ net_cloudtask] tříd pomocí nové metody. Viz také hello [referenční dokumentace](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) hello souboru konvence knihovny.

Hello [zdrojový kód] [ github_file_conventions] hello konvence soubor knihovny je k dispozici na Githubu v hello Microsoft Azure SDK pro .NET úložiště. 

### <a name="explore-other-approaches-for-persisting-output-data"></a>Prozkoumejte jiné postupy zachování výstupních dat

- V tématu [zachovat úlohy a úkolů výstupní tooAzure úložiště](batch-task-output.md) přehled zachování dat úlohy a úlohy.
- V tématu [zachovat data tooAzure úloh úložiště s hello rozhraní API služby Batch](batch-task-output-files.md) toolearn jak toouse hello rozhraní API služby Batch toopersist výstupní data.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Uložená výstupní soubory a uložené protokoly selektory portálu"
[2]: ./media/batch-task-output/task-output-02.png "Výstupy úlohy okno v hello portálu Azure"
