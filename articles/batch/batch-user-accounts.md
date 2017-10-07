---
title: "aaaRun úlohy v části uživatelské účty ve službě Azure Batch | Microsoft Docs"
description: "Konfigurace uživatelských účtů pro spuštění úloh v Azure Batch"
services: batch
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.openlocfilehash: 13d7d76451d89a3cca090c4ef24ed0ed781bbf09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-tasks-under-user-accounts-in-batch"></a>Spuštění úlohy v části uživatelské účty ve službě Batch

Úloha v Azure Batch je vždy spuštěna pod uživatelským účtem. Ve výchozím nastavení úlohy spouštěny pod standardní uživatelské účty, bez oprávnění správce. Tato výchozí nastavení účtu uživatele je obvykle dostatečné. Pro určité scénáře ale je užitečné toobe možné tooconfigure hello uživatelský účet, pod kterým chcete toorun úloh. Tento článek popisuje hello typy uživatelských účtů a způsob jejich konfigurace pro váš scénář.

## <a name="types-of-user-accounts"></a>Typy uživatelských účtů

Azure Batch poskytuje dva typy uživatelských účtů pro spuštění úlohy:

- **Auto uživatelské účty.** Auto uživatelské účty jsou předdefinované uživatelské účty, které automaticky vytvářejí hello služby Batch. Ve výchozím nastavení úlohy spouštěny pod účtem uživatele automaticky. Můžete nakonfigurovat hello uživatele automaticky specifikace pro úlohy tooindicate, které automaticky uživatele by měl účet úloha běžet. specifikace uživatele automaticky Hello vám umožní toospecify hello zvýšení úrovně a obor hello auto uživatelského účtu, který se spustí úloha hello. 

- **Pojmenované uživatelský účet.** Pokud vytvoříte fond hello můžete určit jeden nebo více účtů pojmenovanému uživateli pro fond. Na každém uzlu hello fondu se vytvoří každý uživatelský účet. Kromě toho toohello název účtu, zadejte heslo uživatelského účtu hello, zvýšení úrovně a pro Linux fondy, privátní klíč SSH hello. Když přidáte úlohu, můžete zadat hello s názvem uživatelský účet, pod kterou by měla spouštět tuto úlohu.

