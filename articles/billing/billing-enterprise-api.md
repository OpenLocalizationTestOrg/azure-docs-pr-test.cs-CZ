---
title: "Azure fakturace Enterprise rozhraní API | Microsoft Docs"
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
ms.openlocfilehash: e3a5f9bcd6b54a51c29df649f1ae8ac185b153a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a><span data-ttu-id="0e6d5-103">Přehled rozhraní API pro vytváření sestav pro podnikové zákazníky</span><span class="sxs-lookup"><span data-stu-id="0e6d5-103">Overview of Reporting APIs for Enterprise customers</span></span>
<span data-ttu-id="0e6d5-104">Rozhraní API pro vytváření sestav umožňují zákazníky, kteří Enterprise Azure prostřednictvím kódu programu vyžádání využívání a fakturační údaje do nástrojů pro analýzu dat upřednostňované.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-104">The Reporting APIs enable Enterprise Azure customers to programmatically pull consumption and billing data into preferred data analysis tools.</span></span> 

## <a name="enabling-data-access-to-the-api"></a><span data-ttu-id="0e6d5-105">Povolení přístupu k datům v rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0e6d5-105">Enabling data access to the API</span></span>
* <span data-ttu-id="0e6d5-106">**Generovat nebo načíst klíč rozhraní API** - protokolu v podnikovém portálu a postupujte podle kurzu v části Nápověda - Reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-106">**Generate or retrieve the API key** - Log in to the Enterprise portal and follow the tutorial under Help - Reporting APIs.</span></span> <span data-ttu-id="0e6d5-107">V první části v tomto článku nápovědy vysvětluje, jak vygenerovat nebo načíst klíč rozhraní API pro zadaný registrace.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-107">The first section under this help article explains how to generate or retrieve the API key for the specified enrollment.</span></span>
* <span data-ttu-id="0e6d5-108">**Předávání klíče v rozhraní API** – klíč rozhraní API musí být předán pro každé volání pro ověřování a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-108">**Passing keys in the API** - The API key needs to be passed for each call for Authentication and Authorization.</span></span> <span data-ttu-id="0e6d5-109">Následující vlastnost musí být hlavičkami protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="0e6d5-109">The following property needs to be to the HTTP headers</span></span>

|<span data-ttu-id="0e6d5-110">Klíč hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="0e6d5-110">Request Header Key</span></span> | <span data-ttu-id="0e6d5-111">Hodnota</span><span class="sxs-lookup"><span data-stu-id="0e6d5-111">Value</span></span>|
|-|-|
|<span data-ttu-id="0e6d5-112">Autorizace</span><span class="sxs-lookup"><span data-stu-id="0e6d5-112">Authorization</span></span>| <span data-ttu-id="0e6d5-113">Zadejte hodnotu v tomto formátu: **nosiče {API_KEY}**</span><span class="sxs-lookup"><span data-stu-id="0e6d5-113">Specify the value in this format: **bearer {API_KEY}**</span></span> <br/> <span data-ttu-id="0e6d5-114">Příklad: nosiče eyr... 09</span><span class="sxs-lookup"><span data-stu-id="0e6d5-114">Example: bearer eyr....09</span></span>|

