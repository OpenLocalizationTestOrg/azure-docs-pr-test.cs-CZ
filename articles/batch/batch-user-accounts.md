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
# <a name="run-tasks-under-user-accounts-in-batch"></a><span data-ttu-id="f317b-103">Spuštění úlohy v části uživatelské účty ve službě Batch</span><span class="sxs-lookup"><span data-stu-id="f317b-103">Run tasks under user accounts in Batch</span></span>

<span data-ttu-id="f317b-104">Úloha v Azure Batch je vždy spuštěna pod uživatelským účtem.</span><span class="sxs-lookup"><span data-stu-id="f317b-104">A task in Azure Batch always runs under a user account.</span></span> <span data-ttu-id="f317b-105">Ve výchozím nastavení úlohy spouštěny pod standardní uživatelské účty, bez oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="f317b-105">By default, tasks run under standard user accounts, without administrator permissions.</span></span> <span data-ttu-id="f317b-106">Tato výchozí nastavení účtu uživatele je obvykle dostatečné.</span><span class="sxs-lookup"><span data-stu-id="f317b-106">These default user account settings are typically sufficient.</span></span> <span data-ttu-id="f317b-107">Pro určité scénáře ale je užitečné toobe možné tooconfigure hello uživatelský účet, pod kterým chcete toorun úloh.</span><span class="sxs-lookup"><span data-stu-id="f317b-107">For certain scenarios, however, it's useful toobe able tooconfigure hello user account under which you want a task toorun.</span></span> <span data-ttu-id="f317b-108">Tento článek popisuje hello typy uživatelských účtů a způsob jejich konfigurace pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="f317b-108">This article discusses hello types of user accounts and how you can configure them for your scenario.</span></span>

## <a name="types-of-user-accounts"></a><span data-ttu-id="f317b-109">Typy uživatelských účtů</span><span class="sxs-lookup"><span data-stu-id="f317b-109">Types of user accounts</span></span>

<span data-ttu-id="f317b-110">Azure Batch poskytuje dva typy uživatelských účtů pro spuštění úlohy:</span><span class="sxs-lookup"><span data-stu-id="f317b-110">Azure Batch provides two types of user accounts for running tasks:</span></span>

- <span data-ttu-id="f317b-111">**Auto uživatelské účty.**</span><span class="sxs-lookup"><span data-stu-id="f317b-111">**Auto-user accounts.**</span></span> <span data-ttu-id="f317b-112">Auto uživatelské účty jsou předdefinované uživatelské účty, které automaticky vytvářejí hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="f317b-112">Auto-user accounts are built-in user accounts that are created automatically by hello Batch service.</span></span> <span data-ttu-id="f317b-113">Ve výchozím nastavení úlohy spouštěny pod účtem uživatele automaticky.</span><span class="sxs-lookup"><span data-stu-id="f317b-113">By default, tasks run under an auto-user account.</span></span> <span data-ttu-id="f317b-114">Můžete nakonfigurovat hello uživatele automaticky specifikace pro úlohy tooindicate, které automaticky uživatele by měl účet úloha běžet.</span><span class="sxs-lookup"><span data-stu-id="f317b-114">You can configure hello auto-user specification for a task tooindicate under which auto-user account a task should run.</span></span> <span data-ttu-id="f317b-115">specifikace uživatele automaticky Hello vám umožní toospecify hello zvýšení úrovně a obor hello auto uživatelského účtu, který se spustí úloha hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-115">hello auto-user specification allows you toospecify hello elevation level and scope of hello auto-user account that will run hello task.</span></span> 

