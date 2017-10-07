---
title: "s více instancemi aaaUse úloh aplikací MPI toorun - Azure Batch | Microsoft Docs"
description: "Zjistěte, jak v Azure Batch zadejte tooexecute aplikací rozhraní MPI (Message Passing) pomocí hello úkol s více instancemi."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: 5/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0e3295a6aeb76267c26d5504bcff59de3dc5e22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-multi-instance-tasks-toorun-message-passing-interface-mpi-applications-in-batch"></a>Použít s více instancemi úlohy toorun rozhraní MPI (Message Passing) aplikace ve službě Batch

Úkoly s více instancemi umožní toorun úloh Azure Batch na několika výpočetních uzlech současně. Tyto úlohy povolit scénáře jako aplikací rozhraní MPI (Message Passing) v dávce s vysokým výkonem. V tomto článku se dozvíte, jak pomocí úkolů s více instancemi tooexecute hello [Batch .NET] [ api_net] knihovny.

> [!NOTE]
> Zatímco hello příklady v tomto článku se zaměřují na Batch .NET, MS-MPI, a Windows výpočetní uzly, jsou hello s více instancemi úloh Principy probírané v tomto poli použít tooother platformy a technologie (Python a Intel MPI na uzlech Linux, např.).
>
>

## <a name="multi-instance-task-overview"></a>Přehled úloh s více instancemi
Ve službě Batch, je obvykle každý úkol spustit na jednom výpočetním uzlu – odeslat úlohu tooa více úloh, a hello služba Batch plánuje každý úkol pro spuštění na uzlu. Ale nakonfigurováním úkolu **s více instancemi nastavení**, řekněte Batch tooinstead vytvořit jednu primární úlohu a několik dílčích úkolů, které jsou poté provedeny ve více uzlech.

![Přehled úloh s více instancemi][1]

Při odesílání úlohy s více instancemi nastavení tooa úlohy Batch provádí úlohy jedinečný toomulti instance několik kroků:

1. Hello služba Batch vytvoří jednu **primární** a několik **dílčí úkoly** na základě nastavení hello s více instancemi. Celkový počet úloh (primární plus všechny dílčí úkoly) Hello odpovídá hello počet **instance** (výpočetní uzly) určíte v nastavení hello s více instancemi.
2. Batch označí jeden hello výpočetní uzly jako hello **hlavní**, a plány hello tooexecute primární úlohy na hlavní server hello. Naplánuje hello tooexecute dílčí úkoly na hello zbytek hello výpočetní uzly přidělené toohello úkol s více instancemi, jeden dílčí úlohy na uzlu.
3. Hello primární a všechny dílčí úkoly stahovat **společné soubory prostředků** určíte v nastavení hello s více instancemi.
4. Po stažení společné soubory prostředků hello hello primární a dílčí úkoly spusťte hello **koordinaci příkaz** určíte v nastavení hello s více instancemi. příkaz koordinaci Hello je obvykle používanými tooprepare uzlů pro provádění úlohy hello. To může zahrnovat spouštění služby na pozadí (například [Microsoft MPI][msmpi_msdn]na `smpd.exe`) a ověření, že jsou hello uzly připravené tooprocess mezi uzly zprávy.
5. hello primární úkol Hello **příkaz aplikace** na hlavní uzel hello *po* příkaz koordinaci hello byla úspěšně dokončena, hello primární a všechny dílčí úkoly. příkaz aplikace Hello je úkol s více instancemi hello samotné hello příkazový řádek a spouští pouze primární úloh hello. V [MS-MPI][msmpi_msdn]-řešení, to je, kde můžete spuštění vaší aplikace s povolenými MPI pomocí `mpiexec.exe`.

> [!NOTE]
> Když je funkčně distinct, hello "úkol s více instancemi" není typu jedinečný úloh jako hello [StartTask] [ net_starttask] nebo [JobPreparationTask] [ net_jobprep]. úkol s více instancemi Hello je jednoduše standardní úlohy Batch ([CloudTask] [ net_task] v Batch .NET) byla nakonfigurována nastavení s více instancemi. V tomto článku, označujeme jako hello toothis **úkol s více instancemi**.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Požadavky pro úkoly s více instancemi
Úkoly s více instancemi vyžadují fond s **komunikaci mezi uzly povolena**a s **provedení souběžné úlohy zakázáno**. provedení souběžné úlohy toodisable, sada hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) too1 vlastnost.

