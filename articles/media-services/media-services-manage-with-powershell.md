---
title: "Správa účtů Azure Media Services pomocí prostředí PowerShell"
description: "Zjistěte, jak chcete spravovat účty pro Azure Media Services pomocí rutin prostředí PowerShell."
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
ms.openlocfilehash: 3d999d9e27844bc0164cc3572522b9ec022118a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a><span data-ttu-id="df3b7-103">Správa účtů Azure Media Services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="df3b7-103">Manage Azure Media Services Accounts with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="df3b7-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="df3b7-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="df3b7-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="df3b7-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="df3b7-106">REST</span><span class="sxs-lookup"><span data-stu-id="df3b7-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="df3b7-107">Abyste mohli vytvořit účet Azure Media Services, musí mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="df3b7-107">To be able to create an Azure Media Services account, you must have an Azure account.</span></span> <span data-ttu-id="df3b7-108">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="df3b7-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="df3b7-109">Podrobnosti najdete v článku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Bezplatná zkušební verze Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="df3b7-109">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="df3b7-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="df3b7-110">Overview</span></span>
<span data-ttu-id="df3b7-111">Tento článek obsahuje seznam rutin prostředí Azure PowerShell pro Azure Media Services (AMS) v rámci Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="df3b7-111">This article lists the Azure PowerShell cmdlets for Azure Media Services (AMS) in the Azure Resource Manager framework.</span></span> <span data-ttu-id="df3b7-112">Rutiny v existovat **Microsoft.Azure.Commands.Media** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="df3b7-112">The cmdlets exist in the **Microsoft.Azure.Commands.Media** namespace.</span></span>

## <a name="versions"></a><span data-ttu-id="df3b7-113">Verze</span><span class="sxs-lookup"><span data-stu-id="df3b7-113">Versions</span></span>
<span data-ttu-id="df3b7-114">**ApiVersion**: "2015-10-01"</span><span class="sxs-lookup"><span data-stu-id="df3b7-114">**ApiVersion**:   "2015-10-01"</span></span>

## <a name="new-azurermmediaservice"></a><span data-ttu-id="df3b7-115">New-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="df3b7-115">New-AzureRmMediaService</span></span>
<span data-ttu-id="df3b7-116">Vytvoří služby media service.</span><span class="sxs-lookup"><span data-stu-id="df3b7-116">Creates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="df3b7-117">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="df3b7-117">Syntax</span></span>
<span data-ttu-id="df3b7-118">Sada parametrů: StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-118">Parameter Set: StorageAccountIdParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

<span data-ttu-id="df3b7-119">Sada parametrů: StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-119">Parameter Set: StorageAccountsParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="df3b7-120">Parametry</span><span class="sxs-lookup"><span data-stu-id="df3b7-120">Parameters</span></span>
<span data-ttu-id="df3b7-121">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-121">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-122">Určuje název skupiny prostředků, do které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-122">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="df3b7-123">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-123">Aliases</span></span> | <span data-ttu-id="df3b7-124">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-125">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-125">Required?</span></span> |<span data-ttu-id="df3b7-126">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-126">true</span></span> |
| <span data-ttu-id="df3b7-127">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-127">Position?</span></span> |<span data-ttu-id="df3b7-128">0</span><span class="sxs-lookup"><span data-stu-id="df3b7-128">0</span></span> |
| <span data-ttu-id="df3b7-129">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-129">Default value</span></span> |<span data-ttu-id="df3b7-130">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-130">none</span></span> |
| <span data-ttu-id="df3b7-131">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-131">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-132">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-132">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-133">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-133">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-134">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-134">false</span></span> |

<span data-ttu-id="df3b7-135">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-135">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-136">Určuje název mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-136">Specifies the name of the media service.</span></span>

| <span data-ttu-id="df3b7-137">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-137">Aliases</span></span> | <span data-ttu-id="df3b7-138">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="df3b7-138">Name</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-139">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-139">Required?</span></span> |<span data-ttu-id="df3b7-140">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-140">true</span></span> |
| <span data-ttu-id="df3b7-141">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-141">Position?</span></span> |<span data-ttu-id="df3b7-142">1</span><span class="sxs-lookup"><span data-stu-id="df3b7-142">1</span></span> |
| <span data-ttu-id="df3b7-143">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-143">Default value</span></span> |<span data-ttu-id="df3b7-144">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-144">none</span></span> |
| <span data-ttu-id="df3b7-145">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-145">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-146">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-146">false</span></span> |
| <span data-ttu-id="df3b7-147">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-147">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-148">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-148">false</span></span> |

