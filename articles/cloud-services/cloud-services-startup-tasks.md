---
title: "Spustit spuštění úlohy v cloudových služeb Azure | Microsoft Docs"
description: "Spuštění úlohy pomůže připravit vaše prostředí cloudové služby pro vaši aplikaci. To se naučíte, jak fungují spuštění úlohy a postupy, aby byly"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 886939be-4b5b-49cc-9a6e-2172e3c133e9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 1c1b3aa86dc8211de0c07c9fb68da5685c86f551
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a><span data-ttu-id="ac760-104">Jak nakonfigurovat a spustit úlohy spuštění pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="ac760-104">How to configure and run startup tasks for a cloud service</span></span>
<span data-ttu-id="ac760-105">Spuštění úlohy můžete použít k provádění operací před zahájením roli.</span><span class="sxs-lookup"><span data-stu-id="ac760-105">You can use startup tasks to perform operations before a role starts.</span></span> <span data-ttu-id="ac760-106">Operace, které můžete chtít provést zahrnovat instalaci komponenty, registraci komponenty modelu COM, nastavení klíče registru nebo dlouhotrvající proces.</span><span class="sxs-lookup"><span data-stu-id="ac760-106">Operations that you might want to perform include installing a component, registering COM components, setting registry keys, or starting a long running process.</span></span>

> [!NOTE]
> <span data-ttu-id="ac760-107">Spuštění úlohy se nedají použít k virtuálním počítačům, pouze pro webové služby Cloud a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="ac760-107">Startup tasks are not applicable to Virtual Machines, only to Cloud Service Web and Worker roles.</span></span>
> 
> 

## <a name="how-startup-tasks-work"></a><span data-ttu-id="ac760-108">Jak fungují spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="ac760-108">How startup tasks work</span></span>
<span data-ttu-id="ac760-109">Spuštění úlohy se akcí, které před zahájením role a jsou definovány v [ServiceDefinition.csdef] soubor pomocí [úloh] v rámci [spuštění] element.</span><span class="sxs-lookup"><span data-stu-id="ac760-109">Startup tasks are actions that are taken before your roles begin and are defined in the [ServiceDefinition.csdef] file by using the [Task] element within the [Startup] element.</span></span> <span data-ttu-id="ac760-110">Často spuštění úlohy jsou dávkové soubory, ale lze je také konzolové aplikace nebo dávkové soubory, které se spouštějí skripty prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ac760-110">Frequently startup tasks are batch files, but they can also be console applications, or batch files that start PowerShell scripts.</span></span>

<span data-ttu-id="ac760-111">Proměnné prostředí předání informací do úloha spuštění a místní úložiště slouží k předávání informací mimo spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="ac760-111">Environment variables pass information into a startup task, and local storage can be used to pass information out of a startup task.</span></span> <span data-ttu-id="ac760-112">Například proměnná prostředí můžete zadat cestu k programu, který chcete nainstalovat, a lze zapisovat soubory do místního úložiště, které lze poté číst později vaše role.</span><span class="sxs-lookup"><span data-stu-id="ac760-112">For example, an environment variable can specify the path to a program you want to install, and files can be written to local storage that can then be read later by your roles.</span></span>

<span data-ttu-id="ac760-113">Spuštění úkolu můžete informace a protokolu chyb do adresáře určeného **TEMP** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac760-113">Your startup task can log information and errors to the directory specified by the **TEMP** environment variable.</span></span> <span data-ttu-id="ac760-114">Při spuštění úlohy **TEMP** proměnnou prostředí přeloží na *C:\\prostředky\\temp\\[identifikátor guid]. [[ Rolename]\\RoleTemp* directory při spuštění v cloudu.</span><span class="sxs-lookup"><span data-stu-id="ac760-114">During the startup task, the **TEMP** environment variable resolves to the *C:\\Resources\\temp\\[guid].[rolename]\\RoleTemp* directory when running on the cloud.</span></span>

