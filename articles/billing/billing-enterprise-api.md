---
title: "aaaAzure fakturace Enterprise rozhraní API | Microsoft Docs"
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
ms.openlocfilehash: 017cecc57ad6bdeb402b5d9d57fc95df9b033a42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a><span data-ttu-id="3b5a3-103">Přehled rozhraní API pro vytváření sestav pro podnikové zákazníky</span><span class="sxs-lookup"><span data-stu-id="3b5a3-103">Overview of Reporting APIs for Enterprise customers</span></span>
<span data-ttu-id="3b5a3-104">Hello rozhraní API pro vytváření sestav povolit Enterprise Azure zákazníků tooprogrammatically vyžádání využívání a fakturační údaje do upřednostňovaných dat nástrojů pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-104">hello Reporting APIs enable Enterprise Azure customers tooprogrammatically pull consumption and billing data into preferred data analysis tools.</span></span> 

## <a name="enabling-data-access-toohello-api"></a><span data-ttu-id="3b5a3-105">Povolení toohello API služby data access</span><span class="sxs-lookup"><span data-stu-id="3b5a3-105">Enabling data access toohello API</span></span>
* <span data-ttu-id="3b5a3-106">**Generovat nebo načíst klíč hello rozhraní API** - protokolu v toohello Enterprise portal a postupujte podle hello kurzu v části Nápověda - Reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-106">**Generate or retrieve hello API key** - Log in toohello Enterprise portal and follow hello tutorial under Help - Reporting APIs.</span></span> <span data-ttu-id="3b5a3-107">první část Hello podle tohoto článku nápovědy vysvětluje, jak zadat toogenerate nebo načíst klíč hello rozhraní API pro hello registrace.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-107">hello first section under this help article explains how toogenerate or retrieve hello API key for hello specified enrollment.</span></span>
* <span data-ttu-id="3b5a3-108">**Předávání klíče v hello rozhraní API** -hello API klíč musí toobe předaná pro každé volání pro ověřování a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-108">**Passing keys in hello API** - hello API key needs toobe passed for each call for Authentication and Authorization.</span></span> <span data-ttu-id="3b5a3-109">Hello následující vlastnost potřebuje toobe toohello HTTP hlavičky</span><span class="sxs-lookup"><span data-stu-id="3b5a3-109">hello following property needs toobe toohello HTTP headers</span></span>

|<span data-ttu-id="3b5a3-110">Klíč hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="3b5a3-110">Request Header Key</span></span> | <span data-ttu-id="3b5a3-111">Hodnota</span><span class="sxs-lookup"><span data-stu-id="3b5a3-111">Value</span></span>|
|-|-|
|<span data-ttu-id="3b5a3-112">Autorizace</span><span class="sxs-lookup"><span data-stu-id="3b5a3-112">Authorization</span></span>| <span data-ttu-id="3b5a3-113">Zadejte hodnotu hello v tomto formátu: **nosiče {API_KEY}**</span><span class="sxs-lookup"><span data-stu-id="3b5a3-113">Specify hello value in this format: **bearer {API_KEY}**</span></span> <br/> <span data-ttu-id="3b5a3-114">Příklad: nosiče eyr... 09</span><span class="sxs-lookup"><span data-stu-id="3b5a3-114">Example: bearer eyr....09</span></span>|

