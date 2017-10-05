---
title: "Sledovat a spravovat úlohy Stream Analytics v prostředí PowerShell | Microsoft Docs"
description: "Naučte se používat rutiny prostředí Azure PowerShell a sledovat a spravovat úlohy Stream Analytics."
keywords: "prostředí Azure powershell, rutin prostředí azure powershell, příkaz prostředí powershell, skriptů prostředí powershell"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 514f454e-d18c-4081-8304-ab48577e15e8
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: e3449ee90cc83c5e823e5948a2a2e7e633c454f1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a><span data-ttu-id="9455b-104">Sledovat a spravovat úlohy Stream Analytics pomocí rutin prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9455b-104">Monitor and manage Stream Analytics jobs with Azure PowerShell cmdlets</span></span>
<span data-ttu-id="9455b-105">Zjistěte, jak sledovat a spravovat prostředky Stream Analytics pomocí rutin prostředí Azure PowerShell a skriptů prostředí powershell, které jsou spouštěny základní úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="9455b-105">Learn how to monitor and manage Stream Analytics resources with Azure PowerShell cmdlets and powershell scripting that execute basic Stream Analytics tasks.</span></span>

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a><span data-ttu-id="9455b-106">Požadavky pro spuštění rutiny prostředí Azure PowerShell pro Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9455b-106">Prerequisites for running Azure PowerShell cmdlets for Stream Analytics</span></span>
* <span data-ttu-id="9455b-107">Vytvořte skupinu prostředků Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="9455b-107">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="9455b-108">Toto je ukázkový skript prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9455b-108">The following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="9455b-109">Prostředí Azure PowerShell informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="9455b-109">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

<span data-ttu-id="9455b-110">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-110">Azure PowerShell 0.9.8:</span></span>  

         # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

<span data-ttu-id="9455b-111">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-111">Azure PowerShell 1.0:</span></span>  

         # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> <span data-ttu-id="9455b-112">Úlohy Stream Analytics vytvořené prostřednictvím kódu programu nemají povoleno ve výchozím nastavení monitorování.</span><span class="sxs-lookup"><span data-stu-id="9455b-112">Stream Analytics jobs created programmatically do not have monitoring enabled by default.</span></span>  <span data-ttu-id="9455b-113">Můžete ručně povolit monitorování na portálu Azure tak, že přejdete na stránku úlohy monitorování a kliknutím na tlačítko Povolit nebo to můžete udělat prostřednictvím kódu programu podle kroků v [Azure Stream Analytics – monitorování Stream Analytics úlohy programově](stream-analytics-monitor-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="9455b-113">You can manually enable monitoring in the Azure Portal by navigating to the job’s Monitor page and clicking the Enable button or you can do this programmatically by following the steps located at [Azure Stream Analytics - Monitor Stream Analytics Jobs Programatically](stream-analytics-monitor-jobs.md).</span></span>
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a><span data-ttu-id="9455b-114">Rutiny Azure PowerShell pro Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9455b-114">Azure PowerShell cmdlets for Stream Analytics</span></span>
<span data-ttu-id="9455b-115">Následující rutiny prostředí Azure PowerShell můžete použít ke sledování a správě úlohy Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="9455b-115">The following Azure PowerShell cmdlets can be used to monitor and manage Azure Stream Analytics jobs.</span></span> <span data-ttu-id="9455b-116">Všimněte si, že prostředí Azure PowerShell má různé verze.</span><span class="sxs-lookup"><span data-stu-id="9455b-116">Note that Azure PowerShell has different versions.</span></span> 
<span data-ttu-id="9455b-117">**V uvedených příkladech je první příkaz pro prostředí Azure PowerShell 0.9.8, v druhém příkazu je pro Azure PowerShell 1.0.**</span><span class="sxs-lookup"><span data-stu-id="9455b-117">**In the examples listed the first command is for Azure PowerShell 0.9.8, the second command is for Azure PowerShell 1.0.**</span></span> <span data-ttu-id="9455b-118">Příkazy Azure PowerShell 1.0, bude mít vždy "AzureRM" v příkazu.</span><span class="sxs-lookup"><span data-stu-id="9455b-118">The Azure PowerShell 1.0 commands will always have "AzureRM" in the command.</span></span>

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a><span data-ttu-id="9455b-119">Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="9455b-119">Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="9455b-120">Zobrazí všechny úlohy služby Stream Analytics, které jsou definované v předplatného Azure nebo zadaná skupina prostředků, nebo získá úlohy informace o konkrétní úloze ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="9455b-120">Lists all Stream Analytics jobs defined in the Azure subscription or specified resource group, or gets job information about a specific job within a resource group.</span></span>