Tento fragment kódu ukazuje, jak toocreate fond pro více instancemi úloh, pomocí knihovny Batch .NET hello.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> Pokud se pokusíte toorun s více instancemi úloha ve fondu s komunikací internodium zakázána, nebo s *maxTasksPerNode* hodnotu větší než 1, hello úloh není vůbec naplánováno – zůstane po neomezenou dobu ve stavu "aktivní" hello. 
>
> Úkoly s více instancemi lze spustit pouze v uzlů ve fondech vytvořený po 14. prosince 2015.
>
>

### <a name="use-a-starttask-tooinstall-mpi"></a>Použít StartTask tooinstall MPI
toorun aplikací MPI s více instancemi úloh, musíte nejdřív tooinstall implementace MPI (MS-MPI nebo MPI Intel, např.) na hello výpočetních uzlů ve fondu hello. Toto je vhodná doba toouse [StartTask][net_starttask], který provede vždy, když se uzel připojí fondu nebo je restartovat. Tento fragment kódu vytvoří StartTask, která určuje instalační balíček hello MS-MPI jako [souboru prostředků][net_resourcefile]. Hello spouštěcí úkol příkazový řádek se spustí po souboru prostředků hello stažené toohello uzlu. V takovém případě hello příkazový řádek provede bezobslužnou instalaci MS MPI.

```csharp
// Create a StartTask for hello pool which we use for installing MS-MPI on
// hello nodes as they join hello pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit hello fully configured pool toohello Batch service tooactually create
// hello pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Vzdálený přímý přístup do paměti (vzdáleného počítače RDMA)
Pokud vyberete [RDMA podporovat velikost](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , jako je A9 pro hello výpočetní uzly ve fondu Batch, MPI aplikace mohou využít výhod Azure vysoce výkonné, nízkou latencí paměti vzdálený přímý přístup do (počítače RDMA) sítě.

Podívejte se na velikosti hello definované jako "Podporující RDMA" v hello následující články:

* **CloudServiceConfiguration** fondy

  * [Velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md) (jenom Windows)
* **VirtualMachineConfiguration** fondy

  * [Velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Velikosti virtuálních počítačů v Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> tootake výhod RDMA na [Linuxových výpočetních uzlů](batch-linux-nodes.md), je nutné použít **Intel MPI** na uzlech hello. Další informace o CloudServiceConfiguration a VirtualMachineConfiguration fondy najdete v tématu hello fondu části hello [přehled funkcí Batch](batch-api-basics.md).
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Vytvoření úlohy s více instancemi pomocí rozhraní Batch .NET
Teď, když jsme si zahrnutých hello fondu požadavky a instalace balíčku MPI, vytvoříme hello úkol s více instancemi. V tento fragment kódu, vytvoříme standard [CloudTask][net_task], nakonfigurujte její [MultiInstanceSettings] [ net_multiinstance_prop] vlastnost. Jak už bylo zmíněno dříve, není úkol s více instancemi hello typu distinct úloh, ale standardní úlohy Batch nakonfigurované s nastavením s více instancemi.

```csharp
// Create hello multi-instance task. Its command line is hello "application command"
// and will be executed *only* by hello primary, and only after hello primary and
// subtasks execute hello CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure hello task's MultiInstanceSettings. hello CoordinationCommandLine will be executed by
// hello primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit hello task toohello job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on hello nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primární úlohy a dílčí úkoly
Když vytvoříte hello s více instancemi nastavení pro úlohu, zadáte hello počet výpočetních uzlů, které jsou tooexecute hello úloh. Při odesílání úlohy tooa úloh hello hello služba Batch vytvoří jednu **primární** úlohy a dostatek **dílčí úkoly** společně odpovídající hello počet uzlů, které jste zadali.

