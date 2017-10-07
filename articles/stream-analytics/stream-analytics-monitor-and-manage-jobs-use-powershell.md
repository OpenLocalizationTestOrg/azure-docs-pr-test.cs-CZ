---
title: "aaaMonitor a spravovat úlohy Stream Analytics v prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak Azure PowerShell a rutiny toomonitor toouse a spravovat úlohy Stream Analytics."
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
ms.openlocfilehash: 44abc82f1c44a5ebc1701badd6547b84dac239b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a><span data-ttu-id="3cdc9-104">Sledovat a spravovat úlohy Stream Analytics pomocí rutin prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3cdc9-104">Monitor and manage Stream Analytics jobs with Azure PowerShell cmdlets</span></span>
<span data-ttu-id="3cdc9-105">Zjistěte, jak toomonitor a spravovat prostředky Stream Analytics s rutin prostředí Azure PowerShell a skriptů prostředí powershell, které jsou spouštěny základní úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-105">Learn how toomonitor and manage Stream Analytics resources with Azure PowerShell cmdlets and powershell scripting that execute basic Stream Analytics tasks.</span></span>

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a><span data-ttu-id="3cdc9-106">Požadavky pro spuštění rutiny prostředí Azure PowerShell pro Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3cdc9-106">Prerequisites for running Azure PowerShell cmdlets for Stream Analytics</span></span>
* <span data-ttu-id="3cdc9-107">Vytvořte skupinu prostředků Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-107">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="3cdc9-108">Následující Hello je ukázkový skript prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-108">hello following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="3cdc9-109">Prostředí Azure PowerShell informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="3cdc9-109">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

<span data-ttu-id="3cdc9-110">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-110">Azure PowerShell 0.9.8:</span></span>  

         # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

<span data-ttu-id="3cdc9-111">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-111">Azure PowerShell 1.0:</span></span>  

         # Log in tooyour Azure account
        Login-AzureRmAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> <span data-ttu-id="3cdc9-112">Úlohy Stream Analytics vytvořené prostřednictvím kódu programu nemají povoleno ve výchozím nastavení monitorování.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-112">Stream Analytics jobs created programmatically do not have monitoring enabled by default.</span></span>  <span data-ttu-id="3cdc9-113">Můžete ručně povolit monitorování v hello portálu Azure tak, že přejdete na stránku toohello úlohy monitorování a kliknutím na tlačítko Povolit hello nebo můžete to provést prostřednictvím kódu programu podle následujících kroků hello nacházející se v [Azure Stream Analytics – monitorování datového proudu Analýza úlohy programově](stream-analytics-monitor-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="3cdc9-113">You can manually enable monitoring in hello Azure Portal by navigating toohello job’s Monitor page and clicking hello Enable button or you can do this programmatically by following hello steps located at [Azure Stream Analytics - Monitor Stream Analytics Jobs Programatically](stream-analytics-monitor-jobs.md).</span></span>
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a><span data-ttu-id="3cdc9-114">Rutiny Azure PowerShell pro Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3cdc9-114">Azure PowerShell cmdlets for Stream Analytics</span></span>
<span data-ttu-id="3cdc9-115">Hello následující rutiny prostředí Azure PowerShell můžou být použité toomonitor a spravovat úlohy Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-115">hello following Azure PowerShell cmdlets can be used toomonitor and manage Azure Stream Analytics jobs.</span></span> <span data-ttu-id="3cdc9-116">Všimněte si, že prostředí Azure PowerShell má různé verze.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-116">Note that Azure PowerShell has different versions.</span></span> 
<span data-ttu-id="3cdc9-117">**V příkladech hello, ke kterému se první příkaz uvedené hello prostředí Azure PowerShell 0.9.8 druhý příkaz hello je pro Azure PowerShell 1.0.**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-117">**In hello examples listed hello first command is for Azure PowerShell 0.9.8, hello second command is for Azure PowerShell 1.0.**</span></span> <span data-ttu-id="3cdc9-118">příkazy Hello Azure PowerShell 1.0 bude mít vždy "AzureRM" v příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-118">hello Azure PowerShell 1.0 commands will always have "AzureRM" in hello command.</span></span>

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a><span data-ttu-id="3cdc9-119">Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="3cdc9-119">Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="3cdc9-120">Zobrazí všechny úlohy služby Stream Analytics, které jsou definované v hello předplatného Azure nebo zadaná skupina prostředků, nebo získá úlohy informace o konkrétní úloze ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-120">Lists all Stream Analytics jobs defined in hello Azure subscription or specified resource group, or gets job information about a specific job within a resource group.</span></span>