> [!IMPORTANT] 
> verze služby Batch Hello 2017-01-01.4.0 zavádí narušující změně, která je nutné aktualizovat vaše toocall kód této verze. Pokud se migrace kódu ze starší verze služby Batch, Všimněte si, že hello **runElevated** vlastnost již není podporována v knihovny klienta REST API nebo Batch hello. Použití hello nové **userIdentity** vlastnost úloh toospecify zvýšení úrovně. Hello části s názvem [aktualizovat knihovnu kódu toohello nejnovější Batch klienta](#update-your-code-to-the-latest-batch-client-library) pro rychlé pokyny pro aktualizaci kódu dávky, pokud použijete jednu z knihoven klienta hello.
>
>

> [!NOTE] 
> uživatelské účty Hello popsané v tomto článku nepodporují protokol RDP (Remote Desktop) nebo Secure Shell (SSH), z bezpečnostních důvodů. 
>
> tooconnect tooa uzlu spuštěný hello Linux konfigurace virtuálního počítače pomocí protokolu SSH, najdete v části [pomocí vzdálené plochy tooa virtuálního počítače s Linuxem v Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md). tooconnect toonodes systémem Windows pomocí protokolu RDP, najdete v části [připojit tooa virtuálního počítače Windows serveru](../virtual-machines/windows/connect-logon.md).<br /><br />
> tooconnect tooa spuštěné hello cloudové služby konfigurace uzlu prostřednictvím protokolu RDP, najdete v části [povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).
>
>

## <a name="user-account-access-toofiles-and-directories"></a>Toofiles přístup k účtu uživatele a adresářů

Auto uživatelský účet a s názvem uživatelského účtu mají pracovní adresář pro čtení a zápis přístup toohello úkolu, sdíleného adresáře a adresář úlohy s více instancemi. Oba typy účtů mají přístup pro čtení toohello spuštění a úlohy přípravy adresáře.

Pokud se úloha spouští pod hello stejný účet, který byl použit pro spuštění spouštěcího úkolu, hello úloh má přístup pro čtení a zápis toohello spuštění úloh adresář. Podobně, pokud úlohy běží pod text hello stejný účet, který byl použit pro spuštění úkol přípravy úlohy, hello úloh má adresáře úkolu přípravy úlohy toohello přístup pro čtení a zápis. Pokud úloha běží pod jiným účtem než hello spouštěcí úkol nebo úkol přípravy úlohy, hello úloh má pouze přístup pro čtení toohello příslušných adresáři.

Další informace o přístup k souborů a adresářů z úlohy najdete v tématu [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md#files-and-directories).

## <a name="elevated-access-for-tasks"></a>Přístup se zvýšeným oprávněním pro úlohy 

zvýšení úrovně Hello uživatelský účet určuje, zda úloha běží se zvýšenými oprávněními přístup. Auto uživatelský účet a s názvem uživatelského účtu můžete spustit s oprávněním vyšší úrovně přístupu. pro zvýšení úrovně Hello dvě možnosti jsou:

- **NonAdmin:** hello úloha běží jako standardní uživatel bez přístup se zvýšeným oprávněním. Hello výchozí úroveň zvýšení oprávnění pro uživatelský účet Batch je vždy **NonAdmin**.
- **Správce:** hello úloha běží jako uživatel s oprávněním vyšší úrovně přístupu a funguje s úplnými oprávněními správce. 

## <a name="auto-user-accounts"></a>Auto uživatelské účty

Ve výchozím nastavení úlohy spuštěny v dávce v rámci automatického uživatelský účet, jako standardní uživatel bez přístup se zvýšeným oprávněním a s oborem úloh. V případě specifikace hello uživatele automaticky nastavena pro obor úloh, služba Batch hello vytvoří automaticky uživatelský účet pro tuto úlohu jenom.

rozsah alternativní tootask Hello je fond oboru. Když hello uživatele automaticky specifikace pro úlohy je nakonfigurován pro fond oboru, hello úloha spuštěna automaticky uživatelský účet, který je k dispozici tooany úloh ve fondu hello. Další informace o rozsahu fondu najdete v tématu hello části s názvem [spuštění úlohy, jako hello automaticky uživatelem s oborem fondu](#run-a-task-as-the-autouser-with-pool-scope).   

výchozí obor Hello se liší v uzlech systému Windows a Linux:

- Na uzlech Windows úlohy spuštěny v rámci oboru úloha ve výchozím nastavení.
- Uzly Linux vždy spuštěný fondu oboru.

Existují čtyři možné konfigurace pro uživatele automaticky specifikace hello, z nichž každý odpovídá tooa jedinečný auto uživatelský účet:

- Přístup bez oprávnění správce s oborem úloh (hello výchozí uživatele automaticky specification)
- Přístup správce (zvýšenými) s oborem úloh
- Přístup bez oprávnění správce s oborem fondu
- Přístup správce k oboru fondu

> [!IMPORTANT] 
> Úkoly spuštěné v rámci oboru úloh nemají fakticky přístup tooother úlohy na uzlu. Ale uživatel se zlými úmysly se účet pro přístup k toohello může toto omezení obejít tak, že zadáte úlohu, která spustí s oprávněními správce a přistupuje k jiných adresářích úkolu. Uživatel se zlými úmysly může také pomocí protokolu RDP nebo SSH tooconnect tooa uzlu. Je důležité tooprotect přístup tooyour dávkového účtu klíče tooprevent takové situaci. Pokud se domníváte, že váš účet ohrožený, být jisti tooregenerate klíče.
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a>Spustit úlohu jako auto uživatel s oprávněním vyšší úrovně přístupu

Pokud budete potřebovat toorun úloha s přístup se zvýšeným oprávněním, můžete nakonfigurovat hello uživatele automaticky specifikace pro oprávnění správce. Spouštěcí úkol může například potřebovat přístup se zvýšeným oprávněním tooinstall softwaru na uzlu hello.

> [!NOTE] 
> Obecně platí, je nejvhodnější toouse přístup pouze v případě potřeby se zvýšenými oprávněními. Doporučeným udělení hello minimální oprávnění potřebná tooachieve hello požadovaný výsledek. Například spouštěcí úkol nainstaluje software pro aktuálního uživatele hello, místo pro všechny uživatele, může být schopný tooavoid udělení tootasks přístup se zvýšeným oprávněním. Můžete nakonfigurovat hello uživatele automaticky specifikace oboru a bez oprávnění správce přístup pro všechny úlohy, které je třeba toorun pod hello stejný účet, včetně hello spouštěcí úkol fondu. 
>
>

Hello následující fragmenty kódu ukazují, jak tooconfigure hello specifikace uživatele automaticky. Příklady Hello nastavení hello zvýšení úrovně příliš`Admin` a hello oboru příliš`Task`. Úloha oboru se hello výchozí nastavení, ale jsou zde uvedeny pro hello zájmu příklad.

#### <a name="batch-net"></a>Batch .NET

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a>Batch Java

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a>Batch Python

```python
user = batchmodels.UserIdentity(
    auto_user=batchmodels.AutoUserSpecification(
        elevation_level=batchmodels.ElevationLevel.admin,
        scope=batchmodels.AutoUserScope.task))
task = batchmodels.TaskAddParameter(
    id='task_1',
    command_line='cmd /c "echo hello world"',
    user_identity=user)
batch_client.task.add(job_id=jobid, task=task)
```

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a>Spustit úlohu jako uživatelé automaticky s oborem fondu

Pokud uzel je zřízený, dvě fondu celou automaticky uživatelské účty jsou vytvořeny na každý uzel ve fondu hello, jeden s oprávněním vyšší úrovně přístupu a jeden bez přístup se zvýšeným oprávněním. Nastavení oboru toopool oboru hello automaticky uživatele pro danou úlohu spustí úlohu hello v rámci jednoho z těchto dvou fondu celou automaticky uživatelské účty. 

Když zadáte rozsah fondu pro uživatele automaticky hello, všechny úlohy, které spustí s přístupem správce běh hello stejného fondu celou auto uživatelského účtu. Podobně úlohy, které běží bez oprávnění správce také spouštět pod účtem jednoho fondu celou auto uživatele. 

> [!NOTE] 
> Hello dvě fondu celou auto uživatelské účty jsou samostatné účty. Úlohy spuštěné v rámci účtu správce fondu celou hello nelze sdílet data s úkoly spuštěné pod účtem standardní hello a naopak. 
>
>

Hello využít toorunning pod stejnou auto uživatelský účet je, že úlohy možné tooshare data s ostatními úkoly spuštěné v hello hello stejného uzlu.

Jeden scénář, kde spuštěné úkoly v rámci jednoho z hello dva fondu celou auto uživatelské účty je užitečná sdílení tajných klíčů mezi úlohy je. Předpokládejme například, že spouštěcí úkol musí tooprovision tajný klíč do hello uzlu, který můžete použít jiné úlohy. Můžete použít hello Windows Data Protection API (DPAPI), ale vyžaduje oprávnění správce. Místo toho můžete chránit hello tajný klíč na úrovni uživatele hello. Úkoly spuštěné v rámci hello stejný uživatelský účet přístup hello tajný klíč bez přístup se zvýšeným oprávněním.

Sdílené složky jiný scénář, kde může být vhodné toorun úlohy v části Automatické uživatelský účet s oborem fondu je soubor rozhraní MPI (Message Passing). MPI sdílené složky je užitečné, když hello uzly v hello MPI úloh nutné toowork na hello stejná data souboru. Hello hlavního uzlu vytvoří složku, která hello podřízených uzlů může získat přístup, pokud jsou spuštěny v hello stejnou auto uživatelský účet. 

Následující fragment kódu Hello Nastaví obor toopool oboru hello automaticky uživatele pro úlohu v Batch .NET. zvýšení úrovně Hello je vynechán, takže hello úloha spuštěna hello standardní úrovni fondu automaticky uživatelský účet.

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a>S názvem uživatelské účty

Při vytváření fondu můžete definovat pojmenované uživatelské účty. Pojmenované uživatelský účet má název a heslo, které zadáte. Můžete zadat úroveň hello zvýšení oprávnění pro účet s názvem uživatele. Pro Linux uzly můžete zadat taky privátní klíč SSH.

Pojmenované uživatelský účet existuje na všech uzlech ve fondu hello a je k dispozici tooall úlohy běžet na těchto uzlech. Můžete definovat libovolný počet jmenovaní uživatelé pro fond. Když přidáte úkolu nebo kolekce úloh, můžete tuto úlohu hello spouští v rámci jednoho z hello s názvem uživatelské účty, které jsou definované ve fondu hello.

Pojmenované uživatelský účet je užitečné, když chcete, aby toorun všechny úlohy pro úlohu v části hello stejným uživatelským účtem, ale je izolovat z úloh spuštěných ve jiné úlohy na hello stejnou dobu. Můžete například vytvořit pojmenovanému uživateli pro každou úlohu a spouštět úlohy každou úlohu v části s názvem uživatelského účtu. Každá úloha sdílet tajný klíč s její vlastní úkoly, ale nikoli s úkoly spuštěné v jiné úlohy.

Můžete použít také pojmenovanému uživateli účtu toorun úlohu, která nastaví oprávnění na externím prostředkům, jako jsou sdílené složky. Pomocí pojmenovaného uživatelského účtu řídit hello identitu uživatele a pomocí tohoto oprávnění tooset identity uživatele.  

S názvem uživatelské účty umožňují bez heslo SSH mezi uzly Linux. Můžete vytvořit s názvem uživatelského účtu s Linux uzly, které potřebují úkoly s více instancemi toorun. Každý uzel ve fondu hello můžete spouštět úlohy pod uživatelským účtem na celý fond hello definován. Další informace o úkoly s více instancemi najdete v tématu [použít více\-instance úlohy aplikací MPI toorun](batch-mpi.md).

### <a name="create-named-user-accounts"></a>Vytvořit s názvem uživatelské účty

toocreate s názvem uživatelské účty ve službě Batch, přidejte kolekce fondu toohello uživatelské účty. Hello následující fragmenty kódu ukazují, jak toocreate s názvem uživatelské účty v rozhraní .NET, Java a Python. Tyto fragmenty zobrazit code jak toocreate správce i bez oprávnění správce. s názvem účty ve fondu. Příklady Hello vytvořit fondy pomocí hello cloudové služby konfigurace, ale používáte hello stejné postupovat při vytváření fondu systému Windows nebo Linux pomocí hello konfigurace virtuálního počítače.

#### <a name="batch-net-example-windows"></a>Příklad batch .NET (Windows)

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using hello cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                                         
    virtualMachineSize: "small",                                                
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));   

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount("adminUser", "xyz123", ElevationLevel.Admin),
    new UserAccount("nonAdminUser", "123xyz", ElevationLevel.NonAdmin),
};

