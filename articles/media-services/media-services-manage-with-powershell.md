---
title: "aaaManage Azure Media Services účty pomocí prostředí PowerShell"
description: "Zjistěte, jak účty toomanage Azure Media Services pomocí rutin prostředí PowerShell."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 17a10c25-d94f-421c-b6bc-ae0958e2ac96
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: juliako
ms.openlocfilehash: e8f97bb2393343e45fabf9c437b4fc09f2525dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a><span data-ttu-id="ad245-103">Správa účtů Azure Media Services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad245-103">Manage Azure Media Services Accounts with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ad245-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ad245-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="ad245-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad245-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="ad245-106">REST</span><span class="sxs-lookup"><span data-stu-id="ad245-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="ad245-107">toobe možné toocreate účtu Azure Media Services, musí mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ad245-107">toobe able toocreate an Azure Media Services account, you must have an Azure account.</span></span> <span data-ttu-id="ad245-108">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="ad245-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ad245-109">Podrobnosti najdete v článku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Bezplatná zkušební verze Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="ad245-109">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="ad245-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="ad245-110">Overview</span></span>
<span data-ttu-id="ad245-111">Tento článek obsahuje seznam rutin prostředí Azure PowerShell hello pro Azure Media Services (AMS) v Azure Resource Manager framework hello.</span><span class="sxs-lookup"><span data-stu-id="ad245-111">This article lists hello Azure PowerShell cmdlets for Azure Media Services (AMS) in hello Azure Resource Manager framework.</span></span> <span data-ttu-id="ad245-112">rutiny Hello existovat v hello **Microsoft.Azure.Commands.Media** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ad245-112">hello cmdlets exist in hello **Microsoft.Azure.Commands.Media** namespace.</span></span>

## <a name="versions"></a><span data-ttu-id="ad245-113">Verze</span><span class="sxs-lookup"><span data-stu-id="ad245-113">Versions</span></span>
<span data-ttu-id="ad245-114">**ApiVersion**: "2015-10-01"</span><span class="sxs-lookup"><span data-stu-id="ad245-114">**ApiVersion**:   "2015-10-01"</span></span>

## <a name="new-azurermmediaservice"></a><span data-ttu-id="ad245-115">New-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="ad245-115">New-AzureRmMediaService</span></span>
<span data-ttu-id="ad245-116">Vytvoří služby media service.</span><span class="sxs-lookup"><span data-stu-id="ad245-116">Creates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="ad245-117">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="ad245-117">Syntax</span></span>
<span data-ttu-id="ad245-118">Sada parametrů: StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="ad245-118">Parameter Set: StorageAccountIdParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

<span data-ttu-id="ad245-119">Sada parametrů: StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="ad245-119">Parameter Set: StorageAccountsParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="ad245-120">Parametry</span><span class="sxs-lookup"><span data-stu-id="ad245-120">Parameters</span></span>
<span data-ttu-id="ad245-121">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-121">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-122">Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="ad245-122">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="ad245-123">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-123">Aliases</span></span> | <span data-ttu-id="ad245-124">None</span><span class="sxs-lookup"><span data-stu-id="ad245-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-125">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-125">Required?</span></span> |<span data-ttu-id="ad245-126">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-126">true</span></span> |
| <span data-ttu-id="ad245-127">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-127">Position?</span></span> |<span data-ttu-id="ad245-128">0</span><span class="sxs-lookup"><span data-stu-id="ad245-128">0</span></span> |
| <span data-ttu-id="ad245-129">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-129">Default value</span></span> |<span data-ttu-id="ad245-130">None</span><span class="sxs-lookup"><span data-stu-id="ad245-130">none</span></span> |
| <span data-ttu-id="ad245-131">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-131">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-132">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-132">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-133">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-133">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-134">False</span><span class="sxs-lookup"><span data-stu-id="ad245-134">false</span></span> |

<span data-ttu-id="ad245-135">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-135">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-136">Určuje název hello hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-136">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="ad245-137">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-137">Aliases</span></span> | <span data-ttu-id="ad245-138">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ad245-138">Name</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-139">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-139">Required?</span></span> |<span data-ttu-id="ad245-140">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-140">true</span></span> |
| <span data-ttu-id="ad245-141">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-141">Position?</span></span> |<span data-ttu-id="ad245-142">1</span><span class="sxs-lookup"><span data-stu-id="ad245-142">1</span></span> |
| <span data-ttu-id="ad245-143">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-143">Default value</span></span> |<span data-ttu-id="ad245-144">None</span><span class="sxs-lookup"><span data-stu-id="ad245-144">none</span></span> |
| <span data-ttu-id="ad245-145">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-145">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-146">False</span><span class="sxs-lookup"><span data-stu-id="ad245-146">false</span></span> |
| <span data-ttu-id="ad245-147">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-147">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-148">False</span><span class="sxs-lookup"><span data-stu-id="ad245-148">false</span></span> |

