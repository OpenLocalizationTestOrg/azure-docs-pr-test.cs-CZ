---
title: "aaaStarting a zastavení virtuálních počítačů s Azure Automation – pracovní postup prostředí PowerShell | Microsoft Docs"
description: "Grafické verze scénáře Azure Automation, včetně sady runbook toostart a ukončení klasické virtuální počítače."
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
redirect_document_id: False
ms.openlocfilehash: 273631c7fc5ddb989b3bbdc82b470ac3af6ee482
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a><span data-ttu-id="1a67a-103">Azure Automation scénář – spuštění a zastavení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1a67a-103">Azure Automation scenario - starting and stopping virtual machines</span></span>
<span data-ttu-id="1a67a-104">Tento scénář automatizace Azure zahrnuje runbooky toostart a ukončení klasické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="1a67a-104">This Azure Automation scenario includes runbooks toostart and stop classic virtual machines.</span></span>  <span data-ttu-id="1a67a-105">V tomto scénáři můžete použít pro žádné z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="1a67a-105">You can use this scenario for any of hello following:</span></span>  

* <span data-ttu-id="1a67a-106">Ve svém vlastním prostředí pomocí sady runbook hello bez úprav.</span><span class="sxs-lookup"><span data-stu-id="1a67a-106">Use hello runbooks without modification in your own environment.</span></span>
* <span data-ttu-id="1a67a-107">Upravte funkce tooperform přizpůsobit hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="1a67a-107">Modify hello runbooks tooperform customized functionality.</span></span>  
* <span data-ttu-id="1a67a-108">Volání sady runbook hello z jiné sady runbook v rámci celého řešení.</span><span class="sxs-lookup"><span data-stu-id="1a67a-108">Call hello runbooks from another runbook as part of an overall solution.</span></span>
* <span data-ttu-id="1a67a-109">Pomocí sad runbook hello jako runbook toolearn kurzy vytváření koncepty.</span><span class="sxs-lookup"><span data-stu-id="1a67a-109">Use hello runbooks as tutorials toolearn runbook authoring concepts.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1a67a-110">Grafický</span><span class="sxs-lookup"><span data-stu-id="1a67a-110">Graphical</span></span>](automation-solution-startstopvm-graphical.md)
> * [<span data-ttu-id="1a67a-111">Pracovní postup PowerShellu</span><span class="sxs-lookup"><span data-stu-id="1a67a-111">PowerShell Workflow</span></span>](automation-solution-startstopvm-psworkflow.md)
> 
> 

