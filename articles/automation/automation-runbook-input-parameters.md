---
title: "aaaRunbook vstupní parametry | Microsoft Docs"
description: "Vstupní parametry Runbooku zvyšování flexibility hello sad runbook tak, že vám toopass data tooa runbook, jakmile je spuštěno. Tento článek popisuje různé scénáře, kdy se používá vstupních parametrů v sadách runbook."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 4d3dff2c-1f55-498d-9a0e-eee497e5bedb
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sngun
ms.openlocfilehash: f3abaf92382e7d41019616bafb14af23cf98dd9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-input-parameters"></a><span data-ttu-id="488d8-104">Vstupní parametry runbooku</span><span class="sxs-lookup"><span data-stu-id="488d8-104">Runbook input parameters</span></span>
<span data-ttu-id="488d8-105">Vstupní parametry Runbooku zvyšování flexibility hello sad runbook tím, že toopass data tooit při jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="488d8-105">Runbook input parameters increase hello flexibility of runbooks by allowing you toopass data tooit when it is started.</span></span> <span data-ttu-id="488d8-106">Parametry Hello povolit hello runbook akce toobe určený pro konkrétní scénáře a různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="488d8-106">hello parameters allow hello runbook actions toobe targeted for specific scenarios and environments.</span></span> <span data-ttu-id="488d8-107">V tomto článku jsme vás provede různé scénáře použití vstupních parametrů v sadách runbook.</span><span class="sxs-lookup"><span data-stu-id="488d8-107">In this article, we will walk you through different scenarios where input parameters are used in runbooks.</span></span>

## <a name="configure-input-parameters"></a><span data-ttu-id="488d8-108">Konfigurace vstupní parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-108">Configure input parameters</span></span>
<span data-ttu-id="488d8-109">Vstupní parametry se dá nakonfigurovat v prostředí PowerShell, pracovní postup prostředí PowerShell a grafické runbooky.</span><span class="sxs-lookup"><span data-stu-id="488d8-109">Input parameters can be configured in PowerShell, PowerShell Workflow, and graphical runbooks.</span></span> <span data-ttu-id="488d8-110">Sada runbook může mít několik parametrů s různými datovými typy nebo žádné parametry vůbec.</span><span class="sxs-lookup"><span data-stu-id="488d8-110">A runbook can have multiple parameters with different data types, or no parameters at all.</span></span> <span data-ttu-id="488d8-111">Vstupní parametry lze povinné nebo volitelné a můžete přiřadit výchozí hodnotu pro volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="488d8-111">Input parameters can be mandatory or optional, and you can assign a default value for optional parameters.</span></span> <span data-ttu-id="488d8-112">Můžete přiřadit hodnoty toohello vstupní parametry pro sady runbook po spuštění prostřednictvím jednoho z dostupných metod hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-112">You can assign values toohello input parameters for a runbook when you start it through one of hello available methods.</span></span> <span data-ttu-id="488d8-113">Tyto metody zahrnují spuštění runbooku z portálu hello nebo webové službě.</span><span class="sxs-lookup"><span data-stu-id="488d8-113">These methods include starting  a runbook from hello portal  or a web service.</span></span> <span data-ttu-id="488d8-114">Můžete také spustit jako podřízené sady runbook, která je volána vložené v jiné sady runbook.</span><span class="sxs-lookup"><span data-stu-id="488d8-114">You can also start one as a child runbook that is called inline in another runbook.</span></span>

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a><span data-ttu-id="488d8-115">Konfigurace vstupních parametrů v sadách runbook Powershellu a pracovní postup prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="488d8-115">Configure input parameters in PowerShell and PowerShell Workflow runbooks</span></span>
<span data-ttu-id="488d8-116">Prostředí PowerShell a [runbooky pracovních postupů Powershellu](automation-first-runbook-textual.md) ve službě Azure Automation podporují vstupní parametry, které jsou definovány pomocí hello následující atributy.</span><span class="sxs-lookup"><span data-stu-id="488d8-116">PowerShell and [PowerShell Workflow runbooks](automation-first-runbook-textual.md) in Azure Automation support input parameters that are defined through hello following attributes.</span></span>  