## <a name="consumption-apis"></a><span data-ttu-id="3b5a3-115">Rozhraní API spotřeba</span><span class="sxs-lookup"><span data-stu-id="3b5a3-115">Consumption APIs</span></span>
<span data-ttu-id="3b5a3-116">Koncový bod Swagger je k dispozici [sem](https://consumption.azure.com/swagger/ui/index) pro hello rozhraní API popsané, pod kterou by měl povolit snadno introspection hello rozhraní API a hello možnost toogenerate klientskou sadu SDK pomocí [AutoRest](https://github.com/Azure/AutoRest) nebo [ Swagger CodeGen](http://swagger.io/swagger-codegen/).</span><span class="sxs-lookup"><span data-stu-id="3b5a3-116">A Swagger endpoint is available [here](https://consumption.azure.com/swagger/ui/index) for hello APIs described below which should enable easy introspection of hello API and hello ability toogenerate client SDKs using [AutoRest](https://github.com/Azure/AutoRest) or [Swagger CodeGen](http://swagger.io/swagger-codegen/).</span></span> <span data-ttu-id="3b5a3-117">Data od 1 pravděpodobně 2014 je k dispozici prostřednictvím tohoto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-117">Data beginning May 1, 2014 is available through this API.</span></span> 

* <span data-ttu-id="3b5a3-118">**Souhrn a vyrovnávat** – hello [vyrovnávat a souhrn rozhraní API](billing-enterprise-api-balance-summary.md) nabízí měsíční souhrnné informace o zůstatky, nové nákupy, poplatky za služby Azure Marketplace, přizpůsobení a Nadlimitní poplatky.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-118">**Balance and Summary** - hello [Balance and Summary API](billing-enterprise-api-balance-summary.md) offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments and overage charges.</span></span>

* <span data-ttu-id="3b5a3-119">**Podrobnosti o použití** – hello [podrobnosti o použití rozhraní API](billing-enterprise-api-usage-detail.md) nabízí denní rozpis těchto spotřebované počty a odhadované poplatky podle zápisu.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-119">**Usage Details** - hello [Usage Detail API](billing-enterprise-api-usage-detail.md) offers a daily breakdown of consumed quantities and estimated charges by an Enrollment.</span></span> <span data-ttu-id="3b5a3-120">výsledek Hello také obsahuje informace o instancích, měřidla a oddělení.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-120">hello result also includes information on instances, meters and departments.</span></span> <span data-ttu-id="3b5a3-121">Hello rozhraní API můžete položit dotaz na fakturační období nebo zadaný počáteční a koncové datum.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-121">hello API can be queried by Billing period or by a specified start and end date.</span></span> 

* <span data-ttu-id="3b5a3-122">**Úložiště Marketplace poplatků** – hello [Marketplace úložiště poplatků API](billing-enterprise-api-marketplace-storecharge.md) vrací hello na základě využití marketplace poplatky rozpis podle dne pro hello zadané fakturační období nebo počáteční a koncové datum (jednou poplatky nejsou součástí) .</span><span class="sxs-lookup"><span data-stu-id="3b5a3-122">**Marketplace Store Charge** - hello [Marketplace Store Charge API](billing-enterprise-api-marketplace-storecharge.md) returns hello usage-based marketplace charges breakdown by day for hello specified Billing Period or start and end dates (one time fees are not included).</span></span>

* <span data-ttu-id="3b5a3-123">**Ceník** – hello [Price Sheet API](billing-enterprise-api-pricesheet.md) poskytuje hello použít rychlost pro každé monitorování pro hello daného registrace a fakturační období.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-123">**Price Sheet** - hello [Price Sheet API](billing-enterprise-api-pricesheet.md) provides hello applicable rate for each Meter for hello given Enrollment and Billing Period.</span></span> 

## <a name="helper-apis"></a><span data-ttu-id="3b5a3-124">Pomocná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3b5a3-124">Helper APIs</span></span>
 <span data-ttu-id="3b5a3-125">**Seznam fakturační období** – hello [fakturační období API](billing-enterprise-api-billing-periods.md) vrátí seznam hodnot fakturační období, které mají datům o spotřebě pro hello zadané v chronologickém pořadí zpětného zápisu.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-125">**List Billing Periods** - hello [Billing Periods API](billing-enterprise-api-billing-periods.md) returns a list of billing periods that have consumption data for hello specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="3b5a3-126">Každé období obsahuje vlastnost odkazující toohello trasu rozhraní API pro hello čtyři sady dat – BalanceSummary, UsageDetails, Marketplace poplatky a ceníku.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-126">Each Period contains a property pointing toohello API route for hello four sets of data - BalanceSummary, UsageDetails, Marketplace Charges, and Price Sheet.</span></span>


## <a name="api-response-codes"></a><span data-ttu-id="3b5a3-127">Kódy odpovědí rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3b5a3-127">API Response Codes</span></span>  
|<span data-ttu-id="3b5a3-128">Stavový kód odpovědi</span><span class="sxs-lookup"><span data-stu-id="3b5a3-128">Response Status Code</span></span>|<span data-ttu-id="3b5a3-129">Zpráva</span><span class="sxs-lookup"><span data-stu-id="3b5a3-129">Message</span></span>|<span data-ttu-id="3b5a3-130">Popis</span><span class="sxs-lookup"><span data-stu-id="3b5a3-130">Description</span></span>|
|-|-|-|
|<span data-ttu-id="3b5a3-131">200</span><span class="sxs-lookup"><span data-stu-id="3b5a3-131">200</span></span>| <span data-ttu-id="3b5a3-132">OK</span><span class="sxs-lookup"><span data-stu-id="3b5a3-132">OK</span></span>|<span data-ttu-id="3b5a3-133">Žádná chyba</span><span class="sxs-lookup"><span data-stu-id="3b5a3-133">No error</span></span>|
|<span data-ttu-id="3b5a3-134">401</span><span class="sxs-lookup"><span data-stu-id="3b5a3-134">401</span></span>| <span data-ttu-id="3b5a3-135">Neautorizováno</span><span class="sxs-lookup"><span data-stu-id="3b5a3-135">Unauthorized</span></span>| <span data-ttu-id="3b5a3-136">Klíč rozhraní API nebyl nalezen platný platnost atd.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-136">API Key not found, Invalid, Expired etc.</span></span>|
|<span data-ttu-id="3b5a3-137">404</span><span class="sxs-lookup"><span data-stu-id="3b5a3-137">404</span></span>| <span data-ttu-id="3b5a3-138">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3b5a3-138">Unavailable</span></span>| <span data-ttu-id="3b5a3-139">Koncový bod sestavy nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="3b5a3-139">Report endpoint not found</span></span>|
|<span data-ttu-id="3b5a3-140">400</span><span class="sxs-lookup"><span data-stu-id="3b5a3-140">400</span></span>| <span data-ttu-id="3b5a3-141">Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="3b5a3-141">Bad Request</span></span>| <span data-ttu-id="3b5a3-142">Neplatné parametry – rozsahy dat, EA čísel atd.</span><span class="sxs-lookup"><span data-stu-id="3b5a3-142">Invalid params – Date ranges, EA numbers etc.</span></span>|
|<span data-ttu-id="3b5a3-143">500</span><span class="sxs-lookup"><span data-stu-id="3b5a3-143">500</span></span>| <span data-ttu-id="3b5a3-144">Chyba serveru</span><span class="sxs-lookup"><span data-stu-id="3b5a3-144">Server Error</span></span>| <span data-ttu-id="3b5a3-145">Unexoected při zpracování žádosti</span><span class="sxs-lookup"><span data-stu-id="3b5a3-145">Unexoected error processing request</span></span>| 









