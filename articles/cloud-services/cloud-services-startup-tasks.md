---
title: "aaaRun spuštění úlohy v Azure Cloud Services | Microsoft Docs"
description: "Spuštění úlohy pomůže připravit vaše prostředí cloudové služby pro vaši aplikaci. To se naučíte, jak spuštění úloh pracovní a toomake je"
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
ms.openlocfilehash: 3391a5d7434164f59972b8e497e5c34e33409543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-and-run-startup-tasks-for-a-cloud-service"></a><span data-ttu-id="3c837-104">Jak tooconfigure a spusťte spuštění úlohy pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="3c837-104">How tooconfigure and run startup tasks for a cloud service</span></span>
<span data-ttu-id="3c837-105">Spuštění úlohy tooperform operace můžete před zahájením roli.</span><span class="sxs-lookup"><span data-stu-id="3c837-105">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="3c837-106">Operace může být, že chcete tooperform zahrnovat instalaci komponenty, registraci komponenty modelu COM, nastavení klíče registru nebo dlouhotrvající proces.</span><span class="sxs-lookup"><span data-stu-id="3c837-106">Operations that you might want tooperform include installing a component, registering COM components, setting registry keys, or starting a long running process.</span></span>

> [!NOTE]
> <span data-ttu-id="3c837-107">Spuštění úlohy nejsou použitelné tooVirtual počítače, jenom tooCloud webové služby a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="3c837-107">Startup tasks are not applicable tooVirtual Machines, only tooCloud Service Web and Worker roles.</span></span>
> 
> 

## <a name="how-startup-tasks-work"></a><span data-ttu-id="3c837-108">Jak fungují spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="3c837-108">How startup tasks work</span></span>
<span data-ttu-id="3c837-109">Spuštění úlohy se akcí, které před zahájením role a jsou definovány v hello [ServiceDefinition.csdef] souboru pomocí hello [úloh] v rámci hello [spuštění]elementu.</span><span class="sxs-lookup"><span data-stu-id="3c837-109">Startup tasks are actions that are taken before your roles begin and are defined in hello [ServiceDefinition.csdef] file by using hello [Task] element within hello [Startup] element.</span></span> <span data-ttu-id="3c837-110">Často spuštění úlohy jsou dávkové soubory, ale lze je také konzolové aplikace nebo dávkové soubory, které se spouštějí skripty prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3c837-110">Frequently startup tasks are batch files, but they can also be console applications, or batch files that start PowerShell scripts.</span></span>

<span data-ttu-id="3c837-111">Proměnné prostředí předání informací do úloha spuštění a místní úložiště může být použité toopass informace o spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="3c837-111">Environment variables pass information into a startup task, and local storage can be used toopass information out of a startup task.</span></span> <span data-ttu-id="3c837-112">Například proměnná prostředí můžete určit program tooa cesta hello chcete tooinstall a toolocal úložiště, které lze poté číst později vaše role lze zapisovat soubory.</span><span class="sxs-lookup"><span data-stu-id="3c837-112">For example, an environment variable can specify hello path tooa program you want tooinstall, and files can be written toolocal storage that can then be read later by your roles.</span></span>

<span data-ttu-id="3c837-113">Spuštění úkolu můžete informace a chyby toohello adresář zadaný hello protokolu **TEMP** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c837-113">Your startup task can log information and errors toohello directory specified by hello **TEMP** environment variable.</span></span> <span data-ttu-id="3c837-114">Během spuštění úlohy hello hello **TEMP** proměnnou prostředí přeloží toohello *C:\\prostředky\\temp\\[identifikátor guid]. [[ Rolename]\\RoleTemp* directory při spuštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="3c837-114">During hello startup task, hello **TEMP** environment variable resolves toohello *C:\\Resources\\temp\\[guid].[rolename]\\RoleTemp* directory when running on hello cloud.</span></span>