<span data-ttu-id="df3b7-149">**-Umístění &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-149">**-Location &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-150">Určuje umístění prostředků mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-150">Specifies the resource location of the media service.</span></span>

| <span data-ttu-id="df3b7-151">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-151">Aliases</span></span> | <span data-ttu-id="df3b7-152">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-152">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-153">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-153">Required?</span></span> |<span data-ttu-id="df3b7-154">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-154">true</span></span> |
| <span data-ttu-id="df3b7-155">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-155">Position?</span></span> |<span data-ttu-id="df3b7-156">2</span><span class="sxs-lookup"><span data-stu-id="df3b7-156">2</span></span> |
| <span data-ttu-id="df3b7-157">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-157">Default value</span></span> |<span data-ttu-id="df3b7-158">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-158">none</span></span> |
| <span data-ttu-id="df3b7-159">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-159">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-160">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-160">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-161">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-161">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-162">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-162">false</span></span> |

<span data-ttu-id="df3b7-163">**-StorageAccountId &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-163">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-164">Určuje primárního úložiště účet, který přidružené mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-164">Specifies a primary storage account that associated with the media service.</span></span>

* <span data-ttu-id="df3b7-165">Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků) podporovaná jenom.</span><span class="sxs-lookup"><span data-stu-id="df3b7-165">New storage account (created with the Resource Manager API) supported only.</span></span>
* <span data-ttu-id="df3b7-166">Účet úložiště, musí existovat a má stejné umístění s mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-166">The storage account must exist and has the same location with the media service.</span></span>

| <span data-ttu-id="df3b7-167">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-167">Aliases</span></span> | <span data-ttu-id="df3b7-168">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-168">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-169">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-169">Required?</span></span> |<span data-ttu-id="df3b7-170">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-170">true</span></span> |
| <span data-ttu-id="df3b7-171">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-171">Position?</span></span> |<span data-ttu-id="df3b7-172">3</span><span class="sxs-lookup"><span data-stu-id="df3b7-172">3</span></span> |
| <span data-ttu-id="df3b7-173">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-173">Default value</span></span> |<span data-ttu-id="df3b7-174">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-174">none</span></span> |
| <span data-ttu-id="df3b7-175">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-175">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-176">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-176">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-177">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="df3b7-177">Parameter set name</span></span> |<span data-ttu-id="df3b7-178">StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-178">StorageAccountIdParamSet</span></span> |
| <span data-ttu-id="df3b7-179">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-179">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-180">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-180">false</span></span> |

<span data-ttu-id="df3b7-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="df3b7-182">Určuje účty úložiště, které přidružené mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-182">Specifies storage accounts that associated with the media service.</span></span>

* <span data-ttu-id="df3b7-183">Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků) podporovaná jenom.</span><span class="sxs-lookup"><span data-stu-id="df3b7-183">New storage account (created with the Resource Manager API) supported only.</span></span>
* <span data-ttu-id="df3b7-184">Účet úložiště, musí existovat a má stejné umístění s mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-184">The storage account must exist and has the same location with the media service.</span></span>
* <span data-ttu-id="df3b7-185">Pouze jeden účet úložiště lze zadat jako primární.</span><span class="sxs-lookup"><span data-stu-id="df3b7-185">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="df3b7-186">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-186">Aliases</span></span> | <span data-ttu-id="df3b7-187">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-187">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-188">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-188">Required?</span></span> |<span data-ttu-id="df3b7-189">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-189">true</span></span> |
| <span data-ttu-id="df3b7-190">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-190">Position?</span></span> |<span data-ttu-id="df3b7-191">3</span><span class="sxs-lookup"><span data-stu-id="df3b7-191">3</span></span> |
| <span data-ttu-id="df3b7-192">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-192">Default value</span></span> |<span data-ttu-id="df3b7-193">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-193">none</span></span> |
| <span data-ttu-id="df3b7-194">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-194">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-195">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-195">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-196">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="df3b7-196">Parameter set name</span></span> |<span data-ttu-id="df3b7-197">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-197">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="df3b7-198">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-198">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-199">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-199">false</span></span> |

<span data-ttu-id="df3b7-200">**-Značky &lt;zatřiďovací tabulky&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-200">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="df3b7-201">Určuje tabulku hash značek, které jsou přidruženy mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-201">Specifies a hash table of the tags that are associated with the media service.</span></span>

* <span data-ttu-id="df3b7-202">Příklad: @{"tag1"="hodnota1";" tag2 "=: value2"}</span><span class="sxs-lookup"><span data-stu-id="df3b7-202">Example: @{"tag1"="value1";"tag2"=:value2"}</span></span>

