---
title: "Spuštění a zastavení virtuálních počítačů s Azure Automation – pracovní postup prostředí PowerShell | Microsoft Docs"
description: "Grafické verze scénář automatizace Azure, včetně sady runbook pro spuštění a zastavení klasické virtuální počítače."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d380bd43-d45d-45af-a5b2-78e7f66263c3
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: FALSE
ms.openlocfilehash: 95a7b02b0d11bf18c398daea48d16e0ead30b543
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a><span data-ttu-id="62270-103">Azure Automation scénář – spuštění a zastavení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="62270-103">Azure Automation scenario - starting and stopping virtual machines</span></span>
<span data-ttu-id="62270-104">Tento scénář automatizace Azure zahrnuje sady runbook pro spuštění a zastavení klasické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="62270-104">This Azure Automation scenario includes runbooks to start and stop classic virtual machines.</span></span>  <span data-ttu-id="62270-105">V tomto scénáři můžete použít pro žádné z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="62270-105">You can use this scenario for any of the following:</span></span>  

* <span data-ttu-id="62270-106">Pomocí sad runbook bez úprav ve svém vlastním prostředí.</span><span class="sxs-lookup"><span data-stu-id="62270-106">Use the runbooks without modification in your own environment.</span></span>
* <span data-ttu-id="62270-107">Úprava sady runbook k provedení vlastní funkce.</span><span class="sxs-lookup"><span data-stu-id="62270-107">Modify the runbooks to perform customized functionality.</span></span>  
* <span data-ttu-id="62270-108">Volání sady runbook z jiné sady runbook v rámci celého řešení.</span><span class="sxs-lookup"><span data-stu-id="62270-108">Call the runbooks from another runbook as part of an overall solution.</span></span>
* <span data-ttu-id="62270-109">Pomocí sady runbook jako kurzy se dozvíte vytváření koncepty sady runbook.</span><span class="sxs-lookup"><span data-stu-id="62270-109">Use the runbooks as tutorials to learn runbook authoring concepts.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="62270-110">Grafický</span><span class="sxs-lookup"><span data-stu-id="62270-110">Graphical</span></span>](automation-solution-startstopvm-graphical.md)
> * [<span data-ttu-id="62270-111">Pracovní postup PowerShellu</span><span class="sxs-lookup"><span data-stu-id="62270-111">PowerShell Workflow</span></span>](automation-solution-startstopvm-psworkflow.md)
> 
> 