| <span data-ttu-id="488d8-117">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="488d8-117">**Property**</span></span> | <span data-ttu-id="488d8-118">**Popis**</span><span class="sxs-lookup"><span data-stu-id="488d8-118">**Description**</span></span> |
|:--- |:--- |
| <span data-ttu-id="488d8-119">Typ</span><span class="sxs-lookup"><span data-stu-id="488d8-119">Type</span></span> |<span data-ttu-id="488d8-120">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="488d8-120">Required.</span></span> <span data-ttu-id="488d8-121">datový typ Hello očekávaná hodnota parametru hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-121">hello data type expected for hello parameter value.</span></span> <span data-ttu-id="488d8-122">Libovolného typu .NET je platný.</span><span class="sxs-lookup"><span data-stu-id="488d8-122">Any .NET type is valid.</span></span> |
| <span data-ttu-id="488d8-123">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="488d8-123">Name</span></span> |<span data-ttu-id="488d8-124">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="488d8-124">Required.</span></span> <span data-ttu-id="488d8-125">Hello název parametru hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-125">hello name of hello parameter.</span></span> <span data-ttu-id="488d8-126">Tomu musí být jedinečný v rámci hello runbook a může obsahovat jenom písmena, číslice nebo podtržítka.</span><span class="sxs-lookup"><span data-stu-id="488d8-126">This must be unique within hello runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="488d8-127">Musí začínat písmenem.</span><span class="sxs-lookup"><span data-stu-id="488d8-127">It must start with a letter.</span></span> |
| <span data-ttu-id="488d8-128">Povinné</span><span class="sxs-lookup"><span data-stu-id="488d8-128">Mandatory</span></span> |<span data-ttu-id="488d8-129">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-129">Optional.</span></span> <span data-ttu-id="488d8-130">Určuje, zda musí být zadána hodnota parametru hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-130">Specifies whether a value must be provided for hello parameter.</span></span> <span data-ttu-id="488d8-131">Pokud nastavíte příliš**$true**, potom při spuštění runbooku hello, je třeba zadat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="488d8-131">If you set this too**$true**, then a value must be provided when hello runbook is started.</span></span> <span data-ttu-id="488d8-132">Pokud nastavíte příliš**$false**, pak hodnota je volitelná.</span><span class="sxs-lookup"><span data-stu-id="488d8-132">If you set this too**$false**, then a value is optional.</span></span> |
| <span data-ttu-id="488d8-133">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="488d8-133">Default value</span></span> |<span data-ttu-id="488d8-134">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-134">Optional.</span></span>  <span data-ttu-id="488d8-135">Určuje hodnotu, která se použije pro parametr hello, pokud není hodnota předaná při spuštění runbooku hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-135">Specifies a value that will be used for hello parameter if a value is not passed in when hello runbook is started.</span></span> <span data-ttu-id="488d8-136">Výchozí hodnota se dá nastavit pro libovolný parametr a bude automaticky nastavit hello parametr volitelný bez ohledu na to hello povinné nastavení.</span><span class="sxs-lookup"><span data-stu-id="488d8-136">A default value can be set for any parameter and will automatically make hello parameter optional regardless of hello Mandatory setting.</span></span> |

<span data-ttu-id="488d8-137">Prostředí Windows PowerShell podporuje další atributy vstupní parametry než těch, které zde uvedeny, například ověřování, aliasy, a nastaví parametr.</span><span class="sxs-lookup"><span data-stu-id="488d8-137">Windows PowerShell supports more attributes of input parameters than those listed here, like validation, aliases, and parameter sets.</span></span> <span data-ttu-id="488d8-138">Ale automatizace Azure aktuálně podporuje pouze hello vstupní parametry uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="488d8-138">However, Azure Automation currently supports only hello input parameters listed above.</span></span>

<span data-ttu-id="488d8-139">Definici parametru v runboocích pracovního postupu Powershellu má hello následující obecné formulář, kde jsou několik parametrů oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="488d8-139">A parameter definition in PowerShell Workflow runbooks has hello following general form, where multiple parameters are separated by commas.</span></span>

   ```
     Param
     (
         [Parameter (Mandatory= $true/$false)]
         [Type] Name1 = <Default value>,

         [Parameter (Mandatory= $true/$false)]
         [Type] Name2 = <Default value>
     )
   ```

> [!NOTE]
> <span data-ttu-id="488d8-140">Když definujete parametry, pokud neurčíte hello **povinné** atribut, pak ve výchozím nastavení, je parametr hello považovat za volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-140">When you're defining parameters, if you don’t specify hello **Mandatory** attribute, then by default, hello parameter is considered optional.</span></span> <span data-ttu-id="488d8-141">Navíc pokud nastavíte výchozí hodnotu pro parametr v runboocích pracovního postupu Powershellu, bude považována pomocí prostředí PowerShell jako volitelný parametr, bez ohledu na to hello **povinné** hodnota atributu.</span><span class="sxs-lookup"><span data-stu-id="488d8-141">Also, if you set a default value for a parameter in PowerShell Workflow runbooks, it will be treated by PowerShell as an optional parameter, regardless of hello **Mandatory** attribute value.</span></span>
> 
> 

<span data-ttu-id="488d8-142">Jako příklad nakonfigurujeme hello vstupní parametry runbooku pracovního postupu Powershellu, který zobrazí podrobnosti o virtuální počítače, jeden virtuální počítač nebo všechny virtuální počítače ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="488d8-142">As an example, let’s configure hello input parameters for a PowerShell Workflow runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="488d8-143">Tato sada runbook má dva parametry, jak ukazuje následující snímek obrazovky hello: hello název virtuálního počítače a hello název skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-143">This runbook has two parameters as shown in hello following screenshot: hello name of virtual machine and hello name of hello resource group.</span></span>