| <span data-ttu-id="df3b7-203">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-203">Aliases</span></span> | <span data-ttu-id="df3b7-204">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-204">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-205">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-205">Required?</span></span> |<span data-ttu-id="df3b7-206">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-206">false</span></span> |
| <span data-ttu-id="df3b7-207">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-207">Position?</span></span> |<span data-ttu-id="df3b7-208">S názvem</span><span class="sxs-lookup"><span data-stu-id="df3b7-208">named</span></span> |
| <span data-ttu-id="df3b7-209">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-209">Default value</span></span> |<span data-ttu-id="df3b7-210">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-210">none</span></span> |
| <span data-ttu-id="df3b7-211">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-211">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-212">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-212">false</span></span> |
| <span data-ttu-id="df3b7-213">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-213">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-214">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-214">false</span></span> |

<span data-ttu-id="df3b7-215">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-215">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="df3b7-216">Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="df3b7-216">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="df3b7-217">Vstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-217">Inputs</span></span>
<span data-ttu-id="df3b7-218">Vstupní typ je typ objektů, které můžete rutině předat kanálem.</span><span class="sxs-lookup"><span data-stu-id="df3b7-218">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="df3b7-219">Výstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-219">Outputs</span></span>
<span data-ttu-id="df3b7-220">Výstupní typ je typ objektů, které rutina generuje.</span><span class="sxs-lookup"><span data-stu-id="df3b7-220">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="set-azurermmediaservice"></a><span data-ttu-id="df3b7-221">Set-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="df3b7-221">Set-AzureRmMediaService</span></span>
<span data-ttu-id="df3b7-222">Aktualizace služby media service.</span><span class="sxs-lookup"><span data-stu-id="df3b7-222">Updates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="df3b7-223">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="df3b7-223">Syntax</span></span>
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="df3b7-224">Parametry</span><span class="sxs-lookup"><span data-stu-id="df3b7-224">Parameters</span></span>
<span data-ttu-id="df3b7-225">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-225">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-226">Určuje název skupiny prostředků, do které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-226">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="df3b7-227">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-227">Aliases</span></span> | <span data-ttu-id="df3b7-228">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-228">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-229">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-229">Required?</span></span> |<span data-ttu-id="df3b7-230">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-230">true</span></span> |
| <span data-ttu-id="df3b7-231">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-231">Position?</span></span> |<span data-ttu-id="df3b7-232">0</span><span class="sxs-lookup"><span data-stu-id="df3b7-232">0</span></span> |
| <span data-ttu-id="df3b7-233">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-233">Default Value</span></span> |<span data-ttu-id="df3b7-234">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-234">none</span></span> |
| <span data-ttu-id="df3b7-235">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-235">Accept Pipeline Input?</span></span> |<span data-ttu-id="df3b7-236">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-236">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-237">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-237">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-238">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-238">false</span></span> |

<span data-ttu-id="df3b7-239">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-239">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-240">Určuje název mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-240">Specifies the name of the media service.</span></span>

| <span data-ttu-id="df3b7-241">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-241">Aliases</span></span> | <span data-ttu-id="df3b7-242">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="df3b7-242">Name</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-243">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-243">Required?</span></span> |<span data-ttu-id="df3b7-244">True</span><span class="sxs-lookup"><span data-stu-id="df3b7-244">True</span></span> |
| <span data-ttu-id="df3b7-245">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-245">Position?</span></span> |<span data-ttu-id="df3b7-246">1</span><span class="sxs-lookup"><span data-stu-id="df3b7-246">1</span></span> |
| <span data-ttu-id="df3b7-247">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-247">Default value</span></span> |<span data-ttu-id="df3b7-248">Žádný</span><span class="sxs-lookup"><span data-stu-id="df3b7-248">None</span></span> |
| <span data-ttu-id="df3b7-249">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-249">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-250">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-250">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-251">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-251">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-252">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-252">False</span></span> |

<span data-ttu-id="df3b7-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="df3b7-254">Určuje účty úložiště, které přidružené mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-254">Specifies storage accounts that associated with the media service.</span></span>