<span data-ttu-id="3c837-115">Spuštění úlohy lze také spustit několikrát mezi jednotlivými restartováními.</span><span class="sxs-lookup"><span data-stu-id="3c837-115">Startup tasks can also be executed several times between reboots.</span></span> <span data-ttu-id="3c837-116">Například hello spuštění úlohy se spustí pokaždé, když recykluje hello role a role recykluje nemusí vždy zahrnovat restartování.</span><span class="sxs-lookup"><span data-stu-id="3c837-116">For example, hello startup task will be run each time hello role recycles, and role recycles may not always include a reboot.</span></span> <span data-ttu-id="3c837-117">Spuštění úlohy budou zasílány způsobem, který umožňuje jejich toorun bez problémů.</span><span class="sxs-lookup"><span data-stu-id="3c837-117">Startup tasks should be written in a way that allows them toorun several times without problems.</span></span>

<span data-ttu-id="3c837-118">Spuštění úlohy musí končit **errorlevel** (nebo ukončovací kód) nula pro toocomplete procesu spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="3c837-118">Startup tasks must end with an **errorlevel** (or exit code) of zero for hello startup process toocomplete.</span></span> <span data-ttu-id="3c837-119">Pokud úloha spuštění končí nenulovou **errorlevel**, hello role se nespustí.</span><span class="sxs-lookup"><span data-stu-id="3c837-119">If a startup task ends with a non-zero **errorlevel**, hello role will not start.</span></span>

## <a name="role-startup-order"></a><span data-ttu-id="3c837-120">Pořadí spouštění role</span><span class="sxs-lookup"><span data-stu-id="3c837-120">Role startup order</span></span>
<span data-ttu-id="3c837-121">Hello následující seznam obsahuje hello role spuštění procedury při spuštění v Azure:</span><span class="sxs-lookup"><span data-stu-id="3c837-121">hello following lists hello role startup procedure in Azure:</span></span>

1. <span data-ttu-id="3c837-122">Hello instance je označena jako **počáteční** a nepřijímá provoz.</span><span class="sxs-lookup"><span data-stu-id="3c837-122">hello instance is marked as **Starting** and does not receive traffic.</span></span>
2. <span data-ttu-id="3c837-123">Všechny úlohy spuštění jsou spouštěny podle tootheir **taskType** atribut.</span><span class="sxs-lookup"><span data-stu-id="3c837-123">All startup tasks are executed according tootheir **taskType** attribute.</span></span>
   
   * <span data-ttu-id="3c837-124">Hello **jednoduché** synchronně, provedení úloh po jednom.</span><span class="sxs-lookup"><span data-stu-id="3c837-124">hello **simple** tasks are executed synchronously, one at a time.</span></span>
   * <span data-ttu-id="3c837-125">Hello **pozadí** a **popředí** úloh se úloha spustila asynchronně, paralelní toohello spuštění.</span><span class="sxs-lookup"><span data-stu-id="3c837-125">hello **background** and **foreground** tasks are started asynchronously, parallel toohello startup task.</span></span>  
     
     > [!WARNING]
     > <span data-ttu-id="3c837-126">IIS nemusí být plně nakonfigurované během hello spuštění úloh fáze procesu spouštění hello, tak roli specifických dat nemusí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3c837-126">IIS may not be fully configured during hello startup task stage in hello startup process, so role-specific data may not be available.</span></span> <span data-ttu-id="3c837-127">Spuštění úlohy, které vyžadují specifickou rolí dat využít [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c837-127">Startup tasks that require role-specific data should use [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).</span></span>
     > 
     > 
3. <span data-ttu-id="3c837-128">spuštění Hello role Hostitelský proces a hello stránka je vytvořena ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="3c837-128">hello role host process is started and hello site is created in IIS.</span></span>
4. <span data-ttu-id="3c837-129">Hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="3c837-129">hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method is called.</span></span>
5. <span data-ttu-id="3c837-130">Hello instance je označena jako **připraven** a provoz se směruje toohello instance.</span><span class="sxs-lookup"><span data-stu-id="3c837-130">hello instance is marked as **Ready** and traffic is routed toohello instance.</span></span>
6. <span data-ttu-id="3c837-131">Hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="3c837-131">hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) method is called.</span></span>

