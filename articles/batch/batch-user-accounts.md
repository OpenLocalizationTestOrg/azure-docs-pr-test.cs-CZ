---
title: "Spuštění úlohy v části uživatelské účty ve službě Azure Batch | Microsoft Docs"
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
ms.openlocfilehash: d408c0565c0ed81fc97cc2b3976a4fc233e31302
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-tasks-under-user-accounts-in-batch"></a><span data-ttu-id="3a3d1-103">Spuštění úlohy v části uživatelské účty ve službě Batch</span><span class="sxs-lookup"><span data-stu-id="3a3d1-103">Run tasks under user accounts in Batch</span></span>

<span data-ttu-id="3a3d1-104">Úloha v Azure Batch je vždy spuštěna pod uživatelským účtem.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-104">A task in Azure Batch always runs under a user account.</span></span> <span data-ttu-id="3a3d1-105">Ve výchozím nastavení úlohy spouštěny pod standardní uživatelské účty, bez oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-105">By default, tasks run under standard user accounts, without administrator permissions.</span></span> <span data-ttu-id="3a3d1-106">Tato výchozí nastavení účtu uživatele je obvykle dostatečné.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-106">These default user account settings are typically sufficient.</span></span> <span data-ttu-id="3a3d1-107">Pro určité scénáře je však vhodné, když ji moct konfigurovat uživatelský účet, pod kterou chcete spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-107">For certain scenarios, however, it's useful to be able to configure the user account under which you want a task to run.</span></span> <span data-ttu-id="3a3d1-108">Tento článek popisuje typy uživatelských účtů a způsob jejich konfigurace pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-108">This article discusses the types of user accounts and how you can configure them for your scenario.</span></span>

## <a name="types-of-user-accounts"></a><span data-ttu-id="3a3d1-109">Typy uživatelských účtů</span><span class="sxs-lookup"><span data-stu-id="3a3d1-109">Types of user accounts</span></span>

<span data-ttu-id="3a3d1-110">Azure Batch poskytuje dva typy uživatelských účtů pro spuštění úlohy:</span><span class="sxs-lookup"><span data-stu-id="3a3d1-110">Azure Batch provides two types of user accounts for running tasks:</span></span>

- <span data-ttu-id="3a3d1-111">**Auto uživatelské účty.**</span><span class="sxs-lookup"><span data-stu-id="3a3d1-111">**Auto-user accounts.**</span></span> <span data-ttu-id="3a3d1-112">Auto uživatelské účty jsou předdefinované uživatelské účty, které jsou automaticky vytvořené službou Batch.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-112">Auto-user accounts are built-in user accounts that are created automatically by the Batch service.</span></span> <span data-ttu-id="3a3d1-113">Ve výchozím nastavení úlohy spouštěny pod účtem uživatele automaticky.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-113">By default, tasks run under an auto-user account.</span></span> <span data-ttu-id="3a3d1-114">Specifikace automaticky uživatele pro úlohu k označení, pod které automaticky uživatelský účet bude úloha spuštěna, můžete nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-114">You can configure the auto-user specification for a task to indicate under which auto-user account a task should run.</span></span> <span data-ttu-id="3a3d1-115">Specifikace uživatele automaticky vám umožní určit úroveň zvýšení oprávnění a rozsah automaticky uživatelský účet, který se spustí úlohu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-115">The auto-user specification allows you to specify the elevation level and scope of the auto-user account that will run the task.</span></span> 