<span data-ttu-id="3cdc9-121">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-121">**Example 1**</span></span>

<span data-ttu-id="3cdc9-122">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-122">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob

<span data-ttu-id="3cdc9-123">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-123">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob

<span data-ttu-id="3cdc9-124">Tento příkaz prostředí PowerShell vrátí informace o všechny úlohy služby Stream Analytics hello hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-124">This PowerShell command returns information about all hello Stream Analytics jobs in hello Azure subscription.</span></span>

<span data-ttu-id="3cdc9-125">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-125">**Example 2**</span></span>

<span data-ttu-id="3cdc9-126">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-126">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

<span data-ttu-id="3cdc9-127">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-127">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

<span data-ttu-id="3cdc9-128">Tento příkaz prostředí PowerShell vrátí informace o všechny úlohy služby Stream Analytics hello ve skupině prostředků hello StreamAnalytics výchozí střed USA.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-128">This PowerShell command returns information about all hello Stream Analytics jobs in hello resource group StreamAnalytics-Default-Central-US.</span></span>

<span data-ttu-id="3cdc9-129">**Příklad 3**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-129">**Example 3**</span></span>

<span data-ttu-id="3cdc9-130">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-130">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

<span data-ttu-id="3cdc9-131">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-131">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

<span data-ttu-id="3cdc9-132">Tento příkaz prostředí PowerShell vrátí informace o úloze Stream Analytics hello StreamingJob ve skupině prostředků hello StreamAnalytics výchozí střed USA.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-132">This PowerShell command returns information about hello Stream Analytics job StreamingJob in hello resource group StreamAnalytics-Default-Central-US.</span></span>

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a><span data-ttu-id="3cdc9-133">Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="3cdc9-133">Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="3cdc9-134">Zobrazí seznam všech hello vstupních hodnot, které jsou definované v zadané úloze Stream Analytics, nebo získá informace o specifický vstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-134">Lists all of hello inputs that are defined in a specified Stream Analytics job, or gets information about a specific input.</span></span>

<span data-ttu-id="3cdc9-135">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-135">**Example 1**</span></span>

<span data-ttu-id="3cdc9-136">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-136">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="3cdc9-137">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-137">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="3cdc9-138">Tento příkaz prostředí PowerShell vrátí informace o všech vstupů hello definované v úloze hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-138">This PowerShell command returns information about all hello inputs defined in hello job StreamingJob.</span></span>

<span data-ttu-id="3cdc9-139">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-139">**Example 2**</span></span>

<span data-ttu-id="3cdc9-140">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-140">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

<span data-ttu-id="3cdc9-141">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-141">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

<span data-ttu-id="3cdc9-142">Tento příkaz prostředí PowerShell vrátí informace o hello vstup s názvem EntryStream definované v úloze hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-142">This PowerShell command returns information about hello input named EntryStream defined in hello job StreamingJob.</span></span>

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a><span data-ttu-id="3cdc9-143">Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="3cdc9-143">Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="3cdc9-144">Zobrazí seznam všech hello výstupů, které jsou definované v zadané úloze Stream Analytics, nebo získá informace o konkrétní výstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-144">Lists all of hello outputs that are defined in a specified Stream Analytics job, or gets information about a specific output.</span></span>

<span data-ttu-id="3cdc9-145">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-145">**Example 1**</span></span>

<span data-ttu-id="3cdc9-146">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-146">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="3cdc9-147">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-147">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="3cdc9-148">Tento příkaz prostředí PowerShell vrátí informace o definované v úloze hello StreamingJob výstupy hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-148">This PowerShell command returns information about hello outputs defined in hello job StreamingJob.</span></span>

<span data-ttu-id="3cdc9-149">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-149">**Example 2**</span></span>

<span data-ttu-id="3cdc9-150">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-150">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

<span data-ttu-id="3cdc9-151">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-151">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

