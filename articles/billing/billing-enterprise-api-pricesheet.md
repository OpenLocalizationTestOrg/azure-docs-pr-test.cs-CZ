---
title: "aaaAzure fakturace Enterprise rozhraní API - ceník | Microsoft Docs"
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
ms.openlocfilehash: 4cfe694c63fba266d117054b046d9caacb3b7197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a><span data-ttu-id="59a77-103">Rozhraní API pro vytváření sestav pro podnikové zákazníky - ceníku</span><span class="sxs-lookup"><span data-stu-id="59a77-103">Reporting APIs for Enterprise customers - Price Sheet</span></span>

<span data-ttu-id="59a77-104">Hello Price Sheet API poskytuje hello použít rychlost pro každé monitorování pro hello daného registrace a fakturace období.</span><span class="sxs-lookup"><span data-stu-id="59a77-104">hello Price Sheet API provides hello applicable rate for each Meter for hello given Enrollment and Billing Period.</span></span>

##<a name="request"></a><span data-ttu-id="59a77-105">Žádost</span><span class="sxs-lookup"><span data-stu-id="59a77-105">Request</span></span>
<span data-ttu-id="59a77-106">Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="59a77-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="59a77-107">Pokud není zadán fakturační období, pak data pro aktuální fakturaci hello období vrácena.</span><span class="sxs-lookup"><span data-stu-id="59a77-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span>

|<span data-ttu-id="59a77-108">Metoda</span><span class="sxs-lookup"><span data-stu-id="59a77-108">Method</span></span> | <span data-ttu-id="59a77-109">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="59a77-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="59a77-110">GET</span><span class="sxs-lookup"><span data-stu-id="59a77-110">GET</span></span>|<span data-ttu-id="59a77-111">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / ceník</span><span class="sxs-lookup"><span data-stu-id="59a77-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/pricesheet</span></span>|
|<span data-ttu-id="59a77-112">GET</span><span class="sxs-lookup"><span data-stu-id="59a77-112">GET</span></span>|<span data-ttu-id="59a77-113">/billingPeriods/ https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} {billingPeriod} / ceník</span><span class="sxs-lookup"><span data-stu-id="59a77-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet</span></span>|

> [!Note]
> <span data-ttu-id="59a77-114">verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.</span><span class="sxs-lookup"><span data-stu-id="59a77-114">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="59a77-115">Odpověď</span><span class="sxs-lookup"><span data-stu-id="59a77-115">Response</span></span>

    
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
><span data-ttu-id="59a77-116">Pokud používáte hello Preview rozhraní API, meterId pole není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="59a77-116">If you are using hello Preview API, meterId field is not available.</span></span>
>

<span data-ttu-id="59a77-117">**Definice vlastností odpovědi**</span><span class="sxs-lookup"><span data-stu-id="59a77-117">**Response property definitions**</span></span>