<span data-ttu-id="9455b-121">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-121">**Example 1**</span></span>

<span data-ttu-id="9455b-122">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-122">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob

<span data-ttu-id="9455b-123">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-123">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob

<span data-ttu-id="9455b-124">Tento příkaz prostředí PowerShell vrátí informace o všechny úlohy služby Stream Analytics v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="9455b-124">This PowerShell command returns information about all the Stream Analytics jobs in the Azure subscription.</span></span>

<span data-ttu-id="9455b-125">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="9455b-125">**Example 2**</span></span>

<span data-ttu-id="9455b-126">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-126">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

<span data-ttu-id="9455b-127">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-127">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

<span data-ttu-id="9455b-128">Tento příkaz prostředí PowerShell vrátí informace o všechny úlohy služby Stream Analytics ve skupině prostředků StreamAnalytics výchozí střed USA.</span><span class="sxs-lookup"><span data-stu-id="9455b-128">This PowerShell command returns information about all the Stream Analytics jobs in the resource group StreamAnalytics-Default-Central-US.</span></span>

<span data-ttu-id="9455b-129">**Příklad 3**</span><span class="sxs-lookup"><span data-stu-id="9455b-129">**Example 3**</span></span>

<span data-ttu-id="9455b-130">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-130">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

<span data-ttu-id="9455b-131">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-131">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

<span data-ttu-id="9455b-132">Tento příkaz prostředí PowerShell vrátí informace o úloze Stream Analytics StreamingJob ve skupině prostředků StreamAnalytics výchozí střed USA.</span><span class="sxs-lookup"><span data-stu-id="9455b-132">This PowerShell command returns information about the Stream Analytics job StreamingJob in the resource group StreamAnalytics-Default-Central-US.</span></span>

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a><span data-ttu-id="9455b-133">Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="9455b-133">Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="9455b-134">Zobrazí seznam všech vstupních hodnot, které jsou definované v zadané úloze Stream Analytics, nebo získá informace o specifický vstup.</span><span class="sxs-lookup"><span data-stu-id="9455b-134">Lists all of the inputs that are defined in a specified Stream Analytics job, or gets information about a specific input.</span></span>

<span data-ttu-id="9455b-135">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-135">**Example 1**</span></span>

<span data-ttu-id="9455b-136">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-136">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="9455b-137">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-137">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="9455b-138">Tento příkaz prostředí PowerShell vrátí informace o všech vstupů definované v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-138">This PowerShell command returns information about all the inputs defined in the job StreamingJob.</span></span>

<span data-ttu-id="9455b-139">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="9455b-139">**Example 2**</span></span>

<span data-ttu-id="9455b-140">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-140">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

<span data-ttu-id="9455b-141">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-141">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

<span data-ttu-id="9455b-142">Tento příkaz prostředí PowerShell vrátí informace o vstup s názvem EntryStream definované v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-142">This PowerShell command returns information about the input named EntryStream defined in the job StreamingJob.</span></span>

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a><span data-ttu-id="9455b-143">Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="9455b-143">Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="9455b-144">Zobrazí seznam všech výstupů, které jsou definované v zadané úloze Stream Analytics, nebo získá informace o konkrétní výstup.</span><span class="sxs-lookup"><span data-stu-id="9455b-144">Lists all of the outputs that are defined in a specified Stream Analytics job, or gets information about a specific output.</span></span>