<span data-ttu-id="3cdc9-152">Tento příkaz prostředí PowerShell vrátí informace o výstup hello s názvem definované v úloze hello StreamingJob výstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-152">This PowerShell command returns information about hello output named Output defined in hello job StreamingJob.</span></span>

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a><span data-ttu-id="3cdc9-153">Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota</span><span class="sxs-lookup"><span data-stu-id="3cdc9-153">Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota</span></span>
<span data-ttu-id="3cdc9-154">Získá informace o hello kvótu počtu jednotek v zadané oblasti streamování.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-154">Gets information about hello quota of streaming units in a specified region.</span></span>

<span data-ttu-id="3cdc9-155">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-155">**Example 1**</span></span>

<span data-ttu-id="3cdc9-156">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-156">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

<span data-ttu-id="3cdc9-157">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-157">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

<span data-ttu-id="3cdc9-158">Tento příkaz prostředí PowerShell vrátí informace o hello kvóta a využití jednotek streamování v oblasti hello střed USA.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-158">This PowerShell command returns information about hello quota and usage of streaming units in hello Central US region.</span></span>

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a><span data-ttu-id="3cdc9-159">Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation</span><span class="sxs-lookup"><span data-stu-id="3cdc9-159">Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation</span></span>
<span data-ttu-id="3cdc9-160">Získá informace o konkrétní transformaci definované v úloze Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-160">Gets information about a specific transformation defined in a Stream Analytics job.</span></span>

<span data-ttu-id="3cdc9-161">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-161">**Example 1**</span></span>

<span data-ttu-id="3cdc9-162">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-162">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

<span data-ttu-id="3cdc9-163">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-163">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

<span data-ttu-id="3cdc9-164">Tento příkaz prostředí PowerShell vrátí informace o transformaci hello názvem StreamingJob v úloze hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-164">This PowerShell command returns information about hello transformation called StreamingJob in hello job StreamingJob.</span></span>

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a><span data-ttu-id="3cdc9-165">Nové AzureStreamAnalyticsInput | Nové AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="3cdc9-165">New-AzureStreamAnalyticsInput | New-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="3cdc9-166">Vytvoří nový vstup v rámci úlohy Stream Analytics nebo aktualizuje existující zadaný vstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-166">Creates a new input within a Stream Analytics job, or updates an existing specified input.</span></span>

<span data-ttu-id="3cdc9-167">Hello hello vstupu může být zadán název v souboru .json hello nebo na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-167">hello name of hello input can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="3cdc9-168">Pokud jsou zadány oba název hello na příkazovém řádku hello musí být hello stejné jako hello, jeden v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-168">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="3cdc9-169">Pokud zadáte vstup, který již existuje a není zadán hello – Force parametr rutiny hello zobrazí dotaz, zda tooreplace hello stávající vstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-169">If you specify an input that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing input.</span></span>

<span data-ttu-id="3cdc9-170">Pokud zadáte hello – Force parametr a zadejte existující zadejte název, vstup hello se nahradí bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-170">If you specify hello –Force parameter and specify an existing input name, hello input will be replaced without confirmation.</span></span>

