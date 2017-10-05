---
title: "Azure fakturace rozhraní API Enterprise - poplatky Marketplace | Microsoft Docs"
description: "Další informace o rozhraních API vytváření sestav, které umožňují zákazníkům Enterprise Azure a si vyžádá data spotřeby prostřednictvím kódu programu."
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
ms.openlocfilehash: 5539623f7ae35e14b6dafe6fdf9efe4bcaba4fd3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a><span data-ttu-id="78014-103">Generování sestav rozhraní API pro podnikové zákazníky - poplatků úložiště Marketplace.</span><span class="sxs-lookup"><span data-stu-id="78014-103">Reporting APIs for Enterprise customers - Marketplace Store Charge</span></span>

<span data-ttu-id="78014-104">Rozhraní API Marketplace úložiště poplatků vrátí rozpis poplatky na základě využití marketplace podle den v zadaném období fakturace nebo počáteční a koncové datum (jednou poplatky nejsou součástí).</span><span class="sxs-lookup"><span data-stu-id="78014-104">The Marketplace Store Charge API returns the usage-based marketplace charges breakdown by day for the specified Billing Period or start and end dates (one time fees are not included).</span></span>

##<a name="request"></a><span data-ttu-id="78014-105">Žádost</span><span class="sxs-lookup"><span data-stu-id="78014-105">Request</span></span> 
<span data-ttu-id="78014-106">Nejsou zadány společných vlastností hlavičky, které je třeba přidat [zde](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="78014-106">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="78014-107">Pokud není zadán fakturační období, je vrácena data pro aktuální fakturační období.</span><span class="sxs-lookup"><span data-stu-id="78014-107">If a billing period is not specified, then data for the current billing period is returned.</span></span> <span data-ttu-id="78014-108">Vlastní časových rozsahů lze určit spuštění a ukončení datum parametry, které jsou ve formátu RRRR MM-dd, maximální podporovaná časový rozsah je 36 měsíců.</span><span class="sxs-lookup"><span data-stu-id="78014-108">Custom time ranges can be specified with the start and end date parameters that are in the format yyyy-MM-dd, the maximum supported time range is 36 months.</span></span>  

|<span data-ttu-id="78014-109">Metoda</span><span class="sxs-lookup"><span data-stu-id="78014-109">Method</span></span> | <span data-ttu-id="78014-110">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="78014-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="78014-111">GET</span><span class="sxs-lookup"><span data-stu-id="78014-111">GET</span></span>|<span data-ttu-id="78014-112">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="78014-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacecharges</span></span>|
|<span data-ttu-id="78014-113">GET</span><span class="sxs-lookup"><span data-stu-id="78014-113">GET</span></span>|<span data-ttu-id="78014-114">/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="78014-114">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/marketplacecharges</span></span>|
|<span data-ttu-id="78014-115">GET</span><span class="sxs-lookup"><span data-stu-id="78014-115">GET</span></span>|<span data-ttu-id="78014-116">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10</span><span class="sxs-lookup"><span data-stu-id="78014-116">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacechargesbycustomdate?startTime=2017-01-01&endTime=2017-01-10</span></span>|

> [!Note]
> <span data-ttu-id="78014-117">Pokud chcete používat verzi preview rozhraní API, nahraďte v2 v1 v výše uvedenou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="78014-117">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="78014-118">Odpověď</span><span class="sxs-lookup"><span data-stu-id="78014-118">Response</span></span>
 
    
        [
            {
                "id": "id",
                "subscriptionGuid": "00000000-0000-0000-0000-000000000000",
                "subscriptionName": "subName",
                "meterId": "2core",
                "usageStartDate": "2015-09-17T00:00:00Z",
                "usageEndDate": "2015-09-17T23:59:59Z",
                "offerName": "Virtual LoadMaster™ (VLM) for Azure",
                "resourceGroup": "Res group",
                "instanceId": "id",
                "additionalInfo": "{\"ImageType\":null,\"ServiceType\":\"Medium\"}",
                "tags": "",
                "orderNumber": "order",
                "unitOfMeasure": "",
                "costCenter": "100",
                "accountId": 100,
                "accountName": "Account Name",
                "accountOwnerId": "account@live.com",
                "departmentId": 101,
                "departmentName": "Department 1",
                "publisherName": "Publisher 1",
                "planName": "Plan name",
                "consumedQuantity": 1.15,
                "resourceRate": 0.1,
                "extendedCost": 1.11
            },
            ...
        ]
    

<span data-ttu-id="78014-119">**Definice vlastností odpovědi**</span><span class="sxs-lookup"><span data-stu-id="78014-119">**Response property definitions**</span></span>

|<span data-ttu-id="78014-120">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="78014-120">Property Name</span></span>| <span data-ttu-id="78014-121">Typ</span><span class="sxs-lookup"><span data-stu-id="78014-121">Type</span></span>| <span data-ttu-id="78014-122">Popis</span><span class="sxs-lookup"><span data-stu-id="78014-122">Description</span></span>
|-|-|-|
|<span data-ttu-id="78014-123">id</span><span class="sxs-lookup"><span data-stu-id="78014-123">id</span></span>|<span data-ttu-id="78014-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-124">string</span></span>|<span data-ttu-id="78014-125">Jedinečné Id pro položku poplatků marketplace.</span><span class="sxs-lookup"><span data-stu-id="78014-125">Unique Id for the marketplace charge item</span></span>|
|<span data-ttu-id="78014-126">subscriptionGuid</span><span class="sxs-lookup"><span data-stu-id="78014-126">subscriptionGuid</span></span>|<span data-ttu-id="78014-127">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="78014-127">Guid</span></span>|<span data-ttu-id="78014-128">Guid předplatného</span><span class="sxs-lookup"><span data-stu-id="78014-128">The Subscription Guid</span></span>|
|<span data-ttu-id="78014-129">Název_předplatného</span><span class="sxs-lookup"><span data-stu-id="78014-129">subscriptionName</span></span>|<span data-ttu-id="78014-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-130">string</span></span>|<span data-ttu-id="78014-131">Název odběru</span><span class="sxs-lookup"><span data-stu-id="78014-131">The Subscription Name</span></span>|
|<span data-ttu-id="78014-132">meterId</span><span class="sxs-lookup"><span data-stu-id="78014-132">meterId</span></span>|<span data-ttu-id="78014-133">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-133">string</span></span>|<span data-ttu-id="78014-134">ID pro emitovaného měření</span><span class="sxs-lookup"><span data-stu-id="78014-134">Id for the emitted Meter</span></span>|
|<span data-ttu-id="78014-135">usageStartDate</span><span class="sxs-lookup"><span data-stu-id="78014-135">usageStartDate</span></span>|<span data-ttu-id="78014-136">Data a času</span><span class="sxs-lookup"><span data-stu-id="78014-136">DateTime</span></span>|<span data-ttu-id="78014-137">Počáteční čas pro záznam využití</span><span class="sxs-lookup"><span data-stu-id="78014-137">Start time for the usage record</span></span>|
|<span data-ttu-id="78014-138">usageEndDate</span><span class="sxs-lookup"><span data-stu-id="78014-138">usageEndDate</span></span>|<span data-ttu-id="78014-139">Data a času</span><span class="sxs-lookup"><span data-stu-id="78014-139">DateTime</span></span>|<span data-ttu-id="78014-140">Koncový čas pro záznam využití</span><span class="sxs-lookup"><span data-stu-id="78014-140">End time for the usage record</span></span>|
|<span data-ttu-id="78014-141">offerName</span><span class="sxs-lookup"><span data-stu-id="78014-141">offerName</span></span>|<span data-ttu-id="78014-142">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-142">string</span></span>|<span data-ttu-id="78014-143">Název nabídky</span><span class="sxs-lookup"><span data-stu-id="78014-143">The Offer name</span></span>|
|<span data-ttu-id="78014-144">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="78014-144">resourceGroup</span></span>|<span data-ttu-id="78014-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-145">string</span></span>|<span data-ttu-id="78014-146">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="78014-146">The resource Group</span></span>|
|<span data-ttu-id="78014-147">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="78014-147">instanceId</span></span>|<span data-ttu-id="78014-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-148">string</span></span>|<span data-ttu-id="78014-149">Instance Id</span><span class="sxs-lookup"><span data-stu-id="78014-149">Instance Id</span></span>|
|<span data-ttu-id="78014-150">additionalInfo</span><span class="sxs-lookup"><span data-stu-id="78014-150">additionalInfo</span></span>|<span data-ttu-id="78014-151">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-151">string</span></span>|<span data-ttu-id="78014-152">Další informace o řetězce formátu JSON</span><span class="sxs-lookup"><span data-stu-id="78014-152">Additional info JSON string</span></span>|
|<span data-ttu-id="78014-153">tags</span><span class="sxs-lookup"><span data-stu-id="78014-153">tags</span></span>|<span data-ttu-id="78014-154">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-154">string</span></span>|<span data-ttu-id="78014-155">Řetězce formátu JSON značky</span><span class="sxs-lookup"><span data-stu-id="78014-155">Tag JSON string</span></span>|
|<span data-ttu-id="78014-156">orderNumber</span><span class="sxs-lookup"><span data-stu-id="78014-156">orderNumber</span></span>|<span data-ttu-id="78014-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-157">string</span></span>|<span data-ttu-id="78014-158">Pořadové číslo</span><span class="sxs-lookup"><span data-stu-id="78014-158">The order number</span></span>|
|<span data-ttu-id="78014-159">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="78014-159">unitOfMeasure</span></span>|<span data-ttu-id="78014-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-160">string</span></span>|<span data-ttu-id="78014-161">Měrné jednotky pro měřidla</span><span class="sxs-lookup"><span data-stu-id="78014-161">Unit of measure for the meter</span></span>|
|<span data-ttu-id="78014-162">CostCenter</span><span class="sxs-lookup"><span data-stu-id="78014-162">costCenter</span></span>|<span data-ttu-id="78014-163">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-163">string</span></span>|<span data-ttu-id="78014-164">Nákladové středisko</span><span class="sxs-lookup"><span data-stu-id="78014-164">The cost center</span></span>|
|<span data-ttu-id="78014-165">ID účtu</span><span class="sxs-lookup"><span data-stu-id="78014-165">accountId</span></span>|<span data-ttu-id="78014-166">celá čísla</span><span class="sxs-lookup"><span data-stu-id="78014-166">int</span></span>|<span data-ttu-id="78014-167">Id účtu</span><span class="sxs-lookup"><span data-stu-id="78014-167">The account Id</span></span>|
|<span data-ttu-id="78014-168">název účtu</span><span class="sxs-lookup"><span data-stu-id="78014-168">accountName</span></span>|<span data-ttu-id="78014-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-169">string</span></span> |<span data-ttu-id="78014-170">Název účtu</span><span class="sxs-lookup"><span data-stu-id="78014-170">The Account Name</span></span>|
|<span data-ttu-id="78014-171">accountOwnerId</span><span class="sxs-lookup"><span data-stu-id="78014-171">accountOwnerId</span></span>|<span data-ttu-id="78014-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-172">string</span></span>|<span data-ttu-id="78014-173">Id vlastníka účtu</span><span class="sxs-lookup"><span data-stu-id="78014-173">The Account Owner Id</span></span>|
|<span data-ttu-id="78014-174">departmentId</span><span class="sxs-lookup"><span data-stu-id="78014-174">departmentId</span></span>|<span data-ttu-id="78014-175">celá čísla</span><span class="sxs-lookup"><span data-stu-id="78014-175">int</span></span>|<span data-ttu-id="78014-176">Oddělení Id</span><span class="sxs-lookup"><span data-stu-id="78014-176">The department Id</span></span>|
|<span data-ttu-id="78014-177">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="78014-177">departmentName</span></span>|<span data-ttu-id="78014-178">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-178">string</span></span>|<span data-ttu-id="78014-179">Název oddělení</span><span class="sxs-lookup"><span data-stu-id="78014-179">The department name</span></span>|
|<span data-ttu-id="78014-180">publisherName</span><span class="sxs-lookup"><span data-stu-id="78014-180">publisherName</span></span>|<span data-ttu-id="78014-181">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-181">string</span></span>|<span data-ttu-id="78014-182">Název vydavatele</span><span class="sxs-lookup"><span data-stu-id="78014-182">The publisher name</span></span>|
|<span data-ttu-id="78014-183">planName</span><span class="sxs-lookup"><span data-stu-id="78014-183">planName</span></span>|<span data-ttu-id="78014-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="78014-184">string</span></span>|<span data-ttu-id="78014-185">Název plánu</span><span class="sxs-lookup"><span data-stu-id="78014-185">The Plan name</span></span>|
|<span data-ttu-id="78014-186">consumedQuantity</span><span class="sxs-lookup"><span data-stu-id="78014-186">consumedQuantity</span></span>|<span data-ttu-id="78014-187">Decimal</span><span class="sxs-lookup"><span data-stu-id="78014-187">decimal</span></span>|<span data-ttu-id="78014-188">Spotřebované množství během tohoto období</span><span class="sxs-lookup"><span data-stu-id="78014-188">Consumed Quantity during this time period</span></span>|
|<span data-ttu-id="78014-189">resourceRate</span><span class="sxs-lookup"><span data-stu-id="78014-189">resourceRate</span></span>|<span data-ttu-id="78014-190">Decimal</span><span class="sxs-lookup"><span data-stu-id="78014-190">decimal</span></span>|<span data-ttu-id="78014-191">Jednotkové ceny pro měřidla</span><span class="sxs-lookup"><span data-stu-id="78014-191">Unit price for the meter</span></span>|
|<span data-ttu-id="78014-192">extendedCost</span><span class="sxs-lookup"><span data-stu-id="78014-192">extendedCost</span></span>|<span data-ttu-id="78014-193">Decimal</span><span class="sxs-lookup"><span data-stu-id="78014-193">decimal</span></span>|<span data-ttu-id="78014-194">Odhadované poplatků na základě objemu spotřebovaného a rozšířené náklady</span><span class="sxs-lookup"><span data-stu-id="78014-194">Estimated charge based on Consumed Quantity and Extended cost</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="78014-195">Viz také</span><span class="sxs-lookup"><span data-stu-id="78014-195">See also</span></span>

* [<span data-ttu-id="78014-196">Fakturační období rozhraní API</span><span class="sxs-lookup"><span data-stu-id="78014-196">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="78014-197">Podrobnosti o použití rozhraní API</span><span class="sxs-lookup"><span data-stu-id="78014-197">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="78014-198">Vyrovnávat a souhrn rozhraní API</span><span class="sxs-lookup"><span data-stu-id="78014-198">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="78014-199">Rozhraní API list cena</span><span class="sxs-lookup"><span data-stu-id="78014-199">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)