## <a name="consumption-apis"></a><span data-ttu-id="0e6d5-115">Rozhraní API spotřeba</span><span class="sxs-lookup"><span data-stu-id="0e6d5-115">Consumption APIs</span></span>
<span data-ttu-id="0e6d5-116">Koncový bod Swagger je k dispozici [sem](https://consumption.azure.com/swagger/ui/index) pro rozhraní API popsané, pod kterou by měl povolit snadno introspection rozhraní API a generovat klientské sady SDK, pomocí [AutoRest](https://github.com/Azure/AutoRest) nebo [Swagger CodeGen](http://swagger.io/swagger-codegen/).</span><span class="sxs-lookup"><span data-stu-id="0e6d5-116">A Swagger endpoint is available [here](https://consumption.azure.com/swagger/ui/index) for the APIs described below which should enable easy introspection of the API and the ability to generate client SDKs using [AutoRest](https://github.com/Azure/AutoRest) or [Swagger CodeGen](http://swagger.io/swagger-codegen/).</span></span> <span data-ttu-id="0e6d5-117">Data od 1 pravděpodobně 2014 je k dispozici prostřednictvím tohoto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-117">Data beginning May 1, 2014 is available through this API.</span></span> 

* <span data-ttu-id="0e6d5-118">**Souhrn a vyrovnávat** – [vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md) nabízí měsíční souhrnné informace o zůstatky, nové nákupy, poplatky za služby Azure Marketplace, přizpůsobení a Nadlimitní poplatky.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-118">**Balance and Summary** - The [Balance and Summary API](billing-enterprise-api-balance-summary.md) offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments and overage charges.</span></span>

* <span data-ttu-id="0e6d5-119">**Podrobnosti o použití** – [podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md) nabízí denní rozpis těchto spotřebované počty a odhadované poplatky podle zápisu.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-119">**Usage Details** - The [Usage Detail API](billing-enterprise-api-usage-detail.md) offers a daily breakdown of consumed quantities and estimated charges by an Enrollment.</span></span> <span data-ttu-id="0e6d5-120">Výsledek také obsahuje informace o instancích, měřidla a oddělení.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-120">The result also includes information on instances, meters and departments.</span></span> <span data-ttu-id="0e6d5-121">Rozhraní API můžete položit dotaz na fakturační období nebo zadaný počáteční a koncové datum.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-121">The API can be queried by Billing period or by a specified start and end date.</span></span> 

* <span data-ttu-id="0e6d5-122">**Úložiště Marketplace poplatků** – [Marketplace úložiště poplatků API](billing-enterprise-api-marketplace-storecharge.md) vrátí rozdělení na základě využití marketplace poplatky za den pro zadaný fakturační období nebo počáteční a koncové datum (jednou poplatky nejsou součástí).</span><span class="sxs-lookup"><span data-stu-id="0e6d5-122">**Marketplace Store Charge** - The [Marketplace Store Charge API](billing-enterprise-api-marketplace-storecharge.md) returns the usage-based marketplace charges breakdown by day for the specified Billing Period or start and end dates (one time fees are not included).</span></span>

* <span data-ttu-id="0e6d5-123">**Ceník** – [Price Sheet API](billing-enterprise-api-pricesheet.md) umožňuje použít rychlost pro každé monitorování dané registraci a fakturační období.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-123">**Price Sheet** - The [Price Sheet API](billing-enterprise-api-pricesheet.md) provides the applicable rate for each Meter for the given Enrollment and Billing Period.</span></span> 

## <a name="helper-apis"></a><span data-ttu-id="0e6d5-124">Pomocná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0e6d5-124">Helper APIs</span></span>
 <span data-ttu-id="0e6d5-125">**Seznam fakturační období** – [fakturační období API](billing-enterprise-api-billing-periods.md) vrátí seznam hodnot fakturační období, která mají data energie pro zadanou registraci v obráceném chronologickém pořadí.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-125">**List Billing Periods** - The [Billing Periods API](billing-enterprise-api-billing-periods.md) returns a list of billing periods that have consumption data for the specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="0e6d5-126">Každé období obsahuje vlastnost odkazuje na trasu rozhraní API pro čtyři sady dat – BalanceSummary, UsageDetails, Marketplace poplatky a ceníku.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-126">Each Period contains a property pointing to the API route for the four sets of data - BalanceSummary, UsageDetails, Marketplace Charges, and Price Sheet.</span></span>


## <a name="api-response-codes"></a><span data-ttu-id="0e6d5-127">Kódy odpovědí rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0e6d5-127">API Response Codes</span></span>  
|<span data-ttu-id="0e6d5-128">Stavový kód odpovědi</span><span class="sxs-lookup"><span data-stu-id="0e6d5-128">Response Status Code</span></span>|<span data-ttu-id="0e6d5-129">Zpráva</span><span class="sxs-lookup"><span data-stu-id="0e6d5-129">Message</span></span>|<span data-ttu-id="0e6d5-130">Popis</span><span class="sxs-lookup"><span data-stu-id="0e6d5-130">Description</span></span>|
|-|-|-|
|<span data-ttu-id="0e6d5-131">200</span><span class="sxs-lookup"><span data-stu-id="0e6d5-131">200</span></span>| <span data-ttu-id="0e6d5-132">OK</span><span class="sxs-lookup"><span data-stu-id="0e6d5-132">OK</span></span>|<span data-ttu-id="0e6d5-133">Žádná chyba</span><span class="sxs-lookup"><span data-stu-id="0e6d5-133">No error</span></span>|
|<span data-ttu-id="0e6d5-134">401</span><span class="sxs-lookup"><span data-stu-id="0e6d5-134">401</span></span>| <span data-ttu-id="0e6d5-135">Neautorizováno</span><span class="sxs-lookup"><span data-stu-id="0e6d5-135">Unauthorized</span></span>| <span data-ttu-id="0e6d5-136">Klíč rozhraní API nebyl nalezen platný platnost atd.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-136">API Key not found, Invalid, Expired etc.</span></span>|
|<span data-ttu-id="0e6d5-137">404</span><span class="sxs-lookup"><span data-stu-id="0e6d5-137">404</span></span>| <span data-ttu-id="0e6d5-138">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="0e6d5-138">Unavailable</span></span>| <span data-ttu-id="0e6d5-139">Koncový bod sestavy nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="0e6d5-139">Report endpoint not found</span></span>|
|<span data-ttu-id="0e6d5-140">400</span><span class="sxs-lookup"><span data-stu-id="0e6d5-140">400</span></span>| <span data-ttu-id="0e6d5-141">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="0e6d5-141">Bad Request</span></span>| <span data-ttu-id="0e6d5-142">Neplatné parametry – rozsahy dat, EA čísel atd.</span><span class="sxs-lookup"><span data-stu-id="0e6d5-142">Invalid params – Date ranges, EA numbers etc.</span></span>|
|<span data-ttu-id="0e6d5-143">500</span><span class="sxs-lookup"><span data-stu-id="0e6d5-143">500</span></span>| <span data-ttu-id="0e6d5-144">Chyba serveru</span><span class="sxs-lookup"><span data-stu-id="0e6d5-144">Server Error</span></span>| <span data-ttu-id="0e6d5-145">Unexoected při zpracování žádosti</span><span class="sxs-lookup"><span data-stu-id="0e6d5-145">Unexoected error processing request</span></span>| 









