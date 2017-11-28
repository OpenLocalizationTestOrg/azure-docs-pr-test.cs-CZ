---
title: "aaaAzure fakturace Enterprise rozhraní API – souhrn a vyrovnávat | Microsoft Docs"
description: "Další informace o využití fakturace Azure a RateCard rozhraní API, které jsou používané tooprovide přehled o využívání prostředků Azure a trendy."
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
ms.openlocfilehash: b031de2c347e5abeacd11743cc96024434518918
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a><span data-ttu-id="e5d97-103">Rozhraní API pro vytváření sestav pro podnikové zákazníky – souhrn a vyrovnávat</span><span class="sxs-lookup"><span data-stu-id="e5d97-103">Reporting APIs for Enterprise customers - Balance and Summary</span></span>

<span data-ttu-id="e5d97-104">Hello vyrovnávat a souhrn rozhraní API nabízí měsíční souhrnné informace na zůstatky, nové nákupy, poplatky za služby Azure Marketplace, úpravy a Nadlimitní poplatky.</span><span class="sxs-lookup"><span data-stu-id="e5d97-104">hello Balance and Summary API offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments, and overage charges.</span></span>


##<a name="request"></a><span data-ttu-id="e5d97-105">Žádost</span><span class="sxs-lookup"><span data-stu-id="e5d97-105">Request</span></span> 
<span data-ttu-id="e5d97-106">Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="e5d97-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="e5d97-107">Pokud není zadán fakturační období, pak data pro aktuální fakturaci hello období vrácena.</span><span class="sxs-lookup"><span data-stu-id="e5d97-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span>

|<span data-ttu-id="e5d97-108">Metoda</span><span class="sxs-lookup"><span data-stu-id="e5d97-108">Method</span></span> | <span data-ttu-id="e5d97-109">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="e5d97-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="e5d97-110">GET</span><span class="sxs-lookup"><span data-stu-id="e5d97-110">GET</span></span>| <span data-ttu-id="e5d97-111">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / balancesummary</span><span class="sxs-lookup"><span data-stu-id="e5d97-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/balancesummary</span></span>|
|<span data-ttu-id="e5d97-112">GET</span><span class="sxs-lookup"><span data-stu-id="e5d97-112">GET</span></span>| <span data-ttu-id="e5d97-113">/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / balancesummary</span><span class="sxs-lookup"><span data-stu-id="e5d97-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/balancesummary</span></span>|

> [!Note]
> <span data-ttu-id="e5d97-114">verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e5d97-114">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="e5d97-115">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e5d97-115">Response</span></span>

        {
            "id": "enrollments/100/billingperiods/201507/balancesummaries",
            "billingPeriodId": 201507,
            "currencyCode": "USD",
            "beginningBalance": 0,
            "endingBalance": 1.1,
            "newPurchases": 1,
            "adjustments": 1.1,
            "utilized": 1.1,
            "serviceOverage": 1,
            "chargesBilledSeparately": 1,
            "totalOverage": 1,
            "totalUsage": 1.1,
            "azureMarketplaceServiceCharges": 1,
            "newPurchasesDetails": [
                {
                "name": "",
                "value": 1
                }
            ],
            "adjustmentDetails": [
                {
                "name": "Promo Credit",
                "value": 1.1
                },
                {
                "name": "SIE Credit",
                "value": 1.0
                }
            ]
        }


<span data-ttu-id="e5d97-116">**Definice vlastností odpovědi**</span><span class="sxs-lookup"><span data-stu-id="e5d97-116">**Response property definitions**</span></span>