<span data-ttu-id="ad245-149">**-Umístění &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-149">**-Location &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-150">Určuje umístění prostředků hello hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-150">Specifies hello resource location of hello media service.</span></span>

| <span data-ttu-id="ad245-151">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-151">Aliases</span></span> | <span data-ttu-id="ad245-152">None</span><span class="sxs-lookup"><span data-stu-id="ad245-152">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-153">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-153">Required?</span></span> |<span data-ttu-id="ad245-154">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-154">true</span></span> |
| <span data-ttu-id="ad245-155">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-155">Position?</span></span> |<span data-ttu-id="ad245-156">2</span><span class="sxs-lookup"><span data-stu-id="ad245-156">2</span></span> |
| <span data-ttu-id="ad245-157">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-157">Default value</span></span> |<span data-ttu-id="ad245-158">None</span><span class="sxs-lookup"><span data-stu-id="ad245-158">none</span></span> |
| <span data-ttu-id="ad245-159">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-159">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-160">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-160">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-161">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-161">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-162">False</span><span class="sxs-lookup"><span data-stu-id="ad245-162">false</span></span> |

<span data-ttu-id="ad245-163">**-StorageAccountId &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-163">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-164">Určuje primárního úložiště účet, který přidružené hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-164">Specifies a primary storage account that associated with hello media service.</span></span>

* <span data-ttu-id="ad245-165">Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků hello) podporovaná jenom.</span><span class="sxs-lookup"><span data-stu-id="ad245-165">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="ad245-166">Hello účet úložiště, musí existovat a má hello stejného umístění, hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-166">hello storage account must exist and has hello same location with hello media service.</span></span>

| <span data-ttu-id="ad245-167">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-167">Aliases</span></span> | <span data-ttu-id="ad245-168">None</span><span class="sxs-lookup"><span data-stu-id="ad245-168">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-169">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-169">Required?</span></span> |<span data-ttu-id="ad245-170">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-170">true</span></span> |
| <span data-ttu-id="ad245-171">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-171">Position?</span></span> |<span data-ttu-id="ad245-172">3</span><span class="sxs-lookup"><span data-stu-id="ad245-172">3</span></span> |
| <span data-ttu-id="ad245-173">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-173">Default value</span></span> |<span data-ttu-id="ad245-174">None</span><span class="sxs-lookup"><span data-stu-id="ad245-174">none</span></span> |
| <span data-ttu-id="ad245-175">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-175">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-176">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-176">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-177">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="ad245-177">Parameter set name</span></span> |<span data-ttu-id="ad245-178">StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="ad245-178">StorageAccountIdParamSet</span></span> |
| <span data-ttu-id="ad245-179">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-179">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-180">False</span><span class="sxs-lookup"><span data-stu-id="ad245-180">false</span></span> |

<span data-ttu-id="ad245-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="ad245-182">Určuje účty úložiště, které přísluší službě hello média.</span><span class="sxs-lookup"><span data-stu-id="ad245-182">Specifies storage accounts that associated with hello media service.</span></span>

* <span data-ttu-id="ad245-183">Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků hello) podporovaná jenom.</span><span class="sxs-lookup"><span data-stu-id="ad245-183">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="ad245-184">Hello účet úložiště, musí existovat a má hello stejného umístění, hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-184">hello storage account must exist and has hello same location with hello media service.</span></span>
* <span data-ttu-id="ad245-185">Pouze jeden účet úložiště lze zadat jako primární.</span><span class="sxs-lookup"><span data-stu-id="ad245-185">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="ad245-186">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-186">Aliases</span></span> | <span data-ttu-id="ad245-187">None</span><span class="sxs-lookup"><span data-stu-id="ad245-187">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-188">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-188">Required?</span></span> |<span data-ttu-id="ad245-189">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-189">true</span></span> |
| <span data-ttu-id="ad245-190">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-190">Position?</span></span> |<span data-ttu-id="ad245-191">3</span><span class="sxs-lookup"><span data-stu-id="ad245-191">3</span></span> |
| <span data-ttu-id="ad245-192">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-192">Default value</span></span> |<span data-ttu-id="ad245-193">None</span><span class="sxs-lookup"><span data-stu-id="ad245-193">none</span></span> |
| <span data-ttu-id="ad245-194">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-194">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-195">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-195">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-196">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="ad245-196">Parameter set name</span></span> |<span data-ttu-id="ad245-197">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="ad245-197">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="ad245-198">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-198">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-199">False</span><span class="sxs-lookup"><span data-stu-id="ad245-199">false</span></span> |