* <span data-ttu-id="df3b7-255">Nový účet úložiště jsou (vytvořený pomocí rozhraní API Správce prostředků) podporovaná jenom.</span><span class="sxs-lookup"><span data-stu-id="df3b7-255">New storage account (created with the Resource Manager API) supported only.</span></span>
* <span data-ttu-id="df3b7-256">Účet úložiště, musí existovat a má stejné umístění s mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-256">The storage account must exist and has the same location with the media service.</span></span>
* <span data-ttu-id="df3b7-257">Pouze jeden účet úložiště lze zadat jako primární.</span><span class="sxs-lookup"><span data-stu-id="df3b7-257">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="df3b7-258">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-258">Aliases</span></span> | <span data-ttu-id="df3b7-259">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-259">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-260">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-260">Required?</span></span> |<span data-ttu-id="df3b7-261">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-261">false</span></span> |
| <span data-ttu-id="df3b7-262">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-262">Position?</span></span> |<span data-ttu-id="df3b7-263">S názvem</span><span class="sxs-lookup"><span data-stu-id="df3b7-263">Named</span></span> |
| <span data-ttu-id="df3b7-264">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-264">Default value</span></span> |<span data-ttu-id="df3b7-265">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-265">none</span></span> |
| <span data-ttu-id="df3b7-266">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-266">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-267">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-267">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-268">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="df3b7-268">Parameter set name</span></span> |<span data-ttu-id="df3b7-269">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-269">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="df3b7-270">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-270">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-271">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-271">false</span></span> |

<span data-ttu-id="df3b7-272">**-Značky &lt;zatřiďovací tabulky&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-272">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="df3b7-273">Určuje tabulku hash značek, které jsou spojeny s touto službou média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-273">Specifies a hash table of the tags that are associated with this media service.</span></span>

* <span data-ttu-id="df3b7-274">Značky, které jsou přidruženy mediální služby se nahradí hodnotu zadanou pomocí zákazníka.</span><span class="sxs-lookup"><span data-stu-id="df3b7-274">The tags that are associated with the media service are replaced with value specified by the customer.</span></span>

| <span data-ttu-id="df3b7-275">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-275">Aliases</span></span> | <span data-ttu-id="df3b7-276">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-276">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-277">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-277">Required?</span></span> |<span data-ttu-id="df3b7-278">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-278">False</span></span> |
| <span data-ttu-id="df3b7-279">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-279">Position?</span></span> |<span data-ttu-id="df3b7-280">S názvem</span><span class="sxs-lookup"><span data-stu-id="df3b7-280">Named</span></span> |
| <span data-ttu-id="df3b7-281">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-281">Default value</span></span> |<span data-ttu-id="df3b7-282">Žádný</span><span class="sxs-lookup"><span data-stu-id="df3b7-282">None</span></span> |
| <span data-ttu-id="df3b7-283">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-283">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-284">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-284">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-285">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-285">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-286">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-286">false</span></span> |

<span data-ttu-id="df3b7-287">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-287">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="df3b7-288">Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="df3b7-288">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="df3b7-289">Vstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-289">Inputs</span></span>
<span data-ttu-id="df3b7-290">Vstupní typ je typ objektů, které můžete rutině předat kanálem.</span><span class="sxs-lookup"><span data-stu-id="df3b7-290">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="df3b7-291">Výstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-291">Outputs</span></span>
<span data-ttu-id="df3b7-292">Výstupní typ je typ objektů, které rutina generuje.</span><span class="sxs-lookup"><span data-stu-id="df3b7-292">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="remove-azurermmediaservice"></a><span data-ttu-id="df3b7-293">Remove-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="df3b7-293">Remove-AzureRmMediaService</span></span>
<span data-ttu-id="df3b7-294">Odebere služby media service.</span><span class="sxs-lookup"><span data-stu-id="df3b7-294">Removes a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="df3b7-295">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="df3b7-295">Syntax</span></span>
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="df3b7-296">Parametry</span><span class="sxs-lookup"><span data-stu-id="df3b7-296">Parameters</span></span>
<span data-ttu-id="df3b7-297">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-297">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-298">Určuje název skupiny prostředků, do které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-298">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="df3b7-299">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-299">Aliases</span></span> | <span data-ttu-id="df3b7-300">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-300">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-301">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-301">Required?</span></span> |<span data-ttu-id="df3b7-302">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-302">true</span></span> |
| <span data-ttu-id="df3b7-303">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-303">Position?</span></span> |<span data-ttu-id="df3b7-304">0</span><span class="sxs-lookup"><span data-stu-id="df3b7-304">0</span></span> |
| <span data-ttu-id="df3b7-305">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-305">Default value</span></span> |<span data-ttu-id="df3b7-306">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-306">none</span></span> |
| <span data-ttu-id="df3b7-307">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-307">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-308">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-308">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-309">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-309">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-310">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-310">false</span></span> |

<span data-ttu-id="df3b7-311">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-311">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-312">Určuje název mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-312">Specifies the name of the media service.</span></span>

