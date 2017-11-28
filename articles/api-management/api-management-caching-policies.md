---
title: "Zásady ukládání do mezipaměti Azure API Management | Microsoft Docs"
description: "Další informace o ukládání do mezipaměti zásady, které jsou k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 2a8f012e7e223ef5c1683c8a6c5ecf2f3e96bed8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-caching-policies"></a><span data-ttu-id="9ed60-103">Ukládání do mezipaměti zásady služby API Management</span><span class="sxs-lookup"><span data-stu-id="9ed60-103">API Management caching policies</span></span>
<span data-ttu-id="9ed60-104">Toto téma obsahuje odkaz pro následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="9ed60-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="9ed60-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="9ed60-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="9ed60-106"><a name="CachingPolicies"></a>Zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-106"><a name="CachingPolicies"></a> Caching policies</span></span>  
  
-   <span data-ttu-id="9ed60-107">Odpověď zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-107">Response caching policies</span></span>  
  
    -   <span data-ttu-id="9ed60-108">[Získat z mezipaměti](api-management-caching-policies.md#GetFromCache) -provést mezipaměti vyhledat a vrátit platnou do mezipaměti odpovědi, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9ed60-108">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached responses when available.</span></span>  
  
    -   <span data-ttu-id="9ed60-109">[Úložiště pro mezipaměť](api-management-caching-policies.md#StoreToCache) -ukládá do mezipaměti odpovědi podle konfigurace zadané mezipaměti ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="9ed60-109">[Store to cache](api-management-caching-policies.md#StoreToCache) - Caches responses according to the specified cache control configuration.</span></span>  
  
-   <span data-ttu-id="9ed60-110">Hodnota zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-110">Value caching policies</span></span>  
  
    -   <span data-ttu-id="9ed60-111">[Získat hodnotu z mezipaměti](#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="9ed60-111">[Get value from cache](#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="9ed60-112">[Uložení v mezipaměti hodnoty](#StoreToCacheByKey) -uložit položky do mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="9ed60-112">[Store value in cache](#StoreToCacheByKey) - Store an item in the cache by key.</span></span>  
  
    -   <span data-ttu-id="9ed60-113">[Odebrat hodnotu z mezipaměti](#RemoveCacheByKey) -odebrání položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="9ed60-113">[Remove value from cache](#RemoveCacheByKey) - Remove an item in the cache by key.</span></span>  
  
##  <span data-ttu-id="9ed60-114"><a name="GetFromCache"></a>Získat z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-114"><a name="GetFromCache"></a> Get from cache</span></span>  
 <span data-ttu-id="9ed60-115">Použití `cache-lookup` zásad provést mezipaměti vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9ed60-115">Use the `cache-lookup` policy to perform cache look up and return a valid cached response when available.</span></span> <span data-ttu-id="9ed60-116">Tuto zásadu lze použít v případech, kde odpovědi obsah zůstane statické přes v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="9ed60-116">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="9ed60-117">Ukládání odpovědí do mezipaměti omezuje šířku pásma a požadavky na zpracování uložených na back-end webový server a snižuje latence zaznamenatelného spotřebiteli rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9ed60-117">Response caching reduces bandwidth and processing requirements imposed on the backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="9ed60-118">Tato zásada musí mít odpovídající [úložiště do mezipaměti](api-management-caching-policies.md#StoreToCache) zásad.</span><span class="sxs-lookup"><span data-stu-id="9ed60-118">This policy must have a corresponding [Store to cache](api-management-caching-policies.md#StoreToCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="9ed60-119">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="9ed60-119">Policy statement</span></span>  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">  
  <vary-by-header>Accept</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Accept-Charset</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Authorization</vary-by-header>  
  <!-- should be present when allow-authorized-response-caching is "true"-->  
  <vary-by-header>header name</vary-by-header>  
  <!-- optional, can repeated several times -->  
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>  
  <!-- optional, can repeated several times -->  
</cache-lookup>  
```  
  
### <a name="examples"></a><span data-ttu-id="9ed60-120">Příklady</span><span class="sxs-lookup"><span data-stu-id="9ed60-120">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="9ed60-121">Příklad</span><span class="sxs-lookup"><span data-stu-id="9ed60-121">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true">  
            <vary-by-query-parameter>version</vary-by-query-parameter>  
        </cache-lookup>           
    </inbound>  
    <outbound>  
        <cache-store duration="seconds" />  
        <base />          
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="9ed60-122">Příklad pomocí výrazů zásad</span><span class="sxs-lookup"><span data-stu-id="9ed60-122">Example using policy expressions</span></span>  
 <span data-ttu-id="9ed60-123">Tento příklad ukazuje, jak k ke konfiguraci ukládání do mezipaměti dobu trvání, která odpovídá odpovědi ukládání do mezipaměti back-end službu jako odpověď API Management můžou být specifikované pomocí služby zálohování `Cache-Control` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="9ed60-123">This example shows how to to configure API Management response caching duration that matches the response caching of the backend service as specified by the backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="9ed60-124">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 25:25.</span><span class="sxs-lookup"><span data-stu-id="9ed60-124">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 25:25.</span></span>  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="9ed60-125">Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="9ed60-125">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="9ed60-126">Elementy</span><span class="sxs-lookup"><span data-stu-id="9ed60-126">Elements</span></span>  
  
|<span data-ttu-id="9ed60-127">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-127">Name</span></span>|<span data-ttu-id="9ed60-128">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-128">Description</span></span>|<span data-ttu-id="9ed60-129">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-129">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="9ed60-130">vyhledávání v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-130">cache-lookup</span></span>|<span data-ttu-id="9ed60-131">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="9ed60-131">Root element.</span></span>|<span data-ttu-id="9ed60-132">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-132">Yes</span></span>|  
|<span data-ttu-id="9ed60-133">měnit podle hlavičky</span><span class="sxs-lookup"><span data-stu-id="9ed60-133">vary-by-header</span></span>|<span data-ttu-id="9ed60-134">Spustit ukládání do mezipaměti odpovědi na hodnotu zadaného záhlaví, jako je například přijmout, Accept-Charset, Accept-Encoding, Accept-Language, autorizace, Expect, z hostitele, If-Match.</span><span class="sxs-lookup"><span data-stu-id="9ed60-134">Start caching responses per value of specified header, such as Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match.</span></span>|<span data-ttu-id="9ed60-135">Ne</span><span class="sxs-lookup"><span data-stu-id="9ed60-135">No</span></span>|  
|<span data-ttu-id="9ed60-136">lišit podle--parametr dotazu</span><span class="sxs-lookup"><span data-stu-id="9ed60-136">vary-by-query-parameter</span></span>|<span data-ttu-id="9ed60-137">Spusťte ukládání do mezipaměti odpovědi na hodnotu zadanými parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="9ed60-137">Start caching responses per value of specified query parameters.</span></span> <span data-ttu-id="9ed60-138">Zadejte jeden nebo více parametrů.</span><span class="sxs-lookup"><span data-stu-id="9ed60-138">Enter a single or multiple parameters.</span></span> <span data-ttu-id="9ed60-139">Použijte středník jako oddělovač.</span><span class="sxs-lookup"><span data-stu-id="9ed60-139">Use semicolon as a separator.</span></span> <span data-ttu-id="9ed60-140">Pokud se zadaný žádný, se používají všechny parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="9ed60-140">If none are specified, all query parameters are used.</span></span>|<span data-ttu-id="9ed60-141">Ne</span><span class="sxs-lookup"><span data-stu-id="9ed60-141">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="9ed60-142">Atributy</span><span class="sxs-lookup"><span data-stu-id="9ed60-142">Attributes</span></span>  
  
|<span data-ttu-id="9ed60-143">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-143">Name</span></span>|<span data-ttu-id="9ed60-144">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-144">Description</span></span>|<span data-ttu-id="9ed60-145">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-145">Required</span></span>|<span data-ttu-id="9ed60-146">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9ed60-146">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="9ed60-147">Povolit privátní odpovědi-ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-147">allow-private-response-caching</span></span>|<span data-ttu-id="9ed60-148">Pokud nastavíte hodnotu `true`, umožňuje ukládání do mezipaměti požadavků, které obsahují autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="9ed60-148">When set to `true`, allows caching of requests that contain an Authorization header.</span></span>|<span data-ttu-id="9ed60-149">Ne</span><span class="sxs-lookup"><span data-stu-id="9ed60-149">No</span></span>|<span data-ttu-id="9ed60-150">False</span><span class="sxs-lookup"><span data-stu-id="9ed60-150">false</span></span>|  
|<span data-ttu-id="9ed60-151">podřízený typ ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-151">downstream-caching-type</span></span>|<span data-ttu-id="9ed60-152">Tento atribut musí být nastaven na jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="9ed60-152">This attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="9ed60-153">-none - podřízené ukládání do mezipaměti není povolen.</span><span class="sxs-lookup"><span data-stu-id="9ed60-153">-   none - downstream caching is not allowed.</span></span><br /><span data-ttu-id="9ed60-154">-soukromý – podřízené privátní ukládání do mezipaměti je povolen.</span><span class="sxs-lookup"><span data-stu-id="9ed60-154">-   private - downstream private caching is allowed.</span></span><br /><span data-ttu-id="9ed60-155">-veřejný - privátní a sdílené podřízené ukládání do mezipaměti je povolen.</span><span class="sxs-lookup"><span data-stu-id="9ed60-155">-   public - private and shared downstream caching is allowed.</span></span>|<span data-ttu-id="9ed60-156">Ne</span><span class="sxs-lookup"><span data-stu-id="9ed60-156">No</span></span>|<span data-ttu-id="9ed60-157">None</span><span class="sxs-lookup"><span data-stu-id="9ed60-157">none</span></span>|  
|<span data-ttu-id="9ed60-158">musí revalidate</span><span class="sxs-lookup"><span data-stu-id="9ed60-158">must-revalidate</span></span>|<span data-ttu-id="9ed60-159">Pokud je povoleno ukládání do mezipaměti podřízené tento atribut Zapne nebo vypne `must-revalidate` direktiva ovládací prvek mezipaměti v odpovědi brány.</span><span class="sxs-lookup"><span data-stu-id="9ed60-159">When downstream caching is enabled this attribute turns on or off  the `must-revalidate` cache control directive in gateway responses.</span></span>|<span data-ttu-id="9ed60-160">Ne</span><span class="sxs-lookup"><span data-stu-id="9ed60-160">No</span></span>|<span data-ttu-id="9ed60-161">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="9ed60-161">true</span></span>|  
|<span data-ttu-id="9ed60-162">se liší podle vývojáře</span><span class="sxs-lookup"><span data-stu-id="9ed60-162">vary-by-developer</span></span>|<span data-ttu-id="9ed60-163">Nastavte na `true` do mezipaměti odpovědi na vývojáře klíč.</span><span class="sxs-lookup"><span data-stu-id="9ed60-163">Set to `true` to cache responses per developer key.</span></span>|<span data-ttu-id="9ed60-164">Ne</span><span class="sxs-lookup"><span data-stu-id="9ed60-164">No</span></span>|<span data-ttu-id="9ed60-165">False</span><span class="sxs-lookup"><span data-stu-id="9ed60-165">false</span></span>|  
|<span data-ttu-id="9ed60-166">se liší podle vývojáře skupiny –</span><span class="sxs-lookup"><span data-stu-id="9ed60-166">vary-by-developer-groups</span></span>|<span data-ttu-id="9ed60-167">Nastavte na `true` do mezipaměti odpovědi na roli uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ed60-167">Set to `true` to cache responses per user role.</span></span>|<span data-ttu-id="9ed60-168">Ne</span><span class="sxs-lookup"><span data-stu-id="9ed60-168">No</span></span>|<span data-ttu-id="9ed60-169">False</span><span class="sxs-lookup"><span data-stu-id="9ed60-169">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="9ed60-170">Využití</span><span class="sxs-lookup"><span data-stu-id="9ed60-170">Usage</span></span>  
 <span data-ttu-id="9ed60-171">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="9ed60-171">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="9ed60-172">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="9ed60-172">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="9ed60-173">**Zásady obory:** rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="9ed60-173">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="9ed60-174"><a name="StoreToCache"></a>Ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-174"><a name="StoreToCache"></a> Store to cache</span></span>  
 <span data-ttu-id="9ed60-175">`cache-store` Zásad ukládá do mezipaměti odpovědi podle nastavení zadaný mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9ed60-175">The `cache-store` policy caches responses according to the specified cache settings.</span></span> <span data-ttu-id="9ed60-176">Tuto zásadu lze použít v případech, kde odpovědi obsah zůstane statické přes v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="9ed60-176">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="9ed60-177">Ukládání odpovědí do mezipaměti omezuje šířku pásma a požadavky na zpracování uložených na back-end webový server a snižuje latence zaznamenatelného spotřebiteli rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9ed60-177">Response caching reduces bandwidth and processing requirements imposed on the backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="9ed60-178">Tato zásada musí mít odpovídající [získat z mezipaměti](api-management-caching-policies.md#GetFromCache) zásad.</span><span class="sxs-lookup"><span data-stu-id="9ed60-178">This policy must have a corresponding [Get from cache](api-management-caching-policies.md#GetFromCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="9ed60-179">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="9ed60-179">Policy statement</span></span>  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a><span data-ttu-id="9ed60-180">Příklady</span><span class="sxs-lookup"><span data-stu-id="9ed60-180">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="9ed60-181">Příklad</span><span class="sxs-lookup"><span data-stu-id="9ed60-181">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
          <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">  
                <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->  
        </cache-lookup>  
    </inbound>  
    <outbound>  
        <base />  
            <cache-store duration="3600" />  
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="9ed60-182">Příklad pomocí výrazů zásad</span><span class="sxs-lookup"><span data-stu-id="9ed60-182">Example using policy expressions</span></span>  
 <span data-ttu-id="9ed60-183">Tento příklad ukazuje, jak k ke konfiguraci ukládání do mezipaměti dobu trvání, která odpovídá odpovědi ukládání do mezipaměti back-end službu jako odpověď API Management můžou být specifikované pomocí služby zálohování `Cache-Control` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="9ed60-183">This example shows how to to configure API Management response caching duration that matches the response caching of the backend service as specified by the backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="9ed60-184">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 25:25.</span><span class="sxs-lookup"><span data-stu-id="9ed60-184">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 25:25.</span></span>  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="9ed60-185">Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="9ed60-185">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="9ed60-186">Elementy</span><span class="sxs-lookup"><span data-stu-id="9ed60-186">Elements</span></span>  
  
|<span data-ttu-id="9ed60-187">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-187">Name</span></span>|<span data-ttu-id="9ed60-188">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-188">Description</span></span>|<span data-ttu-id="9ed60-189">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-189">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="9ed60-190">úložiště mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-190">cache-store</span></span>|<span data-ttu-id="9ed60-191">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="9ed60-191">Root element.</span></span>|<span data-ttu-id="9ed60-192">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-192">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="9ed60-193">Atributy</span><span class="sxs-lookup"><span data-stu-id="9ed60-193">Attributes</span></span>  
  
|<span data-ttu-id="9ed60-194">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-194">Name</span></span>|<span data-ttu-id="9ed60-195">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-195">Description</span></span>|<span data-ttu-id="9ed60-196">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-196">Required</span></span>|<span data-ttu-id="9ed60-197">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9ed60-197">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="9ed60-198">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="9ed60-198">duration</span></span>|<span data-ttu-id="9ed60-199">Time-to-live položek v mezipaměti, zadané v sekundách.</span><span class="sxs-lookup"><span data-stu-id="9ed60-199">Time-to-live of the cached entries, specified in seconds.</span></span>|<span data-ttu-id="9ed60-200">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-200">Yes</span></span>|<span data-ttu-id="9ed60-201">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9ed60-201">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="9ed60-202">Využití</span><span class="sxs-lookup"><span data-stu-id="9ed60-202">Usage</span></span>  
 <span data-ttu-id="9ed60-203">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="9ed60-203">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="9ed60-204">**Části zásady:** odchozí</span><span class="sxs-lookup"><span data-stu-id="9ed60-204">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="9ed60-205">**Zásady obory:** rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="9ed60-205">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="9ed60-206"><a name="GetFromCacheByKey"></a>Získat hodnotu z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-206"><a name="GetFromCacheByKey"></a> Get value from cache</span></span>  
 <span data-ttu-id="9ed60-207">Použití `cache-lookup-value` zásady a provádět vyhledávání v mezipaměti podle klíče a vrátí hodnotu uloženou v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9ed60-207">Use the `cache-lookup-value` policy to perform cache lookup by key and return a cached value.</span></span> <span data-ttu-id="9ed60-208">Klíč může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="9ed60-208">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="9ed60-209">Tato zásada musí mít odpovídající [uložení hodnoty v mezipaměti](#StoreToCacheByKey) zásad.</span><span class="sxs-lookup"><span data-stu-id="9ed60-209">This policy must have a corresponding [Store value in cache](#StoreToCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="9ed60-210">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="9ed60-210">Policy statement</span></span>  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value to use if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a><span data-ttu-id="9ed60-211">Příklad</span><span class="sxs-lookup"><span data-stu-id="9ed60-211">Example</span></span>  
 <span data-ttu-id="9ed60-212">Další informace a příklady těchto zásad najdete v tématu [vlastní ukládání do mezipaměti ve službě Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="9ed60-212">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="9ed60-213">Elementy</span><span class="sxs-lookup"><span data-stu-id="9ed60-213">Elements</span></span>  
  
|<span data-ttu-id="9ed60-214">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-214">Name</span></span>|<span data-ttu-id="9ed60-215">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-215">Description</span></span>|<span data-ttu-id="9ed60-216">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-216">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="9ed60-217">hodnota mezipaměti vyhledávání</span><span class="sxs-lookup"><span data-stu-id="9ed60-217">cache-lookup-value</span></span>|<span data-ttu-id="9ed60-218">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="9ed60-218">Root element.</span></span>|<span data-ttu-id="9ed60-219">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="9ed60-220">Atributy</span><span class="sxs-lookup"><span data-stu-id="9ed60-220">Attributes</span></span>  
  
|<span data-ttu-id="9ed60-221">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-221">Name</span></span>|<span data-ttu-id="9ed60-222">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-222">Description</span></span>|<span data-ttu-id="9ed60-223">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-223">Required</span></span>|<span data-ttu-id="9ed60-224">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9ed60-224">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="9ed60-225">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9ed60-225">default-value</span></span>|<span data-ttu-id="9ed60-226">Hodnota, která se přiřadí proměnné Pokud vyhledávání klíčů mezipaměti vedlo k neúspěšnému přístupu.</span><span class="sxs-lookup"><span data-stu-id="9ed60-226">A value that will be assigned to the variable if the cache key lookup resulted in a miss.</span></span> <span data-ttu-id="9ed60-227">Pokud se tento atribut nezadá, `null` je přiřazen.</span><span class="sxs-lookup"><span data-stu-id="9ed60-227">If this attribute is not specified, `null` is assigned.</span></span>|<span data-ttu-id="9ed60-228">Ne</span><span class="sxs-lookup"><span data-stu-id="9ed60-228">No</span></span>|`null`|  
|<span data-ttu-id="9ed60-229">key</span><span class="sxs-lookup"><span data-stu-id="9ed60-229">key</span></span>|<span data-ttu-id="9ed60-230">Hodnota klíče mezipaměti pro použití v vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="9ed60-230">Cache key value to use in the lookup.</span></span>|<span data-ttu-id="9ed60-231">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-231">Yes</span></span>|<span data-ttu-id="9ed60-232">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9ed60-232">N/A</span></span>|  
|<span data-ttu-id="9ed60-233">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="9ed60-233">variable-name</span></span>|<span data-ttu-id="9ed60-234">Název [kontextové proměnné](api-management-policy-expressions.md#ContextVariables) looked až hodnota budou přiřazeny, pokud je úspěšné vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="9ed60-234">Name of the [context variable](api-management-policy-expressions.md#ContextVariables) the looked up value will be assigned to, if lookup is successful.</span></span> <span data-ttu-id="9ed60-235">Pokud vyhledávání výsledkem k neúspěšnému přístupu do, proměnné se přiřadí hodnota `default-value` atribut nebo `null`, pokud `default-value` atribut vynechán.</span><span class="sxs-lookup"><span data-stu-id="9ed60-235">If lookup results in a miss, the variable will be assigned the value of the `default-value` attribute or `null`, if the `default-value` attribute is omitted.</span></span>|<span data-ttu-id="9ed60-236">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-236">Yes</span></span>|<span data-ttu-id="9ed60-237">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9ed60-237">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="9ed60-238">Využití</span><span class="sxs-lookup"><span data-stu-id="9ed60-238">Usage</span></span>  
 <span data-ttu-id="9ed60-239">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="9ed60-239">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="9ed60-240">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="9ed60-240">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="9ed60-241">**Zásady obory:** globální, rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="9ed60-241">**Policy scopes:** global, API, operation, product</span></span>  
  
##  <span data-ttu-id="9ed60-242"><a name="StoreToCacheByKey"></a>Hodnota úložiště v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-242"><a name="StoreToCacheByKey"></a> Store value in cache</span></span>  
 <span data-ttu-id="9ed60-243">`cache-store-value` Provede úložiště mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="9ed60-243">The `cache-store-value` performs cache storage by key.</span></span> <span data-ttu-id="9ed60-244">Klíč může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="9ed60-244">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="9ed60-245">Tato zásada musí mít odpovídající [získat hodnotu z mezipaměti](#GetFromCacheByKey) zásad.</span><span class="sxs-lookup"><span data-stu-id="9ed60-245">This policy must have a corresponding [Get value from cache](#GetFromCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="9ed60-246">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="9ed60-246">Policy statement</span></span>  
  
```xml  
<cache-store-value key="cache key value" value="value to cache" duration="seconds" />  
```  
  
### <a name="example"></a><span data-ttu-id="9ed60-247">Příklad</span><span class="sxs-lookup"><span data-stu-id="9ed60-247">Example</span></span>  
 <span data-ttu-id="9ed60-248">Další informace a příklady těchto zásad najdete v tématu [vlastní ukládání do mezipaměti ve službě Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="9ed60-248">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="9ed60-249">Elementy</span><span class="sxs-lookup"><span data-stu-id="9ed60-249">Elements</span></span>  
  
|<span data-ttu-id="9ed60-250">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-250">Name</span></span>|<span data-ttu-id="9ed60-251">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-251">Description</span></span>|<span data-ttu-id="9ed60-252">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-252">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="9ed60-253">Hodnota úložiště mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-253">cache-store-value</span></span>|<span data-ttu-id="9ed60-254">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="9ed60-254">Root element.</span></span>|<span data-ttu-id="9ed60-255">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-255">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="9ed60-256">Atributy</span><span class="sxs-lookup"><span data-stu-id="9ed60-256">Attributes</span></span>  
  
|<span data-ttu-id="9ed60-257">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-257">Name</span></span>|<span data-ttu-id="9ed60-258">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-258">Description</span></span>|<span data-ttu-id="9ed60-259">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-259">Required</span></span>|<span data-ttu-id="9ed60-260">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9ed60-260">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="9ed60-261">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="9ed60-261">duration</span></span>|<span data-ttu-id="9ed60-262">Hodnota bude do mezipaměti pro hodnotu zadaná doba trvání, zadaný v sekundách.</span><span class="sxs-lookup"><span data-stu-id="9ed60-262">Value will be cached for the provided duration value, specified in seconds.</span></span>|<span data-ttu-id="9ed60-263">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-263">Yes</span></span>|<span data-ttu-id="9ed60-264">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9ed60-264">N/A</span></span>|  
|<span data-ttu-id="9ed60-265">key</span><span class="sxs-lookup"><span data-stu-id="9ed60-265">key</span></span>|<span data-ttu-id="9ed60-266">V části se má uložit klíč mezipaměti hodnota.</span><span class="sxs-lookup"><span data-stu-id="9ed60-266">Cache key the value will be stored under.</span></span>|<span data-ttu-id="9ed60-267">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-267">Yes</span></span>|<span data-ttu-id="9ed60-268">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9ed60-268">N/A</span></span>|  
|<span data-ttu-id="9ed60-269">hodnota</span><span class="sxs-lookup"><span data-stu-id="9ed60-269">value</span></span>|<span data-ttu-id="9ed60-270">Hodnota ukládat do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9ed60-270">The value to be cached.</span></span>|<span data-ttu-id="9ed60-271">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-271">Yes</span></span>|<span data-ttu-id="9ed60-272">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9ed60-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="9ed60-273">Využití</span><span class="sxs-lookup"><span data-stu-id="9ed60-273">Usage</span></span>  
 <span data-ttu-id="9ed60-274">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="9ed60-274">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="9ed60-275">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="9ed60-275">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="9ed60-276">**Zásady obory:** globální, rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="9ed60-276">**Policy scopes:** global, API, operation, product</span></span>  
  
###  <span data-ttu-id="9ed60-277"><a name="RemoveCacheByKey"></a>Odebrat hodnotu z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="9ed60-277"><a name="RemoveCacheByKey"></a> Remove value from cache</span></span>  
 <span data-ttu-id="9ed60-278">`cache-remove-value` Odstraní identifikovaný svůj klíč položky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9ed60-278">The             `cache-remove-value` deletes a cached item identified by its key.</span></span> <span data-ttu-id="9ed60-279">Klíč může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="9ed60-279">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
#### <a name="policy-statement"></a><span data-ttu-id="9ed60-280">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="9ed60-280">Policy statement</span></span>  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="9ed60-281">Příklad</span><span class="sxs-lookup"><span data-stu-id="9ed60-281">Example</span></span>  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a><span data-ttu-id="9ed60-282">Elementy</span><span class="sxs-lookup"><span data-stu-id="9ed60-282">Elements</span></span>  
  
|<span data-ttu-id="9ed60-283">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-283">Name</span></span>|<span data-ttu-id="9ed60-284">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-284">Description</span></span>|<span data-ttu-id="9ed60-285">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-285">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="9ed60-286">hodnota mezipaměti odebrat</span><span class="sxs-lookup"><span data-stu-id="9ed60-286">cache-remove-value</span></span>|<span data-ttu-id="9ed60-287">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="9ed60-287">Root element.</span></span>|<span data-ttu-id="9ed60-288">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-288">Yes</span></span>|  
  
#### <a name="attributes"></a><span data-ttu-id="9ed60-289">Atributy</span><span class="sxs-lookup"><span data-stu-id="9ed60-289">Attributes</span></span>  
  
|<span data-ttu-id="9ed60-290">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9ed60-290">Name</span></span>|<span data-ttu-id="9ed60-291">Popis</span><span class="sxs-lookup"><span data-stu-id="9ed60-291">Description</span></span>|<span data-ttu-id="9ed60-292">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9ed60-292">Required</span></span>|<span data-ttu-id="9ed60-293">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9ed60-293">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="9ed60-294">key</span><span class="sxs-lookup"><span data-stu-id="9ed60-294">key</span></span>|<span data-ttu-id="9ed60-295">Klíče uložené v mezipaměti hodnota, která má být odebrány z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="9ed60-295">The key of the previously cached value to be removed from the cache.</span></span>|<span data-ttu-id="9ed60-296">Ano</span><span class="sxs-lookup"><span data-stu-id="9ed60-296">Yes</span></span>|<span data-ttu-id="9ed60-297">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9ed60-297">N/A</span></span>|  
  
#### <a name="usage"></a><span data-ttu-id="9ed60-298">Využití</span><span class="sxs-lookup"><span data-stu-id="9ed60-298">Usage</span></span>  
 <span data-ttu-id="9ed60-299">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="9ed60-299">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="9ed60-300">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="9ed60-300">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="9ed60-301">**Zásady obory:** globální, rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="9ed60-301">**Policy scopes:** global, API, operation, product</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="9ed60-302">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ed60-302">Next steps</span></span>
<span data-ttu-id="9ed60-303">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="9ed60-303">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  