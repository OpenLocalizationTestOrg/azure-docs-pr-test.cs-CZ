---
title: "aaaAzure fakturace Enterprise rozhraní API - poplatky Marketplace | Microsoft Docs"
description: "Další informace o vytváření sestav rozhraní API, která umožňují Enterprise Azure zákazníků toopull spotřeby dat prostřednictvím kódu programu hello."
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
ms.openlocfilehash: cdf2836b52df06a4bf5ed71a476fe33662c5363c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a><span data-ttu-id="894b9-103">Generování sestav rozhraní API pro podnikové zákazníky - poplatků úložiště Marketplace.</span><span class="sxs-lookup"><span data-stu-id="894b9-103">Reporting APIs for Enterprise customers - Marketplace Store Charge</span></span>

<span data-ttu-id="894b9-104">Hello Marketplace úložiště poplatků API vrátí hello na základě využití marketplace poplatky rozpis podle dne pro hello zadané fakturační období nebo počáteční a koncové datum (jednou poplatky nejsou součástí).</span><span class="sxs-lookup"><span data-stu-id="894b9-104">hello Marketplace Store Charge API returns hello usage-based marketplace charges breakdown by day for hello specified Billing Period or start and end dates (one time fees are not included).</span></span>

##<a name="request"></a><span data-ttu-id="894b9-105">Žádost</span><span class="sxs-lookup"><span data-stu-id="894b9-105">Request</span></span> 
<span data-ttu-id="894b9-106">Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="894b9-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="894b9-107">Pokud není zadán fakturační období, pak data pro aktuální fakturaci hello období vrácena.</span><span class="sxs-lookup"><span data-stu-id="894b9-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span> <span data-ttu-id="894b9-108">Vlastní čas rozsahy lze určit hello počáteční a koncovou datum parametry, které jsou v hello formát rrrr MM-dd hello maximální podporované, když je rozsah 36 měsíců.</span><span class="sxs-lookup"><span data-stu-id="894b9-108">Custom time ranges can be specified with hello start and end date parameters that are in hello format yyyy-MM-dd, hello maximum supported time range is 36 months.</span></span>  

|<span data-ttu-id="894b9-109">Metoda</span><span class="sxs-lookup"><span data-stu-id="894b9-109">Method</span></span> | <span data-ttu-id="894b9-110">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="894b9-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="894b9-111">GET</span><span class="sxs-lookup"><span data-stu-id="894b9-111">GET</span></span>|<span data-ttu-id="894b9-112">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="894b9-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacecharges</span></span>|
|<span data-ttu-id="894b9-113">GET</span><span class="sxs-lookup"><span data-stu-id="894b9-113">GET</span></span>|<span data-ttu-id="894b9-114">/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="894b9-114">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/marketplacecharges</span></span>|
|<span data-ttu-id="894b9-115">GET</span><span class="sxs-lookup"><span data-stu-id="894b9-115">GET</span></span>|<span data-ttu-id="894b9-116">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10</span><span class="sxs-lookup"><span data-stu-id="894b9-116">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacechargesbycustomdate?startTime=2017-01-01&endTime=2017-01-10</span></span>|

> [!Note]
> <span data-ttu-id="894b9-117">verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.</span><span class="sxs-lookup"><span data-stu-id="894b9-117">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="894b9-118">Odpověď</span><span class="sxs-lookup"><span data-stu-id="894b9-118">Response</span></span>
 
    
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
    

<span data-ttu-id="894b9-119">**Definice vlastností odpovědi**</span><span class="sxs-lookup"><span data-stu-id="894b9-119">**Response property definitions**</span></span>

