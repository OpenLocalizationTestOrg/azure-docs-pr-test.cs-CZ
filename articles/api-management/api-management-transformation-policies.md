---
title: "zásad transformace aaaAzure API Management | Microsoft Docs"
description: "Další informace o zásad transformace hello k dispozici pro použití v Azure API Management."
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
ms.openlocfilehash: 2891cc52d0017b717b3c12a98bc4941b5fd7ea78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-transformation-policies"></a><span data-ttu-id="b0cab-103">Transformace zásady služby API Management</span><span class="sxs-lookup"><span data-stu-id="b0cab-103">API Management transformation policies</span></span>
<span data-ttu-id="b0cab-104">Toto téma obsahuje odkaz pro hello následující zásady služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b0cab-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="b0cab-105">Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="b0cab-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="b0cab-106"><a name="TransformationPolicies"></a>Zásad transformace</span><span class="sxs-lookup"><span data-stu-id="b0cab-106"><a name="TransformationPolicies"></a> Transformation policies</span></span>  
  
-   <span data-ttu-id="b0cab-107">[Převést JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) – převede požadavku nebo odpovědi textu z JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="b0cab-107">[Convert JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON tooXML.</span></span>  
  
-   <span data-ttu-id="b0cab-108">[Převod XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – převede požadavku nebo odpovědi textu z XML tooJSON.</span><span class="sxs-lookup"><span data-stu-id="b0cab-108">[Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML tooJSON.</span></span>  
  
-   <span data-ttu-id="b0cab-109">[Najít a nahradit řetězec v textu](api-management-transformation-policies.md#Findandreplacestringinbody) – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b0cab-109">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
-   <span data-ttu-id="b0cab-110">[Maskování adresy URL v obsahu](api-management-transformation-policies.md#MaskURLSContent) -znovu zapíše (masky) odkazy v odpovědi hello body tak, aby ukazovaly toohello ekvivalentní propojení prostřednictvím brány hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-110">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>  
  
-   <span data-ttu-id="b0cab-111">[Nastavení back-end služby](api-management-transformation-policies.md#SetBackendService) -změny hello back-end službu pro příchozí žádosti.</span><span class="sxs-lookup"><span data-stu-id="b0cab-111">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes hello backend service for an incoming request.</span></span>  
  
-   <span data-ttu-id="b0cab-112">[Tělo nastavit](api-management-transformation-policies.md#SetBody) -nastaví hello text zprávy pro příchozí a odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="b0cab-112">[Set body](api-management-transformation-policies.md#SetBody) - Sets hello message body for incoming and outgoing requests.</span></span>  
  
-   <span data-ttu-id="b0cab-113">[Set – hlavička protokolu HTTP](api-management-transformation-policies.md#SetHTTPheader) – přiřadí hodnota tooan existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.</span><span class="sxs-lookup"><span data-stu-id="b0cab-113">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
-   <span data-ttu-id="b0cab-114">[Nastavte parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.</span><span class="sxs-lookup"><span data-stu-id="b0cab-114">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
-   <span data-ttu-id="b0cab-115">[Přepsání adresy URL](api-management-transformation-policies.md#RewriteURL) – převede adresu URL požadavku z formuláře toohello jeho veřejné formuláře očekávanou hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="b0cab-115">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form toohello form expected by hello web service.</span></span>  
  
-   <span data-ttu-id="b0cab-116">[Transformace XML pomocí transformaci XSLT](api-management-transformation-policies.md#XSLTransform) -vztahuje k transformaci tooXML XSL v textu požadavku nebo odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-116">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
##  <span data-ttu-id="b0cab-117"><a name="ConvertJSONtoXML"></a>Převést JSON tooXML</span><span class="sxs-lookup"><span data-stu-id="b0cab-117"><a name="ConvertJSONtoXML"></a> Convert JSON tooXML</span></span>  
 <span data-ttu-id="b0cab-118">Hello `json-to-xml` zásad převede text požadavku nebo odpovědi z JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="b0cab-118">hello `json-to-xml` policy converts a request or response body from JSON tooXML.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-119">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-119">Policy statement</span></span>  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="b0cab-120">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-120">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="b0cab-121">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-121">Elements</span></span>  
  
|<span data-ttu-id="b0cab-122">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-122">Name</span></span>|<span data-ttu-id="b0cab-123">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-123">Description</span></span>|<span data-ttu-id="b0cab-124">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-124">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-125">JSON do xml</span><span class="sxs-lookup"><span data-stu-id="b0cab-125">json-to-xml</span></span>|<span data-ttu-id="b0cab-126">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-126">Root element.</span></span>|<span data-ttu-id="b0cab-127">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-127">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="b0cab-128">Atributy</span><span class="sxs-lookup"><span data-stu-id="b0cab-128">Attributes</span></span>  
  
|<span data-ttu-id="b0cab-129">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-129">Name</span></span>|<span data-ttu-id="b0cab-130">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-130">Description</span></span>|<span data-ttu-id="b0cab-131">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-131">Required</span></span>|<span data-ttu-id="b0cab-132">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-132">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b0cab-133">Použít</span><span class="sxs-lookup"><span data-stu-id="b0cab-133">apply</span></span>|<span data-ttu-id="b0cab-134">Hello musí být nastaven tooone hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b0cab-134">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="b0cab-135">-vždy - vždy použít převod.</span><span class="sxs-lookup"><span data-stu-id="b0cab-135">-   always - always apply conversion.</span></span><br /><span data-ttu-id="b0cab-136">obsah json typ – převod pouze v případě, že přítomnost JSON určuje hlavičku odpovědi Content-Type.</span><span class="sxs-lookup"><span data-stu-id="b0cab-136">-   content-type-json - convert only if response Content-Type header indicates presence of JSON.</span></span>|<span data-ttu-id="b0cab-137">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-137">Yes</span></span>|<span data-ttu-id="b0cab-138">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-138">N/A</span></span>|  
|<span data-ttu-id="b0cab-139">Vezměte v úvahu přijmout – hlavička</span><span class="sxs-lookup"><span data-stu-id="b0cab-139">consider-accept-header</span></span>|<span data-ttu-id="b0cab-140">Hello musí být nastaven tooone hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b0cab-140">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="b0cab-141">Pokud se vyžaduje JSON v požadavku hlavička Accept - true – použít převod.</span><span class="sxs-lookup"><span data-stu-id="b0cab-141">-   true - apply conversion if JSON is requested in request Accept header.</span></span><br /><span data-ttu-id="b0cab-142">-false – vždy použít převod.</span><span class="sxs-lookup"><span data-stu-id="b0cab-142">-   false -always apply conversion.</span></span>|<span data-ttu-id="b0cab-143">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-143">No</span></span>|<span data-ttu-id="b0cab-144">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="b0cab-144">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b0cab-145">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-145">Usage</span></span>  
 <span data-ttu-id="b0cab-146">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-146">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-147">**Části zásady:** příchozích, odchozích, při chybě</span><span class="sxs-lookup"><span data-stu-id="b0cab-147">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="b0cab-148">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-148">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-149"><a name="ConvertXMLtoJSON"></a>Převést XML tooJSON</span><span class="sxs-lookup"><span data-stu-id="b0cab-149"><a name="ConvertXMLtoJSON"></a> Convert XML tooJSON</span></span>  
 <span data-ttu-id="b0cab-150">Hello `xml-to-json` zásad převede text požadavku nebo odpovědi z XML tooJSON.</span><span class="sxs-lookup"><span data-stu-id="b0cab-150">hello `xml-to-json` policy converts a request or response body from XML tooJSON.</span></span> <span data-ttu-id="b0cab-151">Tuto zásadu lze použít toomodernize rozhraní API založené na webových službách jen XML back-end.</span><span class="sxs-lookup"><span data-stu-id="b0cab-151">This policy can be used toomodernize APIs based on XML-only backend web services.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-152">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-152">Policy statement</span></span>  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="b0cab-153">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-153">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="b0cab-154">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-154">Elements</span></span>  
  
|<span data-ttu-id="b0cab-155">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-155">Name</span></span>|<span data-ttu-id="b0cab-156">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-156">Description</span></span>|<span data-ttu-id="b0cab-157">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-157">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-158">XML – json</span><span class="sxs-lookup"><span data-stu-id="b0cab-158">xml-to-json</span></span>|<span data-ttu-id="b0cab-159">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-159">Root element.</span></span>|<span data-ttu-id="b0cab-160">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-160">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="b0cab-161">Atributy</span><span class="sxs-lookup"><span data-stu-id="b0cab-161">Attributes</span></span>  
  
|<span data-ttu-id="b0cab-162">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-162">Name</span></span>|<span data-ttu-id="b0cab-163">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-163">Description</span></span>|<span data-ttu-id="b0cab-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-164">Required</span></span>|<span data-ttu-id="b0cab-165">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-165">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b0cab-166">Typ</span><span class="sxs-lookup"><span data-stu-id="b0cab-166">kind</span></span>|<span data-ttu-id="b0cab-167">Hello musí být nastaven tooone hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b0cab-167">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="b0cab-168">javascript – procházející - hello převést JSON má popisný tooJavaScript vývojáři formuláře.</span><span class="sxs-lookup"><span data-stu-id="b0cab-168">-   javascript-friendly - hello converted JSON has a form friendly tooJavaScript developers.</span></span><br /><span data-ttu-id="b0cab-169">-direct - hello převedený JSON odráží hello původní XML struktury dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-169">-   direct - hello converted JSON reflects hello original XML document's structure.</span></span>|<span data-ttu-id="b0cab-170">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-170">Yes</span></span>|<span data-ttu-id="b0cab-171">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-171">N/A</span></span>|  
|<span data-ttu-id="b0cab-172">Použít</span><span class="sxs-lookup"><span data-stu-id="b0cab-172">apply</span></span>|<span data-ttu-id="b0cab-173">Hello musí být nastaven tooone hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b0cab-173">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="b0cab-174">Převeďte – vždy - vždy.</span><span class="sxs-lookup"><span data-stu-id="b0cab-174">-   always - convert always.</span></span><br /><span data-ttu-id="b0cab-175">obsah typu xml - převést pouze v případě, že hlavičku odpovědi Content-Type označuje přítomnost XML.</span><span class="sxs-lookup"><span data-stu-id="b0cab-175">-   content-type-xml - convert only if response Content-Type header indicates presence of XML.</span></span>|<span data-ttu-id="b0cab-176">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-176">Yes</span></span>|<span data-ttu-id="b0cab-177">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-177">N/A</span></span>|  
|<span data-ttu-id="b0cab-178">Vezměte v úvahu přijmout – hlavička</span><span class="sxs-lookup"><span data-stu-id="b0cab-178">consider-accept-header</span></span>|<span data-ttu-id="b0cab-179">Hello musí být nastaven tooone hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b0cab-179">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="b0cab-180">Pokud se vyžaduje XML v žádosti o hlavička Accept - true – použít převod.</span><span class="sxs-lookup"><span data-stu-id="b0cab-180">-   true - apply conversion if XML is requested in request Accept header.</span></span><br /><span data-ttu-id="b0cab-181">-false – vždy použít převod.</span><span class="sxs-lookup"><span data-stu-id="b0cab-181">-   false -always apply conversion.</span></span>|<span data-ttu-id="b0cab-182">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-182">No</span></span>|<span data-ttu-id="b0cab-183">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="b0cab-183">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b0cab-184">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-184">Usage</span></span>  
 <span data-ttu-id="b0cab-185">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-185">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-186">**Části zásady:** příchozích, odchozích, při chybě</span><span class="sxs-lookup"><span data-stu-id="b0cab-186">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="b0cab-187">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-187">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-188"><a name="Findandreplacestringinbody"></a>Najít a nahradit řetězec v textu</span><span class="sxs-lookup"><span data-stu-id="b0cab-188"><a name="Findandreplacestringinbody"></a> Find and replace string in body</span></span>  
 <span data-ttu-id="b0cab-189">Hello `find-and-replace` zásady najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.</span><span class="sxs-lookup"><span data-stu-id="b0cab-189">hello `find-and-replace` policy finds a request or response substring and replaces it with a different substring.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-190">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-190">Policy statement</span></span>  
  
```xml  
<find-and-replace from="what tooreplace" to="replacement" />  
```  
  
### <a name="example"></a><span data-ttu-id="b0cab-191">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-191">Example</span></span>  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a><span data-ttu-id="b0cab-192">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-192">Elements</span></span>  
  
|<span data-ttu-id="b0cab-193">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-193">Name</span></span>|<span data-ttu-id="b0cab-194">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-194">Description</span></span>|<span data-ttu-id="b0cab-195">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-195">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-196">hledání a nahrazování</span><span class="sxs-lookup"><span data-stu-id="b0cab-196">find-and-replace</span></span>|<span data-ttu-id="b0cab-197">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-197">Root element.</span></span>|<span data-ttu-id="b0cab-198">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-198">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="b0cab-199">Atributy</span><span class="sxs-lookup"><span data-stu-id="b0cab-199">Attributes</span></span>  
  
|<span data-ttu-id="b0cab-200">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-200">Name</span></span>|<span data-ttu-id="b0cab-201">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-201">Description</span></span>|<span data-ttu-id="b0cab-202">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-202">Required</span></span>|<span data-ttu-id="b0cab-203">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-203">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b0cab-204">Z</span><span class="sxs-lookup"><span data-stu-id="b0cab-204">from</span></span>|<span data-ttu-id="b0cab-205">Hello toosearch řetězec pro.</span><span class="sxs-lookup"><span data-stu-id="b0cab-205">hello string toosearch for.</span></span>|<span data-ttu-id="b0cab-206">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-206">Yes</span></span>|<span data-ttu-id="b0cab-207">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-207">N/A</span></span>|  
|<span data-ttu-id="b0cab-208">na</span><span class="sxs-lookup"><span data-stu-id="b0cab-208">to</span></span>|<span data-ttu-id="b0cab-209">Hello náhradní řetězec.</span><span class="sxs-lookup"><span data-stu-id="b0cab-209">hello replacement string.</span></span> <span data-ttu-id="b0cab-210">Zadejte řetězec nulové délky náhradní řetězec tooremove hello vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="b0cab-210">Specify a zero length replacement string tooremove hello search string.</span></span>|<span data-ttu-id="b0cab-211">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-211">Yes</span></span>|<span data-ttu-id="b0cab-212">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-212">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b0cab-213">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-213">Usage</span></span>  
 <span data-ttu-id="b0cab-214">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-214">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-215">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="b0cab-215">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="b0cab-216">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-216">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-217"><a name="MaskURLSContent"></a>Maska adresy URL v obsahu</span><span class="sxs-lookup"><span data-stu-id="b0cab-217"><a name="MaskURLSContent"></a> Mask URLs in content</span></span>  
 <span data-ttu-id="b0cab-218">Hello `redirect-content-urls` zásad znovu zapíše odkazy (masky) v odpovědi hello tak, aby ukazovaly toohello ekvivalentní propojení prostřednictvím brány hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-218">hello `redirect-content-urls` policy re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span> <span data-ttu-id="b0cab-219">V hello odchozí části toore zápisu odpovědi textu odkazy toomake je použijte bod toohello brány.</span><span class="sxs-lookup"><span data-stu-id="b0cab-219">Use in hello outbound section toore-write response body links toomake them point toohello gateway.</span></span> <span data-ttu-id="b0cab-220">Použití v hello příchozí části opačný efekt.</span><span class="sxs-lookup"><span data-stu-id="b0cab-220">Use in hello inbound section for an opposite effect.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="b0cab-221">Tato zásada nezmění žádné hodnoty hlavičky, jako `Location` hlavičky.</span><span class="sxs-lookup"><span data-stu-id="b0cab-221">This policy does not change any header values such as `Location` headers.</span></span> <span data-ttu-id="b0cab-222">hodnoty hlavičky toochange, použijte hello [sadu hlaviček](api-management-transformation-policies.md#SetHTTPheader) zásad.</span><span class="sxs-lookup"><span data-stu-id="b0cab-222">toochange header values, use hello [set-header](api-management-transformation-policies.md#SetHTTPheader) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-223">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-223">Policy statement</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a><span data-ttu-id="b0cab-224">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-224">Example</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a><span data-ttu-id="b0cab-225">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-225">Elements</span></span>  
  
|<span data-ttu-id="b0cab-226">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-226">Name</span></span>|<span data-ttu-id="b0cab-227">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-227">Description</span></span>|<span data-ttu-id="b0cab-228">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-228">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-229">adresy URL přesměrování obsahu</span><span class="sxs-lookup"><span data-stu-id="b0cab-229">redirect-content-urls</span></span>|<span data-ttu-id="b0cab-230">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-230">Root element.</span></span>|<span data-ttu-id="b0cab-231">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-231">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b0cab-232">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-232">Usage</span></span>  
 <span data-ttu-id="b0cab-233">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-233">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-234">**Části zásady:** vstupní, výstupní</span><span class="sxs-lookup"><span data-stu-id="b0cab-234">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="b0cab-235">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-235">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-236"><a name="SetBackendService"></a>Nastavení back-end služby</span><span class="sxs-lookup"><span data-stu-id="b0cab-236"><a name="SetBackendService"></a> Set backend service</span></span>  
 <span data-ttu-id="b0cab-237">Použití hello `set-backend-service` zásad tooredirect příchozí žádosti o různých back-end tooa než hello je zadáno v nastavení hello rozhraní API pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="b0cab-237">Use hello `set-backend-service` policy tooredirect an incoming request tooa different backend than hello one specified in hello API settings for that operation.</span></span> <span data-ttu-id="b0cab-238">Tato zásada se mění hello back-end služby základní adresu URL hello příchozí požadavek toohello uvedené v zásadách hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-238">This policy changes hello backend service base URL of hello incoming request toohello one specified in hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-239">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-239">Policy statement</span></span>  
  
```xml  
<set-backend-service base-url="base URL of hello backend service" />  
```  
  
### <a name="example"></a><span data-ttu-id="b0cab-240">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-240">Example</span></span>  
  
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
<span data-ttu-id="b0cab-241">V tento příklad hello nastavit back-end službu zásady směrování požadavků na základě hodnoty verze hello předaná hello dotazu řetězec tooa různých back-end službu než jeden zadaný v hello hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b0cab-241">In this example hello set backend service policy routes requests based on hello version value passed in hello query string tooa different backend service than hello one specified in hello API.</span></span>
  
<span data-ttu-id="b0cab-242">Původně hello základní adresu URL back-end služby je odvozená od nastavení hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b0cab-242">Initially hello backend service base URL is derived from hello API settings.</span></span> <span data-ttu-id="b0cab-243">Proto hello adrese URL žádosti `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` stane `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` kde `http://contoso.com/api/10.4/` je back-end adresa URL služby hello uvedenou v nastavení hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b0cab-243">So hello request URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` becomes `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` where `http://contoso.com/api/10.4/` is hello backend service URL specified in hello API settings.</span></span>  
  
<span data-ttu-id="b0cab-244">Když hello [< zvolte\> ](api-management-advanced-policies.md#choose) platí prohlášení o zásadách základní adresu URL aplikace hello back-end služby může změnit znovu buď příliš`http://contoso.com/api/8.2` nebo `http://contoso.com/api/9.1`, v závislosti na hello hodnota parametru dotazu požadavku verze hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-244">When hello [<choose\>](api-management-advanced-policies.md#choose) policy statement is applied hello backend service base URL may change again either too`http://contoso.com/api/8.2` or `http://contoso.com/api/9.1`, depending on hello value of hello version request query parameter.</span></span> <span data-ttu-id="b0cab-245">Například pokud hello hodnota je `"2013-15"` hello poslední žádosti se změní na adresu URL `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span><span class="sxs-lookup"><span data-stu-id="b0cab-245">For example, if hello value is `"2013-15"` hello final request URL becomes `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span></span>  
  
<span data-ttu-id="b0cab-246">Pokud transformace hello požadavku je další požadované, ostatní [zásad transformace](api-management-transformation-policies.md#TransformationPolicies) lze použít.</span><span class="sxs-lookup"><span data-stu-id="b0cab-246">If further transformation of hello request is desired, other [Transformation policies](api-management-transformation-policies.md#TransformationPolicies) can be used.</span></span> <span data-ttu-id="b0cab-247">Například tooremove hello verze dotazu parametr teď, když hello požadavku se směrují tooa verze konkrétní back-end, hello [nastavit parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) zásada se dá použít tooremove hello teď redundantní verze atribut.</span><span class="sxs-lookup"><span data-stu-id="b0cab-247">For example, tooremove hello version query parameter now that hello request is being routed tooa version specific backend, hello  [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policy can be used tooremove hello now redundant version attribute.</span></span>  
  
### <a name="example"></a><span data-ttu-id="b0cab-248">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-248">Example</span></span>  
  
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
<span data-ttu-id="b0cab-249">V tento příklad hello zásad trasy hello požadavek tooa služby fabric back-end používání řetězec dotazu userId hello jako klíč oddílu hello a hello primární repliku hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-249">In this example hello policy routes hello request tooa service fabric backend, using hello userId query string as hello partition key and using hello primary replica of hello partition.</span></span>  

### <a name="elements"></a><span data-ttu-id="b0cab-250">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-250">Elements</span></span>  
  
|<span data-ttu-id="b0cab-251">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-251">Name</span></span>|<span data-ttu-id="b0cab-252">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-252">Description</span></span>|<span data-ttu-id="b0cab-253">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-253">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-254">set-back-end service</span><span class="sxs-lookup"><span data-stu-id="b0cab-254">set-backend-service</span></span>|<span data-ttu-id="b0cab-255">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-255">Root element.</span></span>|<span data-ttu-id="b0cab-256">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-256">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="b0cab-257">Atributy</span><span class="sxs-lookup"><span data-stu-id="b0cab-257">Attributes</span></span>  
  
|<span data-ttu-id="b0cab-258">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-258">Name</span></span>|<span data-ttu-id="b0cab-259">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-259">Description</span></span>|<span data-ttu-id="b0cab-260">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-260">Required</span></span>|<span data-ttu-id="b0cab-261">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-261">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b0cab-262">Základní adresa url</span><span class="sxs-lookup"><span data-stu-id="b0cab-262">base-url</span></span>|<span data-ttu-id="b0cab-263">Nový back-end základní adresa URL služby.</span><span class="sxs-lookup"><span data-stu-id="b0cab-263">New backend service base URL.</span></span>|<span data-ttu-id="b0cab-264">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-264">No</span></span>|<span data-ttu-id="b0cab-265">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-265">N/A</span></span>|  
|<span data-ttu-id="b0cab-266">id back-end</span><span class="sxs-lookup"><span data-stu-id="b0cab-266">backend-id</span></span>|<span data-ttu-id="b0cab-267">Identifikátor tooroute back-end hello k.</span><span class="sxs-lookup"><span data-stu-id="b0cab-267">Identifier of hello backend tooroute to.</span></span>|<span data-ttu-id="b0cab-268">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-268">No</span></span>|<span data-ttu-id="b0cab-269">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-269">N/A</span></span>|  
|<span data-ttu-id="b0cab-270">klíč oddílu SF</span><span class="sxs-lookup"><span data-stu-id="b0cab-270">sf-partition-key</span></span>|<span data-ttu-id="b0cab-271">Platí jenom při hello back-end je služba Service Fabric a je určen pomocí back-end id.</span><span class="sxs-lookup"><span data-stu-id="b0cab-271">Only applicable when hello backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="b0cab-272">Použít tooresolve na konkrétní oddíl z překládání adres hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-272">Used tooresolve a specific partition from hello name resolution service.</span></span>|<span data-ttu-id="b0cab-273">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-273">No</span></span>|<span data-ttu-id="b0cab-274">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-274">N/A</span></span>|  
|<span data-ttu-id="b0cab-275">Typ SF repliky</span><span class="sxs-lookup"><span data-stu-id="b0cab-275">sf-replica-type</span></span>|<span data-ttu-id="b0cab-276">Platí jenom při hello back-end je služba Service Fabric a je určen pomocí back-end id.</span><span class="sxs-lookup"><span data-stu-id="b0cab-276">Only applicable when hello backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="b0cab-277">Ovládací prvky, pokud požadavek hello by měli přejít toohello primární nebo sekundární replice oddílu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-277">Controls if hello request should go toohello primary or secondary replica of a partition.</span></span> |<span data-ttu-id="b0cab-278">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-278">No</span></span>|<span data-ttu-id="b0cab-279">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-279">N/A</span></span>|    
|<span data-ttu-id="b0cab-280">SF. vyřešte podmínek</span><span class="sxs-lookup"><span data-stu-id="b0cab-280">sf-resolve-condition</span></span>|<span data-ttu-id="b0cab-281">Platí jenom při hello back-end je služba Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b0cab-281">Only applicable when hello backend is a Service Fabric service.</span></span> <span data-ttu-id="b0cab-282">Podmínka identifikace, pokud hello volat back-end Fabric tooService má toobe opakuje s nové řešení.</span><span class="sxs-lookup"><span data-stu-id="b0cab-282">Condition identifying if hello call tooService Fabric backend has toobe repeated with new resolution.</span></span>|<span data-ttu-id="b0cab-283">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-283">No</span></span>|<span data-ttu-id="b0cab-284">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-284">N/A</span></span>|    
|<span data-ttu-id="b0cab-285">SF-service-instance-name</span><span class="sxs-lookup"><span data-stu-id="b0cab-285">sf-service-instance-name</span></span>|<span data-ttu-id="b0cab-286">Platí jenom při hello back-end je služba Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b0cab-286">Only applicable when hello backend is a Service Fabric service.</span></span> <span data-ttu-id="b0cab-287">Umožňuje toochange instance služby za běhu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-287">Allows toochange service instances at runtime.</span></span> |<span data-ttu-id="b0cab-288">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-288">No</span></span>|<span data-ttu-id="b0cab-289">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-289">N/A</span></span>|    

### <a name="usage"></a><span data-ttu-id="b0cab-290">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-290">Usage</span></span>  
 <span data-ttu-id="b0cab-291">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-291">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-292">**Části zásady:** příchozí back-end</span><span class="sxs-lookup"><span data-stu-id="b0cab-292">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="b0cab-293">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-293">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-294"><a name="SetBody"></a>Sada textu</span><span class="sxs-lookup"><span data-stu-id="b0cab-294"><a name="SetBody"></a> Set body</span></span>  
 <span data-ttu-id="b0cab-295">Použití hello `set-body` zásad tooset hello text zprávy pro příchozí a odchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="b0cab-295">Use hello `set-body` policy tooset hello message body for incoming and outgoing requests.</span></span> <span data-ttu-id="b0cab-296">tělo zprávy, které můžete použít hello hello tooaccess `context.Request.Body` vlastnost nebo hello `context.Response.Body`, v závislosti na tom, zda text hello zásad v hello příchozí nebo odchozí části.</span><span class="sxs-lookup"><span data-stu-id="b0cab-296">tooaccess hello message body you can use hello `context.Request.Body` property or hello `context.Response.Body`, depending on whether hello policy is in hello inbound or outbound section.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="b0cab-297">Všimněte si, že ve výchozím nastavení při přístupu k hello pomocí tělo zprávy `context.Request.Body` nebo `context.Response.Body`, původní zprávy hello textu dojde ke ztrátě a musí být nastaven vrácením textu hello zpět ve výrazu hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-297">Note that by default when you access hello message body using `context.Request.Body` or `context.Response.Body`, hello original message body is lost and must be set by returning hello body back in hello expression.</span></span> <span data-ttu-id="b0cab-298">textu hello toopreserve obsahu, nastavte hello `preserveContent` parametr příliš`true` při přístupu k uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-298">toopreserve hello body content, set hello `preserveContent` parameter too`true` when accessing hello message.</span></span> <span data-ttu-id="b0cab-299">Pokud `preserveContent` je nastaven příliš`true` a jiný text se vrátí hello výrazem hello vrátil textu se používá.</span><span class="sxs-lookup"><span data-stu-id="b0cab-299">If `preserveContent` is set too`true` and a different body is returned by hello expression, hello returned body is used.</span></span>  
>   
>  <span data-ttu-id="b0cab-300">Upozorňujeme hello následující aspekty při používání hello `set-body` zásad.</span><span class="sxs-lookup"><span data-stu-id="b0cab-300">Please note hello following considerations when using hello `set-body` policy.</span></span>  
>   
>  -   <span data-ttu-id="b0cab-301">Pokud používáte hello `set-body` zásad tooreturn nové nebo aktualizované text nepotřebujete tooset `preserveContent` příliš`true` vzhledem k tomu, že jsou explicitně poskytuje nový obsah textu hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-301">If you are using hello `set-body` policy tooreturn a new or updated body you don't need tooset `preserveContent` too`true` because you are explicitly supplying hello new body contents.</span></span>  
> -   <span data-ttu-id="b0cab-302">Zachování obsahu hello odpovědi v kanálu příchozí hello nebude mít smysl, protože nepřijde žádná odpověď ještě.</span><span class="sxs-lookup"><span data-stu-id="b0cab-302">Preserving hello content of a response in hello inbound pipeline doesn't make sense because there is no response yet.</span></span>  
> -   <span data-ttu-id="b0cab-303">Zachování hello obsahu žádosti v kanálu odchozí hello nebude mít smysl, protože hello žádost již byla odeslána toohello back-end v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="b0cab-303">Preserving hello content of a request in hello outbound pipeline doesn't make sense because hello request has already been sent toohello backend at this point.</span></span>  
> -   <span data-ttu-id="b0cab-304">Pokud tato zásada je použita, pokud neexistuje žádný text zprávy, například v příchozí GET, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="b0cab-304">If this policy is used when there is no message body, for example in an inbound GET, an exception is thrown.</span></span>  
  
 <span data-ttu-id="b0cab-305">Další informace najdete v tématu hello `context.Request.Body`, `context.Response.Body`a hello `IMessage` části hello [kontextové proměnné](api-management-policy-expressions.md#ContextVariables) tabulky.</span><span class="sxs-lookup"><span data-stu-id="b0cab-305">For more information, see hello `context.Request.Body`, `context.Response.Body`, and hello `IMessage` sections in hello [Context variable](api-management-policy-expressions.md#ContextVariables) table.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-306">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-306">Policy statement</span></span>  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a><span data-ttu-id="b0cab-307">Příklady</span><span class="sxs-lookup"><span data-stu-id="b0cab-307">Examples</span></span>  
  
#### <a name="literal-text-example"></a><span data-ttu-id="b0cab-308">Příklad textového literálu</span><span class="sxs-lookup"><span data-stu-id="b0cab-308">Literal text example</span></span>  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-hello-body-as-a-string-note-that-we-are-preserving-hello-original-request-body-so-that-we-can-access-it-later-in-hello-pipeline"></a><span data-ttu-id="b0cab-309">Příklad přístup k textu hello jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="b0cab-309">Example accessing hello body as a string.</span></span> <span data-ttu-id="b0cab-310">Všimněte si, původní text žádosti hello jsme se zachováním, tak, aby jsme k němu přístup později v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-310">Note that we are preserving hello original request body so that we can access it later in hello pipeline.</span></span>
  
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
  
#### <a name="example-accessing-hello-body-as-a-jobject-note-that-since-we-are-not-reserving-hello-original-request-body-accesing-it-later-in-hello-pipeline-will-result-in-an-exception"></a><span data-ttu-id="b0cab-311">Příklad přístup k textu hello jako JObject.</span><span class="sxs-lookup"><span data-stu-id="b0cab-311">Example accessing hello body as a JObject.</span></span> <span data-ttu-id="b0cab-312">Pamatujte, že vzhledem k tomu, že jsme nejsou rezervování hello původní text žádosti, accesing ho později v kanálu hello způsobí výjimku.</span><span class="sxs-lookup"><span data-stu-id="b0cab-312">Note that since we are not reserving hello original request body, accesing it later in hello pipeline will result in an exception.</span></span>  
  
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
  
#### <a name="filter-response-based-on-product"></a><span data-ttu-id="b0cab-313">Filtr reakci na produktu</span><span class="sxs-lookup"><span data-stu-id="b0cab-313">Filter response based on product</span></span>  
 <span data-ttu-id="b0cab-314">Tento příklad ukazuje, jak tooperform filtrování obsahu odebráním datové prvky. z odpovědi hello přijal od služby back-end hello při použití hello `Starter` produktu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-314">This example shows how tooperform content filtering by removing data elements from hello response received from hello backend service when using hello `Starter` product.</span></span> <span data-ttu-id="b0cab-315">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too34:30.</span><span class="sxs-lookup"><span data-stu-id="b0cab-315">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too34:30.</span></span> <span data-ttu-id="b0cab-316">Spuštění 31:50 toosee přehled [hello tmavý Sky prognózy API](https://developer.forecast.io/) použít v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="b0cab-316">Start  at 31:50 toosee an overview of [hello Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
```xml  
<!-- Copy this snippet into hello outbound section tooremove a number of data elements from hello response received from hello backend service based on hello name of hello api product -->  
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

### <a name="using-liquid-templates-with-set-body"></a><span data-ttu-id="b0cab-317">Pomocí sady textu kapaliny šablony</span><span class="sxs-lookup"><span data-stu-id="b0cab-317">Using Liquid templates with set body</span></span> 
<span data-ttu-id="b0cab-318">Hello `set-body` zásad může být nakonfigurované toouse hello [kapaliny](https://shopify.github.io/liquid/basics/introduction/) ukázka jazyk tootransfom hello textu požadavku nebo odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b0cab-318">hello `set-body` policy can be configured toouse hello [Liquid](https://shopify.github.io/liquid/basics/introduction/) templating language tootransfom hello body of a request or response.</span></span> <span data-ttu-id="b0cab-319">To může být velmi efektivní, pokud potřebujete toocompletely změna tvaru hello formát zprávy.</span><span class="sxs-lookup"><span data-stu-id="b0cab-319">This can be very effective if you need toocompletely reshape hello format of your message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b0cab-320">Hello implementace kapaliny použít v hello `set-body` v "režimu C#, jsou nakonfigurované zásady.</span><span class="sxs-lookup"><span data-stu-id="b0cab-320">hello implementation of Liquid used in hello `set-body` policy is configured in 'C# mode'.</span></span> <span data-ttu-id="b0cab-321">To je zvlášť důležité při provádění akcí, například filtrování.</span><span class="sxs-lookup"><span data-stu-id="b0cab-321">This is particularly important when doing things such as filtering.</span></span> <span data-ttu-id="b0cab-322">Jako příklad použití Filtr kalendářních dat vyžaduje použití hello Pascal velká a malá písmena a C# datum formátování např:</span><span class="sxs-lookup"><span data-stu-id="b0cab-322">As an example, using a date filter requires hello use of Pascal casing and C# date formatting e.g.:</span></span>
>
> <span data-ttu-id="b0cab-323">{{body.foo.startDateTime| Datum: "yyyyMMddTHH:mm:ddZ"}}</span><span class="sxs-lookup"><span data-stu-id="b0cab-323">{{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b0cab-324">V pořadí toocorrectly vazby tooan textu XML pomocí hello kapaliny šablony, používat `set-header` zásad tooset Content-Type tooeither application/xml, text/xml (nebo libovolný typ konče + xml); pro text JSON, musí být application/json, text/json (nebo libovolný typ ukončování s + json).</span><span class="sxs-lookup"><span data-stu-id="b0cab-324">In order toocorrectly bind tooan XML body using hello Liquid template, use a `set-header` policy tooset Content-Type tooeither application/xml, text/xml (or any type ending with +xml); for a JSON body, it must be application/json, text/json (or any type ending with +json).</span></span>

#### <a name="convert-json-toosoap-using-a-liquid-template"></a><span data-ttu-id="b0cab-325">Převést JSON tooSOAP pomocí kapaliny šablony</span><span class="sxs-lookup"><span data-stu-id="b0cab-325">Convert JSON tooSOAP using a Liquid template</span></span>
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

#### <a name="tranform-json-using-a-liquid-template"></a><span data-ttu-id="b0cab-326">JSON Tranform pomocí kapaliny šablony</span><span class="sxs-lookup"><span data-stu-id="b0cab-326">Tranform JSON using a Liquid template</span></span>
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a><span data-ttu-id="b0cab-327">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-327">Elements</span></span>  
  
|<span data-ttu-id="b0cab-328">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-328">Name</span></span>|<span data-ttu-id="b0cab-329">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-329">Description</span></span>|<span data-ttu-id="b0cab-330">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-330">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-331">Sada textu</span><span class="sxs-lookup"><span data-stu-id="b0cab-331">set-body</span></span>|<span data-ttu-id="b0cab-332">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-332">Root element.</span></span> <span data-ttu-id="b0cab-333">Obsahuje základní text hello nebo výrazy, které vrací text.</span><span class="sxs-lookup"><span data-stu-id="b0cab-333">Contains hello body text or an expressions that returns a body.</span></span>|<span data-ttu-id="b0cab-334">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-334">Yes</span></span>|  

### <a name="properties"></a><span data-ttu-id="b0cab-335">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="b0cab-335">Properties</span></span>  
  
|<span data-ttu-id="b0cab-336">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-336">Name</span></span>|<span data-ttu-id="b0cab-337">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-337">Description</span></span>|<span data-ttu-id="b0cab-338">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-338">Required</span></span>|<span data-ttu-id="b0cab-339">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-339">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b0cab-340">šablona</span><span class="sxs-lookup"><span data-stu-id="b0cab-340">template</span></span>|<span data-ttu-id="b0cab-341">Použít toochange hello ukázka režim, který hello nastavit tělo zásady se spustí v.</span><span class="sxs-lookup"><span data-stu-id="b0cab-341">Used toochange hello templating mode that hello set body policy will run in.</span></span> <span data-ttu-id="b0cab-342">Aktuálně hello podporována pouze hodnota je:</span><span class="sxs-lookup"><span data-stu-id="b0cab-342">Currently hello only supported value is:</span></span><br /><br /><span data-ttu-id="b0cab-343">-kapaliny - hello nastavit tělo zásady bude používat modul kapaliny ukázka hello</span><span class="sxs-lookup"><span data-stu-id="b0cab-343">- liquid - hello set body policy will use hello liquid templating engine</span></span> |<span data-ttu-id="b0cab-344">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-344">No</span></span>|<span data-ttu-id="b0cab-345">kapaliny</span><span class="sxs-lookup"><span data-stu-id="b0cab-345">liquid</span></span>|  

<span data-ttu-id="b0cab-346">Pro přístup k informacím o hello žádosti a odpovědi, můžete šablonu kapaliny hello vázat objekt kontextu tooa s hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b0cab-346">For accessing information about hello request and response, hello Liquid template can bind tooa context object with hello following properties:</span></span> <br />
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



### <a name="usage"></a><span data-ttu-id="b0cab-347">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-347">Usage</span></span>  
 <span data-ttu-id="b0cab-348">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-348">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-349">**Části zásady:** vstupní, výstupní a back-end</span><span class="sxs-lookup"><span data-stu-id="b0cab-349">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="b0cab-350">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-350">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-351"><a name="SetHTTPheader"></a>Set – hlavička protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="b0cab-351"><a name="SetHTTPheader"></a> Set HTTP header</span></span>  
 <span data-ttu-id="b0cab-352">Hello `set-header` zásady přiřadí hodnota tooan existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.</span><span class="sxs-lookup"><span data-stu-id="b0cab-352">hello `set-header` policy assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
 <span data-ttu-id="b0cab-353">Seznam hlaviček protokolu HTTP se vloží do zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0cab-353">Inserts a list of HTTP headers into an HTTP message.</span></span> <span data-ttu-id="b0cab-354">Při umístění v příchozí kanálu se tato zásada nastaví hello hlavičky protokolu HTTP pro žádost hello předávány toohello cílovou službu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-354">When placed in an inbound pipeline, this policy sets hello HTTP headers for hello request being passed toohello target service.</span></span> <span data-ttu-id="b0cab-355">Při umístění v odchozí kanálu se tato zásada nastaví hello hlavičky protokolu HTTP pro hello odpověď odesílanou toohello brány klienta.</span><span class="sxs-lookup"><span data-stu-id="b0cab-355">When placed in an outbound pipeline, this policy sets hello HTTP headers for hello response being sent toohello gateway’s client.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-356">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-356">Policy statement</span></span>  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with hello same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a><span data-ttu-id="b0cab-357">Příklady</span><span class="sxs-lookup"><span data-stu-id="b0cab-357">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="b0cab-358">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-358">Example</span></span>  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a><span data-ttu-id="b0cab-359">Předat kontext informace toohello back-end službu</span><span class="sxs-lookup"><span data-stu-id="b0cab-359">Forward context information toohello backend service</span></span>  
 <span data-ttu-id="b0cab-360">Tento příklad ukazuje, jak zásady tooapply v hello rozhraní API na úrovni toosupply kontextové informace toohello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-360">This example shows how tooapply policy at hello API level toosupply context information toohello backend service.</span></span> <span data-ttu-id="b0cab-361">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too10:30.</span><span class="sxs-lookup"><span data-stu-id="b0cab-361">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too10:30.</span></span> <span data-ttu-id="b0cab-362">Na 12:10 je ukázku volání operace v hello portál pro vývojáře, kde se můžete podívat hello zásad v práci.</span><span class="sxs-lookup"><span data-stu-id="b0cab-362">At 12:10 there is a demo of calling an operation in hello developer portal where you can see hello policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward some context information, user id and hello region hello gateway is hosted in, toohello backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 <span data-ttu-id="b0cab-363">Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="b0cab-363">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="b0cab-364">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-364">Elements</span></span>  
  
|<span data-ttu-id="b0cab-365">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-365">Name</span></span>|<span data-ttu-id="b0cab-366">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-366">Description</span></span>|<span data-ttu-id="b0cab-367">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-367">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-368">set – hlavička</span><span class="sxs-lookup"><span data-stu-id="b0cab-368">set-header</span></span>|<span data-ttu-id="b0cab-369">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-369">Root element.</span></span>|<span data-ttu-id="b0cab-370">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-370">Yes</span></span>|  
|<span data-ttu-id="b0cab-371">hodnota</span><span class="sxs-lookup"><span data-stu-id="b0cab-371">value</span></span>|<span data-ttu-id="b0cab-372">Určuje hodnotu hello hello záhlaví toobe sady.</span><span class="sxs-lookup"><span data-stu-id="b0cab-372">Specifies hello value of hello header toobe set.</span></span> <span data-ttu-id="b0cab-373">Pro více záhlaví s hello přidat další stejný název `value` elementy.</span><span class="sxs-lookup"><span data-stu-id="b0cab-373">For multiple headers with hello same name add additional `value` elements.</span></span>|<span data-ttu-id="b0cab-374">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-374">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="b0cab-375">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="b0cab-375">Properties</span></span>  
  
|<span data-ttu-id="b0cab-376">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-376">Name</span></span>|<span data-ttu-id="b0cab-377">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-377">Description</span></span>|<span data-ttu-id="b0cab-378">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-378">Required</span></span>|<span data-ttu-id="b0cab-379">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-379">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b0cab-380">existuje akce</span><span class="sxs-lookup"><span data-stu-id="b0cab-380">exists-action</span></span>|<span data-ttu-id="b0cab-381">Určuje jaké akce tootake při hello záhlaví byl již zadán.</span><span class="sxs-lookup"><span data-stu-id="b0cab-381">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="b0cab-382">Tento atribut musí mít jednu z následujících hodnot hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-382">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="b0cab-383">-Přepište - hodnotu hello nahradí existující záhlaví hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-383">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="b0cab-384">-skip - nenahrazuje hello existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="b0cab-384">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="b0cab-385">-připojit - připojí hello hodnota toohello existující hodnotu hlavičky.</span><span class="sxs-lookup"><span data-stu-id="b0cab-385">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="b0cab-386">-delete - odebere hello hlavičky požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-386">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="b0cab-387">Pokud nastavíte příliš`override` uvedení více položek s hello stejný název výsledky v záhlaví hello se sada podle tooall položky (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-387">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="b0cab-388">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-388">No</span></span>|<span data-ttu-id="b0cab-389">přepsání</span><span class="sxs-lookup"><span data-stu-id="b0cab-389">override</span></span>|  
|<span data-ttu-id="b0cab-390">jméno</span><span class="sxs-lookup"><span data-stu-id="b0cab-390">name</span></span>|<span data-ttu-id="b0cab-391">Určuje název sady toobe záhlaví hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-391">Specifies name of hello header toobe set.</span></span>|<span data-ttu-id="b0cab-392">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-392">Yes</span></span>|<span data-ttu-id="b0cab-393">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-393">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b0cab-394">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-394">Usage</span></span>  
 <span data-ttu-id="b0cab-395">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-395">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-396">**Části zásady:** vstupní, výstupní a back-end, při chybě</span><span class="sxs-lookup"><span data-stu-id="b0cab-396">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="b0cab-397">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-397">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-398"><a name="SetQueryStringParameter"></a>Parametr řetězce dotazu sady</span><span class="sxs-lookup"><span data-stu-id="b0cab-398"><a name="SetQueryStringParameter"></a> Set query string parameter</span></span>  
 <span data-ttu-id="b0cab-399">Hello `set-query-parameter` přidá zásad, nahradí hodnotu, nebo odstranění požadavku parametr řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-399">hello `set-query-parameter` policy adds, replaces value of, or deletes request query string parameter.</span></span> <span data-ttu-id="b0cab-400">Lze použít toopass očekávanou hello back-end službu parametry dotazu, které jsou volitelné nebo nikdy přítomné v žádosti o hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-400">Can be used toopass query parameters expected by hello backend service which are optional or never present in hello request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-401">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-401">Policy statement</span></span>  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with hello same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a><span data-ttu-id="b0cab-402">Příklady</span><span class="sxs-lookup"><span data-stu-id="b0cab-402">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="b0cab-403">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-403">Example</span></span>  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with hello same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a><span data-ttu-id="b0cab-404">Předat kontext informace toohello back-end službu</span><span class="sxs-lookup"><span data-stu-id="b0cab-404">Forward context information toohello backend service</span></span>  
 <span data-ttu-id="b0cab-405">Tento příklad ukazuje, jak zásady tooapply v hello rozhraní API na úrovni toosupply kontextové informace toohello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-405">This example shows how tooapply policy at hello API level toosupply context information toohello backend service.</span></span> <span data-ttu-id="b0cab-406">Ukázka konfiguraci a použití této zásady, najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a převinutí vpřed too10:30.</span><span class="sxs-lookup"><span data-stu-id="b0cab-406">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too10:30.</span></span> <span data-ttu-id="b0cab-407">Na 12:10 je ukázku volání operace v hello portál pro vývojáře, kde se můžete podívat hello zásad v práci.</span><span class="sxs-lookup"><span data-stu-id="b0cab-407">At 12:10 there is a demo of calling an operation in hello developer portal where you can see hello policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward a piece of context, product name in this example, toohello backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 <span data-ttu-id="b0cab-408">Další informace najdete v tématu [výrazy zásad](api-management-policy-expressions.md) a [kontextové proměnné](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="b0cab-408">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="b0cab-409">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-409">Elements</span></span>  
  
|<span data-ttu-id="b0cab-410">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-410">Name</span></span>|<span data-ttu-id="b0cab-411">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-411">Description</span></span>|<span data-ttu-id="b0cab-412">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-412">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-413">parametr dotazu pro sadu</span><span class="sxs-lookup"><span data-stu-id="b0cab-413">set-query-parameter</span></span>|<span data-ttu-id="b0cab-414">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-414">Root element.</span></span>|<span data-ttu-id="b0cab-415">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-415">Yes</span></span>|  
|<span data-ttu-id="b0cab-416">hodnota</span><span class="sxs-lookup"><span data-stu-id="b0cab-416">value</span></span>|<span data-ttu-id="b0cab-417">Určuje hodnotu hello sady toobe parametr dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-417">Specifies hello value of hello query parameter toobe set.</span></span> <span data-ttu-id="b0cab-418">Pro více parametry dotazu s hello přidat další stejný název `value` elementy.</span><span class="sxs-lookup"><span data-stu-id="b0cab-418">For multiple query parameters with hello same name add additional `value` elements.</span></span>|<span data-ttu-id="b0cab-419">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-419">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="b0cab-420">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="b0cab-420">Properties</span></span>  
  
|<span data-ttu-id="b0cab-421">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-421">Name</span></span>|<span data-ttu-id="b0cab-422">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-422">Description</span></span>|<span data-ttu-id="b0cab-423">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-423">Required</span></span>|<span data-ttu-id="b0cab-424">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-424">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b0cab-425">existuje akce</span><span class="sxs-lookup"><span data-stu-id="b0cab-425">exists-action</span></span>|<span data-ttu-id="b0cab-426">Jaké akce tootake Určuje, pokud již zadán parametr dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-426">Specifies what action tootake when hello query parameter is already specified.</span></span> <span data-ttu-id="b0cab-427">Tento atribut musí mít jednu z následujících hodnot hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-427">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="b0cab-428">-Přepište - hodnota hello nahradí existující parametru hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-428">-   override - replaces hello value of hello existing parameter.</span></span><br /><span data-ttu-id="b0cab-429">-skip - nenahrazuje hello existující hodnota parametru dotazu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-429">-   skip - does not replace hello existing query parameter value.</span></span><br /><span data-ttu-id="b0cab-430">-připojit - připojí hello hodnota toohello existující hodnota parametru dotazu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-430">-   append - appends hello value toohello existing query parameter value.</span></span><br /><span data-ttu-id="b0cab-431">-delete - odebere hello parametr dotazu z požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-431">-   delete - removes hello query parameter from hello request.</span></span><br /><br /> <span data-ttu-id="b0cab-432">Pokud nastavíte příliš`override` uvedení více položek s hello stejný název výsledky v parametru dotazu hello se sada podle tooall položky (které budou uvedeny vícekrát); pouze uvedené hodnoty budou nastaveny ve výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-432">When set too`override` enlisting multiple entries with hello same name results in hello query parameter being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="b0cab-433">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-433">No</span></span>|<span data-ttu-id="b0cab-434">přepsání</span><span class="sxs-lookup"><span data-stu-id="b0cab-434">override</span></span>|  
|<span data-ttu-id="b0cab-435">jméno</span><span class="sxs-lookup"><span data-stu-id="b0cab-435">name</span></span>|<span data-ttu-id="b0cab-436">Určuje název sady toobe parametr dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-436">Specifies name of hello query parameter toobe set.</span></span>|<span data-ttu-id="b0cab-437">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-437">Yes</span></span>|<span data-ttu-id="b0cab-438">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-438">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b0cab-439">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-439">Usage</span></span>  
 <span data-ttu-id="b0cab-440">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-440">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-441">**Části zásady:** příchozí back-end</span><span class="sxs-lookup"><span data-stu-id="b0cab-441">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="b0cab-442">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-442">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-443"><a name="RewriteURL"></a>Přepsání adresy URL</span><span class="sxs-lookup"><span data-stu-id="b0cab-443"><a name="RewriteURL"></a> Rewrite URL</span></span>  
 <span data-ttu-id="b0cab-444">Hello `rewrite-uri` zásad převede adresu URL požadavku z formuláře toohello jeho veřejné formuláře očekávanou hello webové služby, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-444">hello `rewrite-uri` policy converts a request URL from its public form toohello form expected by hello web service, as shown in hello following example.</span></span>  
  
-   <span data-ttu-id="b0cab-445">Veřejnou adresu URL-`http://api.example.com/storenumber/ordernumber`</span><span class="sxs-lookup"><span data-stu-id="b0cab-445">Public URL - `http://api.example.com/storenumber/ordernumber`</span></span>  
  
-   <span data-ttu-id="b0cab-446">Adresa URL požadavku –`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span><span class="sxs-lookup"><span data-stu-id="b0cab-446">Request URL - `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span></span>  
  
 <span data-ttu-id="b0cab-447">Tuto zásadu lze použít, když lidského nebo prohlížeče friendly adresa URL by měla transformuje se na formát adresy URL hello očekávanou hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="b0cab-447">This policy can be used when a human and/or browser-friendly URL should be transformed into hello URL format expected by hello web service.</span></span> <span data-ttu-id="b0cab-448">Tato zásada, stačí toobe použít při vystavení alternativní formátu adresy URL, například vyčištění adresy URL, RESTful adresy URL, uživatelsky přívětivý adresy URL SEO přátelské adresy URL, které jsou čistě strukturální adresy URL, které nebude obsahovat řetězec dotazu a místo toho obsahují pouze hello cestu hello prostředek (po hello schématu a autoritě hello).</span><span class="sxs-lookup"><span data-stu-id="b0cab-448">This policy only needs toobe applied when exposing an alternative URL format, such as clean URLs, RESTful URLs, user-friendly URLs or SEO-friendly URLs that are purely structural URLs that do not contain a query string and instead contain only hello path of hello resource (after hello scheme and hello authority).</span></span> <span data-ttu-id="b0cab-449">To se často provádí estetické, použitelnost nebo vyhledávací web pro účely optimalizace (vyhledávací weby SEO).</span><span class="sxs-lookup"><span data-stu-id="b0cab-449">This is often done for aesthetic, usability, or search engine optimization (SEO) purposes.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="b0cab-450">Pouze můžete přidat pomocí zásad hello parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-450">You can only add query string parameters using hello policy.</span></span> <span data-ttu-id="b0cab-451">Nelze přidat další šablony cestou parametry v hello přepisu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="b0cab-451">You cannot add extra template path parameters in hello rewrite URL.</span></span>  

### <a name="policy-statement"></a><span data-ttu-id="b0cab-452">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-452">Policy statement</span></span>  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a><span data-ttu-id="b0cab-453">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-453">Example</span></span>  
  
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
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set too/get?a={b} -->
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
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set too/get?a={b} -->
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

### <a name="elements"></a><span data-ttu-id="b0cab-454">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-454">Elements</span></span>  
  
|<span data-ttu-id="b0cab-455">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-455">Name</span></span>|<span data-ttu-id="b0cab-456">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-456">Description</span></span>|<span data-ttu-id="b0cab-457">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-457">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-458">Přepište uri</span><span class="sxs-lookup"><span data-stu-id="b0cab-458">rewrite-uri</span></span>|<span data-ttu-id="b0cab-459">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-459">Root element.</span></span>|<span data-ttu-id="b0cab-460">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-460">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="b0cab-461">Atributy</span><span class="sxs-lookup"><span data-stu-id="b0cab-461">Attributes</span></span>  
  
|<span data-ttu-id="b0cab-462">Atribut</span><span class="sxs-lookup"><span data-stu-id="b0cab-462">Attribute</span></span>|<span data-ttu-id="b0cab-463">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-463">Description</span></span>|<span data-ttu-id="b0cab-464">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-464">Required</span></span>|<span data-ttu-id="b0cab-465">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-465">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="b0cab-466">šablona</span><span class="sxs-lookup"><span data-stu-id="b0cab-466">template</span></span>|<span data-ttu-id="b0cab-467">Hello adresa URL skutečné webové služby s všech parametrů řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="b0cab-467">hello actual web service URL with any query string parameters.</span></span> <span data-ttu-id="b0cab-468">Pokud používáte výrazy, hello celou hodnota musí být výraz.</span><span class="sxs-lookup"><span data-stu-id="b0cab-468">When using expressions, hello whole value must be an expression.</span></span>|<span data-ttu-id="b0cab-469">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-469">Yes</span></span>|<span data-ttu-id="b0cab-470">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="b0cab-470">N/A</span></span>|  
|<span data-ttu-id="b0cab-471">kopírování neodpovídající parametry</span><span class="sxs-lookup"><span data-stu-id="b0cab-471">copy-unmatched-params</span></span>|<span data-ttu-id="b0cab-472">Určuje, zda jsou parametry dotazu v hello příchozí žádosti nejsou k dispozici v původní šabloně URL hello přidány toohello URL definované hello znovu zapsat šablony</span><span class="sxs-lookup"><span data-stu-id="b0cab-472">Specifies whether query parameters in hello incoming request not present in hello original URL template are added toohello URL defined by hello re-write template</span></span>|<span data-ttu-id="b0cab-473">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-473">No</span></span>|<span data-ttu-id="b0cab-474">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="b0cab-474">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b0cab-475">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-475">Usage</span></span>  
 <span data-ttu-id="b0cab-476">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-476">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-477">**Části zásady:** příchozí</span><span class="sxs-lookup"><span data-stu-id="b0cab-477">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="b0cab-478">**Zásady obory:** produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-478">**Policy scopes:** product, API, operation</span></span>  
  
##  <span data-ttu-id="b0cab-479"><a name="XSLTransform"></a>Transformace XML pomocí transformaci XSLT</span><span class="sxs-lookup"><span data-stu-id="b0cab-479"><a name="XSLTransform"></a> Transform XML using an XSLT</span></span>  
 <span data-ttu-id="b0cab-480">Hello `Transform XML using an XSLT` zásada se vztahuje k transformaci tooXML XSL v textu požadavku nebo odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="b0cab-480">hello `Transform XML using an XSLT` policy applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b0cab-481">Prohlášení o zásadách</span><span class="sxs-lookup"><span data-stu-id="b0cab-481">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="b0cab-482">Příklad</span><span class="sxs-lookup"><span data-stu-id="b0cab-482">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="b0cab-483">Elementy</span><span class="sxs-lookup"><span data-stu-id="b0cab-483">Elements</span></span>  
  
|<span data-ttu-id="b0cab-484">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b0cab-484">Name</span></span>|<span data-ttu-id="b0cab-485">Popis</span><span class="sxs-lookup"><span data-stu-id="b0cab-485">Description</span></span>|<span data-ttu-id="b0cab-486">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b0cab-486">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b0cab-487">transformace XSL</span><span class="sxs-lookup"><span data-stu-id="b0cab-487">xsl-transform</span></span>|<span data-ttu-id="b0cab-488">Kořenový element.</span><span class="sxs-lookup"><span data-stu-id="b0cab-488">Root element.</span></span>|<span data-ttu-id="b0cab-489">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-489">Yes</span></span>|  
|<span data-ttu-id="b0cab-490">Parametr</span><span class="sxs-lookup"><span data-stu-id="b0cab-490">parameter</span></span>|<span data-ttu-id="b0cab-491">Použít toodefine proměnné používané v hello transformace</span><span class="sxs-lookup"><span data-stu-id="b0cab-491">Used toodefine variables used in hello transform</span></span>|<span data-ttu-id="b0cab-492">Ne</span><span class="sxs-lookup"><span data-stu-id="b0cab-492">No</span></span>|  
|<span data-ttu-id="b0cab-493">: stylesheet</span><span class="sxs-lookup"><span data-stu-id="b0cab-493">xsl:stylesheet</span></span>|<span data-ttu-id="b0cab-494">Kořenový element šablony stylů.</span><span class="sxs-lookup"><span data-stu-id="b0cab-494">Root stylesheet element.</span></span> <span data-ttu-id="b0cab-495">Všechny elementy a atributy definované v rámci postupujte podle hello standard [specifikace XSLT](http://www.w3.org/TR/xslt)</span><span class="sxs-lookup"><span data-stu-id="b0cab-495">All elements and attributes defined within follow hello standard [XSLT specification](http://www.w3.org/TR/xslt)</span></span>|<span data-ttu-id="b0cab-496">Ano</span><span class="sxs-lookup"><span data-stu-id="b0cab-496">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b0cab-497">Využití</span><span class="sxs-lookup"><span data-stu-id="b0cab-497">Usage</span></span>  
 <span data-ttu-id="b0cab-498">Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b0cab-498">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b0cab-499">**Části zásady:** vstupní, výstupní</span><span class="sxs-lookup"><span data-stu-id="b0cab-499">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="b0cab-500">**Zásady obory:** globální, produktu, rozhraní API, operace</span><span class="sxs-lookup"><span data-stu-id="b0cab-500">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="b0cab-501">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0cab-501">Next steps</span></span>
<span data-ttu-id="b0cab-502">Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="b0cab-502">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
