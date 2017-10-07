---
title: "aaaLearning pracovní postup prostředí PowerShell pro Azure Automation | Microsoft Docs"
description: "Tento článek je určený jako rychlé lekce pro autory obeznámeni s prostředí PowerShell toounderstand hello určité rozdíly mezi prostředí PowerShell a pracovní postup prostředí PowerShell a použít tooAutomation koncepty sady runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 84bf133e-5343-4e0e-8d6c-bb14304a70db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 362c504eb96d31b99a826b128e6a591beecaa084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a><span data-ttu-id="5b21f-103">Učení klíčové koncepty pracovního postupu prostředí Windows PowerShell pro automatizaci sady runbook</span><span class="sxs-lookup"><span data-stu-id="5b21f-103">Learning key Windows PowerShell Workflow concepts for Automation runbooks</span></span> 
<span data-ttu-id="5b21f-104">Runbooky ve službě Azure Automation se implementují jako pracovní postupy prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5b21f-104">Runbooks in Azure Automation are implemented as Windows PowerShell Workflows.</span></span>  <span data-ttu-id="5b21f-105">Pracovní postup prostředí Windows PowerShell je skript prostředí Windows PowerShell podobně jako tooa, ale existují některé významné rozdíly, které může být matoucí tooa nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="5b21f-105">A Windows PowerShell Workflow is similar tooa Windows PowerShell script but has some significant differences that can be confusing tooa new user.</span></span>  <span data-ttu-id="5b21f-106">Tento článek je určený toohelp zápisu sady runbook pomocí prostředí PowerShell. pracovní postup, doporučujeme, abyste že zápisu sady runbook pomocí prostředí PowerShell, pokud potřebujete kontrolní body.</span><span class="sxs-lookup"><span data-stu-id="5b21f-106">While this article is intended toohelp you write runbooks using PowerShell workflow, we recommend you write runbooks using PowerShell unless you need checkpoints.</span></span>  <span data-ttu-id="5b21f-107">Existuje několik rozdílů v syntaxi při autorizaci sad runbook PowerShell Workflow a tyto rozdíly vyžaduje trochu další pracovní toowrite efektivní pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="5b21f-107">There are several syntax differences when authoring PowerShell Workflow runbooks and these differences require a bit more work toowrite effective workflows.</span></span>  

<span data-ttu-id="5b21f-108">Pracovní postup je pořadí naprogramovaných, propojených kroků, které provádějí dlouhodobé úlohy nebo vyžadují koordinaci více kroků hello v rámci více zařízení nebo spravovaných uzlů.</span><span class="sxs-lookup"><span data-stu-id="5b21f-108">A workflow is a sequence of programmed, connected steps that perform long-running tasks or require hello coordination of multiple steps across multiple devices or managed nodes.</span></span> <span data-ttu-id="5b21f-109">Hello výhody pracovního postupu oproti běžnému skriptu patří schopnost hello toosimultaneously provádět akci vůči několika zařízením a tooautomatically hello možnost obnovení v případě selhání.</span><span class="sxs-lookup"><span data-stu-id="5b21f-109">hello benefits of a workflow over a normal script include hello ability toosimultaneously perform an action against multiple devices and hello ability tooautomatically recover from failures.</span></span> <span data-ttu-id="5b21f-110">Pracovní postup prostředí Windows PowerShell je skript prostředí Windows PowerShell, který používá Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="5b21f-110">A Windows PowerShell Workflow is a Windows PowerShell script that uses Windows Workflow Foundation.</span></span> <span data-ttu-id="5b21f-111">Když pracovní postup hello napsané pomocí syntaxe prostředí Windows PowerShell a spuštění pomocí prostředí Windows PowerShell, je zpracován programovacím modelem Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="5b21f-111">While hello workflow is written with Windows PowerShell syntax and launched by Windows PowerShell, it is processed by Windows Workflow Foundation.</span></span>

