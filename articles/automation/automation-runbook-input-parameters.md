---
title: "Vstupní parametry Runbooku | Microsoft Docs"
description: "Vstupní parametry Runbooku zvýšíte flexibilitu sad runbook tím, že se můžete k předávání dat k sadě runbook, jakmile je spuštěno. Tento článek popisuje různé scénáře, kdy se používá vstupních parametrů v sadách runbook."
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
ms.openlocfilehash: 1ebf32338f5242e72eb2e2daa2da50d231f4b683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="runbook-input-parameters"></a><span data-ttu-id="36e9a-104">Vstupní parametry runbooku</span><span class="sxs-lookup"><span data-stu-id="36e9a-104">Runbook input parameters</span></span>
<span data-ttu-id="36e9a-105">Vstupní parametry Runbooku zvýšíte flexibilitu sad runbook tím, že se můžete k předávání dat do ní při jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="36e9a-105">Runbook input parameters increase the flexibility of runbooks by allowing you to pass data to it when it is started.</span></span> <span data-ttu-id="36e9a-106">Parametry umožňují akce runbook jako cíle pro konkrétní scénáře a různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="36e9a-106">The parameters allow the runbook actions to be targeted for specific scenarios and environments.</span></span> <span data-ttu-id="36e9a-107">V tomto článku jsme vás provede různé scénáře použití vstupních parametrů v sadách runbook.</span><span class="sxs-lookup"><span data-stu-id="36e9a-107">In this article, we will walk you through different scenarios where input parameters are used in runbooks.</span></span>

## <a name="configure-input-parameters"></a><span data-ttu-id="36e9a-108">Konfigurace vstupní parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-108">Configure input parameters</span></span>
<span data-ttu-id="36e9a-109">Vstupní parametry se dá nakonfigurovat v prostředí PowerShell, pracovní postup prostředí PowerShell a grafické runbooky.</span><span class="sxs-lookup"><span data-stu-id="36e9a-109">Input parameters can be configured in PowerShell, PowerShell Workflow, and graphical runbooks.</span></span> <span data-ttu-id="36e9a-110">Sada runbook může mít několik parametrů s různými datovými typy nebo žádné parametry vůbec.</span><span class="sxs-lookup"><span data-stu-id="36e9a-110">A runbook can have multiple parameters with different data types, or no parameters at all.</span></span> <span data-ttu-id="36e9a-111">Vstupní parametry lze povinné nebo volitelné a můžete přiřadit výchozí hodnotu pro volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="36e9a-111">Input parameters can be mandatory or optional, and you can assign a default value for optional parameters.</span></span> <span data-ttu-id="36e9a-112">Vstupní parametry pro sady runbook můžete přiřadit hodnoty, při spuštění prostřednictvím jednoho z dostupných metod.</span><span class="sxs-lookup"><span data-stu-id="36e9a-112">You can assign values to the input parameters for a runbook when you start it through one of the available methods.</span></span> <span data-ttu-id="36e9a-113">Tyto metody zahrnují spuštění runbooku z portálu nebo webové službě.</span><span class="sxs-lookup"><span data-stu-id="36e9a-113">These methods include starting  a runbook from the portal  or a web service.</span></span> <span data-ttu-id="36e9a-114">Můžete také spustit jako podřízené sady runbook, která je volána vložené v jiné sady runbook.</span><span class="sxs-lookup"><span data-stu-id="36e9a-114">You can also start one as a child runbook that is called inline in another runbook.</span></span>

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a><span data-ttu-id="36e9a-115">Konfigurace vstupních parametrů v sadách runbook Powershellu a pracovní postup prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="36e9a-115">Configure input parameters in PowerShell and PowerShell Workflow runbooks</span></span>
<span data-ttu-id="36e9a-116">Prostředí PowerShell a [runbooky pracovních postupů Powershellu](automation-first-runbook-textual.md) ve službě Azure Automation podporují vstupní parametry, které jsou definovány pomocí následující atributy.</span><span class="sxs-lookup"><span data-stu-id="36e9a-116">PowerShell and [PowerShell Workflow runbooks](automation-first-runbook-textual.md) in Azure Automation support input parameters that are defined through the following attributes.</span></span>  

