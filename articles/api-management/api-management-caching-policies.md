---
title: "ukládání do mezipaměti zásady služby API Management aaaAzure | Microsoft Docs"
description: "Další informace o hello zásady, které jsou k dispozici pro použití v Azure API Management ukládání do mezipaměti."
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
ms.openlocfilehash: bd6b0721945609b28dbf6e7ef0631979c08c8c65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-caching-policies"></a><span data-ttu-id="f611f-103">Ukládání do mezipaměti zásady služby API Management</span><span class="sxs-lookup"><span data-stu-id="f611f-103">API Management caching policies</span></span>
<span data-ttu-id="f611f-104">Toto téma obsahuje odkaz pro hello následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="f611f-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="f611f-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="f611f-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="f611f-106"><a name="CachingPolicies"></a>Zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-106"><a name="CachingPolicies"></a> Caching policies</span></span>  
  
-   <span data-ttu-id="f611f-107">Odpověď zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-107">Response caching policies</span></span>  
  
    -   <span data-ttu-id="f611f-108">[Získat z mezipaměti](api-management-caching-policies.md#GetFromCache) -provést mezipaměti vyhledat a vrátit platnou do mezipaměti odpovědi, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f611f-108">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached responses when available.</span></span>  
  
    -   <span data-ttu-id="f611f-109">[Uložení toocache](api-management-caching-policies.md#StoreToCache) – mezipamětí odpovědí podle konfigurace toohello zadaný mezipaměti ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="f611f-109">[Store toocache](api-management-caching-policies.md#StoreToCache) - Caches responses according toohello specified cache control configuration.</span></span>  
  
-   <span data-ttu-id="f611f-110">Hodnota zásady ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-110">Value caching policies</span></span>  
  
    -   <span data-ttu-id="f611f-111">[Získat hodnotu z mezipaměti](#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="f611f-111">[Get value from cache](#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="f611f-112">[Uložení v mezipaměti hodnoty](#StoreToCacheByKey) -uložit položku hello mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="f611f-112">[Store value in cache](#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>  
  
    -   <span data-ttu-id="f611f-113">[Odebrat hodnotu z mezipaměti](#RemoveCacheByKey) -odebrání položky v mezipaměti hello podle klíče.</span><span class="sxs-lookup"><span data-stu-id="f611f-113">[Remove value from cache](#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>  
  
##  <span data-ttu-id="f611f-114"><a name="GetFromCache"></a>Získat z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-114"><a name="GetFromCache"></a> Get from cache</span></span>  
 <span data-ttu-id="f611f-115">Použití hello `cache-lookup` tooperform mezipaměť zásad vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f611f-115">Use hello `cache-lookup` policy tooperform cache look up and return a valid cached response when available.</span></span> <span data-ttu-id="f611f-116">Tuto zásadu lze použít v případech, kde odpovědi obsah zůstane statické přes v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="f611f-116">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="f611f-117">Ukládání odpovědí do mezipaměti omezuje šířku pásma a požadavky na zpracování uložených na hello back-end webový server a snižuje latence, zaznamenatelného spotřebiteli rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f611f-117">Response caching reduces bandwidth and processing requirements imposed on hello backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="f611f-118">Tato zásada musí mít odpovídající [toocache úložiště](api-management-caching-policies.md#StoreToCache) zásad.</span><span class="sxs-lookup"><span data-stu-id="f611f-118">This policy must have a corresponding [Store toocache](api-management-caching-policies.md#StoreToCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="f611f-119">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="f611f-119">Policy statement</span></span>  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression tooevaluate)">  
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
  
### <a name="examples"></a><span data-ttu-id="f611f-120">Příklady</span><span class="sxs-lookup"><span data-stu-id="f611f-120">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="f611f-121">Příklad</span><span class="sxs-lookup"><span data-stu-id="f611f-121">Example</span></span>  
  
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
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="f611f-122">Příklad pomocí výrazů zásad</span><span class="sxs-lookup"><span data-stu-id="f611f-122">Example using policy expressions</span></span>  
 <span data-ttu-id="f611f-123">Tento příklad ukazuje, jak odpovědi API Management tootooconfigure ukládání do mezipaměti Doba trvání, zda text hello odpovídá ukládání odpovědí do mezipaměti hello back-end služby podle specifikace hello zálohovaný služby `Cache-Control` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="f611f-123">This example shows how tootooconfigure API Management response caching duration that matches hello response caching of hello backend service as specified by hello backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="f611f-124">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too25:25.</span><span class="sxs-lookup"><span data-stu-id="f611f-124">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too25:25.</span></span>  
  
```xml  
<!-- hello following cache policy snippets demonstrate how toocontrol API Management reponse cache duration with Cache-Control headers sent by hello backend service. -->  
  
<!-- Copy this snippet into hello inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into hello outbound section. Note that cache duration is set toohello max-age value provided in hello Cache-Control header received from hello backend service or toohello deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="f611f-125">Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="f611f-125">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="f611f-126">Elementy</span><span class="sxs-lookup"><span data-stu-id="f611f-126">Elements</span></span>  
  
|<span data-ttu-id="f611f-127">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-127">Name</span></span>|<span data-ttu-id="f611f-128">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-128">Description</span></span>|<span data-ttu-id="f611f-129">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-129">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="f611f-130">vyhledávání v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-130">cache-lookup</span></span>|<span data-ttu-id="f611f-131">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="f611f-131">Root element.</span></span>|<span data-ttu-id="f611f-132">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-132">Yes</span></span>|  
|<span data-ttu-id="f611f-133">měnit podle hlavičky</span><span class="sxs-lookup"><span data-stu-id="f611f-133">vary-by-header</span></span>|<span data-ttu-id="f611f-134">Spustit ukládání do mezipaměti odpovědi na hodnotu zadaného záhlaví, jako je například přijmout, Accept-Charset, Accept-Encoding, Accept-Language, autorizace, Expect, z hostitele, If-Match.</span><span class="sxs-lookup"><span data-stu-id="f611f-134">Start caching responses per value of specified header, such as Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match.</span></span>|<span data-ttu-id="f611f-135">Ne</span><span class="sxs-lookup"><span data-stu-id="f611f-135">No</span></span>|  
|<span data-ttu-id="f611f-136">lišit podle--parametr dotazu</span><span class="sxs-lookup"><span data-stu-id="f611f-136">vary-by-query-parameter</span></span>|<span data-ttu-id="f611f-137">Spusťte ukládání do mezipaměti odpovědi na hodnotu zadanými parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="f611f-137">Start caching responses per value of specified query parameters.</span></span> <span data-ttu-id="f611f-138">Zadejte jeden nebo více parametrů.</span><span class="sxs-lookup"><span data-stu-id="f611f-138">Enter a single or multiple parameters.</span></span> <span data-ttu-id="f611f-139">Použijte středník jako oddělovač.</span><span class="sxs-lookup"><span data-stu-id="f611f-139">Use semicolon as a separator.</span></span> <span data-ttu-id="f611f-140">Pokud se zadaný žádný, se používají všechny parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="f611f-140">If none are specified, all query parameters are used.</span></span>|<span data-ttu-id="f611f-141">Ne</span><span class="sxs-lookup"><span data-stu-id="f611f-141">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="f611f-142">Atributy</span><span class="sxs-lookup"><span data-stu-id="f611f-142">Attributes</span></span>  
  
|<span data-ttu-id="f611f-143">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-143">Name</span></span>|<span data-ttu-id="f611f-144">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-144">Description</span></span>|<span data-ttu-id="f611f-145">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-145">Required</span></span>|<span data-ttu-id="f611f-146">Výchozí</span><span class="sxs-lookup"><span data-stu-id="f611f-146">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="f611f-147">Povolit privátní odpovědi-ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-147">allow-private-response-caching</span></span>|<span data-ttu-id="f611f-148">Pokud nastavíte příliš`true`, umožňuje ukládání do mezipaměti požadavků, které obsahují autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="f611f-148">When set too`true`, allows caching of requests that contain an Authorization header.</span></span>|<span data-ttu-id="f611f-149">Ne</span><span class="sxs-lookup"><span data-stu-id="f611f-149">No</span></span>|<span data-ttu-id="f611f-150">False</span><span class="sxs-lookup"><span data-stu-id="f611f-150">false</span></span>|  
|<span data-ttu-id="f611f-151">podřízený typ ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-151">downstream-caching-type</span></span>|<span data-ttu-id="f611f-152">Tento atribut musí být nastavena tooone hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f611f-152">This attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="f611f-153">-none - podřízené ukládání do mezipaměti není povolen.</span><span class="sxs-lookup"><span data-stu-id="f611f-153">-   none - downstream caching is not allowed.</span></span><br /><span data-ttu-id="f611f-154">-soukromý – podřízené privátní ukládání do mezipaměti je povolen.</span><span class="sxs-lookup"><span data-stu-id="f611f-154">-   private - downstream private caching is allowed.</span></span><br /><span data-ttu-id="f611f-155">-veřejný - privátní a sdílené podřízené ukládání do mezipaměti je povolen.</span><span class="sxs-lookup"><span data-stu-id="f611f-155">-   public - private and shared downstream caching is allowed.</span></span>|<span data-ttu-id="f611f-156">Ne</span><span class="sxs-lookup"><span data-stu-id="f611f-156">No</span></span>|<span data-ttu-id="f611f-157">None</span><span class="sxs-lookup"><span data-stu-id="f611f-157">none</span></span>|  
|<span data-ttu-id="f611f-158">musí revalidate</span><span class="sxs-lookup"><span data-stu-id="f611f-158">must-revalidate</span></span>|<span data-ttu-id="f611f-159">Pokud je povoleno ukládání do mezipaměti podřízené tento atribut Zapne nebo vypne hello `must-revalidate` direktiva ovládací prvek mezipaměti v odpovědi brány.</span><span class="sxs-lookup"><span data-stu-id="f611f-159">When downstream caching is enabled this attribute turns on or off  hello `must-revalidate` cache control directive in gateway responses.</span></span>|<span data-ttu-id="f611f-160">Ne</span><span class="sxs-lookup"><span data-stu-id="f611f-160">No</span></span>|<span data-ttu-id="f611f-161">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="f611f-161">true</span></span>|  
|<span data-ttu-id="f611f-162">se liší podle vývojáře</span><span class="sxs-lookup"><span data-stu-id="f611f-162">vary-by-developer</span></span>|<span data-ttu-id="f611f-163">Nastavení příliš`true` toocache odpovědi na vývojáře klíč.</span><span class="sxs-lookup"><span data-stu-id="f611f-163">Set too`true` toocache responses per developer key.</span></span>|<span data-ttu-id="f611f-164">Ne</span><span class="sxs-lookup"><span data-stu-id="f611f-164">No</span></span>|<span data-ttu-id="f611f-165">False</span><span class="sxs-lookup"><span data-stu-id="f611f-165">false</span></span>|  
|<span data-ttu-id="f611f-166">se liší podle vývojáře skupiny –</span><span class="sxs-lookup"><span data-stu-id="f611f-166">vary-by-developer-groups</span></span>|<span data-ttu-id="f611f-167">Nastavení příliš`true` toocache odpovědi na roli uživatele.</span><span class="sxs-lookup"><span data-stu-id="f611f-167">Set too`true` toocache responses per user role.</span></span>|<span data-ttu-id="f611f-168">Ne</span><span class="sxs-lookup"><span data-stu-id="f611f-168">No</span></span>|<span data-ttu-id="f611f-169">False</span><span class="sxs-lookup"><span data-stu-id="f611f-169">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="f611f-170">Využití</span><span class="sxs-lookup"><span data-stu-id="f611f-170">Usage</span></span>  
 <span data-ttu-id="f611f-171">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="f611f-171">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="f611f-172">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="f611f-172">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="f611f-173">**Zásady obory:** rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="f611f-173">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="f611f-174"><a name="StoreToCache"></a>Toocache úložiště</span><span class="sxs-lookup"><span data-stu-id="f611f-174"><a name="StoreToCache"></a> Store toocache</span></span>  
 <span data-ttu-id="f611f-175">Hello `cache-store` odpovědí mezipamětí zásady podle toohello zadaná nastavení mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f611f-175">hello `cache-store` policy caches responses according toohello specified cache settings.</span></span> <span data-ttu-id="f611f-176">Tuto zásadu lze použít v případech, kde odpovědi obsah zůstane statické přes v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="f611f-176">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="f611f-177">Ukládání odpovědí do mezipaměti omezuje šířku pásma a požadavky na zpracování uložených na hello back-end webový server a snižuje latence, zaznamenatelného spotřebiteli rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f611f-177">Response caching reduces bandwidth and processing requirements imposed on hello backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="f611f-178">Tato zásada musí mít odpovídající [získat z mezipaměti](api-management-caching-policies.md#GetFromCache) zásad.</span><span class="sxs-lookup"><span data-stu-id="f611f-178">This policy must have a corresponding [Get from cache](api-management-caching-policies.md#GetFromCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="f611f-179">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="f611f-179">Policy statement</span></span>  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a><span data-ttu-id="f611f-180">Příklady</span><span class="sxs-lookup"><span data-stu-id="f611f-180">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="f611f-181">Příklad</span><span class="sxs-lookup"><span data-stu-id="f611f-181">Example</span></span>  
  
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
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="f611f-182">Příklad pomocí výrazů zásad</span><span class="sxs-lookup"><span data-stu-id="f611f-182">Example using policy expressions</span></span>  
 <span data-ttu-id="f611f-183">Tento příklad ukazuje, jak odpovědi API Management tootooconfigure ukládání do mezipaměti Doba trvání, zda text hello odpovídá ukládání odpovědí do mezipaměti hello back-end služby podle specifikace hello zálohovaný služby `Cache-Control` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="f611f-183">This example shows how tootooconfigure API Management response caching duration that matches hello response caching of hello backend service as specified by hello backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="f611f-184">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too25:25.</span><span class="sxs-lookup"><span data-stu-id="f611f-184">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too25:25.</span></span>  
  
```xml  
<!-- hello following cache policy snippets demonstrate how toocontrol API Management reponse cache duration with Cache-Control headers sent by hello backend service. -->  
  
<!-- Copy this snippet into hello inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into hello outbound section. Note that cache duration is set toohello max-age value provided in hello Cache-Control header received from hello backend service or toohello deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="f611f-185">Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="f611f-185">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="f611f-186">Elementy</span><span class="sxs-lookup"><span data-stu-id="f611f-186">Elements</span></span>  
  
|<span data-ttu-id="f611f-187">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-187">Name</span></span>|<span data-ttu-id="f611f-188">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-188">Description</span></span>|<span data-ttu-id="f611f-189">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-189">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="f611f-190">úložiště mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-190">cache-store</span></span>|<span data-ttu-id="f611f-191">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="f611f-191">Root element.</span></span>|<span data-ttu-id="f611f-192">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-192">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="f611f-193">Atributy</span><span class="sxs-lookup"><span data-stu-id="f611f-193">Attributes</span></span>  
  
|<span data-ttu-id="f611f-194">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-194">Name</span></span>|<span data-ttu-id="f611f-195">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-195">Description</span></span>|<span data-ttu-id="f611f-196">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-196">Required</span></span>|<span data-ttu-id="f611f-197">Výchozí</span><span class="sxs-lookup"><span data-stu-id="f611f-197">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="f611f-198">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="f611f-198">duration</span></span>|<span data-ttu-id="f611f-199">Time-to-live Dobrý den do mezipaměti položky, které jsou zadány v sekundách.</span><span class="sxs-lookup"><span data-stu-id="f611f-199">Time-to-live of hello cached entries, specified in seconds.</span></span>|<span data-ttu-id="f611f-200">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-200">Yes</span></span>|<span data-ttu-id="f611f-201">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="f611f-201">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="f611f-202">Využití</span><span class="sxs-lookup"><span data-stu-id="f611f-202">Usage</span></span>  
 <span data-ttu-id="f611f-203">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="f611f-203">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="f611f-204">**Části zásady:** odchozí</span><span class="sxs-lookup"><span data-stu-id="f611f-204">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="f611f-205">**Zásady obory:** rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="f611f-205">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="f611f-206"><a name="GetFromCacheByKey"></a>Získat hodnotu z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-206"><a name="GetFromCacheByKey"></a> Get value from cache</span></span>  
 <span data-ttu-id="f611f-207">Použití hello `cache-lookup-value` tooperform zásady mezipaměti vyhledávání pomocí klíče a vrátí hodnotu uloženou v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f611f-207">Use hello `cache-lookup-value` policy tooperform cache lookup by key and return a cached value.</span></span> <span data-ttu-id="f611f-208">klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="f611f-208">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="f611f-209">Tato zásada musí mít odpovídající [uložení hodnoty v mezipaměti](#StoreToCacheByKey) zásad.</span><span class="sxs-lookup"><span data-stu-id="f611f-209">This policy must have a corresponding [Store value in cache](#StoreToCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="f611f-210">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="f611f-210">Policy statement</span></span>  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value toouse if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a><span data-ttu-id="f611f-211">Příklad</span><span class="sxs-lookup"><span data-stu-id="f611f-211">Example</span></span>  
 <span data-ttu-id="f611f-212">Další informace a příklady těchto zásad najdete v tématu [vlastní ukládání do mezipaměti ve službě Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="f611f-212">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="f611f-213">Elementy</span><span class="sxs-lookup"><span data-stu-id="f611f-213">Elements</span></span>  
  
|<span data-ttu-id="f611f-214">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-214">Name</span></span>|<span data-ttu-id="f611f-215">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-215">Description</span></span>|<span data-ttu-id="f611f-216">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-216">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="f611f-217">hodnota mezipaměti vyhledávání</span><span class="sxs-lookup"><span data-stu-id="f611f-217">cache-lookup-value</span></span>|<span data-ttu-id="f611f-218">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="f611f-218">Root element.</span></span>|<span data-ttu-id="f611f-219">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="f611f-220">Atributy</span><span class="sxs-lookup"><span data-stu-id="f611f-220">Attributes</span></span>  
  
|<span data-ttu-id="f611f-221">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-221">Name</span></span>|<span data-ttu-id="f611f-222">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-222">Description</span></span>|<span data-ttu-id="f611f-223">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-223">Required</span></span>|<span data-ttu-id="f611f-224">Výchozí</span><span class="sxs-lookup"><span data-stu-id="f611f-224">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="f611f-225">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="f611f-225">default-value</span></span>|<span data-ttu-id="f611f-226">Hodnota, která se přiřadí proměnné toohello Pokud vyhledávání klíčů mezipaměti hello vedlo k neúspěšnému přístupu.</span><span class="sxs-lookup"><span data-stu-id="f611f-226">A value that will be assigned toohello variable if hello cache key lookup resulted in a miss.</span></span> <span data-ttu-id="f611f-227">Pokud se tento atribut nezadá, `null` je přiřazen.</span><span class="sxs-lookup"><span data-stu-id="f611f-227">If this attribute is not specified, `null` is assigned.</span></span>|<span data-ttu-id="f611f-228">Ne</span><span class="sxs-lookup"><span data-stu-id="f611f-228">No</span></span>|`null`|  
|<span data-ttu-id="f611f-229">key</span><span class="sxs-lookup"><span data-stu-id="f611f-229">key</span></span>|<span data-ttu-id="f611f-230">Hodnota klíče toouse hello vyhledávání v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f611f-230">Cache key value toouse in hello lookup.</span></span>|<span data-ttu-id="f611f-231">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-231">Yes</span></span>|<span data-ttu-id="f611f-232">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="f611f-232">N/A</span></span>|  
|<span data-ttu-id="f611f-233">Název proměnné</span><span class="sxs-lookup"><span data-stu-id="f611f-233">variable-name</span></span>|<span data-ttu-id="f611f-234">Název hello [kontextové proměnné](api-management-policy-expressions.md#ContextVariables) hello prohledávat hodnotu budou přiřazeny, pokud je úspěšné vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="f611f-234">Name of hello [context variable](api-management-policy-expressions.md#ContextVariables) hello looked up value will be assigned to, if lookup is successful.</span></span> <span data-ttu-id="f611f-235">Pokud vyhledávání vede k neúspěšnému přístupu do, hello proměnnou, bude přiřazen hello hodnotu hello `default-value` atribut nebo `null`, pokud hello `default-value` atribut vynechán.</span><span class="sxs-lookup"><span data-stu-id="f611f-235">If lookup results in a miss, hello variable will be assigned hello value of hello `default-value` attribute or `null`, if hello `default-value` attribute is omitted.</span></span>|<span data-ttu-id="f611f-236">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-236">Yes</span></span>|<span data-ttu-id="f611f-237">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="f611f-237">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="f611f-238">Využití</span><span class="sxs-lookup"><span data-stu-id="f611f-238">Usage</span></span>  
 <span data-ttu-id="f611f-239">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="f611f-239">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="f611f-240">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="f611f-240">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="f611f-241">**Zásady obory:** globální, rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="f611f-241">**Policy scopes:** global, API, operation, product</span></span>  
  
##  <span data-ttu-id="f611f-242"><a name="StoreToCacheByKey"></a>Hodnota úložiště v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-242"><a name="StoreToCacheByKey"></a> Store value in cache</span></span>  
 <span data-ttu-id="f611f-243">Hello `cache-store-value` provede úložiště mezipaměti podle klíče.</span><span class="sxs-lookup"><span data-stu-id="f611f-243">hello `cache-store-value` performs cache storage by key.</span></span> <span data-ttu-id="f611f-244">klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="f611f-244">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="f611f-245">Tato zásada musí mít odpovídající [získat hodnotu z mezipaměti](#GetFromCacheByKey) zásad.</span><span class="sxs-lookup"><span data-stu-id="f611f-245">This policy must have a corresponding [Get value from cache](#GetFromCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="f611f-246">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="f611f-246">Policy statement</span></span>  
  
```xml  
<cache-store-value key="cache key value" value="value toocache" duration="seconds" />  
```  
  
### <a name="example"></a><span data-ttu-id="f611f-247">Příklad</span><span class="sxs-lookup"><span data-stu-id="f611f-247">Example</span></span>  
 <span data-ttu-id="f611f-248">Další informace a příklady těchto zásad najdete v tématu [vlastní ukládání do mezipaměti ve službě Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="f611f-248">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="f611f-249">Elementy</span><span class="sxs-lookup"><span data-stu-id="f611f-249">Elements</span></span>  
  
|<span data-ttu-id="f611f-250">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-250">Name</span></span>|<span data-ttu-id="f611f-251">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-251">Description</span></span>|<span data-ttu-id="f611f-252">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-252">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="f611f-253">Hodnota úložiště mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-253">cache-store-value</span></span>|<span data-ttu-id="f611f-254">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="f611f-254">Root element.</span></span>|<span data-ttu-id="f611f-255">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-255">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="f611f-256">Atributy</span><span class="sxs-lookup"><span data-stu-id="f611f-256">Attributes</span></span>  
  
|<span data-ttu-id="f611f-257">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-257">Name</span></span>|<span data-ttu-id="f611f-258">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-258">Description</span></span>|<span data-ttu-id="f611f-259">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-259">Required</span></span>|<span data-ttu-id="f611f-260">Výchozí</span><span class="sxs-lookup"><span data-stu-id="f611f-260">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="f611f-261">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="f611f-261">duration</span></span>|<span data-ttu-id="f611f-262">Hodnota bude do mezipaměti pro hello zadat hodnotu doby trvání, zadaný v sekundách.</span><span class="sxs-lookup"><span data-stu-id="f611f-262">Value will be cached for hello provided duration value, specified in seconds.</span></span>|<span data-ttu-id="f611f-263">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-263">Yes</span></span>|<span data-ttu-id="f611f-264">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="f611f-264">N/A</span></span>|  
|<span data-ttu-id="f611f-265">key</span><span class="sxs-lookup"><span data-stu-id="f611f-265">key</span></span>|<span data-ttu-id="f611f-266">Hodnota klíče hello mezipaměti bude uložena v části.</span><span class="sxs-lookup"><span data-stu-id="f611f-266">Cache key hello value will be stored under.</span></span>|<span data-ttu-id="f611f-267">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-267">Yes</span></span>|<span data-ttu-id="f611f-268">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="f611f-268">N/A</span></span>|  
|<span data-ttu-id="f611f-269">hodnota</span><span class="sxs-lookup"><span data-stu-id="f611f-269">value</span></span>|<span data-ttu-id="f611f-270">toobe hodnota Hello do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f611f-270">hello value toobe cached.</span></span>|<span data-ttu-id="f611f-271">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-271">Yes</span></span>|<span data-ttu-id="f611f-272">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="f611f-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="f611f-273">Využití</span><span class="sxs-lookup"><span data-stu-id="f611f-273">Usage</span></span>  
 <span data-ttu-id="f611f-274">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="f611f-274">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="f611f-275">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="f611f-275">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="f611f-276">**Zásady obory:** globální, rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="f611f-276">**Policy scopes:** global, API, operation, product</span></span>  
  
###  <span data-ttu-id="f611f-277"><a name="RemoveCacheByKey"></a>Odebrat hodnotu z mezipaměti</span><span class="sxs-lookup"><span data-stu-id="f611f-277"><a name="RemoveCacheByKey"></a> Remove value from cache</span></span>  
 <span data-ttu-id="f611f-278">Hello `cache-remove-value` odstraní identifikovaný svůj klíč položky v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f611f-278">hello             `cache-remove-value` deletes a cached item identified by its key.</span></span> <span data-ttu-id="f611f-279">klíč Hello může mít hodnotu libovolný řetězec a se většinou poskytuje pomocí výrazů zásad.</span><span class="sxs-lookup"><span data-stu-id="f611f-279">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
#### <a name="policy-statement"></a><span data-ttu-id="f611f-280">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="f611f-280">Policy statement</span></span>  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="f611f-281">Příklad</span><span class="sxs-lookup"><span data-stu-id="f611f-281">Example</span></span>  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a><span data-ttu-id="f611f-282">Elementy</span><span class="sxs-lookup"><span data-stu-id="f611f-282">Elements</span></span>  
  
|<span data-ttu-id="f611f-283">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-283">Name</span></span>|<span data-ttu-id="f611f-284">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-284">Description</span></span>|<span data-ttu-id="f611f-285">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-285">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="f611f-286">hodnota mezipaměti odebrat</span><span class="sxs-lookup"><span data-stu-id="f611f-286">cache-remove-value</span></span>|<span data-ttu-id="f611f-287">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="f611f-287">Root element.</span></span>|<span data-ttu-id="f611f-288">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-288">Yes</span></span>|  
  
#### <a name="attributes"></a><span data-ttu-id="f611f-289">Atributy</span><span class="sxs-lookup"><span data-stu-id="f611f-289">Attributes</span></span>  
  
|<span data-ttu-id="f611f-290">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f611f-290">Name</span></span>|<span data-ttu-id="f611f-291">Popis</span><span class="sxs-lookup"><span data-stu-id="f611f-291">Description</span></span>|<span data-ttu-id="f611f-292">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="f611f-292">Required</span></span>|<span data-ttu-id="f611f-293">Výchozí</span><span class="sxs-lookup"><span data-stu-id="f611f-293">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="f611f-294">key</span><span class="sxs-lookup"><span data-stu-id="f611f-294">key</span></span>|<span data-ttu-id="f611f-295">klíč Hello hello dříve do mezipaměti hodnotu toobe odebraných z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="f611f-295">hello key of hello previously cached value toobe removed from hello cache.</span></span>|<span data-ttu-id="f611f-296">Ano</span><span class="sxs-lookup"><span data-stu-id="f611f-296">Yes</span></span>|<span data-ttu-id="f611f-297">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="f611f-297">N/A</span></span>|  
  
#### <a name="usage"></a><span data-ttu-id="f611f-298">Využití</span><span class="sxs-lookup"><span data-stu-id="f611f-298">Usage</span></span>  
 <span data-ttu-id="f611f-299">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="f611f-299">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="f611f-300">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="f611f-300">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="f611f-301">**Zásady obory:** globální, rozhraní API, operace, produktu</span><span class="sxs-lookup"><span data-stu-id="f611f-301">**Policy scopes:** global, API, operation, product</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="f611f-302">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f611f-302">Next steps</span></span>
<span data-ttu-id="f611f-303">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="f611f-303">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  