<span data-ttu-id="3cdc9-171">Podrobné informace o struktuře souboru JSON hello a obsah, najdete v části toohello [vytvořit vstup (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] části hello [rozhraní API REST správy Stream Analytics Referenční dokumentace knihovny][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="3cdc9-171">For detailed information on hello JSON file structure and contents, refer toohello [Create Input (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-input] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="3cdc9-172">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-172">**Example 1**</span></span>

<span data-ttu-id="3cdc9-173">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-173">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

<span data-ttu-id="3cdc9-174">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-174">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

<span data-ttu-id="3cdc9-175">Tento příkaz prostředí PowerShell vytvoří nový vstup z hello souboru Input.json.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-175">This PowerShell command creates a new input from hello file Input.json.</span></span> <span data-ttu-id="3cdc9-176">Pokud je již definován stávající vstup s názvem hello zadané v souboru definice vstupní hello, hello rutina požádá, jestli tooreplace ho.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-176">If an existing input with hello name specified in hello input definition file is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="3cdc9-177">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-177">**Example 2**</span></span>

<span data-ttu-id="3cdc9-178">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-178">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

<span data-ttu-id="3cdc9-179">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-179">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

<span data-ttu-id="3cdc9-180">Tento příkaz prostředí PowerShell vytvoří nový vstup v úloze hello názvem EntryStream.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-180">This PowerShell command creates a new input in hello job called EntryStream.</span></span> <span data-ttu-id="3cdc9-181">Pokud je již definován stávající vstup s tímto názvem, rutina hello požádá, jestli tooreplace ho.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-181">If an existing input with this name is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="3cdc9-182">**Příklad 3**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-182">**Example 3**</span></span>

<span data-ttu-id="3cdc9-183">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-183">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

<span data-ttu-id="3cdc9-184">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-184">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

<span data-ttu-id="3cdc9-185">Tento příkaz prostředí PowerShell nahrazuje hello Definice hello existující vstupní zdroj volána EntryStream s hello definice ze souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-185">This PowerShell command replaces hello definition of hello existing input source called EntryStream with hello definition from hello file.</span></span>

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a><span data-ttu-id="3cdc9-186">Nové AzureStreamAnalyticsJob | Nové AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="3cdc9-186">New-AzureStreamAnalyticsJob | New-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="3cdc9-187">Vytvoří novou úlohu služby Stream Analytics v Microsoft Azure nebo aktualizuje definice hello existující zadanou úlohu.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-187">Creates a new Stream Analytics job in Microsoft Azure, or updates hello definition of an existing specified job.</span></span>

<span data-ttu-id="3cdc9-188">v souboru .json hello nebo na příkazovém řádku hello může být zadán název Hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-188">hello name of hello job can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="3cdc9-189">Pokud jsou zadány oba název hello na příkazovém řádku hello musí být hello stejné jako hello, jeden v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-189">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="3cdc9-190">Pokud zadáte název úlohy, která již existuje a nezadávejte hello – Force parametr hello rutiny zobrazí dotaz, zda tooreplace hello existující úlohy.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-190">If you specify a job name that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing job.</span></span>

<span data-ttu-id="3cdc9-191">Pokud zadáte hello – Force parametr a zadejte název existující úlohy, definice úlohy hello se nahradí bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-191">If you specify hello –Force parameter and specify an existing job name, hello job definition will be replaced without confirmation.</span></span>

<span data-ttu-id="3cdc9-192">Podrobné informace o struktuře souboru JSON hello a obsah, najdete v části toohello [vytvořit úlohy Stream Analytics] [ msdn-rest-api-create-stream-analytics-job] části hello [referenční příručka Stream Analytics Management REST API Knihovna][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="3cdc9-192">For detailed information on hello JSON file structure and contents, refer toohello [Create Stream Analytics Job][msdn-rest-api-create-stream-analytics-job] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="3cdc9-193">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-193">**Example 1**</span></span>

<span data-ttu-id="3cdc9-194">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-194">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

<span data-ttu-id="3cdc9-195">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-195">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

<span data-ttu-id="3cdc9-196">Tento příkaz prostředí PowerShell vytvoří novou úlohu z definice hello v JobDefinition.json.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-196">This PowerShell command creates a new job from hello definition in JobDefinition.json.</span></span> <span data-ttu-id="3cdc9-197">Pokud je již definován existující úlohy s názvem hello zadaný v souboru definice úlohy hello, hello rutina požádá, jestli tooreplace ho.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-197">If an existing job with hello name specified in hello job definition file is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="3cdc9-198">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-198">**Example 2**</span></span>

<span data-ttu-id="3cdc9-199">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-199">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

<span data-ttu-id="3cdc9-200">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-200">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

<span data-ttu-id="3cdc9-201">Tento příkaz prostředí PowerShell nahrazuje hello definice úlohy pro StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-201">This PowerShell command replaces hello job definition for StreamingJob.</span></span>

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a><span data-ttu-id="3cdc9-202">Nové AzureStreamAnalyticsOutput | Nové AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="3cdc9-202">New-AzureStreamAnalyticsOutput | New-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="3cdc9-203">Vytvoří nový výstupní v rámci úlohy Stream Analytics nebo aktualizuje existující výstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-203">Creates a new output within a Stream Analytics job, or updates an existing output.</span></span>  

<span data-ttu-id="3cdc9-204">v souboru .json hello nebo na příkazovém řádku hello může být zadán název Hello hello výstupu.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-204">hello name of hello output can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="3cdc9-205">Pokud jsou zadány oba název hello na příkazovém řádku hello musí být hello stejné jako hello, jeden v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-205">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="3cdc9-206">Zadáte-li výstupu, který již existuje a není zadán hello – Force parametr hello rutiny zobrazí dotaz, zda tooreplace hello existující výstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-206">If you specify an output that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing output.</span></span>

<span data-ttu-id="3cdc9-207">Pokud zadáte hello – Force parametr a zadejte název existující výstup, výstup hello se nahradí bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-207">If you specify hello –Force parameter and specify an existing output name, hello output will be replaced without confirmation.</span></span>

<span data-ttu-id="3cdc9-208">Podrobné informace o struktuře souboru JSON hello a obsah, najdete v části toohello [vytvoření výstupu (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] části hello [rozhraní API REST správy Stream Analytics Referenční dokumentace knihovny][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="3cdc9-208">For detailed information on hello JSON file structure and contents, refer toohello [Create Output (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-output] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="3cdc9-209">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-209">**Example 1**</span></span>

<span data-ttu-id="3cdc9-210">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-210">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

<span data-ttu-id="3cdc9-211">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-211">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

<span data-ttu-id="3cdc9-212">Tento příkaz prostředí PowerShell vytvoří nový výstupní názvem "výstupní" v úloze hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-212">This PowerShell command creates a new output called "output" in hello job StreamingJob.</span></span> <span data-ttu-id="3cdc9-213">Pokud je již definován existující výstup s tímto názvem, rutina hello požádá, jestli tooreplace ho.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-213">If an existing output with this name is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="3cdc9-214">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-214">**Example 2**</span></span>

<span data-ttu-id="3cdc9-215">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-215">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

<span data-ttu-id="3cdc9-216">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-216">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

<span data-ttu-id="3cdc9-217">Tento příkaz prostředí PowerShell nahrazuje definice hello "výstupní" v úloze hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-217">This PowerShell command replaces hello definition for "output" in hello job StreamingJob.</span></span>

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a><span data-ttu-id="3cdc9-218">Nové AzureStreamAnalyticsTransformation | Nové AzureRMStreamAnalyticsTransformation</span><span class="sxs-lookup"><span data-stu-id="3cdc9-218">New-AzureStreamAnalyticsTransformation | New-AzureRMStreamAnalyticsTransformation</span></span>
<span data-ttu-id="3cdc9-219">Vytvoří nový transformaci v rámci úlohy Stream Analytics nebo aktualizuje existující transformace hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-219">Creates a new transformation within a Stream Analytics job, or updates hello existing transformation.</span></span>

<span data-ttu-id="3cdc9-220">v souboru .json hello nebo na příkazovém řádku hello může být zadán název Hello hello transformace.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-220">hello name of hello transformation can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="3cdc9-221">Pokud jsou zadány oba název hello na příkazovém řádku hello musí být hello stejné jako hello, jeden v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-221">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="3cdc9-222">Pokud zadáte transformaci, která již existuje a není zadán hello – Force parametr hello rutiny zobrazí dotaz, zda tooreplace hello existující transformace.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-222">If you specify a transformation that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing transformation.</span></span>

<span data-ttu-id="3cdc9-223">Pokud zadáte hello – Force parametr a zadejte název existující transformace, hello transformace se nahradí bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-223">If you specify hello –Force parameter and specify an existing transformation name, hello transformation will be replaced without confirmation.</span></span>

<span data-ttu-id="3cdc9-224">Podrobné informace o struktuře souboru JSON hello a obsah, najdete v části toohello [vytvořit transformaci (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] části hello [Stream Analytics správy Knihovna referenční dokumentace rozhraní API REST][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="3cdc9-224">For detailed information on hello JSON file structure and contents, refer toohello [Create Transformation (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-transformation] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="3cdc9-225">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-225">**Example 1**</span></span>

<span data-ttu-id="3cdc9-226">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-226">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

<span data-ttu-id="3cdc9-227">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-227">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

<span data-ttu-id="3cdc9-228">Tento příkaz prostředí PowerShell vytvoří nový transformace názvem StreamingJobTransform v úloze hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-228">This PowerShell command creates a new transformation called StreamingJobTransform in hello job StreamingJob.</span></span> <span data-ttu-id="3cdc9-229">Pokud je již definován transformaci existující s tímto názvem, rutina hello požádá, jestli tooreplace ho.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-229">If an existing transformation is already defined with this name, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="3cdc9-230">**Příklad 2**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-230">**Example 2**</span></span>

<span data-ttu-id="3cdc9-231">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-231">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

<span data-ttu-id="3cdc9-232">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-232">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 <span data-ttu-id="3cdc9-233">Tento příkaz prostředí PowerShell nahrazuje hello Definice StreamingJobTransform v úloze hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-233">This PowerShell command replaces hello definition of StreamingJobTransform in hello job StreamingJob.</span></span>

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a><span data-ttu-id="3cdc9-234">Odebrat AzureStreamAnalyticsInput | Odebrat AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="3cdc9-234">Remove-AzureStreamAnalyticsInput | Remove-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="3cdc9-235">Asynchronně odstraní specifický vstup z úlohy Stream Analytics v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-235">Asynchronously deletes a specific input from a Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="3cdc9-236">Pokud zadáte hello – Force parametr hello vstup bude odstraněn bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-236">If you specify hello –Force parameter, hello input will be deleted without confirmation.</span></span>

<span data-ttu-id="3cdc9-237">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-237">**Example 1**</span></span>

<span data-ttu-id="3cdc9-238">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-238">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

<span data-ttu-id="3cdc9-239">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-239">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

<span data-ttu-id="3cdc9-240">Tento příkaz prostředí PowerShell, že odebere hello vstupní EventStream v úloze hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-240">This PowerShell command removes hello input EventStream in hello job StreamingJob.</span></span>  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a><span data-ttu-id="3cdc9-241">Odebrat AzureStreamAnalyticsJob | Odebrat AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="3cdc9-241">Remove-AzureStreamAnalyticsJob | Remove-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="3cdc9-242">Asynchronně odstraní určité úlohy Stream Analytics v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-242">Asynchronously deletes a specific Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="3cdc9-243">Pokud zadáte hello – Force parametr hello úlohy bude odstraněn bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-243">If you specify hello –Force parameter, hello job will be deleted without confirmation.</span></span>

<span data-ttu-id="3cdc9-244">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-244">**Example 1**</span></span>

<span data-ttu-id="3cdc9-245">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-245">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="3cdc9-246">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-246">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="3cdc9-247">Tento příkaz prostředí PowerShell odebere úlohy hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-247">This PowerShell command removes hello job StreamingJob.</span></span>  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a><span data-ttu-id="3cdc9-248">Odebrat AzureStreamAnalyticsOutput | Odebrat AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="3cdc9-248">Remove-AzureStreamAnalyticsOutput | Remove-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="3cdc9-249">Asynchronně odstraní konkrétní výstup z úlohy Stream Analytics v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-249">Asynchronously deletes a specific output from a Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="3cdc9-250">Pokud zadáte hello – Force parametr výstup hello bude odstraněn bez potvrzení.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-250">If you specify hello –Force parameter, hello output will be deleted without confirmation.</span></span>

<span data-ttu-id="3cdc9-251">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-251">**Example 1**</span></span>

<span data-ttu-id="3cdc9-252">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-252">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="3cdc9-253">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-253">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="3cdc9-254">Toto prostředí PowerShell příkaz odebere hello výstup výstup do úlohy hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-254">This PowerShell command removes hello output Output in hello job StreamingJob.</span></span>  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a><span data-ttu-id="3cdc9-255">Spuštění AzureStreamAnalyticsJob | Počáteční AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="3cdc9-255">Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="3cdc9-256">Asynchronně nasadí a spustí úlohu služby Stream Analytics v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-256">Asynchronously deploys and starts a Stream Analytics job in Microsoft Azure.</span></span>

<span data-ttu-id="3cdc9-257">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-257">**Example 1**</span></span>

<span data-ttu-id="3cdc9-258">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-258">Azure PowerShell 0.9.8:</span></span>  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

<span data-ttu-id="3cdc9-259">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-259">Azure PowerShell 1.0:</span></span>  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

<span data-ttu-id="3cdc9-260">Tento příkaz prostředí PowerShell spustí hello úlohy StreamingJob s časem zahájení vlastní výstup nastavit tooDecember 12, 2012, 12:12:12 UTC.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-260">This PowerShell command starts hello job StreamingJob with a custom output start time set tooDecember 12, 2012, 12:12:12 UTC.</span></span>

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a><span data-ttu-id="3cdc9-261">Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="3cdc9-261">Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="3cdc9-262">Asynchronně Ukončí úlohu služby Stream Analytics z běžící v Microsoft Azure a zrušte přiděluje prostředky, které byly, který se používá.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-262">Asynchronously stops a Stream Analytics job from running in Microsoft Azure and de-allocates resources that were that were being used.</span></span> <span data-ttu-id="3cdc9-263">Hello definice úlohy a metadata bude stále k dispozici v rámci vašeho předplatného prostřednictvím hello portál Azure a rozhraní API pro správu tak, aby hello úlohy lze upravit a restartovat.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-263">hello job definition and metadata will remain available within your subscription through both hello Azure portal and management APIs, such that hello job can be edited and restarted.</span></span> <span data-ttu-id="3cdc9-264">Vám nebude nic účtováno pro úlohu ve stavu hello byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-264">You will not be charged for a job in hello stopped state.</span></span>

<span data-ttu-id="3cdc9-265">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-265">**Example 1**</span></span>

<span data-ttu-id="3cdc9-266">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-266">Azure PowerShell 0.9.8:</span></span>  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="3cdc9-267">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-267">Azure PowerShell 1.0:</span></span>  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="3cdc9-268">Tento příkaz prostředí PowerShell zastaví úlohu hello StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-268">This PowerShell command stops hello job StreamingJob.</span></span>  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a><span data-ttu-id="3cdc9-269">Test AzureStreamAnalyticsInput | Test AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="3cdc9-269">Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="3cdc9-270">Schopnost hello testy tooa tooconnect Stream Analytics zadán vstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-270">Tests hello ability of Stream Analytics tooconnect tooa specified input.</span></span>

<span data-ttu-id="3cdc9-271">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-271">**Example 1**</span></span>

<span data-ttu-id="3cdc9-272">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-272">Azure PowerShell 0.9.8:</span></span>  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

<span data-ttu-id="3cdc9-273">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-273">Azure PowerShell 1.0:</span></span>  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

<span data-ttu-id="3cdc9-274">Toto prostředí PowerShell příkaz testy hello stav připojení hello vstupní EntryStream v StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-274">This PowerShell command tests hello connection status of hello input EntryStream in StreamingJob.</span></span>  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a><span data-ttu-id="3cdc9-275">Test AzureStreamAnalyticsOutput | Test AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="3cdc9-275">Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="3cdc9-276">Schopnost hello testy tooa tooconnect Stream Analytics zadán výstup.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-276">Tests hello ability of Stream Analytics tooconnect tooa specified output.</span></span>

<span data-ttu-id="3cdc9-277">**Příklad 1**</span><span class="sxs-lookup"><span data-stu-id="3cdc9-277">**Example 1**</span></span>

<span data-ttu-id="3cdc9-278">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-278">Azure PowerShell 0.9.8:</span></span>  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="3cdc9-279">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="3cdc9-279">Azure PowerShell 1.0:</span></span>  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="3cdc9-280">Toto prostředí PowerShell příkaz testy hello stav připojení hello výstup výstupu v StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="3cdc9-280">This PowerShell command tests hello connection status of hello output Output in StreamingJob.</span></span>  

## <a name="get-support"></a><span data-ttu-id="3cdc9-281">Získat podporu</span><span class="sxs-lookup"><span data-stu-id="3cdc9-281">Get support</span></span>
<span data-ttu-id="3cdc9-282">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="3cdc9-282">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3cdc9-283">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3cdc9-283">Next steps</span></span>
* [<span data-ttu-id="3cdc9-284">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3cdc9-284">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="3cdc9-285">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3cdc9-285">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="3cdc9-286">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3cdc9-286">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="3cdc9-287">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="3cdc9-287">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="3cdc9-288">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3cdc9-288">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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

