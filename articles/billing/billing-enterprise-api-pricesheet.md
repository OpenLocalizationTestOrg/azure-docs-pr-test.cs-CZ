---
title: "Azure fakturace rozhraní API Enterprise - ceník | Microsoft Docs"
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
ms.openlocfilehash: 2e7d6e883abe4cee13bc5f684baf2a1ea9c6c397
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a><span data-ttu-id="12948-103">Rozhraní API pro vytváření sestav pro podnikové zákazníky - ceníku</span><span class="sxs-lookup"><span data-stu-id="12948-103">Reporting APIs for Enterprise customers - Price Sheet</span></span>

<span data-ttu-id="12948-104">Rozhraní API Price Sheet poskytuje použít rychlost pro každé monitorování pro danou registrace a fakturace období.</span><span class="sxs-lookup"><span data-stu-id="12948-104">The Price Sheet API provides the applicable rate for each Meter for the given Enrollment and Billing Period.</span></span>

##<a name="request"></a><span data-ttu-id="12948-105">Žádost</span><span class="sxs-lookup"><span data-stu-id="12948-105">Request</span></span>
<span data-ttu-id="12948-106">Nejsou zadány společných vlastností hlavičky, které je třeba přidat [zde](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="12948-106">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="12948-107">Pokud není zadán fakturační období, je vrácena data pro aktuální fakturační období.</span><span class="sxs-lookup"><span data-stu-id="12948-107">If a billing period is not specified, then data for the current billing period is returned.</span></span>

|<span data-ttu-id="12948-108">Metoda</span><span class="sxs-lookup"><span data-stu-id="12948-108">Method</span></span> | <span data-ttu-id="12948-109">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="12948-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="12948-110">GET</span><span class="sxs-lookup"><span data-stu-id="12948-110">GET</span></span>|<span data-ttu-id="12948-111">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / ceník</span><span class="sxs-lookup"><span data-stu-id="12948-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/pricesheet</span></span>|
|<span data-ttu-id="12948-112">GET</span><span class="sxs-lookup"><span data-stu-id="12948-112">GET</span></span>|<span data-ttu-id="12948-113">/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / ceník</span><span class="sxs-lookup"><span data-stu-id="12948-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet</span></span>|

> [!Note]
> <span data-ttu-id="12948-114">Pokud chcete používat verzi preview rozhraní API, nahraďte v2 v1 v výše uvedenou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="12948-114">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="12948-115">Odpověď</span><span class="sxs-lookup"><span data-stu-id="12948-115">Response</span></span>

    
        [
            {
                "id": "enrollments/57354989/billingperiods/201601/products/343/pricesheets",
                "billingPeriodId": "201704",
                "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
                "meterName": "A1 VM",
                "unitOfMeasure": "100 Hours",
                "includedQuantity": 0,
                "partNumber": "N7H-00015",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            {
                "id": "enrollments/57354989/billingperiods/201601/products/2884/pricesheets",
                "billingPeriodId": "201404",
                "meterId": "dc210ecb-97e8-4522-8134-5385494233c0",
                "meterName": "Locally Redundant Storage Premium Storage - Snapshots - AU East",
                "unitOfMeasure": "100 GB",
                "includedQuantity": 0,
                "partNumber": "N9H-00402",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            ...
        ]
    

> [!Note]
><span data-ttu-id="12948-116">Pokud používáte rozhraní API verze Preview, meterId pole není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="12948-116">If you are using the Preview API, meterId field is not available.</span></span>
>

<span data-ttu-id="12948-117">**Definice vlastností odpovědi**</span><span class="sxs-lookup"><span data-stu-id="12948-117">**Response property definitions**</span></span>

|<span data-ttu-id="12948-118">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="12948-118">Property Name</span></span>| <span data-ttu-id="12948-119">Typ</span><span class="sxs-lookup"><span data-stu-id="12948-119">Type</span></span>| <span data-ttu-id="12948-120">Popis</span><span class="sxs-lookup"><span data-stu-id="12948-120">Description</span></span>
|-|-|-|
|<span data-ttu-id="12948-121">id</span><span class="sxs-lookup"><span data-stu-id="12948-121">id</span></span>| <span data-ttu-id="12948-122">Řetězec</span><span class="sxs-lookup"><span data-stu-id="12948-122">string</span></span>| <span data-ttu-id="12948-123">Jedinečné Id, který představuje položku určitý Ceník (monitorovat pomocí fakturační období)</span><span class="sxs-lookup"><span data-stu-id="12948-123">The unique Id that represents a particular PriceSheet item (meter by billing period)</span></span>|
|<span data-ttu-id="12948-124">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="12948-124">billingPeriodId</span></span>| <span data-ttu-id="12948-125">Řetězec</span><span class="sxs-lookup"><span data-stu-id="12948-125">string</span></span>| <span data-ttu-id="12948-126">Jedinečné Id, který představuje konkrétní období fakturace</span><span class="sxs-lookup"><span data-stu-id="12948-126">The unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="12948-127">meterId</span><span class="sxs-lookup"><span data-stu-id="12948-127">meterId</span></span>| <span data-ttu-id="12948-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="12948-128">string</span></span>| <span data-ttu-id="12948-129">Identifikátor pro měřidla.</span><span class="sxs-lookup"><span data-stu-id="12948-129">The identifier for the meter.</span></span> <span data-ttu-id="12948-130">Může být namapované na meterId využití.</span><span class="sxs-lookup"><span data-stu-id="12948-130">It can be mapped to the usage meterId.</span></span>|
|<span data-ttu-id="12948-131">meterName</span><span class="sxs-lookup"><span data-stu-id="12948-131">meterName</span></span>| <span data-ttu-id="12948-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="12948-132">string</span></span>| <span data-ttu-id="12948-133">Název měřidla</span><span class="sxs-lookup"><span data-stu-id="12948-133">The meter name</span></span>|
|<span data-ttu-id="12948-134">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="12948-134">unitOfMeasure</span></span>| <span data-ttu-id="12948-135">Řetězec</span><span class="sxs-lookup"><span data-stu-id="12948-135">string</span></span>| <span data-ttu-id="12948-136">Jednotka pro měření služby</span><span class="sxs-lookup"><span data-stu-id="12948-136">The Unit of Measure for measuring the service</span></span>|
|<span data-ttu-id="12948-137">includedQuantity</span><span class="sxs-lookup"><span data-stu-id="12948-137">includedQuantity</span></span>| <span data-ttu-id="12948-138">Decimal</span><span class="sxs-lookup"><span data-stu-id="12948-138">decimal</span></span>| <span data-ttu-id="12948-139">Množství, které je součástí</span><span class="sxs-lookup"><span data-stu-id="12948-139">Quantity that is included</span></span> |
|<span data-ttu-id="12948-140">partNumber</span><span class="sxs-lookup"><span data-stu-id="12948-140">partNumber</span></span>| <span data-ttu-id="12948-141">Řetězec</span><span class="sxs-lookup"><span data-stu-id="12948-141">string</span></span>| <span data-ttu-id="12948-142">Výrobní číslo spojené s měřidla</span><span class="sxs-lookup"><span data-stu-id="12948-142">The part number associated with the Meter</span></span>|
|<span data-ttu-id="12948-143">unitPrice</span><span class="sxs-lookup"><span data-stu-id="12948-143">unitPrice</span></span>| <span data-ttu-id="12948-144">Decimal</span><span class="sxs-lookup"><span data-stu-id="12948-144">decimal</span></span>| <span data-ttu-id="12948-145">Jednotkové ceny pro měřidla</span><span class="sxs-lookup"><span data-stu-id="12948-145">The unit price for the meter</span></span>|
|<span data-ttu-id="12948-146">Kód měny</span><span class="sxs-lookup"><span data-stu-id="12948-146">currencyCode</span></span>| <span data-ttu-id="12948-147">Řetězec</span><span class="sxs-lookup"><span data-stu-id="12948-147">string</span></span>| <span data-ttu-id="12948-148">Kód měny pro pole unitPrice</span><span class="sxs-lookup"><span data-stu-id="12948-148">The currency code for the unitPrice</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="12948-149">Viz také</span><span class="sxs-lookup"><span data-stu-id="12948-149">See also</span></span>

* [<span data-ttu-id="12948-150">Fakturační období rozhraní API</span><span class="sxs-lookup"><span data-stu-id="12948-150">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="12948-151">Podrobnosti o použití rozhraní API</span><span class="sxs-lookup"><span data-stu-id="12948-151">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md)

* [<span data-ttu-id="12948-152">Vyrovnávat a souhrn rozhraní API</span><span class="sxs-lookup"><span data-stu-id="12948-152">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="12948-153">Úložiště Marketplace poplatků rozhraní API</span><span class="sxs-lookup"><span data-stu-id="12948-153">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md)
