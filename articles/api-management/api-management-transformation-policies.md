---
title: "Zásad transformace Azure API Management | Microsoft Docs"
description: "Další informace o zásad transformace, která je k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7406a8ce-5f9c-4fae-9b0f-e574befb2ee9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: c2bed904b82c569b28a6e00d0cc9b49107c148dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-transformation-policies"></a><span data-ttu-id="724a3-103">Transformace zásady služby API Management</span><span class="sxs-lookup"><span data-stu-id="724a3-103">API Management transformation policies</span></span>
<span data-ttu-id="724a3-104">Toto téma obsahuje odkaz pro následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="724a3-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="724a3-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="724a3-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="724a3-106"><a name="TransformationPolicies"></a>Zásad transformace</span><span class="sxs-lookup"><span data-stu-id="724a3-106"><a name="TransformationPolicies"></a> Transformation policies</span></span>  
  
-   <span data-ttu-id="724a3-107">[Převést JSON do XML](api-management-transformation-policies.md#ConvertJSONtoXML) – převede požadavku nebo odpovědi textu z JSON do XML.</span><span class="sxs-lookup"><span data-stu-id="724a3-107">[Convert JSON to XML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON to XML.</span></span>  
  
-   <span data-ttu-id="724a3-108">[Převést na JSON XML](api-management-transformation-policies.md#ConvertXMLtoJSON) – převede požadavku nebo odpovědi textu z XML do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="724a3-108">[Convert XML to JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML to JSON.</span></span>  
  
-   <span data-ttu-id="724a3-109">[Najít a nahradit řetězec v textu](api-management-transformation-policies.md#Findandreplacestringinbody) – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="724a3-109">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
-   <span data-ttu-id="724a3-110">[Maskování adresy URL v obsahu](api-management-transformation-policies.md#MaskURLSContent) -znovu zapíše (masky) odkazy v odpovědi body tak, aby ukazovaly na ekvivalentní propojení prostřednictvím brány.</span><span class="sxs-lookup"><span data-stu-id="724a3-110">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span>  
  
-   <span data-ttu-id="724a3-111">[Nastavení back-end služby](api-management-transformation-policies.md#SetBackendService) -změny službě back-end pro příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="724a3-111">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes the backend service for an incoming request.</span></span>  
  
-   <span data-ttu-id="724a3-112">[Tělo nastavit](api-management-transformation-policies.md#SetBody) -nastaví obsah zprávy pro příchozí a odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="724a3-112">[Set body](api-management-transformation-policies.md#SetBody) - Sets the message body for incoming and outgoing requests.</span></span>  
  
-   <span data-ttu-id="724a3-113">[Set – hlavička protokolu HTTP](api-management-transformation-policies.md#SetHTTPheader) – přiřazuje hodnotu existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.</span><span class="sxs-lookup"><span data-stu-id="724a3-113">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>  
  
-   <span data-ttu-id="724a3-114">[Nastavte parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.</span><span class="sxs-lookup"><span data-stu-id="724a3-114">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
-   <span data-ttu-id="724a3-115">[Přepsání adresy URL](api-management-transformation-policies.md#RewriteURL) – převede adresu URL požadavku z jeho veřejné formuláře do formuláře očekává webovou službou.</span><span class="sxs-lookup"><span data-stu-id="724a3-115">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form to the form expected by the web service.</span></span>  
  
-   <span data-ttu-id="724a3-116">[Transformace XML pomocí transformaci XSLT](api-management-transformation-policies.md#XSLTransform) -použije transformaci XSL na XML v textu požadavku nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="724a3-116">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation to XML in the request or response body.</span></span>  
  
##  <span data-ttu-id="724a3-117"><a name="ConvertJSONtoXML"></a>Převést JSON do XML</span><span class="sxs-lookup"><span data-stu-id="724a3-117"><a name="ConvertJSONtoXML"></a> Convert JSON to XML</span></span>  
 <span data-ttu-id="724a3-118">`json-to-xml` Zásad převede text požadavku nebo odpovědi JSON do XML.</span><span class="sxs-lookup"><span data-stu-id="724a3-118">The `json-to-xml` policy converts a request or response body from JSON to XML.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-119">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-119">Policy statement</span></span>  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="724a3-120">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-120">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <json-to-xml apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="724a3-121">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-121">Elements</span></span>  
  
|<span data-ttu-id="724a3-122">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-122">Name</span></span>|<span data-ttu-id="724a3-123">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-123">Description</span></span>|<span data-ttu-id="724a3-124">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-124">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-125">JSON do xml</span><span class="sxs-lookup"><span data-stu-id="724a3-125">json-to-xml</span></span>|<span data-ttu-id="724a3-126">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-126">Root element.</span></span>|<span data-ttu-id="724a3-127">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-127">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="724a3-128">Atributy</span><span class="sxs-lookup"><span data-stu-id="724a3-128">Attributes</span></span>  
  
|<span data-ttu-id="724a3-129">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-129">Name</span></span>|<span data-ttu-id="724a3-130">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-130">Description</span></span>|<span data-ttu-id="724a3-131">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-131">Required</span></span>|<span data-ttu-id="724a3-132">Výchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-132">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="724a3-133">Použít</span><span class="sxs-lookup"><span data-stu-id="724a3-133">apply</span></span>|<span data-ttu-id="724a3-134">Musí být nastaven na jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="724a3-134">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="724a3-135">-vždy - vždy použít převod.</span><span class="sxs-lookup"><span data-stu-id="724a3-135">-   always - always apply conversion.</span></span><br /><span data-ttu-id="724a3-136">obsah json typ – převod pouze v případě, že přítomnost JSON určuje hlavičku odpovědi Content-Type.</span><span class="sxs-lookup"><span data-stu-id="724a3-136">-   content-type-json - convert only if response Content-Type header indicates presence of JSON.</span></span>|<span data-ttu-id="724a3-137">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-137">Yes</span></span>|<span data-ttu-id="724a3-138">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-138">N/A</span></span>|  
|<span data-ttu-id="724a3-139">Vezměte v úvahu přijmout – hlavička</span><span class="sxs-lookup"><span data-stu-id="724a3-139">consider-accept-header</span></span>|<span data-ttu-id="724a3-140">Musí být nastaven na jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="724a3-140">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="724a3-141">Pokud se vyžaduje JSON v požadavku hlavička Accept - true – použít převod.</span><span class="sxs-lookup"><span data-stu-id="724a3-141">-   true - apply conversion if JSON is requested in request Accept header.</span></span><br /><span data-ttu-id="724a3-142">-false – vždy použít převod.</span><span class="sxs-lookup"><span data-stu-id="724a3-142">-   false -always apply conversion.</span></span>|<span data-ttu-id="724a3-143">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-143">No</span></span>|<span data-ttu-id="724a3-144">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="724a3-144">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="724a3-145">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-145">Usage</span></span>  
 <span data-ttu-id="724a3-146">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-146">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-147">**Části zásady:** příchozích, odchozích, při chybě</span><span class="sxs-lookup"><span data-stu-id="724a3-147">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="724a3-148">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-148">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-149"><a name="ConvertXMLtoJSON"></a>Převod XML do formátu JSON</span><span class="sxs-lookup"><span data-stu-id="724a3-149"><a name="ConvertXMLtoJSON"></a> Convert XML to JSON</span></span>  
 <span data-ttu-id="724a3-150">`xml-to-json` Zásad převede text požadavku nebo odpovědi z XML do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="724a3-150">The `xml-to-json` policy converts a request or response body from XML to JSON.</span></span> <span data-ttu-id="724a3-151">Tato zásada umožňuje modernizovat rozhraní API podle jen XML back-end webové služby.</span><span class="sxs-lookup"><span data-stu-id="724a3-151">This policy can be used to modernize APIs based on XML-only backend web services.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-152">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-152">Policy statement</span></span>  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="724a3-153">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-153">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <xml-to-json kind="direct" apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="724a3-154">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-154">Elements</span></span>  
  
|<span data-ttu-id="724a3-155">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-155">Name</span></span>|<span data-ttu-id="724a3-156">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-156">Description</span></span>|<span data-ttu-id="724a3-157">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-157">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-158">XML – json</span><span class="sxs-lookup"><span data-stu-id="724a3-158">xml-to-json</span></span>|<span data-ttu-id="724a3-159">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-159">Root element.</span></span>|<span data-ttu-id="724a3-160">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-160">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="724a3-161">Atributy</span><span class="sxs-lookup"><span data-stu-id="724a3-161">Attributes</span></span>  
  
|<span data-ttu-id="724a3-162">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-162">Name</span></span>|<span data-ttu-id="724a3-163">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-163">Description</span></span>|<span data-ttu-id="724a3-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-164">Required</span></span>|<span data-ttu-id="724a3-165">Výchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-165">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="724a3-166">Typ</span><span class="sxs-lookup"><span data-stu-id="724a3-166">kind</span></span>|<span data-ttu-id="724a3-167">Musí být nastaven na jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="724a3-167">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="724a3-168">-javascript-friendly - převedený JSON má popisný JavaScript vývojářům formuláře.</span><span class="sxs-lookup"><span data-stu-id="724a3-168">-   javascript-friendly - the converted JSON has a form friendly to JavaScript developers.</span></span><br /><span data-ttu-id="724a3-169">převedený JSON - direct - odráží struktura původního dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="724a3-169">-   direct - the converted JSON reflects the original XML document's structure.</span></span>|<span data-ttu-id="724a3-170">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-170">Yes</span></span>|<span data-ttu-id="724a3-171">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-171">N/A</span></span>|  
|<span data-ttu-id="724a3-172">Použít</span><span class="sxs-lookup"><span data-stu-id="724a3-172">apply</span></span>|<span data-ttu-id="724a3-173">Musí být nastaven na jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="724a3-173">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="724a3-174">Převeďte – vždy - vždy.</span><span class="sxs-lookup"><span data-stu-id="724a3-174">-   always - convert always.</span></span><br /><span data-ttu-id="724a3-175">obsah typu xml - převést pouze v případě, že hlavičku odpovědi Content-Type označuje přítomnost XML.</span><span class="sxs-lookup"><span data-stu-id="724a3-175">-   content-type-xml - convert only if response Content-Type header indicates presence of XML.</span></span>|<span data-ttu-id="724a3-176">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-176">Yes</span></span>|<span data-ttu-id="724a3-177">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-177">N/A</span></span>|  
|<span data-ttu-id="724a3-178">Vezměte v úvahu přijmout – hlavička</span><span class="sxs-lookup"><span data-stu-id="724a3-178">consider-accept-header</span></span>|<span data-ttu-id="724a3-179">Musí být nastaven na jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="724a3-179">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="724a3-180">Pokud se vyžaduje XML v žádosti o hlavička Accept - true – použít převod.</span><span class="sxs-lookup"><span data-stu-id="724a3-180">-   true - apply conversion if XML is requested in request Accept header.</span></span><br /><span data-ttu-id="724a3-181">-false – vždy použít převod.</span><span class="sxs-lookup"><span data-stu-id="724a3-181">-   false -always apply conversion.</span></span>|<span data-ttu-id="724a3-182">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-182">No</span></span>|<span data-ttu-id="724a3-183">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="724a3-183">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="724a3-184">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-184">Usage</span></span>  
 <span data-ttu-id="724a3-185">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-185">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-186">**Části zásady:** příchozích, odchozích, při chybě</span><span class="sxs-lookup"><span data-stu-id="724a3-186">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="724a3-187">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-187">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-188"><a name="Findandreplacestringinbody"></a>Najít a nahradit řetězec v textu</span><span class="sxs-lookup"><span data-stu-id="724a3-188"><a name="Findandreplacestringinbody"></a> Find and replace string in body</span></span>  
 <span data-ttu-id="724a3-189">`find-and-replace` Zásady najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="724a3-189">The `find-and-replace` policy finds a request or response substring and replaces it with a different substring.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-190">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-190">Policy statement</span></span>  
  
```xml  
<find-and-replace from="what to replace" to="replacement" />  
```  
  
### <a name="example"></a><span data-ttu-id="724a3-191">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-191">Example</span></span>  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a><span data-ttu-id="724a3-192">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-192">Elements</span></span>  
  
|<span data-ttu-id="724a3-193">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-193">Name</span></span>|<span data-ttu-id="724a3-194">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-194">Description</span></span>|<span data-ttu-id="724a3-195">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-195">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-196">hledání a nahrazování</span><span class="sxs-lookup"><span data-stu-id="724a3-196">find-and-replace</span></span>|<span data-ttu-id="724a3-197">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-197">Root element.</span></span>|<span data-ttu-id="724a3-198">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-198">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="724a3-199">Atributy</span><span class="sxs-lookup"><span data-stu-id="724a3-199">Attributes</span></span>  
  
|<span data-ttu-id="724a3-200">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-200">Name</span></span>|<span data-ttu-id="724a3-201">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-201">Description</span></span>|<span data-ttu-id="724a3-202">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-202">Required</span></span>|<span data-ttu-id="724a3-203">Výchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-203">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="724a3-204">Z</span><span class="sxs-lookup"><span data-stu-id="724a3-204">from</span></span>|<span data-ttu-id="724a3-205">Hledaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="724a3-205">The string to search for.</span></span>|<span data-ttu-id="724a3-206">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-206">Yes</span></span>|<span data-ttu-id="724a3-207">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-207">N/A</span></span>|  
|<span data-ttu-id="724a3-208">na</span><span class="sxs-lookup"><span data-stu-id="724a3-208">to</span></span>|<span data-ttu-id="724a3-209">Náhradní řetězec.</span><span class="sxs-lookup"><span data-stu-id="724a3-209">The replacement string.</span></span> <span data-ttu-id="724a3-210">Zadejte řetězec nulové délky nahrazení odebrat hledaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="724a3-210">Specify a zero length replacement string to remove the search string.</span></span>|<span data-ttu-id="724a3-211">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-211">Yes</span></span>|<span data-ttu-id="724a3-212">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-212">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="724a3-213">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-213">Usage</span></span>  
 <span data-ttu-id="724a3-214">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-214">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-215">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="724a3-215">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="724a3-216">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-216">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-217"><a name="MaskURLSContent"></a>Maska adresy URL v obsahu</span><span class="sxs-lookup"><span data-stu-id="724a3-217"><a name="MaskURLSContent"></a> Mask URLs in content</span></span>  
 <span data-ttu-id="724a3-218">`redirect-content-urls` Zásad znovu zapíše odkazy (masky) v textu odpovědi tak, aby ukazovaly na ekvivalentní propojení prostřednictvím brány.</span><span class="sxs-lookup"><span data-stu-id="724a3-218">The `redirect-content-urls` policy re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span> <span data-ttu-id="724a3-219">Použijte v části odchozí znovu zapsat odkazy text odpovědi, aby byly bodu k bráně.</span><span class="sxs-lookup"><span data-stu-id="724a3-219">Use in the outbound section to re-write response body links to make them point to the gateway.</span></span> <span data-ttu-id="724a3-220">Použijte v části příchozí pro opačný efekt.</span><span class="sxs-lookup"><span data-stu-id="724a3-220">Use in the inbound section for an opposite effect.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="724a3-221">Tato zásada nezmění žádné hodnoty hlavičky, jako `Location` hlavičky.</span><span class="sxs-lookup"><span data-stu-id="724a3-221">This policy does not change any header values such as `Location` headers.</span></span> <span data-ttu-id="724a3-222">Chcete-li změnit hodnoty hlavičky, použijte [set-– hlavička](api-management-transformation-policies.md#SetHTTPheader) zásad.</span><span class="sxs-lookup"><span data-stu-id="724a3-222">To change header values, use the [set-header](api-management-transformation-policies.md#SetHTTPheader) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-223">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-223">Policy statement</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a><span data-ttu-id="724a3-224">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-224">Example</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a><span data-ttu-id="724a3-225">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-225">Elements</span></span>  
  
|<span data-ttu-id="724a3-226">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-226">Name</span></span>|<span data-ttu-id="724a3-227">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-227">Description</span></span>|<span data-ttu-id="724a3-228">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-228">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-229">adresy URL přesměrování obsahu</span><span class="sxs-lookup"><span data-stu-id="724a3-229">redirect-content-urls</span></span>|<span data-ttu-id="724a3-230">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-230">Root element.</span></span>|<span data-ttu-id="724a3-231">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-231">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="724a3-232">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-232">Usage</span></span>  
 <span data-ttu-id="724a3-233">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-233">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-234">**Části zásady:** vstupní, výstupní</span><span class="sxs-lookup"><span data-stu-id="724a3-234">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="724a3-235">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-235">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-236"><a name="SetBackendService"></a>Nastavení back-end služby</span><span class="sxs-lookup"><span data-stu-id="724a3-236"><a name="SetBackendService"></a> Set backend service</span></span>  
 <span data-ttu-id="724a3-237">Použití `set-backend-service` zásady pro přesměrování příchozí žádosti na jiný back-end než verze zadaná v nastavení rozhraní API pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="724a3-237">Use the `set-backend-service` policy to redirect an incoming request to a different backend than the one specified in the API settings for that operation.</span></span> <span data-ttu-id="724a3-238">Tato zásada změny back-end základní adresa URL služby příchozích požadavků verze zadaná v zásadách.</span><span class="sxs-lookup"><span data-stu-id="724a3-238">This policy changes the backend service base URL of the incoming request to the one specified in the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-239">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-239">Policy statement</span></span>  
  
```xml  
<set-backend-service base-url="base URL of the backend service" />  
```  
  
### <a name="example"></a><span data-ttu-id="724a3-240">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-240">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <choose>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2013-05")">  
                <set-backend-service base-url="http://contoso.com/api/8.2/" />  
            </when>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2014-03")">  
                <set-backend-service base-url="http://contoso.com/api/9.1/" />  
            </when>  
        </choose>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
<span data-ttu-id="724a3-241">V tomto příkladu nastavení zásad služby back-end směruje požadavky na základě hodnoty verze předaný služby back-end jiný než ten, který je zadaný v rozhraní API v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="724a3-241">In this example the set backend service policy routes requests based on the version value passed in the query string to a different backend service than the one specified in the API.</span></span>
  
<span data-ttu-id="724a3-242">Původně adresu URL základní služby back-end je odvozená od nastavení rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="724a3-242">Initially the backend service base URL is derived from the API settings.</span></span> <span data-ttu-id="724a3-243">Proto adrese URL žádosti `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` stane `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` kde `http://contoso.com/api/10.4/` je adresa URL back-end služby zadaný v nastavení rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="724a3-243">So the request URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` becomes `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` where `http://contoso.com/api/10.4/` is the backend service URL specified in the API settings.</span></span>  
  
<span data-ttu-id="724a3-244">Když [< zvolte\> ](api-management-advanced-policies.md#choose) platí prohlášení o zásadách adresu URL základní služby back-end může změnit znovu buď `http://contoso.com/api/8.2` nebo `http://contoso.com/api/9.1`, v závislosti na základě hodnoty parametru dotazu požadavku verze.</span><span class="sxs-lookup"><span data-stu-id="724a3-244">When the [<choose\>](api-management-advanced-policies.md#choose) policy statement is applied the backend service base URL may change again either to `http://contoso.com/api/8.2` or `http://contoso.com/api/9.1`, depending on the value of the version request query parameter.</span></span> <span data-ttu-id="724a3-245">Například, pokud je hodnota `"2013-15"` poslední žádosti se změní na adresu URL `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span><span class="sxs-lookup"><span data-stu-id="724a3-245">For example, if the value is `"2013-15"` the final request URL becomes `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span></span>  
  
<span data-ttu-id="724a3-246">Pokud transformace požadavku je další požadované, ostatní [zásad transformace](api-management-transformation-policies.md#TransformationPolicies) lze použít.</span><span class="sxs-lookup"><span data-stu-id="724a3-246">If further transformation of the request is desired, other [Transformation policies](api-management-transformation-policies.md#TransformationPolicies) can be used.</span></span> <span data-ttu-id="724a3-247">Chcete-li například odeberte parametr dotazu verze teď, když se směrováním požadavku na konkrétní back-end verze [nastavit parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) zásadu lze použít k odebrání atributu teď redundantní verze.</span><span class="sxs-lookup"><span data-stu-id="724a3-247">For example, to remove the version query parameter now that the request is being routed to a version specific backend, the  [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policy can be used to remove the now redundant version attribute.</span></span>  
  
### <a name="example"></a><span data-ttu-id="724a3-248">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-248">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <set-backend-service backend-id="my-sf-service" sf-partition-key="@(context.Request.Url.Query.GetValueOrDefault("userId","")" sf-replica-type="primary" /> 
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
<span data-ttu-id="724a3-249">V tomto příkladu zásady přesměruje požadavek na služby prostředků infrastruktury back-end, používání userId řetězec dotazu jako klíč oddílu a primární repliky oddílu.</span><span class="sxs-lookup"><span data-stu-id="724a3-249">In this example the policy routes the request to a service fabric backend, using the userId query string as the partition key and using the primary replica of the partition.</span></span>  

### <a name="elements"></a><span data-ttu-id="724a3-250">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-250">Elements</span></span>  
  
|<span data-ttu-id="724a3-251">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-251">Name</span></span>|<span data-ttu-id="724a3-252">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-252">Description</span></span>|<span data-ttu-id="724a3-253">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-253">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-254">set-back-end service</span><span class="sxs-lookup"><span data-stu-id="724a3-254">set-backend-service</span></span>|<span data-ttu-id="724a3-255">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-255">Root element.</span></span>|<span data-ttu-id="724a3-256">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-256">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="724a3-257">Atributy</span><span class="sxs-lookup"><span data-stu-id="724a3-257">Attributes</span></span>  
  
|<span data-ttu-id="724a3-258">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-258">Name</span></span>|<span data-ttu-id="724a3-259">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-259">Description</span></span>|<span data-ttu-id="724a3-260">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-260">Required</span></span>|<span data-ttu-id="724a3-261">Výchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-261">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="724a3-262">Základní adresa url</span><span class="sxs-lookup"><span data-stu-id="724a3-262">base-url</span></span>|<span data-ttu-id="724a3-263">Nový back-end základní adresa URL služby.</span><span class="sxs-lookup"><span data-stu-id="724a3-263">New backend service base URL.</span></span>|<span data-ttu-id="724a3-264">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-264">No</span></span>|<span data-ttu-id="724a3-265">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-265">N/A</span></span>|  
|<span data-ttu-id="724a3-266">id back-end</span><span class="sxs-lookup"><span data-stu-id="724a3-266">backend-id</span></span>|<span data-ttu-id="724a3-267">Identifikátor back-end pro směrování.</span><span class="sxs-lookup"><span data-stu-id="724a3-267">Identifier of the backend to route to.</span></span>|<span data-ttu-id="724a3-268">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-268">No</span></span>|<span data-ttu-id="724a3-269">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-269">N/A</span></span>|  
|<span data-ttu-id="724a3-270">klíč oddílu SF</span><span class="sxs-lookup"><span data-stu-id="724a3-270">sf-partition-key</span></span>|<span data-ttu-id="724a3-271">Platí jenom při back-end je služba Service Fabric a je určen pomocí back-end id.</span><span class="sxs-lookup"><span data-stu-id="724a3-271">Only applicable when the backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="724a3-272">Používá k překladu na konkrétní oddíl z překládání adres.</span><span class="sxs-lookup"><span data-stu-id="724a3-272">Used to resolve a specific partition from the name resolution service.</span></span>|<span data-ttu-id="724a3-273">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-273">No</span></span>|<span data-ttu-id="724a3-274">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-274">N/A</span></span>|  
|<span data-ttu-id="724a3-275">Typ SF repliky</span><span class="sxs-lookup"><span data-stu-id="724a3-275">sf-replica-type</span></span>|<span data-ttu-id="724a3-276">Platí jenom při back-end je služba Service Fabric a je určen pomocí back-end id.</span><span class="sxs-lookup"><span data-stu-id="724a3-276">Only applicable when the backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="724a3-277">Ovládací prvky, pokud požadavek by měli přejít na primární nebo sekundární replice oddílu.</span><span class="sxs-lookup"><span data-stu-id="724a3-277">Controls if the request should go to the primary or secondary replica of a partition.</span></span> |<span data-ttu-id="724a3-278">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-278">No</span></span>|<span data-ttu-id="724a3-279">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-279">N/A</span></span>|    
|<span data-ttu-id="724a3-280">SF. vyřešte podmínek</span><span class="sxs-lookup"><span data-stu-id="724a3-280">sf-resolve-condition</span></span>|<span data-ttu-id="724a3-281">Platí jenom při back-end je služba Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="724a3-281">Only applicable when the backend is a Service Fabric service.</span></span> <span data-ttu-id="724a3-282">Podmínka vyhodnocena jako identifikace, pokud volání back-end Service Fabric se musí opakovat s nové řešení.</span><span class="sxs-lookup"><span data-stu-id="724a3-282">Condition identifying if the call to Service Fabric backend has to be repeated with new resolution.</span></span>|<span data-ttu-id="724a3-283">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-283">No</span></span>|<span data-ttu-id="724a3-284">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-284">N/A</span></span>|    
|<span data-ttu-id="724a3-285">SF-service-instance-name</span><span class="sxs-lookup"><span data-stu-id="724a3-285">sf-service-instance-name</span></span>|<span data-ttu-id="724a3-286">Platí jenom při back-end je služba Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="724a3-286">Only applicable when the backend is a Service Fabric service.</span></span> <span data-ttu-id="724a3-287">Umožňuje změnit instance služby za běhu.</span><span class="sxs-lookup"><span data-stu-id="724a3-287">Allows to change service instances at runtime.</span></span> |<span data-ttu-id="724a3-288">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-288">No</span></span>|<span data-ttu-id="724a3-289">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-289">N/A</span></span>|    

### <a name="usage"></a><span data-ttu-id="724a3-290">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-290">Usage</span></span>  
 <span data-ttu-id="724a3-291">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-291">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-292">**Části zásady:** příchozí back-end</span><span class="sxs-lookup"><span data-stu-id="724a3-292">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="724a3-293">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-293">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-294"><a name="SetBody"></a>Sada textu</span><span class="sxs-lookup"><span data-stu-id="724a3-294"><a name="SetBody"></a> Set body</span></span>  
 <span data-ttu-id="724a3-295">Použití `set-body` zásad nastavit text zprávy pro příchozí a odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="724a3-295">Use the `set-body` policy to set the message body for incoming and outgoing requests.</span></span> <span data-ttu-id="724a3-296">Pro přístup k tělo zprávy, můžete použít `context.Request.Body` vlastnost nebo `context.Response.Body`, v závislosti na tom, jestli zásady je v části příchozí nebo odchozí.</span><span class="sxs-lookup"><span data-stu-id="724a3-296">To access the message body you can use the `context.Request.Body` property or the `context.Response.Body`, depending on whether the policy is in the inbound or outbound section.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="724a3-297">Všimněte si, že ve výchozím nastavení při přístupu k zprávu textu pomocí `context.Request.Body` nebo `context.Response.Body`, původní text zprávy dojde ke ztrátě a musí být nastavena vrácením text zpět ve výrazu.</span><span class="sxs-lookup"><span data-stu-id="724a3-297">Note that by default when you access the message body using `context.Request.Body` or `context.Response.Body`, the original message body is lost and must be set by returning the body back in the expression.</span></span> <span data-ttu-id="724a3-298">Chcete-li zachovat obsah textu, nastavte `preserveContent` parametru `true` při přístupu k zprávy.</span><span class="sxs-lookup"><span data-stu-id="724a3-298">To preserve the body content, set the `preserveContent` parameter to `true` when accessing the message.</span></span> <span data-ttu-id="724a3-299">Pokud `preserveContent` je nastaven na `true` a jiný text je vrácený výrazem vrácené body se používá.</span><span class="sxs-lookup"><span data-stu-id="724a3-299">If `preserveContent` is set to `true` and a different body is returned by the expression, the returned body is used.</span></span>  
>   
>  <span data-ttu-id="724a3-300">Upozorňujeme následující aspekty při používání `set-body` zásad.</span><span class="sxs-lookup"><span data-stu-id="724a3-300">Please note the following considerations when using the `set-body` policy.</span></span>  
>   
>  -   <span data-ttu-id="724a3-301">Pokud používáte `set-body` zásad vrátit nové nebo aktualizované text nemusíte nastavit `preserveContent` k `true` vzhledem k tomu, že jsou explicitně poskytuje nový obsah textu.</span><span class="sxs-lookup"><span data-stu-id="724a3-301">If you are using the `set-body` policy to return a new or updated body you don't need to set `preserveContent` to `true` because you are explicitly supplying the new body contents.</span></span>  
> -   <span data-ttu-id="724a3-302">Zachování obsahu odpovědi v příchozí kanálu nebude mít smysl, protože nepřijde žádná odpověď ještě.</span><span class="sxs-lookup"><span data-stu-id="724a3-302">Preserving the content of a response in the inbound pipeline doesn't make sense because there is no response yet.</span></span>  
> -   <span data-ttu-id="724a3-303">Zachování obsahu žádosti v odchozí kanálu nebude mít smysl, protože žádost již byl odeslán do back-end v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="724a3-303">Preserving the content of a request in the outbound pipeline doesn't make sense because the request has already been sent to the backend at this point.</span></span>  
> -   <span data-ttu-id="724a3-304">Pokud tato zásada je použita, pokud neexistuje žádný text zprávy, například v příchozí GET, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="724a3-304">If this policy is used when there is no message body, for example in an inbound GET, an exception is thrown.</span></span>  
  
 <span data-ttu-id="724a3-305">Další informace najdete v tématu `context.Request.Body`, `context.Response.Body`a `IMessage` v částech [kontextové proměnné](api-management-policy-expressions.md#ContextVariables) tabulky.</span><span class="sxs-lookup"><span data-stu-id="724a3-305">For more information, see the `context.Request.Body`, `context.Response.Body`, and the `IMessage` sections in the [Context variable](api-management-policy-expressions.md#ContextVariables) table.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-306">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-306">Policy statement</span></span>  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a><span data-ttu-id="724a3-307">Příklady</span><span class="sxs-lookup"><span data-stu-id="724a3-307">Examples</span></span>  
  
#### <a name="literal-text-example"></a><span data-ttu-id="724a3-308">Příklad textového literálu</span><span class="sxs-lookup"><span data-stu-id="724a3-308">Literal text example</span></span>  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a><span data-ttu-id="724a3-309">Příklad přístup k text jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="724a3-309">Example accessing the body as a string.</span></span> <span data-ttu-id="724a3-310">Všimněte si, že jsme jsou, se kterým jsme můžete později v kanálu zachování původního textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="724a3-310">Note that we are preserving the original request body so that we can access it later in the pipeline.</span></span>
  
```xml  
<set-body>  
@{   
    string inBody = context.Request.Body.As<string>(preserveContent: true);   
    if (inBody[0] =='c') {   
        inBody[0] = 'm';   
    }   
    return inBody;   
}  
</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accesing-it-later-in-the-pipeline-will-result-in-an-exception"></a><span data-ttu-id="724a3-311">Příklad přístup k text jako JObject.</span><span class="sxs-lookup"><span data-stu-id="724a3-311">Example accessing the body as a JObject.</span></span> <span data-ttu-id="724a3-312">Pamatujte, že vzhledem k tomu, že jsme nejsou rezervování původního textu žádosti accesing ho později v kanálu budou mít za následek výjimku.</span><span class="sxs-lookup"><span data-stu-id="724a3-312">Note that since we are not reserving the original request body, accesing it later in the pipeline will result in an exception.</span></span>  
  
```xml  
<set-body>   
@{   
    JObject inBody = context.Request.Body.As<JObject>();   
    if (inBody.attribute == <tag>) {   
        inBody[0] = 'm';   
    }   
    return inBody.ToString();   
}   
</set-body>  
  
```  
  
#### <a name="filter-response-based-on-product"></a><span data-ttu-id="724a3-313">Filtr reakci na produktu</span><span class="sxs-lookup"><span data-stu-id="724a3-313">Filter response based on product</span></span>  
 <span data-ttu-id="724a3-314">Tento příklad ukazuje, jak provést filtrování obsahu odebráním datové prvky. z odpovědi přijal od služby back-end při použití `Starter` produktu.</span><span class="sxs-lookup"><span data-stu-id="724a3-314">This example shows how to perform content filtering by removing data elements from the response received from the backend service when using the `Starter` product.</span></span> <span data-ttu-id="724a3-315">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 34:30.</span><span class="sxs-lookup"><span data-stu-id="724a3-315">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 34:30.</span></span> <span data-ttu-id="724a3-316">Spuštění na 31:50 zobrazíte přehled [rozhraní tmavý Sky prognózy API](https://developer.forecast.io/) použít v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="724a3-316">Start  at 31:50 to see an overview of [The Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
```xml  
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->  
<choose>  
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">  
    <set-body>@{  
        var response = context.Response.Body.As<JObject>();  
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {  
          response.Property (key).Remove ();  
        }  
        return response.ToString();  
      }  
    </set-body>  
  </when>  
</choose>  
```  

### <a name="using-liquid-templates-with-set-body"></a><span data-ttu-id="724a3-317">Pomocí sady textu kapaliny šablony</span><span class="sxs-lookup"><span data-stu-id="724a3-317">Using Liquid templates with set body</span></span> 
<span data-ttu-id="724a3-318">`set-body` Zásad může být nakonfigurován pro použití [kapaliny](https://shopify.github.io/liquid/basics/introduction/) ukázka jazyka transfom textu požadavku nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="724a3-318">The `set-body` policy can be configured to use the [Liquid](https://shopify.github.io/liquid/basics/introduction/) templating language to transfom the body of a request or response.</span></span> <span data-ttu-id="724a3-319">To může být velmi efektivní, pokud je nutné zcela změna tvaru formát zprávy.</span><span class="sxs-lookup"><span data-stu-id="724a3-319">This can be very effective if you need to completely reshape the format of your message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="724a3-320">Implementace kapaliny používány `set-body` v "režimu C#, jsou nakonfigurované zásady.</span><span class="sxs-lookup"><span data-stu-id="724a3-320">The implementation of Liquid used in the `set-body` policy is configured in 'C# mode'.</span></span> <span data-ttu-id="724a3-321">To je zvlášť důležité při provádění akcí, například filtrování.</span><span class="sxs-lookup"><span data-stu-id="724a3-321">This is particularly important when doing things such as filtering.</span></span> <span data-ttu-id="724a3-322">Jako příklad použití Filtr kalendářních dat vyžaduje použití Pascal velká a malá písmena a C# datum formátování např:</span><span class="sxs-lookup"><span data-stu-id="724a3-322">As an example, using a date filter requires the use of Pascal casing and C# date formatting e.g.:</span></span>
>
> <span data-ttu-id="724a3-323">{{body.foo.startDateTime| Datum: "yyyyMMddTHH:mm:ddZ"}}</span><span class="sxs-lookup"><span data-stu-id="724a3-323">{{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}</span></span>

> [!IMPORTANT]
> <span data-ttu-id="724a3-324">Aby bylo možné správně vytvořit vazbu textu XML pomocí kapaliny šablony, používat `set-header` zásad nastavit Content-Type buď application/xml, text/xml (nebo libovolný typ konče + xml); text JSON, musí být application/json, text/json (nebo libovolný typ konče + json).</span><span class="sxs-lookup"><span data-stu-id="724a3-324">In order to correctly bind to an XML body using the Liquid template, use a `set-header` policy to set Content-Type to either application/xml, text/xml (or any type ending with +xml); for a JSON body, it must be application/json, text/json (or any type ending with +json).</span></span>

#### <a name="convert-json-to-soap-using-a-liquid-template"></a><span data-ttu-id="724a3-325">Převést na protokolu SOAP pomocí kapaliny šablony JSON</span><span class="sxs-lookup"><span data-stu-id="724a3-325">Convert JSON to SOAP using a Liquid template</span></span>
```xml
<set-body template="liquid">
    <soap:Envelope xmlns="http://tempuri.org/" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <GetOpenOrders>
                <cust>{{body.getOpenOrders.cust}}</cust>
            </GetOpenOrders>
        </soap:Body>
    </soap:Envelope>
</set-body>
```

#### <a name="tranform-json-using-a-liquid-template"></a><span data-ttu-id="724a3-326">JSON Tranform pomocí kapaliny šablony</span><span class="sxs-lookup"><span data-stu-id="724a3-326">Tranform JSON using a Liquid template</span></span>
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a><span data-ttu-id="724a3-327">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-327">Elements</span></span>  
  
|<span data-ttu-id="724a3-328">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-328">Name</span></span>|<span data-ttu-id="724a3-329">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-329">Description</span></span>|<span data-ttu-id="724a3-330">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-330">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-331">Sada textu</span><span class="sxs-lookup"><span data-stu-id="724a3-331">set-body</span></span>|<span data-ttu-id="724a3-332">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-332">Root element.</span></span> <span data-ttu-id="724a3-333">Obsahuje základní text nebo výrazy, které vrací text.</span><span class="sxs-lookup"><span data-stu-id="724a3-333">Contains the body text or an expressions that returns a body.</span></span>|<span data-ttu-id="724a3-334">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-334">Yes</span></span>|  

### <a name="properties"></a><span data-ttu-id="724a3-335">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="724a3-335">Properties</span></span>  
  
|<span data-ttu-id="724a3-336">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-336">Name</span></span>|<span data-ttu-id="724a3-337">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-337">Description</span></span>|<span data-ttu-id="724a3-338">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-338">Required</span></span>|<span data-ttu-id="724a3-339">Výchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-339">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="724a3-340">šablona</span><span class="sxs-lookup"><span data-stu-id="724a3-340">template</span></span>|<span data-ttu-id="724a3-341">Použít ke změně který nastavit tělo zásady se spustí v režimu ukázka.</span><span class="sxs-lookup"><span data-stu-id="724a3-341">Used to change the templating mode that the set body policy will run in.</span></span> <span data-ttu-id="724a3-342">Aktuálně je jediná podporovaná hodnota:</span><span class="sxs-lookup"><span data-stu-id="724a3-342">Currently the only supported value is:</span></span><br /><br /><span data-ttu-id="724a3-343">-kapaliny - tělo zásady sada bude používat modul kapaliny ukázka</span><span class="sxs-lookup"><span data-stu-id="724a3-343">- liquid - the set body policy will use the liquid templating engine</span></span> |<span data-ttu-id="724a3-344">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-344">No</span></span>|<span data-ttu-id="724a3-345">kapaliny</span><span class="sxs-lookup"><span data-stu-id="724a3-345">liquid</span></span>|  

<span data-ttu-id="724a3-346">Pro přístup k informacím o žádosti a odpovědi, kapaliny šablony lze vázat na objekt kontextu s následujícími vlastnostmi:</span><span class="sxs-lookup"><span data-stu-id="724a3-346">For accessing information about the request and response, the Liquid template can bind to a context object with the following properties:</span></span> <br />
<pre>context.
    Request.
        Url
        Method
        OriginalMethod
        OriginalUrl
        IpAddress
        MatchedParameters
        HasBody
        ClientCertificates
        Headers

    Response.
        StatusCode
        Method
        Headers
Url.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString

OriginalUrl.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString
</pre>



### <a name="usage"></a><span data-ttu-id="724a3-347">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-347">Usage</span></span>  
 <span data-ttu-id="724a3-348">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-348">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-349">**Části zásady:** vstupní, výstupní a back-end</span><span class="sxs-lookup"><span data-stu-id="724a3-349">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="724a3-350">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-350">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-351"><a name="SetHTTPheader"></a>Set – hlavička protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="724a3-351"><a name="SetHTTPheader"></a> Set HTTP header</span></span>  
 <span data-ttu-id="724a3-352">`set-header` Zásady přiřazuje hodnotu existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.</span><span class="sxs-lookup"><span data-stu-id="724a3-352">The `set-header` policy assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>  
  
 <span data-ttu-id="724a3-353">Seznam hlaviček protokolu HTTP se vloží do zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="724a3-353">Inserts a list of HTTP headers into an HTTP message.</span></span> <span data-ttu-id="724a3-354">Při umístění v příchozí kanálu se tato zásada nastaví hlavičky protokolu HTTP pro žádost předávány cílové službě.</span><span class="sxs-lookup"><span data-stu-id="724a3-354">When placed in an inbound pipeline, this policy sets the HTTP headers for the request being passed to the target service.</span></span> <span data-ttu-id="724a3-355">Při umístění v odchozí kanálu se tato zásada nastaví hlavičky protokolu HTTP pro odpověď odesílanou brány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="724a3-355">When placed in an outbound pipeline, this policy sets the HTTP headers for the response being sent to the gateway’s client.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-356">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-356">Policy statement</span></span>  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with the same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a><span data-ttu-id="724a3-357">Příklady</span><span class="sxs-lookup"><span data-stu-id="724a3-357">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="724a3-358">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-358">Example</span></span>  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a><span data-ttu-id="724a3-359">Předávat informace o kontextu ke službě back-end</span><span class="sxs-lookup"><span data-stu-id="724a3-359">Forward context information to the backend service</span></span>  
 <span data-ttu-id="724a3-360">Tento příklad ukazuje, jak použít zásady na úrovni rozhraní API a zadejte informace o kontextu ke službě back-end.</span><span class="sxs-lookup"><span data-stu-id="724a3-360">This example shows how to apply policy at the API level to supply context information to the backend service.</span></span> <span data-ttu-id="724a3-361">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 10:30.</span><span class="sxs-lookup"><span data-stu-id="724a3-361">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 10:30.</span></span> <span data-ttu-id="724a3-362">Na 12:10 je ukázku volání operace v portálu pro vývojáře, kde se můžete podívat zásad v práci.</span><span class="sxs-lookup"><span data-stu-id="724a3-362">At 12:10 there is a demo of calling an operation in the developer portal where you can see the policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 <span data-ttu-id="724a3-363">Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="724a3-363">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="724a3-364">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-364">Elements</span></span>  
  
|<span data-ttu-id="724a3-365">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-365">Name</span></span>|<span data-ttu-id="724a3-366">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-366">Description</span></span>|<span data-ttu-id="724a3-367">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-367">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-368">set – hlavička</span><span class="sxs-lookup"><span data-stu-id="724a3-368">set-header</span></span>|<span data-ttu-id="724a3-369">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-369">Root element.</span></span>|<span data-ttu-id="724a3-370">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-370">Yes</span></span>|  
|<span data-ttu-id="724a3-371">hodnota</span><span class="sxs-lookup"><span data-stu-id="724a3-371">value</span></span>|<span data-ttu-id="724a3-372">Určuje hodnotu záhlaví nastavit.</span><span class="sxs-lookup"><span data-stu-id="724a3-372">Specifies the value of the header to be set.</span></span> <span data-ttu-id="724a3-373">Pro více záhlaví se stejným názvem přidat další `value` elementy.</span><span class="sxs-lookup"><span data-stu-id="724a3-373">For multiple headers with the same name add additional `value` elements.</span></span>|<span data-ttu-id="724a3-374">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-374">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="724a3-375">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="724a3-375">Properties</span></span>  
  
|<span data-ttu-id="724a3-376">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-376">Name</span></span>|<span data-ttu-id="724a3-377">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-377">Description</span></span>|<span data-ttu-id="724a3-378">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-378">Required</span></span>|<span data-ttu-id="724a3-379">Výchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-379">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="724a3-380">existuje akce</span><span class="sxs-lookup"><span data-stu-id="724a3-380">exists-action</span></span>|<span data-ttu-id="724a3-381">Určuje, jaká opatření se mají provést, pokud hlavička byl již zadán.</span><span class="sxs-lookup"><span data-stu-id="724a3-381">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="724a3-382">Tento atribut musí mít jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="724a3-382">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="724a3-383">-override - nahradí hodnotu existující záhlaví.</span><span class="sxs-lookup"><span data-stu-id="724a3-383">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="724a3-384">-skip - nenahrazuje existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="724a3-384">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="724a3-385">-připojit - připojí hodnotu pro existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="724a3-385">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="724a3-386">-delete - odstraní hlavičku ze žádosti.</span><span class="sxs-lookup"><span data-stu-id="724a3-386">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="724a3-387">Pokud nastavíte hodnotu `override` uvedení více položek se stejným názvem výsledků v hlavičce je nastavena podle všech položek (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku.</span><span class="sxs-lookup"><span data-stu-id="724a3-387">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="724a3-388">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-388">No</span></span>|<span data-ttu-id="724a3-389">přepsání</span><span class="sxs-lookup"><span data-stu-id="724a3-389">override</span></span>|  
|<span data-ttu-id="724a3-390">jméno</span><span class="sxs-lookup"><span data-stu-id="724a3-390">name</span></span>|<span data-ttu-id="724a3-391">Určuje název záhlaví nastavit.</span><span class="sxs-lookup"><span data-stu-id="724a3-391">Specifies name of the header to be set.</span></span>|<span data-ttu-id="724a3-392">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-392">Yes</span></span>|<span data-ttu-id="724a3-393">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-393">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="724a3-394">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-394">Usage</span></span>  
 <span data-ttu-id="724a3-395">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-395">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-396">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="724a3-396">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="724a3-397">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-397">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-398"><a name="SetQueryStringParameter"></a>Parametr řetězce dotazu sady</span><span class="sxs-lookup"><span data-stu-id="724a3-398"><a name="SetQueryStringParameter"></a> Set query string parameter</span></span>  
 <span data-ttu-id="724a3-399">`set-query-parameter` Přidá zásad, nahradí hodnotu, nebo odstranění požadavku parametr řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="724a3-399">The `set-query-parameter` policy adds, replaces value of, or deletes request query string parameter.</span></span> <span data-ttu-id="724a3-400">Slouží k předání očekávanou back-end službu, která jsou volitelné parametry dotazu nebo nikdy přítomné v žádosti.</span><span class="sxs-lookup"><span data-stu-id="724a3-400">Can be used to pass query parameters expected by the backend service which are optional or never present in the request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-401">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-401">Policy statement</span></span>  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with the same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a><span data-ttu-id="724a3-402">Příklady</span><span class="sxs-lookup"><span data-stu-id="724a3-402">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="724a3-403">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-403">Example</span></span>  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with the same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a><span data-ttu-id="724a3-404">Předávat informace o kontextu ke službě back-end</span><span class="sxs-lookup"><span data-stu-id="724a3-404">Forward context information to the backend service</span></span>  
 <span data-ttu-id="724a3-405">Tento příklad ukazuje, jak použít zásady na úrovni rozhraní API a zadejte informace o kontextu ke službě back-end.</span><span class="sxs-lookup"><span data-stu-id="724a3-405">This example shows how to apply policy at the API level to supply context information to the backend service.</span></span> <span data-ttu-id="724a3-406">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 10:30.</span><span class="sxs-lookup"><span data-stu-id="724a3-406">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 10:30.</span></span> <span data-ttu-id="724a3-407">Na 12:10 je ukázku volání operace v portálu pro vývojáře, kde se můžete podívat zásad v práci.</span><span class="sxs-lookup"><span data-stu-id="724a3-407">At 12:10 there is a demo of calling an operation in the developer portal where you can see the policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 <span data-ttu-id="724a3-408">Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="724a3-408">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="724a3-409">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-409">Elements</span></span>  
  
|<span data-ttu-id="724a3-410">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-410">Name</span></span>|<span data-ttu-id="724a3-411">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-411">Description</span></span>|<span data-ttu-id="724a3-412">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-412">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-413">parametr dotazu pro sadu</span><span class="sxs-lookup"><span data-stu-id="724a3-413">set-query-parameter</span></span>|<span data-ttu-id="724a3-414">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-414">Root element.</span></span>|<span data-ttu-id="724a3-415">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-415">Yes</span></span>|  
|<span data-ttu-id="724a3-416">hodnota</span><span class="sxs-lookup"><span data-stu-id="724a3-416">value</span></span>|<span data-ttu-id="724a3-417">Určuje hodnotu parametru dotazu nastavit.</span><span class="sxs-lookup"><span data-stu-id="724a3-417">Specifies the value of the query parameter to be set.</span></span> <span data-ttu-id="724a3-418">Pro více parametry dotazu se stejným názvem přidat další `value` elementy.</span><span class="sxs-lookup"><span data-stu-id="724a3-418">For multiple query parameters with the same name add additional `value` elements.</span></span>|<span data-ttu-id="724a3-419">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-419">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="724a3-420">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="724a3-420">Properties</span></span>  
  
|<span data-ttu-id="724a3-421">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-421">Name</span></span>|<span data-ttu-id="724a3-422">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-422">Description</span></span>|<span data-ttu-id="724a3-423">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-423">Required</span></span>|<span data-ttu-id="724a3-424">Výchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-424">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="724a3-425">existuje akce</span><span class="sxs-lookup"><span data-stu-id="724a3-425">exists-action</span></span>|<span data-ttu-id="724a3-426">Určuje, jaká opatření se mají provést, když už je zadaný parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="724a3-426">Specifies what action to take when the query parameter is already specified.</span></span> <span data-ttu-id="724a3-427">Tento atribut musí mít jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="724a3-427">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="724a3-428">-override - nahradí hodnotu parametru existující.</span><span class="sxs-lookup"><span data-stu-id="724a3-428">-   override - replaces the value of the existing parameter.</span></span><br /><span data-ttu-id="724a3-429">-skip - nenahrazuje existující hodnota parametru dotazu.</span><span class="sxs-lookup"><span data-stu-id="724a3-429">-   skip - does not replace the existing query parameter value.</span></span><br /><span data-ttu-id="724a3-430">-připojit - připojí hodnotu pro existující hodnota parametru dotazu.</span><span class="sxs-lookup"><span data-stu-id="724a3-430">-   append - appends the value to the existing query parameter value.</span></span><br /><span data-ttu-id="724a3-431">-delete - Odebere parametr dotazu z požadavku.</span><span class="sxs-lookup"><span data-stu-id="724a3-431">-   delete - removes the query parameter from the request.</span></span><br /><br /> <span data-ttu-id="724a3-432">Pokud nastavíte hodnotu `override` uvedení více položek se stejným názvem výsledkem parametr dotazu je nastavena podle všech položek (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku.</span><span class="sxs-lookup"><span data-stu-id="724a3-432">When set to `override` enlisting multiple entries with the same name results in the query parameter being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="724a3-433">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-433">No</span></span>|<span data-ttu-id="724a3-434">přepsání</span><span class="sxs-lookup"><span data-stu-id="724a3-434">override</span></span>|  
|<span data-ttu-id="724a3-435">jméno</span><span class="sxs-lookup"><span data-stu-id="724a3-435">name</span></span>|<span data-ttu-id="724a3-436">Určuje název parametru dotazu nastavit.</span><span class="sxs-lookup"><span data-stu-id="724a3-436">Specifies name of the query parameter to be set.</span></span>|<span data-ttu-id="724a3-437">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-437">Yes</span></span>|<span data-ttu-id="724a3-438">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-438">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="724a3-439">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-439">Usage</span></span>  
 <span data-ttu-id="724a3-440">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-440">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-441">**Části zásady:** příchozí back-end</span><span class="sxs-lookup"><span data-stu-id="724a3-441">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="724a3-442">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-442">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-443"><a name="RewriteURL"></a>Přepsání adresy URL</span><span class="sxs-lookup"><span data-stu-id="724a3-443"><a name="RewriteURL"></a> Rewrite URL</span></span>  
 <span data-ttu-id="724a3-444">`rewrite-uri` Zásad převede adresu URL požadavku z jeho veřejné formuláře do formuláře očekávanou webové služby, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="724a3-444">The `rewrite-uri` policy converts a request URL from its public form to the form expected by the web service, as shown in the following example.</span></span>  
  
-   <span data-ttu-id="724a3-445">Veřejnou adresu URL-`http://api.example.com/storenumber/ordernumber`</span><span class="sxs-lookup"><span data-stu-id="724a3-445">Public URL - `http://api.example.com/storenumber/ordernumber`</span></span>  
  
-   <span data-ttu-id="724a3-446">Adresa URL požadavku –`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span><span class="sxs-lookup"><span data-stu-id="724a3-446">Request URL - `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span></span>  
  
 <span data-ttu-id="724a3-447">Tuto zásadu lze použít, když lidského nebo prohlížeče friendly URL by měla transformuje se na formát adresy URL očekává webovou službou.</span><span class="sxs-lookup"><span data-stu-id="724a3-447">This policy can be used when a human and/or browser-friendly URL should be transformed into the URL format expected by the web service.</span></span> <span data-ttu-id="724a3-448">Tato zásada stačí pro použití při vystavení alternativní formátu adresy URL, například vyčištění adresy URL, RESTful adresy URL, uživatelsky přívětivý adresy URL SEO přátelské adresy URL, které jsou čistě strukturální adresy URL, které nemají obsahovat řetězec dotazu a místo toho obsahují pouze cesty prostředku (po schéma a oprávnění).</span><span class="sxs-lookup"><span data-stu-id="724a3-448">This policy only needs to be applied when exposing an alternative URL format, such as clean URLs, RESTful URLs, user-friendly URLs or SEO-friendly URLs that are purely structural URLs that do not contain a query string and instead contain only the path of the resource (after the scheme and the authority).</span></span> <span data-ttu-id="724a3-449">To se často provádí estetické, použitelnost nebo vyhledávací web pro účely optimalizace (vyhledávací weby SEO).</span><span class="sxs-lookup"><span data-stu-id="724a3-449">This is often done for aesthetic, usability, or search engine optimization (SEO) purposes.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="724a3-450">Pouze můžete přidat pomocí zásad parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="724a3-450">You can only add query string parameters using the policy.</span></span> <span data-ttu-id="724a3-451">Nelze přidat další šablony cestou parametry v adrese URL přepisování.</span><span class="sxs-lookup"><span data-stu-id="724a3-451">You cannot add extra template path parameters in the rewrite URL.</span></span>  

### <a name="policy-statement"></a><span data-ttu-id="724a3-452">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-452">Policy statement</span></span>  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a><span data-ttu-id="724a3-453">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-453">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/v2/US/hardware/{storenumber}&{ordernumber}?City=city&State=state" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/put" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
<!-- Resulting URL will be /put?c=d -->
```  
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/put" copy-unmatched-params="false" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
<!-- Resulting URL will be /put -->
```

### <a name="elements"></a><span data-ttu-id="724a3-454">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-454">Elements</span></span>  
  
|<span data-ttu-id="724a3-455">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-455">Name</span></span>|<span data-ttu-id="724a3-456">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-456">Description</span></span>|<span data-ttu-id="724a3-457">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-457">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-458">Přepište uri</span><span class="sxs-lookup"><span data-stu-id="724a3-458">rewrite-uri</span></span>|<span data-ttu-id="724a3-459">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-459">Root element.</span></span>|<span data-ttu-id="724a3-460">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-460">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="724a3-461">Atributy</span><span class="sxs-lookup"><span data-stu-id="724a3-461">Attributes</span></span>  
  
|<span data-ttu-id="724a3-462">Atribut</span><span class="sxs-lookup"><span data-stu-id="724a3-462">Attribute</span></span>|<span data-ttu-id="724a3-463">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-463">Description</span></span>|<span data-ttu-id="724a3-464">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-464">Required</span></span>|<span data-ttu-id="724a3-465">Výchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-465">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="724a3-466">šablona</span><span class="sxs-lookup"><span data-stu-id="724a3-466">template</span></span>|<span data-ttu-id="724a3-467">Skutečná adresa URL webové služby s všech parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="724a3-467">The actual web service URL with any query string parameters.</span></span> <span data-ttu-id="724a3-468">Pokud používáte výrazy, celou hodnota musí být výraz.</span><span class="sxs-lookup"><span data-stu-id="724a3-468">When using expressions, the whole value must be an expression.</span></span>|<span data-ttu-id="724a3-469">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-469">Yes</span></span>|<span data-ttu-id="724a3-470">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="724a3-470">N/A</span></span>|  
|<span data-ttu-id="724a3-471">kopírování neodpovídající parametry</span><span class="sxs-lookup"><span data-stu-id="724a3-471">copy-unmatched-params</span></span>|<span data-ttu-id="724a3-472">Určuje, zda jsou parametry dotazu v příchozím požadavku nejsou k dispozici v původní šabloně adresy URL přidány na adresu URL definice šablony znovu zápisu</span><span class="sxs-lookup"><span data-stu-id="724a3-472">Specifies whether query parameters in the incoming request not present in the original URL template are added to the URL defined by the re-write template</span></span>|<span data-ttu-id="724a3-473">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-473">No</span></span>|<span data-ttu-id="724a3-474">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="724a3-474">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="724a3-475">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-475">Usage</span></span>  
 <span data-ttu-id="724a3-476">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-476">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-477">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="724a3-477">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="724a3-478">**Zásady obory:** produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-478">**Policy scopes:** product, API, operation</span></span>  
  
##  <span data-ttu-id="724a3-479"><a name="XSLTransform"></a>Transformace XML pomocí transformaci XSLT</span><span class="sxs-lookup"><span data-stu-id="724a3-479"><a name="XSLTransform"></a> Transform XML using an XSLT</span></span>  
 <span data-ttu-id="724a3-480">`Transform XML using an XSLT` Zásad použije transformaci XSL na XML v textu požadavku nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="724a3-480">The `Transform XML using an XSLT` policy applies an XSL transformation to XML in the request or response body.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="724a3-481">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="724a3-481">Policy statement</span></span>  
  
```xml  
<xsl-transform>  
    <parameter name="User-Agent">@(context.Request.Headers.GetValueOrDefault("User-Agent","non-specified"))</parameter>  
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
        <xsl:output method="xml" indent="yes" />  
        <xsl:param name="User-Agent" />  
        <xsl:template match="* | @* | node()">  
            <xsl:copy>  
                <xsl:if test="self::* and not(parent::*)">  
                    <xsl:attribute name="User-Agent">  
                        <xsl:value-of select="$User-Agent" />  
                    </xsl:attribute>  
                </xsl:if>  
                <xsl:apply-templates select="* | @* | node()" />  
            </xsl:copy>  
        </xsl:template>  
    </xsl:stylesheet>  
  </xsl-transform>  
```  
  
### <a name="example"></a><span data-ttu-id="724a3-482">Příklad</span><span class="sxs-lookup"><span data-stu-id="724a3-482">Example</span></span>  
  
```xml  
<policies>  
  <inbound>  
      <base />  
  </inbound>  
  <outbound>  
      <base />  
      <xsl-transform>  
        <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
            <xsl:output omit-xml-declaration="yes" method="xml" indent="yes" />  
            <!-- Copy all nodes directly-->  
            <xsl:template match="node()| @*|*">  
                <xsl:copy>  
                    <xsl:apply-templates select="@* | node()|*" />  
                </xsl:copy>  
            </xsl:template>  
        </xsl:stylesheet>  
    </xsl-transform>  
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="724a3-483">Elementy</span><span class="sxs-lookup"><span data-stu-id="724a3-483">Elements</span></span>  
  
|<span data-ttu-id="724a3-484">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="724a3-484">Name</span></span>|<span data-ttu-id="724a3-485">Popis</span><span class="sxs-lookup"><span data-stu-id="724a3-485">Description</span></span>|<span data-ttu-id="724a3-486">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="724a3-486">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="724a3-487">transformace XSL</span><span class="sxs-lookup"><span data-stu-id="724a3-487">xsl-transform</span></span>|<span data-ttu-id="724a3-488">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="724a3-488">Root element.</span></span>|<span data-ttu-id="724a3-489">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-489">Yes</span></span>|  
|<span data-ttu-id="724a3-490">Parametr</span><span class="sxs-lookup"><span data-stu-id="724a3-490">parameter</span></span>|<span data-ttu-id="724a3-491">Používá k definování proměnné používané v transformaci</span><span class="sxs-lookup"><span data-stu-id="724a3-491">Used to define variables used in the transform</span></span>|<span data-ttu-id="724a3-492">Ne</span><span class="sxs-lookup"><span data-stu-id="724a3-492">No</span></span>|  
|<span data-ttu-id="724a3-493">: stylesheet</span><span class="sxs-lookup"><span data-stu-id="724a3-493">xsl:stylesheet</span></span>|<span data-ttu-id="724a3-494">Kořenový element šablony stylů.</span><span class="sxs-lookup"><span data-stu-id="724a3-494">Root stylesheet element.</span></span> <span data-ttu-id="724a3-495">Všechny elementy a atributy definované v rámci postupujte podle standardu [specifikace XSLT](http://www.w3.org/TR/xslt)</span><span class="sxs-lookup"><span data-stu-id="724a3-495">All elements and attributes defined within follow the standard [XSLT specification](http://www.w3.org/TR/xslt)</span></span>|<span data-ttu-id="724a3-496">Ano</span><span class="sxs-lookup"><span data-stu-id="724a3-496">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="724a3-497">Využití</span><span class="sxs-lookup"><span data-stu-id="724a3-497">Usage</span></span>  
 <span data-ttu-id="724a3-498">Tuto zásadu lze použít v tyto zásady [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="724a3-498">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="724a3-499">**Části zásady:** vstupní, výstupní</span><span class="sxs-lookup"><span data-stu-id="724a3-499">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="724a3-500">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="724a3-500">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="724a3-501">Další kroky</span><span class="sxs-lookup"><span data-stu-id="724a3-501">Next steps</span></span>
<span data-ttu-id="724a3-502">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="724a3-502">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