| <span data-ttu-id="df3b7-313">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-313">Aliases</span></span> | <span data-ttu-id="df3b7-314">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-314">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-315">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-315">Required?</span></span> |<span data-ttu-id="df3b7-316">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-316">true</span></span> |
| <span data-ttu-id="df3b7-317">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-317">Position?</span></span> |<span data-ttu-id="df3b7-318">2</span><span class="sxs-lookup"><span data-stu-id="df3b7-318">2</span></span> |
| <span data-ttu-id="df3b7-319">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-319">Default value</span></span> |<span data-ttu-id="df3b7-320">Žádný</span><span class="sxs-lookup"><span data-stu-id="df3b7-320">None</span></span> |
| <span data-ttu-id="df3b7-321">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-321">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-322">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-322">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-323">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-323">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-324">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-324">False</span></span> |

<span data-ttu-id="df3b7-325">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-325">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="df3b7-326">Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="df3b7-326">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="df3b7-327">Vstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-327">Inputs</span></span>
<span data-ttu-id="df3b7-328">Vstupní typ je typ objektů, které můžete rutině předat kanálem.</span><span class="sxs-lookup"><span data-stu-id="df3b7-328">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="df3b7-329">Výstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-329">Outputs</span></span>
<span data-ttu-id="df3b7-330">Výstupní typ je typ objektů, které rutina generuje.</span><span class="sxs-lookup"><span data-stu-id="df3b7-330">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="get-azurermmediaservice"></a><span data-ttu-id="df3b7-331">Get-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="df3b7-331">Get-AzureRmMediaService</span></span>
<span data-ttu-id="df3b7-332">Získá všechny služby media services ve skupině prostředků nebo služby media service se zadaným názvem.</span><span class="sxs-lookup"><span data-stu-id="df3b7-332">Gets all media services in a resource group or a media service with a given name.</span></span>

### <a name="syntax"></a><span data-ttu-id="df3b7-333">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="df3b7-333">Syntax</span></span>
<span data-ttu-id="df3b7-334">ParameterSet: ResourceGroupParameterSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-334">ParameterSet: ResourceGroupParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

<span data-ttu-id="df3b7-335">ParameterSet: AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-335">ParameterSet: AccountNameParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="df3b7-336">Parametry</span><span class="sxs-lookup"><span data-stu-id="df3b7-336">Parameters</span></span>
<span data-ttu-id="df3b7-337">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-337">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-338">Určuje název skupiny prostředků, do které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-338">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="df3b7-339">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-339">Aliases</span></span> | <span data-ttu-id="df3b7-340">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-340">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-341">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-341">Required?</span></span> |<span data-ttu-id="df3b7-342">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-342">true</span></span> |
| <span data-ttu-id="df3b7-343">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-343">Position?</span></span> |<span data-ttu-id="df3b7-344">0</span><span class="sxs-lookup"><span data-stu-id="df3b7-344">0</span></span> |
| <span data-ttu-id="df3b7-345">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-345">Default value</span></span> |<span data-ttu-id="df3b7-346">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-346">none</span></span> |
| <span data-ttu-id="df3b7-347">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-347">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-348">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-348">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-349">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="df3b7-349">Parameter set name</span></span> |<span data-ttu-id="df3b7-350">ResourceGroupParameterSet AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-350">ResourceGroupParameterSet, AccountNameParameterSet</span></span> |

<span data-ttu-id="df3b7-351">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-351">Accept wildcard characters?</span></span>   <span data-ttu-id="df3b7-352">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-352">false</span></span>

<span data-ttu-id="df3b7-353">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-353">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-354">Určuje název mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-354">Specifies the name of the media service.</span></span>

| <span data-ttu-id="df3b7-355">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-355">Aliases</span></span> | <span data-ttu-id="df3b7-356">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-356">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-357">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-357">Required?</span></span> |<span data-ttu-id="df3b7-358">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-358">true</span></span> |
| <span data-ttu-id="df3b7-359">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-359">Position?</span></span> |<span data-ttu-id="df3b7-360">1</span><span class="sxs-lookup"><span data-stu-id="df3b7-360">1</span></span> |
| <span data-ttu-id="df3b7-361">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-361">Default value</span></span> |<span data-ttu-id="df3b7-362">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-362">none</span></span> |
| <span data-ttu-id="df3b7-363">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-363">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-364">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-364">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-365">Název sady parametrů</span><span class="sxs-lookup"><span data-stu-id="df3b7-365">Parameter set name</span></span> |<span data-ttu-id="df3b7-366">AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="df3b7-366">AccountNameParameterSet</span></span> |
| <span data-ttu-id="df3b7-367">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-367">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-368">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-368">false</span></span> |