<span data-ttu-id="9455b-145">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-145">**Example 1**</span></span>

<span data-ttu-id="9455b-146">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-146">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="9455b-147">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-147">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="9455b-148">Tento příkaz prostředí PowerShell vrátí informace o výstupy definované v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-148">This PowerShell command returns information about the outputs defined in the job StreamingJob.</span></span>

<span data-ttu-id="9455b-149">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="9455b-149">**Example 2**</span></span>

<span data-ttu-id="9455b-150">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-150">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

<span data-ttu-id="9455b-151">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-151">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

<span data-ttu-id="9455b-152">Tento příkaz prostředí PowerShell vrátí informace o výstup s názvem definované v úloze StreamingJob výstup.</span><span class="sxs-lookup"><span data-stu-id="9455b-152">This PowerShell command returns information about the output named Output defined in the job StreamingJob.</span></span>

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a><span data-ttu-id="9455b-153">Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota</span><span class="sxs-lookup"><span data-stu-id="9455b-153">Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota</span></span>
<span data-ttu-id="9455b-154">Získá informace o kvótu počtu jednotek v zadané oblasti streamování.</span><span class="sxs-lookup"><span data-stu-id="9455b-154">Gets information about the quota of streaming units in a specified region.</span></span>

<span data-ttu-id="9455b-155">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-155">**Example 1**</span></span>

<span data-ttu-id="9455b-156">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-156">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

<span data-ttu-id="9455b-157">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-157">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

<span data-ttu-id="9455b-158">Tento příkaz prostředí PowerShell vrátí informace o kvóta a využití jednotek streamování v oblasti, střed USA.</span><span class="sxs-lookup"><span data-stu-id="9455b-158">This PowerShell command returns information about the quota and usage of streaming units in the Central US region.</span></span>

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a><span data-ttu-id="9455b-159">Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation</span><span class="sxs-lookup"><span data-stu-id="9455b-159">Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation</span></span>
<span data-ttu-id="9455b-160">Získá informace o konkrétní transformaci definované v úloze Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="9455b-160">Gets information about a specific transformation defined in a Stream Analytics job.</span></span>

<span data-ttu-id="9455b-161">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-161">**Example 1**</span></span>

<span data-ttu-id="9455b-162">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-162">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

<span data-ttu-id="9455b-163">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-163">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

<span data-ttu-id="9455b-164">Tento příkaz prostředí PowerShell vrátí informace o transformaci názvem StreamingJob v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-164">This PowerShell command returns information about the transformation called StreamingJob in the job StreamingJob.</span></span>

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a><span data-ttu-id="9455b-165">Nové AzureStreamAnalyticsInput | Nové AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="9455b-165">New-AzureStreamAnalyticsInput | New-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="9455b-166">Vytvoří nový vstup v rámci úlohy Stream Analytics nebo aktualizuje existující zadaný vstup.</span><span class="sxs-lookup"><span data-stu-id="9455b-166">Creates a new input within a Stream Analytics job, or updates an existing specified input.</span></span>

<span data-ttu-id="9455b-167">Název vstupu mohou být zadané v souboru .json nebo na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="9455b-167">The name of the input can be specified in the .json file or on the command line.</span></span> <span data-ttu-id="9455b-168">Pokud jsou zadány oba název na příkazovém řádku musí být stejná jako ta, v souboru.</span><span class="sxs-lookup"><span data-stu-id="9455b-168">If both are specified, the name on the command line must be the same as the one in the file.</span></span>

<span data-ttu-id="9455b-169">Pokud jste zadejte vstup, který již existuje a není zadán parametr-Force, rutina zobrazí dotaz, zda chcete nahradit existující vstup.</span><span class="sxs-lookup"><span data-stu-id="9455b-169">If you specify an input that already exists and do not specify the –Force parameter, the cmdlet will ask whether or not to replace the existing input.</span></span>