// Commit hello pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a>Příklad batch .NET (Linux)

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of hello VM image toouse.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain hello first node agent SKU in hello collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create hello virtual machine configuration toouse toocreate hello pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create hello unbound pool.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                             
    virtualMachineSize: "Standard_A1",                                      
    virtualMachineConfiguration: virtualMachineConfiguration);                  

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount(
        name: "adminUser",
        password: "xyz123",
        elevationLevel: ElevationLevel.Admin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 12345,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
    new UserAccount(
        name: "nonAdminUser",
        password: "123xyz",
        elevationLevel: ElevationLevel.NonAdmin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 45678,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
};

// Commit hello pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a>Příklad Java batch

```java
List<UserAccount> userList = new ArrayList<>();
userList.add(new UserAccount().withName(adminUserAccountName).withPassword(adminPassword).withElevationLevel(ElevationLevel.ADMIN));
userList.add(new UserAccount().withName(nonAdminUserAccountName).withPassword(nonAdminPassword).withElevationLevel(ElevationLevel.NONADMIN));
PoolAddParameter addParameter = new PoolAddParameter()
        .withId(poolId)
        .withTargetDedicatedNodes(POOL_VM_COUNT)
        .withVmSize(POOL_VM_SIZE)
        .withCloudServiceConfiguration(configuration)
        .withUserAccounts(userList);
batchClient.poolOperations().createPool(addParameter);
```

#### <a name="batch-python-example"></a>Příklad batch Python

```python
users = [
    batchmodels.UserAccount(
        name='pool-admin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.admin)
    batchmodels.UserAccount(
        name='pool-nonadmin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.nonadmin)
]
pool = batchmodels.PoolAddParameter(
    id=pool_id,
    user_accounts=users,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        node_agent_sku_id=sku_to_use),
    vm_size=vm_size,
    target_dedicated=vm_count)
batch_client.pool.add(pool)
```

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a>Spustit úlohu v části s názvem uživatelského účtu s oprávněním vyšší úrovně přístupu

toorun úlohu jako uživatel se zvýšenými oprávněními, sada hello úloh na **UserIdentity** tooa vlastnost s názvem uživatelského účtu, který byl vytvořen s jeho **ElevationLevel** vlastností nastavenou příliš`Admin`.

Tento fragment kódu určuje, že tuto úlohu hello měly být spuštěny pod účtem s názvem uživatele. Tento účet pojmenovanému uživateli definované ve fondu hello při vytvoření fondu hello. V takovém případě hello s názvem uživatelský účet byl vytvořen s oprávněními správce:

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-toohello-latest-batch-client-library"></a>Aktualizovat knihovnu kódu toohello nejnovější Batch klienta

verze služby Batch Hello 2017-01-01.4.0 zavádí narušující změně, nahraďte hello **runElevated** vlastnost k dispozici v dřívějších verzích s hello **userIdentity** vlastnost. Hello následující tabulky poskytují jednoduché mapování, které můžete používat tooupdate kódu z dřívějších verzí knihoven klienta hello.

### <a name="batch-net"></a>Batch .NET

| Pokud váš kód používá...                  | Aktualizujte, aby...                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| `CloudTask.RunElevated`není zadaná. | Bez aktualizace                                                                                               |

### <a name="batch-java"></a>Batch Java

| Pokud váš kód používá...                      | Aktualizujte, aby...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| `CloudTask.withRunElevated`není zadaná. | Bez aktualizace                                                                                                                     |

### <a name="batch-python"></a>Batch Python

| Pokud váš kód používá...                      | Aktualizujte, aby...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | `user_identity=user`, kde <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | `user_identity=user`, kde <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| `run_elevated`není zadaná. | Bez aktualizace                                                                                                                                  |


## <a name="next-steps"></a>Další kroky

### <a name="batch-forum"></a>Fórum batch

Hello [fóru služby Azure Batch](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) na webu MSDN je skvělá umístit toodiscuss Batch a klást otázky týkající se služby hello. HEAD na přes pro užitečné definovaného příspěvky a při jejich vzniku při sestavování řešení Batch zveřejněte svoje otázky.