<span data-ttu-id="ac760-115">Spuštění úlohy lze také spustit několikrát mezi jednotlivými restartováními.</span><span class="sxs-lookup"><span data-stu-id="ac760-115">Startup tasks can also be executed several times between reboots.</span></span> <span data-ttu-id="ac760-116">Například úloha spuštění spustí pokaždé, když recykluje roli a roli recykluje nemusí vždy zahrnovat restartování.</span><span class="sxs-lookup"><span data-stu-id="ac760-116">For example, the startup task will be run each time the role recycles, and role recycles may not always include a reboot.</span></span> <span data-ttu-id="ac760-117">Spuštění úlohy budou zasílány způsobem, který umožňuje, aby se spouštěly několikrát bez problémů.</span><span class="sxs-lookup"><span data-stu-id="ac760-117">Startup tasks should be written in a way that allows them to run several times without problems.</span></span>

<span data-ttu-id="ac760-118">Spuštění úlohy musí končit **errorlevel** (nebo ukončovací kód) nula na dokončení procesu spuštění.</span><span class="sxs-lookup"><span data-stu-id="ac760-118">Startup tasks must end with an **errorlevel** (or exit code) of zero for the startup process to complete.</span></span> <span data-ttu-id="ac760-119">Pokud úloha spuštění končí nenulovou **errorlevel**, role se nespustí.</span><span class="sxs-lookup"><span data-stu-id="ac760-119">If a startup task ends with a non-zero **errorlevel**, the role will not start.</span></span>

## <a name="role-startup-order"></a><span data-ttu-id="ac760-120">Pořadí spouštění role</span><span class="sxs-lookup"><span data-stu-id="ac760-120">Role startup order</span></span>
<span data-ttu-id="ac760-121">Postup spuštění role v Azure jsou následující:</span><span class="sxs-lookup"><span data-stu-id="ac760-121">The following lists the role startup procedure in Azure:</span></span>

