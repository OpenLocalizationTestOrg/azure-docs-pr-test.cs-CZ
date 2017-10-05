---
title: "Zásady omezení přístupu služby Azure API Management | Microsoft Docs"
description: "Další informace o omezení zásady přístupu, které jsou k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 4c9991baf3fbcf3b8ea01f8dd573e2336db88b68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-access-restriction-policies"></a><span data-ttu-id="17eb6-103">Zásady omezení přístupu služby API Management</span><span class="sxs-lookup"><span data-stu-id="17eb6-103">API Management access restriction policies</span></span>
<span data-ttu-id="17eb6-104">Toto téma obsahuje odkaz pro následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="17eb6-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="17eb6-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="17eb6-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="17eb6-106"><a name="AccessRestrictionPolicies"></a>Zásady omezení přístupu</span><span class="sxs-lookup"><span data-stu-id="17eb6-106"><a name="AccessRestrictionPolicies"></a> Access restriction policies</span></span>  
  
-   <span data-ttu-id="17eb6-107">[Kontrola HTTP záhlaví](api-management-access-restriction-policies.md#CheckHTTPHeader) -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="17eb6-107">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
-   <span data-ttu-id="17eb6-108">[Omezení četnosti volání podle předplatného](api-management-access-restriction-policies.md#LimitCallRate) -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="17eb6-108">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="17eb6-109">[Omezení četnosti volání podle klíče](#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="17eb6-109">[Limit call rate by key](#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
-   <span data-ttu-id="17eb6-110">[Omezit volající IP adresy](api-management-access-restriction-policies.md#RestrictCallerIPs) -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="17eb6-110">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
-   <span data-ttu-id="17eb6-111">[Nastavení kvóty využití podle předplatného](api-management-access-restriction-policies.md#SetUsageQuota) -umožňuje vynucovat obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="17eb6-111">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="17eb6-112">[Nastavení kvóty využití podle klíče](#SetUsageQuotaByKey) -umožňuje vynucovat obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="17eb6-112">[Set usage quota by key](#SetUsageQuotaByKey) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
-   <span data-ttu-id="17eb6-113">[Ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="17eb6-113">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
##  <span data-ttu-id="17eb6-114"><a name="CheckHTTPHeader"></a>Zkontrolujte hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="17eb6-114"><a name="CheckHTTPHeader"></a> Check HTTP header</span></span>  
 <span data-ttu-id="17eb6-115">Použití `check-header` zásady vynutit, aby požadavek má zadanou hlavičku HTTP.</span><span class="sxs-lookup"><span data-stu-id="17eb6-115">Use the `check-header` policy to enforce that a request has a specified HTTP header.</span></span> <span data-ttu-id="17eb6-116">Volitelně můžete zkontrolovat zda záhlaví obsahuje konkrétní hodnotu nebo zkontrolujte rozsah povolených hodnot.</span><span class="sxs-lookup"><span data-stu-id="17eb6-116">You can optionally check to see if the header has a specific value or check for a range of allowed values.</span></span> <span data-ttu-id="17eb6-117">Pokud kontrola selže, zásady ukončí zpracování žádosti a vrátí stav kód a chybová zpráva HTTP určena v zásadách.</span><span class="sxs-lookup"><span data-stu-id="17eb6-117">If the check fails, the policy terminates request processing and returns the HTTP status code and error message specified by the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="17eb6-118">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="17eb6-118">Policy statement</span></span>  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a><span data-ttu-id="17eb6-119">Příklad</span><span class="sxs-lookup"><span data-stu-id="17eb6-119">Example</span></span>  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a><span data-ttu-id="17eb6-120">Elementy</span><span class="sxs-lookup"><span data-stu-id="17eb6-120">Elements</span></span>  
  
|<span data-ttu-id="17eb6-121">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-121">Name</span></span>|<span data-ttu-id="17eb6-122">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-122">Description</span></span>|<span data-ttu-id="17eb6-123">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-123">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="17eb6-124">Kontrola – hlavička</span><span class="sxs-lookup"><span data-stu-id="17eb6-124">check-header</span></span>|<span data-ttu-id="17eb6-125">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="17eb6-125">Root element.</span></span>|<span data-ttu-id="17eb6-126">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-126">Yes</span></span>|  
|<span data-ttu-id="17eb6-127">hodnota</span><span class="sxs-lookup"><span data-stu-id="17eb6-127">value</span></span>|<span data-ttu-id="17eb6-128">Povolená hodnota hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="17eb6-128">Allowed HTTP header value.</span></span> <span data-ttu-id="17eb6-129">Víc elementů hodnota zadaná, kontroly považuje úspěšné, pokud některá z hodnot shody.</span><span class="sxs-lookup"><span data-stu-id="17eb6-129">When multiple value elements are specified, the check is considered a success if any one of the values is a match.</span></span>|<span data-ttu-id="17eb6-130">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-130">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="17eb6-131">Atributy</span><span class="sxs-lookup"><span data-stu-id="17eb6-131">Attributes</span></span>  
  
|<span data-ttu-id="17eb6-132">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-132">Name</span></span>|<span data-ttu-id="17eb6-133">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-133">Description</span></span>|<span data-ttu-id="17eb6-134">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-134">Required</span></span>|<span data-ttu-id="17eb6-135">Výchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-135">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="17eb6-136">se nezdařila zkontrolujte chybových zpráv</span><span class="sxs-lookup"><span data-stu-id="17eb6-136">failed-check-error-message</span></span>|<span data-ttu-id="17eb6-137">Chybová zpráva pro vrátit v textu odpovědi HTTP, pokud hlavička neexistuje nebo má neplatnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-137">Error message to return in the HTTP response body if the header doesn't exist or has an invalid value.</span></span> <span data-ttu-id="17eb6-138">Tuto zprávu musí mít žádné speciální znaky správně řídicí sekvencí.</span><span class="sxs-lookup"><span data-stu-id="17eb6-138">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="17eb6-139">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-139">Yes</span></span>|<span data-ttu-id="17eb6-140">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-140">N/A</span></span>|  
|<span data-ttu-id="17eb6-141">se nezdařilo httpcode kontrola</span><span class="sxs-lookup"><span data-stu-id="17eb6-141">failed-check-httpcode</span></span>|<span data-ttu-id="17eb6-142">Kód stavu HTTP vrátit, pokud hlavička neexistuje nebo má neplatnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-142">HTTP Status code to return if the header doesn't exist or has an invalid value.</span></span>|<span data-ttu-id="17eb6-143">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-143">Yes</span></span>|<span data-ttu-id="17eb6-144">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-144">N/A</span></span>|  
|<span data-ttu-id="17eb6-145">název hlavičky</span><span class="sxs-lookup"><span data-stu-id="17eb6-145">header-name</span></span>|<span data-ttu-id="17eb6-146">Název záhlaví HTTP ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="17eb6-146">The name of the HTTP Header to check.</span></span>|<span data-ttu-id="17eb6-147">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-147">Yes</span></span>|<span data-ttu-id="17eb6-148">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-148">N/A</span></span>|  
|<span data-ttu-id="17eb6-149">Ignorovat případu</span><span class="sxs-lookup"><span data-stu-id="17eb6-149">ignore-case</span></span>|<span data-ttu-id="17eb6-150">Lze nastavit na hodnotu True nebo False.</span><span class="sxs-lookup"><span data-stu-id="17eb6-150">Can be set to True or False.</span></span> <span data-ttu-id="17eb6-151">Pokud je nastaven na hodnotu True případ se ignoruje, pokud hodnota hlavičky se porovná sadu přijatelných hodnot.</span><span class="sxs-lookup"><span data-stu-id="17eb6-151">If set to True case is ignored when the header value is compared against the set of acceptable values.</span></span>|<span data-ttu-id="17eb6-152">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-152">Yes</span></span>|<span data-ttu-id="17eb6-153">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-153">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="17eb6-154">Využití</span><span class="sxs-lookup"><span data-stu-id="17eb6-154">Usage</span></span>  
 <span data-ttu-id="17eb6-155">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="17eb6-155">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="17eb6-156">**Části zásady:** vstupní, výstupní</span><span class="sxs-lookup"><span data-stu-id="17eb6-156">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="17eb6-157">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="17eb6-157">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="17eb6-158"><a name="LimitCallRate"></a>Omezení četnosti volání podle předplatného</span><span class="sxs-lookup"><span data-stu-id="17eb6-158"><a name="LimitCallRate"></a> Limit call rate by subscription</span></span>  
 <span data-ttu-id="17eb6-159">`rate-limit` Zásada brání API nárazovým zvýšením požadavků na základě předplatného za omezením četnosti volání na zadaný počet za zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="17eb6-159">The `rate-limit` policy prevents API usage spikes on a per subscription basis by limiting the call rate to a specified number per a specified time period.</span></span> <span data-ttu-id="17eb6-160">Když se aktivuje tuto zásadu volající obdrží `429 Too Many Requests` stavový kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="17eb6-160">When this policy is triggered the caller receives a `429 Too Many Requests` response status code.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="17eb6-161">Tuto zásadu lze použít jenom jednou za zásady dokumentu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-161">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="17eb6-162">[Výrazy zásad](api-management-policy-expressions.md) nelze použít v některém z atributy zásad pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-162">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="17eb6-163">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="17eb6-163">Policy statement</span></span>  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a><span data-ttu-id="17eb6-164">Příklad</span><span class="sxs-lookup"><span data-stu-id="17eb6-164">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="17eb6-165">Elementy</span><span class="sxs-lookup"><span data-stu-id="17eb6-165">Elements</span></span>  
  
|<span data-ttu-id="17eb6-166">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-166">Name</span></span>|<span data-ttu-id="17eb6-167">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-167">Description</span></span>|<span data-ttu-id="17eb6-168">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-168">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="17eb6-169">Nastavte limit</span><span class="sxs-lookup"><span data-stu-id="17eb6-169">set-limit</span></span>|<span data-ttu-id="17eb6-170">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="17eb6-170">Root element.</span></span>|<span data-ttu-id="17eb6-171">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-171">Yes</span></span>|  
|<span data-ttu-id="17eb6-172">rozhraní api</span><span class="sxs-lookup"><span data-stu-id="17eb6-172">api</span></span>|<span data-ttu-id="17eb6-173">Přidáte jeden nebo víc z těchto prvků do volání míru omezení na rozhraní API v rámci produktu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-173">Add one  or more of these elements to impose a call rate limit on APIs within the product.</span></span> <span data-ttu-id="17eb6-174">Produkt a rozhraní API volat rychlost, kterou omezení platí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="17eb6-174">Product and API call rate limits are applied independently.</span></span>|<span data-ttu-id="17eb6-175">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-175">No</span></span>|  
|<span data-ttu-id="17eb6-176">operace</span><span class="sxs-lookup"><span data-stu-id="17eb6-176">operation</span></span>|<span data-ttu-id="17eb6-177">Přidáte jeden nebo víc z těchto elementů k omezení na rychlost volání operace v rámci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="17eb6-177">Add one  or more of these elements to impose a call rate limit on operations within an API.</span></span> <span data-ttu-id="17eb6-178">Produktu, rozhraní API a operace četnosti, kterou omezení platí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="17eb6-178">Product, API, and operation call rate limits are applied independently.</span></span>|<span data-ttu-id="17eb6-179">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-179">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="17eb6-180">Atributy</span><span class="sxs-lookup"><span data-stu-id="17eb6-180">Attributes</span></span>  
  
|<span data-ttu-id="17eb6-181">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-181">Name</span></span>|<span data-ttu-id="17eb6-182">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-182">Description</span></span>|<span data-ttu-id="17eb6-183">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-183">Required</span></span>|<span data-ttu-id="17eb6-184">Výchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-184">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="17eb6-185">jméno</span><span class="sxs-lookup"><span data-stu-id="17eb6-185">name</span></span>|<span data-ttu-id="17eb6-186">Název rozhraní API, pro které chcete použít omezení četnosti.</span><span class="sxs-lookup"><span data-stu-id="17eb6-186">The name of the API for which to apply the rate limit.</span></span>|<span data-ttu-id="17eb6-187">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-187">Yes</span></span>|<span data-ttu-id="17eb6-188">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-188">N/A</span></span>|  
|<span data-ttu-id="17eb6-189">volání</span><span class="sxs-lookup"><span data-stu-id="17eb6-189">calls</span></span>|<span data-ttu-id="17eb6-190">Maximální celkový počet volání během zadaného časového intervalu v povoleno `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="17eb6-190">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="17eb6-191">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-191">Yes</span></span>|<span data-ttu-id="17eb6-192">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-192">N/A</span></span>|  
|<span data-ttu-id="17eb6-193">období obnovení</span><span class="sxs-lookup"><span data-stu-id="17eb6-193">renewal-period</span></span>|<span data-ttu-id="17eb6-194">Doba v sekundách, po které se kvóta resetuje.</span><span class="sxs-lookup"><span data-stu-id="17eb6-194">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="17eb6-195">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-195">Yes</span></span>|<span data-ttu-id="17eb6-196">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-196">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="17eb6-197">Využití</span><span class="sxs-lookup"><span data-stu-id="17eb6-197">Usage</span></span>  
 <span data-ttu-id="17eb6-198">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="17eb6-198">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="17eb6-199">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-199">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="17eb6-200">**Zásady obory:** produktu</span><span class="sxs-lookup"><span data-stu-id="17eb6-200">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="17eb6-201"><a name="LimitCallRateByKey"></a>Omezení četnosti volání podle klíče</span><span class="sxs-lookup"><span data-stu-id="17eb6-201"><a name="LimitCallRateByKey"></a> Limit call rate by key</span></span>  
 <span data-ttu-id="17eb6-202">`rate-limit-by-key` Zásada brání API nárazovým zvýšením požadavků na základě za klíč omezením četnosti volání na zadaný počet za zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="17eb6-202">The `rate-limit-by-key` policy prevents API usage spikes on a per key basis by limiting the call rate to a specified number per a specified time period.</span></span> <span data-ttu-id="17eb6-203">Klíč může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="17eb6-203">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="17eb6-204">Volitelné Přírůstek podmínku můžete přidat k určení, které požadavky mělo být započítáno směrem limit.</span><span class="sxs-lookup"><span data-stu-id="17eb6-204">Optional increment condition can be added to specify which requests should be counted towards the limit.</span></span> <span data-ttu-id="17eb6-205">Když se aktivuje tuto zásadu volající obdrží `429 Too Many Requests` stavový kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="17eb6-205">When this policy is triggered the caller receives a `429 Too Many Requests` response status code.</span></span>  
  
 <span data-ttu-id="17eb6-206">Další informace a příklady těchto zásad najdete v tématu [pokročilé omezování požadavků pomocí Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="17eb6-206">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="17eb6-207">Tuto zásadu lze použít jenom jednou za zásady dokumentu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-207">This policy can be used only once per policy document.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="17eb6-208">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="17eb6-208">Policy statement</span></span>  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="17eb6-209">Příklad</span><span class="sxs-lookup"><span data-stu-id="17eb6-209">Example</span></span>  
 <span data-ttu-id="17eb6-210">V následujícím příkladu omezení četnosti se ukládá do klíčů pomocí IP adresy volajícího.</span><span class="sxs-lookup"><span data-stu-id="17eb6-210">In the following example, the rate limit is keyed by the caller IP address.</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="17eb6-211">Elementy</span><span class="sxs-lookup"><span data-stu-id="17eb6-211">Elements</span></span>  
  
|<span data-ttu-id="17eb6-212">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-212">Name</span></span>|<span data-ttu-id="17eb6-213">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-213">Description</span></span>|<span data-ttu-id="17eb6-214">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-214">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="17eb6-215">Nastavte limit</span><span class="sxs-lookup"><span data-stu-id="17eb6-215">set-limit</span></span>|<span data-ttu-id="17eb6-216">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="17eb6-216">Root element.</span></span>|<span data-ttu-id="17eb6-217">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-217">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="17eb6-218">Atributy</span><span class="sxs-lookup"><span data-stu-id="17eb6-218">Attributes</span></span>  
  
|<span data-ttu-id="17eb6-219">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-219">Name</span></span>|<span data-ttu-id="17eb6-220">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-220">Description</span></span>|<span data-ttu-id="17eb6-221">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-221">Required</span></span>|<span data-ttu-id="17eb6-222">Výchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-222">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="17eb6-223">volání</span><span class="sxs-lookup"><span data-stu-id="17eb6-223">calls</span></span>|<span data-ttu-id="17eb6-224">Maximální celkový počet volání během zadaného časového intervalu v povoleno `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="17eb6-224">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="17eb6-225">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-225">Yes</span></span>|<span data-ttu-id="17eb6-226">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-226">N/A</span></span>|  
|<span data-ttu-id="17eb6-227">kontrolní klíč</span><span class="sxs-lookup"><span data-stu-id="17eb6-227">counter-key</span></span>|<span data-ttu-id="17eb6-228">Klíč pro zásady omezení četnosti.</span><span class="sxs-lookup"><span data-stu-id="17eb6-228">The key to use for the rate limit policy.</span></span>|<span data-ttu-id="17eb6-229">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-229">Yes</span></span>|<span data-ttu-id="17eb6-230">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-230">N/A</span></span>|  
|<span data-ttu-id="17eb6-231">Increment – podmínky</span><span class="sxs-lookup"><span data-stu-id="17eb6-231">increment-condition</span></span>|<span data-ttu-id="17eb6-232">Logický výraz, který zadáte, pokud požadavek by měl počítat směrem kvótu (`true`).</span><span class="sxs-lookup"><span data-stu-id="17eb6-232">The boolean expression specifying if the request should be counted towards the quota (`true`).</span></span>|<span data-ttu-id="17eb6-233">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-233">No</span></span>|<span data-ttu-id="17eb6-234">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-234">N/A</span></span>|  
|<span data-ttu-id="17eb6-235">období obnovení</span><span class="sxs-lookup"><span data-stu-id="17eb6-235">renewal-period</span></span>|<span data-ttu-id="17eb6-236">Doba v sekundách, po které se kvóta resetuje.</span><span class="sxs-lookup"><span data-stu-id="17eb6-236">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="17eb6-237">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-237">Yes</span></span>|<span data-ttu-id="17eb6-238">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-238">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="17eb6-239">Využití</span><span class="sxs-lookup"><span data-stu-id="17eb6-239">Usage</span></span>  
 <span data-ttu-id="17eb6-240">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="17eb6-240">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="17eb6-241">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-241">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="17eb6-242">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="17eb6-242">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="17eb6-243"><a name="RestrictCallerIPs"></a>Omezit volající IP adresy</span><span class="sxs-lookup"><span data-stu-id="17eb6-243"><a name="RestrictCallerIPs"></a> Restrict caller IPs</span></span>  
 <span data-ttu-id="17eb6-244">`ip-filter` Zásad filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="17eb6-244">The `ip-filter` policy filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="17eb6-245">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="17eb6-245">Policy statement</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a><span data-ttu-id="17eb6-246">Příklad</span><span class="sxs-lookup"><span data-stu-id="17eb6-246">Example</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a><span data-ttu-id="17eb6-247">Elementy</span><span class="sxs-lookup"><span data-stu-id="17eb6-247">Elements</span></span>  
  
|<span data-ttu-id="17eb6-248">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-248">Name</span></span>|<span data-ttu-id="17eb6-249">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-249">Description</span></span>|<span data-ttu-id="17eb6-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-250">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="17eb6-251">Filtr IP</span><span class="sxs-lookup"><span data-stu-id="17eb6-251">ip-filter</span></span>|<span data-ttu-id="17eb6-252">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="17eb6-252">Root element.</span></span>|<span data-ttu-id="17eb6-253">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-253">Yes</span></span>|  
|<span data-ttu-id="17eb6-254">Adresa</span><span class="sxs-lookup"><span data-stu-id="17eb6-254">address</span></span>|<span data-ttu-id="17eb6-255">Určuje jednu IP adresu, podle kterého chcete filtrovat.</span><span class="sxs-lookup"><span data-stu-id="17eb6-255">Specifies a single IP address on which to filter.</span></span>|<span data-ttu-id="17eb6-256">Alespoň jeden `address` nebo `address-range` prvek je nutný.</span><span class="sxs-lookup"><span data-stu-id="17eb6-256">At least one `address` or `address-range` element is required.</span></span>|  
|<span data-ttu-id="17eb6-257">rozsah adres z = "address" k = "address"</span><span class="sxs-lookup"><span data-stu-id="17eb6-257">address-range from="address" to="address"</span></span>|<span data-ttu-id="17eb6-258">Určuje rozsah IP adres, podle kterého chcete filtrovat.</span><span class="sxs-lookup"><span data-stu-id="17eb6-258">Specifies a range of IP address on which to filter.</span></span>|<span data-ttu-id="17eb6-259">Alespoň jeden `address` nebo `address-range` prvek je nutný.</span><span class="sxs-lookup"><span data-stu-id="17eb6-259">At least one `address` or `address-range` element is required.</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="17eb6-260">Atributy</span><span class="sxs-lookup"><span data-stu-id="17eb6-260">Attributes</span></span>  
  
|<span data-ttu-id="17eb6-261">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-261">Name</span></span>|<span data-ttu-id="17eb6-262">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-262">Description</span></span>|<span data-ttu-id="17eb6-263">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-263">Required</span></span>|<span data-ttu-id="17eb6-264">Výchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-264">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="17eb6-265">rozsah adres z = "address" k = "address"</span><span class="sxs-lookup"><span data-stu-id="17eb6-265">address-range from="address" to="address"</span></span>|<span data-ttu-id="17eb6-266">Rozsah IP adres Pokud chcete povolit nebo odepřít přístup.</span><span class="sxs-lookup"><span data-stu-id="17eb6-266">A range of IP addresses to allow or deny access for.</span></span>|<span data-ttu-id="17eb6-267">Požadováno, pokud `address-range` element se používá.</span><span class="sxs-lookup"><span data-stu-id="17eb6-267">Required when the `address-range` element is used.</span></span>|<span data-ttu-id="17eb6-268">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-268">N/A</span></span>|  
|<span data-ttu-id="17eb6-269">Filtr IP akce = "Povolit &#124; nezakazuje"</span><span class="sxs-lookup"><span data-stu-id="17eb6-269">ip-filter action="allow &#124; forbid"</span></span>|<span data-ttu-id="17eb6-270">Určuje, zda má být volání povoleno, nebo není pro zadané IP adresy a rozsahy.</span><span class="sxs-lookup"><span data-stu-id="17eb6-270">Specifies whether calls should be allowed or not for the specified IP addresses and ranges.</span></span>|<span data-ttu-id="17eb6-271">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-271">Yes</span></span>|<span data-ttu-id="17eb6-272">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="17eb6-273">Využití</span><span class="sxs-lookup"><span data-stu-id="17eb6-273">Usage</span></span>  
 <span data-ttu-id="17eb6-274">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="17eb6-274">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="17eb6-275">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-275">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="17eb6-276">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="17eb6-276">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="17eb6-277"><a name="SetUsageQuota"></a>Nastavení kvóty využití podle předplatného</span><span class="sxs-lookup"><span data-stu-id="17eb6-277"><a name="SetUsageQuota"></a> Set usage quota by subscription</span></span>  
 <span data-ttu-id="17eb6-278">`quota` Zásady vynucují obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="17eb6-278">The `quota` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="17eb6-279">Tuto zásadu lze použít jenom jednou za zásady dokumentu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-279">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="17eb6-280">[Výrazy zásad](api-management-policy-expressions.md) nelze použít v některém z atributy zásad pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-280">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="17eb6-281">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="17eb6-281">Policy statement</span></span>  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a><span data-ttu-id="17eb6-282">Příklad</span><span class="sxs-lookup"><span data-stu-id="17eb6-282">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="17eb6-283">Elementy</span><span class="sxs-lookup"><span data-stu-id="17eb6-283">Elements</span></span>  
  
|<span data-ttu-id="17eb6-284">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-284">Name</span></span>|<span data-ttu-id="17eb6-285">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-285">Description</span></span>|<span data-ttu-id="17eb6-286">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-286">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="17eb6-287">kvóta</span><span class="sxs-lookup"><span data-stu-id="17eb6-287">quota</span></span>|<span data-ttu-id="17eb6-288">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="17eb6-288">Root element.</span></span>|<span data-ttu-id="17eb6-289">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-289">Yes</span></span>|  
|<span data-ttu-id="17eb6-290">rozhraní api</span><span class="sxs-lookup"><span data-stu-id="17eb6-290">api</span></span>|<span data-ttu-id="17eb6-291">Přidáte jeden nebo víc z těchto elementů zavést kvóty na rozhraní API v rámci produktu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-291">Add one  or more of these elements to impose a quota on APIs within the product.</span></span> <span data-ttu-id="17eb6-292">Produkt a rozhraní API kvóty platí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="17eb6-292">Product and API quotas are applied independently.</span></span>|<span data-ttu-id="17eb6-293">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-293">No</span></span>|  
|<span data-ttu-id="17eb6-294">operace</span><span class="sxs-lookup"><span data-stu-id="17eb6-294">operation</span></span>|<span data-ttu-id="17eb6-295">Přidáte jeden nebo víc z těchto elementů zavést kvóty na operace v rámci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="17eb6-295">Add one  or more of these elements to impose a quota on operations within an API.</span></span> <span data-ttu-id="17eb6-296">Kvóty produktu, rozhraní API a operace platí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="17eb6-296">Product, API, and operation quotas are applied independently.</span></span>|<span data-ttu-id="17eb6-297">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-297">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="17eb6-298">Atributy</span><span class="sxs-lookup"><span data-stu-id="17eb6-298">Attributes</span></span>  
  
|<span data-ttu-id="17eb6-299">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-299">Name</span></span>|<span data-ttu-id="17eb6-300">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-300">Description</span></span>|<span data-ttu-id="17eb6-301">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-301">Required</span></span>|<span data-ttu-id="17eb6-302">Výchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-302">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="17eb6-303">jméno</span><span class="sxs-lookup"><span data-stu-id="17eb6-303">name</span></span>|<span data-ttu-id="17eb6-304">Název rozhraní API nebo operaci, pro kterou platí kvótu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-304">The name of the API or operation for which the quota applies.</span></span>|<span data-ttu-id="17eb6-305">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-305">Yes</span></span>|<span data-ttu-id="17eb6-306">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-306">N/A</span></span>|  
|<span data-ttu-id="17eb6-307">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="17eb6-307">bandwidth</span></span>|<span data-ttu-id="17eb6-308">Maximální počet kilobajtů během zadaného časového intervalu v povoleno `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="17eb6-308">The maximum total number of kilobytes allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="17eb6-309">Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.</span><span class="sxs-lookup"><span data-stu-id="17eb6-309">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="17eb6-310">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-310">N/A</span></span>|  
|<span data-ttu-id="17eb6-311">volání</span><span class="sxs-lookup"><span data-stu-id="17eb6-311">calls</span></span>|<span data-ttu-id="17eb6-312">Maximální celkový počet volání během zadaného časového intervalu v povoleno `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="17eb6-312">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="17eb6-313">Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.</span><span class="sxs-lookup"><span data-stu-id="17eb6-313">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="17eb6-314">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-314">N/A</span></span>|  
|<span data-ttu-id="17eb6-315">období obnovení</span><span class="sxs-lookup"><span data-stu-id="17eb6-315">renewal-period</span></span>|<span data-ttu-id="17eb6-316">Doba v sekundách, po které se kvóta resetuje.</span><span class="sxs-lookup"><span data-stu-id="17eb6-316">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="17eb6-317">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-317">Yes</span></span>|<span data-ttu-id="17eb6-318">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-318">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="17eb6-319">Využití</span><span class="sxs-lookup"><span data-stu-id="17eb6-319">Usage</span></span>  
 <span data-ttu-id="17eb6-320">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="17eb6-320">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="17eb6-321">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-321">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="17eb6-322">**Zásady obory:** produktu</span><span class="sxs-lookup"><span data-stu-id="17eb6-322">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="17eb6-323"><a name="SetUsageQuotaByKey"></a>Nastavení kvóty využití podle klíče</span><span class="sxs-lookup"><span data-stu-id="17eb6-323"><a name="SetUsageQuotaByKey"></a> Set usage quota by key</span></span>  
 <span data-ttu-id="17eb6-324">`quota-by-key` Zásady vynucují obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="17eb6-324">The `quota-by-key` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span> <span data-ttu-id="17eb6-325">Klíč může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="17eb6-325">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="17eb6-326">Volitelné Přírůstek podmínku můžete přidat k určení, které požadavky mělo být započítáno směrem kvótu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-326">Optional increment condition can be added to specify which requests should be counted towards the quota.</span></span>  
  
 <span data-ttu-id="17eb6-327">Další informace a příklady těchto zásad najdete v tématu [pokročilé omezování požadavků pomocí Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="17eb6-327">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="17eb6-328">Tuto zásadu lze použít jenom jednou za zásady dokumentu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-328">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="17eb6-329">[Výrazy zásad](api-management-policy-expressions.md) nelze použít v některém z atributy zásad pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-329">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="17eb6-330">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="17eb6-330">Policy statement</span></span>  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="17eb6-331">Příklad</span><span class="sxs-lookup"><span data-stu-id="17eb6-331">Example</span></span>  
 <span data-ttu-id="17eb6-332">V následujícím příkladu kvóta se ukládá do klíčů pomocí IP adresy volajícího.</span><span class="sxs-lookup"><span data-stu-id="17eb6-332">In the following example, the quota is keyed by the caller IP address.</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="17eb6-333">Elementy</span><span class="sxs-lookup"><span data-stu-id="17eb6-333">Elements</span></span>  
  
|<span data-ttu-id="17eb6-334">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-334">Name</span></span>|<span data-ttu-id="17eb6-335">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-335">Description</span></span>|<span data-ttu-id="17eb6-336">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-336">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="17eb6-337">kvóta</span><span class="sxs-lookup"><span data-stu-id="17eb6-337">quota</span></span>|<span data-ttu-id="17eb6-338">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="17eb6-338">Root element.</span></span>|<span data-ttu-id="17eb6-339">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-339">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="17eb6-340">Atributy</span><span class="sxs-lookup"><span data-stu-id="17eb6-340">Attributes</span></span>  
  
|<span data-ttu-id="17eb6-341">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-341">Name</span></span>|<span data-ttu-id="17eb6-342">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-342">Description</span></span>|<span data-ttu-id="17eb6-343">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-343">Required</span></span>|<span data-ttu-id="17eb6-344">Výchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-344">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="17eb6-345">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="17eb6-345">bandwidth</span></span>|<span data-ttu-id="17eb6-346">Maximální počet kilobajtů během zadaného časového intervalu v povoleno `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="17eb6-346">The maximum total number of kilobytes allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="17eb6-347">Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.</span><span class="sxs-lookup"><span data-stu-id="17eb6-347">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="17eb6-348">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-348">N/A</span></span>|  
|<span data-ttu-id="17eb6-349">volání</span><span class="sxs-lookup"><span data-stu-id="17eb6-349">calls</span></span>|<span data-ttu-id="17eb6-350">Maximální celkový počet volání během zadaného časového intervalu v povoleno `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="17eb6-350">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="17eb6-351">Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.</span><span class="sxs-lookup"><span data-stu-id="17eb6-351">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="17eb6-352">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-352">N/A</span></span>|  
|<span data-ttu-id="17eb6-353">kontrolní klíč</span><span class="sxs-lookup"><span data-stu-id="17eb6-353">counter-key</span></span>|<span data-ttu-id="17eb6-354">Klíč pro zásady kvót.</span><span class="sxs-lookup"><span data-stu-id="17eb6-354">The key to use for the quota policy.</span></span>|<span data-ttu-id="17eb6-355">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-355">Yes</span></span>|<span data-ttu-id="17eb6-356">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-356">N/A</span></span>|  
|<span data-ttu-id="17eb6-357">Increment – podmínky</span><span class="sxs-lookup"><span data-stu-id="17eb6-357">increment-condition</span></span>|<span data-ttu-id="17eb6-358">Logický výraz, který zadáte, pokud požadavek by měl počítat směrem kvótu (`true`)</span><span class="sxs-lookup"><span data-stu-id="17eb6-358">The boolean expression specifying if the request should be counted towards the quota (`true`)</span></span>|<span data-ttu-id="17eb6-359">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-359">No</span></span>|<span data-ttu-id="17eb6-360">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-360">N/A</span></span>|  
|<span data-ttu-id="17eb6-361">období obnovení</span><span class="sxs-lookup"><span data-stu-id="17eb6-361">renewal-period</span></span>|<span data-ttu-id="17eb6-362">Doba v sekundách, po které se kvóta resetuje.</span><span class="sxs-lookup"><span data-stu-id="17eb6-362">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="17eb6-363">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-363">Yes</span></span>|<span data-ttu-id="17eb6-364">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-364">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="17eb6-365">Využití</span><span class="sxs-lookup"><span data-stu-id="17eb6-365">Usage</span></span>  
 <span data-ttu-id="17eb6-366">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="17eb6-366">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="17eb6-367">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-367">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="17eb6-368">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="17eb6-368">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="17eb6-369"><a name="ValidateJWT"></a>Ověřit token JWT</span><span class="sxs-lookup"><span data-stu-id="17eb6-369"><a name="ValidateJWT"></a> Validate JWT</span></span>  
 <span data-ttu-id="17eb6-370">`validate-jwt` Zásady vynucují existence a platnosti token JWT extrahovat z buď zadané hlavičky protokolu HTTP nebo parametr zadaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="17eb6-370">The `validate-jwt` policy enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="17eb6-371">`validate-jwt` Zásad vyžaduje, aby `exp` není registrovaný deklarace inlcuded v tokenu JWT, pokud `require-expiration-time` atribut je zadán a nastavený na `false`.</span><span class="sxs-lookup"><span data-stu-id="17eb6-371">The `validate-jwt` policy requires that the `exp` registered claim is inlcuded in the JWT token, unless `require-expiration-time` attribute is specified and set to `false`.</span></span>  
> <span data-ttu-id="17eb6-372">`validate-jwt` Zásad podporuje HS256 a RS256 podpisový algoritmy.</span><span class="sxs-lookup"><span data-stu-id="17eb6-372">The `validate-jwt` policy supports HS256 and RS256 signing algorithms.</span></span> <span data-ttu-id="17eb6-373">Pro HS256 klíč je třeba zadat vložené v rámci zásady v podobě kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="17eb6-373">For HS256 the key must be provided inline within the policy in the base64 encoded form.</span></span> <span data-ttu-id="17eb6-374">RS256 klíč má zajistit prostřednictvím koncového bodu Open ID konfigurace.</span><span class="sxs-lookup"><span data-stu-id="17eb6-374">For RS256 the key has to be provide via an Open ID configuration endpoint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="17eb6-375">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="17eb6-375">Policy statement</span></span>  
  
```xml  
<validate-jwt   
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"   
    failed-validation-httpcode="http status code to return on failure"   
    failed-validation-error-message="error message to return on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of the claim as it appears in the token" match="all|any">  
      <value>claim value as it is expected to appear in the token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="17eb6-376">Příklady</span><span class="sxs-lookup"><span data-stu-id="17eb6-376">Examples</span></span>  
  
#### <a name="azure-mobile-services-token-validation"></a><span data-ttu-id="17eb6-377">Ověření tokenu Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="17eb6-377">Azure Mobile Services token validation</span></span>  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a><span data-ttu-id="17eb6-378">Ověření tokenu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="17eb6-378">Azure Active Directory token validation</span></span>  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a><span data-ttu-id="17eb6-379">Ověření tokenu Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="17eb6-379">Azure Active Directory B2C token validation</span></span>  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-to-operations-based-on-token-claims"></a><span data-ttu-id="17eb6-380">Autorizace přístupu k operacím na základě tokenu deklarací</span><span class="sxs-lookup"><span data-stu-id="17eb6-380">Authorize access to operations based on token claims</span></span>  
 <span data-ttu-id="17eb6-381">Tento příklad ukazuje způsob použití [ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) u tokenu deklarací na základě zásad předem autorizace přístupu k operacím.</span><span class="sxs-lookup"><span data-stu-id="17eb6-381">This example shows how to use the [Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) policy to pre-authorize access to operations based on token claims.</span></span> <span data-ttu-id="17eb6-382">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 13:50.</span><span class="sxs-lookup"><span data-stu-id="17eb6-382">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 13:50.</span></span> <span data-ttu-id="17eb6-383">Rychlé převinutí vpřed do 15:00 v tématu Zásady nakonfigurované v editoru zásad a potom do 18:50 pro předvedení volání operace z portálu pro vývojáře s i bez požadované autorizační token.</span><span class="sxs-lookup"><span data-stu-id="17eb6-383">Fast forward to 15:00 to see the policies configured in the policy editor and then to 18:50 for a demonstration of calling an operation from the developer portal both with and without the required authorization token.</span></span>  
  
```xml  
<!-- Copy the following snippet into the inbound section at the api (or higher) level to pre-authorize access to operations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a><span data-ttu-id="17eb6-384">Elementy</span><span class="sxs-lookup"><span data-stu-id="17eb6-384">Elements</span></span>  
  
|<span data-ttu-id="17eb6-385">Element</span><span class="sxs-lookup"><span data-stu-id="17eb6-385">Element</span></span>|<span data-ttu-id="17eb6-386">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-386">Description</span></span>|<span data-ttu-id="17eb6-387">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-387">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="17eb6-388">ověřit token jwt</span><span class="sxs-lookup"><span data-stu-id="17eb6-388">validate-jwt</span></span>|<span data-ttu-id="17eb6-389">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="17eb6-389">Root element.</span></span>|<span data-ttu-id="17eb6-390">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-390">Yes</span></span>|  
|<span data-ttu-id="17eb6-391">cílové skupiny</span><span class="sxs-lookup"><span data-stu-id="17eb6-391">audiences</span></span>|<span data-ttu-id="17eb6-392">Obsahuje seznam deklarací identity přijatelné cílové skupiny, které mohou existovat na tokenu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-392">Contains a list of acceptable audience claims that can be present on the token.</span></span> <span data-ttu-id="17eb6-393">Pokud nejsou více hodnot cílové skupiny, pak se pokus o každé hodnotě dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje.</span><span class="sxs-lookup"><span data-stu-id="17eb6-393">If multiple audience values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span> <span data-ttu-id="17eb6-394">Je třeba zadat alespoň jednu cílovou skupinu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-394">At least one audience must be specified.</span></span>|<span data-ttu-id="17eb6-395">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-395">No</span></span>|  
|<span data-ttu-id="17eb6-396">Vystavitel podpisového klíče</span><span class="sxs-lookup"><span data-stu-id="17eb6-396">issuer-signing-keys</span></span>|<span data-ttu-id="17eb6-397">Seznam kódováním Base64 zabezpečení klíčů používaných k ověření podepsané tokeny.</span><span class="sxs-lookup"><span data-stu-id="17eb6-397">A list of Base64-encoded security keys used to validate signed tokens.</span></span> <span data-ttu-id="17eb6-398">Pokud nejsou více zabezpečení klíčů, pak se pokus o každý klíč dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje (je to užitečné pro výměnu tokenů).</span><span class="sxs-lookup"><span data-stu-id="17eb6-398">If multiple security keys are present, then each key is tried until either all are exhausted (in which case validation fails) or until one succeeds (useful for token rollover).</span></span> <span data-ttu-id="17eb6-399">Mezi klíčové prvky mít volitelný `id` atribut použitý k porovnání `kid` deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="17eb6-399">Key elements have an optional `id` attribute used to match against `kid` claim.</span></span>|<span data-ttu-id="17eb6-400">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-400">No</span></span>|  
|<span data-ttu-id="17eb6-401">vystavitele</span><span class="sxs-lookup"><span data-stu-id="17eb6-401">issuers</span></span>|<span data-ttu-id="17eb6-402">Seznam přijatelné objektů, které vydán token.</span><span class="sxs-lookup"><span data-stu-id="17eb6-402">A list of acceptable principals that issued the token.</span></span> <span data-ttu-id="17eb6-403">Pokud jsou přítomny více hodnot vystavitele, pak se pokus o každé hodnotě dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje.</span><span class="sxs-lookup"><span data-stu-id="17eb6-403">If multiple issuer values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span>|<span data-ttu-id="17eb6-404">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-404">No</span></span>|  
|<span data-ttu-id="17eb6-405">Konfigurace openid</span><span class="sxs-lookup"><span data-stu-id="17eb6-405">openid-config</span></span>|<span data-ttu-id="17eb6-406">Element použit k určení kompatibilní endpoint konfigurace Open ID, ze kterého lze získat podpisového klíče a vystavitele.</span><span class="sxs-lookup"><span data-stu-id="17eb6-406">The element used for specifying a compliant Open ID configuration endpoint from which signing keys and issuer can be obtained.</span></span>|<span data-ttu-id="17eb6-407">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-407">No</span></span>|  
|<span data-ttu-id="17eb6-408">požadované deklarace identity</span><span class="sxs-lookup"><span data-stu-id="17eb6-408">required-claims</span></span>|<span data-ttu-id="17eb6-409">Obsahuje seznam deklarací identity nacházet na tokenu mohla být považovány za platné očekávání.</span><span class="sxs-lookup"><span data-stu-id="17eb6-409">Contains a list of claims expected to be present on the token for it to be considered valid.</span></span> <span data-ttu-id="17eb6-410">Když `match` je atribut nastaven na `all` každá hodnota deklarace identity v zásady musí být v tokenu pro ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="17eb6-410">When the `match` attribute is set to `all` every claim value in the policy must be present in the token for validation to succeed.</span></span> <span data-ttu-id="17eb6-411">Když `match` je atribut nastaven na `any` nejméně jedna deklarace identity musí být v tokenu pro ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="17eb6-411">When the `match` attribute is set to `any` at least one claim must be present in the token for validation to succeed.</span></span>|<span data-ttu-id="17eb6-412">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-412">No</span></span>|  
|<span data-ttu-id="17eb6-413">záhlaví zumo hlavního klíče</span><span class="sxs-lookup"><span data-stu-id="17eb6-413">zumo-master-key</span></span>|<span data-ttu-id="17eb6-414">Hlavní klíč pro tokeny vydané službou Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="17eb6-414">Master key for tokens issued by Azure Mobile Services</span></span>|<span data-ttu-id="17eb6-415">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-415">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="17eb6-416">Atributy</span><span class="sxs-lookup"><span data-stu-id="17eb6-416">Attributes</span></span>  
  
|<span data-ttu-id="17eb6-417">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="17eb6-417">Name</span></span>|<span data-ttu-id="17eb6-418">Popis</span><span class="sxs-lookup"><span data-stu-id="17eb6-418">Description</span></span>|<span data-ttu-id="17eb6-419">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="17eb6-419">Required</span></span>|<span data-ttu-id="17eb6-420">Výchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-420">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="17eb6-421">hodiny zkosení</span><span class="sxs-lookup"><span data-stu-id="17eb6-421">clock-skew</span></span>|<span data-ttu-id="17eb6-422">Časový interval.</span><span class="sxs-lookup"><span data-stu-id="17eb6-422">Timespan.</span></span> <span data-ttu-id="17eb6-423">Poskytuje některé malé volně mohou v případě, že vypršení platnosti tokenu deklarací identity je k dispozici v tokenu a následuje po aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="17eb6-423">Provides some small leeway in case the token's expiration claim is present in the token and is past the current date / time.</span></span>|<span data-ttu-id="17eb6-424">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-424">No</span></span>|<span data-ttu-id="17eb6-425">0 sekund</span><span class="sxs-lookup"><span data-stu-id="17eb6-425">0 seconds</span></span>|  
|<span data-ttu-id="17eb6-426">se nezdařilo ověření chybových zpráv</span><span class="sxs-lookup"><span data-stu-id="17eb6-426">failed-validation-error-message</span></span>|<span data-ttu-id="17eb6-427">Chybová zpráva pro vrátí v textu odpovědi HTTP, pokud položka JWT neprojde ověřením.</span><span class="sxs-lookup"><span data-stu-id="17eb6-427">Error message to return in the HTTP response body if the JWT does not pass validation.</span></span> <span data-ttu-id="17eb6-428">Tuto zprávu musí mít žádné speciální znaky správně řídicí sekvencí.</span><span class="sxs-lookup"><span data-stu-id="17eb6-428">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="17eb6-429">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-429">No</span></span>|<span data-ttu-id="17eb6-430">Výchozí chybovou zprávu, závisí na ověření problém, například "JWT nejsou k dispozici."</span><span class="sxs-lookup"><span data-stu-id="17eb6-430">Default error message depends on validation issue, for example "JWT not present."</span></span>|  
|<span data-ttu-id="17eb6-431">se nezdařilo httpcode ověření</span><span class="sxs-lookup"><span data-stu-id="17eb6-431">failed-validation-httpcode</span></span>|<span data-ttu-id="17eb6-432">Kód stavu HTTP vrátit, pokud není položka JWT projít ověřením.</span><span class="sxs-lookup"><span data-stu-id="17eb6-432">HTTP Status code to return if the JWT doesn't pass validation.</span></span>|<span data-ttu-id="17eb6-433">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-433">No</span></span>|<span data-ttu-id="17eb6-434">401</span><span class="sxs-lookup"><span data-stu-id="17eb6-434">401</span></span>|  
|<span data-ttu-id="17eb6-435">název hlavičky</span><span class="sxs-lookup"><span data-stu-id="17eb6-435">header-name</span></span>|<span data-ttu-id="17eb6-436">Název záhlaví HTTP, která uchovává token.</span><span class="sxs-lookup"><span data-stu-id="17eb6-436">The name of the HTTP header holding the token.</span></span>|<span data-ttu-id="17eb6-437">Buď `header-name` nebo `query-paremeter-name` musí být zadaný; ale ne pomocí obou.</span><span class="sxs-lookup"><span data-stu-id="17eb6-437">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="17eb6-438">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-438">N/A</span></span>|  
|<span data-ttu-id="17eb6-439">id</span><span class="sxs-lookup"><span data-stu-id="17eb6-439">id</span></span>|<span data-ttu-id="17eb6-440">`id` Atributu u `key` element umožňuje zadejte řetězec, který bude odpovídat proti `kid` deklarací identity v tokenu (pokud existuje) a zjistěte, příslušný klíč k ověření podpisu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-440">The `id` attribute on the `key` element allows you to specify the string that will be matched against `kid` claim in the token (if present) to find out the appropriate key to use for signature validation.</span></span>|<span data-ttu-id="17eb6-441">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-441">No</span></span>|<span data-ttu-id="17eb6-442">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-442">N/A</span></span>|  
|<span data-ttu-id="17eb6-443">Shoda</span><span class="sxs-lookup"><span data-stu-id="17eb6-443">match</span></span>|<span data-ttu-id="17eb6-444">`match` Atributu u `claim` element určuje, zda každá hodnota deklarace identity v zásady musí být přítomen v tokenu pro ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="17eb6-444">The `match` attribute on the `claim` element specifies whether every claim value in the policy must be present in the token for validation to succeed.</span></span> <span data-ttu-id="17eb6-445">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="17eb6-445">Possible values are:</span></span><br /><br /> <span data-ttu-id="17eb6-446">-                          `all`-Každá hodnota deklarace identity v zásady musí být v tokenu pro ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="17eb6-446">-                          `all` - every claim value in the policy must be present in the token for validation to succeed.</span></span><br /><br /> <span data-ttu-id="17eb6-447">-                          `any`-Hodnota nejméně jedna deklarace identity musí být přítomen v tokenu pro ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="17eb6-447">-                          `any` - at least one claim value must be present in the token for validation to succeed.</span></span>|<span data-ttu-id="17eb6-448">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-448">No</span></span>|<span data-ttu-id="17eb6-449">Všechny</span><span class="sxs-lookup"><span data-stu-id="17eb6-449">all</span></span>|  
|<span data-ttu-id="17eb6-450">Název dotazu paremeter</span><span class="sxs-lookup"><span data-stu-id="17eb6-450">query-paremeter-name</span></span>|<span data-ttu-id="17eb6-451">Název parametr dotazu tokenem.</span><span class="sxs-lookup"><span data-stu-id="17eb6-451">The name of the the query parameter holding the token.</span></span>|<span data-ttu-id="17eb6-452">Buď `header-name` nebo `query-paremeter-name` musí být zadaný; ale ne pomocí obou.</span><span class="sxs-lookup"><span data-stu-id="17eb6-452">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="17eb6-453">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-453">N/A</span></span>|  
|<span data-ttu-id="17eb6-454">požadovat čas vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="17eb6-454">require-expiration-time</span></span>|<span data-ttu-id="17eb6-455">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="17eb6-455">Boolean.</span></span> <span data-ttu-id="17eb6-456">Určuje, zda deklaraci identity vypršení platnosti je vyžadováno v tokenu.</span><span class="sxs-lookup"><span data-stu-id="17eb6-456">Specifies whether an expiration claim is required in the token.</span></span>|<span data-ttu-id="17eb6-457">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-457">No</span></span>|<span data-ttu-id="17eb6-458">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="17eb6-458">true</span></span>|
|<span data-ttu-id="17eb6-459">vyžadovat schéma</span><span class="sxs-lookup"><span data-stu-id="17eb6-459">require-scheme</span></span>|<span data-ttu-id="17eb6-460">Název tokenu scheme, například "Nosiče".</span><span class="sxs-lookup"><span data-stu-id="17eb6-460">The name of the token scheme, e.g. "Bearer".</span></span> <span data-ttu-id="17eb6-461">Když tento atribut nastavený, zásady zajistí, že zadané schéma je k dispozici v Hodnota hlavičky ověřování.</span><span class="sxs-lookup"><span data-stu-id="17eb6-461">When this attribute is set, the policy will ensure that specified scheme is present in the Authorization header value.</span></span>|<span data-ttu-id="17eb6-462">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-462">No</span></span>|<span data-ttu-id="17eb6-463">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-463">N/A</span></span>|
|<span data-ttu-id="17eb6-464">Vyžadovat podepsané tokeny</span><span class="sxs-lookup"><span data-stu-id="17eb6-464">require-signed-tokens</span></span>|<span data-ttu-id="17eb6-465">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="17eb6-465">Boolean.</span></span> <span data-ttu-id="17eb6-466">Určuje, jestli je potřeba být podepsaný token.</span><span class="sxs-lookup"><span data-stu-id="17eb6-466">Specifies whether a token is required to be signed.</span></span>|<span data-ttu-id="17eb6-467">Ne</span><span class="sxs-lookup"><span data-stu-id="17eb6-467">No</span></span>|<span data-ttu-id="17eb6-468">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="17eb6-468">true</span></span>|  
|<span data-ttu-id="17eb6-469">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="17eb6-469">url</span></span>|<span data-ttu-id="17eb6-470">Otevřete adresu URL koncového bodu ID konfigurace ze které lze získat metadata Open ID konfigurace.</span><span class="sxs-lookup"><span data-stu-id="17eb6-470">Open ID configuration endpoint URL from where Open ID configuration metadata can be obtained.</span></span> <span data-ttu-id="17eb6-471">Pro Azure Active Directory použít následující adresu URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` nahraďte název vašeho adresáře klienta, například `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="17eb6-471">For Azure Active Directory use the following URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` substituting your directory tenant name, e.g. `contoso.onmicrosoft.com`.</span></span>|<span data-ttu-id="17eb6-472">Ano</span><span class="sxs-lookup"><span data-stu-id="17eb6-472">Yes</span></span>|<span data-ttu-id="17eb6-473">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="17eb6-473">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="17eb6-474">Využití</span><span class="sxs-lookup"><span data-stu-id="17eb6-474">Usage</span></span>  
 <span data-ttu-id="17eb6-475">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="17eb6-475">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="17eb6-476">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="17eb6-476">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="17eb6-477">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="17eb6-477">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="17eb6-478">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17eb6-478">Next steps</span></span>
<span data-ttu-id="17eb6-479">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="17eb6-479">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