## <a name="example-of-a-startup-task"></a><span data-ttu-id="3c837-132">Příklad spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="3c837-132">Example of a startup task</span></span>
<span data-ttu-id="3c837-133">Spuštění úlohy jsou definovány v hello [ServiceDefinition.csdef] souboru ve hello **úloh** elementu.</span><span class="sxs-lookup"><span data-stu-id="3c837-133">Startup tasks are defined in hello [ServiceDefinition.csdef] file, in hello **Task** element.</span></span> <span data-ttu-id="3c837-134">Hello **commandLine** atribut určuje název hello a parametry hello spuštění dávky souboru nebo konzole příkazu hello **executionContext** atribut určuje úroveň oprávnění hello hello spuštění Úloha a hello **taskType** atribut určuje, jak bude proveden hello úloh.</span><span class="sxs-lookup"><span data-stu-id="3c837-134">hello **commandLine** attribute specifies hello name and parameters of hello startup batch file or console command, hello **executionContext** attribute specifies hello privilege level of hello startup task, and hello **taskType** attribute specifies how hello task will be executed.</span></span>

<span data-ttu-id="3c837-135">V tomto příkladu se proměnná prostředí **MyVersionNumber**, se vytvoří pro hello spuštění úloh a nastavte hodnotu toohello "**1.0.0.0**".</span><span class="sxs-lookup"><span data-stu-id="3c837-135">In this example, an environment variable, **MyVersionNumber**, is created for hello startup task and set toohello value "**1.0.0.0**".</span></span>

<span data-ttu-id="3c837-136">**ServiceDefinition.csdef**:</span><span class="sxs-lookup"><span data-stu-id="3c837-136">**ServiceDefinition.csdef**:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

<span data-ttu-id="3c837-137">V následujícím příkladu hello, hello **Startup.cmd** dávkový soubor zapíše řádku hello "hello aktuální verze je 1.0.0.0" toohello StartupLog.txt soubor v adresáři hello určené proměnnou prostředí TEMP hello.</span><span class="sxs-lookup"><span data-stu-id="3c837-137">In hello following example, hello **Startup.cmd** batch file writes hello line "hello current version is 1.0.0.0" toohello StartupLog.txt file in hello directory specified by hello TEMP environment variable.</span></span> <span data-ttu-id="3c837-138">Hello `EXIT /B 0` řádku zajišťuje končí tuto úlohu spuštění hello **errorlevel** nula.</span><span class="sxs-lookup"><span data-stu-id="3c837-138">hello `EXIT /B 0` line ensures that hello startup task ends with an **errorlevel** of zero.</span></span>