| <span data-ttu-id="36e9a-117">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="36e9a-117">**Property**</span></span> | <span data-ttu-id="36e9a-118">**Popis**</span><span class="sxs-lookup"><span data-stu-id="36e9a-118">**Description**</span></span> |
|:--- |:--- |
| <span data-ttu-id="36e9a-119">Typ</span><span class="sxs-lookup"><span data-stu-id="36e9a-119">Type</span></span> |<span data-ttu-id="36e9a-120">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="36e9a-120">Required.</span></span> <span data-ttu-id="36e9a-121">Datový typ pro hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="36e9a-121">The data type expected for the parameter value.</span></span> <span data-ttu-id="36e9a-122">Libovolného typu .NET je platný.</span><span class="sxs-lookup"><span data-stu-id="36e9a-122">Any .NET type is valid.</span></span> |
| <span data-ttu-id="36e9a-123">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="36e9a-123">Name</span></span> |<span data-ttu-id="36e9a-124">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="36e9a-124">Required.</span></span> <span data-ttu-id="36e9a-125">Název parametru.</span><span class="sxs-lookup"><span data-stu-id="36e9a-125">The name of the parameter.</span></span> <span data-ttu-id="36e9a-126">Toto musí být jedinečný v rámci sady runbook a může obsahovat jenom písmena, číslice nebo znak podtržítka.</span><span class="sxs-lookup"><span data-stu-id="36e9a-126">This must be unique within the runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="36e9a-127">Musí začínat písmenem.</span><span class="sxs-lookup"><span data-stu-id="36e9a-127">It must start with a letter.</span></span> |
| <span data-ttu-id="36e9a-128">Povinné</span><span class="sxs-lookup"><span data-stu-id="36e9a-128">Mandatory</span></span> |<span data-ttu-id="36e9a-129">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-129">Optional.</span></span> <span data-ttu-id="36e9a-130">Určuje, zda musí být zadána hodnota pro parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-130">Specifies whether a value must be provided for the parameter.</span></span> <span data-ttu-id="36e9a-131">Pokud nastavíte **$true**, potom při spuštění runbooku, je třeba zadat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="36e9a-131">If you set this to **$true**, then a value must be provided when the runbook is started.</span></span> <span data-ttu-id="36e9a-132">Pokud nastavíte **$false**, pak hodnota je volitelná.</span><span class="sxs-lookup"><span data-stu-id="36e9a-132">If you set this to **$false**, then a value is optional.</span></span> |
| <span data-ttu-id="36e9a-133">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="36e9a-133">Default value</span></span> |<span data-ttu-id="36e9a-134">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-134">Optional.</span></span>  <span data-ttu-id="36e9a-135">Určuje hodnotu, která se použije pro parametr, pokud není hodnota předaná při spuštění runbooku.</span><span class="sxs-lookup"><span data-stu-id="36e9a-135">Specifies a value that will be used for the parameter if a value is not passed in when the runbook is started.</span></span> <span data-ttu-id="36e9a-136">Výchozí hodnota se dá nastavit pro libovolný parametr a bude automaticky nastavit parametr volitelný bez ohledu na povinná nastavení.</span><span class="sxs-lookup"><span data-stu-id="36e9a-136">A default value can be set for any parameter and will automatically make the parameter optional regardless of the Mandatory setting.</span></span> |

<span data-ttu-id="36e9a-137">Prostředí Windows PowerShell podporuje další atributy vstupní parametry než těch, které zde uvedeny, například ověřování, aliasy, a nastaví parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-137">Windows PowerShell supports more attributes of input parameters than those listed here, like validation, aliases, and parameter sets.</span></span> <span data-ttu-id="36e9a-138">Ale automatizace Azure aktuálně podporuje pouze vstupní parametry, které jsou uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="36e9a-138">However, Azure Automation currently supports only the input parameters listed above.</span></span>

<span data-ttu-id="36e9a-139">Definici parametru v runboocích pracovního postupu Powershellu má následující obecné formulář, kde jsou několik parametrů oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="36e9a-139">A parameter definition in PowerShell Workflow runbooks has the following general form, where multiple parameters are separated by commas.</span></span>

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
> <span data-ttu-id="36e9a-140">Když definujete parametry, pokud neurčíte **povinné** atribut, pak ve výchozím nastavení, považuje volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-140">When you're defining parameters, if you don’t specify the **Mandatory** attribute, then by default, the parameter is considered optional.</span></span> <span data-ttu-id="36e9a-141">Navíc pokud nastavíte výchozí hodnotu pro parametr v runboocích pracovního postupu Powershellu, bude považována pomocí prostředí PowerShell jako volitelný parametr, bez ohledu na to **povinné** hodnota atributu.</span><span class="sxs-lookup"><span data-stu-id="36e9a-141">Also, if you set a default value for a parameter in PowerShell Workflow runbooks, it will be treated by PowerShell as an optional parameter, regardless of the **Mandatory** attribute value.</span></span>
> 
> 

<span data-ttu-id="36e9a-142">Jako příklad nakonfigurujeme vstupní parametry runbooku pracovního postupu Powershellu, který zobrazí podrobnosti o virtuální počítače, jeden virtuální počítač nebo všechny virtuální počítače ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="36e9a-142">As an example, let’s configure the input parameters for a PowerShell Workflow runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="36e9a-143">Tato sada runbook má dva parametry, jak je vidět na následujícím snímku obrazovky: název virtuálního počítače a název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="36e9a-143">This runbook has two parameters as shown in the following screenshot: the name of virtual machine and the name of the resource group.</span></span>