<span data-ttu-id="9455b-170">Pokud zadáte – vynutit parametr a zadejte existující zadejte název, vstup se nahradí bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9455b-170">If you specify the –Force parameter and specify an existing input name, the input will be replaced without confirmation.</span></span>

<span data-ttu-id="9455b-171">Podrobné informace o struktuře souboru JSON a obsah, najdete v části [vytvořit vstup (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] části [datového proudu Analytics Management REST API referenční dokumentace knihovny][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="9455b-171">For detailed information on the JSON file structure and contents, refer to the [Create Input (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-input] section of the [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="9455b-172">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-172">**Example 1**</span></span>

<span data-ttu-id="9455b-173">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-173">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

<span data-ttu-id="9455b-174">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-174">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

<span data-ttu-id="9455b-175">Tento příkaz prostředí PowerShell vytvoří nový vstup ze souboru Input.json.</span><span class="sxs-lookup"><span data-stu-id="9455b-175">This PowerShell command creates a new input from the file Input.json.</span></span> <span data-ttu-id="9455b-176">Pokud stávající vstup se zadaným v souboru definice vstupní názvem je již definován, rutina zobrazí dotaz, zda chcete ho nahradit.</span><span class="sxs-lookup"><span data-stu-id="9455b-176">If an existing input with the name specified in the input definition file is already defined, the cmdlet will ask whether or not to replace it.</span></span>

<span data-ttu-id="9455b-177">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="9455b-177">**Example 2**</span></span>

<span data-ttu-id="9455b-178">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-178">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

<span data-ttu-id="9455b-179">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-179">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

<span data-ttu-id="9455b-180">Tento příkaz prostředí PowerShell vytvoří nové vstup do úlohy názvem EntryStream.</span><span class="sxs-lookup"><span data-stu-id="9455b-180">This PowerShell command creates a new input in the job called EntryStream.</span></span> <span data-ttu-id="9455b-181">Pokud stávající vstup s tímto názvem je již definován, rutina zobrazí dotaz, zda chcete ho nahradit.</span><span class="sxs-lookup"><span data-stu-id="9455b-181">If an existing input with this name is already defined, the cmdlet will ask whether or not to replace it.</span></span>

<span data-ttu-id="9455b-182">**Příklad 3**</span><span class="sxs-lookup"><span data-stu-id="9455b-182">**Example 3**</span></span>

<span data-ttu-id="9455b-183">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-183">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

<span data-ttu-id="9455b-184">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-184">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

<span data-ttu-id="9455b-185">Tento příkaz prostředí PowerShell nahradí definici existující vstupní zdroj s názvem EntryStream s definicí ze souboru.</span><span class="sxs-lookup"><span data-stu-id="9455b-185">This PowerShell command replaces the definition of the existing input source called EntryStream with the definition from the file.</span></span>

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a><span data-ttu-id="9455b-186">Nové AzureStreamAnalyticsJob | Nové AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="9455b-186">New-AzureStreamAnalyticsJob | New-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="9455b-187">Vytvoří novou úlohu služby Stream Analytics v Microsoft Azure nebo aktualizuje definice existující zadanou úlohu.</span><span class="sxs-lookup"><span data-stu-id="9455b-187">Creates a new Stream Analytics job in Microsoft Azure, or updates the definition of an existing specified job.</span></span>

<span data-ttu-id="9455b-188">V souboru .json nebo na příkazovém řádku lze zadat název úlohy.</span><span class="sxs-lookup"><span data-stu-id="9455b-188">The name of the job can be specified in the .json file or on the command line.</span></span> <span data-ttu-id="9455b-189">Pokud jsou zadány oba název na příkazovém řádku musí být stejná jako ta, v souboru.</span><span class="sxs-lookup"><span data-stu-id="9455b-189">If both are specified, the name on the command line must be the same as the one in the file.</span></span>

<span data-ttu-id="9455b-190">Pokud můžete zadat název úlohy, která již existuje a není zadán parametr-Force, rutina zobrazí dotaz, zda chcete nahradit existující úlohy.</span><span class="sxs-lookup"><span data-stu-id="9455b-190">If you specify a job name that already exists and do not specify the –Force parameter, the cmdlet will ask whether or not to replace the existing job.</span></span>

<span data-ttu-id="9455b-191">Pokud zadáte – vynutit parametr a zadejte název existující úlohy, definici úlohy se nahradí bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9455b-191">If you specify the –Force parameter and specify an existing job name, the job definition will be replaced without confirmation.</span></span>

<span data-ttu-id="9455b-192">Podrobné informace o struktuře souboru JSON a obsah, najdete v části [vytvořit úlohy Stream Analytics] [ msdn-rest-api-create-stream-analytics-job] části [datového proudu Analytics Management REST API referenční dokumentace knihovny][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="9455b-192">For detailed information on the JSON file structure and contents, refer to the [Create Stream Analytics Job][msdn-rest-api-create-stream-analytics-job] section of the [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="9455b-193">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-193">**Example 1**</span></span>

<span data-ttu-id="9455b-194">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-194">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

<span data-ttu-id="9455b-195">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-195">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

<span data-ttu-id="9455b-196">Tento příkaz prostředí PowerShell vytvoří novou úlohu z definice v JobDefinition.json.</span><span class="sxs-lookup"><span data-stu-id="9455b-196">This PowerShell command creates a new job from the definition in JobDefinition.json.</span></span> <span data-ttu-id="9455b-197">Pokud stávající úloze s názvem zadaným v souboru definice úlohy je již definován, rutina zobrazí dotaz, zda chcete ho nahradit.</span><span class="sxs-lookup"><span data-stu-id="9455b-197">If an existing job with the name specified in the job definition file is already defined, the cmdlet will ask whether or not to replace it.</span></span>

<span data-ttu-id="9455b-198">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="9455b-198">**Example 2**</span></span>

<span data-ttu-id="9455b-199">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-199">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

<span data-ttu-id="9455b-200">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-200">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

<span data-ttu-id="9455b-201">Tento příkaz prostředí PowerShell nahrazuje danou definici úlohy pro StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-201">This PowerShell command replaces the job definition for StreamingJob.</span></span>

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a><span data-ttu-id="9455b-202">Nové AzureStreamAnalyticsOutput | Nové AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="9455b-202">New-AzureStreamAnalyticsOutput | New-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="9455b-203">Vytvoří nový výstupní v rámci úlohy Stream Analytics nebo aktualizuje existující výstup.</span><span class="sxs-lookup"><span data-stu-id="9455b-203">Creates a new output within a Stream Analytics job, or updates an existing output.</span></span>  

<span data-ttu-id="9455b-204">Název výstupu mohou být zadané v souboru .json nebo na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="9455b-204">The name of the output can be specified in the .json file or on the command line.</span></span> <span data-ttu-id="9455b-205">Pokud jsou zadány oba název na příkazovém řádku musí být stejná jako ta, v souboru.</span><span class="sxs-lookup"><span data-stu-id="9455b-205">If both are specified, the name on the command line must be the same as the one in the file.</span></span>

<span data-ttu-id="9455b-206">Pokud zadejte výstup, který již existuje a není zadán parametr-Force, rutina zobrazí dotaz, zda chcete nahradit existující výstup.</span><span class="sxs-lookup"><span data-stu-id="9455b-206">If you specify an output that already exists and do not specify the –Force parameter, the cmdlet will ask whether or not to replace the existing output.</span></span>

<span data-ttu-id="9455b-207">Pokud zadáte – vynutit parametr a zadejte název existující výstup, výstup se nahradí bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9455b-207">If you specify the –Force parameter and specify an existing output name, the output will be replaced without confirmation.</span></span>

<span data-ttu-id="9455b-208">Podrobné informace o struktuře souboru JSON a obsah, najdete v části [vytvoření výstupu (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] části [datového proudu Analytics Management REST API referenční dokumentace knihovny][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="9455b-208">For detailed information on the JSON file structure and contents, refer to the [Create Output (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-output] section of the [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="9455b-209">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-209">**Example 1**</span></span>

<span data-ttu-id="9455b-210">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-210">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

<span data-ttu-id="9455b-211">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-211">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

<span data-ttu-id="9455b-212">Tento příkaz prostředí PowerShell vytvoří nový výstupní názvem "výstupní" v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-212">This PowerShell command creates a new output called "output" in the job StreamingJob.</span></span> <span data-ttu-id="9455b-213">Pokud existující výstup s tímto názvem je již definován, rutina zobrazí dotaz, zda chcete ho nahradit.</span><span class="sxs-lookup"><span data-stu-id="9455b-213">If an existing output with this name is already defined, the cmdlet will ask whether or not to replace it.</span></span>

<span data-ttu-id="9455b-214">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="9455b-214">**Example 2**</span></span>

<span data-ttu-id="9455b-215">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-215">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

<span data-ttu-id="9455b-216">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-216">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

<span data-ttu-id="9455b-217">Tento příkaz prostředí PowerShell nahradí definici "výstupní" v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-217">This PowerShell command replaces the definition for "output" in the job StreamingJob.</span></span>

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a><span data-ttu-id="9455b-218">Nové AzureStreamAnalyticsTransformation | Nové AzureRMStreamAnalyticsTransformation</span><span class="sxs-lookup"><span data-stu-id="9455b-218">New-AzureStreamAnalyticsTransformation | New-AzureRMStreamAnalyticsTransformation</span></span>
<span data-ttu-id="9455b-219">Vytvoří nový transformaci v rámci úlohy Stream Analytics nebo aktualizuje existující transformace.</span><span class="sxs-lookup"><span data-stu-id="9455b-219">Creates a new transformation within a Stream Analytics job, or updates the existing transformation.</span></span>

<span data-ttu-id="9455b-220">V souboru .json nebo na příkazovém řádku, může být zadán název transformace.</span><span class="sxs-lookup"><span data-stu-id="9455b-220">The name of the transformation can be specified in the .json file or on the command line.</span></span> <span data-ttu-id="9455b-221">Pokud jsou zadány oba název na příkazovém řádku musí být stejná jako ta, v souboru.</span><span class="sxs-lookup"><span data-stu-id="9455b-221">If both are specified, the name on the command line must be the same as the one in the file.</span></span>

<span data-ttu-id="9455b-222">Pokud zadejte transformaci, která již existuje a není zadán parametr-Force, rutina zobrazí dotaz, zda chcete nahradit existující transformace.</span><span class="sxs-lookup"><span data-stu-id="9455b-222">If you specify a transformation that already exists and do not specify the –Force parameter, the cmdlet will ask whether or not to replace the existing transformation.</span></span>

<span data-ttu-id="9455b-223">Pokud zadáte – vynutit parametr a zadejte název existující transformace, transformaci se nahradí bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9455b-223">If you specify the –Force parameter and specify an existing transformation name, the transformation will be replaced without confirmation.</span></span>

<span data-ttu-id="9455b-224">Podrobné informace o struktuře souboru JSON a obsah, najdete v části [vytvořit transformaci (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] části [datového proudu Analytics Management REST API referenční dokumentace knihovny][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="9455b-224">For detailed information on the JSON file structure and contents, refer to the [Create Transformation (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-transformation] section of the [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="9455b-225">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-225">**Example 1**</span></span>

<span data-ttu-id="9455b-226">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-226">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

<span data-ttu-id="9455b-227">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-227">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

<span data-ttu-id="9455b-228">Tento příkaz prostředí PowerShell vytvoří nový transformace názvem StreamingJobTransform v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-228">This PowerShell command creates a new transformation called StreamingJobTransform in the job StreamingJob.</span></span> <span data-ttu-id="9455b-229">Pokud s tímto názvem je již definován existující transformace, rutina zobrazí dotaz, zda chcete ho nahradit.</span><span class="sxs-lookup"><span data-stu-id="9455b-229">If an existing transformation is already defined with this name, the cmdlet will ask whether or not to replace it.</span></span>

<span data-ttu-id="9455b-230">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="9455b-230">**Example 2**</span></span>

<span data-ttu-id="9455b-231">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-231">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

<span data-ttu-id="9455b-232">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-232">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 <span data-ttu-id="9455b-233">Tento příkaz prostředí PowerShell nahrazuje definice StreamingJobTransform v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-233">This PowerShell command replaces the definition of StreamingJobTransform in the job StreamingJob.</span></span>

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a><span data-ttu-id="9455b-234">Odebrat AzureStreamAnalyticsInput | Odebrat AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="9455b-234">Remove-AzureStreamAnalyticsInput | Remove-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="9455b-235">Asynchronně odstraní specifický vstup z úlohy Stream Analytics v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9455b-235">Asynchronously deletes a specific input from a Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="9455b-236">Zadáte-li parametr-Force, odstraní se vstup bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9455b-236">If you specify the –Force parameter, the input will be deleted without confirmation.</span></span>

<span data-ttu-id="9455b-237">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-237">**Example 1**</span></span>

<span data-ttu-id="9455b-238">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-238">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

<span data-ttu-id="9455b-239">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-239">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

<span data-ttu-id="9455b-240">Tento příkaz prostředí PowerShell odebere vstupní EventStream v úloze StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-240">This PowerShell command removes the input EventStream in the job StreamingJob.</span></span>  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a><span data-ttu-id="9455b-241">Odebrat AzureStreamAnalyticsJob | Odebrat AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="9455b-241">Remove-AzureStreamAnalyticsJob | Remove-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="9455b-242">Asynchronně odstraní určité úlohy Stream Analytics v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9455b-242">Asynchronously deletes a specific Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="9455b-243">Zadáte-li parametr-Force, úloha se odstraní bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9455b-243">If you specify the –Force parameter, the job will be deleted without confirmation.</span></span>

<span data-ttu-id="9455b-244">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-244">**Example 1**</span></span>

<span data-ttu-id="9455b-245">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-245">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="9455b-246">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-246">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="9455b-247">Tento příkaz prostředí PowerShell odebere úlohy StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-247">This PowerShell command removes the job StreamingJob.</span></span>  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a><span data-ttu-id="9455b-248">Odebrat AzureStreamAnalyticsOutput | Odebrat AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="9455b-248">Remove-AzureStreamAnalyticsOutput | Remove-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="9455b-249">Asynchronně odstraní konkrétní výstup z úlohy Stream Analytics v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9455b-249">Asynchronously deletes a specific output from a Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="9455b-250">Zadáte-li parametr-Force, odstraní se výstup bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="9455b-250">If you specify the –Force parameter, the output will be deleted without confirmation.</span></span>

<span data-ttu-id="9455b-251">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-251">**Example 1**</span></span>

<span data-ttu-id="9455b-252">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-252">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="9455b-253">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-253">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="9455b-254">Tato odebere příkaz prostředí PowerShell výstup výstup do úlohy StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-254">This PowerShell command removes the output Output in the job StreamingJob.</span></span>  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a><span data-ttu-id="9455b-255">Spuštění AzureStreamAnalyticsJob | Počáteční AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="9455b-255">Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="9455b-256">Asynchronně nasadí a spustí úlohu služby Stream Analytics v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9455b-256">Asynchronously deploys and starts a Stream Analytics job in Microsoft Azure.</span></span>

<span data-ttu-id="9455b-257">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-257">**Example 1**</span></span>

<span data-ttu-id="9455b-258">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-258">Azure PowerShell 0.9.8:</span></span>  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

<span data-ttu-id="9455b-259">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-259">Azure PowerShell 1.0:</span></span>  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

<span data-ttu-id="9455b-260">Tento příkaz prostředí PowerShell spustí úlohu StreamingJob s časem zahájení vlastní výstup nastavená na 12 prosinec 2012 12:12:12 UTC.</span><span class="sxs-lookup"><span data-stu-id="9455b-260">This PowerShell command starts the job StreamingJob with a custom output start time set to December 12, 2012, 12:12:12 UTC.</span></span>

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a><span data-ttu-id="9455b-261">Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="9455b-261">Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="9455b-262">Asynchronně Ukončí úlohu služby Stream Analytics z běžící v Microsoft Azure a zrušte přiděluje prostředky, které byly, který se používá.</span><span class="sxs-lookup"><span data-stu-id="9455b-262">Asynchronously stops a Stream Analytics job from running in Microsoft Azure and de-allocates resources that were that were being used.</span></span> <span data-ttu-id="9455b-263">Definice úlohy a metadata bude stále k dispozici v rámci vašeho předplatného prostřednictvím portálu Azure a rozhraní API pro správu tak, aby úlohy můžete upravit a restartovat.</span><span class="sxs-lookup"><span data-stu-id="9455b-263">The job definition and metadata will remain available within your subscription through both the Azure portal and management APIs, such that the job can be edited and restarted.</span></span> <span data-ttu-id="9455b-264">Vám nebude nic účtováno pro úlohu v zastaveném stavu.</span><span class="sxs-lookup"><span data-stu-id="9455b-264">You will not be charged for a job in the stopped state.</span></span>

<span data-ttu-id="9455b-265">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-265">**Example 1**</span></span>

<span data-ttu-id="9455b-266">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-266">Azure PowerShell 0.9.8:</span></span>  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="9455b-267">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-267">Azure PowerShell 1.0:</span></span>  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="9455b-268">Tento příkaz prostředí PowerShell zastaví úlohu StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-268">This PowerShell command stops the job StreamingJob.</span></span>  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a><span data-ttu-id="9455b-269">Test AzureStreamAnalyticsInput | Test AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="9455b-269">Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="9455b-270">Testuje schopnost Stream Analytics se připojit k zadaný vstup.</span><span class="sxs-lookup"><span data-stu-id="9455b-270">Tests the ability of Stream Analytics to connect to a specified input.</span></span>

<span data-ttu-id="9455b-271">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-271">**Example 1**</span></span>

<span data-ttu-id="9455b-272">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-272">Azure PowerShell 0.9.8:</span></span>  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

<span data-ttu-id="9455b-273">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-273">Azure PowerShell 1.0:</span></span>  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

<span data-ttu-id="9455b-274">Tento příkaz prostředí PowerShell testů připojení stav vstupní EntryStream v StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-274">This PowerShell command tests the connection status of the input EntryStream in StreamingJob.</span></span>  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a><span data-ttu-id="9455b-275">Test AzureStreamAnalyticsOutput | Test AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="9455b-275">Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="9455b-276">Testuje schopnost Stream Analytics se připojit k zadaným výstupem.</span><span class="sxs-lookup"><span data-stu-id="9455b-276">Tests the ability of Stream Analytics to connect to a specified output.</span></span>

<span data-ttu-id="9455b-277">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="9455b-277">**Example 1**</span></span>

<span data-ttu-id="9455b-278">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="9455b-278">Azure PowerShell 0.9.8:</span></span>  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="9455b-279">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="9455b-279">Azure PowerShell 1.0:</span></span>  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="9455b-280">Tato testy příkaz prostředí PowerShell stav připojení výstupu výstup v StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="9455b-280">This PowerShell command tests the connection status of the output Output in StreamingJob.</span></span>  

## <a name="get-support"></a><span data-ttu-id="9455b-281">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="9455b-281">Get support</span></span>
<span data-ttu-id="9455b-282">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="9455b-282">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9455b-283">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9455b-283">Next steps</span></span>
* [<span data-ttu-id="9455b-284">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9455b-284">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="9455b-285">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9455b-285">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="9455b-286">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9455b-286">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="9455b-287">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="9455b-287">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="9455b-288">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9455b-288">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