<span data-ttu-id="ad245-200">**-Značky &lt;zatřiďovací tabulky&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-200">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="ad245-201">Určuje tabulku hash hello značky, které jsou přidruženy hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-201">Specifies a hash table of hello tags that are associated with hello media service.</span></span>

* <span data-ttu-id="ad245-202">Příklad: @{"tag1"="hodnota1";" tag2 "=: value2"}</span><span class="sxs-lookup"><span data-stu-id="ad245-202">Example: @{"tag1"="value1";"tag2"=:value2"}</span></span>

| <span data-ttu-id="ad245-203">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-203">Aliases</span></span> | <span data-ttu-id="ad245-204">None</span><span class="sxs-lookup"><span data-stu-id="ad245-204">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-205">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-205">Required?</span></span> |<span data-ttu-id="ad245-206">False</span><span class="sxs-lookup"><span data-stu-id="ad245-206">false</span></span> |
| <span data-ttu-id="ad245-207">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-207">Position?</span></span> |<span data-ttu-id="ad245-208">S názvem</span><span class="sxs-lookup"><span data-stu-id="ad245-208">named</span></span> |
| <span data-ttu-id="ad245-209">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-209">Default value</span></span> |<span data-ttu-id="ad245-210">None</span><span class="sxs-lookup"><span data-stu-id="ad245-210">none</span></span> |
| <span data-ttu-id="ad245-211">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-211">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-212">False</span><span class="sxs-lookup"><span data-stu-id="ad245-212">false</span></span> |
| <span data-ttu-id="ad245-213">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-213">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-214">False</span><span class="sxs-lookup"><span data-stu-id="ad245-214">false</span></span> |

<span data-ttu-id="ad245-215">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-215">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="ad245-216">Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="ad245-216">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="ad245-217">Vstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-217">Inputs</span></span>
<span data-ttu-id="ad245-218">Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="ad245-218">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="ad245-219">Výstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-219">Outputs</span></span>
<span data-ttu-id="ad245-220">Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.</span><span class="sxs-lookup"><span data-stu-id="ad245-220">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="set-azurermmediaservice"></a><span data-ttu-id="ad245-221">Set-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="ad245-221">Set-AzureRmMediaService</span></span>
<span data-ttu-id="ad245-222">Aktualizace služby media service.</span><span class="sxs-lookup"><span data-stu-id="ad245-222">Updates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="ad245-223">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="ad245-223">Syntax</span></span>
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="ad245-224">Parametry</span><span class="sxs-lookup"><span data-stu-id="ad245-224">Parameters</span></span>
<span data-ttu-id="ad245-225">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-225">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-226">Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="ad245-226">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="ad245-227">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-227">Aliases</span></span> | <span data-ttu-id="ad245-228">None</span><span class="sxs-lookup"><span data-stu-id="ad245-228">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-229">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-229">Required?</span></span> |<span data-ttu-id="ad245-230">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-230">true</span></span> |
| <span data-ttu-id="ad245-231">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-231">Position?</span></span> |<span data-ttu-id="ad245-232">0</span><span class="sxs-lookup"><span data-stu-id="ad245-232">0</span></span> |
| <span data-ttu-id="ad245-233">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-233">Default Value</span></span> |<span data-ttu-id="ad245-234">None</span><span class="sxs-lookup"><span data-stu-id="ad245-234">none</span></span> |
| <span data-ttu-id="ad245-235">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-235">Accept Pipeline Input?</span></span> |<span data-ttu-id="ad245-236">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-236">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-237">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-237">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-238">False</span><span class="sxs-lookup"><span data-stu-id="ad245-238">false</span></span> |

<span data-ttu-id="ad245-239">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-239">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-240">Určuje název hello hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-240">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="ad245-241">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-241">Aliases</span></span> | <span data-ttu-id="ad245-242">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ad245-242">Name</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-243">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-243">Required?</span></span> |<span data-ttu-id="ad245-244">True</span><span class="sxs-lookup"><span data-stu-id="ad245-244">True</span></span> |
| <span data-ttu-id="ad245-245">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-245">Position?</span></span> |<span data-ttu-id="ad245-246">1</span><span class="sxs-lookup"><span data-stu-id="ad245-246">1</span></span> |
| <span data-ttu-id="ad245-247">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-247">Default value</span></span> |<span data-ttu-id="ad245-248">Žádný</span><span class="sxs-lookup"><span data-stu-id="ad245-248">None</span></span> |
| <span data-ttu-id="ad245-249">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-249">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-250">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-250">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-251">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-251">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-252">False</span><span class="sxs-lookup"><span data-stu-id="ad245-252">False</span></span> |