<span data-ttu-id="df3b7-369">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-369">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="df3b7-370">Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="df3b7-370">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="df3b7-371">Vstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-371">Inputs</span></span>
<span data-ttu-id="df3b7-372">Vstupní typ je typ objektů, které můžete rutině předat kanálem.</span><span class="sxs-lookup"><span data-stu-id="df3b7-372">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="df3b7-373">Výstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-373">Outputs</span></span>
<span data-ttu-id="df3b7-374">Výstupní typ je typ objektů, které rutina generuje.</span><span class="sxs-lookup"><span data-stu-id="df3b7-374">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="get-azurermmediaservicekeys"></a><span data-ttu-id="df3b7-375">Get-AzureRmMediaServiceKeys</span><span class="sxs-lookup"><span data-stu-id="df3b7-375">Get-AzureRmMediaServiceKeys</span></span>
<span data-ttu-id="df3b7-376">Získá klíče služby media service.</span><span class="sxs-lookup"><span data-stu-id="df3b7-376">Gets keys of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="df3b7-377">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="df3b7-377">Syntax</span></span>
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="df3b7-378">Parametry</span><span class="sxs-lookup"><span data-stu-id="df3b7-378">Parameters</span></span>
<span data-ttu-id="df3b7-379">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-379">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-380">Určuje název skupiny prostředků, do které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-380">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="df3b7-381">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-381">Aliases</span></span> | <span data-ttu-id="df3b7-382">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-382">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-383">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-383">Required?</span></span> |<span data-ttu-id="df3b7-384">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-384">true</span></span> |
| <span data-ttu-id="df3b7-385">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-385">Position?</span></span> |<span data-ttu-id="df3b7-386">0</span><span class="sxs-lookup"><span data-stu-id="df3b7-386">0</span></span> |
| <span data-ttu-id="df3b7-387">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-387">Default value</span></span> |<span data-ttu-id="df3b7-388">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-388">none</span></span> |
| <span data-ttu-id="df3b7-389">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-389">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-390">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-390">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-391">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-391">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-392">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-392">false</span></span> |

<span data-ttu-id="df3b7-393">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-393">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-394">Určuje název mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-394">Specifies the name of the media service.</span></span>

| <span data-ttu-id="df3b7-395">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-395">Aliases</span></span> | <span data-ttu-id="df3b7-396">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-396">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-397">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-397">Required?</span></span> |<span data-ttu-id="df3b7-398">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-398">true</span></span> |
| <span data-ttu-id="df3b7-399">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-399">Position?</span></span> |<span data-ttu-id="df3b7-400">1</span><span class="sxs-lookup"><span data-stu-id="df3b7-400">1</span></span> |
| <span data-ttu-id="df3b7-401">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-401">Default value</span></span> |<span data-ttu-id="df3b7-402">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-402">none</span></span> |
| <span data-ttu-id="df3b7-403">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-403">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-404">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-404">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-405">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-405">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-406">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-406">false</span></span> |

<span data-ttu-id="df3b7-407">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-407">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="df3b7-408">Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="df3b7-408">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="df3b7-409">Vstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-409">Inputs</span></span>
<span data-ttu-id="df3b7-410">Vstupní typ je typ objektů, které můžete rutině předat kanálem.</span><span class="sxs-lookup"><span data-stu-id="df3b7-410">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="df3b7-411">Výstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-411">Outputs</span></span>
<span data-ttu-id="df3b7-412">Výstupní typ je typ objektů, které rutina generuje.</span><span class="sxs-lookup"><span data-stu-id="df3b7-412">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="set-azurermmediaservicekey"></a><span data-ttu-id="df3b7-413">Set-AzureRmMediaServiceKey</span><span class="sxs-lookup"><span data-stu-id="df3b7-413">Set-AzureRmMediaServiceKey</span></span>
<span data-ttu-id="df3b7-414">Regeneruje primární nebo sekundární klíč služby média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-414">Regenerates a primary or secondary key of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="df3b7-415">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="df3b7-415">Syntax</span></span>
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="df3b7-416">Parametry</span><span class="sxs-lookup"><span data-stu-id="df3b7-416">Parameters</span></span>
<span data-ttu-id="df3b7-417">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-417">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-418">Určuje název skupiny prostředků, do které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-418">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="df3b7-419">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-419">Aliases</span></span> | <span data-ttu-id="df3b7-420">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-420">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-421">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-421">Required?</span></span> |<span data-ttu-id="df3b7-422">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-422">true</span></span> |
| <span data-ttu-id="df3b7-423">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-423">Position?</span></span> |<span data-ttu-id="df3b7-424">0</span><span class="sxs-lookup"><span data-stu-id="df3b7-424">0</span></span> |
| <span data-ttu-id="df3b7-425">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-425">Default value</span></span> |<span data-ttu-id="df3b7-426">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-426">none</span></span> |
| <span data-ttu-id="df3b7-427">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-427">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-428">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-428">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-429">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-429">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-430">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-430">false</span></span> |