1. <span data-ttu-id="ac760-122">Instance je označena jako **počáteční** a nepřijímá provoz.</span><span class="sxs-lookup"><span data-stu-id="ac760-122">The instance is marked as **Starting** and does not receive traffic.</span></span>
2. <span data-ttu-id="ac760-123">Všechny úlohy spuštění jsou spouštěny podle jejich **taskType** atribut.</span><span class="sxs-lookup"><span data-stu-id="ac760-123">All startup tasks are executed according to their **taskType** attribute.</span></span>
   
   * <span data-ttu-id="ac760-124">**Jednoduché** synchronně, provedení úloh po jednom.</span><span class="sxs-lookup"><span data-stu-id="ac760-124">The **simple** tasks are executed synchronously, one at a time.</span></span>
   * <span data-ttu-id="ac760-125">**Pozadí** a **popředí** úkoly spouštějí asynchronně, paralelní spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="ac760-125">The **background** and **foreground** tasks are started asynchronously, parallel to the startup task.</span></span>  
     
     > [!WARNING]
     > <span data-ttu-id="ac760-126">Služba IIS nemusí být plně nakonfigurované během fáze spuštění úloh v procesu spuštění proto role specifických dat pravděpodobně není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ac760-126">IIS may not be fully configured during the startup task stage in the startup process, so role-specific data may not be available.</span></span> <span data-ttu-id="ac760-127">Spuštění úlohy, které vyžadují specifickou rolí dat využít [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac760-127">Startup tasks that require role-specific data should use [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span></span>
     > 
     > 
3. <span data-ttu-id="ac760-128">Spuštění procesu role hostitele a vytvoření webu ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="ac760-128">The role host process is started and the site is created in IIS.</span></span>
4. <span data-ttu-id="ac760-129">[Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="ac760-129">The [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method is called.</span></span>
5. <span data-ttu-id="ac760-130">Instance je označena jako **připraven** a provoz se směruje na instanci.</span><span class="sxs-lookup"><span data-stu-id="ac760-130">The instance is marked as **Ready** and traffic is routed to the instance.</span></span>
6. <span data-ttu-id="ac760-131">[Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="ac760-131">The [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method is called.</span></span>

## <a name="example-of-a-startup-task"></a><span data-ttu-id="ac760-132">Příklad spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="ac760-132">Example of a startup task</span></span>
<span data-ttu-id="ac760-133">Spuštění úlohy jsou definovány v [ServiceDefinition.csdef] v souboru **úloh** elementu.</span><span class="sxs-lookup"><span data-stu-id="ac760-133">Startup tasks are defined in the [ServiceDefinition.csdef] file, in the **Task** element.</span></span> <span data-ttu-id="ac760-134">**CommandLine** atribut určuje název a parametry spuštění dávkového souboru nebo konzoly příkazu **executionContext** atribut určuje úroveň oprávnění při spuštění úlohy a **taskType** atribut určuje, jak se úlohu spustit.</span><span class="sxs-lookup"><span data-stu-id="ac760-134">The **commandLine** attribute specifies the name and parameters of the startup batch file or console command, the **executionContext** attribute specifies the privilege level of the startup task, and the **taskType** attribute specifies how the task will be executed.</span></span>

<span data-ttu-id="ac760-135">V tomto příkladu se proměnná prostředí **MyVersionNumber**, se vytvoří pro spuštění úlohy a nastaven na hodnotu "**1.0.0.0**".</span><span class="sxs-lookup"><span data-stu-id="ac760-135">In this example, an environment variable, **MyVersionNumber**, is created for the startup task and set to the value "**1.0.0.0**".</span></span>

<span data-ttu-id="ac760-136">**ServiceDefinition.csdef**:</span><span class="sxs-lookup"><span data-stu-id="ac760-136">**ServiceDefinition.csdef**:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

<span data-ttu-id="ac760-137">V následujícím příkladu **Startup.cmd** dávkový soubor zapíše řádek "aktuální verze je 1.0.0.0" do souboru StartupLog.txt v adresáři zadaném proměnnou prostředí TEMP.</span><span class="sxs-lookup"><span data-stu-id="ac760-137">In the following example, the **Startup.cmd** batch file writes the line "The current version is 1.0.0.0" to the StartupLog.txt file in the directory specified by the TEMP environment variable.</span></span> <span data-ttu-id="ac760-138">`EXIT /B 0` Řádku zajišťuje, že úloha spuštění končí **errorlevel** nula.</span><span class="sxs-lookup"><span data-stu-id="ac760-138">The `EXIT /B 0` line ensures that the startup task ends with an **errorlevel** of zero.</span></span>

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> <span data-ttu-id="ac760-139">V sadě Visual Studio **kopírovat do výstupního adresáře** vlastnost pro váš dávkový soubor spuštění musí být nastavená na **kopie vždy** a zajistit, aby byl váš dávkový soubor spuštění správně nasazen do projektu v Azure (**approot\\bin** pro webové role a **approot** pro role pracovního procesu).</span><span class="sxs-lookup"><span data-stu-id="ac760-139">In Visual Studio, the **Copy to Output Directory** property for your startup batch file should be set to **Copy Always** to be sure that your startup batch file is properly deployed to your project on Azure (**approot\\bin** for Web roles, and **approot** for worker roles).</span></span>
> 
> 

## <a name="description-of-task-attributes"></a><span data-ttu-id="ac760-140">Popis úloh atributů</span><span class="sxs-lookup"><span data-stu-id="ac760-140">Description of task attributes</span></span>
<span data-ttu-id="ac760-141">Následující text popisuje atributy **úloh** element v [ServiceDefinition.csdef] souboru:</span><span class="sxs-lookup"><span data-stu-id="ac760-141">The following describes the attributes of the **Task** element in the [ServiceDefinition.csdef] file:</span></span>

<span data-ttu-id="ac760-142">**commandLine** -určuje příkazový řádek pro spuštění úlohy:</span><span class="sxs-lookup"><span data-stu-id="ac760-142">**commandLine** - Specifies the command line for the startup task:</span></span>

* <span data-ttu-id="ac760-143">Příkaz, s parametry volitelné příkazového řádku, které zahájí spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="ac760-143">The command, with optional command line parameters, which begins the startup task.</span></span>
* <span data-ttu-id="ac760-144">Často to je název souboru dávkový soubor cmd nebo BAT.</span><span class="sxs-lookup"><span data-stu-id="ac760-144">Frequently this is the filename of a .cmd or .bat batch file.</span></span>
* <span data-ttu-id="ac760-145">Tato úloha je relativní vzhledem k AppRoot\\složky Koš služby pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="ac760-145">The task is relative to the AppRoot\\Bin folder for the deployment.</span></span> <span data-ttu-id="ac760-146">Při určení cesty a souboru úlohy nejsou rozbalit proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac760-146">Environment variables are not expanded in determining the path and file of the task.</span></span> <span data-ttu-id="ac760-147">Pokud rozšíření prostředí je potřeba, můžete vytvořit malé .cmd skript, který volá spuštění úkolu.</span><span class="sxs-lookup"><span data-stu-id="ac760-147">If environment expansion is required, you can create a small .cmd script that calls your startup task.</span></span>
* <span data-ttu-id="ac760-148">Může být konzolovou aplikaci nebo dávkového souboru, který začíná [skript prostředí PowerShell](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span><span class="sxs-lookup"><span data-stu-id="ac760-148">Can be a console application or a batch file that starts a [PowerShell script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span></span>

<span data-ttu-id="ac760-149">**executionContext** -určuje úroveň oprávnění pro spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="ac760-149">**executionContext** - Specifies the privilege level for the startup task.</span></span> <span data-ttu-id="ac760-150">Úroveň oprávnění může být omezené nebo se zvýšenými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="ac760-150">The privilege level can be limited or elevated:</span></span>

* <span data-ttu-id="ac760-151">**omezená**</span><span class="sxs-lookup"><span data-stu-id="ac760-151">**limited**</span></span>  
  <span data-ttu-id="ac760-152">Úloha spuštění běží se stejnými oprávněními jako roli.</span><span class="sxs-lookup"><span data-stu-id="ac760-152">The startup task runs with the same privileges as the role.</span></span> <span data-ttu-id="ac760-153">Při **executionContext** atribut pro [Runtime] element je také **omezené**, jsou použita uživatelská oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ac760-153">When the **executionContext** attribute for the [Runtime] element is also **limited**, then user privileges are used.</span></span>
* <span data-ttu-id="ac760-154">**zvýšené**</span><span class="sxs-lookup"><span data-stu-id="ac760-154">**elevated**</span></span>  
  <span data-ttu-id="ac760-155">Spuštění úlohy se spustí s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="ac760-155">The startup task runs with administrator privileges.</span></span> <span data-ttu-id="ac760-156">To umožňuje spouštění úloh k instalaci programy, udělat změny konfigurace služby IIS, provedení změn v registru a další úkoly úrovni správce bez zvýšení úrovně oprávnění role sám sebe.</span><span class="sxs-lookup"><span data-stu-id="ac760-156">This allows startup tasks to install programs, make IIS configuration changes, perform registry changes, and other administrator level tasks, without increasing the privilege level of the role itself.</span></span>  

> [!NOTE]
> <span data-ttu-id="ac760-157">Úroveň oprávnění při spuštění úlohy, která nemusí být stejný jako roli sám sebe.</span><span class="sxs-lookup"><span data-stu-id="ac760-157">The privilege level of a startup task does not need to be the same as the role itself.</span></span>
> 
> 

<span data-ttu-id="ac760-158">**taskType** -určuje způsob, jakým se spustí úloha spuštění.</span><span class="sxs-lookup"><span data-stu-id="ac760-158">**taskType** - Specifies the way a startup task is executed.</span></span>

* <span data-ttu-id="ac760-159">**jednoduché**</span><span class="sxs-lookup"><span data-stu-id="ac760-159">**simple**</span></span>  
  <span data-ttu-id="ac760-160">Úlohy jsou spouštěny synchronně, jeden po druhém, v pořadí zadaném v [ServiceDefinition.csdef] souboru.</span><span class="sxs-lookup"><span data-stu-id="ac760-160">Tasks are executed synchronously, one at a time, in the order specified in the [ServiceDefinition.csdef] file.</span></span> <span data-ttu-id="ac760-161">Pokud jeden **jednoduché** končí úloha spuštění **errorlevel** nulové hodnoty, další **jednoduché** spuštění úlohy je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="ac760-161">When one **simple** startup task ends with an **errorlevel** of zero, the next **simple** startup task is executed.</span></span> <span data-ttu-id="ac760-162">Pokud neexistují žádné další **jednoduché** spuštění úloh provést, pak bude spuštěn roli sám sebe.</span><span class="sxs-lookup"><span data-stu-id="ac760-162">If there are no more **simple** startup tasks to execute, then the role itself will be started.</span></span>   
  
  > [!NOTE]
  > <span data-ttu-id="ac760-163">Pokud **jednoduché** úloh končí nenulovou **errorlevel**, instance se zablokuje.</span><span class="sxs-lookup"><span data-stu-id="ac760-163">If the **simple** task ends with a non-zero **errorlevel**, the instance will be blocked.</span></span> <span data-ttu-id="ac760-164">Následné **jednoduché** spuštění úlohy a role samostatně, se nespustí.</span><span class="sxs-lookup"><span data-stu-id="ac760-164">Subsequent **simple** startup tasks, and the role itself, will not start.</span></span>
  > 
  > 
  
    <span data-ttu-id="ac760-165">K zajištění, že váš dávkový soubor končí **errorlevel** nulové hodnoty, spusťte příkaz `EXIT /B 0` na konci souboru dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="ac760-165">To ensure that your batch file ends with an **errorlevel** of zero, execute the command `EXIT /B 0` at the end of your batch file process.</span></span>
* <span data-ttu-id="ac760-166">**pozadí**</span><span class="sxs-lookup"><span data-stu-id="ac760-166">**background**</span></span>  
  <span data-ttu-id="ac760-167">Úlohy se spustí asynchronně, paralelně s spuštění role.</span><span class="sxs-lookup"><span data-stu-id="ac760-167">Tasks are executed asynchronously, in parallel with the startup of the role.</span></span>
* <span data-ttu-id="ac760-168">**popředí**</span><span class="sxs-lookup"><span data-stu-id="ac760-168">**foreground**</span></span>  
  <span data-ttu-id="ac760-169">Úlohy se spustí asynchronně, paralelně s spuštění role.</span><span class="sxs-lookup"><span data-stu-id="ac760-169">Tasks are executed asynchronously, in parallel with the startup of the role.</span></span> <span data-ttu-id="ac760-170">Klíčovým rozdílem mezi **popředí** a **pozadí** úloh je, že **popředí** úloh brání roli z recyklace nebo dokud nebude úloha skončila vypnutí.</span><span class="sxs-lookup"><span data-stu-id="ac760-170">The key difference between a **foreground** and a **background** task is that a **foreground** task prevents the role from recycling or shutting down until the task has ended.</span></span> <span data-ttu-id="ac760-171">**Pozadí** úlohy nemají toto omezení.</span><span class="sxs-lookup"><span data-stu-id="ac760-171">The **background** tasks do not have this restriction.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="ac760-172">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="ac760-172">Environment variables</span></span>
<span data-ttu-id="ac760-173">Proměnné prostředí jsou způsob, jak předat informace o spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="ac760-173">Environment variables are a way to pass information to a startup task.</span></span> <span data-ttu-id="ac760-174">Například můžete vkládat cestu k objektu blob, který obsahuje program pro instalaci, nebo čísla portů, který bude používat vaše role nebo nastavení k řízení funkcí spuštění úkolu.</span><span class="sxs-lookup"><span data-stu-id="ac760-174">For example, you can put the path to a blob that contains a program to install, or port numbers that your role will use, or settings to control features of your startup task.</span></span>

<span data-ttu-id="ac760-175">Existují dva typy proměnných prostředí pro spuštění úlohy; statické proměnné prostředí, proměnné prostředí založené na členy [ RoleEnvironment] třídy.</span><span class="sxs-lookup"><span data-stu-id="ac760-175">There are two kinds of environment variables for startup tasks; static environment variables and environment variables based on members of the [RoleEnvironment] class.</span></span> <span data-ttu-id="ac760-176">Jsou obě [prostředí] části [ServiceDefinition.csdef] souboru a obě použití [proměnné] elementu a **název** atribut.</span><span class="sxs-lookup"><span data-stu-id="ac760-176">Both are in the [Environment] section of the [ServiceDefinition.csdef] file, and both use the [Variable] element and **name** attribute.</span></span>

<span data-ttu-id="ac760-177">Proměnné prostředí statické používá **hodnotu** atribut [proměnné] elementu.</span><span class="sxs-lookup"><span data-stu-id="ac760-177">Static environment variables uses the **value** attribute of the [Variable] element.</span></span> <span data-ttu-id="ac760-178">Tento příklad vytvoří proměnnou prostředí **MyVersionNumber** jehož statickou hodnotu "**1.0.0.0**".</span><span class="sxs-lookup"><span data-stu-id="ac760-178">The example above creates the environment variable **MyVersionNumber** which has a static value of "**1.0.0.0**".</span></span> <span data-ttu-id="ac760-179">Dalším příkladem může být k vytvoření **StagingOrProduction** proměnné prostředí, kterou můžete ručně nastavit na hodnoty "**pracovní**"nebo"**produkční**" k provedení na základě hodnoty na základě různých spouštěcích akce **StagingOrProduction** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="ac760-179">Another example would be to create a **StagingOrProduction** environment variable which you can manually set to values of "**staging**" or "**production**" to perform different startup actions based on the value of the **StagingOrProduction** environment variable.</span></span>

<span data-ttu-id="ac760-180">Nepoužívejte proměnné prostředí založené na členy třídy RoleEnvironment **hodnotu** atribut [proměnné] elementu.</span><span class="sxs-lookup"><span data-stu-id="ac760-180">Environment variables based on members of the RoleEnvironment class do not use the **value** attribute of the [Variable] element.</span></span> <span data-ttu-id="ac760-181">Místo toho [RoleInstanceValue] podřízený element s příslušnou **XPath** hodnota atributu, se používají k vytvoření proměnné prostředí založené na konkrétní členem [ RoleEnvironment] třídy.</span><span class="sxs-lookup"><span data-stu-id="ac760-181">Instead, the [RoleInstanceValue] child element, with the appropriate **XPath** attribute value, are used to create an environment variable based on a specific member of the [RoleEnvironment] class.</span></span> <span data-ttu-id="ac760-182">Hodnoty **XPath** atribut pro přístup k různé [ RoleEnvironment] hodnoty jsou uvedeny [zde](cloud-services-role-config-xpath.md).</span><span class="sxs-lookup"><span data-stu-id="ac760-182">Values for the **XPath** attribute to access various [RoleEnvironment] values can be found [here](cloud-services-role-config-xpath.md).</span></span>

<span data-ttu-id="ac760-183">Chcete-li například vytvořit proměnnou prostředí, která je "**true**" když je spuštěn v emulátoru služby výpočty v, a "**false**" při spuštění v cloudu, použijte následující [proměnné] a [RoleInstanceValue] prvky:</span><span class="sxs-lookup"><span data-stu-id="ac760-183">For example, to create an environment variable that is "**true**" when the instance is running in the compute emulator, and "**false**" when running in the cloud, use the following [Variable] and [RoleInstanceValue] elements:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a><span data-ttu-id="ac760-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac760-184">Next steps</span></span>
<span data-ttu-id="ac760-185">Zjistěte, jak provést některé [běžné úlohy spuštění](cloud-services-startup-tasks-common.md) s Cloudovou službou.</span><span class="sxs-lookup"><span data-stu-id="ac760-185">Learn how to perform some [common startup tasks](cloud-services-startup-tasks-common.md) with your Cloud Service.</span></span>

<span data-ttu-id="ac760-186">[Balíček](cloud-services-model-and-package.md) cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ac760-186">[Package](cloud-services-model-and-package.md) your Cloud Service.</span></span>  

<span data-ttu-id="ac760-187">[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef</span><span class="sxs-lookup"><span data-stu-id="ac760-187">[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef</span></span>
<span data-ttu-id="ac760-188">[úloh]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task</span><span class="sxs-lookup"><span data-stu-id="ac760-188">[Task]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task</span></span>
<span data-ttu-id="ac760-189">[spuštění]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup</span><span class="sxs-lookup"><span data-stu-id="ac760-189">[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup</span></span>
<span data-ttu-id="ac760-190">[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime</span><span class="sxs-lookup"><span data-stu-id="ac760-190">[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime</span></span>
<span data-ttu-id="ac760-191">[prostředí]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment</span><span class="sxs-lookup"><span data-stu-id="ac760-191">[Environment]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment</span></span>
<span data-ttu-id="ac760-192">[proměnné]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable</span><span class="sxs-lookup"><span data-stu-id="ac760-192">[Variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable</span></span>
<span data-ttu-id="ac760-193">[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue</span><span class="sxs-lookup"><span data-stu-id="ac760-193">[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue</span></span>
<span data-ttu-id="ac760-194">[ RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx</span><span class="sxs-lookup"><span data-stu-id="ac760-194">[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx</span></span>