<span data-ttu-id="ad245-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="ad245-254">Určuje účty úložiště, které přísluší službě hello média.</span><span class="sxs-lookup"><span data-stu-id="ad245-254">Specifies storage accounts that associated with hello media service.</span></span>

* <span data-ttu-id="ad245-255">Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků hello) podporovaná jenom.</span><span class="sxs-lookup"><span data-stu-id="ad245-255">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="ad245-256">Hello účet úložiště, musí existovat a má hello stejného umístění, hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-256">hello storage account must exist and has hello same location with hello media service.</span></span>
* <span data-ttu-id="ad245-257">Pouze jeden účet úložiště lze zadat jako primární.</span><span class="sxs-lookup"><span data-stu-id="ad245-257">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="ad245-258">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-258">Aliases</span></span> | <span data-ttu-id="ad245-259">None</span><span class="sxs-lookup"><span data-stu-id="ad245-259">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-260">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-260">Required?</span></span> |<span data-ttu-id="ad245-261">False</span><span class="sxs-lookup"><span data-stu-id="ad245-261">false</span></span> |
| <span data-ttu-id="ad245-262">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-262">Position?</span></span> |<span data-ttu-id="ad245-263">S názvem</span><span class="sxs-lookup"><span data-stu-id="ad245-263">Named</span></span> |
| <span data-ttu-id="ad245-264">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-264">Default value</span></span> |<span data-ttu-id="ad245-265">None</span><span class="sxs-lookup"><span data-stu-id="ad245-265">none</span></span> |
| <span data-ttu-id="ad245-266">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-266">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-267">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-267">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-268">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="ad245-268">Parameter set name</span></span> |<span data-ttu-id="ad245-269">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="ad245-269">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="ad245-270">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-270">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-271">False</span><span class="sxs-lookup"><span data-stu-id="ad245-271">false</span></span> |

<span data-ttu-id="ad245-272">**-Značky &lt;zatřiďovací tabulky&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-272">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="ad245-273">Určuje tabulku hash hello značky, které jsou spojeny s touto službou média.</span><span class="sxs-lookup"><span data-stu-id="ad245-273">Specifies a hash table of hello tags that are associated with this media service.</span></span>

* <span data-ttu-id="ad245-274">Hello značky, které jsou přidruženy hello mediální služby se nahradí hodnotu zadanou pomocí hello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="ad245-274">hello tags that are associated with hello media service are replaced with value specified by hello customer.</span></span>

| <span data-ttu-id="ad245-275">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-275">Aliases</span></span> | <span data-ttu-id="ad245-276">None</span><span class="sxs-lookup"><span data-stu-id="ad245-276">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-277">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-277">Required?</span></span> |<span data-ttu-id="ad245-278">False</span><span class="sxs-lookup"><span data-stu-id="ad245-278">False</span></span> |
| <span data-ttu-id="ad245-279">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-279">Position?</span></span> |<span data-ttu-id="ad245-280">S názvem</span><span class="sxs-lookup"><span data-stu-id="ad245-280">Named</span></span> |
| <span data-ttu-id="ad245-281">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-281">Default value</span></span> |<span data-ttu-id="ad245-282">Žádný</span><span class="sxs-lookup"><span data-stu-id="ad245-282">None</span></span> |
| <span data-ttu-id="ad245-283">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-283">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-284">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-284">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-285">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-285">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-286">False</span><span class="sxs-lookup"><span data-stu-id="ad245-286">false</span></span> |

<span data-ttu-id="ad245-287">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-287">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="ad245-288">Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="ad245-288">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="ad245-289">Vstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-289">Inputs</span></span>
<span data-ttu-id="ad245-290">Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="ad245-290">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="ad245-291">Výstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-291">Outputs</span></span>
<span data-ttu-id="ad245-292">Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.</span><span class="sxs-lookup"><span data-stu-id="ad245-292">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="remove-azurermmediaservice"></a><span data-ttu-id="ad245-293">Remove-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="ad245-293">Remove-AzureRmMediaService</span></span>
<span data-ttu-id="ad245-294">Odebere služby media service.</span><span class="sxs-lookup"><span data-stu-id="ad245-294">Removes a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="ad245-295">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="ad245-295">Syntax</span></span>
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="ad245-296">Parametry</span><span class="sxs-lookup"><span data-stu-id="ad245-296">Parameters</span></span>
<span data-ttu-id="ad245-297">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-297">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-298">Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="ad245-298">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="ad245-299">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-299">Aliases</span></span> | <span data-ttu-id="ad245-300">None</span><span class="sxs-lookup"><span data-stu-id="ad245-300">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-301">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-301">Required?</span></span> |<span data-ttu-id="ad245-302">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-302">true</span></span> |
| <span data-ttu-id="ad245-303">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-303">Position?</span></span> |<span data-ttu-id="ad245-304">0</span><span class="sxs-lookup"><span data-stu-id="ad245-304">0</span></span> |
| <span data-ttu-id="ad245-305">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-305">Default value</span></span> |<span data-ttu-id="ad245-306">None</span><span class="sxs-lookup"><span data-stu-id="ad245-306">none</span></span> |
| <span data-ttu-id="ad245-307">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-307">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-308">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-308">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-309">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-309">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-310">False</span><span class="sxs-lookup"><span data-stu-id="ad245-310">false</span></span> |