|<span data-ttu-id="59a77-118">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="59a77-118">Property Name</span></span>| <span data-ttu-id="59a77-119">Typ</span><span class="sxs-lookup"><span data-stu-id="59a77-119">Type</span></span>| <span data-ttu-id="59a77-120">Popis</span><span class="sxs-lookup"><span data-stu-id="59a77-120">Description</span></span>
|-|-|-|
|<span data-ttu-id="59a77-121">id</span><span class="sxs-lookup"><span data-stu-id="59a77-121">id</span></span>| <span data-ttu-id="59a77-122">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59a77-122">string</span></span>| <span data-ttu-id="59a77-123">Hello jedinečné Id, který představuje položku určitý Ceník (monitorovat pomocí fakturační období)</span><span class="sxs-lookup"><span data-stu-id="59a77-123">hello unique Id that represents a particular PriceSheet item (meter by billing period)</span></span>|
|<span data-ttu-id="59a77-124">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="59a77-124">billingPeriodId</span></span>| <span data-ttu-id="59a77-125">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59a77-125">string</span></span>| <span data-ttu-id="59a77-126">Hello jedinečné Id, který představuje konkrétní období fakturace</span><span class="sxs-lookup"><span data-stu-id="59a77-126">hello unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="59a77-127">meterId</span><span class="sxs-lookup"><span data-stu-id="59a77-127">meterId</span></span>| <span data-ttu-id="59a77-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59a77-128">string</span></span>| <span data-ttu-id="59a77-129">identifikátor Hello hello měření.</span><span class="sxs-lookup"><span data-stu-id="59a77-129">hello identifier for hello meter.</span></span> <span data-ttu-id="59a77-130">Může být namapované toohello meterId využití.</span><span class="sxs-lookup"><span data-stu-id="59a77-130">It can be mapped toohello usage meterId.</span></span>|
|<span data-ttu-id="59a77-131">meterName</span><span class="sxs-lookup"><span data-stu-id="59a77-131">meterName</span></span>| <span data-ttu-id="59a77-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59a77-132">string</span></span>| <span data-ttu-id="59a77-133">Název měřidla Hello</span><span class="sxs-lookup"><span data-stu-id="59a77-133">hello meter name</span></span>|
|<span data-ttu-id="59a77-134">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="59a77-134">unitOfMeasure</span></span>| <span data-ttu-id="59a77-135">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59a77-135">string</span></span>| <span data-ttu-id="59a77-136">Hello jednotka pro měření hello služby</span><span class="sxs-lookup"><span data-stu-id="59a77-136">hello Unit of Measure for measuring hello service</span></span>|
|<span data-ttu-id="59a77-137">includedQuantity</span><span class="sxs-lookup"><span data-stu-id="59a77-137">includedQuantity</span></span>| <span data-ttu-id="59a77-138">Decimal</span><span class="sxs-lookup"><span data-stu-id="59a77-138">decimal</span></span>| <span data-ttu-id="59a77-139">Množství, které je součástí</span><span class="sxs-lookup"><span data-stu-id="59a77-139">Quantity that is included</span></span> |
|<span data-ttu-id="59a77-140">partNumber</span><span class="sxs-lookup"><span data-stu-id="59a77-140">partNumber</span></span>| <span data-ttu-id="59a77-141">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59a77-141">string</span></span>| <span data-ttu-id="59a77-142">Hello výrobní číslo spojené s hello měření</span><span class="sxs-lookup"><span data-stu-id="59a77-142">hello part number associated with hello Meter</span></span>|
|<span data-ttu-id="59a77-143">unitPrice</span><span class="sxs-lookup"><span data-stu-id="59a77-143">unitPrice</span></span>| <span data-ttu-id="59a77-144">Decimal</span><span class="sxs-lookup"><span data-stu-id="59a77-144">decimal</span></span>| <span data-ttu-id="59a77-145">Hello jednotkové ceny pro měření hello</span><span class="sxs-lookup"><span data-stu-id="59a77-145">hello unit price for hello meter</span></span>|
|<span data-ttu-id="59a77-146">Kód měny</span><span class="sxs-lookup"><span data-stu-id="59a77-146">currencyCode</span></span>| <span data-ttu-id="59a77-147">Řetězec</span><span class="sxs-lookup"><span data-stu-id="59a77-147">string</span></span>| <span data-ttu-id="59a77-148">Kód měny Hello pro hello unitPrice</span><span class="sxs-lookup"><span data-stu-id="59a77-148">hello currency code for hello unitPrice</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="59a77-149">Viz také</span><span class="sxs-lookup"><span data-stu-id="59a77-149">See also</span></span>

* [<span data-ttu-id="59a77-150">Fakturační období rozhraní API</span><span class="sxs-lookup"><span data-stu-id="59a77-150">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="59a77-151">Podrobnosti o použití rozhraní API</span><span class="sxs-lookup"><span data-stu-id="59a77-151">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md)

* [<span data-ttu-id="59a77-152">Vyrovnávat a souhrn rozhraní API</span><span class="sxs-lookup"><span data-stu-id="59a77-152">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="59a77-153">Úložiště Marketplace poplatků rozhraní API</span><span class="sxs-lookup"><span data-stu-id="59a77-153">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md)
