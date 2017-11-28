---
title: "aaaAzure fakturace Enterprise rozhraní API - fakturační období | Microsoft Docs"
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
ms.openlocfilehash: d4e17f25b22729a7f213306fb019ee0dbeca87ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a><span data-ttu-id="9a8bb-103">Rozhraní API pro vytváření sestav pro podnikové zákazníky - fakturační období</span><span class="sxs-lookup"><span data-stu-id="9a8bb-103">Reporting APIs for Enterprise customers - Billing Periods</span></span>

<span data-ttu-id="9a8bb-104">Hello fakturační období API vrátí seznam fakturační období, které mají datům o spotřebě pro hello zadat registrace v obráceném chronologickém pořadí.</span><span class="sxs-lookup"><span data-stu-id="9a8bb-104">hello Billing Periods API returns a list of billing periods that have consumption data for hello specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="9a8bb-105">Každé období obsahuje vlastnost odkazující toohello trasu rozhraní API pro hello čtyři sady dat – BalanceSummary, UsageDetails, Marktplace poplatky a ceník.</span><span class="sxs-lookup"><span data-stu-id="9a8bb-105">Each Period contains a property pointing toohello API route for hello four sets of data - BalanceSummary, UsageDetails, Marktplace Charges, and PriceSheet.</span></span> <span data-ttu-id="9a8bb-106">Pokud hello období neobsahuje žádná data, hello odpovídající vlastnost má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="9a8bb-106">If hello period does not have data, hello corresponding property is null.</span></span> 