<span data-ttu-id="ad245-311">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-311">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-312">Určuje název hello hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-312">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="ad245-313">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-313">Aliases</span></span> | <span data-ttu-id="ad245-314">None</span><span class="sxs-lookup"><span data-stu-id="ad245-314">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-315">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-315">Required?</span></span> |<span data-ttu-id="ad245-316">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-316">true</span></span> |
| <span data-ttu-id="ad245-317">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-317">Position?</span></span> |<span data-ttu-id="ad245-318">2</span><span class="sxs-lookup"><span data-stu-id="ad245-318">2</span></span> |
| <span data-ttu-id="ad245-319">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-319">Default value</span></span> |<span data-ttu-id="ad245-320">Žádný</span><span class="sxs-lookup"><span data-stu-id="ad245-320">None</span></span> |
| <span data-ttu-id="ad245-321">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-321">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-322">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-322">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-323">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-323">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-324">False</span><span class="sxs-lookup"><span data-stu-id="ad245-324">False</span></span> |

<span data-ttu-id="ad245-325">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-325">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="ad245-326">Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="ad245-326">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="ad245-327">Vstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-327">Inputs</span></span>
<span data-ttu-id="ad245-328">Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="ad245-328">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="ad245-329">Výstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-329">Outputs</span></span>
<span data-ttu-id="ad245-330">Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.</span><span class="sxs-lookup"><span data-stu-id="ad245-330">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="get-azurermmediaservice"></a><span data-ttu-id="ad245-331">Get-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="ad245-331">Get-AzureRmMediaService</span></span>
<span data-ttu-id="ad245-332">Získá všechny služby media services ve skupině prostředků nebo služby media service se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="ad245-332">Gets all media services in a resource group or a media service with a given name.</span></span>

### <a name="syntax"></a><span data-ttu-id="ad245-333">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="ad245-333">Syntax</span></span>
<span data-ttu-id="ad245-334">ParameterSet: ResourceGroupParameterSet</span><span class="sxs-lookup"><span data-stu-id="ad245-334">ParameterSet: ResourceGroupParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

<span data-ttu-id="ad245-335">ParameterSet: AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="ad245-335">ParameterSet: AccountNameParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="ad245-336">Parametry</span><span class="sxs-lookup"><span data-stu-id="ad245-336">Parameters</span></span>
<span data-ttu-id="ad245-337">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-337">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-338">Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="ad245-338">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="ad245-339">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-339">Aliases</span></span> | <span data-ttu-id="ad245-340">None</span><span class="sxs-lookup"><span data-stu-id="ad245-340">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-341">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-341">Required?</span></span> |<span data-ttu-id="ad245-342">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-342">true</span></span> |
| <span data-ttu-id="ad245-343">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-343">Position?</span></span> |<span data-ttu-id="ad245-344">0</span><span class="sxs-lookup"><span data-stu-id="ad245-344">0</span></span> |
| <span data-ttu-id="ad245-345">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-345">Default value</span></span> |<span data-ttu-id="ad245-346">None</span><span class="sxs-lookup"><span data-stu-id="ad245-346">none</span></span> |
| <span data-ttu-id="ad245-347">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-347">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-348">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-348">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-349">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="ad245-349">Parameter set name</span></span> |<span data-ttu-id="ad245-350">ResourceGroupParameterSet AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="ad245-350">ResourceGroupParameterSet, AccountNameParameterSet</span></span> |

<span data-ttu-id="ad245-351">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-351">Accept wildcard characters?</span></span>   <span data-ttu-id="ad245-352">False</span><span class="sxs-lookup"><span data-stu-id="ad245-352">false</span></span>

<span data-ttu-id="ad245-353">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-353">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-354">Určuje název hello hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-354">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="ad245-355">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-355">Aliases</span></span> | <span data-ttu-id="ad245-356">None</span><span class="sxs-lookup"><span data-stu-id="ad245-356">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-357">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-357">Required?</span></span> |<span data-ttu-id="ad245-358">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-358">true</span></span> |
| <span data-ttu-id="ad245-359">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-359">Position?</span></span> |<span data-ttu-id="ad245-360">1</span><span class="sxs-lookup"><span data-stu-id="ad245-360">1</span></span> |
| <span data-ttu-id="ad245-361">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-361">Default value</span></span> |<span data-ttu-id="ad245-362">None</span><span class="sxs-lookup"><span data-stu-id="ad245-362">none</span></span> |
| <span data-ttu-id="ad245-363">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-363">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-364">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-364">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-365">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="ad245-365">Parameter set name</span></span> |<span data-ttu-id="ad245-366">AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="ad245-366">AccountNameParameterSet</span></span> |
| <span data-ttu-id="ad245-367">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-367">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-368">False</span><span class="sxs-lookup"><span data-stu-id="ad245-368">false</span></span> |