- <span data-ttu-id="f317b-116">**Pojmenované uživatelský účet.**</span><span class="sxs-lookup"><span data-stu-id="f317b-116">**A named user account.**</span></span> <span data-ttu-id="f317b-117">Pokud vytvoříte fond hello můžete určit jeden nebo více účtů pojmenovanému uživateli pro fond.</span><span class="sxs-lookup"><span data-stu-id="f317b-117">You can specify one or more named user accounts for a pool when you create hello pool.</span></span> <span data-ttu-id="f317b-118">Na každém uzlu hello fondu se vytvoří každý uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="f317b-118">Each user account is created on each node of hello pool.</span></span> <span data-ttu-id="f317b-119">Kromě toho toohello název účtu, zadejte heslo uživatelského účtu hello, zvýšení úrovně a pro Linux fondy, privátní klíč SSH hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-119">In addition toohello account name, you specify hello user account password, elevation level, and, for Linux pools, hello SSH private key.</span></span> <span data-ttu-id="f317b-120">Když přidáte úlohu, můžete zadat hello s názvem uživatelský účet, pod kterou by měla spouštět tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="f317b-120">When you add a task, you can specify hello named user account under which that task should run.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="f317b-121">verze služby Batch Hello 2017-01-01.4.0 zavádí narušující změně, která je nutné aktualizovat vaše toocall kód této verze.</span><span class="sxs-lookup"><span data-stu-id="f317b-121">hello Batch service version 2017-01-01.4.0 introduces a breaking change that requires that you update your code toocall that version.</span></span> <span data-ttu-id="f317b-122">Pokud se migrace kódu ze starší verze služby Batch, Všimněte si, že hello **runElevated** vlastnost již není podporována v knihovny klienta REST API nebo Batch hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-122">If you are migrating code from an older version of Batch, note that hello **runElevated** property is no longer supported in hello REST API or Batch client libraries.</span></span> <span data-ttu-id="f317b-123">Použití hello nové **userIdentity** vlastnost úloh toospecify zvýšení úrovně.</span><span class="sxs-lookup"><span data-stu-id="f317b-123">Use hello new **userIdentity** property of a task toospecify elevation level.</span></span> <span data-ttu-id="f317b-124">Hello části s názvem [aktualizovat knihovnu kódu toohello nejnovější Batch klienta](#update-your-code-to-the-latest-batch-client-library) pro rychlé pokyny pro aktualizaci kódu dávky, pokud použijete jednu z knihoven klienta hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-124">See hello section titled [Update your code toohello latest Batch client library](#update-your-code-to-the-latest-batch-client-library) for quick guidelines for updating your Batch code if you are using one of hello client libraries.</span></span>
>
>

> [!NOTE] 
> <span data-ttu-id="f317b-125">uživatelské účty Hello popsané v tomto článku nepodporují protokol RDP (Remote Desktop) nebo Secure Shell (SSH), z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="f317b-125">hello user accounts discussed in this article do not support Remote Desktop Protocol (RDP) or Secure Shell (SSH), for security reasons.</span></span> 
>
> <span data-ttu-id="f317b-126">tooconnect tooa uzlu spuštěný hello Linux konfigurace virtuálního počítače pomocí protokolu SSH, najdete v části [pomocí vzdálené plochy tooa virtuálního počítače s Linuxem v Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="f317b-126">tooconnect tooa node running hello Linux virtual machine configuration via SSH, see [Use Remote Desktop tooa Linux VM in Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span></span> <span data-ttu-id="f317b-127">tooconnect toonodes systémem Windows pomocí protokolu RDP, najdete v části [připojit tooa virtuálního počítače Windows serveru](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="f317b-127">tooconnect toonodes running Windows via RDP, see [Connect tooa Windows Server VM](../virtual-machines/windows/connect-logon.md).</span></span><br /><br />
> <span data-ttu-id="f317b-128">tooconnect tooa spuštěné hello cloudové služby konfigurace uzlu prostřednictvím protokolu RDP, najdete v části [povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f317b-128">tooconnect tooa node running hello cloud service configuration via RDP, see [Enable Remote Desktop Connection for a Role in Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span></span>
>
>

## <a name="user-account-access-toofiles-and-directories"></a><span data-ttu-id="f317b-129">Toofiles přístup k účtu uživatele a adresářů</span><span class="sxs-lookup"><span data-stu-id="f317b-129">User account access toofiles and directories</span></span>

<span data-ttu-id="f317b-130">Auto uživatelský účet a s názvem uživatelského účtu mají pracovní adresář pro čtení a zápis přístup toohello úkolu, sdíleného adresáře a adresář úlohy s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="f317b-130">Both an auto-user account and a named user account have read/write access toohello task’s working directory, shared directory, and multi-instance tasks directory.</span></span> <span data-ttu-id="f317b-131">Oba typy účtů mají přístup pro čtení toohello spuštění a úlohy přípravy adresáře.</span><span class="sxs-lookup"><span data-stu-id="f317b-131">Both types of accounts have read access toohello startup and job preparation directories.</span></span>

<span data-ttu-id="f317b-132">Pokud se úloha spouští pod hello stejný účet, který byl použit pro spuštění spouštěcího úkolu, hello úloh má přístup pro čtení a zápis toohello spuštění úloh adresář.</span><span class="sxs-lookup"><span data-stu-id="f317b-132">If a task runs under hello same account that was used for running a start task, hello task has read-write access toohello start task directory.</span></span> <span data-ttu-id="f317b-133">Podobně, pokud úlohy běží pod text hello stejný účet, který byl použit pro spuštění úkol přípravy úlohy, hello úloh má adresáře úkolu přípravy úlohy toohello přístup pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="f317b-133">Similarly, if a task runs under hello same account that was used for running a job preparation task, hello task has read-write access toohello job preparation task directory.</span></span> <span data-ttu-id="f317b-134">Pokud úloha běží pod jiným účtem než hello spouštěcí úkol nebo úkol přípravy úlohy, hello úloh má pouze přístup pro čtení toohello příslušných adresáři.</span><span class="sxs-lookup"><span data-stu-id="f317b-134">If a task runs under a different account than hello start task or job preparation task, then hello task has only read access toohello respective directory.</span></span>

<span data-ttu-id="f317b-135">Další informace o přístup k souborů a adresářů z úlohy najdete v tématu [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md#files-and-directories).</span><span class="sxs-lookup"><span data-stu-id="f317b-135">For more information on accessing files and directories from a task, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#files-and-directories).</span></span>

## <a name="elevated-access-for-tasks"></a><span data-ttu-id="f317b-136">Přístup se zvýšeným oprávněním pro úlohy</span><span class="sxs-lookup"><span data-stu-id="f317b-136">Elevated access for tasks</span></span> 

<span data-ttu-id="f317b-137">zvýšení úrovně Hello uživatelský účet určuje, zda úloha běží se zvýšenými oprávněními přístup.</span><span class="sxs-lookup"><span data-stu-id="f317b-137">hello user account's elevation level indicates whether a task runs with elevated access.</span></span> <span data-ttu-id="f317b-138">Auto uživatelský účet a s názvem uživatelského účtu můžete spustit s oprávněním vyšší úrovně přístupu.</span><span class="sxs-lookup"><span data-stu-id="f317b-138">Both an auto-user account and a named user account can run with elevated access.</span></span> <span data-ttu-id="f317b-139">pro zvýšení úrovně Hello dvě možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="f317b-139">hello two options for elevation level are:</span></span>

- <span data-ttu-id="f317b-140">**NonAdmin:** hello úloha běží jako standardní uživatel bez přístup se zvýšeným oprávněním.</span><span class="sxs-lookup"><span data-stu-id="f317b-140">**NonAdmin:** hello task runs as a standard user without elevated access.</span></span> <span data-ttu-id="f317b-141">Hello výchozí úroveň zvýšení oprávnění pro uživatelský účet Batch je vždy **NonAdmin**.</span><span class="sxs-lookup"><span data-stu-id="f317b-141">hello default elevation level for a Batch user account is always **NonAdmin**.</span></span>
- <span data-ttu-id="f317b-142">**Správce:** hello úloha běží jako uživatel s oprávněním vyšší úrovně přístupu a funguje s úplnými oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="f317b-142">**Admin:** hello task runs as a user with elevated access and operates with full Administrator permissions.</span></span> 

## <a name="auto-user-accounts"></a><span data-ttu-id="f317b-143">Auto uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="f317b-143">Auto-user accounts</span></span>

<span data-ttu-id="f317b-144">Ve výchozím nastavení úlohy spuštěny v dávce v rámci automatického uživatelský účet, jako standardní uživatel bez přístup se zvýšeným oprávněním a s oborem úloh.</span><span class="sxs-lookup"><span data-stu-id="f317b-144">By default, tasks run in Batch under an auto-user account, as a standard user without elevated access, and with task scope.</span></span> <span data-ttu-id="f317b-145">V případě specifikace hello uživatele automaticky nastavena pro obor úloh, služba Batch hello vytvoří automaticky uživatelský účet pro tuto úlohu jenom.</span><span class="sxs-lookup"><span data-stu-id="f317b-145">When hello auto-user specification is configured for task scope, hello Batch service creates an auto-user account for that task only.</span></span>

<span data-ttu-id="f317b-146">rozsah alternativní tootask Hello je fond oboru.</span><span class="sxs-lookup"><span data-stu-id="f317b-146">hello alternative tootask scope is pool scope.</span></span> <span data-ttu-id="f317b-147">Když hello uživatele automaticky specifikace pro úlohy je nakonfigurován pro fond oboru, hello úloha spuštěna automaticky uživatelský účet, který je k dispozici tooany úloh ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-147">When hello auto-user specification for a task is configured for pool scope, hello task runs under an auto-user account that is available tooany task in hello pool.</span></span> <span data-ttu-id="f317b-148">Další informace o rozsahu fondu najdete v tématu hello části s názvem [spuštění úlohy, jako hello automaticky uživatelem s oborem fondu](#run-a-task-as-the-autouser-with-pool-scope).</span><span class="sxs-lookup"><span data-stu-id="f317b-148">For more information about pool scope, see hello section titled [Run a task as hello auto-user with pool scope](#run-a-task-as-the-autouser-with-pool-scope).</span></span>   

<span data-ttu-id="f317b-149">výchozí obor Hello se liší v uzlech systému Windows a Linux:</span><span class="sxs-lookup"><span data-stu-id="f317b-149">hello default scope is different on Windows and Linux nodes:</span></span>

- <span data-ttu-id="f317b-150">Na uzlech Windows úlohy spuštěny v rámci oboru úloha ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f317b-150">On Windows nodes, tasks run under task scope by default.</span></span>
- <span data-ttu-id="f317b-151">Uzly Linux vždy spuštěný fondu oboru.</span><span class="sxs-lookup"><span data-stu-id="f317b-151">Linux nodes always run under pool scope.</span></span>

<span data-ttu-id="f317b-152">Existují čtyři možné konfigurace pro uživatele automaticky specifikace hello, z nichž každý odpovídá tooa jedinečný auto uživatelský účet:</span><span class="sxs-lookup"><span data-stu-id="f317b-152">There are four possible configurations for hello auto-user specification, each of which corresponds tooa unique auto-user account:</span></span>

- <span data-ttu-id="f317b-153">Přístup bez oprávnění správce s oborem úloh (hello výchozí uživatele automaticky specification)</span><span class="sxs-lookup"><span data-stu-id="f317b-153">Non-admin access with task scope (hello default auto-user specification)</span></span>
- <span data-ttu-id="f317b-154">Přístup správce (zvýšenými) s oborem úloh</span><span class="sxs-lookup"><span data-stu-id="f317b-154">Admin (elevated) access with task scope</span></span>
- <span data-ttu-id="f317b-155">Přístup bez oprávnění správce s oborem fondu</span><span class="sxs-lookup"><span data-stu-id="f317b-155">Non-admin access with pool scope</span></span>
- <span data-ttu-id="f317b-156">Přístup správce k oboru fondu</span><span class="sxs-lookup"><span data-stu-id="f317b-156">Admin access with pool scope</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="f317b-157">Úkoly spuštěné v rámci oboru úloh nemají fakticky přístup tooother úlohy na uzlu.</span><span class="sxs-lookup"><span data-stu-id="f317b-157">Tasks running under task scope do not have de facto access tooother tasks on a node.</span></span> <span data-ttu-id="f317b-158">Ale uživatel se zlými úmysly se účet pro přístup k toohello může toto omezení obejít tak, že zadáte úlohu, která spustí s oprávněními správce a přistupuje k jiných adresářích úkolu.</span><span class="sxs-lookup"><span data-stu-id="f317b-158">However, a malicious user with access toohello account could work around this restriction by submitting a task that runs with administrator privileges and accesses other task directories.</span></span> <span data-ttu-id="f317b-159">Uživatel se zlými úmysly může také pomocí protokolu RDP nebo SSH tooconnect tooa uzlu.</span><span class="sxs-lookup"><span data-stu-id="f317b-159">A malicious user could also use RDP or SSH tooconnect tooa node.</span></span> <span data-ttu-id="f317b-160">Je důležité tooprotect přístup tooyour dávkového účtu klíče tooprevent takové situaci.</span><span class="sxs-lookup"><span data-stu-id="f317b-160">It's important tooprotect access tooyour Batch account keys tooprevent such a scenario.</span></span> <span data-ttu-id="f317b-161">Pokud se domníváte, že váš účet ohrožený, být jisti tooregenerate klíče.</span><span class="sxs-lookup"><span data-stu-id="f317b-161">If you suspect your account may have been compromised, be sure tooregenerate your keys.</span></span>
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a><span data-ttu-id="f317b-162">Spustit úlohu jako auto uživatel s oprávněním vyšší úrovně přístupu</span><span class="sxs-lookup"><span data-stu-id="f317b-162">Run a task as an auto-user with elevated access</span></span>

<span data-ttu-id="f317b-163">Pokud budete potřebovat toorun úloha s přístup se zvýšeným oprávněním, můžete nakonfigurovat hello uživatele automaticky specifikace pro oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="f317b-163">You can configure hello auto-user specification for administrator privileges when you need toorun a task with elevated access.</span></span> <span data-ttu-id="f317b-164">Spouštěcí úkol může například potřebovat přístup se zvýšeným oprávněním tooinstall softwaru na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-164">For example, a start task may need elevated access tooinstall software on hello node.</span></span>

> [!NOTE] 
> <span data-ttu-id="f317b-165">Obecně platí, je nejvhodnější toouse přístup pouze v případě potřeby se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="f317b-165">In general, it's best toouse elevated access only when necessary.</span></span> <span data-ttu-id="f317b-166">Doporučeným udělení hello minimální oprávnění potřebná tooachieve hello požadovaný výsledek.</span><span class="sxs-lookup"><span data-stu-id="f317b-166">Best practices recommend granting hello minimum privilege necessary tooachieve hello desired outcome.</span></span> <span data-ttu-id="f317b-167">Například spouštěcí úkol nainstaluje software pro aktuálního uživatele hello, místo pro všechny uživatele, může být schopný tooavoid udělení tootasks přístup se zvýšeným oprávněním.</span><span class="sxs-lookup"><span data-stu-id="f317b-167">For example, if a start task installs software for hello current user, instead of for all users, you may be able tooavoid granting elevated access tootasks.</span></span> <span data-ttu-id="f317b-168">Můžete nakonfigurovat hello uživatele automaticky specifikace oboru a bez oprávnění správce přístup pro všechny úlohy, které je třeba toorun pod hello stejný účet, včetně hello spouštěcí úkol fondu.</span><span class="sxs-lookup"><span data-stu-id="f317b-168">You can configure hello auto-user specification for pool scope and non-admin access for all tasks that need toorun under hello same account, including hello start task.</span></span> 
>
>

<span data-ttu-id="f317b-169">Hello následující fragmenty kódu ukazují, jak tooconfigure hello specifikace uživatele automaticky.</span><span class="sxs-lookup"><span data-stu-id="f317b-169">hello following code snippets show how tooconfigure hello auto-user specification.</span></span> <span data-ttu-id="f317b-170">Příklady Hello nastavení hello zvýšení úrovně příliš`Admin` a hello oboru příliš`Task`.</span><span class="sxs-lookup"><span data-stu-id="f317b-170">hello examples set hello elevation level too`Admin` and hello scope too`Task`.</span></span> <span data-ttu-id="f317b-171">Úloha oboru se hello výchozí nastavení, ale jsou zde uvedeny pro hello zájmu příklad.</span><span class="sxs-lookup"><span data-stu-id="f317b-171">Task scope is hello default setting, but is included here for hello sake of example.</span></span>

#### <a name="batch-net"></a><span data-ttu-id="f317b-172">Batch .NET</span><span class="sxs-lookup"><span data-stu-id="f317b-172">Batch .NET</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a><span data-ttu-id="f317b-173">Batch Java</span><span class="sxs-lookup"><span data-stu-id="f317b-173">Batch Java</span></span>

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a><span data-ttu-id="f317b-174">Batch Python</span><span class="sxs-lookup"><span data-stu-id="f317b-174">Batch Python</span></span>

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

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a><span data-ttu-id="f317b-175">Spustit úlohu jako uživatelé automaticky s oborem fondu</span><span class="sxs-lookup"><span data-stu-id="f317b-175">Run a task as an auto-user with pool scope</span></span>

<span data-ttu-id="f317b-176">Pokud uzel je zřízený, dvě fondu celou automaticky uživatelské účty jsou vytvořeny na každý uzel ve fondu hello, jeden s oprávněním vyšší úrovně přístupu a jeden bez přístup se zvýšeným oprávněním.</span><span class="sxs-lookup"><span data-stu-id="f317b-176">When a node is provisioned, two pool-wide auto-user accounts are created on each node in hello pool, one with elevated access, and one without elevated access.</span></span> <span data-ttu-id="f317b-177">Nastavení oboru toopool oboru hello automaticky uživatele pro danou úlohu spustí úlohu hello v rámci jednoho z těchto dvou fondu celou automaticky uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f317b-177">Setting hello auto-user's scope toopool scope for a given task runs hello task under one of these two pool-wide auto-user accounts.</span></span> 

<span data-ttu-id="f317b-178">Když zadáte rozsah fondu pro uživatele automaticky hello, všechny úlohy, které spustí s přístupem správce běh hello stejného fondu celou auto uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="f317b-178">When you specify pool scope for hello auto-user, all tasks that run with administrator access run under hello same pool-wide auto-user account.</span></span> <span data-ttu-id="f317b-179">Podobně úlohy, které běží bez oprávnění správce také spouštět pod účtem jednoho fondu celou auto uživatele.</span><span class="sxs-lookup"><span data-stu-id="f317b-179">Similarly, tasks that run without administrator permissions also run under a single pool-wide auto-user account.</span></span> 

> [!NOTE] 
> <span data-ttu-id="f317b-180">Hello dvě fondu celou auto uživatelské účty jsou samostatné účty.</span><span class="sxs-lookup"><span data-stu-id="f317b-180">hello two pool-wide auto-user accounts are separate accounts.</span></span> <span data-ttu-id="f317b-181">Úlohy spuštěné v rámci účtu správce fondu celou hello nelze sdílet data s úkoly spuštěné pod účtem standardní hello a naopak.</span><span class="sxs-lookup"><span data-stu-id="f317b-181">Tasks running under hello pool-wide administrative account cannot share data with tasks running under hello standard account, and vice versa.</span></span> 
>
>

<span data-ttu-id="f317b-182">Hello využít toorunning pod stejnou auto uživatelský účet je, že úlohy možné tooshare data s ostatními úkoly spuštěné v hello hello stejného uzlu.</span><span class="sxs-lookup"><span data-stu-id="f317b-182">hello advantage toorunning under hello same auto-user account is that tasks are able tooshare data with other tasks running on hello same node.</span></span>

<span data-ttu-id="f317b-183">Jeden scénář, kde spuštěné úkoly v rámci jednoho z hello dva fondu celou auto uživatelské účty je užitečná sdílení tajných klíčů mezi úlohy je.</span><span class="sxs-lookup"><span data-stu-id="f317b-183">Sharing secrets between tasks is one scenario where running tasks under one of hello two pool-wide auto-user accounts is useful.</span></span> <span data-ttu-id="f317b-184">Předpokládejme například, že spouštěcí úkol musí tooprovision tajný klíč do hello uzlu, který můžete použít jiné úlohy.</span><span class="sxs-lookup"><span data-stu-id="f317b-184">For example, suppose a start task needs tooprovision a secret onto hello node that other tasks can use.</span></span> <span data-ttu-id="f317b-185">Můžete použít hello Windows Data Protection API (DPAPI), ale vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="f317b-185">You could use hello Windows Data Protection API (DPAPI), but it requires administrator privileges.</span></span> <span data-ttu-id="f317b-186">Místo toho můžete chránit hello tajný klíč na úrovni uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-186">Instead, you can protect hello secret at hello user level.</span></span> <span data-ttu-id="f317b-187">Úkoly spuštěné v rámci hello stejný uživatelský účet přístup hello tajný klíč bez přístup se zvýšeným oprávněním.</span><span class="sxs-lookup"><span data-stu-id="f317b-187">Tasks running under hello same user account can access hello secret without elevated access.</span></span>

<span data-ttu-id="f317b-188">Sdílené složky jiný scénář, kde může být vhodné toorun úlohy v části Automatické uživatelský účet s oborem fondu je soubor rozhraní MPI (Message Passing).</span><span class="sxs-lookup"><span data-stu-id="f317b-188">Another scenario where you may want toorun tasks under an auto-user account with pool scope is a Message Passing Interface (MPI) file share.</span></span> <span data-ttu-id="f317b-189">MPI sdílené složky je užitečné, když hello uzly v hello MPI úloh nutné toowork na hello stejná data souboru.</span><span class="sxs-lookup"><span data-stu-id="f317b-189">An MPI file share is useful when hello nodes in hello MPI task need toowork on hello same file data.</span></span> <span data-ttu-id="f317b-190">Hello hlavního uzlu vytvoří složku, která hello podřízených uzlů může získat přístup, pokud jsou spuštěny v hello stejnou auto uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="f317b-190">hello head node creates a file share that hello child nodes can access if they are running under hello same auto-user account.</span></span> 

<span data-ttu-id="f317b-191">Následující fragment kódu Hello Nastaví obor toopool oboru hello automaticky uživatele pro úlohu v Batch .NET.</span><span class="sxs-lookup"><span data-stu-id="f317b-191">hello following code snippet sets hello auto-user's scope toopool scope for a task in Batch .NET.</span></span> <span data-ttu-id="f317b-192">zvýšení úrovně Hello je vynechán, takže hello úloha spuštěna hello standardní úrovni fondu automaticky uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="f317b-192">hello elevation level is omitted, so hello task runs under hello standard pool-wide auto-user account.</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a><span data-ttu-id="f317b-193">S názvem uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="f317b-193">Named user accounts</span></span>

<span data-ttu-id="f317b-194">Při vytváření fondu můžete definovat pojmenované uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f317b-194">You can define named user accounts when you create a pool.</span></span> <span data-ttu-id="f317b-195">Pojmenované uživatelský účet má název a heslo, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="f317b-195">A named user account has a name and password that you provide.</span></span> <span data-ttu-id="f317b-196">Můžete zadat úroveň hello zvýšení oprávnění pro účet s názvem uživatele.</span><span class="sxs-lookup"><span data-stu-id="f317b-196">You can specify hello elevation level for a named user account.</span></span> <span data-ttu-id="f317b-197">Pro Linux uzly můžete zadat taky privátní klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="f317b-197">For Linux nodes, you can also provide an SSH private key.</span></span>

<span data-ttu-id="f317b-198">Pojmenované uživatelský účet existuje na všech uzlech ve fondu hello a je k dispozici tooall úlohy běžet na těchto uzlech.</span><span class="sxs-lookup"><span data-stu-id="f317b-198">A named user account exists on all nodes in hello pool and is available tooall tasks running on those nodes.</span></span> <span data-ttu-id="f317b-199">Můžete definovat libovolný počet jmenovaní uživatelé pro fond.</span><span class="sxs-lookup"><span data-stu-id="f317b-199">You may define any number of named users for a pool.</span></span> <span data-ttu-id="f317b-200">Když přidáte úkolu nebo kolekce úloh, můžete tuto úlohu hello spouští v rámci jednoho z hello s názvem uživatelské účty, které jsou definované ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-200">When you add a task or task collection, you can specify that hello task runs under one of hello named user accounts defined on hello pool.</span></span>

<span data-ttu-id="f317b-201">Pojmenované uživatelský účet je užitečné, když chcete, aby toorun všechny úlohy pro úlohu v části hello stejným uživatelským účtem, ale je izolovat z úloh spuštěných ve jiné úlohy na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="f317b-201">A named user account is useful when you want toorun all tasks in a job under hello same user account, but isolate them from tasks running in other jobs at hello same time.</span></span> <span data-ttu-id="f317b-202">Můžete například vytvořit pojmenovanému uživateli pro každou úlohu a spouštět úlohy každou úlohu v části s názvem uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="f317b-202">For example, you can create a named user for each job, and run each job's tasks under that named user account.</span></span> <span data-ttu-id="f317b-203">Každá úloha sdílet tajný klíč s její vlastní úkoly, ale nikoli s úkoly spuštěné v jiné úlohy.</span><span class="sxs-lookup"><span data-stu-id="f317b-203">Each job can then share a secret with its own tasks, but not with tasks running in other jobs.</span></span>

<span data-ttu-id="f317b-204">Můžete použít také pojmenovanému uživateli účtu toorun úlohu, která nastaví oprávnění na externím prostředkům, jako jsou sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="f317b-204">You can also use a named user account toorun a task that sets permissions on external resources such as file shares.</span></span> <span data-ttu-id="f317b-205">Pomocí pojmenovaného uživatelského účtu řídit hello identitu uživatele a pomocí tohoto oprávnění tooset identity uživatele.</span><span class="sxs-lookup"><span data-stu-id="f317b-205">With a named user account, you control hello user identity and can use that user identity tooset permissions.</span></span>  

<span data-ttu-id="f317b-206">S názvem uživatelské účty umožňují bez heslo SSH mezi uzly Linux.</span><span class="sxs-lookup"><span data-stu-id="f317b-206">Named user accounts enable password-less SSH between Linux nodes.</span></span> <span data-ttu-id="f317b-207">Můžete vytvořit s názvem uživatelského účtu s Linux uzly, které potřebují úkoly s více instancemi toorun.</span><span class="sxs-lookup"><span data-stu-id="f317b-207">You can use a named user account with Linux nodes that need toorun multi-instance tasks.</span></span> <span data-ttu-id="f317b-208">Každý uzel ve fondu hello můžete spouštět úlohy pod uživatelským účtem na celý fond hello definován.</span><span class="sxs-lookup"><span data-stu-id="f317b-208">Each node in hello pool can run tasks under a user account defined on hello whole pool.</span></span> <span data-ttu-id="f317b-209">Další informace o úkoly s více instancemi najdete v tématu [použít více\-instance úlohy aplikací MPI toorun](batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="f317b-209">For more information about multi-instance tasks, see [Use multi\-instance tasks toorun MPI applications](batch-mpi.md).</span></span>

### <a name="create-named-user-accounts"></a><span data-ttu-id="f317b-210">Vytvořit s názvem uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="f317b-210">Create named user accounts</span></span>

<span data-ttu-id="f317b-211">toocreate s názvem uživatelské účty ve službě Batch, přidejte kolekce fondu toohello uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f317b-211">toocreate named user accounts in Batch, add a collection of user accounts toohello pool.</span></span> <span data-ttu-id="f317b-212">Hello následující fragmenty kódu ukazují, jak toocreate s názvem uživatelské účty v rozhraní .NET, Java a Python.</span><span class="sxs-lookup"><span data-stu-id="f317b-212">hello following code snippets show how toocreate named user accounts in .NET, Java, and Python.</span></span> <span data-ttu-id="f317b-213">Tyto fragmenty zobrazit code jak toocreate správce i bez oprávnění správce. s názvem účty ve fondu.</span><span class="sxs-lookup"><span data-stu-id="f317b-213">These code snippets show how toocreate both admin and non-admin named accounts on a pool.</span></span> <span data-ttu-id="f317b-214">Příklady Hello vytvořit fondy pomocí hello cloudové služby konfigurace, ale používáte hello stejné postupovat při vytváření fondu systému Windows nebo Linux pomocí hello konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f317b-214">hello examples create pools using hello cloud service configuration, but you use hello same approach when creating a Windows or Linux pool using hello virtual machine configuration.</span></span>

#### <a name="batch-net-example-windows"></a><span data-ttu-id="f317b-215">Příklad batch .NET (Windows)</span><span class="sxs-lookup"><span data-stu-id="f317b-215">Batch .NET example (Windows)</span></span>

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

#### <a name="batch-net-example-linux"></a><span data-ttu-id="f317b-216">Příklad batch .NET (Linux)</span><span class="sxs-lookup"><span data-stu-id="f317b-216">Batch .NET example (Linux)</span></span>

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


#### <a name="batch-java-example"></a><span data-ttu-id="f317b-217">Příklad Java batch</span><span class="sxs-lookup"><span data-stu-id="f317b-217">Batch Java example</span></span>

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

#### <a name="batch-python-example"></a><span data-ttu-id="f317b-218">Příklad batch Python</span><span class="sxs-lookup"><span data-stu-id="f317b-218">Batch Python example</span></span>

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

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a><span data-ttu-id="f317b-219">Spustit úlohu v části s názvem uživatelského účtu s oprávněním vyšší úrovně přístupu</span><span class="sxs-lookup"><span data-stu-id="f317b-219">Run a task under a named user account with elevated access</span></span>

<span data-ttu-id="f317b-220">toorun úlohu jako uživatel se zvýšenými oprávněními, sada hello úloh na **UserIdentity** tooa vlastnost s názvem uživatelského účtu, který byl vytvořen s jeho **ElevationLevel** vlastností nastavenou příliš`Admin`.</span><span class="sxs-lookup"><span data-stu-id="f317b-220">toorun a task as an elevated user, set hello task's **UserIdentity** property tooa named user account that was created with its **ElevationLevel** property set too`Admin`.</span></span>

<span data-ttu-id="f317b-221">Tento fragment kódu určuje, že tuto úlohu hello měly být spuštěny pod účtem s názvem uživatele.</span><span class="sxs-lookup"><span data-stu-id="f317b-221">This code snippet specifies that hello task should run under a named user account.</span></span> <span data-ttu-id="f317b-222">Tento účet pojmenovanému uživateli definované ve fondu hello při vytvoření fondu hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-222">This named user account was defined on hello pool when hello pool was created.</span></span> <span data-ttu-id="f317b-223">V takovém případě hello s názvem uživatelský účet byl vytvořen s oprávněními správce:</span><span class="sxs-lookup"><span data-stu-id="f317b-223">In this case, hello named user account was created with admin permissions:</span></span>

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-toohello-latest-batch-client-library"></a><span data-ttu-id="f317b-224">Aktualizovat knihovnu kódu toohello nejnovější Batch klienta</span><span class="sxs-lookup"><span data-stu-id="f317b-224">Update your code toohello latest Batch client library</span></span>

<span data-ttu-id="f317b-225">verze služby Batch Hello 2017-01-01.4.0 zavádí narušující změně, nahraďte hello **runElevated** vlastnost k dispozici v dřívějších verzích s hello **userIdentity** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f317b-225">hello Batch service version 2017-01-01.4.0 introduces a breaking change, replacing hello **runElevated** property available in earlier versions with hello **userIdentity** property.</span></span> <span data-ttu-id="f317b-226">Hello následující tabulky poskytují jednoduché mapování, které můžete používat tooupdate kódu z dřívějších verzí knihoven klienta hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-226">hello following tables provide a simple mapping that you can use tooupdate your code from earlier versions of hello client libraries.</span></span>

### <a name="batch-net"></a><span data-ttu-id="f317b-227">Batch .NET</span><span class="sxs-lookup"><span data-stu-id="f317b-227">Batch .NET</span></span>

| <span data-ttu-id="f317b-228">Pokud váš kód používá...</span><span class="sxs-lookup"><span data-stu-id="f317b-228">If your code uses...</span></span>                  | <span data-ttu-id="f317b-229">Aktualizujte, aby...</span><span class="sxs-lookup"><span data-stu-id="f317b-229">Update it to....</span></span>                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| <span data-ttu-id="f317b-230">`CloudTask.RunElevated`není zadaná.</span><span class="sxs-lookup"><span data-stu-id="f317b-230">`CloudTask.RunElevated` not specified</span></span> | <span data-ttu-id="f317b-231">Bez aktualizace</span><span class="sxs-lookup"><span data-stu-id="f317b-231">No update required</span></span>                                                                                               |

### <a name="batch-java"></a><span data-ttu-id="f317b-232">Batch Java</span><span class="sxs-lookup"><span data-stu-id="f317b-232">Batch Java</span></span>

| <span data-ttu-id="f317b-233">Pokud váš kód používá...</span><span class="sxs-lookup"><span data-stu-id="f317b-233">If your code uses...</span></span>                      | <span data-ttu-id="f317b-234">Aktualizujte, aby...</span><span class="sxs-lookup"><span data-stu-id="f317b-234">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| <span data-ttu-id="f317b-235">`CloudTask.withRunElevated`není zadaná.</span><span class="sxs-lookup"><span data-stu-id="f317b-235">`CloudTask.withRunElevated` not specified</span></span> | <span data-ttu-id="f317b-236">Bez aktualizace</span><span class="sxs-lookup"><span data-stu-id="f317b-236">No update required</span></span>                                                                                                                     |

### <a name="batch-python"></a><span data-ttu-id="f317b-237">Batch Python</span><span class="sxs-lookup"><span data-stu-id="f317b-237">Batch Python</span></span>

| <span data-ttu-id="f317b-238">Pokud váš kód používá...</span><span class="sxs-lookup"><span data-stu-id="f317b-238">If your code uses...</span></span>                      | <span data-ttu-id="f317b-239">Aktualizujte, aby...</span><span class="sxs-lookup"><span data-stu-id="f317b-239">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | <span data-ttu-id="f317b-240">`user_identity=user`, kde</span><span class="sxs-lookup"><span data-stu-id="f317b-240">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | <span data-ttu-id="f317b-241">`user_identity=user`, kde</span><span class="sxs-lookup"><span data-stu-id="f317b-241">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| <span data-ttu-id="f317b-242">`run_elevated`není zadaná.</span><span class="sxs-lookup"><span data-stu-id="f317b-242">`run_elevated` not specified</span></span> | <span data-ttu-id="f317b-243">Bez aktualizace</span><span class="sxs-lookup"><span data-stu-id="f317b-243">No update required</span></span>                                                                                                                                  |


## <a name="next-steps"></a><span data-ttu-id="f317b-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f317b-244">Next steps</span></span>

### <a name="batch-forum"></a><span data-ttu-id="f317b-245">Fórum batch</span><span class="sxs-lookup"><span data-stu-id="f317b-245">Batch Forum</span></span>

<span data-ttu-id="f317b-246">Hello [fóru služby Azure Batch](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) na webu MSDN je skvělá umístit toodiscuss Batch a klást otázky týkající se služby hello.</span><span class="sxs-lookup"><span data-stu-id="f317b-246">hello [Azure Batch Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="f317b-247">HEAD na přes pro užitečné definovaného příspěvky a při jejich vzniku při sestavování řešení Batch zveřejněte svoje otázky.</span><span class="sxs-lookup"><span data-stu-id="f317b-247">Head on over for helpful pinned posts, and post your questions as they arise while you build your Batch solutions.</span></span>