```cmd
ECHO hello current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> <span data-ttu-id="3c837-139">V sadě Visual Studio, hello **zkopírujte tooOutput Directory** vlastnost pro váš dávkový soubor spuštění musí být nastavená příliš**kopie vždy** toobe se, že je váš dávkový soubor spuštění správně nasadili projekt tooyour v Azure (**approot\\bin** pro webové role a **approot** pro role pracovního procesu).</span><span class="sxs-lookup"><span data-stu-id="3c837-139">In Visual Studio, hello **Copy tooOutput Directory** property for your startup batch file should be set too**Copy Always** toobe sure that your startup batch file is properly deployed tooyour project on Azure (**approot\\bin** for Web roles, and **approot** for worker roles).</span></span>
> 
> 

## <a name="description-of-task-attributes"></a><span data-ttu-id="3c837-140">Popis úloh atributů</span><span class="sxs-lookup"><span data-stu-id="3c837-140">Description of task attributes</span></span>
<span data-ttu-id="3c837-141">Hello následující text popisuje atributy hello hello **úloh** element v hello [ServiceDefinition.csdef] souboru:</span><span class="sxs-lookup"><span data-stu-id="3c837-141">hello following describes hello attributes of hello **Task** element in hello [ServiceDefinition.csdef] file:</span></span>

<span data-ttu-id="3c837-142">**commandLine** -určuje hello příkazový řádek úkolu spuštění hello:</span><span class="sxs-lookup"><span data-stu-id="3c837-142">**commandLine** - Specifies hello command line for hello startup task:</span></span>

* <span data-ttu-id="3c837-143">Hello příkaz s parametry volitelné příkazového řádku, které začne úloha spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="3c837-143">hello command, with optional command line parameters, which begins hello startup task.</span></span>
* <span data-ttu-id="3c837-144">Často jde hello název dávkového souboru CMD nebo BAT.</span><span class="sxs-lookup"><span data-stu-id="3c837-144">Frequently this is hello filename of a .cmd or .bat batch file.</span></span>
* <span data-ttu-id="3c837-145">Úloha Hello je relativní toohello AppRoot\\složky Koš služby pro nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="3c837-145">hello task is relative toohello AppRoot\\Bin folder for hello deployment.</span></span> <span data-ttu-id="3c837-146">Při určování a hello cesta k souboru hello úlohy nejsou rozbalit proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c837-146">Environment variables are not expanded in determining hello path and file of hello task.</span></span> <span data-ttu-id="3c837-147">Pokud rozšíření prostředí je potřeba, můžete vytvořit malé .cmd skript, který volá spuštění úkolu.</span><span class="sxs-lookup"><span data-stu-id="3c837-147">If environment expansion is required, you can create a small .cmd script that calls your startup task.</span></span>
* <span data-ttu-id="3c837-148">Může být konzolovou aplikaci nebo dávkového souboru, který začíná [skript prostředí PowerShell](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span><span class="sxs-lookup"><span data-stu-id="3c837-148">Can be a console application or a batch file that starts a [PowerShell script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).</span></span>

<span data-ttu-id="3c837-149">**executionContext** -určuje úroveň oprávnění hello hello spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="3c837-149">**executionContext** - Specifies hello privilege level for hello startup task.</span></span> <span data-ttu-id="3c837-150">úroveň oprávnění Hello můžete omezené nebo se zvýšenými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="3c837-150">hello privilege level can be limited or elevated:</span></span>

* <span data-ttu-id="3c837-151">**omezená**</span><span class="sxs-lookup"><span data-stu-id="3c837-151">**limited**</span></span>  
  <span data-ttu-id="3c837-152">Úloha spuštění Hello spustí pomocí hello stejné oprávnění jako hello role.</span><span class="sxs-lookup"><span data-stu-id="3c837-152">hello startup task runs with hello same privileges as hello role.</span></span> <span data-ttu-id="3c837-153">Když hello **executionContext** atribut pro hello [Runtime] element je také **omezené**, jsou použita uživatelská oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3c837-153">When hello **executionContext** attribute for hello [Runtime] element is also **limited**, then user privileges are used.</span></span>
* <span data-ttu-id="3c837-154">**zvýšené**</span><span class="sxs-lookup"><span data-stu-id="3c837-154">**elevated**</span></span>  
  <span data-ttu-id="3c837-155">Úloha spuštění Hello spustí s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="3c837-155">hello startup task runs with administrator privileges.</span></span> <span data-ttu-id="3c837-156">To umožňuje spuštění úlohy tooinstall programy, udělat změny konfigurace služby IIS, provedení změn v registru a další úkoly úrovni správce bez zvýšení úrovně oprávnění hello hello role sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3c837-156">This allows startup tasks tooinstall programs, make IIS configuration changes, perform registry changes, and other administrator level tasks, without increasing hello privilege level of hello role itself.</span></span>  

> [!NOTE]
> <span data-ttu-id="3c837-157">Hello úroveň oprávnění při spuštění úlohy nemusí toobe hello stejné jako hello role sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3c837-157">hello privilege level of a startup task does not need toobe hello same as hello role itself.</span></span>
> 
> 

<span data-ttu-id="3c837-158">**taskType** -určuje hello způsob spuštění úlohy se spustí.</span><span class="sxs-lookup"><span data-stu-id="3c837-158">**taskType** - Specifies hello way a startup task is executed.</span></span>

* <span data-ttu-id="3c837-159">**jednoduché**</span><span class="sxs-lookup"><span data-stu-id="3c837-159">**simple**</span></span>  
  <span data-ttu-id="3c837-160">Úlohy jsou spouštěny synchronně, jeden po druhém, v pořadí hello zadaný v hello [ServiceDefinition.csdef] souboru.</span><span class="sxs-lookup"><span data-stu-id="3c837-160">Tasks are executed synchronously, one at a time, in hello order specified in hello [ServiceDefinition.csdef] file.</span></span> <span data-ttu-id="3c837-161">Pokud jeden **jednoduché** končí úloha spuštění **errorlevel** nulové hodnoty, hello vedle **jednoduché** spuštění úlohy je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="3c837-161">When one **simple** startup task ends with an **errorlevel** of zero, hello next **simple** startup task is executed.</span></span> <span data-ttu-id="3c837-162">Pokud neexistují žádné další **jednoduché** spuštění úloh tooexecute, pak bude spuštěn hello role sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3c837-162">If there are no more **simple** startup tasks tooexecute, then hello role itself will be started.</span></span>   
  
  > [!NOTE]
  > <span data-ttu-id="3c837-163">Pokud hello **jednoduché** úloh končí nenulovou **errorlevel**, hello instance se zablokuje.</span><span class="sxs-lookup"><span data-stu-id="3c837-163">If hello **simple** task ends with a non-zero **errorlevel**, hello instance will be blocked.</span></span> <span data-ttu-id="3c837-164">Následné **jednoduché** spuštění úlohy a role hello, samostatně, se nespustí.</span><span class="sxs-lookup"><span data-stu-id="3c837-164">Subsequent **simple** startup tasks, and hello role itself, will not start.</span></span>
  > 
  > 
  
    <span data-ttu-id="3c837-165">tooensure, který končí váš dávkový soubor **errorlevel** nulové hodnoty, spusťte příkaz hello `EXIT /B 0` na konci hello dávkové zpracování souboru.</span><span class="sxs-lookup"><span data-stu-id="3c837-165">tooensure that your batch file ends with an **errorlevel** of zero, execute hello command `EXIT /B 0` at hello end of your batch file process.</span></span>
* <span data-ttu-id="3c837-166">**pozadí**</span><span class="sxs-lookup"><span data-stu-id="3c837-166">**background**</span></span>  
  <span data-ttu-id="3c837-167">Úlohy se spustí asynchronně, paralelně s hello spuštění role hello.</span><span class="sxs-lookup"><span data-stu-id="3c837-167">Tasks are executed asynchronously, in parallel with hello startup of hello role.</span></span>
* <span data-ttu-id="3c837-168">**popředí**</span><span class="sxs-lookup"><span data-stu-id="3c837-168">**foreground**</span></span>  
  <span data-ttu-id="3c837-169">Úlohy se spustí asynchronně, paralelně s hello spuštění role hello.</span><span class="sxs-lookup"><span data-stu-id="3c837-169">Tasks are executed asynchronously, in parallel with hello startup of hello role.</span></span> <span data-ttu-id="3c837-170">Hello spočívá hlavní rozdíl mezi **popředí** a **pozadí** úloh je, že **popředí** úloh brání hello role z recyklace nebo vypnutí, dokud úloha hello byl ukončen.</span><span class="sxs-lookup"><span data-stu-id="3c837-170">hello key difference between a **foreground** and a **background** task is that a **foreground** task prevents hello role from recycling or shutting down until hello task has ended.</span></span> <span data-ttu-id="3c837-171">Hello **pozadí** úlohy nemají toto omezení.</span><span class="sxs-lookup"><span data-stu-id="3c837-171">hello **background** tasks do not have this restriction.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="3c837-172">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="3c837-172">Environment variables</span></span>
<span data-ttu-id="3c837-173">Proměnné prostředí jsou způsob toopass informace tooa spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="3c837-173">Environment variables are a way toopass information tooa startup task.</span></span> <span data-ttu-id="3c837-174">Například můžete umístit objekt hello cesta tooa blob, který obsahuje tooinstall program nebo čísla portů, který bude používat vaše role nebo funkce toocontrol nastavení spuštění úkolu.</span><span class="sxs-lookup"><span data-stu-id="3c837-174">For example, you can put hello path tooa blob that contains a program tooinstall, or port numbers that your role will use, or settings toocontrol features of your startup task.</span></span>

<span data-ttu-id="3c837-175">Existují dva typy proměnných prostředí pro spuštění úlohy; statické proměnné prostředí, proměnné prostředí založené na členy hello [ RoleEnvironment] třídy.</span><span class="sxs-lookup"><span data-stu-id="3c837-175">There are two kinds of environment variables for startup tasks; static environment variables and environment variables based on members of hello [RoleEnvironment] class.</span></span> <span data-ttu-id="3c837-176">Obě jsou v hello [prostředí] části hello [ServiceDefinition.csdef] soubor a obě použití hello [proměnné] elementu a **název** atribut.</span><span class="sxs-lookup"><span data-stu-id="3c837-176">Both are in hello [Environment] section of hello [ServiceDefinition.csdef] file, and both use hello [Variable] element and **name** attribute.</span></span>

<span data-ttu-id="3c837-177">Hello používá proměnné prostředí statickým **hodnotu** atribut hello [proměnné] elementu.</span><span class="sxs-lookup"><span data-stu-id="3c837-177">Static environment variables uses hello **value** attribute of hello [Variable] element.</span></span> <span data-ttu-id="3c837-178">výše uvedený příklad Hello vytvoří proměnnou prostředí hello **MyVersionNumber** jehož statickou hodnotu "**1.0.0.0**".</span><span class="sxs-lookup"><span data-stu-id="3c837-178">hello example above creates hello environment variable **MyVersionNumber** which has a static value of "**1.0.0.0**".</span></span> <span data-ttu-id="3c837-179">Dalším příkladem může být toocreate **StagingOrProduction** proměnné prostředí, kterou můžete nastavit ručně toovalues z "**pracovní**"nebo"**produkční**" tooperform různé spuštění akce na základě hello hodnoty hello **StagingOrProduction** proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c837-179">Another example would be toocreate a **StagingOrProduction** environment variable which you can manually set toovalues of "**staging**" or "**production**" tooperform different startup actions based on hello value of hello **StagingOrProduction** environment variable.</span></span>

<span data-ttu-id="3c837-180">Proměnné prostředí založené na členy hello RoleEnvironment třída nepoužívejte hello **hodnotu** atribut hello [proměnné] elementu.</span><span class="sxs-lookup"><span data-stu-id="3c837-180">Environment variables based on members of hello RoleEnvironment class do not use hello **value** attribute of hello [Variable] element.</span></span> <span data-ttu-id="3c837-181">Místo toho hello [RoleInstanceValue] podřízený element s hello odpovídající **XPath** hodnota atributu, jsou použité toocreate proměnné prostředí založené na konkrétní členem hello [ RoleEnvironment] třídy.</span><span class="sxs-lookup"><span data-stu-id="3c837-181">Instead, hello [RoleInstanceValue] child element, with hello appropriate **XPath** attribute value, are used toocreate an environment variable based on a specific member of hello [RoleEnvironment] class.</span></span> <span data-ttu-id="3c837-182">Hodnoty pro hello **XPath** atribut tooaccess různé [ RoleEnvironment] hodnoty jsou uvedeny [zde](cloud-services-role-config-xpath.md).</span><span class="sxs-lookup"><span data-stu-id="3c837-182">Values for hello **XPath** attribute tooaccess various [RoleEnvironment] values can be found [here](cloud-services-role-config-xpath.md).</span></span>

<span data-ttu-id="3c837-183">Například toocreate proměnná prostředí, která je "**true**" při hello je spuštěn v emulátoru služby výpočty hello, a "**false**" při spuštění v cloudu hello, použijte následující hello [proměnné] a [RoleInstanceValue] prvky:</span><span class="sxs-lookup"><span data-stu-id="3c837-183">For example, toocreate an environment variable that is "**true**" when hello instance is running in hello compute emulator, and "**false**" when running in hello cloud, use hello following [Variable] and [RoleInstanceValue] elements:</span></span>

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create hello environment variable that informs hello startup task whether it is running
                in hello Compute Emulator or in hello cloud. "%ComputeEmulatorRunning%"=="true" when
                running in hello Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in hello cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a><span data-ttu-id="3c837-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c837-184">Next steps</span></span>
<span data-ttu-id="3c837-185">Zjistěte, jak tooperform některé [běžné úlohy spuštění](cloud-services-startup-tasks-common.md) s Cloudovou službou.</span><span class="sxs-lookup"><span data-stu-id="3c837-185">Learn how tooperform some [common startup tasks](cloud-services-startup-tasks-common.md) with your Cloud Service.</span></span>

<span data-ttu-id="3c837-186">[Balíček](cloud-services-model-and-package.md) cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="3c837-186">[Package](cloud-services-model-and-package.md) your Cloud Service.</span></span>  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[úloh]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[spuštění]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[prostředí]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[proměnné]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[ RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
