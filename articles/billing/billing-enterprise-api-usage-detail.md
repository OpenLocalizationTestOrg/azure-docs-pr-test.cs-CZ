---
title: "Azure fakturace Enterprise rozhraní API – podrobnosti o použití | Microsoft Docs"
description: "Další informace o využití fakturace Azure a RateCard rozhraní API, které poskytují přehled o využívání prostředků Azure a trendy."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: 5b49220e6eb27544dba54255ee88c56ad79c3141
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="reporting-apis-for-enterprise-customers---usage-details"></a><span data-ttu-id="318db-103">Rozhraní API pro vytváření sestav pro podnikové zákazníky – podrobnosti o použití</span><span class="sxs-lookup"><span data-stu-id="318db-103">Reporting APIs for Enterprise customers - Usage Details</span></span>

<span data-ttu-id="318db-104">Rozhraní API podrobnosti o využití nabízí denní rozpis těchto spotřebované počty a odhadované poplatky podle zápisu.</span><span class="sxs-lookup"><span data-stu-id="318db-104">The Usage Detail API offers a daily breakdown of consumed quantities and estimated charges by an Enrollment.</span></span> <span data-ttu-id="318db-105">Výsledek také obsahuje informace o instancích, měřidla a oddělení.</span><span class="sxs-lookup"><span data-stu-id="318db-105">The result also includes information on instances, meters and departments.</span></span> <span data-ttu-id="318db-106">Rozhraní API můžete položit dotaz na fakturační období nebo zadaný počáteční a koncové datum.</span><span class="sxs-lookup"><span data-stu-id="318db-106">The API can be queried by Billing period or by a specified start and end date.</span></span> 
## <a name="consumption-apis"></a><span data-ttu-id="318db-107">Rozhraní API spotřeba</span><span class="sxs-lookup"><span data-stu-id="318db-107">Consumption APIs</span></span>