##<a name="request"></a><span data-ttu-id="9a8bb-107">Žádost</span><span class="sxs-lookup"><span data-stu-id="9a8bb-107">Request</span></span> 
<span data-ttu-id="9a8bb-108">Nejsou zadány společných vlastností hlavičky, které je třeba přidat toobe [zde](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="9a8bb-108">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> 

|<span data-ttu-id="9a8bb-109">Metoda</span><span class="sxs-lookup"><span data-stu-id="9a8bb-109">Method</span></span> | <span data-ttu-id="9a8bb-110">Identifikátor URI požadavku</span><span class="sxs-lookup"><span data-stu-id="9a8bb-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="9a8bb-111">GET</span><span class="sxs-lookup"><span data-stu-id="9a8bb-111">GET</span></span>| <span data-ttu-id="9a8bb-112">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / billingperiods</span><span class="sxs-lookup"><span data-stu-id="9a8bb-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingperiods</span></span>|

> [!Note]
> <span data-ttu-id="9a8bb-113">verze preview hello toouse rozhraní API, nahraďte v2 v1 v hello výše adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9a8bb-113">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="9a8bb-114">Odpověď</span><span class="sxs-lookup"><span data-stu-id="9a8bb-114">Response</span></span>
 
    
    
      [
            {
                "billingPeriodId": "201704",
                "billingStart": "2017-04-01T00:00:00Z",
                "billingEnd": "2017-04-30T11:59:59Z",
                "balanceSummary": "/v1/enrollments/100/billingperiods/201704/balancesummary",
                "usageDetails": "/v1/enrollments/100/billingperiods/201704/usagedetails",
                "marketplaceCharges": "/v1/enrollments/100/billingperiods/201704/marketplacecharges",
                "priceSheet": "/v1/enrollments/100/billingperiods/201704/pricesheet"
            },          
            ....
      ]
    

<span data-ttu-id="9a8bb-115">**Definice vlastností odpovědi**</span><span class="sxs-lookup"><span data-stu-id="9a8bb-115">**Response property definitions**</span></span>

|<span data-ttu-id="9a8bb-116">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9a8bb-116">Property Name</span></span>| <span data-ttu-id="9a8bb-117">Typ</span><span class="sxs-lookup"><span data-stu-id="9a8bb-117">Type</span></span>| <span data-ttu-id="9a8bb-118">Popis</span><span class="sxs-lookup"><span data-stu-id="9a8bb-118">Description</span></span>
|-|-|-|
|<span data-ttu-id="9a8bb-119">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="9a8bb-119">billingPeriodId</span></span>| <span data-ttu-id="9a8bb-120">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a8bb-120">string</span></span>| <span data-ttu-id="9a8bb-121">Hello jedinečné Id, který představuje konkrétní období fakturace</span><span class="sxs-lookup"><span data-stu-id="9a8bb-121">hello unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="9a8bb-122">billingStart</span><span class="sxs-lookup"><span data-stu-id="9a8bb-122">billingStart</span></span>| <span data-ttu-id="9a8bb-123">Data a času</span><span class="sxs-lookup"><span data-stu-id="9a8bb-123">datetime</span></span>| <span data-ttu-id="9a8bb-124">ISO 8601 řetězec představující datum začátku období hello</span><span class="sxs-lookup"><span data-stu-id="9a8bb-124">ISO 8601 string representing hello period start date</span></span>|
|<span data-ttu-id="9a8bb-125">billingEnd</span><span class="sxs-lookup"><span data-stu-id="9a8bb-125">billingEnd</span></span>| <span data-ttu-id="9a8bb-126">Data a času</span><span class="sxs-lookup"><span data-stu-id="9a8bb-126">datetime</span></span>| <span data-ttu-id="9a8bb-127">ISO 8601 řetězec představující datum ukončení období hello</span><span class="sxs-lookup"><span data-stu-id="9a8bb-127">ISO 8601 string representing hello period end date</span></span>|
|<span data-ttu-id="9a8bb-128">balanceSummary</span><span class="sxs-lookup"><span data-stu-id="9a8bb-128">balanceSummary</span></span>| <span data-ttu-id="9a8bb-129">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a8bb-129">string</span></span>| <span data-ttu-id="9a8bb-130">Hello cestu adresy URL, který směruje toohello vyrovnávání souhrnná data pro toto období</span><span class="sxs-lookup"><span data-stu-id="9a8bb-130">hello URL path that routes toohello Balance Summary data for this period</span></span>|
|<span data-ttu-id="9a8bb-131">usageDetails</span><span class="sxs-lookup"><span data-stu-id="9a8bb-131">usageDetails</span></span>| <span data-ttu-id="9a8bb-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a8bb-132">string</span></span>| <span data-ttu-id="9a8bb-133">Hello cestu adresy URL, který směruje toohello podrobnosti o použití dat pro toto období</span><span class="sxs-lookup"><span data-stu-id="9a8bb-133">hello URL path that routes toohello Usage Details data for this period</span></span>|
|<span data-ttu-id="9a8bb-134">marketplaceCharges</span><span class="sxs-lookup"><span data-stu-id="9a8bb-134">marketplaceCharges</span></span>| <span data-ttu-id="9a8bb-135">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a8bb-135">string</span></span>| <span data-ttu-id="9a8bb-136">Hello cestu adresy URL, který směruje toohello poplatky Marketplace. data pro toto období</span><span class="sxs-lookup"><span data-stu-id="9a8bb-136">hello URL path that routes toohello Marketplace Charges data for this period</span></span>|
|<span data-ttu-id="9a8bb-137">Ceník</span><span class="sxs-lookup"><span data-stu-id="9a8bb-137">priceSheet</span></span>| <span data-ttu-id="9a8bb-138">Řetězec</span><span class="sxs-lookup"><span data-stu-id="9a8bb-138">string</span></span>| <span data-ttu-id="9a8bb-139">Hello cestu adresy URL, který směruje toohello ceník data pro toto období</span><span class="sxs-lookup"><span data-stu-id="9a8bb-139">hello URL path that routes toohello PriceSheet data for this period</span></span>|

<br/>
## <a name="see-also"></a><span data-ttu-id="9a8bb-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="9a8bb-140">See also</span></span>

* [<span data-ttu-id="9a8bb-141">Vyrovnávat a souhrn rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9a8bb-141">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="9a8bb-142">Podrobnosti o použití rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9a8bb-142">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="9a8bb-143">Úložiště Marketplace poplatků rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9a8bb-143">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="9a8bb-144">Rozhraní API list cena</span><span class="sxs-lookup"><span data-stu-id="9a8bb-144">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)