---
title: "Azure fakturace rozhraní API Enterprise – souhrn a vyrovnávat | Microsoft Docs"
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
ms.openlocfilehash: f6b149f0e656d2263705048aa5b644f5bb4a5712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a><span data-ttu-id="9ed58-103">Rozhraní API pro vytváření sestav pro podnikové zákazníky – souhrn a vyrovnávat</span><span class="sxs-lookup"><span data-stu-id="9ed58-103">Reporting APIs for Enterprise customers - Balance and Summary</span></span>

<span data-ttu-id="9ed58-104">Vyrovnávat a souhrn rozhraní API nabízí měsíční souhrnné informace na zůstatky, nové nákupy, poplatky za služby Azure Marketplace, úpravy a Nadlimitní poplatky.</span><span class="sxs-lookup"><span data-stu-id="9ed58-104">The Balance and Summary API offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments, and overage charges.</span></span>


##<a name="request"></a><span data-ttu-id="9ed58-105">Žádost</span><span class="sxs-lookup"><span data-stu-id="9ed58-105">Request</span></span> 
<span data-ttu-id="9ed58-106">Nejsou zadány společných vlastností hlavičky, které je třeba přidat [zde](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="9ed58-106">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="9ed58-107">Pokud není zadán fakturační období, je vrácena data pro aktuální fakturační období.</span><span class="sxs-lookup"><span data-stu-id="9ed58-107">If a billing period is not specified, then data for the current billing period is returned.</span></span>

|<span data-ttu-id="9ed58-108">Metoda</span><span class="sxs-lookup"><span data-stu-id="9ed58-108">Method</span></span> | <span data-ttu-id="9ed58-109">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="9ed58-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="9ed58-110">GET</span><span class="sxs-lookup"><span data-stu-id="9ed58-110">GET</span></span>| <span data-ttu-id="9ed58-111">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / balancesummary</span><span class="sxs-lookup"><span data-stu-id="9ed58-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/balancesummary</span></span>|
|<span data-ttu-id="9ed58-112">GET</span><span class="sxs-lookup"><span data-stu-id="9ed58-112">GET</span></span>| <span data-ttu-id="9ed58-113">/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / balancesummary</span><span class="sxs-lookup"><span data-stu-id="9ed58-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/balancesummary</span></span>|

> [!Note]
> <span data-ttu-id="9ed58-114">Pokud chcete používat verzi preview rozhraní API, nahraďte v2 v1 v výše uvedenou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9ed58-114">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="9ed58-115">Odpověď</span><span class="sxs-lookup"><span data-stu-id="9ed58-115">Response</span></span>

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


<span data-ttu-id="9ed58-116">**Definice vlastností odpovědi**</span><span class="sxs-lookup"><span data-stu-id="9ed58-116">**Response property definitions**</span></span>

|<span data-ttu-id="9ed58-117">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9ed58-117">Property Name</span></span>| <span data-ttu-id="9ed58-118">Typ</span><span class="sxs-lookup"><span data-stu-id="9ed58-118">Type</span></span>| <span data-ttu-id="9ed58-119">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed58-119">Description</span></span>
|-|-|-|
|<span data-ttu-id="9ed58-120">id</span><span class="sxs-lookup"><span data-stu-id="9ed58-120">id</span></span>|<span data-ttu-id="9ed58-121">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ed58-121">string</span></span>|<span data-ttu-id="9ed58-122">Jedinečné Id pro konkrétní fakturační období a registrace</span><span class="sxs-lookup"><span data-stu-id="9ed58-122">The unique Id for a specific billing period and enrollment</span></span>|
|<span data-ttu-id="9ed58-123">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="9ed58-123">billingPeriodId</span></span>|<span data-ttu-id="9ed58-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ed58-124">string</span></span> |<span data-ttu-id="9ed58-125">Fakturační období Id</span><span class="sxs-lookup"><span data-stu-id="9ed58-125">The billing period Id</span></span>|
|<span data-ttu-id="9ed58-126">Kód měny</span><span class="sxs-lookup"><span data-stu-id="9ed58-126">currencyCode</span></span>|<span data-ttu-id="9ed58-127">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9ed58-127">string</span></span> |<span data-ttu-id="9ed58-128">Kód měny</span><span class="sxs-lookup"><span data-stu-id="9ed58-128">The currency code</span></span>|
|<span data-ttu-id="9ed58-129">beginningBalance</span><span class="sxs-lookup"><span data-stu-id="9ed58-129">beginningBalance</span></span>|<span data-ttu-id="9ed58-130">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-130">decimal</span></span>| <span data-ttu-id="9ed58-131">Počáteční zůstatek za fakturační období</span><span class="sxs-lookup"><span data-stu-id="9ed58-131">The beginning balance for the billing period</span></span>|
|<span data-ttu-id="9ed58-132">endingBalance</span><span class="sxs-lookup"><span data-stu-id="9ed58-132">endingBalance</span></span>|<span data-ttu-id="9ed58-133">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-133">decimal</span></span>| <span data-ttu-id="9ed58-134">Konečný zůstatek za fakturační období (pro otevřete období, které to se budou denně aktualizovat)</span><span class="sxs-lookup"><span data-stu-id="9ed58-134">The ending balance for the billing period (for open periods this will be updated daily)</span></span>|
|<span data-ttu-id="9ed58-135">newPurchases</span><span class="sxs-lookup"><span data-stu-id="9ed58-135">newPurchases</span></span>|<span data-ttu-id="9ed58-136">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-136">decimal</span></span>| <span data-ttu-id="9ed58-137">Celková velikost nového nákupu</span><span class="sxs-lookup"><span data-stu-id="9ed58-137">Total new purchase amount</span></span>|
|<span data-ttu-id="9ed58-138">úpravy</span><span class="sxs-lookup"><span data-stu-id="9ed58-138">adjustments</span></span>|<span data-ttu-id="9ed58-139">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-139">decimal</span></span>| <span data-ttu-id="9ed58-140">Množství celkový počet nastavení</span><span class="sxs-lookup"><span data-stu-id="9ed58-140">Total adjustment amount</span></span>|
|<span data-ttu-id="9ed58-141">využité</span><span class="sxs-lookup"><span data-stu-id="9ed58-141">utilized</span></span>|<span data-ttu-id="9ed58-142">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-142">decimal</span></span>| <span data-ttu-id="9ed58-143">Celkové využití závazků</span><span class="sxs-lookup"><span data-stu-id="9ed58-143">Total Commitment usage</span></span>|
|<span data-ttu-id="9ed58-144">serviceOverage</span><span class="sxs-lookup"><span data-stu-id="9ed58-144">serviceOverage</span></span>|<span data-ttu-id="9ed58-145">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-145">decimal</span></span>| <span data-ttu-id="9ed58-146">Nadlimitní hodnota pro služby Azure</span><span class="sxs-lookup"><span data-stu-id="9ed58-146">Overage for Azure services</span></span>|
|<span data-ttu-id="9ed58-147">chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="9ed58-147">chargesBilledSeparately</span></span>|<span data-ttu-id="9ed58-148">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-148">decimal</span></span>| <span data-ttu-id="9ed58-149">Poplatky za fakturuje samostatně</span><span class="sxs-lookup"><span data-stu-id="9ed58-149">Charges Billed separately</span></span>|
|<span data-ttu-id="9ed58-150">totalOverage</span><span class="sxs-lookup"><span data-stu-id="9ed58-150">totalOverage</span></span>|<span data-ttu-id="9ed58-151">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-151">decimal</span></span>| <span data-ttu-id="9ed58-152">serviceOverage + chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="9ed58-152">serviceOverage + chargesBilledSeparately</span></span>|
|<span data-ttu-id="9ed58-153">totalUsage</span><span class="sxs-lookup"><span data-stu-id="9ed58-153">totalUsage</span></span>|<span data-ttu-id="9ed58-154">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-154">decimal</span></span>| <span data-ttu-id="9ed58-155">Služba Azure závazků + celková Nadlimitní hodnota</span><span class="sxs-lookup"><span data-stu-id="9ed58-155">Azure service commitment + total Overage</span></span>|
|<span data-ttu-id="9ed58-156">azureMarketplaceServiceCharges</span><span class="sxs-lookup"><span data-stu-id="9ed58-156">azureMarketplaceServiceCharges</span></span>|<span data-ttu-id="9ed58-157">Decimal</span><span class="sxs-lookup"><span data-stu-id="9ed58-157">decimal</span></span>| <span data-ttu-id="9ed58-158">Celkové náklady pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="9ed58-158">Total charges for Azure Marketplace</span></span>|
|<span data-ttu-id="9ed58-159">newPurchasesDetails</span><span class="sxs-lookup"><span data-stu-id="9ed58-159">newPurchasesDetails</span></span>|<span data-ttu-id="9ed58-160">Pole JSON řetězec dvojic název hodnota</span><span class="sxs-lookup"><span data-stu-id="9ed58-160">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="9ed58-161">Seznam nových nákupů</span><span class="sxs-lookup"><span data-stu-id="9ed58-161">List of new purchases</span></span>|
|<span data-ttu-id="9ed58-162">adjustmentDetails</span><span class="sxs-lookup"><span data-stu-id="9ed58-162">adjustmentDetails</span></span>|<span data-ttu-id="9ed58-163">Pole JSON řetězec dvojic název hodnota</span><span class="sxs-lookup"><span data-stu-id="9ed58-163">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="9ed58-164">Seznam úpravy (platební povýšení, SIE platební atd.)</span><span class="sxs-lookup"><span data-stu-id="9ed58-164">List of Adjustments (Promo credit, SIE credit etc.)</span></span> |


<br/>
## <a name="see-also"></a><span data-ttu-id="9ed58-165">Viz také</span><span class="sxs-lookup"><span data-stu-id="9ed58-165">See also</span></span>

* [<span data-ttu-id="9ed58-166">Fakturační období rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9ed58-166">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="9ed58-167">Podrobnosti o použití rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9ed58-167">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="9ed58-168">Úložiště Marketplace poplatků rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9ed58-168">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="9ed58-169">Rozhraní API list cena</span><span class="sxs-lookup"><span data-stu-id="9ed58-169">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)