<span data-ttu-id="ad245-369">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-369">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="ad245-370">Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="ad245-370">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="ad245-371">Vstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-371">Inputs</span></span>
<span data-ttu-id="ad245-372">Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="ad245-372">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="ad245-373">Výstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-373">Outputs</span></span>
<span data-ttu-id="ad245-374">Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.</span><span class="sxs-lookup"><span data-stu-id="ad245-374">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="get-azurermmediaservicekeys"></a><span data-ttu-id="ad245-375">Get-AzureRmMediaServiceKeys</span><span class="sxs-lookup"><span data-stu-id="ad245-375">Get-AzureRmMediaServiceKeys</span></span>
<span data-ttu-id="ad245-376">Získá klíče služby media service.</span><span class="sxs-lookup"><span data-stu-id="ad245-376">Gets keys of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="ad245-377">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="ad245-377">Syntax</span></span>
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="ad245-378">Parametry</span><span class="sxs-lookup"><span data-stu-id="ad245-378">Parameters</span></span>
<span data-ttu-id="ad245-379">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-379">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-380">Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="ad245-380">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="ad245-381">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-381">Aliases</span></span> | <span data-ttu-id="ad245-382">None</span><span class="sxs-lookup"><span data-stu-id="ad245-382">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-383">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-383">Required?</span></span> |<span data-ttu-id="ad245-384">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-384">true</span></span> |
| <span data-ttu-id="ad245-385">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-385">Position?</span></span> |<span data-ttu-id="ad245-386">0</span><span class="sxs-lookup"><span data-stu-id="ad245-386">0</span></span> |
| <span data-ttu-id="ad245-387">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-387">Default value</span></span> |<span data-ttu-id="ad245-388">None</span><span class="sxs-lookup"><span data-stu-id="ad245-388">none</span></span> |
| <span data-ttu-id="ad245-389">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-389">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-390">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-390">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-391">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-391">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-392">False</span><span class="sxs-lookup"><span data-stu-id="ad245-392">false</span></span> |

<span data-ttu-id="ad245-393">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-393">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-394">Určuje název hello hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-394">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="ad245-395">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-395">Aliases</span></span> | <span data-ttu-id="ad245-396">None</span><span class="sxs-lookup"><span data-stu-id="ad245-396">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-397">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-397">Required?</span></span> |<span data-ttu-id="ad245-398">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-398">true</span></span> |
| <span data-ttu-id="ad245-399">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-399">Position?</span></span> |<span data-ttu-id="ad245-400">1</span><span class="sxs-lookup"><span data-stu-id="ad245-400">1</span></span> |
| <span data-ttu-id="ad245-401">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-401">Default value</span></span> |<span data-ttu-id="ad245-402">None</span><span class="sxs-lookup"><span data-stu-id="ad245-402">none</span></span> |
| <span data-ttu-id="ad245-403">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-403">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-404">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-404">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-405">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-405">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-406">False</span><span class="sxs-lookup"><span data-stu-id="ad245-406">false</span></span> |

<span data-ttu-id="ad245-407">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-407">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="ad245-408">Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="ad245-408">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="ad245-409">Vstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-409">Inputs</span></span>
<span data-ttu-id="ad245-410">Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="ad245-410">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="ad245-411">Výstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-411">Outputs</span></span>
<span data-ttu-id="ad245-412">Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.</span><span class="sxs-lookup"><span data-stu-id="ad245-412">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="set-azurermmediaservicekey"></a><span data-ttu-id="ad245-413">Set-AzureRmMediaServiceKey</span><span class="sxs-lookup"><span data-stu-id="ad245-413">Set-AzureRmMediaServiceKey</span></span>
<span data-ttu-id="ad245-414">Regeneruje primární nebo sekundární klíč služby média.</span><span class="sxs-lookup"><span data-stu-id="ad245-414">Regenerates a primary or secondary key of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="ad245-415">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="ad245-415">Syntax</span></span>
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="ad245-416">Parametry</span><span class="sxs-lookup"><span data-stu-id="ad245-416">Parameters</span></span>
<span data-ttu-id="ad245-417">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-417">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-418">Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="ad245-418">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="ad245-419">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-419">Aliases</span></span> | <span data-ttu-id="ad245-420">None</span><span class="sxs-lookup"><span data-stu-id="ad245-420">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-421">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-421">Required?</span></span> |<span data-ttu-id="ad245-422">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-422">true</span></span> |
| <span data-ttu-id="ad245-423">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-423">Position?</span></span> |<span data-ttu-id="ad245-424">0</span><span class="sxs-lookup"><span data-stu-id="ad245-424">0</span></span> |
| <span data-ttu-id="ad245-425">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-425">Default value</span></span> |<span data-ttu-id="ad245-426">None</span><span class="sxs-lookup"><span data-stu-id="ad245-426">none</span></span> |
| <span data-ttu-id="ad245-427">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-427">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-428">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-428">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-429">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-429">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-430">False</span><span class="sxs-lookup"><span data-stu-id="ad245-430">false</span></span> |

