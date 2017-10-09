---
title: "zásady omezení přístupu aaaAzure API Management | Microsoft Docs"
description: "Další informace o zásadách omezení přístupu hello k dispozici pro použití v Azure API Management."
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
ms.openlocfilehash: 0ef368c2781d9a5cf9eaaa41a47489c904ed3198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-access-restriction-policies"></a><span data-ttu-id="8592a-103">Zásady omezení přístupu služby API Management</span><span class="sxs-lookup"><span data-stu-id="8592a-103">API Management access restriction policies</span></span>
<span data-ttu-id="8592a-104">Toto téma obsahuje odkaz pro hello následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="8592a-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="8592a-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="8592a-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="8592a-106"><a name="AccessRestrictionPolicies"></a>Zásady omezení přístupu</span><span class="sxs-lookup"><span data-stu-id="8592a-106"><a name="AccessRestrictionPolicies"></a> Access restriction policies</span></span>  
  
-   <span data-ttu-id="8592a-107">[Kontrola HTTP záhlaví](api-management-access-restriction-policies.md#CheckHTTPHeader) -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8592a-107">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
-   <span data-ttu-id="8592a-108">[Omezení četnosti volání podle předplatného](api-management-access-restriction-policies.md#LimitCallRate) -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="8592a-108">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="8592a-109">[Omezení četnosti volání podle klíče](#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="8592a-109">[Limit call rate by key](#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
-   <span data-ttu-id="8592a-110">[Omezit volající IP adresy](api-management-access-restriction-policies.md#RestrictCallerIPs) -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="8592a-110">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
-   <span data-ttu-id="8592a-111">[Nastavení kvóty využití podle předplatného](api-management-access-restriction-policies.md#SetUsageQuota) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="8592a-111">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="8592a-112">[Nastavení kvóty využití podle klíče](#SetUsageQuotaByKey) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="8592a-112">[Set usage quota by key](#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
-   <span data-ttu-id="8592a-113">[Ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="8592a-113">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
##  <span data-ttu-id="8592a-114"><a name="CheckHTTPHeader"></a>Zkontrolujte hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="8592a-114"><a name="CheckHTTPHeader"></a> Check HTTP header</span></span>  
 <span data-ttu-id="8592a-115">Použití hello `check-header` tooenforce zásady, že požadavek je zadaná hlavička protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8592a-115">Use hello `check-header` policy tooenforce that a request has a specified HTTP header.</span></span> <span data-ttu-id="8592a-116">Volitelně můžete zkontrolovat toosee Pokud hlavička hello obsahuje konkrétní hodnotu nebo zkontrolujte rozsah povolených hodnot.</span><span class="sxs-lookup"><span data-stu-id="8592a-116">You can optionally check toosee if hello header has a specific value or check for a range of allowed values.</span></span> <span data-ttu-id="8592a-117">Pokud kontrola hello selže, zásady hello ukončí zpracování žádosti a vrátí hello HTTP stavový kód a chybové zprávy, určeného hello zásad.</span><span class="sxs-lookup"><span data-stu-id="8592a-117">If hello check fails, hello policy terminates request processing and returns hello HTTP status code and error message specified by hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8592a-118">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="8592a-118">Policy statement</span></span>  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a><span data-ttu-id="8592a-119">Příklad</span><span class="sxs-lookup"><span data-stu-id="8592a-119">Example</span></span>  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a><span data-ttu-id="8592a-120">Elementy</span><span class="sxs-lookup"><span data-stu-id="8592a-120">Elements</span></span>  
  
|<span data-ttu-id="8592a-121">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-121">Name</span></span>|<span data-ttu-id="8592a-122">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-122">Description</span></span>|<span data-ttu-id="8592a-123">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-123">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8592a-124">Kontrola – hlavička</span><span class="sxs-lookup"><span data-stu-id="8592a-124">check-header</span></span>|<span data-ttu-id="8592a-125">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="8592a-125">Root element.</span></span>|<span data-ttu-id="8592a-126">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-126">Yes</span></span>|  
|<span data-ttu-id="8592a-127">hodnota</span><span class="sxs-lookup"><span data-stu-id="8592a-127">value</span></span>|<span data-ttu-id="8592a-128">Povolená hodnota hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8592a-128">Allowed HTTP header value.</span></span> <span data-ttu-id="8592a-129">Když je zadaná víc elementů hodnotu, zkontrolujte hello je považován za úspěšné, pokud kterékoli z hodnoty hello je nalezena shoda.</span><span class="sxs-lookup"><span data-stu-id="8592a-129">When multiple value elements are specified, hello check is considered a success if any one of hello values is a match.</span></span>|<span data-ttu-id="8592a-130">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-130">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8592a-131">Atributy</span><span class="sxs-lookup"><span data-stu-id="8592a-131">Attributes</span></span>  
  
|<span data-ttu-id="8592a-132">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-132">Name</span></span>|<span data-ttu-id="8592a-133">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-133">Description</span></span>|<span data-ttu-id="8592a-134">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-134">Required</span></span>|<span data-ttu-id="8592a-135">Výchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-135">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8592a-136">se nezdařila zkontrolujte chybových zpráv</span><span class="sxs-lookup"><span data-stu-id="8592a-136">failed-check-error-message</span></span>|<span data-ttu-id="8592a-137">Chybová zpráva tooreturn v textu odpovědi hello HTTP, pokud hlavička hello neexistuje nebo má neplatnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8592a-137">Error message tooreturn in hello HTTP response body if hello header doesn't exist or has an invalid value.</span></span> <span data-ttu-id="8592a-138">Tuto zprávu musí mít žádné speciální znaky správně řídicí sekvencí.</span><span class="sxs-lookup"><span data-stu-id="8592a-138">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="8592a-139">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-139">Yes</span></span>|<span data-ttu-id="8592a-140">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-140">N/A</span></span>|  
|<span data-ttu-id="8592a-141">se nezdařilo httpcode kontrola</span><span class="sxs-lookup"><span data-stu-id="8592a-141">failed-check-httpcode</span></span>|<span data-ttu-id="8592a-142">Stav protokolu HTTP kód tooreturn Pokud hello záhlaví neexistuje nebo má neplatnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8592a-142">HTTP Status code tooreturn if hello header doesn't exist or has an invalid value.</span></span>|<span data-ttu-id="8592a-143">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-143">Yes</span></span>|<span data-ttu-id="8592a-144">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-144">N/A</span></span>|  
|<span data-ttu-id="8592a-145">název hlavičky</span><span class="sxs-lookup"><span data-stu-id="8592a-145">header-name</span></span>|<span data-ttu-id="8592a-146">Název Hello hello toocheck hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8592a-146">hello name of hello HTTP Header toocheck.</span></span>|<span data-ttu-id="8592a-147">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-147">Yes</span></span>|<span data-ttu-id="8592a-148">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-148">N/A</span></span>|  
|<span data-ttu-id="8592a-149">Ignorovat případu</span><span class="sxs-lookup"><span data-stu-id="8592a-149">ignore-case</span></span>|<span data-ttu-id="8592a-150">Můžete nastavit, tooTrue, nebo hodnotu NEPRAVDA.</span><span class="sxs-lookup"><span data-stu-id="8592a-150">Can be set tooTrue or False.</span></span> <span data-ttu-id="8592a-151">Pokud se ignoruje velikost písmen tooTrue sady při hodnota hlavičky hello se porovná s hello sadu přijatelných hodnot.</span><span class="sxs-lookup"><span data-stu-id="8592a-151">If set tooTrue case is ignored when hello header value is compared against hello set of acceptable values.</span></span>|<span data-ttu-id="8592a-152">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-152">Yes</span></span>|<span data-ttu-id="8592a-153">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-153">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8592a-154">Využití</span><span class="sxs-lookup"><span data-stu-id="8592a-154">Usage</span></span>  
 <span data-ttu-id="8592a-155">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8592a-155">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8592a-156">**Části zásady:** vstupní, výstupní</span><span class="sxs-lookup"><span data-stu-id="8592a-156">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="8592a-157">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="8592a-157">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8592a-158"><a name="LimitCallRate"></a>Omezení četnosti volání podle předplatného</span><span class="sxs-lookup"><span data-stu-id="8592a-158"><a name="LimitCallRate"></a> Limit call rate by subscription</span></span>  
 <span data-ttu-id="8592a-159">Hello `rate-limit` zásada brání použití rozhraní API špičky na základě předplatného za omezením hello volání míra tooa zadané číslo za zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="8592a-159">hello `rate-limit` policy prevents API usage spikes on a per subscription basis by limiting hello call rate tooa specified number per a specified time period.</span></span> <span data-ttu-id="8592a-160">Když se aktivuje tuto zásadu obdrží hello volajícího `429 Too Many Requests` stavový kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8592a-160">When this policy is triggered hello caller receives a `429 Too Many Requests` response status code.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8592a-161">Tuto zásadu lze použít jenom jednou za zásady dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8592a-161">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8592a-162">[Výrazy zásad](api-management-policy-expressions.md) nelze použít v žádném hello zásad atributů pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="8592a-162">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8592a-163">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="8592a-163">Policy statement</span></span>  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a><span data-ttu-id="8592a-164">Příklad</span><span class="sxs-lookup"><span data-stu-id="8592a-164">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8592a-165">Elementy</span><span class="sxs-lookup"><span data-stu-id="8592a-165">Elements</span></span>  
  
|<span data-ttu-id="8592a-166">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-166">Name</span></span>|<span data-ttu-id="8592a-167">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-167">Description</span></span>|<span data-ttu-id="8592a-168">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-168">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8592a-169">Nastavte limit</span><span class="sxs-lookup"><span data-stu-id="8592a-169">set-limit</span></span>|<span data-ttu-id="8592a-170">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="8592a-170">Root element.</span></span>|<span data-ttu-id="8592a-171">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-171">Yes</span></span>|  
|<span data-ttu-id="8592a-172">rozhraní api</span><span class="sxs-lookup"><span data-stu-id="8592a-172">api</span></span>|<span data-ttu-id="8592a-173">Přidáte jeden nebo více z těchto elementů tooimpose omezení četnosti volání na rozhraní API v rámci produktu hello.</span><span class="sxs-lookup"><span data-stu-id="8592a-173">Add one  or more of these elements tooimpose a call rate limit on APIs within hello product.</span></span> <span data-ttu-id="8592a-174">Produkt a rozhraní API volat rychlost, kterou omezení platí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="8592a-174">Product and API call rate limits are applied independently.</span></span>|<span data-ttu-id="8592a-175">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-175">No</span></span>|  
|<span data-ttu-id="8592a-176">operace</span><span class="sxs-lookup"><span data-stu-id="8592a-176">operation</span></span>|<span data-ttu-id="8592a-177">Přidáte jeden nebo více z těchto elementů tooimpose omezení četnosti volání na operace v rámci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8592a-177">Add one  or more of these elements tooimpose a call rate limit on operations within an API.</span></span> <span data-ttu-id="8592a-178">Produktu, rozhraní API a operace četnosti, kterou omezení platí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="8592a-178">Product, API, and operation call rate limits are applied independently.</span></span>|<span data-ttu-id="8592a-179">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-179">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8592a-180">Atributy</span><span class="sxs-lookup"><span data-stu-id="8592a-180">Attributes</span></span>  
  
|<span data-ttu-id="8592a-181">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-181">Name</span></span>|<span data-ttu-id="8592a-182">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-182">Description</span></span>|<span data-ttu-id="8592a-183">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-183">Required</span></span>|<span data-ttu-id="8592a-184">Výchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-184">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8592a-185">jméno</span><span class="sxs-lookup"><span data-stu-id="8592a-185">name</span></span>|<span data-ttu-id="8592a-186">Hello název hello rozhraní API pro omezit rychlost tooapply hello.</span><span class="sxs-lookup"><span data-stu-id="8592a-186">hello name of hello API for which tooapply hello rate limit.</span></span>|<span data-ttu-id="8592a-187">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-187">Yes</span></span>|<span data-ttu-id="8592a-188">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-188">N/A</span></span>|  
|<span data-ttu-id="8592a-189">volání</span><span class="sxs-lookup"><span data-stu-id="8592a-189">calls</span></span>|<span data-ttu-id="8592a-190">maximální počet povolených během časového intervalu hello zadaný v hello volání Hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8592a-190">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8592a-191">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-191">Yes</span></span>|<span data-ttu-id="8592a-192">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-192">N/A</span></span>|  
|<span data-ttu-id="8592a-193">období obnovení</span><span class="sxs-lookup"><span data-stu-id="8592a-193">renewal-period</span></span>|<span data-ttu-id="8592a-194">Hello doba v sekundách, po které hello kvóta resetuje.</span><span class="sxs-lookup"><span data-stu-id="8592a-194">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8592a-195">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-195">Yes</span></span>|<span data-ttu-id="8592a-196">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-196">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8592a-197">Využití</span><span class="sxs-lookup"><span data-stu-id="8592a-197">Usage</span></span>  
 <span data-ttu-id="8592a-198">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8592a-198">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8592a-199">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-199">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8592a-200">**Zásady obory:** produktu</span><span class="sxs-lookup"><span data-stu-id="8592a-200">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="8592a-201"><a name="LimitCallRateByKey"></a>Omezení četnosti volání podle klíče</span><span class="sxs-lookup"><span data-stu-id="8592a-201"><a name="LimitCallRateByKey"></a> Limit call rate by key</span></span>  
 <span data-ttu-id="8592a-202">Hello `rate-limit-by-key` zásada brání použití rozhraní API špičky na základě za klíč omezením hello volání míra tooa zadané číslo za zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="8592a-202">hello `rate-limit-by-key` policy prevents API usage spikes on a per key basis by limiting hello call rate tooa specified number per a specified time period.</span></span> <span data-ttu-id="8592a-203">klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="8592a-203">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="8592a-204">Volitelné Přírůstek podmínku můžete přidat toospecify, které požadavky mělo být započítáno směrem hello limit.</span><span class="sxs-lookup"><span data-stu-id="8592a-204">Optional increment condition can be added toospecify which requests should be counted towards hello limit.</span></span> <span data-ttu-id="8592a-205">Když se aktivuje tuto zásadu obdrží hello volajícího `429 Too Many Requests` stavový kód odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8592a-205">When this policy is triggered hello caller receives a `429 Too Many Requests` response status code.</span></span>  
  
 <span data-ttu-id="8592a-206">Další informace a příklady těchto zásad najdete v tématu [pokročilé omezování požadavků pomocí Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="8592a-206">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8592a-207">Tuto zásadu lze použít jenom jednou za zásady dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8592a-207">This policy can be used only once per policy document.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8592a-208">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="8592a-208">Policy statement</span></span>  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="8592a-209">Příklad</span><span class="sxs-lookup"><span data-stu-id="8592a-209">Example</span></span>  
 <span data-ttu-id="8592a-210">V následujícím příkladu hello omezení četnosti hello se ukládá do klíčů pomocí hello volající IP adresy.</span><span class="sxs-lookup"><span data-stu-id="8592a-210">In hello following example, hello rate limit is keyed by hello caller IP address.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8592a-211">Elementy</span><span class="sxs-lookup"><span data-stu-id="8592a-211">Elements</span></span>  
  
|<span data-ttu-id="8592a-212">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-212">Name</span></span>|<span data-ttu-id="8592a-213">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-213">Description</span></span>|<span data-ttu-id="8592a-214">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-214">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8592a-215">Nastavte limit</span><span class="sxs-lookup"><span data-stu-id="8592a-215">set-limit</span></span>|<span data-ttu-id="8592a-216">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="8592a-216">Root element.</span></span>|<span data-ttu-id="8592a-217">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-217">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8592a-218">Atributy</span><span class="sxs-lookup"><span data-stu-id="8592a-218">Attributes</span></span>  
  
|<span data-ttu-id="8592a-219">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-219">Name</span></span>|<span data-ttu-id="8592a-220">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-220">Description</span></span>|<span data-ttu-id="8592a-221">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-221">Required</span></span>|<span data-ttu-id="8592a-222">Výchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-222">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8592a-223">volání</span><span class="sxs-lookup"><span data-stu-id="8592a-223">calls</span></span>|<span data-ttu-id="8592a-224">maximální počet povolených během časového intervalu hello zadaný v hello volání Hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8592a-224">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8592a-225">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-225">Yes</span></span>|<span data-ttu-id="8592a-226">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-226">N/A</span></span>|  
|<span data-ttu-id="8592a-227">kontrolní klíč</span><span class="sxs-lookup"><span data-stu-id="8592a-227">counter-key</span></span>|<span data-ttu-id="8592a-228">Hello klíče toouse pro zásady omezení četnosti hello.</span><span class="sxs-lookup"><span data-stu-id="8592a-228">hello key toouse for hello rate limit policy.</span></span>|<span data-ttu-id="8592a-229">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-229">Yes</span></span>|<span data-ttu-id="8592a-230">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-230">N/A</span></span>|  
|<span data-ttu-id="8592a-231">Increment – podmínky</span><span class="sxs-lookup"><span data-stu-id="8592a-231">increment-condition</span></span>|<span data-ttu-id="8592a-232">logický výraz Hello zadání, pokud požadavek hello mělo být započítáno směrem hello kvóty (`true`).</span><span class="sxs-lookup"><span data-stu-id="8592a-232">hello boolean expression specifying if hello request should be counted towards hello quota (`true`).</span></span>|<span data-ttu-id="8592a-233">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-233">No</span></span>|<span data-ttu-id="8592a-234">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-234">N/A</span></span>|  
|<span data-ttu-id="8592a-235">období obnovení</span><span class="sxs-lookup"><span data-stu-id="8592a-235">renewal-period</span></span>|<span data-ttu-id="8592a-236">Hello doba v sekundách, po které hello kvóta resetuje.</span><span class="sxs-lookup"><span data-stu-id="8592a-236">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8592a-237">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-237">Yes</span></span>|<span data-ttu-id="8592a-238">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-238">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8592a-239">Využití</span><span class="sxs-lookup"><span data-stu-id="8592a-239">Usage</span></span>  
 <span data-ttu-id="8592a-240">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8592a-240">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8592a-241">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-241">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8592a-242">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="8592a-242">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8592a-243"><a name="RestrictCallerIPs"></a>Omezit volající IP adresy</span><span class="sxs-lookup"><span data-stu-id="8592a-243"><a name="RestrictCallerIPs"></a> Restrict caller IPs</span></span>  
 <span data-ttu-id="8592a-244">Hello `ip-filter` zásad filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.</span><span class="sxs-lookup"><span data-stu-id="8592a-244">hello `ip-filter` policy filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8592a-245">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="8592a-245">Policy statement</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a><span data-ttu-id="8592a-246">Příklad</span><span class="sxs-lookup"><span data-stu-id="8592a-246">Example</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a><span data-ttu-id="8592a-247">Elementy</span><span class="sxs-lookup"><span data-stu-id="8592a-247">Elements</span></span>  
  
|<span data-ttu-id="8592a-248">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-248">Name</span></span>|<span data-ttu-id="8592a-249">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-249">Description</span></span>|<span data-ttu-id="8592a-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-250">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8592a-251">Filtr IP</span><span class="sxs-lookup"><span data-stu-id="8592a-251">ip-filter</span></span>|<span data-ttu-id="8592a-252">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="8592a-252">Root element.</span></span>|<span data-ttu-id="8592a-253">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-253">Yes</span></span>|  
|<span data-ttu-id="8592a-254">Adresa</span><span class="sxs-lookup"><span data-stu-id="8592a-254">address</span></span>|<span data-ttu-id="8592a-255">Určuje jednu IP adresu, na které toofilter.</span><span class="sxs-lookup"><span data-stu-id="8592a-255">Specifies a single IP address on which toofilter.</span></span>|<span data-ttu-id="8592a-256">Alespoň jeden `address` nebo `address-range` prvek je nutný.</span><span class="sxs-lookup"><span data-stu-id="8592a-256">At least one `address` or `address-range` element is required.</span></span>|  
|<span data-ttu-id="8592a-257">rozsah adres z = "address" k = "address"</span><span class="sxs-lookup"><span data-stu-id="8592a-257">address-range from="address" to="address"</span></span>|<span data-ttu-id="8592a-258">Určuje rozsah IP adres, na které toofilter.</span><span class="sxs-lookup"><span data-stu-id="8592a-258">Specifies a range of IP address on which toofilter.</span></span>|<span data-ttu-id="8592a-259">Alespoň jeden `address` nebo `address-range` prvek je nutný.</span><span class="sxs-lookup"><span data-stu-id="8592a-259">At least one `address` or `address-range` element is required.</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8592a-260">Atributy</span><span class="sxs-lookup"><span data-stu-id="8592a-260">Attributes</span></span>  
  
|<span data-ttu-id="8592a-261">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-261">Name</span></span>|<span data-ttu-id="8592a-262">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-262">Description</span></span>|<span data-ttu-id="8592a-263">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-263">Required</span></span>|<span data-ttu-id="8592a-264">Výchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-264">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8592a-265">rozsah adres z = "address" k = "address"</span><span class="sxs-lookup"><span data-stu-id="8592a-265">address-range from="address" to="address"</span></span>|<span data-ttu-id="8592a-266">Rozsah IP adres tooallow nebo odepřít přístup.</span><span class="sxs-lookup"><span data-stu-id="8592a-266">A range of IP addresses tooallow or deny access for.</span></span>|<span data-ttu-id="8592a-267">Požadováno, když hello `address-range` element se používá.</span><span class="sxs-lookup"><span data-stu-id="8592a-267">Required when hello `address-range` element is used.</span></span>|<span data-ttu-id="8592a-268">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-268">N/A</span></span>|  
|<span data-ttu-id="8592a-269">Filtr IP akce = "Povolit &#124; nezakazuje"</span><span class="sxs-lookup"><span data-stu-id="8592a-269">ip-filter action="allow &#124; forbid"</span></span>|<span data-ttu-id="8592a-270">Určuje, zda se má povolit volání, nebo není pro hello zadané IP adresy a rozsahy.</span><span class="sxs-lookup"><span data-stu-id="8592a-270">Specifies whether calls should be allowed or not for hello specified IP addresses and ranges.</span></span>|<span data-ttu-id="8592a-271">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-271">Yes</span></span>|<span data-ttu-id="8592a-272">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8592a-273">Využití</span><span class="sxs-lookup"><span data-stu-id="8592a-273">Usage</span></span>  
 <span data-ttu-id="8592a-274">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8592a-274">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8592a-275">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-275">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8592a-276">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="8592a-276">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8592a-277"><a name="SetUsageQuota"></a>Nastavení kvóty využití podle předplatného</span><span class="sxs-lookup"><span data-stu-id="8592a-277"><a name="SetUsageQuota"></a> Set usage quota by subscription</span></span>  
 <span data-ttu-id="8592a-278">Hello `quota` zásady vynucují obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.</span><span class="sxs-lookup"><span data-stu-id="8592a-278">hello `quota` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8592a-279">Tuto zásadu lze použít jenom jednou za zásady dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8592a-279">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8592a-280">[Výrazy zásad](api-management-policy-expressions.md) nelze použít v žádném hello zásad atributů pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="8592a-280">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8592a-281">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="8592a-281">Policy statement</span></span>  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a><span data-ttu-id="8592a-282">Příklad</span><span class="sxs-lookup"><span data-stu-id="8592a-282">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8592a-283">Elementy</span><span class="sxs-lookup"><span data-stu-id="8592a-283">Elements</span></span>  
  
|<span data-ttu-id="8592a-284">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-284">Name</span></span>|<span data-ttu-id="8592a-285">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-285">Description</span></span>|<span data-ttu-id="8592a-286">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-286">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8592a-287">kvóta</span><span class="sxs-lookup"><span data-stu-id="8592a-287">quota</span></span>|<span data-ttu-id="8592a-288">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="8592a-288">Root element.</span></span>|<span data-ttu-id="8592a-289">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-289">Yes</span></span>|  
|<span data-ttu-id="8592a-290">rozhraní api</span><span class="sxs-lookup"><span data-stu-id="8592a-290">api</span></span>|<span data-ttu-id="8592a-291">Přidáte jeden nebo více z těchto elementů tooimpose kvótu na rozhraní API v rámci produktu hello.</span><span class="sxs-lookup"><span data-stu-id="8592a-291">Add one  or more of these elements tooimpose a quota on APIs within hello product.</span></span> <span data-ttu-id="8592a-292">Produkt a rozhraní API kvóty platí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="8592a-292">Product and API quotas are applied independently.</span></span>|<span data-ttu-id="8592a-293">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-293">No</span></span>|  
|<span data-ttu-id="8592a-294">operace</span><span class="sxs-lookup"><span data-stu-id="8592a-294">operation</span></span>|<span data-ttu-id="8592a-295">Přidáte jeden nebo více z těchto elementů tooimpose kvótu na operace v rámci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8592a-295">Add one  or more of these elements tooimpose a quota on operations within an API.</span></span> <span data-ttu-id="8592a-296">Kvóty produktu, rozhraní API a operace platí nezávisle.</span><span class="sxs-lookup"><span data-stu-id="8592a-296">Product, API, and operation quotas are applied independently.</span></span>|<span data-ttu-id="8592a-297">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-297">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8592a-298">Atributy</span><span class="sxs-lookup"><span data-stu-id="8592a-298">Attributes</span></span>  
  
|<span data-ttu-id="8592a-299">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-299">Name</span></span>|<span data-ttu-id="8592a-300">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-300">Description</span></span>|<span data-ttu-id="8592a-301">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-301">Required</span></span>|<span data-ttu-id="8592a-302">Výchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-302">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8592a-303">jméno</span><span class="sxs-lookup"><span data-stu-id="8592a-303">name</span></span>|<span data-ttu-id="8592a-304">Hello název rozhraní API hello nebo operaci, pro které hello kvóty platí.</span><span class="sxs-lookup"><span data-stu-id="8592a-304">hello name of hello API or operation for which hello quota applies.</span></span>|<span data-ttu-id="8592a-305">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-305">Yes</span></span>|<span data-ttu-id="8592a-306">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-306">N/A</span></span>|  
|<span data-ttu-id="8592a-307">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="8592a-307">bandwidth</span></span>|<span data-ttu-id="8592a-308">Hello maximální celkový počet kilobajtů povolené během časového intervalu hello zadaný v hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8592a-308">hello maximum total number of kilobytes allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8592a-309">Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.</span><span class="sxs-lookup"><span data-stu-id="8592a-309">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8592a-310">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-310">N/A</span></span>|  
|<span data-ttu-id="8592a-311">volání</span><span class="sxs-lookup"><span data-stu-id="8592a-311">calls</span></span>|<span data-ttu-id="8592a-312">maximální počet povolených během časového intervalu hello zadaný v hello volání Hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8592a-312">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8592a-313">Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.</span><span class="sxs-lookup"><span data-stu-id="8592a-313">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8592a-314">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-314">N/A</span></span>|  
|<span data-ttu-id="8592a-315">období obnovení</span><span class="sxs-lookup"><span data-stu-id="8592a-315">renewal-period</span></span>|<span data-ttu-id="8592a-316">Hello doba v sekundách, po které hello kvóta resetuje.</span><span class="sxs-lookup"><span data-stu-id="8592a-316">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8592a-317">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-317">Yes</span></span>|<span data-ttu-id="8592a-318">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-318">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8592a-319">Využití</span><span class="sxs-lookup"><span data-stu-id="8592a-319">Usage</span></span>  
 <span data-ttu-id="8592a-320">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8592a-320">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8592a-321">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-321">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8592a-322">**Zásady obory:** produktu</span><span class="sxs-lookup"><span data-stu-id="8592a-322">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="8592a-323"><a name="SetUsageQuotaByKey"></a>Nastavení kvóty využití podle klíče</span><span class="sxs-lookup"><span data-stu-id="8592a-323"><a name="SetUsageQuotaByKey"></a> Set usage quota by key</span></span>  
 <span data-ttu-id="8592a-324">Hello `quota-by-key` zásady vynucují obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.</span><span class="sxs-lookup"><span data-stu-id="8592a-324">hello `quota-by-key` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span> <span data-ttu-id="8592a-325">klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="8592a-325">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="8592a-326">Volitelné Přírůstek podmínku můžete přidat toospecify, které požadavky mělo být započítáno směrem hello kvóty.</span><span class="sxs-lookup"><span data-stu-id="8592a-326">Optional increment condition can be added toospecify which requests should be counted towards hello quota.</span></span>  
  
 <span data-ttu-id="8592a-327">Další informace a příklady těchto zásad najdete v tématu [pokročilé omezování požadavků pomocí Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="8592a-327">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8592a-328">Tuto zásadu lze použít jenom jednou za zásady dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8592a-328">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8592a-329">[Výrazy zásad](api-management-policy-expressions.md) nelze použít v žádném hello zásad atributů pro tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="8592a-329">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8592a-330">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="8592a-330">Policy statement</span></span>  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="8592a-331">Příklad</span><span class="sxs-lookup"><span data-stu-id="8592a-331">Example</span></span>  
 <span data-ttu-id="8592a-332">V následujícím příkladu hello kvóty hello se ukládá do klíčů pomocí hello volající IP adresy.</span><span class="sxs-lookup"><span data-stu-id="8592a-332">In hello following example, hello quota is keyed by hello caller IP address.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8592a-333">Elementy</span><span class="sxs-lookup"><span data-stu-id="8592a-333">Elements</span></span>  
  
|<span data-ttu-id="8592a-334">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-334">Name</span></span>|<span data-ttu-id="8592a-335">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-335">Description</span></span>|<span data-ttu-id="8592a-336">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-336">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8592a-337">kvóta</span><span class="sxs-lookup"><span data-stu-id="8592a-337">quota</span></span>|<span data-ttu-id="8592a-338">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="8592a-338">Root element.</span></span>|<span data-ttu-id="8592a-339">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-339">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8592a-340">Atributy</span><span class="sxs-lookup"><span data-stu-id="8592a-340">Attributes</span></span>  
  
|<span data-ttu-id="8592a-341">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-341">Name</span></span>|<span data-ttu-id="8592a-342">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-342">Description</span></span>|<span data-ttu-id="8592a-343">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-343">Required</span></span>|<span data-ttu-id="8592a-344">Výchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-344">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8592a-345">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="8592a-345">bandwidth</span></span>|<span data-ttu-id="8592a-346">Hello maximální celkový počet kilobajtů povolené během časového intervalu hello zadaný v hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8592a-346">hello maximum total number of kilobytes allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8592a-347">Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.</span><span class="sxs-lookup"><span data-stu-id="8592a-347">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8592a-348">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-348">N/A</span></span>|  
|<span data-ttu-id="8592a-349">volání</span><span class="sxs-lookup"><span data-stu-id="8592a-349">calls</span></span>|<span data-ttu-id="8592a-350">maximální počet povolených během časového intervalu hello zadaný v hello volání Hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8592a-350">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8592a-351">Buď `calls`, `bandwidth`, nebo společně musí být zadány oba.</span><span class="sxs-lookup"><span data-stu-id="8592a-351">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8592a-352">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-352">N/A</span></span>|  
|<span data-ttu-id="8592a-353">kontrolní klíč</span><span class="sxs-lookup"><span data-stu-id="8592a-353">counter-key</span></span>|<span data-ttu-id="8592a-354">Hello klíče toouse hello zásady kvót.</span><span class="sxs-lookup"><span data-stu-id="8592a-354">hello key toouse for hello quota policy.</span></span>|<span data-ttu-id="8592a-355">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-355">Yes</span></span>|<span data-ttu-id="8592a-356">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-356">N/A</span></span>|  
|<span data-ttu-id="8592a-357">Increment – podmínky</span><span class="sxs-lookup"><span data-stu-id="8592a-357">increment-condition</span></span>|<span data-ttu-id="8592a-358">logický výraz Hello zadání, pokud požadavek hello mělo být započítáno směrem hello kvóty (`true`)</span><span class="sxs-lookup"><span data-stu-id="8592a-358">hello boolean expression specifying if hello request should be counted towards hello quota (`true`)</span></span>|<span data-ttu-id="8592a-359">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-359">No</span></span>|<span data-ttu-id="8592a-360">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-360">N/A</span></span>|  
|<span data-ttu-id="8592a-361">období obnovení</span><span class="sxs-lookup"><span data-stu-id="8592a-361">renewal-period</span></span>|<span data-ttu-id="8592a-362">Hello doba v sekundách, po které hello kvóta resetuje.</span><span class="sxs-lookup"><span data-stu-id="8592a-362">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8592a-363">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-363">Yes</span></span>|<span data-ttu-id="8592a-364">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-364">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8592a-365">Využití</span><span class="sxs-lookup"><span data-stu-id="8592a-365">Usage</span></span>  
 <span data-ttu-id="8592a-366">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8592a-366">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8592a-367">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-367">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8592a-368">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="8592a-368">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8592a-369"><a name="ValidateJWT"></a>Ověřit token JWT</span><span class="sxs-lookup"><span data-stu-id="8592a-369"><a name="ValidateJWT"></a> Validate JWT</span></span>  
 <span data-ttu-id="8592a-370">Hello `validate-jwt` zásady vynucují existence a platnosti token JWT extrahovat z buď zadané hlavičky protokolu HTTP nebo parametr zadaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="8592a-370">hello `validate-jwt` policy enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8592a-371">Hello `validate-jwt` zásad vyžaduje tento hello `exp` není registrovaný deklarace inlcuded v tokenu JWT hello, pokud `require-expiration-time` atribut je zadán a nastavit příliš`false`.</span><span class="sxs-lookup"><span data-stu-id="8592a-371">hello `validate-jwt` policy requires that hello `exp` registered claim is inlcuded in hello JWT token, unless `require-expiration-time` attribute is specified and set too`false`.</span></span>  
> <span data-ttu-id="8592a-372">Hello `validate-jwt` zásad podporuje HS256 a RS256 podpisový algoritmy.</span><span class="sxs-lookup"><span data-stu-id="8592a-372">hello `validate-jwt` policy supports HS256 and RS256 signing algorithms.</span></span> <span data-ttu-id="8592a-373">Pro HS256 hello klíč je třeba zadat ve vloženém hello zásady v podobě hello kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="8592a-373">For HS256 hello key must be provided inline within hello policy in hello base64 encoded form.</span></span> <span data-ttu-id="8592a-374">Pro RS256 hello klíč má toobe zadejte přes koncový bod Open ID konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8592a-374">For RS256 hello key has toobe provide via an Open ID configuration endpoint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8592a-375">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="8592a-375">Policy statement</span></span>  
  
```xml  
<validate-jwt   
    header-name="name of http header containing hello token (use query-parameter-name attribute if hello token is passed in hello URL)"   
    failed-validation-httpcode="http status code tooreturn on failure"   
    failed-validation-error-message="error message tooreturn on failure"   
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
    <claim name="name of hello claim as it appears in hello token" match="all|any">  
      <value>claim value as it is expected tooappear in hello token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of hello configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="8592a-376">Příklady</span><span class="sxs-lookup"><span data-stu-id="8592a-376">Examples</span></span>  
  
#### <a name="azure-mobile-services-token-validation"></a><span data-ttu-id="8592a-377">Ověření tokenu Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="8592a-377">Azure Mobile Services token validation</span></span>  
  
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
  
#### <a name="azure-active-directory-token-validation"></a><span data-ttu-id="8592a-378">Ověření tokenu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8592a-378">Azure Active Directory token validation</span></span>  
  
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

  
#### <a name="azure-active-directory-b2c-token-validation"></a><span data-ttu-id="8592a-379">Ověření tokenu Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="8592a-379">Azure Active Directory B2C token validation</span></span>  
  
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
  
#### <a name="authorize-access-toooperations-based-on-token-claims"></a><span data-ttu-id="8592a-380">Autorizovat toooperations přístupu na základě tokenu deklarací</span><span class="sxs-lookup"><span data-stu-id="8592a-380">Authorize access toooperations based on token claims</span></span>  
 <span data-ttu-id="8592a-381">Tento příklad ukazuje, jak toouse hello [ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) zásad toopre-toooperations přístupu na základě tokenu deklarací autorizace.</span><span class="sxs-lookup"><span data-stu-id="8592a-381">This example shows how toouse hello [Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) policy toopre-authorize access toooperations based on token claims.</span></span> <span data-ttu-id="8592a-382">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too13:50.</span><span class="sxs-lookup"><span data-stu-id="8592a-382">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too13:50.</span></span> <span data-ttu-id="8592a-383">Rychlé převinutí vpřed too15:00 toosee hello zásady nakonfigurované v editoru zásad hello, a potom too18:50 pro předvedení volání operace z portálu pro vývojáře hello s i bez hello vyžaduje autorizační token.</span><span class="sxs-lookup"><span data-stu-id="8592a-383">Fast forward too15:00 toosee hello policies configured in hello policy editor and then too18:50 for a demonstration of calling an operation from hello developer portal both with and without hello required authorization token.</span></span>  
  
```xml  
<!-- Copy hello following snippet into hello inbound section at hello api (or higher) level toopre-authorize access toooperations based on token claims -->  
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
  
### <a name="elements"></a><span data-ttu-id="8592a-384">Elementy</span><span class="sxs-lookup"><span data-stu-id="8592a-384">Elements</span></span>  
  
|<span data-ttu-id="8592a-385">Element</span><span class="sxs-lookup"><span data-stu-id="8592a-385">Element</span></span>|<span data-ttu-id="8592a-386">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-386">Description</span></span>|<span data-ttu-id="8592a-387">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-387">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="8592a-388">ověřit token jwt</span><span class="sxs-lookup"><span data-stu-id="8592a-388">validate-jwt</span></span>|<span data-ttu-id="8592a-389">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="8592a-389">Root element.</span></span>|<span data-ttu-id="8592a-390">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-390">Yes</span></span>|  
|<span data-ttu-id="8592a-391">cílové skupiny</span><span class="sxs-lookup"><span data-stu-id="8592a-391">audiences</span></span>|<span data-ttu-id="8592a-392">Obsahuje seznam přijatelné cílovou skupinu deklarací identity, které mohou existovat na hello tokenu.</span><span class="sxs-lookup"><span data-stu-id="8592a-392">Contains a list of acceptable audience claims that can be present on hello token.</span></span> <span data-ttu-id="8592a-393">Pokud nejsou více hodnot cílové skupiny, pak se pokus o každé hodnotě dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje.</span><span class="sxs-lookup"><span data-stu-id="8592a-393">If multiple audience values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span> <span data-ttu-id="8592a-394">Je třeba zadat alespoň jednu cílovou skupinu.</span><span class="sxs-lookup"><span data-stu-id="8592a-394">At least one audience must be specified.</span></span>|<span data-ttu-id="8592a-395">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-395">No</span></span>|  
|<span data-ttu-id="8592a-396">Vystavitel podpisového klíče</span><span class="sxs-lookup"><span data-stu-id="8592a-396">issuer-signing-keys</span></span>|<span data-ttu-id="8592a-397">Seznam kódování Base64 zabezpečení klíčů používaných toovalidate podepsané tokeny.</span><span class="sxs-lookup"><span data-stu-id="8592a-397">A list of Base64-encoded security keys used toovalidate signed tokens.</span></span> <span data-ttu-id="8592a-398">Pokud nejsou více zabezpečení klíčů, pak se pokus o každý klíč dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje (je to užitečné pro výměnu tokenů).</span><span class="sxs-lookup"><span data-stu-id="8592a-398">If multiple security keys are present, then each key is tried until either all are exhausted (in which case validation fails) or until one succeeds (useful for token rollover).</span></span> <span data-ttu-id="8592a-399">Mezi klíčové prvky mít volitelný `id` toomatch atribut použitý proti `kid` deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="8592a-399">Key elements have an optional `id` attribute used toomatch against `kid` claim.</span></span>|<span data-ttu-id="8592a-400">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-400">No</span></span>|  
|<span data-ttu-id="8592a-401">vystavitele</span><span class="sxs-lookup"><span data-stu-id="8592a-401">issuers</span></span>|<span data-ttu-id="8592a-402">Seznam přijatelné objektů, které vydán hello token.</span><span class="sxs-lookup"><span data-stu-id="8592a-402">A list of acceptable principals that issued hello token.</span></span> <span data-ttu-id="8592a-403">Pokud jsou přítomny více hodnot vystavitele, pak se pokus o každé hodnotě dokud všechny jsou vyčerpány (v takovém případě ověření selže) nebo dokud jeden neuspěje.</span><span class="sxs-lookup"><span data-stu-id="8592a-403">If multiple issuer values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span>|<span data-ttu-id="8592a-404">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-404">No</span></span>|  
|<span data-ttu-id="8592a-405">Konfigurace openid</span><span class="sxs-lookup"><span data-stu-id="8592a-405">openid-config</span></span>|<span data-ttu-id="8592a-406">element Hello použit k určení kompatibilní endpoint konfigurace Open ID, ze kterého lze získat podpisového klíče a vystavitele.</span><span class="sxs-lookup"><span data-stu-id="8592a-406">hello element used for specifying a compliant Open ID configuration endpoint from which signing keys and issuer can be obtained.</span></span>|<span data-ttu-id="8592a-407">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-407">No</span></span>|  
|<span data-ttu-id="8592a-408">požadované deklarace identity</span><span class="sxs-lookup"><span data-stu-id="8592a-408">required-claims</span></span>|<span data-ttu-id="8592a-409">Obsahuje seznam deklarací identity očekává toobe na hello token k dispozici pro jeho toobe považován za platný.</span><span class="sxs-lookup"><span data-stu-id="8592a-409">Contains a list of claims expected toobe present on hello token for it toobe considered valid.</span></span> <span data-ttu-id="8592a-410">Když hello `match` atribut je nastaven příliš`all` každá hodnota deklarace identity v hello zásady musí být součástí hello token pro toosucceed ověření.</span><span class="sxs-lookup"><span data-stu-id="8592a-410">When hello `match` attribute is set too`all` every claim value in hello policy must be present in hello token for validation toosucceed.</span></span> <span data-ttu-id="8592a-411">Když hello `match` atribut je nastaven příliš`any` nejméně jedna deklarace identity musí být k dispozici v tokenu hello pro toosucceed ověření.</span><span class="sxs-lookup"><span data-stu-id="8592a-411">When hello `match` attribute is set too`any` at least one claim must be present in hello token for validation toosucceed.</span></span>|<span data-ttu-id="8592a-412">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-412">No</span></span>|  
|<span data-ttu-id="8592a-413">záhlaví zumo hlavního klíče</span><span class="sxs-lookup"><span data-stu-id="8592a-413">zumo-master-key</span></span>|<span data-ttu-id="8592a-414">Hlavní klíč pro tokeny vydané službou Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="8592a-414">Master key for tokens issued by Azure Mobile Services</span></span>|<span data-ttu-id="8592a-415">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-415">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8592a-416">Atributy</span><span class="sxs-lookup"><span data-stu-id="8592a-416">Attributes</span></span>  
  
|<span data-ttu-id="8592a-417">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="8592a-417">Name</span></span>|<span data-ttu-id="8592a-418">Popis</span><span class="sxs-lookup"><span data-stu-id="8592a-418">Description</span></span>|<span data-ttu-id="8592a-419">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="8592a-419">Required</span></span>|<span data-ttu-id="8592a-420">Výchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-420">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8592a-421">hodiny zkosení</span><span class="sxs-lookup"><span data-stu-id="8592a-421">clock-skew</span></span>|<span data-ttu-id="8592a-422">Časový interval.</span><span class="sxs-lookup"><span data-stu-id="8592a-422">Timespan.</span></span> <span data-ttu-id="8592a-423">Poskytuje některé malé volně mohou v případě, že deklarace identity vypršení platnosti tokenu hello je k dispozici v tokenu hello a následuje po hello aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="8592a-423">Provides some small leeway in case hello token's expiration claim is present in hello token and is past hello current date / time.</span></span>|<span data-ttu-id="8592a-424">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-424">No</span></span>|<span data-ttu-id="8592a-425">0 sekund</span><span class="sxs-lookup"><span data-stu-id="8592a-425">0 seconds</span></span>|  
|<span data-ttu-id="8592a-426">se nezdařilo ověření chybových zpráv</span><span class="sxs-lookup"><span data-stu-id="8592a-426">failed-validation-error-message</span></span>|<span data-ttu-id="8592a-427">Chybová zpráva tooreturn v textu odpovědi hello HTTP, pokud hello JWT nesplňuje podmínky ověření.</span><span class="sxs-lookup"><span data-stu-id="8592a-427">Error message tooreturn in hello HTTP response body if hello JWT does not pass validation.</span></span> <span data-ttu-id="8592a-428">Tuto zprávu musí mít žádné speciální znaky správně řídicí sekvencí.</span><span class="sxs-lookup"><span data-stu-id="8592a-428">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="8592a-429">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-429">No</span></span>|<span data-ttu-id="8592a-430">Výchozí chybovou zprávu, závisí na ověření problém, například "JWT nejsou k dispozici."</span><span class="sxs-lookup"><span data-stu-id="8592a-430">Default error message depends on validation issue, for example "JWT not present."</span></span>|  
|<span data-ttu-id="8592a-431">se nezdařilo httpcode ověření</span><span class="sxs-lookup"><span data-stu-id="8592a-431">failed-validation-httpcode</span></span>|<span data-ttu-id="8592a-432">Stav protokolu HTTP kód tooreturn Pokud hello JWT neprojde ověření.</span><span class="sxs-lookup"><span data-stu-id="8592a-432">HTTP Status code tooreturn if hello JWT doesn't pass validation.</span></span>|<span data-ttu-id="8592a-433">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-433">No</span></span>|<span data-ttu-id="8592a-434">401</span><span class="sxs-lookup"><span data-stu-id="8592a-434">401</span></span>|  
|<span data-ttu-id="8592a-435">název hlavičky</span><span class="sxs-lookup"><span data-stu-id="8592a-435">header-name</span></span>|<span data-ttu-id="8592a-436">Název Hello hello HTTP hlavičky, která uchovává hello token.</span><span class="sxs-lookup"><span data-stu-id="8592a-436">hello name of hello HTTP header holding hello token.</span></span>|<span data-ttu-id="8592a-437">Buď `header-name` nebo `query-paremeter-name` musí být zadaný; ale ne pomocí obou.</span><span class="sxs-lookup"><span data-stu-id="8592a-437">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="8592a-438">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-438">N/A</span></span>|  
|<span data-ttu-id="8592a-439">id</span><span class="sxs-lookup"><span data-stu-id="8592a-439">id</span></span>|<span data-ttu-id="8592a-440">Hello `id` atribut hello `key` element vám umožní toospecify hello řetězec, který bude odpovídat proti `kid` deklarací identity v tokenu (pokud existuje) toofind hello se hello odpovídající klíče toouse pro ověření podpisu.</span><span class="sxs-lookup"><span data-stu-id="8592a-440">hello `id` attribute on hello `key` element allows you toospecify hello string that will be matched against `kid` claim in hello token (if present) toofind out hello appropriate key toouse for signature validation.</span></span>|<span data-ttu-id="8592a-441">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-441">No</span></span>|<span data-ttu-id="8592a-442">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-442">N/A</span></span>|  
|<span data-ttu-id="8592a-443">Shoda</span><span class="sxs-lookup"><span data-stu-id="8592a-443">match</span></span>|<span data-ttu-id="8592a-444">Hello `match` atribut hello `claim` element určuje, zda každá hodnota deklarace identity v hello zásady musí být pro ověření toosucceed k dispozici v tokenu hello.</span><span class="sxs-lookup"><span data-stu-id="8592a-444">hello `match` attribute on hello `claim` element specifies whether every claim value in hello policy must be present in hello token for validation toosucceed.</span></span> <span data-ttu-id="8592a-445">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="8592a-445">Possible values are:</span></span><br /><br /> <span data-ttu-id="8592a-446">-                          `all`-Každá hodnota deklarace identity v hello zásady musí být součástí hello token pro toosucceed ověření.</span><span class="sxs-lookup"><span data-stu-id="8592a-446">-                          `all` - every claim value in hello policy must be present in hello token for validation toosucceed.</span></span><br /><br /> <span data-ttu-id="8592a-447">-                          `any`-Hodnota nejméně jedna deklarace identity musí být dostupná v tokenu hello toosucceed ověření.</span><span class="sxs-lookup"><span data-stu-id="8592a-447">-                          `any` - at least one claim value must be present in hello token for validation toosucceed.</span></span>|<span data-ttu-id="8592a-448">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-448">No</span></span>|<span data-ttu-id="8592a-449">Všechny</span><span class="sxs-lookup"><span data-stu-id="8592a-449">all</span></span>|  
|<span data-ttu-id="8592a-450">Název dotazu paremeter</span><span class="sxs-lookup"><span data-stu-id="8592a-450">query-paremeter-name</span></span>|<span data-ttu-id="8592a-451">Hello název parametru dotazu hello hello podržíte hello token.</span><span class="sxs-lookup"><span data-stu-id="8592a-451">hello name of hello hello query parameter holding hello token.</span></span>|<span data-ttu-id="8592a-452">Buď `header-name` nebo `query-paremeter-name` musí být zadaný; ale ne pomocí obou.</span><span class="sxs-lookup"><span data-stu-id="8592a-452">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="8592a-453">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-453">N/A</span></span>|  
|<span data-ttu-id="8592a-454">požadovat čas vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="8592a-454">require-expiration-time</span></span>|<span data-ttu-id="8592a-455">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="8592a-455">Boolean.</span></span> <span data-ttu-id="8592a-456">Určuje, jestli je v tokenu hello vyžaduje deklaraci identity vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="8592a-456">Specifies whether an expiration claim is required in hello token.</span></span>|<span data-ttu-id="8592a-457">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-457">No</span></span>|<span data-ttu-id="8592a-458">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="8592a-458">true</span></span>|
|<span data-ttu-id="8592a-459">vyžadovat schéma</span><span class="sxs-lookup"><span data-stu-id="8592a-459">require-scheme</span></span>|<span data-ttu-id="8592a-460">název schématu hello tokenu, například Hello "Nosiče".</span><span class="sxs-lookup"><span data-stu-id="8592a-460">hello name of hello token scheme, e.g. "Bearer".</span></span> <span data-ttu-id="8592a-461">Když tento atribut nastavený, zásady hello zajistí, který zadán, je k dispozici v hello hodnota hlavičky ověřování schématu.</span><span class="sxs-lookup"><span data-stu-id="8592a-461">When this attribute is set, hello policy will ensure that specified scheme is present in hello Authorization header value.</span></span>|<span data-ttu-id="8592a-462">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-462">No</span></span>|<span data-ttu-id="8592a-463">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-463">N/A</span></span>|
|<span data-ttu-id="8592a-464">Vyžadovat podepsané tokeny</span><span class="sxs-lookup"><span data-stu-id="8592a-464">require-signed-tokens</span></span>|<span data-ttu-id="8592a-465">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="8592a-465">Boolean.</span></span> <span data-ttu-id="8592a-466">Určuje, zda token je požadovaná toobe podepsané.</span><span class="sxs-lookup"><span data-stu-id="8592a-466">Specifies whether a token is required toobe signed.</span></span>|<span data-ttu-id="8592a-467">Ne</span><span class="sxs-lookup"><span data-stu-id="8592a-467">No</span></span>|<span data-ttu-id="8592a-468">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="8592a-468">true</span></span>|  
|<span data-ttu-id="8592a-469">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="8592a-469">url</span></span>|<span data-ttu-id="8592a-470">Otevřete adresu URL koncového bodu ID konfigurace ze které lze získat metadata Open ID konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8592a-470">Open ID configuration endpoint URL from where Open ID configuration metadata can be obtained.</span></span> <span data-ttu-id="8592a-471">Pro Azure Active Directory použít následující adresu URL hello: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` nahraďte název vašeho adresáře klienta, například `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="8592a-471">For Azure Active Directory use hello following URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` substituting your directory tenant name, e.g. `contoso.onmicrosoft.com`.</span></span>|<span data-ttu-id="8592a-472">Ano</span><span class="sxs-lookup"><span data-stu-id="8592a-472">Yes</span></span>|<span data-ttu-id="8592a-473">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="8592a-473">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8592a-474">Využití</span><span class="sxs-lookup"><span data-stu-id="8592a-474">Usage</span></span>  
 <span data-ttu-id="8592a-475">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8592a-475">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8592a-476">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="8592a-476">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8592a-477">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="8592a-477">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="8592a-478">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8592a-478">Next steps</span></span>
<span data-ttu-id="8592a-479">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="8592a-479">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