##<a name="request"></a><span data-ttu-id="318db-108">Žádost</span><span class="sxs-lookup"><span data-stu-id="318db-108">Request</span></span> 
<span data-ttu-id="318db-109">Nejsou zadány společných vlastností hlavičky, které je třeba přidat [zde](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="318db-109">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="318db-110">Pokud není zadán fakturační období, je vrácena data pro aktuální fakturační období.</span><span class="sxs-lookup"><span data-stu-id="318db-110">If a billing period is not specified, then data for the current billing period is returned.</span></span> <span data-ttu-id="318db-111">Vlastní časových rozsahů lze určit spuštění a ukončení datum parametry, které jsou ve formátu RRRR MM-dd.</span><span class="sxs-lookup"><span data-stu-id="318db-111">Custom time ranges can be specified with the start and end date parameters that are in the format yyyy-MM-dd.</span></span> <span data-ttu-id="318db-112">Maximální podporovaný časový rozsah je 36 měsíců.</span><span class="sxs-lookup"><span data-stu-id="318db-112">The maximum supported time range is 36 months.</span></span>  

|<span data-ttu-id="318db-113">Metoda</span><span class="sxs-lookup"><span data-stu-id="318db-113">Method</span></span> | <span data-ttu-id="318db-114">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="318db-114">Request URI</span></span>|
|-|-|
|<span data-ttu-id="318db-115">GET</span><span class="sxs-lookup"><span data-stu-id="318db-115">GET</span></span>|<span data-ttu-id="318db-116">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / usagedetails</span><span class="sxs-lookup"><span data-stu-id="318db-116">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/usagedetails</span></span> 
|<span data-ttu-id="318db-117">GET</span><span class="sxs-lookup"><span data-stu-id="318db-117">GET</span></span>|<span data-ttu-id="318db-118">/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / usagedetails</span><span class="sxs-lookup"><span data-stu-id="318db-118">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/usagedetails</span></span>|
|<span data-ttu-id="318db-119">GET</span><span class="sxs-lookup"><span data-stu-id="318db-119">GET</span></span>|<span data-ttu-id="318db-120">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / usagedetailsbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10</span><span class="sxs-lookup"><span data-stu-id="318db-120">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/usagedetailsbycustomdate?startTime=2017-01-01&endTime=2017-01-10</span></span>|

> [!Note]
> <span data-ttu-id="318db-121">Pokud chcete používat verzi preview rozhraní API, nahraďte v2 v1 v výše uvedenou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="318db-121">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="318db-122">Odpověď</span><span class="sxs-lookup"><span data-stu-id="318db-122">Response</span></span>

> <span data-ttu-id="318db-123">Z důvodu potenciálně velkého objemu dat výsledek je stránkovaného sady.</span><span class="sxs-lookup"><span data-stu-id="318db-123">Due to the potentially large volume of data the result set is paged.</span></span> <span data-ttu-id="318db-124">Pokud je k dispozici, určuje vlastnost nextLink odkaz na další stránku dat.</span><span class="sxs-lookup"><span data-stu-id="318db-124">The nextLink property, if present, specifies the link for the next page of data.</span></span> <span data-ttu-id="318db-125">Pokud na odkaz je prázdná, označuje to, že je poslední stránky.</span><span class="sxs-lookup"><span data-stu-id="318db-125">If the link is empty, it denotes that is the last page.</span></span> 
<br/>

    {
        "id": "string",
        "data": [
            {                       
            "accountId": 0,
            "productId": 0,
            "resourceLocationId": 0,
            "consumedServiceId": 0,
            "departmentId": 0,
            "accountOwnerEmail": "string",
            "accountName": "string",
            "serviceAdministratorId": "string",
            "subscriptionId": 0,
            "subscriptionGuid": "string",
            "subscriptionName": "string",
            "date": "2017-04-27T23:01:43.799Z",
            "product": "string",
            "meterId": "string",
            "meterCategory": "string",
            "meterSubCategory": "string",
            "meterRegion": "string",
            "meterName": "string",
            "consumedQuantity": 0,
            "resourceRate": 0,
            "Cost": 0,
            "resourceLocation": "string",
            "consumedService": "string",
            "instanceId": "string",
            "serviceInfo1": "string",
            "serviceInfo2": "string",
            "additionalInfo": "string",
            "tags": "string",
            "storeServiceIdentifier": "string",
            "departmentName": "string",
            "costCenter": "string",
            "unitOfMeasure": "string",
            "resourceGroup": "string"
            }
        ],
        "nextLink": "string"
    }

<br/><span data-ttu-id="318db-126">
**Definice vlastností odpovědi**</span><span class="sxs-lookup"><span data-stu-id="318db-126">
**Response property definitions**</span></span>

|<span data-ttu-id="318db-127">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="318db-127">Property Name</span></span>| <span data-ttu-id="318db-128">Typ</span><span class="sxs-lookup"><span data-stu-id="318db-128">Type</span></span>| <span data-ttu-id="318db-129">Popis</span><span class="sxs-lookup"><span data-stu-id="318db-129">Description</span></span>
|-|-|-|
|<span data-ttu-id="318db-130">id</span><span class="sxs-lookup"><span data-stu-id="318db-130">id</span></span>| <span data-ttu-id="318db-131">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-131">string</span></span>| <span data-ttu-id="318db-132">Jedinečné Id pro volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="318db-132">The unique Id for the API call.</span></span> |
|<span data-ttu-id="318db-133">data</span><span class="sxs-lookup"><span data-stu-id="318db-133">data</span></span>| <span data-ttu-id="318db-134">Pole JSON</span><span class="sxs-lookup"><span data-stu-id="318db-134">JSON array</span></span>| <span data-ttu-id="318db-135">Pole pro každý instance\meter denní podrobnosti o použití.</span><span class="sxs-lookup"><span data-stu-id="318db-135">The Array of daily usage details for every instance\meter.</span></span>|
|<span data-ttu-id="318db-136">odkaz nextLink</span><span class="sxs-lookup"><span data-stu-id="318db-136">nextLink</span></span>| <span data-ttu-id="318db-137">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-137">string</span></span>| <span data-ttu-id="318db-138">Pokud jsou dostupné další stránky dat odkaz nextLink odkazuje na URL pro návrat na další stránku data.</span><span class="sxs-lookup"><span data-stu-id="318db-138">When there are more pages of data the nextLink points to the URL to return the next page of data.</span></span> |
|<span data-ttu-id="318db-139">ID účtu</span><span class="sxs-lookup"><span data-stu-id="318db-139">accountId</span></span>| <span data-ttu-id="318db-140">celá čísla</span><span class="sxs-lookup"><span data-stu-id="318db-140">int</span></span>| <span data-ttu-id="318db-141">Zastaralé pole.</span><span class="sxs-lookup"><span data-stu-id="318db-141">Obsolete field.</span></span> <span data-ttu-id="318db-142">Dispozici z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="318db-142">Present for backward compatibility.</span></span> |
|<span data-ttu-id="318db-143">productId</span><span class="sxs-lookup"><span data-stu-id="318db-143">productId</span></span>| <span data-ttu-id="318db-144">celá čísla</span><span class="sxs-lookup"><span data-stu-id="318db-144">int</span></span>| <span data-ttu-id="318db-145">Zastaralé pole.</span><span class="sxs-lookup"><span data-stu-id="318db-145">Obsolete field.</span></span> <span data-ttu-id="318db-146">Dispozici z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="318db-146">Present for backward compatibility.</span></span> |
|<span data-ttu-id="318db-147">resourceLocationId</span><span class="sxs-lookup"><span data-stu-id="318db-147">resourceLocationId</span></span>| <span data-ttu-id="318db-148">celá čísla</span><span class="sxs-lookup"><span data-stu-id="318db-148">int</span></span>| <span data-ttu-id="318db-149">Zastaralé pole.</span><span class="sxs-lookup"><span data-stu-id="318db-149">Obsolete field.</span></span> <span data-ttu-id="318db-150">Dispozici z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="318db-150">Present for backward compatibility.</span></span> |
|<span data-ttu-id="318db-151">consumedServiceID</span><span class="sxs-lookup"><span data-stu-id="318db-151">consumedServiceID</span></span>| <span data-ttu-id="318db-152">celá čísla</span><span class="sxs-lookup"><span data-stu-id="318db-152">int</span></span>| <span data-ttu-id="318db-153">Zastaralé pole.</span><span class="sxs-lookup"><span data-stu-id="318db-153">Obsolete field.</span></span> <span data-ttu-id="318db-154">Dispozici z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="318db-154">Present for backward compatibility.</span></span> |
|<span data-ttu-id="318db-155">departmentId</span><span class="sxs-lookup"><span data-stu-id="318db-155">departmentId</span></span>| <span data-ttu-id="318db-156">celá čísla</span><span class="sxs-lookup"><span data-stu-id="318db-156">int</span></span>| <span data-ttu-id="318db-157">Zastaralé pole.</span><span class="sxs-lookup"><span data-stu-id="318db-157">Obsolete field.</span></span> <span data-ttu-id="318db-158">Dispozici z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="318db-158">Present for backward compatibility.</span></span> |
|<span data-ttu-id="318db-159">accountOwnerEmail</span><span class="sxs-lookup"><span data-stu-id="318db-159">accountOwnerEmail</span></span>| <span data-ttu-id="318db-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-160">string</span></span>| <span data-ttu-id="318db-161">E-mailový účet vlastníka účtu.</span><span class="sxs-lookup"><span data-stu-id="318db-161">Email account of the account owner.</span></span> |
|<span data-ttu-id="318db-162">název účtu</span><span class="sxs-lookup"><span data-stu-id="318db-162">accountName</span></span>| <span data-ttu-id="318db-163">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-163">string</span></span>| <span data-ttu-id="318db-164">Zákazník zadaný název účtu.</span><span class="sxs-lookup"><span data-stu-id="318db-164">Customer entered name of the account.</span></span> |
|<span data-ttu-id="318db-165">serviceAdministratorId</span><span class="sxs-lookup"><span data-stu-id="318db-165">serviceAdministratorId</span></span>| <span data-ttu-id="318db-166">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-166">string</span></span>| <span data-ttu-id="318db-167">E-mailovou adresu z Správce služeb.</span><span class="sxs-lookup"><span data-stu-id="318db-167">Email Address of Service Administrator.</span></span> |
|<span data-ttu-id="318db-168">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="318db-168">subscriptionId</span></span>| <span data-ttu-id="318db-169">celá čísla</span><span class="sxs-lookup"><span data-stu-id="318db-169">int</span></span>| <span data-ttu-id="318db-170">Zastaralé pole.</span><span class="sxs-lookup"><span data-stu-id="318db-170">Obsolete field.</span></span> <span data-ttu-id="318db-171">Dispozici z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="318db-171">Present for backward compatibility.</span></span> |
|<span data-ttu-id="318db-172">subscriptionGuid</span><span class="sxs-lookup"><span data-stu-id="318db-172">subscriptionGuid</span></span>| <span data-ttu-id="318db-173">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-173">string</span></span>| <span data-ttu-id="318db-174">Globální jedinečný identifikátor pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="318db-174">Global Unique Identifier for the subscription.</span></span> |
|<span data-ttu-id="318db-175">Název_předplatného</span><span class="sxs-lookup"><span data-stu-id="318db-175">subscriptionName</span></span>| <span data-ttu-id="318db-176">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-176">string</span></span>| <span data-ttu-id="318db-177">Název odběru.</span><span class="sxs-lookup"><span data-stu-id="318db-177">Name of the subscription.</span></span> |
|<span data-ttu-id="318db-178">Datum</span><span class="sxs-lookup"><span data-stu-id="318db-178">date</span></span>| <span data-ttu-id="318db-179">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-179">string</span></span>| <span data-ttu-id="318db-180">Datum, kdy došlo k využívání.</span><span class="sxs-lookup"><span data-stu-id="318db-180">The date on which consumption occurred.</span></span> |
|<span data-ttu-id="318db-181">Produktu</span><span class="sxs-lookup"><span data-stu-id="318db-181">product</span></span>| <span data-ttu-id="318db-182">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-182">string</span></span>| <span data-ttu-id="318db-183">Další informace o měřidla.</span><span class="sxs-lookup"><span data-stu-id="318db-183">Additional details on the meter.</span></span> <span data-ttu-id="318db-184">Příklad: A1 systému Windows (VM) - Asie a Tichomoří – východ</span><span class="sxs-lookup"><span data-stu-id="318db-184">Example: A1(VM)Windows - AP East</span></span>|
|<span data-ttu-id="318db-185">meterId</span><span class="sxs-lookup"><span data-stu-id="318db-185">meterId</span></span>| <span data-ttu-id="318db-186">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-186">string</span></span>| <span data-ttu-id="318db-187">Identifikátor pro monitorování, které vygenerované využití.</span><span class="sxs-lookup"><span data-stu-id="318db-187">The identifier for the meter which emitted usage.</span></span> |
|<span data-ttu-id="318db-188">meterCategory</span><span class="sxs-lookup"><span data-stu-id="318db-188">meterCategory</span></span>| <span data-ttu-id="318db-189">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-189">string</span></span>| <span data-ttu-id="318db-190">Služba platformy Azure, která byla použita.</span><span class="sxs-lookup"><span data-stu-id="318db-190">The Azure platform service that was used.</span></span> |
|<span data-ttu-id="318db-191">meterSubCategory</span><span class="sxs-lookup"><span data-stu-id="318db-191">meterSubCategory</span></span>| <span data-ttu-id="318db-192">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-192">string</span></span>| <span data-ttu-id="318db-193">Definuje typ služby Azure, který může mít vliv na rychlost.</span><span class="sxs-lookup"><span data-stu-id="318db-193">Defines the Azure service type that can affect the rate.</span></span> <span data-ttu-id="318db-194">Příklad: A1 virtuálních počítačů (jiný systém než Windows</span><span class="sxs-lookup"><span data-stu-id="318db-194">Example: A1 VM (Non-Windows</span></span>|
|<span data-ttu-id="318db-195">meterRegion</span><span class="sxs-lookup"><span data-stu-id="318db-195">meterRegion</span></span>| <span data-ttu-id="318db-196">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-196">string</span></span>| <span data-ttu-id="318db-197">Určuje polohu datového centra. U některých služeb vycházejí ceny z umístění datového centra.</span><span class="sxs-lookup"><span data-stu-id="318db-197">Identifies the location of the datacenter for certain services that are priced based on datacenter location.</span></span> |
|<span data-ttu-id="318db-198">meterName</span><span class="sxs-lookup"><span data-stu-id="318db-198">meterName</span></span>| <span data-ttu-id="318db-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-199">string</span></span>| <span data-ttu-id="318db-200">Název měřidla.</span><span class="sxs-lookup"><span data-stu-id="318db-200">Name of the meter.</span></span> |
|<span data-ttu-id="318db-201">consumedQuantity</span><span class="sxs-lookup"><span data-stu-id="318db-201">consumedQuantity</span></span>| <span data-ttu-id="318db-202">Double</span><span class="sxs-lookup"><span data-stu-id="318db-202">double</span></span>| <span data-ttu-id="318db-203">Množství monitorování, které bylo spotřebováno.</span><span class="sxs-lookup"><span data-stu-id="318db-203">The amount of the meter that has been consumed.</span></span> |
|<span data-ttu-id="318db-204">resourceRate</span><span class="sxs-lookup"><span data-stu-id="318db-204">resourceRate</span></span>| <span data-ttu-id="318db-205">Double</span><span class="sxs-lookup"><span data-stu-id="318db-205">double</span></span>| <span data-ttu-id="318db-206">Míra použít na fakturovatelný jednotku.</span><span class="sxs-lookup"><span data-stu-id="318db-206">The rate applicable per billable unit.</span></span> |
|<span data-ttu-id="318db-207">Náklady</span><span class="sxs-lookup"><span data-stu-id="318db-207">cost</span></span>| <span data-ttu-id="318db-208">Double</span><span class="sxs-lookup"><span data-stu-id="318db-208">double</span></span>| <span data-ttu-id="318db-209">Poplatků, které vznikly pro měřidla.</span><span class="sxs-lookup"><span data-stu-id="318db-209">The charge that has been incurred for the meter.</span></span> |
|<span data-ttu-id="318db-210">resourceLocation</span><span class="sxs-lookup"><span data-stu-id="318db-210">resourceLocation</span></span>| <span data-ttu-id="318db-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-211">string</span></span>| <span data-ttu-id="318db-212">Identifikuje datacenter, kde je spuštěna měřidla.</span><span class="sxs-lookup"><span data-stu-id="318db-212">Identifies the datacenter where the meter is running.</span></span> |
|<span data-ttu-id="318db-213">consumedService</span><span class="sxs-lookup"><span data-stu-id="318db-213">consumedService</span></span>| <span data-ttu-id="318db-214">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-214">string</span></span>| <span data-ttu-id="318db-215">Služba platformy Azure, která byla použita.</span><span class="sxs-lookup"><span data-stu-id="318db-215">The Azure platform service that was used.</span></span> |
|<span data-ttu-id="318db-216">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="318db-216">instanceId</span></span>| <span data-ttu-id="318db-217">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-217">string</span></span>| <span data-ttu-id="318db-218">Tento identifikátor je název prostředku nebo plně kvalifikovaný ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="318db-218">This identifier is the name of the resource or the fully qualified Resource ID.</span></span> <span data-ttu-id="318db-219">Další informace najdete v tématu [rozhraní API služby Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources)</span><span class="sxs-lookup"><span data-stu-id="318db-219">For more information, see [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources)</span></span> |
|<span data-ttu-id="318db-220">serviceInfo1</span><span class="sxs-lookup"><span data-stu-id="318db-220">serviceInfo1</span></span>| <span data-ttu-id="318db-221">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-221">string</span></span>| <span data-ttu-id="318db-222">Metadata pro interní služby Azure.</span><span class="sxs-lookup"><span data-stu-id="318db-222">Internal Azure Service Metadata.</span></span> |
|<span data-ttu-id="318db-223">serviceInfo2</span><span class="sxs-lookup"><span data-stu-id="318db-223">serviceInfo2</span></span>| <span data-ttu-id="318db-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-224">string</span></span>| <span data-ttu-id="318db-225">Například typ image pro virtuální počítač a název poskytovatele internetových služeb pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="318db-225">For example, an image type for a virtual machine and ISP name for ExpressRoute.</span></span> |
|<span data-ttu-id="318db-226">additionalInfo</span><span class="sxs-lookup"><span data-stu-id="318db-226">additionalInfo</span></span>| <span data-ttu-id="318db-227">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-227">string</span></span>| <span data-ttu-id="318db-228">Metadata specifická pro služby.</span><span class="sxs-lookup"><span data-stu-id="318db-228">Service-specific metadata.</span></span> <span data-ttu-id="318db-229">Například bitové kopie typ pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="318db-229">For example, an image type for a virtual machine.</span></span> |
|<span data-ttu-id="318db-230">tags</span><span class="sxs-lookup"><span data-stu-id="318db-230">tags</span></span>| <span data-ttu-id="318db-231">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-231">string</span></span>| <span data-ttu-id="318db-232">Zákazník přidat značky.</span><span class="sxs-lookup"><span data-stu-id="318db-232">Customer added tags.</span></span> <span data-ttu-id="318db-233">Další informace najdete v tématu [uspořádání prostředků Azure pomocí značek](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags).</span><span class="sxs-lookup"><span data-stu-id="318db-233">For more information, see [Organize your Azure resources with tags](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags).</span></span> |
|<span data-ttu-id="318db-234">storeServiceIdentifier</span><span class="sxs-lookup"><span data-stu-id="318db-234">storeServiceIdentifier</span></span>| <span data-ttu-id="318db-235">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-235">string</span></span>| <span data-ttu-id="318db-236">Tento sloupce se nepoužije.</span><span class="sxs-lookup"><span data-stu-id="318db-236">This columns is not used.</span></span> <span data-ttu-id="318db-237">Dispozici z důvodu zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="318db-237">Present for backward compatibility.</span></span> |
|<span data-ttu-id="318db-238">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="318db-238">departmentName</span></span>| <span data-ttu-id="318db-239">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-239">string</span></span>| <span data-ttu-id="318db-240">Název oddělení.</span><span class="sxs-lookup"><span data-stu-id="318db-240">Name of the department.</span></span> |
|<span data-ttu-id="318db-241">CostCenter</span><span class="sxs-lookup"><span data-stu-id="318db-241">costCenter</span></span>| <span data-ttu-id="318db-242">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-242">string</span></span>| <span data-ttu-id="318db-243">Nákladové středisko, které je přidružené využití.</span><span class="sxs-lookup"><span data-stu-id="318db-243">The cost center that the usage is associated with.</span></span> |
|<span data-ttu-id="318db-244">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="318db-244">unitOfMeasure</span></span>| <span data-ttu-id="318db-245">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-245">string</span></span>| <span data-ttu-id="318db-246">Určuje jednotku, která služba je účtován v.</span><span class="sxs-lookup"><span data-stu-id="318db-246">Identifies the unit that the service is charged in.</span></span> <span data-ttu-id="318db-247">Příklad: GB, hodin, 10 000 s.</span><span class="sxs-lookup"><span data-stu-id="318db-247">Example: GB, hours, 10,000 s.</span></span> |
|<span data-ttu-id="318db-248">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="318db-248">resourceGroup</span></span>| <span data-ttu-id="318db-249">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318db-249">string</span></span>| <span data-ttu-id="318db-250">Skupinu prostředků, ve které je nasazený měření spuštěný v.</span><span class="sxs-lookup"><span data-stu-id="318db-250">The resource group in which the deployed meter is running in.</span></span> <span data-ttu-id="318db-251">Další informace naleznete v tématu [Přehled Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="318db-251">For more information, see [Azure Resource Manager overview](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span></span> |
<br/>
## <a name="see-also"></a><span data-ttu-id="318db-252">Viz také</span><span class="sxs-lookup"><span data-stu-id="318db-252">See also</span></span>

* [<span data-ttu-id="318db-253">Fakturační období rozhraní API</span><span class="sxs-lookup"><span data-stu-id="318db-253">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="318db-254">Vyrovnávat a souhrn rozhraní API</span><span class="sxs-lookup"><span data-stu-id="318db-254">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="318db-255">Úložiště Marketplace poplatků rozhraní API</span><span class="sxs-lookup"><span data-stu-id="318db-255">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="318db-256">Rozhraní API list cena</span><span class="sxs-lookup"><span data-stu-id="318db-256">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)