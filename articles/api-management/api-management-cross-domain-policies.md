---
title: "aaaAzure API Management napříč zásady domény | Microsoft Docs"
description: "Další informace o hello křížové zásady domény, které jsou k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: dd5ebfd65b92ebd0c1f589a2bac669a3928d40b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-cross-domain-policies"></a><span data-ttu-id="c2793-103">Zásady pro API Management napříč doménami</span><span class="sxs-lookup"><span data-stu-id="c2793-103">API Management cross domain policies</span></span>
<span data-ttu-id="c2793-104">Toto téma obsahuje odkaz pro hello následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c2793-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="c2793-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="c2793-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="c2793-106"><a name="CrossDomainPolicies"></a>Různé zásady domény</span><span class="sxs-lookup"><span data-stu-id="c2793-106"><a name="CrossDomainPolicies"></a> Cross domain policies</span></span>  
  
-   <span data-ttu-id="c2793-107">[Povolit volání mezi doménami](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -zpřístupní hello rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="c2793-107">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
-   <span data-ttu-id="c2793-108">[CORS](api-management-cross-domain-policies.md#CORS) -přidá sdílení prostředků různých původů (CORS) podporoval tooan operaci nebo volání rozhraní API tooallow napříč doménami založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="c2793-108">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
-   <span data-ttu-id="c2793-109">[JSONP](api-management-cross-domain-policies.md#JSONP) – přidá JSON s odsazení (JSONP) operaci tooan podpory nebo rozhraní API tooallow napříč doménami volá z klientů JavaScript založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c2793-109">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
##  <span data-ttu-id="c2793-110"><a name="AllowCrossDomainCalls"></a>Povolit volání mezi doménami</span><span class="sxs-lookup"><span data-stu-id="c2793-110"><a name="AllowCrossDomainCalls"></a> Allow cross-domain calls</span></span>  
 <span data-ttu-id="c2793-111">Použití hello `cross-domain` zásad toomake hello rozhraní API, které jsou přístupné z Adobe Flash a Microsoft Silverlight klienty založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c2793-111">Use hello `cross-domain` policy toomake hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="c2793-112">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="c2793-112">Policy statement</span></span>  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in hello Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a><span data-ttu-id="c2793-113">Příklad</span><span class="sxs-lookup"><span data-stu-id="c2793-113">Example</span></span>  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a><span data-ttu-id="c2793-114">Elementy</span><span class="sxs-lookup"><span data-stu-id="c2793-114">Elements</span></span>  
  
|<span data-ttu-id="c2793-115">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c2793-115">Name</span></span>|<span data-ttu-id="c2793-116">Popis</span><span class="sxs-lookup"><span data-stu-id="c2793-116">Description</span></span>|<span data-ttu-id="c2793-117">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c2793-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="c2793-118">mezi doménami</span><span class="sxs-lookup"><span data-stu-id="c2793-118">cross-domain</span></span>|<span data-ttu-id="c2793-119">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="c2793-119">Root element.</span></span> <span data-ttu-id="c2793-120">Podřízené elementy musí odpovídat toohello [specifikaci souboru zásady mezi doménami Adobe](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span><span class="sxs-lookup"><span data-stu-id="c2793-120">Child elements must conform toohello [Adobe cross-domain policy file specification](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span></span>|<span data-ttu-id="c2793-121">Ano</span><span class="sxs-lookup"><span data-stu-id="c2793-121">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="c2793-122">Využití</span><span class="sxs-lookup"><span data-stu-id="c2793-122">Usage</span></span>  
 <span data-ttu-id="c2793-123">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="c2793-123">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="c2793-124">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="c2793-124">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="c2793-125">**Zásady obory:** globální</span><span class="sxs-lookup"><span data-stu-id="c2793-125">**Policy scopes:** global</span></span>  
  
##  <span data-ttu-id="c2793-126"><a name="CORS"></a>CORS</span><span class="sxs-lookup"><span data-stu-id="c2793-126"><a name="CORS"></a> CORS</span></span>  
 <span data-ttu-id="c2793-127">Hello `cors` zásad přidá sdílení prostředků různých původů (CORS) podporoval tooan operaci nebo volání rozhraní API tooallow napříč doménami založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="c2793-127">hello `cors` policy adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
 <span data-ttu-id="c2793-128">CORS umožňuje prohlížeče a server toointeract a zjistěte, zda tooallow konkrétní cross-origin požadavky (tj. XMLHttpRequests volání z jazyka JavaScript webové stránce tooother domény).</span><span class="sxs-lookup"><span data-stu-id="c2793-128">CORS allows a browser and a server toointeract and determine whether or not tooallow specific cross-origin requests (i.e. XMLHttpRequests calls made from JavaScript on a web page tooother domains).</span></span> <span data-ttu-id="c2793-129">To umožňuje více flexibility než povolíte jen žádostí stejné zdroji, ale je bezpečnější než povolení všech žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="c2793-129">This allows for more flexibility than only allowing same-origin requests, but is more secure than allowing all cross-origin requests.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="c2793-130">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="c2793-130">Policy statement</span></span>  
  
```xml  
<cors allow-credentials="false|true">  
    <allowed-origins>  
        <origin>origin uri</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="number of seconds">  
        <method>http verb</method>  
    </allowed-methods>  
    <allowed-headers>  
        <header>header name</header>  
    </allowed-headers>  
    <expose-headers>  
        <header>header name</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="example"></a><span data-ttu-id="c2793-131">Příklad</span><span class="sxs-lookup"><span data-stu-id="c2793-131">Example</span></span>  
 <span data-ttu-id="c2793-132">Tento příklad ukazuje, jak před letu toosupport požadavky, například s vlastní hlavičky nebo jiných metod než GET a POST.</span><span class="sxs-lookup"><span data-stu-id="c2793-132">This example demonstrates how toosupport pre-flight requests, such as those with custom headers or methods other than GET and POST.</span></span> <span data-ttu-id="c2793-133">vlastní hlavičky toosupport a další příkazy HTTP, používat hello `allowed-methods` a `allowed-headers` částech, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="c2793-133">toosupport custom headers and additional HTTP verbs, use hello `allowed-methods` and `allowed-headers` sections as shown in hello following example.</span></span>  
  
```xml  
<cors allow-credentials="true">  
    <allowed-origins>  
        <!-- Localhost useful for development -->  
        <origin>http://localhost:8080/</origin>  
        <origin>http://example.com/</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="300">  
        <method>GET</method>  
        <method>POST</method>  
        <method>PATCH</method>  
        <method>DELETE</method>  
    </allowed-methods>  
    <allowed-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
        <header>x-zumo-version</header>  
        <header>x-zumo-auth</header>  
        <header>content-type</header>  
        <header>accept</header>  
    </allowed-headers>  
    <expose-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="elements"></a><span data-ttu-id="c2793-134">Elementy</span><span class="sxs-lookup"><span data-stu-id="c2793-134">Elements</span></span>  
  
|<span data-ttu-id="c2793-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c2793-135">Name</span></span>|<span data-ttu-id="c2793-136">Popis</span><span class="sxs-lookup"><span data-stu-id="c2793-136">Description</span></span>|<span data-ttu-id="c2793-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c2793-137">Required</span></span>|<span data-ttu-id="c2793-138">Výchozí</span><span class="sxs-lookup"><span data-stu-id="c2793-138">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="c2793-139">cors</span><span class="sxs-lookup"><span data-stu-id="c2793-139">cors</span></span>|<span data-ttu-id="c2793-140">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="c2793-140">Root element.</span></span>|<span data-ttu-id="c2793-141">Ano</span><span class="sxs-lookup"><span data-stu-id="c2793-141">Yes</span></span>|<span data-ttu-id="c2793-142">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c2793-142">N/A</span></span>|  
|<span data-ttu-id="c2793-143">Povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="c2793-143">allowed-origins</span></span>|<span data-ttu-id="c2793-144">Obsahuje `origin` prvky, které popisují hello povolené zdroje pro požadavky napříč doménami.</span><span class="sxs-lookup"><span data-stu-id="c2793-144">Contains `origin` elements that describe hello allowed origins for cross-domain requests.</span></span> <span data-ttu-id="c2793-145">`allowed-origins`může obsahovat buď jediný `origin` element, který určuje `*` tooallow jakýkoli původ nebo jeden nebo více `origin` elementy, které obsahují identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="c2793-145">`allowed-origins` can contain either a single `origin` element that specifies `*` tooallow any origin, or one or more `origin` elements that contain a URI.</span></span>|<span data-ttu-id="c2793-146">Ano</span><span class="sxs-lookup"><span data-stu-id="c2793-146">Yes</span></span>|<span data-ttu-id="c2793-147">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c2793-147">N/A</span></span>|  
|<span data-ttu-id="c2793-148">Počátek</span><span class="sxs-lookup"><span data-stu-id="c2793-148">origin</span></span>|<span data-ttu-id="c2793-149">Hello hodnota může být buď `*` tooallow všechny původy nebo identifikátor URI, který určuje jeden počátek.</span><span class="sxs-lookup"><span data-stu-id="c2793-149">hello value can be either `*` tooallow all origins, or a URI that specifies a single origin.</span></span> <span data-ttu-id="c2793-150">Hello identifikátor URI musí obsahovat schéma, hostitele a portu.</span><span class="sxs-lookup"><span data-stu-id="c2793-150">hello URI must include a scheme, host, and port.</span></span>|<span data-ttu-id="c2793-151">Ano</span><span class="sxs-lookup"><span data-stu-id="c2793-151">Yes</span></span>|<span data-ttu-id="c2793-152">Při vynechání je hello port v identifikátoru URI je používá port 80 pro protokol HTTP a port 443 se používá pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c2793-152">If hello port is omitted in a URI, port 80 is used for HTTP and port 443 is used for HTTPS.</span></span>|  
|<span data-ttu-id="c2793-153">povolené metody</span><span class="sxs-lookup"><span data-stu-id="c2793-153">allowed-methods</span></span>|<span data-ttu-id="c2793-154">Tento prvek je nutný, pokud metody jiné než GET nebo POST jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="c2793-154">This element is required if methods other than GET or POST are allowed.</span></span> <span data-ttu-id="c2793-155">Obsahuje `method` elementy, které určují hello podporované příkazy HTTP.</span><span class="sxs-lookup"><span data-stu-id="c2793-155">Contains `method` elements that specify hello supported HTTP verbs.</span></span>|<span data-ttu-id="c2793-156">Ne</span><span class="sxs-lookup"><span data-stu-id="c2793-156">No</span></span>|<span data-ttu-id="c2793-157">Pokud v této části není zadán, jsou podporovány GET a POST.</span><span class="sxs-lookup"><span data-stu-id="c2793-157">If this section is not present, GET and POST are supported.</span></span>|  
|<span data-ttu-id="c2793-158">– Metoda</span><span class="sxs-lookup"><span data-stu-id="c2793-158">method</span></span>|<span data-ttu-id="c2793-159">Určuje příkaz HTTP.</span><span class="sxs-lookup"><span data-stu-id="c2793-159">Specifies an HTTP verb.</span></span>|<span data-ttu-id="c2793-160">Alespoň jeden `method` prvek je nutný, pokud hello `allowed-methods` část je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c2793-160">At least one `method` element is required if hello `allowed-methods` section is present.</span></span>|<span data-ttu-id="c2793-161">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c2793-161">N/A</span></span>|  
|<span data-ttu-id="c2793-162">povolené hlavičky</span><span class="sxs-lookup"><span data-stu-id="c2793-162">allowed-headers</span></span>|<span data-ttu-id="c2793-163">Tento prvek obsahuje `header` elementy zadání názvy hello hlavičky, které můžou být součástí hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="c2793-163">This element contains `header` elements specifying names of hello headers that can be included in hello request.</span></span>|<span data-ttu-id="c2793-164">Ne</span><span class="sxs-lookup"><span data-stu-id="c2793-164">No</span></span>|<span data-ttu-id="c2793-165">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c2793-165">N/A</span></span>|  
|<span data-ttu-id="c2793-166">Zveřejněte hlavičky</span><span class="sxs-lookup"><span data-stu-id="c2793-166">expose-headers</span></span>|<span data-ttu-id="c2793-167">Tento prvek obsahuje `header` elementy zadání názvy hello hlavičky, které budou přístupné klientem hello.</span><span class="sxs-lookup"><span data-stu-id="c2793-167">This element contains `header` elements specifying names of hello headers that will be accessible by hello client.</span></span>|<span data-ttu-id="c2793-168">Ne</span><span class="sxs-lookup"><span data-stu-id="c2793-168">No</span></span>|<span data-ttu-id="c2793-169">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c2793-169">N/A</span></span>|  
|<span data-ttu-id="c2793-170">záhlaví</span><span class="sxs-lookup"><span data-stu-id="c2793-170">header</span></span>|<span data-ttu-id="c2793-171">Určuje název hlavičky.</span><span class="sxs-lookup"><span data-stu-id="c2793-171">Specifies a header name.</span></span>|<span data-ttu-id="c2793-172">Alespoň jeden `header` element je vyžadována ve `allowed-headers` nebo `expose-headers` Pokud hello části nachází.</span><span class="sxs-lookup"><span data-stu-id="c2793-172">At least one `header` element is required in `allowed-headers` or `expose-headers` if hello section is present.</span></span>|<span data-ttu-id="c2793-173">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c2793-173">N/A</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="c2793-174">Atributy</span><span class="sxs-lookup"><span data-stu-id="c2793-174">Attributes</span></span>  
  
|<span data-ttu-id="c2793-175">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c2793-175">Name</span></span>|<span data-ttu-id="c2793-176">Popis</span><span class="sxs-lookup"><span data-stu-id="c2793-176">Description</span></span>|<span data-ttu-id="c2793-177">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c2793-177">Required</span></span>|<span data-ttu-id="c2793-178">Výchozí</span><span class="sxs-lookup"><span data-stu-id="c2793-178">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="c2793-179">Povolit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="c2793-179">allow-credentials</span></span>|<span data-ttu-id="c2793-180">Hello `Access-Control-Allow-Credentials` hlavičky v hello předběžných odpovědi bude nastavte toohello hodnotu tohoto atributu a ovlivnit hello klienta možnost toosubmit přihlašovací údaje v žádosti napříč doménami.</span><span class="sxs-lookup"><span data-stu-id="c2793-180">hello `Access-Control-Allow-Credentials` header in hello preflight response will be set toohello value of this attribute and affect hello client’s ability toosubmit credentials in cross-domain requests.</span></span>|<span data-ttu-id="c2793-181">Ne</span><span class="sxs-lookup"><span data-stu-id="c2793-181">No</span></span>|<span data-ttu-id="c2793-182">False</span><span class="sxs-lookup"><span data-stu-id="c2793-182">false</span></span>|  
|<span data-ttu-id="c2793-183">Kontrola před výstupem výsledek max-age</span><span class="sxs-lookup"><span data-stu-id="c2793-183">preflight-result-max-age</span></span>|<span data-ttu-id="c2793-184">Hello `Access-Control-Max-Age` hlavičky v hello předběžných odpovědi bude nastavte toohello hodnotu tohoto atributu a ovlivnit hello uživatelský agent možnost toocache před letu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c2793-184">hello `Access-Control-Max-Age` header in hello preflight response will be set toohello value of this attribute and affect hello user agent’s ability toocache pre-flight response.</span></span>|<span data-ttu-id="c2793-185">Ne</span><span class="sxs-lookup"><span data-stu-id="c2793-185">No</span></span>|<span data-ttu-id="c2793-186">0</span><span class="sxs-lookup"><span data-stu-id="c2793-186">0</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="c2793-187">Využití</span><span class="sxs-lookup"><span data-stu-id="c2793-187">Usage</span></span>  
 <span data-ttu-id="c2793-188">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="c2793-188">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="c2793-189">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="c2793-189">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="c2793-190">**Zásady obory:** rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="c2793-190">**Policy scopes:** API, operation</span></span>  
  
##  <span data-ttu-id="c2793-191"><a name="JSONP"></a>JSONP</span><span class="sxs-lookup"><span data-stu-id="c2793-191"><a name="JSONP"></a> JSONP</span></span>  
 <span data-ttu-id="c2793-192">Hello `jsonp` zásad přidá JSON s odsazení operace tooan podporu (JSONP) nebo volání rozhraní API tooallow napříč doménami z klientů JavaScript založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c2793-192">hello `jsonp` policy adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span> <span data-ttu-id="c2793-193">JSONP je metoda použitá při JavaScript programy toorequest data ze serveru v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="c2793-193">JSONP is a method used in JavaScript programs toorequest data from a server in a different domain.</span></span> <span data-ttu-id="c2793-194">JSONP obchází hello omezení vynucená většina webových prohlížečů, kde musí být tooweb stránek v hello stejné domény.</span><span class="sxs-lookup"><span data-stu-id="c2793-194">JSONP bypasses hello limitation enforced by most web browsers where access tooweb pages must be in hello same domain.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="c2793-195">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="c2793-195">Policy statement</span></span>  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a><span data-ttu-id="c2793-196">Příklad</span><span class="sxs-lookup"><span data-stu-id="c2793-196">Example</span></span>  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 <span data-ttu-id="c2793-197">Pokud jste volali metodu hello bez parametru zpětného volání hello? cb = XXX vrátí prostý JSON (bez obálku volání funkce).</span><span class="sxs-lookup"><span data-stu-id="c2793-197">If you call hello method without hello callback parameter ?cb=XXX it will return plain JSON (without a function call wrapper).</span></span>  
  
 <span data-ttu-id="c2793-198">Pokud přidáte parametr zpětného volání hello `?cb=XXX` vrátí výsledku JSONP zabalení hello původní výsledky JSON kolem funkce zpětného volání hello jako`XYZ('<json result goes here>');`</span><span class="sxs-lookup"><span data-stu-id="c2793-198">If you add hello callback parameter `?cb=XXX` it will return a JSONP result, wrapping hello original JSON results around hello callback function like `XYZ('<json result goes here>');`</span></span>  
  
### <a name="elements"></a><span data-ttu-id="c2793-199">Elementy</span><span class="sxs-lookup"><span data-stu-id="c2793-199">Elements</span></span>  
  
|<span data-ttu-id="c2793-200">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c2793-200">Name</span></span>|<span data-ttu-id="c2793-201">Popis</span><span class="sxs-lookup"><span data-stu-id="c2793-201">Description</span></span>|<span data-ttu-id="c2793-202">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c2793-202">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="c2793-203">JSONP</span><span class="sxs-lookup"><span data-stu-id="c2793-203">jsonp</span></span>|<span data-ttu-id="c2793-204">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="c2793-204">Root element.</span></span>|<span data-ttu-id="c2793-205">Ano</span><span class="sxs-lookup"><span data-stu-id="c2793-205">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="c2793-206">Atributy</span><span class="sxs-lookup"><span data-stu-id="c2793-206">Attributes</span></span>  
  
|<span data-ttu-id="c2793-207">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c2793-207">Name</span></span>|<span data-ttu-id="c2793-208">Popis</span><span class="sxs-lookup"><span data-stu-id="c2793-208">Description</span></span>|<span data-ttu-id="c2793-209">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c2793-209">Required</span></span>|<span data-ttu-id="c2793-210">Výchozí</span><span class="sxs-lookup"><span data-stu-id="c2793-210">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="c2793-211">Název parametru zpětného volání</span><span class="sxs-lookup"><span data-stu-id="c2793-211">callback-parameter-name</span></span>|<span data-ttu-id="c2793-212">Hello volání funkce JavaScript napříč doménami s předponou hello plně kvalifikovaný název domény, kde hello funkce nachází.</span><span class="sxs-lookup"><span data-stu-id="c2793-212">hello cross-domain JavaScript function call prefixed with hello fully qualified domain name where hello function resides.</span></span>|<span data-ttu-id="c2793-213">Ano</span><span class="sxs-lookup"><span data-stu-id="c2793-213">Yes</span></span>|<span data-ttu-id="c2793-214">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c2793-214">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="c2793-215">Využití</span><span class="sxs-lookup"><span data-stu-id="c2793-215">Usage</span></span>  
 <span data-ttu-id="c2793-216">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="c2793-216">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="c2793-217">**Části zásady:** odchozí</span><span class="sxs-lookup"><span data-stu-id="c2793-217">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="c2793-218">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="c2793-218">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="c2793-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2793-219">Next steps</span></span>
<span data-ttu-id="c2793-220">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="c2793-220">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  