- <span data-ttu-id="3a3d1-116">**Pojmenované uživatelský účet.**</span><span class="sxs-lookup"><span data-stu-id="3a3d1-116">**A named user account.**</span></span> <span data-ttu-id="3a3d1-117">Při vytváření fondu můžete určit jeden nebo více účtů pojmenovanému uživateli pro fond.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-117">You can specify one or more named user accounts for a pool when you create the pool.</span></span> <span data-ttu-id="3a3d1-118">Každý uživatelský účet je vytvořen na všech uzlech fondu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-118">Each user account is created on each node of the pool.</span></span> <span data-ttu-id="3a3d1-119">Kromě název účtu zadejte heslo k uživatelskému účtu, zvýšení úrovně a pro Linux fondy, privátní klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-119">In addition to the account name, you specify the user account password, elevation level, and, for Linux pools, the SSH private key.</span></span> <span data-ttu-id="3a3d1-120">Když přidáte úlohu, můžete s názvem uživatelský účet, pod kterou by měla spouštět tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-120">When you add a task, you can specify the named user account under which that task should run.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="3a3d1-121">Verze služby Batch 2017-01-01.4.0 zavádí narušující změně, která je nutné aktualizovat na volání této verze.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-121">The Batch service version 2017-01-01.4.0 introduces a breaking change that requires that you update your code to call that version.</span></span> <span data-ttu-id="3a3d1-122">Pokud se migrace kódu ze starší verze služby Batch, Všimněte si, že **runElevated** vlastnost již není podporována v knihovny klienta REST API nebo dávce.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-122">If you are migrating code from an older version of Batch, note that the **runElevated** property is no longer supported in the REST API or Batch client libraries.</span></span> <span data-ttu-id="3a3d1-123">Použít novou **userIdentity** vlastnosti úlohy k určení zvýšení úrovně.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-123">Use the new **userIdentity** property of a task to specify elevation level.</span></span> <span data-ttu-id="3a3d1-124">Najdete v části s názvem [kódu aktualizovat na nejnovější klientské knihovny Batch](#update-your-code-to-the-latest-batch-client-library) pro rychlé pokyny pro aktualizaci kódu dávky, pokud použijete jednu z knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-124">See the section titled [Update your code to the latest Batch client library](#update-your-code-to-the-latest-batch-client-library) for quick guidelines for updating your Batch code if you are using one of the client libraries.</span></span>
>
>

> [!NOTE] 
> <span data-ttu-id="3a3d1-125">Uživatelské účty popsané v tomto článku nepodporují protokol RDP (Remote Desktop) nebo Secure Shell (SSH), z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-125">The user accounts discussed in this article do not support Remote Desktop Protocol (RDP) or Secure Shell (SSH), for security reasons.</span></span> 
>
> <span data-ttu-id="3a3d1-126">Chcete-li připojit k uzlu se systémem Linux konfigurace virtuálního počítače pomocí protokolu SSH, přečtěte si téma [pomocí vzdálené plochy pro virtuální počítač s Linuxem v Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="3a3d1-126">To connect to a node running the Linux virtual machine configuration via SSH, see [Use Remote Desktop to a Linux VM in Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md).</span></span> <span data-ttu-id="3a3d1-127">Pro připojení k uzly s Windows pomocí protokolu RDP, najdete v části [připojit k virtuální počítač Windows serveru](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="3a3d1-127">To connect to nodes running Windows via RDP, see [Connect to a Windows Server VM](../virtual-machines/windows/connect-logon.md).</span></span><br /><br />
> <span data-ttu-id="3a3d1-128">Chcete-li připojit k uzlu se systémem konfigurace cloudové služby prostřednictvím protokolu RDP, přečtěte si téma [povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3a3d1-128">To connect to a node running the cloud service configuration via RDP, see [Enable Remote Desktop Connection for a Role in Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).</span></span>
>
>

## <a name="user-account-access-to-files-and-directories"></a><span data-ttu-id="3a3d1-129">Uživatelskému účtu přístup do souborů a adresářů</span><span class="sxs-lookup"><span data-stu-id="3a3d1-129">User account access to files and directories</span></span>

<span data-ttu-id="3a3d1-130">Auto uživatelský účet a s názvem uživatelského účtu mají přístup pro čtení a zápis pracovního adresáře úkolu, sdíleného adresáře a adresář úlohy s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-130">Both an auto-user account and a named user account have read/write access to the task’s working directory, shared directory, and multi-instance tasks directory.</span></span> <span data-ttu-id="3a3d1-131">Oba typy účtů mají přístup pro čtení k adresáři pro spuštění a úlohy přípravy.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-131">Both types of accounts have read access to the startup and job preparation directories.</span></span>

<span data-ttu-id="3a3d1-132">Pokud úloha běží pod stejným účtem, který byl použit ke spuštění spouštěcí úkol, úloha má přístup pro čtení a zápis do adresáře úkolu spuštění.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-132">If a task runs under the same account that was used for running a start task, the task has read-write access to the start task directory.</span></span> <span data-ttu-id="3a3d1-133">Podobně pokud úlohy běží pod stejným účtem, který byl použit pro spuštění úkol přípravy úlohy, úlohy má přístup pro čtení a zápis do adresáře úkolu přípravy úlohy.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-133">Similarly, if a task runs under the same account that was used for running a job preparation task, the task has read-write access to the job preparation task directory.</span></span> <span data-ttu-id="3a3d1-134">Pokud úloha běží pod jiným účtem než spouštěcí úkol nebo úkol přípravy úlohy, úlohy má přístup jenom pro čtení do příslušného adresáře.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-134">If a task runs under a different account than the start task or job preparation task, then the task has only read access to the respective directory.</span></span>

<span data-ttu-id="3a3d1-135">Další informace o přístup k souborů a adresářů z úlohy najdete v tématu [rozsáhlé paralelní vývoj výpočetní řešení pomocí služby Batch](batch-api-basics.md#files-and-directories).</span><span class="sxs-lookup"><span data-stu-id="3a3d1-135">For more information on accessing files and directories from a task, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#files-and-directories).</span></span>

## <a name="elevated-access-for-tasks"></a><span data-ttu-id="3a3d1-136">Přístup se zvýšeným oprávněním pro úlohy</span><span class="sxs-lookup"><span data-stu-id="3a3d1-136">Elevated access for tasks</span></span> 

<span data-ttu-id="3a3d1-137">Uživatelský účet zvýšení úrovně určuje, zda úloha běží se zvýšenými oprávněními přístup.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-137">The user account's elevation level indicates whether a task runs with elevated access.</span></span> <span data-ttu-id="3a3d1-138">Auto uživatelský účet a s názvem uživatelského účtu můžete spustit s oprávněním vyšší úrovně přístupu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-138">Both an auto-user account and a named user account can run with elevated access.</span></span> <span data-ttu-id="3a3d1-139">Jsou dvě možnosti pro zvýšení úrovně:</span><span class="sxs-lookup"><span data-stu-id="3a3d1-139">The two options for elevation level are:</span></span>

- <span data-ttu-id="3a3d1-140">**NonAdmin:** tato úloha se spustí jako standardní uživatel bez přístup se zvýšeným oprávněním.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-140">**NonAdmin:** The task runs as a standard user without elevated access.</span></span> <span data-ttu-id="3a3d1-141">Výchozí úroveň zvýšení oprávnění pro uživatelský účet Batch je vždy **NonAdmin**.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-141">The default elevation level for a Batch user account is always **NonAdmin**.</span></span>
- <span data-ttu-id="3a3d1-142">**Správce:** úloha běží jako uživatel s oprávněním vyšší úrovně přístupu a funguje s úplnými oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-142">**Admin:** The task runs as a user with elevated access and operates with full Administrator permissions.</span></span> 

## <a name="auto-user-accounts"></a><span data-ttu-id="3a3d1-143">Auto uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="3a3d1-143">Auto-user accounts</span></span>

<span data-ttu-id="3a3d1-144">Ve výchozím nastavení úlohy spuštěny v dávce v rámci automatického uživatelský účet, jako standardní uživatel bez přístup se zvýšeným oprávněním a s oborem úloh.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-144">By default, tasks run in Batch under an auto-user account, as a standard user without elevated access, and with task scope.</span></span> <span data-ttu-id="3a3d1-145">V případě specifikace uživatele automaticky nastavena pro obor úloh, služba Batch vytvoří automaticky uživatelský účet pro tuto úlohu jenom.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-145">When the auto-user specification is configured for task scope, the Batch service creates an auto-user account for that task only.</span></span>

<span data-ttu-id="3a3d1-146">Alternativa k oboru úloh je fond oboru.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-146">The alternative to task scope is pool scope.</span></span> <span data-ttu-id="3a3d1-147">Při automatické uživatele specifikace pro úlohy je nakonfigurován pro obor fondu, je úloha spuštěna automaticky uživatelský účet, který je k dispozici pro všechny úlohy ve fondu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-147">When the auto-user specification for a task is configured for pool scope, the task runs under an auto-user account that is available to any task in the pool.</span></span> <span data-ttu-id="3a3d1-148">Další informace o fondu oboru, najdete v části s názvem [spustit úlohu jako uživatel automaticky s oborem fondu](#run-a-task-as-the-autouser-with-pool-scope).</span><span class="sxs-lookup"><span data-stu-id="3a3d1-148">For more information about pool scope, see the section titled [Run a task as the auto-user with pool scope](#run-a-task-as-the-autouser-with-pool-scope).</span></span>   

<span data-ttu-id="3a3d1-149">Výchozí obor se liší v uzlech systému Windows a Linux:</span><span class="sxs-lookup"><span data-stu-id="3a3d1-149">The default scope is different on Windows and Linux nodes:</span></span>

- <span data-ttu-id="3a3d1-150">Na uzlech Windows úlohy spuštěny v rámci oboru úloha ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-150">On Windows nodes, tasks run under task scope by default.</span></span>
- <span data-ttu-id="3a3d1-151">Uzly Linux vždy spuštěný fondu oboru.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-151">Linux nodes always run under pool scope.</span></span>

<span data-ttu-id="3a3d1-152">Existují čtyři možné konfigurace pro uživatele automaticky specifikace, každý z nich odpovídá jedinečný auto uživatelský účet:</span><span class="sxs-lookup"><span data-stu-id="3a3d1-152">There are four possible configurations for the auto-user specification, each of which corresponds to a unique auto-user account:</span></span>

- <span data-ttu-id="3a3d1-153">Přístup bez oprávnění správce s oborem úloh (specifikace uživatele automaticky výchozí)</span><span class="sxs-lookup"><span data-stu-id="3a3d1-153">Non-admin access with task scope (the default auto-user specification)</span></span>
- <span data-ttu-id="3a3d1-154">Přístup správce (zvýšenými) s oborem úloh</span><span class="sxs-lookup"><span data-stu-id="3a3d1-154">Admin (elevated) access with task scope</span></span>
- <span data-ttu-id="3a3d1-155">Přístup bez oprávnění správce s oborem fondu</span><span class="sxs-lookup"><span data-stu-id="3a3d1-155">Non-admin access with pool scope</span></span>
- <span data-ttu-id="3a3d1-156">Přístup správce k oboru fondu</span><span class="sxs-lookup"><span data-stu-id="3a3d1-156">Admin access with pool scope</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="3a3d1-157">Úkoly spuštěné v rámci oboru úloh nemají fakticky přístup k jiné úlohy na uzlu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-157">Tasks running under task scope do not have de facto access to other tasks on a node.</span></span> <span data-ttu-id="3a3d1-158">Ale uživatel se zlými úmysly s přístupem k účtu může toto omezení obejít tím, že odešlete úlohu, která spustí s oprávněními správce a přistupuje k jiných adresářích úkolu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-158">However, a malicious user with access to the account could work around this restriction by submitting a task that runs with administrator privileges and accesses other task directories.</span></span> <span data-ttu-id="3a3d1-159">Uživatel se zlými úmysly může také pomocí protokolu RDP nebo SSH připojit k uzlu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-159">A malicious user could also use RDP or SSH to connect to a node.</span></span> <span data-ttu-id="3a3d1-160">Je důležité chránit přístup k klíče účtu Batch, aby se zabránilo takové situaci.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-160">It's important to protect access to your Batch account keys to prevent such a scenario.</span></span> <span data-ttu-id="3a3d1-161">Pokud se domníváte, že váš účet ohrožený, nezapomeňte znovu vygenerovat klíče.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-161">If you suspect your account may have been compromised, be sure to regenerate your keys.</span></span>
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a><span data-ttu-id="3a3d1-162">Spustit úlohu jako auto uživatel s oprávněním vyšší úrovně přístupu</span><span class="sxs-lookup"><span data-stu-id="3a3d1-162">Run a task as an auto-user with elevated access</span></span>

<span data-ttu-id="3a3d1-163">Když potřebujete spuštění úlohy se zvýšenými oprávněními přístupu můžete nakonfigurovat specifikace automaticky uživatel oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-163">You can configure the auto-user specification for administrator privileges when you need to run a task with elevated access.</span></span> <span data-ttu-id="3a3d1-164">Spouštěcí úkol může například potřebovat přístup se zvýšeným oprávněním k instalaci softwaru na uzlu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-164">For example, a start task may need elevated access to install software on the node.</span></span>

> [!NOTE] 
> <span data-ttu-id="3a3d1-165">Obecně platí je nejvhodnější použít přístup se zvýšeným oprávněním pouze v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-165">In general, it's best to use elevated access only when necessary.</span></span> <span data-ttu-id="3a3d1-166">Doporučeným udělení minimální oprávnění potřebná k dosažení požadovaném výsledku.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-166">Best practices recommend granting the minimum privilege necessary to achieve the desired outcome.</span></span> <span data-ttu-id="3a3d1-167">Například pokud spouštěcí úkol nainstaluje software pro aktuálního uživatele, ne pro všechny uživatele, možná nebudete moct Vyhněte se udělování zvýšenými přístup k úlohám.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-167">For example, if a start task installs software for the current user, instead of for all users, you may be able to avoid granting elevated access to tasks.</span></span> <span data-ttu-id="3a3d1-168">Můžete nakonfigurovat specifikace automaticky uživatele pro přístup fondu oboru a bez oprávnění správce pro všechny úlohy, které je třeba spustit pod stejným účtem, včetně spouštěcího úkolu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-168">You can configure the auto-user specification for pool scope and non-admin access for all tasks that need to run under the same account, including the start task.</span></span> 
>
>

<span data-ttu-id="3a3d1-169">Následující fragmenty kódu ukazují, jak nakonfigurovat specifikace uživatele automaticky.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-169">The following code snippets show how to configure the auto-user specification.</span></span> <span data-ttu-id="3a3d1-170">Příklady nastavit zvýšení oprávnění na úrovni `Admin` a obor pro `Task`.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-170">The examples set the elevation level to `Admin` and the scope to `Task`.</span></span> <span data-ttu-id="3a3d1-171">Úloha obor je výchozí nastavení, ale je zde uveden z důvodu příklad.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-171">Task scope is the default setting, but is included here for the sake of example.</span></span>

#### <a name="batch-net"></a><span data-ttu-id="3a3d1-172">Batch .NET</span><span class="sxs-lookup"><span data-stu-id="3a3d1-172">Batch .NET</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a><span data-ttu-id="3a3d1-173">Batch Java</span><span class="sxs-lookup"><span data-stu-id="3a3d1-173">Batch Java</span></span>

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a><span data-ttu-id="3a3d1-174">Batch Python</span><span class="sxs-lookup"><span data-stu-id="3a3d1-174">Batch Python</span></span>

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

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a><span data-ttu-id="3a3d1-175">Spustit úlohu jako uživatelé automaticky s oborem fondu</span><span class="sxs-lookup"><span data-stu-id="3a3d1-175">Run a task as an auto-user with pool scope</span></span>

<span data-ttu-id="3a3d1-176">Pokud uzel je zřízený, dvě celou fondu automaticky uživatelské účty jsou vytvořeny na každém uzlu ve fondu, s oprávněním vyšší úrovně přístupu a jeden bez přístup se zvýšeným oprávněním.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-176">When a node is provisioned, two pool-wide auto-user accounts are created on each node in the pool, one with elevated access, and one without elevated access.</span></span> <span data-ttu-id="3a3d1-177">Nastavení oboru uživatele automaticky do fondu rozsahu pro danou úlohu spustí úlohu v rámci jednoho z těchto dvou celou fondu automaticky uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-177">Setting the auto-user's scope to pool scope for a given task runs the task under one of these two pool-wide auto-user accounts.</span></span> 

<span data-ttu-id="3a3d1-178">Pokud zadáte rozsah fondu pro všechny úlohy, které spustí s přístupem správce spustit pod stejným účtem auto uživatele v celé fondu automaticky uživatel.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-178">When you specify pool scope for the auto-user, all tasks that run with administrator access run under the same pool-wide auto-user account.</span></span> <span data-ttu-id="3a3d1-179">Podobně úlohy, které běží bez oprávnění správce také spouštět pod účtem jednoho fondu celou auto uživatele.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-179">Similarly, tasks that run without administrator permissions also run under a single pool-wide auto-user account.</span></span> 

> [!NOTE] 
> <span data-ttu-id="3a3d1-180">Dva účty auto uživatele v celé fondu jsou samostatné účty.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-180">The two pool-wide auto-user accounts are separate accounts.</span></span> <span data-ttu-id="3a3d1-181">Úlohy spuštěné v rámci účtu správce fondu celou nelze sdílet data s úkoly spuštěné pod účtem, standard a naopak.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-181">Tasks running under the pool-wide administrative account cannot share data with tasks running under the standard account, and vice versa.</span></span> 
>
>

<span data-ttu-id="3a3d1-182">Výhoda spuštění pod stejnou auto uživatelský účet je moci sdílet data s další úkoly spuštěné na stejném uzlu úlohy.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-182">The advantage to running under the same auto-user account is that tasks are able to share data with other tasks running on the same node.</span></span>

<span data-ttu-id="3a3d1-183">Jeden scénář, kde spuštěné úkoly v jednom ze dvou celou fondu automaticky uživatelské účty je užitečná sdílení tajných klíčů mezi úlohy je.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-183">Sharing secrets between tasks is one scenario where running tasks under one of the two pool-wide auto-user accounts is useful.</span></span> <span data-ttu-id="3a3d1-184">Předpokládejme například, že spouštěcí úkol potřebuje ke zřízení tajný klíč do uzlu, který můžete použít jiné úlohy.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-184">For example, suppose a start task needs to provision a secret onto the node that other tasks can use.</span></span> <span data-ttu-id="3a3d1-185">Můžete použít rozhraní Windows Data Protection API (DPAPI), ale vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-185">You could use the Windows Data Protection API (DPAPI), but it requires administrator privileges.</span></span> <span data-ttu-id="3a3d1-186">Místo toho můžete chránit tajný klíč na úrovni uživatele.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-186">Instead, you can protect the secret at the user level.</span></span> <span data-ttu-id="3a3d1-187">Úkoly spuštěné pod stejným účtem uživatele mají přístup k tajný klíč bez přístup se zvýšeným oprávněním.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-187">Tasks running under the same user account can access the secret without elevated access.</span></span>

<span data-ttu-id="3a3d1-188">Další možností místo spouštět úlohy v části Automatické uživatelský účet s oborem fondu je rozhraní MPI (Message Passing) sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-188">Another scenario where you may want to run tasks under an auto-user account with pool scope is a Message Passing Interface (MPI) file share.</span></span> <span data-ttu-id="3a3d1-189">MPI sdílené složky je užitečné, když uzlů, úlohy MPI potřebují spolupracovat na stejné datové soubory.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-189">An MPI file share is useful when the nodes in the MPI task need to work on the same file data.</span></span> <span data-ttu-id="3a3d1-190">Z hlavního uzlu vytvoří složku, která podřízených uzlů může získat přístup, pokud běží pod stejným účtem uživatele automaticky.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-190">The head node creates a file share that the child nodes can access if they are running under the same auto-user account.</span></span> 

<span data-ttu-id="3a3d1-191">Následující fragment kódu oboru uživatele automaticky nastaví na rozsah fondu pro úlohu v Batch .NET.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-191">The following code snippet sets the auto-user's scope to pool scope for a task in Batch .NET.</span></span> <span data-ttu-id="3a3d1-192">Zvýšení úrovně je vynechán, proto je úloha spuštěna standardní úrovni fondu automaticky uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-192">The elevation level is omitted, so the task runs under the standard pool-wide auto-user account.</span></span>

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a><span data-ttu-id="3a3d1-193">S názvem uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="3a3d1-193">Named user accounts</span></span>

<span data-ttu-id="3a3d1-194">Při vytváření fondu můžete definovat pojmenované uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-194">You can define named user accounts when you create a pool.</span></span> <span data-ttu-id="3a3d1-195">Pojmenované uživatelský účet má název a heslo, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-195">A named user account has a name and password that you provide.</span></span> <span data-ttu-id="3a3d1-196">Můžete zadat úroveň zvýšení oprávnění pro účet s názvem uživatele.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-196">You can specify the elevation level for a named user account.</span></span> <span data-ttu-id="3a3d1-197">Pro Linux uzly můžete zadat taky privátní klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-197">For Linux nodes, you can also provide an SSH private key.</span></span>

<span data-ttu-id="3a3d1-198">Pojmenované uživatelský účet existuje na všech uzlech ve fondu a je k dispozici pro všechny úkoly spuštěné na těchto uzlech.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-198">A named user account exists on all nodes in the pool and is available to all tasks running on those nodes.</span></span> <span data-ttu-id="3a3d1-199">Můžete definovat libovolný počet jmenovaní uživatelé pro fond.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-199">You may define any number of named users for a pool.</span></span> <span data-ttu-id="3a3d1-200">Když přidáte úkolu nebo kolekce úloh, můžete zadat, že tato úloha se spustí v rámci jednoho z pojmenované uživatelské účty definované ve fondu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-200">When you add a task or task collection, you can specify that the task runs under one of the named user accounts defined on the pool.</span></span>

<span data-ttu-id="3a3d1-201">S názvem uživatelského účtu je užitečné, když chcete spustit všechny úlohy pro úlohu pod stejným účtem uživatele, ale je izolovat z úloh spuštěných ve jiné úlohy ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-201">A named user account is useful when you want to run all tasks in a job under the same user account, but isolate them from tasks running in other jobs at the same time.</span></span> <span data-ttu-id="3a3d1-202">Můžete například vytvořit pojmenovanému uživateli pro každou úlohu a spouštět úlohy každou úlohu v části s názvem uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-202">For example, you can create a named user for each job, and run each job's tasks under that named user account.</span></span> <span data-ttu-id="3a3d1-203">Každá úloha sdílet tajný klíč s její vlastní úkoly, ale nikoli s úkoly spuštěné v jiné úlohy.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-203">Each job can then share a secret with its own tasks, but not with tasks running in other jobs.</span></span>

<span data-ttu-id="3a3d1-204">S názvem uživatelského účtu můžete taky spustit úlohu, která nastaví oprávnění na externím prostředkům, jako jsou sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-204">You can also use a named user account to run a task that sets permissions on external resources such as file shares.</span></span> <span data-ttu-id="3a3d1-205">Pomocí pojmenovaného uživatelského účtu řídit identity uživatele a můžete použít tuto identitu uživatele a nastavit oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-205">With a named user account, you control the user identity and can use that user identity to set permissions.</span></span>  

<span data-ttu-id="3a3d1-206">S názvem uživatelské účty umožňují bez heslo SSH mezi uzly Linux.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-206">Named user accounts enable password-less SSH between Linux nodes.</span></span> <span data-ttu-id="3a3d1-207">Můžete vytvořit s názvem uživatelského účtu s Linux uzly, které je potřeba spustit úkoly s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-207">You can use a named user account with Linux nodes that need to run multi-instance tasks.</span></span> <span data-ttu-id="3a3d1-208">Každý uzel ve fondu můžete spouštět úlohy pod uživatelským účtem definované na celý fond.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-208">Each node in the pool can run tasks under a user account defined on the whole pool.</span></span> <span data-ttu-id="3a3d1-209">Další informace o úkoly s více instancemi najdete v tématu [použít více\-instance úlohy ke spuštění aplikací MPI](batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="3a3d1-209">For more information about multi-instance tasks, see [Use multi\-instance tasks to run MPI applications](batch-mpi.md).</span></span>

### <a name="create-named-user-accounts"></a><span data-ttu-id="3a3d1-210">Vytvořit s názvem uživatelské účty</span><span class="sxs-lookup"><span data-stu-id="3a3d1-210">Create named user accounts</span></span>

<span data-ttu-id="3a3d1-211">Pokud chcete vytvořit s názvem uživatelské účty ve službě Batch, přidejte do fondu kolekce uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-211">To create named user accounts in Batch, add a collection of user accounts to the pool.</span></span> <span data-ttu-id="3a3d1-212">Následující fragmenty kódu ukazují, jak můžete vytvořit s názvem uživatelské účty v rozhraní .NET, Java a Python.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-212">The following code snippets show how to create named user accounts in .NET, Java, and Python.</span></span> <span data-ttu-id="3a3d1-213">Tyto fragmenty kódu ukazují, jak vytvořit správce i bez oprávnění správce. s názvem účty ve fondu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-213">These code snippets show how to create both admin and non-admin named accounts on a pool.</span></span> <span data-ttu-id="3a3d1-214">Příklady vytvořit fondy pomocí konfigurace cloudové služby, ale můžete použít ve stejný přístup při vytváření fondu systému Windows nebo Linux pomocí konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-214">The examples create pools using the cloud service configuration, but you use the same approach when creating a Windows or Linux pool using the virtual machine configuration.</span></span>

#### <a name="batch-net-example-windows"></a><span data-ttu-id="3a3d1-215">Příklad batch .NET (Windows)</span><span class="sxs-lookup"><span data-stu-id="3a3d1-215">Batch .NET example (Windows)</span></span>

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using the cloud service configuration.
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

// Commit the pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a><span data-ttu-id="3a3d1-216">Příklad batch .NET (Linux)</span><span class="sxs-lookup"><span data-stu-id="3a3d1-216">Batch .NET example (Linux)</span></span>

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the virtual machine configuration to use to create the pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create the unbound pool.
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

// Commit the pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a><span data-ttu-id="3a3d1-217">Příklad Java batch</span><span class="sxs-lookup"><span data-stu-id="3a3d1-217">Batch Java example</span></span>

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

#### <a name="batch-python-example"></a><span data-ttu-id="3a3d1-218">Příklad batch Python</span><span class="sxs-lookup"><span data-stu-id="3a3d1-218">Batch Python example</span></span>

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

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a><span data-ttu-id="3a3d1-219">Spustit úlohu v části s názvem uživatelského účtu s oprávněním vyšší úrovně přístupu</span><span class="sxs-lookup"><span data-stu-id="3a3d1-219">Run a task under a named user account with elevated access</span></span>

<span data-ttu-id="3a3d1-220">Chcete-li spustit úlohu jako uživatel s oprávněním vyšší úrovně, nastavte úkolu **identity uživatele** vlastnost s názvem uživatelského účtu, který byl vytvořen s jeho **ElevationLevel** vlastnost nastavena na hodnotu `Admin`.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-220">To run a task as an elevated user, set the task's **UserIdentity** property to a named user account that was created with its **ElevationLevel** property set to `Admin`.</span></span>

<span data-ttu-id="3a3d1-221">Tento fragment kódu určuje, zda má být úloha spuštěna pod účtem s názvem uživatele.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-221">This code snippet specifies that the task should run under a named user account.</span></span> <span data-ttu-id="3a3d1-222">Tento účet pojmenovanému uživateli definované ve fondu při vytvoření fondu.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-222">This named user account was defined on the pool when the pool was created.</span></span> <span data-ttu-id="3a3d1-223">V takovém případě byl vytvořen s názvem uživatelský účet s oprávněními správce:</span><span class="sxs-lookup"><span data-stu-id="3a3d1-223">In this case, the named user account was created with admin permissions:</span></span>

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-to-the-latest-batch-client-library"></a><span data-ttu-id="3a3d1-224">Aktualizovat na nejnovější klientské knihovny Batch kódu</span><span class="sxs-lookup"><span data-stu-id="3a3d1-224">Update your code to the latest Batch client library</span></span>

<span data-ttu-id="3a3d1-225">Verze služby Batch 2017-01-01.4.0 zavádí narušující změně nahrazení **runElevated** vlastnost k dispozici v dřívějších verzích se **userIdentity** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-225">The Batch service version 2017-01-01.4.0 introduces a breaking change, replacing the **runElevated** property available in earlier versions with the **userIdentity** property.</span></span> <span data-ttu-id="3a3d1-226">V následujících tabulkách jsou jednoduché mapování, která můžete použít k aktualizaci kódu z dřívějších verzí knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-226">The following tables provide a simple mapping that you can use to update your code from earlier versions of the client libraries.</span></span>

### <a name="batch-net"></a><span data-ttu-id="3a3d1-227">Batch .NET</span><span class="sxs-lookup"><span data-stu-id="3a3d1-227">Batch .NET</span></span>

| <span data-ttu-id="3a3d1-228">Pokud váš kód používá...</span><span class="sxs-lookup"><span data-stu-id="3a3d1-228">If your code uses...</span></span>                  | <span data-ttu-id="3a3d1-229">Aktualizujte, aby...</span><span class="sxs-lookup"><span data-stu-id="3a3d1-229">Update it to....</span></span>                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| <span data-ttu-id="3a3d1-230">`CloudTask.RunElevated`není zadaná.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-230">`CloudTask.RunElevated` not specified</span></span> | <span data-ttu-id="3a3d1-231">Bez aktualizace</span><span class="sxs-lookup"><span data-stu-id="3a3d1-231">No update required</span></span>                                                                                               |

### <a name="batch-java"></a><span data-ttu-id="3a3d1-232">Batch Java</span><span class="sxs-lookup"><span data-stu-id="3a3d1-232">Batch Java</span></span>

| <span data-ttu-id="3a3d1-233">Pokud váš kód používá...</span><span class="sxs-lookup"><span data-stu-id="3a3d1-233">If your code uses...</span></span>                      | <span data-ttu-id="3a3d1-234">Aktualizujte, aby...</span><span class="sxs-lookup"><span data-stu-id="3a3d1-234">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| <span data-ttu-id="3a3d1-235">`CloudTask.withRunElevated`není zadaná.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-235">`CloudTask.withRunElevated` not specified</span></span> | <span data-ttu-id="3a3d1-236">Bez aktualizace</span><span class="sxs-lookup"><span data-stu-id="3a3d1-236">No update required</span></span>                                                                                                                     |

### <a name="batch-python"></a><span data-ttu-id="3a3d1-237">Batch Python</span><span class="sxs-lookup"><span data-stu-id="3a3d1-237">Batch Python</span></span>

| <span data-ttu-id="3a3d1-238">Pokud váš kód používá...</span><span class="sxs-lookup"><span data-stu-id="3a3d1-238">If your code uses...</span></span>                      | <span data-ttu-id="3a3d1-239">Aktualizujte, aby...</span><span class="sxs-lookup"><span data-stu-id="3a3d1-239">Update it to....</span></span>                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | <span data-ttu-id="3a3d1-240">`user_identity=user`, kde</span><span class="sxs-lookup"><span data-stu-id="3a3d1-240">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | <span data-ttu-id="3a3d1-241">`user_identity=user`, kde</span><span class="sxs-lookup"><span data-stu-id="3a3d1-241">`user_identity=user`, where</span></span> <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| <span data-ttu-id="3a3d1-242">`run_elevated`není zadaná.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-242">`run_elevated` not specified</span></span> | <span data-ttu-id="3a3d1-243">Bez aktualizace</span><span class="sxs-lookup"><span data-stu-id="3a3d1-243">No update required</span></span>                                                                                                                                  |


## <a name="next-steps"></a><span data-ttu-id="3a3d1-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a3d1-244">Next steps</span></span>

### <a name="batch-forum"></a><span data-ttu-id="3a3d1-245">Fórum batch</span><span class="sxs-lookup"><span data-stu-id="3a3d1-245">Batch Forum</span></span>

<span data-ttu-id="3a3d1-246">[Fóru služby Azure Batch](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) na webu MSDN je skvělým místem popisují Batch a klást otázky týkající se služby.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-246">The [Azure Batch Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) on MSDN is a great place to discuss Batch and ask questions about the service.</span></span> <span data-ttu-id="3a3d1-247">HEAD na přes pro užitečné definovaného příspěvky a při jejich vzniku při sestavování řešení Batch zveřejněte svoje otázky.</span><span class="sxs-lookup"><span data-stu-id="3a3d1-247">Head on over for helpful pinned posts, and post your questions as they arise while you build your Batch solutions.</span></span>