Tyto úlohy jsou přiřazeny id celé číslo v rozsahu 0 hello příliš*numberOfInstances* - 1. Hello úlohy s id 0 je hello primární úlohy a dílčí úkoly jsou všechny ostatní ID. Například pokud vytvoříte hello následující nastavení s více instancemi pro úlohu, hello primární úloh má id 0 a dílčí úkoly hello by měla mít ID 1 až 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Hlavní uzel
Při odesílání úlohy s více instancemi hello služba Batch označí jeden hello výpočetní uzly jako uzel "hlavní" hello, a plány hello tooexecute primární úlohy na hlavní uzel hello. Hello dílčí úkoly jsou naplánované tooexecute na hello zbytek hello uzlů přidělených toohello úkol s více instancemi.

## <a name="coordination-command"></a>Příkaz spolupráce
Hello **koordinaci příkaz** provedený hello primární i dílčí úkoly.

blokuje Hello vyvolání příkazu koordinaci hello – Batch příkaz aplikace hello nespustí, dokud je úspěšně vrácený příkaz hello spolupráce pro všechny dílčí úkoly. příkaz koordinaci Hello by proto spustit žádné služby, požadované pozadí, ověřte, zda je připravený k použití a poté ukončete. Například tento příkaz koordinaci pro řešení pomocí MS-MPI verze 7 spustí službu SMPD hello na hello uzel, a poté bude ukončen:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Všimněte si použití hello `start` v tomto příkazu spolupráce. Toto je nutné kvůli hello `smpd.exe` aplikace nevrátí okamžitě po spuštění. Bez použití hello hello [spustit] [ cmd_start] příkaz, tento příkaz koordinaci by vrátit a by proto blokovat příkaz aplikace hello spuštění.

## <a name="application-command"></a>Příkaz aplikace
Jakmile hello primární úlohy a všechny dílčí úkoly dokončení provádění příkazu koordinaci hello, úkolů s více instancemi hello příkazový řádek se spustí primární úlohou hello *pouze*. Říkáme tento hello **příkaz aplikace** toodistinguish z příkazu koordinaci hello.