<span data-ttu-id="5b21f-112">Kompletní informace o hello témata v tomto článku najdete v části [Začínáme s pracovním postupem prostředí Windows PowerShell](http://technet.microsoft.com/library/jj134242.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b21f-112">For complete details on hello topics in this article, see [Getting Started with Windows PowerShell Workflow](http://technet.microsoft.com/library/jj134242.aspx).</span></span>

## <a name="basic-structure-of-a-workflow"></a><span data-ttu-id="5b21f-113">Základní struktura pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="5b21f-113">Basic structure of a workflow</span></span>
<span data-ttu-id="5b21f-114">Hello první krok tooconverting pracovního postupu Powershellu tooa skriptu prostředí PowerShell je uveden s hello **pracovního postupu** – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="5b21f-114">hello first step tooconverting a PowerShell script tooa PowerShell workflow is enclosing it with hello **Workflow** keyword.</span></span>  <span data-ttu-id="5b21f-115">Pracovní postup začíná hello **pracovního postupu** – klíčové slovo, za nímž následuje hello textu hello skriptu uzavřené do složených závorek.</span><span class="sxs-lookup"><span data-stu-id="5b21f-115">A workflow starts with hello **Workflow** keyword followed by hello body of hello script enclosed in braces.</span></span> <span data-ttu-id="5b21f-116">Následuje Hello název pracovního postupu hello hello **pracovního postupu** – klíčové slovo, jak je znázorněno v hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="5b21f-116">hello name of hello workflow follows hello **Workflow** keyword as shown in hello following syntax:</span></span>

    Workflow Test-Workflow
    {
       <Commands>
    }

<span data-ttu-id="5b21f-117">Název Hello hello pracovního postupu musí odpovídat názvu hello hello automatizace sady runbook.</span><span class="sxs-lookup"><span data-stu-id="5b21f-117">hello name of hello workflow must match hello name of hello Automation runbook.</span></span> <span data-ttu-id="5b21f-118">Pokud hello runbook importována, pak hello filename musí odpovídat názvu pracovního postupu hello a musí končit *.ps1*.</span><span class="sxs-lookup"><span data-stu-id="5b21f-118">If hello runbook is being imported, then hello filename must match hello workflow name and must end in *.ps1*.</span></span>

<span data-ttu-id="5b21f-119">pracovní postup toohello parametry tooadd, použijte hello **Param** – klíčové slovo stejně jako tooa skriptu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-119">tooadd parameters toohello workflow, use hello **Param** keyword just as you would tooa script.</span></span>

## <a name="code-changes"></a><span data-ttu-id="5b21f-120">Změny kódu</span><span class="sxs-lookup"><span data-stu-id="5b21f-120">Code changes</span></span>
<span data-ttu-id="5b21f-121">Kód pracovního postupu Powershellu vypadá kód skriptu tooPowerShell téměř shodné s výjimkou několik významných změn.</span><span class="sxs-lookup"><span data-stu-id="5b21f-121">PowerShell workflow code looks almost identical tooPowerShell script code except for a few significant changes.</span></span>  <span data-ttu-id="5b21f-122">Hello následující oddíly popisují změny, je nutné, skript prostředí PowerShell tooa toomake pro něj toorun v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-122">hello following sections describe changes that you need toomake tooa PowerShell script for it toorun in a workflow.</span></span>

### <a name="activities"></a><span data-ttu-id="5b21f-123">Aktivity</span><span class="sxs-lookup"><span data-stu-id="5b21f-123">Activities</span></span>
<span data-ttu-id="5b21f-124">Aktivita je specifická úloha v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-124">An activity is a specific task in a workflow.</span></span> <span data-ttu-id="5b21f-125">Stejně jako se skript skládá z jednoho nebo více příkazů, pracovní postup se skládá z jedné nebo více aktivit, které se provádějí v pořadí.</span><span class="sxs-lookup"><span data-stu-id="5b21f-125">Just as a script is composed of one or more commands, a workflow is composed of one or more activities that are carried out in a sequence.</span></span> <span data-ttu-id="5b21f-126">Pracovní postup prostředí Windows PowerShell automaticky převádí, mnoho hello tooactivities rutiny prostředí Windows PowerShell při spuštění pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-126">Windows PowerShell Workflow automatically converts many of hello Windows PowerShell cmdlets tooactivities when it runs a workflow.</span></span> <span data-ttu-id="5b21f-127">Pokud určíte jednu z těchto rutin ve vašem runbooku, hello odpovídající aktivita spuštěna programovacím modelem Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="5b21f-127">When you specify one of these cmdlets in your runbook, hello corresponding activity is run by Windows Workflow Foundation.</span></span> <span data-ttu-id="5b21f-128">Těchto rutin bez odpovídající aktivity pracovního postupu prostředí Windows PowerShell automaticky spustí rutinu hello v rámci [InlineScript](#inlinescript) aktivity.</span><span class="sxs-lookup"><span data-stu-id="5b21f-128">For those cmdlets without a corresponding activity, Windows PowerShell Workflow automatically runs hello cmdlet within an [InlineScript](#inlinescript) activity.</span></span> <span data-ttu-id="5b21f-129">Existuje sada rutin, které jsou vyloučeny a nejde ho použít v pracovním postupu, pokud je výslovně nezahrnete v bloku InlineScript.</span><span class="sxs-lookup"><span data-stu-id="5b21f-129">There is a set of cmdlets that are excluded and cannot be used in a workflow unless you explicitly include them in an InlineScript block.</span></span> <span data-ttu-id="5b21f-130">Další informace o těchto pojmech najdete v části [pomocí aktivity ve skriptových pracovních postupech](http://technet.microsoft.com/library/jj574194.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b21f-130">For further details on these concepts, see [Using Activities in Script Workflows](http://technet.microsoft.com/library/jj574194.aspx).</span></span>

<span data-ttu-id="5b21f-131">Aktivity pracovních postupů sdílejí sadu společné parametry tooconfigure jejich provoz.</span><span class="sxs-lookup"><span data-stu-id="5b21f-131">Workflow activities share a set of common parameters tooconfigure their operation.</span></span> <span data-ttu-id="5b21f-132">Podrobnosti o společných parametrech pracovního postupu hello najdete v tématu [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b21f-132">For details about hello workflow common parameters, see [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="positional-parameters"></a><span data-ttu-id="5b21f-133">Poziční parametry</span><span class="sxs-lookup"><span data-stu-id="5b21f-133">Positional parameters</span></span>
<span data-ttu-id="5b21f-134">Poziční parametry nelze použít s rutinami v pracovním postupu a aktivity.</span><span class="sxs-lookup"><span data-stu-id="5b21f-134">You can't use positional parameters with activities and cmdlets in a workflow.</span></span>  <span data-ttu-id="5b21f-135">To znamená je, že je nutné použít názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="5b21f-135">All this means is that you must use parameter names.</span></span>

<span data-ttu-id="5b21f-136">Zvažte například následující kód, který získá všechny spuštěné služby hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-136">For example, consider hello following code that gets all running services.</span></span>

     Get-Service | Where-Object {$_.Status -eq "Running"}

<span data-ttu-id="5b21f-137">Pokud se pokusíte toorun Tento stejný kód v pracovním postupu, obdržíte zprávu jako "Sadu nelze vyřešit pomocí hello zadaný parametr pojmenované parametry."</span><span class="sxs-lookup"><span data-stu-id="5b21f-137">If you try toorun this same code in a workflow, you receive a message like "Parameter set cannot be resolved using hello specified named parameters."</span></span>  <span data-ttu-id="5b21f-138">toocorrect, zadejte název parametru hello jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-138">toocorrect this, provide hello parameter name as in hello following.</span></span>

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a><span data-ttu-id="5b21f-139">Deserializovaná objekty</span><span class="sxs-lookup"><span data-stu-id="5b21f-139">Deserialized objects</span></span>
<span data-ttu-id="5b21f-140">Objekty v pracovních postupech jsou deserializovat.</span><span class="sxs-lookup"><span data-stu-id="5b21f-140">Objects in workflows are deserialized.</span></span>  <span data-ttu-id="5b21f-141">To znamená, že jejich vlastnosti jsou stále k dispozici, ale není své metody.</span><span class="sxs-lookup"><span data-stu-id="5b21f-141">This means that their properties are still available, but not their methods.</span></span>  <span data-ttu-id="5b21f-142">Zvažte například následující kód prostředí PowerShell, který zastaví službu pomocí metody Stop hello objektu služby hello hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-142">For example, consider hello following PowerShell code that stops a service using hello Stop method of hello Service object.</span></span>

    $Service = Get-Service -Name MyService
    $Service.Stop()

<span data-ttu-id="5b21f-143">Pokud se pokusíte toorun to v pracovním postupu, se zobrazí chyba s oznámením "volání metody není podporováno v pracovním postupu Windows PowerShell".</span><span class="sxs-lookup"><span data-stu-id="5b21f-143">If you try toorun this in a workflow, you receive an error saying "Method invocation is not supported in a Windows PowerShell Workflow."</span></span>  

<span data-ttu-id="5b21f-144">Jednou z možností je toowrap tyto dva řádky kódu [InlineScript](#inlinescript) blokovat v takovém případě $Service bude objekt služby v rámci bloku hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-144">One option is toowrap these two lines of code in an [InlineScript](#inlinescript) block in which case $Service would be a service object within hello block.</span></span>

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

<span data-ttu-id="5b21f-145">Další možností je toouse jiná rutina, která provádí hello stejné funkce jako metoda hello, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5b21f-145">Another option is toouse another cmdlet that performs hello same functionality as hello method, if one is available.</span></span>  <span data-ttu-id="5b21f-146">V našem příkladu rutiny hello Stop-Service poskytuje hello stejné funkce jako metoda Stop hello a můžete použít následující hello pro pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="5b21f-146">In our sample, hello Stop-Service cmdlet provides hello same functionality as hello Stop method, and you could use hello following for a workflow.</span></span>

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a><span data-ttu-id="5b21f-147">InlineScript</span><span class="sxs-lookup"><span data-stu-id="5b21f-147">InlineScript</span></span>
<span data-ttu-id="5b21f-148">Hello **InlineScript** aktivity je užitečné, když potřebujete jeden nebo více příkazů pro toorun jako tradiční skript prostředí PowerShell namísto pracovního postupu Powershellu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-148">hello **InlineScript** activity is useful when you need toorun one or more commands as traditional PowerShell script instead of PowerShell workflow.</span></span>  <span data-ttu-id="5b21f-149">Zatímco příkazy v pracovním postupu jsou odesílány pro zpracování tooWindows Workflow Foundation, příkazy v bloku InlineScript jsou zpracovány prostředím Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5b21f-149">While commands in a workflow are sent tooWindows Workflow Foundation for processing, commands in an InlineScript block are processed by Windows PowerShell.</span></span>

<span data-ttu-id="5b21f-150">Aktivita InlineScript používá hello následující syntaxe uvedená níže.</span><span class="sxs-lookup"><span data-stu-id="5b21f-150">InlineScript uses hello following syntax shown below.</span></span>

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

<span data-ttu-id="5b21f-151">Výstup můžete vrátit InlineScript přiřazením proměnné tooa výstup hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-151">You can return output from an InlineScript by assigning hello output tooa variable.</span></span> <span data-ttu-id="5b21f-152">Hello následující příklad zastaví službu a poté uloží název služby hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-152">hello following example stops a service and then outputs hello service name.</span></span>

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


<span data-ttu-id="5b21f-153">Můžete předat hodnoty do bloku InlineScript, ale musíte použít **$Using** modifikátor oboru.</span><span class="sxs-lookup"><span data-stu-id="5b21f-153">You can pass values into an InlineScript block, but you must use **$Using** scope modifier.</span></span>  <span data-ttu-id="5b21f-154">Hello následující příklad je identické toohello předchozí příklad s tím rozdílem, že hello služby poskytl název proměnné.</span><span class="sxs-lookup"><span data-stu-id="5b21f-154">hello following example is identical toohello previous example except that hello service name is provided by a variable.</span></span>

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"

        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


<span data-ttu-id="5b21f-155">Aktivity InlineScript může být důležité v určitých pracovních postupech, nepodporují konstrukce pracovního postupu a musí být použit pouze v případě potřeby pro hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="5b21f-155">While InlineScript activities may be critical in certain workflows, they do not support workflow constructs and should only be used when necessary for hello following reasons:</span></span>

* <span data-ttu-id="5b21f-156">Nemůžete použít [kontrolní body](#checkpoints) uvnitř bloku InlineScript.</span><span class="sxs-lookup"><span data-stu-id="5b21f-156">You cannot use [checkpoints](#checkpoints) inside an InlineScript block.</span></span> <span data-ttu-id="5b21f-157">V případě selhání v rámci bloku hello musí pokračovat znovu od začátku hello hello bloku.</span><span class="sxs-lookup"><span data-stu-id="5b21f-157">If a failure occurs within hello block, it must be resumed from hello beginning of hello block.</span></span>
* <span data-ttu-id="5b21f-158">Nemůžete použít [paralelní provádění](#parallel-processing) uvnitř InlineScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="5b21f-158">You cannot use [parallel execution](#parallel-processing) inside an InlineScriptBlock.</span></span>
* <span data-ttu-id="5b21f-159">InlineScript ovlivňuje škálovatelnost hello pracovního postupu, protože drží relaci prostředí Windows PowerShell hello pro celou délku bloku InlineScript hello hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-159">InlineScript affects scalability of hello workflow since it holds hello Windows PowerShell session for hello entire length of hello InlineScript block.</span></span>

<span data-ttu-id="5b21f-160">Další informace o používání InlineScript najdete v tématu [spouštění příkazů prostředí Windows PowerShell v pracovním postupu](http://technet.microsoft.com/library/jj574197.aspx) a [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b21f-160">For more information on using InlineScript, see [Running Windows PowerShell Commands in a Workflow](http://technet.microsoft.com/library/jj574197.aspx) and [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span></span>

## <a name="parallel-processing"></a><span data-ttu-id="5b21f-161">Paralelní zpracování</span><span class="sxs-lookup"><span data-stu-id="5b21f-161">Parallel processing</span></span>
<span data-ttu-id="5b21f-162">Jednou z výhod pracovních postupů prostředí Windows PowerShell je hello možnost tooperform sadu příkazů paralelně namísto postupně jako v případě typického skriptu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-162">One advantage of Windows PowerShell Workflows is hello ability tooperform a set of commands in parallel instead of sequentially as with a typical script.</span></span>

<span data-ttu-id="5b21f-163">Můžete použít hello **paralelní** – klíčové slovo toocreate blok skriptu s více příkazy, které běží souběžně.</span><span class="sxs-lookup"><span data-stu-id="5b21f-163">You can use hello **Parallel** keyword toocreate a script block with multiple commands that run concurrently.</span></span> <span data-ttu-id="5b21f-164">Tato služba využívá hello následující syntaxe uvedená níže.</span><span class="sxs-lookup"><span data-stu-id="5b21f-164">This uses hello following syntax shown below.</span></span> <span data-ttu-id="5b21f-165">V takovém případě se aktivity "activity1" a "activity2" spustí na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-165">In this case, Activity1 and Activity2 starts at hello same time.</span></span> <span data-ttu-id="5b21f-166">Aktivita "activity3" spustí až po dokončení aktivity "activity1" a "activity2".</span><span class="sxs-lookup"><span data-stu-id="5b21f-166">Activity3 starts only after both Activity1 and Activity2 have completed.</span></span>

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


<span data-ttu-id="5b21f-167">Zvažte například následující příkazy prostředí PowerShell, které kopírování více cílové sítě tooa soubory hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-167">For example, consider hello following PowerShell commands that copy multiple files tooa network destination.</span></span>  <span data-ttu-id="5b21f-168">Tyto příkazy se spouštějí postupně, takže než jednoho souboru se musí ukončit kopírování před příštím spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-168">These commands are run sequentially so that one file must finish copying before hello next is started.</span></span>     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

<span data-ttu-id="5b21f-169">Hello následující pracovní postup spouští tyto stejné příkazy paralelně, aby všechny spuštění kopírování v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="5b21f-169">hello following workflow runs these same commands in parallel so that they all start copying at hello same time.</span></span>  <span data-ttu-id="5b21f-170">Až poté, co jsou všechny zkopírovat je hello dokončení zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="5b21f-170">Only after they are all copied is hello completion message displayed.</span></span>

    Workflow Copy-Files
    {
        Parallel
        {
            Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


<span data-ttu-id="5b21f-171">Můžete použít hello **ForEach-Parallel** současně vytvořit tooprocess příkazy pro každou položku v kolekci.</span><span class="sxs-lookup"><span data-stu-id="5b21f-171">You can use hello **ForEach -Parallel** construct tooprocess commands for each item in a collection concurrently.</span></span> <span data-ttu-id="5b21f-172">Hello položky v kolekci hello se zpracovávají paralelně, zatímco hello příkazy v bloku skriptu hello spouští sekvenčně.</span><span class="sxs-lookup"><span data-stu-id="5b21f-172">hello items in hello collection are processed in parallel while hello commands in hello script block run sequentially.</span></span> <span data-ttu-id="5b21f-173">Tato služba využívá hello následující syntaxe uvedená níže.</span><span class="sxs-lookup"><span data-stu-id="5b21f-173">This uses hello following syntax shown below.</span></span> <span data-ttu-id="5b21f-174">V takovém případě aktivity "activity1" spustí hello stejný čas pro všechny položky v kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-174">In this case, Activity1 starts at hello same time for all items in hello collection.</span></span> <span data-ttu-id="5b21f-175">Pro každou položku "activity2" spustí po dokončení aktivity "activity1".</span><span class="sxs-lookup"><span data-stu-id="5b21f-175">For each item, Activity2 starts after Activity1 is complete.</span></span> <span data-ttu-id="5b21f-176">Aktivita "activity3" spustí až po dokončení aktivity "activity1" a "activity2" pro všechny položky.</span><span class="sxs-lookup"><span data-stu-id="5b21f-176">Activity3 starts only after both Activity1 and Activity2 have completed for all items.</span></span>

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

<span data-ttu-id="5b21f-177">Následující ukázka Hello je podobné předchozí příklad toohello kopírování souborů paralelně.</span><span class="sxs-lookup"><span data-stu-id="5b21f-177">hello following example is similar toohello previous example copying files in parallel.</span></span>  <span data-ttu-id="5b21f-178">V takovém případě se zobrazí zpráva pro každý soubor, poté, co se zkopíruje.</span><span class="sxs-lookup"><span data-stu-id="5b21f-178">In this case, a message is displayed for each file after it copies.</span></span>  <span data-ttu-id="5b21f-179">Až poté, co jsou všechny úplně zkopírovat je hello konečné dokončení zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="5b21f-179">Only after they are all completely copied is hello final completion message displayed.</span></span>

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }

        Write-Output "All files copied."
    }

> [!NOTE]
> <span data-ttu-id="5b21f-180">Nedoporučujeme systémem podřízené runbooky paralelně vzhledem k tomu, že to se ukázalo toogive nespolehlivé výsledky.</span><span class="sxs-lookup"><span data-stu-id="5b21f-180">We do not recommend running child runbooks in parallel since this has been shown toogive unreliable results.</span></span>  <span data-ttu-id="5b21f-181">Hello výstup podřízeného runbooku hello někdy nezobrazuje a nastavení v jedné podřízené sady runbook může ovlivnit hello jiné paralelní podřízené runbooky</span><span class="sxs-lookup"><span data-stu-id="5b21f-181">hello output from hello child runbook sometimes does not show up, and settings in one child runbook can affect hello other parallel child runbooks</span></span>
>

## <a name="checkpoints"></a><span data-ttu-id="5b21f-182">Kontrolní body</span><span class="sxs-lookup"><span data-stu-id="5b21f-182">Checkpoints</span></span>
<span data-ttu-id="5b21f-183">A *kontrolního bodu* je snímek aktuálního stavu hello hello pracovního postupu, který zahrnuje hello aktuální hodnotu proměnných a výstup generovaný toothat bodu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-183">A *checkpoint* is a snapshot of hello current state of hello workflow that includes hello current value for variables and any output generated toothat point.</span></span> <span data-ttu-id="5b21f-184">Pokud pracovní postup končí chybu nebo je pozastaveno, pak hello při příštím spuštění se začne od svého posledního kontrolního bodu místo hello začátek hello worfklow.</span><span class="sxs-lookup"><span data-stu-id="5b21f-184">If a workflow ends in error or is suspended, then hello next time it is run it will start from its last checkpoint instead of hello start of hello worfklow.</span></span>  <span data-ttu-id="5b21f-185">Můžete nastavit kontrolní bod v pracovním postupu s hello **Checkpoint-Workflow** aktivity.</span><span class="sxs-lookup"><span data-stu-id="5b21f-185">You can set a checkpoint in a workflow with hello **Checkpoint-Workflow** activity.</span></span>

<span data-ttu-id="5b21f-186">V následující ukázkový kód hello dojde k výjimce po způsobuje "activity2" hello tooend pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-186">In hello following sample code, an exception occurs after Activity2 causing hello workflow tooend.</span></span> <span data-ttu-id="5b21f-187">Při dalším spuštění pracovního postupu hello, spustí se spuštěním aktivity Activity2, protože byla poslední po nastavení posledního kontrolního bodu hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-187">When hello workflow is run again, it starts by running Activity2 since this was just after hello last checkpoint set.</span></span>

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

<span data-ttu-id="5b21f-188">Po opakovaných aktivit, které mohou být náchylné k chybám tooexception a neměla by být pokud obnovení hello pracovního postupu byste měli nastavit kontrolní body v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-188">You should set checkpoints in a workflow after activities that may be prone tooexception and should not be repeated if hello workflow is resumed.</span></span> <span data-ttu-id="5b21f-189">Pracovní postup může například vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5b21f-189">For example, your workflow may create a virtual machine.</span></span> <span data-ttu-id="5b21f-190">Můžete nastavit kontrolní bod před i po hello příkazy toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b21f-190">You could set a checkpoint both before and after hello commands toocreate hello virtual machine.</span></span> <span data-ttu-id="5b21f-191">Pokud hello vytváření nezdaří, by hello příkazy opakovat, pokud je hello pracovní postup spustit znovu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-191">If hello creation fails, then hello commands would be repeated if hello workflow is started again.</span></span> <span data-ttu-id="5b21f-192">Pokud hello worfklow selže po vytvoření hello úspěšné, pak hello virtuální počítač nebude znovu vytvořen při obnovení pracovního postupu hello.</span><span class="sxs-lookup"><span data-stu-id="5b21f-192">If hello worfklow fails after hello creation succeeds, then hello virtual machine will not be created again when hello workflow is resumed.</span></span>

<span data-ttu-id="5b21f-193">Následující ukázka Hello zkopíruje více souborů tooa síťové umístění a nastaví kontrolní bod po každý soubor.</span><span class="sxs-lookup"><span data-stu-id="5b21f-193">hello following example copies multiple files tooa network location and sets a checkpoint after each file.</span></span>  <span data-ttu-id="5b21f-194">Pokud dojde ke ztrátě hello umístění v síti, pracovní postup hello končí v chybě.</span><span class="sxs-lookup"><span data-stu-id="5b21f-194">If hello network location is lost, then hello workflow ends in error.</span></span>  <span data-ttu-id="5b21f-195">Když spustíte znovu, bude pokračovat v hello posledního kontrolního bodu, což znamená, že jsou přeskočeny hello pouze soubory, které již byly zkopírovány.</span><span class="sxs-lookup"><span data-stu-id="5b21f-195">When it is started again, it will resume at hello last checkpoint meaning that only hello files that have already been copied are skipped.</span></span>

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }

        Write-Output "All files copied."
    }

<span data-ttu-id="5b21f-196">Protože uživatelské jméno pověření nejsou trvalé po zavolání metody hello [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) aktivity nebo po hello posledního kontrolního bodu, potřebujete tooset hello přihlašovací údaje toonull a potom je znovu načíst z úložiště asset hello po  **Suspend-Workflow** nebo je vyvolán kontrolní bod.</span><span class="sxs-lookup"><span data-stu-id="5b21f-196">Because username credentials are not persisted after you call hello [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) activity or after hello last checkpoint, you need tooset hello credentials toonull and then retrieve them again from hello asset store after **Suspend-Workflow** or checkpoint is called.</span></span>  <span data-ttu-id="5b21f-197">V ostatních případech se může zobrazit hello následující chybová zpráva: *úlohy pracovního postupu hello nelze pokračovat, buď protože trvalosti dat nelze uložit zcela nebo uložit trvalosti dat je poškozeno. Je nutné restartovat hello pracovního postupu.*</span><span class="sxs-lookup"><span data-stu-id="5b21f-197">Otherwise, you may receive hello following error message: *hello workflow job cannot be resumed, either because persistence data could not be saved completely, or saved persistence data has been corrupted. You must restart hello workflow.*</span></span>

<span data-ttu-id="5b21f-198">Hello následující stejný kód ukazuje, jak toohandle to vaše runbooky pracovních postupů Powershellu.</span><span class="sxs-lookup"><span data-stu-id="5b21f-198">hello following same code demonstrates how toohandle this in your PowerShell Workflow runbooks.</span></span>

    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first toocreate hello VM (code not shown)

          # Now add hello VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     }


<span data-ttu-id="5b21f-199">Není požadováno v případě se ověřujete pomocí nakonfigurovaný s hlavní službou účet Spustit jako.</span><span class="sxs-lookup"><span data-stu-id="5b21f-199">This is not required if you are authenticating using a Run As account configured with a service principal.</span></span>  

<span data-ttu-id="5b21f-200">Další informace o kontrolních bodech najdete v tématu [přidání kontrolních bodů tooa pracovní postup skriptu](http://technet.microsoft.com/library/jj574114.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b21f-200">For more information about checkpoints, see [Adding Checkpoints tooa Script Workflow](http://technet.microsoft.com/library/jj574114.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b21f-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b21f-201">Next steps</span></span>
* <span data-ttu-id="5b21f-202">tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="5b21f-202">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