|<span data-ttu-id="e5d97-117">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e5d97-117">Property Name</span></span>| <span data-ttu-id="e5d97-118">Typ</span><span class="sxs-lookup"><span data-stu-id="e5d97-118">Type</span></span>| <span data-ttu-id="e5d97-119">Popis</span><span class="sxs-lookup"><span data-stu-id="e5d97-119">Description</span></span>
|-|-|-|
|<span data-ttu-id="e5d97-120">id</span><span class="sxs-lookup"><span data-stu-id="e5d97-120">id</span></span>|<span data-ttu-id="e5d97-121">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e5d97-121">string</span></span>|<span data-ttu-id="e5d97-122">Hello jedinečné Id pro konkrétní fakturační období a registrace</span><span class="sxs-lookup"><span data-stu-id="e5d97-122">hello unique Id for a specific billing period and enrollment</span></span>|
|<span data-ttu-id="e5d97-123">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="e5d97-123">billingPeriodId</span></span>|<span data-ttu-id="e5d97-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e5d97-124">string</span></span> |<span data-ttu-id="e5d97-125">Hello fakturační období Id</span><span class="sxs-lookup"><span data-stu-id="e5d97-125">hello billing period Id</span></span>|
|<span data-ttu-id="e5d97-126">Kód měny</span><span class="sxs-lookup"><span data-stu-id="e5d97-126">currencyCode</span></span>|<span data-ttu-id="e5d97-127">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e5d97-127">string</span></span> |<span data-ttu-id="e5d97-128">Kód měny Hello</span><span class="sxs-lookup"><span data-stu-id="e5d97-128">hello currency code</span></span>|
|<span data-ttu-id="e5d97-129">beginningBalance</span><span class="sxs-lookup"><span data-stu-id="e5d97-129">beginningBalance</span></span>|<span data-ttu-id="e5d97-130">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-130">decimal</span></span>| <span data-ttu-id="e5d97-131">Hello počáteční zůstatek pro fakturační období hello</span><span class="sxs-lookup"><span data-stu-id="e5d97-131">hello beginning balance for hello billing period</span></span>|
|<span data-ttu-id="e5d97-132">endingBalance</span><span class="sxs-lookup"><span data-stu-id="e5d97-132">endingBalance</span></span>|<span data-ttu-id="e5d97-133">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-133">decimal</span></span>| <span data-ttu-id="e5d97-134">Hello konečný zůstatek hello fakturační období (pro otevřete období, které to se budou denně aktualizovat)</span><span class="sxs-lookup"><span data-stu-id="e5d97-134">hello ending balance for hello billing period (for open periods this will be updated daily)</span></span>|
|<span data-ttu-id="e5d97-135">newPurchases</span><span class="sxs-lookup"><span data-stu-id="e5d97-135">newPurchases</span></span>|<span data-ttu-id="e5d97-136">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-136">decimal</span></span>| <span data-ttu-id="e5d97-137">Celková velikost nového nákupu</span><span class="sxs-lookup"><span data-stu-id="e5d97-137">Total new purchase amount</span></span>|
|<span data-ttu-id="e5d97-138">úpravy</span><span class="sxs-lookup"><span data-stu-id="e5d97-138">adjustments</span></span>|<span data-ttu-id="e5d97-139">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-139">decimal</span></span>| <span data-ttu-id="e5d97-140">Množství celkový počet nastavení</span><span class="sxs-lookup"><span data-stu-id="e5d97-140">Total adjustment amount</span></span>|
|<span data-ttu-id="e5d97-141">využité</span><span class="sxs-lookup"><span data-stu-id="e5d97-141">utilized</span></span>|<span data-ttu-id="e5d97-142">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-142">decimal</span></span>| <span data-ttu-id="e5d97-143">Celkové využití závazků</span><span class="sxs-lookup"><span data-stu-id="e5d97-143">Total Commitment usage</span></span>|
|<span data-ttu-id="e5d97-144">serviceOverage</span><span class="sxs-lookup"><span data-stu-id="e5d97-144">serviceOverage</span></span>|<span data-ttu-id="e5d97-145">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-145">decimal</span></span>| <span data-ttu-id="e5d97-146">Nadlimitní hodnota pro služby Azure</span><span class="sxs-lookup"><span data-stu-id="e5d97-146">Overage for Azure services</span></span>|
|<span data-ttu-id="e5d97-147">chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="e5d97-147">chargesBilledSeparately</span></span>|<span data-ttu-id="e5d97-148">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-148">decimal</span></span>| <span data-ttu-id="e5d97-149">Poplatky za fakturuje samostatně</span><span class="sxs-lookup"><span data-stu-id="e5d97-149">Charges Billed separately</span></span>|
|<span data-ttu-id="e5d97-150">totalOverage</span><span class="sxs-lookup"><span data-stu-id="e5d97-150">totalOverage</span></span>|<span data-ttu-id="e5d97-151">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-151">decimal</span></span>| <span data-ttu-id="e5d97-152">serviceOverage + chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="e5d97-152">serviceOverage + chargesBilledSeparately</span></span>|
|<span data-ttu-id="e5d97-153">totalUsage</span><span class="sxs-lookup"><span data-stu-id="e5d97-153">totalUsage</span></span>|<span data-ttu-id="e5d97-154">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-154">decimal</span></span>| <span data-ttu-id="e5d97-155">Služba Azure závazků + celková Nadlimitní hodnota</span><span class="sxs-lookup"><span data-stu-id="e5d97-155">Azure service commitment + total Overage</span></span>|
|<span data-ttu-id="e5d97-156">azureMarketplaceServiceCharges</span><span class="sxs-lookup"><span data-stu-id="e5d97-156">azureMarketplaceServiceCharges</span></span>|<span data-ttu-id="e5d97-157">Decimal</span><span class="sxs-lookup"><span data-stu-id="e5d97-157">decimal</span></span>| <span data-ttu-id="e5d97-158">Celkové náklady pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="e5d97-158">Total charges for Azure Marketplace</span></span>|
|<span data-ttu-id="e5d97-159">newPurchasesDetails</span><span class="sxs-lookup"><span data-stu-id="e5d97-159">newPurchasesDetails</span></span>|<span data-ttu-id="e5d97-160">Pole JSON řetězec dvojic název hodnota</span><span class="sxs-lookup"><span data-stu-id="e5d97-160">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="e5d97-161">Seznam nových nákupů</span><span class="sxs-lookup"><span data-stu-id="e5d97-161">List of new purchases</span></span>|
|<span data-ttu-id="e5d97-162">adjustmentDetails</span><span class="sxs-lookup"><span data-stu-id="e5d97-162">adjustmentDetails</span></span>|<span data-ttu-id="e5d97-163">Pole JSON řetězec dvojic název hodnota</span><span class="sxs-lookup"><span data-stu-id="e5d97-163">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="e5d97-164">Seznam úpravy (platební povýšení, SIE platební atd.)</span><span class="sxs-lookup"><span data-stu-id="e5d97-164">List of Adjustments (Promo credit, SIE credit etc.)</span></span> |


<br/>
## <a name="see-also"></a><span data-ttu-id="e5d97-165">Viz také</span><span class="sxs-lookup"><span data-stu-id="e5d97-165">See also</span></span>

* [<span data-ttu-id="e5d97-166">Fakturační období rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e5d97-166">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="e5d97-167">Podrobnosti o použití rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e5d97-167">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="e5d97-168">Úložiště Marketplace poplatků rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e5d97-168">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="e5d97-169">Rozhraní API list cena</span><span class="sxs-lookup"><span data-stu-id="e5d97-169">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)