![Pracovní postup automatizace prostředí PowerShell](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

<span data-ttu-id="36e9a-145">V této definici parametru parametry **$VMName** a **$resourceGroupName** jsou jednoduché parametry typu String.</span><span class="sxs-lookup"><span data-stu-id="36e9a-145">In this parameter definition, the parameters **$VMName** and **$resourceGroupName** are simple parameters of type string.</span></span> <span data-ttu-id="36e9a-146">Ale prostředí PowerShell a pracovní postup prostředí PowerShell runbooky podporují všechny typy jednoduché a komplexní typy, jako například **objekt** nebo **PSCredential** pro vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="36e9a-146">However, PowerShell and PowerShell Workflow runbooks support all simple types and complex types, such as **object** or **PSCredential** for input parameters.</span></span>

<span data-ttu-id="36e9a-147">Pokud vaše sada runbook má vstupní parametr typu objektu, použijte prostředí PowerShell zatřiďovací tabulku s (název, hodnotu) páry předat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="36e9a-147">If your runbook has an object type input parameter, then use a PowerShell hashtable with (name,value) pairs to pass in a value.</span></span> <span data-ttu-id="36e9a-148">Například pokud máte v sadě runbook následující parametr:</span><span class="sxs-lookup"><span data-stu-id="36e9a-148">For example, if you have the following parameter in a runbook:</span></span>

     [Parameter (Mandatory = $true)]
     [object] $FullName

<span data-ttu-id="36e9a-149">Můžete poté předat parametr následující hodnotu:</span><span class="sxs-lookup"><span data-stu-id="36e9a-149">Then you can pass the following value to the parameter:</span></span>

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a><span data-ttu-id="36e9a-150">Konfigurace vstupní parametry v grafické runbooky</span><span class="sxs-lookup"><span data-stu-id="36e9a-150">Configure input parameters in graphical runbooks</span></span>
<span data-ttu-id="36e9a-151">K [konfigurace grafický runbook](automation-first-runbook-graphical.md) s vstupní parametry, vytvoříme grafického runbooku, který zobrazí podrobnosti o virtuálních počítačích, buď jeden virtuální počítač nebo všechny virtuální počítače ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="36e9a-151">To [configure a graphical runbook](automation-first-runbook-graphical.md) with input parameters, let’s create a graphical runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="36e9a-152">Konfigurace sady runbook se skládá z dvě hlavní aktivity, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="36e9a-152">Configuring a runbook consists of two major activities, as described below.</span></span>

<span data-ttu-id="36e9a-153">[**Ověření Runbooků pomocí účtu spustit v Azure jako** ](automation-sec-configure-azure-runas-account.md) k ověření pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="36e9a-153">[**Authenticate Runbooks with Azure Run As account**](automation-sec-configure-azure-runas-account.md) to authenticate with Azure.</span></span>

<span data-ttu-id="36e9a-154">[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) získat vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="36e9a-154">[**Get-AzureRmVm**](https://msdn.microsoft.com/library/mt603718.aspx) to get the properties of a virtual machines.</span></span>

<span data-ttu-id="36e9a-155">Můžete použít [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) aktivity k vypsání názvy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36e9a-155">You can use the [**Write-Output**](https://technet.microsoft.com/library/hh849921.aspx) activity to output the names of virtual machines.</span></span> <span data-ttu-id="36e9a-156">Aktivita **Get-AzureRmVm** dva parametry, **název virtuálního počítače** a **název skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="36e9a-156">The activity **Get-AzureRmVm** accepts two parameters, the **virtual machine name** and the **resource group name**.</span></span> <span data-ttu-id="36e9a-157">Vzhledem k tomu, že tyto parametry může vyžadovat různé hodnoty, při každém spuštění sady runbook, můžete přidat vstupních parametrů do runbooku.</span><span class="sxs-lookup"><span data-stu-id="36e9a-157">Since these parameters could require different values each time you start the runbook, you can add input parameters to your runbook.</span></span> <span data-ttu-id="36e9a-158">Tady jsou kroky k přidání vstupních parametrů:</span><span class="sxs-lookup"><span data-stu-id="36e9a-158">Here are the steps to add input parameters:</span></span>

1. <span data-ttu-id="36e9a-159">Vyberte grafický runbook z **Runbooky** okna a potom klikněte na [ **upravit** ](automation-graphical-authoring-intro.md) ho.</span><span class="sxs-lookup"><span data-stu-id="36e9a-159">Select the graphical runbook from the **Runbooks** blade and then click [**Edit**](automation-graphical-authoring-intro.md) it.</span></span>
2. <span data-ttu-id="36e9a-160">V editoru runbooků, klikněte na **vstup a výstup** otevřete **vstup a výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="36e9a-160">From the runbook editor, click **Input and output** to open the **Input and output** blade.</span></span>
   
    ![Grafický runbook automatizace](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. <span data-ttu-id="36e9a-162">**Vstup a výstup** zobrazuje seznam vstupních parametrů, které jsou definovány pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="36e9a-162">The **Input and output** blade displays a list of input parameters that are defined for the runbook.</span></span> <span data-ttu-id="36e9a-163">V tomto okně můžete přidat nový vstupní parametr nebo upravit konfiguraci existující vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-163">On this blade, you can either add a new input parameter or edit the configuration of an existing input parameter.</span></span> <span data-ttu-id="36e9a-164">Chcete-li přidat nový parametr pro sadu runbook, klikněte na tlačítko **přidat vstup** otevřete **vstupní parametr Runbooku** okno.</span><span class="sxs-lookup"><span data-stu-id="36e9a-164">To add a new parameter for the runbook, click **Add input** to open the **Runbook input parameter** blade.</span></span> <span data-ttu-id="36e9a-165">Zde můžete nakonfigurovat následující parametry:</span><span class="sxs-lookup"><span data-stu-id="36e9a-165">There, you can configure the following parameters:</span></span>
   
   | <span data-ttu-id="36e9a-166">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="36e9a-166">**Property**</span></span> | <span data-ttu-id="36e9a-167">**Popis**</span><span class="sxs-lookup"><span data-stu-id="36e9a-167">**Description**</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="36e9a-168">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="36e9a-168">Name</span></span> |<span data-ttu-id="36e9a-169">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="36e9a-169">Required.</span></span>  <span data-ttu-id="36e9a-170">Název parametru.</span><span class="sxs-lookup"><span data-stu-id="36e9a-170">The name of the parameter.</span></span> <span data-ttu-id="36e9a-171">Toto musí být jedinečný v rámci sady runbook a může obsahovat jenom písmena, číslice nebo znak podtržítka.</span><span class="sxs-lookup"><span data-stu-id="36e9a-171">This must be unique within the runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="36e9a-172">Musí začínat písmenem.</span><span class="sxs-lookup"><span data-stu-id="36e9a-172">It must start with a letter.</span></span> |
   | <span data-ttu-id="36e9a-173">Popis</span><span class="sxs-lookup"><span data-stu-id="36e9a-173">Description</span></span> |<span data-ttu-id="36e9a-174">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-174">Optional.</span></span> <span data-ttu-id="36e9a-175">Popis o účelu vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-175">Description about the purpose of input parameter.</span></span> |
   | <span data-ttu-id="36e9a-176">Typ</span><span class="sxs-lookup"><span data-stu-id="36e9a-176">Type</span></span> |<span data-ttu-id="36e9a-177">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-177">Optional.</span></span> <span data-ttu-id="36e9a-178">Datový typ, který je očekávaná hodnota parametru.</span><span class="sxs-lookup"><span data-stu-id="36e9a-178">The data type that's expected for the parameter value.</span></span> <span data-ttu-id="36e9a-179">Parametr podporované typy jsou **řetězec**, **Int32**, **Int64**, **Decimal**, **Boolean**, **data a času**, a **objekt**.</span><span class="sxs-lookup"><span data-stu-id="36e9a-179">Supported parameter types are **String**, **Int32**, **Int64**, **Decimal**, **Boolean**, **DateTime**, and **Object**.</span></span> <span data-ttu-id="36e9a-180">Pokud datový typ není vybraná, je standardně **řetězec**.</span><span class="sxs-lookup"><span data-stu-id="36e9a-180">If a data type is not selected, it defaults to **String**.</span></span> |
   | <span data-ttu-id="36e9a-181">Povinné</span><span class="sxs-lookup"><span data-stu-id="36e9a-181">Mandatory</span></span> |<span data-ttu-id="36e9a-182">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-182">Optional.</span></span> <span data-ttu-id="36e9a-183">Určuje, zda musí být zadána hodnota pro parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-183">Specifies whether a value must be provided for the parameter.</span></span> <span data-ttu-id="36e9a-184">Pokud se rozhodnete **Ano**, potom při spuštění runbooku, je třeba zadat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="36e9a-184">If you choose **yes**, then a value must be provided when the runbook is started.</span></span> <span data-ttu-id="36e9a-185">Pokud se rozhodnete **žádné**, pak hodnota není požadovaná při spuštění runbooku a může být nastaven výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="36e9a-185">If you choose **no**, then a value is not required when the runbook is started, and a default value may be set.</span></span> |
   | <span data-ttu-id="36e9a-186">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="36e9a-186">Default Value</span></span> |<span data-ttu-id="36e9a-187">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-187">Optional.</span></span> <span data-ttu-id="36e9a-188">Určuje hodnotu, která se použije pro parametr, pokud není hodnota předaná při spuštění runbooku.</span><span class="sxs-lookup"><span data-stu-id="36e9a-188">Specifies a value that will be used for the parameter if a value is not passed in when the runbook is started.</span></span> <span data-ttu-id="36e9a-189">Výchozí hodnotu lze nastavit pro parametr, který není povinné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-189">A default value can be set for a parameter that's not mandatory.</span></span> <span data-ttu-id="36e9a-190">Pokud chcete nastavit výchozí hodnotu, zvolte **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="36e9a-190">To set a default value, choose **Custom**.</span></span> <span data-ttu-id="36e9a-191">Tato hodnota se používá, pokud je při spuštění runbooku zadat jinou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="36e9a-191">This value is used unless another value is provided when the runbook is started.</span></span> <span data-ttu-id="36e9a-192">Zvolte **žádné** Pokud nechcete zadat žádnou výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="36e9a-192">Choose **None** if you don’t want to provide any default value.</span></span> |
   
    ![Přidat nové vstup](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. <span data-ttu-id="36e9a-194">Vytvořte dva parametry s následujícími vlastnostmi, které budou používat **Get-AzureRmVm** aktivity:</span><span class="sxs-lookup"><span data-stu-id="36e9a-194">Create two parameters with the following properties that will be used by the **Get-AzureRmVm** activity:</span></span>
   
   * <span data-ttu-id="36e9a-195">**Parametr1:**</span><span class="sxs-lookup"><span data-stu-id="36e9a-195">**Parameter1:**</span></span>
     
     * <span data-ttu-id="36e9a-196">Název – VMName</span><span class="sxs-lookup"><span data-stu-id="36e9a-196">Name - VMName</span></span>
     * <span data-ttu-id="36e9a-197">Typ – řetězec</span><span class="sxs-lookup"><span data-stu-id="36e9a-197">Type - String</span></span>
     * <span data-ttu-id="36e9a-198">Povinné – ne</span><span class="sxs-lookup"><span data-stu-id="36e9a-198">Mandatory - No</span></span>
   * <span data-ttu-id="36e9a-199">**Parameter2:**</span><span class="sxs-lookup"><span data-stu-id="36e9a-199">**Parameter2:**</span></span>
     
     * <span data-ttu-id="36e9a-200">Název – název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="36e9a-200">Name - resourceGroupName</span></span>
     * <span data-ttu-id="36e9a-201">Typ – řetězec</span><span class="sxs-lookup"><span data-stu-id="36e9a-201">Type - String</span></span>
     * <span data-ttu-id="36e9a-202">Povinné – ne</span><span class="sxs-lookup"><span data-stu-id="36e9a-202">Mandatory - No</span></span>
     * <span data-ttu-id="36e9a-203">Výchozí hodnota - vlastní</span><span class="sxs-lookup"><span data-stu-id="36e9a-203">Default value - Custom</span></span>
     * <span data-ttu-id="36e9a-204">Vlastní výchozí hodnota - \<název skupiny prostředků, která obsahuje virtuální počítače ></span><span class="sxs-lookup"><span data-stu-id="36e9a-204">Custom default value - \<Name of the resource group that contains the virtual machines></span></span>
5. <span data-ttu-id="36e9a-205">Jakmile přidáte parametry, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="36e9a-205">Once you add the parameters, click **OK**.</span></span>  <span data-ttu-id="36e9a-206">Teď si můžete zobrazit je do **vstup a výstup okna**.</span><span class="sxs-lookup"><span data-stu-id="36e9a-206">You can now view them in the **Input and output blade**.</span></span> <span data-ttu-id="36e9a-207">Klikněte na tlačítko **OK** znovu a potom klikněte na **Uložit** a **publikovat** runbooku.</span><span class="sxs-lookup"><span data-stu-id="36e9a-207">Click **OK** again, and then click **Save** and **Publish** your runbook.</span></span>

## <a name="assign-values-to-input-parameters-in-runbooks"></a><span data-ttu-id="36e9a-208">Přiřazení hodnoty pro vstupní parametry v sadách runbook</span><span class="sxs-lookup"><span data-stu-id="36e9a-208">Assign values to input parameters in runbooks</span></span>
<span data-ttu-id="36e9a-209">Můžete předat hodnoty pro vstupní parametry v sadách runbook v následujících scénářích.</span><span class="sxs-lookup"><span data-stu-id="36e9a-209">You can pass values to input parameters in runbooks in the following scenarios.</span></span>

### <a name="start-a-runbook-and-assign-parameters"></a><span data-ttu-id="36e9a-210">Spuštění sady runbook a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-210">Start a runbook and assign parameters</span></span>
<span data-ttu-id="36e9a-211">Sady runbook lze spustit mnoho způsobů: prostřednictvím portálu Azure, s webhook, jehož, pomocí rutin prostředí PowerShell, pomocí rozhraní REST API nebo pomocí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="36e9a-211">A runbook can be started many ways: through the Azure portal, with a webhook, with PowerShell cmdlets, with the REST API, or with the SDK.</span></span> <span data-ttu-id="36e9a-212">Níže probereme různé metody pro spuštění sady runbook a přiřazení parametry.</span><span class="sxs-lookup"><span data-stu-id="36e9a-212">Below we discuss different methods for starting a runbook and assigning parameters.</span></span>

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a><span data-ttu-id="36e9a-213">Spusťte publikované sady runbook pomocí portálu Azure a přiřadit parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-213">Start a published runbook by using the Azure portal and assign parameters</span></span>
<span data-ttu-id="36e9a-214">Pokud jste [spuštění runbooku](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), **spuštění Runbooku** otevře se okno a můžete nastavit hodnoty pro parametry, které jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="36e9a-214">When you [start the runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), the **Start Runbook** blade opens and you can configure values for the parameters that you just created.</span></span>

![Začít používat portál](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

<span data-ttu-id="36e9a-216">V popisku pod vstupní pole uvidíte atributy, které byly nastaveny pro parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-216">In the label beneath the input box, you can see the attributes that have been set for the parameter.</span></span> <span data-ttu-id="36e9a-217">Atributy zahrnují povinné nebo volitelné, typ a výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="36e9a-217">Attributes include mandatory or optional, type, and  default value.</span></span> <span data-ttu-id="36e9a-218">V bublině nápovědy vedle názvu parametru se zobrazí všechny klíčové informace, které potřebujete učinit rozhodnutí o vstupní hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="36e9a-218">In the help balloon next to the parameter name, you can see all the key information you need to make decisions about parameter input values.</span></span> <span data-ttu-id="36e9a-219">Tyto informace zahrnují, zda je parametr povinný nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-219">This information includes whether a parameter is mandatory or optional.</span></span> <span data-ttu-id="36e9a-220">Zahrnuje také typ a výchozí hodnota (pokud existuje) a další užitečné poznámky.</span><span class="sxs-lookup"><span data-stu-id="36e9a-220">It also includes the type and default value (if any), and other helpful notes.</span></span>

![Bublině nápovědy](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> <span data-ttu-id="36e9a-222">Podpora parametry typu String **prázdný** hodnoty řetězců.</span><span class="sxs-lookup"><span data-stu-id="36e9a-222">String type parameters support **Empty** String values.</span></span>  <span data-ttu-id="36e9a-223">Zadání **[EmptyString]** v vstupní parametr pole předá prázdný řetězec pro parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-223">Entering **[EmptyString]** in the input parameter box will pass an empty string to the parameter.</span></span> <span data-ttu-id="36e9a-224">Také nepodporují parametry typu řetězec **Null** předáním hodnoty.</span><span class="sxs-lookup"><span data-stu-id="36e9a-224">Also, String type parameters don’t support **Null** values being passed.</span></span> <span data-ttu-id="36e9a-225">Pokud předáte nemáte žádnou hodnotu pro parametr řetězce, pak prostředí PowerShell bude interpretovat jako hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="36e9a-225">If you don’t pass any value to the String parameter, then PowerShell will interpret it as null.</span></span>
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a><span data-ttu-id="36e9a-226">Spustit publikované sady runbook pomocí rutin prostředí PowerShell a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-226">Start a published runbook by using PowerShell cmdlets and assign parameters</span></span>
* <span data-ttu-id="36e9a-227">**Rutiny Azure Resource Manager:** můžete spustit runbook služby automatizace, který byl vytvořen ve skupině prostředků s použitím [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span><span class="sxs-lookup"><span data-stu-id="36e9a-227">**Azure Resource Manager cmdlets:** You can start an Automation runbook that was created in a resource group by using [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span></span>
  
  <span data-ttu-id="36e9a-228">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="36e9a-228">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* <span data-ttu-id="36e9a-229">**Rutiny Azure Service Management:** můžete spustit runbook služby automatizace, který byl vytvořen v do výchozí skupiny prostředků pomocí [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span><span class="sxs-lookup"><span data-stu-id="36e9a-229">**Azure Service Management cmdlets:** You can start an automation runbook that was created in a default resource group by using [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span></span>
  
  <span data-ttu-id="36e9a-230">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="36e9a-230">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> <span data-ttu-id="36e9a-231">Při spuštění sady runbook pomocí rutin prostředí PowerShell, parametr výchozí **MicrosoftApplicationManagementStartedBy** je vytvořena s hodnotou **prostředí PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="36e9a-231">When you start a runbook by using PowerShell cmdlets, a default parameter, **MicrosoftApplicationManagementStartedBy** is created with the value **PowerShell**.</span></span> <span data-ttu-id="36e9a-232">Tento parametr v můžete zobrazit **podrobnosti úlohy** okno.</span><span class="sxs-lookup"><span data-stu-id="36e9a-232">You can view this parameter in the **Job details** blade.</span></span>  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a><span data-ttu-id="36e9a-233">Spuštění sady runbook pomocí sady SDK a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-233">Start a runbook by using an SDK and assign parameters</span></span>
* <span data-ttu-id="36e9a-234">**Azure Resource Manager metoda:** sady runbook můžete spustit pomocí sady SDK programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="36e9a-234">**Azure Resource Manager method:** You can start a runbook by using the SDK of a programming language.</span></span> <span data-ttu-id="36e9a-235">Níže je fragmentu kódu C# pro spuštění sady runbook ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="36e9a-235">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="36e9a-236">Můžete zobrazit všechny kódu v našem [úložiště GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="36e9a-236">You can view all the code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>  
  
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
* <span data-ttu-id="36e9a-237">**Azure Service Management metoda:** sady runbook můžete spustit pomocí sady SDK programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="36e9a-237">**Azure Service Management method:** You can start a runbook by using the SDK of a programming language.</span></span> <span data-ttu-id="36e9a-238">Níže je fragmentu kódu C# pro spuštění sady runbook ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="36e9a-238">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="36e9a-239">Můžete zobrazit všechny kódu v našem [úložiště GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="36e9a-239">You can view all the code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>
  
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
  
  <span data-ttu-id="36e9a-240">Chcete-li spustit tuto metodu, vytvořit adresář k uložení parametry runbooku **VMName** a **resourceGroupName**a jejich hodnoty.</span><span class="sxs-lookup"><span data-stu-id="36e9a-240">To start this method, create a dictionary to store the runbook parameters, **VMName** and  **resourceGroupName**, and their values.</span></span> <span data-ttu-id="36e9a-241">Spusťte sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="36e9a-241">Then start the runbook.</span></span> <span data-ttu-id="36e9a-242">Zde je fragment kódu jazyka C# pro volání metody, která je definována výše.</span><span class="sxs-lookup"><span data-stu-id="36e9a-242">Below is the C# code snippet for calling the method that's defined above.</span></span>
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a><span data-ttu-id="36e9a-243">Spuštění sady runbook pomocí rozhraní REST API a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-243">Start a runbook by using the REST API and assign parameters</span></span>
<span data-ttu-id="36e9a-244">Úlohy runbooku můžete vytvořit a začít s REST API služby Azure Automation pomocí **PUT** metoda s následující identifikátor URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="36e9a-244">A runbook job can be created and started with the Azure Automation REST API by using the **PUT** method with the following request URI.</span></span>

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

<span data-ttu-id="36e9a-245">V identifikátoru URI žádosti vyměňte následující parametry:</span><span class="sxs-lookup"><span data-stu-id="36e9a-245">In the request URI, replace the following parameters:</span></span>

* <span data-ttu-id="36e9a-246">**id předplatného:** ID vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="36e9a-246">**subscription-id:** Your Azure subscription ID.</span></span>  
* <span data-ttu-id="36e9a-247">**Název cloudové služby:** služby název cloudu, na který by měl být požadavek odeslán.</span><span class="sxs-lookup"><span data-stu-id="36e9a-247">**cloud-service-name:** The name of the cloud service to which the request should be sent.</span></span>  
* <span data-ttu-id="36e9a-248">**název účtu Automation:** název účtu automation, který je hostován v rámci zadané cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="36e9a-248">**automation-account-name:** The name of your automation account that's hosted within the specified cloud service.</span></span>  
* <span data-ttu-id="36e9a-249">**id úlohy:** identifikátor GUID pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="36e9a-249">**job-id:** The GUID for the job.</span></span> <span data-ttu-id="36e9a-250">Identifikátory GUID v prostředí PowerShell můžete vytvořit pomocí **[GUID]::NewGuid(). ToString()** příkaz.</span><span class="sxs-lookup"><span data-stu-id="36e9a-250">GUIDs in PowerShell can be created by using the **[GUID]::NewGuid().ToString()** command.</span></span>

<span data-ttu-id="36e9a-251">Chcete-li předat parametry do úlohy runbooku, použijte textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="36e9a-251">In order to pass parameters to the runbook job, use the request body.</span></span> <span data-ttu-id="36e9a-252">Jak dlouho trvá následující dvě vlastnosti zadaný ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="36e9a-252">It takes the following two properties provided in JSON format:</span></span>

* <span data-ttu-id="36e9a-253">**Název sady Runbook:** vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="36e9a-253">**Runbook name:** Required.</span></span> <span data-ttu-id="36e9a-254">Název sady runbook pro úlohu spustit.</span><span class="sxs-lookup"><span data-stu-id="36e9a-254">The name of the runbook for the job to start.</span></span>  
* <span data-ttu-id="36e9a-255">**Parametry sady Runbook:** volitelné.</span><span class="sxs-lookup"><span data-stu-id="36e9a-255">**Runbook parameters:** Optional.</span></span> <span data-ttu-id="36e9a-256">Slovník seznam parametrů v (název, hodnotu) formátu, kde název by měl být typu String a hodnota může být libovolná platná hodnota JSON.</span><span class="sxs-lookup"><span data-stu-id="36e9a-256">A dictionary of the parameter list in (name, value) format where name should be of String type and value can be any valid JSON value.</span></span>

<span data-ttu-id="36e9a-257">Pokud chcete spustit **Get-AzureVMTextual** runbooku, který byl vytvořen již dříve s **VMName** a **resourceGroupName** jako parametry, pomocí následujícího formátu JSON pro textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="36e9a-257">If you want to start the **Get-AzureVMTextual** runbook that was created earlier with **VMName** and **resourceGroupName** as parameters, use the following JSON format for the request body.</span></span>

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

<span data-ttu-id="36e9a-258">Stavový kód HTTP 201 je vrácena, pokud je úloha úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="36e9a-258">A HTTP status code 201 is returned if the job is successfully created.</span></span> <span data-ttu-id="36e9a-259">Další informace o hlavičky odpovědi a text odpovědi, naleznete v článku o tom, jak [vytvořit úlohu runbooku pomocí rozhraní REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span><span class="sxs-lookup"><span data-stu-id="36e9a-259">For more information on response headers and the response body, refer to the article about how to [create a runbook job by using the REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span></span>

### <a name="test-a-runbook-and-assign-parameters"></a><span data-ttu-id="36e9a-260">Otestování sady runbook a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-260">Test a runbook and assign parameters</span></span>
<span data-ttu-id="36e9a-261">Při jste [otestovat verzi konceptu sady runbook](automation-testing-runbook.md) pomocí možnosti testu **testování** otevře se okno a můžete nastavit hodnoty pro parametry, které jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="36e9a-261">When you [test the draft version of your runbook](automation-testing-runbook.md) by using the test option, the **Test** blade opens and you can configure values for the parameters that you just created.</span></span>

![Testování a přiřadit parametry](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a><span data-ttu-id="36e9a-263">Propojit plán s sady runbook a přiřaďte parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-263">Link a schedule to a runbook and assign parameters</span></span>
<span data-ttu-id="36e9a-264">Můžete [propojit plán s](automation-schedules.md) do runbooku, aby sada runbook spuštěna v určitém čase.</span><span class="sxs-lookup"><span data-stu-id="36e9a-264">You can [link a schedule](automation-schedules.md) to your runbook so that the runbook starts at a specific time.</span></span> <span data-ttu-id="36e9a-265">Když vytvoříte plán a sady runbook se používají tyto hodnoty, když se spustila podle plánu přiřadíte vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="36e9a-265">You assign input parameters when you create the schedule, and the runbook will use these values when it is started by the schedule.</span></span> <span data-ttu-id="36e9a-266">Plán nelze uložit, dokud nebudou k dispozici všechny hodnoty povinný parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-266">You can’t save the schedule until all mandatory parameter values are provided.</span></span>

![Parametry plánu a přiřazení](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a><span data-ttu-id="36e9a-268">Vytvoření webhooku pro sadu runbook a přiřazení parametry</span><span class="sxs-lookup"><span data-stu-id="36e9a-268">Create a webhook for a runbook and assign parameters</span></span>
<span data-ttu-id="36e9a-269">Můžete vytvořit [webhooku](automation-webhooks.md) pro runbook a nakonfigurovat vstupní parametry runbooku.</span><span class="sxs-lookup"><span data-stu-id="36e9a-269">You can create a [webhook](automation-webhooks.md) for your runbook and configure runbook input parameters.</span></span> <span data-ttu-id="36e9a-270">Webhook nelze uložit, dokud nebudou k dispozici všechny hodnoty povinný parametr.</span><span class="sxs-lookup"><span data-stu-id="36e9a-270">You can’t save the webhook until all mandatory parameter values are provided.</span></span>

![Vytvoření webhooku a přiřazení parametry](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

<span data-ttu-id="36e9a-272">Při spuštění sady runbook pomocí webhooku, předdefinované vstupní parametr  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  je odeslaný společně vstupní parametry, které jste definovali.</span><span class="sxs-lookup"><span data-stu-id="36e9a-272">When you execute a runbook by using a webhook, the predefined input parameter **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** is sent, along with the input parameters that you defined.</span></span> <span data-ttu-id="36e9a-273">Můžete kliknout na rozšíření **WebhookData** parametr další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="36e9a-273">You can click to expand the **WebhookData** parameter for more details.</span></span>

![Parametr WebhookData](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a><span data-ttu-id="36e9a-275">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36e9a-275">Next steps</span></span>
* <span data-ttu-id="36e9a-276">Další informace o runbook vstup a výstup, najdete v části [Azure Automation: runbook vnořených sad runbook, vstup a výstup](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span><span class="sxs-lookup"><span data-stu-id="36e9a-276">For more information on runbook input and output, see [Azure Automation: runbook input, output, and nested runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span></span>
* <span data-ttu-id="36e9a-277">Podrobnosti o různých způsobech spouštění sady runbook najdete v tématu [spuštění sady runbook](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="36e9a-277">For details about different ways to start a runbook, see [Starting a runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="36e9a-278">Chcete-li upravit textový, přečtěte [úpravy textovou runbooky](automation-edit-textual-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="36e9a-278">To edit a textual runbook, refer to [Editing textual runbooks](automation-edit-textual-runbook.md).</span></span>
* <span data-ttu-id="36e9a-279">Upravit grafický runbook, najdete v tématu [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="36e9a-279">To edit a graphical runbook, refer to [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>