<span data-ttu-id="1a67a-112">Toto je hello verze sady runbook PowerShell Workflow tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="1a67a-112">This is hello PowerShell Workflow runbook version of this scenario.</span></span> <span data-ttu-id="1a67a-113">Je také k dispozici pomocí [grafické runbooky](automation-solution-startstopvm-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="1a67a-113">It is also available using [graphical runbooks](automation-solution-startstopvm-graphical.md).</span></span>

## <a name="getting-hello-scenario"></a><span data-ttu-id="1a67a-114">Získávání scénář hello</span><span class="sxs-lookup"><span data-stu-id="1a67a-114">Getting hello scenario</span></span>
<span data-ttu-id="1a67a-115">Tento scénář se skládá z dvě runbooky pracovních postupů Powershellu, které si můžete stáhnout z hello následující odkazy.</span><span class="sxs-lookup"><span data-stu-id="1a67a-115">This scenario consists of two PowerShell Workflow runbooks that you can download from hello following links.</span></span>  <span data-ttu-id="1a67a-116">V tématu hello [grafické verze](automation-solution-startstopvm-graphical.md) tohoto scénáře pro grafické runbooky toohello odkazy.</span><span class="sxs-lookup"><span data-stu-id="1a67a-116">See hello [graphical version](automation-solution-startstopvm-graphical.md) of this scenario for links toohello graphical runbooks.</span></span>

| <span data-ttu-id="1a67a-117">Runbook</span><span class="sxs-lookup"><span data-stu-id="1a67a-117">Runbook</span></span> | <span data-ttu-id="1a67a-118">Odkaz</span><span class="sxs-lookup"><span data-stu-id="1a67a-118">Link</span></span> | <span data-ttu-id="1a67a-119">Typ</span><span class="sxs-lookup"><span data-stu-id="1a67a-119">Type</span></span> | <span data-ttu-id="1a67a-120">Popis</span><span class="sxs-lookup"><span data-stu-id="1a67a-120">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1a67a-121">Počáteční AzureVMs</span><span class="sxs-lookup"><span data-stu-id="1a67a-121">Start-AzureVMs</span></span> |[<span data-ttu-id="1a67a-122">Spustit virtuální počítače Azure Classic</span><span class="sxs-lookup"><span data-stu-id="1a67a-122">Start Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |<span data-ttu-id="1a67a-123">Pracovní postup prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1a67a-123">PowerShell Workflow</span></span> |<span data-ttu-id="1a67a-124">Spustí všechny klasické virtuální počítače v Azure subscriptionor všechny virtuální počítače s názvem konkrétní služby.</span><span class="sxs-lookup"><span data-stu-id="1a67a-124">Starts all classic virtual machines in an Azure subscriptionor all virtual machines with a particular service name.</span></span> |
| <span data-ttu-id="1a67a-125">Stop-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="1a67a-125">Stop-AzureVMs</span></span> |[<span data-ttu-id="1a67a-126">Zastavte virtuální počítače Azure Classic</span><span class="sxs-lookup"><span data-stu-id="1a67a-126">Stop Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |<span data-ttu-id="1a67a-127">Pracovní postup prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1a67a-127">PowerShell Workflow</span></span> |<span data-ttu-id="1a67a-128">Zastaví všechny virtuální počítače na účtu automation nebo všechny virtuální počítače s názvem konkrétní služby.</span><span class="sxs-lookup"><span data-stu-id="1a67a-128">Stops all virtual machines in an automation account or all virtual machines with a particular service name.</span></span> |

## <a name="installing-and-configuring-hello-scenario"></a><span data-ttu-id="1a67a-129">Instalace a konfigurace hello scénář</span><span class="sxs-lookup"><span data-stu-id="1a67a-129">Installing and configuring hello scenario</span></span>
### <a name="1-install-hello-runbooks"></a><span data-ttu-id="1a67a-130">1. Instalace sady runbook hello</span><span class="sxs-lookup"><span data-stu-id="1a67a-130">1. Install hello runbooks</span></span>
<span data-ttu-id="1a67a-131">Po stažení hello sady runbook, můžete je importovat pomocí postupu hello v [import Runbooku](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span><span class="sxs-lookup"><span data-stu-id="1a67a-131">After downloading hello runbooks, you can import them using hello procedure in [Importing a Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span></span>

### <a name="2-review-hello-description-and-requirements"></a><span data-ttu-id="1a67a-132">2. Zkontrolujte hello popis a požadavky</span><span class="sxs-lookup"><span data-stu-id="1a67a-132">2. Review hello description and requirements</span></span>
<span data-ttu-id="1a67a-133">sady runbook Hello zahrnují komentáři nápovědy text, který obsahuje popis a požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="1a67a-133">hello runbooks include commented help text that includes a description and required assets.</span></span>  <span data-ttu-id="1a67a-134">Můžete také získat hello stejné informace z tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1a67a-134">You can also get hello same information from this article.</span></span>

### <a name="3-configure-assets"></a><span data-ttu-id="1a67a-135">3. Konfigurace prostředků</span><span class="sxs-lookup"><span data-stu-id="1a67a-135">3. Configure assets</span></span>
<span data-ttu-id="1a67a-136">sady runbook Hello vyžadují hello následující prostředky, které musíte vytvořit a naplnit s příslušnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="1a67a-136">hello runbooks require hello following assets that you must create and populate with appropriate values.</span></span>

| <span data-ttu-id="1a67a-137">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="1a67a-137">Asset Type</span></span> | <span data-ttu-id="1a67a-138">Název prostředku</span><span class="sxs-lookup"><span data-stu-id="1a67a-138">Asset Name</span></span> | <span data-ttu-id="1a67a-139">Popis</span><span class="sxs-lookup"><span data-stu-id="1a67a-139">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1a67a-140">Přihlašovací údaj</span><span class="sxs-lookup"><span data-stu-id="1a67a-140">Credential</span></span> |<span data-ttu-id="1a67a-141">AzureCredential</span><span class="sxs-lookup"><span data-stu-id="1a67a-141">AzureCredential</span></span> |<span data-ttu-id="1a67a-142">Obsahuje přihlašovací údaje pro účet, který má autority toostart a ukončete virtuální počítače v hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="1a67a-142">Contains credentials for an account that has authority toostart and stop virtual machines in hello Azure subscription.</span></span>  <span data-ttu-id="1a67a-143">Alternativně můžete zadat jiný prostředek přihlašovacích údajů v hello **pověření** parametr hello **Add-AzureAccount** aktivity.</span><span class="sxs-lookup"><span data-stu-id="1a67a-143">Alternatively, you can specify another credential asset in hello **Credential** parameter of hello **Add-AzureAccount** activity.</span></span> |
| <span data-ttu-id="1a67a-144">Proměnná</span><span class="sxs-lookup"><span data-stu-id="1a67a-144">Variable</span></span> |<span data-ttu-id="1a67a-145">AzureSubscriptionId</span><span class="sxs-lookup"><span data-stu-id="1a67a-145">AzureSubscriptionId</span></span> |<span data-ttu-id="1a67a-146">Obsahuje ID předplatného hello vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="1a67a-146">Contains hello subscription ID of your Azure subscription.</span></span> |

## <a name="using-hello-scenario"></a><span data-ttu-id="1a67a-147">Pomocí hello scénáře</span><span class="sxs-lookup"><span data-stu-id="1a67a-147">Using hello scenario</span></span>
### <a name="parameters"></a><span data-ttu-id="1a67a-148">Parametry</span><span class="sxs-lookup"><span data-stu-id="1a67a-148">Parameters</span></span>
<span data-ttu-id="1a67a-149">Hello runbooky mají hello následující parametry.</span><span class="sxs-lookup"><span data-stu-id="1a67a-149">hello runbooks each have hello following parameters.</span></span>  <span data-ttu-id="1a67a-150">Zadejte hodnoty všech povinných parametrů a volitelně můžete zadat hodnoty pro ostatní parametry v závislosti na požadavcích.</span><span class="sxs-lookup"><span data-stu-id="1a67a-150">You must provide values for any mandatory parameters and can optionally provide values for other parameters depending on your requirements.</span></span>

| <span data-ttu-id="1a67a-151">Parametr</span><span class="sxs-lookup"><span data-stu-id="1a67a-151">Parameter</span></span> | <span data-ttu-id="1a67a-152">Typ</span><span class="sxs-lookup"><span data-stu-id="1a67a-152">Type</span></span> | <span data-ttu-id="1a67a-153">Povinné</span><span class="sxs-lookup"><span data-stu-id="1a67a-153">Mandatory</span></span> | <span data-ttu-id="1a67a-154">Popis</span><span class="sxs-lookup"><span data-stu-id="1a67a-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1a67a-155">ServiceName</span><span class="sxs-lookup"><span data-stu-id="1a67a-155">ServiceName</span></span> |<span data-ttu-id="1a67a-156">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1a67a-156">string</span></span> |<span data-ttu-id="1a67a-157">Ne</span><span class="sxs-lookup"><span data-stu-id="1a67a-157">No</span></span> |<span data-ttu-id="1a67a-158">Pokud je zadána hodnota, jsou všechny virtuální počítače s tímto názvem služby spustit nebo zastavit.</span><span class="sxs-lookup"><span data-stu-id="1a67a-158">If a value is provided, then all virtual machines with that service name are started or stopped.</span></span>  <span data-ttu-id="1a67a-159">Pokud není zadána žádná hodnota, jsou všechny klasické virtuální počítače v hello předplatného Azure spustit nebo zastavit.</span><span class="sxs-lookup"><span data-stu-id="1a67a-159">If no value is provided, then all classic virtual machines in hello Azure subscription are started or stopped.</span></span> |
| <span data-ttu-id="1a67a-160">AzureSubscriptionIdAssetName</span><span class="sxs-lookup"><span data-stu-id="1a67a-160">AzureSubscriptionIdAssetName</span></span> |<span data-ttu-id="1a67a-161">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1a67a-161">string</span></span> |<span data-ttu-id="1a67a-162">Ne</span><span class="sxs-lookup"><span data-stu-id="1a67a-162">No</span></span> |<span data-ttu-id="1a67a-163">Obsahuje název hello hello [variabilní prostředek](#installing-and-configuring-the-scenario) obsahující ID předplatného hello vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="1a67a-163">Contains hello name of hello [variable asset](#installing-and-configuring-the-scenario) that contains hello subscription ID of your Azure subscription.</span></span>  <span data-ttu-id="1a67a-164">Pokud nezadáte hodnotu, *AzureSubscriptionId* se používá.</span><span class="sxs-lookup"><span data-stu-id="1a67a-164">If you don't specify a value, *AzureSubscriptionId* is used.</span></span> |
| <span data-ttu-id="1a67a-165">AzureCredentialAssetName</span><span class="sxs-lookup"><span data-stu-id="1a67a-165">AzureCredentialAssetName</span></span> |<span data-ttu-id="1a67a-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1a67a-166">string</span></span> |<span data-ttu-id="1a67a-167">Ne</span><span class="sxs-lookup"><span data-stu-id="1a67a-167">No</span></span> |<span data-ttu-id="1a67a-168">Obsahuje název hello hello [asset přihlašovacích údajů](#installing-and-configuring-the-scenario) obsahující hello přihlašovací údaje pro hello runbook toouse.</span><span class="sxs-lookup"><span data-stu-id="1a67a-168">Contains hello name of hello [credential asset](#installing-and-configuring-the-scenario) that contains hello credentials for hello runbook toouse.</span></span>  <span data-ttu-id="1a67a-169">Pokud nezadáte hodnotu, *AzureCredential* se používá.</span><span class="sxs-lookup"><span data-stu-id="1a67a-169">If you don't specify a value, *AzureCredential* is used.</span></span> |

### <a name="starting-hello-runbooks"></a><span data-ttu-id="1a67a-170">Spouštění sad runbook hello</span><span class="sxs-lookup"><span data-stu-id="1a67a-170">Starting hello runbooks</span></span>
<span data-ttu-id="1a67a-171">Můžete použít některou z metod hello v [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) toostart buď hello sady runbook v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="1a67a-171">You can use any of hello methods in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) toostart either of hello runbooks in this scenario.</span></span>

<span data-ttu-id="1a67a-172">Následující vzorové příkazy Hello používá prostředí Windows PowerShell toorun **StartAzureVMs** toostart všechny virtuální počítače s názvem služby hello *MyVMService*.</span><span class="sxs-lookup"><span data-stu-id="1a67a-172">hello following sample commands uses Windows PowerShell toorun **StartAzureVMs** toostart all virtual machines with hello service name *MyVMService*.</span></span>

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a><span data-ttu-id="1a67a-173">Výstup</span><span class="sxs-lookup"><span data-stu-id="1a67a-173">Output</span></span>
<span data-ttu-id="1a67a-174">Hello sady runbook bude [na výstup zprávu](automation-runbook-output-and-messages.md) pro každý virtuální počítač označující, zda text hello spuštění nebo zastavení instrukce byla úspěšně odeslána.</span><span class="sxs-lookup"><span data-stu-id="1a67a-174">hello runbooks will [output a message](automation-runbook-output-and-messages.md) for each virtual machine indicating whether or not hello start or stop instruction was successfully submitted.</span></span>  <span data-ttu-id="1a67a-175">Můžete vyhledat konkrétní řetězec v hello výstup toodetermine hello výsledek pro každou sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="1a67a-175">You can look for a specific string in hello output toodetermine hello result for each runbook.</span></span>  <span data-ttu-id="1a67a-176">řetězce výstup Hello jsou uvedeny v následující tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="1a67a-176">hello possible output strings are listed in hello following table.</span></span>

| <span data-ttu-id="1a67a-177">Runbook</span><span class="sxs-lookup"><span data-stu-id="1a67a-177">Runbook</span></span> | <span data-ttu-id="1a67a-178">Podmínka</span><span class="sxs-lookup"><span data-stu-id="1a67a-178">Condition</span></span> | <span data-ttu-id="1a67a-179">Zpráva</span><span class="sxs-lookup"><span data-stu-id="1a67a-179">Message</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="1a67a-180">Počáteční AzureVMs</span><span class="sxs-lookup"><span data-stu-id="1a67a-180">Start-AzureVMs</span></span> |<span data-ttu-id="1a67a-181">Virtuální počítač je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1a67a-181">Virtual machine is already running</span></span> |<span data-ttu-id="1a67a-182">Můjvp je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1a67a-182">MyVM is already running</span></span> |
| <span data-ttu-id="1a67a-183">Počáteční AzureVMs</span><span class="sxs-lookup"><span data-stu-id="1a67a-183">Start-AzureVMs</span></span> |<span data-ttu-id="1a67a-184">Žádost o spuštění pro virtuální počítač, byla úspěšně odeslána</span><span class="sxs-lookup"><span data-stu-id="1a67a-184">Start request for virtual machine successfully submitted</span></span> |<span data-ttu-id="1a67a-185">Můjvp byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1a67a-185">MyVM has been started</span></span> |
| <span data-ttu-id="1a67a-186">Počáteční AzureVMs</span><span class="sxs-lookup"><span data-stu-id="1a67a-186">Start-AzureVMs</span></span> |<span data-ttu-id="1a67a-187">Žádost o spuštění pro virtuální počítač se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="1a67a-187">Start request for virtual machine failed</span></span> |<span data-ttu-id="1a67a-188">Toostart Můjvp se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="1a67a-188">MyVM failed toostart</span></span> |
| <span data-ttu-id="1a67a-189">Stop-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="1a67a-189">Stop-AzureVMs</span></span> |<span data-ttu-id="1a67a-190">Virtuální počítač je již zastaveno.</span><span class="sxs-lookup"><span data-stu-id="1a67a-190">Virtual machine is already stopped</span></span> |<span data-ttu-id="1a67a-191">Můjvp je již zastaveno.</span><span class="sxs-lookup"><span data-stu-id="1a67a-191">MyVM is already stopped</span></span> |
| <span data-ttu-id="1a67a-192">Stop-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="1a67a-192">Stop-AzureVMs</span></span> |<span data-ttu-id="1a67a-193">Zastavení virtuálního počítače byla úspěšně odeslána žádost</span><span class="sxs-lookup"><span data-stu-id="1a67a-193">Stop request for virtual machine successfully submitted</span></span> |<span data-ttu-id="1a67a-194">Můjvp byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="1a67a-194">MyVM has been stopped</span></span> |
| <span data-ttu-id="1a67a-195">Stop-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="1a67a-195">Stop-AzureVMs</span></span> |<span data-ttu-id="1a67a-196">Žádost o zastavení pro virtuální počítač se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="1a67a-196">Stop request for virtual machine failed</span></span> |<span data-ttu-id="1a67a-197">Toostop Můjvp se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="1a67a-197">MyVM failed toostop</span></span> |

<span data-ttu-id="1a67a-198">Například následující fragment kódu ze sady runbook hello pokusí toostart všechny virtuální počítače s názvem služby hello *MyServiceName*.</span><span class="sxs-lookup"><span data-stu-id="1a67a-198">For example, hello following code snippet from a runbook attempts toostart all virtual machines with hello service name *MyServiceName*.</span></span>  <span data-ttu-id="1a67a-199">Pokud některé z hello žádosti o selhání, spusťte můžete provedeny chyby akce.</span><span class="sxs-lookup"><span data-stu-id="1a67a-199">If any of hello start requests fail, then error actions can be taken.</span></span>

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action tootake in case of success.
        }
        else {
            # Action tootake in case of error.
        }
    }


## <a name="detailed-breakdown"></a><span data-ttu-id="1a67a-200">Podrobné rozdělení</span><span class="sxs-lookup"><span data-stu-id="1a67a-200">Detailed breakdown</span></span>
<span data-ttu-id="1a67a-201">Toto je podrobný rozpis hello sady runbook v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="1a67a-201">Following is a detailed breakdown of hello runbooks in this scenario.</span></span>  <span data-ttu-id="1a67a-202">Tyto informace můžete použít tooeither přizpůsobení sady runbook hello nebo jenom toolearn z nich vlastní scénáře automatizace pro vytváření obsahu.</span><span class="sxs-lookup"><span data-stu-id="1a67a-202">You can use this information tooeither customize hello runbooks or just toolearn from them for authoring your own automation scenarios.</span></span>

### <a name="parameters"></a><span data-ttu-id="1a67a-203">Parametry</span><span class="sxs-lookup"><span data-stu-id="1a67a-203">Parameters</span></span>
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

<span data-ttu-id="1a67a-204">Hello pracovní postup spuštěn získáním hello hodnoty pro hello [vstupní parametry](#using-the-scenario).</span><span class="sxs-lookup"><span data-stu-id="1a67a-204">hello workflow starts by getting hello values for hello [input parameters](#using-the-scenario).</span></span>  <span data-ttu-id="1a67a-205">Pokud nejsou zadány hello asset názvy se používají výchozí názvy.</span><span class="sxs-lookup"><span data-stu-id="1a67a-205">If hello asset names are not provided then default names are used.</span></span>

### <a name="output"></a><span data-ttu-id="1a67a-206">Výstup</span><span class="sxs-lookup"><span data-stu-id="1a67a-206">Output</span></span>
    # Returns strings with status messages
    [OutputType([String])]

<span data-ttu-id="1a67a-207">Tento řádek deklaruje, že bude výstup hello hello runbook řetězec.</span><span class="sxs-lookup"><span data-stu-id="1a67a-207">This line declares that hello output of hello runbook will be a string.</span></span>  <span data-ttu-id="1a67a-208">Tento není povinný, ale je vhodné, když se hello runbook používá jako [podřízeného runbooku](automation-child-runbooks.md) tak, aby věděli, nadřízený runbook výstup hello zadejte tooexpect.</span><span class="sxs-lookup"><span data-stu-id="1a67a-208">This is not required but is a best practice for when hello runbook is used as a [child runbook](automation-child-runbooks.md) so that a parent runbook will know hello output type tooexpect.</span></span>

### <a name="authentication"></a><span data-ttu-id="1a67a-209">Authentication</span><span class="sxs-lookup"><span data-stu-id="1a67a-209">Authentication</span></span>
    # Connect tooAzure and select hello subscription toowork against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

<span data-ttu-id="1a67a-210">Další řádky Hello nastavit hello [pověření](automation-credentials.md) a předplatné Azure, který se použije pro hello zbytek hello runbook.</span><span class="sxs-lookup"><span data-stu-id="1a67a-210">hello next lines set hello [credentials](automation-credentials.md) and Azure subscription that will be used for hello rest of hello runbook.</span></span>
<span data-ttu-id="1a67a-211">Nejprve používáme **Get-AutomationPSCredential** tooget hello asset, který obsahuje hello přihlašovací údaje s přístupem k toostart a ukončete virtuální počítače v hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="1a67a-211">First we use **Get-AutomationPSCredential** tooget hello asset that holds hello credentials with access toostart and stop virtual machines in hello Azure subscription.</span></span> <span data-ttu-id="1a67a-212">**Přidat-AzureAccount** pak používá toto pověření hello tooset asset.</span><span class="sxs-lookup"><span data-stu-id="1a67a-212">**Add-AzureAccount** then uses this asset tooset hello credentials.</span></span>  <span data-ttu-id="1a67a-213">výstup Hello je přiřazena tooa fiktivní proměnné, aby není zahrnut ve výstupu runbook hello.</span><span class="sxs-lookup"><span data-stu-id="1a67a-213">hello output is assigned tooa dummy variable so that it isn't included in hello runbook output.</span></span>  

<span data-ttu-id="1a67a-214">Hello variabilní prostředek s ID se potom načte s předplatným hello **Get-AutomationVariable** a hello předplatné s **Select-AzureSubscription**.</span><span class="sxs-lookup"><span data-stu-id="1a67a-214">hello variable asset with hello subscription ID is then retrieved with **Get-AutomationVariable** and hello subscription set with **Select-AzureSubscription**.</span></span>

### <a name="get-vms"></a><span data-ttu-id="1a67a-215">Získat virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="1a67a-215">Get VMs</span></span>
    # If there is a specific cloud service, then get all VMs in hello service,
    # otherwise get all VMs in hello subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

<span data-ttu-id="1a67a-216">**Get-AzureVM** je použité tooretrieve hello runbook bude fungovat s hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1a67a-216">**Get-AzureVM** is used tooretrieve hello virtual machines hello runbook will work with.</span></span>  <span data-ttu-id="1a67a-217">Pokud je hodnota zadaná v hello **ServiceName** vstupní proměnné a pak pouze hello virtuální počítače s tímto názvem služby jsou načteny.</span><span class="sxs-lookup"><span data-stu-id="1a67a-217">If a value is provided in hello **ServiceName** input variable, then only hello virtual machines with that service name are retrieved.</span></span>  <span data-ttu-id="1a67a-218">Pokud **ServiceName** je prázdný, pak jsou načteny všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="1a67a-218">If **ServiceName** is empty, then all virtual machines are retrieved.</span></span>

### <a name="startstop-virtual-machines-and-send-output"></a><span data-ttu-id="1a67a-219">Spuštění a zastavení virtuálních počítačů a odeslání výstupu</span><span class="sxs-lookup"><span data-stu-id="1a67a-219">Start/Stop virtual machines and send output</span></span>
    # Start each of hello stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # hello VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # hello VM needs toobe started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # hello VM failed toostart, so send notice
                Write-Output ($VM.InstanceName + " failed toostart")
            }
            else
            {
                # hello VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

<span data-ttu-id="1a67a-220">Další řádky Hello projděte každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1a67a-220">hello next lines step through each virtual machine.</span></span>  <span data-ttu-id="1a67a-221">Nejprve hello **PowerState** hello virtuálního počítače je zaškrtnuté toosee Pokud už je spuštěná nebo zastavená, v závislosti na hello runbook.</span><span class="sxs-lookup"><span data-stu-id="1a67a-221">First hello **PowerState** of hello virtual machine is checked toosee if it is already running or stopped, depending on hello runbook.</span></span>  <span data-ttu-id="1a67a-222">Pokud je již ve stavu hello cíl, je odeslána zpráva, toooutput a ukončení runbook hello.</span><span class="sxs-lookup"><span data-stu-id="1a67a-222">If it is already in hello target state, then a message is sent toooutput, and hello runbook ends.</span></span>  <span data-ttu-id="1a67a-223">Pokud ne, pak **Start-AzureVM** nebo **Stop-AzureVM** je použité tooattempt toostart nebo zastavení hello virtuální počítač s tímto výsledkem hello hello požadavek uložené tooa proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a67a-223">If not, then **Start-AzureVM** or **Stop-AzureVM** is used tooattempt toostart or stop hello virtual machine with hello result of hello request stored tooa variable.</span></span>  <span data-ttu-id="1a67a-224">Zprávu se pak odešlou toooutput zadání zda byl úspěšně odeslán požadavek toostart hello nebo zastavení.</span><span class="sxs-lookup"><span data-stu-id="1a67a-224">A message is then sent toooutput specifying whether hello request toostart or stop was submitted successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a67a-225">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a67a-225">Next steps</span></span>
* <span data-ttu-id="1a67a-226">toolearn Další informace o práci s podřízené runbooky, najdete v části [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md)</span><span class="sxs-lookup"><span data-stu-id="1a67a-226">toolearn more about working with child runbooks, see [Child runbooks in Azure Automation](automation-child-runbooks.md)</span></span>
* <span data-ttu-id="1a67a-227">Další informace o toolearn výstup zprávy v průběhu spuštění sady runbook a protokolování toohelp řešení najdete v tématu [Runbook výstup a zprávy v Azure Automation.](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="1a67a-227">toolearn more about output messages during runbook execution and logging toohelp troubleshoot, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md)</span></span>