U aplikací, MS-MPI, použijte hello aplikace příkaz tooexecute vaší aplikace s povolenými MPI s `mpiexec.exe`. Zde je ukázka, příkaz aplikace pro řešení pomocí MS-MPI verze 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> Protože MS-MPI `mpiexec.exe` hello používá `CCP_NODES` proměnné ve výchozím nastavení (najdete v části [proměnné prostředí](#environment-variables)) příklad hello aplikace příkazového řádku výše vyloučí ho.
>
>

## <a name="environment-variables"></a>Proměnné prostředí
Batch vytvoří několik [proměnné prostředí] [ msdn_env_var] konkrétní instanci toomulti úlohy na hello výpočetní uzly přiděleny tooa úkol s více instancemi. Příkazové řádky koordinace a aplikace můžete odkazovat těchto proměnných prostředí, jak můžete hello skripty a programů, které se provést.

Hello následující proměnné prostředí jsou vytvořené pomocí služby Batch hello pro použití úkolů s více instancemi:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Úplné podrobnosti o těchto a hello jiné Batch výpočetní uzel proměnné prostředí, včetně jejich obsah a viditelnost, najdete v části [výpočetní uzel proměnné prostředí][msdn_env_var].

> [!TIP]
> Ukázka kódu Hello Batch Linux MPI obsahuje příklad některé z těchto proměnných prostředí použití. Hello [koordinaci cmd] [ coord_cmd_example] Bash skript stáhne běžné aplikace a vstupní soubory ze služby Azure Storage, sdílené složky systému souborů NFS v hello hlavní uzel povolí a nakonfiguruje hello jiné uzly úkol s více instancemi toohello přidělené jako klienti NFS.
>
>

## <a name="resource-files"></a>Soubory prostředků
Existují dvě sady tooconsider soubory prostředků pro úkoly s více instancemi: **společné soubory prostředků** , *všechny* úkoly stahují (obě primární a dílčí úkoly) a hello **soubory prostředků** zadaná pro hello s více instancemi úkolů sebe, což *pouze hello primární* úkolů stahování.

Můžete určit jeden nebo více **společné soubory prostředků** hello s více instancemi nastavení pro úlohu. Tyto společné soubory prostředků se stahují z [Azure Storage](../storage/common/storage-introduction.md) do každého uzlu **sdíleného adresáře úkolu** hello primární a všechny dílčí úkoly. Dostanete hello úloh sdíleného adresáře z aplikace a koordinaci příkazové řádky pomocí hello `AZ_BATCH_TASK_SHARED_DIR` proměnné prostředí. Hello `AZ_BATCH_TASK_SHARED_DIR` cesta je stejná na každý úkol s více instancemi přidělené toohello uzlu, takže můžete sdílet příkaz jeden spolupráce mezi hello primární a všechny dílčí úkoly. Batch "nesdílí" hello adresáře v tom smyslu vzdáleného přístupu, ale můžete ho použít jako přípojný nebo sdílení bodu, jak je uvedeno výše v hello tip na proměnné prostředí.

Soubory prostředků, které zadáte pro hello úkol s více instancemi samotné jsou pracovní adresář pro stažené toohello úkolů, `AZ_BATCH_TASK_WORKING_DIR`, ve výchozím nastavení. Jak je uvedeno, na rozdíl od toocommon soubory prostředků, pouze hello primární úkol stáhne soubory prostředků, zadaný pro vlastní úloha hello s více instancemi.

> [!IMPORTANT]
> Vždy použít proměnné prostředí hello `AZ_BATCH_TASK_SHARED_DIR` a `AZ_BATCH_TASK_WORKING_DIR` toorefer toothese adresářů v příkazové řádky. Ručně nepokoušejte tooconstruct hello cesty.
>
>

## <a name="task-lifetime"></a>Doba platnosti úloh
Doba platnosti Hello životnosti hello primární úloh Ovládací prvky hello hello celý s více instancemi úlohy. Při ukončení hello primární, všechny dílčí úkoly hello ukončena. ukončovací kód Hello hello primární je hello ukončovací kód hello úkolů a je proto použité toodetermine hello úspěch nebo neúspěch hello úlohy pro účely opakování.

Pokud žádný z dílčích úkolů hello selže, ukončení s návratovým kódem nulová, například hello celý s více instancemi úloha nezdaří. úkol s více instancemi Hello je pak byla ukončena a opakovat, až tooits limit opakování.

Při odstranění úlohu s více instancemi podle hello služba Batch odstranit také hello primární a všechny dílčí úkoly. Všechny dílčí úloha adresáře a jejich soubory se odstraní z hello výpočetních uzlů, stejně jako u standardní úlohy.

[TaskConstraints] [ net_taskconstraints] pro úlohu s více instancemi, třeba hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], a [RetentionTime] [ net_taskconstraint_retention] vlastnosti, se neuplatňují, jak jsou pro standardní úlohy a použít toohello primární server a všechny dílčí úkoly. Nicméně pokud změníte hello [RetentionTime] [ net_taskconstraint_retention] vlastnost po přidání hello s více instancemi úloh toohello úlohy, tato změna je použité pouze toohello primární úloh. Všechny dílčí úkoly hello pokračovat původní hello toouse [RetentionTime][net_taskconstraint_retention].

Seznam posledních úkolů výpočetním uzlu odráží hello id dílčí úlohy, pokud poslední úloha hello byla součástí úlohu s více instancemi.

## <a name="obtain-information-about-subtasks"></a>Získání informací o dílčí úkoly
informace o tooobtain na dílčí úkoly pomocí knihovny Batch .NET hello, volání hello [CloudTask.ListSubtasks] [ net_task_listsubtasks] metoda. Tato metoda vrátí informace na všechny dílčí úkoly a informace o hello výpočetního uzlu, který spuštěné úlohy hello. Z těchto informací můžete určit každý dílčí úlohy kořenový adresář, hello id fondu, jeho aktuální stav, ukončovacího kódu a další. Tyto informace můžete použít v kombinaci s hello [PoolOperations.GetNodeFile] [ poolops_getnodefile] metoda tooobtain hello dílčí úlohy na soubory. Všimněte si, že tato metoda nevrátí informace pro primární úlohu hello (id 0).

> [!NOTE]
> Pokud není uvedeno jinak, metody rozhraní Batch .NET, které působí na hello s více instancemi [CloudTask] [ net_task] samotné použít *pouze* toohello primární úloh. Například při volání hello [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metodu úlohu s více instancemi, jsou vráceny pouze soubory hello primární úkolu.
>
>

Hello následující fragment kódu ukazuje, jak tooobtain dílčí úloha informace, a také žádosti o obsah souboru z hello uzlů, ve kterých provést.

```csharp
// Obtain hello job and hello multi-instance task from hello Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain hello list of subtasks for hello task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over hello subtasks and print their stdout and stderr
// output if hello subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Ukázka kódu
Hello [MultiInstanceTasks] [ github_mpi] ukázka kódu na Githubu ukazuje, jak toouse s více instancemi úkolů toorun [MS-MPI] [ msmpi_msdn] aplikace na výpočetních uzlech Batch. Postupujte podle kroků hello v [přípravy](#preparation) a [provádění](#execution) toorun hello ukázka.

### <a name="preparation"></a>Příprava
1. Postupujte podle hello první dva kroky v [jak toocompile a spusťte jednoduchý program MS-MPI][msmpi_howto]. Splňuje to, že hello prerequesites pro hello následující krok.
2. Sestavení *verze* verzi hello [MPIHelloWorld] [ helloworld_proj] ukázka MPI programu. Toto je hello program, který se má spustit na výpočetních uzlech úkol s více instancemi hello.
3. Vytvořte soubor zip obsahující `MPIHelloWorld.exe` (které je vytvořené v kroku 2) a `MSMpiSetup.exe` (který jste stáhli krok 1). Tento soubor zip budete nahrát jako balíček aplikace v dalším kroku hello.
4. Použití hello [portál Azure] [ portal] toocreate dávky [aplikace](batch-application-packages.md) nazývá "MPIHelloWorld" a zadejte soubor zip hello jste vytvořili v předchozím kroku hello jako verze "1.0" balíček aplikace Hello. V tématu [odesílat a spravovat aplikace](batch-application-packages.md#upload-and-manage-applications) Další informace.

> [!TIP]
> Sestavení *verze* verzi `MPIHelloWorld.exe` tak, že nemáte tooinclude žádné další závislosti (například `msvcp140d.dll` nebo `vcruntime140d.dll`) v balíčku aplikace.
>
>

### <a name="execution"></a>Provádění
1. Stáhnout hello [azure-batch-samples] [ github_samples_zip] z Githubu.
2. Otevřete hello MultiInstanceTasks **řešení** v sadě Visual Studio 2015 nebo novější. Hello `MultiInstanceTasks.sln` řešení soubor je umístěný ve:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. Zadejte přihlašovací údaje účtu Batch a úložiště v `AccountSettings.settings` v hello **Microsoft.Azure.Batch.Samples.Common** projektu.
4. **Sestavení a spuštění** hello MultiInstanceTasks řešení tooexecute hello MPI ukázkovou aplikaci na výpočetních uzlů ve fondu služby Batch.
5. *Volitelné*: použití hello [portál Azure] [ portal] nebo hello [Batch Explorer] [ batch_explorer] tooexamine hello ukázka fondu, úlohy, a úloha ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") před odstranit hello prostředky.

> [!TIP]
> Můžete si stáhnout [Visual Studio Community] [ visual_studio] zdarma, pokud nemáte Visual Studio.
>
>

Výstup z `MultiInstanceTasks.exe` je podobné toohello následující:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] toojob [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks toocomplete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a>Další kroky
* Popisuje Hello Microsoft HPC & Azure Batch Team blog [MPI podporu pro systém Linux na Azure Batch][blog_mpi_linux]a obsahuje informace o používání [OpenFOAM] [ openfoam] pomocí služby Batch. Můžete najít ukázky kódu Pythonu pro hello [příklad OpenFOAM na Githubu][github_mpi].
* Zjistěte, jak příliš[vytvořit fondy Linuxových výpočetních uzlů](batch-linux-nodes.md) pro použití v Azure Batch MPI řešení.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Přehled s více instancemi"