<span data-ttu-id="62270-112">Toto je verze sady runbook PowerShell Workflow tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="62270-112">This is the PowerShell Workflow runbook version of this scenario.</span></span> <span data-ttu-id="62270-113">Je také k dispozici pomocí [grafické runbooky](automation-solution-startstopvm-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="62270-113">It is also available using [graphical runbooks](automation-solution-startstopvm-graphical.md).</span></span>

## <a name="getting-the-scenario"></a><span data-ttu-id="62270-114">Získání scénáře</span><span class="sxs-lookup"><span data-stu-id="62270-114">Getting the scenario</span></span>
<span data-ttu-id="62270-115">Tento scénář se skládá ze dvou runbooky pracovních postupů Powershellu, které si můžete stáhnout z následujících odkazů.</span><span class="sxs-lookup"><span data-stu-id="62270-115">This scenario consists of two PowerShell Workflow runbooks that you can download from the following links.</span></span>  <span data-ttu-id="62270-116">Najdete v článku [grafické verze](automation-solution-startstopvm-graphical.md) tohoto scénáře odkazy na grafické runbooky.</span><span class="sxs-lookup"><span data-stu-id="62270-116">See the [graphical version](automation-solution-startstopvm-graphical.md) of this scenario for links to the graphical runbooks.</span></span>

| <span data-ttu-id="62270-117">Runbook</span><span class="sxs-lookup"><span data-stu-id="62270-117">Runbook</span></span> | <span data-ttu-id="62270-118">Odkaz</span><span class="sxs-lookup"><span data-stu-id="62270-118">Link</span></span> | <span data-ttu-id="62270-119">Typ</span><span class="sxs-lookup"><span data-stu-id="62270-119">Type</span></span> | <span data-ttu-id="62270-120">Popis</span><span class="sxs-lookup"><span data-stu-id="62270-120">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="62270-121">Počáteční AzureVMs</span><span class="sxs-lookup"><span data-stu-id="62270-121">Start-AzureVMs</span></span> |[<span data-ttu-id="62270-122">Spustit virtuální počítače Azure Classic</span><span class="sxs-lookup"><span data-stu-id="62270-122">Start Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |<span data-ttu-id="62270-123">Pracovní postup prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="62270-123">PowerShell Workflow</span></span> |<span data-ttu-id="62270-124">Spustí všechny klasické virtuální počítače v Azure subscriptionor všechny virtuální počítače s názvem konkrétní služby.</span><span class="sxs-lookup"><span data-stu-id="62270-124">Starts all classic virtual machines in an Azure subscriptionor all virtual machines with a particular service name.</span></span> |
| <span data-ttu-id="62270-125">Stop-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="62270-125">Stop-AzureVMs</span></span> |[<span data-ttu-id="62270-126">Zastavte virtuální počítače Azure Classic</span><span class="sxs-lookup"><span data-stu-id="62270-126">Stop Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |<span data-ttu-id="62270-127">Pracovní postup prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="62270-127">PowerShell Workflow</span></span> |<span data-ttu-id="62270-128">Zastaví všechny virtuální počítače na účtu automation nebo všechny virtuální počítače s názvem konkrétní služby.</span><span class="sxs-lookup"><span data-stu-id="62270-128">Stops all virtual machines in an automation account or all virtual machines with a particular service name.</span></span> |

## <a name="installing-and-configuring-the-scenario"></a><span data-ttu-id="62270-129">Instalace a konfigurace scénář</span><span class="sxs-lookup"><span data-stu-id="62270-129">Installing and configuring the scenario</span></span>
### <a name="1-install-the-runbooks"></a><span data-ttu-id="62270-130">1. Instalace sady runbook</span><span class="sxs-lookup"><span data-stu-id="62270-130">1. Install the runbooks</span></span>
<span data-ttu-id="62270-131">Po stažení sady runbook, můžete importovat pomocí postupu v [import Runbooku](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span><span class="sxs-lookup"><span data-stu-id="62270-131">After downloading the runbooks, you can import them using the procedure in [Importing a Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span></span>

### <a name="2-review-the-description-and-requirements"></a><span data-ttu-id="62270-132">2. Přečtěte si popis a požadavky</span><span class="sxs-lookup"><span data-stu-id="62270-132">2. Review the description and requirements</span></span>
<span data-ttu-id="62270-133">Sady runbook zahrnují komentáři nápovědy text, který obsahuje popis a požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="62270-133">The runbooks include commented help text that includes a description and required assets.</span></span>  <span data-ttu-id="62270-134">Můžete také získat stejné informace z tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="62270-134">You can also get the same information from this article.</span></span>

### <a name="3-configure-assets"></a><span data-ttu-id="62270-135">3. Konfigurace prostředků</span><span class="sxs-lookup"><span data-stu-id="62270-135">3. Configure assets</span></span>
<span data-ttu-id="62270-136">Sady runbook vyžaduje následující prostředky, které musíte vytvořit a naplnit s příslušnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="62270-136">The runbooks require the following assets that you must create and populate with appropriate values.</span></span>

| <span data-ttu-id="62270-137">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="62270-137">Asset Type</span></span> | <span data-ttu-id="62270-138">Název prostředku</span><span class="sxs-lookup"><span data-stu-id="62270-138">Asset Name</span></span> | <span data-ttu-id="62270-139">Popis</span><span class="sxs-lookup"><span data-stu-id="62270-139">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="62270-140">Přihlašovací údaj</span><span class="sxs-lookup"><span data-stu-id="62270-140">Credential</span></span> |<span data-ttu-id="62270-141">AzureCredential</span><span class="sxs-lookup"><span data-stu-id="62270-141">AzureCredential</span></span> |<span data-ttu-id="62270-142">Obsahuje přihlašovací údaje pro účet, který má oprávnění ke spuštění a zastavení virtuálních počítačů v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="62270-142">Contains credentials for an account that has authority to start and stop virtual machines in the Azure subscription.</span></span>  <span data-ttu-id="62270-143">Alternativně můžete zadat jiný prostředek přihlašovacích údajů v **pověření** parametr **Add-AzureAccount** aktivity.</span><span class="sxs-lookup"><span data-stu-id="62270-143">Alternatively, you can specify another credential asset in the **Credential** parameter of the **Add-AzureAccount** activity.</span></span> |
| <span data-ttu-id="62270-144">Proměnná</span><span class="sxs-lookup"><span data-stu-id="62270-144">Variable</span></span> |<span data-ttu-id="62270-145">AzureSubscriptionId</span><span class="sxs-lookup"><span data-stu-id="62270-145">AzureSubscriptionId</span></span> |<span data-ttu-id="62270-146">Obsahuje ID předplatného vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="62270-146">Contains the subscription ID of your Azure subscription.</span></span> |

## <a name="using-the-scenario"></a><span data-ttu-id="62270-147">Pomocí tohoto scénáře</span><span class="sxs-lookup"><span data-stu-id="62270-147">Using the scenario</span></span>
### <a name="parameters"></a><span data-ttu-id="62270-148">Parametry</span><span class="sxs-lookup"><span data-stu-id="62270-148">Parameters</span></span>
<span data-ttu-id="62270-149">Sady runbook každý musí mít následující parametry.</span><span class="sxs-lookup"><span data-stu-id="62270-149">The runbooks each have the following parameters.</span></span>  <span data-ttu-id="62270-150">Zadejte hodnoty všech povinných parametrů a volitelně můžete zadat hodnoty pro ostatní parametry v závislosti na požadavcích.</span><span class="sxs-lookup"><span data-stu-id="62270-150">You must provide values for any mandatory parameters and can optionally provide values for other parameters depending on your requirements.</span></span>

| <span data-ttu-id="62270-151">Parametr</span><span class="sxs-lookup"><span data-stu-id="62270-151">Parameter</span></span> | <span data-ttu-id="62270-152">Typ</span><span class="sxs-lookup"><span data-stu-id="62270-152">Type</span></span> | <span data-ttu-id="62270-153">Povinné</span><span class="sxs-lookup"><span data-stu-id="62270-153">Mandatory</span></span> | <span data-ttu-id="62270-154">Popis</span><span class="sxs-lookup"><span data-stu-id="62270-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="62270-155">ServiceName</span><span class="sxs-lookup"><span data-stu-id="62270-155">ServiceName</span></span> |<span data-ttu-id="62270-156">Řetězec</span><span class="sxs-lookup"><span data-stu-id="62270-156">string</span></span> |<span data-ttu-id="62270-157">Ne</span><span class="sxs-lookup"><span data-stu-id="62270-157">No</span></span> |<span data-ttu-id="62270-158">Pokud je zadána hodnota, jsou všechny virtuální počítače s tímto názvem služby spustit nebo zastavit.</span><span class="sxs-lookup"><span data-stu-id="62270-158">If a value is provided, then all virtual machines with that service name are started or stopped.</span></span>  <span data-ttu-id="62270-159">Pokud není zadána žádná hodnota, jsou všechny klasické virtuální počítače v rámci předplatného Azure spustit nebo zastavit.</span><span class="sxs-lookup"><span data-stu-id="62270-159">If no value is provided, then all classic virtual machines in the Azure subscription are started or stopped.</span></span> |
| <span data-ttu-id="62270-160">AzureSubscriptionIdAssetName</span><span class="sxs-lookup"><span data-stu-id="62270-160">AzureSubscriptionIdAssetName</span></span> |<span data-ttu-id="62270-161">Řetězec</span><span class="sxs-lookup"><span data-stu-id="62270-161">string</span></span> |<span data-ttu-id="62270-162">Ne</span><span class="sxs-lookup"><span data-stu-id="62270-162">No</span></span> |<span data-ttu-id="62270-163">Obsahuje název [variabilní prostředek](#installing-and-configuring-the-scenario) obsahující ID předplatného vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="62270-163">Contains the name of the [variable asset](#installing-and-configuring-the-scenario) that contains the subscription ID of your Azure subscription.</span></span>  <span data-ttu-id="62270-164">Pokud nezadáte hodnotu, *AzureSubscriptionId* se používá.</span><span class="sxs-lookup"><span data-stu-id="62270-164">If you don't specify a value, *AzureSubscriptionId* is used.</span></span> |
| <span data-ttu-id="62270-165">AzureCredentialAssetName</span><span class="sxs-lookup"><span data-stu-id="62270-165">AzureCredentialAssetName</span></span> |<span data-ttu-id="62270-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="62270-166">string</span></span> |<span data-ttu-id="62270-167">Ne</span><span class="sxs-lookup"><span data-stu-id="62270-167">No</span></span> |<span data-ttu-id="62270-168">Obsahuje název [asset přihlašovacích údajů](#installing-and-configuring-the-scenario) obsahující pověření pro sadu runbook použít.</span><span class="sxs-lookup"><span data-stu-id="62270-168">Contains the name of the [credential asset](#installing-and-configuring-the-scenario) that contains the credentials for the runbook to use.</span></span>  <span data-ttu-id="62270-169">Pokud nezadáte hodnotu, *AzureCredential* se používá.</span><span class="sxs-lookup"><span data-stu-id="62270-169">If you don't specify a value, *AzureCredential* is used.</span></span> |

### <a name="starting-the-runbooks"></a><span data-ttu-id="62270-170">Spuštění sady runbook</span><span class="sxs-lookup"><span data-stu-id="62270-170">Starting the runbooks</span></span>
<span data-ttu-id="62270-171">Můžete použít některou z metod v [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) spustit buď sady runbook v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="62270-171">You can use any of the methods in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) to start either of the runbooks in this scenario.</span></span>

<span data-ttu-id="62270-172">Následující vzorové příkazy prostředí Windows PowerShell používá ke spuštění **StartAzureVMs** k spuštění všech virtuálních počítačů s názvem služby *MyVMService*.</span><span class="sxs-lookup"><span data-stu-id="62270-172">The following sample commands uses Windows PowerShell to run **StartAzureVMs** to start all virtual machines with the service name *MyVMService*.</span></span>

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a><span data-ttu-id="62270-173">Výstup</span><span class="sxs-lookup"><span data-stu-id="62270-173">Output</span></span>
<span data-ttu-id="62270-174">Sady runbook bude [na výstup zprávu](automation-runbook-output-and-messages.md) pro každý virtuální počítač, která určuje, zda byl pokyn spuštění nebo zastavení byla úspěšně odeslána.</span><span class="sxs-lookup"><span data-stu-id="62270-174">The runbooks will [output a message](automation-runbook-output-and-messages.md) for each virtual machine indicating whether or not the start or stop instruction was successfully submitted.</span></span>  <span data-ttu-id="62270-175">Můžete vyhledat konkrétní řetězec ve výstupu určit výsledek pro každou sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="62270-175">You can look for a specific string in the output to determine the result for each runbook.</span></span>  <span data-ttu-id="62270-176">V následující tabulce jsou uvedeny možné výstup řetězce.</span><span class="sxs-lookup"><span data-stu-id="62270-176">The possible output strings are listed in the following table.</span></span>

| <span data-ttu-id="62270-177">Runbook</span><span class="sxs-lookup"><span data-stu-id="62270-177">Runbook</span></span> | <span data-ttu-id="62270-178">Podmínka</span><span class="sxs-lookup"><span data-stu-id="62270-178">Condition</span></span> | <span data-ttu-id="62270-179">Zpráva</span><span class="sxs-lookup"><span data-stu-id="62270-179">Message</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="62270-180">Počáteční AzureVMs</span><span class="sxs-lookup"><span data-stu-id="62270-180">Start-AzureVMs</span></span> |<span data-ttu-id="62270-181">Virtuální počítač je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="62270-181">Virtual machine is already running</span></span> |<span data-ttu-id="62270-182">Můjvp je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="62270-182">MyVM is already running</span></span> |
| <span data-ttu-id="62270-183">Počáteční AzureVMs</span><span class="sxs-lookup"><span data-stu-id="62270-183">Start-AzureVMs</span></span> |<span data-ttu-id="62270-184">Žádost o spuštění pro virtuální počítač, byla úspěšně odeslána</span><span class="sxs-lookup"><span data-stu-id="62270-184">Start request for virtual machine successfully submitted</span></span> |<span data-ttu-id="62270-185">Můjvp byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="62270-185">MyVM has been started</span></span> |
| <span data-ttu-id="62270-186">Počáteční AzureVMs</span><span class="sxs-lookup"><span data-stu-id="62270-186">Start-AzureVMs</span></span> |<span data-ttu-id="62270-187">Žádost o spuštění pro virtuální počítač se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="62270-187">Start request for virtual machine failed</span></span> |<span data-ttu-id="62270-188">Nepodařilo se spustit Můjvp</span><span class="sxs-lookup"><span data-stu-id="62270-188">MyVM failed to start</span></span> |
| <span data-ttu-id="62270-189">Stop-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="62270-189">Stop-AzureVMs</span></span> |<span data-ttu-id="62270-190">Virtuální počítač je již zastaveno.</span><span class="sxs-lookup"><span data-stu-id="62270-190">Virtual machine is already stopped</span></span> |<span data-ttu-id="62270-191">Můjvp je již zastaveno.</span><span class="sxs-lookup"><span data-stu-id="62270-191">MyVM is already stopped</span></span> |
| <span data-ttu-id="62270-192">Stop-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="62270-192">Stop-AzureVMs</span></span> |<span data-ttu-id="62270-193">Zastavení virtuálního počítače byla úspěšně odeslána žádost</span><span class="sxs-lookup"><span data-stu-id="62270-193">Stop request for virtual machine successfully submitted</span></span> |<span data-ttu-id="62270-194">Můjvp byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="62270-194">MyVM has been stopped</span></span> |
| <span data-ttu-id="62270-195">Stop-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="62270-195">Stop-AzureVMs</span></span> |<span data-ttu-id="62270-196">Žádost o zastavení pro virtuální počítač se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="62270-196">Stop request for virtual machine failed</span></span> |<span data-ttu-id="62270-197">Můjvp se nepovedlo zastavit</span><span class="sxs-lookup"><span data-stu-id="62270-197">MyVM failed to stop</span></span> |

<span data-ttu-id="62270-198">Například následující fragment kódu ze sady runbook pokusí spustit všechny virtuální počítače s názvem služby *MyServiceName*.</span><span class="sxs-lookup"><span data-stu-id="62270-198">For example, the following code snippet from a runbook attempts to start all virtual machines with the service name *MyServiceName*.</span></span>  <span data-ttu-id="62270-199">Pokud některý z selhání žádosti o spuštění, můžete provedeny chyby akce.</span><span class="sxs-lookup"><span data-stu-id="62270-199">If any of the start requests fail, then error actions can be taken.</span></span>

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a><span data-ttu-id="62270-200">Podrobné rozdělení</span><span class="sxs-lookup"><span data-stu-id="62270-200">Detailed breakdown</span></span>
<span data-ttu-id="62270-201">Toto je podrobný rozpis sad runbook v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="62270-201">Following is a detailed breakdown of the runbooks in this scenario.</span></span>  <span data-ttu-id="62270-202">Tyto informace slouží k přizpůsobení sady runbook nebo právě dozvědět se od nich vlastní scénáře automatizace pro vytváření obsahu.</span><span class="sxs-lookup"><span data-stu-id="62270-202">You can use this information to either customize the runbooks or just to learn from them for authoring your own automation scenarios.</span></span>

### <a name="parameters"></a><span data-ttu-id="62270-203">Parametry</span><span class="sxs-lookup"><span data-stu-id="62270-203">Parameters</span></span>
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

<span data-ttu-id="62270-204">Pracovní postup spustí získáním hodnoty [vstupní parametry](#using-the-scenario).</span><span class="sxs-lookup"><span data-stu-id="62270-204">The workflow starts by getting the values for the [input parameters](#using-the-scenario).</span></span>  <span data-ttu-id="62270-205">Pokud nejsou zadány názvy datových zdrojů se používají výchozí názvy.</span><span class="sxs-lookup"><span data-stu-id="62270-205">If the asset names are not provided then default names are used.</span></span>

### <a name="output"></a><span data-ttu-id="62270-206">Výstup</span><span class="sxs-lookup"><span data-stu-id="62270-206">Output</span></span>
    # Returns strings with status messages
    [OutputType([String])]

<span data-ttu-id="62270-207">Tento řádek deklaruje, že bude výstup runbooku řetězec.</span><span class="sxs-lookup"><span data-stu-id="62270-207">This line declares that the output of the runbook will be a string.</span></span>  <span data-ttu-id="62270-208">Tento není povinný, ale je vhodné, když sada runbook slouží jako [podřízeného runbooku](automation-child-runbooks.md) tak, aby nadřazený runbook bude vědět, výstupní typ očekávat.</span><span class="sxs-lookup"><span data-stu-id="62270-208">This is not required but is a best practice for when the runbook is used as a [child runbook](automation-child-runbooks.md) so that a parent runbook will know the output type to expect.</span></span>

### <a name="authentication"></a><span data-ttu-id="62270-209">Authentication</span><span class="sxs-lookup"><span data-stu-id="62270-209">Authentication</span></span>
    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

<span data-ttu-id="62270-210">Sada řádků na další [pověření](automation-credentials.md) a předplatné Azure, který se použije pro zbytek sady runbook.</span><span class="sxs-lookup"><span data-stu-id="62270-210">The next lines set the [credentials](automation-credentials.md) and Azure subscription that will be used for the rest of the runbook.</span></span>
<span data-ttu-id="62270-211">Nejprve používáme **Get-AutomationPSCredential** získat asset, který uchovává přihlašovací údaje s přístupem ke spuštění a zastavení virtuálních počítačů v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="62270-211">First we use **Get-AutomationPSCredential** to get the asset that holds the credentials with access to start and stop virtual machines in the Azure subscription.</span></span> <span data-ttu-id="62270-212">**Přidat-AzureAccount** pak použije tento prostředek nastavit přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="62270-212">**Add-AzureAccount** then uses this asset to set the credentials.</span></span>  <span data-ttu-id="62270-213">Výstup je přiřazen fiktivní proměnné tak, aby není zahrnut ve výstupu sady runbook.</span><span class="sxs-lookup"><span data-stu-id="62270-213">The output is assigned to a dummy variable so that it isn't included in the runbook output.</span></span>  

<span data-ttu-id="62270-214">Variabilní prostředek s ID se potom načte se odběru **Get-AutomationVariable** a předplatné s **Select-AzureSubscription**.</span><span class="sxs-lookup"><span data-stu-id="62270-214">The variable asset with the subscription ID is then retrieved with **Get-AutomationVariable** and the subscription set with **Select-AzureSubscription**.</span></span>

### <a name="get-vms"></a><span data-ttu-id="62270-215">Získat virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="62270-215">Get VMs</span></span>
    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

<span data-ttu-id="62270-216">**Get-AzureVM** se používá k načtení sady runbook bude fungovat s virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="62270-216">**Get-AzureVM** is used to retrieve the virtual machines the runbook will work with.</span></span>  <span data-ttu-id="62270-217">Pokud je hodnota zadaná v **ServiceName** vstupní proměnné, pak jsou načteny pouze virtuální počítače s tímto názvem služby.</span><span class="sxs-lookup"><span data-stu-id="62270-217">If a value is provided in the **ServiceName** input variable, then only the virtual machines with that service name are retrieved.</span></span>  <span data-ttu-id="62270-218">Pokud **ServiceName** je prázdný, pak jsou načteny všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="62270-218">If **ServiceName** is empty, then all virtual machines are retrieved.</span></span>

### <a name="startstop-virtual-machines-and-send-output"></a><span data-ttu-id="62270-219">Spuštění a zastavení virtuálních počítačů a odeslání výstupu</span><span class="sxs-lookup"><span data-stu-id="62270-219">Start/Stop virtual machines and send output</span></span>
    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

<span data-ttu-id="62270-220">Dalším krokem řádky prostřednictvím každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="62270-220">The next lines step through each virtual machine.</span></span>  <span data-ttu-id="62270-221">První **PowerState** virtuálního počítače, které se kontroluje k zkontrolujte, jestli je už spuštěná nebo zastavená, v závislosti na sadě runbook.</span><span class="sxs-lookup"><span data-stu-id="62270-221">First the **PowerState** of the virtual machine is checked to see if it is already running or stopped, depending on the runbook.</span></span>  <span data-ttu-id="62270-222">Pokud je již ve stavu cíl, je odeslána zpráva výstup a skončení sady runbook.</span><span class="sxs-lookup"><span data-stu-id="62270-222">If it is already in the target state, then a message is sent to output, and the runbook ends.</span></span>  <span data-ttu-id="62270-223">Pokud ne, pak **Start-AzureVM** nebo **Stop-AzureVM** se používá k pokusu o spuštění nebo zastavení virtuálního počítače s výsledek požadavku uložit do proměnné.</span><span class="sxs-lookup"><span data-stu-id="62270-223">If not, then **Start-AzureVM** or **Stop-AzureVM** is used to attempt to start or stop the virtual machine with the result of the request stored to a variable.</span></span>  <span data-ttu-id="62270-224">Pak je odeslána zpráva do výstupního určující, zda byl úspěšně odeslán požadavek na spuštění nebo zastavení.</span><span class="sxs-lookup"><span data-stu-id="62270-224">A message is then sent to output specifying whether the request to start or stop was submitted successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62270-225">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62270-225">Next steps</span></span>
* <span data-ttu-id="62270-226">Další informace o práci s podřízené runbooky najdete v tématu [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md)</span><span class="sxs-lookup"><span data-stu-id="62270-226">To learn more about working with child runbooks, see [Child runbooks in Azure Automation](automation-child-runbooks.md)</span></span>
* <span data-ttu-id="62270-227">Další informace o zprávách výstup při spuštění sady runbook a protokolování při jeho řešení využít najdete v tématu [Runbook výstup a zprávy v Azure Automation.](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="62270-227">To learn more about output messages during runbook execution and logging to help troubleshoot, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md)</span></span>