<span data-ttu-id="ad245-431">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-431">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-432">Určuje název hello hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-432">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="ad245-433">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-433">Aliases</span></span> | <span data-ttu-id="ad245-434">None</span><span class="sxs-lookup"><span data-stu-id="ad245-434">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-435">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-435">Required?</span></span> |<span data-ttu-id="ad245-436">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-436">true</span></span> |
| <span data-ttu-id="ad245-437">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-437">Position?</span></span> |<span data-ttu-id="ad245-438">1</span><span class="sxs-lookup"><span data-stu-id="ad245-438">1</span></span> |
| <span data-ttu-id="ad245-439">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-439">Default value</span></span> |<span data-ttu-id="ad245-440">None</span><span class="sxs-lookup"><span data-stu-id="ad245-440">none</span></span> |
| <span data-ttu-id="ad245-441">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-441">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-442">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-442">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-443">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-443">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-444">False</span><span class="sxs-lookup"><span data-stu-id="ad245-444">false</span></span> |

<span data-ttu-id="ad245-445">**Typ_klíče - &lt;Typ_klíče.&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-445">**-KeyType &lt;KeyType&gt;**</span></span>

<span data-ttu-id="ad245-446">Určuje typ klíče hello hello média služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-446">Specifies hello key type of hello media service.</span></span>

* <span data-ttu-id="ad245-447">Primární nebo sekundární</span><span class="sxs-lookup"><span data-stu-id="ad245-447">Primary or Secondary</span></span>

| <span data-ttu-id="ad245-448">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-448">Aliases</span></span> | <span data-ttu-id="ad245-449">None</span><span class="sxs-lookup"><span data-stu-id="ad245-449">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-450">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-450">Required?</span></span> |<span data-ttu-id="ad245-451">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-451">true</span></span> |
| <span data-ttu-id="ad245-452">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-452">Position?</span></span> |<span data-ttu-id="ad245-453">2</span><span class="sxs-lookup"><span data-stu-id="ad245-453">2</span></span> |
| <span data-ttu-id="ad245-454">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-454">Default value</span></span> |<span data-ttu-id="ad245-455">None</span><span class="sxs-lookup"><span data-stu-id="ad245-455">none</span></span> |
| <span data-ttu-id="ad245-456">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-456">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-457">False</span><span class="sxs-lookup"><span data-stu-id="ad245-457">false</span></span> |
| <span data-ttu-id="ad245-458">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-458">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-459">False</span><span class="sxs-lookup"><span data-stu-id="ad245-459">false</span></span> |

<span data-ttu-id="ad245-460">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-460">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="ad245-461">Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="ad245-461">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="ad245-462">Vstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-462">Inputs</span></span>
<span data-ttu-id="ad245-463">Hello vstupní typ je typ hello hello objekty toothe rutiny můžete prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="ad245-463">hello input type is hello type of hello objects that you can pipe toothe cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="ad245-464">Výstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-464">Outputs</span></span>
<span data-ttu-id="ad245-465">Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.</span><span class="sxs-lookup"><span data-stu-id="ad245-465">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="sync-azurermmediaservicestoragekeys"></a><span data-ttu-id="ad245-466">Sync-AzureRmMediaServiceStorageKeys</span><span class="sxs-lookup"><span data-stu-id="ad245-466">Sync-AzureRmMediaServiceStorageKeys</span></span>
<span data-ttu-id="ad245-467">Synchronizuje klíče účtu úložiště pro účet úložiště přidružené hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-467">Synchronizes storage account keys for a storage account associated with hello media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="ad245-468">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="ad245-468">Syntax</span></span>
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="ad245-469">Parametry</span><span class="sxs-lookup"><span data-stu-id="ad245-469">Parameters</span></span>
<span data-ttu-id="ad245-470">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-470">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-471">Určuje název hello hello prostředků skupiny toowhich, ke které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="ad245-471">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="ad245-472">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-472">Aliases</span></span> | <span data-ttu-id="ad245-473">None</span><span class="sxs-lookup"><span data-stu-id="ad245-473">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-474">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-474">Required?</span></span> |<span data-ttu-id="ad245-475">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-475">true</span></span> |
| <span data-ttu-id="ad245-476">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-476">Position?</span></span> |<span data-ttu-id="ad245-477">0</span><span class="sxs-lookup"><span data-stu-id="ad245-477">0</span></span> |
| <span data-ttu-id="ad245-478">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-478">Default value</span></span> |<span data-ttu-id="ad245-479">None</span><span class="sxs-lookup"><span data-stu-id="ad245-479">none</span></span> |
| <span data-ttu-id="ad245-480">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-480">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-481">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-481">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-482">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-482">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-483">False</span><span class="sxs-lookup"><span data-stu-id="ad245-483">false</span></span> |

