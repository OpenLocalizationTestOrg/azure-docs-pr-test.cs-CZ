---
title: "aaaPersist úloh a úkolů výstup tooAzure úložiště s hello rozhraní API služby Azure Batch | Microsoft Docs"
description: "Zjistěte, jak toouse rozhraní API služby Batch toopersist dávkové úlohy a úlohy výstupní tooAzure úložiště."
services: batch
author: tamram
manager: timlt
editor: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.openlocfilehash: 71b3f7c0dda2d2a9d8eb3eef83229873c70ca22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="persist-task-data-tooazure-storage-with-hello-batch-service-api"></a>Zachovat data tooAzure úloh úložiště s hello rozhraní API služby Batch

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Počínaje verzí 2017-05-01, hello rozhraní API služby Batch podporuje zachování výstupní data tooAzure úložiště pro úlohy a úkolech Správce úloh, které běží na fondy s hello konfigurace virtuálního počítače. Když přidáte úlohu, můžete ve službě Azure Storage kontejner jako hello cíl pro výstup hello úlohy. Služba Batch Hello pak zapíše kontejner toothat žádné výstup dat po dokončení úloh hello.

Využít toousing hello Batch výstup úlohy toopersist rozhraní API služby se nemusí toomodify hello aplikace, která hello úloha je spuštěna. Místo toho se několik jednoduchých úprav tooyour klientskou aplikaci, můžete zachovat výstup hello úloh v rámci hello kód, který vytvoří hello úloh.   

## <a name="when-do-i-use-hello-batch-service-api-toopersist-task-output"></a>Kdy použít výstup úlohy toopersist hello rozhraní API služby Batch?

Azure Batch poskytuje více než jedním ze způsobů toopersist výstup úlohy. Pomocí hello rozhraní API služby Batch je vhodné přístup, který je nejlepší vhodná toothese scénáře:

- Výstup úlohy toopersist toowrite kód z chcete v rámci klientské aplikace beze změny aplikace hello spuštěná vaše úloha.
- Chcete toopersist výstup z úlohy Batch a úkolech Správce úloh ve fondech vytvořené pomocí hello konfigurace virtuálního počítače.
- Chcete toopersist výstup tooan Azure Storage kontejner s libovolný název.
- Chcete-li toopersist výstup tooan Azure Storage kontejner s názvem podle toohello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

Pokud váš scénář se liší od výše uvedené, může být nutné tooconsider jiný přístup. Například rozhraní API služby Batch hello nepodporuje aktuálně streamování výstup tooAzure úložiště je spuštěna úloha hello. toostream výstup, zvažte použití hello dávkového souboru konvence knihovny, k dispozici pro rozhraní .NET. Pro jiné jazyky budete potřebovat tooimplement vlastní řešení. Další informace o dalších možnostech pro zachování výstup úlohy najdete v tématu [zachovat úlohy a úkolů výstupní tooAzure úložiště](batch-task-output.md). 

## <a name="create-a-container-in-azure-storage"></a>Vytvořit kontejner ve službě Azure Storage