<span data-ttu-id="df3b7-431">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-431">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-432">Určuje název mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-432">Specifies the name of the media service.</span></span>

| <span data-ttu-id="df3b7-433">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-433">Aliases</span></span> | <span data-ttu-id="df3b7-434">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-434">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-435">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-435">Required?</span></span> |<span data-ttu-id="df3b7-436">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-436">true</span></span> |
| <span data-ttu-id="df3b7-437">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-437">Position?</span></span> |<span data-ttu-id="df3b7-438">1</span><span class="sxs-lookup"><span data-stu-id="df3b7-438">1</span></span> |
| <span data-ttu-id="df3b7-439">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-439">Default value</span></span> |<span data-ttu-id="df3b7-440">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-440">none</span></span> |
| <span data-ttu-id="df3b7-441">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-441">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-442">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-442">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-443">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-443">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-444">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-444">false</span></span> |

<span data-ttu-id="df3b7-445">**Typ_klíče - &lt;Typ_klíče.&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-445">**-KeyType &lt;KeyType&gt;**</span></span>

<span data-ttu-id="df3b7-446">Určuje typ klíče mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-446">Specifies the key type of the media service.</span></span>

* <span data-ttu-id="df3b7-447">Primární nebo sekundární</span><span class="sxs-lookup"><span data-stu-id="df3b7-447">Primary or Secondary</span></span>

| <span data-ttu-id="df3b7-448">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-448">Aliases</span></span> | <span data-ttu-id="df3b7-449">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-449">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-450">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-450">Required?</span></span> |<span data-ttu-id="df3b7-451">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-451">true</span></span> |
| <span data-ttu-id="df3b7-452">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-452">Position?</span></span> |<span data-ttu-id="df3b7-453">2</span><span class="sxs-lookup"><span data-stu-id="df3b7-453">2</span></span> |
| <span data-ttu-id="df3b7-454">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-454">Default value</span></span> |<span data-ttu-id="df3b7-455">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-455">none</span></span> |
| <span data-ttu-id="df3b7-456">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-456">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-457">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-457">false</span></span> |
| <span data-ttu-id="df3b7-458">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-458">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-459">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-459">false</span></span> |

<span data-ttu-id="df3b7-460">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-460">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="df3b7-461">Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="df3b7-461">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="df3b7-462">Vstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-462">Inputs</span></span>
<span data-ttu-id="df3b7-463">Vstupní typ je typ objektů, které můžete rutině předat kanálem.</span><span class="sxs-lookup"><span data-stu-id="df3b7-463">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="df3b7-464">Výstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-464">Outputs</span></span>
<span data-ttu-id="df3b7-465">Výstupní typ je typ objektů, které rutina generuje.</span><span class="sxs-lookup"><span data-stu-id="df3b7-465">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="sync-azurermmediaservicestoragekeys"></a><span data-ttu-id="df3b7-466">Sync-AzureRmMediaServiceStorageKeys</span><span class="sxs-lookup"><span data-stu-id="df3b7-466">Sync-AzureRmMediaServiceStorageKeys</span></span>
<span data-ttu-id="df3b7-467">Synchronizuje klíče účtu úložiště pro účet úložiště, který je přidružený k službě média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-467">Synchronizes storage account keys for a storage account associated with the media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="df3b7-468">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="df3b7-468">Syntax</span></span>
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="df3b7-469">Parametry</span><span class="sxs-lookup"><span data-stu-id="df3b7-469">Parameters</span></span>
<span data-ttu-id="df3b7-470">**-ResourceGroupName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-470">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-471">Určuje název skupiny prostředků, do které patří tato služba média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-471">Specifies the name of the resource group to which this media service belongs.</span></span>