<span data-ttu-id="ad245-484">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-484">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-485">Určuje název hello hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-485">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="ad245-486">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-486">Aliases</span></span> | <span data-ttu-id="ad245-487">None</span><span class="sxs-lookup"><span data-stu-id="ad245-487">none</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-488">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-488">Required?</span></span> |<span data-ttu-id="ad245-489">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-489">true</span></span> |
| <span data-ttu-id="ad245-490">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-490">Position?</span></span> |<span data-ttu-id="ad245-491">1</span><span class="sxs-lookup"><span data-stu-id="ad245-491">1</span></span> |
| <span data-ttu-id="ad245-492">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-492">Default value</span></span> |<span data-ttu-id="ad245-493">None</span><span class="sxs-lookup"><span data-stu-id="ad245-493">none</span></span> |
| <span data-ttu-id="ad245-494">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-494">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-495">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-495">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-496">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-496">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-497">False</span><span class="sxs-lookup"><span data-stu-id="ad245-497">false</span></span> |

<span data-ttu-id="ad245-498">**-StorageAccountId &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-498">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="ad245-499">Určuje účet úložiště hello přidružený hello mediální služby.</span><span class="sxs-lookup"><span data-stu-id="ad245-499">Specifies hello storage account associated with hello media service.</span></span>

| <span data-ttu-id="ad245-500">Aliasy</span><span class="sxs-lookup"><span data-stu-id="ad245-500">Aliases</span></span> | <span data-ttu-id="ad245-501">ID</span><span class="sxs-lookup"><span data-stu-id="ad245-501">Id</span></span> |
| --- | --- |
| <span data-ttu-id="ad245-502">Povinné?</span><span class="sxs-lookup"><span data-stu-id="ad245-502">Required?</span></span> |<span data-ttu-id="ad245-503">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="ad245-503">true</span></span> |
| <span data-ttu-id="ad245-504">Pozice?</span><span class="sxs-lookup"><span data-stu-id="ad245-504">Position?</span></span> |<span data-ttu-id="ad245-505">2</span><span class="sxs-lookup"><span data-stu-id="ad245-505">2</span></span> |
| <span data-ttu-id="ad245-506">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="ad245-506">Default value</span></span> |<span data-ttu-id="ad245-507">None</span><span class="sxs-lookup"><span data-stu-id="ad245-507">none</span></span> |
| <span data-ttu-id="ad245-508">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="ad245-508">Accept pipeline input?</span></span> |<span data-ttu-id="ad245-509">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="ad245-509">true(ByPropertyName)</span></span> |
| <span data-ttu-id="ad245-510">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="ad245-510">Accept wildcard characters?</span></span> |<span data-ttu-id="ad245-511">False</span><span class="sxs-lookup"><span data-stu-id="ad245-511">false</span></span> |

<span data-ttu-id="ad245-512">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="ad245-512">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="ad245-513">Tato rutina podporuje běžné parametry hello:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction a - WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="ad245-513">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="ad245-514">Vstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-514">Inputs</span></span>
<span data-ttu-id="ad245-515">Hello vstupní typ je typ hello hello objekty toohello rutiny můžete prostřednictvím kanálu.</span><span class="sxs-lookup"><span data-stu-id="ad245-515">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="ad245-516">Výstupy</span><span class="sxs-lookup"><span data-stu-id="ad245-516">Outputs</span></span>
<span data-ttu-id="ad245-517">Hello výstupní typ je typ hello hello objektů, které hello rutiny vysílá.</span><span class="sxs-lookup"><span data-stu-id="ad245-517">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="next-step"></a><span data-ttu-id="ad245-518">Další krok</span><span class="sxs-lookup"><span data-stu-id="ad245-518">Next step</span></span>
<span data-ttu-id="ad245-519">Podívejte se na kurzů ke službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="ad245-519">Check out Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ad245-520">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ad245-520">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