|<span data-ttu-id="894b9-120">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="894b9-120">Property Name</span></span>| <span data-ttu-id="894b9-121">Typ</span><span class="sxs-lookup"><span data-stu-id="894b9-121">Type</span></span>| <span data-ttu-id="894b9-122">Popis</span><span class="sxs-lookup"><span data-stu-id="894b9-122">Description</span></span>
|-|-|-|
|<span data-ttu-id="894b9-123">id</span><span class="sxs-lookup"><span data-stu-id="894b9-123">id</span></span>|<span data-ttu-id="894b9-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-124">string</span></span>|<span data-ttu-id="894b9-125">Jedinečné Id pro položku poplatků hello marketplace.</span><span class="sxs-lookup"><span data-stu-id="894b9-125">Unique Id for hello marketplace charge item</span></span>|
|<span data-ttu-id="894b9-126">subscriptionGuid</span><span class="sxs-lookup"><span data-stu-id="894b9-126">subscriptionGuid</span></span>|<span data-ttu-id="894b9-127">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="894b9-127">Guid</span></span>|<span data-ttu-id="894b9-128">Hello Guid předplatného</span><span class="sxs-lookup"><span data-stu-id="894b9-128">hello Subscription Guid</span></span>|
|<span data-ttu-id="894b9-129">Název_předplatného</span><span class="sxs-lookup"><span data-stu-id="894b9-129">subscriptionName</span></span>|<span data-ttu-id="894b9-130">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-130">string</span></span>|<span data-ttu-id="894b9-131">Hello název odběru</span><span class="sxs-lookup"><span data-stu-id="894b9-131">hello Subscription Name</span></span>|
|<span data-ttu-id="894b9-132">meterId</span><span class="sxs-lookup"><span data-stu-id="894b9-132">meterId</span></span>|<span data-ttu-id="894b9-133">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-133">string</span></span>|<span data-ttu-id="894b9-134">ID pro hello vygenerované měření</span><span class="sxs-lookup"><span data-stu-id="894b9-134">Id for hello emitted Meter</span></span>|
|<span data-ttu-id="894b9-135">usageStartDate</span><span class="sxs-lookup"><span data-stu-id="894b9-135">usageStartDate</span></span>|<span data-ttu-id="894b9-136">Data a času</span><span class="sxs-lookup"><span data-stu-id="894b9-136">DateTime</span></span>|<span data-ttu-id="894b9-137">Počáteční čas pro záznam využití hello</span><span class="sxs-lookup"><span data-stu-id="894b9-137">Start time for hello usage record</span></span>|
|<span data-ttu-id="894b9-138">usageEndDate</span><span class="sxs-lookup"><span data-stu-id="894b9-138">usageEndDate</span></span>|<span data-ttu-id="894b9-139">Data a času</span><span class="sxs-lookup"><span data-stu-id="894b9-139">DateTime</span></span>|<span data-ttu-id="894b9-140">Koncový čas pro záznam využití hello</span><span class="sxs-lookup"><span data-stu-id="894b9-140">End time for hello usage record</span></span>|
|<span data-ttu-id="894b9-141">offerName</span><span class="sxs-lookup"><span data-stu-id="894b9-141">offerName</span></span>|<span data-ttu-id="894b9-142">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-142">string</span></span>|<span data-ttu-id="894b9-143">Název nabídky Hello</span><span class="sxs-lookup"><span data-stu-id="894b9-143">hello Offer name</span></span>|
|<span data-ttu-id="894b9-144">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="894b9-144">resourceGroup</span></span>|<span data-ttu-id="894b9-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-145">string</span></span>|<span data-ttu-id="894b9-146">Hello prostředků skupiny</span><span class="sxs-lookup"><span data-stu-id="894b9-146">hello resource Group</span></span>|
|<span data-ttu-id="894b9-147">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="894b9-147">instanceId</span></span>|<span data-ttu-id="894b9-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-148">string</span></span>|<span data-ttu-id="894b9-149">Instance Id</span><span class="sxs-lookup"><span data-stu-id="894b9-149">Instance Id</span></span>|
|<span data-ttu-id="894b9-150">additionalInfo</span><span class="sxs-lookup"><span data-stu-id="894b9-150">additionalInfo</span></span>|<span data-ttu-id="894b9-151">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-151">string</span></span>|<span data-ttu-id="894b9-152">Další informace o řetězce formátu JSON</span><span class="sxs-lookup"><span data-stu-id="894b9-152">Additional info JSON string</span></span>|
|<span data-ttu-id="894b9-153">tags</span><span class="sxs-lookup"><span data-stu-id="894b9-153">tags</span></span>|<span data-ttu-id="894b9-154">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-154">string</span></span>|<span data-ttu-id="894b9-155">Řetězce formátu JSON značky</span><span class="sxs-lookup"><span data-stu-id="894b9-155">Tag JSON string</span></span>|
|<span data-ttu-id="894b9-156">orderNumber</span><span class="sxs-lookup"><span data-stu-id="894b9-156">orderNumber</span></span>|<span data-ttu-id="894b9-157">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-157">string</span></span>|<span data-ttu-id="894b9-158">číslo objednávky Hello</span><span class="sxs-lookup"><span data-stu-id="894b9-158">hello order number</span></span>|
|<span data-ttu-id="894b9-159">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="894b9-159">unitOfMeasure</span></span>|<span data-ttu-id="894b9-160">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-160">string</span></span>|<span data-ttu-id="894b9-161">Měrné jednotky pro měření hello</span><span class="sxs-lookup"><span data-stu-id="894b9-161">Unit of measure for hello meter</span></span>|
|<span data-ttu-id="894b9-162">CostCenter</span><span class="sxs-lookup"><span data-stu-id="894b9-162">costCenter</span></span>|<span data-ttu-id="894b9-163">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-163">string</span></span>|<span data-ttu-id="894b9-164">center Hello náklady.</span><span class="sxs-lookup"><span data-stu-id="894b9-164">hello cost center</span></span>|
|<span data-ttu-id="894b9-165">ID účtu</span><span class="sxs-lookup"><span data-stu-id="894b9-165">accountId</span></span>|<span data-ttu-id="894b9-166">celá čísla</span><span class="sxs-lookup"><span data-stu-id="894b9-166">int</span></span>|<span data-ttu-id="894b9-167">Id účtu Hello</span><span class="sxs-lookup"><span data-stu-id="894b9-167">hello account Id</span></span>|
|<span data-ttu-id="894b9-168">název účtu</span><span class="sxs-lookup"><span data-stu-id="894b9-168">accountName</span></span>|<span data-ttu-id="894b9-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-169">string</span></span> |<span data-ttu-id="894b9-170">Hello název účtu</span><span class="sxs-lookup"><span data-stu-id="894b9-170">hello Account Name</span></span>|
|<span data-ttu-id="894b9-171">accountOwnerId</span><span class="sxs-lookup"><span data-stu-id="894b9-171">accountOwnerId</span></span>|<span data-ttu-id="894b9-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-172">string</span></span>|<span data-ttu-id="894b9-173">Hello Id vlastníka účtu</span><span class="sxs-lookup"><span data-stu-id="894b9-173">hello Account Owner Id</span></span>|
|<span data-ttu-id="894b9-174">departmentId</span><span class="sxs-lookup"><span data-stu-id="894b9-174">departmentId</span></span>|<span data-ttu-id="894b9-175">celá čísla</span><span class="sxs-lookup"><span data-stu-id="894b9-175">int</span></span>|<span data-ttu-id="894b9-176">oddělení Hello Id</span><span class="sxs-lookup"><span data-stu-id="894b9-176">hello department Id</span></span>|
|<span data-ttu-id="894b9-177">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="894b9-177">departmentName</span></span>|<span data-ttu-id="894b9-178">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-178">string</span></span>|<span data-ttu-id="894b9-179">název oddělení Hello</span><span class="sxs-lookup"><span data-stu-id="894b9-179">hello department name</span></span>|
|<span data-ttu-id="894b9-180">publisherName</span><span class="sxs-lookup"><span data-stu-id="894b9-180">publisherName</span></span>|<span data-ttu-id="894b9-181">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-181">string</span></span>|<span data-ttu-id="894b9-182">Název vydavatele Hello</span><span class="sxs-lookup"><span data-stu-id="894b9-182">hello publisher name</span></span>|
|<span data-ttu-id="894b9-183">planName</span><span class="sxs-lookup"><span data-stu-id="894b9-183">planName</span></span>|<span data-ttu-id="894b9-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="894b9-184">string</span></span>|<span data-ttu-id="894b9-185">Název plánu Hello</span><span class="sxs-lookup"><span data-stu-id="894b9-185">hello Plan name</span></span>|
|<span data-ttu-id="894b9-186">consumedQuantity</span><span class="sxs-lookup"><span data-stu-id="894b9-186">consumedQuantity</span></span>|<span data-ttu-id="894b9-187">Decimal</span><span class="sxs-lookup"><span data-stu-id="894b9-187">decimal</span></span>|<span data-ttu-id="894b9-188">Spotřebované množství během tohoto období</span><span class="sxs-lookup"><span data-stu-id="894b9-188">Consumed Quantity during this time period</span></span>|
|<span data-ttu-id="894b9-189">resourceRate</span><span class="sxs-lookup"><span data-stu-id="894b9-189">resourceRate</span></span>|<span data-ttu-id="894b9-190">Decimal</span><span class="sxs-lookup"><span data-stu-id="894b9-190">decimal</span></span>|<span data-ttu-id="894b9-191">Jednotkové ceny pro měření hello</span><span class="sxs-lookup"><span data-stu-id="894b9-191">Unit price for hello meter</span></span>|
|<span data-ttu-id="894b9-192">extendedCost</span><span class="sxs-lookup"><span data-stu-id="894b9-192">extendedCost</span></span>|<span data-ttu-id="894b9-193">Decimal</span><span class="sxs-lookup"><span data-stu-id="894b9-193">decimal</span></span>|<span data-ttu-id="894b9-194">Odhadované poplatků na základě objemu spotřebovaného a rozšířené náklady</span><span class="sxs-lookup"><span data-stu-id="894b9-194">Estimated charge based on Consumed Quantity and Extended cost</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="894b9-195">Viz také</span><span class="sxs-lookup"><span data-stu-id="894b9-195">See also</span></span>

* [<span data-ttu-id="894b9-196">Fakturační období rozhraní API</span><span class="sxs-lookup"><span data-stu-id="894b9-196">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="894b9-197">Podrobnosti o použití rozhraní API</span><span class="sxs-lookup"><span data-stu-id="894b9-197">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="894b9-198">Vyrovnávat a souhrn rozhraní API</span><span class="sxs-lookup"><span data-stu-id="894b9-198">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="894b9-199">Rozhraní API list cena</span><span class="sxs-lookup"><span data-stu-id="894b9-199">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)