| <span data-ttu-id="df3b7-472">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-472">Aliases</span></span> | <span data-ttu-id="df3b7-473">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-473">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-474">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-474">Required?</span></span> |<span data-ttu-id="df3b7-475">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-475">true</span></span> |
| <span data-ttu-id="df3b7-476">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-476">Position?</span></span> |<span data-ttu-id="df3b7-477">0</span><span class="sxs-lookup"><span data-stu-id="df3b7-477">0</span></span> |
| <span data-ttu-id="df3b7-478">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-478">Default value</span></span> |<span data-ttu-id="df3b7-479">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-479">none</span></span> |
| <span data-ttu-id="df3b7-480">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-480">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-481">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-481">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-482">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-482">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-483">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-483">false</span></span> |

<span data-ttu-id="df3b7-484">**-AccountName &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-484">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-485">Určuje název mediální služby.</span><span class="sxs-lookup"><span data-stu-id="df3b7-485">Specifies the name of the media service.</span></span>

| <span data-ttu-id="df3b7-486">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-486">Aliases</span></span> | <span data-ttu-id="df3b7-487">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-487">none</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-488">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-488">Required?</span></span> |<span data-ttu-id="df3b7-489">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-489">true</span></span> |
| <span data-ttu-id="df3b7-490">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-490">Position?</span></span> |<span data-ttu-id="df3b7-491">1</span><span class="sxs-lookup"><span data-stu-id="df3b7-491">1</span></span> |
| <span data-ttu-id="df3b7-492">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-492">Default value</span></span> |<span data-ttu-id="df3b7-493">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-493">none</span></span> |
| <span data-ttu-id="df3b7-494">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-494">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-495">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-495">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-496">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-496">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-497">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-497">false</span></span> |

<span data-ttu-id="df3b7-498">**-StorageAccountId &lt;řetězec&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-498">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="df3b7-499">Určuje účet úložiště, který je přidružený k službě média.</span><span class="sxs-lookup"><span data-stu-id="df3b7-499">Specifies the storage account associated with the media service.</span></span>

| <span data-ttu-id="df3b7-500">Aliasy</span><span class="sxs-lookup"><span data-stu-id="df3b7-500">Aliases</span></span> | <span data-ttu-id="df3b7-501">ID</span><span class="sxs-lookup"><span data-stu-id="df3b7-501">Id</span></span> |
| --- | --- |
| <span data-ttu-id="df3b7-502">Vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="df3b7-502">Required?</span></span> |<span data-ttu-id="df3b7-503">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="df3b7-503">true</span></span> |
| <span data-ttu-id="df3b7-504">Pozice?</span><span class="sxs-lookup"><span data-stu-id="df3b7-504">Position?</span></span> |<span data-ttu-id="df3b7-505">2</span><span class="sxs-lookup"><span data-stu-id="df3b7-505">2</span></span> |
| <span data-ttu-id="df3b7-506">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="df3b7-506">Default value</span></span> |<span data-ttu-id="df3b7-507">None</span><span class="sxs-lookup"><span data-stu-id="df3b7-507">none</span></span> |
| <span data-ttu-id="df3b7-508">Přijímat kanálu vstup?</span><span class="sxs-lookup"><span data-stu-id="df3b7-508">Accept pipeline input?</span></span> |<span data-ttu-id="df3b7-509">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="df3b7-509">true(ByPropertyName)</span></span> |
| <span data-ttu-id="df3b7-510">Přijímat zástupné znaky?</span><span class="sxs-lookup"><span data-stu-id="df3b7-510">Accept wildcard characters?</span></span> |<span data-ttu-id="df3b7-511">False</span><span class="sxs-lookup"><span data-stu-id="df3b7-511">false</span></span> |

<span data-ttu-id="df3b7-512">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="df3b7-512">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="df3b7-513">Tato rutina podporuje společné parametry: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction a -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="df3b7-513">This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="df3b7-514">Vstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-514">Inputs</span></span>
<span data-ttu-id="df3b7-515">Vstupní typ je typ objektů, které můžete rutině předat kanálem.</span><span class="sxs-lookup"><span data-stu-id="df3b7-515">The input type is the type of the objects that you can pipe to the cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="df3b7-516">Výstupy</span><span class="sxs-lookup"><span data-stu-id="df3b7-516">Outputs</span></span>
<span data-ttu-id="df3b7-517">Výstupní typ je typ objektů, které rutina generuje.</span><span class="sxs-lookup"><span data-stu-id="df3b7-517">The output type is the type of the objects that the cmdlet emits.</span></span>

## <a name="next-step"></a><span data-ttu-id="df3b7-518">Další krok</span><span class="sxs-lookup"><span data-stu-id="df3b7-518">Next step</span></span>
<span data-ttu-id="df3b7-519">Podívejte se na kurzů ke službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="df3b7-519">Check out Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="df3b7-520">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="df3b7-520">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