![Pracovní postup automatizace prostředí PowerShell](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

<span data-ttu-id="488d8-145">V této definici parametru hello parametry **$VMName** a **$resourceGroupName** jsou jednoduché parametry typu String.</span><span class="sxs-lookup"><span data-stu-id="488d8-145">In this parameter definition, hello parameters **$VMName** and **$resourceGroupName** are simple parameters of type string.</span></span> <span data-ttu-id="488d8-146">Ale prostředí PowerShell a pracovní postup prostředí PowerShell runbooky podporují všechny typy jednoduché a komplexní typy, jako například **objekt** nebo **PSCredential** pro vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="488d8-146">However, PowerShell and PowerShell Workflow runbooks support all simple types and complex types, such as **object** or **PSCredential** for input parameters.</span></span>

<span data-ttu-id="488d8-147">Pokud vaše sada runbook má vstupní parametr typu objektu, pak použít PowerShell zatřiďovací tabulku s (název, hodnotu) páry toopass hodnotu.</span><span class="sxs-lookup"><span data-stu-id="488d8-147">If your runbook has an object type input parameter, then use a PowerShell hashtable with (name,value) pairs toopass in a value.</span></span> <span data-ttu-id="488d8-148">Například pokud máte hello následující parametr v sadě runbook:</span><span class="sxs-lookup"><span data-stu-id="488d8-148">For example, if you have hello following parameter in a runbook:</span></span>

     [Parameter (Mandatory = $true)]
     [object] $FullName

<span data-ttu-id="488d8-149">Pak můžete předat hello následující parametr toohello hodnoty:</span><span class="sxs-lookup"><span data-stu-id="488d8-149">Then you can pass hello following value toohello parameter:</span></span>

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a><span data-ttu-id="488d8-150">Konfigurace vstupní parametry v grafické runbooky</span><span class="sxs-lookup"><span data-stu-id="488d8-150">Configure input parameters in graphical runbooks</span></span>
<span data-ttu-id="488d8-151">příliš[konfigurace grafický runbook](automation-first-runbook-graphical.md) s vstupní parametry, vytvoříme grafického runbooku, který zobrazí podrobnosti o virtuálních počítačích, buď jeden virtuální počítač nebo všechny virtuální počítače ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="488d8-151">too[configure a graphical runbook](automation-first-runbook-graphical.md) with input parameters, let’s create a graphical runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="488d8-152">Konfigurace sady runbook se skládá z dvě hlavní aktivity, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="488d8-152">Configuring a runbook consists of two major activities, as described below.</span></span>

<span data-ttu-id="488d8-153">[**Ověření Runbooků pomocí účtu spustit v Azure jako** ](automation-sec-configure-azure-runas-account.md) tooauthenticate s Azure.</span><span class="sxs-lookup"><span data-stu-id="488d8-153">[**Authenticate Runbooks with Azure Run As account**](automation-sec-configure-azure-runas-account.md) tooauthenticate with Azure.</span></span>

<span data-ttu-id="488d8-154">[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello vlastnosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="488d8-154">[**Get-AzureRmVm**](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello properties of a virtual machines.</span></span>

<span data-ttu-id="488d8-155">Můžete použít hello [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) aktivity toooutput hello názvy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="488d8-155">You can use hello [**Write-Output**](https://technet.microsoft.com/library/hh849921.aspx) activity toooutput hello names of virtual machines.</span></span> <span data-ttu-id="488d8-156">Hello aktivity **Get-AzureRmVm** přijímá dva parametry hello **název virtuálního počítače** a hello **název skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="488d8-156">hello activity **Get-AzureRmVm** accepts two parameters, hello **virtual machine name** and hello **resource group name**.</span></span> <span data-ttu-id="488d8-157">Vzhledem k tomu, že tyto parametry může vyžadovat různé hodnoty, při každém spuštění sady runbook hello, můžete přidat runbook tooyour vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="488d8-157">Since these parameters could require different values each time you start hello runbook, you can add input parameters tooyour runbook.</span></span> <span data-ttu-id="488d8-158">Zde jsou vstupní parametry tooadd hello kroky:</span><span class="sxs-lookup"><span data-stu-id="488d8-158">Here are hello steps tooadd input parameters:</span></span>

1. <span data-ttu-id="488d8-159">Vyberte hello grafický runbook z hello **Runbooky** okna a pak klikněte na tlačítko [ **upravit** ](automation-graphical-authoring-intro.md) ho.</span><span class="sxs-lookup"><span data-stu-id="488d8-159">Select hello graphical runbook from hello **Runbooks** blade and then click [**Edit**](automation-graphical-authoring-intro.md) it.</span></span>
2. <span data-ttu-id="488d8-160">V editoru runbooků hello, klikněte na **vstup a výstup** tooopen hello **vstup a výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="488d8-160">From hello runbook editor, click **Input and output** tooopen hello **Input and output** blade.</span></span>
   
    ![Grafický runbook automatizace](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. <span data-ttu-id="488d8-162">Hello **vstup a výstup** zobrazuje seznam vstupních parametrů, které jsou definovány pro sadu runbook hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-162">hello **Input and output** blade displays a list of input parameters that are defined for hello runbook.</span></span> <span data-ttu-id="488d8-163">V tomto okně můžete přidat nový vstupní parametr nebo upravit konfiguraci hello existující vstupního parametru.</span><span class="sxs-lookup"><span data-stu-id="488d8-163">On this blade, you can either add a new input parameter or edit hello configuration of an existing input parameter.</span></span> <span data-ttu-id="488d8-164">Klikněte na tlačítko tooadd nový parametr hello sady runbook, **přidat vstup** tooopen hello **vstupní parametr Runbooku** okno.</span><span class="sxs-lookup"><span data-stu-id="488d8-164">tooadd a new parameter for hello runbook, click **Add input** tooopen hello **Runbook input parameter** blade.</span></span> <span data-ttu-id="488d8-165">Zde můžete nakonfigurovat hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="488d8-165">There, you can configure hello following parameters:</span></span>
   
   | <span data-ttu-id="488d8-166">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="488d8-166">**Property**</span></span> | <span data-ttu-id="488d8-167">**Popis**</span><span class="sxs-lookup"><span data-stu-id="488d8-167">**Description**</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="488d8-168">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="488d8-168">Name</span></span> |<span data-ttu-id="488d8-169">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="488d8-169">Required.</span></span>  <span data-ttu-id="488d8-170">Hello název parametru hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-170">hello name of hello parameter.</span></span> <span data-ttu-id="488d8-171">Tomu musí být jedinečný v rámci hello runbook a může obsahovat jenom písmena, číslice nebo podtržítka.</span><span class="sxs-lookup"><span data-stu-id="488d8-171">This must be unique within hello runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="488d8-172">Musí začínat písmenem.</span><span class="sxs-lookup"><span data-stu-id="488d8-172">It must start with a letter.</span></span> |
   | <span data-ttu-id="488d8-173">Popis</span><span class="sxs-lookup"><span data-stu-id="488d8-173">Description</span></span> |<span data-ttu-id="488d8-174">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-174">Optional.</span></span> <span data-ttu-id="488d8-175">Popis o hello účel vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="488d8-175">Description about hello purpose of input parameter.</span></span> |
   | <span data-ttu-id="488d8-176">Typ</span><span class="sxs-lookup"><span data-stu-id="488d8-176">Type</span></span> |<span data-ttu-id="488d8-177">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-177">Optional.</span></span> <span data-ttu-id="488d8-178">Hello datový typ, který je očekávaná hodnota parametru hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-178">hello data type that's expected for hello parameter value.</span></span> <span data-ttu-id="488d8-179">Parametr podporované typy jsou **řetězec**, **Int32**, **Int64**, **Decimal**, **Boolean**, **data a času**, a **objekt**.</span><span class="sxs-lookup"><span data-stu-id="488d8-179">Supported parameter types are **String**, **Int32**, **Int64**, **Decimal**, **Boolean**, **DateTime**, and **Object**.</span></span> <span data-ttu-id="488d8-180">V případě, že datový typ není vybrána, bude výchozí příliš**řetězec**.</span><span class="sxs-lookup"><span data-stu-id="488d8-180">If a data type is not selected, it defaults too**String**.</span></span> |
   | <span data-ttu-id="488d8-181">Povinné</span><span class="sxs-lookup"><span data-stu-id="488d8-181">Mandatory</span></span> |<span data-ttu-id="488d8-182">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-182">Optional.</span></span> <span data-ttu-id="488d8-183">Určuje, zda musí být zadána hodnota parametru hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-183">Specifies whether a value must be provided for hello parameter.</span></span> <span data-ttu-id="488d8-184">Pokud se rozhodnete **Ano**, potom při spuštění runbooku hello, je třeba zadat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="488d8-184">If you choose **yes**, then a value must be provided when hello runbook is started.</span></span> <span data-ttu-id="488d8-185">Pokud se rozhodnete **žádné**, pak hodnota není požadovaná při spuštění runbooku hello a může být nastaven výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="488d8-185">If you choose **no**, then a value is not required when hello runbook is started, and a default value may be set.</span></span> |
   | <span data-ttu-id="488d8-186">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="488d8-186">Default Value</span></span> |<span data-ttu-id="488d8-187">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-187">Optional.</span></span> <span data-ttu-id="488d8-188">Určuje hodnotu, která se použije pro parametr hello, pokud není hodnota předaná při spuštění runbooku hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-188">Specifies a value that will be used for hello parameter if a value is not passed in when hello runbook is started.</span></span> <span data-ttu-id="488d8-189">Výchozí hodnotu lze nastavit pro parametr, který není povinné.</span><span class="sxs-lookup"><span data-stu-id="488d8-189">A default value can be set for a parameter that's not mandatory.</span></span> <span data-ttu-id="488d8-190">Zvolte tooset výchozí hodnotu, **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="488d8-190">tooset a default value, choose **Custom**.</span></span> <span data-ttu-id="488d8-191">Tato hodnota se používá, pokud je při spuštění runbooku hello zadat jinou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="488d8-191">This value is used unless another value is provided when hello runbook is started.</span></span> <span data-ttu-id="488d8-192">Zvolte **žádné** Pokud nechcete, aby tooprovide žádné výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="488d8-192">Choose **None** if you don’t want tooprovide any default value.</span></span> |
   
    ![Přidat nové vstup](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. <span data-ttu-id="488d8-194">Vytvořte dva parametry s hello následující vlastnosti, které se použijí hello **Get-AzureRmVm** aktivity:</span><span class="sxs-lookup"><span data-stu-id="488d8-194">Create two parameters with hello following properties that will be used by hello **Get-AzureRmVm** activity:</span></span>
   
   * <span data-ttu-id="488d8-195">**Parametr1:**</span><span class="sxs-lookup"><span data-stu-id="488d8-195">**Parameter1:**</span></span>
     
     * <span data-ttu-id="488d8-196">Název – VMName</span><span class="sxs-lookup"><span data-stu-id="488d8-196">Name - VMName</span></span>
     * <span data-ttu-id="488d8-197">Typ – řetězec</span><span class="sxs-lookup"><span data-stu-id="488d8-197">Type - String</span></span>
     * <span data-ttu-id="488d8-198">Povinné – ne</span><span class="sxs-lookup"><span data-stu-id="488d8-198">Mandatory - No</span></span>
   * <span data-ttu-id="488d8-199">**Parameter2:**</span><span class="sxs-lookup"><span data-stu-id="488d8-199">**Parameter2:**</span></span>
     
     * <span data-ttu-id="488d8-200">Název – název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="488d8-200">Name - resourceGroupName</span></span>
     * <span data-ttu-id="488d8-201">Typ – řetězec</span><span class="sxs-lookup"><span data-stu-id="488d8-201">Type - String</span></span>
     * <span data-ttu-id="488d8-202">Povinné – ne</span><span class="sxs-lookup"><span data-stu-id="488d8-202">Mandatory - No</span></span>
     * <span data-ttu-id="488d8-203">Výchozí hodnota - vlastní</span><span class="sxs-lookup"><span data-stu-id="488d8-203">Default value - Custom</span></span>
     * <span data-ttu-id="488d8-204">Vlastní výchozí hodnota - \<název skupiny prostředků hello, která obsahuje virtuální počítače hello ></span><span class="sxs-lookup"><span data-stu-id="488d8-204">Custom default value - \<Name of hello resource group that contains hello virtual machines></span></span>
5. <span data-ttu-id="488d8-205">Jakmile přidáte hello parametry, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="488d8-205">Once you add hello parameters, click **OK**.</span></span>  <span data-ttu-id="488d8-206">Můžete nyní zobrazit v hello **vstup a výstup okna**.</span><span class="sxs-lookup"><span data-stu-id="488d8-206">You can now view them in hello **Input and output blade**.</span></span> <span data-ttu-id="488d8-207">Klikněte na tlačítko **OK** znovu a potom klikněte na **Uložit** a **publikovat** runbooku.</span><span class="sxs-lookup"><span data-stu-id="488d8-207">Click **OK** again, and then click **Save** and **Publish** your runbook.</span></span>

## <a name="assign-values-tooinput-parameters-in-runbooks"></a><span data-ttu-id="488d8-208">Přiřadit hodnoty tooinput parametrů v sadách runbook</span><span class="sxs-lookup"><span data-stu-id="488d8-208">Assign values tooinput parameters in runbooks</span></span>
<span data-ttu-id="488d8-209">Hodnoty můžete předat parametry tooinput v sadách runbook ve hello následující scénáře.</span><span class="sxs-lookup"><span data-stu-id="488d8-209">You can pass values tooinput parameters in runbooks in hello following scenarios.</span></span>

### <a name="start-a-runbook-and-assign-parameters"></a><span data-ttu-id="488d8-210">Spuštění sady runbook a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-210">Start a runbook and assign parameters</span></span>
<span data-ttu-id="488d8-211">Sady runbook lze spustit mnoho způsobů: prostřednictvím hello portál Azure, s webhook, jehož, pomocí rutin prostředí PowerShell, hello REST API nebo s hello SDK.</span><span class="sxs-lookup"><span data-stu-id="488d8-211">A runbook can be started many ways: through hello Azure portal, with a webhook, with PowerShell cmdlets, with hello REST API, or with hello SDK.</span></span> <span data-ttu-id="488d8-212">Níže probereme různé metody pro spuštění sady runbook a přiřazení parametry.</span><span class="sxs-lookup"><span data-stu-id="488d8-212">Below we discuss different methods for starting a runbook and assigning parameters.</span></span>

#### <a name="start-a-published-runbook-by-using-hello-azure-portal-and-assign-parameters"></a><span data-ttu-id="488d8-213">Spusťte publikované sady runbook pomocí portálu Azure hello a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-213">Start a published runbook by using hello Azure portal and assign parameters</span></span>
<span data-ttu-id="488d8-214">Pokud jste [spuštění sady runbook hello](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **spuštění Runbooku** otevře se okno a můžete nastavit hodnoty pro parametry hello, které jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="488d8-214">When you [start hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **Start Runbook** blade opens and you can configure values for hello parameters that you just created.</span></span>

![Začít používat portál hello](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

<span data-ttu-id="488d8-216">V popisku hello pod hello vstupní pole uvidíte hello atributy, které byly nastaveny pro parametr hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-216">In hello label beneath hello input box, you can see hello attributes that have been set for hello parameter.</span></span> <span data-ttu-id="488d8-217">Atributy zahrnují povinné nebo volitelné, typ a výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="488d8-217">Attributes include mandatory or optional, type, and  default value.</span></span> <span data-ttu-id="488d8-218">V hello nápovědy bublinách další toohello názvu parametru se zobrazí všechny informace hello klíče, které budete potřebovat toomake rozhodnutí o vstupní hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="488d8-218">In hello help balloon next toohello parameter name, you can see all hello key information you need toomake decisions about parameter input values.</span></span> <span data-ttu-id="488d8-219">Tyto informace zahrnují, zda je parametr povinný nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-219">This information includes whether a parameter is mandatory or optional.</span></span> <span data-ttu-id="488d8-220">Zahrnuje také hello typ a výchozí hodnota (pokud existuje) a další užitečné poznámky.</span><span class="sxs-lookup"><span data-stu-id="488d8-220">It also includes hello type and default value (if any), and other helpful notes.</span></span>

![Bublině nápovědy](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> <span data-ttu-id="488d8-222">Podpora parametry typu String **prázdný** hodnoty řetězců.</span><span class="sxs-lookup"><span data-stu-id="488d8-222">String type parameters support **Empty** String values.</span></span>  <span data-ttu-id="488d8-223">Zadání **[EmptyString]** v hello vstupní parametr pole předá parametru toohello prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="488d8-223">Entering **[EmptyString]** in hello input parameter box will pass an empty string toohello parameter.</span></span> <span data-ttu-id="488d8-224">Také nepodporují parametry typu řetězec **Null** předáním hodnoty.</span><span class="sxs-lookup"><span data-stu-id="488d8-224">Also, String type parameters don’t support **Null** values being passed.</span></span> <span data-ttu-id="488d8-225">Pokud předáte nemáte žádné hodnota toohello řetězcový parametr, pak prostředí PowerShell bude interpretovat jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="488d8-225">If you don’t pass any value toohello String parameter, then PowerShell will interpret it as null.</span></span>
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a><span data-ttu-id="488d8-226">Spustit publikované sady runbook pomocí rutin prostředí PowerShell a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-226">Start a published runbook by using PowerShell cmdlets and assign parameters</span></span>
* <span data-ttu-id="488d8-227">**Rutiny Azure Resource Manager:** můžete spustit runbook služby automatizace, který byl vytvořen ve skupině prostředků s použitím [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span><span class="sxs-lookup"><span data-stu-id="488d8-227">**Azure Resource Manager cmdlets:** You can start an Automation runbook that was created in a resource group by using [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span></span>
  
  <span data-ttu-id="488d8-228">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="488d8-228">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* <span data-ttu-id="488d8-229">**Rutiny Azure Service Management:** můžete spustit runbook služby automatizace, který byl vytvořen v do výchozí skupiny prostředků pomocí [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span><span class="sxs-lookup"><span data-stu-id="488d8-229">**Azure Service Management cmdlets:** You can start an automation runbook that was created in a default resource group by using [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span></span>
  
  <span data-ttu-id="488d8-230">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="488d8-230">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> <span data-ttu-id="488d8-231">Při spuštění sady runbook pomocí rutin prostředí PowerShell, parametr výchozí **MicrosoftApplicationManagementStartedBy** je vytvořena s hodnotou hello **prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="488d8-231">When you start a runbook by using PowerShell cmdlets, a default parameter, **MicrosoftApplicationManagementStartedBy** is created with hello value **PowerShell**.</span></span> <span data-ttu-id="488d8-232">Tento parametr můžete zobrazit v hello **podrobnosti úlohy** okno.</span><span class="sxs-lookup"><span data-stu-id="488d8-232">You can view this parameter in hello **Job details** blade.</span></span>  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a><span data-ttu-id="488d8-233">Spuštění sady runbook pomocí sady SDK a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-233">Start a runbook by using an SDK and assign parameters</span></span>
* <span data-ttu-id="488d8-234">**Azure Resource Manager metoda:** sady runbook můžete spustit pomocí hello SDK programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="488d8-234">**Azure Resource Manager method:** You can start a runbook by using hello SDK of a programming language.</span></span> <span data-ttu-id="488d8-235">Níže je fragmentu kódu C# pro spuštění sady runbook ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="488d8-235">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="488d8-236">Můžete zobrazit všechny hello kódu v našem [úložiště GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="488d8-236">You can view all hello code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>  
  
  ```
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```
* <span data-ttu-id="488d8-237">**Azure Service Management metoda:** sady runbook můžete spustit pomocí hello SDK programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="488d8-237">**Azure Service Management method:** You can start a runbook by using hello SDK of a programming language.</span></span> <span data-ttu-id="488d8-238">Níže je fragmentu kódu C# pro spuštění sady runbook ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="488d8-238">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="488d8-239">Můžete zobrazit všechny hello kódu v našem [úložiště GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="488d8-239">You can view all hello code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>
  
  ```      
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```
  
  <span data-ttu-id="488d8-240">toostart této metody vytvoření hello toostore slovník parametrů sady runbook, **VMName** a **resourceGroupName**a jejich hodnoty.</span><span class="sxs-lookup"><span data-stu-id="488d8-240">toostart this method, create a dictionary toostore hello runbook parameters, **VMName** and  **resourceGroupName**, and their values.</span></span> <span data-ttu-id="488d8-241">Spusťte hello runbook.</span><span class="sxs-lookup"><span data-stu-id="488d8-241">Then start hello runbook.</span></span> <span data-ttu-id="488d8-242">Níže je hello C# fragment kódu pro volání metody hello, který je definovaný výše.</span><span class="sxs-lookup"><span data-stu-id="488d8-242">Below is hello C# code snippet for calling hello method that's defined above.</span></span>
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters toohello dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call hello StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-hello-rest-api-and-assign-parameters"></a><span data-ttu-id="488d8-243">Spuštění sady runbook pomocí hello REST API a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-243">Start a runbook by using hello REST API and assign parameters</span></span>
<span data-ttu-id="488d8-244">Úlohy runbooku můžete vytvořit a začít s hello REST API služby Azure Automation pomocí hello **PUT** metoda s hello následující identifikátor URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="488d8-244">A runbook job can be created and started with hello Azure Automation REST API by using hello **PUT** method with hello following request URI.</span></span>

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

<span data-ttu-id="488d8-245">V identifikátoru URI žádosti hello nahraďte hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="488d8-245">In hello request URI, replace hello following parameters:</span></span>

* <span data-ttu-id="488d8-246">**id předplatného:** ID vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="488d8-246">**subscription-id:** Your Azure subscription ID.</span></span>  
* <span data-ttu-id="488d8-247">**Název cloudové služby:** hello název hello žádost o hello toowhich cloudové služby by měly být odeslány.</span><span class="sxs-lookup"><span data-stu-id="488d8-247">**cloud-service-name:** hello name of hello cloud service toowhich hello request should be sent.</span></span>  
* <span data-ttu-id="488d8-248">**název účtu Automation:** hello název účtu automation, který je hostován v rámci hello zadaný cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="488d8-248">**automation-account-name:** hello name of your automation account that's hosted within hello specified cloud service.</span></span>  
* <span data-ttu-id="488d8-249">**id úlohy:** hello identifikátor GUID pro úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-249">**job-id:** hello GUID for hello job.</span></span> <span data-ttu-id="488d8-250">Identifikátory GUID v prostředí PowerShell můžete vytvořit pomocí hello **[GUID]::NewGuid(). ToString()** příkaz.</span><span class="sxs-lookup"><span data-stu-id="488d8-250">GUIDs in PowerShell can be created by using hello **[GUID]::NewGuid().ToString()** command.</span></span>

<span data-ttu-id="488d8-251">V pořadí toopass parametry toohello runbook úlohy použijte hello textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="488d8-251">In order toopass parameters toohello runbook job, use hello request body.</span></span> <span data-ttu-id="488d8-252">Jak dlouho trvá hello následující dvě vlastnosti zadaný ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="488d8-252">It takes hello following two properties provided in JSON format:</span></span>

* <span data-ttu-id="488d8-253">**Název sady Runbook:** vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="488d8-253">**Runbook name:** Required.</span></span> <span data-ttu-id="488d8-254">Hello název sady runbook hello toostart úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-254">hello name of hello runbook for hello job toostart.</span></span>  
* <span data-ttu-id="488d8-255">**Parametry sady Runbook:** volitelné.</span><span class="sxs-lookup"><span data-stu-id="488d8-255">**Runbook parameters:** Optional.</span></span> <span data-ttu-id="488d8-256">Slovník hello seznam parametrů v (název, hodnotu) formátu, kde název by měl být typu String a hodnota může být libovolná platná hodnota JSON.</span><span class="sxs-lookup"><span data-stu-id="488d8-256">A dictionary of hello parameter list in (name, value) format where name should be of String type and value can be any valid JSON value.</span></span>

<span data-ttu-id="488d8-257">Pokud chcete, aby toostart hello **Get-AzureVMTextual** runbooku, který byl vytvořen již dříve s **VMName** a **resourceGroupName** jako parametry, použijte následující formát JSON hello tělo žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="488d8-257">If you want toostart hello **Get-AzureVMTextual** runbook that was created earlier with **VMName** and **resourceGroupName** as parameters, use hello following JSON format for hello request body.</span></span>

   ```
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

<span data-ttu-id="488d8-258">Stavový kód HTTP 201 je vrácena, pokud hello úlohy je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="488d8-258">A HTTP status code 201 is returned if hello job is successfully created.</span></span> <span data-ttu-id="488d8-259">Další informace o hlavičky odpovědi a hello odpovědi, najdete v části článku toohello o příliš[vytvořit úlohu runbooku pomocí hello REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span><span class="sxs-lookup"><span data-stu-id="488d8-259">For more information on response headers and hello response body, refer toohello article about how too[create a runbook job by using hello REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span></span>

### <a name="test-a-runbook-and-assign-parameters"></a><span data-ttu-id="488d8-260">Otestování sady runbook a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-260">Test a runbook and assign parameters</span></span>
<span data-ttu-id="488d8-261">Při jste [testovací hello verzi konceptu sady runbook](automation-testing-runbook.md) pomocí hello testovací možnost hello **testování** otevře se okno a můžete nastavit hodnoty pro parametry hello, které jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="488d8-261">When you [test hello draft version of your runbook](automation-testing-runbook.md) by using hello test option, hello **Test** blade opens and you can configure values for hello parameters that you just created.</span></span>

![Testování a přiřadit parametry](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-tooa-runbook-and-assign-parameters"></a><span data-ttu-id="488d8-263">Propojit plán tooa runbooku a přiřadit parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-263">Link a schedule tooa runbook and assign parameters</span></span>
<span data-ttu-id="488d8-264">Můžete [propojit plán s](automation-schedules.md) tooyour runbook tak, že hello runbook spouští v určitém čase.</span><span class="sxs-lookup"><span data-stu-id="488d8-264">You can [link a schedule](automation-schedules.md) tooyour runbook so that hello runbook starts at a specific time.</span></span> <span data-ttu-id="488d8-265">Když vytvoříte plán hello a hello runbook použije tyto hodnoty, když se spustí podle plánu hello přiřadíte vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="488d8-265">You assign input parameters when you create hello schedule, and hello runbook will use these values when it is started by hello schedule.</span></span> <span data-ttu-id="488d8-266">Plán hello nelze uložit, dokud nebudou k dispozici všechny hodnoty povinný parametr.</span><span class="sxs-lookup"><span data-stu-id="488d8-266">You can’t save hello schedule until all mandatory parameter values are provided.</span></span>

![Parametry plánu a přiřazení](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a><span data-ttu-id="488d8-268">Vytvoření webhooku pro sadu runbook a přiřazení parametry</span><span class="sxs-lookup"><span data-stu-id="488d8-268">Create a webhook for a runbook and assign parameters</span></span>
<span data-ttu-id="488d8-269">Můžete vytvořit [webhooku](automation-webhooks.md) pro runbook a nakonfigurovat vstupní parametry runbooku.</span><span class="sxs-lookup"><span data-stu-id="488d8-269">You can create a [webhook](automation-webhooks.md) for your runbook and configure runbook input parameters.</span></span> <span data-ttu-id="488d8-270">Hello webhooku nelze uložit, dokud nebudou k dispozici všechny hodnoty povinný parametr.</span><span class="sxs-lookup"><span data-stu-id="488d8-270">You can’t save hello webhook until all mandatory parameter values are provided.</span></span>

![Vytvoření webhooku a přiřazení parametry](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

<span data-ttu-id="488d8-272">Při spuštění sady runbook pomocí webhook, jehož, hello předdefinované vstupní parametr  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  je odeslaný společně hello vstupní parametry, které jste definovali.</span><span class="sxs-lookup"><span data-stu-id="488d8-272">When you execute a runbook by using a webhook, hello predefined input parameter **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** is sent, along with hello input parameters that you defined.</span></span> <span data-ttu-id="488d8-273">Můžete kliknout na tooexpand hello **WebhookData** parametr další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="488d8-273">You can click tooexpand hello **WebhookData** parameter for more details.</span></span>

![Parametr WebhookData](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a><span data-ttu-id="488d8-275">Další kroky</span><span class="sxs-lookup"><span data-stu-id="488d8-275">Next steps</span></span>
* <span data-ttu-id="488d8-276">Další informace o runbook vstup a výstup, najdete v části [Azure Automation: runbook vnořených sad runbook, vstup a výstup](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span><span class="sxs-lookup"><span data-stu-id="488d8-276">For more information on runbook input and output, see [Azure Automation: runbook input, output, and nested runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span></span>
* <span data-ttu-id="488d8-277">Podrobnosti o různé způsoby toostart sady runbook najdete v tématu [spuštění sady runbook](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="488d8-277">For details about different ways toostart a runbook, see [Starting a runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="488d8-278">tooedit textovou sady runbook, najdete v příliš[úpravy textovou runbooky](automation-edit-textual-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="488d8-278">tooedit a textual runbook, refer too[Editing textual runbooks](automation-edit-textual-runbook.md).</span></span>
* <span data-ttu-id="488d8-279">tooedit grafický runbook odkazovat příliš[vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="488d8-279">tooedit a graphical runbook, refer too[Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>

