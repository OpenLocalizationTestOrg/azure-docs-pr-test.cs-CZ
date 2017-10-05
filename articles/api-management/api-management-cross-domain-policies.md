---
title: "Azure API Management napříč zásady domény | Microsoft Docs"
description: "Další informace o k dispozici pro použití v Azure API Management napříč doménami zásady."
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
ms.openlocfilehash: ddca9e35b44a21294abbb5eaa4418bcdb85494cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-cross-domain-policies"></a><span data-ttu-id="ccd90-103">Zásady pro API Management napříč doménami</span><span class="sxs-lookup"><span data-stu-id="ccd90-103">API Management cross domain policies</span></span>
<span data-ttu-id="ccd90-104">Toto téma obsahuje odkaz pro následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="ccd90-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="ccd90-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="ccd90-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="ccd90-106"><a name="CrossDomainPolicies"></a>Různé zásady domény</span><span class="sxs-lookup"><span data-stu-id="ccd90-106"><a name="CrossDomainPolicies"></a> Cross domain policies</span></span>  
  
-   <span data-ttu-id="ccd90-107">[Povolit volání mezi doménami](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -zpřístupní rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="ccd90-107">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
-   <span data-ttu-id="ccd90-108">[CORS](api-management-cross-domain-policies.md#CORS) – přidává podporu (CORS), aby operace nebo rozhraní API umožňující volání napříč doménami založené na prohlížeči klientů pro sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="ccd90-108">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>  
  
-   <span data-ttu-id="ccd90-109">[JSONP](api-management-cross-domain-policies.md#JSONP) -přidá JSON s podporou odsazení (JSONP) operace nebo rozhraní API, chcete-li povolit volání mezi doménami z klientů JavaScript založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ccd90-109">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
##  <span data-ttu-id="ccd90-110"><a name="AllowCrossDomainCalls"></a>Povolit volání mezi doménami</span><span class="sxs-lookup"><span data-stu-id="ccd90-110"><a name="AllowCrossDomainCalls"></a> Allow cross-domain calls</span></span>  
 <span data-ttu-id="ccd90-111">Použití `cross-domain` zásady zpřístupněte rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.</span><span class="sxs-lookup"><span data-stu-id="ccd90-111">Use the `cross-domain` policy to make the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="ccd90-112">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="ccd90-112">Policy statement</span></span>  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in the Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a><span data-ttu-id="ccd90-113">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccd90-113">Example</span></span>  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a><span data-ttu-id="ccd90-114">Elementy</span><span class="sxs-lookup"><span data-stu-id="ccd90-114">Elements</span></span>  
  
|<span data-ttu-id="ccd90-115">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ccd90-115">Name</span></span>|<span data-ttu-id="ccd90-116">Popis</span><span class="sxs-lookup"><span data-stu-id="ccd90-116">Description</span></span>|<span data-ttu-id="ccd90-117">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ccd90-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="ccd90-118">mezi doménami</span><span class="sxs-lookup"><span data-stu-id="ccd90-118">cross-domain</span></span>|<span data-ttu-id="ccd90-119">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="ccd90-119">Root element.</span></span> <span data-ttu-id="ccd90-120">Podřízené elementy musí odpovídat [specifikaci souboru zásady mezi doménami Adobe](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span><span class="sxs-lookup"><span data-stu-id="ccd90-120">Child elements must conform to the [Adobe cross-domain policy file specification](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span></span>|<span data-ttu-id="ccd90-121">Ano</span><span class="sxs-lookup"><span data-stu-id="ccd90-121">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="ccd90-122">Využití</span><span class="sxs-lookup"><span data-stu-id="ccd90-122">Usage</span></span>  
 <span data-ttu-id="ccd90-123">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="ccd90-123">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="ccd90-124">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="ccd90-124">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="ccd90-125">**Zásady obory:** globální</span><span class="sxs-lookup"><span data-stu-id="ccd90-125">**Policy scopes:** global</span></span>  
  
##  <span data-ttu-id="ccd90-126"><a name="CORS"></a>CORS</span><span class="sxs-lookup"><span data-stu-id="ccd90-126"><a name="CORS"></a> CORS</span></span>  
 <span data-ttu-id="ccd90-127">`cors` Zásad přidá podporu (CORS), aby operace nebo rozhraní API umožňující volání napříč doménami založené na prohlížeči klientů pro sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="ccd90-127">The `cors` policy adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>  
  
 <span data-ttu-id="ccd90-128">CORS umožňuje prohlížečem a serverem komunikovat a zjistěte, zda chcete povolit konkrétní požadavky cross-origin (tj. XMLHttpRequests volání z jazyka JavaScript na webové stránce do jiných domén).</span><span class="sxs-lookup"><span data-stu-id="ccd90-128">CORS allows a browser and a server to interact and determine whether or not to allow specific cross-origin requests (i.e. XMLHttpRequests calls made from JavaScript on a web page to other domains).</span></span> <span data-ttu-id="ccd90-129">To umožňuje více flexibility než povolíte jen žádostí stejné zdroji, ale je bezpečnější než povolení všech žádostí napříč zdroji.</span><span class="sxs-lookup"><span data-stu-id="ccd90-129">This allows for more flexibility than only allowing same-origin requests, but is more secure than allowing all cross-origin requests.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="ccd90-130">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="ccd90-130">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="ccd90-131">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccd90-131">Example</span></span>  
 <span data-ttu-id="ccd90-132">Tento příklad ukazuje, jak pro podporu požadavků před letu, například těch, které se vlastní hlavičky nebo jiných metod než GET a POST.</span><span class="sxs-lookup"><span data-stu-id="ccd90-132">This example demonstrates how to support pre-flight requests, such as those with custom headers or methods other than GET and POST.</span></span> <span data-ttu-id="ccd90-133">Chcete-li podporovat vlastní hlavičky a další příkazy HTTP, použijte `allowed-methods` a `allowed-headers` částech, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="ccd90-133">To support custom headers and additional HTTP verbs, use the `allowed-methods` and `allowed-headers` sections as shown in the following example.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="ccd90-134">Elementy</span><span class="sxs-lookup"><span data-stu-id="ccd90-134">Elements</span></span>  
  
|<span data-ttu-id="ccd90-135">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ccd90-135">Name</span></span>|<span data-ttu-id="ccd90-136">Popis</span><span class="sxs-lookup"><span data-stu-id="ccd90-136">Description</span></span>|<span data-ttu-id="ccd90-137">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ccd90-137">Required</span></span>|<span data-ttu-id="ccd90-138">Výchozí</span><span class="sxs-lookup"><span data-stu-id="ccd90-138">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="ccd90-139">cors</span><span class="sxs-lookup"><span data-stu-id="ccd90-139">cors</span></span>|<span data-ttu-id="ccd90-140">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="ccd90-140">Root element.</span></span>|<span data-ttu-id="ccd90-141">Ano</span><span class="sxs-lookup"><span data-stu-id="ccd90-141">Yes</span></span>|<span data-ttu-id="ccd90-142">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ccd90-142">N/A</span></span>|  
|<span data-ttu-id="ccd90-143">Povolené zdroje</span><span class="sxs-lookup"><span data-stu-id="ccd90-143">allowed-origins</span></span>|<span data-ttu-id="ccd90-144">Obsahuje `origin` prvky, které popisují povolené zdroje pro požadavky napříč doménami.</span><span class="sxs-lookup"><span data-stu-id="ccd90-144">Contains `origin` elements that describe the allowed origins for cross-domain requests.</span></span> <span data-ttu-id="ccd90-145">`allowed-origins`může obsahovat buď jediný `origin` element, který určuje `*` umožňující jakýkoli původ nebo jeden nebo více `origin` elementy, které obsahují identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="ccd90-145">`allowed-origins` can contain either a single `origin` element that specifies `*` to allow any origin, or one or more `origin` elements that contain a URI.</span></span>|<span data-ttu-id="ccd90-146">Ano</span><span class="sxs-lookup"><span data-stu-id="ccd90-146">Yes</span></span>|<span data-ttu-id="ccd90-147">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ccd90-147">N/A</span></span>|  
|<span data-ttu-id="ccd90-148">Počátek</span><span class="sxs-lookup"><span data-stu-id="ccd90-148">origin</span></span>|<span data-ttu-id="ccd90-149">Hodnota může být buď `*` umožňuje všechny původy nebo identifikátor URI, který určuje jeden počátek.</span><span class="sxs-lookup"><span data-stu-id="ccd90-149">The value can be either `*` to allow all origins, or a URI that specifies a single origin.</span></span> <span data-ttu-id="ccd90-150">Identifikátor URI musí obsahovat schéma, hostitele a portu.</span><span class="sxs-lookup"><span data-stu-id="ccd90-150">The URI must include a scheme, host, and port.</span></span>|<span data-ttu-id="ccd90-151">Ano</span><span class="sxs-lookup"><span data-stu-id="ccd90-151">Yes</span></span>|<span data-ttu-id="ccd90-152">Při vynechání je port v identifikátoru URI je používá port 80 pro protokol HTTP a port 443 se používá pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ccd90-152">If the port is omitted in a URI, port 80 is used for HTTP and port 443 is used for HTTPS.</span></span>|  
|<span data-ttu-id="ccd90-153">povolené metody</span><span class="sxs-lookup"><span data-stu-id="ccd90-153">allowed-methods</span></span>|<span data-ttu-id="ccd90-154">Tento prvek je nutný, pokud metody jiné než GET nebo POST jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="ccd90-154">This element is required if methods other than GET or POST are allowed.</span></span> <span data-ttu-id="ccd90-155">Obsahuje `method` elementy, které určují, podporované příkazy HTTP.</span><span class="sxs-lookup"><span data-stu-id="ccd90-155">Contains `method` elements that specify the supported HTTP verbs.</span></span>|<span data-ttu-id="ccd90-156">Ne</span><span class="sxs-lookup"><span data-stu-id="ccd90-156">No</span></span>|<span data-ttu-id="ccd90-157">Pokud v této části není zadán, jsou podporovány GET a POST.</span><span class="sxs-lookup"><span data-stu-id="ccd90-157">If this section is not present, GET and POST are supported.</span></span>|  
|<span data-ttu-id="ccd90-158">– Metoda</span><span class="sxs-lookup"><span data-stu-id="ccd90-158">method</span></span>|<span data-ttu-id="ccd90-159">Určuje příkaz HTTP.</span><span class="sxs-lookup"><span data-stu-id="ccd90-159">Specifies an HTTP verb.</span></span>|<span data-ttu-id="ccd90-160">Alespoň jeden `method` prvek je nutný, pokud `allowed-methods` část je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ccd90-160">At least one `method` element is required if the `allowed-methods` section is present.</span></span>|<span data-ttu-id="ccd90-161">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ccd90-161">N/A</span></span>|  
|<span data-ttu-id="ccd90-162">povolené hlavičky</span><span class="sxs-lookup"><span data-stu-id="ccd90-162">allowed-headers</span></span>|<span data-ttu-id="ccd90-163">Tento prvek obsahuje `header` elementy zadání názvy seznam hlaviček, které můžou být součástí požadavku.</span><span class="sxs-lookup"><span data-stu-id="ccd90-163">This element contains `header` elements specifying names of the headers that can be included in the request.</span></span>|<span data-ttu-id="ccd90-164">Ne</span><span class="sxs-lookup"><span data-stu-id="ccd90-164">No</span></span>|<span data-ttu-id="ccd90-165">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ccd90-165">N/A</span></span>|  
|<span data-ttu-id="ccd90-166">Zveřejněte hlavičky</span><span class="sxs-lookup"><span data-stu-id="ccd90-166">expose-headers</span></span>|<span data-ttu-id="ccd90-167">Tento prvek obsahuje `header` elementy zadání názvy seznam hlaviček, které budou přístupné klientem.</span><span class="sxs-lookup"><span data-stu-id="ccd90-167">This element contains `header` elements specifying names of the headers that will be accessible by the client.</span></span>|<span data-ttu-id="ccd90-168">Ne</span><span class="sxs-lookup"><span data-stu-id="ccd90-168">No</span></span>|<span data-ttu-id="ccd90-169">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ccd90-169">N/A</span></span>|  
|<span data-ttu-id="ccd90-170">záhlaví</span><span class="sxs-lookup"><span data-stu-id="ccd90-170">header</span></span>|<span data-ttu-id="ccd90-171">Určuje název hlavičky.</span><span class="sxs-lookup"><span data-stu-id="ccd90-171">Specifies a header name.</span></span>|<span data-ttu-id="ccd90-172">Alespoň jeden `header` element je vyžadována ve `allowed-headers` nebo `expose-headers` Pokud se nachází v části.</span><span class="sxs-lookup"><span data-stu-id="ccd90-172">At least one `header` element is required in `allowed-headers` or `expose-headers` if the section is present.</span></span>|<span data-ttu-id="ccd90-173">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ccd90-173">N/A</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="ccd90-174">Atributy</span><span class="sxs-lookup"><span data-stu-id="ccd90-174">Attributes</span></span>  
  
|<span data-ttu-id="ccd90-175">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ccd90-175">Name</span></span>|<span data-ttu-id="ccd90-176">Popis</span><span class="sxs-lookup"><span data-stu-id="ccd90-176">Description</span></span>|<span data-ttu-id="ccd90-177">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ccd90-177">Required</span></span>|<span data-ttu-id="ccd90-178">Výchozí</span><span class="sxs-lookup"><span data-stu-id="ccd90-178">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="ccd90-179">Povolit přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="ccd90-179">allow-credentials</span></span>|<span data-ttu-id="ccd90-180">`Access-Control-Allow-Credentials` Hlavičky v předběžné odpovědi bude nastavena na hodnotu tohoto atributu a vliv na schopnost klienta odeslat přihlašovací údaje v žádosti napříč doménami.</span><span class="sxs-lookup"><span data-stu-id="ccd90-180">The `Access-Control-Allow-Credentials` header in the preflight response will be set to the value of this attribute and affect the client’s ability to submit credentials in cross-domain requests.</span></span>|<span data-ttu-id="ccd90-181">Ne</span><span class="sxs-lookup"><span data-stu-id="ccd90-181">No</span></span>|<span data-ttu-id="ccd90-182">False</span><span class="sxs-lookup"><span data-stu-id="ccd90-182">false</span></span>|  
|<span data-ttu-id="ccd90-183">Kontrola před výstupem výsledek max-age</span><span class="sxs-lookup"><span data-stu-id="ccd90-183">preflight-result-max-age</span></span>|<span data-ttu-id="ccd90-184">`Access-Control-Max-Age` Hlavičky v předběžné odpovědi bude nastavena na hodnotu tohoto atributu a vliv na schopnost uživatelský agent mezipaměti před letu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ccd90-184">The `Access-Control-Max-Age` header in the preflight response will be set to the value of this attribute and affect the user agent’s ability to cache pre-flight response.</span></span>|<span data-ttu-id="ccd90-185">Ne</span><span class="sxs-lookup"><span data-stu-id="ccd90-185">No</span></span>|<span data-ttu-id="ccd90-186">0</span><span class="sxs-lookup"><span data-stu-id="ccd90-186">0</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="ccd90-187">Využití</span><span class="sxs-lookup"><span data-stu-id="ccd90-187">Usage</span></span>  
 <span data-ttu-id="ccd90-188">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="ccd90-188">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="ccd90-189">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="ccd90-189">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="ccd90-190">**Zásady obory:** rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="ccd90-190">**Policy scopes:** API, operation</span></span>  
  
##  <span data-ttu-id="ccd90-191"><a name="JSONP"></a>JSONP</span><span class="sxs-lookup"><span data-stu-id="ccd90-191"><a name="JSONP"></a> JSONP</span></span>  
 <span data-ttu-id="ccd90-192">`jsonp` Zásad přidá JSON s podporou odsazení (JSONP) do operace nebo rozhraní API, chcete-li povolit volání mezi doménami z klientů JavaScript založené na prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ccd90-192">The `jsonp` policy adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span> <span data-ttu-id="ccd90-193">JSONP je metoda použitá v programech, JavaScript při žádosti o data ze serveru v jiné doméně.</span><span class="sxs-lookup"><span data-stu-id="ccd90-193">JSONP is a method used in JavaScript programs to request data from a server in a different domain.</span></span> <span data-ttu-id="ccd90-194">JSONP obchází omezení vynucená většina webových prohlížečů, kde přístup k webovým stránkám musí být ve stejné doméně.</span><span class="sxs-lookup"><span data-stu-id="ccd90-194">JSONP bypasses the limitation enforced by most web browsers where access to web pages must be in the same domain.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="ccd90-195">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="ccd90-195">Policy statement</span></span>  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a><span data-ttu-id="ccd90-196">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccd90-196">Example</span></span>  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 <span data-ttu-id="ccd90-197">Pokud jste volali metodu bez parametru zpětného volání? cb = XXX vrátí prostý JSON (bez obálku volání funkce).</span><span class="sxs-lookup"><span data-stu-id="ccd90-197">If you call the method without the callback parameter ?cb=XXX it will return plain JSON (without a function call wrapper).</span></span>  
  
 <span data-ttu-id="ccd90-198">Pokud přidáte parametr zpětného volání `?cb=XXX` vrátí výsledek JSONP, zabalení původní JSON jako výsledky kolem funkce zpětného volání`XYZ('<json result goes here>');`</span><span class="sxs-lookup"><span data-stu-id="ccd90-198">If you add the callback parameter `?cb=XXX` it will return a JSONP result, wrapping the original JSON results around the callback function like `XYZ('<json result goes here>');`</span></span>  
  
### <a name="elements"></a><span data-ttu-id="ccd90-199">Elementy</span><span class="sxs-lookup"><span data-stu-id="ccd90-199">Elements</span></span>  
  
|<span data-ttu-id="ccd90-200">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ccd90-200">Name</span></span>|<span data-ttu-id="ccd90-201">Popis</span><span class="sxs-lookup"><span data-stu-id="ccd90-201">Description</span></span>|<span data-ttu-id="ccd90-202">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ccd90-202">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="ccd90-203">JSONP</span><span class="sxs-lookup"><span data-stu-id="ccd90-203">jsonp</span></span>|<span data-ttu-id="ccd90-204">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="ccd90-204">Root element.</span></span>|<span data-ttu-id="ccd90-205">Ano</span><span class="sxs-lookup"><span data-stu-id="ccd90-205">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="ccd90-206">Atributy</span><span class="sxs-lookup"><span data-stu-id="ccd90-206">Attributes</span></span>  
  
|<span data-ttu-id="ccd90-207">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ccd90-207">Name</span></span>|<span data-ttu-id="ccd90-208">Popis</span><span class="sxs-lookup"><span data-stu-id="ccd90-208">Description</span></span>|<span data-ttu-id="ccd90-209">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ccd90-209">Required</span></span>|<span data-ttu-id="ccd90-210">Výchozí</span><span class="sxs-lookup"><span data-stu-id="ccd90-210">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="ccd90-211">Název parametru zpětného volání</span><span class="sxs-lookup"><span data-stu-id="ccd90-211">callback-parameter-name</span></span>|<span data-ttu-id="ccd90-212">Volání pro funkce JavaScript se mezi doménami, předponu název plně kvalifikované domény, kde se funkce nachází.</span><span class="sxs-lookup"><span data-stu-id="ccd90-212">The cross-domain JavaScript function call prefixed with the fully qualified domain name where the function resides.</span></span>|<span data-ttu-id="ccd90-213">Ano</span><span class="sxs-lookup"><span data-stu-id="ccd90-213">Yes</span></span>|<span data-ttu-id="ccd90-214">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="ccd90-214">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="ccd90-215">Využití</span><span class="sxs-lookup"><span data-stu-id="ccd90-215">Usage</span></span>  
 <span data-ttu-id="ccd90-216">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="ccd90-216">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="ccd90-217">**Části zásady:** odchozí</span><span class="sxs-lookup"><span data-stu-id="ccd90-217">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="ccd90-218">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="ccd90-218">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="ccd90-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ccd90-219">Next steps</span></span>
<span data-ttu-id="ccd90-220">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="ccd90-220">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  