Úloha toopersist výstup tooAzure úložiště, budete potřebovat toocreate kontejner, který slouží jako hello cíl pro výstupní soubory. Vytvoření kontejneru hello před spuštěním úkolu, pokud možno před odesláním úlohu. toocreate hello kontejner, použijte hello odpovídající Azure Klientská knihovna pro úložiště nebo sady SDK. Další informace o rozhraní API úložiště Azure najdete v tématu hello [dokumentaci pro Azure Storage](https://docs.microsoft.com/azure/storage/).

Například pokud píšete vaší aplikace v jazyce C#, použijte hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Následující příklad ukazuje, jak Hello toocreate kontejner:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await conainer.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-hello-container"></a>Získání sdíleného přístupového podpisu kontejneru hello

Po vytvoření kontejneru hello získáte sdílený přístupový podpis (SAS), s kontejnerem toohello přístup pro zápis. SAS poskytuje Delegovaný přístup toohello kontejneru. Hello SAS uděluje přístup pomocí zadané sady oprávnění a v zadaném časovém intervalu. Hello služba Batch musí SAS s zápisu oprávnění toowrite úloh výstup toohello kontejneru. Další informace o tokenu SAS naleznete v tématu [pomocí sdílené přístupové podpisy \(SAS\) ve službě Azure Storage](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Když získáte SAS pomocí hello rozhraní API úložiště Azure, hello rozhraní API vrátí řetězec tokenu SAS. Tento token řetězec zahrnuje všechny parametry hello SAS, včetně hello oprávnění a hello interval, přes které hello SAS platný. toouse hello SAS tooaccess kontejner ve službě Azure Storage, musíte tooappend hello SAS token řetězec toohello identifikátor URI. Hello identifikátor URI, společně s hello připojí SAS token, poskytuje ověřený přístup tooAzure úložiště.

Hello následující příklad ukazuje, jak tooget jen pro zápis SAS token řetězec pro hello kontejner, a poté připojí hello SAS toohello kontejneru URI:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken; 
```

## <a name="specify-output-files-for-task-output"></a>Zadejte výstupní soubory pro výstup úlohy

výstupní soubory toospecify pro úlohy, vytvořit kolekci [výstupní soubor](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile) objekty a přiřaďte ho toohello [CloudTask.OutputFiles](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) vlastnost při vytvoření úlohy hello. 

Hello následující příklad kódu .NET vytvoří úlohu, která zapisuje náhodná čísla tooa soubor s názvem `output.txt`. Příklad Hello vytvoří výstupní soubor pro `output.txt` toobe zapsána toohello kontejneru. Příklad Hello také vytvoří výstupní soubory pro všechny soubory protokolu, které odpovídají vzor souborů hello `std*.txt` (_například_, `stdout.txt` a `stderr.txt`). URL adresa kontejneru Hello vyžaduje hello SAS, který byl vytvořen dříve hello kontejneru. Hello služba Batch používá hello SAS tooauthenticate přístup toohello kontejneru: 

```csharp
new CloudTask(taskId, "cmd /v:ON /c \"echo off && set && (FOR /L %i IN (1,1,100000) DO (ECHO !RANDOM!)) > output.txt\"")
{
    OutputFiles = new List<OutputFile>
    {
        new OutputFile(
            filePattern: @"..\std*.txt",
            destination: new OutputFileDestination(
         new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId)),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
        new OutputFile(
            filePattern: @"output.txt",
            destination: 
         new OutputFileDestination(new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId + @"\output.txt")),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
}
```

### <a name="specify-a-file-pattern-for-matching"></a>Zadejte odpovídající vzor souboru

Když zadáte výstupního souboru, můžete použít hello [OutputFile.FilePattern](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) toospecify vlastnost odpovídající vzor souboru. vzor souboru Hello může odpovídat nulové soubory, jeden soubor nebo u sady souborů, které jsou vytvořené úlohou hello.

Hello **FilePattern** vlastnost podporuje zástupné znaky standardní systém souborů, jako `*` (pro tohoto nerekurzivního odpovídá) a `**` (pro rekurzivní odpovídá). Například ukázka kódu hello výše určuje hello souboru vzor toomatch `std*.txt` bez rekurzivně: 

`filePattern: @"..\std*.txt"`

tooupload jeden soubor, zadejte vzor souborů s žádné zástupné znaky. Například ukázka kódu hello výše určuje hello souboru vzor toomatch `output.txt`:

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Zadejte podmínku nahrávání

Hello [OutputFileUploadOptions.UploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) vlastnost umožňuje podmíněného odesílání výstupní soubory. Obvyklým scénářem je tooupload jednu sadu souborů, pokud úloha hello úspěšná a jinou sadu souborů, pokud se nezdaří. Můžete například souborů podrobného protokolování tooupload jenom v případě, že hello úloh selže a ukončí se nenulový ukončovací kód. Podobně můžete tooupload výsledek soubory pouze v případě, že úloha hello úspěšná, jak tyto soubory mohou být chybí nebo jsou neúplné v případě selhání úlohy hello.

Hello ukázka kódu výše nastaví hello **UploadCondition** vlastnost příliš**TaskCompletion**. Toto nastavení určuje, že soubor hello je toobe nahrán po dokončení úlohy hello, bez ohledu na hodnotu hello hello ukončovací kód. 

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Další nastavení, najdete v části hello [OutputFileUploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) výčtu.

### <a name="disambiguate-files-with-hello-same-name"></a>Odstranit nejednoznačnost soubory s hello stejným názvem

vytvářet soubory, které mají hello Hello úkoly v úloze stejný název. Například `stdout.txt` a `stderr.txt` jsou vytvořeny pro každý úkol, který běží v rámci úlohy. Protože každá úloha se spustí v jeho vlastní kontextu, není v systému souborů hello uzlu konfliktu tyto soubory. Ale při nahrávání souborů z více úloh tooa sdílené kontejneru, budete potřebovat toodisambiguate soubory s hello stejný název.

Hello [OutputFileBlobContainerDestination.Path](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) vlastnost určuje cílový objekt blob hello nebo virtuální adresář pro výstupní soubory. Můžete použít hello **cesta** vlastnost tooname hello blob nebo virtuální adresář tak, že výstupní soubory se stejným názvem jsou jedinečně pojmenované ve službě Azure Storage hello. Pomocí ID úkolu hello v cestě hello je dobře jedinečné názvy tooensure a snadno identifikovat soubory.

Pokud hello **FilePattern** je nastavena tooa výraz se zástupnými znaky, pak všechny soubory, které odpovídají hello vzor jsou nahrané toohello virtuální adresář zadaný hello **cesta** vlastnost. Například pokud hello kontejneru je `mycontainer`, hello úloh ID je `mytask`, a je vzor souborů hello `..\std*.txt`, pak hello absolutní identifikátory URI toohello výstupní soubory ve službě Azure Storage bude vypadat podobně jako:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

Pokud hello **FilePattern** vlastnost sady toomatch jeden název souboru, což znamená, neobsahuje žádné zástupné znaky, pak hello hodnotu hello **cesta** vlastnost určuje název objektu blob plně kvalifikovaný hello . Pokud očekáváte, že ke konfliktu názvů s jeden soubor z více úloh, zadejte hello název virtuálního adresáře hello jako součást toodisambiguate název souboru hello těchto souborů. Například sada hello **cesta** ID vlastnosti tooinclude hello úloh, hello oddělovací znak (obvykle dopředné lomítko) a název souboru hello:

`path: taskId + @"/output.txt"`

Hello absolutní identifikátory URI toohello výstupní soubory pro sadu úloh bude vypadat podobně jako:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

Další informace o virtuálních adresářů v Azure Storage najdete v tématu [seznamu hello objektů BLOB v kontejneru](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container).


## <a name="diagnose-file-upload-errors"></a>Diagnostikovat chyby odesílání souborů

Pokud odesílání výstupní soubory tooAzure úložiště selže, pak hello úloh přesune toohello **dokončeno** stavu a hello [TaskExecutionInformation.FailureInformation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) je nastavena. Zkontrolujte hello **FailureInformation** toodetermine vlastnost co došlo k chybě. Zde je ukázka, k chybě, která proběhne nahrávání souborů, pokud nelze nalézt kontejner hello: 

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of hello specified Azure container(s) was not found while attempting tooupload an output file
```

Na každých nahrávání souborů Batch zapíše dva protokolu soubory toohello výpočetním uzlu, `fileuploadout.txt` a `fileuploaderr.txt`. Můžete zkontrolovat tyto soubory protokolu toolearn Další informace o konkrétní chyby. V případech, kde hello nahrávání souborů byla nikdy pokus, např. protože vlastní úloha hello nebylo možné spustit pak tyto soubory protokolu neexistují.

## <a name="diagnose-file-upload-performance"></a>Diagnostika výkon nahrávání souborů

Hello `fileuploadout.txt` protokoly průběhu odesílání souboru. Tento soubor toolearn, které další informace o tom, jak dlouho nahrávání souboru trvá můžete zkontrolovat. Mějte na paměti, že existuje celá řada přispívajících faktorů tooupload výkonu, včetně velikosti hello hello uzlu, dalších aktivit na hello uzlu v době hello hello nahrávání, zda text hello cílový kontejner je v hello jsou stejné oblasti jako hello fondu Batch, kolik uzly odesílání toohello účet úložiště v hello stejný čas a tak dále.

## <a name="use-hello-batch-service-api-with-hello-batch-file-conventions-standard"></a>Pomocí rozhraní API služby Batch hello hello dávkového souboru konvence standardní

Můžete zachovat výstup úlohy s hello rozhraní API služby Batch, můžete pojmenovat cílový kontejner a objekty BLOB, ale chcete. Můžete také tooname je podle toohello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). standardní konvence souboru Hello určuje hello názvy hello cílový kontejner a objektů blob v Azure Storage pro danou výstupní soubor založený na názvy hello hello úlohy a úlohy. Pokud používáte hello standardní soubor konvence pro pojmenování výstupní soubory, pak výstupní soubory jsou k dispozici pro zobrazení v hello [portál Azure](https://portal.azure.com).

Pokud vyvíjíte v jazyce C#, můžete použít metody hello součástí hello [konvence souboru Batch library pro .NET](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files). V této knihovně vytvoří hello správně s názvem kontejnerů a objektů blob cesty pro vás. Například můžete volat hello rozhraní API tooget hello správný název kontejneru hello, na základě názvu úlohy hello:

```csharp
string containerName = job.OutputStorageContainerName();
```

Můžete použít hello [CloudJobExtensions.GetOutputStorageContainerUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) metoda tooreturn adresu URL sdílený přístupový podpis (SAS), která je použité toowrite toohello kontejner. Potom můžete předat tuto SAS toohello [OutputFileBlobContainerDestination](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) konstruktor.

Pokud vyvíjíte v jiném jazyce než v C#, je nutné tooimplement hello souboru konvence standard sami.

## <a name="code-sample"></a>Ukázka kódu

Hello [PersistOutputs] [ github_persistoutputs] ukázkový projekt je jedním z hello [ukázky kódu Azure Batch] [ github_samples] na Githubu. Toto řešení sady Visual Studio ukazuje, jak toouse hello Batch Klientská knihovna pro .NET toopersist úloh výstup toodurable úložiště. toorun hello ukázkové, postupujte takto:

1. Hello otevřete projekt v **Visual Studio 2015 nebo novější**.
2. Přidat Batch a Storage **přihlašovací údaje účtu** příliš**AccountSettings.settings** v projektu Microsoft.Azure.Batch.Samples.Common hello.
3. **Sestavení** (ale není spuštěný) hello řešení. Pokud se zobrazí výzva, obnovení všech balíčků NuGet.
4. Použití hello Azure portálu tooupload [balíčku aplikace](batch-application-packages.md) pro **PersistOutputsTask**. Zahrnout hello `PersistOutputsTask.exe` a jeho závislá sestavení v hello ZIP balíčku, ID aplikace hello sady příliš "PersistOutputsTask" a aplikace hello verze balíčku příliš "1.0".
5. **Spustit** (spustit) hello **PersistOutputs** projektu.
6. Když výzvami toochoose hello trvalost technologie toouse pro spuštěné hello ukázku, zadejte **2** toorun hello ukázku pomocí výstup úlohy toopersist hello rozhraní API služby Batch.
7. V případě potřeby hello ukázku spustit znovu, zadávání **3** toopersist výstup s hello rozhraní API služby Batch a také tooname hello cílovou kontejnerů a objektů blob cestu, podle toohello souboru konvence standardní.

## <a name="next-steps"></a>Další kroky

- Další informace o zachování výstup úlohy s knihovnou hello souboru konvence pro rozhraní .NET najdete v tématu [zachovat úloh a úkolů tooAzure dat úložiště s knihovnou hello konvence souboru Batch pro .NET toopersist ](batch-task-output-file-conventions.md).
- Informace o dalších přístupy k zachování výstupní data ve službě Azure Batch, najdete v části [zachovat úlohy a úkolů výstupní tooAzure úložiště](batch-task-output.md).

[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
